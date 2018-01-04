---
title: " Azure (Kaynak Yöneticisi) çalışan bir işlem sunucusu yönetme | Microsoft Docs"
description: "Bu makalede bir yeniden çalışma işlem sunucusu (Resource Manager) kurmak Azure'da açıklar."
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/23/2017
ms.author: anoopkv
ms.openlocfilehash: 035336efa6be0d00c41baba168eaffd80939cc82
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
---
# <a name="manage-a-process-server-running-in-azure-resource-manager"></a>Azure (Kaynak Yöneticisi) çalışan bir işlem sunucusu yönetme
> [!div class="op_single_selector"]
> * [Resource Manager](./site-recovery-vmware-setup-azure-ps-resource-manager.md)
> * [Klasik](./site-recovery-vmware-setup-azure-ps-classic.md)

Yeniden çalışma sırasında Azure sanal ağ ve şirket içi ağınız arasında yüksek gecikme ise işlem sunucusu azure'da dağıtmak için önerilir. Bu makalede nasıl ayarlamak, yapılandırabilir ve Azure'da çalışan işlem sunucuları yönetmek açıklanmaktadır.

> [!NOTE]
> Bu makalede kullandıysanız kullanılacak olan **Resource Manager** yük devretme sırasında sanal makineler için dağıtım modeli olarak. Kullandıysanız **Klasik** dağıtım modeli adımları [nasıl ayarlayın ve yeniden çalışma işlem sunucusu (Klasik) yapılandırma](./site-recovery-vmware-setup-azure-ps-classic.md)

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [site-recovery-vmware-process-server-prerequ](../../includes/site-recovery-vmware-azure-process-server-prereq.md)]

## <a name="deploy-a-process-server-on-azure"></a>Azure üzerinde bir işlem sunucusu Dağıt
1. Kasadaki > **Site Recovery altyapısı** (altında "Manage" Başlık) > **yapılandırma sunucularına** ("İçin VMware ve fiziksel makineler" başlığı altında), yapılandırma sunucusu seçin.
2. Açılan yapılandırma sunucusu Ayrıntıları sayfasında tıklayın "+ sunucu işlemleri"

  ![İşlem sunucusu Galeri Ekle](./media/site-recovery-vmware-setup-azure-ps-arm/add-ps.png)

3.  Üzerinde **işlem Sunucu Ekle** sayfasında, aşağıdaki değerleri seçin

  ![İşlem sunucusu galeri öğesi ekleme](./media/site-recovery-vmware-setup-azure-ps-arm/add-ps-page-1.png)
|**Alan adı**|**Değer**|
|-|-|
|İşlem sunucunuzu dağıtmak istediğiniz yeri seçin|Değeri seçin **azure'da yeniden çalışma işlem sunucusu Dağıt** |
|Abonelik|Sanal makineler üzerinde başarısız olduğu Azure aboneliği seçin|
|Kaynak Grubu|Bu işlem sunucusu dağıtmak veya varolan bir kaynak grubu, işlem sunucusu dağıtmak seçmek için bir kaynak grubu oluştur|
|Konum|Azure veri merkezi içine seçin sanal makineleri içine devredilir burada|
|Azure Ağı|Azure sanal Network(VNet) seçin, sanal makineleri içine yük devredildi durumunda. Sanal makineler üzerinde birden fazla Azure Vnet başarısız olduysa, sonra VNet dağıtılan bir işlem sunucusu gerekir|

4. İşlem sunucusu için özelliklerin doldurun

  ![İşlem sunucusu Özet ekleme](./media/site-recovery-vmware-setup-azure-ps-arm/add-ps-page-2.png)
|**Alan adı**|**Değer**|
|-|-|
|Sunucu Adı|Görünen ad & işlem sunucusu sanal makinesi için ana bilgisayar adı|
| User Name|Bu sanal makinede yönetici hale bir kullanıcı adı|
|Depolama Hesabı|Sanal makinenin sanal diskin yerleştirildiği depolama hesabı adı|
|Alt ağ|Azure sanal makineye bağlı sanal alt ağ|
| IP Adresi|IP adresi kez varsayın için işlem sunucusu istediğiniz önyükleme|
5. İşlem sunucusu sanal makine dağıtımı başlatmak için Tamam düğmesini tıklatın.

> [!NOTE]
> Yeniden çalışma için bu işlem sunucusu kullanabilmek şirket içi yapılandırma sunucusu ile kaydetmeniz gerekir.

## <a name="registering-the-process-server-running-in-azure-to-a-configuration-server-running-on-premises"></a>Yapılandırma (şirket içi çalıştıran) bir sunucuya (Azure'da çalışan) işlem sunucusu kaydetme

[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]

## <a name="upgrading-the-process-server-to-latest-version"></a>İşlem sunucusu son sürümüne yükseltme.

[!INCLUDE [site-recovery-vmware-upgrade-process-server](../../includes/site-recovery-vmware-upgrade-process-server.md)]

## <a name="unregistering-the-process-server-running-in-azure-from-a-configuration-server-running-on-premises"></a>(Azure'da çalışan) işlem sunucusu yapılandırması (şirket içi çalıştıran) bir sunucudan kaydı siliniyor

[!INCLUDE [site-recovery-vmware-unregister-process-server](../../includes/site-recovery-vmware-unregister-process-server.md)]
