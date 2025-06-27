---
layout: post
title: Data Visualization
date: 2025-06-23 18:00:00 +0900
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [공공데이터, 데이터시각화]
categories: [data-analysis]
---
# Data Visualization

## 요약

여러가지 데이터를 시각화 한 결과 중, 몇 가지를 선정하여 정리하였다.

분석을 위해 사용한 언어는 R이다.

여러 공공데이터를 수집하고, 이를 R로 분석한 후 시각화하여 나타낸 결과물이다.

---
<details>
<summary> 목차</summary>
- 거주지역의 교통사고에 대한 시각화<br>
- 거주지역의 CCTV 설치 현황 시각화<br>
- 거주지역의 관심 상점 지도에 시각화<br>
- 관심 있는 주식의 추세를 파악하기 위한 시각화<br>
- 한국인의 삶을 파악하라
</details>


---

## 거주지역의 교통사고에 대한 시각화

데이터를 확인하기 유리한 지역의 데이터를 선정하여 시각화를 진행하였다.

### **경상남도 창원시 월별 교통사고 통계(2018)**

```r
par(bg="lightcyan")
plot(d1, main="경남 창원시 월별 교통사고 통계(2018)", xlab="월", ylab="건수", type='o', col=1, ylim = c(0,400), pch=1, lwd=2)
lines(d2, type='o', col=2, pch=2)
lines(d3, type='o', col=3, pch=3, lwd=2)
lines(d4, type='o', col=4, pch=4, lwd=2)
lines(d5, type='o', col=5, pch=5, lwd=2)
legend(10,220, name_lab5, cex=0.55, col=c(1,2,3,4,5), lty=1, pch=1:5, bg="yellow")

abline(v=seq(10,12,5), lty=3,lwd=2, col="red")
text(10, 400, "사망자수 최대", cex=0.8, col="red")
text(10, 250, "발생건수, 경상 수 최대", cex=0.8, col="red")

arrows(11,20, 11,120)
text(11,11, "중상 수 최대", cex=0.8, col="blue")
```

- 실행 결과

![Untitled](/assets/img/posts/data-visualization/Untitled.png)

### **경상남도 시군구별 교통사고**

```r
par(bg="lightblue")
bpx1 <- barplot(v3, main="경남 시군구별 교통사고", col="green", cex.names=1.0, las=2, ylim=c(0,4200), xlab="시군구", ylab="건수")
par(new=T)
bpx2 <- barplot(v1, col="red", cex.names = 1.0, las=2, ylim=c(0,4200))
par(new=T)
bpx3 <- barplot(v2, col= "blue", cex.names = 1.0, las=2, ylim=c(0,4200))
par(new=T)

plot(v4, type="o", col="#7bcdbf", lwd=2, lty=2, pch=19, axes=F, ann=F, ylim = c(0,4200))
par(new=T)
lines(v5, type="o", col="pink", lwd=2, lty=2, pch=19)

axis(1, at=1:18, lab=name1)

abline(h=seq(0,4200,1000), col="white", lty=2)
abline(v=seq(0, 18, 1), col="white",lty=2)

legend(1, 4200, c("발생건수", "사망자수", "부상자수"), fill=c("red", "blue", "green"), cex=0.6, bg="white")
legend(5, 4200, c("중상", "경상"), col=c("#7bcdbf", "pink"), cex=0.6, bg="white", lty=2)

x3 <- x + 13
y3 <- y*800 + 3500
polygon(x3,y3, border="blue")
arrows(10, 3500, 13, 4000)
text(10,3500, "교통사고 발생 최대 지역: 창원시", cex=0.8 , col="red")
```

- 실행 결과

![Untitled](/assets/img/posts/data-visualization/Untitled%201.png)

## 거주지역의 교통사고에 대한 시각화

데이터 확인을 위해 서울특별시의 데이터를 사용해 분석을 진행하였다.

### **서울특별시 교통사고 현황**

```r
bpx <- barplot(m3, col= col2, beside = T, main = "서울특별시 교통사고 현황", cex.names=1.0, las=2, ylim=c(0, 360))

abline(h=seq(0,500,100), lty=2, col="gray")
abline(v=seq(0,100, 26), lty=3,lwd=2, col="gray")

text(x = bpx, y = m3*1.05, labels = paste(pct2,"%"), col = "black", cex = 0.7)

legend(1, 350, c(">=300", ">=150", ">=50", ">=20", "<20"), col=c("red","green", "blue", "purple", "pink"), cex=0.6, lty=1, lwd=3)
```

- 실행 결과

![Untitled](/assets/img/posts/data-visualization/Untitled%202.png)

### **서울특별시 종로구 월별 교통사고 평균**

```r
label3 <- paste(jongnogu$월, "\n", jongnogu$발생건수,"건")
data_tm04 <- c jongnogu$발생건수

data_tm05 <- data.frame(data_tm04, lab=label3)
treemap(data_tm05, vSize="data_tm04", index=c("lab"))
```

- 실행 결과

![Untitled](/assets/img/posts/data-visualization/Untitled%203.png)

---

## 거주지역의 CCTV 설치 현황을 시각화

CCTV 설치 현황을 확인하기 위해, CCTV가 다수 설치 되어있는 도심의 데이터를 선택하여 분석하였다.

구글에서 구글 지도 API를 가져와 지도 위에 CCTV의 위치가 나타나도록 했다.

### 서울특별시 마포구 CCTV 설치 현황

```r
mapo <- subset(data1, 관리기관명=="서울특별시 마포구청")

g_m <- get_map("서울특별시 마포구", zoom = 13, maptype = "roadmap")

mapo.map <- ggmap(g_m) + geom_point(data= mapo, aes(x=WGS84경도, y=WGS84위도), size=2, alpha=0.7, color="#980000")
```

- 실행 결과

![Untitled](/assets/img/posts/data-visualization/Untitled%204.png)

### **서울특별시 종로구 CCTV 설치 현황**

```r
jongnogu <- subset(data1, 관리기관명=="서울특별시 종로구청")

g_m <- get_map("서울특별시 종로구", zoom = 13, maptype = "roadmap")

jongno.map <- ggmap(g_m) + geom_point(data=jongnogu, aes(x=WGS84경도, y=WGS84위도), size=2, alpha=0.7, color="#980000")
```

- 실행 결과

![Untitled](/assets/img/posts/data-visualization/Untitled%205.png)

---

## 거주 지역에 관심 있는 상점들을 지도에 시각화

데이터 확인을 위해 상점이 많이 분포되어있는 지역의 데이터를 선택해 시각화했다.

구글에서 구글 지도 API를 가져와 지도 위에 CCTV의 위치가 나타나도록 했다.

### **서울특별시 마포구 합정동**

```r
map <- get_googlemap(center=cen, zoom=15, size=c(640,640), maptype="roadmap")
gmap <- ggmap(map)

gmap + geom_point(data = hapjeong_df, 
                  aes(x=경도, y=위도, color=상권업종대분류명), size=2, alpha=0.7) +
  labs(x = "Longitude", y = "Latitude",
       title="합정동 업종별 점포",  color = "업종")
```

- 실행 결과

![Untitled](/assets/img/posts/data-visualization/Untitled%206.png)

### **서울특별시 종로구 이화동**

```r
map <- get_googlemap(center=cen, zoom=15, size=c(640,640), maptype="roadmap")
gmap <- ggmap(map)

gmap + geom_point(data = ihwa_df, 
                  aes(x=경도, y=위도, color=상권업종대분류명), size=2, alpha=0.7) +
  labs(x = "Longitude", y = "Latitude",
       title="이화동 업종별 점포",  color = "업종")
```

- 실행 결과

![Untitled](/assets/img/posts/data-visualization/Untitled%207.png)

## 관심 있는 주식의 추세를 파악하기 위한 시각화

### **디즈니의 2022년 04월 08일부터 2022년 05년 25일까지의 주가**

```r
getSymbols("DIS", from=as.Date("2022-04-08"),to=as.Date("2022-05-25")) 
p <- ggplot(DIS, aes(x=index(DIS), y=DIS.Close))
p <- p + geom_ribbon(aes(min=DIS.Low, max=DIS.High), fill="lightblue", colour="black")
p <- p + geom_point(aes(y=DIS.Close), colour="black", size=5, alpha=0.6)
p <- p + geom_line(aes(y=DIS.Close), colour="blue")
p <- p + stat_smooth(method="loess", se=FALSE, colour="red", lwd=1.2)
p <- p + geom_step(aes(y=DIS.Close), color = "blue", lwd=1)
p
```

- 실행 결과

![Untitled](/assets/img/posts/data-visualization/Untitled%208.png)

---

## 한국인의 삶을 파악하라

UN에서 발표한 연령대를 기준으로 분석했다.

### 연령별 평균 월급을 분석하여 시각화

```r
ggplot(data = ageg_income, aes(x = ageg, y = mean_income)) +
  geom_col() +
  scale_x_discrete(limits = c("underage","youth", "middle", "old"))
```

- 실행 결과

![Untitled](/assets/img/posts/data-visualization/Untitled%209.png)

### **연령대 및 성별 월급 차이**

```r
ggplot(data = sex_income, aes(x = ageg, y = mean_income, fill = sex)) +
  geom_col(position = "dodge") +
scale_x_discrete(limits = c("underage", "youth", "middle", "old"))
```

- 실행 결과

![Untitled](/assets/img/posts/data-visualization/Untitled%2010.png)

### 연령대 및 성별에 따른 가구원 수 분석

0~17세는 평균 4명의 가구원 수를 가지지만, 연령대가 줄어들수록 가구원 수는 대체로 감소한다는 것을 확인할 수 있다.

`ggplot()`을 사용해 분석을 진행했다.

![Untitled](/assets/img/posts/data-visualization/Untitled%2011.png)

같은 결과를 `ggplot2()` 을 사용하여 분석하였다.

![Untitled](/assets/img/posts/data-visualization/Untitled%2012.png)
