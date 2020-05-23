# ABLB - Arts Based Learning in Business

## The code

The frontend can be found at [artscilab/ablb-app](https://github.com/artscilab/ablb-app), and the backend can be found at [artscilab/ablb-backend](https://github.com/artscilab/ablb-backend)

## Architecture

The current architecture is pretty simple: a React front-end created with [Create React App](https://create-react-app.dev/), and a [Express.js](https://expressjs.com/) server.

There are two main aspects to the front-end: the user-facing side of things - everything visible and accessible from the main site, and the admin side of things, accessed at the semi-hidden `/admin` route. 

The backend serves a pretty simple REST API - peruse the routes to find out which ones exist - or add a route that returns all existing routes as an enhancement. The frontend calls this API on most page loads and for all the resources it needs - there's no caching so theres going to be a lot of repeated calls. 

For data storage, we are using MySQL, which is already installed and in use on the server. Read more about how we connect to and use the database in the backend section.

## The frontend 

Some important libraries you'll need to know a but about are [react-router](https://reacttraining.com/react-router/web/guides/quick-start) and [formik](https://jaredpalmer.com/formik/).

React router enables easy, declarative routing, and Formik enables easy creation of all the forms that we need, such as the ones on the Admin page. 

Other than those two libraries, the front-end is set up to be pretty easily editable, with typical React components and not much specialized logic. 

One thing to note is the way we authenticate with the server. Our axios instance passes a authorization token header with each request. This token is a JWT, and you can read more about that in the authentication part of the backend section.

--- 

Contributors: Al Madireddy