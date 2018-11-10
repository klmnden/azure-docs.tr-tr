---
title: Azure Özel Görüntü İşleme nedir?
titlesuffix: Azure Cognitive Services
description: Özel Görüntü İşleme Hizmeti'ni kullanarak Azure bulutunda özel görüntü sınıflandırıcıları oluşturmayı öğrenin.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: overview
ms.date: 10/26/2018
ms.author: anroth
ms.openlocfilehash: 5e8e675b32bfd4c741b82b1ab341a80adbb0529d
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2018
ms.locfileid: "50741649"
---
# <a name="what-is-azure-custom-vision"></a>Azure Özel Görüntü İşleme nedir?

Azure Özel Görüntü İşleme API'si, özel görüntü sınıflandırıcıları oluşturmanıza, dağıtmanıza ve geliştirmenize olanak sağlayan bir bilişsel hizmettir. Görüntü sınıflandırıcı, görüntüleri belirli özelliklerine göre sınıflara (etiketlere) ayıran bir yapay zeka hizmetidir. Özel Görüntü İşleme hizmeti, [Görüntü İşleme](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home) hizmetinden farklı olarak kendi sınıflandırmalarınızı oluşturmanıza imkan tanır.

## <a name="what-it-does"></a>Ne yapar?

Özel Görüntü İşleme hizmeti, görüntüleri sınıflandırmak için bir makine öğrenmesi algoritması kullanır. Geliştirici olarak söz konusu sınıflandırmaları içeren ve içermeyen görüntü grupları göndermeniz gerekir. Gönderme sırasında görüntülerin doğru etiketlerini belirtirsiniz. Ardından algoritma bu verilere göre eğitilir ve aynı verilerle kendini test ederek doğruluk oranını hesaplar. Model eğitildikten sonra uygulamanızın ihtiyaçlarına göre yeni görüntüler sınıflandırmak üzere test edebilir, yeniden eğitebilir ve istediğiniz duruma geldiğinde kullanabilirsiniz. İsterseniz modeli çevrimdışı kullanmak üzere dışarı aktarabilirsiniz.

### <a name="classification-and-object-detection"></a>Sınıflandırma ve nesne algılama

Özel Görüntü İşleme hizmeti iki bölüme ayrılabilir. **Görüntü sınıflandırma**, her görüntüyü belirli sınıflara atar. **Nesne algılama** da benzer bir özelliktir ancak uygulanan etiketlerin görüntünün neresinde olduğunu da koordinatlarla döndürür.

### <a name="optimization"></a>İyileştirme

Özel Görüntü İşleme Hizmetinin kullandığı yöntemler genellikle değişikliklerden fazla etkilenmez ve bu sayede az miktarda veriyle prototip çalışmalarına başlamanızı sağlar. Başlangıç için etiket başına 50 görüntü genellikle yeterli olacaktır. Ancak bu durumda hizmet, görüntülerdeki küçük farklılıkları algılamak (kalite denetimi senaryolarında küçük çatlakların veya eziklerin algılanması gibi) için en uygun durumda olmayacaktır.

Ayrıca önemli yerler veya perakende eşyalar gibi belirli malzemeler için iyileştirilmiş Özel Görüntü İşleme algoritmalarından birini de seçebilirsiniz. Bu konuda daha fazla bilgi için [Sınıflandırıcı oluşturma](getting-started-build-a-classifier.md) belgesine bakın.

## <a name="what-it-includes"></a>Neleri içerir
Özel Görüntü İşleme Hizmeti, yerel SDK'lara ek olarak [Özel Görüntü İşleme giriş sayfası](https://customvision.ai/) üzerinden web tabanlı arabirim aracılığıyla da sunulmaktadır. İstediğiniz arabirimi veya ikisini birlikte kullanarak model oluşturabilir, test edebilir ve eğitebilirsiniz.

![Chrome tarayıcı penceresinde Özel Görüntü İşleme giriş sayfası](media/browser-home.png)

## <a name="next-steps"></a>Sonraki adımlar

Web üzerinde Özel Görüntü İşleme hizmetini kullanmaya başlamak için [Sınıflandırıcı oluşturma](getting-started-build-a-classifier.md) kılavuzunu izleyin veya kod kullanılan bir senaryoyu uygulamak için [Görüntü sınıflandırma öğreticisini](csharp-tutorial.md) tamamlayın.
