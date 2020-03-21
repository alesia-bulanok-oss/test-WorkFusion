Hi! Here is a document that you need to peer review. Please convert this document to [Markdown](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet) using the editor of your choice (Atom, Sublime Text, etc.). Then, submit your Markdown file to [github.com](https://github.com/). To do this, you may need to create an account and a repository. Note that the repository needs to be public so we can review your work.

If you feel that some edits are a matter of style, follow the [Google Developer Style Guide](https://developers.google.com/style/commas), or just leave your considerations as a comment. Thank you, and good luck!

---------------

**Bot Tasks** contain jobs for WorkFusion's Bots. The Bots are designed to perform those tasks in a business process (BP) that are better suited for automation than human labor. Bot Tasks can exist only as a business process step.

**For example:** address parsing, determining the width and height of an image, interacting with your API or repository, or filtering content incompliant with certain criteria.

**Bot Configs** are written using the Web-Harvest library and are executed for each Record separately. For writing the Web-Harvest XML configs, you can use the pre-defined Web-Harvest XML elements described here or WorkFusion plugins.

**The Bot Config Bundle** is a bundled Java library containing a bot config and a java code reference for the bot config.

# How to Create, Test, and Run

1.  Create and test a Web-Harvest config using the WorkFusion Studio (an Eclipse-based IDE).

2. Create a Bot Use Case in WorkFusion. Choose a Bot Use Case type.

   **Note:** The best option is ETL because it is better suited for reusing and adds flexibility.

3. Create a BP and include a Bot Step based on this Use Case. Alternatively, you can use the WorkFusion Repository and import the Bot Config Bundle to WorkFusion.

4. Test the Bot Config in the BP using a small input batch.

5. Make changes, if needed. Adjust the column names, datastore connection, output column names, use proxy.

6. Update the created Bot Use Case and run it in your production BP.

Alternatively, you can create a Bot Task in the BP (the Design Process tab) from scratch (Blank Use Case) and paste your code. However, this approach leads to multiple issues.

# Execution Details

The Web-Harvest XML configuration is executed for each submission. Thus, if your input CSV file contains 40 Records, the XML configuration is executed 40 times—once for each Record.

If an error occurs, the Bot Config is run 5 times.

# Results

The results of a Bot Task are defined in the \<export\> plugin settings.

All columns created by the Bot Task can be used in further BP steps (both Manual and Bot).

# **Hello World Example**

Below, you will find a Bot Config example enabling the following: 

1. It gets an HTTP response from the URLs listed in the **url_to_check column** of the input data ﬁle. 
2. It records the HTTP response into the **http_response** variable. 
3. It exports all original columns and appends a new **http** column containing the **http_response** variable value.

**Bot Config Example**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<config>
 <!--defining variable-->  
 <var-def name="http_response">    
   <!--passing an appropriate value from the url_to_check column into the input data file as a parameter for the http plugin-->  
   <http url="${url_to_check}"></http>
 </var-def>
    
    
 <!--exporting all original input columns-->
 <export include-original-data="true">   
  <!--adding a new column with the http plugin result to the export file-->    
  <single-column name="http" value="${http_response}"/>
 </export>
   
    
</config>
```

