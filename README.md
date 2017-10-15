# easyV.js

An easy to use yet comprehensive react form validation library which make your components tree clean.

## Why use

- Except the schema, adding 3 lines, then you are good to go :)
 - Clean JSX hierarchy
 - Declarative way to define your validation rule.
 - Validation rule is just a plain javascript easy to modify
 - Comprehensive validation rule

I found that the other existing react form validation solution either requires to add so many different kinds of their components or using prop extensively, they all add too much noise to the JSX hierarchy.

I think validation is something which should be decoupled from components tree as much as possible. When something happens, it just tells me whether the result is I want or not, and use that result to render the different status of field item. That's all.

So I built this library.

## Idea of easyV.js

1. Get your defined rule sets from schema.
1. Get the value to check from `onChange` method
1. The background `matchRunner` will match your schema against the value.
1. Validation result will be saved to the state, and calling `setState` automatically. Including:
    - Final value
    - Field status: `normal`, `ok`, `error`
    - Error text
    - Form status according to all the status of the fields item.

## How to use

1.  Write a schema like below, `title` is the `name` of the field item you want to check

```javascript
const formSchema = {
    title: {
        string: {
            minLength: 2,
            maxLength: 5
        }
    }
};
```
 
2. In the constructor of the form component, add this 2 lines, one for binding the schema to `easyV` and one for generating the initial state.

```javascript
constructor() {
    super();
    this.validator = new FormValidator(this, formSchema);
    this.state = this.validator.createInitialState();
  }
```

3. Bind a `onChange` event handler to your form component like this:

``` javascript
<form action="" onChange={this.handleFieldChange}>
```

4. Invoke the validation methods inside the handler:

```javascript
handleFieldChange = e => {
    e.preventDefault();
    return this.validator.validate(e.target);
  };
```

5. Wrap all your field item inside the `<FieldsBinding>` component.
```xml
<BindStatus schema={formSchema} state={this.state}>
    <TextInput
        name="title"
        label="Title"
        placeHolder="Put the title of item here"
    />
</BindStatus>
```
The `FieldsBinding` component will pass 2 `props` to your field item:
- status: `ok`, `error`, `normal`, which you can use inside the field item component to change the CSS style according the status.
- errorText: It will display the error message.

6. Then you are good to go, this is pretty much all of it.

## TODO:
[] Add the table of all rules.
[] Need to add a minimal working example.
[] Let user define the name of `status` and `errorText`
[] Let user customize the error message.
[] The `FieldItem` should be able to do more.
[] Add user's own state when calling `createInitialState()`