---
title: "Bir şablondan bir Azure Service Fabric kümesi oluştur | Microsoft Docs"
description: "Bu makalede, Azure Resource Manager, Azure anahtar kasası ve Azure Active Directory (Azure AD) istemci kimlik doğrulaması için kullanarak azure'da güvenli bir Service Fabric kümesi ayarlamak açıklar."
services: service-fabric
documentationcenter: .net
author: chackdan
manager: timlt
editor: chackdan
ms.assetid: 15d0ab67-fc66-4108-8038-3584eeebabaa
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/22/2017
ms.author: chackdan
ms.openlocfilehash: 47152d05eb7e31e7fe1f35e33a10fe8e903e21e2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-service-fabric-cluster-by-using-azure-resource-manager"></a>Azure Kaynak Yöneticisi'ni kullanarak bir Service Fabric kümesi oluştur
> [!div class="op_single_selector"]
> * [Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
> * [Azure portal](service-fabric-cluster-creation-via-portal.md)
>
>

Bu adım adım kılavuz, Azure Kaynak Yöneticisi'ni kullanarak azure'da güvenli bir Azure Service Fabric kümesi ayarlama aracılığıyla açıklanmaktadır. Makaleyi uzun olduğunu kabul ediyorsunuz. İçerikle baştan sona bilginiz sürece, yine de, her adım dikkatle izlediğinizden emin olun.

Kılavuzu, aşağıdaki yordamları içerir:

* Küme ve uygulama güvenliği için sertifikaları karşıya yüklemek için bir Azure anahtar kasası ayarlama
* Azure Resource Manager kullanarak Azure'da güvenli küme oluşturma
* Kullanıcıların Azure Active Directory (Azure AD) kullanarak küme yönetimi için kimlik doğrulaması

Güvenli bir küme yönetimi işlemleri için yetkisiz erişimi engelleyen bir kümedir. Bu, dağıtma, yükseltme ve uygulamaları, hizmetleri ve içerdikleri veriler silme içerir. Güvenli olmayan bir küme herkes herhangi bir zamanda bağlanmak ve böylelikle yönetim işlemleri bir kümedir. Güvenli olmayan bir küme oluşturmak mümkün olsa da, projenin başından itibaren güvenli bir küme oluşturmak önerilir. Güvenli olmayan bir küme daha sonra korunamıyor olduğundan, yeni bir küme oluşturulması gerekir.

Güvenli küme oluşturma kavramı, Linux oldukları ya da Windows kümeleri aynıdır. Güvenli Linux kümeleri oluşturmak için daha fazla bilgi ve yardımcı komut dosyası için bkz: [Linux'ta güvenli küme oluşturma](#secure-linux-clusters).

## <a name="sign-in-to-your-azure-account"></a>Azure hesabınızda oturum açma
Bu kılavuzu kullanır [Azure PowerShell][azure-powershell]. Yeni bir PowerShell oturumu başlattığınızda, Azure hesabınızda oturum açın ve Azure komutları çalıştırmadan önce aboneliğinizi seçin.

Azure hesabınızda oturum açın:

```powershell
Login-AzureRmAccount
```

Aboneliğinizi seçin:

```powershell
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

## <a name="set-up-a-key-vault"></a>Bir anahtar kasasını oluşturup
Bu bölümde, Service Fabric uygulamaları ve Azure Service Fabric kümesi için bir anahtar kasası oluşturma açıklanmaktadır. Azure anahtar kasası tam bir kılavuz için başvurmak [anahtar kasası kullanmaya başlama Kılavuzu][key-vault-get-started].

Service Fabric X.509 sertifikaları güvenli bir küme ve uygulama güvenlik özellikleri sağlamak için kullanır. Anahtar kasası, Azure Service Fabric kümeleri sertifikalarını yönetmek için kullanın. Bir küme Azure'da dağıtıldığında, Service Fabric kümeleri oluşturmaktan sorumlu Azure kaynak sağlayıcısı sertifikaları anahtar Kasası'nı çeker ve VM'ler kümede yükler.

Aşağıdaki diyagramda Azure anahtar kasası, Service Fabric kümesi ve küme oluşturduğunda, bir anahtar kasasında depolanan sertifika kullanan Azure kaynak sağlayıcısı arasındaki ilişki gösterilmektedir:

![Sertifika Yükleme diyagramı][cluster-security-cert-installation]

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
İlk adım, özellikle anahtar kasanız için bir kaynak grubu oluşturmaktır. Anahtar kasası, kendi kaynak grubuna koymak öneririz. Bu eylem, Service Fabric kümesi anahtarları ve gizli anahtarları kaybetmeden içeren kaynak grubunu da dahil olmak üzere işlem ve depolama kaynak grupları kaldırmanıza olanak sağlar. Anahtar kasanızı içeren kaynak grubunu _aynı bölgede olmalıdır_ tarafından kullanıldığı kümesi olarak.

Birden çok bölgeye kümelerde dağıtmayı planlıyorsanız, kaynak grubu ve anahtar ait hangi bölgede gösterir şekilde kasa adını öneririz.  

```powershell

    New-AzureRmResourceGroup -Name westus-mykeyvault -Location 'West US'
```
Çıktı aşağıdaki gibi görünmelidir:

```powershell

    WARNING: The output object type of this cmdlet is going to be modified in a future release.

    ResourceGroupName : westus-mykeyvault
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/<guid>/resourceGroups/westus-mykeyvault

```
<a id="new-key-vault"></a>

### <a name="create-a-key-vault-in-the-new-resource-group"></a>Yeni kaynak grubunda bir anahtar kasası oluşturma
Anahtar kasası _dağıtımı için etkinleştirilmelidir_ sertifikaları elde ve sanal makine örneklerinde yüklemek işlem kaynak sağlayıcısı izin vermek için:

```powershell

    New-AzureRmKeyVault -VaultName 'mywestusvault' -ResourceGroupName 'westus-mykeyvault' -Location 'West US' -EnabledForDeployment

```

Çıktı aşağıdaki gibi görünmelidir:

```powershell

    Vault Name                       : mywestusvault
    Resource Group Name              : westus-mykeyvault
    Location                         : West US
    Resource ID                      : /subscriptions/<guid>/resourceGroups/westus-mykeyvault/providers/Microsoft.KeyVault/vaults/mywestusvault
    Vault URI                        : https://mywestusvault.vault.azure.net
    Tenant ID                        : <guid>
    SKU                              : Standard
    Enabled For Deployment?          : False
    Enabled For Template Deployment? : False
    Enabled For Disk Encryption?     : False
    Access Policies                  :
                                       Tenant ID                :    <guid>
                                       Object ID                :    <guid>
                                       Application ID           :
                                       Display Name             :    
                                       Permissions to Keys      :    get, create, delete, list, update, import, backup, restore
                                       Permissions to Secrets   :    all


    Tags                             :
```
<a id="existing-key-vault"></a>

## <a name="use-an-existing-key-vault"></a>Var olan bir anahtar kasası kullanın

Var olan bir anahtar kasası kullanmak için _dağıtımı için etkinleştirmeniz gerekir_ sertifikaları elde ve küme düğümlerine yüklemek işlem kaynak sağlayıcısı izin vermek için:

```powershell

Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment

```

<a id="add-certificate-to-key-vault"></a>

## <a name="add-certificates-to-your-key-vault"></a>Sertifikalar, anahtar Kasası'na ekleme

Sertifikalar, Service Fabric’te bir küme ile uygulamalarının çeşitli yönlerini güvenli hale getirmek üzere kimlik doğrulaması ve şifreleme sağlamak için kullanılır. Service Fabric sertifikaların nasıl kullanıldığını daha fazla bilgi için bkz: [Service Fabric kümesi güvenlik senaryoları][service-fabric-cluster-security].

### <a name="cluster-and-server-certificate-required"></a>Küme ve sunucu sertifikası (gerekli)
Bu sertifika, bir küme güvenli ve yetkisiz erişimi önlemek için gereklidir. İki yolla küme güvenlik sağlar:

* Küme kimlik doğrulaması: düğümü düğümü iletişimin küme Federasyon kimlik doğrulamasını yapar. Yalnızca bu sertifikayla kimliğini kanıtlamak düğümleri kümeye katılmasını sağlayabilir.
* Sunucu kimlik doğrulaması: yönetim istemcisi, gerçek kümeye Konuşmayı bilir böylece bir yönetim istemcisi küme yönetim Uç noktalara kimliğini doğrular. Bu sertifika aynı zamanda bir SSL HTTPS yönetim API'si ve Service Fabric Explorer için HTTPS üzerinden sağlar.

Bu amaca hizmet eder için sertifikanın aşağıdaki gereksinimleri karşılamalıdır:

* Sertifika bir özel anahtar içermelidir.
* Sertifikanın bir kişisel bilgi değişimi (.pfx) dosyası verilebilir anahtar değişimi için oluşturulmuş olması gerekir.
* Sertifikanın konu adı, Service Fabric kümesi erişmek için kullandığınız etki alanı eşleşmesi gerekir. Bu eşleşen bir SSL kümenin HTTPS yönetim uç noktaları ve Service Fabric Explorer için sağlamak için gereklidir. İçin bir sertifika yetkilisinden (CA) bir SSL sertifikası alınamıyor. cloudapp.azure.com etki alanı. Özel etki alanı adı, kümeniz için edinmeniz gerekir. Bir CA'dan bir sertifika isteme, sertifikanın konu adı, kümeniz için kullandığınız özel etki alanı adı eşleşmelidir.

### <a name="application-certificates-optional"></a>Uygulama sertifikaları (isteğe bağlı)
Ek sertifikaların herhangi bir sayıda uygulama güvenlik amacıyla bir kümede yüklenebilir. Kümenizi oluşturmadan önce düğümlerine gibi yüklenmesi için bir sertifika gerektiren uygulama güvenlik senaryoları göz önünde bulundurun:

* Şifreleme ve şifre çözme uygulama yapılandırma değerlerini.
* Çoğaltma sırasında şifreleme düğümler arasında veri.

### <a name="formatting-certificates-for-azure-resource-provider-use"></a>Azure kaynak sağlayıcısı kullanmak için sertifikaları biçimlendirme
Ekleme ve özel anahtar dosyaları (.pfx) doğrudan aracılığıyla anahtar kasanızı kullanın. Ancak, Azure işlem kaynak sağlayıcısı anahtarlarının özel bir JavaScript nesne gösterimi (JSON) biçiminde depolanmasını gerektirir. Biçim .pfx dosyası base 64 kodlu dize ve özel anahtar parolası olarak içerir. Bu gereksinimleri karşılamak için anahtarları bir JSON dizesinde yerleştirilen ve gerekir anahtar Kasası'nda "gizli" olarak depolanır.

Bu işlemi kolaylaştırmak için bir [PowerShell modülüdür Github'da bulunan][service-fabric-rp-helpers]. Modülü kullanmak için aşağıdakileri yapın:

1. Depodaki tüm içeriğini bir yerel dizine indirin.
2. Yerel dizinine gidin.
2. PowerShell penceresinde ServiceFabricRPHelpers modülünü içeri aktarın:

```powershell

 Import-Module "C:\..\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"

```

`Invoke-AddCertToKeyVault` Bu PowerShell modülünü komutu otomatik olarak bir sertifika özel anahtarı bir JSON dizeye biçimlendirir ve anahtar Kasası'na yükler. Küme sertifikası ve herhangi bir ek uygulama sertifika anahtar Kasası'na eklemek için komutunu kullanın. Kümenizdeki yüklemek istediğiniz ek sertifika için bu adımı yineleyin.

#### <a name="uploading-an-existing-certificate"></a>Varolan bir sertifikayı karşıya yükleme

```powershell

 Invoke-AddCertToKeyVault -SubscriptionId <guid> -ResourceGroupName westus-mykeyvault -Location "West US" -VaultName mywestusvault -CertificateName mycert -Password "<password>" -UseExistingCertificate -ExistingPfxFilePath "C:\path\to\mycertkey.pfx"

```

Burada, gösterildiği gibi bir hata alırsanız, genellikle kaynak URL çakışması olduğu anlamına gelir. Çakışmayı çözmek için anahtar kasasının adını değiştirin.

```
Set-AzureKeyVaultSecret : The remote name could not be resolved: 'westuskv.vault.azure.net'
At C:\Users\chackdan\Documents\GitHub\Service-Fabric\Scripts\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1:440 char:11
+ $secret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $Certif ...
+           ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : CloseError: (:) [Set-AzureKeyVaultSecret], WebException
    + FullyQualifiedErrorId : Microsoft.Azure.Commands.KeyVault.SetAzureKeyVaultSecret

```

Çakışma çözüldükten sonra çıktı aşağıdaki gibi görünmelidir:

```

    Switching context to SubscriptionId <guid>
    Ensuring ResourceGroup westus-mykeyvault in West US
    WARNING: The output object type of this cmdlet is going to be modified in a future release.
    Using existing value mywestusvault in West US
    Reading pfx file from C:\path\to\key.pfx
    Writing secret to mywestusvault in vault mywestusvault


Name  : CertificateThumbprint
Value : E21DBC64B183B5BF355C34C46E03409FEEAEF58D

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/westus-mykeyvault/providers/Microsoft.KeyVault/vaults/mywestusvault

Name  : CertificateURL
Value : https://mywestusvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47

```

>[!NOTE]
>Üç önceki dizeler, CertificateThumbprint, SourceVault ve CertificateURL, güvenli bir Service Fabric kümesi ayarlayın ve uygulama güvenliği için kullanıyor olabilecek herhangi bir uygulama sertifika edinmek için gerekir. Dizeleri kaydetmezseniz, daha sonra anahtar kasası sorgulayarak almak zor olabilir.

<a id="add-self-signed-certificate-to-key-vault"></a>

#### <a name="creating-a-self-signed-certificate-and-uploading-it-to-the-key-vault"></a>Otomatik olarak imzalanan sertifika oluşturma ve anahtar Kasası'na karşıya yükleme

Anahtar Kasası'na sertifikalarınızı zaten karşıya yüklediyseniz, bu adımı atlayın. Bu adım, yeni bir otomatik olarak imzalanan sertifika oluşturma ve anahtar kasanızı karşıya bağlıdır. Aşağıdaki komut parametreleri değiştirin ve ardından çalıştırdıktan sonra sertifika parola sorulur.  

```powershell

$ResourceGroup = "chackowestuskv"
$VName = "chackokv2"
$SubID = "6c653126-e4ba-42cd-a1dd-f7bf96ae7a47"
$locationRegion = "westus"
$newCertName = "chackotestcertificate1"
$dnsName = "www.mycluster.westus.mydomain.com" #The certificate's subject name must match the domain used to access the Service Fabric cluster.
$localCertPath = "C:\MyCertificates" # location where you want the .PFX to be stored

 Invoke-AddCertToKeyVault -SubscriptionId $SubID -ResourceGroupName $ResourceGroup -Location $locationRegion -VaultName $VName -CertificateName $newCertName -CreateSelfSignedCertificate -DnsName $dnsName -OutputPath $localCertPath

```

Burada, gösterildiği gibi bir hata alırsanız, genellikle kaynak URL çakışması olduğu anlamına gelir. Çakışmayı çözmek için anahtar kasasının adını, RG adı ve benzeri değiştirin.

```
Set-AzureKeyVaultSecret : The remote name could not be resolved: 'westuskv.vault.azure.net'
At C:\Users\chackdan\Documents\GitHub\Service-Fabric\Scripts\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1:440 char:11
+ $secret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $Certif ...
+           ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : CloseError: (:) [Set-AzureKeyVaultSecret], WebException
    + FullyQualifiedErrorId : Microsoft.Azure.Commands.KeyVault.SetAzureKeyVaultSecret

```

Çakışma çözüldükten sonra çıktı aşağıdaki gibi görünmelidir:

```
PS C:\Users\chackdan\Documents\GitHub\Service-Fabric\Scripts\ServiceFabricRPHelpers> Invoke-AddCertToKeyVault -SubscriptionId $SubID -ResourceGroupName $ResouceGroup -Location $locationRegion -VaultName $VName -CertificateName $newCertName -Password $certPassword -CreateSelfSignedCertificate -DnsName $dnsName -OutputPath $localCertPath
Switching context to SubscriptionId 6c343126-e4ba-52cd-a1dd-f8bf96ae7a47
Ensuring ResourceGroup chackowestuskv in westus
WARNING: The output object type of this cmdlet will be modified in a future release.
Creating new vault westuskv1 in westus
Creating new self signed certificate at C:\MyCertificates\chackonewcertificate1.pfx
Reading pfx file from C:\MyCertificates\chackonewcertificate1.pfx
Writing secret to chackonewcertificate1 in vault westuskv1


Name  : CertificateThumbprint
Value : 96BB3CC234F9D43C25D4B547sd8DE7B569F413EE

Name  : SourceVault
Value : /subscriptions/6c653126-e4ba-52cd-a1dd-f8bf96ae7a47/resourceGroups/chackowestuskv/providers/Microsoft.KeyVault/vaults/westuskv1

Name  : CertificateURL
Value : https://westuskv1.vault.azure.net:443/secrets/chackonewcertificate1/ee247291e45d405b8c8bbf81782d12bd

```

>[!NOTE]
>Üç önceki dizeler, CertificateThumbprint, SourceVault ve CertificateURL, güvenli bir Service Fabric kümesi ayarlayın ve uygulama güvenliği için kullanıyor olabilecek herhangi bir uygulama sertifika edinmek için gerekir. Dizeleri kaydetmezseniz, daha sonra anahtar kasası sorgulayarak almak zor olabilir.

 Bu noktada, aşağıdaki öğeleri yerinde olmalıdır:

* Anahtar kasası kaynak grubu.
* Anahtar kasası ve URL'sini (önceki PowerShell çıkışı çağrılan SourceVault).
* Küme sunucu kimlik doğrulama sertifikası ve anahtar Kasası'nda, URL.
* Uygulama sertifikaları ve bunların URL'lerini anahtar Kasası'nda.


<a id="add-AAD-for-client"></a>

## <a name="set-up-azure-active-directory-for-client-authentication"></a>İstemci kimlik doğrulaması için Azure Active Directory'yi ayarlama ayarlayın

Azure AD (kiracılar da bilinir), kuruluşların uygulamalar kullanıcı erişimini yönetmenizi sağlar. Uygulamaları web tabanlı olanlar oturum açma kullanıcı Arabirimi ve yerel istemci deneyimini olanlar ayrılır. Bu makalede, bir kiracı zaten oluşturduğunuzu varsayalım. Yüklemediyseniz, okuyarak başlamanız [Azure Active Directory kiracısı alma][active-directory-howto-tenant].

Web tabanlı dahil olmak üzere Yönetim işlevselliğini birkaç giriş noktalarını bir Service Fabric kümesi sunar [Service Fabric Explorer] [ service-fabric-visualizing-your-cluster] ve [Visual Studio] [ service-fabric-manage-application-in-visual-studio]. Sonuç olarak, Küme erişimi denetlemek için iki Azure AD uygulama, bir web uygulaması ve bir yerel uygulama oluşturun.

Bazı yapılandırma Azure AD'de bir Service Fabric kümesi ile ilgili adımları basitleştirmek için Windows PowerShell komut kümesini oluşturduk.

> [!NOTE]
> Kümeyi oluşturmadan önce aşağıdaki adımları tamamlamanız gerekir. Küme adları ve uç noktaları betikleri beklediğiniz çünkü değerler planlanması gerekir ve, önceden oluşturduğunuz değerleri değil.

1. [Komut dosyası yükleme] [ sf-aad-ps-script-download] bilgisayarınıza.
2. Zip dosyası sağ tıklayın, **özellikleri**seçin **Engellemeyi Kaldır** onay kutusunu işaretleyin ve ardından **Uygula**.
3. Zip dosyasını ayıklayın.
4. Çalıştırma `SetupApplications.ps1`, Tenantıd, ClusterName ve WebApplicationReplyUrl parametre olarak girin. Örneğin:

    ```powershell
    .\SetupApplications.ps1 -TenantId '690ec069-8200-4068-9d01-5aaf188e557a' -ClusterName 'mycluster' -WebApplicationReplyUrl 'https://mycluster.westus.cloudapp.azure.com:19080/Explorer/index.html'
    ```

    PowerShell komutunu yürüterek, Tenantıd bulabilirsiniz `Get-AzureSubscription`. Bu komutu yürütmek her abonelik için Tenantıd görüntüler.

    ClusterName, komut dosyası tarafından oluşturulan Azure AD uygulamaları öneki için kullanılır. Gerçek küme adı tam olarak eşleşmesi gerekmez. Yalnızca Azure AD yapılarını bunlar ile kullanılan Service Fabric kümesi eşlemek kolaylaştırmak için tasarlanmıştır.

    WebApplicationReplyUrl oturum açma işlemini tamamladıktan sonra kullanıcılarınıza Azure AD döndüren varsayılan uç noktadır. Bu uç nokta olan varsayılan olarak, kümeniz için Service Fabric Explorer uç noktası olarak ayarlayın:

    https://&lt;cluster_domain&gt;: 19080/Explorer

    Azure AD kiracısı için yönetici ayrıcalıklarına sahip bir hesapla oturum açmak için istenir. Oturum açtıktan sonra Service Fabric kümesi temsil etmek için yerel uygulamalar ve web komut dosyası oluşturur. Kiracının uygulamaları bakarsanız [Klasik Azure portalı][azure-classic-portal], iki yeni giriş görmeniz gerekir:

   * *ClusterName*\_küme
   * *ClusterName*\_istemci

   Komut dosyası PowerShell penceresi açık tutmak için iyi bir fikirdir için sonraki bölümde, kümeyi oluşturduğunuzda Azure Resource Manager şablonu tarafından gerekli JSON yazdırır.

```json
"azureActiveDirectory": {
  "tenantId":"<guid>",
  "clusterApplication":"<guid>",
  "clientApplication":"<guid>"
},
```

## <a name="create-a-service-fabric-cluster-resource-manager-template"></a>Service Fabric kümesi Resource Manager şablonu oluşturma
Bu bölümde, önceki PowerShell komutlarını çıkışları bir Service Fabric kümesi Resource Manager şablonunda kullanılır.

Örnek Resource Manager şablonları kullanılabilir [github'da Azure hızlı başlangıç Şablon Galerisi][azure-quickstart-templates]. Bu şablonlar, küme şablonunuz için bir başlangıç noktası olarak kullanılabilir.

### <a name="create-the-resource-manager-template"></a>Resource Manager şablonu oluşturma
Bu kılavuzu kullanır [5-node güvenli küme] [ service-fabric-secure-cluster-5-node-1-nodetype] örnek şablonu ve şablon parametreleri. Karşıdan `azuredeploy.json` ve `azuredeploy.parameters.json` bilgisayarınıza ve her iki dosyalarını sık kullandığınız metin düzenleyicisinde açın.

### <a name="add-certificates"></a>Sertifikaları Ekle
Sertifika anahtarlarını içeren anahtar kasası başvurarak sertifikaları bir küme Resource Manager şablonuna ekleyin. Anahtar kasası değerlerin bir Resource Manager şablonu parametreleri dosyasına yerleştirin öneririz. Bunun yapılması Resource Manager şablon dosyası yeniden kullanılabilir ve değerlerin boş belirli bir dağıtımına tutar.

#### <a name="add-all-certificates-to-the-virtual-machine-scale-set-osprofile"></a>Sanal makine ölçek kümesi osProfile tüm sertifikaları Ekle
Kümeye yüklü her sertifikanın ölçek kümesi kaynağı (Microsoft.Compute/virtualMachineScaleSets) osProfile bölümünde yapılandırılması gerekir. Bu eylem, sanal makinelerin sertifikayı yüklemek için kaynak sağlayıcısı bildirir. Bu yükleme, küme sertifika ve uygulamalarınız için kullanmayı planladığınız herhangi bir uygulama güvenlik sertifika içerir:

```json
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "osProfile": {
      ...
      "secrets": [
        {
          "sourceVault": {
            "id": "[parameters('sourceVaultValue')]"
          },
          "vaultCertificates": [
            {
              "certificateStore": "[parameters('clusterCertificateStorevalue')]",
              "certificateUrl": "[parameters('clusterCertificateUrlValue')]"
            },
            {
              "certificateStore": "[parameters('applicationCertificateStorevalue')",
              "certificateUrl": "[parameters('applicationCertificateUrlValue')]"
            },
            ...
          ]
        }
      ]
    }
  }
}
```

#### <a name="configure-the-service-fabric-cluster-certificate"></a>Service Fabric kümesi sertifika yapılandırma
Kümenin kimlik doğrulama sertifikası hem Service Fabric küme kaynağı olarak (Microsoft.ServiceFabric/clusters) yapılandırılması gerekir ve sanal makine ölçek kümesi kaynak sanal makine ölçek için Service Fabric uzantısı ayarlar. Bu düzenlemenin, küme kimlik doğrulaması ve yönetim uç noktaları için sunucu kimlik doğrulaması kullanmak için yapılandırmak Service Fabric kaynak sağlayıcısı sağlar.

##### <a name="virtual-machine-scale-set-resource"></a>Kaynak sanal makine ölçek kümesi:
```json
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "virtualMachineProfile": {
      "extensionProfile": {
        "extensions": [
          {
            "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
            "properties": {
              ...
              "settings": {
                ...
                "certificate": {
                  "thumbprint": "[parameters('clusterCertificateThumbprint')]",
                  "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
                },
                ...
              }
            }
          }
        ]
      }
    }
  }
}
```

##### <a name="service-fabric-resource"></a>Service Fabric kaynak:
```json
{
  "apiVersion": "2016-03-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  "location": "[parameters('clusterLocation')]",
  "dependsOn": [
    "[concat('Microsoft.Storage/storageAccounts/', variables('supportLogStorageAccountName'))]"
  ],
  "properties": {
    "certificate": {
      "thumbprint": "[parameters('clusterCertificateThumbprint')]",
      "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
    },
    ...
  }
}
```

### <a name="insert-azure-ad-configuration"></a>Azure AD Yapılandırması Ekle
Daha önce oluşturduğunuz Azure AD yapılandırma doğrudan Resource Manager şablonuna eklenebilir. Ancak, ilk değerleri Resource Manager şablonu yeniden kullanılabilir tutmak ve bir dağıtım için belirli değerlerin boş bir parametre dosyası içine ayıklamanız önerilir.

```json
{
  "apiVersion": "2016-03-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  ...
  "properties": {
    "certificate": {
      "thumbprint": "[parameters('clusterCertificateThumbprint')]",
      "x509StoreName": "[parameters('clusterCertificateStorevalue')]"
    },
    ...
    "azureActiveDirectory": {
      "tenantId": "[parameters('aadTenantId')]",
      "clusterApplication": "[parameters('aadClusterApplicationId')]",
      "clientApplication": "[parameters('aadClientApplicationId')]"
    },
    ...
  }
}
```

### < bir "yapılandırma-arm" ></a>yapılandırma Resource Manager şablonu parametreleri
<!--- Loc Comment: It seems that <a "configure-arm" > must be replaced with <a name="configure-arm"></a> since the link seems not to be redirecting correctly --->
Son olarak, Parametreler dosyası doldurmak için çıktı değerler anahtar kasası ve Azure AD PowerShell komutlarını kullanın:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        ...
        "clusterCertificateStoreValue": {
            "value": "My"
        },
        "clusterCertificateThumbprint": {
            "value": "<thumbprint>"
        },
        "clusterCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myclustercert/4d087088df974e869f1c0978cb100e47"
        },
        "applicationCertificateStorevalue": {
            "value": "My"
        },
        "applicationCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myapplicationcert/2e035058ae274f869c4d0348ca100f08"
        },
        "sourceVaultvalue": {
            "value": "/subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault"
        },
        "aadTenantId": {
            "value": "<guid>"
        },
        "aadClusterApplicationId": {
            "value": "<guid>"
        },
        "aadClientApplicationId": {
            "value": "<guid>"
        },
        ...
    }
}
```
Bu noktada, aşağıdaki öğeleri yerinde olmalıdır:

* Anahtar kasası kaynak grubu
  * Key Vault
  * Küme sunucu kimlik doğrulama sertifikası
  * Veri şifreleme sertifikası
* Azure Active Directory kiracısı
  * Web tabanlı yönetim ve Service Fabric Explorer için Azure AD uygulaması
  * Yerel istemci yönetimi için Azure AD uygulaması
  * Kullanıcılar ve onların atanan roller
* Service Fabric kümesi Resource Manager şablonu
  * Anahtar kasası ile yapılandırılan sertifikaları
  * Yapılandırılmış Azure Active Directory

Aşağıdaki diyagram, anahtar kasası ve Azure AD yapılandırma Resource Manager şablonunuza nerelerde gösterir.

![Resource Manager bağımlılık Haritası][cluster-security-arm-dependency-map]

## <a name="create-the-cluster"></a>Kümeyi oluşturma
Şimdi kullanarak küme oluşturmak için hazır [Azure kaynak şablon dağıtımı][resource-group-template-deploy].

#### <a name="test-it"></a>test
Resource Manager şablonu ile bir parametre dosyası sınamak için aşağıdaki PowerShell komutunu kullanın:

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

#### <a name="deploy-it"></a>dağıtın
Resource Manager şablonu test geçerse, Resource Manager şablonu ile bir parametre dosyası dağıtmak için aşağıdaki PowerShell komutunu kullanın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

<a name="assign-roles"></a>

## <a name="assign-users-to-roles"></a>Kullanıcıları rollere atama
Kümenizin temsil etmek üzere uygulamalar oluşturduktan sonra kullanıcılarınızın Service Fabric tarafından desteklenen rolleri atayın: salt okunur ve yönetici Kullanarak roller atayabilirsiniz [Klasik Azure portalı][azure-classic-portal].

1. Azure portalında kiracınıza gidin ve ardından **uygulamaları**.
2. Bir ada sahip web uygulaması seçin gibi `myTestCluster_Cluster`.
3. Tıklatın **kullanıcılar** sekmesi.
4. Atayın ve ardından bir kullanıcı seçin **atamak** ekranın altındaki düğmesini.

    ![Kullanıcı rolleri düğmesi atama][assign-users-to-roles-button]
5. Kullanıcıya atamak için rol seçin.

    !["Kullanıcılar atama" iletişim kutusu][assign-users-to-roles-dialog]

> [!NOTE]
> Service Fabric rolleri hakkında daha fazla bilgi için bkz: [Service Fabric istemciler için rol tabanlı erişim denetimi](service-fabric-cluster-security-roles.md).
>
>

 <a name="secure-linux-clusters"></a>
 <!--- Loc Comment: It seems that letter S in cluster was missing, which caused the wrong redirection of the link --->

## <a name="create-secure-clusters-on-linux"></a>Linux'ta güvenli küme oluşturma
İşlemini kolaylaştırmak için sağladık bir [yardımcı betik](http://github.com/ChackDan/Service-Fabric/tree/master/Scripts/CertUpload4Linux). Bu yardımcı betik kullanmadan önce Azure komut satırı yüklü arabirimi (CLI) zaten var ve yolunuzda olduğundan emin olun. Komut dosyası çalıştırarak yürütmek için izinlere sahip olduğundan emin olun `chmod +x cert_helper.py` karşıdan sonra. CLI ile kullanarak Azure hesabınızda oturum açmak için ilk adımdır `azure login` komutu. Azure hesabınızda oturum açtıktan sonra yardımcı komut dosyası aşağıdaki komut gösterildiği gibi CA imzalı bir sertifika ile kullanın:

```sh
./cert_helper.py [-h] CERT_TYPE [-ifile INPUT_CERT_FILE] [-sub SUBSCRIPTION_ID] [-rgname RESOURCE_GROUP_NAME] [-kv KEY_VAULT_NAME] [-sname CERTIFICATE_NAME] [-l LOCATION] [-p PASSWORD]
```

-İfile parametresi bir .pfx dosyasına ya da .pem dosyasını (pfx veya pem ya da kendinden imzalı bir sertifika varsa ss) sertifika türü ile giriş olarak alabilir.
Parametre -h Yardım metni yazdırır.


Bu komut, aşağıdaki üç dizeleri çıktısı olarak döndürür:

* Yeni KeyVault sizin için oluşturulan kaynak grubu için kimliği SourceVaultID
* Sertifika erişmek için CertificateUrl
* Kimlik doğrulaması için kullanılan CertificateThumbprint

Aşağıdaki örnek komutunun nasıl kullanılacağını gösterir:

```sh
./cert_helper.py pfx -sub "fffffff-ffff-ffff-ffff-ffffffffffff"  -rgname "mykvrg" -kv "mykevname" -ifile "/home/test/cert.pfx" -sname "mycert" -l "East US" -p "pfxtest"
```
Yukarıdaki komut yürütme, üç dizeler gibi sunar:

```sh
SourceVault: /subscriptions/fffffff-ffff-ffff-ffff-ffffffffffff/resourceGroups/mykvrg/providers/Microsoft.KeyVault/vaults/mykvname
CertificateUrl: https://myvault.vault.azure.net/secrets/mycert/00000000000000000000000000000000
CertificateThumbprint: 0xfffffffffffffffffffffffffffffffffffffffff
```

Sertifikanın konu adı, Service Fabric kümesi erişmek için kullandığınız etki alanı eşleşmesi gerekir. Bu eşleme kümenin HTTPS yönetim uç noktaları ve Service Fabric Explorer için bir SSL sağlamak için gereklidir. Bir CA için bir SSL sertifikası elde edemiyor `.cloudapp.azure.com` etki alanı. Özel etki alanı adı, kümeniz için edinmeniz gerekir. Bir CA'dan bir sertifika isteme, sertifikanın konu adı, kümeniz için kullandığınız özel etki alanı adı eşleşmelidir.

Bu konu adları (olmadan Azure AD), güvenli bir Service Fabric kümesi oluşturmanız gerekir konusunda açıklandığı gibi girişler [yapılandırma Resource Manager şablonu parametreleri](#configure-arm). Yönergeleri izleyerek güvenli kümeye bağlanabilir [bir küme için istemci erişimi kimlik doğrulaması](service-fabric-connect-to-secure-cluster.md). Linux kümeleri, Azure AD kimlik doğrulamasını desteklemez. Bölümünde açıklandığı gibi yönetici ve istemci roller atayabilirsiniz [kullanıcılara roller atama](#assign-roles) bölümü. Linux kümesi için yönetim ve istemci rolleri belirttiğinizde, kimlik doğrulaması için sertifika parmak izlerini sağlamanız gerekiyor. Hiçbir zincir doğrulama veya iptal gerçekleştirilmekte olduğundan konu adı sağlamaz.

Test etmek için otomatik olarak imzalanan bir sertifika kullanmak istiyorsanız, bir oluşturmak için aynı komut dosyasını kullanabilirsiniz. Bayrak sağlayarak sertifika anahtar kasanızı sonra yükleyebilirsiniz `ss` sertifika adı ve sertifika yolu sağlayan yerine. Örneğin, oluşturma ve otomatik olarak imzalanan bir sertifika karşıya yükleme için aşağıdaki komutu bakın:

```sh
./cert_helper.py ss -rgname "mykvrg" -sub "fffffff-ffff-ffff-ffff-ffffffffffff" -kv "mykevname"   -sname "mycert" -l "East US" -p "selftest" -subj "mytest.eastus.cloudapp.net"
```
Bu komut aynı üç dizeleri döndürür: SourceVault, CertificateUrl ve CertificateThumbprint. Dizeler ardından güvenli bir Linux kümesi ve otomatik olarak imzalanan sertifika yerleştirildiği konum oluşturmak için de kullanabilirsiniz. Kümeye bağlanmak için kendinden imzalı bir sertifika gerekir. Yönergeleri izleyerek güvenli kümeye bağlanabilir [bir küme için istemci erişimi kimlik doğrulaması](service-fabric-connect-to-secure-cluster.md).

Sertifikanın konu adı, Service Fabric kümesi erişmek için kullandığınız etki alanı eşleşmesi gerekir. Bu eşleme kümenin HTTPS yönetim uç noktaları ve Service Fabric Explorer için bir SSL sağlamak için gereklidir. Bir CA için bir SSL sertifikası elde edemiyor `.cloudapp.azure.com` etki alanı. Özel etki alanı adı, kümeniz için edinmeniz gerekir. Bir CA'dan bir sertifika isteme, sertifikanın konu adı, kümeniz için kullandığınız özel etki alanı adı eşleşmelidir.

Azure portalında yardımcı komut parametreleri açıklandığı gibi doldurabilirsiniz [Azure portalında bir küme oluşturmak](service-fabric-cluster-creation-via-portal.md#create-cluster-in-the-azure-portal) bölümü.

## <a name="next-steps"></a>Sonraki adımlar
Bu noktada, Azure Active Directory sağlayan yönetim kimlik doğrulama ile güvenli bir küme var. Ardından, [kümenize bağlanmak](service-fabric-connect-to-secure-cluster.md) ve öğrenin nasıl [uygulama parolaları yönetme](service-fabric-application-secret-management.md).

## <a name="troubleshoot-setting-up-azure-active-directory-for-client-authentication"></a>İstemci kimlik doğrulaması için Azure Active Directory ayarlayan sorun giderme
İstemci kimlik doğrulaması için Azure AD ayarlarken bir sorunu yaşayıp çalıştırırsanız, bu bölümdeki olası çözümleri gözden geçirin.

### <a name="service-fabric-explorer-prompts-you-to-select-a-certificate"></a>Service Fabric Explorer bir sertifika seçmenizi ister
#### <a name="problem"></a>Sorun
Başarılı bir şekilde Azure ad Service Fabric Explorer'da oturum açtıktan sonra tarayıcı giriş sayfasına döndürüyor ancak bir ileti bir sertifika seçmenizi ister.

![SFX select sertifika iletişim kutusu][sfx-select-certificate-dialog]

#### <a name="reason"></a>Neden
Kullanıcı bir rolde Azure AD küme uygulaması atanmamış. Bu nedenle, Service Fabric kümesi üzerinde Azure AD kimlik doğrulaması başarısız olur. Service Fabric Explorer sertifika kimlik doğrulaması için geri döner.

#### <a name="solution"></a>Çözüm
Azure AD ayarlamak için yönergeleri izleyin ve kullanıcı rolleri atayın. Ayrıca, "uygulamaya erişim için gerekli kullanıcı ataması üzerinde" Aç olarak öneririz `SetupApplications.ps1` yapar.

### <a name="connection-with-powershell-fails-with-an-error-the-specified-credentials-are-invalid"></a>PowerShell ile bağlantı bir hata ile başarısız olur: "belirtilen kimlik bilgileri geçersiz"
#### <a name="problem"></a>Sorun
Azure AD ile başarıyla oturum açtıktan sonra "AzureActiveDirectory" güvenlik modunu kullanarak kümeye bağlanmak için PowerShell kullandığınızda, bağlantı bir hata ile başarısız olur: "belirtilen kimlik bilgileri geçersiz."

#### <a name="solution"></a>Çözüm
Bu çözüm, önceki bir ile aynıdır.

### <a name="service-fabric-explorer-returns-a-failure-when-you-sign-in-aadsts50011"></a>Service Fabric Explorer oturum açarken bir hata döndürür: "AADSTS50011"
#### <a name="problem"></a>Sorun
Service Fabric Explorer Azure AD'de oturum açmaya çalıştığınızda hata sayfasını döndürür: "AADSTS50011: yanıt adresini &lt;url&gt; uygulama için yapılandırılan yanıt adresleri eşleşmiyor: &lt;GUID&gt;."

![SFX yanıt adresi eşleşmiyor][sfx-reply-address-not-match]

#### <a name="reason"></a>Neden
Service Fabric Explorer gösteren küme (web) uygulaması karşı Azure AD kimlik doğrulama girişiminde ve isteğin bir parçası yeniden yönlendirme dönüş URL'SİYLE sağlar. Ancak URL Azure AD uygulama içinde listelenmeyen **yanıt URL'si** listesi.

#### <a name="solution"></a>Çözüm
Üzerinde **yapılandırma** sekmesini küme (web) uygulamasının URL'si, Service Fabric Gezgini'ne ekleme **yanıt URL'si** listelemek veya listedeki öğelerden birini değiştirin. İşiniz bittiğinde, değişikliğinizi kaydedin.

![Web uygulaması yanıtı URL'si][web-application-reply-url]

### <a name="connect-the-cluster-by-using-azure-ad-authentication-via-powershell"></a>PowerShell aracılığıyla Azure AD kimlik doğrulaması kullanarak kümesine bağlanın
Service Fabric kümesi bağlanmak için aşağıdaki PowerShell komut örneği kullanın:

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <endpoint> -KeepAliveIntervalInSec 10 -AzureActiveDirectory -ServerCertThumbprint <thumbprint>
```

Connect-ServiceFabricCluster cmdlet hakkında bilgi edinmek için [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx).

### <a name="can-i-reuse-the-same-azure-ad-tenant-in-multiple-clusters"></a>Birden çok küme aynı Azure AD kiracısında yeniden kullanabilir?
Evet. Ancak Service Fabric Explorer URL küme (web) uygulamanıza eklemeyi unutmayın. Aksi takdirde, Service Fabric Explorer işe yaramaz.

### <a name="why-do-i-still-need-a-server-certificate-while-azure-ad-is-enabled"></a>Azure AD etkinken neden hala bir sunucu sertifikası ihtiyacım var mı?
FabricClient ve FabricGateway karşılıklı kimlik doğrulaması gerçekleştirir. Azure AD kimlik doğrulama sırasında sunucuya bir istemci kimliği Azure AD tümleştirme sağlar ve sunucu sertifikası, sunucu kimliğini doğrulamak için kullanılır. Service Fabric sertifikalar hakkında daha fazla bilgi için bkz: [X.509 sertifikalarını ve Service Fabric][x509-certificates-and-service-fabric].

<!-- Links -->
[azure-powershell]:https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[key-vault-get-started]:../key-vault/key-vault-get-started.md
[aad-graph-api-docs]:https://msdn.microsoft.com/library/azure/ad/graph/api/api-catalog
[azure-classic-portal]: https://manage.windowsazure.com
[service-fabric-rp-helpers]: https://github.com/ChackDan/Service-Fabric/tree/master/Scripts/ServiceFabricRPHelpers
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[active-directory-howto-tenant]: ../active-directory/active-directory-howto-tenant.md
[service-fabric-visualizing-your-cluster]: service-fabric-visualizing-your-cluster.md
[service-fabric-manage-application-in-visual-studio]: service-fabric-manage-application-in-visual-studio.md
[sf-aad-ps-script-download]:http://servicefabricsdkstorage.blob.core.windows.net/publicrelease/MicrosoftAzureServiceFabric-AADHelpers.zip
[azure-quickstart-templates]: https://github.com/Azure/azure-quickstart-templates
[service-fabric-secure-cluster-5-node-1-nodetype]: https://github.com/Azure/azure-quickstart-templates/blob/master/service-fabric-secure-cluster-5-node-1-nodetype/
[resource-group-template-deploy]: https://azure.microsoft.com/documentation/articles/resource-group-template-deploy/
[x509-certificates-and-service-fabric]: service-fabric-cluster-security.md#x509-certificates-and-service-fabric

<!-- Images -->
[cluster-security-arm-dependency-map]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-arm-dependency-map.png
[cluster-security-cert-installation]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-cert-installation.png
[assign-users-to-roles-button]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles-button.png
[assign-users-to-roles-dialog]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles.png
[sfx-select-certificate-dialog]: ./media/service-fabric-cluster-creation-via-arm/sfx-select-certificate-dialog.png
[sfx-reply-address-not-match]: ./media/service-fabric-cluster-creation-via-arm/sfx-reply-address-not-match.png
[web-application-reply-url]: ./media/service-fabric-cluster-creation-via-arm/web-application-reply-url.png

