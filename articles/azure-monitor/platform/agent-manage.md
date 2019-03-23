---
title: Azure Log Analytics Aracısı'nı yönetme | Microsoft Docs
description: Bu makalede, Microsoft Monitoring Agent (bir makinede dağıtılan MMA) yaşam döngüsü boyunca genellikle gerçekleştirecek farklı yönetim görevleri açıklanır.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 03/30/2018
ms.author: magoedte
ms.openlocfilehash: 963fd1bfd67a20033f0712d3b447091abda40d11
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58369914"
---
# <a name="managing-and-maintaining-the-log-analytics-agent-for-windows-and-linux"></a>Windows ve Linux için Log Analytics aracısını korumak ve yönetme

Log Analytics Windows veya Linux Aracısı Azure İzleyici'de, ilk dağıtımdan sonra aracıyı yeniden yapılandırın veya bilgisayarın yaşam döngüsü emeklilik aşamasında ulaştıysa kaldırmanız gerekebilir. Bu bakım görevleri el ile veya işlem hatası hem giderlerini azaltan ve Otomasyon aracılığıyla kolayca yönetebilirsiniz.

## <a name="adding-or-removing-a-workspace"></a>Ekleyerek veya kaldırarak bir çalışma alanı

### <a name="windows-agent"></a>Windows Aracısı

#### <a name="update-settings-from-control-panel"></a>Denetim Masası'ndan ayarlarını güncelleştirme

1. Yönetici haklarına sahip bir hesapla bilgisayarda oturum açın.
2. **Denetim Masası**'nı açın.
3. Seçin **Microsoft Monitoring Agent** ve ardından **Azure Log Analytics** sekmesi.
4. Bir çalışma alanı kaldırılıyor, onu seçin ve ardından **Kaldır**. Aracı için raporlamayı sonlandırmak istediğiniz herhangi bir çalışma için bu adımı yineleyin.
5. Bir çalışma alanı ekleme, tıklayın **Ekle** ve **bir Log Analytics çalışma alanı Ekle** iletişim kutusu, yapıştırma çalışma alanı kimliği ve çalışma alanı anahtarı (birincil anahtar). Bilgisayarın Azure kamu bulutundaki bir Log Analytics çalışma alanına raporlaması gerekiyorsa, Azure ABD devlet kurumları Azure bulut aşağı açılan listeden seçin.
6. Değişikliklerinizi kaydetmek için **Tamam**’a tıklayın.

#### <a name="remove-a-workspace-using-powershell"></a>PowerShell kullanarak bir çalışma alanını Kaldır

```PowerShell
$workspaceId = "<Your workspace Id>"
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.RemoveCloudWorkspace($workspaceId)
$mma.ReloadConfiguration()
```

#### <a name="add-a-workspace-in-azure-commercial-using-powershell"></a>Azure PowerShell kullanarak ticari bir çalışma alanı Ekle

```PowerShell
$workspaceId = "<Your workspace Id>"
$workspaceKey = "<Your workspace Key>"
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.AddCloudWorkspace($workspaceId, $workspaceKey)
$mma.ReloadConfiguration()
```

#### <a name="add-a-workspace-in-azure-for-us-government-using-powershell"></a>PowerShell kullanarak ABD için Azure'a bir çalışma alanı ekleyin

```PowerShell
$workspaceId = "<Your workspace Id>"
$workspaceKey = "<Your workspace Key>"
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.AddCloudWorkspace($workspaceId, $workspaceKey, 1)
$mma.ReloadConfiguration()
```

>[!NOTE]
>Komut satırından veya betik önceden yükleyin veya aracıyı yapılandırmak için kullandığınız, `EnableAzureOperationalInsights` tarafından değiştirildi `AddCloudWorkspace` ve `RemoveCloudWorkspace`.
>

### <a name="linux-agent"></a>Linux Aracısı
Aşağıdaki adımları, farklı bir çalışma alanı ile kaydolun veya bir çalışma alanı yapılandırmasını kaldırmak istediğiniz karar verirseniz, Linux Aracısı yapılandırılacağını göstermektedir.

1. Bir çalışma alanına kaydedilir doğrulamak için aşağıdaki komutu çalıştırın:

    `/opt/microsoft/omsagent/bin/omsadmin.sh -l`

    Bir durum aşağıdaki örneğe benzer döndürmesi gerekir:

    `Primary Workspace: <workspaceId>   Status: Onboarded(OMSAgent Running)`

    Durumu da gösterilir, aracıyı çalıştıran, aksi takdirde aracıyı yeniden yapılandırmak için aşağıdaki adımları başarıyla tamamlanmayacaktır önemlidir.

2. Bir çalışma alanıyla zaten kayıtlı değilse, aşağıdaki komutu çalıştırarak kayıtlı çalışma alanını kaldırın. Aksi takdirde, kayıtlı değilse, sonraki adıma geçin.

    `/opt/microsoft/omsagent/bin/omsadmin.sh -X`

3. Farklı bir çalışma alanı ile kaydetmek için aşağıdaki komutu çalıştırın:

    `/opt/microsoft/omsagent/bin/omsadmin.sh -w <workspace id> -s <shared key> [-d <top level domain>]`
    
4. Değişikliklerinizi etkileyen Süren doğrulamak için aşağıdaki komutu çalıştırın:

    `/opt/microsoft/omsagent/bin/omsadmin.sh -l`

    Bir durum aşağıdaki örneğe benzer döndürmesi gerekir:

    `Primary Workspace: <workspaceId>   Status: Onboarded(OMSAgent Running)`

Aracı hizmeti değişikliklerin etkili olması yeniden başlatılması gerekmez.

## <a name="update-proxy-settings"></a>Proxy ayarlarını güncelleştirme
Hizmet bir ara sunucu üzerinden iletişim kurmak için aracıyı yapılandırmak için veya [Log Analytics gateway](gateway.md) dağıtımdan sonra aşağıdaki yöntemlerden birini bu görevi tamamlamak için kullanın.

### <a name="windows-agent"></a>Windows Aracısı

#### <a name="update-settings-using-control-panel"></a>Denetim Masası'nı kullanarak ayarlarını güncelleştirme

1. Yönetici haklarına sahip bir hesapla bilgisayarda oturum açın.
2. **Denetim Masası**'nı açın.
3. Seçin **Microsoft Monitoring Agent** ve ardından **Proxy ayarlarını** sekmesi.
4. Tıklayın **proxy sunucusu kullan** URL'sini sağlayın ve bağlantı noktası proxy sunucusu veya ağ geçidi sayısı. Proxy sunucusu veya Log Analytics Ağ Geçidi kimlik doğrulaması gerektiriyorsa, kullanıcı adı ve parola kimlik doğrulaması ve ardından yazın **Tamam**.

#### <a name="update-settings-using-powershell"></a>PowerShell kullanarak ayarlarını güncelleştirme

Aşağıdaki örnek PowerShell kodu kopyalayın, ortamınıza özgü bilgilerle güncelleştirin ve bir PS1 dosya adı uzantısıyla kaydedin. Log Analytics çalışma alanını Azure İzleyici'de doğrudan bağlanan her bilgisayarda betiği çalıştırın.

```PowerShell
param($ProxyDomainName="https://proxy.contoso.com:30443", $cred=(Get-Credential))

# First we get the Health Service configuration object. We need to determine if we
#have the right update rollup with the API we need. If not, no need to run the rest of the script.
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
Azure'da Linux bilgisayarlarını bir proxy sunucusu veya Log Analytics ağ geçidi iletişim kurması gerekiyorsa aşağıdaki adımları gerçekleştirin. Proxy yapılandırması değeri `[protocol://][user:password@]proxyhost[:port]` sözdizimine sahiptir. *proxyhost* özelliği, ara sunucunun tam etki adı alanı veya IP adresini kabul eder.

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
Komut satırı veya Kurulum Sihirbazı'nı kullanarak Windows veya Linux aracısını kaldırmak için aşağıdaki prosedürlerden birini kullanın.

### <a name="windows-agent"></a>Windows Aracısı

#### <a name="uninstall-from-control-panel"></a>Denetim Masası'ndan kaldırma
1. Yönetici haklarına sahip bir hesapla bilgisayarda oturum açın.
2. İçinde **Denetim Masası**, tıklayın **programlar ve Özellikler**.
3. İçinde **programlar ve Özellikler**, tıklayın **Microsoft Monitoring Agent**, tıklayın **kaldırma**ve ardından **Evet**.

>[!NOTE]
>Aracı Kurulum Sihirbazı'nı çift tıklatılarak da çalıştırılabilir **MMASetup -\<platform\>.exe**, Azure portalında bir çalışma alanından indirilebilir olduğu.

#### <a name="uninstall-from-the-command-line"></a>Komut satırından kaldırın
Aracı için indirilen dosyayı, IExpress ile oluşturulan kendi içinde yükleme paketidir. Destekleyici dosyaları ve aracı için Kurulum programı, pakette yer alan ve doğru bir şekilde aşağıdaki örnekte gösterilen komut satırını kullanarak kaldırmak için ayıklanacak gerekir.

1. Yönetici haklarına sahip bir hesapla bilgisayarda oturum açın.
2. Çalıştırma yükseltilmiş komut isteminden aracı yükleme dosyalarını ayıklamak için `extract MMASetup-<platform>.exe` ve bu dosyaları ayıklamak yol, ister. Alternatif olarak, bağımsız değişkenleri geçirme olan yolu belirtebilirsiniz `extract MMASetup-<platform>.exe /c:<Path> /t:<Path>`. IExpress tarafından desteklenen komut satırı anahtarları hakkında daha fazla bilgi için bkz. [IExpress için komut satırı anahtarları](https://support.microsoft.com/help/197147/command-line-switches-for-iexpress-software-update-packages) ve sonra örneği gereksinimlerinize uyacak şekilde güncelleştirin.
3. İsteminde `%WinDir%\System32\msiexec.exe /x <Path>:\MOMAgent.msi /qb`.

### <a name="linux-agent"></a>Linux Aracısı
Aracıyı kaldırmak için Linux bilgisayarında aşağıdaki komutu çalıştırın. *--temizleme* bağımsız değişkeni, aracıyı ve yapılandırmasını tamamen kaldırır.

   `wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh --purge`

## <a name="configure-agent-to-report-to-an-operations-manager-management-group"></a>Bir aracıyı Operations Manager yönetim grubuna raporlama yapacak yapılandırın

### <a name="windows-agent"></a>Windows Aracısı
Bir System Center Operations Manager yönetim grubuna rapor Windows için Log Analytics aracısını yapılandırmak için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

1. Yönetici haklarına sahip bir hesapla bilgisayarda oturum açın.
2. **Denetim Masası**'nı açın.
3. Tıklayın **Microsoft Monitoring Agent** ve ardından **Operations Manager** sekmesi.
4. Active Directory ile tümleştirme Operations Manager sunucularınız varsa tıklayın **yönetim grubu atamalarını AD DS'den otomatik olarak güncelleştir**.
5. Tıklayın **Ekle** açmak için **Yönetim Grubu Ekle** iletişim kutusu.
6. İçinde **yönetim grubu adı** alanında, yönetim grubunuzun adını yazın.
7. İçinde **birincil yönetim sunucusu** alanında, birincil yönetim sunucusunun bilgisayar adını yazın.
8. İçinde **yönetim sunucusu bağlantı noktası** alanında, TCP bağlantı noktası numarasını yazın.
9. Altında **aracı eylem hesabı**, yerel sistem hesabını veya bir yerel etki alanı hesabı seçin.
10. Tıklayın **Tamam** kapatmak için **Yönetim Grubu Ekle** iletişim kutusunu ve ardından **Tamam** kapatmak için **Microsoft Monitoring Agent özellikleri** iletişim kutusu.

### <a name="linux-agent"></a>Linux Aracısı
Bir System Center Operations Manager yönetim grubuna rapor için Linux için Log Analytics aracısını yapılandırmak için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

1. Dosyayı Düzenle `/etc/opt/omi/conf/omiserver.conf`
2. İle satır başına emin `httpsport=` bağlantı noktası 1270 tanımlar. Örneğin: `httpsport=1270`
3. OMI sunucuyu yeniden başlatın: `sudo /opt/omi/bin/service_control restart`

## <a name="next-steps"></a>Sonraki adımlar

Gözden geçirme [Linux Aracısı sorunlarını giderme](agent-linux-troubleshoot.md) yükleme veya aracıyı yönetme sorunlarla karşılaşırsanız.
