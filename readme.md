# Three.js Physics Playground — Scientific Notes

## Overview

This feature is a browser-based 3D physics playground built with **Three.js**. It simulates simple rigid-body-like motion for geometric objects such as spheres, cubes, and tetrahedrons, all inside an enclosed room scene.

The main goal is to provide an interactive sandbox where users can observe:

- gravitational motion
- elastic collisions
- wall and floor bounce response
- drag-based direct manipulation
- adjustable lighting and material shading

## Core motion model

The simulation uses a basic discrete-time integrator. At each step, velocity and position are updated using the following equations:

$$
\mathbf{v}(t + \Delta t) = \mathbf{v}(t) + \mathbf{a}(t)\,\Delta t
$$

$$
\mathbf{x}(t + \Delta t) = \mathbf{x}(t) + \mathbf{v}(t + \Delta t)\,\Delta t
$$

Where:

- $\mathbf{x}$ is the object position
- $\mathbf{v}$ is the object velocity
- $\mathbf{a}$ is acceleration
- $\Delta t$ is the simulation step size

## Gravity

Gravity is applied as a constant acceleration vector:

$$
\mathbf{a}_g = (0, -g, 0)
$$

The gravity slider controls $g$, which changes the overall fall speed and bounce behavior of the bodies.

## Bounce response

When a body collides with the floor, walls, or ceiling, its velocity is reflected along the collision normal and scaled by a restitution coefficient $e$:

$$
v' = -e\,v
$$

For body-to-body impacts, the response is approximated using a collision impulse along the contact normal:

$$
j = -\frac{(1 + e)\,(\mathbf{v}_{rel} \cdot \mathbf{n})}{\sum m^{-1}}
$$

where:

- $\mathbf{v}_{rel}$ is the relative velocity
- $\mathbf{n}$ is the collision normal
- $m^{-1}$ is inverse mass

This gives the scene a believable bouncing feel without using a full rigid-body solver.

## Lighting model

The playground combines a global directional light with a draggable lamp rig.

### Global light

The main light direction is defined by azimuth and elevation angles. In spherical form, the light direction can be expressed as:

$$
\mathbf{d} = \begin{bmatrix}
\cos(\phi)\cos(\theta) \\
\sin(\phi) \\
\cos(\phi)\sin(\theta)
\end{bmatrix}
$$

where:

- $\theta$ is the azimuth angle
- $\phi$ is the elevation angle

The global light intensity affects the overall scene brightness and ambient fill.

### Lamp rig

The lamp is a draggable 3D object that acts as an additional local light source. Its direction is also controlled with azimuth and elevation sliders.

The lamp contributes:

- a spotlight component for focused illumination
- a point-light component for soft nearby fill
- a visible mesh model so the light source is easy to identify in the scene

## Material shading and gloss

The surface gloss slider modifies the physical appearance of both the room and the shapes.

In practical terms, it adjusts:

- **roughness**: lower roughness means shinier reflections
- **metalness**: slightly increases specular response on materials
- **emissive intensity**: subtle glow contribution for lit shapes

This creates a simple visual model of material reflectance:

$$
\mathsf{shininess} \propto \frac{1}{\mathsf{roughness} + \epsilon}
$$

## Room geometry

The environment is an enclosed room with physics boundaries on the floor, sides, and ceiling.

The enclosure is designed so that objects remain inside the play area unless explicitly allowed to escape through an open boundary in earlier versions.

## Interaction controls

### Object spawning

Users can spawn:

- spheres
- cubes
- tetrahedrons

### Direct manipulation

Objects can be dragged with the mouse. When released, their motion continues using the velocity inferred from the drag movement.

### Other controls

- **Gravity strength** — adjusts downward acceleration
- **Equation variable $\Delta t$** — adjusts the simulation step size
- **Bounce coefficient $e$** — changes collision energy retention
- **Global light** — modifies scene illumination direction and intensity
- **Lamp** — changes the secondary light source and its orientation
- **Surface gloss** — changes the visual shininess of room and objects

## Suggested usage

For a stable and readable scene:

- keep gravity around the default value for natural motion
- use moderate gloss for visible highlights
- use the lamp to create directional shadows on the objects
- increase bounce for more energetic collisions

## Notes

This is a lightweight educational physics model, not a full scientific rigid-body engine. It is intended for visualization, experimentation, and interactive exploration rather than high-precision simulation.

## Keywords

Three.js, physics simulation, gravity, restitution, lighting, shading, rigid-body approximation, draggable lamp, 3D geometry, browser visualization