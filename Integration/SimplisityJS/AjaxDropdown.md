# Ajax DropDownList Example
DropDownLists can be populated by ajax. This example shows 2 dropdownlists, a country list and a region list. When the country list is selected the second list is populated with region data.  

```
<div>
<label>@ResourceKey("businessdirectory.country")</label>
@{
var countrydata = DNNrocket.Country.CountryUtils.CountryListCSV("amylisbusiness");
}
@DropDownList(info, "genxml/dropdownlist/country", "," + countrydata[0].Replace("'",""), "," + countrydata[1].Replace("'", ""), "class='w3-input w3-border simplisity_change' s-cmd='settingcountry_getregion' s-dropdownlist='#region'")
</div>
<div>
<label>@ResourceKey("businessdirectory.region")</label>
@{
var regiondata = DNNrocket.Country.CountryUtils.RegionListCSV(info.GetXmlProperty("genxml/dropdownlist/country"));
}
@DropDownList(info, "genxml/dropdownlist/region", regiondata[0].Replace("'",""), regiondata[1].Replace("'", ""), "class='w3-input w3-border' ")
</div>
```

NOTE: use s-fields='{"allowempty":"true"}' to allow an empty selection in the dropdown.
The client side display is a server razor template. The country dropdown is created by a razor token "@DropDownList" and the initial load populates the dropdown by using the server side "CountryListCSV" function.

The "simplisity_change" class on the country dropdown identifies that a simplisity change event will be triggered when a country is selected. The "s-dropdownllist" command attribute is defined as the jQuery selector for the region dropdownlist, this identifies the dropdownlist that will be populated by ajax.

On inital load we populated the region dropdownlist with the regions for the selected country dropdownlist and the razor token "@DropDownList" deals with the selected value.

The "s-dropdownlist" command attribute requires a json object to be returned by the server. The server returns a json string and this is converted into a json object, by simplisity, which is used to populate the region dropdownlist.  

#### Json Return Format
```
{listkey: ["'key1','key2','key3'"], listvalue: ["'name1','name2','name3'"] }
```
The json return data should have 2 lists, "listkey" and "listvalue", The server side code should place the json string into the return dictionary with a key value of "outputjson"

This functionality uses the "activevalue" param pasted to the server by simplisity.

This functionality must have server side code that returns the correct values.
```
...
case "settingcountry_getregion":
    rtnDic.Add("outputhtml", "");
    var regionlist = CountryUtils.RegionListJson(paramInfo.GetXmlProperty("genxml/hidden/activevalue"), paramInfo.GetXmlPropertyBool("genxml/hidden/allowempty"));
    rtnDic.Add("outputjson", regionlist);
    break;
...
```
```
public static object RegionListJson(string countrycode, bool allowempty = true)
{
    var jsonList = new List<ValuePair>();
    var valuePair = new ValuePair();
    if (allowempty)
    {
        valuePair.Key = "";
        valuePair.Value = "";
        jsonList.Add(valuePair);
    }
    foreach (var i in DNNrocketUtils.GetRegionList(countrycode))
    {
        valuePair = new ValuePair();
        valuePair.Key = i.Key;
        valuePair.Value = i.Value;
        jsonList.Add(valuePair);
    }
    return jsonList;
}
```
