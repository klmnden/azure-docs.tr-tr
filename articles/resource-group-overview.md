<properties
   pageTitle="Azure Resource Manager Genel Bakış | Microsoft Azure"
   description="Azure’daki kaynakların dağıtımı, yönetimi ve erişim denetimi için Azure Resource Manager’ın nasıl kullanılacağı açıklanmaktadır."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/19/2016"
   ms.author="tomfitz"/>

# Azure Resource Manager genel bakış

Uygulamanızın altyapısı genellikle bir sanal makine, depolama hesabı, sanal ağ veya web uygulaması, veritabanı, veritabanı sunucusu ya da 3. taraf hizmetler gibi birçok bileşenden meydana gelir.  Bu bileşenleri ayrı varlıklar olarak değerlendirmez, bunun yerine bunları tek bir varlığın ilgili ve birbirine bağımlı parçaları olarak kabul edersiniz. Bunları gruplar halinde dağıtmak, yönetmek ve izlemek isteyebilirsiniz. Azure Resource Manager, çözümünüzdeki kaynaklar ile gruplar halinde çalışmanıza olanak sağlar. Çözümünüzdeki tüm kaynakları tek ve eşgüdümlü bir işlemle dağıtabilir, güncelleştirebilir veya silebilirsiniz. Dağıtım için bir şablon kullanabilirsiniz. Üstelik bu şablon test, hazırlık ve üretim gibi farklı ortamlarda da çalışabilir. Resource Manager kaynaklarınızı dağıttıktan sonra yönetmenize yardımcı olmak için güvenlik, denetleme ve etiketleme özellikleri sunar. 

## Terminoloji

Azure Resource Manager’ı kullanmaya yeni başladıysanız bilmiyor olabileceğiniz bazı terimler vardır.

- **kaynak** - Azure ile kullanılabilen yönetilebilir bir öğe. Sanal makine, depolama hesabı, web uygulaması, veritabanı ve sanal ağ bazı yaygın kaynaklardandır, ancak çok daha fazlası mevcuttur.
- **kaynak grubu** - Bir uygulama için ilgili kaynakları bir arada tutan bir kapsayıcıdır. Kaynak grubu bir uygulamanın tüm kaynaklarını veya yalnızca birlikte gruplandırdığınız kaynakları içerebilir. Kuruluş için önemli olan faktörleri temel alarak kaynakları kaynak gruplarına nasıl ayıracağınızı belirleyebilirsiniz. Bkz. [Kaynak grupları](#resource-groups).
- **kaynak sağlayıcısı** - Resource Manager aracılığıyla dağıtabileceğiniz ve yönetebileceğiniz kaynakları sağlayan bir hizmet. Her kaynak sağlayıcısı dağıtılan kaynaklarla çalışmaya yönelik işlemler sunar. Sanal makine kaynağı sağlayan Microsoft.Compute, depolama hesabı kaynağı sağlayan Microsoft.Storage ve web uygulamalarıyla ilgili kaynakları sağlayan Microsoft.Web bazı yaygın kaynak sağlayıcılarındandır. Bkz. [Kaynak sağlayıcıları](#resource-providers).
- **Resource Manager şablonu** - Bir kaynak grubuna dağıtılacak bir veya daha fazla kaynağı tanımlayan JavaScript Nesne Gösterimi (JSON) dosyası. Ayrıca dağıtılan kaynaklar arasındaki bağımlılıkları tanımlar. Şablon, kaynakları tutarlı ve sürekli olarak dağıtmak için kullanılabilir. Bkz. [Şablon dağıtımı](#template-deployment).
- **bildirim temelli söz dizimi** - Oluşturmaya yönelik programlama komutları dizisini yazmak zorunda kalmadan "Oluşturmak istediğiniz şeyi" belirtmenize imkan tanıyan söz dizimi. Resource Manager şablonu, bildirim temelli söz diziminin bir örneğidir. Dosya içinde Azure’a dağıtılacak altyapının özelliklerini tanımlarsınız. 

## Resource Manager’ı kullanmanın avantajları

Resource Manager çeşitli avantajlar sunar:

- Çözümünüzdeki tüm kaynakları ayrı ayrı ele almak yerine bunları grup halinde dağıtabilir, yönetebilir ve izleyebilirsiniz.
- Çözümünüzü geliştirme yaşam döngüsü boyunca defalarca dağıtabilirsiniz. Kaynaklarınızın tutarlı bir durumda dağıtılması da size güven verir.
- Altyapınızı betikler yerine bildirim temelli şablonları kullanarak yönetebilirsiniz.
- Doğru sırayla dağıtılmalarını sağlamak için kaynaklarınız arasındaki bağımlılıkları tanımlayabilirsiniz.
- Rol Tabanlı Erişim Denetimi (RBAC) yönetim platformuyla doğrudan tümleşik olduğu için kaynak grubunuzdaki tüm hizmetlere erişim denetimi uygulayabilirsiniz.
- Aboneliğinizdeki tüm kaynakları mantıksal olarak düzenlemek için kaynaklarınıza etiketler ekleyebilirsiniz.
- Aynı etiketi paylaşan tüm kaynak grupları veya bir kaynak grubu için aktarılmış maliyetleri görüntüleyerek kuruluşunuzun faturalarına açıklık getirebilirsiniz.  

Resource Manager çözümlerinizi dağıtmanın ve yönetmenin yeni bir yolunu sunar. Önceki dağıtım modelini kullandıysanız ve değişiklikler hakkında bilgi edinmek isterseniz, bkz. [Resource Manager dağıtımını ve klasik dağıtımı anlama](resource-manager-deployment-model.md)

## Rehber

Çözümleriniz üzerinde çalışırken aşağıdaki önerilerden yararlanarak Resource Manager’dan tam anlamıyla yararlanabilirsiniz.

1. Altyapınızı kesinlik temelli komutlar yerine Resource Manager şablonlarındaki bildirim temelli söz dizimini kullanarak tanımlayabilir ve dağıtabilirsiniz.
2. Şablondaki tüm dağıtım ve yapılandırma adımlarını tanımlayın. Çözümünüzü ayarlarken el ile yapılan hiçbir adım olmaması gerekir.
3. Kaynaklarınızı yönetirken, örneğin bir uygulamayı veya makineyi başlatırken ya da durdururken, kesinlik temelli komutlar çalıştırın.
4. Kaynak grubundaki aynı yaşam döngüsüne sahip kaynakları düzenleyin. Diğer tüm düzenleyici kaynaklar için etiketler kullanın.

Daha fazla öneri için bkz. [Azure Resource Manager şablonları oluşturmaya yönelik en iyi uygulamalar](resource-manager-template-best-practices.md).

## Kaynak grupları

Kaynak gruplarınızı tanımlarken göz önüne almanız gereken bazı önemli faktörler bulunur:

1. Grubunuzdaki tüm kaynaklar aynı yaşam döngüsünü paylaşmalıdır. Bunları birlikte dağıtır, güncelleştirir ve silersiniz. Veritabanı sunucusu gibi bir kaynağın farklı bir dağıtım döngüsünde bulunması gerekiyorsa, bu kaynak farklı bir kaynak grubuna konulmalıdır.
2. Her kaynak yalnızca bir kaynak grubunda yer alabilir.
3. Bir kaynak grubuna dilediğiniz zaman kaynak ekleyebilir veya kaldırabilirsiniz.
4. Bir kaynağı, bir kaynak grubundan bir diğerine taşıyabilirsiniz. Daha fazla bilgi için bkz. [Kaynakları yeni kaynak grubuna veya aboneliğe taşıma](resource-group-move-resources.md).
4. Bir kaynak grubu farklı bölgelerde bulunan kaynakları içerebilir.
5. Bir kaynak grubu, yönetim eylemleri için erişim denetimini incelemek üzere kullanılabilir.
6. İki kaynağın ilişkili olması gerekirken kaynaklar aynı yaşam döngüsünü paylaşmadığında (örneğin, bir veritabanına bağlı web uygulaması) bir kaynak başka bir kaynak grubundaki kaynakla etkileşim kurabilir.

## Kaynak sağlayıcıları

Her kaynak sağlayıcısı teknik alanla çalışmaya yönelik bir dizi kaynak ve işlem sunar. Örneğin, anahtarları ve parolaları saklamak isterseniz **Microsoft.KeyVault** kaynak sağlayıcısı ile çalışırsınız. Bu kaynak sağlayıcısı, anahtar kasasını oluşturmak için **kasalar**, anahtar kasasında bir parola oluşturmak için ise **kasalar/parolalar** adı verilen bir kaynak türü sunar. Ayrıca [Anahtar Kasası REST API’si işlemleri](https://msdn.microsoft.com/library/azure/dn903609.aspx) aracılığıyla işlemler sağlar. REST API’sini doğrudan çağırabilir veya [Anahtar Kasası PowerShell cmdlet’leri](https://msdn.microsoft.com/library/dn868052.aspx) ile [Anahtar Kasası Azure CLI](./key-vault/key-vault-manage-with-cli.md) kullanarak anahtar kasasını yönetebilirsiniz. Ayrıca en fazla kaynak ile çalışmak için birkaç programlama dili kullanabilirsiniz. Daha fazla bilgi için bkz. [SDK’lar ve örnekler](#sdks-and-samples). 

Altyapınızı dağıtmak ve yönetmek için, kaynak sağlayıcıları ile ilgili hangi kaynak türlerini sunduğunu, REST API işlemlerinin sürüm numaralarını, desteklediği işlemleri ve oluşturulacak kaynak türünün değerlerini belirlerken kullanılacak şema gibi konularda ayrıntılı bilgi sahibi olmanız gerekir. Desteklenen kaynak sağlayıcıları hakkında bilgi edinmek için bkz. [Resource Manager sağlayıcıları, bölgeleri, API sürümleri ve şemaları](resource-manager-supported-services.md).

## Şablon dağıtımı

Resource Manager’ı kullanarak uygulamanızdaki dağıtımı ve yapılandırmayı tanımlayan basit bir şablon (JSON biçiminde) oluşturabilirsiniz. Bir şablon kullanarak uygulamanızı yaşam döngüsü boyunca defalarca dağıtabilirsiniz. Kaynaklarınızın tutarlı bir durumda dağıtılması da size güven verir. Azure Resource Manager kaynakların doğru sırada oluşturulmasını sağlamak için bağımlılıkları analiz eder. Daha fazla bilgi için bkz. [Azure Resource Manager’da bağımlılıkları tanımlama](resource-group-define-dependencies.md).

Portaldan bir çözüm oluşturduğunuzda çözüm otomatik olarak bir dağıtım şablonu içerir. Bir şablonla başlayacağınız ve bu şablonu size özel ihtiyaçlara göre özelleştirebileceğiniz için yeni bir şablon oluşturmanız gerekmez. Kaynak grubunun mevcut durumunu bir şablona aktararak veya belirli bir dağıtım için kullanılan şablonu görüntüleyerek mevcut kaynak grubu için bir şablon elde edebilirsiniz. Dışarı aktarılan şablonu görüntülemek şablon söz dizimi hakkında bilgi edinmek için yararlı bir yoldur. Dışarı aktarılan şablonları ile çalışma hakkında daha fazla bilgi edinmek için bkz. [Mevcut kaynaklardan Azure Resource Manager şablonu aktarma](resource-manager-export-template.md).

Bütün altyapınızı tek bir şablonda tanımlamak zorunda değilsiniz. Genellikle, en uygun seçenek dağıtım gereksinimlerinizi hedeflenen, amaca yönelik bir dizi şablona bölüştürmektir. Bu şablonları farklı çözümler için kolayca yeniden kullanabilirsiniz. Belirli bir çözümü dağıtmak için gerekli tüm şablonların bağlandığı bir ana şablon oluşturun. Daha fazla bilgi için bkz. [Azure Resource Manager ile bağlı şablonları kullanma](resource-group-linked-templates.md).

Ayrıca, şablonu altyapıda yapılan güncelleştirmeler için kullanabilirsiniz. Örneğin, uygulamanız için yeni bir kaynak ekleyebilir ve zaten dağıtılmış kaynaklar için yapılandırma kuralları ekleyebilirsiniz. Şablon zaten olan bir kaynak için yeni bir kaynak oluşturulmasını belirtiyorsa, Azure Resource Manager yeni bir varlık oluşturmak yerine bir güncelleştirme gerçekleştirir. Azure Resource Manager varlığı yeni oluşturulacak kaynak ile aynı duruma gelecek şekilde güncelleştirir. Resource Manager’ın şablonda belirtilmeyen tüm kaynakları silmesini de belirtebilirsiniz. Dağıtım işlemi sırasında sunulan farklı seçeneklere göz atmak için bkz. [Azure Resource Manager şablonu ile bir uygulamayı dağıtma](resource-group-template-deploy.md). 

Dağıtımın özelleştirilmesini sağlamak ve esneklik katmak için şablonunuzdaki parametreleri belirtebilirsiniz. Örneğin, dağıtımı test ortamınız için uygun hale getirecek parametre değerlerinin geçmesini sağlayabilirsiniz. Parametreleri belirterek tüm uygulama ortamlarınızda dağıtım için aynı şablonu kullanabilirsiniz.

Resource Manager, kurulumun içermediği belirli bir yazılımı yükleme gibi ek işlemlere ihtiyaç duymanız halinde size uzantılar sunar. Zaten DSC, Chef veya Puppet gibi bir yapılandırma yönetim hizmeti kullanıyorsanız, uzantıları kullanarak bu hizmetle çalışmaya devam edebilirsiniz.

Son olarak, uygulamanızın kaynak kodunun bir parçası haline gelir. Bunu kaynak kod deponuza dahil edebilir ve uygulamanız geliştikçe buna göre güncelleştirebilirsiniz. Visual Studio üzerinden şablonu düzenleyebilirsiniz.

Şablon tanımlama hakkında daha fazla bilgi için bkz. [Azure Resource Manager Şablonları Yazma](resource-group-authoring-templates.md).

Şablon oluşturmanın adım adım yönergeleri için bkz. [Resource Manager Şablonu Kılavuzu](resource-manager-template-walkthrough.md).

Çözümünüzü farklı ortamlarda dağıtmaya yönelik kılavuz için bkz. [Microsoft Azure’da geliştirme ve test ortamları](solution-dev-test-environments.md). 

## Etiketler

Resource Manager, kaynakları yönetme ve fatura gereksinimlerine göre kategorize etmenize olanak tanıyan bir etiketleme özelliği sunar. Karmaşık bir kaynak grubu ve kaynak koleksiyonunuz olduğunda ve bu varlıkları sizin için anlamlı bir şekilde görselleştirmeniz gerektiğinde etiketleri kullanmak isteyebilirsiniz. Örneğin, kuruluşunuzda benzer görevleri üstlenen veya aynı departmana ait olan kaynakları etiketleyebilirsiniz. Kuruluşunuzdaki kullanıcılar etiketleri kullanmadan birden fazla kaynak oluşturduğunda, bunları daha sonra tanımlamak ve yönetmek oldukça zor olabilir. Örneğin, belirli bir projenin tüm kaynaklarını silmek isterseniz, ancak bu kaynaklar bu proje için etiketlenmemişse, bunları el ile bulmak zorunda kalırsınız. Etiketleme, aboneliğinizden doğan gereksiz maliyetleri azaltmanın önemli bir yoludur. 

Kaynakların bir etiketi paylaşması için aynı kaynak grubunda bulunmaları gerekmez. Kuruluşunuzdaki tüm kullanıcıların yanlışlıkla birbirinden kısmen farklı etiketler (örneğin “departman” yerine “depart.”) kullanmak yerine genel etiketler kullanmasını sağlamak için kendi etiket sınıflandırmanızı oluşturabilirsiniz.

Etiketler hakkında daha fazla bilgi için bkz. [Etiketleri kullanarak Azure kaynaklarınızı düzenleme](resource-group-using-tags.md). Dağıtım sırasında kaynaklara etiket eklenmesini gerektiren bir [özelleştirilmiş ilke](#manage-resources-with-customized-policies) oluşturabilirsiniz.

## Erişim denetimi

Resource Manager, belirli eylemlere kuruluşunuzda kimlerin erişebildiğini denetlemenize olanak tanır. Rol Tabanlı Erişim Denetimi’ni (RBAC) doğrudan yönetim platformu ile tümleştirir ve bu erişim denetimini kaynak grubunuzdaki tüm hizmetlere uygular. Kullanıcıları önceden tanımlanmış platform ve kaynağa özel rollere ekleyebilir ve bu rolleri erişimi kısıtlamak üzere bir aboneliğe, kaynak grubuna ya da kaynağa uygulayabilirsiniz. Örneğin, önceden tanımlanmış SQL DB Katılımcısı adında bir rolü kullanarak kullanıcıların veritabanlarını yönetmelerine izin verebilirsiniz. Ancak, bu özellik veritabanı sunucuları veya güvenlik ilkeleri için geçerli değildir.  Kuruluşunuzda SQL DB Katılımcısı rolüne bu tür bir erişimin gerektiği kullanıcılar ekler ve bu rolü aboneliğe, kaynak grubuna ya da kaynağa uygularsınız.

Resource Manager denetleme amacıyla kullanıcı eylemlerini otomatik olarak günlüğe kaydeder. Denetim günlükleri ile çalışma hakkında daha fazla bilgi için bkz. [Resource Manager denetim işlemleri](resource-group-audit.md).

Rol tabanlı erişim denetimi hakkında daha fazla bilgi için bkz. [Azure Rol Tabanlı Erişim Denetimi](./active-directory/role-based-access-control-configure.md). [RBAC: Yerleşik Roller](./active-directory/role-based-access-built-in-roles.md) konusunda, yerleşik rollerin ve izin verilen eylemlerin bir listesi bulunur. Yerleşik roller kullanılabilir olanlara birkaç örnek vermek gerekirse; Sahip, Okuyucu ve Katkıda Bulunan rollerinin yanı sıra Sanal Makine Katılımcısı, Sanal Ağ Katılımcısı ve SQL Güvenlik Yöneticisi gibi hizmete özel roller içerir.

Kullanıcıların kritik kaynakları silmesini ve değiştirmesini önlemek için bunları açıkça kilitleyebilirsiniz. Daha fazla bilgi için bkz. [Azure Resource Manager ile kaynakları kilitleme](resource-group-lock-resources.md).

En iyi uygulamalar için bkz. [Azure Resource Manager güvenliği ile ilgili dikkat edilmesi gerekenler](best-practices-resource-manager-security.md)

## Özelleştirilmiş ilkeler ile kaynakları yönetme

Resource Manager kaynaklarınızı yönetmek üzere özelleştirilmiş ilkeler oluşturmanıza olanak tanır Kaynaklarda adlandırma kuralı kullanılmasını zorlama, hangi kaynak türlerinin ve örneklerinin dağıtılabileceğini kısıtlama, hangi bölgelerin bir kaynak türü barındırabileceğini kısıtlama veya departmanlara göre faturaların düzenlenmesi için kaynaklarda etiket değeri gerektirme gibi senaryolar için ilke türleri oluşturabilirsiniz. Aboneliğinizin maliyetlerini düşürmeye ve tutarlılık sağlanmasına yardımcı olmak üzere ilkeler oluşturabilirsiniz. Daha fazla bilgi için bkz. [Kaynakları yönetmek ve erişimi denetlemek için İlke kullanma](resource-manager-policy.md).

## Tutarlı yönetim katmanı

Resource Manager Azure PowerShell, Mac, Linux ve Windows için Azure CLI, Azure portalı veya REST API aracılığıyla tamamen uyumlu işlemler sağlar. Sizin için en iyi arabirimi kullanabilir ve karışıklık olmadan arabirimler arasında hızlı bir şekilde geçiş yapabilirsiniz. Üstelik, portal dışında gerçekleştirilen eylemler için portalda bildirim görüntülenir.

PowerShell ile ilgili daha fazla bilgi için bkz. [Resource Manager ile Azure PowerShell kullanma](powershell-azure-resource-manager.md) ve [Azure Resource Manager Cmdlet'leri](https://msdn.microsoft.com/library/azure/dn757692.aspx).

Azure CLI hakkında daha fazla bilgi için bkz. [Azure Kaynak Yönetimi ile Mac, Linux ve Windows için Azure CLI’yı kullanma](xplat-cli-azure-resource-manager.md).

REST API hakkında daha fazla bilgi için bkz. [Azure Resource Manager REST API Başvurusu](https://msdn.microsoft.com/library/azure/dn790568.aspx). Dağıtılmış kaynaklarınızın REST işlemlerini görüntülemek için bkz. [Kaynakları görüntülemek ve değiştirmek için Azure Kaynak Gezgini’ni kullanma](resource-manager-resource-explorer.md).

Portal kullanımı hakkında bilgi için bkz. [Kaynakları Resource Manager şablonları ve Azure portalı ile dağıtma](resource-group-template-deploy-portal.md).

Azure Resource Manager çıkış noktaları arası kaynak paylaşımını (CORS) destekler. CORS ile bir web uygulamasından farklı bir etki alanında bulunan Resource Manager REST API’si veya bir Azure hizmeti REST API'si çağırabilirsiniz. CORS desteği olmadan web tarayıcısı bir etki alanındaki uygulamanın başka bir etki alanındaki kaynaklara erişmesini engeller. Resource Manager, geçerli kimlik doğrulama kimlik bilgilerine sahip tüm istekler için CORS’u etkinleştirir.

## SDK'lar ve örnekler

Azure SDK'ları birden çok dil ve platform için kullanılabilir.
Bu dil uygulamalarının her biri ekosistem paket yöneticisi ve GitHub üzerinden kullanılabilir.

Bu SDK’ların her birindeki kod Azure RESTful API belirtimlerinden oluşturulur.
Bu belirtimler açık kaynaklıdır ve Swagger 2.0 belirtimini temel alır.
SDK kodu, AutoRest adlı açık kaynaklı bir proje aracılığıyla oluşturulur.
AutoRest bu RESTful API belirtimlerini birden fazla dilde istemci kitaplıklarına dönüştürür.
SDK’larda oluşturulan kodu herhangi bir açıdan geliştirmek isterseniz SDK’ları oluşturmaya yönelik tüm araçlar açıktır, ücretsiz olarak kullanılabilir ve geniş ölçekte benimsenen bir API belirtim biçimini temel alır.

**Örnekler**: Tercih ettiğiniz dilde hızlı bir şekilde kullanmaya başlayın.

- [.NET](https://azure.microsoft.com/documentation/samples/?service=azure-resource-manager&platform=dotnet)
- [Java](https://azure.microsoft.com/documentation/samples/?service=azure-resource-manager&platform=java)
- [Node.js](https://azure.microsoft.com/documentation/samples/?service=azure-resource-manager&platform=nodejs)
- [Python](https://azure.microsoft.com/documentation/samples/?service=azure-resource-manager&platform=python)
- [PHP](https://azure.microsoft.com/documentation/samples/?service=azure-resource-manager&platform=php) *yakında*
- [Ruby](https://azure.microsoft.com/documentation/samples/?service=azure-resource-manager&platform=ruby)

**Açık Kaynak SDK depoları**: geri bildirimler, sorunlar ve çekme istekleri memnuniyetle karşılanır.

- [.NET](https://github.com/Azure/azure-sdk-for-net)
- [Java](https://github.com/Azure/azure-sdk-for-java)
- [Node.js](https://github.com/Azure/azure-sdk-for-node)
- [PHP](https://github.com/Azure/azure-sdk-for-php)
- [Python](https://github.com/Azure/azure-sdk-for-python)
- [Ruby](https://github.com/Azure/azure-sdk-ruby)

> [AZURE.NOTE] SDK gerekli işlevleri sağlamıyorsa doğrudan [Azure REST API'sine](https://msdn.microsoft.com/library/azure/dn790568.aspx) de çağrı yapabilirsiniz.

## Sonraki adımlar

- Şablonlar ile çalışmaya genel bir giriş yapmak için bkz. [Mevcut kaynaklardan Azure Resource Manager şablonu aktarma](resource-manager-export-template.md).
- Şablon oluşturmayla ilgili daha kapsamlı bir kılavuz için bkz. [Resource Manager Şablonu Kılavuzu](resource-manager-template-walkthrough.md).
- Bir şablonda kullanabileceğiniz işlevleri anlamak için bkz. [Şablon işlevleri](resource-group-template-functions.md)
- Resource Manager ile Visual Studio’yu kullanma hakkında bilgi edinmek için bkz. [Azure kaynak gruplarını Visual Studio aracılığıyla oluşturma ve dağıtma](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).
- Resource Manager ile VS Code kullanma hakkında bilgi için bkz. [Visual Studio Code’da Azure Resource Manager Şablonları ile Çalışma](resource-manager-vs-code.md).

Bu genel bakışın sunulduğu videoya şuradan ulaşabilirsiniz:

[AZURE.VIDEO azure-resource-manager-overview]



<!--HONumber=Aug16_HO1-->


