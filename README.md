# Quiz 8


1. **Imaging Technique Inspiration**  

The imaging technique I have chosen involves using a noise() function to generate dynamic visual textures. It can mimic natural phenomena or create fluid, smooth transitions in our assignment that require a dynamic visual background, thereby enhancing the visual complexity of the output. In terms of interaction,  mouse or keyboard inputs to respond to user interactions can significantly enhance realism and increase engagement. This method is particularly suitable for assignments that need visual dynamic elements or wish to interact with the background through user inputs.
[Artwork Sketch Link](https://openprocessing.org/sketch/2084145)
![An image of the Artwork Sketch1](readmeImages/screenshot1.png)
![An image of the Artwork Sketch2](readmeImages/screenshot2.png)

2. **Coding Technique Exploration**  

The technique I employ involves using the noise() function ， this method uses Perlin Noise to dynamically manipulate visual elements and create flowing effects. The noise() function generates gradually changing values between calls, making the output not only random but also coherent, thus mimicking the seamless changes found in nature. This function can handle inputs in the form of coordinates, allowing for control over the scale and detail of the noise, which helps to achieve effects that simulate natural texture movements, such as flowing water or swaying underwater plants. This creates an interactive background that enhances my intended effects.

[The secondary page contains example code](https://openprocessing.org/sketch/2084145)  

[noise() introduction](https://p5js.org/reference/#/p5/noise)
``` 
/******************
Code by Vamoss
Original code link:
https://openprocessing.org/sketch/2078734

Author links:
http://vamoss.com.br
http://twitter.com/vamoss
http://github.com/vamoss
******************/

var colors = ["#FFF"]
		// ["#FFC30F", "#581845", "#900C3F", "#C70039", "#FF5733", "#FFC30F"];
    // ["#99c2a2", "#bddbbd", "#d9f0d3", "#a2b3c1", "#c7d3e3"]
    // ["#ffadad", "#ffd6a5", "#fdffb6", "#caffbf", "#9bf6ff"]
    // ["#f3d250", "#f2b035", "#e08d4f", "#d65d5d", "#6b717e"]
    // ["#5e7f5e", "#89a7a7", "#aec6cf", "#d1e1e0", "#f0f3f4"]
    // ["#70416d", "#8b5f89", "#a889a5", "#c0aac0", "#e1d4e4"]

var cache = [];

const border = 50;

function setup() 
{
	createCanvas(210*3, 297*3);
	noiseDetail(4);
	colors = colors.map(c => color(c));
	// noLoop();
	//pixelDensity(4);
}

function draw() {
	background(0);
	const intensity = 90;
	const space = 10;
	const freq = 1.3;
	var t = frameCount/100;
	strokeWeight(3);
	var counter = 0;
	for(var y = border; y < height-border; y+=space){
		for(var x = border; x < width-border; x+=space){
			var n = noise(x/100*freq + t, y/100*freq, t*2)+0.14;
			var n2 = n;//noise(x/300*freq + t, y/400*freq, t*2)+0.14;
			
			if(counter == cache.length){
				cache[counter] = {
					c: lerpColors(n, colors),
					nx: (noise(x)-0.5)*30,
					ny: (noise(y)-0.5)*30
				}
			}
			
			var a = n * TWO_PI;
			var xxx = x + cache[counter].ny;
			var yyy = y + cache[counter].nx;
			
			
			const interact = 600;
			var d = sq(xxx - mouseX) + sq(yyy - mouseY);
			var l = 20;
			//if(d < interact*interact){
				l *= (1-sqrt(d)/interact) * 3;
				// var a2 = atan2(mouseY-yyy, mouseX-xxx) + PI;
				// var d2 = (100-sqrt(d));
				// xxx = cos(a2) * d2 + mouseX;
				// yyy = sin(a2) * d2 + mouseY;
			//}
			
			var xx = cos(a) * l + xxx;
			var yy = sin(a) * l + yyy;
			
			stroke(cache[counter].c);
			line(xxx, yyy, xx, yy);
			
			counter++;
		}
	}
}

/**
 * lerp color from multiple color array
 * param {Integer} [t] lerp factor from 0 to 1
 * param {Array} [[color, color]] colors to lerp, minimum 2 colors in array
 */
function lerpColors(t, colors)
{
	let i = Math.floor(t*(colors.length-1));
	if(i < 0) return colors[0];
	if(i >= colors.length-1) return colors[colors.length-1];

	let percent = (t - i / (colors.length-1)) * (colors.length-1);
	return color(
		colors[i]._getRed() + percent*(colors[i+1]._getRed()-colors[i]._getRed()),
		colors[i]._getGreen() + percent*(colors[i+1]._getGreen()-colors[i]._getGreen()),
		colors[i]._getBlue() + percent*(colors[i+1]._getBlue()-colors[i]._getBlue())
	)
}
```

![An image of the introduction](readmeImages/screenshot3.png)