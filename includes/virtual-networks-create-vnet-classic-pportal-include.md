---
title: include dosyası
description: include dosyası
services: virtual-network
author: genli
ms.service: virtual-network
ms.topic: include
ms.date: 04/13/2018
ms.author: genli
ms.custom: include file
ms.openlocfilehash: edfcd4d59eceada9e1c6014dad9afa7fe8ab3c8f
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
## <a name="how-to-create-a-classic-vnet-in-the-azure-portal"></a>Azure portalında klasik bir VNet oluşturma
VNet üzerinde önceki senaryo tabanlı bir Klasik oluşturmak için aşağıdaki adımları izleyin.

1. Tarayıcıdan http://portal.azure.com adresine gidin ve gerekiyorsa Azure hesabınızla oturum açın.
2. **Kaynak oluştur** > **Ağ** > **Sanal Ağ** seçeneğine tıklayın. Dikkat **dağıtım modeli seçin** listesinde zaten gösterir **Klasik**. 3. Tıklatın **oluşturma** aşağıdaki resimde gösterildiği gibi.
   
    ![Azure portalında VNet oluşturma](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure1.gif)
4. Üzerinde **sanal ağ** bölmesi, türü **adı** VNet ve ardından **adres alanı**. VNet ve kendi ilk alt ağ, adres alanı ayarlarını yapılandırın ve ardından **Tamam**. Bizim senaryomuz için CIDR bloğu ayarları aşağıdaki şekilde gösterilmiştir.
   
    ![Adres alanı bölmesi](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure2.png)
5. Tıklatın **kaynak grubu** ve tıklayın veya Vnet'e eklenecek kaynak grubunu seçin **yeni kaynak grubu oluştur** VNet yeni bir kaynak grubuna eklemek için. Kaynak adı verilen yeni bir kaynak grubu için Grup ayarları aşağıdaki şekilde gösterilmiştir **TestRG**. Kaynak grupları hakkında daha fazla bilgi için [Azure Resource Manager’a Genel Bakış](../articles/azure-resource-manager/resource-group-overview.md#resource-groups)’ı ziyaret edin.
   
    ![Kaynak grubu bölmesi oluşturun](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure3.png)
6. Gerekiyorsa, VNet’inizle ilgili **Abonelik** ve **Konum** ayarlarını değiştirin. 
7. VNet’i bir kutucuk olarak görmek istemiyorsanız **Başlangıç panosu**’nda **Başlangıç panosuna sabitlemek** öğesini devre dışı bırakın. 
8. Tıklatın **oluşturma** ve adlı kutucuğa dikkat edin **sanal ağ oluşturuluyor** aşağıdaki resimde gösterildiği gibi.
   
    ![Portalında VNet oluşturma](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure4.png)
9. Oluşturulacak VNet için bekleyin ve döşeme gördüğünüzde, daha fazla alt ağlar eklemek için tıklatın.
   
    ![Portalında VNet oluşturma](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure5.png)
10. Görmeniz gerekir **yapılandırma** gösterildiği gibi VNet için. 
   
    ![Portalında VNet oluşturma](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure6.png)
11. Tıklatın **alt ağlar** > **Ekle**, yazın bir **adı** ve belirtin bir **adres aralığı (CIDR bloğu)** , alt ağ için ve ardından tıklatın **Tamam**. Aşağıdaki şekilde, geçerli senaryomuz için ayarları gösterilmektedir.
    
    ![Azure portalında VNet oluşturma](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure7.gif)

