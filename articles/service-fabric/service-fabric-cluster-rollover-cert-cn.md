---
title: Azure Service Fabric kümesi sertifika aktarma | Microsoft Docs
description: Geçiş için Service Fabric kümesi sertifika sertifika ortak adı tarafından nasıl tanımlanan öğrenin.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: aljo
ms.assetid: 5441e7e0-d842-4398-b060-8c9d34b07c48
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/24/2018
ms.author: ryanwi
ms.openlocfilehash: df919e23fd608cdf41e93844f13342ca00657adb
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="manually-roll-over-a-service-fabric-cluster-certificate"></a>El ile bir Service Fabric kümesi sertifika alma
Service Fabric kümesi sertifikanın süresi dolmak üzere olduğunda sertifika güncelleştirmeniz gerekir.  Sertifika aktarma küme ise basit [ortak adını temel alarak sertifikaları kullan kadar ayarlanmış](service-fabric-cluster-change-cert-thumbprint-to-cn.md) (yerine parmak izi).  Yeni bir sona erme tarihi ile sertifika yetkilisinden yeni bir sertifika alın.  Azure portalında bir Service Fabric kümesi dağıtırken oluşturulan dahil olmak üzere otomatik olarak imzalanan sertifikalar desteklenmez.  Yeni sertifika daha eski sertifika ortak adıyla aynı olmalıdır. 

Aşağıdaki komut dosyasını yeni bir sertifika bir anahtar Kasası'na yükler ve ardından sanal makine ölçek kümesi üzerinde sertifikayı yükler.  Service Fabric kümesi, en son sona erme tarihi ile sertifika otomatik olarak kullanır.

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser -Force

$SubscriptionId  =  <subscription ID>

# Sign in to your Azure account and select your subscription
Login-AzureRmAccount -SubscriptionId $SubscriptionId

$region = "southcentralus"
$KeyVaultResourceGroupName  = "keyvaultgroup"
$VaultName = "cntestvault2"
$certFilename = "C:\users\sfuser\sftutorialcluster20180419110824.pfx"
$certname = "cntestcert"
$Password  = "!P@ssw0rd321"
$VmssResourceGroupName     = "sfclustertutorialgroup"
$VmssName                  = "prnninnxj"

# Create new Resource Group 
New-AzureRmResourceGroup -Name $KeyVaultResourceGroupName -Location $region

# Get the key vault.  The key vault must be enabled for deployment.
$keyVault = Get-AzureRmKeyVault -VaultName $VaultName -ResourceGroupName $KeyVaultResourceGroupName 
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

$certConfig = New-AzureRmVmssVaultCertificateConfig -CertificateUrl $CertificateURL -CertificateStore "My"

# Get current VM scale set 
$vmss = Get-AzureRmVmss -ResourceGroupName $VmssResourceGroupName -VMScaleSetName $VmssName

# Add new secret to the VM scale set.
$vmss = Add-AzureRmVmssSecret -VirtualMachineScaleSet $vmss -SourceVaultId $SourceVault -VaultCertificate $certConfig

# Update the VM scale set 
Update-AzureRmVmss -ResourceGroupName $VmssResourceGroupName -Name $VmssName -VirtualMachineScaleSet $vmss  -Verbose
```

Daha fazla bilgi için aşağıdaki okuyun:
* Hakkında bilgi edinin [küme güvenlik](service-fabric-cluster-security.md).
* [Güncelleştirme ve küme sertifikaları yönetme](service-fabric-cluster-security-update-certs-azure.md)

