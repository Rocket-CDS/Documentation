# Tag Filters

Tag filters are used to select only articles with those properties attached.  This uses simplisity.js to interface to the API. 

There are different elements and razor token that can be used to create the Tag Filter functionality.  A default Tag Filter template can also be used which is the quickest and easiest method. 


Full Example: [rocketdirectoryapi.Products/1.0](https://github.com/Rocket-CDS/AppThemes-W3-CSS/tree/main/rocketdirectoryapi.Products/1.0)



## The Floating Module Method
The method uses 2 separate modules to display the list and the tag filter.  This allows more integration with the skin and a more modular design pattern.  It also allows Tags and Property filters to be independent for each module, because the module settings can be selected per module.  (See 'Module Property Group Settings' Below)

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

### TagFilter.cshtml
```
@inherits RocketDirectoryAPI.Components.RocketDirectoryAPITokens<Simplisity.SimplisityRazor>
@using DNNrocketAPI.Components;
@using RocketDirectoryAPI.Components;
@AssignDataModel(Model)
<!--inject-->
[INJECT:appthemedefault,TagFilter.cshtml]
```
This template is injecting the default TagFilter.cshtml, but it can be your own design.

### Module Template Definition
The TagFilter.cshtml module template needs to be created on the AppTheme with an entry in the dependencies file.  
(This enables the tag filters to be selected from the module settings.)  

*Exanple of dependancy file*
```
<moduletemplates list="true">
    <genxml>
        <file>TagFilter.cshtml</file>
        <name>Tag Filter</name>
        <cmd>list</cmd>
    </genxml>
</moduletemplates>
```

### CSS
The default 'TagFilter.cshtml' template uses standard class names for the display.  These are usually included in the AppTheme and can be change to match the design you want.
```
.rocket-tagbutton {
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
    margin-bottom: 8px;
    margin-right: 8px;
}

.rocket-tagbuttonOn {
    border: none;
    display: inline-block;
    padding: 8px 16px;
    vertical-align: middle;
    overflow: hidden;
    text-decoration: none;
    color: inherit;
    background-color: #98ddde;
    text-align: center;
    cursor: pointer;
    white-space: nowrap;
    margin-bottom: 8px;
    margin-right: 8px;
}

.rocket-tagbutton:hover {
    color: #000 !important;
    background-color: #ccc !important
}

.rocket-tagsgroup {
    font-size: 18px !important
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

### Default TagFilter.cshtml
```
@inherits RocketDirectoryAPI.Components.RocketDirectoryAPITokens<Simplisity.SimplisityRazor>
@using DNNrocketAPI.Components;
@using RocketDirectoryAPI.Components;
@AssignDataModel(Model)
<!--inject-->
<div class="rocket-tags">
    @foreach (var g in moduleData.GetPropertyModuleGroups(catalogSettings))
    {
        <div class="rocket-tagsgroup">@g.Value</div>
        foreach (var p in propertyDataList.GetPropertyTagList(g.Key))
        {
            @TagButton(p.Key, p.Value, sessionParams)
        }
    }
    <div>
    @TagButtonClear(ResourceKey("DNNrocket.clear").ToString(), sessionParams)
    </div>
</div>
@TagJsApiCall(moduleData.SystemKey, "#articlelistdisplay", sessionParams, "ArticleList.cshtml")
```
**Notice:**  The default template uses "ArticleList.cshtml" as the AppTheme list template.  It MUST be the same in the AppTheme.  
Also notice the default 'TagFilter.cshtml' template always uses a standard return element call '#articlelistdisplay'. 

If you want a different filter design you can copy '/DNNrocketModules/RocketDirectoryAPI/Themes/Default/1.0/default/TagFilter.cshtml' to your AppTheme and change it as you require. 

### Razor Tokens
Razor tokens have been created to make the JS and HTML easily function together.  Each element can be done directly in the template if required  

```
@TagButton(p.Key, p.Value, sessionParams)
```
Creates a button that calls the API when clicked.   

```
@TagButtonClear(ResourceKey("DNNrocket.clear").ToString(), sessionParams)
```
Creates a button that clears the tag session parameters.    

```
@TagJsApiCall(moduleData.SystemKey, "#articlelistdisplay", sessionParams, "ArticleList.cshtml")
```
Adds the required JS to make the buttons work.