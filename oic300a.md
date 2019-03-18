<img class="float-right" src="images/j2c-logo.png">

#  **Lab 300 - Part A: Oracle Integration Cloud (OIC) Development Workshop**

# **HCM and EBS Integaration**
> ***Last Updated: March 2019***  

# Fusion HCM with ATOM Feeds Development Lab

## **Introduction**

> This lab is part of a series of OIC Development workshops created to provide users with hands-on experience building functional integrations in the cloud using Oracle Integration Cloud. In this lab, we will configure the connections and other parts of OIC that will be utilized in Lab 300B. This lab assumes that you are already somewhat familiar with the OIC interface and logged into the instance.

> In this lab, you create an integration using the Scheduled Integration Orchestration. The integratio will be responsyble for synhronizing a newly created employee in HCM into EBS.

> The OIC integration that we'll be building is shown in the following picture:

![](images/300/image000.png)

> Here is a description of what is happening with this integration:

 > After a new employee had been created in Oracle HCM Cloud, HCM Atom feeds is triggered by Scheduled Integration. HCM Atom feeds provide notifications of Oracle Fusion Human Capital Management (HCM) events. When an event occurs in Oracle Fusion HCM, the corresponding Atom feed is delivered automatically to the Atom server. The feed contains details of the REST resource on which the event occurred. Subscribers who consume these Atom feeds use the REST resources to retrieve additional information about the resource, in our case it is a newly created employee. After, through EBS connector and proper mapping, that emploeey is synchronized with EBS application. 


## **Objectives**

> 1. Set up HCM Adapter.
> 2. Set up EBS Adapter.
> 3. Create Scheduled Integration.
> 4. Configure the HCM Connection.
> 5. Add a Function Call.
> 6. Mapping.

## **Required Artifacts**

- The following lab and an Oracle Integration Cloud account that will be supplied by your instructor.
- A HCM instance and connection URL that will be provided by your instructor.
- An EBS instance and connection URL that will be provided by your instructor.


---
## Login to OIC Integration Home Page.

- From your browser (Firefox or Chrome recommended) login to the OIC Integration Console using the following URL:

OIC Integration Home Page - https://**OAIC Instance URL**/ic/integration/home/faces/global

**OAIC Home Page URL** provided by your instructor

- Enter your `User Name` and `Password` and click **Sign In** if required

***NOTE:*** the **User Name and Password** values will be given to you by your instructor.

![](images/300/image002.png)  


You will now be presented with the OIC Service Console from which you will be performing the rest of this workshop lab.

![](images/300/image004c.png)

Select the an Integration Tab as it shown bellow

![](images/300/image04c.png)


## **300a.1: HCM Adapter Set Up**

- Select Integrations to open up Integration console. Then click the `Connections` in the left menu under Designer. And click on **Create** in the upper-right

	![](images/300/image006.png) 

- Select the **Oracle HCM Cloud** Connection, by either doing a search, or by scrolling down to the  connection, then click on the **Select** button of the HCM connection.

	![](images/300/image007.png)

- Fill in the information for the new connection 

	- **Name:** Enter in the form of _UserXX HCM where XX is the initials of the user.
	- **Role:** Select _Trigger and Invoke_ since OIC is smart enough to select one or another based on a use case of that connection.

    ![](images/300/image007a.png)

Note that the **Identifier** will automatically be created based on the **Name** you entered.

- Click **Create**

	![](images/300/image008.png) 

- Click the **Configure Connectivity** button


- For the *WSDL URL*, enter the property value for the WSDL as follows:
	_hhttps://ucf1-edku-fa-ext.oracledemos.com/fscmService/ServiceCatalogService?WSDL_

Where _hhttps://ucf1-edku-fa-ext.oracledemos.com_ is your HSM instance URL

- Add an oPTIONAL Interface Catalog URL if you have one.
	![](images/300/image010.png) 

- Select **OK**

- Select the *Configure Security* button insert your HCM **Username Password Token**. 

	![](images/300/image014.png)

- At the top of the connection configuration screen, Click on the **Test** button to test the connection.

	![](images/300/image016.png)

Note how the progress indicator will go from 85% to 100% after the connection tests successfully.
 

- Click on the **Save** button in the upper right corner of the connection configuration screen. And click on the **Close** button in the upper right of the connection configuration screen.

	![](images/300/image018.png) 


Your new HCM connection appears in the list of configured connections.

![](images/300/image018a.png) 

## **300a.2: EBS Adapter Set Up**
---

- Select the `Connections` graphic in the designer portal and click on *Create*

	![](images/300/image006.png) 

- Select the *Oracle E-Business Suite* Connection, by either doing a search, or by scrolling down to the *Oracle E-Business Suite* connection, then click on the *Select* button of the *Oracle E-Business Suite* connection.

	![](images/400/image001.png)

- Fill in the information for the new connection 

	- *Name:* Enter in the form of _UserXX EBS OPERATIONS_ where XX is your initials.
	- *Role:* Select _Invoke_ since we going to use the connection to invoke APIs from EBS

Note that the *Identifier* will automatically be created based on the *Name* you entered.

- Click *Create*

	![](images/400/image002.png)

- Click the *Configure Connectivity* button

	![](images/400/image003.png)

- Enter the *Connection URL* which you will be given by your instructor.  It will be in format like the following: `https://ucf4-ebs0116-gse.oracledemos.com`.

- After entering the *Connection URL*, select the *OK* button to save the value.

	![](images/400/image004.png)

- Select the *Configure Security* button so we can change the default security configuration

	![](images/400/image005.png)

- Select the following options:

	- *Security Policy*: `Basic Authentication`
	- *Username*: `operations`
	- *Password*: `to be provided by your instructor`

After the security policy properties have been setup, click on the *OK* button to dismiss the dialog

![](images/400/image006.png)


- The connection needs to be tested by clicking on the *Test* button in the upper-right of the *UserXX EBS Operations* connection definition page.

	![](images/400/image009.png)

- Now, select the *Save* button to save the connection configuration.

	![](images/400/image010.png)

- Note that after the successful test, the percentage complete in the upper-right should go to *100%*.  After the save, a green banner message will appear in the top indicating a successful save operation.

- Finally, select the *Close* button to exit the connection configuration screen.

	![](images/400/image011.png)

- You will now see your new **Oracle E-Business Suite** connection on top of the **Connections** list.

	![](images/400/image012.png)

## **300a.3: Create Scheduled Integration**

- Select the *Integrations* link and click the **Create** button in the upper-right of the Integrations screen

	![](images/300/image033.png)

- In the **Create Integration - Select a Style**, select the **Scheduled Orchestration** style.  
That type of integration uses a schedule to trigger the integration instead of an adapter. Having Scheduled Orchestration you can schedule when to run it.

	![](images/300/lab3001.png)

- Fill in the information for the new orchestration

	- **What do you want to call your integration?:** Enter the name in the form of _UserXX HCM to EBS_ where XX is your initials.
	- **Identifier:** Accept the default - this value will be generated based on the name you enter.
	- **Version:** Accept the default - if you want to clone and create newer versions later, you can change to a higher version than **01.00.0000** which is the default.

After you've filled in the information, select the **Create** button

![](images/300/lab4001.png)

- Observe the design canvas for the new integration.  (The various features of the OIC designer were covered in lab 100 **Exploring OIC** earlier in this workshop)

	![](images/300/lab4001a.png)


### **300a.4: Configure the HCM Connection**
- Hover a pluss sign and search the HCM connection name that you created previously.

 ![](images/300/1.png)

- Right after you will see _Configure Oracle HCM Cloud Endpoint_ wizard will pop up.

 ![](images/300/2.png)

 - In the *Basic Info* section give the endpoint a name like `Get-HCM-Atom-Feed`.

 - After giving the invocation a name, select the *Next* button.

![](images/300/3.png)

- In the *Web Services* section select **Subscribe to Updates(via ATOM feed). That option allows to receive latest updates since a specific date on new hires, jobs etc. Select the *Next* button.

![](images/300/4.png)

- In the *Operations* section select _Employee New Hire_ as an atom feed. Afer, make shure you check the box for  _Include Business Object in the Event Notifications (ATOM Feeds)_. Select the *Next* button.

![](images/300/5.png)

- In the *Summary* section of the configuration wizard, review the HCM connection which is going to be used, then select the *Done* button.

![](images/300/6.png)
![](images/300/7.png)

### **300a.5: Add a Function Call**
- If you click on mapping sign.

![](images/300/8.png)

- You are going to see _updated-min_ variable that is needed to be added for successful HCM invoking on the right side. The left side represents parametrs that are coming from our Scheduled Integration.

![](images/300/9.png)

- Therefore, in order make a mapping for _updated-min_ variable we need to create a Function Call.

- Close the mapping window.
- Select a plus sign right after Schedule and search for _Function_.

![](images/300/10.png)

- In Create Action insert `GetTimeStamp` name.

![](images/300/11.png)

- After selecting **Create** you are going to see the window as shown bellow.

![](images/300/12.png)

- Before selecting a Function make sure that function exists in Library.

---
---

-  If you have not created a function previously, skipp that section.

- Create a js file and save it in your PC

```
function addTime(ts, z) {
 var d = new Date(ts); 
 d.setMinutes(d.getMinutes() - z);
 var dt = d.toISOString();
  return dt;
}

```

- Novigate to **Library** from Designer Home page.

![](images/300/13.png)


- Then select **Register**.

- In _Register Library_ wizard selct the js file that you just created and insert the required **Name**. Select Create button.

![](images/300/14.png)

- By selecting **Orchestration** as a _Classification Type_ field for _Input_ and _Output_ will be prepopulated.

![](images/300/15.png)

- Save and Close the window.

Now you can access that Function from your Integration Function Action.

![](images/300/16.png)

- Come back to your Scheduled Integration,
---
---

- Select Function button

![](images/300/17.png)

- Pick an appropriate function.

![](images/300/18.png)

- Add Value to your _ts_ Parametr.

![](images/300/19.png)

- Open **Function** folder and find **String Folder**

![](images/300/20.png)

- There you can find _concat_ function. Select, drag, and drop it.

![](images/300/21.png)

- At the end you want your expression looked like that picture bellow.

```
concat( substring-before( /nssrcmpr:schedule/nssrcmpr:startTime, "."),".000Z")
```
![](images/300/22.png)

- Validate and Close the window.

- Add Value to _z_ Parameter 

![](images/300/23.png)

- Assign 10.0 as an Expression. 

![](images/300/24.png)

- Validate and Close the window.

- Validate your Function and Close the window.

![](images/300/25.png)


### **300a.6 Mapping**
- Now it is the right time to make the mapping between 

- Open the **Map** as it shown bellow.

![](images/300/26.png)

- And make a connection by dragging and dropping a point from the fuction call `GetTimeStamp` to _updated-min_ variable from HCM cobbection. That time stamp is needed so the system know how far it should come back and load the new employees.

![](images/300/27.png)

- Validate the mapping and Close the window

## **300a.7: Assign a Variable**
- To assign a variable, click on the flag icon 

<img src="images/500b/image016.png" width="120px">  

- Drag and drop the Assign Icon (found under the Data label) to the integration flow directly under the _getNewHireATOMFeed_ icon

<img src="images/500b/image018.png" width="410px"> --> 
<img src="images/500b/image015.png" width="110px">

- A window will open where you will provide the action with a unique name _AtomFeedResponseCount_ and if you'd like a short description then click the Create button

![](images/500b/image019.png)  

- On the config page set the following values:

![](images/500b/image020.png)

- On the variable column, select counfOfNewHires_assignment_1 from the dropdown
- Set Data Type as string
- The description is optional: _Count the # of new employees returned_
- Select the pencil to edit the Value. 
- Select from _Components_ Functions -> Node-set -> count. And drop it to _Expression_ window.

![](images/500b/image020a.png)

- In the expression builder you will drag and drop it to _Expression_ window the `EmployeeNewHireFeedFeed_Update` array that is coming  from the HCM. Your expression at the end should look like `count($Get-Atom-Feed/nsmpr3:EmployeeNewHireFeedResponse/nsmpr3:EmployeeNewHireFeed_UpdateSet)`

![](images/500b/image020b.png)
![](images/500b/image020c.png)

- Validate the Expression and Close the window.

![](images/500b/image020d.png)

## **THIS LAB IS TO BE CONTINUE AT 300B**