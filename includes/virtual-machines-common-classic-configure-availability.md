---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 32f533d06b7db0284459951e65f9c04fe0bb0285
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61485402"
---
Yardımcı bir kullanılabilirlik kümesi, kesinti sırasında sanal makineleriniz gibi tutmak bakım sırasında. Bir kullanılabilirlik kümesinde iki veya daha çok benzer şekilde yapılandırılmış sanal makinelere yerleştirerek uygulamalar veya sanal makinenizi çalıştıran hizmet kullanılabilirliğini sürdürmek için gereken yedeklilik oluşturur. Bu işlemin nasıl çalıştığı hakkında daha fazla ayrıntı için bkz [sanal makinelerin kullanılabilirliğini yönetme][Manage the availability of virtual machines].

Uygulamanızı her zaman etkili bir şekilde kullanılabilir ve çalışır durumda olduğunu sağlamak için kullanılabilirlik kümeleri hem Yük Dengeleme uç kullanmak için en iyi bir uygulamadır. Yük dengeli uç noktaları hakkında daha fazla ayrıntı için bkz: [Yük Dengeleme için Azure altyapı hizmetlerinde][Load balancing for Azure infrastructure services].

Klasik sanal makineler, iki seçenekten birini kullanarak bir kullanılabilirlik kümesi içine ekleyebilirsiniz:

* [1. seçenek: Bir sanal makine ve bir kullanılabilirlik kümesi aynı anda oluşturma][Option 1: Create a virtual machine and an availability set at the same time]. Ardından, bu sanal makineler oluştururken yeni sanal makineler kümeye ekleyin.
* [2. seçenek: Mevcut bir sanal makine bir kullanılabilirlik kümesine ekleme][Option 2: Add an existing virtual machine to an availability set].

> [!NOTE]
> Klasik modeldeki sanal makinelerin aynı kullanılabilirlik kümesine yerleştirmek istediğiniz aynı bulut hizmetine ait olmalıdır.
> 
> 

## <a id="createset"> </a>1. seçenek: Bir sanal makine ve bir kullanılabilirlik kümesi aynı anda oluşturma
Bunu yapmak için Azure portal veya Azure PowerShell komutlarını kullanabilirsiniz.

Azure portalını kullanmak için:

1. Önceden yapmadıysanız, [Azure portal](https://portal.azure.com)da oturum açın
2. Tıklayın **kaynak Oluştur** > **işlem**.
3. Market, kullanmak istediğiniz sanal makine görüntüsünü seçin. Bir Linux veya Windows sanal makinesi oluşturmayı seçebilirsiniz.
4. Seçilen sanal makine için dağıtım modelini değerine ayarlandığını doğrulayın **Klasik** ve ardından **oluştur**
   
    ![Alternatif resim metni](./media/virtual-machines-common-classic-configure-availability/ChooseClassicModel.png)
5. Bir sanal makine adı, kullanıcı adı ve parola (Windows makineler için) veya SSH ortak anahtarı (Linux makineler için) girin. 
6. VM boyutunu seçin ve ardından **seçin** devam etmek için.
7. Seçin **isteğe bağlı yapılandırma > kullanılabilirlik kümesi**, ve sanal makineye eklemek istediğiniz kullanılabilirlik kümesini seçin.
   
    ![Alternatif resim metni](./media/virtual-machines-common-classic-configure-availability/ChooseAvailabilitySet.png) 
8. Yapılandırma ayarlarınızı gözden geçirin. İşiniz bittiğinde tıklayın **Oluştur**.
9. Azure, sanal makine oluştururken, altında ilerleme durumunu izleyebilirsiniz **sanal makineler** hub menüsünde.

Bir Azure sanal makinesi oluşturun ve yeni veya mevcut bir kullanılabilirlik kümesine eklemek için Azure PowerShell komutlarını kullanmak için bkz: [oluşturma ve önceden yapılandırma Windows tabanlı sanal makineler için Azure PowerShell'i kullanma](../articles/virtual-machines/windows/classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a id="addmachine"> </a>2. seçenek: Bir kullanılabilirlik kümesine mevcut bir sanal makine ekleyin
Azure portalında mevcut Klasik sanal makineleri için mevcut bir kullanılabilirlik kümesi veya bunlar için yeni bir tane oluşturun ekleyebilirsiniz. (Aynı kullanılabilirlik kümesindeki sanal makinelerin aynı bulut hizmetine ait olmalıdır aklınızda tutun.) Adımlar neredeyse aynıdır. Azure PowerShell ile sanal makineyi var olan bir kullanılabilirlik kümesine ekleyebilirsiniz.

1. Zaten yapmadıysanız, oturum [Azure portalında](https://portal.azure.com).
2. Sol menüsünde **sanal makineler (Klasik)**.
   
    ![Alternatif resim metni](./media/virtual-machines-common-classic-configure-availability/ChooseClassicVM.png)
3. Sanal makineler listesinden kümeye eklemek istediğiniz sanal makinenin adını seçin.
4. Seçin **kullanılabilirlik kümesi** sanal makineden **ayarları**.
   
    ![Alternatif resim metni](./media/virtual-machines-common-classic-configure-availability/AvailabilitySetSettings.png)
5. Sanal makineye eklemek istediğiniz kullanılabilirlik kümesi seçin. Sanal makine kullanılabilirlik kümesi aynı bulut hizmetine ait olmalıdır.
   
    ![Alternatif resim metni](./media/virtual-machines-common-classic-configure-availability/AvailabilitySetPicker.png)
6. **Kaydet**’e tıklayın.

Azure PowerShell komutlarını kullanmak için bir yönetici düzeyinde Azure PowerShell oturumu açın ve aşağıdaki komutu çalıştırın. Yer tutucu (gibi &lt;VmCloudServiceName&gt;), tırnak işaretleri dahil olmak üzere, içindeki her şeyi değiştirin < ve > karakterleri doğru ada sahip.

    Get-AzureVM -ServiceName "<VmCloudServiceName>" -Name "<VmName>" | Set-AzureAvailabilitySet -AvailabilitySetName "<AvSetName>" | Update-AzureVM

> [!NOTE]
> Sanal makineyi kullanılabilirlik kümesine ekleme işlemini sonlandırmak için yeniden başlatılması gerekebilir.
> 
> 

<!-- LINKS -->
[Option 1: Create a virtual machine and an availability set at the same time]: #createset
[Option 2: Add an existing virtual machine to an availability set]: #addmachine

[Load balancing for Azure infrastructure services]: ../articles/virtual-machines/virtual-machines-linux-load-balance.md
[Manage the availability of virtual machines]:../articles/virtual-machines/linux/manage-availability.md

[Create a virtual machine running Windows]: ../articles/virtual-machines/virtual-machines-windows-hero-tutorial.md
[Virtual Network overview]: ../articles/virtual-network/virtual-networks-overview.md

