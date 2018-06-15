---
title: Hızlı Başlangıç - CLI 2.0 ile Azure Media Services hesabı oluşturma | Microsoft Docs
description: Azure Media Services hesabı oluşturmak için bu hızlı başlangıç adımlarını izleyin.
services: media-services
documentationcenter: ''
author: Juliako
manager: cflower
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/27/2018
ms.author: juliako
ms.openlocfilehash: f7e5cb28f90466e9366c0a32e2a333e6823b9396
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33779727"
---
# <a name="quickstart-create-an-azure-media-services-account"></a>Hızlı Başlangıç: Azure Media Services hesabı oluşturma

> [!NOTE]
> En son Azure Media Services (2018-03-30) sürümü önizleme aşamasındadır. Bu sürüm, v3 olarak da adlandırılır. 

İster geliştirici, isterse medya içeriği oluşturucusu olun, Azure’da medya içeriğini depolamak, şifrelemek, kodlamak, yönetmek ve akışa almak için bir Media Services hesabı oluşturmanız gerekir. Media Services hesabı oluştururken, bir Azure Depolama hesabı kaynağının kimliğini sağlamanız gerekir. Belirtilen depolama hesabı, Media Services hesabınıza eklenir. Bu depolama hesabı kaynağının, Media Services hesabıyla aynı coğrafi bölgede bulunması gerekir.  

Bu hızlı başlangıçta, CLI 2.0 kullanılarak yeni bir Azure Media Services hesabı oluşturma adımları açıklanmaktadır.  

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="log-in-to-azure"></a>Azure'da oturum açma

[Azure portalında](http://portal.azure.com) oturum açın ve **CloudShell** başlatarak sonraki adımlarda gösterildiği gibi CLI komutlarını yürütün.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu başlığı için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Kullandığınız sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

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

Tek bir **Birincil** depolama hesabınız olması gerekir, ancak Media Services hesabınızla ilişkili istediğiniz sayıda **İkincil** depolama hesabınız olabilir. Media Services, **General-purpose v2** (GPv2) veya **General-purpose v1** (GPv1) hesaplarını destekler. Yalnızca blob hesaplarının **Birincil** olmasına izin verilmez. Depolama hesapları hakkında daha fazla bilgi edinmek istiyorsanız bkz. [Azure Depolama hesap seçenekleri](../../storage/common/storage-account-options.md). 

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
