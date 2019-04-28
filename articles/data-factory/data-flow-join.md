---
title: Azure Data Factory veri akışı birleştirme dönüştürme
description: Azure Data Factory veri akışı birleştirme dönüştürme
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/07/2019
ms.openlocfilehash: 18f713198ef9aa45cb72a6718c0f7b086c019258
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61348580"
---
# <a name="mapping-data-flow-join-transformation"></a>Veri akışı birleştirme dönüştürme eşlemesi

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Birleştirme, veri akışı iki tablodan verileri birleştirmek için kullanın. Sol ilişki ve araç kutusundan bir birleşim dönüştürme Ekle dönüşümü tıklayın. Birleştirme dönüştürme içinde başka bir veri akışı sağ ilişkisi olması, veri akışından seçersiniz.

![Dönüştürme katılın](media/data-flow/join.png "katılın")

## <a name="join-types"></a>Türleri katılın

Birleştirme türü seçme birleştirme dönüşümü için gereklidir.

### <a name="inner-join"></a>İç birleştirme

İç birleştirme, her iki tablodan sütun koşullara uyan satırları üzerinden geçer.

### <a name="left-outer"></a>Sol dış

Birleştirme koşulu karşılamıyor sol akış tablosundan tüm satırları geçtiğini ve iç birleşim tarafından döndürülen tüm satırları yanı sıra diğer tablodaki çıkış sütunu NULL olarak ayarlanır.

### <a name="right-outer"></a>Sağ dış

Birleştirme koşulu karşılamıyor doğru akış tablosundan tüm satırları geçtiğini ve başka bir tabloya karşılık gelen çıkış sütunları INNER JOIN tarafından döndürülen tüm satırları ek olarak NULL olarak ayarlanır.

### <a name="full-outer"></a>Tam dış

Tam dış birleşim tüm sütunları oluşturur ve iki taraflı olan sütunlar için NULL değerler içeren satırları diğer tablosunda yok.

### <a name="cross-join"></a>Çapraz birleştirme

Çapraz çarpımını iki akışları ile bir ifade belirtin. Özel birleştirme koşulları oluşturmak için kullanabilirsiniz.

## <a name="specify-join-conditions"></a>Birleştirme koşulları belirtin

Left JOIN sol tarafında, birleştirme bağlı veri akışı'ndan bir durumdur. RIGHT JOIN, birleştirme ya da başka bir akışa doğrudan bir bağlayıcı ya da başka bir akışa bir başvuru olacaktır alt bağlı ikinci veri akışını bir durumdur.

En az 1 (1..n) birleştirme koşulları girmeniz gerekir. Bunlar, ya da doğrudan açılan menü veya ifadeleri seçili başvurulan alanlar olabilir.

## <a name="join-performance-optimizations"></a>Performans iyileştirmeleri katılın

SSIS gibi Araçları'nda birleştirme birleşimin aksine, ADF veri akışı birleştirme zorunlu birleştirme birleştirme işlemi değil. Bu nedenle, join anahtarlar ilk olarak sıralanmış gerekmez. Birleştirme işlemi, Databricks Spark en uygun birleştirme işleminde temel kullanarak Spark meydana gelir: Yayın / Map tarafı birleştirme:

![Dönüştürme katılın en iyi duruma getirme](media/data-flow/joinoptimize.png "iyileştirme katılın")

Veri kümeniz Databricks çalışan düğümü belleğe sığıyorsa biz birleştirme performansınızı en iyi duruma getirebilirsiniz. Verilerinizi daha iyi çalışan her belleğe sığması veri kümelerini oluşturmak için birleştirme işlemi bölümleme de belirtebilirsiniz.

## <a name="self-join"></a>Kendi kendine birleşim

Diğer ad var olan bir akış seçin dönüşümü kullanarak ADF veri akışı kendi kendine birleşme koşulları elde edebilirsiniz. İlk olarak, bir akıştan "Yeni dalı" oluşturmak, sonra bir Select diğer tüm özgün akışa ekleyin.

![Kendi kendine birleşme](media/data-flow/selfjoin.png "kendi kendine birleşim")

Yukarıdaki diyagramda, Select dönüştürme en üstünde alır. Tüm bu yaptığını diğer ad kullanımı "OrigSourceBatting" için özgün akışıdır. Vurgulanan birleştirme Dönüştür altındaki bu seçim diğer akış sağ birleştirme, aynı anahtarı sol ve sağ tarafında iç birleşim başvurmak bize verme kullandığımızı görebilirsiniz.

## <a name="composite-and-custom-keys"></a>Bileşik ve özel anahtarlar

İç birleşim dönüşümü hareket halindeyken özel ve karma anahtarları oluşturabilirsiniz. Her ilişki satır yanındaki artı işaretini (+) ile ek birleştirme sütunların satır ekleyin. Veya yeni bir anahtar değeri ifade oluşturucu üzerinde halindeyken birleştirme değeri için işlem.

## <a name="next-steps"></a>Sonraki adımlar

Veri katıldıktan sonra böylece [yeni sütunlar oluşturmanıza](data-flow-derived-column.md) ve [verilerinizi hedef veri deposuna havuz](data-flow-sink.md).
