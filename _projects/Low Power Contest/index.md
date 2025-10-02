---
layout: post
title: Low Power Contest
description: TCL script for post Synthesis analysis to optimize the leakage power of a design with a Multi-Vth approach
skills: 
  - VHDL
  - Verilog
  - Modelsim/Questasim
  - Design Compiler
  - TCL

main-image: /base.png
---

---

<!-- # Header 1 
Used for the title (already generated automatically at the top)
## Header 2  
Use this for the header of each section
### Header 3 
Use this to have subsection if needed


## Embedding images 
### External images
{% include image-gallery.html images="https://live.staticflickr.com/65535/52821641477_d397e56bc4_k.jpg, https://live.staticflickr.com/65535/52822650673_f074b20d90_k.jpg" height="400"%}
<span style="font-size: 10px">"Starship Test Flight Mission" from https://www.flickr.com/photos/spacex/52821641477/</span>  
You can put in multiple entries. All images will be at a fixed height in the same row. With smaller window, they will switch to columns.  

### Embeed images
{% include image-gallery.html images="project2.jpg" height="400" %} 
place the images in project folder/images then update the file path.   


## Embedding youtube video
The second video has the autoplay on. copy and paste the 11-digit id found in the url link. <br>
*Example* : https://www.youtube.com/watch?v={**MhVw-MHGv4s**}&ab_channel=engineerguy
{% include youtube-video.html id="MhVw-MHGv4s" autoplay= "false"%}
{% include youtube-video.html id="XGC31lmdS6s" autoplay = "true" %}

you can also set up custom size by specifying the width (the aspect ratio has been set to 16/9). The default size is 560 pixels x 315 pixels.  

The width of the video below. Regardless of initial width, all the videos is responsive and will fit within the smaller screen.
{% include youtube-video.html id="tGCdLEQzde0" autoplay = "false" width= "900px" %}  

<br>

## Adding a hozontal line
---

## Starting a new line
leave two spaces "  " at the end or enter <br>

## Adding bold text
this is how you input **bold text**

## Adding italic text
Italicized text is the *cat's meow*.

## Adding ordered list
1. First item
2. Second item
3. Third item
4. Fourth item

## Adding unordered list
- First item
- Second item
- Third item
- Fourth item

## Adding code block
```ruby
def hello_world
  puts "Hello, World!"
end
```

```python
def start()
  print("time to start!")
```

```javascript
let x = 1;
if (x === 1) {
  let x = 2;
  console.log(x);
}
console.log(x);

```

## Adding external links

[Wikipedia](https://en.wikipedia.org)


## Adding block quote
> A blockquote would look great if you need to highlight something


## Adding table 

| Header 1 | Header 2 |
|----------|----------|
| Row 1, Col 1 | Row 1, Col 2 |
| Row 2, Col 1 | Row 2, Col 2 |

make sure to leave aline betwen the table and the header

 -->
Post-Synthesis leakage power minimization procedure for Synopsys PrimeTime, submitted for the low-power contest of the Synthesis and Optimization of Digital Systems course at Polytechnic of Turin.


The low-power contest is a competition where participants are tasked to optimize some given designs. The goal is to achieve the lowest possible leakage power while maintaining the functionality of the circuit. The methodology used is based on a Multi-Vth approach, which involves the use of multiple threshold voltages to reduce power consumption.

Given the gate-level netlist synthesized with cells at the lowest $V_t$ option, the problem is to implement a procedure to run in Synopsys' PrimeTime to perform multi-Vt cell assignment, such that:

- The leakage power is minimized.
- The worst slack is not negative.
- The cells retain their footprint.
- The optimization lasts no longer than 3 minutes.

The provided library has 3 $V_t$ options: LVT, SVT, HVT and state-dependent cell leakage power characterization data. The effectiveness of the optimization 
is measured by the provided formula:

```math
    S = \frac{P_\text{lkg, initial}-P_\text{lkg, final}}{P_\text{lkg, initial}}
```
The initial state of the designs is based only on LVT cells.

## Solution

The swap function `swap_vt()` leverages the Prime Time command `estimate_eco` to get an estimation of the slack (area, power and other features of a cell) of the alternative cells types available based on the target one. Then if the target cell is LVT it chooses between SVT and HVT by computing 

```math
	\Delta_{L_to_S} = \textit{Slack}_{LVT} - \textit{Slack}_{SVT}\\
	\Delta_{L_to_H} = \textit{Slack}_{LVT} - \textit{Slack}_{SVH}\\
	\Delta_{L_to_S}-\Delta_{L_to_H} < swap\_threshold
```
if (3) is true then the cell is switched to HVT, SVT otherwise. Meanwhile if the target cell is already SVT, it tries to optimize it by swapping it to HVT. 
This optimization is repeated until cells are available. If the current changed cells are the same as the previous iteration target cells or the 5 minutes elapses the algorithm is terminated (actually 290 seconds to leave room for the time to return to the main function, this was a cconstraint of the contest). 

The two variables available for tuning are :

-`cell_split_factor`: limits the amount of cells that are swapped by each iteration of the algorithm before running a timing update
-`swap_threshold`: sets the threshold which chooses whether to swap for a SVT or HVT cell

[Source Code](https://github.com/ilnerdchuck/SODS_Lab_and_Contest)