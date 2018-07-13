---
title: Bir statik genel IP adresiyle - Azure portalında bir VM oluşturma | Microsoft Docs
description: Azure portalını kullanarak statik genel IP adresiyle VM oluşturma konusunda bilgi edinin.
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: e9546bcc-f300-428f-b94a-056c5bd29035
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 524293f9a1ded73ee7cb6ba4f53208a9f9c54ffa
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38670993"
---
# <a name="create-a-vm-with-a-static-public-ip-address-using-the-azure-portal"></a>Azure portalını kullanarak statik genel IP adresiyle VM oluşturma

> [!div class="op_single_selector"]
> * [Azure portal](virtual-network-deploy-static-pip-arm-portal.md)
> * [PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
> * [Azure CLI](virtual-network-deploy-static-pip-arm-cli.md)
> * [PowerShell (Klasik)](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

> [!NOTE]
> Azure’da kaynak oluşturmak ve bunlarla çalışmak için iki farklı dağıtım modeli vardır:  [Resource Manager ve klasik](../resource-manager-deployment-model.md). Bu makale Klasik dağıtım modeli yerine en yeni dağıtımlar için Microsoft'un önerdiği Resource Manager dağıtım modelini incelemektedir.

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="create-a-vm-with-a-static-public-ip"></a>Statik genel IP ile VM oluşturma

Azure portalında bir statik genel IP adresi ile bir VM oluşturmak için aşağıdaki adımları tamamlayın:

1. Tarayıcıdan [Azure portalına](https://portal.azure.com) gidin ve gerekiyorsa Azure hesabınızda oturum açın.
2. Üst sol köşesindeki portal üzerinde tıklayın **kaynak Oluştur**>>**işlem**>**Windows Server 2012 R2 Datacenter**.
3. İçinde **dağıtım modeli seçin** listesinde, seçin **Resource Manager** tıklatıp **Oluştur**.
4. İçinde **Temelleri** bölmesinde, aşağıdaki gibi VM bilgilerini girin ve ardından **Tamam**.
   
    ![Azure portalı - temel](./media/virtual-network-deploy-static-pip-arm-portal/figure1.png)
5. İçinde **bir boyut seçin** bölmesinde tıklayın **standart A1** takip etmelerin ve ardından olarak **seçin**.
   
    ![Azure portalı - bir boyut seçin](./media/virtual-network-deploy-static-pip-arm-portal/figure2.png)
6. İçinde **ayarları** bölmesinde tıklayın **genel IP adresi**, ardından **genel IP adresi oluşturma** bölmesi altında **atama**, tıklayın **Statik** gibi. Ve ardından **Tamam**.
   
    ![Azure portalı - genel IP adresi oluşturma](./media/virtual-network-deploy-static-pip-arm-portal/figure3.png)
7. İçinde **ayarları** bölmesinde tıklayın **Tamam**.
8. Gözden geçirme **özeti** bölmesi, takip etmelerin ve ardından **Tamam**.
   
    ![Azure portalı - genel IP adresi oluşturma](./media/virtual-network-deploy-static-pip-arm-portal/figure4.png)
9. Yeni kutucuğu Panonuzda dikkat edin.
   
    ![Azure portalı - genel IP adresi oluşturma](./media/virtual-network-deploy-static-pip-arm-portal/figure5.png)
10. VM oluşturulduktan sonra **ayarları** bölmesinde görüntülenir:
    
    ![Azure portalı - genel IP adresi oluşturma](./media/virtual-network-deploy-static-pip-arm-portal/figure6.png)

## <a name="set-ip-addresses-within-the-operating-system"></a>İşletim sistemi içinde IP adreslerini ayarlayın

Hiçbir zaman el ile bir Azure sanal makinesi sanal makinenin işletim sistemi içinde atanan genel IP adresi atamanız gerekir. Önerilen, statik olarak bir sanal makinenin işletim sistemi içinde Azure sanal makinesine atanmış özel IP sürece atamayın, gerekli olduğunda gibi [birden çok IP adresleri atama bir Windows VM'ye](virtual-network-multiple-ip-addresses-portal.md). İşletim sistemi içinde özel IP adresini el ile ayarlamanız durumunda Azure atanmış özel IP adresiyle aynı adresi olduğundan emin olun [ağ arabirimi](virtual-network-network-interface-addresses.md#change-ip-address-settings), ya da sanal makineye bağlantı kaybedebilir. Daha fazla bilgi edinin [özel IP adresi](virtual-network-network-interface-addresses.md#private) ayarları.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede oluşturulan VM'ye gelen ve giden ağ trafiğini akabilir. Ağ arabirimi, alt ağ veya her ikisi de gelen ve giden akış trafiğini sınırlandırmak gelen ve giden güvenlik kuralları bir ağ güvenlik grubu içinde tanımlayabilirsiniz. Ağ güvenlik grupları hakkında daha fazla bilgi için bkz. [ağ güvenlik grubu genel bakış](security-overview.md).