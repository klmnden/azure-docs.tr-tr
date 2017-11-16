---
title: "Azure ilkesine genel bakış | Microsoft Docs"
description: "Azure ilke oluşturmak, atamak ve ilke tanımları Azure ortamınızda yönetmek için kullandığınız Azure, bir hizmettir."
services: azure-policy
keywords: 
author: Jim-Parker
ms.author: jimpark; nini
ms.date: 11/06/2017
ms.topic: overview
ms.service: azure-policy
manager: jochan
ms.custom: mvc
ms.openlocfilehash: ef1114f6b1259e4f0d60260febb39bc70b181fbc
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="what-is-azure-policy"></a>Azure ilke nedir?

BT yönetimini iş hedeflerine ve BT projeler arasında netlik oluşturur. İyi BT yönetimini, girişimleri planlama ve stratejik bir düzeyde öncelikleri ayarı içerir. Şirket, çok sayıda çözümlenmiş almak için hiçbir zaman göründüğü BT sorunlarını deneyimi mu? İlkelerini uygulama daha iyi yönetmenize ve bunları önlemeye yardımcı olur. İlkelerini uygulama Azure ilke nereden geldiğini değil.

Azure ilke oluşturmak, atamak ve ilke tanımları yönetmek için kullandığınız Azure bir hizmettir. Bu kaynakları Kurumsal standartları ve hizmet düzeyi sözleşmeleri ile uyumlu olmak için İlke tanımları farklı kurallar ve Eylemler kaynaklarınızı uygulayın. Azure ilke elinizde ilke tanımları ile uyumlu olmayan olanlar için tarama kaynaklarınızı değerlendirme çalışır. Örneğin, sanal makineler yalnızca belirli türdeki izin vermek için bir ilke olabilir. Başka bir tüm kaynakların belirli bir etikete sahip olmasını gerektirir. Bu ilkeler, ardından oluştururken ve kaynakları güncelleştirme değerlendirilir.

## <a name="how-is-it-different-from-rbac"></a>Nasıl RBAC farklı mı?

İlke ve rol tabanlı erişim denetimi (RABC) arasındaki bazı farklar vardır. RBAC farklı kapsamlar kullanıcı eylemlerini odaklanır. Örneğin, istenilen kapsamda bir kaynak grubu için katılımcı rolü için eklenmiş olabilir. Rolü bu kaynak grubuna değişiklik yapmanızı sağlar. İlke dağıtımı sırasında ve zaten mevcut kaynak özellikleri odaklanır kaynakları. Örneğin, ilkeler aracılığıyla sağlanabilir kaynak türleri kontrol edebilirsiniz. Ya da kaynaklar sağlanabilir konumları kısıtlayabilirsiniz. RBAC, bir varsayılan izin ilkedir ve açık sistem engelle.

İlkelerini kullanmak için RBAC kimliğinin doğrulanması gerekir. Özellikle, hesabınızı gerekir:

- `Microsoft.Authorization/policydefinitions/write`ilke tanımlamak için izni.
- `Microsoft.Authorization/policyassignments/write`bir ilke atama izni.

Bu izinleri dahil değildir **katkıda bulunan** rol.


## <a name="policy-definition"></a>İlke tanımı

Her ilke tanımı altında uygulandığı koşul içeriyor. Ve Koşullar karşılanıyorsa gerçekleşir eşlik eden bir eylem vardır.

Azure İlkesi'nde, varsayılan olarak kullanabileceğiniz bazı yerleşik ilkeleri sunuyoruz. Örneğin:

- **SQL Server 12.0 gerektiren**: Bu ilke tanımı tüm SQL Server'lar sürüm 12.0 kullandığınızdan emin olmak için koşullar/kurallarına sahiptir. Bu ölçütleri karşılayan değil tüm sunucuları reddetmek için kendi eylemdir.
- **Depolama hesabı SKU'ları izin**: Bu ilke tanımı dağıtılmakta olan bir depolama hesabı SKU boyutları kümesi içinde olup olmadığını koşullar/kurallar kümesine sahiptir. Eylemi tanımlı SKU boyutları kümesine uymayan tüm sunucuları engellemektir.
- **Kaynak türü izin**: Bu ilke tanımı, kuruluşunuzun dağıtabilirsiniz kaynak türlerini belirtmek için koşullar/kurallar kümesine sahiptir. Eylemi bu tanımlı listeyi parçası olmayan tüm kaynaklar engellemektir.
- **Konumları izin**: Bu ilke, kuruluşunuzun kaynakları dağıtırken belirtebilirsiniz konumları sınırlamak etkinleştirir. Eylemi coğrafi uyumluluk gereksinimleriniz zorlamak için kullanılır.
- **Sanal makine SKU'ları izin**: Bu ilke, sanal makine, kuruluşunuzun dağıtabilirsiniz SKU'ları bir dizi belirtmenize olanak tanır.
- **Etiket ve varsayılan değerini uygulamak**: kullanıcı tarafından belirtilmezse, gerekli bir etiket ve varsayılan değeri, bu ilke uygulanır.
- **Etiket ve değerini zorla**: Bu ilke gerekli bir etiket ve değerini bir kaynağa zorunlu tutar.
- **Kaynak türleri izin verilmiyor**: Bu ilke, kuruluşunuzun dağıtamazsınız kaynak türleri belirtmenize olanak tanır.

Azure portal, PowerShell veya Azure CLI aracılığıyla bu ilkeleri atayabilirsiniz.

İlke tanımları yapılar hakkında daha fazla bilgi için bu makalenin - Ara [ilke tanımı yapısını](policy-definition.md).

## <a name="policy-assignment"></a>İlke ataması

Bir ilke atamasını belirli bir kapsamda gerçekleşmesi için atanmış bir ilke tanımı ' dir. Bu kapsam bir yönetim grubundan bir kaynak grubuna çeşitli işlemleri ifade edebilir. Terim *kapsam* tüm kaynak grupları, abonelikleri veya ilke tanımı atanan yönetim gruplarını başvuruyor. İlke atamalarını tüm alt kaynaklar tarafından devralınır. Bu nedenle, bir kaynak grubu için bir ilke uygulandığında, bu kaynak grubundaki tüm kaynaklara uygulanır. Ancak, bir subscope ilke atama dışarıda bırakabilirsiniz. Örneğin, abonelik kapsamında ağ kaynaklarını oluşturulmasını önleyen bir ilke atayabilirsiniz. Ancak, bir kaynak grubu için ağ altyapısı hedeflenen abonelik içindeki dışlayın. Bu ağ kaynak grubu, ağ kaynaklarını oluşturma konusunda güvendiğiniz kullanıcılara erişim hakkı.

Ayar ilke tanımları ve atamaları hakkında daha fazla bilgi için bkz: [Azure ortamınızda uyumlu olmayan kaynakları tanımlamak için bir ilke atamasını oluşturma](assign-policy-definition.md).

## <a name="policy-parameters"></a>İlke parametreleri

İlke parametreleri oluşturmanız gerekir ilke tanımları sayısını azaltarak ilke yönetimini basitleştirmeye yardımcı. Daha fazla genel yapmak için bir ilke tanımı oluşturulurken parametreleri tanımlayabilirsiniz. Daha sonra bu ilke tanımı farklı senaryolar için yeniden kullanabilirsiniz. Farklı değerler geçirerek ilke tanımı atarken bunu. Örneğin, bir dizi bir abonelik için konumları belirtme.

Parametreleri tanımlı/bir ilke tanımı oluşturulurken oluşturulur. Bir parametre tanımlandığında, verilen bir ad ve isteğe bağlı olarak bir değer verilen. Örneğin, başlıklı bir ilke için bir parametre tanımlayabilirsiniz *konumu*. Bu farklı değerler gibi verebilirsiniz sonra *EastUS* veya *WestUS* bir ilkeyi atarken ne.

<!--
Next link should point to new Concept page for Parameters
-->
İlke parametreleri hakkında daha fazla bilgi için bkz: [kaynak ilkesine genel bakış - parametreleri](policy-definition.md#parameters).

## <a name="initiative-definition"></a>Girişimi tanımı
Bir girişimi tanımı tekil değerlendiriyoruz elde doğrultusunda uyarlanabilir ilke tanımları koleksiyonudur. Girişimi tanımları, yönetme ve ilke tanımları atama basitleştirin. Bunlar, bir ilke kümesi tek bir öğe olarak gruplandırarak basitleştirin. Örneğin, başlıklı bir girişimi oluşturabilirsiniz **Azure Güvenlik Merkezi'nde izlemesini etkinleştir**, Azure Güvenlik Merkezi tüm kullanılabilir güvenlik önerileri izlemek için bir hedefi.

Bu girişim altında ilke tanımları gibi gerekir:

1. **İzleyici şifrelenmemiş SQL veritabanı Güvenlik Merkezi'nde** – şifrelenmemiş SQL veritabanları ve sunucuları izleme.
2. **İşletim sistemi güvenlik açıkları Güvenlik Merkezi'ndeki izleme** – yapılandırılmış temel karşılamadığı sunucularını izlemek için.
3. **Eksik Endpoint Protection Güvenlik Merkezi'ndeki izleme** – yüklü endpoint protection Aracısı olmadan sunucular izleme.

<!--
For more information about initiative definitions, see Initiative Definitions.+ (instead of linking to this, link out to Concept page on Initiative Definitions)
-->

## <a name="initiative-assignment"></a>Girişimi atama
Bir ilke atamasını gibi girişimi atama belirli bir kapsama atanmış bir girişimi bir tanımıdır. Girişimi atamaları her kapsam için birkaç girişimi tanımları yapmak için gereken azaltın. Bu kapsam için bir kaynak grubu yönetim grubundan değişiklik gösterebilir.

Önceki örnekten **Azure Güvenlik Merkezi'nde izlemesini etkinleştir** girişimi için farklı kapsamlar atanabilir. Örneğin, bir atama atanması **subscriptionA**. Başka bir atanabilir **subscriptionB**.

## <a name="initiative-parameters"></a>Girişimi parametreleri
İlke parametreleri gibi girişimi parametreleri artıklık azaltarak girişimi yönetimini basitleştirmeye yardımcı olması. Temelde Initiative içinde ilke tanımları tarafından kullanılan parametrelerin listesi girişimi parametreleridir.

Örneğin, bir girişimi tanımı - sahip olduğu bir senaryo ele **initiativeC**, iki ilke tanımları ile. Bir tanımlanan parametre sahip her ilke tanımı:

| İlke | Parametrenin adı |Parametrenin türü  |Not |
|---|---|---|---|
| policyA | allowedLocations | Dizi  |Parametre türü bir dizi olarak tanımlamış Bu parametre bir değer için dize listesi bekliyor |
| policyB | allowedSingleLocation |Dize |Parametre türü dize olarak tanımlamış Bu parametre bir değer için bir sözcük bekliyor |

Bu senaryoda, girişimi parametrelerini tanımlarken **initiativeC**, üç seçeneğiniz vardır:

1. Bu girişim içinde ilke tanımları parametrelerini kullanın: Bu örnekte, *allowedLocations* ve *allowedSingleLocation* girişimi parametrelerini hale **initiativeC** .
2. Bu girişimi tanımındaki ilke tanımları parametreleri için değerleri girin. Bu örnekte, konumlara listesini sağlayabilirsiniz **policyA'ın parametre – allowedLocations** ve **policyB'ın parametre – allowedSingleLocation**.
Bu girişim atarken değerleri de sağlayabilirsiniz.
3. Bir listesini sağlayın *değeri* bu Initiative atarken kullanılabilir seçenekleri. Bu girişim atadığınızda, ilke tanımları Initiative içinde devralınan parametrelerinden yalnızca olabilir sağlanan bu listeden değerleri.

Örneğin, değer seçeneklerin bir listesini içeren bir girişimi tanımında oluşturabilirsiniz *EastUS*, *WestUS*, *CentralUS*, ve *WestEurope*. Bu nedenle, farklı bir değer gibi giriş bağlanamıyorsanız *Güneydoğu Asya* girişimi atama sırasında listesinin bir parçası olmadığından.

## <a name="recommendations-for-managing-policies"></a>İlkeleri yönetme ile ilgili öneriler

Oluşturma ve ilke tanımları ve atamaları yönetme sırasında biz tutumunuzu izlemeniz gereken birkaç işaretçi şunlardır:

- Ortamınızda ilke tanımları oluşturuyorsanız, ilke tanımı, ortamınızdaki kaynakları üzerindeki etkilerini izlemek için bir izin verme efekti aksine bir denetim etkisi ile başlamanızı öneririz. Komut dosyaları için otomatik ölçeklendirme yerinde uygulamalarınızı zaten varsa, reddetme etkisi ayarı zaten yerinde olduğuna otomasyonlara görevleri aksatabilir.
- Kuruluş hiyerarşileri tanımlar ve atamaları oluşturulurken göz önünde bulundurmanız önemlidir. Bir yüksek düzeyde, örneğin yönetim grubu veya abonelik düzeyinde tanımları oluşturma ve sonraki alt düzeyde atama öneririz. Örneğin, yönetim grubu düzeyinde bir ilke tanımı oluşturursanız, bu yönetim grubu içinde bir abonelik düzey aşağıya doğru tanımın bir ilke atamasını kısıtlanabilir.
- Ortamınızı uyumluluk durumunu daha iyi anlamak için standart fiyatlandırma katmanı, kullanarak öneririz. Bizim fiyatlandırma modeli hakkında daha fazla bilgi için ve hangi bunların her birini sunar, göz atın [fiyatlandırma](https://azure.microsoft.com/pricing/details/azure-policy).
- Yalnızca bir ilke göz önünde olsa bile her zaman girişimi tanımları ilke tanımları yerine kullanmanızı öneririz. Örneğin, bir ilke tanımı – varsa *policyDefA* ve altında girişimi tanımı - oluşturmak *initiativeDefC*, daha sonra içinbaşkabirilketanımıoluşturmayakararverirseniz*policyDefB* hedefleri olarak benzer *policyDefA*, altında ekleyebileceğiniz *initiativeDefC* ve böylece bunları daha iyi izleyebilirsiniz.

   Tüm yeni ilke tanımları otomatik olarak girişimi tanımına eklenen bir girişimi tanımından girişimi atama oluşturduktan sonra geri alma girişimi tanımın altında girişimi atamaları altında aklınızda bulundurun. Ancak, yeni ilke tanımı sunulan yeni bir parametre ise girişimi tanımı veya atama düzenleyerek girişimi tanımı ve atamaları güncelleştirmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Azure ilkesine genel bakış ve bazı biz Tanıtımı anahtar kavramlar sahip olduğunuza göre önerilen sonraki adımlar şunlardır:

- [Bir ilke tanımı atayın](./assign-policy-definition.md)
- [Azure CLI kullanarak bir ilke tanımı atayın](./assign-policy-definition-cli.md)
- [PowerShell kullanarak bir ilke tanımı atayın](./assign-policy-definition-ps.md)
