#***mojule***  
#mojule.Color v1.01

A comprehensive JavaScript color class.

Part of [***mojule***][0]  
Copyright 2011, [Information Age Ltd][1]  
Licensed under the [MIT License][2]

##Features

* Standalone, no dependencies
* 4.02KB minified + gzipped
* Create from and convert to multiple color formats, rgb, hsl, css values etc.
* Add additional color formats via plug-ins
* Get information about colors, red component, hue, lightness, brightness etc.
* Manipulate colors in a number of ways
* Check color contrast, get another color that contrasts with given color etc.
* Generate a range of colors

##Quick Start

Quick demo of most features:

``` js
var colors = [
  new Color( '#39f' ),
  new Color( 'rgba( 51, 153, 255, 0.5' ),
  new Color({ red: 51, green: 154, blue: 255, alpha: 0.5 }),
  new Color({ hue: 210, saturation: 100, lightness: 50, alpha: 0.5 })
];

color[ 0 ].red( 255 ).alpha( 0.5 );

alert( color[ 0 ].red() );
alert( color[ 0 ].css() );

var hsla = color[ 0 ].hsla();
var rgba = color[ 0 ].rgba();
var cloned = color.clone();

var rainbow = color.Range({ hue: 0, saturation: 100, lightness: 50 }, { hue: 338 }, 7 );

var darker = [];
for( var i = 0; i < colors.length; i++ ) {
  darker.push( colors[ i ].clone().multiplyHsla({ lightness: 0.75 }) );
}

var textColor = new Color( '#39f' );
var backgroundColor = textColor.getContrastingTone();
    
var colorArr = [ 51, 153, 255, 0.5 ];
if( !Color.canParse( colorArr ) ) {
  var arrayParser = {
    predicate:  function( value ) {
                  return value instanceof Array && value.length > 0;
                },
    parse:  function( value, color ) {
              var rgba = {};
              rgba[ 'red' ] = value[ 0 ];
              if( value.length > 1 ) {
                rgba[ 'green' ] = value[ 1 ];
              }
              if( value.length > 2 ) {
                rgba[ 'blue' ] = value[ 2 ];
              }
              if( value.length > 3 ) {
                rgba[ 'alpha' ] = value[ 3 ];
              }
              return color.rgba( rgba );
            }
  };
  Color.addParser( 'array', arrayParser );  
}
var colorFromArr = new Color( colorArr );
```    
    
##Usage

An alias called `Color` is created if doing so does not conflict with an 
existing variable, otherwise the color class is accessed via `mojule.Color`.

##Creating a color

Basic usage:

    var color = new Color( value );

If you do not pass a value the returned color will be `#000`, black.
    
The constructor will accept colors in a number of formats:

##Creating colors from CSS Colors

As defined by [W3][3]

Examples:
  
``` js
var hex3 = new Color( '#39f' );    
var hex6 = new Color( '#3399ff' );    
var rgb = new Color( 'rgb( 51, 153, 255 )' );    
var rgbPercent = new Color( 'rgb( 20%, 60%, 100% )' );    
var rgba = new Color( 'rgba( 51, 153, 255, 0.5 )' );    
var rgbaPercent = new Color( 'rgba( 20%, 60%, 100%, 0.5 )' );    
var hsl = new Color( 'hsl( 210, 100%, 60% )' );    
var hsla = new Color( 'hsl( 210, 100%, 60%, 0.5 )' );    
var named = new Color( 'dodgerblue' );
```
    
##Creating colors from objects

* RGB values are in the range `[0..255].` Values outside of this range will be clamped so that they fall within it.
* Alpha is in the range `[0..1].` Values outside of this range will be clamped so that they fall within it.
* Hue represents an angle in the range `[0..360]`. Values outside of this range will wrap, that is, `540` will become `180`, `-180` will become `180` etc.
* Saturation is in the range `[0..100]`. Values outside of this range will be clamped so that they fall within it.
* Lightness is in the range `[0..100]`. Values outside of this range will be clamped so that they fall within it.

The initial values of a color are: 

``` js
{
  red: 0, 
  green: 0, 
  blue: 0,
  hue: 360,
  saturation: 0,
  lightness: 0,
  alpha: 1
}
```
    
If you pass a partial object to the constructor, any properties omitted from 
that object will retain their initial values.

Examples:  

``` js
var redGreenBlue = new Color({ red: 51, green: 153, blue: 255 });    
var rgb = new Color({ r: 51, g: 153, b: 255 });    
var hueSaturationLightness = new Color({ hue: 210, saturation: 100, lightness: 60 });    
var hsl = new Color({ h: 210, s: 100, l: 60 });    
var redGreenBlueAlpha = new Color({ red: 51, green: 153, blue: 255, alpha: 0.5 });    
var rgba = new Color({ r: 51, g: 153, b: 255, a: 0.5 });    
var hueSaturationLightnessAlpha = new Color({ hue: 210, saturation: 100, lightness: 60, alpha: 0.5 });    
var hsla = new Color({ h: 210, s: 100, l: 60, a: 0.5 });    
```

##Creating colors from other colors

You also can pass the constructor an existing `Color`.

Example:

``` js
var oldColor = new Color( '#39f' );
var newColor = new Color( oldColor );
```

This is essentially the same as `clone()` (discussed later):

``` js
var oldColor = new Color( '#39f' );
var newColor = oldColor.clone();
```

##Getting information about a color

These functions tell you something about a color without changing it. They all
require you to create an instance of a color first.

###.red()

Returns the red component of the color. 

Example:

``` js
var color = new Color( '#39f' );
var red = color.red(); //value will be 51
```

**Note**: If you pass a value to this function it will set the value instead of 
getting it and *will not return the value but an instance of the current 
color*, see `Modifying functions` for the rationale behind this.
    
###.green()

Returns the green component of the color

Example:

``` js
var color = new Color( '#39f' );
var green = color.green(); //value will be 153
```

**Note**: If you pass a value to this function it will set the value instead of 
getting it and *will not return the value but an instance of the current 
color*, see `Modifying functions` for the rationale behind this.
    
###.blue()

Returns the blue component of the color

Example:

```js
var color = new Color( '#39f' );
var blue = color.blue(); //value will be 255 
```

**Note**: If you pass a value to this function it will set the value instead of 
getting it and *will not return the value but an instance of the current 
color*, see `Modifying functions` for the rationale behind this.
  
###.hue()

Returns the hue component of the color

Example:

```js
var color = new Color( '#39f' );
var hue = color.hue(); //value will be 210 
```

**Note**: If you pass a value to this function it will set the value instead of 
getting it and *will not return the value but an instance of the current 
color*, see `Modifying functions` for the rationale behind this.
    
###.saturation()

Returns the saturation component of the color

Example:

```js
var color = new Color( '#39f' );
var saturation = color.saturation(); //value will be 100   
```

**Note**: If you pass a value to this function it will set the value instead of 
getting it and *will not return the value but an instance of the current 
color*, see `Modifying functions` for the rationale behind this.
    
###.lightness()

Returns the lightness component of the color

Example:

```js
var color = new Color( '#39f' );
var lightness = color.lightness(); //value will be 50
```

**Note**: If you pass a value to this function it will set the value instead of 
getting it and *will not return the value but an instance of the current 
color*, see `Modifying functions` for the rationale behind this.

###.alpha()

Returns the alpha component of the color

Example:

``` js
var color = new Color( 'rgba( 51, 153, 255, 0.5 )' );
var alpha = color.alpha(); //value will be 0.5
```

**Note**: If you pass a value to this function it will set the value instead of 
getting it and *will not return the value but an instance of the current 
color*, see `Modifying functions` for the rationale behind this.

###.rgba()

Returns an object describing the color in the rgba color space, and accompanying
css for the object.

Example:

``` js
var color = new Color( 'rgba( 51, 153, 255, 0.5 )' );
var rgba = color.rgba(); 
```

Return value:

``` js
{
  red: 51,
  green: 153,
  blue: 255,
  alpha: 0.5,
  css: 'rgba( 51, 153, 255, 0.5 )'
}     
```

**Note**: If you pass a value to this function it will set the value instead of 
getting it and *will not return the value but an instance of the current 
color*, see `Modifying functions` for the rationale behind this.

###.rgb()

An alias for `.rgba()`

###.hsla()

Returns an object describing the color in the hsla color space, and accompanying
css for the object.

Example:

``` js
var color = new Color( 'rgba( 51, 153, 255, 0.5 )' );
var hsla = color.hsla(); 
```

Return value:

``` js
{
  hue: 210,
  saturation: 100,
  lightness: 50,
  alpha: 0.5,
  css: 'hsla( 210, 100%, 50%, 0.5 )'
}  
```

**Note**: If you pass a value to this function it will set the value instead of 
getting it and *will not return the value but an instance of the current 
color*, see `Modifying functions` for the rationale behind this.
     
###.css()

Returns a string representing the css value for the color.  If the color has no
alpha (alpha is set to `1.0`) then a hex value will be returned, otherwise an
rgba value.

Example:

``` js
var color = new Color( 'rgba( 51, 153, 255 )' );
var css = color.css(); //value will be '#39f'    

var alpha = new Color( 'rgba( 51, 153, 255, 0.5 )' );
var alphaCss = alpha.css(); //value will be 'rgba( 51, 153, 255, 0.5 )'
```
    
###.hex()

Returns a hex representation of the color. The main difference between this and
`.css()` is that `.hex()` will always drop the alpha channel regardless of its
value.

Example:

``` js
var color = new Color( 'rgba( 51, 153, 255, 0.5 )' );
var css = color.hex(); //value will be '#39f'    
```

**Note**: If you pass a value to this function it will set the value instead of 
getting it and *will not return the value but an instance of the current 
color*, see `Modifying functions` for the rationale behind this.    
    
###.brightness()

Gets the relative brightness of the current color. This differs from lightness 
in that brightness takes into account that the human eye sees red, green and 
blue differently and adjusts accordingly. The return value will be in the range
`[0.255]`.

Example:

``` js    
var color = new Color( '#39f' );
var brightness = color.brightness(); //value will be 134.13    
```

###.brightnessDifference( compareValue )

Get the difference in brightness between the current color and the color passed
in. `compareValue` can be any value that would be accepted by the constructor,
as outlined in **Creating a color** above. 

Example:

``` js
var color = new Color( '#39f' );
var diff = color.brightnessDifference( '#fff' ); //value will be 120.87
```

###.colorDifference( compareValue )

Gets the difference in color between the current color and the color passed in.
`compareValue` can be any value that would be accepted by the constructor,
as outlined in **Creating a color** above.

Example:

``` js
var color = new Color( '#39f' ); 
var diff = color.colorDifference( '#f90' ); //value will be 459
```

###.hasEnoughContrast( compareValue, colorThreshold, brightnessThreshold )

Determines if the current color contrasts well enough against `compareValue` to be
easily readable by a wide range of people. `colorThreshold` and 
`brightnessThreshold` are optional and if omitted default to the [W3 recommendations][4]
of `500` and `125` respectively.

Example:

``` js  
var color = new Color( '#39f' );
var enough = color.hasEnoughContrast( '#f90' ); //will be false    
var enoughForced = color.hasEnoughContrast( '#f90', 450, 30 ); //will be true
```

###.getContrastingTone()

Returns either black or white depending on which would be more readable when 
text is drawn in the current color on a black/white background or black/white
text is drawn on a background of the current color. Use to ensure that text is 
always readable. Alternatively if you want something other than black/white you 
can test two colors yourself using `.hasEnoughContrast()`. Future versions of
`mojule.Color` will enhance and automate this process.

Example:
  
``` js
var background = new Color( '#39f' );
var text = background.getContrastingTone(); //value will be '#fff'
```
    
##Modifying functions

These functions allow you to change an instance of a color in various ways.
Each of these functions returns an instance of Color so you can chain methods,
for example:

``` js
var color = new Color( '#39f' ).hue( 40 ).alpha( 0.5 );
```
      
###.red( value )

Sets the red component of the current color and returns an instance of the
current color to allow for method chaining.

Example: 

``` js
var color = new Color( '#39f' );
color.red( 255 ); //color is now '#f9f'
```

###.green( value )

Sets the green component of the current color and returns an instance of the
current color to allow for method chaining.

Example: 

``` js
var color = new Color( '#39f' );
color.green( 255 ); //color is now '#3ff'
```

###.blue( value )

Sets the blue component of the current color and returns an instance of the
current color to allow for method chaining.

Example:

``` js 
var color = new Color( '#39f' );
color.blue( 0 ); //color is now '#390'
```

###.hue( value )

Sets the hue component of the current color and returns an instance of the
current color to allow for method chaining.

Example:

``` js 
var color = new Color( '#f00' );
color.hue( 210 ); //color is now '#007fff'
```

###.saturation( value )

Sets the saturation component of the current color and returns an instance of the
current color to allow for method chaining.

Example: 

``` js
var color = new Color( '#f00' );
color.saturation( 50 ); //color is now '#bf4040'
```

###.lightness( value )

Sets the lightness component of the current color and returns an instance of the
current color to allow for method chaining.

Example: 

``` js
var color = new Color( '#f00' );
color.lightness( 25 ); //color is now '#800000'
```

###.alpha( value )

Sets the alpha component of the current color and returns an instance of the
current color to allow for method chaining.

Example: 

``` js
var color = new Color( '#f00' );
color.alpha( 25 ); //color is now 'rgba( 255, 0, 0, 0.5 )'
```

###.rgb( value )

Alias for `.rgba( value )`.

###.rgba( value )

Sets the red, green and blue components of the color from the passed object.
The object must have some or all of the properties `red`, `green`, `blue` and 
`alpha` or some of all of the properties `r`, `g`, `b` and `a`. You can pass 
partial objects through and only the values passed will be set. Returns an 
instance of the current color to allow for method chaining.

Examples:

``` js
var color = new Color( '#000' );
color.rgba({ red: 51, green: 153, blue: 255 }); //value is '#39f'
    
var color = new Color( '#000' );
color.rgba({ red: 255, green: 153 }); //value is '#f90'
```

###.hsl( value )

Alias for `.hsla( value )`.

###.hsla( value )

Sets the hue, saturation and lightness components of the color from the passed object.
The object must have some or all of the properties `hue`, `saturation`, `lightness` and 
`alpha` or some of all of the properties `h`, `s`, `l` and `a`. You can pass 
partial objects through and only the values passed will be set. Returns an 
instance of the current color to allow for method chaining.

Examples:

``` js
var color = new Color( '#000' );
color.hsla({ hue: 210, saturation: 100, lightness: 50 }); //value is '#39f'
    
var color = new Color( '#f00' );
color.hsla({ hue: 210, alpha: 0.5 }); //value is 'rgba( 0, 127, 255, 0.5 )'
```

###.hex( value )

Sets the red, green and blue components of the color to those specified in the 
hex string. Note that because hex strings do not have alpha information, the 
existing alpha component of the color will be unchanged. Returns an instance of 
the current color to allow for method chaining.

Examples:

``` js
var color = new Color( '#000' );
color.hex( '#3399ff' ); //value is '#39f'
color.hex( '#f00' ); //value is '#f00'

var color = new Color({red: 255, green: 0, blue: 0, alpha: 0.5});
color.hex( '#39f' ); //value is 'rgba( 51, 153, 255, 0.5 )'
```
    
###.multiplyHsla( modifiers )

Modify the hue, saturation, lightness and alpha components of an image by 
multiplying the values together with the values specified in modifiers.
`modifiers` is an object that must have some or all of the properties `hue`, 
`saturation`, `lightness` and `alpha` or some of all of the properties `h`, `s`, 
`l` and `a`. You can pass partial objects through and only the values passed 
will be modified. Returns an instance of the current color to allow for method 
chaining.

Examples:

``` js
var color = new Color( '#39f' );
color.multiplyHsla({
  hue: 0.5, 
  saturation: 0.5, 
  lightness: 0.5, 
  alpha: 0.5
}); //value is rgba( 57, 115, 38, 0.5 )
```
        
###.sumHsla( modifiers )
 
Modify the hue, saturation, lightness and alpha components of an image by 
summing the values together with the values specified in modifiers.
`modifiers` is an object that must have some or all of the properties `hue`, 
`saturation`, `lightness` and `alpha` or some of all of the properties `h`, `s`, 
`l` and `a`. You can pass partial objects through and only the values passed 
will be modified. Returns an instance of the current color to allow for method 
chaining.

Examples:

``` js
var color = new Color( '#39f' );
color.sumHsla({
  hue: -105, 
  saturation: -50, 
  lightness: -30, 
  alpha: -0.5
}); //value is rgba( 57, 115, 38, 0.5 )   
```
    
##Miscellaneous instance functions    

###.clone()

Returns a copy of a color that can be modified without affecting the original
color.

Examples:

``` js
var color = new Color( '#39f' );
var alpha = color.clone().alpha( 0.5 ); //value is 'rgba( 51, 153, 255, 0.5 )'
```    
    
##Static functions

These functions can be called without creating an instance of a color.

###.range( start, end, steps )

Produces an array of colors with a length of steps where the first item equals 
the start color, the last item equals the end color and the intermediate items 
are interpolated values in between. 

`start` and `end` can be any color format accepted by the constructor and 
`steps` is an integer.

You can pass a partial value for end and the  undefined properties will be taken 
from start, so for example to create a range where just the hue gets 
interpolated you could call it thusly:

``` js
var start = {
  hue: 0,
  saturation: 100,
  lightness: 50
}
var end = {
  hue: 338
}
var hueRange = Color.range( start, end, 16 );
```

###.canParse( value )

Returns a boolean indicating whether or not the Color class can parse the given
value.

Examples:

``` js
var canParseHex = Color.canParse( '#39f' ); //value is true
var canParseArray = Color.canParse( [ 51, 153, 255 ] ); //value is false
```
    
###.parserType( value )

Returns a string indicating the type of parser used to handle the given value.
The built in types are 'undefinedValue', 'cssHex', 'cssRgb', 'cssRgbPercent', 
'cssRgba', 'cssRgbaPercent', 'cssHsl', 'cssHsla', 'cssNamedColor', 'color', 
'rgba', 'hsla'. The method may also return a user defined parser if one has been
added - see `.addParser()` below. Returns undefined if no parser can handle the
value.

Examples:

``` js
    var parserType = Color.parserType( '#39f' ); //value is 'cssHex'
    var parserType2 = Color.parserType({red: 255}); //value is 'rgba'
    var parserType3 = Color.parserType( 0 ); //value is 'undefinedValue'
```
    
###.addParser( name, parser )

Adds a user-defined parser to handle other color formats. `name` should be a 
descriptive unique identifier for the parser. `parser` should be an object like:

``` js
{
  predicate: function( value ){ 
                // Return a boolean representing whether this parser 
                // handles given value
             },
  parse: function( value, color ) {
           // Set the value on color then return color so that methods 
           // can be chained
         }
}
```
    
Example:

``` js
var colorArr = [ 51, 153, 255, 0.5 ];
var canParse = Color.canParse( colorArr ); //false
var arrayParser = {
  predicate:  function( value ) {
                return value instanceof Array && value.length > 0;
              },
  parse:  function( value, color ) {
            var rgba = {};
            rgba[ 'red' ] = value[ 0 ];
            if( value.length > 1 ) {
              rgba[ 'green' ] = value[ 1 ];
            }
            if( value.length > 2 ) {
              rgba[ 'blue' ] = value[ 2 ];
            }
            if( value.length > 3 ) {
              rgba[ 'alpha' ] = value[ 3 ];
            }
            return color.rgba( rgba );
          }
};
Color.addParser( 'array', arrayParser );
var canParseNow = Color.canParse( colorArr ); //true
var colFromArray = new Color( arrValues ); //value is 'rgba( 51, 153, 255, 0.5 )'
```

###.removeParser( name )
Removes the named parser. Only works on user-defined parsers.

Example:

``` js
var colorArr = [ 51, 153, 255, 0.5 ];
var canParse = Color.canParse( colorArr ); //false
var arrayParser = {
  /* ... */
};
Color.addParser( 'array', arrayParser );
var canParseNow = Color.canParse( colorArr ); //true  
Color.removeParser( 'array' );
var howAboutNow = Color.canParse( colorArr ); //false
```
    
[0]: http://mojule.co.nz/    
[1]: http://informationage.co.nz/
[2]: http://www.opensource.org/licenses/mit-license.php
[3]: http://www.w3.org/TR/css3-color/#colorunits
[4]: http://www.w3.org/TR/2000/WD-AERT-20000426#color-contrast