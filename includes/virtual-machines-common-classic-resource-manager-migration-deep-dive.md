## <a name="migrate-iaas-resources-from-the-classic-deployment-model-to-azure-resource-manager"></a>Iaas kaynaklarını Klasik dağıtım modelinden Azure Resource Manager geçirme
İlk olarak, hizmet (Iaas) kaynaklar olarak altyapı veri düzlemi ve Yönetim düzeyi işlemleri arasındaki farkı anlamak önemlidir.

* *Yönetim/denetim düzlemi* Yönetimi/denetim düzlemi veya kaynakları değiştirme API uygulamasına gelen çağrıları açıklar. Örneğin, VM oluşturma, bir sanal makineyi yeniden başlatma ve çalışmakta olan kaynakları yönetmek için bir sanal ağı yeni alt ağla güncelleştirme gibi işlemler. Bunlar Vm'lere bağlanması doğrudan etkilemez.
* *Veri düzlemi* (uygulama) uygulamanın çalışma zamanı açıklar ve Azure API aracılığıyla geçmez, örnekleri ile etkileşimi içerir. Örneğin, Web sitenizi erişimi veya çalışan bir SQL Server örneği ya da bir MongoDB sunucudan veri çekme verilerdir düzlemi veya uygulama etkileşimler. Bir blob depolama hesabından kopyalama ve sanal makinede Uzak Masaüstü Protokolü (RDP) veya güvenli Kabuk (SSH) kullanmak için bir ortak IP adresi erişme diğer örnekleri içerir. Bu işlemler, uygulamanın işlem, ağ ve depolama kaynaklarında çalışmaya devam etmesini sağlar.

Veri düzlemi Klasik dağıtım modeli ve Resource Manager yığınları arasında aynıdır. Geçiş işlemi sırasında Microsoft kaynaklardan Klasik dağıtım modeli, kaynak yöneticisi yığınında gösterimini çevirir farktır. Sonuç olarak, kaynak yöneticisi yığınında kaynaklarınızı yönetmek için yeni araçlar, API'ler ve SDK'ları kullanmanız gerekebilir.

![Yönetim/denetim düzlemi ve veri düzlemi arasındaki farkı gösteren diyagram](../articles/virtual-machines/media/virtual-machines-windows-migration-classic-resource-manager/data-control-plane.png)


> [!NOTE]
> Bazı geçiş senaryolarında, Azure platformu sanal makinenizi durdurur, serbest bırakır ve yeniden başlatır. Bu kısa bir veri düzlemi kapalı kalma neden olur.
>

## <a name="the-migration-experience"></a>Geçiş deneyimi
Geçişe başlamadan önce:

* Geçirmek istediğiniz kaynakların desteklenmeyen özellikleri veya yapılandırmaları olmadığından emin olun. Bu sorunlar genellikle platform tarafından algılanır ve bir hata oluşturulur.
* Bir sanal ağda olmayan VM'ler varsa, bunlar durduruldu ve hazırlama işleminin bir parçası olarak serbest bırakıldı. Genel IP adresi kaybetmek istemiyorsanız, hazırlama işlemi tetiklemeden önce IP adresi ayırma göz önünde bulundurun. Sanal makineleri bir sanal ağ varsa, bunlar durduruldu serbest ve değil.
* Geçiş sırasında gerçekleşebilecek beklenmeyen hataları göz önünde bulundurarak geçişinizi çalışma saatleri dışına gerçekleşecek şekilde planlayın.
* Hazırlama adımı tamamlandıktan sonra doğrulamayı kolaylaştırmak için PowerShell’i, komut satırı arabirimi (CLI) komutlarını veya REST API’leri kullanarak VM’lerinizin geçerli yapılandırmasını indirin.
* Geçişe başlamadan önce Resource Manager dağıtım modeli işlemek için Otomasyonu ve operationalization komut dosyalarınızı güncelleştirin. İsteğe bağlı olarak, kaynaklar hazırlanmış durumdayken GET işlemleri gerçekleştirebilirsiniz.
* Iaas kaynaklarına Klasik dağıtım modelinde yapılandırılır ve geçiş tamamlandıktan sonra planlama rol tabanlı erişim denetimi (RBAC) İlkeleri değerlendirin.

Geçiş iş akışı aşağıdaki gibidir:

![Geçiş iş akışını gösteren diyagram](../articles/virtual-machines/windows/media/migration-classic-resource-manager/migration-workflow.png)

> [!NOTE]
> Aşağıdaki bölümlerde açıklanan tüm ıdempotent işlemleridir. Desteklenmeyen bir özellik veya bir yapılandırma hatası başka bir sorun varsa, hazırlama yeniden, veya tamamlanmaya işlemi. Azure eylemi yeniden dener.
>
>

### <a name="validate"></a>Doğrulama
Doğrulama işlemi, geçiş sürecinin ilk adımıdır. Bu adımın amacı, Klasik dağıtım modelinde geçirmek istediğiniz kaynakların durumunu analiz etmektir. İşlem kaynakları (başarılı veya başarısız) geçişini yeteneğine sahip olup olmadığını değerlendirir.

(Sanal bir ağa değilse) sanal ağ veya bir bulut hizmeti seçin, geçiş için doğrulamak istediğiniz. Kaynağın geçiş uyumlu değilse, Azure nedenleri neden listeler.

#### <a name="checks-not-done-in-the-validate-operation"></a>Doğrulama işlemi yapılmadı denetler

Doğrulama işlemi, yalnızca klasik dağıtım modelinde kaynakların durumunu analiz eder. Tüm hataları ve desteklenmeyen senaryolar Klasik dağıtım modelinde çeşitli yapılandırmaları nedeniyle kontrol. Azure Kaynak Yöneticisi yığınında kaynaklardaki geçiş sırasında zorunlu tuttukları tüm sorunları olup olmadığını denetlemek mümkün değildir. Geçiş (Hazırlama işlemi) bir sonraki adımda dönüştürme kaynakları uygulanabilecek olduğunda bu sorunlar yalnızca denetlenir. Aşağıdaki tabloda doğrulama işlemi denetlenmedi tüm sorunları listeler:


|Doğrulama işleminde ağ denetimleri|
|-|
|ER ve VPN ağ geçitleri sahip bir sanal ağ.|
|Bir sanal ağ ağ geçidi bağlantı kesildi durumunda.|
|Tüm ER bağlantı hatları Azure Kaynak Yöneticisi yığını tarafından önceden geçirilir.|
|Ağ kaynakları için Azure Kaynak Yöneticisi kota denetler. Örneğin: statik genel IP, dinamik genel IP'ler, yük dengeleyici, ağ güvenlik grupları, yol tablolarını ve ağ arabirimleri. |
| Tüm yük dengeleyici kuralları, dağıtım ve sanal ağ arasında geçerli değil. |
| Aynı sanal ağda Dur serbest VM'ler arasında çakışan özel IP. |

### <a name="prepare"></a>Hazırlama
Hazırlama işlemi, geçiş sürecinin ikinci adımıdır. Bu adımın amacı, Iaas kaynaklarına Klasik dağıtım modelinden Resource Manager kaynaklarını dönüşümü benzetimini yapmaktır. Ayrıca, hazırlama işlemi bu yan yana görselleştirmek size gösterir.

> [!NOTE] 
> Kaynaklarınızın Klasik dağıtım modelinde, bu adım sırasında değiştirilmez. Geçiş çalışıyorsanız çalıştırmak için güvenli bir adımdır. 

(Bir sanal ağ değilse), sanal ağ veya Bulut hizmeti geçişe hazırlamak istediğinizi seçin.

* Kaynağın geçiş uyumlu değilse, Azure geçiş işlemi durdurur ve hazırlama işlemi neden başarısız neden listeler.
* Kaynak geçişini yeteneğine sahipse, geçiş kaynaklarınıza Yönetim düzeyi işlemlerinde aşağı Azure kilitler. Örneğin, geçirilmekte olan bir VM’ye veri diski ekleyemezsiniz.

Azure sonra meta veri geçişini Klasik dağıtım modelinden Resource Manager geçirme kaynaklarını başlatır.

Hazırlama işlemi tamamlandıktan sonra Klasik dağıtım modeli ve Resource Manager kaynakları görselleştirme seçeneğiniz vardır. Azure platformu, klasik dağıtım modelindeki her bulut hizmeti için `cloud-service-name>-Migrated` deseninde bir kaynak grubu adı oluşturur.

> [!NOTE]
> Geçirilen kaynakları için oluşturulan bir kaynak grubu adını seçmek mümkün değildir (diğer bir deyişle, "-geçişi"). Ancak, geçiş işlemi tamamlandıktan sonra kaynakları istediğiniz herhangi bir kaynak grubuna taşımak için Azure Kaynak Yöneticisi'nin taşıma özelliğini kullanabilirsiniz. Daha fazla bilgi için bkz. [Kaynakları yeni kaynak grubuna veya aboneliğe taşıma](../articles/resource-group-move-resources.md).

Aşağıdaki iki ekran görüntüleri bir yürütmeye hazırladıktan sonra işlem sonucu gösterir. Birinci özgün bulut hizmeti içeren bir kaynak grubu gösterir. İkinci yeni gösterir "-geçişi" eşdeğer Azure Resource Manager kaynaklarını içeren kaynak grubu.

![Özgün bulut hizmeti gösteren ekran görüntüsü](../articles/virtual-machines/windows/media/migration-classic-resource-manager/portal-classic.png)

![Azure Resource Manager kaynaklarını Hazırlama işlemi gösteren ekran görüntüsü](../articles/virtual-machines/windows/media/migration-classic-resource-manager/portal-arm.png)

Hazırlık aşaması tamamlandıktan sonra kaynaklarınızı Perde Arkası göz aşağıdadır. Veri düzlemi kaynağında aynı olduğunu unutmayın. (Klasik dağıtım modeli) yönetim düzeyi ve denetim düzlemi (Resource Manager) gösterilir.

![Hazırlık aşaması diyagramı](../articles/virtual-machines/windows/media/migration-classic-resource-manager/behind-the-scenes-prepare.png)

> [!NOTE]
> Klasik dağıtım modelinde bir sanal ağ içinde olmayan VM'ler durdurulur ve bu geçiş aşamasında serbest bırakıldı.
>

### <a name="check-manual-or-scripted"></a>Denetim (el ile veya betikle)
Onay adımda geçiş doğru görünüyorsa doğrulamak için daha önce indirdiğiniz yapılandırma kullanma seçeneğiniz vardır. Alternatif olarak, portal ve nokta onay özelliklerini ve kaynaklarını meta veri geçiş iyi göründüğünü doğrulamak için oturum açabilirsiniz.

Sanal ağ geçişi yapıyorsanız, sanal makinelerin çoğu yapılandırması yeniden başlatılmaz. Bu vm'lerde uygulamalar için uygulama hala çalıştığını doğrulayabilirsiniz.

Sanal makinelerin beklendiği gibi çalıştığını ve güncelleştirilmiş komut dosyalarınızı düzgün çalışması görmek için izleme ve işletimsel komut dosyalarınızı test edebilirsiniz. Kaynaklar hazırlanmış durumdayken yalnızca GET işlemleri desteklenir.

Önce geçiş yürütmek gereken süreyi kümesi penceresi yok. Bu durumdayken dilediğiniz kadar bekleyebilirsiniz. Bununla birlikte, bu kaynaklar için geçişi durdurana veya işleyene kadar yönetim düzlemi kilitli kalır.

Herhangi bir sorun yaşarsanız dilediğiniz zaman geçişi durdurabilir ve klasik dağıtım modeline dönebilirsiniz. Geri dönün sonra Azure Klasik dağıtım modelinde bu vm'lerde normal işlemleri devam edebilmeniz için kaynaklar üzerinde yönetim düzeyi işlemlerine açar.

### <a name="abort"></a>Durdurma
Klasik dağıtım modeli için yaptığınız değişiklikleri geri almak ve geçiş durdurmak istiyorsanız, bu isteğe bağlı bir adımdır. Bu işlem, Kaynak Yöneticisi'ni (hazırlama adımda oluşturulan) meta veri kaynaklarınız için siler. 

![Abort adım diyagramı](../articles/virtual-machines/windows/media/migration-classic-resource-manager/behind-the-scenes-abort.png)


> [!NOTE]
> Yürütme işlemi tetiklemesi sonra bu işlemi gerçekleştirilemez.     
>

### <a name="commit"></a>İşleme
Doğrulama adımını tamamladıktan sonra geçişi işleyebilirsiniz. Kaynaklar Klasik dağıtım modelinde artık görünmez ve yalnızca Resource Manager dağıtım modelinde kullanılabilir. Geçirilen kaynaklar yalnızca yeni portalda yönetilebilir.

> [!NOTE]
> Bu, bir kere etkili olan bir işlemdir. Başarısız olursa, işlemi yeniden deneyin. Başarısız, destek bileti oluşturun veya oluşturmak devam ederse "ClassicIaaSMigration" ile bir forum gönderisi etiketi bizim [VM Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=WAVirtualMachinesforWindows).
>
>

![Yürütme adım diyagramı](../articles/virtual-machines/windows/media/migration-classic-resource-manager/behind-the-scenes-commit.png)

## <a name="migration-flowchart"></a>Geçiş akış çizelgesi

Geçirme işlemine devam etmek nasıl oluşturulduğunu gösteren bir akış çizelgesi şöyledir:

![Geçiş adımlarını gösteren ekran görüntüsü](../articles/virtual-machines/windows/media/migration-classic-resource-manager/migration-flow.png)

## <a name="translation-of-the-classic-deployment-model-to-resource-manager-resources"></a>Resource Manager kaynaklarını Klasik dağıtım modeline çevirisi
Aşağıdaki tabloda Klasik dağıtım modeli ve Resource Manager kaynakları gösterimlerini bulabilirsiniz. Diğer özellikler ve kaynaklar şu an desteklenmemektedir.

| Klasik gösterim | Resource Manager gösterimi | Notlar |
| --- | --- | --- |
| Bulut hizmeti adı |DNS adı |Geçiş sırasında, her bulut hizmeti için `<cloudservicename>-migrated` adlandırma deseni kullanılarak yeni bir kaynak grubu oluşturulur. Bu kaynak grubu tüm kaynaklarınızı içerir. Bulut hizmeti adı, genel IP adresiyle ilişkili bir DNS adı olur. |
| Sanal makine |Sanal makine |Sanal makineye özgü özellikler değiştirilmeden geçirilir. Bilgisayar adı gibi belirli osProfile bilgileri Klasik dağıtım modelinde depolanmaz ve geçiş sonrasında boş kalır. |
| Sanal makineye bağlı disk kaynakları |Sanal makineye bağlı örtük diskler |Resource Manager dağıtım modelinde diskler en üst düzey kaynaklar olarak modellenmez. Diskler, VM altındaki örtük diskler olarak geçirilir. Şu anda yalnızca sanal makineye bağlı diskler desteklenmektedir. Kaynak Yöneticisi Vm'leri artık herhangi bir güncelleştirme kolayca geçirilmesi için diskleri verir Klasik dağıtım modelinde depolama hesapları'nı kullanabilirsiniz. |
| VM uzantıları |VM uzantıları |Klasik dağıtım modelinden XML uzantıları dışındaki tüm kaynak uzantıları geçirilir. |
| Sanal makine sertifikaları |Azure Key Vault’ta Sertifikalar |Bir bulut hizmeti hizmet sertifikaları içeriyorsa, geçiş yeni bir Azure anahtar kasası başına bulut hizmeti oluşturur ve sertifika anahtar kasasını taşır. VM’ler, anahtar kasasındaki sertifikalara başvuracak şekilde güncelleştirilir. <br><br> Anahtar kasası silmeyin. Bu, başarısız bir duruma dönmek VM neden olabilir. |
| WinRM yapılandırması |osProfile altındaki WinRM yapılandırması |Windows Uzaktan Yönetimi yapılandırması, geçiş kapsamında değiştirilmeden taşınır. |
| Kullanılabilirlik kümesi özelliği |Kullanılabilirlik kümesi kaynağı | Kullanılabilirlik kümesi belirtimi, Klasik dağıtım modelinde VM üzerindeki bir özelliktir. Kullanılabilirlik kümeleri, geçiş kapsamında en üst düzey kaynağa dönüşür. Şu yapılandırmalar desteklenmez: bulut hizmeti başına birden çok kullanılabilirlik kümesi veya bir bulut hizmetindeki herhangi bir kullanılabilirlik kümesinde olmayan VM’lerle birlikte bir veya daha fazla kullanılabilirlik kümesi. |
| Bir VM’deki ağ yapılandırması |Birincil ağ arabirimi |Bir VM’deki ağ yapılandırması, geçişten sonra birincil ağ arabirimi kaynağı olarak gösterilir. Bir sanal ağda olmayan VM’lerin iç IP adresi geçiş sırasında değişir. |
| Bir VM’de birden çok ağ arabirimi |Ağ arabirimleri |Bir VM ile ilişkili birden çok ağ arabirimi varsa, her bir ağ arabirimine en üst düzey bir kaynak tüm özelliklerinin yanı sıra geçişin parçası olarak haline gelir. |
| Yük dengeli uç nokta kümesi |Yük dengeleyici |Klasik dağıtım modelinde, platform tarafından her bulut hizmetine örtük bir yük dengeleyici atanıyordu. Geçiş sırasında yeni bir yük dengeleyici kaynağı oluşturulur ve yük dengeleme uç noktası kümesi, yük dengeleyici kurallarına dönüşür. |
| Gelen NAT kuralları |Gelen NAT kuralları |VM’de tanımlanmış giriş uç noktaları, geçiş sırasında yük dengeleyici altındaki gelen ağ adresi çevirisi kurallarına dönüştürülür. |
| VIP adresi |DNS adına sahip genel IP adresi |Sanal IP adresi genel bir IP adresi olur ve yük dengeleyici ile ilişkilidir. Yalnızca giriş uç noktası atanmış sanal IP’ler geçirilebilir. |
| Sanal ağ |Sanal ağ |Sanal ağ, tüm özellikleriyle birlikte Resource Manager dağıtım modeline geçirilir. `-migrated` adlı yeni bir kaynak grubu oluşturulur. |
| Ayrılmış IP’ler |Statik ayırma yöntemi kullanan genel IP adresi |Yük dengeleyiciyle ilişkili ayrılmış IP’ler, bulut hizmetinin veya sanal makinenin geçişiyle birlikte geçirilir. İlişkili olmayan ayrılmış IP geçişi şu an desteklenmemektedir. |
| VM başına genel IP adresi |Dinamik ayırma yöntemi kullanan genel IP adresi |VM’le ilişkili genel IP adresi, ayırma yöntemi statik olarak ayarlanarak bir genel IP adresi kaynağı olarak dönüştürülür. |
| NSG'ler |NSG'ler |Bir alt ağla ilişkili ağ güvenlik grupları, geçiş kapsamında Resource Manager dağıtım modeline kopyalanır. Geçiş sırasında, klasik dağıtım modelindeki NSG kaldırılmaz. Bununla birlikte, geçiş sürdüğü sırada NSG için yönetim düzlemi işlemleri engellenir. |
| DNS sunucuları |DNS sunucuları |Bir sanal ağ veya VM’le ilişkili DNS sunucuları, kendilerine karşılık gelen kaynağın geçişi kapsamında, tüm özellikleriyle birlikte geçirilir. |
| UDR’ler |UDR’ler |Bir alt ağla ilişkili kullanıcı tanımlı rotalar, geçiş kapsamında Resource Manager dağıtım modeline kopyalanır. Geçiş sırasında, klasik dağıtım modelindeki UDR kaldırılmaz. Geçiş sürdüğü sırada UDR için yönetim düzlemi işlemleri engellenir. |
| Bir sanal makinenin ağ yapılandırmasında IP iletme özelliği |NIC’de IP iletme özelliği |Bir VM’deki IP iletme özelliği, geçiş sırasında ağ arabirimindeki bir özelliğe dönüştürülür. |
| Birden çok IP’si olan yük dengeleyici |Birden çok genel IP kaynağı olan yük dengeleyici |Yük Dengeleyici ile ilişkili her genel IP için genel IP kaynağı dönüştürülür ve geçişten sonra Yük Dengeleyici ile ilişkili. |
| VM’deki iç DNS adları |NIC’deki iç DNS adları |Geçiş sırasında, VM’lerin iç DNS son ekleri NIC’deki “InternalDomainNameSuffix” adlı salt okunur bir özelliğe geçirilir. Geçişten sonra soneki değişmez ve VM çözümleme olarak daha önce çalışmaya devam etmelidir. |
| Sanal ağ geçidi |Sanal ağ geçidi |Sanal ağ geçidi özellikleri değişmeden geçirilir. Ağ geçidiyle ilişkili VIP de değişmez. |
| Yerel ağ sitesi |Yerel ağ geçidi |Yerel ağ sitesi özellikleri bir yerel ağ geçidi olarak adlandırılan yeni bir kaynak için değişmeden geçirilir. Bu, şirket içi adres öneklerini ve uzak ağ geçidi IP temsil eder. |
| Bağlantı başvuruları |Bağlantı |Ağ geçidi ile ağ yapılandırmasında yerel ağ sitesi arasındaki bağlantı başvuruları bağlantı adlı yeni bir kaynak temsil edilir. Ağ yapılandırma dosyalarında bağlantı başvurusu tüm özelliklerini bağlantı kaynağı değişmeden kopyalanır. Klasik dağıtım modelinde sanal ağlar arasında bağlantı yerel ağ sitelerine sanal ağlar temsil eden iki IPSec tüneli oluşturarak elde edilir. Bu sanal ağ-sanal-ağa bağlantı türüne Resource Manager modelinde, yerel ağ geçitleri gerek kalmadan dönüştürüldüğünde. |

## <a name="changes-to-your-automation-and-tooling-after-migration"></a>Geçiş sonrası otomasyon ve araçlarınızda gerçekleşecek değişiklikler
Kaynaklarınızın Klasik dağıtım modelinden Resource Manager dağıtım modeline geçiş işleminin parçası olarak, var olan Otomasyon veya geçişten sonra çalışmaya devam ettiğinden emin olmak için araç güncelleştirmeniz gerekir.
