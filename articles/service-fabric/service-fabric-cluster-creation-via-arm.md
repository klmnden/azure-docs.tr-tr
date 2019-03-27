---
title: Bir Azure Service Fabric kümesi oluşturma | Microsoft Docs
description: Azure Resource Manager'ı kullanarak azure'da güvenli bir Service Fabric kümesi ayarlama konusunda bilgi edinin.  Varsayılan bir şablon kullanarak veya kendi küme şablonu kullanarak bir küme oluşturabilirsiniz.
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
ms.date: 08/16/2018
ms.author: aljo
ms.openlocfilehash: 4ebd53db9622c5a40f67cba04aa35cbfbaa78c8d
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58446098"
---
# <a name="create-a-service-fabric-cluster-using-azure-resource-manager"></a>Azure Resource Manager kullanarak bir Service Fabric kümesi oluşturma 
> [!div class="op_single_selector"]
> * [Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
> * [Azure portal](service-fabric-cluster-creation-via-portal.md)
>
>

Bir [Azure Service Fabric kümesi](service-fabric-deploy-anywhere.md) bir ağa bağlı sanal makinelerin, mikro hizmetlerin dağıtılıp yönetildiği kümesidir.  Azure'da çalışan bir Service Fabric kümesi, bir Azure kaynağıdır ve Azure Resource Manager kullanılarak dağıtılır. Bu makalede, Resource Manager'ı kullanarak azure'da güvenli bir Service Fabric kümesi dağıtmayı açıklar. Varsayılan bir küme şablon veya özel bir şablon kullanabilirsiniz.  Özel bir şablon yoksa, şunları yapabilirsiniz [nasıl oluşturacağınızı öğrenin](service-fabric-cluster-creation-create-template.md).

Küme güvenliği, kümenin ilk kurulum ve daha sonra değiştirilemez yapılandırılır. Salt okunur bir küme ayarı önce [Service Fabric kümesi güvenlik senaryoları][service-fabric-cluster-security]. Azure'da Service Fabric kullanımları x509 güvenli kümenize ve onun uç noktaları, istemcilerin kimliğini doğrulamak için sertifika ve şifrelersiniz. Ayrıca, Azure Active Directory Yönetim uç noktalarına erişimi güvenli hale getirmek için tavsiye edilir. Azure AD kiracılar ve kullanıcılar küme oluşturmadan önce oluşturulmalıdır.  Daha fazla bilgi için okuma [istemcilerin kimliğini doğrulamak için Azure AD'yi ayarlarken ayarlamak](service-fabric-cluster-creation-setup-aad.md).

Üretim iş yüklerini çalıştırmak için bir üretim kümesi oluşturuyorsanız, ilk okuma öneririz [üretim hazırlık denetim](service-fabric-production-readiness-checklist.md).

## <a name="prerequisites"></a>Önkoşullar 
Bu makalede, bir küme dağıtmak için Service Fabric RM powershell veya Azure CLI modüllerini kullanın:

* [Azure PowerShell 4.1 ve üzeri][azure-powershell]
* [Azure CLI Sürüm 2.0 ve üzeri][azure-CLI]

Service Fabric modülleri için başvuru belgeleri bulabilirsiniz:
* [AzureRM.ServiceFabric](https://docs.microsoft.com/powershell/module/azurerm.servicefabric)
* [az SF CLI Modülü](https://docs.microsoft.com/cli/azure/sf?view=azure-cli-latest)

### <a name="sign-in-to-azure"></a>Azure'da oturum açma

Komutlardan herhangi birine bu makaledeki çalıştırmadan önce öncelikle Azure'da oturum açın.

```powershell
Connect-AzureRmAccount
Set-AzureRmContext -SubscriptionId <subscriptionId>
```

```azurecli
az login
az account set --subscription $subscriptionId
```

## <a name="create-a-new-cluster-using-a-system-generated-self-signed-certificate"></a>Sistem tarafından oluşturulan kendinden imzalı bir sertifika kullanarak yeni bir küme oluşturun

Sistem tarafından oluşturulan kendinden imzalı bir sertifika ile güvenli bir küme oluşturmak için aşağıdaki komutları kullanın. Bu komut, bu sertifikayı kullanarak yönetim işlemlerini gerçekleştirmek için küme güvenlik ve yönetim erişimi ayarlamak için kullanılan bir birincil küme sertifikası ayarlar.  Otomatik olarak imzalanan sertifikalar, test kümeleri güvenliğini sağlamak için kullanışlıdır.  Üretim kümeleri bir sertifika yetkilisinden (CA) bir sertifika ile güvenli hale getirilmelidir.

### <a name="use-the-default-cluster-template-that-ships-in-the-module"></a>Modülde birlikte gelen varsayılan küme şablonu kullanın

Varsayılan şablonu kullanarak olabildiğince az parametre belirterek hızlı bir şekilde, bir küme oluşturmak için aşağıdaki komutu kullanın.

Kullanılan şablon edinilebilir [Azure Service Fabric şablonu örnekleri: windows şablonu](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-1-NodeTypes-Secure-NSG) ve [Ubuntu şablonu](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Ubuntu-1-NodeTypes-Secure)

Aşağıdaki komutu ya da Windows oluşturabilir veya Linux kümeleri, işletim sistemi uygun şekilde belirtmeniz gerekir. PowerShell/CLI komutları da belirtilen sertifika çıkış *CertificateOutputFolder*; ancak, önceden oluşturulmuş emin sertifika klasör oluşturun. Komut diğer parametre VM SKU gibi de alır.

> [!NOTE]
> Aşağıdaki PowerShell komutu, yalnızca Azure Resource Manager PowerShell ile çalışır sürüm > 6.1. Geçerli Azure Resource Manager PowerShell sürümü denetlemek için "Get-Module AzureRM" aşağıdaki PowerShell komutunu çalıştırın. İzleyin [bu bağlantıyı](/powershell/azure/azurerm/install-azurerm-ps) , Azure Resource Manager PowerShell sürümüne yükseltmek için. 
>
>

PowerShell kullanarak kümeye dağıtın:

```powershell
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

Azure CLI kullanarak kümeye dağıtın:

```azurecli
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

### <a name="use-your-own-custom-template"></a>Kendi özel şablonunuzu kullanın

Gereksinimlerinize göre özel bir şablon oluşturmak ihtiyacınız varsa mevcut şablonlardan birini ile Başlat önerilir [Azure Service Fabric şablonu örnekleri](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master). Bilgi edinmek için nasıl [küme şablonunuzu özelleştirme][customize-your-cluster-template].

Özel bir şablon zaten varsa, tüm üç sertifika şablonu ve parametre dosyası Parametreler şu şekilde adlandırılır ve aşağıdaki gibi null değerler ilgili denetleyin:

```json
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

PowerShell kullanarak kümeye dağıtın:

```powershell
$resourceGroupLocation="westus"
$resourceGroupName="mycluster"
$CertSubjectName="mycluster.westus.cloudapp.azure.com"
$certPassword="Password!1" | ConvertTo-SecureString -AsPlainText -Force 
$certOutputFolder="c:\certificates"

$parameterFilePath="c:\mytemplates\mytemplateparm.json"
$templateFilePath="c:\mytemplates\mytemplate.json"

New-AzureRmServiceFabricCluster -ResourceGroupName $resourceGroupName -CertificateOutputFolder $certOutputFolder -CertificatePassword $certpassword -CertificateSubjectName $CertSubjectName -TemplateFile $templateFilePath -ParameterFile $parameterFilePath 
```

Azure CLI kullanarak kümeye dağıtın:

```azurecli
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

## <a name="create-a-new-cluster-using-your-own-x509-certificate"></a>Kendi X.509 sertifikası kullanarak yeni bir küme oluşturun

Kümenizle güvenliğini sağlamak için kullanmak istediğiniz bir sertifika varsa kümesi oluşturmak için aşağıdaki komutu kullanın.

Bu, diğer amaçlar için kullanarak son bulur, CA imzalı bir sertifika ise, özellikle anahtar kasanız için ayrı bir kaynak grubuna sağlamanız önerilir. Anahtar Kasası'nı kendi kaynak grubuna yerleştirin öneririz. Bu eylem, anahtarları ve gizli bilgilerinizi kaybetmeden Service Fabric kümenizi içeren kaynak grubunu da dahil olmak üzere bilgi işlem ve depolama kaynak gruplarını kaldırmaya olanak tanır. **Anahtar kasanızı içeren kaynak grubunu *aynı bölgede olmalıdır* onu kullanarak bir küme.**

### <a name="use-the-default-five-node-one-node-type-template-that-ships-in-the-module"></a>Varsayılan beş düğüm, modüldeki birlikte gelen bir düğüm türü şablonu kullanın
Kullanılan şablon edinilebilir [Azure örnekleri: Windows şablon](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-1-NodeTypes-Secure-NSG) ve [Ubuntu şablonu](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Ubuntu-1-NodeTypes-Secure)

PowerShell kullanarak kümeye dağıtın:

```powershell
$resourceGroupLocation="westus"
$resourceGroupName="mylinux"
$vaultName="myvault"
$vaultResourceGroupName="myvaultrg"
$certPassword="Password!1" | ConvertTo-SecureString -AsPlainText -Force 
$vmpassword=("Password!4321" | ConvertTo-SecureString -AsPlainText -Force) 
$vmuser="myadmin"
$os="WindowsServer2016DatacenterwithContainers"

New-AzureRmServiceFabricCluster -ResourceGroupName $resourceGroupName -Location $resourceGroupLocation -KeyVaultResourceGroupName $vaultResourceGroupName -KeyVaultName $vaultName -CertificateFile C:\MyCertificates\chackocertificate3.pfx -CertificatePassword $certPassword -OS $os -VmPassword $vmpassword -VmUserName $vmuser 
```

Azure CLI kullanarak kümeye dağıtın:

```azurecli
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

### <a name="use-your-own-custom-cluster-template"></a>Kendi özel küme şablonu kullanın
Gereksinimlerinize göre özel bir şablon oluşturmak ihtiyacınız varsa mevcut şablonlardan birini ile Başlat önerilir [Azure Service Fabric şablonu örnekleri](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master). Bilgi edinmek için nasıl [küme şablonunuzu özelleştirme][customize-your-cluster-template].

Zaten sahip özel bir şablon sonra olun emin olun, tüm üç sertifika şablonu ve parametre dosyası Parametreler şu şekilde adlandırılır ilgili kontrol edin ve değerleri null gibidir.

```json
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

PowerShell kullanarak kümeye dağıtın:

```powershell
$resourceGroupLocation="westus"
$resourceGroupName="mylinux"
$vaultName="myvault"
$vaultResourceGroupName="myvaultrg"
$certPassword="Password!1" | ConvertTo-SecureString -AsPlainText -Force 
$os="WindowsServer2016DatacenterwithContainers"
$parameterFilePath="c:\mytemplates\mytemplateparm.json"
$templateFilePath="c:\mytemplates\mytemplate.json"
$certificateFile="C:\MyCertificates\chackonewcertificate3.pem"

New-AzureRmServiceFabricCluster -ResourceGroupName $resourceGroupName -Location $resourceGroupLocation -TemplateFile $templateFilePath -ParameterFile $parameterFilePath -KeyVaultResourceGroupName $vaultResourceGroupName -KeyVaultName $vaultName -CertificateFile $certificateFile -CertificatePassword $certPassword
```

Azure CLI kullanarak kümeye dağıtın:

```azurecli
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

### <a name="use-a-pointer-to-a-secret-uploaded-into-a-key-vault"></a>Bir anahtar kasasına yüklenmiş bir gizli dizi için bir işaretçi kullanın

Var olan bir anahtar Kasası'nı kullanmak için anahtar kasası olmalıdır [dağıtım için etkin](../key-vault/key-vault-manage-with-cli2.md#bkmk_KVperCLI) işlem kaynak sağlayıcısı sertifikaları almak ve küme düğümlerine yüklemek izin vermek için.

PowerShell kullanarak kümeye dağıtın:

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment

$parameterFilePath="c:\mytemplates\mytemplate.json"
$templateFilePath="c:\mytemplates\mytemplateparm.json"
$secretID="https://test1.vault.azure.net:443/secrets/testcertificate4/55ec7c4dc61a462bbc645ffc9b4b225f"

New-AzureRmServiceFabricCluster -ResourceGroupName $resourceGroupName -SecretIdentifier $secretId -TemplateFile $templateFilePath -ParameterFile $parameterFilePath 
```

Azure CLI kullanarak kümeye dağıtın:

```azurecli
declare $resourceGroupName = "testRG"
declare $parameterFilePath="c:\mytemplates\mytemplate.json"
declare $templateFilePath="c:\mytemplates\mytemplateparm.json"
declare $secertId="https://test1.vault.azure.net:443/secrets/testcertificate4/55ec7c4dc61a462bbc645ffc9b4b225f"

az sf cluster create --resource-group $resourceGroupName --location $resourceGroupLocation  \
    --secret-identifier az $secretID  \
    --template-file $templateFilePath --parameter-file $parametersFilePath 
```

## <a name="next-steps"></a>Sonraki adımlar
Bu noktada, Azure'da çalışan güvenli bir kümeye sahip. İleri [kümenize bağlanın](service-fabric-connect-to-secure-cluster.md) ve bilgi edinmek için nasıl [uygulama parolalarını yönetme](service-fabric-application-secret-management.md).

JSON söz dizimi ve bir şablonu kullanmak için özellikler için bkz: [Microsoft.ServiceFabric/clusters şablon başvurusu](/azure/templates/microsoft.servicefabric/clusters).

<!-- Links -->
[azure-powershell]:https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps
[azure-CLI]:https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[customize-your-cluster-template]: service-fabric-cluster-creation-create-template.md
