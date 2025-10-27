---
layout: default
title:  "ESP32 Cauldron Project Pt 1."
date:   2025-10-24 00:00:00 +0000
categories: ESP32 Arduino Electronics
---

I wanted to actually finish a project for a change, and since it's October... 'tis the season for spooky things! 


## Initial Idea

The idea was to turn a plastic prop cauldron I bought last year to do something spooky with [but didn't end up having time (I'm sure others can relate...)] into... well something spooky.

My _rough_ idea was to put a strip of addressable RGBW LEDs in to the bottom of the cauldron and have them light up in some "spooky" effect, over the top of these LEDs would be something to diffuse the light a little and then some irridescent translucent baubles to simulate bubbles from what ever was in the cauldron (think George's marvellous medicine).

## Bill of Materials

I'm not actually sure where I got the cauldron from [I think maybe `B & M Bargains`]; the baubles I got from [Amazon][1], they were nice set of varying sizes and should make effective "bubbles". 

For this project I'm using an [ESP32-C3 Super mini][3]; the LED Strip, I just used some that we had at the [Hackspace][5] but something like [these strips][4] should be sufficient.

The repository for this project can be found [here][1] if you want to make one yourself; however, it _is_ a work in progress so YMMV on the actual implementation. With that being said lets get into the actual implementation!


## Initial Prototype
The inital code for the prototype I _borrowed_ from another project I had that used the same LEDs and the same ESP32 - both projects make use of the fantastic [Adafruit_NeoPixel][6] library and to develop I'm using the [PlatformIO][7] Extensions for VSCode - if you're following along with the repo, install the platformIO extenstion into VSCode _first_ and then when you open the project folder it will take care of the set up for you! - although setting up PlatformIO is beyond the scope of this blog post.

---

Note: From here on out I'll be referring to the LED strips as 'pixelStrip' (in the code as well as the related blog posts).

---

First thing we need to do is "light up" some LEDs to do that we need to _tell_ the controller a little bit about the pixelStrip; this goes in our `LED.h` (again if you've following along at home this will make sense in the repo structure):

```c
// Change this number here to reflect how many LEDs we have in the pixelStrip
static const int NUM_PIXELS_PER_STRIP = 7 ; 
static int pixelPin = 5 ; // The pin for the _data_ to address the LEDs in the pixelStrip
/**
Create our pixelStrip - if we want to use more than one 
we can use this same line of code just swap out `pixelPin` with another pin number
*/
static Adafruit_NeoPixel pixelStrip(NUM_PIXELS_PER_STRIP, pixelPin, NEO_GRB + NEO_KHZ800); 
static unsigned long lastBlinkTime = 0;
static bool ledOn = false;

void ledUpdateBlinkingPixel(uint32_t colour, int intervalMs);
```

And then in our `LED.c` we can implement the code for `ledUpdateBlinkingPixel`:

```c
void ledUpdateBlinkingPixel(uint32_t colour, int intervalMs) {
    pixelStrip.setBrightness(30);
    while (true){
        ledOn = !ledOn;
        /**
        Although brigtness _can_ go to 255 - you'll not really notice the difference after about 50... 
        it's do do with how eyes perceive light
        */

        pixelStrip.setPixelColor(0, ledOn ? colour : 0); // This uses just the first LED in the strip
        delay(intervalMs);
        pixelStrip.show();
    }
}
```

Next we need to initialise and call this (in the arduino way) our `setup` and `loop` functions will look something like this:

```c
void setup() {
    Serial.begin(115200);
    while (!Serial)
        delay(10);

    pixelStrip.begin();           // INITIALIZE NeoPixel strip object (REQUIRED)
    pixelStrip.setBrightness(50); // Set BRIGHTNESS to about 1/5 (max = 255)

    pixelStrip.setPixelColor(0,255,0,0);
    pixelStrip.show();
}

void loop() {
    if (!bootComplete) {
        if (bootStartTime == 0)
            bootStartTime = millis();

    unsigned long elapsed = (millis() - bootStartTime) / 1000;
    unsigned long remaining = 5 - elapsed;
    static unsigned long lastSecond = 999;
    if (remaining != lastSecond && remaining != 0) {
        lastSecond = remaining;
        String countdown = "Starting in " + String(remaining) + "s";
    }

    if (remaining == 0) {
        bootComplete = true;
        currentLedColor = Adafruit_NeoPixel::Color(0, 0, 255);  // Blue for main menu
        ledUpdateBlinkingPixel(currentLedColor,200);
    }
    delay(200);
  }
}
```

Now this isn't the only way to do this - but it's an easy way to do it - once we've deployed this it will blink an LED _blue_ roughly twice every second... yay!

## Measuring How Many LEDs In The Strip

Next we need to count how many LEDs are in the length of strip we have ... depending on the length this can be quite a tedius process but we're problem-solvers and programmers here so let's use programming to solve this problem!

I created a function called `RelerLEDs` and for every 10th LED made it a different colour that stood out from the rest - in this case I made it a yellow-ish colour with the rest of the LEDs staying blue keeping with the last example:

```c
void RulerLEDs(){
  int length_of_LEDs = 600;
  int i =0; 
  run = true;
  pixelStrip.setBrightness(50); // Set BRIGHTNESS to about 1/5 (max = 255)
  while (true){
      if (i% 10 == 0){
        pixelStrip.setPixelColor(i,245,150,0);
      }else{
        pixelStrip.setPixelColor(i,0,0,255);
      }
      pixelStrip.show();
      delay(100);
      i++;
    }
  }
```
Now all we need to do is count how many _yellow_ LEDs we have and then how many blue LEDs after the final yellow. 

---
Note: Subtract 10 from the final number as the first LED will also be yellow because of the modulo operation `0 % 0 == 0` (this is because the array is _zero indexed_)

---

With my prototype pixelStrip, `6` yellow and `9` blue after the final yellow so 59 LEDs in total `((6*10)+9)-10` to confirm this we can change the LED that blinks from the first example to be the last LED in the pixelStrip, we end up with someting like this:

```c
pixelStrip.setPixelColor(58, ledOn ? colour : 0); 
```

Now We're up and running, we can start to experiment with different effects. but that is for [another post][8]!

Hopefully, this was interesting

Phyu


[1]: https://github.com/phyushin/CauldronESP32
[2]: https://www.amazon.co.uk/dp/B0B985G92X
[3]: https://www.aliexpress.com/item/1005006904525598.html
[4]: https://www.aliexpress.com/item/1005009231346728.html
[5]: https://leighhack.org
[6]: https://github.com/adafruit/Adafruit_NeoPixel
[7]: https://github.com/platformio
[8]: {% post_url ../2025-10-26-ESP32-Cauldron-project-pt2 %}