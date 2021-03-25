![GitHub Last Commit](https://img.shields.io/github/last-commit/samrogers15/Online-Offline-Budget-Tracker?style=plastic)
![GitHub Repo Size](https://img.shields.io/github/repo-size/samrogers15/Online-Offline-Budget-Tracker?style=plastic)
![GitHub Followers](https://img.shields.io/github/followers/samrogers15?style=social)
![GitHub](https://img.shields.io/github/languages/top/samrogers15/Online-Offline-Budget-Tracker?style=plastic)


# Online-Offline-Budget-Tracker
> This is a repository for an application that allows a user to track their budget regardless of whether they are connected to the internet or not. A user can add in transactions while connected to the internet, and if their connection is interrupted, they can still enter in transactions. Once the internet connection is restored, the transactions will automatically be entered into the database.
 
## Table of contents
* [General Info](#general-info)
* [User Story](#user-story)
* [Technologies](#technologies)
* [Live Link](#Live-Link)
* [Screen Shot](#Screen-Shot)
* [Code Snippets](#code-snippets)
* [Sources](#sources)
* [Contact](#contact)

## General Info
The database was initialized using MongoDB the model was set up utilizing Mongoose. The application server was initialized using an Express router and the API routes include queries to the MongoDB database to display previous transactions that have been added into the budget tracker. If the internet connection is lost, the user can still enter in transactions through the use of an indexed DB - once the connection comes back online, those transactions that were entered while offline are added to the live database. The application is deployed to Heroku and the database has been connected to MongoDB Atlas.

## User Story
AS AN avid traveller
I WANT to be able to track my withdrawals and deposits with or without a data/internet connection
SO THAT my account balance is accurate when I am traveling

## Technologies
* Javascript
* HTML/CSS
* Node
* NPM Express
* NPM MongoDB
* NPM Mongoose
* NPM Morgan
* NPM Compression
* Heroku
* MongoDB
* MongoDB Atlas
* MongoDB Compass

## Live Link
[Online-Offline Budget Tracker](UPDATE)

## Screen Shot
![Online-Offline Budget Tracker Screen Shot](UPDATE)

## Code Snippets

The below example code shows the service worker being installed in the browser:
```js
// install service-worker
self.addEventListener("install", function (evt) {
  // pre cache transaction data
  evt.waitUntil(
    caches.open(DATA_CACHE_NAME).then((cache) => cache.add("/api/transaction"))
  );

  // pre cache all static assets
  evt.waitUntil(
    caches.open(CACHE_NAME).then((cache) => cache.addAll(FILES_TO_CACHE))
  );

  // tell the browser to activate this service worker immediately once it has finished installing
  self.skipWaiting();
});
```

The below example code shows the setup of the web manifest:
```js
{
    "name": "Budget Tracker App",
    "short_name": "Tracker App",
    "icons": [
        {
            "src": "icons/icon-192x192.png",
            "sizes": "192x192",
            "type": "image/png"
        },
        {
            "src": "icons/icon-512x512.png",
            "sizes": "512x512",
            "type": "image/png"
        }
    ],
    "theme_color": "#ffffff",
    "background_color": "#ffffff",
    "start_url": "/",
    "display": "standalone"    
}
```

The below example code shows the function responsible for checking the indexed DB once the internet connection is restored to ensure any offline entries are correctly added into the database.
```js
function checkDatabase() {
  // open a transaction on your pending db
  const transaction = db.transaction(["pending"], "readwrite");
  // access your pending object store
  const store = transaction.objectStore("pending");
  // get all records from store and set to a variable
  const getAll = store.getAll();

  getAll.onsuccess = function () {
    if (getAll.result.length > 0) {
      fetch("/api/transaction/bulk", {
        method: "POST",
        body: JSON.stringify(getAll.result),
        headers: {
          Accept: "application/json, text/plain, */*",
          "Content-Type": "application/json",
        },
      })
        .then((response) => response.json())
        .then(() => {
          // if successful, open a transaction on your pending db
          const transaction = db.transaction(["pending"], "readwrite");

          // access your pending object store
          const store = transaction.objectStore("pending");

          // clear all items in your store
          store.clear();
        });
    }
  };
}
```

## Sources
Application enabled using the following sources:

* [NPM Express](https://www.npmjs.com/package/express)
* [NPM MongoDB](https://www.npmjs.com/package/mongodb)
* [NPM Mongoose](https://www.npmjs.com/package/mongoose)
* [NPM Morgan](https://www.npmjs.com/package/morgan)
* [NPM Compression](https://www.npmjs.com/package/compression)

## Contact
Created by Sam Rogers - feel free to contact me to collaborate on this project or any other project!

[LinkedIn](https://www.linkedin.com/in/samuelerogers/)

[Portfolio](https://samrogers15.github.io/Current_Portfolio/index.html)