---
title: Ağ güvenlik grupları - Azure portalında oluşturma | Microsoft Docs
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
ms.openlocfilehash: dd05df542327f9d8dae924b7097d247980a0558b
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="create-network-security-groups-using-the-azure-portal"></a>Ağ güvenlik grupları Azure portalını kullanarak oluşturma

[!INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Bu makalede Resource Manager dağıtım modeli anlatılmaktadır. Ayrıca [Klasik dağıtım modelinde Nsg'leri oluşturma](virtual-networks-create-nsg-classic-ps.md).

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

Aşağıdaki komutları önceden oluşturulmuş basit bir ortam beklediğiniz PowerShell örnek yukarıdaki senaryo tabanlı. Bu belgede gösterildiği komutları çalıştırmak istiyorsanız, önce test ortamında dağıtarak yapı [bu şablonu](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd), tıklatın **Azure'a Dağıt**, gerekirse varsayılan parametre değerlerini değiştirin ve Portalı'ndaki yönergeleri izleyin. Aşağıdaki adımları kullanın **RG NSG** şablonu için dağıtılan kaynak grubunun adı.

## <a name="create-the-nsg-frontend-nsg"></a>NSG ön uç NSG oluşturma
Oluşturmak için **NSG ön uç** NSG yukarıdaki senaryoda gösterildiği gibi aşağıdaki adımları izleyin.

1. Tarayıcıdan http://portal.azure.com adresine gidin ve gerekiyorsa Azure hesabınızla oturum açın.
2. Tıklatın **Gözat >** > **ağ güvenlik grupları**.
   
    ![Azure portal - Nsg'ler](./media/virtual-networks-create-nsg-arm-pportal/figure11.png)
3. İçinde **ağ güvenlik grupları** dikey penceresinde tıklatın **Ekle**.
   
    ![Azure portal - Nsg'ler](./media/virtual-networks-create-nsg-arm-pportal/figure12.png)
4. İçinde **oluşturma ağ güvenlik grubu** dikey penceresinde adlı bir NSG oluşturma *NSG ön uç* içinde *RG NSG* kaynak grubu ve ardından **oluşturma**.
   
    ![Azure portal - Nsg'ler](./media/virtual-networks-create-nsg-arm-pportal/figure13.png)

## <a name="create-rules-in-an-existing-nsg"></a>Mevcut bir NSG’de kurallar oluşturma
Azure portalından var olan bir NSG kuralları oluşturmak için aşağıdaki adımları izleyin.

1. Tıklatın **Gözat >** > **ağ güvenlik grupları**.
2. Nsg'ler listesinde tıklatın **NSG ön uç** > **gelen güvenlik kuralları**
   
    ![Azure portal - NSG ön uç](./media/virtual-networks-create-nsg-arm-pportal/figure2.png)
3. Listesinde **gelen güvenlik kuralları**, tıklatın **Ekle**.
   
    ![Azure portal - Kuralı Ekle](./media/virtual-networks-create-nsg-arm-pportal/figure3.png)
4. İçinde **gelen Güvenlik Kuralı Ekle** dikey penceresinde adlı bir kural oluşturmak *web kuralı* önceliğini ile *200* aracılığıyla erişime izin verme *TCP* bağlantı noktasına *80* herhangi bir VM için kaynak ve ardından **Tamam**. Bu ayarların çoğu varsayılan değerleri zaten olduğuna dikkat edin.
   
    ![Azure portal - kural ayarları](./media/virtual-networks-create-nsg-arm-pportal/figure4.png)
5. Birkaç saniye sonra yeni kural nsg'deki görürsünüz.
   
    ![Azure portalı - yeni kural](./media/virtual-networks-create-nsg-arm-pportal/figure5.png)
6. Adlı bir gelen kuralı oluşturmak için 6 adımlarını yineleyin *rdp kural* önceliği *250* aracılığıyla erişime izin verme *TCP* bağlantı noktasına *3389* herhangi bir kaynaktan herhangi bir VM.

## <a name="associate-the-nsg-to-the-frontend-subnet"></a>NSG ile Ön Uç alt ağını ilişkilendirme
1. Tıklatın **Gözat >** > **kaynak grupları** > **RG NSG**.
2. İçinde **RG NSG** dikey penceresinde tıklatın **...**   >  **TestVNet**.
   
    ![Azure portal - TestVNet](./media/virtual-networks-create-nsg-arm-pportal/figure14.png)
3. İçinde **ayarları** dikey penceresinde tıklatın **alt ağlar** > **ön uç** > **ağ güvenlik grubu** > **NSG ön uç**.
   
    ![Azure portal - alt ağ ayarları](./media/virtual-networks-create-nsg-arm-pportal/figure15.png)
4. İçinde **ön uç** dikey penceresinde tıklatın **kaydetmek**.
   
    ![Azure portal - alt ağ ayarları](./media/virtual-networks-create-nsg-arm-pportal/figure16.png)

## <a name="create-the-nsg-backend-nsg"></a>NSG arka uç NSG oluşturma
Oluşturmak için **NSG arka uç** NSG ve kendisine ilişkilendirmek **arka uç** alt ağ, aşağıdaki adımları izleyin.

1. Adımlarını yineleyin [NSG ön uç NSG oluşturma](#Create-the-NSG-FrontEnd-NSG) adlı bir NSG oluşturmak için *NSG arka uç*
2. Adımlarını yineleyin [içinde varolan bir NSG kuralları oluşturma](#Create-rules-in-an-existing-NSG) oluşturmak için **gelen** aşağıdaki tabloda kurallar.
   
   | Gelen kuralı | Giden kuralı |
   | --- | --- |
   | ![Azure portal - gelen kuralı](./media/virtual-networks-create-nsg-arm-pportal/figure17.png) |![Azure portal - giden kuralı](./media/virtual-networks-create-nsg-arm-pportal/figure18.png) |
3. Adımlarını yineleyin [FrontEnd alt ağı için NSG ilişkilendirmek](#Associate-the-NSG-to-the-FrontEnd-subnet) ilişkilendirilecek **NSG arka uç** NSG'yi **arka uç** alt ağ.

## <a name="next-steps"></a>Sonraki Adımlar
* Bilgi edinmek için nasıl [varolan Nsg'ler yönetme](manage-network-security-group.md)
* [Günlük kaydını etkinleştir](virtual-network-nsg-manage-log.md) Nsg'ler için.

