---
title: Kubernetes için Azure Stack dağıtma | Microsoft Docs
description: Kubernetes için Azure Stack dağıtmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2019
ms.author: mabrigg
ms.reviewer: waltero
ms.lastreviewed: 01/16/2019
ms.openlocfilehash: c96fdbfb56fe5274de941c0f9ab1a12cbcc241b6
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55658428"
---
# <a name="deploy-kubernetes-to-azure-stack"></a>Kubernetes için Azure Stack dağıtma

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

> [!Note]  
> Azure Stack'te Kubernetes önizlemeye sunuldu. Azure Stack bağlantısı kesilmiş senaryo preview tarafından şu anda desteklenmiyor.

Dağıtma ve kaynakları Kubernetes için tek ve eşgüdümlü bir işlemle ayarlamak için bu makaledeki adımları izleyebilirsiniz. Bir Azure Resource Manager çözüm şablonu adımları kullanın. , Azure Stack yüklemesi hakkında gerekli bilgileri toplamak için şablonu oluşturun ve ardından, buluta dağıtın. Azure Stack şablon genel Azure'da sunulan aynı yönetilen AKS hizmeti kullanmaz.

## <a name="kubernetes-and-containers"></a>Kubernetes ve kapsayıcılar

Kubernetes ACS-Engine Azure Stack'te tarafından oluşturulan Azure Resource Manager şablonlarını kullanarak yükleyebilirsiniz. [Kubernetes](https://kubernetes.io) dağıtımı otomatik hale getirmek için bir açık kaynak sistemi ölçeklendirme ve uygulamaların kapsayıcıları yönetme. A [kapsayıcı](https://www.docker.com/what-container) bir görüntüsüdür. Kapsayıcı görüntüsünü bir sanal makine bir VM, ancak benzer, kapsayıcıya yalnızca bir uygulama, kod, kod, belirli kitaplıkları ve ayarları yürütmek için çalışma zamanı gibi çalışması için gereken kaynakları içerir.

Kubernetes için kullanabilirsiniz:

- Saniyeler içinde dağıtılabilir yüksek düzeyde ölçeklenebilir ve yükseltilebilir, uygulamaları geliştirin. 
- Uygulamanızın tasarımını basitleştirmek ve farklı Helm uygulamalar tarafından ve güvenilirliği geliştirmek. [Helm](https://github.com/kubernetes/helm) yükleyin ve Kubernetes uygulamaların yaşam döngüsünü yönetmenize yardımcı olan bir açık kaynak paketleme aracıdır.
- Kolayca izleyin ve ölçek ile uygulamalarınızın durumunu tanılayın ve işlevsellik yükseltin.

Yalnızca kümenizi destekleyen düğümleri tarafından gerekli bilgi işlem kullanımı için ücret ödersiniz. Daha fazla bilgi için [kullanım ve faturalandırma Azure Stack'te](https://docs.microsoft.com/azure/azure-stack/azure-stack-billing-and-chargeback).

## <a name="deploy-kubernetes"></a>Kubernetes dağıtma

Azure Stack'te bir Kubernetes kümesi dağıtma adımları, kimlik yönetimi hizmetiniz bağlı olacaktır. Azure Stack yüklemenizin tarafından kullanılan kimlik yönetimi çözümü doğrulayın. Kimlik Yönetimi hizmetinizi doğrulamak için Azure Stack yöneticinize başvurun.

- **Azure Active Directory (Azure AD)**  
Azure AD kullanılırken kümesini yükleme ile ilgili yönergeler için bkz: [dağıtma Kubernetes için Azure Active Directory (Azure AD) kullanarak Azure Stack](azure-stack-solution-template-kubernetes-azuread.md).

- **Active Directory Federasyon Hizmetleri (AD FS)**  
AD FS kullanırken kümesini yükleme ile ilgili yönergeler için bkz: [dağıtma Kubernetes için Azure Active Directory Federasyon Hizmetleri'nde (AD FS) kullanarak yığınını](azure-stack-solution-template-kubernetes-adfs.md).

## <a name="connect-to-your-cluster"></a>Kümenize bağlanın

Artık kümenize bağlanmaya hazırsınız. Ana küme kaynak grubunuzda bulunabilir ve adlı `k8s-master-<sequence-of-numbers>`. Ana düğümüne bağlanmak için bir SSH İstemcisi'ni kullanın. Asıl kullanabileceğiniz **kubectl**, kümenizi yönetmek için Kubernetes komut satırı istemcisi. Yönergeler için [Kubernetes.io](https://kubernetes.io/docs/reference/kubectl/overview).

Ayrıca bulabilirsiniz **Helm** paket Yöneticisini Yükleme ve kümenize uygulamalarını dağıtmak için yararlıdır. Yükleme ve Helm ile kümenizi kullanma yönergeleri için bkz: [helm.sh](https://helm.sh/).

## <a name="next-steps"></a>Sonraki adımlar

[Bir Kubernetes Market'te (Azure Stack operatörü için) ekleyin.](../azure-stack-solution-template-kubernetes-cluster-add.md)

[Kubernetes Azure Active Directory (Azure AD) kullanarak Azure Stack'e dağıtma](azure-stack-solution-template-kubernetes-azuread.md)

[Kubernetes için Azure Active Directory Federasyon Hizmetleri'nde (AD FS) kullanarak yığını dağıtma](azure-stack-solution-template-kubernetes-adfs.md)

[Azure'da Kubernetes](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-kubernetes-walkthrough)
