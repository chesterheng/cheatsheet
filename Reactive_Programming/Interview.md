What is RxJS?
> It’s a library that is very much like Lodash but it’s Lodash for Async events. So you can get events and treat them as sets of things, which allows you to filter, map, and combine them, so you can build very powerful apps with sets of events over time.

What is the main difference between Promises and Observables?
* Observables are a set of values, zero to an infinite number of values, where Promises are only one value. 
* Observables are unicast whereas Promises are multicast
* Observables can be synchronized or asynchronized but Promises are always asynchronized. 
* Observables can be canceled and Promises cannot be canceled.

So tell us when to use each, when is the best case to use Promises and when it’s recommended to use Observables.
> The best time to use Promises is when you want an absolute guarantee that you will get a value. It’s called a Promise because you get one value in the future. So if you want a guarantee, Promises are great. Actually, there is an API right now on RxJS 5 that uses Promises where you can subscribe to Observables with “foreach()” and it returns a Promise that tells you whether or not it concluded successfully because it’s a non-cancelable way to subscribe to an Observable. Observables, on the other hand, are best used in situations when you need to compose a lot of different types of events: key ups or mouse events or if you’re doing a lot of live streaming, like Websockets. They are great for animations, they are great for a variety of general use.

Anything that lets you stream values. Right?
> Yes. But not even just streams, you can do things that are one-time used, like AJAX, for example, where you want to be able to abort the underlying XHR so that you will be able to abort the AJAX requests. The Observables distinctly work well for that as opposed to Promises, because you can’t abort a Promise.

Alright. So for beginners, what’s your advice on how to get started with RxJS or just Observables?
> The best thing to do to get started with Observables is probably to jump in and import RxJS from the CDN and start playing around with it. The thing I like to do is to create an interval Observable so you can say: Observable.interval(), and give it some timespan which creates this set Observables that gives you the values 0, 1, 2, etc. over time and that way you got a series of events that you can play with and get to know some of the operators. Then, the first one I would start with is map and filter (because people are usually familiar with those) and then I would look at some of the merge operations like mergeMap and switchMap.

What about the resources available on the internet?
*
* 

Let’s talk about RxJS: what’s coming next?
> We’ll first release version 5, and there’s a plan for 6th. The idea is that when you have a major release there’s going to be some breaking changes. For 6, the API changes will be very minimal. But there are a few small changes that will affect people, I think around 1% of users that are building their own operators and things like that. But we want to make some changes that will allow a much better speed, and the other thing that will be interesting is that it will give known users the ability to « panic and die » if there’s an error. A lot of known developers want the ability to panic and die if there is an error, but right now Promises and Observables catch those errors and brings them along. So what we are trying to do is that when there’s an error, it doesn’t catch it at all, but lets the process die so that the developers can get better insight to what happened.

Nice. Speaking about errors, what is the best way to catch errors?
> The best way to catch errors is with the catch operator, which works like the catch operator that works for Promises where you give a function and it gives you an error. You can say catch this and go ahead and we try the previous Observable.

What is your favorite operator and why?
> There are so many. I’ll drill down to my two favorites. One is SwitchMap because it’s probably the most common one that is used, like merge operator, and it’s very important. The other one is retryWhen, because it’s the first one I implemented for RxJS and there are a lot of very interesting features to it. And it’s fun.

Did you implement all the operators in RxJS?
> Yes in RxJS 5.

What was the purpose behind the rewrites of RxJS 5?
> The purpose with the rewrite was a few things. One: we wanted better performance than the previous versions. Two: Better debug capacity, the ability to have cleaner call stacks. And we were able to achieve both of those things. RxJS 5 is 5x to 25x faster than the previous versions (depending on what you are doing). The third and final thing we wanted is that our Observables match the Observables specs in the TC39 proposal. So if Observable land in JavaScript, people who are using RxJS will have an advantage because it will be roughly the same.

Last question, how come angular 2 uses RxJS heavily?
> Angular 2 uses RxJS heavily because, I kind of have some friends on the core team! I went there and I lobbied them then I brought Jafar Husain to come and lobby them as well. Jokes aside, for a single page apps, the previous option was to use Promises. And Promises are really only a great fit for AJAX. With the exception of a Single Page App where users are only switching views here and there. If you go to a view with one HTTP value and then you go to some other view before that HTTP value comes back, then you can’t cancel the Promise, and you are going to execute a logic you probably didn’t want to. A better option is to use the asynchrony that comes in Observables. That was an easy sell. Once they saw what you could do, they were convinced.

Anything to add?
If people are interested, they should check a few videos, and just start hacking around with it, and not worry too much.
