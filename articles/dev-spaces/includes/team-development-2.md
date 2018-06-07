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
ms.openlocfilehash: f6245a97f5d94c90e022ac509b61da477f4d9494
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2018
ms.locfileid: "34823841"
---
### <a name="run-the-service"></a>Hizmetin çalıştırılması

1. F5 isabet (veya türü `azds up` Terminal penceresinde) hizmetini çalıştırmak için. Hizmet otomatik olarak yeni seçilen alanınızda çalışacak `scott`. 
1. Hizmetinizi çalıştırarak kendi alanı çalıştığını onaylayın `azds resource list` yeniden. İlk olarak, bir örneğini göreceksiniz `mywebapi` şu anda çalışıyor `scott` alanı (çalışan sürüm `default` alanı hala çalışıyor, ancak listede olmayan). İkinci olarak, erişim noktası için URL `webfrontend` şimdi ön ekine sahip `scott.s.`. Bu URL için benzersiz olan `scott` alanı ve "Tan URL'ye" gönderilen istekleri varsayılan Hizmetleri'nde yönlendirmek için deneyecek anlamına gelir `scott` alanı ve hiçbir özel ise `scott` örneği hizmet için istekleri döner geri Hizmetleri `default` alanı.

```
Name         Space     Chart              Ports   Updated     Access Points
-----------  --------  -----------------  ------  ----------  -------------
mywebapi     scott     mywebapi-0.1.0     80/TCP  15s ago     http://localhost:61466
webfrontend  default  webfrontend-0.1.0  80/TCP  5h ago      http://scott.s.webfrontend-contosodev.1234abcdef.eastus.aksapp.io
```

![](../media/common/space-routing.png)

Test Azure Dev alanları etkinleştirir, bu yerleşik yetenek yerlerini Hizmetleri'nde tam yığını yeniden oluşturmak her geliştirici gerek kalmadan baştan sona bir paylaşılan geliştirme alanında kod. Bu yönlendirme iletme yayma üst bilgiler, uygulama kodunuzda bu kılavuzun önceki adımda gösterildiği gibi gerektirir.

### <a name="test-code-in-a-space"></a>Boşluk Testi kodu
Yeni sürümünü test etmek için `mywebapi` ile `webfrontend`, webfrontend genel erişim noktası URL'si tarayıcınızı açın ve hakkında sayfasına gidin. Yeni, ileti görürsünüz.

Şimdi, kaldırma `scott.s.` parçası URL ve tarayıcıyı yenileyin. Eski davranışı görmeniz gerekir (tarafından sergilenen `mywebapi` çalışır durumda sürüm `default`)