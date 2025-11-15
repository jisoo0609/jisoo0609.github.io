---
layout: post
title: "[django] Django Tutorial - TEST CRUD API 3"
date: 2025-11-10 17:32:20 +0900
description: JWT 인증 추가
img: # Add image post (optional)
fig-caption: # Add figcaption (optional)
categories: [project, etc]
---
# JWT 인증 추가
## JWT


## `djangorestframework-simplejwt` 라이브러리를 사용하여 JWT 인증 추가하기
### 1. 설치
```bash
pip install djangorestframework-simplejwt
```
### 2. setting.py 설정
```bash
INSTALLED_APPS = [
    # ...
    'rest_framework',
    'rest_framework_simplejwt',
    # ...
]

REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': (
        'rest_framework_simplejwt.authentication.JWTAuthentication',
    ),
    'DEFAULT_PERMISSION_CLASSES': (
        'rest_framework.permissions.IsAuthenticatedOrReadOnly',
    ),
}

from datetime import timedelta

SIMPLE_JWT = {
    'ACCESS_TOKEN_LIFETIME': timedelta(minutes=60),
    'REFRESH_TOKEN_LIFETIME': timedelta(days=1),
    'ROTATE_REFRESH_TOKENS': False,
    'BLACKLIST_AFTER_ROTATION': True,
    'ALGORITHM': 'HS256',
    'SIGNING_KEY': SECRET_KEY,
    'AUTH_HEADER_TYPES': ('Bearer',),
}
```
### 3. urls.py 설정
기존 구조를 유지하면서 JWT 엔드포인트만 추가하면 된다.
```bash
from django.urls import path, include
from rest_framework.routers import DefaultRouter
from rest_framework_simplejwt.views import (
    TokenObtainPairView,
    TokenRefreshView,
)
from .views import PostViewSet

router = DefaultRouter()
router.register('board', PostViewSet, basename='post')

urlpatterns = [
    path('v1/', include(router.urls)),
    path('v1/token/', TokenObtainPairView.as_view(), name='token_obtain_pair'),
    path('v1/token/refresh/', TokenRefreshView.as_view(), name='token_refresh'),
]
```
- 게시글 API: `/v1/board/`
- 토큰 발급 `/v1/token`
- 토큰 갱신 `/v1/token/refresh/`


## TEST
### 1. 회원가입
- POST `/api/v1/register/`
```json
{
"username": "john",
"password": "password123",
"email": "john@example.com"
}
```
![img.png](/assets/img/posts/project/etc/django/3/img8.png)
### 2. 토큰 발급
- POST `/api/v1/token/`
```bash
{
    "username": "john",
    "password": "password123"
}
```
![img.png](/assets/img/posts/project/etc/django/3/img.png)
### 3. 인증이 필요한 작업
- POST `/api/v1/board/`
```bash
Headers: Authorization: Bearer <access_token>
{
"title": "제목",
"content": "내용",
"author": "john"
}
```
