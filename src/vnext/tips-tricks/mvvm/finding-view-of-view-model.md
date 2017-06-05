# Finding the view of a view model

Sometimes it is required to find the view of a view model. For example, this comes in handy when implementing drag and drop where you only want to support code via view models.

Internally, Catel uses with the *IViewManager* for this. As soon as a view is loaded (via the Loaded event), the view is registered to the view manager. The view manager will keep an eye on the events of the view and notice view model changes.

A view is removed from the manager as soon as it is unloaded (via the *Unloaded* event). From this moment on, it is no longer possible to retrieve a view via its view model.

Remember that only controls implementing *IView* are supported by the *IViewManager*

## Retrieving the view of a view model

To find the view of a view model, use the steps below:

1) Resolve the view from *IViewManager* from the *ServiceLocator*:

``` {.java data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"}
var viewManager = ServiceLocator.Default.ResolveType<IViewManager>();
```

2) Resolve the view:

``` {.java data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"}
var views = viewManager.GetViewsOfViewModel(myViewModel);
```

Note that it is possible that multiple views are linked to the same view model

