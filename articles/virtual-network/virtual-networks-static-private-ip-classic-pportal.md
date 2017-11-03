---
title: "Özel IP adreslerini VM'ler için (Klasik) - Azure portalında yapılandırın | Microsoft Docs"
description: "Azure Portalı'nı kullanarak sanal makineleri (Klasik) için özel IP adresleri yapılandırmayı öğrenin."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: b8ef8367-58b2-42df-9f26-3269980950b8
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bde6de3495c2909b63b1f85e420a4ff5e7ac2c1a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-classic-using-the-azure-portal"></a>Azure Portalı'nı kullanarak sanal makine (Klasik) için özel IP adreslerini yapılandırın

[!INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Bu makale, klasik dağıtım modelini kapsamaktadır. Ayrıca [Resource Manager dağıtım modelinde statik özel IP adresi yönetmek](virtual-networks-static-private-ip-arm-pportal.md).

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Aşağıdaki örnek adımları zaten oluşturulmuş basit bir ortam bekler. Bu belgede gösterildiği adımları çalıştırmak istiyorsanız, önce açıklanan test ortamı oluşturmak [vnet oluşturma](virtual-networks-create-vnet-classic-pportal.md).

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Bir VM oluşturulurken özel bir statik IP adresi belirtme
Adlı bir VM oluşturmak için *DNS01* içinde *ön uç* adlı bir sanal ağ alt ağı *TestVNet* özel bir statik IP *192.168.1.101*, aşağıdaki adımları izleyin:

1. Tarayıcıdan http://portal.azure.com adresine gidin ve gerekiyorsa Azure hesabınıza giriş yapın.
2. Tıklatın **yeni** > **işlem** > **Windows Server 2012 R2 Datacenter**, dikkat **dağıtımmodeliseçin** listesinde zaten gösterir **Klasik**ve ardından **oluşturma**.
   
    ![Azure Portalı'nda VM oluşturma](./media/virtual-networks-static-ip-classic-pportal/figure01.png)
3. İçinde **VM Oluştur** dikey penceresinde oluşturulacak VM adını girin (*DNS01* bizim senaryoda), yerel yönetici hesabı ve parolası.
   
    ![Azure Portalı'nda VM oluşturma](./media/virtual-networks-static-ip-classic-pportal/figure02.png)
4. Tıklatın **isteğe bağlı yapılandırma** > **ağ** > **sanal ağ**ve ardından **TestVNet**. Varsa **TestVNet** kullanılamıyor kullandığınızdan emin olun *Orta ABD* konumu ve bu makalenin başında açıklanan test ortamında oluşturdunuz.
   
    ![Azure Portalı'nda VM oluşturma](./media/virtual-networks-static-ip-classic-pportal/figure03.png)
5. İçinde **ağ** dikey penceresinde seçili alt ağı olduğundan emin olun *ön uç*, ardından **IP adreslerini**altında **IP adresi ataması**tıklatın **statik**ve ardından girin *192.168.1.101* için **IP adresi** aşağıda görüldüğü gibi.
   
    ![Azure Portalı'nda VM oluşturma](./media/virtual-networks-static-ip-classic-pportal/figure04.png)    
6. ' I tıklatın **Tamam** içinde **IP adreslerini** dikey penceresinde, ardından **Tamam** içinde **ağ** dikey penceresinde'ye tıklayın **Tamam** içinde **isteğe bağlı yapılandırma** dikey.
7. İçinde **VM Oluştur** dikey penceresinde tıklatın **oluşturma**. Panonuzda görüntülenen döşemenin aşağıdaki dikkat edin.
   
    ![Azure Portalı'nda VM oluşturma](./media/virtual-networks-static-ip-classic-pportal/figure05.png)

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Bir VM için özel statik IP adresi bilgilerini alma
Yukarıdaki adımları ile oluşturulan sanal makine için statik özel IP adresi bilgilerini görüntülemek için aşağıdaki adımları yürütün.

1. Azure Azure Portalı'ndan tıklatın **TÜMÜNE Gözat** > **sanal makineleri (Klasik)** > **DNS01** > **tüm ayarları** > **IP adreslerini** ve IP adresi ve IP adresi ataması aşağıda görüldüğü gibi dikkat edin.
   
    ![Azure Portalı'nda VM oluşturma](./media/virtual-networks-static-ip-classic-pportal/figure06.png)

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Özel bir statik IP adresi bir VM'den kaldırma
Özel statik IP adresi yukarıda oluşturduğunuz sanal makineden kaldırmak için aşağıdaki adımları izleyin.

1. Gelen **IP adreslerini** , yukarıda gösterilen dikey tıklayın **dinamik** sağındaki **IP adresi ataması**, ardından **kaydetmek**ve ardından **Evet**.
   
    ![Azure Portalı'nda VM oluşturma](./media/virtual-networks-static-ip-classic-pportal/figure07.png)

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Mevcut bir VM'yi özel bir statik IP adresi ekleme
Yukarıdaki adımları kullanılarak oluşturulan VM için özel bir statik IP adresi eklemek için aşağıdaki adımları izleyin:

1. Gelen **IP adreslerini** , yukarıda gösterilen dikey tıklayın **statik** sağındaki **IP adresi ataması**.
2. Tür *192.168.1.101* için **IP adresi**, ardından **kaydetmek**ve ardından **Evet**.

## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [ayrılmış genel IP](virtual-networks-reserved-public-ip.md) adresleri.
* Hakkında bilgi edinin [örnek düzeyinde ortak IP (ILPIP)](virtual-networks-instance-level-public-ip.md) adresleri.
* Başvurun [ayrılmış IP REST API'leri](https://msdn.microsoft.com/library/azure/dn722420.aspx).

