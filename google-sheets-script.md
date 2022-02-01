# Google Sheets Script to Keep Your Dyno Awake

[<- Back](./heroku-resources.md)

1. Create a new Google Sheet document
2. Enter 1 at cell B1, which will work as a counter.
3. Go to `Extensions -> Apps Script`
4. Enter the following code:

   ```javascript
    function myFunction() {
        // NOTE: Google Sheets does not support let or const currently as far as I know
        const d = new Date();
        const hours = d.getHours();
        const currentTime = d.toLocaleDateString();
        const counter = SpreadsheetApp.getActiveSheet().getRange(‘B1’).getValues();

        if (hours >= 6 && hours <= 18) {
            // If you want your app to run only on weekdays add the next conditional:
            if (d.getDay() > 5 || d.getDay() === 0) return;
            // Duplicate this next line for all projects
            const response = UrlFetchApp.fetch(“your_heroku_app_address");
            SpreadsheetApp.getActiveSheet().getRange(‘A’ + counter).setValue(`Visited at ${currentTime} ${hours} h`);
            SpreadsheetApp.getActiveSheet().getRange(‘B1’).setValue(Number(counter) + 1);
        }
    }
   ```

5. Save your script by hitting the little disk icon on the upper left part of the page
6. On the navigation menu on the left, click the little clock to get to the `triggers` section.
7. On the bottom right part of the page, click `+ Add Trigger`
8. For the `event source`, select `Time-driven`
9. For the `type of time based trigger`, select `Minutes timer`
10. For the `select minute interval`, select `Every 30 minutes`
11. Click save at the bottom of the screen
12. Profit
