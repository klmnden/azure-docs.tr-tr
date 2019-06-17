---
title: Azure DevTest labs'deki paylaşılan IP adreslerini anlama | Microsoft Docs
description: Paylaşılan IP adresleri Azure DevTest Labs Laboratuvarınızı Vm'leri erişmek için gereken genel IP adreslerini en aza indirmek için nasıl kullandığını öğrenin.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.assetid: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/12/2019
ms.author: spelluru
ms.openlocfilehash: f7c9feedddab1aea031cb3a8879e868aae04df00
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65236878"
---
# <a name="understand-shared-ip-addresses-in-azure-devtest-labs"></a>Azure DevTest labs'deki paylaşılan IP adreslerini anlama

Azure DevTest Labs Laboratuvar Vm'leri aynı genel IP adresini tek tek laboratuvarınız sanal makineleri erişmek için gereken genel IP adresleri sayısını en aza indirmek için paylaşım sağlar.  Bu makalede, paylaşılan IP'ler iş ve bunların ilgili yapılandırma seçenekleri açıklanır.

## <a name="shared-ip-setting"></a>Paylaşılan IP ayarı

Bir laboratuvar oluşturma bir sanal ağ bir alt ağ oluşturulur.  Varsayılan olarak, bu alt ağ ile oluşturulan **etkinleştir paylaşılan genel IP** kümesine *Evet*.  Bu yapılandırma tüm alt ağa bir genel IP adresi oluşturur.  Sanal ağlar ve alt ağları yapılandırma hakkında daha fazla bilgi için bkz. [Azure DevTest Labs'de sanal ağ yapılandırma](devtest-lab-configure-vnet.md).

![Yeni Laboratuvar alt ağ](media/devtest-lab-shared-ip/lab-subnet.png)

Varolan Laboratuvar için bu seçeneği belirleyerek etkinleştirebilirsiniz **yapılandırması ve ilkelerini > sanal ağlar**. Ardından, listeden bir sanal ağ seçin ve seçin **etkinleştirme genel IP paylaşılan** seçili bir alt ağ için. Genel bir IP adresi Laboratuvar VM'ler arasında paylaşmak istemiyorsanız, herhangi bir laboratuvar bu seçeneği devre dışı bırakabilirsiniz.

Paylaşılan IP bu Laboratuvar varsayılan olarak oluşturulmuş tüm Vm'leri.  İçinde sanal makine oluştururken, bu ayar gösterilebilir **Gelişmiş ayarlar** altındaki **IP adresi yapılandırması**.

![Yeni VM](media/devtest-lab-shared-ip/new-vm.png)

- **Paylaşılan:** Olarak oluşturulan tüm sanal makinelerin **paylaşılan** (RG) bir kaynak grubuna yerleştirilir. Bunun için RG ve tüm sanal makineler rg bu IP adresini kullanacak tek bir IP adresi atanır.
- **Genel:** Oluşturduğunuz her VM kendi IP adresi vardır ve kendi kaynak grubunda oluşturulur.
- **Özel:** Oluşturduğunuz her bir VM'nin özel IP adresi kullanır. Uzak Masaüstü ile internet'ten doğrudan bu VM'ye bağlanamıyor.

Etkin paylaşılan ıp'li VM alt ağ ile eklendiğinde, DevTest Labs, otomatik olarak VM için yük dengeleyici ekler ve VM'de RDP bağlantı noktası iletme genel IP adresi üzerinde bir TCP bağlantı noktası numarası atar.  

## <a name="using-the-shared-ip"></a>Paylaşılan IP kullanma

- **Linux kullanıcıları:** IP adresini veya tam etki alanı adını, bağlantı noktası tarafından üste, ardından kullanarak sanal Makineye SSH. Örneğin, aşağıdaki görüntüde, VM'ye bağlanmak için RDP adresidir `mydevtestlab597975021002.eastus.cloudapp.azure.com:50661`.

  ![VM örneği](media/devtest-lab-shared-ip/vm-info.png)

- **Windows kullanıcıları:** Seçin **Connect** önceden yapılandırılmış bir RDP dosyasını indirin ve sanal Makineye erişmek için Azure portalında düğmesi.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure DevTest Labs'de Laboratuvar ilkelerini tanımlama](devtest-lab-set-lab-policy.md)
* [Azure DevTest Labs'de sanal ağ yapılandırma](devtest-lab-configure-vnet.md)





