---
title: "Azure Machine Learning veriler hazırlıkları kullanma hakkında ayrıntılı kılavuz | Microsoft Docs"
description: "Bu belge, genel bir bakış ve Azure Machine Learning veriler hazırlıkları ile veri sorunları hakkında ayrıntılı bilgi sağlar."
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: 
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 09/07/2017
ms.openlocfilehash: 1a1e12dbb5e32f62266ee6a3cdca9e781569e58c
ms.sourcegitcommit: 2d1153d625a7318d7b12a6493f5a2122a16052e0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2017
---
# <a name="data-preparations-user-guide"></a>Veriler hazırlıkları Kullanıcı Kılavuzu 
Azure Machine Learning veriler hazırlıkları deneyimi çok zengin işlevsellik sağlar. Bu makale deneyimi derin bölümlerini içermektedir.

### <a name="step-execution-history-and-caching"></a>Adım yürütme, geçmiş ve önbelleğe alma 
Veriler hazırlıkları adım Geçmiş bir dizi performans nedenleriyle önbellekleri tutar. Bir adımı seçin ve bir Önbelleği İsabetli Okuma, yeniden yürütmez. Son adım geçmişinin yazma bloğundaki varsa ve adımları ileri ve geri çevirme, ancak hiçbir değişiklik yazma sonra ilk kez tetiklenen değil. Yeni bir yazma oluşur ve varsa eskisinin üzerine yazılır:

- Değişiklikleri yazma bloğuna yapın.
- Yeni bir dönüştürme blok ekleyin ve önbellek geçersiz kılma oluşturur yazma blok üstüne taşıyın.
- Bir blok önbellek geçersiz kılma oluşturur yazma blok üstüne özelliklerini değiştirin.
- Yenileme (Bu nedenle geçersiz kılmalarını tüm önbellekleri) olan bir örneği seçin.

### <a name="error-values"></a>Hata değerleri

Bu değer uygun şekilde işlenemiyor çünkü veri dönüşümleri için bir giriş değerinin başarısız olabilir. Örneğin, giriş dizesi değerini belirtilen hedef türüne yayınlanamıyor, türü zorlama işlemleri söz konusu olduğunda, zorlama başarısız olur. Tür zorlama işlemi dize türünde bir sütun bir sayısal ya da Boole türüne dönüştürme veya yinelenen mevcut olmayan bir sütun çalışılıyor. (Bu hatayı taşıma sonucu olarak ortaya çıkar *sütun X Sil* işlemi tamamlanmadan *yinelenen sütun X* işlemi.)

Bu durumlarda, veriler hazırlıkları bir hata değeri çıktı olarak üretir. Hata değerlerini önceki bir işlem için belirtilen değer başarısız olduğunu gösterir. Dahili olarak, birinci sınıf değer türü olarak kabul, ancak bir sütun tamamen hata değerden oluşur olsa bile varlıklarını bir sütunun temel alınan tür değiştirmez.

Hata değerleri tanımlamak kolaydır. Kırmızı ile vurgulanan ve okuma "Hatası." Hatanın nedenini belirlemek için hata metni açıklamasını görmek için bir hata değeri üzerine gelerek.

Hata değerlerini yayılır. Bir hatanın ardından değeri oluşur, çoğu işlemleri üzerinden hata olarak çoğu durumda yayar. Değiştirin veya bunları kaldırmak için üç yolu vardır:

* Değiştir
    -  Bir sütunu sağ tıklatın ve seçin **hata değerlerini değiştirmek**. Sonra bir değiştirme değeri sütununda bulunan her hata değerinin seçebilirsiniz.

* Kaldır
    - Veriler hazırlıkları korumak veya hata değerlerini kaldırmak için etkileşimli filtreler içerir.
    - Bir sütunu sağ tıklatın ve seçin **filtre sütunu**. Korumak veya hata değerlerini kaldırmak için bir koşullu koşulu ile oluşturma *"hata olduğu"* veya *"hata değil."*

* Koşullu hata değerlerini üzerinde çalışması için bir Python deyimi kullanın. Daha fazla bilgi için bkz: [Python Uzantıları bölümüne](data-prep-python-extensibility-overview.md).

### <a name="sampling"></a>Örnekleme
Bir veri kaynakları dosyayı yerel dosya sistemine veya uzak bir konumdaki bir veya daha fazla kaynaklardan gelen ham verileri alır. Örnek blok mi örnekleri oluşturarak verilerinin bir kısmını çalışmaya belirtmenize olanak tanır. Daha sonraki adımlarda işlemleri gerçekleştirdiğinizde örneği büyük bir veri kümesi yerine verileri genellikle işletim daha iyi performans için yol gösterir.

Her veri kaynakları dosyası için birden fazla örnek oluşturulur ve depolanır. Ancak, yalnızca bir örnek etkin örnek olarak ayarlanabilir. Oluşturmak, düzenlemek veya veri kaynağı Sihirbazı'nda veya örnek blok düzenleyerek örneklerini silin. Bir veri kaynağına kendiliğinden başvuran herhangi bir veriler hazırlıkları dosya veri kaynaklarını dosyasında belirtilen örnek kullanın.

Bir dizi örnekleme stratejileri kullanılabilir, her farklı yapılandırılabilir parametrelerle vardır.

#### <a name="top"></a>Sayfanın Üstü
Bu strateji, yerel veya uzak dosyalara uygulanabilir. Veri kaynağında (sayısı tarafından belirtilen) ilk N satırları sürer.

#### <a name="random-n"></a>Rastgele N 
Bu strateji, yalnızca yerel dosyalara uygulanabilir. İşlem, veri kaynağında rastgele N satırları (sayısı tarafından belirtilen) alır. Aynı örnek oluşturulan koşuluyla sayısı da aynı olduğundan emin olmak için belirli bir çekirdek sağlayabilir.

#### <a name="random-"></a>Rastgele % 
Bu strateji, yerel veya uzak dosyalara uygulanabilir. Her iki durumda da, bir olasılık ve çekirdek olmalıdır sağlanan rastgele N stratejisi benzer.

Uzak dosyaları örnekler için ek parametreler sağlanması gerekir:

- Örnek oluşturucusu 
  - Spark kümesi seçin veya uzak Docker işlem örnek oluşturma için kullanılacak hedef. Projenin önceden bu listesinde görünmesi için işlem hedef oluşturulması gerekir. Bölümündeki adımları "oluşturma yeni bir işlem hedef" içinde izleyin [GPU Azure Machine Learning kullanma](how-to-use-gpu.md) işlem hedeflerini oluşturmak için.
- Örnek Depolama 
  - Uzak örnek depolamak için bir ara depolama konumu belirtin. Bu yol giriş dosyası konumundan farklı bir dizin olmalıdır.

#### <a name="full-file"></a>Tam dosya 
Bu strateji, veri kaynağında tam dosya alma yalnızca yerel dosyalara uygulanabilir. Dosya çok büyük ise, bu seçeneği uygulamayı gelecekteki işlemlerde aşağı yavaşlatabilir. Farklı örnekleme stratejisi kullanmak daha uygun bulabilirsiniz.


### <a name="fork-merge-and-append"></a>Çatallaştırma, birleştirme ve ekleme

Bir veri kümesi üzerinde filtre uyguladığınızda işlemi verileri iki sonuç kümeleri halinde ayırır.: bir küme filtresinde başarılı kayıtları temsil eder ve başarısız olan kayıtları için başka bir kümesidir. Her iki durumda da, kullanıcının görüntülemek için hangi sonuç kümesini seçebilirsiniz. Kullanıcı, diğer veri kümesi atmak ya da yeni bir veri akışı yerleştirin. İkinci seçeneği forking olarak adlandırılır.

Çatallaştırma için: 
1. Sütun, sağ tıklatın ve seçin seçin **filtre** sütun.

2. Altında **t istediğiniz**seçin **tutmak satırları** filtresini geçen sonuç kümesini görüntülemek için.

3. Seçin **satırları Kaldır** başarısız kümesini görüntülemek için.

4. Sonra **koşullar**seçin **çıkışı filtre satırları içeren Oluştur veri akışı** görüntü olmayan sonuç kümesinde yeni bir veri akışına dizisinde çatallaştırmak için.


Bu yöntem, genellikle bir ek hazırlık gerektirir veri kümesi ayırmak için kullanılır. Forked dataset wrangle sonra sonuç özgün veri akışı kümesi ile verileri birleştirmek için yaygın bir sorundur. Bir birleştirme (çatalı işlemi ters) gerçekleştirmek için aşağıdaki eylemlerden birini kullanın:

- **Satır sona**. İki veya daha fazla veri akışları dikey birleştirme (row-wise). 
- **Sütunlar ekleme**. İki veya daha fazla veri akışları yatay birleştirme (column-wise).


>[!NOTE]
>Bir sütun çakışma olursa sütunları başarısız ekleyin.


Birleştirme işleminden sonra bir veya daha fazla veri akışları kaynak veri akışı tarafından başvurulur. Veriler hazırlıkları simgesiyle sizi uyarır adımlarının listesi altında uygulamanın sağ alt köşesinde bir bildirim.


Başvurulan veri akışı üzerinde herhangi bir işlemi başvurulan veri akışından kullanılan örnek yenilemek için ana veri akışı gerektirir. Bu olay, bir onay iletişim kutusunda sağ alt köşedeki veri akışı başvuru bildirimi yerini alır. Bu iletişim kutusu, hiçbir bağımlılık veri akışları yapılan değişiklikler ile eşitlemek için veri akışı yenilemeleri gerektiğini doğrular.

### <a name="list-of-appendices"></a>Ekler listesi 
* [Desteklenen veri kaynakları](data-prep-appendix2-supported-data-sources.md)  
* [Desteklenen dönüşümler](data-prep-appendix3-supported-transforms.md)  
* [Desteklenen denetçiler](data-prep-appendix4-supported-inspectors.md)  
* [Desteklenen hedefleri](data-prep-appendix5-supported-destinations.md)  
* [Python örnek filtre ifadelerinde](data-prep-appendix6-sample-filter-expressions-python.md)  
* [Örnek dönüştürme veri akışı Python ifadelerde](data-prep-appendix7-sample-transform-data-flow-python.md)  
* [Örnek veri kaynaklarında Python](data-prep-appendix8-sample-source-connections-python.md)  
* [Python örnek hedef bağlantıları](data-prep-appendix9-sample-destination-connections-python.md)  
* [Örnek sütun Python içinde dönüştürür](data-prep-appendix10-sample-custom-column-transforms-python.md)  
