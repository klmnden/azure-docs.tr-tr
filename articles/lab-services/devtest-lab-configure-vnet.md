---
title: Azure DevTest Labs'de sanal ağ yapılandırma | Microsoft Docs
description: Mevcut bir sanal ağ ve alt ağ yapılandırın ve bunları Azure DevTest Labs ile bir sanal makinede kullanmak hakkında bilgi edinin
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 6cda99c2-b87e-4047-90a0-5df10d8e9e14
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/05/2018
ms.author: spelluru
ms.openlocfilehash: 8fb3b4ac748fcae2e3aad5b3bfb2a893340dc61a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60694857"
---
# <a name="configure-a-virtual-network-in-azure-devtest-labs"></a>Azure DevTest Labs'de sanal ağ yapılandırma
Makalesinde açıklandığı gibi [bir VM'yi bir laboratuvara ekleme](devtest-lab-add-vm.md), laboratuvarda, bir VM oluşturduğunuzda, yapılandırılmış bir sanal ağ da belirtebilirsiniz. Örneğin, ExpressRoute veya siteden siteye VPN ile yapılandırılan sanal ağ'ı kullanarak sanal makinelerinize kaynaklarınızı corpnet erişimi gerekebilir.

Bu makalede, böylece VM'ler oluşturulurken seçmek kullanılabilir var olan sanal ağınızda bir laboratuvar sanal ağ ayarlarını ekleme açıklanmaktadır.

## <a name="configure-a-virtual-network-for-a-lab-using-the-azure-portal"></a>Sanal ağ için Azure portalını kullanarak bir laboratuvar yapılandırma
Aşağıdaki adımlar aynı laboratuvarda VM oluşturduğunuz sırada kullanılabilir böylece var olan sanal ağı (ve alt ağ), laboratuvara eklerken size kılavuzluk. 

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
1. Seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden.
1. İstenen Laboratuvar labs listesinden seçin. 
1. Laboratuvar ana bölmeden **yapılandırması ve ilkelerini**.

    ![Laboratuvar yapılandırması ve ilkelerini erişim](./media/devtest-lab-configure-vnet/policies-menu.png)
1. İçinde **dış KAYNAKLARA** bölümünden **sanal ağları**. Geçerli Laboratuvar için yapılandırılan sanal ağların bir listesini, laboratuvarınız için oluşturulan varsayılan sanal ağ yanı sıra görüntülenir. 
1. **+ Ekle** öğesini seçin.
   
    ![Laboratuvarınız için mevcut bir sanal ağı Ekle](./media/devtest-lab-configure-vnet/lab-settings-vnet-add.png)
1. Üzerinde **sanal ağ** bölmesinde **[sanal ağ seçin]**.
   
    ![Mevcut bir sanal ağ seçin](./media/devtest-lab-configure-vnet/lab-settings-vnets-vnet1.png)
1. Üzerinde **sanal ağ Seç** bölmesinde istediğiniz sanal ağı seçin. Tüm Laboratuvar abonelikte aynı bölgede altında olan sanal ağların gösteren bir liste görüntülenir.
1. Bir sanal ağ'ı seçtikten sonra döndürülürsünüz **sanal ağ** bölmesi. Listenin altındaki alt ağ seçin.

    ![Alt ağ listesi](./media/devtest-lab-configure-vnet/lab-settings-vnets-vnet2.png)
    
    Laboratuvar alt bölmesi görüntülenir.

    ![Laboratuvar alt bölmesi](./media/devtest-lab-configure-vnet/lab-subnet.png)
     
   - Belirtin bir **Laboratuvar alt ağ adı**.
   - Laboratuvarda VM oluşturma kullanılacak bir alt ağ izin vermek için seçin **sanal makine oluşturma işlemi kullanımda**.
   - Etkinleştirmek için bir [genel IP adresi paylaşılan](devtest-lab-shared-ip.md)seçin **etkinleştir paylaşılan genel IP**.
   - Genel IP adresleri bir alt ağında izin vermek için seçin **genel IP oluşturmaya izin ver**.
   - İçinde **kullanıcı başına en fazla sanal makine** alan, her alt ağ için kullanıcı başına en fazla VM belirtin. Sınırsız sayıda sanal makineleri istiyorsanız bu alanı boş bırakın.
1. Seçin **Tamam** Laboratuvar alt bölmesini kapatın.
1. Seçin **Kaydet** sanal ağ bölmesini kapatın.

Yapılandırılmış bir sanal ağ, VM oluşturduğunuz sırada seçilebilir. VM oluşturma ve bir sanal ağ belirtin, makaleye bakın yapılacağını görmek için [bir VM'yi bir laboratuvara ekleme](devtest-lab-add-vm.md). 

Azure'nın [sanal ağ belgeleri](https://docs.microsoft.com/azure/virtual-network) sanal ağlar, ayarlamak ve VNet yönetmek ve şirket içi ağınıza bağlanmak nasıl dahil olmak üzere kullanma hakkında daha fazla bilgi sağlar.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Sonraki adımlar
Laboratuvarınız için istenen sanal ağ ekledikten sonra sonraki adım olarak [bir VM, laboratuvara ekleme](devtest-lab-add-vm.md).

