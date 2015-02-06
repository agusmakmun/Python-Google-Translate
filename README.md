# Python-Google-Translate
Python Google Translate by python.web.id

<img src="http://python.web.id/wp-content/uploads/2015/02/Project-Google-Translate-1024x686.png" title="Project Python Google Translate" alt="Project Python Google Translate"/>

<pre>
/*******************************
Credit By : http://python.web.id
Author    : Agus Makmun (Summon Agus)
Name      : Python Google Translate.
Powered   : Python
Licence   : GNU GENERAL PUBLIC LICENSE Version 2, June 1991.
Tanks to  : Kamyar Ghasemlou ( for adding tab file ).
********************************/
</pre>

<strong>Project Python Google Translate</strong> - Program ini merupakan program Translate Bahasa yang memanfaatkan Layanan Google Translate yang diambil fungsi translatenya kemudian di terapkan pada Python Programming.

<strong>Program ini ada 2 file berupa:</strong>
<b>1.</b> Translate.py ( merupakan programnya )
<b>2.</b> id_country.txt ( merupakan database ID Negara dan Nama Negara )

Untuk File lengkapnya bisa langsung bisa dilihat di github:
<b><a href="https://github.com/agusmakmun/Python-Google-Translate/" title="Python Google Translate" target="_blank">https://github.com/agusmakmun/Python-Google-Translate/</a></b>

<h2>Cara Kerja:</h2>
Untuk cara kerjanya sendiri, jadi kita mengambil hasil result dari Google Translate ketika kita inputkan data tertentu untuk kemudian di translate ke bahasa yang kita inginkan.
Dan module yang kita gunakan adalah module <code>urllib2</code> untuk mengakses ke Internet.

<h3>Pengambilan ID Country dan Name Country</h3>
Disini kami sedikit mengakali bagaimana caranya agar mendapatkan ID dan Nama Negara saja dengan menggunakan Python, karena untuk keperluar tampilan ID dan Nama Negaranya di Program yang kita buat.

<pre>
import urllib2
url = 'https://translate.google.com/m?mui=tl'
agent = {'User-Agent':'Mozilla/5.0'}
cari_hasil = 'div class="small"&gt;'
end_tag = 'div class="small center"&gt;'
request = urllib2.Request(url, headers=agent)
page = urllib2.urlopen(request).read()
result = page[page.find(cari_hasil)+len(cari_hasil):]
res = result.replace('&lt;br&gt;', '').replace('Send us feedback', '').replace('in:&lt;b&gt;Mobile&lt;/b&gt;', '')

operasi = res.split()

count = 0
while True:
    try:
        count += 1
        spl = operasi[count].split()
        y = spl
        for s in y:
            repp = s.replace('&lt;/a&gt;&lt;a', '').replace('href="http://translate.google.com/m?tl=', '').replace('"&gt;', '')            
            kode = repp[:2]
            negara = repp[2:].replace('&lt;/a&gt;&lt;/div&gt;&lt;div', '')
                
            if negara &gt;= 'ass="small':
                break
            print kode, negara
        
    except IndexError:
        break
</pre>

Dari script diatas, data yang kita dapatkan hasilnya adalah seperti ini..
<pre>
af Afrikaans
sq Albanian
ar Arabic
hy Armenian
az Azerbaijani
eu Basque
be Belarusian
bn Bengali
bs Bosnian
bg Bulgarian
ca Catalan
ny Chichewa
zh -CNChinese
zh -TWChinese
hr Croatian
cs Czech
da Danish
.....etc
</pre>

<strong>Masalah yang dihadapi:</strong>
Dari situ kita sudah mendapatkan data dengan <code>ID</code> dan <code>Name Country</code>, namun yang menjadi masalah adalah pada bagian data <code>zh -CNChinese</code> dan <code>zh -TWChinese</code> yang seharusnya <code>ID</code>-nya adalah <code>zh-CN</code> dan <code>zh-TW</code> (tanpa spasi).

Itu sebabnya mengapa kita membuat Database baru yang bernama <code>id_country.txt</code>, sebenarnya tanpa ini kita juga sudah bisa memanfaatkan Python Google Translate ini. Namun untuk mempermudah user dengan adanya tampilan data dari <code>ID</code> dan <code>Name Country</code>, sehingga tidak bingung ketika ingin memasukkan <code>ID</code> pada bahasa yang mereka tidak kenal.

Didalam code>id_country.txt</code> sendiri jika memang datanya seperti pada <code>zh -CNChinese</code> dan <code>zh -TWChinese</code> dengan menggunakan spasi, seperti yang ada pada data diatas, kami juga telah mengutiknya dengan membuatkan program seperti ini:


<pre>
import UserString

f = open('id_country.txt', 'r')
count_me = 0
lines = f.readlines()
for i in lines:
    count_me += 1
    if i[3] == '-':
        depan = i[:2] + i[3:6]
        belakang = UserString.MutableString(i).replace('\n', '')
        del belakang[:6]
        print depan,'\t', belakang            

    else:
        print i[:2], '\t', i[3:].replace('\n', '')
</pre>

Dan akan menghasilkan seperti ini (dengan tab):
<pre>
af 	Afrikaans
sq 	Albanian
ar 	Arabic
hy 	Armenian
az 	Azerbaijani
eu 	Basque
be 	Belarusian
bn 	Bengali
bs 	Bosnian
bg 	Bulgarian
ca 	Catalan
ny 	Chichewa
zh-CN 	Chinese
zh-TW 	Chinese
hr 	Croatian
cs 	Czech
da 	Danish
.....etc
</pre>

Dengan adanya tampilan yang demikian, maka muncul kendala lagi, yaitu akan memboroskan tampilan layar, oleh sebab itu alhamdulillah dengan bantuan dari forum stackoverflow oleh jawaban <em>Kamyar Ghasemlou</em> terjawab dengan menambahkan tab baru dengan panjang dari data yang dibagi 2, dan tampilan yang setelahnya menambah tab baru (hal ini akan lebih meminimalisir tampilan dari layar).

Kemudian kita kembangkan lagi menjadi 3 tab dengan script berikut ini:
<pre>
f = open('id_country.txt','r')
lines = f.readlines()
panjang = len(lines)

for i in range(panjang/3):
    print lines[i].replace('\n', ''),'\t', lines[i + panjang/2].replace('\n', ''),'\t', lines[i + panjang/3].replace('\n', '')
if panjang%2: #if it is odd
    print lines[-1].replace('\n', '')
</pre>

Yang pada hasil endingnya adalah menjadi seperti ini:
<pre>
af Afrikaans 	ko Korean 	ht Haitian
sq Albanian 	lo Lao  	ha Hausa
ar Arabic 	la Latin 	iw Hebrew
hy Armenian 	lv Latvian 	hi Hindi
az Azerbaijani 	lt Lithuanian 	hu Hungarian
eu Basque 	mk Macedonian 	is Icelandic
be Belarusian 	mg Malagasy 	ig Igbo
bn Bengali 	ms Malay 	id Indonesian
bs Bosnian 	ml Malayalam 	ga Irish
bg Bulgarian 	mt Maltese 	it Italian
ca Catalan 	mi Maori 	ja Japanese
ny Chichewa 	mr Marathi 	jw Javanese
zh -CNChinese 	mn Mongolian 	kn Kannada
zh -TWChinese 	my Myanmar 	kk Kazakh
hr Croatian 	ne Nepali 	km Khmer
cs Czech 	no Norwegian 	ko Korean
....etc
</pre>

Naah demikianlah dokumentasi singkat dari proses pembuatan <strong><em>Project Python Google Translate</em></strong>, jika dirasakan orang yang baru mengenal python seperti saya, rasanya sangat puas apabila sukses membuat program sesuai dengan apa yang kita inginkan.. :D 

