---
title: Creating an API to test an API
author_name: Marc
tags:
  - postman
  - express
  - api testing
  - mongoose
  - mongo db
---

I wasted a lot of time back in the day writing my API test frameworks. I learned
a lot from doing it, so I suppose it wasn't a total waste. The main thing I learned
was not to roll by own API frameworks.

These days I'm using [Postman](https://www.getpostman.com/). Its strength and weakness
is that it does exactly one thing: sends API requests and tests the results. There is a 
JavaScript sandbox available, but the Postman team seems to have been very deliberate
about limiting the inclusion of extraneous JS libraries. So my natural instinct to want 
my tests to upload test files, check database records, monitor log files, etc., is shut
down.

But recently I cam across a scenario where I needed to verify an API response that required
checking a value that is intentionally not exposed to the user. For example, if a user is
blacklisted you want to make sure that they are not allowed access to the system but you 
don't necessarily want the user to know why they are blacklisted.

So I set out to create the simplest API I could to check this so that my Postman scripts can
call the API to check the user status.  The database we're using is MongoDB, so I'm using 
Mongoose. I've been using Express for some other projects I have in flight, so I decided to use
it here as well.

The whole thing is one file that clocks in at 24 lines of code. Let's take a look.

```
require ('dotenv').config()
```
`dotenv` allows the script to pull info out of a `.env` file
so that environment configuration is stored separately from
the script.

```
var express = require('express')
var app = express()
var port = process.env.PORT || 4001
```
This is the configuration for the web server. The port number
is pulled from the `.env` file. If it doesn't exist there, 
it will default to use port 4001.

As an aside, I'm never sure how developers determine what
default port to use for their projects. Using 80 or 8080 seems
presumption to assume those are not already in use. I chose
4001 because there is a [Rickenbacker 4001](https://en.wikipedia.org/wiki/Rickenbacker_4001) sitting 
on the stand next to the desk in my home office.

<figure style="width: 300px" class="align-center">
	<img src="/assets/images/rickenbacker.jpg">
	<figcaption>Author with Rickenbacker 4001 back when he still had hair.</figcaption>
</figure>

```
var mongoose = require('mongoose')
mongoose.connect(process.env.MONGO_CONNECTION_STRING,
    { useNewUrlParser: true, useFindAndModify: false,
    'useCreateIndex': true })
```
MONGO_CONNECTION_STRING is how we tell mongoose to 
connect to a specific MongoDB instance. 

```
var Schema = mongoose.Schema

var blacklisted = new Schema({
    _id: mongoose.Types.ObjectId,
    blacklisted: Object,
})

var BlacklistedModel = mongoose.model('user', blacklistedSchema)
```
The mongoose schema defines the rules of what
data can be stored in the database. In a non-trivial
app, schemas can be complicated beasts. My initial
thought was to grab a copy of the actual schema our
application was using, but realized I would be going down the path of creating a bigger API than the one I was testing. So I chose to restrict the schema to 
the `_id` of the user and their `blacklisted` status.

```
app.get('/user/:id', async (req, res) => {
    var user = await BlacklistedModel.findOne(
      {'_id': mongoose.Types.ObjectId(req.params.id)}, 
      {'_internal': 1})
      .exec()
    res.send(user)
})
```
This is the piece that does the heavy lifting for our API. The route
is define as `/user/:id`. When a request comes to that, mongoose will query the `user` collection with the id specified as `:id` and will return the response to the user.

Request:
```
http://localhost:4000/user/somemongodbuserid
```

Response:
```
{
	"_id": "somemongodbuserid",
	"blacklisted": {
		"reason": "illegal activity",
		"date": "2019-01-03"
	}
}
```

With this I can now add requests to my Postman scripts
to check user data that is not available our application's API.

After using this for a short time, I made a couple of changes:
* Change the model and express route to a variable so that I could query any collection in the database
* Added a separate route to except query parameters so I could look up users by fields other than `_id`

### TL;DR
* `git clone git@github.com:marcallenbetts/simple-mongo-api.git`
* `npm install`
* `npm start`