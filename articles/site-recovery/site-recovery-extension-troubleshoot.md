---
title: Azure Site Recovery aracıları ile ilgili sorunları giderme | Microsoft Docs
description: Belirtiler, nedenleri ve çözümleri, Azure Site Recovery Aracısı hataları hakkında bilgi sağlar.
author: asgang
manager: rochakm
ms.service: site-recovery
ms.topic: troubleshooting
ms.date: 11/27/2018
ms.author: asgang
ms.openlocfilehash: 5ea701682c03370cea46f9126ecf78427a776371
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61280680"
---
# <a name="troubleshoot-issues-with-the-azure-site-recovery-agent"></a>Azure Site Recovery Aracısı ile ilgili sorunları giderme

Bu makale, Azure Site Recovery hataları VM aracısı ve uzantı ile ilgili yardımcı olacak sorun giderme adımlarını çözmek sağlar.


## <a name="azure-site-recovery-extension-time-out"></a>Azure Site Recovery uzantısı zaman aşımı  

Hata iletisi: "Görev Yürütme uzantı işleminin başlatılması izlenirken zaman aşımına uğradı"<br>
Hata kodu: "151076"

 Azure Site Recovery, sanal makinede korumayı etkinleştirme işini bir parçası olarak bir uzantı yükleyin. Aşağıdaki koşullardan herhangi biri işinin başarısız olmasına ve tetiklenmekte karşı koruma engelleyebilir. Aşağıdaki sorun giderme adımlarını tamamlayın ve sonra işlemi yeniden deneyin:

**1. neden: [VM Aracısı yüklendi, ancak (Windows VM'ler için) yanıt vermiyor](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**    
**2. neden: [Sanal Makineye yüklenen Aracı (Linux VM'ler için) güncel değil](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**3. neden: [Site Recovery uzantısı yükleme veya güncelleştirme başarısız](#the-site-recovery-extension-fails-to-update-or-load)**  

Hata iletisi: "Önceki site recovery uzantısı işlemi beklenenden uzun sürüyor."<br>
Hata kodu: "150066"<br>

**1. neden: [VM Aracısı yüklendi, ancak (Windows VM'ler için) yanıt vermiyor](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**    
**2. neden: [Sanal Makineye yüklenen Aracı (Linux VM'ler için) güncel değil](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**3. neden: [Site Recovery uzantı durumu yanlış](#the-site-recovery-extension-fails-to-update-or-load)**  

## <a name="protection-fails-because-the-vm-agent-is-unresponsive"></a>VM aracısı yanıt vermediği için koruma başarısız olur.

Hata iletisi: "Görev Yürütme uzantı işleminin başlatılması izlenirken zaman aşımına uğradı."<br>
Hata kodu: "151099"<br>

Azure Konuk Aracısı sanal makinede hazır durumda değilse, bu hata oluşabilir.
Azure Konuk aracı durumunu kontrol edebilirsiniz [Azure portalında](https://portal.azure.com/). Koruma ve durum iade aktarmaya çalıştığınız sanal makineye gidin "VM > Ayarlar > Özellikler > aracı durumu". Çoğu zaman aracının durumunu haline hazır sanal makine yeniden başlatıldıktan sonra. Ancak, yeniden başlatma, olası bir seçenek değil veya sorunu hala karşılaştığınız, ardından aşağıdaki sorun giderme adımlarını tamamlayın.

**1. neden: [VM Aracısı yüklendi, ancak (Windows VM'ler için) yanıt vermiyor](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**    
**2. neden: [Sanal Makineye yüklenen Aracı (Linux VM'ler için) güncel değil](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  


Hata iletisi: "Görev Yürütme uzantı işleminin başlatılması izlenirken zaman aşımına uğradı."<br>
Hata kodu: "151095"<br>

Bu Linux makinesine aracı sürümü eski olduğunda oluşur. Lütfen aşağıdaki sorun giderme adımı tamamlayın.<br>
  **1. neden: [Sanal Makineye yüklenen Aracı (Linux VM'ler için) güncel değil](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
## <a name="causes-and-solutions"></a>Nedenler ve çözümler

### <a name="the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>VM Aracısı yüklendi, ancak (Windows VM'ler için) yanıt vermiyor

#### <a name="solution"></a>Çözüm
VM Aracısı bozulduysa veya hizmet durdurulmuş. VM aracısını yeniden yüklemeyi en son sürümü Al yardımcı olur. Ayrıca, hizmeti ile iletişimi yeniden yardımcı olur.

1. Belirlemek olup olmadığını "Windows Azure Konuk Aracısı hizmeti" VM Hizmetleri (services.msc) çalışıyor. Yeniden başlatmayı deneyin "Windows Azure Konuk aracısı hizmetinin".    
2. Windows Azure Konuk aracı hizmetini Hizmetleri'nde, Denetim Masası ' nda görünür değilse Git **programlar ve Özellikler** Windows Konuk aracısı yüklü olup olmadığını belirlemek için.
4. Windows Azure Konuk Aracısı görünürse **programlar ve Özellikler**, Windows Konuk Aracısı'nı kaldırın.
5. İndirme ve yükleme [Aracısı MSI en son sürümünü](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Yüklemeyi tamamlamak için yönetici hakları olmalıdır.
6. Windows Azure Konuk aracı hizmetleri Hizmetleri'nde göründüğünden emin olun.
7. Koruma işi yeniden başlatın.

Ayrıca, doğrulayın [Microsoft .NET 4.5 yüklü](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) VM. .NET 4.5 hizmetiyle iletişim kurmak VM aracısı gereklidir.

### <a name="the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>Sanal Makineye yüklenen Aracı (Linux VM'ler için) güncel değil

#### <a name="solution"></a>Çözüm
Aracı veya uzantı ilgili hataları Linux Vm'leri için en güncel olmayan bir VM Aracısı etkileyen sorunları nedeniyle oluşur. Bu sorunu gidermek için bu yönergeleri izleyin:

1. Yönergelerini izleyin [Linux VM Aracısı güncelleştirilirken](../virtual-machines/linux/update-agent.md).

   > [!NOTE]
   > Biz *önemle tavsiye* yalnızca bir dağıtım deposu aracılığıyla aracıyı güncelleştirin. Aracı kodu doğrudan Github'dan indiriliyor ve güncelleştirirken önermiyoruz. Dağıtımınız için en son aracıyı yüklemek yönergeler mevcut, ilgili kişi dağıtım desteği değilse. En son aracı için denetlenecek Git [Windows Azure Linux Aracısı](https://github.com/Azure/WALinuxAgent/releases) GitHub deposunda sayfası.

2. Aşağıdaki komutu çalıştırarak Azure Aracısı VM üzerinde çalıştığından emin olun: `ps -e`

   İşlem çalışıyor durumda değilse, aşağıdaki komutları kullanarak yeniden başlatın:

   * Ubuntu için: `service walinuxagent start`
   * Diğer dağıtımlar için: `service waagent start`

3. [Otomatik yeniden başlatma aracı yapılandırma](https://github.com/Azure/WALinuxAgent/wiki/Known-Issues#mitigate_agent_crash).
4. Sanal makine korumasını etkinleştirin.



### <a name="the-site-recovery-extension-fails-to-update-or-load"></a>Site Recovery uzantısı yükleme veya güncelleştirme başarısız
Uzantı durumu ise "boş ', 'NotReady' veya makalesindeki.

#### <a name="solution"></a>Çözüm

Uzantıyı kaldırın ve işlemi yeniden başlatın.

Uzantıyı kaldırmak için:

1. İçinde [Azure portalında](https://portal.azure.com/)yedekleme hatası yaşayan VM'yi gidin.
2. Seçin **ayarları**.
3. **Uzantılar**'ı seçin.
4. Seçin **Site kurtarma uzantısı**.
5. Seçin **kaldırma**.

Linux VM, VMSnapshot uzantısı Azure Portalı'nda görünmüyorsa için [Azure Linux aracısını güncelleştirme](../virtual-machines/linux/update-agent.md), ve ardından korumayı çalıştırın. 

Bu adımları sırasında koruma yüklenmesi uzantısı neden olur.


