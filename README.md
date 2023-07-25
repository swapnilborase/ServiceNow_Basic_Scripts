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

