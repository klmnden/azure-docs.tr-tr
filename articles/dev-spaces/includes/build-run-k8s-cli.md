---
title: include dosyası
description: include dosyası
ms.custom: include file
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: include
manager: douge
ms.openlocfilehash: 1cf301ab3b4430c7bb79f6fdd72243f9f2ad1d9b
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
## <a name="build-and-run-code-in-kubernetes"></a>Derleme ve içinde Kubernetes kodu çalıştırma
Şimdi kodumuza çalıştırın! Terminal penceresinde, bu komutu çalıştırmak **kök kod klasörü**, webfrontend:

```cmd
azds up
```

Komutunun çıktısını üzerinde takip, ilerledikçe birkaç şey göreceksiniz:
- Kaynak kodu Azure geliştirme ortamında için eşitlenmedi.
- Bir kapsayıcı görüntüsü kod klasörünüzdeki Docker varlıklar tarafından belirtilen Azure üzerinde oluşturulmuştur.
- Kubernetes nesneleri belirtilen kapsayıcı görüntüsü kod klasörünüzdeki Helm grafik tarafından kullanan oluşturulur.
- Kapsayıcının uç ilgili bilgiler görüntülenir. Örneğimizde, biz ortak bir HTTPS URL'si bekliyorsunuz.
- Yukarıdaki aşamalarını tamamlamak başarıyla varsayıldığında, size görmek başlaması gereken `stdout` (ve `stderr`) kapsayıcı başlatıldığında gibi çıktı.

> [!Note]
> Bu adımları ilk kez daha uzun sürer `up` komutunu çalıştırın, ancak sonraki çalıştırır daha hızlı.

## <a name="test-the-web-app"></a>Web uygulamasını test etme
Konsol çıkışı tarafından oluşturulan ortak URL hakkında bilgi için tarama `up` komutu. Biçiminde olacaktır: 

`Running at public URL: https://<servicename>-<environmentname>.<guid>.<region>.aksapp.io` 

Bu URL'yi bir tarayıcı penceresi açın ve yük web uygulaması görmeniz gerekir. Kapsayıcı yürütür gibi `stdout` ve `stderr` terminal penceresi çıkış akışı.
