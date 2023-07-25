## SNOW_Basic_Scripts

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
