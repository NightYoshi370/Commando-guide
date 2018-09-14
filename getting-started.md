# Getting Started

When you got your first bot up and running with Discord.js, you should've installed Discord.js using npm, Node.js' Package Manager. The same applies to Commando, which must be separately installed. You can do this in one of two ways:

Stable: `npm i -S discordjs/Commando#11.2`  
Master: `npm i -S discordjs/Commando`

If you're using Discord.js master, you'll need Commando master, and vice-versa.

Also note that you are going to need at least Node.js version 7.6.0. Download Node [here](https://nodejs.org/en/). If on master, you'll need at least Node 8.

#### Creating Your index File

The index file is your main file for your bot, which handles everything from registering new commands to logging in your client. There are a few extra steps that need to be done to get your bot up and running compared to discord.js.

First thing is to require Commando. Unless you will use an embed, you do **not** need to require Discord.js to use Commando. We'll also be requiring `path` for use later on. Don't worry, you don't have to install `path`.

```js
const { CommandoClient } = require('discord.js-commando');
const path = require('path');
```

Look familiar? That's just about the same way you required Discord.js in your first bot. The next step is to create a new Commando Client. There's also a few options we are going to set. Some of these are optional, but recommended.

```js
const client = new CommandoClient({
    commandPrefix: '<Insert Your Prefix Here>',
    owner: '<Insert Your User ID Here>',
    disableEveryone: true,
    invite: '<Insert a link here>'
});
```

Looks quite similar to your Discord.js Client doesn't it? Well, aside from all the options and stuff.

In `commandPrefix`, you'll need to insert the prefix you intend to have for your bot. However, you can also do a user mention for a prefix. Note that this can be changed per-guild if you set the default commands & the administrator of the server types `!prefix <insert your prefix here>`. 

After that is the `owner` field, which should contain the User ID for the owner of the bot. **The user you set here has complete control over the bot and ignores command throttling and user permissions!** So, be sure to only give this to yourself. If you have multiple owners, you can put it in an array

The third option, `disableEveryone`, simply prevents the bot from mentioning `@everyone`. This is simply to prevent things from getting annoying. After all, do you want someone mentioning everyone with the bot? Didn't think so.

The final option (`invite`) allows you to specify the link if a user wants to call you, should the bot ever break.

Next we're going to register the command groups, args types, and default commands, and register the commands in a folder called `commands`. You can name the folder whatever you want, but I personally recommend `commands` , as it... Well, it makes the most sense.

```js
client.registry
    .registerDefaultTypes()
    .registerGroups([
        ['group1', 'Our First Command Group']
    ])
    .registerDefaultGroups()
    .registerDefaultCommands()
    .registerCommandsIn(path.join(__dirname, 'commands'));
```

Remember how I said we'd need path? There you go.

Doing this we've also created our first command group, in the folder `group1` in our `commands` folder. It will be displayed in the help command as 'Our First Command Group'. You can name both of these whatever you like.

Should you want to disable a default command, say, the help command, you can pass that in `registerDefaultCommands`.

```js
.registerDefaultCommands({
    help: false
})
```

Next, we're going to create a ready event, which is about the same as the one in your first Discord.js Bot.

```js
client.on('ready', () => {
    console.log('Logged in!');
    client.user.setActivity('game');
});
```

This will send `Logged in!` to your console when the bot is ready, and set the Bot's Game to 'Game'. Set that to whatever you wish.

Last but certainly not least, let's log the bot in.

```js
client.login('Your Secret Token');
```

You can also use environment variables or a `config.json` for your token instead of passing it directly.

And there you have it! You've set up your `index.js` file! You should now create the `commands` and `group1` folders, in the end your file structure should look like this, along with whatever `.gitignore` or `config.json` you may have:

```markdown
commands
- group1
index.js
package.json
```

And your `index.js` file should look something like this:

```js
const { CommandoClient } = require('discord.js-commando');
const path = require('path');

const client = new CommandoClient({
    commandPrefix: '<Insert Your Prefix Here>',
    unknownCommandResponse: false,
    owner: '<Insert Your User ID Here>',
    disableEveryone: true
});

client.registry
    .registerDefaultTypes()
    .registerGroups([
        ['group1', 'Our First Command Group']
    ])
    .registerDefaultGroups()
    .registerDefaultCommands()
    .registerCommandsIn(path.join(__dirname, 'commands'));

client.on('ready', () => {
    console.log('Logged in!');
    client.user.setActivity('game');
});

client.login('Your Secret Token');
```

In the next section, we'll create our first Commando command! How exciting! See you then!

