---
title: "Azure yığın anahtar kasası parolaları almak uygulama izin | Microsoft Docs"
description: "Azure yığın anahtar kasası ile çalışmak için örnek bir uygulama kullanın"
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 3748b719-e269-4b48-8d7d-d75a84b0e1e5
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/26/2017
ms.author: mabrigg
ms.openlocfilehash: 50103dca21d047c5cee211b2250e750739131bc1
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="sample-application-that-uses-keys-and-secrets-stored-in-a-key-vault"></a>Anahtarlar ve gizli anahtar kasasında depolanan kullanan örnek uygulama

Bu makalede, sizi, anahtarlar ve gizli bir anahtar kasası Azure yığınında alır örnek bir uygulama (HelloKeyVault) nasıl çalıştırılacağını gösterir.

## <a name="prerequisites"></a>Ön koşullar 

Aşağıdaki Önkoşullar herhangi birinden çalıştırmak [Geliştirme Seti](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), veya kullanıyorsanız Windows tabanlı bir dış istemcinin [VPN üzerinden bağlı](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn):

* Yükleme [Azure yığın uyumlu Azure PowerShell modülleri](azure-stack-powershell-install.md).  
* Karşıdan [Azure yığın ile çalışmak için gereken araçları](azure-stack-powershell-download.md). 

## <a name="create-and-get-the-key-vault-and-application-settings"></a>Oluşturma ve anahtar kasasını ve uygulama ayarlarını al

İlk olarak, Azure yığınında bir anahtar kasası oluşturun ve bir uygulamayı Azure Active Directory (Azure AD) kaydetme gerekir. Oluşturun ve anahtar kasalarını Azure portal veya PowerShell kullanarak kaydedin. Bu makalede görevleri gerçekleştirmek için PowerShell yolunu gösterir. Varsayılan olarak, bu PowerShell Betiği Active Directory'de yeni bir uygulama oluşturur. Ancak, mevcut uygulamalarınızı birini de kullanabilirsiniz. İçin bir değer verdiğinizden emin olun `aadTenantName` ve `applicationPassword` değişkenleri. İçin bir değer belirtmezseniz `applicationPassword` değişkeni, bu komut dosyası rastgele bir parola oluşturur. 

```powershell
$vaultName           = 'myVault'
$resourceGroupName   = 'myResourceGroup'
$applicationName     = 'myApp'
$location            = 'local' 

# Password for the application. If not specified, this script will generate a random password during app creation.
$applicationPassword = '' 
                         
# Function to generate a random password for the application.
Function GenerateSymmetricKey()
{
    $key = New-Object byte[](32)
    $rng = [System.Security.Cryptography.RNGCryptoServiceProvider]::Create()
    $rng.GetBytes($key)
    return [System.Convert]::ToBase64String($key)
}

Write-Host 'Please log into your Azure Stack user environment' -foregroundcolor Green

$tenantARM = "https://management.local.azurestack.external"
$aadTenantName = "PLEASE FILL THIS IN WITH YOUR AAD TENANT NAME. FOR EXAMPLE: myazurestack.onmicrosoft.com"

# Configure the Azure Stack operator’s PowerShell environment.
Add-AzureRMEnvironment `
  -Name "AzureStackUser" `
  -ArmEndpoint $tenantARM

Set-AzureRmEnvironment `
  -Name "AzureStackAdmin" `
  -GraphAudience "https://graph.windows.net/"

$TenantID = Get-AzsDirectoryTenantId `
  -AADTenantName $aadTenantName `
  -EnvironmentName AzureStackUser

# Sign in to the user portal.
Login-AzureRmAccount `
  -EnvironmentName "AzureStackUser" `
  -TenantId $TenantID `
  
$now = [System.DateTime]::Now
$oneYearFromNow = $now.AddYears(1)

$applicationPassword = GenerateSymmetricKey
    
# Create a new Azure AD application.
$identifierUri = [string]::Format("http://localhost:8080/{0}",[Guid]::NewGuid().ToString("N"))
$homePage = "http://contoso.com"

Write-Host "Creating a new AAD Application"
$ADApp = New-AzureRmADApplication `
  -DisplayName $applicationName `
  -HomePage $homePage `
  -IdentifierUris $identifierUri `
  -StartDate $now `
  -EndDate $oneYearFromNow `
  -Password $applicationPassword

Write-Host "Creating a new AAD service principal"
$servicePrincipal = New-AzureRmADServicePrincipal `
  -ApplicationId $ADApp.ApplicationId

# Create a new resource group and a key vault within that resource group.
New-AzureRmResourceGroup `
  -Name $resourceGroupName `
  -Location $location   

Write-Host "Creating vault $vaultName"
$vault = New-AzureRmKeyVault -VaultName $vaultName `
  -ResourceGroupName $resourceGroupName `
  -Sku standard `
  -Location $location

# Specify full privileges to the vault for the application.
Write-Host "Setting access policy"
Set-AzureRmKeyVaultAccessPolicy -VaultName $vaultName `
  -ObjectId $servicePrincipal.Id `
  -PermissionsToKeys all `
  -PermissionsToSecrets all

Write-Host "Paste the following settings into the app.config file for the HelloKeyVault project:"
'<add key="VaultUrl" value="' + $vault.VaultUri + '"/>'
'<add key="AuthClientId" value="' + $servicePrincipal.ApplicationId + '"/>'
'<add key="AuthClientSecret" value="' + $applicationPassword + '"/>'
Write-Host

``` 

Aşağıdaki ekran görüntüsü önceki betiği çıktısını gösterir:

![Uygulama yapılandırması](media/azure-stack-kv-sample-app/settingsoutput.png)

Not **VaultUrl**, **AuthClientId**, ve **AuthClientSecret** önceki komut dosyası tarafından döndürülen değer. Bu değerleri HelloKeyVault uygulamayı çalıştırmak için kullanın.

## <a name="download-and-run-the-sample-application"></a>Karşıdan yükleme ve örnek uygulamayı çalıştırma

Azure anahtar kasası örnek indirme [anahtar kasası istemci örnekleri](https://www.microsoft.com/en-us/download/details.aspx?id=45343) sayfası. Geliştirme iş istasyonunuzu üzerine .zip dosyasının içeriğini ayıklayın. İki örnek örnekleri klasördeki vardır. Bu makalede HellpKeyVault örnek kullanırız. Gözat **Microsoft.Azure.KeyVault.Samples** > **örnekleri** > **HelloKeyVault** klasörü ve açık HelloKeyVault uygulama Visual Studio'da. 

HelloKeyVault\App.config dosyasını açın ve değerlerini değiştirmek <appSettings> öğeyle **VaultUrl**, **AuthClientId**, ve **AuthClientSecret** değerleri önceki komut dosyası tarafından döndürülen. Varsayılan olarak için bir yer tutucu App.config içerdiğine dikkat edin *AuthCertThumbprint*, ancak *AuthClientSecret* yerine. Ayarları değiştirdikten sonra çözümü yeniden derleyin ve uygulamayı başlatın.

![Uygulama ayarları](media/azure-stack-kv-sample-app/appconfig.png)
 
Uygulama için Azure AD oturum açtığında ve Azure yığınında anahtar kasası kimlik doğrulaması için belirtecini kullanır. Uygulama oluşturma, şifreleme, sarmalama ve anahtarları ve gizli anahtar kasasının silme gibi işlemleri gerçekleştirir. Belirli parametreleri gibi geçirebilirsiniz *şifrelemek* ve *şifresini* uygulamaya hangi sağlar uygulamayı yalnızca bu işlemler kasa karşı yürütür. 


## <a name="next-steps"></a>Sonraki adımlar
[Anahtar Kasası parolası ile VM dağıtma](azure-stack-kv-deploy-vm-with-secret.md)

[Anahtar kasası sertifikayla bir VM'yi dağıtmak](azure-stack-kv-push-secret-into-vm.md)



