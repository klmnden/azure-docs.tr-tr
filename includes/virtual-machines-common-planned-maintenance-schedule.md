---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: c7fe0d6f8e03501cca7a8b98f95286b6a21c0476
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60232788"
---
## <a name="multi-and-single-instance-vms"></a>Birden çok ve tek örnekli VM'ler
Azure sayısına kritik Vm'lerini planlı bakım nedeniyle kapalı kalma süresi--geçmeleri olduğunda bunlar zamanlayabilirsiniz çalışan pek çok müşteriniz yaklaşık 15 dakika--bu bakım sırasında gerçekleşir. Kullanılabilirlik kümeleri, sağlanan VM'ler planlanmış bakım aldığınızda denetlemenize yardımcı olmak için kullanabilirsiniz.

Azure üzerinde çalışan sanal makineler için iki olası yapılandırmalar vardır. VM'ler, ya da çok örnekli veya tek örnek olarak yapılandırılır. Bir kullanılabilirlik kümesindeki VM'ler varsa bunlar çok örnekli yapılandırılır. Unutmayın, hatta Vm'leri tek bir kullanılabilirlik kümesine dağıtılabilir ve böylece çok örnekli kabul edilir. Ardından bir kullanılabilirlik kümesindeki VM'ler emin değilseniz, tek örnek yapılandırılır.  Kullanılabilirlik kümeleri hakkında daha fazla bilgi için bkz: [, Windows sanal makinelerinin kullanılabilirliğini yönetme](../articles/virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) veya [, Linux sanal makinelerinin kullanılabilirliğini yönetme](../articles/virtual-machines/linux/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Tek Örnekli ve çok örnekli VM'ler için planlı bakım güncelleştirmeleri ayrı ayrı gerçekleşir. Vm'lerinizi Tek Örnekli (çok örnekli olmaları durumunda) veya (Tek Örnekli olmaları durumunda) çok örnekli olacak şekilde yeniden yapılandırarak Vm'lerini planlı bakım aldığınızda kontrol edebilirsiniz. Bkz: [Azure Linux sanal makineleri için planlı Bakımı](../articles/virtual-machines/linux/planned-maintenance.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veya [Azure Windows sanal makineler için planlı Bakımı](../articles/virtual-machines/windows/planned-maintenance.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) Azure Vm'lerinde planlı bakım hakkında ayrıntılı bilgi için.

## <a name="for-multi-instance-configuration"></a>Çok örnekli yapılandırma için
Planlı bakım Vm'lerinizi bir kullanılabilirlik kümesi yapılandırma bu VM'lerin kullanılabilirlik kümelerini kaldırarak dağıtılan etkiler zaman seçebilirsiniz.

1. E-posta için önce bir çok örnekli yapılandırmadaki sanal makineleriniz için planlı bakım yedi takvim günü gönderilir. E-postanın gövdesinde, etkilenen çok örnekli sanal makinelerin adlarını ve abonelik kimlikleri dahil edilir.
2. Bu yedi gün boyunca örneklerinizin çok örnekli Vm'lerinizi bu bölgede, kullanılabilirlik kümesinden kaldırarak güncelleştirildiğinde seçebilirsiniz. Bu yapılandırma değişikliği bakım, bakım için hedeflenen olmayan başka bir fiziksel konağa için hedeflenen bir fiziksel ana bilgisayardan bir sanal makine taşıma gibi bir yeniden başlatma neden olur.
3. VM Azure portalında kullanılabilirliğini kaldırabilirsiniz.

   1. Portalda, kullanılabilirlik kümesinden kaldırmak için VM'yi seçin.  

   2. Altında **ayarları**, tıklayın **kullanılabilirlik kümesi**.

      ![Kullanılabilirlik kümesi seçimi](./media/virtual-machines-planned-maintenance-schedule/availabilitysetselection.png)

   3. Kullanılabilirlik kümesindeki açılır menüsünde, "Bölümü bir kullanılabilirlik kümesine ait." seçin

      ![Kümesinden Kaldır](./media/virtual-machines-planned-maintenance-schedule/availabilitysetwarning.png)

   4. En üstünde tıklayın **Kaydet**. Tıklayın **Evet** kabul Bu eylem, VM'yi yeniden başlatır.

   >[!TIP]
   >Listelenen kullanılabilirlik kümelerinden birini seçerek çok örnekli VM daha sonra yeniden yapılandırabilirsiniz.

4. Kullanılabilirlik kümesinden kaldırıldı Vm'leri Tek Örnekli Konaklara taşınır ve kullanılabilirlik kümesi yapılandırmaları için planlı bakım sırasında güncelleştirilmez.
5. Kullanılabilirlik kümesi sanal makineleri için güncelleştirme (zamanlama) özgün e-postada özetlenen göre tamamlandıktan sonra kullanılabilirlik kümeleri halinde Vm'leri eklemeniz gerekir. Bir kullanılabilirlik kümesinin bir parçası haline çok örnekli VM'ler yeniden yapılandırır ve bir yeniden başlatılmasına neden olur. Tüm çok örnekli güncelleştirmeler arasında tüm Azure ortamınızı tamamlandığında, genellikle, tek örnekli bakım izler.

VM bir kullanılabilirlik kümesinden kaldırılması, ayrıca Azure PowerShell kullanarak gerçekleştirilebilir:

```
Get-AzureVM -ServiceName "<VmCloudServiceName>" -Name "<VmName>" | Remove-AzureAvailabilitySet | Update-AzureVM
```

## <a name="for-single-instance-configuration"></a>Tek Örnekli yapılandırma için
Planlı bakım, Vm'leri Tek Örnekli yapılandırmada bu VM'lerin kullanılabilirlik kümeleri halinde ekleyerek etkiler zaman seçebilirsiniz.

Adım adım

1. E-posta için yedi takvim günü boyunca sanal makineleri için planlı bakım Tek Örnekli yapılandırmada önce gönderilir. E-postanın gövdesinde, etkilenen tek örnekli sanal makinelerin adlarını ve abonelik kimlikleri dahil edilir.
2. Bu yedi gün boyunca, tek örnekli sanal makinelerinizin bir kullanılabilirlik kümesi, aynı bölgede ekleyerek örneğinizin yeniden zaman seçebilirsiniz. Bu yapılandırma değişikliği bakım, bakım için hedeflenen olmayan başka bir fiziksel konağa için hedeflenen bir fiziksel ana bilgisayardan bir sanal makine taşıma gibi bir yeniden başlatma neden olur.
3. Azure portalı ve Azure PowerShell ile kullanılabilirlik kümeleri halinde mevcut Vm'leri eklemek için yönergeleri izleyin. (Bu adımları takip eden Azure PowerShell örneğine bakın.)
4. Bu sanal makineler, çok örnekli yapılandırılır sonra tek örnekli VM'ler için planlı bakım dışlandığını.
5. (Özgün e-postayı zamanlama göre) Tek Örnekli VM güncelleştirmesi tamamlandıktan sonra kullanılabilirlik kümelerini Vm'leri kaldırarak Tek Örnekli VM'ler döndürebilir.

Kullanılabilirlik kümesine de bir VM ekleme, Azure PowerShell kullanarak gerçekleştirilebilir:

    Get-AzureVM -ServiceName "<VmCloudServiceName>" -Name "<VmName>" | Set-AzureAvailabilitySet -AvailabilitySetName "<AvSetName>" | Update-AzureVM

<!--Anchors-->



<!--Link references-->
[Virtual Machines Manage Availability]: virtual-machines-windows-tutorial.md
[Understand planned versus unplanned maintenance]: virtual-machines-manage-availability.md#Understand-planned-versus-unplanned-maintenance/
