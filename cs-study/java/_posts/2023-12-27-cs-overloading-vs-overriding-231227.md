---
layout: post
title: "Overloading VS Overriding"
date: 2023-12-27 14:27:20 +0900
description: 'CS: 오버로딩과 오버라이딩'
img: # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [Overloading, Overriding]
categories: [cs-study, java]
---
## 오버로딩 (Overloading)

- 매개변수의 유형과 개수를 다르게 하여 같은 이름의 메서드를 여러 개 가지는 기법
- 동일한 메서드(또는 함수)의 이름을 사용하면서 매개변수의 타입이나 개수를 다르게 하는 것.
- 같은 이름의 메서드를 여러 형태로 사용하여 다양한 매개변수에 대응할 수 있게 함.
- 예시)

```java
public class Calculator {
    public int add(int a, int b) {
        return a + b;
    }

    public double add(double a, double b) {
        return a + b;
    }
}
```

- add()는 정수형 매개변수를 받는 메서드와 실수형 매개변수를 받는 버전으로 오버로딩

## 오버라이딩 (Overriding)

- 상위 클래스에서 정의한 일반 메서드의 구현을 하위 클래스에서 무시하고 재정의 할 수 있는 기법
- 상속 관계에서 하위 클래스에서 상위 클래스의 메서드를 재정의하여 자식 클래스에서 특화된 동작을 수행할 수 있게 함.
- 예시)

```java
class Animal {
    public void makeSound() {
        System.out.println("Some generic sound");
    }
}
```

```java
class Dog extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Bark! Bark!");
    }
}
```

- Dog 클래스는 Animal 클래스의 makeSound 메서드를 오버라이딩
