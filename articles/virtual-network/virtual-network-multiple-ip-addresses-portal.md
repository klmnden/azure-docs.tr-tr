---
title: Azure sanal makineleri - Portal için birden çok IP adresi | Microsoft Docs
description: Azure portalını kullanarak bir sanal makine için birden çok IP adresi atama hakkında bilgi edinin | Resource Manager.
services: virtual-network
documentationcenter: na
author: anavinahar
manager: narayan
editor: ''
tags: azure-resource-manager
ms.assetid: 3a8cae97-3bed-430d-91b3-274696d91e34
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/30/2016
ms.author: annahar
ms.openlocfilehash: 85eefd0d15ed08eaa82983c6901faa0aa1ff303c
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="assign-multiple-ip-addresses-to-virtual-machines-using-the-azure-portal"></a>Azure Portalı'nı kullanarak sanal makineleri için birden çok IP adresi atayın

>[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]
>
Bu makalede, Azure portalını kullanarak Azure Resource Manager dağıtım modeli sanal makine (VM) oluşturma açıklanmaktadır. Birden çok IP adresi Klasik dağıtım modeli aracılığıyla oluşturulan kaynakları atanamaz. Azure dağıtım modelleri hakkında daha fazla bilgi için okuma [dağıtım modellerini anlama](../resource-manager-deployment-model.md) makalesi.

[!INCLUDE [virtual-network-multiple-ip-addresses-template-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <a name = "create"></a>Birden çok IP adresiyle bir VM oluşturma

Birden çok IP adresi ya da özel bir statik IP adresi bir VM oluşturmak istiyorsanız, PowerShell veya Azure CLI kullanarak oluşturmanız gerekir. Bilgi edinmek için nasıl, bu makalenin üst PowerShell veya CLI Seçenekler'i tıklatın. Tek bir dinamik özel IP adresi ile tek bir ortak IP adresi (isteğe bağlı olarak), bir VM oluşturabilirsiniz. İçindeki adımları izleyerek portal'ı kullanmanızı [bir Windows VM oluşturma](../virtual-machines/virtual-machines-windows-hero-tutorial.md) veya [bir Linux VM oluşturma](../virtual-machines/linux/quick-create-portal.md) makaleleri. VM oluşturduktan sonra IP adresi türü dinamik statik olarak değiştirin ve adımları izleyerek Portalı'nı kullanarak ek IP adresleri ekleme [eklemek IP adresleri için bir VM](#add) bu makalenin.

## <a name="add"></a>Bir VM için IP adreslerini ekleyin

Adımları tamamlayarak bir Azure ağı arabirimi özel ve genel IP adresleri ekleyebilirsiniz. Aşağıdaki bölümlerde yer alan örnekler, zaten bir VM açıklanan üç IP yapılandırmaya sahip olduğunu varsayın [senaryo](#Scenario), ancak gerekli değildir.

### <a name="coreadd"></a>Çekirdek adımları

1. Azure portalında göz https://portal.azure.com ve oturum açın, gerekiyorsa.
2. Portalı'nda tıklatın **daha fazla hizmet** > türü *sanal makineleri* filtre kutusuna ve ardından **sanal makineleri**.
3. İçinde **sanal makineleri** bölmesinde, IP eklemek istediğiniz VM adresleri için tıklatın. Tıklatın **ağ arabirimleri** görünür ve IP eklemek istediğiniz ağ arabirimi seçin bölmesinde sanal makinenin adresleri. Aşağıdaki resimde gösterilen örnekte NIC adlı *myNIC* adlı VM'den *myVM* seçilir:

    ![Ağ arabirimi](./media/virtual-network-multiple-ip-addresses-portal/figure1.png)

4. Seçtiğiniz NIC görüntülenen bölmesinde **IP yapılandırmaları**.

Eklemek istediğiniz IP adresi türüne göre aşağıdaki bölümlerde birindeki adımları tamamlayın.

### <a name="add-a-private-ip-address"></a>**Özel bir IP adresi Ekle**

Yeni bir özel IP adresi eklemek için aşağıdaki adımları tamamlayın:

1. Bölümündeki adımları tamamlamanız [çekirdek adımları](#coreadd) bu makalenin.
2. **Ekle**'ye tıklayın. İçinde **eklemek IP yapılandırması** görünen bölmesi adlı IP yapılandırması oluşturun *IPConfig 4* ile *10.0.0.7* olarak bir *statik* Özel IP adresi ve ardından **Tamam**.

    > [!NOTE]
    > Statik bir IP adresi eklerken, NIC bağlı alt ağda kullanılmayan, geçerli bir adresi belirtmeniz gerekir. Seçtiğiniz adres kullanılabilir durumda değilse, portal IP adresi için bir X görüntüler ve farklı bir seçmeniz gerekir.

3. Tamam'ı tıklattığınızda bölmesini kapatır ve listelenen yeni IP yapılandırması bakın. Tıklatın **Tamam** kapatmak için **eklemek IP yapılandırması** bölmesi.
4. Tıklayabilirsiniz **Ekle** ek IP yapılandırmaları eklemek veya IP adresleri ekleme işlemini tamamlamak için tüm açık dikey penceresini kapatmak için.
5. İçindeki adımları tamamlayarak VM işletim sistemine özel IP adresleri ekleme [eklemek IP adresleri bir VM işletim sistemine](#os-config) bu makalenin.

### <a name="add-a-public-ip-address"></a>Bir ortak IP adresi Ekle

Bir ortak IP adresi, yeni bir IP yapılandırması ya da var olan bir IP yapılandırması için genel bir IP adresi kaynağı ilişkilendirerek eklenir.

> [!NOTE]
> Nominal bir ücret ortak IP adresine sahip. IP adresi fiyatlandırma hakkında daha fazla bilgi için okuma [IP adresi fiyatlandırma](https://azure.microsoft.com/pricing/details/ip-addresses) sayfası. Bir abonelikte kullanılabilir genel IP adresleri sayısına bir sınır yoktur. Sınırlar hakkında daha fazla bilgi için [Azure limitleri](../azure-subscription-service-limits.md#networking-limits) makalesini okuyun.
> 

### <a name="create-public-ip"></a>Bir ortak IP adresi kaynağı oluşturun

Bir ortak IP adresi, bir ortak IP adresi kaynağı için bir ayardır. Şu anda bir IP yapılandırmasına ilişkilendirmek istediğiniz bir IP yapılandırmasıyla ilişkilendirilmemiş bir ortak IP adresi kaynağı varsa, aşağıdaki adımları atlayın ve gereksinim duyduğunuz kadar aşağıdaki bölümlerde birindeki adımları tamamlayın. Kullanılabilir bir ortak IP adresi kaynak yoksa, oluşturmak için aşağıdaki adımları tamamlayın:

1. Azure portalında göz https://portal.azure.com ve oturum açın, gerekiyorsa.
3. Portalı'nda tıklatın **kaynak oluşturma** > **ağ** > **genel IP adresi**.
4. İçinde **ortak IP adresi oluştur** görünen bölmesinde girin bir **adı**seçin bir **IP adresi ataması** türü, bir **abonelik**, **Kaynak grubu**ve bir **konumu**, ardından **oluşturma**, aşağıdaki resimde gösterildiği gibi:

    ![Bir ortak IP adresi kaynağı oluşturun](./media/virtual-network-multiple-ip-addresses-portal/figure5.png)

5. Bir IP yapılandırması için genel IP adresi kaynağı ilişkilendirmek için aşağıdaki bölümlerde birindeki adımları tamamlayın.

#### <a name="associate-the-public-ip-address-resource-to-a-new-ip-configuration"></a>Yeni bir IP yapılandırması için genel IP adresi kaynağı ilişkilendirme

1. Bölümündeki adımları tamamlamanız [çekirdek adımları](#coreadd) bu makalenin.
2. **Ekle**'ye tıklayın. İçinde **eklemek IP yapılandırması** görünen bölmesi adlı IP yapılandırması oluşturun *IPConfig 4*. Etkinleştirme **genel IP adresi** ve bir var olan, kullanılabilir ortak IP adresi kaynaktan seçin **genel IP adresi seçin** bölmesinde görüntülenir.

    Ortak IP adresi kaynağı seçtikten sonra tıklatın **Tamam** ve bölmesini kapatır. Var olan bir ortak IP adresi yoksa, bir içindeki adımları tamamlayarak oluşturabilirsiniz [ortak IP adresi kaynak oluşturma](#create-public-ip) bu makalenin. 

3. Yeni IP yapılandırmasını gözden geçirin. Özel bir IP adresi açıkça atanan değildi olsa bile, tüm IP yapılandırmalarının özel bir IP adresi olması gerektiğinden bir otomatik olarak IP yapılandırması için atanmıştır.
4. Tıklayabilirsiniz **Ekle** ek IP yapılandırmaları eklemek veya IP adresleri ekleme işlemini tamamlamak için tüm açık dikey penceresini kapatmak için.
5. İşletim sisteminiz için adımları tamamlayarak VM işletim sistemine özel IP adresi Ekle [eklemek IP adresleri bir VM işletim sistemine](#os-config) bu makalenin. Genel IP adresi işletim sistemine eklemeyin.

#### <a name="associate-the-public-ip-address-resource-to-an-existing-ip-configuration"></a>Var olan bir IP yapılandırması için genel IP adresi kaynağı ilişkilendirme

1. Bölümündeki adımları tamamlamanız [çekirdek adımları](#coreadd) bu makalenin.
2. Genel bir IP adresi kaynağı eklemek istediğiniz IP Yapılandırması'nı tıklatın.
3. Görüntülenen IPConfig bölmesinde **IP adresi**.
4. İçinde **genel IP adresi seçin** görünen bölmesinde bir ortak IP adresi seçin.
5. Tıklatın **kaydetmek** ve bölmeleri kapatın. Var olan bir ortak IP adresi yoksa, bir içindeki adımları tamamlayarak oluşturabilirsiniz [ortak IP adresi kaynak oluşturma](#create-public-ip) bu makalenin.
3. Yeni IP yapılandırmasını gözden geçirin.
4. Tıklayabilirsiniz **Ekle** ek IP yapılandırmaları eklemek veya IP adresleri ekleme işlemini tamamlamak için tüm açık dikey penceresini kapatmak için. Genel IP adresi işletim sistemine eklemeyin.


[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]
