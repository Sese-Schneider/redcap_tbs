REDCap TBS
==========
The REDCap TBS plugin generates formatted reports for individual REDCap records using the Tiny But Strong PHP template engine. Template files can be written as HTML/CSS for layout and formatting. Word (.docx) template files can also be used.

__How it works:__ The template file includes placeholder tags for REDCap field names. The TBS template engine replaces these tags with the actual values from REDCap for the specific record.
 
__How it's used:__ REDCap TBS can be used to generated form letters with dynamic content based on REDCap variables. It can also be used to print the content of REDCap data collection instruments in a more concise format than the default PDF feature.

### Requirements
* REDCap
  * version 6.2.2+ (not fully tested on earlier versions)
  * Bookmarks enabled
* [TinyButStrong](http://tinybutstrong.com)  3.9 - file included with plugin (licensed under LGPL)
* [OpenTBS](http://www.tinybutstrong.com/plugins/opentbs/tbs_plugin_opentbs.html) 1.9.2 - creates Open Office and MS Office documents. File included with plugin (licensed under LGPL)
* Familiarity with editing HTML/CSS files


### Installation
Download the .zip archive and extract the redcap_tbs folder into the REDCap plugins folder (redcap/plugins/redcap_tbs/)

### Initial Testing

The .docx templates depend on the OpenTBS plugin for TinyButStrong. Confirm that the installation is configured correctly by opening `https://YOUR_SERVER/redcap/plugins/redcap_tbs/demo/index.php`. Follow the prompts and use the OpenTBS documentation/support forums for further troubleshooting if necessary. 

#### 1. Add the Template Files

The template subfolder contains the template files. Template files must contain valid HTML/CSS  or be a valid .docx file. Additional subfolders can be used to organize templates by project. 

REDCap variables are inserted into the template content with this tag format `[onshow.data.foo]` where 'foo' is a REDCap Project field name. 

* Basic usage - `<P>Hello [onshow.data.name]</P> ` ==> `Hello Peter`  
* Reformatting dates - `<P>Date: [onshow.data.date; frm='m/d/yy']</P> ` ==> `Date:  9/1/13`  
* Dynamic images (HTML) - ``<IMG SRC="./[onshow.data.cardiology]-small.png?raw"</IMG>'`` ==> An image name can be based on the value of a REDCap field. The image files can be located in the same folder as the template. 
* Checkbox Fields - There are two ways to show checkbox fields - display literal checkmarks or show the values of the selected options. 

This example uses a REDCap checkbox field-type named 'checkboxes' with three options (as shown below) and a record where the first and third option have been selected.

```
1, Check #1
2, Check #2
A, Check A
```

  __Show Checkmarks__ - Displays a checked box character next to the selected option and an empty box next to unselected options.  

This TBS code block in HTML
```html
    [onload.data.checkboxes___1;if [val]='Checked';then '&#9745';else '&#9744'] Check #1<BR>
    [onload.data.checkboxes___2;if [val]='Checked';then '&#9745';else '&#9744'] Check #2<BR>
    [onload.data.checkboxes___a;if [val]='Checked';then '&#9745';else '&#9744'] Check A<BR>
```

Will show something like this:

&#9745; Check #1  
&#9744; Check #2  
&#9745; Check A  


  __Show Values__ - Displays the values when options are selected. 
This TBS code block in HTML
```html
    [onload.data.checkboxes___1;if [val]='Checked';then 'Check #1';else '']
    [onload.data.checkboxes___2;if [val]='Checked';then 'Check #2';else '']
    [onload.data.checkboxes___a;if [val]='Checked';then 'Check A';else ''] Check A<BR>
```

Will show something like this:

Check #1, Check A 

Additional information can be found on the [http://tinybutstrong.com/examples.php](TinyButStrong Examples) page and in the TinyButStrong [http://tinybutstrong.com/manual.php](Documentaion Manual).


#### 2. Add a new Project Bookmark.  

* Link Type - Select Advanced Link. This verifies the identity of the user.
* User Access - Selecting 'All users' will give access to any user that has access to this project. The 'Selected Users' option can be used to restrict the link to specific users. 
* Link URL - `https://YOUR_SERVER/redcap/plugins/redcap_tbs/index.php?template=example.html`
	* change 'EXAMPLE.ORG' to the domain name of the REDCap installation.
	* OPTIONS:
		* change the value after 'template='  to the desired template filename.
		* Disable the REDCap Project header by adding `&header=false`
		* Template subfolders can be accessed by adding the folder names to the template parameter (ie. `?template=fruit/demo.docx`
* Check the boxes for "Append record ID" and "Append project ID".


### Feedback
You are welcome to:
* submit suggestions and bug-reports at: https://github.com/rparrish/redcap_tbs/issues
* compose a friendly email to: rollie.parrish@ampa.org


#### Authors/Contributors
* __Author__: Rollie Parrish (AMPA)
* __Contributor: [@RHeib](https://github.com/RHeimb)
