---
title: Azure kapsayıcı durumlarda liveness araştırmalar yapılandırın
description: Sağlıksız kapsayıcıları Azure kapsayıcı örnekleri yeniden başlatmak için liveness araştırmalar yapılandırmayı öğrenin
services: container-instances
author: jluk
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 06/08/2018
ms.author: juluk
ms.openlocfilehash: 76ca4db28d99702532ae656a19f0d54b479a13fe
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35249384"
---
# <a name="configure-liveness-probes"></a>Liveness araştırmalar yapılandırın

Kapsayıcılı uygulamalar için kapsayıcı başlatarak onarılması gerekebilir bozuk durumları sonuçta uzun süre çalışabilir. Azure kapsayıcı örnekleri kritik işlevsellik çalışmıyorsa, kapsayıcı yeniden başlayabilmesi için yapılandırmaları içerecek şekilde liveness araştırmalar destekler. 

Bu makalede nasıl otomatik olarak yeniden başlatma benzetimli sağlıksız kapsayıcı gösteren bir liveness araştırması içeren bir kapsayıcı grubu dağıtılacağı açıklanmaktadır.

## <a name="yaml-deployment"></a>YAML dağıtımı

Oluşturma bir `liveness-probe.yaml` aşağıdaki kod parçacığıyla dosya. Bu dosya sonunda bozulur bir NGNIX kapsayıcının oluşan bir kapsayıcı grubu tanımlar. 

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
  restartPolicy: Never
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

Bu kapsayıcı grubu yukarıdaki YAML yapılandırma ile dağıtmak için aşağıdaki komutu çalıştırın:

```azurecli-interactive
az container create --resource-group myResourceGroup --name livenesstest -f liveness-probe.yaml
```

### <a name="start-command"></a>Başlat komutu

Dağıtım kapsayıcı ilk başladığında çalıştıran, tarafından tanımlanan çalıştırmak için başlangıç komutu tanımlar `command` kabul eden bir dize dizisi özelliği. Bu örnekte, bu işlem bir bash oturumu başlatmak ve adlı bir dosya oluşturun `healthy` içinde `/tmp` bu komut geçirerek dizin:

```bash
/bin/sh -c "touch /tmp/healthy; sleep 30; rm -rf /tmp/healthy; sleep 600"
```

 Ardından dosyayı silmeden önce 30 saniye için uyku ve ardından 10 dakika uyku girer.

### <a name="liveness-command"></a>Liveness komutu

Bu dağıtım tanımlayan bir `livenessProbe` destekleyen bir `exec` liveness denetimi olarak davranan liveness komutu. Bu komutu sıfır olmayan bir değer ile bulunup bulunmadığını kapsayıcı sonlandırıldı ve yeniden, sinyal `healthy` dosya bulunamadı. Bu komut başarıyla çıkış kodu 0 ile bulunup bulunmadığını hiçbir işlem yapılmadı.

`periodSeconds` Özelliği atayan liveness bağlamını 5 saniyede.

## <a name="verify-liveness-output"></a>Liveness çıkış doğrulayın

İlk 30 saniye içinde `healthy` Başlat komutu tarafından oluşturulan dosya yok. Ne zaman liveness komutu denetler `healthy` dosyasının varlığı, durum kodu hiçbir yeniden oluşmaz başarılı, sinyal sıfır olarak döndürür.

30 saniye sonra `cat /tmp/healthy` başarısız olmasına sağlıksız ve sonlandırılması olayları oluşmasına neden başlar. 

Bu olaylar Azure portalında veya Azure CLI 2.0 görüntülenebilir.

![Portal sağlıksız olay][portal-unhealthy]

Azure portal, türünde olayları olayları görüntüleyerek `Unhealthy` bağlı liveness komutu başarısız tetiklenir. Sonraki olay türünü olacaktır `Killing`, yeniden başlayabilmesi için bir kapsayıcı silme gösterir. Kapsayıcı için yeniden başlatma sayısı her defasında artırır.

Yeniden başlatma tamamlandıktan ve düğüme özgü içeriği muhafaza edilir kaynaklar gibi genel IP adresleri için yerinde.

![Portal yeniden başlatma sayacı][portal-restart]

Liveness araştırma sürekli olarak başarısız olursa ve çok sayıda yeniden başlatmalar tetikler, kapsayıcı bir üstel geri alma gecikme girin.

## <a name="liveness-probes-and-restart-policies"></a>Liveness araştırmalar ve yeniden başlatma ilkeleri

Yeniden başlatma ilkeleri liveness araştırmalar tarafından tetiklenen yeniden başlatma davranışını geçersiz kılar. Örneğin, ayarlarsanız bir `restartPolicy = Never` *ve* liveness araştırması kapsayıcısı Grup durumunda başarısız liveness onay yeniden başlatılmaz. Kapsayıcı grubu için kapsayıcı grubun yeniden başlatma ilkesi bunun yerine geçecektir `Never`.

## <a name="next-steps"></a>Sonraki adımlar

Görev tabanlı senaryoları liveness araştırma zel önkoşul işlevi düzgün çalışmıyorsa, otomatik yeniden başlatmaları etkinleştirmek gerektirebilir. Görev tabanlı kapsayıcıları çalıştırma hakkında daha fazla bilgi için bkz: [Azure kapsayıcı durumlarda kapsayıcılı görevleri çalıştırmak](container-instances-restart-policy.md).

<!-- IMAGES -->
[portal-unhealthy]: ./media/container-instances-liveness-probe/unhealthy-killing.png
[portal-restart]: ./media/container-instances-liveness-probe/portal-restart.png