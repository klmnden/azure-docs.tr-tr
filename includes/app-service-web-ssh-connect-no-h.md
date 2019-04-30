---
title: include dosyası
description: include dosyası
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 03/29/2019
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 060bc1039982cc0a77214d5dbe2a08de7a839c84
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60850247"
---
Kapsayıcınızı ile doğrudan bir SSH oturumu açılmasını sağlamak için uygulamanızı çalıştırıyor olmalıdır.

Aşağıdaki URL'yi değiştirin ve tarayıcı yapıştırın \<-adı > Uygulama adınız:

```
https://<app-name>.scm.azurewebsites.net/webssh/host
```

Henüz doğrulandıktan bağlanmak için Azure aboneliğiniz ile kimlik doğrulaması isteniyor. Kimlik doğrulandıktan sonra bir tarayıcı içi Kabuk burada kapsayıcınızın komutları çalıştırabilirsiniz görürsünüz.

![SSH bağlantısı](./media/app-service-web-ssh-connect-no-h/app-service-linux-ssh-connection.png)
