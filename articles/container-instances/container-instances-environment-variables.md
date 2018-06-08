---
title: Azure kapsayıcı durumlarda ortam değişkenlerini ayarlama
description: Azure kapsayıcı örnekleri çalıştırmak kapsayıcılarında ortam değişkenlerini ayarlama öğrenin
services: container-instances
author: mmacy
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 06/07/2018
ms.author: marsma
ms.openlocfilehash: bc30352f50344031f8356d2be1b800dd035f12ad
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34830471"
---
# <a name="set-environment-variables"></a>Ortam değişkenlerini belirleme

Kapsayıcı örneklerinizi ortam değişkenlerini ayarlama, uygulamanın veya komut dosyasını çalıştır kapsayıcı tarafından dinamik yapılandırma sağlamanıza olanak verir. Bir kapsayıcıda ortam değişkenlerini ayarlamak için bir kapsayıcı örneği oluşturduğunuzda, bunları belirtin. Bir kapsayıcı ile başlattığınızda ortam değişkenlerini ayarlayabilirsiniz [Azure CLI](#azure-cli-example), [Azure PowerShell](#azure-powershell-example)ve [Azure portal](#azure-portal-example).

Örneğin, çalıştırırsanız [aci/microsoft-wordcount] [ aci-wordcount] kapsayıcı görüntüsü, aşağıdaki ortam değişkenleri belirterek davranışını değiştirebilirsiniz:

*NumWords*: STDOUT gönderilen sözcük sayısı.

*MinLength*: en az bir sözcük, sayılması için karakter sayısı. Sık kullanılan sözcük gibi "," ve "." daha yüksek bir sayı yoksayar

Gizli ortam değişkenleri olarak geçirmek gerekiyorsa, Azure kapsayıcı örnekleri destekleyen [güvenli değerleri](#secure-values) güvenli Windows ve Linux kapsayıcıları için değerler.

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

## <a name="secure-values"></a>Güvenli değerleri
Güvenli değerlerle nesneleri parolaları veya uygulamanız için anahtarlar gibi hassas bilgileri tutmak üzere tasarlanmıştır. Güvenli değerleri için ortam değişkenleri kullanılarak güvenli ve, kapsayıcının görüntüsüne dahil daha esnektir. Başka bir seçenek açıklanan gizli birimleri kullanmaktır [Azure kapsayıcı durumlarda gizli bir birim](container-instances-volume-secret.md).

Değer yalnızca kapsayıcı içinde erişilebilir şekilde güvenli değerleriyle güvenli bir ortam değişkenleri güvenli bir değerle, kapsayıcının özelliklerinde ortaya olmaz. Örneğin, Azure portalında kapsayıcı özellikleri görüntülenemez veya Azure CLI bir ortam değişkeni güvenli bir değerle görüntülenmez.

Güvenli bir ortam değişkeni ayarlamak `secureValue` özelliği normal yerine `value` değişkenin türü. Aşağıdaki YAML içinde tanımlanan iki değişken iki değişken türleri gösterilmektedir.

### <a name="yaml-deployment"></a>YAML dağıtımı

Oluşturma bir `secure-env.yaml` aşağıdaki kod parçacığıyla dosya.

```yaml
apiVersion: 2018-06-01
location: westus
name: securetest
properties:
  containers:
  - name: mycontainer
    properties:
      environmentVariables:
        - "name": "SECRET"
          "secureValue": "my-secret-value"
        - "name": "NOTSECRET"
          "value": "my-exposed-value"
      image: nginx
      ports: []
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
  osType: Linux
  restartPolicy: Always
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

Kapsayıcı grubu YAML ile dağıtmak için aşağıdaki komutu çalıştırın.

```azurecli-interactive
az container create --resource-group myRG --name securetest -f secure-env.yaml
```

### <a name="verify-environment-variables"></a>Ortam değişkenlerini doğrulayın

Sorgu, kapsayıcının ortam değişkenleri için için aşağıdaki komutu çalıştırın.

```azurecli-interactive
az container show --resource-group myRG --name securetest --query 'containers[].environmentVariables`
```

Bu kapsayıcı için ayrıntılarla JSON yanıt yalnızca güvenli olmayan ortam değişkeni Göster ve ortam değişkeninin anahtar güvenliğini sağlayın.

```json
  "environmentVariables": [
    {
      "name": "NOTSECRET",
      "value": "my-exposed-value"
    },
    {
      "name": "SECRET"
    }
```

Güvenli gözden geçirebilirsiniz ortam değişkeni ayarlandığında `exec` çalışan bir kapsayıcı içindeki bir komut yürütülürken sağlayan komutu. 

Kapsayıcıyla bir etkileşimli bash oturumu başlatmak için aşağıdaki komutu çalıştırın.
```azurecli-interactive
az container exec --resource-group myRG --name securetest --exec-command "/bin/bash"
```

Kapsayıcı içinde ortam değişkeni aşağıdaki bash komutuyla yazdırma.
```bash
echo $SECRET
```

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
