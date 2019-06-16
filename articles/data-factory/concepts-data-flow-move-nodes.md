---
title: Azure Data Factory veri akışı taşıma düğümleri
description: Bir Azure veri fabrikası eşleme veri akışı diyagramda düğümleri taşıma
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/04/2018
ms.openlocfilehash: 951a5d4fcbd561b085b0377bde48e820dc8972a2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65519963"
---
# <a name="mapping-data-flow-move-nodes"></a>Veri akışı taşıma düğümleri eşleştirme

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

![Dönüştürme seçenekleri toplama](media/data-flow/agghead.png "toplayıcısı üstbilgisi")

Azure Data Factory, veri akışı tasarım yüzeyine veri akışları yukarıdan aşağıya, soldan sağa, oluşturduğunuz bir "yapı" yüzeyidir. Her dönüştürme bir artı ile bağlı bir araç kutusu yok (+) simgesi. Serbest biçimli DAG ortamında kenarlar aracılığıyla düğümlerini bağlayan yerine iş mantığınıza yoğunlaşır.

Bu nedenle, bir Sürükle ve bırak paradigma "dönüştürme düğümü, Taşı" yoludur gelen değiştirmek için. Bunun yerine, dönüşümler "gelen akış" değiştirerek taşınacaktır.

## <a name="streams-of-data-inside-of-data-flow"></a>Veri akışı içinde veri akışları

Azure Data Factory veri akışı, akış veri akışını temsil eder. Dönüştürme ayarlar bölmesinde, bir "Gelen Buhar" alan görürsünüz. Bu, hangi gelen veri akışını, bu dönüştürme besleme bildirir. Gelen Stream adına tıklayarak ve başka bir veri akışı seçerek, grafik dönüştürme düğümünde fiziksel konumunu değiştirebilirsiniz. Geçerli dönüşümü, stream'deki tüm sonraki dönüşümler yanı sıra, ardından yeni konumuna taşınır.

Bir veya daha fazla dönüşümler ile dönüştürme sonraki taşıyorsanız yeni konumda veri akışı yeni bir dal katılır.

Seçtiğiniz düğümünün sonra sonraki hiçbir dönüştürme varsa, yalnızca dönüşüm yeni konumuna taşıyın.

## <a name="next-steps"></a>Sonraki adımlar

Hata Ayıkla düğmesine veri akışı tasarımınızı tamamladıktan sonra ' ı açma ve kapatma hata ayıklama modunda ya da doğrudan test [veri akışı Tasarımcısı](concepts-data-flow-debug-mode.md) veya [işlem hattı hata ayıklama](control-flow-execute-data-flow-activity.md).
