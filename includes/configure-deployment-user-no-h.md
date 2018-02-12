---
title: "include dosyası"
description: "include dosyası"
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 02/02/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 463e8b0122339831d5d6a65b6e41d4f697a82013
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2018
---
Cloud Shell içinde, [`az webapp deployment user set`](/cli/azure/webapp/deployment/user?view=azure-cli-latest#az_webapp_deployment_user_set) komutuyla dağıtım kimlik bilgileri oluşturun. Web uygulamasına FTP ve yerel Git dağıtımı için bu dağıtım kullanıcısı gereklidir. Kullanıcı adı ve parola, hesap düzeyindedir. _Bunlar Azure aboneliği kimlik bilgilerinizden farklıdır._

Şu örnekte, *\<username>* ve *\<password>* kısımlarını (köşeli ayraçlar dahil) yeni bir kullanıcı adı ve parola ile değiştirin. Kullanıcı adı Azure içinde benzersiz olmalıdır. Parola, en az sekiz karakter uzunluğunda olma ve şu üç öğeyi de içermelidir: harf, rakam, sembol. 

```azurecli-interactive
az webapp deployment user set --user-name <username> --password <password>
```

`null` olarak gösterilen parolayla bir JSON çıkışı almanız gerekir. `'Conflict'. Details: 409` hatası alırsanız kullanıcı adını değiştirin. ` 'Bad Request'. Details: 400` hatası alırsanız daha güçlü bir parola kullanın.

Bu dağıtım kullanıcısını bir kez oluşturduktan sonra tüm Azure dağıtımlarınız için kullanabilirsiniz.

> [!NOTE]
> Kullanıcı adını ve parolayı kaydedin. Daha sonra web uygulamasının dağıtımı için bunlara ihtiyacınız olacaktır.
>
>
