[![Build Status](https://travis-ci.org/vulcandigital/silverstripe-sendgrid.svg?branch=master)](https://travis-ci.org/vulcandigital/silverstripe-sendgrid)

## silverstripe-sendgrid
A module to assist developers in sending template emails via SendGrid

## Requirements
* silverstripe/framework: ^4.0

## Installation
```bash
composer require vulcandigital/silverstripe-sendgrid *
```

## Configuration
**mysite/_config/sendgrid.yml:**
```yml
Vulcan\SendGrid\SendGrid:
  api_key: 'REPLACE-WITH-YOUR-API-KEY'
```

## Usage
```php
$sendGrid = \Vulcan\SendGrid\SendGrid::create();
$sendGrid->setSubject("We have a sale for you!");
$sendGrid->setFrom('marketing@example.com');
$sendGrid->setFromName('My Site');
$sendGrid->setReplyTo('sales@example.com');
$sendGrid->addRecipient('reece@vulcandigital.co.nz', 'Reece Alexander', [
    ':salutation' => 'Mr',
    ':button_link' => 'https://example.com/store/offer?id=aASdGdjnklashewjk12321hjkasd213'
]);
$sendGrid->setBody("<p>We thought you'd like this awesome t-shirt!</p>");
$sendGrid->setTemplateId('your-template-id');
$sendGrid->addAttachment(Image::get()->first());
$sendGrid->send();
```

You can add as many recipients as you want.

### Attachments
You can add as many attachments you want totalling up to 30 MB. The attachment must be a `File` object or a subclass of it such as itself or `Image`.

```php
$file = Image::get()->first();
$sendGrid->addAttachment($file, $filename = null, $forcePublish = false); 
```

or you can use an absolute path to a file instead:

```php
$sendgrid->addAttachment('/public_html/path/to/image.png'));
$sendgrid->addAttachment(Controller::join_links(Director::baseFolder(), '/path/to/image2.png'));
```

If you provide `$filename`, make sure you provide the correct extension as well to prevent any errors

If the provided file is a `File` object and `$forcePublish` is set to `true` _and_ the `File` you have provided has not been published, it will be forcibly published.

### Scheduling
You can schedule emails to be sent at a later date:

```php
$sendGrid->setScheduleTo(DBDatetime::now()->getTimestamp() + 3600); // Schedule to send in 1 hour
```

> **Important:** Ensure that you have specified your correct timezone in your SendGrid account's settings, otherwise this may have unexpected results.
> 
> Your database timezone should also match the timezone you have specified in your account. See [Core Environment Variables](https://docs.silverstripe.org/en/4/getting_started/environment_management/#core-environment-variables) for information on how to modify the timezone used by your database.
>
> It is always advised when dealing with dates and times in SilverStripe to use the functionality it has provided you as shown in the example above.

## License
[BSD-3-Clause](LICENSE.md) - [Vulcan Digital Ltd](https://vulcandigital.co.nz)