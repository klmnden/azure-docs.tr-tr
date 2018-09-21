---
title: Azure Disk şifrelemesi Windows Iaas Vm'leri için etkinleştirme | Microsoft Docs
description: Bu makalede, Microsoft Azure Disk şifrelemesi için Windows Iaas Vm'leri etkinleştirme hakkında yönergeler sağlar.
author: mestew
ms.service: security
ms.subservice: Azure Disk Encryption
ms.topic: article
ms.author: mstewart
ms.date: 09/19/2018
ms.openlocfilehash: 04077c9acbd9556a66e8337ab8f415de86df1d5a
ms.sourcegitcommit: 8b694bf803806b2f237494cd3b69f13751de9926
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2018
ms.locfileid: "46498207"
---
# <a name="enable-azure-disk-encryption-for-windows-iaas-vms"></a>Windows Iaas Vm'leri için Azure Disk şifrelemesini etkinleştirme 

Çok sayıda disk şifreleme senaryoları etkinleştirebilirsiniz ve adımları senaryoya göre değişiklik gösterebilir. Aşağıdaki bölümlerde, Windows Iaas Vm'leri için daha ayrıntılı senaryoları kapsar. Disk şifreleme kullanmadan önce [Azure Disk şifrelemesi önkoşulları](../security/azure-security-disk-encryption-prerequisites.md) tamamlanması gerekir. 

Ele bir [anlık görüntü](../virtual-machines/windows/snapshot-copy-managed-disk.md) ve/veya diskler şifrelenir önce yedekleyin. Yedekleme kurtarma seçeneğini şifreleme sırasında beklenmeyen bir hata oluşması durumunda olası olduğundan emin olun. Şifreleme gerçekleşmeden önce yönetilen disklere sahip VM'ler yedeklemesini gerektirir. Bir yedekleme yapıldıktan sonra - skipVmBackup parametresini belirterek yönetilen disklerini şifrelemek için Set-AzureRmVMDiskEncryptionExtension cmdlet'ini kullanabilirsiniz. Yedekleme ve şifrelenmiş Vm'leri geri yükleme hakkında daha fazla bilgi için bkz. [Azure Backup](../backup/backup-azure-vms-encryption.md) makalesi. 

>[!WARNING]
> Azure Disk şifrelemesi, anahtar kasası ve VM'lerin aynı bölgede bulunması gerekir. Oluşturun ve şifrelenmiş VM ile aynı bölgede olan bir anahtar Kasası'nı kullanın. 


## <a name="bkmk_RunningWinVM"></a> Var olan veya Iaas Windows Vm'leri çalıştırma şifrelemeyi etkinleştir
Bu senaryoda, bir şablon, PowerShell cmdlet'leri veya CLI komutları kullanarak şifreleyebilirsiniz. Aşağıdaki bölümlerde, Azure Disk şifrelemesi etkinleştirme konusunda daha ayrıntılı olarak açıklanmaktadır. Sanal makine uzantısı için şema bilgileri gerekiyorsa bkz [Windows için Azure Disk şifrelemesi uzantı](../virtual-machines/extensions/azure-disk-enc-windows.md) makalesi.

>[!IMPORTANT]
 >Anlık görüntü için zorunludur ve/veya yedekleme yönetilen disk sanal makine örneğinin dışında ve Azure Disk şifrelemesi etkinleştirilmeden önce temel. Yönetilen diskin anlık görüntüsünü portaldan alınabilir veya [Azure Backup](../backup/backup-azure-vms-encryption.md) kullanılabilir. Yedekleme kurtarma seçeneğini şifreleme sırasında beklenmeyen bir hata olması durumunda olası olduğundan emin olun. Bir yedekleme yapıldıktan sonra Set-AzureRmVMDiskEncryptionExtension cmdlet - skipVmBackup parametresini belirterek yönetilen disklerini şifrelemek için kullanılabilir. Set-AzureRmVMDiskEncryptionExtension komut karşı temel alan bir yönetilen disk Vm'leri yedekleme yapılan ve bu parametre belirtilen kadar başarısız olur. 
>
>Şifrelenirken veya şifreleme devre dışı bırakıldığında VM yeniden başlatılmasına neden olabilir. 
>

### <a name="bkmk_RunningWinVMPSH"></a> Var olan veya Azure PowerShell ile sanal makine çalıştırma şifrelemeyi etkinleştir 
Kullanım [Set-AzureRmVMDiskEncryptionExtension](/powershell/module/azurerm.compute/set-azurermvmdiskencryptionextension) azure'da çalıştırılan Iaas sanal makine şifrelemesini etkinleştirmek için cmdlet'i. 

-  **Çalışan VM şifreleme:** aşağıdaki betiği değişkenlerinizi başlatır ve Set-AzureRmVMDiskEncryptionExtension cmdlet'ini çalıştırır. Kaynak grubunu, VM'yi ve anahtar kasası zaten önkoşul olarak oluşturulmuş olmalıdır. MySecureRg MySecureVM ve MySecureVault değerleriniz ile değiştirin.

     ```azurepowershell-interactive
      $rgName = 'MySecureRg';
      $vmName = 'MySecureVM';
      $KeyVaultName = 'MySecureVault';
      $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
      $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;

      Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId;
    ```
- **KEK kullanarak çalışan bir VM şifrele:** 

     ```azurepowershell-interactive
     $rgName = 'MySecureRg';
     $KeyVaultName = 'MySecureVault';
     $keyEncryptionKeyName = 'MyKeyEncryptionKey';
     $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
     $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     $keyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;

     Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId;

     ```
     
   >[!NOTE]
   > Disk şifreleme keyvault parametresinin değeri söz diziminin tam tanımlayıcı bir dizedir: / subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> Anahtar şifreleme anahtarı parametresinin değeri sözdizimi KEK olarak tam URI şudur: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

- **Diskleri şifrelenmiş doğrulayın:** Iaas VM şifreleme durumunu denetlemek için kullanmak [Get-AzureRmVmDiskEncryptionStatus](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus) cmdlet'i. 
     ```azurepowershell-interactive
     Get-AzureRmVmDiskEncryptionStatus -ResourceGroupName 'MySecureRg' -VMName 'MySecureVM'
     ```
    
- **Disk şifreleme devre dışı bırak:** şifreleme devre dışı bırakmak için [Disable-AzureRmVMDiskEncryption](/powershell/module/azurerm.compute/disable-azurermvmdiskencryption) cmdlet'i. Hem işletim sistemi ve veri diskleri şifrelediğinizde Windows VM'de disk şifrelemeyi devre dışı bırakma, beklendiği gibi çalışmaz. Bunun yerine tüm diskleri şifreleme devre dışı bırakın.

     ```azurepowershell-interactive
     Disable-AzureRmVMDiskEncryption -ResourceGroupName 'MySecureRG' -VMName 'MySecureVM'
     ```

### <a name="bkmk_RunningWinVMCLI"></a>Var olan veya Azure CLI ile sanal makine çalıştırma şifrelemeyi etkinleştir
Kullanım [az vm şifrelemeyi etkinleştirme](/cli/azure/vm/encryption#az-vm-encryption-enable) azure'da çalıştırılan Iaas sanal makine üzerinde şifrelemeyi etkinleştirmek için komutu.

-  **Çalışan bir VM şifrele:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --disk-encryption-keyvault "MySecureVault" --volume-type [All|OS|Data]
     ```

- **KEK kullanarak çalışan bir VM şifrele:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type [All|OS|Data]
     ```

     >[!NOTE]
     > Disk şifreleme keyvault parametresinin değeri söz diziminin tam tanımlayıcı bir dizedir: / subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name] </br> Anahtar şifreleme anahtarı parametresinin değeri sözdizimi KEK olarak tam URI şudur: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

- **Diskleri şifrelenmiş doğrulayın:** Iaas VM şifreleme durumunu denetlemek için kullanmak [az vm şifreleme show](/cli/azure/vm/encryption#az-vm-encryption-show) komutu. 

     ```azurecli-interactive
     az vm encryption show --name "MySecureVM" --resource-group "MySecureRg"
     ```

- **Şifreleme devre dışı bırak:** şifreleme devre dışı bırakmak için [az vm şifreleme devre dışı](/cli/azure/vm/encryption#az-vm-encryption-disable) komutu. Hem işletim sistemi ve veri diskleri şifrelediğinizde Windows VM'de disk şifrelemeyi devre dışı bırakma, beklendiği gibi çalışmaz. Bunun yerine tüm diskleri şifreleme devre dışı bırakın.

     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MySecureRg" --volume-type [ALL, DATA, OS]
     ```
 
 > [!NOTE]
 >Anlık görüntü için zorunludur ve/veya yedekleme yönetilen disk sanal makine örneğinin dışında ve Azure Disk şifrelemesi etkinleştirilmeden önce temel. Yönetilen diskin anlık görüntüsünü portaldan alınabilir veya [Azure Backup](../backup/backup-azure-vms-encryption.md) kullanılabilir. Yedekleme kurtarma seçeneğini şifreleme sırasında beklenmeyen bir hata olması durumunda olası olduğundan emin olun. Bir yedekleme yapıldıktan sonra Set-AzureRmVMDiskEncryptionExtension cmdlet - skipVmBackup parametresini belirterek yönetilen disklerini şifrelemek için kullanılabilir. Bu komut, yönetilen disk tabanlı Vm'leri karşı yapılan bir yedekleme ve bu parametre belirtildi kadar başarısız olacak. 
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
| keyVaultName | BitLocker anahtarı karşıya yüklenmelidir anahtar kasasının adı. Cmdlet'ini kullanarak elde edeceğinizi `(Get-AzureRmKeyVault -ResourceGroupName <MyResourceGroupName>). Vaultname` veya Azure CLI komutunu ' az keyvault list--resource-group "MySecureGroup" |Convertfrom-JSON'|
| keyVaultResourceGroup | Anahtar kasasını içeren kaynak grubunun adı|
|  KeyEncryptionKeyURL | Oluşturulan BitLocker anahtarı şifrelemek için kullanılan anahtar şifreleme anahtarı URL'si. Bu seçerseniz isteğe bağlı bir parametredir **nokek** UseExistingKek aşağı açılan listesinde. Seçerseniz **kek** UseExistingKek aşağı açılan listeden girmelisiniz _keyEncryptionKeyURL_ değeri. |
| VolumeType | Şifreleme işlemi gerçekleştirilir birim türü. Geçerli değerler _işletim sistemi_, _veri_, ve _tüm_. 
| forceUpdateTag | İşlemi çalıştırmak olması zorlamak gereken her zaman bir GUID gibi benzersiz bir değer geçirin. |
| resizeOSDisk | Sistem birimi bölme önce tam işletim sistemi VHD'si kaplayacak şekilde işletim sistemi bölümü yeniden boyutlandırılması. |
| location | Tüm kaynaklar için konum. |

## <a name="encrypt-virtual-machine-scale-sets"></a>Sanal makine ölçek kümeleri şifrele
[Azure sanal makine ölçek kümeleri](../virtual-machine-scale-sets/overview.md) oluşturma ve yönetme bir grup özdeş, yük dengeli sanal makineler sağlar. Tanımlı bir zamanlamaya veya talebe yanıt olarak sanal makine örneği sayısı otomatik olarak artabilir ya da azalabilir. Sanal makine ölçek kümeleri şifrelemek için CLI veya Azure PowerShell kullanın.


### <a name="register-for-disk-encryption-preview-using-azure-powershell"></a>Azure Powershell kullanarak disk şifreleme Önizleme için kaydolun

Önizleme sanal makine ölçek kümeleri için Azure disk şifrelemesi, kendi aboneliğinize kaydetmeniz gerektirir [Register-AzureRmProviderFeature](/powershell/module/azurerm.resources/register-azurermproviderfeature). Yalnızca ilk kez disk şifreleme önizleme özelliğini kullandığınızda aşağıdaki adımları gerekir:

```azurepowershell-interactive
Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName "UnifiedDiskEncryption"
```

Bu, kayıt isteği yaymak için 10 dakikaya kadar sürebilir. Kayıt durumunu denetleyebilirsiniz [Get-AzureRmProviderFeature](/powershell/module/AzureRM.Resources/Get-AzureRmProviderFeature). Zaman `RegistrationState` raporları *kayıtlı*, yeniden kaydetmeniz *Mirosoft.Compute* sağlayıcısıyla [Register-AzureRmResourceProvider](/powershell/module/AzureRM.Resources/Register-AzureRmResourceProvider):

```azurepowershell-interactive
Get-AzureRmProviderFeature -ProviderNamespace "Microsoft.Compute" -FeatureName "UnifiedDiskEncryption"
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute
```

### <a name="encrypt-virtual-machine-scale-sets-with-azure-powershell"></a>Azure PowerShell ile sanal makine ölçek kümeleri şifrele

Kullanım [Set-AzureRmVmssDiskEncryptionExtension](/powershell/module/azurerm.compute/set-azurermvmssdiskencryptionextension) cmdlet'ini bir Windows sanal makine ölçek kümesi şifrelemeyi etkinleştirin. Kaynak grubunu, VM'yi ve anahtar kasası zaten önkoşul olarak oluşturulmuş olmalıdır.

-  **Çalışan bir sanal makine ölçek kümesi şifrelemek**:
    ```azurepowershell-interactive
     $rgName= "MySecureRg";
     $VmssName = "MySecureVmss";
     $KeyVaultName= "MySecureVault";
     $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgName;
     $DiskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     Set-AzureRmVmssDiskEncryptionExtension -ResourceGroupName $rgName -VMScaleSetName $VmssName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId;


-  **Encrypt a running virtual machine scale set using KEK to wrap the key**:

    ```azurepowershell-interactive
     $rgName= "MySecureRg";
     $VmssName = "MySecureVmss";
     $KeyVaultName= "MySecureVault";
     $keyEncryptionKeyName = "MyKeyEncryptionKey";
     $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgName;
     $DiskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     Set-AzureRmVmssDiskEncryptionExtension -ResourceGroupName $rgName -VMScaleSetName $VmssName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId;
    ```

- **Bir sanal makine ölçek kümesi için şifreleme durumunu al:** kullanım [Get-AzureRmVmssVMDiskEncryption](/powershell/module/azurerm.compute/get-azurermvmssvmdiskencryption) cmdlet'i.
    
    ```azurepowershell-interactive
    get-AzureRmVmssVMDiskEncryption -ResourceGroupName "MySecureRG" -VMScaleSetName "MySecureVmss"
    ```

- **Bir sanal makine ölçek kümesi üzerinde şifrelemeyi devre dışı**: kullanım [Disable-AzureRmVmssDiskEncryption](/powershell/module/azurerm.compute/disable-azurermvmssdiskencryption) cmdlet'i. 

    ```azurepowershell-interactive
    Disable-AzureRmVmssDiskEncryption -ResourceGroupName "MySecureRG" -VMScaleSetName "MySecureVmss"
    ```

### <a name="register-for-disk-encryption-preview-using-azure-cli"></a>Azure CLI kullanarak disk şifreleme Önizleme için kaydolun

Önizleme sanal makine ölçek kümeleri için Azure disk şifrelemesi, kendi aboneliğinize kaydetmeniz gerektirir [az özelliği kayıt](/cli/azure/feature#az_feature_register). Yalnızca ilk kez disk şifreleme önizleme özelliğini kullandığınızda aşağıdaki adımları gerekir:

```azurecli-interactive
az feature register --name UnifiedDiskEncryption --namespace Microsoft.Compute
```

Bu, kayıt isteği yaymak için 10 dakikaya kadar sürebilir. Kayıt durumunu denetleyebilirsiniz [az özelliği show](/cli/azure/feature#az_feature_show). Zaman `State` raporları *kayıtlı*, yeniden kaydetmeniz *Mirosoft.Compute* sağlayıcısıyla [az provider register](/cli/azure/provider#az_provider_register):

```azurecli-interactive
az provider register --namespace Microsoft.Compute
```



###  <a name="encrypt-virtual-machine-scale-sets-with-azure-cli"></a>Azure CLI ile sanal makine ölçek kümeleri şifrele

Kullanım [az vmss şifrelemeyi etkinleştirme](/cli/azure/vmss/encryption#az-vmss-encryption-enable) bir Windows sanal makine ölçek kümesi şifrelemesini etkinleştirmek için. Yükseltme İlkesi el ile ölçek kümesi, şifreleme ile Başlat [az vmss update-instances](/cli/azure/vmss#az-vmss-update-instances). Kaynak grubunu, VM'yi ve anahtar kasası zaten önkoşul olarak oluşturulmuş olmalıdır.

-  **Çalışan bir sanal makine ölçek kümesi şifrele**
    ```azurecli-interactive
     az vmss encryption enable --resource-group "MySecureRG" --name "MySecureVmss" --disk-encryption-keyvault "MySecureVault" 
    ```

-  **Şifreleme anahtarı sarmalama için KEK kullanarak çalışan bir sanal makine ölçek**
    ```azurecli-interactive
     az vmss encryption enable --resource-group "MySecureRG" --name "MySecureVmss" --disk-encryption-keyvault "MySecureVault" --key-encryption-key "MyKEK" --key-encryption-keyvault "MySecureVault" 

     ```
- **Bir sanal makine ölçek kümesi için şifreleme durumunu al:** kullanım [az vmss şifreleme Göster](/cli/azure/vmss/encryption#az-vmss-encryption-show)

    ```azurecli-interactive
     az vmss encryption show --resource-group "MySecureRG" --name "MySecureVmss"
    ```

- **Bir sanal makine ölçek kümesi üzerinde şifrelemeyi devre dışı**: kullanım [az vmss şifrelemeyi devre dışı bırakma](/cli/azure/vmss/encryption#az-vmss-encryption-disable)
    ```azurecli-interactive
     az vmss encryption disable --resource-group "MySecureRG" --name "MySecureVmss"
    ```

## <a name="bkmk_VHDpre"> </a>VHD ve şifreleme anahtarları müşteri şifrelenmiş oluşturulan yeni Iaas VM'ler
Bu senaryoda, PowerShell cmdlet'lerini veya CLI komutları kullanarak şifreleme etkinleştirebilirsiniz. 

Yönergeleri ekte, Azure'da kullanılabilen önceden şifrelenmiş görüntüleri hazırlama için kullanın. Görüntü oluşturulduktan sonra şifrelenmiş bir Azure VM oluşturmak için sonraki bölümde adımları kullanabilirsiniz.

* [Önceden şifrelenmiş bir Windows VHD hazırlama](azure-security-disk-encryption-appendix.md#bkmk_preWin)
* [Önceden şifrelenmiş bir Linux VHD hazırlama](azure-security-disk-encryption-appendix.md#bkmk_preLinux)

>[!IMPORTANT]
 >Anlık görüntü için zorunludur ve/veya yedekleme yönetilen disk sanal makine örneğinin dışında ve Azure Disk şifrelemesi etkinleştirilmeden önce temel. Yönetilen diskin anlık görüntüsünü portaldan alınabilir veya [Azure Backup](../backup/backup-azure-vms-encryption.md) kullanılabilir. Yedekleme kurtarma seçeneğini şifreleme sırasında beklenmeyen bir hata olması durumunda olası olduğundan emin olun. Bir yedekleme yapıldıktan sonra Set-AzureRmVMDiskEncryptionExtension cmdlet - skipVmBackup parametresini belirterek yönetilen disklerini şifrelemek için kullanılabilir. Set-AzureRmVMDiskEncryptionExtension komut karşı temel alan bir yönetilen disk Vm'leri yedekleme yapılan ve bu parametre belirtilen kadar başarısız olur. 
>
>Şifrelenirken veya şifreleme devre dışı bırakıldığında VM yeniden başlatılmasına neden olabilir. 


### <a name="bkmk_VHDprePSH"> </a> Azure PowerShell ile önceden şifrelenmiş VHD'ler ile VM'lerin şifrele
Disk şifreli VHD'nizi PowerShell cmdlet'ini kullanarak şifreleyebilirsiniz [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk#examples). Aşağıdaki örnek, bazı ortak parametreler sunar. 

```azurepowershell-interactive
$VirtualMachine = New-AzureRmVMConfig -VMName "MySecureVM" -VMSize "Standard_A1"
$VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name "SecureOSDisk" -VhdUri "os.vhd" Caching ReadWrite -Windows -CreateOption "Attach" -DiskEncryptionKeyUrl "https://mytestvault.vault.azure.net/secrets/Test1/514ceb769c984379a7e0230bddaaaaaa" -DiskEncryptionKeyVaultId "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.KeyVault/vaults/mytestvault"
New-AzureRmVM -VM $VirtualMachine -ResouceGroupName "MySecureRG"
```

## <a name="enable-encryption-on-a-newly-added-data-disk"></a>Yeni eklenen verileri disk şifrelemeyi etkinleştirin
Yapabilecekleriniz [Windows PowerShell kullanarak bir VM için yeni bir disk ekleyin](../virtual-machines/windows/attach-disk-ps.md), veya [Azure portalı üzerinden](../virtual-machines/windows/attach-managed-disk-portal.md). 

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-powershell"></a>Azure PowerShell ile yeni eklenen bir disk şifrelemeyi etkinleştirin
 Windows VM'ler için yeni bir disk şifrelemek için PowerShell kullanırken, yeni bir dizisi sürümü belirtilmelidir. Dizisi sürümü benzersiz olması gerekir. Aşağıdaki komut dizisi sürümü için bir GUID oluşturur. Bazı durumlarda yeni eklenen veri diski tarafından Azure Disk şifrelemesi uzantısını otomatik olarak şifrelenebilir. Bu meydana gelirse, Set-AzureRmVmDiskEncryptionExtension cmdlet'i yeni dizisi sürümü ile yeniden çalıştırmanızı öneririz.
 

-  **Çalışan VM şifreleme:** aşağıdaki betiği değişkenlerinizi başlatır ve Set-AzureRmVMDiskEncryptionExtension cmdlet'ini çalıştırır. Kaynak grubunu, VM'yi ve anahtar kasası zaten önkoşul olarak oluşturulmuş olmalıdır. MySecureRg MySecureVM ve MySecureVault değerleriniz ile değiştirin. Bu örnekte, "All" hem işletim sistemi ve veri birimlerini içeren - VolumeType parametresi için kullanılır. Yalnızca işletim sistemi birimi şifrelemek istiyorsanız, - VolumeType parametresi için "OS" kullanın. 

     ```azurepowershell-interactive
      $sequenceVersion = [Guid]::NewGuid();
      $rgName = 'MySecureRg';
      $vmName = 'MySecureVM';
      $KeyVaultName = 'MySecureVault';
      $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
      $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;

      Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -VolumeType "All" –SequenceVersion $sequenceVersion;
    ```
- **KEK kullanarak çalışan bir VM şifreleme:** Bu örnek, hem işletim sistemi ve veri birimlerini içeren - VolumeType parametresi için "All" kullanır. Yalnızca işletim sistemi birimi şifrelemek istiyorsanız, - VolumeType parametresi için "OS" kullanın.

     ```azurepowershell-interactive
     $sequenceVersion = [Guid]::NewGuid();
     $rgName = 'MySecureRg';
     $vmName = 'MyExtraSecureVM';
     $KeyVaultName = 'MySecureVault';
     $keyEncryptionKeyName = 'MyKeyEncryptionKey';
     $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
     $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     $keyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;

     Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId -VolumeType "All" –SequenceVersion $sequenceVersion;

     ```

    >[!NOTE]
    > Disk şifreleme keyvault parametresinin değeri söz diziminin tam tanımlayıcı bir dizedir: / subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> Anahtar şifreleme anahtarı parametresinin değeri sözdizimi KEK olarak tam URI şudur: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-cli"></a>Azure CLI ile yeni eklenen bir disk şifrelemeyi etkinleştirin
 Şifrelemeyi etkinleştirmek için komutu çalıştırdığınızda Azure CLI komutunu otomatik olarak sizin için yeni bir sıra sürüm sağlayacaktır. Örnekte, "All" birim türü parametresi için kullanılır. Birim türü parametresi yalnızca işletim sistemi disk şifreleme işletim sistemine değiştirmeniz gerekebilir. PowerShell sözdizimi aksine, şifreleme etkinleştirilirken benzersiz dizisi sürümü sağlamak kullanıcı CLI gerektirmez. CLI'yı otomatik olarak oluşturur ve kendi benzersiz dizisi sürümü değeri kullanır.   

-  **Çalışan bir VM şifrele:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --disk-encryption-keyvault "MySecureVault" --volume-type "All"
     ```

- **KEK kullanarak çalışan bir VM şifrele:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type "All"
     ```


## <a name="disable-encryption"></a>Şifrelemeyi devre dışı bırakma
Azure PowerShell, Azure CLI kullanarak şifreleme devre dışı bırakabilir veya bir Resource Manager şablonu ile. Hem işletim sistemi ve veri diskleri şifrelediğinizde Windows VM'de disk şifrelemeyi devre dışı bırakma, beklendiği gibi çalışmaz. Bunun yerine tüm diskleri şifreleme devre dışı bırakın.

- **Azure PowerShell ile disk şifrelemesini devre dışı bırak:** şifreleme devre dışı bırakmak için [Disable-AzureRmVMDiskEncryption](/powershell/module/azurerm.compute/disable-azurermvmdiskencryption) cmdlet'i. 
     ```azurepowershell-interactive
     Disable-AzureRmVMDiskEncryption -ResourceGroupName 'MySecureRG' -VMName 'MySecureVM' -VolumeType "all"
     ```

- **Azure CLI ile şifreleme devre dışı bırak:** şifreleme devre dışı bırakmak için [az vm şifreleme devre dışı](/cli/azure/vm/encryption#az-vm-encryption-disable) komutu. 
     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MySecureRg" --volume-type "all"
     ```
- **Resource Manager şablonu ile şifreleme devre dışı bırakın:** 

    1. Tıklayın **azure'a Dağıt** gelen [Windows VM çalıştırma disk şifreleme devre dışı bırak](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-windows-vm-without-aad) şablonu.
    2. Abonelik, kaynak grubu, konum, VM, birim türü, yasal koşulları ve Sözleşmesi'ni seçin.
    3.  Tıklayın **satın alma** çalışan bir Windows VM'de disk şifreleme devre dışı bırakmak için. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Linux için Azure Disk şifrelemesini etkinleştirme](azure-security-disk-encryption-linux.md)
