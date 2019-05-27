---
title: include dosyası
description: include dosyası
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 02/02/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: cd7fc7487a41979f37c9a55baeb0b8e172e808c4
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66133156"
---
Dağıtım kimlik bilgileri Azure Cloud Shell'de yapılandırma [ `az webapp deployment user set` ](/cli/azure/webapp/deployment/user?view=azure-cli-latest#az-webapp-deployment-user-set) komutu. Web uygulamasına FTP ve yerel Git dağıtımı için bu dağıtım kullanıcısı gereklidir. Kullanıcı adı ve parola, hesap düzeyindedir. _Bunlar, Azure aboneliği kimlik bilgilerinizden farklı._

Aşağıdaki örnekte, değiştirin  *\<kullanıcıadı >* ve  *\<parola >*, yeni kullanıcı adı ve parola köşeli ayraçlar dahil. Kullanıcı adı Azure içinde benzersiz olmalıdır. Parola en az sekiz karakter uzunluğunda olmalı, şu üç öğeyi sahip olmalıdır: harf, rakam ve semboller. 

```azurecli-interactive
az webapp deployment user set --user-name <username> --password <password>
```

Olarak gösterilen parolayla bir JSON çıkışı size `null`. `'Conflict'. Details: 409` hatası alırsanız kullanıcı adını değiştirin. `'Bad Request'. Details: 400` hatası alırsanız daha güçlü bir parola kullanın. Dağıtım kullanıcı adı değil içermelidir ' @' yerel Git gönderim için simge.

Bu dağıtım kullanıcısını yalnızca bir kez yapılandırın. Tüm Azure dağıtımlarınız için kullanabilirsiniz.

> [!NOTE]
> Kullanıcı adı ve parolayı kaydedin. Daha sonra web uygulamasının dağıtımı için bunlara ihtiyacınız olacaktır.
>
>
