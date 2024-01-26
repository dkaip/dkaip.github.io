---
layout: default
title: On 3D Graphics
parent: My Thoughts
---

# 3D Graphics for the Ignorant
{: .no_toc }

Welcome to my thoughts on 3D computer graphics. Here I hope to be able
to give you some vital information as you start your journey into the
world of 3D computer graphics.
{: .fs-5 .fw-300 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Why am I writing this?

I am an experienced software engineer with many
years of experience with creating some not so simple software systems.  I
decided that I needed to implement some 3D graphics in order to help me
to visualize what is going on in an automated material handling system
that I am working on. This material handling system is currently
operating in a virtual environment, thus, the need for visualization.
{: .fs-5 .fw-300 }

Well, I went out to the WEB and got smacked in the face with a huge amount
of information.  Most of which is geared for people who
already know something about it.  Yes, I now know much more about it, but,
it was a pretty painful process. I am writing this because I hope to impart
some important information that will help you to come up to speed on this
faster than I did.
{: .fs-5 .fw-300 }

## Introduction

 Here is that vital information that I mentioned above:
{: .fs-5 .fw-300 }

**3D computer graphics is about vertices and color!**

That is it, thank
you for coming and don't forget to tip your waitresses.  Seriously,
vertices and color are pretty much all that it is about.  Unfortunately,
both vertices and color are pretty much a universe unto themselves.
Hopefully when you are finished here you will have enough information
to be able to use the other resources available on the WEB without
feeling like you are climbing a cliff face.
{: .fs-5 .fw-300 }

## Vertices

What is a vertex? In the context of 3D graphics, it is a point in 3D space
or a 3D coordinate system. Uh, what is 3D space? Refer to
[Coordinate System](#coordinate-system) for more information.
Being called a vertex pretty much implies that there are other vertices associated
with this one. If a vertex is not associated with
any other vertices it would most likely be called just a &quot;point&quot;.
{: .fs-5 .fw-300 }

### A Vertex versus a Point

I'm still not getting it. Consider firing a shotgun loaded with bird shot. You
might think of the individual pellets of shot as points. These points are all independent
of one another and will all have their own &quot;destiny&quot;.  Some will continue going
mostly straight while others may hit things and veer off in other directions.  The point
here is that you would track, model, simulate, these points separate from each other.
{: .fs-5 .fw-300 }

Now, consider a square. It has 4 corners. In modeling
this square in computer graphics it would have 4 vertices, with each vertex representing
one of its 4 corners.  You would always keep track of these vertices as belonging together
because together they may be used to form a specific shape.  A square is a two dimensional(2D) shape.
For a three dimensional(3D) object you might
think of a cube. A cube has 8 vertices each vertex represents
a corner of the cube. When you want to &quot;draw&quot; this cube on the screen
you will need to submit these vertices to the drawing software so that it can use them to
produce an image on the screen. The 8 vertices actually are the vertices of
the 6 squares that form the 6 sides of the cube.
Don't worry, it is **way** more complicated than
this. At this point though, just take to heart the fact that you will need to keep
track of these vertices as belonging to this specific object, in this case
a cube.
{: .fs-5 .fw-300 }

Keep in mind that neither a point nor a vertex has any volume. It just represents a
location in space.  When you use your drawing software to draw points you are effectively
telling it to draw a little circle or sphere of a specified size and color at a particular
location.
{: .fs-5 .fw-300 }

### Just Vertices

A cube is a pretty simple object in terms of 3D modelling. As mentioned above it may
be thought of as the intersection of the 6 squares that form each of its 6 faces.
Hmmm, let's think on this a minute and let us assume that the cube is a 1 unit cube.
The unit is arbitrary at this point. It could be an inch, a foot, a centimeter, etc.
The coordinates of the vertices could be:
{: .fs-5 .fw-300 }

- [-0.5 -0.5 -0.5]
- [-0.5  0.5 -0.5]
- [ 0.5  0.5 -0.5]
- [ 0.5 -0.5 -0.5]
- [-0.5 -0.5  0.5]
- [-0.5  0.5  0.5]
- [ 0.5  0.5  0.5]
- [ 0.5 -0.5  0.5]
{: .fs-5 .fw-300 }

This will give us a 1 unit cube that is centered at the origin
([0 0 0]) of our world.
Consider the following cube:
{: .fs-5 .fw-300 }

- [-99.5 -99.5 -99.5]
- [-99.5  100.5 -99.5]
- [ 100.5  100.5 -99.5]
- [ 100.5 -99.5 -99.5]
- [-99.5 -99.5  100.5]
- [-99.5  100.5  100.5]
- [ 100.5  100.5  100.5]
- [ 100.5 -99.5  100.5]
{: .fs-5 .fw-300 }

This will give us a 1 unit cube that is centered at [100 100 100] in
our world.
{: .fs-5 .fw-300 }

Okay, now consider the situation where you might have hundreds
of cubes.  Think of the amount of work it will be to generate the vertices
for all of those cubes in our world. Now consider that many of the objects
in a 3D world may be made up of tens to many thousands of vertices each, or more.
In addition consider the prospect of having to move the objects around
in the 3D space.  As you might expect this become practically unmanageable
very quickly.
{: .fs-5 .fw-300 }

### A Transformation?

You may relax, a little bit, this is not how things are done except in the
most trivial of cases. In the normal case the objects to be displayed in
our &quot;world&quot; are created in something called model space. Model
space is another 3D coordinate system that a model is built in. Models
are typically created centered around the origin [0 0 0] of the coordinate
system they are created in.  Our first set of vertices above are an example of this.
They are centered around the origin of the model space. Okay, so what.
Now, consider our world space and the two
cubes mentioned above. Let's say we want a cube in our world centered at
[100 100 100]. In this situation we take our first model (its vertices)
that is in model space and perform a translation, rotation, and scale operation
on it and it will appear in our world centered at the position [100 100 100].
If we want another cube in our world centered at the origin we take
our model (its vertices) in model space and perform a translation, rotation,
and scale operation on it as well. In this case, the translation, rotation,
and scaling, do not cause any changes to the values of the vertices because
world space maps directly into model space. If we want to
move one of the cubes we just perform a different translation, rotation,
and scale operation on the original set of vertices.
{: .fs-5 .fw-300 }

What is going on here? We have one model, in this case, representing the
vertices of a cube.  We have the two translation, rotation, and scaling
operations that place cubes where we want them in our world space. The
translation, rotation, and scaling operations are represented in what is
called a transformation matrix. In simple terms, we are using the transformation
matrix on all of the vertices in the model to generate new values for the vertices.
These new values are sent to the drawing software and the cube appears where we want it.
The original vertices of the model are not changed. If we have a bunch of cubes
all we would have is one model and a bunch of translation matrices that would
place the cubes in the desired positions in the world. These desired
positions (called translations) are not the only thing that can happen. Using the transform matrices we can change the size of the cubes and their rotations (orientations) as well.
{: .fs-5 .fw-300 }

### More Transformations?

Wow, that is a lot of transformation.  You ain't seen nothing yet.
{: .fs-5 .fw-300 }

Consider that you are modelling your house in a 3D virtual environment.
You have been doing a lot of work creating, or buying, a lot of models. Chairs, tables, dressers, TVs, stereo equipment, ovens,
ranges, refrigerator, etc., you get the idea.  In addition, you have been creating
the walls, doorways, cabinets, rooms, closets, sinks, toilets, etc. Now that you have all of the objects that are required it is time to place the objects within your home.
How are you going to do it?
What is the reference point you are going to use in placing the objects within your
home?  Are you going to use the front door, the kitchen sink, the reference datum that
surveyors used to determine your property line?
{: .fs-5 .fw-300 }

If you were doing this in the real world
you would probably just choose some point in the current room and set things up in
relation to that, for example it might be a window or perhaps the door to enter the
room.  In this case you would just look at the room and decide where to
put stuff. In your virtual 3D world that is not good enough. You will need to measure
from some fixed point in the room and determine the exact coordinates where you want
to place the model.  Ultimately, you would do this by creating an additional transformation matrix that can be applied to any previous ones that may be in effect for the model
you are working with.  Great, with a bit of work you get all of the remaining
objects (models) placed in your room.
{: .fs-5 .fw-300 }

What about the rest of the rooms in the house? You would basically just do the same thing.
Select a position in the room that will be chosen as the origin for the objects in that room and create the appropriate transformation matrices for all of these items in order to
place them where you want them. Finally finished! Time to enjoy the 3D model of my house.
Well..., no not yet.
{: .fs-5 .fw-300 }

How are you going to look at your house?  Unless you are &quot;on the grid&quot; like
in the TRON movies you cannot see your house or the elements within it.  Your house
and its contents only exist in the 3D virtual world that you have created in the
form of a number of collections of related vertices. You will need
to create a virtual camera of sorts, place it in your world, and display what it
is &quot;looking at&quot; on your computer's screen.  The first thing to figure out
is where the camera is located in your virtual world and which way it is looking.
There is more information you need, but, we are not going going to worry about
that now.
{: .fs-5 .fw-300 }

Okay, we are going to put the camera in the kitchen and have it look at the
breakfast table.  On the table, there are place settings with flatware, napkins,
plates, etc.  Well, the place setting were arranged by using the center of the
surface of the table as their origin.  (It makes sense to do that because
they are evenly spaced around the outer edge of the table.) What do we do?
The camera cannot &quot;see&quot; the place settings because they are not in
the same space.  We need to transform the vertices for the place settings
into the current space that the camera is in.  We do this by, yep you guessed it,
applying the appropriate transformation to the place settings to bring them
into the same space that the camera it in.
{: .fs-5 .fw-300 }

I hate to be the one to break this to you, but, we are not finished with
transformations yet.  We placed the camera in a space, in this case the
kitchen space, and pointed it in a direction.  What I failed to mention
was that the camera was on a tall tripod and, me being a ding dong, I
managed to mount the camera to the tripod upside down.  Also the camera
is pointing down (well up to it) a bit because the kitchen table that I am looking at
is nowhere near as tall as the tripod.  Welcome to camera space!
{: .fs-5 .fw-300 }

Camera space has a specific shape associated with it. It is called a frustum.
You will need to look this one up since its explanation is to much for me to
go into here.  In short, it is shaped a bit like the great pyramid in Giza.
The bottom and top are flat and parallel.  Camera space basically defines
a volume that contains objects that are visible to the camera.  You have the
near plane (think top of the pyramid when looking down on it from above) and
the far plane (think bottom of the pyramid when looking down on it from above)
and the four sides.
If the objects are not contained (at least partially) within this volume, defined
by the top, bottom, and four sides, the camera does not consider them visible.
{: .fs-5 .fw-300 }

The next transformation that needs to take place is the transformation into
a canonical view volume.  The canonical view volume is a cubic volume that is
2 units on a side.  The X, Y, and Z coordinates together
bound the space between -1.0 to 1.0 on all three axes. Notice that these
are floating point values and opposed to possibly integer values in the
previous transformations. Whoa! What just happened here. In short the
pyramid shaped volume was transformed into a cubic shaped volume.
{: .fs-5 .fw-300 }

In addition, another transformation has taken place in this transformation to
conical view volume.  It is called the projection
transformation.  There are two main type of projection transformation that
are typically used in 3D graphics. One is an orthographic transformation and
the other is a perspective transformation.  There are plenty of explanations of
what these are on the WEB. Both types of projection transformations have their
uses, but, probably in most cases a perspective transformation it used.  To
get an idea of what a perspective transformation does try getting a couple
of identical glasses from you cupboard. Sit at your table and place one directly
in front of you on the table and the other at arms length in front of you on
the table. You will notice that the one that is further away appears to be
smaller to you. You know they are the same size, but, still the one further
away looks smaller. This normal phenomenon in our daily lives is what the
perspective projection seeks to replicate.
{: .fs-5 .fw-300 }

There is one last transformation that needs to occur.  That is the transformation
into viewport space. In other words this is a translation from the 3D canonical
view volume onto a two dimensional window on your computer screen.
{: .fs-5 .fw-300 }

A very important point to take away from this is that **the order of
applying transformations matters**.  You need to apply them from the
model towards the camera.
{: .fs-5 .fw-300 }

### Enough with the ^*&*&@#% Transformations Already

Just a little more and I'll stop.  Think about writing the software for
a 3D game of chess or perhaps checkers. What would you have? Well, for
checkers it is pretty easy. You would have a model for a playing piece
and one for the playing surface. For checkers you could get away with
having only one playing piece model since the player's pieces are typically
differentiated only by color.  For chess, you would need a small number of
models for each type of piece in addition to the model for the playing surface.
Since for both games the &quot;world&quot; is very simple you could easily
get away with only having model space and world space in terms of your 3D
application.  Yes, you will still need the transformations between your camera
and your viewport, but, your world manipulation will be very simple. Basically
you would only need to perform a model space to world space transformation
for all of the pieces. Changing the individual model space to world space
transforms as needed when the pieces are moved around on the playing surface.
{: .fs-5 .fw-300 }

Now consider a space game or perhaps a simulation. In this situation you might have
the solar system moving through the galaxy. You would have the planet(s) orbiting
around their sun and any moons for the planets would be orbiting around their
respective planets.  If you had a spaceship in orbit around a planet it would make
sense for the planet to be the center of the coordinate system, at least in terms
of calculating the spaceship's position over time.  If the camera were inside the
spaceship then the coordinate system for the camera might be centered within
the spaceship.  If the camera were looking out the window towards the planet then
the visible parts of the planet would need to be translated into the local coordinate
system for a proper view. If the spaceship were to leave orbit and head out to orbit the
planet's moon then it would make sense to reset the base coordinate system, for at
least position calculations. In this example it is quite obvious that numerous
coordinate systems are going to be required if for no other reason than to simplify
the math required for object location over time.
{: .fs-5 .fw-300 }

### Some of the rest of the story

I have only provided a cursory overview of some of concepts involving
vertices that are associated with creating a 3D computer graphics application.
There are a couple more things that I want to mention.
{: .fs-5 .fw-300 }

#### Triangles Everywhere

In the examples above I used a square (that was defined by 4 vertices) and
a cube (defined by 8 vertices). Much of the software that will be used in the
creation and manipulation of 3D models will only accept triangles when asked
to &quot;render&quot; an image to a display screen. So what does that mean? It means
that for a square we will need to represent it using 6 vertices instead of
4.
{: .fs-5 .fw-300 }

Picture a square on the desk in front of you. We'll call the upper left corner
vertex 1, the lower left corner vertex 2, the upper right corner vertex 3
and the lower right corner vertex 4. We'll define the coordinates of the
vertices as follows:
{: .fs-5 .fw-300 }

- [-0.5 0.5] Vertex 1
- [-0.5 -0.5] Vertex 2
- [0.5 -0.5] Vertex 3
- [0.5 -0.5] Vertex 4
{: .fs-5 .fw-300 }

If we want some graphics software to display this on a screen we will have to
send the vertices as follows:
{: .fs-5 .fw-300 }

- [-0.5 0.5, 0.5 -0.5, -0.5 -0.5] Vertex 1, Vertex 3, Vertex 2
- [-0.5 -0.5, 0.5 -0.5, 0.5 -0.5] Vertex 2, Vertex 3, Vertex 4
{: .fs-5 .fw-300 }

This will cause the graphics software to render two triangle on the screen.
Their combined shape, however, will produce the square we wanted.
{: .fs-5 .fw-300 }

This sounds crazy! Yes it does, but, there are very good reasons it is done this way.
Perhaps the most important reason is that the vertices of a triangle are **ALWAYS**
on the same plane. This is, at the moment anyway, needed for the computation of
the color of (or perhaps better the color on) the triangle, more on this later. In order to draw the cube mentioned
above we will need to send 12 sets of vertices; 2 for each square that makes up
each cube face. Complex 3D scenes may be made up of thousands to many thousands of
triangles. For scenes that are not generated in real time the triangle count may be
way higher.
{: .fs-5 .fw-300 }

#### Vertex order matters

In the example immediately above look at the order of the vertices I specified.
Vertex 1, Vertex 3, and Vertex 2 for the first triangle and Vertex 2, Vertex 3, and Vertex 4
for the second triangle. If you trace these sets of vertices out with your finger you will
notice that both of them, when traced, cause your finder to trace a path in the
clockwise direction. This order is sometimes called the winding direction. This
winding direction or winding order is used in order to determine the front face
of the triangle. This is important because many times back side culling is used to
not render the triangles faces that are not &quot;visible&quot;.
Only on rare occasions do you want to waste effort dealing with the surfaces on the inside
of a cube or any other model for that matter. You will need to know this winding order
when you are submitting sets of vertices to be rendered / drawn. Depending on the
software you are using it may be either clockwise or counterclockwise. It is even
possible that you will have a choice and be able to specify it yourself.
{: .fs-5 .fw-300 }

#### Organization of Vertex Sets

I will only mention it here, but, there are several different ways to send
vertices to your software. In the example above we sent 6 vertices 
3 for each triangle to our software. In this example we don't care, but,
the way we did it was not the most efficient way. Two of the vertices
are shared by both triangles. But, we sent them twice. This case
is trivial, but, if we were dealing with a complex shape with many
thousands of triangles there could be significant savings by using
an alternate method to send our vertices.
{: .fs-5 .fw-300 }

### Some last things for Vertices

In this text we have, for the most part, talked about groups of vertices in
terms of representing shapes. It is also possible for the vertices just
to represent points or lines.
{: .fs-5 .fw-300 }

Just a side note: One of the reasons to have a model's vertices centered about the
origin is so that rotations applied to the model have the desired effect.  Usually,
when rotating an object we tend to think about rotating it about its center. In
the case of the cube it might not produce the desired result if rotated about
one of the corner vertices.
{: .fs-5 .fw-300 }

There are a number of software packages available that are suitable for making
3D models.  These models may be exported and then used as the vertices for an
object that you want in your world. You need to keep in mind though that when
the models are &quot;exported&quot; they are written out to a file on the file
system. In order to import and actually use the model you will either need to export
the model in a format that your 3D graphics software has a loader for or you
will need to write the loader yourself.
{: .fs-5 .fw-300 }

Blender&reg; is one such package. Look it up.
{: .fs-5 .fw-300 }

## Color

Working on it!
{: .fs-5 .fw-300 }

## A Great Resource

Fairly recently, I stumbled on a YouTube&reg; resource that I wish I would have
had quite a few years ago. Go on to YouTube and search for &quot;**Cem Yuksel**&quot;.
Once you get to his page check out his Playlists. One of them is called
&quot;Introduction to Computer Graphics&quot;. Watch the videos in that playlist.
If you want more information watch the videos in the playlist
&quot;Interactive Computer Graphics&quot;. These are invaluable resources
if you are just starting out in 3D computer graphics.
{: .fs-5 .fw-300 }

## Glossary

This glossary is just a container for more information on specific concepts.
{: .fs-5 .fw-300 }

### Coordinate System

When thinking about a point in space you really need to ask yourself,
&quot;relative to what?&quot;.  Relative to you? Relative to the burger
joint? Relative to your Mom's house? When talking in terms of
computer graphics it is almost always relative to the origin of the
coordinate system you are currently working within.
{: .fs-5 .fw-300 }

Think about a piece of normal (the lines in the horizontal and vertical directions
are equidistant and perpendicular to each other) graph paper.  In this case you
would normally think of the **X** direction and increasing to the right and
the **Y** direction increasing up. With this piece of graph paper it is easy
to visualize a 2D Cartesian (rectangular) coordinate system with the origin (X = 0 and Y = 0)
at the lower left hand side of the grids.  In this 2D environment you could have
a vertex at X=10 and Y=5, or in a vector notation [10 5]. You could take your
pencil and make a mark counting (from the lower left corner of the intersecting
horizontal and vertical lines) 10 vertical lines to the right and 5 horizontal
lines up. In looking at the mark you just made you would be looking at a
single vertex. In the case where that vertex is not associated with any other
vertices you would typically call it a point instead of a vertex.
In this simple example we placed the origin (the point where X=0 and Y=0) at the
lower left side of the graph paper. It would be a very normal thing to pick
a spot in the center of the graph paper and call it the origin instead.  In
this case it would be easy to mark points that had negative X and or negative
Y values.
{: .fs-5 .fw-300 }

This simple example used a 2D coordinate system. When working with 3D graphics
it is normal for everything to have a third dimension as well. In this situation
the coordinates of a vertex would have an **X** component, a **Y** components, and
a **Z** component. The location of the vertex could be portrayed as X=10, Y=5,
z=5, or in vector notation [10 5 5].
{: .fs-5 .fw-300 }

When working with 3D coordinate systems you need to know and understand the
&quot;handedness&quot; you are operating in. A 3D coordinate system is either right
handed or left handed.  Consider looking at a computer screen in front of you.
It is a right handed system if the X direction increases to the right, the Y
direction increases going up and the Z direction increases coming out of the screen
towards you.  If the Z direction increases going away from you it is a left handed
coordinate system.  Search the WEB for right-hand rule for more information.
{: .fs-5 .fw-300 }
