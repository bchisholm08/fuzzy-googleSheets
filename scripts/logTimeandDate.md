Google sheets macros for my own time keeping program. 

Old notes: 
changed from row 2 to row 6 for whole function on 10/7/2024, inserted pay stats in top rows, froze top 4 rows
Only persistent issue seems to be that the number formatting for the total hours errors out of you edit any part of the times that the function generates 

```javascript

function logTimeAndDate() {
    var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
    
    sheet.insertRowBefore(6); 
    
    // curr date and time in CST
    var now = new Date();
    var timeZone = "CST";
    
    // Curr date, MM/DD/YY format
    var dateCell = sheet.getRange("A6");
    dateCell.setValue(Utilities.formatDate(now, timeZone, "MM/dd/yy"));
    
    // Curr time, HHMM format
    var timeCell = sheet.getRange("B6");
    timeCell.setValue(Utilities.formatDate(now, timeZone, "HHmm"));
}

function onEdit(e) { // see notes above 
    var sheet = e.source.getActiveSheet();
    var editedCell = e.range.getA1Notation();
    
    if (editedCell == "C6") { 
        var startTime = sheet.getRange("B6").getValue();
        var endTime = sheet.getRange("C6").getValue();
        
        if (startTime && endTime) {
            // need to change times to an object for accurate math 
            var timeZone = "CST";
            var startTimeDate = Utilities.parseDate(startTime, timeZone, "HHmm");
            var endTimeDate = Utilities.parseDate(endTime, timeZone, "HHmm");
            // get time diff 
            var diffMilliseconds = endTimeDate.getTime() - startTimeDate.getTime();
            var diffHours = diffMilliseconds / (1000 * 60 * 60);
            // calculated difference 
            sheet.getRange("D6").setValue(diffHours);
        }
    }
}
```
