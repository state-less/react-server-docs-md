- A **BFF** serves the purpose of rendering your client side code on the server. It fetches data on the server and prerenders raw html which can be served to your client. You can, but don't have to use React Server as BFF. You can also use Next.js as BFF and use react-client's `renderComponent` utility to fetch data during SSR.
- A **Backend** does more than a BFF. It provides the backend business logic for your (micro)services and communicates with the BFF / Frontend through an API. React Server helps you write backend services using components, states and effects.

In a production setting you would usually have both. A _BFF_ which is deployed together with your frontend on the CDN's Edge and a **Backend** which probably runs in some kind of cluster although you could also use an EC2 micro instance to host a backend if you don't expect a lot of traffic.

The BFF can facilitate data fetching libraries like Apollo client to fetch and cache your data on the CDN's edge. This reduces TTFB and allows your web application to serve prerendered HTML which may offer additional benefits.
