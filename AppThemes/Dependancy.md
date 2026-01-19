# AppTheme Dependencies

AppThemes can define CSS and JavaScript files that they need to work properly. This is done through a dependency file that tells the system what files to load on your website pages.

## Where is the Dependency File?

The dependency file is located in the `dep` subfolder of your AppTheme:

```
/DesktopModules/RocketThemes/#AppThemeProjectName#/#systemkey#.#AppThemeName#/#version#/dep
```

The file is in XML format with a `.dep` extension.

## How Dependencies Work

Dependencies are CSS and JavaScript files that your theme needs. The system automatically prevents duplicate files from being loaded multiple times, which keeps your website fast and efficient.

**Important:** Dependencies only work on the public-facing pages (Front View) of your website. For Admin View, use the `AdminFirstHeader.cshtml` or `AdminLastHeader.cshtml` templates instead.

**Modules Using Dependencies:** The dependency system is used by RocketContentMod and RocketDirectoryMod modules. When these modules are placed on a page, the system reads the dependency file from the selected AppTheme and loads the required files.

## Basic Dependency Structure

Each dependency entry needs to specify:

**ctrltype** - The type of file to load
- `css` - For CSS stylesheets
- `js` - For JavaScript files

**url** - The location of the file
- Can be a full path starting with `/`
- Can use tokens (see below) for dynamic paths
- Can be a special token like `{jquery}`

**ecofriendly** - Controls loading based on ECO Mode setting
- `true` - File loads even when ECO Mode is ON
- `false` - File only loads when ECO Mode is OFF

Here's a simple example:

```xml
<deps list="true">
    <genxml>
        <ctrltype>css</ctrltype>
        <url>/DesktopModules/DNNrocket/css/w3.css</url>
        <ecofriendly>true</ecofriendly>
    </genxml>
    <genxml>
        <ctrltype>js</ctrltype>
        <url>/DesktopModules/MyTheme/js/custom.js</url>
        <ecofriendly>false</ecofriendly>
    </genxml>
</deps>
```

## Special Tokens for URLs

You can use special tokens in your URL paths that the system will automatically replace:

**{domainurl}** - Replaced with the full domain URL (including http:// or https://)

Example: `{domainurl}/path/to/file.css` becomes `https://yoursite.com/path/to/file.css`

**{appthemefolder}** - Replaced with the relative path to your theme's folder

Example: `{appthemefolder}/css/style.css` becomes `/DesktopModules/RocketThemes/MyProject/rocketcontentapi.MyTheme/1.0/css/style.css`

**{appthemesystemfolder}** - Replaced with the relative path to the system folder

Example: `{appthemesystemfolder}/css/system.css`

**{jquery}** - Special token that loads the jQuery library

Example:
```xml
<genxml>
    <ctrltype>js</ctrltype>
    <url>{jquery}</url>
    <ecofriendly>true</ecofriendly>
</genxml>
```

**Using Multiple Tokens:**
```xml
<genxml>
    <ctrltype>css</ctrltype>
    <url>{domainurl}{appthemefolder}/css/mystyle.css</url>
    <ecofriendly>true</ecofriendly>
</genxml>
```

## ECO Mode

ECO Mode is a module-level performance feature that helps speed up your website by loading only essential files.

**How It Works:**

Each module has an ECO Mode setting that can be turned ON or OFF.

When ECO Mode is **ON**:
- Only files marked `<ecofriendly>true</ecofriendly>` will load
- Files marked `<ecofriendly>false</ecofriendly>` are skipped
- Results in faster page load times

When ECO Mode is **OFF**:
- All dependency files load regardless of their ecofriendly setting
- Provides full functionality with all enhancements

**Best Practice:**
- Mark essential files (framework CSS, core JavaScript) as `ecofriendly="true"`
- Mark optional enhancements (animations, special effects) as `ecofriendly="false"`
- This lets users choose between performance and full features

## Skipping Files Based on Skin

Sometimes your DNN skin already includes certain CSS files (like Bootstrap or W3.CSS). You can tell the system to skip loading your dependency file if a specific skin is being used.

Use the `<ignoreonskin>` tag with a comma-separated list of skin names:

```xml
<genxml>
    <ctrltype>css</ctrltype>
    <url>/DesktopModules/DNNrocket/css/w3.css</url>
    <ecofriendly>true</ecofriendly>
    <ignoreonskin>rocketw3,anotherskin</ignoreonskin>
</genxml>
```

**How Matching Works:**

The system checks if the page's skin source (SkinSrc) contains any of the skin names you list.

For example:
- If your skin is `/Portals/_default/Skins/rocketw3-blue/`, it will match `rocketw3`
- If your skin is `/Portals/0/Skins/MyRocketW3Theme/`, it will match `rocketw3`
- Partial matches work, so be specific enough to avoid false matches

**Multiple Skins:**
```xml
<ignoreonskin>rocketw3,bootstrapskin,w3css,mycustomskin</ignoreonskin>
```

**Common Use Cases:**
- Skip loading W3.CSS if your skin already includes it
- Skip loading Bootstrap if your skin is Bootstrap-based
- Skip loading jQuery if your skin already loads it

## Skipping Files Based on Display Template

You can tell the system to skip loading a dependency when specific display templates are being used in the module. This is useful when certain functionality is only needed for particular views.

Use the `<ignoreontemplate>` tag with a comma-separated list of template filenames:

```xml
<genxml>
    <ctrltype>js</ctrltype>
    <url>{appthemefolder}/js/slider.js</url>
    <ecofriendly>false</ecofriendly>
    <ignoreontemplate>view.cshtml,listonly.cshtml</ignoreontemplate>
</genxml>
```

**How It Works:**

The display template filename is passed from the module settings to the dependency system. When processing dependencies:
- The system checks if the current display template filename matches any names in `ignoreontemplate`
- Matching is case-insensitive
- If a match is found, that dependency file is skipped
- The template filename comes from the `<file>` node in the module templates section (e.g., `view.cshtml`, not the `cmd` value like `list`)

**Display Template Examples:**

The template name to use is the **filename** from your module templates, NOT the cmd value.

For RocketContentMod:
- `view.cshtml` - Main content view template
- Any custom template filename you've created (e.g., `myview.cshtml`)

For RocketDirectoryMod:
- `view.cshtml` - Combined list and detail view
- `listonly.cshtml` - List view only
- `detailonly.cshtml` - Detail view only
- `hozcatmenu2lvl.cshtml` - Horizontal category menu (2 levels)
- Any custom template filename you've created

**Important:** Use the template **filename** (from the `<file>` node in module templates), not the cmd value.

**Practical Examples:**

Skip image slider on list views:
```xml
<genxml>
    <ctrltype>js</ctrltype>
    <url>{appthemefolder}/js/lightbox.js</url>
    <ecofriendly>false</ecofriendly>
    <ignoreontemplate>listonly.cshtml,hozcatmenu2lvl.cshtml</ignoreontemplate>
</genxml>
```

Skip list pagination on detail views:
```xml
<genxml>
    <ctrltype>js</ctrltype>
    <url>{appthemefolder}/js/pagination.js</url>
    <ecofriendly>false</ecofriendly>
    <ignoreontemplate>detailonly.cshtml</ignoreontemplate>
</genxml>
```

Skip form validation on read-only templates:
```xml
<genxml>
    <ctrltype>js</ctrltype>
    <url>{appthemefolder}/js/formvalidation.js</url>
    <ecofriendly>false</ecofriendly>
    <ignoreontemplate>listonly.cshtml,detailonly.cshtml,hozcatmenu2lvl.cshtml</ignoreontemplate>
</genxml>
```

## Complete Dependency Example

Here's a full example showing different types of dependencies with explanations:

```xml
<deps list="true">
    <!-- Essential: Load jQuery - needed by most JavaScript -->
    <genxml>
        <ctrltype>js</ctrltype>
        <url>{jquery}</url>
        <ecofriendly>true</ecofriendly>
    </genxml>
    
    <!-- Essential: Load W3.CSS framework - but skip if skin already has it -->
    <genxml>
        <ctrltype>css</ctrltype>
        <url>/DesktopModules/DNNrocket/css/w3.css</url>
        <ecofriendly>true</ecofriendly>
        <ignoreonskin>rocketw3,w3skin</ignoreonskin>
    </genxml>
    
    <!-- Essential: Load theme-specific CSS - always needed -->
    <genxml>
        <ctrltype>css</ctrltype>
        <url>{domainurl}{appthemefolder}/css/theme.css</url>
        <ecofriendly>true</ecofriendly>
    </genxml>
    
    <!-- Optional: Load animation library - enhancement only -->
    <genxml>
        <ctrltype>js</ctrltype>
        <url>{appthemefolder}/js/animations.js</url>
        <ecofriendly>false</ecofriendly>
    </genxml>
    
    <!-- Optional: Load image slider - only needed for detail view -->
    <genxml>
        <ctrltype>js</ctrltype>
        <url>{appthemefolder}/js/slider.js</url>
        <ecofriendly>false</ecofriendly>
        <ignoreontemplate>listonly.cshtml,hozcatmenu2lvl.cshtml</ignoreontemplate>
    </genxml>
    
    <!-- Optional: Load from CDN with full URL -->
    <genxml>
        <ctrltype>css</ctrltype>
        <url>https://cdn.example.com/library/1.0/style.css</url>
        <ecofriendly>false</ecofriendly>
    </genxml>
</deps>
```

## Combining Options

You can combine `ecofriendly`, `ignoreonskin`, and `ignoreontemplate` for fine-grained control:

```xml
<genxml>
    <ctrltype>js</ctrltype>
    <url>{appthemefolder}/js/advanced-gallery.js</url>
    <ecofriendly>false</ecofriendly>
    <ignoreonskin>rocketw3</ignoreonskin>
    <ignoreontemplate>listonly.cshtml,hozcatmenu2lvl.cshtml</ignoreontemplate>
</genxml>
```

This dependency:
1. Only loads when ECO Mode is OFF (not essential)
2. Skips loading if using rocketw3 skin
3. Skips loading on listonly.cshtml and hozcatmenu2lvl.cshtml templates

## Tips for Designers

**1. Mark Essential Files as ECO-Friendly**

Any CSS or JavaScript your theme can't work without should have `<ecofriendly>true</ecofriendly>`. These typically include:
- CSS framework files (W3.CSS, Bootstrap)
- jQuery or other required libraries
- Your main theme CSS file
- Core JavaScript for basic functionality

**2. Use Skin Ignore to Prevent Duplicates**

Check what your DNN skins already include:
- Look in the skin's `skin.ascx` file for CSS and JS references
- Add those skin names to `<ignoreonskin>` tags
- This prevents loading the same library twice

**3. Use Template Ignore for View-Specific Files**

Think about which features are needed in which views:
- Image galleries only on detail views (skip on `listonly.cshtml`)
- Pagination only on list views (skip on `detailonly.cshtml`)
- Category navigation only on category menus
- Form validation only on forms

Remember to use the **template filename** (e.g., `view.cshtml`), not the cmd value (e.g., `list`).

**4. Use Tokens for Portability**

Always use `{appthemefolder}` for theme files:
- Makes your theme work in any installation
- Handles version folders automatically
- Easier to maintain and update

**5. Test in Different Scenarios**

Test your theme with:
- ECO Mode ON and OFF
- Different display templates
- Different skins
- Make sure nothing breaks in any combination

**6. Load Order is Automatic**

You don't need to worry about file load order:
- All CSS files load before JavaScript files
- Files are loaded in the order they appear in your dependency file
- The system handles the details

**7. External URLs Work Too**

You can load files from CDNs:
```xml
<genxml>
    <ctrltype>css</ctrltype>
    <url>https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css</url>
    <ecofriendly>false</ecofriendly>
</genxml>
```

**8. Keep Dependencies Organized**

Group your dependencies logically:
- Essential files at the top
- Optional enhancements below
- Add XML comments to explain what each file does
- Use consistent naming conventions

**9. Consider Performance**

Remember that each dependency is a separate HTTP request:
- Minimize the number of files when possible
- Combine related CSS/JS files if appropriate
- Use ECO Mode wisely to balance features and performance
- Consider which files are truly necessary vs. "nice to have"

**10. Document Your Dependencies**

Keep notes about:
- What each dependency file does
- Why certain files are marked as essential
- Which templates need which files
- Any special requirements or dependencies between files

## Module Templates
A ModuleTemplate is part of an AppTheme. It defines what templates and commands can be used on a Module at startup.

The XML contains metadata that is used by the system. There is no UI in the AppTheme editor, this data is static to the AppTheme.

*Example:*
```xml
<moduletemplates list="true">
    <genxml>
        <file>view.cshtml</file>
        <name>List View</name>
        <cmd>list</cmd>
    </genxml>
    <genxml>
        <file>hozcatmenu2lvl.cshtml</file>
        <name>Horizontal Category Menu (2 levels)</name>
        <cmd>catmenu</cmd>
    </genxml>
</moduletemplates>
```

**file** = File name of the template  
**name** = Friendly name displayed in module settings  
**cmd** = The data command that will be used

### cmd values for RocketDirectory

**listdetail** = Display detail or list by using "articleid" param (default)  
**list** = Same as listdetail (Deprecated)  
**listonly** = List data only  
**detailonly** = Article data only  
**catmenu** = Category Data  
**satellite** = List data without populating it. The call for data should be made in the razor template

## Admin Panel Interfaces

An AppTheme for "rocketdirectoryapi" system (or a wrapper system) can select which options are available in the Admin Panel of the system.

By default, options are shown. If you want to hide options, you need to have an "adminpanelinterfacekeys" section in the dependency file of the AppTheme.

There are 5 options that can be shown:
- Article Admin
- Category Admin
- Property Admin
- Settings Admin
- Portal System Admin (Host only)

The superuser will always see all admin options.

All activated plugins will be shown on the Admin Panel.

Setting the show node to "False" will hide the option.

*Example:*
```xml
<adminpanelinterfacekeys list="true">
    <genxml>
        <interfacekey>articleadmin</interfacekey>
        <show>true</show>
    </genxml>
    <genxml>
        <interfacekey>categoryadmin</interfacekey>
        <show>true</show>
    </genxml>
    <genxml>
        <interfacekey>propertyadmin</interfacekey>
        <show>true</show>
    </genxml>
    <genxml>
        <interfacekey>settingsadmin</interfacekey>
        <show>true</show>
    </genxml>
    <genxml>
        <interfacekey>rocketdirectoryadmin</interfacekey>
        <show>true</show>
    </genxml>
</adminpanelinterfacekeys>
```

## QueryParams

With the directory system, you may have a list and detail structure.

### Format
```xml
<queryparams list="true">
    <genxml>
        <queryparam>articleid</queryparam>
        <tablename>rocketdirectoryapi</tablename>
        <systemkey>rocketnewsapi</systemkey>
        <datatype>article</datatype>
    </genxml>
</queryparams>
```

**queryparam** = The URL param that will be looked for. The value is used to read records from the Database  
**tablename** = The name of the table that will be used. Usually "rocketdirectoryapi" or "rocketecommerceapi"  
**systemkey** = The systemkey for the queryparam  
**datatype** = The type of data (article, category, etc.)

The QueryParams do 2 important actions when the detail page needs to be seen: **SEO** and **Activation of the Detail Display**.

### Categories

The category article list query param is also defined in the dependency file:

```xml
<queryparams list="true">
    <genxml>
        <queryparam>catid</queryparam>
        <tablename>rocketdirectoryapi</tablename>
        <systemkey>rocketnewsapi</systemkey>
        <datatype>category</datatype>
    </genxml>
</queryparams>
```

This allows each AppTheme to have its own category menu injected into the DDRMenu.

### SEO

The detail should contain SEO data in the header. The SEO data is read by using a URL parameter, this parameter is defined in the dependencies file. Saving the directory settings will also update the Page data so the Meta.ascx can capture the detail data with an ItemId.

The data record must have some default field names that will be used for SEO:

**metatitle** = "genxml/lang/genxml/textbox/seotitle" or "genxml/lang/genxml/textbox/articlename"  
**metadescription** = "genxml/lang/genxml/textbox/seodescription" or "genxml/lang/genxml/textbox/articlesummary"  
**metatagwords** = "genxml/lang/genxml/textbox/seokeyword"

*NOTE: Keywords are no longer used by search engines and do not have to be included.*

It will also look for the first image called "imagepatharticleimage" or "imagepathproductimage".

These field names are the default names used in the Shared Templates. If you are not using the shared templates, you must use the same names to make the SEO header work.

### DDRMenu Provider

The categories can be added to the menu by the menu provider. (See MenuManipulator documentation)

```xml
<menuprovider>
    <genxml>
        <assembly>RocketDirectoryAPI</assembly>
        <namespaceclass>RocketDirectoryAPI.Components.MenuDirectory</namespaceclass>
        <systemkey>rocketblogapi</systemkey>
    </genxml>
</menuprovider>
```

### Activation of the detail display

The detail page is displayed in a module by using the itemid in the URL. The name of the query param for the itemid is defined in the dependency file.

The systemkey is also defined, so that only modules using the defined systemkey are activated for detail.

In some situations, multiple systems/modules may want to display the detail. This can be done but it will affect the SEO, only the first detail SEO will be added to the page.

## DNN search

RocketContent can use the dependency file to define what field data should be included in the DNN search.

*NOTE: Other Rocket systems like RocketDirectory have defined fields for the search and therefore do not use this section*

### Dependency File Section (example)

```xml
<searchindex list="true">
    <genxml>
        <searchtitle>genxml/lang/genxml/textbox/title</searchtitle>
        <searchdescription>genxml/lang/genxml/textbox/richtext</searchdescription>
        <searchbody>genxml/lang/genxml/textbox/richtext</searchbody>
    </genxml>
</searchindex>
```

The dependency section only has 1 entry. A CSV list can be used to concatenate multiple fields.

## Example of a Full Dependency File

*Example:*
```xml
<genxml>
    <deps list="true">
        <genxml>
            <ctrltype>js</ctrltype>
            <url>{jquery}</url>
            <ecofriendly>true</ecofriendly>
        </genxml>
        <genxml>
            <ctrltype>css</ctrltype>
            <url>/DesktopModules/DNNrocket/css/w3.css</url>
            <ecofriendly>true</ecofriendly>
            <ignoreonskin>rocketw3</ignoreonskin>
        </genxml>
        <genxml>
            <ctrltype>css</ctrltype>
            <url>{domainurl}{appthemefolder}/css/HtmlContent.css</url>
            <ecofriendly>true</ecofriendly>
        </genxml>
    </deps>
    <moduletemplates list="true">
        <genxml>
            <file>view.cshtml</file>
            <name>List View</name>
            <cmd>list</cmd>
        </genxml>
        <genxml>
            <file>hozcatmenu2lvl.cshtml</file>
            <name>Horizontal Category Menu (2 levels)</name>
            <cmd>catmenu</cmd>
        </genxml>
    </moduletemplates>
    <adminpanelinterfacekeys list="true">
        <genxml>
            <interfacekey>articleadmin</interfacekey>
            <show>true</show>
        </genxml>
        <genxml>
            <interfacekey>categoryadmin</interfacekey>
            <show>true</show>
        </genxml>
        <genxml>
            <interfacekey>propertyadmin</interfacekey>
            <show>true</show>
        </genxml>
        <genxml>
            <interfacekey>settingsadmin</interfacekey>
            <show>true</show>
        </genxml>
        <genxml>
            <interfacekey>rocketdirectoryadmin</interfacekey>
            <show>true</show>
        </genxml>
    </adminpanelinterfacekeys>
    <queryparams list="true">
        <genxml>
            <queryparam>articleid</queryparam>
            <tablename>rocketdirectoryapi</tablename>
            <systemkey>rocketnewsapi</systemkey>
            <datatype>article</datatype>
        </genxml>
    </queryparams>
    <searchindex list="true">
        <genxml>
            <searchtitle>genxml/lang/genxml/textbox/title</searchtitle>
            <searchdescription>genxml/lang/genxml/textbox/richtext</searchdescription>
            <searchbody>genxml/lang/genxml/textbox/richtext</searchbody>
        </genxml>
    </searchindex>
</genxml>
```
