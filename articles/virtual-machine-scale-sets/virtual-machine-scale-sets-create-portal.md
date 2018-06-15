---
title: Azure portalında bir sanal makine ölçek kümesi oluşturma | Microsoft Docs
description: Hızla bir sanal makine ölçek Azure portalında oluşturmayı öğrenin
keywords: sanal makine ölçek kümeleri
services: virtual-machine-scale-sets
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: 9c1583f0-bcc7-4b51-9d64-84da76de1fda
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm
ms.devlang: na
ms.topic: article
ms.date: 12/19/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6157f15ef471676424a2f7027c7e9299e2218d4b
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2018
ms.locfileid: "30201468"
---
# <a name="create-a-virtual-machine-scale-set-in-the-azure-portal"></a>Azure portalında bir sanal makine ölçek kümesi oluşturma
Sanal makine ölçek kümesi, birbiriyle aynı ve otomatik olarak ölçeklendirilen sanal makine kümesi dağıtmanızı ve yönetmenizi sağlar. Ölçek kümesi içindeki VM sayısını el ile ölçeklendirebilir veya CPU, bellek isteği ya da ağ trafiği gibi kaynak kullanımını temel alan otomatik ölçeklendirme kuralları tanımlayabilirsiniz. Bu alma başlatılan makalede, Azure portalında kümesi bir sanal makine ölçek oluşturun. Bir ölçek kümesi de oluşturabilirsiniz [Azure CLI 2.0](virtual-machine-scale-sets-create-cli.md) veya [Azure PowerShell](virtual-machine-scale-sets-create-powershell.md).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.


## <a name="log-in-to-azure"></a>Azure'da oturum açma
http://portal.azure.com adresinden Azure portalında oturum açın.


## <a name="create-virtual-machine-scale-set"></a>Sanal makine ölçek kümesi oluşturma
Bir Windows Server yansıması ya da Linux görüntü RHEL, CentOS, Ubuntu veya SLES gibi kümesiyle bir ölçek dağıtabilirsiniz.

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesine tıklayın.
2. Arama *ölçek kümesini*, seçin **sanal makine ölçek kümesi**seçeneğini belirleyip **oluşturma**.
3. Ölçek kümesi için bir ad girin *myScaleSet*.
4. İstenen işletim sistemi türü gibi seçin *Windows Server 2016 Datacenter*.
5. Gibi istenen kaynak grubu adı girin *myResourceGroup*ve konum gibi *Doğu ABD*.
6. İstenen kullanıcı adınızı girin ve tercih ettiğiniz hangi kimlik doğrulama türünü seçin.
    - A **parola** en az 12 karakter uzunluğunda ve üç dışında dört aşağıdaki karmaşıklık gereksinimlerini karşılayacak olması gerekir: bir küçük harf karakter, bir büyük harf karakter, bir sayı ve bir özel karakter. Daha fazla bilgi için bkz: [kullanıcı adı ve parola gereksinimleri](../virtual-machines/windows/faq.md#what-are-the-username-requirements-when-creating-a-vm).
    - Linux işletim sistemi disk görüntüsü seçerseniz, bunun yerine seçebileceğiniz **SSH ortak anahtarını**. Yalnızca ortak anahtarınız gibi sağlamak *~/.ssh/id_rsa.pub*. Azure bulut Kabuğu'ndan portalına kullanabilirsiniz [oluşturma ve SSH anahtarları kullanma](../virtual-machines/linux/mac-create-ssh-keys.md).

7. Girin bir **ortak IP adresi adı**, gibi *myPublicIP*.
8. Benzersiz bir girin **etki alanı adı etiketi**, gibi *myuniquedns*. Bu DNS etiketi FQDN ölçek kümesini önünde yük dengeleyici için temel oluşturur.
9. Ölçek seçeneklerini ayarlama doğrulamak için şunu seçin **oluşturma**.

    ![Azure portalında kümesi bir sanal makine ölçek oluşturma](./media/virtual-machine-scale-sets-create-portal/create-scale-set.png)


## <a name="connect-to-a-vm-in-the-scale-set"></a>VM ölçek kümesi ile bağlanma
Ölçeği portalda Ayarla oluşturduğunuzda, bir yük dengeleyici oluşturulur. Ağ adresi çevirisi (NAT) kuralları RDP veya SSH gibi uzak bağlantı için ölçek kümesi örnek trafiğini dağıtmak için kullanılır.

Bu NAT görüntülemek için kurallar ve bağlantı bilgilerini, Ölçek örnekleri ayarlayın:

1. Örneğin önceki adımda oluşturduğunuz kaynak grubunu seçin *myResourceGroup*.
2. Kaynaklar listesinden seçin, **yük dengeleyici**, gibi *myScaleSetLab*.
3. Seçin **gelen NAT kuralları** penceresinin sol taraftaki menüden.

    ![Gelen NAT kuralları, sanal makine ölçek kümesi örneklerine bağlanmaya izin ver](./media/virtual-machine-scale-sets-create-portal/inbound-nat-rules.png)

Bu NAT kuralları kullanılarak ayarlanan ölçeğinde her bir VM bağlanabilir. Her VM örneği, bir hedef IP adresini ve TCP bağlantı noktası değeri listeler. Örneğin, hedef IP adresi ise *104.42.1.19* ve TCP bağlantı noktası *50001*, VM örneğine şu şekilde bağlanın:

- Bir Windows ölçek kümesi için RDP VM örneğine bağlanın `104.42.1.19:50001`
- Linux ölçek kümesi için SSH VM örneğine bağlanın `ssh azureuser@104.42.1.19 -p 50001`

Ölçek kümesi oluşturduğunuzda, belirtilen kimlik bilgileri istendiğinde, önceki adımda girin. Etkileşim kurabildikleri normal VM ölçek kümesi örnekleri olan normal olarak. Dağıtma ve uygulamaları, Ölçek kümesi örneklerinde Çalıştır hakkında daha fazla bilgi için bkz: [uygulamanızı sanal makine ölçek kümeleri üzerinde dağıtma](virtual-machine-scale-sets-deploy-app.md)


## <a name="clean-up-resources"></a>Kaynakları temizleme
Artık gerekli, kaynak grubunu silin, Ölçek kümesi ve ilişkili tüm kaynakları. Bunu yapmak için VM’nin kaynak grubunu seçin ve **Sil**’e tıklayın.


## <a name="next-steps"></a>Sonraki adımlar
Bu alma başlatılan makalede, Azure portalında ayarlamak temel bir ölçek oluşturuldu. Daha fazla ölçeklenebilirlik ve otomasyon için ölçek kümenizi aşağıdaki nasıl yapılır makaleleriyle genişletin:

- [Uygulamanızı sanal makine ölçek kümelerine dağıtma](virtual-machine-scale-sets-deploy-app.md)
- [Azure PowerShell](virtual-machine-scale-sets-autoscale-powershell.md), [Azure CLI](virtual-machine-scale-sets-autoscale-cli.md) veya [Azure portalı](virtual-machine-scale-sets-autoscale-portal.md) ile otomatik ölçeklendirme
- [Ölçek kümesi VM örnekleriniz için otomatik işletim sistemi yükseltmelerini kullanma](virtual-machine-scale-sets-automatic-upgrade.md)
