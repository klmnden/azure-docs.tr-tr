---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: dc871b29cdafa57d337f9be6cf01e76212f31b67
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66167079"
---
## <a name="migrate-iaas-resources-from-the-classic-deployment-model-to-azure-resource-manager"></a>Iaas kaynaklarını Klasik dağıtım modelinden Azure Resource Manager'a geçirme
İlk olarak, bir hizmet (Iaas) kaynak olarak altyapıyı veri düzlemi ve yönetim düzlemi işlemleri arasındaki farkı anlamak önemlidir.

* *Yönetim/denetim düzlemi* yönetim/denetim düzlemi veya kaynakların değiştirilmesi için API'ye gelen çağrıları açıklar. Örneğin, VM oluşturma, bir sanal makineyi yeniden başlatma ve çalışmakta olan kaynakları yönetmek için bir sanal ağı yeni alt ağla güncelleştirme gibi işlemler. Bunlar Vm'lere bağlanması doğrudan etkilemez.
* *Veri düzlemi* (uygulama) uygulamanın kendisinin çalışma zamanını açıklar ve örneklerle Azure API üzerinden gerçekleştirilmeyen etkileşimi içerir. Örneğin, sitenize erişme ya da çalışan bir SQL Server örneği veya bir MongoDB sunucusundan veri çekme verilerdir düzlemi veya uygulama etkileşimleri. Diğer, bir depolama hesabından blob kopyalama ve sanal makineye Uzak Masaüstü Protokolü (RDP) veya güvenli Kabuk (SSH) kullanmak için bir genel IP adresi erişim verilebilir. Bu işlemler, uygulamanın işlem, ağ ve depolama kaynaklarında çalışmaya devam etmesini sağlar.

Veri düzlemi, Klasik dağıtım modeli ve Resource Manager yığını arasında aynıdır. Geçiş işlemi sırasında Klasik dağıtım modelinden Resource Manager yığınında kaynakları, gösterimini Microsoft çevirir farktır. Sonuç olarak, yeni araçlar, API'ler ve SDK'lar, Resource Manager yığınında kaynaklarınızı yönetmek için kullanmanız gerekir.

![Yönetim/denetim düzlemi ve veri düzlemi arasındaki farkı gösteren diyagram](../articles/virtual-machines/media/virtual-machines-windows-migration-classic-resource-manager/data-control-plane.png)


> [!NOTE]
> Bazı geçiş senaryolarında, Azure platformu sanal makinenizi durdurur, serbest bırakır ve yeniden başlatır. Bu kısa bir veri düzlemi kapalı kalma neden olur.
>

## <a name="the-migration-experience"></a>Geçiş deneyimi
Geçişe başlamadan önce:

* Geçirmek istediğiniz kaynakların desteklenmeyen özellikleri veya yapılandırmaları olmadığından emin olun. Bu sorunlar genellikle platform tarafından algılanır ve bir hata oluşturulur.
* Bir sanal ağda olmayan Vm'leriniz varsa, bunlar durdurulur ve serbest hazırlama işleminin bir parçası olarak. Genel IP adresini kaybetmek istemiyorsanız, hazırlama işlemini tetiklemeden önce IP adresi ayırma göz önünde bulundurun. VM'ler sanal ağdaysa, durduruldu serbest ve değil.
* Geçiş sırasında gerçekleşebilecek beklenmeyen hataları göz önünde bulundurarak geçişinizi çalışma saatleri dışına gerçekleşecek şekilde planlayın.
* Hazırlama adımı tamamlandıktan sonra doğrulamayı kolaylaştırmak için PowerShell’i, komut satırı arabirimi (CLI) komutlarını veya REST API’leri kullanarak VM’lerinizin geçerli yapılandırmasını indirin.
* Otomasyon ve kullanıma hazır hale getirme betiklerinizi Resource Manager dağıtım modeline Geçişe başlamadan önce işlemek için güncelleştirin. İsteğe bağlı olarak, kaynaklar hazırlanmış durumdayken GET işlemleri gerçekleştirebilirsiniz.
* Iaas kaynaklarını Klasik dağıtım modelinde yapılandırılır ve geçiş tamamlandıktan sonra planlama rol tabanlı erişim denetimi (RBAC) ilkelerini değerlendirin.

Geçiş iş akışı aşağıdaki gibidir:

![Geçiş iş akışını gösteren diyagram](../articles/virtual-machines/windows/media/migration-classic-resource-manager/migration-workflow.png)

> [!NOTE]
> Aşağıdaki bölümlerde açıklanan işlem, tüm bir kere etkili olur. Desteklenmeyen bir özellik veya bir yapılandırma hatası dışında bir sorun varsa, hazırlama, yeniden deneme durdurma veya işlemeyi işlemi. Azure eylemi yeniden dener.
>
>

### <a name="validate"></a>Doğrula
Doğrulama işlemi, geçiş sürecinin ilk adımıdır. Bu adımın amacı, Klasik dağıtım modelinde geçirmek istediğiniz kaynakların durumunu analiz sağlamaktır. İşlem kaynakları (başarı veya başarısızlık) geçişe uygun olup olmadığını değerlendirir.

(Sanal ağ içinde değilse) sanal ağ veya Bulut hizmeti seçin, geçiş için doğrulamak istediğiniz. Kaynak geçişe uygun değilse, Azure neden olma nedenlerini listeler.

#### <a name="checks-not-done-in-the-validate-operation"></a>Doğrulama işlemi içinde yapılmadı denetler

Doğrulama işlemi, yalnızca klasik dağıtım modelinde kaynakların durumunu analiz eder. Tüm hataları ve desteklenmeyen senaryolar Klasik dağıtım modelinde çeşitli yapılandırmalarda nedeniyle kontrol edebilirsiniz. Azure Resource Manager yığınında geçiş sırasında kaynaklar üzerinde oluşturabileceğini tüm sorunları olup olmadığını denetlemek mümkün değildir. Kaynakları (Hazırlama işlemi) geçişin sonraki adımda dönüşüm geçmeleri olduğunda bu sorunlar yalnızca denetlenir. Aşağıdaki tabloda, doğrulama işleminde alınmamış tüm sorunları listeler:


|Ağ denetimleri doğrulama işlemi içinde değil|
|-|
|ER hem de VPN ağ geçitleri olan sanal ağı.|
|Bağlantısı kesik durumdaki bir sanal ağ geçidi bağlantısı.|
|Tüm ER devreler için Azure Resource Manager yığını önceden geçirilir.|
|Azure Resource Manager kotası, ağ kaynakları için denetler. Örneğin: statik genel IP, dinamik genel IP'ler, yük dengeleyici, ağ güvenlik grupları, yönlendirme tablolarını ve ağ arabirimleri. |
| Tüm yük dengeleyici kuralları, dağıtım ve sanal ağ arasında geçerlidir. |
| Aynı sanal ağda durdu-serbest VM'ler arasında çakışan özel IP'ler. |

### <a name="prepare"></a>Hazırlama
Hazırlama işlemi, geçiş sürecinin ikinci adımıdır. Bu adımın amacı, Iaas kaynaklarını Klasik dağıtım modelinden Resource Manager kaynaklarına dönüştürme içindir. Ayrıca, bu yan yana görselleştirmenizi için hazırlama işlemi sunar.

> [!NOTE] 
> Kaynaklarınızı Klasik dağıtım modelinde, bu adım sırasında değiştirilmez. Geçiş çalışıyorsanız çalıştırmak için güvenli bir adımdır. 

(Bir sanal ağ değilse), sanal ağ veya Bulut hizmeti, geçiş için hazırlamak istediğinizi seçin.

* Kaynak geçişe uygun değilse, Azure geçiş işlemini durdurur ve hazırlama işlemi başarısız olmasının nedeni listeler.
* Azure, kaynağın geçirilebilir durumda, Geçirilmekte olan kaynaklar için yönetim düzlemi işlemlerini kilitler. Örneğin, geçirilmekte olan bir VM’ye veri diski ekleyemezsiniz.

Azure daha sonra meta verileri geçişi Klasik dağıtım modelinden Resource Manager kaynakların geçirilmesi için başlatır.

Hazırlama işlemi tamamlandıktan sonra Klasik dağıtım modeli ve Resource Manager kaynakları görselleştirme seçeneğiniz vardır. Azure platformu, klasik dağıtım modelindeki her bulut hizmeti için `cloud-service-name>-Migrated` deseninde bir kaynak grubu adı oluşturur.

> [!NOTE]
> Geçirilen kaynaklar için oluşturulan bir kaynak grubu adını seçmek mümkün değildir (diğer bir deyişle, "-geçirildi"). Ancak, geçiş tamamlandıktan sonra kaynakları, istediğiniz herhangi bir kaynak grubuna taşımak için Azure Resource Manager'ın taşıma özelliğini kullanabilirsiniz. Daha fazla bilgi için bkz. [Kaynakları yeni kaynak grubuna veya aboneliğe taşıma](../articles/resource-group-move-resources.md).

Aşağıdaki iki ekran görüntüleri, başarılı bir hazırlama sonra işlem sonucunu göstermektedir. İlki, özgün bulut hizmetini içeren bir kaynak grubunu gösterir. İkinci yeni gösterilir "-geçirildi" eşdeğer Azure Resource Manager kaynaklarını içeren kaynak grubu.

![Özgün bulut hizmetini gösteren ekran görüntüsü](../articles/virtual-machines/windows/media/migration-classic-resource-manager/portal-classic.png)

![Hazırlama işlemi Azure Resource Manager kaynaklarını gösteren ekran görüntüsü](../articles/virtual-machines/windows/media/migration-classic-resource-manager/portal-arm.png)

Hazırlık aşaması tamamlandıktan sonra kaynaklarınızı Sahne Arkası göz aşağıdadır. Veri düzlemi kaynak aynı olduğunu unutmayın. Hem yönetim düzlemi (Klasik dağıtım modeli) hem de denetim düzlemi (Resource Manager) gösterilir.

![Hazırlık aşamasının diyagramı](../articles/virtual-machines/windows/media/migration-classic-resource-manager/behind-the-scenes-prepare.png)

> [!NOTE]
> Klasik dağıtım modelinde bir sanal ağda olmayan VM'lerin durdurulur ve serbest geçişin bu aşamasında.
>

### <a name="check-manual-or-scripted"></a>Denetim (el ile veya betikle)
Denetim adımında, daha önce indirmiş olduğunuz yapılandırmayı geçişte bir sorun olmadığını doğrulamak için kullanılacak seçeneğiniz vardır. Alternatif olarak, portalı ve Noktasal özellikleri ve meta verileri geçişi iyi görünüyor doğrulamak için kaynakları oturum açabilirsiniz.

Sanal ağ geçişi yapıyorsanız, sanal makinelerin çoğu yapılandırması yeniden başlatılmaz. Bu vm'lerdeki uygulamalar için uygulama hala çalıştığını doğrulayabilirsiniz.

VM'lerin beklendiği gibi çalıştığını ve güncelleştirilen betiklerinizin doğru çalışması için izleme ve operasyonel betiklerinizi test edebilirsiniz. Kaynaklar hazırlanmış durumdayken yalnızca GET işlemleri desteklenir.

Önce Geçişi tamamlamak gereken süreyi ayarlama penceresi yok. Bu durumdayken dilediğiniz kadar bekleyebilirsiniz. Bununla birlikte, bu kaynaklar için geçişi durdurana veya işleyene kadar yönetim düzlemi kilitli kalır.

Herhangi bir sorun yaşarsanız dilediğiniz zaman geçişi durdurabilir ve klasik dağıtım modeline dönebilirsiniz. Geri döndüğünüzde, Azure Klasik dağıtım modelinde bu VM'ler üzerinde normal işlemleri devam edebilir, kaynaklar üzerindeki yönetim düzlemi işlemlerini açar.

### <a name="abort"></a>Durdur
Değişikliklerinizi Klasik dağıtım modeline dönmek ve geçiş işlemini durdurmak istiyorsanız, bu isteğe bağlı bir adımdır. Bu işlem, kaynaklarınız için Resource Manager (hazırlama adımda oluşturulan) meta verileri siler. 

![Abort adım diyagramı](../articles/virtual-machines/windows/media/migration-classic-resource-manager/behind-the-scenes-abort.png)


> [!NOTE]
> Yürütme işlemini tetiklendikten sonra bu işlem gerçekleştirilemez.     
>

### <a name="commit"></a>Yürüt
Doğrulama adımını tamamladıktan sonra geçişi işleyebilirsiniz. Kaynaklar artık Klasik dağıtım modelinde görünmez ve yalnızca Resource Manager dağıtım modelinde kullanılabilir. Geçirilen kaynaklar yalnızca yeni portalda yönetilebilir.

> [!NOTE]
> Bu, bir kere etkili olan bir işlemdir. Başarısız olursa, işlemi yeniden deneyin. Bu, bir destek bileti oluşturun veya oluşturmak başarısız olmaya devam ederse etiketiyle bir forum gönderisi "Classicıaasmigration" bizim [VM forumumuzda](https://social.msdn.microsoft.com/Forums/azure/home?forum=WAVirtualMachinesforWindows).
>
>

![İşleme adımı diyagramı](../articles/virtual-machines/windows/media/migration-classic-resource-manager/behind-the-scenes-commit.png)

## <a name="migration-flowchart"></a>Geçişi akış çizelgesi

Geçişe devam gösteren akış grafiği aşağıda verilmiştir:

![Geçiş adımlarını gösteren ekran görüntüsü](../articles/virtual-machines/windows/media/migration-classic-resource-manager/migration-flow.png)

## <a name="translation-of-the-classic-deployment-model-to-resource-manager-resources"></a>Klasik dağıtım modeli Resource Manager kaynaklarına çevrilmesi
Aşağıdaki tabloda Klasik dağıtım modelini ve kaynakları Resource Manager gösterimlerini bulabilirsiniz. Diğer özellikler ve kaynaklar şu an desteklenmemektedir.

| Klasik gösterim | Resource Manager gösterimi | Notlar |
| --- | --- | --- |
| Bulut hizmeti adı |DNS adı |Geçiş sırasında, her bulut hizmeti için `<cloudservicename>-migrated` adlandırma deseni kullanılarak yeni bir kaynak grubu oluşturulur. Bu kaynak grubu tüm kaynaklarınızı içerir. Bulut hizmeti adı, genel IP adresiyle ilişkili bir DNS adı olur. |
| Sanal makine |Sanal makine |Sanal makineye özgü özellikler değiştirilmeden geçirilir. Bilgisayar adı gibi belirli osProfile bilgileri Klasik dağıtım modelinde depolanmaz ve geçişten sonra boş kalır. |
| Sanal makineye bağlı disk kaynakları |Sanal makineye bağlı örtük diskler |Resource Manager dağıtım modelinde diskler en üst düzey kaynaklar olarak modellenmez. Diskler, VM altındaki örtük diskler olarak geçirilir. Şu anda yalnızca sanal makineye bağlı diskler desteklenmektedir. Resource Manager Vm'lerinde diskler herhangi bir güncelleştirme gerekmeksizin kolayca geçirilmesini sağlayan Klasik dağıtım modelinde, depolama hesapları artık kullanabilirsiniz. |
| VM uzantıları |VM uzantıları |Klasik dağıtım modelinden XML uzantıları dışındaki tüm kaynak uzantıları geçirilir. |
| Sanal makine sertifikaları |Azure Key Vault’ta Sertifikalar |Bir bulut hizmeti hizmet sertifikaları içeriyorsa, geçiş bulut hizmeti başına yeni bir Azure anahtar kasası oluşturulur ve sertifikalar buraya taşınır. VM’ler, anahtar kasasındaki sertifikalara başvuracak şekilde güncelleştirilir. <br><br> Anahtar kasası silmeyin. Bu VM, başarısız bir duruma geçmesine neden olabilir. |
| WinRM yapılandırması |osProfile altındaki WinRM yapılandırması |Windows Uzaktan Yönetimi yapılandırması, geçiş kapsamında değiştirilmeden taşınır. |
| Kullanılabilirlik kümesi özelliği |Kullanılabilirlik kümesi kaynağı | Kullanılabilirlik kümesi belirtimi, Klasik dağıtım modelinde VM'deki bir özelliktir. Kullanılabilirlik kümeleri, geçiş kapsamında en üst düzey kaynağa dönüşür. Şu yapılandırmalar desteklenmez: bulut hizmeti başına birden çok kullanılabilirlik kümesi veya bir bulut hizmetindeki herhangi bir kullanılabilirlik kümesinde olmayan VM’lerle birlikte bir veya daha fazla kullanılabilirlik kümesi. |
| Bir VM’deki ağ yapılandırması |Birincil ağ arabirimi |Bir VM’deki ağ yapılandırması, geçişten sonra birincil ağ arabirimi kaynağı olarak gösterilir. Bir sanal ağda olmayan VM’lerin iç IP adresi geçiş sırasında değişir. |
| Bir VM’de birden çok ağ arabirimi |Ağ arabirimleri |Bir VM ile ilişkili birden çok ağ arabirimi varsa, her ağ arabirimi düzey bir kaynakla tam olarak birlikte tüm özellikler geçişin bir parçası haline gelir. |
| Yük dengeli uç nokta kümesi |Yük dengeleyici |Klasik dağıtım modelinde, platform tarafından her bulut hizmetine örtük bir yük dengeleyici atanıyordu. Geçiş sırasında yeni bir yük dengeleyici kaynağı oluşturulur ve yük dengeleme uç noktası kümesi, yük dengeleyici kurallarına dönüşür. |
| Gelen NAT kuralları |Gelen NAT kuralları |VM’de tanımlanmış giriş uç noktaları, geçiş sırasında yük dengeleyici altındaki gelen ağ adresi çevirisi kurallarına dönüştürülür. |
| VIP adresi |DNS adına sahip genel IP adresi |Sanal IP adresi genel bir IP adresine dönüşür ve yük dengeleyiciyle ilişkilendirilir. Yalnızca giriş uç noktası atanmış sanal IP’ler geçirilebilir. |
| Sanal ağ |Sanal ağ |Sanal ağ, tüm özellikleriyle birlikte Resource Manager dağıtım modeline geçirilir. `-migrated` adlı yeni bir kaynak grubu oluşturulur. |
| Ayrılmış IP’ler |Statik ayırma yöntemi kullanan genel IP adresi |Yük dengeleyiciyle ilişkili ayrılmış IP’ler, bulut hizmetinin veya sanal makinenin geçişiyle birlikte geçirilir. İlişkili olmayan ayrılmış IP geçişi şu an desteklenmemektedir. |
| VM başına genel IP adresi |Dinamik ayırma yöntemi kullanan genel IP adresi |VM’le ilişkili genel IP adresi, ayırma yöntemi statik olarak ayarlanarak bir genel IP adresi kaynağı olarak dönüştürülür. |
| NSG'ler |NSG'ler |Bir alt ağla ilişkili ağ güvenlik grupları, geçiş kapsamında Resource Manager dağıtım modeline kopyalanır. Geçiş sırasında, klasik dağıtım modelindeki NSG kaldırılmaz. Bununla birlikte, geçiş sürdüğü sırada NSG için yönetim düzlemi işlemleri engellenir. |
| DNS sunucuları |DNS sunucuları |Bir sanal ağ veya VM’le ilişkili DNS sunucuları, kendilerine karşılık gelen kaynağın geçişi kapsamında, tüm özellikleriyle birlikte geçirilir. |
| UDR’ler |UDR’ler |Bir alt ağla ilişkili kullanıcı tanımlı rotalar, geçiş kapsamında Resource Manager dağıtım modeline kopyalanır. Geçiş sırasında, klasik dağıtım modelindeki UDR kaldırılmaz. Geçiş sürdüğü sırada UDR için yönetim düzlemi işlemleri engellenir. |
| Bir sanal makinenin ağ yapılandırmasında IP iletme özelliği |NIC’de IP iletme özelliği |Bir VM’deki IP iletme özelliği, geçiş sırasında ağ arabirimindeki bir özelliğe dönüştürülür. |
| Birden çok IP’si olan yük dengeleyici |Birden çok genel IP kaynağı olan yük dengeleyici |Yük dengeleyiciyle ilişkili her genel IP, genel bir IP kaynağına dönüştürülür ve geçişten sonra Yük dengeleyiciyle ilişkili. |
| VM’deki iç DNS adları |NIC’deki iç DNS adları |Geçiş sırasında, VM’lerin iç DNS son ekleri NIC’deki “InternalDomainNameSuffix” adlı salt okunur bir özelliğe geçirilir. Geçişten sonra son ek değişmez ve VM çözümleme eskisi gibi çalışmaya devam etmesi gerekir. |
| Sanal ağ geçidi |Sanal ağ geçidi |Sanal ağ geçidi özellikleri değişmeden geçirilir. Ağ geçidiyle ilişkili VIP de değişmez. |
| Yerel ağ sitesi |Yerel ağ geçidi |Yerel ağ sitesi özellikleri değişmeden yerel ağ geçidi adı verilen yeni bir kaynağa geçirilir. Bu, şirket içi adres ön eklerini ve uzak ağ geçidi IP'sini temsil eder. |
| Bağlantı başvuruları |Bağlantı |Ağ geçidi ve ağ yapılandırmasının yerel ağ sitesi arasındaki bağlantı başvuruları bağlantı adlı yeni bir kaynak tarafından temsil edilir. Ağ yapılandırma dosyasındaki bağlantı başvurusunun tüm özellikleri değişmeden bağlantı kaynağına kopyalanır. Klasik dağıtım modelinde sanal ağlar arasında bağlantı, sanal ağları temsil eden yerel ağ sitelerine iki IPSec tüneli oluşturularak elde edilir. Bu kaynak yöneticisi modelindeki sanal ağ-sanal-ağ bağlantı türü için yerel ağ geçitleri gerektirmeden dönüştürülür. |

## <a name="changes-to-your-automation-and-tooling-after-migration"></a>Geçiş sonrası otomasyon ve araçlarınızda gerçekleşecek değişiklikler
Kaynaklarınızı Klasik dağıtım modelinden Resource Manager dağıtım modeline geçiş işleminin bir parçası olarak, var olan Otomasyon veya geçişten sonra çalışmaya devam etmesini sağlamak için araçları güncelleştirmeniz gerekir.
