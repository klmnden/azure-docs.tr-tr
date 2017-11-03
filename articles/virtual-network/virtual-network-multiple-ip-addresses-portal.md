---
title: "Azure sanal makineleri - Portal için birden çok IP adresi | Microsoft Docs"
description: "Azure portalını kullanarak bir sanal makine için birden çok IP adresi atama hakkında bilgi edinin | Resource Manager."
services: virtual-network
documentationcenter: na
author: anavinahar
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: 3a8cae97-3bed-430d-91b3-274696d91e34
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/30/2016
ms.author: annahar
ms.openlocfilehash: d264bd47d76db8015a64f09248c57c94572e2693
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="assign-multiple-ip-addresses-to-virtual-machines-using-the-azure-portal"></a>Azure Portalı'nı kullanarak sanal makineleri için birden çok IP adresi atayın

>[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]
>
Bu makalede, Azure portalını kullanarak Azure Resource Manager dağıtım modeli sanal makine (VM) oluşturma açıklanmaktadır. Birden çok IP adresi Klasik dağıtım modeli aracılığıyla oluşturulan kaynakları atanamaz. Azure dağıtım modelleri hakkında daha fazla bilgi için okuma [dağıtım modellerini anlama](../resource-manager-deployment-model.md) makalesi.

[!INCLUDE [virtual-network-multiple-ip-addresses-template-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <a name = "create"></a>Birden çok IP adresiyle bir VM oluşturma

Birden çok IP adresi ya da özel bir statik IP adresi bir VM oluşturmak istiyorsanız, PowerShell veya Azure CLI kullanarak oluşturmanız gerekir. Bilgi edinmek için PowerShell veya CLI bu makalenin üst seçenekleri nasıl. Tek bir dinamik özel IP adresi ve (isteğe bağlı) içindeki adımları izleyerek Portalı'nı kullanarak tek bir ortak IP adresi ile bir VM oluşturma [bir Windows VM oluşturma](../virtual-machines/virtual-machines-windows-hero-tutorial.md) veya [bir Linux VM oluşturma](../virtual-machines/linux/quick-create-portal.md) makaleleri. VM oluşturduktan sonra IP adresi türü dinamik statik olarak değiştirin ve adımları izleyerek Portalı'nı kullanarak ek IP adresleri ekleme [eklemek IP adresleri için bir VM](#add) bu makalenin.

## <a name="add"></a>Bir VM için IP adreslerini ekleyin

Adımları tamamlayarak bir NIC'ye özel ve genel IP adresleri ekleyebilirsiniz. Aşağıdaki bölümlerde yer alan örnekler, zaten bir VM açıklanan üç IP yapılandırmaya sahip olduğunu varsayın [senaryo](#Scenario) Bu makale, ancak gerekli değildir, yapın.

### <a name="coreadd"></a>Çekirdek adımları

1. Https://portal.azure.com ve oturum Azure portalında, gerekirse göz atın.
2. Portalı'nda tıklatın **daha fazla hizmet** > türü *sanal makineleri* filtre kutusuna ve ardından **sanal makineleri**.
3. İçinde **sanal makineleri** dikey penceresinde, IP eklemek istediğiniz VM adresleri için tıklatın. Tıklatın **ağ arabirimleri** görünür ve IP eklemek istediğiniz ağ arabirimi seçin dikey adresleri için sanal makinede. Aşağıdaki resimde gösterilen örnekte NIC adlı *myNIC* adlı VM'den *myVM* seçilir:

    ![Ağ arabirimi](./media/virtual-network-multiple-ip-addresses-portal/figure1.png)

4. Seçtiğiniz NIC görünür dikey penceresinde, tıklayın **IP yapılandırmaları**.

Eklemek istediğiniz IP adresi türüne göre aşağıdaki bölümlerde birindeki adımları tamamlayın.

### <a name="add-a-private-ip-address"></a>**Özel bir IP adresi Ekle**

Yeni bir özel IP adresi eklemek için aşağıdaki adımları tamamlayın:

1. Bölümündeki adımları tamamlamanız [çekirdek adımları](#coreadd) bu makalenin.
2. **Ekle**'ye tıklayın. İçinde **eklemek IP yapılandırması** görünür, dikey adlı IP yapılandırması oluşturun *IPConfig-4* ile *10.0.0.7* olarak bir *statik* özel IP adresi ve ardından **Tamam**.

    > [!NOTE]
    > Statik bir IP adresi eklerken, NIC bağlı alt ağda kullanılmayan, geçerli bir adresi belirtmeniz gerekir. Seçtiğiniz adres kullanılabilir durumda değilse, portal IP adresi için bir X işareti gösterilir ve farklı bir seçmeniz gerekir.

3. Tamam'a tıkladıktan sonra dikey penceresi kapanır ve listelenen yeni IP yapılandırması görürsünüz. Tıklatın **Tamam** kapatmak için **eklemek IP yapılandırması** dikey.
4. Tıklayabilirsiniz **Ekle** ek IP yapılandırmaları eklemek veya IP adresleri ekleme işlemini tamamlamak için tüm açık dikey penceresini kapatmak için.
5. İşletim sisteminiz için adımları tamamlayarak VM işletim sistemine özel IP adresleri ekleme [eklemek IP adresleri bir VM işletim sistemine](#os-config) bu makalenin.

### <a name="add-a-public-ip-address"></a>Bir ortak IP adresi Ekle

Bir ortak IP adresi, yeni bir IP yapılandırması ya da var olan bir IP yapılandırması için genel bir IP adresi kaynağı ilişkilendirerek eklenir.

> [!NOTE]
> Nominal bir ücret ortak IP adresine sahip. IP adresi fiyatlandırma hakkında daha fazla bilgi için okuma [IP adresi fiyatlandırma](https://azure.microsoft.com/pricing/details/ip-addresses) sayfası. Bir abonelikte kullanılabilir genel IP adresleri sayısına bir sınır yoktur. Sınırlar hakkında daha fazla bilgi için [Azure limitleri](../azure-subscription-service-limits.md#networking-limits) makalesini okuyun.
> 

### <a name="create-public-ip"></a>Bir ortak IP adresi kaynağı oluşturun

Bir ortak IP adresi, bir ortak IP adresi kaynağı için bir ayardır. Şu anda bir IP yapılandırmasına ilişkilendirmek istediğiniz bir IP yapılandırmasıyla ilişkilendirilmemiş bir ortak IP adresi kaynağı varsa, aşağıdaki adımları atlayın ve gereksinim duyduğunuz kadar aşağıdaki bölümlerde birindeki adımları tamamlayın. Kullanılabilir bir ortak IP adresi kaynak yoksa, oluşturmak için aşağıdaki adımları tamamlayın:

1. Https://portal.azure.com ve oturum Azure portalında, gerekirse göz atın.
3. Portalı'nda tıklatın **yeni** > **ağ** > **genel IP adresi**.
4. İçinde **ortak IP adresi oluştur** görünür, dikey girin bir **adı**seçin bir **IP adresi ataması** türü, bir **abonelik**, **kaynak grubu**ve bir **konumu**,'ye tıklayın **oluşturma**, aşağıdaki resimde gösterildiği gibi:

    ![Bir ortak IP adresi kaynağı oluşturun](./media/virtual-network-multiple-ip-addresses-portal/figure5.png)

5. Bir IP yapılandırması için genel IP adresi kaynağı ilişkilendirmek için aşağıdaki bölümlerde birindeki adımları tamamlayın.

#### <a name="associate-the-public-ip-address-resource-to-a-new-ip-configuration"></a>Yeni bir IP yapılandırması için genel IP adresi kaynağı ilişkilendirme

1. Bölümündeki adımları tamamlamanız [çekirdek adımları](#coreadd) bu makalenin.
2. **Ekle**'ye tıklayın. İçinde **eklemek IP yapılandırması** görünür, dikey penceresi oluşturma adlı IP yapılandırması *IPConfig 4*. Etkinleştirme **genel IP adresi** bir var olan, kullanılabilir ortak IP adresi kaynaktan seçip **genel IP adresi seçin** görünür dikey.

    Ortak IP adresi kaynağı seçtikten sonra tıklatın **Tamam** ve dikey penceresini kapatın. Var olan bir ortak IP adresi yoksa, bir içindeki adımları tamamlayarak oluşturabilirsiniz [ortak IP adresi kaynak oluşturma](#create-public-ip) bu makalenin. 

3. Yeni IP yapılandırmasını gözden geçirin. Özel bir IP adresi açıkça atanan değildi olsa bile, tüm IP yapılandırmalarının özel bir IP adresi olması gerektiğinden bir otomatik olarak IP yapılandırması için atanmıştır.
4. Tıklayabilirsiniz **Ekle** ek IP yapılandırmaları eklemek veya IP adresleri ekleme işlemini tamamlamak için tüm açık dikey penceresini kapatmak için.
5. İşletim sisteminiz için adımları tamamlayarak VM işletim sistemine özel IP adresi Ekle [eklemek IP adresleri bir VM işletim sistemine](#os-config) bu makalenin. Genel IP adresi işletim sistemine eklemeyin.

#### <a name="associate-the-public-ip-address-resource-to-an-existing-ip-configuration"></a>Var olan bir IP yapılandırması için genel IP adresi kaynağı ilişkilendirme

1. Bölümündeki adımları tamamlamanız [çekirdek adımları](#coreadd) bu makalenin.
2. Genel bir IP adresi kaynağı eklemek istediğiniz IP Yapılandırması'nı tıklatın.
3. Görüntülenen IPConfig dikey penceresinde tıklayın **IP adresi**.
4. İçinde **genel IP adresi seçin** görünür, dikey penceresinde bir ortak IP adresi seçin.
5. Tıklatın **kaydetmek** ve dikey pencereler kapatılacak. Var olan bir ortak IP adresi yoksa, bir içindeki adımları tamamlayarak oluşturabilirsiniz [ortak IP adresi kaynak oluşturma](#create-public-ip) bu makalenin.
3. Yeni IP yapılandırmasını gözden geçirin.
4. Tıklayabilirsiniz **Ekle** ek IP yapılandırmaları eklemek veya IP adresleri ekleme işlemini tamamlamak için tüm açık dikey penceresini kapatmak için. Genel IP adresi işletim sistemine eklemeyin.


[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]
