# Django Trac #17522 — proof assets

These assets accompany the PR that ports Sebastian Goll's 2012 fix for
[Trac #17522](https://code.djangoproject.com/ticket/17522) — allowing admin
methods whose `admin_order_field` points to a real model field (or annotation)
to appear in `ModelAdmin.ordering` without tripping `admin.E033`.

- Fix branch (fork): `ticket_17522_admin_ordering_admin_order_field`
- Fix HEAD SHA: `c3744a61a710fee859174d12ae098585de12be6e`
- Base SHA (Django `main`): `f0e7bd90884b5da0a7418f3c78d1fd309c905014`

## Assets

| File | What it shows |
| --- | --- |
| `01-reproducer-before.png` / `.txt` | Base `main` (pre-fix). Running the ticket's reproducer raises `admin.E033` — `ModelAdmin.check()` rejects `ordering=("sorted_name",)` even though `sorted_name` declares `admin_order_field="name"` via `@admin.display(ordering="name")`. |
| `02-reproducer-after.png` / `.txt` | Same reproducer, same working tree, fix commit checked out. `ModelAdmin.check()` now returns no errors and the script prints `OK`. |
| `03-tests-pass.png` / `.txt` | `python -Wa ./runtests.py modeladmin.test_checks.OrderingCheckTests modeladmin.tests` — 58 tests pass (includes the five new `OrderingCheckTests` cases covering admin-method + `@admin.display(ordering=...)`, legacy `method.admin_order_field`, missing `admin_order_field`, `admin_order_field` pointing at a nonexistent field, and `admin_order_field` resolving to a custom `get_queryset()` annotation). |

## Reproduce locally

```powershell
git clone https://github.com/Aditya060806/django.git
cd django
git checkout ticket_17522_admin_ordering_admin_order_field
python -m venv .venv
.\.venv\Scripts\python.exe -m pip install -e .
cd tests
..\.venv\Scripts\python.exe -Wa .\runtests.py modeladmin.test_checks.OrderingCheckTests modeladmin.tests
```

The `.txt` files are the exact PowerShell transcripts (command + raw
stdout+stderr) that the `.png` screenshots render.
