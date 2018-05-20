---
title: Azure kapsayıcı durumlarda ortam değişkenlerini ayarlama
description: Azure kapsayıcı örnekleri çalıştırmak kapsayıcılarında ortam değişkenlerini ayarlama öğrenin
services: container-instances
author: mmacy
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 05/16/2018
ms.author: marsma
ms.openlocfilehash: 1a025ce647cb3c071a6549a433e6505b85409fdc
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="set-environment-variables"></a>Ortam değişkenlerini belirleme

Kapsayıcı örneklerinizi ortam değişkenlerini ayarlama, uygulamanın veya komut dosyasını çalıştır kapsayıcı tarafından dinamik yapılandırma sağlamanıza olanak verir. Bir kapsayıcıda ortam değişkenlerini ayarlamak için bir kapsayıcı örneği oluşturduğunuzda, bunları belirtin. Bir kapsayıcı ile başlattığınızda ortam değişkenlerini ayarlayabilirsiniz [Azure CLI](#azure-cli-example), [Azure PowerShell](#azure-powershell-example)ve [Azure portal](#azure-portal-example).

Örneğin, çalıştırırsanız [aci/microsoft-wordcount] [ aci-wordcount] kapsayıcı görüntüsü, aşağıdaki ortam değişkenleri belirterek davranışını değiştirebilirsiniz:

*NumWords*: STDOUT gönderilen sözcük sayısı.

*MinLength*: en az bir sözcük, sayılması için karakter sayısı. Sık kullanılan sözcük gibi "," ve "." daha yüksek bir sayı yoksayar

## <a name="azure-cli-example"></a>Azure CLI örneği

Varsayılan çıkışını görmek için [aci/microsoft-wordcount] [ aci-wordcount] çalıştırın önce bu kapsayıcı, [az kapsayıcı oluşturmak] [ az-container-create] komutu (Hayır Belirtilen ortam değişkenleri):

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer1 \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure
```

Çıktı değiştirmek için ikinci bir kapsayıcı ile başlangıç `--environment-variables` eklenen, bağımsız değişkeni için değerleri belirtme *NumWords* ve *MinLength* değişkenleri:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer2 \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure \
    --environment-variables NumWords=5 MinLength=8
```

Olarak hem kapsayıcıları durumunu gösterir sonra *kesildi* (kullanmak [az kapsayıcı Göster] [ az-container-show] durumunu denetlemek için), kendi günlükleri ile görüntülemek [az kapsayıcı günlükleri] [ az-container-logs] çıkışı görmek için.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer1
az container logs --resource-group myResourceGroup --name mycontainer2
```

Kapsayıcılar çıktısını Göster nasıl ortam değişkenlerini ayarlama ikinci kapsayıcının komut davranışını değiştiren.

```console
azureuser@Azure:~$ az container logs --resource-group myResourceGroup --name mycontainer1
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

azureuser@Azure:~$ az container logs --resource-group myResourceGroup --name mycontainer2
[('CLAUDIUS', 120),
 ('POLONIUS', 113),
 ('GERTRUDE', 82),
 ('ROSENCRANTZ', 69),
 ('GUILDENSTERN', 54)]
```

## <a name="azure-powershell-example"></a>Azure PowerShell örneği

PowerShell'de ortam değişkenlerini ayarlama CLI benzer, ancak kullanan `-EnvironmentVariable` komut satırı bağımsız değişkeni.

İlk olarak, başlatma [aci/microsoft-wordcount] [ aci-wordcount] varsayılan yapılandırması bu kapsayıcıda [yeni AzureRmContainerGroup] [ new-azurermcontainergroup]komutu:

```azurepowershell-interactive
New-AzureRmContainerGroup `
    -ResourceGroupName myResourceGroup `
    -Name mycontainer1 `
    -Image microsoft/aci-wordcount:latest
```

Şimdi aşağıdaki komutu çalıştırarak [yeni AzureRmContainerGroup] [ new-azurermcontainergroup] komutu. Bu bir belirtir *NumWords* ve *MinLength* bir dizi değişkeni doldurma sonra ortam değişkenleri `envVars`:

```azurepowershell-interactive
$envVars = @{NumWords=5;MinLength=8}
New-AzureRmContainerGroup `
    -ResourceGroupName myResourceGroup `
    -Name mycontainer2 `
    -Image microsoft/aci-wordcount:latest `
    -RestartPolicy OnFailure `
    -EnvironmentVariable $envVars
```

Her iki kapsayıcıları durum olduğunda *kesildi* (kullanmak [Get-AzureRmContainerInstanceLog] [ azure-instance-log] durumunu denetlemek için), kendi günlükleri ile çekme [ Get-AzureRmContainerInstanceLog] [ azure-instance-log] komutu.

```azurepowershell-interactive
Get-AzureRmContainerInstanceLog -ResourceGroupName myResourceGroup -ContainerGroupName mycontainer1
Get-AzureRmContainerInstanceLog -ResourceGroupName myResourceGroup -ContainerGroupName mycontainer2
```

Her kapsayıcı için çıktı, ortam değişkenleri kapsayıcı tarafından çalıştırılmasını betik nasıl değiştirmiş gösterir.

```console
PS Azure:\> Get-AzureRmContainerInstanceLog -ResourceGroupName myResourceGroup -ContainerGroupName mycontainer1
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

Azure:\
PS Azure:\> Get-AzureRmContainerInstanceLog -ResourceGroupName myResourceGroup -ContainerGroupName mycontainer2
[('CLAUDIUS', 120),
 ('POLONIUS', 113),
 ('GERTRUDE', 82),
 ('ROSENCRANTZ', 69),
 ('GUILDENSTERN', 54)]

Azure:\
```

## <a name="azure-portal-example"></a>Azure portal örneği

Azure portalında bir kapsayıcı başlattığınızda ortam değişkenlerini ayarlamak için bunları belirtin **yapılandırma** kapsayıcısı oluşturduğunuzda, sayfa.

Portal ile dağıttığınızda, üç değişkenlere şu anda sınırlı ve şu biçimde girmeniz gerekir: `"variableName":"value"`

Bir örnek görmek için başlangıç [aci/microsoft-wordcount] [ aci-wordcount] kapsayıcıyla *NumWords* ve *MinLength* değişkenleri.

1. İçinde **yapılandırma**ayarlayın **yeniden İlkesi** için *hatasında*
2. Girin `"NumWords":"5"` ilk değişken için seçin **Evet** altında **ek ortam değişkenleri eklemek**ve girin `"MinLength":"8"` ikinci değişken için. Seçin **Tamam** doğrulayın ve ardından kapsayıcıyı dağıtmak için.

![Ortam değişkeni Etkinleştir düğmesini ve metin kutuları gösteren portal sayfası][portal-env-vars-01]

Kapsayıcının günlükleri altında görüntülemek için **ayarları** seçin **kapsayıcıları**, ardından **günlükleri**. Çıktıya benzer önceki CLI ve PowerShell bölümleri, betiğin davranışı ortam değişkenleri tarafından nasıl değiştirildiği görebilirsiniz gösterilir. Yalnızca beş sözcükler görüntülenir, her biri en az sekiz karakter uzunluğu.

![Portal gösteren kapsayıcı günlük çıktısı][portal-env-vars-02]

## <a name="next-steps"></a>Sonraki adımlar

Toplu işleme çeşitli kapsayıcıları sahip büyük bir veri kümesi gibi görev tabanlı senaryoları çalışma zamanında özel ortam değişkenleri yararlanabilir. Görev tabanlı kapsayıcıları çalıştırma hakkında daha fazla bilgi için bkz: [Azure kapsayıcı durumlarda kapsayıcılı görevleri çalıştırmak](container-instances-restart-policy.md).

<!-- IMAGES -->
[portal-env-vars-01]: ./media/container-instances-environment-variables/portal-env-vars-01.png
[portal-env-vars-02]: ./media/container-instances-environment-variables/portal-env-vars-02.png

<!-- LINKS - External -->
[aci-wordcount]: https://hub.docker.com/r/microsoft/aci-wordcount/

<!-- LINKS Internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-logs]: /cli/azure/container#az-container-logs
[az-container-show]: /cli/azure/container#az-container-show
[azure-cli-install]: /cli/azure/
[azure-instance-log]: /powershell/module/azurerm.containerinstance/get-azurermcontainerinstancelog
[azure-powershell-install]: /powershell/azure/install-azurerm-ps
[new-azurermcontainergroup]: /powershell/module/azurerm.containerinstance/new-azurermcontainergroup
[portal]: https://portal.azure.com