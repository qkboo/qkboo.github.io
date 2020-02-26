## Path

### Polygon

We can convert between polar and cartesian coords with this formula:

```java
val x = radius * Math.cos(angle);
val y = radius * Math.sin(angle);
```

내각이 일정한 다각형을 그리는데 극좌표가 유용하다. 내각의 합이 360이 되게 계산하고 각 정점에 대해 내각과 반지름으로 곱하면 된다. 그리고 이 점을 데카르트 좌표로 변환할 수 있다.


#### makePolygon()

먼저 다각형의 정점 수에 따른 점의 각을 계산한다.

원주의 둘레는 \\(2*\pi * R\\) 이고, 라디안으로 PI를 정점 수로 나누면 한 점의 각이 된다.

```java
double angle = 2.0 * Math.PI / sides;
```

각 정점을 그리기 위해 시작 점을 찾는다.

```java
double startAngle = Math.PI / 2.0 + Math.toRadians(360.0 / (2 * sides));
```