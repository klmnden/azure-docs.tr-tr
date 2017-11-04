---
title: "Azure yığın Geliştirme Seti dağıtımının önkoşulları | Microsoft Docs"
description: "Azure yığın Geliştirme Seti (bulut işleci) için ortamı ve donanım gereksinimlerini görüntüleyin."
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: byronr
editor: 
ms.assetid: 32a21d9b-ee42-417d-8e54-98a7f90f7311
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/11/2017
ms.author: erikje
ms.openlocfilehash: 73e7efb7d789fe12846d68066c0927bb123831a2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-stack-deployment-prerequisites"></a>Azure Stack dağıtım önkoşulları

*Uygulandığı öğe: Azure yığın Geliştirme Seti*

Dağıtmadan önce [Azure yığın Geliştirme Seti](azure-stack-poc.md), bilgisayarınızın aşağıdaki gereksinimleri karşıladığından emin olun:


## <a name="hardware"></a>Donanım
| Bileşen | Minimum | Önerilen |
| --- | --- | --- |
| Disk sürücüleri: İşletim Sistemi |Sistem bölümü için en az 200 GB'lık kullanılabilir alana sahip 1 işletim sistemi diski (SSD veya HDD) |Sistem bölümü için en az 200 GB'lık kullanılabilir alana sahip 1 işletim sistemi diski (SSD veya HDD) |
| Disk sürücüleri: Genel Geliştirme Seti veri * |4 disk. Her disk, en az 140 GB'lık kapasiteye (SSD veya HDD) sahiptir. Mevcut tüm diskler kullanılır. |4 disk. Her disk, en az 250 GB'lık kapasiteye (SSD veya HDD) sahiptir. Mevcut tüm diskler kullanılır. |
| İşlem: CPU |Çift Yuvalı: 12 Fiziksel Çekirdek (toplam) |Çift Yuvalı: 16 Fiziksel Çekirdek (toplam) |
| İşlem: Bellek |96 GB RAM |128 GB RAM (PaaS kaynak sağlayıcıları desteklemek için en düşük gereksinimdir.)|
| İşlem: BIOS |Hyper-V Etkin (SLAT desteğiyle) |Hyper-V Etkin (SLAT desteğiyle) |
| Ağ: NIC |NIC için Windows Server 2012 R2 Sertifikası gerekir; özelleştirilmiş herhangi bir özellik gerekmez |NIC için Windows Server 2012 R2 Sertifikası gerekir; özelleştirilmiş herhangi bir özellik gerekmez |
| Donanım logosu sertifikası |[Windows Server 2012 R2 için sertifikalıdır](http://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0) |[Windows Server 2012 R2 için sertifikalıdır](http://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0) |

\*Birçok eklemeyi düşünüyorsanız, bu kapasite önerilen fazlasını gerekir [Market öğesi](azure-stack-download-azure-marketplace-item.md) azure'dan.

**Veri disk sürücüsü yapılandırması:** Tüm veri sürücüleri aynı türde (tümü SAS veya tümü SATA) ve kapasitede olmalıdır. SAS disk sürücüleri kullanılıyorsa disk sürücülerinin (MPIO kullanılmaz, çok yollu destek sağlanır) tek bir yolla bağlanması gerekir.

**HBA yapılandırma seçenekleri**

* (Tercih edilen) Basit HBA
* RAID HBA - Bağdaştırıcının "geçiş" modunda yapılandırılmış olması gerekir
* RAID HBA - Disklerin Tek Disk ve RAID-0 olarak yapılandırılmış olması gerekir

**Desteklenen veri yolu ve ortam birleşimleri yazın**

* SATA HDD
* SAS HDD
* RAID HDD
* RAID SSD (Medya türü belirtilmemişse/bilinmiyorsa\*)
* SATA SSD + SATA HDD
* SAS SSD + SAS HDD

\*RAID denetleyiciler geçiş yetenek olmadan medya türü tanıyamıyor. Bu tür denetleyiciler, hem HDD'yi hem de SSD'yi Belirtilmemiş olarak işaretler. Bu durumda, kalıcı depolama alanı olarak önbelleğe alma cihazları yerine SSD kullanılır. Bu nedenle, bu SSD Geliştirme Seti dağıtabilirsiniz.

**Örnek HBA'lar**: geçiş modunda LSI 9207-8i, LSI-9300-8i veya LSI-9265-8i

Örnek OEM yapılandırmaları kullanılabilir.

## <a name="operating-system"></a>İşletim sistemi
|  | **Gereksinimleri** |
| --- | --- |
| **İşletim sistemi sürümü** |Windows Server 2012 R2 veya sonraki bir sürümü. Dağıtıma başlamadan önce işletim sistemi sürümü Azure yığın yüklemede VHD içine ana bilgisayar önyükleme gibi kritik değildir. İşletim sistemi ve tüm gerekli düzeltme eklerini zaten görüntüsüne tümleşiktir. Tüm anahtarları development Kit'te kullanılan herhangi bir Windows Server örneklerini etkinleştirmek için kullanmayın. |

## <a name="deployment-requirements-check-tool"></a>Aracı dağıtım gereksinimlerini denetleyin
İşletim sisteminin yükledikten sonra kullanabilirsiniz [Azure yığını için dağıtım denetleyicisi](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) donanımınız tüm gereksinimleri karşıladığını onaylamak için.

## <a name="account-requirements"></a>Hesap gereksinimleri
Genellikle, burada, Microsoft Azure bağlanabilirsiniz internet bağlantısıyla Geliştirme Seti dağıtın. Bu durumda, Geliştirme Seti dağıtmak için bir Azure Active Directory (Azure AD) hesabı yapılandırmanız gerekir.

Ortamınız internet'e bağlı değil veya Azure AD kullanmak istemiyorsanız, Active Directory Federasyon Hizmetleri (AD FS) kullanarak Azure yığın dağıtabilirsiniz. Geliştirme Seti kendi AD FS ve Active Directory etki alanı Hizmetleri örnekleri içerir. Bu seçeneği kullanarak dağıtırsanız, önceden hesapları ayarlamanız gerekmez.

>[!NOTE]
AD FS seçeneğini kullanarak dağıtırsanız, Azure AD ile geçiş yapmak için Azure yığını yeniden dağıtmanız gerekir.

### <a name="azure-active-directory-accounts"></a>Azure Active Directory hesapları
Bir Azure AD hesabı kullanarak Azure yığın dağıtmak için dağıtım PowerShell komut dosyasını çalıştırmadan önce bir Azure AD hesabının hazırlamanız gerekir. Bu hesap, Azure AD Kiracı için genel yönetici olur. Sağlamak ve uygulamalar ve hizmet asıl adı Azure Active Directory ve grafik API'si etkileşime tüm Azure yığın hizmetlerini temsilci seçmek için kullanılır. (Bu, daha sonra değiştirebilirsiniz) varsayılan sağlayıcı aboneliğin sahibi da kullanılır. Azure yığın sisteminizin Yönetici portalına bu hesabı kullanarak oturum.

1. İçin en az bir Azure AD directory yönetici olan bir Azure AD hesabı oluşturun. Zaten bir hesabınız varsa bu hesabı kullanabilirsiniz. Aksi takdirde, bir ücretsiz oluşturabilirsiniz [http://azure.microsoft.com/en-us/pricing/free-trial/](http://azure.microsoft.com/pricing/free-trial/) (Çin'de ziyaret <http://go.microsoft.com/fwlink/?LinkID=717821> yerine). İçin daha sonra planlıyorsanız [Azure yığın Azure ile kaydedin](azure-stack-register.md), ayrıca bir abonelik bu hesap yeni oluşturulmuş olması gerekir.
   
    6. adımda bu kimlik bilgilerini kullanmak için kaydetme [Geliştirme Seti dağıtmak](azure-stack-run-powershell-script.md#deploy-the-development-kit). Bu *hizmet yöneticisi* hesabı; kaynak bulutlarını, kullanıcı hesaplarını, kiracı planlarını, kotaları ve fiyatlandırmayı yapılandırıp yönetebilir. Portalda web sitesi bulutları, sanal makine özel bulutları ve planlar oluşturup kullanıcı aboneliklerini yönetebilir.
2. [Oluşturma](azure-stack-add-new-user-aad.md) en az bir hesabı böylece Kiracı Geliştirme Seti için oturum açabilirsiniz.
   
   | **Azure Active Directory hesabı** | **Destekleniyor mu?** |
   | --- | --- |
   | Geçerli ortak Azure aboneliği ile iş veya Okul hesabı |Evet |
   | Geçerli Genel Azure Aboneliğine sahip Microsoft Hesabı |Evet |
   | Geçerli Çin Azure aboneliği ile iş veya Okul hesabı |Evet |
   | Geçerli US Government Azure aboneliği ile iş veya Okul hesabı |Evet |

## <a name="network"></a>Ağ
### <a name="switch"></a>Anahtar
Kullanılabilir bir bağlantı noktasına Geliştirme Seti makine için bir anahtar.  

Geliştirme Seti makine anahtarı erişim bağlantı noktası veya santral bağlantı noktasına bağlanmayı destekler. Anahtar üzerinde özelleştirilmiş herhangi bir özellik gerekmez. Santral bağlantı noktası kullanıyorsanız veya VLAN kimliği yapılandırmanız gerekiyorsa dağıtım parametresi olarak VLAN kimliğini sağlamanız gerekir. Örneklere bakın [dağıtım parametrelerin listesi](azure-stack-run-powershell-script.md).

### <a name="subnet"></a>Alt ağ
Geliştirme Seti makine aşağıdaki alt ağlara bağlanma:

* 192.168.200.0/24
* 192.168.100.0/27
* 192.168.101.0/26
* 192.168.102.0/24
* 192.168.103.0/25
* 192.168.104.0/25

Bu alt Geliştirme Seti ortamında iç ağlar için ayrılmıştır.

### <a name="ipv4ipv6"></a>IPv4/IPv6
Yalnızca IPv4 desteklenir. IPv6 ağı oluşturamazsınız.

### <a name="dhcp"></a>DHCP
Ağ üzerinde NIC'nin bağlanabileceği kullanılabilir bir DHCP sunucusunun olduğundan emin olun. Kullanılabilir bir DHCP sunucusu yoksa ana bilgisayar tarafından kullanılanın dışında ek bir statik IPv4 ağı hazırlamanız gerekir. Dağıtım parametresi olarak bu IP adresini ve ağ geçidini sağlamanız gerekir. Örneklere bakın [dağıtım parametrelerin listesi](azure-stack-run-powershell-script.md).

### <a name="internet-access"></a>İnternet erişimi
Azure yığını, doğrudan ya da saydam bir proxy üzerinden Internet erişimi gerektirir. Azure yığın Internet erişimi etkinleştirmek için bir web proxy yapılandırmasını desteklemez. Hem ana bilgisayar IP hem de (DHCP veya statik IP) için MAS BGPNAT01 atanan yeni IP Internet erişimine sahip olmalıdır. 80 ve 443 numaralı bağlantı noktalarını altındaki graph.windows.net ve login.microsoftonline.com etki alanları kullanılır.

## <a name="telemetry"></a>Telemetri

Telemetri Azure yığın gelecek sürümlerinde şekil yardımcı olur. Bize geri bildirim için hızlı yanıt, yeni özellikleri sağlar ve kalitesini artırmak sağlar. Microsoft Azure yığın Windows Server 2016 ve SQL Server 2014'ü içerir. Bu ürünler hiçbiri varsayılan ayarlardan değiştirilir ve her ikisi tarafından Microsoft Enterprise gizlilik bildirimi açıklanmıştır. Azure yığını telemetri Microsoft'a göndermeyi değiştirilmemiş açık kaynak yazılımının de içerir. Azure yığın telemetri verilerini bazı örnekleri şunlardır:

- Dağıtım kayıt bilgileri
- bir uyarı zaman açık ve kapalı
- ağ kaynaklarının sayısı

Telemetri veri akışı desteklemek için bağlantı noktası 443 (HTTPS), ağınızdaki açık olması gerekir. İstemci uç noktası https://vortex-win.data.microsoft.com'dur.

Azure yığını için telemetri sağlamak istemiyorsanız, Geliştirme Seti konağı ve aşağıda açıklandığı gibi altyapı sanal makineler üzerinde kapatabilirsiniz.

### <a name="turn-off-telemetry-on-the-development-kit-host-optional"></a>Geliştirme Seti konak telemetriyi (isteğe bağlı) Kapat

>[!NOTE]
Telemetriyi Geliştirme Seti konağını etkinleştirmek istiyorsanız, bunu, dağıtım komut dosyası çalıştırılmadan önce yapmalısınız.

Önce [asdk installer.ps1 komut dosyası çalıştırarak]() Geliştirme Seti konak dağıtmak için önyükleme CloudBuilder.vhdx ve yükseltilmiş bir PowerShell penceresinde aşağıdaki betiği çalıştırın:
```powershell
### Get current AllowTelmetry value on DVM Host
(Get-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\DataCollection" `
-Name AllowTelemetry).AllowTelemetry
### Set & Get updated AllowTelemetry value for ASDK-Host 
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\DataCollection" `
-Name "AllowTelemetry" -Value '0'  
(Get-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\DataCollection" `
-Name AllowTelemetry).AllowTelemetry
```

Ayarı **AllowTelemetry** telemetri 0 kapatır hem Windows hem de Azure yığın dağıtılmak için. Yalnızca kritik güvenlik olayları işletim sisteminden gönderilir. Ayar Windows telemetri tüm tüm konaklar ve altyapı VM'ler denetler ve ölçek genişletme işlemleri oluştuğunda yeni düğümler/VM'ler için yeniden.


### <a name="turn-off-telemetry-on-the-infrastructure-virtual-machines-optional"></a>Telemetri altyapı sanal makinelerde (isteğe bağlı) Kapat

Dağıtım başarılı olduktan sonra aşağıdaki komut dosyasını (olarak AzureStack\AzureStackAdmin kullanıcı) yükseltilmiş bir PowerShell penceresinde Geliştirme Seti konakta çalıştırın:

```powershell
$AzSVMs= get-vm |  where {$_.Name -like "AzS-*"}
### Show current AllowTelemetry value for all AzS-VMs
invoke-command -computername $AzSVMs.name {(Get-ItemProperty -Path `
"HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\DataCollection" -Name AllowTelemetry).AllowTelemetry}
### Set & Get updated AllowTelemetry value for all AzS-VMs
invoke-command -computername $AzSVMs.name {Set-ItemProperty -Path `
"HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\DataCollection" -Name "AllowTelemetry" -Value '0'}
invoke-command -computername $AzSVMs.name {(Get-ItemProperty -Path `
"HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\DataCollection" -Name AllowTelemetry).AllowTelemetry}
```

SQL Server telemetri yapılandırmak için bkz: [SQL Server 2016 yapılandırma](https://support.microsoft.com/en-us/help/3153756/how-to-configure-sql-server-2016-to-send-feedback-to-microsoft).

### <a name="usage-reporting"></a>Kullanım raporlaması

Kayıt Azure yığın Azure iletme kullanım bilgilerini de yapılandırılır. Kullanım raporlama telemetrisinden bağımsız olarak denetlenir. Ne zaman raporlama kullanım bırakabilir [kaydetme](azure-stack-register.md) Github'da komut dosyası kullanarak. Ayarlamanız yeterlidir **$reportUsage** parametresi **$false**.

Kullanım verileri biçimlendirilmiş içinde ayrıntılı olarak [rapor Azure yığın kullanım verilerini Azure](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-usage-reporting). Azure yığın Geliştirme Seti kullanıcılar gerçekte ücretlendirilen değil. Bu işlevsellik, böylece kullanım raporlama nasıl çalıştığını görmek için test Geliştirme Seti dahil edilir. 


## <a name="next-steps"></a>Sonraki adımlar
[Azure yığın Geliştirme Seti dağıtım paketini karşıdan yükle](https://azure.microsoft.com/overview/azure-stack/try/?v=try)

[Azure yığın Geliştirme Seti dağıtma](azure-stack-run-powershell-script.md)

