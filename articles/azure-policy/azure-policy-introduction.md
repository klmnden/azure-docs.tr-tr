---
title: Azure İlkesine Genel Bakış
description: Azure İlkesi, Azure ortamında ilke tanımlarınızı oluşturmak, atamak ve yönetmek için kullandığınız bir Azure hizmetidir.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 07/31/2018
ms.topic: overview
ms.service: azure-policy
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: 405f69ae1c37e478758d984ddf7dc0e267910fef
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/17/2018
ms.locfileid: "42023549"
---
# <a name="what-is-azure-policy"></a>Azure İlkesi nedir?

BT Yönetimi, kuruluşunuzun verimli ve etkili BT kullanımıyla hedeflerine ulaşmasını sağlar. Bunun için işletmenizin hedefleriyle BT projeleriniz arasında net bir çizgi çizer.

Şirketiniz asla çözülmeyecek gibi görünen önemli sayıda BT sorunlarıyla mi karşılaşıyor?
İyi bir BT yönetimine sorunların yönetilmesine ve önlenmesine yardımcı olma amacıyla girişimlerinizi planlama ve önceliklerinizi stratejik düzeyde belirleme dahildir. Azure İlkesi tam da bu noktada devreye girer.

Azure İlkesi, ilkelerinizi oluşturmak, atamak ve yönetmek için kullandığınız bir Azure hizmetidir. Bu ilkeler, kaynaklarınız üzerinden farklı kuralları ve eylemleri uygulatarak kaynaklarınızın kurumsal standartlarınız ve hizmet düzeyi sözleşmelerinizle uyumlu kalmasını sağlar. Azure İlkesi bunun için kaynaklarınızı değerlendirir ve oluşturduğunuz ilkelere uygun olmayanları bulur. Örneğin, ortamınızda yalnızca belirli SKU boyutuna sahip sanal makinelere izin veren bir ilkeniz olabilir. Bu ilke uygulandıktan sonra kaynak oluşturma ve güncelleştirme adımlarının yanı sıra önceden oluşturduğunuz kaynaklar için de değerlendirilir. Bu belgenin ilerleyen bölümlerinde Azure İlkesi ile ilke oluşturma ve uygulama konuları daha ayrıntılı bir şekilde anlatılmaktadır.

> [!IMPORTANT]
> Azure İlkesi’nin uyumluluk değerlendirmesi artık fiyatlandırma katmanından bağımsız olarak tüm atamalara sağlanır. Atamalarınız uyumluluk verilerini göstermezse, lütfen aboneliğin Microsoft.PolicyInsights kaynak sağlayıcısı ile kaydolduğundan emin olun.

## <a name="how-is-it-different-from-rbac"></a>RBAC ile farkları nelerdir?

İlke ve rol tabanlı erişim denetimi (RBAC) arasında bazı önemli farklar vardır. RBAC farklı kapsamlardaki kullanıcı eylemlerine odaklanır. Örneğin, istenilen kapsamda bir kaynak grubu için katkıda bulunan rolüne eklenmiş olabilirsiniz. Bu rol, kaynak grubunda değişiklik yapmanıza olanak verir.
İlke, dağıtım sırasında ve zaten varolan kaynaklar için kaynak özelliklerine odaklanır. Örneğin, sağlanabilen kaynak türlerini ilkeler aracılığıyla denetleyebilirsiniz. Dilerseniz kaynakların sağlanabileceği konumları kısıtlayabilirsiniz. RBAC’nin aksine, ilke, varsayılan bir izin ve açık reddetme sistemidir.

### <a name="rbac-permissions-in-azure-policy"></a>Azure İlkesi'ndeki RBAC İzinleri

Azure İlkesi, iki farklı Kaynak Sağlayıcısındaki işlemler olarak temsil edilen izinlere sahiptir:

- [Microsoft.Authorization](../role-based-access-control/resource-provider-operations.md#microsoftauthorization)
- [Microsoft.PolicyInsight](../role-based-access-control/resource-provider-operations.md#microsoftpolicyinsights)

Yerleşik rollerin birçoğu Azure İlkesi kaynaklarında farklı izin düzeylerine sahiptir. Örneğin **Güvenlik Yöneticisi** ilke atamalarını ve tanımlarını yönetebilir ancak uyumluluk bilgilerini görüntüleyemez. **Okuyucu** ise ilke atamaları ve tanımlarıyla ilgili ayrıntılı bilgileri okuyabilir ancak uyumluluk bilgilerini görüntüleyemez ve üzerinde değişiklik yapamaz. **Sahip** tüm haklara sahipken **Katılımcı**, herhangi bir Azure İlkesi iznine sahip değildir. İlke uyumluluk ayrıntılarını görüntüleme izni vermek için [özel rol](../role-based-access-control/custom-roles.md) oluşturun.

## <a name="policy-definition"></a>İlke tanımı

Azure İlkesi'nde bir ilke oluşturmak ve uygulamak için önce ilke tanımını oluşturmanız gerekir. Her ilke tanımında, bu ilkelerin uygulandığı koşullar bulunur. Koşullar karşılandığında gerçekleşmek üzere eşlik eden bir eyleme de sahiptir.

Azure İlkesi'nde, varsayılan olarak kullanabileceğiniz bazı yerleşik ilkeler sunuyoruz. Örnek:

- **SQL Server 12.0 gerektirir**: Bu ilke tanımı, tüm SQL sunucularının 12.0 sürümünü kullanmasını sağlamayı amaçlayan koşullara veya kurallara sahiptir. Sahip olduğu eylem ise bu ölçütleri karşılamayan tüm sunucuları reddetmektir.
- **İzin Verilen Depolama Hesabı SKU’ları**: Bu ilke tanımı, dağıtılan bir depolama hesabının SKU boyutları kümesine dahil olup olmadığını belirleyen koşullara veya kurallara sahiptir. Sahip olduğu eylem ise tanımlı SKU boyutları kümesine bağlı kalmayan tüm depolama hesaplarını reddetmektir.
- **İzin Verilen Kaynak Türü**: Bu ilke tanımı, kuruluşunuzun dağıtabileceği kaynak türlerini belirleyen koşullara veya kurallara sahiptir. Sahip olduğu eylem ise bu tanımlı listenin bir parçası olmayan tüm kaynakları reddetmektir.
- **İzin Verilen Konumlar**: Bu ilke, kuruluşunuzun kaynakları dağıtırken belirleyebileceği konumları kısıtlamanıza olanak verir. Sahip olduğu eylem ise coğrafi uyumluluk gereksinimlerinizi uygulamaktır.
- **İzin Verilen Sanal Makine SKU’ları**: Bu ilke, kuruluşunuzun dağıtabileceği sanal makine SKU’ları kümesini belirlemenize olanak verir.
- **Uygulama etiketi ve varsayılan değeri**: Bu ilke, kullanıcı tarafından belirlenmediyse, gerekli etiketi ve varsayılan değerini uygular.
- **Etiket ve değerini zorunlu kılma**: Bu ilke gerekli bir etiket ve değerini bir kaynağa zorunlu kılar.
- **İzin verilmeyen kaynak türleri**: Bu ilke, kuruluşunuzun dağıtamayacağı kaynak türlerini belirlemenize olanak verir.

Bu ilke tanımlarını (hem yerleşik hem de özel tanımlar) uygulamak için atamanız gerekir. Bu ilkelerden herhangi birini Azure portalı, PowerShell veya Azure CLI üzerinden atayabilirsiniz.

İlke yeniden değerlendirme işlemlerinin saatte bir gerçekleştiğini ve bu nedenle ilkeyi uyguladıktan (ilke ataması oluşturduktan) sonra ilke tanımında değişiklik yapmanız durumunda kaynaklarınızda bir saat sonra yeniden değerlendirme yapılacağını unutmayın.

İlke tanımlarının yapıları hakkında daha fazla bilgi edinmek için [İlke Tanımı Yapısı](policy-definition.md) adlı makaleye göz atın.

## <a name="policy-assignment"></a>İlke ataması

İlke ataması, belirli bir kapsamda gerçekleşmesi için atanmış bir ilke tanımıdır. Bu kapsamın dahilinde [yönetim gruplarından](../azure-resource-manager/management-groups-overview.md) kaynak gruplarına kadar birçok grup bulunabilir. *Kapsam*, ilke tanımının atandığı tüm kaynak gruplarını, abonelikleri veya yönetim gruplarını ifade eder. İlke atamaları, tüm alt kaynaklar tarafından devralınır. Bu da bir kaynak grubu için bir ilke uygulandığında, bu kaynak grubundaki tüm kaynaklara uygulanacağı anlamına gelir. Ancak, dilerseniz bir alt kapsamı ilke atamasından dışlayabilirsiniz.

Örneğin abonelik kapsamında, ağ kaynaklarının oluşturulmasını önleyen bir ilke atayabilirsiniz. Ancak, ağ alt yapısı için hedeflenen bir abonelikten bir kaynak grubunu dışlayabilirsiniz. Ağ kaynaklarını oluşturma konusunda güvendiğiniz kullanıcılara bu ağ kaynak grubuna erişim hakkı sağlayabilirsiniz.

Başka bir örnekte, yönetim grubu düzeyinde bir kaynak türü beyaz listeye alma ilkesi atamak isteyebilirsiniz. Daha sonra bir alt yönetim grubunda veya doğrudan aboneliklerde daha esnek bir ilke (daha fazla kaynak türüne izin veren) atayın. Ancak ilke, açık bir reddetme sistemi olduğundan bu örnek çalışmaz. Bunun yerine, alt yönetim grubunu veya aboneliğini yönetim grubu düzeyinde ilke atamasının dışında bırakmanız gerekir. Daha sonra alt yönetim grubunda veya abonelik düzeyinde daha esnek ilkeyi atayın. Özetlemek gerekirse, herhangi bir ilke bir kaynağın reddedilmesiyle sonuçlanırsa kaynağa izin vermenin tek yolu, reddetme ilkesinin değiştirilmesidir.

İlke tanımlarını ve atamalarını ayarlama hakkında daha fazla bilgi için bkz. [Azure ortamınızdaki uyumlu olmayan kaynakları tanımlamak için bir ilke ataması oluşturma](assign-policy-definition.md).

## <a name="policy-parameters"></a>İlke parametreleri

İlke parametreleri, oluşturmanız gereken ilke tanımlarının sayısını azaltarak ilke yönetiminizi basitleştirmeye yardımcı olur. İlke tanımı oluştururken parametreleri belirleyerek bunları daha genel hale getirebilirsiniz. Daha sonra bu ilke tanımını farklı senaryolar için kullanabilirsiniz. Bu işlemi, ilke tanımlarının atamasını yaparken farklı değerler girerek gerçekleştirebilirsiniz. Örneğin, abonelik için bir konum kümesi belirtme.

Parametreler, bir ilke tanımı oluşturulurken tanımlanır veya oluşturulur. Tanımlanan parametreye bir ad ve isteğe bağlı olarak bir değer verilir. Örneğin, *konum* başlıklı bir ilke için parametre tanımlayabilirsiniz. Daha sonra, ilkenin atamasını yaparken *EastUS* veya *WestUS* gibi farklı değerler verebilirsiniz.

İlke parametreleri hakkında daha fazla bilgi için bkz. [Kaynak İlkesine Genel Bakış - Parametreler](policy-definition.md#parameters).

## <a name="initiative-definition"></a>Girişim tanımı

Girişim tanımı, tekil kapsamlı bir hedefi gerçekleştirmek üzere belirlenmiş ilke tanımlarının bir koleksiyonudur. Girişim tanımları, ilke tanımlarının yönetimini ve atanmasını basitleştirir. İlke kümelerini gruplandırıp tek bir öğe haline getirerek basitleştirirler. Örneğin, Azure Güvenlik Merkezinizdeki tüm kullanılabilir güvenlik önerilerini izlemeyi hedefleyen **Azure Güvenlik Merkezi'nde İzlemeyi Etkinleştirme** başlıklı bir girişim oluşturabilirsiniz.

Bu girişimin altında sahip olabileceğiniz ilke tanımlarından bazıları şunlardır:

- **Güvenlik Merkezi’ndeki şifrelenmemiş SQL Veritabanı’nı izleme** – Şifrelenmemiş SQL veritabanlarını ve sunucuları izlemek için.
- **Güvenlik Merkezi'ndeki işletim sistemi güvenlik açıklarını izleme** – Yapılandırılmış ana hattı karşılamayan sunucuları izlemek için.
- **Güvenlik Merkezi’ndeki eksik Endpoint Protection’ı izleme** – Yüklü bir bitiş noktası koruma aracısı olmadan sunucuları izlemek için.

## <a name="initiative-assignment"></a>Girişim ataması

İlke ataması gibi, girişim ataması da belirli bir kapsama atanmış olan girişim tanımıdır. Girişim atamaları, her kapsam için birkaç girişim tanımları yapma ihtiyacını azaltır. Bu kapsamın dahilinde de yönetim gruplarından kaynak gruplarına kadar birçok grup bulunabilir.

Önceki örnekten, **Azure Güvenlik Merkezi'nde İzlemeyi Etkinleştirme** girişimi de farklı kapsamlara atanabilir. Örneğin, bir atama **subscriptionA** aboneliğine atanabilir.
Bir başkası ise **subscriptionB** aboneliğine atanabilir.

## <a name="initiative-parameters"></a>Girişim parametreleri

İlke parametreleri gibi, girişim parametreleri de fazlalıkları azaltarak girişim yönetiminin basitleştirilmesine yardımcı olur. Girişim parametreleri temelde girişim dahilindeki ilke tanımları tarafından kullanılan parametrelerin listesidir.

Örneğin **initiativeC** adına her biri farklı bir parametre türü bekleyen **policyA** ve **policyB** ilke tanımlarına sahip olan bir girişim tanımına sahip olduğunuzu düşünelim:

| İlke | Parametrenin adı |Parametrenin türü  |Not |
|---|---|---|---|
| policyA | allowedLocations | array  |Parametre türü “array” olarak tanımlandığından bu parametre, bir değer için dizilerin bulunduğu bir liste bekler |
| policyB | allowedSingleLocation |string |Parametre türü “array” olarak tanımlandığından bu parametre, bir değer için bir sözcük bekler |

Bu senaryoda **initiativeC** için girişim parametreleri tanımlanırken üç seçeneğiniz vardır:

- Bu girişim dahilinde ilke tanımlarının parametrelerini kullanın: Bu örnekte, *allowedLocations* ve *allowedSingleLocation*, **initiativeC** için girişim parametreleri olur.
- Bu girişim tanımındaki ilke tanımlarının parametrelerine değerler sağlayın. Bu örnekte, **policyA’nın parametresi – allowedLocations** ve **policyB’nin parametresi – allowedSingleLocation** için konum listesi sağlayabilirsiniz. Bu girişimin atamasını yaparken değerleri de sağlayabilirsiniz.
- Bu girişimin atamasını yaparken kullanılabilecek bir *değer* seçenekleri listesi sağlayın. Bu girişimi atadığınızda, girişim dahilindeki ilke tanımlarından alınan parametreler yalnızca bu sağlanan listedeki değerleri alabilir.

Örneğin, *EastUS*, *WestUS*, *CentralUS*, ve *WestEurope* içeren bir girişim tanımındaki değer seçeneklerinin listesini oluşturabilirsiniz. Öyleyse, listenin bir parçası olmadığı için girişim ataması sırasında *Southeast Asia* gibi farklı bir değer giremezsiniz.

## <a name="maximum-count-of-policy-objects"></a>İlke nesnelerinin maksimum sayısı

[!INCLUDE [policy-limits](../../includes/azure-policy-limits.md)]

## <a name="recommendations-for-managing-policies"></a>İlkeleri yönetme ile ilgili öneriler

İlke tanımlarını ve atamalarını yönetirken ve oluştururken izlemenizi ve aklınızda tutmanızı önerdiğimiz birkaç noktayı burada bulabilirsiniz:

- Ortamınızda ilke tanımları oluşturuyorsanız, ilke tanımlarınızın ortamınızdaki kaynaklar üzerindeki etkisini izleyebilmek için reddetme etkisinin aksine bir denetim etkisiyle başlamanızı öneririz. Uygulamalarınızı otomatik olarak ölçeklendirmek için komut dosyalarınız zaten varsa, reddetme etkisi belirlemek varolan bu otomatikleştirme görevlerini sekteye uğratabilir.
- Tanımları ve atamaları oluştururken kuruluş hiyerarşilerini göz önünde bulundurmanız önemlidir. Tanımları, yönetim grubu veya abonelik düzeyi ve sonraki alt düzeyde atama yapma gibi daha yüksek bir düzeyde oluşturmanızı öneririz. Örneğin, yönetim grubu düzeyinde bir ilke tanımı oluşturursanız, bu tanımın ilke atamasının kapsamı bu yönetim grubu dahilindeki abonelik düzeyine azaltılabilir.
- Aklınızda yalnızca bir ilke olsa bile, her zaman ilke tanımları yerine girişim tanımlarını kullanmanızı öneririz. Örneğin, bir ilke tanımınız (*policyDefA*) varsa ve bunu girişim tanımı (*initiativeDefC*) altında oluşturursanız, daha sonra *policyDefA* ilkesindekine benzer hedeflerle *policyDefB* için başka bir ilke tanımı oluşturmaya karar verdiğinizde, bunu *initiativeDefC* altına ekleyip daha iyi bir şekilde izleyebilirsiniz.
- Bir ilke tanımından ilke ataması oluşturduğunuzda, girişim tanımına eklenen herhangi bir yeni ilke tanımı otomatik olarak girişim tanımının altındaki girişim atamasının altında toplanır.
- Girişim ataması tetiklendiğinde girişimde bulunan tüm ilkeler de tetiklenir. Ancak, bir ilkeyi tek başına yürütmeniz gerekiyorsa, ilkeyi bir girişime eklememeniz önerilir.

## <a name="video-overview"></a>Genel Bakış Videosu

Aşağıdaki Azure İlkesi genel bakış videosu Build 2018 etkinliğinde kaydedilmiştir. Slaytları veya videoyu indirmek için lütfen Channel 9'daki [Govern your Azure environment through Azure Policy](https://channel9.msdn.com/events/Build/2018/THR2030) (Azure ortamınızı Azure İlkesi aracılığıyla yönetme) sayfasına gidin.

> [!VIDEO https://channel9.msdn.com/events/Build/2018/THR2030/player]

## <a name="next-steps"></a>Sonraki adımlar

Artık Azure İlkesi ve bazı önemli kavramlar ile ilgili bir genel bakışa sahipsiniz, önerdiğimiz diğer adımları aşağıda bulabilirsiniz:

- [İlke tanımı atama](assign-policy-definition.md)
- [Azure CLI kullanarak bir ilke tanımı atama](assign-policy-definition-cli.md)
- [PowerShell kullanarak bir ilke tanımı atama](assign-policy-definition-ps.md)
- [Kaynaklarınızı Azure yönetim gruplarıyla düzenleme](../azure-resource-manager/management-groups-overview.md) bölümünde yönetim gruplarını gözden geçirebilirsiniz
- Channel 9'daki [Govern your Azure environment through Azure Policy](https://channel9.msdn.com/events/Build/2018/THR2030) (Azure ortamınızı Azure İlkesi aracılığıyla yönetme) sayfasını görüntüleyin