# GraphQL

- Beginners course
- s

# 0.0
- Apollo client is a state management library that allows you to fetch data from API but you can handle local state in our application.
- We fetch API data and handle local data. 
- Apollo can synchronize and mix two different data. 

- Most state management 
Redux, redux-saga, react-query, recoil.

# 0.1 

- You have to know reactjs if you want to take this course. 
- You are going to leran to fetch data from react. 
- You have to know what is GraphQL API and maybe it is better than REST API. 
- The server that we build on that couse is built on this course. 
- The server that we build last time will be used for this course. 

# 0.2 Installation

- create-react-app

- Make sure that you change the current directory to movieql-client before you install apollo and reac-router-dom.

- install apolo
```
npm install @apollo/client graphql
```
- react-router-dom v6

- No error: Apollo is compatible with React 18

- import browserRouter, routes, and route.
- Create a Route to Movies and 

## 1.0 Steup

- Run your GraphQL server on the background.
- Apolla client will communicate with the GraphQL server.
- Work on the nc-apollo-movieql-client
- Apollo Client has hooks. You will use hooks 99.99% of the time. 
- Import client in App.js. Then, 
```js
import client from "./client";
```
- Then, below will be executed.
```js
client.query({
    query: gql`
    {
        allMovies {
            title
        }
    }
    `
}).then(data=> console.log(data))
```
- Notice from the devtool and see the console, we see that data has been outputed.

*What did we do?*
1. created a Apollo Client
2. we wrote the query
3. send the query to localhost 4000
4. Then, console.log the data.

## 1.1 ApolloProvider. 

- Index.js - import ApoloProvider.
- We are going to surround our application with ApoloProvider. It needs a client that we just created. 
- If you want to access the client from the movie screen, you can use the hook useApolloClient(). This will be the same client that you use here. 

*How do you send data across the route or the page?*

- State the ApolloClient in the index.js. Then, send the client data.

index.js
```js
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';
import { ApolloProvider } from "@apollo/client";
import client from "./client";

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <ApolloProvider client={client}>
      <App />
    </ApolloProvider>
  </React.StrictMode>
);

```
- Once sent to app.js, it will send it to the router. 
- You can receive client data from the page as below

```js
import { gql, useApolloClient } from "@apollo/client";
import { useEffect, useState } from "react";

export default function Movies() {
  const [movies, setMovies] = useState([]);
  const client = useApolloClient();
  useEffect(() => {
    client
      .query({
        query: gql`
          {
            allMovies {
              title
              id
            }
          }
        `,
      })
      .then((results) => setMovies(results.data.allMovies));
  }, [client]);
  return (
    <ul>
      {movies.map((movie) => (
        <li key={movie.id}>{movie.title}</li>
      ))}
    </ul>
  );
}
```

- This looks like you are fetching API but there is a better way of doing this by using useQuery hook.
- useQuery hook will do the useState and client for us. 

## 1.2 useQuery

- We are going to write graphQL query. 

*How does it work?*

Without getting the ApolloClientData, you can create a const variable called ALL_MOVIES = gql``;

src/routes/Movies.js
```js
// inside, you can create a GraphQL query.
const ALL_MOVIES = gql`
    {
        allMovies {
            title
            id
        }
    }
`;
```

*How do you use useQuery hook?*
- useQuery comes from ApolloClient.
- It will give us a lot of stuff. We get data, client, loading and error. when we call the useQuery hook. 
- declarative code: you only write code to get what you want. We just express what we want, and useQuery hook will give it to us. 
- imperative code: write every step of the way. step by step.

## useQuery Variable

20230310
- Connect from Movies to movie screen.
- Send variable to movie. 

- When the react component provide 
- GraphQL and apollo will provide apollo .

- create a graphql script save it in the variable. 
- Use useQuery to fetch the data.

*Power of Apollo Cache?*

- When you click the movie title to get into the detailed page, it never loaded. 
- Apollo will save the result of the query to the memory of the browser or cache. When you go to a different screen and come back, you will see the data right away.

## Apollo Dev Tools

- Go to Apollo Client Devtools or search in Google. 
- This is a tool that you can inspect cache in a very cool way. 
- You can add to the chrome. 
- After it is added, you can refresh the page and inspect.
- At the end, you can see apollo. 
- You can view More Tools and you will see other things. 
- You are going to see the Queries that you have run before. 
- It allows you to explore the API. 
- It is the same explorer that you have. 
- If you ever work in application, you can open the apollo dev tool and still find information about the API. types and all the queries and mutation and what they need. 
- All the information about the cache or the query is there. 

- Apollo will normalize the data for you. 
- In the movie.js, you can view the small,page,image. 
- In the movie, you are seeing that. 
- Apollo knows that field IDs are there. 
- Cache tab exist in there. when you search by the ID number. 
- Apollo cache knows about the movie. 
- If you request more data with the same ID, it will find the same ID and put the new data inside the cache. Which is super smart and cool.l 
- 