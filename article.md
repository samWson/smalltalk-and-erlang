# The Object Oriented Abstraction in Elixir

That's a bold statement. When you describe Elixir and its ancestor Erlang you use words like functional, concurrent, process oriented, actor based etc. Object Oriented is not in that list at all.

## Introduction  

This talk was prompted by a few things. Several years ago I started to learn about the Smalltalk programming language. The key underlying philosophy of Smalltalk is "everything is an object, and behavior happens by sending messages". Every now and again I have made an effort to learn functional programming. For me this has been slow incremental progress. At some point I learned of the Erlang programming language. I discovered that a key philosophy of Erlang is "everything is a process, and processes communicated by sending messages". With Smalltalk and Erlang seeming to share some key fundamental concepts I wondered if this was a way for me to finally understand functional programming. It wasn't quite that. Understanding functional programming would still take many slow incremental steps but I did learn a lot more along the way which shaped an idea in my mind. Processes as exemplified in Erlang are the next logical step in the Object Oriented paradigm as a programming language feature.

This talk is about a perspective, a point of view I have gained through my experiences. And maybe a minor rant. The purpose is to share a bit of history and give you something to think about.

About myself: The first language I knew enough to do something with was Java. Eventually I came accross the Smalltalk programming language. I work with Ruby for a living which is heavily inspired by Smalltalk. Smalltalk is the first Object Oriented programming language. I've invested a lot of personal time learning Smalltalk and studying its history to the point where I am now a bit of an Object Oriented purist/snob.

A caveat: I'm new to Elixir and Erlang but not Object Oriented Programming. If I say anything that's just plain wrong or misguided about Elixir or Erlang that is my own ignorance at work.

For this talk to make sense we first have to have a clear definition of Object Oriented Programming so you can understand where I'm coming from when I describe Elixir and Erlang. For that we have to go back to the source.

## What is Object Oriented Programming?

Smalltalk was created at Xerox PARC in the 1970s based on ideas gleaned from the Simula programming language and the visions of Alan Kay. Alan Kay was the person who came up with the term "Object Oriented". For me Alan Kays and the Smalltalk-80 definition of Smalltalk are canonical and Smalltalk was an attempt to implement Object Oriented as a paradigm at the language level.

### Smalltalk-80 Blue Book

In 1980 the first public release of Smalltalk was made, Smalltalk-80. The language, class library, and an implementation, were described in the Smalltalk-80 book, often called the **Blue Book**. I'm going to be referencing it a lot because it is Smalltalk and Object Oriented programming in the words of its creators.

The creators state that the concepts that Smalltalk uses have unusually terminology. Smalltalks uniform concept across the system is described as _object-message orientation_[1]. The vocabulary of a Smalltalk system is _object, message, class, instance, and method_[1]. While common today these words and concepts were unheard of back in the 1970's having been seen only in the Simula programming language before.

#### Communicating Objects

_Smalltalk is built on the model of communicating objects. Large applications are viewed in the same way as the fundamental units from which the system is built._[2]

Smalltalk is a pure object oriented language. The most fundamental parts of the language are objects: integers, strings, characters etc. Objects are composed together to make more complex objects, and abstractions are made out of the interactions between these objects.

Notice before in the Smalltalk vocabulary that _message_ and _method_ are two different things.

#### Objects are a Black Box

_A message is a request for an object to carry out one of its operations. A message specifies which operation is desired, but not how that operation should be carried out. The receiver, the object to which the message was sent, determines how to carry out the requested operation._[3]

An object is encapsulated. Another object can send a message to request an operation but the sender has no control over how that operation is carried out. Only the receiver of a message can run one of its own methods. Other objects can't _call_ methods. We can understand then that objects are independent of each other, their operations and state isolated from each other. Behavior happens in response to messages.

_insert Alan Kays definition of Object Oriented_
_touch on the Pharo object model_

summarize with a definition of what objects are. This will be used to compare with Elixir later_

#### Summarizing Object Oriented

An Object Oriented system is one where the system behavior is modeled as a network of communicating objects. The key characteristics of an Object Oriented system are:

##### Isolation

Objects do not share state. To an outsider they are a black box. One object will not know another objects internal behavior or state.

##### Message Passing

All behavior happens when an object sends a message to another object. A sender can tell another object to do a specific behavior with a known interface but the sender has no control over what happens once the message is received. The receiver of the message decides what to do on receipt of a message.

## How does Elixir fit in?

At the language level Elixir is not Object Oriented but functional instead. However the language and the Erlang virtual machine on which it runs has the model of communicating processes. The difference here is that while Smalltalk is objects all the way down to the simplest parts of the language Elixir is communicating processes only at the higher level of abstraction. At the module level the basic parts of the language are not processes.

I love exploring history. There is a lot to learn in programming history. Eventually I came across Erlang. Erlang is built along the Actor model. Actors are implemented as processes.

### Erlang

Joe Armstrong, one of the authors of Erlang uses the following words to describe Erlang:[6]
* Everything is a process.
* Processes are strongly isolated.
* Process creation and destruction is a lightweight operation.
* Message passing is the only way for processes to interact.
* Processes have unique names.
* If you know the name of a process you can send it a message.
* Processes share no resources.
* Error handling is non-local.
* Processes do what they are supposed to do or fail.

#### Concurrency Oriented Programming

_In our system concurrency plays a central role, so much so that I have coined the term Concurrency Oriented Programming to distinguish this style of programming from other programming styles._[7]

_Concurrency Oriented Programming also provides the two major advantages commonly associated with object-oriented programming. These are polymorphism and the use of defined protocols having the same message passing interface between instances of different process types._[7]

_In the real world sequential activities are a rarity. As we walk down the street we would be very surprised to find only one thing happening, we expect to encounter many simultaneous events._[8]

Joe Armstrong defines programming languages that have good support for concurrency at the language level as Concurrency Oriented Languages or COPL for short. In the case of Erlang this is provided by independently executing concurrent processes.

### Characteristics of a COPL

Joe Armstrong states that COPLs are characterized by the following six properties:[9]
1. COPLs must support processes. A process can be thought of as a self-contained virtual machine.
2. Several processes operating on the same machine must be strongly isolated. A fault in one process should not adversely affect another process, unless such interaction is explicitly programmed.
3. Each process must be identified by a unique unforgeable identifier.We will call this the Pid of the process.
4. There should be no shared state between processes. Processes interact by sending messages. If you know the Pid of a process then you can send a message to the process.
5. Message passing is assumed to be unreliable with no guarantee of delivery.
6. It should be possible for one process to detect failure in another process. We should also know the reason for failure.

### Processes

_Processes communicate by sending and receiving messages... Message sending is asynchronous and safe, the message is guaranteed to eventually reach the recipient, provided that the recipient exists._[5]

#### Summarizing Concurrency Oriented Programming

A Concurrency Oriented system is all about how processes work. System behavior is modeled as a network of processes. The characteristics of such a system are:

##### Isolation

Processes are strongly isolated. There is no shared state.

##### Message Passing

Processes can interact by sending messages to each other. If you know the PID of a process then you can send it a message. The sender has no control over how the receiver responds to the message.

It used to when I first started making things with Elixir I would turn to processes as an equivalent to objects. This equivalence is wrong for the most part. The difference is in the level of abstraction. Objects are the most fundamental element of Smalltalk programming but in Elixir modules it's functions and data. But when you start to introduce processes to model the behavior of your program then Elixir starts to resemble the message passing objects of an object oriented language.

### Where they Differ

We can see that objects and processes have key characteristics in common. Erlang took the concept further though.

Processes don't just isolate state they also isolate execution. When an object sends a message to another the thread of execution passes from the sender to the receiver while the behavior is carried out and then the thread of execution returns to the receiver. When a process sends a message to another process the senders thread of execution continues independently of the receiver. The sender may wait for a response from the receiver, in which case it is blocking, or it may continue asynchronously. Message passing between processes is assumed to be unreliable with no guarantee of delivery.

Collectively what this means is that processes are object oriented system made safe for concurrency and fault tolerance.

### Message Passing Objects and Processes

Here is a pair of counters one a Ruby object the other an Elixir process. They both increase and return a number each time they are sent the `increment` message.


```ruby
# counter.rb

class Counter
  def initialize
    @count = 0
  end

  def increment
    @count += 1
  end
end
```

```shell
$ irb -r ./counter.rb
.irbrc file loaded
irb(main):001:0> counter = Counter.new
=> #<Counter:0x00007fc73f162b60 @count=0>
irb(main):002:0> counter.increment
=> 1
irb(main):003:0> counter.increment
=> 2
irb(main):004:0> counter.increment
=> 3
irb(main):005:0> counter.increment
=> 4
```

This Elixir example is adapted from another example in the getting started guide.
```elixir
# counter.exs
defmodule Counter do
  @moduledoc """
  A simple counter that increases the number each time it receives and increment
  message.
  """

  def start_link do
    Task.start_link(fn -> loop(0) end) # Start a process with an initial state of 0.
  end

  defp loop(count) do
    receive do
      {:increment, caller} ->
        send caller, count + 1
        loop(count + 1)
    end
  end
end
```

```shell
$ iex counter.exs
iex(1)> {:ok, pid} = Counter.start_link
{:ok, #PID<0.109.0>}
iex(2)> send(pid, {:increment, self()})
{:increment, #PID<0.107.0>}
iex(3)> send(pid, {:increment, self()})
{:increment, #PID<0.107.0>}
iex(4)> send(pid, {:increment, self()})
{:increment, #PID<0.107.0>}
iex(5)> flush()
1
2
3
:ok
```

The syntax is different but overall the semantics are the same: a message is sent to an entity. As long as the entity understands the message then some behavior is triggered. The sender will also pass a reference to `self` so the receiver knows who to return the response to. It an object oriented language this is implicit.

### The Object Oriented Abstraction in Elixir

The previous section is a simple low level example but we almost never need to create this behavior ourself in Elixir. The `Agent` module is a simple abstraction that allows state to be retrieved and updated.

Agent is a part of OTP, a framework that Erlang is well known for and there are many more abstractions based on processes there too. GenServers a process that abstracts the server of a client-server relation. Tasks will typically execute one particular action throughout their lifetime.

So an Elixir application using OTP is likely to have many independent processes communicating by sending messages to each other. Processes are being supervised, killed, restarted, and spawned all the time.

_comparison of objects and messages_

A key point of difference is that Processes take the idea of encapsulation even further than objects. In a single thread of execution when a message is sent to another object that thread of execution will move from the sender to the receiver, into any operation, and back to the sender again as a result is returned. Processes are on their own threads of execution. When a process sends a message to another that thread of execution is totally isolated. Not only do processes isolate state, they also isolate threads of execution.

_OTP may be an example of object oriented design at the architecture level_

_other possible examples of object oriented programing e.g. the Internet_

_ending: does this even matter?_

A key thing I found is that the definition of Object Oriented Programming is that was never intended to be static, as in it's just something to be learned (I don't refer to static vs. dynamic programming systems). Object Oriented as a concept went through several iterations of Smalltalk internally at Xerox and evolved with each iteration.

With that in mind it's reasonable to take the idea of objects and messages an implement them in other novel ways, such as independent communicating processes.

### Message Oriented Programming?

This is my favorite quote from Alan Kay because it really says something important about Object Oriented as a model for system interaction:

_I'm sorry that I long ago coined the term "objects" for this topic because it gets many people to focus on the lesser idea. The big idea is "messaging"..._

OTP is a high level frame work that models the behavior of a program as message communicating processes. Smalltalk is a pure Object Oriented language. The language is objects all the way down and so too higher level frameworks must also be modeled on the idea of message sending objects.

The idea was that the program is all about the messages that need to be sent. It's not about which objects you need. So perhaps it should have been called **Message Oriented Programming**.

### Object Oriented is not just a Language Feature

While doing my research for this topic I came across more words from Alan Kay that got me thinking:

_Think of the internet -- to live, it (a) has to allow many different kinds of ideas and realizations that are beyond any single standard and (b) to allow varying degrees of safe interoperability between these ideas._

_If you focus on just messaging... then much of the language-, UI-, and OS based discussions on this thread are really quite moot._

_Smalltalk-80 never really was mutated into the next better versions of OOP. Given the current low state of programming in general, I think this is a real mistake._[4]

Object Oriented is not just a language feature. Message sending objects can be a model for behavior in frameworks, networks, or other systems. Smalltalk-80 and Erlang are a few years apart and OTP would come along much later. I have seen no direct lineage or shared knowledge in the development between the two systems but the developers of both thought the idea of message sending was worth using to solve their particular set of problems. It that last quote was anything to go by then we may see new implementations of communicating objects or improvements on the idea in the future.

When I was first learning to program I read a lot of heated opinions on how Object Oriented Programming was a mistake, a grand disaster, the cause of all our problems in software today. These same opinions would almost always evangelize that Functional Programming was the silver bullet, the one true way to write understandable error free software. This tribal war is still continuing among many others in programming.

Smalltalk-80, the first general release of the first Object Oriented Programing language was unveiled in 1980. In 1986 the Erlang programming language first appeared, only a few years between the two. In 1996 Ericsson would deliver the first prototype of the Open Telecom Protocol, a framework that would contain some Object Oriented abstractions built in a functional language.

I still see a lot of salt thrown around during the eternal tribal war of Object Oriented vs. Functional paradigms. Smalltalk was an implementation of the idea a system of message communicating objects. Erlang and OTP is an implementation of the idea of message communicating processes. When the idea is messages the implementation does not matter only the interface. We have likely not seen the end of object and message systems and they may have new names for what is already a familiar thing.

I found there are strong similarities to the ideas implemented in Smalltalk and Erlang but the different domain problems both languages were built for created very significant distinctions in how they behaved.

And those are my opinions. Maybe you think I'm right, maybe you think I am mad. But my interest in Smalltalk and the history of Object Oriented Programming led me to discovering Erlang and processes and by extension Elixir. So I got to learn some great languages, frameworks, and patterns along the way. I'm always happy to share what I've found.

## References
* [1] Smalltalk-80, viii Preface, Smalltalk has few concepts.  
* [2] Smalltalk-80, ix Preface, Smalltalk is a big system.
* [3] Smalltalk-80, pg. 6 Part 1, Objects and Messages.
* [4] [prototypes vs classes was: Re: Sun's HotSpot](http://lists.squeakfoundation.org/pipermail/squeak-dev/1998-October/017019.html)
* [5] [Erlang -- Processes](https://erlang.org/doc/reference_manual/processes.html#message-sending)
* [6] [Making reliable distributed systems in the presence of software errors, pg 39, chapter 3.1, Erlang](http://erlang.org/download/armstrong_thesis_2003.pdf)
* [7] [Making reliable distributed systems in the presence of software errors, pg 19, chaper 2.4 Concurrency oriented programming](http://erlang.org/download/armstrong_thesis_2003.pdf)
* [8] [Making reliable distributed systems in the presence of software errors, pg 20, chaper 2.4 Concurrency oriented programming](http://erlang.org/download/armstrong_thesis_2003.pdf)
* [9] [Making reliable distributed systems in the presence of software errors, pg 22, chapter 2.4.2 Characteristics of a COPL](http://erlang.org/download/armstrong_thesis_2003.pdf)
