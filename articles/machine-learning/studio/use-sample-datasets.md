---
title: Machine Learning Studio'daki örnek veri kümelerini kullanan | Microsoft Docs
description: Machine Learning Studio'da bulunan örnek modellerindeki kullanılan veri kümelerindeki açıklamaları. Bu örnek veri kümeleri denemelerinizi için kullanabilirsiniz.
services: machine-learning
documentationcenter: ''
author: heatherbshapiro
ms.author: hshapiro
manager: hjerez
editor: cgronlun
ms.assetid: 03a0b844-e8a7-4896-996f-d3c7a0db7a50
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/19/2018
ms.openlocfilehash: 63f079f6c8e584ff3a89503104060daf056763ed
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34836309"
---
# <a name="use-the-sample-datasets-in-azure-machine-learning-studio"></a>Azure Machine Learning Studio'da örnek veri kümelerini kullanma
[top]: #machine-learning-sample-datasets

Örnek veri kümelerini ve denemeleri sayısı, Azure Machine Learning ile yeni bir çalışma alanı oluşturduğunuzda, varsayılan olarak dahil edilir. Bu örnek veri kümelerini birçoğu örnek modellerinde tarafından kullanılan [Azure AI galeri](http://gallery.cortanaintelligence.com/). Diğer çeşitli genellikle machine learning'de kullanılan veri türlerine örnek olarak dahil edilir.

Bu veri kümeleri bazıları, Azure Blob Depolama alanında kullanılabilir. Bu veri kümeleri için aşağıdaki tabloda doğrudan bir bağlantı sağlar. Kullanarak bu veri kümeleri, denemeler kullanabileceğiniz [veri içeri aktarma] [ import-data] modülü.

Bu örnek veri kümelerini geri kalanı kullanılabilir altında çalışma alanınızdaki **kaydedilen veri kümeleri**. Machine Learning Studio'da deneme tuvaline solundaki modül paletindeki bulabilirsiniz.
Bu veri kümeleri birini kendi deney deneme tuvale sürükleyerek kullanabilirsiniz.


[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

<table>

<tr>
  <th>Veri kümesi adı</th>
  <th>Veri kümesi tanımı</th>
</tr>

<tr>
  <td>Yetişkin Census gelir ikili sınıflandırma veri kümesi</td>
  <td>
Bir alt kümesini 16 yaş > 100 ayarlandı geliri dizini ile üzerinden çalışma yetişkinler kullanarak 1994 Census veritabanı.
<p></p>
<b>Kullanım:</b> bir kişi üzerinde 50 K yılda kazandığı olup olmadığını tahmin etmek için demografisine kullanan kişilerin sınıflandırmak.
<p></p>
<b>İlgili araştırma:</b> Kohavi, r, Becker, b, (1996). Depo öğrenme UCI makine <a href="http://archive.ics.uci.edu/ml"> http://archive.ics.uci.edu/ml </a>. Irvine, CA: California Üniversitesi, okul daha fazla bilgi ve bilgisayar bilimi </td>
</tr>

<tr>
  <td>Havaalanı kodları veri kümesi</td>
  <td>
ABD havaalanı kodları.
<p></p>
Bu veri kümesi havaalanı kimliği sayısı ve konumu Şehir ve bölge ile birlikte ad sağlayan her ABD havaalanı için bir satır içerir.
  </td>
</tr>

<tr>
  <td>Otomobil fiyat verileri (ham)</td>
  <td>
Tarafından otomobiller hakkında bilgi marka ve model, fiyat de dahil olmak üzere, silindir ve MPG yanı sıra, bir sigorta risk puanı sayısı gibi özellikler.
<p></p>
Risk puanını başlangıçta otomatik fiyat ile ilişkilidir. Ardından, gerçek risk actuaries symboling olarak bilinen bir işlemle için ayarlanır. + 3 arası değerini otomatik risklidir ve -3 değeri olan büyük olasılıkla güvenlidir gösterir.
<p></p>
<b>Kullanım:</b> risk puanı regresyon veya multivariate sınıflandırmasını kullanan özellikler tarafından tahmin. 
<p></p>
<b>İlgili araştırma:</b> Schlimmer, J.C. (1987). Depo öğrenme UCI makine <a href="http://archive.ics.uci.edu/ml"> http://archive.ics.uci.edu/ml </a>. Irvine, CA: California Üniversitesi, okul daha fazla bilgi ve bilgisayar bilimi </td>
</tr>

<tr>
  <td>Bisiklet kiralama UCI veri kümesi</td>
  <td>
Gerçek veri Washington DC bir bisiklet kiralama ağda tutar sermaye Bikeshare şirketten dayalı UCI bisiklet kiralama veri kümesi.
<p></p>
Veri kümesi 17,379 satırların toplam 2011 ve 2012, her gün için her saat için bir satır vardır. Saatlik bisiklet kiralamaları 1'den için 977 aralığıdır.

  </td>
</tr>

<tr>
  <td>Fatura geçitleri RGB görüntüsü</td>
  <td>
Genel kullanıma açık görüntü dosyası için CSV verileri dönüştürülür.
<p></p>
Görüntüyü dönüştürme kodunu sağlanan <strong>renk K-ortalamaları kümeleme kullanarak boyutlandırma</strong> modeli ayrıntı sayfası.
  </td>
</tr>

<tr>
  <td>Kan Bağış veri</td>
  <td>
Bir veri alt kümesini kan Transfusion hizmeti, merkezi Hsin Chu Şehir, Tayvan kan Bağış veritabanından.
<p></p>
Bağış verileri içeren ay boyunca son Bağış itibaren) ve sıklığı veya bağışlanır kan miktarını ve Bağışlar, son Bağış bu yana geçen süre toplam sayısı.
<p></p>
<b>Kullanım:</b> Bağış kan Mart 2007, burada 1 gösteren bir Bağış hedef dönemi ve 0 sırasında Bağış olmayan bağışlanır olup olmadığını sınıflandırma tahmin etmek için belirtilir. 
<p></p>
<b>İlgili araştırma:</b> Yeh, I.C., (2008). Depo öğrenme UCI makine <a href="http://archive.ics.uci.edu/ml"> http://archive.ics.uci.edu/ml </a>. Irvine, CA: California Üniversitesi, okul daha fazla bilgi ve bilgisayar bilimi
<p></p>
Yeh, t-Cheng, Yang, Jang Kol-gönderdi ve toplantı, Tao msiexec, "bilgi bulmayı Bernoulli dizisi,"Uzman sistemler uygulamalarla 2008 kullanılarak RFM modeli <a href="http://dx.doi.org/10.1016/j.eswa.2008.07.018">http://dx.doi.org/10.1016/j.eswa.2008.07.018</a>
  </td>
</tr>

<tr>
  <td>Breast kanseri verileri</td>
  <td>
Machine learning belgeleri sık görüntülenen Oncology Enstitüsü'nün tarafından sağlanan üç kanseri ilişkili veri kümeleri biri. Laboratuar analiz özelliklerinden tanılama bilgilerini yaklaşık 300 dokulu örnekleri birleştirir.
<p></p>
<b>Kullanım:</b> kanseri türünü sınıflandırmak ve 9 özniteliklerini temel alarak, bazıları doğrusal kategorik bazılarıdır. 
<p></p>
<b>İlgili araştırma:</b> Wohlberg, W.H., Sokak, W.N. ve Mangasarian, O.L. (1995). Depo öğrenme UCI makine <a href="http://archive.ics.uci.edu/ml"> http://archive.ics.uci.edu/ml </a>. Irvine, CA: California Üniversitesi, okul daha fazla bilgi ve bilgisayar bilimi </td>
</tr>

<tr>
  <td>Breast Kanseri özellikleri <td>
Veri kümesi her 117 özellikleri tarafından açıklanan röntgen görüntüleri 102 K şüpheli bölgeler (adayları) için bilgiler içerir. Özellikleri özeldir ve anlamları dataset oluşturucuları (Siemens sağlık) gösterilmez. 
  </td>
</tr>

<tr>
  <td>Breast Kanseri bilgisi</td>
  <td>
Veri kümesi röntgen görüntüsünün şüpheli her bölge için ek bilgiler içerir. Her örneğin bilgileri sağlar (örneğin, etiket, hasta kimliği, düzeltme eki görüntünün tamamını göre koordinatları) Breast Kanseri özellikleri kümesindeki karşılık gelen satır numarasını hakkında. Her hasta örnek sayısına sahip. Bir kanseri sahip hastalar için bazı örnekler pozitif ve negatif bazılarıdır. Bir kanseri yok hastalar tüm örnekler negatiftir. Veri kümesi 102 K örnekler vardır. Veri kümesi %0,6 noktalarının pozitif ağırlıklı, rest negatif. Veri kümesi Siemens sağlık tarafından kullanılabilir hale getirildi.
  </td>
</tr>

<tr>
  <td>Paylaşılan CRM Appetency etiketleri</td>
  <td>
KDD fincanı 2009 müşteri ilişkileri tahmin sınama etiketlerden (<a href="http://www.sigkdd.org/site/2009/files/orange_small_train_appetency.labels">orange_small_train_appetency.labels</a>).
  </td>
</tr>

<tr>
  <td>Paylaşılan CRM karmaşası etiketleri</td>
  <td>
KDD fincanı 2009 müşteri ilişkileri tahmin sınama etiketlerden (<a href="http://www.sigkdd.org/site/2009/files/orange_small_train_churn.labels">orange_small_train_churn.labels</a>).
  </td>
</tr>

<tr>
  <td>Paylaşılan CRM veri kümesi</td>
  <td>
Bu veriler KDD fincanı 2009 müşteri ilişkileri tahmin challenge gelir (<a href="http://www.sigkdd.org/kdd-cup-2009-customer-relationship-prediction - orange_small_train.data.zip">orange_small_train.data.zip</a>).
<p></p>
Veri kümesi Fransızca telekomünikasyon şirket turuncu 50 K müşterilerden içerir. Her bir müşteri 190 de sayısal 230 anonim özelliklere sahiptir ve 40 kategorik. Çok seyrek özellikleridir.
  </td>
</tr>

<tr>
  <td>Paylaşılan CRM Upselling etiketleri</td>
  <td>
KDD fincanı 2009 müşteri ilişkileri tahmin sınama etiketlerden (<a href="http://www.sigkdd.org/site/2009/files/orange_large_train_upselling.labels">orange_large_train_upselling.labels</a>).
  </td>
</tr>

<tr>
  <td>Enerji verimliliğine regresyon veri</td>
  <td>
Benzetimli enerji profilleri, 12 farklı yapı şekillerine göre koleksiyonu. Bina sekiz özellikleri tarafından ayrılır. Bu alan, glazing alan dağıtım ve yönlendirme glazing içerir.
<p></p>
<b>Kullanım:</b> regresyon veya sınıflandırma iki gerçek değerli yanıtları biri olarak tabanlı derecelendirme enerji verimliliğine tahmin etmek için kullanın. Çok sınıfı için en yakın tamsayıya yanıt değişkeni round sınıflandırmasıdır. 
<p></p>
<b>İlgili araştırma:</b> Xifara, a & Tsanas, A. (2012). Depo öğrenme UCI makine <a href="http://archive.ics.uci.edu/ml"> http://archive.ics.uci.edu/ml </a>. Irvine, CA: California Üniversitesi, okul daha fazla bilgi ve bilgisayar bilimi </td>
</tr>

<tr>
  <td>Uçuş veri geciktirir</td>
  <td>
Yolcu uçuş zamanında ABD TranStats veri koleksiyondan alınan performans verileri Departman, taşıma (<a href="http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time">zamanında</a>).
<p></p>
Veri kümesi süre Nisan-Ekim 2013 kapsar. Azure Machine Learning Studio karşıya yüklemeden önce veri kümesi gibi işlenmiş:
<ul>
  <li>Veri kümesi yalnızca 70 yoğun saatinde havaalanları ABD continental de kapsayacak şekilde filtre uygulanan</li>
  <li>İptal edilen uçuşlar 15 dakikadan tarafından Gecikmeli olarak etiketlenmiş</li>
  <li>Yolu saptırabilir uçuşlar filtre</li>
  <li>Aşağıdaki sütun seçildi: yıl, ay, DayofMonth, DayOfWeek, taşıyıcı, OriginAirportID, DestAirportID, CRSDepTime, DepDelay, DepDel15, CRSArrTime, ArrDelay, ArrDel15, iptal edildi</li>
</ul>
</td>
</tr>

<tr>
  <td>Uçuş zamanında performans (ham)</td>
  <td>
Uçak uçuş gelişleri ve departures Ekim 2011 Birleşik Devletler içinde kaydı.
<p></p>
<b>Kullanım:</b> uçuş gecikmeler tahmin etmek. 
<p></p>
<b>İlgili araştırma:</b> , ABD bölüm taşıma gelen <a href="http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time"> http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time </a>.
  </td>
</tr>

<tr>
  <td>Orman yangını verileri</td>
  <td>
Sıcaklık ve nem dizinlerini ve Rüzgar hızı gibi hava durumu verilerini içerir. Veri orman ateşlenir kayıtlarıyla birleştirilmiş Kuzey Doğu Portekiz alanı alınır.
<p></p>
<b>Kullanım:</b> AIM orman ateşlenir yazılan alanını tahmin olduğu zor regresyon görev budur. 
<p></p>
<b>İlgili araştırma:</b> Cortez, P. & Morais, A. (2008). Depo öğrenme UCI makine <a href="http://archive.ics.uci.edu/ml"> http://archive.ics.uci.edu/ml </a>. Irvine, CA: California Üniversitesi, okul daha fazla bilgi ve bilgisayar bilimi
<p></p>
[Cortez ve Morais, 2007] P. Cortez ve A. Morais. Bir veri madenciliği yaklaşım tahmin orman Meteorological verileri kullanarak etkinleşir. J. Neves, M. f Santos ve J. Machado Eds., yapay zeka, 13 EPIA 2007 - yapay zeka, aralık, Guimarães, Portekiz pp. 512-523, 2007 Portekizce konferans bildirileri yeni eğilimler. APPIA, ISBN-13 978-989-95618-0-9'İ TIKLATIN. Bulunabilir: <a href="http://www.dsi.uminho.pt/~pcortez/fires.pdf"> http://www.dsi.uminho.pt/~pcortez/fires.pdf </a>.
  </td>
</tr>

<tr>
  <td>Almanca kredi kartı UCI veri kümesi</td>
  <td>
UCI Statlog (Almanca kredi kartı) veri kümesi (<a href="http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">Statlog + Almanca + kredi + verilerini</a>), german.data dosyasını kullanarak.
<p></p>
Veri kümesi, kişiler, düşük veya yüksek kredi riskleri olarak öznitelikleri kümesi tarafından açıklanan sınıflandırır. Her örneğin bir kişi temsil eder. Sayısal ve kategorik, 20 özellikleri ve ikili etiket (kredi riski değer) vardır. Yüksek kredi riski girişleriniz etiket = 2, düşük kredi riski girişleriniz etiket = 1. Yüksek riskli örneğin düşük misclassifying maliyetini 5 iken yüksek olarak düşük riskli örnek misclassifying maliyeti 1 ' dir.
  </td>
</tr>

<tr>
  <td>IMDB başlık</td>
  <td>
Veri kümesi Twitter tweet'leri derecelendirilmiş filmler hakkında bilgiler içerir: IMDB film kimliği, film adı, tarzını ve üretim yıl. Veri kümesini 17K filmler vardır. Veri kümesi "S. yazıda sunulmuştur Dooms, t De Pessemier ve L. Martens. MovieTweetings: Veri kümesi derecelendirme film Twitter'dan toplanır. Atölye kitle kaynak ve İnsan hesaplama öneren sistemleri, RecSys 2013'te CrowdRec."
  </td>
</tr>

<tr>
  <td>Iris iki sınıf verileri</td>
  <td>
Desen tanıma belgelerinde bulunması için en iyi bilinen bir veritabanı belki de budur. Veri kümesi 50 örnekleri her üç iris çeşit yaprağı ölçümler içeren görece küçük.
<p></p>
<b>Kullanım:</b> ölçümleri iris türünden tahmin etmek.  
<p></p>
<b>İlgili araştırma:</b> Fisher, R.A. (1988). Depo öğrenme UCI makine <a href="http://archive.ics.uci.edu/ml"> http://archive.ics.uci.edu/ml </a>. Irvine, CA: California Üniversitesi, okul daha fazla bilgi ve bilgisayar bilimi </td>
</tr>

<tr>
  <td>Film Tweet'leri</td>
  <td>
Veri kümesi film Tweetings dataset genişletilmiş bir sürümüdür. Veri kümesi Twitter'da iyi yapılandırılmış tweet'leri ayıklanan filmler 170 K derecelendirmesi sahiptir. Her örneğinin tweet temsil eder ve bir tanımlama grubu olduğundan: kullanıcı kimliği, IMDB film kimliği, derecelendirme, zaman damgası, bu tweet ve bu tweet retweets sayısı için Sık Kullanılanlara numarası. Veri kümesi A. Denirse, S. Dooms, B. Loni ve D. Tikk tarafından öneren sistemleri sınama 2014 için kullanılabilir hale getirildi.
  </td>
</tr>

<tr>
  <td>Çeşitli otomobiller için MPG verileri</td>
  <td>
Bu veri kümesi Carnegie Mellon University StatLib kitaplığı tarafından sağlanan dataset biraz değiştirilmiş bir sürümüdür. Veri kümesi 1983 Amerikan istatistiksel ilişkilendirme Exposition içinde kullanıldı.
<p></p>
Verileri galon başına mil içinde çeşitli otomobiller için yakıt tüketimini listeler. Ayrıca, silindir, altyapısı öteleme, beygir gücü, toplam ağırlığı ve Hızlandırma sayısı gibi bilgileri içerir.
<p></p>
<b>Kullanım:</b> üç birden çok değerli ayrık öznitelikleri ve beş sürekli öznitelikleri temel alınarak yakıt ekonomi tahmin etmek. 
<p></p>
<b>İlgili araştırma:</b> StatLib, Carnegie Mellon University (1993). Depo öğrenme UCI makine <a href="http://archive.ics.uci.edu/ml"> http://archive.ics.uci.edu/ml </a>. Irvine, CA: California Üniversitesi, okul daha fazla bilgi ve bilgisayar bilimi </td>
</tr>

<tr>
  <td>Pima ana dilidir Diabetes ikili sınıflandırma veri kümesi</td>
  <td>
Bir veri alt kümesini National Institute Diabetes ve Digestive ve serbest Diseases veritabanı. Veri kümesi Pima Hindistan miras, kadın hastalar odaklanmak filtre. Veriler lifestyle Etkenler yanı sıra glucose ve insulin düzeyleri gibi sağlık verilerini içerir.
<p></p>
<b>Kullanım:</b> konu diabetes (ikili sınıflandırma) sahip olup olmadığını tahmin etmek. 
<p></p>
<b>İlgili araştırma:</b> Sigillito, V. (1990). Depo öğrenme UCI makine <a href="http://archive.ics.uci.edu/ml"> http://archive.ics.uci.edu/ml"</a>. Irvine, CA: California Üniversitesi, okul daha fazla bilgi ve bilgisayar bilimi </td>
</tr>

<tr>
  <td>Restoran müşteri verileri</td>
  <td>
Müşteriler, demografisine ve tercihleri de dahil olmak üzere ilgili meta verileri kümesi.
<p></p>
<b>Kullanım:</b> eğitmek ve öneren sistem test etmek için diğer iki Restoran veri kümeleri ile birlikte bu veri kümesi kullanın. 
<p></p>
<b>İlgili araştırma:</b> Bache, k ve Lichman, M. (2013). Depo öğrenme UCI makine <a href="http://archive.ics.uci.edu/ml"> http://archive.ics.uci.edu/ml </a>. Irvine, CA: California Üniversitesi, okul daha fazla bilgi ve bilgisayar bilimi.
  </td>
</tr>

<tr>
  <td>Restoran özelliği verileri</td>
  <td>
Restoran ve yemek türü, yerinizi stil ve konum gibi özelliklerine hakkındaki meta verileri kümesi.
<p></p>
<b>Kullanım:</b> eğitmek ve öneren sistem test etmek için diğer iki Restoran veri kümeleri ile birlikte bu veri kümesi kullanın. 
<p></p>
<b>İlgili araştırma:</b> Bache, k ve Lichman, M. (2013). Depo öğrenme UCI makine <a href="http://archive.ics.uci.edu/ml"> http://archive.ics.uci.edu/ml </a>. Irvine, CA: California Üniversitesi, okul daha fazla bilgi ve bilgisayar bilimi.
  </td>
</tr>

<tr>
  <td>Restoran derecelendirme</td>
  <td>
Bir ölçekte kullanıcılar tarafından Restoran için 2'ye 0'dan verilen derecelendirmeleri içeriyor.
<p></p>
<b>Kullanım:</b> eğitmek ve öneren sistem test etmek için diğer iki Restoran veri kümeleri ile birlikte bu veri kümesi kullanın. 
<p></p>
<b>İlgili araştırma:</b> Bache, k ve Lichman, M. (2013). Depo öğrenme UCI makine <a href="http://archive.ics.uci.edu/ml"> http://archive.ics.uci.edu/ml </a>. Irvine, CA: California Üniversitesi, okul daha fazla bilgi ve bilgisayar bilimi.
  </td>
</tr>

<tr>
  <td>Çelik Annealing çok sınıf veri kümesi</td>
  <td>
Bu veri kümesi denemeler annealing çelik kayıtlarından bir dizi içeriyor. Fiziksel öznitelikleri (genişlik, kalınlığı, elde edilen türünü (bobini, sayfası, vb.) çelik türleri. içerir
<p></p>
<b>Kullanım:</b> iki sayısal sınıf öznitelikleri; sertliği veya gücü tahmin etmek. Ayrıca öznitelikler arasında bağıntıları analiz.
<p></p>
Çelik dereceleri izleyin kümesi standart SAE ve diğer kuruluşlar tarafından tanımlanan. Özel bir 'düzeyde' (sınıf değişkeni) aradığınız ve gerekli değerleri öğrenmek istiyorsanız. 
<p></p>
<b>İlgili araştırma:</b> sterlin, d & Buntine, W. (NA). Depo öğrenme UCI makine <a href="http://archive.ics.uci.edu/ml"> http://archive.ics.uci.edu/ml </a>. Irvine, CA: California Üniversitesi, okul daha fazla bilgi ve bilgisayar bilimi
<p></p>
Dereceleri çelik için yararlı bir kılavuz şurada bulunabilir: <a href="http://www.outokumpu.com/SiteCollectionDocuments/Outokumpu-steel-grades-properties-global-standards.pdf">http://www.outokumpu.com/SiteCollectionDocuments/Outokumpu-steel-grades-properties-global-standards.pdf</a>
  </td>
</tr>

<tr>
  <td>Teleskop veri</td>
  <td>
Yüksek enerji gama parçacık kaydı arka plan gürültü yanı sıra, her ikisi de Monte Carlo bir işlemi kullanarak benzetimli WINS'e.
<p></p>
Plan tabanlı hava Cherenkov gama telescopes doğruluğunu artırmak için benzetimi amacı oluştu. Bu, istenen sinyal (Cherenkov Radyasyonu duşlar) ve arka plan gürültü (hadronic duşlar cosmic Işınları üst Atmosfer içinde tarafından başlatılan) birbirinden ayırmak için istatistiksel yöntemler kullanılarak gerçekleştirilir.
<p></p>
Verileri önceden işlenmiş elongated bir küme ile uzun ekseni kamera merkezi yönlendirilmiş oluşturmaktır. Bu üç nokta (Hillas parametreleri olarak da adlandırılır) özelliklerini Ayrımcılığı için kullanılabilir görüntü parametreleri arasındadır.
<p></p>
<b>Kullanım:</b> hediye görüntüsü sinyal ya da arka plan gürültü temsil edip etmediğini tahmin etmek.
<p></p>
<b>Notlar:</b> basit sınıflandırma doğruluğu değil Bu verileri için anlamlı sinyal sinyal olayı arka olarak sınıflandırma değerinden daha zayıf olduğu gibi bir arka plan olay sınıflandırma itibaren. Farklı sınıflandırıcı bir karşılaştırması için ROC grafik kullanılmalıdır. Sinyal aşağıdaki eşiklerin biri olmalıdır gibi bir arka plan olayı kabul olasılığını: 0,01 0,02, 0,05, 0,1 veya 0.2.
<p></p>
Ayrıca, arka plan olayları (hadronic duşlar için y) sayısı önemsemedi olduğunu unutmayın. Gerçek ölçüleri olayları çoğunluğu h veya gürültü sınıfı temsil eder. 
<p></p>
<b>İlgili araştırma:</b> Bock, R.K. (1995). Depo öğrenme UCI makine <a href="http://archive.ics.uci.edu/ml"> http://archive.ics.uci.edu/ml </a>. Irvine, CA: University of California Okul daha fazla bilgi </td>
</tr>

<tr>
  <td>Hava durumu veri kümesi</td>
  <td>
Saatlik kara bulunduğunuz gözlemleri NOAA gelen (<a href="http://cdo.ncdc.noaa.gov/qclcd_ascii/, merged data from 201304 to 201310">birleştirilmiş 201310 201304 verilerini</a>).
<p></p>
Hava durumu verileri havaalanı hava durumu istasyonlarını Nisan-Ekim 2013 zaman aralığını kapsayan yapılan gözlemleri kapsar. Azure Machine Learning Studio karşıya yüklemeden önce veri kümesi gibi işlenmiş:
<ul>
  <li>Hava durumu istasyon kimlikleri karşılık gelen havaalanı kimlikleri eşleştirilmiş</li>
  <li>Hava durumu 70 yoğun saatinde havaalanları ile ilişkili olmayan istasyonlar filtre</li>
  <li>Tarih sütunu ayrı yıl, ay ve gün sütunlara bölme</li>
  <li>Aşağıdaki sütun seçildi: AirportID yıl, ay, gün, saat, saat dilimi, SkyCondition, görünürlük, WeatherType, DryBulbFarenheit, DryBulbCelsius, WetBulbFarenheit, WetBulbCelsius, DewPointFarenheit, DewPointCelsius, RelativeHumidity, WindSpeed, WindDirection, ValueForWindCharacter, StationPressure, PressureTendency, PressureChange, SeaLevelPressure, RecordType, HourlyPrecip, Altimeter</li>
</ul>
  </td>
</tr>

<tr>
  <td>Wikipedia SP 500 veri kümesi</td>
  <td>
Veri Wikipedia türetilen (<a href="http://www.wikipedia.org/">http://www.wikipedia.org/</a>) XML verileri olarak depolanan her S & P 500 şirket makalelerinin temel.
<p></p>
Azure Machine Learning Studio karşıya yüklemeden önce veri kümesi gibi işlenmiş:
<ul>
  <li>Belirli her şirket için metin içeriği Ayıkla</li>
  <li>Wiki biçimlendirme Kaldır</li>
  <li>Alfasayısal olmayan karakter Kaldır</li>
  <li>Tüm metni küçük harfe Dönüştür</li>
  <li>Bilinen şirket kategorileri eklendi</li>
</ul>
<p></p>
Kayıt sayısını 500'den az olacak şekilde bir makale şirketler, bazı için Not bulunamadı.
  </td>
</tr>

<tr>
  <td><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/direct_marketing.csv">direct_marketing.csv</a></td>
  <td>
Veri kümesi, müşteri verileri ve bunların yanıtını doğrudan bir posta kampanya hakkında göstergeleri içerir. Her satır bir müşteri temsil eder. Kullanıcı demografisine hakkında ve geçmiş davranışı dokuz özellik veri kümesi içerdiğinden ve üç sütun etiket (, dönüştürme, ziyaret edin ve harcamanızı).  Ziyaret müşteri sonra pazarlama kampanyası ziyaret belirten ikili bir sütundur. Dönüştürme, bir müşteri bir şey satın gösterir. Harcamanız harcandığını tutar.  Veri kümesi, Kevin Hillstrom MineThatData e-posta analizi ve veri madenciliği sınaması için tarafından kullanılabilir olarak yapıldı.
  </td>
</tr>

<tr>
  <td><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/lyrl2004_tokens_test.csv">lyrl2004_tokens_test.csv</a></td>
  <td>
Test örnekleri RCV1 V2 Reuters haber kümesindeki özellikleri. Bir veri kümesine 781 K haber makalelerini kimlikleri birlikte sahiptir (ilk sütun kümesi). Her makalede, stopworded, simgeleştirilmiş ve stemmed. Veri kümesi David tarafından kullanılabilir hale getirildi. D. Gamze.
  </td>
</tr>

<tr>
  <td><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/lyrl2004_tokens_train.csv">lyrl2004_tokens_train.csv</a></td>
  <td>
Eğitim örneklerde RCV1 V2 Reuters haber veri kümesi özellikleri. 23 K haber makalelerini kimlikleri birlikte bir veri kümesine sahiptir (ilk sütun kümesi). Her makalede, stopworded, simgeleştirilmiş ve stemmed. Veri kümesi David tarafından kullanılabilir hale getirildi. D. Gamze.
  </td>
</tr>

<tr>
  <td><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/network_intrusion_detection.csv">network_intrusion_detection.csv</a><br></td>
  <td>
KDD fincanı 1999 bilgi bulma ve veri madenciliği kümesinden araçları rekabet (<a href="http://kdd.ics.uci.edu/databases/kddcup99/kddcup99.html">kddcup99.html</a>).
<p></p>
Veri kümesi indirilir ve Azure Blob depolama alanına depolanır (<a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/network_intrusion_detection.csv">network_intrusion_detection.csv</a>) ve hem eğitim ve sınama veri kümesi içerir. Eğitim veri kümesi yaklaşık 126 K satırları ve etiketleri de dahil olmak üzere 43 sütunları vardır. Üç sütun etiket bilgilerini bir parçasıdır ve 40 sütunlar, sayısal ve dize/kategorik özelliklerini oluşan modeli eğitmek için kullanılabilir. Test verileri eğitim verileri olduğu gibi aynı 43 sütunlarla örnekleri test yaklaşık 22,5 K vardır.
  </td>
</tr>

<tr>
  <td><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/rcv1-v2.topics.qrels.csv">rcv1-v2.topics.qrels.csv</a></td>
  <td>
Haber makalelerini RCV1 V2 Reuters haber kümesindeki atamaları konu. Bir haber makalesi çeşitli konulara atanabilir. Her satır biçimi "&lt;konu adı&gt; &lt;belge kimliği&gt; 1". Veri kümesi 2.6 M konu atamaları içeriyor. Veri kümesi David tarafından kullanılabilir hale getirildi. D. Gamze.
  </td>
</tr>

<tr>
  <td><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/student_performance.txt">student_performance.txt</a></td>
  <td>
Bu veriler KDD fincanı 2010 öğrenci performans değerlendirme sınama gelir (<a href="http://www.kdd.org/kdd-cup-2010-student-performance-evaluation">Öğrenci Performans değerlendirmesinin</a>). Kullanılan verileri Algebra_2008_2009 eğitim kümesidir (Stamper, J., Niculescu-Mizil, A., Ritter, S. Gordon, G.J. & Koedinger, K.R. (2010). Cebiri ı 2008-2009. KDD fincanı 2010 eğitim veri madenciliği sınama kümesinden sınaması. Şimdi Bul <a href="http://pslcdatashop.web.cmu.edu/KDDCup/downloads.jsp">downloads.jsp</a> veya <a href="http://www.kdd.org/sites/default/files/kddcup/site/2010/files/algebra_2008_2009.zip">algebra_2008_2009.zip</a>.
<p></p>
Veri kümesi indirilir ve Azure Blob depolama alanına depolanır (<a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/student_performance.txt">student_performance.txt</a>) ve sistem özel Ders öğrencinin günlük dosyalarını içerir. Sağlanan özellikler sorun kimliği ve kısa açıklamasını, Öğrenci Kimliği, zaman damgası ve kaç tane denemeleri doğru şekilde çözme önce yaptığınız Öğrenci içerir. Özgün veri kümesi 8,9 M kaydeder; yine de sahip istiyor musunuz? Bu veri kümesi, ilk 100 K satırları aşağı örneklenen olmuştur. Çeşitli türlerdeki 23 sekmeyle ayrılmış sütun kümesi vardır: kategorik, sayısal ve zaman damgası.
  </td>
</tr>

</table>


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
