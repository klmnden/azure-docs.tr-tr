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
ms.openlocfilehash: 85f8632aae8a70b1282155881dbca6b25734a6c5
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36936406"
---
### <a name="run-the-service"></a>Hizmetin çalıştırılması

1. F5 isabet (veya türü `azds up` Terminal penceresinde) hizmetini çalıştırmak için. Hizmet otomatik olarak yeni seçilen alanınızda çalışacak `default/scott`. 
1. Hizmetinizi çalıştırarak kendi alanı çalıştığını onaylayın `azds list` yeniden. İlk olarak, bir örneğini göreceksiniz `mywebapi` şu anda çalışıyor `default/scott` alanı (çalışan sürüm `default` hala çalışıyor, ancak listede olmayan). İkincisi, erişim noktası için URL `webfrontend` "scott.s." metni ile önek. Bu URL için benzersiz olan `default/scott` alanı. "Tan URL'ye" gönderilen istekleri Hizmetleri'nde ilk yol çalışılacak özel URL güveninin `default/scott` , başarısız olursa, ancak alanı, bunlar döner geri Hizmetleri'nde `default` alanı.

```
Name         Space          Chart              Ports   Updated     Access Points
-----------  --------       -----------------  ------  ----------  -------------
mywebapi     default/scott  mywebapi-0.1.0     80/TCP  15s ago     http://localhost:61466
webfrontend  default        webfrontend-0.1.0  80/TCP  5h ago      http://scott.s.webfrontend-contosodev.1234abcdef.eastus.aksapp.io
```

![](../media/common/space-routing.png)

Bu yerleşik özellik Azure Dev alanları yerlerini Hizmetleri'nde tam yığını yeniden oluşturmak her geliştirici gerek kalmadan kodu paylaşılan bir alanı test olanak sağlar. Bu yönlendirme iletme yayma üst bilgiler, uygulama kodunuzda bu kılavuzun önceki adımda gösterildiği gibi gerektirir.

### <a name="test-code-in-a-space"></a>Boşluk Testi kodu
Yeni sürümünü test etmek için `mywebapi` ile `webfrontend`, genel erişim noktası URL'si tarayıcınızı açın `webfrontend` ve hakkında sayfasına gidin. Yeni, ileti görürsünüz.

Şimdi, "scott.s." Kaldır bölümü, URL ve tarayıcıyı yenileyin. Eski davranışı görmeniz gerekir (ile `mywebapi` çalışır durumda sürüm `default`)
