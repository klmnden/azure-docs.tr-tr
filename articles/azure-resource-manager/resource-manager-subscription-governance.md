---
title: "Azure'a taşıma kuruluşlar için en iyi uygulamaları | Microsoft Docs"
description: "Kuruluşlar, güvenli ve yönetilebilir bir ortam sağlamak için kullanabileceğiniz iskele açıklar."
services: azure-resource-manager
documentationcenter: na
author: rdendtler
manager: timlt
editor: tysonn
ms.assetid: 8692f37e-4d33-4100-b472-a8da37ce628f
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/31/2017
ms.author: rodend;karlku;tomfitz
ms.openlocfilehash: 3b5087faaf3db087b15b77fedac8df0d7e4a899a
ms.sourcegitcommit: 094061b19b0a707eace42ae47f39d7a666364d58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2017
---
# <a name="azure-enterprise-scaffold---prescriptive-subscription-governance"></a>Azure enterprise iskele - Düzenleyici abonelik yönetimi
Kuruluşlar Git Gide daha fazla esneklik ve Çeviklik için genel bulut geliştirilmektedir. Bunlar bulutun gücü veya iş için kaynakları en iyi duruma gelir oluşturmak için çalışan. Microsoft Azure, kuruluşların yapı taşları gibi çok çeşitli iş yüklerini ve uygulamaları adres birleştirebilirsiniz birçok sağlar. 

Ancak, başlamak nereye bilerek genellikle zordur. Azure kullanmaya karar vermeden sonra birkaç sorular yaygın olarak ortaya:

* "Nasıl ı bizim yasal veri egemenliği bazı ülkelerde gereksinimlerini?"
* "Nasıl ı birisi yanlışlıkla bir kritik sistem değiştirmemesini emin olursunuz?"
* "Nasıl ı hesap ve fatura geri doğru şekilde ne her kaynak destekliyor anlarım?"

Hiçbir koruma rayları boş bir aboneliğe aday zorlu olur. Bu boş alanı Azure taşınma engel olabilir.

Bu makalede idare gereksinimini adres ve çeviklik gereksinimini ile dengelemek teknik uzmanları için bir başlangıç noktası sağlar. Uygulama ve bunların Azure Aboneliklerini Yönetme kuruluşlar kılavuzları bir kurumsal iskele kavramını sunmaktadır. 

## <a name="need-for-governance"></a>İdare gereksinimini
Azure'a taşırken, kuruluş içindeki bulut başarılı kullanımını sağlamak için erken idare konu incelemeniz gerekir. Ne yazık ki, zaman kapsamlı idare sistem oluşturma geçmişi anlamına gelir ve kurumsal BT karıştırılmaksızın bazı iş grupları doğrudan satıcılara gidin. Kaynakları düzgün yönetilmeyen varsa bu yaklaşımı Kurumsal güvenlik açıkları için açık bırakabilirsiniz. -Çeviklik, esneklik ve tüketim tabanlı fiyatlandırma - genel bulut özelliklerini hızla müşteriler (iç ve dış) isteklerini karşılamak için gereken iş grupları için önemlidir. Ancak, kurumsal BT veriler ve sistemlerle etkili bir şekilde korunduğundan emin olması gerekir.

Gerçek Hayatta yapı iskelesi yapısı temeli oluşturmak için kullanılır. İskele genel anahattı yönlendirir ve takılması daha kalıcı sistemler için bağlantı noktaları sağlar. Bir kuruluş iskele aynıdır: esnek denetimler ve genel bulut üzerinde oluşturulmuş Hizmetleri için ortamı ve bağlayıcılarını yapısına sağlayan Azure özellikleri kümesi. Oluşturucular sağlar (BT ve iş grupları) oluşturmak ve yeni hizmetler eklemek için bir temel.

İskele birçok katılımlar çeşitli boyutlarda istemcilerle toplanan olduğu yöntemler temel alır. Bu küçük kuruluşlar Fortune 500 şirketler ve geçirme ve bulut çözümleri geliştirme bağımsız yazılım satıcıları için bulut çözümleri geliştirme istemcileri arasındadır. Kurumsal iskele "Geleneksel BT iş yüklerini ve Çevik iş yükleri; desteklemek için esnek amaca" gibi geliştiricilerin yazılım olarak-hizmet (SaaS) uygulamaları oluşturma Azure yeteneklerini temel.

Kurumsal iskele her yeni abonelik Azure içinde foundation olması amaçlanmıştır. İş grupları ve geliştiricilerin kendi hedefleri hızlı bir şekilde toplantı önleme olmadan iş yükleri, bir kuruluşun en düşük idare gereksinimleri karşıladığından emin olmak yöneticilerin sağlar.

> [!IMPORTANT]
> İdare Azure başarısı için önemlidir. Bu makalede bir kurumsal iskele teknik uyarlamasını hedefler ancak daha geniş işlem ve bileşenleri arasındaki ilişkiler yalnızca dokunur. İlke idare üstten aşağı akar ve hangi iş elde etmek istediği tarafından belirlenir. Doğal olarak, Azure idare modeli oluşturulmasında temsilcileri içerir BT, ancak daha da önemlisi, iş grubu kılavuzları ve güvenliği ve risk yönetimi güçlü gösteriminden olması gerekir. Sonunda, bir kuruluş iskele, bir kuruluşun misyonu ve hedefleri kolaylaştırmak için iş riskin azaltılmasıyla ilgilidir.
> 
> 

Aşağıdaki resimde iskele bileşenlerini açıklar. Departmanlar, hesaplar ve abonelikler için sağlam bir planı foundation kullanır. Resource Manager ilkeleri ve güçlü adlandırma standartları ayaklar oluşur. İskele kalan çekirdek Azure özellikleri gelir ve bu etkinleştir güvenli ve yönetilebilir bir ortam özellikleri.

![iskele bileşenleri](./media/resource-manager-subscription-governance/components.png)

> [!NOTE]
> Azure hızlı bir şekilde kendi giriş 2008'de bu yana haline geldi. Bu büyüme mühendislik ekipleri Microsoft Hizmetleri dağıtma ve yönetme için kendi yaklaşımı zorlanıyor gereklidir. Azure Resource Manager modeli 2014'te kullanıma sunulmuştur ve klasik dağıtım modeli değiştirir. Resource Manager kuruluşlarının daha kolay dağıtmak, düzenlemek ve Azure kaynaklarını denetlemenize olanak tanır. Kaynak Yöneticisi, karmaşık, birbirine çözümlerinin hızlı dağıtımı için kaynakları oluştururken paralelleştirme içerir. Ayrıntılı erişim denetimi ve meta veri etiketi kaynaklarla yeteneğini de içerir. Microsoft, tüm kaynaklara Resource Manager modeli aracılığıyla oluşturduğunuz önerir. Kurumsal iskele açıkça Resource Manager modeli için tasarlanmıştır.
> 
> 

## <a name="define-your-hierarchy"></a>Hiyerarşinizi tanımlayın
İskele Azure işletme kaydı (ve Enterprise Portal'da) temelini oluşturur. İşletme kaydı şekil tanımlar ve şirket içindeki Azure Hizmetleri kullanın ve çekirdek idare yapısıdır. Kurumsal Anlaşma içinde müşteriler daha fazla Departmanlar, hesapları ve son olarak, abonelikleri ortamına ayırabilir imkanınız olur. Bir Azure aboneliği tüm kaynakların bulunduğu temel birimidir. Ayrıca, Azure, çekirdek, kaynaklar, vb. sayısı gibi birkaç sınırlarda tanımlar.

![hiyerarşi](./media/resource-manager-subscription-governance/agreement.png)

Her Kuruluş farklıdır ve önceki görüntüde hiyerarşisinde Azure şirket içinde nasıl düzenlendiğini önemli esneklik sağlar. Bu belgede yer alan yönergeleri uygulamadan önce hiyerarşinizdeki model ve faturalama, kaynak erişimi ve karmaşıklık üzerindeki etkisini anlamak gerekir.

Azure kayıtları için üç ortak desenler şunlardır:

* **İşlevsel** düzeni
  
    ![işlev](./media/resource-manager-subscription-governance/functional.png)
* **Departman** düzeni 
  
    ![başlar](./media/resource-manager-subscription-governance/business.png)
* **Coğrafi** düzeni
  
    ![coğrafi](./media/resource-manager-subscription-governance/geographic.png)

Kurumsal gereksinimleri aboneliğe idare genişletmek için abonelik düzeyinde iskele uygulayın.

## <a name="naming-standards"></a>Adlandırma standartları
İskele, ilk sütun adlandırma standartlarının. İyi tasarlanmış adlandırma standartları portalında bir fatura ve komut dosyaları içinde kaynakları tanımlamak etkinleştirin. Büyük olasılıkla, şirket içi altyapı için adlandırma standartlarının zaten vardır. Azure ortamınıza eklerken, bu adlandırma standartları Azure kaynaklarınızı genişletmelidir. Adlandırma standardı, tüm düzeylerdeki ortamının daha verimli yönetimini kolaylaştırmak.

> [!TIP]
> Adlandırma kuralları için:
> * Gözden geçirin ve mümkün olduğunda benimsemeyi [desenleri ve uygulamalar kılavuzunu](../guidance/guidance-naming-conventions.md). Bu kılavuz üzerinde anlamlı bir adlandırma standart bir karar vermenize yardımcı olur.
> * (Örneğin, contoso.com ve vnetNetworkName) kaynakların adları için camelCasing kullanın. Not: Vardır depolama hesapları gibi kaynaklara nerede tek seçenek küçük (ve diğer özel karakterler) kullanmaktır.
> * Adlandırma standartları zorlamak için Azure Resource Manager ilkeleri (sonraki bölümde açıklanmıştır) kullanmayı düşünün.
> 
> Önceki ipuçları tutarlı bir adlandırma kuralı uygulamanıza yardımcı olur.

## <a name="policies-and-auditing"></a>İlkeleri ve Denetim
İskele, ikinci sütun oluşturursunuz [Azure ilkeleri](../azure-policy/azure-policy-introduction.md) ve [etkinlik günlüğü denetleme](resource-group-audit.md). Kaynak Yöneticisi ilkeleri, Azure risk yönetme olanağı sağlar. Veri egemenliği kısıtlama zorlamayı veya belirli eylemleri denetim olun ilkeleri tanımlayabilirsiniz. 

* Varsayılan bir ilkedir **izin** sistem. Tanımlama ve kaynaklara Reddet veya Eylemler kaynaklar üzerinde denetim ilkeleri atayarak Eylemler denetler.
* İlke tanım dili (IF then koşulları) ilkesi tanımlarında tarafından ilkeleri açıklanmaktadır.
* Oluşturduğunuz JSON (Javascript nesne gösterimi) ile biçimlendirilmiş dosyalar ilkeleri. Bir ilke tanımladıktan sonra belirli bir kapsam atadığınız: Abonelik, kaynak grubu veya kaynak.

İlkeleri senaryolarınız için ayrıntılı bir yaklaşım için izin birden çok eylem vardır. Eylemler şunlardır:

* **Reddetme**: kaynak isteği engeller
* **Denetim**: İstek sağlar ancak (uyarıları sağlamak ya da runbook'ları tetiklemek için kullanılabilir) etkinlik günlüğü için bir satır ekler
* **Append**: Belirtilen bilgi kaynağı ekler. Örneğin, bir kaynakta "CostCenter" etiketi değilse, varsayılan bir değerle bu etiketi ekleyin.

### <a name="common-uses-of-resource-manager-policies"></a>Resource Manager İlkeleri'nın yaygın kullanımları
Azure Resource Manager Azure araç setindeki güçlü bir araç ilkelerdir. Etiketleme aracılığıyla kaynakları için bir maliyet merkezi tanımlamak ve uyumluluk gereksinimlerinin karşılandığından emin olmak için beklenmeyen maliyetleri önlemek sağlar. İlkeleri yerleşik denetim özellikleri ile birleştirildiğinde, karmaşık ve esnek çözümleri deneyerek. İlkeleri "Geleneksel BT" iş yükleri ve "Çevik" iş yükleri için denetimleri sağlamak şirketlerin izin verir; gibi müşteri uygulamaları geliştirme. Biz ilkeleri için bkz: en yaygın modeli şunlardır:

* **Coğrafi-uyumluluk/veri egemenliği** -Azure bölgeleri dünya genelindeki sağlar. İşletmeler genellikle kaynakları (mi, yoksa veri egemenliği emin olmak için yalnızca kaynakları kaynakları son tüketicileri yakın oluşturulmasını sağlamak için) oluşturulduğu denetlemek istiyor.
* **Yönetim maliyeti** -bir Azure aboneliği birçok türleri ve ölçek kaynakları içerebilir. Şirketler genellikle standart abonelikleri dolar ayda bir veya daha fazla yüzlerce maliyet düşükse kaynakları kullanmaktan kaçının olun ister.
* **Varsayılan idare gerekli etiketleri aracılığıyla** -etiketleri gerektirmektir en yaygın ve yüksek oranda istenen özelliklerden biri. Azure Resource Manager ilkelerini kullanarak kuruluşların bir kaynağa uygun şekilde etiketli emin olmak kullanabilirsiniz. En yaygın etiketler: departmanı, kaynak sahibi ve ortam türünü (örneğin - üretim, test, geliştirme)

**Örnekler**

"Geleneksel BT" abonelik satırı iş kolu uygulamaları için

* Departman zorlamak ve tüm kaynaklara sahip etiketleri
* Kuzey Amerika bölgesine kaynak oluşturma kısıtla
* G-serisi VM'ler ve Hdınsight kümeleri oluşturmak için yeteneğini kısıtla

Bir iş biriminin bulut uygulamaları oluşturmak için "Çevik" ortamı

* Veri egemenliği gereksinimlerini karşılamak için kaynakları oluşturulmasına izin yalnızca belirli bir bölgedeki.
* Ortam etiketi tüm kaynaklar hakkında zorlar. Bir etiketi olmayan bir kaynak oluşturduysanız, sona **ortam: Bilinmeyen** kaynağa etiketi.
* Denetim kaynakları Kuzey Amerika dışında oluşturulur ancak engel olmaz.
* Yüksek maliyetli kaynakları oluşturulduğunda denetim.

> [!TIP]
> Resource Manager ilkeleri kuruluşlar arasında en yaygın kullanımı denetimidir *nerede* kaynakları oluşturulabilir ve *ne* kaynak türleri oluşturulabilir. Üzerinde denetim sağlamanın yanı sıra *nerede* ve *ne*, pek çok kurum kaynaklarınız uygun meta veri tüketimi için faturalandırmak emin olmak için ilkeleri kullanın. Abonelik düzeyinde ilkeleri uygulayarak öneririz:
> 
> * Coğrafi-uyumluluk/veri egemenliği
> * Maliyet yönetimi
> * Gerekli etiketleri (Determined BillTo, uygulama sahibi gibi iş gereksinimleri tarafından)
> 
> Kapsam daha düşük düzeylerdeki ek ilkeler uygulayabilirsiniz.
> 
> 

### <a name="audit---what-happened"></a>Denetim - ne oldu?
Ortamınızı nasıl çalıştığını görmek için kullanıcı etkinliğini denetleyin gerekir. Çoğu kaynak türleri Azure içinde bir günlük araçla veya Azure Operations Management Suite çözümleyebilirsiniz tanılama günlüklerini oluşturun. Etkinlik günlükleri birden çok bir departman sağlamak için abonelikleri veya Kurumsal görünümünde toplayabilirsiniz. Önemli bir tanılama aracı ve tetikleyici olayları Azure ortamında çok önemli bir mekanizma denetim kayıtları için uygulanır.

Resource Manager dağıtımları etkinlik günlükleri belirlemek etkinleştirme **operations** yerinde ve bunları gerçekleştiren sürdü. Etkinlik günlükleri toplanabilir ve günlük analizi gibi araçları kullanarak bir araya getirilir.

## <a name="resource-tags"></a>Kaynak etiketleri
Kuruluşunuzdaki kullanıcılar kaynakları aboneliğe eklemek gibi kaynakları uygun departmanı, müşteri ve ortam ile ilişkilendirmek giderek daha çok önemli hale gelir. Meta veri kaynaklarına ekleyebilirsiniz [etiketleri](resource-group-using-tags.md). Kaynak veya sahibi hakkında bilgi sağlamak için etiketler kullanın. Etiketleri yalnızca toplama ve kaynakları çeşitli şekillerde gruplamak, ancak bu verileri geri ödeme amaçları için kullanmak üzere etkinleştirin. Kaynaklarla 15 anahtar: değer eşlerini kadar etiketleyebilirsiniz. 

Kaynak etiketleri esnektir ve çoğu kaynaklarına bağlı olması. Ortak kaynak etiketleri örnekleri şunlardır:

* BillTo
* Departman (veya iş birimi)
* Ortam (üretim, aşama, geliştirme)
* Katman (Web katmanı, uygulama katmanı)
* Uygulama sahibi
* ProjectName

![etiketler](./media/resource-manager-subscription-governance/resource-group-tagging.png)

Daha fazla etiket örnekleri için bkz: [Azure kaynakları için adlandırma kurallarını önerilen](../guidance/guidance-naming-conventions.md).

> [!TIP]
> İçin etiketleme olması zorunlu tutulmuştur ilkesinde yapmadan göz önünde bulundurun:
> 
> * Kaynak grupları
> * Depolama
> * Virtual Machines
> * Uygulama hizmeti ortamları/web sunucuları
> 
> Bu etiketleme stratejisi hangi meta verileri iş, Finans, güvenlik, risk yönetimi ve ortamın Genel Yönetim için gerekli olan aboneliklerinizi tanımlar. 

## <a name="resource-group"></a>Kaynak grubu
Resource Manager, kaynakları yönetim, faturalama veya doğal benzeşimi anlamlı gruplar halinde yerleştirilmesine olanak sağlar. Daha önce belirtildiği gibi Azure iki dağıtım modeline sahiptir. Önceki Klasik modelde temel yönetim birimidir aboneliği karşılaşıldı. Çok sayıda abonelikleri oluşturulmasına neden bir abonelik içindeki kaynaklara ayırmanız zordu. Resource Manager modeli ile kaynak gruplarının giriş gördük. Kaynak grupları, ortak bir yaşam döngüsü veya "tüm SQL sunucuları" gibi bir özniteliği paylaşan kaynakları kapsayıcılar olan ya da "Uygulama A".

Kaynak grupları diğer içinde yer alamaz ve kaynakları yalnızca bir kaynak grubuna ait olabilir. Bir kaynak grubundaki tüm kaynaklar üzerinde belirli eylemleri uygulayabilirsiniz. Örneğin, bir kaynak grubunun silinmesi tüm kaynakların kaynak grubunda kaldırır. Genellikle, tüm uygulama veya ilgili sistem aynı kaynak grubunda yerleştirin. Örneğin, Contoso Web uygulaması olarak adlandırılan bir üç katmanlı uygulamayı web sunucusu, uygulama sunucusu ve SQL server ile aynı kaynak grubunda içerecektir.

> [!TIP]
> Kaynak gruplarınızı düzenleme nasıl "Geleneksel BT" iş yüklerini "Çevik BT" iş yükleri için farklı olabilir:
> 
> * "Geleneksel BT" iş yükleri en yaygın bir uygulama gibi aynı yaşam döngüsü içinde öğelerine göre gruplandırılır. Tek tek uygulama yönetimi için uygulama tarafından gruplandırma sağlar.
> * "Çevik BT" iş yüklerini eğilimindedir dış müşterilerle bulut uygulamaları odaklanmıştır. Kaynak grupları (örneğin, Web katmanı, uygulama katmanı) dağıtım katmanları yansıtmalıdır ve yönetimi.
> 
> İş yükünüzün anlama, bir kaynak grubu stratejisi geliştirme yardımcı olur.

## <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi
Büyük olasılıkla kendiniz "kimin kaynaklara erişimi olması gerekir mi?" istiyoruz. ve "Bu erişimin nasıl denetlerim?" İzin verme veya Azure portalına erişim izin vermeme ve Portalı'nda kaynaklara erişimi denetlemek için çok önemlidir. 

Azure başlangıçta yayımlandığında, bir abonelik için erişim denetimleri temel: Yöneticisi veya ortak Yöneticisi. Klasik modeli kapsanan tüm kaynaklara erişim izni Portalı'nda bir abonelikte erişim sağlar. Bu eksiklik hassas bir denetim için bir Azure kayıt düzeyi makul erişim denetimi sağlamak için abonelikleri çoğalmasıdır gerektiriyordu.

Bu abonelikleri çoğalmasıdır artık gerekli değildir. Rol tabanlı erişim denetimi ile standart rolleri (örneğin, "okuyucu" ve "yazıcı" karşılaşılan rollerinin) kullanıcıları atayabilirsiniz. Özel roller tanımlayabilir.

> [!TIP]
> Rol tabanlı erişim denetimi uygulamak için:
> * Azure AD Connect aracını kullanarak Active Directory için Kurumsal kimlik deponuza (en yaygın olarak Active Directory) bağlayın.
> * Yönetim/ortak yöneticisi olan bir yönetilen kimliğini kullanarak bir abonelik denetler. **Verme** yönetici/ortak yönetici için yeni bir abonelik sahibi atayın. Bunun yerine, sağlamak için RBAC rolleri kullanın **sahibi** bir grup veya tek tek hakları.
> * Azure kullanıcıları grubuna (örneğin, uygulama X sahipleri) Active Directory'de ekleyin. Uygulamayı içeren kaynak grubunu yönetmek için uygun haklara Grup üyeleri sağlamak için eşitlenen grubu kullanın.
> * Verme ilkesini izleyin **en az ayrıcalık** beklenen işlerini gerçekleştirmek için gerekli. Örneğin:
>   * Dağıtım grubu: yalnızca kaynakları dağıtamaz grubu.
>   * Sanal Makine Yönetimi: VM'ler (işlemleri için) yeniden başlatmanız mümkün olan bir grubu
> 
> Bu ipuçları aboneliğinizi kullanıcı erişimi yönetmenize yardımcı olur.

## <a name="azure-resource-locks"></a>Azure kaynak kilitleri
Kuruluşunuz için abonelik Çekirdek Hizmetleri ekler gibi bu hizmetleri iş kesintisi yaşamamak kullanılabilir olmasını sağlamak giderek daha çok önemli olur. [Kaynak kilitleri](resource-group-lock-resources.md) değiştirilmesini veya silinmesini sahip olduğu uygulamalar veya Bulut altyapısı üzerinde önemli bir etkisi yüksek değerli kaynaklardaki işlemlerini kısıtlamak etkinleştirin. Kilitleri, bir abonelik, kaynak grubuna ya da kaynağa uygulayabilirsiniz. Genellikle, sanal ağlar, ağ geçitleri ve depolama hesapları gibi temel kaynaklarına kilitleri uygulayın. 

Kaynak kilitleri şu anda iki değer destekler: CanNotDelete ve salt okunur. Kullanıcılar (uygun haklarıyla) hala CanNotDelete anlamına gelir okuma veya bir kaynak değiştirme ancak bunu silemezsiniz. Salt okunur, yetkili kullanıcıların silemez veya bir kaynak değiştirme olduğunu anlamına gelir.

Oluşturmak veya yönetim kilitleri silmek için erişimi olmalıdır `Microsoft.Authorization/*` veya `Microsoft.Authorization/locks/*` eylemler.
Yerleşik roller bu eylemleri yalnızca sahip ve kullanıcı erişimi Yöneticisi verilir.

> [!TIP]
> Çekirdek Ağ Seçenekleri Kilitleriyle korunmalıdır. Siteden siteye VPN ağ geçidi yanlışlıkla silinmesini, bir Azure aboneliğine felaket niteliğinde olacaktır. Azure kullanımda olan bir sanal ağı silme izin vermez, ancak daha fazla kısıtlamaları yararlı bir önlemidir. 
> 
> * Sanal ağ: CanNotDelete
> * Ağ güvenlik grubu: CanNotDelete
> * İlkeleri: CanNotDelete
> 
> İlkeler ayrıca uygun denetimleri bakım için önemli değildir. Uyguladığınız öneririz bir **CanNotDelete** kullanımda olan ilkeleri için kilitle.

## <a name="core-networking-resources"></a>Çekirdek Ağ kaynakları
Kaynaklara erişimi (içinde corporation'ın ağ) iç veya dış (Internet) aracılığıyla olabilir. Yanlışlıkla kaynakları içinde yanlış yere koyun ve potansiyel olarak bunları kötü amaçlı erişime açmak, kuruluşunuzdaki kullanıcılar için kolay bir işlemdir. Şirket içi cihazları gibi kuruluşların Azure kullanıcıların doğru kararlar emin olmak için uygun denetimleri eklemeniz gerekir. Abonelik Yönetimi için temel Denetim erişim sağlayan çekirdek kaynakları belirleyin. Çekirdek kaynakları oluşur:

* **Sanal ağlar** alt ağlar için kapsayıcı nesnelerdir. Kesinlikle gerekli olsa, uygulamaları dahili şirket kaynaklarına bağlanırken çoğunlukla kullanılır.
* **Ağ güvenlik grupları** bir Güvenlik Duvarı'na benzer ve nasıl kaynak "ağ üzerinden iletişim kurabilirsiniz için" kuralları sağlar. Nasıl üzerinde ayrıntılı denetim sağladıkları / Internet veya diğer alt ağlara aynı sanal ağ için bir alt ağ (veya sanal makine) bağlanabilir.

![Çekirdek Ağ](./media/resource-manager-subscription-governance/core-network.png)

> [!TIP]
> Ağ iletişimi için:
> * Dışa dönük iş yükleri ve dahili kullanıma yönelik iş yükleri ayrılmış sanal ağı oluşturun. Bu yaklaşım yanlışlıkla bir dış karşılıklı alanı iç iş yükleri için tasarlanmıştır sanal makineleri yerleştirme olasılığını azaltır.
> * Erişimi sınırlamak için ağ güvenlik gruplarını yapılandırın. En azından, iç sanal ağlardan internet'e erişimi engellemek ve dış sanal ağlardan şirket ağına erişimi engelleme.
> 
> Bu ipuçları güvenli ağ kaynaklarını uygulamanıza yardımcı olur.

### <a name="automation"></a>Otomasyon
Kaynaklarını ayrı ayrı yönetmek, olan her ikisini birden çok zaman alır ve büyük olasılıkla hataya belirli işlemler için. Azure, Azure Automation, Logic Apps ve Azure işlevleri dahil olmak üzere çeşitli Otomasyon özellikleri sağlar. [Azure Otomasyonu](../automation/automation-intro.md) oluşturmak ve kaynakları yönetme, ortak görevler işlemek için runbook'ları tanımlamak yöneticilerin sağlar. PowerShell Kod düzenleyicisinde veya bir grafik Düzenleyicisi kullanılarak runbook'lar oluşturun. Karmaşık çok aşamalı iş akışları oluşturabilir. Azure Otomasyonu, çoğunlukla kullanılmayan kaynakları kapatılıyor veya İnsan eli gerek kalmadan yanıt olarak belirli bir tetikleyicinin kaynakları oluşturma gibi genel görevleri işlemek için kullanılır.

> [!TIP]
> Otomasyon için:
> * Bir Azure Otomasyonu hesabı oluşturun ve kullanılabilir runbook'ları (grafik ve komut satırı) bulunan gözden [Runbook Galerisi](../automation/automation-runbook-gallery.md).
> * İçeri aktarma ve anahtar runbook'ları, kendi kullanımınız için özelleştirebilirsiniz.
> 
> Yaygın bir senaryo Başlat/Kapat sanal makinelere bir zamanlamaya göre özelliğidir. Bu senaryoyu ele hem genişletmek öğretmek galerisinde kullanılabilir olan örnek runbook'lar vardır.
> 
> 

## <a name="azure-security-center"></a>Azure Güvenlik Merkezi
Belki de benimseme buluta büyük blockers biri üzerinde güvenliği endişelere olmuştur. BT risk yöneticileri ve güvenlik Departmanlar Azure kaynaklarında güvenli olduğundan emin olmak gerekir. 

[Azure Güvenlik Merkezi](../security-center/security-center-intro.md) Aboneliklerde kaynakların güvenlik durumu merkezi bir görünümünü sağlar ve tehlike giren kaynaklarını önlemeye yardımcı öneriler sağlar. Daha ayrıntılı ilkeleri (örneğin, uygulama ilkeleri kendi duruş ele alır riske uyarlamak Kurumsal izin belirli kaynak gruplarına) etkinleştirebilirsiniz. Son olarak, Azure Güvenlik Merkezi Microsoft iş ortakları ve bağımsız yazılım satıcıları yeteneklerini geliştirmek için Azure Güvenlik Merkezi'nde takılan yazılım oluşturmasına olanak tanıyan bir açık platformudur. 

> [!TIP]
> Azure Güvenlik Merkezi, her Abonelikteki varsayılan olarak etkindir. Ancak, Azure Güvenlik Merkezi, aracıyı yüklemek ve veri toplamaya başlamak izin vermek için sanal makinelerden veri toplama etkinleştirmeniz gerekir.
> 
> ![Veri toplama](./media/resource-manager-subscription-governance/data-collection.png)
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
* Abonelik idare hakkında öğrendiniz, uygulamada bu önerilerini görmek için zaman yapılır. Bkz: [Azure aboneliği idare uygulamanın örnekleri](resource-manager-subscription-examples.md).

