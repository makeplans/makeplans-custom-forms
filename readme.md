# MakePlans Custom Forms

* [Get started](#get-started)
* [Complete examples](#complete-examples)
* [Available standard fields and custom field types](#fields)
* [Advanced usage](#advanced-usage)

## Get started

Booking, person, service, event, resource and category supports adding custom fields using a customized form. The form is built with [Liquid](http://liquidmarkup.org). Please read the [Liquid documentation](https://github.com/Shopify/liquid/wiki) for more information. You can use HTML and all standard Liquid syntax. In additional to the standard Liquid tags MakePlans has made available various standard fields (for booking and person in public booking form) as well as custom field types (for booking, person, service, event, resource and category).

There are two types of forms:

1. **The booking form on the public booking site.**
This allows you to create a custom form for customers making a booking. You can change the label of the standard fields and add custom fields.

2. **Additional form fields in the administration system.**
This allows you to add custom fields to a booking, person, service, event, resource or category form in the administration system. You do not need to add standard fields to these forms, whatever you add is in addition to the standard fields. The custom fields can have a different label than on the public booking site but the field name must be identical. You can also add custom fields here that are not available on the public booking site.

## Complete examples

### Public booking form

Default form:

```
{% name %}
{% phone %}
{% email %}
{% count %}
{% note %}
```

Count is only included if the service allows more than 1 person per reservation.

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
{% count %}
{% note %}
```

Custom label for phone number and custom checkbox field:

```
{% name %}
{% phone 'Mobile phone number' %}
{% checkbox 'Do you want lunch when you arrive?', 'lunch', 'checked="checked"' %}
```

Make email and phone number required:

```
{% name %}
{% phone nil, 'data-required="true"' %}
{% email nil, 'data-required="true"' %}
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

Standard fields:
* [Name](#name)
* [Email](#email)
* [Phone](#phone)
* [Count](#count)
* [Note](#note)
* [Date of Birth](#date-of-birth)
* [National Id Number](#national-id-number)
* [Street](#street)
* [Postal code](#postal-code)
* [City](#city)
* [State](#state)
* [Country](#country)
* [Terms](#terms-acceptance)
* [Marketing concent](#opt-in-for-marketing)

Custom field types:
* [Text](#text)
* [Checkbox](#checkbox)
* [Select](#select)
* [Textarea](#textarea)
* [Hidden](#hidden)
* [Date](#date)
* [URL](#url)


### Standard fields - public booking form

These fields are only available in the public booking form. They are already present in the person administration form.

For all these fields you can omit the label and it will use the standard label for the specified field. `{% name %}` will produce a label with 'Name'.

All standard fields are saved to the person except for count and note which is saved to the booking.

Important: You can only define these fields once in a form. If you add two name or notes fields you will lose input data. So for example to have multiple notes fields you can add additional [custom fields](#custom-fields---all-forms), but these will then be unique separate fields and not related to the standard MakePlans notes field.

Full syntax: `{% field 'label', 'html_attributes', 'options' %}`

The `html_attributes` and `options` parameters are optional.

#### Name

Basic example: `{% name %}`

Custom label example: `{% name 'Full name' %}`

#### Email

Basic example: `{% email %}`

Custom label example: `{% email 'E-mail address' %}`

#### Phone

Basic example: `{% phone %}`

Custom label example: `{% phone 'Cellular' %}`

#### Count

Basic example: `{% count %}`

Custom label example: `{% count 'How many are coming?' %}`

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

#### Terms acceptance

A checkbox for accepting terms.

Basic example: `{% terms_accepted %}`

Custom label example: `{% terms_accepted 'I accept' %}`

You can add your terms using `<p>{{ booking.client.terms }}</p>`.

#### Opt in for marketing

A checkbox for giving concent to adding to third party newsletter services.

Basic example: `{% opt_in_marketing %}`

Custom label example: `{% opt_in_marketing 'Yes but do not spam me' %}`


### Custom fields - all forms

Custom fields can be used in both the public booking form and all administration forms.

Example syntax (exact syntax differs between custom field types): `{% custom_field_type 'label', 'field_name', 'html_attributes', 'options' %}`

The `html_attributes` and `options` parameters are optional. `field_name` is used internally for key identification and also in all API as well as data exports.

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

Extended example: `{% select 'Favourite colour', 'colour', ['Red','Yellow','Blue','Green'], 'class="funkyclass"', '{"selected":"Blue"}' %}`

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

#### URL

A URL field will display a clickable URL.

Syntax: `{% url 'label', 'field_name', 'html_attributes', 'options' %}`.

Example: `{% url 'Website', 'website_link' %}`

## Advanced usage

### Localisation (i18n)

You can use `locale` to render output based on language. For example:

`
{% if locale == 'nb' %}
Viking
{% else %}
Not viking
{% endif %}
`

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

In addition you can have basic client side requirement of filling out fields. This is done by setting `data-required` attribute to `true`. There is a HTML5 attribute for required fields but as browser behaviour differs and it is not supported in all browsers we recommend that you specify `data-required="true"` instead of `required="true"`.

Standard field example: `{% street 'Your street', 'data-required="true"' %}`

Custom field example: `{% text 'Car model', 'car_model', 'data-required="true"' %}`

Please be aware that this is only done client side in the browser. We do not do any server side verification of these additional required fields.

### Use standard labels for standard fields

If you need to override HTML attributes or set options for a standard field you do not need to set a custom label. You can use the standard system label by setting the label to `nil`: `{% phone nil, 'data-required="true"' %}`.

### Additional options

With the parameter `options` you can define object specific configurations.

#### Save to a child object

By default all custom fields are saved to the main object in the form (for example booking in the public booking form). You can save a field to another object, for example a person instead of the booking, by specifying that a field should be saved to another object: `'{"object":"person"}'`. Please note that this only works in the public booking form and applies only to the person when saving a booking.

#### Select box

To set the default selected option for a select box: `'{"selected":"Value"}'`.