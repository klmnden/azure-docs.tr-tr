---
title: Azure - Azure API FHIR için desteklenen FHIR özellikleri
description: Bu makalede, Azure API FHIR için uygulanan FHIR belirtimi özelliklerine açıklanmaktadır.
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.topic: reference
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: 1752ec8b2f846b51ef8222c54a00d5a5a0cdd05a
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55875209"
---
# <a name="features"></a>Özellikler

Azure API FHIR için tam olarak yönetilen bir Azure için Microsoft FHIR Server dağıtımını sağlar. Sunucu uygulamasıdır [FHIR](https://hl7.org/fhir) standart. Bu belge, FHIR sunucunun ana özelliklerini listeler.

## <a name="fhir-version"></a>FHIR sürümü

Geçerli sürüm: `3.0.1`

## <a name="rest-api"></a>REST API

| API                            | Desteklenen | Açıklama |
|--------------------------------|-----------|---------|
| oku                           | Evet       |         |
| vread                          | Evet       |         |
| update                         | Evet       |         |
| Güncelleştirme ile İyimser kilitleme | Evet       |         |
| Güncelleştirme (koşullu)           | Hayır        |         |
| Düzeltme Eki                          | Hayır        |         |
| delete                         | Evet       |         |
| (koşullu) Sil           | Hayır        |         |
| oluşturmaya                         | Evet       | Her iki POST/PUT desteği |
| (koşullu) oluşturma           | Hayır        |         |
| ara                         | Kısmi   | Aşağıya bakın |
| Özellikleri                   | Evet       |         |
| toplu iş                          | Hayır        |         |
| İşlem                    | Hayır        |         |
| geçmiş                        | Evet       |         |
| Disk belleği                         | Kısmi   | `self` ve `next` desteklenir |
| Aracılar                 | Hayır        |         |

## <a name="search"></a>Arama

Tüm arama parametre türleri desteklenir. Zincirleme parametreleri ve ters zincirleme olan *değil* desteklenir.

| Arama parametresi türü | Desteklenen | Açıklama |
|-----------------------|-----------|---------|
| Sayı                | Evet       |         |
| Tarih/tarih saat         | Evet       |         |
| String                | Evet       |         |
| Belirteç                 | Evet       |         |
| Başvuru             | Evet       |         |
| Bileşik             | Evet       |         |
| Miktar              | Evet       | Sorunu [#103](https://github.com/Microsoft/fhir-server/issues/103) |
| URI                   | Evet       |         |


| Değiştiriciler             | Desteklenen | Açıklama |
|-----------------------|-----------|---------|
|`:missing`             | Evet       |         |
|`:exact`               | Evet       |         |
|`:contains`            | Evet       |         |
|`:text`                | Evet       |         |
|`:in` (belirteç)          | Hayır        |         |
|`:below` (belirteç)       | Hayır        |         |
|`:above` (belirteç)       | Hayır        |         |
|`:not-in` (belirteç)      | Hayır        |         |
|`:[type]` (başvuru)  | Hayır        |         |
|`:below` (URI)         | Evet       |         |
|`:above` (URI)         | Hayır        | Sorunu [#158](https://github.com/Microsoft/fhir-server/issues/158) |

| Genel arama parametresi | Desteklenen | Açıklama |
|-------------------------| ----------|---------|
| `_id`                   | Evet       |         |
| `_lastUpdated`          | Evet       |         |
| `_tag`                  | Evet       |         |
| `_profile`              | Evet       |         |
| `_security`             | Evet       |         |
| `_text`                 | Hayır        |         |
| `_content`              | Hayır        |         |
| `_list`                 | Hayır        |         |
| `_has`                  | Hayır        |         |
| `_type`                 | Evet       |         |
| `_query`                | Hayır        |         |

| İşlemleri ara       | Desteklenen | Açıklama |
|-------------------------|-----------|---------|
| `_filter`               | Hayır        |         |
| `_sort`                 | Hayır        |         |
| `_score`                | Hayır        |         |
| `_count`                | Evet       |         |
| `_summary`              | Kısmi   | `_summary=count` desteklenir |
| `_include`              | Hayır        |         |
| `_revinclude`           | Hayır        |         |
| `_contained`            | Hayır        |         |
| `_elements`             | Hayır        |         |

## <a name="persistence"></a>Kalıcılık

Microsoft FHIR sunucu takılabilir Kalıcılık modülü sahiptir (bkz [ `Microsoft.Health.Fhir.Core.Features.Persistence` ](https://github.com/Microsoft/fhir-server/tree/master/src/Microsoft.Health.Fhir.Core/Features/Persistence)).

Şu anda bir uygulama için FHIR sunucu açık kaynak kodu içeren [Azure Cosmos DB](../cosmos-db/index-overview.md).

Cosmos DB (SQL API'si, MongoDB API'si, vb.) global olarak dağıtılmış çok modelli bir veritabanıdır. Farklı destekler [tutarlılık düzeyleri](../cosmos-db/consistency-levels.md). Varsayılan dağıtım şablonu FHIR sunucuyla yapılandırır `Strong` tutarlılık ancak tutarlılık İlkesi değiştirilebilir (genellikle esnek) bir istek tarafından istek başına kullanma `x-ms-consistency-level` istek üst bilgisi.

## <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi

FHIR sunucusunun kullandığı [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) erişim denetimi için. Özellikle, rol tabanlı erişim denetimi (RBAC), varsa uygulanmaz `FhirServer:Security:Enabled` yapılandırma parametresi ayarlandığında `true`ve tüm istekleri (dışında `/metadata`) FHIR sunucusuna sahip olmanız gerekir `Authorization` kümesine istek üstbilgisi `Bearer <TOKEN>`. Belirteç, sınıfında tanımlandığı gibi bir veya daha fazla rol içermesi gereken `roles` talep. İstek belirteci rol içeriyorsa, belirtilen kaynak üzerinde belirtilen eylemi sağlayan izin verilir.

Şu anda, belirli bir rol için izin verilen eylemleri uygulanan *genel* API üzerinde.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure API desteklenen FHIR özelliklerini FHIR için makaleyi okudunuz. Sonraki FHIR API azure'da dağıtın.
 
>[!div class="nextstepaction"]
>[Açık kaynak FHIR sunucusu dağıtma](fhir-oss-powershell-quickstart.md)