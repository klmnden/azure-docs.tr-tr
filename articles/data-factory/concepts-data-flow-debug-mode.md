---
title: Azure veri fabrikası veri akışı hata ayıklama modu eşleme
description: Ne zaman verilerini kullanarak bina akışları bir etkileşimli hata ayıklama oturumu başlatın
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/04/2018
ms.openlocfilehash: a50778db5fd57202c17f05407045259371912586
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66239185"
---
# <a name="mapping-data-flow-debug-mode"></a>Eşleme veri akışı hata ayıklama modu

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Azure Data Factory eşleme veri akışı tasarım yüzeyine üst kısmındaki veri akışı hata ayıklama düğmesiyle arasında geçiş yapabilen bir hata ayıklama modu vardır. Ne zaman veri akışları, hata ayıklama modu ayarını tasarlama işlem sonrasında, verileri etkileşimli olarak izlemek yapı ve veri akışlarınızı hata ayıklama, dönüştürme şekil. Hata ayıklama oturumu, hem veri akışı tasarım oturumlarda yanı sıra veri akışı işlem hattı hata ayıklama yürütme sırasında kullanılabilir.

![Düğme hata ayıklama](media/data-flow/debugbutton.png "Hata Ayıkla düğmesine")

## <a name="overview"></a>Genel Bakış
Hata ayıklama modu etkin olduğunda, etkileşimli veri akışınızı etkin bir Spark kümesi oluşturacaksınız. Hata ayıklama Azure Data Factory'de kapattığınızda oturumu sona erecektir. Hata ayıklama oturumunuz açık olduğu süre boyunca Azure Databricks tarafından tahakkuk saatlik ücretler farkında olmalıdır.

Çoğu durumda, böylece iş mantığınızı doğrulamak ve Azure Data Factory'de iş yayımlamadan önce veri Bağlantılarınızdaki görüntülemek, veri akışları hata ayıklama modunda oluşturmak için bir alışkanlıktır. Bir işlem hattı içinde veri akışınızı test etmek için'ardışık düzen paneline "Debug" düğmesi de kullanmalısınız.

> [!NOTE]
> Hata ayıklama modu açık Data Factory araç çubuğundaki yeşil olsa da, veri akışı hata ayıklama fiyatı 8 çekirdek/SA genel işlem ile 60 dakika yaşam süresi ücretlendirilirsiniz 

## <a name="debug-mode-on"></a>Hata ayıklama modu
Hata ayıklama modu arasında geçiş yaptığınızda, etkileşimli Azure Databricks kümesine gelin ve seçeneklerini kaynak örnekleme için istek bir yan bölme form istenir. Azure Databricks etkileşimli bir kümeden kullanın ve her birinden bir örnekleme boyutu, kaynak dönüşümler seçin veya bir metin dosyası kullanmak için test verilerini çekme gerekir.

<img src="media/data-flow/upload.png" width="400">

> [!NOTE]
>Veri akışı, hata ayıklama modunda çalışırken, verileriniz için havuz yazılmayacak dönüştürün. Bir test olarak görev yapacak bir hata ayıklama oturumu kullanılmaya > Bağlantılarınızdaki bandınız. Havuzlar, hata ayıklama sırasında gerekli değildir ve veri akışınızı yoksayılır. Verileri yazma test etmek isterseniz, >, havuz, bir Azure Data Factory işlem hattından yürütme veri akışı ve hata ayıklama yürütmeyi bir işlem hattı kullanın.

## <a name="debug-settings"></a>Hata ayıklama ayarları
Hata ayıklama ayarları her kaynaktan veri akışı yan bölmede görünür ve "kaynağı ayarlarından veri akışı Tasarımcısı araç çubuğundan"'i seçerek de düzenlenebilir olabilir. Sınırları ve/veya kaynak dönüşümünüzü burada her biri için kullanılacak dosya kaynağı seçebilirsiniz. Yalnızca geçerli hata ayıklama oturumu için bu ayarı satır limitlerdir. Örnekleme ayarı kaynak kaynak dönüşümünü satırları sınırlamak için de kullanabilirsiniz.

## <a name="cluster-status"></a>Küme durumu
Küme hata ayıklama için hazır olduğunda, yeşile döner tasarım yüzeyinde üst küme durum göstergesi yoktur. Kümeniz zaten sıcak ise, yeşil göstergesi neredeyse anında görüntülenir. Hata ayıklama moduna girildiğinde kümeniz zaten çalışmıyorsa, kümenin dönmesi 5-7 dakika beklemeniz gerekecektir. Hazır olana kadar gösterge ışığını sarı olur. Kümenizi veri akışı hata ayıklama için hazır hale geldikten sonra gösterge ışığını yeşile döner.

Uygulamanızı hata ayıklama ile işiniz bittiğinde, hata ayıklama anahtarı, Azure Databricks kümeyi sonlandırabilirsiniz ve hata ayıklama etkinliği için artık faturalandırılırsınız kapatın.

<img src="media/data-flow/datapreview.png" width="400">

## <a name="data-preview"></a>Veri önizlemesi
İle hata ayıklama üzerinde veri Önizleme sekmesini açık alt panelde yukarı. Hata ayıklama modunu, veri akışı, yalnızca geçerli meta verilerin her Bağlantılarınızdaki içine ve dışına inceleyin sekmede gösterilmektedir. Veri önizleme yalnızca hata ayıklama ayarlarınızda sınırınızı ayarladığınız satır sayısını sorgular. "Verileri getirmek" tıklaymanız gerekebilir yenilemek veri önizlemesi için.

<img src="media/data-flow/stats.png" width="400">

## <a name="data-profiles"></a>Veri profilleri
Seçerek tek tek sütun, veri Önizleme sekmesinde açılır bir grafik en sağdaki her alan hakkında ayrıntılı istatistiklerle, veri kılavuzunun üzerinde çalışır. Azure Data Factory görüntülemek için grafik hangi tür veri Örnekleme sırasında bir belirlemeyi hale getirir. Yüksek kardinalite alanları varsayılan NULL / veri değeri sıklığı gösteren çubuk grafikler düşük önem düzeyi olan kategorik hem de sayısal verileri görüntüler ancak grafikleri NULL değil. Ayrıca görürsünüz en büyük / uzun dize alanları uzunluğunu min / en büyük sayısal alanlar, standart sapma, yüzdebirliklerini, sayıları ve ortalama değerler. 

<img src="media/data-flow/chart.png" width="400">

## <a name="next-steps"></a>Sonraki adımlar

Tamamlanmış derleme ve hata ayıklama veri akışınız olduktan sonra [bir işlem hattından yürütme.](control-flow-execute-data-flow-activity.md)

Bir veri akışı işlem hattınızı test ederken, işlem hattını kullanma [yürütme seçeneği hata ayıklama çalıştırın.](iterative-development-debugging.md)
