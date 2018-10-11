---
title: Azure Sanal Ağ ile kapsayıcı ağı oluşturma | Microsoft Docs
description: Kapsayıcıları bir Azure Sanal Ağı kullanacak şekilde yapılandırmayı öğrenin.
services: virtual-network
documentationcenter: na
author: aanandr
manager: NarayanAnnamalai
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 9/18/2018
ms.author: aanandr
ms.custom: ''
ms.openlocfilehash: 9c168598d3237f7fd6dcc0e1c6414750e59c287b
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46973854"
---
# <a name="enable-containers-to-use-azure-virtual-network-capabilities"></a>Kapsayıcıların Azure Sanal Ağ özelliklerini kullanmasını sağlama

Sanal makinelerin kullandığı yazılımla tanımlanan ağ yığınını kullanarak gelişmiş Azure ağ özelliklerini kapsayıcılara ekleyin. Azure Sanal Ağ kapsayıcı ağı arabirimi (CNI) eklentisi bir Azure Sanal Makinesine yüklenir. Eklenti, sanal makinede oluşturulan kapsayıcılara bir sanal ağdan IP adresi atayarak onları sanal ağa ekler ve doğrudan diğer kapsayıcılar ile sanal ağ kaynaklarına bağlar. Eklenti, bağlantı için ağ katmanlarını veya yolları kullanmaz ve sanal makinelerle aynı performansı sunar. Eklentinin sunduğu özelliklerden en önemli olanları aşağıda verilmiştir:

- Her poda bir sanal ağ IP adresi atanır ve bir podda bir veya daha fazla kapsayıcı bulunabilir.
- Podlar eşlenen sanal ağlara veya ExpressRoute ya da siteden siteye VPN bağlantısı üzerinden şirket içi ağlara bağlanabilir. Eşlenen veya şirket içi ağlardan da podlara erişim sağlanabilir.
- Podlar sanal ağ hizmet uç noktalarıyla korunan Azure Depolama ve SQL Veritabanı gibi hizmetlere erişebilir.
- Ağ güvenlik grupları ve yollar doğrudan podlara uygulanabilir.
- Podlar sanal makineler gibi doğrudan bir Azure iç veya genel Yük Dengeleyici arkasına yerleştirilebilir.
- Podlara genel IP adresi atanarak doğrudan internet üzerinden erişim sağlanabilir. Podlar da internete erişebilir.
- Hizmetler, Giriş denetleyicileri ve Kube DNS gibi Kubernetes kaynaklarıyla sorunsuz bir şekilde çalışır. Bir Kubernetes Hizmeti, Azure Load Balancer aracılığıyla iç veya dış kullanıma sunulabilir.

Aşağıdaki resimde eklentinin podlara Azure Sanal Makinesi özelliklerini nasıl sağladığı gösterilmiştir:

![Kapsayıcı ağına genel bakış](./media/container-networking/container-networking-overview.png)

Eklenti hem Linux hem de Windows platformlarını destekler.

## <a name="connecting-pods-to-a-virtual-network"></a>Podları sanal ağa bağlama

Podlar sanal ağın parçası olan bir sanal makinede oluşturulur. Sanal makinenin ağ arabiriminde ikinci adres olarak podlar için bir IP adresi havuzu yapılandırılır. Azure CNI podlar için temel ağ bağlantısı ayarlarını yapar ve havuzdaki IP adreslerinin kullanımını yönetir. Sanal makinede bir pod oluşturulduğunda Azure CNI havuzdan kullanılabilir IP adresi atar ve podu sanal makinedeki yazılım köprüsüne bağlar. Pod sonlandırıldığında IP adresi havuza geri gönderilir. Aşağıdaki resimde podların sanal ağa nasıl bağlandığı gösterilmiştir:

![Kapsayıcı ağı ayrıntıları](./media/container-networking/container-networking-detail.png)

## <a name="internet-access"></a>İnternet erişimi

Podların internete erişmesini sağlamak için eklenti *iptables* kurallarını yapılandırarak podlardan internete giden trafikte ağ adresi çevirisi (NAT) gerçekleştirir. Paketin kaynak IP adresi sanal makine ağ arabiriminin birincil IP adresine çevrilir. Windows sanal makineleri otomatik olarak sanal makinenin bulunduğu alt ağın dışında bir IP adresine gönderilen NAT (SNAT) trafiği için kaynak oluşturur. Genellikle sanal ağın IP aralığının dışındaki bir IP adresine gönderilen tüm trafik çevrilir.

## <a name="limits"></a>Sınırlar

Eklenti sanal makine başına 250 pod ve bir sanal ağda 16.000 pod için destek sunar. Bu sınırlar [Azure Kubernetes Service](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#kubernetes-service-limits) için farklıdır.

## <a name="using-the-plug-in"></a>Eklentiyi kullanma

Eklenti, podlar veya Docker kapsayıcılarına temel sanal ağ bağlantısı sağlamak üzere şu şekillerde kullanılabilir:

- **Azure Kubernetes Service**: Eklenti Azure Kubernetes Service (AKS) ile tümleşiktir ve *Gelişmiş Ağ* seçeneği ile kullanılabilir. Gelişmiş Ağ, Kubernetes kümesini var olan veya yeni bir sanal ağa dağıtmanızı sağlar. Gelişmiş Ağ ve ayarlama adımları hakkında daha fazla bilgi için bkz. [AKS'de ağ yapılandırması](../aks/networking-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
- **ACS-Engine**: ACS-Engine, Azure'da Kubernetes kümesi dağıtımı için Azure Resource Manager şablonu oluşturan bir araçtır. Ayrıntılı yönergeler için bkz. [Eklentiyi ACS-Engine Kubernetes kümeleri için dağıtma](deploy-container-networking.md#deploy-plug-in-for-acs-engine-kubernetes-cluster).
- **Azure'da kendi Kubernetes kümenizi oluşturma**: Eklentiyi AKS veya ACS-Engine gibi araçlar olmadan kendi oluşturduğunuz Kubernetes kümelerindeki podlar için temel ağ özellikleri sağlamak için kullanabilirsiniz. Bu durumda eklentinin kümedeki tüm sanal makinelere yüklenmesi ve etkinleştirilmesi gerekir. Ayrıntılı yönergeler için bkz. [Eklentiyi kendi dağıttığınız Kubernetes kümesi için dağıtma](deploy-container-networking.md#deploy-plug-in-for-a-kubernetes-cluster).
- **Azure'daki Docker kapsayıcılarına sanal ağ ekleme**: Eklenti bir Kubernetes kümesi kullanmak istemediğiniz ve sanal makinelerde sanal ağa eklenmiş Docker kapsayıcısı oluşturmak istediğiniz durumlarda kullanılabilir. Ayrıntılı yönergeler için bkz. [Eklentiyi Docker için dağıtma](deploy-container-networking.md#deploy-plug-in-for-docker-containers).

## <a name="next-steps"></a>Sonraki adımlar

Kubernetes kümeleri veya Docker kapsayıcıları için [eklentiyi dağıtma](deploy-container-networking.md)