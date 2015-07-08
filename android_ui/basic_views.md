#Basic Android Views

##Button
The button control is a view that is specifically set up to allow click input from the user. While almost every view in the Android SDK accepts click input, buttons are styled to appear raised up which draws the user to them as an obvious visual affordance. Buttons also contain different touch states so that the button changes to appear pressed when the user clicks it. You can get this effect with other views, but you'd have to set it up yourself.

##TextView
TextViews are used to display text to the user. A text view is styled to be flat to allow the text to stand out. With a text view, you can change the text style, color, font, and size by setting a few properties in XML or calling a few Java functions. Text views do not allow for the user to directly alter the text that is shown by this view.

##EditText
EditText is a subclass of TextView, so everything you can do with a TextView, you can do with an EditText. The key difference is that EditTexts are meant to accept user input. Edit text fields can be set to accept several different types of input such as numbers, emails, multiline, or phone numbers. Setting the input type will then change the keyboard that is used to enter the data into the edit field. Setting the keyboard to phone number would switch the keyboard to the dial pad, while setting the input to email would change the keyboard to text with an added key for the @ symbol. Edit text fields should not be used to show static text.

##ImageView
As the name implies, an ImageView is used to display an image to the screen. When setting an image to an ImageView, you can change how the image is cropped, scaled, and centered within the view by setting the scaleType property. You can have the image stay the original size and show up in the center of the view, or you could have the image stretch to fill the container while ignoring the aspect ratio entirely. Try tinkering with the different scale types to find the one that suits your needs the best.

##CheckBox
Checkboxes are used to accept boolean input from the user. Whenever the user needs to input an acceptance/acknowledgment or other similar a/b answer, a check box is likely the right control to use. There is also the Switch control which functions very similarly to a CheckBox but is styled to look like a horizontal switch. A check box is more appropriate when selecting an option in a form, while the switch view is better suited when turning a feature or option off or on.