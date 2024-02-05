---
layout: post
title: "Building OrbitQuest: AuburnHacks 2024"
date: 2024-02-04
---

**OrbitQuest is a web application that tracks the ISS location in real-time. The 3D scene is rendered with `three.js`, which is based on WebGL. After learning about C++ OpenGL programming a few weeks ago, I felt very comfortable using a higher-level graphics framework.**

## Overview of the Project

The main feature of the web app is the 3D visualization of the ISS location in real-time. This is done through periodic API requests to accurately represent the ISS position. There is also a "Picture of the Day" page which shows NASA's picture of the day.

![Homepage](https://private-user-images.githubusercontent.com/33563846/302117217-65e0e480-ccb9-4998-b204-4e9fec270fb4.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MDcxMjg5NTQsIm5iZiI6MTcwNzEyODY1NCwicGF0aCI6Ii8zMzU2Mzg0Ni8zMDIxMTcyMTctNjVlMGU0ODAtY2NiOS00OTk4LWIyMDQtNGU5ZmVjMjcwZmI0LnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDAyMDUlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQwMjA1VDEwMjQxNFomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTVhOGU1MmRmNTI5NmRjMWU2NWE0NTNmNmZlZDQ4NDhmYjgyYmI4YzYwMjIwZGY0ZDA2ZjgwNDk5YmFjODQzNzImWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.plgEI5E7jQ-jpUHMqNuJRtDML6aY---98RkRi5U8kfs)

![Tracker](https://d112y698adiu2z.cloudfront.net/photos/production/software_photos/002/753/940/datas/original.png)

![APOD](https://d112y698adiu2z.cloudfront.net/photos/production/software_photos/002/754/340/datas/gallery.jpg)

The website also renders very well on a mobile viewport, as seen above.


## 3D Render with `three.js`

My experience with OpenGL made `three.js` easy to pick up. After applying an Earth texture to a sphere, and using an ISS model I found from NASA's website, the scene already looks very nice. Next, I had to pull Latitude/Longitude from an API, which is done every 20 seconds. \
Converting latitude and longitude to rectangular coordinates in 3D space was actually pretty straight forward. Apparently longitude corresponds to theta and latitude corresponds to phi in spherical coordinates. 

![Math](https://math.libretexts.org/@api/deki/files/8986/imageedit_17_3241908382.png?revision=1)

With theta and phi, we just need rho, which I set to a constant about 2x the radius of the Earth. The conversion of spherical to rectangular is what allows the 2D coordinates to be visualized in 3D space. 

[3D Graphics Source](https://github.com/TheSlabby/OrbitQuest/blob/main/OrbitQuest/static/js/main.js)

## Architecture of the Web App


- **Django:** Django was used for the back-end. Django also performs ISS and NASA API requests, so our API keys aren't exposed to the client.
- **Django Templates:** The front-end was rendered with Django Templates. Material bootstrap was also used to make the UI visually appealing.
- **Three.js:** The client-side JavaScript utilized the `three.js` for 3D graphics.
- **Hosting and Domain:** The server was hosted on Digital Ocean, with the domain acquired from GoDaddy.
- **API Integration:** 3 APIs were used: NASA picture of the day, ISS coordinates, and reverse geocoding.


## Conclusion

Overall, this project further developed my understanding of web development. Most importantly, I learned a lot of about writing JavaScript. Because of how prevalent JavaScript is, I'm glad I took the time to learn client-side development with JavaScript.


---
