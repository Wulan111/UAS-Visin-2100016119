```LANGKAH PEMBUATAN TABUKASI SILANG
1. Buka Google Sheets dan buatlah Spreadsheet baru atau gunakan yang sudah ada.
2. Import file yang akan dibuat tabulasi silang
3. Buka skrip editor (Script Editor) dengan mengklik "Extensions" di atas, kemudian pilih "Apps Script."
4. Masukkan kode JavaScript ke dalam script editor.
5. Simpan skrip dengan memberikan nama sesuai yang diinginkan
6. Jalankan fungsi typeAndYear() dengan mengklik tombol play (ikon segitiga) di atas atau dengan mengetikkan nama fungsi tersebut di baris perintah dan menekan Enter.
7. Kode akan mengeksekusi dan membuat lembar kerja baru dengan nama 'Kematian Berdasarkan Tahun dan Tipe' yang berisi tabulasi silang dari data kematian berdasarkan tahun dan jenis kematian.
8. Lembar kerja baru akan berisi jenis kematian pada kolom A, tahun pada baris pertama, dan jumlah kematian berdasarkan tipe dan tahun di sel-sel lainnya.
9. Data pada lembar kerja baru akan diurutkan berdasarkan tahun secara menaik.




```KODE JAVASCRIPT
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
