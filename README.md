# Enum-from-SQL-Field-for-EspoCRM
Enum type field that can be populated by custom SQL statements specified in metadata 

For usage instructions follow the example below:

# Example Assumptions:

- We have a "WorkOrder" entity that has a not storable field "serviceCategory" and a regular (storable) field called "serviceTask".

- We also have two other entities: "ServiceCategory" and "ServiceTask". "ServiceCategory" is linked to "ServiceTask" in a One-to-Many relationship.

- "WorkOrder" is linked to "ServiceTask" in a One-to-Many relationship.

- When we create or update a WorkOrder we want to have a dropdown element (select1) that displays all values for "serviceCategoy" (for example "Plumbing") and depending on this choice, another dropdown (select2) will display all values for "serviceTask" that belong to the selected "servicecategory" (for example "Fix leaky faucet").

- The values for "serviceCategory" are stored as a collection of "ServiceCastegory" records, and the values for "serviceTask" are stored as a collection of "ServiceTask" records.

- The values displayed for both "serviceCategory" and "serviceTask" correspond to the "name" field on each entity, so in our example "Plumbing" is the name of the selected "ServiceCategory" record and "Fix leaky faucet" is the name of the selected "ServiceTask" record.

# Example Steps:

1. Dowload and install this extension, then clear cache and rebuild.

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
