--

Welcome everyone & thank them for coming

As by consensus vote in slack, we are going to learn a bit about ray tracing today

--

Kaylee has graciously prepared a shell application in Python for us to use as a starting point tonight

If you want to start from scratch or in a different language, you may, but you probably won't have time to finish tonight

--

This talk will lay the foundation of what we are going to implement at a high level. We'll see a few algorithms, but the focus will be abstract and I'll provide more detail & have resources available tonight

Light models are ways of conceptualizing light that attempt to approach a physical approximation of the behavior of light while still being calcuable

--

Ray tracing v raster graphics = movies v games

I can talk about raster graphics later if anyone is interested

Shadows -> Super Mario 64

Reflection -> Portal

Ray tracing is old school & mostly not used in practice
Path tracing allows for more realistic effects of how light actually behaves in a scene

Cycles renderer in Blender

--

Cameras are how we simulate an observer in a scene

a simple camera has a position, a vector that represents its direction of view, a field of view angle, and an aspect ratio

We use the camera to translate between image space (2D coordinates) and model / world space (3D coordinates)

With ray tracing, we generate a ray or a set of rays per pixel in the image and get the color for each pixel by tracing them

--

Surfaces are things that exist in the world that can interact with light.

In order to interact with light in the form of rays, we need well-defined equations of intersection. From those equations, we can then perform lightling calculations.

Surfaces have materials that allow us to customize their properties. In more complex implementations, this can be things that give a rough, textured appearance instead of a perfectly smooth one. For our purposes, surfaces will have a color and a reflective property.

--

When we search for intersections in a scene, we want the closest intersection along the ray, because that will be the object directly in our view.

Once we have that, we perform additional intersection tests with all of the lights in the scene. These are able to tell us if a particular light contributes to the illumination model or not.

--

In the lighting models we're going to cover, there are three types of light. (don't make a Law & Order joke)

Ambient light is the base level of light in a scene. In most ray tracing implementations, this is a constant color. In path tracing implementations, this can be achieved through tracing rays.

Diffuse light is the light that is reflected off a surface and scattered. White walls and ping-pong balls are classic examples of diffuse light.

Specular light is the light that is reflected off ao surface directly toward the viewer. Pool balls are the classic example of specular light.

--

As I mentioned, pingpong balls are a good example of diffuse light. When you look at the picture, you can see that the color more directly under the light is brighter and the color at the edges is darker.

Diffuse light is relevant to the surface and the light source. Because of this, you can walk in a circle around the object and the light does not change. This contrasts with specular highlights, which are relevant to the viewer in addition to the surface and light source.

Lambertian illumination is a way to simulate the effect of diffuse reflection. Lambert observed that the amount of light reflected was related to the cosine of the angle between the surface normal and the direction toward the light source.

--

So we're going to go over some equations here, but the important thing is to understand their shape rather than memorize them outright. I'll provide a set of equations tonight that are more tailored towards implementations.

In this equation, L is the direction toward the light and N is the surface normal. In vector math, the dot product of two vectors is the cosine of the angle between them. C is the color of the surface and IL is the intensity of the light.

--

The Phong illumination model takes the work that had been done with Lambertian illumination and adds a specular highlight component. This image shows how each individual piece of lighting contributes to the final image.

If you remember the original Toy Story movie, Phong illumination can be used to create renderings that have the same plastic quality that the movie had.

--

If we look at this equation, we can see a couple pieces. The first one is the ambient light component with the color and the amount that contributes to the final color. The next piece can be written separately, but here it is written as a sum of two pieces. For every light, we are going to calculate the lambertian component for diffuse light and also a specular component.

The specular piece is not that important to our purposes, but it is interesting to understand. It uses the equation below to calculate the dot product between the vector toward the viewer and a vector that represents the angle of perfect reflection of light from that source off of the surface. It then has an exponent to model if the surface has tight, bright reflection (large exponent) or if the reflection is larger and slightly less glaring.

--

Welcome to the coolest images that computer graphics in 1982 had to offer.

This is one of the images from Whitted's paper. In my opinion, the paper is pretty interesting and approachable. I've linked it in the references.

This image was interesting because it demonstrated several advancements over previous approaches. It has reflection, refraction, surface bump mapping, shadows, and a few other interesting pieces.

The major advancement that the paper puts forward is a model that uses a bounded depth of recursion to more accurately create a scene. In the results section of the paper, they mention that this took over an hour to create on a VAX computer.

--

Whitted's equation looks similar to Phong's but instead of calculating the specular component by direction, it is calculated by tracing a ray in the perfect reflection direction and adding the color that it produces. It also has a refraction component that works the same way.

We're going to work our way towards this equation tonight, but we definitely won't have time to get into refraction. Refraction gets complicated because refraction is modeled by Snell's law, in which the change of angle is related to the ratio of the densities of materials. This requires a good deal of bookkeeping to ensure that you know when you are inside a volume & refracting out or outside & refracting in.

--

Anti-aliasing: When you trace a single ray per pixel, you get an image quickly, but you will get sharp artifacts because a pixel will have to be entirely a single color. You can raise the resolution, but this means storing more pixel data. Instead, you can cast many rays per pixel and combine their colors to get a smoother effect. You can do this as four pixel corners and the center, as n random rays inside a pixel, or various other approaches.

Textures: You can map a 2d texture onto a sphere such that whenever a point is inside that texture, you pull the color from the image rather than the default color. This is easier on planes, but can be done a number of ways on other surfaces.

Refraction: Add the density bookkeeping and the refraction equation and you should get cool refraction effects. If you are going to make complex shapes (like cubes) that don't have well-defined volumes, this can have fun edge cases.

Triangle Meshes + 3D Modeling: Blender can output a variety of formats that you can then read in to your program. They include a large amount of data, but you can pick and choose what you incorporate. This is fun because it lets you more easily create your own scenes.

Toon Shading: Once you set up a physical model, you don't have to restrict it to realism. For cartoon shading, you can do a lambertian model with banded colors. You can get border lines by rendering for depth and for surface normal and then post-processing those with a Sobel filter.

Animation: If you render a series of images, you can use an image library to create a gif. Keyframe animation is where you have specific frames (position, orientation, etc.) and ways to interpolate between them. You can run a physics simulation with gravity, friction, springs, and other forces and use the output data to generate your scenes.

360 Degree Camera: Why limit yourself to a simple 2D camera. You can create a camera that can trace rays in all directions and then transform the results down to a 2D image using a mapping function. You can also do things like give your camera lens effects like depth of field and foreshortening.

--

The paper I mentioned. Short, approachable, has pictures. What's not to like?

Ray tracing in one weekend is a book that is available for free online. In it, they go through a simple path tracer.

This is one of the books that I brought. It is the textbook that I used in my computer graphics class. I <3 books.

--

Thanks for coming! Any questions?

--
