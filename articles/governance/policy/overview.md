---
title: Azure İlkesine Genel Bakış
description: Azure İlkesi, Azure ortamında ilke tanımlarınızı oluşturmak, atamak ve yönetmek için kullandığınız bir Azure hizmetidir.
author: DCtheGeek
ms.author: dacoulte
ms.date: 12/06/2018
ms.topic: overview
ms.service: azure-policy
manager: carmonm
ms.openlocfilehash: 2dd31ab29479fade21d27b8e2c23952f905f530a
ms.sourcegitcommit: 59fd8dc19fab17e846db5b9e262a25e1530e96f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65979151"
---
# <a name="overview-of-the-azure-policy-service"></a>Azure İlkesi hizmetine genel bakış

İdare doğrular, kuruluşunuzun hedeflerine ulaşmak etkili ve verimli kullanımı aracılığıyla elde edebilirsiniz BT. Bu, iş hedefleri ve BT projeleri arasında netlik oluşturarak bu gereksinimi karşılayan.

Şirketiniz asla çözülmeyecek gibi görünen önemli sayıda BT sorunlarıyla mi karşılaşıyor?
İyi bir BT yönetimine sorunların yönetilmesine ve önlenmesine yardımcı olma amacıyla girişimlerinizi planlama ve önceliklerinizi stratejik düzeyde belirleme dahildir. Stratejik bu gereksinim, Azure İlkesi burada devreye girer.

Azure İlkesi, ilkelerinizi oluşturmak, atamak ve yönetmek için kullandığınız bir Azure hizmetidir. Bu ilkeler, kaynaklarınız üzerinden farklı kuralları ve eylemleri uygulatarak kaynaklarınızın kurumsal standartlarınız ve hizmet düzeyi sözleşmelerinizle uyumlu kalmasını sağlar. Azure İlkesi, uyumsuzluk atanan ilkelerle kaynaklarınızı değerlendirerek bu gereksinimini karşılar. Örneğin, ortamınızda yalnızca belirli SKU boyutuna sahip sanal makinelere izin veren bir ilkeniz olabilir. Bu ilke uygulandıktan sonra yeni ve mevcut kaynakları uyumluluk için değerlendirilir. Doğru ilke türü ile var olan kaynakları uyumlu hale getirilebilir. Bu belgede daha sonra oluşturun ve Azure İlkesi ile ilkeleri uygulama hakkında daha fazla ayrıntıyı gideceğiz.

> [!IMPORTANT]
> Azure İlkesi’nin uyumluluk değerlendirmesi artık fiyatlandırma katmanından bağımsız olarak tüm atamalara sağlanır. Atamalarınız uyumluluk verilerini göstermezse, lütfen aboneliğin Microsoft.PolicyInsights kaynak sağlayıcısı ile kaydolduğundan emin olun.

## <a name="how-is-it-different-from-rbac"></a>RBAC ile farkları nelerdir?

Azure İlkesi ile rol tabanlı erişim denetimi (RBAC) arasında bazı önemli farklar vardır. RBAC farklı kapsamlardaki kullanıcı eylemlerine odaklanır. Bir kaynak grubu için katkıda bulunan rolüne kaynak grubunda değişiklik yapmanıza olanak sağlayan eklenmiş olabilir. Kaynakları var olan kaynak özellikleri dağıtım sırasında ve zaten için Azure İlkesi odaklanır. Azure İlkesi türleri veya kaynak konumları gibi özellikleri denetler. RBAC'nin aksine, Azure ilkesi varsayılan bir izin olduğu ve açık reddetme sistemidir.

### <a name="rbac-permissions-in-azure-policy"></a>Azure İlkesi'ndeki RBAC İzinleri

Azure İlkesi iki Kaynak Sağlayıcısı’nda işlemler olarak bilinen bazı izinlere sahiptir:

- [Microsoft.Authorization](../../role-based-access-control/resource-provider-operations.md#microsoftauthorization)
- [Microsoft.PolicyInsights](../../role-based-access-control/resource-provider-operations.md#microsoftpolicyinsights)

Birçok Yerleşik rol Azure İlkesi kaynaklarına izin verir. **Kaynak ilkesine katkıda bulunan (Önizleme)** rolü, çoğu Azure ilke işlemleri içerir. **Sahibi** tam haklarına sahip. Her ikisi de **katkıda bulunan** ve **okuyucu** tüm okuma Azure İlkesi işlemlerini kullanabilirsiniz ancak **katkıda bulunan** düzeltme de tetikleyebilirsiniz.

Yerleşik rollerin hiçbirinde gerekli izinler yoksa [özel rol](../../role-based-access-control/custom-roles.md) oluşturun.

## <a name="policy-definition"></a>İlke tanımı

Azure İlkesi'nde bir ilke oluşturmak ve uygulamak için önce ilke tanımını oluşturmanız gerekir. Her ilke tanımında, ilkelerin uygulandığı koşullar vardır. Ve bu Koşullar karşılanıyorsa, gerçekleşir, tanımlı bir etkisi.

Azure İlkesi'nde, varsayılan olarak kullanılabilen çeşitli yerleşik ilkeler sunuyoruz. Örneğin:

- **SQL Server 12.0 gerektir**: Tüm SQL sunucularının 12.0 sürümünü kullanmasını doğrular. Bu ölçütlerini sağlamayan tüm sunucuları reddetmektir kendi etkisidir.
- **İzin verilen depolama hesabı SKU'ları**: Dağıtılan bir depolama hesabı SKU boyutları bir dizi içinde olup olmadığını belirler. Etkisini tanımlı SKU boyutları kümesine bağlı olmayan tüm depolama hesapları engellemektir.
- **İzin verilen kaynak türüyle**: Dağıtabileceğiniz kaynak türlerini tanımlar. Bu tanımlı listenin bir parçası olmayan tüm kaynakları reddetmektir kendi etkisidir.
- **İzin verilen Konumlar**: Yeni kaynaklar için mevcut konumlardan kısıtlar. Sahip olduğu eylem ise coğrafi uyumluluk gereksinimlerinizi uygulamaktır.
- **Sanal makine SKU'ları izin**: Sanal makine SKU'ları dağıtabileceğiniz bir dizi belirtir.
- **Etiketi ve varsayılan değerini Uygula**: Dağıtım isteği tarafından belirtilmezse gerekli etiketi ve varsayılan değerini geçerlidir.
- **Etiketi ve değerini zorunlu kıl**: Gerekli etiketi ve değerini bir kaynağa zorunlu kılar.
- **İzin verilmeyen kaynak türleri**: Kaynak türleri listesi dağıtılmasını engeller.

Bu ilke tanımları (yerleşik ve özel tanımları) uygulamak için onları atamanız gerekir. Bu ilkelerden herhangi birini Azure portalı, PowerShell veya Azure CLI üzerinden atayabilirsiniz.

İlke atama veya ilke güncelleştirmeleri gibi çeşitli farklı eylemler ile ilke değerlendirmesi gerçekleşir. Tam bir listesi için bkz. [ilke değerlendirme Tetikleyicileri](./how-to/get-compliance-data.md#evaluation-triggers).

İlke tanımlarının yapıları hakkında daha fazla bilgi edinmek için [İlke Tanımı Yapısı](./concepts/definition-structure.md) adlı makaleye göz atın.

## <a name="policy-assignment"></a>İlke ataması

İlke ataması, belirli bir kapsamda gerçekleşmesi için atanmış bir ilke tanımıdır. Bu kapsamın dahilinde [yönetim gruplarından](../management-groups/overview.md) kaynak gruplarına kadar birçok grup bulunabilir. *Kapsam*, ilke tanımının atandığı tüm kaynak gruplarını, abonelikleri veya yönetim gruplarını ifade eder. İlke atamaları, tüm alt kaynaklar tarafından devralınır. Bu tasarım, kaynak grubundaki kaynaklar için bir kaynak grubuna uygulanan bir ilke de geçerli anlamına gelir. Ancak, dilerseniz bir alt kapsamı ilke atamasından dışlayabilirsiniz.

Örneğin abonelik kapsamında, ağ kaynaklarının oluşturulmasını önleyen bir ilke atayabilirsiniz. Bu abonelikte ağ alt yapısı için hedeflenen bir kaynak grubu hariç. Ağ kaynaklarını oluşturma konusunda güvendiğiniz kullanıcılara bu ağ kaynak grubuna erişim verin.

Başka bir örnekte, bir kaynak atamak isteyebilirsiniz listesi İlkesi Yönetim grubu düzeyinde izin verebilirsiniz. Daha sonra bir alt yönetim grubunda veya doğrudan aboneliklerde daha esnek bir ilke (daha fazla kaynak türüne izin veren) atayın. Ancak ilke, açık bir reddetme sistemi olduğundan bu örnek çalışmaz. Bunun yerine, alt yönetim grubunu veya aboneliğini yönetim grubu düzeyinde ilke atamasının dışında bırakmanız gerekir. Daha sonra alt yönetim grubunda veya abonelik düzeyinde daha esnek ilkeyi atayın. Herhangi bir ilke bir kaynağın reddedilmesiyle sonuçlanırsa kaynağa izin vermenin tek yolu reddetme ilkesinin olduğu.

Portaldan ilke tanımlarını ve atamalarını ayarlama hakkında daha fazla bilgi için bkz. [Azure ortamınızdaki uyumlu olmayan kaynakları tanımlamak için bir ilke ataması oluşturma](assign-policy-portal.md). [PowerShell](assign-policy-powershell.md) ve [Azure CLI](assign-policy-azurecli.md) adımları da mevcuttur.

## <a name="policy-parameters"></a>İlke parametreleri

İlke parametreleri, oluşturmanız gereken ilke tanımlarının sayısını azaltarak ilke yönetiminizi basitleştirmeye yardımcı olur. İlke tanımı oluştururken parametreleri belirleyerek bunları daha genel hale getirebilirsiniz. Daha sonra bu ilke tanımını farklı senaryolar için kullanabilirsiniz. Bu işlemi, ilke tanımlarının atamasını yaparken farklı değerler girerek gerçekleştirebilirsiniz. Örneğin, abonelik için bir konum kümesi belirtme.

Parametreler, bir ilke tanımı oluşturulurken tanımlanır. Tanımlanan parametreye bir ad ve isteğe bağlı olarak verilen bir değer. Örneğin, *konum* başlıklı bir ilke için parametre tanımlayabilirsiniz. Daha sonra, ilkenin atamasını yaparken *EastUS* veya *WestUS* gibi farklı değerler verebilirsiniz.

İlke parametreleri hakkında daha fazla bilgi için bkz: [tanım yapısı - parametreleri](./concepts/definition-structure.md#parameters).

## <a name="initiative-definition"></a>Girişim tanımı

Girişim tanımı, tekil kapsamlı bir hedefi gerçekleştirmek üzere belirlenmiş ilke tanımlarının bir koleksiyonudur. Girişim tanımları, ilke tanımlarının yönetimini ve atanmasını basitleştirir. İlke kümelerini gruplandırıp tek bir öğe haline getirerek basitleştirirler. Örneğin, Azure Güvenlik Merkezinizdeki tüm kullanılabilir güvenlik önerilerini izlemeyi hedefleyen **Azure Güvenlik Merkezi'nde İzlemeyi Etkinleştirme** başlıklı bir girişim oluşturabilirsiniz.

Bu girişimin altında sahip olabileceğiniz ilke tanımlarından bazıları şunlardır:

- **Güvenlik Merkezi’ndeki şifrelenmemiş SQL Veritabanı’nı izleme** – Şifrelenmemiş SQL veritabanlarını ve sunucuları izlemek için.
- **İzleme Güvenlik Merkezi'nde işletim sistemi güvenlik açıklarını** – yapılandırılmış temeli karşılamayan sunucuları izlemek için.
- **Güvenlik Merkezi’ndeki eksik Endpoint Protection’ı izleme** – Yüklü bir bitiş noktası koruma aracısı olmadan sunucuları izlemek için.

## <a name="initiative-assignment"></a>Girişim ataması

İlke ataması gibi, girişim ataması da belirli bir kapsama atanmış olan girişim tanımıdır. Girişim atamaları, her kapsam için birkaç girişim tanımları yapma ihtiyacını azaltır. Bu kapsamın dahilinde de yönetim gruplarından kaynak gruplarına kadar birçok grup bulunabilir.

Her girişim farklı kapsamlara atanabilir. Her ikisi için de bir girişim atanabilir **subscriptionA** ve **subscriptionB**.

## <a name="initiative-parameters"></a>Girişim parametreleri

İlke parametreleri gibi, girişim parametreleri de fazlalıkları azaltarak girişim yönetiminin basitleştirilmesine yardımcı olur. Girişim parametreleri, girişim dahilindeki ilke tanımları tarafından kullanılan parametrelerdir.

Örneğin **initiativeC** adına her biri farklı bir parametre türü bekleyen **policyA** ve **policyB** ilke tanımlarına sahip olan bir girişim tanımına sahip olduğunuzu düşünelim:

| İlke | Parametrenin adı |Parametrenin türü  |Not |
|---|---|---|---|
| policyA | allowedLocations | array  |Parametre türü “array” olarak tanımlandığından bu parametre, bir değer için dizilerin bulunduğu bir liste bekler |
| policyB | allowedSingleLocation |string |Parametre türü “array” olarak tanımlandığından bu parametre, bir değer için bir sözcük bekler |

Bu senaryoda **initiativeC** için girişim parametreleri tanımlanırken üç seçeneğiniz vardır:

- Bu girişim dahilinde ilke tanımlarının parametrelerini kullanın: Bu örnekte, *allowedLocations* ve *allowedSingleLocation* için girişim parametreleri **initiativeC**.
- Bu girişim tanımındaki ilke tanımlarının parametrelerine değerler sağlayın. Bu örnekte, **policyA’nın parametresi – allowedLocations** ve **policyB’nin parametresi – allowedSingleLocation** için konum listesi sağlayabilirsiniz. Bu girişimin atamasını yaparken değerleri de sağlayabilirsiniz.
- Bu girişimin atamasını yaparken kullanılabilecek bir *değer* seçenekleri listesi sağlayın. Bu girişimi atadığınızda, girişim dahilindeki ilke tanımlarından alınan parametreler yalnızca bu sağlanan listedeki değerleri alabilir.

Bir girişim tanımındaki değer seçenekleri oluştururken, listenin bir parçası olmadığı için girişim ataması sırasında farklı bir değer giriş alamıyoruz.

## <a name="maximum-count-of-azure-policy-objects"></a>Azure İlkesi nesnelerini en yüksek sayısı

[!INCLUDE [policy-limits](../../../includes/azure-policy-limits.md)]

## <a name="recommendations-for-managing-policies"></a>İlkeleri yönetme ile ilgili öneriler

İşaretçiler ve göz önünde bulundurmanız gereken birkaç şunlardır:

- Ortamınızdaki kaynaklar üzerindeki etkisini, ilke tanımı izlemek için reddetme etkisinin yerine bir denetim etkisiyle başlayın. Otomatik olarak ölçeklendirmek için komut dosyaları zaten varsa, reddetme etkisi ayarlamak, uygulamalarınıza gibi otomasyon görevleri zaten yerinde aksatabilir.

- Tanımları ve atamaları oluştururken kuruluş hiyerarşilerini göz önünde bulundurun. Yönetim grubu gibi üst düzey veya abonelik düzeyinde tanımları oluşturmanızı öneririz. Ardından, sonraki alt düzeyde atama oluşturun. Bir yönetim grubu tanımı oluşturursanız, bir abonelik veya kaynak grubu, yönetim grubu içinde aşağı atama sınırlayabilirsiniz.

- Oluşturma ve hatta tek bir ilke tanımı için girişim tanımları atama öneririz.
Örneğin, ilke tanımı sahip *policyDefA* ve girişim tanımı oluşturma *initiativeDefC*. Daha sonra başka bir ilke tanımı oluşturursanız *Policydefa* hedefleri benzer *policyDefA*, altında ekleyebilirsiniz *initiativeDefC* ve birlikte izleyin.

- Girişim ataması oluşturduktan sonra eklenen girişime ilke tanımları da bu girişim atamaları bir parçası haline gelir.

- Girişim ataması değerlendirildiğinde, girişim dahilindeki tüm ilkeleri ayrıca değerlendirilir. Ayrı ayrı bir ilke değerlendirmeniz gerekiyorsa, içinde bir girişim içermeyecek şekilde daha iyidir.

## <a name="video-overview"></a>Genel bakış videosu

Aşağıdaki Azure İlkesi genel bakış videosu Build 2018 etkinliğinde kaydedilmiştir. Slayt veya video indirme için ziyaret [Azure İlkesi aracılığıyla Azure ortamınızı yöneten](https://channel9.msdn.com/events/Build/2018/THR2030) Channel 9.

> [!VIDEO https://www.youtube.com/embed/dxMaYF2GB7o]

## <a name="next-steps"></a>Sonraki adımlar

Artık Azure İlkesi ve bazı önemli kavramlar ile ilgili bir genel bakışa sahipsiniz, önerdiğimiz diğer adımları aşağıda bulabilirsiniz:

- [Portalı kullanarak bir ilke tanımı atama](assign-policy-portal.md).
- [Azure CLI kullanarak bir ilke tanımı atama](assign-policy-azurecli.md).
- [PowerShell kullanarak bir ilke tanımı atama](assign-policy-powershell.md).
- Bir yönetim grubu olan gözden geçirme [kaynaklarınızı Azure yönetim gruplarıyla düzenleme](..//management-groups/overview.md).
- Görünüm [Azure İlkesi aracılığıyla Azure ortamınızı yöneten](https://channel9.msdn.com/events/Build/2018/THR2030) Channel 9.