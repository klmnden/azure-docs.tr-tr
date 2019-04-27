---
title: Azure analiz için ortak veri kümelerine | Microsoft Docs
description: Genel veri kümeleri hakkında bilgi edinin prototip kullanın ve test Azure Analiz Hizmetleri ve çözümleri.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: reference
author: WenJason
ms.author: v-jay
ms.reviewer: ''
manager: digimobile
origin.date: 10/01/2018
ms.date: 04/08/2019
ms.openlocfilehash: e7d01a6512c2d39c86da9f7020aa3988c9680c6b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60584463"
---
# <a name="public-data-sets-for-testing-and-prototyping"></a>Ortak veri kümelerine test etme ve prototip oluşturma

Prototip ve test depolama ve Analiz Hizmetleri ve çözümleri için kullandığınız veriler için ortak veri kümelerine bu listesine göz atın.

## <a name="us-government-and-agency-data"></a>ABD Kamu ve Aracısı verileri

| Veri kaynağı | Veriler hakkında | Dosyaları hakkında |
|---|---|---|
| [ABD devlet kurumları veri](https://www.census.gov/data.html) | Tarım, iklim, tüketici, Eko, eğitim, enerji, kapsayan veri kümeleri üzerinde 190,000 Finans, sistem durumu, yerel devlet, üretim, maritime, Okyanus, kamu güvenliği ve Bilim ve ABD'de araştırma | HTML, XML, CSV, JSON, Excel ve diğer birçok dahil olmak üzere çeşitli biçimlerde çeşitli boyutlardaki dosyaları. Dosya biçimi tarafından kullanılabilir veri kümelerini filtreleyebilirsiniz. |
| [ABD Görselleştirmenizdeki verilerin](https://www.census.gov/data.html) | ABD nüfuslarıyla ilgili istatistiksel veriler | Çeşitli biçimlerde veri kümeleridir. |
| [NASA Earth bilimi verileri](https://earthdata.nasa.gov/) | Tarım, Atmosfer, biosphere, iklim, cryosphere, İnsan boyutları, hydrosphere, land surface, oceans, sun earth etkileşimleri ve daha fazlasını kapsayan üzerinde 32.000 veri koleksiyonları. | Çeşitli biçimlerde veri kümeleridir. |
| [Havayolu uçuş gecikme ve diğer taşıma verileri](https://www.transtats.bts.gov/OT_Delay/OT_DelayCause1.asp) | "ABD Büyük hava operatörler tarafından işletilen yurtiçi uçuşlar tarihlere ait zamanında kalkış performansı nakliye istatistikleri (BTS) parçalar (nokta) bürosu ulaştırma Bakanlığı güçlendirin. Özet bilgileri zamanında, geciken, iptal edilen ve yolu saptırabilir uçuşlar sayısı... Bu Web sitesine gönderilen Özet tabloları görüntülenir." | CSV biçiminde dosyalardır. |
| [Trafik fatalities - BİZE Fatality analiz raporlama sistemi (FARS)](https://www.nhtsa.gov/FARS) | "Sağlama NHTSA, Kongre, ülke çapında bir sayım FARS olduğundan ve ilgili önemli injuries Amerikan genel yıllık verileri motor vehicle trafiği kilitlenmelere çalışmaya başlayamıyorsa." | "Çevrimiçi çalıştırmak FARS sorgu sistemini kullanarak kendi fatality veri oluşturun. Veya tüm FARS veri 1975 FTP sitesinden sunmak için indirin." |
| [Toxic kimyasal verileri - EPA Toxicity ForeCaster (ToxCast™)](https://www.epa.gov/chemical-research/toxicity-forecaster-toxcasttm-data) | "Chemicals binlerce EPA'ın en güncel, genel kullanıma açık yüksek performanslı toxicity verileri. Bu veriler EPA'ın ToxCast araştırma çaba oluşturulur." | Veri kümeleri, elektronik tablolar, R paketleri ve MySQL veritabanı dosyaları dahil olmak üzere çeşitli biçimlerde kullanılabilir. |
| [Toxic kimyasal veri - NIH Tox21 veri sınama 2014](https://tripod.nih.gov/tox21/challenge/) | "2014 Tox21 veri sınama uzmanları chemicals ve Toxicology Biyolojik yollarla toxic etkilere neden olabilir bir şekilde kesintiye 21. yüzyıla uygun girişim de aracılığıyla test edilen bileşimden anlamanıza yardımcı olmak için tasarlanmıştır." | Veri kümeleri GÜLÜMSEMELER ve SDF kullanılabilir biçimleri. Veri sağlayan "assay etkinlik verilerinizi ve yaklaşık 10.000 bileşimden Tox21 koleksiyonunu kimyasal yapıları (Tox21 10K)." |
| [NCBI biotechnology ve genom verileri](https://www.ncbi.nlm.nih.gov/guide/data-software/) | Birden çok veri kümesi genes, genom ve geliyor. | Veri kümeleri, metin, XML, ÖLÇEKTEYDİ ve diğer biçimlere ' dir. ÖLÇEKTEYDİ uygulaması kullanılabilir. |

## <a name="other-statistical-and-scientific-data"></a>Diğer istatistiksel ve bilimsel veriler

| Veri kaynağı | Veriler hakkında | Dosyaları hakkında |
|---|---|---|
| [New York taksi verilerini](http://www.nyc.gov/html/tlc/html/about/trip_record_data.shtml) | "Taksi seyahat kayıtlarını yakalama toplama alanları dahil dropoff tarihleri/saatleri, toplama ve dropoff konumları, seyahat uzaklıkları, listelenen fares, oranı türleri, ödeme türleri ve yolcular sürücüsü tarafından bildirilen sayar." | Veri kümeleri, aya göre CSV dosyalar sahiptir. |
| [Microsoft Research veri kümeleri - "Araştırma için veri bilimi"](https://www.microsoft.com/research/academic-program/data-science-microsoft-research/) | Birden çok veri bilgisayar insan etkileşimi, ses/video, veri araştırma/bilgi alma, Jeo-uzamsal/konum, doğal dil işleme ve robotlara ilişkin/görüntü işleme kapsayan ayarlar. | Çeşitli biçimlerdeki indirme için daraltılmış veri kümesidir. |
| [Ortak olarak genom veri](https://www.completegenomics.com/public-data/) | "Farklı bir veri kümesinin tamamını İnsan genom genetik bir araştırma geliştirmek üzere... genel kullanım için ücretsiz olarak kullanılabilir" Sağlayıcı, tam Genomiks bir özel açık bir şirkettir. | Veri kümeleri, ayıklama sonra UNIX metin biçimindedir. Analiz Araçları de mevcuttur. |
| [Açık bilimi verileri bulut veri](https://www.opensciencedatacloud.org/) | "Açık bilimi verileri bulut depolama, paylaşma ve terabayt ve Petabayt ölçekli bilimsel veri kümeleri çözümleme için kaynaklar ile bilimsel community sunuyor."| Çeşitli biçimlerde veri kümeleridir. |
| [Genel iklim verilerini - WorldClim](https://worldclim.org/) | "WorldClim genel iklim katmanları (gridded iklim verilerini) yaklaşık 1 km2 bir uzamsal çözünürlüğü kümesidir. Bu veri eşleme ve uzamsal modelleme için kullanılabilir." | Bu dosyalar, Jeo-uzamsal veriler içerir. Daha fazla bilgi için bkz. [veri biçimi](https://worldclim.org/formats1). |
| [İnsan society - GDELT Project ilgili veriler](https://www.gdeltproject.org/data.html) | "GDELT projedir en büyük, en kapsamlı ve yüksek çözünürlük bugüne kadar oluşturulmuş İnsan society veritabanı açın." | CSV biçiminde ham veriler dosyalarıdır. |
| [Machine learning hizmetinden Criteo tahmin verilerini reklam tıklayın](https://labs.criteo.com/2013/12/download-terabyte-click-logs/) | "En büyük hiç olmadığı kadar genel olarak yayımlanan ML veri kümesi." Daha fazla bilgi için bkz. [Criteo'nın 1 TB'ı tıklatın tahmin Dataset](https://blogs.technet.microsoft.com/machinelearning/20../../now-available-on-azure-ml-criteos-1tb-click-prediction-dataset/). | |
| [Lemur projeden ClueWeb09 metin araştırma veri kümesi](https://www.lemurproject.org/clueweb09.php/) | "ClueWeb09 veri kümesini araştırma bilgi alma ve ilgili İnsan dil teknolojileri desteklemek üzere oluşturulmuştur. Yaklaşık 1 milyar web sayfaları'nda Ocak ve Şubat 2009 toplanmış olan 10 dil oluşur." | Bkz: [veri kümesi bilgilerini](https://www.lemurproject.org/clueweb09/datasetInformation.php).|

## <a name="online-service-data"></a>Çevrimiçi hizmet verileri

| Veri kaynağı | Veriler hakkında | Dosyaları hakkında |
|---|---|---|
| [GitHub arşiv](https://www.githubarchive.org/) | "GitHub arşiv genel GitHub çizelgesine [olayların ile] kaydetmesini, arşivlemek ve daha fazla çözümleme için kolayca erişilebilir olması için bir proje var." | JSON olarak kodlanmış olay arşivleri .gz (Gzip) biçiminde bir web istemcisinde indirin. |
| [GitHub etkinlik verisinden GHTorrent proje](http://ghtorrent.org/) | "GHTorrent [GitHub REST API aracılığıyla sunulan veri ölçeklenebilir, sorgulanabilir, çevrimdışı bir yansıtma oluşturmak için çaba projedir]. GitHub genel olay zaman çizelgesinin GHTorrent izler. Her olay için içeriği ve bunların bağımlılıklarını ayrıntısına alır." | MySQL veritabanı dökümleri CSV biçimindedir. |
| [Yığın Taşması Veri dökümü](https://archive.org/details/stackexchange) | "[Stack Overflow dahil olmak üzere] Stack Exchange ağdaki tüm kullanıcı tarafından katkıda bulunulan içeriğin anonim bir döküm sağlıyor." | "[Stack Overflow] gibi-her sitenin aracılığıyla XML dosyasından oluşan ayrı bir arşiv daraltılmış olarak biçimlendirilmiş 7-zip bzıp2 sıkıştırın. Her site arşiv gönderileri, kullanıcılar, oy, yorumlar, PostHistory ve PostLinks içerir." |
