# Chapter 09 - Add Styles

[previous chapter](Chapter_08.md) <----> [next chapter](Chapter_10.md) | [home](README.md)

Now that all functionalities are in place, we can start giving this 1998 mess a more
beautiful appearance.

## General styles
Let's start with our general styles.
Say we need all our buttons to have the same look and feel.
Open your `styles.css` file and add the following code:
```
button {
  position: relative;

  display: block;
  padding: 16px;

  text-transform: uppercase;

  overflow: hidden;

  border-width: 0;
  outline: none;
  border-radius: 2px;
  box-shadow: 0 1px 4px rgba(0, 0, 0, .6);

  background-color: #2ecc71;
  color: #ecf0f1;
}
```
We are just making our buttons look a bit more nice, with some padding and a shadow.
In the end you can customize them as more as you like, but for now we are ok.

Let's add, also, a rule for our font:
```
html {
  font-family: 'Roboto', sans-serif;
}
```
We don't want any serifs showing up out of nowhere, this is not the New York Times.

## App header styles
Our app header resides in our `app.component`, so let's go to our `app.component.css`
to fix this.
At the moment we don't care much about anything else but our logout button position,
which will be in the way of our chat content. So, let's put this button where it
should be, on the top right of our application, stuck forever.

In our `app.component.css` type the following:
```
button.logout-btn {
  position: absolute;
  top: 15px;
  right: 15px;
}
```
(our `<button>` element in our `app.component.html` template already has the class 'logout-btn'. If it
doesn't, add it there quickly!).

## Chat container styles
Open your `chat.component.css` and type the following code:
```
.full-container {
  height: 85vh;
  display: flex;
  flex-direction: column;
}

.msg-container {
  width: 100%;
  overflow-y: scroll;
  padding-bottom: 20px;
  flex-grow: 1;
  display: flex;
  flex-direction: column;
  align-self: flex-end;
}
```
What we are doing here is this: We are using flex to position our two elements,
the input container and the messages container. The `flex-direction: column;` will
position the two elements one bellow the other.
The `flex-grow:1` property of our message container, will make it cover all
available space left by our input container, which has a defined height and position!

The `align-self: flex-end;` property will make all the contents of our message container
stick to the bottom of our container. In this way, we will have our messages
appear at the bottom, as you would expect of a messaging application interface.
Again, the flex direction and the `display:flex;` properties make the content of
our message container (the messages) display in a column, one bellow the other!

### Scroll to bottom
However, there is one extra thing that we need here! We want our chat container, which is
scrollable (`overflow-y: scroll;`), to scroll to the bottom every time new content is loaded.
Just as a chat application would do. Once loaded, the container would be scrolled to the bottom,
and in order to see older messages you would have to scroll up.

So, how do we do this?
We need a reference to our chat container HTML element, so that we can manipulate it
in our component. We also need this to run after our view is checked and updated.
So, first of all we import the necessary things from `@angular/core`:
```
import { Component, OnInit, Input, AfterViewChecked, ElementRef, ViewChild } from '@angular/core';
```

Then, we create a reference to the message container element, like this:
```
@ViewChild('msgContainer') private messagesContainer: ElementRef;
```

Now, we need to create a method that will scroll our element to its bottom:
```
scrollToBottom(): void {
    try {
        this.messagesContainer.nativeElement.scrollTop = this.messagesContainer.nativeElement.scrollHeight;
    } catch(err) { }
}
```

Finally, we need to call this method both at the beginning of our application, and
also after a change is made. So, we call it in the end of `ngOnInit()`,  and we also call it
in `ngAfterViewChecked()`:
```
ngAfterViewChecked() {
    this.scrollToBottom();
}
```
And now we are good to go!

## Input styles
First of all, we need to modify our template code. Open your `input.component.html`
and type the following code:
```
<div class="input-container">
  <input #chatMsg name="chat-msg" type="text" placeholder="Type your message">
  <button (click)="sendMsg(chatMsg.value)">
     Post
  </button>
</div>
```
We just wrap everything in a `<div>`.

Open your `input.component.css` and type the following code:
```
.input-container {
  width: 100%;
  margin: 0;
  position: fixed;
  bottom: 0;
  background-color: #ffffff;
  border-top: 1px solid rgba(0, 0, 0, 0.12);
  padding: 7px 8px 7px 8px;
  display: flex;
  flex-direction: row;
}

input[type=text] {
  flex-grow: 1;
  padding: 17px 20px;
  margin: 0;
  background-color: #fafafa;
  box-sizing: border-box;
  border-radius: 1px;
  border: 0px;
  box-shadow: 0;
}

button {
 margin-right: 30px;
}
```
Here, we are positioning our input container at the bottom of our chat container. We are also giving it
some styles and a flex direction of row, so that the submit button is in the same row as our input field.
We also give some styling to our input field, and again a `flex-grow:1;` so that it
can cover all remaining space from the button!

## Message Styles
First of all, we need to modify our template code. Open your `message.component.html`
and type the following code:
```
<div class="chat-msg" [ngClass]="{'other': userAuth !== message.user, 'own': userAuth === message.user}">
 <span>
   {{message.user}} - {{message.msg}}
 </span>
</div>
```
Notice how we wrap each message in a div and give it a conditional class.

Open your `message.component.css` and type the following code:
```
.chat-msg {
  display: inline-block;
  padding: 8px;
  margin-bottom: 8px;
  font-size: 14px;
  width: auto;
  max-width: 90%;
  word-wrap: break-word;
  border-radius: 3px;
  border-style: solid;
  border-width: 0;
}

.chat-msg.own {
  background-color: rgb(159, 124, 150);
  color: white;
  float: right;
}

.chat-msg.other {
  background-color: rgb(224, 224, 224);
  color: black;
  float: left;
}
```

Now, each message will have the width of the message it contains (`display: inline-block;`
  helps us here), and a maximum width of 90% of the parent component.

### Conditional Classes
Of course, we want to distinguish between our own messages and the
messages of others. So, we color our messages differently, and we position our own
messages to the right, whereas the messages from other users to the left. Much like
most messaging applications (think of Facebook Messenger, WhatsApp, Viber etc).
In order to achieve that, we use `[ngClass]`, which makes sure that when
 the active user (`userAuth`) is the same as the `message.user` username, then
 we are styling our own messages. ([Here](https://toddmotto.com/ng-class-angular-classes) is
 a nice class by Todd Motto on conditional classes).


[previous chapter](Chapter_08.md) <----> [next chapter](Chapter_10.md) | [home](README.md)
