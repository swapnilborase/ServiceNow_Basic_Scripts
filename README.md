# SNOW_Basic_Scripts

 ### Script to make one fied value attach to another field value. eg: Make Temporary address similer to Permanent Address
  - On Change Client Script
  - ```function onChange(control, oldValue, newValue, isLoading, isTemplate) {
      if (isLoading || newValue === '') {
         return;
     }
     var address = g_form.getValue("u_temp_address");
     g_form.setValue("u_per_address",address);   
 }```
   
