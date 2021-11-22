# simple-contact-form 
This repo contains a simple example of a contact form with JavaScript processing. Feel free to also checkout our [YouTube Video](https://youtu.be/eizAGVwCv0M) where we code this in real time.

## HTML's Form Element
The **form** element represents a section of the document which contains input prompts and controls for the user to fill out. When the user has completed filling out the prompts, they can click the form's 'Submit' button to send the data. These inputs are represented by HTML **input** elements. Every input element associated with a particular form will appear within that form's open and close tags. A webpage can have more than one input form displayed to the user at once. A very basic form example is shown below:
```html
<form id="exampleForm">
    <label for="fullName">Enter your full name:</label>
    <input type="text" name="fullName" />

    <input type="submit" value="Submit">
</form>
```

## Inputs Names and Labels
When creating the markup for input elements, the html element **label** can be optionally used to have the document associate a particular label text to the input element it is linked to. To link an label to an input element, create a **name** attribute for the input element, then set the **for** attribute of the label to that same name.
```html
<label for="fullName">Enter your full name:</label>
<input type="text" name="fullName" />
```
Names are also important for input elements, even if they do not have an associated label. When the form is submitted, the browser will associate the value of that input element to it's name, so the backend system receiving the data, or the JavaScript handling the event (depending upon how we handle the submit event) will have these associated as key value pairs.

## Types of Input Elements
The input types shown below are some of the most commoly used types. there are dozens off different input types not listed here, relating to calendar dates, select drop down boxes, radio buttons, etc.
* **Text:** this is the most common input element. The user can type any string into this input box.
* **Number:** this is an input which will only accept number characters. By default it will also include up and down arrows to increment / decrement by one.
* **Email:** this enables the user to enter an email and will show a tooltip / hover message if the user attempts to submit an invalid email address.
* **Password:** this will obscure the user's input by showing dots or asterisks instead of the user's contents. It also prevents the user from copying values from the field to their clipboard.
* **Submit:** this is a special input type which indicates that it will be the button which submits the form when clicked.
To specify which type of input we want to use, use the **type** attribute:
```html
<!-- A few examples of different input types -->
<input type="text" name="fullName" />
<input type="number" name="age" />
<input type="email" name="userEmail" />
```

**Note:** many types of input boxes will prevent the user from entering certain kinds of characters / data, but this can be circumvented. To ensure that bad data does not get used, the user should validate the inputs on the server when it receives the data. Further discussion of this falls outside the scope of this page but it is worth mentioning nonetheless.

## Textareas with Forms
There are a few special cases where we need to specify elements the user can interact with which are not input elements. One such example is a **textarea** element.  In such cases, we can still include the control within the form object, and it will follow the same rules regarding labels and name attributes. The example below shows a textarea input:
```html
<!-- Message Input -->
<label for="message">Message:</label>
<textarea name="message"></textarea>
```

## Form Submit Event
By default, when the user clicks on the "Submit" button, the page will send the data to the backend and reload the current page. In that scenario, there are two commonly used methods, get and post. The form's **method** attribute will specify which of these two methods to use. The **get** method will append the url with the key value pairs as a query string and reload the page. The **post** method will not append to the url. Both of these methods will send the data to the backend in such a way that the backend can access and process the data.

In this example, we're using JavaScript to process the contents instead. The JavaScript will prevent the normal action of reloading and sending the data to the backend and handle the data in its own way. In order to do this, we will need to add an event listener and prevent the default event:
```javascript
//get the form element reference; our form will have: id="contact-form"
const form = document.getElementById("contact-form");
//add an event listener to the "submit" event
form.addEventListener("submit", event => {
    //call "event.preventDefault()" to ensure the page doesn't reload
    event.preventDefault();
});
```
Once that **preventDefault** method is called on the event, we can do what we want with the data. A simple way of getting the data from the form in an object is to create a **FormData** object, pass in the form object from our **getElementById** call, then get the object as a dictionary (key-value pair) by calling **Object.fromEntries**. For example:
```javascript
//Create a dictionary object of the form data
const formData = Object.fromEntries(
    new FormData(form)
);
```
Once we reach this point, we can do whatever we want with the data. Since the default action of reloading the page will no longer happen, we'll need to handle:
1. Sending the data to whatever backend / service we need to.
2. Getting the response and displaying it to the user: success or failure in most cases.
3. On success, clearing the form entries and possibly removing the form altogether.