## <a name="understand-vm-reboots---maintenance-vs-downtime"></a>VM Yeniden Başlatma İşlemlerini Anlama - bakım ve kapalı kalma süresi
Etkilenip azure'da sanal makine açabilir üç senaryo vardır: planlanmamış donanım bakım, beklenmeyen kapalı kalma süresi ve planlı Bakım.

* **Plansız Donanım Bakımı Olayı**, Azure platformu donanımın veya fiziksel makineyle ilişkili herhangi bir platform bileşeninin arıza yapmak üzere olduğunu tahmin ettiğinde gerçekleşir. Platform bir arıza öngördüğünde, donanımda barındırılan sanal makineler üzerindeki etkiyi azaltmak amacıyla plansız donanım bakımı olayı düzenler. Azure, arızalı donanımdaki Sanal Makineleri sağlıklı bir fiziksel makineye geçirmek için Dinamik Geçiş teknolojisini kullanır. Dinamik Geçiş, Sanal Makineyi yalnızca kısa bir süre için duraklatan bir VM koruma işlemidir. Bellek, açık dosyalar ve ağ bağlantıları korunur, ancak olaydan önce ve/veya sonra performans azalabilir. Dinamik Geçişin kullanılamadığı durumlarda VM, aşağıda açıklanan Beklenmeyen Kapalı Kalma Süresi yaşar.


* **Beklenmeyen Kapalı Kalma Süresi**, sanal makinenizin altında yatan donanım ya da fiziksel altyapı bir şekilde arıza yaptığında nadiren gerçekleştirilir. Buna yerel ağ hataları, yerel disk hataları veya raf düzeyinde diğer hatalar dahildir. Bu tür bir arıza tespit edildiğinde Azure platformu otomatik olarak geçirir (heals) sanal makinenize aynı veri merkezinde sağlıklı bir fiziksel makine. İyileştirme yordamı sırasında sanal makineler kapalı kalır (yeniden başlatma) ve bazı durumlarda geçici sürücü kaybı yaşar. Bağlı işletim sistemi ve veri diskleri her zaman korunur. 

  Sanal makineler de kapalı kalma süresi ile kurtarılamaz bir kesinti veya veri merkezinin tamamı ya da tüm bir bölgeyi etkileyen olağanüstü durum yaşayabilirsiniz. Bu senaryolar için koruma seçeneklerini de dahil olmak üzere Azure sağlar [kullanılabilirlik bölgeleri](../articles/availability-zones/az-overview.md) ve [eşleştirilmiş bölgeleri](../articles/best-practices-availability-paired-regions.md#what-are-paired-regions).

* **Planlı Bakım olayları**, Microsoft tarafından sanal makinelerinizin çalıştığı platforma ait genel güvenilirlik, performans ve güvenliği artırmak amacıyla temel alınan Azure platformunda yapılan periyodik güncelleştirmelerdir. Bu güncelleştirmelerin çoğu Sanal Makine veya Bulut Hizmetlerinizi etkilemeden gerçekleştirilir (bkz. [VM Koruyucu Bakım](https://docs.microsoft.com/azure/virtual-machines/windows/preserving-maintenance)). Azure platformu mümkün olan tüm durumlarda VM Koruyucu Bakımı kullanmaya çalışsa da, gerekli güncelleştirmelerin temel alınan altyapıya uygulanması için bu güncelleştirmelerin sanal makineyi yeniden başlatmayı gerektirdiği nadir örnekler vardır. Bu durumda, uygun zaman penceresi içinde VM’lere yönelik bakımı başlatarak Maintenance-Redeploy işlemi ile Azure Planlı Bakımını gerçekleştirebilirsiniz. Daha fazla bilgi için bkz. [Sanal Makineler için Planlı Bakım](https://docs.microsoft.com/azure/virtual-machines/windows/planned-maintenance/).


Bu olayların bir veya daha fazlası nedeniyle kapalı kalma süresinin etkisini azaltmak için, sanal makinelerinizde aşağıdaki yüksek kullanılabilirlik en iyi uygulamalarının kullanılması önerilir:

* [Bir kullanılabilirlik kümesindeki birden fazla sanal makineyi yedeklilik için yapılandırma]
* [Bir kullanılabilirlik kümesindeki VM’ler için yönetilen diskleri kullanma]
* [Önceden yanıt olayları etkileyen VM için zamanlanmış olaylarını kullanın] (https://docs.microsoft.com/azure/virtual-machines/virtual-machines-scheduled-events)
* [Her uygulama katmanını ayrı kullanılabilirlik kümeleri halinde yapılandırma]
* [Yük Dengeleyiciyi kullanılabilirlik kümeleri ile birleştirme]
* [Veri merkezi düzeyi arızasına karşı korumak için kullanılabilirlik bölgelerini kullanın]

## <a name="configure-multiple-virtual-machines-in-an-availability-set-for-redundancy"></a>Bir kullanılabilirlik kümesindeki birden fazla sanal makineyi yedeklilik için yapılandırma
Uygulamanıza yedeklilik sağlamak için bir kullanılabilirlik kümesinde iki veya daha fazla sanal makinenin gruplandırılması önerilir. Bu yapılandırma bir veri merkezinde bulunan ya da bir planlı veya plansız bir bakım olayı sırasında en az bir sanal makine kullanılabilir %99,95 karşılayan olmasını sağlar ve Azure SLA. Daha fazla bilgi için bkz. [Sanal Makineler için SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/).

> [!IMPORTANT]
> Tek örnekli bir sanal makineyi bir kullanılabilirlik kümesinde tek başına bırakmaktan kaçının. Bu yapılandırmadaki sanal makineler SLA garantisine uygun değildir ve tek bir VM’nin [Azure Premium Depolama](../articles/virtual-machines/windows/premium-storage.md) kullandığı durumlar dışında planlı Azure bakım olayları sırasında kapalı kalma süresiyle karşılaşır. Premium depolama kullanan tek VM'ler için Azure SLA geçerlidir.

Kullanılabilirlik kümenizdeki her sanal makineye, temel alınan Azure platformu tarafından bir **güncelleme etki alanı** ve bir **hata etki alanı** atanır. Belirli bir kullanılabilirlik kümesi için, aynı anda yeniden başlatılabilecek sanal makine gruplarını ve temel alınan fiziksel donanımları göstermek üzere, kullanıcı tarafından yapılandırılabilen beş güncelleme etki alanı varsayılan olarak atanır (Resource Manager dağıtımları daha sonra en fazla 20 güncelleme etki alanı sağlayacak şekilde artırılabilir). Tek bir kullanılabilirlik kümesinde beşten fazla sanal makine yapılandırıldığında, altıncı sanal makine birinci sanal makine ile, yedinci sanal makine ikinci sanal makine ile aynı güncelleme etki alanına yerleştirilir ve yerleştirme işlemi bu düzende devam eder. Yeniden başlatılmakta olan güncelleme etki alanlarının sırası, planlanan bakım sırasında sıralı olarak uygulanmayabilir, ancak aynı anda yalnızca bir güncelleme etki alanı yeniden başlatılır. Yeniden başlatılmış bir güncelleme etki alanının kurtarılması için, farklı bir güncelleme etki alanında bakım başlatılmadan önce 30 dakika beklenir.

Hata etki alanları ortak bir güç kaynağı ve ağ anahtarını paylaşan sanal makine grubunu tanımlar. Varsayılan olarak, kullanılabilirlik kümenizde yapılandırılmış olan sanal makineler, Resource Manager dağıtımları için en fazla üç hata etki alanı (Klasik için iki etki alanı) arasında ayrılır. Sanal makinelerinizin bir kullanılabilirlik kümesine yerleştirilmesi uygulamanızı işletim sistemine veya uygulamaya özel hatalardan korumasa da, olası fiziksel donanım hatalarının, ağ kesintilerinin veya güç kesintilerinin etkilerini sınırlar.

<!--Image reference-->
   ![Güncelleme etki alanı ve hata etki alanı yapılandırmasının kavramsal çizimi](./media/virtual-machines-common-manage-availability/ud-fd-configuration.png)

## <a name="use-managed-disks-for-vms-in-an-availability-set"></a>Bir kullanılabilirlik kümesindeki VM’ler için yönetilen diskleri kullanma
Şu anda yönetilmeyen disklere sahip VM’ler kullanıyorsanız, [Kullanılabilirlik Kümesindeki VM’leri Yönetilen Diskleri kullanacak şekilde dönüştürmeniz](../articles/virtual-machines/windows/convert-unmanaged-to-managed-disks.md) önemle tavsiye edilir.

[Yönetilen diskler](../articles/virtual-machines/windows/managed-disks-overview.md), bir Kullanılabilirlik Kümesindeki VM disklerinin tek arıza noktalarından kaçınmak üzere birbirinden yeterince ayrılmasını sağlayarak Kullanılabilirlik Kümeleri için daha fazla güvenilirlik sunar. Bunu otomatik olarak diskleri (depolama kümeleri) farklı depolama hatası alanlarında yerleştirip VM hata etki alanı ile hizalama yapar. Depolama hata etki alanı donanım veya yazılım arızasından dolayı başarısız olursa, yalnızca VM örneği depolama hata etki alanı disklerle başarısız olur.
![Yönetilen disklerde FDs](./media/virtual-machines-common-manage-availability/md-fd-updated.png)

> [!IMPORTANT]
> Yönetilen kullanılabilirlik kümelerine yönelik arıza etki alanlarının sayısı bölgeye göre farklılık gösterir (bölge başına iki veya üç). Aşağıdaki tabloda bölge başına sayı gösterilmektedir

[!INCLUDE [managed-disks-common-fault-domain-region-list](managed-disks-common-fault-domain-region-list.md)]

VM’leri [yönetilmeyen diskler](../articles/virtual-machines/windows/about-disks-and-vhds.md#types-of-disks) ile kullanmayı planlıyorsanız, VM sanal sabit disklerinin (VHD) [sayfa blobu](https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs#about-page-blobs) olarak depolandığı Depolama hesapları en iyi deneyimlerini uygulayın.

1. **Bir VM ile ilişkili tüm diskleri (işletim sistemi ve veri) aynı depolama hesabında tutma**
2. Bir depolama hesabına daha fazla VHD eklemeden önce **Depolama hesabındaki yönetilmeyen disk sayısına ilişkin [limitleri](../articles/storage/common/storage-scalability-targets.md) gözden geçirin**
3. **Bir Kullanılabilirlik Kümesindeki her VM için ayrı depolama hesabı kullanın.** Depolama hesaplarını aynı Kullanılabilirlik Kümesinde birden fazla VM ile paylaşmayın. Farklı kullanılabilirlik en iyi yöntemler izlediyseniz depolama hesaplarında paylaşmak için kümeleri arasında VM'ler için kabul edilebilir ![diskleri FDs yönetilmeyen](./media/virtual-machines-common-manage-availability/umd-updated.png)

## <a name="configure-each-application-tier-into-separate-availability-sets"></a>Her uygulama katmanını ayrı kullanılabilirlik kümeleri halinde yapılandırma
Sanal makinelerinizin neredeyse tümü aynıysa ve uygulamanız için aynı amaca hizmet ediyorsa, uygulamanızın her katmanı için bir kullanılabilirlik kümesi yapılandırmanız önerilir.  Aynı kullanılabilirlik kümesine iki farklı katman yerleştirirseniz, aynı uygulama katmanındaki tüm sanal makineler tek seferde yeniden başlatılabilir. Her katman için bir kullanılabilirlik kümesinde en az iki sanal makineyi yapılandırarak, her katmanda en az bir sanal makinenin kullanılabilir olmasını garanti edersiniz.

Örneğin, tüm sanal makineleri tek bir kullanılabilirlik kümesinde IIS, Apache, Nginx çalıştıran uygulamanızın ön ucuna yerleştirebilirsiniz. Yalnızca ön uç sanal makinelerin aynı kullanılabilirlik kümesine yerleştirildiğinden emin olun. Benzer şekilde, çoğaltılmış SQL Server sanal makineleriniz ya da MySQL sanal makineleriniz gibi yalnızca veri katmanı sanal makinelerinin kendi kullanılabilirlik kümelerine yerleştirildiğinden emin olun.

<!--Image reference-->
   ![Uygulama katmanları](./media/virtual-machines-common-manage-availability/application-tiers.png)

## <a name="combine-a-load-balancer-with-availability-sets"></a>Yük dengeleyiciyi kullanılabilirlik kümeleri ile birleştirme
En fazla uygulama dayanıklılığını elde etmek için [Azure Load Balancer](../articles/load-balancer/load-balancer-overview.md)’ı bir kullanılabilirlik kümesiyle birleştirin. Azure Load Balancer, birden fazla sanal makine arasında trafiği dağıtır. Standart katman sanal makinelerimize Azure Load Balancer dahildir. Tüm sanal makine katmanları Azure Load Balancer hizmetini içermez. Sanal makinelerinizde yük dengeleme hakkında daha fazla bilgi için bkz. [Sanal makinelerde yük dengeleme](../articles/virtual-machines/virtual-machines-linux-load-balance.md).

Yük dengeleyici birden fazla sanal makine arasında trafiği dengeleyecek şekilde yapılandırılmamışsa, planlı bakım olayları yalnızca trafik sunan sanal makineyi etkiler ve uygulama katmanınızda kesintiye neden olur. Aynı yük dengeleyici ve kullanılabilirlik kümesi altına aynı katmandaki birden fazla sanal makinenin yerleştirilmesi, trafiğin en az bir örnek tarafından sürekli olarak sunulmasını sağlar.

## <a name="use-availability-zones-to-protect-from-datacenter-level-failures"></a>Veri merkezi düzeyi arızasına karşı korumak için kullanılabilirlik bölgelerini kullanın

[Kullanılabilirlik bölgeleri](../articles/availability-zones/az-overview.md) kullanılabilirlik için bir alternatif (Önizleme), ayarlar, uygulamaları ve verileri, vm'lerde kullanılabilirliğini sürdürmek zorunda denetim düzeyini genişletin. Kullanılabilirlik Alanı, bir Azure bölgesinin içinde fiziksel olarak ayrılmış bir alandır. Desteklenen bir Azure bölgesine başına üç kullanılabilirlik bölge vardır. Her kullanılabilirlik bölge farklı olan güç kaynağı, ağ ve soğutma ve diğer bölgelerden kullanılabilirlik Azure bölge içindeki mantıksal olarak farklıdır. Çoğaltılmış VM'ler bölgeleri kullanmak için çözüm mimarisi oluşturma, uygulamaları ve verileri bir veri merkezinde kaybına karşı koruyabilirsiniz. Bir bölge aşılırsa, ardından çoğaltılan uygulamaları ve verileri başka bir bölgede hemen kullanılabilir. 

![Kullanılabilirlik bölgeleri](./media/virtual-machines-common-regions-and-availability/three-zones-per-region.png)

[!INCLUDE [availability-zones-preview-statement.md](availability-zones-preview-statement.md)]

Dağıtma hakkında daha fazla bilgi bir [Windows](../articles/virtual-machines/windows/create-powershell-availability-zone.md) veya [Linux](../articles/virtual-machines/linux/create-cli-availability-zone.md) kullanılabilirlik bölgesinde VM.


<!-- Link references -->
[Bir kullanılabilirlik kümesindeki birden fazla sanal makineyi yedeklilik için yapılandırma]: #configure-multiple-virtual-machines-in-an-availability-set-for-redundancy
[Her uygulama katmanını ayrı kullanılabilirlik kümeleri halinde yapılandırma]: #configure-each-application-tier-into-separate-availability-sets
[Yük Dengeleyiciyi kullanılabilirlik kümeleri ile birleştirme]: #combine-a-load-balancer-with-availability-sets
[Avoid single instance virtual machines in availability sets]: #avoid-single-instance-virtual-machines-in-availability-sets
[Bir kullanılabilirlik kümesindeki VM’ler için yönetilen diskleri kullanma]: #use-managed-disks-for-vms-in-an-availability-set
[Veri merkezi düzeyi arızasına karşı korumak için kullanılabilirlik bölgelerini kullanın]: #use-availability-zones-to-protect-from-datacenter-level-failures
