# Select
​
``jello-select`` is a control that provides a way to select one option between a list of options.
​
![Select](./assets/screenshot.png)
​
## Installation
​
```shell
npm i @jello/ds
```
​
## Usage
​
You can import the library and the Jello Theme to stylize the component:
​
```JavaScript
import '@jello/ds/theme';
import '@jello/ds/select';
```
​
And use in the DOM like this:
​
```html
<!--
    const options = [
        { label: "Option 1", value: "value-1" },
        { label: "Option 2", value: "value-2" },
        { label: "Option 3", value: "value-3" }
    ];
-->
<jello-select
    label="Select an option"
    options='[{"label":"Option 1","value":"value-1"},{"label":"Option 2","value":"value-2"},{"label":"Option 3","value":"value-3"}]'
></jello-select>
```
​
> **Note**
> The component is registered globally, so you don't need to import any variable.
​
## Attributes
​
| Name               | Description                                                                                           | Type    | Default Value |
|--------------------|-------------------------------------------------------------------------------------------------------|---------|---------------|
| autocomplete       | Suggests values based on previously entered values by the user                                        | String  | ''            |
| autofocus          | Automatically focus the control when the page is loaded                                               | Boolean | false         |
| block              | Full width layout                                                                                     | Boolean | false         |
| disabled           | Disable the component and apply disabled style                                                        | Boolean | false         |
| error-message      | Error message text when ``mark-as-error`` is set to true                                              | String  |               |
| help-message       | Optional hint message text                                                                            | String  |               |
| label (\*)         | The label of the control                                                                              | String  |               |
| mark-as-error      | Apply error style to the component                                                                    | Boolean | false         |
| options            | Array of objects with options data. [See How to feed the jello-select](#how-to-feed-the-jello-select) | Array   | []            |
| required           | Marks the label with an asterisk. Only presentational                                                 | Boolean | false         |
| selected-index     | The index of the selected element                                                                     | Number  | -1            |
| validate-on-blur   | Calls validate method when the component loses the focus                                              | Boolean | false         |
| validate-on-change | Calls validate method when the selected option changes                                                | Boolean | false         |
| value              | The value of the selected option when there is one. Otherwise empty string                            | String  | ''            |
​
(\*) Required attribute
​
## JavaScript Methods and properties
​
| Property      | Description                                                                                                    |
| ------------- | -------------------------------------------------------------------------------------------------------------- |
| isValid (\*)  | Returns false if selected value is not valid according to all specified conditions (mark-as-error, rules, ...) |
| rules         | Array of functions through which the input value will be passed to validate it. [More info](#rules)            |
​
(\*) Read only
​
| Method        | Description                                                                                                    |
| ------------- | -------------------------------------------------------------------------------------------------------------- |
| validate      | Checks all validations conditions and sets mark-as-error to true if any condition fails                        |
​
​
## Custom events
​
| Event name           | Description                                   | Detail |
| -------------------- | --------------------------------------------- | ------ |
| jello-select-change  | Dispatched when the selected option changes.  |        |
​
## How to feed the jello-select
The jello-select component needs a list of options.
The options must be an array of objects.
Each object of the array must have a mandatory ``label`` and ``value`` properties, and optionally a ``disabled`` one. Example:
​
```js
const options = [
    { label: "Option 1", value: "value-1" }, // disabled is false by default
    { label: "Option 2", value: "value-2", disabled: false },
    { label: "Option 3", value: "value-3", disabled: true }
]
```
​
It is also possible to organize options in groups.
​
```js
const options = [
    {
        label: 'Europe',
        options: [
            { label: 'Spain', value: 'ES' },
            { label: 'Switzerland', value: 'CH' }
        ]
    },
    {
        label: 'Asia',
        options: [
            { label: 'India', value: 'IN' },
            { label: 'Thailand', value: 'TH' },
        ]
    },
    {
        disabled: true,
        label: 'America',
        options: [ // disabled the full group
            { label: 'Canada', value: 'CA' },
            { label: 'Alaska', value: 'AK' },
        ]
    }
];
```
​
>
> **Important!**
> If any object of the array is not correctly formed, an error will be thrown.
>
​
>
> **Note**
> If two or more options has the same value, setting the value will produce unexpected results.
> Use selectedIndex in this case to specify the option you want to select.
>
​
​
### Setting the options by attribute
You can specify the list of options by passing a stringy json to by attribute. Example:
​
```html
<!--
    const options = [
        { label: "Option 1", value: "value-1" },
        { label: "Option 2", value: "value-2" },
        { label: "Option 3", value: "value-3" }
    ];
-->
<jello-select
    label="Select an option"
    options='[{"label":"Option 1","value":"value-1"},{"label":"Option 2","value":"value-2"},{"label":"Option 3","value":"value-3"}]'
></jello-select>
```
​
### Setting the option by JavaScript
Alternatively you can specify the list of options programmatically using JavaScript
​
```html
<jello-select label="Select an option"></jello-select>
​
<script>
    const options = [
        { label: "Option 1", value: "value-1" },
        { label: "Option 2", value: "value-2" },
        { label: "Option 3", value: "value-3" }
    ];
​
    document.querySelector('jello-select').options = options;
</script>
```
​
​
## Rules
​
Array of functions through which the select value will be passed to validate it.
Each function receive the select value as an argument and must return either true or the error message to be shown.
​
How to use a rule:
​
```javascript
const isEqual = (message, response) => value => {
    if (value === response) {
        return { isValid: true };
    }
    return { isValid: false, message };
};
​
const select = document.querySelector('jello-select');
const errorMessage = 'Wrong answer';
const validOption = 'diplodocus';
​
const rules = [
    isEqual(errorMessage, validOption)
];
​
document.querySelector('jello-select').rules = rules;
​
```
​
> **Important!**
> Each validation rule is processed in order.
> All the rules will be processed regardless if one of them fails.
> The shown error message will be the one from the first failed rule.
