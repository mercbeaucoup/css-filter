# Rose Filter

## Learning Objective
- Understand relevance of hexadecimal arithmetic to CSS
- Create a prototype of a filter (often used in apps like Instagram)

## Prompt

Design a simple web app that features a color swatch preview (in 6 color hex notation, e.g. #A02BFF), and filter button. When pressed, the filter button should apply a 'Rose' filter, making the color of the swatch 20% more red, 10% less green, and 10% more blue. You should write this web app in pure JS, HTML, and CSS if possible. 


## Interviewer Strategy Guide

* This problem involves knowing how CSS represents colors with 6-digit hex strings, as well as converting between base 10 and base 16.

* A CSS color string (eg, `#F3A256`) actually is a concatenation of 3 2-digit hex strings ranging from 0 to 255 (FF in hex), for the Red, Green, and Blue Saturation of a color.

* Prompt interviewee for how they would subtract/add 10% for the G values (10% must be converted to hexadecimal, which is approx. 25)


* How does the interviewee plan to handle edge cases when the RGB value is already at its max or min values (0 or 255)

* `ParseInt()` is a handy method for base conversions!

* Once the logical approach is solidified, how do we wire it together within the callback function of a click handler?

* A few examples for you, as the interviewer, to be aware of:

  * #1AB20D	=> #4C9926

  * #4C9926 => #7E803F

  * #A02BFF => #D212FF

### Starting Point
Share the link to this [JSFiddle Code Playground](https://jsfiddle.net/Lvkcbtq9/), which has the following starting-point code in it:

```html

<body>
  <div id=swatch></div>
  <p>
    You are viewing: <span id='value'></span>
  </p>
  <button id='filter'>Rose Filter</button>
</body>

```

```css

#swatch {
  height: 250px;
  width: 250px;
  border: 2px solid black
}

```

```javascript
let colorStr = '1AB20D';

const swatch = document.getElementById('swatch');
const span = document.getElementById('value')
const roseBtn = document.getElementById('filter');

swatch.style.backgroundColor = span.innerHTML = `#${colorStr}`;

// Your Code Below

```
### Additional questions to ask your interviewee to prompt their thinking:

* How is measuring a number hexadecimal (base-16) different from decimal (base-10). What is 10% in base-16?

* How is a 6-digit hex string constructed? How do those values correspond to a given RGB color value?

* How can we convert numbers from decimal to hex, and vice versa?

* What are some edge cases to consider? What if a color is completely saturated (#FF) or completely unsaturated (#00)?


## Solution

```html

<body>
  <div id=swatch></div>
  <p>
    You are viewing: <span id='value'></span>
  </p>
  <button id='filter'>Rose Filter</button>
</body>

```

```css

#swatch {
  height: 250px;
  width: 250px;
  border: 2px solid black
}

```

```javascript

let colorStr = '1AB20D';

const swatch = document.getElementById('swatch');
const span = document.getElementById('value')
const roseBtn = document.getElementById('filter');


swatch.style.backgroundColor = span.innerHTML = `#${colorStr}`;

const colorize = () => {
  // slice each 2-digift color hex val, convert to base-10;
  // NB: 255 === FF in hex
  let redVal = (parseInt(colorStr.slice(0, 2), 16));
  let greenVal = (parseInt(colorStr.slice(2, 4), 16));
  let blueVal = (parseInt(colorStr.slice(4, 6), 16));

  // adjust values to be more rose, within (0,255) range
  // NB: 25 == 10% of 255 (FF) (NOTE: THIS IS AN ESTIMATION. 10% OF 255 IS 25.5, BUT 25 IS EASIER TO WORK WITH)
  redVal < 205 ? redVal += 50 : redVal = 255; 
  greenVal > 24 ? greenVal -= 25 : greenVal = 0;
  blueVal < 230 ? blueVal += 25 : blueVal = 255;
  
  //convert back to hex && add buffer 0 to single-digit strings
  const convertAndAddBuffer = val => val.toString(16).length === 1 ? '0' + val.toString(16) : val.toString(16);

  colorStr = [redVal, greenVal, blueVal].map(convertAndAddBuffer).join('');
  swatch.style.backgroundColor = span.innerHTML = `#${colorStr.toUpperCase()}`;

};

roseBtn.addEventListener('click', colorize));

```

### Resources

* [JSFiddle Solution Code](https://jsfiddle.net/d1oescjn/)
* [Slides](https://docs.google.com/presentation/d/1egDcITOpw6DkRkgxdg5XmSlP7fegPQz2sT4X93nqwZ8/edit#slide=id.gbe1461736e_0_13)
* [parseInt Documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseInt)
* [YouTube Video About Hexadecimal And Why We Use It](https://www.youtube.com/watch?v=bOl7jGfossI)
