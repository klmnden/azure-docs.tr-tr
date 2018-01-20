---
title: "Nasıl yapılır veri profil veri kaynakları"
description: "Nasıl yapılır makalesi vurgulama Azure veri Kataloğu'nda veri kaynaklarını kaydederek tablo ve sütun düzeyi veri profilleri dahil etme ve veri kaynaklarını anlaşılması veri profillerini kullanın."
services: data-catalog
documentationcenter: 
author: spelluru
manager: NA
editor: 
tags: 
ms.assetid: 94a8274b-5c9c-4962-a4b1-2fed38a3d919
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 01/18/2018
ms.author: spelluru
ms.openlocfilehash: 80181b729ffa6d6cbc2d17fe8a5ae8ee4e3d41ab
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="data-profile-data-sources"></a>Veri profili veri kaynakları
## <a name="introduction"></a>Giriş
**Microsoft Azure veri Kataloğu** bir kayıt ve sistemi kurumsal veri kaynakları için bulma görevi gören bir tam olarak yönetilen bir bulut hizmetidir. Diğer bir deyişle, **Azure veri Kataloğu** tüm bulmak, anlamak ve veri kaynaklarını kullanan kişilerin ve bunların var olan verilerden daha fazla değer almak için kuruluşlar hakkındadır. Ne zaman bir veri kaynağı kayıtlı ile **Azure veri Kataloğu**, meta verilerini kopyalanır ve hizmet tarafından dizine ancak Öykü yok sonlanmıyor.

**Profil oluşturma verilerini** özelliği **Azure veri Kataloğu** kataloğunuzu desteklenen veri kaynaklarından veri inceler ve istatistikleri ve bu verileri hakkında bilgiler toplar. Veri varlıklarınız profilini dahil kolaydır. Bir veri varlığına kaydederken seçin **dahil veri profili** veri kaynağı kayıt aracında.

## <a name="what-is-data-profiling"></a>Profil oluşturma verilerini nedir
Profil oluşturma verilerini Kaydedilmekte veri kaynağındaki verileri inceler ve istatistikleri ve bu verileri hakkında bilgiler toplar. Veri kaynağı bulma sırasında bu İstatistikler verilerin iş sorunlarını çözmek için uygunluğunu belirlemenize yardımcı olabilir.

<!-- In [How to discover data sources](data-catalog-how-to-discover.md), you learn about **Azure Data Catalog's** extensive search capabilities including searching for data assets that have a profile. See [How to include a data profile when registering a data source](#howto). -->

Profil oluşturma verilerini aşağıdaki veri kaynaklarını destekler:

* SQL Server'ın (Azure SQL DB ve Azure SQL Data Warehouse dahil) tabloları ve görünümleri
* Oracle tabloları ve görünümleri
* Teradata tabloları ve görünümleri
* Hive tabloları

Veri varlıklarını kaydetme kullanıcılar yardımcı olduğunda dahil olmak üzere veri profilleri de dahil olmak üzere, veri kaynakları ile ilgili soruları yanıtlayın:

* My iş sorununu çözmek için kullanılabilir mi?
* Verileri, belirli standartlara veya desenler için uygun mu?
* Bazı veri kaynağının anormallikleri nelerdir?
* Bu veriler my uygulamasına tümleştirme olası zorluklar nelerdir?

> [!NOTE]
> Verileri bir uygulamaya nasıl tümleşik tanımlamak için bir varlığı için belgeleri de ekleyebilirsiniz. Bkz: [veri kaynaklarını belgeleme](data-catalog-how-to-documentation.md).
>
>

<a name="howto"/>

## <a name="how-to-include-a-data-profile-when-registering-a-data-source"></a>Data source kaydedilirken bir veri profili ekleme
Bir profili, veri kaynağınızın dahil kolaydır. İçinde bir veri kaynağını kaydettiğinizde **kaydedilecek nesneler** Masası veri kaynağı kayıt aracını seçin **dahil veri profili**.

![](media/data-catalog-data-profile/data-catalog-register-profile.png)

Veri kaynaklarını kaydetme hakkında daha fazla bilgi için bkz: [veri kaynaklarını kaydetme](data-catalog-how-to-register.md) ve [Azure veri Kataloğu ile çalışmaya başlama](data-catalog-get-started.md).

## <a name="filtering-on-data-assets-that-include-data-profiles"></a>Veri profillerini içeren veri varlıklarını üzerinde filtreleme
Veri profili Ekle veri varlıklarını bulmak için dahil edebileceğiniz `has:tableDataProfiles` veya `has:columnsDataProfiles` arama terimleri biri olarak.

> [!NOTE]
> Seçme **dahil veri profili** veri kaynağında tablo ve sütun düzeyi profil bilgilerini kayıt aracı içerir. Ancak, veri Kataloğu API'si veri varlıklarını eklenen profili bilgileri yalnızca bir kümesi kaydedilmesine izin verir.
>
>

## <a name="viewing-data-profile-information"></a>Veri profili bilgilerini görüntüleme
Bir profili uygun veri kaynağıyla bulduktan sonra veri profili ayrıntıları görüntüleyebilirsiniz. Veri profilini görüntülemek için bir veri varlığı seçin ve Seç **veri profili** veri Kataloğu portalı penceresinde.

![](media/data-catalog-data-profile/data-catalog-view.png)

Bir veri profilinde **Azure veri Kataloğu** tablo ve sütun profil bilgileri de dahil olmak üzere gösterir:

### <a name="object-data-profile"></a>Nesne veri profili
* Satır sayısı
* Tablo boyutu
* Nesnenin son güncelleştirildiği

### <a name="column-data-profile"></a>Sütun veri profili
* Sütun veri türü
* Farklı değerleri sayısı
* NULL değerlere sahip satır sayısı
* Minimum, maksimum, ortalama ve sütun değerleri için standart sapma

## <a name="summary"></a>Özet
Profil oluşturma veri istatistikleri ve verilerin iş sorunlarını çözmek için uygunluğunu belirlemenize yardımcı olmak için kayıtlı veri varlıklarını hakkında bilgi sağlar. Açıklama ve veri kaynaklarını belgeleme yanı sıra veri profilleri, verilerinizin daha derin bir anlayış kullanıcılara verebilirsiniz.

## <a name="see-also"></a>Ayrıca Bkz.
* [Veri kaynaklarını kaydetme](data-catalog-how-to-register.md)
* [Azure Veri Kataloğu ile çalışmaya başlama](data-catalog-get-started.md)
