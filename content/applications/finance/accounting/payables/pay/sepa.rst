=============
Pay with SEPA
=============

SEPA, the Single Euro Payments Area, is a payment-integration initiative of the European Union to
simplify of bank transfers denominated in EURO. SEPA allows you to send payment orders to your
bank to automate bank wire transfers.

SEPA is supported by the banks of the 27 EU member states, as well as:

EFTA countries:

- Iceland;
- Liechtenstein;
- Norway;
- Switzerland.

Non-EEA SEPA countries:

- Andorra;
- Monaco;
- San Marino;
- United Kingdom;
- Vatican City State.

Non-EEA territories:

- Saint-Pierre-et-Miquelon;
- Guernsey;
- Jersey;
- Isle of Man.

When paying a bill in Odoo, you can select SEPA mandates as a payment option. At the end of the day,
the manager can generate the SEPA file containing all bank wire transfers and send it to the bank.

By default, the file follows the SEPA Credit Transfer **'pain.001.001.03'** specifications. This is
a well-defined standard among banks. However, according to the country set on your company, another
format can be used : **'pain.001.001.03.ch.02'** for Switzerland and **'pain.001.003.03'** for
Germany.

Once the payments are processed by your bank, you can directly import the account statement in
Odoo. The bank reconciliation process will seamlessly match the SEPA orders you sent to your bank
with actual bank statements.

Configuration
=============

Activate the SEPA Credit Transfer (SCT)
---------------------------------------

To pay suppliers with SEPA, you must activate the **SEPA Credit Transfer** setting.
To do so, go to :menuselection:`Accounting --> Configuration --> Settings --> Vendor Payments -->
SEPA Credit Transfer (SCT)`. By activating the setting and filling in your company data, you will be
able to add the SCT option when paying your vendor.

Activate the SEPA Direct Debit (SDD)
-------------------------------------
To get paid with SEPA, you must activate the **SEPA Direct Debit** setting. To do so, go to
:menuselection:`Accounting --> Configuration --> Settings --> Customer Payments -->
SEPA Direct Debit (SDD)`.

.. note::

   According to the localization package installed, the **SEPA Direct Debit** and **SEPA Credit
   Transfer** modules may be installed by default. If not, they need to be
   :doc:`installed </applications/general/apps_modules>`.

Activate SEPA payment methods on banks
--------------------------------------

From the accounting dashboard, click on the drop-down menu (⋮) on your bank journal and go to
:guilabel:`Configuration`.
To activate SEPA, click the **Incoming payments** tab for incoming payments and
**Outgoing Payments** tab for outgoing payments. You can add, if not already present, SEPA Direct
Debit and SEPA Credit Transfer as a payment method.

Make sure to specify the IBAN account number (domestic account numbers do not work with SEPA) and
the BIC (bank identifier code) in the **Journal Entries** tab.

Pay with SEPA
=============

Register your payments
----------------------

You can register both customer payments and vendor payments made with SEPA. To do so, use the top
menu :menuselection:`Purchases --> Payments`. Register your payment and select a payment method by
Sepa Direct Debit for customers and Sepa Credit Transfer for vendors.

If it is the first time you pay a vendor with SEPA, you will have to fill in the
:guilabel:`Recipient Bank Account` field with the bank name, IBAN, and BIC (Bank Identifier Code).
Odoo will automatically verify the IBAN format.

For future payments to this vendor, Odoo will automatically suggest you the bank account, but it
remains possible to select  a new one.

If you pay a specific supplier bill, put the reference of the bill in the :guilabel:`memo` field.

Once your payment is registered, do not forget to confirm it. You can also pay vendor bills from the
bill directly using the :guilabel:`Register Payment` button at the top of a vendor bill.
The form is the same, but the payment is directly linked to the bill and will be automatically
reconciled with it.

Generate SEPA files
-------------------

Once you have SEPA payments, you can access them through :menuselection:`Vendor --> Payments`.
After validation, you can automatically download the .XML file.
