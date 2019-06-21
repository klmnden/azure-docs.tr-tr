---
title: Azure veri fabrikası veri akışı hata ayıklama modu eşleme
description: Ne zaman verilerini kullanarak bina akışları bir etkileşimli hata ayıklama oturumu başlatın
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/04/2018
ms.openlocfilehash: d86725718217caf7fd1d9dd6d5d67362e5de7270
ms.sourcegitcommit: 72f1d1210980d2f75e490f879521bc73d76a17e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/14/2019
ms.locfileid: "67147386"
---
# <a name="mapping-data-flow-debug-mode"></a>Eşleme veri akışı hata ayıklama modu

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Azure Data Factory eşleme veri akışı'nın hata ayıklama modu tasarım yüzeyine üst kısmındaki "Veri akışı Debug" düğmesiyle değiştirilebilir. Ne zaman veri akışları, hata ayıklama modunu açma tasarlama işlem sonrasında, verileri etkileşimli olarak izlemek yapı ve veri akışlarınızı hata ayıklama, dönüştürme şekil. Hata ayıklama oturumu, hem veri akışı tasarım oturumlarda yanı sıra veri akışı işlem hattı hata ayıklama yürütme sırasında kullanılabilir.

![Düğme hata ayıklama](media/data-flow/debugbutton.png "Hata Ayıkla düğmesine")

## <a name="overview"></a>Genel Bakış
Hata ayıklama modu etkin olduğunda, etkileşimli veri akışınızı etkin bir Spark kümesi oluşturacaksınız. Hata ayıklama Azure Data Factory'de kapattığınızda oturumu sona erecektir. Hata ayıklama oturumunuz açık olduğu süre boyunca Azure Databricks tarafından tahakkuk saatlik ücretler farkında olmalıdır.

Çoğu durumda, böylece iş mantığınızı doğrulamak ve Azure Data Factory'de iş yayımlamadan önce veri Bağlantılarınızdaki görüntülemek, veri akışları hata ayıklama modunda oluşturmak için bir alışkanlıktır. "Debug" düğmesini'ardışık düzen paneline bir işlem hattı içinde veri akışınızı test etmek için kullanın.

> [!NOTE]
> Hata ayıklama modu açık Data Factory araç çubuğundaki yeşil olsa da, veri akışı hata ayıklama fiyatı 8 çekirdek/SA genel işlem ile 60 dakika yaşam süresi ücretlendirilirsiniz 

> [!NOTE]
>Veri akışı, hata ayıklama modunda çalışırken, verileriniz için havuz yazılmayacak dönüştürün. Hata ayıklama oturumunun Bağlantılarınızdaki için test bandı olarak hizmet vermek için tasarlanmıştır. Havuzlar, hata ayıklama sırasında gerekli değildir ve veri akışınızı yoksayılır. Veri havuzunuzu yazmak test etmek istiyorsanız, bir Azure Data Factory işlem hattından yürütme veri akışı ve hata ayıklama yürütmeyi bir işlem hattı kullanın.

## <a name="debug-settings"></a>Hata ayıklama ayarları
Hata ayıklama ayarları düzenlenebilir "Hata ayıklama ayarları" tıklayarak veri akışı Tuval araç. Sınırları ve/veya her biri, kaynak dönüştürmeleri için kullanılacak dosya kaynağı seçebilirsiniz. Yalnızca geçerli hata ayıklama oturumu için bu ayarı satır limitlerdir. SQL DW kaynağı için kullanılacak hazırlama bağlı hizmetini de seçebilirsiniz. 

![Hata ayıklama ayarları](media/data-flow/debug-settings.png "hata ayıklama ayarları")

## <a name="cluster-status"></a>Küme durumu
Küme hata ayıklama için hazır olduğunda, yeşile döner tasarım yüzeyinde üst küme durum göstergesi yoktur. Kümeniz zaten sıcak ise, yeşil göstergesi neredeyse anında görüntülenir. Hata ayıklama moduna girildiğinde kümeniz zaten çalışıyor durumda değilse, kümenin dönmesi 5-7 dakika beklemeniz gerekecektir. Gösterge, hazır kadar dilediğiniz.

Uygulamanızı hata ayıklama ile işiniz bittiğinde, hata ayıklama anahtarı, Azure Databricks kümeyi sonlandırabilirsiniz ve, artık hata ayıklama etkinliği için faturalandırılırsınız kapatın.

## <a name="data-preview"></a>Veri önizlemesi
İle hata ayıklama üzerinde veri Önizleme sekmesini açık alt panelde yukarı. Hata ayıklama modunu, veri akışı, yalnızca geçerli meta verilerin her Bağlantılarınızdaki içine ve dışına inceleyin sekmede gösterilmektedir. Veri önizleme yalnızca hata ayıklama ayarlarınızda sınırınızı ayarladığınız satır sayısını sorgular. "Verileri getirmek" tıklaymanız gerekebilir yenilemek veri önizlemesi için.

![Veri önizleme](media/data-flow/datapreview.png "veri önizlemesi")

## <a name="data-profiles"></a>Veri profilleri
Sütunları tek tek veri Önizleme sekmesini seçerek bir grafik en sağdaki her alan hakkında ayrıntılı istatistiklerle, veri kılavuzunun üzerinde sorar. Azure Data Factory görüntülemek için grafik hangi tür veri Örnekleme sırasında bir belirlemeyi hale getirir. Veri değeri sıklığı gösteren çubuk grafikler düşük önem düzeyi olan kategorik hem de sayısal verileri görüntüler ancak kardinalite yüksek alanları varsayılan olarak /NOT NULL grafiklere olacaktır. Dize alanları, en düşük/en yüksek değerleri sayısal alanlar, standart sapma, yüzdebirliklerini, sayıları ve ortalama en büyük/uzun uzunluğunu de görürsünüz. 

![Sütun istatistikleri](media/data-flow/stats.png "sütun istatistikleri")

## <a name="next-steps"></a>Sonraki adımlar

Derleme ve hata ayıklama veri akışınız bitirdikten sonra [bir işlem hattından yürütme.](control-flow-execute-data-flow-activity.md)

Bir veri akışı işlem hattınızı test ederken, işlem hattını kullanma [yürütme seçeneği hata ayıklama çalıştırın.](iterative-development-debugging.md)
