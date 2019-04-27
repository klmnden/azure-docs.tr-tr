---
title: Örnek veri kümelerini kullanma
titleSuffix: Azure Machine Learning Studio
description: Machine Learning Studio'da bulunan örnek modellerde kullanılan veri kümelerine açıklamaları. Bu örnek veri kümeleri için denemelerinizi kullanabilirsiniz.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 01/19/2018
ms.openlocfilehash: f86ae4977621927a09d9b83287a00dfa3bc17196
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60736624"
---
# <a name="use-the-sample-datasets-in-azure-machine-learning-studio"></a>Azure Machine Learning Studio'da örnek veri kümelerini kullanma
[top]: #machine-learning-sample-datasets

Birkaç örnek veri kümeleri ve denemeleri, Azure Machine Learning Studio'da yeni bir çalışma alanı oluşturduğunuzda, varsayılan olarak dahil edilir. Bu örnek veri kümeleri birçoğu örnek modellerinde tarafından kullanılan [Azure AI Gallery](https://gallery.azure.ai/). Diğer çeşitli genellikle machine learning'de kullanılan verilere ilişkin örnekler dahil edilir.

Bu veri kümelerini bazıları, Azure Blob Depolama alanında kullanılabilir. Bu veri kümeleri için aşağıdaki tabloda, doğrudan bir bağlantı sağlar. Bu veri kümelerini kullanarak denemelerinizi kullanabilirsiniz [verileri içeri aktarma] [ import-data] modülü.

Bu örnek veri kümeleri kalan kullanılabilir altında çalışma alanınızdaki **kaydedilmiş veri kümeleri**. Machine Learning Studio'da deneme tuvaline solundaki modül paletindeki bulabilirsiniz.
Bu veri kümelerini birini kendi denemenizde, deneme tuvaline sürükleyerek kullanabilirsiniz.

## <a name="datasets"></a>Veri kümeleri

<table>

<tr>
  <th>Veri kümesi adı</th>
  <th>Veri kümesi açıklaması</th>
</tr>

<tr>
  <td>Yetişkin Görselleştirmenizdeki gelir ikili sınıflandırma veri kümesi</td>
  <td>
16 yaş ayarlanmış gelir dizin > 100 ile üzerinden çalışma yetişkinler kullanarak 1994 Görselleştirmenizdeki veritabanı bir alt kümesi.
<p></p>
<b>Kullanım:</b> Bir kişi yılı aşkın 50 bin işletmeyse olup olmadığını tahmin etmek için demografik bilgileri kullanan kişiler sınıflandırın.
<p></p>
<b>İlgili araştırma:</b> Kohavi, R., Becker, B., (1996). UCI makine öğrenimi depo <a href="https://archive.ics.uci.edu/ml"> https://archive.ics.uci.edu/ml </a>. Irvine, CA: California Üniversitesi, okul bilgi ve bilgisayar bilimleri </td>
</tr>

<tr>
  <td>Havaalanı kodları veri kümesi</td>
  <td>
ABD havaalanı kodu.
<p></p>
Bu veri kümesi havaalanı kimlik numarasını ve konumunu Şehir ve eyaleti yanı sıra adı sağlayarak her ABD havaalanı için bir satır içerir.
  </td>
</tr>

<tr>
  <td>Otomobil fiyat verileri (ham)</td>
  <td>
Otomobil tarafından hakkında bilgi, marka ve model, fiyata dahil olmak üzere silindir ve MPG yanı sıra, bir sigorta risk puanı sayısı gibi özellikleri.
<p></p>
Risk puanı, başlangıçta otomatik fiyat ile ilişkilidir. Ardından, gerçek riskli aktüerlerin symboling olarak bilinen bir işlem olarak ayarlanır. Otomatik riskli ve -3 değeri olan büyük olasılıkla güvenlidir + 3 arası değerini gösterir.
<p></p>
<b>Kullanım:</b> Risk puanı, gerileme veya çok değişkenli sınıflandırma özellikleri tarafından tahmin edin. 
<p></p>
<b>İlgili araştırma:</b> Schlimmer, J.C. (1987). UCI makine öğrenimi depo <a href="https://archive.ics.uci.edu/ml"> https://archive.ics.uci.edu/ml </a>. Irvine, CA: California Üniversitesi, okul bilgi ve bilgisayar bilimleri </td>
</tr>

<tr>
  <td>Bisiklet kiralama UCI veri kümesi</td>
  <td>
Washington DC bir bisiklet kiralama ağında tutan büyük Bikeshare şirketin gerçek verileri temel alan UCI bisiklet kiralama veri kümesi.
<p></p>
Veri kümesi için toplam 17,379 satır 2011 ve 2012, her günün her saati için bir satır vardır. Saatlik bisiklet kiralamalar 1 için 977 aralığıdır.

  </td>
</tr>

<tr>
  <td>Bill Gates'le RGB görüntüsü</td>
  <td>
Genel kullanıma açık bir görüntü dosyası için CSV verileri dönüştürülür.
<p></p>
Kod, görüntüyü dönüştürmek için sağlanan <strong>renk K-ortalamaları Kümelemesi kullanarak boyutlandırma</strong> model Ayrıntıları sayfası.
  </td>
</tr>

<tr>
  <td>Tansiyon Bağış veri</td>
  <td>
Tansiyon Transfusion hizmet Center'ın Hsin Chu Şehir, Tayvan tansiyon Bağış veritabanından verilerin bir alt kümesi.
<p></p>
Bağış veri içeren son Bağış itibaren ay) ve sıklığı veya Bağışları, son Bağış daraltılmasından toplam sayısı ve Bağış tansiyon miktarı.
<p></p>
<b>Kullanım:</b> Sınıflandırma Bağış tansiyon Mart 2007 burada 1 hedef süreye ve 0 sırasında bir Bağış gösterir, bağış olmayan Bağış olup olmadığını tahmin etmek için kullanılan hedeftir. 
<p></p>
<b>İlgili araştırma:</b> Yeh, I.C., (2008). UCI makine öğrenimi depo <a href="https://archive.ics.uci.edu/ml"> https://archive.ics.uci.edu/ml </a>. Irvine, CA: California Üniversitesi, okul bilgi ve bilgisayar bilimleri
<p></p>
Yeh, ı-Cheng, Yang, Kıng-Jang gönderdi ve Eğerlendirmesi, etiketi mı, "bilgi bulma Bernoulli dizisi,"Uzman sistemler ile uygulamalar, 2008, kullanılarak RFM modeli <a href="https://dx.doi.org/10.1016/j.eswa.2008.07.018">https://dx.doi.org/10.1016/j.eswa.2008.07.018</a>
  </td>
</tr>

<tr>
  <td>Breast kanser veri</td>
  <td>
Machine learning belgeleri sık görüntülenen Oncology Enstitüsü tarafından sağlanan üç kanser ilişkili veri kümelerinden birini. 300 dokulu örnekleri laboratuar analiz özellikleri ile tanılama bilgileri bir araya getirir.
<p></p>
<b>Kullanım:</b> Kanser türünü sınıflandırmak, 9 özniteliklerine dayalı, bazıları doğrusal ve bazı kategorik. 
<p></p>
<b>İlgili araştırma:</b> Wohlberg, W.H., Street, W.N., & Mangasarian, O.L. (1995). UCI makine öğrenimi depo <a href="https://archive.ics.uci.edu/ml"> https://archive.ics.uci.edu/ml </a>. Irvine, CA: California Üniversitesi, okul bilgi ve bilgisayar bilimleri </td>
</tr>

<tr>
  <td>Breast Kanser özellikleri <td>
Veri kümesi, her 117 özellikleri tarafından açıklanan röntgen görüntüleri 102 K şüpheli bölgeler (aday) için bilgiler içerir. Özellikleri özeldir ve veri kümesi (Siemens sağlık) creators anlamları gösterilmez. 
  </td>
</tr>

<tr>
  <td>Breast Kanser bilgileri</td>
  <td>
Veri kümesi röntgen görüntüsünün şüpheli her bölge için ek bilgiler içerir. Her örnek bilgileri sağlar (örneğin, etiket, hasta kimliği, düzeltme eki görüntünün tamamını göreli koordinatları) hakkında Breast Kanser özellikleri kümesindeki karşılık gelen satır numarası. Her bir Hasta birkaç örnek var. Bir kanser olan hastaların candelete pozitif ve negatif bazılarıdır. Bir kanser dağıtılmayan hastalar için tüm örnekler negatiftir. Veri kümesi 102 K örnekler vardır. Veri noktalarının %0,6 pozitif ağırlıklı, rest negatif olur. Veri kümesi tarafından Siemens sağlık sunulmuştur.
  </td>
</tr>

<tr>
  <td>Paylaşılan CRM Deneyde etiketleri</td>
  <td>
KDD Kupası 2009 müşteri ilişkisi tahmin sınama etiketlerinden (<a href="http://www.sigkdd.org/site/2009/files/orange_small_train_appetency.labels">orange_small_train_appetency.labels</a>).
  </td>
</tr>

<tr>
  <td>Paylaşılan CRM karmaşası etiketleri</td>
  <td>
KDD Kupası 2009 müşteri ilişkisi tahmin sınama etiketlerinden (<a href="http://www.sigkdd.org/site/2009/files/orange_small_train_churn.labels">orange_small_train_churn.labels</a>).
  </td>
</tr>

<tr>
  <td>Paylaşılan CRM veri kümesi</td>
  <td>
Bu veriler KDD Kupası 2009 müşteri ilişkisi tahmin sınama gelir (<a href="http://www.sigkdd.org/site/2009/files/orange_small_train.data.zip">orange_small_train.data.zip</a>).
<p></p>
Fransızca telekomünikasyon şirketinin turuncu 50 bin müşterilerden veri kümesini içerir. Her müşteri 190 biri sayısal 230 anonimleştirilmiş özelliklere sahiptir ve 40 kategorik. Çok seyrek özellikleridir.
  </td>
</tr>

<tr>
  <td>Paylaşılan CRM Upselling etiketleri</td>
  <td>
KDD Kupası 2009 müşteri ilişkisi tahmin sınama etiketlerinden (<a href="http://www.sigkdd.org/site/2009/files/orange_large_train_upselling.labels">orange_large_train_upselling.labels</a>).
  </td>
</tr>

<tr>
  <td>Enerji verimliliğini regresyon veri</td>
  <td>
12 farklı yapı şekline göre benzetimli enerji profil koleksiyonu. Binalar sekiz özellikleri tarafından ayrılır. Bu alan, glazing alan dağıtım ve yönlendirmesini glazing içerir.
<p></p>
<b>Kullanım:</b> İki gerçek değerli yanıtları biri olarak tabanlı enerji verimliliğini derecelendirme tahmin etmek için regresyon veya sınıflandırma kullanın. En yakın tamsayıya yanıt değişkeni yuvarlak çok sınıflı sınıflandırma için olur. 
<p></p>
<b>İlgili araştırma:</b> Xifara, A. & Tsanas, A. (2012). UCI makine öğrenimi depo <a href="https://archive.ics.uci.edu/ml"> https://archive.ics.uci.edu/ml </a>. Irvine, CA: California Üniversitesi, okul bilgi ve bilgisayar bilimleri </td>
</tr>

<tr>
  <td>Uçuş verileri geciktirir</td>
  <td>
Yolcular uçuş zamanında ABD TranStats veri koleksiyondan alınan performans verileri Ulaştırma Bakanlığı (<a href="https://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time">zamanında</a>).
<p></p>
Veri kümesi süre Nisan-Ekim 2013 kapsar. Azure Machine Learning Studio'da karşıya yüklemeden önce veri kümesi şu şekilde işlendi:
<ul>
  <li>Kıta ABD'si sınırları içinde yalnızca 70'ten en yoğun saatinde havaalanları kapsayacak şekilde filtre veri kümesi</li>
  <li>İptal edilen uçuşlar 15 dakikadan fazla Gecikmeli olarak etiketlenmiş</li>
  <li>Yolu saptırabilir uçuşlar filtrelendi</li>
  <li>Seçilen aşağıdaki sütunlar: Yıl, ay, DayofMonth, DayOfWeek, taşıyıcı, OriginAirportID, DestAirportID, CRSDepTime, DepDelay, DepDel15, CRSArrTime, ArrDelay, ArrDel15, iptal edildi</li>
</ul>
</td>
</tr>

<tr>
  <td>Uçuş tarihlere ait zamanında kalkış performansı (ham)</td>
  <td>
Uçak uçuş varış ve Amerika Birleşik Devletleri Ekim 2011'den departures kaydı.
<p></p>
<b>Kullanım:</b> Uçuş gecikme tahmin edin. 
<p></p>
<b>İlgili araştırma:</b> Öğesinden, ABD bölüm nakliye <a href="https://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time"> https://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time </a>.
  </td>
</tr>

<tr>
  <td>Orman yangını verileri</td>
  <td>
Sıcaklık ve nem dizinleri ve Rüzgar hızı gibi hava durumu verileri içerir. Veri alanı orman ateşlenir kayıtlarıyla birlikte Kuzey Doğu Portekiz alınır.
<p></p>
<b>Kullanım:</b> Bu bir zor regresyon, bir yandan orman ateşlenir yazılan alanını tahmin olduğu görevdir. 
<p></p>
<b>İlgili araştırma:</b> Cortez, P., & Morais, A. (2008). UCI makine öğrenimi depo <a href="https://archive.ics.uci.edu/ml"> https://archive.ics.uci.edu/ml </a>. Irvine, CA: California Üniversitesi, okul bilgi ve bilgisayar bilimleri
<p></p>
[Cortez ve Morais, 2007] P. Cortez ve A. Morais. Veri madenciliği yönelik bir yaklaşım tahmin orman rutin verileri kullanarak ateşlenir. In J. Neves, M. F. Santos ve J. Machado EDT, yapay zeka, Study of 13 EPIA 2007 - Portekizce konferansı tutanaklarında yapay zeka, aralık, Guimarães, Portekiz, ss. 512-523, 2007 yeni eğilimler. APPIA, ISBN 13 978-989-95618-0-9. Bulunabilir: <a href="http://www.dsi.uminho.pt/~pcortez/fires.pdf"> http://www.dsi.uminho.pt/~pcortez/fires.pdf </a>.
  </td>
</tr>

<tr>
  <td>Almanca kredi kartı UCI veri kümesi</td>
  <td>
UCI Statlog (Almanca kredi kartı) veri kümesi (<a href="https://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">Statlog + Almanca + kredi + veri</a>), german.data dosyasını kullanarak.
<p></p>
Öznitelik, düşük veya yüksek kredi riskleri olarak bir dizi tarafından açıklanan kişiler, veri kümesini sınıflandırır. Her örnek, bir kişiyi temsil eder. Sayısal ve kategorik, 20 özellikleri ve ikili bir etiket (kredi riski değeri) vardır. Yüksek kredi riski girdilere sahip etiket = 2, düşük kredi riski girdilere sahip etiket = 1. Düşük riskli Örneğin yüksek misclassifying maliyeti, yüksek riskli örneğin düşük misclassifying maliyeti 5 bilgileriyse 1 ' dir.
  </td>
</tr>

<tr>
  <td>IMDB başlık</td>
  <td>
Veri kümesi Twitter tweetleri değerlendirildi filmlerle ilgili bilgiler içerir: IMDB film kimliği, film adı, türe ve üretim yılı. Veri kümesinde 17K filmler bulunur. Veri kümesi "S. kağıt kullanılmaya başlandı Dooms, T. De Pessemier and L. Martens. MovieTweetings: Veri kümesi derecelendirme film Twitter'dan toplanır. Atölyesi kitle kaynak ve İnsan hesaplama öneren sistemleri RecSys 2013'te CrowdRec."
  </td>
</tr>

<tr>
  <td>Iris iki sınıf verileri</td>
  <td>
Desen tanıma belgelerinde bulunacak en iyi bilinen veritabanı belki de budur. 50 örnekleri her üç Iris çeşitleri petal ölçümler içeren nispeten küçük veri kümesi.
<p></p>
<b>Kullanım:</b> Ölçümler iris türünü tahmin edin.  
<p></p>
<b>İlgili araştırma:</b> Fisher, R.A. (1988). UCI makine öğrenimi depo <a href="https://archive.ics.uci.edu/ml"> https://archive.ics.uci.edu/ml </a>. Irvine, CA: California Üniversitesi, okul bilgi ve bilgisayar bilimleri </td>
</tr>

<tr>
  <td>Film Tweetleri</td>
  <td>
Veri kümesi, film Tweetings veri kümesi, genişletilmiş bir sürümüdür. 170 K dereceleri Twitter'da iyi yapılandırılmış tweetleri ayıklanan filmler, bir veri kümesine sahiptir. Her olay bir tweet temsil eder ve bir dizi olan: kullanıcı kimliği, IMDB film kimliği, derecelendirme, zaman damgası, Sık Kullanılanlar'Bu tweet sayısı ve bu tweet'i retweets sayısı. Veri kümesi a Bununla birlikte, S. Dooms, B. Loni ve D. Tikk tarafından öneren sistemleri sınama 2014 için sunulmuştur.
  </td>
</tr>

<tr>
  <td>Çeşitli otomobiller için MPG verileri</td>
  <td>
Bu veri kümesi Carnegie Mellon University StatLib kitaplığı tarafından sağlanan dataset biraz değiştirilmiş bir sürümüdür. Veri kümesi 1983 Amerikan istatistiksel ilişkilendirme Exposition kullanıldı.
<p></p>
Verileri yakıt tüketim galon başına mil içindeki çeşitli otomobiller için listeler. Ayrıca, silindir, altyapısı öteleme, beygir gücü, toplam ağırlığı ve Hızlandırma sayısı gibi bilgileri içerir.
<p></p>
<b>Kullanım:</b> Birden çok değerli üç farklı özniteliklerin ve sürekli beş özniteliğe dayalı yakıt ekonomi tahmin edin. 
<p></p>
<b>İlgili araştırma:</b> StatLib, Carnegie Mellon University, (1993). UCI makine öğrenimi depo <a href="https://archive.ics.uci.edu/ml"> https://archive.ics.uci.edu/ml </a>. Irvine, CA: California Üniversitesi, okul bilgi ve bilgisayar bilimleri </td>
</tr>

<tr>
  <td>İkili sınıflandırma Ailelere Pima ana dilidir veri kümesi</td>
  <td>
Bir veri alt kümelerinin Digestive ve serbest hastalıklara veritabanı ve Ailelere National Institute. Veri kümesi Pima Hindistan miras, kadın hastalara odaklanmak için filtre. Veriler glucose ve insulin düzeyleri yanı sıra, yaşam faktörler gibi tıbbi veriler içerir.
<p></p>
<b>Kullanım:</b> Konu ailelere (ikili sınıflandırma) olup olmadığını tahmin edin. 
<p></p>
<b>İlgili araştırma:</b> Sigillito, V. (1990). UCI makine öğrenimi depo <a href="https://archive.ics.uci.edu/ml"> https://archive.ics.uci.edu/ml"</a>. Irvine, CA: California Üniversitesi, okul bilgi ve bilgisayar bilimleri </td>
</tr>

<tr>
  <td>Restoran müşteri verileri</td>
  <td>
Müşterilerle ilgili demografik bilgileri ve tercihleri de dahil olmak üzere, meta veri kümesi.
<p></p>
<b>Kullanım:</b> Bu veri kümesi eğitmek ve öneren sistem test etmek için diğer iki Restoran veri kümeleri ile birlikte kullanın. 
<p></p>
<b>İlgili araştırma:</b> Bache, K. ve Lichman, M. (2013). UCI makine öğrenimi depo <a href="https://archive.ics.uci.edu/ml"> https://archive.ics.uci.edu/ml </a>. Irvine, CA: California Üniversitesi, okul bilgi ve bilgisayar Bilimine.
  </td>
</tr>

<tr>
  <td>Restoran özelliği verileri</td>
  <td>
Lokantalar ve yemek türü, Yemek stil ve konum gibi bunların özellikleri hakkındaki meta verileri kümesi.
<p></p>
<b>Kullanım:</b> Bu veri kümesi eğitmek ve öneren sistem test etmek için diğer iki Restoran veri kümeleri ile birlikte kullanın. 
<p></p>
<b>İlgili araştırma:</b> Bache, K. ve Lichman, M. (2013). UCI makine öğrenimi depo <a href="https://archive.ics.uci.edu/ml"> https://archive.ics.uci.edu/ml </a>. Irvine, CA: California Üniversitesi, okul bilgi ve bilgisayar Bilimine.
  </td>
</tr>

<tr>
  <td>Restoran derecelendirmeleri</td>
  <td>
Bir ölçekte kullanıcılar tarafından restoranlar için 2 için 0 ile verilen derecelendirmeleri içerir.
<p></p>
<b>Kullanım:</b> Bu veri kümesi eğitmek ve öneren sistem test etmek için diğer iki Restoran veri kümeleri ile birlikte kullanın. 
<p></p>
<b>İlgili araştırma:</b> Bache, K. ve Lichman, M. (2013). UCI makine öğrenimi depo <a href="https://archive.ics.uci.edu/ml"> https://archive.ics.uci.edu/ml </a>. Irvine, CA: California Üniversitesi, okul bilgi ve bilgisayar Bilimine.
  </td>
</tr>

<tr>
  <td>Çelik Annealing çok sınıflı veri kümesi</td>
  <td>
Bu veri kümesi, bir dizi denemeler annealing çelik kayıtları içerir. İçerdiği fiziksel öznitelikleri (genişlik, kalınlığı, sonuç türü (serpantini, sayfa, vb.) çelik türleri.
<p></p>
<b>Kullanım:</b> İki sayısal sınıfı öznitelikleri tahmin etme; sertlik veya gücü. Ayrıca öznitelikler arasında bağıntılar analiz.
<p></p>
Çelik derece izleyin bir kümesi standart SAE ya da diğer kuruluşlar tarafından tanımlanmış. Özel bir 'sınıf' (sınıf değişkeni) arayan ve gerekli değerleri öğrenmek mi istiyorsunuz. 
<p></p>
<b>İlgili araştırma:</b> Sterling'le, d & Buntine, W. (NA). UCI makine öğrenimi depo <a href="https://archive.ics.uci.edu/ml"> https://archive.ics.uci.edu/ml </a>. Irvine, CA: California Üniversitesi, okul bilgi ve bilgisayar bilimleri
<p></p>
Derece çelik için yararlı bir kılavuz burada bulunabilir: <a href="https://otk-sitecore-prod-v2-cdn.azureedge.net/-/media/from-sharepoint/documents/product/outokumpu-steel-grades-properties-global-standards.pdf">https://otk-sitecore-prod-v2-cdn.azureedge.net/-/media/from-sharepoint/documents/product/outokumpu-steel-grades-properties-global-standards.pdf</a>
  </td>
</tr>

<tr>
  <td>Teleskop veri</td>
  <td>
Yüksek enerji gama parçacık kaydı arka plan gürültüsü ile birlikte, hem bir Monte Carlo işlemi kullanarak simulated artışlarını.
<p></p>
Benzetim amacı, taban tabanlı Atmosfer Cherenkov gama telescopes doğruluğunu artırmak için oluştu. Bu, istenen sinyal (Cherenkov Radyasyonu duşlar) ve arka plan gürültüsü (üst Atmosfer içinde cosmic ışınlarındaki başlatan hadronic duşlar) arasındaki farkı anlamak için istatistiksel yöntemler kullanarak gerçekleştirilir.
<p></p>
Önceden işlenmiş hesaplandıktan elongated bir küme ile uzun eksen kamera merkezi yönlendirilmiş oluşturmaktır. Bu üç nokta (Hillas parametreleri olarak da adlandırılır) özelliklerini Ayrımcılığı için kullanılabilir görüntü parametreleri arasındadır.
<p></p>
<b>Kullanım:</b> Hediye görüntüsü sinyali veya arka plan gürültüsü temsil edip etmediğini tahmin edin.
<p></p>
<b>Notlar:</b> Sinyal bir sinyal olayı arka plan olarak sınıflandırma değerinden daha kötü olduğu gibi bir arka plan olay sınıflandırma beri özelliği basit sınıflandırma doğruluğu bu veriler için anlamlı değildir. ROC graf farklı sınıflandırıcılar karşılaştırması için kullanılmalıdır. Sinyal aşağıdaki eşiklerden birine olması gerektiği bir arka plan olayı kabul etme olasılığı: 0,01, 0,02, 0,05, 0,1 veya 0.2.
<p></p>
Ayrıca, arka plan olayları (hadronic duşlar için h) sayısını önemsememiştir olduğunu unutmayın. Gerçek ölçüleri olayları çoğunu h veya gürültü sınıfı temsil eder. 
<p></p>
<b>İlgili araştırma:</b> Bock, R.K. (1995). UCI makine öğrenimi depo <a href="https://archive.ics.uci.edu/ml"> https://archive.ics.uci.edu/ml </a>. Irvine, CA: California Üniversitesi, okul bilgileri </td>
</tr>

<tr>
  <td>Hava durumu veri kümesi</td>
  <td>
Saatlik land bulunduğunuz gözlemler noaa'ya (<a href="https://az754797.vo.msecnd.net/data/WeatherDataset.csv">201310 201304 verilerinin birleştirilmiş</a>).
<p></p>
Hava Durumu verilerini havaalanı hava durumu istasyonlarını Nisan-Ekim 2013 zaman aralığını kapsayan yapılan gözlemler kapsar. Azure Machine Learning Studio'da karşıya yüklemeden önce veri kümesi şu şekilde işlendi:
<ul>
  <li>Hava durumu istasyonu kimlikleri karşılık gelen havaalanı kimlikleri eşlendi</li>
  <li>Hava durumu istasyonu 70'ten en yoğun saatinde havaalanları ile ilişkili olmayan filtrelendi</li>
  <li>Tarih sütununu ayrı yıl, ay ve gün sütuna bölmek</li>
  <li>Seçilen aşağıdaki sütunlar: AirportID, yıl, ay, gün, saat, saat dilimi, SkyCondition, görünürlük, WeatherType, DryBulbFarenheit, DryBulbCelsius, WetBulbFarenheit, WetBulbCelsius, DewPointFarenheit, DewPointCelsius, RelativeHumidity, WindSpeed, WindDirection, ValueForWindCharacter, StationPressure, PressureTendency, PressureChange, SeaLevelPressure, RecordType, HourlyPrecip, Altimeter</li>
</ul>
  </td>
</tr>

<tr>
  <td>Wikipedia SP 500 veri kümesi</td>
  <td>
Veri Wikipedia türetilmiştir (<a href="https://www.wikipedia.org/">https://www.wikipedia.org/</a>) XML verileri olarak depolanan her S & P 500 şirket makalelerinin temel.
<p></p>
Azure Machine Learning Studio'da karşıya yüklemeden önce veri kümesi şu şekilde işlendi:
<ul>
  <li>Her özel bir şirket için metin içeriği ayıklama</li>
  <li>Wiki Biçimlendirmeyi Kaldır</li>
  <li>Alfasayısal olmayan karakterleri Kaldır</li>
  <li>Tüm metni küçük harfe Dönüştür</li>
  <li>Bilinen şirket kategorileri eklendi</li>
</ul>
<p></p>
Kayıt sayısı 500'den az olacak şekilde, bazı şirketler bir makale not bulunamadı.
  </td>
</tr>

<tr>
  <td><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/direct_marketing.csv">direct_marketing.csv</a></td>
  <td>
Veri kümesi, müşteri verileri ve doğrudan bir posta kampanya yanıtını hakkında göstergeleri içerir. Her satır bir müşteri temsil eder. Veri kümesini içeren kullanıcı demografik bilgileri ve davranışını son dokuz özellikleri ve üç sütun etiket (ziyaret edin, dönüştürme ve harcama).  Ziyaret gösteren bir müşteri sonra pazarlama kampanyası ziyaret ikili bir sütundur. Dönüştürme, bir müşterinin bir şey satın gösterir. Harcama geçen süre miktarıdır.  Veri kümesi, Kevin Hillstrom MineThatData e-posta analiz ve veri madenciliği sınaması için tarafından kullanılabilir olarak yapıldı.
  </td>
</tr>

<tr>
  <td><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/lyrl2004_tokens_test.csv">lyrl2004_tokens_test.csv</a></td>
  <td>
Test örneklerde RCV1 V2 Reuters haber veri kümesi özellikleri. 781 K haber makalelerini kimlikleri yanı sıra bir veri kümesine sahiptir (ilk sütun kümesi). Her makalede, stopworded, simgeleştirilmiş ve stemmed. Veri kümesi tarafından David sunulmuştur. D. Lewis.
  </td>
</tr>

<tr>
  <td><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/lyrl2004_tokens_train.csv">lyrl2004_tokens_train.csv</a></td>
  <td>
Eğitim örneklerde RCV1 V2 Reuters haber veri kümesi özellikleri. 23 K haber makalelerini kimlikleri yanı sıra bir veri kümesine sahiptir (ilk sütun kümesi). Her makalede, stopworded, simgeleştirilmiş ve stemmed. Veri kümesi tarafından David sunulmuştur. D. Lewis.
  </td>
</tr>

<tr>
  <td><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/network_intrusion_detection.csv">network_intrusion_detection.csv</a><br></td>
  <td>
Veri kümesi KDD Kupası 1999 bilgi bulma ve veri madenciliği araçları yarışma (<a href="https://kdd.ics.uci.edu/databases/kddcup99/kddcup99.html">kddcup99.html</a>).
<p></p>
Veri kümesi indirildi ve Azure Blob depolamada depolanan (<a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/network_intrusion_detection.csv">network_intrusion_detection.csv</a>) ve hem eğitim hem de test içerir. Eğitim veri kümesi, yaklaşık 126 bin satır ve etiketleri içeren 43 sütunları vardır. Üç sütun etiket bilgilerini bir parçasıdır ve 40 sütun, sayısal ve dize/kategorik özelliklerini içeren, modeli eğitmek için kullanılabilir. Test verilerini yaklaşık 22,5 örnekleri eğitim verileri olduğu gibi aynı 43 sütunlarla test K sahiptir.
  </td>
</tr>

<tr>
  <td><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/rcv1-v2.topics.qrels.csv">rcv1-v2.topics.qrels.csv</a></td>
  <td>
Haber makaleleri RCV1 V2 Reuters haber kümesindeki konu atamaları. Haber makalesinin çeşitli konulara atanabilir. Her satır biçimi "&lt;konu adı&gt; &lt;belge kimliği&gt; 1". Veri kümesi 2,6 milyon konu atamaları içeriyor. Veri kümesi tarafından David sunulmuştur. D. Lewis.
  </td>
</tr>

<tr>
  <td><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/student_performance.txt">student_performance.txt</a></td>
  <td>
Bu veriler KDD Kupası 2010 Öğrenci performansı değerlendirme sınama gelir (<a href="https://www.kdd.org/kdd-cup/view/kdd-cup-2010-student-performance-evaluation">Öğrenci performansı değerlendirme</a>). Kullanılan veri Algebra_2008_2009 eğitim kümesidir (Stamper, J., Niculescu-Mizil, A., Ritter, S. Gordon, G.J. ve Koedinger, K.R. (2010). Cebir miyim 2008-2009. Sınama kümesinden KDD Kupası 2010 eğitim veri madenciliği sınaması. Şimdi Bul <a href="https://pslcdatashop.web.cmu.edu/KDDCup/downloads.jsp">downloads.jsp</a>.
<p></p>
Veri kümesi indirildi ve Azure Blob depolamada depolanan (<a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/student_performance.txt">student_performance.txt</a>) ve sistem özel Ders bir öğrenci günlük dosyalarını içerir. Sağlanan özellikler doğru şekilde sorun çözme önce yapılan Öğrenci sorun kimliği ve kısa açıklamasını, Öğrenci Kimliği, zaman damgası ve sayısını içerir. Özgün veri kümesinden 8,9 milyon kayda sahiptir; Bu veri kümesi için ilk 100 bin satır alt örneklenen olmuştur. Çeşitli türlerde 23 sekmeyle ayrılmış sütun bir veri kümesine sahiptir: kategorik, sayısal ve zaman damgası.
  </td>
</tr>

</table>

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Kickstart denemelerinizin örnekler ile](sample-experiments.md)

<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
