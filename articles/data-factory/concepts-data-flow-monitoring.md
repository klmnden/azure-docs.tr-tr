---
title: Azure veri fabrikası eşleme veri akışı görsel izleme
description: Azure Data Factory veri akışları görsel olarak izleme
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/01/2019
ms.openlocfilehash: 90aa6261aebb9d1f7da89c101854bad8061dd6ff
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61269040"
---
# <a name="monitor-data-flows"></a>Veri akışları izleyebilirsiniz

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Derleme ve hata ayıklama veri akışınız tamamladıktan sonra bağlamı bir işlem hattı içerisinde bir zamanlamaya göre çalıştırmak için veri akışı zamanlama isteyeceksiniz. Tetikleyicileri kullanarak Azure Data Factory işlem hattından zamanlayabilirsiniz. Veya Azure Data Factory işlem hattı Oluşturucusu şimdi Tetikle seçeneğinden, veri akışı işlem hattı bağlam içinde test etmek için tek Tıkla Çalıştır yürütme yürütmek için kullanabilirsiniz.

İşlem hattınızın yürüttüğünüzde, işlem hattı ve tüm veri akışı etkinliği içeren işlem hattı, etkinlikleri izlemek mümkün olacaktır. Sol taraftaki Azure Data Factory UI panelinde İzleyici simgesine tıklayın. Aşağıdakine benzer bir ekran görürsünüz. Vurgulanan simgeler, veri akışı etkinliği içeren işlem hattı etkinliklerinin incelemek izin verir.

<img src="media/data-flow/mon001.png" width="800">

Göreceğiniz istatistikleri Bu düzey iyi inculding çalışma zamanları ve durumu. Etkinlik düzeyinde çalıştırma kimliği, çalıştırma işlem hattı düzeyinde Kimliği farklıdır. Önceki düzeyde çalıştırması kimliği için işlem hattı ' dir. Gözlük tıklayarak veri akışı yürütmeyi üzerinde derin ayrıntıları verir.

<img src="media/data-flow/mon002.png" width="800">

İzleme görünümü grafik düğümünde olduğunda basitleştirilmiş bir salt sürümünü, veri akış grafiği görürsünüz.

<img src="media/data-flow/mon003.png" width="800">

## <a name="view-data-flow-execution-plans"></a>Görünüm veri akışı yürütme planları

Databricks'te veri akışınız çalıştırıldığında, Azure Data Factory, veri akışının entirity üzerinde göre en iyi kod yollarını belirler. Ayrıca, yürütme yolları farklı ölçeklendirme düğümleri ve veri bölümleri üzerinde meydana gelebilir. Bu nedenle, izleme graf Bağlantılarınızdaki yürütme yolunu dikkate alarak, akışınız tasarımını temsil eder. Tek düğümler üzerinden tıklattığınızda, "küme üzerinde birlikte yürütülen kodun temsil gruplandırmaları" görürsünüz. Her bir adımı tasarımınızda aksine bu gruplara zamanlamaları ve gördüğünüz sayıları temsil eder.

<img src="media/data-flow/mon004.png" width="800"> 

* Boş bir alanı İzleme penceresinde tıkladığınızda, zamanlama istatistikleri alt bölmede görüntülenir ve her bir havuz ve havuz veri dönüştürme hatlarınız için götüren dönüştürmeleri için satır sayılarını.

* Tek dönüştürmeler seçtiğinizde gösteren bölüm istatistikleri, sütun sayıları komutunu (nasıl veri dağılıyor bölümler arasında), sağ panelde ek geri bildirim alırsınız ve basıklık (verileri nasıl spikey).

* Düğüm görünümü havuzunda tıkladığınızda sütun kökenini görürsünüz. Üç farklı yöntemle sütunları havuzunda yerleşmesi, veri akışı boyunca toplanır. Bunlar:

  * Hesaplanan: Koşullu işlem için veya bir ifade içinde veri akışı sütunu kullanın, ancak havuzunda kavuşmak değil
  * Türetilmiş: Sütun akışınızı oluşturulan yeni bir sütun, yani kaynak mevcut değil
  * Eşlenmiş: Sütunu, kaynak ve olduğunuz kaynaklanan bir havuz alan eşleme
  
## <a name="monitor-icons"></a>İzleyici simgeleri

Bu simge, hesaba gerçekleştirdiğinizden zamanlama ve yürütme yolunu bu nedenle dönüştürme veri kümesinde önbelleğe alınma olduğunu belirtir:

<img src="media/data-flow/mon005.png" width="800"> 

Ayrıca, dönüşümün yeşil daire simgeleri görürsünüz. Bunlar, verileri içine akar havuzlarını sayısını temsil eder.
