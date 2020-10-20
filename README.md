# Sweet Home 3D Online (SH3DO) - Getting Started

This git repository gathers the documentation and resources to get started with embeding [SH3DO](https://www.sweethome3d.online) into your own Web Site.

## Purpose

The purpose of SH3DO is to provide an easy-to-install and easy-to-use SaaS for 3D-powered plan edition, floor planning, and so on. SH3DO provides a REST API, so that it is easy to use it independently from the programming language.

SH3DO comes with a free 'starter' mode to enable plan editing and 3D visualization in your web site. Please visit https://www.sweethome3d.online. As explained below, you can also try out embeding locally for development purpose.

## Getting Started

You can try out the demo service, just by using the given demos provided in the demos directory. 

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

## Documentation

Online documentation is available [here](docs/sh3d.online.md).

Browse the [docs](docs) directory for more details.

## History and Licensing Note

SH3DO is built on the top of SweetHome3D and SweetHome3DJS Open Source projects, which are available on SourceForge. However, SH3DO is *not* Open Source. 

SH3DO is born from a collaboration between Renaud Pawlak (CINCHEO) and Emmanuel Puybaret (eTeks). Renaud Pawlak, creator and owner of [JSweet.org](http://wwww.jsweet.org), helped Emmanuel Puybaret to translate the Java version of Sweet Home 3D to JavaScript (partly thanks to the JSweet tranpiler). The result of this work is fully Open Source and is available in the SweetHome3DJS project (see www.sweethome3d.com for more details and access the source code on sourceforge.net).

Thanks to an excluse partnership between CINCHEO and eTeks, SH3DO leverages SweetHome3D and SweetHome3DJS to make it SaaS and make it available to anyone with minimal development efforts.
