---
title: "Azure veri Kataloğu terminolojisi | Microsoft Docs"
description: "Bu makalede, kavramlar ve terimler Azure veri Kataloğu belgelerde kullanılan bir giriş sağlar."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 6fec74d9-4a3c-4b4b-88ba-cad5ad143331
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 557b529f45c7fbc286b7e1893d4b4688e088ed91
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2017
---
# <a name="azure-data-catalog-terminology"></a>Azure veri Kataloğu terminolojisi
## <a name="catalog"></a>Katalog
Azure veri Kataloğu, veri kaynakları ve veri varlıklarını kaydedilebilir bulut tabanlı meta veri deposudur. Kataloğu, veri kaynağından ayıklanan yapısal meta verileri ve kullanıcılar tarafından eklenen açıklayıcı meta verileri için merkezi depolama konumu olarak görev yapar.

## <a name="data-source"></a>Veri kaynağı
Bir veri kaynağı bir sistem veya veri varlıklarını yönetir kapsayıcı ' dir. Örnekler, SQL Server veritabanları, Oracle veritabanları, (tablo veya çok boyutlu) SQL Server Analysis Services veritabanları ve SQL Server Reporting Services sunucuları içerir.

## <a name="data-asset"></a>Veri varlığı
Veri varlıklarını kataloğa kayıtlı veri kaynaklarına içindeki nesneleridir. SQL Server tabloları ve görünümleri, Oracle tabloları ve görünümleri, SQL Server Analysis Services ölçüler, boyutlar ve KPI'leri örnekler ve SQL Server Reporting Services raporları.

## <a name="data-asset-location"></a>Veri varlık konumu
Kataloğu, veri kaynağı veya bir istemci uygulaması kullanarak kaynağına bağlanmak için kullanılan veri varlığına konumunu depolar. Biçim ve konumu ayrıntılarını veri kaynağı türüne göre değişir. Örneğin, bir SQL Server Raporlama Hizmetleri rapor URL'sini göre tanımlanabilir sırasında bir SQL Server tablo dört bölümü adıyla – sunucu adı, veritabanı adı, şema adı, nesne adı – tanımlanabilir.

## <a name="structural-metadata"></a>Yapısal meta verileri
Yapısal meta verilerin bir veri varlığına yapısını açıklayan bir veri kaynağından ayıklanan meta verilerdir. Bu varlıklar konumu, nesne adı ve türü ve ek türüne özgü özellikleri içerir. Örneğin, adları ve veri türleri nesnenin sütunlar için tabloları ve görünümleri yapısal meta verileri içerir.

## <a name="descriptive-metadata"></a>Açıklayıcı meta verileri
Açıklayıcı meta verileri amacını veya bir veri varlığına amacını açıklayan meta veriler oluşturur. Açıklayıcı meta verileri Azure veri Kataloğu Portalı'nı kullanarak katalog kullanıcıları tarafından genellikle eklendiği, ancak bu ayrıca veri kaynağından kayıt sırasında ayıklanabilir. Örneğin, Azure veri Kataloğu kayıt aracı açıklamaları SQL Server Analysis Services ve SQL Server Reporting Services ve gelen açıklama özelliğinden ayıklar [genişletilmiş özellik ms_description](https://technet.microsoft.com/library/ms190243.aspx) SQL Server veritabanlarında, bu özellikleri değerlerle doldurulduğunu varsa.

## <a name="request-access"></a>Erişim isteği
Bir veri varlığın açıklayıcı meta verileri veri varlığına veya veri kaynağına erişim isteme konusunda bilgiler içerebilir. Bu bilgiler, veri varlık konumu ile sunulan ve bir veya daha fazla aşağıdaki seçeneklerden birini içerebilir:

* Kullanıcı veya veri kaynağına erişim vermek için sorumlu takım e-posta adresi.
* Kullanıcıların veri kaynağına erişmek için izlemeniz gereken belgelenen işlem URL'si.
* Veri kaynağına erişmek için kullanılan bir kimlik ve erişim yönetimi aracını (Microsoft Identity Manager gibi) URL'si.
* Kullanıcılar veri kaynağına erişim nasıl kazanmadan açıklar serbest metin girişi.

## <a name="preview"></a>Önizleme
Azure veri Kataloğu önizlemede kayıt sırasında veri kaynağından ayıklanan ve veri varlık meta verilerle kataloğunda depolanan en fazla 20 kayıt anlık görüntüsüdür. Önizleme yardımcı olabilecek bir veri varlığına Bul kullanıcıları daha iyi anlamak işlevi ve amacı. Diğer bir deyişle, örnek verileri görmesini yalnızca sütun adları ve veri türleri görmesini değerinden daha yararlı olabilir.
Önizleme yalnızca tablo ve görünümler için desteklenir ve kayıt sırasında kullanıcı tarafından açıkça seçilmelidir.

## <a name="data-profile"></a>Veri profili
Azure veri Kataloğu veri profilinde kayıt sırasında veri kaynağından ayıklanan ve veri varlık meta verilerle kataloğunda depolanan bir kayıtlı veri varlığını ilgili tablo ve sütun düzeyi meta verilerin bir anlık görüntüdür. Veri profili yardımcı olabilecek bir veri varlığına Bul kullanıcıları daha iyi anlamak işlevi ve amacı. Benzer şekilde önizlemeler, veri profilleri açıkça kullanıcı tarafından kayıt sırasında seçilmelidir.

> [!NOTE]
> Veri profili ayıklanıyor büyük tabloları ve görünümleri için pahalı bir işlem olabilir ve veri kaynağını kaydetme için gereken zamanı önemli ölçüde artırabilir.
>
>

## <a name="user-perspective"></a>Kullanıcı perspektifi
Azure veri Kataloğu'nda herhangi bir kullanıcı için bir kayıtlı veri varlığına açıklayıcı meta verileri sağlayabilir. Her kullanıcı verileri ve kullanımına ayrı bir perspektif sahiptir. Örneğin, bir sunucu için sorumlu Yöneticisi hizmet düzeyi sözleşmesi (SLA) veya yedekleme pencereleri ayrıntılarını sağlayabilir; bir veri steward verilerinizin desteklediği iş için belgelere bağlantıları işler sağlayabilir; Ayrıca bir analist açıklamasını diğer analistleri için en uygun olan ve olabilen bulmak ve verileri anlamak için ihtiyacınız olan kullanıcılar için en değerli koşullarını sağlayabilir.

Her bu Perspektifleri kendiliğinden değerli ve Azure veri Kataloğu ile her kullanıcının tüm kullanıcıların verileri ve amacı anlamak için bu bilgiyi kullanabilirsiniz, ancak bunları için anlamlı bilgiler sağlayabilir.

## <a name="expert"></a>Uzman
Uzman bilinçli "Uzman" bakış açısı için bir veri varlığına sahip olarak belirlenmiştir kullanıcıdır. Herhangi bir kullanıcı, bir varlık için bir uzman olarak kendilerini veya başka bir kullanıcı ekleyebilirsiniz. Uzmanı listelenmiş herhangi bir ek ayrıcalık Azure veri Kataloğu'nda iletmek değil; Kullanıcıların bir varlığın açıklayıcı meta verileri gözden geçirirken kullanışlı olması büyük olasılıkla bu Perspektifler kolayca bulmasına olanak sağlar.

## <a name="owner"></a>Sahip
Azure veri Kataloğu'ndaki bir veri varlığına yönetmek için ek ayrıcalıklarına sahip bir kullanıcı sahibidir. Kullanıcılar, kayıtlı veri varlıklarının sahipliğini alabilir ve sahipleri ikincil sahip diğer kullanıcılar ekleyebilirsiniz. Daha fazla bilgi için bkz: [veri varlıklarını yönetme](data-catalog-how-to-manage.md)  

> [!NOTE]
> Sahipliği ve yönetimi, yalnızca, Azure veri Kataloğu standart sürümü kullanılabilir.
>
>

## <a name="registration"></a>Kayıt
Kayıt bir veri kaynağından veri varlık meta veri çıkarılıyor ve Azure veri Kataloğu hizmetine kopyalanması işlemidir. Kayıtlı veri varlıklarını sonra açıklama bulunan ve.

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Veri Kataloğu nedir?](data-catalog-what-is-data-catalog.md) -Bu makalede Azure veri Kataloğu hizmeti, sağladığı değer ve desteklediği senaryolar genel bakış sağlar.
* [Azure veri Kataloğu ile çalışmaya başlama](data-catalog-get-started.md) -bu makalede Azure veri Kataloğu veri kaynağı bulma için kullanmak üzere nasıl oluşturulduğunu gösteren bir uçtan uca öğretici sağlar.  
