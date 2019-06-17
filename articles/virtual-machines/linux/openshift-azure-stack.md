---
title: OpenShift Azure Stack'te dağıtma | Microsoft Docs
description: OpenShift Azure Stack'te dağıtın.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldwongms
manager: joraio
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 10/23/2018
ms.author: haroldw
ms.openlocfilehash: 91b37753ae80596612eda9d3ccd34858691e35ad
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60771558"
---
# <a name="deploy-openshift-container-platform-or-okd-in-azure-stack"></a>OpenShift kapsayıcı platformu veya Azure Stack'te OKD dağıtın

Azure Stack'te OpenShift dağıtılabilir. Dağıtım biraz farklılık gösterir ve özellikleri de biraz farklılık gösterir böylece Azure'da ve Azure Stack'te arasındaki bazı temel farklar vardır.

Şu anda, Azure bulut sağlayıcısı Azure Stack çalışmaz. Bu nedenle, disk kullanmayı mümkün olmayacaktır kalıcı depolama Azure Stack'te için iliştirin. Bunun yerine, diğer depolama seçenekleri iSCSI, GlusterFS, NFS gibi yapılandırabileceğiniz vs. Alternatif olarak, CNS etkinleştirebilir ve GlusterFS kalıcı depolama için kullanın. CNS etkinleştirilirse, üç ek düğümler GlusterFS kullanımı için ek depolama alanı ile dağıtılır.

OpenShift kapsayıcı platformu veya Azure Stack'te OKD dağıtmak için birkaç yöntemden birini kullanabilirsiniz:

- Gerekli Azure altyapı bileşenlerini el ile dağıtın ve izleyin [OpenShift kapsayıcı platformu belgeleri](https://docs.openshift.com/container-platform) veya [OKD belgeleri](https://docs.okd.io).
- Mevcut bir kullanabilirsiniz [Resource Manager şablonu](https://github.com/Microsoft/openshift-container-platform/) , OpenShift kapsayıcı platformu küme dağıtımını basitleştirir.
- Mevcut bir kullanabilirsiniz [Resource Manager şablonu](https://github.com/Microsoft/openshift-origin) , OKD küme dağıtımını basitleştirir.

Resource Manager şablonu kullanarak, uygun dalı (azurestack yayın 3.x) seçin. API sürümleri Azure ve Azure Stack arasında farklı olduğundan, şablonları Azure için çalışmaz. RHEL görüntüsü başvuru şu anda sabit kodlanmış azuredeploy.json dosyasındaki bir değişken olarak ve görüntünüzü eşleşecek şekilde değiştirilmesi gerekir.

```json
"imageReference": {
    "publisher": "Redhat",
    "offer": "RHEL-OCP",
    "sku": "7-4",
    "version": "latest"
}
```

Red Hat aboneliklerini tüm seçenekler için gereklidir. Dağıtım sırasında Red Hat Enterprise Linux örneği Red Hat aboneliğe kayıtlı ve OpenShift kapsayıcı platformu için yetkilendirmeleri içeren havuzu kimliğine bağlı.
Bir geçerli Red Hat Abonelik Yöneticisi (RHSM) kullanıcı adı, parola ve havuzu kimliği olduğundan emin olun Alternatif olarak, bir etkinleştirme anahtarı, kuruluş kimliği ve Havuz kimliği kullanabilirsiniz  Oturum açarak bu bilgileri doğrulayabilirsiniz https://access.redhat.com.

## <a name="azure-stack-prerequisites"></a>Azure Stack önkoşulları

RHEL görüntüsü (OpenShift kapsayıcı platformu) veya CentOS görüntüsü (OKD) OpenShift kümeyi dağıtmak için Azure Stack ortamınıza eklenmesi gerekir. Bu görüntüleri eklemek için Azure Stack yöneticinize başvurun. Yönergeler burada bulunabilir:

- https://docs.microsoft.com/azure/azure-stack/azure-stack-add-vm-image
- https://docs.microsoft.com/azure/azure-stack/azure-stack-marketplace-azure-items
- https://docs.microsoft.com/azure/azure-stack/azure-stack-redhat-create-upload-vhd

## <a name="deploy-by-using-the-openshift-container-platform-or-okd-resource-manager-template"></a>OpenShift kapsayıcı platformu veya OKD Resource Manager şablonu kullanarak dağıtın

Resource Manager şablonu kullanarak dağıtmak için giriş parametreleri sağlamak için bir parametre dosyası kullanın. Daha fazla dağıtımı özelleştirmek için GitHub depo çatalını oluşturmanız ve uygun öğeleri değiştirin.

Bazı ortak özelleştirme seçenekleri içerir, ancak bunlarla sınırlı değildir:

- Savunma VM boyutu (azuredeploy.json değişkeninde)
- Adlandırma kuralları (azuredeploy.json değişkenlerinde)
- OpenShift küme özellikleri, hosts dosyasını (deployOpenShift.sh) değiştirme
- RHEL görüntüsü başvurusu (azuredeploy.json değişkeninde)

Azure CLI kullanarak dağıtma adımları için uygun bölümünde izleyin [OpenShift kapsayıcı platformu](./openshift-container-platform.md) bölüm veya [OKD](./openshift-okd.md) bölümü.

## <a name="next-steps"></a>Sonraki adımlar

- [Dağıtım sonrası görevler](./openshift-post-deployment.md)
- [Azure'da OpenShift dağıtım sorunlarını giderme](./openshift-troubleshooting.md)