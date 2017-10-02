This project was bootstrapped with [Create React Native App](https://github.com/react-community/create-react-native-app).

App.js is the only file changed from the default project.
Notes:
Send mail implementation uses a 3rd party SMTP relay server.  The auth worked but email send has internal server error = 500.  Support ticket opened.  Spent several hours on SendPulse's API because it looked interesting and provided the best user experience.  Not sure when support ticket will resolve but basic idea is code complete.  Refactoring/code cleanup would be another time allotment.  Would more than likely end up ejecting the "create-react-native-app" and expanding on the attachment selection to future proof functionality.    


Design Decisions:

ReactNative Predefined app VS. ejected app.
-------------------------------------------

  Ejected app ultimately wins for customization and debugging.  
  Predefined wins for quick and dirty demo.

Send Mail
---------

  Used the free tier of SendPulse.com's SMTP relay server api.  
  The authentication worked but there were problems with the email api.  
  After working with support for an hour, they decided to debug api with my client code (still waiting on response).
  If for some reason SendPulse API fails, a similar in house solution with simple NODE.JS server acting as relay could replace.

  Many ways to do this. But two basic camps when considering UX.
  1. Popup users email client with Predefined email.  User must SEND it.
  2. POST request to an SMTP relay server.  No additional popup.

  "Popup users email client" method
  PROS:
  1. Doesn't require relay service.
  2. User can modify message before sending.
  3. User can track message in their sentMail folder.
  4. Secondary confirmation before a manual email send.
  CONS:
  1. Requires native call to trigger email client
     I. Use a 3rd party plugin that may or may not work properly (lot of garbage to weed through).
    II. Simple to do natively but requires native work on all platforms including web.
  2. UX has redundant confirmation and swap to users email client.
  3. Broken if user does not have an email client setup.

  "POST to SMTP relay server"
  PROS:
  1. Crossplatform: Mobile & web
  2. No need for client to have email client setup.
  3. Streamlined UX, no email client popup.
  4. Faster, quick POST call, even with an AUTH round trip its faster than engaging email client.
  CONS:
  1. Requires special SMTP relay server.
  2. Requires base64 conversion of image attachment.

Image Inclusion
---------------
Since the Send Mail approach used the POST to relay server solution the image needed to be converted to base64.
For speed of development (not getting paid for this :) I included the hardcoded base64 conversion in a JS string variable.
If the app was to select from several embedded images or even user produced images, best to convert binary to base64 on the fly.
