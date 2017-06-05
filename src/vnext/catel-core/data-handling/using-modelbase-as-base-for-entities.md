# Using ModelBase as base for entities

It is possible to use the *ModelBase* as base class when using EF or any other OR mapper.

-   [Setting up ModelBase as base class](#UsingModelBaseasbaseforentities-SettingupModelBaseasbaseclass)
-   [Ignoring default Catel properties in models](#UsingModelBaseasbaseforentities-IgnoringdefaultCatelpropertiesinmodels)

# Setting up ModelBase as base class

There are a few caveats when using the *ModelBase* as base class for your entities. One of them is that *IsDirty* is always true because the properties from the persistence store are set after the constructor. This guide will explain how to work past that problem.

1. Create a class named *EntityBase* with the following code:

``` {.java data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"}
public class EntityBase : ModelBase
{
    protected EntityBase() { }
    
    protected EntityBase(SerializationInfo info, StreamingContext context)
        : base(info, context) { }
        
    internal void ClearDirtyFlag()
    {
        IsDirty = false;
    }
}
```

2. Derive from *EntityBase* instead of *ModelBase* so the layer that loads the data can clear the *IsDirty* flag.

3. When loading the data from the database and setting the initial values, use this code:

``` {.java data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"}
var company = new DTO.Company()
{
    Address = domainEntity.Address,
    City = domainEntity.City,
    CompanyID = domainEntity.CompanyID, 
    CompanyName = domainEntity.CompanyName, 
    PostalCode = domainEntity.PostalCode,
    PostalCodeAndCity = domainEntity.PostalCodeAndCity
};

company.ClearDirtyFlag();
return company;
```

Note the *ClearDirtyFlag* call, which is very important to make the *IsDirty* property behave correctly.

4. Check the *IsDirty* of the model, not the view model when checking whether the model is dirty inside a view model.

# Ignoring default Catel properties in models

It is possible to ignore the default Catel properties in the models for EF code-first. To accomplish this, use the following code:

``` {.java data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"}
protected override void OnModelCreating(DbModelBuilder modelBuilder)
{
   modelBuilder.Types().Configure(c => c.Ignore("IsDirty"));
   modelBuilder.Types().Configure(c => c.Ignore("IsReadOnly"));
 
   base.OnModelCreating(modelBuilder);
}
```

Or, if you only want to do this for classes inheriting Modelbase, use the following code:

``` {.c# data-syntaxhighlighter-params="brush: c#; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: c#; gutter: false; theme: Confluence"}
protected override void OnModelCreating(DbModelBuilder modelBuilder)
{
   modelBuilder.Types().Where(t => t.IsSubclassOf(typeof(ModelBase))).Configure(c => c.Ignore("IsDirty"));
   modelBuilder.Types().Where(t => t.IsSubclassOf(typeof(ModelBase))).Configure(c => c.Ignore("IsReadOnly"));

   base.OnModelCreating(modelBuilder);
}
```


