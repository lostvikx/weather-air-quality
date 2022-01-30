# Weather & Air Quality

This project took quite a while to build, along the way I discovered various caching strategies which are implemented on front-end, web-browsers, and back-end, proxy servers.

This project was made possible by a number of diffrent JavaScript libraries and public APIs.
* Leaflet.js
* Express.js
* [Openweathermap](https://openweathermap.org/)
* [OpenAQ](https://openaq.org/#/)

The website is a simple way to get weather and air quality data from a number of different locations around the globle.

I implemented a simple and intuitive UI, on load users are asked to allow their device location. After they have allowed the site to access their location data, the site sends a GET request to the server, which is acting as a proxy. The actual data comes from the two aforementioned websites. 

This makes sense as CORS is a massive security threat, we shouldn't just send HTTP requests to any other domains except our own.

The request has 