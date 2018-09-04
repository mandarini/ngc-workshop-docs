# Chapter 07 - Fetch and post data

[previous chapter](Chapter_06.md) <----> [next chapter](Chapter_08.md) | [home](README.md)

Now that we have created our collection in the Cloud Firestore through our console,
we can start fetching and posting data from and to it.

## Create an interface for our Message objects

First of all let us define how our Message objects will look. We will create an
interface to define the requirements of these objects, so that we can later use it
as a type for the contents of our collections.

In the app folder, create a file named `message.model.ts` and type the following code in it:
```
export interface Message {
    user: string;
    msg: string;
    timestamp: string;
}
```
Notice how we just defined how our Message object looks. The same way we created a
message in the previous step in our Firestore collection. We want our message to contain
three strings, for _user_, _msg_ and _timestamp_.

## Create a service that will handle the Firestore functions

This step is optional, but it makes things easier, especially if in the future
we need to handle more functions than just adding a new message (eg. in CRUD interfaces).
For the moment, it is just an additional step that will just set a foundation for any
future needs.

In your console, type:
```
ng g s chat
```
which is short for `ng generate service chat`.

This will create a new service called `ChatService` in the file `chat.service.ts`.
Open the file, and add the following imports:
```
import { Message } from "./message.model";
import { AngularFirestore, AngularFirestoreCollection } from "angularfire2/firestore";
```
We are importing our Message interface because we will need it to tell the collection
what type of data to expect.

In the `ChatService` class define the `messages` as a Firestore Collection like this:
```
messages: AngularFirestoreCollection<Message>;
```
and then define Firestore in the constructor and fetch our `messages` collection like this:
```
constructor(private db: AngularFirestore) {
   this.messages = db.collection<Message>("messages");
}
```

Now we have linked our Angular application with our Firestore collection! We can,
therefore, call the `.add()` method of Firestore (for official documentation and more
  examples and use cases see [here](https://firebase.google.com/docs/firestore/manage-data/add-data)).

We create a new method called `addMsg` so that we can call it from our components later, like this:
```
addMsg(msg) {
  this.messages.add(msg);
}
```

Now our service is ready, and we can go to our `ChatComponent` to fetch the messages collection.

## Fetching the Firestore collection

Go to your `chat.component.ts` file and import `AngularFirestore` and `AngularFirestoreCollection` from `angularfire2/firestore`.

Import also `Observable` from `rxjs` and our `Message` interface.

On top of our component, we define two variables:
```
messages: Observable<any[]>;
private msgRef: AngularFirestoreCollection<Message>;
```
An Observable named `messages` that will contain all our messages from Firestore,
and a Firestore Collection reference, called `msgRef` that will be a collection of
Message objects.

Again, in our constructor, we define Firestore and then we connect Angular
to our collection. Notice how here we query our data ordered by the timestamp,
this way making sure that we receive the messages in the correct order!
```
constructor(private db: AngularFirestore) {
  this.msgRef = db.collection<Message>('messages', ref => ref.orderBy('timestamp'));
}
```

In our `ngOnInit` method, that is _"after Angular has initialized all data-bound properties"_ ([OnInit hook](https://angular.io/api/core/OnInit)), we use the `.valueChanges()` method to receive all
the messages in our collection. Each time a new message is added, our `messages` will be updated.
```
this.messages = this.msgRef.valueChanges();
```

If you want to see the contents of `this.messages` on your console for debugging purposes,
or if you need to manipulate them in any way, you can subscribe to the observable get
its contents like this:
```
this.messages.subscribe(res => console.log(res));
```

However, for our application now, we will not be needing this.

## Ready to move on

So, now we have seen how we can add new messages and how we can fetch all messages!
So, let's move on to start fixing our user interface at last!

[previous chapter](Chapter_06.md) <----> [next chapter](Chapter_08.md) | [home](README.md)
