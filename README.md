# Discord Webhook Sender
[![npm (scoped)](https://img.shields.io/npm/v/@prince25/discord-webhook-sender)](https://www.npmjs.com/package/@prince25/discord-webhook-sender)
![npm](https://img.shields.io/npm/dt/@prince25/discord-webhook-sender)
[![GitHub package.json Github](https://img.shields.io/github/package-json/v/prince25/discord-webhook-sender?color=orange)](https://github.com/Prince25/discord-webhook-sender/packages/682316)


This is an updated version of the original [package](https://github.com/matthew1232/discord-webhook-node) made by [matthew1232](https://github.com/matthew1232).
Since that package hasn't been updated recently, I decided to make a fork.
<br>

- [Installation](#installation)
- [Examples](#examples)
    - [Basic use](#basic-use)
    - [Custom embeds](#custom-embeds)
    - [Sending files](#sending-files)
    - [Preset messages](#preset-messages)
    - [Custom settings](#custom-settings)
- [Notes](#notes)
- [API](#api)
    - [Webhook](#webhook---class)
    - [MessageBuilder](#messagebuilder---class)
- [License](#license)

# Installation
```npm install discord-webhook-sender``` or ```yarn add discord-webhook-sender```

# Examples

## Basic use
```js
const { Webhook } = require('discord-webhook-sender');
const hook = new Webhook("YOUR WEBHOOK URL");

const IMAGE_URL = 'https://homepages.cae.wisc.edu/~ece533/images/airplane.png';
hook.setUsername('Discord Webhook Node Name');
hook.setAvatar(IMAGE_URL);

hook.send("Hello there!");
```

## Custom embeds
```js
const { Webhook, MessageBuilder } = require('discord-webhook-sender');
const hook = new Webhook("YOUR WEBHOOK URL");

const embed = new MessageBuilder()
.setTitle('My title here')
.setAuthor('Author here', 'https://cdn.discordapp.com/embed/avatars/0.png', 'https://www.google.com')
.setURL('https://www.google.com')
.addField('First field', 'this is inline', true)
.addField('Second field', 'this is not inline')
.setColor('#00b0f4')
.setThumbnail('https://cdn.discordapp.com/embed/avatars/0.png')
.setDescription('Oh look a description :)')
.setImage('https://cdn.discordapp.com/embed/avatars/0.png')
.setFooter('Hey its a footer', 'https://cdn.discordapp.com/embed/avatars/0.png')
.setTimestamp();

hook.send(embed);
```

Keep in mind that the custom embed method `setColor` takes in a decimal color/a hex color (pure black and white hex colors will be innacurate). You can convert colors to decimal or hex using this website here: [https://convertingcolors.com](https://convertingcolors.com)

## Sending files
```js
const { Webhook } = require('discord-webhook-sender');
const hook = new Webhook('YOUR WEBHOOK URL');

hook.sendFile('../yourfilename.png');
```

## Preset messages
```js
const { Webhook } = require('discord-webhook-sender');
const hook = new Webhook('YOUR WEBHOOK URL');

// Sends an information message
hook.info('**Information hook**', 'Information field title here', 'Information field value here');

// Sends a success message
hook.success('**Success hook**', 'Success field title here', 'Success field value here');

// Sends an warning message
hook.warning('**Warning hook**', 'Warning field title here', 'Warning field value here');

// Sends an error message
hook.error('**Error hook**', 'Error field title here', 'Error field value here');
```

## Custom settings
```js
const { Webhook } = require('discord-webhook-sender');
const hook = new Webhook({
    url: "YOUR WEBHOOK URL",

    // If throwErrors is set to false, no errors will be thrown if there is an error sending
    throwErrors: false,

    // retryOnLimit gives you the option to not attempt to send the message again if rate limited
    retryOnLimit: false,

    // If a HTTP proxy is proxy, it will send the hook via that proxy
    // Use the format, "ip:port", for IP-based authentication proxies or username:password@ip:port
    proxy: "123.456.789.10:1112"
});

hook.setUsername('Username');       // Overrides the default webhook username
hook.setAvatar('YOUR_AVATAR_URL');  // Overrides the default webhook avatar
```

# Notes
discord-webhook-sender is a promise based library, which means you can use `.catch`, `.then`, and `await`, although if successful will not return any values. For example:

```js
const { Webhook } = require('discord-webhook-sender');
const hook = new Webhook("YOUR WEBHOOK URL");

hook.send("Hello there!")
.then(() => console.log('Sent webhook successfully!'))
.catch(err => console.log(err.message));
```

or using async:
```js
const { Webhook } = require('discord-webhook-sender');
const hook = new Webhook("YOUR WEBHOOK URL");

(async () => {
    try {
        await hook.send('Hello there!');
        console.log('Successfully sent webhook!');
    }
    catch(e){
        console.log(e.message);
    };
})();
```

By default, it will handle Discord's rate limiting, and if there is an error sending the message (other than rate limiting), an error will be thrown. You can change these options with the custom settings options below.

# API
## Webhook - class
Constructor
- options (optional) : object
    - throwErrors (optional) : boolean
    - retryOnLimit (optional) : boolean
    - proxy (optional) : string

Methods
- setUsername(username : string) returns this
- setAvatar(avatarURL : string (image url)) returns this
- async sendFile(filePath : string)
- async send(payload : string/MessageBuilder)
- async info(title : string, fieldName (optional) : string, fieldValue (optional) : string, inline (optional) : boolean)
- async success(title : string, fieldName (optional) : string, fieldValue (optional) : string, inline (optional) : boolean)
- async warning(title : string, fieldName (optional) : string, fieldValue (optional) : string, inline (optional) : boolean)
- async error(title : string, fieldName (optional) : string, fieldValue (optional) : string, inline (optional) : boolean)

## MessageBuilder - class
Methods
- setText(text: string)
- setAuthor(author: string (text), authorImage (optional) : string (image url), authorUrl (optional) : string (link))
- setTitle(title: string)
- setURL(url: string)
- setThumbnail(thumbnail : string (image url))
- setImage(image : string (image url))
- setTimestamp(date (optional) number/date object)
- setColor(color : string/number (hex or decimal color))
- setDescription(description : string)
- addField(fieldName : string, fieldValue: string, inline (optional) : boolean)
- setFooter(footer : string, footerImage (optional) : string (image url))

# License
MIT
