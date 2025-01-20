# 0x00. Pagination
## The Domains/Concepts covered in this project: `Back-end` `JavaScript` `ES6` `Redis` `NodeJS` `ExpressJS` `Kue`

This project introduced me to implementing a queuing system using Kue, Express, Node.js, Redis, and JavaScript to manage and process tasks efficiently. I learned how to set up and configure task queues, handle job lifecycles, and leverage Redis for reliable and scalable asynchronous processing in backend applications.

## 1. package.json

```
{
    "name": "queuing_system_in_js",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "scripts": {
      "lint": "./node_modules/.bin/eslint",
      "check-lint": "lint [0-9]*.js",
      "test": "./node_modules/.bin/mocha --require @babel/register --exit",
      "dev": "nodemon --exec babel-node --presets @babel/preset-env"
    },
    "author": "",
    "license": "ISC",
    "dependencies": {
      "chai-http": "^4.3.0",
      "express": "^4.17.1",
      "kue": "^0.11.6",
      "redis": "^2.8.0"
    },
    "devDependencies": {
      "@babel/cli": "^7.8.0",
      "@babel/core": "^7.8.0",
      "@babel/node": "^7.8.0",
      "@babel/preset-env": "^7.8.2",
      "@babel/register": "^7.8.0",
      "eslint": "^6.4.0",
      "eslint-config-airbnb-base": "^14.0.0",
      "eslint-plugin-import": "^2.18.2",
      "eslint-plugin-jest": "^22.17.0",
      "nodemon": "^2.0.2",
      "chai": "^4.2.0",
      "mocha": "^6.2.2",
      "request": "^2.88.0",
      "sinon": "^7.5.0"
    }
  }
```

## 2 .babelrc

```
 
{
  "presets": [
    "@babel/preset-env"
  ]
}
```

## Tasks :page_with_curl:

**0. Install a redis instance**

Download, extract, and compile the latest stable Redis version (higher than 5.0.7 - [https://redis.io/downloads/](https://redis.io/downloads/)

```
$ wget http://download.redis.io/releases/redis-6.0.10.tar.gz
$ tar xzf redis-6.0.10.tar.gz
$ cd redis-6.0.10
$ make
```

  * Start Redis in the background with `src/redis-server`

```
$ src/redis-server &
```

  * Make sure that the server is working with a ping `src/redis-cli ping`

```
PONG
```

  * Using the Redis client again, set the value `School` for the key `ALX`

```
127.0.0.1:[Port]> set ALX School
OK
127.0.0.1:[Port]> get ALX
"School"
```

  * Kill the server with the process id of the redis-server (hint: use `ps` and `grep`)

```
$ kill [PID_OF_Redis_Server]
```

Copy the `dump.rdb` from the `redis-5.0.7` directory into the root of the Queuing project.

Requirements:

  * Running `get ALX` in the client, should return `School`

  * [dump.rdb](./dump.rdb)

**1. Node Redis Client**

Install [node_redis](https://github.com/redis/node-redis) using npm

Using Babel and ES6, write a script named `0-redis_client.js`. It should connect to the Redis server running on your machine:

  * It should log to the console the message `Redis client connected to the server` when the connection to Redis works correctly
  * It should log to the console the message `Redis client not connected to the server:` `ERROR_MESSAGE` when the connection to Redis does not work

**Requirements:**

  * To import the library, you need to use the keyword `import`

```
bob@dylan:~$ ps ax | grep redis-server
 2070 pts/1    S+     0:00 grep --color=auto redis-server
bob@dylan:~$ 
bob@dylan:~$ npm run dev 0-redis_client.js 

> queuing_system_in_js@1.0.0 dev /root
> nodemon --exec babel-node --presets @babel/preset-env "0-redis_client.js"

[nodemon] 2.0.4
[nodemon] to restart at any time, enter `rs`
[nodemon] watching path(s): *.*
[nodemon] watching extensions: js,mjs,json
[nodemon] starting `babel-node --presets @babel/preset-env 0-redis_client.js`
Redis client not connected to the server: Error: Redis connection to 127.0.0.1:6379 failed - connect ECONNREFUSED 127.0.0.1:6379
Redis client not connected to the server: Error: Redis connection to 127.0.0.1:6379 failed - connect ECONNREFUSED 127.0.0.1:6379
Redis client not connected to the server: Error: Redis connection to 127.0.0.1:6379 failed - connect ECONNREFUSED 127.0.0.1:6379
^C
bob@dylan:~$ 
bob@dylan:~$ ./src/redis-server > /dev/null 2>&1 &
[1] 2073
bob@dylan:~$ ps ax | grep redis-server
 2073 pts/0    Sl     0:00 ./src/redis-server *:6379
 2078 pts/1    S+     0:00 grep --color=auto redis-server
bob@dylan:~$
bob@dylan:~$ npm run dev 0-redis_client.js 

> queuing_system_in_js@1.0.0 dev /root
> nodemon --exec babel-node --presets @babel/preset-env "0-redis_client.js"

[nodemon] 2.0.4
[nodemon] to restart at any time, enter `rs`
[nodemon] watching path(s): *.*
[nodemon] watching extensions: js,mjs,json
[nodemon] starting `babel-node --presets @babel/preset-env 0-redis_client.js`
Redis client connected to the server
^C
bob@dylan:~$
```

  * [0-redis_client.js](./0-redis_client.js)

**2. Node Redis client and basic operations**

In a file `1-redis_op.js`, copy the code you previously wrote (`0-redis_client.js`).

Add two functions:

  * `setNewSchool`:
    * It accepts two arguments `schoolName`, and `value`.
    * It should set in Redis the value for the key `schoolName`
    * It should display a confirmation message using `redis.print`
  * `displaySchoolValue`:
    * It accepts one argument `schoolName`.
    * It should log to the console the value for the key passed as argument

At the end of the file, call:

  * `displaySchoolValue('ALX');`
  * `setNewSchool('ALXSanFrancisco', '100');`
  * `displaySchoolValue('ALXSanFrancisco');`

**Requirements:**

  * Use callbacks for any of the operation, we will look at async operations later

```
bob@dylan:~$ npm run dev 1-redis_op.js 

> queuing_system_in_js@1.0.0 dev /root
> nodemon --exec babel-node --presets @babel/preset-env "1-redis_op.js"

[nodemon] 2.0.4
[nodemon] to restart at any time, enter `rs`
[nodemon] watching path(s): *.*
[nodemon] watching extensions: js,mjs,json
[nodemon] starting `babel-node --presets @babel/preset-env 1-redis_op.js`
Redis client connected to the server
School
Reply: OK
100
^C

bob@dylan:~$
```

  * [1-redis_op.js](./1-redis_op.js)

**3. Node Redis client and async operations**

In a file `2-redis_op_async.js`, let’s copy the code from the previous exercise (`1-redis_op.js`)

Using `promisify`, modify the function `displaySchoolValue` to use ES6 `async / await`

Same result as `1-redis_op.js`

```
bob@dylan:~$ npm run dev 2-redis_op_async.js

> queuing_system_in_js@1.0.0 dev /root
> nodemon --exec babel-node --presets @babel/preset-env "2-redis_op_async.js"

[nodemon] 2.0.4
[nodemon] to restart at any time, enter `rs`
[nodemon] watching path(s): *.*
[nodemon] watching extensions: js,mjs,json
[nodemon] starting `babel-node --presets @babel/preset-env 2-redis_op_async.js`
Redis client connected to the server
School
Reply: OK
100
^C

bob@dylan:~$
```

  * [2-redis_op_async.js](./2-redis_op_async.js)

**4. Node Redis client and advanced operations**

In a file named `4-redis_advanced_op.js`, let’s use the client to store a hash value

**Create Hash:**
Using `hset`, let’s store the following:

  * The key of the hash should be `ALX`
  * It should have a value for:
    * `Portland=50`
    * `Seattle=80`
    * `New York=20`
    * `Bogota=20`
    * `Cali=40`
    * `Paris=2`
  * Make sure you use `redis.print` for each `hset`

**Display Hash:**

Using `hgetall`, display the object stored in Redis. It should return the following:

**Requirements:**

  * Use callbacks for any of the operation, we will look at async operations later

```
bob@dylan:~$ npm run dev 4-redis_advanced_op.js 

> queuing_system_in_js@1.0.0 dev /root
> nodemon --exec babel-node --presets @babel/preset-env "4-redis_advanced_op.js"

[nodemon] 2.0.4
[nodemon] to restart at any time, enter `rs`
[nodemon] watching path(s): *.*
[nodemon] watching extensions: js,mjs,json
[nodemon] starting `babel-node --presets @babel/preset-env 4-redis_advanced_op.js`
Redis client connected to the server
Reply: 1
Reply: 1
Reply: 1
Reply: 1
Reply: 1
Reply: 1
{
  Portland: '50',
  Seattle: '80',
  'New York': '20',
  Bogota: '20',
  Cali: '40',
  Paris: '2'
}
^C
bob@dylan:~$
```

  * [4-redis_advanced_op.js](./4-redis_advanced_op.js)

**5. Node Redis client publisher and subscriber**

In a file named `5-subscriber.js`, create a redis client:

  * On connect, it should log the message `Redis client connected to the server`
  * On error, it should log the message `Redis client not connected to the server: ERROR MESSAGE`
  * It should subscribe to the channel `ALXchannel`
  * When it receives message on the channel `ALX channel`, it should log the message to the console
  * When the message is `KILL_SERVER`, it should unsubscribe and quit

In a file named `5-publisher.js`, create a redis client:

  * On connect, it should log the message `Redis client connected to the server`
  * On error, it should log the message `Redis client not connected to the server: ERROR MESSAGE`
  * Write a function named `publishMessage`:
    * It will take two arguments: `message` (string), and `time` (integer - in ms)
    * After `time` millisecond:
      * The function should log to the console `About to send MESSAGE`
      * The function should publish to the channel `ALX channel`, the message passed in argument after the time passed in arguments
  * At the end of the file, call:

```
publishMessage("ALX Student #1 starts course", 100);
publishMessage("ALX Student #2 starts course", 200);
publishMessage("KILL_SERVER", 300);
publishMessage("ALX Student #3 starts course", 400);
```

**Requirements:**

  * You only need one Redis server to execute the program
  * You will need to have two node processes to run each script at the same time

**Terminal 1:**

```
bob@dylan:~$ npm run dev 5-subscriber.js 

> queuing_system_in_js@1.0.0 dev /root
> nodemon --exec babel-node --presets @babel/preset-env "5-subscriber.js"

[nodemon] 2.0.4
[nodemon] to restart at any time, enter `rs`
[nodemon] watching path(s): *.*
[nodemon] watching extensions: js,mjs,json
[nodemon] starting `babel-node --presets @babel/preset-env 5-subscriber.js`
Redis client connected to the server
```

**Terminal 2:**

```
bob@dylan:~$ npm run dev 5-publisher.js 

> queuing_system_in_js@1.0.0 dev /root
> nodemon --exec babel-node --presets @babel/preset-env "5-publisher.js"

[nodemon] 2.0.4
[nodemon] to restart at any time, enter `rs`
[nodemon] watching path(s): *.*
[nodemon] watching extensions: js,mjs,json
[nodemon] starting `babel-node --presets @babel/preset-env 5-publisher.js`
Redis client connected to the server
About to send ALX Student #1 starts course
About to send ALX Student #2 starts course
About to send KILL_SERVER
About to send ALX Student #3 starts course
^C
bob@dylan:~$ 
```

**And in the same time in Terminal 1:**

```
Redis client connected to the server
ALX Student #1 starts course
ALX Student #2 starts course
KILL_SERVER
[nodemon] clean exit - waiting for changes before restart
^C
bob@dylan:~$ 
```

Now you have a basic Redis-based queuing system where you have a process to generate job and a second one to process it. These 2 processes can be in 2 different servers, which we also call “background workers”.

  * [5-subscriber.js](./5-subscriber.js)
