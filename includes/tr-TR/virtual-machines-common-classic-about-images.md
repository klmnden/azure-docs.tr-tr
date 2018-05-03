

Görüntüleri Azure'da bir işletim sistemine sahip yeni bir sanal makine sağlamak için kullanılır. Görüntüyü bir veya daha fazla veri diski de sahip olabilir. Görüntüleri çeşitli kaynaklardan kullanılabilir:

* Azure'un sunduğu görüntülerinde [Market](https://azure.microsoft.com/gallery/virtual-machines/). Windows Server'ın en son sürümleri ve Linux işletim sistemi dağıtımlarını vardır. Bazı görüntüleri de SQL Server gibi uygulamalar içerir. MSDN avantaj ve MSDN Kullandıkça Öde aboneleri ek görüntülere erişimi.
* Açık kaynak topluluk görüntülerini aracılığıyla sunar [VM deposu](http://vmdepot.msopentech.com/List/Index).
* Ayrıca depolayın ve kendi görüntülerini Azure üzerinde bir görüntü olarak kullanmak için mevcut bir Azure sanal makine yakalama veya görüntüyü karşıya kullanın.

## <a name="about-vm-images-and-os-images"></a>VM görüntüleri ve işletim sistemi görüntüleri hakkında
Azure'da iki görüntü türleri kullanılabilir: *VM görüntüsü* ve *işletim sistemi görüntüsü*. Bir VM görüntüsü, işletim sistemi ve görüntü oluşturulduğunda, bir sanal makineye bağlı tüm diskleri içerir. Bir VM görüntüsü, daha yeni resim türünde. VM görüntüleri sunulmadan önce Azure görüntüdeki genelleştirilmiş bir işletim sistemi ve hiçbir ek disk olabilir. Genelleştirilmiş bir işletim sistemini içeren bir VM görüntüsü görüntüsü, işletim sistemi görüntüsü özgün türü olarak aynıdır.

Azure'da sanal makine veya başka bir yerde çalışan bir sanal makine göre kendi görüntülerinizden kopyalayın ve karşıya yükleme oluşturabilirsiniz. Görüntünün birden fazla sanal makine oluşturmak için kullanmak istiyorsanız, bir resim olarak kullanılacak genelleme tarafından hazırlayın gerekir. Bir Windows Server görüntüsü oluşturmak için sunucuda .vhd dosyasını karşıya yüklemeden önce genelleştirmek için Sysprep komutunu çalıştırın. Sysprep hakkında daha fazla ayrıntı için bkz: [kullanım Sysprep nasıl: Giriş](http://go.microsoft.com/fwlink/p/?LinkId=392030) ve [sunucu rolleri için Sysprep desteği](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles). VM yedeklemesi, Sysprep çalıştırılmadan önce yedekleyin. Bir Linux görüntü oluşturma dağıtım göre değişir. Genellikle, bir dizi dağıtım noktasına özeldir ve Azure Linux Aracısı'nı Çalıştırma komutları çalıştırmanız gerekir.
