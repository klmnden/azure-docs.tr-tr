---
title: El ile Azure Container Instances'da kapsayıcı başlatma veya durdurma
description: El ile Azure Container Instances'da bir kapsayıcı grubu başlatmak veya durdurmak öğrenin.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 04/15/2019
ms.author: danlep
ms.openlocfilehash: 50f3ecf69561313a5bda67827cfb02d2f61d461f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60653670"
---
# <a name="manually-stop-or-start-containers-in-azure-container-instances"></a>El ile Azure Container Instances'da kapsayıcı başlatma veya durdurma

[Yeniden başlatma ilkesi](container-instances-restart-policy.md) nasıl container Instances Başlat veya Durdur varsayılan olarak bir kapsayıcı grubu ayarı belirler. Varsayılan el ile durdurma veya bir kapsayıcı grubu başlatma ayarı geçersiz kılabilirsiniz.

## <a name="stop"></a>Durdur

El ile çalıştırılan bir kapsayıcı grubu - Örneğin, kullanarak durdurun [az container durdurma] [ az-container-stop] komutu veya Azure portalında. Belirli kapsayıcı iş yükleri, maliyet tasarrufu için tanımlanan bir süre sonra uzun süre çalışan bir kapsayıcı grubu durdurmak isteyebilirsiniz. 

*Kapsayıcı grubu durduruldu durumuna girdiğinde, sonlandırır ve gruptaki tüm kapsayıcıları geri dönüştürür. Kapsayıcı durumu korumaz.*

Durdurulmuş kapsayıcı grubundaki kapsayıcı dönüştürülünceye rağmen [kaynakları](container-instances-container-groups.md#resource-allocation) kullanım için ayrılmış şekilde kalır. Bu nedenle, durdurulmuş kapsayıcı grubu için faturalandırma devam eder.

Kapsayıcı grubu zaten sonlandırıldıysa durdurma eylemi etkiye sahip değildir (başarılı veya başarısız durumda). Örneğin, bir kapsayıcı grubu başarıyla çalıştı kez çalıştırılan kapsayıcı görevleri başarılı durumunda sonlandırır. Durum değiştirme durumu, grubun durdurmaya çalışır. 

## <a name="start"></a>Başlatma

Ya da kapsayıcıları, kendi sonlandırıldı veya grubu - el ile durduruldu çünkü bir kapsayıcı grubu durdurulduğunda - kapsayıcıları başlayabilirsiniz. Örneğin, [az container başlangıç] [ az-container-start] komutu veya Azure portalında kapsayıcı grubuna el ile başlatmak için. Her kapsayıcı için kapsayıcı görüntüsü güncelleştirdiyseniz, yeni bir görüntü alınır. 

Bir kapsayıcı grubu başlayarak yeni bir dağıtım ile aynı kapsayıcı yapılandırması başlar. Bu eylem, hızlı bir şekilde beklediğiniz gibi çalışır bir bilinen kapsayıcı grubunun yapılandırması yeniden yardımcı olabilir. Aynı iş yükünü çalıştırmak için yeni bir kapsayıcı grubu oluşturmanız gerekmez.

Bir kapsayıcı grubundaki tüm kapsayıcıları, bu eylem tarafından başlatılır. Belirli bir kapsayıcı grubunda başlatılamıyor.

El ile başlatın veya bir kapsayıcı grubu yeniden sonra kapsayıcı grubu çalıştırmalar göre yapılandırılmış ilke yeniden başlatın.
  
## <a name="restart"></a>Yeniden Başlatma

-Örneğin, kullanarak çalışırken, bir kapsayıcı grubu yeniden başlatabilirsiniz [az container yeniden] [ az-container-restart] komutu. Bu eylem tüm kapsayıcıları, kapsayıcı grubunda yeniden başlatır. Her kapsayıcı için kapsayıcı görüntüsü güncelleştirdiyseniz, yeni bir görüntü alınır. 

Kapsayıcı grubu yeniden başlatma, bir dağıtım sorunu gidermek istediğinizde yararlıdır. Geçici kaynak sınırlaması kapsayıcılarınızı başarıyla çalışmasını engelliyorsa, örneğin, grubu yeniden başlatılması sorunu çözebilir.

Bir kapsayıcı grubundaki tüm kapsayıcıları, bu eylem tarafından başlatılır. Belirli bir kapsayıcı grubunda yeniden başlatılamıyor.

Kapsayıcı grubu çalıştırmalar göre yapılandırılmış bir kapsayıcı grubu el ile yeniden başlatıldıktan sonra ilke yeniden başlatın.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [ilke ayarlarını yeniden](container-instances-restart-policy.md) Azure Container ınstances'da.

El ile durdurma ve kapsayıcı grubu mevcut yapılandırmayla başlatma ek olarak, şunları yapabilirsiniz [ayarlarını güncelleştirme](container-instances-update.md) çalışan bir kapsayıcı grubunun.

<!-- LINKS - External -->

<!-- LINKS - Internal -->
[az-container-restart]: /cli/azure/container?view=azure-cli-latest#az-container-restart
[az-container-start]: /cli/azure/container?view=azure-cli-latest#az-container-start
[az-container-stop]: /cli/azure/container?view=azure-cli-latest#az-container-stop
