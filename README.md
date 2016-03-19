### What is it?

This bundle provides features that help you running your symfony2 testsuite more efficiently with isolated tests.

It provides a `StaticDriver` that will wrap your originally configured `Driver` class (like `DBAL\Driver\PDOMysql\Driver`) and keeps a database connection statically in the current php process.

With the help of a PHPUnit listener class it will begin a transaction before every testcase and roll it back again after the test finished for all configured DBAL connections. This results in a performance boost as there is no need to rebuild the schema, import a backup SQL dump or re-insert fixtures before every testcase. As long as you avoid issuing queries that result in implicit transaction commits (Like `ALTER TABLE`, `DROP TABLE` etc) your tests will be isolated and all see the same database state.

Also it includes a `StaticArrayCache` that can be automatically configured as meta data & query cache for all EntityManagers. This improved the speed and memory usage for my testsuites dramatically!

### How to install and use this Bundle?

1. install via composer

    ```json
    ...
    ```

2. Enable the bundle for your test environment in your `AppKernel.php`

    ```php
    if (in_array($env, ['dev', 'test'])) {
        ...
        if ($env === 'test') {
            $bundles[] = new \DAMA\DoctrineTestBundle\DMDoctrineTestBundle();
        }
    }
    ```
    
3. Add the PHPUnit test listener in your xml config (e.g. `app/phpunit.xml`)

    ```xml
    <listeners>
        <listener class="\DAMA\DoctrineTestBundle\PHPUnit\PHPUnitStaticDbConnectionListener" file="../vendor/dama/doctrine-test-bundle/src/DAMA/DoctrineTestBundle/PHPUnit/PHPUnitStaticDbConnectionListener.php" />
    </listeners>
    ```
    
### Configuration

The bundle exposes a configuration that looks like this by default:
    
```yaml
dama_doctrine_test:
  enable_static_connection: true
  enable_static_meta_data_cache: true
  enable_static_query_cache: true
```
    