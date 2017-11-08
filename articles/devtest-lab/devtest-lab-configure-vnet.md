---
title: "Azure DevTest Labs'de sanal ağ yapılandırma | Microsoft Docs"
description: "Bir varolan sanal ağ ve alt yapılandırmak ve bir VM'de Azure DevTest Labs kullanılacakları hakkında bilgi edinin"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 6cda99c2-b87e-4047-90a0-5df10d8e9e14
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/30/2017
ms.author: tarcher
ms.openlocfilehash: 4b4c91805a7d5cbf37c8ba3fa3248e7cb0eb02b0
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2017
---
# <a name="configure-a-virtual-network-in-azure-devtest-labs"></a>Azure DevTest Labs'de sanal ağ yapılandırma
Makalesinde açıklandığı gibi [yapı içeren bir VM'yi laboratuvara ekleme](devtest-lab-add-vm-with-artifacts.md), bir laboratuar ortamında bir VM oluştururken yapılandırılmış bir sanal ağ belirtin. Örneğin, ExpressRoute veya siteden siteye VPN ile yapılandırılmış sanal ağını kullanarak, Vm'lerde, corpnet kaynaklarına gerekebilir.

Bu makalede, varolan sanal ağınızda, Laboratuvar ait sanal ağ ayarlarını ekleyebilirsiniz, böylece sanal makineleri oluştururken seçtiğiniz kullanılabilir açıklanmaktadır.

## <a name="configure-a-virtual-network-for-a-lab-using-the-azure-portal"></a>Azure Portalı'nı kullanarak bir laboratuvar için bir sanal ağ yapılandırma
Aşağıdaki adımlar, böylece aynı laboratuar ortamında bir VM oluştururken, kullanılabilir var olan sanal ağ (ve alt ağ), laboratuvara eklerken size yol. 

1. [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
1. Seçin **daha Hizmetleri**ve ardından **DevTest Labs** listeden.
1. İstenen Laboratuvar labs listesinden seçin. 
1. Laboratuvar ait ana bölmede, seçin **yapılandırma ve ilkeleri**.

    ![Laboratuvar ait yapılandırma ve ilkeleri erişim](./media/devtest-lab-configure-vnet/policies-menu.png)
1. İçinde **dış KAYNAKLARA** bölümünde, select **sanal ağlar**. Laboratuvarınız için oluşturulan varsayılan sanal ağ yanı sıra geçerli Laboratuvar için yapılandırılan sanal ağlar listesi görüntülenir. 
1. Seçin **+ Ekle**.
   
    ![Laboratuvarınız için varolan bir sanal ağ ekleme](./media/devtest-lab-configure-vnet/lab-settings-vnet-add.png)
1. Üzerinde **sanal ağ** bölmesinde, **[Select sanal ağ]**.
   
    ![Varolan bir sanal ağı seçin](./media/devtest-lab-configure-vnet/lab-settings-vnets-vnet1.png)
1. Üzerinde **Seç sanal ağ** bölmesinde, istenen sanal ağı seçin. Tüm abonelik Laboratuvar olarak aynı bölgede altındaki sanal ağlar gösteren bir liste görüntülenir.
1. Bir sanal ağı seçtikten sonra döndürülürsünüz **sanal ağ** bölmesi. Alt kısımdaki listesinden seçin.

    ![Alt ağ listesi](./media/devtest-lab-configure-vnet/lab-settings-vnets-vnet2.png)
    
    Laboratuvar alt bölmesinde görüntülenir.

    ![Laboratuvar alt bölmesi](./media/devtest-lab-configure-vnet/lab-subnet.png)
     
   - Belirtin bir **Laboratuvar alt ağ adı**.
   - VM oluşturma laboratuvarda kullanılacak bir alt ağ izin vermek için seçin **sanal makine oluşturma kullanımda**.
   - Etkinleştirmek için bir [genel IP adresi paylaşılan](devtest-lab-shared-ip.md)seçin **etkinleştir paylaşılan ortak IP**.
   - Genel IP adresleri bir alt ağda izin vermek için seçin **ortak IP oluşturmaya izin**.
   - İçinde **kullanıcı başına en fazla sanal makine** alan, her alt ağ için kullanıcı başına en fazla VMs belirtin. Sanal makineleri sınırsız sayıda istiyorsanız bu alanı boş bırakın.
1. Seçin **Tamam** Laboratuvar alt bölmesini kapatın.
1. Seçin **kaydetmek** sanal ağ bölmesini kapatın.

Sanal ağ yapılandırıldıysa, bir VM oluşturulurken seçilebilir. Bir VM oluşturma ve bir sanal ağ belirtin, makalesine başvurun görmek için [yapı içeren bir VM'yi laboratuvara ekleme](devtest-lab-add-vm-with-artifacts.md). 

Azure'nın [sanal ağ belgeleri](https://docs.microsoft.com/azure/virtual-network) ayarlamak ve VNet yönetmek ve şirket içi ağınıza bağlanmak nasıl dahil olmak üzere sanal ağlar kullanma hakkında daha fazla bilgi sağlar.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Sonraki adımlar
Laboratuvarınız için istenen sanal ağ ekledikten sonra sıradaki adım olacak [Laboratuvarınızı için bir VM eklemek](devtest-lab-add-vm-with-artifacts.md).

