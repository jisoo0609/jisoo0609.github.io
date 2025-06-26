---
layout: post
title: "[Project] WorkMate-240322"
date: 2024-03-22 13:32:20 +0900
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: 
---
## 3월 22일 목요일
### To Do List
[ ] 로그인 페이지 구현 → static image 추가
- `/static` 패키지가 없어 정적 파일이 localhost:8080/환경에서 나타나지 않음
- 파일 경로 설정 확인 필요하다.

[x] 로그인 후 등장하는 프로필 페이지에서 아르바이트 요청 가능하게 함
- 아르바이트를 요청하면 해당 아르바이트를 요청한 사람의 정보가 `AccountShop`에 저장됨
    - `/profile/{accountId}/submit?name=` 에서 요청이 가능하다.
    - `name` 은 요청을 전달할 `shop`의 이름이 된다.
- `Shop`의 매니저가 승낙하면, 요청이 승낙된다. 거절한 경우 요청이 삭제된다.
    - `/profile/{accountId}/accpet/{AccountShopId}?flag=` 에서 요청 승낙 / 거절 테스트 가능하다.
    - `LocalDateTime`을 이용해 아르바이트 요청이 승낙됨과 동시에, 해당 날짜를 일을 시작한 날짜로 등록하는 방법도 고려해 볼 필요가 있을 것 같다
- `/static` 패키지가 없어 정적 파일이 `localhost:8080/`환경에서 나타나지 않음
- 파일 경로 설정 확인 필요하다.

----
## 프로젝트 1차 코칭
### 로그인 시 토큰 전달 문제

백엔드에서 로그인 후 인증이 필요한 페이지로 넘어갈 때 토큰을 전달하는 방법에 대해 질문했다.

1. **Form Login을 사용하는 방법을 고려할 수 있다.**
2. **JWT 토큰을 Cookie를 이용해 client에 전달하는 방법을 고려할 수 있다.**
    
    Jwt 토큰에서 필요한 정보를 Response Body가 아닌 Cookie에 저장해서 전달하는 방식이다.하지만, 이 경우 Cookie에 Jwt 정보를 전달해 저장하면 csrf 보안에 취약해진다는 단점이 있다. Jwt 토큰을 사용하는 의미가 없어질 수 있다.
    
3. **AJAX를 이용해 토큰을 전달할 수 있다.**
가장 일반적으로 Jwt토큰을 전달하는 방법이다.
하지만, 이 경우에는 JavaScript를 사용하는 백엔드를 벗어난 프론트엔드와 가까운 영역이다. 따라서, JavaScript를 구현하기 위해 공부하는 시간이 조금 필요할 수 있다.
가장 구현 가능한, 적합한 기술을 찾아 적용해야 할 것 같다.
### 정적 파일 관리 문제

인텔리제이에서는 이미지 파일이 미리보기가 되는데, `localhost:/`에서 실행하게 되면 이미지 파일이 손상되어 보이지 않는 오류가 있었다.

`/static` 폴더가 누락되어 정적 파일의 경로가 `/static`을 포함하고 있지 않았기 때문에 오류가 발생할 수 있다.

또한, WebSecurity가 켜져 있기 때문에, 보안이 필요한 페이지에서 정적 파일을 로드하지 못하고 있을 수 있다. 정적 파일은 WebSecurity와 관계 없이 로드 할 수 있도록 설정해야 한다.

교안의 정적 파일 관리법을 참고하여 해결을 시도해 봐야 할 것 같다.

### NCP API 위치 관리 문제
### JQuery 조회 속도 문제
두 기간을 검색해서 사이의 데이터들을 불러오기
시작시간과 끝시간이 있는데 시작시간을 기준으로 검색하는 경우
- `service`
    ```java List<WorkTime> workTimes = workTimeRepo
    .findAllByShop_IdAndWorkStartTimeBetween(
    shopId, startDay, endDay
    );
    ```
- `repository`
    ```java List<WorkTime> findAllByShop_IdAndWorkStartTimeBetween(
        Long ShopId, LocalDateTime start, LocalDateTime end);
    ```
- console
    ```
    Hibernate: select wt1_0.id,wt1_0.account_id,wt1_0.shop_id,wt1_0.work_end_time,wt1_0.work_role,wt1_0.work_star
    ```
쿼리를 돌렸을 때, 단순히 속도가 느려지는 부분은 없다. 다른 부분의 확인이 필요하다.

---
## Error
`merge`과정에서 변경된 파일로 인해 애플리케이션이 정상적으로 실행되지 않는 오류가 있었다.

애플리케이션 실행 후 오류 로그를 확인했을 때,
![error](/assets/img/posts/240322/스크린샷%202024-03-22%20182207.png)
해당 오류가 나는 것을 확인할 수 있었다.

오류는 `OAuth2SuccessHandler`에서 `jwt.secret`주입 과정에 문제가 있다고 명시되어 있었지만, 해당 파일은 오늘 작업 중에 수정한 적이 없었다. 오류가 해결되지 않아 오늘의 `pull Request` 기록을 확인했다.

`application.yaml`이 수정된 기록을 확인 할 수 있었다.
원래 `application.yaml`은 이렇게 설정되어 있었다.
```yaml 
profiles:
  active: git
```
오늘 커밋 과정에서 수정된 yaml이 반영되었고, 이 과정에서 
```yaml 
profiles:
  active: home
```
로 설정이 변경되면서 오류가 발생한 것 같았다.

일단, 현재 `git`에 올라가있는 설정은 `application-git.yaml`이니 `home`을 `git`으로 수정하는 것으로 오류를 해결했다.

