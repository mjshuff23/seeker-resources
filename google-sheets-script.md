# Google Sheets Script to Keep Your Dyno Awake

1. Create a new Google Sheet document, go to Tools -> Script Editor
1. Enter 1 at cell B1, which will work as a counter.
1. Enter the following code:

   ```javascript
    function myFunction() {
        // NOTE: Google Sheets does not support let or const currently as far as I know
        var d = new Date();
        var hours = d.getHours();
        var currentTime = d.toLocaleDateString();
        var counter = SpreadsheetApp.getActiveSheet().getRange(‘B1’).getValues();

        if (hours >= 6 && hours <= 18) {
            // If you want your app to run only on weekdays add the next conditional:
            if (d.getDay() > 5 || d.getDay() === 0) return;
            // Duplicate this next line for all projects
            var response = UrlFetchApp.fetch(“your_heroku_app_address");
            SpreadsheetApp.getActiveSheet().getRange(‘A’ + counter).setValue(‘Visted at ‘ + currentTime + “ “ + hours + “h”);
            SpreadsheetApp.getActiveSheet().getRange(‘B1’).setValue(Number(counter) + 1);
        }
    }
   ```
