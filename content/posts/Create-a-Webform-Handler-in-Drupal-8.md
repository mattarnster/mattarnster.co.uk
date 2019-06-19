---
title: "Create a Webform Handler in Drupal 8"
date: 2017-03-10
aliases: [
  "/tutorials/create-a-webform-handler-in-drupal-8/"
]
draft: false
summary: "Follow this easy tutorial on how to build a Webform form handler in Drupal 8"
categories: ['Tutorials']
---

The newly built Webform module for Drupal 8 has a ton of improvements for users and developers; here’s how to use the Webform Handler to post form data to a third party.

## Creating a form handler
You’ll first need to create a module, or have a module already written that you’d like to integrate the handler into.

For simplicity’s sake, we’ll cover creating a new module for an example form handler.

You’ll need the following folder structure for your module:

```
▾ modules/
  ▾ custom/
    ▾ my_custom_form_handler/
      ▾ src/
        ▾ Plugin/
          ▾ WebformHandler/
```

You should then create your handler file in the _WebformHandler_ directory. We’ll call this one _ExampleFormHandler.php_, and the file should follow this template:

```php
<?php
namespace Drupal\my_custom_form_handler\Plugin\WebformHandler;
 
use Drupal\Core\Session\Account\Interface;
use Drupal\Core\Serialization\Yaml;
use Drupal\Core\Form\FormStateInterface;
use Drupal\webform\Plugin\WebformHandlerBase;
use Drupal\webform\WebformSubmissionInterface;
 
use Guzzle\Http\Client;
use Guzzle\Http\Exception\RequestException;
 
/**
 * Form submission handler.
 *
 * @WebformHandler(
 *   id = "example_form_handler",
 *   label = @Translation("Example form handler"),
 *   category = @Translation("Examples"),
 *   description = @Translation("An example form handler"),
 *   cardinality = Drupal\webform\Plugin\WebformHandlerInterface::CARDINALITY_SINGLE,
 *   results = Drupal\webform\Plugin\WebformHandlerInterface::RESULTS_PROCESSED,
 * )
 */
class ExampleFormHandler extends WebformHandlerBase {
 
  /**
   * {@inheritdoc}
   */
  public function submitForm(array &$form, FormStateInterface $form_state, WebformSubmissionInterface $webform_submission) {
    // Your code here.
  }
}
```

It really is as simple as that! The only step left is to write the code to send the submission off to a 3rd party.

```php
public function submitForm(array &$form, FormStateInterface $form_state, WebformSubmissionInterface $webform_submission) {
  $client = new Drupal::httpClient();
  $response = $client->request('POST', 'https://<your_third_party_service_here>',
    'form_params' => [
      'first_name' => $webform_submission->getData('first_name')
    ]
  );
 
  $code = $response->getStatusCode();
 
  if ($code >= 400 || $code === 0) {
    // Handle the error
  }
}
```

Thanks to the following people who have provided edits to the post on my previous website:

Joery Lemmens

John Lewis

If there is anything wrong with this post, please contact me using the links in the sidebar.
