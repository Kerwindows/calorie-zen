# ReactJS Calorie-zen


[View Live Page](https://kerwindows.github.io/calorie-zen/).

## Lists and Keys

Imagine that we're coding a messenger feature that has a list of chats sorted according to the time of the last message. Each item in this list is an instance of the Chat component. We receive the array with chat data from the API in the following form:

<pre>
<code>
const chats = [{
  id: 10,
  name: 'Gregory',
  lastMessageAt: '20:45',
}, {
  id: 5,
  name: 'Allison',
  lastMessageAt: '12:31',
}, {
  id: 3,
  name: 'James',
  lastMessageAt: '07:40',
}]; 
</code>
</pre>
<pre>
<code>
In JSX, we work with lists using the map() method:
ReactDOM.render((
  <>
    <h2>Chats</h2>
    {chats.map((chat) => (
      <Chat id={chat.id} name={chat.name} lastMessageAt={chat.lastMessageAt} />
    ))}
  </>
), document.querySelector('#root')); 
</code>
</pre>

##

The Chat component itself can be really simple:
<pre>
<code>
function Chat(props) {
  return (
    <div className="chat">
      <img src={`img/${props.id}.png`} width="75" />
      <h2>{props.name}</h2>
      <div className="date">{props.lastMessageAt}</div>
    </div>
  );
} 
</code>
</pre>


Using the code above, we can make an app that looks something like this:
image
When there is a new message in one of the chats, it should move to the top of the list. Accordingly, all other chats should be shifted down one position. But when this happens, the React algorithm will begin comparing the old element trees with the new ones, and it won't be able to understand that the node has changed, as both the node type and the component name remain the same. So, it will start looking for changes in their internal content (avatars, usernames, message time) and as a result, will end up making a lot of unnecessary changes to the DOM.
image
But we have an even more serious problem: if these components store an internal state (for example, counters of unread messages), then their values will be mixed up, because React will only replace the text content of all chats, while leaving the component instances in their initial order.
In fact, React only has to update the contents of only one chat and move its DOM element to the top of the list:
image
To help the algorithm understand that the order of elements has changed we'll use a special key attribute. The key is a unique identifier for the node, and is used as the third criterion in the comparison. React will compare new chat lists using this attribute to see if the elements have changed their order within the list.

<pre>
<code>
ReactDOM.render((
  <>
    <h2>Chats</h2>
    {chats.map((chat) => (
      <Chat
                key={chat.id}
                id={chat.id}
                name={chat.name}
                lastMessageAt={chat.lastMessageAt}
            />
    ))}
  </>
), document.querySelector('#root')); 
</code>
</pre>


Keys Outside Lists
key can be used for more than just lists. For example, after selecting a chat, it may be necessary to replace the entire message list for which the MessageList component is responsible.
Sometimes, the easiest solution is to "ask" React to completely unmount the old MessageList component and to mount a new one in its place. This can be done by assigning the component a new key that corresponds to the selected chat id:
<pre>
<code>
ReactDOM.render((
  <>
    <h2>Chats</h2>
    {chats.map((chat) => (
      <Chat
                key={chat.id}
                id={chat.id}
                name={chat.name}
                lastMessageAt={chat.lastMessageAt}
            />
    ))}

        <h2>Messages</h2>
        <MessageList key={selectedChatId} />
  </>
), document.querySelector('#root')); 
</code>
</pre>

By changing the key, we're telling React that the current instance of the MessageList component must be completely replaced with a new instance.
