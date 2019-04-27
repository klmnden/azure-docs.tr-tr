---
title: Yinelemeli geliştirme ve Azure Data Factory'de hata ayıklama | Microsoft Docs
description: Geliştirme ve Data Factory işlem hattı çalıştırmalarınızı Azure Portalı'nda hata ayıklama hakkında bilgi edinin.
ms.date: 09/26/2018
ms.topic: conceptual
ms.service: data-factory
services: data-factory
documentationcenter: ''
ms.workload: data-services
ms.tgt_pltfrm: na
author: gauravmalhot
ms.author: gamal
manager: craigg
ms.openlocfilehash: a8028fdde93d06f7b25bf9bd8b4ed5a560a35f83
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60686316"
---
# <a name="iterative-development-and-debugging-with-azure-data-factory"></a>Yinelemeli geliştirme ve Azure Data Factory ile hata ayıklama

Azure Data Factory, yinelemeli olarak geliştirme ve Data Factory işlem hatlarını hata ayıklama sağlar.

Bir sekiz dakikalık bir giriş ve bu özelliği için şu videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Iterative-development-and-debugging-with-Azure-Data-Factory/player]

## <a name="iterative-debugging-features"></a>Yineleyici hata ayıklama özellikleri
İşlem hatları oluşturun ve test çalıştırmaları kullanarak **hata ayıklama** tek satırlık bir kod yazmadan işlem hattı tuvalinde yeteneği.

![İşlem hattı tuvalinde yeteneğini hata ayıklama](media/iterative-development-debugging/iterative-development-image1.png)

Test sonuçlarını çalışan görünümü **çıkış** işlem hattı tuvalinde penceresi.

![İşlem hattı tuvalinde çıkış penceresi](media/iterative-development-debugging/iterative-development-image2.png)

Bir test çalıştırması başarılı olduktan sonra daha fazla etkinlik ardışık düzeninize ekleme ve yinelemeli bir şekilde hata ayıklamaya devam et. Ayrıca **iptal** devam ederken testi.

![Bir test çalıştırması iptal et](media/iterative-development-debugging/iterative-development-image3.png)

Test çalıştırmaları, seçtiğiniz önce yaptığınız değişiklikleri data factory'de yayımlamak zorunda değilsiniz **hata ayıklama**. Bu özellik değişiklikleri veri fabrikası iş akışı güncelleştirmeden önce beklendiği gibi çalıştığından emin olmak için istediğiniz senaryolarda yararlıdır.

> [!IMPORTANT]
> Seçme **hata ayıklama** aslında işlem hattını çalıştırır. İşlem hattının kopyalama etkinliği içeriyorsa, bu nedenle, örneğin, test çalıştırması verileri kaynaktan hedefe kopyalar. Sonuç olarak, test klasörleri kopyalama etkinliklerinizi ve diğer etkinlikler ayıklanırken kullanmanızı öneririz. İşlem hattı hata ayıklaması sonra normal işlemlerde kullanmak istediğiniz gerçek klasörleri geçin.

## <a name="visualizing-debug-runs"></a>Görselleştirildiği hata ayıklama çalıştırır

Veri fabrikanızın tek bir yerde sürmekte olan tüm hata ayıklama çalıştırmalarını görselleştirebilirsiniz. Seçin **görünümü hata ayıklama çalıştıran** sayfanın sağ üst köşesindeki. Bu özellik, burada alt işlem hatları için hata ayıklama çalıştırmalarını kapalı başlatılmadan ana işlem hatları sahip ve tek bir görünümde, tüm etkin hata ayıklama çalıştırmalarını görmek için istediğiniz senaryolarda yararlıdır.

![Etkin hata ayıklama çalıştırmalarını görüntüle simgesini seçin](media/iterative-development-debugging/view-debug-runs-image1.png)

![Etkin hata ayıklama çalıştırmalarını örnek listesi](media/iterative-development-debugging/view-debug-runs-image2.png)

## <a name="monitoring-debug-runs"></a>Hata ayıklama izleme çalıştırır

İle başlatılan çalıştırmalar **hata ayıklama** özellik listesinde kullanılabilir olmayan **İzleyici** sekmesi. Bkz: ile tetiklenen çalışır yalnızca **şimdi Tetikle**, **zamanlama**, veya **atlayan pencere** içinde tetikler **İzleyici** sekmesi. İle başlatılan son test gördüğünüz **hata ayıklama** özelliği **çıkış** işlem hattı tuvalinde penceresi.

## <a name="setting-breakpoints-for-debugging"></a>Hata ayıklama için kesme noktaları ayarlama

Data Factory işlem hattı tuvalinde belirli bir etkinliğe ulaşana kadar hata ayıklama da olanak tanır. Henüz bir kesme noktası kadar istediğiniz test ve etkinliği koyduğunuz **hata ayıklama**. Veri fabrikası, test yalnızca kesme noktası etkinlik kadar işlem hattı tuvalinde çalıştırmasını sağlar. Bu *hata ayıklamak kadar* özelliği, işlem hattının tamamı, ancak yalnızca bir alt işlem hattı içindeki etkinliklerin test istemediğiniz zaman yararlıdır.

![İşlem hattı tuvalinde kesme noktaları](media/iterative-development-debugging/iterative-development-image4.png)

Bir kesme noktası ayarlamak için işlem hattı tuvalinde bir öğe seçin. A *hata ayıklamak kadar* olarak boş bir kırmızı halka öğe sağ üst köşesindeki seçeneği görüntülenir.

![Seçilen öğe üzerinde bir kesme noktası ayarlamadan önce](media/iterative-development-debugging/iterative-development-image5.png)

Seçtikten sonra *hata ayıklamak kadar* seçeneği, kesme noktası etkin belirtmek için doldurulmuş kırmızı bir daire için değiştirir.

![Seçilen öğe üzerinde bir kesme noktası ayarladıktan sonra](media/iterative-development-debugging/iterative-development-image6.png)

## <a name="next-steps"></a>Sonraki adımlar
[Sürekli tümleştirme ve dağıtım Azure Data factory'de](continuous-integration-deployment.md)
