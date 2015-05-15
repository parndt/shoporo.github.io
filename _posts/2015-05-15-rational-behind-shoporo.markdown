---
layout: post
title: Rational Behind Shoporo
modified:
categories:
excerpt:
tags: []
image:
  feature:
date: 2015-05-15T18:46:41+01:00
---

This post will describe the background and reason why and what I’m trying to solve by building Shoporo. Do note that everything you read after this is bound to change when thoughts meet the real world :)

## Some background first
For years (since 2008) I have been working with and on Spree. I still believe it’s a great framework but it has grown too much. It’s complex by nature, I mean we are talking about an e-commerce framework, setup to be as flexible and extendable as possible.

For a lot of my clients (and for my personally) this is too much of what they need. During my time at Spree we experimented a lot with PORO versions of Spree and never really succeeded. Always bound within the existing constraints of the framework. A very large group of people and businesses are depending on the framework as is. Therefore we could never really make it happen. To make it right (IMHO), it would break everything leaving those companies out to dry.

Since May 1st I’m no longer working for Spree, while I will still be working and using Spree myself I wanted to just start writing the platform that would suit me the best. Yes it will be very opinionated and it will not be suitable for everyone out there. When you are running a large e-commerce site, you are better of with Spree, and with the current state of this project i.e. non-existing as of yet, you should go with Spree as well :)

That was enough background and introduction, let’s get into some of the details and ideas now.

## API’s and PORO’s
I have always had a hate-love relationship with ActiveRecord, loved it for rapid prototyping, hated it when things get more complex and never really got used to the fact that the AR instances in the view had direct access to the database. With Shoporo this will be not there. The [Lotus web framework](http://lotusrb.org/) will be the underlying system. The first part that will be written is `Shoporo::Model`. This gem will be the heart of the system, containing all the entities and public API’s needed for running a webshop. There will be no html, no views, no session state just `Entities`, `Repositories`, `Interactors` and probably some utilities and helper classes. Oh and of course, ton loads of specs and documentation.

When I say API, I do not talk about a RESTFull JSON api, but about the programmers interface of the system. This API will be clean and easy to understand.

I do have plans to add `Shoporo::Api` as well, following the [JSONApi Standard](http://jsonapi.org/) so developers can build their beautiful javascript html5 responsive webshops the way they like it. Even using nothing more then HTML and javascript.

As I said before this project is very opinionated and I like Postgresql a lot. Shoporo will be running on Postgresql 9.4. I have no idea what the future would hold, but from the start this will be designed and developed using Postgresql. 9.4 has very powerful json options, with jsonb indexes we will have a very fast and interesting options available to model the entities.

That being said, the architecture does allows for swapping out adapters to connect to other data storage systems.

## Core ideas and guidelines
One of the main ideas and mantras for Shoporo is that it should be simple. That does not make it easy though. Like I said, e-commerce is complex and will always be complex. However, I intent to keep the data and relations as lightweight and slim as possible. There will be no Product > Variant relationship for example, these will be (semi) dynamic based on the specifications on the product (more on that later). There will also not be any stock locations or split shipments, and for sure no state machines that are triggered from callbacks.

Speed! the interaction and calculations should be blazing fast, everything added to the project should ensure it either improves or keeps the performance equal. (Boyscout rule)

Documentation, all ruby classes will have inline comments describing the behaviour, including samples.

When `Shoporo::Model` is done it should provide any ruby developer to extend it for his or her needs.

We also follow the patterns and architecture from The [Lotus web framework](http://lotusrb.org/).

## Entities
The following list contains the first draft of the entities that will be in `Shoporo::Model`

* `Store`
* `Product`
* `Order`
* `Payment`
* `Shipment`
* `Tax`
* `User`

You might think that I forgot to mention `Cart`, but I intentionally left that one off the list. That part should not be in the core. The cart is basically a Hash or JSON that will be the parameter when you create the `Order` in the system. While this idea has not been fully thought out, it might change in the future :)

Also, this is not the final list. We should be able to handle any coupons or other adjustments for example. More on this in later post, if you want to follow along or provide input the repository can be found here: [Shoporo::Model](https://github.com/shoporo/model)
