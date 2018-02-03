Implement a Vector
==================
Until otherwise noted, just store :code:`int`.  Templates are coming, but
they'll complicate things for now.  To make your life simpler, add a type
alias (:code:`typedef` or :code:`using`) so there's less churn down the road.

If you have questions as to what these functions should do, check
cppreference_.


Phase 1
-------
Add a default constructor, :code:`push_back`, :code:`size`, and :code:`empty`.
Make sure you have some way to verify the items are being added correctly, but
it can be a visual inspection for now.


Phase 2
-------
Add support for :code:`operator []` and :code:`at`.  Update the visual
inspection from the previous step to rely on :code:`[]` or :code:`at`.  Better
yet, write tests to use both.

Remember that you'll need two forms of :code:`operator []` (:code:`const` and
non-:code:`const`).


Phase 3
-------
Add the copy and constructors, the assignment and move operators, and a
destructor.  The prototypes should look something like the following:

.. code:: c++

  Vector(Vector const &other); // copy ctor
  Vector(Vector &&other) noexcept; // move ctor
  Vector& operator = (Vector const &rhs); // assignment operator
  Vector& operator = (Vector &&rhs) noexcept; // move operator

Make sure your move constructor and operator leave the other :code:`Vector` in
a valid state so your destructor can work.  This is a good time to run your
tests with valgrind to verify you're not leaking memory (e.g., your destructor
works).


Phase 4
-------
Add support for reserving space.  Update the default constuctor and
:code:`push_back` to manage the new state.  After you have it working, add
:code:`capacity` and :code:`reserve`.

Think about the way to ensure sufficient storage is available.  If you add an
:code:`if` to :code:`push_back` it works, but eventually we're adding multiple
methods to add data; try to choose something that can be reused.


Phase 5
-------
Add iterator support:

- :code:`begin`
- :code:`cbegin`
- :code:`end`
- :code:`cend`
- :code:`rbegin`
- :code:`crbegin`
- :code:`rend`
- :code:`crend`

Don't worry about working with the standard algorithms yet, but you should
work with a C++-98 :code:`for`.


Phase 6
-------
Get your iterators working with the standard library.  This requires
specializing a `template class`_ and implementing a handful of functions.

Test your code with :code:`std::find`, :code:`std::rotate`, and
:code:`std::sort`.


Phase 7
-------
Add the remaining constructors.  The big one here is
:code:`std::initializer_list`, but add anything else you care about.


Phase 8
-------
Templatize :code:`Vector`.

Don't worry about supporting multiple types, just get your code working as
:code:`Vector<int>`.  To make your life simpler, liberally use :code:`#if 0`
to avoid attempting to compile all your code at once (both implementation and
test).


Phase 9
-------
Make your code work with other primitive types (:code:`double` is enough if
you're lazy about writing tests).  Once that code runs and passes tests, get
:code:`Vector<std::string>` working.


Phase 10
--------
Create a type that *doesn't* have a default constructor; make
:code:`Vector<Foo>` work.

Depending on how you've been implmenting things, this may result in a large
amount of churn.  You'll need to allocate raw memory (:code:`::operator new`
or :code:`malloc`) and placement :code:`new`.  You'll also need to explicitly
invoke destructors when releasing memory.

Sanitizers will really help during this section.


Phase 11
--------
Harden your code.  Make a new type that will :code:`throw` after so many have
been constructed, then make sure your code propagates the exception properly
without leaking resources.  This is especially important in any code that adds
multiple items at once; check your constructors and assignment operator.


.. _cppreference: http://en.cppreference.com/w/cpp/container/vector
.. _template class: http://en.cppreference.com/w/cpp/iterator/iterator_traits
