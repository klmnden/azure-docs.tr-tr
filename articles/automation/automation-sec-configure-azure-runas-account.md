<properties
    pageTitle="Azure Farklı Çalıştır Hesabını Yapılandırma | Microsoft Azure"
    description="Eğitici, Azure Automation’da güvenlik temel elemanı kimlik doğrulaması oluşturulması, test edilmesi ve örneklerinde size yol göstermektedir."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor=""
    keywords="hizmet asıl adı, setspn, azure kimlik doğrulaması"/>
<tags
    ms.service="automation"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/29/2016"
    ms.author="magoedte"/>

# Azure Farklı Çalıştır hesabıyla Kimlik Doğrulaması Runbook’ları

Bu konuda, Azure Resource Manager veya Azure Service Management’ta kaynakları yöneten runbook’ların kimliğini doğrulamak üzere Farklı Çalıştır hesap özelliği kullanılarak Azure portalından bir Otomasyon hesabının nasıl yapılandırılacağı gösterilecektir.

Azure portalında yeni bir Otomasyon hesabı oluşturduğunuzda otomatik olarak şunlar oluşturulur:

- Azure Active Directory’de yeni bir hizmet sorumlusu ve bir sertifika oluşturan ve Katkı Yapana runbook’lar kullanılarak Resource Manager kaynaklarını yönetmek için kullanılacak rol tabanlı erişim denetimi (RBAC) atayan Farklı Çalıştır hesabı.   
- Azure Service Management’ı ya da runbook kullanan klasik kaynakları yönetmek için kullanılacak bir yönetim sertifikasını karşıya yükleyen Klasik Farklı Çalıştır hesabı.  

İşlemi sizin için basitleştirir ve otomasyon gerekliliklerini desteklemek için runbook’ları oluşturmaya ve dağıtmaya hemen başlamanıza yardımcı olur.      

Farklı Çalıştır ve Klasik Farklı Çalıştır hesabını kullanarak şunları yapabilirsiniz:

- Azure portalındaki runbook’lardan Azure Resource Manager veya Azure Service Management kaynakları yönetilirken Azure ile kimlik doğrulaması için standartlaştırılmış bir yol sağlama.  
- Azure Uyarıları’nda yapılandırılmış genel runbook'ların kullanımını otomatikleştirme


>[AZURE.NOTE] Otomasyon Genel Runbook’larına sahip Azure [Uyarı tümleştirme özelliği](../azure-portal/insights-receive-alert-notifications.md) için Farklı Çalıştır ve Klasik Farklı Çalıştır hesabıyla yapılandırılmış bir Otomasyon hesabı gerekir. Farklı Çalıştır ve Klasik Farklı Çalıştır hesabı zaten tanımlanmış bir Otomasyon hesabı seçebilir ya da yeni bir tane de oluşturabilirsiniz.

Otomasyon hesabının Azure portalından oluşturulması, bir Otomasyon hesabının PowerShell kullanılarak güncelleştirilmesi ve runbook’larınızda kimlik doğrulamasının nasıl yapılacağı size gösterilecektir.

Bunu yapmadan önce anlamanız ve göz önünde bulundurmanız gereken birkaç nokta vardır.

1. Bu durum klasik veya Resource Manager dağıtım modelinde daha önce oluşturulmuş var olan Otomasyon hesaplarını etkilemez.  
2. Bu özellik yalnızca Azure portalından oluşturulan Otomasyon hesapları için çalışır.  Klasik portaldan bir hesap oluşturulmaya çalışıldığında Farklı Çalıştır hesabının yapılandırması çoğaltılmaz.
3. Şu anda klasik kaynakları yönetmek için daha önce oluşturulmuş runbook’lara ve varlıklara (zamanlamalar, değişkenler vb.) sahipseniz ve bu runbook’ların yeni Klasik Farklı Çalıştır hesabıyla kimlik doğrulamasını yapmak istiyorsanız bunları yeni Otomasyon hesabına geçirmeniz veya var olan hesabınızı aşağıdaki PowerShell komut dosyasıyla güncelleştirmeniz gerekir.  
4. Yeni Farklı Çalıştır hesabını ve Klasik Farklı Çalıştır Otomasyon hesabını kullanarak kimlik doğrulamak için var olan runbook’larınızı aşağıdaki örnek kod ile değiştirmeniz gerekir.  Farklı Çalıştır hesabının sertifika tabanlı hizmet sorumlusu kullanan Resource Manager kaynaklarına göre kimlik doğrulamasına, Klasik Farklı Çalıştır hesabının ise yönetim sertifikasına sahip Service Management kaynaklarına göre kimlik doğrulamasına yönelik olduğunu **lütfen unutmayın**.     


## Azure Portal'dan yeni Automation Hesabı oluşturma

Bu bölümde, yeni bir Azure Otomasyonu hesabını Azure portalından oluşturmak için aşağıdaki adımları gerçekleştireceksiniz.  Bu işlem hem Farklı Çalıştır hem de klasik Farklı Çalıştır hesabı oluşturur.  

>[AZURE.NOTE] Bu adımları gerçekleştiren kullanıcı, Abonelik Yöneticileri rolünün üyesi ve kullanıcıya abonelik erişimi veren aboneliğin ortak yöneticisi *olmalıdır*.  Kullanıcı ayrıca ilgili aboneliğin varsayılan Active Directory’sine Kullanıcı olarak eklenmelidir; hesabın ayrıcalıklı bir role atanması gerekmez. 

1. Azure portalında Abonelik Yöneticileri rolünün üyesi ve aboneliğin ortak yöneticisi olan bir hesapla oturum açın.
2. **Automation Hesapları**’nı seçin.
3. Automation Hesapları dikey penceresinde **Ekle**’ye tıklayın.<br>![Automation Hesabı ekleme](media/automation-sec-configure-azure-runas-account/create-automation-account-properties.png)

    >[AZURE.NOTE] **Otomasyon Hesabı Ekle** dikey penceresinde aşağıdaki uyarıyı görürseniz, bunun nedeni hesabınızın Abonelik Yöneticileri rolünün üyesi ya da aboneliğin ortak yöneticisi olmamasıdır.<br>![Automation Hesabı Uyarısı ekleme](media/automation-sec-configure-azure-runas-account/create-account-without-perms.png)

4. **Automation Hesabı Ekle** dikey penceresinde, **Ad** kutusuna, yeni Automation hesabınız için bir ad yazın.
5. Birden fazla aboneliğiniz varsa, yeni hesap için mevcut veya yeni **Kaynak grubu** ve Azure veri merkezi **konumu** ile birlikte bir tane belirtin.
6. **Azure Farklı Çalıştır hesabı oluştur** seçeneği için **Evet** değerinin seçili olduğunu doğrulayın ve **Oluştur** düğmesine tıklayın.  

    >[AZURE.NOTE] **Hayır** seçeneğini belirleyerek Farklı Çalıştır hesabı oluşturmamayı seçerseniz, **Automation Hesabı Ekle** dikey penceresinde bir uyarı iletisi görürsünüz.  Hesap Azure portalında oluşturulurken klasik veya Resource Manager abonelik dizininizde ona karşılık gelen bir kimlik doğrulama kimliği olmaz ve bu nedenle aboneliğinizdeki kaynaklara erişemez.  Bu durum, bu hesaba başvuran runbook’ların söz konusu dağıtım modellerindeki kaynaklara göre kimlik doğrulama yapmasını ve görevler gerçekleştirmesini engeller.
    
    >![Automation Hesabı Uyarısı ekleme](media/automation-sec-configure-azure-runas-account/create-account-decline-create-runas-msg.png)<br>
    Hizmet sorumlusu oluşturulmadığında Katkıda Bulunan rolü atanmaz.


7. Azure Automation hesabını oluşturduğu sırada menünün **Bildirimler** öğesi altında ilerleme durumunu izleyebilirsiniz.

### Kaynaklar dahil

Otomasyon hesabı başarıyla oluşturulduğunda bazı kaynaklar sizin için otomatik olarak oluşturulur.  Aşağıdaki tabloda Farklı Çalıştır hesabının kaynakları özetlenmektedir.<br>

Kaynak|Açıklama 
----|----
AzureAutomationTutorial Runbook|Farklı Çalıştır hesabını kullanarak kimlik doğrulaması yapmayı gösteren ve tüm Resource Manager kaynaklarını alan örnek bir PowerShell runbook.
AzureAutomationTutorialScript Runbook|Farklı Çalıştır hesabını kullanarak kimlik doğrulaması yapmayı gösteren ve tüm Resource Manager kaynaklarını alan örnek bir PowerShell runbook. 
AzureRunAsCertificate|Otomasyon hesabı oluşturulurken otomatik olarak oluşturulan ya da var olan bir hesap için aşağıdaki PowerShell komut dosyası kullanılarak oluşturulan sertifika varlığı.  Azure Resource Manager kaynaklarını runbook’lardan yönetebilmeniz için Azure kimlik doğrulaması yapmanıza imkan tanır.  Bu sertifikanın bir yıllık kullanım ömrü vardır. 
AzureRunAsConnection|Otomasyon hesabı oluşturulurken otomatik olarak oluşturulan ya da var olan bir hesap için aşağıdaki PowerShell komut dosyası kullanılarak oluşturulan bağlantı varlığı.

Aşağıdaki tabloda Klasik Farklı Çalıştır hesabının kaynakları özetlenmektedir.<br>

Kaynak|Açıklama 
----|----
AzureClassicAutomationTutorial Runbook|Bir abonelikteki tüm Klasik Sanal Makineleri, Klasik Farklı Çalıştır Hesabı (sertifika) kullanarak alan ve sonra sanal makine adını ve durumunu çıkaran örnek runbook.
AzureClassicAutomationTutorial Script Runbook|Bir abonelikteki tüm Klasik Sanal Makineleri, Klasik Farklı Çalıştır Hesabı (sertifika) kullanarak alan ve sonra sanal makine adını ve durumunu çıkaran örnek runbook.
AzureClassicRunAsCertificate|Otomatik olarak oluşturulan ve Azure klasik kaynaklarını runbook’lardan yönetebilmeniz için Azure kimlik doğrulaması yapmak üzere kullanılan sertifika varlığı.  Bu sertifikanın bir yıllık kullanım ömrü vardır. 
AzureClassicRunAsConnection|Otomatik olarak oluşturulan ve Azure klasik kaynaklarını runbook’lardan yönetebilmeniz için Azure kimlik doğrulaması yapmak üzere kullanılan bağlantı varlığı.  

## Farklı Çalıştır kimlik doğrulamasını onaylama

Yeni Farklı Çalıştır hesabını kullanarak kimlik doğrulamasını sorunsuz yaptığınızı doğrulamak için burada küçük bir testle devam ediyoruz.     

1. Azure Portal'da, daha önce oluşturduğunuz Automation hesabını açın.  
2. Runbook'ların listesini açmak için **Runbook'lar** kutucuğuna tıklayın.
3. **AzureAutomationTutorialScript** runbook’unu seçin ve ardından **Başlat**’a tıklayarak runbook’u başlatın.  Runbook'u başlatmak istediğinizi doğrulayan bir ileti alacaksınız.
4. Bir [runbook işi](automation-runbook-execution.md) oluşturulur, İş dikey penceresi gösterilir ve iş durumu **İş Özeti** kutucuğunda gösterilir.  
5. İş durumu, bulutta bir runbook çalışanının kullanılabilir hale gelmesinin beklendiğini gösteren şekilde *Sırada* olarak başlar. Ardından, bir çalışan işi talep ettiğinde *Başlatılıyor* olarak ve runbook gerçekten çalışmaya başladığında *Çalışıyor* olarak değiştirilir.  
6. Runbook işi tamamlandığında **Tamamlandı** durumunu görmeniz gerekir.<br> ![Güvenlik Sorumlusu Runbook Testi](media/automation-sec-configure-azure-runas-account/job-summary-automationtutorialscript.png)<br>
7. Runbook’un ayrıntılı sonuçlarını görmek için **Çıktı** kutucuğuna tıklayın.
8. **Çıktı** dikey penceresinde kimlik doğrulamasının başarıyla yapıldığını ve kaynak grubunda mevcut olan tüm kaynakların bir listesini döndürdüğünü görmeniz gerekir. 
9. **Çıktı** dikey penceresini kapatarak **İş Özeti** dikey penceresine geri dönün.
13. **İş Özeti**’ni ve karşılık gelen **AzureAutomationTutorialScript** runbook dikey penceresini kapatın.

## Klasik Farklı Çalıştır kimlik doğrulamasını onaylama

Yeni Klasik Farklı Çalıştır hesabını kullanarak kimlik doğrulamasını sorunsuz yaptığınızı doğrulamak için burada küçük bir testle devam ediyoruz.     

1. Azure Portal'da, daha önce oluşturduğunuz Automation hesabını açın.  
2. Runbook'ların listesini açmak için **Runbook'lar** kutucuğuna tıklayın.
3. **AzureClassicAutomationTutorialScript** runbook’unu seçin ve ardından **Başlat**’a tıklayarak runbook’u başlatın.  Runbook'u başlatmak istediğinizi doğrulayan bir ileti alacaksınız.
4. Bir [runbook işi](automation-runbook-execution.md) oluşturulur, İş dikey penceresi gösterilir ve iş durumu **İş Özeti** kutucuğunda gösterilir.  
5. İş durumu, bulutta bir runbook çalışanının kullanılabilir hale gelmesinin beklendiğini gösteren şekilde *Sırada* olarak başlar. Ardından, bir çalışan işi talep ettiğinde *Başlatılıyor* olarak ve runbook gerçekten çalışmaya başladığında *Çalışıyor* olarak değiştirilir.  
6. Runbook işi tamamlandığında **Tamamlandı** durumunu görmeniz gerekir.<br> ![Güvenlik Sorumlusu Runbook Testi](media/automation-sec-configure-azure-runas-account/job-summary-automationclassictutorialscript.png)<br>
7. Runbook’un ayrıntılı sonuçlarını görmek için **Çıktı** kutucuğuna tıklayın.
8. **Çıktı** dikey penceresinde kimlik doğrulamasının başarıyla yapıldığını ve abonelikteki tüm klasik sanal makinelerin bir listesini döndürdüğünü görmeniz gerekir. 
9. **Çıktı** dikey penceresini kapatarak **İş Özeti** dikey penceresine geri dönün.
13. **İş Özeti**’ni ve karşılık gelen **AzureClassicAutomationTutorialScript** runbook dikey penceresini kapatın.

## PowerShell kullanarak Automation Hesabını güncelleştirme

Burada aşağıdaki durumlarda var olan Otomasyon hesabınızı güncelleştirmek için PowerShell kullanma seçeneği sunulmaktadır:

1. Bir Otomasyon hesabı oluşturdunuz, ancak Farklı Çalıştır hesabı oluşturmayı reddettiniz 
2. Resource Manager kaynaklarını yönetmek için bir Otomasyon hesabınız zaten var ve runbook kimlik doğrulaması için Farklı Çalıştır hesabını içerecek şekilde güncelleştirmek istiyorsunuz 
2. Klasik kaynakları yönetmek için bir Otomasyon hesabınız zaten var ve yeni bir hesap oluşturup runbook’larınızı ve varlıklarınızı ona geçirmek yerine Klasik Farklı Çalıştır’ı kullanacak şekilde güncelleştirmek istiyorsunuz   

Devam etmeden önce lütfen şunları doğrulayın:

1. Windows 7 çalıştırıyorsanız [Windows Management Framework (WMF) 4.0](https://www.microsoft.com/download/details.aspx?id=40855) uygulamasını indirdiğinizi ve yüklediğinizi.   
    Windows Server 2012 R2, Windows Server 2012, Windows 2008 R2, Windows 8.1, ve Windows 7 SP1 çalıştırıyorsanız [Windows Management Framework 5.0](https://www.microsoft.com/download/details.aspx?id=50395) uygulamasının yüklemeye hazır olduğunu.
2. Azure PowerShell 1.0. Bu sürüm ve nasıl yükleneceği hakkında daha fazla bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](../powershell-install-configure.md). 
3. Bir otomasyon hesabı oluşturduğunuzu.  Bu hesaba aşağıdaki komut dosyasında yer alan –AutomationAccountName ve -ApplicationDisplayName parametre değeri olarak başvurulacaktır.

Komut dosyaları için gerekli parametreler olan *SubscriptionID*, *ResourceGroup* ve *AutomationAccountName* değerlerini almak için Azure portalında **Otomasyon hesabı** dikey penceresinden Otomasyon hesabınızı seçin ve **Tüm ayarlar** seçeneğini belirleyin.  **Tüm ayarlar** dikey penceresindeki **Hesap Ayarları** altında **Özellikler**’i seçin.  **Özellikler** dikey penceresinde bu değerleri fark edebilirsiniz.<br> ![Otomasyon Hesabı özellikleri](media/automation-sec-configure-azure-runas-account/automation-account-properties.png)  

### Farklı Çalıştır Hesabı PowerShell komut dosyası oluşturma

Aşağıdaki PowerShell komut dosyası şunları yapılandırır:

- Otomatik olarak imzalanan sertifikayla kimlik doğrulaması yapılacak Azure AD uygulaması, Azure AD'de bu uygulama için bir hizmet sorumlusu hesabı oluşturun ve geçerli aboneliğinizde bu hesap için Katılımcı rolü (bunu Sahip veya herhangi başka bir rolle değiştirebilirsiniz) atayın.  Daha fazla bilgi için lütfen[Azure Automation’da Rol Tabanlı Erişim Denetimi](../automation/automation-role-based-access-control.md) makalesini inceleyin.
- Hizmet sorumlusu tarafından kullanılan sertifikayı tutan **AzureRunAsCertificate** adlı, belirtilen Otomasyon hesabındaki bir Otomasyon sertifikası varlığı.
- applicationId, tenantId, subscriptionId ve certificate thumbprint öğelerini tutan **AzureRunAsConnection** adlı, belirtilen Automation hesabındaki bir Automation sertifikası varlığı.    

Aşağıdaki adımlar komut dosyası yürütme işleminde size kılavuzluk edecektir.

1. Aşağıdaki betiği bilgisayarınıza kaydedin.  Bu örnekte, **New-AzureServicePrincipal.ps1** dosya adıyla kaydedin.  

        #Requires -RunAsAdministrator
        Param (
        [Parameter(Mandatory=$true)]
        [String] $ResourceGroup,

        [Parameter(Mandatory=$true)]
        [String] $AutomationAccountName,

        [Parameter(Mandatory=$true)]
        [String] $ApplicationDisplayName,

        [Parameter(Mandatory=$true)]
        [String] $SubscriptionId,

        [Parameter(Mandatory=$true)]
        [String] $CertPlainPassword,

        [Parameter(Mandatory=$false)]
        [int] $NoOfMonthsUntilExpired = 12
        )

        Login-AzureRmAccount
        Import-Module AzureRM.Resources
        Select-AzureRmSubscription -SubscriptionId $SubscriptionId

        $CurrentDate = Get-Date
        $EndDate = $CurrentDate.AddMonths($NoOfMonthsUntilExpired)
        $KeyId = (New-Guid).Guid
        $CertPath = Join-Path $env:TEMP ($ApplicationDisplayName + ".pfx")

        $Cert = New-SelfSignedCertificate -DnsName $ApplicationDisplayName -CertStoreLocation cert:\LocalMachine\My -KeyExportPolicy Exportable -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider"

        $CertPassword = ConvertTo-SecureString $CertPlainPassword -AsPlainText -Force
        Export-PfxCertificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $CertPath -Password $CertPassword -Force | Write-Verbose

        $PFXCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate -ArgumentList @($CertPath, $CertPlainPassword)
        $KeyValue = [System.Convert]::ToBase64String($PFXCert.GetRawCertData())

        $KeyCredential = New-Object  Microsoft.Azure.Commands.Resources.Models.ActiveDirectory.PSADKeyCredential
        $KeyCredential.StartDate = $CurrentDate
        $KeyCredential.EndDate= $EndDate
        $KeyCredential.KeyId = $KeyId
        $KeyCredential.Type = "AsymmetricX509Cert"
        $KeyCredential.Usage = "Verify"
        $KeyCredential.Value = $KeyValue

        # Use Key credentials
        $Application = New-AzureRmADApplication -DisplayName $ApplicationDisplayName -HomePage ("http://" + $ApplicationDisplayName) -IdentifierUris ("http://" + $KeyId) -KeyCredentials $keyCredential

        New-AzureRMADServicePrincipal -ApplicationId $Application.ApplicationId | Write-Verbose
        Get-AzureRmADServicePrincipal | Where {$_.ApplicationId -eq $Application.ApplicationId} | Write-Verbose

        $NewRole = $null
        $Retries = 0;
        While ($NewRole -eq $null -and $Retries -le 6)
        {
           # Sleep here for a few seconds to allow the service principal application to become active (should only take a couple of seconds normally)
           Sleep 5
           New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
           Sleep 10
           $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
           $Retries++;
        } 

        # Get the tenant id for this subscription
        $SubscriptionInfo = Get-AzureRmSubscription -SubscriptionId $SubscriptionId
        $TenantID = $SubscriptionInfo | Select TenantId -First 1

        # Create the automation resources
        New-AzureRmAutomationCertificate -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Path $CertPath -Name AzureRunAsCertificate -Password $CertPassword -Exportable | write-verbose

        # Create a Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
        $ConnectionAssetName = "AzureRunAsConnection"
        Remove-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Name $ConnectionAssetName -Force -ErrorAction SilentlyContinue
        $ConnectionFieldValues = @{"ApplicationId" = $Application.ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Cert.Thumbprint; "SubscriptionId" = $SubscriptionId}
        New-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Name $ConnectionAssetName -ConnectionTypeName AzureServicePrincipal -ConnectionFieldValues $ConnectionFieldValues

2. Bilgisayarınızda **Windows PowerShell**’i yükseltilmiş kullanıcı haklarına sahip **Başlat** ekranından başlatın.
3. Yükseltilmiş PowerShell komut satırı kabuğundan, 1. adımda oluşturduğunuz betiğin bulunduğu klasöre gidin, *–ResourceGroup*, *-AutomationAccountName*, *-ApplicationDisplayName*, *-SubscriptionId* ve *-CertPlainPassword* parametrelerinin değerini değiştirerek bu betiği yürütün.<br>

    >[AZURE.NOTE] Betiği yürüttükten sonra Azure’ün kimlik doğrulamasını yapmanız istenecektir. Abonelik Yöneticileri rolünün üyesi ve aboneliğin ortak yöneticisi olan bir hesapla oturum açmanız gerekir.
    
        .\New-AzureServicePrincipal.ps1 -ResourceGroup <ResourceGroupName> 
        -AutomationAccountName <NameofAutomationAccount> `
        -ApplicationDisplayName <DisplayNameofAutomationAccount> `
        -SubscriptionId <SubscriptionId> `
        -CertPlainPassword "<StrongPassword>"  
<br>

Komut dosyası başarıyla tamamlandıktan sonra Resource Manager kaynakları kimlik doğrulaması yapmak ve kimlik bilgisi yapılandırmasını doğrulamak için aşağıdaki [örnek koduna](#sample-code-to-authenticate-with-resource-manager-resources) bakın. 

### Klasik Farklı Çalıştır hesabı PowerShell komut dosyası oluşturma

Aşağıdaki PowerShell komut dosyası şunları yapılandırır:

- Runbook’ların kimliğini doğrulamak için kullanılan sertifikayı tutan **AzureClassicRunAsCertificate** adlı, belirtilen Otomasyon hesabındaki bir Otomasyon sertifikası varlığı.
- Subscription name, subscriptionId ve certificate asset name öğelerini tutan **AzureClassicRunAsConnection** adlı, belirtilen Otomasyon hesabındaki bir Otomasyon bağlantı varlığı.

Komut dosyası otomatik olarak imzalanan bir yönetim sertifikası oluşturur ve bilgisayarınızda PowerShell oturumunu yürütmek için kullanılan kullanıcı profili altındaki geçici dosyalar klasörüne kaydeder - *%USERPROFILE%\AppData\Local\Temp*.  Komut dosyası yürütme sonrasında Otomasyon hesabının oluşturulduğu abonelik için yönetim deposuna Azure yönetim sertifikasını yüklemeniz gerekir.  Aşağıdaki adımlar komut dosyası yürütme ve sertifikayı karşıya yükleme işleminde size kılavuzluk edecektir.  

1. Aşağıdaki betiği bilgisayarınıza kaydedin.  Bu örnekte **New-AzureClassicRunAsAccount.ps1** dosya adıyla kaydedin.

        #Requires -RunAsAdministrator
        Param (
        [Parameter(Mandatory=$true)]
        [String] $ResourceGroup,

        [Parameter(Mandatory=$true)]
        [String] $AutomationAccountName,

        [Parameter(Mandatory=$true)]
        [String] $ApplicationDisplayName,

        [Parameter(Mandatory=$true)]
        [String] $SubscriptionId,

        [Parameter(Mandatory=$true)]
        [String] $CertPlainPassword,

        [Parameter(Mandatory=$false)]
        [int] $NoOfMonthsUntilExpired = 12
        )

        Login-AzureRmAccount
        Import-Module AzureRM.Resources
        $Subscription = Select-AzureRmSubscription -SubscriptionId $SubscriptionId
        $SubscriptionName = $subscription.Subscription.SubscriptionName

        $CurrentDate = Get-Date
        $EndDate = $CurrentDate.AddMonths($NoOfMonthsUntilExpired)
        $KeyId = (New-Guid).Guid
        $CertPath = Join-Path $env:TEMP ($ApplicationDisplayName + ".pfx")
        $CertPathCer = Join-Path $env:TEMP ($ApplicationDisplayName + ".cer")

        $Cert = New-SelfSignedCertificate -DnsName $ApplicationDisplayName -CertStoreLocation cert:\LocalMachine\My -KeyExportPolicy Exportable -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider"

        $CertPassword = ConvertTo-SecureString $CertPlainPassword -AsPlainText -Force
        Export-PfxCertificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $CertPath -Password $CertPassword -Force | Write-Verbose
        Export-Certificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $CertPathCer -Type CERT | Write-Verbose

        # Create the automation resources
        $ClassicCertificateAssetName = "AzureClassicRunAsCertificate"
        New-AzureRmAutomationCertificate -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Path $CertPath -Name $ClassicCertificateAssetName  -Password $CertPassword -Exportable | write-verbose

        # Create a Automation connection asset named AzureClassicRunAsConnection in the Automation account. This connection uses the ClassicCertificateAssetName.
        $ConnectionAssetName = "AzureClassicRunAsConnection"
        Remove-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Name $ConnectionAssetName -Force -ErrorAction SilentlyContinue
        $ConnectionFieldValues = @{"SubscriptionName" = $SubscriptionName; "SubscriptionId" = $SubscriptionId; "CertificateAssetName" = $ClassicCertificateAssetName}
        New-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Name $ConnectionAssetName -ConnectionTypeName AzureClassicCertificate -ConnectionFieldValues $ConnectionFieldValues 

        Write-Host -ForegroundColor red "Please upload the cert $CertPathCer to the Management store by following the steps below."
        Write-Host -ForegroundColor red "Log in to the Microsoft Azure Management portal (https://manage.windowsazure.com) and select Settings -> Management Certificates."
        Write-Host -ForegroundColor red "Then click Upload and upload the certificate $CertPathCer"

2. Bilgisayarınızda **Windows PowerShell**’i yükseltilmiş kullanıcı haklarına sahip **Başlat** ekranından başlatın.  
3. Yükseltilmiş PowerShell komut satırı kabuğundan, 1. adımda oluşturduğunuz betiğin bulunduğu klasöre gidin, *–ResourceGroup*, *-AutomationAccountName*, *-ApplicationDisplayName*, *-SubscriptionId* ve *-CertPlainPassword* parametrelerinin değerini değiştirerek bu betiği yürütün.<br>

    >[AZURE.NOTE] Betiği yürüttükten sonra Azure’ün kimlik doğrulamasını yapmanız istenecektir. Abonelik Yöneticileri rolünün üyesi ve aboneliğin ortak yöneticisi olan bir hesapla oturum açmanız gerekir.
   
        .\New-AzureClassicRunAsAccount.ps1 -ResourceGroup <ResourceGroupName> 
        -AutomationAccountName <NameofAutomationAccount> `
        -ApplicationDisplayName <DisplayNameofAutomationAccount> `
        -SubscriptionId <SubscriptionId> `
        -CertPlainPassword "<StrongPassword>" 

Komut dosyası başarıyla tamamlandıktan sonra kullanıcı profilinizin **Temp** klasöründe oluşturulan sertifikayı kopyalamanız gerekir.  Klasik Azure portalına [yönetim API sertifikası yükleme](../azure-api-management-certs.md) adımlarını izleyin ve ardından [örnek koduna](#sample-code-to-authenticate-with-service-management-resources) bakarak Service Management kaynaklarıyla kimlik doğrulama yapılandırmasını doğrulayın. 

## Resource Manager kaynaklarıyla kimlik doğrulaması için örnek kod

Runbook’larınızla Resource Manager kaynaklarını yönetecek Farklı Çalıştır hesabını kullanarak kimlik doğrulaması yapmak için **AzureAutomationTutorialScript** örnek runbook’undan alınan aşağıdaki güncelleştirilmiş örnek kodu kullanabilirsiniz.   

    $connectionName = "AzureRunAsConnection"
    $SubId = Get-AutomationVariable -Name 'SubscriptionId'
    try
    {
       # Get the connection "AzureRunAsConnection "
       $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         
       
       "Logging in to Azure..."
       Add-AzureRmAccount `
         -ServicePrincipal `
         -TenantId $servicePrincipalConnection.TenantId `
         -ApplicationId $servicePrincipalConnection.ApplicationId `
         -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint 
       "Setting context to a specific subscription"  
       Set-AzureRmContext -SubscriptionId $SubId             
    }
    catch {
        if (!$servicePrincipalConnection)
        {
           $ErrorMessage = "Connection $connectionName not found."
           throw $ErrorMessage
         } else{
            Write-Error -Message $_.Exception
            throw $_.Exception
         }
    } 
   

Bu betikte, abonelik bağlamına başvuruyu desteklemek için iki ek kod satırı bulunmaktadır; böylece birden fazla abonelikte kolayca çalışabilirsiniz. SubscriptionId adlı değişken varlığında aboneliğin kimliği vardır; Add-AzureRmAccount cmdlet’i deyiminden sonra da [Set-AzureRmContext cmdlet’i](https://msdn.microsoft.com/library/mt619263.aspx) *-SubscriptionId* parametre kümesiyle belirtilir. Değişken adı çok genelse, amaçlarınız doğrultusunda tanımlanmasını kolaylaştırmak amacıyla bir önek veya başka diğer adlandırma kuralı eklemek için değişken adını gözden geçirebilirsiniz. Alternatif olarak, ilgili değişken varlığıyla -SubscriptionId yerine -SubscriptionName parametre kümesini kullanabilirsiniz.  

Runbook’ta kimlik doğrulaması için kullanılan cmdlet’in (**Add-AzureRmAccount**) *ServicePrincipalCertificate* parametre kümesini kullandığına dikkat edin.  Kimlik bilgilerini değil, hizmet asıl sertifikasını kullanarak kimlik doğrulamasını yapar.  

## Service Management kaynaklarıyla kimlik doğrulaması için örnek kod

Runbook’larınızla klasik kaynakları yönetecek Klasik Farklı Çalıştır hesabını kullanarak kimlik doğrulaması yapmak için **AzureClassicAutomationTutorialScript** örnek runbook’undan alınan aşağıdaki güncelleştirilmiş örnek kodu kullanabilirsiniz. 
    
    $ConnectionAssetName = "AzureClassicRunAsConnection"
    # Get the connection
    $connection = Get-AutomationConnection -Name $connectionAssetName        
    
    # Authenticate to Azure with certificate
    Write-Verbose "Get connection asset: $ConnectionAssetName" -Verbose
    $Conn = Get-AutomationConnection -Name $ConnectionAssetName
    if ($Conn -eq $null)
    {
       throw "Could not retrieve connection asset: $ConnectionAssetName. Assure that this asset exists in the Automation account."
    }
      
    $CertificateAssetName = $Conn.CertificateAssetName
    Write-Verbose "Getting the certificate: $CertificateAssetName" -Verbose
    $AzureCert = Get-AutomationCertificate -Name $CertificateAssetName
    if ($AzureCert -eq $null)
    {
       throw "Could not retrieve certificate asset: $CertificateAssetName. Assure that this asset exists in the Automation account."
    }
      
    Write-Verbose "Authenticating to Azure with certificate." -Verbose
    Set-AzureSubscription -SubscriptionName $Conn.SubscriptionName -SubscriptionId $Conn.SubscriptionID -Certificate $AzureCert 
    Select-AzureSubscription -SubscriptionId $Conn.SubscriptionID


## Sonraki adımlar

- Hizmet Sorumluları hakkında daha fazla bilgi için bkz. [Uygulama Nesneleri ve Hizmet Sorumlusu Nesneleri](../active-directory/active-directory-application-objects.md).
- Azure Automation’da Rol Tabanlı Erişim Denetimi hakkında daha fazla bilgi için bkz. [Azure Automation’da rol tabanlı erişim denetimi](../automation/automation-role-based-access-control.md).
- Sertifikalar ve Azure hizmetleri hakkında daha fazla bilgi için bkz. [Azure Cloud Services sertifikalarına genel bakış](../cloud-services/cloud-services-certs-create.md)



<!--HONumber=Aug16_HO1-->


