---
title: Azure Container Instances'da canlılık araştırmalarını yapılandırma
description: Azure Container Instances'da sağlıksız kapsayıcıları yeniden canlılık araştırmalarını yapılandırma hakkında bilgi edinin
services: container-instances
author: dlepow
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 06/08/2018
ms.author: danlep
ms.openlocfilehash: 89b76fc68c113b7931894c0cf003ffd846c646ab
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60583848"
---
# <a name="configure-liveness-probes"></a>Canlılık yoklaması yapılandırma

Kapsayıcılı uygulamaları için kapsayıcıyı yeniden başlatarak onarılması gerekebilir bozuk bildiren výsledek uzun süre çalışabilir. Azure Container Instances kapsayıcınızı kritik işlevler çalışmıyorsa yeniden başlayabilmesi için yapılandırmaları içerecek şekilde canlılık araştırmaları destekler.

Bu makalede sanal sağlıksız kapsayıcı otomatik yeniden başlatma gösteren bir canlılık araştırması içeren bir kapsayıcı grubu dağıtmayı açıklar.

## <a name="yaml-deployment"></a>YAML dağıtım

Oluşturma bir `liveness-probe.yaml` aşağıdaki kod parçacığı dosyası. Bu dosya sonunda kötüleşir bir NGNIX kapsayıcının oluşan bir kapsayıcı grubunu tanımlar.

```yaml
apiVersion: 2018-06-01
location: eastus
name: livenesstest
properties:
  containers:
  - name: mycontainer
    properties:
      image: nginx
      command:
        - "/bin/sh"
        - "-c"
        - "touch /tmp/healthy; sleep 30; rm -rf /tmp/healthy; sleep 600"
      ports: []
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
      livenessProbe:
        exec:
            command:
                - "cat"
                - "/tmp/healthy"
        periodSeconds: 5
  osType: Linux
  restartPolicy: Always
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

Yukarıdaki YAML yapılandırma bu kapsayıcı grubu dağıtmak için aşağıdaki komutu çalıştırın:

```azurecli-interactive
az container create --resource-group myResourceGroup --name livenesstest -f liveness-probe.yaml
```

### <a name="start-command"></a>Başlat komutu

Kapsayıcı ilk kez başlatıldığında, tarafından tanımlanan çalıştırılan çalıştırmak için bir başlangıç komutu dağıtım tanımlar `command` kabul eden bir dize dizisi özelliği. Bu örnekte, bir bash oturumu başlatın ve adlı bir dosya oluşturun `healthy` içinde `/tmp` bu komut geçirerek dizin:

```bash
/bin/sh -c "touch /tmp/healthy; sleep 30; rm -rf /tmp/healthy; sleep 600"
```

 Ardından dosyayı silmeden önce 30 saniye için uyku, ardından 10 dakika uyku girer.

### <a name="liveness-command"></a>Canlılık komutu

Bu dağıtım tanımlayan bir `livenessProbe` destekleyen bir `exec` canlılık denetimi olarak davranan canlılık komutu. Bu komut ile sıfır olmayan bir değer varsa, kapsayıcıyı sonlandırıldı ve yeniden, sinyal `healthy` dosyası bulunamadı. Bu komut 0 çıkış kodu ile başarıyla çıkılıyorsa hiçbir eylem gerçekleştirilebilir.

`periodSeconds` Özellik atar canlılık bağlamını 5 saniyede.

## <a name="verify-liveness-output"></a>Canlılık çıktıyı doğrulama

İlk 30 saniye içinde `healthy` başlangıç komutu tarafından oluşturulan dosya yok. Canlılık komut zaman denetler `healthy` dosyanın varlığını durum kodu, hiçbir yeniden başlatma gerçekleşir. Bu nedenle, başarı, sinyal sıfır döndürür.

30 saniye sonra `cat /tmp/healthy` başarısız olmasına neden oluşacak sağlıksız ve sonlandırma olayları başlar.

Bu olaylar, Azure portal veya Azure CLI görüntülenebilir.

![Portal sağlıksız olay][portal-unhealthy]

Azure portalında, olay türü olayları görüntüleyerek `Unhealthy` canlılık komutu başarısız olan temel tetiklenir. Sonraki olay türünde olacaktır `Killing`, yeniden başlatma başlayabilmesi için kapsayıcı silme işlemini gösterir. Bu her ortaya çıktığında kapsayıcı için yeniden başlatma sayısını artırır.

Yeniden başlatma tamamlandıktan düğüme özgü içeriği korunur ve kaynaklar gibi genel IP adresleri için yerinde.

![Portal yeniden sayacı][portal-restart]

Canlılık araştırması sürekli olarak başarısız olursa ve çok fazla yeniden tetikleyen bir üstel geri gecikme alma kapsayıcınızı girer.

## <a name="liveness-probes-and-restart-policies"></a>Canlılık araştırmaları ve yeniden başlatma ilkeleri

Yeniden başlatma ilkeleri canlılık araştırmalarla tetiklenen yeniden başlatma davranışı geçersiz kılar. Örneğin, ayarlarsanız bir `restartPolicy = Never` *ve* bir canlılık araştırması başarısız canlılık onay durumunda kapsayıcı grubu yeniden başlatmaz. Kapsayıcı grubu için kapsayıcı grubun yeniden başlatma ilkesi bunun yerine geçecektir `Never`.

## <a name="next-steps"></a>Sonraki adımlar

Görev tabanlı senaryoları önkoşul işlevi düzgün çalışmıyorsa, otomatik yeniden başlatmaları etkinleştirmek için bir canlılık araştırması gerektirebilir. Görev tabanlı kapsayıcı çalıştırma hakkında daha fazla bilgi için bkz. [Azure Container Instances'da kapsayıcılı görevleri çalıştırma](container-instances-restart-policy.md).

<!-- IMAGES -->
[portal-unhealthy]: ./media/container-instances-liveness-probe/unhealthy-killing.png
[portal-restart]: ./media/container-instances-liveness-probe/portal-restart.png
