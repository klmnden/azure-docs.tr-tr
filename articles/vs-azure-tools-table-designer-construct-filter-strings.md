---
title: Filtre dizeleri oluşturmak için Tablo Tasarımcısı | Microsoft Docs
description: Filtre dizeleri oluşturmak için Tablo Tasarımcısı
services: visual-studio-online
author: ghogen
manager: douge
assetId: a1a10ea1-687a-4ee1-a952-6b24c2fe1a22
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 11/18/2016
ms.author: ghogen
ms.openlocfilehash: 1e2fd16d25d9552783d12000820cac56803fca63
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2018
ms.locfileid: "50969387"
---
# <a name="constructing-filter-strings-for-the-table-designer"></a>Filtre dizeleri oluşturmak için Tablo Tasarımcısı
## <a name="overview"></a>Genel Bakış
Visual Studio içinde görüntülenen bir Azure tablosu verilere filtre uygulamak **Tablo Tasarımcısı**, bir filtre dizesi oluşturmak ve filtre alanına girin. Filtre dizesi söz dizimi WCF Veri Hizmetleri tarafından tanımlanır ve bir SQL WHERE yan tümcesine benzer, ancak tablo hizmeti bir HTTP isteği aracılığıyla gönderilir. **Tablo Tasarımcısı** uygun sizin için kodlama işler üzerinde istenen özellik değerini filtrelemek için yalnızca özellik adı, karşılaştırma işleci, ölçüt değeri ve isteğe bağlı, Boolean girmeniz filtre alanına işleci. $Filter sorgu seçeneği aracılığıyla bir tabloyu sorgulamak üzere bir URL oluştururken yaptığınız gibi dahil gerekmez [depolama hizmetleri REST API Başvurusu](http://go.microsoft.com/fwlink/p/?LinkId=400447).

WCF Veri Hizmetleri dayalı [açık veri Protokolü](http://go.microsoft.com/fwlink/p/?LinkId=214805) (OData). Filtre sistem sorgusu seçeneği hakkında ayrıntılı bilgi için (**$filter**), bkz: [OData URI kurallarını belirtimi](http://go.microsoft.com/fwlink/p/?LinkId=214806).

## <a name="comparison-operators"></a>Karşılaştırma İşleçleri
Aşağıdaki mantıksal işleçler için tüm özellik türleri desteklenir:

| Mantıksal işleci | Açıklama | Örnek filtre dizesi |
| --- | --- | --- |
| EQ |eşittir |Şehir eq 'Redmond' |
| gt |Büyüktür |Fiyat gt 20 |
| Ge |Büyük veya eşit |Fiyat ge 10 |
| lt |Şu değerden az: |Fiyat lt 20 |
| le |Küçüktür veya eşittir |Fiyat le 100 |
| ne |Eşit değildir |Şehir ne 'Londra' |
| ve |Ve |Fiyat le 200 ve fiyat gt 3.5 |
| or |Veya |Fiyat le 3.5 veya 200 fiyat gt |
| değil |değil |IsAvailable değil |

Bir filtre dizesi oluştururken aşağıdaki kuralları önemlidir:

* Mantıksal işleçler bir özelliği bir değerle karşılaştırmak için kullanılır. Bir özelliği dinamik değerle karşılaştırmak mümkün değil olduğunu unutmayın. ifadenin bir tarafı sabit olmalıdır.
* Filtre dizesinin tüm kısımları büyük/küçük harfe duyarlıdır.
* Filtrenin geçerli sonuçlar döndürmesi için sabit değer, özellikle aynı veri türünde olmalıdır. Desteklenen özellik türleri hakkında daha fazla bilgi için bkz. [Tablo Hizmeti Veri Modelini anlama](http://go.microsoft.com/fwlink/p/?LinkId=400448).

## <a name="filtering-on-string-properties"></a>Dize özellikleri üzerinde filtreleme
Dize özellikleri filtrelerken, dize sabiti tek tırnak işaretleri içine alın.

Aşağıdaki örnek filtrelerle **PartitionKey** ve **RowKey** özellikleri; anahtar olmayan ek özellikler için filtre dizesini de eklenebiliyordu:

    PartitionKey eq 'Partition1' and RowKey eq '00001'

Gerekli olmamasına rağmen her filtre ifadesi, parantez içine:

    (PartitionKey eq 'Partition1') and (RowKey eq '00001')

Tablo hizmeti, joker karakter sorguları desteklemiyor ve Tablo Tasarımcısı'nda ya da desteklenmeyen bir durumdur unutmayın. Ancak, önek eşleştirme istenen ön ek Karşılaştırma işleçleri kullanarak gerçekleştirebilirsiniz. Aşağıdaki örnek, varlıklar 'A' harfi ile başlayan bir soyadı özelliği ile döndürür:

    LastName ge 'A' and LastName lt 'B'

## <a name="filtering-on-numeric-properties"></a>Sayısal özellik üzerinde filtreleme
Bir tamsayı veya kayan noktalı sayı filtrelemek için tırnak işaretleri olmadan bir sayı belirtin.

Bu örnek değeri 30'dan büyük olan bir yaş özelliğine sahip tüm varlıkları döndürür:

    Age gt 30

Bu örnek değeri 100.25 küçük veya ona eşit olan bir AmountDue özelliğine sahip tüm varlıkları döndürür:

    AmountDue le 100.25

## <a name="filtering-on-boolean-properties"></a>Boole özellikleri üzerinde filtreleme
Bir Boolean değeri filtrelemek için belirtin **true** veya **false** tırnak işaretleri olmadan.

Aşağıdaki örnek Isactive özelliğinin ayarlandığı tüm varlıkları döndürür **true**:

    IsActive eq true

Bu filtre ifadesi Mantıksal işleci olmadan da yazabilirsiniz. Aşağıdaki örnekte, tablo Hizmeti ayrıca tüm varlıklar Isactive olduğu döndürür **true**:

    IsActive

Isactive false olduğu tüm varlıklar döndürmek için değil kullanabilirsiniz işleci:

    not IsActive

## <a name="filtering-on-datetime-properties"></a>DateTime özellikleri üzerinde filtreleme
Bir DateTime değeri filtrelemek için belirtin **datetime** tek tırnak işaretleri içindeki tarih/saat sabiti ardından anahtar sözcüğü. Tarih/saat sabiti açıklandığı birleşik UTC biçiminde olmalıdır [DateTime özellik değerlerini biçimlendirme](http://go.microsoft.com/fwlink/p/?LinkId=400449).

Aşağıdaki örnek, CustomerSince özelliği 10 Temmuz 2008'e eşit olduğu varlıklar döndürür:

    CustomerSince eq datetime'2008-07-10T00:00:00Z'
