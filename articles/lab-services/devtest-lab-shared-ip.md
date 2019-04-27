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
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: e7080901118dde33ed07c8a80f254b9b0d2e221c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60623036"
---
# <a name="understand-shared-ip-addresses-in-azure-devtest-labs"></a>Azure DevTest labs'deki paylaşılan IP adreslerini anlama

Azure DevTest Labs Laboratuvar Vm'leri aynı genel IP adresini tek tek laboratuvarınız sanal makineleri erişmek için gereken genel IP adresleri sayısını en aza indirmek için paylaşım sağlar.  Bu makalede, paylaşılan IP'ler iş ve bunların ilgili yapılandırma seçenekleri açıklanır.

## <a name="shared-ip-setting"></a>Paylaşılan IP ayarı

Bir laboratuvar oluşturduğunuzda, bir sanal ağ alt ağında yer alıyor.  Varsayılan olarak, bu alt ağ ile oluşturulan **etkinleştir paylaşılan genel IP** kümesine *Evet*.  Bu yapılandırma tüm alt ağa bir genel IP adresi oluşturur.  Sanal ağlar ve alt ağları yapılandırma hakkında daha fazla bilgi için bkz. [Azure DevTest Labs'de sanal ağ yapılandırma](devtest-lab-configure-vnet.md).

![Yeni Laboratuvar alt ağ](media/devtest-lab-shared-ip/lab-subnet.png)

Varolan Laboratuvar için bu seçeneği belirleyerek etkinleştirebilirsiniz **yapılandırması ve ilkelerini > sanal ağlar**. Ardından, listeden bir sanal ağ seçin ve seçin **etkinleştirme genel IP paylaşılan** seçili bir alt ağ için. Genel bir IP adresi Laboratuvar VM'ler arasında paylaşmak istemiyorsanız, herhangi bir laboratuvar bu seçeneği devre dışı bırakabilirsiniz.

Paylaşılan IP bu Laboratuvar varsayılan olarak oluşturulmuş tüm Vm'leri.  İçinde sanal makine oluştururken, bu ayar gösterilebilir **Gelişmiş ayarlar** altındaki dikey penceresinde **IP adresi yapılandırması**.

![Yeni VM](media/devtest-lab-shared-ip/new-vm.png)

- **Paylaşılan:** Olarak oluşturulan tüm sanal makinelerin **paylaşılan** (RG) bir kaynak grubuna yerleştirilir. Bunun için RG ve tüm sanal makineler rg bu IP adresini kullanacak tek bir IP adresi atanır.
- **Genel:** Oluşturduğunuz her VM kendi IP adresi vardır ve kendi kaynak grubunda oluşturulur.
- **Özel:** Oluşturduğunuz her bir VM'nin özel IP adresi kullanır. Uzak Masaüstü ile internet'ten doğrudan bu VM'ye bağlanmak mümkün olmayacaktır.

Etkin paylaşılan ıp'li VM alt ağ ile eklendiğinde, DevTest Labs, otomatik olarak VM için yük dengeleyici ekler ve VM'de RDP bağlantı noktası iletme genel IP adresi üzerinde bir TCP bağlantı noktası numarası atar.  

## <a name="using-the-shared-ip"></a>Paylaşılan IP kullanma

- **Linux kullanıcıları:** IP adresini veya tam etki alanı adını, bağlantı noktası tarafından üste, ardından kullanarak sanal Makineye SSH. Örneğin, aşağıdaki görüntüde, VM'ye bağlanmak için RDP adresidir `doclab-lab13998814308000.centralus.cloudapp.azure.com:51686`.

  ![VM örneği](media/devtest-lab-shared-ip/vm-info.png)

- **Windows kullanıcıları:** Seçin **Connect** önceden yapılandırılmış bir RDP dosyasını indirin ve sanal Makineye erişmek için Azure portalında düğmesi.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure DevTest Labs'de Laboratuvar ilkelerini tanımlama](devtest-lab-set-lab-policy.md)
* [Azure DevTest Labs'de sanal ağ yapılandırma](devtest-lab-configure-vnet.md)





