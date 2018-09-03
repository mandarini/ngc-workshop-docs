# Chapter 08 - Fetch and post data

[previous chapter](Chapter_07.md) <----> [next chapter](Chapter_09.md) | [home](README.md)

So, at the moment we have created an Angular application, we have set up Firebase (both within the application
  using angularfire2 and on the console) we have set up authentication on the Firebase
  console and on the application and we have created a container component called `<app-chat>` which
  will our chat application (the content for authorized users).
  We have also created our data collection and the ways to fetch data from it and post
  data to it!

Now, we need to create any other components we think we might need.
It is quite useful and makes a cleaner code if we break up our application
in its building parts, and create a component for each separate element we might
think we have.

As discussed while laying out our application, we needed three separate
components. One is the enclosing component (`<app-chat>`) that will contain the messages
and the input field. The other two are the input field component, and the message component!

Let's see how we are going to create these!

## Creating the Chat container component

Since we have already generated this component in the previous step, let's see
what code we are going to put here!

Let's start by thinking how this component will look:

* It will have to be right beneath our app head, which is the greeting message and the
log out button (as laid out in the parent component `<app-root>`).
* It will contain one space where all the messages will go, which will have to
scroll.
* It will contain an input field which will have to be always visible, and stuck to the
bottom of our screen.
* Also, it would be good if the whole thing is wrapped in a container, so that we can style
it properly.

So, our code would preferably look something like this:

```
<div class="full-container">
 <app-input [userAuth]="userAuth"></app-input>
 <div class="msg-container" #msgContainer>
     <app-message *ngFor="let msg of messages | async;" [message]="msg" [userAuth]="userAuth"></app-message>
 </div>
</div>
```

Notice two things here:
1. We are passing the user's display name (_userAuth_) down into our children components
as properties, `[userAuth]`. The same way as we passed this property down to our `<app-chat>`
component from `<app-root>`.
2. We create a container for the messages, and inside that we are looping through
the `messages` array that we received from Firebase in the previous step. Then, we are passing
each received message again as a property to the `<app-message>` component.

At the moment this will not work since we do not have the `<app-input>` and `<app-message>`
components. So, let's create them.

## Creating the Message component

In your console, type:
```
ng g c message
```
which is short for `ng generate component message`.

Now, since we are passing two properties in this component from our parent component,
we have to declare them with the `@Input` decorator:
1. Add `Input` to your imports from `@angular/core`.
2. On top of your message component class declare the two properties (`message` and `userAuth`).

In our template, we just need to show the message, so we just need to type:
```
<div>
   {{message.user}} - {{message.msg}}
</div>
```
Do this and see the result in your screen.
Obviously, at the moment there are no styles, so you will just see some plain text.
We will add styles in a bit.

## Creating the Input component

In your console, type:
```
ng g c input
```
which is short for `ng generate component input`.

Now, don't forget to declare the property you are passing from the parent
component using the `@Input` decorator in your input component.

In our template, we need an input field and a button to submit it.
Type:
```
<input #chatMsg name="chat-msg" type="text" placeholder="Type your message">
<button (click)="sendMsg(chatMsg.value)">
   Post
</button>
```
Notice how we are adding a reference to our input element, which we use in our button
to retrieve its value.
On clicking the button, the sendMsg() method is called with the value typed in the
input field. So, let us create this method!

### The method for sending the message
On top of your component, import the `AppService`, where we have already created a
method to send messages to our Firestore collection.
```
import { AppService } from '../app.service';
```
In our constructor, let's declare the `AppService` as `msgService`:
```
constructor(private msgService: AppService) { }
```
so that we can use it in our `sendMsg()` method.

And now, let's create our `sendMsg()` method!
If the click send and the message is not empty, then call the `.addMsg()` method
of our `AppService` with the message:
```
sendMsg(msg) {
  if (msg !== null) {
    let message = {
      msg: msg,
      user: this.userAuth,
      timestamp: firebase.firestore.FieldValue.serverTimestamp()
    };
    this.msgService.addMsg(message);
  }
}
```
So, here, we create a new `message` object that contains the message text,
the user's name, and a timestamp!
Notice how we are not using `Date.now()` and we are using the `.serverTimestamp()`
method of the Firebase Firestore. This is preferable, since we want our messages
to have the server's timestamp (when they were received and added in our database),
and not our local system's one (when they left our system).
In order to use this method, we have to import `firebase`:
```
import * as firebase from 'firebase';
```

### Ready to go!

Now we are ready!
Let's try to post some messages and see what happens!
Look at your user interface, and look at your console as well!


[previous chapter](Chapter_07.md) <----> [next chapter](Chapter_09.md) | [home](README.md)
