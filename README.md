# Create CRM Tasks and Standalone Functions



## How it Works
Standalones are great to use when you want to reuse code and call it from different functions. Keeps code a lot cleaner.


## Set Up Standalone Function

Create the standalone function that you want to call

<img src="Image 9-19-23 at 4.50 AM.jpeg" width="600">

Add the parameter and make note of the function name with underscores. Add the task code (leads snippet below)

<img src="Image 9-19-23 at 5.32 AM.jpeg" width="600">

```
lead_record = zoho.crm.getRecordById("Leads",lead_id);
reminder_date_time = zoho.currenttime.addDay(1).toTime();
//info reminderTime;
lead_map = Map();
lead_map.put("Subject","Lead Follow up");
lead_map.put("$se_module","Leads");
lead_map.put("What_Id",input.lead_id);
lead_map.put("Owner",lead_record.get("Owner").get("id"));
lead_map.put("Due_Date",zoho.currenttime.toDate().addDay(2));
lead_map.put("Remind_At",{"ALARM":"FREQ=NONE;ACTION=EMAIL;TRIGGER=DATE-TIME:" + reminder_date_time.toString("yyyy-MM-dd'T'HH:mm:ss'+05:30'")});
lead_map.put("Status","Not Started");
lead_map.put("Send_Notification_Email",true);
createTask = zoho.crm.createRecord("Tasks",lead_map);
info lead_map;
info createTask;


return createTask;

```
If you need information from the standalone function, add it to the return statement at the end. For example, send the createTask response over to the function that will call the standalone.

Now create another function where you will call the standalone from. I created a simple update to the lead record if the phone number was updated. Call the standalone by using the "standalone.function_name(standalone_pameter)" syntax. If you just want to call it, you don't need to store it in a variable, although storing it in a variable can provide feedback if something goes wrong in the standalone.

```
update_lead_status = zoho.crm.updateRecord("Leads",lead_id,{"Lead_Status":"Contact in Future"});

response_from_standalone = standalone.create_lead_task(lead_id);

info response_from_standalone;

```



```

```
