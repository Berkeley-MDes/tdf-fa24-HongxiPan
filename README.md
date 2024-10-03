# Welcome to my workspace!
Welcome to Hongxi Pan's workspace! 

# Outline
[week 5](README.md#week-5)

[week 4](README.md#week-4)

[week 3](README.md#week-3)

[week 2](README.md#week-2)

[week 1](README.md#week-1)

---

# Week 5 #
## Week of 10/03/2024

This week:

### Monday - testing ###

<img width="850" alt="" src="assets/week5/1.png">

I struggled with how to compile my file the whole weekend, it usually took more than an hour and then I had to kill it. I tried several times but it not worked.

Then with the help from Upasana, I opened this:

<img width="850" alt="" src="assets/week5/6.png">

And then I could see every steps it took and the process took halp an hour. Then finally:

<img width="850" alt="" src="assets/week5/7.png">

<img width="850" alt="" src="assets/week5/2.png">

<img width="850" alt="" src="assets/week5/3.png">

The first two file worked well, so I decided to try the blinking one.
The compiling and flashing process were successful, and I connected the circuits, it was not working:

<img width="850" alt="" src="assets/week5/8.jpg">

So I checked the circuits, I realized the botton was not connected in the rcircuit. I fixed that, it still not worked.

Then I looked into the code, and I saw that:

<img width="850" alt="" src="assets/week5/4.png">

<img width="850" alt="" src="assets/week5/9.jpg">

Then I realized maybe I connected the wrong pin, so I fixed that.

And it started to blink and I thought the button was not working. That really confused me a lot. 

<img width="400" alt="" src="assets/2.gif"> <img width="400" alt="" src="assets/3.gif">

### Thursday - testing ###

1.I tried the LED color one, I tried to compiled the code:

<img width="600" alt="" src="assets/week5/10.png">

But some thing went wrong and the code couldn't work. Then I decided to ask chatGPT:

<img width="600" alt="" src="assets/week5/12.png">

Then I tried to changed the position of some lines of code, it worked:

<img width="600" alt="" src="assets/week5/11.png">

<img width="600" alt="" src="assets/week5/3.gif">

But then I realized that I could never reach Aqua (which is my favorite color), Red was the best I can do, so I decided to look a little bit more into what was wrong.

First I wanted to see if the led could not show aqua, so I changed the setting red into aqua and see if it worked.

<img width="600" alt="" src="assets/week5/15.png">

<img width="600" alt="" src="assets/week5/4.gif">

The led could show aqua. Then I thought since the aqua was the last stage, maybe the value couldn't reach that high, so I added the function to monitor the value of the sensor.

<img width="600" alt="" src="assets/week5/16.png">

It showed bash, so that meant my code had error. I tried modify th code several times, then it worked, which meant I can see the real time value of the sensor in the serial monitor(refresh each second).

<img width="600" alt="" src="assets/week5/5.gif">

It turned out that it was because the best value I could reach was 3000, which means that I could never see aqua if I stuck on the original setting. So this could be fix easily, I just needed to change the value range of each color and made them under 3000, then I have all the colors.






---

# Week 4 #
## Week of 09/26/2024

This week:

### 1.Interaction Ecosystems ###

<img width="850" alt="" src="assets/week4/1.jpg">

In this diagram, I list the interaction ecology of the output stage of the product design I'm working on, where the black text represents the function and action, and the grey represents the information being transported.

Throughout the process, my touchpoints are my laptop and iPad, processing and transferring information through web and desktop tools, and finally materialising the product through 3D printing.(As 3D printers are not personal devices, they are not listed among the touch points on the left side of the chart.)

---

# Week 3 #
## Week of 09/19/2024

This week:

### 1. work on project 1: Gh ###

video link: https://youtu.be/iMBRTu1CeQQ

I decided to challenge myself to axolotl level and start a GH project myself. I've always had a passion for jewellery design and I want to do it this time.

I found this very nice ring with hammered pattern on its surface. So I decided to make one myself. I did a little research on how these beautiful hammered patterns are created. They are hammered by these metal sticks with round balls on the end. When they are hammered, they create a recessed texture on the surface of the ring.

<img width="400" alt="phone stand" src="assets/week3/11.jpg">
<img width="400" alt="phone stand" src="assets/week3/22.jpg">

So my making process is clear now. First I create a cylinder, then I make it into a solid ring. I can also create some spheres around it and use Boolean to subtract their overlap with the ring.

<img width="800" alt="phone stand" src="assets/week3/19.png">

At first, I wanted to create two surfaces (inner and outer surfaces) and then merged them togeter into a solid. But I failed. I could not cap it.

<img width="800" alt="phone stand" src="assets/week3/10.png">

Then I tried to make a ring first. And it worked.

Since rings come in different sizes, I want users to be able to adjust the size, height, and thickness of the ring using these sliders.

<img width="800" alt="phone stand" src="assets/week3/6.png">

Then I created another cylinder and used population geometry to randomly generate points on the surface. I can also control the number of these points. Then I generated spheres based on these points and adjust the sizes of these spheres and make sure they look good. 

<img width="800" alt="phone stand" src="assets/week3/2.png">

<img width="800" alt="phone stand" src="assets/week3/3.png">

<img width="800" alt="phone stand" src="assets/week3/4.png">

<img width="800" alt="phone stand" src="assets/week3/5.png">

And also The reason why I did not use the outer surface of the ring to generate these points is because the spheres use the points as centers, so the texture will be too deep in that case. I also connected the thickness parameters, the radius of the points surface and the size of the spheres to make sure the ring is at least 1 millimeter thick.

<img width="800" alt="phone stand" src="assets/week3/11.png">

Then I merged these spheres into a solid and used it as a tool to cut the ring. 

<img width="800" alt="phone stand" src="assets/week3/8.png">

<img width="800" alt="phone stand" src="assets/week3/9.png">

<img width="800" alt="phone stand" src="assets/week3/14.png">

Then here is the pattern. 

<img width="800" alt="phone stand" src="assets/week3/18.png">

Based on this process I created, I made a series of rings with different sizes, heights and textures for my friends.

<img width="800" alt="phone stand" src="assets/week3/9181.png">


### 2. work on project 1: manufacturing  ###

I started my manufacturing process with a single ring because I wasn't sure if the printer would be able to achieve these subtle patterns. But it worked pretty well. 

<img width="800" alt="phone stand" src="assets/week3/21.png">

Then I printed the whole series of my rings. 

<img width="800" alt="phone stand" src="assets/week3/22.png">

<img width="800" alt="phone stand" src="assets/week3/36.png">

I think it will be better to print them with metal, but this is good enough.


### 3. work on project 1: reflection  ###

I think one of the most interesting things about this project is that I tried to simulate handmade textures through parametric design and still keep some of their original characteristics, like the randomness of the patterns. I think this makes this project Axolotl level. 

I think that using computational tools in the design process allows me to think more rationally about artifacts and gives me a deeper understanding of the logic of making.:)

---

# Week 2 #
## Week of 09/12/2024

This week:

### 1. diagram of the workflow in GH ###

<img width="800" alt="phone stand" src="assets/week2/gh -phone stand.png">

I tried to figure out every step in the sample file, took screenshots of them and made the images into a flowchart. I think it is clear for me to understand how it works.

### 2. play with the sample file ###

Monday

<img width="800" alt="2" src="assets/week2/21.png">

I tried to want to show all the invisible forms to figure out how the final shape was formed. (There are also some images about this in diagram)

<img width="800" alt="1" src="assets/week2/1.png">

<img width="800" alt="3" src="assets/week2/3.png">

After trying to adjust all the sliders, I tried changing the shape of the base for the first time. But unfortunately this time it failed because the cylindrical shape is just a surface not a solid.

Thursday

<img width="800" alt="4" src="assets/week2/4.png">

This time I decided to change the base to a cube. It didn't look very successful at first though.

<img width="800" alt="5" src="assets/week2/5.png">

Then I realized that it was the cutting tool that was so big that it was cutting almost all of the cube off, so I adjusted some parameters.

<img width="800" alt="6" src="assets/week2/6.png">

The center of gravity and assembly didn't look too good, so I adjusted the position of the base, and the size of the cut out section.

<img width="800" alt="7" src="assets/week2/7.png">

<img width="800" alt="8" src="assets/week2/8.png">

Finally green. But I didn't like the idea of a round hole in the center in the middle of a cube, so I changed it to a square.

Looks good. And then I baked it. (Why does it look so much like an ashtray?)

<img width="800" alt="" src="assets/week2/9.png">

### 3. 3D Printing ###

I tried 3D printing this week. The first attempt wasn't very successful, and Cody threw it in the trash.

<img width="800" alt="0" src="assets/week2/heart2.jpg">

But neither of us were quite sure what the problem was, so Cody helped me print it on a different machine.

<img width="800" alt="0" src="assets/week2/heart1.jpg">

And it worked. Many thanks to Cody.

---

# Week 1 #
## Week of 09/05/2024

This week:
1. Learning how to use github, still learning how to write markdowns (like right now, hopefully I'm doing it right).

2. Got my maker pass, and still working on the laser cutter training.

3. Watched some rhino and grasshopper tutorials.

Fun stuff:

https://adam-6zhapuwfc-adam-f5567005.vercel.app/

This is a AI+CAD tool made by guys from cohort 3, and it's really cool to generate simple models from text, but that's what happens when it deals with complex semantics or relatively organic objects:

It can make a fish:

<img width="600" alt="It thinks this is a fish" src="assets/week1/fish.png">

Also, this is a dog:

<img width="600" alt="Also, this is a dog" src="assets/week1/dog.png">

The most interesting thing is that it can also do design:

<img width="600" alt="Interestingly, it can do design as well" src="assets/week1/design.png">

---

# Github Background Information & Context
If you’re new to GitHub, you can think of this as a shared file space (like a Google Drive folder, or a like a USB drive that’s hosted online.) 

This is your space to store project files, videos, PDFs, notes, images, etc., and (hopefully, neatly) organize so it's easy for viewers (and you!) to navigate. That said, it’s super easy for you to share any file or folder with us (your TDF instructional team) - just send us the link!  As a start, feel free to simply add images to the `/assets` folder, which is located [here](/assets). 

The specific file that I’m typing into right now is the **README.md** for this repo. 
##### (💡 TIP: The .md indicates that we’re using [Markdown formatting.](https://www.markdownguide.org/cheat-sheet/)) #####
<h6> (💡 TIP 2: GitHub Markdown supports <a href="https://gist.github.com/seanh/13a93686bf4c2cb16e658b3cf96807f2"> <em>HTML formatting</em> too, including emojis 😄</a>, in case that helps!) </h6>

### :star: Whatever you write in your **README.md** will show up on the “front page” of your GitHub repo. This is where we’ll be looking for your [weekly progress reports](https://github.com/Berkeley-MDes/24f-desinv-202/wiki/3.0-Weekly-Submissions#weekly-progress-report). They might look something like this: ###


# Week 1: Example Report 1 #
## Week of 09/05/2024

This week, I designed a cool phone stand made of rocks. Check out all my cool sketches and progress photos from this week below, etc., etc....

<img width="200" alt="Cool Phone Stand made of rocks" src="assets/exampleimg.png">

---

It's time to start making this space your own! If you want to save these instructions, make a copy.  Also, feel empowered to delete everything in this README.md and start documenting! 

Excited to work with you,
your TDF teaching team

PS: let us know if you have any questions!!

PPS: 

## Quick Links, compiled here for your convenience: ##

- [TDF Wiki](https://github.com/Berkeley-MDes/24f-desinv-202/wiki) - the ultimate source for truth and information about the course and assignments
- [Google Drive Folder](https://drive.google.com/drive/u/0/folders/1DJ1b6sSDwHXX6NRcQYt10ivyQSgU0ND6) - slides and other resources
- [bCourses](https://bcourses.berkeley.edu/courses/1537533) - where the grading happens
