---
title: Genel veri kümeleri için Azure analizi | Microsoft Docs
description: Genel veri kümeleri hakkında prototip için kullanın ve Azure Analiz Hizmetleri ve çözümleri test öğrenin.
services: sql-database
author: douglaslMS
manager: craigg
ms.custom: reference
ms.service: sql-database
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: douglasl
ms.openlocfilehash: be93dbff39c47ed1d8834efa3ef6c033525b4bea
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34648923"
---
# <a name="public-data-sets-for-testing-and-prototyping"></a>Test ve prototipi oluşturulurken ortak veri kümeleri

Bu prototip ve test depolama ve Analiz Hizmetleri ve çözümleri için kullanabileceğiniz veriler için ortak veri kümeleri listesini bulun.

## <a name="us-government-and-agency-data"></a>ABD Kamu ve Teşkilatı verileri

| Veri kaynağı | Veriler hakkında | Dosyaları hakkında |
|---|---|---|
| [ABD devlet kurumları veri](https://www.census.gov/data.html) | Üzerinde 190,000 veri kümelerini kapsayan Tarım, iklim, tüketici, ekosistemlerini, eğitim, enerji, Finans, sistem durumu, yerel kamu, üretim, maritime, Okyanusu, genel güvenlik ve Bilim ve ABD'de araştırma | HTML, XML, CSV, JSON, Excel ve diğer birçok dahil olmak üzere çeşitli biçimlerde çeşitli boyutlarda dosyaları. Dosya biçimi tarafından kullanılabilir veri kümelerini filtreleyebilirsiniz. |
| [ABD Census veri](https://www.census.gov/data.html) | ABD popülasyonunu hakkında istatistiksel veriler | Veri, çeşitli biçimlerde kümeleridir. |
| [NASA Earth Bilim verileri](https://earthdata.nasa.gov/) | Tarım, Atmosfer, biosphere, iklim, cryosphere, İnsan boyutları, hydrosphere, kara yüzeyini, oceans, sun earth etkileşimleri ve daha fazlasını kapsayan üzerinde 32.000 veri koleksiyonları. | Veri, çeşitli biçimlerde kümeleridir. |
| [Uçak uçuş gecikmeleri ve diğer taşıma verileri](https://www.transtats.bts.gov/OT_Delay/OT_DelayCause1.asp) | "ABD Departman, taşıma (nokta) kuruluşu parçalarının yurtiçi uçuşlar zamanında performansını işletilen taşıma istatistikleri (BTS) büyük hava hizmet sağlayıcılar tarafından kullanıcının. Özet bilgileri zamanında, Gecikmeli, iptal edilmiş ve yolu saptırabilir uçuşlar sayısı... Bu Web sitesinde yayınlanan Özet tabloları görüntülenir." | CSV biçiminde dosyalardır. |
| [Trafik fatalities - BİZE Fatality analiz raporlama sistemi (FARS)](http://www.nhtsa.gov/FARS) | "FARS NHTSA, Kongre, sağlama ülke çapında bir census olduğu ve motor araç trafiği kilitlenmelere önemli injuries ilgili Amerikan ortak yıllık verileri başlayamıyorsa." | "FARS sorgu sistemi kullanarak çevrimiçi çalıştırmak için kendi fatality verilerinizi oluşturun. Veya tüm FARS veri FTP sitesinden sunmak için 1975'ten indirin." |
| [Toxic kimyasal verileri - EPA Toxicity ForeCaster (ToxCast™)](https://www.epa.gov/chemical-research/toxicity-forecaster-toxcasttm-data) | "En güncel, genel kullanıma açık yüksek verimlilik toxicity verileri chemicals binlerce EPA'ın. Bu veriler EPA'ın ToxCast araştırma çaba oluşturulur." | Veri kümeleri, elektronik tablolar, R paketleri ve MySQL veritabanı dosyaları dahil olmak üzere çeşitli biçimlerde kullanılabilir. |
| [Toxic kimyasal data - NIH Tox21 veri sınama 2014](https://tripod.nih.gov/tox21/challenge/) | "2014 Tox21 veri sınama bilimcilerine chemicals ve toxic efektleri sonuçlanabilir yolla Biyolojik yollarıyla kesintiye girişimi 21, Toxicology aracılığıyla sınanan bileşimlerini olasılığı anlamanıza yardımcı olmak için tasarlanmıştır." | Veri kümeleri GÜLÜMSEMELER ve SDF kullanılabilir biçimleri. Veri sağlayan "assay etkinlik verileri ve ~ 10.000 bileşimlerini Tox21 koleksiyonunu kimyasal yapıları (Tox21 10K)." |
| [NCBI biotechnology ve genome verileri](https://www.ncbi.nlm.nih.gov/guide/data-software/) | Birden çok veri kümesi genes, genomes ve proteins kapsayan. | Veri kümeleri, metin, XML, topluca ve diğer biçimlere ' dir. TOPLUCA uygulama kullanılamaz. |

## <a name="other-statistical-and-scientific-data"></a>Diğer istatistiksel ve bilimsel veri

| Veri kaynağı | Veriler hakkında | Dosyaları hakkında |
|---|---|---|
| [New York şehrinde ücreti verileri](http://www.nyc.gov/html/tlc/html/about/trip_record_data.shtml) | "Toplama yakalama alanları ücreti seyahat kayıtları içerir ve teslim tarih/zaman, toplama ve teslim konumları, seyahat uzaklıkları, dökümü fares, oranı türleri, ödeme türleri ve sürücü bildirilen yolcu sayar." | Veri kümeleri CSV aya göre dosyalarıdır. |
| [Microsoft Research veri kümelerini - "Research için veri bilimi"](https://www.microsoft.com/research/academic-program/data-science-microsoft-research/) | Birden çok veri bilgisayar insan etkileşimi, ses/video, veri araştırma/bilgi alma, Jeo-uzamsal/konum, doğal dil işleme ve robotics/bilgisayar görme kapsayan ayarlar. | Veri indirme için daraltılmış çeşitli biçimlerde kümeleridir. |
| [Ortak genome veri](http://www.completegenomics.com/public-data/) | "Tüm İnsan genomes farklı bir veri kümesinin hiçbir genomic incelemesi geliştirmek... genel kullanım için ücretsiz kullanılabilir" Sağlayıcı, tam Genomics bir özel for-profit bir şirkettir. | Veri kümeleri, ayıklama işleminden sonra UNIX metin biçimindedir. Çözümleme Araçları de kullanılabilir. |
| [Açık Bilim veri bulut veri](https://www.opensciencedatacloud.org/) | "Açık Bilim veri bulut depolama, paylaşımı ve terabayt ve Petabayt ölçekli bilimsel veri kümeleri çözümleme kaynaklarla bilimsel topluluk sağlar."| Veri, çeşitli biçimlerde kümeleridir. |
| [Genel iklim data - WorldcLIM](http://worldclim.org/) | "WorldClim genel iklim Katmanlar (gridded iklim veri) yaklaşık 1 km2 uzamsal çözünürlüğü ile kümesidir. Bu verileri eşleştirmesi ve uzamsal modelleme için kullanılabilir." | Bu dosyalar Jeo-uzamsal verileri içerir. Daha fazla bilgi için bkz: [veri biçimi](http://worldclim.org/formats1). |
| [Veri İnsan topluluğu - GDELT Project hakkında](http://www.gdeltproject.org/data.html) | "GDELT projedir en büyük, en kapsamlı ve en yüksek çözünürlük İnsan topluluğu şimdiye kadar oluşturulan veritabanı açın." | Ham verileri, CSV biçiminde dosyalardır. |
| [Reklam tahmin veri Criteo gelen machine learning için tıklatın](http://labs.criteo.com/2013/12/download-terabyte-click-logs/) | "En büyük herhangi bir zamanda genel olarak yayımlanan ML dataset." Daha fazla bilgi için bkz: [Criteo'nın 1 TB'ı tıklatın tahmin Dataset](https://blogs.technet.microsoft.com/machinelearning/2015/04/01/now-available-on-azure-ml-criteos-1tb-click-prediction-dataset/). | |
| [ClueWeb09 metin araştırma veri Lemur Project kümesinden](http://www.lemurproject.org/clueweb09.php/) | "ClueWeb09 dataset araştırma bilgileri alma ve ilgili İnsan dil teknolojilerini desteklemek için oluşturuldu. Yaklaşık 1 milyar web sayfaları Ocak ve Şubat 2009 toplanan 10 dillerde oluşur." | Bkz: [veri kümesi bilgilerini](http://www.lemurproject.org/clueweb09/datasetInformation.php).|

## <a name="online-service-data"></a>Çevrimiçi hizmet verileri

| Veri kaynağı | Veriler hakkında | Dosyaları hakkında |
|---|---|---|
| [GitHub arşiv](https://www.githubarchive.org/) | "GitHub arşiv ortak GitHub zaman çizelgesi [events] kaydetmek, arşivlemek ve daha fazla çözümleme için kolayca erişilebilir hale getirmek için bir projedir." | JSON encloded olay arşivler .gz (Gzip) biçiminde bir web istemcisinden indirin. |
| [GitHub etkinlik verileri GHTorrent projeden](http://ghtorrent.org/) | "GHTorrent [GitHub REST API aracılığıyla sunulan veri ölçeklenebilir, sorgulanabilir, çevrimdışı bir yansıtma oluşturmak için çaba projedir]. GHTorrent GitHub genel olay zaman çizelgesi izler. Her olay için içeriği ve onların bağımlılıkları ayrıntısına alır." | MySQL veritabanı dökümleri CSV biçimindedir. |
| [Yığın Taşması Veri dökümü](https://archive.org/details/stackexchange) | "[Yığın taşması dahil olmak üzere] yığını Exchange ağ üzerindeki tüm kullanıcı katkıda bulunan içerik anonim bir döküm olduğu." | "[Yığın taşması] gibi-her site XML dosyalarını oluşan ayrı bir arşiv aracılığıyla daraltılmış olarak biçimlendirilmiş 7-zip bzıp2 sıkıştırma kullanılarak. Her site arşiv gönderileri, kullanıcılar, oyları, yorumlar, PostHistory ve PostLinks içerir." |
