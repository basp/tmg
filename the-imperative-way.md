# The Imperative Way
- Bas Pennings
- meticulous
- 2013/10/07
- Programming
- published

`WORK IN PROGRESS`

Let me start of by saying that I usually dislike imperative programming because there is just too much of it. To be more exact: there is too much of it in the wrong places. There is nothing inheritly wrong with imperative programming but for us programmers it is often the only paradigm we know. Or the easiest one to apply given a pressure situation. 

If we are aware of other paradigms, then we often don't know how to apply them or when. I'm greatly saddened by this situation and with this post want to clear up a few things for myself and other people. If at the end of this (probably kinda longish) text you start to feel guilty (or maybe even dirty or even better: ashamed) about doing imperative programming when you have alternatives then I will have accomplished my goal. Just in case you are not going to reach that far please consider this:

> For a lot of us, imperative programming is a privilege we may not have for much longer.

I'm not joking. If you wan't to find out why exactly the above is true you'll just have to soldier on through the text (hint: the free ride is over).

### Hindsight is 20/20
Now that I almost finished up this post I can say that writing it was suprising. This post was supposed to be quick touch-up of something I wrote a few years back. In that post I was much more vile and evil spirited and in the end I ended up rewriting most of the original matter. Some of the hooks remain but the wording is mostly different and much more benign.

The last version of this matter was full of anger, hatred and pointing fingers. This one is to set my own toughts straight on how I feel nowadays. For one, I'm not pointing fingers - I'm one of these people that writes way too much imperative code just because it's so easy to do. Even when I know there are alternatives.

### Imperialism
According to global consensus, here's a definition of imperialism by the **Dictionary of Human Geography** (whatever that is, I can only hope it is somewhat trustworthy):

> [Imperialism is] an unequal human and territorial relationship, usually in the form of an empire, based on ideas of superiority and practices of dominance, and involving the extension of authority and control of one state or people over another.

This might seem like a good deal for the imperialist and it usually is (at first). According to the definition above, it's all about feeling superior to and controlling others. If you think about it, this is a bit like what we are doing to our environments when we are coding in an imperative way - we exert control over our machine or environment by telling it exactly how it should do its work.

Control might seem fun, you get all the power. Unfortunately, we end up getting all the responsibilities too and that's where the fun stops. Like with the huge imperialistic empires of history, eventually they will fall apart because of control loss. It will be slow at first, just here and there, but can quickly spread and fuel its own acceleration in the process We must not let our code suffer the same fate. 

The above sounds a lot like *broken windows* and it probably is comparable. If the analogy is correct, it would suggest that big scale imperative programming is eventually to fail in some whay or another. This begs the question, how scalable is imperative programming anyway?

### Imperative Programming
Imperative programming is sometimes required, often convenient and sometimes even the clearest way to express a solution. As such, it doesn't look like a big problem but it seems that for a large part of the general programmer populace it's the only way. This is not good.

Imperative programming is telling the machine or environment **how** to do it's job. This in contrast to telling it **what** to do. This is not favorable because it often hides intent and tends to lead to mutable state programming as a (pun not intended) side-effect.

I feel that instead of being the norm, imperative programming should be regarded as the assembly language of modern development. It is especially suited for doing small contained stuff in fast (and usually very easy to read) way. It is less useful to express big ideas. Orthogonally, when your solution space is very static (like a machine) then, imperative programming is usually more *safe* than when your space is dynamic (usually anything involving other non-programmer people). 

To illustrate my main beef with imperative programming I'm gonna conjure up a very stupid problem: write a function `get_init_vector` with the following specifications:

The function `get_init_vector` takes one argument named `count` of type `int`. It returns a `vector` (array) of `int`. As part of the contract, the length of the result vector will be equal to `count`. The result vector will contain pseudorandom integers in the range of greater than or equal to zero and smaller than ten. 

Let's look at the imperative solution:

	def get_init_vector(count):
		vector = []
		for i in range(count):
			x = random.randrange(10)
			vector.push(x)
		return vector

This solution works, there is nothing really wrong with it but Pythagonistas are probably cringing and rightfully so. It *should* be written like this (or more or less like it, I'm not a Pythagonista really):

	def get_init_vector(count):
		[ random.randrange(10) for x in range(3) ]

That would be more functional and more true to the spirit of Python. So how's this better to the imperative solution above? Well, at least we don't **introduce** mutable state. The second implementation has no state - it's just a declaration. Also it's shorter, that's usually a plus too. Now, is it easier to read? Probably not. That's a pity and a huge one too. And now we are getting to some interresting stuff which I actually didn't plan for when I started this example.

### Better Defined
When I say I think *this something* is better than *that something* it's probably fair to more clearly define what is meant by *better*. What is better? Well of course the answer is: `it depends` but there is this document... This small piece of wisdom: [PEP 20](http://www.python.org/dev/peps/pep-0020/ "The Zen of Python") that we can turn to:

#### The Zen of Python

	Beautiful is better than ugly.
    Explicit is better than implicit.
    Simple is better than complex.
    Complex is better than complicated.
    Flat is better than nested.
    Sparse is better than dense.
    Readability counts.
    Special cases aren't special enough to break the rules.
    Although practicality beats purity.
    Errors should never pass silently.
    Unless explicitly silenced.
    In the face of ambiguity, refuse the temptation to guess.
    There should be one-- and preferably only one --obvious way to do it.
    Although that way may not be obvious at first unless you're Dutch.
    Now is better than never.
    Although never is often better than *right* now.
    If the implementation is hard to explain, it's a bad idea.
    If the implementation is easy to explain, it may be a good idea.
    Namespaces are one honking great idea -- let's do more of those!

Those ideas basically sum up the programmer ethos. It's not just for Python, this should be the first thing any programmer learns. It's the **Zen of Programming** and it probably deserves a little bit more evangelism.

### The Free Ride

### Aaaaaaaand.... It's gone
