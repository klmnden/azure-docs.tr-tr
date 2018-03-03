---
title: "Azure üzerinde hizmet haritası yapılandırma | Microsoft Docs"
description: "Hizmet Eşlemesi, Windows ve Linux sistemleri üzerindeki uygulama bileşenlerini otomatik olarak bulan ve hizmetler arasındaki iletişimi eşleyen bir Azure çözümüdür. Bu makalede hizmet Haritası ortamınıza dağıtmak ve çeşitli senaryolarda içinde kullanma ile ilgili ayrıntıları sağlar."
services: operations-management-suite
documentationcenter: 
author: daveirwin1
manager: jwhit
editor: tysonn
ms.assetid: d3d66b45-9874-4aad-9c00-124734944b2e
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/18/2016
ms.author: daseidma;bwren;dairwin
ms.openlocfilehash: 66023a501360a8d73f88e2fe9682c1bf8f207a07
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="configure-service-map-in-azure"></a>Hizmet eşlemesi Azure'da yapılandırın
Hizmet Eşlemesi, Windows ve Linux sistemleri üzerindeki uygulama bileşenlerini otomatik olarak bulur ve hizmetler arasındaki iletişimi eşler. Bunları--Kritik hizmetler sunan birbirine bağlı sistemler olarak düşündüğünüz sunucularınızı görüntülemek için kullanabilirsiniz. Hizmet eşlemesi gerekli, bir aracı yüklemesini dışındaki herhangi bir yapılandırma TCP bağlı mimarisiyle boyunca sunucuları, işlemleri ve bağlantı noktaları arasındaki bağlantıları gösterir.

Bu makalede hizmet Haritası ve ekleme aracıları yapılandırma ayrıntılarını açıklanmaktadır. Hizmet eşlemesi kullanarak hakkında daha fazla bilgi için bkz: [Azure hizmet Haritası çözümü kullanan](operations-management-suite-service-map.md).

## <a name="dependency-agent-downloads"></a>Bağımlılık Aracısı indirir
| Dosya | İşletim Sistemi | Sürüm | SHA-256 |
|:--|:--|:--|:--|
| [InstallDependencyAgent-Windows.exe](https://aka.ms/dependencyagentwindows) | Windows | 9.4.1 | 0DCCE16495E7A3254A5FE1B5EADE66110984C3BE799A1FAAD7D119F23614592E |
| [InstallDependencyAgent-Linux64.bin](https://aka.ms/dependencyagentlinux) | Linux | 9.4.2 | E0888727125FA4E4ECACFB4B2633284C014933EE0CC2F7A9F93F36AEDBD6C2C4  |


## <a name="connected-sources"></a>Bağlı kaynaklar
Hizmet eşlemesi Microsoft bağımlılık Aracısı'ndan verileri alır. Günlük analizi için kendi bağlantıları için OMS aracısının bağımlılık Aracısı'nı bağlıdır. Bunun anlamı bir sunucuya yüklenir ve yapılandırılır. ilk OMS Aracısı olması gerekir ve ardından bağımlılık Aracısı yüklenir. Aşağıdaki tabloda hizmet Haritası çözümünü destekler bağlı kaynakları açıklanmaktadır.

| Bağlı kaynak | Desteklenen | Açıklama |
|:--|:--|:--|
| Windows aracıları | Evet | Hizmet eşlemesi analiz eder ve Windows Aracısı bilgisayarlardan verileri toplar. <br><br>Ek olarak [OMS Aracısı](../log-analytics/log-analytics-windows-agent.md), Windows aracıları Microsoft bağımlılık Aracısı gerektirir. Bkz: [desteklenen işletim sistemleri](#supported-operating-systems) işletim sistemi sürümleri tam bir listesi. |
| Linux aracıları | Evet | Hizmet eşlemesi analiz eder ve Linux Aracısı bilgisayarlardan verileri toplar. <br><br>Ek olarak [OMS Aracısı](../log-analytics/log-analytics-linux-agents.md), Linux aracılarını Microsoft bağımlılık Aracısı gerektirir. Bkz: [desteklenen işletim sistemleri](#supported-operating-systems) işletim sistemi sürümleri tam bir listesi. |
| System Center Operations Manager yönetim grubu | Evet | Hizmet eşlemesi Windows ve Linux aracıları bağlı bir veri toplar ve analiz eder [System Center Operations Manager yönetim grubu](../log-analytics/log-analytics-om-agents.md). <br><br>System Center Operations Manager Aracısı bilgisayardan doğrudan bağlantı günlük analizi için gereklidir. Veri yönetim grubu için günlük analizi çalışma alanına iletilir.|
| Azure depolama hesabı | Hayır | Azure depolama biriminden toplamak için hiçbir veri nedenle hizmet eşlemesi Aracısı bilgisayarlardan verileri toplar. |

Hizmet eşlemesi yalnızca 64-bit platformları destekler.

Windows, Microsoft İzleme Aracısı'nı (MMA) hem System Center Operations Manager hem de günlük analizi toplamak ve göndermek için kullanılan izleme verileri. (Bu aracı System Center Operations Manager Aracısı, OMS Aracısı, günlük analizi Aracısı, MMA veya doğrudan Aracısı bağlamı bağlı olarak adlandırılır.) System Center Operations Manager ve günlük analizi MMA farklı çıkış-yepyeni sürümleri sağlar. Bu sürümleri her System Center Operations Manager, günlük analizi için veya her ikisine de bildirebilirsiniz.  

Linux, OMS Aracısı Linux toplar ve izleme için günlük analizi verilerini gönderir. Hizmet eşlemesi OMS doğrudan aracılarıyla sunucularda veya günlük analizi için System Center Operations Manager Yönetim grupları bağlı olan sunucuları kullanabilirsiniz.  

Bu makalede, sizi tüm aracılara--başvurmak olup olmadığını Linux veya Windows, System Center Operations Manager yönetim grubu veya--"OMS Aracısı." günlük analizi için doğrudan bağlı olup olmadığını Bağlam için yalnızca ihtiyaç duyduğunuzda aracısının belirli dağıtım adı kullanacağız.

Hizmet eşlemesi Aracısı tüm verileri aktarmaz ve güvenlik duvarları veya bağlantı noktalarını herhangi bir değişiklik gerektirmez. Hizmet eşlemesi verileri her zaman günlük analizi için OMS aracısı tarafından doğrudan veya OMS ağ geçidi üzerinden aktarılır.

![Hizmet eşlemesi aracıları](media/oms-service-map/agents.png)

Günlük analizi için bağlı yönetim grubu ile System Center Operations Manager müşteri varsa:

- System Center Operations Manager aracıları günlük Analizi'ne bağlanmak için Internet erişebiliyorsanız, ek yapılandırma gereklidir.  
- System Center Operations Manager aracıları, Internet üzerinden günlük analizi erişemiyorsanız, System Center Operations Manager ile çalışmak için OMS ağ geçidi yapılandırmanız gerekir.
  
OMS doğrudan Aracısı'nı kullanıyorsanız, OMS günlük analizi veya OMS ağ geçidine bağlanmak için aracının kendisi yapılandırmanız gerekir. OMS ağ geçidi yüklenebilir [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=52666).

### <a name="management-packs"></a>Yönetim paketleri
Hizmet Haritası günlük analizi çalışma etkinleştirildiğinde, 300-KB Yönetim Paketi bu çalışma alanındaki tüm Windows sunucularına gönderilir. System Center Operations Manager aracıları kullanıyorsanız, bir [bağlı yönetim grubu](../log-analytics/log-analytics-om-agents.md), hizmet Haritası Yönetim Paketi System Center Operations Manager'dan dağıtılır. Aracıları doğrudan bağlıysanız, günlük analizi Yönetim Paketi sunar.

Yönetim Paketi Microsoft.IntelligencePacks.ApplicationDependencyMonitor olarak adlandırılır. %Programfiles%\Microsoft izleme Agent\Agent\Health hizmet State\Management Packs\ yazılır. Yönetim Paketi kullanan veri kaynağı % Program files%\Microsoft izleme Agent\Agent\Health hizmet State\Resources olan\<AutoGeneratedID > \Microsoft.EnterpriseManagement.Advisor.ApplicationDependencyMonitorDataSource.dll.

## <a name="installation"></a>Yükleme
### <a name="install-the-dependency-agent-on-microsoft-windows"></a>Microsoft Windows bağımlılık Aracısı'nı yükleme
Yüklemek veya aracıyı kaldırmak için yönetici ayrıcalıkları gerekir.

Bağımlılık Aracısı'nı InstallDependencyAgent Windows.exe üzerinden Windows bilgisayarlara yüklenir. Bu yürütülebilir dosya seçenekleri olmadan çalıştırırsanız, etkileşimli olarak yüklemek için izleyebileceğiniz Sihirbazı'nı başlatır.  

Her Windows bilgisayarda bağımlılık Aracısı'nı yüklemek için aşağıdaki adımları kullanın:

1.  Kısmındaki yönergeleri kullanarak OMS Aracısı'nı yükleme [Azure günlük analizi hizmeti bağlanmak Windows bilgisayarlara](../log-analytics/log-analytics-windows-agent.md).
2.  Windows Aracısı'nı indirin ve çalıştırın aşağıdaki komutu kullanarak: <br>`InstallDependencyAgent-Windows.exe`
3.  Aracıyı yüklemek için sihirbazı izleyin.
4.  Bağımlılık Aracısı'nı başlatmak başarısız olursa, ayrıntılı hata bilgileri için günlükleri denetleyin. Windows aracısında %Programfiles%\Microsoft bağımlılık Agent\logs günlük dizindir. 

#### <a name="windows-command-line"></a>Windows komut satırı
Bir komut satırından yüklemek için aşağıdaki tablodaki seçenekleri kullanın. Yükleme bayrakları listesini görmek için yükleyiciyi kullanarak çalıştırmak /? aşağıdaki gibi bayrak.

    InstallDependencyAgent-Windows.exe /?

| Bayrağı | Açıklama |
|:--|:--|
| /? | Komut satırı seçeneklerinin listesini alın. |
| /S | Kullanıcı etkileşimi ile sessiz bir yükleme gerçekleştirin. |

Dosyalar Windows bağımlılık aracısı için varsayılan olarak C:\Program Files\Microsoft bağımlılık Aracısı yerleştirilir.

### <a name="install-the-dependency-agent-on-linux"></a>Linux'ta bağımlılık Aracısı'nı yüklemek
Kök erişimi yüklemek veya aracıyı yapılandırmak için gereklidir.

Bağımlılık Aracısı'nı InstallDependencyAgent Linux64.bin, kendiliğinden açılan bir ikili içeren bir kabuk betiği aracılığıyla Linux bilgisayarlara yüklenir. Dosyayı Paylaş kullanarak çalıştırın ya da eklemek yürütme dosya izinlerinin.
 
Her bir Linux bilgisayarda bağımlılık Aracısı'nı yüklemek için aşağıdaki adımları kullanın:

1.  Kısmındaki yönergeleri kullanarak OMS Aracısı'nı yükleme [toplamak ve Linux bilgisayarları veri yönetmek](https://technet.microsoft.com/library/mt622052.aspx).
2.  Linux bağımlılık Aracısı'nı, aşağıdaki komutu kullanarak kök olarak yükleyin:<br>`sh InstallDependencyAgent-Linux64.bin`
3.  Bağımlılık Aracısı'nı başlatmak başarısız olursa, ayrıntılı hata bilgileri için günlükleri denetleyin. Linux aracısında /var/opt/microsoft/dependency-agent/log günlük dizindir.

Çalıştır yükleme bayrakları listesini görmek için yükleme programını Yardım bayrağı gibi.

    InstallDependencyAgent-Linux64.bin -help

| Bayrağı | Açıklama |
|:--|:--|
| -Yardım | Komut satırı seçeneklerinin listesini alın. |
| -s | Kullanıcı etkileşimi ile sessiz bir yükleme gerçekleştirin. |
| --denetleyin | İzinler ve işletim sistemi denetle, ancak aracı yüklemeyin. |

Bağımlılık aracısı için dosyalar aşağıdaki dizinlerindeki yerleştirilir:

| Dosyalar | Konum |
|:--|:--|
| Çekirdek dosyaları | /OPT/Microsoft/Dependency-Agent |
| Günlük dosyaları | /var/OPT/Microsoft/Dependency-Agent/log |
| Yapılandırma dosyaları | /etc/OPT/Microsoft/Dependency-Agent/config |
| Hizmet yürütülebilir dosyalar | /OPT/Microsoft/Dependency-Agent/bin/Microsoft-Dependency-Agent<br>/OPT/Microsoft/Dependency-Agent/bin/Microsoft-Dependency-Agent-Manager |
| İkili depolama dosyaları | /var/OPT/Microsoft/Dependency-Agent/Storage |

## <a name="installation-script-examples"></a>Yükleme komut dosyası örnekleri
Kolayca bağımlılık Aracısı pek çok sunucu üzerinde aynı anda dağıtmak için bir komut dosyası kullanmak yararlı olur. Aşağıdaki komut örnekleri indirin ve Windows ya da Linux bağımlılık Aracısı'nı yüklemek için kullanabilirsiniz.

### <a name="powershell-script-for-windows"></a>Windows PowerShell Betiği
```PowerShell
Invoke-WebRequest "https://aka.ms/dependencyagentwindows" -OutFile InstallDependencyAgent-Windows.exe

.\InstallDependencyAgent-Windows.exe /S
```

### <a name="shell-script-for-linux"></a>Linux için kabuk komut dosyası
```
wget --content-disposition https://aka.ms/dependencyagentlinux -O InstallDependencyAgent-Linux64.bin
sudo sh InstallDependencyAgent-Linux64.bin -s
```

## <a name="azure-vm-extension"></a>Azure VM uzantısı
Bağımlılık Aracısı'nı kullanarak Azure sanal makinelerini kolayca dağıtabilirsiniz bir [Azure VM uzantısı](https://docs.microsoft.com/azure/virtual-machines/windows/extensions-features).  Azure VM uzantısıyla bağımlılık Aracısı'nı Vm'leriniz VM'in Azure Resource Manager şablonunda doğrudan veya bir PowerShell komut dosyası aracılığıyla dağıtabilirsiniz.  Uzantı hem Windows (DependencyAgentWindows) hem de Linux (DependencyAgentLinux) için kullanılabilir.  Azure VM uzantısı dağıtırsanız, en son sürümlerine aracılarınızı otomatik olarak güncelleştirilebilir.

PowerShell aracılığıyla Azure VM uzantısı dağıtmak için aşağıdaki örnekte kullanabilirsiniz:
```PowerShell
#
# Deploy the Dependency Agent to every VM in a Resource Group
#

$version = "9.3"
$ExtPublisher = "Microsoft.Azure.Monitoring.DependencyAgent"
$OsExtensionMap = @{ "Windows" = "DependencyAgentWindows"; "Linux" = "DependencyAgentLinux" }
$rmgroup = "<Your Resource Group Here>"

Get-AzureRmVM -ResourceGroupName $rmgroup |
ForEach-Object {
    ""
    $name = $_.Name
    $os = $_.StorageProfile.OsDisk.OsType
    $location = $_.Location
    $vmRmGroup = $_.ResourceGroupName
    "${name}: ${os} (${location})"
    Date -Format o
    $ext = $OsExtensionMap.($os.ToString())
    $result = Set-AzureRmVMExtension -ResourceGroupName $vmRmGroup -VMName $name -Location $location `
    -Publisher $ExtPublisher -ExtensionType $ext -Name "DependencyAgent" -TypeHandlerVersion $version
    $result.IsSuccessStatusCode
}
```

Emin olmak için daha kolay bir yol bağımlılık Aracısı'nı açıktır, VM'lerin her birinde Aracısı, Azure Resource Manager şablonu eklemektir.  Bağımlılık Aracısı'nı OMS aracısında hala bağlı olduğunu unutmayın böylece [OMS Aracısı VM uzantısı](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-vm-extension) önce dağıtılmalıdır.  Aşağıdaki kod parçacığında, JSON eklenebilir *kaynakları* bölümü.
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
    "typeHandlerVersion": "9.3",
    "autoUpgradeMinorVersion": true
}

```


## <a name="desired-state-configuration"></a>İstenen Durum Yapılandırması
Bağımlılık Aracısı istenen durum yapılandırması üzerinden dağıtmak için xPSDesiredStateConfiguration modülü ve biraz kod aşağıdaki gibi kullanabilirsiniz:
```
configuration ServiceMap {

Import-DscResource -ModuleName xPSDesiredStateConfiguration

$DAPackageLocalPath = "C:\InstallDependencyAgent-Windows.exe"

Node localhost
{ 
    # Download and install the Dependency Agent
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

## <a name="uninstallation"></a>Kaldırma
### <a name="uninstall-the-dependency-agent-on-windows"></a>Windows üzerinde bağımlılık Aracısı'nı kaldırın
Bir yönetici için bağımlılık Aracısı Windows Denetim Masası'ndan kaldırabilirsiniz.

Bir yönetici, %Programfiles%\Microsoft Agent\Uninstall.exe bağımlılık Aracısı'nı kaldırmak için bağımlılık olarak da çalıştırabilirsiniz.

### <a name="uninstall-the-dependency-agent-on-linux"></a>Linux üzerinde bağımlılık Aracısı'nı kaldırın
Aşağıdaki komutla Linux bağımlılık aracısını kaldırabilirsiniz.
<br>RHEL, CentOs veya Oracle:
```
sudo rpm -e dependency-agent
```
Ubuntu:
```
sudo apt -y purge dependency-agent
```
## <a name="troubleshooting"></a>Sorun giderme
Yükleme veya hizmet Haritası çalıştıran herhangi bir sorun varsa, bu bölümde, yardımcı olabilir. Sorununuzu hala çözümleyemiyorsa, lütfen Microsoft Support başvurun.

### <a name="dependency-agent-installation-problems"></a>Bağımlılık Aracısı yükleme sorunları
#### <a name="installer-asks-for-a-reboot"></a>Yeniden başlatma için yükleyici sorar
Bağımlılık Aracısı'nı *genellikle* yükleme veya kaldırma işlemi sırasında yeniden başlatma gerektirmez. Ancak, bazı nadir durumlarda, Windows Server yükleme işlemine devam etmek için yeniden başlatma gerektirir. Bu, genellikle Microsoft Visual C++ yeniden dağıtılabilir, bir bağımlılık nedeniyle kilitli bir dosyayı yeniden başlatma gerektiren ortaya çıkar.

#### <a name="message-unable-to-install-dependency-agent-visual-studio-runtime-libraries-failed-to-install-code--codenumber-appears"></a>İleti "bağımlılık aracısı yüklemek için oluşturulamıyor: Visual Studio çalışma zamanı kitaplıkları yüklenemedi (kod [code_number] =)" görüntülenir

Microsoft bağımlılık Aracısı Microsoft Visual Studio çalışma zamanı kitaplıkları'nı yerleşik olarak bulunur. Kitaplıkları yükleme sırasında bir sorun varsa bir ileti alırsınız. 

Çalışma Zamanı Kitaplığı yükleyicileri günlükleri %LOCALAPPDATA%\temp klasöründe oluşturun. The file is dd_vcredist_arch_yyyymmddhhmmss.log, where *arch* is "x86" or "amd64" and *yyyymmddhhmmss* is the date and time (24-hour clock) when the log was created. Günlük yükleme engelleme sorunu hakkında ayrıntılar sağlar.

Yüklemek yararlı olabilecek [son çalışma zamanı kitaplıkları](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) kendiniz ilk.

Aşağıdaki tabloda, kod numaraları ve önerilen çözümler listelenmektedir.

| Kod | Açıklama | Çözüm |
|:--|:--|:--|
| 0x17 | Kitaplık yükleyicinin yüklenmemiştir bir Windows güncelleştirmesi gerekir. | En son kitaplığı yükleyici günlüğüne bakın.<br><br>"Windows8.1-KB2999226-x64.msu" başvuru bir çizgiyle izlediyseniz "hata 0x80240017: MSU paketini yürütülemedi" KB2999226 yüklemek amacıyla önkoşulları yok. Önkoşullar bölümünde'ndaki yönergeleri izleyin [Windows Evrensel C çalışma zamanı](https://support.microsoft.com/kb/2999226). Windows Update'i çalıştırın ve önkoşulları yüklemek için birden çok kez yeniden başlatma gerekebilir.<br><br>Microsoft bağımlılık Aracısı yükleyiciyi yeniden çalıştırın. |

### <a name="post-installation-issues"></a>Yükleme sonrası sorunları
#### <a name="server-doesnt-appear-in-service-map"></a>Sunucu hizmet eşlemesinde görünmüyor
Bağımlılık Aracısı yükleme başarılı oldu, ancak sunucunuzun hizmet Haritası çözümde görmüyorum varsa:
* Bağımlılık Aracısı'nı başarıyla yüklenir? Bu hizmeti yüklü olup olmadığını görmek için denetleme ve çalıştırarak doğrulayabilirsiniz.<br><br>
**Windows**: "Microsoft Dependency Aracısı" adlı hizmet için Ara<br>
**Linux**: çalışan işlem için "microsoft-bağımlılık-agent." arayın

* Bulunduğunuz [ücretsiz fiyatlandırma katmanı Operations Management Suite/Log Analytics,](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions#offers-and-pricing-tiers)? Ücretsiz planı en fazla beş benzersiz hizmet Haritası sunucularda izin verir. Önceki beş artık veri gönderiyor olsa bile herhangi bir sonraki sunucu hizmet eşlemesinde görünmez.

* Sunucu gönderen günlük ve performans verileri günlük Analizi'ne mi? Günlük arama gidin ve bilgisayarınız için aşağıdaki sorguyu çalıştırın: 

        * Computer="<your computer name here>" | measure count() by Type
        
  Sonuçlarda olayları çeşitli mı aldınız? Verilerin son mi? Bu durumda, OMS Aracısı doğru işletim ve günlük analizi ile iletişim kurarken. Değilse, sunucunuzdaki OMS Aracısı kontrol edin: [OMS Aracısı Windows için sorun giderme](https://support.microsoft.com/help/3126513/how-to-troubleshoot-operations-management-suite-onboarding-issues) veya [Linux sorun giderme için OMS aracısının](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/Troubleshooting.md).

#### <a name="server-appears-in-service-map-but-has-no-processes"></a>Sunucu hizmet eşlemesinde görünür ancak hiçbir işlem var
Hizmet eşlemesi sunucunuzun bakın, ancak hiçbir işlem veya bağlantı veri varsa, bağımlılık aracısı yüklü olduğundan ve çalıştığından, ancak çekirdek sürücüsü yüklenmeyen gösterir. 

C:\Program Files\Microsoft bağımlılık Agent\logs\wrapper.log dosyası (Windows) veya /var/opt/microsoft/dependency-agent/log/service.log dosyası (Linux) denetleyin. Dosyanın son satırları çekirdek neden yüklenmeyen belirtmeniz gerekir. Örneğin, çekirdek güncelleştirilmiş çekirdek Linux'ta desteklenmeyebilir.

## <a name="data-collection"></a>Veri toplama
Kabaca 25 MB günde nasıl sistemi bağımlılıkları karmaşıktır bağlı olarak iletmek için her bir aracının bekleyebilirsiniz. Her bir aracının 15 dakikada hizmet Haritası bağımlılık verileri gönderir.  

Bağımlılık Aracısı'nı genellikle sistem belleğinin yüzde 0,1 ve sistem CPU yüzdesi 0,1 tüketir.

## <a name="supported-azure-regions"></a>Desteklenen Azure bölgeleri
Hizmet eşlemesi aşağıdaki Azure bölgelerinde şu anda kullanılabilir değil:
- Doğu ABD
- Batı Avrupa
- Batı Orta ABD
- Güneydoğu Asya


## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri
Aşağıdaki bölümlerde bağımlılık aracısı için desteklenen işletim sistemleri listelenmektedir. Hizmet eşlemesi 32-bit mimariler için herhangi bir işletim sistemini desteklemiyor.

### <a name="windows-server"></a>Windows Server
- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2 SP1

### <a name="windows-desktop"></a>Windows masaüstü
- Windows 10
- Windows 8.1
- Windows 8
- Windows 7

### <a name="red-hat-enterprise-linux-centos-linux-and-oracle-linux-with-rhel-kernel"></a>Red Hat Enterprise Linux, CentOS Linux ve Oracle Linux (ile RHEL çekirdek)
- Yalnızca varsayılan ve SMP Linux çekirdek sürümleri desteklenir.
- PAE ve Xen, desteklenmez için tüm Linux dağıtım gibi standart olmayan çekirdek serbest bırakır. Örneğin, "2.6.16.21-0.8-xen" sürüm dizesi sistemiyle desteklenmiyor.
- Standart çekirdekleri yeniden derlemelerinin dahil olmak üzere özel çekirdekleri desteklenmez.
- CentOSPlus çekirdek desteklenmiyor.
- Oracle kesilemeyen kurumsal çekirdek (UEK), bu makalenin sonraki bölümlerde ele alınmıştır.


#### <a name="red-hat-linux-7"></a>Red Hat Linux 7
| OS sürümü | Çekirdek sürümü |
|:--|:--|
| 7.0 | 3.10.0-123 |
| 7.1 | 3.10.0-229 |
| 7.2 | 3.10.0-327 |
| 7.3 | 3.10.0-514 |
| 7.4 | 3.10.0-693 |

#### <a name="red-hat-linux-6"></a>Red Hat Linux 6
| OS sürümü | Çekirdek sürümü |
|:--|:--|
| 6.0 | 2.6.32-71 |
| 6.1 | 2.6.32-131 |
| 6.2 | 2.6.32-220 |
| 6.3 | 2.6.32-279 |
| 6.4 | 2.6.32-358 |
| 6.5 | 2.6.32-431 |
| 6.6 | 2.6.32-504 |
| 6.7 | 2.6.32-573 |
| 6.8 | 2.6.32-642 |
| 6.9 | 2.6.32-696 |

#### <a name="red-hat-linux-5"></a>Red Hat Linux 5
| OS sürümü | Çekirdek sürümü |
|:--|:--|
| 5.8 | 2.6.18-308 |
| 5.9 | 2.6.18-348 |
| 5.10 | 2.6.18-371 |
| 5.11 | 2.6.18-398<br>2.6.18-400<br>2.6.18-402<br>2.6.18-404<br>2.6.18-406<br>2.6.18-407<br>2.6.18-408<br>2.6.18-409<br>2.6.18-410<br>2.6.18-411<br>2.6.18-412<br>2.6.18-416<br>2.6.18-417<br>2.6.18-419<br>2.6.18-420 |

### <a name="ubuntu-server"></a>Ubuntu Server
- Standart çekirdekleri yeniden derlemelerinin dahil olmak üzere özel çekirdekleri desteklenmez.

| OS sürümü | Çekirdek sürümü |
|:--|:--|
| 16.04 | 4.4.\*<br>4.8.\*<br>4.10.\*<br>4.11.\*<br>4.13.\* |
| 14.04 | 3.13.\*<br>4.4.\* |

### <a name="oracle-enterprise-linux-with-unbreakable-enterprise-kernel"></a>Oracle Enterprise Linux kesilemeyen kurumsal çekirdek ile
#### <a name="oracle-linux-6"></a>Oracle Linux 6
| OS sürümü | Çekirdek sürümü
|:--|:--|
| 6.2 | Oracle 2.6.32-300 (UEK R1) |
| 6.3 | Oracle 2.6.39-200 (UEK R2) |
| 6.4 | Oracle 2.6.39-400 (UEK R2) |
| 6.5 | Oracle 2.6.39-400 (UEK R2 i386) |
| 6.6 | Oracle 2.6.39-400 (UEK R2 i386) |

#### <a name="oracle-linux-5"></a>Oracle Linux 5

| OS sürümü | Çekirdek sürümü
|:--|:--|
| 5.10 | Oracle 2.6.39-400 (UEK R2) |
| 5.11 | Oracle 2.6.39-400 (UEK R2) |

#### <a name="suse-linux-enterprise-server"></a>SUSE Linux Enterprise Server

#### <a name="suse-linux-11"></a>SUSE Linux 11
| OS sürümü | Çekirdek sürümü
|:--|:--|
| 11 SP2 | 3.0.101-0.7 |
| 11 SP3 | 3.0.101-0.47 |
| 11 SP4 | 3.0.101-65 |


## <a name="diagnostic-and-usage-data"></a>Tanılama ve kullanım verileri
Microsoft otomatik olarak hizmet Haritası hizmet kullanımınız vasıtasıyla kullanım ve performans verilerini toplar. Microsoft bu verileri sağlamak ve kalitesini, güvenlik ve hizmet eşlemesi hizmet bütünlüğünü geliştirmek için kullanır. Veriler, işletim sistemi ve sürümü gibi yazılımınızın yapılandırması hakkında bilgi içerir. Ayrıca IP adresi, DNS adı ve iş istasyonu adı doğru ve etkili sorun giderme özellikleri sunmak için içerir. Ad, adres veya diğer kişi bilgilerini toplamaz.

Veri toplama ve kullanım hakkında daha fazla bilgi için bkz: [Microsoft Online Services gizlilik bildirimi](https://go.microsoft.com/fwlink/?LinkId=512132).



## <a name="next-steps"></a>Sonraki adımlar
- Bilgi nasıl [hizmet eşlemesi kullanmak](operations-management-suite-service-map.md) sonra dağıtılan ve yapılandırılmış.
