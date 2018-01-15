

## <a name="what-is-happening"></a>Ne oluyor?

Sektör çapında, donanım tabanlı güvenlik açığı olan [3 Ocak'ta duyurulmuş](https://googleprojectzero.blogspot.com/2018/01/reading-privileged-memory-with-side.html). Müşteriler tutulması bizim en önemli öncelik her zaman güvenli ve biz hiçbir Azure müşteri bu açıklarına maruz emin olmak için etkin adımları sürüyor.

Güvenlik açığının ortak açığa çıkması ile biz [planlı bakım zamanlaması hızlandırılmış](https://azure.microsoft.com/blog/securing-azure-customers-from-cpu-vulnerability/) ve hala güncelleştirme gereken sanal makineleri otomatik olarak yeniden başlatılmasını başladı.
 
## <a name="how-can-i-see-which-of-my-vms-are-already-updated"></a>Nasıl ı görebilir Vm'lerimin hangisinin zaten güncelleştirildi mi? 

Vm'leriniz, durumunu görebilirsiniz ve yeniden başlatma tamamlandı, buna [VM listesi Azure portalında](https://aka.ms/T08tdc). Güncelleştirme uygulanan veya "Güncelleştirme hala gerekliyse zamanlanmış" varsa, sanal makineleri "Zaten güncelleştirme" ya da listelenir. "Zamanlanmış" başvurmak için VM'lerin görmek istiyorsanız, [Azure hizmet durumu](https://portal.azure.com/).

## <a name="can-i-find-out-exactly-when-my-vms-will-be-rebooted"></a>Tam olarak ne zaman Vm'lerimin yeniden başlatılacak çıkışı bulabilirim?

Yeniden başlatma hakkında uyarı almak için en iyi yolu yapılandırmaktır [zamanlanmış olayları](https://docs.microsoft.com/azure/virtual-machines/windows/scheduled-events). Bu, 15 dakika bildirim VM bakım nedeniyle giderek sağlar.

## <a name="can-i-manually-redeploy-now-to-perform-the-required-maintenance"></a>I el ile artık gerekli bakım gerçekleştirmek için yeniden dağıtabilirsiniz? 

Güncelleştirilmiş bir ana bilgisayara yeniden dağıtılan VM ayrılır garanti edemez. Mümkün olduğunda, Azure dokusunu zaten güncelleştirildi konaklar için sanal makineleri tahsis dener. Bir VM dağıtarak güncelleştirilmiş olmayan bir konakta güden, bu durumda, zamanlanmış bakım bir parçası olarak zorlanmış ikinci bir yeniden başlatma tabi olabilir mümkündür. Sonuç olarak, el ile yeniden dağıtır geçici bir çözüm olarak önerilmez.

## <a name="how-long-will-the-reboot-take"></a>Yeniden başlatma ne kadar sürer? 

Çoğu yeniden başlatmalar yaklaşık kaplayan **30 dakika**.

## <a name="does-the-guest-os-need-to-be-updated"></a>Konuk işletim sistemi güncellenmesi gerekiyor mu? 

Bu Azure Altyapı güncelleştirme hiper yönetici düzeyinde bildirilen açığına ve Windows veya Linux VM görüntüleri için bir güncelleştirme gerektirmez. Ancak, her zaman olarak VM görüntüleri için en iyi güvenlik uygulamalarını uygulamak devam. Lütfen, işletim sistemlerinin güncelleştirmeleri ve yönergeler için satıcı ile gerektiğinde bakın. Windows Server VM müşteriler için yönerge şimdi yayımlanan ve kullanılabilir [burada](../articles/virtual-machines/windows/mitigate-se.md).

## <a name="will-there-be-a-performance-impact-as-a-result-of-resolving-this-update"></a>Bu güncelleştirme Çözümleme sonucunda bir performans etkisi vardır olacak?

Azure müşterilerin çoğu değil belirgin performans etkisi Bu güncelleştirme ile görülür. Biz CPU ve disk g/ç yolu en iyi duruma getirme çalıştığınız ve düzeltme uygulandıktan sonraki fark edilebilir performans etkisi görmediğinizden. Müşteriler, küçük bir kümesini bazı ağ performansına etkisi karşılaşabilirsiniz. Bu hızlandırılmış Azure ağı kullanarak çözülebilir [Windows](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-powershell) veya [Linux](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli), tüm Azure müşterilerine kullanılabilir boş bir özellik değil.

## <a name="i-follow-your-recommendations-for-high-availability-will-my-environment-remain-highly-available-during-the-reboot"></a>Yüksek kullanılabilirlik için önerilerinizi izleyin, my ortamını başlatma işlemi sırasında yüksek oranda kullanılabilir kalacak?

Evet, bir kullanılabilirlik dağıtılan sanal makineleri ayarlama veya sanal makine ölçek kümeleri güncelleştirme etki alanları (UD) yapısına sahip. Bakımı gerçekleştirirken Azure UD kısıtlaması geliştirir ve sanal makinelerden farklı UD (içinde aynı kullanılabilirlik kümesinde) yeniden değil. Yüksek kullanılabilirlik hakkında daha fazla bilgi için bkz [azure'da Windows sanal makinelerin kullanılabilirliğini yönetme](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability) veya [azure'daki Linux sanal makinelerin kullanılabilirliğini yönetme](https://docs.microsoft.com/azure/virtual-machines/linux/manage-availability).

## <a name="i-have-architected-my-business-continuitydisaster-recovery-plan-using-region-pairs-will-reboots-to-my-vms-occur-in-region-pairs-at-the-same-time"></a>İş sürekliliği/olağanüstü durum kurtarma planı bölge çiftleri kullanılarak tasarlanmış. Yeniden başlatma Vm'lerimin için aynı anda bölge çiftler halinde meydana gelir?

Normalde, Azure planlı bakım etkinliği eşleştirilmiş bölgeler bir hem bölgelerde bozulma riskini en aza indirmek için bir zaman alınır. Ancak, bu güvenlik güncelleştirmesi Acil yapısı nedeniyle, biz güncelleştirme tüm bölgeler için aynı anda geri.

## <a name="what-about-paas-services-on-azure"></a>Azure'da PaaS hizmetler hakkında neler?  

Web ve mobil, IOT, data services de dahil olmak üzere Azure platform hizmetlerini sunucusuz, vb. ele güvenlik açığı. Bu hizmetleri kullanan müşteriler için gereken eylemi yok.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz: [CPU güvenlik açığı güvenliğini sağlama Azure müşterilerden](https://azure.microsoft.com/blog/securing-azure-customers-from-cpu-vulnerability/).
