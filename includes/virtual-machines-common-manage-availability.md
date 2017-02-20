## <a name="understand-planned-vs-unplanned-maintenance"></a>Planlı ve plansız bakımı anlama
Sanal makinelerinizin kullanılabilirliğini etkileyebilecek iki tür Microsoft Azure platform olayı vardır: planlı bakım ve plansız bakım.

* **Planlı bakım olayları**, Microsoft tarafından sanal makinenizin çalıştığı platforma ait genel güvenilirlik, performans ve güvenliği artırmak amacıyla temel alınan Azure platformunda yapılan periyodik güncelleştirmelerdir. Bu güncelleştirmelerin çoğu sanal makine veya bulut hizmetlerinizi etkilemeden gerçekleştirilir. Ancak, gerekli güncelleştirmelerin platform altyapısına uygulanması için bu güncelleştirmelerin sanal makinenizi yeniden başlatmayı gerektirdiği örnekler vardır.
* **Plansız bakım olayları**, sanal makinenizin altında yatan donanım ya da fiziksel altyapı bir şekilde arıza yaptığında gerçekleştirilir. Buna yerel ağ hataları, yerel disk hataları veya raf düzeyinde diğer hatalar dahildir. Böyle bir hata algılandığında, Azure platformu sanal makinenizi barındırıldığı sağlıksız fiziksel makineden sağlıklı bir fiziksel makineye otomatik olarak geçirir. Bu tür olaylar nadirdir, ancak sanal makinenizin yeniden başlatılmasına da neden olabilir.

Bu olayların bir veya daha fazlası nedeniyle kapalı kalma süresinin etkisini azaltmak için, sanal makinelerinizde aşağıdaki yüksek kullanılabilirlik en iyi uygulamalarının kullanılması önerilir:

* [Bir kullanılabilirlik kümesindeki birden fazla sanal makineyi yedeklilik için yapılandırma]
* [Her uygulama katmanını ayrı kullanılabilirlik kümeleri halinde yapılandırma]
* [Yük Dengeleyiciyi kullanılabilirlik kümeleri ile birleştirme]
* [Her kullanılabilirlik kümesi için birden fazla depolama hesabı kullanma]

## <a name="configure-multiple-virtual-machines-in-an-availability-set-for-redundancy"></a>Bir kullanılabilirlik kümesindeki birden fazla sanal makineyi yedeklilik için yapılandırma
Uygulamanıza yedeklilik sağlamak için bir kullanılabilirlik kümesinde iki veya daha fazla sanal makinenin gruplandırılması önerilir. Bu yapılandırma, planlı veya plansız bakım olayı sırasında en az bir sanal makinenin kullanılabilir olmasını ve %99,95 Azure SLA hedefini karşılamasını sağlar. Daha fazla bilgi için bkz. [Sanal Makineler için SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/).

> [!IMPORTANT]
> Tek örnekli bir sanal makineyi bir kullanılabilirlik kümesinde tek başına bırakmaktan kaçının. Bu yapılandırmadaki sanal makineler SLA garantisine uygun değildir ve tek bir VM’nin [Azure Premium Depolama](../articles/storage/storage-premium-storage.md) kullandığı durumlar dışında planlı Azure bakım olayları sırasında kapalı kalma süresiyle karşılaşır. Premium depolama kullanan tek VM'ler için Azure SLA geçerlidir.

Kullanılabilirlik kümenizdeki her sanal makineye, temel alınan Azure platformu tarafından bir **güncelleme etki alanı** ve bir **hata etki alanı** atanır. Belirli bir kullanılabilirlik kümesi için, aynı anda yeniden başlatılabilecek sanal makine gruplarını ve temel alınan fiziksel donanımları göstermek üzere, kullanıcı tarafından yapılandırılabilen beş güncelleme etki alanı varsayılan olarak atanır (Resource Manager dağıtımları daha sonra en fazla 20 güncelleme etki alanı sağlayacak şekilde artırılabilir). Tek bir kullanılabilirlik kümesinde beşten fazla sanal makine yapılandırıldığında, altıncı sanal makine birinci sanal makine ile, yedinci sanal makine ikinci sanal makine ile aynı güncelleme etki alanına yerleştirilir ve yerleştirme işlemi bu düzende devam eder. Yeniden başlatılmakta olan güncelleme etki alanlarının sırası, planlanan bakım sırasında sıralı olarak uygulanmayabilir, ancak aynı anda yalnızca bir güncelleme etki alanı yeniden başlatılır.

Hata etki alanları ortak bir güç kaynağı ve ağ anahtarını paylaşan sanal makine grubunu tanımlar. Varsayılan olarak, kullanılabilirlik kümenizde yapılandırılmış olan sanal makineler, Resource Manager dağıtımları için en fazla üç hata etki alanı (Klasik için iki etki alanı) arasında ayrılır. Sanal makinelerinizin bir kullanılabilirlik kümesine yerleştirilmesi uygulamanızı işletim sistemine veya uygulamaya özel hatalardan korumasa da, olası fiziksel donanım hatalarının, ağ kesintilerinin veya güç kesintilerinin etkilerini sınırlar.

<!--Image reference-->
   ![Güncelleme etki alanı ve hata etki alanı yapılandırmasının kavramsal çizimi](./media/virtual-machines-common-manage-availability/ud-fd-configuration.png)

### <a name="managed-disk-fault-domains-and-availability-sets"></a>Yönetilen Disk hata etki alanları ve kullanılabilirlik kümeleri
[Azure Yönetilen Diskler](../articles/storage/storage-faq-for-disks.md)’i kullanan sanal makineler, yönetilen kullanılabilirlik kümesi kullanılırken yönetilen disk hata etki alanları ile hizalanır. Bu hizalama, bir VM'ye bağlı tüm yönetilen disklerin, aynı yönetilen disk hata etki alanı içinde olmasını sağlar. Yönetilen bir kullanılabilirlik kümesinde yalnızca, yönetilen disklere sahip VM’ler oluşturulabilir. Yönetilen disk hata etki alanlarının sayısı bölgeye göre farklılık gösterir (bölge başına iki ya da üç yönetilen disk hata etki alanı).


## <a name="configure-each-application-tier-into-separate-availability-sets"></a>Her uygulama katmanını ayrı kullanılabilirlik kümeleri halinde yapılandırma
Sanal makinelerinizin neredeyse tümü aynıysa ve uygulamanız için aynı amaca hizmet ediyorsa, uygulamanızın her katmanı için bir kullanılabilirlik kümesi yapılandırmanız önerilir.  Aynı kullanılabilirlik kümesine iki farklı katman yerleştirirseniz, aynı uygulama katmanındaki tüm sanal makineler tek seferde yeniden başlatılabilir. Her katman için bir kullanılabilirlik kümesinde en az iki sanal makineyi yapılandırarak, her katmanda en az bir sanal makinenin kullanılabilir olmasını garanti edersiniz.

Örneğin, tüm sanal makineleri tek bir kullanılabilirlik kümesinde IIS, Apache, Nginx çalıştıran uygulamanızın ön ucuna yerleştirebilirsiniz. Yalnızca ön uç sanal makinelerin aynı kullanılabilirlik kümesine yerleştirildiğinden emin olun. Benzer şekilde, çoğaltılmış SQL Server sanal makineleriniz ya da MySQL sanal makineleriniz gibi yalnızca veri katmanı sanal makinelerinin kendi kullanılabilirlik kümelerine yerleştirildiğinden emin olun.

<!--Image reference-->
   ![Uygulama katmanları](./media/virtual-machines-common-manage-availability/application-tiers.png)

## <a name="combine-a-load-balancer-with-availability-sets"></a>Yük dengeleyiciyi kullanılabilirlik kümeleri ile birleştirme
En fazla uygulama dayanıklılığını elde etmek için [Azure Load Balancer](../articles/load-balancer/load-balancer-overview.md)’ı bir kullanılabilirlik kümesiyle birleştirin. Azure Load Balancer, birden fazla sanal makine arasında trafiği dağıtır. Standart katman sanal makinelerimize Azure Load Balancer dahildir. Tüm sanal makine katmanları Azure Load Balancer hizmetini içermez. Sanal makinelerinizde yük dengeleme hakkında daha fazla bilgi için bkz. [Sanal makinelerde yük dengeleme](../articles/virtual-machines/virtual-machines-linux-load-balance.md).

Yük dengeleyici birden fazla sanal makine arasında trafiği dengeleyecek şekilde yapılandırılmamışsa, planlı bakım olayları yalnızca trafik sunan sanal makineyi etkiler ve uygulama katmanınızda kesintiye neden olur. Aynı yük dengeleyici ve kullanılabilirlik kümesi altına aynı katmandaki birden fazla sanal makinenin yerleştirilmesi, trafiğin en az bir örnek tarafından sürekli olarak sunulmasını sağlar.

## <a name="use-multiple-storage-accounts-for-each-availability-set"></a>Her kullanılabilirlik kümesi için birden fazla depolama hesabı kullanma
Azure Yönetilen Diskleri kullanıyorsanız aşağıdaki yönergeleri atlayabilirsiniz. Diskler VM kullanılabilirlik kümelerinizle hizalanan hata etki alanlarına depolandığı için, Azure Yönetilen Diskler kendiliğinden yüksek kullanılabilirlik ve yedeklilik sağlar. Daha fazla bilgi için bkz. [Azure Yönetilen Disklere Genel Bakış](../articles/storage/storage-managed-disks-overview.md).

Yönetilmeyen diskler kullanıyorsanız, sanal makine içinde Sanal Sabit Diskler (VHD) tarafından kullanılan depolama hesaplarıyla ilgili izlenmesi gereken en iyi uygulamalar mevcuttur. Her disk (VHD) bir Azure Depolama hesabındaki sayfa blobudur. Kullanılabilirlik Kümesindeki VM’lerde yüksek kullanılabilirlik sağlamak için, depolama hesaplarında yedeklilik ve yalıtım olduğundan emin olun.

1. **Bir VM ile ilişkili tüm diskleri (işletim sistemi ve veri) aynı depolama hesabında tutma**
2. Bir depolama hesabına başka VHD’ler eklenirken **depolama hesabı [sınırları](../articles/storage/storage-scalability-targets.md) göz önünde bulundurulmalıdır**
3. **Bir Kullanılabilirlik Kümesindeki her VM için ayrı depolama hesabı kullanın.** Aynı kullanılabilirlik kümesindeki birden fazla VM, depolama hesaplarını PAYLAŞMAMALIDIR. Önceki en iyi uygulamalar izlendiği sürece, farklı Kullanılabilirlik Kümelerindeki VM’ler depolama hesaplarını paylaşabilir

<!-- Link references -->
[Bir kullanılabilirlik kümesindeki birden fazla sanal makineyi yedeklilik için yapılandırma]: #configure-multiple-virtual-machines-in-an-availability-set-for-redundancy
[Her uygulama katmanını ayrı kullanılabilirlik kümeleri halinde yapılandırma]: #configure-each-application-tier-into-separate-availability-sets
[Yük Dengeleyiciyi kullanılabilirlik kümeleri ile birleştirme]: #combine-a-load-balancer-with-availability-sets
[Avoid single instance virtual machines in availability sets]: #avoid-single-instance-virtual-machines-in-availability-sets
[Her kullanılabilirlik kümesi için birden fazla depolama hesabı kullanma]: #use-multiple-storage-accounts-for-each-availability-set



<!--HONumber=Feb17_HO2-->


