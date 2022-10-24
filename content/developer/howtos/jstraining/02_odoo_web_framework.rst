.. _howto/jstraining/02_odoo_web_framework:

==============================
Chapter 2: Odoo web framework
==============================

We will now learn to use the Odoo javascript framework. In this module, we will
improve our Awesome dashboard. This will be a good opportunity to discover many useful features.


.. image:: 02_odoo_web_framework/overview_02.png
   :align: center
   :alt: overview


A new Layout
================

Most screens in the Odoo web client uses a common layout: a control panel on top,
with some buttons, and a main content zone just below. This is done using a
``Layout`` component, available in ``@web/search/layout``.

- Update the ``AwesomeDashboard`` component to use the ``Layout`` component

.. note::

   The ``Layout`` component has been primarily designed with the current
   views in mind. It is kind of awkward to use in another context, so it is highly
   suggested to have a look at how it is done in the link provided in resources.

.. spoiler:: Preview
      
   .. image:: 02_odoo_web_framework/newLayout.png
      :align: center
      :alt: new Layout


.. note:: References:

   - `example: use of Layout in client action <https://github.com/odoo/odoo/blob/16d55910c151daafa00338c26298d28463254a55/addons/web/static/src/webclient/actions/reports/report_action.js>`_ and `template <https://github.com/odoo/odoo/blob/16d55910c151daafa00338c26298d28463254a55/addons/web/static/src/webclient/actions/reports/report_action.xml>`_
   - `example: use of Layout in kanban view <https://github.com/odoo/odoo/blob/16d55910c151daafa00338c26298d28463254a55/addons/web/static/src/views/kanban/kanban_controller.xml>`_
   - `code: Layout component <https://github.com/odoo/odoo/blob/16d55910c151daafa00338c26298d28463254a55/addons/web/static/src/search/layout.js>`_

Add some buttons for quick navigation
=========================================

Bafien Ckinpaers want buttons for easy access to common views in Odoo. Let us
add three buttons in the control panel bottom left zone:


- A button ``Customers`` , which opens a kanban view with all customers (this action already exists, so you should use its xml id)
- A button ``New Orders``\ , which opens a list view with all orders created in the last 7 days
- A button ``Cancelled Order``\ , which opens a list of all orders created in the last 7 days, but already cancelled

.. spoiler:: Preview

   .. image:: 02_odoo_web_framework/navigationButtons.png
      :align: center
      :alt: buttons for quick navigation


.. note:: References:
   - :ref:`odoo: page on services <frontend/services>`
   - `example: doaction use <https://github.com/odoo/odoo/blob/16d55910c151daafa00338c26298d28463254a55/addons/account/static/src/components/journal_dashboard_activity/journal_dashboard_activity.js#L35>`_
   - `data: action displaying res.partner <https://github.com/odoo/odoo/blob/16d55910c151daafa00338c26298d28463254a55/odoo/addons/base/views/res_partner_views.xml#L511>`_
   - `code: action service <https://github.com/odoo/odoo/blob/16d55910c151daafa00338c26298d28463254a55/addons/web/static/src/webclient/actions/action_service.js#L1456>`_

Call the server, add some statistics
========================================

Let's improve the dashboard by adding a few cards (see the ``Card`` component
made in the Owl training) containing a few statistics. There is a route
``/awesome_tshirt/statistics`` that will perform some computations and return an
object containing some useful informations.


- Change ``Dashboard`` so that it uses the ``rpc`` service
- Call the statistics route in the ``onWillStart`` hook
- Display a few cards in the dashboard containing:

  - Number of new orders this month
  - Total amount of new orders this month
  - Average amount of t-shirt by order this month
  - Number of cancelled orders this month
  - Average time for an order to go from 'new' to 'sent' or 'cancelled'

.. spoiler:: Preview

   .. image:: 02_odoo_web_framework/statistics.png
      :align: center
      :alt: statistics cards


.. note:: References:

   - :ref:`odoo: rpc service <frontend/services/rpc>`
   - `code: rpc service <https://github.com/odoo/odoo/blob/master/addons/web/static/src/core/network/rpc_service.js>`_
   - `example: calling a route in willStart <https://github.com/odoo/odoo/blob/16d55910c151daafa00338c26298d28463254a55/addons/lunch/static/src/views/search_model.js#L21>`_

Cache network calls, create a service
=========================================

If you open your browser dev tools, in the network tabs, you will probably see
that the call to ``/awesome_tshirt/statistics`` is done every time the client
action is displayed. This is because the ``onWillStart`` hook is called each
time the ``Dashboard`` component is mounted. But in this case, we probably would
prefer to do it only the first time, so we actually need to maintain some state
outside of the ``Dashboard`` component. This is a nice use case for a service!


- Implements a new ``awesome_tshirt.statistics`` service
- It should provide a function ``loadStatistics`` that, once called, performs the
  actual rpc, and always return the same information
- Maybe use the ``memoize`` utility function from ``@web/core/utils/functions``
- Use it in the ``Dashboard`` component
- Check that it works as expected

.. note:: References:

   - `example: simple service <https://github.com/odoo/odoo/blob/16d55910c151daafa00338c26298d28463254a55/addons/web/static/src/core/network/http_service.js>`_
   - `example: service with a dependency <https://github.com/odoo/odoo/blob/baecd946a09b5744f9cb60318563a9720c5475f9/addons/web/static/src/core/user_service.js>`_
   - `code: memoize function <https://github.com/odoo/odoo/blob/16d55910c151daafa00338c26298d28463254a55/addons/web/static/src/core/utils/functions.js#L11>`_

Display a pie chart
=======================

Everyone likes charts (!), so let us add a pie chart in our dashboard, which
displays the proportions of t-shirts sold for each size: S/M/L/XL/XXL

For this exercise, we will use Chart.js. It is the chart library used by the
graph view. However, it is not loaded by default, so we will need to either add
it to our assets bundle, or lazy load it (usually better, since our users will not have
to load the chartjs code every time if they don't need it).


- Load chartjs
- In a ``Card`` (from previous exercises), display a pie chart in the dashboard that displays the correct quantity for each
  sold tshirts in each size (that information is available in the statistics route)

.. spoiler:: Preview

   .. image:: 02_odoo_web_framework/pieChart.png
      :align: center
      :alt: pie chart


.. note:: References:

   - `Chart.js website <https://www.chartjs.org/>`_
   - `Chart.js documentation on pie chart <https://www.chartjs.org/docs/latest/samples/other-charts/pie.html>`_
   - `example: lazy loading a js file <https://github.com/odoo/odoo/blob/16d55910c151daafa00338c26298d28463254a55/addons/web/static/src/views/graph/graph_renderer.js#L53>`_
   - `code: loadJs function <https://github.com/odoo/odoo/blob/16d55910c151daafa00338c26298d28463254a55/addons/web/static/src/core/assets.js#L23>`_
   - `example: rendering a chart in a component <https://github.com/odoo/odoo/blob/3eb1660e7bee4c5b2fe63f82daad5f4acbea2dd2/addons/web/static/src/views/graph/graph_renderer.js#L630>`_

Misc
========

Here is a list of some small improvements you could try to do if you have the
time:


- Make sure your application can be translated (with ``env._t``\ )
- Clicking on a section of the pie chart should open a list view of all orders
  which have the corresponding size
- Add a scss file and see if you can change the background color of the dashboard action

.. spoiler:: Preview

   .. image:: 02_odoo_web_framework/misc.png
      :align: center
      :alt: background color and translation


.. note:: References:
   - :ref:`odoo: translating modules <reference/translations>`
   - `example: use of env._t function <https://github.com/odoo/odoo/blob/16d55910c151daafa00338c26298d28463254a55/addons/account/static/src/components/bills_upload/bills_upload.js#L64>`_
   - `code: translation code in web/ <https://github.com/odoo/odoo/blob/16d55910c151daafa00338c26298d28463254a55/addons/web/static/src/core/l10n/translation.js#L16>`_
