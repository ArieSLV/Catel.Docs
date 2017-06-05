# Fragments (user controls)

The user controls in Android are called fragments. This means that if a user control must be created, it must derive from the *Fragment* class. Catel provides a base implementation of this class to ensure full compatibility with the MVVM framework that ships with Catel.

## Creating the view model

The view model can be created (or added as a linked file from another project) just like any platform using Catel (they are all equal on all platforms).

## Creating the view

Make sure that a *Views* folder exists in the project so the views and view models can automatically be hooked together by Catel. Then create a new class to the views folder, in this case *PersonView*:

``` {.java data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"}
public class PersonView : Catel.Android.App.Fragment
{
    public override View OnCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
    {
        var inflateResult = inflater.Inflate(Resource.Layout.Fragment_Person, container, false);
        return inflateResult;
    }
}
```

Note that the class derives from *Catel.Android.App.Fragment*

## Designing the view

To create the actual user interface of the fragment, add a new *axml* file to the *Resources/layout* folder, in your case *Fragment\_Person* (but you can name it whatever you want). Then use the following source:

``` {.java data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"}
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent">
    <TextView
        android:text="First name"
        android:textAppearance="?android:attr/textAppearanceMedium"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/textView1" />
    <EditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/firstNameText" />
    <TextView
        android:text="Last name"
        android:textAppearance="?android:attr/textAppearanceMedium"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/textView2" />
    <EditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/lastNameText" />
</LinearLayout>
```

![](attachments/25329685/25559041.png?width=350)

## Setting up synchronization

In Android it is required to manually synchronize the values between the view and view model. Below is the fully extended *PersonView* class containing these mapping functionality:

``` {.java data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"}
public class PersonView : Catel.Android.App.Fragment
{
    public override View OnCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
    {
        var inflateResult = inflater.Inflate(Resource.Layout.Fragment_Person, container, false);
        return inflateResult;
    }

    public override void OnActivityCreated(Bundle savedInstanceState)
    {
        base.OnActivityCreated(savedInstanceState);

        // Note: at this stage the visual tree is guaranteed in Android
        _firstNameEditText = Activity.FindViewById<EditText>(Resource.Id.firstNameText);
        _lastNameEditText = Activity.FindViewById<EditText>(Resource.Id.lastNameText);
    }

    protected override void SyncViewModel()
    {
        var vm = GetViewModel<PersonViewModel>();
        if (vm == null)
        {
            return;
        }

        _firstNameEditText.Text = vm.FirstName;
        _lastNameEditText.Text = vm.LastName;
    }
}
```

## Attachments:

![](images/icons/bullet_blue.gif) [image2014-4-1 23:5:19.png](attachments/25329685/25559041.png) (image/png)

