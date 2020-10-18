# Sweet Home 3D Online Documentation


-   [Introduction](#introduction)
-   [<span class="toc-section-number">0.0.1</span> Getting
    started](#getting-started)
-   [<span class="toc-section-number">0.0.2</span> Integration and
    Customization examples](#integration-and-customization-examples)
-   [<span class="toc-section-number">0.0.3</span> API](#api)

### Introduction

Sweet Home 3D™ is a 2D/3D floor planning and plan editor/viewer Open
Source software. It has been developed by Emmanuel Puybaret (eTeks) for
the last 15 years and has a strong and active community.

Sweet Home 3D JS is also Open Source and is the Web version of Sweet
Home 3D. It has been co-developed by Emmanuel Puybaret (eTeks) and
Renaud Pawlak (Cinchéo). A demo version can be found at
<http://www.sweethome3d.com/SweetHome3DOnlineManager.jsp>.

The Sweet Home 3D Online™ product (SH3DO™ for short) allows businesses
to take advantage of Sweet Home 3D and Sweet Home 3D JS. It provides a
ready-to-use server and an Web-Service-based API for easy integration in
any web site. The constraints of the Open Source licensing are relaxed
so that one can fully customize the product for its own constraints and
needs.

SH3DO™ entails with the following characteristics and components:

-   2D plan editor for the Web;

-   3D viewer (in sync with the 2D plan editor);

-   furniture catalog web component;

-   compatibility with Safari, Chrome and IE, including mobile versions;

-   easy ecommerce website integration (pure HTML/Javascript,
    frameworkless);

-   cloud servers for serving the viewer component + reading and saving
    project files (for development, pre-production, and production
    environments);

-   maintenance;

-   backups;

-   an integration and customization API (brand, colors, layout, etc);

-   tools for furniture / object catalog customization.

This document aims at providing the technical documentation to integrate
and customize SH3DO™ in your own Web site. It requires basic Web
programming knowledge. If you do not have programming knowledge in
house, Cinchéo provides development services in order to help you with
SH3DO™ integration and customization.

### Getting started

#### Registration and Retailer Keys

The commercial licensing of Sweet Home 3D Online is brought to you by
Cinchéo / eTeks. This license allows you to tune the Sweet Home 3D
Online product and integrate it in a website. Once you have purchased a
license, you will get a Retailer Key that will allow you to access one
of the available servers (environments).

For each environment, a Retailer Key is bound to a Domain Name (for
instance <https://mycompany.com>), which is the domain that host the web
site you want to integrate SH3DO with. Any request coming from another
origin than the declared domain name will not be allowed.

Note that the communication between your web site and the SH3DO services
is secured with CORS and HTTPS.

#### Environments (dev, preprod, prod)

Your Retailer Key will allow you to securely access the following
environments:

-   Your development environment (<https://dev.sh3d.online>). This
    environment will accept requests with your Retailer Key coming only
    from <http://localhost:8000> or <http://localhost:8080>. It is to be
    used for development and debugging purposes only. Data and files may
    be erased at any time and should be stored for testing purpose.

-   Your pre-production/QA environment (<https://preprod.sh3d.online>).
    This optional environment will accept requests with your Retailer
    Key coming from the origin domain you will have declared to us. It
    is to be used for QA and testing purposes only. Data and files may
    be erased at any time and should be stored for testing purpose.

-   Your production environment (<https://prod.sh3d.online>). This is
    the actual environment, which will accept requests with your
    Retailer Key coming from the origin domain you will have declared to
    us. User data will be safely backed-up and will remain there as long
    as you need it and hold a valid license.

#### The Demo environment

For testing the API without having your Retailer Key available yet, you
may access the demo environment, which is open to everyone and can be
used with the Retailer Key 12345.

The following code shows a typical example to show a full SH3DO plan
editor / 3D view / Furniture catalog using the 12345 Retailer Key on the
demo server. In order to make it work, you need to start a local HTTP
server to serve the file.

Assuming that you have Python installed on your computer, one of the
simplest way is to use a Python HTTP server. Follow the steps:

-   Create an [index.html](index.html) file and copy the HTML content
    below in the file.

-   Open a terminal, go to the folder where you just have created the
    index file.

-   Launch the HTTP server with the command:
    `python -m SimpleHTTPServer`.

-   Open the <http://localhost:8000> with your favorite browser.

<!-- -->

    ]
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="minimal-ui, user-scalable=no, initial-scale=1, maximum-scale=1, minimum-scale=1, width=device-width">
    <meta name="format-detection" content="telephone=no">
    <meta name="msapplication-tap-highlight" content="no">
    <title>SweetHome3DJS Test</title>
    <script type="text/javascript" src="https://demo.sh3d.online/lib/sweethome3d.min.js"></script>
    <link rel="stylesheet" type="text/css" href="https://demo.sh3d.online/css/sweethome3d.css">
    </head>
    <body>

    <div id="home-pane">
      <canvas id="home-3D-view" style="background-color: #CCCCCC;" 
              tabIndex="1"></canvas>
      <div id="home-plan" style="background-color: #FFFFFF; color: #000000"         
              tabIndex="2" ></div>
      <div id="home-pane-toolbar"></div>
      <div id="furniture-catalog"></div>
    </div>

    <script type="text/javascript">
    var homeName = "HomeTest";
    var urlBase = "https://demo.sh3d.online/";
    var application = new SweetHome3DJSApplication(
        {readHomeURL:       urlBase + "api/readHome/12345/test?home=%s",
         writeHomeEditsURL: urlBase + "api/writeHomeEdits/12345/test?home=%s",
         //closeHomeURL:      urlBase + "closeHome.jsp?home=%s",
         pingURL:           urlBase + "api/ping",
         furnitureCatalogURLs: [urlBase + "lib/resources/DefaultFurnitureCatalog.json"],
         furnitureResourcesURLBase: urlBase,
         autoWriteDelay:    1000,
         autoWriteTrackedStateChange: true,
         writingObserver:   {
             writeStarted: function(update) {
               console.info("Update started", update);
             },
             writeSucceeded: function(update) {
               console.info("Update succeeded", update);
             },
             writeFailed: function(update, errorStatus, errorText) {
               console.info("Update failed", update);
             },
             connectionFound: function() {
               console.info("Back to online mode");
             },
             connectionLost: function(errorStatus, errorText) {
               console.info("Lost server connection - going offline");
             }
           }
        });

    // Set a few settings
    application.getUserPreferences().setNewRoomFloorColor(0xFF9999A0);
    application.getUserPreferences().setAerialViewCenteredOnSelectionEnabled(true);

    // Read and display test file 
    application.getHomeRecorder().readHome(homeName, 
        {
          homeLoaded: function(home) {
            home.setName(homeName);
            application.addHome(home);
            
            window.addEventListener("unload", function() {
                application.deleteHome(home);
              });
          },
          homeError: function(err) {
            console.error(err);
          },
          progression: function(part, info, percentage) {
          }
        });
    </script>

    </body>
    </html>

### Integration and Customization examples

One can integrate SH3DO with any web site. Integration requires the
following:

-   Use the REST API to a SH3DO server to connect, create users, create
    and save plans, and so on;

-   Use the JavaScript API to create the components and customize the
    behavior;

-   Use HTML and CSS to customize the page layout, tune the Look and
    Feel, and integrate the SH3DO components in your web site.

#### How to customize the layout

The layout is fully defined in the
[lib/sweethome3d.css](lib/sweethome3d.css) stylesheet. If you want to
tune the default layout, you need to understand the structure of this
file, then create your own CSS rules. You can place the new CSS in the
header of your HTML file, or create a new .css file and link it after
[lib/sweethome3d.css](lib/sweethome3d.css).

As shown in
listing <a href="#lst:basic-example" data-reference-type="ref" data-reference="lst:basic-example">[lst:basic-example]</a>,
each SweetHome3D component has an id. So, the CSS rule will apply to
these:

    [...]
    <div id="home-pane">
      <canvas id="home-3D-view" tabIndex="1"></canvas>
      <div id="home-plan" tabIndex="2" ></div>
      <div id="home-pane-toolbar"></div>
      <div id="furniture-catalog"></div>
    </div>
    [...]

As an example, the following CSS will remove the 3D view (id:
home-3D-view) from the page and make the plan view (id:home-plan) full
page:

    #home-3D-view {
      display: none
    }

    #home-plan {
      height: calc(100% - 16px);
    }

#### How to change the default colors

Similarly to the layout, color can be changed tuning the
[lib/sweethome3d.css](lib/sweethome3d.css) stylesheet. Be aware that CSS
rule can be overridden, especially by local declaration directly in the
HTML style attribute of the elements.

For instance, in order to invert the colors of the plan view (black
background and white foreground), you can change the styling of the plan
view HTML element like this:

    <div id="home-plan" style="background-color: black; color: white"         
              tabIndex="2" ></div>

You can also put it in a CSS file:

    #home-plan {
        background-color: black; 
        color: white;
    }         

#### How to change the default user preferences

You can access the user preferences with JavaScript. The entry point is
the application variable accessible from the root scope (the HTML page).

For example, you can force the 3D aerial view to be centered around the
selection with the following JavaScript line to be added after the
creation of the application object:

    application.getUserPreferences()
            .setAerialViewCenteredOnSelectionEnabled(true);

The full API for the user preferences is available at:
<http://www.sweethome3d.com/javadoc/com/eteks/sweethome3d/model/UserPreferences.html>.

#### How to add a custom button to the toolbar

There are several ways to add a new button to the toolbar. A simple way
to do it is through the following JavaScript function.

    function addButton(id, title, imageUrl, onclick) {
        var toolbar = document.getElementById('home-pane-toolbar');
        toolbar.insertAdjacentHTML('beforeend', "<span class='toolbar-button-group'><button title='"+title+"' class='toolbar-button toggle selected' id='"+id+"' style='background-image: url(&quot;"+imageUrl+"&quot;); background-position: center center; background-repeat: no-repeat;background-color:#fff'></button></span>");
        document.getElementById(id).addEventListener('click', onclick);
    }

You can call this function once the home is created, in the "homeLoaded"
handler of the application:

    [...]
    application.getHomeRecorder().readHome(homeName, 
       {
          homeLoaded: function(home) {
            home.setName(homeName);
            application.addHome(home);

            addButton(
                'custom-button-id', 
                'my new button', 
                'http://demo.sh3d.online/lib/search.gif', 
                function() {
                    alert("hello from my new button");
                });
    [...]

### API

#### General-purpose REST API

##### Endpoint `ping`

This endpoint may be used by the client to check the server
availability.

| Path        | `ping`               |
|:------------|:---------------------|
| HTTP method | GET                  |
| Parameters  | \-                   |
| Result      | status object (JSON) |

##### Endpoint `getRetailer`

This endpoint gets the retailer information for the given retailer key.
Keys are provided to you by CINCHEO. Please contact Renaud Pawlak to get
your own key.

<table>
<thead>
<tr class="header">
<th style="text-align: left;">Path</th>
<th style="text-align: left;"><code>getRetailer/retailerKey</code></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: left;">HTTP method</td>
<td style="text-align: left;">GET</td>
</tr>
<tr class="even">
<td style="text-align: left;">Parameters</td>
<td style="text-align: left;"><ul>
<li><p><code>retailerKey</code> (path parameter): the retailer key</p></li>
</ul></td>
</tr>
<tr class="odd">
<td style="text-align: left;">Result</td>
<td style="text-align: left;"><p>A JSON object containin the following data:</p>
<ul>
<li><p><code>domain</code>: the retailer’s web site domain</p></li>
<li><p><code>email</code>: the retailer’s contact email</p></li>
<li><p><code>key</code>: the retailer’s key</p></li>
<li><p><code>enabled</code>: a flag telling if the retailer’s account is activated</p></li>
<li><p><code>plan</code>: the name of the plan (DEMO | STARTER | BASIC | PROFESSIONAL | PREMIUM)</p></li>
<li><p><code>planExtensionCode1</code>: an optional integer encoding an extension for the retailer’s plan</p></li>
<li><p><code>planExtensionCode2</code>: an optional integer encoding an extension for the retailer’s plan</p></li>
</ul></td>
</tr>
</tbody>
</table>

##### Endpoint `getRetailerStats`

This endpoint gets the retailer statistics for the given retailer key.

<table>
<thead>
<tr class="header">
<th style="text-align: left;">Path</th>
<th style="text-align: left;"><code>getRetailerStats/retailerKey</code></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: left;">HTTP method</td>
<td style="text-align: left;">GET</td>
</tr>
<tr class="even">
<td style="text-align: left;">Parameters</td>
<td style="text-align: left;"><ul>
<li><p><code>retailerKey</code> (path parameter): the retailer key</p></li>
</ul></td>
</tr>
<tr class="odd">
<td style="text-align: left;">Result</td>
<td style="text-align: left;"><p>A JSON object containin the following data:</p>
<ul>
<li><p><code>lastReadHome</code>: the retailer’s web site domain</p></li>
<li><p><code>userCount</code>: the total number of users for this retailer</p></li>
<li><p><code>homeCount</code>: the total number of homes (plans) for this retailer</p></li>
<li><p><code>usedSpace</code>: the total space used by this retailer’s homes (in bytes)</p></li>
<li><p><code>lastReadHome</code>: the date when the last readHome access occurred</p></li>
<li><p><code>readHomeCount</code>: the total count of readHome access since the beginning of the plan</p></li>
<li><p><code>dailyReadHomeCount</code>: the total count of readHome access since the beginning of the current day (or since the lastReadHome date if no accesses)</p></li>
<li><p><code>monthlyReadHomeCount</code>: the total count of readHome access since the beginning of the current month (or since the lastReadHome date if no accesses)</p></li>
<li><p><code>yearlyReadHomeCount</code>: the total count of readHome access since the beginning of the current year (or since the lastReadHome date if no accesses)</p></li>
<li><p><code>lastWriteHome</code>: the date when the last readHome access occurred</p></li>
<li><p><code>writeHomeCount</code>: the total count of writeHome access since the beginning of the plan</p></li>
<li><p><code>writeReadHomeCount</code>: the total count of writeHome access since the beginning of the current day (or since the lastWriteHome date if no accesses)</p></li>
<li><p><code>monthlyWriteHomeCount</code>: the total count of writeHome access since the beginning of the current month (or since the lastWriteHome date if no accesses)</p></li>
<li><p><code>yearlyWriteHomeCount</code>: the total count of writeHome access since the beginning of the current year (or since the lastWriteHome date if no accesses)</p></li>
</ul></td>
</tr>
</tbody>
</table>

#### User-management REST API

Retailers can register and manage users. Without users (or at least a
default user) registered on the server, retailers cannot create new
homes (plans).

##### Endpoint `registerUser`

This endpoint creates a new user for a given retailer on the server.
Once created, a use will have a space on the server to create new homes
(see createNewHome).

<table>
<thead>
<tr class="header">
<th style="text-align: left;">Path</th>
<th style="text-align: left;"><code>registerUser/retailerKey/userId</code></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: left;">HTTP method</td>
<td style="text-align: left;">GET</td>
</tr>
<tr class="even">
<td style="text-align: left;">Parameters</td>
<td style="text-align: left;"><ul>
<li><p><code>retailerKey</code> (path parameter): the retailer key</p></li>
<li><p><code>userId</code> (path parameter): the new user id (must not exist yet)</p></li>
</ul></td>
</tr>
<tr class="odd">
<td style="text-align: left;">Result</td>
<td style="text-align: left;">status object (JSON)</td>
</tr>
</tbody>
</table>

##### Endpoint `getUsers`

This endpoint returns all the user ids (list of strings) that have been
registered for this retailer (see registerUser).

<table>
<thead>
<tr class="header">
<th style="text-align: left;">Path</th>
<th style="text-align: left;"><code>getUsers/retailerKey</code></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: left;">HTTP method</td>
<td style="text-align: left;">GET</td>
</tr>
<tr class="even">
<td style="text-align: left;">Parameters</td>
<td style="text-align: left;"><ul>
<li><p><code>retailerKey</code> (path parameter): the retailer key</p></li>
</ul></td>
</tr>
<tr class="odd">
<td style="text-align: left;">Result</td>
<td style="text-align: left;">a list of user ids as JSON</td>
</tr>
</tbody>
</table>

##### Endpoint `deleteUser`

This endpoint creates a new user for a given retailer on the server.
Once created, a user will have a space on the server to create new homes
(see createNewHome and uploadHome).

<table>
<thead>
<tr class="header">
<th style="text-align: left;">Path</th>
<th style="text-align: left;"><code>deleteUser/retailerKey/userId</code></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: left;">HTTP method</td>
<td style="text-align: left;">GET</td>
</tr>
<tr class="even">
<td style="text-align: left;">Parameters</td>
<td style="text-align: left;"><ul>
<li><p><code>retailerKey</code> (path parameter): the retailer key</p></li>
<li><p><code>userId</code> (path parameter): the new user id (must exist for the given retailer)</p></li>
</ul></td>
</tr>
<tr class="odd">
<td style="text-align: left;">Result</td>
<td style="text-align: left;">status object (JSON)</td>
</tr>
</tbody>
</table>

#### Home-management REST API

Homes are SH3D files that are created for each user registered on the
server. A home is a plan that can be edited (and saved if the plan
allows it) and visualized in 3D.

##### Endpoint `createNewHome`

This endpoint creates a new (empty) home for the given user.

<table>
<thead>
<tr class="header">
<th style="text-align: left;">Path</th>
<th style="text-align: left;"><code>createNewHome/retailerKey/userId/homeName</code></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: left;">HTTP method</td>
<td style="text-align: left;">GET</td>
</tr>
<tr class="even">
<td style="text-align: left;">Parameters</td>
<td style="text-align: left;"><ul>
<li><p><code>retailerKey</code> (path parameter): the retailer key</p></li>
<li><p><code>userId</code> (path parameter): the user id, belonging to the retailer, as provided to registerUser</p></li>
<li><p><code>homeName</code> (path parameter): the home name (should not contain special characters or accents to avoid potential Locale issues)</p></li>
</ul></td>
</tr>
<tr class="odd">
<td style="text-align: left;">Result</td>
<td style="text-align: left;">status object (JSON)</td>
</tr>
<tr class="even">
<td style="text-align: left;">Errors</td>
<td style="text-align: left;"><ul>
<li><p><code>IOException</code>: if home already exists of if a problem occurs while creating the new home file on the server</p></li>
<li><p><code>RecorderException</code>: if Sweet Home 3D recorder fails at creating a new empty home</p></li>
</ul></td>
</tr>
</tbody>
</table>

##### Endpoint `uploadHome`

This endpoint creates a new home for the given user by uploading an SH3D
file.

<table>
<thead>
<tr class="header">
<th style="text-align: left;">Path</th>
<th style="text-align: left;"><code>uploadHome/retailerKey/userId</code></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: left;">HTTP method</td>
<td style="text-align: left;">POST</td>
</tr>
<tr class="even">
<td style="text-align: left;">Parameters</td>
<td style="text-align: left;"><ul>
<li><p><code>retailerKey</code> (path parameter): the retailer key</p></li>
<li><p><code>userId</code> (path parameter): the user id, belonging to the retailer, as provided to registerUser</p></li>
<li><p><code>homeName</code> (form parameter): the home name (should not contain special characters or accents to avoid potential Locale issues)</p></li>
<li><p><code>file</code> (multi-part file form parameter): the home file as provided to the form file upload</p></li>
</ul></td>
</tr>
<tr class="odd">
<td style="text-align: left;">Result</td>
<td style="text-align: left;">status object (JSON)</td>
</tr>
<tr class="even">
<td style="text-align: left;">Errors</td>
<td style="text-align: left;"><ul>
<li><p><code>IOException</code>: if home already exists of if a problem occurs while creating the new home file on the server</p></li>
<li><p><code>RecorderException</code>: if Sweet Home 3D recorder fails at creating a new empty home</p></li>
</ul></td>
</tr>
</tbody>
</table>

##### Endpoint `getHomes`

This endpoint gets all the created homes (plans) for the given user.

<table>
<thead>
<tr class="header">
<th style="text-align: left;">Path</th>
<th style="text-align: left;"><code>getHomes/retailerKey/userId</code></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: left;">HTTP method</td>
<td style="text-align: left;">GET</td>
</tr>
<tr class="even">
<td style="text-align: left;">Parameters</td>
<td style="text-align: left;"><ul>
<li><p><code>retailerKey</code> (path parameter): the retailer key</p></li>
<li><p><code>userId</code> (path parameter): the user id, belonging to the retailer, as provided to registerUser</p></li>
</ul></td>
</tr>
<tr class="odd">
<td style="text-align: left;">Result</td>
<td style="text-align: left;">a list of home names (string) as JSON</td>
</tr>
</tbody>
</table>

##### Endpoint `readHome`

This endpoint reads the given home on the server and downloads it in
SH3D file format. It is to be used by a Sweet Home 3D JavaScript client.

This endpoint comes with a POST and a GET version.

<table>
<thead>
<tr class="header">
<th style="text-align: left;">Path</th>
<th style="text-align: left;"><code>readHome/retailerKey/userId/home</code></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: left;">HTTP method</td>
<td style="text-align: left;">GET</td>
</tr>
<tr class="even">
<td style="text-align: left;">Parameters</td>
<td style="text-align: left;"><ul>
<li><p><code>retailerKey</code> (path parameter): the retailer key</p></li>
<li><p><code>userId</code> (path parameter): the user id, belonging to the retailer, as provided to registerUser</p></li>
<li><p><code>home</code> (path parameter): the home name, as provided to <code>createNewHome</code></p></li>
</ul></td>
</tr>
<tr class="odd">
<td style="text-align: left;">Result</td>
<td style="text-align: left;">a byte stream containing the home file in binary format</td>
</tr>
<tr class="even">
<td style="text-align: left;">Errors</td>
<td style="text-align: left;"><ul>
<li><p><code>IOException</code>: if home does not exist or if a system error occurs while reading the file</p></li>
</ul></td>
</tr>
</tbody>
</table>

<table>
<thead>
<tr class="header">
<th style="text-align: left;">Path</th>
<th style="text-align: left;"><code>writeHomeEdits/retailerKey/userId</code></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: left;">HTTP method</td>
<td style="text-align: left;">POST</td>
</tr>
<tr class="even">
<td style="text-align: left;">Parameters</td>
<td style="text-align: left;"><ul>
<li><p><code>retailerKey</code> (path parameter): the retailer key</p></li>
<li><p><code>userId</code> (path parameter): the user id, belonging to the retailer, as provided to registerUser</p></li>
<li><p><code>homeName</code> (form/header parameter): the home name, as provided to <code>readHome</code></p></li>
<li><p><code>edits</code> (form/header parameter): the list of edits to apply to the home</p></li>
</ul></td>
</tr>
<tr class="odd">
<td style="text-align: left;">Result</td>
<td style="text-align: left;">a result object containing the count of applied edits (JSON)</td>
</tr>
<tr class="even">
<td style="text-align: left;">Errors</td>
<td style="text-align: left;"><ul>
<li><p><code>Throwable</code>: if any of the parameter is wrong or if the edits are not consistent with the target home</p></li>
</ul></td>
</tr>
</tbody>
</table>

##### Endpoint `deleteHome`

This endpoint permanently deletes a home from the server.

<table>
<thead>
<tr class="header">
<th style="text-align: left;">Path</th>
<th style="text-align: left;"><code>deleteHome/retailerKey/userId/home</code></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: left;">HTTP method</td>
<td style="text-align: left;">GET</td>
</tr>
<tr class="even">
<td style="text-align: left;">Parameters</td>
<td style="text-align: left;"><ul>
<li><p><code>retailerKey</code> (path parameter): the retailer key</p></li>
<li><p><code>userId</code> (path parameter): the user id, belonging to the retailer, as provided to registerUser</p></li>
<li><p><code>home</code> (path parameter): the home name, as provided to <code>createNewHome</code></p></li>
</ul></td>
</tr>
<tr class="odd">
<td style="text-align: left;">Result</td>
<td style="text-align: left;">a byte stream containing the home file in binary format</td>
</tr>
<tr class="even">
<td style="text-align: left;">Errors</td>
<td style="text-align: left;"><ul>
<li><p><code>IOException</code>: if home does not exist or if a system error occurs while reading the file</p></li>
</ul></td>
</tr>
</tbody>
</table>

#### JavaScript API

Although there are minor differences and that some elements are not
fully supported, the JavaScript API mainly conforms to the Java API of
SweetHome3D. Please note that the model and controller packages are
automatically generated from Java with the JSweet Java to JavaScript
compiler.

In order to navigate the API, go to
<http://www.sweethome3d.com/javadoc/>. This API will give you a good
starting point to access the application and the underlying home model,
and dynamically interact with is in JavaScript (see the
com.eteks.sweethome3d.controller and com.eteks.sweethome3d.model
packages).

Additionally, you can refer to the following APIs, which have been
generated with jsdoc from the JavaScript source file of SweetHome3DJS.

<div id="classes">

##### Classes

</div>

IncrementalHomeRecorder

SweetHome3DJSApplication

PlanComponent

<div id="incrementalhomerecorder">

##### IncrementalHomeRecorder

</div>

**Kind**: global class  
**Author**: Renaud Pawlak  
**Author**: Emmanuel Puybaret

-   [IncrementalHomeRecorder](#IncrementalHomeRecorder)

    -   [new IncrementalHomeRecorder(application,
        \[configuration\])](#new_IncrementalHomeRecorder_new)

    -   *instance*

        -   [.getTrackedHomeProperties()](#IncrementalHomeRecorder+getTrackedHomeProperties)

        -   [.configure(configuration)](#IncrementalHomeRecorder+configure)

        -   [.readHome(homeName,
            observer)](#IncrementalHomeRecorder+readHome)

        -   [.addHome(home)](#IncrementalHomeRecorder+addHome)

        -   [.removeHome(home)](#IncrementalHomeRecorder+removeHome)

        -   [.sendUndoableEdits(home)](#IncrementalHomeRecorder+sendUndoableEdits)

    -   *static*

        -   [.DEFAULT\_TRACKED\_HOME\_PROPERTIES](#IncrementalHomeRecorder.DEFAULT_TRACKED_HOME_PROPERTIES)

<div id="new-incrementalhomerecorderapplication-configuration">

###### new IncrementalHomeRecorder(application, \[configuration\])

</div>

A home recorder that is able to save the application homes incrementally
by sending undoable edits to the SH3D server.

| Param             | Type                                                  | Description                |
|:------------------|:------------------------------------------------------|:---------------------------|
| application       | [SweetHome3DJSApplication](#SweetHome3DJSApplication) |                            |
| \[configuration\] | Object                                                | the recorder configuration |

<div id="incrementalhomerecorder.gettrackedhomeproperties">

###### incrementalHomeRecorder.getTrackedHomeProperties()

</div>

Gets the home properties that are tracked and potentially written by
this recorder.

**Kind**: instance method of
[IncrementalHomeRecorder](#IncrementalHomeRecorder)  

<div id="incrementalhomerecorder.configureconfiguration">

###### incrementalHomeRecorder.configure(configuration)

</div>

Configures this incremental recorder’s behavior.

**Kind**: instance method of
[IncrementalHomeRecorder](#IncrementalHomeRecorder)

| Param         | Type   | Description                               |
|:--------------|:-------|:------------------------------------------|
| configuration | Object | an object containing configuration fields |

<div id="incrementalhomerecorder.readhomehomename-observer">

###### incrementalHomeRecorder.readHome(homeName, observer)

</div>

Reads a home with this recorder.

**Kind**: instance method of
[IncrementalHomeRecorder](#IncrementalHomeRecorder)

| Param    | Type   | Description                                                                          |
|:---------|:-------|:-------------------------------------------------------------------------------------|
| homeName | string | the home name on the server or the URL of the home if readHomeURL service is missing |
| observer | Object | callbacks used to follow the reading of the home                                     |

<div id="incrementalhomerecorder.addhomehome">

###### incrementalHomeRecorder.addHome(home)

</div>

Adds a home to be incrementally saved by this recorder.

**Kind**: instance method of
[IncrementalHomeRecorder](#IncrementalHomeRecorder)

| Param | Type |
|:------|:-----|
| home  | Home |

<div id="incrementalhomerecorder.removehomehome">

###### incrementalHomeRecorder.removeHome(home)

</div>

Removes a home previously added with addHome.

**Kind**: instance method of
[IncrementalHomeRecorder](#IncrementalHomeRecorder)

| Param | Type |
|:------|:-----|
| home  | Home |

<div id="incrementalhomerecorder.sendundoableeditshome">

###### incrementalHomeRecorder.sendUndoableEdits(home)

</div>

Sends to the server all the currently waiting edits for the given home.

**Kind**: instance method of
[IncrementalHomeRecorder](#IncrementalHomeRecorder)

| Param | Type |
|:------|:-----|
| home  | Home |

<div id="incrementalhomerecorder.default_tracked_home_properties">

###### IncrementalHomeRecorder.DEFAULT\_TRACKED\_HOME\_PROPERTIES

</div>

The home properties that are tracked by default by incremental
recorders.

**Kind**: static property of
[IncrementalHomeRecorder](#IncrementalHomeRecorder)  

<div id="sweethome3djsapplication">

##### SweetHome3DJSApplication

</div>

**Kind**: global class  
**Author**: Emmanuel Puybaret  
**Author**: Renaud Pawlak  

<div id="new-sweethome3djsapplicationparams">

###### new SweetHome3DJSApplication(\[params\])

</div>

Defines HomeApplication implementation for JavaScript.

| Param      | Type   | Description                                                                                            |
|:-----------|:-------|:-------------------------------------------------------------------------------------------------------|
| \[params\] | Object | the URLs of resources and services required on server (if undefined, will use local files for testing) |

<div id="plancomponent">

##### PlanComponent

</div>

**Kind**: global class  
**Author**: Emmanuel Puybaret  
**Author**: Renaud Pawlak

-   [PlanComponent](#PlanComponent)

    -   [new PlanComponent(containerOrCanvasId, home, preferences,
        \[object3dFactory\], controller)](#new_PlanComponent_new)

    -   *instance*

        -   [.getHTMLElement()](#PlanComponent+getHTMLElement)

        -   [.getPreferredSize()](#PlanComponent+getPreferredSize) ⇒
            java.awt.Dimension

        -   [.getPaintedItems()](#PlanComponent+getPaintedItems) ⇒
            Array.&lt;Object&gt;

        -   [.getItemBounds(g, item)](#PlanComponent+getItemBounds) ⇒
            java.awt.geom.Rectangle2D

        -   [.getTextBounds(text, style, x, y,
            angle)](#PlanComponent+getTextBounds) ⇒ Array

        -   [.getFont(\[defaultFont\],
            \[textStyle\])](#PlanComponent+getFont) ⇒ string

        -   [.getFontMetrics(defaultFont,
            textStyle)](#PlanComponent+getFontMetrics) ⇒ FontMetrics

        -   [.setBackgroundPainted(backgroundPainted)](#PlanComponent+setBackgroundPainted)

        -   [.isBackgroundPainted()](#PlanComponent+isBackgroundPainted)
            ⇒ boolean

        -   [.setSelectedItemsOutlinePainted(selectedItemsOutlinePainted)](#PlanComponent+setSelectedItemsOutlinePainted)

        -   [.isSelectedItemsOutlinePainted()](#PlanComponent+isSelectedItemsOutlinePainted)
            ⇒ boolean

        -   [.repaint()](#PlanComponent+repaint)

        -   [.getGraphics()](#PlanComponent+getGraphics) ⇒ Graphics2D

        -   [.paintComponent(g)](#PlanComponent+paintComponent)

        -   [.getPrintPreferredScale(g,
            pageFormat)](#PlanComponent+getPrintPreferredScale) ⇒ number

        -   [.createTransferData(dataType)](#PlanComponent+createTransferData)
            ⇒ Object

        -   [.getClipboardImage()](#PlanComponent+getClipboardImage) ⇒
            HTMLImageElement

        -   [.isFormatTypeSupported(formatType)](#PlanComponent+isFormatTypeSupported)
            ⇒ boolean

        -   [.exportData(out, formatType,
            settings)](#PlanComponent+exportData)

        -   [.getForegroundColor(mode)](#PlanComponent+getForegroundColor)
            ⇒ string

        -   [.getBackgroundColor(mode)](#PlanComponent+getBackgroundColor)
            ⇒ string

        -   [.paintHomeItems(g, planScale, backgroundColor,
            foregroundColor, paintMode)](#PlanComponent+paintHomeItems)

        -   [.getSelectionColor()](#PlanComponent+getSelectionColor) ⇒
            string

        -   [.getFurnitureOutlineColor()](#PlanComponent+getFurnitureOutlineColor)
            ⇒ string

        -   [.getIndicator(item,
            indicatorType)](#PlanComponent+getIndicator) ⇒ Object

        -   [.isViewableAtSelectedLevel(item)](#PlanComponent+isViewableAtSelectedLevel)
            ⇒ boolean

        -   [.setRectangleFeedback(x0, y0, x1,
            y1)](#PlanComponent+setRectangleFeedback)

        -   [.makeSelectionVisible()](#PlanComponent+makeSelectionVisible)

        -   [.makePointVisible(x, y)](#PlanComponent+makePointVisible)

        -   [.moveView(dx, dy)](#PlanComponent+moveView)

        -   [.getScale()](#PlanComponent+getScale) ⇒ number

        -   [.setScale(scale)](#PlanComponent+setScale)

        -   [.convertXPixelToModel(x)](#PlanComponent+convertXPixelToModel)
            ⇒ number

        -   [.convertYPixelToModel(y)](#PlanComponent+convertYPixelToModel)
            ⇒ number

        -   [.convertXModelToScreen(x)](#PlanComponent+convertXModelToScreen)
            ⇒ number

        -   [.convertYModelToScreen(y)](#PlanComponent+convertYModelToScreen)
            ⇒ number

        -   [.getPixelLength()](#PlanComponent+getPixelLength) ⇒ number

        -   [.setCursor(cursorType)](#PlanComponent+setCursor)

        -   [.setToolTipFeedback(toolTipFeedback)](#PlanComponent+setToolTipFeedback)

        -   [.setToolTipEditedProperties(toolTipEditedProperties,
            toolTipPropertyValues,
            x, y)](#PlanComponent+setToolTipEditedProperties)

        -   [.deleteToolTipFeedback()](#PlanComponent+deleteToolTipFeedback)

        -   [.setResizeIndicatorVisible(resizeIndicatorVisible)](#PlanComponent+setResizeIndicatorVisible)

        -   [.setAlignmentFeedback(alignedObjectClass, alignedObject, x,
            y, showPointFeedback)](#PlanComponent+setAlignmentFeedback)

        -   [.setAngleFeedback(xCenter, yCenter, x1, y1, x2,
            y2)](#PlanComponent+setAngleFeedback)

        -   [.setDraggedItemsFeedback(draggedItems)](#PlanComponent+setDraggedItemsFeedback)

        -   [.setDimensionLinesFeedback(dimensionLines)](#PlanComponent+setDimensionLinesFeedback)

        -   [.deleteFeedback()](#PlanComponent+deleteFeedback)

        -   [.canImportDraggedItems(items,
            x, y)](#PlanComponent+canImportDraggedItems) ⇒ boolean

        -   [.getPieceOfFurnitureSizeInPlan(piece)](#PlanComponent+getPieceOfFurnitureSizeInPlan)
            ⇒ Array

        -   [.isFurnitureSizeInPlanSupported()](#PlanComponent+isFurnitureSizeInPlanSupported)
            ⇒ boolean

        -   [.getHorizontalRuler()](#PlanComponent+getHorizontalRuler) ⇒
            Object

        -   [.getVerticalRuler()](#PlanComponent+getVerticalRuler) ⇒
            Object

    -   *static*

        -   [.IndicatorType](#PlanComponent.IndicatorType)

            -   [new
                PlanComponent.IndicatorType()](#new_PlanComponent.IndicatorType_new)

            -   [.name()](#PlanComponent.IndicatorType+name) ⇒ string

            -   [.toString()](#PlanComponent.IndicatorType+toString) ⇒
                string

        -   [.PaintMode](#PlanComponent.PaintMode)

        -   [.getDefaultSelectionColor(planComponent)](#PlanComponent.getDefaultSelectionColor)
            ⇒ string

<div
id="new-plancomponentcontainerorcanvasid-home-preferences-object3dfactory-controller">

###### new PlanComponent(containerOrCanvasId, home, preferences, \[object3dFactory\], controller)

</div>

Creates a new plan that displays home.

| Param               | Type            | Description                                                                                                                                                                                                                                                  |
|:--------------------|:----------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| containerOrCanvasId | string          | the ID of a HTML DIV or CANVAS                                                                                                                                                                                                                               |
| home                | Home            | the home to display                                                                                                                                                                                                                                          |
| preferences         | UserPreferences | user preferences to retrieve used unit, grid visibility…                                                                                                                                                                                                     |
| \[object3dFactory\] | Object          | a factory able to create 3D objects from home furniture. The \[Selectable, boolean) createObject3D\](Object3DFactory\#createObject3D(Home,) of this factory is expected to return an instance of [Object3DBranch](Object3DBranch) in current implementation. |
| controller          | PlanController  | the optional controller used to manage home items modification                                                                                                                                                                                               |

<div id="plancomponent.gethtmlelement">

###### planComponent.getHTMLElement()

</div>

Returns the HTML element used to view this component at screen.

**Kind**: instance method of [PlanComponent](#PlanComponent)  

<div id="plancomponent.getpreferredsize-java.awt.dimension">

###### planComponent.getPreferredSize() ⇒ java.awt.Dimension

</div>

Returns the preferred size of this component in actual screen pixels
size.

**Kind**: instance method of [PlanComponent](#PlanComponent)  

<div id="plancomponent.getpainteditems-array.object">

###### planComponent.getPaintedItems() ⇒ Array.&lt;Object&gt;

</div>

Returns the collection of walls, furniture, rooms and dimension lines of
the home painted by this component wherever the level they belong to is
selected or not.

**Kind**: instance method of [PlanComponent](#PlanComponent)  

<div id="plancomponent.getitemboundsg-item-java.awt.geom.rectangle2d">

###### planComponent.getItemBounds(g, item) ⇒ java.awt.geom.Rectangle2D

</div>

Returns the bounds of the given item.

**Kind**: instance method of [PlanComponent](#PlanComponent)

| Param | Type       |
|:------|:-----------|
| g     | Graphics2D |
| item  | Object     |

<div id="plancomponent.gettextboundstext-style-x-y-angle-array">

###### planComponent.getTextBounds(text, style, x, y, angle) ⇒ Array

</div>

Returns the coordinates of the bounding rectangle of the text centered
at the point (x,y).

**Kind**: instance method of [PlanComponent](#PlanComponent)

| Param | Type      |
|:------|:----------|
| text  | string    |
| style | TextStyle |
| x     | number    |
| y     | number    |
| angle | number    |

<div id="plancomponent.getfontdefaultfont-textstyle-string">

###### planComponent.getFont(\[defaultFont\], \[textStyle\]) ⇒ string

</div>

Returns the HTML font matching a given text style.

**Kind**: instance method of [PlanComponent](#PlanComponent)

| Param           | Type      |
|:----------------|:----------|
| \[defaultFont\] | string    |
| \[textStyle\]   | TextStyle |

<div id="plancomponent.getfontmetricsdefaultfont-textstyle-fontmetrics">

###### planComponent.getFontMetrics(defaultFont, textStyle) ⇒ FontMetrics

</div>

Returns the font metrics matching a given text style.

**Kind**: instance method of [PlanComponent](#PlanComponent)

| Param       | Type      |
|:------------|:----------|
| defaultFont | string    |
| textStyle   | TextStyle |

<div id="plancomponent.setbackgroundpaintedbackgroundpainted">

###### planComponent.setBackgroundPainted(backgroundPainted)

</div>

Sets whether plan’s background should be painted or not. Background may
include grid and an image.

**Kind**: instance method of [PlanComponent](#PlanComponent)

| Param             | Type    |
|:------------------|:--------|
| backgroundPainted | boolean |

<div id="plancomponent.isbackgroundpainted-boolean">

###### planComponent.isBackgroundPainted() ⇒ boolean

</div>

Returns true if plan’s background should be painted.

**Kind**: instance method of [PlanComponent](#PlanComponent)  

<div
id="plancomponent.setselecteditemsoutlinepaintedselecteditemsoutlinepainted">

###### planComponent.setSelectedItemsOutlinePainted(selectedItemsOutlinePainted)

</div>

Sets whether the outline of home selected items should be painted or
not.

**Kind**: instance method of [PlanComponent](#PlanComponent)

| Param                       | Type    |
|:----------------------------|:--------|
| selectedItemsOutlinePainted | boolean |

<div id="plancomponent.isselecteditemsoutlinepainted-boolean">

###### planComponent.isSelectedItemsOutlinePainted() ⇒ boolean

</div>

Returns true if the outline of home selected items should be painted.

**Kind**: instance method of [PlanComponent](#PlanComponent)  

<div id="plancomponent.repaint">

###### planComponent.repaint()

</div>

Repaints this component elements when possible.

**Kind**: instance method of [PlanComponent](#PlanComponent)  

<div id="plancomponent.getgraphics-graphics2d">

###### planComponent.getGraphics() ⇒ Graphics2D

</div>

Returns a Graphics2D object.

**Kind**: instance method of [PlanComponent](#PlanComponent)  

<div id="plancomponent.paintcomponentg">

###### planComponent.paintComponent(g)

</div>

Paints this component.

**Kind**: instance method of [PlanComponent](#PlanComponent)

| Param | Type       |
|:------|:-----------|
| g     | Graphics2D |

<div id="plancomponent.getprintpreferredscaleg-pageformat-number">

###### planComponent.getPrintPreferredScale(g, pageFormat) ⇒ number

</div>

Returns the print preferred scale of the plan drawn in this component to
make it fill pageFormat imageable size.

**Kind**: instance method of [PlanComponent](#PlanComponent)

| Param      | Type                      |
|:-----------|:--------------------------|
| g          | Graphics2D                |
| pageFormat | java.awt.print.PageFormat |

<div id="plancomponent.createtransferdatadatatype-object">

###### planComponent.createTransferData(dataType) ⇒ Object

</div>

Returns an image of selected items in plan for transfer purpose.

**Kind**: instance method of [PlanComponent](#PlanComponent)

| Param    | Type                      |
|:---------|:--------------------------|
| dataType | TransferableView.DataType |

<div id="plancomponent.getclipboardimage-htmlimageelement">

###### planComponent.getClipboardImage() ⇒ HTMLImageElement

</div>

Returns an image of the selected items displayed by this component
(camera excepted) with no outline at scale 1/1 (1 pixel = 1cm).

**Kind**: instance method of [PlanComponent](#PlanComponent)  

<div id="plancomponent.isformattypesupportedformattype-boolean">

###### planComponent.isFormatTypeSupported(formatType) ⇒ boolean

</div>

Returns true if the given format is SVG.

**Kind**: instance method of [PlanComponent](#PlanComponent)

| Param      | Type                      |
|:-----------|:--------------------------|
| formatType | ExportableView.FormatType |

<div id="plancomponent.exportdataout-formattype-settings">

###### planComponent.exportData(out, formatType, settings)

</div>

Writes this plan in the given output stream at SVG (Scalable Vector
Graphics) format if this is the requested format.

**Kind**: instance method of [PlanComponent](#PlanComponent)

| Param      | Type                      |
|:-----------|:--------------------------|
| out        | java.io.OutputStream      |
| formatType | ExportableView.FormatType |
| settings   | Object                    |

<div id="plancomponent.getforegroundcolormode-string">

###### planComponent.getForegroundColor(mode) ⇒ string

</div>

Returns the foreground color used to draw content.

**Kind**: instance method of [PlanComponent](#PlanComponent)

| Param | Type                                  |
|:------|:--------------------------------------|
| mode  | [PaintMode](#PlanComponent.PaintMode) |

<div id="plancomponent.getbackgroundcolormode-string">

###### planComponent.getBackgroundColor(mode) ⇒ string

</div>

Returns the background color used to draw content.

**Kind**: instance method of [PlanComponent](#PlanComponent)

| Param | Type                                  |
|:------|:--------------------------------------|
| mode  | [PaintMode](#PlanComponent.PaintMode) |

<div
id="plancomponent.painthomeitemsg-planscale-backgroundcolor-foregroundcolor-paintmode">

###### planComponent.paintHomeItems(g, planScale, backgroundColor, foregroundColor, paintMode)

</div>

Paints home items at the given scale, and with background and foreground
colors. Outline around selected items will be painted only under PAINT
mode.

**Kind**: instance method of [PlanComponent](#PlanComponent)

| Param           | Type                                  |
|:----------------|:--------------------------------------|
| g               | Graphics2D                            |
| planScale       | number                                |
| backgroundColor | string                                |
| foregroundColor | string                                |
| paintMode       | [PaintMode](#PlanComponent.PaintMode) |

<div id="plancomponent.getselectioncolor-string">

###### planComponent.getSelectionColor() ⇒ string

</div>

Returns the color used to draw selection outlines.

**Kind**: instance method of [PlanComponent](#PlanComponent)  

<div id="plancomponent.getfurnitureoutlinecolor-string">

###### planComponent.getFurnitureOutlineColor() ⇒ string

</div>

Returns the color used to draw furniture outline of the shape where a
user can click to select a piece of furniture.

**Kind**: instance method of [PlanComponent](#PlanComponent)  

<div id="plancomponent.getindicatoritem-indicatortype-object">

###### planComponent.getIndicator(item, indicatorType) ⇒ Object

</div>

Returns the shape of the given indicator type.

**Kind**: instance method of [PlanComponent](#PlanComponent)

| Param         | Type                                          |
|:--------------|:----------------------------------------------|
| item          | Object                                        |
| indicatorType | [IndicatorType](#PlanComponent.IndicatorType) |

<div id="plancomponent.isviewableatselectedlevelitem-boolean">

###### planComponent.isViewableAtSelectedLevel(item) ⇒ boolean

</div>

Returns true if the given item can be viewed in the plan at the selected
level.

**Kind**: instance method of [PlanComponent](#PlanComponent)

| Param | Type   |
|:------|:-------|
| item  | Object |

<div id="plancomponent.setrectanglefeedbackx0-y0-x1-y1">

###### planComponent.setRectangleFeedback(x0, y0, x1, y1)

</div>

Sets rectangle selection feedback coordinates.

**Kind**: instance method of [PlanComponent](#PlanComponent)

| Param | Type   |
|:------|:-------|
| x0    | number |
| y0    | number |
| x1    | number |
| y1    | number |

<div id="plancomponent.makeselectionvisible">

###### planComponent.makeSelectionVisible()

</div>

Ensures selected items are visible at screen and moves scroll bars if
needed.

**Kind**: instance method of [PlanComponent](#PlanComponent)  

<div id="plancomponent.makepointvisiblex-y">

###### planComponent.makePointVisible(x, y)

</div>

Ensures the point at (x, y) is visible, moving scroll bars if needed.

**Kind**: instance method of [PlanComponent](#PlanComponent)

| Param | Type   |
|:------|:-------|
| x     | number |
| y     | number |

<div id="plancomponent.moveviewdx-dy">

###### planComponent.moveView(dx, dy)

</div>

Moves the view from (dx, dy) unit in the scrolling zone it belongs to.

**Kind**: instance method of [PlanComponent](#PlanComponent)

| Param | Type   |
|:------|:-------|
| dx    | number |
| dy    | number |

<div id="plancomponent.getscale-number">

###### planComponent.getScale() ⇒ number

</div>

Returns the scale used to display the plan.

**Kind**: instance method of [PlanComponent](#PlanComponent)  

<div id="plancomponent.setscalescale">

###### planComponent.setScale(scale)

</div>

Sets the scale used to display the plan. If this component is displayed
in a scrolled panel the view position is updated to ensure the center’s
view will remain the same after the scale change.

**Kind**: instance method of [PlanComponent](#PlanComponent)

| Param | Type   |
|:------|:-------|
| scale | number |

<div id="plancomponent.convertxpixeltomodelx-number">

###### planComponent.convertXPixelToModel(x) ⇒ number

</div>

Returns x converted in model coordinates space.

**Kind**: instance method of [PlanComponent](#PlanComponent)

| Param | Type   |
|:------|:-------|
| x     | number |

<div id="plancomponent.convertypixeltomodely-number">

###### planComponent.convertYPixelToModel(y) ⇒ number

</div>

Returns y converted in model coordinates space.

**Kind**: instance method of [PlanComponent](#PlanComponent)

| Param | Type   |
|:------|:-------|
| y     | number |

<div id="plancomponent.convertxmodeltoscreenx-number">

###### planComponent.convertXModelToScreen(x) ⇒ number

</div>

Returns x converted in screen coordinates space.

**Kind**: instance method of [PlanComponent](#PlanComponent)

| Param | Type   |
|:------|:-------|
| x     | number |

<div id="plancomponent.convertymodeltoscreeny-number">

###### planComponent.convertYModelToScreen(y) ⇒ number

</div>

Returns y converted in screen coordinates space.

**Kind**: instance method of [PlanComponent](#PlanComponent)

| Param | Type   |
|:------|:-------|
| y     | number |

<div id="plancomponent.getpixellength-number">

###### planComponent.getPixelLength() ⇒ number

</div>

Returns the length in centimeters of a pixel with the current scale.

**Kind**: instance method of [PlanComponent](#PlanComponent)  

<div id="plancomponent.setcursorcursortype">

###### planComponent.setCursor(cursorType)

</div>

Sets the cursor of this component.

**Kind**: instance method of [PlanComponent](#PlanComponent)

| Param      | Type                       |
|:-----------|:---------------------------|
| cursorType | PlanView.CursorType string |

<div id="plancomponent.settooltipfeedbacktooltipfeedback">

###### planComponent.setToolTipFeedback(toolTipFeedback)

</div>

Sets tool tip text displayed as feedback.

**Kind**: instance method of [PlanComponent](#PlanComponent)

| Param           | Type   | Description                                                            |
|:----------------|:-------|:-----------------------------------------------------------------------|
| toolTipFeedback | string | the text displayed in the tool tip or null to make tool tip disappear. |

<div
id="plancomponent.settooltipeditedpropertiestooltipeditedproperties-tooltippropertyvalues-x-y">

###### planComponent.setToolTipEditedProperties(toolTipEditedProperties, toolTipPropertyValues, x, y)

</div>

Set tool tip edition.

**Kind**: instance method of [PlanComponent](#PlanComponent)

| Param                   | Type   |
|:------------------------|:-------|
| toolTipEditedProperties | Array  |
| toolTipPropertyValues   | Array  |
| x                       | number |
| y                       | number |

<div id="plancomponent.deletetooltipfeedback">

###### planComponent.deleteToolTipFeedback()

</div>

Deletes tool tip text from screen.

**Kind**: instance method of [PlanComponent](#PlanComponent)  

<div id="plancomponent.setresizeindicatorvisibleresizeindicatorvisible">

###### planComponent.setResizeIndicatorVisible(resizeIndicatorVisible)

</div>

Sets whether the resize indicator of selected wall or piece of furniture
should be visible or not.

**Kind**: instance method of [PlanComponent](#PlanComponent)

| Param                  | Type    |
|:-----------------------|:--------|
| resizeIndicatorVisible | boolean |

<div
id="plancomponent.setalignmentfeedbackalignedobjectclass-alignedobject-x-y-showpointfeedback">

###### planComponent.setAlignmentFeedback(alignedObjectClass, alignedObject, x, y, showPointFeedback)

</div>

Sets the location point for alignment feedback.

**Kind**: instance method of [PlanComponent](#PlanComponent)

| Param              | Type    |
|:-------------------|:--------|
| alignedObjectClass | Object  |
| alignedObject      | Object  |
| x                  | number  |
| y                  | number  |
| showPointFeedback  | boolean |

<div id="plancomponent.setanglefeedbackxcenter-ycenter-x1-y1-x2-y2">

###### planComponent.setAngleFeedback(xCenter, yCenter, x1, y1, x2, y2)

</div>

Sets the points used to draw an angle in plan view.

**Kind**: instance method of [PlanComponent](#PlanComponent)

| Param   | Type   |
|:--------|:-------|
| xCenter | number |
| yCenter | number |
| x1      | number |
| y1      | number |
| x2      | number |
| y2      | number |

<div id="plancomponent.setdraggeditemsfeedbackdraggeditems">

###### planComponent.setDraggedItemsFeedback(draggedItems)

</div>

Sets the feedback of dragged items drawn during a drag and drop
operation, initiated from outside of plan view.

**Kind**: instance method of [PlanComponent](#PlanComponent)

| Param        | Type                 |
|:-------------|:---------------------|
| draggedItems | Array.&lt;Object&gt; |

<div id="plancomponent.setdimensionlinesfeedbackdimensionlines">

###### planComponent.setDimensionLinesFeedback(dimensionLines)

</div>

Sets the given dimension lines to be drawn as feedback.

**Kind**: instance method of [PlanComponent](#PlanComponent)

| Param          | Type                        |
|:---------------|:----------------------------|
| dimensionLines | Array.&lt;DimensionLine&gt; |

<div id="plancomponent.deletefeedback">

###### planComponent.deleteFeedback()

</div>

Deletes all elements shown as feedback.

**Kind**: instance method of [PlanComponent](#PlanComponent)  

<div id="plancomponent.canimportdraggeditemsitems-x-y-boolean">

###### planComponent.canImportDraggedItems(items, x, y) ⇒ boolean

</div>

Returns true.

**Kind**: instance method of [PlanComponent](#PlanComponent)

| Param | Type                 |
|:------|:---------------------|
| items | Array.&lt;Object&gt; |
| x     | number               |
| y     | number               |

<div id="plancomponent.getpieceoffurnituresizeinplanpiece-array">

###### planComponent.getPieceOfFurnitureSizeInPlan(piece) ⇒ Array

</div>

Returns the size of the given piece of furniture in the horizontal plan,
or null if the view isn’t able to compute such a value.

**Kind**: instance method of [PlanComponent](#PlanComponent)

| Param | Type                 |
|:------|:---------------------|
| piece | HomePieceOfFurniture |

<div id="plancomponent.isfurnituresizeinplansupported-boolean">

###### planComponent.isFurnitureSizeInPlanSupported() ⇒ boolean

</div>

Returns true if this component is able to compute the size of
horizontally rotated furniture.

**Kind**: instance method of [PlanComponent](#PlanComponent)  

<div id="plancomponent.gethorizontalruler-object">

###### planComponent.getHorizontalRuler() ⇒ Object

</div>

Returns the component used as an horizontal ruler for this plan.

**Kind**: instance method of [PlanComponent](#PlanComponent)  

<div id="plancomponent.getverticalruler-object">

###### planComponent.getVerticalRuler() ⇒ Object

</div>

Returns the component used as a vertical ruler for this plan.

**Kind**: instance method of [PlanComponent](#PlanComponent)  

<div id="plancomponent.indicatortype">

###### PlanComponent.IndicatorType

</div>

**Kind**: static class of [PlanComponent](#PlanComponent)

-   [.IndicatorType](#PlanComponent.IndicatorType)

    -   [new
        PlanComponent.IndicatorType()](#new_PlanComponent.IndicatorType_new)

    -   [.name()](#PlanComponent.IndicatorType+name) ⇒ string

    -   [.toString()](#PlanComponent.IndicatorType+toString) string

<div id="new-plancomponent.indicatortype">

####### new PlanComponent.IndicatorType()

</div>

Indicator types that may be displayed on selected items.

<div id="indicatortype.name-string">

####### indicatorType.name() ⇒ string

</div>

**Kind**: instance method of
[IndicatorType](#PlanComponent.IndicatorType)  

<div id="indicatortype.tostring-string">

####### indicatorType.toString() ⇒ string

</div>

**Kind**: instance method of
[IndicatorType](#PlanComponent.IndicatorType)  

<div id="plancomponent.paintmode">

###### PlanComponent.PaintMode

</div>

The circumstances under which the home items displayed by this component
will be painted.

**Kind**: static enum of [PlanComponent](#PlanComponent)  
**Properties**

| Name      | Type                                  |
|:----------|:--------------------------------------|
| PAINT     | [PaintMode](#PlanComponent.PaintMode) |
| PRINT     | [PaintMode](#PlanComponent.PaintMode) |
| CLIPBOARD | [PaintMode](#PlanComponent.PaintMode) |
| EXPORT    | [PaintMode](#PlanComponent.PaintMode) |

<div id="plancomponent.getdefaultselectioncolorplancomponent-string">

###### PlanComponent.getDefaultSelectionColor(planComponent) string

</div>

Returns the default color used to draw selection outlines. Note that the
default selection color may be forced using CSS by setting
backgroundColor with the ::selection selector. In case the browser does
not support the ::selection selector, one can force the selectio color
in JavaScript by setting PlanComponent.DEFAULT\_SELECTION\_COLOR.

**Kind**: static method of [PlanComponent](#PlanComponent)

| Param         | Type                            |
|:--------------|:--------------------------------|
| planComponent | [PlanComponent](#PlanComponent) |
