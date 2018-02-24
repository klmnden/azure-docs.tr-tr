


Yardımcı bir kullanılabilirlik kümesi sanal makinelerinizin kapalı kalma süresi sırasında kullanılabilir gibi tutmak bakımı sırasında. Bir kullanılabilirlik kümesinde iki veya daha fazla benzer şekilde yapılandırılmış sanal makine yerleştirme uygulamalar veya sanal makinenize çalışan hizmetleri kullanılabilirliğini sürdürmek için gerekli artıklık oluşturur. Bunun nasıl çalıştığı hakkında daha fazla bilgi için bkz [sanal makinelerin kullanılabilirliğini yönetme][Manage the availability of virtual machines].

Uygulamanızın her zaman verimli bir şekilde kullanılabilir olduğundan ve çalıştığından emin olmak için kullanılabilirlik kümeleri ve Yük Dengeleme uç noktaları kullanmak için en iyi bir uygulamadır. Yük dengeli uç noktalar hakkında daha fazla ayrıntı için bkz: [Yük Dengeleme için Azure altyapı hizmetleri][Load balancing for Azure infrastructure services].

Kullanılabilirlik kümesi iki seçenekten birini kullanarak içine Klasik sanal makineleri ekleyin:

* [Seçenek 1: bir sanal makine kullanılabilirlik aynı anda kümesi oluşturup][Option 1: Create a virtual machine and an availability set at the same time]. Ardından, bu sanal makineleri oluşturduğunuzda, yeni sanal makineler kümeye ekleyin.
* [Seçenek 2: var olan bir sanal makine bir kullanılabilirlik kümesine ekleme][Option 2: Add an existing virtual machine to an availability set].

> [!NOTE]
> Klasik modelde aynı kullanılabilirlik kümesinde yerleştirmek istediğiniz sanal makineleri aynı bulut hizmetine ait olmalıdır.
> 
> 

## <a id="createset"> </a>Seçenek 1: bir sanal makine ve kullanılabilirlik aynı anda kümesi oluşturma
Bunu yapmak için Azure portalında veya Azure PowerShell komutlarını kullanabilirsiniz.

Azure Portalı'nı kullanmak için:

1. Önceden yapmadıysanız, [Azure portal](https://portal.azure.com)da oturum açın
2. Tıklatın **kaynak oluşturma** > **işlem**.
3. Kullanmak istediğiniz Market sanal makine görüntüsü seçin. Bir Linux veya Windows sanal makine oluşturmak seçebilirsiniz.
4. Seçili sanal makine için dağıtım modeli değerine ayarlandığını doğrulayın **Klasik** ve ardından **oluştur**
   
    ![Alt görüntü metin](./media/virtual-machines-common-classic-configure-availability/ChooseClassicModel.png)
5. Bir sanal makine adı, kullanıcı adı ve parolası (Windows makineler) veya SSH ortak anahtarı (Linux makinelerde) girin. 
6. VM boyutunu seçin ve ardından **seçin** devam etmek için.
7. Seçin **isteğe bağlı yapılandırma > kullanılabilirlik kümesi**, ve sanal makineye eklemek istediğiniz kullanılabilirlik kümesini seçin.
   
    ![Alt görüntü metin](./media/virtual-machines-common-classic-configure-availability/ChooseAvailabilitySet.png) 
8. Yapılandırma ayarlarınızı gözden geçirin. İşiniz bittiğinde tıklatın **oluşturma**.
9. Azure sanal makine oluştururken altında ilerleme durumunu izleyebilirsiniz **sanal makineleri** hub menüsünde.

Bir Azure sanal makinesi oluşturmak ve yeni veya var olan kullanılabilirlik kümesine eklemek için Azure PowerShell komutlarını kullanmak için bkz: [oluşturmak ve Windows tabanlı sanal makineleri önceden için Azure PowerShell kullanma](../articles/virtual-machines/windows/classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a id="addmachine"> </a>Seçenek 2: var olan bir sanal makine bir kullanılabilirlik kümesi ekleyin.
Azure Portal'da, mevcut bir kullanılabilirlik Klasik varolan sanal makinelere veya bunları için yeni bir tane oluşturmak ekleyebilirsiniz. (Sanal makineler aynı kullanılabilirlik kümesindeki aynı bulut hizmetine ait olması gerektiğini aklınızda tutun.) Adımlar neredeyse aynıdır. Azure PowerShell ile sanal makine var olan bir kullanılabilirlik kümesine ekleyebilirsiniz.

1. Zaten yapmadıysanız, oturum [Azure portal](https://portal.azure.com).
2. Sol menüsünde **sanal makineleri (Klasik)**.
   
    ![Alt görüntü metin](./media/virtual-machines-common-classic-configure-availability/ChooseClassicVM.png)
3. Sanal makineler listesinden kümeye eklemek istediğiniz sanal makinenin adını seçin.
4. Seçin **kullanılabilirlik kümesi** sanal makineden **ayarları**.
   
    ![Alt görüntü metin](./media/virtual-machines-common-classic-configure-availability/AvailabilitySetSettings.png)
5. Sanal makineye eklemek istediğiniz kullanılabilirlik kümesi seçin. Sanal makine kullanılabilirlik kümesi aynı bulut hizmetine ait olmalıdır.
   
    ![Alt görüntü metin](./media/virtual-machines-common-classic-configure-availability/AvailabilitySetPicker.png)
6. **Kaydet**’e tıklayın.

Azure PowerShell komutlarını kullanmak için bir yönetici düzeyi Azure PowerShell oturumu açın ve aşağıdaki komutu çalıştırın. Yer tutucuları için (gibi &lt;VmCloudServiceName&gt;), dahil olmak üzere tırnak işaretleri içindeki her şeyi değiştirin < ve > karakterleri doğru adlara sahip.

    Get-AzureVM -ServiceName "<VmCloudServiceName>" -Name "<VmName>" | Set-AzureAvailabilitySet -AvailabilitySetName "<AvSetName>" | Update-AzureVM

> [!NOTE]
> Sanal makine kullanılabilirlik kümesine ekleme işlemini tamamlamak için yeniden başlatılması gerekebilir.
> 
> 

<!-- LINKS -->
[Option 1: Create a virtual machine and an availability set at the same time]: #createset
[Option 2: Add an existing virtual machine to an availability set]: #addmachine

[Load balancing for Azure infrastructure services]: ../articles/virtual-machines/virtual-machines-linux-load-balance.md
[Manage the availability of virtual machines]:../articles/virtual-machines/linux/manage-availability.md

[Create a virtual machine running Windows]: ../articles/virtual-machines/virtual-machines-windows-hero-tutorial.md
[Virtual Network overview]: ../articles/virtual-network/virtual-networks-overview.md

