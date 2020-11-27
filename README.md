# Sweet Home 3D Online (SH3DO) - Getting Started

This git repository gathers the documentation and resources to get started with embeding a pure HTML5/JavaScript plan editor / 3D view into your own Web Site.

> _NOTE:_ The current version is still beta and may require small adjustments and changes in the future. All will be done to keep the initial API intact and unlikely breaking changes will be carefuly notified to prod users by email.

## Purpose

The purpose of SH3DO is to provide an easy-to-install and easy-to-use SaaS for 3D-powered plan edition, floor planning, and so on. SH3DO provides a REST API, so that it is easy to use it independently from the programming language.

SH3DO comes with a free 'starter' mode to enable plan editing and 3D visualization in your web site. As explained below, you can also try out embeding locally for development purpose.

## Getting Started

To embed a plan editor and 3D view in your website, make sure you include the following resources in the head of your pages:

```html
<html>
  <head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="minimal-ui, user-scalable=no, initial-scale=1, maximum-scale=1, minimum-scale=1, width=device-width">
    <meta name="format-detection" content="telephone=no">
    <meta name="msapplication-tap-highlight" content="no">
    <title>SH3DO demo: edit an empty file</title>
    <script type="text/javascript" src="https://demo.sh3d.online/lib/big.min.js" crossorigin="anonymous"></script>
    <script type="text/javascript" src="https://demo.sh3d.online/lib/gl-matrix-min.js" crossorigin="anonymous"></script>
    <script type="text/javascript" src="https://demo.sh3d.online/lib/jszip.min.js" crossorigin="anonymous"></script>
    <script type="text/javascript" src="https://demo.sh3d.online/lib/jsXmlSaxParser.min.js" crossorigin="anonymous"></script>
    <script type="text/javascript" src="https://demo.sh3d.online/lib/core.min.js" crossorigin="anonymous"></script>
    <script type="text/javascript" src="https://demo.sh3d.online/lib/geom.min.js" crossorigin="anonymous"></script>
    <script type="text/javascript" src="https://demo.sh3d.online/lib/stroke.min.js" crossorigin="anonymous"></script>
    <script type="text/javascript" src="https://demo.sh3d.online/lib/swingundo.min.js" crossorigin="anonymous"></script>
    <script type="text/javascript" src="https://demo.sh3d.online/lib/batik-svgpathparser.min.js" crossorigin="anonymous"></script>
    <script type="text/javascript" src="https://demo.sh3d.online/lib/triangulator.min.js" crossorigin="anonymous"></script>
    <script type="text/javascript" src="https://demo.sh3d.online/lib/graphics2d.min.js" crossorigin="anonymous"></script>
    <script type="text/javascript" src="https://demo.sh3d.online/lib/sweethome3d.min.js" crossorigin="anonymous"></script>
    <link rel="stylesheet" type="text/css" href="https://demo.sh3d.online/css/sweethome3d.css" crossorigin="anonymous">
    <script src="https://code.jquery.com/jquery-3.5.1.min.js" integrity="sha256-9/aliU8dGd2tb6OSsuzixeV4y/faTqgFtohetphbbj0=" crossorigin="anonymous"></script>
  </head>
``` 

Then, you need the following elements in your document's body:

```html
  <body>
    <div id="home-pane">
      <canvas id="home-3D-view" tabIndex="1"></canvas>
      <div id="home-plan" tabIndex="2" style="background: white"></div>
      <div id="home-pane-toolbar"></div>
      <div id="furniture-catalog"></div>
    </div>
```

Finally, the JavaScript code to be added at the end of the page:

```html
    <script type="text/javascript">
var homeName = "emptyHome";
var urlBase = "https://demo.sh3d.online/";
var application = new SweetHome3DJSApplication(
    {readHomeURL:       urlBase + "api/readHome/12345/test/%s",
     furnitureCatalogURLs: [urlBase + "lib/resources/DefaultFurnitureCatalog.json"],
     furnitureResourcesURLBase: urlBase
    });
application.getUserPreferences().setAerialViewCenteredOnSelectionEnabled(true);
application.getHomeRecorder().readHome(homeName, 
    {
      homeLoaded: function(home) {
        home.setName(homeName);
        application.addHome(home);
        window.addEventListener("unload", function() {
            application.deleteHome(home);
          });
      },
      homeError: function(err) {},
      progression: function(part, info, percentage) {}
    });

    </script>
  </body>
</html>
```

Run your website on localhost:8000 (as explained hereafter) to get the following result:

![SH3DO screenshot](https://github.com/cincheo/sh3do-getting-started/blob/main/screenshot.png?raw=true)

For a quickstart and more examples, you can try out the demo service, just by using the given demos provided in the demos directory. 

- Clone this repo
- In a console, cd to the repo directory
- Start a local web server running on 8000 (for instance with Python 2: ``python -m SimpleHTTPServer``)
- Test by opening ``http://localhost:8000/demos/demo0.html`` in a browser, to open a defautl sh3d file

> :warning: For security reasons, the demo server will only work when accessed from a site that is running on localhost:8000. In order to run the examples from another origin (for instance, your own domain, then you need a retailer API key and you need to contact us for getting one).

You can also try out the other demos in the demos directory (also available through https://www.sweethome3d.online). Here is the list of all available demos.

- [demos/demo0.html](demos/demo0.html): opens a default simple plan, demonstrating various capabilities of SH3DO to edit and visualize (3D) plans.
- [demos/demo1.html](demos/demo1.html): opens a full editor for a home.
- [demos/demo2.html](demos/demo2.html): opens a 3D real-time interactive showroom visit.
- [demos/demo3.html](demos/demo3.html): opens a 3D real-time museum-like interactive visit.
- [demos/demo4.html](demos/demo4.html): opens a 2D floor planing map of a hotel (located in the French Alps).
- [demos/demo5.html](demos/demo5.html): opens a 2D/3D outdoor landscaping editor.
- [demos/demo6.html](demos/demo6.html): opens a empty plan editor with the ability to download the images of the plans.

## Moving to prod

Convinced with the demos? Now you can embed SH3DO into your actual production website. Please visit [SH3DO](https://www.sweethome3d.online) to carefully select a plan that fits your need and contact CINCHEO to get the corresponding API key. 

Not sure yet how it works or what it can do? Read more in the documentation section below and contact CINCHEO @[SH3DO](https://www.sweethome3d.online) if you have specific questions. Please take into account that this service is still beta and that the documentation will be improved in a close future.

Not sure yet if it can work for your use case? Take into account that CINCHEO also provides development services to customize SH3DO to any business requirements. Please don't hesitate to contact CINCHEO @[SH3DO](https://www.sweethome3d.online) in order to discuss it.

## Documentation

Documentation is available [here](docs/sh3d.online.pdf) ([online version](docs/sh3d.online.md)).

## History and Licensing Note

SH3DO is built on the top of SweetHome3D Open Source project, which is available on SourceForge. However, SH3DO is *not* Open Source. 

SH3DO is born from a collaboration between CINCHEO (Renaud Pawlak) and eTeks (Emmanuel Puybaret). CINCHEO, creator and owner of [JSweet.org](http://www.jsweet.org), helped eTeks to translate the Java version of Sweet Home 3D to JavaScript (partly thanks to the JSweet transpiler). The result of this work is fully Open Source and is available in the SweetHome3DJS project (see www.sweethome3d.com for more details and access the source code on sourceforge.net).

Thanks to an exclusive partnership between CINCHEO and eTeks, SH3DO leverages SweetHome3D and SweetHome3DJS to make it SaaS and make it available to anyone with minimal development efforts.
