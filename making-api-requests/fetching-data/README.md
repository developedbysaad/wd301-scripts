# Text
Hello and welcome to this new lesson where you will learn how to fetch data from an API edpoint in React.JS.

# Script
Data fetching is an important aspect of building web applications, and React JS provides several ways to fetch data from external sources. In this lesson, we'll explore the most common methods of data fetching in React JS.

First, let's start with the basics. In React JS, we typically fetch data using a method called `fetch`. This method is built into modern web browsers and allows us to make HTTP requests to external APIs.

Here's an example of using the fetch method to retrieve data from an external API:

```javascript
fetch('https://api.example.com/data')
  .then(response => response.json())
  .then(data => console.log(data));
```
In this example, we're fetching data from the URL "https://api.example.com/data". The fetch method returns a *promise*, which we can use to handle the response from the API. 

As we know, a **Promise** in JavaScript is an object that represents a value that might not be available yet, but will be resolved at some point in the future.

A Promise has three states:
1. Pending: The initial state when a Promise is created.
2. Fulfilled: The state when a Promise is resolved with a value.
3. Rejected: The state when a Promise is rejected with an error.

Then we're using the `json()` method to parse the response as JSON data, and then logging the data to the console.

Now, let's see how we can use this `fetch` method in a React JS component. 
For that, let's open our React application in VS code and in the `src` directory we will create a new file called `ReactPlayground.tsx`. 
> Action: Open VS code and create ReactPlayground.tsx in src folder.

Here's we will write a simple component that fetches data from an external API, and then we will render that data. Here, we will use an open API from JSONPlaceholder, to get list of all posts (https://jsonplaceholder.typicode.com/posts).

```jsx
import React, { useState, useEffect } from 'react';

const ReactPlayground = () => {
  const [data, setData] = useState([]);

  useEffect(() => {
    fetch('https://jsonplaceholder.typicode.com/posts')
      .then(response => response.json())
      .then(data => setData(data))
      .catch(error => console.error(error));
  }, []);
  console.log(data);

  return (
    <div>
      <h1 className="text-4xl">Posts</h1>
    </div>
  );
}

export default ReactPlayground;
```
In this component, we're using the `useState` and `useEffect` hooks to manage the component's state and lifecycle. We're initializing the state with an empty array, and then using the `useEffect` hook to fetch data from the API and update the state when the component mounts.

Now, let's import the `ReactPlayground` component in our `App.tsx` file. 
```jsx
import React from 'react';
...
...
import ReactPlayground from './ReactPlayground';
...
...

const App = () => {
  return (
    <>
      <ReactPlayground />
      <RouterProvider router={router} />
    </>
  );
}

export default App;
```

Next, let's go back to the browser to check if it's working?

> Visit http://localhost:5173 in browser and open the network console.
So, as you can see, whenever we refresh the page, an API call is made to JSONPlaceholder, to get the list of all posts. Now let's use this data and print id, title and body attribute of each post in our `ReactPlayground` component.

So, first we will define an interface call `Post` in the `ReactPlayground` component:
```tsx
interface Post {
  id: number;
  title: string;
  body: string;
}
```
Then, we will use this interface in the `useState` hook:
```tsx
const [data, setData] = useState<Post[]>([]);
```

And finally, we will render the `data` in the component using the `map` method to create a list of posts.
```tsx
import React, { useState, useEffect } from 'react';

interface Post {
  id: number;
  title: string;
  body: string;
}

const ReactPlayground = () => {
  const [data, setData] = useState<Post[]>([]);
  useEffect(() => {
    fetch('https://jsonplaceholder.typicode.com/posts')
      .then(response => response.json())
      .then(data => setData(data))
      .catch(error => console.error(error));
  }, []);
  console.log(data)
  return (
    <div>
      <h1 className="text-4xl">Posts</h1>
      {data.map(item => (
        <div key={item.id}>
          <h2 className='text-2xl'>{item.title}</h2>
          <p>{item.body}</p>
        </div>
      ))}
    </div>
  );
}
export default ReactPlayground;
```
Finally, let's go back to the browser and check if the list of posts are coming or not.

Yes!

As you see, we're making a request to the **JSONPlaceholder API** to fetch some sample data, and then we are showing that data in our component.

So, that's it for this lesson. We've covered how to fetch data from an external API in a React component using the fetch API. I hope you found this helpful! 
