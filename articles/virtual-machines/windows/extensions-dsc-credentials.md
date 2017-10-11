---
title: "DSC kullanarak Azure için kimlik bilgilerini geçirme | Microsoft Docs"
description: "PowerShell istenen durum yapılandırması kullanarak Azure sanal makineleri için kimlik bilgileri güvenli bir şekilde geçirme hakkında genel bakış"
services: virtual-machines-windows
documentationcenter: 
author: zjalexander
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
keywords: 
ms.assetid: ea76b7e8-b576-445a-8107-88ea2f3876b9
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 09/15/2016
ms.author: zachal
ms.openlocfilehash: acd768c0219ec23c0453a65c575faf5213d9c616
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="passing-credentials-to-the-azure-dsc-extension-handler"></a>Kimlik bilgileri Azure DSC uzantısı işleyicisine geçirme
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Bu makalede, Azure için istenen durum yapılandırması uzantısı yer almaktadır. DSC uzantı işleyicisi genel bir bakış bulunabilir [Azure istenen durum yapılandırması uzantısı işleyici giriş](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

## <a name="passing-in-credentials"></a>Kimlik bilgilerinde
Yapılandırma işleminin bir parçası olarak gerektiğinde kullanıcı hesaplarını, ayarlamak için hizmetlere erişmek veya kullanıcı bağlamında bir program yüklemek. Bu işlemler yapmak için kimlik bilgilerini sağlamanız gerekir. 

DSC kimlik bilgileri, yapılandırmaya geçirilen ve güvenli bir şekilde MOF dosyasında depolanan parametreli yapılandırmalarını sağlar. Azure uzantısı işleyici sertifikaların otomatik yönetimi sağlayarak kimlik bilgileri yönetimi basitleştirir. 

Belirtilen parola ile yerel bir kullanıcı hesabı oluşturur aşağıdaki DSC yapılandırma betiği göz önünde bulundurun:

*user_configuration.ps1*

```
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

Eklenmesi önemlidir *düğümü localhost* yapılandırmasının bir parçası olarak. Bu bildirimi yoksa, uzantı işleyici düğümü localhost deyim için özel olarak göründüğü gibi aşağıdaki adımları çalışmaz. Typecast eklemek önemlidir *[PsCredential]*gibi belirli bu tür kimlik bilgilerini şifrelemek için uzantı tetikler. 

Bu komut dosyası blob depolama alanına yayımlayın:

`Publish-AzureVMDscConfiguration -ConfigurationPath .\user_configuration.ps1`

Azure DSC uzantısı ayarlayabilir ve kimlik bilgilerini girin:

```
$configurationName = "Main"
$configurationArguments = @{ Credential = Get-Credential }
$configurationArchive = "user_configuration.ps1.zip"
$vm = Get-AzureVM "example-1"

$vm = Set-AzureVMDSCExtension -VM $vm -ConfigurationArchive $configurationArchive 
-ConfigurationName $configurationName -ConfigurationArgument @configurationArguments

$vm | Update-AzureVM
```
## <a name="how-credentials-are-secured"></a>Kimlik bilgileri güvenliği nasıl
Bu kodu çalıştırmak için kimlik bilgilerini ister. Sağlanan sonra onu kısaca bellekte depolanır. Ne zaman onu yayımlanır ile `Set-AzureVmDscExtension` cmdlet, bu VM için HTTPS üzerinden iletilen, Azure burada depolar yerel VM sertifika kullanarak diskte şifrelenmiş. Ardından bu kısaca bellekte şifresi çözülür ve DSC için geçirmek için yeniden şifrelenir.

Bu davranış farklıdır [uzantısı işleyici olmadan güvenli yapılandırmaları kullanarak](https://msdn.microsoft.com/powershell/dsc/securemof). Azure ortamı yapılandırma verilerinin sertifikalar yoluyla güvenli bir şekilde iletmek için bir yöntem sunar. DSC uzantı işleyici kullanırken, $CertificatePath veya bir $CertificateID sağlamak için gerek yoktur / ConfigurationData $Thumbprint girişi.

## <a name="next-steps"></a>Sonraki adımlar
Azure DSC uzantısı işleyici hakkında daha fazla bilgi için bkz: [Azure istenen durum yapılandırması uzantısı işleyici giriş](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

İncelemek [DSC uzantısı için Azure Resource Manager şablonu](extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

PowerShell DSC hakkında daha fazla bilgi için [PowerShell Belge Merkezi ziyaret](https://msdn.microsoft.com/powershell/dsc/overview). 

PowerShell DSC ile yönetebileceğiniz ek işlevsellik bulmak için [PowerShell Galerisi Gözat](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) daha fazla DSC kaynakları için.

