# Google reCAPTCHA widget for Yii2

Based on Google reCaptcha API 2.0 and 3.0.

**NOTE**: This is a fork of https://github.com/himiklab/yii2-recaptcha-widget  @v2.1.1

The original project seems stalled, and there's some issues that needed some attention, hence the fork

Any versions released from this repo should follow the following mask for tags 2.1.1.x

If there are any updates upstream it should be able to accommodate those changes and cope with upstream versions.

-----

[![Packagist](https://img.shields.io/packagist/dt/websvc/yii2-recaptcha-widget.svg)]() [![Packagist](https://img.shields.io/packagist/v/websvc/yii2-recaptcha-widget.svg)]()  [![license](https://img.shields.io/badge/License-MIT-yellow.svg)]()

Upgrade to 2.x version
------------
Warning! Classes `ReCaptcha` and `ReCaptchaValidator` is deprecated. Please replace their to `ReCaptchaConfig`,
`ReCaptcha2` and `ReCaptchaValidator2`.

Installation
------------
The preferred way to install this extension is through [composer](http://getcomposer.org/download/).

* Either run

```
php composer.phar require --prefer-dist "websvc/yii2-recaptcha-widget" "*"
```

or add

```json
"websvc/yii2-recaptcha-widget" : "*"
```

to the `require` section of your application's `composer.json` file.

* [Sign up for an reCAPTCHA API keys](https://www.google.com/recaptcha/admin/create).

* Configure the component in your configuration file (web.php). The parameters siteKey and secret are optional.
But if you leave them out you need to set them in every validation rule and every view where you want to use this widget.
If a siteKey or secret is set in an individual view or validation rule that would overrule what is set in the config.

```php
'components' => [
    'reCaptcha' => [
        'class' => 'websvc\yii2\recaptcha\ReCaptchaConfig',
        'siteKeyV2' => 'your siteKey v2',
        'secretV2' => 'your secret key v2',
        'siteKeyV3' => 'your siteKey v3',
        'secretV3' => 'your secret key v3',
    ],
    ...
```

or use DI container:

```php
'container' => [
    'definitions' => [
        websvc\yii2\recaptcha\ReCaptcha2::className() => function ($container, $params, $config) {
            return new websvc\yii2\recaptcha\ReCaptcha2(
                'your siteKey v2',
                '', // default
                $config
            );
        },
        websvc\yii2\recaptcha\ReCaptchaValidator2::className() => function ($container, $params, $config) {
            return new websvc\yii2\recaptcha\ReCaptchaValidator2(
                'your secret key v2',
                '', // default
                null, // default
                null, // default
                $config
            );
        },
    ],
],
```

* Add `ReCaptchaValidator2` or `ReCaptchaValidator3` in your model, for example:

v2

```php

public $reCaptcha;

public function rules()
{
  return [
      // ...
      [['reCaptcha'], \websvc\yii2\recaptcha\ReCaptchaValidator2::className(),
        'secret' => 'your secret key', // unnecessary if reСaptcha is already configured
        'uncheckedMessage' => 'Please confirm that you are not a bot.'],
  ];
}
```

v3

```php
public $reCaptcha;

public function rules()
{
  return [
      // ...
      [['reCaptcha'], \websvc\yii2\recaptcha\ReCaptchaValidator3::className(),
        'secret' => 'your secret key', // unnecessary if reСaptcha is already configured
        'threshold' => 0.5,
        'action' => 'homepage',
      ],
  ];
}
```

Usage
-----
For example:

v2

```php
<?= $form->field($model, 'reCaptcha')->widget(
    \websvc\yii2\recaptcha\ReCaptcha2::className(),
    [
        'siteKey' => 'your siteKey', // unnecessary is reCaptcha component was set up
    ]
) ?>
```

v3

```php
<?= $form->field($model, 'reCaptcha')->widget(
    \websvc\yii2\recaptcha\ReCaptcha3::className(),
    [
        'siteKey' => 'your siteKey', // unnecessary is reCaptcha component was set up
        'action' => 'homepage',
    ]
) ?>
```

or

v2

```php
<?= \websvc\yii2\recaptcha\ReCaptcha2::widget([
    'name' => 'reCaptcha',
    'siteKey' => 'your siteKey', // unnecessary is reCaptcha component was set up
    'widgetOptions' => ['class' => 'col-sm-offset-3'],
]) ?>
```

v3
```php
<?= \websvc\yii2\recaptcha\ReCaptcha3::widget([
    'name' => 'reCaptcha',
    'siteKey' => 'your siteKey', // unnecessary is reCaptcha component was set up
    'action' => 'homepage',
    'widgetOptions' => ['class' => 'col-sm-offset-3'],
]) ?>
```

* NOTE: Please disable ajax validation for ReCaptcha field!

Resources
---------
* [Google reCAPTCHA v2](https://developers.google.com/recaptcha)
* [Google reCAPTCHA v3](https://developers.google.com/recaptcha/docs/v3)
