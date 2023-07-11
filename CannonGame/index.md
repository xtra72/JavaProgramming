# Cannon Game

## 1. 개요



### 1-1. 목적

* Java 기초 과정에서 배운 내용을 연습하고 개발에 필요한 기본 지식 습득을 위한 연습을 목적으로 한다.



### 1-2. 학습 범위

* Java 프로그래밍 연습

* ADT(Abstract Data Type)

* ad hoc polymorphism



### 1-3. 준비

* Library 설치
  * [JUnit5](https://junit.org/junit5/)
  * [Log4J2](https://logging.apache.org/log4j/2.x/index.html)

* Reference

  * [The Java Tutorials](https://docs.oracle.com/javase/tutorial/java/)
  * [JUnit5 User Guide](https://junit.org/junit5/docs/current/user-guide/#overview-getting-started)
  * [Log4J 2 API](https://logging.apache.org/log4j/2.x/manual/api.html)
  * [Testing Java with Visual Studio Code](https://code.visualstudio.com/docs/java/java-testing)



### 1-4. 참고

#### 1-4-1. 필수 라이브러리

프로젝트를 생성하고, 실습에 필요한 라이브러리는 다음과 같다.

* hamcrest-core-1.3.jar+

* junit-jupiter-api-5.9.3.jar+

* Junit-platform-console-standalone-1.9.3.jar+

* log4j-api-2.20.0.jar+

* log4j-core-2.20.0.jar+



#### 1-4-2. Project Package 구성

* source code - org.nhnacademy
* test code - test
* example code - exam


<p align="center">
  <img src="./image/figure01.png" alt="프로젝트 구성"/>
</p>


## 2. Ball World

**Keyword**

* AWT(Abstract Window Toolkit) / Swing
* accessor / mutator


### 2-1. Ball 클래스



#### 2-1-1. 정의

<p align="center">
  <img src="./image/figure02.png" alt="Ball"/>
</p>


* Ball world를 구성하는 기본 요소
* 2차원 평면에서 ball을 설명할 수 있는 최소한의 정보
* 생성 후 이동이나 정보 변경 불가



##### Field(필드)

* 중심좌표(x, y)
  * 생성할 때 지정
* 반지름(radius)
  * 생성할 때 지정
* 색(color)
  * 생성할 때 생략 가능
  * 기본 - 파란색




##### Method(함수)

* ball 정보를 얻기 위한 accessor
  * 중심점 좌표 얻기(getX, getY)
  * 반지름 얻기(getRadius)
  * 색 얻기(getColor)



#### 참고. Accessor와 Mutator

* Accessor
  * 인스턴스 필드 값을 반환
    * private 필드에 대한 접근 지원
    * 클래스 외부에서 직접적인 접근이 필요한 경우에만 지원
  * get + \<field name>
    * getRadius, getColor, ...
  * Getter

* Mutator
  * 인스턴스 필드 값을 변경
    * private 필드에 대한 변경 지원
    * 클래스 외부에서 직접적인 변경이 필요한 경우에만 지원
  * set + \<field name>
    * setRadius, setColor, ...
  * Setter



---

#### 문제 01. Ball 클래스를 구현하라.

* x, y로 이루어진 중심점 좌표, 반지름, 색을 갖는다.
* 중심점 좌표와 반지름은 생성할 때 설정한다.
* 색은 생성할 때 설정할 수 있고, 기본색은 파란색으로 지정한다.
* 각 필드 값을 요청할 수 있다.
* 코드 중복은 최소화하라.
* 코딩 규칙을 따라 작성한다.



**Ball 클래스**

~~~java
package org.nhnacademy;

import java.awt.Color;

public class Ball {
        ...

    public Ball(int x, int y, int radius, Color color) {
        ...
    }

    public Ball(int x, int y, int radius) {
        ...
    }

    public int getX() {
        ...
    }

    public int getY() {
        ...
    }

    public int getRadius() {
        ...
    }

    public Color getColor() {
        ...
    }
}
~~~



**Test code**

~~~java
package test;

import static org.junit.Assert.assertEquals;
import static org.junit.jupiter.api.Assertions.assertAll;

import java.awt.Color;
import java.util.Random;

import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;
import org.apache.logging.log4j.core.config.Configurator;
import org.apache.logging.log4j.Level;

import org.junit.jupiter.api.BeforeAll;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.TestInfo;
import org.nhnacademy.Ball;

public class Exam01 {
    static final int MIN_X = 0;
    static final int MAX_X = 1000;
    static final int MIN_Y = 0;
    static final int MAX_Y = 1000;
    static final int MIN_RADIUS = 10;
    static final int MAX_RADIUS = 100;
    static final int TEST_COUNT = 10;
    static final Color[] colors = { Color.BLUE, Color.RED, Color.WHITE,
        Color.BLACK, Color.GREEN };

    Logger logger = LogManager.getLogger(Exam01.class.getName());
    Random random = new Random();

    @BeforeAll
    static void testInit() {
        Configurator.setLevel(Exam01.class.getName(), Level.WARN);
    }

    @Test
    public void testColorBall(TestInfo testInfo) {
        int x = MIN_X + random.nextInt(MAX_X - MIN_X);
        int y = MIN_Y + random.nextInt(MAX_Y - MIN_Y);
        int radius = MIN_RADIUS + random.nextInt(MAX_RADIUS - MIN_RADIUS);
        Color color = colors[random.nextInt(colors.length)];

        Ball ball = new Ball(x, y, radius, color);
        String message = String.format("%3d, %3d, %3d, %s -> %3d, %3d, %3d %s",
                x, y, radius, color,
                ball.getX(), ball.getY(), ball.getRadius(), ball.getColor());
        logger.info(message);
        assertAll(testInfo.getDisplayName() + "-creation",
                () -> assertEquals(x, ball.getX()),
                () -> assertEquals(y, ball.getY()),
                () -> assertEquals(radius, ball.getRadius()),
                () -> assertEquals(color, ball.getColor()));
    }
}

~~~

* JUnit
  * BeforeAll
    * 테스트 코드가 실행되기 전에 실행됨.
  * Test
    * 테스트 코드
    * 여러 개의 테스트 함수를 정의할 수 있음
  * assertAll
    * 두 번째 매개변수부터 주어지는 조건들이 모두 통과될 경우에만 통과
    * 두 번째 매개변수부터는 Executable이 와야 하며, 순차적으로 실행됨
  * assertEquals
    * 두 개의 매개변수가 equals로 동일해야 함.
* Log4J2
  * LogManager
    * logger 관리
    * 생성되어 있는 logger를 반환하거나 새로 생성 후 반환
  * Configurator
    * Log4J2 설정



**물음**

* Ball 클래스를 정의하고, 테스트 코드가 정상 수행되는가?

* 로그가 출력되나? 무엇을 수정하면 될까?



**추가 문제**

* 테스트 코드를 수정해서 testColorBall을 10번 수행하도록 하라.
  * JUnit [@RepeatedTest](https://junit.org/junit5/docs/current/user-guide/#writing-tests-repeated-tests) annotation 사용
  * 반복 수행 정보를 얻기 위해서는 매개 변수에 [RepetitionInfo](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/RepetitionInfo.html) 추가
* DEFAULT_COLOR를 사용해 생성하는 경우에 대해 테스트 코드를 작성하라.
  * 개별 함수마다  테스트가 수행되도록 할 수 있다.

---



#### 2-1-2. 그리기

* 화면에 그리기는 Java AWT를 사용하므로, AWT에서 요구하는 형식에 맞춰 구성
  * AWT에서는 component를 다시 그려야 하는 시점에 paint 함수를 호출
  * paint함수에서 매개변수로 전달되는  Graphics를 이용해 그리기 가능
* ball을 화면상에 표시하기 위한 함수 정의 필요
  * 그리기(paint)




**참고. AWT를 이용해 ball 그리기**

* awt library에서 원을 그리기 위해  fillOval 사용
* Graphics 클래스에서는 원을 그리기 위한 별도의 방법을 제공하지 않고 타원 그리기 방법 이용
* 원은 폭과 높이가 같은 타원

<p align="center">
<img src="./image/figure03.png" alt="oval"/>
</p>

---

#### 문제 02. ball을 화면에 출력하기.

* awt graphics context를 매개변수로 받아 그릴 수 있도록 함수를 추가한다.
* 도형의 색은 graphics context에서 설정할 수 있다. (setColor)
* 일반적으로 외부의 자원을 활용할 경우, 자원 활용 후 활용 전 설정을 최대한 복원해 두는 것이 좋다.
  따라서, graphics context의 색 설정을 변경하기 전에 기존 색을 저장하였다 복원에 사용하도록 구성한다.



**참고**

* Graphics는 화면에 출력하기 위한 것으로 결과에 대한 검증이 어렵다.
* Graphics 클래스를 확장해서 만든 DummyGraphics를 이용해 해당 함수가 정상적으로 이루어졌는지 확인한다.



**DummyGraphics 클래스**

~~~java
package test;

import java.awt.Color;
...

public class DummyGraphics extends java.awt.Graphics {
    private Color color;
    List<String> callList;
    Map<String, Integer> callCount;
    Map<String, Object> drawOvalParams;
    Map<String, Object> fillOvalParams;
		...

    public DummyGraphics() {
        this.clear();
    }

    public void clear() {
        callList = new LinkedList<>();
        callCount= new HashMap<>();
        drawOvalParams = new HashMap<>();
        fillOvalParams = new HashMap<>();
    }
  	...

    @Override
    public void fillOval(int x, int y, int width, int height) {
        callList.add("fillOval");
        try {
            callCount.put("fillOval", callCount.get("fillOval") + 1);
        } catch(NullPointerException ignore) {
            callCount.put("fillOval", 1);
        }
        fillOvalParams = new HashMap<>();

        fillOvalParams.put("x", x);
        fillOvalParams.put("y", y);
        fillOvalParams.put("width", width);
        fillOvalParams.put("height", height);
        fillOvalParams.put("color", color);
    }
  	...
}
~~~



**테스트 코드**

~~~java
package test;

import static org.junit.Assert.assertEquals;
import static org.junit.jupiter.api.Assertions.assertAll;

import java.awt.Color;
import java.util.Random;

import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;
import org.apache.logging.log4j.core.config.Configurator;
import org.apache.logging.log4j.Level;

import org.junit.jupiter.api.BeforeAll;
import org.junit.jupiter.api.RepeatedTest;
import org.junit.jupiter.api.RepetitionInfo;
import org.junit.jupiter.api.TestInfo;
import org.nhnacademy.Ball;

public class Exam02 {
    static final int MIN_X = 0;
    static final int MAX_X = 1000;
    static final int MIN_Y = 0;
    static final int MAX_Y = 1000;
    static final int MIN_RADIUS = 10;
    static final int MAX_RADIUS = 100;
    static final int TEST_COUNT = 10;
    static final Color[] colors = { Color.BLUE, Color.RED, Color.WHITE,
        Color.BLACK, Color.GREEN };

    Logger logger = LogManager.getLogger(Exam02.class.getName());
    Random random = new Random();
    DummyGraphics graphics = new DummyGraphics();

    @BeforeAll
    static void testInit() {
        Configurator.setLevel(Exam02.class.getName(), Level.ALL);
    }

    @RepeatedTest(10)
    public void testColorBall(RepetitionInfo repetitionInfo, TestInfo testInfo) {
        int x = MIN_X + random.nextInt(MAX_X - MIN_X);
        int y = MIN_Y + random.nextInt(MAX_Y - MIN_Y);
        int radius = MIN_RADIUS + random.nextInt(MAX_RADIUS - MIN_RADIUS);
        Color color = colors[random.nextInt(colors.length)];

        Ball ball = new Ball(x, y, radius, color);

        graphics.clear();
        graphics.setColor(Color.BLACK);

        ball.paint(graphics);
        assertAll(testInfo.getDisplayName()+"-paint",
            ()->assertEquals((x - radius), (int)graphics.getFillOvalParams().get("x")),
            ()->assertEquals((y - radius), (int)graphics.getFillOvalParams().get("y")),
            ()->assertEquals((2 * radius), (int)graphics.getFillOvalParams().get("width")),
            ()->assertEquals((2 * radius), (int)graphics.getFillOvalParams().get("height")),
            ()->assertEquals(color, graphics.getFillOvalParams().get("color")),
            ()->assertEquals(Color.BLACK, graphics.getColor()));

    }
}

~~~

* DummyGraphics
  * clear
    * 내부 수행 정보를 초기화한다.
  * setColor
    * 색을 설정한다.
    * ball을 그린 후 원래 색으로 복원해 두었는지 확인을 위해서 색을 설정한다.
  * getColor
    * 현재 설정되어 있는 색을 반환한다.
  * getFillOvalParam
    * fillOval을 호출하면서 사용된 인수를 저장 후 반환한다.
    * fillOval이 호출되지 않았다면, exception이 발생한다.
    * Map<String, Object>를 반환한다.

---



### 2-2. World 클래스

#### 2-2-1. 정의

* ball이 존재할 공간이면서 화면에 출력될 영역
* Java graphics library인 swing의 JPanel component 확장




##### 필드

* ball 관리

  * ball 목록(ballList)



##### 함수

* ball을 관리하기 위한 함수
  * ball 추가(add)
  * ball 제거(remove)
  * ball 개수(getBallCount)
  * 특정 번째 ball 가져오기 (getBall)
  * 특정 번째 ball 제거하기 (removeBall)
* 화면 출력
  * 그리기(paint)
    * Panel을 다시 그려야 하는 시점에 ball을 그릴 수 있도록 **그리기에 대한 재정의** 필요



---

#### 문제 03. World 클래스를 구현하라.

* JPanel을 확장하여 정의한다.



**테스트 코드**

~~~java
package test;

import java.awt.Color;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;

import javax.swing.JFrame;

import org.apache.logging.log4j.Level;
import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;
import org.apache.logging.log4j.core.config.Configurator;
import org.nhnacademy.World;

public class Exam03 {

    static final int WIDTH = 400;
    static final int HEIGHT = 300;

    public static void main(String [] args) {
        Logger logger = LogManager.getLogger(Exam03.class.getName());
        Configurator.setLevel(Exam03.class.getName(), Level.ALL);

        JFrame frame = new JFrame();
        frame.setSize(WIDTH, HEIGHT);
        frame.addWindowListener(new WindowAdapter() {
            @Override
            public void windowClosing(WindowEvent e) {
                System.exit(0);
            }
        });

        World world = new World();
        logger.info("world 생성 완료");

        frame.add(world);
        logger.info("Frame에 추가 완료");

        frame.setEnabled(true);
        frame.setVisible(true);
        logger.info("화면 출력");
    }
}
~~~

* JFrame을 이용한 Window를 생성하여 출력한다.

* JFrame

  * setSize
    * Window 크기를 설정한다.
  * addWindowListener
    * window에 발생하는 이벤트를 frame에 전달한다.
    * windowClosing 설정을 통해 close 이벤트 생성할 때 System.exit(0)을 이용해 프로그램을 종료한다.
  * add
    * world는 JPanel을 확장하여 정의하였으므로, awt component이다.
    * frame에서 관리하는 component로 추가한다.
  * setEnabled
    * Component 클래스에 정의된 함수로서, component 사용 여부를 설정한다.
  * setVisible
    * Component를 생성한다고 무조건 출력되지는 않는다.
    * visible 설정을 해야 하고, 기본 값은 false로 출력되지 않는다.

**실행 결과**

* 실행 후 아무런 그림이 없는 window가 실행된다.

<p align="center">
  <img src="./image/figure04.png" alg="실행 결과"/>
</p>

* log4j2를 이용해 로그가 출력되도록 구성하였으므로, 실행할 때 다음과 같은 로그가 출력된다.

  ~~~sh
  10:15:01.456 [main] INFO  exam.Exam_2_2_1 - world 생성 완료
  10:15:01.459 [main] INFO  exam.Exam_2_2_1 - Frame에 추가 완료
  10:15:01.505 [main] INFO  exam.Exam_2_2_1 - 화면 출력
  ~~~

---



World 클래스를 정의하고, 테스트 코드를 이용해 world가 생성됨을 확인하였다.

이제 개발하려는 게임의 가장 기본이 되는 world와 ball이 정의되었으므로, world를 이용해 ball을 출력해 보자.



---

#### 문제 04. world를 생성하고, ball을 추가해 출력하라.

* world
  * 크기는 가로 400, 세로 300으로 한다.

* ball
  * 10개를 생성하여 추가한다.
  * 크기는 10에서 50으로 제한한다.
  * 위치는 world 내 임의 위치로 한다.
    * world를 벗어나거나 경계에 걸쳐 출력되지 않도록 한다.

  * 다섯 가지 색 중 임의의 하나를 적용한다.
    * BLUE, RD, WHITE, BLACK, GREEN 


**테스트 코드**

* 테스트 코드 작성이 목적이므로, 문제 2-2-1을 참고한다.

* for문을 사용하지 않는다.



**결과**

* 테스트 코드를 실행한 결과는 다음과 같이 출력된다.

<p align="center">
  <img src="./image/figure05.png" alt="exam_2_2_2_1">
</p>

* 로그 출력은 다음과 같다.

  ~~~sh
  10:14:10.482 [main] INFO  exam.Exam_2_2_2 - world 생성 완료
  10:14:10.485 [main] INFO  exam.Exam_2_2_2 - ball 추가 : 215, 254,  16, ffff0000
  10:14:10.485 [main] INFO  exam.Exam_2_2_2 - ball 추가 :  64, 177,  31, ffffffff
  10:14:10.486 [main] INFO  exam.Exam_2_2_2 - ball 추가 : 206,  63,  37, ff000000
  10:14:10.486 [main] INFO  exam.Exam_2_2_2 - ball 추가 : 351, 271,  37, ff0000ff
  10:14:10.486 [main] INFO  exam.Exam_2_2_2 - ball 추가 : 340, 212,  38, ffffffff
  10:14:10.486 [main] INFO  exam.Exam_2_2_2 - ball 추가 : 362,  73,  42, ff0000ff
  10:14:10.486 [main] INFO  exam.Exam_2_2_2 - ball 추가 : 348,  52,  29, ff0000ff
  10:14:10.486 [main] INFO  exam.Exam_2_2_2 - ball 추가 :  58, 128,  48, ff000000
  10:14:10.486 [main] INFO  exam.Exam_2_2_2 - ball 추가 : 171, 148,  17, ffff0000
  10:14:10.486 [main] INFO  exam.Exam_2_2_2 - ball 추가 : 113,  54,  17, ff000000
  10:14:10.486 [main] INFO  exam.Exam_2_2_2 - Frame에 추가 완료
  10:14:10.535 [main] INFO  exam.Exam_2_2_2 - 화면 출력
  ~~~

---



## 3. Movable Ball World

Ball과 World를 만들어 보았다. 하지만, 단순히 다양한 모양의 ball을 그려주는 과정만 수행하였다. 이제는 ball을 이동시키며 이와 관련된 여러 가지 것들을 알아보도록 하자



ball의 이동은 시간이 변화함에 따라 위치가 변화함을 말한다.

공간에서 ball의 이동을 나타내기 위해서는 단위 시간과 단위 시간 동안의 이동 거리가 있어야 한다.

단위 시간 dt는 화면을 구성하는 시간 간격 또는 행위를 수행할 의미 있는 단위 시간으로  Ball이나 World에서는 Ball을 이동시키고 화면을 출력하는 과정이 될 것이다.

<p align="center">
  <img src="./image/figure06.png" alt="단위 시간 변화">
</p>



### 3-1. MovableBall 클래스

#### 3-1-1. 정의

* Ball 클래스

* 공간(world)에서 이동시킬 수 있다.

  * 특정 위치로 옮길 수 있다.
  * 설정된 변화량만큼 이동하여 옮길 수 있다.

* 변화량을 가진다.

  * 이동 명령(move)에 따라 지정된 변화량만큼 이동한다.

<p align="center">
  <img src="./image/figure07.png" alt="Movable Ball">
</p>

  * 변화량은 변경할 수 있다.



##### 필드

* 단위 시간 동안의 x축 이동량을 나타내는 $dx$(dx)
* 단위 시간 동안의 y축 이동량을 나타내는 $dy$(dy)



##### 함수

* 단위 시간당 이동량 설정하고 얻어오기(setDX, setDY, getDX, getDY)
* 단위 시간만큼 이동시키기(move)
* 특정 위치로 옮기기(moveTo)



---

#### 문제 05. MovableBall 클래스를 구현하라

* ball의 위치 정보 x, y이고, Ball 클래스의 private 정보이다.
* MovableBall 클래스 이동 구현을 위해서는 x, y를 변경할 수 있는 mutator가 필요하다.
* Ball 클래스에 x, y에 대한 mutator를 추가한다. 단, package 외부에서는 접근이 되지 않도록 해야 한다.



**테스트 코드**

~~~java
package test;

import static org.junit.Assert.assertEquals;
import static org.junit.jupiter.api.Assertions.assertAll;

import java.awt.Color;
import java.util.Random;

import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;
import org.apache.logging.log4j.core.config.Configurator;
import org.apache.logging.log4j.Level;

import org.junit.jupiter.api.BeforeAll;
import org.junit.jupiter.api.RepeatedTest;
import org.junit.jupiter.api.TestInfo;
import org.nhnacademy.MovableBall;

public class Exam_3_1_1 {
    class Pair {
        public int x;
        public int y;
        public Pair(int x, int y) {
            this.x = x;
            this.y = y;
        }
    }

    static final int MIN_X = 0;
    static final int MAX_X = 1000;
    static final int MIN_Y = 0;
    static final int MAX_Y = 1000;
    static final int MIN_DX = 10;
    static final int MAX_DX = 100;
    static final int MIN_DY = 10;
    static final int MAX_DY = 100;
    static final int MIN_RADIUS = 10;
    static final int MAX_RADIUS = 100;
    static final int MIN_STEP = 10;
    static final int MAX_STEP = 100;
    static final Color[] colors = { Color.BLUE, Color.RED, Color.WHITE,
        Color.BLACK, Color.GREEN };

    Logger logger = LogManager.getLogger(Exam_3_1_1.class.getName());
    Random random = new Random();

    @BeforeAll
    static void testInit() {
        Configurator.setLevel(Exam_3_1_1.class.getName(), Level.ALL);
    }

    @RepeatedTest(10)
    public void testColorBall(TestInfo testInfo) {
        final Pair pair = new Pair(MIN_X + random.nextInt(MAX_X - MIN_X),
                MIN_Y + random.nextInt(MAX_Y - MIN_Y));
        int dx = MIN_DX + random.nextInt(MAX_DX - MIN_DX);
        int dy = MIN_DY + random.nextInt(MAX_DY - MIN_DY);
        int radius = MIN_RADIUS + random.nextInt(MAX_RADIUS - MIN_RADIUS);
        int stepCount = MIN_STEP + random.nextInt(MAX_STEP - MIN_STEP);

        MovableBall ball = new MovableBall(pair.x, pair.y, radius);
        String message = String.format("creation : %3d, %3d, %3d",
                pair.x, pair.y, radius);
        logger.info(message);
        ball.setDX(dx);
        ball.setDY(dy);
        message = String.format("set DX/DY : %3d, %3d", dx, dy);
        logger.info(message);

        for(int i = 0 ; i < stepCount ; i++) {
            pair.x += dx;
            pair.y += dy;
            ball.move();
            assertAll(testInfo.getDisplayName(),
                    () -> assertEquals(pair.x, ball.getX()),
                    () -> assertEquals(pair.y, ball.getY()));
            message = String.format("move : %3d, %3d", ball.getX(), ball.getY());
            logger.info(message);
        }
    }
}
~~~

* JUnit

  * Pair 클래스를 정의하고, location을 final로 생성하여 사용한다.



**실행 결과**

* 테스트 코드가 성공적으로 수행되면 아래와 같은 로그가 출력된다.

  ~~~sh
  11:29:13.965 [main] INFO  test.Exam_3_1_1 - creation : 645, 297,  93
  11:29:13.966 [main] INFO  test.Exam_3_1_1 - set DX/DY :  31,  15
  11:29:13.968 [main] INFO  test.Exam_3_1_1 - move : 676, 312
  11:29:13.968 [main] INFO  test.Exam_3_1_1 - move : 707, 327
  11:29:13.968 [main] INFO  test.Exam_3_1_1 - move : 738, 342
  11:29:13.969 [main] INFO  test.Exam_3_1_1 - move : 769, 357
  11:29:13.969 [main] INFO  test.Exam_3_1_1 - move : 800, 372
  11:29:13.969 [main] INFO  test.Exam_3_1_1 - move : 831, 387
  ...
  11:29:13.970 [main] INFO  test.Exam_3_1_1 - move : 1513, 717
  11:29:13.976 [main] INFO  test.Exam_3_1_1 - creation : 832, 939,  17
  11:29:13.977 [main] INFO  test.Exam_3_1_1 - set DX/DY :  14,  40
  11:29:13.977 [main] INFO  test.Exam_3_1_1 - move : 846, 979
  11:29:13.977 [main] INFO  test.Exam_3_1_1 - move : 860, 1019
  11:29:13.977 [main] INFO  test.Exam_3_1_1 - move : 874, 1059
  11:29:13.977 [main] INFO  test.Exam_3_1_1 - move : 888, 1099
  11:29:13.977 [main] INFO  test.Exam_3_1_1 - move : 902, 1139
  ...
  ~~~



 **물음**

* 테스트 코드에서 location을 따로 정의해서 사용하는 이유는? x, y를 직접 사용할 경우, 어떠한 문제가 있는가?



---



### 3-2. MovableWorld 클래스

#### 3-2-1. 정의

* World 클래스
* 공간에서 ball을 단위 시간 단위로 이동시킨다
  * 단위 시간 단위 이동이란?
    * 추가된 ball을 한번 이동시키는 걸 말한다.



##### 필드

* 실행하는 동안 이동 횟수(movementCount)

* 최대 이동 횟수(maxMoveCount)



##### 함수

* 상태를 초기화한다(reset)
  * 설정한 maxMovementCount는 초기화하지 않는다.

* 단위 시간 단위 이동(move)
  * 호출 시 등록된 볼 중에서 이동할 수 있는 MovableBall만 1회 이동시킨다
  * 이동하고 나면 화면을 다시 그려야 한다.
    * AWT에서는 component를 다시  그리기 위해 repaint 함수 지원

  * 이동 횟수를 저장한다.
  * 최대 이동 횟수를 넘지는 않는다.

* 지정한 횟수 또는 시간 동안 ball을 이동시킨다. (run)

  * 최대 이동 횟수가 0이면, 계속 이동한다.

* 이동 횟수를 반환한다. (getMovementCount)
* 최대 이동 횟수를 반환한다.(getMaxMovementCount)
* 최대 이동 횟수를 설정한다.(setMaxMovementCount)



---

#### 문제 06. MovableWorld 클래스를 구현하라.

* World 클래스의 필드를 직접 접근해야 할 경우, World 클래스를 수정하고 직접적인 접근은 방지하라.
* 테스트 코드를 이용해 확인한다.



**테스트 코드**

~~~java
package test;

import static org.junit.Assert.assertEquals;
import static org.junit.jupiter.api.Assertions.assertAll;

import java.awt.Color;
import java.util.Random;

import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;
import org.apache.logging.log4j.core.config.Configurator;
import org.apache.logging.log4j.Level;

import org.junit.jupiter.api.BeforeAll;
import org.junit.jupiter.api.RepeatedTest;
import org.junit.jupiter.api.RepetitionInfo;
import org.junit.jupiter.api.TestInfo;
import org.nhnacademy.MovableBall;
import org.nhnacademy.MovableWorld;

public class Exam_3_2_1 {
    class Pair {
        public int x;
        public int y;
        public Pair(int x, int y) {
            this.x = x;
            this.y = y;
        }
    }

    static final int MIN_X = 0;
    static final int MAX_X = 1000;
    static final int MIN_Y = 0;
    static final int MAX_Y = 1000;
    static final int MIN_DX = 10;
    static final int MAX_DX = 100;
    static final int MIN_DY = 10;
    static final int MAX_DY = 100;
    static final int MIN_RADIUS = 10;
    static final int MAX_RADIUS = 100;
    static final int MIN_STEP = 10;
    static final int MAX_STEP = 100;
    static final Color[] colors = { Color.BLUE, Color.RED, Color.WHITE,
        Color.BLACK, Color.GREEN };

    Logger logger = LogManager.getLogger(Exam_3_2_1.class.getName());
    Random random = new Random();

    @BeforeAll
    static void testInit() {
        Configurator.setLevel(Exam_3_2_1.class.getName(), Level.ALL);
    }

    @RepeatedTest(10)
    public void testColorBall(RepetitionInfo repetitionInfo, TestInfo testInfo) {
        final Pair location = new Pair(MIN_X + random.nextInt(MAX_X - MIN_X),
                MIN_Y + random.nextInt(MAX_Y - MIN_Y));
        int dx = MIN_DX + random.nextInt(MAX_DX - MIN_DX);
        int dy = MIN_DY + random.nextInt(MAX_DY - MIN_DY);
        int radius = MIN_RADIUS + random.nextInt(MAX_RADIUS - MIN_RADIUS);
        int stepCount = MIN_STEP + random.nextInt(MAX_STEP - MIN_STEP);

        String message = String.format("Repetition : %3d / %3d",
                repetitionInfo.getCurrentRepetition(),
                repetitionInfo.getTotalRepetitions());
        logger.info(message);
        MovableWorld world = new MovableWorld();
        world.setMaxMovementCount(stepCount);
        message = String.format("set max movement count : %3d -> %3d",
                stepCount, world.getMaxMovementCount());
        logger.info(message);
        assertEquals(stepCount, world.getMaxMovementCount());

        MovableBall ball = new MovableBall(location.x, location.y, radius);
        message = String.format("creation : %3d, %3d, %3d",
                location.x, location.y, radius);
        logger.info(message);
        ball.setDX(dx);
        ball.setDY(dy);
        message = String.format("set DX/DY : %3d, %3d", dx, dy);
        logger.info(message);

        location.x += dx * stepCount;
        location.y += dy * stepCount;
        message = String.format("target : %3d, %3d", location.x, location.y);
        logger.info(message);

        world.add(ball);
        logger.info("ball added");
        logger.info("world run start");
        world.run();
        logger.info("world run finished");

        assertAll(testInfo.getDisplayName(),
                () -> assertEquals(location.x, ball.getX()),
                () -> assertEquals(location.y, ball.getY()));
        message = String.format("ball location : %3d, %3d", ball.getX(), ball.getY());
        logger.info(message);
    }
}
~~~



**실행 로그**

~~~sh
14:04:25.718 [main] INFO  test.Exam_3_2_1 - Repetition :   1 /  10
14:04:25.990 [main] INFO  test.Exam_3_2_1 - set max movement count :  28 ->  28
14:04:25.991 [main] INFO  test.Exam_3_2_1 - creation :  44, 668,  10
14:04:25.991 [main] INFO  test.Exam_3_2_1 - set DX/DY :  45,  59
14:04:25.992 [main] INFO  test.Exam_3_2_1 - target : 1304, 2320
14:04:25.992 [main] INFO  test.Exam_3_2_1 - ball added
14:04:25.992 [main] INFO  test.Exam_3_2_1 - world run start
14:04:25.992 [main] INFO  test.Exam_3_2_1 - world run finished
14:04:25.994 [main] INFO  test.Exam_3_2_1 - ball location : 1304, 2320
...
14:04:26.015 [main] INFO  test.Exam_3_2_1 - Repetition :  10 /  10
14:04:26.015 [main] INFO  test.Exam_3_2_1 - set max movement count :  38 ->  38
14:04:26.015 [main] INFO  test.Exam_3_2_1 - creation : 771, 855,  93
14:04:26.015 [main] INFO  test.Exam_3_2_1 - set DX/DY :  34,  31
14:04:26.015 [main] INFO  test.Exam_3_2_1 - target : 2063, 2033
14:04:26.015 [main] INFO  test.Exam_3_2_1 - ball added
14:04:26.015 [main] INFO  test.Exam_3_2_1 - world run start
14:04:26.015 [main] INFO  test.Exam_3_2_1 - world run finished
14:04:26.015 [main] INFO  test.Exam_3_2_1 - ball location : 2063, 2033
~~~

---



MovableWorld 클래스를 정의하고, 테스트 코드를 이용해 기본 동작은 확인하였다.

 실제 화면상에도 동일하게 동작하는지 확인해 보자.



---

#### 문제 07. MovableWorld 클래스 구동을 위한 프로그램을 만들어 확인하라.

* JFrame을 이용해 window를 만들고, MovableWorld를 component로 사용해 적용하라.
  * Window 크기는 가로 400, 세로 300으로 한다.

* 10개의 ball을 생성하라.
  * 시작 위치 window 내로 제한한다.
  * 크기는 반지름 10~50 사이로 한다.
  * 변화량은 가로, 세로 10에서 30 사이로 한다.
  * ball 간 중첩되지 않도록 한다.
    * ball 간 중첩은 ball에서 지원하는 함수로 구현할 수 있다.




**실행 결과**



* 이동 횟수가 적을 경우, 다음과 같이 멈춰 있다.

<p align="center">
  <img src="./image/figure08.png" alt="이동 횟수 적음">
</p>

* 이동 횟수가 조금만 늘어도 화면에는 몇 개 보이지 않는다.

<p align="center">
  <img src="./image/figure09.png" alt="이동 횟수 중간">
</p>

* 이동 횟수가 조금만 많아져도 출력되는 ball은 하나도 없다.

<p align="center">
  <img src="./image/figure10.png" alt="이동 횟수 많음">
</p>

**물음**

* MovableWorld에서는 ball이 이동해야 한다. ball이 이동하는가?

  확인을 위해 코드를 일부 수정하여 ball의 위치를 확인해 보자.

  ~~~java
  public class MovableBall extends Ball {
    Logger logger;
    ...
      public void move() {
          ...
          logger.debug("X : {}, Y : {}", getX(), getY());
    }
  }
  ~~~

  출력 터미널에는 아래와 같이 출력된다.

  ~~~shell
  18:07:22.562 [main] DEBUG exam.Exam_3_2_2 - world created
  18:07:22.571 [main] DEBUG exam.Exam_3_2_2 - add ball : 331, 187, 36
  18:07:22.623 [main] DEBUG MovableBall - 360, 194
  18:07:22.623 [main] DEBUG MovableBall - 389, 201
  18:07:22.623 [main] DEBUG MovableBall - 418, 208
  18:07:22.623 [main] DEBUG MovableBall - 447, 215
  18:07:22.623 [main] DEBUG MovableBall - 476, 222
  18:07:22.623 [main] DEBUG MovableBall - 505, 229
  18:07:22.623 [main] DEBUG MovableBall - 534, 236
  18:07:22.623 [main] DEBUG MovableBall - 563, 243
  18:07:22.623 [main] DEBUG MovableBall - 592, 250
  18:07:22.623 [main] DEBUG MovableBall - 621, 257
  ~~~

  시작은 설정한 위치에서 시작해서 설정된 변화량만큼씩 증가되어 정상 동작으로 보이지만, 반복 횟수가 증가하면서 MovableWorld의 영역을 벗어난 것을 알 수 있다.

  그리고, X축 10, Y축 10의 변화량을 주었는데 ball의 이동을 보면 좌측 상단에서 우측하단으로 이동한다.

  정리하면 위 테스트 프로그램에서는 3가지 문제점을 확인할 수 있다.

  * ball의 이동이 보이질 않는다. (아주 느린 장비에서는 보일 수도 있다)
  * ball이 공간을 벗어난다.
  * ball의 이동이 좌측 하단에서 우측 상단이 아니라 좌측 상단에서 우측 하단으로 의도와 다르게 움직인다.

  무엇이 문제일?

---

### 3-3. ball의 이동 시간

* MovableWorld는 아래 그림과 같이  $dt$ 간격마다 ball을 이동시킴

<p align="center">
  <img src="./image/figure11.png" alt="Zero DT">
</p>

* 앞에서 구현한 MovableWorld에서는 $dt$에 대해 정의하지 않음

* $dt$ 가 아주 작은 값을 주거나 0이라면 결과는 어떻게 될까?



---

#### 문제 08. 다음 코드는 MovableBall을 수정하여 ball 이동이 보이지 않는 문제에 관해 확인해 보자.

MovableWorldTest에서는 dt를 설정하고 있지 않다. MovableBall을 수정하여 다시 확인해 보자.

~~~java
public class MovableBall extends Ball {
    private long movementCount = 0;
  	...

    public void move() {
        ...
        logger.debug("{} : {}, {}, {}",
                     getMovementCount(), System.currentTimeMillis(), getX(), getY());
 		}
}
~~~

* World 클래스에 로그 출력을 위한 로거 추가

* 이동 횟수(movementCount)를 추가한다.

* System 클래스에서 시스템의 현재시간을 밀리초 단위로 구할 수 있다.



**실행 로그**

~~~shell
18:16:25.206 [main] DEBUG exam.Exam_3_3_1 - world created
18:16:25.214 [main] DEBUG exam.Exam_3_3_1 - add ball : 201, 108, 41, 0, -17
18:16:25.266 [main] DEBUG MovableWorld - start : 1686647785266
18:16:25.269 [main] DEBUG MovableBall - 1, 1686647785266, 201, 91
18:16:25.270 [main] DEBUG MovableBall - 2, 1686647785270, 201, 74
18:16:25.270 [main] DEBUG MovableBall - 3, 1686647785270, 201, 57
18:16:25.270 [main] DEBUG MovableBall - 4, 1686647785270, 201, 40
18:16:25.270 [main] DEBUG MovableBall - 5, 1686647785270, 201, 23
...
18:16:25.274 [main] DEBUG MovableBall - 53, 1686647785274, 201, -793
18:16:25.274 [main] DEBUG MovableBall - 54, 1686647785274, 201, -810
18:16:25.274 [main] DEBUG MovableBall - 55, 1686647785274, 201, -827
18:16:25.274 [main] DEBUG MovableBall - 56, 1686647785274, 201, -844
...
18:16:25.276 [main] DEBUG MovableBall - 98, 1686647785276, 201, -1558
18:16:25.276 [main] DEBUG MovableBall - 99, 1686647785276, 201, -1575
18:16:25.276 [main] DEBUG MovableBall - 100, 1686647785276, 201, -1592
18:16:25.276 [main] DEBUG MovableWorld - finished : 1686647785276 - 10
~~~

 각 라인은 화면이 출력될 때마다 시간과 ball의 위치를 나타낸다.

시간은 밀리초(milliseconds) 단위로 표시되는데, ball은 100번 이동하는 동안 최대 10밀리 초가 소모되지 않았음을 알 수 있다.

---



#### 3-3-1. 단위시간 dt

단위 시간을 추가해 보자.

단위 시간은 ball의 이동 간격 사이 일정 시간 기다림으로 구현할 수 있으며, Thread.sleep()을 이용해 밀리초 단위로 설정할 수 있다

* 단위 시간 저장을 위한 필드를 추가한다(dt)
* 단위 시간 설정 관련 함수를 추가한다.(setDT, getDT)
* run 함수를 수정하여, move 후 지정된 단위 시간만큼 대기 상태가 되도록 한다.



---

#### 문제 09. MovableWorld 클래스에 단위 시간을 추가하라

*  move 함수가 호출된 후 설정된 단위 시간만큼 대기한다.



단위 시간을 추가한 후 로그 출력은 다음과 같다.

**로그 출력**

~~~sh
2:28:10.274 [main] DEBUG exam.Exam_3_3_2 - world created
22:28:10.285 [main] DEBUG exam.Exam_3_3_2 - add ball : 217, 281, 28, 13, -25
22:28:10.337 [main] DEBUG MovableWorld - start : 1686662890337
22:28:10.337 [main] DEBUG MovableBall - 1, 1686662890337, 230, 256
22:28:10.350 [main] DEBUG MovableBall - 2, 1686662890350, 243, 231
22:28:10.362 [main] DEBUG MovableBall - 3, 1686662890362, 256, 206
22:28:10.375 [main] DEBUG MovableBall - 4, 1686662890375, 269, 181
22:28:10.388 [main] DEBUG MovableBall - 5, 1686662890388, 282, 156
22:28:10.400 [main] DEBUG MovableBall - 6, 1686662890400, 295, 131
22:28:10.412 [main] DEBUG MovableBall - 7, 1686662890412, 308, 106
22:28:10.425 [main] DEBUG MovableBall - 8, 1686662890425, 321, 81
22:28:10.438 [main] DEBUG MovableBall - 9, 1686662890438, 334, 56
...
22:28:11.080 [main] DEBUG MovableBall - 60, 1686662891080, 997, -1219
22:28:11.093 [main] DEBUG MovableBall - 61, 1686662891093, 1010, -1244
22:28:11.106 [main] DEBUG MovableBall - 62, 1686662891106, 1023, -1269
22:28:11.119 [main] DEBUG MovableBall - 63, 1686662891119, 1036, -1294
22:28:11.132 [main] DEBUG MovableBall - 64, 1686662891132, 1049, -1319
...
22:28:11.576 [main] DEBUG MovableBall - 99, 1686662891576, 1504, -2194
22:28:11.587 [main] DEBUG MovableBall - 100, 1686662891587, 1517, -2219
22:28:11.599 [main] DEBUG MovableWorld - finished : 1686662891599 - 1262
~~~

* 100회 이동하는 동안 1,262밀리 초 소모

* 단위 시간을 10밀리초로 설정하였기에 계산상으로는 1,000밀리 초 소모되어야 정상

  * move 1회 호출 후 무조건 10 밀리초 대기(마지막도 동일함)

**물음**

* 로그로 출력된 소요 시간이 1,262밀리 초인 이유는?

---



수행 시간은 다음 그림과 같다.

move 간 단위 시간(dt)을 줄 경우 실제 수행시간은 $T=dt * n$ 이 아니다.

이는 move 처리시간을 감안하지 않을 것으로서 실제 수행 시간은 $T = (\alpha + dt) * n$ 가 된다.

<p align="center">
  <img src="./image/figure12.png" alt="단위시간 오차">
</p>

---

#### 문제 10. MovableWorld에서 move 후 변경되는 시간의 오차를 최소화하라.

이 문제는 간단하다.

move를 수행한 후 다음 move를 호출하기까지의 대기 시간을 단위 시간이 아닌 예정 시간까지 남은 시간으로 하면 된다.

다시 말해, move와 다시 그리기 등의 추가 작업을 위해 1ms를 소비했다고 한다면, 다음 move를 위해 대기 시간은 단위 시간 - 1이 되어야 할 것이다.



다음 그림은 실제 수행 작업을 고려한 단위 시간을 나타낸 것이다.

<p align="center">
  <img src="./image/figure13.png" alt="단위시간 오차 보정">
</p>

수정한 결과가 아래와 같이 출력되는지 확인해 보자.

**로그 출력**

~~~sh
23:02:58.564 [main] DEBUG exam.Exam_3_3_2 - world created
23:02:58.573 [main] DEBUG exam.Exam_3_3_2 - add ball : 238, 224, 34, -27, -29
23:02:58.620 [main] DEBUG MovableWorld - start : 1686664978620
23:02:58.621 [main] DEBUG MovableBall - 1, 1686664978621, 211, 195
23:02:58.632 [main] DEBUG MovableBall - 2, 1686664978632, 184, 166
23:02:58.641 [main] DEBUG MovableBall - 3, 1686664978641, 157, 137
23:02:58.652 [main] DEBUG MovableBall - 4, 1686664978652, 130, 108
23:02:58.661 [main] DEBUG MovableBall - 5, 1686664978661, 103, 79
23:02:58.672 [main] DEBUG MovableBall - 6, 1686664978672, 76, 50
...
23:02:59.062 [main] DEBUG MovableBall - 45, 1686664979062, -977, -1081
23:02:59.072 [main] DEBUG MovableBall - 46, 1686664979072, -1004, -1110
23:02:59.083 [main] DEBUG MovableBall - 47, 1686664979083, -1031, -1139
23:02:59.091 [main] DEBUG MovableBall - 48, 1686664979091, -1058, -1168
23:02:59.102 [main] DEBUG MovableBall - 49, 1686664979102, -1085, -1197
...
23:02:59.582 [main] DEBUG MovableBall - 97, 1686664979582, -2381, -2589
23:02:59.592 [main] DEBUG MovableBall - 98, 1686664979592, -2408, -2618
23:02:59.603 [main] DEBUG MovableBall - 99, 1686664979603, -2435, -2647
23:02:59.611 [main] DEBUG MovableBall - 100, 1686664979611, -2462, -2676
23:02:59.621 [main] DEBUG MovableWorld - finished : 1686664979621 - 1001
~~~



결과는 1,001밀리 초로 거의 비슷한 결과를 보인다. 나머지 오차는 단위 문제 및 기타 연산의 영향으로 차이 날 수 있다.

**물음**

* 0.999 밀리초에서 시작해 1.001 밀리초에 끝났다고 가정하자. 이 과정의 수행 시간은?

---



## 4. Bounded Ball World

**Keyword**

* Collision detection
* Bounds




ball은 시간이 흐름에 따라 지정된 방향으로 이동한다. 그리고, 정해진 공간을 벗어나더라도 이를 알지 못하고 계속 이동해 버려 결국에는 공간을 벗어나 보이지 않게 된다.

경계가 있는 세상이란 정해진 공간의 외곽에는 보이지 않는 벽으로 구성되어 있고, 벽에 ball이 부딪칠 경우 멈추거나 튕겨 나와야 한다.

사각의 경계가 있는 세상에서 ball이 벽에 부딪히는 경우는 아래 그림과 같을 것이다.

<img src="./image/figure14.png" alt="닫힌 공간에서의 볼"/>



이를 앞에서 설명한 위치 변화로 표현하면 다음과 같다.

1. 왼쪽 벽 : (dx, dy) -> (-dx, dy)
2. 아래쪽 벽 : (dx, dy) -> (dx, -dy)
3. 오른쪽 벽 : (dx, dy) -> (-dx, dy)
4. 위쪽 벽 : (dx, dy) -> (dx, -dy)



설명에 따라 ball이 벽에 부딪혔을 경우 ball이 가지고 있던 단위 변화량만 변경해서 주면 된다.

이제 결정이 필요하다. 누가 이 작업을 해줄 것인가? ball? world?

ball 스스로가 변경 작업을 해야 한다면 충돌감지를 판단할 수 있는 정보가 제공되어야 할 것이고, world가 변경 작업을 도와준다면 ball의 위치를 항상 감시해야 할 것이다.

두 가지 방법 모두 만들어 보도록 하자.



### 4-1. BoundedBall 클래스

#### 4-1-1. 정의

* 경계영역 정보를 가진다.
* 경계영역은 벽으로 막혀 있다.
* 벽에 부딪히면 튕겨 난다.
* 벽은 무한히 단단하여 부딪힌 속도로 튕겨져 나온다.



##### 필드

BoundedBall에는 자신이 움직일 수 있는 영역 정보가 필요하다

* 경계 정보(bounds)



##### 함수

* 경계영역과 관련된 함수가 추가된다.

  * 경계 정보를 설정하거나 반환한다.(getBounds, setBounds)

  * 이동 후 경계를 벗어났는지 확인한다.(isOutOfBounds)

* 이동 중 경계영역을 만난 경우, 추가 동작을 한다.(move)

* 경계를 벗어 경우 벽에서 튕긴다.(bounce)





---

#### 문제 11. BoundedBall 클래스를 구현하라.

- 경계영역은 사각형으로 설정한다.

  - AWT에서 Rectangle 클래스를 지원한다.
  - Rectangle 클래스에는 두 개의 사각형이 겹쳤는지 확인하거나, 겹친 영역을 확인할 수 있는 함수가 제공된다.
  -  ball과 경계영역이 겹치는 것은  ball을 둘러싸는 최소한의 사각형이 경계영역과 겹치는 것이 동일하다.
  - 특정한 점이 사격형을 벗어난 것은 contains 함수로도 알 수 있다.

<p align="center">
  <img src="./image/figure15.png" alt="exam_4_1_1_1">
</p>

- 벽에 튕기는 것은 다음의 경우로 분류된다.

  - 왼쪽이나 오른쪽 벽에 부딪힐 경우, X의 이동 방향이 변경된다. 즉, X축의 변화량 $dx$가 변경된다.

    - ball의 왼쪽 끝부분이 경계영역을 벗어나면 왼쪽 벽에 부딪힌 것이다.
    - ball의 오른쪽 끝부분이 경계영역을 벗어나면 오른쪽 벽에 부딪힌 것이다.

  - 위쪽이나 아래쪽 벽에 부딪힐 경우, Y의 이동 방향이 변경된다. 즉, Y축의 변화량 $dy$가 변경된다.

    - ball의 위쪽 끝부분이 경계영역을 벗어나면 위쪽 벽에 부딪힌 것이다.
    - ball의 아래쪽 끝부분이 경계영역을 벗어나면 아래쪽 벽에 부딪힌 것이다.

<p align="center">
  <img src="./image/figure16.png" alt="exam_4_1_1_1">
</p>



**실행 결과**

<p align="center">
  <img src="./image/figure17.png" alt="실행 결과">
</p>

* 경계영역을 벗어난 경우, 튕겨 나간다.
* ball이 경계영역에 벗어나는 시점에 튕겨 나가지 않고, 일부는 영역을 벗어났다 튕겨 나간다.
* 왜 그럴까? 해결 방법은?


---



오른쪽 경계에 부딪힌 후 튕겨 난 경우는 아래 그림과 같다.



<p align="center">
  <img src="./image/figure18.png" alt="bounce 보정">
</p>

 그림을 바탕으로 오른쪽 경계 충돌 후 이동 후 좌표를 계산하면 아래와 같다.
$$
\begin{align*}
X_R & = X_2 - r\\
x_2 &= x_1 + |d_x|\\
x_3 &= X_R - (|d_x| - (X_R - x_1))\\
    &= 2X_R - x_1 - |d_x|\\
    &= 2{(X_2 - r)} - x_1 - |d_x|\\
    &= 2{(X_2 - r)} - x_2\\
y_3 &= y_1 + dy\\
\end{align*}
$$


 그림을 바탕으로 왼쪽 경계 충돌 후 이동 후 좌표를 계산하면 아래와 같다.
$$
\begin{align*}
X_L & = X_1 + r\\
x_2 & = x_1 - |d_x|\\
x_3 &= X_L + (|d_x| - (x_1 - X_L))\\
    &= 2X_L - x_1 + |d_x|\\
    &= 2{(X_1 + r)} - x_1 + |d_x|\\
    &= 2{(X_1 + r)} - (x_1 - |d_x|)\\
    &= 2{(X_1 + r)} - x_2\\
y_3 &= y_1 + d_y\\
\end{align*}
$$
그림을 바탕으로 위쪽 경계 충돌 후 이동 후 좌표를 계산하면 아래와 같다.
$$
\begin{align*}
Y_T & = Y_2 - r\\
y_2 &= y_1 + |d_y|\\
y_3 &= Y_T - (|d_y| - (Y_T - y_1))\\
    &= 2Y_T - y_1 - |d_y|\\
    &= 2{(Y_2 - r)} - y_1 - |d_y|\\
    &= 2{(Y_2 - r)} - y_2\\
x_3 &= x_1 + d_x\\
\end{align*}
$$


 그림을 바탕으로 아래쪽 경계 충돌 후 이동 후 좌표를 계산하면 아래와 같다.
$$
\begin{align*}
Y_B & = Y_1 + r\\
y_2 & = y_1 - |d_y|\\
y_3 &= Y_B + (|d_y| - (y_1 - Y_B))\\
    &= 2Y_B - y_1 + |d_y|\\
    &= 2{(Y_1 + r)} - y_1 + |d_y|\\
    &= 2{(Y_1 + r)} - (y_1 - |d_y|)\\
    &= 2{(Y_1 + r)} - y_2\\
x_3 &= x_1 + d_x\\
\end{align*}
$$





---

#### 문제 12. 경계영역을 벗어나지 않도록 수정하라.(어려우면  넘어가기)

* 경계 영역을 벗어난 경우, 추가적인 처리를 통해 위치를 보정하라.
* Rectangle의 contains로 경계 검사를 할 때, 해당 점이 경계 위에 존재할 때 어떻게 처리할지 결정해야 한다.
* 위 식에서 변화량($|d_x|$, $|d_y|$)는 절댓값을 나타냄을 주의하라.(다행히 최종 계산에는 사용되지 않음)



**실행 결과**

<p align="center">
  <img src="./image/figure19.png" alt="실행 결과">
</p>

* 보정식을 적용한 결과는 경계영역을 벗어나는 경우가 보이지 않는다.
* 하지만, 여전히 아래쪽으로 벗어날 수 있다. 이는 경계영역이 보이는 것보다 아래 있기 때문이다.

<p align="center">
  <img src="./image/figure20.png" alt="경계영역">
</p>

---



### 4-2. BoundedWorld 클래스

#### 4-2-1. 정의

BoundedBall 클래스를 구현함으로써 ball을 이용한 닫힌 세상에서 움직임을 확인해 보았다. 그럼, ball이 아닌 world를 이용한 경우는 어떠한지 확인해 보자.



움직이는 ball이 주어진 공간을 벗어나는지에 대해 world에서는 지속적인 감시를 통해 알 수 있다.

또한, 현재까지 구현에서 world는 ball의 움직임을 관리하고 있으므로 더욱더 쉽게 구현할 수 있고 이를 BoundedWorld라고 하자.



BoundedWorld는 ball이 허용 공간을 벗어났는지 확인하고, 그러한 경우 적절하게 이동 방향을 변경하도록 변화량을 재설정해 주어야 한다.



##### 필드

* BoundedWorld는 자신의 공간 정보가 경계 정보가 되므로, 별도의 추가는 필요 없다.



##### 함수

* BoundedWorld는 ball이 경계를 벗어났는지 확인하고, 새로운 위치를 계산해 줄 필요가 있다.

  * world의 영역을 가져온다(getBounds)
  * ball이 경계를 벗어났는지 확인한다(outOfBounds)

  * ball의 새로운 좌표를 계산하여 설정한다.(bounceBall)




---

#### 문제 12. BoundedWorld 클래스를 구현하라.

**getBounds**

* World의 영역에 대한 정보로서 World 클래스에 추가한다.
* awt component에서는 getBounds 함수를 지원하므로, 새롭게 정의할 필요는 없다.



**outOfBounds**

* ball이 world를 벗어났는지 확인한다.

* BoundedWorld 영역과 ball 영역의 중첩 영역을 구해 ball 영역과 다를 경우 벗어난 것으로 판단한다.



**bounceBall**

* ball이 경계영역 벽에 부딪혔을 때 튕겨 나온 위치로 이동시킨다.
* MovableBall만 해당한다.
* BoundedBall의  bounce를 참고한다.



**move**

* ball을 이동시키고, 충돌 검사를 해야 하므로 기능 변경이 필요하다



**테스트 코드**

* Exam_4_2_1.java 참고
* Exam_4_1_1.java를 수정하여 작성
  * MovableWorld를 BoundedWorld로 변경
  * BoundedBall를 MovableBall로 변경하고, bounds 설정 변경



---



### 4-3. 물체 간 충돌

**Keyword**

* Collision detection
* Bounds



BoundedBall은 경계영역을 설정하고 해당 영역을 벗어날 경우, 튕겨져 나온다.

그럼, ball이 하나 이상 존재할 때 다른 ball이 차지하고 있는 공간은 어떻게 해야 할까?

또한, 경계영역은 ball에 허용되는 반면 다른 ball이 차지한 공간의 경우 허용되지 않는 영역이다. 따라서, 공간에 대해 허용 영역이 안인지 밖이지 구별이 필요하다.

<p align="center">
  <img src="./image/figure21.png" alt="물체 간 충돌">
</p>

* 흰색 ball을 기준으로 한다.
* 파란색은 앞에서 정의하고 있는 world가 된다.
* 붉은색 ball은 중첩이 허용되지 않는 다른 물체가 된다.
* 붉은색으로 표시된 영역은 흰색 ball에 허용되지 않는 영역이다.
* world를 기존으로 할 경우 내부 영역이 허용 영역이고, 다른 ball을 기준으로 할 경우, 외부 영역이 허용 영역이 된다.





#### 4-3-1. 충돌 감지



* ball이 겹침은 ball 중간 거리가 두 ball의 반지름 합보다 크면 된다.

<p align="center">
  <img src="./image/figure22.png" alt="ball 간 거리">
</p>

* ball 간 거리는
  $$
  \begin{align*}
  중심 간 거리(D) &= r1+r2+d=\sqrt{{(x1-x2)}^2 + {(y1-y2)}^2}\\
  ball 간 거리(d) &= \sqrt{{(x1-x2)}^2 + {(y1-y2)}^2} - (r1 + r2)
  \end{align*}
  $$


ball 간 거리가 두 ball의 반지름 합보다 작을 경우, 두 ball은 충돌한 상태다.



---

#### 문제 14. 가려지는 ball이 없도록 생성하라

임의의 위치에 생성한 결과 일부 ball이 겹침을 ball 수 있다. 이는 앞서 추가된 ball이 어디에 얼만한 크기로 존재하는지 확인하지 않고 추가해 발생한 문제이다.  world에 ball이 추가될 때 해당 영역을 다른 ball이 없는지 확인하고 추가하도록 수정한다. 만약, 다른 ball이 차지하고 있어 새로운 ball의 추가가 어렵다면 exception을 발생시켜서 다른 위치에 추가될 수 있도록 한다.



**참고**

* 제곱근 함수는 Math.sqrt()를 이용한다.

* 반복해서 ball을 생성할 때, 반드시 for문을 사용해야 하는 것은 아니다.




**실행 결과**

<p align="center">
  <img src="./image/figure23.png" alt="exam_4_3_1_1">
</p>

---



다음 그림은 ball과 box 간 충돌을 나타낸 것이다.

<p align="center">
  <img src="./image/figure24.png" alt="볼과 박스 간 거리">
</p>

$$
\begin{align*}
두 점의 중심 간 거리(d) &= \sqrt {{(x1-x2)}^2 - {(y1-y2)}^2}\\
최소 충돌 거리(c) &= r1 + {w2 \over 2}
\end{align*}
$$


ball과 box의 충돌 역시 복잡해 보이지는 않는다. 중심 간 거리가 최소 충돌 거리도 짧으면 충돌이다.

하지만, 다음 그림을 보자.

<p align="center">
  <img src="./image/figure25.png" alt="볼과 박스 간 거리">
</p>

복잡한 식을 이용하면 구할 수도 있을 것이다.

**하지만, 본 과정에서는 중요한 문제가 되지 못한다.**  물체가 충돌한 조건을 정의하고, 충돌 시 그에 대한 행동만 정의할 수 있으면 된다.



다음 그림은 두 ball의 충돌을 intersects 함수로 이용할 경우를 표현한 것이다.

<p align="center">
  <img src="./image/figure26.png" alt="intersects">
</p>

실제 충돌하지는 않았지만, 충돌한 것으로 가정한다. 대신 box에도 적용할 수 있어 문제를 단순화시킬 수 있다.



---

#### 문제 15. intersects 함수를 이용해 가려지는 ball이 없도록 생성하라.

* Ball 클래스에 있는 충돌 확인 함수를 수정한다..
* Exam_4_3_1.java를 이용해서 확인하라.
  * 무슨 문제가 생기는가?
  * 이유는?

* 생성되는 ball을 크기를 조절해 본다.



**실행 결과**

<p align="center">
  <img src="./image/figure27.png" alt="exam_4_3_2_1">
</p>

* ball 크기를 조절할 경우 배치가 가능하지만, 앞서보다 충돌이 많이 발생한다.

---



---

#### 문제 16. 충돌 부분을 표시하라.(추가)

* ball에 충돌이 발생한 경우, 충돌 부분을 붉은색으로 표시한다.
* 충돌 영역을 얻어 낼 수 있어야 한다.
* 충돌을 감지할 때와 그릴 때가 달라 따로 저장해야 한다.
* 저장된 충돌 영역은 매번 갱신되어야 한다.



결과는 다음과 같다.

<p align="center">
  <img src="./image/figure28.png" alt="충돌">
</p>

---



#### 4-3-2. 충돌 후 튕기기

움직이는 두 ball이 충돌하면 서로 튕겨 나간다. 여기서는 동시에 튕기는 것을 구현하기는 복잡하므로, 문제를 단순화하여 특정 순간에 하나의 ball만 움직여서 고정된 ball에 부딪히는 것으로 한다.

이럴 경우, 움직이던 ball은 어디를 부딪치느냐에 따라 특정한 방향으로 꺾여서 튕겨 나가게 된다.

다음 그림은 두 ball이 충돌하였을 때, 겹치는 부분을 표시한 것이다.

<p align="center">
  <img src="./image/figure29.png" alt="중첩 영역">
</p>

겹친 영역을 번호로 하여, 1, 3, 6, 8은 진행 방향의 반대로, 2, 7은 X축을 기준으로 반대로(즉, dy를 변경), 4, 5는 Y축을 기준으로 반대로 움직이도록 하면 정확하지는 않지만, 충돌 후 튕김을 구현할 수 있다.



큰 ball이 움직일 경우도 마찬가지가 된다.



---

#### 문제 17. 하나의 ball을 고정해 둔 상태에서 다른 하나의 ball을 움직이도록 하여 충돌 시 튕김을 구현하라.

**참고**

* 교차 영역을 구하고 영역의 폭과 높이로 3가지 그룹 중 하나로 구분할 수 있다

---



## 5. 간추려 사용하기

**Keyworld**

* ADT
* Vector(mathematics and physics)



### 5-1. Region 활용하기

Ball 클래스 코드를 보도록 하자.

~~~java
package org.nhnacademy;

import java.awt.Color;
import java.awt.Graphics;
import java.awt.Rectangle;

import org.apache.logging.log4j.Level;
import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;
import org.apache.logging.log4j.core.config.Configurator;

public class Ball {
    public static final Color DEFAULT_COLOR = Color.BLUE;
    private int x;
    private int y;
    private int radius;
    private Color color;
    private Logger logger;
    private Rectangle region;

    public Ball(int x, int y, int radius, Color color) {
        this.x = x;
        this.y = y;
        this.radius = radius;
        this.color = color;
        region = new Rectangle(x - radius, y - radius, 2 * radius, 2 * radius);
        logger = LogManager.getLogger(getClass().getSimpleName());
        Configurator.setLevel(getClass().getSimpleName(), Level.DEBUG);
    }

    public Ball(int x, int y, int radius) {
        this(x, y, radius, Ball.DEFAULT_COLOR);
    }

    public int getX() {
        return  x;
    }

    protected void setX(int x) {
        this.x = x;
        region.setLocation(x - getRadius(), getY() - getRadius());
    }

    public int getY() {
        return  y;
    }

    protected void setY(int y) {
        this.y = y;
        region.setLocation(getX() - getRadius(), y - getRadius());
    }

    public int getRadius() {
        return  radius;
    }

    public Color getColor() {
        return  color;
    }

    public Rectangle getRegion() {
        return region;
    }

    public Logger getLogger() {
        return  logger;
    }

    public boolean isCollision(int x, int y, int radius) {
        return  getRegion().intersects(x - radius, y - radius, 2 * radius, 2 * radius);
    }

    public boolean isCollision(Ball ball) {
        return  getRegion().intersects(ball.getRegion());
    }

    public void paint(Graphics g) {
        Color saveColor = g.getColor();

        g.setColor(color);
        g.fillOval(getX() - getRadius(), getY() - getRadius(),
                   2 * getRadius(), 2 * getRadius());
        g.setColor(saveColor);
    }
}
~~~

* ball의 위치를 위해 x,y 좌표를, 크기를 위해 radius를 선언하고 있다.

* ball의 차지하고 있는 영역을 사각으로 둘러싸는 최소 영역을 region으로 선언하고 있다.



**물음**

* x, y, radius와 region은 관계는 어떻게 될까?
* x, y, radius는 private 필드이다. 그렇다면, 다른 클래스에서 해당 필드를 직접 참조할 경우는 없다. accessor와 mutator만 이용한다면 어떻게 될까?




---

#### 문제 18. x,y 좌표, radius 대신 region을 이용하도록 수정하라.

~~~java
package org.nhnacademy.cannongame;

import java.awt.Color;
import java.awt.Graphics;
import java.awt.Rectangle;

public class Ball {
    private Rectangle region;
    private Color color;

  	...
}
~~~

* 코드를 많이 수정하였나?
* 앞에 만들어 둔 테스트 코드에는 문제가 없나? 모두 정상?

**참고**

*  필드를 직접 사용하는 것보다 accessor나 mutator를 사용하였다면, accessor와 mutator만 수정해서 적용할 수 있는 것이다.

---



### 5-2. Motion 클래스

#### 5-2-1. 정의

앞에서 2차원 공간에서의 물체 이동은 $(dx, dy)$, 즉, x축의 변화량과 y축의 변화량을 사용하였다. 하지만, 이것은 물체의 이동을 나타내는 데 한계가 있다.

예를 들어, ball을 30도 각도($\theta$)를 10만큼의 크기($v$)로 쏜다고 해보자. x축과 y축의 변화량은 얼마인가?
$$
dx = v sin \theta\\
dy = v cos \theta
$$
계산은 가능하다. 하지만, 이것을 매번 하거나 여러 변위량이 중첩하게 된다면 매우 복잡한 계산을 반복적으로 해야 하는 번거로움이 생길 것이다.

이러한 문제 해결을 위해 2차원 공간에서의 물체 이동과 관련된 클래스를 만들도록 한다.



Motion  클래스는 물리학이나 수학에서 말하는 Vector를 표현한 것이다.

<p align="center">
  <img src="./image/figure30.png" alt="Motion">
</p>

##### 필드

Motion은 좌표계를 기준으로 함으로 dx, dy를 기본으로 갖는다.

* x축 변화량(dx)
* y축 변화량(dy)



##### 함수

Motion은 수학과 물리학에서 이야기하는 벡터로서 각각의 성분을 개별적으로 반환하거나, motion 간 연산이 가능하다.

* x축과 y축 변화량으로 생성할 수 있다(PositionVector).
* 각도(angle)와 크기(magnitude)로 생성할 수 있다(DisplacementVector).
* motion을 더하거나 뺄 수 있다.(add, sub)




---

#### 문제 19. Motion 클래스를 구현하라.

* x축과 y축의 변화량을 줄 경우와 각도와 크기를 줄 경우를 구분하기 어렵다(정수와 실수로 구분할 수는 있지만….)
* Motion을  정의하고, 생성자는 클래스 함수로 정의한다.
  * x축과 y축의 변화량은 createPosition
  * 각도와 크기는 createDisplacement



**테스트 코드**

~~~java
package test;

import static org.junit.Assert.assertEquals;
import static org.junit.jupiter.api.Assertions.assertDoesNotThrow;

import java.util.Random;

import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;
import org.apache.logging.log4j.core.config.Configurator;
import org.apache.logging.log4j.Level;

import org.junit.jupiter.api.BeforeAll;
import org.junit.jupiter.api.RepeatedTest;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.TestInfo;
import org.nhnacademy.Motion;

public class Exam_5_2_1 {
    Logger logger = LogManager.getLogger(Exam_5_2_1.class.getName());
    Random random = new Random();

    @BeforeAll
    static void testInit() {
        Configurator.setLevel(Exam_5_2_1.class.getName(), Level.WARN);
    }

    @Test
    public void testMotion(TestInfo testInfo) {
        assertDoesNotThrow(()->Motion.createDisplacement(100, 90));
    }

    @Test
    public void testPositionToDisplacement(TestInfo testInfo) {
        for(int x = -10 ; x < 10 ; x++) {
            for(int y = -10 ; y < 10 ; y++) {
                Motion motion = Motion.createPosition(x, y);

                assertEquals((int)Math.sqrt((x*x+y*y)), (int)motion.getMagnitude()) ;
                assertEquals((int)Math.atan2(y, x), (int)motion.getAngle()) ;
            }
        }
    }

    @Test
    public void testDisplacementToPosition(TestInfo testInfo) {
        for(double angle = -360 ; angle < 360 ; angle++) {
            for(double magnitude = 0 ; magnitude < 10 ; magnitude++) {
                Motion motion = Motion.createDisplacement(magnitude, angle);

                int x = (int)(magnitude * Math.cos(Math.toRadians(angle)));
                int y = (int)(magnitude * Math.sin(Math.toRadians(angle)));
                assertEquals(x, motion.getDX()) ;
                assertEquals(y, motion.getDY()) ;
            }
        }
    }

    @RepeatedTest(10)
    public void testAdd(TestInfo testInfo) {
        int x = random.nextInt(100);
        int y = random.nextInt(100);

        Motion motion = Motion.createPosition(x, y);
        for(int i = 0 ; i < 10 ; i++) {
            int dx = random.nextInt(100);
            int dy = random.nextInt(100);

            motion.add(Motion.createPosition(dx, dy));
            x += dx;
            y += dy;
            assertEquals(x, motion.getDX());
            assertEquals(y, motion.getDY());
        }
    }

    @RepeatedTest(10)
    public void testSub(TestInfo testInfo) {
        int x = random.nextInt(100);
        int y = random.nextInt(100);

        Motion motion = Motion.createPosition(x, y);
        for(int i = 0 ; i < 10 ; i++) {
            int dx = random.nextInt(100);
            int dy = random.nextInt(100);

            motion.sub(Motion.createPosition(dx, dy));
            x -= dx;
            y -= dy;
            assertEquals(x, motion.getDX());
            assertEquals(y, motion.getDY());
        }
    }
}
~~~



---



### 5-3. 공간에서의 이동

공간에서의 이동은 물체가 방향과 크기(motion)에 따라 위치가 변하는 것을 말하며, 이를 Motion 클래스로 정의하였다.



#### 5-3-1. Ball의 이동

앞의 MovableBall은 단위 이동량을 dx, dy로 설정하였다.

이를 Motion 클래스로 변경하여 적용해 보자..



---

#### 문제 20. 단위 이동량을 Motion 클래스로 수정하라.

* 기존에 사용하던 move(int dx, int dy)는 호환성을 위해 두도록 한다.
* Ball 테스트 코드에 Motion 추가와 관련된 테스트 코드를 추가한다.
* 기존에 작성했던, Exam_4_3_3.java를 실행해 동일하고 동작하는지 확인한다.

**변경 전**

~~~java
public class MovableBall extends Ball {
    private int dx = 0;
    private int dy = 0;
    private long movementCount = 0;
		...
}
~~~



**변경 후**

~~~java
public class MovableBall extends Ball {
    private motion = new Motion();
    private long movementCount = 0;
		...
}
~~~



---



## 6. 새로운 물체의 출현

현재의 world에는 ball만 존재한다. ball 이외의 다른 물체가 존재한다면 어떻게 될 것인가?

사각형의 box(box)를 world에 추가해 보도록 하자.



### 6-1. Box 클래스

#### 6-1-1. 정의

2차원 공간에서의 Box는 우리가 흔히 알고 있는 사각형이다.

<p align="center">
  <img src="./image/figure31.png" alt="box">
</p>

* 생성 후 이동이나 정보 변경 불가

##### 필드

box는 중심 좌표와 폭과 높이를 갖는다.

* 중심 좌표

* 폭
* 높이



##### 함수

box에는 값을 얻기 위한 함수와  화면상에 표시하기 위한 함수가 제공되어야 하며, 위에서 정의한 필드의 값을 얻을 수 있도록 다음의 함수가 요구된다.

* 중심점 좌표 얻기(getX, getY)

* 폭 얻기(getWidth)

* 높이 얻기(getHeight)

* 영역 반환

* 색 얻기 (getColor)

* 그리기(paint)



기본적인 Box 클래스 구성이 완료되었다면, 화면상에 표시하기 위해서도 다음의 함수가 요구된다.

화면에 그리기는 awt library를 사용하므로, 라이브러리에서 요구하는 형식에 맞춰 구성되어야 한다.

AWT에서는 Graphics context 제공하여 화면 출력이 가능하도록 지원하므로, 그리기 함수에서는 제공되는 context를 이용해 그려야 한다.

~~~java
void paint(Graphics g) {...}
~~~



**참고. AWT에서 사각형 그리기**

Box 클래스에서 도형을 그리는 paint 함수를 보면, 사각형을 그리기 위해  fillRect를 사용한다.

<p align="center">
  <img src="./image/figure32.png" alt="fillRect">
</p>

* (x, y)는 box의 중심 좌표를 나타낸다.
* fillRect는 우측 상단 꼭짓점과 폭, 높이 정보가 필요하다.



---

#### 문제 21. Box 클래스를 구현하라.

* x, y로 이루어진 중심점 좌표, 폭, 높이, 색을 갖는다.
* 중심점 좌표, 폭, 높이는 생성 시 설정한다.
* 복제 생성자를 구성한다.
* 색은 생성 시 설정할 수 있고, 기본색은 파란색으로 지정한다.
* 각 필드 값을 요청할 수 있다.
* 코드 중복은 최소화하라.
* 코딩 규칙을 따라 작성한다.
* awt Graphics context를 매개변수로 받아 그릴 수 있도록 함수를 추가한다.
* 도형의 색은 graphics context에서 설정할 수 있다. (setColor)
* 일반적으로 외부의 자원을 활용할 경우, 자원 활용 후 활용 전 설정을 최대한 복원해 두는 것이 좋다. 따라서, graphics context의 색 설정을 변경하기 전에 기존 색을 저장하였다 복원에 사용한다.



**테스트 코드**

~~~java
package test;

import static org.junit.Assert.assertEquals;
import static org.junit.jupiter.api.Assertions.assertAll;

import java.awt.Color;
import java.util.Random;

import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;
import org.apache.logging.log4j.core.config.Configurator;
import org.apache.logging.log4j.Level;

import org.junit.jupiter.api.BeforeAll;
import org.junit.jupiter.api.RepeatedTest;
import org.junit.jupiter.api.RepetitionInfo;
import org.junit.jupiter.api.TestInfo;
import org.nhnacademy.Box;

public class Exam_6_1_1 {
    static final int MIN_X = 0;
    static final int MAX_X = 1000;
    static final int MIN_Y = 0;
    static final int MAX_Y = 1000;
    static final int MIN_WIDTH = 10;
    static final int MAX_WIDTH = 100;
    static final int MIN_HEIGHT = 10;
    static final int MAX_HEIGHT = 100;
    static final int TEST_COUNT = 10;
    static final Color[] colors = { Color.BLUE, Color.RED, Color.WHITE,
        Color.BLACK, Color.GREEN };

    Logger logger = LogManager.getLogger(Exam_6_1_1.class.getName());
    Random random = new Random();
    DummyGraphics graphics = new DummyGraphics();

    @BeforeAll
    static void testInit() {
        Configurator.setLevel(Exam_6_1_1.class.getName(), Level.WARN);
    }

    @RepeatedTest(10)
    public void testColorBox(TestInfo testInfo) {
        int x = MIN_X + random.nextInt(MAX_X - MIN_X);
        int y = MIN_Y + random.nextInt(MAX_Y - MIN_Y);
        int width = MIN_WIDTH + random.nextInt(MAX_WIDTH - MIN_WIDTH);
        int height = MIN_HEIGHT + random.nextInt(MAX_HEIGHT - MIN_HEIGHT);
        Color color = colors[random.nextInt(colors.length)];

        Box box = new Box(x, y, width, height, color);
        String message = String.format("%3d, %3d, %3d, %3d, %s -> %3d, %3d, %3d, %3d, %s",
                x, y, width, height, color,
                box.getX(), box.getY(), box.getWidth(), box.getHeight(), box.getColor());
        logger.info(message);
        assertAll(testInfo.getDisplayName() + "-creation",
                () -> assertEquals(x, box.getX()),
                () -> assertEquals(y, box.getY()),
                () -> assertEquals(width, box.getWidth()),
                () -> assertEquals(height, box.getHeight()),
                () -> assertEquals(color, box.getColor()));
    }


    @RepeatedTest(10)
    public void testColorBox(RepetitionInfo repetitionInfo, TestInfo testInfo) {
        int x = MIN_X + random.nextInt(MAX_X - MIN_X);
        int y = MIN_Y + random.nextInt(MAX_Y - MIN_Y);
        int width = MIN_WIDTH + random.nextInt(MAX_WIDTH - MIN_WIDTH);
        int height = MIN_HEIGHT + random.nextInt(MAX_HEIGHT - MIN_HEIGHT);
        Color color = colors[random.nextInt(colors.length)];

        Box box = new Box(x, y, width, height, color);

        graphics.clear();
        graphics.setColor(Color.BLACK);

        box.paint(graphics);
        assertAll(testInfo.getDisplayName()+"-paint",
            ()->assertEquals(x - width / 2, (int)graphics.getFillRectParams().get("x")),
            ()->assertEquals(y - height / 2, (int)graphics.getFillRectParams().get("y")),
            ()->assertEquals(width, (int)graphics.getFillRectParams().get("width")),
            ()->assertEquals(height, (int)graphics.getFillRectParams().get("height")),
            ()->assertEquals(color, graphics.getFillRectParams().get("color")),
            ()->assertEquals(Color.BLACK, graphics.getColor()));

    }
}

~~~

* Exam_2_1_1.java와 Exam_2_1_2.java 참조
* DummyGraphics에 Rect 관련 함수를 추가한다.(테스트 코드 참조)

---



---

#### 문제 22. World에서 Box 클래스를 지원할 수 있도록 추가하라.

World에는 Ball만 추가되도록 구성되어 있다. World에 Box를 추가할 수 있도록 수정해 보자.

두 가지 방법이 있을 것 같다.

첫 번째, Box 클래스를 위한 함수들을 추가해 보자.



**실행 결과**

<p align="center">
  <img src="./image/figure33.png" alt="exam_6_1_2_1">
</p>

* Box 추가에 문제는 없나?
* Data type만 다를 뿐 동일한 작업은 문제없나?
* 새로운 종류가 추가된다면?



---



box를 관리하기 위한 필드를 추가할 뿐만 아니라 관련된 함수들을 모두 추가해야 한다. 생각보다 번거로운 일이 아닐 수 없다.

뿐만 아니라 별 등의 다른 물체가 추가된다고 하면 이와 같은 과정을 반복해서 작업해야 한다.

이는 World 클래스가 확장성을 전혀 가지고 있지 못하다는 것을 보여 준다.



두 번째 방법으로 Ball 클래스와  Box 클래스의 상위 클래스인 Object 클래스를 이용하는 방법이 있을 수 있다.



---

#### 문제 23. World 클래스의 오브젝트들을 Obect 클래스로 단일화시켜 관리토록 바꿔 보자.



**실행 결과**

<p align="center">
  <img src="./image/figure34.png" alt="exam_6_1_3_1">
</p>

* 새로운 종류 추가에 문제가 없는가?
* paint에서 Object 클래스에 대해 처리가 가능한가?

---



두 가지 방식 모두 좋아 보이지 않는다. 그렇다고 하더라고 나머지 추가 작업이 없다면 사용할 수 있을 것이다.

하지만, World 클래스를 확장해서 정의한 MovableWorld, BoundedWorld는 어떻게 해야 하나?

World 클래스에서 했던 작업을 동일하게 반복해야 한다.

문제가 간단하지만은 않은 듯하다.



## 7. 복잡한 세상을 조금 더 단순하게

**Keyword**

* Interface
* Subclassing
* Subtyping



문제를 단순화하기 위해 Box 클래스가 추가된 World 클래스를 다시 보자.

World 클래스 내에서 ball과 box를 구분해야 할 곳은 어디인가?

이것만 일반화할 수 있다면 문제가 쉽게 해결되지 않을까?





### 7-0. Regionable 인터페이스

World 클래스는 출력되는 오브젝트들은 모두 일정한 영역을 갖는다. 앞서 정의한 ball이나 box에서도 getRegion 함수를 이용해 영역 확인이 가능하다.

이러한 공통적인 기능들이 제공되는 type을 정의한다.



#### 7-0-1. 정의

* 영역을 가지는 type



##### 요구 함수

* 영역 가져오기(getRegion)





---

#### 문제 24. Regionable 인터페이스를 선언하고, World 클래스에는 Regionable 오브젝트를 받아서 관리할 수 있도록 수정하라.

* 어떠한 문제점이 있는가?
* 해결 방법은? Regionable에 그리기 함수 추가?

---



### 7-1. Paintable 인터페이스

World 클래스는 도형을 받아서 화면에 출력하는 작업을 한다. 따라서, 실제로 필요한 것은 paint 함수를 가진 오브젝트면 어떠한 종류든 상관이 없다.

#### 7-1-1. 정의

* 그리기 지원



##### 요구 함수

* 그리기(paint)



---

#### 문제 25. Paintable 인터페이스를 선언하고, World 클래스에는 Paintable 오브젝트를 받아서 관리할 수 있도록 수정하라.

* 문제 7-0-1에서 발생한 문제는 해결되었나?

---





---

#### 문제 26. Ball 클래스와 Box 클래스를 World 클래스에 적용할 수 있도록 수정하라.

* 클래스가 수정되었다면 앞서 만들어 둔 SingleBallWorldTest를 이용해 동작을 확인해 보자.
* MultiBallWorldTest에서는 에러가 발생할 수 있다. 이는 확인을 위해 getBall 함수를 이용해 Ball을 가져오기 때문이다. 수정해 보도록 한다.



**테스트 코드**

~~~java
package exam;

import java.awt.Color;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;
import java.util.Random;

import javax.swing.JFrame;

import org.apache.logging.log4j.Level;
import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;
import org.apache.logging.log4j.core.config.Configurator;
import org.nhnacademy.Ball;
import org.nhnacademy.Box;
import org.nhnacademy.World;

public class Exam_7_1_2 {

    static final int WIDTH = 400;
    static final int HEIGHT = 300;
    static final int MIN_RADIUS = 10;
    static final int MAX_RADIUS = 50;
    static final int MIN_WIDTH = 10;
    static final int MAX_WIDTH = 50;
    static final int MIN_HEIGHT = 10;
    static final int MAX_HEIGHT = 50;
    static final int BALL_COUNT = 10;
    static final int BOX_COUNT = 10;
    static final Color[] colors = { Color.BLUE, Color.RED, Color.WHITE,
        Color.BLACK, Color.GREEN };

    public static void main(String [] args) {
        Logger logger = LogManager.getLogger(Exam_7_1_2.class.getName());
        Configurator.setLevel(Exam_7_1_2.class.getName(), Level.ALL);

        JFrame frame = new JFrame();
        frame.setSize(WIDTH, HEIGHT);
        frame.addWindowListener(new WindowAdapter() {
            @Override
            public void windowClosing(WindowEvent e) {
                System.exit(0);
            }
        });

        Random random = new Random();
        World world = new World();
        logger.info("world 생성 완료");

        while(world.getCount() < BALL_COUNT) {
            int radius = MIN_RADIUS + random.nextInt(MAX_RADIUS - MIN_RADIUS);
            int x = radius + random.nextInt(WIDTH - radius);
            int y = radius + random.nextInt(HEIGHT - radius);
            Color color = colors[random.nextInt(colors.length)];

            world.add(new Ball(x, y, radius, color));
            String message = String.format("ball 추가 : %3d, %3d, %3d, %x",
                    x, y, radius, color.getRGB());
            logger.info(message);
        }

        while(world.getCount() < BALL_COUNT + BOX_COUNT) {
            int width = MIN_WIDTH + random.nextInt(MAX_HEIGHT - MIN_HEIGHT);
            int height = MIN_HEIGHT + random.nextInt(MAX_HEIGHT - MIN_HEIGHT);
            int x = width / 2 + random.nextInt(WIDTH - width);
            int y = height / 2 + random.nextInt(HEIGHT - height);
            Color color = colors[random.nextInt(colors.length)];

            world.add(new Box(x, y, width, height, color));
            String message = String.format("ball 추가 : %3d, %3d, %3d, %3d, %x",
                    x, y, width, height, color.getRGB());
            logger.info(message);
        }

        frame.add(world);
        logger.info("Frame에 추가 완료");

        frame.setEnabled(true);
        frame.setVisible(true);
        logger.info("화면 출력");
    }
}
~~~



**실행 결과**

<p align="center">
  <img src="./image/figure35.png" alt="exam_7_1_2_1">
</p>

---



### 7-2. Movable 인터페이스

#### 7-2-1. 정의

* MovableBall, MovableBox 그리고 MovableWorld? Movable!

* MovableWorld에서 오브젝트를 이동하기 위해 필요한 것은 해당 오브젝트에서 이동에 필요한 함수 지원 여부



##### 요구 함수


  * 단위 시간당 이동량 (setMotion, getMotion)
  * 단위 시간당 이동량만큼 이동하기(move)
  * 특정 위치로 이동하기(moveTo)



---

#### 문제 27. MovableBall과 MovableWorld도 Movable 인터페이스를 선언해 해결하는 것과 같은 방법으로 해결하라.

* 기존 코드에서 많은 부분을 바꿔야 하나?

---



### 7-3. Bounded 인터페이스

#### 7-3-1
정의

* BoundedBall, BoundedBox 그리고 BoundedWorld?? Bounded!



##### 요구 함수

* 경계 정보를 설정하거나 반환한다.(getBounds, setBounds)

* 이동 후 경계를 벗어났는지 확인한다.(isOutOfBounds)
* 경계를 벗어 경우 벽에서 튕긴다.(bounce)



---

#### 문제 28. BoundedBall과 BoundedWorld도 Bounded 인터페이스를 선언해 해결하는 것과 같은 방법으로 해결하라.

---



## 8. 거꾸로 세상

**Keyword**

* Model, View 	// 간단히만 소개 정도만



### 8-1. 화면상의 좌표

컴퓨터 화면의 좌표와 사람들이 일반적으로 생각하는 좌표계와는 다르다.



<p align="center">
  <img src="./image/figure35.png" alt="좌표계">
</p>


#### 8-1-1. 좌표 변환



* 회전(rotate)
* 이동(translate)
* 크기 조정(scale)



---

#### 문제 29. World 클래스에 회전 함수를  추가해 보자.

* 회전 방향은 오브젝트 단위로 설정하거나, 함수 호출 시 지정할 수 있다. (setRotation, rotate)
* X축 또는 Y축을 기준으로 회전한다.(rotate)
  * 회전할 축은 enum으로 선언한다.

---



---

#### 문제 30. World 클래스에 이동 함수를 추가해 보자.

* 이동량을 미리 설정하거나, 함수 호출 시 지정할 수 있다(setTranslation, translate)
* X축, Y축 또는 양 축을 기준으로 이동한다.(translate)

---



---

#### 문제 31. World 클래스에 확대/축소 함수를 추가해 보자.

* 크기 조정 비율을 미리 설정하거나, 함수 호출 시 지정할 수 있다.(setScale, scale)
* 공간 크기를 확대 또는 축소한다(scale)

---



#### 도형 그리기

* 단순히 하나의 점에 대한 좌표 변환은 X축을 기준으로 회전시킨 후 Y의 시작 위치를 조정하면 된다.
* 도형의 경우, 기준점 변경은 문제가 되지 않지만, 그려지는 도형이 위에서 아래로 그려지는지, 아래에서 위로 그려지는지에 따라 도형의 위치가 달라질 수 있다.
* 화면에 출력하는 라이브러리를 사용할 기준점을 변경하더라도 도형을 그리는 방향이 위에서 아래 방향으로 생각하는 것과 반대가 된다.
* 도형은 회전뿐만 아니라 위치 이동도 필요하다.



---

#### 문제 32. 도형 그릴 때 좌표의 변환이 필요하다. Ball, box 등에서 도형을 그릴 때 좌표가 변환된 도형을 그리도록 수정해 보자.

* 도형은 화면상에서 좌측 위를 기준으로 한다. 좌표를 변환하게 되면, 우측 아래로 변경되어 기준점을 변경해 주어야 한다.

---



## 9. 외부 영향

**Keyword**

* Acceleration



물체의 움직임을 변화시키는 것은 외부 영향이다. 앞서 정의한 벽에 부딪힐 경우 물체의 진행 방향이 변경되는 것 또한 외부 영향이며,  게임 환경에서는 물체의 움직임에 영향을 줄 수 있는 다양한 요소들의 추가가 가능하다.



우리가 목표로 하는 대포게임은 대포를 쏘아 상대방 목표물을 맞히는 것이다. 실제 환경에 대포에 의해 발사된 포탄은 바람, 중력, 공기 저항 등 다양한 요소들의 영향을 받는다.

이에 따라 우리가 만드는 게임에서는 중력과 바람의 영향을 넣어 보도록 한다.

이들은 특정 값으로 설정되기보다는 포탄이 날아가는 단위 시간 동안 일정량의 영향을 주는 형태로 표현될 것이다.

일반적으로 이는 오브젝트의 이동에 영향을 주는 외부 효과라 할 수 있다.



### 9-1. 외부 효과

외부 효과는 world 내에서 물체에 영향을 주지만, 변화량과 같이 고정되지 않고 추가되는 성질을 가지고 있다.

기존의 변화량은 단위 시간당 변화량과 같이 지속해 같은 영향을 주기보다는 물체가 움직일 때마다 추가적인 영향을 줌으로써 물체가 가지고 있는 단위 시간당 변화량의 변화를 준다.

외부 효과도 물체의 이동과 관계되므로  Motion을 이용해 표현할 수 있다.



**참고**

* 속도
  $$
  v = {dx \over dt}
  $$

  * 물체의 속도와 운동 방향으로 물체의 단위 시간당 위치 변화를 나타낸다.
  * 시간이 지남에 따라 물체의 위치가 변경된다.

* 가속도

  $$
  a={dv \over dt}
  $$

  * 속도의 변화로써 물체의 단위 시간당 속도 변화를 나타낸다.
  * 시간이 지남에 따라 물체의 속도가 변경된다.



### 9-2. MovableWorld 클래스

ball이나 box가 움직일 때, 외부 효과를 추가하기 위해서는 Movable 인터페이스에 외부 효과를 주면서 움직일 수 있는 함수를 추가하고, 각각의 클래스에서 함수를 구현한다.

MovableWorld에는 외부 효과를 추가하여 관리할 수 있도록 필드와 함수를 추가한다.


#### 9-2-1. 수정
기능 추가를 위해 다음 함수를 추가한다.

##### 함수

* 외부 효과를 추가한다.(addEffect)
  * 외부 효과는 0개 이상 적용될 수 있다.
* 추가된 외부 효과의 수를 반환한다(getEffectCount)
* 외부 효과를 가져온다(getEffect)
* 외부 효과를 제거한다(removeEffect)



---

#### 문제 33. MovableWorld에서 하나의 ball을 생성하여, ball이 움직일 때마다 외부 효과를 추가해 보자.

MovableWorld에 추가된 함수들을 구현한다.



**테스트 코드**

~~~java
package exam;

import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;

import javax.swing.JFrame;

import org.apache.logging.log4j.Level;
import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;
import org.apache.logging.log4j.core.config.Configurator;
import org.nhnacademy.Motion;
import org.nhnacademy.MovableBall;
import org.nhnacademy.MovableWorld;

public class Exam_9_1_1 {
    static final int WIDTH=400;
    static final int HEIGHT=300;
    static final int MAX_MOVEMENT_COUNT=100;
    static final int DT= 100;
    public static void main(String[] args) {
        Logger logger = LogManager.getLogger(Exam_9_1_1.class.getName()) ;
        Configurator.setLevel(Exam_9_1_1.class.getName(), Level.ALL);

        JFrame frame = new JFrame();
        frame.setSize(WIDTH, HEIGHT);
        frame.addWindowListener(new WindowAdapter() {
            @Override
            public void windowClosing(WindowEvent e) {
                System.exit(0);
            }
        });

        MovableWorld world = new MovableWorld();
        logger.debug("world created");
        world.setDT(DT);
        world.addEffect(Motion.createPosition(-5, 0));
        frame.add(world);

        MovableBall ball = new MovableBall(50, 100, 50);
        ball.setDX(50);
        ball.setDY(0);

        world.add(ball);
        logger.debug("add ball : {}, {}, {}, {}, {}",
            ball.getX(), ball.getY(), ball.getRadius(), ball.getDX(), ball.getDY());

        world.setMaxMovementCount(MAX_MOVEMENT_COUNT);


        frame.setEnabled(true);
        frame.setVisible(true);
        world.run();
    }
}
~~~



---



---

#### 문제 34. MovableWorld에 외부 영향으로 중력을 추가해 보자

* 중력 가속도($g$)는 $a=-9.8 {m \over s^2}$  로 변화량 계산에 추가하기 위해서는 복잡하다.

* 여기서, 이를 단순화시켜서 단위 시간당 일정한 변화량을 추가하는 것으로 정리한다. 다만, 방향에는 주의하자.

  * $g$ 는 Y축의 음수 방향으로 가해지는 변화량이다.

<p align="center">
  <img src="./image/figure36.png" alt="중력가속도">
</p>


---



---

#### 문제 35. MovableWorld에 외부 영향으로 바람을 추가해 보자

* 중력은 일정한 방향으로 일정하게 적용된다.

* 바람은 시간에 따라 변할 수 있다.


<p align="center">
  <img src="./image/figure37.png" alt="중력가속도">
</p>

---



Movable 오브젝트들은 게임상에서 움직이는 물체들을 나타내고 있다. MovableWorld에서 등록된 오브젝트 중 Movable type의 오브젝트들만 찾아서 반복문을 통해 순차적으로 이동시켰지만, Thread를 적용하면 Movable type 스스로 움직이도록 할 수 있다.



먼저, Thread에 대해 알아보고 계속 진행하자



## 10. 스스로 움직이는 물체

**Keyword**

* Thread
* Synchronization



Thread를 학습하기 전까지는 프로그램의 대부분이 하나의 thread에서 동작하였다. (정확히 말해서는 그렇지 않지만, 최소한 여러분이 작성한 코드에 대해서는 그러하다)

이제는 thread에 대해서 학습하고 연습하였으므로 게임에 적용해 보도록 하자.





###  10-1. Movable 인터페이스

Thread에 대해 이해하였다면, Movable을 Thread 적용이 가능한 Runnable 인터페이스의 확장으로 정의한다.

Movable은 스스로 움직일 수 있는 type으로서, 이에 필요한 함수 추가가 요구된다.



#### 10-1-1. 정의



##### 함수

* 단위 시간을 관리한다 (getDT, setDT)

* 움직임 시작한다(start)

* 움직임 멈춘다(stop)



---

#### 문제 36. Movable 인터페이스를 Runnable 인터페이스의 확장으로 정의하고, MovableBall, MovableBox 등을 thread로 동작하도록 수정하라.

* 구현 클래스는 별도의 Thread 인스턴스의 도움을 받지 않도록 구현한다.



**주의**

* paint 함수가 어느 thread에서 실행되는지 확인하라.
* 동일한 자원에 대해 두 개 이상의 thread가 동시 접근 시 문제가 될 수 있다.
  * Thread 학습에서 배운 내용으로 대처하라

---



### 10-2. MovableWorld 클래스

MovableWorld에서는 일정한 시간 간격으로 오브젝트를 이동시키는 작업을 수행하였다.

오브젝트를 Runnable type으로 전환하여 개별적으로 동작하게 할 경우, 오브젝트 이동을 위한 작업은 필요 없게 되고 전체적인 동작의 시작과 멈춤만 수행하면 된다.



또한, MovableWorld에서만의 관리 작업이 필요하다면, MovableWorld도 thread를 이용해 독립적으로 동작하는 오브젝트로 변경하면 된다.

이를 위해 함수를 수정한다.



#### 10-2-1. 수정

* Thread에 의해 개별 동작할 것은 Runnable 인터페이스를 구현한다.



##### 필드

* Thread 관리를 위한 thread 추가한다.



##### 함수

* Thread를 이용해 구동할 것으로 run을 제거하고,
* Thread 구동을 위한 main 함수 추가한다.
* Thread 시작과 멈춤을 추가한다(start/stop)



---

#### 문제 37.  MovableWorld를 thread로 동작할 수 있도록 수정하라.

* 이전과 같이 동작하나?
* 오브젝트는 동작 중에 추가되거나 삭제될 수 있다.
  * 자원 동시 접근 문제가 발생할 수 있다.




**테스트 코드**

~~~java
~~~



---



### 10-2. 충돌 감지 및 튕기기

앞서서는 BoundedWorld에서 오브젝트를 이동시키고, 충돌 감지 및 그에 따른 튕기기를 구현하였다. Movable 개체에 대해 thread를 적용하여 스스로 움직임을 구현한 후  더 이상 BoundedWorld에서 개별 오브젝트에 대한 충돌 검출 및 튕기기 구현이 어려워졌다. 어떻게 해야 할까?



BoundedWorld의 기본 역할은 유지하지만, 오브젝트를 이동시키는 주체가 변경되었으므로 이를 조정해 보자.

BoundedWorld에서 이동과 충동 검출을 함께 할 경우, 이동 후 시점에 대해서 스스로 알 수 있지만, 개별 오브젝트가 스스로 움직일 경우 해당 시점을 알 수 없다.

어떻게 해결해야 할까?



고려 사항은 다음과 같다.

* 움직이는 주체는 오브젝트이다.
* 경계 영역에 대한 정보는 해당 오브젝트가 가질 수 있다.
* 오브젝트 간 충돌 확인을 위해서는 World의 오브젝트 목록이 필요하다.
  * 오브젝트 등록 시 목록을 받고, 신규 등록 시 업데이트되거나
  * 매번 World에 요청하거나




---

#### 문제 38. 오브젝트가 이동 후 World로부터 정보를 받아 충돌 감지 및 튕김 구현이 가능하도록 수정하라.

* World에 오브젝트를 추가할 때, 오브젝트에 world를 참조할 수 있도록 정보를 제공한다.

* 오브젝트는 이동 후 충돌 확인 world로부터 장애물(경계영역, 다른 오브젝트)에 대한 정보를 받아 충돌 확인한다.

* 충돌 검출 시 튕기면 처리한다.



---





## 11. Java Graphics

**Keyword**

* GUI
* AWT, Swing
* Delegation

* Event-Driven Programming





### 11-1. Component 클래스

Java Swing에서는 아래와 같은 component를 지원한다.

~~~mermaid
classDiagram
	Object <|-- Component
  Component <|-- Container
  Container <|-- Window
  Window <|-- Frame
  Window <|-- Dialog
	Frame <|-- JFrame
	Dialog <|-- JDialog
	Container <|-- JComponent
	JComponent <|-- Button
	JComponent <|-- JLabel
	JComponent <|-- JCheckbox
	JComponent <|-- JList
	JComponent <|-- JProgressBar
	JComponent <|-- AbstractButton
	AbstractButton <|-- JButton
	AbstractButton <|-- JToggleButton
	JToggleButton <|-- JCheckBox
~~~



게임에서 필요한 몇 가지 component에 대해 알아보자.

#### 11-1-1. JLabel 클래스

* 화면상에 문자열 출력
* 색, 크기, 폰트 등 지정 가능




#### 11-1-2. JTextField 클래스

* 화면상에 문자열 입력을 위한 입력창 생성
* 입력 글자 수, 형식  등 지정 가능



#### 11-1-3. JBotton 클래스

* 흔히 알고 있는 push 버튼
* 상태



#### 11-1-4. JToggleButton 클래스

* 두 가지 상태 중 하나의 상태를 가지고 있는 버튼




#### 11-1-5. JSlider 클래스

* 제한된 간격 내에서 손잡이를 밀어 사용자가 값 선택
* 옵션으로 눈금 표시





### 11-2. Container 클래스

컨테이너 클래스는 다양한 컴포넌트를 담아서 하나의 컴포넌트처럼 관리할 수 있도록 지원하는 클래스이다.

* TextField, Label 등과 같은 다른 구성 요소를 포함할 수 있는 awt 구성 요소이다
* Frame, Dialog, Panel 클래스가 여기에 속한다.
* 구성 요소를 특정 위치에 배치되는 화면으로써, 구성 요소의 레이아웃을 포함하고 제어한다.



컨테이너의 4가지 유형은 다음과 같다.

* Window
  * 테두리와 메뉴 표시줄이 없다
* Panel
  * 제목 표시줄, 테두리 또는 메뉴 표시줄을 포함하지 않는다.
  * Button, TextField 등의 다른 구성 요소들을 배치하기 위한 용도로 사용된다.

* Frame
  * 제목 표시줄, 테두리 또는 메뉴 표시줄을 포함하지 않는다.
  * Button, TextField 등의 다른 구성 요소들을 배치하기 위한 용도로 사용된다.

* Dialog



#### 11-2-1. Layout

AWT에서는 컨테이너 안에서 구성 요소들의 위치를 자동으로 지정해 주는 다양한 layout을 지원한다.

Layout의 종류는 다음과 같다.

* BorderLayout
* FlowLayout
* GridLayout
* GridBagLayout
* BoxLayout
* CardLayout
* GroupLayout
* SpringLayout



몇 가지 layout에 대해 알아보자.



##### 11-2-1-1. BorderLayout

* Frame 기본 layout
* 구성 요소를 container 상/하/좌/우/중앙에 위치



#### 문제 39. Frame의 layout을 BorderLayout으로 설정하고 버튼을 추가해 보자.

다음 코드는 버튼을 Frame의 동쪽에 붙이는 코드이다.

~~~java
package exam;

import java.awt.BorderLayout;

import javax.swing.JButton;
import javax.swing.JFrame;

public class Exam_11_3_1_1 {
	/**
	 * @param args
	 * @throws InterruptedException
	 */
	public static void main(String[] args) {
		JFrame frame = new JFrame();

		// 제목 설정
		frame.setTitle("BorderLayout Frame");
		// 크기 설정
		frame.setSize(300, 300);
		// Layout 종류 지정
		...

    // 버튼 생성 후 추가
    ...

		frame.setVisible(true);
	}
}

~~~

결과는 다음과 같다.

<p>
	<img src="./image/figure38.png" alt="image-BorderLayout" />
</p>



##### 11-2-1-2. GridBagLayout

* Container 내부를 행과 열로 구분
* 구성 요소를 행과 열을 지정하여 배치
* 구성 요소  크기 지정 가능



#### 예제. GridBagLayout을 이용해 다음과 같이 구성하라.

<p>
  <img src="./image/figure39.png" alt="exam_11_3_1_2_1_1" />
</p>



각 버튼의 속성을 보면 아래와 같다.

|          | 가로 위치 | 세로 위치 | 폭 가중치 | 가로 격자수 | 높이 |
| :------: | :-------: | :-------: | :-------: | :----------: | :--: |
| Button 1 |     0     |     0     |           |              |      |
| Button 2 |     1     |     0     |    0.5    |              |      |
| Button 3 |     2     |     0     |    0.5    |              |      |
| Button 4 |     3     |     0     |    0.5    |              |      |
| Button 5 |     1     |     1     |    0.5    |      2       |  40  |



**예제 코드**

~~~java
import java.awt.*;
import javax.swing.*;

public class Exam_11_3_1_2_1 {
	/**
	 * @param args
	 * @throws InterruptedException
	 */
	public static void main(String[] args) throws InterruptedException {
		JFrame frame = new JFrame();
		GridBagConstraints constraints = new GridBagConstraints();
		constraints.fill = GridBagConstraints.HORIZONTAL;
		// 제목 설정
		frame.setTitle("GridBagLayout");
		//// 크기 설정
		frame.setSize(400, 130);
		//
		frame.setLayout(new GridBagLayout());

		JButton button = new JButton("Button 1");
		constraints.gridx = 0;
		constraints.gridy = 0;
		frame.add(button, constraints);

		button = new JButton("Button 2");
		constraints.weightx = 0.5;
		constraints.gridx = 1;
		constraints.gridy = 0;
		frame.add(button, constraints);

		button = new JButton("Button 3");
		constraints.weightx = 0.5;
		constraints.gridx = 2;
		constraints.gridy = 0;
		frame.add(button, constraints);

		button = new JButton("Button 4");
		constraints.weightx = 0.5;
		constraints.gridx = 3;
		constraints.gridy = 0;
		frame.add(button, constraints);

		button = new JButton("Button 5");
		constraints.weightx = 0.5;
		constraints.gridx = 1;
		constraints.gridwidth = 2;
		constraints.gridy = 1;
		constraints.ipady = 40;
		frame.add(button, constraints);

		frame.setVisible(true);
	}
}
~~~

#### 문제 40. 계산기 만들기

* 아래 그림과 같은 계산기를 구성한다.
* 버튼
  * 0~9 숫자(0, 1, 2, 3, 4, 5, 6, 7, 8, 9)
  * 4칙 연산자(+, -, *, /)
  * 초기화(AC)
  * 계산(=)

<p>
  <img src="./image/figure40.png" alt="계산기" />
</p>


##### 11-2-1-3. AbsoluteLayout

* 절대 좌표를 이용한 구성요소 배치
  * SetBounds
  * 자동 배치가 되지 않아 원하는 구성이 나오지 않을 수 있음
* LayoutManager 없음
  * setLayout(null)





#### 문제. 계산기를 AbsoluteLayout을 이용해 구성해 보자.





## 12. Event-Driven Programming

**Keyword**

* [Event Listener](https://docs.oracle.com/javase/tutorial/uiswing/events/intro.html)



### 12-1. Java Event

Java는 delegation event model(델리게이션 이벤트 모델)을 사용하여 이벤트를 처리한다. 이 모델은 이벤트를 생성하고 처리하는 표준 메커니즘으로 정의되고 다음과 같은 요소들로 구성된다.

![event_model](./image/event_model.png)

- Event Object(이벤트 오브젝트)
  - 이벤트가 발생 시 해당 이벤트를 식별할 수 있는 각종 묶음 정보
  - 이벤트 발생으로 생성되고, 이벤트 처리에 이용된다.
  - 이벤트 오브젝트에 대한 클래스를 제공하거나 용도에 맞게 정의되어야 한다.
- Event Source(이벤트 소스)
  - 소스는 이벤트가 발생하는 오브젝트
  - 발생한 이벤트에 대한 정보를 핸들러에 제공할 책임이 있다
  - Java는 소스 오브젝트에 대한 클래스를 제공한다.
- Event Listener(이벤트 리스너)
  - 이벤트에 대한 응답을 생성
  - 인터페이스 제공
  - 구현을 통한 실체화로 이벤트 핸들러 생성
  - 이벤트를 수신할 때까지 기다리다 이벤트가 수신되면, 인터페이스를 이용해 구현된 함수를 통해 이벤트 처리
  - 인터페이스를 이용해 구현된 오브젝트를 이벤트 핸들러(**Event Handler**)라고도 한다.



이러한 접근 방식의 이점은 사용자 인터페이스와 이벤트 생성을 완전히 분리된다는 것이고, 사용자 인터페이스는 이벤트 처리를 별도의 코드로 위임할 수 있다.

이 모델에서는 listener가 이벤트 알림을 수신할 수 있도록 listener를 소스 오브젝트에 등록해야 한다. 이벤트 알림을 수신하려는 listener에만 이벤트 알림이 전송되기 때문에 이는 이벤트를 처리하는 효율적인 방법이다.



**이벤트 오브젝트(Event Object)**

이벤트란 개체의 상태 변화를 말한다.  즉, 이벤트는 이벤트 소스의 상태 변화를 설명합니다.

이벤트는 그래픽 사용자 인터페이스 구성 요소와의 사용자 상호 작용의 결과로 생성된다. 예를 들어 버튼 클릭, 마우스 이동, 키보드로 문자 입력, 목록에서 항목 선택, 페이지 스크롤 등이 이벤트를 발생시키는 활동이다.

Java AWT와 swing에서 구현된 이벤트의 종류들에는 아래와 같은 이벤트들이 지원된다.



**Java AWT와 swing에서의 이벤트 종류**

| 이벤트          | 발생 상황                                                    |
| --------------- | ------------------------------------------------------------ |
| ActionEvent     | 오브젝트의 상태 변화, 즉, 클릭, 메뉴 선택, 입력 완료 등 사용자의 행동과 관련된 이벤트 |
| ItemEvent       | 오브젝트에 포함된 여러 개의 아이템 중 하나 이상이 선택될 경우 발생하는 이벤트 |
| KeyEvent        | 키보드 키 입력                                          |
| MouseEvent      | 마우스 커서의 이동이나 버튼의 상태 변화가 발생할 경우        |
| FocusEvent      | 컴포넌트가 선택되거나 해제된 경우                            |
| TextEvent       | 텍스트가 변경될 때                                           |
| WindowEvent     | Window를 상속받는 모든 컴포넌트에 대해 window 활성화, 비활성화 등 |
| AdjustmentEvent | 스크롤바를 움직일 때                                         |
| ComponentEvent  | 컴포넌트가 사라지거나 이동, 크기 변경                        |
| ContainerEvent  | 컨테이너에 컴포넌트의 추가/삭제                              |



**이벤트 소스(Event Source)**

이벤트 소스랑 이벤트를 발생시키는 오브젝트를 말한다. 즉, 화면을 구성하거나 특정한 기능을 수행하는 오브젝트로서 외부적인 요인이나 특정 조건 등이 만족할 때 해당 이벤트를 생성한다.

Java AWT와 swing에서는 이벤트 소스에 이벤트 listener를 등록하여 이벤트 발생 시 처리할 수 있도록 지원한다.



**Java AWT와 swing에서의 이벤트 소스와 이벤트**

| 이벤트 소스       | 이벤트          | 발생 상황                                                    |
| ----------------- | --------------- | ------------------------------------------------------------ |
| Container         | ContainerEvent  | 컨테이너에 컴포넌트의 추가/삭제                              |
| Component         | ComponentEvent  | 컴포넌트가 사라지거나 이동, 크기 변경                        |
|                   | FocusEvent      | 컴포넌트가 포커스를 받거나 잃을 때                           |
|                   | KeyEvent        | 키를 누르거나 뗄 때                                          |
|                   | MouseEvent      | 마우스 버튼을 누르거나 뗄 때, 마우스 버튼을 클릭할 때, 컴포넌트 위에 마우스가 올라갈 때 등.. |
| Window            | WindowEvent     | Window를 상속받는 모든 컴포넌트에 대해 window 활성화, 비활성화 등 |
| JButton           | ActionEvent     | 마우스로 버튼 클릭                                           |
| JList             | ActionEvent     | 아이템을 더블클릭하여 리스트 아이템 선택                     |
| JMenuItem         | ActionEvent     | 특정 메뉴의 선택                                             |
| JTextField        | ActionEvent     | 텍스트를 입력한 후 엔터키 입력                               |
| JCheckBox         | ItemEvent       | 체크박스의 선택/해제                                         |
| JCheckBoxMenuItem | ItemEvent       | 체크박스 메뉴아이템의 선택/해제                              |
| JList             | ItemEvent       | 리스트 아이템 선택                                           |
| JScrollBar        | AdjustmentEvent | 스크롤바를 움직일 때                                         |



**Event Listener**

이벤트가 발생했을 때 그 처리를 담당하는 오브젝트를 가리키며, 해당 오브젝트에서 이벤트를 처리하는 함수를 event handler라고도 한다.

Java에서는 인터페이스를 이용해 event listener의 기본 형태를 정의하고 있으며, 개별 응용에 따라 해당 인터페이스를 클래스로 구현하여 이벤트를 처리할 수 있도록 한다.



**Event Listener Interface**

event listener interface는 이벤트 소스에서 이벤트가 발생할 경우, 이에 대한 처리를 등록하기 위한 인터페이스로서 각각의 이벤트별로 정의되어 있다. 이벤트 소스에서는 이벤트가 발생하면 등록된 event listener의 정해진 함수를 호출하여 처리하게 되므로, 해당 이벤트 소스로부터 이벤트를 받아 처리하고자 할 경우에는 해당 이벤트의 event listener interface를 이용해 구현 후 등록하면 된다.

Java에서 유효한 event listener interface는 아래와 같은 종류들이 있다.

**Java AWT와 swing 이벤트와 event listener interface**

| Evnet | Event Listener Interface | 설명                                                         |
| -------------- | :----------------------- | :----------------------------------------------------------- |
| ActionEvent    | ActionListener           | 버튼 클릭이나 텍스트 필드 변경 등의 작업에 응답한다.          |
| ItemEvent      | ItemListener             | 개별 항목 변경 사항을 수신한다.(예: checkbox)                |
| KeyEvent       | KeyListener              | 키보드 입력을 수신한다.                                      |
| MouseEvent     | MouseListener            | 마우스에서  발생하는 이벤트를 수신한다(예 : 클릭, 더블 클릭, 오른쪽 클릭 등) |
|                | MouseMotionListener      | 마우스 움직임을 수신한다.                                    |
| FocusEvent     | FocusListener            | component가 포커스를 받거나 잃을 때 수신한다.                 |
| WindowEvent    | WindowListener           | window에서 발생하는 이벤트를 수신한다.                       |
| ContainerEvent | ContainerListener        | 컨테이너에서 발생하는 이벤트를 수신한다.(예 : JFrame, JPanel 등) |
| ComponentEvent | ComponentListener        | 구성 요소의 변경 사항을 수신한다. (예 : 레이블 이동, 크기 조정 등) |
| AdustmentEvent | AdjustmentListener       | 조정을 수신한다. (예 : 스크롤 바 작동)                       |



#### 문제 41. 계산기 만들기

* 간단한 정수 계산기를 만든다
* AC를 누르면 초기화된다.
* 피연산자가 입력된 상태에서 연산자를 누르면 계산된다.
  * 피연산자, 연산자, 피연산자가 입력된 상태일 때, 연산자를 누르면 계산 후 피연산자, 연산자 표시된다.
  * 피연산자 하나만 있을 경우, 피연산자, 연산자로 표시된다.
  * 피연산자 입력 시에는 피연산자만 보인다.


<p>
  <img src="./image/figure40.png" alt="계산기" />
</p>


#### 문제 42. 프레임에서 키보드 사용으로 인해 발생하는 KeyEvent를 처리하라

*  프레임을 생성한다
*  프레임에서 발생하는 KeyEvent 처리를 위한 KeyListener를 등록한다
*  KeyListener에서 필요한 이벤트 핸들러는 아래와 같다
   * keyPressed(KeyEvent e)
   * keyReleased(KeyEvent e)
   * keyTyped(KeyEvent e)
*  Event handler에서는 이벤트 발생 시 화면에 키값을 출력한다



**실행 결과**
<p>
  <img src="./image/figure41.png" />
</p>


#### 문제 43. KeyEvent 사용 시 문제점을 해결하라

* KeyEvent 처리를 위해 KeyListener를 등록하고 키를 누르고 있는 경우, keyPressed와 keyTyped가 반복적으로 호출된다.

* 해당 키를 반복적으로 입력하기 위해 누르고 있는 경우라면 의도에 맞게 처리될 수 있지만, 키를 누르는 순간을 검출하기에는 부족한 면이 있다.

* keyPressed는 키를 누르는 순간 1회만 발생하도록 수정하라.



#### 문제 44.  반투명 프레임을 만들고, 슬라이드를 이용해 반투명도 제어하기

* setOpacity(float f)를 이용해 프레임의 불투명도 설정 가능. 단, 불투명도 제어를 위한 조건이 만족하여야 한다.

* 불투명도는 0 ~ 100으로 설정할 수 있으며, 100이 가장 불투명한 상태이다.

* 불투명도가 변경되면, 해당 값을 아래에 출력한다.



**참고**

* setPaintTicks(boolean b) : 눈금 표시

* setPaintLabels(boolean b) : 수치 레이블 표시

* setMajorTickSpacing(int n) : 큰 눈금 간격

* setMinorTickSpacing(int n) : 작은 눈금 간격

* addChangeListener(ChangeListener l) : Event listener 설정



**실행 결과**
<p>
  <img src="./image/figure42.png" alt="Opacity01" style="zoom:50%;" />
</p>

**샘플 코드**

~~~java
import javax.swing.*;
import javax.swing.event.*;

class OpacityFrameEx1 extends JFrame implements ChangeListener {
	JSlider slider;
	JLabel label;

	public OpacityFrameEx1() {
		// 불투명도 설정 슬라이드 (0 ~ 100)
		this.slider = new JSlider(0, 100, 100);
		// 값 출력용 레이블
		this.label = new JLabel();

		// 프레임 크기 설정
		this.setSize(300, 100);

		// 슬라이드에 트랙, 틱,  표시
		this.slider.setPaintTrack(true);
		this.slider.setPaintTicks(true);
		this.slider.setPaintLabels(true);

		// 슬라이드 눈금을 표시(작은 눈금 5, 큰 눈금 20)
		this.slider.setMajorTickSpacing(20);
		this.slider.setMinorTickSpacing(5);

		// 슬라이드값이 변경될 때마다 호
		this.slider.addChangeListener(this);

		// 패널 구성
		JPanel panel = new JPanel();
		panel.add(this.slider);
		panel.add(this.label);

		this.add(panel);

		// 초기 표시
		this.stateChanged(null);
	}

	@Override
	public void stateChanged(ChangeEvent e) {
		// 불투명도 출력
		this.label.setText("Opacity value is =" + this.slider.getValue());
		// 불투명도 설정
		this.setOpacity(this.slider.getValue() * 0.01f);
	}

	// main class
	public static void main(String[] args)
	{
		OpacityFrameEx1 frame = new OpacityFrameEx1();

		// 프레임을 불투명도로 제어하기 위해서는 타이틀바 등의 장식이 없어야 한다.
		frame.setUndecorated(true);

		// 타이틀바가 없어 이동할 수 없음. 생성 시 특정 위치에 생성
		frame.setLocation(500, 300);

		// 화면 출력
        frame.setVisible(true);
	}
}
~~~



### 12-2. 사용자 정의 이벤트



## 13. 게임 만들기

**Keyword**

* GUI





### 13-1. 게임 구성 요소 만들기

게임 화면에는 다양한 물체들에 존재할 수 있다.

포탄을 발사하는 포, 포탄이 튕겨져 나오는 벽이나 물체, 포탄이 박혀 버리는 늪지대 그리고 포탄으로 맞추면 점수를 얻을 수 있는 대상 물체 등이 있다.



#### 13-1-1. 포 만들기

* 포탄을 발사하는 물체로서 포탄의 각도, 속도 등을 조정할 수 있다.

* 포신의 끝에서 포탄이 발사된다.

* 포신은 한쪽이 고정되어 있고, 다른 한쪽은 원을 그리며 회전할 수 있다.

* 포탄을 맞으면 게임은 끝난다.





#### 13-1-2. 늪지대 만들기

* 포탄은 물체에 부딪힐 경우 튕기지만 늪지대에서는 튕기지 않고, 멈춰 버린다.
* 포탄이 늪지대에 빠지면 해당 게임은 목표물을 맞히지 못한 것이다.
* 늪지대가 반드시 바닥에 위치할 필요는 없다.



#### 13-1-3. 목표물 만들기

* 포탄을 맞으면 게임이 승리로 끝난다.





### 13-2. GameWorld 클래스

#### 13-2-1. 정의

* 게임이 실행되는 공간으로 다용한 구성 요소가 포함된다.

* * 대포

  * 장애물

  * 목표물



* 게임 운영에 필요한 제어가 추가된다.

  * 포탄 발사

  * 초기화



* 게임 내에서 발생하는 사건(?)을 외부로 알리기 위한 방법 제공

  * 포탄 발사 이벤트
    * 목표물 맞히기가 끝나기 전까지 다음 포탄 발사와 같이 UI를 제어할 수 있다.
  * 목표물 맞히기 성공
    * 점수를 증가시키고, 다음 장면을 준비한다.
  * 목표물 맞히기 실패
    * 실패 횟수를 증가시키고, 동일한 환경에서 다시 실행할 수 있도록 준비한다.



##### 필드

* 구성 요소 목록
* 발사 횟수
* 성공 횟수
* 게임 상태



##### 함수

* 포탄 발사 (fire)
* 초기화 (init)
* 포탄 발사 준비 확인(isReady)
* 포탄 발사 횟수(getNumberOfFire)
* 성공 횟수(getNumberOfHit)



### 13-3. Game 클래스

* 게임을 구성할 Frame
* 다양한 component를 이용하여 게임 인터페이스 구성
* 인터페이스와 게임 화면이 연결될 수 있도록 지원한다.



#### 13-3-1. 화면 구성하기

* 게임 화면을 구성한다.



##### 문제. JFrame을 확장하여 Game Frame을 정의하고, 다음에 나열된 기능 지원을 위한 component를 추가하여 배치한다.

* 게임 화면을 추가한다.
* 포탄 발사 버튼을 추가한다.
* 외부 효과인 바람 세기를 위한 슬라이드를 추가한다.
* 점수판을 추가한다.



#### 13-3-2. 제어 연결

* 구성된 화면에서 component가 동작할 수 있도록 동작을 추가해 보자.



## 13. 벽돌 깨기 만들기

고전적인 게임 중 벽돌 깨기가 있다. 게임 플레이어가 발사한 볼은 공간에 튕기면서 돌아다니고, 공간 내에 존재하는 다양한 막대들은 특성에 따라 깨지거나 반사만 시키는 등 다양한 행동을 취한다.

앞서 만든 대포 게임과 다른 게임이지만 기본 동작은 동일하다.



​
