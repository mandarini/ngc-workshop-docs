# Chapter 10 - Deploy

[previous chapter](Chapter_09.md) | [home](README.md)

Now that all is said and done, we are ready to deploy!
A nice step by step guide found in the Angular-CLI GitHub repo is [here](https://github.com/angular/angular-cli/wiki/stories-github-pages).

We are going to follow this guide and maybe explain some things on the way!

## Pushing our project to GitHub

First of all, your project must be published to GitHub. The following steps
are very explanatory, in case someone has never used git before, so bear with me here!
If you are a pro, just push your project to GitHub and move to the next step!

1. Go to your GitHub profile, and on the top right click the plus sign (__+__) button
to create a _New Repository_.
2. Name your new repository. For your own convenience, it might be better if you
give your new repository the name you already have for your project.
3. Leave all the other settings as they are and click _Create Repository_.
4. In your terminal, in your project folder, type:
```
git init
```
This command will initialize an empty git repository in your folder. We are not
going to explain how git works at the moment.

_However, you can think of a git
repository as a bucket that waits to be filled with your code. At the moment it is empty.
Each time you change your code, you have to say that you changed it (`commit` with
  an explanation message, that will act as a milestone), and then you have to add
  your new code to that bucket again.
  The bucket will take care to keep track of all these changes. I'm sure this is not
a very good explanation of git, but it will do for now._

Now, as we explained, you have to tell git that you have new code (like a milestone)
 and then add your code to your empty repository. So, in your terminal type:
```
git commit -m 'initial commit or whatever else you want'
```
and then
```
git add --all
```
to add all the code in the repository.
5. Now that all code is added in our local repository, we have to upload it (push it)
to GitHub. So, from the GitHub page of your empty new repository, copy the line
that adds a remote origin to your local repository:
```
git remote add origin https://github.com/YOURUSERNAME/YOUR-PROJECT-NAME.git
```
We will be using `https` instead of `SSH` to simplify things for you, for the moment.
This will connect your local repository to the repository on GitHub (remote).

Then, just push your local copy to GitHub, typing:
```
git push -u origin master
```
6. Refresh the page on your GitHub and see that all your code is there!

## Publishing our project on GitHub pages

Following the steps of [this](https://github.com/angular/angular-cli/wiki/stories-github-pages)
awesome guide here:

1. In your terminal, in your project folder, type:
```
ng build --prod --output-path docs --base-href https://YOURUSERNAME.github.io/YOUR-PROJECT-NAME/
```
This will build your project, and add as a base link to it the future link of
your project (all your projects that are published on GitHub pages are under
  `https://YOURUSERNAME.github.io/`). It will also add the build in a folder named
  `/docs`.

2. As it says in the guide, _Make a copy of docs/index.html and name it docs/404.html_.
This will be your 404 page, obviously.

3. Now, notice how we have made changes to our project! So, what do we do?
  * We tell git that we made changes:
    ```
    git commit -m 'new build'
    ```
  * We push these changes
    ```
    git push
    ```
Everything is uploaded on GitHub now.

4. Go to the GitHub page of your project and go to the _Settings_ tab.

5. Scroll all the way down to the _GitHub Pages_ section.

6. As _Source_, select __master branch /docs folder__ and click _Save_.

7. Now your project is published!

_An extra tip:
You might want to hide API keys and configuration from your "unbuilt" code.
In that case you can remove them from the `app.module.ts` and push your code again.
The build in the docs folder will not be affected of course (until you build it again).
This does not guarantee security, of course, since anyone can see the API keys in
past commits or in the build, however it's just a small thin layer of
security._

[previous chapter](Chapter_09.md) | [home](README.md)
