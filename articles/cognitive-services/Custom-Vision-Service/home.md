---
title: Özel Görüntü İşleme Hizmeti makine öğrenmesine genel bakış - Azure Bilişsel Hizmetler | Microsoft Docs
description: Özel Görüntü İşleme Hizmeti, Azure platformunda özel görüntü sınıflandırıcılar oluşturmanızı sağlayan bir Microsoft Bilişsel Hizmet bileşenidir.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: overview
ms.date: 05/02/2018
ms.author: anroth
ms.openlocfilehash: d2daf7c211f9474f5636b6af69c5b700d597aa14
ms.sourcegitcommit: f94f84b870035140722e70cab29562e7990d35a3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43285253"
---
# <a name="what-is-the-custom-vision-service"></a>Özel Görüntü İşleme Hizmeti nedir?

Özel Görüntü İşleme Hizmeti, özel görüntü sınıflandırıcılar oluşturmanızı sağlayan bir Microsoft Bilişsel Hizmet bileşenidir. Görüntü sınıflandırıcı oluşturma, dağıtma ve geliştirme işlemlerini oldukça kolay ve hızlı hale getirir. Özel Görüntü İşleme Hizmeti, görüntülerinizi yüklemek ve sınıflandırıcıyı eğitmek için kullanabileceğiniz bir REST API'si ve web arabirimi sunar.

## <a name="what-does-custom-vision-service-do-well"></a>Özel Görüntü İşleme Hizmeti hangi konularda iyidir?

Özel Görüntü İşleme Hizmeti, en iyi performansı sınıflandırmaya çalıştığınız öğe görüntüde baskın olduğunda sunar. 

Sınıflandırıcı veya algılayıcı oluşturmak için birkaç görüntü kullanılması gerekir. Prototipinizi başlatmak için sınıf başına 50 görüntü yeterli olacaktır. Özel Görüntü İşleme Hizmetinin kullandığı metotlar, değişikliklerden fazla etkilenmez ve bu sayede az miktarda veriyle prototip çalışmalarına başlamanızı sağlar. Bu da Özel Görüntü İşleme Hizmetinin küçük farkları algılamak istediğiniz senaryolar için pek uygun olmadığı anlamına gelir. Buna örnek olarak kalite denetimi senaryolarındaki küçük çatlaklar veya göçükler verilebilir.

## <a name="next-steps"></a>Sonraki adımlar

[Sınıflandırıcı oluşturmayı öğrenin](getting-started-build-a-classifier.md)
