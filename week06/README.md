# Week 06 · Managing and Collecting Data 

Let's put everything we've learned into a deployable package, and talk about how to plan for, measure, and minimize power use in embedded applications.

- [Components](#components): HC-SR04 Distance Sensor
- [Circuits](#circuits): Weather Station
- [Code](#code): pulseIn, compound conditionals, low-power library, serial plotter, creating datasets
- [Homework](#homework) : Debate Prep and Download Fusion
- [Battery Life Calculator](https://www.digikey.com/en/resources/conversion-calculators/conversion-calculator-battery-life)

-----

### Components

#### HC-SR04

![Distance Sensor](https://2.bp.blogspot.com/-UsoLt1EynwE/V1ZlbCI7SjI/AAAAAAAAF24/GaHFzkrVmc8Roo3IunswHXkPLRhdW-VSgCLcB/s320/hcsr04.jpg)

The HC-SR04 is a very cheap sensor that can accurately determine how far away the nearest object is to it. It is simple to use, and highly accurate when set up with a knowledge of its limitations and abilities. When a signal is sent to a certain *trigger* pin on the HC-SR04, an unaudible sonic *ping* is generated that propogates outwards in a conic shape with around 30 degrees of spread. Quickly switching the *trigger* pin off, and turning on the *echo* pin forces the sensor into listening mode, and it will wait until it hears its own voice bounce back. It alerts a microcontroller when it hears its voice, and the microcontroller can determine how much time passed. With knowledge of the speed of sound, acurate distances can then be calculated. 

![model](https://hackster.imgix.net/uploads/attachments/208568/HCSR04-Schema.jpg?auto=compress%2Cformat&w=680&h=510&fit=max)

The range of the sensor's voice and sensitivity of its ears allows it to measure distances from 2cm to 400cm in ideal conditions, though its real precision will be dependent on the accuracy of the driving microcontroller's clock and ambient environmental and sonic conditions. For maximum precision, the humidity and distance from sea-level of the sensor need to be taken into account, and there are many ways to [add even more precision](https://www.intorobotics.com/8-tutorials-to-solve-problems-and-improve-the-performance-of-hc-sr04/) by modulating how often the ping is emmitted to achieve better range. In ultrasonically noisy environments, a more expensive though more accurate [light-based time of flight (ToF) sensor](https://www.sparkfun.com/products/12785) is a much better choice.

![bats!](https://askabiologist.asu.edu/sites/default/files/echolocation.jpg)

The HC-SR04 sensor was developed [biomimetically](https://www.cnbc.com/2015/05/22/biomimetics-improving-sonar-by-borrowing-from-nature.html) — and effectively recreates the [echolocational](https://en.wikipedia.org/wiki/Animal_echolocation) spatial awareness systems of dolphins and other cetaceans, bats, tenrecs, and switfts. It also derived from the same *Sonar* sensor logic lineage implemented by [submarines](https://en.wikipedia.org/wiki/Sonar) and [sounding vessels](https://en.wikipedia.org/wiki/Echo_sounding) underwater — and a waterproofed version of the sensor is [similarly available](http://chinaultrasound.com/product/1mhz-ultrasonic-transducer-depth-measurement-td1000ka/). Humans even have a [weak form of native echolocation](https://en.wikipedia.org/wiki/Human_echolocation) based on our own voice, ears, and our cognitive understanding of the Doppler effect. This sense can be [further developed](https://www.sciencealert.com/humans-are-being-taught-to-echolocate-like-dolphins-and-it-s-surprisingly-easy) — and has been by several sighted and vision-impaired individuals — for improved sightless navigational awareness inside and outside of low-light conditions.

----- 

### Circuits

Remember to try to wire with an encoding schema in mind...

- Red for 5V Power
- Orange for 3V3 Power
- Black, White, Gray, or Brown for Ground
- Yellow or Purple for Generic Signals
- Green and Blue for I2C Communication

#### Weather Station

Here, let's combine a bunch of sensors into a single weather station object.

- DHT-11 to measure temperature and humidity
- DC Motor to measure wind speeds (modeling an [anemometer](https://www.adafruit.com/product/1733))
- Trimpot to measure wind direction (modeling a high precision [anemogoniometer](https://uk.rs-online.com/web/p/products/2943690/))
- Photoresistor to measure solar radiation (modeling a wide dynamic range [photometer](https://www.adafruit.com/product/1980))
- HC-SR04 to measure nearest object proximity, to determine if there is something obstructing accurate readings.
- We probably would also want to add a [barometer](https://www.adafruit.com/product/1893) to detect incoming storms likelihood, but there's nothing in our kits for that! 

![weather station](weatherstation.png)

-----

### Code

Double check that "Tools" -> "Board" is set to "Arduino/Genuino Uno" and that "Tools" -> "Port" is set to whichever "COM" USB port has a connected "Arduino Uno".

Download and install the [Low-Power Library](https://learn.sparkfun.com/tutorials/reducing-arduino-power-consumption) to put the Arduino into hibernation.

#### Distance Sensor

```c
// Listening 'Echo' Pin
int echoPin = 7;
// Blasting 'Trigger' Pin
int trigPin = 8 ;
// Red subdiode of RGB LED
int rPin = 9; 
// Green subdiode of RGB LED
int gPin = 10; 

//define bounds in cm
int maxRange = 200; 
int minRange = 10; 

//create needed variables at boot to save time later
long duration, distance; // Duration used to calculate distance

void setup() {
  //enable USB serial logging
  Serial.begin (9600);

  //necessary pin modes
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode(rPin, OUTPUT); // Use LED indicator (if required)
}

void loop() {
  //set trigger low  to ensure we don't have any noise from previous cycle
  digitalWrite(trigPin, LOW); 
  //wait a very small amount of time to let electricity to clear the system
  delayMicroseconds(2); 

  //blast out sound!
  digitalWrite(trigPin, HIGH);
  //make sure electricity gets to sensor and emits enough sound
  delayMicroseconds(10); 

  //turn off blaster
  digitalWrite(trigPin, LOW);

  //turn listening on and wait
  duration = pulseIn(echoPin, HIGH);

  //calculate the distance (in cm) based on the speed of sound.
  //since the amount of time is a round trip, we need to first divide by 2
  //sound travels 343 meter/second in sea-level, room temp air
  //343 m/s = .0343 centimeter/microsecond
  // 1 microsecond / .0343 = 29.1 cm of sound travel / microsecond
  distance = ( duration / 2 ) / 29.1;

  //error catch
  if (distance >= maxRange || distance <= minRange){
    // if sensor reads out of range, send a clear signal that the reading isn't trustworthy
    Serial.println("-1");

    //signal bad reading
    digitalWrite(rPin, HIGH); 
    digitalWrite(gPin, LOW); 
  }

  else {
    //print reading over usb
    Serial.println(distance);
    //signal good reading
    digitalWrite(rPin, LOW); 
    digitalWrite(gPin, HIGH); 
  }

  //Delay 50ms before reading again
  delay(50);
}
```

-----

### Homework

We need to make some decisions for going forward. In assigned teams, please research the following microcontroller boards and prepare a **10 slide** presentation to convince the class that we should definitively pursue these platforms for our *wearable* projects.

Some things to think about...

- Explore the general capabilities of the board and its weaknesses. What makes it unique?
- What are the Input and Output options of the board?
- How affordable is the board? Any interesting accessories designed for it?
- How compatible is the board with common sensors? For what kinds of sensor-based applications is it a perfect fit?
- How easy to use and program is the board? How much will need to change our regular Arduino Uno code?
- How power hungry is the board? Does it have any power-related specifics?
- How *wearable* is the board? Anything notable about its dimensions?
- Find inspirational projects that were created with the board. What has the Arduino community already accomplished with this technology?
- Research the boards below that other teams were assigned. Attack them for their weakenesses, and prepare to defend your own board against obvious challenges!

The debate structure will be as follows:

- Team presents their 10 slide deck (5-7 minutes)
- 3 minutes for questions and challenges from the audience
- 2 minute rebuttal

CS + BS : [Particle Electron](https://www.particle.io/products/hardware/electron-cellular-2g-3g-lte/)

PT + SAG : [Paricle Photon](https://www.particle.io/products/hardware/photon-wifi)

AG + LN : [Arduino Nano](https://store.arduino.cc/usa/arduino-nano) / [Pro Mini](https://store.arduino.cc/usa/arduino-pro-mini) and [ESP8266](https://www.amazon.com/HiLetgo-Internet-Development-Wireless-Micropython/dp/B010O1G1ES/ref=sr_1_3?ie=UTF8&qid=1537895379&sr=8-3&keywords=wifi+Arduino) Wifi Board

JS + TB : [Arduino Lilypad Family](https://learn.sparkfun.com/tutorials/choosing-a-lilypad-arduino-for-your-project)

JK : [Breadboard Arduino](https://www.arduino.cc/en/Main/Standalone)

-----

Also, for 3d modeling lessons in the future, please [download and install Fusion360 from Autodesk](https://www.autodesk.com/products/fusion-360/students-teachers-educators?td=aexfusion). You'll need to create an Education Account, but it's free!

