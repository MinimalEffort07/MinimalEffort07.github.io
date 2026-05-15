---
layout: post
title:  "Buffering In C"
date:   2025-8-12
categories: [C Language]
---
<br>
## Table of Contents
<ol class="toc">
    <li> <a href="#what-is-buffering"><span class="title">What is Buffering</span></a> </li>
</ol>

# What is Buffering
Buffer is when data that is inteded to be sent to some hardware device like a monitor is stored 
temporarily in memory before being flushed (sent) to the hardware device.  

Common Style of buffering are:
1. No buffering -- Data is sent immediatley to the hardware device
2. Line buffering -- Data is sent to the hardware device on encountering a '\\n' character
3. Full Buffering -- Data is sent to the hardware device only once the buffer where the data is 
intermediatley stored is full. 

## No Buffering
Easiest to understand, and will be very familiar..
NOTE: This won't actually work on windows, read on to understand why..  
```c
int main() {
  char buf[20] = {0};
  if (setvbuf(stdout, buf, _IOLBF, sizeof(buf)) != 0) {
    puts("Failed to set buffering type");
  }
  fflush(stdout);

  printf("I want to print this text, but I haven't added a newline character..");
  getchar();
  printf("This print occured after a getchar call, i have a newline on the end to flush the buffer\n");
}
```
<br>
In theory what should happen is that the contents of the first printf should be written to the 
buffer "buf" but not printed to screen yet because it doesn't have a new line character. Then the 
call to getchar() will be made and you will be asked for input. Finally the contents of the second 
printf call get appended to buf, the newline character is reach signalling that the contents of buf
should be sent to stdout..  
<br>
Expected output:
```
<..getchar asking for input..>
"I want to print this text, but I haven't added a newline character..This print occured after a 
getchar call, i have a newline on the end to flush the buffer\n"  
```
<br>
Actual Output:
```
"I want to print this text, but I haven't added a newline cha"
<..getchar asking for input..>
racter..This print occured after a getchar call, i have a newline on the end to flush the buffer\n"
```
<br>
Why did this happen? Well ZZ
