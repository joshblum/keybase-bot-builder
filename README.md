# keybase-bot-builder
This is a nice wrapper for a keybase bot that has built in support for type checking and parsing. Also, it has really
easy methods for sending things to the client and receiving commands.

## Example
```typescript
import * as FS from "fs";
import * as Path from "path";
import {KBBot, KBResponse, KBMessage, KBConversation} from "keybase-bot-builder";

(async (): Promise<void> => {

	// The third parameter to the KBBot.init() static method is optional but is shown below
	// with all optional properties defined.
	const paperKey: string = FS.readFileSync(Path.resolve("./paperkey.txt")).toString("utf8");
	const bot: KBBot = await KBBot.init("otto_bot", paperKey, {
		logging: true, // whether all events should be logged
		debugging: true, // whether debugging mode should be enabled (allows extra commands)
		hostname: "my-keybase-bot", // the hostname to show up in logs
		checkAllMessages: true // whether all messages should be executed or just those that start with '!'
	});

	bot.onNewConversation(async(conv: KBConversation, res: KBResponse): Promise<void> => {

		await res.send(`Hello ${conv.getUserName()}! Nice of you to chat with me! Use a \`!\` to execute my commands.`);

	});

	bot.command({
		name: "add",
		description: "Add all the parameters together.",
		usage: "!add 1 2 3",
		handler: async (msg: KBMessage, res: KBResponse): Promise<void> => {

			const nums: (number | string)[] = msg.getParameters();
			let total: number = 0;
			for (const num of nums) if (typeof num === "number") total += num;

			await res.send(total);

		}
	});

	bot.start();

})();
```

## Documentation
Everything is completely documented. You can view the
[source code](https://github.com/elijahjcobb/keybase-bot-builder/tree/master/ts) on GitHub.

## Bugs
If you find any bugs please [create an issue on GitHub](https://github.com/elijahjcobb/keybase-bot-builder/issues) or
if you are old fashioned email me at [elijah@elijahcobb.com](mailto:elijah@elijahcobb.com).