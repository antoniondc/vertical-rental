# Sale Rental - Odoo 18 migration notes

## Scope

This package is a conservative port of the provided `sale_rental` module from Odoo 16 style metadata/views to Odoo 18 style metadata/views. The business logic was kept as close as possible to the original module.

## Commit comments by file

### `__manifest__.py`
**Commit comment:** `chore: bump sale_rental manifest to Odoo 18`

Changed the module version from `16.0.1.0.4` to `18.0.1.0.0`.

### `data/ir_crons.xml`
**Commit comment:** `fix: remove deprecated ir.cron numbercall field`

Removed the `numbercall` field from the scheduled action definition to avoid installation failures on newer Odoo versions where this field is no longer expected in cron XML data.

### `models/product.py`
**Commit comment:** `fix: adapt rentable product domain for Odoo 18 product types`

Changed the rented product domain from the old `product/consu` selection pair to a broader non-service domain. This keeps rental services restricted to service products while allowing rentable stock/goods products under the newer product type model.

### `models/sale_rental.py`
**Commit comment:** `refactor: migrate rental display name computation for Odoo 18`

Added `_compute_display_name()` and a helper `_get_rental_display_name()` so the rental record label works with the newer display-name mechanism. Kept `name_get()` as a compatibility shim. Also corrected the reminder template log message from `your_module...` to `sale_rental...`.

### `views/product.xml`
**Commit comment:** `fix: migrate product rental view modifiers to Odoo 18 syntax`

Replaced legacy `attrs` expressions with direct `invisible` expressions supported by modern Odoo views.

### `views/sale_order.xml`
**Commit comment:** `fix: migrate sale order rental line modifiers and list xpath`

Replaced legacy `attrs` with direct `invisible` and `required` expressions. Updated the order-line subview XPath to target either `tree` or `list`, which makes the inheritance more tolerant during the Odoo 18 migration.

### `views/sale_rental.xml`
**Commit comment:** `fix: migrate rental tree views to list views`

Replaced `<tree>` views with `<list>` views and changed the action `view_mode` from `tree` to `list`.

### `views/stock_warehouse.xml`
**Commit comment:** `fix: migrate warehouse rental view modifiers to Odoo 18 syntax`

Replaced legacy `attrs` expressions with direct `invisible` expressions.

### `views/res_config_settings_views.xml`
**Commit comment:** `refactor: port sale rental settings block to Odoo 18 settings layout`

Reworked the settings view inheritance from the older `o_setting_box` layout to the newer `app/block/setting` layout under Sales settings.

### `wizard/create_rental_product.py`
**Commit comment:** `fix: replace tree view mode with list in rental product wizard action`

Changed the wizard return action from `form,tree,kanban` to `form,list,kanban`.

## Validation performed

- XML files were parsed successfully with `lxml`.
- Python files were compiled successfully with `python -B -m py_compile`.
- Removed macOS metadata and Python cache files from the final ZIP.

## Not validated here

This sandbox does not include a running Odoo 18 server with the required dependencies, so the module was not installed against a live database. Recommended next step:

```bash
odoo -d <database> -i sale_rental --stop-after-init
```

or, if already installed:

```bash
odoo -d <database> -u sale_rental --stop-after-init
```
