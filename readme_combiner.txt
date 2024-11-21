After installing the Nuget package the only thing you need to do is specify which pages 
you would like to apply the compression on. 

In MVC this is achieved by using an attribute [Combine(...)] on any action method 
and passing true/false for the desired options.

There are three constructors that can be used for the mvc attribute (for convenience):

Combiner() => will turn all features to on ( 'PrependCdnHostToImages' in the web.config must be set to some url

Combiner(bool applyOutputCaching) => will turn all features on and you can control the 'applyOutputCaching' feature

Combiner(bool applyOutputCaching, bool combineJs, bool combineCss, bool versionOnly, bool minifyJs, bool minifyCss, bool prependCdnHostToImages)

Note: the 'prependCdnHostToImages' feature will be ignored if the 'imagesCdnHostToPrepend' web.config value is blank.


In Webforms this is achieved by using the server control CombinerWebControl and setting the desired options.

That's all you need to do to get the tool up and running.

/////////////////////////////

This tool uses 
-EntLib for logging errors (errors by default will be logged to errors.txt file, can be changed in logging.config), 
-HtmlAgilityPack for parsing the Dom and 
-Yui.Compressor for compressing the js and css.

/////////////////////////////
There is a combinerSettings.txt file that can be used to turn on/off the various features 'live'
without having to mess with the web.config.

This file can also be used to specify a 'CssVersion' or 'JsVersion' which will fingerprint the combined url 
(append a query to the resource url).
This version setting will also fingerprint the urls or the individual resources even when the tools's 
other settings are turned off, as long as the 'VersionOnly' setting is set to true. This 'VersionOnly' setting
is only applicable when the 'CombineJs' and 'CombineCss' are set to false, and is ignored otherwise (all resources will be versioned).

////////////////////////////
In the web.config's combinerSettings node there is an attribute 'imagesCdnHostToPrepend' 
which can be used to specify a subdomain or cdn url that will be used to replace the
image's root paths (this feature must be set to on in the attribute filter or web control).
See the 'prependCdnHostToImages' attribute filter parameter or the 'PrependCdnHostToImages'
property of the web control.

/////////////////////////////
Important: There are certain scripts that should not be moved to the bottom nor combined, such as 'mbox.js' and it's related inline script that contains
the constructor 'mboxCreate'. These are hardcoded to be excluded from the process. (To do: will add capability to specify a list
of files to be excluded in config file). 

If you do not want a particular inline script to be moved to the bottom of the page then specify an attribute 'nocombine' such as
<script nocombine='true' type='text/javascript'>some script here</script>.

The main jquery file is also not going to be moved or combined. (To do: make this option configurable). I found that this file
is best to be left at the top of the page, in case other scripts that are dynamically loaded depend on it. Also, having this large
file separate will actually help performance since it will be cached by all pages using it, while if combined it will have to be loaded
on different pages that might have a different set of combined scripts.

///////////////////////////
Troubleshooting tips:

- Do not use @import directives inside your css files since they will not work when the css is combined, since
they must be at the top of any css in order to work. Or make sure they appear at the top of the first css on the page.

- Do not use ie specific css directives such as 'filter' since non-IE browsers stop parsing the css file on which
they are declared. Move them to IE specific files and add them to the page inside IE conditional comments such as:
<!--[if IE]><link type='text/css' rel='stylesheet' href='/someFile.css'/><![endif]-->


/////////////////////////////
If you have any questions or suggestions contact me at: jalva01@gmail.com
