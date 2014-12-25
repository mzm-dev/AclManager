# AclManager for CakePHP 2.x

This plugins allows you to easily manage your permissions in CakePHP 2.x through the Acl module.

## Features

* Managing permissions for each node
* Updating Database with missing AROs (Users, Roles, ...)
* Updating Database with missing ACOs (Controller actions)
* Revoking all permissions

## Requirements

* CakePHP 2.x

## How to install

### 1. Set up your Acl environment

* Install SQL tables through Cake Console
 
 ```code
 ./Console/cake acl initdb
 ```
 
* parentNode() method on your requester models

See: [CakePHP: Simple ACL Controlled Application](http://book.cakephp.org/2.0/en/tutorials-and-examples/simple-acl-controlled-application/simple-acl-controlled-application.html)

### 2. Configure Auth in your AppController

It should look something like this:

```php
public $helpers = array('Html', 'Form', 'Session');
    public $components = array(
        'Session', 'Acl',
        'Auth' => array(
            'authorize' => array(
                'Controller',
                'Actions' => array('actionPath' => 'controllers')
            ),
            'loginRedirect' => array(
                'controller' => 'posts',
                'action' => 'index'
            ),
            'logoutRedirect' => array(
                'controller' => 'pages',
                'action' => 'display',
                'home'
            ),
            'authenticate' => array(
                'Form' => array(
                    'fields' => array(
                        'username' => 'username', //Default is 'username' in the userModel
                        'password' => 'password'  //Default is 'password' in the userModel
                    )
                )
            ),
            'authError' => "Sorry, you're not allowed to do that."
        )
    );

    function isAuthorized($user) {
        #return false; 
        return $this->Auth->loggedIn(); //disable after final setup
    }
```

### 3. Download AclManager

#### Manually

Download the stable branch (https://github.com/FMCorz/AclManager/archive/stable.zip) and paste the content in your `app/Plugin/` directory.

#### With Composer

1. [Install composer](http://getcomposer.org/doc/00-intro.md#locally) in the `app/` folder of your project.
2. Add `"fmcorz/acl-manager": "stable"` to your `require` key in your `composer.json` file. ([More about this](http://getcomposer.org/doc/01-basic-usage.md#the-require-key))
3. Run `php composer.phar install` to install the plugin.

[Composer documentation](http://getcomposer.org/doc/)

### 4. Configure the plugin

See `AclManager/Config/bootstrap.php`

AclManager.aros : write in there your requester models aliases (the order is important)

### 5. Enable the plugin

In `app/Config/bootstrap.php`

    CakePlugin::load('AclManager', array('bootstrap' => true));

### 6. Login with an existing user

The plugin conflicts with `$this->Auth->allow()`, do not use it. Just make sure that you are logged in.

### 7. Access the plugin at `/acl_manager/acl`

   * Update your AROs and ACOs
   * Set up your permissions (do not forget to enable your own public actions!)
   
### 8. Disable the authorizer Controller

Or uncomment `return false` in `AppController::isAuthorized()`

### 9. You're done!

Enjoy!

Licence
-------

Licensed under the [MIT License](http://www.opensource.org/licenses/mit-license.php)
