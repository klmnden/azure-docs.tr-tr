---
title: 'Azure Site kurtarma aracısı hatası sorunlarını giderme: Konuk Aracısı durumu kullanılamıyor | Microsoft Docs'
description: Belirtiler, nedenleri ve çözümlemeleri aracısı ve uzantı ilgili Azure Site Recovery hatalar
services: site-recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.date: 05/02/2018
ms.author: asgang
ms.openlocfilehash: 9bfe181b2271f4e8af6f43e1728167712dade8ee
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="troubleshoot-azure-site-recovery-extension-failures-issues-with-the-agent-or-extension"></a>Azure Site Recovery uzantı hatalarını giderme: aracı veya uzantısı ile ilgili sorunları

Bu makale, yardımcı olabilecek sorun giderme adımlarını VM aracısı ve uzantı ilgili Azure Site Recovery hataları çözümleyin sağlar.


## <a name="azure-site-recovery-extension-time-out"></a>Azure Site Recovery uzantısı zaman aşımı  

Hata iletisi: "Görev Yürütme zaman aşımına uğradı izleme uzantısı işlemin başlatılması sırasında"<br>
Hata kodu: "151076"

 Azure Site Recovery koruma etkinleştirme işini bir parçası olarak sanal makinede uzantı yükleyin. Aşağıdaki koşullardan herhangi biri tetiklenen gelen koruma ve başarısız işi engelleyebilir. Aşağıdaki sorun giderme adımları tamamlayın ve ardından işlemi yeniden deneyin:

**1. neden: [aracı VM, ancak, yanıt vermeyen (Windows VM'ler için) yüklendi](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**    
**Neden 2: [VM'yi yüklü Aracı (Linux VM'ler için) güncel değil](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**3. neden: [güncelleştirmek veya yüklemek Site Recovery uzantısı başarısız](#the-site-recovery-extension-fails-to-update-or-load)**  

Hata iletisi: "Önceki site kurtarma uzantısı işlemi beklenenden daha uzun sürüyor."<br>
Hata kodu: "150066"<br>

**1. neden: [aracı VM, ancak, yanıt vermeyen (Windows VM'ler için) yüklendi](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**    
**Neden 2: [VM'yi yüklü Aracı (Linux VM'ler için) güncel değil](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**3. neden: [Site Recovery uzantı durumu geçersiz](#the-site-recovery-extension-fails-to-update-or-load)**  

## <a name="protection-fails-because-the-vm-agent-is-unresponsive"></a>VM aracısı yanıt vermiyor koruma başarısız olur

Hata iletisi: "Görev Yürütme zaman aşımına uğradı izleme uzantısı işlemin başlatılması sırasında."<br>
Hata kodu: "151099"<br>

Sanal makinede Azure Konuk Aracısı hazır durumda değilse, bu hata oluşabilir.
Azure Konuk aracı durumunu kontrol edebilirsiniz [Azure portal](https://portal.azure.com/). Sanal makine korumak ve durumunu denetlemek için çalıştığınız gidin "VM > Ayarlar > Özellikler > aracı durumu". Çoğu zaman aracının durumunu duruma hazır sanal makine yeniden başlatmanın ardından. Ancak, yeniden başlatma olası bir seçenek değil veya sorun hala bakacak, ardından aşağıdaki sorun giderme adımlarını tamamlayın.

**1. neden: [aracı VM, ancak, yanıt vermeyen (Windows VM'ler için) yüklendi](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**    
**Neden 2: [VM'yi yüklü Aracı (Linux VM'ler için) güncel değil](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  


Hata iletisi: "Görev Yürütme zaman aşımına uğradı izleme uzantısı işlemin başlatılması sırasında."<br>
Hata kodu: "151095"<br>

Bu aracı sürümü Linux makinesinde eski olduğunda oluşur. Aşağıdaki sorun giderme adımı tamamlayın.<br>
  **1. neden: [VM'yi yüklü Aracı (Linux VM'ler için) güncel değil](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
## <a name="causes-and-solutions"></a>Nedenler ve çözümler

### <a name="the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>VM Aracısı yüklenir, ancak (Windows VM'ler için) yanıt vermiyor

#### <a name="solution"></a>Çözüm
VM Aracısı bozulmuş veya hizmet durdurulmuş. VM Aracısı'nı yeniden yüklemek için en son sürümü edinmek yardımcı olur. Ayrıca, hizmeti ile iletişimi yeniden yardımcı olur.

1. Belirlemek olup olmadığını "Windows Azure Konuk Aracısı hizmeti" VM Hizmetleri (services.msc) çalışıyor. Yeniden başlatmayı deneyin "Windows Azure Konuk Aracısı hizmeti".    
2. Windows Azure Konuk Aracısı hizmeti Hizmetleri'nde, Denetim Masası ' nda görünür durumda değilse Git **programlar ve Özellikler** Windows Konuk aracısı yüklü olup olmadığını belirleme.
4. Windows Azure Konuk Aracısı görünüyorsa **programlar ve Özellikler**, Windows Konuk Aracısı'nı kaldırın.
5. İndirme ve yükleme [MSI Aracısı en son sürümünü](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Yüklemeyi tamamlamak için yönetici haklarına sahip olmalıdır.
6. Windows Azure Konuk Aracısı hizmetleri Hizmetleri'nde göründüğünden emin olun.
7. Koruma işi yeniden başlatın.

Ayrıca, doğrulayın [Microsoft .NET 4.5 yüklü](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) VM'deki. Hizmet ile iletişim kurmak VM aracısı için .NET 4.5 gereklidir.

### <a name="the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>VM'yi yüklü Aracı (Linux VM'ler için) güncel değil

#### <a name="solution"></a>Çözüm
Aracı veya uzantı ilgili hataları Linux VM'ler için en güncel olmayan bir VM Aracısı etkileyen sorunları neden olur. Bu sorunu gidermek için aşağıdaki genel yönergeleri izleyin:

1. Yönergeleri izleyin [Linux VM Aracısı'nı güncelleştirme](../virtual-machines/linux/update-agent.md).

 > [!NOTE]
 > Biz *tavsiye* yalnızca bir dağıtım deposu aracılığıyla aracıyı güncelleştirin. Doğrudan Github'dan aracısı kodu indiriliyor ve güncelleştirmeden önermiyoruz. Dağıtımınız için en son Aracı nasıl yükleneceği hakkında yönergeler için kullanılabilir, ilgili kişi dağıtım desteği değilse. En son aracı için denetlemek için Git [Windows Azure Linux Aracısı](https://github.com/Azure/WALinuxAgent/releases) GitHub deposuna sayfasında.

2. Aşağıdaki komutu çalıştırarak Azure Aracısı VM üzerinde çalıştığından emin olun: `ps -e`

 İşlem çalışmıyorsa, aşağıdaki komutları kullanarak yeniden başlatın:

 * Ubuntu için: `service walinuxagent start`
 * Diğer dağıtımlar için: `service waagent start`

3. [Otomatik yeniden başlatma Aracısı'nı yapılandırma](https://github.com/Azure/WALinuxAgent/wiki/Known-Issues#mitigate_agent_crash).
4. Sanal makinenin korumasını etkinleştirin.



### <a name="the-site-recovery-extension-fails-to-update-or-load"></a>Güncelleştirme veya yüklemek Site Recovery uzantısı başarısız
Uzantıları durumu ise "boş ', 'NotReady' ya da makalesindeki geçme.

#### <a name="solution"></a>Çözüm

Uzantıyı kaldırın ve işlemi yeniden başlatın.

Uzantıyı kaldırmak için:

1. İçinde [Azure portal](https://portal.azure.com/), yedekleme hatasının oluştuğu VM gidin.
2. Seçin **ayarları**.
3. Seçin **uzantıları**.
4. Seçin **Site kurtarma uzantısı**.
5. Seçin **kaldırma**.

Linux VMSnapshot uzantısını Azure Portal'da görünmüyorsa VM için [Azure Linux Aracısı güncelleştirme](../virtual-machines/linux/update-agent.md), ve ardından koruma çalıştırın. 

Bu adımları tamamladıktan koruma sırasında yüklenmesi uzantısı neden olur.


