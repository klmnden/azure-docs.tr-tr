---
title: "Azure veri Kataloğu'nda veri kaynaklarını kaydetme | Microsoft Docs"
description: "Bu makalede, Azure veri Kataloğu, kayıt sırasında ayıklanan meta veri alanları da dahil olmak üzere veri kaynaklarını kaydetme vurgular."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: bab89906-186f-4d35-9ffd-61b1d903905d
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 30166823b33669dda88b41a4aee2dfc34f01466f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="register-data-sources-in-azure-data-catalog"></a>Azure veri Kataloğu'nda veri kaynaklarını kaydetme
## <a name="introduction"></a>Giriş
Azure veri Kataloğu kayıt ve bulma kurumsal veri kaynakları için bir sistem görevi gören bir tam olarak yönetilen bir bulut hizmetidir. Diğer bir deyişle, Bul, anlamak ve veri kaynaklarını kullanan kişilerin veri Kataloğu yardımcı olur ve daha fazla değer, var olan verilerden alma kuruluşlar yardımcı olur. Bir veri kaynağına veri Kataloğu aracılığıyla bulunabilir olmasını ilk adım, bu veri kaynağına kaydetmek için olması önerilir.

## <a name="register-data-sources"></a>Veri kaynaklarını kaydetme
Kayıt veri kaynağından meta verilerin ayıklanması ve bu verileri veri Kataloğu hizmetine kopyalanması işlemidir. Veriler o anda bulunduğu yerde kalır ve geçerli sistemin yöneticilerinin ve ilkelerinin denetiminde olmaya devam eder.

Bir veri kaynağına kaydetmek için aşağıdakileri yapın:
1. Azure veri Kataloğu portalında veri Kataloğu veri kaynağı kayıt aracını başlatın. 
2. İş veya Okul hesabınızı portalında oturum açmak için kullandığınız aynı Azure Active Directory kimlik bilgileriyle oturum açın.
3. Kaydetmek istediğiniz veri kaynağını seçin.

Daha fazla adım adım ayrıntılar için bkz: [Azure veri Kataloğu ile çalışmaya başlama](data-catalog-get-started.md) Öğreticisi.

Veri kaynağı kaydınız sonra katalog konumunu izler ve meta verilerini dizinler. Kullanıcıların arama, göz atın ve veri kaynağını Bul ve buna uygulama veya kendi seçtikleri aracını kullanarak bağlanmak için konumuna kullanın.

## <a name="supported-data-sources"></a>Desteklenen veri kaynakları
Şu anda desteklenen veri kaynaklarının listesi için bkz: [veri Kataloğu DSR](data-catalog-dsr.md).

## <a name="structural-metadata"></a>Yapısal meta verileri
Bir veri kaynağını kaydettiğinizde, kayıt aracı seçtiğiniz nesnelerin yapısı hakkında bilgi ayıklar. Bu bilgiler yapısal meta verilerin adlandırılır.

Tüm nesneler için bu yapısal meta verilerin nesnenin konumunu içerir verileri Bul kullanıcılar kendi seçtikleri istemci araçlarında nesnesine bağlamak için bu bilgileri kullanabilir. Nesne adı ve türü diğer yapısal meta verileri içerir ve öznitelik/sütun adı ve veri türü.

## <a name="descriptive-metadata"></a>Açıklayıcı meta verileri
Veri kaynağından ayıklanan çekirdek yapısal meta verilerin yanı sıra, veri kaynağı kayıt aracını açıklayıcı meta verileri ayıklar. SQL Server Analysis Services ve SQL Server Reporting Services için bu hizmetleri tarafından sunulan açıklama özellikleri bu meta veriler alınır. SQL Server, ms kullanarak sağlanan değerler için\_genişletilmiş özellik açıklama ayıklanır. Oracle veritabanı için veri kaynağı kayıt aracını açıklamalar sütununda tüm ayıklar\_sekmesini\_açıklamaları görünümü.

Veri kaynağından ayıklanan açıklayıcı meta verileri ek olarak, kullanıcılar veri kaynağı kayıt aracını kullanarak açıklayıcı meta verileri girebilirsiniz. Kullanıcılar etiketleri ekleyebilir ve Kaydedilmekte nesneler için uzmanlar tanımlayabilirsiniz. Bu tanımlayıcı meta veri Kataloğu hizmetinin yapısal meta verilerin yanı sıra kopyalanır.

## <a name="include-previews"></a>Önizlemeler içerir
Varsayılan olarak, yalnızca meta veri kaynaklarından ayıklanan ve veri Kataloğu hizmet ancak içerdiği verilerin bir örnek görüntülediğinizde, bir veri kaynağı genellikle kolaylaştırılır anlama kopyalanır.

Veri Kataloğu veri kaynağı kayıt aracını kullanarak, verilerin bir anlık görüntü önizlemesini her tablo ve kayıtlı görünüm içerebilir. Kayıt sırasında önizlemeleri eklemeyi seçerseniz, kayıt aracı her tablo ve görünüm en fazla 20 kayıt içerir. Bu anlık görüntü sonra yapısal ve açıklayıcı meta verileri birlikte kataloğa kopyalanır.

> [!NOTE]
> Çok sayıda sütun geniş tablolarla 20'den az kayıtları kendi Önizleme'de dahil olabilir.
>
>

## <a name="include-data-profiles"></a>Veri profiller içerir
Dahil olmak üzere önizlemeleri veri Kataloğu veri kaynaklarında arama kullanıcılar için değerli bağlamı yalnızca sağlayabilir gibi bir veri profili dahil olmak üzere, bulunan veri kaynaklarını anlamasına olanak kolaylaştırabilir.

Veri Kataloğu veri kaynağı kayıt aracını kullanarak, her bir tablo ve kayıtlı görünüm için bir veri profili içerebilir. Kayıt sırasında veri profili Ekle seçerseniz, kayıt aracı veri ilgili toplu istatistikler her tablo ve görünüm içeren dahil olmak üzere:

* Satır ve nesnesindeki verilerin boyutunu sayısı.
* Veri ve nesne şemasının en son güncelleştirme tarihi.
* Boş kayıtlar ve sütunlar için farklı değerleri sayısı.
* Sütunlar için minimum, maksimum, ortalama ve standart sapma değerleri.

Bu istatistikler sonra yapısal ve açıklayıcı meta verileri birlikte kataloğa kopyalanır.

> [!NOTE]
> Metin ve tarih sütunlarını ortalama veya standart sapma istatistikleri kendi veri profili dahil etmeyin.
>
>

## <a name="update-registrations"></a>Güncelleştirme kayıtları
Meta veri ve isteğe bağlı Önizleme kayıt sırasında ayıklanan kullandığınızda bir veri kaynağı kaydetme veri Kataloğu'nda bulunabilir kolaylaştırır. Veri kaynağı (örneğin, bir nesne şema değişti, başlangıçta dışlanan tabloları dahil edilecek veya önizlemelerde dahil verileri güncelleştirmek istediğiniz varsa) katalogdaki güncelleştirilmesi gerekiyorsa, veri kaynağı kayıt aracını yeniden çalıştırabilirsiniz.

Zaten kayıtlı veri kaynağı yeniden kaydetme birleştirme "upsert" işlemi gerçekleştirir: var olan nesneleri güncelleştirilir ve yeni nesneler oluşturulur. Veri Kataloğu portalı yoluyla kullanıcılar tarafından sağlanan herhangi bir meta veri korunur.

## <a name="summary"></a>Özet
Bu yapısal ve açıklayıcı meta verileri veri kaynağından Kataloğu hizmetine kopyalar olduğundan, veri Kataloğu'nda veri kaynağı kaydetme veri bulunmasını ve anlaşılmasını kolaylaştırır. Veri kaynağı kaydettikten sonra açıklama, yönetmek ve veri Kataloğu portalını kullanarak keşfedin.

## <a name="next-steps"></a>Sonraki adımlar
Veri kaynaklarını kaydetme hakkında daha fazla bilgi için bkz: [Azure veri Kataloğu ile çalışmaya başlama](data-catalog-get-started.md) Öğreticisi.
