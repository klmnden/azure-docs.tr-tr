---
title: VM ayarlarını Azure Backup hizmeti ile bir Azure VM'yi yedekleme
description: Azure Backup hizmeti ile bir Azure VM'yi yedekleme konusunda bilgi edinin
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 02/17/2019
ms.author: raynew
ms.openlocfilehash: 2fe786d90612feff312983dbd25dc6d691be6e70
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60318842"
---
# <a name="back-up-an-azure-vm-from-the-vm-settings"></a>Sanal makine ayarlarla bir Azure VM'yi yedekleme

Bu makalede, Azure sanal makinelerini nasıl yedekleyeceğiniz açıklanmaktadır [Azure Backup](backup-overview.md) hizmeti. Azure Vm'lerini birkaç yöntem kullanarak yedekleyebilirsiniz:

- Azure VM tek: Bu makaledeki yönergeleri VM ayarlarını doğrudan bir Azure VM'yi yedekleme konusunda açıklanmaktadır.
- Birden fazla Azure sanal makineler: Bir kurtarma Hizmetleri kasası kurar ve birden çok Azure VM için yedeklemeyi yapılandırın. Bölümündeki yönergeleri [bu makalede](backup-azure-arm-vms-prepare.md) bu senaryo için.

 

## <a name="before-you-start"></a>Başlamadan önce

1. [Bilgi](backup-architecture.md#how-does-azure-backup-work) yedekleme nasıl çalıştığını ve [doğrulayın](backup-support-matrix.md#azure-vm-backup-support) destek gereksinimleri. 
2. [Genel bakışın](backup-azure-vms-introduction.md) Azure VM yedekleme.

### <a name="azure-vm-agent-installation"></a>Azure VM Aracısı yüklemesi

Azure Vm'lerini yedekleme için bir uzantı makine üzerinde çalışan VM Aracısı Azure Backup yükler. Sanal makinenize bir Azure Market görüntüsünden oluşturulduysa, aracıyı çalıştırırsınız. Bazı durumlarda, örneğin özel bir VM oluşturmak veya şirket içinden bir makineyi geçirme. aracıyı el ile yüklemeniz gerekebilir. 

- VM Aracısı'nı el ile yüklemeniz gerekiyorsa, yönergelerini izleyin [Windows](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows) veya [Linux](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) VM'ler. 
- Yedeklemeyi etkinleştirdiğinizde aracıyı yükledikten sonra Azure Backup Aracısı ile yedekleme uzantısını yükler. Güncelleştirmeleri ve yamaları uzantısı kullanıcı müdahalesi olmadan.

## <a name="back-up-from-azure-vm-settings"></a>Azure VM ayarlarını yedekleme


1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Tıklayın **tüm hizmetleri** filtrede yazın **sanal makineler**ve ardından **sanal makineler**. 
3. VM listesinden yedeklemek istediğiniz VM'yi seçin.
4. VM menüsünde **yedekleme**. 
5. İçinde **kurtarma Hizmetleri kasası**, aşağıdakileri yapın:
   - Bir kasa zaten varsa, tıklayın **var olanı Seç**ve bir kasa seçin.
   - Bir kasa yoksa tıklayın **Yeni Oluştur**. Kasa için bir ad belirtin. VM aynı bölge ve kaynak grubunda oluşturulur. VM ayarlarını doğrudan yedekten etkinleştirdiğinizde, bu ayarları değiştiremezsiniz.

   ![Yedekleme Sihirbazını Etkinleştirme](./media/backup-azure-vms-first-look-arm/vm-menu-enable-backup-small.png)

6. İçinde **yedekleme ilkesi seçmek**, aşağıdakileri yapın:

   - Varsayılan ilkesini bırakın. Bu VM'yi belirtilen zaman günde bir kez yedekler ve kasadaki yedekler 30 gün boyunca tutar.
   - Varsa mevcut bir yedekleme İlkesi'ni seçin.
   - Yeni bir ilke oluşturun ve ilke ayarlarını tanımlayın.  

   ![Yedekleme ilkesini seçme](./media/backup-azure-vms-first-look-arm/set-backup-policy.png)

7. Tıklayın **etkinleştirme yedekleme**. Bu yedekleme İlkesi VM ile ilişkilendirir. 

    ![Yedeklemeyi Etkinleştir düğmesi](./media/backup-azure-vms-first-look-arm/vm-management-menu-enable-backup-button.png)

8. Portal bildirimleri yapılandırma ilerleme durumunu izleyebilirsiniz.
9. VM menüsünde, iş tamamlandıktan sonra tıklayın **yedekleme**. Sayfada yedekleme durumu sanal makine, Kurtarma noktaları, çalışan işleri ve verilen uyarılar hakkında daha fazla bilgi için gösterilir.

   ![Yedekleme durumu](./media/backup-azure-vms-first-look-arm/backup-item-view-update.png)

10. Backup'ı etkinleştirdikten sonra ilk yedekleme çalışır. İlk yedeklemeyi hemen başlatmak veya yedekleme zamanlamasına uygun olarak başlatana kadar bekleyin.
    - İlk yedekleme işlemi tamamlanana kadar **son yedekleme durumu** olarak gösterir **uyarı (ilk yedekleme Beklemede)**.
    - Sonraki zamanlanmış yedekleme işinin ne zaman görmek için yedekleme ilkesi adı'nı tıklatın.
    
   

> [!NOTE]
> Azure Backup, adlandırma biçimi ile geri yükleme noktalarının depolanacağı ayrı bir kaynak grubu (dışında VM kaynak grubu) oluşturur **AzureBackupRG_geography_number** (örnek: AzureBackupRG_northeurope_1). Bu kaynak grubu kilit olmamalıdır.



## <a name="run-a-backup-immediately"></a>Hemen bir yedekleme Çalıştır 

1. Yedekleme VM menüsünde, hemen çalıştırmak için tıklayın **yedekleme** > **Şimdi Yedekle**.

    ![Yedekleme Çalıştır](./media/backup-azure-vms-first-look-arm/backup-now-update.png)

2. İçinde **Şimdi Yedekle** kadar kurtarma noktası korunur, seçmek için takvim denetimini kullanın > ve **Tamam**.
  
    ![Yedekleme bekletme gün](./media/backup-azure-vms-first-look-arm/backup-now-blade-calendar.png)

3. Portal bildirimleri, yedekleme işinin tetiklendiğini bilmenizi sağlar. Yedekleme ilerleme durumunu izlemek için tıklayın **işlerin tümünü görüntülemek**.




## <a name="back-up-from-the-recovery-services-vault"></a>Kurtarma Hizmetleri kasasından yedekleme

Bir Azure yedekleme kurtarma Hizmetleri kasası kurar ayarlayıp yedekleme Kasası'nda etkinleştirme için Azure sanal makinelerini yedeklemeyi etkinleştirmek için bu makaledeki yönergeleri izleyin.

## <a name="next-steps"></a>Sonraki adımlar

- Bu makaledeki yordamlardan herhangi birini içeren sorunlar varsa başvurun [sorun giderme kılavuzu](backup-azure-vms-troubleshoot.md).
- [Hakkında bilgi edinin](backup-azure-manage-vms.md) yedeklemelerinizi yönetme.

