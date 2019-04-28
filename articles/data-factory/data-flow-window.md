---
title: Azure veri fabrikası veri akışı penceresi dönüştürme eşlemesi
description: Azure veri fabrikası veri akışı penceresi dönüştürme eşlemesi
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/30/2019
ms.openlocfilehash: 6f3f06ff54fc76416ba63f4f09835897d546f8dc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61349963"
---
# <a name="azure-data-factory-window-transformation"></a>Azure veri fabrikası penceresi dönüştürme

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Burada, veri akışlarını pencere tabanlı toplamalar sütunların tanımlayacaksınız penceresi dönüşümdür. İfade Oluşturucu'da, farklı türde veri veya zaman windows (SQL OVER yan) temelinde toplamalar tanımlayabilirsiniz sağlama, GECİKME, NTILE, CUMEDIST, sıralama, vb. gibi.). Çıkışınızda bu toplamalar içeren yeni bir alan oluşturuldu. Ayrıca isteğe bağlı gruplandırma ölçütü alanları ekleyebilirsiniz.

![Pencere seçenekleri](media/data-flow/windows1.png "windows 1")

## <a name="over"></a>Üzerinden
Pencere dönüşümünüzü sütun verileri bölümleme ayarlayın. SQL eşdeğerdir ```Partition By``` Over yan tümcesinde SQL. Bir hesaplama veya bir bölümleme için kullanılacak bir ifade oluşturmak istiyorsanız, sütun adının üzerine gelerek yapın ve "hesaplanan sütun" seçin.

![Pencere seçenekleri](media/data-flow/windows4.png "windows 4")

## <a name="sort"></a>Sırala
Over yan tümcesi başka bir parçası ayarlama ```Order By```. Bu sıralama veri sıralama ayarlar. Calculate değeri bir ifade sıralamak için bu sütun alanı oluşturabilirsiniz.

![Pencere seçenekleri](media/data-flow/windows5.png "windows 5")

## <a name="range-by"></a>Tarafından aralığı
Ardından, pencere çerçevesi, Unbounded veya sınırlanmış olarak ayarlayın. Bir sınırsız pencere çerçevesi ayarlamak için kaydırıcıyı Unbounded için her iki End'i ayarlayın. Ardından, Unbounded ve Current Row arasında bir ayarı seçerseniz, uzaklığı başlangıç ayarlayın ve değerleri son gerekir. Her iki değer, pozitif tam sayılar olacaktır. Verilerinizden göreli sayılar veya değerini kullanabilirsiniz.

Pencere kaydırıcıyı ayarlamak için iki değere sahiptir: geçerli satır ve geçerli satırda sonraki değerler önce değerleri. Başlangıç ve bitiş uzaklığı kaydırıcı üzerinde iki seçiciler eşleşir.

![Pencere seçenekleri](media/data-flow/windows6.png "windows 6")

## <a name="window-columns"></a>Penceresi sütunları
Son olarak, verilerle windows boyut, sayısı, MIN, MAX kullanmak istediğiniz toplamalar tanımlamak için ifade oluşturucu, YOĞUN derece, müşteri adayı, GECİKME, vb. kullanın.

![Pencere seçenekleri](media/data-flow/windows7.png "windows 7")

Toplama ve ADF veri akışı ifade dili ifade Oluşturucu ile kullanmak için analitik işlevler tam listesini burada listelenir: https://aka.ms/dataflowexpressions.

## <a name="next-steps"></a>Sonraki adımlar

Basit grup olarak bir toplama için arıyorsanız kullanın [toplama dönüşümü](data-flow-aggregate.md)
