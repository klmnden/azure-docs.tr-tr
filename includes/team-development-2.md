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
ms.openlocfilehash: 8580e19f1955becb2cfaee13742054af1a9edb7f
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47410467"
---
### <a name="run-the-service"></a>Hizmeti çalıştırma

1. Hizmeti çalıştırmak için F5'e basın (veya Terminal Penceresine `azds up` yazın). Hizmet, yeni seçtiğiniz alanda otomatik olarak çalıştırılır `default/scott`. 
1. Hizmetin kendi alanında çalıştırıldığını onaylamak için `azds list-up` komutunu yeniden çalıştırabilirsiniz. İlk olarak, bir `mywebapi` örneğinin `default/scott` alanında çalıştırıldığını görürsünüz (`default` içinde çalıştırılan sürüm yine çalışmaktadır ama listelenmez). `azds list-uris` komutunu çalıştırırsanız `webfrontend` için erişim noktası URL'sinin önüne "scott.s." metninin eklendiğini fark edeceksiniz. Bu URL, `default/scott` alanına özeldir. Özel URL, "scott URL" konumuna gönderilen isteklerin önce `default/scott` alanındaki hizmetlere yönlendirileceğini, ama bu başarısız olursa `default` alanındaki hizmetlere geri döneceğini belirtir.

```
Name                      DevSpace  Type     Updated  Status
------------------------  --------  -------  -------  -------
mywebapi                  scott     Service  3m ago   Running
mywebapi-bb4f4ddd8-sbfcs  scott     Pod      3m ago   Running
webfrontend               default   Service  26m ago  Running
```

```
Uri                                                            Status
-------------------------------------------------------------  ---------
http://localhost:53831 => mywebapi.scott:80                    Tunneled
http://webfrontend.6364744826e042319629.canadaeast.aksapp.io/  Available
```

![](../articles/dev-spaces/media/common/space-routing.png)

Azure Dev Spaces’ın bu yerleşik özelliği, her geliştiricinin alanlarındaki hizmetlerin tam yığınını yeniden oluşturmasına gerek kalmadan kodu paylaşılan bir alanda test etmenize olanak sağlar. Bu yönlendirme, bu kılavuzun önceki adımında gösterildiği gibi uygulama kodunuzun yayma üst bilgilerini iletmesini gerektirir.

### <a name="test-code-in-a-space"></a>Alanda kodu test etme
`webfrontend` ile `mywebapi` hizmetinin yeni sürümünü test etmek için, tarayıcınızı `webfrontend` için genel erişim noktası URL’sine açın ve Hakkında sayfasına gidin. Yeni iletinizin görüntülendiğini görmelisiniz.

Şimdi, URL'nin "scott.s." bölümünü kaldırın ve tarayıcıyı yenileyin. Eski davranışı görmelisiniz (`mywebapi` sürümü `default` olarak çalışırken).
