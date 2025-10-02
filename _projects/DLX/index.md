---
layout: post
title: DLX Processor
description: An Implementation of a DLX processor including Simulation, Synthesis and Physical Design
skills: 
  - VHDL
  - Modelsim/Questasim
  - Innovus
  - Docker 
  - Git
  - Scripting

main-image: /datapath.png
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
The primary objectives of the DLX project were:

- Design and implement a functional DLX microprocessor using VHDL.
- To understand the principles of RISC architecture and pipeline processing.
- To validate the design through simulation.
- Synthesize the processor.
- Use a standard cell layout for Physical Design.
- Full automation for simulation, synthesis and physical design via scripts

## DLX Features

Not the full instruction set has been implemented but only a subset, focusing on the
fundamental operations required for basic computation:

- Arithmetic Instructions: R-type: ADD, SUB, SLL, SRL, SNE, SLE, SGE
- Arithmetic Instructions: I-type: ADDi, SUBi, SLLi, SRLi, SNEi, SLEi, SGEi
- Logical Instructions: R-Type: AND, OR, NOT, XOR
- Logical Instructions: I-Type: ANDi, ORi, NOTi, XORi
- Memory Instructions: R-Type: LOAD (LW), STORE (SW)
- Control Flow Instructions: JUMP, JAL, BNEZ, BEQZ

There have been implemented two features that enanches the DLX processor:

- Forwarding Unit: This unit is designed to handle data hazards by forwarding the results of
previous instructions to subsequent instructions that require them, thus reducing the need for
stalls in the pipeline.
- Branch Prediction: The DLX microprocessor incorporates a simple static branch prediction
mechanism to improve the performance of control flow instructions. This feature helps to mini-
mize pipeline stalls caused by branch instructions.

## Implementation Structure
The DLX has been divided in a classical Control Unit and Datapath schema. 
The Datapath has been implemented via an Hardwired structure. Meanwhile the Datapath has been split in sub-components 
for each stage of the pipeline:

- Fetch
- Decode
- Execute
- Memory
- Write Back

[Source Code](https://gitfront.io/r/ilnerdchuck/mwejCaw3AVcb/DLX-Microprocessor/)