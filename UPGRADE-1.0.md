# Upgrade from 0.6.x to 1.0

## Requirements

- **Sylius 2.0** is now required (Sylius 1.x is no longer supported)
- **Symfony 6.4 or 7.4** is now required
- **PHP 8.2** minimum 

---

## 1. Update routing import

The routing resource paths have changed. Update your routing file:

```yaml
# config/routes/setono_sylius_feed.yaml

# Before
setono_sylius_feed:
    resource: "@SetonoSyliusFeedPlugin/Resources/config/routing.yaml"

# After
setono_sylius_feed:
    resource: "@SetonoSyliusFeedPlugin/config/routing.yaml"
```

Same applies if you use the non-localized variant:

```yaml
# Before
resource: "@SetonoSyliusFeedPlugin/Resources/config/routing_non_localized.yaml"

# After
resource: "@SetonoSyliusFeedPlugin/config/routing_non_localized.yaml"
```

---

## 2. Update app config import

```yaml
# config/packages/setono_sylius_feed.yaml

# Before
imports:
    - { resource: "@SetonoSyliusFeedPlugin/Resources/config/app/config.yaml" }

# After
imports:
    - { resource: "@SetonoSyliusFeedPlugin/config/app/config.yaml" }
```

---

## 3. Template overrides

All plugin templates have moved from `src/Resources/views/` to `templates/` at the bundle root. If you overrode any plugin template in your application, you need to update the paths in `templates/bundles/SetonoSyliusFeedPlugin/`.

### State label templates

| Before | After |
|--------|-------|
| `Admin/Feed/Label/State/error.html.twig` | `admin/feed/label/state/error.html.twig` |
| `Admin/Feed/Label/State/processing.html.twig` | `admin/feed/label/state/processing.html.twig` |
| `Admin/Feed/Label/State/ready.html.twig` | `admin/feed/label/state/ready.html.twig` |
| `Admin/Feed/Label/State/unprocessed.html.twig` | `admin/feed/label/state/unprocessed.html.twig` |

Note: the new label templates use Bootstrap badge markup (Sylius 2 admin) instead of Semantic UI labels. If you overrode these to change styling, review your overrides accordingly.

### Feed show page

The monolithic `Admin/Feed/show.html.twig` has been replaced with a set of **Twig Hook** fragments (Sylius 2 admin UI pattern). If you overrode `Admin/Feed/show.html.twig`, your override will no longer have any effect.

Instead, use Sylius Twig Hooks to customize the feed show page. The hook prefix is `setono_sylius_feed.admin.feed`. Available hooks are defined in `config/twig_hooks/admin/feed/show.yaml`.

### Feed templates (feed output)

| Before | After |
|--------|-------|
| `Feed/feed.txt.twig` | `feed/feed.txt.twig` |
| `Feed/Google/Shopping/_item.txt.twig` | `feed/google/shopping/_item.txt.twig` |
| `Feed/Google/Shopping/feed.txt.twig` | `feed/google/shopping/feed.txt.twig` |

---

## 4. Admin create/update form

The feed create and update pages now use Twig Hooks for their form sections. If you previously overrode the admin create/update templates via Twig bundle overrides, replace those with Twig Hook configuration targeting the `setono_sylius_feed.admin.feed` hook prefix (see `config/twig_hooks/admin/feed/create.yaml` and `config/twig_hooks/admin/feed/update.yaml`).
