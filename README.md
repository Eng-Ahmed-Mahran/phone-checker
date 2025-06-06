This is Google app script

function doGet(e) {
  const phone = e.parameter.phone;
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName("Sheet1");
  const data = sheet.getRange("B2:C").getValues(); // names in B, phones in C

  const normalize = num => {
    if (!num) return "";
    return num.toString().replace(/\D/g, '') // remove non-digits
               .replace(/^20/, '')           // remove country code if present
               .replace(/^0/, '');           // remove leading 0
  };

  const inputPhone = normalize(phone);

  for (let row of data) {
    const name = row[0];
    const sheetPhone = normalize(row[1]);

    if (inputPhone === sheetPhone) {
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
