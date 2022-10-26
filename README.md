# Image Stitching using OpenCV-Python

  <img src="images\stitching_example.jpg" width="1000"/>
  
## 1. Objective

The objective of his section is to demonstrate image stitching using OpenCV Python API.

## 2. Image Stitching

Image Stitching is the process of modifying the perspective of images and blending them, so that the photographs can be aligned seamlessly. 

Image stitching is the combination and blending of images with overlapping sections to create a single panoramic or high-resolution image. It enables the combination of multiple shots to create a larger picture that is beyond the normal aspect ratio and resolution (super resolution) of the camera’s individual shots. The technology enables positioning for dramatically wide shots without duplicated objects or distortion.

The most familiar use of image stitching is in the creation of panoramic photographs, often used for landscapes. Wide-angle and super-resolution images created by image stitching are used in artistic photography, medical imaging, high-resolution photo mosaics, satellite photography and more.

## 3.  Data

For illustration, we shall use the following 3 input images, which will be stitched together.

<img src="images\The-3-input-images.png" width="1000"/>
  
## 4.  Development

Next, we illustrate the code development of image stitching using OpenCV-Python Stitcher class.

* Project: Image Stitching using OpenCV Python Stitcher() class:
* The objective of this project is to demonstrate image stitching using OpenCV-Python Stitcher class:
  * Author: Mohsen Ghazel (mghazel)
  * Date: March 31st, 2021

### 4.1. Step 1: Imports and global variables


<pre style="color:#000020;background:#e6ffff;font-size:10px;line-height:1.5;"><span style="color:#595979; ">#-------------------------------------------</span>
<span style="color:#595979; "># Imports:</span>
<span style="color:#595979; ">#-------------------------------------------</span>
<span style="color:#595979; "># OpenCV</span>
<span style="color:#200080; font-weight:bold; ">import</span> cv2
<span style="color:#595979; "># Numpy</span>
<span style="color:#200080; font-weight:bold; ">import</span> numpy <span style="color:#200080; font-weight:bold; ">as</span> np
<span style="color:#595979; "># matplotlib</span>
<span style="color:#200080; font-weight:bold; ">import</span> matplotlib<span style="color:#308080; ">.</span>pyplot <span style="color:#200080; font-weight:bold; ">as</span> plt
<span style="color:#200080; font-weight:bold; ">import</span> matplotlib<span style="color:#308080; ">.</span>image <span style="color:#200080; font-weight:bold; ">as</span> mpimg
<span style="color:#200080; font-weight:bold; ">import</span> sys
<span style="color:#595979; "># I/O</span>
<span style="color:#200080; font-weight:bold; ">import</span> os
<span style="color:#595979; "># time </span>
<span style="color:#200080; font-weight:bold; ">import</span> time

<span style="color:#595979; "># Make figures visible</span>
<span style="color:#44aadd; ">%</span>matplotlib notebook
<span style="color:#595979; ">#-------------------------------------------</span>
<span style="color:#595979; "># Check imports and display version numbers</span>
<span style="color:#595979; ">#-------------------------------------------</span>
<span style="color:#200080; font-weight:bold; ">print</span><span style="color:#308080; ">(</span><span style="color:#1060b6; ">"Python version : {0} "</span><span style="color:#308080; ">.</span>format<span style="color:#308080; ">(</span>sys<span style="color:#308080; ">.</span>version<span style="color:#308080; ">)</span><span style="color:#308080; ">)</span>
<span style="color:#200080; font-weight:bold; ">print</span><span style="color:#308080; ">(</span><span style="color:#1060b6; ">"OpenCV version : {0} "</span><span style="color:#308080; ">.</span>format<span style="color:#308080; ">(</span>cv2<span style="color:#308080; ">.</span>__version__<span style="color:#308080; ">)</span><span style="color:#308080; ">)</span>
<span style="color:#200080; font-weight:bold; ">print</span><span style="color:#308080; ">(</span><span style="color:#1060b6; ">"Numpy version  : {0}"</span><span style="color:#308080; ">.</span>format<span style="color:#308080; ">(</span>np<span style="color:#308080; ">.</span>__version__<span style="color:#308080; ">)</span><span style="color:#308080; ">)</span>

Python version <span style="color:#308080; ">:</span> <span style="color:#008000; ">3.8</span><span style="color:#308080; ">.</span><span style="color:#008c00; ">5</span> <span style="color:#308080; ">(</span>default<span style="color:#308080; ">,</span> Sep  <span style="color:#008c00; ">3</span> <span style="color:#008c00; ">2020</span><span style="color:#308080; ">,</span> <span style="color:#008c00; ">21</span><span style="color:#308080; ">:</span><span style="color:#008c00; ">29</span><span style="color:#308080; ">:</span><span style="color:#008c00; ">0</span><span style="color:#ffffff; background:#dd9999; font-weight:bold; font-style:italic; ">8</span><span style="color:#308080; ">)</span> <span style="color:#308080; ">[</span>MSC v<span style="color:#308080; ">.</span><span style="color:#008c00; ">1916</span> <span style="color:#008c00; ">64</span> bit <span style="color:#308080; ">(</span>AMD64<span style="color:#308080; ">)</span><span style="color:#308080; ">]</span> 
OpenCV version <span style="color:#308080; ">:</span> <span style="color:#008000; ">4.5</span><span style="color:#308080; ">.</span><span style="color:#008c00; ">1</span> 
Numpy version  <span style="color:#308080; ">:</span> <span style="color:#008000; ">1.19</span><span style="color:#308080; ">.</span><span style="color:#008c00; ">2</span>


</pre>

### 4.2. Step 2: Define image stitching function:


<pre style="color:#000020;background:#e6ffff;font-size:10px;line-height:1.5;"><span style="color:#595979; ">'''</span>
<span style="color:#595979; ">Perform image stitching using OpenCV-Python Stitcher class</span>
<span style="color:#595979; ">to generate a panorama of the input image</span>
<span style="color:#595979; "></span>
<span style="color:#595979; ">Input:</span>
<span style="color:#595979; ">&nbsp;&nbsp;&nbsp;&nbsp;input_path: the path of the folder containing the input images</span>
<span style="color:#595979; ">&nbsp;&nbsp;&nbsp;&nbsp;output_fname: the file name of the output panorama</span>
<span style="color:#595979; ">&nbsp;&nbsp;&nbsp;&nbsp;</span>
<span style="color:#595979; ">Returns:</span>
<span style="color:#595979; ">&nbsp;&nbsp;&nbsp;&nbsp;None.</span>
<span style="color:#595979; ">'''</span>
<span style="color:#200080; font-weight:bold; ">def</span> stitch_images<span style="color:#308080; ">(</span>input_path<span style="color:#308080; ">,</span> output_fname<span style="color:#308080; ">)</span><span style="color:#308080; ">:</span>
    <span style="color:#595979; ">#-------------------------------------------------</span>
    <span style="color:#595979; "># Step 1: Iterate over the image sin the input </span>
    <span style="color:#595979; ">#         data folder:</span>
    <span style="color:#595979; ">#-------------------------------------------------</span>
    <span style="color:#595979; "># craete a list to store the images</span>
    imgs <span style="color:#308080; ">=</span> <span style="color:#308080; ">[</span><span style="color:#308080; ">]</span>
    <span style="color:#595979; "># iterate over the files in the input foldsr</span>
    <span style="color:#200080; font-weight:bold; ">for</span> <span style="color:#308080; ">(</span>root<span style="color:#308080; ">,</span> dirs<span style="color:#308080; ">,</span> files<span style="color:#308080; ">)</span> <span style="color:#200080; font-weight:bold; ">in</span> os<span style="color:#308080; ">.</span>walk<span style="color:#308080; ">(</span>input_path<span style="color:#308080; ">)</span><span style="color:#308080; ">:</span>
        <span style="color:#595979; "># create a list of file names</span>
        images <span style="color:#308080; ">=</span> <span style="color:#308080; ">[</span>f <span style="color:#200080; font-weight:bold; ">for</span> f <span style="color:#200080; font-weight:bold; ">in</span> files<span style="color:#308080; ">]</span>
        <span style="color:#595979; "># display the list of inpit file name</span>
        <span style="color:#200080; font-weight:bold; ">print</span><span style="color:#308080; ">(</span><span style="color:#1060b6; ">'The list of images to be stitched together: '</span><span style="color:#308080; ">)</span>
        <span style="color:#200080; font-weight:bold; ">print</span><span style="color:#308080; ">(</span>images<span style="color:#308080; ">)</span>
        <span style="color:#595979; "># read all the image files</span>
        <span style="color:#200080; font-weight:bold; ">for</span> i <span style="color:#200080; font-weight:bold; ">in</span> <span style="color:#400000; ">range</span><span style="color:#308080; ">(</span><span style="color:#008c00; ">0</span><span style="color:#308080; ">,</span><span style="color:#400000; ">len</span><span style="color:#308080; ">(</span>images<span style="color:#308080; ">)</span><span style="color:#308080; ">)</span><span style="color:#308080; ">:</span>
            <span style="color:#595979; "># read the next image file</span>
            curImg <span style="color:#308080; ">=</span> cv2<span style="color:#308080; ">.</span>imread<span style="color:#308080; ">(</span>input_path <span style="color:#44aadd; ">+</span> images<span style="color:#308080; ">[</span>i<span style="color:#308080; ">]</span><span style="color:#308080; ">)</span>
            <span style="color:#595979; "># append the read image to the imgs list</span>
            imgs<span style="color:#308080; ">.</span>append<span style="color:#308080; ">(</span>curImg<span style="color:#308080; ">)</span>

    <span style="color:#595979; ">#-------------------------------------------------</span>
    <span style="color:#595979; "># Step 2: Stitch the input  images using: Stitcher </span>
    <span style="color:#595979; ">#         class</span>
    <span style="color:#595979; ">#-------------------------------------------------</span>
    <span style="color:#595979; "># instantiate the Stitcher class</span>
    stitcher <span style="color:#308080; ">=</span> cv2<span style="color:#308080; ">.</span>Stitcher<span style="color:#308080; ">.</span>create<span style="color:#308080; ">(</span>mode <span style="color:#308080; ">=</span> <span style="color:#008c00; ">0</span><span style="color:#308080; ">)</span>
    <span style="color:#595979; "># stitcher = cv2.Stitcher_create(mode = 0)</span>
    <span style="color:#595979; "># Stich the images using Stitcher class</span>
    status<span style="color:#308080; ">,</span> result <span style="color:#308080; ">=</span> stitcher<span style="color:#308080; ">.</span>stitch<span style="color:#308080; ">(</span>imgs<span style="color:#308080; ">)</span>
    <span style="color:#595979; "># check the execution status</span>
    <span style="color:#200080; font-weight:bold; ">if</span> status <span style="color:#44aadd; ">!=</span> cv2<span style="color:#308080; ">.</span>Stitcher_OK<span style="color:#308080; ">:</span>
        <span style="color:#595979; "># in cas eof an error</span>
        <span style="color:#200080; font-weight:bold; ">print</span><span style="color:#308080; ">(</span><span style="color:#1060b6; ">"Can't stitch images, error code = %d"</span> <span style="color:#44aadd; ">%</span> status<span style="color:#308080; ">)</span>
        sys<span style="color:#308080; ">.</span>exit<span style="color:#308080; ">(</span><span style="color:#44aadd; ">-</span><span style="color:#008c00; ">1</span><span style="color:#308080; ">)</span>
    <span style="color:#595979; ">#-------------------------------------------------</span>
    <span style="color:#595979; "># Step 3: Save the output panorama image</span>
    <span style="color:#595979; ">#-------------------------------------------------</span>
    <span style="color:#595979; "># save the output panorama file name</span>
    cv2<span style="color:#308080; ">.</span>imwrite<span style="color:#308080; ">(</span>output_fname<span style="color:#308080; ">,</span> result<span style="color:#308080; ">)</span>
    <span style="color:#595979; "># return</span>
    <span style="color:#200080; font-weight:bold; ">return</span>

</pre>

### 4.3. Step 3: Define input and output paths:

* The folder containing the input images to be stitched together
* The full-path file name of the output panorama image

<pre style="color:#000020;background:#e6ffff;font-size:10px;line-height:1.5;"><span style="color:#595979; "># the input path</span>
input_path <span style="color:#308080; ">=</span> <span style="color:#1060b6; ">'../images/'</span>

<span style="color:#595979; "># the name of the generated panorama image</span>
panorama_fname <span style="color:#308080; ">=</span> <span style="color:#1060b6; ">'../results/panorama.jpg'</span>
</pre>


### 4.3. Step 4: Run the image stitching functionality:

* Call the panorama generator function to stitch the images together


<pre style="color:#000020;background:#e6ffff;font-size:10px;line-height:1.5;"><span style="color:#595979; "># display amessage</span>
<span style="color:#200080; font-weight:bold; ">print</span><span style="color:#308080; ">(</span><span style="color:#1060b6; ">'-------------------------------------------'</span><span style="color:#308080; ">)</span>
<span style="color:#200080; font-weight:bold; ">print</span><span style="color:#308080; ">(</span><span style="color:#1060b6; ">'Start of image stitching:'</span><span style="color:#308080; ">)</span>
<span style="color:#200080; font-weight:bold; ">print</span><span style="color:#308080; ">(</span><span style="color:#1060b6; ">'-------------------------------------------'</span><span style="color:#308080; ">)</span>
<span style="color:#595979; "># keep track of the start time to compute the execution time</span>
start_time <span style="color:#308080; ">=</span> time<span style="color:#308080; ">.</span>time<span style="color:#308080; ">(</span><span style="color:#308080; ">)</span>
<span style="color:#595979; "># call the image stitching function</span>
stitch_images<span style="color:#308080; ">(</span>input_path<span style="color:#308080; ">,</span> panorama_fname<span style="color:#308080; ">)</span>
<span style="color:#595979; "># keep track of the finish time to compute the execution time</span>
finish_time <span style="color:#308080; ">=</span> time<span style="color:#308080; ">.</span>time<span style="color:#308080; ">(</span><span style="color:#308080; ">)</span>
<span style="color:#595979; "># compute the execution time</span>
execution_time <span style="color:#308080; ">=</span> finish_time <span style="color:#44aadd; ">-</span> start_time
<span style="color:#595979; "># display amessage</span>
<span style="color:#200080; font-weight:bold; ">print</span><span style="color:#308080; ">(</span><span style="color:#1060b6; ">'Image stitching completed successfully!'</span><span style="color:#308080; ">)</span>
<span style="color:#200080; font-weight:bold; ">print</span><span style="color:#308080; ">(</span><span style="color:#1060b6; ">"Execution time = "</span> <span style="color:#44aadd; ">+</span> <span style="color:#400000; ">str</span><span style="color:#308080; ">(</span>execution_time<span style="color:#308080; ">)</span> <span style="color:#44aadd; ">+</span> <span style="color:#1060b6; ">" secs."</span><span style="color:#308080; ">)</span>
<span style="color:#200080; font-weight:bold; ">print</span><span style="color:#308080; ">(</span><span style="color:#1060b6; ">'-------------------------------------------'</span><span style="color:#308080; ">)</span>
<span style="color:#595979; "># wait for user to hit any key to terminate session</span>
cv2<span style="color:#308080; ">.</span>waitKey<span style="color:#308080; ">(</span><span style="color:#008c00; ">0</span><span style="color:#308080; ">)</span>
<span style="color:#595979; "># close all open figures</span>
cv2<span style="color:#308080; ">.</span>destroyAllWindows<span style="color:#308080; ">(</span><span style="color:#308080; ">)</span>

<span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span>
Start of image stitching<span style="color:#308080; ">:</span>
<span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span>
The <span style="color:#400000; ">list</span> of images to be stitched together<span style="color:#308080; ">:</span> 
<span style="color:#308080; ">[</span><span style="color:#1060b6; ">'1Hill.JPG'</span><span style="color:#308080; ">,</span> <span style="color:#1060b6; ">'2Hill.JPG'</span><span style="color:#308080; ">,</span> <span style="color:#1060b6; ">'3Hill.JPG'</span><span style="color:#308080; ">]</span>
Image stitching completed successfully!
Execution time <span style="color:#308080; ">=</span> <span style="color:#008000; ">4.738342761993408</span> secs<span style="color:#308080; ">.</span>
<span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span><span style="color:#44aadd; ">-</span>

</pre>

* The generated panorama of the 3 output images is illustrated in the next figure.

  <img src="images\Generated--Panorama.jpg" width="1000"/>
  
  ### 4.5. Step 5: Display a successful execution message:
  

<pre style="color:#000020;background:#e6ffff;font-size:10px;line-height:1.5;"><span style="color:#004a43; ">#</span><span style="color:#004a43; "> display a final message</span>
<span style="color:#004a43; ">#</span><span style="color:#004a43; "> current time</span>
now <span style="color:#308080; ">=</span> datetime<span style="color:#308080; ">.</span>datetime<span style="color:#308080; ">.</span>now<span style="color:#308080; ">(</span><span style="color:#308080; ">)</span>
<span style="color:#004a43; ">#</span><span style="color:#004a43; "> display a message</span>
print<span style="color:#308080; ">(</span><span style="color:#ffffff; background:#dd9999; font-weight:bold; font-style:italic; ">'Program executed successfully on: '</span><span style="color:#308080; ">+</span> str<span style="color:#308080; ">(</span>now<span style="color:#308080; ">.</span>strftime<span style="color:#308080; ">(</span><span style="color:#800000; ">"</span><span style="color:#1060b6; ">%Y-%m-</span><span style="color:#007997; ">%d</span><span style="color:#1060b6; "> %H:%M:</span><span style="color:#007997; ">%S</span><span style="color:#800000; ">"</span><span style="color:#308080; ">)</span> <span style="color:#308080; ">+</span> <span style="color:#800000; ">"</span><span style="color:#1060b6; ">...Goodbye!</span><span style="color:#0f69ff; ">\n</span><span style="color:#800000; ">"</span><span style="color:#308080; ">)</span><span style="color:#308080; ">)</span>

Program executed successfully on<span style="color:#406080; ">:</span> <span style="color:#008c00; ">2021</span><span style="color:#308080; ">-</span><span style="color:#008c00; ">04</span><span style="color:#308080; ">-</span><span style="color:#008c00; ">14</span> <span style="color:#008c00; ">10</span><span style="color:#406080; ">:</span><span style="color:#008c00; ">32</span><span style="color:#406080; ">:</span><span style="color:#008c00; ">54</span><span style="color:#308080; ">.</span><span style="color:#308080; ">.</span><span style="color:#308080; ">.</span>Goodbye<span style="color:#308080; ">!</span>
</pre>

## 5.  Analysis

Image stitching using OpenCV Python Stitcher() class is simple, fast, efficient and generate high-quality panorama image:

* The image border are blended together nicely
* Any illumination differences between the 3 images are mot apparent in the panorama image.

6.  Future Work

* We plan to implement a simplified image stitching from scratch in order to demonstrate the different steps involved in image stitching. The typical image stitching algorithm can be summarized in the following four key steps:

  * Detecting key-points  and extracting local invariant descriptors from two input images
  * Matching the descriptors between the images
  * Using the RANSAC algorithm to estimate a Homography matrix using our matched feature vectors
  * Applying a warping transformation using the Homography matrix obtained from Step 3.

We plan to implement each of these image-stitching steps using ordered ordered images.


4.  References

1. Adrian Rosebrock. Image Stitching with OpenCV and Python. https://www.pyimagesearch.com/2018/12/17/image-stitching-with-opencv-and-python/ 
2. Thalles Silva. Image Panorama Stitching with OpenCV. https://towardsdatascience.com/image-panorama-stitching-with-opencv-2402bde6b46c 
3. Data Hacker. How to create a panorama image using OpenCV with Python. http://datahacker.rs/005-how-to-create-a-panorama-image-using-opencv-with-python/ 
4. Naveksha Sood. Image Stitching to create a Panorama. https://medium.com/@navekshasood/image-stitching-to-create-a-panorama-5e030ecc8f7 
5. Vagdevi Kommineni. Image Stitching. https://vagdevik.wordpress.com/author/



  
  

