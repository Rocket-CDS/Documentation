# Search And Filters Rules

The Directory systems follow a number of server side rules to ensure searching and filtering work correctly.  
*These rules can be adapted to deal with different situation by adapting the AppTheme. Or a new search rule can be introduced by creating a server side plugin.*   

## Server Side Search and Filter Functionality
### Category and Text Search
"Category Select" and "Text Search" are mutually exclusive.  This is to make searching as simple as possible for end users.  
The "Text Search" is given priority.

*NOTE: If text search needs to be made on categories a plugin or system can be created with different rules.*  

### When a text search is activated
- The category selected will be cleared.  
- The Filters and Tags will continue to persists and be applied to the results.  

### When a Category is selected (Without a "Text Search")
- The category results will be displayed.
- The Filters and Tags will continue to persists and be applied to the results.  

### When a Category is selected (With a "Text Search")
- Any text search will **NOT** be cleared.    
- **The category selected will be ignored**, because the "Text Search" is active. *(See Clearing the "Text Search")*  
- The Filters and Tags will continue to persists and be applied to the results.  

### Default Category for a module
If there is a default category for a module it will be ignored if a text search is made.  The text search will be across all the articles.  

## JS to change to adapt the search and filter

In some situation the server side functionality can be adapted by using JS in the AppTheme.  

Here are some examples of functionality that can be adapted.  Depending on the AppTheme names and requirements the JS functions may need to match the AppTheme.  

The control is done by using the "simplisity_setSessionField()" and "simplisity_getSessionField()" JS methods to control the session parameters that are passed to the server.

### Category select clearing the search and filters
*This is the most common request for change.*  

The click of the Category calls a JS function which clears the search and filter setting.  


```
<div class="rocket-categories" style="padding-right:8px;padding-left:8px;padding-top:@(moduleData.GetSetting("toppadding"))px;padding-bottom:@(moduleData.GetSetting("bottompadding"))px;">

    <div class="rocket-categoriestitle">@ResourceKey("DNNrocket.categories")</div>

    <div class="w3-bar-block">
        <a href="@ListUrl(moduleData.ListPageTabId())" onclick="clearFiltersCategories();$('.simplisity_loader').show();" class="w3-bar-item w3-button rocket-categorylink">@ResourceKey("DNNrocket.all")</a>

        @foreach (var catData in categoryDataList.GetCategoryTree())
        {
            var selectedCat = "";
            var showloader = "$('.simplisity_loader').show();"; // Page does not refresh if anchor is on a link.
            if (catData.CategoryId == categoryData.CategoryId)
            {
                selectedCat = " w3-theme-l3 ";
                showloader = "";
            }
            if (!catData.Hidden && !catData.HiddenByCulture && catData.Level == 0)
            {
                if (catData.Level == 0)
                {
                    var l = catData.GetDirectChildren();
                    if (l.Count == 0)
                    {
                        <a href="@ListUrl(moduleData.ListPageTabId(), catData)" class="w3-bar-item w3-button rocket-categorylink @selectedCat" onclick="@(showloader)clearFiltersCategories();" style="text-decoration: none;">@catData.Name (@catData.GetArticles().RecordCount)</a>
                    }
                    else
                    {
                        <a href="@ListUrl(moduleData.ListPageTabId(), catData)" class="w3-bar-item w3-button rocket-categorylink @selectedCat" onclick="@(showloader)clearFiltersCategories();" style="text-decoration: none;">@catData.Name (@catData.GetArticles().RecordCount)</a>
                        <div class="w3-margin-left">
                            @foreach (var child in l)
                            {
                                selectedCat = "";
                                if (child.CategoryId == categoryData.CategoryId)
                                {
                                    selectedCat = " w3-theme-l3 ";
                                    showloader = "";
                                }
                                <a href="@ListUrl(moduleData.ListPageTabId(), child)" class="w3-bar-item w3-button rocket-categorylink @selectedCat" onclick="@(showloader)clearFiltersCategories();" style="text-decoration: none;">@child.Name  (@child.GetArticles().RecordCount)</a>
                            }
                        </div>
                    }
                }
            }
        }
    </div>
</div>

<script>
    function clearFiltersCategories() {
        $('#viewsearchtext').val('');
        simplisity_setSessionField('monthidx', '');
        simplisity_setSessionField('viewsearchtext', '');
        $('.rocket-monthdates').removeClass('w3-theme-l3');
        simplisity_setSessionField('searchdate1', '');
        simplisity_setSessionField('searchdate2', '');
    }
</script>
```

### Month select clearing the search and filters

The "showSelected()" JS function is called when a month has been clicked and the "clearFiltersMonths()" JS function when the page is reloaded for the default list selection.

```
@inherits RocketDirectoryAPI.Components.RocketDirectoryAPITokens<Simplisity.SimplisityRazor>
@using DNNrocketAPI.Components;
@using RocketDirectoryAPI.Components;
@using Simplisity;
@AssignDataModel(Model)
@AddProcessDataResx(appTheme, true)
@AddProcessData("resourcepath", "/DesktopModules/DNNrocketModules/RocketDirectoryAPI/App_LocalResources/")
<!--inject-->
@{
    var monthList = RocketDirectoryAPIUtils.GetArticlesByMonth(systemData.SystemKey, DateTime.Now.AddMonths(-6), 12, "publisheddate", sessionParams.GetInt("catid"));
}
<div class="rocket-months " style="padding-right:8px;padding-left:8px;padding-top:@(moduleData.GetSetting("toppadding"))px;padding-bottom:@(moduleData.GetSetting("bottompadding"))px;">

    <a href="@ListUrl(moduleData.ListPageTabId(), categoryData)" class="w3-button w3-block w3-left-align rocket-monthdateslink" onclick="$('.simplisity_loader').show();clearFiltersMonths()">
        <span class="w3-text-black">
            @ResourceKey("RocketBlogAPI.latest")
        </span>
        <span class="w3-right">
        </span>
    </a>

    @{
        var monthidx = 1;
    }
    @foreach (var a in monthList)
    {
        if (a.Value.Count > 0)
        {
            <div class="w3-button w3-block w3-left-align rocket-monthdates rocket-monthdates@(monthidx) " monthidx="@(monthidx)" onclick="showSelected($(this).attr('monthidx'),'@a.Key.ToString("yyyy-MM-dd")', '@a.Key.AddMonths(1).AddDays(-1).ToString("yyyy-MM-dd")');">
                <span>
                    @a.Key.ToString("MMMM yyyy")
                </span>
                <span class="w3-right">
                    @a.Value.Count
                </span>
            </div>
        }
        monthidx += 1;
    }

</div>

@DateJsApiCall(moduleData, "#rocket-blog", sessionParams)

<script>
    $(document).ready(function () {
        if (simplisity_getSessionField('searchdate1') != '' && simplisity_getSessionField('viewsearchtext') == '') {
            monthidx = simplisity_getSessionField('monthidx');
            $('.rocket-monthdates').removeClass('w3-theme-l3');
            $('.rocket-monthdates' + monthidx).addClass('w3-theme-l3');
        }
        $('.simplisity_loader').hide();
    });
    function showSelected(monthidx, date1, date2) {
        simplisity_setSessionField('page', '1');
        $('#viewsearchtext').val('');
        $('.rocket-monthdates').removeClass('w3-theme-l3');
        $('.rocket-monthdates' + monthidx).addClass('w3-theme-l3');
        simplisity_setSessionField('monthidx', monthidx);
        simplisity_setSessionField('viewsearchtext', '');
        var element_to_scroll_to = document.getElementById('rocketblogdisplay');
        element_to_scroll_to.scrollIntoView();
        doDateSearchReload(date1, date2);
    }
    function clearFiltersMonths() {
        $('#viewsearchtext').val('');
        simplisity_setSessionField('monthidx', '');
        simplisity_setSessionField('viewsearchtext', '');
        $('.rocket-monthdates').removeClass('w3-theme-l3');
        simplisity_setSessionField('searchdate1', '');
        simplisity_setSessionField('searchdate2', '');
        simplisity_setSessionField('rocketpropertyidtag', '0');        
        $('.rocket-filtercheckbox').each(function (i, obj) { simplisity_setSessionField(this.id, false); });
    }

</script>

```

