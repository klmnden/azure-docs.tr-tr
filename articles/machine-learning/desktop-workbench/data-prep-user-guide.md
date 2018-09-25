---
title: Azure Machine Learning veri hazırlıkları kullanma hakkında ayrıntılı kılavuz | Microsoft Docs
description: Bu belge, bir genel bakış ve Azure Machine Learning veri hazırlıkları veri sorunları çözmek hakkında ayrıntılı bilgi sağlar.
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: ''
ms.devlang: ''
ms.topic: article
ms.date: 02/01/2018
ROBOTS: NOINDEX
ms.openlocfilehash: 7536e67d0ae4973008c8acc91a99a7d0d286f9b8
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46988742"
---
# <a name="data-preparations-user-guide"></a>Veri hazırlıkları Kullanıcı Kılavuzu 

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 


Azure Machine Learning veri hazırlıkları deneyimi çok zengin işlevsellik sağlar. Bu makale deneyimi en derin bölümlerini içermektedir.

### <a name="step-execution-history-and-caching"></a>Adım yürütme geçmişini ve önbelleğe alma 
Bir dizi önbellek Performans nedeniyle veri hazırlıkları adım geçmişini tutar. Bir adım seçin ve bir önbellek isabet, yeniden yürütülmez. Bir yazma blok adım geçmişi sonunda varsa ve adımları ileri ve geri çevirme, ancak hiçbir değişiklik yazma sonra ilk kez tetiklenmez. Yeni bir yazma oluşur ve eskisinin üzerine yazar:

- Yazma blokla değişiklikleri yapın.
- Yeni bir dönüştürme bloğu ekleyin ve önbellekte geçersiz kılma oluşturur yazma blok üstüne taşıyın.
- Bir blok önbellekte geçersiz kılma oluşturur yazma blok üstüne özelliklerini değiştirin.
- (Bu nedenle tüm önbellekleri geçersiz kılmalarını) örnek üzerinde Yenile'yi seçin.

### <a name="error-values"></a>Hata değerlerini

Bu değeri uygun şekilde işlenemez çünkü veri dönüşümleri giriş değeri için başarısız olabilir. Örneğin, giriş dizesi değerini belirtilen hedef türüne yayınlanamıyor, türü zorlama işlemleri söz konusu olduğunda, zorlama başarısız olur. Bir tür zorlama işlemi dize türünde bir sütun bir sayısal veya Boolean türüne dönüştürme veya var olmayan bir sütunu yinelenen çalışılıyor. (Bu geçişin sonucu olarak arıza *X sütununu Sil* işlemden önce *yinelenen sütun X* işlemi.)

Bu durumlarda, veri hazırlıkları çıktı olarak bir hata değeri oluşturur. Hata değeri için belirtilen değer bir önceki işlemin başarısız olduğunu gösterir. Dahili olarak, bunlar bir birinci sınıf bir değer türü kabul, ancak bir sütun değerlerini tamamen Hata oluşsa bile varlıklarını bir sütunun temel alınan türü değiştirmez.

Hata değerlerini tanımlamak kolaydır. Kırmızı renkte vurgulanmış ve okuma "Hatası." Hatanın nedenini belirlemek için hatanın metin açıklamasını görmek için bir hata değeri üzerine gelin.

Hata değerlerini yayar. Bir hatanın ardından değeri ortaya çıkar, çoğu durumda çoğu işlemi aracılığıyla hata olarak yayar. Değiştirin veya bunları kaldırmak için üç yol vardır:

* Değiştir
    -  Bir sütuna sağ tıklayın ve seçin **hata değerleri Değiştir**. Sonra bir değiştirme değeri sütununda bulunan her bir hata değeri için seçebilirsiniz.

* Kaldır
    - Veri hazırlıkları korumak veya hata değerlerini kaldırmak için etkileşimli bir filtre içerir.
    - Bir sütuna sağ tıklayın ve seçin **filtre sütunu**. Korumak ya da hata değerlerini kaldırmak için koşulunu sağlayacak bir koşullu oluşturma *"hata olduğu"* veya *"hata değil."*

* Hata değerlerini koşullu olarak çalışmak için Python ifade kullanın. Daha fazla bilgi için [Python uzantıları bölümünde](data-prep-python-extensibility-overview.md).

### <a name="sampling"></a>Örnekleme
Bir veri kaynağı dosyası ya da yerel dosya sistemine veya uzak bir konumdan bir veya daha fazla kaynaktan alınan ham verileri alır. Örnek blok örnekleri oluşturarak bir veri alt kümesi ile çalışmak etkinleştirilip etkinleştirilmeyeceğini belirtmenizi sağlar. Sonraki adımlarda işlemleri gerçekleştirdiğinizde örneği büyük bir veri kümesi yerine verileri genellikle işletim daha iyi performansa neden olur.

Her veri kaynağı dosyası için birden fazla örnek oluşturulur ve depolanır. Ancak, tek örnek etkin örnek olarak ayarlanabilir. Oluşturmak, düzenlemek veya veri kaynağı Sihirbazı'nda veya örnek blok düzenleyerek örneklerini silin. Veri kaynakları dosyasında belirtilen örnek doğası gereği bir veri kaynağına başvuran veri hazırlıkları dosyaları kullanın.

Bir dizi örnekleme stratejileri kullanılabilir, her biri farklı yapılandırılabilir parametreler vardır.

#### <a name="top"></a>Üst
Bu strateji, yerel veya uzak dosyalara uygulanabilir. İşlem veri kaynağının ilk N satırları (sayısı tarafından belirtilen) alır.

#### <a name="random-n"></a>Rastgele N 
Bu strateji, yalnızca yerel dosyalar için uygulanabilir. İşlem, veri kaynağında (sayısı tarafından belirtilen) rastgele N satırları alır. Sayısı da aynı olması şartıyla, aynı örnek oluşturulan emin olmak için belirli bir çekirdek sağlayabilir.

#### <a name="random-"></a>Rastgele % 
Bu strateji, yerel veya uzak dosyalara uygulanabilir. Her iki durumda da bir olasılık ve çekirdek olmalıdır belirtilmezse, rastgele N stratejisi benzer.

Uzak dosyaları örnekleri için ek parametreler sağlanması gerekir:

- Örnek oluşturucusu 
  - Bir Spark kümesi seçin veya uzak bir Docker örnek oluşturmak için kullanılacak hedef işlem. İşlem hedef projenin bu listede görünmesini için önceden oluşturulmuş olması gerekir. Aşağıdaki bölümde yer alan adımları "oluşturma yeni bir işlem hedefine" içinde [Azure Machine Learning'de GPU kullanmayı](how-to-use-gpu.md) işlem hedeflerini oluşturmak için.
- Örnek Depolama 
  - Uzak örneği depolamak için bir ara depolama konumunu belirtin. Bu yol, giriş dosyası konumundan farklı bir dizin olmalıdır.

#### <a name="full-file"></a>Tam dosya 
Bu strateji veri kaynağının tam dosya alma yalnızca yerel dosyalara uygulanabilir. Dosya çok büyük ise, bu seçeneği uygulamayı gelecekteki işlemlerde aşağı yavaşlatabilir. Farklı bir örnekleme stratejisi kullanmak daha uygun bulabilirsiniz.


### <a name="fork-merge-and-append"></a>Çatal, birleştirme ve ekleme

Bir veri kümesi üzerinde bir filtre uyguladığınızda, işlemi verileri iki sonuç kümeleri halinde böler.: bir küme filtresinde başarılı kayıtlar temsil eder ve başarısız olan kayıtları için başka bir kümesidir. Her iki durumda da, kullanıcının görüntülemek için hangi sonuç kümesini seçebilirsiniz. Kullanıcı, başka bir veri kümesi atmak ya da yeni bir veri akışı yerleştirin. İkinci seçeneği çatal olarak adlandırılır.

Çatal için: 
1. Bir sütun, sağ tıklatın ve seçin seçin **filtre** sütun.

2. Altında **istiyorum**seçin **satırları tutmak** filtrenin geçirir sonuç kümesini görüntülemek için.

3. Seçin **satırları Kaldır** ayarlanamadı görüntülenecek.

4. Sonra **koşullar**seçin **Oluştur veri akışı dışarı filtrelenen satırlar içeren** görünen olmayan sonuç yeni bir veri akışına kümesini dizisinde çatallaştırmak için.


Bu yöntem, genellikle bir ek hazırlık gerektirir veri kümesini ayırmak için kullanılır. Çatalı oluşturulan veri kümesi hazır sonra veriler özgün veri akışı sonuç birleştirmek için yaygındır. Bir birleştirme (Çatal işlemi tersine) gerçekleştirmek için aşağıdaki eylemlerden birini kullanın:

- **Satırlar ekleme**. İki veya daha fazla veri akışları dikey birleştirme (row-wise). 
- **Sütunlar ekleme**. Yatay olarak iki veya daha fazla veri akışları Birleştir (column-wise).


>[!NOTE]
>Bir sütun çakışma oluşursa sütunları başarısız ekleyin.


Sonra bir birleştirme işlemi, bir veya daha fazla veri akışı, bir kaynak veri akışı tarafından başvurulur. Veri hazırlıkları bildirir, adımları listesi altına uygulamanın sağ üst köşedeki bildirimi.


Başvurulan veri akışından kullanılan örnek yenilemek için ana veri akışı başvurulan veri akışı üzerinde herhangi bir işlem gerektirir. Bu olay, onay iletişim kutusunda sağ alt köşedeki veri akışı başvuru bildirim değiştirir. Bu iletişim kutusu herhangi bir bağımlılık veri akışı değişiklikler ile eşitlemek için veri akışı yenilemeleri gerektiğini doğrular.

### <a name="list-of-appendices"></a>Eklerin listesi 
* [Desteklenen veri kaynakları](data-prep-appendix2-supported-data-sources.md)  
* [Desteklenen dönüşümler](data-prep-appendix3-supported-transforms.md)  
* [Desteklenen denetçiler](data-prep-appendix4-supported-inspectors.md)  
* [Desteklenen hedefleri](data-prep-appendix5-supported-destinations.md)  
* [Python'da filtre ifadeleri örneği](data-prep-appendix6-sample-filter-expressions-python.md)  
* [Python'da örnek dönüşüm veri akışı ifadeleri](data-prep-appendix7-sample-transform-data-flow-python.md)  
* [Python örnek veri kaynakları](data-prep-appendix8-sample-source-connections-python.md)  
* [Python'da hedef bağlantıları örneği](data-prep-appendix9-sample-destination-connections-python.md)  
* [Python örnek sütun dönüşümleri](data-prep-appendix10-sample-custom-column-transforms-python.md)  
