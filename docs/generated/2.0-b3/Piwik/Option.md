<small>Piwik</small>

Option
======

Convenient key-value storage for user specified options and temporary data that needs to be persisted beyond one request.

Description
-----------

### Examples

**Setting and getting options**

    $optionValue = Option::get('MyPlugin.MyOptionName');
    if ($optionValue === false) {
        // if not set, set it
        Option::set('MyPlugin.MyOptionName', 'my option value');
    }

**Storing user specific options**

    $userName = // ...
    Option::set('MyPlugin.MyOptionName.' . $userName, 'my option value');

**Clearing user specific options**

    Option::deleteLike('MyPlugin.MyOptionName.%');


Methods
-------

The class defines the following methods:

- [`get()`](#get) &mdash; Returns the option value for the requested option `$name`.
- [`set()`](#set) &mdash; Sets an option value by name.
- [`delete()`](#delete) &mdash; Deletes an option.
- [`deleteLike()`](#deletelike) &mdash; Deletes all options that match the supplied pattern.
- [`clearCache()`](#clearcache) &mdash; Clears the option value cache and forces a reload from the Database.
- [`getValue()`](#getvalue)
- [`setValue()`](#setvalue)
- [`deleteValue()`](#deletevalue)
- [`deleteNameLike()`](#deletenamelike)
- [`autoload()`](#autoload) &mdash; Initialize cache with autoload settings.

<a name="get" id="get"></a>
### `get()`

Returns the option value for the requested option `$name`.

#### Signature

- It accepts the following parameter(s):
    - `$name`
- _Returns:_ The value or false, if not found.
    - `string`
    - `bool`

<a name="set" id="set"></a>
### `set()`

Sets an option value by name.

#### Signature

- It accepts the following parameter(s):
    - `$name`
    - `$value`
    - `$autoload`
- It does not return anything.

<a name="delete" id="delete"></a>
### `delete()`

Deletes an option.

#### Signature

- It accepts the following parameter(s):
    - `$name`
    - `$value`
- It does not return anything.

<a name="deletelike" id="deletelike"></a>
### `deleteLike()`

Deletes all options that match the supplied pattern.

#### Signature

- It accepts the following parameter(s):
    - `$namePattern`
    - `$value`
- It does not return anything.

<a name="clearcache" id="clearcache"></a>
### `clearCache()`

Clears the option value cache and forces a reload from the Database.

#### Description

Used in unit tests to reset the state of the object between tests.

#### Signature

- It returns a(n) `void` value.

<a name="getvalue" id="getvalue"></a>
### `getValue()`

#### Signature

- It accepts the following parameter(s):
    - `$name`
- It does not return anything.

<a name="setvalue" id="setvalue"></a>
### `setValue()`

#### Signature

- It accepts the following parameter(s):
    - `$name`
    - `$value`
    - `$autoLoad`
- It does not return anything.

<a name="deletevalue" id="deletevalue"></a>
### `deleteValue()`

#### Signature

- It accepts the following parameter(s):
    - `$name`
    - `$value`
- It does not return anything.

<a name="deletenamelike" id="deletenamelike"></a>
### `deleteNameLike()`

#### Signature

- It accepts the following parameter(s):
    - `$name`
    - `$value`
- It does not return anything.

<a name="autoload" id="autoload"></a>
### `autoload()`

Initialize cache with autoload settings.

#### Signature

- It returns a(n) `void` value.
