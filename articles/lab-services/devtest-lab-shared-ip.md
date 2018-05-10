---
title: Azure DevTest Labs paylaşılan IP adresleri anlama | Microsoft Docs
description: Paylaşılan IP adreslerini Azure DevTest Labs laboratuvarınız sanal makineleri erişmek için gereken genel IP adresleri en aza indirmek için nasıl kullandığını öğrenin.
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
ms.openlocfilehash: c62f8808565022371484b936f5a2bdaba1f8900e
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="understand-shared-ip-addresses-in-azure-devtest-labs"></a>Azure DevTest Labs paylaşılan IP adresleri anlama

Azure DevTest Labs Laboratuvar sanal makineleri tek tek laboratuvarınız sanal makineleri erişmek için gereken genel IP adresleri sayısını en aza indirmek için ortak aynı IP adresini paylaşan olanak sağlar.  Bu makalede, paylaşılan IP'leri iş ve bunların ilgili yapılandırma seçenekleri açıklanmaktadır.

## <a name="shared-ip-setting"></a>IP ayarı paylaşılan

Bir laboratuvar oluşturduğunuzda, bir sanal ağ alt ağında yer alıyor.  Varsayılan olarak, bu alt ağ ile oluşturulan **etkinleştir paylaşılan ortak IP** kümesine *Evet*.  Bu yapılandırma tüm alt ağa bir genel IP adresi oluşturur.  Sanal ağlar ve alt ağları yapılandırma hakkında daha fazla bilgi için bkz: [Azure DevTest Labs'de sanal ağ yapılandırma](devtest-lab-configure-vnet.md).

![Yeni Laboratuvar alt ağ](media/devtest-lab-shared-ip/lab-subnet.png)

Var olan Laboratuarları için bu seçeneği seçerek etkinleştirebilirsiniz **yapılandırma ve ilkeleri > sanal ağlar**. Ardından, listeden bir sanal ağı seçin ve seçin **etkinleştirmek genel IP paylaşılan** seçilen alt ağ için. Bir ortak IP adresi Laboratuvar VM'ler arasında paylaşmak istemiyorsanız, bu seçenek, tüm laboratuvarında devre dışı bırakabilirsiniz.

Bu Laboratuvar varsayılan paylaşılan IP olarak oluşturulan herhangi bir VM.  İçinde VM oluştururken, bu ayar gösterilebilir **Gelişmiş ayarları** altında dikey **IP adresi yapılandırması**.

![Yeni VM](media/devtest-lab-shared-ip/new-vm.png)

- **Paylaşılan:** olarak oluşturulan tüm sanal makineleri **paylaşılan** (RG) bir kaynak grubuna yerleştirilir. Tek bir IP adresi için RG ve tüm sanal makineleri rg bu IP adresini kullanacak atanır.
- **Genel:** oluşturduğunuz her VM kendi IP adresi vardır ve kendi kaynak grubunda oluşturulur.
- **Özel:** oluşturduğunuz her VM özel IP adresini kullanır. Bu VM için Uzak Masaüstü ile internet'ten doğrudan bağlanabiliyor olmaz.

Paylaşılan IP etkin olan bir VM alt ağına eklendiğinde DevTest Labs otomatik olarak VM için bir yük dengeleyici ekler ve VM üzerinde RDP noktasına ileten bir TCP bağlantı noktası numarası ortak IP adresini atar.  

## <a name="using-the-shared-ip"></a>Paylaşılan IP kullanma

- **Linux kullanıcıları:** VM IP adresini veya tam etki alanı adını, bağlantı noktası tarafından üste ve ardından kullanarak SSH. Örneğin, aşağıdaki resimde VM'e bağlanmak için RDP adresidir `doclab-lab13998814308000.centralus.cloudapp.azure.com:51686`.

  ![VM örneği](media/devtest-lab-shared-ip/vm-info.png)

- **Windows kullanıcıları:** seçin **Bağlan** önceden yapılandırılmış bir RDP dosyasını karşıdan yüklemek ve VM erişmek için Azure Portal'da düğmesi.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure DevTest Labs'de Laboratuvar ilkeleri tanımlar](devtest-lab-set-lab-policy.md)
* [Azure DevTest Labs'de sanal ağ yapılandırma](devtest-lab-configure-vnet.md)





