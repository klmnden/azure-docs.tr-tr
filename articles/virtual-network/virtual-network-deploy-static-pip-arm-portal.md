---
title: "Bir statik genel IP adresi ile - Azure portalında bir VM oluşturma | Microsoft Docs"
description: "Azure Portalı'nı kullanarak bir statik genel IP adresi ile VM oluşturmayı öğrenin."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
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
ms.openlocfilehash: 53e9b55a82e7750393dfb6c4818681028c5c4d2f
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="create-a-vm-with-a-static-public-ip-address-using-the-azure-portal"></a>Azure Portalı'nı kullanarak bir statik genel IP adresiyle bir VM oluşturma

> [!div class="op_single_selector"]
> * [Azure portalı](virtual-network-deploy-static-pip-arm-portal.md)
> * [PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
> * [Azure CLI](virtual-network-deploy-static-pip-arm-cli.md)
> * [Şablon](virtual-network-deploy-static-pip-arm-template.md)
> * [PowerShell (Klasik)](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

> [!NOTE]
> Azure’da kaynak oluşturmak ve bunlarla çalışmak için iki farklı dağıtım modeli vardır:  [Resource Manager ve klasik](../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli yerine en yeni dağıtımlar için Microsoft önerir Resource Manager dağıtım modelini kullanarak yer almaktadır.

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="create-a-vm-with-a-static-public-ip"></a>Bir statik genel IP ile bir VM oluşturma

Azure portalında bir statik genel IP adresine sahip bir VM oluşturmak için aşağıdaki adımları tamamlayın:

1. Tarayıcıdan [Azure portalına](https://portal.azure.com) gidin ve gerekiyorsa Azure hesabınızda oturum açın.
2. Üst köşesindeki portalı üzerinde tıklatın **kaynak oluşturma**>>**işlem**>**Windows Server 2012 R2 Datacenter**.
3. İçinde **dağıtım modeli seçin** listesinde, seçin **Resource Manager** tıklatıp **oluşturma**.
4. İçinde **Temelleri** bölmesinde, aşağıdaki gibi VM bilgileri girin ve ardından **Tamam**.
   
    ![Azure portal - temelleri](./media/virtual-network-deploy-static-pip-arm-portal/figure1.png)
5. İçinde **bir boyutu seçin** bölmesinde tıklatın **A1 standart** aşağıdaki ve ardından olarak **seçin**.
   
    ![Azure portal - bir boyutu seçin](./media/virtual-network-deploy-static-pip-arm-portal/figure2.png)
6. İçinde **ayarları** bölmesinde, tıklatın **genel IP adresi**, sonra **ortak IP adresi oluştur** bölmesi altında **atama**, 'ıtıklatın **Statik** olarak gibi. Ve ardından **Tamam**.
   
    ![Azure portal - ortak IP adresi oluştur](./media/virtual-network-deploy-static-pip-arm-portal/figure3.png)
7. İçinde **ayarları** bölmesinde tıklatın **Tamam**.
8. Gözden geçirme **Özet** bölmesinde, aşağıdaki ve ardından olarak **Tamam**.
   
    ![Azure portal - ortak IP adresi oluştur](./media/virtual-network-deploy-static-pip-arm-portal/figure4.png)
9. Yeni bir kutucuk Panonuzda dikkat edin.
   
    ![Azure portal - ortak IP adresi oluştur](./media/virtual-network-deploy-static-pip-arm-portal/figure5.png)
10. VM oluşturulduktan sonra **ayarları** bölmesi görüntüler:
    
    ![Azure portal - ortak IP adresi oluştur](./media/virtual-network-deploy-static-pip-arm-portal/figure6.png)

