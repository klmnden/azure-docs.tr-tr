---
title: 'Azure yedekleme hatası sorunlarını giderme: Konuk Aracısı durumu kullanılamıyor'
description: Belirtiler, nedenler ve çözümler Aracısı, uzantı ve diskleri ilgili Azure Backup hatalar.
services: backup
author: genlin
manager: cshepard
keywords: Azure yedekleme; VM Aracısı; Ağ bağlantısı;
ms.service: backup
ms.topic: troubleshooting
ms.date: 01/09/2018
ms.author: genli
ms.openlocfilehash: 63cded007af499455e7bb4fc23d26d56caf96678
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34606367"
---
# <a name="troubleshoot-azure-backup-failure-issues-with-the-agent-or-extension"></a>Azure yedekleme hatası sorunlarını giderme: aracı veya uzantısı ile ilgili sorunları

Bu makale, yardımcı olabilecek sorun giderme adımlarını uzantısı ve VM Aracısı ile iletişim için ilgili Azure Backup hataları gidermek sağlar.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="vm-agent-unable-to-communicate-with-azure-backup"></a>VM Aracısı Azure yedekleme ile iletişim kuramıyor

Hata iletisi: "VM Aracısı Azure yedekleme ile iletişim kuramıyor"<br>
Hata kodu: "UserErrorGuestAgentStatusUnavailable"

Kaydolun ve yedekleme hizmeti için bir VM zamanlama sonra yedekleme işi zaman içinde nokta anlık almak için VM Aracısı ile iletişim kurarak başlatır. Aşağıdaki koşullardan herhangi biri, anlık görüntü tetiklenen gelen engelleyebilir. Bir anlık görüntü değil tetiklendiğinde yedekleme başarısız olabilir. Aşağıdaki sorun giderme adımları listelendikleri sırada tamamlayın ve işlemi yeniden deneyin:

**1. neden: [VM Internet erişimi yok](#the-vm-has-no-internet-access)**  
**Neden 2: [aracı VM, ancak, yanıt vermeyen (Windows VM'ler için) yüklendi](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**    
**3. neden: [VM'yi yüklü Aracı (Linux VM'ler için) güncel değil](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**4. neden: [anlık görüntü durumu alınamıyor olabilir ya da bir anlık görüntü alınamaz](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**    
**5. neden: [güncelleştirmek veya yüklemek backup uzantısı başarısız](#the-backup-extension-fails-to-update-or-load)**  

## <a name="snapshot-operation-failed-due-to-no-network-connectivity-on-the-virtual-machine"></a>Sanal makine ağa bağlı değil anlık görüntü işlemi başarısız olur

Hata iletisi: "anlık görüntü işlemi sanal makinedeki ağ bağlantısı nedeniyle başarısız oldu"<br>
Hata kodu: "ExtensionSnapshotFailedNoNetwork"

Kaydolun ve Azure Backup hizmeti için bir VM zamanlama sonra yedekleme işi zaman içinde nokta anlık görüntüyü almaya VM yedekleme uzantısı ile iletişim kurarak başlatır. Aşağıdaki koşullardan herhangi biri, anlık görüntü tetiklenen gelen engelleyebilir. Anlık görüntü tetiklenen değil, bir yedekleme hatası ortaya çıkabilir. Aşağıdaki sorun giderme adımları listelendikleri sırada tamamlayın ve işlemi yeniden deneyin:    
**1. neden: [VM Internet erişimi yok](#the-vm-has-no-internet-access)**  
**Neden 2: [anlık görüntü durumu alınamıyor olabilir ya da bir anlık görüntü alınamaz](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  
**3. neden: [güncelleştirmek veya yüklemek backup uzantısı başarısız](#the-backup-extension-fails-to-update-or-load)**  

## <a name="vmsnapshot-extension-operation-failed"></a>VMSnapshot uzantısı işlemi başarısız

Hata iletisi: "VMSnapshot uzantısı işlemi başarısız oldu"<br>
Hata kodu: "ExtentionOperationFailed"

Kaydolun ve Azure Backup hizmeti için bir VM zamanlama sonra yedekleme işi zaman içinde nokta anlık görüntüyü almaya VM yedekleme uzantısı ile iletişim kurarak başlatır. Aşağıdaki koşullardan herhangi biri, anlık görüntü tetiklenen gelen engelleyebilir. Anlık görüntü tetiklenen değil, bir yedekleme hatası ortaya çıkabilir. Aşağıdaki sorun giderme adımları listelendikleri sırada tamamlayın ve işlemi yeniden deneyin:  
**1. neden: [anlık görüntü durumu alınamıyor olabilir ya da bir anlık görüntü alınamaz](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  
**Neden 2: [güncelleştirmek veya yüklemek backup uzantısı başarısız](#the-backup-extension-fails-to-update-or-load)**  
**3. neden: [aracı VM, ancak, yanıt vermeyen (Windows VM'ler için) yüklendi](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  
**4. neden: [VM'yi yüklü Aracı (Linux VM'ler için) güncel değil](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**

## <a name="backup-fails-because-the-vm-agent-is-unresponsive"></a>VM aracısı yanıt vermiyor yedekleme başarısız olur

Hata iletisi: "anlık görüntü durum için VM Aracısı ile iletişim kuramadı" <br>
Hata kodu: "GuestAgentSnapshotTaskStatusError"

Kaydolun ve Azure Backup hizmeti için bir VM zamanlama sonra yedekleme işi zaman içinde nokta anlık görüntüyü almaya VM yedekleme uzantısı ile iletişim kurarak başlatır. Aşağıdaki koşullardan herhangi biri, anlık görüntü tetiklenen gelen engelleyebilir. Anlık görüntü tetiklenen değil, bir yedekleme hatası ortaya çıkabilir. Aşağıdaki sorun giderme adımları listelendikleri sırada tamamlayın ve işlemi yeniden deneyin:  
**1. neden: [aracı VM, ancak, yanıt vermeyen (Windows VM'ler için) yüklendi](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  
**Neden 2: [VM'yi yüklü Aracı (Linux VM'ler için) güncel değil](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**3. neden: [VM Internet erişimi yok](#the-vm-has-no-internet-access)**  

## <a name="backup-fails-with-an-internal-error"></a>Bir iç hata ile yedekleme başarısız oluyor

Hata iletisi: "Yedekleme bir iç hatayla başarısız oldu - Lütfen işlemi birkaç dakika içinde yeniden deneyin" <br>
Hata kodu: "BackUpOperationFailed" / "BackUpOperationFailedV2"

Kaydolun ve Azure Backup hizmeti için bir VM zamanlama sonra yedekleme işi zaman içinde nokta anlık görüntüyü almaya VM yedekleme uzantısı ile iletişim kurarak başlatır. Aşağıdaki koşullardan herhangi biri, anlık görüntü tetiklenen gelen engelleyebilir. Anlık görüntü tetiklenen değil, bir yedekleme hatası ortaya çıkabilir. Aşağıdaki sorun giderme adımları listelendikleri sırada tamamlayın ve işlemi yeniden deneyin:  
**1. neden: [VM Internet erişimi yok](#the-vm-has-no-internet-access)**  
**Neden 2: [aracısının VM, ancak bunu yüklü (Windows VM'ler için) yanıt vermiyor](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  
**3. neden: [VM'yi yüklü Aracı (Linux VM'ler için) güncel değil](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**4. neden: [anlık görüntü durumu alınamıyor olabilir ya da bir anlık görüntü alınamaz](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  
**5. neden: [güncelleştirmek veya yüklemek backup uzantısı başarısız](#the-backup-extension-fails-to-update-or-load)**  
**6. neden: [Backup hizmeti, bir kaynak grubu kilit nedeniyle eski geri yükleme noktaları silmek için izne sahip değil](#backup-service-does-not-have-permission-to-delete-the-old-restore-points-due-to-resource-group-lock)**

## <a name="causes-and-solutions"></a>Nedenler ve çözümler

### <a name="the-vm-has-no-internet-access"></a>VM internet erişimi yok
Dağıtımı gereksinimi VM Internet erişimi yok. Veya, Azure altyapı erişimi engelleyen kısıtlamalar olabilir.

Doğru çalışması için Backup uzantısının Azure genel IP adreslerine bağlantısı gerektirir. Uzantısını komutları VM anlık görüntülerini yönetmek için bir Azure depolama uç nokta için (HTTP URL) gönderir. Uzantı genel internet erişimi yoksa, yedekleme sonunda başarısız olur.

Bu VM trafiği yönlendirmek için bir proxy sunucusu dağıtmayı mümkün.
##### <a name="create-a-path-for-http-traffic"></a>HTTP trafiği için bir yol oluşturma

1. (Örneğin, bir ağ güvenlik grubu) yerinde ağ kısıtlamalarını varsa, trafiği yönlendirmek için bir HTTP proxy sunucusu dağıtın.
2. Erişim HTTP proxy sunucusundan internet'e izin vermek için varsa kuralları ağ güvenlik grubuna ekleyin.

Bir HTTP proxy VM yedeklemeler için ayarlama hakkında bilgi edinmek için bkz: [Azure sanal makineleri yedeklemek için ortamınızı hazırlama](backup-azure-arm-vms-prepare.md#establish-network-connectivity).

Yedeklenen VM veya proxy sunucu üzerinden trafik yönlendirilir Azure genel IP adreslerine erişim izni gerektirir

####  <a name="solution"></a>Çözüm
Sorunu çözmek için aşağıdaki yöntemlerden birini deneyin:

##### <a name="allow-access-to-azure-storage-that-corresponds-to-the-region"></a>Bölgesine karşılık gelen Azure depolama erişmesine izin vermek

Kullanabileceğiniz [hizmet etiketleri](../virtual-network/security-overview.md#service-tags) belirli bölgenin depolama bağlantılara izin vermek için. Depolama hesabına erişim izni veren Kuralı Internet erişimi engeller kural daha yüksek önceliğe sahip olduğundan emin olun. 

![Bir bölge için depolama etiketlerle ağ güvenlik grubu](./media/backup-azure-arm-vms-prepare/storage-tags-with-nsg.png)

Servis etiketlerini yapılandırmak için adım adım yordam anlamak için izleme [bu videoyu](https://youtu.be/1EjLQtbKm1M).

> [!WARNING]
> Depolama hizmet etiketleri önizlemede. Bunlar yalnızca belirli bölgelerde kullanılabilir. Bölgelerin bir listesi için bkz: [hizmet depolama için etiketler](../virtual-network/security-overview.md#service-tags).

Azure yönetilen diskleri kullanıyorsanız, güvenlik duvarları hakkında ek bağlantı noktası açma (bağlantı noktası 8443) gerekebilir.

### <a name="the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>VM Aracısı yüklenir, ancak (Windows VM'ler için) yanıt vermiyor

#### <a name="solution"></a>Çözüm
VM Aracısı bozulmuş veya hizmet durdurulmuş. VM Aracısı'nı yeniden yüklemek için en son sürümü edinmek yardımcı olur. Ayrıca, hizmeti ile iletişimi yeniden yardımcı olur.

1. VM Hizmetleri (services.msc) Windows Konuk aracısı hizmetinin çalışıp çalışmadığını belirleyin. Windows Konuk Aracısı hizmetini yeniden başlatın ve yedekleme başlatmak deneyin.    
2. Windows Konuk Aracısı hizmeti Hizmetleri'nde, Denetim Masası ' nda görünür durumda değilse Git **programlar ve Özellikler** Windows Konuk aracısı yüklü olup olmadığını belirleme.
4. Windows Konuk Aracısı görünüyorsa **programlar ve Özellikler**, Windows Konuk Aracısı'nı kaldırın.
5. İndirme ve yükleme [MSI Aracısı en son sürümünü](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Yüklemeyi tamamlamak için yönetici haklarına sahip olmalıdır.
6. Windows Konuk Aracısı hizmetlerinin Hizmetleri'nde göründüğünden emin olun.
7. Bir talep üzerine yedekleme çalıştırın: 
    * Portalı'nda seçin **Şimdi Yedekle**.

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
4. Yeni bir test yedekleme çalıştırın. Hata devam ederse aşağıdaki günlüklerini sanal makineden toplayın:

   * /var/lib/waagent/*.XML
   * /var/log/waagent.log
   * / var/oturum/azure / *

Ayrıntılı günlük kaydı için waagent gerekiyorsa, şu adımları izleyin:

1. /Etc/waagent.conf dosyasında aşağıdaki satırı bulun: **ayrıntılı günlük kaydını etkinleştir (y | n)**
2. Değişiklik **Logs.Verbose** değeri *n* için *y*.
3. Değişikliği kaydetmek ve bu bölümde daha önce açıklanan adımları izleyerek waagent yeniden başlatın.

###  <a name="the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>Anlık görüntü durumu alınamıyor olabilir ya da bir anlık görüntü alınamaz
VM yedekleme, bir anlık görüntü komutu alttaki depolama hesabına veren kullanır. Yedekleme, depolama hesabına erişimi olduğundan veya anlık görüntü görevi yürütme gecikmesi nedeniyle başarısız olabilir.

#### <a name="solution"></a>Çözüm
Aşağıdaki koşullar ve anlık görüntü görevi başarısız olmasına neden olabilir:

| Nedeni | Çözüm |
| --- | --- |
| Uzak Masaüstü Protokolü (RDP) VM'yi kapatın olduğundan VM durumu yanlış bildirilir. | RDP VM'yi kapatın, VM durumu doğru olup olmadığını belirlemek için portal denetleyin. Doğru değilse, Portalı'nda VM kullanarak kapatmak **kapatma** VM Panoda seçeneği. |
| VM konak veya doku adresi DHCP'den alınamıyor. | DHCP çalışmaya Iaas VM yedekleme için konuk içinde etkinleştirilmesi gerekir. VM konak veya doku adresi 245 DHCP yanıttan alınamıyor, dosya indirme veya tüm uzantılar çalıştırın. Statik bir özel IP gerekiyorsa platformu ile yapılandırın. VM içindeki DHCP seçeneği sol etkinleştirilmiş olmalıdır. Daha fazla bilgi için bkz: [statik iç özel IP ayarlamak](../virtual-network/virtual-networks-reserved-private-ip.md). |

### <a name="the-backup-extension-fails-to-update-or-load"></a>Backup uzantısı güncelleştirmek veya yüklemek başarısız olur
Uzantılar yüklenemiyor çünkü bir anlık görüntü alınamaz yedekleme başarısız olur.

#### <a name="solution"></a>Çözüm

Yeniden yüklemek için VMSnapshot uzantısı zorlamak için uzantıyı kaldırın. Sonraki yedekleme girişiminde uzantısı yeniden yükler.

Uzantıyı kaldırmak için:

1. İçinde [Azure portal](https://portal.azure.com/), yedekleme hatasının oluştuğu VM gidin.
2. Seçin **ayarları**.
3. Seçin **uzantıları**.
4. Seçin **Vmsnapshot uzantısı**.
5. Seçin **kaldırma**.

Linux VMSnapshot uzantısını Azure Portal'da görünmüyorsa VM için [Azure Linux Aracısı güncelleştirme](../virtual-machines/linux/update-agent.md), ve ardından yedekleme çalıştırın. 

Bu adımları tamamladıktan sonraki yedekleme sırasında yüklenmesi uzantısı neden olur.

### <a name="backup-service-does-not-have-permission-to-delete-the-old-restore-points-due-to-resource-group-lock"></a>Yedekleme hizmeti bir kaynak grubu kilit nedeniyle eski geri yükleme noktaları silmek için izne sahip değil
Bu sorun, kullanıcının kaynak grubu kilitler yönetilen sanal makineleri için özeldir. Bu durumda, yedekleme hizmeti eski geri yükleme noktaları silemiyor. 18 geri yükleme noktası sınırı olduğundan, yeni yedeklemeler başarısız olmaya başlıyor.

#### <a name="solution"></a>Çözüm

Sorunu çözmek için kaynak grubundan kilidi kaldırmak ve geri yükleme noktası koleksiyonu kaldırmak için aşağıdaki adımları tamamlayın: 
 
1. Kilidi VM bulunduğu kaynak grubunu kaldırın. 
2. ARMClient Chocolatey kullanarak yükleyin: <br>
   https://github.com/projectkudu/ARMClient
3. ARMClient için oturum açın: <br>
    `.\armclient.exe login`
4. VM karşılık gelen geri yükleme noktası koleksiyonunun alın: <br>
    `.\armclient.exe get https://management.azure.com/subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Compute/restorepointcollections/AzureBackup_<VM-Name>?api-version=2017-03-30`

    Örnek: `.\armclient.exe get https://management.azure.com/subscriptions/f2edfd5d-5496-4683-b94f-b3588c579006/resourceGroups/winvaultrg/providers/Microsoft.Compute/restorepointcollections/AzureBackup_winmanagedvm?api-version=2017-03-30`
5. Geri yükleme noktası koleksiyonunu silin: <br>
    `.\armclient.exe delete https://management.azure.com/subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Compute/restorepointcollections/AzureBackup_<VM-Name>?api-version=2017-03-30` 
6. Sonraki zamanlanmış yedekleme, geri yükleme noktası koleksiyonu ve yeni geri yükleme noktaları otomatik olarak oluşturur.

Bunu yaptıktan sonra yeniden geri kilidi VM kaynak grubunda koyabilirsiniz. 
