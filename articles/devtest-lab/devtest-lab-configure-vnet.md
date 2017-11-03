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
ms.date: 03/16/2017
ms.author: tarcher
ms.openlocfilehash: 19dbb1625f46f8864413dc538a96b2413bc6eea0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="configure-a-virtual-network-in-azure-devtest-labs"></a>Azure DevTest Labs'de sanal ağ yapılandırma
Makalesinde açıklandığı gibi [yapı içeren bir VM'yi laboratuvara ekleme](devtest-lab-add-vm-with-artifacts.md), bir laboratuar ortamında bir VM oluştururken yapılandırılmış bir sanal ağ belirtin. Bunu yapmak için bir ExpressRoute veya siteden siteye VPN ile yapılandırılmış sanal ağını kullanarak, vm'lerden corpnet kaynaklarınıza erişmek gereken bir senaryodur. Aşağıdaki bölümlerde, böylece sanal makineleri oluştururken seçmek kullanılabilir, varolan sanal ağınızda Laboratuvar ait sanal ağ ayarlarını ekleme gösterilmektedir.

## <a name="configure-a-virtual-network-for-a-lab-using-the-azure-portal"></a>Azure Portalı'nı kullanarak bir laboratuvar için bir sanal ağ yapılandırma
Aşağıdaki adımlar, böylece aynı laboratuar ortamında bir VM oluştururken, kullanılabilir var olan sanal ağ (ve alt ağ), laboratuvara eklerken size yol. 

1. [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
2. Seçin **daha Hizmetleri**ve ardından **DevTest Labs** listeden.
3. İstenen Laboratuvar labs listesinden seçin. 
4. Laboratuvar 's dikey penceresinde, seçin **yapılandırma ve ilkeleri**.
5. Laboratuvar 's üzerinde **yapılandırma ve ilkeleri** dikey penceresinde, select **sanal ağlar**.
6. Üzerinde **sanal ağlar** dikey penceresinde, geçerli Laboratuvar ve bunun yanı sıra, laboratuvarınız için oluşturulan varsayılan sanal ağ için yapılandırılan sanal ağların bir listesini görürsünüz. 
7. Seçin **+ Ekle**.
   
    ![Laboratuvarınız için varolan bir sanal ağ ekleme](./media/devtest-lab-configure-vnet/lab-settings-vnet-add.png)
8. Üzerinde **sanal ağ** dikey penceresinde, select **[Select sanal ağ]**.
   
    ![Varolan bir sanal ağı seçin](./media/devtest-lab-configure-vnet/lab-settings-vnets-vnet1.png)
9. Üzerinde **Seç sanal ağ** dikey penceresinde istenen sanal ağı seçin. Dikey Laboratuvar olarak abonelik aynı bölgede altındaki tüm sanal ağları gösterir.  
10. Bir sanal ağı seçtikten sonra döndürülürsünüz **sanal ağ** dikey pencerenin altındaki listesinde alt ağına tıklayın.

    ![Alt ağ listesi](./media/devtest-lab-configure-vnet/lab-settings-vnets-vnet2.png)
    
    Laboratuvar alt dikey penceresinde görüntülenir.

    ![Laboratuvar alt dikey penceresi](./media/devtest-lab-configure-vnet/lab-subnet.png)

11. Belirtin bir **Laboratuvar alt ağ adı**.
12. VM oluşturma laboratuvarda kullanılacak bir alt ağ izin vermek için seçin **sanal makine oluşturma kullanımda**.
13. Etkinleştirmek için bir [genel IP adresi paylaşılan](devtest-lab-shared-ip.md)seçin **etkinleştir paylaşılan ortak IP**.
14. Genel IP adresleri bir alt ağda izin vermek için seçin **ortak IP oluşturmaya izin**.
15. İçinde **kullanıcı başına en fazla sanal makine** alan, her alt ağ için kullanıcı başına en fazla VMs belirtin. Sanal makineleri sınırsız sayıda istiyorsanız bu alanı boş bırakın.
16. Seçin **Tamam** Laboratuvar alt dikey penceresini kapatın.
17. Seçin **kaydetmek** sanal ağ dikey penceresini kapatın.
18. Sanal ağ yapılandırıldıysa, bir VM oluşturulurken seçilebilir. 
    Bir VM oluşturma ve bir sanal ağ belirtin, makalesine başvurun görmek için [yapı içeren bir VM'yi laboratuvara ekleme](devtest-lab-add-vm-with-artifacts.md). 

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Sonraki adımlar
Laboratuvarınız için istenen sanal ağ ekledikten sonra sıradaki adım olacak [Laboratuvarınızı için bir VM eklemek](devtest-lab-add-vm-with-artifacts.md).

