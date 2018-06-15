---
title: -Azure portalında bir ağ güvenlik grubu oluşturun | Microsoft Docs
description: Azure Portalı'nı kullanarak ağ güvenlik grupları oluşturup öğrenin.
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 5bc8fc2e-1e81-40e2-8231-0484cd5605cb
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d8a66de0b0239fef12168733eca7af85c8b08f82
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
ms.locfileid: "31793652"
---
# <a name="create-a-network-security-group-using-the-azure-portal"></a>Azure Portalı'nı kullanarak bir ağ güvenlik grubu oluşturun

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Bu makalede Resource Manager dağıtım modeli anlatılmaktadır. Ayrıca [Klasik dağıtım modelinde Nsg'leri oluşturma](virtual-networks-create-nsg-classic-ps.md).

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]


## <a name="create-the-nsg-frontend-nsg"></a>NSG ön uç NSG oluşturma
Oluşturmak için **NSG ön uç** NSG senaryoda gösterildiği gibi şu adımları tamamlayın:

1. Tarayıcıdan https://portal.azure.com adresine gidin ve gerekiyorsa Azure hesabınızla oturum açın.
2. Seçin **+ kaynak oluşturma >** > **ağ güvenlik grupları**.
   
    ![Azure portal - Nsg'ler](./media/virtual-networks-create-nsg-arm-pportal/figure11.png)
3. Altında **ağ güvenlik grupları**seçin **Ekle**.
   
    ![Azure portal - Nsg'ler](./media/virtual-networks-create-nsg-arm-pportal/figure12.png)
4. Altında **oluşturma ağ güvenlik grubu**, adlandırılmış bir NSG oluşturma *NSG ön uç* içinde *RG NSG* kaynak grubunu ve ardından **oluşturma** .
   
    ![Azure portal - Nsg'ler](./media/virtual-networks-create-nsg-arm-pportal/figure13.png)

## <a name="create-rules-in-an-existing-nsg"></a>Mevcut bir NSG’de kurallar oluşturma
Azure portalından var olan bir NSG kuralları oluşturmak için aşağıdaki adımları tamamlayın:

1. Seçin **tüm hizmetleri**, ardından arama **ağ güvenlik grupları**. Zaman **ağ güvenlik grupları** görünür, onu seçin.
2. Nsg'ler listesinde seçin **NSG ön uç** > **gelen güvenlik kuralları**
   
    ![Azure portal - NSG ön uç](./media/virtual-networks-create-nsg-arm-pportal/figure2.png)
3. Listesinde **gelen güvenlik kuralları**seçin **Ekle**.
   
    ![Azure portal - Kuralı Ekle](./media/virtual-networks-create-nsg-arm-pportal/figure3.png)
4. Altında **gelen Güvenlik Kuralı Ekle**, adlandırılmış bir kural oluşturmak *web kuralı* önceliğini ile *200* aracılığıyla erişime izin verme *TCP* numaralıbağlantınoktasına*80* herhangi bir VM için kaynak ve ardından **Tamam**. Bu ayarların çoğu varsayılan değerleri zaten olduğuna dikkat edin.
   
    ![Azure portal - kural ayarları](./media/virtual-networks-create-nsg-arm-pportal/figure4.png)
5. Birkaç saniye sonra yeni kural nsg'deki bakın.
   
    ![Azure portalı - yeni kural](./media/virtual-networks-create-nsg-arm-pportal/figure5.png)
6. Adlı bir gelen kuralı oluşturmak için 6 adımlarını yineleyin *rdp kural* önceliği *250* aracılığıyla erişime izin verme *TCP* bağlantı noktasına *3389* herhangi bir kaynaktan herhangi bir VM.

## <a name="associate-the-nsg-to-the-frontend-subnet"></a>NSG ile Ön Uç alt ağını ilişkilendirme

1. Seçin **tüm hizmetleri >**, girin **kaynak grupları**seçin **kaynak grupları** göründüğünde, ardından **RG NSG**.
2. Altında **RG NSG**seçin **...**   >  **TestVNet**.
   
    ![Azure portal - TestVNet](./media/virtual-networks-create-nsg-arm-pportal/figure14.png)
3. Altında **ayarları**seçin **alt ağlar** > **ön uç** > **ağ güvenlik grubu**  >  **NSG ön uç**.
   
    ![Azure portal - alt ağ ayarları](./media/virtual-networks-create-nsg-arm-pportal/figure15.png)
4. İçinde **ön uç** dikey penceresinde, select **kaydetmek**.
   
    ![Azure portal - alt ağ ayarları](./media/virtual-networks-create-nsg-arm-pportal/figure16.png)

## <a name="create-the-nsg-backend-nsg"></a>NSG arka uç NSG oluşturma
Oluşturmak için **NSG arka uç** NSG ve kendisine ilişkilendirmek **arka uç** alt ağ, aşağıdaki adımları tamamlayın:

1. Adlı bir NSG oluşturmak için *NSG arka uç*, adımlarını yineleyin [NSG ön uç NSG oluşturma](#Create-the-NSG-FrontEnd-NSG).
2. Oluşturmak için **gelen** kuralları aşağıdaki tablodaki adımları yineleyin [içinde varolan bir NSG kuralları oluşturma](#Create-rules-in-an-existing-NSG).
   
   | Gelen kuralı | Giden kuralı |
   | --- | --- |
   | ![Azure portal - gelen kuralı](./media/virtual-networks-create-nsg-arm-pportal/figure17.png) |![Azure portal - giden kuralı](./media/virtual-networks-create-nsg-arm-pportal/figure18.png) |
3. İlişkilendirilecek **NSG arka uç** NSG'yi **arka uç** alt adımlarını yineleyin [FrontEnd alt ağı için NSG ilişkilendirmek](#Associate-the-NSG-to-the-FrontEnd-subnet).

## <a name="next-steps"></a>Sonraki Adımlar
* Bilgi edinmek için nasıl [varolan Nsg'ler yönetme](manage-network-security-group.md)
* [Günlük kaydını etkinleştir](virtual-network-nsg-manage-log.md) Nsg'ler için.