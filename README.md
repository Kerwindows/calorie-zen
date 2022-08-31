# ReactJS Calorie-zen


[View Live Page](https://kerwindows.github.io/calorie-zen/).

## Lists and Keys

Keys Outside Lists
key can be used for more than just lists. For example, after selecting a chat, it may be necessary to replace the entire message list for which the MessageList component is responsible.
Sometimes, the easiest solution is to "ask" React to completely unmount the old MessageList component and to mount a new one in its place. This can be done by assigning the component a new key that corresponds to the selected chat id:


By changing the key, we're telling React that the current instance of the MessageList component must be completely replaced with a new instance.
