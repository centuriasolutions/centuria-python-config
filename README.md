# centuria-python-config

Shared Python lint + type configs for Centuria projects.

## Install

In your project's `pyproject.toml`:

```toml
[tool.ruff]
extend = "https://raw.githubusercontent.com/centuriasolutions/centuria-python-config/v0.1.0/ruff.toml"
```

For mypy, copy `mypy.ini` to your project root, or add equivalent settings to `pyproject.toml`:

```toml
[tool.mypy]
strict = true
# ... (copy values from mypy.ini)
```

## Versioning

Pin to a tag in your extend URL. Do not use `main` — changes could break your CI.
