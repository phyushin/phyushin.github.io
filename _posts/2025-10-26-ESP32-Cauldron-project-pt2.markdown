---
layout: default
title:  "ESP32 Cauldron Project Pt 2."
date:   2025-10-26 00:00:00 +0000
categories: ESP32 Arduino Electronics
---

Following on from the [previous post][1] we know our prototype works now, and how many LEDs we have in the pixelStrip next it's time to make some cool effects just to experiment and see what we can do.

## First Effect
So for our first effect, lets fill the pixelStrip with all red LEDs, this can be easily accomplished with the following code:

```c
void ledRolling(uint32_t colour, int intervalMs,  int length_of_LEDs){
  run = true;
  bool ledOn = true;

  unsigned long now = millis();
  int i = 0;
    while (run){
      i = i%(length_of_LEDs +1);
      pixelStrip.setBrightness(MED); // Set BRIGHTNESS to about 1/5 (max = 255)
      pixelStrip.setPixelColor(i, (ledOn ? colour : 0));
      pixelStrip.show();
      delay(100);
      if (i == length_of_LEDs){
        ledOn = !ledOn;
      }
      i++;
    }
  }
```
This will light up each LED sequentially until it gets to the end and then switch them off sequentially in the same manner, which will give the impression the LEDs are _rolling_ round. It's quite a simple effect but looks quite nice. 

## Using Constant Colours (with a U)

Another thing I did was to create some constant colours to make it more straight forward to easily specify colours, I added the following _constants_:

```c
  static const uint32_t purple = Adafruit_NeoPixel::Color(151,16,245);
  static const uint32_t orange = Adafruit_NeoPixel::Color(255,20,0);
  static const uint32_t yellow = Adafruit_NeoPixel::Color(255,120,0);
  static const uint32_t red = Adafruit_NeoPixel::Color(255,0,0); 
  static const uint32_t green =  Adafruit_NeoPixel::Color(0,255,0); 
  static const uint32_t blue = Adafruit_NeoPixel::Color(0,0,255); 
```

This way I can just pass `purple` or `orange` to one of the effects that use a simple colour such as `ledRolling` or `ledChaser` rather than having to work out what RGB values to use; each time I like a colour I can add the constant and... well you get the idea! For example:

```c
  ledRolling(orange,500,59);
```

## Scanning Animation
Next up, I wanted to do some scanning animation - think a Cylon or Kitt from knight rider, there's a couple of things to consider here the length of the _scanning_; the speed; and how to get the _scan_ to "bounce" back when it gets to the end.
After some trial and error I found that a length of 11 was sufficient (oddly any more than that seems to currently crash - so 11 it is!) the way I accomplished the _bouncing_ was to keep a _direction_ variable `j` and every itteration of the loop add `j` to our increment counter (good old `i`) once the counter got to the size of the array (minus one, zero indexed arrays remember?) we flip the sign on `j` this results in the itteration count coming _down_.

It works something like this:

```c
/**
* For some reason this crashes if the length is set higher than 11
* need to investigate - FRAKKIN TOASTER!
*/
void cylonChaser(int intervalMs, int length_of_LEDs){
  unsigned long now = millis();
  pixelStrip.setBrightness(30);
  int i,j = 0;
  if (now - lastBlinkTime >= intervalMs) {
    lastBlinkTime = now;
    ledOn = !ledOn;
    while (true){
      if (i > 0) {
        pixelStrip.setPixelColor(i-1,0);
        pixelStrip.setPixelColor(i,0);
      }

      pixelStrip.setPixelColor(i, red);
      pixelStrip.setPixelColor(i+1,0);
      pixelStrip.show();
      delay(40);
      i = i + j;
      if (i >= (length_of_LEDs - 1)){// We're at the top! Go back down!
        j = -1;
        delay(20);
      }
      if (i <= 0){ // We're at the bottom! Go up again!
        j = 1; 
        delay(20);
      }
    }
  }
```
And for the classic knight rider "scanning" it's two LEDs at the same time which is mostly the same code
```c

void kittChaser(int intervalMs, int length_of_LEDs){// Like the front of the car from knight righer
  unsigned long now = millis();
  int i,j = 0;
  pixelStrip.setBrightness(30); 

  if (now - lastBlinkTime >= intervalMs) {
    lastBlinkTime = now;
    ledOn = !ledOn;
    while (true){
      if (i > 0) {
        pixelStrip.setPixelColor(i-2,0);
        pixelStrip.setPixelColor(i,0);
      }
      pixelStrip.setPixelColor(i, red);
      pixelStrip.setPixelColor(i-1,red);
      pixelStrip.setPixelColor(i+1,0);
      pixelStrip.show();
      delay(80);
      i = i + j;
      if (i >= (length_of_LEDs -1)){
        j = -1;
        delay(20);
      }
      if (i <= 0){
        j = 1;
        delay(20);
      }
    }
  }
}

```

That covers a couple of different effects we've explored to see how to approach animation effects on the pixelStrip, next time we'll be looking at how to stick this into the cauldron!

Hopefully, this was interesting

Phyu


[1]: {% post_url ../2025-10-24-ESP32-Cauldron-project-pt1 %}
