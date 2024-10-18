# Adding CSS and JS files

AppThemes often require their own CSS and JavaScript files.  This can be done inline in the template or by linking to a file.

#### Adding a CSS file

Create a folder..

```plaintext
/DesktopModules/RocketThemes/AppThemes-W3-CSS/rocketcontentapi.example1/1.0/css
```

Create a CSS file called "example1.css" in this folder and add any CSS required.  
The CSS file is added to the AppTheme by using the dependency file.

Create a folder called "dep" in the AppTheme

```plaintext
/DesktopModules/RocketThemes/AppThemes-W3-CSS/rocketcontentapi.example1/1.0/dep
```

Create a file called "example1.dep" with the following XML format.  This example is using the {appthemefolder} token.

```plaintext
<genxml>
    <deps list="true">
        <genxml>
            <ctrltype>css></ctrltype>
            <url>{appthemefolder}/css/example1.css></url>
			<ecofriendly>true</ecofriendly>
		</genxml>
    </deps>
</genxml>
```

This ensures that the resource "example1.css" is not loaded onto the page multiple times if multiple AppThemes require it.

#### Adding a JS file

Create a folder..

```plaintext
/DesktopModules/RocketThemes/AppThemes-W3-CSS/rocketcontentapi.example1/1.0/js
```

Create a JS file called "example1.js" in this folder and add any JS required.  
The JS file is added to the AppTheme by using the dependency file.

Create a folder called "dep" in the AppTheme

```plaintext
/DesktopModules/RocketThemes/AppThemes-W3-CSS/rocketcontentapi.example1/1.0/dep
```

Create a file called "example1.dep" with the following XML format.  This example is using the {appthemefolder} token.

```plaintext
<genxml>
    <deps list="true">
        <genxml>
            <ctrltype>css></ctrltype>
            <url>{appthemefolder}/css/example1.js></url>
			<ecofriendly>true</ecofriendly>        
        </genxml>
    </deps>
</genxml>
```

This ensures that the resource "example1.js" is not loaded onto the page multiple times if multiple AppThemes require it.