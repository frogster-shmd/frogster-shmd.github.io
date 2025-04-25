---
layout: post
---

### Monitor Stand Part 1
Currently the monitor on my desk at home resides on a stack of four books. Which has worked well since the inception of my desk five years ago, but I have always wondered what it would be like to have a stand that allowed for useable space below my monitor.


![Desk image 1](/assets/img/desk_image_1.jpg)


I thought it would be nice to have a space where my keyboard could be put while not in use. Or maybe the ability to have some pen holders attached to the stand itself. This is where the idea intially came for a monitor stand.

#### Ideation
The current idea is to create the side pieces for the monitor stand via 3D printing, and to have the platform where the monitor will sit be a plank/planks of wood. I am hopeful that the use of both 3D printed sides and wood will give the stand a look that matches the desk as a whole.


![Monitor stand sketch 1](/assets/img/monitor_stand_sketch_1.jpg)


This is just a rough drawing of the stand. I think that I can either have a singular plank of wood span across or multiple planks with minimal spacing between them.

The repository with scripts for this project can be found [here](https://github.com/frogster-shmd/monitor_stand)

#### Tools
The printer that I am going to use for this is a Prusa MK4s and the CAD software is FreeCAD. The reason for choosing FreeCAD is that it is one.. free and two, it looked similar enough to solidworks, which I have taken a course in previously, to feel confident I would get the hang of it. A bit more about FreeCAD will come later.

#### Test 1 Basic Hexagons With Lofts
For the 3D printed sides I knew that I wanted to have a pattern that would fill in the inside of the part. I thought that hexagons would look nice giving it both strength and a classic geometric look. I ran this idea past a friend and they asked about adding depth to the hexagon. We decided that we could add depth to the hexagon be creating a slope on the interior of the hexagons that would rise in an outward angle from the bottom to the top of the hexagon.


![Basic hexagon test 1](/assets/img/basic_hexagon_test_1.png)
![Basic hexagon test 2](/assets/img/basic_hexagon_test_2.jpg)


This was just an intial print getting an idea for how FreeCAD works. It was also a test for what was possible from my printer to begin with. This is the first print that was printed by something I created myself. The way that I created it was in the 2D space that is, a bottom sketch of the lines for my bottom hexagon. Then created another 2D sketch above the first at the given thickness that I wanted. Then lofted the sketch on the bottom to the sketch on the top.

I liked the depth that this print created, obviously my sizing was off, but it gave me confidence that I was on the right track.

Creating this print also raised some questions namely:
- How was I going to create the hexagon pattern?
- I only wanted the inside of the hexagon to have a slope, but this print has the slope both internally and externally. How would I solve that?
- Was there a different way to create this hexagon?

#### FreeCAD
After the questions that arose from the first print I realized I needed to understand FreeCAD a bit more.

**2D vs 3D Paradigm**\
Coming from a more classic style of learning CAD modeling I was taught that you imagine the part in a 2D paradigm. You sketch in the 2D space, and then extrude/loft your given 2D sketches to create a more complex shape.

What I was unaware of was a second paradigm which is designing in the 3D space. In this paradigm you build parts as a combination of simpler 3D shapes. Learning about this made me want to try to experiment with this paradigm.

**Python and FreeCAD**\
FreeCAD also allows for you to use a Python console/scripting/marcos for creating parts in its application. This [resource](https://wiki.freecad.org/Manual:A_gentle_introduction) is a great place to start learning about how to use Python in FreeCAD. It also helps you learn how to create objects in the 3D paradigm.

The combination of both Python and the 3D paradigm sent me in a much different direction than I had intially thought when doing test 1.

#### Test 2 Seven Hexagons
**Firstly**\
A huge thank you goes to this resource right [here](https://www.redblobgames.com/grids/hexagons). This is practically a bible for working with hexagons. It was particularly helpful for me in calculating the distance between hexagons.

For this test I wanted to use Python in some way when creating the part and I wanted multiple hexagons to be in a pattern on a square.

**Plan**
1. To create the depth that I wanted the bottom size of the hexagon needed to be smaller than the top.
2. I wanted there to be space between the hexagons themselves. Using the above resource I calculated the distance to the other hexagons based on a larger size hexagon compared to the other two hexagons. This gave me the hexagon pattern of other hexagons.
3. After creating these 7 3D hexagon solids, I would cut them out of a solid cube of the proper size and thickness and be left with the look I was going for.


![Seven hexagons 1](/assets/img/seven_hexagons_1.png)
![Seven hexagons 2](/assets/img/seven_hexagons_2.jpg)


I was pleased with the depth of the first print, but it was a bit too dramatic. I also didn't like that only one side of the print had depth. I realized that you would be able to see that internal face, and with no depth it would just be a flat hexagon which I didn't want.

So I decided to just halve the thickness in my script, and then mirror the part from the GUI so that I had depth on both inside and outside.


![Seven hexagons 3](/assets/img/seven_hexagons_3.png)
![Seven hexagons 4](/assets/img/seven_hexagons_4.jpg)


For this print I ran out of my sample fillament that I was given so I had to switch in the middle of the print and then subsequently ran out of that fillament as well, but it gave me the idea that the mirroring and the sizing was getting closer to what it is that I wanted.

#### Test 3 Patterning Hexagons
Up until this point I had been using my own calculated origins for the 7 hexagons based on the `distance` variable that I specified, but that wasn't going to cut it. I knew I needed a more dynamic script to get the shape that I wanted.

**Plan**
1. I needed to know the shape that I wanted to fill with this pattern, and knowning the part was going to be relativley simple I used a FreeCAD bounding box.
2. From that box I knew that I wanted my pattern to be centered at the middle point of that box having the edges be cut off if need be.
3. I would fill this box with equally spaced hexagons, and then cut the hexagon shapes out to give me a similar shape to test 2.

![Pattern hexagons 1](/assets/img/pattern_hexagons_1.png)

My thought for filling the bounding box was this; create a list of origins from the center going right, while we are `<= x_max`, then we do the same going left while we are `>= x_min`. This gave us our first list centered at the origin. From there we can build our other rows based upon the row we created at the origin. From the hexagon bible I learned that basically every other row (odd number) shifts some amount as you go up. So when I got to an odd numbered row I would shift it in the `x` direction that was needed. Creating a full list for all the rows above the origin then below. The script for this is in the repository.

That process gives me the origins of my hexagons that I want. I then create my hexagons at those origins, and cut them from the shape of the cube/bounding box.

This is where I am currently at. I have not printed this shape, and don't think I will until I get a bit closer to knowing exactly how I want to have the wood for the stand implemented.

I also got cute and thought that I could loft between 3 wires in FreeCAD, basically the bottom, middle, and top hexagon wires. I was not aware that in FreeCAD that would give me a smooth shape which you can tell from the picture above. There is no sharp slope on the inside of the part rather it is a smooth transition. This might be a parameter that I can change when I create the loft I am not sure.

#### Future Steps
I need to decide if I want to keep going down this Python scripting route, or if I am going to go back to using the GUI for creating certain features.

I am also unsure if I will have a singular plank of wood or multiple. Also I am unsure how I want the wood to slide in. I am thinking that I want it to be tight when I slide the 3D printed sides onto the wood almost like an interference fit, but I am not sure how capable the printer is at creating this.

I am also sure that there is plenty of refactoring that I could do on my scripts. They are currently in a state that works, but working well and working are completley different states.
