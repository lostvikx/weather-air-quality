# Weather & Air Quality

This project is hosted on Heroku, here is the [link](https://weather-and-air-quality.herokuapp.com/).

This project took quite a while to build, along the way I discovered various caching strategies which are implemented on front-end, web-browsers, and back-end, proxy servers.

This project was made possible by multiple diffrent JavaScript libraries and public APIs.
* Leaflet.js
* Express.js
* [OpenWeatherMap](https://openweathermap.org/)
* [OpenAQ](https://openaq.org/#/)

The website is a simple way to get weather and air quality data from a number of different locations around the globle.

I implemented a simple and intuitive UI, on load users are asked to allow their device location. After they have allowed the site to access their location data, the site sends a GET request to the server, which is acting as a proxy. The actual data comes from the two aforementioned websites. 

This makes sense as CORS is a massive security threat, we shouldn't just send HTTP requests to any other domains except our own.

The request is sent from my server to the API endpoints. I used `Promise.all()` to make non-blocking asynchronous calls to the two API endpoints. We now only need to wait for the two endpoints to respond. I have made an extensive fail proof machenism, using a try-catch block, that catches any exceptions thrown then logs it, then finally returns an `res`.

```javascript
{ failed: true, message: "failed to fetch the data" }
```

Then I thought to cache user's `coords` data on the client side. So, I implemented this feature, but turns out that you could have done that using the Web API of `navigator`. I set the `timeToLive` as 180s.

I also stored the user request of weather and air quality data in the `LocalStorage` of the client. This saves the round trip on every reload.

The two data sources were combined into one object data structure. Then I stored this object in the `LocalStorage`.

The data structure was used to plot markers on a map canvas, div actually, provided by [OpenStreetMap](https://www.openstreetmap.org).

I not only wanted our client's current weather and air quality information, but also so predefined areas on interest. So, I stored some locations from around the world with their coords into a json file. 

The json file was read by a Node.js API which then I called the two API endpoints for relevant data for all the coords in the json file. Again I reused the same methods from earlier and used `Promise.all()` to create many non-blocking requests.

The json file contains more than 25 different places, so I had to find a way to not call these API endpoint for every page load. So, I learned about server side caching. This is incredible, as I set the `timeToLive` to this data about 1800 seconds. This is good as even other users can benefit from this.

This caching on the proxy server can be attributed to the creators of `node-fetch` which stores the data into the memory and is cleared with the server stop functioning.

Later, I added another functionality to my application. This is to click anywhere on the map and we can get the `allData` from to two resources.
