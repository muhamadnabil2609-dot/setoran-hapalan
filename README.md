<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Setoran & Poin Santri</title>
<style>
* { box-sizing: border-box; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; margin: 0; padding: 0; }
body { background-color: #f4f6f9; padding: 15px; color: #333; }
.container { max-width: 500px; margin: 0 auto; background: #fff; border-radius: 12px; padding: 20px; box-shadow: 0 4px 10px rgba(0,0,0,0.1); }
h2, h3 { text-align: center; color: #1b5e20; margin-bottom: 15px; }
.card { background: #e8f5e9; border-left: 5px solid #2e7d32; padding: 15px; border-radius: 8px; margin-bottom: 20px; }
.card select, .card input, .card button { width: 100%; padding: 10px; margin: 5px 0 10px 0; border: 1px solid #ccc; border-radius: 6px; }
button { background-color: #2e7d32; color: white; border: none; font-weight: bold; cursor: pointer; }
button:hover { background-color: #1b5e20; }
.santri-list { margin-top: 20px; }
.santri-item { background: #f9f9f9; padding: 12px; border-radius: 6px; margin-bottom: 10px; border: 1px solid #e0e0e0; display: flex; justify-content: space-between; align-items: center; }
.gift-card { background: #fff3e0; border-left: 5px solid #ef6c00; padding: 12px; border-radius: 8px; margin-bottom: 10px; display: flex; justify-content: space-between; align-items: center; }
</style>
</head>
<body>
<div class="container">
<h2>📖 Setoran & Poin Santri</h2>
<!-- Input Setoran Hafalan -->
<div class="card">
<h3>Catat Setoran Hafalan</h3>
<label>Pilih Santri:</label>
<select id="santri-select"></select>
<label>Surah / Juz:</label>
<input type="text" id="surah-input" placeholder="Contoh: Al-Baqarah ayat 1-5">
<label>Kategori:</label>
<select id="kategori-select">
<option value="lancar">Lancar (+10 Poin)</option>
<option value="ulang">Perlu Diulang (+5 Poin)</option>
</select>
<button onclick="tambahSetoran()">Simpan Setoran</button>
</div>
<!-- Daftar Poin Santri -->
<div class="card" style="background: #e3f2fd; border-left-color: #1565c0;">
<h3>Poin Santri</h3>
<div id="santri-list" class="santri-list"></div>
</div>
<!-- Tukar Hadiah -->
<div class="card" style="background: #fff3e0; border-left-color: #ef6c00;">
<h3>🎁 Tukar Hadiah</h3>
<label>Pilih Santri:</label>
<select id="tukar-santri-select"></select>
<label>Pilih Hadiah:</label>
<select id="hadiah-select">
<option value="Kitab Baru|50">Kitab Baru (50 Poin)</option>
<option value="Pensil & Buku Catatan|30">Pensil & Buku Catatan (30 Poin)</option>
<option value="Snack Enak|20">Snack Enak (20 Poin)</option>
</select>
<button onclick="tukarHadiah()" style="background-color: #ef6c00;">Tukar Poin</button>
</div>
</div>
<script>
let santriData = JSON.parse(localStorage.getItem('santriData')) || {
"Ahmad": 40,
"Fatimah": 65,
"Umar": 25
};
function updateSantriView() {
const select1 = document.getElementById('santri-select');
const select2 = document.getElementById('tukar-santri-select');
const listDiv = document.getElementById('santri-list');
select1.innerHTML = '';
select2.innerHTML = '';
listDiv.innerHTML = '';
for (let name in santriData) {
select1.innerHTML += ⁠<option value="${name}">${name}</option>⁠;
select2.innerHTML += ⁠<option value="${name}">${name}</option>⁠;
listDiv.innerHTML += ⁠<div class="santri-item"><span><b>${name}</b></span><span><b>${santriData[name]} Poin</b></span></div>⁠;
}
}
function tambahSetoran() {
const name = document.getElementById('santri-select').value;
const surah = document.getElementById('surah-input').value;
const kategori = document.getElementById('kategori-select').value;
if (!surah) {
alert('Mohon isi surah atau hafalan terlebih dahulu!');
return;
}
let poin = (kategori === 'lancar') ? 10 : 5;
santriData[name] += poin;
localStorage.setItem('santriData', JSON.stringify(santriData));
document.getElementById('surah-input').value = '';
updateSantriView();
alert(⁠Berhasil! ${name} mendapat +${poin} poin.⁠);
}
function tukarHadiah() {
const name = document.getElementById('tukar-santri-select').value;
const hadiahVal = document.getElementById('hadiah-select').value;
const [giftName, costStr] = hadiahVal.split('|');
const cost = parseInt(costStr);
if (santriData[name] >= cost) {
santriData[name] -= cost;
localStorage.setItem('santriData', JSON.stringify(santriData));
alert(⁠Selamat! ${name} berhasil menukarkan ${giftName}. Silakan ambil ke Ustadz.⁠);
updateSantriView();
} else {
alert('Maaf, poin santri belum mencukupi!');
}
}
updateSantriView();
</script>
</body>
</html>
