Azure sanal makineleri (VM'ler) bazen yok, yeniden başlatma işlemi başlatılan, kanıt olmadan belirgin bir nedenle yeniden başlatılması. Bu makalede Eylemler ve yeniden başlatmak VM'ler neden olabilir ve beklenmeyen bir yeniden başlatma sorunları önlemenize veya bu tür sorunları etkisini azaltmak nasıl bir anlayış sağlayan olayları listelenmektedir.

## <a name="configure-the-vms-for-high-availability"></a>VM'ler yüksek kullanılabilirlik için yapılandırma
Bir VM'ye karşı Azure üzerinde çalışan bir uygulama korumak için en iyi yolu yeniden başlatır ve kapalı kalma süresi VM'ler yüksek kullanılabilirlik için yapılandırılır.

Bu düzeyde uygulamanıza yedeklilik sağlamak için bir kullanılabilirlik kümesinde iki veya daha fazla VM Grup öneririz. Bu yapılandırma ya da bir planlı veya plansız bir bakım olayı sırasında en az bir VM kullanılabilir 99,95 yüzde karşılayan olmasını sağlar ve [Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_5/).

Kullanılabilirlik kümeleri hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Sanal makineleri kullanılabilirliğini yönetme](../articles/virtual-machines/windows/manage-availability.md)
- [Sanal makineleri kullanılabilirliği yapılandırma](../articles/virtual-machines/windows/classic/configure-availability.md)

## <a name="resource-health-information"></a>Kaynak sistem durumu bilgileri 
Azure kaynak durumu Azure kaynakların durumunu gösterir ve sorun giderme için işlem yapılabilir Kılavuzu sağlayan bir hizmettir. Burada sunucular veya altyapı öğeleri doğrudan erişmesini mümkün değilse bulut ortamında, kaynak durumu sorunlarını gidermeyle ilgili harcadığı zamanı azaltmak üzere hedefidir. Özellikle, AIM sorun kök uygulama veya bir olayı Azure platformu içinde kaynaklandığını olup olmadığını belirleme harcadığı zamanı azaltmaktır. Daha fazla bilgi için bkz: [anlayın ve kullanım kaynak durumu](../articles/resource-health/resource-health-overview.md).

## <a name="actions-and-events-that-can-cause-the-vm-to-reboot"></a>Eylemleri ve yeniden başlatmak VM neden olan olayları

### <a name="planned-maintenance"></a>Planlı bakım
Microsoft Azure, güncelleştirmelerinin güvenilirliği, performansı ve VM'ler altını çizen konak altyapısının güvenliğini artırmak için dünya çapında düzenli olarak gerçekleştirir. Bellek koruma güncelleştirmeleri dahil olmak üzere bu güncelleştirmeler, birçoğu, sanal makineleri üzerinde hiçbir etkisi olmadan gerçekleştirilen veya Bulut Hizmetleri.

Ancak, bazı güncelleştirmeler yeniden başlatma gerektirir. Böyle durumlarda, sanal makineleri biz altyapı düzeltme eki ve ardından sanal makineleri yeniden kapatılır.

Hangi Azure planlanan Bakım ve Linux VM'ler kullanılabilirliğini nasıl etkileyebileceğini anlamak için burada listelenen makalelerine bakın. Makaleler Azure hakkında arka plan bakım işlemi ve daha fazla etkisini azaltmak için planlı bakım zamanlama planlanan sağlar.

- [Azure VM'ler için planlı bakım](../articles/virtual-machines/windows/planned-maintenance.md)
- [Azure vm'lerinde planlı bakım zamanlama](../articles/virtual-machines/windows/classic/planned-maintenance-schedule.md)

### <a name="memory-preserving-updates"></a>Bellek koruma güncelleştirmeleri   
Microsoft Azure güncelleştirmelerinde Bu sınıf için kullanıcılar çalışan Vm'leri üzerinde hiçbir etkisi yaşar. Bu güncelleştirmelerin çoğu, çalışan örneği engellemeden güncelleştirilebilecek bileşenlere veya hizmetler içindir. VM'ler başlatmadan uygulanabilir konak işletim sistemi platformu altyapı güncelleştirmeleri bazılarıdır.

Bu bellek koruma güncelleştirmeleri yerinde dinamik geçiş olanağını sunar teknolojisi ile yapılır. Güncelleştirilmekte, VM yerleştirilen bir *duraklatıldı* durumu. Bu durum, temel işletim sistemi gerekli güncelleştirmeleri ve düzeltme eklerini alırken RAM bellek korur. VM duraklatıldıktan sonra 30 saniye içinde devam ettirilir. VM çıktıktan sonra kendi saati otomatik olarak eşitlenir.

Kısa duraklatma süresi nedeniyle güncelleştirmeleri bu mekanizma büyük ölçüde dağıtma VM'ler üzerindeki etkisini azaltır. Ancak, güncelleştirmelerinin tümü yok, bu şekilde dağıtılabilir. 

Çok örnekli (bir kullanılabilirlik kümesinde VM'ler için) uygulanan bir güncelleştirme etki alanı aynı anda güncelleştirmelerdir.

> [!NOTE]
> Eski çekirdek sürümlerde Linux makineler, bu güncelleştirme yöntemi sırasında çekirdek Panik tarafından etkilenir. Bu sorunu önlemek için çekirdek sürüm 3.10.0-327.10.1 veya sonraki bir sürüme güncelleştirin. Daha fazla bilgi için bkz: [3.10 tabanlı çekirdek bir Azure Linux VM'de panics bir ana bilgisayar düğümü yükseltmeden sonra](https://support.microsoft.com/help/3212236).     
    
### <a name="user-initiated-reboot-or-shutdown-actions"></a>Kullanıcı tarafından başlatılan yeniden başlatma veya kapatma Eylemler
 
Azure portal, Azure PowerShell, komut satırı arabirimini veya sıfırlama API yeniden başlatma işlemi gerçekleştirirseniz, olayda bulabilirsiniz [Azure etkinlik günlüğü](../articles/monitoring-and-diagnostics/monitoring-overview-activity-logs.md).

Sanal makinenin işletim sisteminden eylem gerçekleştirirseniz, olay sistem günlüklerine bulabilirsiniz.

Genellikle VM'nin yeniden neden diğer senaryolar birden fazla yapılandırma değişikliği eylemleri içerir. Normalde, belirli bir eylemi yürütürken belirten bir uyarı iletisi VM bir yeniden başlatma sonuçlanır görürsünüz. Yönetici hesabının parolasını değiştirme ve bir statik IP adresi ayarı VM yeniden boyutlandırma işlemleri örnekler.

### <a name="azure-security-center-and-windows-update"></a>Azure Güvenlik Merkezi ve Windows Update
Azure Güvenlik Merkezi, işletim sistemi güncelleştirmeleri eksik günlük Windows ve Linux VM'ler izler. Güvenlik Merkezi hizmeti bağlı olarak bir Windows VM üzerinde yapılandırıldığı Windows Update veya Windows Server Update Services (WSUS) kullanılabilir güvenlik ve kritik güncelleştirmeler listesini alır. Güvenlik Merkezi, ayrıca Linux sistemler için en son güncelleştirmeleri denetler. VM'yi bir sistem güncelleştirmesi eksikse, Güvenlik Merkezi sistem güncelleştirmeleri uygulamanızı önerir. Bu sistem güncelleştirmelerin uygulanması, Azure portalında Güvenlik Merkezi aracılığıyla denetlenir. Bazı güncelleştirmeler uygulandıktan sonra VM yeniden başlatma gerekli olabilir. Daha fazla bilgi için bkz: [Azure Güvenlik Merkezi'nde sistem güncelleştirmeleri uygulamak](../articles/security-center/security-center-apply-system-updates.md).

Bu makineler, kullanıcılar tarafından yönetilmesine yönelik olduğundan gibi şirket içi sunucular, Azure güncelleştirmeleri Windows Update'ten Windows Azure VM'ler için göndermez. Ancak, otomatik Windows Update ayar etkin olarak bırakmak için teşvik. Windows Update'ten güncelleştirmeleri otomatik olarak yüklenmesini de güncelleştirmeleri uygulandıktan sonra gerçekleşecek şekilde yeniden başlatmalar neden olabilir. Daha fazla bilgi için bkz: [Windows Update SSS](https://support.microsoft.com/help/12373/windows-update-faq).

### <a name="other-situations-affecting-the-availability-of-your-vm"></a>VM kullanılabilirliğini etkileyen diğer durumlar
Hangi Azure etkin olarak bir VM kullanımını askıya diğer durumlar vardır. Bu eylem önce temel sorunları gidermek için bir fırsat gerekir böylece e-posta bildirimi alırsınız. VM kullanılabilirliği etkileyen sorunlar örnekleri güvenlik ihlalleri ve sona erme tarihini ödeme yöntemleri içerir.

### <a name="host-server-faults"></a>Ana bilgisayar sunucu hataları 
VM, Azure veri merkezi içinde çalıştıran fiziksel bir sunucuda barındırılır. Fiziksel sunucu birkaç Azure bileşenlere ek konak Aracısı denilen ve aracıya çalışır. Bu Azure yazılım bileşenleri fiziksel sunucuda yanıt veremez duruma geldiğinde izleme sistemi yeniden başlatma kurtarma girişiminde ana bilgisayar sunucusunun tetikler. VM beş dakika içinde yeniden genellikle kullanılabilir ve daha önce olarak aynı ana bilgisayardaki canlı devam eder.

Sunucu hatalarının genellikle bir sabit disk veya katı hal sürücüsü gibi donanım hatası nedeniyle oluşur. Azure sürekli olarak bu örnekleri izler, temel alınan hataları tanımlar ve azaltma uygulanan ve test sonra güncelleştirmeleri yapar.

Bazı ana bilgisayar sunucu hatalarının bu sunucuya özgü olabileceğinden, yinelenen bir VM yeniden başlatma durumu el ile başka bir ana bilgisayar sunucusuna VM dağıtarak tarafından geliştirilmiş. Bu işlemi kullanarak tetiklenebilir **dağıtmanız** VM veya VM Azure portalında durdurup tarafından seçeneği Ayrıntıları sayfasında.

### <a name="auto-recovery"></a>Otomatik Kurtarma
Ana bilgisayar sunucusu için herhangi bir nedenle yeniden olamaz, Azure platformu daha fazla araştırma için döndürme dışında hatalı ana bilgisayar sunucusu almak için bir otomatik kurtarma eylemi başlatır. 

Bu konaktaki tüm sanal makineleri farklı, sağlıklı ana bilgisayar sunucusuna otomatik olarak yerleştirilir. Bu işlem 15 dakika içinde genellikle tamamlanır. Otomatik Kurtarma işlemi hakkında daha fazla bilgi için bkz: [otomatik kurtarma VM'lerin](https://azure.microsoft.com/blog/service-healing-auto-recovery-of-virtual-machines).

### <a name="unplanned-maintenance"></a>Plansız bakım
Nadir durumlarda, Azure işlemleri takım Azure platformu genel durumunu emin olmak için bakım etkinlikler gerçekleştirmeniz gerekebilir. Bu davranış VM kullanılabilirliğini etkileyebilir ve bu genellikle aynı otomatik kurtarma eylemi daha önce açıklandığı gibi sonuçlanır.  

Planlanmamış maintenances aşağıdakileri içerir:

- Acil düğümü birleştirme
- Acil ağ anahtarı güncelleştirmeleri

### <a name="vm-crashes"></a>VM çökme (Crash)
VM'ler, VM dahilinde sorunları nedeniyle yeniden başlatılabilir. İş yükü veya VM'de çalışan rolü konuk işletim sistemi içinde bir hata denetimi tetikleyebilir. Kilitlenme nedeni belirleme konusunda yardım için sistem ve uygulama günlüklerini Windows VM'ler için ve Linux VM'ler için seri günlüklerini görüntüleyin.

### <a name="storage-related-forced-shutdowns"></a>Depolama ilgili zorla kapatma
Azure'da Vm'leri sanal diskler işletim sistemi ve Azure depolama altyapısını barındırılan veri depolama için kullanır. Kullanılabilirlik veya VM ile ilişkili sanal diskler arasında bağlantı 120 saniyeden fazla bir süre için etkilenen her Azure platformu veri bozulmasını önlemek için VM'lerin zorlanmış kapatma gerçekleştirir. Depolama bağlantı geri yüklendikten sonra sanal makineleri otomatik olarak geri güç sağlar. 

Kapatma süresi beş dakikadan kısa olabilir ancak önemli ölçüde uzun olabilir. Depolama ilgili zorla kapatma ile ilişkilendirilmiş belirli durumlarda biri şudur: 

**G/ç aşan sınırlar**

Birimi g/ç işlemlerinin (IOPS) saniyede disk g/ç sınırı aştığından g/ç istekleri tutarlı bir şekilde kısıtlanan VM'ler geçici olarak kapatıldığında. (Standart disk depolama alanını 500 IOPS sınırlıdır.) Bu sorunu azaltmak için disk şeritleme kullanın veya depolama alanı iş yüküne bağlı olarak VM Konuk içinde yapılandırın. Ayrıntılar için bkz [yapılandırma Azure VM'ler için en iyi depolama performansı](http://blogs.msdn.com/b/mast/archive/2014/10/14/configuring-azure-virtual-machines-for-optimal-storage-performance.aspx).

Daha yüksek IOPS sınırları Azure Premium Storage 80.000 en fazla IOPS ile aracılığıyla kullanılabilir. Daha fazla bilgi için bkz: [yüksek performanslı Premium depolama](../articles/virtual-machines/windows/premium-storage.md).

### <a name="other-incidents"></a>Diğer olaylar
Nadir durumlarda, Azure veri merkezinde birden çok sunucuya yaygın sorun etkileyebilir. Bu sorun oluşursa, Azure ekibi etkilenen abonelikleri e-posta bildirimleri gönderir. Kontrol edebilirsiniz [Azure hizmet durumu Panosu](https://azure.microsoft.com/status/) ve devam eden kesintiler ve olaylar geçmiş durumunu için Azure portalı.
