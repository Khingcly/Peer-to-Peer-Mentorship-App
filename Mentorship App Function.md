## Let's have a look at some complex functions embeded inside this application

The SignIn button in the Mentor screen and Mentee screen has a SignIn button integrated with the fuctions below. The aim of this function is to complete successful login of user.

### PowerApps Function for Mentee SignIn Button
``` 
If(
    !IsBlank(textboxx) And !IsBlank(TextBox2_1.Value),
    If(
        CountRows(
            Filter(
                'Mentee Tables',
                textboxx.Value in 'Mentee Tables'.Username And
                TextBox2_1.Value in 'Mentee Tables'.Password
            )
        ) > 0,
        Navigate(MenteeScreen, ScreenTransition.None),
        Notify("Invalid username or password. Please try again.", NotificationType.Error)
```     
### PowerApps Function for Mentor SignIn Button 

```
If(
    !IsBlank(textboxx_1) And !IsBlank(TextBox2_2.Value),
    If(
        CountRows(
            Filter(
                'Mentor Tables',
                textboxx_1.Value in 'Mentor Tables'.Display_Name And
                TextBox2_2.Value in 'Mentor Tables'.Password
            )
        ) > 0,
        Navigate(MentorScreen, ScreenTransition.None),
        Notify("Invalid username or password. Please try again.", NotificationType.Error)
    ),
```
## PowerApps Function to Submit New Mentor Data If New Mentor Has Filled All Necessary Columns
This function is for the SignUp button 
```
If(
    And(
        !IsBlank(DataCardValue12.Value),
        !IsBlank(DataCardValue13.Value),
        !IsBlank(DataCardValue14.Value),
        !IsBlank(DataCardValue16.SelectedItems),
        !IsBlank(DataCardValue15.SelectedItems),
        !IsBlank(DataCardValue17.SelectedItems)

    ),
    SubmitForm(Form1_1), // replace Form1 with the name of your form
    Notify("Please fill in all required fields.", ErrorSeverity.Warning) // notify the user to fill in all required fields
); Navigate(MentorScreen)
```
## PowerApps Function for Editing Mentee Profile from Data Source
This function is to allow user edit their data from the data source. Please note that "menteeusername" is a global variable, I also used the same function for mentor edit profile screen.
```
Patch('Mentee Tables', 
First(Filter('Mentee Tables', Username = menteeusername.Value)),
Form2.Updates)
```
## PowerApps Function to Display Welcome Message To Mentor
This function is embeded inside a text label that shows a welcome message to mentee upon successful login. The value 'mentorusername' is a global variable while for mentee it is ' menteeusername' (still a global variabe).
```
"Welcome, Dear " & Mentorusername.Value 
```
## Approval flow function and onselect property of button send data to MentorshiRequestTable for mentorhip request
```
Sendapprovalandfollowupviaemail.Run(Body1, Subtitle2_1);
Navigate(MenteeScreen);

Patch('Mentorship Requests', Defaults('Mentorship Requests'), {MenteeName: Title2_1.Text, MentorName: Subtitle2.Text,'Area of Interest': DataCardValue33.Selected.Value})
```
