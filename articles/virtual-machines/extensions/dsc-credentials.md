---
title: Kimlik bilgileri Desired State Configuration ' ı kullanarak Azure'a geçirin
description: Güvenli bir şekilde PowerShell Desired State Configuration (DSC) kullanarak Azure sanal makineleri için kimlik bilgilerini geçirmek hakkında bilgi edinin.
services: virtual-machines-windows
documentationcenter: ''
author: bobbytreed
manager: carmonm
editor: ''
tags: azure-resource-manager
keywords: DSC
ms.assetid: ea76b7e8-b576-445a-8107-88ea2f3876b9
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 05/02/2018
ms.author: robreed
ms.openlocfilehash: 723d0cfe6e292c4b8013de4da55779a6c675d610
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64705935"
---
# <a name="pass-credentials-to-the-azure-dscextension-handler"></a>Kimlik bilgilerini geçirmek için Azure DSCExtension işleyicisi

Bu makale için Azure Desired State Configuration ' nı (DSC) uzantısı kapsar. DSC uzantısı işleyicisine genel bakış için bkz. [Azure Desired State Configuration uzantısı işleyicisi giriş](dsc-overview.md).

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="pass-in-credentials"></a>Kimlik bilgilerini geçirin

Yapılandırma işleminin bir parçası olarak, gerektiğinde, kullanıcı hesaplarını ayarlamak için hizmetlere erişmek veya kullanıcı bağlamında bir program yüklemek. Bunları yapmak için kimlik bilgilerini sağlamanız gerekir.

DSC, parametreli yapılandırmalarını ayarlamak için kullanabilirsiniz. Parametreli bir yapılandırmada, kimlik bilgileri yapılandırmasını geçirilen ve güvenli bir şekilde .mof dosyalarında depolanır. Azure uzantısı işleyicisi, sertifikaların otomatik yönetimi sağlayarak kimlik bilgileri yönetimi basitleştirir.

Şu DSC yapılandırma betiği, belirtilen parolayla yerel bir kullanıcı hesabı oluşturur:

```powershell
configuration Main
{
    param(
        [Parameter(Mandatory=$true)]
        [ValidateNotNullorEmpty()]
        [PSCredential]
        $Credential
    )
    Node localhost {
        User LocalUserAccount
        {
            Username = $Credential.UserName
            Password = $Credential
            Disabled = $false
            Ensure = "Present"
            FullName = "Local User Account"
            Description = "Local User Account"
            PasswordNeverExpires = $true
        }
    }
}
```

Eklenmesi önemlidir **düğüm localhost** yapılandırmasının bir parçası olarak. Uzantı işleyici özellikle arar **düğüm localhost** deyimi. Aşağıdaki adımlar, bu deyimi yoksa, çalışmaz. Typecast eklenmesi önemlidir **[PsCredential]** . Bu belirli tür uzantısı kimlik bilgisi şifrelenemedi tetikler.

Bu betik, Azure Blob depolama alanına yayımlama için:

`Publish-AzVMDscConfiguration -ConfigurationPath .\user_configuration.ps1`

Azure DSC uzantısı ve kimlik bilgileri sağlamak için:

```powershell
$configurationName = 'Main'
$configurationArguments = @{ Credential = Get-Credential }
$configurationArchive = 'user_configuration.ps1.zip'
$vm = Get-AzVM -Name 'example-1'

$vm = Set-AzVMDscExtension -VMName $vm -ConfigurationArchive $configurationArchive -ConfigurationName $configurationName -ConfigurationArgument @configurationArguments

$vm | Update-AzVM
```

## <a name="how-a-credential-is-secured"></a>Bir kimlik bilgisi güvenliği nasıl sağlanır

Bu kodu çalıştırmak için kimlik bilgilerini ister. Kimlik bilgileri sağlandıktan sonra kısaca bellekte depolanır. Kimlik bilgisi, yayımlanan kullanarak **kümesi AzVMDscExtension** cmdlet'i, kimlik bilgisi, sanal makineye HTTPS üzerinden iletilir. VM, Azure yerel sanal makine sertifikası kullanarak diskte şifrelenmiş kimlik bilgileri depolar. Kimlik bilgisi kısaca bellekte şifresi çözülür ve ardından DSC için geçirmek için yeniden şifrelenir.

Bu işlem farklıdır [uzantı işleyici olmadan güvenli yapılandırmaları kullanarak](/powershell/dsc/securemof). Azure ortamı, yapılandırma verilerini sertifikaları aracılığıyla güvenli bir şekilde aktarmak için bir yol sunar. DSC uzantısı işleyicisine kullandığınızda sağlamanıza gerek kalmaması **$CertificatePath** veya **$CertificateID**/  **$Thumbprint** girişte**ConfigurationData**.

## <a name="next-steps"></a>Sonraki adımlar

- Alma bir [giriş Azure DSC uzantısı işleyicisine](dsc-overview.md).
- İnceleme [DSC uzantısı için Azure Resource Manager şablonu](dsc-template.md).
- PowerShell DSC hakkında daha fazla bilgi için Git [PowerShell Belge Merkezi](/powershell/dsc/overview).
- PowerShell DSC kullanarak yönetebilirsiniz. daha fazla işlevsellik ve daha fazla DSC kaynakları için Gözat [PowerShell Galerisi](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0).