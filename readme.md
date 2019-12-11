# MakePlans Custom Forms - Version 1.1

## Get started

Booking, person, service, event, resource and category supports adding custom attributes via a customized form. The form is built with [Liquid](http://liquidmarkup.org). Please read the [Liquid documentation](https://github.com/Shopify/liquid/wiki) for more information. You can use HTML and all standard Liquid syntax. In additional to the standard Liquid tags MakePlans has made available various standard fields (for booking and person in public booking form) as well as custom field types (for booking, person, service, event, resource and category).

## Complete examples

### Public booking form

Default form:

```
{% name %}
{% phone %}
{% email %}
{% note %}
```

Form with all standard fields:

```
{% name %}
{% phone %}
{% email %}
{% date_of_birth %}
{% national_id_no %}
{% street %}
{% postal_code %}
{% city %}
{% state %}
{% country_code %}
{% note %}
```

Custom label for phone number and custom checkbox field:

```
{% name %}
{% phone 'Mobile phone number' %}
{% checkbox 'Do you want lunch when you arrive?', 'lunch', 'checked="checked"' %}
```

### Service administration form

```
{% textarea 'Information from supplier', 'supplier_info' %}
{% select 'Expertise level', 'level', ['Beginner','Average','Expert'] %}
```

With default select value:

```
{% textarea 'Information from supplier', 'supplier_info' %}
{% select 'Expertise level', 'level', ['Beginner','Average','Expert'], '', '{"selected":"Average"}' %}
```


## Fields

The html_attributes attribute is optional.

### Standard fields - public booking form

For all these fields you can omit the label and it will use the standard label for the specified field. `{% name %}` will produce a label with 'Name'.

These fields are only available in the public booking form. They are already present in the person administration form.

Important: You can only define these fields once in a form. If you add two name or notes fields you will lose input data. To add have multiple notes fields you can add additional [custom fields](#custom-fields---all-forms).

Full syntax: `{% field 'label', 'html_attributes', 'options' %}`

#### Name

Basic example: `{% name %}`

Custom label example: `{% name 'Full name' %}`

#### Email

Basic example: `{% email %}`

Custom label example: `{% email 'E-mail address' %}`

#### Phone

Basic example: `{% phone %}`

Custom label example: `{% phone 'Cellular' %}`

#### Note

Basic example: `{% note %}`

Custom label example: `{% note 'Additional details' %}`

#### Date of Birth

Basic example: `{% date_of_birth %}`

Custom label example: `{% date_of_birth 'Your birthday' %}`

#### National Id Number

Basic example: `{% national_id_no %}`

Custom label example: `{% national_id_no 'Social Security Number' %}`

#### Street

Basic example: `{% street %}`

Custom label example: `{% street 'Street address' %}`

#### Postal code

Basic example: `{% postal_code %}`

Custom label example: `{% postal_code 'Zipcode' %}`

#### City

Basic example: `{% city %}`

Custom label example: `{% city 'Town' %}`

#### State

Basic example: `{% state %}`

Custom label example: `{% state 'Region' %}`

#### Country

Produces a drop-down of all countries which defaults to the country specified on the account.

Basic example: `{% country %}`

Custom label example: `{% country 'Select your country' %}`

### Custom fields - all forms

Custom fields can be used in both the public booking form and all administration forms.

#### Text

Syntax: `{% text 'label', 'field_name', 'html_attributes', 'options' %}`.

Basic example: `{% text 'Describe yourself with one word', 'self_description' %}`

Extended example: `{% text 'Describe yourself with one word', 'self_description', 'data-required="true"' %}`

#### Checkbox

Syntax: `{% checkbox 'label', 'field_name', 'html_attributes', 'options' %}`.

Example: `{% checkbox 'Do you want lunch?', 'lunch', 'checked="checked"' %}`

#### Select

By default a blank option is added to ensure the user is not selecting a value by mistake. To set a different default selected value specify it in `options`.

Syntax: `{% select 'label', 'field_name', ['value1', 'value2'], 'html_attributes', 'options' %}`.

Basic example: `{% select 'Favourite food', 'food', ['Pizza','Taco','Sushi','Pinnekjøtt'] %}`

Extended example: `{% select 'Favourite colour', 'colour', [ 'Red','Yellow','Blue','Green' ], 'class="funkyclass"', '{"selected":"Blue"}' %}`

#### Textarea

Syntax: `{% textarea 'label', 'field_name', 'html_attributes', 'options' %}`.

Example: `{% textarea 'Information from supplier', 'supplier_info' %}`

#### Hidden

Syntax: `{% hidden 'field_name', 'html_attributes', 'options' %}`.

Example: `{% hidden 'secret' %}`

#### Date

A date field will trigger a datepicker on the field in any form. In addition the name of the field should be suffixed with `_at` such as `last_visit_at`. This will ensure that the value is always treated as a date. Just using `date` will only ensure so visually in the form.

Syntax: `{% date 'label', 'field_name', 'html_attributes', 'options' %}`.

Example: `{% date 'Membership date', 'became_member_at' %}`

## Advanced usage

### HTML Output

Fields will produce a `<label>` and `<input>` or other specified form field and wrapped in a container such as `<p>` or `<div>`.

Standard fields which fails validation will have the `<input>` field wrapped in a `<div class="field_with_errors">`.

There is no guaranteed way to validate custom fields. The only way to do some sort of validation is using JavaScript or HTML5 form validation which both can be skipped by the user.

### Data types

All values are stored as strings.

### HTML attributes

You can add additional HTML attributes in the `html_attributes` parameter.

### Required fields

By default the required fields are based on your verification method. Name is always required. If you have SMS verification then phone number is a required field, and if you have selected e-mail verification then e-mail address is required.

In addition you can have basic client side requirement of filling out fields. This is done by setting `data-required` attribute to `true`. There is a HTML5 attribute for required fields but as browser behaviour differs and it is not supported in all browsers we recommend that you specify `data-required="true"` instead of `required≈"true"`.

Standard field example: `{% street 'Your street', 'data-required="true"' %}`

Custom field example: `{% text 'Car model', 'car_model', 'data-required="true"' %}`

### Additional options

With the parameter `options` you can define object specific configurations.

#### Save to a child object

By default all custom fields are saved to the main object in the form (for example booking in the public booking form). You can save a field to another object, for example a person instead of the booking, by specifying that a field should be saved to another object: `'{"object":"person"}'`. Please note that this only works in the public booking form and applies only to the person when saving a booking.

#### Selectbox

To set the default selected option for a select box: `'{"selected":"Value"}'`.