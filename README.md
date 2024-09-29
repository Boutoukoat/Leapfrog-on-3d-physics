# 3D physics

<p>I needed a simple visual animation on idle screens during the dead times of a demo.</p>

<p>These html files are based on examples originating from (https://exploratoria.github.io/exhibits/mechanics/) </p>

<p>They have been substantially modified to fix the bugs, correct the dynamics and side effects, handle
collisions and then implement the leapfrog technique (https://en.wikipedia.org/wiki/Leapfrog_integration)
for a better stability and improved rendering</p>

<p>The file [elastic_collisions_in_3d](https://boutoukoat.github.io/elastic_collisions_in_3d.html) shows the trajectory of colliding spheres of variable size, with an initial random speed</p>

<p>The file [attraction_repulsion_3d](https://boutoukoat.github.io/attraction_repulsion_3d.html) shows a red sphere attracting green spheres while the green spheres repulse themselves, all together constrained in a cube.</p>

<p>The file [repulsion_3d](https://boutoukoat.github.io/repulsion_3d.html) shows a variable number of repulsing green spheres with an initial random speed, constrained in a sphere. 4 spheres should converge to the vertices of a regular tetrapod. Will this stabilize numerically ? 8 spheres should converge to the corners of a cube. Will this be numeroically stable ?</p>

# Bugs fixed in original source code
 
  - overlap at creation time
  - sphere overlaps at high speeds
  - sphere overlaps after a collision
  - bouncing outside of the cube
  - order of operations across spheres
  - ...
  - ...

# Improvements

 The aim is not to build an exact physics engine. This task is impossible within a few hundreds lines of code.
 The aim is to make a realistic animation without side effects 

# Usual algorithm (from wikipedia)

```
 loop invariant : x[i], v[i] known
 a[i] = attraction_forces(x[i])
 x[i+1] = x[i] + v[i]\*dt + 1/2 \* a[i]\*dt^2
 v[i+1] = v[i] + a[i]\*dt
 x[i+1], v[i+1] are now known, loop i+1 can start
```
 
# Leapfrog algorithm (from wikipedia)  (compute the speed once from an averaged acceleration along the time interval)

```
 loop invariant : x[i], v[i], a[i] known
 x[i+1] = x[i] + v[i]\*dt + 1/2 a[i]\*dt^2
 a[i+1] = attraction_forces(x[i+1])
 v[i+1] = v[i] + 1/2 \* (a[i]+a[i+1])\*dt
 x[i+1], v[i+1], a[i+1] are now known, loop i+1 can start
```
 
# Leapfrog algorithm (from wikipedia)  (compute the speed twice at mid-time interval and at end of time interval)

```
 loop invariant : x[i], v[i], a[i] known
 vtemp = v[i] + a[i]\*dt
 x[i+1] = x[i] + vtemp\*dt
 a[i+1] = attraction_forces(x[i+1])/2
 v[i+1] = vtemp + a[i+1]\*dt
 x[i+1], v[i+1], a[i+1] are now known, loop i+1 can start
```
 
 # Interaction across elements inside the Leapfrog loop

 when considering the interation between elements (attraction, repulsion, gravity, overlap and collisions, bounding volume, ....)

```
 v1temp = v1[i] + a1[i]\*(dt/2)
 v2temp = v2[i] + a2[i]\*(dt/2)
 ...
 vntemp = vn[i] + an[i]\*(dt/2)
```

 between elements b1 at position x1[i] and b2 at position x2[i], solve overlaps by making b1 and b2 adjacent, and not changing the speed.
 (overlaps are created by the order of calculations during collisions across 3 and more elements, they can't be avoided with simple code)
```
    dx = x1[i] - x2[i]
    if (dx length indicates an existing overlap)
        x1[i] -= overlap/2
        x2[i] += overlap/2
```

 pairwise evaluate dx = x1[i+1] - x2[i+1] 
```
    dx = (x1[i] + v1temp[i]\*dt) - (x2[i] + v2temp[i]\*dt)
```
    
    anticipate on collisions by modifying v1temp and v2temp (take care of mass, friction, amortization ...)
```
    if (dx length is too small)
       v1temp = elastic_collision(v1temp, dx)
       v2temp = elastic_collision(v2temp, dx)
```

```
 x1[i+1] = x1[i] + v1temp\*dt
 x2[i+1] = x2[i] + v2temp\*dt
 ...
 xn[i+1] = xn[i] + vntemp\*dt
```

```
 a1[i+1] = attraction_forces(x[i+1])
 a2[i+1] = attraction_forces(x[i+1])
 ...
 an[i+1] = attraction_forces(x[i+1])
```

 bounce on bounding volume if needed
```
    v1temp modified (sign inverted)
    a1[i+1] = 0
    x1[i+1] = set on bounding limits 
```

```
 v1[i+1] = v1temp + a1[i+1]\*(dt/2)
 v2[i+1] = v2temp + a2[i+1]\*(dt/2)
 ...
 vn[i+1] = vntemp + an[i+1]\*(dt/2)
``

```
 render(x[i+1])
```
  

