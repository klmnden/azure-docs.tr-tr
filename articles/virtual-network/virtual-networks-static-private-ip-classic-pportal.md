---
title: Özel IP adresleri yapılandırmak için VM'ler (Klasik) - Azure portalı | Microsoft Docs
description: Azure portalını kullanarak sanal makineler (Klasik) için özel IP adresleri yapılandırmayı öğrenin.
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
ms.openlocfilehash: 72d1c4d2ea3adf7d8751adfbb013435f8f2530f0
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62125755"
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-classic-using-the-azure-portal"></a>Azure portalını kullanarak sanal makine (Klasik) için özel IP adreslerini yapılandırın

[!INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Bu makale, klasik dağıtım modelini kapsamaktadır. Ayrıca [Resource Manager dağıtım modelinde statik özel IP adresi yönetme](virtual-networks-static-private-ip-arm-pportal.md).

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Aşağıdaki örnek adımları zaten oluşturulmuş basit bir ortam bekler. Açıklanan test ortamı adımları bu belgede gösterildiği çalıştırmak istiyorsanız, öncelikle yapı [vnet oluşturma](virtual-networks-create-vnet-classic-pportal.md).

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Bir VM oluşturulurken özel bir statik IP adresi belirtme
Adlı bir VM oluşturmak için *DNS01* içinde *ön uç* adlı bir sanal ağ alt ağı *TestVNet* bir statik özel IP'si ile *192.168.1.101*tam Aşağıdaki adımlar:

1. Tarayıcıdan https://portal.azure.com adresine gidin ve gerekiyorsa Azure hesabınızla oturum açın.
2. Seçin **yeni** > **işlem** > **Windows Server 2012 R2 Datacenter**, dikkat **dağıtımmodeliseçin** listesinde zaten gösterir **Klasik**ve ardından **Oluştur**.
   
    ![Azure portalını kullanarak VM oluşturma](./media/virtual-networks-static-ip-classic-pportal/figure01.png)
3. Altında **VM Oluştur**, oluşturulacak VM adını girin (*DNS01* senaryoda), yerel yönetici hesabı ve parolası.
   
    ![Azure portalını kullanarak VM oluşturma](./media/virtual-networks-static-ip-classic-pportal/figure02.png)
4. Seçin **isteğe bağlı yapılandırma** > **ağ** > **sanal ağ**ve ardından **TestVNet** . Varsa **TestVNet** kullanılamaz kullandığınızdan emin olun *Orta ABD* konumu ve bu makalenin başında açıklanan test ortamında oluşturdunuz.
   
    ![Azure portalını kullanarak VM oluşturma](./media/virtual-networks-static-ip-classic-pportal/figure03.png)
5. Altında **ağ**, şu anda seçilen alt ağda olduğundan emin olun *ön uç*, ardından **IP adresleri**altında **IP adresi ataması** seçin **statik**yazıp enter *192.168.1.101* için **IP adresi** aşağıda görüldüğü gibi.
   
    ![Azure portalını kullanarak VM oluşturma](./media/virtual-networks-static-ip-classic-pportal/figure04.png)    
6. Seçin **Tamam** altında **IP adresleri**seçin **Tamam** altında **ağ**ve ardından **Tamam** altında **İsteğe bağlı yapılandırma**.
7. Altında **VM Oluştur**seçin **Oluştur**. Panonuzda görüntülenen döşemenin aşağıdaki noktalara dikkat edin:
   
    ![Azure portalını kullanarak VM oluşturma](./media/virtual-networks-static-ip-classic-pportal/figure05.png)

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Bir sanal makine için statik özel IP adresi bilgilerini alma
Yukarıdaki adımları ile oluşturulan sanal makine için statik özel IP adresi bilgilerini görüntülemek için aşağıdaki adımları yürütün.

1. Azure portalından seçin **TÜMÜNE Gözat** > **sanal makineler (Klasik)** > **DNS01** > **tüm ayarları** > **IP adresleri** ve aşağıda görüldüğü gibi IP adresi ve IP adresi ataması dikkat edin.
   
    ![Azure portalını kullanarak VM oluşturma](./media/virtual-networks-static-ip-classic-pportal/figure06.png)

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>VM'den özel statik IP adresi kaldırma

Altında **IP adresleri**seçin **dinamik** sağındaki **IP adresi ataması**seçin **Kaydet**ve ardından  **Evet**, aşağıdaki resimde gösterildiği gibi:
   
    ![Create VM in Azure portal](./media/virtual-networks-static-ip-classic-pportal/figure07.png)

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Mevcut bir VM'ye statik özel IP adresi ekleme

1. Altında **IP adresleri**seçin, daha önce gösterilen **statik** sağındaki **IP adresi ataması**.
2. Tür *192.168.1.101* için **IP adresi**seçin **Kaydet**ve ardından **Evet**.

## <a name="set-ip-addresses-within-the-operating-system"></a>İşletim sistemi içinde IP adreslerini ayarlayın

Statik olarak bir sanal makinenin işletim sistemi içinde Azure sanal makinesine atanmış özel IP sürece atamayın, önerilen gerekli. İşletim sistemi içinde özel IP adresini el ile ayarlamanız durumunda Azure VM'sine atanan özel IP adresiyle aynı adresi olduğundan emin olun veya sanal makineye bağlantı kaybedebilir. Hiçbir zaman el ile bir Azure sanal makinesi sanal makinenin işletim sistemi içinde atanan genel IP adresi atamanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [ayrılmış genel IP](virtual-networks-reserved-public-ip.md) adresleri.
* Hakkında bilgi edinin [örnek düzeyi genel IP (ILPIP)](virtual-networks-instance-level-public-ip.md) adresleri.
* Başvurun [ayrılmış IP REST API'leri](https://msdn.microsoft.com/library/azure/dn722420.aspx).

