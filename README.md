Project Overview

Project #4 of Udacity's Front-End Web Developer Nanodegree. The task was to identify and perform 
optimization to achieve a PageSpeed score of 90 on index.html and ensure a consistent frame rate at 
60 frames per second when scrolling in pizza.html.


How to run:
1. Go to https://developers.google.com/speed/pagespeed/insights/
2. Enter https://developers.google.com/speed/pagespeed/insights/ to analyze.



Optimizations done for the project:

The following changes were made:
------------
Index.html

CSS
-Inlined all of the CSS into the head of the document and added the HTML media="print" 
  attribute to the external style sheet link for print styles.
- Minified the files using http://refresh-sf.com/

JS
- Added the HTML async attribute to all script tags and moved them to the end of the body tag.
- Also minified the files using http://refresh-sf.com/

Images
- Compressed all images using http://compressnow.com/ 

-------------


main.js

- Using getElementsByClassName as it is more faster way to access DOM element
	and moving it out of the for loop
- Declaring elem out of the for loop
- Removing  height and width styles from the generated pizza elements to CSS
- Get number of pizza needed based on the screen size using function getPizzaNumber()


CODE

	// Generates the sliding pizzas when the page loads.
	document.addEventListener('DOMContentLoaded', function() {
		var cols = 8;
		var s = 256;
		
		//Get number of pizza needed based on the screen size
		var getPizzaNumber = function() {
			var h = window.innerHeight;
			var w = window.innerWidth;
			var r;
			r = Math.round((h * 8) / w);
			return r * cols;
		}

		//Reduce the number of pizza need to be rendered
		var pizzaCount = getPizzaNumber();
		
		//var movingPizzas = document.querySelector("#movingPizzas1");
		/*Using getElementsByClassName as it is more faster way to access DOM element
		and moving it out of the for loop*/
		var movingPizzas = document.getElementById("movingPizzas1");
		
		//Declaring elem out of the for loop
		var elem;

		for (var i = 0; i < pizzaCount; i++) {
			elem = document.createElement('img');
			elem.className = 'mover';
			elem.src = "images/pizza.png";
			/*Removing  height and width styles from the generated pizza elements to CSS*/
			//elem.style.height = "100px";
			//elem.style.width = "73.333px";
			elem.style.left = (i % cols) * s + 'px';
			elem.style.top = (Math.floor(i / cols) * s) + 'px';
			movingPizzas.appendChild(elem);
		}
		updatePositions();
	});

--------------

- Using getElementsByClassName as it is more faster way to access DOM element and 
	moving it out of the for loop.
- Moving document.body.scrollTop out of the for loop	
- Declaring phase outside the for loop
- use CSS transform to move the item back and forth on the x-axis


CODE

	// Moves the sliding background pizzas based on scroll position
	function updatePositions() {
		frame++;
		window.performance.mark("mark_start_frame");
		
		// var items = document.querySelectorAll('.mover');
		/*Using getElementsByClassName as it is more faster way to access DOM element and 
		moving it out of the for loop.*/
		var items = document.getElementsByClassName('mover');
		
		//Moving document.body.scrollTop out of the for loop
		var scrollTop = document.body.scrollTop / 1250;
		
		//Declaring phase outside the for loop
		var phase;
	 
		for (var i = 0; i < items.length; i++) {
			phase = Math.sin(scrollTop +(i % 5));
			//items[i].style.left = items[i].basicLeft + 100 * phase + 'px';
		
			//using CSS transform to move the item back and forth on the x-axis
			items[i].style.transform = 'translate3d(' + 100 * phase + 'px, 0px, 0px)';
		}

		// User Timing API to the rescue again. Seriously, it's worth learning.
		// Super easy to create custom metrics.
		window.performance.mark("mark_end_frame");
		window.performance.measure("measure_frame_duration", "mark_start_frame", "mark_end_frame");
		if (frame % 10 === 0) {
			var timesToUpdatePosition = window.performance.getEntriesByName("measure_frame_duration");
			logAverageFrame(timesToUpdatePosition);
		}
	}

-----------------



Resizing Pizza

- Using getElementsByClassName as it is more faster way to access DOM element
- Saving array length in a local variable

CODE


    // Iterates through pizza elements on the page and changes their widths
    function changePizzaSizes(size) {
		var newWidth = 100;
		switch(size) {
			case "1":
				newWidth = 25;
				break;
			case "2":
				newWidth = 33.3;
				break;
			case "3":
				newWidth = 50;
				break;
			default:
				console.log("bug in sizeSwitcher");
		}
    
		//var Pizza = document.querySelectorAll(".randomPizzaContainer");
		//Using getElementsByClassName as it is more faster way to access DOM element
		var Pizza = document.getElementsByClassName('randomPizzaContainer');
		
		//Saving array length in a local variable
		for (var i = 0, len = Pizza.length; i < len; i++) {
			Pizza[i].style.width = newWidth + "%";   //sets the width in percentage
		}
	}


-------------------------------------





