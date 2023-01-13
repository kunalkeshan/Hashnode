# The Life Cycle of Developing a Website 🚴‍♀️

As Developers we love jumping to code and working on our ideas immediately. This spontaneity is what helps us learn and explore new things and get better at what we do.

Sometimes this very spontaneity can lead to frustration when you are working on large projects. When you've used the wrong framework, the incorrect database, or have implemented a feature way too early, or tying to add too many things before actually testing them out.

Here's a framework that can save you tons of time in your development process.

For understanding purposes let's take an example of a simple social media application that allows you to put up text posts (something like Twitter).

### 💡 Idea

The Ideation stage is where you get the idea to build a web app. This is where you're the most excited and motivated to make it and get the rush to start on it immediately. But wait! What features will the web app actually have?

## 🔦 Planning the MVP

Considering it's a Twitter-like application, the Minimum Viable Product (MVP) of the app should consist of the following features,

\- A Login and a Signup, with a, forgot password feature

\- User is able to update their profile details

\- Able to make text posts

\- Edit and delete their posts

\- View a feed of posts

\- View posts of other users

\- Follow other users

This should be enough to get started. The MVP is should consist of just enough features to get your idea up and running. You can always add more later. Your MVP should focus on being good enough, it doesn't have to be perfect.

### 📚 Requirements

In the Requirements phase, you'll plan out what you'll need to implement your MVP. From the tech stack, to how much time you'll need to work on it, will be using a CDN, where you will host the application is all covered here.

For the social media example, for the front end, we'll use something like Next.js or any other SSR/SSGs-supported framework to facilitate SEO for the user's profile and their posts made. Along with that, we'll use Tailwind CSS for styling.

A database, preferably a relational one to relate the users and their posts made. The following feature would also be easily implemented with the help of a relational database.

A backend to build custom endpoints for our application can be built using Node.js / Express.js. We can also implement web sockets using [Socket.io](http://Socket.io) for real-time feed updates.

### 🎨 Design

The look and feel of the website. Your website's typography, colour scheme, and base styling preferences all come under the Design phase of the application. You can use tools of your preference. Figma and Web Flow are great tools that help in designing and prototyping your website.

You can always use other platforms like Dribble and Behance for inspiration. 😜

### 💻 Development

This is where the fun begins. With the help of the last four stages, you'll have a clear idea of how to approach your code and even if you get stuck, you can always go back to your plans and get them sorted out quickly.

You may or may not work in a team, however, it's always best to use a VCS like Git to keep track of your changes and use GitHub if you're working on a team. Keep separate branches for your work and merge them as and when you're done with them.

### 🧪 Testing

Not so fun part for many, but it's one of the most important ones. There are a lot of different types of tests out there, and you should try and cover the ones that are important for your needs.

If I'm working on the signup feature, I'd preferably create a branch called feat/signup using Git, run tests in that branch, then merge it to the main branch and test it out there one more time with the already existing features.

### 🌐 Deployment

After you're done with the MVP, it's time to make it public and open to access. And to do that, you'll have to deploy your app to the web. For the social media app (I've used Next.js) I'll deploy it using Vercel as of now because I'm not expecting high traffic immediately. I've using PostgreSQL to host my data.

You've already covered the cost requirements in the Requirements phase here, so it should be pretty much smooth out here.

### ⚙ Maintenance

Deploying your website is not the end, once you've put it out there, you will have bugs to face, new features to implement and maybe the project really hits off, so you'll need to consider scaling as well.

At the maintenance phase, you can consider increasing your costs for keeping the app up and running, maybe getting a team to resolve the bugs, or making your application open source and putting those out as issues.

### 🏗 Retirement

Your project has a good run, or maybe it hasn't. The retirement phase, often least or not thought of at all is how you'll take down or stop the project altogether.

This being a web project, there are usually no costs associated with it. (But it does come in handy for construction projects where you're building a new property and have to take it down later. It can consist of manpower costs, utility costs, etc)

What flow do you usually follow when you approach building a website? Is there anything that can improve the life cycle mentioned above?