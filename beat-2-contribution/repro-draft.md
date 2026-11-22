Reproduced on `main` at `ac40f8d` (orm-lens 1.18.2, installed from my fork with `cd cli && pip install -e ".[dev]"`), Python 3.14.7 in a venv on Windows 11 Pro (Git Bash). Django and django-prometheus are not installed: orm-lens only reads the source, so neither is needed.

Steps, using the issue's own repro:

1. Create `shop/models.py` in an empty folder:

```python
from django.db import models
from django_prometheus.models import ExportModelOperationsMixin


class Comment(ExportModelOperationsMixin("comment"), models.Model):
    text = models.TextField()


class Plain(models.Model):
    text = models.TextField()
```

2. From that folder, run `orm-lens list -v`.

Output:

```text
$ orm-lens list -v; echo "exit $?"
scanned 1 files in 14ms, found 1 apps / 1 models
shop.Plain
exit 0
```

Expected `found 1 apps / 2 models` with `shop.Comment` and `shop.Plain`. `Comment` is dropped with no message, and the command still exits 0.

Control: with `ExportModelOperationsMixin("comment"), ` deleted, so the header is `class Comment(models.Model):`, the same command finds both:

```text
$ orm-lens list -v
scanned 1 files in 11ms, found 1 apps / 2 models
shop.Comment
shop.Plain
```

The wrapped header from "How to test" fails the same way. Same imports and `Plain` class as above, with `Comment` replaced by:

```python
class Foo(
    ExportModelOperationsMixin("foo"),
    models.Model,
):
    text = models.TextField()
```

```text
$ orm-lens list -v; echo "exit $?"
scanned 1 files in 13ms, found 1 apps / 1 models
shop.Plain
exit 0
```

With the `ExportModelOperationsMixin("foo"),` line deleted, it finds `shop.Foo` and `shop.Plain` (`found 1 apps / 2 models`).

So the call in the base list is what drops the model, in both the one-line and the wrapped header, which matches the cause in the issue.
