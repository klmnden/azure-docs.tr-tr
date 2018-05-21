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
ms.openlocfilehash: 01b830f8a5e6e5f957b36bc2d6ef035754c06157
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
### <a name="run-the-service"></a>Hizmetin çalıştırılması

1. F5 isabet (veya türü `azds up` Terminal penceresinde) hizmetini çalıştırmak için. Hizmet otomatik olarak yeni seçilen alanınızda çalışacak `scott`. 
1. Hizmetinizi çalıştırarak kendi alanı çalıştığını onaylayın `azds resource list` yeniden. İlk olarak, bir örneğini göreceksiniz `mywebapi` şu anda çalışıyor `scott` alanı (çalışan sürüm `default` hala çalışıyor, ancak listede olmayan). İkincisi, erişim noktası için URL `webfrontend` metinle önekli *scott.s.*. Bu URL için benzersiz olan `scott` boşluk ve "Tan URL'ye" gönderilen istekleri Hizmetleri'nde ilk yol dener güveninin `scott` boşluk ve Hizmetleri için döner `default` alanı.

```
Name         Space     Chart              Ports   Updated     Access Points
-----------  --------  -----------------  ------  ----------  -------------
mywebapi     scott     mywebapi-0.1.0     80/TCP  15s ago     http://localhost:61466
webfrontend  default  webfrontend-0.1.0  80/TCP  5h ago      http://scott.s.webfrontend-contosodev.1234abcdef.eastus.aksapp.io
```

![](../media/common/space-routing.png)

Test Azure Dev alanları etkinleştirir, bu yerleşik yetenek yerlerini Hizmetleri'nde tam yığını yeniden oluşturmak her geliştirici gerek kalmadan baştan sona paylaşılan bir ortamda kod. Bu yönlendirme iletme yayma üst bilgiler, uygulama kodunuzda bu kılavuzun önceki adımda gösterildiği gibi gerektirir.

## <a name="test-code-in-a-space"></a>Boşluk Testi kodu
Yeni sürümünü test etmek için `mywebapi` birlikte `webfrontend`, webfrontend genel erişim noktası URL'si tarayıcınızı açın ve hakkında sayfasına gidin. Yeni, ileti görürsünüz.

Şimdi, "scott.s." Kaldır bölümü, URL ve tarayıcıyı yenileyin. Eski davranışı görmeniz gerekir (tarafından sergilenen `mywebapi` çalışır durumda sürüm `default`)