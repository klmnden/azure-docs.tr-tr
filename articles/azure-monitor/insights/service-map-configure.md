---
title: Azure'da hizmet eşlemesi yapılandırma | Microsoft Docs
description: Hizmet Eşlemesi, Windows ve Linux sistemleri üzerindeki uygulama bileşenlerini otomatik olarak bulan ve hizmetler arasındaki iletişimi eşleyen bir Azure çözümüdür. Bu makalede, ortamınızda hizmet eşlemesi dağıtmak ve çeşitli senaryoları de kullanım için Ayrıntılar sağlanır.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: d3d66b45-9874-4aad-9c00-124734944b2e
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/11/2019
ms.author: magoedte
ms.openlocfilehash: ef5ab3d4fe71f688a0b4f3879248ea1715874a0b
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58793739"
---
# <a name="configure-service-map-in-azure"></a>Azure'da hizmet eşlemesi yapılandırma

Hizmet Eşlemesi, Windows ve Linux sistemleri üzerindeki uygulama bileşenlerini otomatik olarak bulur ve hizmetler arasındaki iletişimi eşler. Bunları--kritik Hizmetleri sunmak birbirine sistemleri düşündüğünüz sunucularınızın görüntülemek için kullanabilirsiniz. Hizmet eşlemesi, tüm TCP bağlantılı mimarisi, gerekli bir aracı yüklemesini dışında hiçbir yapılandırma boyunca sunucuları, işlemler ve bağlantı noktaları arasındaki bağlantıları gösterir.

Bu makalede hizmet eşlemesi ve onboarding aracıları yapılandırma ayrıntılarını açıklar. Hizmet eşlemesi kullanma hakkında daha fazla bilgi için bkz. [Azure'da hizmet eşlemesi çözümünü kullanma]( service-map.md).

## <a name="supported-azure-regions"></a>Desteklenen Azure bölgeleri

Hizmet eşlemesi aşağıdaki Azure bölgeleri şu anda kullanılabilir:
- Doğu ABD
- Batı Orta ABD
- Orta Kanada
- Birleşik Krallık Güney
- Batı Avrupa
- Güneydoğu Asya

## <a name="supported-windows-operating-systems"></a>Desteklenen Windows işletim sistemleri

Aşağıdaki bölümde, Windows üzerinde bağımlılık aracısını için desteklenen işletim sistemleri listesi. 

>[!NOTE]
>Hizmet eşlemesi, yalnızca 64 bit platformları destekler.
>

### <a name="windows-server"></a>Windows Server

- Windows Server 2019
- Windows Server 2016 1803
- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2 SP1

### <a name="windows-desktop"></a>Windows masaüstü

- Windows 10, 1803
- Windows 10
- Windows 8.1
- Windows 8
- Windows 7

## <a name="supported-linux-operating-systems"></a>Desteklenen Linux işletim sistemleri

Aşağıdaki bölümde, Linux üzerinde bağımlılık aracısını için desteklenen işletim sistemleri listesi.  

- Yalnızca varsayılan ve SMP Linux çekirdek sürümleri desteklenir.
- PAE ve Xen gibi standart dışı çekirdek sürümleri hiçbir Linux dağıtımında desteklenmez. Örneğin, "2.6.16.21-0.8-xen" sürüm dizesi sistemiyle desteklenmiyor.
- Standart çekirdeklerin yeniden derlemeleri de dahil olmak üzere özel çekirdekler desteklenmez.

### <a name="red-hat-linux-7"></a>Red Hat Linux 7

| İşletim sistemi sürümü | Çekirdek sürümü |
|:--|:--|
| 7.4 | 3.10.0-693 |
| 7.5 | 3.10.0-862 |
| 7.6 | 3.10.0-957 |

### <a name="red-hat-linux-6"></a>Red Hat Linux 6

| İşletim sistemi sürümü | Çekirdek sürümü |
|:--|:--|
| 6.9 | 2.6.32-696 |
| 6.10 | 2.6.32-754 |

### <a name="centosplus"></a>CentOSPlus

| İşletim sistemi sürümü | Çekirdek sürümü |
|:--|:--|
| 6.9 | 2.6.32-696.18.7<br>2.6.32-696.30.1 |
| 6.10 | 2.6.32-696.30.1<br>2.6.32-754.3.5 |

### <a name="ubuntu-server"></a>Ubuntu Server

| İşletim sistemi sürümü | Çekirdek sürümü |
|:--|:--|
| Ubuntu 18.04 | kernel 4.15.\*<br>4.18* |
| Ubuntu 16.04.3 | Çekirdek 4.15. * |
| 16.04 | 4.4.\*<br>4.8.\*<br>4.10.\*<br>4.11.\*<br>4.13.\* |
| 14.04 | 3.13.\*<br>4.4.\* |

### <a name="suse-linux-11-enterprise-server"></a>SUSE Linux 11 Enterprise Server

| İşletim sistemi sürümü | Çekirdek sürümü
|:--|:--|
| 11 SP4 | 3.0.* |

### <a name="suse-linux-12-enterprise-server"></a>SUSE Linux 12 kuruluş sunucusu

| İşletim sistemi sürümü | Çekirdek sürümü
|:--|:--|
| 12 SP2 | 4.4. * |
| 12 SP3 | 4.4. * |

## <a name="dependency-agent-downloads"></a>Bağımlılık aracısını yükleme

| Dosya | İşletim Sistemi | Sürüm | SHA-256 |
|:--|:--|:--|:--|
| [InstallDependencyAgent-Windows.exe](https://aka.ms/dependencyagentwindows) | Windows | 9.7.4 | A111B92AB6CF28EB68B696C60FE51F980BFDFF78C36A900575E17083972989E0 |
| [InstallDependencyAgent-Linux64.bin](https://aka.ms/dependencyagentlinux) | Linux | 9.7.4 | AB58F3DB8B1C3DEE7512690E5A65F1DFC41B43831543B5C040FCCE8390F2282C |

## <a name="connected-sources"></a>Bağlı kaynaklar

Hizmet eşlemesi kendi verilerini Microsoft Dependency Aracıdan alır. Log Analytics aracısını Log analytics'e bağlantıları için bağımlılık Aracısı'nı kullanır. Başka bir deyişle, bir sunucuya yüklenmiş ve yapılandırılmış bağımlılık aracısını Log Analytics aracısını olması gerekir.  Aşağıdaki tabloda, hizmet eşlemesi çözümünü destekleyen bağlı kaynaklar açıklanmaktadır.

| Bağlı kaynak | Desteklenen | Açıklama |
|:--|:--|:--|
| Windows aracıları | Evet | Hizmet eşlemesi, analiz eder ve Windows bilgisayarlardan verileri toplar. <br><br>Ek olarak [Windows için Log Analytics aracısını](../../azure-monitor/platform/log-analytics-agent.md), Windows aracıları Microsoft Dependency Aracısı gerektirir. Desteklenen işletim sistemleri işletim sistemlerinin tam listesi için bkz. |
| Linux aracıları | Evet | Hizmet eşlemesi, analiz eder ve Linux bilgisayarlardan verileri toplar. <br><br>Ek olarak [Linux için Log Analytics aracısını](../../azure-monitor/platform/log-analytics-agent.md), Linux aracıları Microsoft Dependency Aracısı gerektirir. Desteklenen işletim sistemleri işletim sistemlerinin tam listesi için bkz. |
| System Center Operations Manager yönetim grubu | Evet | Hizmet eşlemesi analiz eder ve Windows ve Linux aracılarını bir bağlı olarak veri toplar [System Center Operations Manager yönetim grubu](../../azure-monitor/platform/om-agents.md). <br><br>System Center Operations Manager aracısının doğrudan Log Analytics’e bağlanması gerekir. |
| Azure depolama hesabı | Hayır | Azure Depolama'dan toplamak için hiçbir veri bu nedenle hizmet eşlemesi Aracısı bilgisayarlardan verileri toplar. |

Windows üzerinde Microsoft Monitoring Agent (MMA) hem System Center Operations Manager hem de Log Analytics tarafından toplayın ve göndermek için kullanılan izleme verileri. (Bu aracı System Center Operations Manager Aracısı, Log Analytics aracısını, MMA veya doğrudan aracı bağlama bağlı olarak adlandırılır.) System Center Operations Manager ve Log Analytics MMA'yı farklı çıkış-hazır sürümleri sağlar. Bu sürümlerin her biri System Center Operations Manager'a, Log Analytics'e veya her ikisine birden raporlayabilir.  

Linux üzerinde Log Analytics aracısını Linux toplar ve izleme verilerini Log analytics'e gönderir. Hizmet eşlemesi ile Log Analytics aracılarını hizmete doğrudan bağlı sunucularda kullanabilirsiniz veya Log Analytics ile tümleşik bir Operations Manager yönetim grubuna raporlama.  

Bu makalede, Linux veya Windows olarak System Center Operations Manager yönetim grubu veya doğrudan Log Analytics'e bağlı olup olmadığını biz tüm aracıları için başvuracağınız *Log Analytics aracısını*. 

Hizmet eşlemesi aracısının tüm veriler aktarmaz ve güvenlik duvarları ya da bağlantı noktalarını herhangi bir değişiklik gerektirmez. Hizmet eşlemesi verileri her zaman Log Analytics aracısını Log Analytics hizmetine tarafından doğrudan veya Log Analytics ağ geçidi üzerinden iletilir.

![Hizmet eşlemesi Aracısı](media/service-map-configure/agents.png)

Log Analytics'e bağlı bir yönetim grubuna sahip bir System Center Operations Manager müşterisi iseniz:

- System Center Operations Manager aracıları Log Analytics'e bağlanmak için Internet'e erişebiliyorsa ek yapılandırma gerekmiyor.  
- System Center Operations Manager aracılarının Log Analytics Internet üzerinden erişemiyorsanız, System Center Operations Manager ile çalışmak için Log Analytics ağ geçidi'ni yapılandırmanız gerekir.
  
Windows veya Linux bilgisayarlarınızın doğrudan hizmetine bağlanamazsa, ağ geçidini kullanarak Log Analytics çalışma alanına bağlamak için Log Analytics aracısını yapılandırmanız gerekir. Log Analytics ağ geçidi yapılandırmak ve dağıtmak hakkında daha fazla bilgi için bkz. [Log Analytics ağ geçidini kullanarak Internet erişimi bilgisayarları bağlama](../../azure-monitor/platform/gateway.md).  

### <a name="management-packs"></a>Yönetim paketleri

Log Analytics çalışma alanında hizmet eşlemesi etkinleştirildiğinde, 300 KB Yönetim Paketi bu çalışma alanındaki tüm Windows sunucuları iletilir. System Center Operations Manager aracıları kullanıyorsanız bir [bağlı yönetim grubu](../../azure-monitor/platform/om-agents.md), hizmet eşlemesi Yönetim Paketi, System Center Operations Manager üzerinden dağıtılır. 

Yönetim paketinin adı Microsoft.IntelligencePacks.ApplicationDependencyMonitor'dır. Bu, %Programfiles%\Microsoft izleme Agent\Agent\Health hizmet State\Management Packs\ yazılır. Yönetim Paketi kullanan veri kaynağı % Program files%\Microsoft izleme Agent\Agent\Health hizmet State\Resources,\<AutoGeneratedID > \ Microsoft.EnterpriseManagement.Advisor.ApplicationDependencyMonitorDataSource.dll.

## <a name="data-collection"></a>Veri toplama

Sistem bağımlılıklarınızı nasıl karmaşık olan bağlı olarak, günde yaklaşık 25 MB iletmek için her bir aracı bekleyebilirsiniz. Her aracı, her 15 saniyede hizmet eşlemesi bağımlılık verileri gönderir.  

Bağımlılık Aracısı'nı genellikle sistem belleğinin yüzde 0,1 ve sistemin CPU yüzdesi 0,1 tüketir.

## <a name="diagnostic-and-usage-data"></a>Tanılama ve kullanım verileri

Microsoft hizmet eşlemesi hizmeti kullanımınız vasıtasıyla kullanım ve performans verilerini otomatik olarak toplar. Microsoft, kalite, güvenlik ve hizmet eşlemesi hizmeti bütünlüğünü geliştirmek için bu verileri kullanır. Veriler işletim sistemi ve sürümü gibi yazılımınızın yapılandırması hakkında bilgiler içerir. Ayrıca IP adresi, DNS adı ve iş istasyonu adı doğru ve etkili sorun giderme özellikleri sağlamak için içerir. Ad, adres veya diğer iletişim bilgilerinizi toplamaz.

Veri toplama ve kullanım hakkında daha fazla bilgi için bkz. [Microsoft Online Services gizlilik bildirimi](https://go.microsoft.com/fwlink/?LinkId=512132).

## <a name="installation"></a>Yükleme

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

### <a name="azure-vm-extension"></a>Azure VM uzantısı

Bir uzantı (DependencyAgentWindows) Windows ve Linux (DependencyAgentLinux) için kullanılabilir ve bağımlılık Aracısı'nı kullanarak Azure sanal makinelerini kolayca dağıtım yapabilir bir [Azure VM uzantısı](https://docs.microsoft.com/azure/virtual-machines/windows/extensions-features).  Azure VM uzantısı ile Windows ve Linux bir PowerShell Betiği kullanılarak veya doğrudan sanal Makineye bir Azure Resource Manager şablonu kullanarak sanal makine için bağımlılık Aracısı'nı dağıtabilirsiniz.  Azure VM uzantısı aracıyla dağıtırsanız, aracılarınızı en son sürüme otomatik olarak güncelleştirilir.

PowerShell ile Azure VM uzantısı dağıtmak için aşağıdaki örneği kullanabilirsiniz:

```powershell
#
# Deploy the Dependency agent to every VM in a Resource Group
#

$version = "9.4"
$ExtPublisher = "Microsoft.Azure.Monitoring.DependencyAgent"
$OsExtensionMap = @{ "Windows" = "DependencyAgentWindows"; "Linux" = "DependencyAgentLinux" }
$rmgroup = "<Your Resource Group Here>"

Get-AzVM -ResourceGroupName $rmgroup |
ForEach-Object {
    ""
    $name = $_.Name
    $os = $_.StorageProfile.OsDisk.OsType
    $location = $_.Location
    $vmRmGroup = $_.ResourceGroupName
    "${name}: ${os} (${location})"
    Date -Format o
    $ext = $OsExtensionMap.($os.ToString())
    $result = Set-AzVMExtension -ResourceGroupName $vmRmGroup -VMName $name -Location $location `
    -Publisher $ExtPublisher -ExtensionType $ext -Name "DependencyAgent" -TypeHandlerVersion $version
    $result.IsSuccessStatusCode
}
```

Bağımlılık Aracısı'nı Vm'leriniz üzerinde yüklü emin olmak amacıyla daha kolay bir yol, Azure Resource Manager şablonunuzda aracı eklemektir.  Aşağıdaki JSON kod örneği eklenebilir *kaynakları* şablonunuzun bölümü.

```JSON
"type": "Microsoft.Compute/virtualMachines/extensions",
"name": "[concat(parameters('vmName'), '/DependencyAgent')]",
"apiVersion": "2017-03-30",
"location": "[resourceGroup().location]",
"dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', parameters('vmName'))]"
],
"properties": {
    "publisher": "Microsoft.Azure.Monitoring.DependencyAgent",
    "type": "DependencyAgentWindows",
    "typeHandlerVersion": "9.4",
    "autoUpgradeMinorVersion": true
}

```

### <a name="install-the-dependency-agent-on-microsoft-windows"></a>Microsoft Windows üzerinde bağımlılık aracısını yükleme

Bağımlılık aracısını el ile Windows bilgisayarlarda çalıştırarak yüklenebilir `InstallDependencyAgent-Windows.exe`. Bu yürütülebilir dosya hiçbir seçenek olmadan çalıştırırsanız, etkileşimli bir şekilde yüklemek için izlemeniz gereken bir Kurulum Sihirbazı başlar.  

>[!NOTE]
>Aracıyı yüklemek veya kaldırmak için yönetici ayrıcalıkları gereklidir.

Bağımlılık Aracısı'nı her bir Windows bilgisayara yüklemek için aşağıdaki adımları kullanın:

1.  Açıklanan yöntemlerden birini uygulayarak Windows için Log Analytics aracısını yükleme [Log Analytics Aracısı genel bakış](../../azure-monitor/platform/log-analytics-agent.md).
2.  Windows Aracısı'nı indirin ve aşağıdaki komutu kullanarak çalıştırabilirsiniz: 
    
    `InstallDependencyAgent-Windows.exe`

3.  Aracıyı yüklemek için Kurulum sihirbazını izleyin.
4.  Bağımlılık Aracısı'nı başlatmak başarısız olursa, ayrıntılı hata bilgileri için günlükleri denetleyin. Windows aracıları, %Programfiles%\Microsoft bağımlılık Agent\logs günlük dizindir. 

#### <a name="windows-command-line"></a>Windows komut satırı

Komut satırından yüklemek için aşağıdaki tabloda yer alan seçenekleri kullanın. Yükleme bayraklarının listesini görmek için yükleyiciyi çalıştırırken aşağıda gösterildiği gibi /? bayrağını kullanın.

    InstallDependencyAgent-Windows.exe /?

| Bayrağı | Açıklama |
|:--|:--|
| /? | Komut satırı seçeneklerinin listesini alır. |
| /S | Kullanıcıdan bilgi istenmeden sessiz yükleme gerçekleştirir. |

Windows bağımlılık aracısını dosyaları C:\Program Files\Microsoft bağımlılık Aracısı varsayılan olarak yerleştirilir.

### <a name="install-the-dependency-agent-on-linux"></a>Linux üzerinde bağımlılık aracısını yükleme

Bağımlılık Aracısı'nı Linux bilgisayarlardan yüklenir `InstallDependencyAgent-Linux64.bin`, kendi kendine ayıklanan bir ikili içeren bir kabuk betiği. Kullanarak dosyayı çalıştırabilirsiniz `sh` veya yürütme izinleri dosya için.

>[!NOTE]
> Aracıyı yüklemek veya yapılandırmak için kök erişimi gerekir.

Bağımlılık Aracısı'nı her Linux bilgisayara yüklemek için aşağıdaki adımları kullanın:

1.  Açıklanan yöntemlerden birini uygulayarak Log Analytics aracısını yükleme [Log Analytics Aracısı genel bakış](../../azure-monitor/platform/log-analytics-agent.md).
2.  Linux bağımlılık aracısını kök olarak aşağıdaki komutu çalıştırarak yükleyin:
    
    `sh InstallDependencyAgent-Linux64.bin`

3.  Bağımlılık Aracısı'nı başlatmak başarısız olursa, ayrıntılı hata bilgileri için günlükleri denetleyin. Linux aracıları, /var/opt/microsoft/dependency-agent/log günlük dizindir.

Çalıştırma yükleme bayrakları bir listesini görmek için yükleme programını Yardım bayrağı gibi.

    InstallDependencyAgent-Linux64.bin -help

| Bayrağı | Açıklama |
|:--|:--|
| -Yardım | Komut satırı seçeneklerinin listesini alır. |
| -s | Kullanıcıdan bilgi istenmeden sessiz yükleme gerçekleştirir. |
| --denetleyin | İzinleri ve işletim sistemini denetleyin ama aracıyı yüklemeyin. |

Bağımlılık Aracısı'nı dosyaları aşağıdaki dizinlerde yerleştirilir:

| Dosyalar | Konum |
|:--|:--|
| Çekirdek dosyaları | /opt/microsoft/dependency-agent |
| Günlük dosyaları | /var/opt/microsoft/dependency-agent/log |
| Yapılandırma dosyaları | /etc/opt/microsoft/dependency-agent/config |
| Hizmet yürütülebilir dosyaları | /opt/microsoft/dependency-agent/bin/microsoft-dependency-agent<br>/opt/microsoft/dependency-agent/bin/microsoft-dependency-agent-manager |
| İkili depolama dosyaları | /var/opt/microsoft/dependency-agent/storage |

## <a name="installation-script-examples"></a>Yükleme betiği örnekleri

Bağımlılık aracısını birçok sunucuda tek seferde kolayca dağıtmak için aşağıdaki betiği örneği indirip bağımlılık aracısını Windows veya Linux'ta sağlanır.

### <a name="powershell-script-for-windows"></a>Windows için PowerShell betiği

```powershell
Invoke-WebRequest "https://aka.ms/dependencyagentwindows" -OutFile InstallDependencyAgent-Windows.exe

.\InstallDependencyAgent-Windows.exe /S
```

### <a name="shell-script-for-linux"></a>Linux için kabuk betiği

```
wget --content-disposition https://aka.ms/dependencyagentlinux -O InstallDependencyAgent-Linux64.bin
sudo sh InstallDependencyAgent-Linux64.bin -s
```

## <a name="desired-state-configuration"></a>İstenen Durum Yapılandırması

Desired State Configuration ' nı (DSC) kullanarak bağımlılık aracısını dağıtmak için aşağıdaki örnek kod ile xPSDesiredStateConfiguration modülü kullanabilirsiniz:

```powershell
configuration ServiceMap {

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    $DAPackageLocalPath = "C:\InstallDependencyAgent-Windows.exe"

    Node localhost
    {
        # Download and install the Dependency agent
        xRemoteFile DAPackage 
        {
            Uri = "https://aka.ms/dependencyagentwindows"
            DestinationPath = $DAPackageLocalPath
        }

        xPackage DA
        {
            Ensure="Present"
            Name = "Dependency Agent"
            Path = $DAPackageLocalPath
            Arguments = '/S'
            ProductId = ""
            InstalledCheckRegKey = "HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\DependencyAgent"
            InstalledCheckRegValueName = "DisplayName"
            InstalledCheckRegValueData = "Dependency Agent"
            DependsOn = "[xRemoteFile]DAPackage"
        }
    }
}
```

## <a name="remove-the-dependency-agent"></a>Bağımlılık aracısını Kaldır

### <a name="uninstall-agent-on-windows"></a>Windows aracısını kaldırma

Bir yönetici, Windows Denetim Masası'ndan için bağımlılık Aracısı kaldırabilirsiniz.

Bir yönetici, %Programfiles%\Microsoft bağımlılık Aracısı'nı kaldırmak için bağımlılık Agent\Uninstall.exe olarak da çalıştırabilirsiniz.

### <a name="uninstall-agent-on-linux"></a>Linux aracısını kaldırma

Aşağıdaki komutla Linux'taki bağımlılık aracısını kaldırabilirsiniz.

RHEL, CentOs veya Oracle:

```
sudo rpm -e dependency-agent
```

Ubuntu:

```
sudo apt -y purge dependency-agent
```

## <a name="troubleshooting"></a>Sorun giderme

Bu bölümde, yüklenmesini veya çalıştırılmasını hizmet eşlemesi herhangi bir sorun varsa, size yardımcı olabilir. Sorununuzu hala çözümleyememesi durumunda Microsoft Support'ne başvurun.

### <a name="dependency-agent-installation-problems"></a>Bağımlılık Aracısı yükleme sorunları

#### <a name="installer-prompts-for-a-reboot"></a>Yükleyici için yeniden başlatma ister

Bağımlılık Aracısı'nı *genellikle* yükleme veya kaldırma işlemi sırasında bir yeniden başlatma gerektirmez. Ancak, bazı nadir durumlarda, Windows Server bir yükleme işlemine devam etmek için bir yeniden başlatma gerektirir. Genellikle Microsoft Visual C++ yeniden dağıtılabilir, bir bağımlılık nedeniyle kilitli bir dosyayı yeniden başlatma gerektirdiğinde bu gerçekleşir.

#### <a name="message-unable-to-install-dependency-agent-visual-studio-runtime-libraries-failed-to-install-code--codenumber-appears"></a>İleti "bağımlılık aracısını yükleme yüklenemiyor: Visual Studio çalışma zamanı kitaplıkları yüklenemedi (kod [code_number] =) "görünür

Microsoft Dependency Aracısı, Microsoft Visual Studio çalışma zamanı kitaplıkları oluşturulmuştur. Kitaplık yüklenirken bir sorun varsa bir ileti alırsınız. 

Çalışma Zamanı Kitaplığı yükleyicileri %LOCALAPPDATA%\temp klasöründe günlükleri oluşturun. Dosyasıdır dd_vcredist_arch_yyyymmddhhmmss.log, burada *arch* "x86" veya "amd64" ve *yyyyaaggssddss* tarih ve saat (24 saatlik düzende) günlük ne zaman oluşturulduğu. Günlük yükleme engelleme sorunu hakkında ayrıntılar sağlar.

Yüklemek yararlı olabilir [en son çalışma zamanı kitaplıkları](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) kendiniz ilk.

Aşağıdaki tabloda, kod sayıları ve önerilen çözümler listelenmektedir.

| Kod | Açıklama | Çözüm |
|:--|:--|:--|
| 0x17 | Kitaplık yükleyici, yüklü olmayan bir Windows update gerektirir. | En son kitaplık yükleyicisi günlüğe bakın.<br><br>"Windows8.1-KB2999226-x64.msu" başvuru bir çizgiyle izlediyseniz "hatası 0x80240017: MSU paketi çalıştırılamadı,"KB2999226 yüklemek için gereken önkoşulları yoktur. Önkoşullar bölümünde yönergeleri [Windows Evrensel C çalışma zamanı](https://support.microsoft.com/kb/2999226). Windows Update'i çalıştırın ve birden çok kez önkoşulları yüklemek için yeniden başlatma gerekebilir.<br><br>Microsoft Dependency aracı yükleyiciyi yeniden çalıştırın. |

### <a name="post-installation-issues"></a>Yükleme sonrası sorunları

#### <a name="server-doesnt-appear-in-service-map"></a>Sunucu hizmeti haritada görünmüyor

Bağımlılık Aracısı yüklendi, ancak hizmet eşlemesi çözüm sunucunuzda görmüyorsanız:
* Bağımlılık Aracısı'nı başarıyla yüklü mü? Bu hizmet yüklü olup olmadığını denetliyor ve çalıştırarak doğrulayabilirsiniz.<br><br>
**Windows**: "Microsoft Dependency agent" adlı hizmet için Ara<br>
**Linux**: "Microsoft-bağımlılık-agent." çalışan işlemini arayın

* Bulunduğunuz [ücretsiz fiyatlandırma katmanı, Operations Management Suite/Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions)? Ücretsiz planı için en fazla beş benzersiz hizmet eşlemesi sunucularının sağlar. Önceki beş artık veri gönderen bile hizmet eşlemesinde, sonraki tüm sunucuları görünmez.

* Sunucu gönderen günlük ve performans verilerini Log Analytics'e mi? Günlük araması gidin ve bilgisayarınız için aşağıdaki sorguyu çalıştırın: 

    Kullanım | Burada bilgisayar == "bilgisayar-adı" | SUM(Quantity), veri türü tarafından any(QuantityUnit) özetleme

Sonuçlar arasında olaylar çeşitli mı aldınız? Verilerin güncel mi? Bu durumda, Log Analytics aracısını düzgün çalışmasını ve Log Analytics ile iletişim kurulurken. Aksi takdirde, sunucudaki aracı denetleyin: [Windows sorun giderme için log Analytics aracısını](https://support.microsoft.com/help/3126513/how-to-troubleshoot-monitoring-onboarding-issues) veya [Linux sorun giderme için Log Analytics aracısını](../../azure-monitor/platform/agent-linux-troubleshoot.md).

#### <a name="server-appears-in-service-map-but-has-no-processes"></a>Sunucu, hizmet eşlemesinde görünür ancak hiçbir işlem sahiptir

Hizmet eşlemesi sunucunuzun bakın, ancak işlem veya bağlantı veri yok, bağımlılık Aracısı'nı yüklü ve çalışır durumdadır, ancak çekirdek sürücüsü yüklenmedi gösterir. 

C:\Program Files\Microsoft bağımlılık Agent\logs\wrapper.log dosyası (Windows) veya /var/opt/microsoft/dependency-agent/log/service.log dosyası (Linux) kontrol edin. Dosyanın son satırları çekirdek neden yüklenmedi belirtmeniz gerekir. Örneğin, çekirdek güncelleştirilmiş çekirdek Linux üzerinde desteklenmeyebilir.

## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [Hizmet eşlemesini kullanmak]( service-map.md) dağıttıktan ve yapılandırılmış sonra.