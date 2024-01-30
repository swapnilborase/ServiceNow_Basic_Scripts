## SNOW_Basic_Scripts

### Script to convert INCIDENT from CLOSE > IN PROGRESS
```
var incidentSysID = "PUT INCIDENT SYSID IN HERE";
var incidentRec = new GlideRecord("incident");
incidentRec.addQuery("sys_id", incidentSysID);
incidentRec.query();
while (incidentRec.next()) {
   incidentRec.state = 2;   //2 = In Progress, 6 = Resolved
   incidentRec.active = true;
   incidentRec.autoSysFields(false);   // Do not update sys_updated_on, sys_updated_by, and sys_mod_count
   incidentRec.setWorkflow(false);       // Do not run any other business rules
   incidentRec.update();
}
```

### Script to Validate Mobile No:
```
 function onChange(control, oldValue, newValue, isLoading) {
    if (isLoading || newValue == '') {
        return;
    }

   var reg = /^[0]\d{10}/;
    if (!reg.test(newValue)) {
        g_form.setValue('u_phone_no', ''); //your field name;
        g_form.showFieldMsg('u_phone_no', 'Phone number should start with 0 and be 11 digits', "error");
    }
}

```
### Script to Validate Email:
```
function onChange(control, oldValue, newValue, isLoading, isTemplate) {
    if (isLoading || newValue === '') {
        return;
    }
 var email = /^[^@]+@[^@]+.[a-z]{2,}$/i;
    if (newValue.search(email) == -1) {
        alert('A valid e-mail address is required eg:-abc@domain.com.');
        g_form.setValue('u_email', '');
    }
}

```
### Script to validate Future Date `Future date can't be selected in date field`
```
function onChange(control, oldValue, newValue, isLoading, isTemplate) {
   if (isLoading || newValue === '') {
      return;
   }
   //Type appropriate comment here, and begin script below
     var currentDateObj = new Date();

            var currentDateStr = formatDate(currentDateObj, g_user_date_format);
            var currentDateNum = getDateFromFormat(currentDateStr, g_user_date_format);
            var startDateNum = getDateFromFormat(newValue, g_user_date_format);

            if (startDateNum > currentDateNum) {
                        alert("Date is in the future");              
                        g_form.clearValue("u_dob");
                        return false;

            }}
```
### Script to Validate Date Field `Past Date should not select`
 - `Apply Script Include`
 	```
  	Name : CheckDate
	Client callable : true
	Script :
		var CheckDate = Class.create();
		CheckDate.prototype = Object.extendsObject(AbstractAjaxProcessor, {
		chkCatDate : function() {
		var start = this.getParameter('sysparm_date');
		var currDay = gs.now();
			if(start < currDay){
				return false;
				}else
			{ return true; } } });
      ```
  - `Apply OnChange Client Script`
    ```
    	function onChange(control, oldValue, newValue, isLoading)
	{
		if(isLoading){ return; }
	if(newValue != ''){
	var ga = new GlideAjax('CheckDate');
	ga.addParam('sysparm_name', 'chkCatDate');
	ga.addParam('sysparm_date',g_form.getValue('date_time'));
	ga.getXML(DatParse);
	}
	function DatParse(response){
	var answer = response.responseXML.documentElement.getAttribute("answer");
	if(answer == 'false'){
		alert("Date cannot exist in past.");
		g_form.setValue('date_time', ''); // Empty the variable.
		}}}		
    ```
 - `Using UI Policy`
    ```
    	- Condition : [Date Field] > [befor] > [Today]
    	- Script [execute if true]:
    		g_from.addErrorMessage('Date should be in future');
    		g_form.clearValue('YOUR DATE FIELD');
    ``` 

### Script to validate UID:
```
function onChange(control, oldValue, newValue, isLoading, isTemplate) {
   if (isLoading || newValue === '') {
      return;
   }

   //Type appropriate comment here, and begin script below
   var pattern = /^[(]?(\d{4})[)]?[-|\s]?(\d{4})[-|\s]?(\d{4})$/;
    var aadhar = g_form.getValue("u_uid");
    if (!pattern.test(aadhar)) {
        g_form.showFieldMsg("u_uid", "please enter correct 12 digit Aadhar No format 4444-5555-6666","error",true);
    }  }

```
### Script to get City, State, Country with considering Pincode:
```
 function onChange(control, oldValue, newValue, isLoading, isTemplate) {
   if (isLoading || newValue === '') {
      return;
   }
   var pincode = g_form.getReference("u_pin", setcity);
    function setcity(pincode) {
        g_form.setValue("u_city", pincode.u_city); //your city field
        g_form.setValue("u_string_14", pincode.u_state); //your state field
      g_form.setValue("u_country", pincode.u_country); //your country field
    } 
}
```
### Script to validate Number length:
```
function onChange(control, oldValue, newValue, isLoading, isTemplate) {
   if (isLoading || newValue === '') {
      return;
   }
	var regexp = /^\d{6}$/;
    if (!regexp.test(newValue)) {
        g_form.setValue('u_pin', ''); // pincode length is validate here // enter your number field
        g_form.showFieldMsg('u_pin', 'Please enter a 6 digit pincode', "error");
    }  
}
```
### Script to get Age using DOB
  - `Apply Script include`
      ```
      var ageScript = Class.create();
      ageScript.prototype = Object.extendsObject(AbstractAjaxProcessor, {
         checkAge: function() {
             var dob = this.getParameter('sysparam_id'); //param from client script
             var today = new GlideDateTime();
             var todayYear = today.getYearLocalTime();
             var bday = new GlideDateTime(dob.toString());
             var bdayYear = bday.getYearLocalTime();
             var age = todayYear - bdayYear;
        return age;
    },
    type: 'ageScript'
     });
      ```
 - `Apply On Change Client Script`
      ```
      function onChange(control, oldValue, newValue, isLoading, isTemplate) {
    if (isLoading || newValue === '') {
        return;
    }
    var ga = new GlideAjax('ageScript');
    ga.addParam('sysparm_name', 'checkAge');
    ga.addParam('sysparam_id', newValue);
    ga.getXML(Process);
   }
   function Process(response) {
    var answer = response.responseXML.documentElement.getAttribute("answer");
    //alert(answer);
    var a = parseInt(answer);
    //alert(a);
    if (a <= 3) {
        g_form.setValue('u_age', '');
        g_form.showFieldMsg('u_age', 'admission avalilable only for above 3 years chidren ', "error");
    } else if (a >= 18) {
        g_form.setValue('u_age', '');
        g_form.showFieldMsg('u_age', 'admission avalilable only for below 18 years students ', "error");
    } else if (a > 3 && a < 18) {
        g_form.setValue("u_age", a);
    }}
      ```

### Script to Re-Trigger the Workflow
   
   ```
   var gr = new GlideRecord("sc_req_item"); //table name
	gr.addQuery("sys_id", "a990135997cc8a10d9d936c71153af40"); //sys_id
	gr.query();
	if (gr.next()) {
		new Workflow().restartWorkflow(gr);
	}

   ```

### Script to Cancel/ Stop Workflow
```
var gr = new GlideRecord("sc_req_item"); //table name
gr.addQuery("number", "RITM001"); //number or sys_id
gr.query();
if (gr.next()) {
	// cancel old running workflow
	var flows = new Workflow().getRunningFlows(gr);
	while(flows.next()) {
		new Workflow().cancelContext(flows);
	}  
}
```
### Culculate Custom variable Value using bunch of question
```
function onSubmit() {
    //Type appropriate comment here, and begin script below
    var value1 = g_form.getValue('u_choice_1');
    var value2 = g_form.getValue('u_choice_2');
    var value3 = g_form.getValue('u_choice_3');
    var value4 = g_form.getValue('u_choice_4');
    var value5 = g_form.getValue('u_choice_5');
    var value6 = g_form.getValue('u_choice_6');

    if (value1 == "")
    {
        value1 = 0;
    }
    if (value2 == "")
    {
        value2 = 0;
    }
    if (value3 == "")
    {
        value3 = 0;
    }
    if (value4 == "")
    {
        value4 = 0;
    }
    if (value5 == "")
    {
        value5 = 0;
    }
    if (value6 == "")
    {
        value6 = 0;
    }

    var number1 = parseInt(value1);
    var number2 = parseInt(value2);
    var number3 = parseInt(value3);
    var number4 = parseInt(value4);
    var number5 = parseInt(value5);
    var number6 = parseInt(value6);

    var total = number1 + number2 + number3 + number4 + number5 + number6;
   
    if (total == 8 || total == 7)
        g_form.setValue('risk', '2'); // high

    else if (total == 6 || total == 5 || total == 4)
        g_form.setValue('risk', '3'); //moderate
    else if (total == 3 || total == 2 || total == 1 || total == 0)
        g_form.setValue('risk', '4'); //low
}
```
### Auto-Populate User / Caller Details

- `Script Include`
  ```
    var contact = '';
        var user = this.getParameter('sysparm_user');
        var gr1 = new GlideRecord('sys_user');
        gr1.addQuery('sys_id', user);
        gr1.query();
        if (gr1.next()) {
            contact = gr1.mobile_phone;
        }
        return contact;
    },
    PopulateEmployee: function() {
        var obj = {};
        var id = this.getParameter('sysparm_user');
        var gr = new GlideRecord('sys_user');
        gr.addQuery('sys_id', id);
        gr.query();
        if (gr.next()) {
            obj['title'] = gr.getValue('title'); //User table fields
            obj['department'] = gr.getValue('department');  //User table fields
            obj['location'] = gr.getValue('location');  //User table fields
            obj['cost_centre'] = gr.getValue('cost_center');  //User table fields
            obj['manager'] = gr.getValue('manager');  //User table fields
            obj['mobile_phone'] = gr.getValue('mobile_phone');  //User table fields
            obj['email'] = gr.getValue('email');  //User table fields
            obj['user_name'] = gr.getValue('user_name');  //User table fields
        }
        return JSON.stringify(obj);
    },
    PopulateOwner: function() {
        var abc = {};
        var app_id = this.getParameter('sysparm_user');
        var app = new GlideRecord('cmdb_ci_appl');
        app.addQuery('sys_id',app_id);
        app.query();
        if (app.next()) {
            abc['owned_by'] = gr.getValue('owned_by');
        }
        return JSON.stringify(abc);
    },
  ```
  - `Client Script - Create both OnChange & OnLoad`
    ```
        //Type appropriate comment here, and begin script below
    var ga = new GlideAjax('Populate_Contact_Number');
    ga.addParam('sysparm_name', 'PopulateEmployee');
    ga.addParam('sysparm_user', g_form.getValue('ea_requested_for')); // give here your variable name
    ga.getXMLAnswer(function(answer) {
        if (answer != 'null') {
            var parser = JSON.parse(answer);
            alert('value of'+ answer);
            g_form.setValue('user_name',parser.user_name); // give here your variable name
            g_form.setValue('ea_requested_for_email', parser.email);  // give here your variable name
            g_form.setValue('manager', parser.manager);  // give here your variable name
            g_form.setValue('title', parser.title);  // give here your variable name
            g_form.setValue('location', parser.location);  // give here your variable name
            g_form.setValue('mobile_phone', parser.mobile_phone);  // give here your variable name
            g_form.setValue('department', parser.department);  // give here your variable name

        }
    });
    ```
