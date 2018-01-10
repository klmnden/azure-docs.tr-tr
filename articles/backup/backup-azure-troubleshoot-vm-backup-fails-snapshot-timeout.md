---
title: "Azure yedekleme hatası sorunlarını giderme: Konuk Aracısı durumu kullanılamıyor | Microsoft Docs"
description: "Belirtiler, nedenleri ve çözümlemeleri ilgili Aracısı, uzantısı, diskler için Azure Backup hatalar"
services: backup
documentationcenter: 
author: genlin
manager: cshepard
editor: 
keywords: "Azure yedekleme; VM Aracısı; Ağ bağlantısı;"
ms.assetid: 4b02ffa4-c48e-45f6-8363-73d536be4639
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 01/09/2018
ms.author: genli;markgal;sogup;
ms.openlocfilehash: 5eb326dfd89d9cc64eb0e05286e64c87e090e0a1
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2018
---
# <a name="troubleshoot-azure-backup-failure-issues-with-agent-andor-extension"></a>Azure yedekleme hatası sorunlarını giderme: aracı ve/veya uzantısı ile ilgili sorunları

Bu makale, yedekleme hataları iletişim sorunlarını VM aracısı ve uzantısı ile ilgili gidermenize yardımcı olmak için sorun giderme adımlarını sağlar.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="vm-agent-unable-to-communicate-with-azure-backup"></a>VM Aracısı Azure yedekleme ile iletişim kuramıyor

> [!NOTE]
> Azure Linux VM'de Yedeklemelerinizin ve 4 Ocak 2018 sonrasında bu hata ile başarısız başlattıysanız etkilenen VM'ler içinde aşağıdaki komutu çalıştırın ve yedeklemeleri yeniden deneyin

    sudo rm -f /var/lib/waagent/*.[0-9]*.xml

Kaydolun ve Azure Backup hizmeti için bir VM zamanlama sonra yedekleme işi zaman içinde nokta anlık almak için VM Aracısı ile iletişim kurarak başlatır. Aşağıdaki koşullardan herhangi biri olan sırayla yedekleme hatasına neden olabilecek tetiklenen, gelen anlık görüntü engelleyebilir. Sorun giderme adımları verilen sırayla aşağıda izleyin ve işlemi yeniden deneyin.

##### <a name="cause-1-the-vm-has-no-internet-accessthe-vm-has-no-internet-access"></a>1. neden: [VM Internet erişim yok](#the-vm-has-no-internet-access)
##### <a name="cause-2-the-agent-is-installed-in-the-vm-but-is-unresponsive-for-windows-vmsthe-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>Neden 2: [aracı VM'yi yüklendi, ancak (Windows VM'ler için) yanıt vermiyor](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)
##### <a name="cause-3-the-agent-installed-in-the-vm-is-out-of-date-for-linux-vmsthe-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>3. neden: [VM'yi yüklü Aracı (Linux VM'ler için) güncel değil](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)
##### <a name="cause-4-the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-takenthe-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>4. neden: [anlık görüntü durumu alınamıyor olabilir veya bir anlık görüntü alınamaz](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)
##### <a name="cause-5-the-backup-extension-fails-to-update-or-loadthe-backup-extension-fails-to-update-or-load"></a>5. neden: [güncelleştirmek veya yüklemek backup uzantısı başarısız](#the-backup-extension-fails-to-update-or-load)
##### <a name="cause-6-azure-classic-vms-may-require-additional-step-to-complete-registrationazure-classic-vms-may-require-additional-step-to-complete-registration"></a>6. neden: [Azure Klasik sanal makineleri, kayıt işlemini tamamlamak için ek bir adım gerektirebilir](#azure-classic-vms-may-require-additional-step-to-complete-registration)

## <a name="snapshot-operation-failed-due-to-no-network-connectivity-on-the-virtual-machine"></a>Sanal makinedeki ağ bağlantısı nedeniyle anlık görüntü işlemi başarısız oldu
Kaydolun ve Azure Backup hizmeti için bir VM zamanlama sonra yedekleme işi zaman içinde nokta anlık görüntüyü almaya VM yedekleme uzantısı ile iletişim kurarak başlatır. Aşağıdaki koşullardan herhangi biri olan sırayla yedekleme hatasına neden olabilecek tetiklenen, gelen anlık görüntü engelleyebilir. Sorun giderme adımları verilen sırayla aşağıda izleyin ve işlemi yeniden deneyin.
##### <a name="cause-1-the-vm-has-no-internet-accessthe-vm-has-no-internet-access"></a>1. neden: [VM Internet erişim yok](#the-vm-has-no-internet-access)
##### <a name="cause-2-the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-takenthe-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>Neden 2: [anlık görüntü durumu alınamıyor olabilir veya bir anlık görüntü alınamaz](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)
##### <a name="cause-3-the-backup-extension-fails-to-update-or-loadthe-backup-extension-fails-to-update-or-load"></a>3. neden: [güncelleştirmek veya yüklemek backup uzantısı başarısız](#the-backup-extension-fails-to-update-or-load)

## <a name="vmsnapshot-extension-operation-failed"></a>VMSnapshot uzantısı işlemi başarısız oldu

Kaydolun ve Azure Backup hizmeti için bir VM zamanlama sonra yedekleme işi zaman içinde nokta anlık görüntüyü almaya VM yedekleme uzantısı ile iletişim kurarak başlatır. Aşağıdaki koşullardan herhangi biri olan sırayla yedekleme hatasına neden olabilecek tetiklenen, gelen anlık görüntü engelleyebilir. Sorun giderme adımları verilen sırayla aşağıda izleyin ve işlemi yeniden deneyin.
##### <a name="cause-1-the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-takenthe-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>1. neden: [anlık görüntü durumu alınamıyor olabilir veya bir anlık görüntü alınamaz](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)
##### <a name="cause-2-the-backup-extension-fails-to-update-or-loadthe-backup-extension-fails-to-update-or-load"></a>Neden 2: [güncelleştirmek veya yüklemek backup uzantısı başarısız](#the-backup-extension-fails-to-update-or-load)
##### <a name="cause-3-the-vm-has-no-internet-accessthe-vm-has-no-internet-access"></a>3. neden: [VM Internet erişim yok](#the-vm-has-no-internet-access)
##### <a name="cause-4-the-agent-is-installed-in-the-vm-but-is-unresponsive-for-windows-vmsthe-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>4. neden: [aracı VM'yi yüklendi, ancak (Windows VM'ler için) yanıt vermiyor](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)
##### <a name="cause-5-the-agent-installed-in-the-vm-is-out-of-date-for-linux-vmsthe-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>5. neden: [VM'yi yüklü Aracı (Linux VM'ler için) güncel değil](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)

## <a name="unable-to-perform-the-operation-as-the-vm-agent-is-not-responsive"></a>VM aracısı yanıt verebilir durumda olmadığından işlem gerçekleştirilemiyor

Kaydolun ve Azure Backup hizmeti için bir VM zamanlama sonra yedekleme işi zaman içinde nokta anlık görüntüyü almaya VM yedekleme uzantısı ile iletişim kurarak başlatır. Aşağıdaki koşullardan herhangi biri olan sırayla yedekleme hatasına neden olabilecek tetiklenen, gelen anlık görüntü engelleyebilir. Sorun giderme adımları verilen sırayla aşağıda izleyin ve işlemi yeniden deneyin.
##### <a name="cause-1-the-agent-is-installed-in-the-vm-but-is-unresponsive-for-windows-vmsthe-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>1. neden: [aracı VM'yi yüklendi, ancak (Windows VM'ler için) yanıt vermiyor](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)
##### <a name="cause-2-the-agent-installed-in-the-vm-is-out-of-date-for-linux-vmsthe-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>Neden 2: [VM'yi yüklü Aracı (Linux VM'ler için) güncel değil](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)
##### <a name="cause-3-the-vm-has-no-internet-accessthe-vm-has-no-internet-access"></a>3. neden: [VM Internet erişim yok](#the-vm-has-no-internet-access)

## <a name="backup-failed-with-an-internal-error---please-retry-the-operation-in-a-few-minutes"></a>Yedekleme bir iç hatayla başarısız oldu - Lütfen işlemi birkaç dakika içinde yeniden deneyin

Kaydolun ve Azure Backup hizmeti için bir VM zamanlama sonra yedekleme işi zaman içinde nokta anlık görüntüyü almaya VM yedekleme uzantısı ile iletişim kurarak başlatır. Aşağıdaki koşullardan herhangi biri olan sırayla yedekleme hatasına neden olabilecek tetiklenen, gelen anlık görüntü engelleyebilir. Sorun giderme adımları verilen sırayla aşağıda izleyin ve işlemi yeniden deneyin.
##### <a name="cause-1-the-vm-has-no-internet-accessthe-vm-has-no-internet-access"></a>1. neden: [VM Internet erişim yok](#the-vm-has-no-internet-access)
##### <a name="cause-2-the-agent-installed-in-the-vm-but-unresponsive-for-windows-vmsthe-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>Neden 2: [yanıt vermeyen ancak VM (Windows VM'ler için) yüklü aracı](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)
##### <a name="cause-3-the-agent-installed-in-the-vm-is-out-of-date-for-linux-vmsthe-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>3. neden: [VM'yi yüklü Aracı (Linux VM'ler için) güncel değil](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)
##### <a name="cause-4-the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-takenthe-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>4. neden: [anlık görüntü durumu alınamıyor olabilir veya bir anlık görüntü alınamaz](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)
##### <a name="cause-5-the-backup-extension-fails-to-update-or-loadthe-backup-extension-fails-to-update-or-load"></a>5. neden: [güncelleştirmek veya yüklemek backup uzantısı başarısız](#the-backup-extension-fails-to-update-or-load)
##### <a name="cause-6-backup-service-does-not-have-permission-to-delete-the-old-restore-points-due-to-resource-group-lockbackup-service-does-not-have-permission-to-delete-the-old-restore-points-due-to-resource-group-lock"></a>6. neden: [Backup hizmeti, kaynak grubu kilit nedeniyle eski geri yükleme noktaları silmek için izne sahip değil](#backup-service-does-not-have-permission-to-delete-the-old-restore-points-due-to-resource-group-lock)

## <a name="the-specified-disk-configuration-is-not-supported"></a>Belirtilen Disk yapılandırması desteklenmiyor

> [!NOTE]
> 1 TB’den düşük ve yönetilmeyen disklere sahip sanal makineler için yedeklemeyi destekleyen özel bir önizlememiz var. Ayrıntı için [büyük disk VM yedekleme desteği için özel Önizleme](https://gallery.technet.microsoft.com/Instant-recovery-point-and-25fe398a)
>
>

Azure Backup disk boyutları şu anda desteklememektedir [1023 GB'den büyük](https://docs.microsoft.com/azure/backup/backup-azure-arm-vms-prepare#limitations-when-backing-up-and-restoring-a-vm). 
- 1 TB’den büyük diskleriniz varsa, 1 TB’den düşük [yeni diskleri kullanıma açın](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal) <br>
- Ardından 1 TB’den büyük olan diskten verileri kopyalayıp yeni oluşturulan 1 TB’den küçük disklere aktarın. <br>
- Tüm verilerin kopyalandığından emin olun ve 1 TB’den büyük diskleri kaldırın
- Yedekleme başlatmak

## <a name="causes-and-solutions"></a>Nedenler ve çözümler

### <a name="the-vm-has-no-internet-access"></a>VM Internet erişim yok
Dağıtımı gereksinimi başına VM Internet erişimi yok veya erişim Azure altyapı engelleyen kısıtlamalar yerinde var.

Doğru çalışması için yedekleme uzantısını Azure genel IP adreslerine bağlantısı gerektirir. Uzantısını komutları bir Azure Storage uç (HTTP VM anlık görüntülerini yönetmek için URL) gönderir. Uzantı genel Internet erişimi varsa, yedekleme sonunda başarısız olur.

####  <a name="solution"></a>Çözüm
Sorunu çözmek için burada listelenen yöntemlerden birini deneyin.
##### <a name="allow-access-to-the-azure-datacenter-ip-ranges"></a>Azure veri merkezi IP aralıkları erişmesine izin vermek

1. Elde [Azure veri merkezi IP listesi](https://www.microsoft.com/download/details.aspx?id=41653) erişmesine izin vermek için.
2. Çalıştırarak IP'leri engellemesini **yeni NetRoute** cmdlet'i yükseltilmiş bir PowerShell penceresinde Azure VM'deki. Cmdlet'i yönetici olarak çalıştırın.
3. IP'leri erişmesine izin vermek için kurallar varsa ağ güvenlik grubuna ekleyin.

##### <a name="create-a-path-for-http-traffic-to-flow"></a>Akış HTTP trafiği için bir yol oluşturma

1. (Örneğin, bir ağ güvenlik grubu) yerinde ağ kısıtlamalarını varsa, trafiği yönlendirmek için bir HTTP proxy sunucusu dağıtın.
2. Erişim HTTP proxy sunucusundan Internet'e izin vermek için varsa kuralları ağ güvenlik grubuna ekleyin.

Bir HTTP proxy VM yedeklemeler için ayarlama hakkında bilgi edinmek için bkz: [Azure sanal makineleri yedeklemek için ortamınızı hazırlama](backup-azure-arm-vms-prepare.md#establish-network-connectivity).

Yönetilen diskleri kullanarak durumunda bir ek bağlantı noktası (8443) açarak güvenlik duvarlarında gerekebilir.

### <a name="the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>Yanıt vermeyen ancak VM (Windows VM'ler için) yüklü aracı

#### <a name="solution"></a>Çözüm
VM Aracısı bozulmuş veya hizmet durdurulmuş. VM aracısı yeniden yüklenmesi, en son sürümünü alın ve iletişim yeniden yardımcı olacaktır.

1. Doğrulama olup olmadığını Hizmetleri sanal makine (services.msc) çalıştıran Windows Konuk Aracısı hizmeti. Windows Konuk Aracısı hizmetini yeniden başlatın ve yedekleme başlatmak deneyin<br>
2. hizmetlerinde görünür durumda değilse, programlar ve özellikler Windows Konuk aracısı yüklü olup olmadığını doğrulayın.
4. Programlarda görüntüleyemez ve özellikleri Windows Konuk Aracısı'nı kaldırmak istiyorsanız.
5. İndirme ve yükleme [Aracısı MSI en son sürümünü](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Yüklemeyi tamamlamak için Yönetici ayrıcalıklarına sahip olmanız gerekir.
6. Ardından Windows Konuk Aracısı hizmetlerinin Hizmetleri'nde görmeye olmalıdır
7. "Yedekleme şimdi" portalda tıklayarak üzerinde-isteğe bağlı/geçici bir yedeklemeyi çalıştırmayı deneyin.

Ayrıca, sanal makinenin doğrulayın  **[.NET 4.5 sistemde yüklü](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed)**. Hizmet ile iletişim kurmak VM aracısı için gereklidir

### <a name="the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>VM'yi yüklü Aracı (Linux VM'ler için) güncel değil

#### <a name="solution"></a>Çözüm
Aracı veya uzantı ilgili hataları Linux VM'ler için en güncel olmayan bir VM Aracısı etkileyen sorunları neden olur. Bu sorunu gidermek için aşağıdaki genel yönergeleri izleyin:

1. Yönergeleri izleyin [Linux VM Aracısı'nı güncelleştirme](../virtual-machines/linux/update-agent.md).

 >[!NOTE]
 >Biz *tavsiye* yalnızca bir dağıtım deposu aracılığıyla aracıyı güncelleştirin. Doğrudan Github'dan aracısı kodu indiriliyor ve güncelleştirmeden önermiyoruz. Dağıtımınız için en son aracı kullanılamıyorsa, nasıl yükleneceği hakkında yönergeler için dağıtım desteğine başvurun. En son aracı için denetlemek için Git [Windows Azure Linux Aracısı](https://github.com/Azure/WALinuxAgent/releases) GitHub deposuna sayfasında.

2. Aşağıdaki komutu çalıştırarak Azure Aracısı VM üzerinde çalıştığından emin olun:`ps -e`

 İşlem çalışmıyorsa, aşağıdaki komutları kullanarak yeniden başlatın:

 * Ubuntu için:`service walinuxagent start`
 * Diğer dağıtımlar için:`service waagent start`

3. [Otomatik yeniden başlatma Aracısı'nı yapılandırma](https://github.com/Azure/WALinuxAgent/wiki/Known-Issues#mitigate_agent_crash).
4. Yeni bir test yedekleme çalıştırın. Lütfen hata devam ederse, Müşteri'nin VM'den aşağıdaki günlüklerini toplayın:

   * /var/lib/waagent/*.XML
   * /var/log/waagent.log
   * / var/oturum/azure / *

Ayrıntılı günlük kaydı için waagent gerekiyorsa, şu adımları izleyin:

1. /Etc/waagent.conf dosyasında aşağıdaki satırı bulun: **ayrıntılı günlük kaydını etkinleştir (y | n)**
2. Değişiklik **Logs.Verbose** değeri  *n*  için *y*.
3. Değişikliği kaydetmek ve bu bölümde önceki adımları izleyerek waagent yeniden başlatın.

### <a name="the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>Anlık görüntü durumu alınamıyor olabilir veya bir anlık görüntü alınamaz
VM yedekleme, bir anlık görüntü komutu alttaki depolama hesabına veren kullanır. Yedekleme, depolama hesabına erişimi olduğundan veya anlık görüntü görevi yürütme gecikmesi nedeniyle başarısız olabilir.

#### <a name="solution"></a>Çözüm
Aşağıdaki koşullar anlık görüntü görev başarısız olmasına neden olabilir:

| Nedeni | Çözüm |
| --- | --- |
| VM yapılandırılmış SQL Server Yedekleme sahiptir. | Varsayılan olarak, bir VM yedeğinin bir VSS tam yedekleme Windows sanal makinelerin çalışır. Hangi SQL Server ve SQL Server tabanlı sunucular çalıştırmakta olan VM'ler üzerinde anlık görüntü yürütme gecikmeler oluşabilir yedekleme yapılandırılır.<br><br>Bir yedekleme hata nedeniyle bir anlık görüntü sorun yaşıyorsanız, aşağıdaki kayıt defteri anahtarını ayarlayın:<br><br>**[HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT] "USEVSSCOPYBACKUP"="TRUE"** |
| RDP VM'yi kapatın olduğundan VM durumu yanlış bildirilir. | Uzak Masaüstü Protokolü (RDP), VM'yi kapatın, VM durumu doğru olup olmadığını belirlemek için portal denetleyin. Doğru değilse, Portalı'nda VM kullanarak kapatmak **kapatma** VM Panoda seçeneği. |
| Çok sayıda sanal makineleri aynı bulut hizmetinden aynı anda yedeklemek için yapılandırılır. | Yedekleme zamanlamaları VM'ler için aynı bulut hizmetinden yaymak için en iyi bir uygulamadır. |
| VM yüksek CPU veya bellek kullanımı çalışıyor. | VM yüksek CPU kullanımı (yüzde 90) veya yüksek bellek kullanımı çalışıyorsa, anlık görüntü görev sıraya alındı ve Gecikmeli ve zaman aşımına uğrar. Bu durumda, bir talep üzerine yedekleme deneyin. |
| VM konak/doku adresi DHCP'den alamaz. | DHCP çalışmaya Iaas VM yedekleme için konuk içinde etkinleştirilmesi gerekir.  VM konak/doku adresi 245 DHCP yanıttan alınamıyor, dosya indirme veya tüm uzantılar çalıştırın. Statik bir özel IP gerekiyorsa platformu ile yapılandırmanız gerekir. VM içindeki DHCP seçeneği sol etkinleştirilmiş olmalıdır. Daha fazla bilgi için bkz: [statik iç özel IP ayarı](../virtual-network/virtual-networks-reserved-private-ip.md). |

### <a name="the-backup-extension-fails-to-update-or-load"></a>Backup uzantısı güncelleştirmek veya yüklemek başarısız olur
Uzantılar yüklenemiyor çünkü bir anlık görüntü alınamaz yedekleme başarısız olur.

#### <a name="solution"></a>Çözüm

**Windows konuklar için:** iaasvmprovider hizmetinin etkin ve bir başlangıç türü doğrulayın *otomatik*. Hizmet bu şekilde yapılandırılmamışsa, sonraki yedekleme başarılı olup olmadığını belirlemek için etkinleştirin.

**Linux konuklar için:** Linux (Yedekleme tarafından kullanılan uzantılı) 1.0.91.0 için VMSnapshot en son sürümünü doğrulayın.<br>


Backup uzantısı güncelleştirmek veya yüklemek yine başarısız olursa, uzantısı kaldırarak yüklenecek VMSnapshot uzantısı zorlayabilirsiniz. Sonraki yedekleme girişiminde uzantısı yeniden yükleyecektir.

Uzantıyı kaldırmak için aşağıdakileri yapın:

1. [Azure Portal](https://portal.azure.com/) gidin.
2. Yedekleme sorunları olan VM bulun.
3. Tıklatın **ayarları**.
4. Tıklatın **uzantıları**.
5. Tıklatın **Vmsnapshot uzantısı**.
6. Tıklatın **kaldırma**.

Bu yordam sonraki yedekleme sırasında yüklenmesi uzantısı neden olur.

### <a name="azure-classic-vms-may-require-additional-step-to-complete-registration"></a>Azure Klasik Vm'leri kayıt işlemini tamamlamak için ek bir adım gerektirebilir
Yedekleme hizmeti bağlantı kurmak ve yedekleme başlatmak için Azure Klasik Vm'leri aracıyı kaydedilmelidir

#### <a name="solution"></a>Çözüm

VM Konuk aracısının yükledikten sonra Azure PowerShell'i başlatın <br>
1. Azure hesabı kullanarak oturum açma <br>
       `Login-AzureAsAccount`<br>
2. Sanal makinenin ProvisionGuestAgent özelliği True olarak aşağıdaki komutlar tarafından ayarlanmış olup olmadığını doğrulayın <br>
        `$vm = Get-AzureVM –ServiceName <cloud service name> –Name <VM name>`<br>
        `$vm.VM.ProvisionGuestAgent`<br>
3. Özelliği FALSE olarak ayarlanmışsa, TRUE olarak ayarlamak için komutları aşağıdaki izleyin<br>
        `$vm = Get-AzureVM –ServiceName <cloud service name> –Name <VM name>`<br>
        `$vm.VM.ProvisionGuestAgent = $true`<br>
4. VM güncelleştirmek için aşağıdaki komutu çalıştırın <br>
        `Update-AzureVM –Name <VM name> –VM $vm.VM –ServiceName <cloud service name>` <br>
5. Yedekleme başlatmayı deneyin. <br>

### <a name="backup-service-does-not-have-permission-to-delete-the-old-restore-points-due-to-resource-group-lock"></a>Yedekleme hizmeti kaynak grubu kilit nedeniyle eski geri yükleme noktaları silme iznine sahip değil
Bu sorun olduğu kaynak grubu kullanıcı kilitler ve yedekleme hizmeti eski geri yükleme noktaları silemedi olmadığı yönetilen sanal makineleri için özeldir. Bunun nedeni, yeni yedeklemeler arka ucundan uygulanan en fazla 18 geri yükleme noktaları sınırı olarak başarısız olmaya başlar.

#### <a name="solution"></a>Çözüm

Sorunu çözmek için lütfen geri yükleme noktası koleksiyonu kaldırmak için aşağıdaki adımları kullanın: <br>
 
1. Kaynak grubu kilitlemek VM bulunduğu Kaldır 
     
2. ARMClient Chocolatey kullanarak yükleme <br>
   https://github.com/projectkudu/ARMClient
     
3. ARMClient oturum açma <br>
             `.\armclient.exe login`
         
4. VM karşılık gelen get geri yükleme noktası koleksiyonu <br>
    `.\armclient.exe get https://management.azure.com/subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Compute/restorepointcollections/AzureBackup_<VM-Name>?api-version=2017-03-30`

    Örnek:`.\armclient.exe get https://management.azure.com/subscriptions/f2edfd5d-5496-4683-b94f-b3588c579006/resourceGroups/winvaultrg/providers/Microsoft.Compute/restorepointcollections/AzureBackup_winmanagedvm?api-version=2017-03-30`
             
5. Geri yükleme noktası koleksiyonunu silin <br>
            `.\armclient.exe delete https://management.azure.com/subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Compute/restorepointcollections/AzureBackup_<VM-Name>?api-version=2017-03-30` 
 
6. Sonraki zamanlanmış yedekleme geri yükleme noktası toplama ve yeni geri yükleme noktaları otomatik olarak oluştur 
 
7. Sorun kaynak grubu olarak yeniden kilitlemek, yalnızca daha sonra yedeklemeler başarısız olmaya başlıyor 18 geri yükleme noktası bir sınırı yoktur yeniden görünür 

