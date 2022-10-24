:show-content:

.. _howto/jstraining:

=====================================
Working with the javascript framework
=====================================

For this training, we will put ourselves in the shoes of the IT staff for the fictional
Awesome T-Shirt company, which is in the business of printing customised tshirts for online customers.
The Awesome T-Shirt company uses Odoo for managing its orders, and built a dedicated odoo module to
manage their workflow. The project is currently a simple kanban view, with a few columns.

The usual process is the following: a customer looking for a nice t-shirt can simply order it on
the Awesome T-Shirt website, and give the url for any image that he wants. He also has to fill some
basic informations, such as the desired size, and amount of t-shirts. Once he confirms his order,
and once the payment is validated, the system will create a task in our project application.

The Awesome T-shirt big boss, Bafien Ckinpaers, is not happy with our implementation.
He believe that by micromanaging more, he will be able to extract more revenue from his employees.
As the IT staff for Awesome T-shirt, we are tasked with improving the system. Various independant tasks need to be done.

Let us now practice our odoo skills!

.. _howtos/jstraining/setup:

Setup
=============

In order to follow the training, it is necessary to have a recent version of Odoo installed.
If you don't have it installed yet, we recommend to install it from the :ref:`source <setup/install/source>` 
(:dfn:`running Odoo from the source code`).

.. warning:: Take care to follow the training on version 16.0 minimum. Odoo 16 is the beginning of a (mostly)
     completely new javascript codebase, following the training in an older version may either cause bugs or not work at all.

To setup your development environment you can also follow the dedicated chapter in
:doc:`Getting Started Odoo tutorial: Development environment setup <rdtraining/02_setup>`

The last things to do is to:

- Clone the `JS Training respository <https://github.com/ged-odoo/odoo-js-training-public>`_
- Add the modules to your addons path
- Prepare a new database with awesome_tshirt installed

You are now ready to follow the training, let's get started !

Exercices
=============

.. toctree::
    :caption: JS Training
    :titlesonly:
    :glob:

    jstraining/*

* :doc:`jstraining/01_owl_framework`
* :doc:`jstraining/02_odoo_web_framework`
* :doc:`jstraining/03_fields_and_views`
* :doc:`jstraining/04_miscellaneous`
* :doc:`jstraining/05_custom_kanban_view`
* :doc:`jstraining/06_creating_view_from_scratch`
* :doc:`jstraining/07_testing`

