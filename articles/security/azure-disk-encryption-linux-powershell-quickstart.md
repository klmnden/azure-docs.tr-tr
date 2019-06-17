---
title: Oluşturma ve Azure Powershell ile Linux VM şifreleme
description: Bu hızlı başlangıçta, Azure Powershell oluşturmak ve bir Linux sanal makinesini şifrelemek için kullanmayı öğrenin
author: msmbaldwin
ms.author: mbaldwin
ms.service: security
ms.topic: quickstart
ms.date: 05/17/2019
ms.openlocfilehash: 7c44ffefccd4d78713d29aa2f9ab6006b8ba819e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67081486"
---
# <a name="quickstart-create-and-encrypt-a-linux-virtual-machine-in-azure-with-powershell"></a>Hızlı Başlangıç: Oluşturma ve Azure PowerShell ile bir Linux sanal makinesi şifreleme

Azure PowerShell modülü, PowerShell komut satırından veya betik içinden Azure kaynakları oluşturmak ve yönetmek için kullanılır. Bu hızlı başlangıçta bir Linux sanal makinesi (VM) oluşturma, şifreleme anahtarları, depolama için bir anahtar kasası oluşturun ve VM şifreleme için Azure PowerShell modülünü kullanmayı gösterir. Bu hızlı başlangıçta Ubuntu 16.04 LTS Market görüntüsünden Canonical ve bir VM Standard_D2S_V3 boyutu kullanılır. 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Bir Azure kaynak grubu oluşturun [yeni AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır:

```powershell
New-AzResourceGroup -Name "myResourceGroup" -Location "EastUS"
```

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

Bir Azure sanal makinesi oluşturma [New-AzVM](/powershell/module/az.compute/new-azvm), VM yapılandırması iletmeden yukarıda oluşturulan nesne.

```powershell
$securePassword = ConvertTo-SecureString 'AZUREuserPA$$W0RD' -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential ("azureuser", $securePassword)

New-AzVM -Name MyVm -Credential $cred -ResourceGroupName MyResourceGroup -Image Canonical:UbuntuServer:16.04-LTS:latest  -Size Standard_D2S_V3
```

VM'nizin dağıtılması birkaç dakika sürer. 

## <a name="create-a-key-vault-configured-for-encryption-keys"></a>Şifreleme anahtarları için yapılandırılmış bir Key Vault oluşturma

Azure disk şifrelemesini bir Azure anahtar Kasası'nda şifreleme anahtarını depolar. Key Vault ile oluşturma [AzKeyvault yeni](/powershell/module/az.keyvault/new-azkeyvault). Şifreleme anahtarları depolamak için Key Vault etkinleştirmek için - EnabledForDiskEncryption parametresini kullanın.

> [!Important]
> Her Key Vault benzersiz bir ad olmalıdır. Aşağıdaki örnekte adlı bir anahtar kasası oluşturulmaktadır *myKV*, ancak sizinki farklı bir şey adlandırmalısınız.

```powershell
New-AzKeyvault -name MyKV -ResourceGroupName myResourceGroup -Location EastUS -EnabledForDiskEncryption
```

## <a name="encrypt-the-virtual-machine"></a>Sanal makinesi şifreleme

Şifreleme ile sanal makinenize [kümesi AzVmDiskEncryptionExtension](/powershell/module/az.compute/set-azvmdiskencryptionextension). 

Set-AzVmDiskEncryptionExtension bazı değerler, anahtar kasası nesnesinden gerektirir. Anahtar kasanızı benzersiz adını geçirerek bu değerleri edinebilirsiniz [Get-AzKeyvault](/powershell/module/az.keyvault/get-azkeyvault).

```powershell
$KeyVault = Get-AzKeyVault -VaultName MyKV -ResourceGroupName MyResourceGroup

Set-AzVMDiskEncryptionExtension -ResourceGroupName MyResourceGroup -VMName MyVM -DiskEncryptionKeyVaultUrl $KeyVault.VaultUri -DiskEncryptionKeyVaultId $KeyVault.ResourceId -SkipVmBackup -VolumeType All
```

Birkaç dakika sonra işlemi aşağıdaki döndürür:

```
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK
```

Şifreleme işlemi çalıştırarak doğrulayabilirsiniz [Get-AzVmDiskEncryptionStatus](/powershell/module/az.compute/Get-AzVMDiskEncryptionStatus).

```powershell
Get-AzVmDiskEncryptionStatus -VMName MyVM -ResourceGroupName MyResourceGroup
```

Şifreleme etkin olduğunda, döndürülen çıkış aşağıdaki görürsünüz:

```
OsVolumeEncrypted          : EncryptionInProgress
DataVolumesEncrypted       : NotMounted
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OS disk encryption started
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) cmdlet'ini kaynak grubunu, VM'yi ve tüm ilgili kaynakları kaldırmak için:

```powershell
Remove-AzResourceGroup -Name "myResourceGroup"
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir sanal makine etkinleştirmek için şifreleme anahtarlarını ve VM şifreli bir Key Vault oluşturdunuz, oluşturuldu.  IaaS VM'leri için Azure Disk Şifrelemesi önkoşulları hakkında daha fazla bilgi edinmek üzere bir sonraki makaleye geçin.

> [!div class="nextstepaction"]
> [Azure Disk şifrelemesi önkoşulları](azure-security-disk-encryption-prerequisites.md)