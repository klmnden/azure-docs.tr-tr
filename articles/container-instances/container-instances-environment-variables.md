---
title: Azure kapsayıcı durumlarda ortam değişkenlerini ayarlama
description: Azure kapsayıcı durumlarda ortam değişkenlerini ayarlama öğrenin
services: container-instances
author: david-stanford
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 03/13/2018
ms.author: dastanfo
ms.openlocfilehash: 37fde41b6dc2ea0a4d3b4b38a0e3df81a297c125
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="set-environment-variables"></a>Ortam değişkenlerini belirleme

Kapsayıcı örneklerinizi ortam değişkenlerini ayarlama, uygulamanın veya komut dosyasını çalıştır kapsayıcı tarafından dinamik yapılandırma sağlamanıza olanak verir.

Ortam değişkenleri CLI ve PowerShell şu anda ayarlayabilir. Her iki durumda da, bir bayrak komutlarını ortam değişkenleri ayarlamak için kullanırsınız. İçinde ACI ortam değişkenlerini ayarlama benzer `--env` komut satırı bağımsız değişkeni `docker run`. Örneğin, aci/microsoft-wordcount kapsayıcı görüntü kullanırsanız aşağıdaki ortam değişkenleri belirterek davranışını değiştirebilirsiniz:

*NumWords*: STDOUT gönderilen sözcük sayısı.

*MinLength*: en az bir sözcük, sayılması için karakter sayısı. Sık kullanılan sözcük gibi "," ve "." daha yüksek bir sayı yoksayar

## <a name="azure-cli-example"></a>Azure CLI örneği

Kapsayıcı çıktısını varsayılan görmek için aşağıdaki komutu çalıştırın:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer1 \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure
```

Belirterek `NumWords=5` ve `MinLength=8` kapsayıcının ortam değişkenleri için farklı bir çıkış kapsayıcı günlükleri görüntülenmelidir.

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer2 \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure \
    --environment-variables NumWords=5 MinLength=8
```

Kapsayıcı durumu olarak gösterilir sonra *kesildi* (kullanmak [az kapsayıcı Göster] [ az-container-show] durumunu denetlemek için), çıktı görmek için günlükleri görüntüleme.  Ortam değişkenleri ayarlanmadı--kapsayıcıyla çıktısını görüntülemek için mycontainer2 yerine mycontainer1 olarak adlandırın.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer2
```

## <a name="azure-powershell-example"></a>Azure PowerShell örneği

Kapsayıcı çıktısını varsayılan görmek için aşağıdaki komutu çalıştırın:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer1 \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure
```

Belirterek `NumWords=5` ve `MinLength=8` kapsayıcının ortam değişkenleri için farklı bir çıkış kapsayıcı günlükleri görüntülenmelidir.

```azurepowershell-interactive
$envVars = @{NumWord=5;MinLength=8}
New-AzureRmContainerGroup `
    -ResourceGroupName myResourceGroup `
    -Name mycontainer2 `
    -Image microsoft/aci-wordcount:latest `
    -RestartPolicy OnFailure `
    -EnvironmentVariable $envVars
```

Kapsayıcı durum olduğunda *kesildi* (kullanmak [Get-AzureRmContainerInstanceLog] [ azure-instance-log] durumunu denetlemek için), çıktı görmek için günlükleri görüntüleme. Kapsayıcı görüntülemek için hiç ortam değişkeni günlükleriyle ContainerGroupName mycontainer2 yerine mycontainer1 olacak şekilde ayarlayın.

```azurepowershell-interactive
Get-AzureRmContainerInstanceLog `
    -ResourceGroupName myResourceGroup `
    -ContainerGroupName mycontainer2
```

## <a name="example-output-without-environment-variables"></a>Ortam değişkenleri olmadan örnek çıkış

```bash
[('the', 990),
 ('and', 702),
 ('of', 628),
 ('to', 610),
 ('I', 544),
 ('you', 495),
 ('a', 453),
 ('my', 441),
 ('in', 399),
 ('HAMLET', 386)]
```

## <a name="example-output-with-environment-variables"></a>Ortam değişkenleri ile örnek çıkış

```bash
[('CLAUDIUS', 120),
 ('POLONIUS', 113),
 ('GERTRUDE', 82),
 ('ROSENCRANTZ', 69),
 ('GUILDENSTERN', 54)]
```

## <a name="next-steps"></a>Sonraki adımlar

Giriş, kapsayıcıya özelleştirmeyi bildiğinize göre tamamlanıncaya kadar çalışabilmesi kapsayıcıları çıktısını kalıcı öğrenin.
> [!div class="nextstepaction"]
> [Azure kapsayıcı örneği ile bir Azure dosya paylaşımı oluşturma](container-instances-mounting-azure-files-volume.md)

<!-- LINKS Internal -->
[azure-cloud-shell]: ../cloud-shell/overview.md
[azure-cli-install]: /cli/azure/
[azure-powershell-install]: /powershell/azure/install-azurerm-ps
[azure-instance-log]: /powershell/module/azurerm.containerinstance/get-azurermcontainerinstancelog
[az-container-show]: /cli/azure/container?view=azure-cli-latest#az_container_show