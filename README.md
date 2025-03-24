# Metro Ağı Rota Planlama Projesi
Bu proje bir metro ağının istasyonlarını ve bağlantılarını temsil eden, Python yazılımı dilini kullanan bir uygulamadır. Bir metro ağı içinde başlangıç ve hedef istasyonları arasındaki en hızlı ve en az aktarmalı rotaları bulmak amacıyla geliştirilmiştir. Bu proje içerisinde iki ana algoritma üzerinde durulmuş ve kullanılmmıştır. Bunlar: Breadth-First-Search (BFS) ve A* algoritmalarıdır.

# Kullanılan Teknolojiler ve Kütüphaneler
Python: Projenin yazılım dilidir ve Python'un güçlü veri yapıların ve algoritmalarına dayanan çözümler projenin içinde yer almaktadır.

Collections: Bir çeşit bir sözlüktürdür de denilebilir. Varsayılan değerleri vardır ve bunları döndürür(collections.defaultdict). Bu projede, istasyonları gruplamak ve hatları ilişkilendirmek için kullanıldı.
** collections.deque: Çift uçlu kuyruk anlamına gelmektedir (Double-Ended Queue). Her iki uca da öğelerin eklenmesini ve kaldırılmasına izin veren bir veri yapısıdır. Genellikle kuyruk (FIFO) yapıları için kullanılır. Bu projede BFS algortimasındaki kuyurk işlemleri için kullanılmıştır.

Heapq: Öncelik kuyrupu olarak da bilinmektedir. En küçük veya en büyük öğeye hızlı bir şekilde erişilmesini ve sıralanmasını sağlayan bir veri yapısıdır. Bu modül, bir yığın olarak, eleman ekleme ve çıkarma yöntemleri sunar. Bu projede A* algoritmasının daha verimli çalışabilmesi ve rota hesaplamları yapılmak için kullanılmıştır.

Typing: Kodun okunabilirliğini arttırır ve veri tiplerinin belirtimesini sağlar. Bu projede Dict, Set, Tuple, List, Optional bu doğrultuda kullanılmıştır.

# Algoritmaların Çalışma Mantığı

## BFS Algoritması: BFS, graf veya ağaç yapılarında kullanılır. Bu algoritma, başlangıç düğümünden başlayarak tüm komşu düğümleri ziyaret eder ve ardından bir seviye derinliğe iner. Fakat bir sonraki derinlik seviyesindeki düğümlere geçmeden önce mevvut derinlikteki tüm derinlikleri araştırır. 
**Nasıl çalışır**
Belirlenen başlangıç istasyonunda başlayarak tüm komşu istasyonları sırayla ziyaret eder.
Ziyaret edilmemiş istasyonları bir kuyruğa ekler ve kuyruktan çıkarılan istasyonların komşularını ziyaret eder.
Hedef istasyona verıldığında, o ana kadar izlenen yolu geri döndürür.
Ve kuyruk boşalana kadar bu işlem devam eder.
FIFO (ilk giren ilk çıkar) mantığı ile çalışır ve bu nedenle en az aktarma, dolayısıyla en kısa yolu bulma tarzı projeler ve yapılar için idealdir. Çünkü, özellikle bu proje için, her geçilen durak için sadece bir aktarma yapılır.
Bu projede `en_az_aktarma_bul` fonksiyonunda kullanılmıştır.

## A*(A Yıldız) Algoritması: Bir grafik geçişi ve yol bulma algoritmasıdır. bir kaynak düğümü ve bir hedef düğümü verildiğinde, algoritma kaynak ile hedef arasındaki en kısa yolu bulur. Bu nedenle özellikle yolculukların maliyetini minimum düzeyde tutmak için sıklıkla kullanılır. Bunu başlangıç ​​düğümünden kaynaklanan bir yol ağacı tutarak ve bu yolları hedef düğüme ulaşılana kadar her seferinde bir kenar uzatarak yapar.
**Nasıl çalışır**
Başlangıç istasyonundan hedef istasyona en hızlı rotayı bulmak için kullanılır.
Bir öncelik kuyruğu kullanarak her adımda her istasyonun mevcut maliyetini ve hedefe olan tahmini maliyetini birleştiri ve en düşük toplam maliyeti olan istasyona geçmek hedeflenir. Burada tahmini maliyet, gerçek maliyet ve sezgisel fonksiyonun toplamı olmaktadır.
Sezgisel fonksiyon mevcut istasyondan hedef istasyona olan tahmini uzaklığı hesaplar.
Hedef istasyon bulunana kadar bu işlemler devam eder.
A* algortiması sezgisel fonksiyon ile birlikte çalışır ve sezgisel fonksiyonun iyi tanımlanmış olması A* algoritmasının hızını doğrudan etkiler. Bu sayede en kısa sürede en iyi yolu bulmayı garanti eder.

# Neden Bu Algoritmalar Kullanıldı

BFS algoritması kullanıldı çünkü gerek tüm komşuları tek tek ziyaret etmesi gerek FIFO yapısını kullanması sebebiyle aktarma sayısını minimize edebildi. Özellikle metro hattı gibi gerçek dünya uygulamasında metrodaki her istasyon ve hat, yeni bir aktarma noktası oluşturan birer unsurlardır. Bu istasyonları ve hatları takip ederken FIFO yapısı sayesinde gidilen her istasyonun komuşları da ziyaret edilir ve hedefe giden en kısa yol bu hat üzerinden bulunur. Kısacası ve bu sebeplerden dolayı, BFS algoritması her adımda minimum aktarmayı sağlamak için en uygun çözümü sunar.

A* algoritması kullanıldı çünkü bu algoritma sezgisel fonksiyonun da yardımıyla maliyet hesabı bazlı çalışmaktadır. Gidilen her istasyondan sonra bir kenar oluşturulması ve bu kenar istasyonunda maliyetinin hesaplanması bu algortimayı en hızlı rota bulmak için ideal bir algoritma konumuna taşımaktadır.

# Örnek Kullanım ve Test Sonuçları

# Senaryo 1: Ulus'tan Gar'a
print("\n4. Ulus'tan Gar'a:")
rota = metro.en_az_aktarma_bul("K2", "M4")
if rota:
    print("En az aktarmalı rota:", " -> ".join(i.ad for i in rota))

sonuc = metro.en_hizli_rota_bul("K2", "M4")
if sonuc:
    rota, sure = sonuc
    print(f"En hızlı rota ({sure} dakika):", " -> ".join(i.ad for i in rota))

**Çıktı:** 1. Ulus'tan Gar'a:
En az aktarmalı rota: Ulus -> Kızılay -> Kızılay -> Sıhhiye -> Gar
En hızlı rota (13 dakika): Ulus -> Kızılay -> Kızılay -> Sıhhiye -> Gar

   # Senaryo 2: Keçiören'den Kızılay'a
print("\n6. Keçiören'den Kızılay'a:")
rota = metro.en_az_aktarma_bul("T4", "K1")
if rota:
    print("En az aktarmalı rota:", " -> ".join(i.ad for i in rota))

sonuc = metro.en_hizli_rota_bul("T4", "K1")
if sonuc:
    rota, sure = sonuc
    print(f"En hızlı rota ({sure} dakika):", " -> ".join(i.ad for i in rota))

**Çıktı:** 2. Keçiören'den Kızılay'a:
En az aktarmalı rota: Keçiören -> Gar -> Demetevler -> Demetevler -> Ulus -> Kızılay
En hızlı rota (16 dakika): Keçiören -> Gar -> Gar -> Sıhhiye -> Kızılay -> Kızılay

Bunlar test senaryolarıdır ve örnekler bu şekilde artırılabilir.

# Projeyi Geliştirme Fikirleri

Bu proje her ne kadar kullanışlı bir proje olsa da elbette geliştirilebilir yanları var. En basit örneği ile bu proje daha kullanıcı dostu ve daha anlaşılabilir hale getirilebilir. Örenğin bir arayüz tasarlayarak son kullanıcının aktarmaları ve rota hesaplamalarını daha kolay bir şekilde görmesini ve istediği zaman bu istasyonları değiştimesi sağlanabilir. Kullanıcıların sefer gecikmeleri olduğunda bundan haberdar olması sağlanabilir fakat bunun için dinamik bir yapı kurulmalı ve SQL gibi veri yapıları sistemleri kullanılması gerekecektir.
