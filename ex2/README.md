# Goal

To understand how to write data with Webcom, we will create a chat app.

User have to write its name, and when he writes a message and send it, we have to push it in a ordered way into a namespace location.

# Create a namespace

Follow [exercise 0](https://github.com/webcom-components/tutorials/blob/master/ex0/README.md) to create an account and a namespace

# Create a reference to your namespace

To start, go to this [JS Bin](https://jsbin.com/gejebif/edit?js,output). [JS Bin](https://jsbin.com) is a web online editor, useful to play with code and libraries.

Create an account, it's more convenient. Otherwise, it will reloads the editor each time you save.

HTML markup and CSS styles are already provisioned.

For this exercise, We will focus only on Javascript.

First, you need to [create a reference](https://datasync.orange.com/doc/Webcom.html) to your freshly created namespace :

```javascript
var ref = new Webcom('https://datasync.orange.com/base/<YOUR NAMESPACE>');
```

Replace `<YOUR NAMESPACE>` by your namespace.

For more details on references, check [data navigation article](https://datasync.orange.com/doc/tutorial-data-navigation.html).


# Push chat messages

To push messages, first, get DOM references on message field (msgText) and nickname field (nickname)

```javascript
var msgText = document.getElementById('msgText');
var nickname = document.getElementById('nickname');
```

Next, we have to create a `send()` function. This one is called on keypress event by message field.

```javascript
function send() {
  // Do nothing if no text in message field
  if (!msgText.value) {
    return;
  }
  if (event.keyCode === 13) {
  
	...
	
  }
}
```

Now, we need to push messages inside a child named 'messages'. To do this, we have to use [child(<location>)](https://datasync.orange.com/doc/Webcom.html#child) method. 
It returns a new reference at child location specified in parameter.

With [push](https://datasync.orange.com/doc/Webcom.html#push) method, we will add data. This one generates a key which is unique and garantees push order. 
It accepts JSON object as parameter.

```javascript
if (event.keyCode === 13) {

	// add { name: '...' , text: '...' } object with a generated key into 'messages' node
	ref.child('messages').push({
	  name: nickname.value || 'anonymous',
	  text: msgText.value
	});
	// Clear message field for convenience
	msgText.value = '';    

}
```

# Show messages in dashboard

Enter a name and a message and press enter key.

![Image of chat](https://raw.githubusercontent.com/webcom-components/tutorials/master/ex2/chat.png)

Next, go to your dashboard and manage your namespace. On data tab, you can see a child node 'messages' at root level. 

![Image of data tab in dashboard](https://raw.githubusercontent.com/webcom-components/tutorials/master/ex2/dashboard.png)

It works ! You pushed new messages in 'messages' child node.

Push new messages, you will see them automatically in 'messages' node.

Notes format of each message key.

# Display messages

To display sent messages, we have to get each one and add them into a list. 
First, get DOM reference of the list :

```javascript
var messagesList = document.getElementById('messages');
```

Next, we need to subscribe to 'child_added' event of 'messages' nodes. 
For this, we will use [on('child_added', callback)](https://datasync.orange.com/doc/Webcom.html#on) method.
Each time a new child is added under 'messages' node, callback will be fired.

```javascript
ref.child('messages').on('child_added', function(snap) {
  // get value snapshot
  var val = snap.val();

  ...
  
});
```

`snap` parameter is a [data snapshot](https://datasync.orange.com/doc/api.DataSnapshot.html) at a given time.
It is also immutable. To update data, you have to use [set](https://datasync.orange.com/doc/Webcom.html#set), 
[update](https://datasync.orange.com/doc/Webcom.html#update) or 
[push](https://datasync.orange.com/doc/Webcom.html#push) methods on a reference object.

To get JSON value, you need to call [val()](https://datasync.orange.com/doc/api.DataSnapshot.html#val) on data snapshot.

After retrieving data, we just have to create a list item with message text and add it to the HTML list.

```javascript
  ...

  var msg;

  if (val) {
  	// Create new list item with message text and it to message list
    msg =document.createElement('li');
    msg.innerText = val.name + ': ' + val.text;
    msg.setAttribute('id', snap.name());
    messagesList.appendChild(msg);
    // To always show latest messages, scroll message list to bottom
    messagesList.scrollTop = messagesList.scrollHeight;
  }

  ...
```

# Final result

Check final result [here](https://jsbin.com/tevovo/edit?js,output)

To enable chat, uncomment the first line 

```javascript
// var ref = new Webcom('https://datasync.orange.com/base/<YOUR NAMESPACE>');
```

And replace token `<YOUR NAMESPACE>` by your namespace