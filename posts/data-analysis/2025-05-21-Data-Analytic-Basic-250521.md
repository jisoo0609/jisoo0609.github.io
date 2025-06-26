---
layout: post
title: Data Analytic - Basic
date: 2025-06-23 14:00:00 +0900
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [데이터분석]
categories: [posts, data-analysis]
---
# Data Analytic - Basic

<details>
<summary> 목차</summary>
- box plot<br>
- bar plot<br>
- pie plot<br>
- strip chart
</details>
---

## Box plot

data’ 파일에서 남자와 여자의 나이를 box plot으로 만들어 비교한다.

```r
> boxplot(subset(data, Gender == "M")$Age, subset(data, Gender == "F")$Age, col = c("blue", "red"), names=c("male", "female"), main = "Age between male and female", ylab="Age")
```

![Untitled](/assets/img/posts/bio-informatics/data-analytic-basic/Untitled.png)

---

## bar plot

‘data’ 파일에서 제2형 당뇨병(T2D)에 흡연 여부의 영향을 판단해 볼 수 있는 bar plot를 만든다.

```r
> t2dTable <- table(data$T2D)
> smoker <- table(data$Smoker)
> t2dSmo <- table(data$T2D, data$Smoker)
> barplot(t2dSmo, col = c("red", "green"), names = c("Nonsmoker", "Smoker"), main ="T2D vs Smoking", ylab = "Number")
```

![Untitled](/assets/img/posts/bio-informatics/data-analytic-basic/Untitled%201.png)

---

## pie plot

data’ 파일에서 제2형 당뇨병 환자의 남녀 비율을 pie plot으로 표현한다.

```r
> m_num <- nrow(subset(data, Gender == "M"))
> f_num <- nrow(subset(data, Gender == "F"))
> tot<- c(m_num, f_num)
> names(tot) <- c("M", "F")
```

- 남녀의 숫자를 pie plot에 labelling 하는 경우

```r
> lab1 <-paste(names(tot), "\n", tot, "명")
> pie(tot, labels = lab1)
```

![Untitled](/assets/img/posts/bio-informatics/data-analytic-basic/Untitled%202.png)

- 남녀 숫자의 %값을 소수점 2에서 반올림하여 pie plot에 labling하는 경우

```r
> pct <- round(tot/sum(tot)*100, 2)
> lab2 <- paste(names(tot),"\n", pct, "%")
> pie(tot, labels = lab2)
```

![Untitled](/assets/img/posts/bio-informatics/data-analytic-basic/Untitled%203.png)

---

## strip chart

‘data’ 파일에서 제2형 당뇨병 환자와 정상인의 나이 분포를 비교하여 확인할 수 있는
strip chart를 만든다.

```r
> dib <- subset(data, T2D == "2")
> normal <- subset(T2D == "1")
> wtcom<-list("dib" = dib$Age, "normal"=normal$Age)
> stripchart(wtcom, vertical = T)
> stripchart(wtcom, vertical = T, main = "T2D age",method = "jitter", col = c("green", "red"), xlab ="T2D", ylab = "Number", pch = 5)
```

![Untitled](/assets/img/posts/bio-informatics/data-analytic-basic/Untitled%204.png)