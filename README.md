# Status
[![Project Status: Inactive – The project has reached a stable, usable state but is no longer being actively developed; support/maintenance will be provided as time allows.](https://www.repostatus.org/badges/latest/inactive.svg)](https://www.repostatus.org/#inactive)

# Qlik Field Usage Automation

- [About](#about)
  * [What is it](#what-is-it)
  * [What makes this different from other tools or products](#what-makes-this-different-from-other-tools-or-products)
  * [When is this automation not a fit](#when-is-this-automation-not-a-fit)
  * [Video Overview and Demo](#video-overview-and-demo)
- [Screenshots](#screenshots)
  * [Generated App](#generated-app)
  * [Automation Output](#automation-output)
- [Capabilities](#capabilities)
- [Setup](#setup)
- [Running the automation](#running-the-automation)
  * [Via the Automation Directly](#via-the-automation-directly)
  * [Via another Automation Directly](#via-another-automation-directly)
  * [Via REST](#via-rest)
- [Definitions](#definitions)
- [Updates and Versioning](#updates-and-versioning)
- [Current Scope and Limitations](#current-scope-and-limitations)
- [Field and Variable Recognition](#field-and-variable-recognition)
  * [Field Recognition](#field-recognition)
  * [Variable Recognition](#variable-recognition)
  * [Nested Variable Referencing](#nested-variable-referencing)

## About
### What is it
A Qlik Application Automation template and programmatically generated app for Qlik Sense apps in Qlik Cloud that exposes field usage and allows for impact analysis. In short, this application allows the user to quickly identify and drop unused fields in a Qlik Sense application, as well as perform impact analysis for Fields, Variables, and Master Items. 

> _Keep your Qlik Sense apps slim and performant, and gain an understanding of where and how fields are used._

### What makes this different from other tools or products
- This automation is completely native to Qlik Cloud, and has zero footprint (i.e. it requires no additional hardware or external code/programs)
- As this is an automation, it can be run as part of a pipeline/workflow in Qlik Cloud. As an example, one could create a publish process that  runs this automation, and if the contains more than ~20% unused RAM, it is kicked back to the owner to drop X fields with the generated statement.
- This automation is open source.

### When is this automation not a fit
- If the goal is to optimize the objects and/or the fields/expressions themselves, this is not the utility to do so as it does not "exercise" the objects nor does it provide recommendations to optimize the fields. The [App Evaluator](https://community.qlik.com/t5/Qlik-Product-Innovation-Blog/Manage-performance-with-the-new-Application-Evaluator-available/ba-p/1799878) which is native to Qlik Cloud can assist with identifying poorly performing objects, and other third-party document analyzers offer more prescriptive approaches to field optimization.

### Video Overview and Demo
[![Field Usage Overview Youtube](/images/youtube_screen.png)](https://www.youtube.com/watch?v=xS7KmAjboWk)

## Screenshots
### Generated App
![](/images/field_usage_2.png)
![](/images/field_usage_3.png)
![](/images/field_usage_4.png)

### Automation Output
![](/images/run3.png)

## Capabilities
This automation can identify unused:
- Fields
- Variables
- Master Objects
- Master Measures\*
- Master Dimensions\*

Regarding impact analysis, this automation traces field usage across:
- Objects
- Master Objects
- Variables
- Nested Variable References
- Bookmarks
- Master Dimensions*
- Master Measures*

\* Note that this automation does not currently support the identification of Master Measure/Master Dimension references, e.g. using [Total Sum of Sales] (the name of a Master Measure) as a reference in an expression. This does not affect the algorithm used for determining unused fields, but it would not show those references for impact analysis.

## Setup
Ultimately, all that needs to be done to configure this automation is to import the `automation_template.json` template into a new, blank automation in Qlik Cloud. There are however some nuances as well as tips and tricks to this, so the suggested flow is documented below:

1. On this GitHub repository, select the **Code** button on the top right, and then select **Download zip**.

![](/images/setup1.png)

2. Once downloaded, unzip the file and locate the `automation_template.json` file. This is the automation template that will be uploaded into Qlik Cloud.

4. Navigate to Qlik Cloud, and select **Add new**, then select **New automation**.

![](/images/setup2.png)

4. A modal will appear where a template can be selected. Find the **Blank automation** template, hover over it, and select **Use template**.

![](/images/setup3.png)

5. Provide a Name and optionally a Description, and then select **Save**.

![](/images/setup4.png)

6. In the new automation editor, mouse-over anywhere on the canvas (the whitespace) and right-click. An options dropdown will appear. Select **Upload workspace**. Be patient while the workflow is generated (it might look broken at first and the UI might lock up).

![](/images/setup5.png)

    ---
    **IMPORTANT!**
    This process can take several minutes as this automation is complex. This process only needs to be done a single time, and this screen never has to be returned to, as the automation can be triggered from the Overview in the future which does not trigger the rendering of the automation workflow.

    ---
7. Once the UI is fully rendered (make sure by clicking on something and confirming that the UI reacts), select the **Save** button on the top right. This can take ~10 seconds until it is complete.

![](/images/setup6.png)

8. Now that the template has been uploaded and saved, return to the Hub, and then open the automation by simply hovering over it and clicking **Open automation**. This ensures that the heavy rendering of the workflow GUI has been flushed, and this will take the user straight to the **Overview** section of the automation. This will keep the UI speedy, and this is the suggested way to always open and run the automation in the UI, as the automation itself never needs to be edited and will slow down the browser.

![](/images/setup7.png)

9. The automation is now ready to be run!

## Running the automation
### Via the Automation Directly
1. In the Hub, open the automation by simply hovering over it and clicking **Open automation**. This will take the user straight to the **Overview** section of the automation. This will keep the UI speedy (vs going to the Editor), and is the suggested way to always open and run the automation in the UI, as the automation itself never needs to be edited and will slow down the browser.
2. On the top right of the screen, select **Run**

![](/images/run1.png)

3. A number of user inputs will appear, however the only required input is **AppId to Scan**, which is the AppId of the Qlik Sense application that it should scan. If only that input is entered and submitted, the automation will just output the results directly on the output screen. This is helpful if the user only wants the DROP FIELDS statement generated for that Qlik Sense application and does not care about any additional analysis that could be done in a generated Qlik Sense application.
3a. The additional inputs are shown and documented below (directly in the automation) and provide the user the ability to generate an output application (integral for impact analysis), as well as to control what types of sheets are scanned (Base or All).

![](/images/run2.png)

4. Once the desired inputs have been filled out/selected, select the **Submit** button to start the automation.
5. The automation can take anywhere from ~1 minute to ~30 minutes or more depending on the amount of objects/sheets that are to be scanned. Scanning _Base_ sheets only decreases the execution time by eliminating the scanning of _Community_ or any of the executing user's _Personal_ sheets.
6. Once complete, the output will be displayed directly on the screen, as well as in an app if that selection was toggled. 
7. If an application was generated, it will appear in the selected space (Personal if no space was selected), and should be fully populated and reloaded with all of the output data.

### Via another Automation Directly
If this automation is to be triggered via another automation, ensure that the following required inputs are passed:
- AppId to Scan
- Scan Base Sheets Only (Yes/No input)
- Generate Output App (Yes/No input)
- Use Custom Template (Yes/No input)

### Via REST
If its desired to trigger this automation remotely via REST, all of the inputs can be passed as parameters. The required parameters are:
- AppId to Scan
- Scan Base Sheets Only (Yes/No input)
- Generate Output App (Yes/No input)
- Use Custom Template (Yes/No input)

The full path to the URL and X-Execution-Token along with example call templates can be found by navigating to the Editor of the automation template and clicking on the **Start** block.
![](run4.png)

## Definitions
**Unused Fields**
- A field is determined to be unused if it is not found in any Object, Master Dimension, Master Measure, Master Object, Bookmark, or Variable and that field is also not a Key field. If a field is found only in a Master Measure and that Master Measure is not used, the field will still be marked as used.

**Unused Variables**
- A variable is determined to be unused if it is not in any other variable that is used, it is not a script created variable, and it isn't found in any Object, Master Dimension, Master Measure, Master Object, or Bookmark.

**Unused Master Objects**
- A Master Object is determined to be unused if it is not found on any scanned sheet.

**Unused Master Dimensions/Master Measures**
- Master Dimensions and Master Measures can be determined to be unused if they are not found on any scanned sheets, **_however_** this automation does not scan for Master Dimension or Master Measure references, so this cannot be guaranteed.

## Updates and Versioning
The automation will automatically let you know (in its Output) when a new version of the template is available to be downloaded from this location. Updates are not forced, as the template is locked to a specific version (branch), so it is up to the user when a new template should be fetched.

## Current Scope and Limitations
This automation does not currently support:
- Scanning of QVW files. Only Qlik Sense apps are supported
- Scanning Master Measure and Master Dimension references (e.g. [Total Sum of Sales] used in an expression)
- Logging of parent objects (containers, filterpanes) that do not have matches in any child objects (e.g. a container that holds a text object with no matches -- the text object will still be logged with no hit, but the parent container will not)

This automation is currently limited to:
- Scanning a single Qlik Sense application at a time

## Field and Variable Recognition
### Field Recognition
The field scanning algorithm classifies fields that are recognized into two types:
- Exact
- Wild

An _Exact_ match is captured when the string value matches any of the following patterns:
- `[{fieldName}]`
- `"{fieldName}"`
- `({fieldName})`
- `` `{fieldName}` ``

or if the string value matches the following verbatim:
- `={fieldName}`
- `{fieldName}`

A _Wild_ match is captured when an _Exact_ match is not found, but that field is found as part of a substring.

In the output app, the field `"FieldExactMatchFound"` indicates if there was ever an exact match of that field found in the scan (meaning it must be used), whereas if an exact match was never found, that field's value will be set to `False`. The user can then manually identify in the app by looking at the expressions that were captured to see if the field is in fact actually used. The automation is in this way overly conservative, in that it might indicate a number of fields are used when they in fact are not, but the opposite should never be true.

### Variable Recognition
The variable scanning algorithm classifies variables that are recognized into two types:
- Exact
- Wild

Does the Dimension entity contain an exact match ($({Variable}), $(={Variable}), ({Variable})) or does the entire entity match the Variable name exactly?

An _Exact_ match is captured when the string value matches any of the following patterns:
- `$({variableName})`
- `$(={variableName})`
- `({variableName})`

or if the string value matches the following verbatim:
- `={variableName}`
- `{variableName}`

A _Wild_ match is captured when an _Exact_ match is not found, but that variable is found as part of a substring.

### Nested Variable Referencing
Variables are also scanned recursively for the presence of other variable names, and thereby associated to any fields that those nested variables contain. 

For example, if a Qlik Sense application contains the following three variables:
- Name: `vCost`, Definition: `=Sum(Cost)`
- Name: `vSales`, Definition: `=Sum(Sales)`
- Name: `vProfit`, Definition: `$(vSales) - $(vCost)`

`vProfit` will be associated with the fields `"Cost"` and `"Sales"`, along with any asset that uses that variable (e.g. if an Object uses `vProfit`, it will also be associated with those fields.)
