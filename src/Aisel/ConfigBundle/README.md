AiselConfigBundle
-----------------------------------
Administration settings interface for Symfony2 projects. Can be easily integrated with SonataAdminBundle

Notes
-------------
Bundle not stable, only for development purposes

Documentation
-------------

1.  Add to AppKernel.php<br/>
```bash
    new Aisel\ConfigBundle\AiselConfigBundle(),
```
And to config.yml<br/>
```bash
aisel_config:
    settings_route: /settings/{editLocale}
    route_prefix: config_ # ex: config_contact
    entities:
        homepage:
            order: 0
            controller: TestConfigBundle:ConfigHomepage:modify
```

2. Then add routes to routing.yml<br/>
```bash
aisel_config:
    resource: "@AiselConfigBundle/Resources/config/routing.yml"
    prefix:   /
```

3. Create typical form class<br/>
```bash
<?php

namespace TestConfig\Bundle\Form\Type;

use Symfony\Component\Form\AbstractType;
use Symfony\Component\Form\FormBuilderInterface;

class ConfigHomepageType extends AbstractType
{

    /**
     * {@inheritDoc}
     */
    public function buildForm(FormBuilderInterface $builder, array $options)
    {
        $builder
            ->add('content', 'text', array('label' => 'Content'))
            ->add('save', 'submit', array('label' => 'Save', 'attr' => array('class' => 'btn btn-primary')));
    }

    /**
     * {@inheritDoc}
     */
    public function getName()
    {
        return 'config_homepage';
    }

}
```

4. Create and extend controller from the class SettingsController<br/>
   then set $form variable with the path to your ConfigHomepageType
```bash
<?php

namespace Test\ConfigBundle\Controller;

use Aisel\ConfigBundle\Controller\SettingsController;

class ConfigHomepageController extends SettingsController
{

    public $form = "\Test\Config\Bundle\Form\Type\ConfigHomepageType";

    /**
     * {@inheritdoc }
     */
    protected function getTemplateVariables()
    {
        return $this->templateVariables;
    }

}
```
5. Check that route available with router:debug
```bash
php app/console router:debug
config_homepage          ANY    ANY    ANY  /settings/{editLocale}/homepage
```
editLocale is your current locale param, controller should be available by:<br/>
/settings/en/homepage, where en is your locale parameter

Multiple locales
-----------------------------------
Bundle supports multiple locales, use "locales" param separated by "|"
```bash
locales: en|ru|es|de
```
SonataAdminBundle compatibility
-----------------------------------
It also works fine with SonataAdminBundle, to make it work<br/>
   change getTemplateVariables() function in your controller as show bellow
```bash
    protected function getTemplateVariables()
    {
        $this->templateVariables['base_template'] = 'AiselSettingsBundle::layout.html.twig'; //Your sonata layout template
        $this->templateVariables['admin_pool'] = $this->container->get('sonata.admin.pool');
        $this->templateVariables['blocks'] = $this->container->getParameter('sonata.admin.configuration.dashboard_blocks');
        return $this->templateVariables;
    }
```


MIT License
-----------------------------------

License can be found [here](https://github.com/ivanproskuryakov/Aisel/blob/master/LICENSE).

Authors
-----------------------------------

Part of Aisel package, was originally created by [Ivan Proskuryakov](http://www.magazento.com).
List of [contributors](https://github.com/ivanproskuryakov/AiselConfigBundle/graphs/contributors).