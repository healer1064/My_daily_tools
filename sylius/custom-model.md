# How to create custom model

## Generate the entity

```bash
> bin/console make:entity
Class name of the entity to create or update (e.g. TinyPizza):
> CarCategory
created: src/Entity/CarCategory.php
created: src/Repository/CarCategoryRepository.php
```

or make with sub folder

```bash
> bin/console make:entity "Car\Brand"
```

## Update columns of entity

`src/Entity/CarCategory.php`

## Migration database

```bash
> php bin/console doctrine:schema:update --force
```

## Register your entity as resource

`config/packages/sylius_resource.yml`

```bash
sylius_resource:
  resources:
    app.car.category:
      driver: doctrine/orm
      classes:
        model: App\Entity\CarCategory
```

## Checking container

```bash
> bin/console debug:container | grep car
```

## Define grid structure for entity

`config/packages/_sylius.yaml`

```bash
sylius_grid:
    grids:
        app_admin_car_category:
            driver:
                name: doctrine/orm
                options:
                    class: App\Entity\CarCategory
            fields:
                name:
                    type: string
                    label: sylius.ui.name
                enabled:
                    type: twig
                    label: sylius.ui.enabled
                    options:
                        template: "@SyliusUi/Grid/Field/enabled.html.twig"
            actions:
                main:
                    create:
                        type: create
                item:
                    update:
                        type: update
                    delete:
                        type: delete
```

## Define routing for entity

config/routes.yaml

```bash
app_admin_car_category:
  resource: |
    alias: app.car_category
    section: admin
    path: admin/car-category
    templates: SyliusAdminBundle:Crud
    redirect: update
    grid: app_admin_car_category
    vars:
        all:
            subheader: app.ui.car_category
        index:
            icon: 'file image outline'
  type: sylius.resource
```

## Checking route

```bash
> php bin/console debug:router
```

## Reference

<https://docs.sylius.com/en/1.6/cookbook/entities/custom-model.html>