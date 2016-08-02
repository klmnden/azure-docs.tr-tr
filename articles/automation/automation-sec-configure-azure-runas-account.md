<properties
    pageTitle="Azure Farklı Çalıştır Hesabını Yapılandırma | Microsoft Azure"
    description="Eğitici, Azure Automation’da güvenlik temel elemanı kimlik doğrulaması oluşturulması, test edilmesi ve örneklerinde size yol göstermektedir."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor=""
    keywords="service principal name, setspn, azure authentication"/>
<tags
    ms.service="automation"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="06/07/2016"
    ms.author="magoedte"/>

# Azure Farklı Çalıştır hesabıyla Kimlik Doğrulaması Runbook’ları
Bu konu başlığı altında, Automation runbook’larıyla aboneliğinizdeki Azure Resource Manager kaynaklarına erişmek için yeni Farklı Çalıştır hesabı özelliğini (hizmet sorumlusu olarak da bilinir) kullanarak Azure portalından Automation hesabının nasıl yapılandırılacağı gösterilmektedir.  Azure portalında yeni bir Automation hesabı oluşturduğunuzda, otomatik olarak yeni bir hizmet sorumlusu oluşturur; varsayılan olarak abonelikte Katılımcı rolü tabanlı erişim denetimi (RBAC) rolüne atanmıştır.  İşlemi sizin için basitleştirir ve otomasyon gerekliliklerini desteklemek için runbook’ları oluşturmaya ve dağıtmaya hemen başlamanıza yardımcı olur.      

Hizmet sorumlusunu kullanarak şunları yapabilirsiniz:

* Runbook'ları kullanılarak Azure Resource Manager kaynakları yönetilirken Azure’ün kimlik doğrulamasını yapmak için standartlaştırılmış bir yol sağlama
* Azure Uyarıları’nda yapılandırılmış genel runbook'ların kullanımını otomatikleştirme


>[AZURE.NOTE] Automation Genel Runbook’larına sahip Azure [Uyarı tümleştirme özelliği](../azure-portal/insights-receive-alert-notifications.md) için hizmet sorumlusuyla yapılandırılmış bir Automation hesabı gerekir. Zaten kullanıcı tanımlı bir hizmet sorumlusuna sahip Automation hesaplarından seçebileceğiniz gibi yeni bir tane de oluşturabilirsiniz.



Hem Azure portalından, hem de Azure PowerShell kullanılarak Farklı Çalıştır hesabıyla hesap güncelleştirmeden Automation hesabı oluşturulmasını ve runbook'larınızdaki bu hizmet sorumlusuyla kimlik doğrulaması yapılmasını göstereceğiz.  

## Azure Portal'dan yeni Automation Hesabı oluşturma
Bu bölümde, yeni bir Azure Automation hesabını ve hizmet sorumlusunu Azure portalından oluşturmak için aşağıdaki adımları gerçekleştireceksiniz.

>[AZURE.NOTE] Bu adımları gerçekleştirecek kullanıcının Abonelik Yöneticileri rolünün üyesi olması *gerekir*.

1. Azure Portalı’nda, yönetmek istediğiniz Azure aboneliği için hizmet yöneticisi olarak oturum açın.
2. **Automation Hesapları**’nı seçin.
3. Automation Hesapları dikey penceresinde **Ekle**’ye tıklayın.<br>![Automation Hesabı ekleme](media/automation-sec-configure-azure-runas-account/add-automation-acct-properties.png)
4. **Automation Hesabı Ekle** dikey penceresinde, **Ad** kutusuna, yeni Automation hesabınız için bir ad yazın.
5. Birden fazla aboneliğiniz varsa, yeni hesap için, mevcut veya yeni **Kaynak grubu** ve Azure veri merkezi **konumu** ile birlikte bir tane belirtin.
6. **Azure Farklı Çalıştır hesabı oluştur** seçeneği için **Evet** değerinin seçili olduğunu doğrulayın ve **Oluştur** düğmesine tıklayın.  

    ![Automation Hesabı Uyarısı ekleme](media/automation-sec-configure-azure-runas-account/add-account-decline-create-runas-msg.png)

    >[AZURE.NOTE] **Hayır** seçeneğini belirleyerek Farklı Çalıştır hesabı oluşturmamayı seçerseniz, **Automation Hesabı Ekle** dikey penceresinde bir uyarı iletisi görürsünüz.  Hesap oluşturulup abonelikteki **Katılımcı** rolüne atandığı sırada, abonelikler dizini hizmetinizde karşılık gelen bir kimlik doğrulaması kimliği olmaz, bu nedenle de aboneliğinizde kaynaklara erişim de olmaz.  Azure Resource Manager kaynaklarına karşı kimlik doğrulama ve görev gerçekleştirme becerisinden bu hesaba başvuran runbook’ları koruyacaktır.

    ![Automation Hesabı Uyarısı ekleme](media/automation-sec-configure-azure-runas-account/add-automation-acct-properties-error.png)

    >[AZURE.NOTE] **Oluştur** düğmesine tıkladıktan sonra iznin reddedildiği hata iletisini alırsanız, bu nedeni hesabınızın Abonelik yöneticileri rolünün üyesi olmamasıdır.  

7. Azure Automation hesabını oluşturduğu sırada menünün **Bildirimler** öğesi altında ilerleme durumunu izleyebilirsiniz.

### Kaynaklar dahil
Otomasyon hesabının oluşturulması tamamlandığında, bazı kaynaklar sizin için otomatik olarak oluşturulur. Bunlar aşağıdaki tabloda özetlenmiştir.

Kaynak|Açıklama 
----|----
AzureAutomationTutorial Runbook|Farklı Çalıştır hesabı kullanılarak kimlik doğrulaması yapmayı gösteren ve aboneliğinizde ilk 10 Azure VM’yi görüntüleyen bir örnek runbook.
AzureRunAsCertificate|Automation hesabı oluşturulurken Farklı Çalıştır hesabının oluşturulmasını seçtiyseniz oluşturulan ya da mevcut hesap için aşağıdaki PowerShell betiği kullanılarak oluşturulan sertifika varlığı.  Bu sertifikanın bir yıllık kullanım ömrü vardır. 
AzureRunAsConnection|Automation hesabı oluşturulurken Farklı Çalıştır hesabının oluşturulmasını seçtiyseniz oluşturulan ya da mevcut hesap için aşağıdaki PowerShell betiği kullanılarak oluşturulan bağlantı varlığı.
Modules|Runbook’larınızda hemen kullanılmaya başlaması için Azure, PowerShell ve Automation cmdlet'lerine sahip 15 modül.  

## PowerShell kullanarak Automation Hesabını güncelleştirme
Aşağıdaki yordam, mevcut Automation hesabını güncelleştirir ve PowerShell kullanarak hizmet sorumlusu oluşturur.  Bu yordam, bir hesap oluşturursanız, ancak Farklı Çalıştır hesabı oluşturmanız reddedilirse gerekir.

Devam etmeden önce lütfen şunları doğrulayın:

1. Windows 7 çalıştırıyorsanız [Windows Management Framework (WMF) 4.0](https://www.microsoft.com/download/details.aspx?id=40855) uygulamasını indirdiğinizi ve yüklediğinizi.   
    Windows Server 2012 R2, Windows Server 2012, Windows 2008 R2, Windows 8.1, ve Windows 7 SP1 çalıştırıyorsanız [Windows Management Framework 5.0](https://www.microsoft.com/download/details.aspx?id=50395) uygulamasının yüklemeye hazır olduğunu.
2. Azure PowerShell 1.0. Bu sürüm ve nasıl yükleneceği hakkında daha fazla bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](../powershell-install-configure.md). 
3. Bir otomasyon hesabı oluşturduğunuzu.  Bu hesaba aşağıdaki betikte yer alan –AutomationAccountName ve -ApplicationDisplayName parametre değeri olarak başvurulacaktır.


PowerShell betiği şunları yapılandırır:

* Otomatik olarak imzalanan sertifikayla kimlik doğrulaması yapılacak Azure AD uygulaması, Azure AD'de bu uygulama için bir hizmet sorumlusu hesabı oluşturun ve geçerli aboneliğinizde bu hesap için Katılımcı rolü (bunu Sahip veya herhangi başka bir rolle değiştirebilirsiniz) atayın.  Daha fazla bilgi için lütfen[Azure Automation’da Rol Tabanlı Erişim Denetimi](../automation/automation-role-based-access-control.md) makalesini inceleyin.  
* Hizmet sorumlusunda kullanılan sertifikayı tutan **AzureRunAsCertificate** adlı, belirtilen Automation hesabındaki bir Automation sertifikası varlığı.
* applicationId, tenantId, subscriptionId ve certificate thumbprint öğelerini tutan **AzureRunAsConnection** adlı, belirtilen Automation hesabındaki bir Automation sertifikası varlığı.  


### PowerShell betiğini çalıştırma

1. Aşağıdaki betiği bilgisayarınıza kaydedin.  Bu örnekte, **New-AzureServicePrincipal.ps1** dosya adıyla kaydedin.  

    ```
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
    While ($NewRole -eq $null -and $Retries -le 2)
    {
      # Sleep here for a few seconds to allow the service principal application to become active (should only take a couple of seconds normally)
      Sleep 5
      New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
      Sleep 5
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
    $ConnectionFieldValues = @{"ApplicationId" = $Application.ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Cert.Thumbprint; "SubscriptionId" = $SubscriptionId.SubscriptionId}
    New-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Name $ConnectionAssetName -ConnectionTypeName AzureServicePrincipal -ConnectionFieldValues $ConnectionFieldValues
    ```
<br>
2. Bilgisayarınızda **Windows PowerShell**’i yükseltilmiş kullanıcı haklarına sahip **Başlat** ekranından başlatın.
3. Yükseltilmiş PowerShell komut satırı kabuğundan, 1. adımda oluşturduğunuz betiğin bulunduğu klasöre gidin, *–ResourceGroup*, *-AutomationAccountName*, *-ApplicationDisplayName*, *-SubscriptionId* ve *-CertPlainPassword* parametrelerinin değerini değiştirerek bu betiği yürütün.<br>

    ```
    .\New-AzureServicePrincipal.ps1 -ResourceGroup <ResourceGroupName> 
     -AutomationAccountName <NameofAutomationAccount> `
     -ApplicationDisplayName <DisplayNameofAutomationAccount> `
     -SubscriptionId <SubscriptionId> `
     -CertPlainPassword "<StrongPassword>"
    ```   
<br>

    >[AZURE.NOTE] Betiği yürüttükten sonra Azure’ün kimlik doğrulamasını yapmanız istenecektir.  Abonelikte Hizmet yöneticisi olan bir hesapla oturum açmanız *gerekir*.  
<br>
4. Betik başarıyla tamamlandıktan sonra, yeni kimlik bilgileri yapılandırmasını test etmek ve doğrulamak için sonraki bölüme geçin.

### Kimliği doğrulama
Yeni hizmet sorumlusunu kullanarak kimlik doğrulamasını sorunsuz yaptığınızı doğrulamak için burada küçük bir testle devam ediyoruz. Kimlik doğrulamasını sorunsuz yapamıyorsanız, 1. adıma dönüp önceki her önceki adımı yeniden onaylayın.    

1. Azure Portal'da, daha önce oluşturduğunuz Automation hesabını açın.  
2. Runbook'ların listesini açmak için **Runbook'lar** kutucuğuna tıklayın.
3. **Runbook ekle** düğmesine tıklayın ve ardından **Runbook Ekle** dikey penceresinde **Yeni bir runbook oluştur**’u seçerek yeni bir runbook oluşturun.
4. Runbook’a *Test SecPrin Runbook* adını verin ve **Runbook Türü** için PowerShell’i seçin.  Runbook oluşturmak için **Oluştur**’a tıklayın.
5. **PowerShell Runbook'unu Düzenle** dikey penceresinde tuvale aşağıdaki kodu yapıştırın:<br>

    ```
     $Conn = Get-AutomationConnection -Name AzureRunAsConnection 
     Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
     -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
    ```  
<br>
6. **Kaydet**’e tıklayarak runbook’u kaydedin.
7. **Test** dikey penceresini açmak için **Test bölmesi**’ne tıklayın.
8. Testi başlatmak için **Başlat**’a tıklayın.
9. Bölmede bir [runbook işi](automation-runbook-execution.md) oluşturulur ve durumu görüntülenir.  
10. İş durumu, bulutta bir runbook çalışanının kullanılabilir hale gelmesinin beklendiğini gösteren şekilde *Sırada* olarak başlar. Ardından, bir çalışan işi talep ettiğinde *Başlatılıyor* olarak ve runbook gerçekten çalışmaya başladığında *Çalışıyor* olarak değiştirilir.  
11. Runbook işi tamamlandığında çıktısı görüntülenir. Bu durumda, **Tamamlandı** durumunu görmemiz gerekir.<br> ![Güvenlik Sorumlusu Runbook Testi](media/automation-sec-configure-azure-runas-account/runbook-test-results.png)<br>
12. Tuvale geri dönmek için **Test** dikey penceresini kapatın.
13. **PowerShell Runbook'unu Düzenle** dikey penceresini kapatın.
14. **Test-SecPrin-Runbook** dikey penceresini kapatın.

## Resource Manager kaynaklarıyla kimlik doğrulaması için örnek kod

Runbook’larınızla Resource Manager kaynaklarını yönetecek Farklı Çalıştır hesabını kullanarak kimlik doğrulaması yapmak için AzureAutomationTutorial örnek runbook’undan alınan aşağıdaki güncelleştirilmiş örnek kodu kullanabilirsiniz. 

   ```
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
   ```

Bu betikte, abonelik bağlamına başvuruyu desteklemek için iki ek kod satırı bulunmaktadır; böylece birden fazla abonelikte kolayca çalışabilirsiniz. SubscriptionId adlı değişken varlığında aboneliğin kimliği vardır; Add-AzureRmAccount cmdlet’i deyiminden sonra da [Set-AzureRmContext cmdlet’i](https://msdn.microsoft.com/library/mt619263.aspx) *-SubscriptionId* parametre kümesiyle belirtilir. Değişken adı çok genelse, amaçlarınız doğrultusunda tanımlanmasını kolaylaştırmak amacıyla bir önek veya başka diğer adlandırma kuralı eklemek için değişken adını gözden geçirebilirsiniz. Alternatif olarak, ilgili değişken varlığıyla -SubscriptionId yerine -SubscriptionName parametre kümesini kullanabilirsiniz.  

## Sonraki Adımlar
- Hizmet Sorumluları hakkında daha fazla bilgi için bkz. [Uygulama Nesneleri ve Hizmet Sorumlusu Nesneleri](../active-directory/active-directory-application-objects.md).
- Azure Automation’da Rol Tabanlı Erişim Denetimi hakkında daha fazla bilgi için bkz. [Azure Automation’da rol tabanlı erişim denetimi](../automation/automation-role-based-access-control.md).



<!---HONumber=Jun16_HO2-->


