---
title: Azure veri fabrikası veri akışı Pivot dönüştürme eşlemesi
description: Azure veri fabrikası eşleme veri akışı Pivot dönüştürme kullanarak sütunları satır özet verileri
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/30/2019
ms.openlocfilehash: e16cac281b77f3ca93d9ef358ae806203bc8b663
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/11/2019
ms.locfileid: "59490196"
---
# <a name="azure-data-factory-pivot-transformation"></a>Azure veri fabrikası pivot dönüştürme
[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Pivot ADF veri akışı içinde bir toplama ' farklı satır değerlerinin tek tek sütunlara dönüştürülmüş sahip olduğu bir veya daha fazla gruplandırma sütunlarının kullanın. Esas olarak, yeni sütuna (veri meta verilere kapatmak) satır değerlerinin Özet.

![Özet Seçenekleri](media/data-flow/pivot1.png "Özet 1")

## <a name="group-by"></a>Gruplandırma ölçütü:

![Özet Seçenekleri](media/data-flow/pivot2.png "Özet 2")

İlk olarak, pivot toplama için istediğiniz Gruplanacak sütunları ayarlayın. İle burada 1'den fazla sütun ayarlayabilirsiniz + sütun listesi yanındaki işareti.

## <a name="pivot-key"></a>Pivot anahtarı

![Özet Seçenekleri](media/data-flow/pivot3.png "Özet 3")

ADF satırdan sütuna Özet sütun Pivot anahtardır. Varsayılan olarak, bu alan için veri kümesindeki her bir benzersiz değer için bir sütun Özet. Ancak, Özet sütun değerleri istediğiniz veri kümesinden isteğe bağlı olarak değerleri girebilirsiniz. Oluşturulacak yeni sütunlar belirleyen sütun budur.

## <a name="pivoted-columns"></a>Özetlenmiş sütun

![Özet Seçenekleri](media/data-flow/pivot4.png "Özet 4")

Son olarak, ve yeni çıkış projeksiyon dönüşümü gelen görüntülenecek sütunları yönteminizi özetlenmiş değerler için kullanmak istediğiniz toplama seçersiniz.

(İsteğe bağlı) Bir adlandırma deseni bir önek, Orta ve satır değerleri her yeni sütun adına eklenecek son eki ile ayarlayabilirsiniz.

Örneği için "Sales" özetleme "Bölgeye göre" yeni sütun değerlerini her bir değerden satış, yani neden olur "25", "50", "1000" vs. "Satış-" ön eki değeri ayarlarsanız, ancak her bir sütunun değeri "Sales-" değerinin başına eklersiniz.

![Özet Seçenekleri](media/data-flow/pivot5.png "Özet 5")

Sütun düzenini "Normal" ayarını Grup birlikte tüm toplanan değerleriyle özetlenmiş sütun. Sütunları düzenleme için "Yanal" ayarı, sütun ve değer arasında alternatif.

### <a name="aggregation"></a>Toplama

Özet değerler için kullanmak istediğiniz toplama ayarlamak için alanın özetlenmiş sütun bölmesinin alt kısmındaki tıklayın. Burada bir toplama ifadesi oluştur ve yeni, toplanan değerler için bir tanımlayıcı diğer ad sağlamak ADF veri akışı ifade oluşturucu içine girer.

Özetlenmiş sütun dönüşümlerini ifade oluşturucu açıklamak için ADF veri akışı ifade dili kullanma: https://aka.ms/dataflowexpressions.

## <a name="pivot-metadata"></a>Pivot meta verileri

Pivot dönüşümü gelen verilerinizi temel alan dinamik yeni sütun adları üretecektir. Pivot anahtarını her yeni sütun adı değerlerini üretir. Etmez değerlerini ayrı ayrı belirtin ve dinamik sütun adları her bir benzersiz değer için Özet anahtarınızı oluşturmak istediğiniz UI inceleyin meta verileri görüntülenmez ve havuz dönüşümü hiçbir sütun yayılması olacaktır. Pivot anahtarı için değerleri ayarlarsanız, ADF yeni sütun adlarını belirleyebilirsiniz ve bu sütun adlarını inceleyin kullanılabilir ve eşleme havuz.

### <a name="landing-new-columns-in-sink"></a>Yeni havuz sütunlarında giriş

Bile dinamik sütun adları ile Özet olarak, yine de yeni sütun adları ve değerleri hedef deponuza havuz. "Şema değişikliklerini izin ver" üzerinde havuz ayarlarınızda ayarlamanız yeterlidir. Yeni dinamik adlarını, sütun meta verilerinde görmeyeceğiniz, ancak veri anlaşmaları gerçekleştirmek şema değişikliklerini seçeneği sağlayacak.

### <a name="view-metadata-in-design-mode"></a>Tasarım modu meta verilerini görüntüleme

Yeni sütun adlarını inceleyin meta veriler olarak görüntülemek istediğiniz ve havuz dönüşümü açıkça yaymak sütunları görmek istiyorsanız, açık değerler Pivot anahtar sekmede ayarlayın.

### <a name="how-to-rejoin-original-fields"></a>Özgün alanları yeniden katılmasına nasıl
Pivot dönüşümü yalnızca toplama, gruplandırma ve Özet eylem kullanılan sütunları proje. Akışınızı önceki adımdan gelen diğer sütunları eklemek isterseniz, önceki adımdan gelen yeni bir dal kullanın ve akış özgün metaverileri gereğince ile bağlanmak için kendi kendine birleşme düzeni'ni kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Deneyin [dönüştürme Çöz](data-flow-unpivot.md) sütun değerleri satır değerlerinin açmak için. 
