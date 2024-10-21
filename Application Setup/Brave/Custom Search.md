##### DuckDuckGo Custom Default Search 
duckduckgo !shebang for added privacy, cleaner search UI 
- set to browser homepage / new tab url

Search Engine > Manage Search Engines and Site Search 
Add > e.g. :
```
Name: Custom DDG 
Shortcut: ddg
URL with %s in place of query: https://duckduckgo.com/?q=%s&t=brave&kc=-1&kav=1&k1=-1&kax=-1&kak=-1&kaq=-1&kap=-1&kao=-1&kau=-1&kz=1&kp=-2&kae=d&kac=-1`
```
The `URL` is Brave-specific (`&t=brave`) to enable "Make default" in Search engines after adding to Site search. 

Included in above url:
```sh
#Safe Search kp = -2 for Off
"kp":"-2"}
#Instant Answers -need for translation
"kz":"1"}
#Infinite Scroll for Images, Videos, and Shopping
{"kc":"-1"} 			
#Infinite Scroll - Loads more results when scrolling
{"kav":"1"}				
#Autocomplete Suggestions - Shows suggestions under the search box as you type
{"kac":"-1"}
#Advertisements
{"k1":"-1"}			
#Install DuckDuckGo
{"kax":"-1","kak":"-1"} 
#Privacy Newsletters
{"kaq":"-1","kap":"-1"} 
#Homepage Privacy Tips
{"kao":"-1"}
#Help Improve DuckDuckGo
{"kau":"-1"}
#Theme - d for Dark
&kae=d
#Autoload Images 
kbc=1 
```


**Additional Params**
https://help.duckduckgo.com/settings/params/







