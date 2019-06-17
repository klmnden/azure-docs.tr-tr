---
title: Bir Azure Service Fabric küme sertifikası aktarma | Microsoft Docs
description: Nasıl geçiş için bir Service Fabric küme sertifikası sertifika ortak adına göre tanımlanan öğrenin.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: aljo
ms.assetid: 5441e7e0-d842-4398-b060-8c9d34b07c48
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/24/2018
ms.author: aljo
ms.openlocfilehash: dd4b6026772a20c522532e1ba65c6846addfa161
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66159908"
---
# <a name="manually-roll-over-a-service-fabric-cluster-certificate"></a>El ile bir Service Fabric küme sertifikası alma
Bir Service Fabric küme sertifikasının süresi dolmak üzere olduğunda sertifika güncelleştirmeniz gerekiyor.  Küme ise, Basit sertifika geçişi [ortak adını temel alarak sertifikaları kullan kadar ayarlanmış](service-fabric-cluster-change-cert-thumbprint-to-cn.md) (yerine parmak izi).  Yeni bir sona erme tarihi ile sertifika yetkilisinden yeni bir sertifika alın.  Otomatik olarak imzalanan sertifikaları Azure portalı küme oluşturma iş akışı sırasında oluşturulan sertifikalar dahil etmek için üretim Service Fabric kümeleri için destek değildir. Yeni sertifika, ortak eski sertifikanın adıyla aynı olmalıdır. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Service Fabric kümesini otomatik olarak kullanacağınız bildirilen sertifika ile daha fazla gelecekteki sona erme tarihi; içine birden fazla doğruladığınızda sertifika konakta yüklenir. Azure kaynakları sağlamak için Resource Manager şablonu kullanmak iyi bir uygulamadır. Üretim dışı ortam için aşağıdaki betiği bir anahtar Kasası'na yeni bir sertifika yüklemek için kullanılabilir ve ardından sanal makine ölçek kümesinde sertifikayı yükler: 

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser -Force

$SubscriptionId  =  <subscription ID>

# Sign in to your Azure account and select your subscription
Login-AzAccount -SubscriptionId $SubscriptionId

$region = "southcentralus"
$KeyVaultResourceGroupName  = "keyvaultgroup"
$VaultName = "cntestvault2"
$certFilename = "C:\users\sfuser\sftutorialcluster20180419110824.pfx"
$certname = "cntestcert"
$Password  = "!P@ssw0rd321"
$VmssResourceGroupName     = "sfclustertutorialgroup"
$VmssName                  = "prnninnxj"

# Create new Resource Group 
New-AzResourceGroup -Name $KeyVaultResourceGroupName -Location $region

# Get the key vault.  The key vault must be enabled for deployment.
$keyVault = Get-AzKeyVault -VaultName $VaultName -ResourceGroupName $KeyVaultResourceGroupName 
$resourceId = $keyVault.ResourceId  

# Add the certificate to the key vault.
$PasswordSec = ConvertTo-SecureString -String $Password -AsPlainText -Force
$KVSecret = Import-AzureKeyVaultCertificate -VaultName $vaultName -Name $certName  -FilePath $certFilename -Password $PasswordSec

$CertificateThumbprint = $KVSecret.Thumbprint
$CertificateURL = $KVSecret.SecretId
$SourceVault = $resourceId
$CommName    = $KVSecret.Certificate.SubjectName.Name

Write-Host "CertificateThumbprint    :"  $CertificateThumbprint
Write-Host "CertificateURL           :"  $CertificateURL
Write-Host "SourceVault              :"  $SourceVault
Write-Host "Common Name              :"  $CommName    

Set-StrictMode -Version 3
$ErrorActionPreference = "Stop"

$certConfig = New-AzVmssVaultCertificateConfig -CertificateUrl $CertificateURL -CertificateStore "My"

# Get current VM scale set 
$vmss = Get-AzVmss -ResourceGroupName $VmssResourceGroupName -VMScaleSetName $VmssName

# Add new secret to the VM scale set.
$vmss = Add-AzVmssSecret -VirtualMachineScaleSet $vmss -SourceVaultId $SourceVault -VaultCertificate $certConfig

# Update the VM scale set 
Update-AzVmss -ResourceGroupName $VmssResourceGroupName -Name $VmssName -VirtualMachineScaleSet $vmss  -Verbose
```

>[!NOTE]
> Her bir gizli dizi tutulan benzersiz bir kaynak olarak sanal makine ölçek kümesi gizli iki ayrı gizli dizileri için aynı kaynak kimliği desteği olmayan hesaplar. 

Daha fazla bilgi için aşağıdakileri okuyun:
* Hakkında bilgi edinin [küme güvenlik](service-fabric-cluster-security.md).
* [Küme sertifikalarını yönetme ve güncelleştirme](service-fabric-cluster-security-update-certs-azure.md)

