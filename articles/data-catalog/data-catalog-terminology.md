---
title: Azure veri Kataloğu terminolojisi
description: Bu makalede, kavramlar ve terimler Azure veri Kataloğu belgelerde kullanılan bir giriş sağlar.
author: JasonWHowell
ms.author: jasonh
ms.service: data-catalog
ms.topic: conceptual
ms.date: 04/05/2019
ms.openlocfilehash: a6f2cf1dcee6a85376c8d767e57c504b6b246e5d
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60011393"
---
# <a name="azure-data-catalog-terminology"></a>Azure veri Kataloğu terminolojisi

Bu makalede, kavramlar ve terimler Azure veri Kataloğu belgelerde kullanılan bir giriş sağlar.

## <a name="catalog"></a>Katalog

Azure veri Kataloğu, veri kaynakları ve veri varlıklarını kaydedilebilir meta verileri bulut tabanlı bir depodur. Kataloğu, veri kaynağından ayıklanan yapısal meta verilere ve kullanıcılar tarafından eklenen açıklayıcı meta veri merkezi depolama konumu olarak görev yapar.

## <a name="data-source"></a>Veri kaynağı

Bir veri kaynağı, bir sistem veya veri varlıklarını yöneten kapsayıcı ' dir. SQL Server veritabanları, Oracle veritabanları, SQL Server Analysis Services veritabanları (tablosal veya çok boyutlu) ve SQL Server Reporting Services sunucularında örneklerindendir.

## <a name="data-asset"></a>Veri varlığı

Veri varlıklarını kataloğa kayıtlı veri kaynaklarını içindeki nesneleridir. SQL Server Reporting Services raporları ve SQL Server tabloları ve görünümleri, Oracle tabloları ve görünümleri, SQL Server Analysis Services ölçüler, boyutlar ve KPI'leri örnekleri içerir.

## <a name="data-asset-location"></a>Veri varlığı konumu

Kataloğu veri kaynağı veya bir istemci uygulaması kullanarak kaynağına bağlanmak için kullanılan veri varlığına konumunu saklar. Biçimi ve konumu ayrıntılarını veri kaynağı türüne göre değişir. Örneğin, bir SQL Server Raporlama Hizmetleri rapor URL'sini tarafından tanımlanabilir sırada bir SQL Server tablo Dört bölümlü adıyla – sunucu adı, veritabanı adı, şema adı, nesne adı – tanımlanabilir.

## <a name="structural-metadata"></a>Yapısal meta verileri

Yapısal meta verilerin bir veri varlığına yapısını açıklayan bir veri kaynağından ayıklanan meta verileri ' dir. Bu varlıklar konumu, nesne adı ve türü ve ek türe özgü özelliklerini içerir. Örneğin, adları ve veri türleri nesnenin sütunları için tabloları ve görünümleri yapısal meta verileri içerir.

## <a name="descriptive-metadata"></a>Açıklayıcı meta verileri

Açıklayıcı meta verileri, amacını ve amacı, bir veri varlığı tanımlayan meta verilerdir. Açıklayıcı meta verileri Azure veri Kataloğu portalını kullanarak katalog kullanıcıları tarafından genellikle eklenir, ancak bunu ayrıca veri kaynağından sırasında kayıt ayıklanabileceği. Azure veri Kataloğu kayıt aracı SQL Server Analysis Services ve SQL Server Reporting Services ve gelen açıklama özelliğinden açıklamaları gibi ayıklayacaktır [genişletilmiş özelliği ms_description](https://technet.microsoft.com/library/ms190243.aspx) SQL Bu özellikler değerlerle doldurulduğunu, server veritabanları.

## <a name="request-access"></a>Erişim izni iste

Bir veri varlığına ait açıklayıcı meta verileri, veri varlığı veya veri kaynağına erişim isteme konusunda bilgi içerebilir. Bu bilgiler, veri varlığı konumu ile sunulan ve bir veya daha fazla aşağıdaki seçeneklerden birini içerebilir:

* Bir kullanıcı veya ekibe veri kaynağına erişim hakkı vermekten sorumlu e-posta adresi.
* Kullanıcılar veri kaynağına erişmek için izlemeniz gereken belgelenmiş işlem URL'si.
* Veri kaynağına erişmek için kullanılan bir kimlik ve erişim yönetim aracı (Microsoft Identity Manager gibi) URL'si.
* Kullanıcılar veri kaynağına erişim elde edebilir nasıl açıklar serbest metin girişi.

## <a name="preview"></a>Önizleme

Bir Azure veri Kataloğu kayıt sırasında veri kaynağından ayıklanan ve veri varlık meta verilerle kataloğunda depolanan en fazla 20 kayıt anlık görüntüsünü önizlemededir. Önizleme yardımcı olabilecek bir veri varlığı bulma kullanıcılar daha iyi anlamak, işlevi ve amacı. Diğer bir deyişle, örnek verileri görme hakkındaki yalnızca sütun adları ve veri türleri görmeye değerinden daha değerli olabilir.
Önizlemeler, yalnızca tablolar ve görünümler için desteklenir ve kayıt sırasında kullanıcı tarafından açıkça seçilmelidir.

## <a name="data-profile"></a>Veri Profili

Azure veri Kataloğu'nda veri profili, kayıt sırasında veri kaynağından ayıklanan ve veri varlık meta verilerle kataloğunda depolanan bir kayıtlı veri varlığı hakkındaki meta verileri tablo ve sütun düzeyi anlık görüntüsüdür. Veri profili yardımcı olabilecek bir veri varlığı bulma kullanıcılar daha iyi anlamak, işlevi ve amacı. Benzer şekilde önizlemeleri, veri profilleri açıkça kullanıcı tarafından kayıt sırasında seçilmelidir.

> [!NOTE]
> Veri profili ayıklanması büyük tablolar ve görünümler için pahalı bir işlem olabilir ve bir veri kaynağına kaydetmek için gereken zamanı önemli ölçüde artırabilirsiniz.


## <a name="user-perspective"></a>Kullanıcı perspektifi

Azure veri Kataloğu'nda herhangi bir kullanıcı bir kayıtlı veri varlığı için açıklayıcı meta verileri sağlayabilirsiniz. Her kullanıcı verileri ve kullanımına ayrı bir perspektif sahiptir. Örneğin, bir sunucu için sorumlu yönetici, hizmet düzeyi sözleşmesi (SLA) veya yedekleme pencereleri ayrıntılarını sağlayabilir; Veri Görevlisi işletmelere yönelik belgelere bağlantılar verilerinizin desteklediği işlemleri sağlayabilir; ve diğer analistleri için en uygun olan ve olabilen bulmak ve verileri anlamak için gereken bu kullanıcılar için en değerli koşulları açıklamasında Analistin sağlayabilir.

Her biri bu Perspektifleri kendiliğinden değerlidir ve Azure veri Kataloğu ile her kullanıcının tüm kullanıcıların veri ve amacını anlamak için bu bilgiyi kullanabilirsiniz, ancak bunları için anlamlı bilgiler sağlayabilir.

## <a name="expert"></a>Uzman

Uzman bilinçli bir "Uzman" perspektifi için bir veri varlığına sahip olarak tanımlanmış bir kullanıcıdır. Herhangi bir kullanıcı, bir varlık için uzman kendileri veya başka bir kullanıcı ekleyebilirsiniz. Azure veri Kataloğu'nda herhangi bir ek ayrıcalık Uzman listelenmesini BELİRTMEMEKTEDİR; Bu, kullanıcıların bir varlığın açıklayıcı meta verileri gözden geçirirken kullanışlı olması büyük olasılıkla bu Perspektifler kolayca bulmasına olanak tanır.

## <a name="owner"></a>Sahip

Azure veri Kataloğu'nda bir veri varlığına yönetmek için ek ayrıcalıklarına sahip bir kullanıcı sahibidir. Kullanıcılar, kayıtlı veri varlıklarının sahipliğini alabilir ve sahipleri ortak sahip olarak diğer kullanıcıları ekleyebilirsiniz. Daha fazla bilgi için [veri varlıklarını yönetme](data-catalog-how-to-manage.md)  

> [!NOTE]
> Sahipliği ve yönetimi, yalnızca, Azure veri Kataloğu standart sürümü içinde kullanılabilir.

## <a name="registration"></a>Kayıt

Kayıt, veri kaynağından veri varlık meta verilerini ayıklayarak ve Azure veri Kataloğu hizmetine kopyalama işlemidir. Kayıtlı veri varlıklarını ardından ek açıklama ve bulundu.

## <a name="next-steps"></a>Sonraki adımlar

[Hızlı Başlangıç: Bir Azure veri Kataloğu oluşturma](data-catalog-get-started.md) 
