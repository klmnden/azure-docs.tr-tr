## <a name="meaning-of-migration-of-iaas-resources-from-classic-to-resource-manager"></a>IaaS kaynaklarının klasikten Resource Manager’a geçirilmesinin anlamı
Ayrıntılara geçmeden önce, IaaS kaynakları üzerindeki veri düzlemi işlemleri ile yönetim düzlemi işlemleri arasındaki farka bakalım.

* *Yönetim/Denetim düzlemi*, kaynakların değiştirilmesi için yönetim/denetim düzlemine veya API’ye gelen çağrıları açıklar. Örneğin, VM oluşturma, bir sanal makineyi yeniden başlatma ve çalışmakta olan kaynakları yönetmek için bir sanal ağı yeni alt ağla güncelleştirme gibi işlemler. Bunlar örneklere bağlanmayı doğrudan etkilemez.
* *Veri düzlemi* (uygulama), uygulamanın kendisinin çalışma zamanını açıklar ve örneklerle Azure API üzerinden gerçekleştirilmeyen etkileşimleri ilgilendirir. Web sitenize erişme ya da çalışmakta olan bir SQL Server örneğinden veya MongoDB sunucusundan veri çekme işlemleri, veri düzlemi veya uygulama etkileşimi olarak görülür. Bir depolama hesabından blob kopyalama ve genel bir IP adresine erişerek sanal makinede RDP veya SSH oturumu açma da veri düzlemidir. Bu işlemler, uygulamanın işlem, ağ ve depolama kaynaklarında çalışmaya devam etmesini sağlar.

![Yönetim/denetim düzlemi ile veri düzlemi arasındaki farkı gösteren ekran görüntüsü](../articles/virtual-machines/media/virtual-machines-windows-migration-classic-resource-manager/data-control-plane.png)

> [!NOTE]
> Bazı geçiş senaryolarında, Azure platformu sanal makinenizi durdurur, serbest bırakır ve yeniden başlatır. Bu durum, kısa bir veri düzlemi kapalı kalma süresine yol açar.
>
>

## <a name="the-migration-experience"></a>Geçiş deneyimi
Geçiş deneyimine başlamadan önce aşağıdakileri yapmanız önerilir:

* Geçirmek istediğiniz kaynakların desteklenmeyen özellikleri veya yapılandırmaları olmadığından emin olun. Bu sorunlar genellikle platform tarafından algılanır ve bir hata oluşturulur.
* Sanal ağda olmayan VM’leriniz varsa, hazırlama işlemi kapsamında bunlar durdurulur ve serbest bırakılır. Genel IP adresini kaybetmek istemiyorsanız, hazırlama işlemini tetiklemeden önce IP adresini koruma konusunu araştırın. Bununla birlikte, VM’ler sanal ağdaysa durdurulmaz ve serbest bırakılmaz.
* Geçiş sırasında gerçekleşebilecek beklenmeyen hataları göz önünde bulundurarak geçişinizi çalışma saatleri dışına gerçekleşecek şekilde planlayın.
* Hazırlama adımı tamamlandıktan sonra doğrulamayı kolaylaştırmak için PowerShell’i, komut satırı arabirimi (CLI) komutlarını veya REST API’leri kullanarak VM’lerinizin geçerli yapılandırmasını indirin.
* Geçişi başlatmadan önce otomasyon/çalışır hale getirme betiklerinizi Resource Manager dağıtım modeline uygun olacak şekilde güncelleştirin. İsteğe bağlı olarak, kaynaklar hazırlanmış durumdayken GET işlemleri gerçekleştirebilirsiniz.
* Klasik IaaS kaynaklarında yapılandırılmış RBAC ilkelerini değerlendirin ve geçiş sonrası için plan yapın.

Geçiş iş akışı aşağıdaki gibidir

![Geçiş iş akışını gösteren ekran görüntüsü](../articles/virtual-machines/windows/media/migration-classic-resource-manager/migration-workflow.png)

> [!NOTE]
> Aşağıdaki bölümlerde açıklanan tüm işlemler bir kere etkili olur. Desteklenmeyen özellik veya yapılandırma hatası dışında bir sorun yaşıyorsanız hazırlama, durdurma veya işlemeyi yeniden denemeniz önerilir. Azure platformu eylemi yeniden dener.
>
>

### <a name="validate"></a>Doğrulama
Doğrulama işlemi, geçiş sürecinin ilk adımıdır. Bu adımın amacı, geçiş kapsamındaki kaynaklar için verilerin arka planda analiz edilmesi ve kaynakların geçişe uygun olup olmadığına ilişkin başarı/hata sonucu döndürülmesidir.

Geçiş için doğrulamak istediğiniz sanal ağı veya barındırılan hizmeti (sanal ağ değilse) seçersiniz.

* Kaynak geçişe uygun değilse, kaynağın geçiş için desteklenmemesine neden olan tüm sebepler Azure platformu tarafından listelenir.

Depolama hizmetlerini doğrularken, geçirilen hesabı depolama hesabınızla aynı ada sahip, ancak sonuna “-Geçirildi” eklenmiş bir kaynak grubunda bulabilirsiniz.  Örneğin, depolama hesabınızın adı "depolamam" ise, Azure Resource Manager özellikli kaynağınızı "depolamam-Geçirildi" adlı bir kaynak grubunda bulursunuz ve bu grup "depolamam" adlı bir depolama hesabı içerir.

### <a name="prepare"></a>Hazırlama
Hazırlama işlemi, geçiş sürecinin ikinci adımıdır. Bu adımın amacı, IaaS kaynaklarının klasikten Resource Manager kaynaklarına dönüştürülmesinin benzetimini yapmak ve görselleştirmeniz için bunları yan yana sunmaktır.

Geçiş için hazırlamak istediğiniz sanal ağı veya barındırılan hizmeti (sanal ağ değilse) seçersiniz.

* Kaynak geçişe uygun değilse, Azure platformu geçiş işlemini durdurur ve hazırlama işleminin başarısız olmasına yol açan sebepleri listeler.
* Kaynak geçişe uygunsa, Azure platformu ilk olarak geçirilmekte olan kaynaklar için yönetim düzlemi işlemlerini kilitler. Örneğin, geçirilmekte olan bir VM’ye veri diski ekleyemezsiniz.

Daha sonra, Azure platformu kaynakların geçirilmesi için meta verilerin klasikten Resource Manager’a geçişini başlatır.

Hazırlama işlemi tamamlandıktan sonra hem klasik hem de Resource Manager’da kaynakları görselleştirme seçeneğiniz vardır. Azure platformu, klasik dağıtım modelindeki her bulut hizmeti için `cloud-service-name>-Migrated` deseninde bir kaynak grubu adı oluşturur.

> [!NOTE]
> Geçirilen kaynaklar için oluşturulan Kaynak Grubunun adını (“-Geçirildi”) seçmek mümkün değildir, ancak geçiş tamamlandıktan sonra Azure Resource Manager’ın taşıma özelliğini kullanarak kaynakları dilediğiniz Kaynak Grubuna taşıyabilirsiniz. Bu konu hakkında daha fazla bilgi için bkz. [Kaynakları yeni kaynak grubuna veya aboneliğe taşıma](../articles/resource-group-move-resources.md)

Burada, başarılı bir Hazırlama işleminin sonucunu gösteren iki ekran gösterilmiştir. İlk ekranda özgün bulut hizmetini içeren bir Kaynak Grubu görünmektedir. İkinci ekranda ise eşdeğer Azure Resource Manager kaynaklarını içeren yeni “-Geçirildi” adlı kaynak grubu görünmektedir.

![Portal klasik bulut hizmetini gösteren ekran görüntüsü](../articles/virtual-machines/windows/media/migration-classic-resource-manager/portal-classic.png)

![Hazırlama aşamasında Portal Azure Resource Manager kaynaklarını gösteren ekran görüntüsü](../articles/virtual-machines/windows/media/migration-classic-resource-manager/portal-arm.png)

> [!NOTE]
> Klasik bir Sanal Ağda olmayan Sanal Makineler, geçişin bu aşamasında durdurulur ve serbest bırakılır.
>
>

### <a name="check-manual-or-scripted"></a>Denetim (el ile veya betikle)
Denetim adımında, isteğe bağlı olarak daha önce geçişte bir sorun olmadığını doğrulamak için indirmiş olduğunuz yapılandırmayı kullanabilirsiniz. Alternatif olarak, portalda oturum açabilir ve meta veri geçişinin sorunsuz bir biçimde gerçekleştirildiğini doğrulamak için özellikler ve kaynakları noktasal olarak denetleyebilirsiniz.

Sanal ağ geçişi yapıyorsanız, sanal makinelerin çoğu yapılandırması yeniden başlatılmaz. Bu VM’lerdeki uygulamalar için uygulamanın hala çalışmakta olduğunu doğrulayabilirsiniz.

VM’lerin beklendiği gibi çalıştığını doğrulamak ve güncelleştirilen betiklerinizin doğru çalışıp çalışmadığını denetlemek için izleme/otomasyon ve operasyonel betiklerinizi test edebilirsiniz. Kaynaklar hazırlanmış durumdayken yalnızca GET işlemleri desteklenir.

Geçişi işlemeniz için belirli bir zaman kısıtlaması yoktur. Bu durumdayken dilediğiniz kadar bekleyebilirsiniz. Bununla birlikte, bu kaynaklar için geçişi durdurana veya işleyene kadar yönetim düzlemi kilitli kalır.

Herhangi bir sorun yaşarsanız dilediğiniz zaman geçişi durdurabilir ve klasik dağıtım modeline dönebilirsiniz. Geri döndüğünüzde, Azure platformu kaynaklar üzerindeki yönetim düzlemi işlemlerini açar ve klasik dağıtım modelinde bu VM’ler üzerinde normal işlemleri gerçekleştirmeye devam edebilirsiniz.

### <a name="abort"></a>Durdurma
Durdurma, değişikliklerinizi geri alarak klasik dağıtım modeline dönmek ve geçiş işlemini durdurmak için kullanabileceğiniz isteğe bağlı bir adımdır.

> [!NOTE]
> İşlemeyi tetiklendikten sonra bu işlemi yürütülemez.     
>
>

### <a name="commit"></a>İşleme
Doğrulama adımını tamamladıktan sonra geçişi işleyebilirsiniz. Kaynaklar artık klasik modelde görünmez ve yalnızca Resource Manager dağıtım modelinde kullanılabilir. Geçirilen kaynaklar yalnızca yeni portalda yönetilebilir.

> [!NOTE]
> Bu, bir kere etkili olan bir işlemdir. Başarısız olması durumunda işlemi yeniden denemeniz önerilir. Başarısız olmaya devam ederse bir destek bileti oluşturun veya [VM forumumuzda](https://social.msdn.microsoft.com/Forums/azure/home?forum=WAVirtualMachinesforWindows) ClassicIaaSMigration etiketiyle bir forum gönderisi oluşturun.
>
>
<br>
Aşağıda, geçiş sürecinde gerçekleştirilecek adımların akış çizelgesi görünmektedir

![Geçiş adımlarını gösteren ekran görüntüsü](../articles/virtual-machines/windows/media/migration-classic-resource-manager/migration-flow.png)

## <a name="translation-of-classic-to-azure-resource-manager-resources"></a>Klasik kaynakların Azure Resource Manager kaynaklarına çevrilmesi
Aşağıdaki tabloda, kaynakların klasik gösterimlerini ve Resource Manager gösterimlerini görebilirsiniz. Diğer özellikler ve kaynaklar şu an desteklenmemektedir.

| Klasik gösterim | Resource Manager gösterimi | Ayrıntılı notlar |
| --- | --- | --- |
| Bulut hizmeti adı |DNS adı |Geçiş sırasında, her bulut hizmeti için `<cloudservicename>-migrated` adlandırma deseni kullanılarak yeni bir kaynak grubu oluşturulur. Bu kaynak grubu tüm kaynaklarınızı içerir. Bulut hizmeti adı, genel IP adresiyle ilişkili bir DNS adı olur. |
| Sanal makine |Sanal makine |Sanal makineye özgü özellikler değiştirilmeden geçirilir. Bilgisayar adı gibi belirli osProfile bilgileri klasik dağıtım modelinde depolanmaz ve geçişten sonra boş kalır. |
| Sanal makineye bağlı disk kaynakları |Sanal makineye bağlı örtük diskler |Resource Manager dağıtım modelinde diskler en üst düzey kaynaklar olarak modellenmez. Diskler, VM altındaki örtük diskler olarak geçirilir. Şu anda yalnızca sanal makineye bağlı diskler desteklenmektedir. Resource Manager VM’leri artık klasik depolama hesaplarını kullanabildiğinden, diskler herhangi bir güncelleştirme gerekmeksizin kolayca geçirilebilir. |
| VM uzantıları |VM uzantıları |Klasik dağıtım modelinden XML uzantıları dışındaki tüm kaynak uzantıları geçirilir. |
| Sanal makine sertifikaları |Azure Key Vault’ta Sertifikalar |Hizmet sertifikası içeren bulut hizmetleri için bulut hizmeti başına yeni bir Azure anahtar kasası oluşturulur ve sertifikalar buraya taşınır. VM’ler, anahtar kasasındaki sertifikalara başvuracak şekilde güncelleştirilir. <br><br> **NOT:** Anahtar kasasının silinmesi VM’nin başarısız bir duruma geçmesine yol açabileceğinden, lütfen bunu yapmayın. Anahtar Kasalarının güvenli bir biçimde silinebilmesi veya VM’le birlikte yeni bir aboneliğe taşınabilmesi için arka uçtaki kaynakları geliştiriyoruz. |
| WinRM yapılandırması |osProfile altındaki WinRM yapılandırması |Windows Uzaktan Yönetimi yapılandırması, geçiş kapsamında değiştirilmeden taşınır. |
| Kullanılabilirlik kümesi özelliği |Kullanılabilirlik kümesi kaynağı | Kullanılabilirlik kümesi belirtimi, klasik dağıtım modelinde VM’deki bir özellikti. Kullanılabilirlik kümeleri, geçiş kapsamında en üst düzey kaynağa dönüşür. Şu yapılandırmalar desteklenmez: bulut hizmeti başına birden çok kullanılabilirlik kümesi veya bir bulut hizmetindeki herhangi bir kullanılabilirlik kümesinde olmayan VM’lerle birlikte bir veya daha fazla kullanılabilirlik kümesi. |
| Bir VM’deki ağ yapılandırması |Birincil ağ arabirimi |Bir VM’deki ağ yapılandırması, geçişten sonra birincil ağ arabirimi kaynağı olarak gösterilir. Bir sanal ağda olmayan VM’lerin iç IP adresi geçiş sırasında değişir. |
| Bir VM’de birden çok ağ arabirimi |Ağ arabirimleri |Bir sanal makineyle ilişkili birden çok ağ arabirimi varsa, her ağ arabirimi ve bunlara ait tüm özellikler geçişin bir parçası olarak Resource Manager dağıtım modelinde en üst düzey kaynağa dönüşür. |
| Yük dengeli uç nokta kümesi |Yük dengeleyici |Klasik dağıtım modelinde, platform tarafından her bulut hizmetine örtük bir yük dengeleyici atanıyordu. Geçiş sırasında yeni bir yük dengeleyici kaynağı oluşturulur ve yük dengeleme uç noktası kümesi, yük dengeleyici kurallarına dönüşür. |
| Gelen NAT kuralları |Gelen NAT kuralları |VM’de tanımlanmış giriş uç noktaları, geçiş sırasında yük dengeleyici altındaki gelen ağ adresi çevirisi kurallarına dönüştürülür. |
| VIP adresi |DNS adına sahip genel IP adresi |Sanal IP adresi genel IP adresine dönüşür ve yük dengeleyiciyle ilişkilendirilir. Yalnızca giriş uç noktası atanmış sanal IP’ler geçirilebilir. |
| Sanal ağ |Sanal ağ |Sanal ağ, tüm özellikleriyle birlikte Resource Manager dağıtım modeline geçirilir. `-migrated` adlı yeni bir kaynak grubu oluşturulur. |
| Ayrılmış IP’ler |Statik ayırma yöntemi kullanan genel IP adresi |Yük dengeleyiciyle ilişkili ayrılmış IP’ler, bulut hizmetinin veya sanal makinenin geçişiyle birlikte geçirilir. İlişkili olmayan ayrılmış IP geçişi şu an desteklenmemektedir. |
| VM başına genel IP adresi |Dinamik ayırma yöntemi kullanan genel IP adresi |VM’le ilişkili genel IP adresi, ayırma yöntemi statik olarak ayarlanarak bir genel IP adresi kaynağı olarak dönüştürülür. |
| NSG'ler |NSG'ler |Bir alt ağla ilişkili ağ güvenlik grupları, geçiş kapsamında Resource Manager dağıtım modeline kopyalanır. Geçiş sırasında, klasik dağıtım modelindeki NSG kaldırılmaz. Bununla birlikte, geçiş sürdüğü sırada NSG için yönetim düzlemi işlemleri engellenir. |
| DNS sunucuları |DNS sunucuları |Bir sanal ağ veya VM’le ilişkili DNS sunucuları, kendilerine karşılık gelen kaynağın geçişi kapsamında, tüm özellikleriyle birlikte geçirilir. |
| UDR’ler |UDR’ler |Bir alt ağla ilişkili kullanıcı tanımlı rotalar, geçiş kapsamında Resource Manager dağıtım modeline kopyalanır. Geçiş sırasında, klasik dağıtım modelindeki UDR kaldırılmaz. Geçiş sürdüğü sırada UDR için yönetim düzlemi işlemleri engellenir. |
| Bir sanal makinenin ağ yapılandırmasında IP iletme özelliği |NIC’de IP iletme özelliği |Bir VM’deki IP iletme özelliği, geçiş sırasında ağ arabirimindeki bir özelliğe dönüştürülür. |
| Birden çok IP’si olan yük dengeleyici |Birden çok genel IP kaynağı olan yük dengeleyici |Yük dengeleyici ile ilişkili her genel IP bir genel IP kaynağına dönüştürülür ve geçişten sonra yük dengeleyici ile ilişkilendirilir. |
| VM’deki iç DNS adları |NIC’deki iç DNS adları |Geçiş sırasında, VM’lerin iç DNS son ekleri NIC’deki “InternalDomainNameSuffix” adlı salt okunur bir özelliğe geçirilir. Geçişten sonra son ek değişmez ve VM çözümleme eskisi gibi çalışmaya devam etmelidir. |
| Sanal Ağ Geçidi |Sanal Ağ Geçidi |Sanal Ağ Geçidi özellikleri değişmeden geçirilir. Ağ geçidiyle ilişkili VIP de değişmez. |
| Yerel ağ sitesi |Yerel Ağ Geçidi |Yerel ağ sitesi özellikleri değişmeden Yerel Ağ Geçidi adlı yeni bir kaynağa geçirilir. Bu, şirket içi adres ön eklerini ve uzak ağ geçidi IP’sini temsil eder. |
| Bağlantı başvuruları |Bağlantı |Ağ yapılandırmasında ağ geçidi ile yerel ağ sitesi arasındaki bağlantı başvuruları, geçişten sonra Resource Manager’da yeni oluşturulan Bağlantı adlı bir kaynakla temsil edilir. Ağ yapılandırma dosyasındaki bağlantı başvurusunun tüm özellikleri değiştirilmeksizin yeni oluşturulan Bağlantı kaynağına kopyalanır. Klasik modeldeki sanal sğdan sanal ağa bağlantı, VNetsanal ağları temsil eden yerel ağ sitelerine yönelik iki IPsec tüneli oluşturularak elde edilir. Resource Manager modelinde bu, yerel ağ geçitleri gerektirmeden sanal ağdan sanal ağa bağlantı türüne dönüştürülür. |

## <a name="changes-to-your-automation-and-tooling-after-migration"></a>Geçiş sonrası otomasyon ve araçlarınızda gerçekleşecek değişiklikler
Kaynaklarınızın Klasik dağıtım modelinden Resource Manager dağıtım modeline geçirilmesi kapsamında, mevcut otomasyon altyapınızı ve araçlarınızı geçişten sonra çalışmaya devam edecek şekilde güncelleştirmeniz gerekir.
