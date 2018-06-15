---
title: Azure yığınında güvenli bir Service Fabric kümesini dağıtma | Microsoft Docs
description: Azure yığınında güvenli bir Service Fabric kümesi dağıtmayı öğrenin
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
ms.date: 05/08/2018
ms.author: mattbriggs
ms.reviewer: shnatara
ms.openlocfilehash: a88d8dd2af94ac796a3b2e3c667fd40a308f02a1
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
ms.locfileid: "33883028"
---
# <a name="deploy-a-service-fabric-cluster-in-azure-stack"></a>Azure yığınında Service Fabric kümesi dağıtma

Kullanım **Service Fabric kümesi** Azure yığınında güvenli bir Service Fabric kümesi dağıtmak için Azure Marketi'nden öğesi. 

Service Fabric ile çalışma hakkında daha fazla bilgi için bkz: [Azure hizmet Frabric genel bakış](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview) ve [Service Fabric kümesi güvenlik senaryoları](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security), Azure belgelerinde.

## <a name="prerequisites"></a>Önkoşullar

Service Fabric kümesi dağıtmak için aşağıdakiler gereklidir:
1. **Küme sertifikası**  
   KeyVault için Service Fabric dağıtırken eklemek X.509 sunucu sertifikasıdır. 
   - **CN** bu sertifikayı üzerinde tam etki alanı adı (FQDN) oluşturduğunuz Service Fabric kümesi ile eşleşmesi gerekir. 
   - Ortak ve özel anahtarlarını gerektiği şekilde sertifika biçimi PFX, olması gerekir. 
   Bkz: [gereksinimleri](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security) bu sunucu tarafı sertifikayı oluşturmak için.

    > [!NOTE]  
    > Test amaçları için otomatik olarak imzalanan sertifika inplace x.509 sunucu sertifikası kullanabilirsiniz. Otomatik olarak imzalanan sertifikalar kümesi FQDN'si eşleşmesi gerekmez.

2.  **Yönetici istemci sertifikası** Bu, istemci otomatik imzalanan Service Fabric kümesi için kimlik doğrulaması için kullanacağınız sertifikadır. Bkz: [gereksinimleri](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security) bu istemci sertifikası oluşturmak için.

3.  **Aşağıdaki öğeler Azure yığın Marketi'nde kullanılabilir olması gerekir:**
     - **Windows Server 2016** – küme oluşturmak için Windows Server 2016 görüntü şablonu kullanır.  
     - **Müşteri betik uzantısı** -Microsoft sanal makine uzantısı.  
     - **PowerShell istenen hazırlamak Yapılandırması** -Microsoft sanal makine uzantısı.


## <a name="add-a-secret-to-key-vault"></a>Key Vault’a gizli dizi ekleme
Service Fabric kümesi dağıtmak için doğru KeyVault belirtin *gizli tanımlayıcı* veya Service Fabric kümesi için URL. Azure Resource Manager şablonu KeyVault giriş olarak alır ve ardından küme sertifika Service Fabric kümesi yükleme sırasında alır. 

> [!IMPORTANT]  
> Service Fabric ile kullanılmak KeyVault bir gizlilik eklemek için PowerShell kullanmanız gerekir. Portal kullanmayın.  

KeyVault oluşturmak ve eklemek için aşağıdaki komut dosyasını kullanın *küme sertifika* ona. (Bkz [Önkoşullar](#prerequisites).) Komut dosyasını çalıştırmadan önce örnek komut dosyasını gözden geçirin ve ortamınıza uyum sağlaması için belirtilen parametreleri güncelleştirin. Bu komut dosyası ayrıca Azure Resource Manager şablonu sağlamanız gereken değerleri çıkarır. 

> [!TIP]  
> Betik başarılı olabilmesi için önce işlem, ağ, depolama ve KeyVault hizmetleri içeren ortak bir teklif olmalıdır. 

  ```PowerShell
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


Daha fazla bilgi için bkz: [yönetmek KeyVault PowerShell ile Azure yığında](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-kv-manage-powershell).

## <a name="deploy-the-marketplace-item"></a>Market öğesi dağıtma

1. Kullanıcı Portalı'nda Git **yeni** > **işlem** > **Service Fabric kümesi**. 

   ![Service Fabric kümesi seçin](./media/azure-stack-solution-template-service-fabric-cluster/image2.png)

2. Her bir sayfa için gibi *Temelleri*, dağıtım formu doldurun. Değerini emin değilseniz varsayılan ayarları kullanın. Seçin **Tamam** sonraki sayfaya geçmek için:

   ![Temel Bilgiler](media/azure-stack-solution-template-service-fabric-cluster/image3.png)

3. Üzerinde *ağ ayarlarını* sayfasında, uygulamalarınız için açmak için belirli bağlantı noktalarını belirtebilirsiniz:

   ![Ağ ayarları](media/azure-stack-solution-template-service-fabric-cluster/image4.png)

4. Üzerinde *güvenlik* sayfasında, gelen aldığınız değerlerini eklemek [Azure KeyVault oluşturma](#add-a-secret-to-key-vault) ve gizli karşıya yükleme.

   İçin *yönetici istemci sertifikası parmak izi*, parmak izini girin *yönetici istemci sertifikası*. (Bkz [Önkoşullar](#prerequisites).)
   
   - Kaynak anahtar kasasının: tüm belirtin *keyVault kimliği* betik sonuçlarından dize. 
   - Küme sertifikası URL'si: tüm URL'yi belirtin *gizli kimliği* betik sonuçlarından. 
   - Sertifika parmak izi küme: belirtin *küme sertifika parmak izi* betik sonuçlarından.
   - Yönetici istemci sertifikası parmak: Belirtin *yönetici istemci sertifikası parmak izi* Önkoşullar oluşturdunuz. 

   ![Betik çıktısı](media/azure-stack-solution-template-service-fabric-cluster/image5.png)

   ![Güvenlik](media/azure-stack-solution-template-service-fabric-cluster/image6.png)

5. Sihirbazı tamamlayın ve ardından **oluşturma** Service Fabric kümesi dağıtmak için.



## <a name="access-the-service-fabric-cluster"></a>Service Fabric kümesi erişim
Service Fabric Explorer veya Service Fabric PowerShell kullanarak Service Fabric kümesi erişebilirsiniz.


### <a name="use-service-fabric-explorer"></a>Service Fabric Explorer kullanın
1.  Web tarayıcısı yönetici istemci sertifikanızın erişimi ve Service Fabric kümenize doğrulanabilir doğrulayın.  

    a. Internet Explorer'ı açın ve gidin **Internet Seçenekleri** > **içerik** > **Sertifikalar**.
  
    b. Sertifika Seç **alma** başlatmak için *Sertifika Alma Sihirbazı'nı*ve ardından **sonraki**. Üzerinde *Alınacak dosya* sayfasında **Gözat**seçip **yönetici istemci sertifikası** Azure Resource Manager şablonu sağlanan.
        
       > [!NOTE]  
       > Bu sertifika KeyVault için önceden eklendi küme sertifika değil.  

    c. "Kişisel bilgi dosya Gezgini penceresi uzantısı açılır listede seçilen Değişimi" olduğundan emin olun.  

       ![Kişisel bilgi değişimi](media/azure-stack-solution-template-service-fabric-cluster/image8.png)  

    d. Üzerinde *sertifika deposu* sayfasında, **kişisel**ve ardından Sihirbazı tamamlayın.  
       ![Sertifika deposu](media/azure-stack-solution-template-service-fabric-cluster/image9.png)  
2. Service Fabric kümesi FQDN'sini bulmak için:  

    a. Service Fabric ile ilişkili kaynak grubu için küme bulun gidip *genel IP adresi* kaynak. Açmak için genel IP adresi ile ilişkili nesneyi seçin *genel IP adresi* dikey.  

      ![Genel IP adresi](media/azure-stack-solution-template-service-fabric-cluster/image10.png)   

    b. Genel IP adresi dikey olarak FQDN görüntüler *DNS adı*.  

      ![DNS adı](media/azure-stack-solution-template-service-fabric-cluster/image11.png)  

3. Service Fabric Explorer ve istemci bağlantı uç noktasının URL'sini bulmak için şablon dağıtımı sonuçlarını gözden geçirin.

4. Tarayıcınızda, https:// için Git*FQDN*: 19080. Değiştir *FQDN* 2. adımda, Service Fabric kümesi FQDN'si ile.   
   Otomatik olarak imzalanan sertifika kullandıysanız, bağlantının güvenli olmadığını bildiren bir uyarı alırsınız. Web sitesine devam etmek için seçmeniz **daha fazla bilgi**ve ardından **Web sayfasına gidin**. 

5. Site için kimlik doğrulaması için kullanılacak bir sertifika seçmeniz gerekir. Seçin **daha fazla seçenek**, uygun sertifikayı seçin ve ardından **Tamam** Service Fabric Gezgini'ne bağlanmak için. 

   ![Kimlik doğrulaması](media/azure-stack-solution-template-service-fabric-cluster/image14.png)



## <a name="use-service-fabric-powershell"></a>Service Fabric PowerShell kullanın

1. Yükleme *Microsoft Azure Service Fabric SDK* gelen [Windows geliştirme ortamınızı hazırlama](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-get-started#install-the-sdk-and-tools) Azure Service Fabric belgelerinde.  

2. Yükleme tamamlandıktan sonra Service Fabric cmdlet'leri Powershell'den erişilebilir olduğundan emin olmak için sistem ortam değişkenlerini yapılandırın.  
    
    a. Git **Denetim Masası** > **sistem ve güvenlik** > **sistem**ve ardından **Gelişmiş Sistem ayarları**.  
    
      ![Denetim Masası](media/azure-stack-solution-template-service-fabric-cluster/image15.png) 

    b. Üzerinde **Gelişmiş** sekmesinde *Sistem Özellikleri*seçin **ortam değişkenleri**.  

    c. İçin *sistem değişkenleri*, düzenleme **yolu** emin olun **C:\\Program Files\\Microsoft Service Fabric\\bin\\yapı \\Fabric.Code** en üstünde ortam değişkenleri listesi yer alır.  

      ![Ortam değişken listesi](media/azure-stack-solution-template-service-fabric-cluster/image16.png)

3. Ortam değişkenleri sırasını değiştirme sonra PowerShell yeniden başlatın ve sonra Service Fabric kümesi erişmek için şu PowerShell betiğini çalıştırın:

   ````PowerShell  
    Connect-ServiceFabricCluster -ConnectionEndpoint "\[Service Fabric
    CLUSTER FQDN\]:19000" \`

    -X509Credential -ServerCertThumbprint
    761A0D17B030723A37AA2E08225CD7EA8BE9F86A \`

    -FindType FindByThumbprint -FindValue
    0272251171BA32CEC7938A65B8A6A553AA2D3283 \`

    -StoreLocation CurrentUser -StoreName My -Verbose
   ````
   
   > [!NOTE]  
   > Var olan hiçbir *https://* betikteki kümenin adını önce. Bağlantı noktası 19000 gereklidir.
 
