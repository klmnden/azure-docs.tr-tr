---
title: Bir şablondan bir Azure Service Fabric kümesi oluşturma | Microsoft Docs
description: Bu makalede, istemci kimlik doğrulaması için Azure Resource Manager, Azure anahtar kasası ve Azure Active Directory (Azure AD) kullanarak azure'da güvenli Service Fabric kümesi ayarlama açıklar.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: timlt
editor: chackdan
ms.assetid: 15d0ab67-fc66-4108-8038-3584eeebabaa
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/07/2017
ms.author: aljo
ms.openlocfilehash: e963b0f816d30411aa7d1e8c172ca0c2e5ddf0f1
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37444370"
---
# <a name="create-a-service-fabric-cluster-by-using-azure-resource-manager"></a>Azure Resource Manager'ı kullanarak bir Service Fabric kümesi oluşturma 
> [!div class="op_single_selector"]
> * [Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
> * [Azure Portal](service-fabric-cluster-creation-via-portal.md)
>
>

Bu adım adım kılavuzda, Azure güvenli Azure Service Fabric kümesinde Azure Resource Manager kullanarak kurma açıklanmaktadır. Biz, makaleyi uzun olduğunu kabul edersiniz. Zaten içerikle aşina olduğunuz sürece, yine de, her adım dikkatlice uyguladığınızdan emin olun.

Kılavuz, aşağıdaki yordamları içerir:

* Bir Service Fabric kümesinde dağıtılmadan önce dikkat edilmesi gereken temel kavramlar.
* Bir küme, Service Fabric Resource Manager modüllerini kullanarak Azure'da oluşturuluyor.
* Azure Active Directory'yi (Azure AD) ayarlama küme üzerinde yönetim işlemlerini gerçekleştirme kullanıcıların kimliklerinin doğrulanması için ayarlanıyor.
* Kümeniz için özel bir Azure Resource Manager şablon yazma ve dağıtma.

## <a name="key-concepts-to-be-aware-of"></a>Dikkat edilmesi gereken temel kavramları
Azure'da Service Fabric, taahhütlerin x x509 kullanmanızı kümenizi ve kendi uç güvenliğini sağlamak için sertifika. Sertifikalar, Service Fabric’te bir küme ile uygulamalarının çeşitli yönlerini güvenli hale getirmek üzere kimlik doğrulaması ve şifreleme sağlamak için kullanılır. İstemci erişim/kümede dağıtma, yükseltme ve uygulamalarınıza, hizmetlerinize ve içerdikleri veriler silme gibi yönetim işlemlerini gerçekleştirmek için sertifikaları veya Azure Active Directory kimlik bilgilerini kullanabilirsiniz. Bu sertifikalar, istemcilerde paylaşılmasını engellemek için tek yolu olduğundan Azure Active Directory kullanımı yüksek oranda önerilir.  Sertifikaların Service Fabric'te nasıl kullanıldığı hakkında daha fazla bilgi için bkz. [Service Fabric kümesi güvenlik senaryoları][service-fabric-cluster-security].

Service Fabric küme güvenliğini sağlama ve uygulama güvenlik özellikler sağlamak için X.509 sertifikaları kullanır. Kullandığınız [Key Vault] [ key-vault-get-started] azure'da Service Fabric kümelerine ait sertifikaları yönetmek için. 


### <a name="cluster-and-server-certificate-required"></a>Küme ve sunucu sertifikası (gerekli)
Bu sertifikalar (birincil ve isteğe bağlı olarak ikincil) küme güvenliğini sağlama ve yetkisiz erişimi önlemek için gereklidir. Bu iki yolla küme güvenliği sağlar:

* **Küme kimlik doğrulaması:** düğümden düğüme iletişim için küme Federasyon kimlik doğrulaması yapar. Bu sertifika ile kimliğini kanıtlamak düğüm kümesine katılabilirsiniz.
* **Sunucu kimlik doğrulaması:** yönetim istemcinin konuştuğu gerçek bir küme ve olmayan bir 'ortadaki adam' bilebilmesi bir yönetim istemcisinde küme yönetimi Uç noktalara kimliğini doğrular. Bu sertifika aynı zamanda bir SSL HTTPS yönetim API'si ve Service Fabric Explorer için HTTPS üzerinden sağlar.

Bu amaçla için sertifikanın aşağıdaki gereksinimleri karşılamalıdır:

* Sertifika özel anahtar içermelidir. Bu sertifikalar, genellikle uzantıları .pfx veya .pem çalıştırılır  
* Sertifikanın bir kişisel bilgi değişimi (.pfx) dosyasına aktarılabilen anahtar değişimi için oluşturulmuş olması gerekir.
* **Sertifikanın konu adı, Service Fabric kümesine erişmek için kullandığınız etki alanı eşleşmelidir**. Bu eşleşen bir SSL kümenin HTTPS yönetim uç noktası ve Service Fabric Explorer için sağlamak için gereklidir. İçin bir sertifika yetkilisinden (CA) bir SSL sertifikası alınamıyor *. cloudapp.azure.com etki alanı. Kümeniz için özel bir etki alanı adı edinmeniz gerekir. CA’dan sertifika istediğinizde sertifikanın konu adı, kümeniz için kullandığınız özel etki alanı adıyla eşleşmelidir.

### <a name="set-up-azure-active-directory-for-client-authentication-optional-but-recommended"></a>İstemci kimlik doğrulaması (isteğe bağlı, ancak önerilir) için Azure Active Directory ayarlayın

Azure AD (kiracılar bilinir), kuruluşların uygulamalara kullanıcı erişimini yönetmenizi sağlar. Uygulamaları olan web tabanlı oturum açma kullanıcı Arabirimi hem de yerel istemci deneyimi ile ayrılır. Bu makalede, zaten bir kiracı oluşturmuş varsayıyoruz. Tamamlamadıysanız, okuyarak başlamanız [bir Azure Active Directory kiracısı edinme][active-directory-howto-tenant].

Service Fabric kümesi birden çok giriş noktası için web tabanlı dahil olmak üzere Yönetim işlevselliğini sunar [Service Fabric Explorer] [ service-fabric-visualizing-your-cluster] ve [Visual Studio] [ service-fabric-manage-application-in-visual-studio]. Sonuç olarak, kümeye erişimi denetlemek için iki Azure AD uygulaması, bir web uygulaması ve yerel bir uygulama oluşturun.

Daha fazla bilgi bu belgede daha sonra nasıl ayarlanır.

### <a name="application-certificates-optional"></a>Uygulama sertifikaları (isteğe bağlı)
Uygulama güvenlik nedenleriyle bir kümedeki herhangi bir sayıda ek sertifikalar yüklenebilir. Kümenizi oluşturmadan önce düğümler üzerinde gibi yüklenmesi için bir sertifika gerektiren uygulama güvenliği senaryoları göz önünde bulundurun:

* Şifreleme ve şifre çözme uygulama yapılandırma değerlerini.
* Çoğaltma sırasında düğümler üzerinden verilerin şifrelenmesi.

Güvenli kümeleri oluşturma kavramı, bunlar Linux veya Windows kümelerinde aynıdır. 

### <a name="client-authentication-certificates-optional"></a>İstemci kimlik doğrulama sertifikaları (isteğe bağlı)
Herhangi bir sayıda ek sertifikalar, yönetici veya kullanıcı istemci işlemleri için belirtilebilir. Varsayılan olarak, küme sertifikası Yöneticisi istemci ayrıcalıklarına sahiptir. Bu ek istemci sertifikalarını kümeye yüklü olmamalıdır, bunu yalnızca bir küme yapılandırmasında izin olarak belirtilmesi gerekir, ancak bunlar kümeye bağlanın ve herhangi bir yönetim gerçekleştirmek için istemci bilgisayarlarında yüklü olması gerekir işlemler.


## <a name="prerequisites"></a>Önkoşullar 
Güvenli kümeleri oluşturma kavramı, bunlar Linux veya Windows kümelerinde aynıdır. Bu kılavuz, yeni kümeler oluşturmak için Azure PowerShell veya Azure CLI kullanımı kapsar. Önkoşullar ya da bulunmaktadır:

-  [Azure PowerShell 4.1 ve üzeri] [ azure-powershell] veya [Azure CLI 2.0 ve üzeri][azure-CLI].
-  Burada - Service Fabric modüller hakkında ayrıntılar bulabilirsiniz [AzureRM.ServiceFabric](https://docs.microsoft.com/powershell/module/azurerm.servicefabric) ve [az SF CLI Modülü](https://docs.microsoft.com/cli/azure/sf?view=azure-cli-latest)


## <a name="use-service-fabric-rm-module-to-deploy-a-cluster"></a>Bir küme dağıtmak için Service Fabric RM modülü kullanın

Bu belgede, Service Fabric RM powershell kullanacağız ve birden fazla senaryo için bir küme, PowerShell veya CLI modülü komutunu dağıtmak için CLI modülü sağlar. Bize her birine gidin. Çekme en iyi düşündüğünüzü senaryo gereksinimlerinize uygun. 

- Yeni küme oluşturma 
    - bir sistemi kullanılarak oluşturulan kendinden imzalı bir sertifika
    - bir sertifika kullanarak zaten sahip olduğunuz

Bir varsayılan küme şablonu veya sahip olduğunuz bir şablon kullanabilirsiniz

### <a name="create-new-cluster----using-a-system-generated-self-signed-certificate"></a>Yeni küme oluşturma - bir sistemi kullanılarak oluşturulan kendinden imzalı bir sertifika

Sistemin otomatik olarak imzalanan bir sertifika oluşturur ve kümenizin güvenliğini sağlama için kullanmak istiyorsanız, küme oluşturmak için aşağıdaki komutu kullanın. Bu komut, bu sertifikayı kullanarak yönetim işlemlerini gerçekleştirmek için küme güvenlik ve yönetim erişimi ayarlamak için kullanılan bir birincil küme sertifikası ayarlar.

### <a name="login-to-azure"></a>Azure'da oturum aç

```PowerShell
Connect-AzureRmAccount
Set-AzureRmContext -SubscriptionId <guid>
```

```CLI
azure login
az account set --subscription $subscriptionId
```
#### <a name="use-the-default-5-node-1-node-type-template-that-ships-in-the-module-to-set-up-the-cluster"></a>Küme oluşturma modüldeki birlikte gelen varsayılan 5 düğüm 1 düğüm türü şablonu kullanın

Hızlı bir şekilde olabildiğince az parametre belirterek bir küme oluşturmak için aşağıdaki komutu kullanın.

Kullanılan şablon edinilebilir [Azure Service Fabric şablonu örnekleri: windows şablonu](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-1-NodeTypes-Secure-NSG) ve [Ubuntu şablonu](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Ubuntu-1-NodeTypes-Secure)

Çalışan Windows ve Linux kümeleri oluşturmak için aşağıdaki komutları, yalnızca işletim sistemi uygun şekilde belirtmeniz gerekir. PowerShell/CLI komutları da belirtilen CertificateOutputFolder sertifikayı çıktı; Ancak, önceden oluşturulmuş emin sertifika klasörüne olun. Komut diğer parametre VM SKU gibi de alır.

> [!NOTE]
> Aşağıdaki Powershell komutu yalnızca Azure Resource Manager PowerShell ile çalışır. sürüm > 6.1. Geçerli Azure Resource Manager PowerShell sürümü denetlemek için "Get-Module AzureRM" aşağıdaki PowerShell komutunu çalıştırın. Azure Resource Manager PowerShell sürümünüzü yükseltmek için bu bağlantıyı izleyin. https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps?view=azurermps-6.3.0
>
>
```PowerShell
$resourceGroupLocation="westus"
$resourceGroupName="mycluster"
$vaultName="myvault"
$vaultResourceGroupName="myvaultrg"
$CertSubjectName="mycluster.westus.cloudapp.azure.com"
$certPassword="Password123!@#" | ConvertTo-SecureString -AsPlainText -Force 
$vmpassword="Password4321!@#" | ConvertTo-SecureString -AsPlainText -Force
$vmuser="myadmin"
$os="WindowsServer2016DatacenterwithContainers"
$certOutputFolder="c:\certificates"

New-AzureRmServiceFabricCluster -ResourceGroupName $resourceGroupName -Location $resourceGroupLocation -CertificateOutputFolder $certOutputFolder -CertificatePassword $certpassword -CertificateSubjectName $CertSubjectName -OS $os -VmPassword $vmpassword -VmUserName $vmuser
```

```CLI
declare resourceGroupLocation="westus"
declare resourceGroupName="mylinux"
declare vaultResourceGroupName="myvaultrg"
declare vaultName="myvault"
declare CertSubjectName="mylinux.westus.cloudapp.azure.com"
declare vmpassword="Password!1"
declare certpassword="Password!4321"
declare vmuser="myadmin"
declare vmOs="UbuntuServer1604"
declare certOutputFolder="c:\certificates"

az sf cluster create --resource-group $resourceGroupName --location $resourceGroupLocation  \
    --certificate-output-folder $certOutputFolder --certificate-password $certpassword  \
    --vault-name $vaultName --vault-resource-group $resourceGroupName  \
    --template-file $templateFilePath --parameter-file $parametersFilePath --vm-os $vmOs  \
    --vm-password $vmpassword --vm-user-name $vmuser
```

#### <a name="use-the-custom-template-that-you-already-have"></a>Zaten sahip olduğunuz özel bir şablon kullanmak 

Gereksinimlerinize göre özel bir şablon oluşturmak ihtiyacınız varsa mevcut şablonlardan birini ile Başlat önerilir [Azure Service Fabric şablonu örnekleri](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master). Yönergeler ve açıklamaları için izleme [küme şablonunuzu özelleştirme] [ customize-your-cluster-template] bölümüne bakın.

Zaten sahip özel bir şablon sonra olun emin olun, tüm üç sertifika şablonu ve parametre dosyası Parametreler şu şekilde adlandırılır ilgili kontrol edin ve değerleri null gibidir.

```Json
   "certificateThumbprint": {
      "value": ""
    },
    "sourceVaultValue": {
      "value": ""
    },
    "certificateUrlValue": {
      "value": ""
    },
```


```PowerShell
$resourceGroupLocation="westus"
$resourceGroupName="mycluster"
$CertSubjectName="mycluster.westus.cloudapp.azure.com"
$certPassword="Password!1" | ConvertTo-SecureString -AsPlainText -Force 
$certOutputFolder="c:\certificates"

$parameterFilePath="c:\mytemplates\mytemplateparm.json"
$templateFilePath="c:\mytemplates\mytemplate.json"

New-AzureRmServiceFabricCluster -ResourceGroupName $resourceGroupName -CertificateOutputFolder $certOutputFolder -CertificatePassword $certpassword -CertificateSubjectName $CertSubjectName -TemplateFile $templateFilePath -ParameterFile $parameterFilePath 
```

Aynı işlemi gerçekleştirmek için eşdeğer CLI komutu aşağıda verilmiştir. Declare deyimlerini değerleri uygun değerlerle değiştirin. CLI, yukarıdaki PowerShell komutunu destekleyen diğer tüm parametreleri destekler.

```CLI
declare certPassword=""
declare resourceGroupLocation="westus"
declare resourceGroupName="mylinux"
declare certSubjectName="mylinuxsecure.westus.cloudapp.azure.com"
declare parameterFilePath="c:\mytemplates\linuxtemplateparm.json"
declare templateFilePath="c:\mytemplates\linuxtemplate.json"
declare certOutputFolder="c:\certificates"

az sf cluster create --resource-group $resourceGroupName --location $resourceGroupLocation  \
    --certificate-output-folder $certOutputFolder --certificate-password $certPassword  \
    --certificate-subject-name $certSubjectName \
    --template-file $templateFilePath --parameter-file $parametersFilePath
```


### <a name="create-new-cluster---using-the-certificate-you-bought-from-a-ca-or-you-already-have"></a>Yeni küme oluşturma - sertifika kullanarak bir CA'dan satın aldığınız ya da zaten sahip

Kümenizle güvenliğini sağlamak için kullanmak istediğiniz bir sertifika varsa kümesi oluşturmak için aşağıdaki komutu kullanın.

Bu, diğer amaçlar için kullanarak son bulur, CA imzalı bir sertifika ise, özellikle anahtar kasanız için ayrı bir kaynak grubuna sağlamanız önerilir. Anahtar Kasası'nı kendi kaynak grubuna yerleştirin öneririz. Bu eylem, anahtarları ve gizli bilgilerinizi kaybetmeden Service Fabric kümenizi içeren kaynak grubunu da dahil olmak üzere bilgi işlem ve depolama kaynak gruplarını kaldırmaya olanak tanır. **Anahtar kasanızı içeren kaynak grubunu _aynı bölgede olmalıdır_ onu kullanarak bir küme.**


#### <a name="use-the-default-5-node-1-node-type-template-that-ships-in-the-module"></a>Modülde birlikte gelen varsayılan 5 düğüm 1 düğüm türü şablonu kullanın
Kullanılan şablon edinilebilir [Azure örnekleri: Windows şablon](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-1-NodeTypes-Secure-NSG) ve [Ubuntu şablonu](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Ubuntu-1-NodeTypes-Secure)

```PowerShell
$resourceGroupLocation="westus"
$resourceGroupName="mylinux"
$vaultName="myvault"
$vaultResourceGroupName="myvaultrg"
$certPassword="Password!1" | ConvertTo-SecureString -AsPlainText -Force 
$vmpassword=("Password!4321" | ConvertTo-SecureString -AsPlainText -Force) 
$vmuser="myadmin"
$os="WindowsServer2016DatacenterwithContainers"

New-AzureRmServiceFabricCluster -ResourceGroupName $resourceGroupName -Location $resourceGroupLocation -KeyVaultResouceGroupName $vaultResourceGroupName -KeyVaultName $vaultName -CertificateFile C:\MyCertificates\chackocertificate3.pfx -CertificatePassword $certPassword -OS $os -VmPassword $vmpassword -VmUserName $vmuser 
```

```CLI
declare vmPassword="Password!1"
declare certPassword="Password!1"
declare vmUser="myadmin"
declare resourceGroupLocation="westus"
declare resourceGroupName="mylinux"
declare vaultResourceGroupName="myvaultrg"
declare vaultName="myvault"
declare certificate-file="c:\certificates\mycert.pem"
declare vmOs="UbuntuServer1604"

az sf cluster create --resource-group $resourceGroupName --location $resourceGroupLocation  \
    --certificate-file $certificate-file --certificate-password $certPassword  \
    --vault-name $vaultName --vault-resource-group $vaultResourceGroupName  \
    --vm-os vmOs \
    --vm-password $vmPassword --vm-user-name $vmUser
```

#### <a name="use-the-custom-template-that-you-have"></a>Sahip olduğunuz özel bir şablon kullanmak 
Gereksinimlerinize göre özel bir şablon oluşturmak ihtiyacınız varsa mevcut şablonlardan birini ile Başlat önerilir [Azure Service Fabric şablonu örnekleri](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master). Yönergeler ve açıklamaları için izleme [küme şablonunuzu özelleştirme] [ customize-your-cluster-template] bölümüne bakın.

Zaten sahip özel bir şablon sonra olun emin olun, tüm üç sertifika şablonu ve parametre dosyası Parametreler şu şekilde adlandırılır ilgili kontrol edin ve değerleri null gibidir.

```Json
   "certificateThumbprint": {
      "value": ""
    },
    "sourceVaultValue": {
      "value": ""
    },
    "certificateUrlValue": {
      "value": ""
    },
```


```PowerShell
$resourceGroupLocation="westus"
$resourceGroupName="mylinux"
$vaultName="myvault"
$vaultResourceGroupName="myvaultrg"
$certPassword="Password!1" | ConvertTo-SecureString -AsPlainText -Force 
$os="WindowsServer2016DatacenterwithContainers"
$parameterFilePath="c:\mytemplates\mytemplateparm.json"
$templateFilePath="c:\mytemplates\mytemplate.json"
$certificateFile="C:\MyCertificates\chackonewcertificate3.pem"

New-AzureRmServiceFabricCluster -ResourceGroupName $resourceGroupName -Location $resourceGroupLocation -TemplateFile $templateFilePath -ParameterFile $parameterFilePath -KeyVaultResouceGroupName $vaultResourceGroupName -KeyVaultName $vaultName -CertificateFile $certificateFile -CertificatePassword $certPassword
```

Aynı işlemi gerçekleştirmek için eşdeğer CLI komutu aşağıda verilmiştir. Declare deyimlerini değerleri uygun değerlerle değiştirin.

```CLI
declare certPassword="Password!1"
declare resourceGroupLocation="westus"
declare resourceGroupName="mylinux"
declare vaultResourceGroupName="myvaultrg"
declare vaultName="myvault"
declare parameterFilePath="c:\mytemplates\linuxtemplateparm.json"
declare templateFilePath="c:\mytemplates\linuxtemplate.json"

az sf cluster create --resource-group $resourceGroupName --location $resourceGroupLocation  \
    --certificate-file $certificate-file --certificate-password $password  \
    --vault-name $vaultName --vault-resource-group $vaultResourceGroupName  \
    --template-file $templateFilePath --parameter-file $parametersFilePath 
```

#### <a name="use-a-pointer-to-the-secret-you-already-have-uploaded-into-the-key-vault"></a>Gizli anahtar kasasına zaten karşıya bir işaretçi kullanın

Var olan bir anahtar Kasası'nı kullanmak için _dağıtımı için etkinleştirmeniz gerekir_ işlem kaynak sağlayıcısı sertifikaları almak ve küme düğümlerine yüklemek izin vermek için:

```PowerShell
Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment

$parameterFilePath="c:\mytemplates\mytemplate.json"
$templateFilePath="c:\mytemplates\mytemplateparm.json"
$secretID="https://test1.vault.azure.net:443/secrets/testcertificate4/55ec7c4dc61a462bbc645ffc9b4b225f"

New-AzureRmServiceFabricCluster -ResourceGroupName $resourceGroupName -SecretIdentifier $secretId -TemplateFile $templateFilePath -ParameterFile $parameterFilePath 
```
Aynı işlemi gerçekleştirmek için eşdeğer CLI komutu aşağıda verilmiştir. Declare deyimlerini değerleri uygun değerlerle değiştirin.

```CLI
declare $resourceGroupName = "testRG"
declare $parameterFilePath="c:\mytemplates\mytemplate.json"
declare $templateFilePath="c:\mytemplates\mytemplateparm.json"
declare $secertId="https://test1.vault.azure.net:443/secrets/testcertificate4/55ec7c4dc61a462bbc645ffc9b4b225f"

az sf cluster create --resource-group $resourceGroupName --location $resourceGroupLocation  \
    --secret-identifier az $secretID  \
    --template-file $templateFilePath --parameter-file $parametersFilePath 
```

<a id="add-AAD-for-client"></a>

## <a name="set-up-azure-active-directory-for-client-authentication"></a>İstemci kimlik doğrulaması için Azure Active Directory ayarlayın

Azure AD (kiracılar bilinir), kuruluşların uygulamalara kullanıcı erişimini yönetmenizi sağlar. Uygulamaları olan web tabanlı oturum açma kullanıcı Arabirimi hem de yerel istemci deneyimi ile ayrılır. Bu makalede, zaten bir kiracı oluşturmuş varsayıyoruz. Tamamlamadıysanız, okuyarak başlamanız [bir Azure Active Directory kiracısı edinme][active-directory-howto-tenant].

Service Fabric kümesi birden çok giriş noktası için web tabanlı dahil olmak üzere Yönetim işlevselliğini sunar [Service Fabric Explorer] [ service-fabric-visualizing-your-cluster] ve [Visual Studio] [ service-fabric-manage-application-in-visual-studio]. Sonuç olarak, kümeye erişimi denetlemek için iki Azure AD uygulaması, bir web uygulaması ve yerel bir uygulama oluşturun.

Bazı yapılandırma Azure AD'de bir Service Fabric kümesi ile yer alan adımların basitleştirmek için Windows PowerShell komutları kümesi oluşturduk.

> [!NOTE]
> Kümeyi oluşturmadan önce aşağıdaki adımları tamamlamanız gerekir. Küme adları ve uç noktaları betikleri beklediğiniz çünkü değerleri planlanmalıdır ve, zaten oluşturduğunuz değerleri değil.

1. [Betiklerini indirme] [ sf-aad-ps-script-download] bilgisayarınıza.
2. Zip dosyasını sağ tıklayın, **özellikleri**seçin **Engellemeyi Kaldır** onay kutusunu işaretleyin ve ardından **Uygula**.
3. Zip dosyasını ayıklayın.
4. Çalıştırma `SetupApplications.ps1`ve parametrelere Tenantıd, ClusterName ve WebApplicationReplyUrl girin. Örneğin:

```PowerShell
.\SetupApplications.ps1 -TenantId '690ec069-8200-4068-9d01-5aaf188e557a' -ClusterName 'mycluster' -WebApplicationReplyUrl 'https://mycluster.westus.cloudapp.azure.com:19080/Explorer/index.html'
```

PowerShell komutunu yürüterek Tenantıd'nizi bulabilirsiniz `Get-AzureSubscription`. Bu komut yürütülürken, her abonelik için Tenantıd görüntüler.

ClusterName betiği tarafından oluşturulan Azure AD uygulamaları önek olarak eklemek için kullanılır. Gerçek bir küme adı tam olarak eşleşmesi gerekmez. Yalnızca bunlar ile kullanılan Service Fabric kümesine Azure AD'ye yapıtları eşlemek kolaylaştırmak için tasarlanmıştır.

WebApplicationReplyUrl oturum açma işlemini tamamladıktan sonra kullanıcılara Azure AD döndüren varsayılan uç noktadır. Bu uç nokta olan varsayılan olarak, kümenizin Service Fabric Explorer uç nokta olarak ayarlayın:

https://&lt;cluster_domain&gt;: 19080/Explorer

Azure AD kiracısı için yönetici ayrıcalıklarına sahip bir hesap için oturum açmanız istenir. Oturum açtıktan sonra Service Fabric kümenizi temsil etmek için yerel uygulamalar ve web betik oluşturur. Kiracının uygulamaları bakarsanız [Azure portalında][azure-portal], iki yeni giriş görmeniz gerekir:

   * *ClusterName*\_küme
   * *ClusterName*\_istemci

PowerShell penceresini açık tutmak için iyi bir fikirdir, bu nedenle sonraki bölümde, kümeyi oluşturduğunuzda Azure Resource Manager şablon tarafından gereken JSON betiği yazdırır.

```json
"azureActiveDirectory": {
  "tenantId":"<guid>",
  "clusterApplication":"<guid>",
  "clientApplication":"<guid>"
},
```

<a id="customize-arm-template" ></a>

## <a name="create-a-service-fabric-cluster-resource-manager-template"></a>Service Fabric Küme Kaynak Yöneticisi şablonu oluştur
Bu bölümde, Service Fabric Küme Kaynak Yöneticisi şablonunu özel isteyen kullanıcıların Yazar aranır. bir şablonu oluşturduktan sonra hala geri dönün ve bunu dağıtmak için PowerShell veya CLI modülleri'ni kullanın. 

Örnek Resource Manager şablonları kullanılabilir [github'daki Azure örnekleri](https://github.com/Azure-Samples/service-fabric-cluster-templates). Bu şablonlar, küme şablonunuza için başlangıç noktası olarak kullanılabilir.

### <a name="create-the-resource-manager-template"></a>Resource Manager şablonu oluşturma
Bu kılavuzda kullanan [güvenli 5 düğümlü küme] [ service-fabric-secure-cluster-5-node-1-nodetype] örnek şablonu ve şablon parametreleri. İndirme `azuredeploy.json` ve `azuredeploy.parameters.json` bilgisayarınıza ve iki dosyayı da sık kullandığınız metin düzenleyicinizde açın.

### <a name="add-certificates"></a>Sertifika Ekle
Sertifika anahtarlarını içeren anahtar kasası başvurarak sertifikaları için Küme Kaynak Yöneticisi şablonu ekleyin. Resource Manager şablon parametreleri dosyasında (azuredeploy.parameters.json), bu anahtar kasası parametrelerini ve değerlerini ekleyin. 

#### <a name="add-all-certificates-to-the-virtual-machine-scale-set-osprofile"></a>Bir sanal makine ölçek kümesi osProfile tüm sertifikaları Ekle
Ölçek kümesi kaynak (Microsoft.Compute/virtualMachineScaleSets) osProfile bölümünde kümeye yüklü her sertifikanın yapılandırılması gerekir. Bu eylem Vm'lerde sertifikayı yüklemek için kaynak sağlayıcısı bildirir. Bu yükleme, küme sertifikası hem de uygulamalarınız için kullanmayı planladığınız herhangi bir uygulama güvenlik sertifikaları içerir:

```json
{
  "apiVersion": "[variables('vmssApiVersion')]",
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

#### <a name="configure-the-service-fabric-cluster-certificate"></a>Service Fabric küme sertifikası yapılandırma
Küme kimlik doğrulama sertifikası hem de Service Fabric küme kaynak (Microsoft.ServiceFabric/clusters) yapılandırılması gerekir ve sanal makine ölçek kümesi kaynak sanal makine ölçek için Service Fabric uzantısı ayarlar. Bu düzenleme, küme kimlik doğrulaması ve yönetim uç noktaları için sunucu kimlik doğrulaması kullanmak için yapılandırmak Service Fabric kaynak sağlayıcısı sağlar.

##### <a name="add-the-certificate-information-the-virtual-machine-scale-set-resource"></a>Kaynak sertifika bilgilerini sanal makine ölçek kümesi ekleyin:
```json
{
  "apiVersion": "[variables('vmssApiVersion')]",
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
                  "commonNames": ["[parameters('certificateCommonName')]"],
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

##### <a name="add-the-certificate-information-to-the-service-fabric-cluster-resource"></a>Service Fabric küme kaynağı için sertifika bilgileri ekleyin:
```json
{
  "apiVersion": "2018-02-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  "location": "[parameters('clusterLocation')]",
  "dependsOn": [
    "[concat('Microsoft.Storage/storageAccounts/', variables('supportLogStorageAccountName'))]"
  ],
  "properties": {
    "certificateCommonNames": {
        "commonNames": [
        {
            "certificateCommonName": "[parameters('certificateCommonName')]",
            "certificateIssuerThumbprint": ""
        }
        ],
        "x509StoreName": "[parameters('certificateStoreValue')]"
    },
    ...
  }
}
```

### <a name="add-azure-ad-configuration-to-use-azure-ad-for-client-access"></a>Azure AD istemci erişimi için kullanılacak Azure AD Yapılandırması Ekle

Küme Kaynak Yöneticisi şablonu için Azure AD yapılandırmasının sertifika anahtarlar içeren anahtar kasası başvurarak ekleyin. Resource Manager şablon parametreleri dosyasında (azuredeploy.parameters.json) bu Azure AD parametrelerini ve değerlerini ekleyin.

```json
{
  "apiVersion": "2018-02-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  ...
  "properties": {
    "certificateCommonNames": {
        "commonNames": [
        {
            "certificateCommonName": "[parameters('certificateCommonName')]",
            "certificateIssuerThumbprint": ""
        }
        ],
        "x509StoreName": "[parameters('certificateStoreValue')]"
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

### <a name="populate-the-parameter-file-with-the-values"></a>Parametre dosyasını değerlerle doldurmak
Son olarak, parametre dosyasını doldurmak için çıkış değerleri anahtar kasası ve Azure AD PowerShell komutlarını kullanın.

Ardından Azure service fabric RM PowerShell modüllerini kullanmayı planlıyorsanız, küme sertifika bilgilerini doldurmak gerekmez. Sisteminin imzalı self oluşturmasını istiyorsanız, kümenin güvenliği için sertifika, null olarak kalmasını. 

> [!NOTE]
> RM modülleri almak ve bu boş parametre değerleri doldurmak parametre adları çok aşağıdaki adlarla eşleşir.

```json
"clusterCertificateThumbprint": {
    "value": ""
},
"certificateCommonName": {
    "value": ""
},
"clusterCertificateUrlValue": {
    "value": ""
},
"sourceVaultvalue": {
    "value": ""
},
```

Uygulama sertifikaları kullanarak ya da anahtar kasasına yüklenmiş mevcut bir kümeye kullanıyorsanız, bu bilgileri alın ve bunu doldurmak gerekir.

RM modülleri, Azure AD yapılandırmasının oluşturma yeteneği yoktur için istemci erişimi için Azure AD kullanmayı planlıyorsanız, bu doldurmanız gerekir.

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

### <a name="test-your-template"></a>Şablonunuzu test  
Resource Manager şablonunuzu bir parametre dosyasıyla test etmek için aşağıdaki PowerShell komutunu kullanın:

```PowerShell
Test-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

Bir sorunla karşılaşırsanız ve şifreli iletileri alma durumunda, kullanın, ardından "-Debug" seçeneği olarak.

```PowerShell
Test-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json -Debug
```

Aşağıdaki diyagram, Azure AD yapılandırma ve anahtar kasası Resource Manager şablonunuzu nerelerde gösterir.

![Resource Manager bağımlılık Haritası][cluster-security-arm-dependency-map]


## <a name="encrypting-the-disks-attached-to-your-windows-cluster-nodevirtual-machine-instances"></a>İçin windows küme düğümü/sanal makine örnekleri bağlı diskleri şifreleme

Düğümlerine ekli diskleri (işletim sistemi sürücüsü ve diğer yönetilen diskler) şifrelemek için size Azure Disk şifrelemesi yararlanın. Azure Disk şifrelemesi yardımcı olan yeni bir özellik olan [Windows sanal makine disklerinizi şifreleyin](service-fabric-enable-azure-disk-encryption-windows.md). Azure Disk şifrelemesi, endüstri standardı yararlanır [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) işletim sistemi birimi için birim şifrelemesi sağlamak için Windows özelliğidir. İle tümleşik bir çözüm [Azure anahtar kasası](https://azure.microsoft.com/documentation/services/key-vault/) denetlemenize ve disk şifreleme anahtarlarını ve gizli anahtar kasası aboneliğinizi yönetmenize yardımcı olmak için. Çözüm ayrıca sanal makine disklerindeki tüm veriler Azure depolama alanınızda bekleyen şifrelenmesini sağlar. 

## <a name="encrypting-the-disks-attached-to-your-linux-cluster-nodevirtual-machine-instances"></a>Kendi Linux küme düğümü/sanal makine örneklerine bağlı diskleri şifreleme

Düğümlerine ekli disklerin (veri sürücüsü ve diğer yönetilen diskler) şifrelemek için size Azure Disk şifrelemesi yararlanın. Azure Disk şifrelemesi yardımcı olan yeni bir özellik olan [Linux sanal makine disklerinizi şifreleyin](service-fabric-enable-azure-disk-encryption-linux.md). Azure Disk şifrelemesi, endüstri standardı yararlanır [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) veri diskleri için birim şifrelemesi sağlamak için Linux özelliğidir. İle tümleşik bir çözüm [Azure anahtar kasası](https://azure.microsoft.com/documentation/services/key-vault/) denetlemenize ve disk şifreleme anahtarlarını ve gizli anahtar kasası aboneliğinizi yönetmenize yardımcı olmak için. Çözüm ayrıca sanal makine disklerindeki tüm veriler Azure depolama alanınızda bekleyen şifrelenmesini sağlar. 


## <a name="create-the-cluster-using-azure-resource-template"></a>Azure kaynak şablonu kullanarak küme oluşturma 

Belgenin önceki bölümlerinde anlatılan adımları kullanarak kümeyi şimdi dağıtabilir veya parametre dosyasında doldurulmuş değerler varsa, ardından artık kullanarak küme oluşturmaya hazırsınız [Azure kaynak şablon dağıtımı] [ resource-group-template-deploy] doğrudan.

```PowerShell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

Bir sorunla karşılaşırsanız ve şifreli iletileri alma durumunda, kullanın, ardından "-Debug" seçeneği olarak.


<a name="assign-roles"></a>

## <a name="assign-users-to-roles"></a>Kullanıcı rollerine atama
Kümenizi temsil etmek için uygulamaları oluşturduktan sonra kullanıcılarınızın Service Fabric tarafından desteklenen roller atama: salt okunur ve yönetici Rolleri kullanarak atayabilirsiniz [Azure portalında][azure-portal].

1. Azure portalında, sağ üst köşedeki kiracınızı seçin.

    ![Kiracı düğmeyi seçin][select-tenant-button]
2. Seçin **Azure Active Directory** sol sekmesini ve ardından seçin "Kurumsal uygulamalar".
3. "Tüm uygulamalar" seçin ve ardından bulmak ve seçmek bir ada sahip web uygulaması gibi `myTestCluster_Cluster`.
4. Tıklayın **kullanıcılar ve gruplar** sekmesi.

    ![Kullanıcılar ve Gruplar sekmesinde][users-and-groups-tab]
5. Tıklayın **Kullanıcı Ekle** düğmesini yeni sayfada, bir kullanıcı ve rol atayın ve ardından seçin **seçin** sayfanın alt kısmındaki düğmesi.

    ![Kullanıcı rolleri sayfasına atama][assign-users-to-roles-page]
6. Tıklayın **atama** sayfanın alt kısmındaki düğmesi.

    ![Atama onayı Ekle][assign-users-to-roles-confirm]

> [!NOTE]
> Service fabric'te rolleri hakkında daha fazla bilgi için bkz. [Service Fabric istemciler için rol tabanlı erişim denetimi](service-fabric-cluster-security-roles.md).
>
>


## <a name="troubleshooting-help-in-setting-up-azure-active-directory"></a>Azure Active Directory'yi ayarlama konusunda Yardım sorunlarını giderme
Burada olduklarından bazı işaretçiler sorunla ilgili hataları ayıklamak için yapabilecekleriniz hakkında Azure AD'yi ayarlama ve kullanma, zor olabilir.

### <a name="service-fabric-explorer-prompts-you-to-select-a-certificate"></a>Service Fabric Explorer bir sertifika seçmenizi ister.
#### <a name="problem"></a>Sorun
Başarılı bir şekilde Azure AD'ye Service Fabric Explorer'da oturum açtıktan sonra tarayıcı giriş sayfasına döndürür ancak iletiye bir sertifika seçmenizi ister.

![SFX sertifika iletişim kutusu][sfx-select-certificate-dialog]

#### <a name="reason"></a>Neden
Kullanıcı Azure AD küme uygulaması bir rol atanmamıştır. Bu nedenle, Service Fabric kümesinde Azure AD kimlik doğrulaması başarısız olur. Service Fabric Explorer, sertifika kimlik doğrulaması için geri döner.

#### <a name="solution"></a>Çözüm
Azure AD'yi ayarlama yönergelerini izleyin ve kullanıcı rolleri atayın. Ayrıca, "uygulamasına erişmek için kullanıcı ataması gerekli üzerinde" Kapat olarak öneririz `SetupApplications.ps1` yapar.

### <a name="connection-with-powershell-fails-with-an-error-the-specified-credentials-are-invalid"></a>PowerShell ile bağlantı bir hata ile başarısız oluyor: "belirtilen kimlik bilgileri geçersiz"
#### <a name="problem"></a>Sorun
Azure AD'ye başarıyla oturum açtıktan sonra "AzureActiveDirectory" güvenlik modunu kullanarak kümeye bağlanmak için PowerShell kullanırken, bağlantının bir hatayla başarısız oluyor: "belirtilen kimlik bilgileri geçersiz."

#### <a name="solution"></a>Çözüm
Bu çözüm, önceki bir ile aynıdır.

### <a name="service-fabric-explorer-returns-a-failure-when-you-sign-in-aadsts50011"></a>Service Fabric Explorer, oturum açtığınızda bir hata döndürür: "AADSTS50011"
#### <a name="problem"></a>Sorun
Service Fabric Explorer'ın Azure AD'de oturum açmaya çalıştığınızda, sayfanın bir hata döndürür: "AADSTS50011: yanıt adresi &lt;url&gt; uygulama için yapılandırılan yanıt adresleriyle eşleşmiyor: &lt;GUID&gt;."

![SFX yanıt adresi eşleşmiyor][sfx-reply-address-not-match]

#### <a name="reason"></a>Neden
Service Fabric Explorer'ı temsil eden bir küme (web) uygulaması, Azure AD'de bir kimlik doğrulama girişiminde ve isteğe bağlı olarak, isteğin bir parçası yeniden yönlendirme dönüş URL'si sağlar. Ancak Azure AD uygulama URL'sini listelenmeyen **yanıt URL'si** listesi.

#### <a name="solution"></a>Çözüm
AAD sayfasında "Uygulama kayıtları"'i seçin, küme uygulamanızı seçin ve ardından **yanıt URL'leri** düğmesi. "Yanıt URL'leri" sayfasında, Service Fabric Explorer URL'si listeye ekleyin veya listedeki öğelerden birini değiştirin. İşiniz bittiğinde değişikliğinizi kaydedin.

![Web uygulamasının yanıt URL'si][web-application-reply-url]

### <a name="connect-the-cluster-by-using-azure-ad-authentication-via-powershell"></a>PowerShell aracılığıyla Azure AD kimlik doğrulamasını kullanarak kümeye bağlanın.
Service Fabric kümesine bağlanmak için aşağıdaki PowerShell komutu örneği kullanın:

```PowerShell
Connect-ServiceFabricCluster -ConnectionEndpoint <endpoint> -KeepAliveIntervalInSec 10 -AzureActiveDirectory -ServerCertThumbprint <thumbprint>
```

Connect-ServiceFabricCluster cmdlet hakkında bilgi edinmek için [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx).

### <a name="can-i-reuse-the-same-azure-ad-tenant-in-multiple-clusters"></a>Birden çok kümeleri aynı Azure AD kiracısında yeniden kullanabilir miyim?
Evet. Ancak, küme (web) uygulamanız için Service Fabric Explorer URL'si eklemeyi unutmayın. Aksi takdirde, Service Fabric Explorer çalışmaz.

### <a name="why-do-i-still-need-a-server-certificate-while-azure-ad-is-enabled"></a>Azure AD etkinken neden hala bir sunucu sertifikası ihtiyacım var?
FabricClient ve FabricGateway bir karşılıklı kimlik doğrulaması gerçekleştirin. Azure AD kimlik doğrulaması sırasında sunucuya bir istemci kimliği Azure AD tümleştirme sağlar ve sunucu sertifikası sunucu kimliğini doğrulamak için kullanılır. Service Fabric sertifikalar hakkında daha fazla bilgi için bkz. [X.509 sertifikaları ve Service Fabric][x509-certificates-and-service-fabric].

## <a name="next-steps"></a>Sonraki adımlar
Bu noktada, Azure Active Directory sağlayarak yönetim kimlik doğrulama ile güvenli bir kümeye sahip. İleri [kümenize bağlanın](service-fabric-connect-to-secure-cluster.md) ve bilgi edinmek için nasıl [uygulama parolalarını yönetme](service-fabric-application-secret-management.md).


<!-- Links -->
[azure-powershell]:https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[azure-CLI]:https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest
[key-vault-get-started]:../key-vault/key-vault-get-started.md
[aad-graph-api-docs]:https://msdn.microsoft.com/library/azure/ad/graph/api/api-catalog
[azure-portal]: https://portal.azure.com/
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[active-directory-howto-tenant]: ../active-directory/active-directory-howto-tenant.md
[service-fabric-visualizing-your-cluster]: service-fabric-visualizing-your-cluster.md
[service-fabric-manage-application-in-visual-studio]: service-fabric-manage-application-in-visual-studio.md
[sf-aad-ps-script-download]:http://servicefabricsdkstorage.blob.core.windows.net/publicrelease/MicrosoftAzureServiceFabric-AADHelpers.zip
[azure-quickstart-templates]: https://github.com/Azure/azure-quickstart-templates
[service-fabric-secure-cluster-5-node-1-nodetype]: https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-1-NodeTypes-Secure
[resource-group-template-deploy]: https://azure.microsoft.com/documentation/articles/resource-group-template-deploy/
[x509-certificates-and-service-fabric]: service-fabric-cluster-security.md#x509-certificates-and-service-fabric
[customize-your-cluster-template]: service-fabric-cluster-creation-via-arm.md#create-a-service-fabric-cluster-resource-manager-template

<!-- Images -->
[cluster-security-arm-dependency-map]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-arm-dependency-map.png
[cluster-security-cert-installation]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-cert-installation.png
[select-tenant-button]: ./media/service-fabric-cluster-creation-via-arm/select-tenant-button.png
[users-and-groups-tab]: ./media/service-fabric-cluster-creation-via-arm/users-and-groups-tab.png
[assign-users-to-roles-page]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles-page.png
[assign-users-to-roles-confirm]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles-confirm.png
[sfx-select-certificate-dialog]: ./media/service-fabric-cluster-creation-via-arm/sfx-select-certificate-dialog.png
[sfx-reply-address-not-match]: ./media/service-fabric-cluster-creation-via-arm/sfx-reply-address-not-match.png
[web-application-reply-url]: ./media/service-fabric-cluster-creation-via-arm/web-application-reply-url.png

