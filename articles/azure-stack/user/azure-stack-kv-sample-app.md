---
title: Azure Stack Key Vault gizli dizilerini alma uygulamaları | Microsoft Docs
description: Azure Stack anahtar kasası ile çalışmak için örnek uygulama kullanma
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 3748b719-e269-4b48-8d7d-d75a84b0e1e5
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/27/2018
ms.author: sethm
ms.openlocfilehash: b17f6301a41dbb1f64edf9d027dff0f57c09282c
ms.sourcegitcommit: 9f87a992c77bf8e3927486f8d7d1ca46aa13e849
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/28/2018
ms.locfileid: "53808783"
---
# <a name="a-sample-application-that-uses-keys-and-secrets-stored-in-a-key-vault"></a>Anahtarlar ve gizli anahtar kasasında depolanan kullanan bir örnek uygulaması

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Adlandırılmış bir örnek uygulamayı çalıştırmak için bu makaledeki adımları **HelloKeyVault** , anahtarları alır ve gizli diziler bir anahtar kasası Azure Stack'te.

## <a name="prerequisites"></a>Önkoşullar

Azure yığını aşağıdaki önkoşulları yükleyebilirsiniz [Geliştirme Seti](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), ya da Eğer bir Windows tabanlı dış istemciden [VPN üzerinden bağlı](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn):

* Yükleme [Azure Stack ile uyumlu Azure PowerShell modüllerini](azure-stack-powershell-install.md).
* İndirme [Azure Stack ile çalışması için gereken araçları](azure-stack-powershell-download.md).

## <a name="create-and-get-the-key-vault-and-application-settings"></a>Oluşturma ve anahtar kasasını ve uygulama ayarlarını alma

Örnek uygulama için hazırlamak için:

* Azure Stack'te bir anahtar kasası oluşturma.
* Bir uygulamayı Azure Active Directory (Azure AD) kaydedin.

Örnek uygulama için hazırlamak için Azure portalını veya PowerShell'i kullanabilirsiniz. Bu makalede, bir anahtar kasası oluşturma ve PowerShell kullanarak bir uygulamayı kaydetme işlemleri gösterilmektedir.

>[!NOTE]
>Varsayılan olarak, PowerShell Betiği Active Directory'de yeni bir uygulama oluşturur. Bununla birlikte, mevcut uygulamalarınızı birini kaydedebilirsiniz.

Aşağıdaki komut dosyasını çalıştırmadan önce değerleri için sağladığınız emin olun `aadTenantName` ve `applicationPassword` değişkenleri. İçin bir değer belirtmezseniz `applicationPassword`, bu betik, rastgele bir parola oluşturur.

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
$aadTenantName = "FILL THIS IN WITH YOUR AAD TENANT NAME. FOR EXAMPLE: myazurestack.onmicrosoft.com"

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

Aşağıdaki görüntüde, anahtar kasası oluşturmak için kullanılan komut dosyası çıktısı gösterilmektedir:

![Anahtar kasası ile erişim tuşları](media/azure-stack-kv-sample-app/settingsoutput.png)

Not **VaultUrl**, **AuthClientId**, ve **AuthClientSecret** önceki komut dosyası tarafından döndürülen değer. HelloKeyVault uygulamayı çalıştırmak için şu değerleri kullanın.

## <a name="download-and-configure-the-sample-application"></a>İndirme ve örnek uygulamayı yapılandırma

Azure anahtar kasası örneği indirin [anahtar kasası istemci örnekleri](https://www.microsoft.com/download/details.aspx?id=45343) sayfası. Geliştirme iş istasyonunuzda .zip dosyasının içeriğini ayıklayın. Örnekler klasörüne iki uygulama vardır. Bu makalede **HelloKeyVault**.

Yüklenecek **HelloKeyVault** örnek:

* Gözat **Microsoft.Azure.KeyVault.Samples** > **örnekleri** > **HelloKeyVault** klasör.
* Açık **HelloKeyVault** Visual Studio'da uygulama.

### <a name="configure-the-sample-application"></a>Örnek uygulamayı yapılandırma

Visual Studio'da:

* HelloKeyVault\App.config dosyasını açın ve bulma &lt; **appSettings** &gt; öğesi.
* Güncelleştirme **VaultUrl**, **AuthClientId**, ve **AuthClientSecret** anahtarlarla anahtar kasası oluşturmak için kullanılan olanlar tarafından döndürülen değerler. Varsayılan olarak, App.config dosyası için bir yer tutucu sahip `AuthCertThumbprint`. Bu yer tutucu ile değiştirin `AuthClientSecret`.

  ![Uygulama ayarları](media/azure-stack-kv-sample-app/appconfig.png)

* Çözümü yeniden derleyin.

## <a name="run-the-application"></a>Uygulamayı çalıştırma

HelloKeyVault çalıştırdığınızda, uygulamanın Azure AD'ye açar ve sonra anahtar kasasına Azure Stack'te kimliğini doğrulamak için AuthClientSecret belirtecini kullanır.

HelloKeyVault örneğe kullanabilirsiniz:

* Anahtarları ve gizli anahtarları gibi oluştururken, şifrelemek, Kaydır ve silme temel işlemleri gerçekleştirin.
* Gibi parametreler `encrypt` ve `decrypt` HelloKeyVault için ve bir anahtar kasasına belirtilen değişiklikleri uygulayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Anahtar Kasası parolası ile VM dağıtma](azure-stack-kv-deploy-vm-with-secret.md)
- [Anahtar kasası sertifikası ile VM dağıtma](azure-stack-kv-push-secret-into-vm.md)
