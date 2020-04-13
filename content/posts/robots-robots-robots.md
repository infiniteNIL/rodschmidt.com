---
title: 'Robots! Robots! Robots!'
date: Thu, 14 May 2015 00:21:25 +0000
draft: false
tags: ['Robots']
---

I’ve been getting into robots lately. For a long time I was searching for the perfect robot kit to use with my Arduino. Then I decided I’d rather use my Raspberry Pi to control my robots so I could use better programming languages like Clojure, Elixir, Scala, Ruby, etc. So I was looking around for the perfect Raspberry Pi robot kit.

For Christmas, my parents gave me the [OWI Robotic Arm Edge](https://www.amazon.com/gp/product/B0017OFRCY/ref=as_li_tl?ie=UTF8&tag=rodschmidt-20&camp=1789&creative=9325&linkCode=as2&creativeASIN=B0017OFRCY&linkId=bf904572cfaa824e0d4a13b79c042132). For the longest time, I didn’t like it. I wanted a moving robot and something I could program.   

Finally in April, I decided to pull the plug on the [GoPiGo](https://www.amazon.com/gp/product/B01LZMH0CZ/ref=as_li_tl?ie=UTF8&tag=rodschmidt-20&camp=1789&creative=9325&linkCode=as2&creativeASIN=B01LZMH0CZ&linkId=c278025b45a49276ad7b4c46da62d98e), a Raspberry Pi robot kit. It was a little challenging to put together the motors at first and one of the encoders broke. I asked for a new one and they sent it right out. I ran the examples that simply let me remote control the GoPiGo by ssh’ing into the Raspberry Pi on the robot. I had no sensors or anything else. My goal is to create an autonomous robot. No remote control.

So I ordered an ultrasonic sensor, and a Raspberry Pi camera and waited. It took probaby a month before those all came in.

In the meantime, I decided to put the robot arm together. 48 steps! Took me 2 days, but it wasn’t that hard. I ended up with this.

{{<figure src="/images/completed-robot-arm.png" alt="Completed robot arm" caption="Completed robot arm" width="370">}}

Then I went to our local Microcontroller meetup hosted by [Make Salt Lake](http://makesaltlake.org). There were lots of cool projects everybody was working on, and I talked a little about my GoPiGo. I expressed my desire to make it autonomous and also to have a battery that I could recharge. Somebody mentioned the [iRobot Create](http://www.irobot.com/About-iRobot/STEM/Create-2.aspx) and that got me thinking. A couple of weeks later, I ordered one, and a week later it showed up.

{{<figure src="/images/irobot-create2.png" alt="iRobot Create 2" caption="iRobot Create 2" width="370">}}

Finally, my GoPiGo parts came and I added those to my robot. That was a little bit of a challenge as well. Here’s the GoPiGo:

{{<figure src="/images/gopigo.png" alt="Completed GoPiGo" caption="Completed GoPiGo" width="370">}}

I ran the examples for that demonstrating how to use the sensor and camera. They worked. I decided to order a compass and a GPS module to add to the GoPiGo because there are examples that use that. I want to make this thing as smart as possible. So, lots of things to do. I want to:    

* Control the robot arm wirelessly
* Make the GoPiGo roam around on its own.
* Use Clojure to program the Roombda (iRobot Create) and see what I can do with that. 
* Someday, maybe add a Kinect to it, or attach the robot arm to it.

  I don’t know where all of this is going, but I’m just having fun exploring and seeing what I can do. I’ll keep you informed.
