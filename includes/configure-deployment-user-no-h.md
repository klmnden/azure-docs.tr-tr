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
ms.openlocfilehash: b5512ed5810ebd05596dc3ca7a29957b4c595c59
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50035269"
---
Cloud Shell içinde, [`az webapp deployment user set`](/cli/azure/webapp/deployment/user?view=azure-cli-latest#az-webapp-deployment-user-set) komutuyla dağıtım kimlik bilgileri oluşturun. Web uygulamasına FTP ve yerel Git dağıtımı için bu dağıtım kullanıcısı gereklidir. Kullanıcı adı ve parola, hesap düzeyindedir. _Bunlar Azure aboneliği kimlik bilgilerinizden farklıdır._

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
