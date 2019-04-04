## Basic formulas:
### 2-dimensional
$$ \vec p_1 = (x_1, y_1) $$

$$ \vec p_2 = (x_2, y_2) $$

$$ \theta = \angle p_1 O p_2 $$

$$
\vec p_1 \centerdot \vec p_2 = x_1 \centerdot x_2 + y_1 \centerdot y_2 = || \vec p_1 || \centerdot || \vec p_2 || \centerdot cos \theta
$$

$$
\vec p_1 \times \vec p_2 = x_1 \centerdot y_2 - y_1 \centerdot x_2 = || \vec p_1 || \centerdot || \vec p_2 || \centerdot sin \theta
$$

$$ || \vec p_1 || = \sqrt{x_1^2 + y_1^2}$$





### 3-dimensional
$$ \vec p_1 = (x_1, y_1, z_1) $$

$$ \vec p_2 = (x_2, y_2, z_2) $$

$$ \theta = \angle p_1 O p_2 $$

$$
\vec p_1 \centerdot \vec p_2 = x_1 \centerdot x_2 + y_1 \centerdot y_2 + z_1 \centerdot z_2 = || \vec p_1 || \centerdot || \vec p_2 || \centerdot cos \theta
$$

$$
\vec p_1 \times \vec p_2 =
    \begin{vmatrix}
        i & j & k \\
        x_1 & y_1 & z_1 \\
        x_2 & y_2 & z_2 \\
    \end{vmatrix}
    = (x_3i, y_3j, z_3k)
$$





## Parallel / Orthogonal of Lines
$$ \because p_1 // p_2 $$

$$ \therefore p_1 \times p_2 = 0 \&\& p_1 \varsubsetneq p_2 $$

$$ \because p_1 \bot p_2 $$

$$ \therefore p_1 \centerdot p_2 = 0$$




## Rotate the Vector :
Vector : Vector $ p = (x, y) $
Angle : $ \theta $

$$
(xcos \theta - y sin \theta, x sin \theta + y cos \theta)
$$



## Whether Point is on Segment
Segment : Point $p_1$ && Point $p_2$
Point : Point p
$$
(\vec p - \vec p_1) \times (\vec p_2 - \vec p_1) = 0
$$

$$
(\vec p - \vec p_1) \centerdot (\vec p - \vec p_2) <= 0 $$ ##The Intesection of Segments: $$ \vec p = \vec p_1 + ( \vec p_2 - \vec p_1) \centerdot \frac {(\vec p_1 - \vec q_1) \times (\vec q_2 - \vec q_1) } {(\vec p_2 - \vec p_1) \times (\vec q_2 - \vec q_1)} $$ ##Point to Line Projection: Line : Point s && Direction $\vec d$ Point : Point p Get Point a that is the Projection of p to Line l $$ \vec a = \vec s + \vec d \centerdot \frac {( \vec p - \vec s) \times \vec d} {||\vec d||} $$ ##The Distance of Point to Line(Segment which need to discuss the situations): Line : Point $p_1$ && Point $p_2$ Point : Point p $$ d = \frac {| \vec a \times \vec b |} {| \vec a |} = \frac {| (\vec p_2 - \vec p_1) \times ( \vec p - \vec p_1) | } {| \vec p_2 - \vec p_1 |} $$ 

## Whether Segments are Intersacted Segment0 : Point $p_1$ && Point $p_2$ Segment1 : Point $q_1$ && Point $q_2$ 

* 快速排斥试验 
    ```cpp max(p1.x, p2.x)>= min(q2.x, q2.y)
    max(p1.y, p2.y) >= min(q1.y, q2.y)
    max(q1.x, q2.x) >= min(p1.x, p2.x)
    max(q1.y, q2.y) >= min(p1.y, p2.y)
    ```

* 跨立试验
$$
    (q_1 - p_1) \times (p_2 - p_1) \centerdot (q_2 - p_1) \times (p_2 - p_1)  <= 0 \&\& (p_1 - q_1) \times (p_2 - q_1) \centerdot (p_2 - q_1) \times (p_2 - p_1) <= 0
$$



## The Volume formula:
$$
    \begin{Vmatrix}
        x_1 & y_1 & z_1 & 1\\
        x_2 & y_2 & z_2 & 1\\
        x_3 & y_2 & z_3 & 1\\
        y_4 & y_4 & z_4 & 1\\
    \end{Vmatrix}
$$

## The superficial area:
$$
    \begin{Vmatrix} (b-a)\times(c-a) \end{Vmatrix}+
    \begin{Vmatrix} (b-a)\times(d-a) \end{Vmatrix}+
    \begin{Vmatrix} (c-a)\times(d-a) \end{Vmatrix}+
    \begin{Vmatrix} (c-b)\times(d-b) \end{Vmatrix}
$$

## The radius of inscribled sphere:
$$
    \frac {
        \begin{Vmatrix}
            x_1 & y_1 & z_1 & 1\\
            x_2 & y_2 & z_2 & 1\\
            x_3 & y_2 & z_3 & 1\\
            y_4 & y_4 & z_4 & 1\\
        \end{Vmatrix}
    } {
        \begin{Vmatrix} (b-a)\times(c-a) \end{Vmatrix}+
        \begin{Vmatrix} (b-a)\times(d-a) \end{Vmatrix}+
        \begin{Vmatrix} (c-a)\times(d-a) \end{Vmatrix}+
        \begin{Vmatrix} (c-b)\times(d-b) \end{Vmatrix}
    }
$$

## The center of the inscribled sphere:
$$
    \frac {
        \begin{Vmatrix} (b-a)\times(c-a) \end{Vmatrix}\times d+
        \begin{Vmatrix} (b-a)\times(d-a) \end{Vmatrix}\times c+
        \begin{Vmatrix} (c-a)\times(d-a) \end{Vmatrix}\times b+
        \begin{Vmatrix} (c-b)\times(d-b) \end{Vmatrix}\times a
    } {
        \begin{Vmatrix} (b-a)\times(c-a) \end{Vmatrix}+
        \begin{Vmatrix} (b-a)\times(d-a) \end{Vmatrix}+
        \begin{Vmatrix} (c-a)\times(d-a) \end{Vmatrix}+
        \begin{Vmatrix} (c-b)\times(d-b) \end{Vmatrix}
    }
$$
