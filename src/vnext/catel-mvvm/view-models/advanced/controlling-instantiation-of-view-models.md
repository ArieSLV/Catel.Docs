# Controlling the instantiation of view models

Starting with Catel 3.1, it is possible to take control of the view model instantiation of a view dynamically at runtime. This feature can be used when the construction of a view model is more complex than injecting the datacontext as model, or when the view model instance in a custom repository should be re-used.

-   [How to control the view model instantiation using the IViewModelFactory](#Controllingtheinstantiationofviewmodels-HowtocontroltheviewmodelinstantiationusingtheIViewModelFactory)
-   [How to control the view model instantiation using a UserControl](#Controllingtheinstantiationofviewmodels-HowtocontroltheviewmodelinstantiationusingaUserControl)
-   [How to control the view model instantiation using a behavior](#Controllingtheinstantiationofviewmodels-Howtocontroltheviewmodelinstantiationusingabehavior)
-   [Preventing the logic to create a view model by itself](#Controllingtheinstantiationofviewmodels-Preventingthelogictocreateaviewmodelbyitself)

Note that this feature is available on all controls an behaviors, not only for the *UserControl*

# How to control the view model instantiation using the IViewModelFactory

The *IViewModelFactory* is the best way to customize the instantiation of view models in Catel. This allows full control for all view models because the factory will be used to create all view models, except when any other methods described below are used.

If customization of the view model instantiation is required, it is best the derive from the default *ViewModelFactory* class to be able to fall back to the default behavior in non-special cases.

``` {.java data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"}
public class CustomViewModelFactory : ViewModelFactory
{
    public override IViewModel CreateViewModel(Type viewModelType, object dataContext)
    {
        if (viewModelType == typeof(MySpecialViewModel))
        {
            // Use custom constructor with dependency injection
            return new MySpecialViewModel(dep1, dep2, dep3, dataContext);
        }

        // Fall back to default behavior
        return base.CreateViewModel(viewModelType, dataContext);
    }
}
```

When a custom factory is used, it is important to register it in the ServiceLocator:

``` {.java data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"}
ServiceLocator.Default.RegisterType<IViewModelFactory, CustomViewModelFactory>();
```

# How to control the view model instantiation using a UserControl

Note that it is best to use the *IViewModelFactory* for view model instantation because it is a more generic solution

Controlling the instantiation of the view model dynamically when using the *UserControl* is extremely easy. You can override the *GetViewModelInstance(object)* method like this:

``` {.java data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"}
protected override IViewModel GetViewModelInstance(object dataContext)
{
    if (dataContext is Rectangle)
    {
        return new RectangleViewModel((Rectangle)dataContext);
    }

    return null;
}
```

When the method returns *null*, the logic will try to construct the view model by itself.

# How to control the view model instantiation using a behavior

Note that it is best to use the *IViewModelFactory* for view model instantation because it is a more generic solution

Controlling the instantiation of the view model dynamically when using behaviors must be done via the *DetermineViewModelInstance* event like this:

``` {.java data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"}
mvvmBehavior.DetermineViewModelInstance += (sender, e) =>
{
    var dataContext = e.DataContext;
    if (dataContext is Rectangle)
    {
        e.ViewModel = new RectangleViewModel((Rectangle)dataContext);
    }
};
```

There is no need to set the *e.ViewModel* to *null* because that is the default value.

# Preventing the logic to create a view model by itself

When using the behavior, it is possible to prevent the logic to instantiate a view model. In this case, it is really possible to have full control over view model instantiation. To prevent the logic to create a view model, use this code:

``` {.java data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"}
mvvmBehavior.DetermineViewModelInstance += (sender, e) =>
{
    var dataContext = e.DataContext;
    if (dataContext is Rectangle)
    {
        e.ViewModel = new RectangleViewModel((Rectangle)dataContext);
    }

    e.DoNotCreateViewModel = true;
};
```
