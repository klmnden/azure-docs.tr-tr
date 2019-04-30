---
title: include dosyası
description: include dosyası
services: virtual-network
author: genlin
ms.service: virtual-network
ms.topic: include
ms.date: 04/13/2018
ms.author: genli
ms.custom: include file
ms.openlocfilehash: aa88bf3bd6c5037b41c09e9ffe47921f1b9dc9be
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60937530"
---
## <a name="how-to-create-a-classic-vnet-in-the-azure-portal"></a>Azure portalında bir Klasik sanal ağ oluşturma
Bir Klasik sanal ağ, yukarıdaki senaryo temel alınarak oluşturmak için aşağıdaki adımları izleyin.

1. Tarayıcıdan http://portal.azure.com adresine gidin ve gerekiyorsa Azure hesabınızla oturum açın.
2. **Kaynak oluştur** > **Ağ** > **Sanal Ağ** seçeneğine tıklayın. Dikkat **dağıtım modeli seçin** listesinde zaten gösterir **Klasik**. 3. Tıklayın **Oluştur** aşağıdaki şekilde gösterildiği gibi.
   
    ![Azure portalında VNet oluşturma](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure1.gif)
4. Üzerinde **sanal ağ** bölmesinde, türü **adı** VNet ve ardından **adres alanı**. Sanal ağ ile ilk alt ağ için adres alanı ayarlarınızı yapılandırın, ardından tıklayın **Tamam**. Bizim senaryomuz için CIDR bloğu ayarlarını aşağıdaki şekilde gösterilmektedir.
   
    ![Adres alanı bölmesi](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure2.png)
5. Tıklayın **kaynak grubu** ve Vnet'e eklemek veya bir kaynak grubu seçin **yeni kaynak grubu oluştur** Vnet'i yeni bir kaynak grubuna eklemek için. Adlı yeni bir kaynak grubu için Grup ayarlarını kaynağı aşağıdaki şekilde gösterilmiştir **TestRG**. Kaynak grupları hakkında daha fazla bilgi için [Azure Resource Manager’a Genel Bakış](../articles/azure-resource-manager/resource-group-overview.md#resource-groups)’ı ziyaret edin.
   
    ![Kaynak grubu bölmesi oluşturun](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure3.png)
6. Gerekiyorsa, VNet’inizle ilgili **Abonelik** ve **Konum** ayarlarını değiştirin. 
7. VNet’i bir kutucuk olarak görmek istemiyorsanız **Başlangıç panosu**’nda **Başlangıç panosuna sabitlemek** öğesini devre dışı bırakın. 
8. Tıklayın **Oluştur** ve adlı kutucuğa dikkat edin **sanal ağ oluşturuluyor** aşağıdaki şekilde gösterildiği gibi.
   
    ![Portalında VNet oluşturma](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure4.png)
9. Oluşturulacak sanal ağ için bekleyin ve kutucuk gördüğünüzde, daha fazla alt ağı eklemek için tıklayın.
   
    ![Portalında VNet oluşturma](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure5.png)
10. Görmelisiniz **yapılandırma** gösterildiği gibi VNet için. 
   
    ![Portalında VNet oluşturma](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure6.png)
11. Tıklayın **alt ağlar** > **Ekle**, yazarak bir **adı** belirtin bir **adres aralığı (CIDR bloğu)** alt ağınızın ve ardından tıklayın **Tamam**. Aşağıdaki şekilde, geçerli senaryomuz için ayarları gösterir.
    
    ![Azure portalında VNet oluşturma](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure7.gif)

