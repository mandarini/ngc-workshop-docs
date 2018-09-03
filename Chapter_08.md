# Chapter 08 - Fetch and post data

[previous chapter](Chapter_07.md) <----> [next chapter](Chapter_09.md) | [home](README.md)

So, at the moment we have created an Angular application, we have set up Firebase (both within the application
  using angularfire2 and on the console) we have set up authentication on the Firebase
  console and on the application and we have created a container component called `<app-chat>` which
  will our chat application (the content for authorized users).

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
2. We create a container for the messages, and inside that we are looping through the array
of messages we are going to receive from Firebase (in the next step). Then, we are passing
the received message again as a property to the `<app-message>` component.



[previous chapter](Chapter_07.md) <----> [next chapter](Chapter_09.md) | [home](README.md)
