# FlockOS--Postmates-Bot
Dev Week 2017 Hackathon App 2

A [Flock][] app to that interacts with postmates. Built using [FlockOS][] and its [node.js sdk][].

This app is a [slash command][] bot  that allows users to order food from postmates,


## Download and Installation

```
git clone https://github.com/dangerstephen/FlockOS--Postmates-Bot.git
cd FlockOS--Postmates-Bot
npm install
```

## Getting Started

You will need an HTTPS endpoint for your app that is publicly available. To do this I used 
[ngrok][].

Take note of the public HTTPS endpoint for your app provided by ngrok. You will need this while configuring the app in the steps
below.

[Create your app](https://docs.flock.co/display/flockos/Creating+an+App) in the
developer dashboard. After creating the app, provide the following in the
advanced info section:

* Event listener URL -- your app will receive [events][] on this URL. Set this to
  `https://<your-ngrok-endpoint>/events`.

* Slash command -- configure your slash command. Set the name of the command to
  `scrap` (or anything else to your liking -- here we assume that this is what
  you've used). Provide a description and select the action "Send to event listener".

* Chat tab button -- Provide a tooltip text, select action "Open
  widget", and desktop type to "Sidebar". Set the action URL to
  `https://<your-ngrok-endpoint>/order-postmates`.
  
  * Bot -- Turn this on you, it will generate a Bot Id and Token which we will use later on `.

Save your changes. After saving them, you will be provided with your app id and
app secret (click "Show" next to the app secret to see it).

Go back to your repository root, and create a file called `config.js` with the
following info:

```js
module.exports = {
    port: 8080           // this is the default
    appId: '<app id>',
    appSecret: '<app secret>',
    botUserId: '<bot user id>',
    botToken: '<bot token>',
    endpoint: '<your-ngrok-endpoint>'
};
```

`endpoint` is the publicly available endpoint for your app.

Now start the app:

```
node index.js
```

To test the app, you need to install it into your account. You can
install the app by clicking on the "Install" button on the app page in
the developer dashboard. Make sure the app is running when you click
on "Install". For details, see [How do I install and test my
app?](https://docs.flock.co/display/flockos/Creating+an+App#CreatinganApp-AppInstallation).

## How it works

The app listens for these two events on the event listener URL
i.e. `https://<your-public-endpoint>/events`:

* [app.install][] -- on receiving this event, the app saves the user id and
  token in an in-memory database.

* [client.slashCommand][] -- on receiving this event, the app saves the scrap in
  an in-memory database. It also sends a message to all members of the
  conversation on behalf of the user that a new scrap has been created. To send
  the message, it uses the token received on `app.install` for each user who
  has installed the app.

## After Installation 
To test simply use the slash command '/order-postmates' in the intalled team

[flock]: https://flock.co
[flockos]: https://docs.flock.co
[node.js sdk]: https://github.com/flockchat/flockos-node-sdk
[ngrok]: https://ngrok.com
[events]: https://docs.flock.co/display/flockos/Events
[slash command]: https://docs.flock.co/display/flockos/Slash+Commands
[chat tab button]: https://docs.flock.co/display/flockos/Chat+Tab+Buttons
[widget]: https://docs.flock.co/display/flockos/Widgets
[app.install]: https://docs.flock.co/display/flockos/app.install
[client.slashCommand]: https://docs.flock.co/display/flockos/client.slashCommand

