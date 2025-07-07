# Settings

## Subject
Subject text of the email.

## From Email
This is the email address that the form uses to send the email.  Usually the SMTP email, but if the DNS zone has been setup with a SPF record it can be different.

## Recipient (Manager Email)
This is the email address that the form email will be sent to.

## ReplyTo Email 
The reply email defaults to a field in the "view" form called "replytoemail".
```
@TextBox(info, "genxml/textbox/replytoemail", "type='email' required")
```
If the "replytoemail" field does NOT exist in the form a field called "email" in the "view" form is used. 
```
@TextBox(info, "genxml/textbox/email", "type='email' required")
```
*If both the "email" and "replytoemail" do not exist the "manageremail" is used.*  

## Email Language
The language that the email will be sent.  Default is en-US.

## Copying to the sender
For reasons of spam penalities on the SMTP server this fuctionality does not exist in the default Email functionality.  The form can be adapted but a required security protocol will need to be imposed.  

## emailsubjectprefix Field
A field called "emailsubjectprefix" can be created on the "view" form, the vaue of this field will prefix the email subject.  
## emailsubjectappendix Field
A field called "emailsubjectappendix" can be created on the "view" form, the vaue of this field will append the email subject.  
