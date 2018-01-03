---
title: "Azure Service Fabric CLI betik örnek dağıtma"
description: "Azure Service Fabric CLI kullanarak Azure'da güvenli bir Service Fabric Linux kümesi oluşturun."
services: service-fabric
documentationcenter: 
author: rwike77
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 09/18/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 571d1d599508f09aff4dde27dffcf0f8e3c6a7d3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-secure-service-fabric-linux-cluster-in-azure"></a>Güvenli bir Service Fabric Linux kümesi oluşturma

Bu komutu otomatik olarak imzalanan bir sertifika oluşturur, bir anahtar Kasası'na ekler ve sertifika yerel olarak indirir.  Yeni sertifika, dağıtırken küme güvenliğini sağlamak için kullanılır.  Yeni bir tane oluşturmak yerine var olan bir sertifikayı da kullanabilirsiniz.  Her iki durumda da, Service Fabric kümesi erişmek için kullandığınız etki alanı sertifikanın konu adı eşleşmelidir. Bu eşleme kümenin HTTPS yönetim uç noktaları ve Service Fabric Explorer için bir SSL sağlamak için gereklidir. Bir CA için bir SSL sertifikası elde edemiyor `.cloudapp.azure.com` etki alanı. Özel etki alanı adı, kümeniz için edinmeniz gerekir. Bir CA'dan bir sertifika isteme, sertifikanın konu adı, kümeniz için kullandığınız özel etki alanı adı eşleşmelidir.

Gerekirse, yükleme [Azure CLI 2.0](/en-us/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="sample-script"></a>Örnek komut dosyası

[!code-sh[main](../../../cli_scripts/service-fabric/create-cluster/create-cluster.sh "Deploy an application to a cluster")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Komut dosyası örneği çalıştırdıktan sonra kaynak grubu, küme ve tüm ilgili kaynaklar kaldırmak için aşağıdaki komutu kullanılabilir.

```azurecli
ResourceGroupName = "aztestclustergroup"
az group delete --name $ResourceGroupName
```

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut dosyasını aşağıdaki komutları kullanır. Komut belirli belgeleri tablo bağlanan her komut.

| Komut | Notlar |
|---|---|
| [az BT kümesi oluşturma](https://docs.microsoft.com/en-us/cli/azure/sf/cluster?view=azure-cli-latest#az_sf_cluster_create) | Yeni bir Service Fabric kümesi oluşturur.  |

## <a name="next-steps"></a>Sonraki adımlar

Azure Service Fabric için ek hizmet doku CLI örnek bulunabilir [Service Fabric CLI örnekleri](../samples-cli.md).
