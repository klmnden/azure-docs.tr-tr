---
title: "Windows bilgisayarları Azure günlük Analizi'ne bağlamak | Microsoft Docs"
description: "Bu makalede, özelleştirilmiş bir sürümünü Microsoft İzleme Aracısı'nı (MMA) kullanarak şirket içi altyapınızda Windows bilgisayarları günlük analizi hizmetine bağlanmak için gereken adımları gösterir."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 932f7b8c-485c-40c1-98e3-7d4c560876d2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/03/2017
ms.author: magoedte
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e5f04f3b9135167c0f339c58323ebd931b260109
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="connect-windows-computers-to-the-log-analytics-service-in-azure"></a>Windows bilgisayarları Azure günlük analizi hizmetine bağlanın

Bu makalede Windows bilgisayarları şirket içi altyapınızda OMS çalışma alanları için özelleştirilmiş bir sürümünü Microsoft İzleme Aracısı'nı (MMA) kullanarak bağlanmak için gereken adımları gösterir. Yükleme ve yerleşik sırayla veri göndermek için günlük analizi hizmeti görüntülemek ve bu veriler üzerinde hareket için bunları istediğiniz tüm bilgisayarların için aracıları bağlanmak gerekir. Her bir aracı birden çok çalışma alanına bildirebilirsiniz.

Kurulum, komut satırı kullanılarak aracıları yükleyebilirsiniz veya ile istenen durum yapılandırması (DSC) Azure Automation.  

>[!NOTE]
Azure'da çalışan sanal makineler için yükleme kullanarak basitleştirebilirsiniz [sanal makine uzantısı](log-analytics-azure-vm-extension.md).

Internet bağlantısı olan bilgisayarlarda aracı için OMS veri göndermek için Internet bağlantısı kullanır. Internet bağlantısına sahip olmayan bilgisayarlar için bir proxy sunucu kullanabilirsiniz veya [OMS ağ geçidi](log-analytics-oms-gateway.md).

Windows bilgisayarları için OMS bağlanma üç basit adımları kullanarak basittir:

1. OMS Portalı'ndan aracı kurulum dosyasını indirin
2. Seçtiğiniz yöntemi kullanarak aracı yükleme
3. Aracı yapılandırma veya gerekiyorsa, ek çalışma alanları ekleme

Yüklü ve aracıları yapılandırdıktan sonra aşağıdaki diyagramda, Windows bilgisayarları ve OMS arasındaki ilişkiyi gösterir.

![OMS doğrudan Aracısı diyagramı](./media/log-analytics-windows-agents/oms-direct-agent-diagram.png)

BT güvenlik ilkelerinizi bilgisayarları Internet'e bağlanmak için ağınızdaki izin vermiyorsa, OMS ağ geçidine bağlanmak üzere, bilgisayarlarınız yapılandırabilirsiniz. Daha fazla bilgi ve sunucularınızı OMS hizmetine bir OMS ağ geçidi üzerinden iletişim kurmak için yapılandırma adımları için bkz: [OMS ağ geçidini kullanarak OMS bilgisayarları bağlamak](log-analytics-oms-gateway.md).

## <a name="system-requirements-and-required-configuration"></a>Sistem gereksinimleri ve gerekli yapılandırma
Yükleyip aracıları dağıtmak için önce belirtilen gereksinimleri karşıladığından emin olmak için aşağıdaki ayrıntıları gözden geçirin.

- Yalnızca Windows Server 2008 SP 1 veya sonrasını çalıştıran bilgisayarlar veya Windows 7 SP1 OMS MMA yükleyebilirsiniz ya da daha sonra.
- Bir Azure aboneliği gerekir.  Daha fazla bilgi için bkz: [günlük Analytics ile çalışmaya başlama](log-analytics-get-started.md).
- Her Windows bilgisayarı HTTPS kullanarak Internet'e veya OMS ağ geçidi bağlanabilmesi gerekir. Bu bağlantıyı doğrudan OMS ağ geçidi veya proxy aracılığıyla olabilir.
- Tek başına bilgisayarların, sunucuların ve sanal makinelerin OMS MMA yükleyebilirsiniz. İçin OMS Azure barındırılan sanal makinelere bağlanmak istiyorsanız, bkz: [bağlanmak Azure sanal makineleri için günlük analizi](log-analytics-azure-vm-extension.md).
- Aracı çeşitli kaynaklar için TCP bağlantı noktası 443 kullanması gerekir.

### <a name="network"></a>Ağ

Windows aracılarının bağlanmak ve OMS hizmete kaydolmak etki alanı URL'lerin ve bağlantı noktası numaraları gibi ağ kaynaklarına erişimi gerekir.

- Proxy sunucuları için, aracı ayarlarında uygun proxy sunucusu kaynaklarının yapılandırıldığından emin olmanız gerekir.
- Internet'e erişimi kısıtlamak, güvenlik duvarları için siz veya ağ mühendisleri için OMS erişime izin vermek için güvenlik duvarını yapılandırmanız gerekir. Aracı ayarlarında bir işlem yapılması gerekmez.

Aşağıdaki tabloda iletişim için gereken kaynaklar gösterilmektedir.

>[!NOTE]
>Aşağıdaki kaynaklardan bazıları, operasyonel Öngörüler, günlük analizi için önceki bir ad olduğu gerekeceğinden buraya.

| Aracı Kaynağı | Bağlantı Noktaları | HTTPS denetlemesini atlama |
|---|---|---|
| *.ods.opinsights.azure.com | 443 | Evet |
| *.oms.opinsights.azure.com | 443 | Evet |
| *.blob.core.windows.net | 443 | Evet |
| *.azure-automation.net | 443 | Evet |



## <a name="download-the-agent-setup-file-from-oms"></a>OMS Aracısı kurulum dosyasını indirin
1. İçinde [OMS portalı](https://www.mms.microsoft.com), **genel bakış** sayfasında, **ayarları** döşeme.  Tıklatın **bağlı kaynakları** üst sekmesini.  
    ![Bağlı kaynaklar sekmesi](./media/log-analytics-windows-agents/oms-direct-agent-connected-sources.png)
2. Tıklatın **Windows sunucuları** ve ardından **Windows Aracısı indirme** kurulum dosyasını karşıdan yüklemek için bilgisayarınızın işlemci türü için uygulanabilir.
3. Sağ tarafındaki **çalışma alanı kimliği**Kopyala simgesine tıklayın ve kodu Not Defteri'ne yapıştırın.
4. Sağ tarafındaki **birincil anahtar**Kopyala simgesine ve anahtarı Not Defteri'ne yapıştırın.     

## <a name="install-the-agent-using-setup"></a>Kurulumu kullanarak aracı yükleme
1. Yönetmek istediğiniz bir bilgisayara aracı yüklemek için Kurulumu çalıştırın.
2. Hoş Geldiniz sayfasında, tıklatın **sonraki**.
3. Lisans koşulları sayfasında, lisans okuyun ve ardından **ediyorum**.
4. Hedef klasör sayfasında, değiştirmek veya varsayılan yükleme klasörünü ve ardından **sonraki**.
5. Aracı Kur seçenekleri sayfasında Azure günlük analizi (OMS için), Operations Manager Aracısı bağlanmayı seçebilirsiniz veya aracı daha sonra yapılandırmak istiyorsanız seçimleri boş bırakabilirsiniz. **İleri**’ye tıklayın.   
    - Azure günlük analizi (OMS) bağlanmak seçerseniz, yapıştırma **çalışma alanı kimliği** ve **çalışma alanı anahtarı (birincil anahtar)** önceki yordamda Not Defteri'ne kopyalanan ve ardından **sonraki** .  
        ![Çalışma alanı kimliği ve birincil anahtar yapıştırın](./media/log-analytics-windows-agents/connect-workspace.png)
    - Operations Manager'e Bağla seçerseniz, yazın **yönetim grubu adı**, **Management Server** adı ve **yönetim sunucusu bağlantı noktası**ve ardından **Sonraki**. Aracı eylem hesabı sayfasında, yerel sistem hesabı veya bir yerel etki alanı hesabı seçin ve ardından **sonraki**.  
        ![Yönetim grubu Yapılandırması](./media/log-analytics-windows-agents/oms-mma-om-setup01.png)![aracı eylem hesabı](./media/log-analytics-windows-agents/oms-mma-om-setup02.png)

6. Hazır sayfasında Yükle seçimlerinizi gözden geçirin ve ardından **yükleme**.
7. Yapılandırması başarıyla tamamlandı sayfasında, **son**.
8. Tamamlandığında, **Microsoft İzleme Aracısı** görünür **Denetim Masası**. Vardır, yapılandırmanızı gözden geçirin ve aracı Operational Insights (OMS için) bağlı olduğunu doğrulayın. Bildiren bir ileti aracısı için OMS bağlıyken görüntüler: **Microsoft Monitoring Agent Microsoft Operations Management Suite hizmetine başarıyla bağlandı.**

## <a name="configure-proxy-settings"></a>Proxy ayarlarını yapılandır

Denetim Masası'nı kullanarak Microsoft İzleme Aracısı'na ilişkin ara sunucu ayarlarını yapılandırmak için aşağıdaki yordamı kullanabilirsiniz. Her sunucu için bu yordamı kullanmanız gerekir. Yapılandırmanız gereken birden çok sunucu olması durumunda, bu işlemi otomatikleştirmek için bir betik kullanmak sizin için daha kolay olabilir. Bu durumda, bir sonraki [Microsoft İzleme Aracısı için ara sunucu ayarlarını betik kullanarak yapılandırma](#to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script) yordamına bakabilirsiniz.

### <a name="to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-control-panel"></a>Denetim Masası'nı kullanarak Microsoft İzleme Aracısı için ara sunucu ayarlarını yapılandırma
1. **Denetim Masası**'nı açın.
2. **Microsoft İzleme Aracısı**'nı açın.
3. **Ara Sunucu Ayarları** sekmesine tıklayın.  
    ![ara sunucu ayarları sekmesi](./media/log-analytics-windows-agents/proxy-direct-agent-proxy.png)
4. **Ara sunucu kullan**'ı seçin ve gerekiyorsa gösterilen örneğe benzer şekilde URL ile bağlantı noktasını yazın. Ara sunucunuz kimlik doğrulaması gerektiriyorsa ara sunucuya erişmek için kullanıcı adını ve parolayı yazın.


### <a name="verify-agent-connectivity-to-oms"></a>OMS Aracısı bağlanabildiğini doğrulayın

Kolayca aracılarınızı aşağıdaki yordamı kullanarak OMS ile iletişim kuran olup olmadığını doğrulayın:

1.  Windows aracısı içeren bir bilgisayarda, Denetim Masası açın.
2.  Microsoft Monitoring Agent'ı açın.
3.  Azure günlük analizi (OMS) sekmesine tıklayın.
4.  Durum sütununda aracı Operations Management Suite hizmetine başarıyla bağlandı görmeniz gerekir.

![bağlı aracı](./media/log-analytics-windows-agents/mma-connected.png)


### <a name="to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script"></a>Betik kullanarak Microsoft İzleme Aracısı için ara sunucu ayarlarını yapılandırma
Aşağıdaki örneği kopyalayın, ortamınıza özgü bilgilerle güncelleştirin, bir PS1 dosya adı uzantısıyla kaydedin ve ardından betiği doğrudan OMS hizmetine bağlanan her bilgisayardan çalıştırın.

    param($ProxyDomainName="http://proxy.contoso.com:80", $cred=(Get-Credential))

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



## <a name="install-the-agent-using-the-command-line"></a>Komut satırını kullanarak aracı yükleme
- Değiştirin ve ardından komut satırını kullanarak aracı yüklemek için aşağıdaki örneği kullanın. Örneğin, tam olarak sessiz yükleme gerçekleştirir.

    >[!NOTE]
    Bir aracıyı yükseltmek istiyorsanız, betik API'si günlük analizi kullanmanız gerekir. Bir aracıyı yükseltmek için sonraki bölüme bakın.

    ```dos
    MMASetup-AMD64.exe /Q:A /R:N /C:"setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=<your workspace id> OPINSIGHTS_WORKSPACE_KEY=<your workspace key> AcceptEndUserLicenseAgreement=1"
    ```

Aracının IExpress kullanarak kendi ayıklayıcısı kullandığı `/c` komutu. Komut satırı anahtarları adresindeki görebilirsiniz [IExpress komut satırı anahtarları](https://support.microsoft.com/help/197147/command-line-switches-for-iexpress-software-update-packages) ve örnek gereksinimlerinize uyacak şekilde güncelleştirin.

|MMA özgü seçenekleri                   |Notlar         |
|---------------------------------------|--------------|
|ADD_OPINSIGHTS_WORKSPACE               | 1 = çalışma alanına bildirmeye Aracısı Yapılandırma                |
|OPINSIGHTS_WORKSPACE_ID                | Eklemek için çalışma alanı kimliği (GUID) çalışma alanı için                    |
|OPINSIGHTS_WORKSPACE_KEY               | Başlangıçta çalışma alanıyla kimliğini doğrulamak için kullanılan çalışma alanı anahtarı |
|OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE  | Çalışma alanı bulunduğu bulut ortamını belirtin <br> 0 = azure ticari bulut (varsayılan) <br> 1 azure kamu = |
|OPINSIGHTS_PROXY_URL               | Kullanılacak proxy için URI |
|OPINSIGHTS_PROXY_USERNAME               | Kimliği doğrulanmış bir proxy sunucusuna erişmek için kullanıcı adı |
|OPINSIGHTS_PROXY_PASSWORD               | Kimliği doğrulanmış bir proxy sunucusuna erişmek için parola |

>[!NOTE]
IExpress komut satırı uzunluğunu basarsa önlemek için yapılandırılmış hiçbir çalışma ile aracıyı yükleyin ve çalışma alanı için yapılandırmayı ayarlamak için bir komut dosyası kullanın.

>[!NOTE]
Alırsanız bir `Command line option syntax error.` kullanırken `OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE` parametresi, aşağıdaki geçici çözüm kullanabilirsiniz:
```dos
MMASetup-AMD64.exe /C /T:.\MMAExtract
cd .\MMAExtract
setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE=1 OPINSIGHTS_WORKSPACE_ID=<your workspace id> OPINSIGHTS_WORKSPACE_KEY=<your workspace key> AcceptEndUserLicenseAgreement=1
```

## <a name="add-a-workspace-using-a-script"></a>Bir komut dosyası kullanarak bir çalışma alanı Ekle
Aşağıdaki örnek ile günlük analizi aracı komut dosyası API kullanarak bir çalışma alanı ekleyin:

```PowerShell
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.AddCloudWorkspace($workspaceId, $workspaceKey)
$mma.ReloadConfiguration()
```

Bir çalışma alanı Azure ABD devlet kurumları için eklemek için aşağıdaki komut dosyası örneği kullanın:
```PowerShell
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.AddCloudWorkspace($workspaceId, $workspaceKey, 1)
$mma.ReloadConfiguration()
```

>[!NOTE]
Komut satırı veya komut dosyasını önceden yüklemek veya aracısını yapılandırmak için kullandığınız varsa `EnableAzureOperationalInsights` tarafından değiştirildi `AddCloudWorkspace`.

## <a name="install-the-agent-using-dsc-in-azure-automation"></a>Azure Otomasyonu'nda DSC kullanarak aracı yükleme

Azure Otomasyonu'nda DSC kullanarak aracıyı yüklemek için aşağıdaki komut dosyası örneği kullanabilirsiniz. Örnek tarafından tanımlanan 64-bit aracı yükler `URI` değeri. URI değerini değiştirerek 32-bit sürümünü de kullanabilirsiniz. URI'ler için her iki sürümü vardır:

- Windows 64-bit Aracısı - https://go.microsoft.com/fwlink/?LinkId=828603
- Windows 32-bit Aracısı - https://go.microsoft.com/fwlink/?LinkId=828604


>[!NOTE]
Bu yordam ve komut dosyası örneği var olan bir aracıyı yükseltmez.

1. İçeri aktarma xPSDesiredStateConfiguration DSC modülünden [http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration](http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration) Azure Otomasyonu içine.  
2.  İçin Azure Otomasyonu değişken varlıkları oluşturma *OPSINSIGHTS_WS_ID* ve *OPSINSIGHTS_WS_KEY*. Ayarlama *OPSINSIGHTS_WS_ID* OMS günlük analizi çalışma alanı kimliği ve kümesi *OPSINSIGHTS_WS_KEY* çalışma alanınızın birincil anahtarı.
3.  Aşağıdaki komut dosyasını kullanın ve MMAgent.ps1 Kaydet
4.  Değiştirin ve ardından Azure Otomasyonu'nda DSC kullanarak aracıyı yüklemek için aşağıdaki örneği kullanın. MMAgent.ps1 Azure Otomasyon arabirimi veya cmdlet'ini kullanarak Azure Automation'a aktarabilir.
5.  Bir düğüm yapılandırması atayın. 15 dakika içinde düğüm yapılandırmasını denetler ve MMA düğüme gönderilir.

```PowerShell
Configuration MMAgent
{
    $OIPackageLocalPath = "C:\MMASetup-AMD64.exe"
    $OPSINSIGHTS_WS_ID = Get-AutomationVariable -Name "OPSINSIGHTS_WS_ID"
    $OPSINSIGHTS_WS_KEY = Get-AutomationVariable -Name "OPSINSIGHTS_WS_KEY"


    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    Node OMSnode {
        Service OIService
        {
            Name = "HealthService"
            State = "Running"
            DependsOn = "[Package]OI"
        }

        xRemoteFile OIPackage {
            Uri = "https://go.microsoft.com/fwlink/?LinkId=828603"
            DestinationPath = $OIPackageLocalPath
        }

        Package OI {
            Ensure = "Present"
            Path  = $OIPackageLocalPath
            Name = "Microsoft Monitoring Agent"
            ProductId = "8A7F2C51-4C7D-4BFD-9014-91D11F24AAE2"
            Arguments = '/C:"setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=' + $OPSINSIGHTS_WS_ID + ' OPINSIGHTS_WORKSPACE_KEY=' + $OPSINSIGHTS_WS_KEY + ' AcceptEndUserLicenseAgreement=1"'
            DependsOn = "[xRemoteFile]OIPackage"
        }
    }
}


```

### <a name="get-the-latest-productid-value"></a>En son ProductID değeri Al

`ProductId value` MMAgent.ps1 komut dosyası her aracı sürümü için benzersizdir. Her bir aracının güncelleştirilmiş bir sürümü yayımlandığında, ProductID değerini değiştirir. Bu nedenle, ProductID gelecekte değiştiğinde, basit bir komut dosyası kullanarak aracı sürümü bulabilirsiniz. Bir test sunucusunda yüklü Aracısı'nın en son sürümünü çalıştırdıktan sonra yüklü ProductID değeri almak için aşağıdaki komut dosyasını kullanabilirsiniz. En son ProductID değerini kullanarak, MMAgent.ps1 komut dosyasındaki değeri güncelleştirebilirsiniz.

```PowerShell
$InstalledApplications  = Get-ChildItem hklm:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall


foreach ($Application in $InstalledApplications)

{

     $Key = Get-ItemProperty $Application.PSPath

     if ($Key.DisplayName -eq "Microsoft Monitoring Agent")

     {

        $Key.DisplayName

        Write-Output ("Product ID is: " + $Key.PSChildName.Substring(1,$Key.PSChildName.Length -2))

     }

}  
```

## <a name="configure-an-agent-manually-or-add-additional-workspaces"></a>Bir aracı el ile yapılandırma veya ek çalışma alanları ekleme
Aracıları yüklediyseniz bunları yapılandırmadı ancak veya birden çok çalışma alanına bildirmeye Aracısı istiyorsanız, bir aracı etkinleştirmek veya onu yeniden yapılandırmak için aşağıdaki bilgileri kullanabilirsiniz. Aracısı'nı yapılandırdıktan sonra Aracı hizmeti ile kaydeder ve gerekli yapılandırma bilgilerini ve çözüm bilgileri içeren yönetim paketleri alır.

1. Microsoft İzleme Aracısı yükledikten sonra açmak **Denetim Masası**.
2. Açık **Microsoft İzleme Aracısı** ve ardından **Azure günlük analizi (OMS)** sekmesi.   
3. Tıklatın **Ekle** açmak için **günlük analizi çalışma alanı Ekle** kutusu.
4. Yapıştır **çalışma alanı kimliği** ve **çalışma alanı anahtarı (birincil anahtar)** ekleyin ve ardından istediğiniz çalışma alanı için bir önceki yordamda Not Defteri'ne kopyaladığınız **Tamam**.  
    ![Operasyonel Öngörüler yapılandırın](./media/log-analytics-windows-agents/add-workspace.png)

Aracı tarafından izlenen bilgisayarlardan veriler toplandıktan sonra OMS tarafından izlenen bilgisayarların sayısını OMS portalında görünür **bağlı kaynakları** sekmesinde **ayarları** olarak **sunucuları Bağlı**.


## <a name="to-disable-an-agent"></a>Bir aracı devre dışı bırakmak için
1. Aracıyı yükledikten sonra açmak **Denetim Masası**.
2. Microsoft Monitoring Agent'ı açın ve ardından **Azure günlük analizi (OMS)** sekmesi.
3. Bir çalışma alanını seçin ve ardından **kaldırmak**. Bu adım için diğer çalışma alanları yineleyin.


## <a name="optionally-configure-agents-to-report-to-an-operations-manager-management-group"></a>İsteğe bağlı olarak, bir Operations Manager yönetim grubu için rapor aracıları yapılandırın

BT altyapınızın Operations Manager'ı kullanırsanız, MMA aracı Operations Manager aracısı olarak kullanabilirsiniz.

### <a name="to-configure-mma-agents-to-report-to-an-operations-manager-management-group"></a>Bir Operations Manager yönetim grubu için rapor için MMA aracıları yapılandırmak için
1.  Aracının yüklü olduğu bilgisayarda açın **Denetim Masası**.  
2.  Açık **Microsoft İzleme Aracısı** ve ardından **Operations Manager** sekmesi.  
    ![Microsoft İzleme Aracısı Operations Manager sekmesi](./media/log-analytics-windows-agents/om-mg01.png)
3.  Operations Manager sunucularınızın Active Directory ile tümleştirme varsa, tıklatın **yönetim grubu atamalarını AD DS'den otomatik olarak güncelleştir**.
4.  Tıklatın **Ekle** açmak için **bir yönetim grubu Ekle** iletişim kutusu.  
    ![Microsoft İzleme Aracısı Yönetim Grubu Ekle](./media/log-analytics-windows-agents/oms-mma-om02.png)
5.  İçinde **yönetim grubu adı** kutusuna, yönetim grubunuzun adını yazın.
6.  İçinde **birincil yönetim sunucusu** birincil yönetim sunucusunun bilgisayar adını yazın.
7.  İçinde **yönetim sunucusu bağlantı noktası** TCP bağlantı noktası numarasını yazın.
8.  Altında **aracı eylem hesabı**, yerel sistem hesabı veya bir yerel etki alanı hesabı seçin.
9.  Tıklatın **Tamam** kapatmak için **bir yönetim grubu Ekle** iletişim kutusunu ve ardından **Tamam** kapatmak için **Microsoft Monitoring Agent özellikleri** iletişim kutusu.


## <a name="next-steps"></a>Sonraki adımlar

- İşlev eklemek ve veri toplamak için bkz. [Çözüm Galerisinden Log Analytics çözümleri ekleme](log-analytics-add-solutions.md).
