---
title: Windows bilgisayarları Azure Log Analytics'e bağlayın | Microsoft Docs
description: Bu makalede, diğer Bulut veya şirket için Log Analytics ile Microsoft Monitoring Agent (MMA) barındırılan Windows bilgisayarları bağlama açıklar.
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
ms.date: 04/29/2019
ms.author: magoedte
ms.openlocfilehash: 2d57e619ec17e183bc8c9bb155f3e111f43b85f1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65952475"
---
# <a name="connect-windows-computers-to-azure-monitor"></a>Azure İzleyici Windows bilgisayarları bağlama

İzleyin ve sanal makine ya da yerel veri merkezinizde veya diğer bulut ortamında Azure İzleyici ile fiziksel bilgisayarları yönetmek için (Microsoft Monitoring Agent (MMA) olarak da bilinir) Log Analytics aracısını dağıtmayı ve şekilde yapılandırmanız gerekir bir veya daha fazla Log Analytics çalışma alanına raporlama. Aracı, karma Runbook çalışanı rolü için Azure Otomasyonu da destekler.  

İzlenen bir Windows bilgisayarda, aracıyı Microsoft Monitoring Agent hizmeti olarak listelenir. Microsoft Monitoring Agent hizmeti, günlük dosyaları ve Windows olay günlüğü, performans verilerini ve diğer telemetriyi olayları toplar. Aracının rapor Azure İzleyici ile iletişim kurmada başarısız olsa bile, aracı çalışmaya devam eder ve toplanan verileri izlenen bilgisayarın diskinde sıralar. Bağlantı geri geldiğinde, Microsoft Monitoring Agent hizmeti toplanan verileri hizmetine gönderir.

Aracı, aşağıdaki yöntemlerden birini kullanarak yüklenebilir. Çoğu yüklemeleri, farklı bilgisayar gruplarını uygun şekilde yüklemek için bu yöntemlerin bir bileşimi kullanın.  Her yöntemi kullanma hakkında ayrıntılar, makalenin sonraki bölümlerinde verilmiştir.

* El ile yükleme. Kurulum, komut satırından Kurulum sihirbazını kullanarak bilgisayarı el ile çalıştırmak veya mevcut bir yazılım dağıtım aracıyla dağıtılır.
* Azure Otomasyonu (DSC) Desired State Configuration. DSC ortamınızda dağıtılmış Windows bilgisayarlar için bir komut dosyası ile Azure Otomasyonu'nda kullanma.  
* PowerShell Betiği.
* Windows Şirket içi Azure Stack'te çalışan sanal makineler için Resource Manager şablonu. 

>[!NOTE]
>Azure Güvenlik Merkezi (ASC) Microsoft Monitoring Agent (Log Analytics Windows aracısı olarak da bilinir) bağlıdır ve yükleyecek ve kendi dağıtımının bir parçası bir Log Analytics çalışma alanına raporlama yapacak yapılandırın. ASC, Log Analytics Windows Aracısı, aboneliğinizdeki tüm sanal makineler otomatik olarak yüklenmesini sağlar ve belirli bir çalışma alanına raporlama yapacak yapılandırır otomatik sağlama seçeneği de sunar. Bu seçenek hakkında daha fazla bilgi için bkz. [Log Analytics aracısını otomatik sağlamayı etkinleştirme](../../security-center/security-center-enable-data-collection.md#enable-automatic-provisioning-of-microsoft-monitoring-agent-).
>

Birden fazla çalışma alanına raporlama yapacak aracı yapılandırmanız gerekiyorsa, bu yalnızca daha sonra göre Denetim Masası veya PowerShell bölümünde anlatıldığı gibi güncelleştirme ayarlarını ilk kurulum sırasında gerçekleştirilemiyor [ekleme veya bir çalışma alanıkaldırma](agent-manage.md#adding-or-removing-a-workspace).  

Desteklenen yapılandırmayı anlamak için [desteklenen Windows işletim sistemlerini](log-analytics-agent.md#supported-windows-operating-systems) ve [ağ güvenlik duvarı yapılandırmasını](log-analytics-agent.md#network-firewall-requirements) inceleyin.

## <a name="obtain-workspace-id-and-key"></a>Çalışma alanı kimliği ve anahtarını alma
Windows için Log Analytics aracısını yüklemeden önce çalışma alanı kimliği ve anahtarına ihtiyacınız olacak Log Analytics çalışma alanınız için.  Bu bilgiler, her bir yükleme yöntemi, düzgün bir şekilde yapılandırın ve başarılı bir şekilde Azure İzleyici'de Azure ticari ve ABD kamu Bulutu ile iletişim kurabildiğinden olun kurulumu sırasında gereklidir. 

1. Azure portalında **Tüm hizmetler**’e tıklayın. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Log Analytics**’i seçin.
2. Log Analytics çalışma alanlarınızın listesinde aracının yapılandırılmasında üzerinde rapor istediğiniz çalışma alanını seçin.
3. **Gelişmiş ayarlar**’ı seçin.<br><br> ![Log Analytics Gelişmiş Ayarlar](media/agent-windows/log-analytics-advanced-settings-01.png)<br><br>  
4. **Bağlı Kaynaklar**’ı seçin ve ardından **Windows Sunucuları**’nı seçin.   
5. Sık kullandığınız düzenleyicinizi kopyalayıp **çalışma alanı kimliği** ve **birincil anahtar**.    
   
## <a name="configure-agent-to-use-tls-12"></a>TLS 1.2 kullanmak üzere Aracısı'nı yapılandırma
Kullanımını yapılandırmak için [TLS 1.2](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings#tls-12) protokolü Windows Aracısı'nı ve Log Analytics hizmeti arasındaki iletişimi için aracıyı sanal makineye yüklenmeden önce etkinleştirmek için aşağıdaki adımları takip edebilirsiniz veya daha sonra.   

1. Aşağıdaki kayıt defteri alt anahtarını bulun: **HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols**
2. Altında bir alt anahtarını oluşturmak **protokolleri** TLS 1.2 **HKLM\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2**
3. Oluşturma bir **istemci** alt anahtar, daha önce oluşturduğunuz TLS 1.2 protokolü sürümü alt anahtarı altında. Örneğin, **HKLM\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client**.
4. Altında aşağıdaki DWORD değerleri oluşturmak **HKLM\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client**:

    * **Etkin** [değer = 1]
    * **DisabledByDefault** [değer = 0]  

.NET Framework 4.6 yapılandırabilir veya daha sonra güvenli desteklemek için varsayılan olarak olarak şifreleme devre dışı bırakıldı. [Güçlü şifreleme](https://docs.microsoft.com/dotnet/framework/network-programming/tls#schusestrongcrypto) TLS 1.2 gibi daha güvenli ağ protokollerini kullanır ve güvenli olmayan protokolleri engeller. 

1. Aşağıdaki kayıt defteri alt anahtarını bulun: **HKEY_LOCAL_MACHINE\Software\Microsoft\\. NETFramework\v4.0.30319**.  
2. DWORD değeri oluşturun **SchUseStrongCrypto** bu alt anahtar değerine sahip altında **1**.  
3. Aşağıdaki kayıt defteri alt anahtarını bulun: **HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\\. NETFramework\v4.0.30319**.  
4. DWORD değeri oluşturun **SchUseStrongCrypto** bu alt anahtar değerine sahip altında **1**. 
5. Ayarların etkili olması sistemi yeniden başlatın. 

## <a name="install-the-agent-using-setup-wizard"></a>Kurulum sihirbazını kullanarak aracı yükleme
Aşağıdaki adımlar, yükleme ve bilgisayarınızda Aracısı Kurulum Sihirbazı'nı kullanarak Azure'da ve Azure kamu bulutunda Log Analytics aracısını yapılandırın. Ayrıca raporlayacağı bir System Center Operations Manager yönetim grubu için yapılandırma hakkında bilgi edinmek isterseniz [Operations Manager aracısını Aracı Kurulum sihirbazıyla dağıtma](https://docs.microsoft.com/system-center/scom/manage-deploy-windows-agent-manually#to-deploy-the-operations-manager-agent-with-the-agent-setup-wizard).

1. Log Analytics çalışma alanınızda gelen **Windows sunucuları** çıkıldığında daha önce uygun seçmek için sayfayı **Windows Agent'ı indir** İşlemci mimarisi bağlı olarak indirmek için sürümü Windows işletim sistemi.   
2. Aracıyı bilgisayarınıza yüklemek için Kurulum'u çalıştırın.
2. **Hoş Geldiniz** sayfasında **İleri**'ye tıklayın.
3. **Lisans Koşulları** sayfasında, lisansı okuyun ve **Kabul Ediyorum**'a tıklayın.
4. **Hedef Klasör** sayfasında, varsayılan yükleme klasörünü değiştirin veya koruyun ve ardından **İleri**'ye tıklayın.
5. **Aracı Kurulum Seçenekleri** sayfasında, aracıyı Azure Log Analytics'e bağlamayı seçin ve ardından **İleri**'ye tıklayın.   
6. **Azure Log Analytics** sayfasında aşağıdakileri yapın:
   1. Daha önce kopyaladığınız **Çalışma Alanı Kimliği** ve **Çalışma Alanı Anahtarı (Birincil Anahtar)** değerlerini yapıştırın.  Bilgisayarın Azure Kamu bulutundaki bir Log Analytics çalışma alanına raporlaması gerekiyorsa, **Azure Cloud** açılan listesinden **Azure ABD Kamu**'yu seçin.  
   2. Bilgisayarın Log Analytics hizmetiyle bir ara sunucu üzerinden iletişim kurması gerekiyorsa, **Gelişmiş**'e tıklayın ve ara sunucunun URL'siyle bağlantı noktası numarasını sağlayın.  Ara sunucunuz kimlik doğrulaması gerektiriyorsa, ara sunucuyla kimlik doğrulaması yapmak için kullanıcı adını ve parolayı yazın, ardından **İleri**'ye tıklayın.  
7. Gerekli yapılandırma ayarlarını sağlamayı tamamladığınızda **İleri**'ye tıklayın.<br><br> ![Çalışma Alanı Kimliği ve Birincil Anahtarı'nı yapıştırın](media/agent-windows/log-analytics-mma-setup-laworkspace.png)<br><br>
8. **Yüklemeye Hazır** sayfasında seçimlerinizi gözden geçirin ve ardından **Yükle**'ye tıklayın.
9. **Yapılandırma başarıyla tamamlandı** sayfasında **Son**'a tıklayın.

Tamamlandığında, **Denetim Masası**'nda **Microsoft Monitoring Agent** gösterilir. Log Analytics'e raporlama bunu doğrulamak için gözden [Log Analytics aracı bağlantısını doğrulamak](#verify-agent-connectivity-to-log-analytics). 

## <a name="install-the-agent-using-the-command-line"></a>Komut satırını kullanarak aracı yükleme
Aracı için indirilen dosyayı kendi içinde yükleme paketidir.  Destekleyici dosyaları ve aracı için Kurulum programı, pakette yer alan ve düzgün bir şekilde aşağıdaki örneklerde gösterildiği komut satırını kullanarak yüklemek için ayıklanan gerekir.    

>[!NOTE]
>Bir aracıyı yükseltmek istiyorsanız, Log Analytics betik API'si kullanmanız gerekir. Konusuna [yönetme ve Windows ve Linux için Log Analytics aracısını korumak](agent-manage.md) daha fazla bilgi için.

Aşağıdaki tabloda, Automation DSC kullanılarak dağıtıldığında dahil olmak üzere aracı için kurulum tarafından desteklenen belirli parametreleri vurgulanmaktadır.

|MMA özgü seçenekleri                   |Notlar         |
|---------------------------------------|--------------|
| NOAPM = 1                               | İsteğe bağlı parametre. Olmadan .NET uygulama performansı izleme Aracısı'nı yükler.|   
|ADD_OPINSIGHTS_WORKSPACE               | 1 = çalışma alanına raporlama yapacak aracısını yapılandırma                |
|OPINSIGHTS_WORKSPACE_ID                | Eklemek için çalışma alanı kimliği (GUID) çalışma alanı için                    |
|OPINSIGHTS_WORKSPACE_KEY               | Başlangıçta çalışma alanı ile kimlik doğrulaması için kullanılan çalışma alanı anahtarı |
|OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE  | Çalışma alanının bulunduğu bulut ortamını belirtin <br> 0 = azure ticari bulutundaki (varsayılan) <br> 1 = azure kamu |
|OPINSIGHTS_PROXY_URL               | Proxy'nin kullanması URI |
|OPINSIGHTS_PROXY_USERNAME               | Kimliği doğrulanmış bir proxy sunucusuna erişmek için kullanıcı adı |
|OPINSIGHTS_PROXY_PASSWORD               | Kimliği doğrulanmış bir proxy sunucusuna erişmek için parola |

1. Çalıştırma yükseltilmiş komut isteminden aracı yükleme dosyalarını ayıklamak için `MMASetup-<platform>.exe /c` ve bu dosyaları ayıklamak yol, ister.  Alternatif olarak, bağımsız değişkenleri geçirme olan yolu belirtebilirsiniz `MMASetup-<platform>.exe /c /t:<Full Path>`.  
2. Sessizce aracıyı yükleyin ve bunu Azure ticari bulutundaki bir çalışma alanına klasöründen bildirmek için yapılandırmak üzere yazmak için Kurulum dosyaları ayıklanır: 
   
     ```dos
    setup.exe /qn NOAPM=1 ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE=0 OPINSIGHTS_WORKSPACE_ID=<your workspace ID> OPINSIGHTS_WORKSPACE_KEY=<your workspace key> AcceptEndUserLicenseAgreement=1
    ```

   veya, Azure ABD kamu Bulutu bildirmek için aracıyı yapılandırmak için şunu yazın: 

     ```dos
    setup.exe /qn NOAPM=1 ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE=1 OPINSIGHTS_WORKSPACE_ID=<your workspace ID> OPINSIGHTS_WORKSPACE_KEY=<your workspace key> AcceptEndUserLicenseAgreement=1
    ```

## <a name="install-the-agent-using-dsc-in-azure-automation"></a>Azure Otomasyonu DSC kullanarak aracı yükleme

Aşağıdaki kod örneği, Azure Automation DSC kullanarak aracıyı yüklemek için kullanabilirsiniz.   Bir Otomasyon hesabı yoksa bkz [Azure Otomasyonu ile çalışmaya başlama](/azure/automation/) gereksinimleri ve Automation DSC kullanmadan önce gerekli bir Otomasyon hesabı oluşturma adımları anlamak için.  Automation DSC ile ilgili bilgi sahibi değilseniz, gözden [Automation DSC ile çalışmaya başlama](../../automation/automation-dsc-getting-started.md).

Aşağıdaki örnek tarafından tanımlanmış 64 bit aracı yükler `URI` değeri. URI değerini değiştirerek, 32 bit sürümü de kullanabilirsiniz. Bir URI'leri iki sürümü vardır:

- Windows 64-bit Aracısı- https://go.microsoft.com/fwlink/?LinkId=828603
- Windows 32-bit Aracısı- https://go.microsoft.com/fwlink/?LinkId=828604


>[!NOTE]
>Bu yordam ve betik örnek, zaten bir Windows bilgisayara dağıtılan aracı yükseltmeyi desteklemez.

Aracı paketi 32-bit ve 64-bit sürümleri farklı ürün kodları ve yayımlanan yeni sürümleri de benzersiz bir değere sahiptir.  Ürün kodu, bir uygulama veya ürün asıl kimliği ve Windows Installer tarafından temsil edilen bir GUID'dir **ProductCode** özelliği.  `ProductId` Değerini **MMAgent.ps1** betik, 32 bit veya 64 bit aracı Yükleyici paketini ürün kodu eşleşmelidir.

Ürün kodu aracı yükleme paketinden doğrudan almak için gelen Orca.exe'yi kullanabilirsiniz [Windows Installer geliştiriciler için Windows SDK Bileşenleri](https://msdn.microsoft.com/library/windows/desktop/aa370834%28v=vs.85%29.aspx) diğer bir deyişle bir bileşeni olan Windows Yazılım Geliştirme Seti veya kullanma PowerShell aşağıdaki bir [örnek betiği](https://www.scconfigmgr.com/2014/08/22/how-to-get-msi-file-information-with-powershell/) bir Microsoft Valuable Professional (MVP) tarafından yazılmış.  Her iki yaklaşım için önce ayıklamak ihtiyacınız **MOMagent.msi** MMASetup yükleme paketinden dosya.  Bu bölümünde daha önce ilk adımda gösterilen [komut satırını kullanarak aracı yükleme](#install-the-agent-using-the-command-line).  

1. İçeri aktarma xPSDesiredStateConfiguration DSC modülünden [ https://www.powershellgallery.com/packages/xPSDesiredStateConfiguration ](https://www.powershellgallery.com/packages/xPSDesiredStateConfiguration) Azure Otomasyonu ile.  
2.  Azure Otomasyonu değişken varlıkları için oluşturma *OPSINSIGHTS_WS_ID* ve *OPSINSIGHTS_WS_KEY*. Ayarlama *OPSINSIGHTS_WS_ID* Log Analytics çalışma alanı kimliği ve kümesi *OPSINSIGHTS_WS_KEY* çalışma alanınızın birincil anahtarı.
3.  Betiği kopyalayın ve MMAgent.ps1 kaydedin.

    ```powershell
    Configuration MMAgent
    {
        $OIPackageLocalPath = "C:\Deploy\MMASetup-AMD64.exe"
        $OPSINSIGHTS_WS_ID = Get-AutomationVariable -Name "OPSINSIGHTS_WS_ID"
        $OPSINSIGHTS_WS_KEY = Get-AutomationVariable -Name "OPSINSIGHTS_WS_KEY"

        Import-DscResource -ModuleName xPSDesiredStateConfiguration
        Import-DscResource -ModuleName PSDesiredStateConfiguration

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
                Arguments = '/C:"setup.exe /qn NOAPM=1 ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=' + $OPSINSIGHTS_WS_ID + ' OPINSIGHTS_WORKSPACE_KEY=' + $OPSINSIGHTS_WS_KEY + ' AcceptEndUserLicenseAgreement=1"'
                DependsOn = "[xRemoteFile]OIPackage"
            }
        }
    }

    ```

4. Güncelleştirme `ProductId` Aracısı'nın en son sürümü ayıklanan ürün kodu betiğiyle değeri, daha önce önerilen yöntemleri kullanarak paketini yükleyin. 
5. [MMAgent.ps1 yapılandırma betiğini alma](../../automation/automation-dsc-getting-started.md#importing-a-configuration-into-azure-automation) Otomasyon hesabınızda. 
5. [Bir Windows bilgisayarına veya düğüm atama](../../automation/automation-dsc-getting-started.md#onboarding-an-azure-vm-for-management-with-azure-automation-state-configuration) yapılandırması. 15 dakika içinde düğüm yapılandırmasını denetler ve aracının düğüme gönderilir.

## <a name="verify-agent-connectivity-to-log-analytics"></a>Log Analytics aracısını bağlantısını doğrulayın

Aracı yüklemesi tamamlandıktan sonra bu doğrulama başarıyla bağlandı ve raporlama iki şekilde gerçekleştirilebilir.  

Bilgisayarın **Denetim Masası** sayfasında **Microsoft Monitoring Agent**'ı bulun.  Bu girişi seçtiğinizde **Azure Log Analytics** sekmesinde aracının şu iletiyi görüntülemesi gerekir: **Microsoft Monitoring Agent Microsoft Operations Management Suite hizmetine başarıyla bağlandı.**<br><br> ![MMA'nın Log Analytics'e bağlantı durumu](media/agent-windows/log-analytics-mma-laworkspace-status.png)

Ayrıca, Azure portalında basit günlük sorgusu gerçekleştirebilirsiniz.  

1. Azure portalında **Tüm hizmetler**’e tıklayın. Kaynak listesinde yazın **Azure İzleyici**. Yazmaya başladığınızda liste, girişinize göre filtrelenir. Seçin **Azure İzleyici**.  
2. Seçin **günlükleri** menüsünde. 
2. Günlükleri bölmesinde, sorgu alan türü:  

    ```
    Heartbeat 
    | where Category == "Direct Agent" 
    | where TimeGenerated > ago(30m)  
    ```

Döndürülen arama sonuçlarında, bağlı olduğu belirten ve raporlama hizmete bilgisayar için sinyal kayıtlarını görmeniz gerekir.   

## <a name="next-steps"></a>Sonraki adımlar

Gözden geçirme [yönetme ve Windows ve Linux için Log Analytics aracısını korumak](agent-manage.md) makinelerinize dağıtım yaşam döngüsü sırasında Aracısı'nı yönetme hakkında bilgi edinmek için.  
