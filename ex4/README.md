# Goal

Webcom provide you multiple authentication mechanisms. Email and password is one of them. 
You have a list of users for each namespace.

Let's revisit our chat sample in [exercise 3](https://github.com/webcom-components/tutorials/blob/master/ex3/README.md) and add authentication system.

# Create a namespace if needed

If you don't have a namespace, follow [exercise 0](https://github.com/webcom-components/tutorials/blob/master/ex0/README.md) to create it.

# Authentication

Let's start from exercise 3 [code base](https://jsbin.com/qusibi/edit?js,output)

To enable chat, uncomment the first line 

```javascript
// var ref = new Webcom('https://webcom.orange.com/base/<YOUR NAMESPACE>');
```

And replace token `<YOUR NAMESPACE>` by your namespace

Now, go to your dashboard and manage your namespace, and click on authentication tab.

Create a new user. You will use it in our chat app.

# Only authenticated users can send messages

Click on security tab. 

![security tab](https://raw.githubusercontent.com/webcom-components/tutorials/master/ex3/security.png)

This page helps you to edit security rules on your data. 

Watch out [exercise 3](https://github.com/webcom-components/tutorials/blob/master/ex4/README.md) for futher informations on security rules.

Let's constraint sending messages to authenticated users :

```json
{
  "rules": {
    ".read": true,
    "messages": {
      "$message": {
        ".write": "auth != null"
      }
    }
  }
}
```
Click on save button to confirm new rules.

# Test our rule

Returns to your JS Bin, and try to send a message.

...

Add DOM references on password field and login button.

```javascript
var pwdText = document.getElementById('password');
var btnLogin = document.getElementById('btnLogin');
```

New login function to add

```javascript
function login() {
  var login = nickname.value,
      password = pwdText.value;
  
  localStorage.removeItem('webcom:session');
  
  if (login && password) {
    ref.authWithPassword({
      email : login,
      password : password,
      rememberMe : false
   }, function(err, auth) {
            
      if (auth) {
        btnLogin.style.display = 'none';
      }
   });
  }
}
```

# Final result

Check final result [here](https://jsbin.com/wurizo/edit?js,console,output)

To enable chat, uncomment the first line 

```javascript
// var ref = new Webcom('https://webcom.orange.com/base/<YOUR NAMESPACE>');
```

And replace token `<YOUR NAMESPACE>` by your namespace
