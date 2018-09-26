

Görüntüler, Azure'da bir işletim sistemine sahip yeni bir sanal makine sağlamak için kullanılır. Görüntü, bir veya daha fazla veri diski olarak da olabilir. Görüntüleri çeşitli kaynaklardan kullanılabilir:

* Azure'un sunduğu görüntülerde [Market](https://azure.microsoft.com/gallery/virtual-machines/). Windows Server'ın yeni sürümleri ve Linux işletim sistemi dağıtımlarını vardır. Bazı görüntüler, SQL Server gibi uygulamalar da içerir. MSDN avantajı ve MSDN Kullandıkça Öde aboneleri ek görüntülere erişebilir.
* Açık kaynak topluluk görüntüleri aracılığıyla sunar [olan VM Deposu'nda](http://vmdepot.msopentech.com/List/Index).
* Ayrıca depolayın ve bir görüntü olarak kullanmak için mevcut bir Azure sanal makine yakalama veya görüntüyü karşıya yükleme, Azure'da kendi görüntülerinizi kullanın.

## <a name="about-vm-images-and-os-images"></a>VM görüntüleri ve işletim sistemi görüntüleri hakkında
Azure'da iki görüntü türleri kullanılabilir: *VM görüntüsü* ve *işletim sistemi görüntüsü*. Bir VM görüntüsü, işletim sistemi ve görüntü oluşturulduğunda bir sanal makineye bağlı tüm diskleri içerir. Bir VM görüntüsü, yeni görüntü türüdür. VM görüntüleri sunulmadan önce görüntüyü azure'da genelleştirilmiş bir işletim sistemi ve hiçbir ek diskler olabilir. Yalnızca genelleştirilmiş bir işletim sistemini içeren bir VM görüntüsü görüntüsü, işletim sistemi görüntüsünü özgün türü olarak aynıdır.

Azure'da bir sanal makine veya başka bir yerde çalışan bir sanal makine göre kendi görüntülerinizi kopyalama ve yükleme oluşturabilirsiniz. Birden fazla sanal makine oluşturmak için bir görüntü kullanmak istiyorsanız, bunu bir görüntü olarak kullanılmaya genelleştirerek hazırlamanız gerekir. Bir Windows Server görüntüsü oluşturmak için sunucuda .vhd dosyasını karşıya yüklemeden önce bunu genelleştirmek için Sysprep komutunu çalıştırın. Sysprep hakkında daha fazla ayrıntı için bkz: [Sysprep işlemini kullanma: Giriş](http://go.microsoft.com/fwlink/p/?LinkId=392030) ve [sunucu rolleri için Sysprep Destek](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles). Sysprep çalıştırılmadan önce VM'yi yedekleme. Bir Linux görüntüsü oluşturma, dağıtım göre değişir. Genellikle, kümesi dağıtımına özgü olan ve Azure Linux Aracısı'nı Çalıştır komutları çalıştırmanız gerekir.
