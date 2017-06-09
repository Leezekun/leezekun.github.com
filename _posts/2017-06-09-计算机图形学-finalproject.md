# Raytracing 实验步骤：
1.未使用opengl等开源库。自己写了一些基本的图形，如cuboid,cylinder,sphere,spheroid,triangle,torus，以上都属于SolidObject类。在solid.c文件中定义。

2.对于每一种物体，都定义了如何求与光线的所有交点的函数：AppendAllIntersections（）。例如其中sphere与光线的交点函数：
<pre><code>
    void Sphere::AppendAllIntersections(
        const Vector& vantage, 
        const Vector& direction, 
        IntersectionList& intersectionList) const
    {
        const Vector displacement = vantage - Center();
        const double a = direction.MagnitudeSquared();
        const double b = 2.0 * DotProduct(direction, displacement);
        const double c = displacement.MagnitudeSquared() - radius*radius;
        const double radicand = b*b - 4.0*a*c;
        if (radicand >= 0.0)
        {            const double root = sqrt(radicand);
            const double denom = 2.0 * a;
            const double u[2] = {
                (-b + root) / denom,
                (-b - root) / denom
            };

            for (int i=0; i < 2; ++i)
            {
                if (u[i] > EPSILON)
                {
                    Intersection intersection;
                    const Vector vantageToSurface = u[i] * direction;
                    intersection.point = vantage + vantageToSurface;
                    intersection.surfaceNormal = 
                        (intersection.point - Center()).UnitVector();

                    intersection.distanceSquared = 
                        vantageToSurface.MagnitudeSquared();

                    intersection.solid = this;
                    intersectionList.push_back(intersection);
                }
            }
        }}
</code></pre>  

3.编写了algebra文件，定义了向量之间的一些操作，为之后的操作做准备。
例如求解线性方程的函数：
<pre><code>
bool SolveLinearEquations(
  
        double D, double E, double F, double G,
        double H, double I, double J, double K,
        double L, double M, double N, double P,
        double &u, double &v, double &w)
    {
        

        if (fabs(F) >= TOLERANCE)      /        {
            const double b = E*J - F*I;
            if (fabs(b) >= TOLERANCE)      /            {
                const double a = D*J - F*H;
                const double d = H*N - J*L;
                const double e = I*N - J*M;
                const double denom = a*e - b*d;
                if (fabs(denom) >= TOLERANCE)                      {
                   
                    const double c = G*J - F*K;
                    const double f = K*N - J*P;

                    u = (b*f - e*c) / denom;
                    v = -(a*u + c) / b;
                    w = -(D*u + E*v + G) / F;

                    return true;                     }
            }
        }
</code></pre> 
例如求解二次方程的函数：
<pre><code>
int SolveQuadraticEquation(complex a, complex b, complex c, complex roots[2])
    {
        if (IsZero(a))
        {
            if (IsZero(b))
            {
                          return 0;   
            }
            else
            {
                roots[0] = -c / b;
                return 1;  
            }
        }
        else
        {
            const complex radicand = b*b - 4.0*a*c;
            if (IsZero(radicand))
            {
                roots[0] = -b / (2.0 * a);
                return 1;
            }
            else
            {
    
                const complex r = sqrt(radicand);
                const complex d = 2.0 * a;

                roots[0] = (-b + r) / d;
                roots[1] = (-b - r) / d;
                return 2;
            }
        }
    }

    complex cbrt (complex a, int n)
    {

        const double TWOPI = 2.0 * 3.141592653589793238462643383279502884;

        double rho   = pow(abs(a), 1.0/3.0);
        double theta = ((TWOPI * n) + arg(a)) / 3.0;
        return complex (rho * cos(theta), rho * sin(theta));
    }
</code></pre>

4.定义了scene类，即场景。在场景中可以添加各种solidobject类。
在scene类中定义了具体的raytracing的操作。
（1）定义了最大递归次数,定义了最小光照强度：
<pre><code>
const int MAX_OPTICAL_RECURSION_DEPTH = 10;//设置为1时即为No Raytracing
const double MIN_OPTICAL_INTENSITY = 0.001;
</code></pre>
（2）定义了traceray函数,选择出距离视点最近的交点，并算出此点的color像素。
<pre><code>
Color Scene::TraceRay(
        const Vector& vantage,
        const Vector& direction,
        double refractiveIndex,
        Color rayIntensity,
        int recursionDepth) const
    {
        Intersection intersection;
        const int numClosest = FindClosestIntersection(
            vantage, 
            direction, 
            intersection);

        switch (numClosest)
        {
        case 0:
            return rayIntensity * backgroundColor;

        case 1:
            return CalculateLighting(
                intersection,
                direction,
                refractiveIndex,
                rayIntensity,
                1 + recursionDepth);

        default:
            throw AmbiguousIntersectionException();
        }
}
</code></pre>

(3)定义了findclosestintersectio（）函数根据direction选择最近交点。
<pre><code>
int Scene::FindClosestIntersection(
        const Vector& vantage, 
        const Vector& direction, 
        Intersection& intersection) const
    {
        // Build a list of all intersections from all objects.
        cachedIntersectionList.clear();     // empty any previous contents
        SolidObjectList::const_iterator iter = solidObjectList.begin();
        SolidObjectList::const_iterator end  = solidObjectList.end();
        for (; iter != end; ++iter)
        {
            const SolidObject& solid = *(*iter);
            solid.AppendAllIntersections(
                vantage, 
                direction, 
                cachedIntersectionList);
        }
        return PickClosestIntersection(cachedIntersectionList, intersection);    }
 </code></pre>
 
 (4)（5）定义了计算反射光，漫反射光，折射光等函数，求得交点出的color。
 <pre><code>
 Color Scene::CalculateReflection(
        const Intersection& intersection, 
        const Vector& incidentDir, 
        double refractiveIndex,
        Color rayIntensity,
        int recursionDepth) 
Color Scene::CalculateRefraction(
        const Intersection& intersection, 
        const Vector& direction, 
        double sourceRefractiveIndex,
        Color rayIntensity,
        int recursionDepth,
        double& outReflectionFactor)
Color Scene::CalculateLighting(
        const Intersection& intersection, 
        const Vector& direction, 
        double refractiveIndex,
        Color rayIntensity,
        int recursionDepth) 
 </code></pre>
 
 
# 以下为效果展示：
## 效果比较图，上为raytrace 20 times的效果图，下为no raytrace的图。
(1)chessboard:

![](/images/blog/chessboard_20.png)
![](/images/blog/chessboard_0.png)

左raytracing，右no raytracing：

![](/images/blog/halfhalf.png)

(2)multisphere:

![](/images/blog/multisphere_20.png)
![](/images/blog/multisphere_1.png)
