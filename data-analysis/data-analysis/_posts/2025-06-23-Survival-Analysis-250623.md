---
layout: post
title: Survival Analysis
date: 2025-06-23 17:00:00 +0900
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [Survival Analysis]
categories: [data-analysis]
---
# Survival Analysis
선택한 암종(cancer type)에 대하여 UCSC Xena ([https://xena.ucsc.edu/)](https://xena.ucsc.edu/)) 사이트에서 survival data와 phenotype data를 다운받아 다음의 survival analysis를 실행한다.

---
## Summary
<details>
<summary> 목차</summary>
- 파일 불러오기<br>
- 개인의 흡연습관에 유의미하게 영향을 받는 질환<br>
- 공분산 (covariance) 및 상관계수(correlation coefficient)
</details>

---

## 파일 불러오기

```r
> survGene <- read.table("C:/Users/user/Desktop/TCGA-UCEC.survival.tsv", header = T, sep = "\t")
> survival <- subset(survGene, select=c(id,Status,Time))

> pheno <- read.table("C:/Users/user/Desktop/pheno.txt", header = T, sep = "\t")

> top_DEG <- read.table("C:/Users/user/Desktop/top_DEG.txt", header=T, sep = "\t")

> phenotype <- merge(pheno, top_DEG, by="id")

> install.packages("survival")
> library(survival)

> survPheno <- merge(survival, phenotype, by = "id")

> attach(survPheno)
```

---

## 생존분석(LogRank test)

해당 암 환자들의 생존 기간에 영향을 미치는 요인들에 대하여 생존분석(LogRank test)을 시도하고 각 요인의 그룹 간에 유의한 생존 차이를 보이는 요인이 어떤 것인지 확인한다.

확인한 요인에 대한 생존 그래프를 생성한다.

<aside>
✅ 분석에 사용할 요인들의 list

- age_at_index.demographic
- gender.demographic
- race.demographic
- tumor_stage.diagnoses
- disease_type
- bmi.exposures
- top_DEG

</aside>

---

### age_at_index.demographic

```r
> survdiff(formula = Surv(Time, Status == 1)~age>median(age, na.rm=T))
Call:
survdiff(formula = Surv(Time, Status == 1) ~ age > median(age, 
    na.rm = T))

n=565, 결측으로 인하여 2개의 관측치가 삭제되었습니다..

                                     N Observed Expected (O-E)^2/E (O-E)^2/V
age > median(age, na.rm = T)=FALSE 306       39     53.6      3.97      9.19
age > median(age, na.rm = T)=TRUE  259       56     41.4      5.14      9.19

 Chisq= 9.2  on 1 degrees of freedom, p= 0.002
```

```r
> fitA <- survfit(Surv(Time, Status == 1)~age>median(age, na.rm=T))
> plot(fitA, col = 1:4)
```

![Untitled](/assets/img/posts/data-analysis/bio-informatics/survival-analysis/Untitled.png)

age에 따른 생존률의 차이가 존재하는 것을 확인할 수 있다.

### gender.demographic

```r
> survdiff(formula = Surv(Time, Status == 1)~gender.demographic)
Error in survdiff.fit(y, groups, strata.keep, rho) : 
  There is only 1 group
```

해당 암종은 Uterine corpus endometrial carcinoma (UCEC)로 자궁 내막 암이기 때문에 남성에게는 발병하지 않고 여성에게만 발병한다.

### race.demographic

```r
> survdiff(formula = Surv(Time, Status == 1)~race.demographic)
Call:
survdiff(formula = Surv(Time, Status == 1) ~ race.demographic)

                                                             N Observed
race.demographic=american indian or alaska native            4        1
race.demographic=asian                                      20        2
race.demographic=black or african american                 110       19
race.demographic=native hawaiian or other pacific islander   9        2
race.demographic=not reported                               32        2
race.demographic=white                                     392       69
                                                           Expected (O-E)^2/E
race.demographic=american indian or alaska native             0.503  0.490368
race.demographic=asian                                        5.150  1.926475
race.demographic=black or african american                   16.350  0.429389
race.demographic=native hawaiian or other pacific islander    1.958  0.000885
race.demographic=not reported                                 5.551  2.271260
race.demographic=white                                       65.488  0.188377
                                                           (O-E)^2/V
race.demographic=american indian or alaska native           0.494153
race.demographic=asian                                      2.053215
race.demographic=black or african american                  0.521357
race.demographic=native hawaiian or other pacific islander  0.000907
race.demographic=not reported                               2.418920
race.demographic=white                                      0.607848

 Chisq= 5.3  on 5 degrees of freedom, p= 0.4 
```

```r
> fitR <- survfit(Surv(Time, Status == 1)~race.demographic)
> plot(fitR)
```

![Untitled](/assets/img/posts/data-analysis/bio-informatics/survival-analysis/Untitled%201.png)

인종에 따른 생존률의 차이가 존재한다.

### tumor_stage.diagnoses

```r
> survdiff(formula = Surv(Time, Status == 1)~tumor_stage.diagnoses)
Error in survdiff.fit(y, groups, strata.keep, rho) : 
  There is only 1 group
```

해당 암종의 tumor_stage.diagnoses는 not reported 되어있어, 확인할 수 없다.

### disease_type

```r
> survdiff(formula = Surv(Time, Status == 1)~disease_type)
Call:
survdiff(formula = Surv(Time, Status == 1) ~ disease_type)

                                                     N Observed Expected
disease_type=Adenomas and Adenocarcinomas          420       49   72.203
disease_type=Cystic, Mucinous and Serous Neoplasms 145       46   22.433
disease_type=Epithelial Neoplasms, NOS               2        0    0.365
                                                   (O-E)^2/E (O-E)^2/V
disease_type=Adenomas and Adenocarcinomas              7.456    31.423
disease_type=Cystic, Mucinous and Serous Neoplasms    24.760    32.798
disease_type=Epithelial Neoplasms, NOS                 0.365     0.366

 Chisq= 33  on 2 degrees of freedom, p= 7e-08 
```

```r
> fitDis <- survfit(Surv(Time, Status == 1)~disease_type)
> plot(fitDis, col=1:4)
```

![Untitled](/assets/img/posts/data-analysis/bio-informatics/survival-analysis/Untitled%202.png)

disease_type에 따른 생존률의 차이가 존재한다.

### bmi.exposures

```r
> survdiff(formula = Surv(Time, Status == 1)~(bmi.exposures>median(bmi.exposures, na.rm =T)))
Call:
survdiff(formula = Surv(Time, Status == 1) ~ (bmi.exposures > 
    median(bmi.exposures, na.rm = T)))

n=534, 결측으로 인하여 33개의 관측치가 삭제되었습니다..

                                                         N Observed Expected
bmi.exposures > median(bmi.exposures, na.rm = T)=FALSE 267       50     46.7
bmi.exposures > median(bmi.exposures, na.rm = T)=TRUE  267       41     44.3
                                                       (O-E)^2/E (O-E)^2/V
bmi.exposures > median(bmi.exposures, na.rm = T)=FALSE     0.232     0.484
bmi.exposures > median(bmi.exposures, na.rm = T)=TRUE      0.245     0.484

 Chisq= 0.5  on 1 degrees of freedom, p= 0.5 
```

```r
> fitB <- survfit(Surv(Time, Status == 1)~(bmi.exposures>median(bmi.exposures, na.rm =T))) 
> plot(fitB, col=1:4)
```

![Untitled](/assets/img/posts/data-analysis/bio-informatics/survival-analysis/Untitled%203.png)

bmi.exposures에 따른 생존률의 차이가 존재한다.

### top_DEG

```r
> survdiff(formula = Surv(Time, Status == 1)~(deg>median(deg, na.rm=T)))
Call:
survdiff(formula = Surv(Time, Status == 1) ~ (deg > median(deg, 
    na.rm = T)))

                                     N Observed Expected (O-E)^2/E (O-E)^2/V
deg > median(deg, na.rm = T)=FALSE 284       49     49.3   0.00141   0.00293
deg > median(deg, na.rm = T)=TRUE  283       46     45.7   0.00152   0.00293

 Chisq= 0  on 1 degrees of freedom, p= 1 
```

```r
> fitD <- survfit(Surv(Time, Status == 1)~(deg>median(deg, na.rm=T)))
> plot(fitD, col = 1:4)
```

![Untitled](/assets/img/posts/data-analysis/bio-informatics/survival-analysis/Untitled%204.png)

deg에 따른 생존률의 차이가 존재한다.

### result

해당 암종은 Uterine corpus endometrial carcinoma (UCEC)로 자궁 내막 암이기 때문에 남성에게는 발병하지 않고 여성에게만 발병한다.

나이, 인종, 질병 타입, BMI, DEG에 따른 생존률의 차이가 존재한다.

하지만, tumor_stage.diagnoses는 not reported 되어있어, 확인할 수 없다.

---

## Hazard Ratio - Coxph

Log-rank test에서 유의하게 나온 요인에 대하여 coxph함수를 이용한 modeling을 시도하고 reference에 대한 각 factor의 hazard ratio를 확인한다.

### exception

UCEC는 여성에게만 발병하는 질병에 해당하기 때문에, gender에 따른 생존률의 고려가 필요 하지 않을 것으로 판단되어 제외했다.

### age

```r
> coxph(formula = Surv(Time, Status == 1)~(age>median(age, na.rm=T)))
Call:
coxph(formula = Surv(Time, Status == 1) ~ (age > median(age, 
    na.rm = T)))

                                   coef exp(coef) se(coef)     z       p
age > median(age, na.rm = T)TRUE 0.6225    1.8635   0.2092 2.975 0.00293

Likelihood ratio test=9.04  on 1 df, p=0.002642
n= 565, number of events= 95 
   (결측으로 인하여 2개의 관측치가 삭제되었습니다.)
```

exp(coef)값이 1.8635로 exp(coef) > 1이다. 따라서 reference보다 예후가 더 좋지 않음을 의미한다.

### race.demographic

```r
> coxph(formula = Surv(Time, Status == 1)~race.demographic)
Call:
coxph(formula = Surv(Time, Status == 1) ~ race.demographic)

                                                             coef exp(coef)
race.demographicasian                                     -1.6466    0.1927
race.demographicblack or african american                 -0.5435    0.5807
race.demographicnative hawaiian or other pacific islander -0.6799    0.5067
race.demographicnot reported                              -1.7173    0.1796
race.demographicwhite                                     -0.6419    0.5263
                                                          se(coef)      z
race.demographicasian                                       1.2284 -1.340
race.demographicblack or african american                   1.0277 -0.529
race.demographicnative hawaiian or other pacific islander   1.2282 -0.554
race.demographicnot reported                                1.2265 -1.400
race.demographicwhite                                       1.0093 -0.636
                                                              p
race.demographicasian                                     0.180
race.demographicblack or african american                 0.597
race.demographicnative hawaiian or other pacific islander 0.580
race.demographicnot reported                              0.161
race.demographicwhite                                     0.525

Likelihood ratio test=6.54  on 5 df, p=0.2568
n= 567, number of events= 95 
```

exp(coef) < 1이므로 reference보다 예후가 더 좋은 prognostic factor로써 작용함을 의미한다.

### disease_type

```r
> coxph(formula = Surv(Time, Status == 1)~disease_type)
Call:
coxph(formula = Surv(Time, Status == 1) ~ disease_type)

                                                        coef  exp(coef)
disease_typeCystic, Mucinous and Serous Neoplasms  1.122e+00  3.070e+00
disease_typeEpithelial Neoplasms, NOS             -1.383e+01  9.813e-07
                                                    se(coef)      z        p
disease_typeCystic, Mucinous and Serous Neoplasms  2.067e-01  5.428 5.71e-08
disease_typeEpithelial Neoplasms, NOS              2.019e+03 -0.007    0.995

Likelihood ratio test=28.52  on 2 df, p=6.412e-07
n= 567, number of events= 95 
```

exp(coef)의 값이 1보다 크므로 reference보다 예후가 더 좋지 않음을 의미한다.

### bmi.exposures

```r
> coxph(formula = Surv(Time, Status == 1)~(bmi.exposures>median(bmi.exposures, na.rm =T)))
Call:
coxph(formula = Surv(Time, Status == 1) ~ (bmi.exposures > median(bmi.exposures, 
    na.rm = T)))

                                                        coef exp(coef)
bmi.exposures > median(bmi.exposures, na.rm = T)TRUE -0.1449    0.8651
                                                     se(coef)      z     p
bmi.exposures > median(bmi.exposures, na.rm = T)TRUE   0.2120 -0.684 0.494

Likelihood ratio test=0.47  on 1 df, p=0.4935
n= 534, number of events= 91 
   (결측으로 인하여 33개의 관측치가 삭제되었습니다.)
```

exp(coef)값이 0.8651로 exp(coef) < 1이다. 따라서, reference보다 예후가 더 좋은 prognostic factor로써 작용한다고 해석할 수 있다.

### deg

```r
> coxph(formula = Surv(Time, Status == 1)~(deg>median(deg, na.rm=T)))
Call:
coxph(formula = Surv(Time, Status == 1) ~ (deg > median(deg, 
    na.rm = T)))

                                    coef exp(coef) se(coef)     z     p
deg > median(deg, na.rm = T)TRUE 0.01072   1.01078  0.20541 0.052 0.958

Likelihood ratio test=0  on 1 df, p=0.9584
n= 567, number of events= 95 
```

exp(coef)값이 1.01078로 exp>1이다. 따라서, reference보다 예후가 더 좋지 않음을 의미한다.

---

## Hazard Ratio Visualization

상기 내용을 망라할 수 있는 (covariate들을 포함하는) multi-model을 생성하고 이로부터 각 독립변수들의 hazard ratio를 시각화하여 확인한다.

```r
multiModel <- coxph(formula = Surv(Time, Status == 1)~(age>median(age, na.rm=T))+race.demographic+disease_type+(deg>median(deg, na.rm=T))+(bmi.exposures>median(bmi.exposures, na.rm =T)))

> install.packages("moonBook")
> library(moonBook)
> HRplot(multiModel, type=2, show.CI = T, main = "Harzard ratio")
```

![Untitled](/assets/img/posts/data-analysis/bio-informatics/survival-analysis/Untitled%205.png)
