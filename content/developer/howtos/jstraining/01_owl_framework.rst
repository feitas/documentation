.. _howto/jstraining/01_owl_framework:

===========================
Chapter 1: Owl Framework 🦉
===========================

Components are the basic UI building blocks in Odoo. Odoo components are made with the
`Owl framework <https://github.com/odoo/owl>`_, which is a component system custom
made for Odoo.

Let us take some time to get used to Owl itself. The exercises in this section may be artificial,
but their purpose is to understand and practice the basic notions of Owl quickly.

Here is an overview of what we will do in this chapter:

.. image:: 01_owl_framework/overview_01.png
   :align: center
   :alt: Overview of chapter 01

A Counter Component
===================

Let us see first how to create a sub component

- Extract the counter code from the ``AwesomeDashboard`` component into a new ``Counter`` component.
- You can do it in the same file first, but once it's done, update your code to move the ``Counter``
  in its own file.
- Make sure the template is in its own file, with the same name.

.. warning:: Don't forget the ``/** @odoo-module **/`` in your javascript files, more information
    on this can be found :ref:`here <frontend/modules/native_js>`.

.. note:: References:

    - `owl: github repository <https://github.com/odoo/owl>`_
    - `owl: documentation <https://github.com/odoo/owl#documentation>`_
    - `owl: using sub components <https://github.com/odoo/owl/blob/master/doc/reference/component.md#sub-components>`_
    - :ref:`odoo: assets documentation <reference/assets>`
    

.. spoiler:: Preview

    .. image:: 01_owl_framework/counter.png
       :align: center
       :alt: A counter component


.. spoiler:: Solution

    - `Solution of the exercice can be found here <https://github.com/ged-odoo/odoo-js-training-public/commit/efd7bdbf6f12abd44479de6de5ae96525649d925>`_

A Todo Component
================

We will modify the AwesomeDashboard component to keep track of a list of todos. This will be done
incrementally in multiple exercises, that will introduce various concepts.


First, let's create a ``Todo`` component that display a task, which is described by an
id (number), a description (string) and a status done (boolean). For example:

    .. code-block:: javascript

        { id: 3, description: "buy milk", done: false }

- Create a ``Todo`` component that receive a ``todo`` in props, and display it: it should show
  something like ``3. buy milk``
- Add the bootstrap classes ``text-muted`` and ``text-decoration-line-through`` on the task
  if it is done
- modify ``AwesomeDashboard`` to display a ``Todo component``, with some hardcoded props to
  test it first. For example

    .. code-block:: javascript

        setup() {
            ...
            this.todo = { id: 3, description: "buy milk", done: false };
        }

.. note:: References:

    - `owl: props <https://github.com/odoo/owl/blob/master/doc/reference/props.md>`_
    - `owl: Dynamic attributes <https://github.com/odoo/owl/blob/master/doc/reference/templates.md#dynamic-attributes>`_
    - `owl: Dynamic class attributes <https://github.com/odoo/owl/blob/master/doc/reference/templates.md#dynamic-class-attribute>`_

.. spoiler:: Preview

    .. image:: 01_owl_framework/todo.png
       :align: center
       :alt: A Todo component


.. spoiler:: Solution

    - `Solution of the exercice can be found here (TODO) <https://github.com/ged-odoo/odoo-js-training-public/commit/efd7bdbf6f12abd44479de6de5ae96525649d925>`_

Props Validation
================

The Todo component has an implicit API: it expects to receive in its props the description of a
todo in a specified format: `id`, `description` and `done`. Let us make that API more explicit:
we can add a props definition that will let Owl perform a validation step in dev mode.
It is a good practice to do that for every component.

- Add `props validation <https://github.com/odoo/owl/blob/master/doc/reference/props.md#props-validation>`_ to ``Todo``
- Make sure it fails in dev mode

.. note:: References:

    - `owl: props validation <https://github.com/odoo/owl/blob/master/doc/reference/props.md#props-validation>`_
    - :ref:`odoo: debug mode <frontend/framework/debug_mode>`
    - :ref:`odoo: activate debug mode <developer-mode>`

.. spoiler:: Solution

    - `Solution of the exercice can be found here (TODO) <https://github.com/ged-odoo/odoo-js-training-public/commit/efd7bdbf6f12abd44479de6de5ae96525649d925>`_

A List of todos
===============

Now, let us display a list of todos instead of just one todo. For now, we can
still hardcode the list.

- Change the code to display a list of todos, instead of just one, and use
  `t-foreach` in the template
- Think about how it should be keyed

.. note:: References:

    - `owl: t-foreach <https://github.com/odoo/owl/blob/master/doc/reference/templates.md#loops>`_

.. spoiler:: Preview

    .. image:: 01_owl_framework/todoList.png
       :align: center
       :alt: A TodoList


.. spoiler:: Solution

    - `Solution of the exercice can be found here (TODO) <https://github.com/ged-odoo/odoo-js-training-public/commit/efd7bdbf6f12abd44479de6de5ae96525649d925>`_

Adding a todo
=============

So far, the todos in our list are hardcoded. Let us make it more useful by allowing the user to add
a todo to the list.

- Add input above the task list with placeholder ``Enter a new task``
- Add an event handler on the ``keyup`` event named ``addTodo``
- Implement ``addTodo`` to check if enter was pressed (``ev.keyCode === 13``), and
  in that case, create a new todo with the current content of the input as description
- Make sure it has a unique id (it can be just a counter that increments at each todo)
- Then clear the input of all content
- Bonus point: don't do anything if input is empty

Notice that nothing updates in the UI: this is because Owl does not know that it
should update the UI. This can be fixed by wrapping the todo list in a `useState`

.. code-block:: javascript

    this.todos = useState([]);

.. note:: References:

    - `owl: reactivity <https://github.com/odoo/owl/blob/master/doc/reference/reactivity.md>`_
    - `owl: event handling <https://github.com/odoo/owl/blob/master/doc/reference/event_handling.md>`_

.. spoiler:: Preview

    .. image:: 01_owl_framework/createTodo.png
       :align: center
       :alt: Creating todos

.. spoiler:: Solution

    - `Solution of the exercice can be found here (TODO) <https://github.com/ged-odoo/odoo-js-training-public/commit/efd7bdbf6f12abd44479de6de5ae96525649d925>`_

Focusing the input
==================

Let's see how we can access the DOM with `t-ref <https://github.com/odoo/owl/blob/master/doc/reference/refs.md>`. 

- Focus the ``input`` from the previous exercise whenever the dashboard is mounted.
- Bonus point: extract the code into a specialized hook ``useAutofocus``

.. note:: References:

    - `owl: component lifecycle <https://github.com/odoo/owl/blob/master/doc/reference/component.md#lifecycle>`_
    - `owl: hooks <https://github.com/odoo/owl/blob/master/doc/reference/hooks.md>`_
    - `owl: useRef <https://github.com/odoo/owl/blob/master/doc/reference/hooks.md#useref>`_

.. spoiler:: Solution

    - `Solution of the exercice can be found here (TODO) <https://github.com/ged-odoo/odoo-js-training-public/commit/efd7bdbf6f12abd44479de6de5ae96525649d925>`_


Toggling todos
==============

Now, let's add a new feature: mark a todo as completed. This is actually
trickier than one might think: the owner of the state is not the same as the
component that displays it. So, the `Todo` component need to communicate to its
parent that the todo state needs to be toggled. One classic way to do this is
by using a callback prop `toggleState`

- Add an input of ``type="checkbox"`` before the id of the task, which is checked if
  the ``done`` state is true
- Add a callback props ``toggleState``
- Add a ``click`` event handler on the input in ``Todo``, and make sure it calls
  the `toggleState` function with the todo id.
- Make it work!

.. note:: References:

    - `owl: binding function props <https://github.com/odoo/owl/blob/master/doc/reference/props.md#binding-function-props>`_

.. spoiler:: Preview

    .. image:: 01_owl_framework/togglingTodo.png
       :align: center
       :alt: Toggling todos

.. spoiler:: Solution

    - `Solution of the exercice can be found here (TODO) <https://github.com/ged-odoo/odoo-js-training-public/commit/efd7bdbf6f12abd44479de6de5ae96525649d925>`_

Deleting todos
==============

The final touch is to let the user delete a todo.

- Add a new callback prop ``removeTodo``
- Add a ``<span class="fa fa-remove">`` in the Todo component
- Ahenever the user clicks on it, it should call the ``removeTodo`` method
- Make it work as expected

.. spoiler:: Preview

    .. image:: 01_owl_framework/deletingTodo.png
       :align: center
       :alt: Deleting todos

.. spoiler:: Solution

    - `Solution of the exercice can be found here (TODO) <https://github.com/ged-odoo/odoo-js-training-public/commit/efd7bdbf6f12abd44479de6de5ae96525649d925>`_

Generic components with Slots
=============================

Owl has a powerful slot system to allow you to write generic components. This is
useful to factorize common layout between different parts of the interface.


- Write a ``Card`` component, using the following bootstrap html structure:

    .. code-block:: html

        <div class="card" style="width: 18rem;">
            <img src="..." class="card-img-top" alt="..." />
            <div class="card-body">
            <h5 class="card-title">Card title</h5>
            <p class="card-text">
                Some quick example text to build on the card title and make up the bulk
                of the card's content.
            </p>
            <a href="#" class="btn btn-primary">Go somewhere</a>
            </div>
        </div>

- This component should have two slots: one slot for the title, and one for
  the content (the default slot). For example, here is how one could use it:

    .. code-block:: html
        
        <Card>
            <t t-set-slot="title">Card title</t>
            <p class="card-text">Some quick example text...</p>
            <a href="#" class="btn btn-primary">Go somewhere</a>
        </Card>

- Bonus point: if the ``title`` slot is not given, the ``h5`` should not be
  rendered at all

.. note:: References:

    - `owl: slots <https://github.com/odoo/owl/blob/master/doc/reference/slots.md>`_
    - `owl: slot props <https://github.com/odoo/owl/blob/master/doc/reference/slots.md#slots-and-props>`_
    - `bootstrap: documentation on cards <https://getbootstrap.com/docs/5.2/components/card/>`_

.. spoiler:: Preview

    .. image:: 01_owl_framework/card.png
       :align: center
       :alt: Creating card with slots

.. spoiler:: Solution

    - `Solution of the exercice can be found here (TODO) <https://github.com/ged-odoo/odoo-js-training-public/commit/efd7bdbf6f12abd44479de6de5ae96525649d925>`_

Miscellaneous small tasks
=========================

- Add prop validation on the ``Card`` component
- Try to express in the prop validation system that it requires a ``default``
  slot, and an optional ``title`` slot

.. note:: References:

    - `owl: props validation <https://github.com/odoo/owl/blob/master/doc/reference/props.md#props-validation>`_
