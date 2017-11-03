## <a name="overview"></a>Genel Bakış
Oluşturduğunuzda, yeni bir sanal makine (VM) bir kaynak grubunda bir görüntüden dağıtarak [Azure Marketi](https://azure.microsoft.com/marketplace/), varsayılan işletim sistemi sürücüsü (bazı görüntüleri sahip küçük işletim sistemi disk boyutları varsayılan olarak) 127 GB görülür. VM’ye veri diskleri eklemek (kaç tane ekleyebileceğiniz seçtiğiniz SKU’ya bağlıdır) mümkün olmasına, hatta uygulamaları ve yoğun CPU kullanımlı iş yüklerini bu ek disklere yüklemeniz önerilmesine rağmen, sıklıkla müşterilerin aşağıdaki gibi belirli senaryoları etkinleştirmesi için işletim sistemi sürücüsünü genişletmesi gerekir:

1. İşletim sistemi sürücüsüne bileşen yükleyen eski uygulamaları destekleme.
2. Fiziksel bir bilgisayarı veya sanal makineyi daha büyük bir işletim sistemi sürücüsüne sahip şirket içi kaynaktan geçirme.

> [!IMPORTANT]
> Azure’da kaynak oluşturmak ve bunlarla çalışmak için iki farklı dağıtım modeli vardır: Resource Manager ve Klasik. Bu makalede Resource Manager modelinin kullanımı anlatılmaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.
> 
> 

## <a name="resize-the-os-drive"></a>İşletim sistemi sürücüsünü yeniden boyutlandırma
Bu makalede, [Azure Powershell](/powershell/azureps-cmdlets-docs)’in kaynak yöneticisi modüllerini kullanarak işletim sistemi sürücüsünü yeniden boyutlandırma görevini gerçekleştireceğiz. Powershell ISE veya Powershell pencerenizi yönetim modunda açın ve aşağıdaki adımları izleyin:

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
5. İşte beklediğimiz an geldi! Aşağıdakileri yaparak işletim sistemi diskinin boyutunu istenen değere ayarlayın ve VM’yi güncelleştirin:
   
   ```Powershell
   $vm.StorageProfile.OSDisk.DiskSizeGB = 1023
   Update-AzureRmVM -ResourceGroupName $rgName -VM $vm
   ```
   
   > [!WARNING]
   > Yeni boyut mevcut disk boyutundan büyük olmalıdır. İzin verilen maksimum 2048 GB'dir. (VHD blob bu boyut ötesine genişletmek mümkündür, ancak işletim Sisteminin yalnızca alanı ilk 2048 GB ile çalışabilmek için görüntülenir.)
   > 
   > 
6. VM güncelleştirmesi biraz zaman alabilir. Komutun yürütülmesi tamamlandığında aşağıdakileri yaparak VM’yi yeniden başlatın:
   
   ```Powershell
   Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
   ```

Hepsi bu! Şimdi RDP ile sanal makinenize girin, Bilgisayar Yönetimi’ni (veya Disk Yönetimi) açın ve yeni ayrılan alanı kullanarak sürücüyü genişletin.

## <a name="summary"></a>Özet
Bu makalede, PowerShell’in Azure Resource Manager modüllerini kullanarak bir IaaS sanal makinesinin işletim sistemi sürücüsünü genişlettik. Aşağıda başvurabilmeniz için betiğin tamamının kopyası verilmiştir:

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

## <a name="next-steps"></a>Sonraki Adımlar
Bu makalede öncelikli olarak sanal makinenin işletim sistemi diskini genişletmeye odaklanmış olsak da geliştirilen betik, tek bir kod satırının değiştirilmesiyle VM’ye bağlı veri disklerinin genişletilmesi için de kullanılabilir. Örneğin, VM’ye bağlı ilk veri diskini genişletmek için ```StorageProfile``` öğesinin ```OSDisk``` nesnesini ```DataDisks``` dizisi ile değiştirin ve aşağıda gösterildiği gibi sayısal bir dizin kullanarak ilk bağlanan veri diskinin başvurusunu edinin:

```Powershell
$vm.StorageProfile.DataDisks[0].DiskSizeGB = 1023
```
Benzer şekilde, yukarıdaki gibi bir dizini ya da diskin ```Name``` özelliğini aşağıda gösterildiği gibi kullanarak sanal makinenize bağlı diğer veri disklerine de başvurabilirsiniz:

```Powershell
($vm.StorageProfile.DataDisks | Where {$_.Name -eq 'my-second-data-disk'})[0].DiskSizeGB = 1023
```

Azure Resource Manager sanal makinesine disk bağlama hakkında bilgi edinmek istiyorsanız bu [makaleyi](../articles/virtual-machines/windows/attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) inceleyin.

