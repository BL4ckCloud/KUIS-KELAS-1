<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Kuis Kelas 1 SD - 100 Soal</title>
<style>
  body { font-family: Arial, sans-serif; background: #f0f8ff; margin: 0; padding: 20px; }
  .container { max-width: 800px; margin: auto; background: #fff; padding: 30px; border-radius: 15px; box-shadow: 0 0 15px rgba(0,0,0,0.1); }
  h1 { text-align: center; color: #2c3e50; }
  .soal { margin-bottom: 20px; padding: 10px; border-bottom: 1px solid #ddd; }
  .mapel { font-size: 12px; color: #888; text-transform: uppercase; }
  .pertanyaan { font-size: 16px; font-weight: bold; margin: 5px 0; }
  .opsi label { display: block; margin: 5px 0; cursor: pointer; padding: 5px; border-radius: 5px; }
  .opsi input { margin-right: 8px; }
  .isian input { padding: 8px; width: 80%; max-width: 300px; font-size: 14px; margin-top: 5px; }
  .btn-submit { display: block; width: 100%; padding: 15px; background: #27ae60; color: white; font-size: 20px; border: none; border-radius: 10px; cursor: pointer; margin: 30px 0; }
  .btn-submit:hover { background: #219a52; }
  .hasil { text-align: center; font-size: 24px; font-weight: bold; color: #2c3e50; margin: 20px 0; }
  .benar-soal { background-color: #d4edda; }
  .salah-soal { background-color: #f8d7da; }
  .option-correct { background-color: #d4edda; font-weight: bold; }
  .option-incorrect { background-color: #f8d7da; text-decoration: line-through; }
  .isian-correct { background-color: #d4edda; }
  .isian-incorrect { background-color: #f8d7da; }
  .kunci { color: #155724; font-style: italic; margin-left: 10px; }
  button:disabled { background: #aaa; cursor: not-allowed; }
  .reset-btn { display: block; width: 200px; margin: 20px auto; padding: 10px; background: #3498db; color: white; border: none; border-radius: 5px; cursor: pointer; }
</style>
</head>
<body>
<div class="container">
  <h1>📚 Kuis Kelas 1 SD</h1>
  <p style="text-align:center;">100 Soal Campuran: Matematika, B. Inggris, B. Indonesia, Pancasila, Agama Islam, Seni Budaya, PJOK</p>
  <div id="quiz"></div>
  <button id="submitBtn" class="btn-submit">Kumpulkan Jawaban</button>
  <div id="hasil" class="hasil"></div>
  <button id="resetBtn" class="reset-btn" style="display:none;">Ulangi Kuis</button>
</div>
<script>
  const soalData = [
    { mapel:"Matematika", tipe:"pg", soal:"2 + 3 = ...", pilihan:["4","5","6","7"], jawaban:"B" },
    { mapel:"Matematika", tipe:"pg", soal:"7 - 2 = ...", pilihan:["5","4","6","3"], jawaban:"A" },
    { mapel:"Matematika", tipe:"pg", soal:"Lambang bilangan 'lima' adalah ...", pilihan:["4","5","6","7"], jawaban:"B" },
    { mapel:"Matematika", tipe:"pg", soal:"10 - 4 = ...", pilihan:["5","6","7","8"], jawaban:"B" },
    { mapel:"Matematika", tipe:"pg", soal:"3 + 6 = ...", pilihan:["8","9","10","7"], jawaban:"B" },
    { mapel:"Matematika", tipe:"pg", soal:"Urutan bilangan setelah 8 adalah ...", pilihan:["7","9","10","6"], jawaban:"B" },
    { mapel:"Matematika", tipe:"pg", soal:"5 + 5 = ...", pilihan:["9","10","11","8"], jawaban:"B" },
    { mapel:"Matematika", tipe:"pg", soal:"9 - 1 = ...", pilihan:["7","8","9","10"], jawaban:"B" },
    { mapel:"Matematika", tipe:"pg", soal:"4 + 2 = ...", pilihan:["5","6","7","8"], jawaban:"B" },
    { mapel:"Matematika", tipe:"pg", soal:"Angka 7 dibaca ...", pilihan:["enam","tujuh","delapan","sembilan"], jawaban:"B" },
    { mapel:"Matematika", tipe:"pg", soal:"Benda yang berbentuk lingkaran adalah ...", pilihan:["bola","buku","penggaris","meja"], jawaban:"A" },
    { mapel:"Matematika", tipe:"pg", soal:"6 + 1 = ...", pilihan:["5","6","7","8"], jawaban:"C" },
    { mapel:"Matematika", tipe:"pg", soal:"8 - 3 = ...", pilihan:["4","5","6","7"], jawaban:"B" },
    { mapel:"Matematika", tipe:"pg", soal:"Lebih banyak: 9 ... 7. Tanda yang tepat adalah ...", pilihan:[">","<","=","≠"], jawaban:"A" },
    { mapel:"Matematika", tipe:"pg", soal:"10 - 5 = ...", pilihan:["4","5","6","7"], jawaban:"B" },
    { mapel:"Matematika", tipe:"pg", soal:"Hasil dari 3 + 4 adalah ...", pilihan:["6","7","8","9"], jawaban:"B" },
    { mapel:"Matematika", tipe:"pg", soal:"2 + 2 + 2 = ...", pilihan:["4","5","6","8"], jawaban:"C" },
    { mapel:"Matematika", tipe:"pg", soal:"Bilangan sebelum 10 adalah ...", pilihan:["8","9","11","7"], jawaban:"B" },
    { mapel:"Matematika", tipe:"pg", soal:"1 + 8 = ...", pilihan:["7","8","9","10"], jawaban:"C" },
    { mapel:"Matematika", tipe:"pg", soal:"5 - 5 = ...", pilihan:["0","1","5","10"], jawaban:"A" },
    { mapel:"Matematika", tipe:"isian", soal:"4 + 3 = ...", jawaban:"7" },
    { mapel:"Matematika", tipe:"isian", soal:"10 - 6 = ...", jawaban:"4" },
    { mapel:"Matematika", tipe:"isian", soal:"Lambang bilangan 'delapan' adalah ...", jawaban:"8" },
    { mapel:"Matematika", tipe:"isian", soal:"5 + 2 = ...", jawaban:"7" },
    { mapel:"Matematika", tipe:"isian", soal:"9 - 5 = ...", jawaban:"4" },
    { mapel:"Bahasa Inggris", tipe:"pg", soal:"What is 'apel' in English?", pilihan:["Banana","Apple","Orange","Grape"], jawaban:"B" },
    { mapel:"Bahasa Inggris", tipe:"pg", soal:"'Cat' in Indonesian is ...", pilihan:["Anjing","Kucing","Kelinci","Burung"], jawaban:"B" },
    { mapel:"Bahasa Inggris", tipe:"pg", soal:"The color of the sky is ...", pilihan:["red","blue","yellow","green"], jawaban:"B" },
    { mapel:"Bahasa Inggris", tipe:"pg", soal:"'Selamat pagi' in English is ...", pilihan:["Good night","Good morning","Good afternoon","Goodbye"], jawaban:"B" },
    { mapel:"Bahasa Inggris", tipe:"pg", soal:"One, two, three, ...", pilihan:["four","five","six","seven"], jawaban:"A" },
    { mapel:"Bahasa Inggris", tipe:"pg", soal:"'Book' means ...", pilihan:["pensil","buku","penghapus","penggaris"], jawaban:"B" },
    { mapel:"Bahasa Inggris", tipe:"pg", soal:"I have two ... (mata)", pilihan:["ears","eyes","nose","mouth"], jawaban:"B" },
    { mapel:"Bahasa Inggris", tipe:"pg", soal:"A: 'How are you?' B: 'I am ...'", pilihan:["five years old","fine","book","red"], jawaban:"B" },
    { mapel:"Bahasa Inggris", tipe:"pg", soal:"The number after nine is ...", pilihan:["eight","ten","eleven","seven"], jawaban:"B" },
    { mapel:"Bahasa Inggris", tipe:"pg", soal:"'Dog' in Indonesian is ...", pilihan:["kucing","anjing","ikan","sapi"], jawaban:"B" },
    { mapel:"Bahasa Inggris", tipe:"pg", soal:"I drink ... (air)", pilihan:["milk","water","juice","tea"], jawaban:"B" },
    { mapel:"Bahasa Inggris", tipe:"pg", soal:"My mother's name is ... (ibu)", pilihan:["mother","father","sister","brother"], jawaban:"A" },
    { mapel:"Bahasa Inggris", tipe:"pg", soal:"A banana is ... (kuning)", pilihan:["red","blue","yellow","green"], jawaban:"C" },
    { mapel:"Bahasa Inggris", tipe:"pg", soal:"'Goodbye' in Indonesian is ...", pilihan:["halo","selamat tinggal","selamat pagi","maaf"], jawaban:"B" },
    { mapel:"Bahasa Inggris", tipe:"pg", soal:"I have one ... (hidung)", pilihan:["mouth","nose","eye","ear"], jawaban:"B" },
    { mapel:"Bahasa Inggris", tipe:"pg", soal:"The color of grass is ...", pilihan:["blue","green","white","black"], jawaban:"B" },
    { mapel:"Bahasa Inggris", tipe:"pg", soal:"'I love you' means ...", pilihan:["aku benci kamu","aku suka kamu","aku sayang kamu","aku rindu"], jawaban:"C" },
    { mapel:"Bahasa Inggris", tipe:"pg", soal:"'Thank you' in Indonesian is ...", pilihan:["maaf","tolong","terima kasih","sama-sama"], jawaban:"C" },
    { mapel:"Bahasa Inggris", tipe:"pg", soal:"This is a ... (pencil)", pilihan:["book","ruler","pencil","bag"], jawaban:"C" },
    { mapel:"Bahasa Inggris", tipe:"pg", soal:"I am ... years old. (7)", pilihan:["five","six","seven","eight"], jawaban:"C" },
    { mapel:"Bahasa Inggris", tipe:"isian", soal:"'Good morning' means ...", jawaban:"selamat pagi" },
    { mapel:"Bahasa Inggris", tipe:"isian", soal:"What is 'kucing' in English?", jawaban:"cat" },
    { mapel:"Bahasa Inggris", tipe:"isian", soal:"The number 10 in English is ...", jawaban:"ten" },
    { mapel:"Bahasa Inggris", tipe:"isian", soal:"'Red' in Indonesian is ...", jawaban:"merah" },
    { mapel:"Bahasa Inggris", tipe:"isian", soal:"'Thank you' artinya ...", jawaban:"terima kasih" },
    { mapel:"Bahasa Indonesia", tipe:"pg", soal:"Huruf vokal terdiri dari ...", pilihan:["a, b, c","a, i, u, e, o","b, c, d","k, l, m"], jawaban:"B" },
    { mapel:"Bahasa Indonesia", tipe:"pg", soal:"'b - u - k - u' jika disusun menjadi ...", pilihan:["buku","buka","kubu","ubuk"], jawaban:"A" },
    { mapel:"Bahasa Indonesia", tipe:"pg", soal:"Tanda titik (.) digunakan untuk ...", pilihan:["bertanya","mengakhiri kalimat","seru","koma"], jawaban:"B" },
    { mapel:"Bahasa Indonesia", tipe:"pg", soal:"Kalimat tanya diakhiri tanda ...", pilihan:["titik (.)","seru (!)","tanya (?)","koma (,)"], jawaban:"C" },
    { mapel:"Bahasa Indonesia", tipe:"pg", soal:"'Aku suka makan apel.' Kalimat tersebut termasuk kalimat ...", pilihan:["tanya","perintah","berita","seru"], jawaban:"C" },
    { mapel:"Bahasa Indonesia", tipe:"pg", soal:"Kata yang terdiri dari dua suku kata adalah ...", pilihan:["meja","buku","pensil","bola"], jawaban:"A" },
    { mapel:"Bahasa Indonesia", tipe:"pg", soal:"'Dina membaca buku.' Siapa yang membaca buku?", pilihan:["Dina","Buku","Membaca","Tidak ada"], jawaban:"A" },
    { mapel:"Bahasa Indonesia", tipe:"pg", soal:"Huruf kapital digunakan pada ...", pilihan:["akhir kalimat","awal kalimat dan nama orang","tengah kalimat","sembarang"], jawaban:"B" },
    { mapel:"Bahasa Indonesia", tipe:"pg", soal:"'Adik menangis karena ...' Kata yang tepat:", pilihan:["gembira","sedih","lucu","kenyang"], jawaban:"B" },
    { mapel:"Bahasa Indonesia", tipe:"pg", soal:"Persamaan kata 'pintar' adalah ...", pilihan:["bodoh","rajin","pandai","malas"], jawaban:"C" },
    { mapel:"Bahasa Indonesia", tipe:"pg", soal:"'Ibu memasak di ...'", pilihan:["kamar mandi","dapur","kelas","taman"], jawaban:"B" },
    { mapel:"Bahasa Indonesia", tipe:"pg", soal:"'Ani suka warna merah. Budi suka warna biru.' Siapa yang suka biru?", pilihan:["Ani","Budi","Ani dan Budi","Tidak ada"], jawaban:"B" },
    { mapel:"Bahasa Indonesia", tipe:"isian", soal:"Lengkapi: 'a - p - e - l' dibaca ...", jawaban:"apel" },
    { mapel:"Bahasa Indonesia", tipe:"isian", soal:"Kata 'bola' terdiri dari ... suku kata.", jawaban:"2" },
    { mapel:"Bahasa Indonesia", tipe:"isian", soal:"Tanda seru (!) untuk kalimat ...", jawaban:"seru" },
    { mapel:"Pendidikan Pancasila", tipe:"pg", soal:"Lambang Pancasila sila pertama adalah ...", pilihan:["rantai","bintang","pohon beringin","padi dan kapas"], jawaban:"B" },
    { mapel:"Pendidikan Pancasila", tipe:"pg", soal:"Bunyi sila kedua Pancasila adalah ...", pilihan:["Ketuhanan Yang Maha Esa","Kemanusiaan yang adil dan beradab","Persatuan Indonesia","Kerakyatan"], jawaban:"B" },
    { mapel:"Pendidikan Pancasila", tipe:"pg", soal:"Simbol sila ketiga adalah ...", pilihan:["bintang","rantai","pohon beringin","kepala banteng"], jawaban:"C" },
    { mapel:"Pendidikan Pancasila", tipe:"pg", soal:"'Persatuan Indonesia' adalah sila ke ...", pilihan:["1","2","3","4"], jawaban:"C" },
    { mapel:"Pendidikan Pancasila", tipe:"pg", soal:"Ketuhanan Yang Maha Esa adalah sila ke ...", pilihan:["1","2","3","4"], jawaban:"A" },
    { mapel:"Pendidikan Pancasila", tipe:"pg", soal:"Bintang lambang sila pertama berwarna ...", pilihan:["kuning emas","putih","hitam","merah"], jawaban:"A" },
    { mapel:"Pendidikan Pancasila", tipe:"pg", soal:"Sikap saling menghormati termasuk sila ke ...", pilihan:["1","2","3","4"], jawaban:"B" },
    { mapel:"Pendidikan Pancasila", tipe:"pg", soal:"Kita harus ... terhadap teman yang berbeda suku.", pilihan:["mengejek","menghormati","memusuhi","menjauhi"], jawaban:"B" },
    { mapel:"Pendidikan Pancasila", tipe:"isian", soal:"Sila pertama Pancasila berbunyi ...", jawaban:"ketuhanan yang maha esa" },
    { mapel:"Pendidikan Pancasila", tipe:"isian", soal:"Lambang sila keempat adalah ...", jawaban:"kepala banteng" },
    { mapel:"Agama Islam", tipe:"pg", soal:"Agama Islam berpedoman pada kitab suci ...", pilihan:["Injil","Taurat","Al-Qur'an","Weda"], jawaban:"C" },
    { mapel:"Agama Islam", tipe:"pg", soal:"Rukun Islam yang pertama adalah ...", pilihan:["shalat","puasa","syahadat","zakat"], jawaban:"C" },
    { mapel:"Agama Islam", tipe:"pg", soal:"'Allah Maha Esa' artinya Allah itu ...", pilihan:["dua","tiga","satu","banyak"], jawaban:"C" },
    { mapel:"Agama Islam", tipe:"pg", soal:"Shalat yang dilakukan pada malam hari bulan Ramadhan adalah ...", pilihan:["tarawih","subuh","dzuhur","ashar"], jawaban:"A" },
    { mapel:"Agama Islam", tipe:"pg", soal:"Nabi yang menerima kitab Al-Qur'an adalah ...", pilihan:["Nabi Musa","Nabi Isa","Nabi Muhammad SAW","Nabi Ibrahim"], jawaban:"C" },
    { mapel:"Agama Islam", tipe:"pg", soal:"Rukun Islam ada ...", pilihan:["3","4","5","6"], jawaban:"C" },
    { mapel:"Agama Islam", tipe:"pg", soal:"Malaikat yang bertugas menyampaikan wahyu adalah ...", pilihan:["Jibril","Mikail","Israfil","Izrail"], jawaban:"A" },
    { mapel:"Agama Islam", tipe:"pg", soal:"Huruf pertama dalam Al-Qur'an adalah ...", pilihan:["Alif","Ba","Ta","Nun"], jawaban:"A" },
    { mapel:"Agama Islam", tipe:"isian", soal:"Agama yang kita anut adalah ...", jawaban:"islam" },
    { mapel:"Agama Islam", tipe:"isian", soal:"Nabi terakhir adalah Nabi ...", jawaban:"muhammad" },
    { mapel:"Seni Budaya", tipe:"pg", soal:"Alat musik pianika dimainkan dengan cara ...", pilihan:["dipukul","ditiup","digesek","dipetik"], jawaban:"B" },
    { mapel:"Seni Budaya", tipe:"pg", soal:"Lagu 'Balonku' adalah ciptaan ...", pilihan:["Ibu Sud","Pak Kasur","AT Mahmud","Papa T Bob"], jawaban:"C" },
    { mapel:"Seni Budaya", tipe:"pg", soal:"Warna merah dicampur kuning menjadi ...", pilihan:["hijau","oranye","ungu","coklat"], jawaban:"B" },
    { mapel:"Seni Budaya", tipe:"pg", soal:"Alat untuk mewarnai gambar adalah ...", pilihan:["pensil","krayon","pulpen","penggaris"], jawaban:"B" },
    { mapel:"Seni Budaya", tipe:"pg", soal:"Tari Saman berasal dari ...", pilihan:["Jawa","Bali","Aceh","Sumatera Barat"], jawaban:"C" },
    { mapel:"Seni Budaya", tipe:"pg", soal:"Patung dari tanah liat disebut ...", pilihan:["gerabah","anyaman","batik","ukiran"], jawaban:"A" },
    { mapel:"Seni Budaya", tipe:"isian", soal:"Lagu 'Naik Delman' diciptakan oleh ...", jawaban:"ibu sud" },
    { mapel:"PJOK", tipe:"pg", soal:"Sebelum olahraga kita harus ...", pilihan:["tidur","makan berat","pemanasan","minum es"], jawaban:"C" },
    { mapel:"PJOK", tipe:"pg", soal:"Gerakan melompat melatih kekuatan otot ...", pilihan:["tangan","kaki","perut","leher"], jawaban:"B" },
    { mapel:"PJOK", tipe:"pg", soal:"Senam yang diiringi musik disebut ...", pilihan:["senam lantai","senam irama","senam alat","senam bebas"], jawaban:"B" },
    { mapel:"PJOK", tipe:"pg", soal:"Permainan kasti termasuk olahraga ...", pilihan:["bola kecil","bola besar","air","atletik"], jawaban:"A" },
    { mapel:"PJOK", tipe:"pg", soal:"Saat berenang, kita menggunakan pakaian ...", pilihan:["seragam","renang","pesta","tidur"], jawaban:"B" },
    { mapel:"PJOK", tipe:"pg", soal:"Sikap awal berdiri tegak, tangan di pinggang, lalu menekuk lutut adalah gerakan ...", pilihan:["push up","sit up","squat jump","lari"], jawaban:"C" },
    { mapel:"PJOK", tipe:"isian", soal:"Lari termasuk olahraga ...", jawaban:"atletik" },
    { mapel:"PJOK", tipe:"isian", soal:"Permainan sepak bola menggunakan ...", jawaban:"bola" }
  ];

  const quizDiv = document.getElementById("quiz");
  function renderQuiz() {
    let html = '';
    soalData.forEach((s, i) => {
      html += `<div class="soal" id="soal-${i}">`;
      html += `<div class="mapel">${s.mapel} | Soal ${i+1}</div>`;
      html += `<div class="pertanyaan">${s.soal}</div>`;
      if (s.tipe === "pg") {
        html += `<div class="opsi">`;
        s.pilihan.forEach((opsi, j) => {
          const huruf = String.fromCharCode(65 + j);
          html += `<label><input type="radio" name="q${i}" value="${huruf}"> ${huruf}. ${opsi}</label>`;
        });
        html += `</div>`;
      } else {
        html += `<div class="isian"><input type="text" name="q${i}" placeholder="Jawaban..."></div>`;
      }
      html += `</div>`;
    });
    quizDiv.innerHTML = html;
  }
  renderQuiz();

  document.getElementById("submitBtn").addEventListener("click", function() {
    let skor = 0;
    const allInputs = document.querySelectorAll("#quiz input");
    allInputs.forEach(inp => inp.disabled = true);
    document.getElementById("submitBtn").disabled = true;
    document.getElementById("resetBtn").style.display = "block";

    soalData.forEach((s, i) => {
      const soalDiv = document.getElementById("soal-" + i);
      let userAnswer = null;
      if (s.tipe === "pg") {
        const selected = document.querySelector(`input[name="q${i}"]:checked`);
        if (selected) userAnswer = selected.value;
        const labels = soalDiv.querySelectorAll(".opsi label");
        labels.forEach((label, idx) => {
          const optValue = String.fromCharCode(65 + idx);
          if (optValue === s.jawaban) label.classList.add("option-correct");
          if (selected && optValue === userAnswer && optValue !== s.jawaban) label.classList.add("option-incorrect");
        });
        if (userAnswer === s.jawaban) { skor++; soalDiv.classList.add("benar-soal"); }
        else { soalDiv.classList.add("salah-soal"); }
      } else {
        const input = soalDiv.querySelector("input[type='text']");
        if (input) {
          userAnswer = input.value.trim().toLowerCase();
          if (userAnswer === s.jawaban.toLowerCase()) {
            skor++; input.classList.add("isian-correct"); soalDiv.classList.add("benar-soal");
          } else {
            input.classList.add("isian-incorrect"); soalDiv.classList.add("salah-soal");
            const kunciSpan = document.createElement("span");
            kunciSpan.className = "kunci";
            kunciSpan.textContent = "Jawaban: " + s.jawaban;
            input.parentNode.appendChild(kunciSpan);
          }
        } else { soalDiv.classList.add("salah-soal"); }
      }
    });

    document.getElementById("hasil").innerHTML = `Skor Kamu: ${skor} / 100 (${Math.round(skor/100*100)}%)`;
    window.scrollTo({ top: 0, behavior: 'smooth' });
  });

  document.getElementById("resetBtn").addEventListener("click", () => location.reload());
</script>
</body>
</html>