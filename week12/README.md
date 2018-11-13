# Week 12 · Modeling Progress

After project reviews last week, let's do some targeted work. Most students last week received meaningful feedback from their peers (those that didn't present last week certainly should chat with everyone today!)

It would be best to spend today sketching, 3d modeling, and considering the form and materiality of the objects/services to be. Recall that there is [work to be done](../briefs.md) outside of electronics bits and coding, and that you will need to order prototyping parts for your smaller-than-Arduino-Uno microcontroller (like Particle Photons or Arduino Nanos) and battery, as well as your project enclosure(s) and physical affordances.

-----

In addition to thinking about form and materiality, make some progress! Even if you don't have all the parts you need, you can certainly *model* the behavior of sensors and actuators with comparable sensors, simple potentiometers and buttons, and even [`random()`](https://www.arduino.cc/reference/en/language/functions/random-numbers/random/) numbers.

Recall through the [*black box* model of electronics](https://en.wikipedia.org/wiki/Black_box) that we can envision our projects as composed of three main aspects.

INPUT  : The sensor-derived numbers, ingested data, and any other information flowing into your system.
OUTPUT : The actuators, feedback elements, and any other ouput behaviors flowing out from your system.
ALGORITHTM : The arithmetic, analysis, remapping, and data manipulation that occurs to convert the *input* into the *output*.

We might also want to add a fourth.

DESIGN : How all the INPUT and OUTPUT components are orientated, supported, contained, and interfaced with the world.

We've been thinking alot about INPUT, a little about OUTPUT, and often doing a lot of hand-waving around the ALGORITHM part, which is commonly the most complex and difficult to build. We've also done almost no DESIGN work — at a design school!

----

Each of you would be advised to, as quickly as possible, finish these simple proofs-of-concepts for your projects. Spend as much time on the software and *magical algorithms* that need to be built, rather than the electronics, this week. Let's try to abstract the parts away as much as possible and focus on software and, when appropriate, physical build out.

AG : Breadboard a circuit consisting of an IMU, a button, and several LEDs. Depending on the orientation of the sensor, can different leds be illuminated, perhaps two for each dimension, for positive or negative rotation? Could the button be used to set a *recall* position, which the LEDs then teach a user to recreate?

BS : Play with your sonic rangefinder as a model for a person's relative position in a single dimension. We might imagine arraying range finders as a cheap and simple way to instrument a space. As the value changes, how might discrete information be presented elsewhere, perhaps modeled by color on an RGB led? Alternatively, is an even cheaper button matrix worth considering to determine where people are in a space? See [this document](thread_touch.pdf) for a walkthrough on that aimed at conductive thread and copper tape or [this page for a wired or copper tape version](https://paulbleisch.com/2015/01/19/custom-arduino-membrane-keypad/).

CS : Breadboard a circuit consisting of a piezo buzzer and several trimpots/buttons (as models for brainwave data) to control a simple drum machine or keyboard. Beat duration, rest duration, pitch, vibrato, polyphonic chord, attack/decay... Start by making these controls very obvious, and then synthesize them into a repertoire of composite controls. Can you ensure that pleasant sounds come out, regardless of the positions of the pots by pulling from arrays rather than just remapped values?

JS : Is [this](https://www.sparkfun.com/products/13116) worth purchasing? With multiple solenoids and motors, can you collect some volume values into an array, and then move those values through the different motors like a traditional scrolling music visualizer? 

LAN : Breadboard a circuit consisting of an SD card reader, a motion sensor, and a photoresistor to model breakbeams or other sensors. [Write the value](https://learn.adafruit.com/adafruit-micro-sd-breakout-board-card-tutorial?view=all) from the sensors as well as a relative timestamp to a file on the SD card at regular intervals. Consider also how all this information might later be visualized?

PT : Breadboard a circuit consisting of a gas sensor, thermistor or temperature sensor, and a trimpot to model pulse rate. Then, consider how to provide audiovisual feedback (in a HUD?) with LEDS, LCD displays, and a piezo buzzer. Maybe even [small oleds](https://www.adafruit.com/product/1431) or [bargraphs](https://www.adafruit.com/product/1815)?

SAG : Breadboard a set of simple analog sensors like a photoresistor, a trimpot, and a thermistor. Take the set of sensors, and pretend each is a biometric indicator. How is *wellbeing* evaluated and modeled based on those sensors? How are activities modeled, distinguished, and tracked? How will this information be presented to users (audiovisually or with aggregated data visualization)?

TB : Breadboard a continuous rotation servo motor to control a paper roll feed, or an advancing and tensioning system. How will the system handle keeping the paper unspooling cleanly? How will the roll be loaded, fed, and have possible jams cleared?

-----

### Homework

Make some meaningful progress! 