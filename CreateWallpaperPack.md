

# Introduction #

WeatherPaper supports custom wallpaper packs.

# Creating a Custom Wallpaper Pack #

## wallpapers.xml ##

This is the file that tells WeatherPaper which images are included in the pack and to what weather conditions they correspond.

The first line is always the XML declaration. Following that is an optional information section. Title, author and url should each be on a single line only. It is preferred that the  identifiers, "@title", "@author", "@url" should contain only lowercase letters.

```
<?xml version="1.0" encoding="UTF-8"?>
<!--
@title Tango Wallpaper Pack
@author Steven Nichols & Tango Desktop Project
@url http://tango.freedesktop.org/Tango_Desktop_Project
//-->
```

The next section links wallpaper images with their respective weather conditions. All image declarations should be contained within the "`<wallpapers>`" tag.

```
<wallpapers>
  <!-- Image information will go here //-->
</wallpapers>
```

Each image has the following basic structure:
```
<image codes="<!-- condition codes //--->">
  <file><!-- filename //--></file>
</image>
```

The filename is just the name of the image file, such as, "clear-sky.jpg".

Codes is a comma separated list of weather condition codes. For example, "32" is a sunny day and "37" is isolated thunderstorms. The full list of possible code value may be found at http://developer.yahoo.com/weather/#codes

You should **always** include at least one image with the special error code, "-1". This is primarily used when a connection to the Internet cannot be established. This is different from "3200", which is used when no image is defined for the current condition.

Putting it all together:
```
<?xml version="1.0" encoding="UTF-8"?>
<!--
@title Tango Wallpaper Pack
@author Steven Nichols & Tango Desktop Project
@url http://tango.freedesktop.org/Tango_Desktop_Project
//-->
<wallpapers>
    <!-- Stormy weather //-->
    <image codes="0,1,2,3,9,11,12,37,38,39,40,45,47">
        <file>storm.jpg</file>
    </image>
    <!-- All other weather //-->
    <image codes="3200">
        <file>unknown.jpg</file>
    </image>
    <!-- Errors, such as connection problems //-->
    <image codes="-1">
        <file>error.jpg</file>
    </image>
</wallpapers>
```

## overlay.xml ##

Wallpaper pack designers can also specify exactly what the text overlay should look like. This is defined in the "overlay.xml" file. Once again, the first line is the XML declaration. Overlay information is defined between the "`<overlay>`" tag.

For example:
```
<?xml version="1.0" encoding="UTF-8"?>
<overlay>
    <font name="TimesNewRoman.ttf" align="right" size="12" fill="#fff" border="2" bordercolor="#000">
        <line x="-5" y="-120">"%title%"</line>
        <line>by %author%</line>
        <line></line>
        <line>%condition% %temp%%degree%%unit% (Feels like %feelslike%%degree%%unit%)</line>
        <line>%date%</line>
        <line>Forecast: %forecast%</line>
        
        <errorline x="-5" y="-100">%errormsg%</errorline>
    </font>
</overlay>
```

Line elements should always be enclosed by a font element so the program knows how to display the text. You can use multiple fonts, just remember to include the ".ttf" file for each font in your wallpaper pack.

The line element specifies what text to draw and where to draw it. The positioning is done in terms of x,y-coordinates. Positive values specify a location on the wallpaper using the top-left corner as the origin (e.g. "`<line x="15" y="30">my text</line>`" would draw "my text" starting 15 pixels from the left of edge of the wallpaper and 30 pixels down from the top edge.) Similarly, negative values specify locations from the bottom-right corner.

The x,y-coordinates may be omitted for subsequent lines. This causes each additional line to be placed immediately below the previous line. Additionally, if an empty line element is used, e.g. "`<line></line>`", a blank line will be drawn.

The errorline element is a special line element which is displayed instead of the normal overlay when an error occurs.

The following table shows all the variables that can be included in the overlay:

| **Variable** | **Description** |
|:-------------|:----------------|
| %author% | The author of the current wallpaper image |
| %condition% | Plain-text description of the current conditions, e.g., "Partly Cloudy" |
| %date% | The datetime stamp of the last update |
| %degree% | The degree symbol|
| %errormsg% | The error message, e.g. "Could not connect" |
| %forecast% | Plain-text description of the forecast for the rest of the day |
| %feelslike% | Heat index or wind chill |
| %temp% | Current temperature, e.g. "86" |
| %title% | The title of the current wallpaper image |
| %unit% | The temperature unit abbreviation, e.g., "F" |


# Example #

Lets say I found a pack of high-resolution weather icons in the public domain. I would like to use these as the images for a new wallpaper pack.

The images are: "sunny.jpg", "cloudy.jpg", "storm.jpg", "unknown.jpg". I will create a new folder, "Tango", and copy all my images into it. Next I create two new documents, "wallpapers.xml" and "overlay.xml" in that same directory:

```
Tango
   |
   +--sunny.jpg
   |
   +--cloudy.jpg
   |
   +--storm.jpg
   |
   +--unknown.jpg
   |
   +--storm.jpg
   |
   +--wallpapers.xml
   |
   +--overlay.xml
```

Following the guidelines for wallpapers.xml, I come up with:
```
<?xml version="1.0" encoding="UTF-8"?>
<!--
@title Tango Wallpaper Pack
@author Steven Nichols & Tango Desktop Project
@url http://tango.freedesktop.org/Tango_Desktop_Project
//-->
<wallpapers>
    <image codes="0,1,2,3,9,11,12,37,38,39,40,45,47">
        <file>storm.jpg</file>
    </image>
    <image codes="31,32,36">
        <file>sunny.jpg</file>
    </image>
    <image codes="27,28,29,33,34,44">
        <file>cloudy.jpg</file>
    </image>
    <image codes="3200">
        <file>unknown.jpg</file>
    </image>
</wallpapers>
```

And likewise, for overlay.xml, I have:
```
<?xml version="1.0" encoding="UTF-8"?>
<overlay>
    <font file="AlteHaasGroteskBold.ttf" align="right" size="72" fill="#444">
        <line x="-30" y="140">%temp%%degree%%unit%</line>
    </font>
    <font file="AlteHaasGroteskBold.ttf" align="right" size="56" fill="#444">
        <line x="-30" y="70">%condition%</line>
    </font>
    
    <font file="AlteHaasGroteskBold.ttf" align="right" size="56" fill="#444">
        <errorline x="-30" y="70">%errormsg%</errorline>
    </font>
</overlay>
```

Since I have chosen to use a non-standard font, "AlteHaasGroteskBold.ttf", I will need to copy the font file into the folder as well.

The final step is to create a ZIP file from my Tango folder. I can now share "Tango.zip" with my friends!

##  ##