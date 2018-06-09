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
ms.openlocfilehash: 78dca327a470394d19e6befc6578abf2d499850c
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35247594"
---
### <a name="run-the-service"></a>Hizmetin çalıştırılması

1. F5 isabet (veya türü `azds up` Terminal penceresinde) hizmetini çalıştırmak için. Hizmet otomatik olarak yeni seçilen alanınızda çalışacak `scott`. 
1. Hizmetinizi çalıştırarak kendi alanı çalıştığını onaylayın `azds space list` yeniden. İlk olarak, bir örneğini göreceksiniz `mywebapi` şu anda çalışıyor `scott` alanı (çalışan sürüm `default` hala çalışıyor, ancak listede olmayan). İkincisi, erişim noktası için URL `webfrontend` "scott.s." metni ile önek. Bu URL için benzersiz olan `scott` alanı. "Tan URL'ye" gönderilen istekleri Hizmetleri'nde ilk yol çalışılacak özel URL güveninin `scott` , başarısız olursa, ancak alanı, bunlar döner geri Hizmetleri'nde `default` alanı.

```
Name         Space     Chart              Ports   Updated     Access Points
-----------  --------  -----------------  ------  ----------  -------------
mywebapi     scott     mywebapi-0.1.0     80/TCP  15s ago     http://localhost:61466
webfrontend  default  webfrontend-0.1.0  80/TCP  5h ago      http://scott.s.webfrontend-contosodev.1234abcdef.eastus.aksapp.io
```

![](../media/common/space-routing.png)

Bu yerleşik özellik Azure Dev alanları yerlerini Hizmetleri'nde tam yığını yeniden oluşturmak her geliştirici gerek kalmadan kodu paylaşılan bir alanı test olanak sağlar. Bu yönlendirme iletme yayma üst bilgiler, uygulama kodunuzda bu kılavuzun önceki adımda gösterildiği gibi gerektirir.

### <a name="test-code-in-a-space"></a>Boşluk Testi kodu
Yeni sürümünü test etmek için `mywebapi` ile `webfrontend`, genel erişim noktası URL'si tarayıcınızı açın `webfrontend` ve hakkında sayfasına gidin. Yeni, ileti görürsünüz.

Şimdi, "scott.s." Kaldır bölümü, URL ve tarayıcıyı yenileyin. Eski davranışı görmeniz gerekir (ile `mywebapi` çalışır durumda sürüm `default`)
