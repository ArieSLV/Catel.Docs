# Using the validation context

The *ViewModelBase* derives from *ModelBase*, thus all information here also applies to the *ViewModelBase*

Sometimes detailed information about validation is required. This is possible in Catel thanks to the ValidationContext class. The ValidationContext serves as the container for all validation results that are gathered via the available validation methods. The ValidationContext has lots of methods that all return lists of either IFieldValidationResult or IBusinessRuleValidationResult.

The examples below are shown a starter examples, but you can gather every type of validation result by using the *ValidationContext*. To retrieve the validation context of an object, use the following code:

``` {.java data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"}
var modelValidation = new MyViewModel() as IModelValidation;
var validationContext = modelValidation.ValidationContext;
```

## Getting the number of or warnings and errors

 To retrieve the total number of warnings and errors, use the following code:

``` {.java data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"}
int count = validationContext.GetValidationCount();
```

##  Getting all the field errors

To retrieve all the field errors, use the following code:

``` {.java data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"}
var fieldErrors = validationContext.GetFieldErrors();
```

## Getting all the field errors of a specific property

 To retrieve all the field errors of a specific property, use the following code:

``` {.java data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"}
var fieldErrors = validationContext.GetFieldErrors("MyProperty");
```

## Getting all the business rule warnings

To retrieve all the business rule warnings:

``` {.java data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"}
var businessRuleWarnings = validationContext.GetBusinessRuleWarnings();
```

## Getting all the business rule errors with a specific tag

To retrieve all the business rule errors with a specific tag, use the following code:

``` {.java data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"}
var businessRuleErrors = validationContext.GetBusinessRuleErrors("myTag");
```
