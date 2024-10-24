# Welcome to my workspace!
Welcome to Hongxi Pan's workspace! 

# Outline
[week 8](README.md#week-8)

[week 7](README.md#week-7)

[week 6](README.md#week-6)

[week 5](README.md#week-5)

[week 4](README.md#week-4)

[week 3](README.md#week-3)

[week 2](README.md#week-2)

[week 1](README.md#week-1)

---

# Week 8 #
## Week of 10/24/2024

This week:

### Video ###

https://youtu.be/62e6_m3k8bE?feature=shared

### Merge the circut ###

I put two sensor circuits(photo cell and dial) together, linked them to a same photon, and drew a circuits diagram here.

<img width="850" alt="" src="assets/week8/1.png">

Also, the code I wrote before only sent 1 and 0 (yes or no)to the particle cloud, so I had to add question types into this system. The logic is: the user opens the book, chooses a question type, then closes the book and think about the question, open the book, and get a answer. So what my code is doing right now is to mark the question types as strings(1,2,3), and the certain string will be sent whenever the book is detected open. Here is the modified code (just for record as well):

```C++
#include "Particle.h"

int ledPin = D7; 
int photoResistorPin = A0; 
int potentiometerPin = A2;   
int yellowLEDPin = D0;         
int greenLEDPin = D1;         
int blueLEDPin = D2;

int threshold = 200;   
int lightLevelNew, lightLevelOld;

int threshold1 = 1365;  // Potentiometer value
int threshold2 = 2730;

int type = 0; // type 0 is closed, type 123 are question types   
int potentiometerValue;

void setup() {
    pinMode(ledPin, OUTPUT);
    Serial.begin(9600);
    while (!Serial) {}

    // Initialize the first light level reading
    lightLevelOld = analogRead(photoResistorPin);
    //Particle.subscribe("0a10aced202194944a05a510/Received");

    pinMode(yellowLEDPin, OUTPUT);
    pinMode(greenLEDPin, OUTPUT);
    pinMode(blueLEDPin, OUTPUT);

    // Set up potentiometer
    digitalWrite(yellowLEDPin, LOW);
    digitalWrite(greenLEDPin, LOW);
    digitalWrite(blueLEDPin, LOW);
    
}

void loop() {
    
    lightLevelOld = lightLevelNew;

    // potentiometerPin
    
    potentiometerValue = analogRead(potentiometerPin);

    if (0 < potentiometerValue && potentiometerValue < threshold1) {
        
        digitalWrite(greenLEDPin, LOW);
        digitalWrite(yellowLEDPin, HIGH);
        digitalWrite(blueLEDPin, HIGH);
        type = 1;
    
    } 
    else if (threshold1 < potentiometerValue && potentiometerValue < threshold2) {
        
        digitalWrite(greenLEDPin, HIGH);
        digitalWrite(yellowLEDPin, LOW);
        digitalWrite(blueLEDPin, HIGH);
        type = 2;
        
    } 
    else {
       
        digitalWrite(yellowLEDPin, HIGH);
        digitalWrite(greenLEDPin, HIGH);
        digitalWrite(blueLEDPin, LOW);
        type = 3;
        
     } 

    Serial.println(potentiometerValue);

     // light sensor
    lightLevelOld = lightLevelNew;
    lightLevelNew = analogRead(photoResistorPin);  // Read the current light level
   
    Serial.print("Current Light Level: ");
    Serial.println(lightLevelNew);
    
    if (lightLevelNew > lightLevelOld + threshold) {
        digitalWrite(ledPin, LOW);  

        if(type == 1 || type == 2 || type == 3){
            Particle.publish("Question type", String(type), PUBLIC);
            Serial.println(String(type));
        } 
    }

     else if (lightLevelNew < lightLevelOld - threshold ) {
        digitalWrite(ledPin, HIGH);  // Turn LED off

        Serial.println("LED is OFF");
        type = 0;
       
    }
   
    delay(1000);
}
```

---

# Week 7 #
## Week of 10/17/2024

This week:

### photo cell - CdS ###

<img width="850" alt="" src="assets/week7/1.jpg">

So, this is our part of the diagram for the whole system. 
What we are trying to do is: open the book, and it will send the message to particle cloud, which can be received by Nora(other team)'s Photon. The led will be used for debuging. 

I found two sensor maybe useful in our box, one is the cds, and the other is the HC-SR04(I don't konw it's name, but I call it distant sensor). Since the HC-SR04 can be influenced easily by the movement of the user, so I decided to try the CdS first.

After several attempts, I found that it was not easy to make everything just right:

<img width="850" alt="" src="assets/week7/2.png">

### 1. my computer: ### 
It runs really slow in vsCode and I don't know why. It took me a lot of time compiling those codes, which means it took me more time to debug:(

### solution: ### 
There is no solution. I have to live with it.

### 2. relative value: ### 
I know the sensor reads the relative values of environmental testing, but it is not stable at all. Since there is an error in each reading, it is not possible to use the absolute value of the change to make judgments.

<img width="850" alt="" src="assets/week7/7.png">

### solution: ### 
I used a threshold to make sure when the light changes greatly, the led lights up and sends a message. But it took a lot of experiments. And also, I added a resistor, trying to make it more stable.

<img width="850" alt="" src="assets/week7/9.jpg">
<img width="850" alt="" src="assets/week7/3.png">


### 3. Frequent messages: ### 
In the experiment, since the status of led was always detected as on after the book was opened, messages would be sent to cloud frequently, which means that requests to call the API would be sent frequently, leading to message stacking.

<img width="400" alt="" src="assets/week7/4.png"> <img width="400" alt="" src="assets/week7/5.png">

### solution: ### 
I want to keep this on/off after detection until the huge light change happens again. Therefore, the code needs to continuously detect changes in the environment (since I can't predict the interval time between two activities of the user is, I can't use delay to maintain the state).

### 4. the threshold: ### 
I had to calculate the threshold every time due to the change of multiple factors: environment, led status and the interaction with the physical book, which made the led blinking all the time.

### solution: ### 
I tried to add a hysteresis to prevent the LEDs from switching on and off frequently If the light changes above a certain threshold (positive increase or negative decrease), it will change the LED state. This prevents frequent flickering of the LED due to small light changes. But I am still working on this one.

<img width="400" alt="" src="assets/week7/4.png">

<img width="400" alt="" src="assets/week7/g1.gif">

Here is the code for now, just for record:

```C++
#include "Particle.h"

int ledPin = D7; 
int photoResistorPin = A0; 
int threshold = 300;   // Define a fixed threshold for light change
bool ledState = false;   // Track the current LED state
int lightLevelNew, lightLevelOld;

void setup() {
    pinMode(ledPin, OUTPUT);
    Serial.begin(9600);
    while (!Serial) {}
    
    // Initialize the first light level reading
    lightLevelOld = analogRead(photoResistorPin);
}

void loop() {
    lightLevelOld = lightLevelNew;
    lightLevelNew = analogRead(photoResistorPin);  // Read the current light level
    
    Serial.print("Current Light Level: ");
    Serial.println(lightLevelNew);
    
    // Check if the light level has increased significantly (by the threshold) and LED is off
    if (lightLevelNew > lightLevelOld + threshold && !ledState) {
        digitalWrite(ledPin, HIGH);  // Turn LED on
        ledState = true;  // Update the LED state
        Serial.println("LED is ON");
        Particle.publish("LED Status", "open", PRIVATE);
    } 
    // Check if the light level has decreased significantly and LED is on
    else if (lightLevelNew < lightLevelOld - threshold && ledState) {
        digitalWrite(ledPin, LOW);  // Turn LED off
        ledState = false;  // Update the LED state
        Serial.println("LED is OFF");
        Particle.publish("LED Status", "close", PRIVATE);
    }

    // Update the old light level to the new one for the next loop
    lightLevelOld = lightLevelNew;

    delay(1000);  // Wait 1 second before the next reading
}
```

---

# Week 6 #
## Week of 10/10/2024

This week:

### stemma 1 - MPU6050 ###

I tried out the MPU6050 sensor, I used the sample file but there are lots of problems:

<img width="850" alt="" src="assets/week6/1.png">

But it still worked, I could compile it and flash it:

<img width="850" alt="" src="assets/week6/2.png">

I tried to make the circuit based on the code given, I saw there was a led in this circuit, but I could not know what it did, so I asked ChatGPT and it told me that it was for indicating the status of the sensor:

### stemma 1 - MPU6050 ###

Then I opened the serial monitor, and I could see the value of the sensor:

<img width="850" alt="" src="assets/week6/1.gif">

<img width="850" alt="" src="assets/week6/3.png">

<img width="850" alt="" src="assets/week6/4.png">

But it was so fast that I could not follow it :( It took really long time to compile (more than 30 mins), so I looked a little bit into some interesting application of this sensor, I saw some really fun staff like this, but I think they used Arduino or other kind of boards. There is no reference for Fhoton :( So I am still trying and want to see what I can do with it.

<img width="850" alt="" src="assets/week6/2.gif">

### stemma 2 - APDS-9960 ###

I tried the second one, which had a lot of problems but still worked:

<img width="850" alt="" src="assets/week6/6.png">

Also, I saw a tutorial about this sensor: https://www.adafruit.com/product/3595, and here is a example using the color sensor:

<img width="850" alt="" src="assets/week6/7.png">

<img width="850" alt="" src="assets/week6/8.png">

My board worked perfectly, and you can see the value changed with different color:

<img width="850" alt="" src="assets/week6/3.gif">

I just realized that I didn't delete the code that detects proximity, so I can see both values in serial monitor.

### 2 - project 2 proposal ###

Our group is cooperating with Nora and Selena's group, so we discussed a system like this (There are more details in that proposal and I am pretty sure whoever is reading this report has the access to that):

<img width="850" alt="" src="assets/week6/9.png">

To be honest, I really hope we can do bigger projects with TDF in this semester. My classmates in this cohort are from various backgrounds and I am quite sure that they are super smart people and are very professional in their own field. But I think the way we work together now doesn't bring out the best in us, and I feel really bad about that.

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

So I checked the circuits, I realized the botton was not connected in the circuit. I fixed that, it still not worked.

Then I looked into the code, and I saw that:

<img width="850" alt="" src="assets/week5/4.png">

<img width="850" alt="" src="assets/week5/9.jpg">

Then I realized maybe I connected the wrong pin, so I fixed that.

And it started to blink and I thought the button was not working. That really confused me a lot. 

<img width="400" alt="" src="assets/2.gif"> <img width="400" alt="" src="assets/3.gif">

### Thursday - testing ###

1.I tried the LED color one, I tried to compiled the code:

<img width="850" alt="" src="assets/week5/10.png">

But some thing went wrong and the code couldn't work. Then I decided to ask chatGPT:

<img width="850" alt="" src="assets/week5/12.png">

Then I tried to changed the position of some lines of code, it worked:

<img width="850" alt="" src="assets/week5/11.png">

<img width="850" alt="" src="assets/week5/3.gif">

But then I realized that I could never reach Aqua (which is my favorite color), Red was the best I can do, so I decided to look a little bit more into what was wrong.

First I wanted to see if the led could not show aqua, so I changed the setting red into aqua and see if it worked.

<img width="850" alt="" src="assets/week5/15.png">

<img width="850" alt="" src="assets/week5/4.gif">

The led could show aqua. Then I thought since the aqua was the last stage, maybe the value couldn't reach that high, so I added the function to monitor the value of the sensor.

<img width="850" alt="" src="assets/week5/16.png">

It showed bash, so that meant my code had error. I tried modify th code several times, then it worked, which meant I can see the real time value of the sensor in the serial monitor(refresh each second).

<img width="850" alt="" src="assets/week5/5.gif">

It turned out that it was because the best value I could reach was 3000, which means that I could never see aqua if I stuck on the original setting. So this could be fix easily, I just needed to change the value range of each color and made them under 3000, then I have all the colors. Or maybe I can change a different resistor:)

I think what's different about these new projects is that the value they enter changes from yes and no (0 or 1) to a variable value. And these values can represent any of the variables in the entire ecosystem, for example, it can be the number of the pictures that I pinned in Pinterest just now and it can indicate my level of creativity; or it can present the the humidity of air and then use AI to provide a prediction of when I should water my plants. 

Take the creativity ecosystem I demonstrated last week as an example, I think machine learning can really help with the prediction of my creativity status. Like the Pintrest using machine learning to make sure I am surrounded by the images I like, I am wondering if it possible for it to break the information cocoon and provide something new that I never noticed before? I think it is going to be very helpful to add this part into my creativity process.





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
