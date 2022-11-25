It's probably no surprise to you that React Router is the most popular 3rd party library in the React ecosystem. In fact, during the last 6 months, React Router has been included in 44% of all React projects. This statistic alone is enough to declare React Router as essential knowledge for any serious React developer.

The problem, honestly, lies in the fact that no one wants to spend their weekend learning about a router – regardless of how popular it is. To make that pill a little easier to swallow, this post is a culmination of everything you'll need to know in order to be effective with React Router.

In this React Router tutorial, we'll start off with a high-level look at what React Router is. From there we'll dive into the fundamentals of the API. Finally, we'll finish off looking at a few lot of different use cases you may face in the real-world.

What is React Router?
First created in 2014, React Router is a declarative, component based, client and server-side routing library for React. Just as React gives you a declarative and composable API for adding to and updating application state, React Router gives you a declarative and composable API for adding to and updating the user's navigation history.

If you're new to React, it may come as a surprise to know that a router isn't baked into the library itself, but that's foundational to React's ethos. React focuses on giving you UI primitives for building your application, and nothing more.

Poetically, React Router follows a similar ethos, except instead of UI primitives, they give you routing primitives. To align with React, naturally, these "routing primitives" are really just a collection of React components and Hooks.

Let's dive into the most important ones before we look at specific use cases.

BrowserRouter
Naturally, in order to do its thing, React Router needs to be both aware and in control of your app's location. The way it does this is with its BrowserRouter component.

Under the hood, BrowserRouter uses both the history library as well as React Context. The history library helps React Router keep track of the browsing history of the application using the browser's built-in history stack, and React Context helps make history available wherever React Router needs it.

There's not much to BrowserRouter, you just need to make sure that if you're using React Router on the web, you wrap your app inside of the BrowserRouter component.

import ReactDOM from 'react-dom'
import * as React from 'react'
import { BrowserRouter } from 'react-router-dom'
import App from './App`

ReactDOM.render(
  <BrowserRouter>
    <App />
  </BrowserRouter>
, document.getElementById('app))
The other <Routers />
If you're using React Router in an environment that isn't the browser, check out MemoryRouter and StaticRouter.

MemoryRouter keeps track of the history of the application in memory, rather than in the URL. Use this instead of BrowserRouter if you're developing a React Native application.

StaticRouter, as the name implies, is useful in environments where the app's location never actually changes, like when rendering a single route to static HTML on a server.

Now that you know how to enable React Router via the BrowserRouter component, let's look at how you can actually tell React Router to create a new route.

Route
Put simply, Route allows you to map your app's location to different React components. For example, say we wanted to render a Dashboard component whenever a user navigated to the /dashboard path. To do so, we'd render a Route that looked like this.

<Route path="/dashboard" element={<Dashboard />} />
The mental model I use for Route is that it always has to render something – either its element prop if the path matches the app's current location or null, if it doesn't.

You can render as many Routes as you'd like.

<Route path="/" element={<Home />} />
<Route path="/about" element={<About />} />
<Route path="/settings" element={<Settings />} />
You can even render nested routes, which we'll talk about later on in this post.

With our Route elements in this configuration, it's possible for multiple routes to match on a single URL. You might want to do that sometimes, but most often you want React Router to only render the route that matches best. Fortunately, we can easily do that with Routes.

Routes
You can think of Routes as the metaphorical conductor of your routes. Whenever you have one or more Routes, you'll most likely want to wrap them in a Routes.

import { Routes, Route } from "react-router-dom";

function App() {
  return (
    <Routes>
      <Route path="/" element={<Home />} />
      <Route path="/about" element={<About />} />
      <Route path="/settings" element={<Settings />} />
      <Route path="*" element={<NotFound />} />
    </Routes>
  );
}
The reason for this is because it's Routes job is to understand all of its children Route elements, and intelligently choose which ones are the best to render.

Though it's not shown in the simple example above, once we start adding more complex Routes to our application, Routes will start to do more work like enabling intelligent rendering and relative paths. We'll see these scenarios in a bit.

Next up, linking between pages.

Link
Now that you know how to map the app's location to certain React components using Routes and Route, the next step is being able to navigate between them. This is the purpose of the Link component.

To tell Link what path to take the user to when clicked, you pass it a to prop.

<nav>
  <Link to="/">Home</Link>
  <Link to="/about">About</Link>
  <Link to="/settings">Settings</Link>
</nav>
If you need more control over Link, you can also pass to as an object. Doing so allows you to add a query string via the search property or pass along any data to the new route via state.

<nav>
  <Link to="/">Home</Link>
  <Link to="/about">About</Link>
  <Link
    to={{
      pathname: "/settings",
      search: "?sort=date",
      state: { fromHome: true },
    }}
  >
    Settings
  </Link>
</nav>
We'll cover state, Query Strings, and how React Router supports relative paths in more depth later on in this post.

At this point we've covered both the history and the absolute fundamentals of React Router, but one thing should already be clear - by embracing composition, React Router is truly a router for React. I believe React will make you a better JavaScript developer and React Router will make you a better React developer.

Now, instead of just walking you through the rest of the API, we'll take a more practical approach by breaking down all of the common use cases you'll need when using React Router.

URL Parameters
Like function parameters allow you to declare placeholders when you define a function, URL Parameters allow you to declare placeholders for portions of a URL.

Take Wikipedia for example. When you visit a topic on Wikipedia, you'll notice that the URL pattern is always the same, wikipedia.com/wiki/{topicId}.

Instead of defining a route for every topic on the site, they can declare one route with a placeholder for the topic's id. The way you tell React Router that a certain portion of the URL is a placeholder (or URL Parameter), is by using a : in the Route's path prop.

<Route path="/wiki/:topicId" element={<Article />} />
Now whenever anyone visits a URL that matches the /wiki/:topicId pattern (/wiki/javascript, /wiki/Brendan_Eich, /wiki/anything) , the Article component is rendered.

Now the question becomes, how do you access the dynamic portion of the URL – in this case, topicId – in the component that's rendered?

As of v5.1, React Router comes with a useParams Hook that returns an object with a mapping between the URL parameter(s) and its value.

import * as React from 'react'
import { useParams } from 'react-router-dom'
import { getArticle } from '../utils'

function Article () {
  const [article, setArticle] = React.useState(null)
  const { topicId } = useParams()

  React.useEffect(() => {
    getArticle(topicId)
      .then(setUser)
  }, [topicId])

  return (
    ...
  )
}
Want more?
For a much more comprehensive explanation, visit The Complete Guide to URL parameters with React Router.

Nested Routes
Nested Routes allow the parent Route to act as a wrapper and control the rendering of a child Route.

route style:
nested
A real-life example of this UI could look similar to Twitter's /messages route. When you go to /messages, you see all of your previous conversations on the left side of the screen. Then, when you go to /messages/:id, you still see all your messages, but you also see your chat history for :id.

Let's look at how we could implement this sort of nested routes pattern with React Router. We'll start off with some basic Routes.

// App.js
function App() {
  return (
    <Routes>
      <Route path="/" element={<Home />} />
      <Route path="/messages" element={<Messages />} />
      <Route path="/settings" element={<Settings />} />
    </Routes>
  );
}
Now, if we want Messages to be in control of rendering a child Routes, what's stopping us from just rendering another Routes component inside Messages? Something like this:

function Messages() {
  return (
    <Container>
      <Conversations />

      <Routes>
        <Route path=":id" element={<Chat />} />
      </Routes>
    </Container>
  );
}
Now when the user navigates to /messages, React Router renders the Messages component. From there, Messages shows all our conversations via the Conversations component and then renders another Routes with a Route that maps /messages/:id to the Chat component.

Relative Routes
Notice that we don't have to include the full /messages/:id path in the nested Route. This is because Routes is intelligent and by leaving off the leading /, it assumes we want this path to be relative to the parent's location, /messages.

Looks good, but there's one subtle issue. Can you spot it?

Messages only gets rendered when the user is at /messages. When they visit a URL that matches the /messages/:id pattern, Messages no longer matches and therefore, our nested Routes never gets rendered.

To fix this, naturally, we need a way to tell React Router that we want to render Messages both when the user is at /messages or any other location that matches the /messages/* pattern.

Wait. What if we just update our path to be /messages/*?

// App.js
function App() {
  return (
    <Routes>
      <Route path="/" element={<Home />} />
      <Route path="/messages/*" element={<Messages />} />
      <Route path="/settings" element={<Settings />} />
    </Routes>
  );
}
Much to our delight, that'll work. By appending a /* to the end of our /messages path, we're essentially telling React Router that Messages has a nested Routes component and our parent path should match for /messages as well as any other location that matches the /messages/* pattern. Exactly what we wanted.

At this point, we've looked at how you can create nested routes by appending /* to our Route's path and rendering, literally, a nested Routes component. This works when you want your child Route in control of rendering the nested Routes, but what if we wanted our App component to contain all the information it needed to create our nested routes rather than having to do it inside of Messages?

Because this is a common preference, React Router supports this way of creating nested routes as well. Here's what it looks like.

function App() {
  return (
    <Routes>
      <Route path="/" element={<Home />} />
      <Route path="/messages" element={<Messages />}>
        <Route path=":id" element={<Chats />} />
      </Route>
      <Route path="/settings" element={<Settings />} />
    </Routes>
  );
}
You declaratively nest the child Route as a children of the parent Route. Like before, the child Route is now relative to the parent, so you don't need to include the parent (/messages) path.

Now, the last thing you need to do is tell React Router where in the parent Route (Messages) should it render the child Route (Chats).

To do this, you use React Router's Outlet component.

import { Outlet } from "react-router-dom";

function Messages() {
  return (
    <Container>
      <Conversations />

      <Outlet />
    </Container>
  );
}
If the app's location matches the nested Route's path, this Outlet component will render the Route's element. So based on our Routes above, if we were at /messages, the Outlet component would render null, but if we were at /messages/1, it would render the <Chats /> component.

Want more?
For a much more comprehensive explanation, visit The Guide to Nested Routes with React Router.

Pass props to Router Components
In previous versions of React Router (v4), this was non-trivial since React Router was in charge of creating the React element.

However, with React Router v6, since you're in charge of creating the element, you just pass a prop to the component as you normally would.

<Route path="/dashboard" element={<Dashboard authed={true} />} />
Want more?
For a much more comprehensive explanation, visit How to Pass Props to a Component Rendered by React Router.

Programmatically Navigate
React Router offers two different ways to programmatically navigate, depending on your preference. First is the imperative navigate method and second is the declarative Navigate component.

To get access to the imperative navigate method, you'll need to use React Router's useNavigate Hook. From there, you can pass navigate the new path you'd like the user to be taken to when navigate is invoked.

import { useNavigate } from 'react-router-dom

function Register () {
  const navigate = useNavigate()

  return (
    <div>
      <h1>Register</h1>
      <Form afterSubmit={() => navigate('/dashboard')} />
    </div>
  )
}
If you'd prefer a more declarative approach, you can use React Router's Navigate component.

Navigate works just like any other React component, however, instead of rendering some UI, it navigates the user to a new location.

import { Navigate } from "react-router-dom";

function Register() {
  const [toDashboard, setToDashboard] = React.useState(false);

  if (toDashboard === true) {
    return <Navigate to="/dashboard" />;
  }

  return (
    <div>
      <h1>Register</h1>
      <Form afterSubmit={() => toDashboard(true)} />
    </div>
  );
}
It is more typing, but I'd argue that explicit state leading to a declarative API is better than implicit state handled by an imperative API.

Want more?
For a much more comprehensive explanation, visit How to Programmatically Navigate with React Router.

Query Strings
You've almost certainly run into query strings before. They're the ? and & you see appended onto URLs. They're a fundamental aspect of how the Web works as they allow you to pass state via the URL.

Query String Example
twitter.com/search?q=ui.dev&src=typed_query&f=live
Above is an example of a query string you'd see if you searched for ui.dev on Twitter.

As of v6, React Router relies heavily on the URLSearchParams API to deal with query strings. URLSearchParams is built into all browsers (except for IE) and gives you utility methods for dealing with query strings. To do this, React Router comes with a custom useSearchParams Hook which is a small wrapper over URLSearchParams.

useSearchParams returns an array with the first element being an instance of URLSearchParams and the second element being a way to update the query string.

Using the Twitter URL we saw above, here's how we would get the values from our query string using useSearchParams.

import { useSearchParams } from 'react-router-dom'

const Results = () => {
  const [searchParams, setSearchParams] = useSearchParams();

  const q = searchParams.get('q')
  const src = searchParams.get('src')
  const f = searchParams.get('f')

  return (
    ...
  )
}
Want more?
For a much more comprehensive explanation, visit A Guide to Query Strings with React Router.

Catch all (404) Pages
All you have to do is render a Route with a path of *, and React Router will make sure to only render the element if none of the other Routes match.

<Routes>
  <Route path="*" element={<NotFound />} />
  <Route path="/" element={<Home />} />
  <Route path="/about" element={<About />} />
  <Route path="/settings" element={<Settings />} />
</Routes>
Unlike previous versions of React Router, the order of the children Routes doesn't matter since Routes is intelligent – meaning an algorithm now determines which is the best Route to render. This makes rendering a 404 component pretty simple.

Want more?
For a much more comprehensive explanation, visit How to create a 404 page with React Router.

Pass props to Link
To pass data through a Link component to a new route, use Link's state prop.

<Link to="/onboarding/profile" state={{ from: "occupation " }}>
  Next Step
</Link>
Anytime you pass data along via the state prop, that data will be available on the location's state property, which you can get access to by using the custom useLocation Hook that comes with React Router.

import { useLocation } from 'react-router-dom'

function Profile () {
  const location = useLocation()
  const { from } = location.state

  return (
    ...
  )
}
Want more?
For a much more comprehensive explanation, visit How to Pass Props Through React Router's Link Component.

Rendering a Sidebar
Rendering a sidebar with React Router isn't particularly interesting as it's just a collection of Links. However, what if we wanted that sidebar to also be aware of the app's location? Sure you could use React Router's useLocation Hook for this, but React Router comes with a better tools for mapping the app's location to certain components, namely Routes and Route.

The key to rendering a location aware sidebar is understanding that with React Router, you can render as many Routes as you'd like. You're probably used to rendering Routes at the top level of your application, but there's nothing stopping you from rendering another Routes somewhere else in your app, like in the sidebar.

export default function App() {
  return (
    <div className="wrapper">
      <div className="sidebar">
        <ul className="nav">
          <li>
            <Link to="/">Home</Link>
          </li>
          <li>
            <Link to="/p">Profile</Link>
          </li>
          <li>
            <Link to="/s">Settings</Link>
          </li>
        </ul>

        <Routes>
          <Route path="/" element={<HomeDesc />} />
          <Route path="/p" element={<ProfileDesc />} />
          <Route path="/s" element={<SettingsDesc />} />
        </Routes>
      </div>

      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/p" element={<Profile />} />
        <Route path="/s" element={<Settings />} />
      </Routes>
    </div>
  );
}
Want more?
For a much more comprehensive explanation, visit How To Create a Location Aware Sidebar with React Router.

Customizing Link
One thing I love about React Router is how composable it is. This concept really shines when you need to build your own custom Link component. Because React Router has a component first API, you can compose Link just like you'd compose any other React component.

Let's say we wanted to create a custom Link component that "glowed" and added the 👉 emoji to whatever Link was active. To do that, all we have to do it compose Link and then useLocation to get the app's current location.

import { useLocation } from 'react-router-dom'

function GlowLink ({ children, to }) {
  const location = useLocation()
  const match = location.pathname === to

  return (
    <span className={match ? 'glow' : ''}>
      {match ? '👉 ' : ''}
      <Link to={to}>
        {children}
      </Link>
    </span>
  )
}

...


<nav>
  <GlowLink to='/'>Home</GlowLink>
  <GlowLink to='/about'>About</GlowLink>
  <GlowLink to='/features'>Features</GlowLink>
</nav>
Want more?
For a much more comprehensive explanation, visit How to Create a Custom Link Component with React Router.

Animated Transitions
Unfortunately, if you're using React Router v6, there's not currently a great story for adding animated transitions to your app. This is because React Router got rid of the Switch component, which is a fundamental part in how you accomplished it with previous versions.

Once this issue is resolved, we'll update this post.

If you're using another version of React Router, check out one of the following posts.

Animated Transitions with React Router v4
Animated Transitions with React Router v5
Want more?
For a much more comprehensive explanation, visit React Router Animated Transitions.

Code Splitting
If there's one stereotype of JavaScript developers that holds true more often than it should, it's the lack of care for large bundle sizes. The problem is historically it's been too easy to bloat your JavaScript bundle and too hard to do anything about it. This is where Code Splitting can help.

The idea is simple, don't download code until the user needs it. Your users shouldn't have to download your entire app when all they need is a piece of it. If a user is creating a new post, it doesn't make sense to have them download all the code for the /registration route. If a user is registering, they don't need the huge rich text editor your app needs on the /settings route. It's wasteful and some would argue disrespectful to those users who don't have the privilege of unlimited bandwidth. Code Splitting has not only gained much more popularity in recent years, but it's also become exponentially easier to pull off.

Here's how it works. Instead of treating import as a keyword as you typically would, you use it like a function that returns a Promise. This Promise will resolve with the module once the module is completely loaded.

if (editingPost === true) {
  import('./editpost')
    .then((module) => module.showEditor())
    .catch((e) => )
}
Now there's one more piece to the code splitting puzzle we need to look at and that's React.lazy.

React.lazy takes in a single argument, a function that invokes a dynamic import, and returns a regular React Component.

const LazyHomeComponent = React.lazy(
  () => import('./Home')
)

...

<LazyHomeComponent />
What's special about LazyHomeComponent is React won't load it until it's needed, when it's rendered. That means, if we combine React.lazy with React Router, we can hold off on loading any component until a user visits a certain path.

import * as React from "react";
import { BrowserRouter as Router, Routes, Route, Link } from "react-router-dom";

import Loading from "./Loading";

const Home = React.lazy(() => import("./Home"));
const Topics = React.lazy(() => import("./Topics"));
const Settings = React.lazy(() => import("./Settings"));

export default function App() {
  return (
    <Router>
      <div>
        <ul>
          <li>
            <Link to="/">Home</Link>
          </li>
          <li>
            <Link to="/topics">Topics</Link>
          </li>
          <li>
            <Link to="/settings">Settings</Link>
          </li>
        </ul>

        <hr />

        <React.Suspense fallback={<Loading />}>
          <Routes>
            <Route path="/" element={<Home />} />
            <Route path="/topics" element={<Topics />} />
            <Route path="/settings" element={<Settings />} />
          </Routes>
        </React.Suspense>
      </div>
    </Router>
  );
}
Notice we do need to wrap out lazy Routes inside of React.Suspense. What's nice about React.Suspense is that Suspense can take in multiple, lazily loaded components while still only rendering one fallback element.

Now instead of loading our entire app up front, React will only load our Home, Topics, and Settings components when they're needed.

Want more?
For a much more comprehensive explanation, visit Code Splitting with React, React.lazy, and React Router.

Protected Routes
Often times when building a web app, you'll need to protect certain routes in your application from users who don't have the proper authentication.

Though React Router doesn't provide any functionality for this out of the box, because it was built with composability in mind, adding it is fairly straight forward.

Let me propose what the final API might look like, before we dive into the implementation. What if, for every route we want to be private, instead of giving our Routes element prop the component we want it to render directly, we wrap it inside of a new component we'll call RequireAuth.

<Routes>
  <Route path="/" element={<Home />} />
  <Route path="/pricing" element={<Pricing />} />
  <Route
    path="/dashboard"
    element={
      <RequireAuth>
        <Dashboard />
      </RequireAuth>
    }
  />
  <Route
    path="/settings"
    element={
      <RequireAuth>
        <Settings />
      </RequireAuth>
    }
  />
  <Route path="/login" element={<Login />} />
</Routes>
At this point, we know two main things about RequireAuth. First, it's only api is a children element. Second, if the user is authenticated, it should render that children element, if not, it should redirect the user to a page where they can authenticate (in our case, /login).

Assuming you can get the authentication status of your user from a custom useAuth hook, RequireAuth becomes pretty simple.

function RequireAuth({ children }) {
  const { authed } = useAuth();
  const location = useLocation();

  return authed === true ? (
    children
  ) : (
    <Navigate to="/login" replace state={{ path: location.pathname }} />
  );
}
Notice because we get the initial location the user is trying to visit via the useLocation hook, and pass that as a state prop when we redirect them to /login, after they authenticate, we can redirect them back to this original path.

// In the Login component
const handleLogin = () => {
  login().then(() => {
    navigate(state?.path || "/dashboard");
  });
};
Want more?
For a much more comprehensive explanation, visit Protected Routes and Authentication with React Router.

Preventing Transitions
As of today, React Router v6 doesn't ship with support for preventing transitions. Once this issue is resolved, we'll update this post with the recommended way to prevent transitions in your app.

Want more?
If you absolutely need to prevent transitions in your app, check out How to Prevent Transitions with React Router for a "hacky" approach that works.

Route Config
React Router v6 comes with a useRoutes Hook that makes collocating your routes into a central route config not only possible, but also simple with a first class API.

Say we had the following paths in our application.

/
/invoices
  :id
  pending
  complete
/users
  :id
  settings
Typically if you wanted to map those paths to different React components, you'd render something like this.

return (
  <Routes>
    <Route path="/" element={<Home />} />
    <Route path="/invoices" element={<Invoices />}>
      <Route path=":id" element={<Invoice />} />
      <Route path="pending" element={<Pending />} />
      <Route path="complete" element={<Complete />} />
    </Route>
    <Route path="/users/*" element={<Users />} />
  </Routes>
);
Now with useRoutes, instead of declaring your routes using React elements (JSX), you can do it using JavaScript objects.

useRoutes takes in an array of JavaScript objects which represent the routes in your application. Similar to the React element API with <Route>, each route has a path, element, and an optional children property.

import { useRoutes } from "react-router-dom";

const routes = useRoutes([
  { path: "/", element: <Home /> },
  {
    path: "/invoices",
    element: <Invoices />,
    children: [
      { path: ":id", element: <Invoice /> },
      { path: "/pending", element: <Pending /> },
      { path: "/complete", element: <Complete /> },
    ],
  },
  {
    path: "/users",
    element: <Users />,
    children: [
      { path: ":id", element: <Profile /> },
      { path: "/settings", element: <Settings /> },
    ],
  },
]);

export default function App() {
  return (
    <div>
      <Navbar />
      {routes}
    </div>
  );
}
What makes useRoutes even more interesting is how React Router uses it internally. In fact, when you use the React element API to create your Routes, it's really just a wrapper around useRoutes.

Want more?
For a much more comprehensive explanation, visit Creating a Central Route Config with React Router.

Server Rendering
If server rendering is a new concept to you, it's important to grasp the big picture of how all the pieces of server rendering fit together before diving into the details.

1. A user types your URL into their web browser and hits enter
2. Your server sees there is a GET request
3. The server renders your React app to an HTML string, wraps it inside of a standard HTML doc (DOCTYPE and all), and sends the whole thing back as a responseSSR response
4. The browser sees it got an HTML document back from the server and its rendering engine goes to work rendering the page
5. Once done, the page is viewable and the browser starts downloading any <script>s located in the documentSSR waterfall
6. Once the scripts are downloaded, React takes over and the page becomes interactive
Notice that with server rendering, the response the browser gets from the server is raw HTML that is immediately ready to be rendered. This is the opposite of what happens with regular client-side rendering which just spits back a blank HTML document with a JavaScript bundle.

By sending back a finished HTML document, the browser is able to show the user some UI immediately without having to wait for the JavaScript the finish downloading.

Now that you get the big picture, you're probably ready to add server rendering to your React app. Unfortunately, that process is way too long to include here. Instead, check out the full post below – all 14 minutes worth.

Want more?
For a much more comprehensive explanation, visit Server Rendering with React and React Router.

Recursive Routes
It may seem impractical, but having the ability to render recursive routes will serve as both a solid exercise to solidify your understanding of React Router as well as give you the ability to solve potentially tricky UI problems down the road. When would you ever want to render recursive routes? Well, like porn, you'll know it when you see it.

The main idea is that since React Router is just components, theoretically, you can create recursive, and therefore infinite, routes.

As there's no way to really summarize this topic, you'll have to check out the full post below for more details.

Want more?
