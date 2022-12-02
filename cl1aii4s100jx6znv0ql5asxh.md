# Create a Contact Form with Node, Express, Nodemailer, and TailwindCSS.

In this tutorial, I'll show you how to make a contact form using Node, Express, Nodemailer and TailwindCSS. We'll set up a custom route to accept form responses, serve the HTML file to the browser, add functionality to the form and style it using TailwindCSS. 

## Prerequisites

1. NodeJs is installed in your system. If not, install it from [here](https://nodejs.org/en/). 
2. Basic understanding of HTML and CSS.
3. Basic understanding of Express. 

Here's the GitHub Repo Link to this Project if you want to skip to the code directly.
[https://github.com/kunalkeshan/node-express-nodemailer-tailwindcss-contact-form](https://github.com/kunalkeshan/node-express-nodemailer-tailwindcss-contact-form)

Let's dive into it! 

---

## Getting Started

- Create a new project (If you've not made one already). I'll call it `node-express-nodemailer-tailwindcss-contact-form` 😅 and open your terminal or VS Code within this project.
- Run npm init -y to start a node project.
- Install the following dependencies in the project. 

```bash
npm i express nodemailer dotenv
npm i -D tailwindcss postcss autoprefixer nodemon concurrently
```

Express and Nodemailer are the core packages that will allow us to add the contact form functionalities and dotenv is to protect our email password.
 
TailwindCSS, PostCSS, and Autoprefixer are development dependencies that allow us to use tailwind classes in our project, generate a clean output CSS(postcss) file with cross-browser compatibility (autoprefixer).
 
Nodemon and Concurrently are development dependencies that allow the server to restart when there are new changes(nodemon) and run multiple scripts together(concurrently).

- Add the following scripts to the package.json.

```json
"start": "npm run build && node index.js",
"dev": "concurrently \"nodemon index.js\" \"npm run tailwind:watch\"",
"build": "npm run tailwind",
"tailwind": "npx tailwindcss -i tailwind.css -o public/style.css",
"tailwind:watch": "npx tailwindcss -i tailwind.css -o public/style.css --watch"
```

- You'll need three parts for this project, a request handler, a function to send the email and the frontend with the functionality. 

---

## Contact Form Request Handler

Create a `index.js` in the root of your project and let's take a look at it part by part.

- Import all the dependencies required. Note: See how dotenv is imported before all the custom functions as we'll need it to access the environment variables in the `.env` file.

```javascript
/** index.js
* Contact Form Application
*/

// Dependencies
const express = require('express');
const path = require('path');
require('dotenv').config();
// const { sendContactEmail } = require('./mailer');
```

- Set up Middlewares. 
Express JSON middleware is used to parse incoming requests as JSON. Express URLencoded middleware is used to parse URL encoded requests and attach them to the request body and finally Express static is used to serve the public files to the browser.
 

```javascript
// Initializing Express App
const app = express();

// Setting up middleware
app.use(express.json());
app.use(express.urlencoded({ extended: false }));
app.use(express.static(path.resolve(__dirname, 'public')));
```

- Contact form Route.
Nothing too complicated. A route that accepts POST requests at `/api/contact`. A basic contact form will collect the name, email and message of the person who wants to contact you, so at the start, we're destructuring those details from the request body. 
Next, we're passing on the details to a mailer function (which we'll get to in a few) and if all goes well we respond with success and a status 200 and if anything goes wrong, the catch block responds with an error and a status 400.

```javascript
// Application routes
/**
* @description Accept contact form data and send it to the server
* @api POST /api/contact
* @data {string} name, {string} email, {string} message
* @access Public
*/
app.post('/api/contact', async (req, res) => {
// Collecting required information from the Request Body
const { name, email, message } = req.body;
try {
// Sending the email
// await sendContactEmail({ to: email, name, message });
res
.status(200)
.json({
message: 'Email sent successfully',
data: { name, email, message },
success: true
});
} catch (error) {
console.log(error);
return res
.status(400)
.json({
message: 'Unable to process request',
data: {},
success: false,
})
}
})
```

- Start the Server.
We're extracting the PORT from the environment and if one is not available, we assign it a value of 3000. Next, we start the server using the app listen method.

```javascript
// Initialzing Server
const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
console.log(`Server running at http://localhost:${PORT}`);
});
```

---

## Mailer Function
Create a `mail.js` file in the root and let's explore its content part by part. 

- Importing all Dependencies. Along with Nodemailer, we're getting our email and password from the `.env` file as well (We'll see more about it in a few). 

```javascript
/** mail.js
* Node Mailer Setup
*/

// Dependencies
const nodemailer = require('nodemailer');
const email = process.env.MAIL_EMAIL;
const password = process.env.MAIL_PASSWORD;
```

- Creating a Mail Transport. We're using Gmail as the nodemailer service, you can use any SMTP that you wish, Gmail is easy and quick to set up. 

```javascript
// Mail Transporter
const transport = nodemailer.createTransport({
service: 'gmail',
auth: {
user: email,
pass: password,
},
from: 'Kunal Keshan <example@gmail.com>'
});
```

- Function to send the contact form submission to your email. Using the `transport` `sendMail` method, and setting up the options, the function send's email to your account. 
You can send HTML or replace it with text as well if you want something simple. And finally, we're invoking the `sendMail` with the options and returning it. 

```javascript
/**
* @description Send email to the user
* @param {object} options
* @param {string} options.to
* @param {string} options.subject
* @param {string} options.message
*/
exports.sendContactEmail = ({ to, name, message }) => {
    const mailOptionsToOwner = {
        to: email,
        subject: `Contact Form Submission from ${name} <${to}>`,
        html: `
            <h1>Contact Form Submission</h1>
            <p>Name: ${name} <${to}></p>
            <p>${message}</p>
        `
    }

    const mailOptionsToUser = {
        to,
        subject: 'Thanks for contacting me!',
        text: 'I will get back to you soon!'
    }

    return Promise.all([transport.sendMail(mailOptionsToOwner), transport.sendMail(mailOptionsToUser)]);
}
```

- Create a `.env` file and add your email and password to it. To add a Gmail account as a nodemailer service, you'll need to have 2FA enabled and you'll have to create an app password. Check this out on how to do it. 


```env
# .env
# NODEMAILER CONFIG
MAIL_EMAIL=<example@gmail.com>
MAIL_PASSWORD=<app password here>
```

- In the index.js uncomment the import and invoke of the `sendContactMail()` function. 

---

## The Frontend
- Setting it up.
Create a `public ` directory in your project and three files in it - `index.html`, `style.css`, and `script.js`.

At the Root of the project, create a `tailwind.css` file and add the following lines to it. 

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

In your command line run - `npx tailwindcss init -p`. This will create two files `tailwindcss.config.js` and `postcss.config.js` in your project. 
Open `tailwindcss.config.js` and you'll notice that you'll have the following object configuration present already.
 

```javascript
module.exports = {
content: [],
theme: {
extend: {},
},
plugins: [],
}
```

Modify the `content: []` and add this to it `content: ["./public/**/*.html"]`. This is telling tailwindcss to watch the classes in the HTML files.
Do you remember the scripts that we added, in the beginning, to the `package.json`? Here's it another time.

```json
"start": "npm run build && node index.js",
"dev": "concurrently \"nodemon index.js\" \"npm run tailwind:watch\"",
"build": "npm run tailwind",
"tailwind": "npx tailwindcss -i tailwind.css -o public/style.css",
"tailwind:watch": "npx tailwindcss -i tailwind.css -o public/style.css --watch"
```

We use the tailwind cli to watch any changes that we do to our project (specifically the HTML files as we mentioned in the content for the tailwind config) and output the classes we used with the styles into a styles.css in the same public directory. Notice that the --watch flag is being used to keep track of changes, something similar to nodemon. 

We also have a build script that runs the tailwindcss cli and outputs the style in a separate file. 
It's a keep only what you use approach.

### HTML - index.html

Add the following code to the `index.html` file.

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Contact Form</title>
<link rel="preconnect" href="https://fonts.googleapis.com" />
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
<link href="https://fonts.googleapis.com/css2?family=Open+Sans:ital,wght@0,300;0,400;0,500;0,600;0,700;0,800;1,300;1,400;1,500;1,600;1,700;1,800&display=swap" rel="stylesheet" />
<link rel="stylesheet" href="/style.css" />
</head>
<body class="w-full h-screen flex flex-col gap-2 items-center justify-center bg-gradient-to-br from-green-400 to-blue-500 text-center">
<h1 class="text-2xl font-semibold">Contact</h1>
<form id="contact" class="w-full p-2 flex flex-col md:w-2/3 gap-2 lg:w-1/2" autocomplete="off">
<input type="text" name="name" id="name" placeholder="eg: John Smith" required class="py-1 px-2 border border-black rounded" />
<input type="email" name="email" id="email" placeholder="example@gmail.com" required class="py-1 px-2 border border-black rounded" />
<textarea name="message" id="message" placeholder="Hey! Let's get in touch, I want to..." required class="py-1 px-2 border border-black rounded resize-y" ></textarea>
<button type="submit" class="bg-blue-500 py-2 px-1 rounded text-md w-fit mx-auto font-semibold text-white hover:bg-opacity-100 bg-opacity-80" > Get in Touch! </button>
</form>
<div id="success" class="hidden text-md font-semibold"> You've successfully contacted me, I'll get back to you soon!</div>
<div id="error" class="hidden text-md font-semibold flex-col items-center justify-center">
<p>Oops! There's some error while sending me the contact details.</p>
<button class="bg-blue-500 py-2 px-1 rounded w-fit mx-auto text-white bg-opacity-80 hover:bg-opacity-100" onclick="javascript:window.location.reload();">Try again</button>
</div>
<div id="loading" class="hidden text-md font-semibold">Your Submission is being sent...</div>
<script src="/script.js"></script>
</body>
</html>
```

Now if you start the server, using npm run dev you should be able to see in your browser the following website.

![Contact Form website Output](https://cdn.hashnode.com/res/hashnode/image/upload/v1648317984911/0yqg4gFuo.jpg)
 
### Functionality - script.js

Add the following code to the script.js file. And let's examine what's actually happening. 


```javascript
/**
* Contact Form Functionality
*/

// Containers
const contactForm = document.getElementById('contact');
const loading = document.getElementById('loading');
const success = document.getElementById('success');
const errorEl = document.getElementById('error');

// Hide Container Function
const hideAllContainers = () => {
contactForm.style.display = 'none';
loading.style.display = 'none';
success.style.display = 'none';
errorEl.style.display = 'none';
}

// Contact Form Submit Handler
const handleContactFormSubmit = async (e) => {
e.preventDefault();
try {
contactForm.classList.add('animate-pulse');
loading.style.display = 'block';
const { name, email, message } = e.target;
const body = {
name: name.value,
email: email.value,
message: message.value,
}
const response = await fetch('/api/contact', {
method: 'POST',
headers: {
'Content-Type': 'application/json',
},
body: JSON.stringify(body),
});
if(response.status !== 200) throw response;
hideAllContainers();
contactForm.classList.remove('animate-pulse');
success.style.display = 'block';
} catch (error) {
hideAllContainers();
errorEl.style.display = 'flex';
}
}

document.addEventListener('DOMContentLoaded', () => {
hideAllContainers();
contactForm.style.display = 'flex';
contactForm.addEventListener("submit", handleContactFormSubmit);
});
```

1. All the DOM Elements are being called using the DOM API and are being stored in variables. 
2. A function `hideAllContainers()` is used to hide all the containers by accessing their style property. 
3. A function `handleContactFormSubmit()` is used to process the form submission. If all goes well, the success div is shown and if anything goes wrong the error div is displayed prompting to try to fill the form again.
4. On the document object, an event listener is added called 'DOMContentLoaded' that only fires the callback function once the HTML file is loaded.
5. Once the HTML file is loaded hide all containers, then display the form alone and finally add the submit event to the form and assign the `handleContactFormSubmit` as the callback function.

As simple as that, you've just made a functional contact form. Test it out and you'll receive the email from the person who's trying to contact you. 

Let me know how it worked out for you and if there's something wrong here, please do call it out.