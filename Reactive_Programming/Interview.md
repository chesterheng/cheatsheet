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

1. Is RxJS following the “push” or “pull” pattern?
* In RxJS new data is being “pushed” to observers.

2. What’s the difference between next(), complete() and error()?
* These are the callbacks an Observable / Flowable will receive. 
* The first one is called for each emission of the Observable / Flowable (e.g. zero to infinity times). 
* complete() and error() are mutually exclusive – only ONE of them will be called at most once. 
* In other words a stream cannot complete and error out at the same time.

3. How many times can each of the above be called?
* next() – from zero between infinite number of times
* complete() – maximum once per stream
* error() – maximum once per stream

4. When does an observable start emitting items?
* There’s two types of Observables – Cold and Hot. 
* Cold ones perform work (and subsequently emit items) only once someone is listening for it (e.g. someone has subscribed to them). 
* Hot observables perform work and emit items regardless if there are any observers or not.

5. What’s the difference between a COLD and HOT observables?
* They start emitting items differently (see #4).
* Cold observables are created multiple times and each instance can be triggered on it’s own. 
* Hot observables are like a “stream” of ongoing events – observers can come and go, but the stream is created ones and just goes on.

6. Can you transform a COLD observable to a HOT one and vice-versa?
* One way to make a Cold observable Hot is by using publish() and connect(). 
* publish() converts the Cold observable to a ConnectableObservable, which pretty much behaves like a Hot one. 
* Once triggered with the connect() operator, it’ll publish events regardless if there are any subscribers.

* Another way to transform a Cold observable to a Hot one is by wrapping it with a Subject.
* The Subject subscribes to the Cold observable immediately and exposes itself as an Observable to future subscribers. 
* Again, the work is performed regardless whether there are any subscribers … and on the other hand multiple subscribers to the Subject won’t trigger the initial work multiple times.

7. … and a HOT into to a COLD one?
> The first way of transforming (or rather “masking”) a Hot observable to Cold is by using the defer() operator. It defers the creation of the Hot observable altogether, so each new subscriber will trigger the work again (feature of a Cold observable).

> Depending on the use-case the pattern mentioned above might be quite wasteful, so another strategy is using the replay().autoConnect(0) paradigm. The replay() operator will cache the values emitted by the Hot observable and re-emit them to future subscribers. autoConnect(0) returns an observable that can be triggered even when there are no subscribers to the underlaying Hot observable. The combination of both just replays cached values from the Hot observable as a Cold one.

8. What’s a Scheduler? Why does RxJS use Schedulers?
* By default RxJS is single-threaded – all operations are executed on a single thread. 
* Schedulers are the means to switch the execution to a different thread. 
* They’re also an abstraction over the concept of “time”, which is needed for time-sensitive operations (delay(), timeout(), buffer(), window(), etc).

9. What’s an Observable chain?
* A list of operations / transformations performed between the source and the end subscriber. 
* A simple example is emitting a User object, filtering out admin users (filter()), checking whether they’re authenticated (filter()) and finally emitting they’re full name (map()).

10. Can you have multiple operators of the same type in a single chain (e.g. map().map().map()…)?
Yes, and it’s actually a good practice to have these. Ideally every operator is a “pure” function (see #39) that performs a single filtering / transformation / collection from an input -> output. Splitting complex functions into multiple simple ones makes them more composable and easy to understand. A small downside is performance (since every operator in a chain requires a bit of execution time and perhaps memory), so as always – balance is needed.

11. Does the above work with all operators?
Works as expected for pretty much all operators from the standard library except subscribeOn(). See #13 for details why multiple subscribeOn() operators don’t make sense.

12. What’s the difference between observeOn() and subscribeOn()?
subscribeOn() denotes the Scheduler on which the source work will be performed on. Since there’s only one initial source of an Observable chain, it makes sense to only have one subscribeOn() operator.

observeOn() denotes the Scheduler on which all downstream operations will be performed. In other words it changes the Scheduler for all operators after it. Since there can be many such operators, having multiple observeOn() operators in a single chain makes sense and works as expected.

13. What’s going to happen if you have multiple subscribeOn() operators in a chain?
Only the very first subscribeOn() in the chain has the desired effect, all subsequent ones would not have any effect, apart from potentially wasting system resources (depending on the Scheduler specified). For a detailed explanation read THIS article.

14. What’s going to happen if you have multiple observeOn() operators in a chain?
Each observeOn() switches the Scheduler (thread) on which all subsequent operators will be executed on. Complex RxJava streams can absolutely benefit from multiple observeOn() operators. For a detailed explanation read THIS article.

15. What’s the difference between a map() and a flatMap()?
The map() operator maps from a concrete ValueA to a concrete ValueB (e.g. from an Int -> String, or User -> String). flatMap() maps from a concrete ValueA to a Stream<ValueB>. If Stream<ValueB> emits multiple items, all of these will be eventually served to the original observer (e.g. they are being “flattened” to a single Observer). Since there’s no restrictions on the Stream<ValueB>, flatMap() is useful to introduce parallelism in executing tasks (see #35).

16. What’s the difference between flatMap(), concatMap() and switchMap()?
The difference is best illustrated using marble diagrams (see #38), but …

flatMap() can split a chain to multiple intermediate streams (let’s call them [A, B, C]), the results of which are all “flattened” to a single stream again. The emissions of these intermediate streams are propagated directly to the main stream without any guarantees about the order in which they’ll appear. It’s entirely possible the end result to look like this: [B1, A1, B2, C1, B3, A2, C2, C3, A3]

concatMap() works similarly to flatMap() with the exception that the intermediate streams are “activated” in the order they appear. The end result will look like this: [A1, A2, A3, B1, B2, B3, C1, C2, C3]

switchMap() is again similar to flatMap() with the exception that whenever a new item is emitted from the source Observable, all previously generated intermediate streams are terminated (e.g. disposed()). At this moment only the latest intermediate Observable remains active. With switchMap() it’s entirely possible for events to be dismissed altogether, so a possible output from the example above is: [A1, B1, B2, C1, C2, C3]. In this example the source Observable emitted item B when the A intermediate observable has emitted only A1. Similarly, the source emitted C before the intermediate stream B could finish (thus B3 event is missing).

17. What’s the most complex operator you know?
It’s really hard to single out one here. The operations of ConnectableObservable and their correct usage took a while to grasp (publish().connect(), refCount(), etc). Also getting familiar with the most complete overload of flatMap() is quite beneficial, as it helps you understand how the operator works, but also unlocks other paradigms like parallelism (see #35).

18. Can one create custom operators in RxJava? Anything to be aware of?
Although you can create your own operator, it’s highly recommended to reuse existing ones or any combination of them. Implementing a new operator correctly is very tricky and there’s too much room for errors (backpressure, breaking an API contract, thread safety, etc). Still, if required, THIS is the best read to get started.

19. What’s a Transformer?
A Transformer is a convenient way to encapsulate common operations in a reusable way. This encapsulated logic can be tested in isolation, which is easier, and further simplifies the tests of all chains that use it. For example if you find yourself repeating a sequence of operators multiple times in your code (e.g. .map(user -> user.age).filter(age -> age > 18)), this logic can be factored out in a UserLegalAgeTransformer. It can be reused in the chains like this .compose(new UserLegalAgeTransformer()).

20. Describe the most complex chain you’ve implemented? What was the tricky bit there?
Can’t recall a specific example, but it must be a long one that combines multiple data sources. Generally the tricky bit is decomposing the business requirements to multiple pure functions (see #39). Once that’s done one can find really good and descriptive name for each operation in the chain, thus making it easier to reason about.

Here’s a paradigm I really like for implementing a very simple Repository pattern using the concat().take(1) operators:

1
2
3
4
5
6
7
8
9
fun fetchData(val id: Long): Single<Data> {
    return Observable.concat(
        getFromLocalCache(id),
        getFromPersistentCache(id),
        getFromNetwork(id)
           )
        .take(1)
        .singleOrError()
}
All complex logic is hidden away in the respective getFrom() methods, thus making the chain easily understandable. Concat() will exhaust each getFrom() method until it finishes. If a value is received, the take(1) operator “stops” the whole chain, so subsequent getFrom() methods won’t be called. For example, if there’s no data in the local cache, getFromLocalCache() will just complete without emitting any values. If getFromPersistentCache() returns a value, the take(1) operator terminates the rest of the chain, so getFromNetwork() will not be executed at all.

21. How can you test observable chains? Mention a few classes that can help and their function.
TestObserver and TestSubscriber are common classes used to test Observable / Flowable operations. With these you can wait for, and inspect all received events and their exact values.

The TestScheduler class is very useful when testing time-based operators (e.g. timeout(), buffer(), window(), etc) – it allows to manually control the “time” to you can test all possible code paths in your chain.

22. What error handling operators do you know in RxJava?
There’s two main categories of such operators – one for performing side-effects only, after which the error is passed downstream (doOnError(...)). The other category is the onErrorReturn(...), onErrorResumeNext(...) ones, which can handle an error and continue the stream successfully.

23. What will happen if there’s multiple errors in a chain?
All unhandled errors (via any of the error handling operators, see #22) are propagated downstream. A chain can have only one terminal error event (e.g. one call to onError(throwable)), so the first unhandled error will terminate the stream. In case there’s other undelivered exceptions, please see #24.

24. Handling multiple errors (see example code).
What will happen (in terms of error handling) if getAllUsers() emits a sequence of 10 userIds and getUserProfile(userId) emits error for every userId? Is the behaviour different between RxJava1 and RxJava2?

1
2
3
4
getAllUsers()
    .flatMap(userId -> getUserProfile(userId).subscribeOn(Schedulers.io()))
    .onErrorResumeNext(...)
    .subscribe(...)
In RxJava1 the first propagated error will terminate the stream. All other started parallel streams (that’ll error out as well) are the so-called “undelivered exceptions”, which are just “swallowed” (printed in console by default). Difference with RxJava2 comes from the handling of these undeliverable exceptions – they’ll be sent to a global error handler (set via RxJavaPlugins.setErrorHandler()), where further handing can occur. If such error handler isn’t set, the exceptions are propagated upstream to the calling thread (e.g. will cause a crash of the app).

25. What are the biggest differences you know between RxJava1 and RxJava2?
Here’s the 3 differences one surely needs to take into account when updating to RxJava2:

Null not supported anymore – previously Observable.just(user) would work even if user == null. In RxJava2 the same will throw a NullPointerException. Not supporting null values makes streams a bit easier to work with (as there’s no null checks everywhere), but one must be more cautious what data flows through them. Wrapping emissions with classes like Java Optional is handy for places where you’re not sure if the data is null-safe.

RxJava2 Observable type doesn’t support backpressure anymore, so places where that might be an issue had to be changed to use Flowable instead. It’s a great change as it makes pretty clear where Backpressure care is needed. On the other hand – it requires careful inspection of the flows that were using RxJava1 Observable. Common places where Backpressure might occur is when fetching data from a database or mapping user-actions to events. On a related note – in RxJava2 Subjects no longer support Backpressure. The new Processors classes do.

Global error handling – in RxJava2 no error can be swallowed (as it could in RxJava1). All such undelivered errors are passed to a global error handler, which can be set using the RxJavaPlugins.setErrorHandler(). Such exceptions usually occur on streams that have parallel execution. When updating, one must always set such handler and at least log the errors. Ideally this global error handler will never receives anything.

26. Which RxJava construct you’ll use to represent an API call that needs to be called at some point in the future?
Usually API calls either return a response (onSuccess()) or error out (onError()), so Single is a great fit here.

27. What if the API call is a “fire-and-forget” one?
Since we’re not interested in the response of the API call, a Completable is a good it. If we’re not interested in errors either, adding onErrorComplete() operator will achieve the “fire-and-forget” nature.

28. Do you know any RxJava constructs other than Observable? Describe them?
The main constructs are:

Completable – maps an operation that either completes without returning a value (onComplete()) or errors out (onError(throwable)).

Single – either returns a value (onSuccess(value)) or errors out (onError(throwable)).

Maybe – has 3 options – returns a value successfully (onSuccess(value)), completes successfully without any value (onComplete()) or errors out (onError(throwable)).

Observable – represents a stream of events that emits zero to many events (onNext(value)), then either completes (onComplete()) or errors out (onError(throwable)). It does NOT support backpressure (see #32).

Flowable – like an Observable, however it DOES support backpressure.

29. What’s a Subject in RxJava and what’s it used for?
A Subject is both a Subscriber and an Observer at the same time. My favourite analogy is a pipe – it can receive events from one end (because it’s a Subscriber) and let them through (“emit them”) via the other end because it’s an Observable. With Subjects one can transform Cold observables to Hot ones (see #5). Subjects also are one of the easiest ways to introduce some type of local, temporary caching of a stream. Last but not least – Subjects can help you transform your non-reactive code to reactive if you don’t find any of the Observable.create() operators useful for your use-case.

30. Are there different types of Subjects?
There are 4 common Subject types:

PublishSubject – just passes incoming events to all it’s subscribers. New Subscribers will receive only the events emitted from the point of Subscription onwards.

BehaviourSubject – like PublishSubject, however each new Subscriber also receives the latest value of the stream (or a default value). The default value often provides nice user experience, as consumers of the Subject don’t need to worry about receiving empty streams.

AsyncSubject – emits only the last value emitted by the source Observable and only after that source Observables completes. Common use-case is for operations that need to finish before all clients (Subscribers) can proceed – e.g. the initial loading of an application.

ReplaySubject – every Subscriber will receive all events emitted by the source Observable, regardless at which point they subscribe. In other words all events are “cached” via the Subject. That’s one of the easiest ways to cache a stream, but one needs to be careful of memory issues (if the source Observable emits too many items that need to stay in the memory cache).

31. What’s the difference between Subject and RxRelay?
A Relay is a Subject that cannot be terminated (cannot call onError() or onComplete() on it). That’s sometimes useful, as terminating a regular Subject makes it unusable in the future.

32. What is backpressure?
Backpressure is the inability of a Subscriber to handle all incoming events in time. In other words Backpressure can occur when the Producer of events is faster than the Consumer. If not handled correctly it can error out a stream.

33. How to deal with backpressure issues?
The first important thing is to choose the correct RxJava construct for your stream. If you thing Backpressure might occur, then Flowable with a correct BackpressureStrategy is the safest choice. You can also try to manually “slow-down” the Producer by adding buffer-type (buffer(), window(), etc) operators before your event handling. Finally you can try to speed up your Consumer – ideally it should be doing small and fast operations. If you require more computation-intensive ones, perhaps some of that logic can be moved to- and parallelised by the Rx stream ifself.

34. How does data flow in RxJava by default?
Pick all that apply: sequential, parallel, overlapping, non-overlapping, single-threaded, multi-threaded?

RxJava is single-threaded by default. One can use Schedulers (see #8) to change this if needed. Data flows sequentially by default, and events don’t overlap each other unless an operation is specifically parallelised.

35. Does RxJava support parallelism? If so, how to achieve it?
The two most common ways to execute a set of tasks in parallel are:

a) Using the flatMap() operator. Each inner stream inside the flatMap() operation should subscribeOn() a background thread (preferably Schedulers.io()). An example to note the difference:

1
2
3
4
5
6
7
8
9
// This version is sequential
getUserIds()
    .flatMap(userId -> getUserProfile(userId))
    .subscribe(...)
 
// This version is parallel
getUserIds()
    .flatMap(userId -> getUserProfile(userId).subscribeOn(Scheudlers.io()))
    .subscribe(...)
The first example is synchronous and there’s no parallel execution – the User profiles are fetched one after the other. That’s done on the Scheduler inherited from the getUserIds() Observable. In the second example however user profiles are fetched in parallel. The number of parallel executions can be controlled if an overload of flatMap() is used, where you specify the maxConcurrency parameter.

b) Although the above pattern works correctly to achieve parallelism, in recent versions of RxJava a better construct was introduced – the ParallelFlowable. It provides an easier and more explicit API to achieve parallelism. Let’s rewrite the example from above:

1
2
3
4
5
6
getUserIds()
    .parallel(16)
    .runOn(Schedulers.io())
    .map(userId -> getUserProfile(userId))
    .sequential()
    .subscribe(...)
The maxConcurrency is specified explicitly in the parallel() method. The scheduler (or thread pool) where the parallel streams should be executed is specified by the runOn() operator. The sequential() operator is how you switch back to the “sequential” world. All operations between .parallel() and .sequential() are executed in parallel streams.

36. Are memory leaks an issue when using RxJava? How would you protect from such?
As a general good practice in programming, one must clean-up the used resources after they’re no longer needed. In the case of RxJava this means disposing your Disposables correctly. A common pattern is to keep adding all long-running operations from a screen in a CompositeDisposable and ensuring that’s clean-up when the screen is gone.

One thing to watch out for is creating new Disposables in the middle of a chain.

1
2
3
4
5
compositeDisposable.add(
        getUserFromNetwork(userId)
       .doOnNext(user -> fireAndForgetOperation(user).subscribeOn(Schedulers.io()).subscribe())
       .subscribe(...)
)
In the example above fireAndForgetOperation(user).subscribeOn(Schedulers.io()).subscribe() creates a new Disposable that won’t be automatically cleaned up if the compositeDisposable is disposed. A memory leak can occur for the duration of the fireAndForgetOperation() operation.

37. Is RxJava working good in combination with Kotlin?
Yes, they’re fully compatible. The built-in support for Lambdas and single abstract methods (SAM) makes RxJava streams even more concise and easy to read in Kotlin. There is also the RxKotlin library, which is a set of useful extension functions on top of RxJava.

38. What’s a marble diagram?
It’s a graphical representation of how RxJava operators work. In most cases it has a source stream, an operator and a resulting stream. Each stream is represented by a timeline with all emissions (“marbles”) and terminating events (completion / error).

39. What’s a “pure” function?
A pure function is one that doesn’t have any side effects and has stable output – e.g. the same input will always produce the same output. Working with pure functions makes code easier to reason, as there’s no hidden side effects and implicit dependencies between functions. Given the composable nature of RxJava operators, a very good combination is keeping each operation a highly isolated pure function – this way alterations of the stream are easier.

40. Do you know any other reactive libraries? What’s the biggest differences, pros, cons?
Even the Java8 language supports a few “reactive” constructs (CompletableFuture, Stream, etc). Other popular libraries are Project Reactor and Akka. Project Reactor is quite similar to RxJava2 in terms of constructs and API, but it targets Java8 which makes it unusable for some Android projects. A good high-level comparison between reactive libraries can be found HERE.
