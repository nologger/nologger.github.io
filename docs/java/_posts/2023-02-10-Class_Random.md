---
layout: post
title: "Class Random"
---

# {{ page.title }}

> Package : java.util.Random : https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Random.html

* * *

1. 정의

* * *

## 1. 정의

- `public class Random extends Object implements Serializable`
- `java.util.Random` 클래스의 인스턴스는 의사 난수 스트림을 생성하는 데 사용된다.(의사난수란 처음에 주어지는 초기값을 이용하여 이미 결정되어 있는 매커니즘에 의해 상성되는 수를 의미한다.)
- 만약 같은 시드를 사용하여 두개의 인스턴스를 생성한다면, 두 인스턴스에서 생성되는 난수 스트림은 동일하다.
- `java.util.Random` 클래스의 인스턴스는 스레드로부터 안전하지만, 여러 스레드에서 하나의 클래스에서 생성된 인스턴스를 동시에 사용하면 경합이 발생한다. 이로 인해 성능이 저하될 가능성이 존재한다.
- `java.util.Random`클래스를 다중 스레드 환경하에서 사용하여야 한다면, `ThreadLocalRandom`를 사용하는 것을 고려하라.
- `java.util.Random`의 인스턴스는 암호학적으로 안전하지 않다. 암호학적으로 보완된 의사 난수 스트림을 생성하고자 한다면 `SecureRandom`을 사용하는 것을 고려하라.