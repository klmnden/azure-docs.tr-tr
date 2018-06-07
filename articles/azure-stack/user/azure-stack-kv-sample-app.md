---
title: Azure yığın anahtar kasası parolaları almak uygulama izin | Microsoft Docs
description: Azure yığın anahtar kasası ile çalışmak için örnek bir uygulama kullanın
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 3748b719-e269-4b48-8d7d-d75a84b0e1e5
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/11/2018
ms.author: mabrigg
ms.openlocfilehash: 39bce286c756660cd8755358cf98f2c8d35ce351
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2018
ms.locfileid: "34807389"
---
# <a name="a-sample-application-that-uses-keys-and-secrets-stored-in-a-key-vault"></a>Anahtarlar ve gizli anahtar kasasında depolanan kullanan örnek bir uygulama

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Anahtarlar ve gizli bir anahtar kasası Azure yığınında alır bir örnek uygulamayı (HelloKeyVault) çalıştırmak için bu makaledeki adımları izleyin.

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki Önkoşullar Azure yığınından yükleyebilirsiniz [Geliştirme Seti](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), veya kullanıyorsanız Windows tabanlı bir dış istemcinin [VPN üzerinden bağlı](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn):

* Yükleme [Azure yığın uyumlu Azure PowerShell modülleri](azure-stack-powershell-install.md).
* Karşıdan [Azure yığın ile çalışmak için gereken araçları](azure-stack-powershell-download.md).

## <a name="create-and-get-the-key-vault-and-application-settings"></a>Oluşturma ve anahtar kasasını ve uygulama ayarlarını al

Örnek uygulama için hazırlamak için:

* Bir anahtar kasası Azure yığınında oluşturun.
* Bir uygulamayı Azure Active Directory (Azure AD) kaydedin.

Örnek uygulama için hazırlamak için Azure portal veya PowerShell kullanın. Bu makalede, bir anahtar kasası oluşturma ve PowerShell kullanarak bir uygulamayı kaydetme gösterilmektedir.

>[!NOTE]
>Varsayılan olarak, PowerShell Betiği Active Directory'de yeni bir uygulama oluşturur. Ancak, mevcut uygulamalarınızı birini kaydedebilirsiniz.

 Aşağıdaki komut dosyasını çalıştırmadan önce için değerleri belirtin emin olun `aadTenantName` ve `applicationPassword` değişkenleri. İçin bir değer belirtmezseniz `applicationPassword`, bu komut dosyası rastgele bir parola oluşturur.

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
Add-AzureRmAccount `
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

# Create a new resource group and a key vault in that resource group.
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

Sonraki ekran yakalama anahtar kasası oluşturmak için kullanılan komut dosyasından çıkış şunları gösterir:

![Anahtar kasası ile erişim tuşları](media/azure-stack-kv-sample-app/settingsoutput.png)

Not **VaultUrl**, **AuthClientId**, ve **AuthClientSecret** önceki komut dosyası tarafından döndürülen değer. Bu değerleri HelloKeyVault uygulamayı çalıştırmak için kullanın.

## <a name="download-and-configure-the-sample-application"></a>İndirme ve örnek uygulamayı yapılandırma

Azure anahtar kasası örnek indirme [anahtar kasası istemci örnekleri](https://www.microsoft.com/en-us/download/details.aspx?id=45343) sayfası. .Zip dosyası, geliştirme iş istasyonunuzdaki içeriğini ayıklayın. Samples klasöründeki iki uygulama vardır, bu makalede HelloKeyVault kullanır.

HelloKeyVault örnek yüklemek için:

* Gözat **Microsoft.Azure.KeyVault.Samples** > **örnekleri** > **HelloKeyVault** klasör.
* HelloKeyVault uygulamayı Visual Studio'da açın.

### <a name="configure-the-sample-application"></a>Örnek uygulamayı yapılandırma

Visual Studio'da:

* HelloKeyVault\App.config dosyasını açın ve bulmak için Gözat &lt; **appSettings** &gt; öğesi.
* Güncelleştirme **VaultUrl**, **AuthClientId**, ve **AuthClientSecret** anahtar kasası oluşturmak için kullanılan tarafından döndürülen değerlerle anahtarları. (Varsayılan olarak, App.config dosyası için bir yer tutucu sahip *AuthCertThumbprint*. Bu yer tutucu ile Değiştir *AuthClientSecret*.)

  ![Uygulama ayarları](media/azure-stack-kv-sample-app/appconfig.png)

* Çözümü yeniden derleyin.

## <a name="run-the-application"></a>Uygulamayı çalıştırma

HelloKeyVault çalıştırdığınızda, uygulama için Azure AD oturum açtığında ve Azure yığınında anahtar kasası kimlik doğrulaması için bu AuthClientSecret belirteci kullanır.

HelloKeyVault örnek kullanabilirsiniz:

* Anahtarları ve gizli anahtarları gibi oluştururken, şifreleme, sarmalama ve silme temel işlemleri gerçekleştirir.
* Parametreler gibi *şifrelemek* ve *şifresini* HelloKeyVault için ve bir anahtar kasası için belirtilen değişiklikleri uygulayın.

## <a name="next-steps"></a>Sonraki adımlar

[Anahtar Kasası parolası ile VM dağıtma](azure-stack-kv-deploy-vm-with-secret.md)

[Anahtar kasası sertifikayla bir VM'yi dağıtmak](azure-stack-kv-push-secret-into-vm.md)
