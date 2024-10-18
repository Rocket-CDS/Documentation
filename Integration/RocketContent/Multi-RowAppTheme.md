# Multi-Row AppTheme

AppThemes can work with multiple rows of data, this can be used for a list and detail layout or a simple list.

We are going to use example1 AppTheme, which we created in the "Create an AppTheme" tutorial.  
We will create a new version of the the AppTheme.

Copy the folder

```plaintext
/DesktopModules/RocketThemes/AppThemes-W3-CSS/rocketcontentapi.example1/1.0
```

To a destination folder of

```plaintext
/DesktopModules/RocketThemes/AppThemes-W3-CSS/rocketcontentapi.example1/2.0
```

We now have a new version of the AppTheme which we can work on without any change to v1.0, so all modules using it will be unaffected.

Edit the v2.0 version of "**AdminDetail.cshtml**" to this code:

```plaintext
@inherits RocketContentAPI.Components.RocketContentAPITokens<Simplisity.SimplisityRazor>
@AssigDataModel(Model)
@AddProcessDataResx(appThemeView, true)
<!--inject-->

<div class="w3-row w3-padding">
    <label>@ResourceKey("DNNrocket.heading")</label>
    @TextBox(headerData, "genxml/header/headingtitle", " class='w3-input w3-border' autocomplete='off' ", "", false, 0)
</div>

<div class="w3-row">
    <div id="articledetailpanel" class="w3-threequarter">
        [INJECT:apptheme,AdminRow.cshtml]
    </div>
    <div class="w3-quarter">
        [INJECT:appthemesystem,AdminRowSelect.cshtml]
    </div>
</div>
```

Edit the v2.0 version of "**View.cshtml**" to this codeL

```plaintext
@inherits RocketContentAPI.Components.RocketContentAPITokens<Simplisity.SimplisityRazor>
@AssigDataModel(Model)
@AddProcessDataResx(appThemeView, true)
<!--inject-->

<h1>@headerData.GetXmlProperty("genxml/header/headingtitle")</h1>

@foreach (var articleRowData in articleData.GetRows())
{
    var rowData = articleRowData.Info;

    <h2>@rowData.GetXmlProperty("genxml/lang/genxml/textbox/title")</h2>
    <img src="@ImageUrl(articleRowData.GetImage(0).RelPathWebp,200,200,"","webp")" />
}
```

Change the RocketContent module setting to use "example1\\v2.0" and you will see that we have converted the AppTheme to a multi-Row AppTheme for v2.0.