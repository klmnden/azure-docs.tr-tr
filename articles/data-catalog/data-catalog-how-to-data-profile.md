---
title: Azure veri Kataloğu veri kaynakları profil oluşturma verilerini kullanma
description: Nasıl yapılır makalesi Azure veri Kataloğu'nda veri kaynaklarını kaydetme, tablo ve sütun düzeyinde veri profiller ekleme ve veri kaynaklarını anlamasına olanak veri profilleri kullanma vurgulama.
services: data-catalog
author: JasonWHowell
ms.author: jasonh
ms.assetid: 94a8274b-5c9c-4962-a4b1-2fed38a3d919
ms.service: data-catalog
ms.topic: conceptual
ms.date: 01/18/2018
ms.openlocfilehash: 64185a951b25b4e04ea5fc65aeede9b0e617d0c5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61001729"
---
# <a name="data-profile-data-sources"></a>Veri profili veri kaynakları
## <a name="introduction"></a>Giriş
**Microsoft Azure veri Kataloğu** kayıt ve kurumsal veri kaynakları için bulma sistemi olarak görev yapan tam yönetilen bir bulut hizmetidir. Diğer bir deyişle, **Azure veri Kataloğu** keşfedin, anlamak ve veri kaynaklarını kullanan kişilerin yardımcı olur ve var olan verilerden daha fazla değer elde yardımcı İşte. Ne zaman bir veri kaynağı kayıtlı ile **Azure veri Kataloğu**, meta verileri kopyalanır ve hizmet tarafından ancak bir hikayesi vardır sonlanmıyor.

**Profil oluşturma verilerini** özelliği **Azure veri Kataloğu** kataloğunuzdaki desteklenen veri kaynaklarından alınan verileri inceler ve istatistikleri ve bu verileri hakkında bilgi toplar. Bir profili, veri varlıklarınızı eklemek kolaydır. Bir veri varlığına kaydettiğinizde seçin **veri profili Ekle** veri kaynağı Kayıt Aracı'nda.

## <a name="what-is-data-profiling"></a>Profil oluşturma verilerini nedir
Profil oluşturma verilerini kayıtlı veri kaynağındaki verileri inceler ve istatistikleri ve bu verileri hakkında bilgi toplar. Veri kaynağı bulma sırasında bu İstatistikler verilerin iş sorunlarını çözmek için uygun olup olmadığını belirlemenize yardımcı olabilir.

<!-- In [How to discover data sources](data-catalog-how-to-discover.md), you learn about **Azure Data Catalog's** extensive search capabilities including searching for data assets that have a profile. See [How to include a data profile when registering a data source](#howto). -->

Profil oluşturma verilerini aşağıdaki veri kaynaklarını destekler:

* SQL Server'ın (Azure SQL DB ile Azure SQL veri ambarı dahil) tabloları ve görünümleri
* Oracle tabloları ve görünümleri
* Teradata tabloları ve görünümleri
* Hive tablolarını

Veri varlıklarını kaydetme, kullanıcıların yardımcı olur dahil olmak üzere veri profilleri gibi veri kaynakları hakkında sorularını yanıtlayın:

* My iş sorununu çözmek için kullanılabilir mi?
* Verileri belirli standartlara veya desenleri için uygun mu?
* Veri kaynağının anomalileri bazıları nelerdir?
* Bu veriler my uygulamayla tümleştirmek olası sorunlarından nelerdir?

> [!NOTE]
> Verileri bir uygulamaya nasıl tümleşik açıklamak için bir varlığa belgeleri de ekleyebilirsiniz. Bkz: [veri kaynaklarını belgeleme](data-catalog-how-to-documentation.md).
>
>

<a name="howto"/>

## <a name="how-to-include-a-data-profile-when-registering-a-data-source"></a>Data source kaydedilirken veri profili ekleme
Veri kaynağınızın bir profil eklemek kolaydır. İçinde bir veri kaynağını kaydettiğinizde **kaydedilecek nesneler** paneli veri kaynağı Kayıt Aracı ' seçin **veri profili Ekle**.

![](media/data-catalog-data-profile/data-catalog-register-profile.png)

Veri kaynaklarını kaydetme hakkında daha fazla bilgi için bkz: [veri kaynaklarını kaydetme](data-catalog-how-to-register.md) ve [Azure veri Kataloğu ile çalışmaya başlama](data-catalog-get-started.md).

## <a name="filtering-on-data-assets-that-include-data-profiles"></a>Veri profillerini içeren veri varlıklarını üzerinde filtreleme
Veri profili Ekle veri varlıklarını bulmak için dahil edebileceğiniz `has:tableDataProfiles` veya `has:columnsDataProfiles` arama terimlerinizi biri olarak.

> [!NOTE]
> Seçme **veri profili Ekle** veri kaynağında kayıt aracı hem tablo ve sütun düzeyinde profil bilgilerini içerir. Ancak, yalnızca bir profil bilgilerini dahil kümesiyle kaydedilecek veri varlıklarını veri Kataloğu API'si sağlar.
>
>

## <a name="viewing-data-profile-information"></a>Veri profili bilgilerini görüntüleme
Bir profili bir uygun veri kaynağıyla bulduktan sonra veri profil ayrıntılarını görüntüleyebilirsiniz. Veri profili görüntülemek için bir veri varlığına seçip **veri profili** veri Kataloğu portalı penceresinde.

![](media/data-catalog-data-profile/data-catalog-view.png)

Bir veri profilinde **Azure veri Kataloğu** tablo ve sütun profilini bilgiler dahil olmak üzere gösterir:

### <a name="object-data-profile"></a>Nesne veri profili
* Satır sayısı
* Tablo boyutu
* Nesnenin son güncelleştirildiği

### <a name="column-data-profile"></a>Sütun veri profili
* Sütun veri türü
* Benzersiz değer sayısı
* NULL değerlerle satır sayısı
* Minimum, maksimum, ortalama ve sütun değerleri için standart sapma

## <a name="summary"></a>Özet
Profil oluşturma veri istatistikleri ve verilerin iş sorunlarını çözmek için uygun olup olmadığını belirlemenize yardımcı olmak için kayıtlı veri varlıklarını hakkında bilgi sağlar. Not eklemek ve veri kaynaklarını belgeleme yanı sıra veri profilleri kullanıcı verilerinizi daha derin bir anlayış verebilirsiniz.

## <a name="see-also"></a>Ayrıca Bkz.
* [Veri kaynaklarını kaydetme](data-catalog-how-to-register.md)
* [Azure Veri Kataloğu ile çalışmaya başlama](data-catalog-get-started.md)
