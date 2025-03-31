# System Rules
Each API module is defined into "systems", these systems are the base functionality of the module.  They can be extended by other defined systems inheriting the base system or by a plugin.

The **system.rules** file defines the system interfaces and providers.

This file is usually on the system root path.  However with a plugin the file is copied to the "%system%/plugin" folder.  

## Format 
```
<genxml>
    <systemkey>rocketblogapi</systemkey>
    <basesystemkey>rocketdirectoryapi</basesystemkey>
    <systemname>RocketBlog</systemname>
    <sqlindex list="true"></sqlindex>
    <interfacedata list="true"></interfacedata>
    <providerdata list="true"></providerdata>
    <groupsdata list="true"></groupsdata>
</genxml>
```

### systemkey
The unique name of the system.
### basesystemkey (optional)
Identifies if the system inherits a base system.  
### systemname (optional)
Friendly Name
### plugin
Identifies if the module is a base system or a plugin to an existing system. (Default value is false.)

### sqlindex (optional)
Sorting on XML is very slow and an index should be created to deal with sorting XML.  
*Selecting on XML is quick and no real need for a special index column to be created.*
```
<genxml>
    <systemkey>rocketblogapi</systemkey>
    <ref>articlename</ref>
    <xpath>genxml/lang/genxml/textbox/articlename</xpath>
    <typecode>rocketblogapiART</typecode>
</genxml>
```
### interfacedata
These create UI menu options and process on the API. 
The *interfacekey* is unique and is used for access to the API.  It redirects any API calls to the correct system.

```
\DesktopModules\DNNrocket\API\ApiControllers\RocketController.cs
```
It is used to display commands on the menu and to check the user has security.
The "groupref" can be added if you require the option to appear under a group/category.
NOTE: The defaultcommand MUST be valid for the user or the interface will not appear on the menu.

**Example:**
```
<genxml>
    <textbox>
        <interfacekey>rocketintrarecommend</interfacekey>
        <namespaceclass>RocketIntraRecommend.API.StartConnect</namespaceclass>
        <assembly>RocketIntraRecommend</assembly>
        <interfaceicon>recommend</interfaceicon>
        <defaultcommand></defaultcommand>
        <relpath>/DesktopModules/DNNrocketModules/RocketIntraRecommend</relpath>
    </textbox>
    <providertype></providertype>
    <dropdownlist>
        <group></group>
    </dropdownlist>
    <checkbox>
        <onmenu>false</onmenu>
        <active>true</active>
    </checkbox>
    <radio>
        <securityrolesadministrators>1</securityrolesadministrators>
        <securityrolesmanager>1</securityrolesmanager>
        <securityroleseditor>1</securityroleseditor>
        <securityrolesclienteditor>1</securityrolesclienteditor>
        <securityrolesregisteredusers>0</securityrolesregisteredusers>
        <securityrolessubscribers>0</securityrolessubscribers>
        <securityrolesall>0</securityrolesall>
    </radio>
    <notes>
    </notes>
</genxml>

```

### providerdata

Providers are assemblies that are used by the system, these are lose coupled to the system and are initiated by creating an instance based on the provider assembly, namespaceclass.  

Providers are functionality like "scheduler", "events" and "plugins".  

The "providertype" set to "plugin" will make the provider appear on the list of plugins for a system.   

### groupsdata (optional)
This is a list of groups that can exist on the menu, the term group refers to a top level menu.  Each UI interface can be a submenu of the group.  Some systems do not have sub-menus, in which case this can be ignored.
```
<groupsdata list="true">
    <genxml>
        <textbox>
            <groupref>recommend</groupref>
            <groupicon>recommend</groupicon>
        </textbox>
    </genxml>
</groupsdata>
```
Building of the menu is done in razor.
**Example:**
```
\DesktopModules\DNNRocketModules\RocketIntra\Themes\config-w3\1.0\default\AdminPanel.cshtml
```
NOTE: Only 2 levels are supported.  

## Usage
This file is read by the **SystemLimpet** class.  Which is then used to control the system interfaces.  

The SystemLimpet class can be considered as the controlling unit for a system, each system must have one.  

