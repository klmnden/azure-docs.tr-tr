---
title: Yinelemeli geliştirme ve Azure Data Factory'de hata ayıklama | Microsoft Docs
description: Geliştirme ve yinelemeli olarak Azure Portal'daki Data Factory işlem hatlarını hata ayıklama hakkında bilgi edinin.
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.date: 05/14/2018
ms.topic: conceptual
ms.service: data-factory
services: data-factory
documentationcenter: ''
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.openlocfilehash: ef30958ed0a88853b20278e6d628639ffc0cea34
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34619482"
---
# <a name="iterative-development-and-debugging-with-azure-data-factory"></a>Yinelemeli geliştirme ve Azure Data Factory ile hata ayıklama

Azure Data Factory tekrarlayarak geliştir ve Data Factory işlem hatlarını hata ayıklama olanak sağlar.

Bir sekiz dakikalık bir giriş ve bu özellik tanıtımı için aşağıdaki videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Iterative-development-and-debugging-with-Azure-Data-Factory/player]

## <a name="iterative-debugging-features"></a>Yineleyici hata ayıklama özelliği
İşlem hatlarını oluşturmak ve kullanarak çalışmalarını test **hata ayıklama** tek satırlık bir kod yazmayı olmadan ardışık düzen tuvale özelliği.

![Ardışık Düzen tuval üzerinde özelliğini hata ayıklama](media/iterative-development-debugging/iterative-development-image1.png)

Test sonuçlarını çalışan görünümü **çıkış** ardışık düzen tuvalin penceresi.

![Ardışık Düzen tuvalin çıktı penceresi](media/iterative-development-debugging/iterative-development-image2.png)

Testi başarılı olduktan sonra daha fazla etkinlikleri ardışık düzeninize ekleyin ve yinelemeli bir şekilde hata ayıklama devam edin. Ayrıca **iptal** işlemi devam ederken testi.

![Bir test çalışması iptal et](media/iterative-development-debugging/iterative-development-image3.png)

Çalışmalarını test ettiğinizde seçmeden önce değişikliklerinizi data factory yayımlamak zorunda değilsiniz **hata ayıklama**. Bu değişiklikleri veri fabrikası iş akışı güncelleştirmeden önce beklendiği gibi çalıştığından emin olmak için istediğiniz senaryolarda faydalıdır.

## <a name="more-info-about-debugging"></a>Hata ayıklama hakkında daha fazla bilgi

1. Test ile başlatılan çalışmaları **hata ayıklama** yetenek listesinde kullanılabilir değildir **İzleyici** sekmesi. Bkz: ile Tetiklenmiş çalıştıran yalnızca **şimdi tetikleyici**, **zamanlama**, veya **atlayan pencereyi** içinde tetikler **İzleyici** sekmesi. İle başlatılan son test görebilirsiniz **hata ayıklama** özelliği **çıkış** ardışık düzen tuvalin penceresi.

2. Seçme **hata ayıklama** gerçekten ardışık düzen çalışır. Ardışık Düzen kopyalama etkinliği içeriyorsa, bu nedenle, örneğin, test çalıştırma veri kaynağından hedefe kopyalar. Sonuç olarak, kopyalama etkinliklerinizi ve diğer etkinlikleri hata ayıklama sırasında test klasörleri kullanmanızı öneririz. Ardışık Düzen hata ayıklaması sonra normal işlemlerinde kullanmak istediğiniz gerçek klasörleri geçin.

## <a name="setting-breakpoints-for-debugging"></a>Hata ayıklama kesme noktalarını ayarlama

Veri Fabrikası Ayrıca, belirli bir etkinliğin ardışık düzen tuval üzerinde ulaşana kadar hata ayıklama olanak tanır. Henüz bir kesme noktası kadar istediğiniz test ve seçmek faaliyete koyduğunuz **hata ayıklama**. Veri Fabrikası test ardışık düzen tuvalde yalnızca kesme noktası etkinlik kadar çalıştırmasını sağlar. Bu *hata ayıklama kadar* özelliği, tüm ardışık düzen, ancak yalnızca bir alt etkinliklerin ardışık düzen içinde test etmek istemediğiniz zaman yararlıdır.

![Ardışık Düzen tuval üzerinde kesme noktaları](media/iterative-development-debugging/iterative-development-image4.png)

Kesme noktası ayarlamak için ardışık düzen tuval üzerinde bir öğe seçin. A *hata ayıklama kadar* seçeneği öğeyi sağ üst köşesindeki boş bir kırmızı daire olarak görünür.

![Seçili öğe üzerinde bir kesme noktası ayarlamadan önce](media/iterative-development-debugging/iterative-development-image5.png)

Siz seçtikten sonra *hata ayıklama kadar* seçeneğini göstermek için doldurulmuş kırmızı bir daire için değişiklikleri kesme etkinleştirilir.

![Seçili öğe üzerinde bir kesme noktası ayarladıktan sonra](media/iterative-development-debugging/iterative-development-image6.png)

## <a name="next-steps"></a>Sonraki adımlar
[Sürekli tümleştirme ve Azure Data Factory dağıtımında](continuous-integration-deployment.md)
