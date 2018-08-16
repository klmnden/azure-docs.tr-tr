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
ms.openlocfilehash: 875dd9d768b5f40f2d4ed3fad5b14a6cd6f6f6ba
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2018
ms.locfileid: "40129372"
---
### <a name="sign-in-to-azure-cli"></a>Azure CLI'da oturum açma
Azure'da oturum açın. Bir terminal penceresine aşağıdaki komutu yazın:

```cmd
az login
```

> [!Note]
> Azure aboneliğiniz yoksa [ücretsiz hesap](https://azure.microsoft.com/free) oluşturabilirsiniz.

#### <a name="if-you-have-multiple-azure-subscriptions"></a>Birden çok Azure aboneliğiniz varsa...
Şunu çalıştırarak aboneliklerinizi görüntüleyebilirsiniz: 

```cmd
az account list
```
JSON çıkışında `isDefault: true` bulunan aboneliği bulun.
Kullanmak istediğiniz abonelik bu değilse, varsayılan aboneliği değiştirebilirsiniz:

```cmd
az account set --subscription <subscription ID>
```
