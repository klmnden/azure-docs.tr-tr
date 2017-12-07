---
title: "Azure İlkesine Genel Bakış | Microsoft Docs"
description: "Azure İlkesi, Azure ortamında ilke tanımlarınızı oluşturmak, atamak ve yönetmek için kullandığınız bir Azure hizmetidir."
services: azure-policy
keywords: 
author: bandersmsft
ms.author: banders; nini
ms.date: 11/06/2017
ms.topic: overview
ms.service: azure-policy
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: 8ffa4c22c39bcd241b36b55bbcba24ac3f75fff2
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="what-is-azure-policy"></a>Azure İlkesi nedir?

BT yönetimi, iş hedefleri ve BT projeleri arasında netlik oluşturur. İyi bir BT yönetimine girişimlerinizi planlama ve önceliklerinizi stratejik düzeyde belirleme dahildir. Şirketiniz asla çözülmeyecek gibi görünen önemli sayıda BT sorunlarıyla mi karşılaşıyor? İlkeleri uygulayarak bunları daha iyi yönetebilir ve önleyebilirsiniz. Azure İlkesi, ilkeleri uygulama sırasında devreye girer.

Azure İlkesi, ilke tanımlarınızı oluşturmak, atamak ve yönetmek için kullandığınız bir Azure hizmetidir. İlke tanımları, kaynaklarınız üzerinden farklı kuralları ve eylemleri uygulatarak kaynaklarınızın kurumsal standartlarınız ve hizmet düzeyi sözleşmelerinizle uyumlu kalmasını sağlar. Azure İlkesi kaynaklarınızın bir değerlendirmesini yaparak sahip olduğunuz ilke tanımlarıyla uyumsuz olanları tarar. Örneğin, yalnızca belirli türlerdeki sanal makinelere izin veren bir ilkeniz olabilir. Bir başka ilke ise tüm kaynakların belirli bir etikete sahip olmasını gerektirir. Bu ilkeler, kaynakların oluşturulup güncelleştirildiği sırada değerlendirilir.

## <a name="how-is-it-different-from-rbac"></a>RBAC ile farkları nelerdir?

İlke ve rol tabanlı erişim denetimi (RBAC) arasında bazı önemli farklar vardır. RBAC farklı kapsamlardaki kullanıcı eylemlerine odaklanır. Örneğin, istenilen kapsamda bir kaynak grubu için katkıda bulunan rolüne eklenmiş olabilirsiniz. Bu rol, kaynak grubunda değişiklik yapmanıza olanak verir. İlke, dağıtım sırasında ve zaten varolan kaynaklar için kaynak özelliklerine odaklanır. Örneğin, sağlanabilen kaynak türlerini ilkeler aracılığıyla denetleyebilirsiniz. Dilerseniz kaynakların sağlanabileceği konumları kısıtlayabilirsiniz. RBAC’nin aksine, ilke, varsayılan bir izin ve açık reddetme sistemidir.

İlkeleri kullanmak için RBAC aracılığıyla kimliğinizi doğrulamış olmanız gerekir. Hesabınız özellikle şunlara ihtiyaç duyar:

- İlke tanımlamak için `Microsoft.Authorization/policydefinitions/write` izni.
- İlke atamak için `Microsoft.Authorization/policyassignments/write` izni.

Bu izinler, **Katkıda bulunan** rolüne dahil değildir.


## <a name="policy-definition"></a>İlke tanımı

Her ilke tanımında, bu ilkelerin uygulandığı koşullar bulunur. Koşullar karşılandığında gerçekleşmek üzere eşlik eden bir eyleme de sahiptir.

Azure İlkesi'nde, varsayılan olarak kullanabileceğiniz bazı yerleşik ilkeler sunuyoruz. Örneğin:

- **SQL Server 12.0 gerektirir**: Bu ilke tanımı, tüm SQL sunucularının 12.0 sürümünü kullanmasını sağlamayı amaçlayan koşullara veya kurallara sahiptir. Sahip olduğu eylem ise bu ölçütleri karşılamayan tüm sunucuları reddetmektir.
- **İzin Verilen Depolama Hesabı SKU’ları**: Bu ilke tanımı, dağıtılan bir depolama hesabının SKU boyutları kümesine dahil olup olmadığını belirleyen koşullara veya kurallara sahiptir. Sahip olduğu eylem ise tanımlı SKU boyutları kümesine bağlı kalmayan tüm sunucuları reddetmektir.
- **İzin Verilen Kaynak Türü**: Bu ilke tanımı, kuruluşunuzun dağıtabileceği kaynak türlerini belirleyen koşullara veya kurallara sahiptir. Sahip olduğu eylem ise bu tanımlı listenin bir parçası olmayan tüm kaynakları reddetmektir.
- **İzin Verilen Konumlar**: Bu ilke, kuruluşunuzun kaynakları dağıtırken belirleyebileceği konumları kısıtlamanıza olanak verir. Sahip olduğu eylem ise coğrafi uyumluluk gereksinimlerinizi uygulamaktır.
- **İzin Verilen Sanal Makine SKU’ları**: Bu ilke, kuruluşunuzun dağıtabileceği sanal makine SKU’ları kümesini belirlemenize olanak verir.
- **Uygulama etiketi ve varsayılan değeri**: Bu ilke, kullanıcı tarafından belirlenmediyse, gerekli etiketi ve varsayılan değerini uygular.
- **Etiket ve değerini zorunlu kılma**: Bu ilke gerekli bir etiket ve değerini bir kaynağa zorunlu kılar.
- **İzin verilmeyen kaynak türleri**: Bu ilke, kuruluşunuzun dağıtamayacağı kaynak türlerini belirlemenize olanak verir.

Bu ilkelerden herhangi birini Azure portalı, PowerShell veya Azure CLI üzerinden atayabilirsiniz.

İlke tanımlarının yapıları hakkında daha fazla bilgi edinmek için [İlke Tanımı Yapısı](policy-definition.md) adlı makaleye göz atın.

## <a name="policy-assignment"></a>İlke ataması

İlke ataması, belirli bir kapsamda gerçekleşmesi için atanmış bir ilke tanımıdır. Bu kapsamın dahilinde yönetim gruplarından kaynak gruplarına kadar birçok grup bulunabilir. *Kapsam*, ilke tanımının atandığı tüm kaynak gruplarını, abonelikleri veya yönetim gruplarını ifade eder. İlke atamaları, tüm alt kaynaklar tarafından devralınır. Bu nedenle, bir kaynak grubu için bir ilke uygulandığında, bu kaynak grubundaki tüm kaynaklara uygulanmış olur. Ancak, dilerseniz bir alt kapsamı ilke atamasından dışlayabilirsiniz. Örneğin abonelik kapsamında, ağ kaynaklarının oluşturulmasını önleyen bir ilke atayabilirsiniz. Ancak, ağ alt yapısı için hedeflenen bir abonelikten bir kaynak grubunu dışlayabilirsiniz. Ağ kaynaklarını oluşturma konusunda güvendiğiniz kullanıcılara bu ağ kaynak grubuna erişim hakkı sağlayabilirsiniz.

İlke tanımlarını ve atamalarını ayarlama hakkında daha fazla bilgi için bkz. [Azure ortamınızdaki uyumlu olmayan kaynakları tanımlamak için bir ilke ataması oluşturma](assign-policy-definition.md).

## <a name="policy-parameters"></a>İlke parametreleri

İlke parametreleri, oluşturmanız gereken ilke tanımlarının sayısını azaltarak ilke yönetiminizi basitleştirmeye yardımcı olur. İlke tanımı oluştururken parametreleri belirleyerek bunları daha genel hale getirebilirsiniz. Daha sonra bu ilke tanımını farklı senaryolar için kullanabilirsiniz. Bu işlemi, ilke tanımlarının atamasını yaparken farklı değerler girerek gerçekleştirebilirsiniz. Örneğin, abonelik için bir konum kümesi belirtme.

Parametreler, bir ilke tanımı oluşturulurken tanımlanır veya oluşturulur. Tanımlanan parametreye bir ad ve isteğe bağlı olarak bir değer verilir. Örneğin, *konum* başlıklı bir ilke için parametre tanımlayabilirsiniz. Daha sonra, ilkenin atamasını yaparken *EastUS* veya *WestUS* gibi farklı değerler verebilirsiniz.

<!--
Next link should point to new Concept page for Parameters
-->
İlke parametreleri hakkında daha fazla bilgi için bkz. [Kaynak İlkesine Genel Bakış - Parametreler](policy-definition.md#parameters).

## <a name="initiative-definition"></a>Girişim tanımı
Girişim tanımı, tekil kapsamlı bir hedefi gerçekleştirmek üzere belirlenmiş ilke tanımlarının bir koleksiyonudur. Girişim tanımları, ilke tanımlarının yönetimini ve atanmasını basitleştirir. İlke kümelerini gruplandırıp tek bir öğe haline getirerek basitleştirirler. Örneğin, Azure Güvenlik Merkezinizdeki tüm kullanılabilir güvenlik önerilerini izlemeyi hedefleyen **Azure Güvenlik Merkezi'nde İzlemeyi Etkinleştirme** başlıklı bir girişim oluşturabilirsiniz.

Bu girişimin altında sahip olabileceğiniz ilke tanımlarından bazıları şunlardır:

1. **Güvenlik Merkezi’ndeki şifrelenmemiş SQL Veritabanı’nı izleme** – Şifrelenmemiş SQL veritabanlarını ve sunucuları izlemek için.
2. **Güvenlik Merkezi'ndeki işletim sistemi güvenlik açıklarını izleme** – Yapılandırılmış ana hattı karşılamayan sunucuları izlemek için.
3. **Güvenlik Merkezi’ndeki eksik Endpoint Protection’ı izleme** – Yüklü bir bitiş noktası koruma aracısı olmadan sunucuları izlemek için.

<!--
For more information about initiative definitions, see Initiative Definitions.+ (instead of linking to this, link out to Concept page on Initiative Definitions)
-->

## <a name="initiative-assignment"></a>Girişim ataması
İlke ataması gibi, girişim ataması da belirli bir kapsama atanmış olan girişim tanımıdır. Girişim atamaları, her kapsam için birkaç girişim tanımları yapma ihtiyacını azaltır. Bu kapsamın dahilinde de yönetim gruplarından kaynak gruplarına kadar birçok grup bulunabilir.

Önceki örnekten, **Azure Güvenlik Merkezi'nde İzlemeyi Etkinleştirme** girişimi de farklı kapsamlara atanabilir. Örneğin, bir atama **subscriptionA** aboneliğine atanabilir. Bir başkası ise **subscriptionB** aboneliğine atanabilir.

## <a name="initiative-parameters"></a>Girişim parametreleri
İlke parametreleri gibi, girişim parametreleri de fazlalıkları azaltarak girişim yönetiminin basitleştirilmesine yardımcı olur. Girişim parametreleri temelde girişim dahilindeki ilke tanımları tarafından kullanılan parametrelerin listesidir.

Örneğin, iki ilke tanımına sahip **initiativeC** girişim tanımının olduğu bir senaryoyu ele alın. Tanımlanmış bir parametreye sahip her ilke tanımı:

| İlke | parametrenin adı |Parametrenin türü  |Not |
|---|---|---|---|
| policyA | allowedLocations | array  |Parametre türü “array” olarak tanımlandığından bu parametre, bir değer için dizilerin bulunduğu bir liste bekler |
| policyB | allowedSingleLocation |string |Parametre türü “array” olarak tanımlandığından bu parametre, bir değer için bir sözcük bekler |

Bu senaryoda **initiativeC** için girişim parametreleri tanımlanırken üç seçeneğiniz vardır:

1. Bu girişim dahilinde ilke tanımlarının parametrelerini kullanın: Bu örnekte, *allowedLocations* ve *allowedSingleLocation*, **initiativeC** için girişim parametreleri olur.
2. Bu girişim tanımındaki ilke tanımlarının parametrelerine değerler sağlayın. Bu örnekte, **policyA’nın parametresi – allowedLocations** ve **policyB’nin parametresi – allowedSingleLocation** için konum listesi sağlayabilirsiniz.
Bu girişimin atamasını yaparken değerleri de sağlayabilirsiniz.
3. Bu girişimin atamasını yaparken kullanılabilecek bir *değer* seçenekleri listesi sağlayın. Bu girişimi atadığınızda, girişim dahilindeki ilke tanımlarından alınan parametreler yalnızca bu sağlanan listedeki değerleri alabilir.

Örneğin, *EastUS*, *WestUS*, *CentralUS*, ve *WestEurope* içeren bir girişim tanımındaki değer seçeneklerinin listesini oluşturabilirsiniz. Öyleyse, listenin bir parçası olmadığı için girişim ataması sırasında *Southeast Asia* gibi farklı bir değer giremezsiniz.

## <a name="recommendations-for-managing-policies"></a>İlkeleri yönetme ile ilgili öneriler

İlke tanımlarını ve atamalarını yönetirken ve oluştururken izlemenizi önerdiğimiz birkaç noktayı burada bulabilirsiniz:

- Ortamınızda ilke tanımları oluşturuyorsanız, ilke tanımlarınızın ortamınızdaki kaynaklar üzerindeki etkisini izleyebilmek için reddetme etkisinin aksine bir denetim etkisiyle başlamanızı öneririz. Uygulamalarınızı otomatik olarak ölçeklendirmek için komut dosyalarınız zaten varsa, reddetme etkisi belirlemek varolan bu otomatikleştirme görevlerini sekteye uğratabilir.
- Tanımları ve atamaları oluştururken kuruluş hiyerarşilerini göz önünde bulundurmanız önemlidir. Tanımları, yönetim grubu veya abonelik düzeyi ve sonraki alt düzeyde atama yapma gibi daha yüksek bir düzeyde oluşturmanızı öneririz. Örneğin, yönetim grubu düzeyinde bir ilke tanımı oluşturursanız, bu tanımın ilke atamasının kapsamı bu yönetim grubu dahilindeki abonelik düzeyine azaltılabilir.
- Ortamınızın uyumluluk durumunu daha iyi anlamak için standart fiyatlandırma katmanı kullanmanızı öneririz. Fiyatlandırma modellerimiz ve bu modellerin sunduklarıyla ilgili daha fazla bilgi için bkz. [Fiyatlandırma](https://azure.microsoft.com/pricing/details/azure-policy).
- Aklınızda yalnızca bir ilke olsa bile, her zaman ilke tanımları yerine girişim tanımlarını kullanmanızı öneririz. Örneğin, bir ilke tanımınız (*policyDefA*) varsa ve bunu girişim tanımı (*initiativeDefC*) altında oluşturursanız, daha sonra *policyDefA* ilkesindekine benzer hedeflerle *policyDefB* için başka bir ilke tanımı oluşturmaya karar verdiğinizde, bunu *initiativeDefC* altına ekleyip daha iyi bir şekilde izleyebilirsiniz.

   Bir ilke tanımından ilke ataması oluşturduğunuzda, girişim tanımına eklenen herhangi bir yeni ilke tanımı otomatik olarak girişim tanımının altındaki girişim atamasının altında toplanır. Ancak, yeni ilke tanımına tanıtılan yeni bir parametre varsa, girişim tanımını veya atamasını düzenleyerek girişim tanımını ve atamalarını güncelleştirmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Artık kullanıma sunduğumuz Azure İlkesi ve bazı önemli kavramlar ile ilgili bir genel bakışa sahipsiniz, önerdiğimiz diğer adımları aşağıda bulabilirsiniz:

- [İlke tanımı atama](./assign-policy-definition.md)
- [Azure CLI kullanarak bir ilke tanımı atama](./assign-policy-definition-cli.md)
- [PowerShell kullanarak bir ilke tanımı atama](./assign-policy-definition-ps.md)
