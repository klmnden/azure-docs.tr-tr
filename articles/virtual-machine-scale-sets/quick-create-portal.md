---
title: Hızlı Başlangıç - Azure portalında sanal makine ölçek kümesi oluşturma | Microsoft Docs
description: Azure portalında hızlıca sanal makine ölçek kümesi oluşturmayı öğrenin
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
ms.topic: quickstart
ms.custom: H1Hack27Feb2017
ms.date: 03/27/18
ms.author: iainfou
ms.openlocfilehash: 03b4468fd1032b94cde5d83bc871c32e0c21df1c
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
ms.locfileid: "30247367"
---
# <a name="quickstart-create-a-virtual-machine-scale-set-in-the-azure-portal"></a>Hızlı Başlangıç: Azure portalında sanal makine ölçek kümesi oluşturma
Sanal makine ölçek kümesi, birbiriyle aynı ve otomatik olarak ölçeklendirilen sanal makine kümesi dağıtmanızı ve yönetmenizi sağlar. Ölçek kümesi içindeki sanal makine sayısını el ile ölçeklendirebilir veya CPU, bellek talebi ya da ağ trafiği gibi kaynak kullanımını temel alan otomatik ölçeklendirme kuralları tanımlayabilirsiniz. Azure Load Balancer daha sonra ölçek kümesindeki sanal makine örneklerine trafiği dağıtır. Bu hızlı başlangıçta, Azure portalında sanal makine ölçek kümesi oluşturacaksınız.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.


## <a name="log-in-to-azure"></a>Azure'da oturum açma
http://portal.azure.com adresinden Azure portalında oturum açın.


## <a name="create-virtual-machine-scale-set"></a>Sanal makine ölçek kümesi oluşturma
RHEL, CentOS, Ubuntu veya SLES gibi bir Linux görüntüsü ya da Windows Server görüntüsü ile ölçek kümesi dağıtabilirsiniz.

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesine tıklayın.
2. *Ölçek kümesi* için arama yapın, **Sanal makine ölçek kümesi**’ni seçin ve sonra **Oluştur**’u seçin.
3. Ölçek kümesi için bir ad girin (örn. *myScaleSet*).
4. İstediğiniz işletim sistemi türünü seçin (örn. *Windows Server 2016 Datacenter*).
5. İstediğiniz kaynak grubu adını (örn. *myResourceGroup*) ve konumu (örn. *Doğu ABD*) girin.
6. İstediğiniz kullanıcı adını girin ve tercih ettiğiniz kimlik doğrulaması türünü seçin.
    - **Parola** en az 12 karakter uzunluğunda olmalı ve şu dört karmaşıklık gereksiniminden üçünü karşılamalıdır: bir küçük harf karakter, bir büyük harf karakter, bir sayı ve bir özel karakter. Daha fazla bilgi için [kullanıcı adı ve parola gereksinimlerine](../virtual-machines/windows/faq.md#what-are-the-username-requirements-when-creating-a-vm) bakın.
    - Bir Linux OS disk görüntüsü seçerseniz bunun yerine **SSH genel anahtarını** seçebilirsiniz. Yalnızca genel anahtarınızı sağlayın (örn. *~/.ssh/id_rsa.pub*). [SSH anahtarları oluşturmak ve kullanmak](../virtual-machines/linux/mac-create-ssh-keys.md) için portaldan Azure Cloud Shell’i kullanabilirsiniz.

7. Bir **Genel IP adresi adı** girin (örn. *myPublicIP*).
8. Benzersiz bir **Etki alanı adı etiketi** girin (örn. *myuniquedns*). Bu DNS etiketi, ölçek kümesinin önünde yük dengeleyici için FQDN temelini oluşturur.
9. Ölçek kümesi seçeneklerini onaylamak için **Oluştur**’u seçin.

    ![Azure portalında sanal makine ölçek kümesi oluşturma](./media/virtual-machine-scale-sets-create-portal/create-scale-set.png)


## <a name="connect-to-a-vm-in-the-scale-set"></a>Ölçek kümesindeki bir sanal makineye bağlanma
Portalda ölçek kümesi oluşturduğunuzda bir yük dengeleyici oluşturulur. RDP veya SSH gibi uzak bağlantıya yönelik ölçek kümesi örneklerine trafiği dağıtmak için Ağ Adresi Çevirisi (NAT) kuralları kullanılır.

Ölçek kümesi örneklerinize yönelik bu NAT kurallarını ve bağlantı bilgilerini görüntülemek için:

1. Önceki adımda oluşturduğunuz kaynak grubunu seçin (örn. *myResourceGroup*.
2. Kaynak listesinden **Yük dengeleyicinizi** seçin (örn. *myScaleSetLab*).
3. Pencerenin sol tarafındaki menüden **Gelen NAT kuralları**’nı seçin.

    ![Gelen NAT kuralları, sanal makine ölçek kümesi örneklerine bağlanmanıza olanak sağlar.](./media/virtual-machine-scale-sets-create-portal/inbound-nat-rules.png)

Bu NAT kurallarını kullanarak ölçek kümesindeki her bir sanal makineye bağlanabilirsiniz. Her sanal makine örneği bir hedef IP adresini ve TCP bağlantı noktası değerini listeler. Örneğin, hedef IP adresi *104.42.1.19* ise ve TCP bağlantı noktası *50001* ise aşağıdaki adımları uygulayarak sanal makine örneğine bağlanırsınız:

- Windows ölçek kümesi için, `104.42.1.19:50001` üzerinde RDP ile sanal makine örneğine bağlanın
- Linux ölçek kümesi için, `ssh azureuser@104.42.1.19 -p 50001` üzerinde SSH ile sanal makine örneğine bağlanın

İstendiğinde, önceki adımda ölçek kümesini oluştururken belirttiğiniz kimlik bilgilerini girin. Ölçek kümesi örnekleri, normal şekilde etkileşim kurabileceğiniz normal sanal makinelerdir. Ölçek kümesi örneklerinizde uygulamaları dağıtma ve çalıştırma hakkında daha fazla bilgi için bkz. [Sanal makine ölçek kümelerinde uygulamanızı dağıtma](virtual-machine-scale-sets-deploy-app.md)


## <a name="clean-up-resources"></a>Kaynakları temizleme
Artık gerekli olmadığında kaynak grubunu, ölçek kümesini ve tüm ilgili kaynakları silin. Bunu yapmak için VM’nin kaynak grubunu seçin ve **Sil**’e tıklayın.


## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta, Azure portalında temel bir ölçek kümesi oluşturdunuz. Daha fazla bilgi edinmek için Azure sanal makine ölçek kümelerinin nasıl oluşturulacağı ve yönetileceğine ilişkin öğreticiyle devam edin.

> [!div class="nextstepaction"]
> [Azure sanal makine ölçek kümeleri oluşturma ve yönetme](tutorial-create-and-manage-powershell.md)