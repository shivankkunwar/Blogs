---
title: "Building a Face Detection App with PERN & Typescript: A Full Stack Case Study"
datePublished: Thu Apr 27 2023 05:57:47 GMT+0000 (Coordinated Universal Time)
cuid: clgypr3aj000s0amd58qa0q6p
slug: building-a-face-detection-app-with-pern-typescript-a-full-stack-case-study
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/o0Qqw21-0NI/upload/e1c5b17de1f56690115a4f3e502a8711.jpeg
tags: javascript, web-development, nodejs, reactjs, typescript

---

Welcome to my case study on building a face detection app with PERN stack and Typescript. As a Full Stack developer, I recently completed this project, which challenged me to utilize the power of PERN stack and Typescript in my development process. In this case study, I will provide an overview of the project, and discuss its purpose, features, and technical details. I will also share my motivation, challenges, achievements, and lessons learned from this experience. Join me as I share insights and reflections on building a Full Stack PERN project with Typescript.

## Background:

As I was Looking to learn something that I could put on my resume I stumbled upon the eye-catching and in-demand field of Full stack development the idea of creating a website on my own motivated me into indulging in this field. I decided to start a course on learning how to create an application from its front end to the back end to hosting the application. This is a Case study on the Application that I created while learning those technologies.

## Overview:

My face detection app is a web-based application that allows users to upload images and automatically detects and highlights human faces in the images. The app is built using the PERN stack (PostgreSQL, Express, React, and Node.js) along with Typescript, which has enabled me to create a robust and scalable full-stack application with improved type safety and developer productivity.

The main functionality of the app is centered around the Face.js API, an open-source facial recognition and detection library.

Key features of the app include image uploading, automatic face detection using the Face.js API, image rendering with highlighted faces, user authentication and authorization, and image management functionalities. The app is designed to be intuitive and user-friendly, with a sleek and modern interface that enhances the overall user experience.

Here's a working version of the application in the first draft and I will keep changing things in the UI and functionalities as I learn new technologies.

![](https://ik.imagekit.io/knavihs/1.gif?updatedAt=1682519527136 align="center")

## [Github link for the application](https://github.com/shivankkunwar/FaceD)

## Technical Details:

After creating Small applications with HTML, CSS, and Javascript the next step was to learn a framework or Library that will provide a better and easier experience in creating complex applications and makes them more scalable and efficient.

# FRONTEND:

# React

The reasons why I chose React were mainly because it was the most in-demand technology, large community and it provides a Component-based architecture which was fascinating to me so instead of working on the whole puzzle at the same time you can work on the smaller parts and then piece them together and virtual DOM of react instead of making changes to the object DOM, make changes to the virtual and compare to make minimal manipulations required that makes the rendering faster.

My application is divided into the following components. The hierarchy displays the architecture of the application

**Components:**

* **Navigation Bar**: A dynamic Navigation bar that displays the Application's name and routing buttons like Sign In, Register, and Sign out.
    
* **Sign-in**: A component that displays a form that takes the details of the user (email, password) this triggers an HTTP POST request to the database via a backend server and authenticates access.
    
* **Register**: similar to Sign-In displays a form that takes details like email, name, and password then on-submit an HTTP POST request to the database to store those values
    
* **Home page**
    
    * **NewFaceCode:** This is the main component that displays the face detection functionality of the application.
        
        * When an image is passed to the state it triggers another component **newPost** which fetches the details the FaceApi provides according to the image.
            
        * Then according to the details provided a box is drawn using the canvas element of html.
            
    * **Rank:** A component that displays the number of attempts of Face detection or API fetching that have been taken by the specific user and it also shows the application's ability to fetch details about the user from the database.
        
    * **Logo:** A interactive logo for the application that is created by using the react-parallax-tilt library gives a unique look to the user interface.
        

# FaceAPI.js

JavaScript API for face detection and face recognition in the browser implemented on top of the tensorflow.js core API

Face recognition, Face similarity, Face Expression Recognition, Face Landmark Detection, and Real-Time face tracking are some of the functionalities provided by the API.

[To learn More](https://justadudewhohacks.github.io/face-api.js/docs/index.html)

![](https://user-images.githubusercontent.com/31125521/47384002-41e36f80-d706-11e8-8cd9-b3102c1bee67.png align="center")

## DATABASE:

### PostgreSQL:

A powerful and popular open-source relational database management system, to store and manage data. PostgreSQL provides robust support for advanced data types, indexing, and transactions, making it suitable for complex applications with high data volume and concurrency requirements.

I chose a SQL-based database over a NoSQL database as new to database engineering I wanted to learn the Core functionality of the databases, the concepts of data modeling, normalization, and designing efficient and scalable database schemas.

The tables used in the project are Users and Login.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682512077226/650338d9-c9d9-4dc6-bee5-53ff2f540131.png align="left")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682512091938/915116a0-98be-4f9b-ac21-a4c0177f995c.png align="left")

## BACKEND:

After working on the front end of the application it was time to start my journey into learning backend technologies, there were many options but as I was already working on javascript technology I decided to learn Node js as the backend technology for my applications.

The idea is to create a connection between the front end and back end of the application here a Protocol is required and the best protocol for such requests is HTTP.

### Node js:

a JavaScript runtime built on Chrome's V8 engine, played a crucial role in my project as it enabled server-side JavaScript execution.

as we were working with an API so its event-driven, non-blocking I/O model made it perfect for managing APIs and concurrent connections.

### Express.js:

A fast and minimalist framework extends the capabilities of Node.js by providing tools for building RESTful APIs and managing routes.

handles HTTP requests and responses, and implements middleware for functionalities like Authentication, error handling, and logging.

MIddleware used in the project:

* **body-parser**: parsing the incoming request body of HTTP requests into JavaScript object. like JSON, URL-encoded data, etc.
    
* **Cors**: (cross-origin Resource Sharing) security feature that restricts or allows web pages to make requests to a different domain than the one that served the web page.
    
    below shows that Cors allow any domain to request app resources you can use Cors to specify which domains are permitted
    

The server-side functionalities I used in the project are

### Routes:

* /**signIn:** When a POST request is made to `/signin` endpoint with the user's email and password in the request body, the server uses Knex to query the `login` table in the database to retrieve the hashed password associated with the email. It then uses bcrypt to compare the provided password with the stored hashed password to verify if the credentials are valid.
    
* ```javascript
        app.post('/signin/',(req,res)=>{
            
            db.select('email','hash').from('login')
            .where('email','=',req.body.email)
            .then(data=>{
                const isvalid=bcrypt.compareSync(req.body.password,data[0].hash);
                console.log(isvalid)
                if(isvalid){
                    return  db.select('*').from('users')
                    .where('email','=',req.body.email)
                    .then(user =>{
                        res.json(user[0])
                    })
                    .catch(err => res.status(400).json('unable to get user'));
                }else{
                    res.status(400).json(`wrong credentials ${err}`)
                }
                
            })
            .catch(err => res.status(400).json(`wrong credentials ${err}`))
        })
    ```
    
* /**Register**: When a POST request is made to `/register` endpoint with the user's email, name, and password in the request body, the server uses Knex to initiate a database transaction. It generates a salt and uses bcrypt to hash the password with the generated salt.
    
    * inserts the hashed password and email into the `login` table, and then inserts the email, name, and current date as the `joined` field into the `users` table.
        
* ```javascript
        app.post('/register/',async (req,res)=>{
            console.log("register working")
            const { email , name , password }=req.body; 
            console.log(req)
            const salt = await bcrypt.genSaltSync(10);
            const hash = await bcrypt.hashSync(password,salt);
           if(!email || !name || !password){
            return res.status(400).json("incorrect form submission");
           }
                db.transaction(trx=>{
                trx.insert({
                    hash:hash,
                    email:email
                })
                .into('login')
                .returning('email')
                .then(loginEmail => {
                    return  trx('users')
                    .returning('*')
                    .insert({
                        email: loginEmail[0].email,
                        name:name,
                        joined:new Date()
                    })
                    .then(user=>{
                        res.json(user[0]);
                    })
                })
                .then(trx.commit)
                .catch(trx.rollback)
                })
                .catch(err=>{
                res.status(400).json(`unable to Join${err}`);
                })
        
        })
    ```
    
* /**profile**: a GET request route to handle profile requests from the client. The route is associated with the `/profile/:id` endpoint, where `:id` represents the user's unique identifier.
    
    * When a GET request is made to this endpoint with the user's ID, the Express route retrieves the corresponding user details from the database based on the ID and sends the user's profile data as a response to the client.
        
    * Enables users to securely access and view their profile information.
        
    * ```javascript
            app.get("/profile/:id/",(req,res)=>{
                const { id } = req.params;
                
               db.select('*').from('users').where({id}).then(user=>{
            
                if(user.length){
                    res.json(user[0]);
                }
                else{
                    res.status(400).json('not found');
                }
               }).catch(res=>{
                res.status(400).json("error getting user ")
               })
            
            })
        ```
        
* /**image**: When a PUT request is made to this endpoint with the `id` parameter in the request body, the server increments the `entries` field of the user with the matching `id` in the database by 1. The updated `entries` count is then returned as a JSON response to the client.
    
* ```javascript
        app.put('/image/',(req,res)=>{
            const { id } = req.body;
            db('users').where('id','=',id)
            .increment('entries',1)
            .returning('entries')
            .then(entries=>{
                res.json(entries[0]);
            }).catch(err=>res.status(400).json('unable to get entries'))
        } )
        app.listen(process.env.PORT||3000,()=>{
        
            console.log(`app is running perfectly on ${process.env.PORT}`)
        });
    ```
    

# TypeScript:

After creating the project in the PERN (PostgreSQL, Express, React, Node.js) stack, I decided to further enhance my project by incorporating TypeScript into my development workflow. TypeScript is a statically-typed superset of JavaScript that offers additional features, such as optional static typing, interfaces, and advanced error checking, which can greatly improve the quality and maintainability of a codebase.

* **Static Typing**: specify the data types of variables, function parameters, and return values. This helps catch type-related errors at compile-time, providing early feedback on potential issues and improving code quality.
    
* ```javascript
        // TypeScript
        function greet(name: string): string {
          return `Hello, ${name}!`;
        }
        
        console.log(greet("John")); // Output: Hello, John!
        console.log(greet(123)); // Error: Argument of type 'number' is not assignable to parameter of type 'string'.
    ```
    
    In particular, I wanted to ensure type safety for the state management of my form component, which included fields like `email`, `password`, and `count`. To achieve this, I could define a TypeScript type called `FormState` to represent the shape of the state, specifying the types for each field.
    

```javascript
import React, { useState } from 'react';

// Define a type for the state
type FormState = {
  email: string;
  password: string;
  count: number;
};

// Define a functional component with typed state
const MyForm: React.FC = () => {
  const [state, setState] = useState<FormState>({
    email: '',
    password: '',
    count: 0
  });
```

# Challenges Faced:

### Class component to Functional component:

The first challenge I faced In this project was the conversion of all the code which I wrote in the class components into functional components. This decision to migrate was driven by the advantages offered by functional components such as improved performance, easier testing, and better readability.

To tackle this challenge, I did a thorough review of the existing class component involving differences in syntax and behavior between class and function.

Hooks provided all the equivalent to the life cycle methods like `componentDidMount`, `componentDidUpdate` to `useEffect`, `useState` .

State management logic, event handling, and prop handling were appropriately updated to align with functional components.

This helped me understand the nuances of functional components and how they are used in contemporary React development. I learned about differences in syntax, state management, event handling, and prop handling between class and functional components, resulting in improved code performance and maintainability. This experience deepened my understanding of modern React development best practices.

## Overcoming Challenges with API Updates

Previously I was using a Face detection API provider named Clarifai but During the development of my project,

I encountered a challenge when the API I was using for face detection underwent an update that included changes in syntax and functionality. Despite my efforts to update my code to accommodate the changes, I faced difficulties due to the lack of comprehensive documentation and an inactive community around the API.

It was challenging to find solutions to the errors I encountered, and the updates were not easily understandable. After struggling with these issues, I made the decision to switch to a different face detection API, specifically the "face-api.js" library, which provided clearer documentation. This decision allowed me to overcome the challenges associated with the previous API and continue the development of my project smoothly.

# Summary

In conclusion, the journey of creating a face detection application using the PERN stack and incorporating TypeScript and the FACE API.js library presented both opportunities and challenges. Through this project, I gained a deeper understanding of modern web development practices, including working with React functional components, database engineering, and utilizing TypeScript for improved code quality.

# Call to Action:

* # [Linkedin](https://www.linkedin.com/in/shivank-kunwar/)
    
* # [Github](https://github.com/shivankkunwar)
    

If you're interested in discussing potential job opportunities or learning more about my skills and experience, feel free to connect with me on LinkedIn. I'm open to new opportunities and would love to connect with professionals in the industry.