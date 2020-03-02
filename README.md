# Enum-from-SQL-Field-for-EspoCRM
Enum type field that can be populated by custom SQL statements specified in metadata 

For usage instructions follow the example below:

# Example Assumptions:

- We have a "WorkOrder" entity that is linked to a "ServiceTask" entity in a One-to-Many relationship. (One WorkOrder can have many ServiceTasks).

- The "ServiceTask" entity is itself linked to a "ServiceCategory" entity in a Many-to-One relationship. (One ServiceCategory can have many ServiceTasks)

- When we create or update a WorkOrder we want to have a dropdown element (select1) that displays first all possible values for "serviceCategoy" (for example "Plumbing") and depending on this choice, there is another dropdown (select2) that will display all values for "serviceTask" that belong to the "serviceCategory" selected. (For example "Fix leaky faucet").

- Since it would be redundant to store the ServiceCategory for each ServiceTask entity that is linked to a WorkOrder, we define the "serviceCategory" field in the WorkOrder as notStorable so we can manipulate it in the client side but will not create a field in the work_order (WorkOrder) db table.

- The values for "serviceCategory" are stored as a collection of "ServiceCastegory" records, and the values for "serviceTask" are stored as a collection of "ServiceTask" records.

- The values displayed for both "serviceCategory" and "serviceTask" correspond to the "name" field on each entity, so in our example "Plumbing" is the name of the selected "ServiceCategory" record and "Fix leaky faucet" is the name of the selected "ServiceTask" record.

# Example Steps:

1. Dowload the compressed folder (zip) and install this extension, then clear cache and rebuild.

1. Go to Administration >> Entity Manager >> WorkOrder >> Fields and click "Add Field", select "Enum from SQL".

2. Edit file custom/Custom/Espo/Resources/metadata/entityDefs/WorkOrder.json as follows:

```"fields": {

       "entityType": {

         "type": "enum-from-sql",
	 
         "required": true,
	 
         "options": [],
       
         "isCustom": true,
	 
         "selectSQL": "Select name AS value FROM service_category ORDER by name"
	 
       },
   
       "entitySubType": {
	 
          "type": "enum-from-sql",
			
          "required": true,
			
          "options": [],
			
          "isCustom": true,
			
          "selectSQL": "SELECT name AS value FROM service_task WHERE service_task.service_category_id = @@{{serviceCategoryValue}}/@@ ORDER BY name",
			
          "placeholders":{
			
              "serviceCategoryValue":"model.attributes.serviceCategory"
				 
           }
			 
       }  
	 
    }   
```
3. Clear cache and rebuild.
