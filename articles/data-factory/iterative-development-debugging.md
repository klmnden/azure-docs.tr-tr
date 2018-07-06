---
title: Yinelemeli geliştirme ve Azure Data Factory'de hata ayıklama | Microsoft Docs
description: Geliştirme ve Data Factory işlem hattı çalıştırmalarınızı Azure Portalı'nda hata ayıklama hakkında bilgi edinin.
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
ms.openlocfilehash: a4d3f991dbba8a686c7242aabff11d9228300777
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37865174"
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

Test çalıştırmaları, seçtiğiniz önce yaptığınız değişiklikleri data factory'de yayımlamak zorunda değilsiniz **hata ayıklama**. Bu değişiklikleri veri fabrikası iş akışı güncelleştirmeden önce beklendiği gibi çalıştığından emin olmak için istediğiniz senaryolarda faydalıdır.

## <a name="more-info-about-debugging"></a>Hata ayıklama hakkında daha fazla bilgi

1. İle başlatılan çalıştırmalar **hata ayıklama** özellik listesinde kullanılabilir olmayan **İzleyici** sekmesi. Bkz: ile tetiklenen çalışır yalnızca **şimdi Tetikle**, **zamanlama**, veya **atlayan pencere** içinde tetikler **İzleyici** sekmesi. İle başlatılan son test gördüğünüz **hata ayıklama** özelliği **çıkış** işlem hattı tuvalinde penceresi.

2. Seçme **hata ayıklama** aslında işlem hattını çalıştırır. İşlem hattının kopyalama etkinliği içeriyorsa, bu nedenle, örneğin, test çalıştırması verileri kaynaktan hedefe kopyalar. Sonuç olarak, test klasörleri kopyalama etkinliklerinizi ve diğer etkinlikler ayıklanırken kullanmanızı öneririz. İşlem hattı hata ayıklaması sonra normal işlemlerde kullanmak istediğiniz gerçek klasörleri geçin.

## <a name="setting-breakpoints-for-debugging"></a>Hata ayıklama için kesme noktaları ayarlama

Data Factory işlem hattı tuvalinde belirli bir etkinliğe ulaşana kadar hata ayıklama da olanak tanır. Henüz bir kesme noktası kadar istediğiniz test ve etkinliği koyduğunuz **hata ayıklama**. Veri fabrikası, test yalnızca kesme noktası etkinlik kadar işlem hattı tuvalinde çalıştırmasını sağlar. Bu *hata ayıklamak kadar* özelliği, işlem hattının tamamı, ancak yalnızca bir alt işlem hattı içindeki etkinliklerin test istemediğiniz zaman yararlıdır.

![İşlem hattı tuvalinde kesme noktaları](media/iterative-development-debugging/iterative-development-image4.png)

Bir kesme noktası ayarlamak için işlem hattı tuvalinde bir öğe seçin. A *hata ayıklamak kadar* olarak boş bir kırmızı halka öğe sağ üst köşesindeki seçeneği görüntülenir.

![Seçilen öğe üzerinde bir kesme noktası ayarlamadan önce](media/iterative-development-debugging/iterative-development-image5.png)

Seçtikten sonra *hata ayıklamak kadar* seçeneği, kesme noktası etkin belirtmek için doldurulmuş kırmızı bir daire için değiştirir.

![Seçilen öğe üzerinde bir kesme noktası ayarladıktan sonra](media/iterative-development-debugging/iterative-development-image6.png)

## <a name="next-steps"></a>Sonraki adımlar
[Sürekli tümleştirme ve dağıtım Azure Data factory'de](continuous-integration-deployment.md)
