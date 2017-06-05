# UrlLocator

The *IUrlLocator* class is responsible for resolving the right urls for the xaml views for a view model in navigation based applications. Before Catel 3.0, the *INavigationService* was responsible for resolving the url, but this responsibility is now taken over by the *IUrlLocator*. The *NavigationService* internally uses the *IUrlLocator* to resolve the views.

# Resolving by naming convention

It is possible to resolve views using the *IUrlLocator*. Then you can use the *ResolveUrl* method to resolve the url based on the type of the view model.

For example, the following view model:

``` {.java data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"}
Catel.Examples.ViewModels.MyViewModel
```

will be resolved as:

``` {.java data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"}
/Views/MyPage.xaml
```

Note that the *UrlLocator* checks whether the resource actually exists. If the resource does not exists, it will not be able to resolve a view

# Manually resolving a naming convention

To manually resolve a naming convention, use the following code:

``` {.java data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"}
var urlLocator = ServiceLocator.Instance.ResolveType<IUrlLocator>();
var url = urlLocator.ResolveUrl(typeof(MyViewModel));
```

# Customizing naming conventions

By default, the *IUrlLocator* uses the following naming conventions to resolve urls:

-   /Views/[VM].xaml
-   /Views/[VM]View.xaml
-   /Views/[VM]Control.xaml
-   /Views/[VM]Page.xaml
-   /Views/[VM]Window.xaml
-   /Controls/[VM].xaml
-   /Controls/[VM]Control.xaml
-   /Pages/[VM].xaml
-   /Pages/[VM]Page.xaml
-   /Windows/[VM].xaml
-   /Windows/[VM]Window.xaml
-   /UI.Views/[VM].xaml
-   /UI.Views/[VM]View.xaml
-   /UI.Views/[VM]Control.xaml
-   /UI.Views/[VM]Page.xaml
-   /UI.Views/[VM]Window.xaml
-   /UI.Controls/[VM].xaml
-   /UI.Controls/[VM]Control.xaml
-   /UI.Pages/[VM].xaml
-   /UI.Pages/[VM]Page.xaml
-   /UI.Windows/[VM].xaml
-   /UI.Windows/[VM]Window.xaml
-   /[VM].xaml
-   /[VM]Control.xaml
-   /[VM]Page.xaml
-   /[VM]Window.xaml

For more information about naming conventions, see [Naming conventions](Naming_conventions)

However, it is possible to add or remove new naming conventions to support your own naming convention. For example, to add a new naming convention for a different assembly, use this code:

``` {.java data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"}
var urlLocator = ServiceLocator.Instance.ResolveType<IUrlLocator>();
urlLocator.NamingConventions.Add("/MyPages/[VM]Page.xaml");
```

# Registering custom urls

Sometimes, a class doesn't follow a naming convention (for whatever reason possible). In such a case, it is possible to register a mapping manually using the following code:

``` {.java data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"}
var urlLocator = ServiceLocator.Instance.ResolveType<IUrlLocator>();
urlLocator.Register(typeof(MyViewModelNotFollowingNamingConvention), "/MyVerySpecialUrl.xaml");
```

# Using a custom UrlLocator

If you want to have total freedom to determine which url is provided per view model (maybe there are other services that have an impact on this), it is possible to create a custom *IUrlLocator* implementation. Then the only thing to do is to register it using the following code:

``` {.java data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"}
ServiceLocator.Default.Register<IUrlLocator, MyUrlLocator>();
```
