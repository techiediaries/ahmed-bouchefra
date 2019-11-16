---
layout: post
title:  "React/GraphQL Tutorial: Build a First Example App in 4 Easy Steps"
date:   2019-11-16
---

In this tutorial, we'll learn how to build a React application in four easy steps. we'll learn how to:

- Set up a development environment for React development,
- Initialize a React project,
- Use React Hooks,
- Consume a GraphQL API,
- Building the final production bundles so you can deploy them to the cloud


We'll be consuming a third-party GraphQL API available from this [URL](https://graphql-pokemon.now.sh/)

We'll build our application in four easy and clear steps as follows:

- Step 1 - Setting up a development environment 
- Step 2 - Creating your first React project
- Step 3 - Consuming and rendering the GraphQL API
- Step 4 - Building the React application



## Prerequisites

Before diving into the actual steps, let's first see the prerequisites that you will need to have in order to follow this tutorial:

- You will need to have Node.JS and NPM installed on your machine,
- You need to be familiar with modern JavaScript and ES6+ features. 

If you don't have Node installed, you can download the installers from the [official website](https://nodejs.org/en/download/) or you can also use [NVM](https://github.com/nvm-sh/nvm), a POSIX-compliant bash script to install and manage multiple active versions of Node in your system.

> **Note**: You may be thincking, Node is required for building server-side apps but we are here building a front-end app so why do we need it? Simply because most front-end libraries nowadays have CLI (Command-line Interfaces) tools to easily scaffold and work with projects including React. These tools are built on top of Node.js. We need to have Node and NPM to install and run the `create-react-app` tool.

Now that we have seen the prerequisites of our tutorial, let's start with the first step. 

# Step 1 - Setting up a development environment 

In this first step, we'll start by setting up a development environment for building our first React application.

Since, as stated in the prerequisites, Node and NPM should have been already installed on your machine we only need to install the `create-react-app` tool to set up our development environment.

The `create-react-app` utility is the official tool for initializing and working (serving and building etc.) with React projects.


Head over to a new command-line interface and run the `npm install` command as follows:

```bash
$ npm install -g create-react-app
```

> **Note:** Depending on your operating system and npm configuration, You may get EACCESS errors when installing the tool globally. In this case, you can simply add `sudo` before your command in Linux and macOS, or use a command prompt with the administrator rights in Windows. You can also simply fix your [npm permissions](https://docs.npmjs.com/resolving-eacces-permissions-errors-when-installing-packages-globally). 

As the time of working in this tutorial,  **create-react-app v3.1.1** was installed.

That's all for this step, we are ready for creating a new React project.

## Step 2 - Initializing a New React Project

In the second step, we'll initialize a new React project.


Head over to your command-line interface and execute the following command:

```bash
$ create-react-app react-graphql-app
```

Wait for `create-react-app` to generate the project's files and install the required dependencies from npm and then run the following commands to serve the application locally using a development server:


```bash
$ cd react-graphql-app
$ npm start
```

The development server will be available from the `http://localhost:3000` address.

This is a screenshot of our React application running inside a web browser: 

![React GraphQL Example](https://www.diigo.com/file/image/rscqpoqzocdeperodzdsodqdbs/React+App.jpg?k=44aa45f1b25ab934e8566dba04800718)

Now that we hava initialized our React application and served it using a local server that has support for live-reload, let's start coding!

## Step 3 - Consuming and rendering the GraphQL API

In this third step, we'll see how to consume our GraphQL API using the Apollo client. But what's Apollo client?

It is a GraphQL client that provides many advanced features such as intelligent caching that makes it a single source of truth for the local and remote data in your application.


These are the required libraries for setting up Apollo:

To set up Apollo and add GraphQL support in our application we'll need to install a few libraries from npm.

Open a new command-line interface and run the following commands:

```bash
$ npm install graphql --save 
$ npm install graphql-tag --save

$ npm install apollo-client --save 
$ npm install apollo-link-http --save 
$ npm install apollo-cache-inmemory --save 

$ npm install react-apollo --save 
```

We first installed `graphql` which provides the GraphQL implementation for JavaScript, next  `graphql-tag` which provides a set of useful utilities that we'll be working with in our app. Next we installed the apollo client alongside its in-memory cache and HTTP link modules, finally we installed the Apollo bindings for React. 


Now, let's use the GraphQL client in our app.

Open the `src/index.js` file in your React project and add the following imports:

```js
import { ApolloClient } from 'apollo-client';
import { InMemoryCache } from 'apollo-cache-inmemory';
import { HttpLink } from 'apollo-link-http';
```

We imported the Apollo client, in-memory cache and HTTP link modules.

Next, create in-memory cache and the HTTP link instances as follows

```js
const inmemoryCache = new InMemoryCache();
const httpLink = new HttpLink({
  uri: 'https://graphql-pokemon.now.sh/'
})
```

The HTTP link allows us to send HTTP requests to the GraphQL API.

Finally, create the Apollo client instance as follows:

```js
const apolloClient = new ApolloClient({
  inmemoryCache,
  httpLink
})
```

Now we have an instance of Apollo client linked to the in-memory cache and HTTP link instances.

What is left is to connect the Apollo client with our React component(s) using `ApolloProvider`.

Go to the `src/index.js` file and import `ApolloProvider` as follows:  

```js
import { ApolloProvider } from '@apollo/react-hooks';
```

Next, wrap the `App` component with `ApolloProvider` as follows:

```js
/* [...] */
ReactDOM.render(<ApolloProvider client={client}><App /></ApolloProvider>, document.getElementById('root'));
```

Now, how to send a GraphQL query to the API?

Apollo provides a React hook that makes it easy to call GraphQL operations from the UI components.

Go to the `src/App.js` file and import `useQuery` and `gql` as follows:

```js
import { useQuery } from '@apollo/react-hooks';
import gql from "graphql-tag";
```

The `gql` tag allows you to wrap GraphQL queries.

The `useQuery` hook exposes the error, loading and data properties from a result object.

Next, add the following example GraphQL query in the `src/App.js` file:

```js
const GRAPHQL_QUERY = gql`
{
    pokemons(first: 100) {
      id
      number
      name,
      image
    }
  }
```

This query will allow you to get the first 100 pokémons with their id, number, name, and image.

Next, send the GraphQL query with the `useQuery` hook as follows:  

```js
function App() {
  const { data, loading, error } = useQuery(GRAPHQL_QUERY);

  if (loading) return <p>Still loading..</p>;
  if (error) return <p>There is an error!</p>;
```

We simply destructure the object returned from the `useQuery()` hook to get the data, loading and error attributes.

When `loading` equals `true` i.e when data is still being received, the `App` component will render **Still loading..**.

If there is an error the component will render **There is an error!**.

Otherwise we'll have our data in the data variable which we can render as follows:

```js
  return (
    <React.Fragment>
      <div className="container">

        {data && data.pokemons &&
          data.pokemons.map((pokemon, index) => (

            <div key={index}>

              <img src={pokemon.image} />
              <div>
                <h3>{pokemon.name}</h3>
                
              </div>
            </div>

          ))}
      </div>
    </React.Fragment>
  );
```

That's it, we have set up the GraphQL client in our application and sent a query to fetch data from our API then rendered data in our React component.

## Step 4 - Building the React Application

In this final step, we'll proceed to build our React application.

Simpy to your command-line interface and run the followng command from the root of your React project:

```bash
$ npm run build
```

This will generate a production-ready bundle inside the `build` folder of your project that you can deploy to your cloud server.


## Conclusion

As a recap of our tutorial. We have learned in four easy steps how to create a React application and fetch data from a GraphQL API and display it in our component then finally build the app for production.
