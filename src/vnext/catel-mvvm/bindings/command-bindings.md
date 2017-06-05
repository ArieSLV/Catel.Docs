# Command bindings

Catel will automatically hook up the *CanExecute* of the command to the *Enabled* property of the element it is bound to. A command binding is always bound to an event of a specific element as shown below.

## Android

``` {.java data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"}
protected override void AddBindings(BindingContext bindingContext, IViewModel viewModel)
{
    var vm = (MainViewModel) viewModel;

    bindingContext.AddCommandBinding(_testButton, "Click", vm.RunCommand);
}
```

## iOS

iOS not yet documented

