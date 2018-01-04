---
title: "Azure portalında ayarlamak Linux sanal makine ölçek oluşturun | Microsoft Docs"
description: "Hızla bir sanal makine ölçek Azure portalında oluşturmayı öğrenin"
keywords: "Sanal makine ölçekleme kümeleri"
services: virtual-machine-scale-sets
documentationcenter: 
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
ms.openlocfilehash: fa6bf6b34d8b93ffa9aceaf7c6112c63d4cb9f1c
ms.sourcegitcommit: 901a3ad293669093e3964ed3e717227946f0af96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
---
# <a name="create-a-virtual-machine-scale-set-in-the-azure-portal"></a>Azure portalında bir sanal makine ölçek kümesi oluşturma
Bir sanal makine ölçek kümesini dağıtmak ve aynı, otomatik ölçeklendirme sanal makineler kümesi yönetmenize olanak sağlar. Ölçek kümesindeki VM'lerin sayısını elle ölçeklendirme ya da CPU, bellek isteğe bağlı veya ağ trafiğini gibi kaynak kullanımına bağlı olarak otomatik ölçeklendirme kurallarını tanımlayabilirsiniz. Bu alma başlatılan makalede, Azure portalında kümesi bir sanal makine ölçek oluşturun. Bir ölçek kümesi de oluşturabilirsiniz [Azure CLI 2.0](virtual-machine-scale-sets-create-cli.md) veya [Azure PowerShell](virtual-machine-scale-sets-create-powershell.md).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.


## <a name="log-in-to-azure"></a>Azure'da oturum açma
http://portal.azure.com sayfasından Azure portalda oturum açın.


## <a name="create-virtual-machine-scale-set"></a>Sanal makine ölçek kümesi oluşturma
Bir Windows Server yansıması ya da Linux görüntü RHEL, CentOS, Ubuntu veya SLES gibi kümesiyle bir ölçek dağıtabilirsiniz.

1. Azure portalının sol üst köşesinde bulunan **Yeni** düğmesine tıklayın.
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

- Bir Windows ölçek kümesi için RDP VM örneğine bağlanın`104.42.1.19:50001`
- Linux ölçek kümesi için SSH VM örneğine bağlanın`ssh azureuser@104.42.1.19 -p 50001`

Ölçek kümesi oluşturduğunuzda, belirtilen kimlik bilgileri istendiğinde, önceki adımda girin. Etkileşim kurabildikleri normal VM ölçek kümesi örnekleri olan normal olarak. Dağıtma ve uygulamaları, Ölçek kümesi örneklerinde Çalıştır hakkında daha fazla bilgi için bkz: [uygulamanızı sanal makine ölçek kümeleri üzerinde dağıtma](virtual-machine-scale-sets-deploy-app.md)


## <a name="clean-up-resources"></a>Kaynakları temizleme
Artık gerekli, kaynak grubunu silin, Ölçek kümesi ve ilişkili tüm kaynakları. Bunu yapmak için VM’nin kaynak grubunu seçin ve **Sil**’e tıklayın.


## <a name="next-steps"></a>Sonraki adımlar
Bu alma başlatılan makalede, Azure portalında ayarlamak temel bir ölçek oluşturuldu. Daha fazla esneklik ve Otomasyon için aşağıdaki nasıl yapılır makaleleri ile ayarlamak, ölçeğini genişletin:

- [Sanal makine ölçek kümeleri, uygulamanızda dağıtma](virtual-machine-scale-sets-deploy-app.md)
- İle otomatik olarak ölçeklendirme [Azure PowerShell](virtual-machine-scale-sets-autoscale-powershell.md), [Azure CLI](virtual-machine-scale-sets-autoscale-cli.md), veya [Azure portalı](virtual-machine-scale-sets-autoscale-portal.md)
- [Otomatik işletim sistemi yükseltme, Ölçek kümesi VM örnekleri için kullanın](virtual-machine-scale-sets-automatic-upgrade.md)
