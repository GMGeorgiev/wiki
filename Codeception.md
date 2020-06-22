# Install via Composer
```
composer require "codeception/codeception" --dev
composer require --dev codeception/module-webdriver
composer require --dev codeception/module-db
```
## Setup after composer install
Before you start writing your own tests, you have to setup the folder infrastructure. This is done automatically by Codeception, you only have to run the following command:
```
php vendor\codeception\codeception\codecept bootstrap
```

### Add _bootstrap.php in your /tests folder with your autoloader reuired inside. Add:
> **bootstrap: _bootstrap.php**
### In codeception.yml

# Acceptance Test
## Local testing infrastructure - Selenium
To run Selenium Server you need Java as well as Chrome or Firefox browser installed.

* Download [Selenium Standalone Server](http://docs.seleniumhq.org/download/)
* To use Chrome, install [ChromeDriver](https://sites.google.com/a/chromium.org/chromedriver/getting-started). To use Firefox, install [GeckoDriver](https://github.com/mozilla/geckodriver).
* Launch the Selenium Server: ```java -jar selenium-server-standalone-3.xx.xxx.jar```. To locate Chromedriver binary use ```-Dwebdriver.chrome.driver=./chromedriver.exe``` option **before the java command**. For Geckodriver use ```-Dwebdriver.gecko.driver=./geckodriver.exe```.
* Configure this module (in acceptance.suite.yml) by setting url and browser:

## Codeception - Acceptance suite configuration
You have to configure Codeception to use the webdriver and not the PHPBrowser for test execution, like shown:
```
actor: AcceptanceTester
modules:
  enabled:
  - WebDriver:
      url: http://localhost/mvc/public
      browser: chrome
  - \Helper\Acceptance
    
```

### Create an Acceptance test
```
php vendor\codeception\codeception\codecept generate:cest acceptance HomePage
```

### Start the selenium server
Launch Selenium Server from the folder where your selenium server and chrome drive resides before executing tests.
```
java -jar -Dwebdriver.chrome.driver=./chromedriver.exe selenium-server-standalone-3.141.59.jar
```

### Execute all Acceptance tests
```
php vendor\codeception\codeception\codecept run acceptance
```

# Unit testing 
## Creation
```
php vendor\codeception\codeception\codecept generate:test unit UserTest
```

## Execution
```
php vendor\codeception\codeception\codecept run unit UserTest
```

### Create PageObject:
```
php vendor/codeception/codeception/codecept g:pageobject acceptance CompleteOrder
```