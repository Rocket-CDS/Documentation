# Snippets - RocketTokens
These snippets and examples give help for building AppThemes.    

### ResourceCSV
This token is used to add a CSV list of localized values.  The list is the key of the value in the resx file.  Giving the method a CSV list of keys will return the value content in the resx file.  This is primarily used for DropDownList and RadioButtonList tokens.

*Defined as*:
```
ResourceCSV(String resourceFileKey, string keyListCSV, string lang = "", string resourceExtension = "Text")
```

*Example to output a CSV list*:
```
@ResourceCSV(String resourceFileKey, string keyListCSV, string lang = "")
```
*Example using DropdownList token*:
```
@DropDownList(info,"genxml/select/type", "type1,type3,type4", ResourceCSV("MyResx", "type1,type3,type4").ToString())
```
The above example should have data keys and values in the "MyResx" file.
```
type1.Text = "Type One"
type2.Text = "Type Two"
type3.Text = "Type Three"
```

*To display the localized value use the ResourceKey with the value added:*

```
@ResourceKey("MyResx." + info.GetXmlProperty("genxml/select/type"))
```
