---
title: Azure veri Kataloğu'nda veri kaynaklarına bağlanma
description: Azure veri Kataloğu ile bulunan veri kaynaklarına bağlanmak nasıl yapılır makalesi.
services: data-catalog
author: JasonWHowell
ms.author: jasonh
ms.assetid: 4e6b27a5-cf75-4012-b88c-333c1fe638e8
ms.service: data-catalog
ms.topic: conceptual
ms.date: 01/18/2018
ms.openlocfilehash: c64340491dba11870364610a6c2ff62e25c1328a
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60000581"
---
# <a name="how-to-connect-to-data-sources"></a>Veri kaynaklarına bağlanma
## <a name="introduction"></a>Giriş
**Microsoft Azure veri Kataloğu** kayıt ve kurumsal veri kaynakları için bulma sistemi olarak görev yapan tam yönetilen bir bulut hizmetidir. Diğer bir deyişle, **Azure veri Kataloğu** keşfedin, anlamak ve veri kaynaklarını kullanan kişilerin yardımcı olur ve var olan verilerden daha fazla değer elde yardımcı İşte. Bu senaryonun temel bir yönü, verileri kullanarak bir kullanıcı bir veri kaynağını bulan ve amacı anlayan sonra kullanmak için kendi verilerini yerleştirmek için veri kaynağına bağlanmak için sonraki adım içerir.

## <a name="data-source-locations"></a>Veri kaynağı konumları
Veri kaynağı kaydı sırasında **Azure veri Kataloğu** veri kaynağıyla ilgili meta verilerini alır. Bu meta veriler veri kaynağının konumu ile ilgili ayrıntıları içerir. Konumun ayrıntılarını veri kaynağından veri kaynağına farklılık gösterir ancak bu her zaman bağlanmak için gereken bilgileri içerir. Örneğin, sunucu adını ve yolunu rapor için bir SQL Server Reporting Services raporunun konumu içerse de bir SQL Server tablo konumunu sunucu adı, veritabanı adı, şema adı ve tablo adı içerir. Diğer veri kaynağı türleri yapısı ve kaynak sistemi özelliklerini yansıtan konumları sahip olur.

## <a name="integrated-client-tools"></a>Tümleşik istemci araçlarında
En basit yolu, bir veri kaynağına bağlanmak için kullanılacak olan "içinde Aç. …" menüde **Azure veri Kataloğu** portalı. Bu menü, seçili veri varlığına bağlanma seçeneklerin bir listesini görüntüler.
Varsayılan döşeme görünümünü kullanırken, her bir kutucuğundaki bu menüde kullanılabilir.

 ![Bir SQL Server tablo, veri varlığı kutucuğundan Excel'de açma](./media/data-catalog-how-to-connect/data-catalog-how-to-connect1.png)

Liste görünümünü kullanırken, menü portal penceresinin üst kısmındaki arama çubuğunda kullanılabilir.

 ![Bir SQL Server Reporting Services rapor arama Rapor Yöneticisi'nde açma](./media/data-catalog-how-to-connect/data-catalog-how-to-connect2.png)

## <a name="supported-client-applications"></a>Desteklenen istemci uygulamaları
Kullanırken "içinde Aç. …" İstemci bilgisayarda doğru istemci uygulamasının Azure veri Kataloğu Portalı'nda veri kaynakları için menü yüklenmesi gerekir.

| Uygulamada Aç | Dosya uzantısı / protokolünü | Desteklenen uygulama sürümleri |
| --- | --- | --- |
| Excel |.odc |Excel 2010 veya üzeri |
| Excel (ilk 1000) |.odc |Excel 2010 veya üzeri |
| Power Query |.xlsx |Excel 2016 veya Excel 2010 veya Excel eklentisi için Power Query ile Excel 2013 yüklü |
| Power BI Desktop |.pbix |Power BI Desktop Temmuz 2016 veya sonraki sürümleri |
| SQL Server Veri Araçları |vsweb:// |Visual Studio 2013 Update 4 veya üzeri yüklü olan SQL Server araçları ile |
| Rapor Yöneticisi |http:// |Bkz: [SQL Server Reporting Services için tarayıcı gereksinimleri](https://technet.microsoft.com/library/ms156511.aspx) |

## <a name="your-data-your-tools"></a>Verileriniz, sizin araçlarınız
Menüsünde kullanılabilir seçenekleri, veri varlığı şu anda seçili türüne bağlıdır. Tüm olası araçları dahil edilecektir, "içinde Aç. …" Menü, ancak herhangi bir istemci aracı kullanarak veri kaynağına bağlanmak yine de kolay değildir. İçinde bir veri varlığına seçildiğinde **Azure veri Kataloğu** portal, tam yolunu Özellikler bölmesinde da görüntülenir.

 ![Bağlantı bilgilerini bir SQL Server tablosu](./media/data-catalog-how-to-connect/data-catalog-how-to-connect3.png)

Veri kaynağı türü için bağlantı bilgilerini ayrıntılarını veri kaynak türünden farklılık gösterir, ancak portalda bilgileri, herhangi bir istemci aracında veri kaynağına bağlanmak için gereken her şeyi size sunar. Kullanıcılar, kullanılarak bulunan veri kaynakları için bağlantı ayrıntıları kopyalayabilir **Azure veri Kataloğu**, bunları kendi aracıyla ilgili verilerle çalışacak şekilde etkinleştirme.

## <a name="connecting-and-data-source-permissions"></a>Bağlanma ve veri kaynağı izinleri
Ancak **Azure veri Kataloğu** veri kaynakları bulunabilir, erişim sağlar verileri kendi veri kaynak sahibinin veya yöneticisinin denetimi altında kalır. Bir veri kaynağı bulma **Azure veri Kataloğu** bir kullanıcının veri kaynağına erişmek için herhangi bir izin vermez.

Bir veri kaynağı ancak kendi verilerine erişim izni olmayan kullanıcılar için kolaylaştırmak için kullanıcılar erişim isteği özelliği bir veri kaynağı yorumlama durumlarda sağlayabilir. Burada da dahil olmak üzere işlem ya da veri kaynağı erişim kazanmak için bir iletişim noktası bağlantılar – sağlanan bilgiler, Portalı'nda veri kaynağı konum bilgileri ile birlikte sunulur.

 ![Bağlantı bilgilerini kullanarak sağlanan istek erişim yönergeleri](./media/data-catalog-how-to-connect/data-catalog-how-to-connect4.png)

## <a name="summary"></a>Özet
Bir veri kaynağı ile kaydetme **Azure veri Kataloğu** yapısal ve açıklayıcı meta veri Kataloğu hizmetine veri kaynağından kopyalayarak bu verileri bulunabilir hale getirir. Bir veri kaynağı kaydedildi, bulunan ve sonra kullanıcılar veri kaynağından bağlanabilir **Azure veri Kataloğu** portalı "içinde Aç..." " menü veya tercih ettiğiniz veri araçlarını kullanarak.

## <a name="see-also"></a>Ayrıca bkz.
* [Azure veri Kataloğu ile çalışmaya başlama](data-catalog-get-started.md) veri kaynaklarına bağlanmak nasıl hakkında bilgi için adım adım öğretici.
