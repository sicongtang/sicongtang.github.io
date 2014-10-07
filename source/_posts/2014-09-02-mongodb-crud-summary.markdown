---
layout: post
title: "MongoDB CRUD Summary"
date: 2014-09-02 13:41:21 +0800
comments: true
categories: sql
---

```
w, {Number/String, > -1 || ‘majority’ || tag name} the write concern for the operation where < 1 is no acknowlegement of write and w >= 1, w = ‘majority’ or tag acknowledges the write
j, (Boolean, default:false) write waits for journal sync before returning
```
##system admin
{% codeblock lang:js %}

db.serverStatus()
db.serverStatus().connections

{% endcodeblock %}

##insert
{% codeblock lang:js %}

db.foo.insert({"bar" : "baz"})
db.foo.batchInsert([{"_id" : 0}, {"_id" : 1}, {"_id" : 2}])
//One of the basic structure checks is size: all documents must be smaller than 16 MB.
Object.bsonsize(doc)

{% endcodeblock %}

##delete
{% codeblock lang:js %}

//like detele from, high time cost
db.foo.remove({})
db.users.remove({status: "D"})
//like truncate
db.tester.drop()

{% endcodeblock %}

##update
{% codeblock lang:js %}

db.people.update({"_id" : ObjectId("4b2b9f67a1f631733d917a7c")}, joe)

db.users.update({"name" : "joe"}, {"$set" : {"favorite book" : ["Cat's Cradle", "Foundation Trilogy", "Ender's Game"]}})
db.blog.posts.update({"author.name" : "joe"}, {"$set" : {"author.name" : "joe schmoe"}})
db.users.update({"name" : "joe"}, {"$unset" : {"favorite book" : 1}})


db.blog.update({"post" : post_id}, {"$inc" : {"comments.0.votes" : 1}})
db.blog.update({"comments.author" : "John"}, {"$set" : {"comments.$.author" : "Jim"}})


//full-document replacement, replacing the matched document with {"foo" : "bar"} 
db.coll.update(criteria, {"foo" : "bar"})

db.users.update({"_id" : ObjectId("4b2d75476cc613d5ee930164")},
                      {"$addToSet" : {"emails" : {"$each" : ["joe@php.net", "joe@example.com", "joe@python.org"]}}})

db.movies.find({"genre" : "horror"}, 
     {"$push" : {"top10" : {
          "$each" : [{"name" : "Nightmare on Elm Street", "rating" : 6.6}, {"name" : "Saw", "rating" : 4.3}],
          "$slice" : -10,
          "$sort" : {"rating" : -1}}}})

{"$pop": {"key": 1}}
{"$pop": {"key": -1}}


//remove all the matching documents
db.lists.update({}, {"$pull" : {"todo" : "laundry"}})

//positional array modification
db.blog.update({"post" : post_id}, {"$inc" : {"comments.0.votes" : 1}})
db.blog.update({"comments.author" : "John"}, {"$set" : {"comments.$.author" : "Jim"}})


//upserts
//eliminate the race condition and faster and atomic

//If no document is found that matches the update criteria, a new document will be created by combining the criteria and updated documents.
db.analytics.update({"url" : "/blog"}, {"$inc" : {"pageviews" : 1}}, true)
//If we run this update again, it will match the existing document, nothing will be inserted, and so the "createdAt" field will not be changed.
db.users.update({}, {"$setOnInsert" : {"createdAt" : new Date()}}, true)
db.foo.save(x)
db.foo.update({"_id": x._id}, x)

//updating multiple documents

db.count.update({x : 1}, {$inc : {x : 1}}, false, true) 
db.runCommand({getLastError : 1})
{
"err" : null,
"updatedExisting" : true, 
"n" : 5,
"ok" : true
}



{% endcodeblock %}

##select
{% codeblock lang:js %}

//
db.users.find()
//only display cols
db.users.find({}, {"username" : 1)
//only display cols without _id
db.users.find({}, {"username" : 1, "_id" : 0})

//count
db.users.count()

//conditionals are an inner document key, and modifiers are always a key in the outer document.
//There are a few “meta-operators” that go in the outer document: "$and“, "$or“, and "$not“.
db.users.find({"id_num" : {"$not" : {"$mod" : [5, 1]}}})

db.users.find({"x" : {"$lt" : 1, "$in" : [4]}})

// will return all the docs
db.c.find({"z":null})
// only display null value, please use this
db.c.find({"z" : {"$in" : [null], "$exists" : true}})

//indexes cannot be used for case-insensitive searches
db.users.find({"name" : /joe/i})
db.users.find({"name" : /joey?/i})

//querying array
db.food.find({fruit : {$all : ["apple", "banana"]}})
db.food.find({"fruit" : {"$size" : 3}})
db.blog.posts.findOne(criteria, {"comments" : {"$slice" : [23, 10]}})
db.blog.posts.find({"comments.name" : "bob"}, {"comments.$" : 1})

//$where

//pagination
db.stock.find({"desc" : "mp3"}).limit(50).skip(50).sort({"price" : -1})

//find a random document
var random = Math.random()
result = db.foo.findOne({"random" : {"$gt" : random}}) 
if (result == null) {
  result = db.foo.findOne({"random" : {"$lt" : random}}) 
} 


//cursor
cursor = db.foo.find();
while (cursor.hasNext()) {
  var doc = cursor.next();
  doc = process(doc);
  db.foo.save(doc);
}

//command
use temp
db.runCommand({shutdown:1})
db.adminCommand({"shutdown" : 1}) 


{% endcodeblock %}

