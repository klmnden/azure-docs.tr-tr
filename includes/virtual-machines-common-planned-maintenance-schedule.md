

## <a name="multi-and-single-instance-vms"></a>Birden çok ve tek örnek VM'ler
Azure sayısına kritik planlı bakım nedeniyle kapalı kalma süresi--Vm'leri geçmelerini zaman bunlar zamanlayabilirsiniz çalışan pek çok müşteriniz yaklaşık 15 dakika--bu bakım sırasında oluşur. Kullanılabilirlik kümeleri, sağlanan VM'ler planlanmış bakım aldığınızda denetlemeye yardımcı olmak üzere kullanabilirsiniz.

Azure üzerinde çalışan sanal makineler için iki olası yapılandırmaları vardır. Sanal makineleri ya da çok örnekli veya tek örnek yapılandırılır. Sanal makineleri bir kullanılabilirlik kümesine varsa, bunlar birden çok örneği olarak yapılandırılır. Unutmayın, hatta VM'ler tek bir kullanılabilirlik kümesinde birden çok örneği olarak kabul edilir böylece dağıtılabilir. Ardından sanal makineleri bir kullanılabilirlik kümesine emin değilseniz, tek örnek yapılandırılır.  Kullanılabilirlik kümeleri hakkında daha fazla bilgi için bkz: [, Windows sanal makinelerin kullanılabilirliğini yönetme](../articles/virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) veya [, Linux sanal makinelerin kullanılabilirliğini yönetme](../articles/virtual-machines/linux/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Tek Örnekli ve çok örnekli VM'ler planlanmış bakım güncelleştirmeleri ayrı ayrı gerçekleşir. Vm'leriniz Tek Örnekli (çok örnekli olmaları durumunda) veya (tek örnek varsa) birden çok örneği olacak şekilde yeniden yapılandırarak Vm'leri planlı bakım aldığınızda kontrol edebilirsiniz. Bkz: [planlı bakım Azure Linux sanal makineleri için](../articles/virtual-machines/linux/planned-maintenance.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veya [planlı bakım Azure Windows sanal makineler için](../articles/virtual-machines/windows/planned-maintenance.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) Azure VM'ler için planlı bakım hakkında ayrıntılı bilgi için.

## <a name="for-multi-instance-configuration"></a>Çok örnekli yapılandırması
Bu sanal makineleri kullanılabilirlik kümesinden kaldırarak bir kullanılabilirlik kümesi yapılandırmasında dağıtılan Vm'leriniz planlı bakım etkiler zaman seçebilirsiniz.

1. Bir e-posta için çok örnekli yapılandırmasında Vm'leriniz için planlı bakım önce yedi Takvim günleri gönderilir. Abonelik kimlikleri ve etkilenen çok örnekli sanal makinelerin adlarını e-posta gövdesinde dahil edilir.
2. Bu yedi gün boyunca örneklerinizi çok örnekli Vm'leriniz bu bölgede kendi kullanılabilirlik kümesinden kaldırarak güncelleştirildiğinde seçebilirsiniz. Sanal makinenin bakım için hedeflenmiş değil başka bir fiziksel konağa bakım için hedeflenen bir fiziksel ana bilgisayardan taşınması gibi bu yapılandırma değişikliği bir yeniden başlatma neden olur.
3. Azure portalında, kullanılabilirlik VM kaldırabilirsiniz.

   1. Portalda, kullanılabilirlik kümesinden kaldırmak için VM seçin.  

   2. Altında **ayarları**, tıklatın **kullanılabilirlik kümesi**.

      ![Kullanılabilirlik kümesi seçimi](./media/virtual-machines-planned-maintenance-schedule/availabilitysetselection.png)

   3. Kullanılabilirlik kümesindeki açılır menüsünde, "Bölümünü bir kullanılabilirlik kümesinin." seçin

      ![Kümesinden Kaldır](./media/virtual-machines-planned-maintenance-schedule/availabilitysetwarning.png)

   4. En üstte tıklatın **kaydetmek**. Tıklatın **Evet** Bu eylem VM yeniden bildiremedi.

   >[!TIP]
   >Listelenen kullanılabilirlik kümeleri birini seçerek çok örnekli VM daha sonra yeniden yapılandırabilirsiniz.

4. Kullanılabilirlik kümesinden kaldırıldı VM'ler Tek Örnekli ana taşınır ve kullanılabilirlik kümesi yapılandırmaları için planlı bakım sırasında güncelleştirilmez.
5. Kullanılabilirlik kümesi Vm'lerine güncelleştirmeye (göre zamanlama) özgün e-postasında gösterilen tamamlandıktan sonra geri kullanılabilirlik kümeleri halinde VM'ler eklemeniz gerekir. Bir kullanılabilirlik kümesinin parçası olma VM'ler çok örneği olarak yeniden yapılandırır ve bir önyükleme sonuçlanır. Genellikle, tüm Azure ortamı genelinde tüm çok örnekli güncelleştirmeleri tamamlandığında, tek örnek bakım izler.

VM bir kullanılabilirlik kümesinden kaldırılması, ayrıca Azure PowerShell kullanarak yapabiliriz:

```
Get-AzureVM -ServiceName "<VmCloudServiceName>" -Name "<VmName>" | Remove-AzureAvailabilitySet | Update-AzureVM
```

## <a name="for-single-instance-configuration"></a>Tek örnek yapılandırması
Planlı bakım, sanal makineleri tek örnek yapılandırmada kullanılabilirlik kümeleri halinde bu Vm'lere ekleyerek etkiler zaman seçebilirsiniz.

Adım adım

1. Bir e-posta için yedi Takvim günleri VM'ler için planlı bakım tek örnek yapılandırmasında önce gönderilir. Abonelik kimlikleri ve etkilenen tek örnekli sanal makinelerin adlarını e-posta gövdesinde dahil edilir.
2. Bu yedi gün boyunca kullanılabilirlik kümesi, aynı bölgede Tek Örnekli Vm'leriniz ekleyerek örneğinizi yeniden zaman seçebilirsiniz. Sanal makinenin bakım için hedeflenmiş değil başka bir fiziksel konağa bakım için hedeflenen bir fiziksel ana bilgisayardan taşınması gibi bu yapılandırma değişikliği bir yeniden başlatma neden olur.
3. Azure portalı ve Azure PowerShell kullanarak kullanılabilirlik kümeleri var olan sanal makineleri eklemek için buraya yönergeleri izleyin. (Şu adımları izler Azure PowerShell örnek bakın.)
4. Bu Vm'lere çok örnekli olarak yapılandırılır sonra planlı bakım için tek örnek VM'ler bırakılır.
5. (Göre zamanlama özgün e-posta) Tek Örnekli VM güncelleştirmesi tamamlandıktan sonra bunların kullanılabilirlik kümesinden VM'ler kaldırarak sanal makineleri Tek Örnekli geri dönebilirsiniz.

Kullanılabilirlik de kümesi için bir VM ekleme, Azure PowerShell kullanarak yapabiliriz:

    Get-AzureVM -ServiceName "<VmCloudServiceName>" -Name "<VmName>" | Set-AzureAvailabilitySet -AvailabilitySetName "<AvSetName>" | Update-AzureVM

<!--Anchors-->



<!--Link references-->
[Virtual Machines Manage Availability]: virtual-machines-windows-tutorial.md
[Understand planned versus unplanned maintenance]: virtual-machines-manage-availability.md#Understand-planned-versus-unplanned-maintenance/
