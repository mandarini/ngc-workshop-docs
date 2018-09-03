# Chapter 05 - Set up Authentication

[previous chapter](Chapter_04.md) <----> [next chapter](Chapter_06.md) | [home](README.md)

Now it's time to set up Authentication for our application!

## Set up Authentication in the Firebase Console

First off, you need to set up the Authentication in your Firebase Console.

1. On the side menu of your project in your Firebase Console, click *Authentication*.
2. On the __Sign-In method__ tab, choose *Google*. We will use Google Authentication
  for our project today. You can use any other provider you want in your future
  projects.
3. Click *enable* and *save*
4. Scroll down to authorized domains. These are domains that Firebase has authorized
to use Authentication. Check that `localhost` is there, for our development purposes.
Once we deploy, we will add the domain of our deployed application.

## Set up Authentication in our application

What we need here, essentially, is a way to block the content from users who are
not logged in (authenticated).

So, we will create a sign-in button, and a conditional, showing the users
the content if they are signed in, or the sign-in button if they are not signed in.

### Separating private content
First, let's create a component that will contain the part of our application which
we want to be inaccessible by non-authenticated users. Let us say that
this component will be the `chat` component, containing all the chat app.
In your console, type:
```
ng g c chat
```
which is short for `ng generate component chat`.

Now that we have the separate component, let us see how we are only going to show
it to the authenticated users.

### Creating the conditional

In your `app.component.html` file, replace all the code with the following:
```
<div *ngIf="afAuth.user | async as user; else showLogin">
  <h2>Hello {{ user.displayName }}!</h2>
  <app-chat [userAuth]="user.displayName"></app-chat>
  <button class="logout-btn" (click)="logout()">Logout</button>
</div>
<ng-template #showLogin>
  <p>Please login.</p>
  <button (click)="login()">Login with Google</button>
</ng-template>
```

As you can see, we are adding a conditional, which checks if there is an
authenticated user present. In this case, the user is greeted, a log out button is
displayed, along with our chat component (`<app-chat>`).
If there is no authenticated user, the `showLogin` template is displayed, prompting
the user to login.

#### Small note
Notice how we are passing the user's display name (_userAuth_) down into our child component (`<app-chat>`)
as a property, `[userAuth]`.
In order for this to work, we have to define this property as an input in our
`ChatComponent`. So, open your `chat.component.ts` file and do the following:
1. Add `Input` to your imports from `@angular/core`.
2. On top of your chat component class, add the following line:
```  
@Input() userAuth: string;
```
The `@Input` decorator declares the `userAuth` component property so that we can
use it in our component.

### Creating the handling

In your `app.component.ts` file, add these in your imports:
```
import { AngularFireAuth } from 'angularfire2/auth';
import { auth } from 'firebase/app';
```

In your app constructor, declare the AngularFire Authentication like this:
```
constructor(public afAuth: AngularFireAuth) {
}
```
and create the functions for log in and log out, inside your
AppComponent class. Copy and paste the following code below the constructor:
```
login() {
  this.afAuth.auth.signInWithPopup(new auth.GoogleAuthProvider());
}
logout() {
  this.afAuth.auth.signOut();
}
```
This is the default code to set up Authentication with Google.

### Test what we did
Let us test it now. We should check that the conditions work as expected.

[previous chapter](Chapter_04.md) <----> [next chapter](Chapter_06.md) | [home](README.md)
