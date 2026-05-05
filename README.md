<!DOCTYPE html>
<html lang="id">    
<head>
</head>

<style>
body {
    font-family: Arial;
    margin: 20px;
    background: #c04d0a;
}
</style>
Pada bagian ini, <!DOCTYPE html> digunakan untuk mendeklarasikan bahwa dokumen menggunakan HTML5. Tag <html lang="id"> menunjukkan bahwa bahasa halaman adalah Bahasa Indonesia. Bagian <head> merupakan tempat metadata, walaupun di sini kosong. Selanjutnya <style> digunakan untuk mengatur tampilan, dimana body diberi font Arial, margin 20px, dan background warna oranye agar tampilan tidak polos.
form {
    background: #fff;
    padding: 20px;
    border-radius: 10px;
    margin-bottom: 20px;
}

input, textarea, select {
    width: 100%;
    padding: 8px;
    margin: 5px 0 15px;
}

button {
    padding: 10px;
    margin-right: 5px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

.submit {
    background: green;
    color: white;
}

.reset {
    background: red;
    color: white;
}
Bagian CSS ini mengatur tampilan form dan komponennya. Form dibuat seperti kotak putih dengan padding dan sudut melengkung agar rapi. Input, textarea, dan select dibuat full lebar supaya sejajar. Tombol diatur agar nyaman diklik, dengan warna hijau untuk submit (menyimpan data) dan merah untuk reset (menghapus isi form).
table {
    width: 100%;
    border-collapse: collapse;
    background: #fff;
}

th, td {
    border: 1px solid #ccc;
    padding: 10px;
    text-align: center;
}

button:hover {
    opacity: 0.8;
}
Kode ini mengatur tampilan tabel. Tabel dibuat memenuhi lebar layar dan memiliki border yang rapi. Setiap sel diberi padding agar tidak sempit. Efek hover pada tombol membuat tombol sedikit transparan saat disentuh, sehingga terasa interaktif.

.jk-table {
    width: 100%;
    margin-bottom: 15px;
    border-collapse: collapse;
}

.jk-table td {
    border: 1px solid #ccc;
    padding: 10px;
    text-align: center;
    background: #f9f9f9;
    cursor: pointer;
}

.jk-table td:hover {
    background: #e6f0ff;
}

input[type="radio"]{
    width:auto;
}

.aksi-btn{
    background:none;
    border:none;
    font-size:20px;
    cursor:pointer;
}
</style>
Bagian ini khusus untuk tampilan jenis kelamin dan tombol aksi. .jk-table digunakan agar pilihan Pria dan Wanita tampil dalam bentuk tabel sehingga lebih rapi. Saat disentuh, warnanya berubah agar terlihat bisa diklik. Radio button tetap digunakan sebagai logika pilihan. .aksi-btn digunakan untuk tombol edit dan hapus agar tampil sebagai ikon tanpa background.
<body>

<h2>Form Data Mahasiswa</h2>

<form id="formMahasiswa">
Masuk ke bagian <body>, ini adalah isi utama halaman. Judul ditampilkan agar pengguna tahu fungsi halaman. Form dengan id formMahasiswa digunakan untuk menampung input data mahasiswa.
<label>NIM</label>
<input type="text" id="nim" required>

<label>Nama</label>
<input type="text" id="nama" required>

<label>Alamat</label>
<textarea id="alamat"></textarea>
Bagian ini adalah input data. NIM dan Nama wajib diisi (required), sedangkan Alamat menggunakan textarea agar bisa menampung teks panjang.
<label>Jenis Kelamin</label>

<table class="jk-table">
<tr>
<td>
<label>
<input type="radio" name="jk" value="Pria"> Pria
</label>
</td>
<td>
<label>
<input type="radio" name="jk" value="Wanita"> Wanita
</label>
</td>
</tr>
</table>
Jenis kelamin dibuat dalam bentuk tabel agar pilihan terlihat sejajar dan rapi. Radio button memastikan hanya satu pilihan yang bisa dipilih, yaitu Pria atau Wanita.
<label>Tanggal Lahir</label>

<select id="tanggal"></select>

<select id="bulan">
<option value="01">Januari</option>
<option value="02">Februari</option>
</select>

<select id="tahun"></select>
Bagian ini digunakan untuk memilih tanggal lahir. Dropdown tanggal dan tahun akan diisi otomatis oleh JavaScript, sedangkan bulan sudah ditentukan secara manual.

<label>Password</label>
<input type="password" id="password">

<button type="submit" class="submit">Submit</button>
<button type="reset" class="reset">Reset</button>

</form>
Input password digunakan untuk menyimpan kata sandi. Tombol submit berfungsi untuk menyimpan data, sedangkan reset untuk mengosongkan form.
<h2>Data Mahasiswa</h2>

<table>
<thead>
<tr>
<th>NIM</th>
<th>Nama</th>
<th>Alamat</th>
<th>JK</th>
<th>TTL</th>
<th>Password</th>
<th>Aksi</th>
</tr>
</thead>

<tbody id="tableBody"></tbody>

</table>
Bagian ini adalah tabel untuk menampilkan data mahasiswa yang sudah diinput. <tbody> akan diisi secara dinamis menggunakan JavaScript.
<script>

const tanggalSelect = document.getElementById("tanggal");
for(let i=1;i<=31;i++){
    tanggalSelect.innerHTML += `<option value="${i}">${i}</option>`;
}
Script ini mengisi dropdown tanggal dari 1 sampai 31 menggunakan perulangan, sehingga tidak perlu ditulis manual.
const tahunSelect = document.getElementById("tahun");
for(let i=1980;i<=2025;i++){
    tahunSelect.innerHTML += `<option value="${i}">${i}</option>`;
}
Kode ini mengisi dropdown tahun secara otomatis dari 1980 sampai 2025.
const form = document.getElementById("formMahasiswa");
const tableBody = document.getElementById("tableBody");

let dataMahasiswa = JSON.parse(localStorage.getItem("dataMahasiswa")) || [];
let editIndex = -1;

tampilkanData();
Bagian ini mengambil elemen form dan tabel. Data mahasiswa diambil dari localStorage agar tidak hilang saat refresh. Variabel editIndex digunakan untuk menandai data yang sedang diedit.
form.addEventListener("submit", function(e){
    e.preventDefault();
Saat tombol submit ditekan, halaman tidak akan reload karena preventDefault(). Ini penting agar data bisa diproses dengan JavaScript.
let nim = document.getElementById("nim").value;
let nama = document.getElementById("nama").value;
let alamat = document.getElementById("alamat").value;
let jk = document.querySelector('input[name="jk"]:checked')?.value || "";
Kode ini mengambil semua nilai input dari form, termasuk jenis kelamin yang dipilih.
let tanggal = document.getElementById("tanggal").value;
let bulan = document.getElementById("bulan").value;
let tahun = document.getElementById("tahun").value;

let ttl = tanggal + "-" + bulan + "-" + tahun;
Bagian ini menggabungkan tanggal, bulan, dan tahun menjadi satu string tanggal lahir.
let data = { nim, nama, alamat, jk, ttl, password };
Semua data dimasukkan ke dalam satu objek agar mudah disimpan.
if(editIndex == -1){
    dataMahasiswa.push(data);
} else {
    dataMahasiswa[editIndex] = data;
    editIndex = -1;
}
Jika tidak sedang edit, data akan ditambahkan. Jika sedang edit, data lama akan diperbarui.
localStorage.setItem("dataMahasiswa", JSON.stringify(dataMahasiswa));
tampilkanData();
form.reset();
});
Data disimpan ke localStorage agar tidak hilang, lalu tabel diperbarui dan form dikosongkan.
function tampilkanData(){
    tableBody.innerHTML = "";
Fungsi ini digunakan untuk menampilkan ulang data ke tabel.
dataMahasiswa.forEach((item,index)=>{
    tableBody.innerHTML += `
        <tr>
            <td>${item.nim}</td>
            <td>${item.nama}</td>
            <td>${item.alamat}</td>
            <td>${item.jk}</td>
            <td>${item.ttl}</td>
            <td>${item.password}</td>
            <td>
                <button onclick="editData(${index})">✏️</button>
                <button onclick="hapusData(${index})">🗑️</button>
            </td>
        </tr>
    `;
});
}
Perulangan ini menampilkan setiap data mahasiswa ke dalam tabel lengkap dengan tombol edit dan hapus.
function hapusData(index){
    dataMahasiswa.splice(index,1);
    localStorage.setItem("dataMahasiswa", JSON.stringify(dataMahasiswa));
    tampilkanData();
}
Fungsi ini digunakan untuk menghapus data berdasarkan index, lalu menyimpan ulang dan memperbarui tabel.
function editData(index){
    let item = dataMahasiswa[index];
Fungsi ini digunakan untuk mengambil data yang akan diedit.
document.getElementById("nim").value = item.nim;
document.getElementById("nama").value = item.nama;
document.getElementById("alamat").value = item.alamat;
document.getElementById("password").value = item.password;
Data dimasukkan kembali ke dalam form agar bisa diubah.
document.querySelector(`input[name="jk"][value="${item.jk}"]`).checked = true;
Radio button jenis kelamin otomatis dipilih sesuai data.
let ttl = item.ttl.split("-");
document.getElementById("tanggal").value = ttl[0];
document.getElementById("bulan").value = ttl[1];
document.getElementById("tahun").value = ttl[2];

editIndex = index;
}
</script>
Tanggal lahir dipecah kembali agar bisa dimasukkan ke dropdown. Kemudian editIndex disimpan agar saat submit data akan diupdate, bukan ditambah.

