# Data Tentang Penyebab-Penyebab Kematian Di Indonesia
https://docs.google.com/spreadsheets/d/1RHxC8572NOE-7_rhTGV4BAjo-1eT7_iHl24BYIGFDq4/edit?hl=id#gid=1718486135

# Data Tentang Jumlah Kematian Di Indonesia Berdasarkan Tahun dan Tipe
https://docs.google.com/spreadsheets/d/1RHxC8572NOE-7_rhTGV4BAjo-1eT7_iHl24BYIGFDq4/edit?hl=id#gid=25699201

# Jelaskan proses pembuatan tabulasi silang tersebut
 langkah-langkah berikut:
 1. Buka Google SpreadSheet pada link berikut: "Data Kematian Di Indonesia" (https://docs.google.com/spreadsheets/d/1RHxC8572NOE-7_rhTGV4BAjo-1eT7_iHl24BYIGFDq4/edit?hl=id#gid=1343501521)

 2. Klik "Extensions" lalu pilih "Apps Script"

 3. Salin kode berikut pada skrip di Apps Script:
```javascript
 function typeAndYear() {
  let ss = SpreadsheetApp.getActive();
  let sheet = ss.getSheetByName('Penyebab Kematian di Indonesia yang Dilaporkan - Clean');
  let numberRows = sheet.getDataRange().getNumRows();
  let numberCols = sheet.getLastColumn();

  let data = sheet.getRange(2, 1, numberRows - 1, numberCols).getValues();

  // Get unique years and types
  let years = [];
  let types = [];
  for (let i = 0; i < data.length; i++) {
    let year = data[i][2];
    let type = data[i][1];
    if (years.indexOf(year) == -1) years.push(year);
    if (types.indexOf(type) == -1) types.push(type);
  }

  // Sort years in ascending order
  years.sort((a, b) => a - b);

  // Create new sheet
  let newSheet = ss.insertSheet('Kematian Berdasarkan Tahun dan Tipe');
  newSheet.getRange('A1').setValue('Type');
  for (let i = 0; i < years.length; i++) {
    newSheet.getRange(1, i + 2).setValue(years[i]);
  }

  // Calculate totals and add to sheet
  for (let i = 0; i < types.length; i++) {
    let type = types[i];
    newSheet.getRange(i + 2, 1).setValue(type);
    for (let j = 0; j < years.length; j++) {
      let year = years[j];
      let totalDeaths = 0;
      for (let k = 0; k < data.length; k++) {
        if (data[k][2] == year && data[k][1] == type) {
          totalDeaths += Number(data[k][4]);
        }
      }
      newSheet.getRange(i + 2, j + 2).setValue(totalDeaths);
    }
  }
}
```
4. simpan dan jalankan kode tersebut

5. jika fungsi berhasil maka pada data spreadsheet akan menambah sheet dengan nama "Kematian Berdasarkan Tahun dan Tipe"