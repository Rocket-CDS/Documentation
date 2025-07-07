# Adding Resx

Translation can be done using resx files.  You can add these to a AppTheme by creating a resx file in the resx folder of the AppTheme.

Create a folder..

```plaintext
/DesktopModules/RocketThemes/AppThemes-W3-CSS/rocketcontentapi.example1/1.0/resx
```

Create a file called "example1.resx" and "example1.#culturecode#.resx"  in this folder.  The naming convention is to use the same name as the AppTheme.  
If you are using Visual Studio simply add a resx file to this folder and enter your resx data.  If you are using a different editor you will need to use XML format. 

If you are not familiar with resx files you should find out more.  But, basically the default language is "example1.resx", the French translation file is "example1.fr-FR.resx".

```plaintext
  <?xml version="1.0" encoding="utf-8"?>
<root>
  <data name="backgroundcolor.Text" xml:space="preserve">
    <value>Background Color</value>
  </data>
  <data name="backgroundcolorclass.Text" xml:space="preserve">
    <value>Background Color Class</value>
  </data>
</root>
```

After the file has been added it can be used in the template by adding a token.

```plaintext
    @AddProcessDataResx(appThemeView, true);
```