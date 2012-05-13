# pallet-thread

A library to simplify working with Java threads from clojure.

## Usage

To create a cached thread pool, and run something:

```clj
(use 'pallet.thread.executor)
(let [pool (executor {:prefix "mypool" :thread-group-name "mypool"})
      f (execute pool #(+ 1 2))]
  (println @f)
```

You can create a fixed size thread pool:

```clj
(use 'pallet.thread.executor)
(executor {:pool-size 2 :prefix "mypool" :thread-group-name "mypool"})
```

There are also scheduled executors:

```clj
(use 'pallet.thread.executor)
(with-executor [pool (executor {:prefix "mypool" :scheduled true :pool-size 1})]
  (let [_ (execute-after pool #(println "hello") 1000 :ms)
        _ (execute-after pool #(println "there") 2 :s)
        f1 (execute-fixed-rate pool #(println ".") 1 :s 2)
        f2 (execute-fixed-delay pool #(println "!") 2 :s 2)]
    (Thread/sleep 10000)
    (future-cancel f1)
    (future-cancel f2)))
```


[API docs](http://pallet.github.com/pallet-thread/0.1)

## Installation

To use pallet-thread, add the following to your :dependencies:

[pallet-thread "0.1.0"]

[![Build Status](https://secure.travis-ci.org/pallet/pallet-thread.png)](http://travis-ci.org/pallet/pallet-thread)

## License

Copyright © 2012 Hugo Duncan

Distributed under the Eclipse Public License.
