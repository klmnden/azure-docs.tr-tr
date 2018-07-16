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
ms.openlocfilehash: 2f2177b289ba9d13ba635bddcd61748e1e8ab0d1
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39063041"
---
### <a name="run-the-service"></a>Hizmeti çalıştırma

1. Hizmeti çalıştırmak için F5'e basın (veya Terminal Penceresine `azds up` yazın). Hizmet, yeni seçtiğiniz alanda otomatik olarak çalıştırılır `default/scott`. 
1. Hizmetin kendi alanında çalıştırıldığını onaylamak için `azds list-up` komutunu yeniden çalıştırabilirsiniz. İlk olarak, bir `mywebapi` örneğinin `default/scott` alanında çalıştırıldığını görürsünüz (`default` içinde çalıştırılan sürüm yine çalışmaktadır ama listelenmez). İkinci olarak, `webfrontend` için erişim noktası URL'sine önek olarak "scott.s." metni eklenir. Bu URL, `default/scott` alanına özeldir. Özel URL, "scott URL" konumuna gönderilen isteklerin önce `default/scott` alanındaki hizmetlere yönlendirileceğini, ama bu başarısız olursa `default` alanındaki hizmetlere geri döneceğini belirtir.

```
Name         Space          Chart              Ports   Updated     Access Points
-----------  --------       -----------------  ------  ----------  -------------
mywebapi     default/scott  mywebapi-0.1.0     80/TCP  15s ago     http://localhost:61466
webfrontend  default        webfrontend-0.1.0  80/TCP  5h ago      http://scott.s.webfrontend-contosodev.1234abcdef.eastus.aksapp.io
```

![](../media/common/space-routing.png)

Azure Dev Spaces’ın bu yerleşik özelliği, her geliştiricinin alanlarındaki hizmetlerin tam yığınını yeniden oluşturmasına gerek kalmadan kodu paylaşılan bir alanda test etmenize olanak sağlar. Bu yönlendirme, bu kılavuzun önceki adımında gösterildiği gibi uygulama kodunuzun yayma üst bilgilerini iletmesini gerektirir.

### <a name="test-code-in-a-space"></a>Alanda kodu test etme
`webfrontend` ile `mywebapi` hizmetinin yeni sürümünü test etmek için, tarayıcınızı `webfrontend` için genel erişim noktası URL’sine açın ve Hakkında sayfasına gidin. Yeni iletinizin görüntülendiğini görmelisiniz.

Şimdi, URL'nin "scott.s." bölümünü kaldırın ve tarayıcıyı yenileyin. Eski davranışı görüyor olmalısınız (`default`'ta çalışan `mywebapi` sürümü ile)
