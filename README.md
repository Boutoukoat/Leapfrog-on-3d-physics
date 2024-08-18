# 3D physics

<p>I needed a simple visual animation on idle screens during the dead times of a demo.</p>

<p>These html files are based on examples originating from (https://exploratoria.github.io/exhibits/mechanics/) </p>

<p>They have been substantially modified to fix the bugs, correct the dynamics and side effects, handle
collisions and then implement the leapfrog technique (https://en.wikipedia.org/wiki/Leapfrog_integration)
for a better stability and improved rendering</p>

<p>The file [elastic_collisions_in_3d](https://boutoukoat.github.io/elastic_collisions_in_3d.html) shows the trajectory of colliding spheres of variable size, with an initial random speed</p>

<p>The file [attraction_repulsion_3d](https://boutoukoat.github.io/attraction_repulsion_3d.html) shows a red sphere attracting green spheres while the green spheres repulse themselves, all together constrained in a cube.</p>

<p>The file [repulsion_3d](https://boutoukoat.github.io/repulsion_3d.html) shows a variable number of repulsing green spheres with an initial random speed, constrained in a sphere. 4 spheres should converge to the vertices of a regular tetrapod. Will this stabilize numerically ? 8 spheres should converge to the corners of a cube. Will this be numeroically stable ?</p>

# Fixed bugs
 
  - overlap at creation time
  - sphere overlaps at high speeds
  - sphere overlaps after a collision
  - bouncing outside of the cube
  - order of operations across spheres
  - ...
  - ...




