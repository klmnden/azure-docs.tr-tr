---
title: Azure veri Kataloğu'nda veri kaynaklarını kaydetme
description: Bu makalede, kayıt sırasında ayıklanan meta veri alanları dahil olmak üzere, Azure veri Kataloğu'nda veri kaynaklarını kaydetme vurgulanır.
services: data-catalog
author: JasonWHowell
ms.author: jasonh
ms.assetid: bab89906-186f-4d35-9ffd-61b1d903905d
ms.service: data-catalog
ms.topic: conceptual
ms.date: 01/18/2018
ms.openlocfilehash: 57b9a040b875c584b126e2062e4938b37875a31b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61001302"
---
# <a name="register-data-sources-in-azure-data-catalog"></a>Azure veri Kataloğu'nda veri kaynaklarını kaydetme
## <a name="introduction"></a>Giriş
Azure veri Kataloğu, kayıt ve kurumsal veri kaynakları için bulma sistemi olarak görev yapan tam yönetilen bir bulut hizmetidir. Diğer bir deyişle, keşfedin, anlamak ve veri kaynaklarını kullanan kişiler veri Kataloğu yardımcı olur ve bu kuruluşların var olan verilerden daha fazla değer elde etmesine yardımcı olur. Bir veri kaynağı, veri Kataloğu aracılığıyla bulunabilir hale ilk adımı, bu veri kaynağını kaydetme sağlamaktır.

## <a name="register-data-sources"></a>Veri kaynaklarını kaydetme
Kayıt, veri kaynağından meta verilerin ayıklanması ve bu verileri veri Kataloğu hizmetine kopyalanması işlemidir. Veriler o anda bulunduğu yerde kalır ve geçerli sistemin yöneticilerinin ve ilkelerinin denetiminde olmaya devam eder.

Bir veri kaynağına kaydetmek için aşağıdakileri yapın:
1. Azure veri Kataloğu Portalı'nda, veri Kataloğu veri kaynağı kayıt aracını başlatın. 
2. Portalda oturum açmak için kullandığınız aynı Azure Active Directory kimlik bilgileri, bir iş veya Okul hesabınızla oturum açın.
3. Kaydetmek istediğiniz veri kaynağını seçin.

Adım adım daha fazla ayrıntı için [Azure veri Kataloğu ile çalışmaya başlama](data-catalog-get-started.md) öğretici.

Veri kaynağı kaydettikten sonra katalog konumunu izler ve meta verilerini dizinler. Kullanıcıların arama, göz atın ve veri kaynağı bulma ve sonra uygulama veya kendi seçtikleri aracını kullanarak bağlanmak için konumu kullanın.

## <a name="supported-data-sources"></a>Desteklenen veri kaynakları
Şu anda desteklenen veri kaynakları listesi için bkz. [veri Kataloğu DSR](data-catalog-dsr.md).

## <a name="structural-metadata"></a>Yapısal meta verileri
Bir veri kaynağını kaydettiğinizde, kayıt aracı, seçtiğiniz nesnelerin yapısı hakkında bilgi ayıklar. Bu bilgiler, yapısal meta verilerin adlandırılır.

Verileri bulma kullanıcılar istedikleri istemci araçlarını nesnesinde bağlanmak için bu bilgileri kullanabilir, böylece tüm nesneler için nesnenin konumu bu yapısal meta verileri içerir. Nesne adı ve türü diğer yapısal meta verileri içerir ve öznitelik/sütun adı ve veri türü.

## <a name="descriptive-metadata"></a>Açıklayıcı meta verileri
Veri kaynağından ayıklanan çekirdek yapısal meta verilere ek olarak, veri kaynağı kayıt aracını açıklayıcı meta verileri ayıklar. SQL Server Analysis Services ve SQL Server Reporting Services için bu hizmetleri tarafından kullanıma sunulan açıklama özellikleri bu meta veriler alınır. SQL Server, ms tarafından sağlanan değerler için\_genişletilmiş özelliği Açıklama ayıklanır. Oracle veritabanı için veri kaynağı kayıt aracını COMMENTS sütunu tüm ayıklar\_sekmesini\_açıklamaları görüntüle.

Veri kaynağından ayıklanan açıklama meta veriler yanı sıra, kullanıcıların açıklayıcı meta verileri veri kaynağı kayıt aracını kullanarak girebilirsiniz. Kullanıcılar etiketler ekleyebilir ve bunlar experts routesuffix nesneler için belirleyebilirsiniz. Bu tanımlayıcı meta veri Kataloğu hizmetine yapısal meta verilerin yanı sıra kopyalanır.

## <a name="include-previews"></a>Önizlemeler içerir
Varsayılan olarak, yalnızca meta veri kaynaklarından ayıklanıp ve veri Kataloğu hizmeti, ancak içerdiği verilerin bir örnek görüntülediğinizde bir veri kaynağı genellikle daha kolay anlama kopyalanır.

Veri Kataloğu veri kaynağı kayıt aracını kullanarak, her bir tablo ve kayıtlı görünüm verilerin bir anlık görüntü önizlemesini içerebilir. Kayıt sırasında önizlemeleri eklemek isterseniz, kayıt aracı her bir tablo ve görünüm en fazla 20 kayıt içerir. Bu anlık görüntü, ardından birlikte yapısal ve açıklayıcı meta veri Kataloğu'na kopyalanır.

> [!NOTE]
> Çok sayıda sütun içeren geniş tablolarda, 20'den az kayıt, preview sürümüne sahip olabilir.
>
>

## <a name="include-data-profiles"></a>Veri profilleri içerir
Önizlemeleri de dahil olmak üzere veri Kataloğu'nda veri kaynakları için arama kullanıcılar için değerli bağlam yalnızca sağlayabilir gibi bir veri profili dahil olmak üzere, bulunan veri kaynaklarını anlamasına olanak kolaylaştırabilir.

Veri Kataloğu veri kaynağı kayıt aracını kullanarak, her bir tablo ya da kayıtlı görünüm için bir veri profili içerebilir. Veri profili kayıt sırasında içerecek şekilde seçerseniz, kayıt aracı verileri ilgili toplu istatistikler her bir tablo ve görünüm içerir dahil olmak üzere:

* Satır ve nesne verilerin boyutunu sayısı.
* Veri nesnesinin şema ve en son güncelleştirme tarihi.
* Null kayıtları ve sütun için farklı değerler sayısı.
* Sütunlar için minimum, maksimum, ortalama ve standart sapma değerleri.

Bu istatistikler, ardından birlikte yapısal ve açıklayıcı meta veri Kataloğu'na kopyalanır.

> [!NOTE]
> Metin ve tarih sütunlarını ortalama veya standart sapma istatistikleri veri profilinde dahil değildir.
>
>

## <a name="update-registrations"></a>Güncelleştirme kayıtları
Meta veriler ve isteğe bağlı Önizleme kayıt sırasında ayıklanan kullandığınızda bir veri kaynağı kaydetme, veri Kataloğu'nda bulunabilir kolaylaştırır. Veri kaynağı (örneğin, bir nesnenin şeması değişmiş, ilk olarak çıkarılan tabloları dahil edilmesi gereken veya önizlemelerinde dahil veri güncelleştirmek istediğiniz varsa) katalogdaki güncelleştirilmesi gerekiyorsa, veri kaynağı kayıt aracını yeniden çalıştırabilirsiniz.

Zaten kayıtlı veri kaynağı'nı yeniden kaydederek bir birleştirme "upsert" işlemi gerçekleştirir: varolan nesneleri güncelleştirilir ve yeni nesneler oluşturulur. Veri Kataloğu portalı yoluyla kullanıcılar tarafından sağlanan herhangi bir meta veri korunur.

## <a name="summary"></a>Özet
Bu yapısal ve açıklayıcı meta verileri bir veri kaynağından için katalog hizmeti kopyaladığı için veri Kataloğu'nda veri kaynağı kaydetme verileri bulunmasını ve anlaşılmasını kolaylaştırır. Veri kaynağı kaydettikten sonra ek açıklama, yönetmek ve veri Kataloğu portalını kullanarak keşfedin.

## <a name="next-steps"></a>Sonraki adımlar
Veri kaynaklarını kaydetme hakkında daha fazla bilgi için bkz. [Azure veri Kataloğu ile çalışmaya başlama](data-catalog-get-started.md) öğretici.
