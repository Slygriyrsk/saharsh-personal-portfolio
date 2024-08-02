# 📚 My Portfolio Website


Welcome to my portfolio website! This project showcases my skills, activities, and educational background, and demonstrates my ability to create a fully responsive and interactive website using HTML, CSS, and JavaScript. Below, you'll find a comprehensive overview of the features and technologies used.

## 📜 Overview


This portfolio website serves as a digital resume and showcases my work and achievements. It is built with a focus on modern web development practices, ensuring a smooth and engaging user experience across all devices.

## 🛠️ Technologies Used


-   **HTML**: Structure and content of the website.
-   **CSS**: Styling and responsive design.
-   **JavaScript**: Interactivity, text animations, and dynamic features.
-   **Boxicons**: Icons for skills, social media, and other important elements.
-   **Google Sheets & AppScript**: Handling user contact form submissions.

## 🚀 Features

###  1\.  Responsive Design 📱💻

- The website adapts seamlessly to different screen sizes, including mobile phones, tablets, and desktops.
###  2\. Text Animations ✨

-  Dynamic text animations are implemented using JavaScript to enhance visual appeal.
###  3\.  Menu Navigation 🍔

-   A collapsible menu bar is provided for navigation on mobile devices, ensuring easy access to different sections.
###  4\.  Tab Navigation 🗂️

-  Smooth transitions between sections including Skills, Activities, and Education using tab links.
###  5\.  Boxicons 🔲

-  Utilized Boxicons for representing skills, social media icons, and other key features.
###  6\.  Contact Form Integration ✉️

-   User contact form submissions are handled via Google Sheets with AppScript integration for streamlined communication.

## 🖥️ Installation and Setup

To view or modify the portfolio website locally, follow these steps:

###  1\.  Clone the Repository:

    git clone https://github.com/Slygriyrsk/personal-portfolio.git


###  2\.  Navigate to the Project Directory:

    
    cd personal-portfolio
    

###  3\.  Open the `index.html` File:

-   Open `index.html` in your preferred web browser to view the website.

## ⚙️ Configuration


###  1\. Google Sheets Contact Form:

-  Make sure to set up Google Sheets and AppScript for handling contact form submissions. Follow the Google Apps Script documentation for setup instructions.

###  2\.  Boxicons Integration:

-  Ensure that the Boxicons CDN link is included in your HTML file to properly display icons.

### 📋 Submit a Form to Google Sheets | Demo

Learn how to create an HTML form that stores the submitted form data in Google Sheets using JavaScript (ES6), Google Apps Script, Fetch, and FormData.

###  1\.  Create a New Google Sheet

-   First, go to [Google Sheets](https://docs.google.com/spreadsheets) and `Start a new spreadsheet` with the `Blank` template.
-   Rename it `Email Subscribers`. Or whatever, it doesn't matter.
-   Put the following headers into the first row:

###  2\.  Create a Google Apps Script

- Navigate to **Tools** > **Script Editor** to open a new tab.
- Rename the project to "Submit Form to Google Sheets" and save it.
- Delete the existing `myFunction() {}` block in the Code.gs tab and replace it with the following script:

        
        var sheetName = 'Sheet1'
        var scriptProp = PropertiesService. getScriptProperties()

        function intialSetup () {
          var activeSpreadsheet = SpreadsheetApp.getActiveSpreadsheet()
          scriptProp.setProperty('key', activeSpreadsheet.getId())
        }

        function doPost (e) {
          var lock = LockService.getScriptLock()
          lock.tryLock(10000)

          try {
            var doc = SpreadsheetApp.openById(scriptProp.getProperty('key'))
            var sheet = doc.getSheetByName(sheetName)

            var headers = sheet.getRange(1, 1, 1, sheet.getLastColumn()).getValues()[0]
            var nextRow = sheet.getLastRow() + 1

            var newRow = headers.map(function(header) {
              return header === 'timestamp' ? new Date() : e.parameter[header]
            })

            sheet.getRange(nextRow, 1, 1, newRow.length).setValues([newRow])

            return ContentService
              .createTextOutput(JSON.stringify({ 'result': 'success', 'row': nextRow }))
              .setMimeType(ContentService.MimeType.JSON)
          }

          catch (e) {
            return ContentService
              .createTextOutput(JSON.stringify({ 'result': 'error', 'error': e }))
              .setMimeType(ContentService.MimeType.JSON)
          }

          finally {
            lock.releaseLock()
          }
        }

 -  For a detailed explanation of the script, check out the `form-script-commented.js` file in the repository.
### 3\. Run the setup function


[](https://github.com/jamiewilson/form-to-google-sheets#3-run-the-setup-function)

-   Next, go to `Run > Run Function > initialSetup` to run this function.
-   In the `Authorization Required` dialog, click on `Review Permissions`.
-   Sign in or pick the Google account associated with this projects.
-   You should see a dialog that says `Hi {Your Name}`, `Submit Form to Google Sheets wants to`...
-   Click `Allow`

### 4\. Add a new project trigger


[](https://github.com/jamiewilson/form-to-google-sheets#4-add-a-new-project-trigger)

-   Click on `Edit > Current project's triggers`.
-   In the dialog click `No triggers set up. Click here to add one now.`
-   In the dropdowns select `doPost`
-   Set the events fields to `From spreadsheet` and `On form submit`
-   Then click `Save`

### 5\. Publish the project as a web app


[](https://github.com/jamiewilson/form-to-google-sheets#5-publish-the-project-as-a-web-app)

-   Click on `Publish > Deploy as web app...`.
-   Set `Project Version` to `New` and put `initial version` in the input field below.
-   Leave `Execute the app as:` set to `Me(your@address.com)`.
-   For `Who has access to the app:` select `Anyone, even anonymous`.
-   Click `Deploy`.
-   In the popup, copy the `Current web app URL` from the dialog.
-   And click `OK`.

> IMPORTANT! If you have a custom domain with Gmail, you *might* need to click `OK`, refresh the page, and then go to `Publish > Deploy as web app...` again to get the proper web app URL. It should look something like `https://script.google.com/a/yourdomain.com/macros/s/XXXX...`.




[](https://github.com/jamiewilson/form-to-google-sheets#6-input-your-web-app-url)

Open the file named `index.html`. On line 12 replace `<SCRIPT URL>` with your script url:
    **Important:** If you have a custom domain with Gmail, you may need to re-deploy to get the correct web app URL.

###  6\. Input Your Web App URL

-   Open `index.html` and replace `<SCRIPT URL>` on line 12 with your web app URL:

                
        <form name="submit-to-google-sheet">
          <input name="email" type="email" placeholder="Email" required>
          <button type="submit">Send</button>
        </form>

        <script> const scriptURL = '<SCRIPT URL>'
          const form = document.forms['submit-to-google-sheet']

          form.addEventListener('submit', e => {
            e.preventDefault()
            fetch(scriptURL, { method: 'POST', body: new FormData(form)})
              .then(response => console.log('Success!', response))
              .catch(error => console.error('Error!', error.message))
          }) </script>
          

## 🌟 Contributions


Contributions are welcome! If you'd like to contribute to the development of this portfolio, please fork the repository and submit a pull request with your changes.

## 📧 Contact


For any questions or feedback, feel free to reach out to me via the contact form on the website or directly via email.