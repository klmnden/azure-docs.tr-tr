---
title: Özel IP adreslerini VM'ler için (Klasik) - Azure portalında yapılandırın | Microsoft Docs
description: Azure Portalı'nı kullanarak sanal makineleri (Klasik) için özel IP adresleri yapılandırmayı öğrenin.
services: virtual-network
documentationcenter: na
author: genlin
manager: cshepard
editor: tysonn
tags: azure-service-management
ms.assetid: b8ef8367-58b2-42df-9f26-3269980950b8
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: genli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1aae74d8077a75e5ab556703b1c1531f540bbdb4
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
ms.locfileid: "31791908"
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-classic-using-the-azure-portal"></a>Azure Portalı'nı kullanarak sanal makine (Klasik) için özel IP adreslerini yapılandırın

[!INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Bu makale, klasik dağıtım modelini kapsamaktadır. Ayrıca [Resource Manager dağıtım modelinde statik özel IP adresi yönetmek](virtual-networks-static-private-ip-arm-pportal.md).

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Örnek adımları zaten oluşturulmuş basit bir ortam bekler. Bu belgede gösterildiği adımları çalıştırmak istiyorsanız, önce açıklanan test ortamı oluşturmak [vnet oluşturma](virtual-networks-create-vnet-classic-pportal.md).

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Bir VM oluşturulurken özel bir statik IP adresi belirtme
Adlı bir VM oluşturmak için *DNS01* içinde *ön uç* adlı bir sanal ağ alt ağı *TestVNet* özel bir statik IP *192.168.1.101*, tamamlandı Aşağıdaki adımlar:

1. Tarayıcıdan https://portal.azure.com adresine gidin ve gerekiyorsa Azure hesabınızla oturum açın.
2. Seçin **yeni** > **işlem** > **Windows Server 2012 R2 Datacenter**, dikkat **dağıtımmodeliseçin** listesinde zaten gösterir **Klasik**ve ardından **oluşturma**.
   
    ![Azure Portalı'nda VM oluşturma](./media/virtual-networks-static-ip-classic-pportal/figure01.png)
3. Altında **VM Oluştur**, oluşturulacak VM adını girin (*DNS01* senaryoda), yerel yönetici hesabı ve parolası.
   
    ![Azure Portalı'nda VM oluşturma](./media/virtual-networks-static-ip-classic-pportal/figure02.png)
4. Seçin **isteğe bağlı yapılandırma** > **ağ** > **sanal ağ**ve ardından **TestVNet** . Varsa **TestVNet** kullanılamıyor kullandığınızdan emin olun *Orta ABD* konumu ve bu makalenin başında açıklanan test ortamında oluşturdunuz.
   
    ![Azure Portalı'nda VM oluşturma](./media/virtual-networks-static-ip-classic-pportal/figure03.png)
5. Altında **ağ**, şu anda seçilen alt ağ olduğundan emin olun *ön uç*seçeneğini belirleyip **IP adreslerini**altında **IP adresi ataması** seçin **statik**ve ardından girin *192.168.1.101* için **IP adresi** aşağıda görüldüğü gibi.
   
    ![Azure Portalı'nda VM oluşturma](./media/virtual-networks-static-ip-classic-pportal/figure04.png)    
6. Seçin **Tamam** altında **IP adreslerini**seçin **Tamam** altında **ağ**ve ardından **Tamam** altında **İsteğe bağlı yapılandırma**.
7. Altında **VM Oluştur**seçin **oluşturma**. Panonuzda görüntülenen döşemenin aşağıdaki noktalara dikkat edin:
   
    ![Azure Portalı'nda VM oluşturma](./media/virtual-networks-static-ip-classic-pportal/figure05.png)

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Bir VM için özel statik IP adresi bilgilerini alma
Yukarıdaki adımları ile oluşturulan sanal makine için statik özel IP adresi bilgilerini görüntülemek için aşağıdaki adımları yürütün.

1. Azure portalından seçin **TÜMÜNE Gözat** > **sanal makineleri (Klasik)** > **DNS01** > **tüm ayarları** > **IP adreslerini** ve IP adresi ve IP adresi ataması aşağıda görüldüğü gibi dikkat edin.
   
    ![Azure Portalı'nda VM oluşturma](./media/virtual-networks-static-ip-classic-pportal/figure06.png)

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Özel bir statik IP adresi bir VM'den kaldırma

Altında **IP adreslerini**seçin **dinamik** sağındaki **IP adresi ataması**seçin **kaydetmek**ve ardından  **Evet**, aşağıdaki resimde gösterildiği gibi:
   
    ![Create VM in Azure portal](./media/virtual-networks-static-ip-classic-pportal/figure07.png)

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Mevcut bir VM'yi özel bir statik IP adresi ekleme

1. Altında **IP adreslerini**, daha önce gösterilen, select **statik** sağındaki **IP adresi ataması**.
2. Tür *192.168.1.101* için **IP adresi**seçin **kaydetmek**ve ardından **Evet**.

## <a name="set-ip-addresses-within-the-operating-system"></a>İşletim sistemi içinde IP adreslerini ayarlayın

Statik olarak bir VM işletim sistemi içinde Azure sanal makineye atanan özel IP sürece atadığınız değil, önerilen gerekli. İşletim sistemi içinde özel IP adresini el ile ayarlayın, Azure VM'ye atanan özel IP adresi aynı adresi olduğundan emin olun veya sanal makineye bağlantısını kaybedebilir. Hiçbir zaman el ile bir Azure sanal makinesi sanal makinenin işletim sistemi içinde atanan genel IP adresi atamanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [ayrılmış genel IP](virtual-networks-reserved-public-ip.md) adresleri.
* Hakkında bilgi edinin [örnek düzeyinde ortak IP (ILPIP)](virtual-networks-instance-level-public-ip.md) adresleri.
* Başvurun [ayrılmış IP REST API'leri](https://msdn.microsoft.com/library/azure/dn722420.aspx).

