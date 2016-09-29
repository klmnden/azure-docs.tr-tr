<properties
    pageTitle="Azure Stack POC'yi dağıtmadan önce| Microsoft Azure"
    description="Azure Stack POC için ortam ve donanım gereksinimlerini görüntüleyin (hizmet yöneticisi)."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="06/29/2016"
    ms.author="erikje"/>


# Azure Stack dağıtım önkoşulları

Azure Stack POC'yi ([Kavram Kanıtı](azure-stack-poc.md)) dağıtmadan önce, bilgisayarınızın aşağıdaki gereksinimleri karşıladığından emin olun.
Bu gereksinimler, yalnızca Azure Stack POC için geçerlidir ve gelecekteki sürümlerde değişebilir.

Ayrıca dağıtıma ilişkin şu öğretici videosunu izlemeniz de faydalı olabilir:

[AZURE.VIDEO microsoft-azure-stack-tp1-poc-deployment-tutorial]

## Donanım

| Bileşen | Minimum  | Önerilen |
|---|---|---|
| Disk sürücüleri: İşletim Sistemi | Sistem bölümü için en az 200 GB'lık kullanılabilir alana sahip 1 işletim sistemi diski (SSD veya HDD) | Sistem bölümü için en az 200 GB'lık kullanılabilir alana sahip 1 işletim sistemi diski (SSD veya HDD) |
| Disk sürücüleri: Genel Azure Stack POC Verileri | 4 disk. Her disk, en az 140 GB'lık kapasiteye (SSD veya HDD) sahiptir. Mevcut tüm diskler kullanılır. | 4 disk. Her disk, en az 250 GB'lık kapasiteye (SSD veya HDD) sahiptir. Mevcut tüm diskler kullanılır.|
| İşlem: CPU | Çift Yuvalı: 12 Fiziksel Çekirdek (toplam)  | Çift Yuvalı: 16 Fiziksel Çekirdek (toplam) |
| İşlem: Bellek | 96 GB RAM  | 128 GB RAM |
| İşlem: BIOS | Hyper-V Etkin (SLAT desteğiyle)  | Hyper-V Etkin (SLAT desteğiyle) |
| Ağ: NIC | NIC için Windows Server 2012 R2 Sertifikası gerekir; özelleştirilmiş herhangi bir özellik gerekmez | NIC için Windows Server 2012 R2 Sertifikası gerekir; özelleştirilmiş herhangi bir özellik gerekmez |
| Donanım logosu sertifikası | [Windows Server 2012 R2 için sertifikalı](http://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0) |[Windows Server 2012 R2 için sertifikalı](http://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0)|

Gereksinimlerinizi doğrulamak için [Deployment Checker for Azure Stack Technical Preview 1](https://gallery.technet.microsoft.com/Deployment-Checker-for-76d824e1) paketini kullanabilirsiniz.

**Veri disk sürücüsü yapılandırması:** Tüm veri sürücüleri aynı türde (tümü SAS veya tümü SATA) ve kapasitede olmalıdır. SAS disk sürücüleri kullanılıyorsa disk sürücülerinin (MPIO kullanılmaz, çok yollu destek sağlanır) tek bir yolla bağlanması gerekir.

**HBA yapılandırma seçenekleri**
 
- (Tercih edilen) Basit HBA
- RAID HBA - Bağdaştırıcının "geçiş" modunda yapılandırılmış olması gerekir
- RAID HBA - Disklerin Tek Disk ve RAID-0 olarak yapılandırılmış olması gerekir

**Desteklenen veri yolu ve medya türü bileşimleri**

-   SATA HDD

-   SAS HDD

-   RAID HDD

-   RAID SSD (Medya türü belirtilmemişse/bilinmiyorsa\*)

-   SATA SSD + SATA HDD

-   SAS SSD + SAS HDD

\* Geçiş özelliği olmayan RAID denetleyicileri, medya türünü tanıyamaz. Bu tür denetleyiciler, hem HDD'yi hem de SSD'yi Belirtilmemiş olarak işaretler. Bu durumda, kalıcı depolama alanı olarak önbelleğe alma cihazları yerine SSD kullanılır. Bu nedenle Microsoft Azure Stack POC'yi bu SDD'ler üzerinde dağıtabilirsiniz.

**Örnek HBA'lar**: geçiş modunda LSI 9207-8i, LSI-9300-8i veya LSI-9265-8i

Örnek OEM yapılandırmaları kullanılabilir.




## İşletim sistemi

| | **Gereksinimler**  |
|---|---|
| **İşletim Sistemi Sürümü** | Son önemli güncelleştirmelerin yüklü olduğu, Windows Server 2016 Datacenter Edition **Technical Preview 4**. WindowsServer2016Datacenter.vhdx, indirme paketine dahildir. Bu VHDX'e önyükleme yapabilir ve ardından VHDX'i Azure Stack POC dağıtımı için temel işletim sistemi olarak kullanabilirsiniz.|
| **Yükleme Yöntemi** | Temiz yükleme. İşletim sistemini Azure Stack POC makinenize hızlı şekilde yüklemek için dağıtım paketinde sağlanan WindowsServer2016Datacenter.vhdx'i kullanabilirsiniz. |
| **Etki alanına katılmış mı?** | Hayır. |


## Microsoft Azure Active Directory hesapları

1. En az bir Azure Active Directory'nin dizin yöneticisi olan bir Azure AD hesabı oluşturun. Zaten bir hesabınız varsa bu hesabı kullanabilirsiniz. Aksi halde, şu adrese giderek ücretsiz bir hesap oluşturabilirsiniz: [http://azure.microsoft.com/tr-TR/pricing/free-trial/](http://azure.microsoft.com/pricing/free-trial/) (Çin'de, <http://go.microsoft.com/fwlink/?LinkID=717821> adresini ziyaret edin.)

    Bu kimlik bilgilerini, [PowerShell dağıtım betiğini çalıştırma](azure-stack-run-powershell-script.md#run-the-powershell-deployment-script) adlı makalenin 6. adımında kullanmak üzere kaydedin. Bu *hizmet yöneticisi* hesabı; kaynak bulutlarını, kullanıcı hesaplarını, kiracı planlarını, kotaları ve fiyatlandırmayı yapılandırıp yönetebilir. Portalda web sitesi bulutları, sanal makine özel bulutları ve planlar oluşturup kullanıcı aboneliklerini yönetebilir.

2. Azure Stack POC'de kiracı olarak oturum açabilmeniz için en az bir hesap [oluşturun](azure-stack-add-new-user-aad.md).

  	| **Azure Active Directory hesabı**  | **Destekleniyor mu?** |
  	|---|---| 
  	| Geçerli Genel Azure Aboneliğine sahip Kuruluş Kimliği  | Yes |
  	| Geçerli Genel Azure Aboneliğine sahip Microsoft Hesabı  | Yes |
  	| Geçerli Çin Azure Aboneliğine sahip Kuruluş Kimliği  | Yes |
  	| Geçerli ABD Azure Aboneliğine sahip Kuruluş Kimliği  | Hayır |

>[AZURE.NOTE] Azure Stack POC, yalnızca Azure Active Directory kimlik doğrulamasını destekler.


## Ağ

### Anahtar

POC makinesi için anahtar üzerinde mevcut bir bağlantı noktası.  

Azure Stack POC makinesi, anahtar erişimi bağlantı noktasına veya santral bağlantı noktasına yönelik erişimi destekler. Anahtar üzerinde özelleştirilmiş herhangi bir özellik gerekmez. Santral bağlantı noktası kullanıyorsanız veya VLAN kimliği yapılandırmanız gerekiyorsa dağıtım parametresi olarak VLAN kimliğini sağlamanız gerekir. Örneğin:

    DeployAzureStack.ps1 –Verbose –PublicVLan 305

Bu parametre belirtildiğinde yalnızca ana bilgisayar ve NATVM için VLAN kimliği ayarlanır.

### Alt ağ

POC makinesini 192.168.200.0/24, 192.168.100.0/24 veya 192.168.133.0/24 alt ağına bağlamayın. Bu ağlar, Microsoft Azure Stack POC ortamındaki iç ağlar için ayrılmıştır.

### IPv4/IPv6

Yalnızca IPv4 desteklenir. IPv6 ağı oluşturamazsınız.

### DHCP

Ağ üzerinde NIC'nin bağlanabileceği kullanılabilir bir DHCP sunucusunun olduğundan emin olun. Kullanılabilir bir DHCP sunucusu yoksa ana bilgisayar tarafından kullanılanın dışında ek bir statik IPv4 ağı hazırlamanız gerekir. Dağıtım parametresi olarak bu IP adresini ve ağ geçidini sağlamanız gerekir. Örneğin:

    DeployAzureStack.ps1 -Verbose -NATVMStaticIP 10.10.10.10/24 -NATVMStaticGateway 10.10.10.1

### İnternet erişimi

NIC'nin İnternet'e bağlanabildiğinden emin olun. Hem NATVM'ye atanan yeni IP'nin (DHCP veya statik IP tarafından) hem de ana bilgisayar IP'sinin İnternet erişiminin olması gerekir. Bağlantı noktası 80 ve 443, graph.windows.net ve login.windows.net etki alanlarının altında kullanılır.

### Ara sunucu

Ortamınızda bir ara sunucu gerekiyorsa dağıtım parametresi olarak ara sunucu adresini ve bağlantı noktasını belirtin. Örneğin:

    DeployAzureStack.ps1 -Verbose -ProxyServer 172.11.1.1:8080

Azure Stack POC, ara sunucu kimlik doğrulamasını desteklemez. 

### Telemetri

Bağlantı noktası 443 (HTTPS) ağınız için açık olmalıdır. İstemci uç noktası https://vortex-win.data.microsoft.com'dur.


## Sonraki adımlar

[Azure Stack POC dağıtım paketini indirme](https://azure.microsoft.com/overview/azure-stack/try/?v=try)

[Azure Stack POC'yi dağıtma](azure-stack-run-powershell-script.md)



<!--HONumber=Sep16_HO3-->


