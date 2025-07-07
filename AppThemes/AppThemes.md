# AppThemes

An AppTheme is a group of razor templates to render content on a display or admin page.
You do not need to know razor to build an AppTheme.  Rocket AppThemes use a standard format to help.

**If you do not know HTML or CSS, you should learn that before trying to build your own AppTheme.**

## DNN AppTheme Folders
All AppThemes are stored within the DNN filesystem.  

```
\DesktopModules\RocketThemes\#AppThemeProjectName#\#systemkey#.#AppThemeName#\#version#
```
### All AppThemes
```
\DesktopModules\RocketThemes
```

### AppTheme Project Folder
Each AppTheme Project is kept in GitHub.  The Repository name is the AppTheme project name.  
*Example: AppThemes-W3-CSS*

```
\DesktopModules\RocketThemes\AppThemes-W3-CSS
```

### Folder Structure
Each AppTheme has a name prefix matching the system it is created for.  The direct sub-folder is the version, the "default" folder beneath the version is for the razor templates.  Other folders are optional depending on what files exist in the AppTheme.  
*Example: rocketcontentapi.HeroHeader*
```
rocketcontentapi.HeroHeader
    >1.0
        >css
        >default
        >dep
        >img
        >js
        >rex
```

