# Search And Filters Rules

The Directory systems follow a number of server side rules to ensure searching and filtering work correctly.  
These rules can be adapted to deal with different situation by adapting the AppTheme or a new search rule can be introduced by creating a server side plugin.  

**These situations can get highly complex**

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

In some situation the functionality can be adapted by using JS in the AppTheme.  

The control is done by using the "simplisity_setSessionField()" and "simplisity_getSessionField()" JS methods to control the session parameters that are passed to the server.

### Make a Category select clear the search and filters
*This is the most common request for change.*  

The onclick of the Category link can call a JS function which clears the search and filter settings.  
```
<script>
    function clearSearchAndFilters() {

        // Clear the search field
        $('#viewsearchtext').val('');
        simplisity_setSessionField('viewsearchtext', '');

        // Clear the date range selection (This is a search)
        simplisity_setSessionField('monthidx', '');
        simplisity_setSessionField('searchdate1', '');
        simplisity_setSessionField('searchdate2', '');

        // Clear tag selection        
        simplisity_setSessionField('rocketpropertyidtag', '0');        

        // Clear Filter checkboxes
        $('.rocket-filtercheckbox').each(function (i, obj) { simplisity_setSessionField(this.id, false); });

    }
</script>
```

## AppTheme Example 
To explain more we will refer to the example templates in the W3 AppTheme project.  

Look at "rocketblogapi.Blog/1.0"

[https://github.com/Rocket-CDS/AppThemes-W3-CSS](https://github.com/Rocket-CDS/AppThemes-W3-CSS)
