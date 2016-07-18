# Symfony Behat Debug Bundle

Bridge between Symfony and PhpSpec which adds PhpSpec formatters to Symfony DI Container.

## Usage

* Add package to your application dependencies

```
composer require --dev filipgolonka/symfony-behat-debug-bundle
```

* Enable bundle in your `AppKernel`:

```
// app/AppKernel.php
class AppKernel extends Kernel
{
    public function registerBundles()
    {
        if ($this->getEnvironment == 'test') {
            $bundles = array(
                // ...
                new FilipGolonka\SymfonyBehatDebugBundle\FilipGolonkaSymfonyBehatDebugBundle(),
            );

            // ...
        }
    }
}
```

* Import services configuration file to your application:

```
// app/config/services.yml
imports:
    - { resource: "@FilipGolonkaSymfonyBehatDebugBundle/Resources/config/services.yml" }
```

* Just use it in your behat contexts:

```
<?php

namespace AppBundle;

use Behat\Symfony2Extension\Context\KernelAwareContext;
use Behat\Symfony2Extension\Context\KernelDictionary;

class DataContext implements KernelAwareContext
{
    use KernelDictionary;
    
    /**
     * @Then test difference formatting
     */
    public function testDifferenceFormatting()
    {
    
        $expectedContent = ['foo' => 'bar'];
        $actualContent = ['baz' => 'bat'];
        
        if ($expectedContent != $actualContent) {
            throw new \Exception(
                $this->getContainer()->get('behat_debug.formatter')->format(
                    $this->getContainer()->get('behat_debug.differ')->compare($expectedContent, $actualContent)
                )
            );
        }
    }
}
```
