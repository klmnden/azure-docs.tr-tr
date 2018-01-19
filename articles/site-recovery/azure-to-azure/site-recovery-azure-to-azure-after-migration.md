---
title: "Azure Site Recovery kullanarak geçiş sonrasında Azure bölgeler arasında olağanüstü durum kurtarma ayarlamak için makineler hazırlama | Microsoft Docs"
description: "Bu makalede, Azure Site Recovery kullanarak Azure geçişten sonra Azure bölgeler arasında olağanüstü durum kurtarma ayarlamak için makineler hazırlama açıklar."
services: site-recovery
documentationcenter: 
author: ponatara
manager: abhemraj
editor: 
ms.assetid: 9126f5e8-e9ed-4c31-b6b4-bf969c12c184
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: ponatara
ms.openlocfilehash: 7658bedc0bd5c4a289f3271504a006ba54c783b6
ms.sourcegitcommit: 828cd4b47fbd7d7d620fbb93a592559256f9d234
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2018
---
# <a name="replicate-azure-vms-to-another-region-after-migration-to-azure-by-using-azure-site-recovery"></a>Azure Vm'lerini başka bir bölgeye Azure geçişten sonra Azure Site Recovery kullanarak çoğaltma

>[!NOTE]
> Azure sanal makineleri (VM'ler) için Azure Site Recovery çoğaltma şu anda önizlemede değil.

## <a name="overview"></a>Genel Bakış

Bu makalede, Azure sanal makineleri bu makinelere şirket içi ortamından Azure için Azure Site Recovery kullanarak geçirildikten sonra iki Azure bölgeler arasında çoğaltma hazırlamanıza yardımcı olur.

## <a name="disaster-recovery-and-compliance"></a>Olağanüstü durum kurtarma ve uyumluluk
Bugün, daha da fazla kuruluşlar, iş yüklerini Azure'a taşıyor. Kuruluşlara kritik taşıma üretim içi iş yüklerini azure'a, uyumluluk ve bir Azure bölgesindeki kesintileri karşı koruma için bu iş yükleri için olağanüstü durum kurtarma kurma zorunludur.

## <a name="steps-for-preparing-migrated-machines-for-replication"></a>Geçirilen makineleri çoğaltma için hazırlama adımları
Hazırlamak için başka bir Azure bölgesine çoğaltma ayarlama makineler geçişi:

1. Geçişi tamamlayın.
2. Gerekirse Azure aracısını yükleyin.
3. Mobility hizmeti kaldırın.  
4. VM’yi yeniden başlatın.

Bu adımları aşağıdaki bölümlerde daha ayrıntılı olarak açıklanmıştır.

### <a name="step-1-migrate-workloads-running-on-hyper-v-vms-vmware-vms-and-physical-servers-to-run-on-azure-vms"></a>Adım 1: Hyper-V sanal makineleri, VMware sanal makinelerini ve Azure Vm'lerinde çalıştırmak için fiziksel sunucularda çalışan iş yüklerini geçirme

Çoğaltmayı ayarlama ve şirket içi geçirmek için Hyper-V, VMware ve fiziksel iş yüklerini azure'a, adımları [Azure Site Recovery ile Azure bölgeler arasında geçirmek Azure Iaas sanal makineleri](site-recovery-migrate-azure-to-azure.md) makalesi. 

Geçişten sonra yürütün veya bir yük devretme silme gerekmez. Bunun yerine, seçin **tam geçiş** seçeneği geçiş yapmak istediğiniz her makine için:
1. **Çoğaltılan Öğeler**’de VM’ye sağ tıklayıp **Geçişi Tamamla**’ya tıklayın. Tıklatın **Tamam** adımı tamamlamak için. Tam geçiş işine izleyerek VM Özellikleri'nde ilerleme durumunu izleyebilirsiniz **Site Recovery işleri**.
2. **Tam geçiş** eylem geçiş işlemi tamamlandığında, makine için çoğaltmayı kaldırır ve makine için Site Recovery Faturalaması durdurulur.

### <a name="step-2-install-the-azure-vm-agent-on-the-virtual-machine"></a>2. adım: Azure VM Aracısı sanal makineye yükleme
Azure [VM Aracısı](../../virtual-machines/windows/classic/agents-and-extensions.md#azure-vm-agents-for-windows-and-linux) çözmek ve VM korunmasına yardımcı olmak için Site Recovery uzantı sanal makineye yüklenmesi gerekir.

>[!IMPORTANT]
>Sürüm 9.7.0.0, Windows sanal makinelerde itibaren Mobility hizmeti yükleyicisinin en son kullanılabilir Azure VM Aracısı de yükler. Geçiş, sanal makine Site Recovery uzantısına dahil, herhangi bir VM uzantısı kullanılarak için önkoşul aracı yüklemesi karşılar. Azure VM Aracısı yalnızca geçirilen makinede Mobility hizmeti 9.6 veya önceki bir sürümü ise el ile yüklenmesi gerekir.

Aşağıdaki tabloda, yüklü olduğu VM Aracısı'nı yükleme ve doğrulama hakkında ek bilgi sağlar:

| **İşlem** | **Windows** | **Linux** |
| --- | --- | --- |
| VM Aracısı yükleme |[Aracı MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) dosyasını indirip yükleyin. Yüklemeyi tamamlamak için yönetici ayrıcalıkları gerekir. |En son yükleme [Linux Aracısı](../../virtual-machines/linux/agent-user-guide.md). Yüklemeyi tamamlamak için yönetici ayrıcalıkları gerekir. Aracı dağıtım depodan yüklemenizi öneririz. Biz *değil önerilir* doğrudan Github'dan Linux VM Aracısı yükleme.  |
| VM Aracısı yüklemesini doğrulama |1. Azure VM'de C:\WindowsAzure\Packages klasöre göz atın. WaAppAgent.exe dosyasını görmeniz gerekir. <br>2. Dosyaya sağ tıklayın, **Özellikler**'e gidin ve ardından **Ayrıntılar** sekmesini seçin. **Ürün sürümü** alanı 2.6.1198.718 olmalıdır ya da daha yüksek. |Yok |


### <a name="step-3-remove-the-mobility-service-from-the-migrated-virtual-machine"></a>3. adım: Mobility hizmeti geçirilen sanal makineden kaldırın.

Şirket içi VMware makineleri veya fiziksel Windows/Linux sunucularda geçirdiyseniz, el ile Kaldır/geçirilen sanal makine Mobility hizmetinden kaldırmanız gerekir.

>[!IMPORTANT]
>Hyper-V Vm'lerini Azure'a geçişi için bu adım gerekli değildir.

#### <a name="uninstall-the-mobility-service-on-a-windows-server-vm"></a>Windows Server VM Mobility hizmeti Kaldır
Windows Server bilgisayarındaki Mobility hizmeti kaldırmak için aşağıdaki yöntemlerden birini kullanın.

##### <a name="uninstall-by-using-the-windows-ui"></a>Windows kullanıcı arabirimini kullanarak kaldırma
1. Denetim Masası'nda seçin **programlar**.
2. Seçin **Microsoft Azure Site Recovery Mobility hizmeti/ana hedef sunucu**ve ardından **kaldırma**.

##### <a name="uninstall-at-a-command-prompt"></a>Bir komut isteminde kaldırma
1. Yönetici olarak bir komut istemi penceresi açın.
2. Mobility hizmetini kaldırmak için aşağıdaki komutu çalıştırın:

   ```
   MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
   ```

#### <a name="uninstall-the-mobility-service-on-a-linux-computer"></a>Bir Linux bilgisayar üzerindeki mobilite hizmetini kaldırın
1. Linux sunucunuzda olarak oturum açın bir **kök** kullanıcı.
2. Bir terminale için /user/local/ASR gidin.
3. Mobility hizmetini kaldırmak için aşağıdaki komutu çalıştırın:

   ```
   uninstall.sh -Y
   ```

### <a name="step-4-restart-the-vm"></a>4. adım: VM yeniden başlatma

Mobility hizmetinin kaldırdıktan sonra başka bir Azure bölgesine çoğaltma ayarlama önce VM'yi yeniden başlatın.


## <a name="next-steps"></a>Sonraki adımlar
- İş yükleri tarafından korumaya başlamak [Azure sanal makineleri çoğaltmak](azure-to-azure-quickstart.md).
- Daha fazla bilgi edinmek [Azure sanal makineleri çoğaltmak için kılavuz ağ](site-recovery-azure-to-azure-networking-guidance.md).
