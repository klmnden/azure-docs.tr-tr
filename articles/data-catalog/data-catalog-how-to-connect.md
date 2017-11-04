---
title: "Veri kaynaklarına bağlanma | Microsoft Docs"
description: "Nasıl yapılır makalesi Azure veri Kataloğu ile bulunan veri kaynaklarına bağlanmak nasıl vurgulama."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 4e6b27a5-cf75-4012-b88c-333c1fe638e8
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 11/01/2017
ms.author: maroche
ms.openlocfilehash: 8176a952107a630d42d557e568a230f1cdc840aa
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="how-to-connect-to-data-sources"></a>Veri kaynaklarına bağlanma
## <a name="introduction"></a>Giriş
**Microsoft Azure veri Kataloğu** bir kayıt ve sistemi kurumsal veri kaynakları için bulma görevi gören bir tam olarak yönetilen bir bulut hizmetidir. Diğer bir deyişle, **Azure veri Kataloğu** tüm bulmak, anlamak ve veri kaynaklarını kullanan kişilerin ve bunların var olan verilerden daha fazla değer almak için kuruluşlar hakkındadır. Temel bir yönü bu senaryo, veri – kullanarak bir kullanıcı bir veri kaynağı bulur ve amacı anlar sonra kullanmak için verileri yerleştirmek için veri kaynağına bağlanmak için sonraki adım içerir.

## <a name="data-source-locations"></a>Veri kaynağı konumları
Veri kaynağı kaydı sırasında **Azure veri Kataloğu** veri kaynağı ile ilgili meta verileri alır. Bu meta veriler veri kaynağının konumu ayrıntılarını içerir. Konumun ayrıntılarını veri kaynağından veri kaynağına farklılık gösterir, ancak bu her zaman bağlanmak için gereken bilgileri içerir. Örneğin, sunucu adını ve yolunu rapora bir SQL Server Reporting Services raporunun konumunu içerir ancak bir SQL Server tablosu için konum sunucu adı, veritabanı adı, şema adı ve tablo adı içerir. Diğer veri kaynağı türleri yapısı ve kaynak sistemi özelliklerini yansıtan konumları sahip olur.

## <a name="integrated-client-tools"></a>Tümleşik istemci araçlarında
Bir veri kaynağına bağlanmak için en basit yolu kullanmaktır "içinde Aç..." menüde **Azure veri Kataloğu** portal. Bu menüsü seçili veri varlığına bağlanma seçeneklerin bir listesini görüntüler.
Varsayılan döşeme görünümünü kullanırken, bu menü her bölme üzerinde kullanılabilir.

 ![Bir SQL Server tablo Excel'de veri varlık kutucuğunda açma](./media/data-catalog-how-to-connect/data-catalog-how-to-connect1.png)

Liste görünümünü kullanırken arama çubuğunda portal pencerenin üstündeki menü kullanılabilir.

 ![SQL Server Reporting Services raporunun Rapor Yöneticisi'nde arama çubuğunda açma](./media/data-catalog-how-to-connect/data-catalog-how-to-connect2.png)

## <a name="supported-client-applications"></a>Desteklenen istemci uygulamaları
Kullanırken, "içinde Aç..." Menü veri kaynakları Azure veri Kataloğu portalında doğru istemci uygulaması için istemci bilgisayara yüklenmesi gerekir.

| Uygulama Aç | Dosya uzantısı / Protokolü | Desteklenen uygulama sürümleri |
| --- | --- | --- |
| Excel |.odc |Excel 2010 veya üzeri |
| Excel (ilk 1000) |.odc |Excel 2010 veya üzeri |
| Power Query |.xlsx |Excel 2016 veya Excel 2010 veya Excel eklentisi için Power Query ile Excel 2013 yüklü |
| Power BI Desktop |.pbix |Power BI Desktop Temmuz 2016 veya sonraki |
| SQL Server Veri Araçları |vsweb: / / |Visual Studio 2013 güncelleştirme 4 veya üstü yüklü SQL Server araçları ile |
| Rapor Yöneticisi |http:// |Bkz: [SQL Server Reporting Services için tarayıcı gereksinimleri](https://technet.microsoft.com/en-us/library/ms156511.aspx) |

## <a name="your-data-your-tools"></a>Araçlarınızı verilerinizi
Menüsünde kullanılabilir seçenekleri şu anda seçili veri varlık türüne bağlıdır. Tüm olası araçları dahil edilecek doğal olarak, "içinde Aç..." Menü, ancak herhangi bir istemci araç kullanarak veri kaynağına bağlanmak hala kolay. İçinde bir veri varlığına seçildiğinde **Azure veri Kataloğu** portal, tam konum Özellikler bölmesinde da görüntülenir.

 ![Bağlantı bilgilerini bir SQL Server tablosu](./media/data-catalog-how-to-connect/data-catalog-how-to-connect3.png)

Veri kaynağı türü için bağlantı bilgileri ayrıntıları veri kaynağı türünden farklılık gösterir, ancak portalda eklenen bilgileri size herhangi bir istemci aracında veri kaynağına bağlanmak için gereken her şeyi sağlar. Kullanıcılar, kullanılarak bulunan veri kaynakları için bağlantı ayrıntıları kopyalayabilir **Azure veri Kataloğu**, bunları kendi aracıyla ilgili verilerle çalışmak etkinleştirme.

## <a name="connecting-and-data-source-permissions"></a>Bağlanma ve veri kaynağı izinleri
Rağmen **Azure veri Kataloğu** bulunabilir, access veri kaynaklarını yapar veriler için kendi veri kaynağına sahip veya yönetici denetimi altında kalır. Bir veri kaynağı bulma **Azure veri Kataloğu** bir kullanıcının veri kaynağına erişmek için tüm izinleri vermez.

Bir veri kaynağını Bul ancak kendi verilerine erişim izni yoksa kullanıcılar için kolaylaştırmak için kullanıcılar istek erişimi özelliği bir veri kaynağına açıklama durumlarda sağlayabilir. – Dahil olmak üzere işlem ya da veri kaynağına erişim kazanmak için iletişim noktası bağlantılar – burada sağlanan bilgiler, Portalı'nda veri kaynağı konumu bilgileri yanında sunulur.

 ![Sağlanan istek erişim yönergeleri ile bağlantı bilgileri](./media/data-catalog-how-to-connect/data-catalog-how-to-connect4.png)

## <a name="summary"></a>Özet
Veri kaynağı ile kaydetme **Azure veri Kataloğu** yapısal ve açıklayıcı meta veri kaynağından Kataloğu hizmetine kopyalayarak verileri bulunabilir hale getirir. Bir veri kaynağı kaydedildi, bulunan ve sonra kullanıcılar veri kaynağından bağlanabilir **Azure veri Kataloğu** portalı "içinde Aç..." " menü veya tercih ettiğiniz kendi veri Araçları'nı kullanarak.

## <a name="see-also"></a>Ayrıca bkz.
* [Azure veri Kataloğu ile çalışmaya başlama](data-catalog-get-started.md) veri kaynaklarına bağlanma hakkında adım adım ayrıntılar için Öğreticisi.
