---
title: VM'ler ve Azure savunma'nde Nsg'ler ile çalışma | Microsoft Docs
description: Bu makalede Azure savunma NSG erişimle edileceğini açıklar
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: conceptual
ms.date: 06/03/2019
ms.author: cherylmc
ms.openlocfilehash: 5312ad2593e732f4c84eb67ed263bc9e4666a67a
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67594185"
---
# <a name="working-with-nsg-access-and-azure-bastion-preview"></a>NSG erişim ve Azure savunma (Önizleme) ile çalışma

Azure savunma ile çalışırken, ağ güvenlik grupları (Nsg'ler) kullanabilirsiniz. Daha fazla bilgi için [güvenlik grupları](../virtual-network/security-overview.md). 

> [!IMPORTANT]
> Bu genel önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılmamalıdır. Belirli özellikler desteklenmiyor olabilir, kısıtlı yeteneklere sahip olabilir veya tüm Azure konumlarında mevcut olmayabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

![Mimari](./media/bastion-nsg/nsg_architecture.png)

Bu diyagramda:

* Kale ana bilgisayarı sanal ağ içinde dağıtılır.
* Kullanıcı, Azure portalında herhangi bir HTML5 tarayıcı kullanarak bağlanır.
* Kullanıcı bağlanmak için sanal makine seçer.
* Tek bir tıklamayla RDP/SSH oturumundan tarayıcıda açılır.
* Azure sanal makinesinde hiçbir genel IP gereklidir.

## <a name="nsg"></a>Ağ güvenlik grupları

* **AzureBastionSubnet:** Azure savunma belirli AzureBastionSubnet içinde dağıtılır.  
    * **Genel İnternet'ten gelen trafiği giriş:** Azure savunma giriş trafiği için genel IP etkin 443 numaralı bağlantı noktasını gereken bir genel IP oluşturacaksınız. Bağlantı noktası 3389/22 üzerinde AzureBastionSubnet açılmasını gerekli değildir.
    * **Çıkış trafiği hedef sanal makineler için:** Azure savunma hedef Vm'lerden özel IP ulaşacak. Diğer hedef VM alt ağları çıkış trafiğine izin vermek Nsg'ler gerekir.
* **Hedef VM alt ağı:** RDP veya SSH ile istediğiniz hedef sanal makinenin bulunduğu alt ağ budur.
    * **Azure savunma giriş akışından:** Azure savunma hedef sanal Makineyi özel IP ulaşacak. RDP/SSH bağlantı noktası (3389 numaralı bağlantı noktaları ve 22, sırasıyla) üzerinden özel IP hedef VM yan açılması gerekir.

## <a name="apply"></a>Nsg'ler için AzureBastionSubnet Uygula

Nsg'leri uygularsanız **AzureBastionSubnet**, Azure denetim düzlemi ve altyapınız için aşağıdaki iki hizmet etiketleri izin ver:

* **(Yalnızca Resource Manager) GatewayManager**: Bu etiket Azure Ağ Geçidi Yöneticisi hizmetin adres ön eklerini belirtir. GatewayManager değeri belirtirseniz, trafiğe izin verilir veya trafik reddedilir GatewayManager için.  Nsg'ler hakkında AzureBastionSubnet oluşturuyorsanız, GatewayManager etiketi gelen trafik için etkinleştirin.

* **(Yalnızca Resource Manager) AzureCloud**: Bu etiket, tüm veri merkezi genel IP adresleri de dahil olmak üzere Azure için IP adresi alanını belirtir. AzureCloud değeri belirtirseniz, trafiğe izin verilir veya trafik reddedilir Azure genel IP adresleri. Yalnızca belirli bir bölgede AzureCloud erişime izin vermek istiyorsanız bölgeyi belirtebilirsiniz. Örneğin, yalnızca Doğu ABD bölgesindeki Azure AzureCloud erişimine izin vermek istiyorsanız, bir hizmet etiketiyle AzureCloud.EastUS belirtebilirsiniz. Nsg'ler hakkında AzureBastionSubnet oluşturuyorsanız, AzureCloud etiketi giden trafik için etkinleştirin.

## <a name="next-steps"></a>Sonraki adımlar

Azure savunma hakkında daha fazla bilgi için bkz: [SSS](bastion-faq.md)
