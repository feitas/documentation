.. _howto/jstraining/03_fields_and_views:

===========================
Chapter 3: Fields and Views
===========================

Fields and views are among the most important concepts in the Odoo user interface.
They are key to many important user interactions, and should therefore work
perfectly.

An ``image_preview`` field
==============================

Each new order on the website will be created as an ``awesome_tshirt.order``. This
model has a ``image_url`` field (of type char), which is currently only visible as
a string. We want to be able to see it in the form view.

For this task, we need to create a new field component ``image_preview``. This
component is specified as follows:

In readonly mode, it is only an image tag with the correct src if field is set.
In edit mode, it also behaves like classical char fields (you can use the ``CharField``
in your template by passing it props): an ``input`` should be displayed with the
text value of the field, so it can be edited.


- Register your field in the proper registry
- Update the arch of the form view to use your new field.

.. note::

   It is possible to solve this exercise by inheriting ``CharField``\ , but the
   goal of this exercise is to create a field from scratch.

.. spoiler:: Preview

   .. image:: 03_fields_and_views/imageField.png
      :align: center
      :alt: image field

.. note:: References:

   - `code: CharField <https://github.com/odoo/odoo/blob/baecd946a09b5744f9cb60318563a9720c5475f9/addons/web/static/src/views/fields/char/char_field.js>`_
   - `owl: t-props directive <https://github.com/odoo/owl/blob/master/doc/reference/props.md#dynamic-props>`_

Improving the image_preview field
=====================================

We want to improve the widget of the previous task to help the staff recognize
orders for which some action should be done. 
In particular, we want to display a warning 'MISSING TSHIRT DESIGN' in red, if there is no image url
specified on the order.

.. spoiler:: Preview

   .. image:: 03_fields_and_views/missingImage.png
      :align: center
      :alt: missing image when no url

Customizing a field component
=================================

Let's see how to use inheritance to extend an existing component.

There is a ``is_late``\ , readonly, boolean field on the task model. That would be
a useful information to see on the list/kanban/view. Then, let us say that
we want to add a red word ``Late!`` next to it whenever it is set to true.


- Create a new field ``late_order_boolean`` inheriting from ``BooleanField``
- Use it in the list/kanban/form view
- Modify it to add a red ``Late`` next to it, as requested


.. spoiler:: Preview

   .. image:: 03_fields_and_views/lateField.png
      :align: center
      :alt: is late field


.. note:: References:

   - `example: field inheriting another (js) <https://github.com/odoo/odoo/blob/f7b8f07501315233c8208e99b311935815039a3a/addons/account/static/src/components/account_type_selection/account_type_selection.js>`_
   - `example: field inheriting another (xml) <https://github.com/odoo/odoo/blob/f7b8f07501315233c8208e99b311935815039a3a/addons/account/static/src/components/account_type_selection/account_type_selection.xml>`_
   - :ref:`odoo: doc on xpath <reference/views/inheritance>`

Message for some customers
==============================

Odoo form views support a ``widget`` api, which is like a field, but more generic.
It is useful to insert arbitrary components in the form view. Let us see how we
can use it.

For a super efficient workflow, we would like to display a message/warning box
with some information in the form view, with specific messages depending on some
conditions:


- If the image_url field is not set, it should display "No image"
- If the amount of the order is higher than 100 euros, it should display "Add promotional material"
- Make sure that your widget is updated in real time.

.. spoiler:: Preview

   .. image:: 03_fields_and_views/warningWidget.png
      :align: center
      :alt: Warning widget

.. note:: References:

   - `example: using tag <widget> in a form view <https://github.com/odoo/odoo/blob/f7b8f07501315233c8208e99b311935815039a3a/addons/calendar/views/calendar_views.xml#L197>`_
   - `example: implementation of widget (js) <https://github.com/odoo/odoo/blob/f7b8f07501315233c8208e99b311935815039a3a/addons/web/static/src/views/widgets/week_days/week_days.js>`_
   - `example: implementation of widget (xml) <https://github.com/odoo/odoo/blob/f7b8f07501315233c8208e99b311935815039a3a/addons/web/static/src/views/widgets/week_days/week_days.xml>`_

Use ``markup``
==================

Let's see how we can display raw html in a template. Before, there was a ``t-raw``
directive that would just output anything as html. This was unsafe, and has been
replaced by a ``t-out`` directive, that acts like a ``t-esc`` unless the data has
been marked explicitely with a ``markup`` function.


- Modify the previous exercise to put the ``image`` and ``material`` words in bold
- The warnings should be markuped, and the template should be modified to use ``t-out``

This is an example of a safe use of ``t-out``\ , since the string is static.


.. spoiler:: Preview

   .. image:: 03_fields_and_views/warningWidget2.png
      :align: center
      :alt: Warning widget


.. note:: References:

   - `owl: doc on t-out <https://github.com/odoo/owl/blob/master/doc/reference/templates.md#outputting-data>`_

Add buttons in control panel
================================

In practice, once the t-shirt order is printed, we need to print a label to put
on the package. To help with that, let us add a button in the order form view control panel:


- Create a customized form view extending the web form view and register it as ``awesome_tshirt.order_form_view``
- Add a ``js_class`` attribute to the arch of the form view so Odoo will load it
- Create a new template inheriting from the form controller template to add a button after the create button
- Add a button, clicking on this button should call the method ``print_label`` from the model
  ``awesome_tshirt.order``\ , with the proper id (note: ``print_label`` is a mock method, it only display a message in the logs)
- It should be disabled if the current order is in ``create`` mode (i.e., it does not exist yet)
- It should be displayed as a primary button if the customer is properly set and if the task stage is ``printed``. Otherwise, it is only displayed as a secondary button.

   .. note::
      You can use the ``orm`` service instead of the ``rpc`` service. It provides a
      higher level interface when interacting with models.

- Bonus point: clicking twice on the button should not trigger 2 rpcs


.. spoiler:: Preview

   .. image:: 03_fields_and_views/formButton.png
      :align: center
      :alt: control panel button


.. note:: References:

   - `example: extending a view (js) <https://github.com/odoo/odoo/blob/f7b8f07501315233c8208e99b311935815039a3a/addons/mass_mailing/static/src/views/mailing_contact_view_list.js>`_
   - `example: extending a view (xml) <https://github.com/odoo/odoo/blob/f7b8f07501315233c8208e99b311935815039a3a/addons/mass_mailing/static/src/views/mass_mailing_views.xml>`_
   - `example: using a js_class attribute <https://github.com/odoo/odoo/blob/f7b8f07501315233c8208e99b311935815039a3a/addons/mass_mailing/views/mailing_contact_views.xml#L44>`_
   - `code: orm service <https://github.com/odoo/odoo/blob/f7b8f07501315233c8208e99b311935815039a3a/addons/web/static/src/core/orm_service.js>`_
   - `example: using the orm service <https://github.com/odoo/odoo/blob/f7b8f07501315233c8208e99b311935815039a3a/addons/account/static/src/components/open_move_widget/open_move_widget.js>`_

Auto reload the kanban view
===============================

Bafien is upset: he wants to see the kanban view of the tshirt orders on his
external monitor, but it needs to be up to date. He is tired of clicking on
the ``refresh`` icon every 30s, so he tasked you to find a way to do it automatically.

Just like the previous exercise, that kind of customization requires creating a
new javascript view.


- Extend the kanban view/controller to reload its data every minute
- Register it in the view registry, under the ``awesome_tshirt.autoreloadkanban``
- Use it in the arch of the kanban view (with the ``js_class`` attribute)

.. warning::

   Make sure that if you use a ``setInterval``\ , or something similar, that it is
   properly cancelled when your component is unmounted. Otherwise, you would introduce
   a memory leak.
