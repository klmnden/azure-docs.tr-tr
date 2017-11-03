---
title: "Azure DC/OS kümesi Bakiye kapsayıcılarında yük | Microsoft Docs"
description: "Bir Azure kapsayıcı hizmeti DC/OS kümesinde birden çok kapsayıcı arasında Yük Dengelemesi."
services: container-service
documentationcenter: 
author: rgardler
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Kapsayıcılar, Mikro hizmetler, DC/OS, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/02/2017
ms.author: rogardle
ms.custom: mvc
ms.openlocfilehash: 78725c9d23e13d307821a188028ef573d1def038
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="load-balance-containers-in-an-azure-container-service-dcos-cluster"></a>Bir Azure kapsayıcı hizmeti DC/OS kümesi Yük Dengelemesi kapsayıcıları
Bu makalede, Marathon-LB kullanarak bir DC/OS yönetilen Azure kapsayıcı Hizmeti'nde bir iç yük dengeleyici oluşturma keşfedin. Bu yapılandırma, uygulamalarınızı yatay ölçek sağlar. Ayrıca, ortak ve özel aracı kümeleri ortak küme ve uygulama kapsayıcılarınızı özel kümede yük dengeleyici koyarak yararlanmak sağlar. Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Marathon yük dengeleyici yapılandırma
> * Yük Dengeleyici kullanarak bir uygulamayı dağıtma
> * Yapılandırma ve Azure yük dengeleyici

Bu öğreticide adımları tamamlamak için bir ACS DC/OS kümesi gerekir. Gerekirse, [bu komut dosyası örneği](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) sizin için bir tane oluşturabilirsiniz.

Bu öğretici, Azure CLI 2.0.4 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükseltme gerekiyorsa, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="load-balancing-overview"></a>Yükü Dengeleme'ye genel bakış

Bir Azure kapsayıcı hizmeti DC/OS kümesinde iki yük dengeleyici katmanı vardır: 

**Azure yük dengeleyici** ortak giriş noktaları (son kullanıcıların erişim olanlar) sağlar. Bir Azure LB Azure kapsayıcı hizmeti tarafından otomatik olarak sağlanır ve, bağlantı noktası 80, 443 ve 8080 kullanıma sunmak için yapılandırılmış varsayılan olarak açıktır.

**Marathon yük dengeleyici (marathon-lb)** yollar gelen istekleri bu istekleri kapsayıcı örnekleri. Biz bizim web hizmet sağlayan kapsayıcıları ölçeklendirirken marathon-lb dinamik olarak uyum sağlar. Bu yük dengeleyici varsayılan olarak, kapsayıcı hizmeti tarafından sağlanan değil, ancak yüklemek kolaydır.

## <a name="configure-marathon-load-balancer"></a>Marathon yük dengeleyici yapılandırma

Marathon Yük Dengeleyici dağıttığınız kapsayıcılara göre kendini dinamik olarak yeniden yapılandırır. Ayrıca bu olursa, Apache Mesos başka yerde kapsayıcıyı yeniden başlatır ve marathon-lb uyum bir kapsayıcı veya bir aracı - kaybı esnek değildir.

Marathon yük dengeleyici genel aracısının kümede yüklemek için aşağıdaki komutu çalıştırın.

```azurecli-interactive
dcos package install marathon-lb
```

## <a name="deploy-load-balanced-application"></a>Yük dengeli uygulama dağıtma

Marathon-lb paketine sahip olduğumuza göre yük dengeleme işlemi uygulamak istediğimiz bir uygulama kapsayıcısını dağıtabiliriz. 

İlk olarak, genel olarak sunulan aracıları FQDN'sini alın.

```azurecli-interactive
az acs list --resource-group myResourceGroup --query "[0].agentPoolProfiles[0].fqdn" --output tsv
```

Ardından, adlı bir dosya oluşturun *hello web.json* ve aşağıdaki içeriği kopyalayın. `HAPROXY_0_VHOST` Etiket DC/OS aracıları FQDN ile güncelleştirilmesi gerekiyor. 

```json
{
  "id": "web",
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "yeasy/simple-web",
      "network": "BRIDGE",
      "portMappings": [
        { "hostPort": 0, "containerPort": 80, "servicePort": 10000 }
      ],
      "forcePullImage":true
    }
  },
  "instances": 3,
  "cpus": 0.1,
  "mem": 65,
  "healthChecks": [{
      "protocol": "HTTP",
      "path": "/",
      "portIndex": 0,
      "timeoutSeconds": 10,
      "gracePeriodSeconds": 10,
      "intervalSeconds": 2,
      "maxConsecutiveFailures": 10
  }],
  "labels":{
    "HAPROXY_GROUP":"external",
    "HAPROXY_0_VHOST":"YOUR FQDN",
    "HAPROXY_0_MODE":"http"
  }
}
```

Uygulamayı çalıştırmak için DC/OS CLI kullanın. Varsayılan olarak Marathon dağıtır özel kümeye uygulama. Bu, genellikle istenen davranışı olduğu yukarıdaki dağıtım yalnızca, yük dengeleyici erişilebilir olduğu anlamına gelir.

```azurecli-interactive
dcos marathon app add hello-web.json
```

Uygulama dağıtıldıktan sonra Yük dengeli uygulama görüntülemek için aracı küme FQDN'si için göz atın.

![Yük dengeli uygulama görüntüsü](./media/container-service-load-balancing/lb-app.png)

## <a name="configure-azure-load-balancer"></a>Azure yük dengeleyici yapılandırma

Varsayılan olarak, Azure Load Balancer 80, 8080 ve 443 bağlantı noktalarını ortaya çıkarır. Bu bağlantı noktalarından birini kullanıyorsanız (yukarıdaki örnekte olduğu gibi) bir şey yapmanıza gerek yoktur. Aracı yük dengeleyicinin FQDN isabet gerekir ve her yenilediğinizde, üç web sunucusundan bir hepsini şekilde birini isabet. 

Farklı bir bağlantı noktası kullanıyorsanız, kullandığınız bağlantı noktası için yük dengeleyicide hepsini bir kez deneme kuralı ve bir araştırma eklemeniz gerekir. Bunu [Azure CLI](../../azure-resource-manager/xplat-cli-azure-resource-manager.md)’dan `azure network lb rule create` ve `azure network lb probe create` komutlarıyla yapabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdaki eylemleri de dahil olmak üzere Marathon ve Azure yük dengeleyici ile ACS dengelemesini hakkında öğrenilen:

> [!div class="checklist"]
> * Marathon yük dengeleyici yapılandırma
> * Yük Dengeleyici kullanarak bir uygulamayı dağıtma
> * Yapılandırma ve Azure yük dengeleyici

Azure depolama DC/OS Azure ile tümleştirme hakkında bilgi edinmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [DC/OS kümesi bağlama Azure dosya paylaşımı](container-service-dcos-fileshare.md)