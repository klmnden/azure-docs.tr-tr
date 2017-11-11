---
title: "Temizleme ve Azure Machine Learning için verileri hazırlama | Microsoft Docs"
description: "Machine learning için hazırlamak üzere önceden işlem ve temiz verileri."
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: bdf659ec-4881-4324-8b9c-747cbfa0c3cd
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: bradsev
ms.openlocfilehash: 7f0c1f0f549e746cc99db3b47f6c90bb51145d5d
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="tasks-to-prepare-data-for-enhanced-machine-learning"></a>Gelişmiş machine learning için verileri hazırlama görevleri
Ön işleme ve veri temizleme dataset machine learning için etkili bir şekilde kullanılabilmesi için önce genellikle gerçekleştirilmesi gereken önemli görevlerdir. Ham verileri gürültülü ve güvenilmeyen görülür ve değerleri eksik olabilir. Bu tür veriler için modelleme kullanarak yanıltıcı sonuçlara yol açabilir. Bu görevleri takım veri bilimi işlem (TDSP) bir parçasıdır ve genellikle ilk incelenmesi bulmak ve gerekli ön işleme planı için kullanılan bir veri kümesinin izleyin. Daha ayrıntılı TDSP işlemi hakkında yönergeler için bkz: özetlenen adımları [takım veri bilimi işlemi](overview.md).

Ön işleme ve veri araştırması görev gibi görevleri temizleme ortamları, çeşitli araçlar ve R veya Python, verilerinizin depolandığı bağlı olarak gibi dilleri ve SQL veya Hive veya Azure Machine Learning Studio gibi çeşitli gerçekleştirilebilme ve nasıl biçimlendirilir. TDSP doğası gereği yinelemeli olduğundan, bu görevleri iş akışı işleminin çeşitli adımları sırasında gerçekleşebilir.

Bu makalede, çeşitli veri işleme kavramları ve önce veya sonra Azure Machine Learning veri alma üstlendiği görevleri tanıtır.

Veri keşfi ve Azure Machine Learning studio içinde yapılan ön işleme örneği için bkz: [Azure Machine Learning Studio'da verilerin önceden işlenmesi](https://azure.microsoft.com/documentation/videos/preprocessing-data-in-azure-ml-studio/) video.

## <a name="why-pre-process-and-clean-data"></a>Neden önceden işleyebilir ve veri temizleme?
Gerçek dünya veriler çeşitli kaynaklardan toplanır ve işlemleri ve sıradışı veya veri kümesi kalitesini tehlikeye bozuk veriler içeriyor olabilir. Ortaya çıkan tipik veri kalitesi sorunlar verilmiştir:

* **Tamamlanmamış**: öznitelikleri veya eksik değerler içeren veri eksik.
* **Gürültülü**: verileri hatalı kayıtları veya aykırı değerlerini içerir.
* **Tutarsız**: çakışan kayıtları veya tutarsızlıklar veri içeriyor.

Kalite veri kalitesi Tahmine dayalı modelleri için bir önkoşuldur. "Çöp girişi, atık" kaçının veri kalitesini geliştirmek ve bu nedenle performans modeli için veri sorunları erken nokta ve karşılık gelen veri işleme ve temizleme adımları karar için veri sistem durumu ekranı yürütmek için zorunludur.

## <a name="what-are-some-typical-data-health-screens-that-are-employed"></a>Çalışan bazı tipik veri sistem durumu ekranlar nelerdir?
Biz verilerin genel kalitesini denetleyerek denetleyebilirsiniz:

* Sayısı **kayıtları**.
* Sayısı **öznitelikleri** (veya **özellikleri**).
* Öznitelik **veri türleri** (nominal, sıralı veya sürekli).
* Sayısı **eksik değerleri**.
* **Doğru biçimlendirilmiş** verileri.
  * Veri TSV veya CSV, sütun ayırıcılar ve satır ayırıcı sütunları ve satırları her zaman doğru ayrı kontrol edin.
  * Verileri HTML veya XML biçiminde ise, verileri doğru kendi ilgili standartlarına göre biçimlendirildiğinden olup olmadığını denetleyin.
  * Ayrıştırma de bu kadar yarı yapılandırılmış veya yapılandırılmamış verileri yapılandırılmış bilgi ayıklamak için gerekli olabilir.
* **Tutarsız veri kayıtlarını**. Onay değerleri aralığı izin verilir. Örneğin Öğrenci GPA, belirlenen aralıkta GPA olup olmadığını denetleyin veri içeriyorsa, 0 söyleyin ~ 4.

Verileri ile ilgili sorunları bulduğunuzda **işleme adımları** genellikle temizleme eksik değerleri, veri normalleştirme, ayrılma kapsar gerekli, kaldırma ve/veya değiştirmek için metin işleme katıştırılmış veri etkileyebilir karakterleri hizalama, karma veri alanları ve diğerleri ortak yazar.

**Azure Machine Learning tüketir doğru biçimlendirilmiş tablo veri**.  Veriler tablo biçiminde ise, veri ön işleme doğrudan Machine Learning Studio'daki Azure Machine Learning ile gerçekleştirilebilir.  Veri Tablo formunda, XML'de olduğu say değilse ayrıştırma verileri tablo biçimine dönüştürmek için gerekli olabilir.  

## <a name="what-are-some-of-the-major-tasks-in-data-pre-processing"></a>Bazı önemli görevler verileri ön işleme nelerdir?
* **Veri temizleme**: doldurun veya eksik değerleri algılamak ve gürültülü veri ve aykırı değerlerini kaldırın.
* **Veri dönüştürme**: boyutları ve gürültü azaltmak için veri normalleştirin.
* **Veri azaltma**: örnek veri kayıtlarını ya da daha kolay veri işleme için öznitelikler.
* **Veri ayrılma**: Dönüştür sürekli özniteliklerin kullanım kolaylığı için kategorik öznitelikler belirli machine learning yöntemleriyle.
* **Metin Temizleme**: Örneğin, bir sekmeyle ayrılmış veri dosyasında katıştırılmış sekmeleri katıştırılmış için kayıtları, vb. kesilebilir yeni satırlar veri uyuşmazlığın neden olabilecek katıştırılmış karakterleri kaldırın.

Aşağıdaki bölümlerde bazı veri işleme adımlar ayrıntılı olarak açıklanmaktadır.

## <a name="how-to-deal-with-missing-values"></a>Eksik değerleri ile mücadele etmek nasıl?
Eksik değerleri ile mücadele etmek için daha iyi işlemek için eksik değerleri nedeni sorunu belirlemek öncelikle en iyisidir. Tipik eksik değeri işleme yöntemler şunlardır:

* **Silme**: eksik değerleri ile kayıt kaldırma
* **Kukla değiştirme**: eksik değerleri bir kukla değer ile değiştirin: Örneğin, *bilinmeyen* kategorik veya 0 için sayısal değerler için.
* **Değiştirme anlamına**: eksik verileri sayısal ise, eksik değerlerin ortalaması ile değiştirin.
* **Sık kullanılan alternatifi**: eksik verileri kategorik ise, eksik değerleri en sık kullanılan öğe ile değiştirin.
* **Regresyon değiştirme**: gerileyen değerlerle eksik değerleri değiştirmek için bir regresyon yöntemi kullanın.  

## <a name="how-to-normalize-data"></a>Veri normalleştirmek nasıl?
Veri normalleştirme sayısal değerlerini belirtilen bir aralıkta yeniden ölçeklendirir. Popüler veri normalleştirme yöntemler şunlardır:

* **Min-Max normalleştirme**: doğrusal olarak aralığı için veri dönüştürme, 0 ve 1 ' en küçük değer nerede ölçeği 0 ve en büyük değeri 1 arasında söyleyin.
* **Z-score normalleştirme**: ölçeklendirme ortalama ve standart sapma göre verileri: verileri ortalaması arasındaki fark standart sapmayı bölün.
* **Ondalık ölçeklendirme**: öznitelik değerinin Ondalık ayırıcının taşıyarak veri ölçeklendirin.  

## <a name="how-to-discretize-data"></a>Veri ayırmak için nasıl?
Veri nominal öznitelikleri veya aralıkları sürekli değerleri dönüştürerek ayrılmış. Bunun yapılması bazı yöntemler şunlardır:

* **Eşit genişlikte Binning**: bir özniteliğin tüm olası değerleri aralığı aynı boyutta N gruba ayırın ve kalan değerler bir depo depo numarasıyla atayın.
* **Eşittir yükseklikli Binning**: bir özniteliğin tüm olası değerleri aralığı N gruplara her örnekleri aynı sayıda içeren bölme sonra kalan değerler bir depo depo numarasıyla atayın.  

## <a name="how-to-reduce-data"></a>Verileri azaltmak nasıl?
Daha kolay veri işleme için veri boyutunu düşürmek için çeşitli yöntemler vardır. Veri boyutu ve etki alanına bağlı olarak, aşağıdaki yöntemlerden uygulanabilir:

* **Kayıt örnekleme**: örnek veri kayıtlarını ve yalnızca temsili alt verileri seçin.
* **Öznitelik örnekleme**: verileri yalnızca bir alt en önemli özniteliklerini seçin.  
* **Toplama**: verileri gruba ayırın ve her grup için sayıları depolamak. Örneğin, son 20 yılda bir restoran zinciri günlük gelir numaralarını verilerin boyutunu azaltmak için aylık gelir kümelenebilir.  

## <a name="how-to-clean-text-data"></a>Metin verilerini temizlemek nasıl?
**Tablo verisi metin alanlarında** sütunları hizalama ve/veya kaydı sınırları etkileyen karakter içerebilir. Örneğin, bir sekmeyle ayrılmış dosya neden sütun uyuşmazlığın içinde sekmeleri katıştırılmış ve katıştırılmış yeni satır karakterlerini kayıt satırları bölün. Metin yazma/okuma sırasında hatalı metin kodlama işleme için bilgi kaybı, giriş okunamaz karakter, örn., null değerlere ve de etkiler metni ayrıştırma yanlışlıkla yol açar. Dikkatli ayrıştırma ve düzenleme metin alanları uygun hizalama ve/veya metin yapılandırılmamış veya yarı yapılandırılmış verilerden yapılandırılmış extract verilere temizlemek için gerekli olabilir.

**Veri keşfi** veri erken bir görünüme sunar. Veri sorunları sayısı bu adım sırasında bitişik olabilir ve bu sorunları gidermek için karşılık gelen yöntemleri uygulanabilir.  Sorun kaynağı nedir ve nasıl sorun tanıtılmıştır gibi sorular sormak önemlidir. Bu da, bunları çözmek için yapılması gereken veri işleme adımı karar vermenize yardımcı olur. Bir veri türetilen amaçlayan Öngörüler türü, veri işleme çaba önceliğini belirlemek için de kullanılabilir.

## <a name="references"></a>Başvurular
> *Veri araştırma: Kavramlar ve teknikler*, üçüncü baskı, Morgan Kaufmann 2011, Jiawei Han, Micheline Kamber ve Jian Pei
> 
> 

