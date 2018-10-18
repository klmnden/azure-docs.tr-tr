---
title: Hızlı Başlangıç - Azure CLI ile Azure Media Services hesabı oluşturma | Microsoft Docs
description: Azure Media Services hesabı oluşturmak için bu hızlı başlangıç adımlarını izleyin.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: quickstart
ms.custom: mvc
ms.date: 10/15/2018
ms.author: juliako
ms.openlocfilehash: de54571308b737b9160a39ee4ba5d4b2d9f15775
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49376542"
---
# <a name="quickstart-create-an-azure-media-services-account"></a>Hızlı Başlangıç: Azure Media Services hesabı oluşturma

İster geliştirici, isterse medya içeriği oluşturucusu olun, Azure’da medya içeriğini depolamak, şifrelemek, kodlamak, yönetmek ve akışa almak için bir Media Services hesabı oluşturmanız gerekir. Media Services hesabı oluştururken, bir Azure Depolama hesabı kaynağının kimliğini sağlamanız gerekir. Belirtilen depolama hesabı, Media Services hesabınıza eklenir. Bu depolama hesabı kaynağının, Media Services hesabıyla aynı coğrafi bölgede bulunması gerekir.  

Bu hızlı başlangıçta, Azure CLI kullanılarak yeni bir Azure Media Services hesabı oluşturma adımları açıklanmaktadır.  

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="log-in-to-azure"></a>Azure'da oturum açma

[Azure portalında](http://portal.azure.com) oturum açın ve **CloudShell** başlatarak sonraki adımlarda gösterildiği gibi CLI komutlarını yürütün.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu başlığı için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Kullandığınız sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme]( /cli/azure/install-azure-cli). 

## <a name="set-the-azure-subscription"></a>Azure aboneliğini ayarlama

Aşağıdaki komutta, Media Services hesabı için kullanmak istediğiniz Azure abonelik kimliğini sağlayın. [Abonelikler](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)’e giderek erişimine sahip olduğunuz aboneliklerin listesini görebilirsiniz.

```azurecli-interactive
az account set --subscription <mySubscriptionId>
```

## <a name="create-an-azure-resource-group"></a>Azure Kaynak Grubu oluşturma

Aşağıdaki komut, Depolama ve Media Services hesabınızın olmasını istediğiniz bir kaynak grubu oluşturur. *myresourcegroup* yer tutucusunu, kaynak grubunuz için kullanmak istediğiniz adla değiştirin.

```azurecli-interactive
az group create -n <myresourcegroup> -l westus2
```

## <a name="create-an-azure-storage-account"></a>Azure Depolama hesabı oluşturma

Media Services hesabı oluştururken, bir Azure Depolama hesabı kaynağının kimliğini sağlamanız gerekir. Belirtilen depolama hesabı, Media Services hesabınıza eklenir. 

Tek bir **Birincil** depolama hesabınız olması gerekir, ancak Media Services hesabınızla ilişkili istediğiniz sayıda **İkincil** depolama hesabınız olabilir. Media Services, **general-purpose v2** veya **general-purpose v1** hesaplarını destekler. Blob depolama hesaplarının **Birincil** olmasına izin verilmez. Depolama hesapları hakkında daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](../../storage/common/storage-account-overview.md). 

Aşağıdaki komut, Media Services Hesabı (birincil) ile ilişkilendirilecek Depolama hesabını oluşturur. Aşağıdaki betikte, *storageaccountforams* yer tutucusunu değiştirin. 'account_name', en fazla 24 karakter uzunluğunda olmalıdır.

```azurecli-interactive
az storage account create -n <storageaccountforams> -g <myresourcegroup>
```

## <a name="create-an-azure-media-services-account"></a>Azure Media Services hesabı oluşturma

Aşağıda, yeni bir Media Services hesabı oluşturan Azure CLI komutlarını bulabilirsiniz. Yalnızca aşağıdaki vurgulanan değerleri değiştirmeniz gerekir:

* *myamsaccountname*
* *myresourcegroup*
* *storageaccountforams*

```azurecli-interactive
az ams create -n <myamsaccountname> -g <myresourcegroup> --storage-account <storageaccountforams>
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu Hızlı Başlangıçta oluşturduğunuz Media Services hesabı dahil olmak üzere, kaynak grubunuzdaki hiçbir kaynağa artık ihtiyacınız yoksa kaynak grubunu silin.

**CloudShell**’de aşağıdaki komutu yürütün:

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir dosyayı akışa alma](stream-files-dotnet-quickstart.md)
