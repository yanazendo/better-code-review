When I worked at Twitter I often received the complaint that my code was too sophisticated and my colleagues couldn't review it. That was definitely bullshit. They *could* review my code -- they were just being lazy.

But software engineers are supposed to be lazy, so we just have to accept and enjoy laziness.

Although I did my best to present my code well, no one ever reviewed it, so, eventually, I quit.

Today, I believe I've solved this problem with *Better Code Review*, a better code-review tool that makes it very enjoyable for lazy engineers to review sophisticated code.

So, next time your reviewer complains that your code is too sophisticated, don't quit your job. Just write your documentation in better-code-review format and continue making big money.

The big idea behind Better Code Review is to present code and its documentation in [Sidenote format](##sidenote).

Check out this example: [pyalgebra.py](##pyalgebra)

I am using a novel [engineering methodology](##development-method) to develop this software.

Maintainer: <a href="http://yanazendo.org">Yana Zendo</a>

~development-method
## Engineering methodology

I am developing Better Code Review using a novel engineering methodology based on my personal laziness:

1. I publish the specifications for a project on github
2. Sufficiently inspired developers submit pull requests and we collaborate.
    - If you would like me to pay you for your pull request, don't send it right away. Rather, file an issue explaining your pull request and we can negotiate compensation from there.

The specs for Better Code review are located [here](http://github.com).

~sidenote
## Sidenote

The documentation you are reading right now is presented in *Sidenote format*. Sidenote is awesome. I hope its awesomeness is intuively obvious to you.

Here are some example Sidenote documents:

- [Sidenote.io](http://sidenote.io), the homepage for Sidenote
- [Beer Garden](http://mikegagnon.com/beergarden), a well-tested system for defending web applications against *high-density* attacks
- [Ultra Rationality And The Prisoner's Dilemma](http://mikegagnon.com/ultra-rationality/), a very sketchy proof that selflessness is a winning strategy
- [FAQ: What You Need to Know About the NSA's Surveillance Programs
](http://mikegagnon.com/nsa-faq), an excellent document explaining aspects of NSA's surveillance programs

~pyalgebra
## Pyalgebra.py

Download <a href="https://raw.githubusercontent.com/mikegagnon/pyalgebra/master/pyalgebra.py">pyalgebra.py.</a>

<!--
HACK: I generated the HTML below by opening pyalgebra.html in Chrome, then inspecting the document, and copying the dynamically generated HTML into here. Then I manually added the hyperlinks.
-->

<pre>


<code data-language="python" class="rainbow"><a href="
javascript:Sidenote.openColumnLoud('#pyalgebra','#pya-overview','Overview')" style="color: inherit; text-decoration: inherit"># Overview</a>
<a href="
javascript:Sidenote.openColumnLoud('#pyalgebra','#pya-monoids','__MONOIDS__')" style="color: inherit; text-decoration: inherit">
__MONOIDS__ = {}</a>

<a href="
javascript:Sidenote.openColumnLoud('#pyalgebra','#pya-getMonoid','getMonoid')" style="color: inherit; text-decoration: inherit"><span class="storage function">def</span> <span class="entity name function">getMonoid</span>(x, y = <span class="constant language">None</span>, monoids = __MONOIDS__):
    if y == <span class="constant language">None</span>:
        typ = <span class="support function python">type</span>(x)
        if typ <span class="keyword">not</span> <span class="keyword">in</span> monoids:
            <span class="keyword">raise</span> ValueError("%s does <span class="keyword">not</span> have an associated monoid" % x)
        return monoids[typ]
    else:
        xMonoid = getMonoid(x, <span class="constant language">None</span>, monoids)
        yMonoid = getMonoid(y, <span class="constant language">None</span>, monoids)
        if xMonoid != yMonoid:
            xMonStr = <span class="support function python">str</span>(x)[:80]
            yMonStr = <span class="support function python">str</span>(y)[:80]
            <span class="keyword">raise</span> ValueError(("Cannot get monoid for \n%s\n---and---\n%s\n" +
                "because they don't have the same monoid") % (xMonStr, yMonStr))
        return xMonoid
</a>
<a href="
javascript:Sidenote.openColumnLoud('#pyalgebra','#pya-class-monoid','class Monoid')" style="color: inherit; text-decoration: inherit">
<span class="storage class">class</span> <span class="entity name class">Monoid</span>(<span class="entity other inherited-class">object</span>):
    <span class="storage function">def</span> <span class="entity name function">isNonZero</span>(<span class="variable self">self</span>, v):
        return <span class="variable self">self</span>.zero() != v
</a>
<span class="storage class">class</span> <span class="entity name class">IntMonoid</span>(<span class="entity other inherited-class">Monoid</span>):
    <span class="storage function">def</span> <span class="entity name function">zero</span>(<span class="variable self">self</span>): return 0
    <span class="storage function">def</span> <span class="entity name function">plus</span>(<span class="variable self">self</span>, x, y, monoids = <span class="constant language">None</span>): return x + y
__MONOIDS__[int] = IntMonoid()

# Warning: plus modifies either x or y (whichever <span class="keyword">is</span> bigger)
<span class="storage class">class</span> <span class="entity name class">DictMonoid</span>(<span class="entity other inherited-class">Monoid</span>):
    <span class="storage function">def</span> <span class="entity name function">zero</span>(<span class="variable self">self</span>): return {}
    <span class="storage function">def</span> <span class="entity name function">plus</span>(<span class="variable self">self</span>, x, y, monoids = __MONOIDS__):
        if <span class="support function python">len</span>(x) &gt; <span class="support function python">len</span>(y):
            bigger, smaller = x, y
            bigOnLeft = <span class="constant language">True</span>
        else:
            bigger, smaller = y, x
            bigOnLeft = <span class="constant language">False</span>
        for k, v <span class="keyword">in</span> smaller.iteritems():
            if k <span class="keyword">not</span> <span class="keyword">in</span> bigger:
                bigger[k] = v
            else:
                if bigOnLeft:
                    left = bigger[k]
                    right = v
                else:
                    left = v
                    right = bigger[k]

                subMonoid = getMonoid(left, right, monoids)
                newV = subMonoid.plus(left, right, monoids)
                if subMonoid.isNonZero(newV):
                    bigger[k] = newV
                else:
                    <span class="keyword">del</span> bigger[k]
        return bigger
__MONOIDS__[dict] = DictMonoid()

<span class="storage class">class</span> <span class="entity name class">ListMonoid</span>(<span class="entity other inherited-class">Monoid</span>):
    <span class="storage function">def</span> <span class="entity name function">zero</span>(<span class="variable self">self</span>): return []
    <span class="storage function">def</span> <span class="entity name function">plus</span>(<span class="variable self">self</span>, x, y, monoids = <span class="constant language">None</span>): return x + y
__MONOIDS__[list] = ListMonoid()

<span class="storage class">class</span> <span class="entity name class">SetMonoid</span>(<span class="entity other inherited-class">Monoid</span>):
    <span class="storage function">def</span> <span class="entity name function">zero</span>(<span class="variable self">self</span>): return <span class="support function python">set</span>()
    <span class="storage function">def</span> <span class="entity name function">plus</span>(<span class="variable self">self</span>, x, y, monoids = <span class="constant language">None</span>): return x | y
__MONOIDS__[set] = SetMonoid()

<span class="storage function">def</span> <span class="entity name function">plus</span>(x, y, monoids = __MONOIDS__):
    monoid = getMonoid(x, y, monoids)
    return monoid.plus(x, y, monoids)

<span class="storage function">def</span> <span class="entity name function">total</span>(items, monoids = __MONOIDS__):
    if <span class="support function python">len</span>(items) == 0:
        return <span class="constant language">None</span>
    else:
        total = items[0]
        for item <span class="keyword">in</span> items[1:]:
            total = plus(total, item, monoids)
        return total

    
if <span class="support magic">__name__</span> == "__main__":
    assert 3 == plus(1, 2)
    assert [1,2,3] == plus([1], [2, 3])
    assert <span class="support function python">set</span>([1,2,3]) == plus(<span class="support function python">set</span>([1, 2]), <span class="support function python">set</span>([2, 3]))
    assert {"a": 3} == plus({"a": 1}, {"a": 2})
    assert {"a": 3, 0:1} == plus({"a": 1, 0:1}, {"a": 2})
    assert {"a": 3, 0:1} == plus({"a": 1}, {"a": 2, 0:1})
    assert {"a": [1,2,3], "b": 1} == plus(
        {
            "a": [1,2],
            "b": 1
        },
        {
            "a": [3]
        })
    assert {"a": [1,2,3], "b": 1} == plus(
        {
            "a": [1],
        },
        {
            "a": [2,3],
            "b": 1
        })

    left = {
        "a": 1,
        "b": ["x", "y", "z"],
        "c": {
            "a": <span class="support function python">set</span>([1,2,3]),
            "b": 1
        },
        "q": 3,
        "x": 5
    }

    right = {
        "a": 3,
        "b": [1,2,3],
        "c": {
            "a": <span class="support function python">set</span>([1,2,5,6]),
            "b": 7,
            "d": 3
        },
        "p": 5,
        "x": -5
    }

    combined = {
        "a": 4,
        "b": ["x", "y", "z", 1, 2, 3],
        "c": {
            "a": <span class="support function python">set</span>([1,2,3,5,6]),
            "b": 8,
            "d": 3
        },
        "q": 3,
        "p": 5
    }

    assert plus(left, right) == combined

    assert <span class="constant language">None</span> == total([])
    assert 1 == total([1])
    assert [1,2,3,4,5] == total([[1], [2,3], [4,5]])

    import copy
    monoids = copy.deepcopy(__MONOIDS__)
    <span class="storage class">class</span> <span class="entity name class">MockIntMonoid</span>(<span class="entity other inherited-class">Monoid</span>):
        <span class="storage function">def</span> <span class="entity name function">zero</span>(<span class="variable self">self</span>): return 0
        <span class="storage function">def</span> <span class="entity name function">plus</span>(<span class="variable self">self</span>, x, y, monoids = <span class="constant language">None</span>): return x - y
    monoids[int] = MockIntMonoid()
    assert {"a": 3} == plus({"a": 1}, {"a": 2})
    assert {"a": -1} == plus({"a": 1}, {"a": 2}, monoids)

</code>
</pre>

<link rel="stylesheet" type="text/css" href="css/solarized-light.css">
<script src="js/rainbow.js"></script>
<script src="js/python.js"></script>

~pya-overview
<pre><code data-language="python" class="rainbow"># Overview</code></pre>

pyalgebra.py shows off how simple monoids are in Python.

### Note

Read [this explanation](##pya-monoids-general) if you're not already familiar with the concept of *monoids* in general.

~pya-monoids
<pre><code data-language="python" class="rainbow">__MONOIDS__</code></pre>

<tt data-language="python" class="rainbow">`__MONOIDS__`</tt> is a dict that maps each supported datatype to an instance of the monoid for that datatype.

### Note

Read [this explanation](##pya-monoids-general) if you're not already familiar with the concept of *monoids* in general.

~pya-monoids-general
## Monoids

Monoids are one of the most beautiful abstractions in the history of *The Human Understanding of Computation*.
Monoids are great because they allow you to eloquently and easily express your wildest computations. As a consequence of eloquence and ease, you are more likely to have fun while writing correct code. Monoid computations also tend to be efficient.

Monoids are win, win, win all around.

Monoids used to be very estoric. But a relatively small community of hackers at various institutions teamed up and began releasing libraries that took monoids from theory into mainstream software engineering.

In Silicon Valley fashion, you are pretty hip if you know monoids. [Checkout my cool friends](http://www.wired.com/2013/11/twitter-summingbird/).

A lot of people think that monoids are super complicated, but, in fact, they are super simple. Monoids only seem complicated because discussions of monoids usually occur at unfamiliar levels of abstraction. Also, incumbent hipsters are incentivized to keep monoids esoteric.

I am betraying my hipster friends here and making monoids very simple in this documentation.

### So, what are monoids?

*Monoids* are the things that allow Python to perform the plus computations in these [examples](##pya-examples). You're smart, so study those examples and understand monoids.

### How monoids work in pyalgebra.py

Every data type has a different monoid implementation.

For example, I have implemented monoids for:

- [Integers](##pya-int)
- [Lists](##pya-list)
- [Sets](##pya-set)
- [Dictionaries](##pya-dict)

As you can see, in pyalgebra a monoid is just a class that implements sensible <code data-language="python" class="rainbow">zero</code> and <code data-language="python" class="rainbow">plus</code> methods.

All monoids subclass the [Monoid class](##pya-class-monoid).

### What's next?

That's all there is to know about monoids.

So now you should learn about [type classes](https://github.com/mikegagnon/type_class_tech_talk/blob/master/README.md) and become even more hip. Plus you'll need to understand type classes if you want to understand my other projects.

~pya-int
## Integers

<pre><code data-language="python" class="rainbow"><span class="storage class">class</span> <span class="entity name class">IntMonoid</span>(<span class="entity other inherited-class">Monoid</span>):
    <span class="storage function">def</span> <span class="entity name function">zero</span>(<span class="variable self">self</span>): return 0
    <span class="storage function">def</span> <span class="entity name function">Plus</span>(<span class="variable self">self</span>, x, y, monoids = <span class="constant language">None</span>): return x + y
__MONOIDS__[int] = IntMonoid()
</code></pre>

~pya-list
## Lists

<pre><code data-language="python" class="rainbow"><span class="storage class">class</span> <span class="entity name class">ListMonoid</span>(<span class="entity other inherited-class">Monoid</span>):
    <span class="storage function">def</span> <span class="entity name function">zero</span>(<span class="variable self">self</span>): return []
    <span class="storage function">def</span> <span class="entity name function">plus</span>(<span class="variable self">self</span>, x, y, monoids = <span class="constant language">None</span>): return x + y
__MONOIDS__[list] = ListMonoid()
</code></pre>

~pya-set
## Sets

<pre><code data-language="python" class="rainbow"><span class="storage class">class</span> <span class="entity name class">SetMonoid</span>(<span class="entity other inherited-class">Monoid</span>):
    <span class="storage function">def</span> <span class="entity name function">zero</span>(<span class="variable self">self</span>): return <span class="support function python">set</span>()
    <span class="storage function">def</span> <span class="entity name function">plus</span>(<span class="variable self">self</span>, x, y, monoids = <span class="constant language">None</span>): return x | y
__MONOIDS__[set] = SetMonoid()
</code></pre>

~pya-dict
## Dictionaries

<pre><code data-language="python" class="rainbow"># Warning: plus modifies either x or y (whichever <span class="keyword">is</span> bigger)
<span class="storage class">class</span> <span class="entity name class">DictMonoid</span>(<span class="entity other inherited-class">Monoid</span>):
    <span class="storage function">def</span> <span class="entity name function">zero</span>(<span class="variable self">self</span>): return {}
    <span class="storage function">def</span> <span class="entity name function">plus</span>(<span class="variable self">self</span>, x, y, monoids = __MONOIDS__):
        if <span class="support function python">len</span>(x) &gt; <span class="support function python">len</span>(y):
            bigger, smaller = x, y
            bigOnLeft = <span class="constant language">True</span>
        else:
            bigger, smaller = y, x
            bigOnLeft = <span class="constant language">False</span>
        for k, v <span class="keyword">in</span> smaller.iteritems():
            if k <span class="keyword">not</span> <span class="keyword">in</span> bigger:
                bigger[k] = v
            else:
                if bigOnLeft:
                    left = bigger[k]
                    right = v
                else:
                    left = v
                    right = bigger[k]

                subMonoid = getMonoid(left, right, monoids)
                newV = subMonoid.plus(left, right, monoids)
                if subMonoid.isNonZero(newV):
                    bigger[k] = newV
                else:
                    <span class="keyword">del</span> bigger[k]
        return bigger
__MONOIDS__[dict] = DictMonoid()
</code></pre>

~pya-examples
## Examples


<pre><code data-language="python" class="rainbow"><a href="
javascript:Sidenote.openColumnLoud('#pya-examples','#pya-examples-comment','Dummy Comment')" style="color: inherit; text-decoration: inherit">assert 3 == plus(1, 2)
assert [1,2,3] == plus([1], [2, 3])
assert <span class="support function python">set</span>([1,2,3]) == plus(<span class="support function python">set</span>([1, 2]), <span class="support function python">set</span>([2, 3]))
assert {"a": 3} == plus({"a": 1}, {"a": 2})
assert {"a": 3, 0:1} == plus({"a": 1, 0:1}, {"a": 2})
assert {"a": 3, 0:1} == plus({"a": 1}, {"a": 2, 0:1})
assert {"a": [1,2,3], "b": 1} == plus(
    {
        "a": [1,2],
        "b": 1
    },
    {
        "a": [3]
    })
assert {"a": [1,2,3], "b": 1} == plus(
    {
        "a": [1],
    },
    {
        "a": [2,3],
        "b": 1
    })

left = {
    "a": 1,
    "b": ["x", "y", "z"],
    "c": {
        "a": <span class="support function python">set</span>([1,2,3]),
        "b": 1
    },
    "q": 3,
    "x": 5
}

right = {
    "a": 3,
    "b": [1,2,3],
    "c": {
        "a": <span class="support function python">set</span>([1,2,5,6]),
        "b": 7,
        "d": 3
    },
    "p": 5,
    "x": -5
}

combined = {
    "a": 4,
    "b": ["x", "y", "z", 1, 2, 3],
    "c": {
        "a": <span class="support function python">set</span>([1,2,3,5,6]),
        "b": 8,
        "d": 3
    },
    "q": 3,
    "p": 5
}

assert plus(left, right) == combined

assert <span class="constant language">None</span> == total([])
assert 1 == total([1])
assert [1,2,3,4,5] == total([[1], [2,3], [4,5]])
</a>
</code></pre>

~pya-examples-comment
## Dummy Comment

This code doesn't actually require a comment.

I just did this to show off Better Code Review's ability to handle recursion.

Isn't that cool? :-)

~pya-getMonoid
<pre><code data-language="python" class="rainbow"><span class="storage function">def</span> <span class="entity name function">getMonoid</span>(x, y = <span class="constant language">None</span>, monoids = __MONOIDS__):</code></pre>

### Returns

the monoid for the datatype <code data-language="python" class="rainbow">x</code>.

### Arguments

* <code data-language="python" class="rainbow">x</code> is a datatype such as <code data-language="python" class="rainbow">int</code>, <code data-language="python" class="rainbow">list</code>, or <code data-language="python" class="rainbow">dict</code>
* <code data-language="python" class="rainbow">y</code> is either <code data-language="python" class="rainbow">None</code> or a datatype such as <code data-language="python" class="rainbow">int</code>, <code data-language="python" class="rainbow">list</code>, or <code data-language="python" class="rainbow">dict</code>.
    * If <code data-language="python" class="rainbow">y</code> is not <code data-language="python" class="rainbow">None</code> then it should be equivalent to <code data-language="python" class="rainbow">x</code>. To be precise, equivalence here means that the monoid associated with <code data-language="python" class="rainbow">y</code> should be the same as the monoid associated with <code data-language="python" class="rainbow">x</code>.

### Note

Read [this explanation](##pya-monoids-general) if you're not already familiar with the concept of *monoids* in general.

~pya-class-monoid

<pre><code data-language="python" class="rainbow"><span class="storage class">class</span> <span class="entity name class">Monoid</span>(<span class="entity other inherited-class">object</span>):</code></pre>

<code data-language="python" class="rainbow">Monoid</code> is an abstract base class.

Concrete subclasses should implement sensible <code data-language="python" class="rainbow">zero</code> and <code data-language="python" class="rainbow">plus</code> methods for a specific datatype.

The methods are considered sensible if, and only if:

1. For any <code data-language="python" class="rainbow">x</code>, <code data-language="python" class="rainbow">y</code>, and <code data-language="python" class="rainbow">z</code> from the datatype, <pre><code data-language="python" class="rainbow">plus(plus(x, y), z) == plus(x, plus(y, z))</code></pre>
2. For any <code data-language="python" class="rainbow">x</code> from the datatype, <pre><code data-language="python" class="rainbow">plus(zero(), x) == plus(x, zero()) == x</code> </pre>
### Concrete subclasses

- [IntMonoid](##pya-int)
- [ListMonoid](##pya-list)
- [SetMonoid](##pya-set)
- [DictMonoid](##pya-dict)


### Note

Read [this explanation](##pya-monoids-general) if you're not already familiar with the concept of *monoids* in general.