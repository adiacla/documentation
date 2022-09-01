========
Colombia
========

.. |DIAN| replace:: :abbr:`DIAN (Dirección de Impuestos y Aduanas Nacionales)`

Odoo *localization for Colombia* provides accounting, fiscal and legal features in Colombia such as
chart of accounts, taxes and electronic invoicing.

Please refer to the documentation below regarding Colombia localization configuration, workflows and
financial reporting.

In addition, we have a series of videos covering how to start from scratch, configuration, main
workflows, as well as special use cases.

- :ref:`Configure master data for Colombia <colombia/master-data>`
- :ref:`Use and configure electronic invoicing in Odoo for Colombia <colombia/electronic-invoicing>`
- :ref:`Invoice creation <colombia/invoice-creation>` and :ref:`validation
  <colombia/invoice-validation>`
- :ref:`Reception of legal XML and PDF <colombia/invoice-xml>`
- :ref:`Avoid common mistakes <colombia/common-errors>`
- :ref:`Financial reports <colombia/reports>`

.. seealso::
   `Odoo Colombian localization videos
   <https://www.youtube.com/playlist?list=PL1-aSABtP6ABxZshems3snMjx7bj_7ZsZ>`_.

.. _colombia/configuration:

Configuration
=============

Modules installation
--------------------

:ref:`Install <general/install>` the following modules to get all the features of the Colombian
localization:

.. list-table::
   :header-rows: 1
   :widths: 25 25 50

   * - Name
     - Technical name
     - Description
   * - :guilabel:`Colombia - Accounting`
     - `l10n_co`
     - Default :ref:`fiscal localization package <fiscal_localizations/packages>`. This module adds
       the base accounting features for the Colombian localization: chart of accounts, taxes,
       withholdings, identification document type.
   * - :guilabel:`Colombian - Accounting Reports`
     - `l10n_co_reports`
     - Includes accounting reports for sending certifications to suppliers for withholdings applied.
   * - :guilabel:`Electronic invoicing for Colombia with Carvajal`
     - `l10n_co_edi`
     - This module includes the features that are required for the integration with Carvajal and
       generates the electronic invoices and support document for vendor bills based on |DIAN|
       regulations.
   * - :guilabel:`Colombian - Point of Sale`
     - `l10n_co_pos`
     - Includes Point of Sale Receipt for Colombian Localization.

.. note::
   When a database is created from scratch selecting :guilabel:`Colombia` as the country, Odoo
   automatically installs the base module *Colombia - Accounting* and *Colombia - Accounting
   Reports*.

Configure your company
----------------------

To configure your company information, go to the :menuselection:`Contacts` app and search the name
given to your company or activate :ref:`developer mode <developer-mode>` and go to
:menuselection:`Company --> Contact` and then edit the contact to configure the following
information:

#. General data

   - :guilabel:`Company Name`
   - :guilabel:`Address`: Include :guilabel:`City`, :guilabel:`Department` and :guilabel:`Zip Code`.
   - :guilabel:`Tax ID`: When it is a `NIT` it must have the *verification digit* at the end of the
     ID followed by a hyphen.

   .. image:: colombia/company-configuration.png
      :align: center
      :alt: Configure your company contact form in Odoo.

#. :guilabel:`Fiscal Information`

   On the :guilabel:`Sales \& Purchases` tab, configure the following information:

   - :guilabel:`Fiscal Position`: Determines the taxes/accounts used for this contract.
   - :guilabel:`Obligaciones y Responsabilidades`
   - :guilabel:`Gran Contribuyente`
   - :guilabel:`Fiscal Regimen`
   - :guilabel:`Commercial Name`

   .. image:: colombia/fiscal-information.png
      :align: center
      :alt: Configure the fiscal information of your company in Odoo.

Configure credentials for Carvajal web service
----------------------------------------------

Once the modules are installed, the user credentials need to be configured in order to connect with
Carvajal Web Service. First, navigate to :menuselection:`Accounting --> Configuration --> Settings`
and look for the :guilabel:`Colombian Electronic Invoice` section. Then, fill in the required
configuration information provided by Carvajal.

Complete the following data:

- :guilabel:`Username` and :guilabel:`Password`: Corresponds to the username and password provided
  by Carvajal to the company.
- :guilabel:`Company Registry`: Company's NIT number without the verification code.
- :guilabel:`Account ID`: Company ID followed by `_01`.
- :guilabel:`Colombia Template Code`: Select one of the two available templates (:guilabel:`CGEN03`
  or :guilabel:`CGNE04`) to be used in the PDF format of the electronic invoice.

.. image:: colombia/carvajal-configuration.png
   :align: center
   :alt: Configure credentials for Carvajal web service in Odoo.

Check the :guilabel:`Test mode` checkbox to connect with the Carvajal testing environment.

.. important::
   The testing mode must be used only on replicated databases but **NOT** in the production
   environment.

Once Odoo and Carvajal are fully configured and ready for production, uncheck the :guilabel:`Test
mode` checkbox to use the production database.

Configure report data
---------------------

Report data can be defined for the fiscal section and the bank information in the PDF as part of the
configurable information that is sent in the XML.

Navigate to :menuselection:`Accounting --> Configuration --> Settings` and look for the
:guilabel:`Colombian Electronic Invoice` section.

.. image:: colombia/report-config.png
   :align: center
   :alt: Configure the report data in Odoo.

.. _colombia/master-data:

Configure master data
---------------------

Partner
~~~~~~~

Configure the *identification information* and *fiscal information*.

Identification information
**************************

As part of the Colombian Localization, the document types defined by the |DIAN| are now available on
the partner form. Colombian partners have to have their :guilabel:`Identification Number` (VAT) and
:guilabel:`Document Type` set:

.. image:: colombia/partner-rut-doc-type.png
   :align: center
   :alt: Identification number and the document type set in Odoo.

.. tip::
   When the :guilabel:`Document Type` is `NIT` the :guilabel:`Identification Number` needs to be
   configured in Odoo including the *verification digit*, Odoo will split this number when the data
   to the third party vendor is sent.

Fiscal information
******************

The partner's responsibility codes (section 53 in the RUT document) are included as part of the
electronic invoice module given it is part of the information required by the |DIAN|.

The required fields can be found in :menuselection:`Partner --> Sales & Purchase Tab --> Fiscal
Information`.

.. image:: colombia/partner-fiscal-information.png
   :align: center
   :alt: The fiscal information included in the electronic invoice module in Odoo.

Additionally, two boolean fields were added in order to specify the :guilabel:`Fiscal Regimen` of
the partner.

Products
~~~~~~~~

In addition to adding the basic information in the product form, the :guilabel:`UNSPSC Category`,
:guilabel:`Barcode` or the :guilabel:`Internal Reference` field should be configured.

.. image:: colombia/product-configuration.png
   :align: center
   :alt: Configuring the UNSPSC Category field in Odoo on a product form.

Taxes
~~~~~

If sales transactions include products with taxes, the :guilabel:`Value Type` field in the
:guilabel:`Advanced Options` tab needs to be configured per tax.

Retention tax types (:guilabel:`ICA`, :guilabel:`IVA`, :guilabel:`Fuente`) are also included in the
options to configure taxes. This configuration is used in order to correctly display taxes in the
invoice PDF.

.. image:: colombia/retention-tax-types.png
   :align: center
   :alt: The ICA, IVA and Fuente fields in the Advanced Options tab in Odoo.

Sales journals
~~~~~~~~~~~~~~

Once the |DIAN| has assigned the official sequence and prefix for the electronic invoice resolution,
the sales journals related to the invoice documents need to be updated in Odoo.

Navigate to :menuselection:`Accounting --> Configuration --> Journals`.

.. image:: colombia/sales-journal.png
   :align: center
   :alt: Example of a sales journal being configured in Odoo.

Complete the following data in the :guilabel:`Advanced Settings` tab:

- :guilabel:`Electronic invoicing`: Check :guilabel:`UBL 2.1 (Colombia)`.
- :guilabel:`Invoicing Resolution`: Resolution number issued by |DIAN| to the company.
- :guilabel:`Resolution Date`: Initial effective date of the resolution.
- :guilabel:`Resolution end date`: End date of the resolution's validity.
- :guilabel:`Range of Numbering (minimum)`: First authorized invoice number.
- :guilabel:`Range of Numbering (maximum)`: Last authorized invoice number.

.. image:: colombia/sales-journal-advanced.png
   :align: center
   :alt: The Advanced Settings tab configuration on a sales journal in Odoo.

.. note::
   The sequence and resolution on the journal, should match with the one configured in Carvajal and
   the |DIAN|.

Invoice sequence
****************

The invoice sequence and prefix must be correctly configured when the first document is created.

.. image:: colombia/invoice-sequence.png
   :align: center
   :alt: Configuring an invoice sequence and prefix in Odoo.

.. note::
   Odoo will automatically assign a prefix and sequence to the following documents.

Purchase journals
*****************

Once the |DIAN| has assigned the official sequence and prefix for the support document for vendor
bills, the purchase journals related to their supporting documents need to be updated in Odoo. The
process is similar to the configuration of the sales journals.

Chart of accounts
*****************

The :doc:`chart of accounts
</applications/finance/accounting/getting_started/initial_configuration/chart_of_accounts>` is
installed by default as part of the set of data included in the localization module, the accounts
are mapped automatically in taxes, default account payable, default account receivable. The chart of
accounts for Colombia is based on the PUC (Plan Unico de Cuentas).

.. image:: colombia/chart-of-accounts.png
   :align: center
   :alt: Cart of Account configuration in Odoo for Colombia localization.

.. _colombia/workflows:

Main workflows
==============

Odoo *Localization for Colombia* includes the workflow for electronic invoices and support documents
for vendor bills. The following diagram illustrates how electronic invoices are generated and
transmitted to acquirers and |DIAN| once you have configured your database.

.. image:: colombia/electronic-invoice-workflow.png
   :align: center
   :alt: Electronic invoice workflow in Odoo.

.. _colombia/electronic-invoicing:

Electronic invoices
-------------------

With all of the master data and credentials configured, it is possible to start using the electronic
invoice workflow.

.. _colombia/invoice-creation:

Invoice creation
~~~~~~~~~~~~~~~~

.. note::
   The functional workflow that takes place before an invoice validation doesn't alter the main
   changes that are introduced with the electronic invoice.

Electronic invoices are generated and sent to the |DIAN| and the customer through Carvajal's web
service integration. These documents can be created from your sales order or manually. Go to
:menuselection:`Accounting --> Customers --> Invoices`

Complete the following data:

- :guilabel:`Customer`: Customer's information.
- :guilabel:`Journal`: Journal for electronic invoices.
- :guilabel:`Electronic Invoice Type`: Select the type of document. By default, :guilabel:`Factura
  de Venta` is selected.
- :guilabel:`Invoice Line`: Specify the products with the correct taxes.

Once reviewed you can click on the :guilabel:`Confirm` button.

.. image:: colombia/electronic-invoice-creation.png
   :align: center
   :alt: Configuring the electronic invoice in Odoo.

.. _colombia/invoice-validation:

Invoice validation
~~~~~~~~~~~~~~~~~~

After the invoice is confirmed, an :file:`.XML` file is created and sent automatically to Carvajal.
The invoice will be processed asynchronously by the E-invoicing service: UBL 2.1 (Colombia).

This file is also displayed in the chatter.

.. image:: colombia/invoice-sent.png
   :align: center
   :alt: Carvajal XML invoice file in Odoo chatter.

The :guilabel:`Electronic Invoice Name` field is now displayed in the :guilabel:`EDI Documents` tab
with the name of the :file:`.XML` file. Additionally, the :guilabel:`Electronic Invoice Status`
field is displayed with the initial value :guilabel:`In progress`.

.. image:: colombia/invoice-sent-status.png
   :align: center
   :alt: Electronic Invoice EDI Document XML file in the EDI Documents tab.

.. _colombia/invoice-xml:

Reception of legal XML and PDF
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The electronic invoice vendor (Carvajal) receives the :file:`.XML` file and proceeds to validate the
structure and the information in it.

In the :menuselection:`Action` dropdown, select the :guilabel:`Check Carvajal Status` button. If
everything is correct the :guilabel:`Electronic Invoice Status` field value changes to
:guilabel:`Validated`. Then, proceed to generate a legal XML which includes a digital signature and
a unique code (CUFE), a PDF invoice that includes a QR code and the CUFE is also generated.

A :file:`.ZIP` containing the legal electronic invoice in XML format and the invoice in PDF format
is downloaded and displayed in the invoice chatter:

  .. image:: colombia/invoice-zip.png
     :align: center
     :alt: ZIP file displayed in the invoice chatter in Odoo.

The electronic invoice status changes to :guilabel:`Accepted`.

Credit notes
------------

The process for credit note is exactly the same as the invoice, the functional workflow remains the
same as well. To create a credit note with reference to an invoice, go to :menuselection:`Accounting
--> Customers --> Invoices`. On the invoice select the :guilabel:`Add Credit Note` button, by
clicking on this button you will be directed to the create credit note form. Then complete the
following information:

- :guilabel:`Credit Method`: Select the type of credit method.

  - :guilabel:`Partial Refund`: Use this option when it is a partial credit note.
  - :guilabel:`Full Refund`: Use this option if the credit note is for the total invoice.
  - :guilabel:`Full refund and new draft invoice`: Use this option if the credit note is for the
    total invoice and you need the credit note and auto-create a new draft invoice.

- :guilabel:`Reason`: Type the reason for the credit note.
- :guilabel:`Reversal Date`: Select if you want a specific date for the credit note or if it is the
  journal entry date.
- :guilabel:`Use Specific Journal`: Select the journal for your credit note, or leave it empty if
  you want to use the same journal as the original invoice.
- :guilabel:`Refund Date`: If you chose a specific date, select the date for the credit note.

Once reviewed, you can click on the :guilabel:`Reverse` button.

.. image:: colombia/credit-note.png
   :align: center
   :alt: Completing the credit note form in Odoo.

Debit notes
-----------

The process for a debit note is similar to the credit note. The functional workflow remains the same
as well. To create a debit note with reference to an invoice, go to :menuselection:`Accounting -->
Customers --> Invoices`. On the invoice select the :guilabel:`Add Debit Note` button, by clicking on
this button you will be directed to the debit note form. Then complete the following information:

- :guilabel:`Reason`: Type the reason for the debit note.
- :guilabel:`Debit note date`: Select the specific options.
- :guilabel:`Copy lines`: Select this option if you need to register a debit note with the same
  lines of invoice.
- :guilabel:`Use Specific Journal`: Select the printer point for your debit note, or leave it empty
  if you want to use the same journal as the original invoice.

Once reviewed you can click on the :guilabel:`Create debit note` button.

.. image:: colombia/debit-note.png
   :align: center
   :alt: Completing the debit note form in Odoo.

Support document for vendor bills
---------------------------------

With all of the master data, credentials, and purchase journal configured for support documents for
vendor bills, it is possible to start using the support document workflow. The workflow is analogous
to the invoicing process.

Support documents for vendor bills can be created from your purchase order or manually. Go to
:menuselection:`Accounting --> Vendors --> Bills`.

Complete the following data:

- :guilabel:`Vendor`: Type the vendor's information.
- :guilabel:`Bill Date`: Select the date of the bill.
- :guilabel:`Journal`: Select the journal for support documents for vendor bills.
- :guilabel:`Invoiced Lines`: Specify the products with the correct taxes.

Once reviewed, you can click on the :guilabel:`Confirm` button.

.. image:: colombia/support-document.png
   :align: center
   :alt: Completing the support document form in Odoo.

Once the support document is confirmed, an :file:`.XML` file is created and automatically sent to
Carvajal. The validation and reception process is very similar to the electronic invoice process.

.. _colombia/common-errors:

Common errors
-------------

During the XML validation the most common errors are usually related to missing master data
(*Contact Tax ID*, *Address*, *Products*, *Taxes*). In such cases, error messages are shown in the
chatter after updating the electronic invoice status.

.. image:: colombia/xml-validation-errors.png
   :align: center
   :alt: XML validation errors shown in the invoice chatter in Odoo.

After the master data is corrected, it's possible to reprocess the XML with the new data and send
the updated version, using the following button in the :menuselection:`Action` dropdown.

.. _colombia/reports:

Financial reports
=================

This information is a quick reference to the accounting reports included in the *Colombian
Localization Accounting Reports* module.

The reports module (:guilabel:`l10n_co_reports`) is automatically installed when creating a database
for the country Colombia.

.. image:: colombia/colombia-accounting-reports-module.png
   :align: center
   :alt: Installed Colombia Accounting Reports Module in Odoo apps.

Certificado de Retención en ICA
-------------------------------

This report is a certification to vendors for withholdings made for the Colombian Industry and
Commerce tax (ICA).

Go to :menuselection:`Accounting --> Reporting --> Colombian Statements --> Certificado de Retención
en ICA`.

.. image:: colombia/ica-report.png
   :align: center
   :alt: Certificado de Retención en ICA report in Odoo Accounting.

Certificado de Retención en IVA
-------------------------------

This report issues a certificate on the amount withheld from vendors for VAT withholding.

Go to :menuselection:`Accounting --> Reporting --> Colombian Statements --> Certificado de Retención
en IVA`.

.. image:: colombia/iva-report.png
   :align: center
   :alt: Certificado de Retención en IVA report in Odoo Accounting.

Certificado de Retención en la Fuente
-------------------------------------

This certificate is issued to partners for the withholdings tax that they have made.

Go to :menuselection:`Accounting --> Reporting --> Colombian Statements --> Certificado de Retención
en Fuente`.

.. image:: colombia/fuente-report.png
   :align: center
   :alt: Certificado de Retención en Fuente report in Odoo Accounting.
