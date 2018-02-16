---
title: "Azure günlük analizi Aracısı yönetme | Microsoft Docs"
description: "Bu makalede, Microsoft İzleme Aracısı (bir makinede dağıtılan MMA) yaşam döngüsü sırasında genellikle gerçekleştirecek farklı yönetim görevleri açıklar."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/09/2018
ms.author: magoedte
ms.openlocfilehash: 2e4daebf18d5edeba92bc14d5a4f699fbd2d94ce
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2018
---
# <a name="managing-and-maintaining-the-log-analytics-agent-for-windows-and-linux"></a>Yönetme ve Windows ve Linux için günlük analizi aracı Bakımı

Günlük analizi için Windows veya Linux Aracısı'nın ilk dağıtımdan sonra aracıyı yeniden yapılandırın veya kendi ömrü sona erme aşamasında ulaştıysa bilgisayardan kaldırmanız gerekebilir.  Bu bakım görevleri, el ile veya işlem hatası ve giderler azaltır Otomasyon aracılığıyla kolayca yönetebilirsiniz.

## <a name="adding-or-removing-a-workspace"></a>Ekleme veya bir çalışma alanı kaldırma 

### <a name="windows-agent"></a>Windows Aracısı

#### <a name="update-settings-from-control-panel"></a>Denetim Masası'ndan güncelleştirme ayarları

1. Yönetici haklarına sahip bir hesapla bilgisayarda oturum açma.
2. **Denetim Masası**'nı açın.
3. Seçin **Microsoft İzleme Aracısı** ve ardından **Azure günlük analizi (OMS)** sekmesi.
4. Bir çalışma alanı kaldırılırsa, onu seçin ve ardından **kaldırmak**. Raporlama durdurmak için aracı istediğiniz herhangi bir çalışma için bu adımı yineleyin.
5. Bir çalışma alanı ekleme tıklatmak **Ekle** ve **günlük analizi çalışma alanı Ekle** iletişim kutusu, Yapıştır çalışma alanı kimliği ve çalışma alanı anahtarı (birincil anahtar). Günlük analizi çalışma alanı Azure kamu bulutta bilgisayarın raporlama, Azure ABD devlet kurumları Azure bulut aşağı açılan listeden seçin. 
6. Değişikliklerinizi kaydetmek için **Tamam**’a tıklayın.

#### <a name="remove-a-workspace-using-powershell"></a>PowerShell kullanarak bir çalışma alanı Kaldır 

```PowerShell
$workspaceId = "<Your workspace Id>"
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.RemoveCloudWorkspace($workspaceId)
$mma.ReloadConfiguration()
```

#### <a name="add-a-workspace-in-azure-commercial-using-powershell"></a>Azure PowerShell kullanarak ticari bir çalışma alanı Ekle 

```PowerShell
$workspaceId = "<Your workspace Id>"
$workspaceKey = "<Your workspace Key>”
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.AddCloudWorkspace($workspaceId, $workspaceKey)
$mma.ReloadConfiguration()
```

#### <a name="add-a-workspace-in-azure-for-us-government-using-powershell"></a>ABD PowerShell kullanarak devlet kurumları için Azure'da bir çalışma alanı Ekle 

```PowerShell
$workspaceId = "<Your workspace Id>"
$workspaceKey = "<Your workspace Key>”
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.AddCloudWorkspace($workspaceId, $workspaceKey, 1)
$mma.ReloadConfiguration()
```

>[!NOTE]
>Komut satırı veya komut dosyasını önceden yüklemek veya aracısını yapılandırmak için kullandığınız varsa `EnableAzureOperationalInsights` tarafından değiştirildi `AddCloudWorkspace` ve `RemoveCloudWorkspace`.
>

## <a name="update-proxy-settings"></a>Proxy ayarlarını güncelleştirme 
Hizmet bir proxy sunucu üzerinden iletişim kurmak için aracıyı yapılandırmak için veya [OMS ağ geçidi](log-analytics-oms-gateway.md) dağıtıldıktan sonra aşağıdaki yöntemlerden birini bu görevi tamamlamak için kullanın.

### <a name="windows-agent"></a>Windows Aracısı

#### <a name="update-settings-using-control-panel"></a>Denetim Masası'nı kullanarak güncelleştirme ayarları

1. Yönetici haklarına sahip bir hesapla bilgisayarda oturum açma.
2. **Denetim Masası**'nı açın.
3. Seçin **Microsoft İzleme Aracısı** ve ardından **Proxy ayarlarını** sekmesi.
4. Tıklatın **bir proxy sunucu kullanacak** URL'sini sağlayın ve bağlantı noktası proxy sunucusu veya ağ geçidi sayısı. Proxy sunucusu veya OMS Ağ Geçidi kimlik doğrulaması gerektiriyorsa, kullanıcı adı ve parola kimlik doğrulaması ve ardından yazın **Tamam**. 

#### <a name="update-settings-using-powershell"></a>PowerShell kullanarak güncelleştirme ayarları 

Aşağıdaki örnek PowerShell kodu kopyalayıp, ortamınıza özgü bilgilerle güncelleştirmek ve bir PS1 dosya adı uzantısıyla kaydedin.  Günlük analizi hizmetine doğrudan bağlanan her bilgisayarda komut dosyasını çalıştırın.

```PowerShell
param($ProxyDomainName="https://proxy.contoso.com:30443", $cred=(Get-Credential))

# First we get the Health Service configuration object.  We need to determine if we
#have the right update rollup with the API we need.  If not, no need to run the rest of the script.
$healthServiceSettings = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'

$proxyMethod = $healthServiceSettings | Get-Member -Name 'SetProxyInfo'

if (!$proxyMethod)
{
     Write-Output 'Health Service proxy API not present, will not update settings.'
     return
}

Write-Output "Clearing proxy settings."
$healthServiceSettings.SetProxyInfo('', '', '')

$ProxyUserName = $cred.username

Write-Output "Setting proxy to $ProxyDomainName with proxy username $ProxyUserName."
$healthServiceSettings.SetProxyInfo($ProxyDomainName, $ProxyUserName, $cred.GetNetworkCredential().password)
```  

### <a name="linux-agent"></a>Linux Aracısı
Bir proxy sunucusu veya günlük analizi için OMS ağ geçidi üzerinden iletişim kurmak Linux bilgisayarlarınızı ihtiyacınız varsa aşağıdaki adımları gerçekleştirin.  Proxy yapılandırması değeri `[protocol://][user:password@]proxyhost[:port]` sözdizimine sahiptir.  *proxyhost* özelliği, ara sunucunun tam etki adı alanı veya IP adresini kabul eder.

1. Aşağıdaki komutları çalıştırıp değerleri kendi ayarlarınıza göre değiştirerek `/etc/opt/microsoft/omsagent/proxy.conf` dosyasını düzenleyin.

    ```
    proxyconf="https://proxyuser:proxypassword@proxyserver01:30443"
    sudo echo $proxyconf >>/etc/opt/microsoft/omsagent/proxy.conf
    sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/proxy.conf 
    ```

2. Aşağıdaki komutu çalıştırarak aracıyı yeniden başlatın: 

    ```
    sudo /opt/microsoft/omsagent/bin/service_control restart [<workspace id>]
    ``` 

## <a name="uninstall-agent"></a>Aracıyı kaldır
Komut satırı veya Kurulum Sihirbazı'nı kullanarak Windows veya Linux aracısını kaldırmak için aşağıdaki yordamlardan birini kullanın.

### <a name="windows-agent"></a>Windows Aracısı

#### <a name="uninstall-from-control-panel"></a>Denetim Masası'ndan kaldırma
1. Yönetici haklarına sahip bir hesapla bilgisayarda oturum açma.  
2. İçinde **Denetim Masası**, tıklatın **programlar ve Özellikler**.
3. İçinde **programlar ve Özellikler**, tıklatın **Microsoft İzleme Aracısı**, tıklatın **kaldırma**ve ardından **Evet**.

>[!NOTE]
>Aracı Kurulum Sihirbazı'nı çift tıklatarak da çalıştırılabilir **MMASetup -<platform>.exe**, Azure portalında bir çalışma alanından indirme için kullanılabilir olduğu.

#### <a name="uninstall-from-the-command-line"></a>Komut satırından kaldırma
Aracı için indirilen dosya ile IExpress oluşturulan müstakil yükleme paketidir.  Destekleyici dosyaları ve aracısı için Kurulum programını pakette yer alan ve düzgün bir şekilde aşağıdaki örnekte gösterilen komut satırını kullanarak kaldırmak için ayıklanacak gerekir. 

1. Yönetici haklarına sahip bir hesapla bilgisayarda oturum açma.  
2. Aracı yükleme dosyalarını çalıştırmak yükseltilmiş komut isteminden ayıklamak için `extract MMASetup-<platform>.exe` ve bu dosyaları ayıklamak yol için ister.  Bağımsız değişkenler geçirerek yolunu alternatif olarak, belirtebilirsiniz `extract MMASetup-<platform>.exe /c:<Path> /t:<Path>`.  IExpress tarafından desteklenen komut satırı swtiches hakkında daha fazla bilgi için bkz: [IExpress komut satırı anahtarları](https://support.microsoft.com/help/197147/command-line-switches-for-iexpress-software-update-packages) ve örnek gereksinimlerinize uyacak şekilde güncelleştirin.
3. İsteminde `%WinDir%\System32\msiexec.exe /x <Path>:\MOMAgent.msi /qb`.  

### <a name="linux-agent"></a>Linux Aracısı
Aracıyı kaldırmak için Linux bilgisayarda aşağıdaki komutu çalıştırın.  *--Temizleme* bağımsız değişkeni tamamen kaldırır aracı ve yapılandırması.

   `wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh --purge`

## <a name="configure-agent-to-report-to-an-operations-manager-management-group"></a>Bir Operations Manager yönetim grubuna bildirmeye Aracısı'nı yapılandırma

### <a name="windows-agent"></a>Windows Aracısı
OMS aracısı için bir System Center Operations Manager yönetim grubuna raporlamak için Windows yapılandırmak için aşağıdaki adımları gerçekleştirin. 

1. Yönetici haklarına sahip bir hesapla bilgisayarda oturum açma.
2. **Denetim Masası**'nı açın. 
3. Tıklatın **Microsoft İzleme Aracısı** ve ardından **Operations Manager** sekmesi.
4. Operations Manager sunucularınızın Active Directory ile tümleştirme varsa, tıklatın **yönetim grubu atamalarını AD DS'den otomatik olarak güncelleştir**.
5. Tıklatın **Ekle** açmak için **bir yönetim grubu Ekle** iletişim kutusu.
6. İçinde **yönetim grubu adı** alanında, yönetim grubunuzun adını yazın.
7. İçinde **birincil yönetim sunucusu** alanında, birincil yönetim sunucusunun bilgisayar adını yazın.
8. İçinde **yönetim sunucusu bağlantı noktası** alan, TCP bağlantı noktası numarasını yazın.
9. Altında **aracı eylem hesabı**, yerel sistem hesabı veya bir yerel etki alanı hesabı seçin.
10. Tıklatın **Tamam** kapatmak için **bir yönetim grubu Ekle** iletişim kutusunu ve ardından **Tamam** kapatmak için **Microsoft Monitoring Agent özellikleri** iletişim kutusu.

### <a name="linux-agent"></a>Linux Aracısı
System Center Operations Manager yönetim grubu için rapor Linux için OMS aracısının yapılandırmak için aşağıdaki adımları gerçekleştirin. 

1. Dosyayı düzenleyin. `/etc/opt/omi/conf/omiserver.conf`
2. Satır başlayarak emin `httpsport=` bağlantı noktası 1270 tanımlar. Örneğin: `httpsport=1270`
3. OMI sunucuyu yeniden başlatın: `sudo /opt/omi/bin/service_control restart`

## <a name="next-steps"></a>Sonraki adımlar

Gözden geçirme [Linux Aracısı'nı sorun giderme](log-analytics-agent-linux-support.md) yükleme veya aracı yönetme sırasında sorunlarla karşılaşırsanız.  