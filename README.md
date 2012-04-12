NelmioApiDocBundle
==================

The **NelmioApiDocBundle** bundle allows you to generate a decent documentation for your APIs.


## Installation ##

Register the namespace in `app/autoload.php`:

    // app/autoload.php
    $loader->registerNamespaces(array(
        // ...
        'Nelmio'          => __DIR__.'/../vendor/bundles',
    ));

Register the bundle in `app/AppKernel.php`:

    // app/AppKernel.php
    public function registerBundles()
    {
        return array(
            // ...
            new Nelmio\ApiDocBundle\NelmioApiDocBundle(),
        );
    }

Import the routing definition in `routing.yml`:

    # app/config/routing.yml
    NelmioApiDocBundle:
        resource: "@NelmioApiDocBundle/Resources/config/routing.yml"
        prefix:   /

Enable the bundle's configuration in `app/config/config.yml`:

    # app/config/config.yml
    nelmio_api_doc: ~


## Usage ##

The main problem with documentation is to keep it up to date. That's why the **NelmioApiDocBundle**
uses introspection a lot. Thanks to an annotation, it's really easy to document an API method.

### The ApiDoc() annotation ###

The bundle provides an `ApiDoc()` annotation for your controllers:

``` php
<?php

namespace Your\Namespace;

class YourController extends Controller
{
    /**
     * @ApiDoc(
     *  resource="true",
     *  description="This is a description of your API method",
     *  filters={
     *      {"name"="a-filter", "dataType"="integer"},
     *      {"name"="another-filter", "dataType"="string", "pattern"="(foo|bar) ASC|DESC"}
     *  }
     * )
     */
    public function getAction()
    {
    }

    /**
     * @ApiDoc(
     *  description="Create a new Object",
     *  formType="Your\Namespace\Form\Type\YourType"
     * )
     */
    public function postAction()
    {
    }
}
```

The following parameters are recognized:

* `resource`: whether the method describes a resource or not;

* `description`: a description of the API method;

* `filters`: an array of filters;

* `formType`: the Form Type associated to the method, useful for POST|PUT methods.

Each _filter_ has to define a `name` parameter, but other parameters are free. Filters are often optional
parameters, and you can document them as you want, but keep in mind to be consistent for the whole documentation.

If you set a `formType`, then the bundle automatically extracts parameters based on the given type,
and determines for each parameter its data type, and if it's required or not.

The bundle will also get information from the routing definition (`requirements`, `pattern`, etc).


### Documentation on-the-fly ###

By calling an URL with the parameter `_doc=1`, you will get the corresponding documentation if available.


### Web Interface ###

You can browse the whole documentation at: `http://yourproject/api/doc`.

![](https://github.com/nelmio/NelmioApiDocBundle/raw/master/Resources/doc/webview.png)

![](https://github.com/nelmio/NelmioApiDocBundle/raw/master/Resources/doc/webview2.png)


### Command ###

A command is provided in order to dump the documentation in `json`, `markdown, or `html`.

    php app/console api:doc:dump [--format="..."]

The `--format` option allows to choose the format (default is: `markdown`).


## Credits ##

The design is heavily inspired by the [swagger-ui](https://github.com/wordnik/swagger-ui) project.