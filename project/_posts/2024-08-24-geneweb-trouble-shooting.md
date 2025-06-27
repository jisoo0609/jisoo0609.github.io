---
layout: post
title: 회원 가입 시 권한 설정 
date: 2024-08-24 13:32:20 +0900
description: 회원가입 시 권한 설정 ERROR
img: # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [Time complexity]
categories: [project]
---
## 회원가입 시 권한 설정 ERROR
애플리케이션 실행 시 `admin`계정 create되어야 하는데 `createUser()`에서 오류 발생

![image.png](/assets/img/posts/study/til/240824/img1.png)

![image.png](/assets/img/posts/study/til/240824/img2.png)

```jsx
log.info("authority: {}", accountDetails.getAuthorities());
```

해당 부분에서 권한 받아오지 못함.

`AccountDetails`에서 확인 필요

### `AccountDetails`

- 기존코드

    ```java
    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        if (authority != null) {
            String role = authority.getAuthority();
            return Collections.singleton(new SimpleGrantedAuthority(role));
        } else {
            return Collections.emptySet();
        }
    }
    ```

- `authority`가 권한 제대로 불러오지 않아 `emtySet()` 리턴

  `AccountDetails`내부에

    ```java
    private Authority authority;
    ```

  로 선언되어 있었기 때문에 `getAuthority()`를 이용해 권한을 불러와야 함

- 수정

    ```java
    	@Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        if (authority.getAuthority() != null) {
            String role = authority.getAuthority();
            return Collections.singleton(new SimpleGrantedAuthority(role));
        } else {
            return Collections.emptySet();
        }
    }
    ```

- 제대로 실행되는 것 확인 가능

![image.png](/assets/img/posts/study/til/240824/img3.png)
