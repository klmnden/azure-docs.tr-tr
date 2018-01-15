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
ms.date: 12/07/2017
ms.author: chackdan
ms.openlocfilehash: e5dd1ebd290c950c7f2bda3dae088f3ee7f836fd
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2018
---
# <a name="create-a-service-fabric-cluster-by-using-azure-resource-manager"></a>Azure Kaynak Yöneticisi'ni kullanarak bir Service Fabric kümesi oluştur 
> [!div class="op_single_selector"]
> * [Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
> * [Azure portalı](service-fabric-cluster-creation-via-portal.md)
>
>

Bu adım adım kılavuz, Azure Kaynak Yöneticisi'ni kullanarak azure'da güvenli bir Azure Service Fabric kümesi ayarlama aracılığıyla açıklanmaktadır. Makaleyi uzun olduğunu kabul ediyorsunuz. İçerikle baştan sona bilginiz sürece, yine de, her adım dikkatle izlediğinizden emin olun.

Kılavuzu, aşağıdaki yordamları içerir:

* Service fabric kümesi dağıtmadan önce devre dışı haberdar olmanız gereken önemli kavramlar.
* Küme hizmeti doku Resource Manager modüllerini kullanarak Azure'da oluşturma.
* Azure Active Directory'yi (Azure AD) ayarlama küme üzerinde yönetim işlemlerini gerçekleştirme kullanıcıların kimliğini doğrulamak için ayarlama.
* Kümeniz için özel bir Azure Resource Manager şablonu yazma ve dağıtmayı.

## <a name="key-concepts-to-be-aware-of"></a>Dikkat edilmesi gereken temel kavramları
Azure'da, Service fabric, olması zorunlu tutulmuştur, bir x509 kullanmak için Küme ve kendi uç noktaları korumak için sertifika. Sertifikalar, Service Fabric’te bir küme ile uygulamalarının çeşitli yönlerini güvenli hale getirmek üzere kimlik doğrulaması ve şifreleme sağlamak için kullanılır. İstemci erişim/kümede dağıtma, yükseltme ve uygulamaları, hizmetleri ve içerdikleri verileri silme gibi yönetim işlemlerini gerçekleştirmek için sertifikalar veya Azure Active Directory kimlik bilgileri kullanabilirsiniz. Bu sertifikaların, istemcilerde paylaşımı önlemek için tek yolu olduğundan Azure Active Directory kullanımını yüksek oranda önerilir.  Service Fabric sertifikaların nasıl kullanıldığını daha fazla bilgi için bkz: [Service Fabric kümesi güvenlik senaryoları][service-fabric-cluster-security].

Service Fabric X.509 sertifikaları güvenli bir küme ve uygulama güvenlik özellikleri sağlamak için kullanır. Kullandığınız [anahtar kasası] [ key-vault-get-started] Azure Service Fabric kümeleri sertifikalarını yönetmek için. 


### <a name="cluster-and-server-certificate-required"></a>Küme ve sunucu sertifikası (gerekli)
Bu sertifikaları (bir birincil ve isteğe bağlı olarak ikincil) bir küme güvenli ve yetkisiz erişimi önlemek için gereklidir. İki yolla küme güvenlik sağlar:

* **Küme kimlik doğrulaması:** düğümü düğümü iletişimin küme Federasyon kimlik doğrulamasını yapar. Yalnızca bu sertifikayla kimliğini kanıtlamak düğümleri kümeye katılmasını sağlayabilir.
* **Sunucu kimlik doğrulaması:** böylece Konuşmayı gerçek küme ve olmayan bir 'adam ortasında' Yönetimi istemci bildiği bir yönetim istemcisi küme yönetim Uç noktalara kimliğini doğrular. Bu sertifika aynı zamanda bir SSL HTTPS yönetim API'si ve Service Fabric Explorer için HTTPS üzerinden sağlar.

Bu amaca hizmet eder için sertifikanın aşağıdaki gereksinimleri karşılamalıdır:

* Sertifika bir özel anahtar içermelidir. Bu sertifikalar genellikle uzantıları .pfx veya .pem sahip  
* Sertifikanın bir kişisel bilgi değişimi (.pfx) dosyası verilebilir anahtar değişimi için oluşturulmuş olması gerekir.
* **Sertifikanın konu adı, Service Fabric kümesi erişmek için kullandığınız etki alanı eşleşmelidir**. Bu eşleşen bir SSL kümenin HTTPS yönetim uç noktası ve Service Fabric Explorer için sağlamak için gereklidir. İçin bir sertifika yetkilisinden (CA) bir SSL sertifikası elde edemiyor *. cloudapp.azure.com etki alanı. Özel etki alanı adı, kümeniz için edinmeniz gerekir. Bir CA'dan bir sertifika isteme, sertifikanın konu adı, kümeniz için kullandığınız özel etki alanı adı eşleşmelidir.

### <a name="set-up-azure-active-directory-for-client-authentication-optional-but-recommended"></a>İstemci kimlik doğrulaması (isteğe bağlı, ancak önerilen) için Azure Active Directory'yi ayarlama ayarlayın

Azure AD (kiracılar da bilinir), kuruluşların uygulamalar kullanıcı erişimini yönetmenizi sağlar. Uygulamaları web tabanlı olanlar oturum açma kullanıcı Arabirimi ve yerel istemci deneyimini olanlar ayrılır. Bu makalede, bir kiracı zaten oluşturduğunuzu varsayalım. Yüklemediyseniz, okuyarak başlamanız [Azure Active Directory kiracısı alma][active-directory-howto-tenant].

Web tabanlı dahil olmak üzere Yönetim işlevselliğini birkaç giriş noktalarını bir Service Fabric kümesi sunar [Service Fabric Explorer] [ service-fabric-visualizing-your-cluster] ve [Visual Studio] [ service-fabric-manage-application-in-visual-studio]. Sonuç olarak, Küme erişimi denetlemek için iki Azure AD uygulama, bir web uygulaması ve bir yerel uygulama oluşturun.

Bu belgenin sonraki bölümlerinde nasıl oluşturulacağı hakkında daha fazla.

### <a name="application-certificates-optional"></a>Uygulama sertifikaları (isteğe bağlı)
Ek sertifikaların herhangi bir sayıda uygulama güvenlik amacıyla bir kümede yüklenebilir. Kümenizi oluşturmadan önce düğümlerine gibi yüklenmesi için bir sertifika gerektiren uygulama güvenlik senaryoları göz önünde bulundurun:

* Şifreleme ve şifre çözme uygulama yapılandırma değerlerini.
* Çoğaltma sırasında şifreleme düğümler arasında veri.

Güvenli küme oluşturma kavramı, Linux oldukları ya da Windows kümeleri aynıdır. 

### <a name="client-authentication-certificates-optional"></a>İstemci kimlik doğrulama sertifikaları (isteğe bağlı)
Ek sertifikaların herhangi bir sayıda yönetici veya kullanıcı istemci işlemleri için belirtilebilir. Varsayılan olarak küme sertifika yönetici istemci ayrıcalıklarına sahiptir. Bu ek istemci sertifikaları kümesine yüklenmemelidir, bunu yalnızca bir küme yapılandırmasında izin olarak belirtilmesi gerekiyor, ancak, bunlar kümeye bağlanın ve herhangi bir yönetim gerçekleştirmek için istemci bilgisayarlarında yüklü olması gerekir işlemler.


## <a name="prerequisites"></a>Önkoşullar 
Güvenli küme oluşturma kavramı, Linux oldukları ya da Windows kümeleri aynıdır. Bu kılavuz, azure powershell veya yeni kümeleri oluşturmak için azure CLI kullanımını kapsar. Önkoşullar ya da bulunmaktadır 

-  [Azure PowerShell 4.1 ve yukarıdaki] [ azure-powershell] veya [Azure CLI 2.0 ve üstü][azure-CLI].
-  Hizmet fabic modülleri burada - ayrıntılarını bulabilirsiniz [AzureRM.ServiceFabric](https://docs.microsoft.com/powershell/module/azurerm.servicefabric) ve [az BT CLI Modülü](https://docs.microsoft.com/cli/azure/sf?view=azure-cli-latest)


## <a name="use-service-fabric-rm-module-to-deploy-a-cluster"></a>Bir küme dağıtmak için Service fabric RM modülü kullanın

Bu belgede, biz service fabric RM powershell kullanır ve birden çok senaryolar için bir küme, powershell veya CLI modülü komutu dağıtmak için CLI modülü sağlar. Bunların her biri bize gidin. Çekme en iyi düşündüğünüz senaryo ihtiyaçlarınıza uygun. 

- Yeni küme oluşturma - bir sistem kullanılarak oluşturulan otomatik olarak imzalanan sertifika
    - Bir varsayılan küme şablonu kullanın
    - zaten bir şablon kullanın
- Zaten sahip olduğunuz bir sertifikayı kullanarak yeni bir küme - oluşturun
    - Bir varsayılan küme şablonu kullanın
    - zaten bir şablon kullanın

### <a name="create-new-cluster----using-a-system-generated-self-signed-certificate"></a>Yeni küme oluşturma - bir sistem kullanılarak oluşturulan otomatik olarak imzalanan sertifika

Varsa sistemin otomatik olarak imzalanan sertifika oluşturmak ve kümenizi güvenliğini sağlamak için kullanmak istediğiniz küme oluşturmak için aşağıdaki komutu kullanın. Bu komut, küme güvenlik ve yönetim erişimi ayarlamak için bu sertifikayı kullanarak yönetim işlemlerini gerçekleştirmek için kullanılan birincil küme sertifika ayarlar.

### <a name="login-in-to-azure"></a>Azure oturum açma.

```Powershell

Login-AzureRmAccount
Set-AzureRmContext -SubscriptionId <guid>

```

```CLI

azure login
az account set --subscription $subscriptionId

```
#### <a name="use-the-default-5-node-1-nodetype-template-that-ships-in-the-module-to-set-up-the-cluster"></a>Kümesini ayarlamak için modülünde birlikte gelen varsayılan 5 düğüm 1 nodetype şablonu kullanın

En az parametrelerini belirterek hızlı bir şekilde, bir küme oluşturmak için aşağıdaki komutu kullanın

Kullanılan şablon kullanılabilir [azure service fabric şablon örnekleri: windows şablonu](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-1-NodeTypes-Secure-NSG) ve [Ubuntu şablonu](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Ubuntu-1-NodeTypes-Secure)

Works Windows ve Linux kümeleri oluşturmak için aşağıdaki komutları, yalnızca işletim sistemi uygun şekilde belirtmeniz gerekir. Powershell / CLI komutları da belirtildiği theCertificateOutputFolder sertifika sertifikada çıkarır. Komut diğer parametre VM SKU gibi de alır.

```Powershell

$resourceGroupLocation="westus"
$resourceGroupName="mycluster"
$vaultName="myvault"
$vaultResourceGroupName="myvaultrg"
$CertSubjectName="mycluster.westus.cloudapp.azure.com"
$certPassword="Password!1" | ConvertTo-SecureString -AsPlainText -Force 
$vmpassword="Password!1" | ConvertTo-SecureString -AsPlainText -Force
$vmuser="myadmin"
$os="WindowsServer2016DatacenterwithContainers"
$certOutputFolder="c:\certificates"

New-AzureRmServiceFabricCluster -ResourceGroupName $resourceGroupName -CertificateOutputFolder $certOutputFolder -CertificatePassword $certpassword -CertificateSubjectName $CertSubjectName -OS $os -VmPassword $vmpassword -VmUserName $vmuser 

```

```CLI

declare resourceGroupLocation="westus"
declare resourceGroupName="mylinux"
declare vaultResourceGroupName="myvaultrg"
declare vaultName="myvault"
declare CertSubjectName="mylinux.westus.cloudapp.azure.com"
declare vmpassword="Password!1"
declare certpassword="Password!1"
declare vmuser="myadmin"
declare vmOs="UbuntuServer1604"
declare certOutputFolder="c:\certificates"



az sf cluster create --resource-group $resourceGroupName --location $resourceGroupLocation  \
    --certificate-output-folder $certOutputFolder --certificate-password $certpassword  \
    --vault-name $vaultName --vault-resource-group $resourceGroupName  \
    --template-file $templateFilePath --parameter-file $parametersFilePath --vm-os $vmOs  \
    --vm-password $vmpassword --vm-user-name $vmuser

```

#### <a name="use-the-custom-template-that-you-already-have"></a>Sahip olduğunuz özel bir şablon kullanmak 

Gereksinimlerinize uygun olarak özel bir şablon Yazar ihtiyacınız varsa, yüksek oranda kullanılabilir şablonlardan birini ile başlamanız önerilir [azure service fabric şablon örnekleri](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master). Kılavuzu ve açıklamalar için izleyin [küme şablonunuzu özelleştirin] [ customize-your-cluster-template] bölümüne bakın.

Zaten bir özel şablon sahip sonra olun emin, tüm üç sertifika şablonu ve parametre dosyası parametrelerinde gibi adlı ilgili çift onay ve değerler null aşağıdaki gibidir.

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


```Powershell


$resourceGroupLocation="westus"
$resourceGroupName="mycluster"
$CertSubjectName="mycluster.westus.cloudapp.azure.com"
$certPassword="Password!1" | ConvertTo-SecureString -AsPlainText -Force 
$certOutputFolder="c:\certificates"

$parameterFilePath="c:\mytemplates\mytemplateparm.json"
$templateFilePath="c:\mytemplates\mytemplate.json"


New-AzureRmServiceFabricCluster -ResourceGroupName $resourceGroupName -CertificateOutputFolder $certOutputFolder -CertificatePassword $certpassword -CertificateSubjectName $CertSubjectName -TemplateFile $templateFilePath -ParameterFile $parameterFilePath 

```

Burada, aynı işlemi gerçekleştirmek için eşdeğer CLI komut verilmiştir. Declare deyimlerini değerlerde uygun değerlerle değiştirin. CLI yukarıdaki powershell komutunu destekleyen diğer tüm parametreleri destekler.

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


### <a name="create-new-cluster---using-the-certificate-you-bought-from-a-ca-or-you-already-have"></a>Yeni küme oluşturma - sertifika kullanarak bir CA'dan satın aldığınız ya da zaten sahip.

Kümenizle güvenliğini sağlamak için kullanmak istediğiniz bir sertifika varsa kümeyi oluşturmak için aşağıdaki komutu kullanın.

Bu, diğer amaçlar için kullanarak ulaşır CA imzalı bir sertifika varsa, özel anahtar kasanız için ayrı kaynak grubu sağlamak önerilir. Anahtar kasası, kendi kaynak grubuna koymak öneririz. Bu eylem, Service Fabric kümesi anahtarları ve gizli anahtarları kaybetmeden içeren kaynak grubunu da dahil olmak üzere işlem ve depolama kaynak grupları kaldırmanıza olanak sağlar. **Anahtar kasanızı içeren kaynak grubunu _aynı bölgede olmalıdır_ tarafından kullanıldığı kümesi olarak.**


#### <a name="use-the-default-5-node-1-nodetype-template-that-ships-in-the-module"></a>Modül birlikte gelen varsayılan 5 düğüm 1 nodetype şablonu kullanın
Kullanılan şablon kullanılabilir [azure örneklerinden: windows şablonu](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-1-NodeTypes-Secure-NSG) ve [Ubuntu şablonu](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Ubuntu-1-NodeTypes-Secure)

```Powershell
$resourceGroupLocation="westus"
$resourceGroupName="mylinux"
$vaultName="myvault"
$vaultResourceGroupName="myvaultrg"
$certPassword="Password!1" | ConvertTo-SecureString -AsPlainText -Force 
$vmpassword=("Password!1" | ConvertTo-SecureString -AsPlainText -Force) 
$vmuser="myadmin"
$os="WindowsServer2016DatacenterwithContainers"

New-AzureRmServiceFabricCluster -ResourceGroupName $resourceGroupName -KeyVaultResouceGroupName $vaultResourceGroupName -KeyVaultName $vaultName -CertificateFile C:\MyCertificates\chackocertificate3.pfx -CertificatePassword $certPassword -OS $os -VmPassword $vmpassword -VmUserName $vmuser 

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
Gereksinimlerinize uygun olarak özel bir şablon Yazar ihtiyacınız varsa, yüksek oranda kullanılabilir şablonlardan birini ile başlamanız önerilir [azure service fabric şablon örnekleri](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master). Kılavuzu ve açıklamalar için izleyin [küme şablonunuzu özelleştirin] [ customize-your-cluster-template] bölümüne bakın.

Zaten bir özel şablon sahip sonra olun emin, tüm üç sertifika şablonu ve parametre dosyası parametrelerinde gibi adlı ilgili çift onay ve değerler null aşağıdaki gibidir.

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


```Powershell

$resourceGroupLocation="westus"
$resourceGroupName="mylinux"
$vaultName="myvault"
$vaultResourceGroupName="myvaultrg"
$certPassword="Password!1" | ConvertTo-SecureString -AsPlainText -Force 
$os="WindowsServer2016DatacenterwithContainers"
$parameterFilePath="c:\mytemplates\mytemplateparm.json"
$templateFilePath="c:\mytemplates\mytemplate.json"
$certificateFile="C:\MyCertificates\chackonewcertificate3.pem"


New-AzureRmServiceFabricCluster -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath -ParameterFile $parameterFilePath -KeyVaultResouceGroupName $vaultResourceGroupName -KeyVaultName $vaultName -CertificateFile $certificateFile -CertificatePassword #certPassword

```

Burada, aynı işlemi gerçekleştirmek için eşdeğer CLI komut verilmiştir. Declare deyimlerini değerlerde uygun değerlerle değiştirin.

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

#### <a name="use-a-pointer-to-the-secret-you-already-have-uploaded-into-the-keyvault"></a>Keyvault Karşıya zaten gizli bir işaretçi kullanın

Var olan bir anahtar kasası kullanmak için _dağıtımı için etkinleştirmeniz gerekir_ sertifikaları elde ve küme düğümlerine yüklemek işlem kaynak sağlayıcısı izin vermek için:

```powershell

Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment


$parameterFilePath="c:\mytemplates\mytemplate.json"
$templateFilePath="c:\mytemplates\mytemplateparm.json"
$secertId="https://test1.vault.azure.net:443/secrets/testcertificate4/55ec7c4dc61a462bbc645ffc9b4b225f"


New-AzureRmServiceFabricCluster -ResourceGroupName $resourceGroup -SecretIdentifier $secretID -TemplateFile $templateFile -ParameterFile $templateParmfile 

```
Burada, aynı işlemi gerçekleştirmek için eşdeğer CLI komut verilmiştir. Declare deyimlerini değerlerde uygun değerlerle değiştirin.

```cli

declare $parameterFilePath="c:\mytemplates\mytemplate.json"
declare $templateFilePath="c:\mytemplates\mytemplateparm.json"
declare $secertId="https://test1.vault.azure.net:443/secrets/testcertificate4/55ec7c4dc61a462bbc645ffc9b4b225f"


az sf cluster create --resource-group $resourceGroupName --location $resourceGroupLocation  \
    --secret-identifieraz $secretID  \
    --template-file $templateFilePath --parameter-file $parametersFilePath 

```

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

Azure AD kiracısı için yönetici ayrıcalıklarına sahip bir hesapla oturum açmak için istenir. Oturum açtıktan sonra Service Fabric kümesi temsil etmek için yerel uygulamalar ve web komut dosyası oluşturur. Kiracının uygulamaları bakarsanız [Azure portal][azure-portal], iki yeni giriş görmeniz gerekir:

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

<a id="customize-arm-template" ></a>

## <a name="create-a-service-fabric-cluster-resource-manager-template"></a>Service Fabric kümesi Resource Manager şablonu oluşturma
Bu bölümde, Service Fabric kümesi Resource Manager şablonu özel olarak isteyen kullanıcıların Yazar aranır. bir şablonu oluşturduktan sonra hala geri dönün ve bunu dağıtmak için powershell veya CLI modülleri kullanın. 

Örnek Resource Manager şablonları kullanılabilir [github'da Azure örneklerinden](https://github.com/Azure-Samples/service-fabric-cluster-templates). Bu şablonlar, küme şablonunuz için bir başlangıç noktası olarak kullanılabilir.

### <a name="create-the-resource-manager-template"></a>Resource Manager şablonu oluşturma
Bu kılavuzu kullanır [5-node güvenli küme] [ service-fabric-secure-cluster-5-node-1-nodetype] örnek şablonu ve şablon parametreleri. Karşıdan `azuredeploy.json` ve `azuredeploy.parameters.json` bilgisayarınıza ve her iki dosyalarını sık kullandığınız metin düzenleyicisinde açın.

### <a name="add-certificates"></a>Sertifikaları Ekle
Sertifika anahtarlarını içeren anahtar kasası başvurarak sertifikaları bir küme Resource Manager şablonuna ekleyin. Bu anahtar kasası parametrelerini ve değerlerini bir Resource Manager şablonu parametreleri dosyasında (azuredeploy.parameters.json) ekleyin. 

#### <a name="add-all-certificates-to-the-virtual-machine-scale-set-osprofile"></a>Sanal makine ölçek kümesi osProfile tüm sertifikaları Ekle
Kümeye yüklü her sertifikanın ölçek kümesi kaynağı (Microsoft.Compute/virtualMachineScaleSets) osProfile bölümünde yapılandırılması gerekir. Bu eylem, sanal makinelerin sertifikayı yüklemek için kaynak sağlayıcısı bildirir. Bu yükleme, küme sertifika ve uygulamalarınız için kullanmayı planladığınız herhangi bir uygulama güvenlik sertifika içerir:

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

#### <a name="configure-the-service-fabric-cluster-certificate"></a>Service Fabric kümesi sertifika yapılandırma
Kümenin kimlik doğrulama sertifikası hem Service Fabric küme kaynağı olarak (Microsoft.ServiceFabric/clusters) yapılandırılması gerekir ve sanal makine ölçek kümesi kaynak sanal makine ölçek için Service Fabric uzantısı ayarlar. Bu düzenlemenin, küme kimlik doğrulaması ve yönetim uç noktaları için sunucu kimlik doğrulaması kullanmak için yapılandırmak Service Fabric kaynak sağlayıcısı sağlar.

##### <a name="add-the-certificate-information-the-virtual-machine-scale-set-resource"></a>Kaynak sertifika bilgilerini sanal makine ölçek kümesine ekleyin:
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

##### <a name="add-the-certificate-information-to-the-service-fabric-cluster-resource"></a>Sertifika bilgilerini Service Fabric küme kaynağı ekleyin:
```json
{
  "apiVersion": "[variables('sfrpApiVersion')]",
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

### <a name="add-azure-ad-configuration-to-use-azure-ad-for-client-access"></a>Azure AD istemci erişimi için kullanılacak Azure AD Yapılandırması Ekle

Sertifika anahtarlarını içeren anahtar kasası başvurarak Azure AD yapılandırma s bir küme Resource Manager şablonuna ekleyin. Bu Azure AD parametrelerini ve değerlerini bir Resource Manager şablonu parametreleri dosyasında (azuredeploy.parameters.json) ekleyin.

```json
{
  "apiVersion": "[variables('sfrpApiVersion')]",
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

### <a name="populate-the-parameter-file-with-the-values"></a>Parametre dosyası değerlerle doldurun.
Son olarak, Parametreler dosyası doldurmak için çıktı değerler anahtar kasası ve Azure AD powershell komutlarını kullanın:

Azure service fabric RM powershell modülleri kullanmayı planlıyorsanız, ardından ihtiyacınız olmayan küme sertifika bilgilerini doldurmak için imzalı self oluşturmak için sistem isterseniz küme güvenlik için sertifika, null olarak kalmasını. 

> [!NOTE]
> RM modülleri almak ve bu boş parametre değerleri doldurmak parametreler çok adları adlarıyla
>

```json
        "clusterCertificateThumbprint": {
            "value": ""
        },
        "clusterCertificateUrlValue": {
            "value": ""
        },
        "sourceVaultvalue": {
            "value": ""
        },
```

Uygulama sertifikaları kullanarak ya da keyvault karşıya yüklediğiniz var olan bir küme kullanıyorsanız, bu bilgileri almak ve bunu doldurmak gerekir 

RM modülleri geneate yeteneği, Azure AD yapılandırma yok. Bu nedenle Azure AD istemci erişimi için kullanmayı planlıyorsanız, bu doldurmak gerekir.

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
Resource Manager şablonu ile bir parametre dosyası sınamak için aşağıdaki PowerShell komutunu kullanın:

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

Sorunla çalıştırın ve şifreli iletileri alma durumunda, daha sonra kullanmak "-Debug" bir seçenek olarak.

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json -Debug
```

Aşağıdaki diyagram, anahtar kasası ve Azure AD yapılandırma Resource Manager şablonunuza nerelerde gösterir.

![Resource Manager bağımlılık Haritası][cluster-security-arm-dependency-map]

## <a name="create-the-cluster-using-azure-resource-template"></a>Azure kaynak şablonu kullanarak küme oluşturma 

Belgenin önceki bölümlerinde açıklanan adımları kullanarak kümeyi şimdi dağıtabilir veya doldurulan değerleri parametre dosyasında varsa, daha sonra artık kullanarak küme oluşturmaya hazırsınız [Azure kaynak şablon dağıtımı] [ resource-group-template-deploy] doğrudan.

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

Sorunla çalıştırın ve şifreli iletileri alma durumunda, daha sonra kullanmak "-Debug" bir seçenek olarak.


<a name="assign-roles"></a>

## <a name="assign-users-to-roles"></a>Kullanıcıları rollere atama
Kümenizin temsil etmek üzere uygulamalar oluşturduktan sonra kullanıcılarınızın Service Fabric tarafından desteklenen rolleri atayın: salt okunur ve yönetici Kullanarak roller atayabilirsiniz [Azure portal][azure-portal].

1. Azure portalında kiracınızın sağ üst köşede seçin.

    ![Kiracı düğmesini seçin][select-tenant-button]
2. Seçin **Azure Active Directory** sol sekmesini ve ardından seçin "Kurumsal uygulamalar".
3. "Tüm uygulamaları" seçin ve ardından bulmak ve bir ada sahip web uygulaması seçin gibi `myTestCluster_Cluster`.
4. Tıklatın **kullanıcılar ve gruplar** sekmesi.

    ![Kullanıcılar ve Gruplar sekmesinde][users-and-groups-tab]
5. Tıklatın **Kullanıcı Ekle** düğmesini yeni sayfada, bir kullanıcı rolü atayın ve ardından seçip **seçin** altındaki sayfasının düğmesini.

    ![Kullanıcı rolleri sayfasına atama][assign-users-to-roles-page]
6. Tıklatın **atamak** altındaki sayfasının düğmesini.

    ![Atama onay Ekle][assign-users-to-roles-confirm]

> [!NOTE]
> Service Fabric rolleri hakkında daha fazla bilgi için bkz: [Service Fabric istemciler için rol tabanlı erişim denetimi](service-fabric-cluster-security-roles.md).
>
>


## <a name="troubleshooting-help-in-setting-up-azure-active-directory"></a>Azure Active Directory'yi ayarlama hakkında Yardım sorunlarını giderme
Burada; bu nedenle bazı işaretçiler sorunu hata ayıklamak için yapabilecekleriniz üzerinde Azure AD ayarlama ve kullanılarak, zor olabilir.

### <a name="service-fabric-explorer-prompts-you-to-select-a-certificate"></a>Service Fabric Explorer bir sertifika seçmenizi ister
#### <a name="problem"></a>Sorun
Başarılı bir şekilde Azure ad Service Fabric Explorer'da oturum açtıktan sonra tarayıcı giriş sayfasına döndürüyor ancak bir ileti bir sertifika seçmenizi ister.

![SFX sertifika iletişim kutusu][sfx-select-certificate-dialog]

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
AAD sayfasında "Uygulamanın kayıtlar" seçin, küme uygulamanızı seçin ve ardından **yanıt URL'leri** düğmesi. "Yanıt URL'leri" sayfasında, listedeki öğelerden birini değiştirin veya Service Fabric Explorer URL listeye ekleyin. İşiniz bittiğinde, değişikliğinizi kaydedin.

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

## <a name="next-steps"></a>Sonraki adımlar
Bu noktada, Azure Active Directory sağlayan yönetim kimlik doğrulama ile güvenli bir küme var. Ardından, [kümenize bağlanmak](service-fabric-connect-to-secure-cluster.md) ve öğrenin nasıl [uygulama parolaları yönetme](service-fabric-application-secret-management.md).


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

