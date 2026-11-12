# Odoo boilerplate

Keep your own Odoo modules in Git and deploy them with Wodby on top of the
prebuilt Odoo image.

Use it with the [buildable Odoo](https://github.com/wodby/service-odoo-buildable)
service (`odoo-buildable`). For Odoo without your own modules, use the
[Odoo](https://github.com/wodby/service-odoo) service instead.

## Odoo's version

Choose the Odoo version (18, 19 or 20) for the service when you create the app.
The build starts from [`wodby/odoo`](https://github.com/wodby/odoo) for that
version, which brings Odoo Community Edition with every Community app and
Wodby's setup. Your modules must support that version.

## Adding modules

Put each module in its own directory under `addons/`:

```
addons/
  my_module/
    __manifest__.py
    __init__.py
    models/
    views/
```

Modules can also live in other directories up to three levels deep, for
example one directory per module collection copied from a repository of the
Odoo Community Association. The build adds every
directory that holds modules to Odoo's addons path and prints it in the build
log.

The build fails if a module has the name of one of Odoo's own modules, because
Odoo would ignore it.

## Python packages

List the Python packages your modules need in `requirements.txt`. The build
installs them with pip. There is no compiler in the image, so use packages
that have prebuilt wheels.

## Deployments

- The first deployment installs Odoo with the apps from the service's "Apps
  to install" setting. Add your own modules there to install them right away.
- Later deployments update the installed modules whose files changed, or whose
  manifest version went up.
- New modules appear in Odoo's Apps menu. Install them there.
- Uninstall a module in Odoo before you remove it from the repository.

## Files

| Path | Purpose |
| --- | --- |
| `addons/` | Your Odoo modules |
| `requirements.txt` | Python packages your modules need |
| `.wodby/pipeline.yml` | The Wodby CI pipeline that builds and deploys the image |
| `.dockerignore` | Files left out of the image |
