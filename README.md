# Klickr.io

## Contents  
[What is Klickr.io?](#about)  
[Is Klickr.io live?](#live)  
[Team](#team)  
[Screenshots](#screenshots)  
[Tech Stack](#techstack)  
[Technical Challenges](#challenges)  
[Roadmap for Codebase](#roadmap)  

## <a name="about"/> What is Klickr.io

Following the age-old mantra of "a picture speaks a thousand words," Klickr.io makes it super easy to create shareable tutorial-style snippets of web content and share them with others. We address the problem of how difficult it to put together instructions to teach someone how to do even the simplest of tasks.

Using our Chrome extension, you can record and annotate a web snippet, or Klick, on your browser which you can share with others. These snippets are not videos - they play back natively on the Chrome browser.

## <a name="live"/> Is Klickr.io live?

In short, yes. Visit us at [http://klickr.io](http://klickr.io).

Note that Klickr is in Beta. It has bugs and lacks important features. That said, even with its current flaws, we believe it brings enough value to make it useful, fun and share-worthy. We also want you to try it and let us know what you think.

## <a name="team"/> Team

* [Willson Mock](https://medium.com/@fay_jai): Project manager, chrome extension annotating capability and web architecture
* [Justin Yek](http://www.penguinhustle.com/blog): Overall chrome extension and web architecture, code integration, site design, deployment, chrome extension recording capability and video editing
* [Luke Ramsey](https://github.com/lramsey): Chrome extension playback capability and web architecture
* [Stephan Dacosta](https://github.com/stephandacosta): Chrome extension user interface and code refactoring

## <a name="screenshots"/> Screenshots

![Klickr.io Home Page](https://raw.github.com/klickr/klickr/master/app/images/klickrio-home-page.png)

## Gallery page
- gallery page

## Download page
- download page
(Wait till after refactor)

## <a name="techstack"/> Tech Stack

Klickr is comprised of a Chrome extension and a web application. The tech stack we used is as follows:

**Chrome extension:** Manifest 2, Angular

**Web application:** Backend: Node, Express, MongoDB and Mongoose; Frontend: Angular.js, CSS3

## <a name="challenges"/> Technical Challenges

* **Architecture:** Engineering Klickr required access beyond the browser DOM. We considered desktop apps, browser plugins, bookmarklets and node apps before choosing a Chrome extension.

* **Chrome Extension:** We had to work extensively in a new technology, the Chrome extension framework, which involved asyncronous messaging between webpage and application level code. We applied our knowledge from other MVC frameworks and organized our chrome extension in a component-based, Backbone inspired manner. 

* **Recording and Playback Architecture:** As there were no existing solutions we could work on, we designed and implemented a proprietary recording, annotation and playback framework without any dependencies to suit our needs of recording and playing back natively on the browser.

* **Playback:** Webpages are not designed for native playbacks. We implemented several workarounds to enable compatability with different screen sizes, annotations, trigger clicks, keypresses, scrolling and multipage playback in the browser by injecting Javascript into the page DOM.

## <a name="roadmap"/> Codebase Roadmap
Our codebase has two separate components: one representing the front- and back-end interface
for running our web application and another for the chrome browser plugin.

### Web Application
Key files for backend:

  1. server.js and server-config.js
  2. server/models/klicks.js

We used Yeoman to scaffold our front- and back-end web application so it should follow a
familiar layout. Starting with the backend experience, the server.js and server-config.js
are standard setups for Express servers. We set up our Mongo connection in server/config.js
and maintain one schema for collecting all our Klickr data in server/models/klicks.js. 
The CRUD aspect of our application logic can be found in the route-handlers within
lib/request-handler.js. 

Key files for frontend:
  
  1. landing.html and gallery.html
  2. scripts/

The frontend component consists of Angular. Our landing view consists of some added 
features where we add custom directives for handling how certain elements scroll into 
place. Our gallery view is currently a listing of all saved Klickr's from all users. 
We added functionality to filter by Klickr description and included the initial concept
of building "hype" around each Klickr, in addition to being able to  see basic statistics 
such as the duration and total number of views for each Klickr.

### Chrome Extension
Key files:

  1. manifest.json
  2. popups/popup.html and popups/app.js
  3. bg/
    * background.js
    * bg-editor.js
    * bg-player.js
    * bg-recorder.js
  4. content-scripts/
    * message.js
    * player.js
    * recorder.js

The manifest.json file is the configuration file for our chrome browser plugin. There are two main sections within the manifest.json that have significant ramifications for how we developed our application: 1) background, and 2) content_scripts. The scripts within the background section describe the JavaScript files that consist of the background process that stays persistent while the chrome browser is open. Therefore, these files are particular useful for holding data that needs to be kept around across multiple DOM sessions (such as multi-page recordings). The content script section describes the JavaScript files that will be injected into each DOM page whenever a user loads a new one. Of particular note is that the files within these two sections do not have direct communication with each other because they exist in different JavaScript execution contexts. This is where the PubSub paradigm is prevalent and message passing occurs between the two sets of files.

The popup.html file represents the primary front end interface for our end users. This is where all our buttons exist - play, stop, replay, pause, annotate, delete, and save. This file combined with the app.js file represent the entire Angular frontend. The functions defined within the PopupCtrl control when the buttons should appear for users 
based on the status of the different parts of our application.

[INSERT INFO FOR RECORDER.JS]

[INSERT INFO FOR PLAYER.JS]

The message.js file represents our Message class.  You can think of a message instance as 
any of annotations that appear on screen. Each message object has the ability to fade out
from the screen when appropriate.

[INSERT INFO FOR BACKGROUND.JS]

[INSERT INFO FOR BG-RECORDER.JS]

[INSERT INFO FOR BG-PLAYER.JS]

The bg-editor.js file represents our Editor class. You can think of an editor instance 
as the controller between a particular player and recorder instance. It dictates when
a player instance can pause and resume playback, and when a user can add annotations
to a Klickr recording.
