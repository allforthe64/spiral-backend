<h1>Discover Spiral: The Backend</h1>
<br>
If you've already looked through the readme for this project's frontend, you'll know that Discover Spiral Learning features not only a flashy forward facing design, but also a backend which we built to store all of the great learning resources available on the site. Our backend needed to store objects made up of text and images and a have a system to perform CRUD operations on the resources. With these parameters in mind, we decided to keep it simple and use a Express API connected to a MongoDB cluster. Read below to see how we combined these tools to build the backend for the Spiral project:
<br>
<br> 
<br>
<br>
<div align='center'>
  <img width='50%' src='https://media.giphy.com/media/Vwf0G57SrWa4bqkouc/giphy.gif'/>
</div>
<br>
<h1>How It's Made:</h1>
<br>

**Tech Used:** Express.js, Node.js, Postman, MongoDB
<br>
<p>As I mentioned in the .Readme for the Spiral frontend repo, we built the Discover Spiral app using the MERN stack. Our backend API is built using Node.js/Express connected to MongoDB. Express made it easy for us to quickly setup our basic routes and controllers for each of the different data objects we would be returning: Resources, Goals, and Notes. After connecting to our MongoDB cluster, we tested CRUD operations on each route/controller pair using Postman</p>
<br>
<p>Once our routes were setup, we needed to secure them. We did this using JWTs and refresh tokens, which we passed to our React.js frontend app to be stored in context. We set the inital tokens to expire rather quickly and the refresh tokens to expire within 24 hours, preventing a user from remaining permanently logged in. However, we also added a persist option, which keeps the refresh token in memory for longer, allowing the user to login repeatedly before their tokens expire. To actually protect our routes, we wrote some middleware to check if a JWT/refresh token was sent with the request from the frontent. If there isn't a token, the request is blocked and the user is prompted to login or signup for an account.</p>
<br>
<p>We also wanted to make some routes only accessible by certain users, even if they were logged in at the time (e.g. admin route is only accessible by admin). To do this, we gave each user object a set of roles and wrote middleware to check if the user making a request was allowed to access the route. Using this we protected our admin page from anyone without the admin role assigned to them, since the admin route allowed for editing of user information and goals, etc. We also used this same method to control whether a user was allowed to edit the content of learning resources.</p>
<br>
<p>Finally, we wrote more middleware to upload image files to Cloudinary. This middleware let us do a bunch of cool stuff, like setting up profile pictures for users and adding image galleries for learning resources. Using this middleware we were able to connect to Cloudinary and upload images to an image bucket. We then got urls for those images, which we tied into user profiles or learning resources.</p>
<br>
<h1>Areas To Improve</h1>
<br>
<p>One of the routes we needed to protect was our learning resource route. Unfortunately, this route contains the function for fetching all of the available learning resources and passing them to the frontend. As I mentioned in the React app .Readme, the first request made to the resource route is automatically denied because the user isn't logged in. This occasionally causes problem with the page loading on the frontend if the request hangs. We were looking for a way to make the resources route accessible for a single request, but unfortunately we couldn't figure out how to allow a request without disabling protection for the entire route. The best way to handle this issue would be to create a public route that returns all of the learning resources and takes in a request parameter that is true or false based on if the request is being made for the first time or not. If the param is false, the route could just kick out and not execute, thereby allowing us to have our cake and eat it too: our route would return the data we need for the frontend and be protected from malicious attackers trying to steal the learning resources for free :0</p>
<br>
<h1>Lessons I Learned</h1>
<br>
<p>Hitherto working on this project, I'd never really worked with a non-relational database like MongoDB before. At least, not at this scale. My experience was mostly with SQL, so making the switch over to a non-relational database was certainly a learning experience. Figuring out how to structure our data in a way that made sense was a challenge and one I enjoyed tackling and solving. I had to pay attention to how I modified data, moved and sorted data, and returned data, all in a way that used as few calls and iterable functions as possible to keep load times on the frontend to a minimum.</p>
<br>
<p>In addition, this was my first time working with JWTs and refresh tokens, which was another giant step up from previous projects. To go from using a basic username and password system, to managing headers and tokens on every request taught me so much about how apps interact with APIs and how to properly manage and protect routes. Using axios, we were able to add request controllers and manage headers, which for me, coming from the fetch API, was a totally new experience, and one I thoroughly enjoyed. Axios made it easy to send data and tokens to our backend and Express and Mongo made it easy to manage the data.</p>
<br>
<p>All and all, I had a great time developing the Discover Spiral app. It was truly a labor of love working on a project with such significant personal value. Not only did I learn new things from working on this project, but I also published a product meant to better the world, which was a great feeling to have. As a self taught developer, the idea of working on a project that might inspire a future developer to start learning to code is one of the coolest things I've ever had to work on. If you ask me, I couldn't have asked for a more perfect project to work on and learn from. I hope you enjoyed this .Readme, and until next time, peace &#9996.</p>
