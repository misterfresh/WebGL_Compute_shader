This is a fork of https://github.com/9ballsyndrome/WebGL_Compute_shader

As an exercise to understand the code, I adapted the demos so that they can be used with Chrome's builtin local files server. 
Open chrome dev tools, go to Sources tab and after cloning the repo, add the repo folder as "Overrides".
Then, with your DevTools open, open "http://bitonic.local/" for example.


To get it to work, you might also need to add at the end of your etc/hosts file:

```
127.0.0.1		boids.local
127.0.0.1		bitonic.local
127.0.0.1		simple.local
127.0.0.1		vertex.local
```
If you get an "incorrect mimetype" error in the console, you will also need to enable the following Chrome extension to set correct mimetypes for JS and CSS files : https://github.com/misterfresh/mimetype-enforcer
. Clone the repo and then on the chrome://extensions/ page, add the folder as unpacked extension.

# WebGL 2.0 Compute shader Demos

This repository shows samples of WebGL Compute shader. For detail, see [Intent to Implement: WebGL 2.0 Compute](https://groups.google.com/a/chromium.org/forum/#!msg/blink-dev/bPD47wqY-r8/5DzgvEwFBAAJ) and [WebGL 2.0 Compute Specification - Proposal](https://docs.google.com/document/d/1EwhDJO_lBH1mGMMwheQUXGhhFk9yoC98Ant3TPqwmmA/view).

As of Dec 2018, WebGL Compute shader runs only in **Windows [Chrome Canary](https://www.google.com/chrome/canary/)** launched with below command line flags.

- `--enable-webgl2-compute-context`  
Enable WebGL Compute shader, WebGL2ComputeRenderingContext  
Or you can enable this flag via chrome://flags/, choose "WebGL 2.0 Compute"

- `--use-angle=gl`  
Run ANGLE with OpenGL backend because Shader Storage Buffer Object is now on implementing and does not work well in D3D backend yet  
Or you can enable this flag via chrome://flags/, choose "Choose ANGLE graphics backend"

- `--use-cmd-decoder=passthrough`  
In some environments, it could run well only after adding this flag. So try this if could not work with above two flags


*Note that some environments could not run WebGL Compute shader, especially in older GPU nor display driver (nor perhaps integrated GPU).


## Boids simulation

Interaction calculations such as Boids simulation are suited for Compute shader because it can be run in parallel.
In this demo, it calculates coordinates and velocities of each Boids using Compute shader, and draws them using Instancing.
**In CPU, it is not doing anything except calling graphics API**.

[![181218_webgl-compute-boids_demo.gif](https://raw.githubusercontent.com/9ballsyndrome/WebGL_Compute_shader/master/document/181218_webgl-compute-boids_demo.gif)](https://9ballsyndrome.github.io/WebGL_Compute_shader/webgl-compute-boids/dist/)

- [View demo (Make sure you are on a system with WebGL 2.0 Compute enabled)](https://9ballsyndrome.github.io/WebGL_Compute_shader/webgl-compute-boids/dist/)
- [Source](https://github.com/9ballsyndrome/WebGL_Compute_shader/tree/master/webgl-compute-boids)


## Bitonic sort

Bitonic sort is one of the sort algorithm that can be executed in parallel. This demo sorts an array with selected number of elements by CPU (JavaScript built-in `sort()`) and GPU (Compute shader), then compares each elapsed time.
In CPU sort, it simply measures the time to execute `sort()`. In GPU sort, it measures **the total time** to copy the array to GPU, execute Bitonic sort in Compute shader and copy the result data back to CPU.
Note that WebGL initialization time, for example shader compile, is not included.

[![181218_webgl-compute-bitonicSort_demo.gif](https://raw.githubusercontent.com/9ballsyndrome/WebGL_Compute_shader/master/document/181218_webgl-compute-bitonicSort_demo.gif)](https://9ballsyndrome.github.io/WebGL_Compute_shader/webgl-compute-bitonicSort/dist/)

- [View demo (Make sure you are on a system with WebGL 2.0 Compute enabled)](https://9ballsyndrome.github.io/WebGL_Compute_shader/webgl-compute-bitonicSort/dist/)
- [Source](https://github.com/9ballsyndrome/WebGL_Compute_shader/tree/master/webgl-compute-bitonicSort)


## Simplest Compute shader

This is for the first step to implement Compute shader. It copies an array defined in CPU to GPU as the input data, writes its thread index to the array through Shader Storage Buffer Object in Compute shader, then copies the result data back to CPU and read it.

[![181218_webgl-compute-simple_demo.png](https://raw.githubusercontent.com/9ballsyndrome/WebGL_Compute_shader/master/document/181218_webgl-compute-simple_demo.png)](https://9ballsyndrome.github.io/WebGL_Compute_shader/webgl-compute-simple/dist/)

- [View demo (Make sure you are on a system with WebGL 2.0 Compute enabled)](https://9ballsyndrome.github.io/WebGL_Compute_shader/webgl-compute-simple/dist/)
- [Source](https://github.com/9ballsyndrome/WebGL_Compute_shader/tree/master/webgl-compute-simple)

## Vertex Compute shader

We can bind Shader Storage Buffer Object as ARRAY_BUFFER and use it as vertex attribute in Vertex shader.
So, we can use the result, calculated in Compute shader, in Vertex shader without going through CPU. This means there is no memory copy between CPU and GPU.
In this sample, it calculates particle coordinates in Compute shader and writes the result to Shader Storage Buffer Object, then renders the particle using it as attribte in Vertex shader.

[![181225_webgl-compute-vertex_demo.gif](https://raw.githubusercontent.com/9ballsyndrome/WebGL_Compute_shader/master/document/181225_webgl-compute-vertex_demo.gif)](https://9ballsyndrome.github.io/WebGL_Compute_shader/webgl-compute-vertex/dist/)

- [View demo (Make sure you are on a system with WebGL 2.0 Compute enabled)](https://9ballsyndrome.github.io/WebGL_Compute_shader/webgl-compute-vertex/dist/)
- [Source](https://github.com/9ballsyndrome/WebGL_Compute_shader/tree/master/webgl-compute-vertex)
