# Property Filters

Property filters are used to select only articles with those properties attached.  Property Filters use a checkbox to select the property, which allows multiple selections.  This uses simplisity.js to interface to the API. 

There are different elements and razor token that can be used to create the Property Filter functionality.  A default Property Filter template can also be used which is the quickest and easiest method. 


Full Example: [rocketdirectoryapi.Products/1.0](https://github.com/Rocket-CDS/AppThemes-W3-CSS/tree/main/rocketdirectoryapi.Products/1.0)



## The Floating Module Method
The method uses 2 separate modules to display the list and the Property filter.  This allows more integration with the skin and a more modular design pattern.  It also allows Tags and Property filters to be independent for each module, because the module settings can be selected per module.  (See 'Module Property Group Settings' Below)

### View.cshtml
```
@inherits RocketDirectoryAPI.Components.RocketDirectoryAPITokens<Simplisity.SimplisityRazor>
@using RocketDirectoryAPI.Components;
@using DNNrocketAPI.Components;
@AssignDataModel(Model)
<!--inject-->

<!-- Archor so the page detail jumps to the correct position -->
<div id="articledisplay" style="position:relative;top:-165px;"></div>

<div class="containerouter ">
    <div class="w3-section containerinner">
    @if (articleData != null)
    {
        <div id="articlelistdisplay" class="w3-row">
                [INJECT:apptheme,ArticleDetail.cshtml]
        </div>
    }
    else
    {
        <div id="articlelistdisplay" class="w3-row">
            [INJECT:apptheme,ArticleList.cshtml]
        </div>
    }
    </div>
</div>
<!-- Loading css for ajax. -->
<div class="w3-overlay simplisity_loader" style="z-index:999;"></div>

```

### PropertyFilter.cshtml
```
@inherits RocketDirectoryAPI.Components.RocketDirectoryAPITokens<Simplisity.SimplisityRazor>
@using DNNrocketAPI.Components;
@using RocketDirectoryAPI.Components;
@AssignDataModel(Model)
<!--inject-->
[INJECT:appthemedefault,PropertyFilter.cshtml]
```
This template is injecting the default PropertyFilter.cshtml, but it can be your own design.

### Module Template Definition
The PropertyFilter.cshtml module template needs to be created on the AppTheme with an entry in the dependencies file.  
(This enables the Property filters to be selected from the module settings.)  

*Exanple of dependancy file*
```
<moduletemplates list="true">
    <genxml>
        <file>PropertyFilter.cshtml</file>
        <name>Property Filter</name>
        <cmd>list</cmd>
    </genxml>
</moduletemplates>
```

### CSS
The default 'PropertyFilter.cshtml' template uses standard class names for the display.  These are usually included in the AppTheme and can be change to match the design you want.
```
.rocket-filters {
    margin-top: 16px;
    margin-bottom: 16px;
}

.rocket-filtersgroup {
    font-size: 18px !important;
    margin-top: 8px;
}
.rocket-filtercheckbox {
    width: 24px;
    height: 24px;
    position: relative;
    top: 6px
}
.rocket-filtercheckbox-name {
    padding-left: 8px;
}
.rocket-filterbuttonclear {
    border: none;
    display: inline-block;
    padding: 8px 16px;
    vertical-align: middle;
    overflow: hidden;
    text-decoration: none;
    color: inherit;
    background-color: #d5f1f2;
    text-align: center;
    cursor: pointer;
    white-space: nowrap;
    margin-top: 8px;
    margin-bottom: 8px;
}
```


### Module Property Group Settings.
The property groups to be displayed for the tag system is defined in the module settings, using the AppTheme ThemeSettings.cshtml template. 

### ThemeSettings.cshtml
```
@inherits RocketDirectoryAPI.Components.RocketDirectoryAPITokens<Simplisity.SimplisityRazor>
@using RocketDirectoryAPI.Components;
@using DNNrocketAPI;
@using Simplisity;
@using RocketPortal.Components;
@using DNNrocketAPI.Components;
@using System.Globalization;
@using Rocket.AppThemes.Components;
@AssignDataModel(Model)
<!--inject-->
@{
    var info = new SimplisityInfo(moduleData.Record);
    //NOTE: xPath for module settings must use "genxml/settings/*"
}

<div class="w3-row  w3-padding">
    <div class="w3-row">
        <div class='w3-row w3-padding'>
            <div class="w3-large w3-margin-bottom">@ResourceKey("RC.propertygroups")</div>

            @FilterGroupCheckBox(info, catalogSettings)

        </div>
    </div>
</div>

```

# Extra Information

### Default PropertyFilter.cshtml
```
@inherits RocketDirectoryAPI.Components.RocketDirectoryAPITokens<Simplisity.SimplisityRazor>
@using DNNrocketAPI.Components;
@using RocketDirectoryAPI.Components;
@AssignDataModel(Model)
<!--inject-->

<div class="rocket-filters">
    @foreach (var g in moduleData.GetPropertyModuleGroups(catalogSettings))
    {
        <div class="rocket-filtersgroup">@g.Value</div>
        foreach (var p in propertyDataList.GetPropertyFilterList(g.Key))
        {
            <div>
            @FilterCheckBox(p.Key, p.Value, "#articlelistdisplay", sessionParams.Info.GetXmlPropertyBool("r/" + p.Key), "", "propertyname='" + p.Value.Replace("'", "&apos;") + "'")
            </div>
        }
    }
    <div>
        @FilterClearButton(ResourceKey("DNNrocket.clear").ToString(), "#articlelistdisplay")
    </div>
</div>
@FilterJsApiCall(moduleData.SystemKey, sessionParams, "ArticleList.cshtml")
```
**Notice:**  The default template uses "ArticleList.cshtml" as the AppTheme list template.  It MUST be the same in the AppTheme.  
Also notice the default 'PropertyFilter.cshtml' template always uses a standard return element call '#articlelistdisplay'. 

If you want a different filter design you can copy '/DNNrocketModules/RocketDirectoryAPI/Themes/Default/1.0/default/PropertyFilter.cshtml' to your AppTheme and change it as you require. 

### Razor Tokens
Razor tokens have been created to make the JS and HTML easily function together.  Each element can be done directly in the template if required  

```
@FilterCheckBox(string checkboxId, string textName, string sreturn, bool value, string cssClass = "", string attributes = "")
```
Creates a checkbox that calls the API when clicked.   

```
@FilterClearButton(string textName, string sreturn)
```
Creates a button that clears the property session parameters.    

```
@FilterJsApiCall(string systemKey, SessionParams sessionParams, string templateName = "articlelist.cshtml")
```
Adds the required JS to make the checkboxes work.
