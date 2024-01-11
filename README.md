
Project Documentation
Overview
This documentation provides an overview of the project, outlining the libraries used and the core functionalities implemented. The project is a web-based phone application built on SIP.js, incorporating various features such as audio and video calling, hold, mute, transfer, DTMF sending, and conference calling.

Libraries Used
The project utilizes the following libraries, each serving a specific purpose:

jQuery (v3.6.1):

Description: A fast, small, and feature-rich JavaScript library.
Source: jQuery
CDN: https://dtd6jl0d42sve.cloudfront.net/lib/jquery/jquery-3.6.1.min.js
jQuery UI (v1.13.2):

Description: A curated set of user interface interactions, effects, widgets, and themes.
Source: jQuery UI
CDN: https://dtd6jl0d42sve.cloudfront.net/lib/jquery/jquery-ui-1.13.2.min.js
jQuery MD5 (v1.0.0):

Description: A lightweight MD5 hash function for jQuery.
CDN: https://dtd6jl0d42sve.cloudfront.net/lib/jquery/jquery.md5-min.js (deferred loading)
Chart.js (v2.7.2):

Description: Simple yet flexible JavaScript charting library for designers and developers.
Source: Chart.js
CDN: https://dtd6jl0d42sve.cloudfront.net/lib/Chart/Chart.bundle-2.7.2.min.js (deferred loading)
SIP.js (v0.20.0):

Description: A simple, easy-to-use, and powerful JavaScript library for handling SIP signaling.
Source: SIP.js
CDN: https://dtd6jl0d42sve.cloudfront.net/lib/SipJS/sip-0.20.0.min.js (deferred loading)
Fabric.js (v2.4.6):

Description: A powerful and simple HTML5 canvas library.
Source: Fabric.js
CDN: https://dtd6jl0d42sve.cloudfront.net/lib/FabricJS/fabric-2.4.6.min.js (deferred loading)
Moment.js (v2.24.0):

Description: Parse, validate, manipulate, and display dates and times in JavaScript.
Source: Moment.js
CDN: https://dtd6jl0d42sve.cloudfront.net/lib/Moment/moment-with-locales-2.24.0.min.js (deferred loading)
Croppie (v2.6.4):

Description: A fast and easy-to-use image cropping plugin.
Source: Croppie
CDN: https://dtd6jl0d42sve.cloudfront.net/lib/Croppie/Croppie-2.6.4/croppie.min.js (deferred loading)
Strophe.js (v1.4.1):

Description: A JavaScript library for XMPP (Extensible Messaging and Presence Protocol) protocol.
Source: Strophe.js
CDN: https://dtd6jl0d42sve.cloudfront.net/lib/XMPP/strophe-1.4.1.umd.min.js (deferred loading)
Project Structure
The core functionality of the web phone is implemented in the phone.js file. This file contains functions responsible for various features, including audio and video calling, hold, mute, transfer, DTMF sending, and conference calling.
Code contains html components designed for a different ui but can be replaced by your own ui.In this case, all you need is to 
add your custom js code in the pre-defined functions in phone.js Required libraries are being used via cdn inindex.html for testing purposes.
However, lib directory contains all the included packages files.
Refer to the comments and documentation within the phone.js file for details on each implemented functionality. Customize and extend the functions as needed for your specific use case.

Feel free to reach out to the development team for any further assistance or inquiries.
