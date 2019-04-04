---
title: Azure stack'teki güvenli bir Service Fabric kümesine dağıtma | Microsoft Docs
description: Azure stack'teki güvenli bir Service Fabric kümesi dağıtmayı öğrenin
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/25/2019
ms.author: mabrigg
ms.reviewer: shnatara
ms.lastreviewed: 01/25/2019
ms.openlocfilehash: 8041e7e02b117b8938f0f7c18da2d57c31dddb34
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58482273"
---
# <a name="deploy-a-service-fabric-cluster-in-azure-stack"></a>Azure Stack'te bir Service Fabric kümesine dağıtma

Kullanım **Service Fabric kümesi** Azure Stack'te güvenli bir Service Fabric kümesini dağıtmak için Azure Market'ten öğesi. 

Service Fabric ile çalışma hakkında daha fazla bilgi için bkz. [Azure Service Fabric genel bakış](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview) ve [Service Fabric kümesi güvenlik senaryoları](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security), Azure belgeleri.

Azure Stack Service Fabric kümesinde kaynak sağlayıcısı Microsoft.ServiceFabric kullanmıyor. Bunun yerine, Azure Stack'te Service Fabric kümesi bir sanal makine ölçek kümesi Desired State Configuration ' nı (DSC) kullanarak önceden yüklenmiş yazılım kümesiyle ' dir.

## <a name="prerequisites"></a>Önkoşullar

Service Fabric kümesine dağıtmak için aşağıdakiler gereklidir:
1. **Küme sertifikası**  
   Anahtar kasası için Service Fabric dağıtırken ekleme X.509 sunucu sertifikasıdır. 
   - **CN** üzerinde bu sertifika, tam etki alanı adı (FQDN) oluşturduğunuz Service Fabric kümesinin eşleşmelidir. 
   - Ortak ve özel anahtarlar gerektiğinde, PFX sertifika biçimi olmalıdır. 
     Bkz: [gereksinimleri](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security) bu sunucu tarafı sertifika oluşturmak için.

     > [!NOTE]  
     > Test amaçları için x.509 sunucu sertifikasının bir otomatik olarak imzalanan sertifika yerinde kullanabilirsiniz. Otomatik olarak imzalanan sertifikalar kümenin FQDN'sini eşleşmesi gerekmez.

1. **Yönetici istemci sertifikası** bu istemci otomatik imzalı Service Fabric kümesi için kimlik doğrulaması için kullanacağı sertifikadır. Bkz: [gereksinimleri](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security) bu istemci sertifikası oluşturmak için.

1. **Aşağıdaki öğeler Azure Stack Market'te kullanılabilir olmalıdır:**
    - **Windows Server 2016** – kümeyi oluşturmak için Windows Server 2016 görüntüsü şablonu kullanır.  
    - **Müşteri betik uzantısını** -Microsoft gelen sanal makine uzantısı.  
    - **PowerShell istenen aşama yapılandırma** -Microsoft gelen sanal makine uzantısı.


## <a name="add-a-secret-to-key-vault"></a>Key Vault’a gizli dizi ekleme
Bir Service Fabric kümesi dağıtmayı doğru anahtar kasası belirtin *gizli dizi tanımlayıcısı* veya Service Fabric kümesi için URL. Azure Resource Manager şablonu bir KeyVault girdi olarak alır. Ardından şablonu, Service Fabric kümesine yüklerken küme sertifikası alır.

> [!IMPORTANT]  
> Service Fabric ile kullanılmak için KeyVault gizli dizi eklemek için PowerShell kullanmanız gerekir. Portal kullanmayın.  

Anahtar kasası oluşturmak ve eklemek için aşağıdaki betiği kullanın *küme sertifikası* ona. (Bkz [önkoşulları](#prerequisites).) Betiği çalıştırmadan önce örnek komut dosyasını gözden geçirin ve ortamınızla eşleşecek şekilde belirtilen parametreleri güncelleştirin. Bu betik ayrıca Azure Resource Manager şablonu için sağlamanız gereken değerleri çıkarır. 

> [!TIP]  
> Betik başarılı olabilmesi için önce bilgi işlem, ağ, depolama ve KeyVault hizmetleri içeren genel bir teklif olmalıdır. 

  ```powershell
    function Get-ThumbprintFromPfx($PfxFilePath, $Password) 
        {
            return New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($PfxFilePath, $Password)
        }
    
    function Publish-SecretToKeyVault ($PfxFilePath, $Password, $KeyVaultName)
       {
            $keyVaultSecretName = "ClusterCertificate"
            $certContentInBytes = [io.file]::ReadAllBytes($PfxFilePath)
            $pfxAsBase64EncodedString = [System.Convert]::ToBase64String($certContentInBytes)
    
            $jsonObject = ConvertTo-Json -Depth 10 ([pscustomobject]@{
                data     = $pfxAsBase64EncodedString
                dataType = 'pfx'
                password = $Password
            })
    
            $jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
            $jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)
            $secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText -Force
            $keyVaultSecret = Set-AzureKeyVaultSecret -VaultName $KeyVaultName -Name $keyVaultSecretName -SecretValue $secret
            
            $pfxCertObject = Get-ThumbprintFromPfx -PfxFilePath $PfxFilePath -Password $Password
    
            Write-Host "KeyVault id: " -ForegroundColor Green
            (Get-AzureRmKeyVault -VaultName $KeyVaultName).ResourceId
            
            Write-Host "Secret Id: " -ForegroundColor Green
            (Get-AzureKeyVaultSecret -VaultName $KeyVaultName -Name $keyVaultSecretName).id
    
            Write-Host "Cluster Certificate Thumbprint: " -ForegroundColor Green
            $pfxCertObject.Thumbprint
       }
    
    #========================== CHANGE THESE VALUES ===============================
    $armEndpoint = "https://management.local.azurestack.external"
    $tenantId = "your_tenant_ID"
    $location = "local"
    $clusterCertPfxPath = "Your_path_to_ClusterCert.pfx"
    $clusterCertPfxPassword = "Your_password_for_ClusterCert.pfx"
    #==============================================================================
    
    Add-AzureRmEnvironment -Name AzureStack -ARMEndpoint $armEndpoint
    Login-AzureRmAccount -Environment AzureStack -TenantId $tenantId
    
    $rgName = "sfvaultrg"
    Write-Host "Creating Resource Group..." -ForegroundColor Yellow
    New-AzureRmResourceGroup -Name $rgName -Location $location
    
    Write-Host "Creating Key Vault..." -ForegroundColor Yellow
    $Vault = New-AzureRmKeyVault -VaultName sfvault -ResourceGroupName $rgName -Location $location -EnabledForTemplateDeployment -EnabledForDeployment -EnabledForDiskEncryption
    
    Write-Host "Publishing certificate to Vault..." -ForegroundColor Yellow
    Publish-SecretToKeyVault -PfxFilePath $clusterCertPfxPath -Password $clusterCertPfxPassword -KeyVaultName $vault.VaultName
   ``` 


Daha fazla bilgi için [yönetme anahtar kasası PowerShell ile Azure Stack'te](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-key-vault-manage-powershell).

## <a name="deploy-the-marketplace-item"></a>Market öğesi dağıtma

1. Kullanıcı Portalı'nda Git **+ kaynak Oluştur** > **işlem** > **Service Fabric kümesi**. 

   ![Service Fabric kümesi seçin](./media/azure-stack-solution-template-service-fabric-cluster/image2.png)

1. Her sayfa için gibi *Temelleri*, dağıtım formu doldurun. Değerini emin değilseniz varsayılan ayarları kullanın. Seçin **Tamam** sonraki sayfaya ilerlemek için:

   ![Temel Bilgiler](media/azure-stack-solution-template-service-fabric-cluster/image3.png)

1. Üzerinde *ağ ayarlarını* sayfasında, uygulamalarınız için açmak için belirli bağlantı noktaları belirleyebilirsiniz:

   ![Ağ Ayarları](media/azure-stack-solution-template-service-fabric-cluster/image4.png)

1. Üzerinde *güvenlik* sayfasında, aldığınız değerleri eklemek [Azure anahtar kasası oluşturma](#add-a-secret-to-key-vault) ve gizli dizi karşıya yükleniyor.

   İçin *yönetici istemci sertifikası parmak izi*, parmak izini girin *yönetici istemci sertifikası*. (Bkz [önkoşulları](#prerequisites).)
   
   - Kaynak Key Vault:  Tüm belirtin *keyVault kimliği* betik sonuçlarını dizeden. 
   - Küme sertifikası URL'si: Tüm URL'den belirtin *gizli kimliği* betik sonuçlarından. 
   - Küme sertifikası parmak izi: Belirtin *küme sertifikası parmak izi* betik sonuçlarından.
   - Yönetici istemci sertifikası parmak izi: Belirtin *yönetici istemci sertifikası parmak izi* önkoşullarda oluşturulur. 

   ![Betik çıktısı](media/azure-stack-solution-template-service-fabric-cluster/image5.png)

   ![Güvenlik](media/azure-stack-solution-template-service-fabric-cluster/image6.png)

1. Sihirbazı tamamlayın ve ardından **Oluştur** Service Fabric kümesi dağıtmayı.



## <a name="access-the-service-fabric-cluster"></a>Service Fabric kümenize erişim
Service Fabric PowerShell veya Service Fabric Explorer'ı kullanarak Service Fabric kümesine erişebilirsiniz.


### <a name="use-service-fabric-explorer"></a>Service Fabric Explorer kullanacaksınız
1.  Web tarayıcısı yönetici istemci sertifikanızın erişebilir ve Service Fabric kümenize doğrulanabilir doğrulayın.  

    a. Internet Explorer'ı açın ve gidin **Internet Seçenekleri** > **içerik** > **sertifikaları**.
  
    b. Sertifika Seç **alma** başlatmak için *Sertifika Alma Sihirbazı*ve ardından **sonraki**. Üzerinde *içeri aktarılacak dosya* sayfasında **Gözat**seçip **yönetici istemci sertifikası** Azure Resource Manager şablonu için sağlanan.
        
       > [!NOTE]  
       > Bu sertifika için anahtar kasası daha önceden eklenmiş küme sertifikası değil.  

    c. "Kişisel bilgi dosya Gezgini penceresinin uzantısı açılan menüde seçtiğiniz Değişimi" olduğundan emin olun.  

       ![Kişisel bilgi değişimi](media/azure-stack-solution-template-service-fabric-cluster/image8.png)  

    d. Üzerinde *sertifika Store* sayfasında **kişisel**ve ardından Sihirbazı tamamlayın.  
       ![Sertifika deposu](media/azure-stack-solution-template-service-fabric-cluster/image9.png)  
1. Service Fabric kümenizi FQDN'sini bulmak için:  

    a. Bulun ve küme, Service Fabric ile ilişkili kaynak grubuna gidin *genel IP adresi* kaynak. Açmak için genel IP adresi ile ilişkili nesneyi seçin *genel IP adresi* dikey penceresi.  

      ![Genel IP adresi](media/azure-stack-solution-template-service-fabric-cluster/image10.png)   

    b. Genel IP adresi dikey penceresinde, FQDN olarak görüntüler. *DNS adı*.  

      ![DNS adı](media/azure-stack-solution-template-service-fabric-cluster/image11.png)  

1. Service Fabric Explorer ve istemci bağlantı uç noktası URL'sini bulmak için şablon dağıtımının sonuçlarını gözden geçirin.

1. Tarayıcınızda, https:// Git*FQDN*: 19080. Değiştirin *FQDN* 2. adım, Service Fabric kümenizden FQDN'si ile.   
   Kendinden imzalı bir sertifika kullandıysanız, bağlantının güvenli olmadığını bildiren bir uyarı alırsınız. Web sitesine devam etmek için seçmeniz **daha fazla bilgi**, ardından **Web sayfasına gidin**. 

1. Site için kimlik doğrulaması için kullanılacak bir sertifika seçmeniz gerekir. Seçin **daha fazla seçenek**uygun sertifikayı seçin ve ardından **Tamam** Service Fabric Explorer'a bağlanmak için. 

   ![Kimlik doğrulaması](media/azure-stack-solution-template-service-fabric-cluster/image14.png)



## <a name="use-service-fabric-powershell"></a>Service Fabric PowerShell kullanma

1. Yükleme *Microsoft Azure Service Fabric SDK'sı* gelen [Windows üzerinde geliştirme ortamınızı hazırlama](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started#install-the-sdk-and-tools) Azure Service Fabric belgelerinde.  

1. Yükleme tamamlandıktan sonra Service Fabric cmdlet'leri PowerShell üzerinden erişilebilir olmasını sağlamak için sistem ortam değişkenlerini yapılandırın.  
    
    a. Git **Denetim Masası** > **sistem ve güvenlik** > **sistem**ve ardından **Gelişmiş Sistem ayarları**.  
    
      ![Denetim Masası](media/azure-stack-solution-template-service-fabric-cluster/image15.png) 

    b. Üzerinde **Gelişmiş** sekmesinde *Sistem Özellikleri*seçin **ortam değişkenlerini**.  

    c. İçin *sistem değişkenlerini*, Düzen **yolu** emin olun **C:\\Program dosyaları\\Microsoft Service Fabric\\bin\\Fabric \\Fabric.Code** ortam değişkenlerinin bir listesini üstte.  

      ![Ortam değişken listesi](media/azure-stack-solution-template-service-fabric-cluster/image16.png)

1. Ortam değişkenlerini sırasını değiştirme sonra PowerShell yeniden başlatın ve ardından Service Fabric kümesine erişmek için aşağıdaki PowerShell betiğini çalıştırın:

   ```powershell  
    Connect-ServiceFabricCluster -ConnectionEndpoint "\[Service Fabric
    CLUSTER FQDN\]:19000" \`

    -X509Credential -ServerCertThumbprint
    761A0D17B030723A37AA2E08225CD7EA8BE9F86A \`

    -FindType FindByThumbprint -FindValue
    0272251171BA32CEC7938A65B8A6A553AA2D3283 \`

    -StoreLocation CurrentUser -StoreName My -Verbose
   ```
   
   > [!NOTE]  
   > Var olan hiçbir *https://* betiğinde kümesinin addan önce. 19000 bağlantı noktası gereklidir.

## <a name="next-steps"></a>Sonraki adımlar

[Kubernetes için Azure Stack dağıtma](azure-stack-solution-template-kubernetes-deploy.md)
