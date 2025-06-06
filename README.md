this is Google App Script

function doGet(e) {
  const phone = e.parameter.phone;
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName("Sheet1");
  const data = sheet.getRange("B2:C").getValues(); // Get names and phone numbers

  for (let row of data) {
    const name = row[0]; // Column B
    const phoneInSheet = row[1]; // Column C

    if (phoneInSheet == phone) {
      return ContentService.createTextOutput(JSON.stringify({
        found: true,
        name: name
      })).setMimeType(ContentService.MimeType.JSON);
    }
  }

  return ContentService.createTextOutput(JSON.stringify({
    found: false
  })).setMimeType(ContentService.MimeType.JSON);
}
