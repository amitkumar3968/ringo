`objective-ddp`
===============

In this branch represents ongoing work to replace Ringo's cumbersome socket.io + http screenshot posting mechanisms with a better alternative using meteor.js (http://meteor.com) and DDP. It's not currently in a working state, this is a work in progress.

### Todo:

- [x] Sample app is now using [ObjectiveDDP](https://github.com/boundsj/ObjectiveDDP). It can connect to the meteor backend but not much else
- [x] Simple browser-to-browser OpenTok sessions
- [x] Implement browser-to-ios OpenTok sessions
- [x] Remote agent drawing/signalling on iOS screen
- [ ] Upload device screenshots using something like [CollectionFS](https://github.com/CollectionFS/Meteor-CollectionFS)
- [ ] Implement a simple agent/endUser identification

### Meteor instructions:

You must run and deploy the meteor application from the meteor subdirectory:

```
$ cd meteor
```

Your OpenTok account credentials must be supplied via a ```settings.json``` file, for example:

```
{
    "env": {
        "OPENTOK_KEY": 1234567890,
        "OPENTOK_SECRET": "abcdefg1234567890"
    }
}
```

When you run the application, specify these settings on the command line:

```
$ meteor --settings settings.json
```

You must also specify these settings when you deploy:

```
$ meteor deploy --settings settings.json yoursite.meteor.com
```

Ringo
============

Add one-way live video, two-way audio chat, screen sharing and annotations to your iOS app in minutes.
Use this to create an experience similar to Amazon Mayday within your app.

This project was built as part of a Hackathon at Radialpoint (http://radialpoint.com) in Montreal. We thought
that the experience created by Mayday was groovy and wanted to see if we could build an SDK that encourages
more apps to adopt its experience.

See our demo video and learn more at http://radialpoint.github.io/ringo

Currently the code is highly experimental but shouldn't be too hard for you to clean up
and add into your app if you love the great end-user experience it can provide.

Requirements
============

iOS SDK:
 * AFNetworking
 * SocketIO
 * SocketRocket
 * OpenTok - You *must* sign up for an API key (a free trial is available) at http://opentok.com

Agent Backend:
 * nodejs
 * mongodb (Mongo HQ works just fine)
 * SocketIO

Adding Ringo to your iOS App
===========

### Project configuration

1. Copy the "Ringo" folder from the distribution above into your XCode project.
2. Add AFNetworking, SocketIO and SocketRocket to your project if you haven't already.
3. Download and configure the OpenTok iOS SDK from http://tokbox.com/opentok/libraries/client/ios/ - *Carefully* follow the instructions on that page to set up OpenTok, it's a little tricky
4. In your project's build settings, set the `Architectures` setting to `$(ARCHS_STANDARD_32_BIT)`

### Code Changes

1. In the file `RGOVideoViewController.m` replace `kApiKey` and `kApiEndpoint` with your OpenTok API key and the URL where intend to host your Ringo backend, which we'll configure in the next section.

2. In your app, `#import "Ringo.h"` and simply call `[Ringo showTime]` in your event handler should kick off the video session with a support agent.

Take a quick look at the AwesomeApp sample in this source tree if you have any questions.

Deploying the Agent Web App & Backend
===========

The web app and backend exist as a node.js + express + MongoDB application located in the `server` directory.

To run it locally you must first have node.js (http://nodejs.org/) installed. You must also have a  running MongoDB instance handy. You can install MongoDB and run it locally (http://docs.mongodb.org/manual/installation/) or you can use a service such as MongoHQ (mongohq.com) if you prefer. You can specify your mongo URL in `server/config/secrets.js`, which points to localhost by default.

Once you have your prerequisites set up,

1. Install the npm pacakge dependencies:

    ```
    cd server
    npm install
    ```
2. Configure your OpenTok key and secret in `server/config/secrets.js`
3. Start the application like so:
    ```
    node app.js
    ```

You should now see the web app running at http://localhost:3000

Once you have all of that working, you should be ready to deploy the application to your favorite node could platform such as Heroku.


Limitations
===========

* The backend currently supports only one connection at a time. We're toying with multiple session support and queueing but would love some help!
* Apps must be compiled for 32 bit ARM (can't be built specifically for iPhone 5S) - this is purely an OpenTok limitation.
* It's buggy, but fun. Enjoy!


Stay groovy, brothers & sisters!
