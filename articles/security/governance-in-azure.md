---
title: Azure'da idare | Microsoft Docs
description: "Geniş işlem örnekleri dahil bulut tabanlı bilgi işlem Hizmetleri & Yukarı ve aşağı otomatik olarak uygulamanızı veya Kurumsal ihtiyaçlarını karşılamak üzere ölçeği hizmetleri hakkında bilgi edinin."
services: security
documentationcenter: na
author: UnifyCloud
manager: swadhwa
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/01/2017
ms.author: TomSh
ms.openlocfilehash: 03e86448a1b0a13addab789bf2aea62e5d84149b
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2017
---
# <a name="governance-in-azure"></a>Azure Yönetimi

Güvenlik bulutta iş ve nasıl önemli Azure güvenliği hakkında doğru ve güncel bilgi bulma olduğunu olduğunu biliyoruz. Çeşitli güvenlik araçları ve yetenekleri yararlanmak için uygulamalar ve hizmetler için Azure kullanmak için en iyi nedenlerinden biridir. Bu araçları ve yetenekleri güvenli Azure platformunda güvenli çözümler oluşturmak mümkün kılar yardımcı olur.

İdare dizisi daha iyi anlamanıza yardımcı olması için Müşteri'nin ve Microsoft operations perspektiften, bu makalede, "İdare içinde Azure", Microsoft Azure içinde uygulanan denetimleri yazılır idare kapsamlı bir bakış sağlar Microsoft Azure ile kullanılabilen özellikleri.

## <a name="azure-platform"></a>Azure platformu

Azure işletim sistemleri, programlama dilleri, çerçeveleri, Araçlar, veritabanları ve aygıtları geniş çapta destekleyen bir genel bulut hizmeti platformudur. Linux kapsayıcıları Dockers tümleşme çalıştırabilirsiniz; JavaScript, Python, .NET, PHP, Java ve Node.js ile uygulamalar oluşturma; Yapı geri-iOS, Android ve Windows cihazları sona erer. Azure genel bulut Hizmetleri aynı teknolojileri geliştiriciler milyonlarca destekler ve BT uzmanları zaten kullanır ve güven.

Oluşturmanıza veya BT varlıklarına geçirmek, uygulamalar ve hizmetler ve denetimleri ile verileri korumak için bir kuruluşun yeteneklerini güvenmek bir genel bulut hizmeti sağlayıcısı bulut tabanlı varlıklarınızı güvenliği yönetmek için sağlarlar.

Azure'nın altyapı tesis aynı anda milyonlarca müşteri barındırmak için uygulamalar için tasarlanmıştır ve güvenlik gereksinimlerine bağlı işletmeler karşılayabilecek güvenilir bir temel sağlar. Ayrıca, Azure, çok sayıda güvenlik seçenekleri ve kuruluşunuzun dağıtımları benzersiz gereksinimlerini karşılamak için güvenlik özelleştirebilirsiniz böylece bunları denetleme olanağı sağlar.

Bu belge, Azure idare özellikleri bu gereksinimleri karşılamak nasıl yardımcı olabileceğini anlamanıza yardımcı olur.

## <a name="abstract"></a>Soyut

Microsoft Azure bulut idare bir tümleşik denetim ve gözden geçirmek ve Azure platformu kullanıcıların kullanımına kuruluşlar bildiren için danışmanlık yaklaşımı sağlar. Microsoft Azure bulut idare başvurduğu işlerinizdeki karar verme işlemlerini, ölçüt ve ilkeleri planlama mimarisi, edinme, dağıtım, işlem ve bir bulutun yönetim ilgili bilgi işlem.

Microsoft Azure bulut idare yönelik bir plan oluşturmak için kişiler, işlemleri ve teknolojileri ilişkin ayrıntılı bir bakış şu anda gerçekleşmesi ve kolaylaştırır çerçeveler oluşturmak gereken sürekli olarak son kullanıcıların sağlarken iş gereksinimlerini desteklemek için BT Microsoft Azure güçlü özelliklerini kullanma esnekliği.

Bu yazı, BT kaynaklarınız Microsoft Azure İdaresi yükseltilmiş bir düzeyde nasıl elde edebilirsiniz açıklar. Bu yazı, Microsoft Azure için yerleşik güvenlik ve idare özellikleri anlamanıza yardımcı olabilir.

Bu yazıda, idare sorunları ele ana şunlardır:

- Uygulama ilkeleri, işlemler ve yordamlar kuruluş hedefleri göredir.

- Güvenlik ve kuruluş standartlarıyla sürekli uyumluluk

- Uyarı verme ve izleme

## <a name="implementation-of-policies-processes-and-procedures"></a>Uygulama ilkeleri, işlemler ve yordamlar 

Yönetim rolleri ve sorumlulukları bilgi güvenlik ilkesi ve işletimsel sürekliliği uygulaması Azure işleten oluşturmuştur. Microsoft Azure yönetim, güvenlik ve ilgili takımlar (üçüncü tarafların dahil olmak üzere) ve güvenlik ilkeleri, işlemleri ve standartları ile gönderilmesini kolaylaştıran Uyumluluk dahilinde sürekliliği yöntemler gözlemledikten sorumludur.

Gelişen faktörleri şunlardır:

- Hesap Sağlama

- Abonelik denetimleri

- Rol tabanlı erişim denetimleri

- Kaynak Yönetimi

- Kaynak İzleme

- Kritik kaynak denetimi

- Faturalama bilgileri API erişimi

- Ağ denetimleri

## <a name="account-provisioning"></a>Hesap sağlama

Hesap hiyerarşisi tanımlama kullanın ve Azure yapısı için büyük bir adımı şirket içinde Hizmetleri ve çekirdek idare yapısı olur. Kurumsal anlaşma ile müşteriler durumunda, müşterilerin daha fazla Departmanlar, hesapları ve son olarak, abonelikleri ortamına ayırabilir.

![Hesap Sağlama](./media/governance-in-azure/security-governance-in-azure-fig1.png)

Bir Kurumsal Anlaşma yoksa kullanmayı [Azure etiketleri](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) hiyerarşisi tanımlamak için abonelik düzeyinde. Bir Azure aboneliği tüm kaynakların bulunduğu temel birimidir. Ayrıca, Azure, çekirdek, kaynaklar, vb. sayısı gibi birkaç sınırlarda tanımlar. Abonelikleri içerebilir [kaynak grupları](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview), kaynakları içerebilir. [RBAC](https://docs.microsoft.com/azure/api-management/api-management-role-based-access-control) ilkeler bu üç düzeyde uygulanır.

Her Kuruluş farklıdır ve kurumsal olmayan müşteriler durumunda Azure etiketleri kullanarak hiyerarşi Azure şirket içinde nasıl düzenlendiğini önemli esneklik sağlar. Microsoft Azure kaynaklarında dağıtmadan önce hiyerarşi model ve faturalama, kaynak erişimi ve karmaşıklık üzerindeki etkisini anlamak gerekir.

## <a name="subscription-controls"></a>Abonelik denetimleri

Abonelik, kaynak kullanımı nasıl bildirilen ve fatura denetler. Abonelikleri Kurulum ayrı faturalandırma ve ödeme için olabilir. Belirtilen önceki altında bir Azure hesabı olarak biz birden fazla abonelik olabilir. Abonelikler, bir şirkette birden çok Departmanlar Azure kaynak kullanımını belirlemek için kullanılabilir.

Örneğin, bir şirket varsa BT, İK ve pazarlama departmanları ve bu Departmanlar çalıştıran farklı projelere sahip. Sanal makineler gibi Azure kaynaklarını kullanımını her departmana göre bağlı olarak, bunlar uygun şekilde fatura. Bu her bölüm Finans denetleyebilirsiniz.

Azure abonelikleri üç parametre kurar:

- benzersiz abone kimliği

- Fatura konumu

- Kullanılabilir kaynak kümesi

Tüketim sınırları, abonelik türüne bağlı olarak Microsoft zorlar karşın, bir kullanıcı için bir Microsoft hesabı kimliği, kredi kartı numarası ve Azure Hizmetleri--tam dizisi içerir.

Azure kayıt hiyerarşileri, hizmetleri bir Kurumsal Anlaşma içinde nasıl yapılandırıldığı tanımlayın. Enterprise Portal esnek hiyerarşileri kuruluşun benzersiz gereksinimlerine göre özelleştirilebilir dayalı bir kurumsal anlaşma ile ilişkili Azure kaynaklarına erişimi bölmek olanak tanır. Böylece ilişkili faturalama ve kaynak erişim doğru şekilde olunması hiyerarşi düzeni kuruluşun yönetim ve coğrafi yapısı eşleşmelidir.

Üç üst düzey desenleri, hesap gruplandırmaları için yönetimsel bir yapı olarak işlev, iş birimi ve coğrafi, kullanarak Departmanlar alır. Her bölüm içinde hesapları siloları fatura ve birkaç anahtar sınırları için (örneğin, sayısı VM'ler, depolama hesapları, vb.) oluşturma abonelikleri atanabilir.

![Abonelik denetimleri](./media/governance-in-azure/security-governance-in-azure-fig2.png)


Bir Kurumsal Anlaşma olan kuruluşlar için Azure abonelikleri dört düzeyli bir hiyerarşiye izleyin:

- Kuruluş kayıt Yöneticisi

- Bölüm Yöneticisi

- hesap sahibi

- Hizmet yöneticisi

Bu hiyerarşide aşağıdaki yönetir:

- Faturalama ilişkisi

- Hesap Yönetimi

- Rol tabanlı erişim denetimi (RBAC) yapılara

- Sınırları/sınırları

- Sınırları

  - Kullanım ve fatura (teklif sayılara göre oranı kartı)

  - Sınırlar

  - Sanal Ağ

- 1 AAD bağlı (1 AAD birçok aboneliği ile ilişkili)

- Bir kurumsal kayıt hesapla ilişkili

## <a name="role-based-access-controls"></a>Rol tabanlı erişim denetimleri

Azure başlangıçta yayımlandığında, bir abonelik için erişim denetimleri temel: Yöneticisi veya ortak Yöneticisi. Klasik modeli kapsanan tüm kaynaklara erişim izni Portalı'nda bir abonelikte erişim sağlar. Bu eksiklik hassas bir denetim için bir Azure kayıt düzeyi makul erişim denetimi sağlamak için abonelikleri çoğalmasıdır gerektiriyordu.

![Rol tabanlı erişim denetimleri](./media/governance-in-azure/security-governance-in-azure-fig3.png)

Bu abonelikleri çoğalmasıdır artık gerekli değildir. Rol tabanlı erişim denetimi ile standart rolleri (örneğin, "okuyucu" ve "yazıcı" karşılaşılan rollerinin) kullanıcıları atayabilirsiniz. Özel roller tanımlayabilir.

[Azure rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) Azure için ayrıntılı erişim yönetimi sağlar. RBAC kullanarak, yalnızca kullanıcıların işlerini yapmak için gereksinim duyduğu erişim miktarını verebilirsiniz. Güvenlik odaklı şirketler çalışanlar gereksinim duydukları izinleri tam vermiş odaklanmanız gerekir. Çok fazla izinler saldırganlar bir hesaba kullanıma sunar. Çok az izinleri çalışanlar verimli bir şekilde işlerini alınamıyor anlamına gelir. Azure rol tabanlı erişim denetimi (RBAC), Azure için ayrıntılı erişim yönetimi sunarak bu sorunu gidermeye yardımcı olur. RBAC ekibiniz içinde görevleri kurabilmeleri ve işlerini yapmak için gereksinim duydukları kullanıcılara sadece erişim miktarını verebilirsiniz yardımcı olur. Kısıtlanmamış izinlerini herkes vermek yerine, Azure aboneliği veya kaynakları yalnızca belirli eylemleri izin verebilirsiniz.

Örneğin, bir çalışan başka bir SQL veritabanları aynı abonelik içindeki yönetebilirsiniz sırada bir Abonelikteki sanal makineleri yönetme izin vermek için RBAC kullanın.

Azure RBAC tüm kaynak türleri için geçerli üç temel rol vardır:

- **Sahibi** temsilci başkalarına erişimi hakkı dahil olmak üzere tüm kaynaklara tam erişimi vardır.

- **Katkıda bulunan** olabilir oluşturun ve tüm türlerini Azure kaynaklarını yönetmek ancak başkalarına erişim izni veremiyor.

- **Okuyucu** mevcut Azure kaynaklarını görüntüleyebilirsiniz.

Azure RBAC rollerin geri kalanı belirli Azure kaynaklarının yönetimini sağlar. Örneğin, sanal makine katılımcı rolü oluşturmak ve sanal makineleri yönetmek kullanıcının sağlar. Bunları erişimi sanal ağ veya sanal makine bağlandığı alt sağlamaz.

[RBAC yerleşik rolleri](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) mevcut rolleri listeler. Her yerleşik rol kullanıcılara veren kapsam ve işlemleri belirtir.

Kullanıcılar, gruplar ve belirli bir kapsamda uygulamalara uygun RBAC rolü atayarak erişim sağla. Rol atamasının kapsamı, bir abonelik, bir kaynak grubu veya tek bir kaynak olabilir. Bir üst kapsamda atanan bir rolü de içerdiği alt öğelerine erişim verir.

Örneğin, bir kaynak grubu erişimi olan bir kullanıcı, Web siteleri, sanal makineler ve alt ağlar gibi içerdiği tüm kaynaklar yönetebilirsiniz.

Azure RBAC Azure portalı ve Azure Resource Manager API'leri yalnızca Azure kaynaklarını yönetim işlemlerini destekler. Azure kaynakları için tüm veri düzeyi işlemleri yetkilendirilemiyor. Birisi depolama hesaplarını yönetmek için yetkilendirmek Örneğin, ancak BLOB veya bir depolama hesabı içindeki tabloları olamaz. Benzer şekilde, bir SQL veritabanı, içindeki tabloları ancak yönetilebilir.

RBAC'nin erişimi yönetmenize nasıl yardımcı olduğu konusunda daha fazla bilgi isterseniz bkz. [Rol Tabanlı Erişim Denetimi Nedir?](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is).

Ayrıca [özel bir rol oluşturmak](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles) yerleşik roller hiçbiri belirli erişiminizi karşılamıyorsa Azure rol tabanlı erişim denetimi (RBAC) gerekir. Özel roller kullanılarak oluşturulabilir [Azure PowerShell](https://docs.microsoft.com/azure/active-directory/role-based-access-control-manage-access-powershell), [Azure komut satırı arabirimi (CLI)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-manage-access-azure-cli)ve [REST API](https://docs.microsoft.com/azure/active-directory/role-based-access-control-manage-access-rest). Yalnızca yerleşik roller gibi özel roller, kullanıcılar, gruplar ve uygulamalar abonelik, kaynak grubu ve kaynak kapsamlar için atanabilir.

Her abonelikte en fazla 2000 rol ataması verebilirsiniz.

## <a name="resource-management"></a>Kaynak yönetimi

Azure başlangıçta yalnızca klasik dağıtım modeli sağlanan. Bu modelde, bağımsız olarak her bir kaynağın var; İlgili kaynaklar gruplamak için hiçbir yolu yoktu. Bunun yerine, el ile çözüm ya da uygulama yapılan hangi kaynaklara izlemek ve eşgüdümlü bir yaklaşım yönetmeyi unutmayın zorunda kaldı.

Bir çözümü dağıtmak için Azure portalı üzerinden ayrı ayrı her bir kaynak oluşturmak veya tüm kaynakların doğru sırada dağıtılan bir komut dosyası oluşturmak zorunda kalındı. Bir çözümü silmek için ayrı ayrı her bir kaynağın silme gerekiyordu. Kolayca uygulanır ve ilgili kaynaklar için erişim denetimi ilkeleri güncelleştirin. Son olarak, geçerli yardımcı şartlarını etiket kaynaklarına etiketleri kaynaklarınızı izleme ve faturalama yönetme.

2014'te Azure Kaynak Yöneticisi, bir kaynak grubu kavramı eklenen kullanıma sunuldu. Bir kaynak grubu, ortak bir yaşam döngüsü paylaşmak kaynaklar için bir kapsayıcıdır. Resource Manager dağıtım modeli çeşitli avantajlar sunar:

- Dağıtmak, yönetmek ve bu işleme ayrı ayrı hizmetleri yerine, çözümünüz için tüm Hizmetleri Grup olarak izleme.

- Art arda çözümünüzü yaşam döngüsü dağıtmak ve kaynaklarınızın tutarlı bir durumda dağıtılması da size güven verir.

- Kaynak grubunuzdaki tüm kaynaklara erişim denetimini uygulayabilirsiniz ve yeni kaynaklar kaynak grubuna eklendiğinde bu ilkeleri otomatik olarak uygulanır.

- Aboneliğinizdeki tüm kaynakları mantıksal olarak düzenlemek için kaynaklarınıza etiketler ekleyebilirsiniz.

- Çözümünüz için altyapıyı tanımlamak için JavaScript nesne gösterimi (JSON) kullanabilirsiniz. JSON dosyasının bir Resource Manager şablonu olarak bilinir.

- Doğru sırayla dağıtılmalarını sağlamak için kaynaklarınız arasındaki bağımlılıkları tanımlayabilirsiniz.

![Kaynak Yönetimi](./media/governance-in-azure/security-governance-in-azure-fig4.png)

Resource Manager, kaynakları yönetim, faturalama veya doğal benzeşimi anlamlı gruplar halinde yerleştirilmesine olanak sağlar. Daha önce belirtildiği gibi Azure iki dağıtım modeline sahiptir. Önceki içinde [Klasik modeli](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model), temel yönetim birimidir aboneliği karşılaşıldı. Çok sayıda abonelikleri oluşturulmasına neden bir abonelik içindeki kaynaklara ayırmanız zordu. Resource Manager modeli ile kaynak gruplarının giriş gördük.

Bir kaynak grubu Azure çözümünü ilgili kaynaklara tutan bir kapsayıcıdır. [Kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) çözüm için tüm kaynakları veya yalnızca bir grup olarak yönetmek istediğiniz kaynakları içerebilir. Kuruluş için önemli olan faktörleri temel alarak kaynakları kaynak gruplarına nasıl ayıracağınıza siz karar verirsiniz.

Şablonlar hakkında öneriler için bkz. [Azure Resource Manager şablonları oluşturmaya yönelik en iyi uygulamalar](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices).

Azure Resource Manager kaynakların doğru sırada oluşturulmasını sağlamak için bağımlılıkları analiz eder. Bir kaynak başka bir kaynaktaki değere bağımlıysa (diskler için depolama hesabına ihtiyaç duyan bir sanal makine gibi) bir bağımlılık ayarlayın.

>[!Note]
>Daha fazla bilgi için bkz. [Azure Resource Manager’da bağımlılıkları tanımlama](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-define-dependencies).

Ayrıca, şablonu altyapıda yapılan güncelleştirmeler için kullanabilirsiniz. Örneğin, çözümünüze bir kaynak ekleyebilir ve zaten dağıtılmış kaynaklar için yapılandırma kuralları ekleyebilirsiniz. Şablon zaten var olan bir kaynak için bir kaynak oluşturulmasını belirtiyorsa, Azure Resource Manager yeni bir varlık oluşturmak yerine bir güncelleştirme gerçekleştirir. Azure Resource Manager varlığı yeni oluşturulacak kaynak ile aynı duruma gelecek şekilde güncelleştirir.

Kurulumunda bulunmayan yazılım yükleme gibi ek işlemlere ihtiyaç resource Manager senaryoları için uzantılar sağlar.

## <a name="resource-tracking"></a>Kaynak İzleme

Kuruluşunuzdaki kullanıcılar kaynakları aboneliğe eklemek gibi kaynakları uygun departmanı, müşteri ve ortam ile ilişkilendirmek giderek daha çok önemli hale gelir. Meta veri kaynaklarına etiketleri üzerinden ekleyebilirsiniz. Kullandığınız [etiketleri](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) kaynak veya sahibi hakkında bilgi sağlamak için. Etiketleri yalnızca toplama ve kaynakları çeşitli şekillerde gruplamak, ancak bu verileri geri ödeme amaçları için kullanmak üzere etkinleştirin.

Karmaşık bir kaynak grubu ve kaynak koleksiyonunuz olduğunda ve bu varlıkları sizin için anlamlı bir şekilde görselleştirmeniz gerektiğinde etiketleri kullanabilirsiniz. Örneğin, kuruluşunuzda benzer görevleri üstlenen veya aynı departmana ait olan kaynakları etiketleyebilirsiniz.

Kuruluşunuzdaki kullanıcılar etiketleri kullanmadan birden fazla kaynak oluşturduğunda, bunları daha sonra tanımlamak ve yönetmek zor olabilir. Örneğin, bir proje için tüm kaynakları silmek isteyebilirsiniz. Bu kaynaklar için proje etiketlenir değil, bunları el ile bulmalıdır. Etiketleme, aboneliğinizden doğan gereksiz maliyetleri azaltmanın önemli bir yoludur.

Kaynakların bir etiketi paylaşması için aynı kaynak grubunda bulunmaları gerekmez. Kuruluşunuzdaki tüm kullanıcıların yanlışlıkla birbirinden kısmen farklı etiketler (örneğin “departman” yerine “depart.”) kullanmak yerine genel etiketler kullanmasını sağlamak için kendi etiket sınıflandırmanızı oluşturabilirsiniz.

Kaynak ilkeler, kuruluşunuz için standart kurallar oluşturmanıza olanak sağlar. Kaynakları uygun değerlerle etiketlenir olun ilkeleri oluşturabilirsiniz.

> [!Note]
> Daha fazla bilgi için bkz: [faturalama İlkesi Initiative etiketleri](../azure-policy/scripts/billing-tags-policy-init.md).

Azure portalından etiketli kaynakları da görüntüleyebilirsiniz.

Aboneliğinize ait [kullanım raporu](https://docs.microsoft.com/azure/billing/billing-understand-your-bill), maliyetleri etiketlere göre ayırmanızı sağlayan etiket adları ve değerler içerir.

> [!Note]
> Etiketler hakkında daha fazla bilgi için bkz: [faturalama İlkesi Initiative etiketleri](../azure-policy/scripts/billing-tags-policy-init.md).

Etiketler için aşağıdaki sınırlamalar geçerlidir:

- Her kaynak veya kaynak grubu en fazla 15 etiketi anahtar/değer çifti olabilir. Bu sınırlama kaynak grubu veya kaynak doğrudan uygulanan etiketleri yalnızca uygular. Bir kaynak grubu, her 15 etiketi anahtar/değer çiftleri sahip birçok kaynakları içerebilir.

- Etiket adı 512 karakterle sınırlıdır.

- Etiket değeri 256 karakterle sınırlıdır.

- Kaynak grubuna uygulanan etiketler, bu kaynak grubundaki kaynaklar tarafından devralınmaz.

Bir kaynak ile ilişkilendirmeniz gereken 15'ten fazla değer varsa, etiket değeri için JSON dizesi kullanın. JSON dizesindeki tek etiket anahtarına uygulanan birçok değerler içerebilir.

### <a name="tags-and-billing"></a>Etiketleri ve faturalama

Etiketlerin fatura verilerinizi gruplandırmak etkinleştirin. Örneğin, birden çok VM farklı kuruluşlarda çalıştırıyorsanız, etiketleri Grup kullanım için maliyet merkezi tarafından kullanın. Etiketler, maliyetleri çalışma zamanı ortamı tarafından kategorilere ayırmak için de kullanabilirsiniz; Üretim ortamında çalışan sanal makineler için fatura kullanımı gibi.

Etiketler hakkında bilgi alabilir [Azure kaynak kullanımı ve RateCard API'leri](https://docs.microsoft.com/azure/billing/billing-usage-rate-card-overview) veya kullanım virgülle ayrılmış değerler (CSV) dosyası. Kullanım dosyasını indirin [Azure hesap portalını](https://account.windowsazure.com/) veya [EA portal](https://ea.azure.com/).

>[!Note]
> Faturalandırma bilgileri programlı erişim hakkında daha fazla bilgi için bkz: [Microsoft Azure kaynak tüketimini Öngörüler elde](https://docs.microsoft.com/azure/billing/billing-usage-rate-card-overview). REST API işlemleri için bkz: [Azure faturalama REST API Başvurusu](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c).

Faturalama etiketleriyle destekleyen hizmetler için kullanım CSV yüklediğinizde, etiketler etiketleri sütununda görünür.

## <a name="critical-resource-controls"></a>Kritik kaynak denetimleri

Kuruluşunuz için abonelik Çekirdek Hizmetleri ekler gibi bu hizmetleri iş kesintisi yaşamamak kullanılabilir olmasını sağlamak giderek daha çok önemli olur. [Kaynak kilitleri](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-lock-resources) değiştirilmesini veya silinmesini sahip olduğu uygulamalar veya Bulut altyapısı üzerinde önemli bir etkisi yüksek değerli kaynaklardaki işlemlerini kısıtlamak etkinleştirin. Kilitleri, bir abonelik, kaynak grubuna ya da kaynağa uygulayabilirsiniz. Genellikle, sanal ağlar, ağ geçitleri ve depolama hesapları gibi temel kaynaklarına kilitleri uygulayın.

Kaynak kilitleri şu anda iki değer destekler: CanNotDelete ve salt okunur. Kullanıcılar (uygun haklarıyla) hala CanNotDelete anlamına gelir okuma veya bir kaynak değiştirme ancak bunu silemezsiniz. Salt okunur, yetkili kullanıcıların silemez veya bir kaynak değiştirme olduğunu anlamına gelir.

Resource Manager kilitleri uygulamak gönderilen işlemleri oluşan yönetim düzeyi gerçekleşen işlemlerine <https://management.azure.com>. Kilitler nasıl kaynakları kendi işlevleri gerçekleştirmek kısıtlamaz. Kaynak kısıtlı değişir, ancak kaynak işlemlerinin sınırlı değildir. Örneğin, bir SQL veritabanı salt okunur kilit silme veya veritabanı değiştirme engeller, ancak bu, oluşturma, güncelleştirme veya silme verilerden veritabanındaki engellemez.

Uygulama **ReadOnly** gibi görünen bazı işlemler gerektirir ek eylemleri okuma olduğundan beklenmeyen sonuçlara yol açabilir. Örneğin, yerleştirme bir **ReadOnly** kilit bir depolama hesabında anahtarları listeleme tüm kullanıcıları engeller. Yazma işlemlerini listenin döndürülen anahtarları için kullanılabilir olmadığından anahtarları işlemi bir POST isteği gerçekleştirilir.

![Kritik kaynak denetimleri](./media/governance-in-azure/security-governance-in-azure-fig5.png)

Başka bir örnek için bir uygulama hizmeti kaynakta bir salt okunur kilidi yerleştirme, Visual Studio Sunucu Gezgini'bu etkileşimi yazma erişimi gerektirdiğinden kaynak dosyaları görüntülenmesini engeller.

Rol tabanlı erişim denetimi farklı olarak, tüm kullanıcılar ve roller bir kısıtlama uygulamak için yönetim kilitleri kullanın. Kullanıcılar ve roller için izinleri ayarlama bilgi edinmek için [Azure rol tabanlı erişim denetimi](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure).

Bir üst kapsamda kilit uyguladığınızda, kapsamı içindeki tüm kaynakların aynı kilit devralır. Daha sonra eklediğiniz bile kaynakları kilidi üst devralır. Devralmada en kısıtlayıcı kilidi önceliklidir.

Oluşturmak veya yönetim kilitleri silmek için Microsoft.Authorization/ erişimi olmalıdır _veya Microsoft.Authorization/locks/_ eylemler. Yerleşik roller, yalnızca **sahibi** ve **kullanıcı erişimi Yöneticisi** bu eylemleri verilir.

## <a name="api-access-to-billing-information"></a>Faturalandırma bilgileri API erişimi

Azure faturalama API'leri kullanımı ve kaynak veri çekmek için tercih edilen veri analizi araçlarınızı kullanın. Azure kaynak kullanımı ve RateCard API'leri doğru şekilde tahmin etmek ve maliyetlerinizi yönetmenize yardımcı olabilir. API'ler bir kaynak sağlayıcısı ve Azure Resource Manager tarafından kullanıma sunulan API ailesinin bir parçası olarak uygulanır.

### <a name="azure-resource-usage-api-preview"></a>Azure kaynak kullanım API'si (Önizleme)

Azure kullanmak [kaynak kullanım API'si](https://msdn.microsoft.com/library/azure/mt219003) tahmini Azure tüketim verilerinizi almak için. API içerir:

- **Azure rol tabanlı erişim denetimi** -yapılandırma erişim ilkeleri [Azure portal](https://portal.azure.com/) aracılığıyla veya [Azure PowerShell cmdlet'lerini](https://docs.microsoft.com/powershell/azure/overview) hangi kullanıcılar veya uygulamalar için erişim sağlayabilmek için belirtmek için Aboneliğin kullanım verileri. Arayanlar, kimlik doğrulaması için standart Azure Active Directory belirteçleri kullanmanız gerekir. Çağıran, belirli bir Azure aboneliği kullanım verileri erişmek için faturalama Okuyucu, okuyucu, sahibi veya katkıda bulunan rolü ekleyin.

- **Saatlik veya günlük toplamalar** - arayanlar olup Azure kullanım verilerini saatlik istedikleri aralıkları belirtebilirsiniz veya günlük aralıkları. Varsayılan günlük.

- **(Kaynak etiketleri içerir) örneği meta veri** – alma tam Kaynak URI gibi örnek düzeyi ayrıntısı (/subscriptions/ {subscrıptıon-ID} /..), kaynak grubu bilgileri ve kaynak etiketleri. Çapraz ücretlendirme kullanım örnekleri ister için bu meta veriler, belirleyici biçimde ve program aracılığıyla kullanım etiketlere göre ayırmak yardımcı olur.

- **Kaynak meta verilerini** -kaynak ayrıntılarını ölçüm adı, ölçer kategori, ölçüm alt kategorisi, birim ve bölge gibi daha iyi ne tüketilen anlamak çağıran verin. Ayrıca Azure portalı kaynak meta verileri terminolojisi hizalamak için çalışıyoruz Azure kullanım CSV, CSV ve diğer genel kullanıma yönelik deneyimleri deneyimleri arasında verilerin bağıntısını olanak faturalama EA.

- **Tüm kullanım teklif türleri** – kullanım verilerini, tüm Kullandıkça Öde, MSDN, parasal taahhüt, kredi ve EA gibi türlerinde sunmak için kullanılabilir.

**Azure kaynak RateCard API (Önizleme)**

Mevcut Azure kaynakları ve her biri için tahmini fiyatlandırma bilgileri listesini almak için Azure kaynak RateCard API'sini kullanın. API içerir:

- **Azure rol tabanlı erişim denetimi** - Azure portalında veya hangi kullanıcıların belirtmek için Azure PowerShell cmdlet'leri aracılığıyla erişim ilkelerinizi yapılandırmak veya uygulamaları RateCard verilere erişim alabilirsiniz. Arayanlar, kimlik doğrulaması için standart Azure Active Directory belirteçleri kullanmanız gerekir. Çağıran, belirli bir Azure aboneliği kullanım verileri erişmek için okuyucu, sahibi veya katkıda bulunan rolü ekleyin.

- **Kullandıkça Öde, MSDN, parasal taahhüt ve kredi desteği sunar (EA desteklenmiyor)** -bu API Azure teklifi düzeyi oranı bilgi sağlar. Bu API'yi çağıran kaynak ayrıntılarını ve ücretlerin almak için teklif bilgileri geçmesi gerekir. EA teklifleri kayıt göre oranları özelleştirdikten çünkü EA oranları sağlamak şu anda kaydedemiyoruz. Kullanım ve RateCard API'leri birleşimiyle olası hale getirilen senaryolardan bazıları şunlardır:

- **Azure ay sırasında harcadığı** - kullanım birleşimini kullanın ve RateCard API'ları, bulut daha iyi fikir almak için bir ay sırasında harcadığı. Kullanım ve Ücret tahminleri saatlik ve günlük demet analiz edebilirsiniz.

- **Uyarıları ayarlamak** – tahmini bulut kullanımı ve ücretleri almak ve kaynak veya parasal tabanlı uyarıları ayarlamak için kullanım ve RateCard API'lerini kullanın.

- **Fatura tahmin** – tahmini tüketim ve bulut harcamanız ve fatura fatura döneminin sonunda ne olacağını tahmin etmek için makine öğrenimi algoritmalarını uygulamak alın.

- **Analiz maliyetiyle öncesi tüketim** –, iş yükleri için Azure taşıdığınızda ne kadar faturanızı beklenen kullanımınız için olacaktır tahmin etmek için RateCard API'sini kullanın. Ayrıca var olan iş yükleri diğer Bulut veya özel bulutlara varsa, Azure ile kullanımınızı eşleştirebilirsiniz oranları Azure daha iyi kestirmek için harcadıkları. Bu tahmin teklifini Özet ve parasal taahhüt ve kredi gibi Kullandıkça Öde ötesinde farklı teklif türleri arasında karşılaştırma olanağı sağlar. API da bölgeye göre maliyet farklılıkları görme olanağı verir ve dağıtım kararları vermenize yardımcı olmak için durum maliyet çözümlemesi yapmanıza izin verir.

- **Çözümlemeleri** -başka bir bölgede ya da başka bir Azure kaynak yapılandırması iş yüklerini çalıştırmak için daha uygun maliyetli olup olmadığını belirleyebilirsiniz. Azure kaynak maliyetleri kullanmakta olduğunuz Azure bölgesinde değişebilir.

- Başka bir Azure Teklif türü bir Azure kaynağı üzerinde daha iyi oranı sağlar, ayrıca belirleyebilirsiniz.

## <a name="networking-controls"></a>Ağ denetimleri

Kaynaklara erişimi (içinde corporation'ın ağ) iç veya dış (Internet) aracılığıyla olabilir. Yanlışlıkla kaynakları içinde yanlış yere koyun ve potansiyel olarak bunları kötü amaçlı erişime açmak, kuruluşunuzdaki kullanıcılar için kolay bir işlemdir. Olduğu gibi şirket içi / aygıtlar, kuruluşların Azure kullanıcıların doğru kararlar emin olmak için uygun denetimleri eklemelisiniz.

![Ağ denetimleri](./media/governance-in-azure/security-governance-in-azure-fig6.png)

Abonelik Yönetimi için temel Denetim erişim sağlayan çekirdek kaynakları belirleyin. Çekirdek kaynakları oluşur:

### <a name="network-connectivity"></a>Ağ bağlantısı

[Sanal ağlar](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) alt ağlar için kapsayıcı nesnelerdir. Kesinlikle gerekli olsa, uygulamaları dahili şirket kaynaklarına bağlanırken çoğunlukla kullanılır. Azure sanal ağ hizmeti, güvenli bir şekilde Azure kaynaklarını birbirlerine sanal ağlar (Vnet'ler) bağlanmanıza olanak sağlar.

Bir VNet kendi ağ bulutta gösterimidir. Bir VNet Azure bulutunun aboneliğinize adanmış mantıksal bir yalıtım ' dir. Ayrıca, sanal ağlar, şirket içi ağınıza bağlanabilir.

Azure sanal ağlar için özellikleri şunlardır:

- **Yalıtım**: sanal ağlar birbirinden yalıtılmış. Geliştirme, test ve üretim için aynı CIDR adres bloklarını kullanan ayrı Vnet'ler oluşturabilirsiniz. Buna karşılık, ağları birbirine bağlamak ve farklı CIDR adres bloklarını kullanan birden çok sanal ağlar oluşturabilirsiniz. Bir sanal ağ birden çok alt ağa bölebilirsiniz. Azure, bir sanal ağa bağlı VM'ler ve bulut Hizmetleri rol örnekleri için dahili ad çözümlemesi sağlar. İsteğe bağlı olarak bir VNet Azure dahili ad çözümlemesi kullanmak yerine kendi DNS sunucularını kullanmak için yapılandırabilirsiniz.

- **Internet bağlantısı**: bir sanal ağa bağlı tüm Azure sanal makineler (VM) ve bulut Hizmetleri rol örnekleri varsayılan olarak, Internet erişimi vardır. Belirli kaynaklara gelen erişim gerektiğinde de etkinleştirebilirsiniz.

- **Azure kaynak bağlantısı**: Bulut Hizmetleri ve sanal makineleri gibi Azure kaynaklarını aynı Vnet'e bağlı. Farklı alt ağlarda olsalar bile, kaynakları birbirlerine özel IP adresleri kullanarak bağlanabilir. Azure, yapılandırmak ve yollar yönetmek zorunda kalmamak için varsayılan alt ağlar, sanal ağlar ve şirket içi ağlar arasında yönlendirme sağlar.

- **VNet bağlantısı**: sanal ağlar bağlanması birbirine herhangi bir kaynak başka bir VNet ile iletişim kurmak için hiçbir sanal ağa bağlı kaynaklar etkinleştirme.

- **Şirket içi bağlantı**: sanal ağlara bağlanabilir ağınız ve Azure arasında özel ağ bağlantıları üzerinden veya siteden siteye VPN bağlantısı üzerinden şirket içi ağlar Internet üzerinden.

- **Trafik filtreleme**: rol örneklerini ağ trafiğini VM ve bulut Hizmetleri filtre gelen ve giden kaynak IP adresi ve bağlantı noktası, hedef IP adresi ve bağlantı noktası ve protokolü tarafından.

- **Yönlendirme**: isteğe bağlı olarak, kendi yolları yapılandırmak veya bir ağ geçidi üzerinden BGP yollarını kullanarak yönlendirme Azure'un varsayılan ayarlarını geçersiz kılabilir.

## <a name="network-access-controls"></a>Ağ erişim denetimleri

[Ağ güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) gibi bir güvenlik duvarının ve nasıl kaynak "iletişim kurabilirsiniz için" ağ üzerinden kurallar sağlar. Nasıl üzerinde ayrıntılı denetim sağladıkları / Internet veya diğer alt ağlara aynı sanal ağ için bir alt ağ (veya sanal makine) bağlanabilir.

Ağ güvenlik grubu (NSG), Azure Sanal Ağlara (VNet) bağlı kaynaklara ağ trafiğine izin veren veya reddeden güvenlik kurallarının listesini içerir. Ağ güvenlik grupları (NSG’ler), alt ağlarla, ayrı ayrı VM’lerle (klasik) veya VM’lere bağlı ağ arabirimleri ile ilişkilendirilebilir (Resource Manager).

Bir NSG bir alt ağ ile ilişkilendirildiğinde kurallar alt ağa bağlı tüm kaynaklar için geçerli olur. Bir NSG’nin bir VM veya ağ arabirimi ile ilişkilendirilmesi yoluyla da trafik kısıtlanabilir.

## <a name="security-and-continuous-compliance-with-organizational-standards"></a>Güvenlik ve kuruluş standartlarıyla sürekli uyumluluk

Her iş farklı gereksinimleri vardır ve her iş bulut çözümleri den farklı avantajları yararlanmasını. Yine de, her tür müşteriler buluta taşıma hakkında aynı temel endişeleriniz. Denetim verilerini korumak istedikleri ve tüm saydamlık ve uyumluluk korurken güvenli ve özel tutulması için bu verilerin istedikleri.

Müşterilere bulut sağlayıcılardan istediğinizi aşağıdaki gibidir:

- **Verilerimizi güvenli** veri güvenliği ve yönetim denetimini buluta sağlayabilir aktarımının artan sırada BT yöneticileri hala buluta geçiş bunları daha savunmasız saldırganlar, geçerli'den şirket içi bırakır, duyarlıdır çözümleri.

- **Verilerimizi korumak** özel bulut Hizmetleri işletmeler için benzersiz gizlilik zorlukları Yükselt. Şirketler altyapı giderlerinden tasarruf edersiniz ve kendi esnekliğinde geliştirmek için bulut Ara gibi bunlar Ayrıca, verilerini depolandıkları, kimin erişmekte olduğunu ve nasıl kullanıldıkları denetim kaybetme endişesi.

- **Bize denetime** daha fazla yenilikçi çözümlerini dağıtmak için bulut yararlanarak gibi şirket verilerini denetimini kaybetmesini hakkında çok endişe. Yasal ve çok yasal yollarla müşteri verilerine erişme kamu kurumlarının son açıklamalarına bazı CIO verilerini buluta depolamaya çok dikkatli olun.

- **Saydamlık Yükselt** güvenlik, gizlilik ve denetim için iş karar verenler önemli olsa da, aynı zamanda bağımsız olarak kendi verilerinin ne nasıl depolandığını, erişilen ve güvenli doğrulama olanağı istedikleri.

- **Uygunluğun sürdürülmesine** kullanımlarını bulut teknolojileri Şirketleri Genişlet gibi gelişmesi standartları ve düzenlemelere kapsamını ve karmaşıklık devam. Şirketler, uyumluluk standartlarını karşılaması gereken ve o uyumluluk düzenlemeleri değişiklik zaman içinde değiştikçe bilmeniz gerekir.

## <a name="security-configuration-monitoring-and-alerting"></a>Güvenlik Yapılandırması, izleme ve uyarma

Azure aboneleri yönetim iş istasyonları, geliştirici PC’leri ve hatta göreve özel izinleri bulunan ayrıcalıklı son kullanıcı cihazları dahil birden fazla cihazda kendi bulut ortamlarını yönetebilir. Bazı durumlarda, yönetim işlevleri Azure portalı gibi web tabanlı konsollar aracılığıyla gerçekleştirilir. Diğer durumlarda, Sanal Özel Ağlar (VPN), Terminal Hizmetleri, istemci uygulaması protokolleri ya da (programlı olarak) Azure Service Management API (SMAPI) üzerinden şirket için sistemlerden Azure’a bağlantılar olabilir. Ayrıca, istemci uç noktaları ya da etki alanına katılmış veya yalıtılmış ve yönetilmeyen olabilir, tabletler veya akıllı telefonlar gibi.

Çoklu erişim ve yönetim özellikleri zengin seçenekler sunsa da, bu değişkenlik bulut dağıtımına önemli bir risk ekleyebilir. Yönetim eylemlerini yönetmek, izlemek ve denetlemek güç olabilir. Bu değişkenlik bulut hizmetlerini yönetmek için kullanılan istemci uç noktalarına düzenlenmemiş erişim aracılığıyla güvenlik tehditlerine neden olabilir. Altyapı geliştirme ve yönetme amacıyla genel ya da kişisel iş istasyonlarını kullanmak, web’e gözatma (örneğin, su kaynağı saldırıları) ya da e-posta (örneğin, sosyal mühendislik ve kimlik avı) gibi öngörülemeyen tehdit vektörlerini açar.

İzleme, günlüğe kaydetme ve denetim yönetim etkinliklerini takip etme ve anlamaya ilişkin bir temel sunar ancak, oluşturulan veri miktarı nedeniyle tüm eylemlerin her ayrıntısının denetlenmesi uygulanabilir olmayabilir. Ancak, yönetim ilkelerinin verimliliğini denetlemek en iyi uygulamadır.

Azure güvenlik idare yöneticiler Windows denetlemek için AD DS GPO gelen, dosya paylaşımı gibi arabirimleri. Yönetim iş istasyonlarını denetim, izleme ve günlüğe kaydetme işlemlerine dahil edin. Tüm yönetici ve geliştirici erişimini ve kullanımını izleyin.

### <a name="azure-security-center"></a>Azure Güvenlik Merkezi

[Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro) Aboneliklerde kaynakların güvenlik durumu merkezi bir görünümünü sağlar ve tehlike giren kaynaklarını önlemeye yardımcı öneriler sağlar. Daha ayrıntılı ilkeleri (örneğin, uygulama ilkeleri kendi duruş ele alır riske uyarlamak Kurumsal izin belirli kaynak gruplarına) etkinleştirebilirsiniz.

![Azure Güvenlik Merkezi](./media/governance-in-azure/security-governance-in-azure-fig7.png)

Güvenlik Merkezi, Azure aboneliklerinize arasında tümleşik güvenlik izleme ve ilke yönetimi sağlar, kaçabilecek tehditleri ve güvenlik çözümlerinin geniş ekosistemiyle çalışır algılamaya yardımcı olur. Etkinleştirdikten sonra [güvenlik ilkeleri](https://docs.microsoft.com/azure/security-center/security-center-policies) bir aboneliğin kaynakları için Güvenlik Merkezi olası güvenlik açıklarını tanımlamak amacıyla kaynaklarınızın güvenliğini analiz eder. Ağ yapılandırmanız ile ilgili bilgiler hemen kullanımınıza sunulur.

Azure Güvenlik Merkezi, en iyi yöntem analiz ve Güvenlik İlkesi Yönetimi için Azure aboneliği içindeki tüm kaynakların bileşimini temsil eder. Bu güçlü ve kullanımı kolay araç güvenlik ekipleri ve risk engellemenize, algılamanıza ve otomatik olarak toplar ve Azure kaynaklarınızdan, ağdan ve kötü amaçlı yazılımdan gibi iş ortağı çözümlerinden güvenlik verilerini analiz eder gibi güvenlik tehditlerine karşı yanıt görevlileri sağlar programları ve güvenlik duvarları.

Ayrıca, Azure Güvenlik Merkezi Microsoft ürünleri ve Hizmetleri, Microsoft dijital Suçlar birimi (DCU), Microsoft küresel tehdit bilgilerinden yararlanarak sırasında makine öğrenme ve davranış analizi dahil olmak üzere gelişmiş analizler uygular Güvenlik Yanıt Merkezi (MSRC) ve dış akışların. [Güvenlik idare](https://www.credera.com/blog/credera-site/azure-governance-part-4-other-tools-in-the-toolbox/) kapsamlı abonelik düzeyinde uygulanan veya kaynakların ilke tanımı aracılığıyla uygulanan özel ve ayrıntılı gereksinimleri daraltabilir.

Son olarak, Azure Güvenlik Merkezi bu ilkelerine bağlı olarak kaynak güvenlik durumu analiz eder ve bu ayrıntılı panolar ve kötü amaçlı yazılım algılama veya kötü amaçlı bir IP bağlantısı gibi olaylar için deneme uyarı sağlamak için kullanır.

>[!Note]
> Önerileri uygulama hakkında daha fazla bilgi için [Azure Güvenlik Merkezi'nde güvenlik önerilerini uygulama](https://docs.microsoft.com/azure/security-center/security-center-recommendations)'yı okuyun.

Güvenlik Merkezi güvenlik durumlarına değerlendirmek, güvenlik önerileri sağlamak ve tehditlere karşı sizi uyarmak için sanal makinelerinizi veri toplar. Güvenlik Merkezi ilk eriştiğinizde, veri toplama, aboneliğinizin tüm sanal makinelerin etkinleştirilir. Veri toplama önerilir ancak, tarafından çevirin [veri toplamayı devre dışı bırakma](https://docs.microsoft.com/azure/security-center/security-center-faq) Güvenlik Merkezi ilkesinde.

Son olarak, Azure Güvenlik Merkezi Microsoft iş ortakları ve bağımsız yazılım satıcıları yeteneklerini geliştirmek için Azure Güvenlik Merkezi'nde takılan yazılım oluşturmasına olanak tanıyan bir açık platformudur.

Azure Güvenlik Merkezi aşağıdaki Azure kaynakları izler:

- Sanal makineler (VM'ler) (bulut Hizmetleri dahil)

- Azure Sanal Ağları

- Azure SQL Hizmeti

- İş ortağı çözümleri gibi bir web uygulaması güvenlik duvarı sanal makineleri ve üzerinde Azure aboneliğinizle tümleşik [uygulama hizmeti ortamı](https://docs.microsoft.com/azure/app-service/app-service-app-service-environments-readme).

### <a name="operations-management-suite"></a>Operations Management Suite

OMS yazılım geliştirme ve hizmet ekibin bilgi güvenliği ve [idare program](https://github.com/Microsoft/azure-docs/blob/master/articles/log-analytics/log-analytics-security.md) yasalarına ve düzenlemelerine konusunda açıklandığı gibi aynılarını ve kendi iş gereksinimlerini destekleyen [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/) ve [Microsoft Güven Merkezi Uyumluluk](https://www.microsoft.com/TrustCenter/Compliance/default.aspx). Nasıl OMS güvenlik gereksinimlerini belirlemek, güvenlik denetimleri tanımlar, yönetir ve riskleri izleyen de açıklanmaktadır vardır. Yıllık, biz gözden geçirme ilkeler, standartlar, yordamlar ve yönergeleri.

Her OMS geliştirme ekibi üyesi resmi uygulama güvenlik eğitimi alır. Dahili olarak, sürüm denetimi sistemi yazılım geliştirme için kullanırız. Her yazılım projesi sürüm denetimi sistemi tarafından korunur.

Microsoft, denetlediği ve tüm Microsoft hizmetlerinde değerlendirir güvenlik ve uyumluluk bir ekip sahiptir. Bilgi güvenliği görevlileri ekip olun ve OMS geliştirmek mühendislik Departmanlar ile ilişkili değildir. Güvenlik görevlileri kendi yönetim zinciri varsa ve güvenlik ve uyumluluk sağlamak üzere ürünleri ve Hizmetleri, bağımsız değerlendirmeleri yürütün.

Operations Management Suite (OMS olarak da bilinir), en başından itibaren bulutta tasarlanan bir yönetim hizmetleri koleksiyonudur. Dağıtma ve şirket içi kaynakları yönetmek yerine, OMS bileşenleri tamamen Azure'da barındırılır. Çok az yapılandırma gerektirir ve yalnızca birkaç dakikada kullanmaya başlayabilirsiniz.

![Operations Manager paketi](./media/governance-in-azure/security-governance-in-azure-fig8.png)

OMS hizmetlerinin bulutta çalışıyor olması, şirket içi ortamınızı etkili bir şekilde yönetemeyecekleri anlamına gelmez.

Bir aracı üzerinde herhangi bir Windows put veya Linux bilgisayarda veri merkezinizi ve burada Bulut veya şirket içi Hizmetleri toplanan tüm verileri birlikte analiz edilmeden günlük analizi veri gönderir. Azure Backup ve Azure Site Recovery bulut için yedekleme ve yüksek kullanılabilirlik içindeki kaynaklar yararlanmak için kullanın.

Buluttaki runbook'lar genellikle şirket içi kaynaklarınıza erişemez ancak veri merkezinizde runbook'ları barındıracak bir veya daha fazla bilgisayara bir aracı yükleyebilirsiniz. Bir runbook’u başlattığınızda yalnızca bulutta mı yoksa yerel bir çalışan olarak mı çalışacağını belirtirsiniz.

OMS’nin temel işlevleri Azure’da çalışan bir dizi hizmet tarafından sağlanır. Her hizmet belirli bir yönetim işlevi sağlar ve farklı hizmetleri birleştirerek farklı yönetim senaryoları elde edebilirsiniz.

![Operations Manager paketi](./media/governance-in-azure/security-governance-in-azure-fig9.JPG)

Azure işlem yöneticisi, kendi işlevler yönetim çözümleri sağlayarak genişletir. [Yönetim çözümleri](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-solutions) bir veya daha fazla OMS Hizmetleri yararlanarak Yönetimi senaryosu uygulayan paketlenmiş mantığı kümeleridir.

![Azure işlemi yönetme](./media/governance-in-azure/security-governance-in-azure-fig10.png)

Microsoft ve iş ortakları tarafından sağlanan ve OMS yatırımınızın değerini artırmak için Azure aboneliğinize kolayca ekleyebileceğiniz farklı çözümler vardır.

Bir iş ortağı olarak, uygulama ve hizmetlerinize desteklemek ve bunları Azure Marketi veya hızlı başlangıç şablonlarından aracılığıyla kullanıcılara sağlamak üzere kendi çözümleri oluşturabilirsiniz.

## <a name="performance-alerting-and-monitoring"></a>Performans uyarı verme ve izleme

### <a name="alerting"></a>Uyarı

Azure kaynak ölçümleri, olayları ve günlükleri izleme yöntemi uyarılar ve bir koşul belirttiğinizde bildirilmesini karşılanır.

**Farklı Azure Hizmetleri uyarıları**

Uyarılar dahil olmak üzere farklı Hizmetleri kullanılabilir:

- Application Insights: web testi ve ölçüm uyarılar sağlar.

>[!Note]
> Bkz: [Application Insights'ta uyarıları ayarlama](https://docs.microsoft.com/azure/application-insights/app-insights-alerts) ve [izlemek kullanılabilirlik ve yanıt herhangi bir Web sitesinin](https://docs.microsoft.com/azure/application-insights/app-insights-monitor-web-app-availability).

- Günlük analizi (Operations Management Suite): etkinlik ve günlük analizi için tanılama günlükleri yönlendirilmesini sağlar. Operations Management Suite, ölçüm, günlük ve diğer uyarı türleri sağlar.

>[!Note]
> Daha fazla bilgi için bkz [günlük analizi](https://docs.microsoft.com/azure/log-analytics/log-analytics-alerts).

- Azure İzleyici: ölçüm değerleri ve etkinlik günlüğü olaylarını göre uyarıları sağlar. Kullanabileceğiniz [Azure İzleyici REST API](https://msdn.microsoft.com/library/dn931943.aspx) Uyarıları yönetme.

>[!Note]
> Daha fazla bilgi için bkz: [uyarıları oluşturmak için Azure portal, PowerShell veya komut satırı arabirimini kullanarak](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-alerts-portal).

### <a name="monitoring"></a>İzleme

Performans sorunlarını bulut uygulamanızda iş etkileyebilir. Birden çok birbirine bağlı bileşenleri ve sık sürümleri ile degradations herhangi bir zamanda oluşabilir. Ve kullanıcılarınızın bir uygulama geliştiriyorsanız, genellikle testinde bulamadı sorunları keşfedin. Bu hemen bilmeniz ve sorunlarını tanılamak ve araçları olması gerekir. Microsoft Azure Araçları bu sorunları tanımlamak için bir aralığı yok.

**Azure bulut uygulamalarım nasıl izlerim?**

Çeşitli araçlar Azure uygulamaları ve Hizmetleri izleme yoktur. Bazı özelliklerine çakışıyor. Kısmen geçmiş nedenlerle ve geliştirme ve bir uygulamanın işlemi Bulanıklaştırma nedeniyle kısmen budur.

Asıl araçlar şunlardır:

- **Azure İzleyici** Azure üzerinde çalışan hizmetleri izlemek için temel araçtır. Bir hizmet ve çevresindeki ortamı üretimini ilgili altyapı düzeyinde verileri sağlar. Uygulamalarınızda tüm Azure yönetiyorsanız, yukarı veya aşağı kaynaklar, Ölçek karar sonra Azure İzleyicisi, başlatmak için kullandığınız sağlar.

- **Application Insights** geliştirme için ve bir üretim izleme çözümü olarak kullanılabilir. Uygulamanıza bir paketi yükleyerek çalışır ve bu nedenle neler olup bittiğini daha iç bir görünümünü sağlar. Verileri bağımlılıkları, özel durum izleme, anlık görüntüler, yürütme profilleri hata ayıklama yanıt sürelerini içerir. Tüm bu telemetrinin bir uygulama hata ayıklama yardımcı olması ve kullanıcıların ne ile yaptıklarını anlamanıza yardımcı olması için çözümlemek için güçlü akıllı araçlar sağlar. Bir ani artış yanıt süreleri nedeniyle bir şeyler bir uygulama veya dış bazı resourcing sorunu olup olmadığını belirtebilirsiniz. Bunu düzeltmek için Visual Studio kullanıyorsanız ve uygulama hataya neden olduğunu, kod sorunu satır sağ alınabilir.

- **Günlük analizi** performansı ayarlamak ve üretimde çalışan uygulamalara bakım planı için ihtiyacınız olanlar içindir. Azure'da temel alır. Toplar ve 10-15 dakikalık bir gecikme olsa birçok kaynaktan verileri toplar. Azure için bir bütünsel BT yönetim çözümü sunar şirket içi ve üçüncü taraf bulut tabanlı altyapı (örneğin, Amazon Web Hizmetleri). Daha fazla kaynakları genelinde verileri çözümlemek için daha zengin araçları tüm günlükleri karmaşık sorgular sağlar ve belirtilen koşullara proaktif olarak uyarabilir sağlar. Özel verileri sorgulamak ve onu görselleştirmek için kendi merkezi depoya bile toplayabilirsiniz.

- **System Center Operations Manager (SCOM)** yönetme ve büyük bulut yüklemeleri izleme içindir. Zaten bir yönetim aracı için Windows Server ve Hyper-V tabanlı Bulutlar şirket içi ancak aynı zamanda tümleştirileceğini ve Azure uygulamalarını yönetme gibi ile bilgi sahibi olmanız. Bunun yanı sıra, Application Insights mevcut Canlı uygulamaları yükleyebilirsiniz. Bir uygulama kullanılamaz hale gelirse, saniye cinsinden belirtir.


## <a name="next-steps"></a>Sonraki adımlar

- [En iyi uygulamalar Azure Resource Manager şablonları oluşturmak için](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices).

- [Azure aboneliği idare uygulamanın örnekleri](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-subscription-examples).

- [Microsoft Azure kamu](https://docs.microsoft.com/azure/azure-government/).
