## <a name="overview"></a>Genel Bakış
Oluşturduğunuzda, yeni bir sanal makine (VM) bir kaynak grubunda bir görüntüden dağıtarak [Azure Marketi](https://azure.microsoft.com/marketplace/), varsayılan işletim sistemi sürücüsü (bazı görüntüleri sahip küçük işletim sistemi disk boyutları varsayılan olarak) 127 GB görülür. VM’ye veri diskleri eklemek (kaç tane ekleyebileceğiniz seçtiğiniz SKU’ya bağlıdır) mümkün olmasına, hatta uygulamaları ve yoğun CPU kullanımlı iş yüklerini bu ek disklere yüklemeniz önerilmesine rağmen, sıklıkla müşterilerin aşağıdaki gibi belirli senaryoları etkinleştirmesi için işletim sistemi sürücüsünü genişletmesi gerekir:

1. İşletim sistemi sürücüsüne bileşen yükleyen eski uygulamaları destekleme.
2. Fiziksel bir bilgisayarı veya sanal makineyi daha büyük bir işletim sistemi sürücüsüne sahip şirket içi kaynaktan geçirme.

> [!IMPORTANT]
> Azure’da kaynak oluşturmak ve bunlarla çalışmak için iki farklı dağıtım modeli vardır: Resource Manager ve Klasik. Bu makalede Resource Manager modelinin kullanımı anlatılmaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.
> 
> 

## <a name="resize-the-os-drive"></a>İşletim sistemi sürücüsünü yeniden boyutlandırma
Bu makalede, [Azure Powershell](/powershell/azureps-cmdlets-docs)’in kaynak yöneticisi modüllerini kullanarak işletim sistemi sürücüsünü yeniden boyutlandırma görevini gerçekleştireceğiz. Her iki disk türleri arasında diskleri yeniden boyutlandırmak için yaklaşımı farklı olduğundan işletim sistemi sürücüsü için Unamanged ve yönetilen diskleri yeniden boyutlandırma göstereceğiz.

### <a name="for-resizing-unmanaged-disks"></a>Yönetilmeyen diskleri yeniden boyutlandırmak için:

Powershell ISE veya Powershell pencerenizi yönetim modunda açın ve aşağıdaki adımları izleyin:

1. Aşağıdakileri yaparak Microsoft Azure hesabınızda kaynak yönetimi modunda oturum açın ve aboneliğinizi seçin:
   
   ```Powershell
   Login-AzureRmAccount
   Select-AzureRmSubscription –SubscriptionName 'my-subscription-name'
   ```
2. Aşağıdakileri yaparak kaynak grubunuzun adını ve VM adını ayarlayın:
   
   ```Powershell
   $rgName = 'my-resource-group-name'
   $vmName = 'my-vm-name'
   ```
3. Aşağıdakileri yaparak sanal makineniz için bir başvuru edinin:
   
   ```Powershell
   $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
   ```
4. Aşağıdakileri yaparak diski yeniden boyutlandırmadan önce VM’yi durdurun:
   
    ```Powershell
    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    ```
5. İşte beklediğimiz an geldi! Yönetilmeyen işletim sistemi diskinin boyutu istenen değere ayarlayın ve VM şu şekilde güncelleştirin:
   
   ```Powershell
   $vm.StorageProfile.OSDisk.DiskSizeGB = 1023
   Update-AzureRmVM -ResourceGroupName $rgName -VM $vm
   ```
   
   > [!WARNING]
   > Yeni boyut mevcut disk boyutundan büyük olmalıdır. İzin verilen en yüksek işletim sistemi diskler için 2048 GB'dir. (VHD blob bu boyut ötesine genişletmek mümkündür, ancak işletim Sisteminin yalnızca alanı ilk 2048 GB ile çalışabilmek için görüntülenir.)
   > 
   > 
6. VM güncelleştirmesi biraz zaman alabilir. Komutun yürütülmesi tamamlandığında aşağıdakileri yaparak VM’yi yeniden başlatın:
   
   ```Powershell
   Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
   ```

### <a name="for-resizing-managed-disks"></a>Yönetilen diskleri yeniden boyutlandırmak için:

Powershell ISE veya Powershell pencerenizi yönetim modunda açın ve aşağıdaki adımları izleyin:

1. Aşağıdakileri yaparak Microsoft Azure hesabınızda kaynak yönetimi modunda oturum açın ve aboneliğinizi seçin:
   
   ```Powershell
   Login-AzureRmAccount
   Select-AzureRmSubscription –SubscriptionName 'my-subscription-name'
   ```
2. Aşağıdakileri yaparak kaynak grubunuzun adını ve VM adını ayarlayın:
   
   ```Powershell
   $rgName = 'my-resource-group-name'
   $vmName = 'my-vm-name'
   ```
3. Aşağıdakileri yaparak sanal makineniz için bir başvuru edinin:
   
   ```Powershell
   $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
   ```
4. Aşağıdakileri yaparak diski yeniden boyutlandırmadan önce VM’yi durdurun:
   
    ```Powershell
    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    ```
5. Yönetilen işletim sistemi diski başvurusu edinin. İstenen değeri yönetilen işletim sistemi disk boyutunu ayarlayın ve Disk şu şekilde güncelleştirin:
   
   ```Powershell
   $disk= Get-AzureRmDisk -ResourceGroupName $rgName -DiskName $vm.StorageProfile.OsDisk.Name
   $disk.DiskSizeGB = 1023
   Update-AzureRmDisk -ResourceGroupName $rgName -Disk $disk -DiskName $disk.Name
   ```   
   > [!WARNING]
   > Yeni boyut mevcut disk boyutundan büyük olmalıdır. İzin verilen en yüksek işletim sistemi diskler için 2048 GB'dir. (VHD blob bu boyut ötesine genişletmek mümkündür, ancak işletim Sisteminin yalnızca alanı ilk 2048 GB ile çalışabilmek için görüntülenir.)
   > 
   > 
6. VM güncelleştirmesi biraz zaman alabilir. Komutun yürütülmesi tamamlandığında aşağıdakileri yaparak VM’yi yeniden başlatın:
   
   ```Powershell
   Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
   ```

Hepsi bu! Şimdi RDP ile sanal makinenize girin, Bilgisayar Yönetimi’ni (veya Disk Yönetimi) açın ve yeni ayrılan alanı kullanarak sürücüyü genişletin.

## <a name="summary"></a>Özet
Bu makalede, PowerShell’in Azure Resource Manager modüllerini kullanarak bir IaaS sanal makinesinin işletim sistemi sürücüsünü genişlettik. Aşağıda çoğaltılamaz yönetilmeyen ve yönetilen diskleri için daha sonra başvurmak üzere tam komut verilmiştir:

Unamanged diskler:

```Powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName 'my-subscription-name'
$rgName = 'my-resource-group-name'
$vmName = 'my-vm-name'
$vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName
$vm.StorageProfile.OSDisk.DiskSizeGB = 1023
Update-AzureRmVM -ResourceGroupName $rgName -VM $vm
Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
```
Yönetilen Diskler:

```Powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName 'my-subscription-name'
$rgName = 'my-resource-group-name'
$vmName = 'my-vm-name'
$vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
Stop-AzureRMVM -ResourceGroupName $rgName -Name $vmName
$disk= Get-AzureRmDisk -ResourceGroupName $rgName -DiskName $vm.StorageProfile.OsDisk.Name
$disk.DiskSizeGB = 1023
Update-AzureRmDisk -ResourceGroupName $rgName -Disk $disk -DiskName $disk.Name
Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
```

## <a name="next-steps"></a>Sonraki Adımlar
Bu makalede, biz öncelikle VM'nin Unamanged/yönetilen işletim sistemi diski genişletme üzerinde odaklanmış olsa, geliştirilmiş betik VM'ye bağlı veri disklerinden genişletmek için de kullanılabilir. Örneğin, VM’ye bağlı ilk veri diskini genişletmek için ```StorageProfile``` öğesinin ```OSDisk``` nesnesini ```DataDisks``` dizisi ile değiştirin ve aşağıda gösterildiği gibi sayısal bir dizin kullanarak ilk bağlanan veri diskinin başvurusunu edinin:

Unamanged Disk:
```Powershell
$vm.StorageProfile.DataDisks[0].DiskSizeGB = 1023
```
Yönetilen Disk:
```Powershell
$disk= Get-AzureRmDisk -ResourceGroupName $rgName -DiskName $vm.StorageProfile.DataDisks[0].Name
$disk.DiskSizeGB = 1023
```

Benzer şekilde, yukarıdaki gibi bir dizini ya da diskin ```Name``` özelliğini aşağıda gösterildiği gibi kullanarak sanal makinenize bağlı diğer veri disklerine de başvurabilirsiniz:

Unamanged Disk:
```Powershell
($vm.StorageProfile.DataDisks | Where ({$_.Name -eq 'my-second-data-disk'}).DiskSizeGB = 1023
```
Manged Disk:
```Powershell
(Get-AzureRmDisk -ResourceGroupName $rgName -DiskName ($vm.StorageProfile.DataDisks | Where ({$_.Name -eq 'my-second-data-disk'})).Name).DiskSizeGB = 1023
```

Azure Resource Manager sanal makinesine disk bağlama hakkında bilgi edinmek istiyorsanız bu [makaleyi](../articles/virtual-machines/windows/attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) inceleyin.

