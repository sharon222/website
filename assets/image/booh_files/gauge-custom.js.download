//Refrence: http://bernii.github.io/gauge.js/   
var opts = {
         lines: 25, // The number of lines to draw
        angle: 0, // The length of each line
        lineWidth: 0.04, // The line thickness
        //paddingBottom: -10,
        pointer: {
            length: 0.52, // The radius of the inner circle
            strokeWidth: 0.065, // The rotation offset
            color: '#000000' // Fill color
        },
        limitMax: 'false', // If true, the pointer will not go past the end of the gauge
        colorStart: '#6FADCF', // Colors
        colorStop: '#8FC0DA', // just experiment with them
        strokeColor: '#E0E0E0', // to see which ones work best for you
        generateGradient: true,
        percentColors: [[0.0, "#f9c802"], [0.50, "#f9c802"], [1.0, "#f9c802"]]
    };
if(document.getElementById('mycanvas')){
    var target = document.getElementById('mycanvas'); // your canvas element
    var textField = document.getElementById('mycanvas'); // your canvas element
    var gauge = new Gauge(target).setOptions(opts); // create sexy gauge!
    gauge.maxValue = 100; // set max gauge value
    gauge.animationSpeed = 32; // set animation speed (32 is default value)
    gauge.setTextField(document.getElementById('preview'));
    gauge.set(charity_speedo_percent);// set actual value
    }  
/*

var opts = {
  lines: 12, // The number of lines to draw
  angle: 0.35, // The length of each line
  lineWidth: 0.12, // The line thickness
  pointer: {
    length: 1, // The radius of the inner circle
    strokeWidth: 0.065, // The rotation offset
    color: '#000000' // Fill color
  },
  limitMax: 'false',   // If true, the pointer will not go past the end of the gauge
  colorStart: '#6F6EA0',   // Colors
  colorStop: '#C0C0DB',    // just experiment with them
  strokeColor: '#EEEEEE',   // to see which ones work best for you
  generateGradient: true,
  percentColors: [[0.0, "#f9c802"], [0.50, "#f9c802"], [1.0, "#f9c802"]]
};
var target = document.getElementById('mycanvas'); // your canvas element
var textField = document.getElementById('mycanvas');
var gauge = new Donut(target).setOptions(opts); // create sexy gauge!
gauge.maxValue = 100; // set max gauge value
gauge.animationSpeed = 32; // set animation speed (32 is default value)
gauge.setTextField(document.getElementById('preview'));
gauge.set(88); // set actual value*/
