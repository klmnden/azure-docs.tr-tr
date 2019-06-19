---
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 11/09/2018
ms.author: jingwang
ms.openlocfilehash: 1ab404b838af65dcb75395dfeee1ca0553e497a1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66122519"
---
## <a name="specifying-structure-definition-for-rectangular-datasets"></a>Dikdörtgen veri kümeleri için yapı tanımını belirtme
JSON veri kümesi yapısı bölümünde bir **isteğe bağlı** bölümünde dikdörtgen tablolarla (satırlar ve sütunlar) için ve bir tablonun sütunlarını koleksiyonunu içerir. Tür dönüştürmeleri sağlayan iki tür bilgileri veya sütun eşlemelerini yapmak için yapı bölüm kullanır. Aşağıdaki bölümlerde, bu özelliklerin ayrıntılı açıklanmaktadır. 

Her sütun, aşağıdaki özellikleri içerir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| name |Sütunun adı. |Evet |
| type |Sütunun veri türü. Görmek için aşağıdaki tür dönüştürmeleri bölümüne daha fazla ayrıntı ile ilgili ne zaman tür bilgileri belirtmeniz gerekir |Hayır |
| culture |.NET türü belirtildi ve .NET türü Datetime veya Datetimeoffset olduğunda kullanılacak kültürü temel. Varsayılan değer "en-us". |Hayır |
| format |Datetime veya Datetimeoffset türü belirtildi ve .NET olduğunda kullanılacak biçim dizesi yazın. |Hayır |

Aşağıdaki örnek, üç sütun kimliği, adı ve lastlogindate sahip bir tablo yapısı bölümü JSON gösterir.

```json
"structure": 
[
    { "name": "userid"},
    { "name": "name"},
    { "name": "lastlogindate"}
],
```

Lütfen "yapı" bilgileri içerecek şekilde ne zaman ve hangi eklemek için aşağıdaki yönergeleri kullanın **yapısı** bölümü.

* **Yapılandırılmış veri kaynakları için** deposu veri şema ve tür bilgilerini birlikte verilerin kendisi (yalnızca istiyorsanız "yapı" bölümünde belirtmelidir kaynakları SQL Server, Oracle, Azure tablo vb. gibi), belirli bir kaynak sütun eşlemesi yapın Havuz ve adları belirli sütunlardaki sütunları (sütun eşleme bölümüne aşağıdaki ayrıntılara bakın) aynı değildir. 
  
    Yukarıda belirtildiği gibi tür bilgilerini "yapı" bölümünde isteğe bağlıdır. Yapılandırılmış kaynakları için tür bilgilerini zaten kullanılabilir veri deposundaki veri kümesi tanımının bir parçası, bu nedenle dahil tür bilgileri "yapı" bölümü eklediğinizde.
* **Şema (özellikle, Azure blob) salt okunur veri kaynaklarında** herhangi bir şema veya türü bilgi veri depolamadan veri saklamayı da seçebilirsiniz. Bu veri kaynağı türleri için aşağıdaki 2 durumda "yapı" içermelidir:
  * Sütun eşlemesi yapmak istiyorsunuz.
  * Veri kümesi bir kopyalama etkinliği kaynağı olduğunda, "yapı" tür bilgilerini sağlayabilir ve data factory, bu tür bilgiler havuz için yerel türlere dönüşüm için kullanır. Bkz: [için ve Azure Blob veri taşıma](../articles/data-factory/v1/data-factory-azure-blob-connector.md) makale daha fazla bilgi için.

### <a name="supported-net-based-types"></a>Desteklenmiyor. AĞ tabanlı türleri
Data factory, Azure blob gibi salt okunur veri kaynakları üzerinde şema için "yapı" tür bilgileri sağlamak için aşağıdaki CLS uyumlu temel .NET türü değerleri destekler.

* Int16
* Int32 
* Int64
* Single
* Double
* Decimal
* Byte[]
* Bool
* String 
* Guid
* Datetime
* Datetimeoffset
* Timespan 

DateTime ve Datetimeoffset için Ayrıca isteğe bağlı olarak, özel tarih saat dizesi ayrıştırma kolaylaştırmak için "kültür" & "biçim" dizesi belirtebilirsiniz. Aşağıdaki tür dönüştürme için örneğine bakın.

