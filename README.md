<style>
  table {
    width: 100%;
    border-collapse: collapse;
}
th, td {
    border: 1px solid;
    padding: 8px;
    text-align: left;
}
th {
  background-color: #a86969;
}
</style>

<div id="inputForm">Tambahkan barang</div>
<hr>
<div id="listBarang">barang</div>
<hr>
<div id="listTas">tas</div>
<div id="listTasItem">item tas</div>

</div>

<script type="text/javascript">

let listBarang = [
  {id:1, nama:'Laptop', harga:13000, banyak:1 },
  {id:2, nama:'Televisi', harga:12500, banyak:2 },
  {id:3, nama:'Radio', harga:30000, banyak:3 },
];

let listTas = [
  {id:1, tanggal:'2024-01-09', total:100, kasir:'Selamat Berbelanja'},
];

let listTasItem = [
  {id:1, idtas:1, idbarang:1, nama:'Laptop', harga:100, banyak:1, jumlah:100},
  {id:2, idtas:1, idbarang:3, nama:'Radio', harga:300, banyak:3, jumlah:900},
];

let inputFields=['id','nama','harga','banyak'];
let out=`<form name='frm'>`;
inputFields.forEach(function(item,index){
  out += `${item}  <input name=""> <br>`;
});
out+=`</form>`;

out += `<button onclick="tambahInput()"> Tambahkan item</button>`;
document.getElementById('inputForm').innerHTML=out;


function init(){

  out=``;
  listBarang.forEach(function(item,index){
    out += `${item.id} | ${item.banyak} | ${item.nama} | ${item.harga} <button onclick="tambah(${item.id})">tambah</button> <br/> `;
  });
  document.getElementById('listBarang').innerHTML=out;


  out=` <button onclick="buatBaru()"> tas keranjang baru </button> <button onclick="cetak()"> cetak</button><br>`;
  listTas.forEach(function(item,index){
    out += 'Tanggal:'+item.tanggal + " <br> ";
    out += 'kasir:'+item.kasir + " <br> ";
    out += 'Kode:'+item.id + " <br> ";
    out += 'Total:'+item.total + " <br> ";
  });
  document.getElementById('listTas').innerHTML=out;

  out=``;
  out=`<table>`;
  out+=`<tr>
  <th>id tas</th>
  <th>id barang</th>
  <th>banyak barang</th>
  <th>nama barang</th>
  <th>harga barang</th>
  <th>jumlah</th>
  <th>aksi</th>
  </tr>`;
  listTasItem.forEach(function(item,index){
    out+=`<tr>
    <td>${item.idtas}</td>
    <td>${item.idbarang}</td>
    <td>${item.banyak}</td>
    <td>${item.nama}</td>
    <td>${item.harga}</td>
    <td>${item.jumlah}</td>
    <td><button onclick="hapus(${index})"> hapus </button></td>
    </tr>`; 
  });

  out += `</table>`;
  document.getElementById('listTasItem').innerHTML=out;
}

init();

function tambah(i){
  let barangDipilih = listBarang.find(e => e.id === i);
  let itemBaru = {
    id: listTasItem.length + 1,
    idtas: listTas.length,
    idbarang: barangDipilih.id,
    nama: barangDipilih.nama,
    harga: barangDipilih.harga,
    banyak: barangDipilih.banyak,
    jumlah: barangDipilih.harga * barangDipilih.banyak,
  };
  listTasItem.push(itemBaru);
  updateTotal();
  init();
}

function tambahInput(){
  listBarang.push({
    id: frm.elements[0].value,
    nama: frm.elements[1].value,
    harga: frm.elements[2].value,
    banyak: frm.elements[3].value,
  });
  init();
}

function buatBaru(){
  let today = new Date().toISOString().slice(0, 10);
  listTas.push({id: listTas.length + 1, tanggal: today, total: 0, kasir: 'Selamat Berbelanja'});
  init();
}

function hapus(i){
  listTasItem.splice(i, 1);
  updateTotal();
  init();
}

function cetak(){
  window.print();
}

function updateTotal() {
  let total = listTasItem.reduce((acc, curr) => acc + curr.jumlah, 0);
  listTas[listTas.length - 1].total = total;
  init();
}

</script>
