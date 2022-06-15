# alx-files_manager

# 0x04. Files manager

a simple platform to upload and view files:

* User authentication via a token
* List all files
* Upload a new file
* Change permission of a file
* View a file
* Generate thumbnails for images

## Resources
Read or watch:

* [Node JS getting started](https://nodejs.org/en/docs/guides/getting-started-guide/)
* [Process API doc](https://node.readthedocs.io/en/latest/api/process/)
* [Express getting started](https://expressjs.com/en/starter/installing.html)
* [Mocha documentation](https://mochajs.org/)
* [Nodemon documentation](https://github.com/remy/nodemon#nodemon)
* [MongoDB](https://github.com/mongodb/node-mongodb-native)
* [Bull](https://github.com/OptimalBits/bull)
* [Image thumbnail](https://www.npmjs.com/package/image-thumbnail)
* [Mime-Types](https://www.npmjs.com/package/mime-types)
* [Redis](https://github.com/redis/node-redis)

## Provided files
[package.json](./package.json)

[.eslintrc.js](./.eslintrc.js)

[babel.config.js](./babel.config.js)

##### and…
Don’t forget to run `$ npm install` when you have the `package.json`

# Tasks

## [0. Redis utils](./utils/redis.js)
Inside the folder `utils`, create a file `redis.js` that contains the class `RedisClient`.

`RedisClient` should have:
* the constructor that creates a client to Redis:
  `any error of the redis client must be displayed in the console (you should use on('error') of the redis client)`
* a function `isAlive` that returns `true` when the connection to Redis is a success otherwise, `false`
* an asynchronous function `get` that takes a string key as argument and returns the Redis value stored for this key
* an asynchronous function `set` that takes a string key, a value and a duration in second as arguments to store it in Redis (with an expiration set by the duration argument)
* an asynchronous function `del` that takes a string key as argument and remove the value in Redis for this key

After the class definition, create and export an instance of `RedisClient` called `redisClient`
```
bob@dylan:~$ npm run dev main-0.js
true
null
12
null
```

## [1. MongoDB utils](./utils/db.js)
Inside the folder `utils`, create a file `db.js` that contains the class `DBClient`.

`DBClient` should have:
* the constructor that creates a client to MongoDB:
```
  - host: from the environment variable `DB_HOST` or default: `localhost`
  - port: from the environment variable `DB_PORT` or default: `27017`
  - database: from the environment variable `DB_DATABASE` or default: `files_manager`
```
* a function `isAlive` that returns `true` when the connection to MongoDB is a success otherwise, `false`
* an asynchronous function `nbUsers` that returns the number of documents in the collection `users`
* an asynchronous function `nbFiles` that returns the number of documents in the collection `files`

After the class definition, create and export an instance of `DBClient` called `dbClient`
```
bob@dylan:~$ npm run dev main.js
false
true
4
30
```

## [2. First API](./server.js), [index.js](./routes/index.js), [AppController.js](./controllers/AppController.js)
Inside `server.js`, create the Express server:
* it should listen on the port set by the environment variable `PORT` or by default 5000
* it should load all routes from the file `routes/index.js`

Inside the folder `routes`, create a file `index.js` that contains all endpoints of our API:
* `GET /status` => `AppController.getStatus`
* `GET /stats` => `AppController.getStats`

Inside the folder `controllers`, create a file `AppController.js` that contains the definition of the 2 endpoints:
* `GET /status` should return if Redis is alive and if the DB is alive too by using the 2 utils created previously: `{ "redis": true, "db": true }` with a status code 200
* `GET /stats` should return the number of users and files in DB: `{ "users": 12, "files": 1231 }` with a status code 200
```
  - `users` collection must be used for counting all users
  - `files` collection must be used for counting all files
```
Terminal 1:
```
bob@dylan:~$ npm run start-server
Server running on port 5000
...
```
Terminal 2:
```
bob@dylan:~$ curl 0.0.0.0:5000/status ; echo ""
{"redis":true,"db":true}
bob@dylan:~$ curl 0.0.0.0:5000/stats ; echo ""
{"users":4,"files":30}
```
