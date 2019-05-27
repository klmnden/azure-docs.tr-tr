---
title: Windows Iaas Vm'leri için Azure Disk şifrelemesini etkinleştirme
description: Bu makalede, Microsoft Azure Disk şifrelemesi için Windows Iaas Vm'leri etkinleştirme hakkında yönergeler sağlar.
author: msmbaldwin
ms.service: security
ms.topic: article
ms.author: mbaldwin
ms.date: 03/12/2019
ms.custom: seodec18
ms.openlocfilehash: 376df206d75780a4b814873d72d9c56554f6b0b8
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65956605"
---
# <a name="enable-azure-disk-encryption-for-windows-iaas-vms"></a>Windows Iaas Vm'leri için Azure Disk şifrelemesini etkinleştirme

Bu makalede, Microsoft Azure Disk şifrelemesi için Windows Iaas sanal makineleri (VM'ler) etkinleştirme hakkında yönergeler sağlar. Disk şifrelemesi kullanabilmeniz için önce tamamlamalısınız [Azure Disk şifrelemesi önkoşulları](../security/azure-security-disk-encryption-prerequisites.md). 

Ayrıca önemle tavsiye edilir, [anlık görüntü oluşturma](../virtual-machines/windows/snapshot-copy-managed-disk.md) ve/disklerinizi önce şifreleme yedekleme veya. Yedekleme kurtarma seçeneğini şifreleme sırasında beklenmeyen bir hata oluşması durumunda olası olduğundan emin olun. Şifreleme gerçekleşmeden önce yönetilen disklere sahip VM'ler yedeklemesini gerektirir. Bir yedekleme yapıldıktan sonra kullanabileceğiniz [kümesi AzVMDiskEncryptionExtension cmdlet'i](/powershell/module/az.compute/set-azvmdiskencryptionextension) - skipVmBackup parametresini belirterek yönetilen disklerini şifrelemek için. Yedekleme ve şifrelenmiş Vm'leri geri yükleme hakkında daha fazla bilgi için bkz. [şifrelenmiş Azure VM geri](../backup/backup-azure-vms-encryption.md) makalesi.

>[!WARNING]
> - Daha önce kullandıysanız [Azure AD uygulaması ile Azure Disk şifrelemesi](azure-security-disk-encryption-prerequisites-aad.md) bu sanal Makineyi şifrelemek için devam gerekecektir VM'nizi şifrelemek için bu seçeneği kullanın. Kullanamazsınız [Azure Disk şifrelemesi](azure-security-disk-encryption-prerequisites.md) bu desteklenen bir senaryo değildir gibi şifrelenmiş bu VM üzerinde bu VM şifreli için AAD uygulaması uzağa anlamı geçiş henüz desteklenmiyor. 
> - Azure Disk şifrelemesi, anahtar kasası ve VM'lerin aynı bölgede bulunması gerekir. Oluşturun ve şifrelenmiş VM ile aynı bölgede olan bir anahtar Kasası'nı kullanın. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="bkmk_RunningWinVM"></a> Var olan veya Iaas Windows Vm'leri çalıştırma şifrelemeyi etkinleştir
Şablon, PowerShell cmdlet'leri veya CLI komutları kullanarak şifreleyebilirsiniz. Sanal makine uzantısı için şema bilgileri gerekiyorsa bkz [Windows için Azure Disk şifrelemesi uzantı](../virtual-machines/extensions/azure-disk-enc-windows.md) makalesi.

>[!IMPORTANT]
 > Anlık görüntü için zorunludur ve/veya yedekleme yönetilen disk sanal makine örneğinin dışında ve Azure Disk şifrelemesi etkinleştirilmeden önce temel. Yönetilen diskin anlık görüntüsünü portaldan alınabilir veya [Azure Backup](../backup/backup-azure-vms-encryption.md) kullanılabilir. Yedekleme kurtarma seçeneğini şifreleme sırasında beklenmeyen bir hata olması durumunda olası olduğundan emin olun. Bir yedekleme yapıldıktan sonra Set-AzVMDiskEncryptionExtension cmdlet - skipVmBackup parametresini belirterek yönetilen disklerini şifrelemek için kullanılabilir. Set-AzVMDiskEncryptionExtension komutu, karşı temel alan bir yönetilen disk Vm'leri yedekleme yapılan ve bu parametre belirtilen kadar başarısız olacak. 
>
> Şifrelenirken veya şifreleme devre dışı bırakıldığında VM yeniden başlatılmasına neden olabilir. 
>

### <a name="bkmk_RunningWinVMPSH"></a> Var olan veya Azure PowerShell ile sanal makine çalıştırma şifrelemeyi etkinleştir 
Kullanım [kümesi AzVMDiskEncryptionExtension](/powershell/module/az.compute/set-azvmdiskencryptionextension) azure'da çalıştırılan Iaas sanal makine şifrelemesini etkinleştirmek için cmdlet'i. 

-  **Çalışan bir VM şifrele:** Aşağıdaki komut dosyası değişkenleri başlatır ve Set-AzVMDiskEncryptionExtension cmdlet'ini çalıştırır. Kaynak grubunu, VM'yi ve anahtar kasası zaten önkoşul olarak oluşturulmuş olmalıdır. MyKeyVaultResourceGroup, MyVirtualMachineResourceGroup MySecureVM ve MySecureVault değerleriniz ile değiştirin.

     ```azurepowershell
      $KVRGname = 'MyKeyVaultResourceGroup';
      $VMRGName = 'MyVirtualMachineResourceGroup';
      $vmName = 'MySecureVM';
      $KeyVaultName = 'MySecureVault';
      $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
      $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;

      Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGname -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId;
    ```
- **KEK kullanarak çalışan bir VM şifrele:** 

     ```azurepowershell
     $KVRGname = 'MyKeyVaultResourceGroup';
     $VMRGName = 'MyVirtualMachineResourceGroup';
     $vmName = 'MyExtraSecureVM';
     $KeyVaultName = 'MySecureVault';
     $keyEncryptionKeyName = 'MyKeyEncryptionKey';
     $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
     $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     $keyEncryptionKeyUrl = (Get-AzKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;

     Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGname -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId;

     ```
     
   >[!NOTE]
   > Disk şifreleme keyvault parametresinin değeri söz diziminin tam tanımlayıcı bir dizedir: / subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> Anahtar şifreleme anahtarı parametresinin değeri sözdizimi KEK olarak tam URI şudur: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

- **Diskler şifrelenir doğrulayın:** Iaas VM şifreleme durumunu denetlemek için kullanmak [Get-AzVmDiskEncryptionStatus](/powershell/module/az.compute/get-azvmdiskencryptionstatus) cmdlet'i. 
     ```azurepowershell-interactive
     Get-AzVmDiskEncryptionStatus -ResourceGroupName 'MyVirtualMachineResourceGroup' -VMName 'MySecureVM'
     ```
    
- **Disk şifreleme devre dışı bırakın:** Şifreleme devre dışı bırakmak için [devre dışı bırak AzVMDiskEncryption](/powershell/module/az.compute/disable-azvmdiskencryption) cmdlet'i. Hem işletim sistemi ve veri diskleri şifrelediğinizde Windows VM'de disk şifrelemeyi devre dışı bırakma, beklendiği gibi çalışmaz. Bunun yerine tüm diskleri şifreleme devre dışı bırakın.

     ```azurepowershell-interactive
     Disable-AzVMDiskEncryption -ResourceGroupName 'MyVirtualMachineResourceGroup' -VMName 'MySecureVM'
     ```

### <a name="bkmk_RunningWinVMCLI"></a>Var olan veya Azure CLI ile sanal makine çalıştırma şifrelemeyi etkinleştir
Kullanım [az vm şifrelemeyi etkinleştirme](/cli/azure/vm/encryption#az-vm-encryption-enable) azure'da çalıştırılan Iaas sanal makine üzerinde şifrelemeyi etkinleştirmek için komutu.

- **Çalışan bir VM şifrele:**

    ```azurecli-interactive
    az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --disk-encryption-keyvault "MySecureVault" --volume-type [All|OS|Data]
    ```

- **KEK kullanarak çalışan bir VM şifrele:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type [All|OS|Data]
     ```

     >[!NOTE]
     > Disk şifreleme keyvault parametresinin değeri söz diziminin tam tanımlayıcı bir dizedir: / subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name] </br> Anahtar şifreleme anahtarı parametresinin değeri sözdizimi KEK olarak tam URI şudur: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

- **Diskler şifrelenir doğrulayın:** Iaas VM şifreleme durumunu denetlemek için kullanmak [az vm şifreleme show](/cli/azure/vm/encryption#az-vm-encryption-show) komutu. 

     ```azurecli-interactive
     az vm encryption show --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup"
     ```

- **Şifreleme devre dışı bırakın:** Şifreleme devre dışı bırakmak için [az vm şifreleme devre dışı](/cli/azure/vm/encryption#az-vm-encryption-disable) komutu. Hem işletim sistemi ve veri diskleri şifrelediğinizde Windows VM'de disk şifrelemeyi devre dışı bırakma, beklendiği gibi çalışmaz. Bunun yerine tüm diskleri şifreleme devre dışı bırakın.

     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup" --volume-type [ALL, DATA, OS]
     ```
 
  > [!NOTE]
  >Anlık görüntü için zorunludur ve/veya yedekleme yönetilen disk sanal makine örneğinin dışında ve Azure Disk şifrelemesi etkinleştirilmeden önce temel. Yönetilen diskin anlık görüntüsünü portaldan alınabilir veya [Azure Backup](../backup/backup-azure-vms-encryption.md) kullanılabilir. Yedekleme kurtarma seçeneğini şifreleme sırasında beklenmeyen bir hata olması durumunda olası olduğundan emin olun. Bir yedekleme yapıldıktan sonra Set-AzVMDiskEncryptionExtension cmdlet - skipVmBackup parametresini belirterek yönetilen disklerini şifrelemek için kullanılabilir. Bu komut, yönetilen disk tabanlı Vm'leri karşı yapılan bir yedekleme ve bu parametre belirtildi kadar başarısız olacak. 
  >
  >Şifrelenirken veya şifreleme devre dışı bırakıldığında VM yeniden başlatılmasına neden olabilir. 

### <a name="bkmk_RunningWinVMwRM"> </a>Resource Manager şablonu kullanma

Disk şifrelemesi kullanarak Azure'da Iaas Windows Vm'leri çalıştırma ya da varolan etkinleştirebilirsiniz [çalışan bir Windows VM şifreleme için Resource Manager şablonunu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm-without-aad).


1. Azure Hızlı Başlangıç şablonu üzerinde **azure'a Dağıt**.

2. Abonelik, kaynak grubu, konum, ayarları, yasal koşulları ve Sözleşmesi'ni seçin. Tıklayın **satın alma** var olan veya çalışan Iaas VM üzerinde şifrelemeyi etkinleştirmek için.

Mevcut veya sanal makineleri çalıştırmak için Resource Manager şablon parametreleri aşağıdaki tabloda listelenmektedir:

| Parametre | Açıklama |
| --- | --- |
| vmName | Şifreleme işlemi çalıştırmak için sanal makinenin adı. |
| keyVaultName | BitLocker anahtarı karşıya yüklenmelidir anahtar kasasının adı. Cmdlet'ini kullanarak elde edeceğinizi `(Get-AzKeyVault -ResourceGroupName <MyKeyVaultResourceGroupName>). Vaultname` veya Azure CLI komutu `az keyvault list --resource-group "MyKeyVaultResourceGroup"`|
| keyVaultResourceGroup | Anahtar kasasını içeren kaynak grubunun adı|
|  KeyEncryptionKeyURL | Oluşturulan BitLocker anahtarı şifrelemek için kullanılan anahtar şifreleme anahtarı URL'si. Bu seçerseniz isteğe bağlı bir parametredir **nokek** UseExistingKek aşağı açılan listesinde. Seçerseniz **kek** UseExistingKek aşağı açılan listeden girmelisiniz _keyEncryptionKeyURL_ değeri. |
| VolumeType | Şifreleme işlemi gerçekleştirilir birim türü. Geçerli değerler _işletim sistemi_, _veri_, ve _tüm_. 
| forceUpdateTag | İşlemi çalıştırmak olması zorlamak gereken her zaman bir GUID gibi benzersiz bir değer geçirin. |
| resizeOSDisk | Sistem birimi bölme önce tam işletim sistemi VHD'si kaplayacak şekilde işletim sistemi bölümü yeniden boyutlandırılması. |
| location | Tüm kaynaklar için konum. |

## <a name="encrypt-virtual-machine-scale-sets"></a>Sanal makine ölçek kümeleri şifrele

[Azure sanal makine ölçek kümeleri](../virtual-machine-scale-sets/overview.md) oluşturma ve yönetme bir grup özdeş, yük dengeli sanal makineler sağlar. Tanımlı bir zamanlamaya veya talebe yanıt olarak sanal makine örneği sayısı otomatik olarak artabilir ya da azalabilir. Sanal makine ölçek kümeleri şifrelemek için CLI veya Azure PowerShell kullanın.

### <a name="encrypt-virtual-machine-scale-sets-with-azure-powershell"></a>Azure PowerShell ile sanal makine ölçek kümeleri şifrele

Kullanım [kümesi AzVmssDiskEncryptionExtension](/powershell/module/az.compute/set-azvmssdiskencryptionextension) bir Windows sanal makine ölçek kümesi şifrelemesini etkinleştirmek için cmdlet'i. Kaynak grubu, sanal makine ölçek kümesi ve anahtar kasası zaten önkoşul olarak oluşturulmuş olmalıdır.

-  **Çalışan bir sanal makine ölçek kümesi şifrelemek**:
    ```azurepowershell
     $KVRGname = 'MyKeyVaultResourceGroup';
     $VMSSRGname = 'MyVMScaleSetResourceGroup';
     $VmssName = "MySecureVmss";
     $KeyVaultName= "MySecureVault";
     $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
     $DiskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     Set-AzVmssDiskEncryptionExtension -ResourceGroupName $VMSSRGname -VMScaleSetName $VmssName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId;


-  **Encrypt a running virtual machine scale set using KEK to wrap the key**:

    ```azurepowershell
     $KVRGname = 'MyKeyVaultResourceGroup';
     $VMSSRGname = 'MyVMScaleSetResourceGroup';
     $VmssName = "MySecureVmss";
     $KeyVaultName= "MySecureVault";
     $keyEncryptionKeyName = "MyKeyEncryptionKey";
     $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
     $DiskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     $KeyEncryptionKeyUrl = (Get-AzKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;
     Set-AzRmVmssDiskEncryptionExtension -ResourceGroupName $VMSSRGname -VMScaleSetName $VmssName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $KeyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId;
    ```

   >[!NOTE]
   > Disk şifreleme keyvault parametresinin değeri söz diziminin tam tanımlayıcı bir dizedir: / subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> Anahtar şifreleme anahtarı parametresinin değeri sözdizimi KEK olarak tam URI şudur: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

- **Bir sanal makine ölçek kümesi için şifreleme durumunu alın:** Kullanım [Get-AzVmssDiskEncryption](/powershell/module/az.compute/get-azvmssdiskencryption) cmdlet'i.
    
    ```azurepowershell-interactive
    get-AzVmssVMDiskEncryption -ResourceGroupName "MyVMScaleSetResourceGroup" -VMScaleSetName "MySecureVmss"
    ```

- **Bir sanal makine ölçek kümesi üzerinde şifrelemeyi devre dışı**: Kullanım [devre dışı bırak AzVmssDiskEncryption](/powershell/module/az.compute/disable-azvmssdiskencryption) cmdlet'i. 

    ```azurepowershell-interactive
    Disable-AzVmssDiskEncryption -ResourceGroupName "MyVMScaleSetResourceGroup" -VMScaleSetName "MySecureVmss"
    ```

### <a name="encrypt-virtual-machine-scale-sets-with-azure-cli"></a>Azure CLI ile sanal makine ölçek kümeleri şifrele

Kullanım [az vmss şifrelemeyi etkinleştirme](/cli/azure/vmss/encryption#az-vmss-encryption-enable) bir Windows sanal makine ölçek kümesi şifrelemesini etkinleştirmek için. Yükseltme İlkesi el ile ölçek kümesi, şifreleme ile Başlat [az vmss update-instances](/cli/azure/vmss#az-vmss-update-instances). Kaynak grubu, sanal makine ölçek kümesi ve anahtar kasası zaten önkoşul olarak oluşturulmuş olmalıdır.

-  **Çalışan bir sanal makine ölçek kümesi şifrele**
    ```azurecli-interactive
     az vmss encryption enable --resource-group "MyVMScaleSetResourceGroup" --name "MySecureVmss" --disk-encryption-keyvault "MySecureVault" 
    ```

-  **Şifreleme anahtarı sarmalama için KEK kullanarak çalışan bir sanal makine ölçek**
    ```azurecli-interactive
     az vmss encryption enable --resource-group "MyVMScaleSetResourceGroup" --name "MySecureVmss" --disk-encryption-keyvault "MySecureVault" --key-encryption-key "MyKEK" --key-encryption-keyvault "MySecureVault" 

     ```
     
   >[!NOTE]
   > Disk şifreleme keyvault parametresinin değeri söz diziminin tam tanımlayıcı bir dizedir: / subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> Anahtar şifreleme anahtarı parametresinin değeri sözdizimi KEK olarak tam URI şudur: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

- **Bir sanal makine ölçek kümesi için şifreleme durumunu alın:** Kullanım [az vmss şifreleme Göster](/cli/azure/vmss/encryption#az-vmss-encryption-show)

    ```azurecli-interactive
     az vmss encryption show --resource-group "MyVMScaleSetResourceGroup" --name "MySecureVmss"
    ```

- **Bir sanal makine ölçek kümesi üzerinde şifrelemeyi devre dışı**: Kullanım [az vmss şifrelemeyi devre dışı bırakma](/cli/azure/vmss/encryption#az-vmss-encryption-disable)
    ```azurecli-interactive
     az vmss encryption disable --resource-group "MyVMScaleSetResourceGroup" --name "MySecureVmss"
    ```

### <a name="azure-resource-manager-templates-for-windows-virtual-machine-scale-sets"></a>Azure Resource Manager Şablonları Windows sanal makine ölçek için ayarlar

Şifrelemek veya Windows sanal makine ölçek şifresini çözmek için ayarlar, Azure Resource Manager şablonları ve aşağıdaki yönergeleri kullanın:

- [Bir Windows sanal makine ölçek kümesi şifrelemeyi etkinleştirin](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-vmss-windows)
- [Bir Windows sanal makine ölçek kümesi üzerinde şifrelemeyi devre dışı bırak](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-vmss-windows)

     1. **Azure’a dağıt**’a tıklayın.
     2. Gerekli alanları doldurun, sonra hüküm ve koşulları kabul etmiş olursunuz.
     3. Tıklayın **satın alma** kullanarak şablonu dağıtabilirsiniz.

## <a name="bkmk_VHDpre"> </a> VHD ve şifreleme anahtarları müşteri şifrelenmiş oluşturulan yeni Iaas VM'ler

Bu senaryoda, PowerShell cmdlet'lerini veya CLI komutları kullanarak şifreleme etkinleştirebilirsiniz. 

Yönergeleri ekte, Azure'da kullanılabilen önceden şifrelenmiş görüntüleri hazırlama için kullanın. Görüntü oluşturulduktan sonra şifrelenmiş bir Azure VM oluşturmak için sonraki bölümde adımları kullanabilirsiniz.

* [Önceden şifrelenmiş bir Windows VHD hazırlama](azure-security-disk-encryption-appendix.md#bkmk_preWin)
* [Önceden şifrelenmiş bir Linux VHD hazırlama](azure-security-disk-encryption-appendix.md#bkmk_preLinux)

>[!IMPORTANT]
 >Anlık görüntü için zorunludur ve/veya yedekleme yönetilen disk sanal makine örneğinin dışında ve Azure Disk şifrelemesi etkinleştirilmeden önce temel. Yönetilen diskin anlık görüntüsünü portaldan alınabilir veya [Azure Backup](../backup/backup-azure-vms-encryption.md) kullanılabilir. Yedekleme kurtarma seçeneğini şifreleme sırasında beklenmeyen bir hata olması durumunda olası olduğundan emin olun. Bir yedekleme yapıldıktan sonra Set-AzVMDiskEncryptionExtension cmdlet - skipVmBackup parametresini belirterek yönetilen disklerini şifrelemek için kullanılabilir. Set-AzVMDiskEncryptionExtension komutu, karşı temel alan bir yönetilen disk Vm'leri yedekleme yapılan ve bu parametre belirtilen kadar başarısız olacak. 
>
>Şifrelenirken veya şifreleme devre dışı bırakıldığında VM yeniden başlatılmasına neden olabilir. 


### <a name="bkmk_VHDprePSH"> </a> Azure PowerShell ile önceden şifrelenmiş VHD'ler ile VM'lerin şifrele
Disk şifreli VHD'nizi PowerShell cmdlet'ini kullanarak şifreleyebilirsiniz [kümesi AzVMOSDisk](/powershell/module/az.compute/set-azvmosdisk#examples). Aşağıdaki örnek, bazı ortak parametreler sunar. 

```azurepowershell
$VirtualMachine = New-AzVMConfig -VMName "MySecureVM" -VMSize "Standard_A1"
$VirtualMachine = Set-AzVMOSDisk -VM $VirtualMachine -Name "SecureOSDisk" -VhdUri "os.vhd" Caching ReadWrite -Windows -CreateOption "Attach" -DiskEncryptionKeyUrl "https://mytestvault.vault.azure.net/secrets/Test1/514ceb769c984379a7e0230bddaaaaaa" -DiskEncryptionKeyVaultId "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myKVresourcegroup/providers/Microsoft.KeyVault/vaults/mytestvault"
New-AzVM -VM $VirtualMachine -ResourceGroupName "MyVirtualMachineResourceGroup"
```

## <a name="enable-encryption-on-a-newly-added-data-disk"></a>Yeni eklenen verileri disk şifrelemeyi etkinleştirin
Yapabilecekleriniz [Windows PowerShell kullanarak bir VM için yeni bir disk ekleyin](../virtual-machines/windows/attach-disk-ps.md), veya [Azure portalı üzerinden](../virtual-machines/windows/attach-managed-disk-portal.md). 

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-powershell"></a>Azure PowerShell ile yeni eklenen bir disk şifrelemeyi etkinleştirin
 Windows VM'ler için yeni bir disk şifrelemek için PowerShell kullanırken, yeni bir dizisi sürümü belirtilmelidir. Dizisi sürümü benzersiz olması gerekir. Aşağıdaki komut dizisi sürümü için bir GUID oluşturur. Bazı durumlarda yeni eklenen veri diski tarafından Azure Disk şifrelemesi uzantısını otomatik olarak şifrelenebilir. Otomatik şifreleme, genellikle yeni diski çevrimiçi olduktan sonra VM yeniden başlatıldığında gerçekleşir. Bu, genellikle "All" belirtilmiş olduğundan birim türünü disk şifrelemesi daha önce VM üzerinde çalıştırdığınızda kaynaklanır. Yeni eklenen verileri disk üzerinde otomatik şifreleme ortaya çıkarsa, Set-AzVmDiskEncryptionExtension cmdlet'i yeni dizisi sürümü ile yeniden çalıştırmanızı öneririz. Varsa, yeni bir veri diski şifreli otomatik ve şifrelenmiş istiyor musunuz tüm sürücüleri ilk şifresini sonra işletim sistemi için birim türünü belirterek yeni bir dizisi sürümü ile yeniden şifrelenemedi. 
  
 

-  **Çalışan bir VM şifrele:** Aşağıdaki komut dosyası değişkenleri başlatır ve Set-AzVMDiskEncryptionExtension cmdlet'ini çalıştırır. Kaynak grubunu, VM'yi ve anahtar kasası zaten önkoşul olarak oluşturulmuş olmalıdır. MyKeyVaultResourceGroup, MyVirtualMachineResourceGroup MySecureVM ve MySecureVault değerleriniz ile değiştirin. Bu örnekte, "All" hem işletim sistemi ve veri birimlerini içeren - VolumeType parametresi için kullanılır. Yalnızca işletim sistemi birimi şifrelemek istiyorsanız, - VolumeType parametresi için "OS" kullanın. 

     ```azurepowershell
      $KVRGname = 'MyKeyVaultResourceGroup';
      $VMRGName = 'MyVirtualMachineResourceGroup';
      $vmName = 'MySecureVM';
      $KeyVaultName = 'MySecureVault';
      $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
      $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;
      $sequenceVersion = [Guid]::NewGuid();

      Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGname -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -VolumeType "All" –SequenceVersion $sequenceVersion;
    ```
- **KEK kullanarak çalışan bir VM şifrele:** Bu örnekte, "All" hem işletim sistemi ve veri birimlerini içeren - VolumeType parametresi için kullanılır. Yalnızca işletim sistemi birimi şifrelemek istiyorsanız, - VolumeType parametresi için "OS" kullanın.

     ```azurepowershell
     $KVRGname = 'MyKeyVaultResourceGroup';
     $VMRGName = 'MyVirtualMachineResourceGroup';
     $vmName = 'MyExtraSecureVM';
     $KeyVaultName = 'MySecureVault';
     $keyEncryptionKeyName = 'MyKeyEncryptionKey';
     $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
     $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     $keyEncryptionKeyUrl = (Get-AzKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;
     $sequenceVersion = [Guid]::NewGuid();

     Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGname -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId -VolumeType "All" –SequenceVersion $sequenceVersion;

     ```

    >[!NOTE]
    > Disk şifreleme keyvault parametresinin değeri söz diziminin tam tanımlayıcı bir dizedir: / subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> Anahtar şifreleme anahtarı parametresinin değeri sözdizimi KEK olarak tam URI şudur: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-cli"></a>Azure CLI ile yeni eklenen bir disk şifrelemeyi etkinleştirin
 Şifrelemeyi etkinleştirmek için komutu çalıştırdığınızda Azure CLI komutunu otomatik olarak sizin için yeni bir sıra sürüm sağlayacaktır. Örnekte, "All" birim türü parametresi için kullanılır. Birim türü parametresi yalnızca işletim sistemi disk şifreleme işletim sistemine değiştirmeniz gerekebilir. PowerShell sözdizimi aksine, şifreleme etkinleştirilirken benzersiz dizisi sürümü sağlamak kullanıcı CLI gerektirmez. CLI'yı otomatik olarak oluşturur ve kendi benzersiz dizisi sürümü değeri kullanır.   

-  **Çalışan bir VM şifrele:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --disk-encryption-keyvault "MySecureVault" --volume-type "All"
     ```

- **KEK kullanarak çalışan bir VM şifrele:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type "All"
     ```


## <a name="disable-encryption"></a>Şifrelemeyi devre dışı bırakma
Azure PowerShell, Azure CLI kullanarak şifreleme devre dışı bırakabilir veya bir Resource Manager şablonu ile. Hem işletim sistemi ve veri diskleri şifrelediğinizde Windows VM'de disk şifrelemeyi devre dışı bırakma, beklendiği gibi çalışmaz. Bunun yerine tüm diskleri şifreleme devre dışı bırakın.

- **Azure PowerShell ile disk şifrelemesini devre dışı bırakın:** Şifreleme devre dışı bırakmak için [devre dışı bırak AzVMDiskEncryption](/powershell/module/az.compute/disable-azvmdiskencryption) cmdlet'i. 
     ```azurepowershell-interactive
     Disable-AzVMDiskEncryption -ResourceGroupName 'MyVirtualMachineResourceGroup' -VMName 'MySecureVM' -VolumeType "all"
     ```

- **Azure CLI ile şifreleme devre dışı bırakın:** Şifreleme devre dışı bırakmak için [az vm şifreleme devre dışı](/cli/azure/vm/encryption#az-vm-encryption-disable) komutu. 
     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup" --volume-type "all"
     ```
- **Resource Manager şablonu ile şifreleme devre dışı bırakın:** 

    1. Tıklayın **azure'a Dağıt** gelen [Windows VM çalıştırma disk şifreleme devre dışı bırak](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-windows-vm-without-aad) şablonu.
    2. Abonelik, kaynak grubu, konum, VM, birim türü, yasal koşulları ve Sözleşmesi'ni seçin.
    3.  Tıklayın **satın alma** çalışan bir Windows VM'de disk şifreleme devre dışı bırakmak için. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Linux için Azure Disk şifrelemesini etkinleştirme](azure-security-disk-encryption-linux.md)
