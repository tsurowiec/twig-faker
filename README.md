# TwigFaker

An extension to add the [faker] library into twig templates, providing a fast efficient way to create dummy data within your templates. Here's a quick example:

Configure your fake data type `factories/person.php`

```
<?php

return [
    'first_name' => $faker->firstName,
    'last_name' => $faker->lastName,
    'avatar' => $faker->imageUrl(300, 300)
];
```

Use in your templates

```
{% for person in fake('person', 20) %}
    <p>Hello {{ person.first_name }} {{ person.first_name }}</p>
    <img src="{{ person.avatar }}">
{% endfor %}
```

## Usage

### Step 1: Install

Pull this package in through Composer.

```
"require": {
    "alanablett/twig-faker": "~1.0"
}
```

### Step 2: Register the TwigFaker extension with twig

```
$twig->addExtension(new Ablett\TwigFaker\TwigFakerExtension);
```

### Step 3: Create a Factories File

TwigFaker isn't magic. You need to describe the type of data that should be generated.

Each factory file you create will automatically have access to a `$faker` variable. This is just a normal factory that you would create by doing `Faker\Factory::create`.

Within a `factories` directory in the root of your project, you may create any number of PHP files that will automatically be loaded by TwigFaker. Why don't you start with a generic `factories/person.php` file.

```
<?php

return [
    'first_name' => $faker->firstName,
    'last_name' => $faker->lastName,
    'avatar' => $faker->imageUrl(300, 300)
];
```

### Step 4: Call your new factory in a twig template

With your new factory defined, you can now use it in your twig template.

```
{% for person in fake('person', 20) %}
    <p>Hello {{ person.first_name }} {{ person.first_name }}</p>
    <img src="{{ person.avatar }}">
{% endfor %}
```

The first parameter passed to the `fake` method is the factory name to be used. The second parameter is the number of fake instances you want to loop through (default is 1 if no parameter is passed).

**Note:** You can organise your factories in folders such as `factories/subfolder/another/example.php`

```
{% for example in fake('subfolder/another/example', 20) %}
    <p>{{ example.value }}</p>
{% endfor %}
```

## Usage With Sculpin

### Step 1: Install

Pull this package in through Composer by adding it to the `sculpin.json` file.

```
"require": {
    "alanablett/twig-faker": "~1.0"
}
```

### Step 2: Register the TwigFaker extension with sculpin

To use the extension in sculpin we have to add it to the extensions that sculpin knows about. This is done by adding it to `app/config/sculpin_kernel.yml`

```
services:
  twig.extension.faker:
    class: Ablett\TwigFaker\TwigFakerExtension
    tags:
      - { name: twig.extension }
```

Follow the further steps above to create new factories and use them in your templates.

[faker]: https://github.com/fzaninotto/Faker