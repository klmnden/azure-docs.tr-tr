---
title: "Azure CLI toplu işleminde bir uygulama eklemek örnek - komut dosyası | Microsoft Docs"
description: "Azure CLI toplu işleminde bir uygulama eklemek örnek - komut dosyası"
services: batch
documentationcenter: 
author: annatisch
manager: daryls
editor: tysonn
ms.assetid: 
ms.service: batch
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/02/2017
ms.author: antisch
ms.openlocfilehash: cbfe8ab565ecf7f298a9a6c0f0c8298c675f178c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="adding-applications-to-azure-batch-with-azure-cli"></a>Azure CLI ile Azure Batch uygulamalarına ekleme

Bu komut, bir uygulama için bir Azure Batch havuzu ya da görev ile kullanmak üzere ayarlamak gösterilmiştir. Bir uygulamayı kurmak için bir .zip dosyasına tüm bağımlılıkları ile birlikte, yürütülebilir paketi. Bu örnekte yürütülebilir zip dosyası olarak adlandırılır ' my-uygulama-exe.zip'.

## <a name="prerequisites"></a>Ön koşullar

- Sağlanan yönergeleri kullanarak Azure CLI yükleme [Azure CLI Yükleme Kılavuzu'na](https://docs.microsoft.com/cli/azure/install-azure-cli)zaten yapmadıysanız,.
- Zaten yoksa, bir toplu işlem hesabı oluşturun. Bkz: [Azure CLI ile bir toplu işlem hesabı oluşturun](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) bir hesap oluşturan bir örnek komut dosyası için.

## <a name="sample-script"></a>Örnek komut dosyası

[!code-azurecli[main](../../../cli_scripts/batch/add-application/add-application.sh "Add Application")]

## <a name="clean-up-application"></a>Uygulamayı oluşturan Temizle

Yukarıdaki örnek komut dosyasını çalıştırdıktan sonra uygulama ve onun karşıya yüklenen uygulama paketleri kaldırmak için aşağıdaki komutları çalıştırın.

```azurecli
az batch application package delete -g myresourcegroup -n mybatchaccount --application-id myapp --version 1.0 --yes
az batch application delete -g myresourcegroup -n mybatchaccount --application-id myapp --yes
```

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut, bir uygulama oluşturmak ve bir uygulama paketini karşıya yüklemek için aşağıdaki komutları kullanır.
Komut özgü belgelere Tablo bağlantıları her komut.

| Komut | Notlar |
|---|---|
| [az batch uygulaması oluşturma](https://docs.microsoft.com/cli/azure/batch/application#az_batch_application_create) | Bir uygulama oluşturur.  |
| [az toplu uygulama ayarlama](https://docs.microsoft.com/cli/azure/batch/application#az_batch_application_set) | Bir uygulamanın özelliklerini güncelleştirir.  |
| [az toplu uygulama paketi oluşturma](https://docs.microsoft.com/cli/azure/batch/application/package#az_batch_application_package_create) | Belirtilen uygulama için bir uygulama paketi ekler.  |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/overview).

Ek toplu CLI kod örnekleri bulunabilir [Azure Batch CLI belgelerine](../batch-cli-samples.md).
