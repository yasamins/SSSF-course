# WebRTC
  * RTC = Real Time Communications
  * [Getting started](https://www.html5rocks.com/en/tutorials/webrtc/basics/)
  * [Some server related stuff](https://www.html5rocks.com/en/tutorials/webrtc/basics/)
  * [WebRTC tutorial](https://codelabs.developers.google.com/codelabs/webrtc-web/#0)
  * [Browser support](http://iswebrtcreadyyet.com/)
  
## SimpleWebRTC
  * [Home page](https://simplewebrtc.com/)
  * [On GitHub](https://github.com/andyet/SimpleWebRTC)
  
## Signalmaster
  * [On GitHub](https://github.com/andyet/signalmaster)
  
#### Excercise: Create a simple videochat on localhost
##### TODO  
  1. Write a simple express.js server to serve 'public' folder over [https](https://github.com/ilkkamtk/SSSF-course/blob/master/Slides/Week3/W3-4-https-passport.md)
     * create following folder structure
     ```
     -server.js
     public
       -videochat.html
       js
         -rtc.js
     ```
      
      videochat.html:
      ```html
        <!DOCTYPE html>
        <html>
        <head>
            <script src="https://simplewebrtc.com/latest-v2.js"></script>
            <style>
                #remoteVideos video {
                    height: 150px;
                }
                #localVideo {
                    height: 150px;
                }
            </style>
        </head>
        <body>
        <video id="localVideo"></video>
        <div id="remoteVideos"></div>
        <script src="js/rtc.js"></script>
        </body>
        </html>
      ```
      rtc.js:
      ```javascript
        const webrtc = new SimpleWebRTC({
            // the id/element dom element that will hold "our" video
            localVideoEl: 'localVideo',
            // the id/element dom element that will hold remote videos
            remoteVideosEl: 'remoteVideos',
            // immediately ask for camera access
            autoRequestMedia: true,
        });
        
        // we have to wait until it's ready
        webrtc.on('readyToCall', function () {
            // you can name it anything
            webrtc.joinRoom('your room name');
        });
      ```
  2. Start server, open `https://localhost:3000/videochat.html`
     * If you open the same address in another browser window you should see two videos
  3. To chat with your classmates, you need to move 'public' folder to users.metropolia.fi (use https) or deploy project to Jelastic
  4. Previous example uses simpleWebRTC:s server to handle the signaling. Create your own signalling server:
     * Start another project and write a simple express.js [https](https://github.com/ilkkamtk/SSSF-course/blob/master/Slides/Week3/W3-4-https-passport.md) server **_without_** force redirection
        * use port 8888 instead of 3000 because you will have two server instances running on localhost
     * Add [Signalmaster](https://github.com/andyet/signalmaster) and [getconfig](https://github.com/HenrikJoreteg/getconfig) to the project:
      
       `npm install signal-master getconfig --save`
       
     * add/modify followind to server.js
     ```javascript
        // add this with other requires:
        const config = require('getconfig');
        const sockets = require('signal-master/sockets');
        // modify this line:
        const server = https.createServer(options, app).listen(8888);
        // add this to the end of file
        sockets(server, config);   
       
     ```
     * create folder 'config' and add development.json
     development.json:
     ```json
        {
          "isDev": true,
          "rooms": {
            "/* maxClients */": "/* maximum number of clients per room. 0 = no limit */",
            "maxClients": 0
          },
          "stunservers": [
            {
              "url": "stun:stun.l.google.com:19302"
            }
          ],
          "turnservers": [
            {
              "urls": ["turn:your.turn.servers.here"],
              "secret": "turnserversharedsecret",
              "expiry": 86400
            }
          ]
        }
     ```
   5. Start your server, open https://localhost:8888 and accept secirity execption
   6. Modify rtc.js:
      ```javascript
      const webrtc = new SimpleWebRTC({
          ...
          url: 'https://localhost:8888',
      });
      ```
   7. Open `https://localhost:3000/videochat.html` in two broser windows (or on Chrome and Firefox) to test the application. Use developer tools->network tab to troubleshoot.
   8. If you would deploy signalling server to Jelastic, change the port number to 3000
   9. _FYI: Public signalling server is at https://signaler.jelastic.metropolia.fi (edit url in rtc.js if you want to test this)_
    
#### Excercise 2: Stylise video chat
  * [Instructions](https://simplewebrtc.com/notsosimple.html)
  * You can follow the instructions but try to do your own version. Like picture in picture, add nicknames etc.
  
### Extra links
  * [TURN server installation](https://github.com/alongubkin/phonertc/wiki/Installation)
