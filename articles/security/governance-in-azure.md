---
title: Azure'da idare | Microsoft Docs
description: Uygulamanızı veya Kurumsal ihtiyaçlarını karşılamak üzere yukarı ve aşağı ölçeklenebilen bulut tabanlı bilgi işlem hizmetleri hakkında bilgi edinin.
services: security
documentationcenter: na
author: UnifyCloud
manager: mbaldwin
editor: TomSh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/01/2017
ms.author: TomSh
ms.openlocfilehash: 9c6509f25be7fe520a427e17ca1206e10f296fea
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37110771"
---
# <a name="governance-in-azure"></a>Azure’da idare

Azure, çok sayıda güvenlik seçenekleri ve böylece kuruluşunuzun dağıtımları benzersiz gereksinimlerini karşılamak bunları denetleme olanağı sağlar.

Azure bulut idare işlerinizdeki karar verme işlemlerini, ölçüt ve ilkeleri planlama söz konusu başvuruyor, mimarisi, edinme, dağıtım, işlem ve yönetimini bulut. Azure bulut idare bir tümleşik denetim ve gözden geçirmek ve Azure platformu kullanıcıların kullanımına kuruluşlar bildiren için danışmanlık yaklaşımı sağlar. 

Azure bulut idare yönelik bir plan oluşturmak için kişiler, işlemleri ve teknolojileri ilişkin ayrıntılı bir bakış şimdi gerçekleşmesi gerekir. Daha sonra kolaylaştırır çerçeveleri oluşturabilir tutarlı bir şekilde Azure özelliklerini kullanmak için kullanıcılar esnekliği sağlarken iş gereksinimlerini desteklemek için BT.

## <a name="implementation-of-policies-processes-and-standards"></a>Uygulama ilkeleri, işlemleri ve standartları 

Yönetim rolleri ve sorumlulukları Azure arasında bilgi güvenlik ilkeleri ve işletimsel sürekliliği uyarlamasını işleten oluşturmuştur. Azure yönetim, güvenlik ve sürekliliği yöntemler (üçüncü tarafların dahil) kendi ilgili ekipleri içinde gözlemledikten sorumludur. Ayrıca, güvenlik ilkeleri, işlemleri ve standartları ile uyumluluk da kolaylaştırır.

### <a name="account-provisioning"></a>Hesap sağlama

Hesap hiyerarşisi tanımlama kullanırsanız ve bir şirket içinde Azure Hizmetleri için önemli bir adımdır. Bu çekirdek idare yapısıdır. Bir Kurumsal Anlaşma (Kurumsal Sözleşme) sahip olan müşteriler Departmanlar, hesapları ve abonelikleri ortamına ayırabilir.

![Hesap sağlama](./media/governance-in-azure/security-governance-in-azure-fig1.png)

Bir Kurumsal Anlaşma yoksa kullanmayı [Azure etiketleri](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) hiyerarşisi tanımlamak için abonelik düzeyinde. Bir Azure aboneliği tüm kaynakları içeren temel birimidir. Ayrıca, Azure, çekirdek sayısı ve kaynakları gibi çeşitli sınırlarda tanımlar. Abonelikleri içerebilir [kaynak grupları](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview), kaynakları içerebilir. [Rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/en-us/azure/role-based-access-control/overview) ilkeler bu üç düzeyde uygulanır.

Her Kuruluş farklıdır. Kurumsal olmayan şirketler için Azure etiketleri kullanarak hiyerarşi Azure nasıl düzenlendiğini esneklik sağlar. Azure kaynaklarında dağıtmadan önce bir hiyerarşi model ve faturalama, kaynak erişimi ve karmaşıklık üzerindeki etkisini anlamak gerekir.

### <a name="subscription-controls"></a>Abonelik denetimleri

Abonelikler, kaynak kullanımı nasıl bildirilen ve fatura belirler. Abonelikler için ayrı faturalandırma ve ödeme ayarlayabilirsiniz. Bir Azure hesabı birden fazla abonelik olabilir. Abonelikler, bir şirkette birden çok Departmanlar Azure kaynak kullanımını belirlemek için kullanılabilir.

Örneğin, bir şirket olan BT, ik, ve pazarlama departmanları ve bu Departmanlar farklı projelere çalışıyor. Şirket, faturalama her bölümün kullanımı sanal makineler gibi Azure kaynakları hakkında temel alabilir. Şirket, ardından her bölüm Finans kontrol edebilirsiniz.

Azure abonelikleri üç parametre kurar:

- Benzersiz abone kimliği

- Faturalama konumu

- Kullanılabilir kaynak kümesi

Bir kullanıcı için bu parametreleri bir Microsoft hesabı kimliği, kredi kartı numarası ve Azure Hizmetleri tam dizisi içerir. Microsoft kullanım sınırları, abonelik türüne bağlı olarak zorlar.

Azure kayıt hiyerarşileri, hizmetleri bir Kurumsal Anlaşma içinde nasıl yapılandırıldığı tanımlayın. Kurumsal Anlaşma portal, bir kuruluşun gereksinimlerine göre özelleştirilebilir esnek Hiyerarşiler dayalı bir kurumsal anlaşma ile ilişkili Azure kaynaklarına erişimi bölmek müşteriler sağlar. Hiyerarşi deseni, bir kuruluşun yönetim ve ilişkili faturalama ve kaynak erişim için hesap için coğrafi yapısı eşleşmelidir.

İşlev, iş birimi üç üst düzey hiyerarşi desenleri alır ve coğrafi. Departmanlar hesap gruplandırmaları için yönetimsel bir yapı ' dir. Her bölüm içinde hesapları siloları fatura ve birkaç anahtar sınırları için (örneğin, sanal makineleri ve depolama hesapları süre) oluşturma abonelikleri atanabilir.

![Abonelik denetimleri](./media/governance-in-azure/security-governance-in-azure-fig2.png)


Bir Kurumsal Anlaşma olan kuruluşlar için Azure abonelikleri dört düzeyli bir hiyerarşiye izleyin:

1. Kuruluş kayıt Yöneticisi

2. Bölüm Yöneticisi

3. hesap sahibi

4. Hizmet yöneticisi

Bu hiyerarşide aşağıdaki yönetir:

- Faturalama ilişki.

- Hesap yönetimi.

- RBAC üzerinden kaynaklara erişimi.

- Sınırları:

  - Kullanım ve fatura (teklif sayılara göre oranı kartı)

  - Sınırlar

  - Sanal ağ

- Ek Azure Active Directory (Azure AD). Bir Azure AD örneğinde birçok aboneliği ile ilişkili olabilir.

- Bir kurumsal kayıt hesapla ilişki.

### <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi

[RBAC](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) Azure kaynaklarının ayrıntılı erişim yönetimi sağlar. RBAC kullanarak, kullanıcıların işlerini yapmak için gereksinim duyduğu sadece erişim miktarını verebilirsiniz. Şirketler, çalışanların ihtiyaç duydukları izinleri tam vermiş odaklanmanız gerekir. Çok fazla izinler saldırganlar bir hesaba kullanıma sunar. Çok az izinleri çalışanlar verimli bir şekilde işlerini alınamıyor anlamına gelir. 

Kısıtlanmamış izinlerini herkes vermek yerine, Azure aboneliği veya kaynakları yalnızca belirli eylemleri izin verebilirsiniz. Örneğin, bir çalışan başka bir çalışan SQL veritabanları aynı abonelikte yönetir sırada bir Abonelikteki sanal makineleri yönetme izin vermek için RBAC kullanabilirsiniz.

Erişim vermek için kullanıcılara, gruplara veya belirli bir kapsamda uygulamaları rolleri atayın. Rol atamasının kapsamı, bir abonelik, bir kaynak grubu veya tek bir kaynak olabilir. Bir üst kapsamda atanan bir rolü de içerdiği alt öğelerine erişim verir. Örneğin, bir kaynak grubu erişimi olan bir kullanıcı, Web siteleri, sanal makineler ve alt ağlar gibi içerdiği tüm kaynakları yönetebilir. İçinde her abonelik, en fazla 2000 rol atamalarını oluşturabilirsiniz.

Bir rolü izinleri koleksiyonudur ve RBAC birkaç yerleşik rol yok. Aşağıdaki yerleşik roller tüm kaynak türleri için geçerlidir:

- **Sahibi** temsilci başkalarına erişimi hakkı dahil olmak üzere tüm kaynaklara tam erişimi vardır.

- **Katkıda bulunan** olabilir oluşturun ve tüm türlerini Azure kaynaklarını yönetmek ancak başkalarına erişim izni veremiyor.

- **Okuyucu** tüm Azure kaynaklarına görüntüleyebilirsiniz.

Azure'da yerleşik rollerin geri kalanı belirli Azure kaynaklarının yönetimini sağlar. Örneğin, sanal makine katılımcı rolü oluşturmak ve sanal makineleri yönetmek kullanıcının sağlar. Yerleşik roller ve işlemlerini listesi için bkz: [RBAC yerleşik rolleri](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles).

RBAC, Azure portalı ve Azure Resource Manager API'leri, Azure kaynaklarının yönetim işlemlerini destekler. Çoğu durumda, Azure kaynakları için veri düzeyindeki işlemleri RBAC yetkilendirilemiyor. RBAC veri işlemleri genişletilen nasıl hakkında daha fazla bilgi için bkz: [rol tanımları anlayın](https://docs.microsoft.com/en-us/azure/role-based-access-control/role-definitions).

Yerleşik roller belirli erişim ihtiyaçlarınızı karşılamıyorsa, aşağıdakileri yapabilirsiniz [özel bir rol oluşturmak](https://docs.microsoft.com/azure/role-based-access-control/custom-roles). Yalnızca yerleşik roller gibi özel roller kullanıcılar, gruplar ve uygulamalar abonelik, kaynak grubu ve kaynak kapsam atanabilir. Kullanarak özel roller oluşturabilirsiniz [Azure PowerShell](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-powershell), [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli)ve [REST API](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-rest).

### <a name="resource-management"></a>Kaynak yönetimi

Azure iki dağıtım modeli sağlar: [Klasik](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model) ve Azure Kaynak Yöneticisi.

Klasik modelde bağımsız olarak her kaynak mevcut. İlgili kaynaklar gruplamak için bir yolu yoktur. Hangi kaynakların çözüm veya uygulama oluşturan ve eşgüdümlü bir yaklaşım yönetmeyi unutmayın el ile izlemek, vardır. Temel yönetim abonelik birimidir. Çok sayıda abonelikleri oluşturma için müşteri adayları bir abonelik içindeki kaynaklara ayırmanız zordur.

Kavramı, Resource Manager dağıtım modeli içeren bir [kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview). Kaynak grubu, ortak bir yaşam döngüsünü paylaşan kaynaklara yönelik bir kapsayıcıdır. Çözüm için tüm kaynakları veya yalnızca bir grup olarak yönetmek istediğiniz kaynakları içerebilir. Kuruluş için önemli olan faktörleri temel alarak kaynakları kaynak gruplarına nasıl ayıracağınıza siz karar verirsiniz.

Resource Manager dağıtım modeli çeşitli avantajlar sunar:

- Çözümünüze yönelik tüm hizmetleri ayrı ayrı ele almak yerine grup halinde dağıtabilir, yönetebilir ve izleyebilirsiniz.

- Art arda çözümünüzü yaşam döngüsü dağıtmak ve kaynaklarınızın tutarlı bir durumda dağıtılması da size güven verir.

- Kaynak grubunuzdaki tüm kaynaklara erişim denetimini uygulayabilirsiniz. Yeni kaynaklar kaynak grubuna eklendiğinde bu ilkeleri otomatik olarak uygulanır.

- Aboneliğinizdeki tüm kaynakları mantıksal olarak düzenlemek için kaynaklarınıza etiketler ekleyebilirsiniz.

- Çözümünüzün altyapısını tanımlamak için JavaScript Nesne Gösterimi (JSON) kullanabilirsiniz. JSON dosyası bir Resource Manager şablonu olarak bilinir.

- Doğru sırayla dağıtmış sağlamak için kaynaklarınız arasındaki bağımlılıkları tanımlayabilirsiniz.

![Resource Manager](./media/governance-in-azure/security-governance-in-azure-fig4.png)

Şablonlar hakkında öneriler için bkz. [Azure Resource Manager şablonları oluşturmaya yönelik en iyi uygulamalar](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices).

Azure Resource Manager kaynakların doğru sırada oluşturulmasını sağlamak için bağımlılıkları analiz eder. Bir kaynak (örneğin, bir sanal diskler için depolama hesabı gerek makine), başka bir kaynaktan bir değer kullanır varsa, [bir bağımlılık ayarlama](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-define-dependencies) şablondaki.

Ayrıca, şablonu altyapıda yapılan güncelleştirmeler için kullanabilirsiniz. Örneğin, çözümünüze bir kaynak ekleyebilir ve zaten dağıtılmış kaynaklar için yapılandırma kuralları ekleyebilirsiniz. Resource Manager şablonu kaynak zaten olan bir kaynak oluşturulmasını belirtiyorsa, yeni bir varlık oluşturmak yerine bir güncelleştirme gerçekleştirir. Yeni gibi Kaynak Yöneticisi'ni aynı duruma varlığı güncelleştirir.

Kurulumunda bulunmayan yazılım yükleme gibi daha fazla işlem gerektiğinde resource Manager senaryoları için uzantılar sağlar.

### <a name="resource-tracking"></a>Kaynak İzleme

Kuruluşunuzdaki kullanıcılar kaynakları aboneliğe eklemek gibi kaynakları uygun departmanı, müşteri ve ortam ile ilişkilendirmek daha önemli hale gelir. Meta veri kaynaklarına ekleyebilirsiniz [etiketleri](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags). Kaynak veya sahibi hakkında bilgi sağlamak için etiketler kullanın. Etiketler, yalnızca toplama ve kaynakları çeşitli şekillerde gruplamak, ancak geri ödeme amacıyla bu verilerin de olanak tanır.

Kaynak grupları, kaynaklar ve da, karmaşık bir koleksiyonu olduğunda etiketler kullanın. Bu varlıkları çoğu sizin için anlamlı bir şekilde görselleştirmeniz. Örneğin, kuruluşunuzda benzer bir rolü hizmet eden veya aynı departmana ait kaynakları etiketleyebilirsiniz.

Etiketleri, kuruluşunuzdaki kullanıcılar daha sonra tanımlamak ve yönetmek zor olabilir birden fazla kaynak oluşturabilirsiniz. Örneğin, bir proje için tüm kaynakları silmek isteyebilirsiniz. Bu kaynaklar için proje etiketlenir değil, bunları el ile bulmalıdır. Etiketleme, aboneliğinizden doğan gereksiz maliyetleri azaltmanın önemli bir yoludur.

Kaynakların bir etiketi paylaşması için aynı kaynak grubunda bulunmaları gerekmez. Kuruluşunuzdaki tüm kullanıcıların yanlışlıkla uygulanan biraz farklı etiketler (örneğin "departman" yerine"") yerine genel etiketler kullanmasını sağlamak için kendi etiket sınıflandırmanızı oluşturabilirsiniz.

Kaynak ilkeler, kuruluşunuz için standart kurallar oluşturmanıza olanak sağlar. Kaynakları uygun değerlerle etiketlenir emin olmak için ilkeleri oluşturabilirsiniz.

Azure portalından etiketli kaynakları da görüntüleyebilirsiniz. [Kullanım raporu](https://docs.microsoft.com/azure/billing/billing-understand-your-bill) aboneliğiniz etiket adları ve değerleri içerir için bu nedenle, bölün maliyetleri etiketlere göre.

Etiketler hakkında daha fazla bilgi için bkz: [faturalama İlkesi Initiative etiketleri](../azure-policy/scripts/billing-tags-policy-init.md).

Etiketler için aşağıdaki sınırlamalar geçerlidir:

- Her kaynak veya kaynak grubu en fazla 15 etiketi anahtar/değer çifti olabilir. Bu sınırlama yalnızca kaynak grubu veya kaynağa doğrudan uygulanan etiketler için geçerlidir. Bir kaynak grubu, her 15 etiketi anahtar/değer çiftleri sahip birçok kaynakları içerebilir.

- Etiket adı 512 karakterle sınırlıdır.

- Etiket değeri 256 karakterle sınırlıdır.

- Kaynak grubuna uygulanan etiketler, bu kaynak grubundaki kaynaklar tarafından devralınmaz.

Bir kaynak ile ilişkilendirmeniz gereken 15'ten fazla değer varsa, etiket değeri için JSON dizesi kullanın. JSON dizesindeki tek etiket anahtarına uygulanan birçok değerler içerebilir.

#### <a name="tags-for-billing"></a>Faturalama için etiketler

Etiketlerin fatura verilerinizi gruplandırmak etkinleştirin. Örneğin, farklı kuruluşlarda birden çok VM çalıştırıyorsanız, maliyet merkezi tarafından Grup kullanım etiketleri kullanın. Etiketler, maliyetleri üretim ortamında çalışan sanal makineler için fatura kullanımı gibi çalışma zamanı ortamı tarafından kategorilere ayırmak için de kullanabilirsiniz.

Etiketler hakkında bilgi alabilir [Azure kaynak kullanımı ve RateCard API'leri](https://docs.microsoft.com/azure/billing/billing-usage-rate-card-overview) veya kullanım virgülle ayrılmış değerler (CSV) dosyası. Kullanım dosyasını indirin [Azure hesap portalını](https://account.windowsazure.com/) veya [EA portal](https://ea.azure.com/).

Faturalandırma bilgileri programlı erişim hakkında daha fazla bilgi için bkz: [Microsoft Azure kaynak tüketimini Öngörüler elde](https://docs.microsoft.com/azure/billing/billing-usage-rate-card-overview). REST API işlemleri için bkz: [Azure faturalama REST API Başvurusu](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c).

Faturalama etiketleriyle destekleyen hizmetler için kullanım CSV yüklediğinizde, etiketler etiketleri sütununda görünür.

### <a name="critical-resource-controls"></a>Kritik kaynak denetimleri

Kuruluşunuz için abonelik Çekirdek Hizmetleri ekler gibi bu hizmetleri iş kesintisi yaşamamak kullanılabilir olduğundan emin olmak daha önemli hale gelir. [Kaynak kilitleri](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-lock-resources) değiştirilmesini veya silinmesini sahip olduğu uygulamalar veya Bulut altyapısı üzerinde önemli bir etkisi yüksek değerli kaynaklardaki işlemlerini kısıtlamak etkinleştirin. Kilitleri, bir abonelik, kaynak grubuna ya da kaynağa uygulayabilirsiniz. Genellikle, sanal ağlar, ağ geçitleri ve depolama hesapları gibi temel kaynaklarına kilitleri uygulayın.

Kaynak kilitleri şu anda iki değer destekler: **CanNotDelete** ve **salt okunur**. **CanNotDelete** kullanıcılarla (uygun hakları) hala anlamına gelir okumak veya bir kaynak değiştirme ancak bunu silemezsiniz. **Salt okunur** yetkili kullanıcıların anlamına gelir silinemez veya bir kaynak değiştirilemez.

Gönderilen işlemleri oluşan yönetim düzeyi gerçekleşen işlemleri için kaynak kilitleri Uygula <https://management.azure.com>. Kilitler nasıl kaynakları kendi işlevleri gerçekleştirmek kısıtlama yok. Kaynak kısıtlı değişir, ancak kaynak işlemlerinin sınırlı değildir. Örneğin, bir **ReadOnly** kilit bir SQL veritabanında silme veya veritabanı değiştirme engeller, ancak oluşturma, güncelleştirme ya da veritabanındaki verileri silme engellemez.

Uygulama **ReadOnly** gibi görünen bazı işlemler gerektirir ek eylemleri okuma olduğundan beklenmeyen sonuçlara yol açabilir. Örneğin, yerleştirme bir **ReadOnly** kilit bir depolama hesabında anahtarları listeleme tüm kullanıcıları engeller. Döndürülen anahtarları yazma işlemleri için kullanılabilir olmadığından anahtarları listeleme işlemi bir POST isteği gerçekleştirilir.

![Kritik kaynak denetimleri](./media/governance-in-azure/security-governance-in-azure-fig5.png)

Başka bir örneğin yerleştirme bir **ReadOnly** kilit bir Azure uygulama hizmeti kaynağına yazma erişimi bu etkileşimi gerektirmesi nedeniyle kaynak dosyalarını görüntüleme Visual Studio Sunucu Gezgini engeller.

Rol tabanlı erişim denetimi farklı olarak, tüm kullanıcılar ve roller bir kısıtlama uygulamak için yönetim kilitleri kullanın. İzinleri ayarlama bilgi edinmek için [RBAC ve Azure portalını kullanarak erişimini yönetme](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal).

Bir üst kapsamda kilit uyguladığınızda, kapsamı içindeki tüm kaynakların aynı kilit devralır. Daha sonra eklediğiniz bile kaynakları kilidi üst devralır. Devralmada en kısıtlayıcı kilidi önceliklidir.

Oluşturmak veya yönetim kilitleri silmek için Microsoft.Authorization/ veya Microsoft.Authorization/locks/ Eylemler erişiminiz olması gerekir. Yerleşik roller bu eylemleri yalnızca sahip ve kullanıcı erişimi Yöneticisi verilir.

### <a name="api-access-to-billing-information"></a>Faturalandırma bilgileri API erişimi

Azure faturalama API'leri kullanımı ve kaynak veri çekmek için tercih edilen veri analizi araçlarınızı kullanın. Azure kaynak kullanımı ve RateCard API'leri doğru şekilde tahmin etmek ve maliyetlerinizi yönetmenize yardımcı olabilir. API'ler bir kaynak sağlayıcısı ve Azure Resource Manager tarafından kullanıma sunulan API ailesinin bir parçası olarak uygulanır.

#### <a name="resource-usage-api-preview"></a>Kaynak kullanımı API (Önizleme)

Azure kullanmak [kaynak kullanım API'si](https://msdn.microsoft.com/library/azure/mt219003) tahmini Azure tüketim verilerinizi almak için. API içerir:

- **RBAC**: yapılandırma erişim ilkeleri [Azure portal](https://portal.azure.com/) aracılığıyla veya [Azure PowerShell cmdlet'lerini](https://docs.microsoft.com/powershell/azure/overview) hangi kullanıcıların veya uygulamaların aboneliğin kullanım verilerine erişim elde edebilirsiniz belirtmek için. Arayanlar, kimlik doğrulaması için standart Azure Active Directory belirteçleri kullanmanız gerekir. Çağıran, belirli bir Azure aboneliği kullanım verileri erişmek için faturalama Okuyucu, okuyucu, sahibi veya katkıda bulunan rolü ekleyin.

- **Günlük veya saatlik toplamalar**: arayanlar, günlük veya saatlik aralıklarla Azure kullanım verilerini isteyip istemediğinizi belirtebilirsiniz. Varsayılan günlük.

- **(Kaynak etiketleri içerir) örneği meta veri**: Get tam kaynak URI'si gibi örnek düzeyi ayrıntısı (/subscriptions/ {subscrıptıon-ID} /..), kaynak grubu bilgileri ve kaynak etiketleri. Bu meta veriler belirleyici biçimde ve program aracılığıyla etiketleriyle çapraz ücretlendirme gibi kullanım örnekleri için kullanım ayırmaya yardımcı olur.

- **Kaynak meta verilerini**: kaynak ayrıntılarını ölçüm adı, ölçüm kategori, ölçüm alt kategorisi, birim ve bölge gibi daha iyi ne tüketilen anlamak çağıran verin. Ayrıca Azure portalı kaynak meta verileri terminolojisi hizalamak için çalışıyoruz Azure kullanım CSV, faturalama CSV ve diğer genel kullanıma yönelik deneyimleri deneyimleri arasında verilerin bağıntısını yardımcı olacak EA.

- **Tüm kullanım teklif türleri**: kullanım verileri, tümü Kullandıkça Öde, MSDN, parasal taahhüt, kredi ve EA gibi türleri sunmak için kullanılabilir.

#### <a name="resource-ratecard-api"></a>Kaynak RateCard API

Mevcut Azure kaynakları ve her biri için tahmini fiyatlandırma bilgileri listesini almak için Azure kaynak RateCard API'sini kullanın. API içerir:

- **RBAC**: Azure portal veya hangi kullanıcıların veya uygulamaların RateCard verilere erişim elde edebilirsiniz belirtmek için Azure PowerShell cmdlet'leri aracılığıyla erişim ilkelerini yapılandırın. Arayanlar, kimlik doğrulaması için standart Azure Active Directory belirteçleri kullanmanız gerekir. Çağrıyı yapan belirli bir Azure aboneliği kullanım verileri erişmek için okuyucu, sahibi veya katkıda bulunan rolü ekleyin.

- **Kullandıkça Öde, MSDN, parasal taahhüt ve kredi sunar (ancak değil EA) için destek**: Bu API Azure teklifi düzeyi oranı bilgi sağlar. Bu API'yi çağıran kaynak ayrıntılarını ve ücretlerin almak için teklif bilgileri geçmesi gerekir. EA teklifleri kayıt göre oranları özelleştirdikten için EA şu anda desteklenmiyor. 

#### <a name="scenarios"></a>Senaryolar

Kullanım ve RateCard API'leri birleşimi bu senaryoları mümkün kılar:

- **Azure anlamak ay sırasında harcadığı**: kullanım birleşimini kullanın ve RateCard bulutunuzu daha iyi fikir almak için API harcamanızı ay. Saatlik ve günlük kullanımı ve Ücret tahminleri analiz edebilirsiniz.

- **Uyarıları ayarlamak**: RateCard API'leri ve kullanım tahmini bulut kullanımı ve ücretleri almak ve kaynak veya parasal tabanlı uyarıları ayarlamak için kullanın.

- **Fatura tahmin**: tahmini tüketim ve bulut harcamanız ve fatura fatura döneminin sonunda ne olacağını tahmin etmek için makine öğrenimi algoritmalarını uygulamak alın.

- **Analiz maliyetiyle öncesi tüketim gerçekleştirmek**: Azure'a iş yüklerinizi taşıdığınızda ne kadar faturanızı beklenen kullanımınız için olacaktır tahmin etmek için RateCard API'sini kullanın. Ayrıca var olan iş yükleri diğer Bulut veya özel bulutlara varsa, Azure ile kullanımınızı eşleştirebilirsiniz oranları Azure daha iyi kestirmek için harcadıkları. Bu tahmin teklifini Özet ve parasal taahhüt ve kredi gibi Kullandıkça Öde ötesinde farklı teklif türleri arasında karşılaştırma olanağı sağlar.

- **Bir çözümlemeleri gerçekleştirmek**: başka bir bölgede ya da başka bir Azure kaynak yapılandırması iş yüklerini çalıştırmak için daha uygun maliyetli olup olmadığını belirleyebilirsiniz. Azure kaynak maliyetleri kullanmakta olduğunuz Azure bölgesinde değişebilir. Başka bir Azure Teklif türü bir Azure kaynağı üzerinde daha iyi oranı sağlar, ayrıca belirleyebilirsiniz.

### <a name="networking-controls"></a>Ağ denetimleri

Kaynaklara erişimi (içinde corporation'ın ağ) iç veya dış (Internet) aracılığıyla olabilir. Yanlışlıkla kaynakları içinde yanlış yere koyun ve potansiyel olarak bunları kötü amaçlı erişime açmak, kuruluşunuzdaki kullanıcılar için kolay bir işlemdir. Şirket içi cihazları gibi kuruluşların Azure kullanıcıların doğru kararlar emin olmak için uygun denetimleri eklemeniz gerekir.

![Ağ denetimleri](./media/governance-in-azure/security-governance-in-azure-fig6.png)

Abonelik Yönetimi için aşağıdaki çekirdek kaynakları erişim temel bir denetim sağlar.

#### <a name="network-connectivity"></a>Ağ bağlantısı

[Sanal ağlar](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) alt ağlar için kapsayıcı nesnelerdir. Kesinlikle gerekli olsa, bir sanal ağ çoğunlukla uygulamaları iç şirket kaynaklarına bağlanmak için kullanılır. Azure Virtual Network service, güvenli bir şekilde Azure kaynakları ile sanal ağları birbirine olanak sağlar.

Bir sanal ağ kendi ağ bulutta gösterimidir. Azure bulutunun aboneliğinize adanmış mantıksal ayırma bir sanal ağdır. Ayrıca, sanal ağlar, şirket içi ağınıza bağlanabilir.

Azure sanal ağlar için özellikleri şunlardır:

- **Yalıtım**: sanal ağlar birbirinden yalıtılmış. Geliştirme, test ve üretim için aynı CIDR adres bloklarını kullanan ayrı sanal ağlar oluşturabilirsiniz. Buna karşılık, ağları birbirine bağlamak ve farklı CIDR adres bloklarını kullanan birden çok sanal ağlar oluşturabilir. Bir sanal ağ birden çok alt ağa bölebilirsiniz. Azure, sanal bir ağa bağlı sanal makineleri ve Azure Cloud Services rol örnekleri için dahili ad çözümlemesi sağlar. İsteğe bağlı olarak bir sanal ağ Azure dahili ad çözümlemesi kullanmak yerine kendi DNS sunucularını kullanmak için yapılandırabilirsiniz.

- **Internet bağlantısı**: tüm Azure sanal makineler ve sanal bir ağa bağlı bulut Hizmetleri rol örnekleri varsayılan olarak, internet erişimi vardır. Belirli kaynaklara gelen erişim gerektiğinde de etkinleştirebilirsiniz.

- **Azure kaynak bağlantısı**: Bulut Hizmetleri ve sanal makineleri, gibi Azure kaynakları aynı sanal ağa bağlanabilir. Farklı alt ağlarda bile olsa, kaynakları aracılığıyla özel IP adresleri, birbirlerine bağlanabilir. Azure, yapılandırmak ve yollar yönetmek zorunda kalmamak için varsayılan alt ağlar, sanal ağlar ve şirket içi ağlar arasında yönlendirme sağlar.

- **Sanal ağ bağlantısı**: Sanal ağları birbirine bağlanabilir. Herhangi bir sanal ağa bağlı kaynakları sonra herhangi bir sanal ağ üzerindeki herhangi bir kaynağa iletişim kurabilir.

- **Şirket içi bağlantı**: ağınız ve Azure arasında özel ağ bağlantıları üzerinden veya siteden siteye sanal özel ağ (VPN) bağlantısı üzerinden şirket içi ağlar için internet üzerinden sanal ağlara bağlanabilir.

- **Trafik filtreleme**: ağ trafiği (gelen ve giden) sanal makineler ve bulut Hizmetleri için kaynak IP adresi ve bağlantı noktası, hedef IP adresi ve bağlantı noktası ve protokolü göre filtreleyebilirsiniz.

- **Yönlendirme**: isteğe bağlı olarak, kendi yolları yapılandırmak veya bir ağ geçidi üzerinden BGP yollarını kullanarak Azure yönlendirme varsayılan ayarlarını geçersiz kılabilir.

#### <a name="network-access-controls"></a>Ağ erişim denetimleri

[Ağ güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) (Nsg'ler) olduğu gibi bir güvenlik duvarı ve nasıl kaynak "ağ üzerinden iletişim kurabilirsiniz için" kuralları sağlar. Bunlar, bir alt ağ (veya sanal makine) Internet veya diğer alt ağlara aynı sanal ağda nasıl bağlanabilir üzerinde denetim sağlar.

Ağ güvenlik grubu izin veren veya reddeden Azure sanal ağlar için bağlı kaynaklar için ağ trafiği güvenlik kurallarının bir listesini içerir. Nsg'ler alt ağlar, tek tek sanal makineleri (Klasik) veya VM'ler (Resource Manager) bağlı tek tek ağ arabirimlerine (NIC'ler) ile ilişkili olabilir.

Bir NSG'yi bir alt ağ ile ilişkili olduğunda, kurallar alt ağına bağlı tüm kaynaklara uygulanır. Bir NSG'yi bir VM veya NIC ile ilişkilendirme tarafından daha fazla trafiği kısıtlayabilir

## <a name="security-and-compliance-with-organizational-standards"></a>Güvenlik ve kuruluş standartlarıyla uyumluluğu

Her iş farklı gereksinimleri vardır ve bulut çözümleri den farklı avantajları yararlanmasını. Yine de, her tür müşteriler buluta taşıma hakkında aynı temel endişeleriniz. Müşterilere bulut sağlayıcılardan istediğinizi aşağıdaki gibidir:

- **Verilerimizi güvenli**: BT yöneticileri kabul bulut artan veri güvenliği ve yönetim denetimini sağlayabilirsiniz. Ancak buluta geçiş bunları daha bilgisayar korsanlarının geçerli şirket içi çözümleri daha savunmasız, hala ilgilenen.

- **Verilerimizi özel kalmasını**: Bulut Hizmetleri, benzersiz gizlilik zorluklar Yükselt. Şirketler altyapı giderlerinden tasarruf edersiniz ve kendi esnekliğinde geliştirmek için bulut Ara gibi bunlar Ayrıca, verilerini depolandıkları, kimin erişmekte olduğunu ve nasıl kullanıldıkları denetim kaybetme endişesi.

- **Bize denetime**: şirketler daha fazla yenilikçi çözümlerini dağıtmak için bulut yararlanarak gibi bunlar denetim verilerini kaybetmeden hakkında endişelisiniz. Yasal ve extralegal yollarla müşteri verilerine erişme kamu kurumlarının son açıklamalarına bazı CIO verilerini buluta depolamaya çok dikkatli olun.

- **Saydamlık Yükselt**: iş karar verenler güvenlik, gizlilik ve denetim önemini anlamanız. Ancak, bağımsız olarak kendi verilerinin ne nasıl depolandığını, erişilen ve güvenli doğrulama olanağı da istedikleri.

- **Uygunluğun sürdürülmesine**: şirketler kendi bulut teknolojileri kullanımını genişletme gibi gelişmesi standartları ve düzenlemelere kapsamını ve karmaşıklık devam. Şirketler, uyumluluk standartlarını karşılanması bilmeniz gerekir.

## <a name="security-configuration-for-monitoring-logging-and-auditing"></a>İzleme, günlüğe kaydetme ve denetim için güvenlik yapılandırması

Azure aboneleri birden çok cihazı bulut ortamlarını yönetebilir. Bu aygıtlar, yönetim iş istasyonları, geliştirici PC'leri ve görev özel izinlere sahip bile ayrıcalıklı son kullanıcı cihazları içerebilir. 

Bazı durumlarda, yönetim işlevleri Azure portalı gibi web tabanlı konsollar aracılığıyla gerçekleştirilir. Diğer durumlarda, olabilir doğrudan bağlantılar Azure için şirket içi sistemlerden VPN'ler, Terminal Hizmetleri, istemci uygulaması protokolleri veya Azure Hizmet Yönetimi API'si (SMAPI) (programlı olarak). Ayrıca, istemci uç noktaları ya da etki alanına katılmış veya yalıtılmış ve yönetilmeyen, tabletler veya akıllı telefonlar gibi olabilir.

Bu değişkenlik bulut dağıtımına önemli riski ekleyebilirsiniz. Yönetme, izleme ve yönetim eylemlerini denetlemek zor olabilir. Bu değişkenlik bulut hizmetlerini yönetmek için kullanılan istemci uç noktalarına düzenlenmemiş erişim aracılığıyla güvenlik tehditlerine de ortaya çıkarabilir. Altyapı geliştirme ve yönetme amacıyla genel ya da kişisel iş istasyonlarını kullanmak, web’e gözatma (örneğin, su kaynağı saldırıları) ya da e-posta (örneğin, sosyal mühendislik ve kimlik avı) gibi öngörülemeyen tehdit vektörlerini açar.

İzleme, günlüğe kaydetme ve Denetim izleme ve yönetim etkinlikleri anlamak için bir temel sağlar. Tüm eylemleri ayrıntısının denetimi her zaman oluşturulan veri miktarı nedeniyle uygun olmayabilir. Ancak, yönetim ilkelerinin verimliliğini denetlemek en iyi bir uygulamadır.

Azure güvenlik idare Azure Active Directory etki alanı Hizmetleri (AD DS) GPO'ları gelen, dosya paylaşımı gibi yöneticiler Windows arabirimleri denetlemenize yardımcı olabilir. Yönetim iş istasyonları, izleme, günlüğe kaydetme ve işlemleriniz içerir. Tüm yönetici ve geliştirici erişimini ve kullanımını izleyin.

### <a name="azure-security-center"></a>Azure Güvenlik Merkezi

[Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro) abonelikleri kaynakların güvenlik durumu merkezi bir görünümünü sağlar. Ele geçirilen kaynakları önlemeye yardımcı önerileri sağlar. Kendi duruş adresleme risk uyarlamak Kurumsal izin belirli kaynak gruplarına ilkeleri uygulama ayrıntılı ilkeleri--Örneğin, etkinleştirebilirsiniz.

![Azure Güvenlik Merkezi](./media/governance-in-azure/security-governance-in-azure-fig7.png)

Güvenlik Merkezi, Azure aboneliklerinize arasında tümleşik güvenlik izleme ve ilke yönetimi sağlar, kaçabilecek tehditleri ve güvenlik çözümlerinin geniş ekosistemiyle çalışır algılamaya yardımcı olur. Etkinleştirdikten sonra [güvenlik ilkeleri](https://docs.microsoft.com/azure/security-center/security-center-policies) bir aboneliğin kaynakları için Güvenlik Merkezi olası güvenlik açıklarını tanımlamak amacıyla kaynaklarınızın güvenliğini analiz eder. Ağ yapılandırmanız ile ilgili bilgiler hemen kullanımınıza sunulur.

Azure Güvenlik Merkezi, en iyi yöntem analiz ve Güvenlik İlkesi Yönetimi için Azure aboneliği içindeki tüm kaynakların bileşimini temsil eder. Güvenlik ekipleri ve engellemenize, algılamanıza ve otomatik olarak toplar ve Azure kaynaklarınızdan, ağdan ve kötü amaçlı yazılımdan koruma programları ve güvenlik duvarları gibi iş ortağı çözümlerinden güvenlik verilerini analiz eder gibi güvenlik tehditlerine karşı yanıt için risk görevlileri sağlar.

Ayrıca, Azure Güvenlik Merkezi Makine öğrenme ve davranış analizi dahil olmak üzere gelişmiş analizler uygular. Dış akışları ve küresel tehdit bilgilerinden Microsoft ürünleri ve Hizmetleri, Microsoft dijital Suçlar birimi (DCU), Microsoft Güvenlik Yanıt Merkezi (MSRC) kullanır. Uygulayabileceğiniz [güvenlik idare](https://www.credera.com/blog/credera-site/azure-governance-part-4-other-tools-in-the-toolbox/) abonelik düzeyinde kapsamlı. Veya belirli gereksinimleri aşağıya doğru daraltmak ve bunları tek tek ilke tanımı kaynaklarına uygulayabilirsiniz.

Son olarak, Azure Güvenlik Merkezi bu ilkelerine bağlı olarak kaynak güvenlik durumu analiz eder ve ayrıntılı panolar ve kötü amaçlı yazılım algılama veya kötü amaçlı bir IP bağlantısı gibi olaylar için deneme uyarı sağlamak için bu bilgileri kullanır. Önerileri uygulama hakkında daha fazla bilgi için bkz: [Azure Güvenlik Merkezi'nde güvenlik önerilerini uygulama](https://docs.microsoft.com/azure/security-center/security-center-recommendations).

Azure Güvenlik Merkezi aşağıdaki Azure kaynakları izler:

- Sanal makineler (VM'ler) (bulut Hizmetleri dahil)

- Sanal ağlar

- SQL veritabanları

- İş ortağı çözümleri gibi bir web uygulaması güvenlik duvarı sanal makineleri ve üzerinde Azure aboneliğiniz ile tümleşik [uygulama hizmeti ortamı](https://docs.microsoft.com/azure/app-service/app-service-app-service-environments-readme)

Güvenlik Merkezi ilk eriştiğinizde, veri toplama, aboneliğinizin tüm sanal makinelerin etkinleştirilir. Veri toplama etkin tutmak ancak yapabilecekleriniz öneririz [devre dışı](https://docs.microsoft.com/azure/security-center/security-center-faq) Güvenlik Merkezi ilkesinde.

### <a name="log-analytics"></a>Log Analytics

Azure günlük analizi yazılım geliştirme ve hizmet ekibin bilgi güvenliği ve [idare program](https://github.com/Microsoft/azure-docs/blob/master/articles/log-analytics/log-analytics-security.md) kendi iş gereksinimlerini destekler. Bunu yasalarına ve düzenlemelerine konusunda açıklandığı gibi aynılarını [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/) ve [Microsoft Güven Merkezi Uyumluluk](https://www.microsoft.com/TrustCenter/Compliance/default.aspx). Günlük analizi, güvenlik gereksinimleri nasıl kurar güvenlik denetimleri tanımlar ve yönetir ve izleyiciler riskleri de açıklanmıştır vardır. Yıllık, takım ilkeler, standartlar, yordamlar ve yönergeleri gözden geçirir.

Her günlük analizi geliştirme ekibi üyesi resmi uygulama güvenlik eğitimi alır. Sürüm denetimi sistemi geliştirme her yazılım projesinde korunmasına yardımcı olur.

Microsoft, denetlediği ve tüm Microsoft hizmetlerinde değerlendirir güvenlik ve uyumluluk bir ekip sahiptir. Ekip Kur bilgi güvenlik görevlileri yapın ve bunlar günlük analizi geliştirmek mühendislik Departmanlar ile ilişkili değil. Güvenlik görevlileri kendi yönetim zinciri var. Bunlar, güvenlik ve uyumluluk sağlamak üzere ürünleri ve Hizmetleri, bağımsız değerlendirmeleri yürütün.

Günlük analizi çekirdek işlevselliğini Azure üzerinde çalışan hizmetleri kümesi tarafından sağlanır. Her hizmet belirli bir yönetim işlevi sağlar ve farklı hizmetleri birleştirerek farklı yönetim senaryoları elde edebilirsiniz.

![Yönetim için Azure Hizmetleri](./media/governance-in-azure/security-governance-in-azure-fig9.JPG)

Bu Yönetim Hizmetleri bulutta tasarlanmıştır. Dağıtma ve şirket içi kaynakları yönetme içermeyen şekilde tamamen Azure üzerinde barındırılan. En az bir yapılandırmadır ve birkaç dakika içinde açık ve çalışıyor olması.

Bir aracı herhangi bir Windows veya Linux bilgisayarda merkezinizdeki koyabilir ve günlük analizi veri gönderir. Burada, Bulut veya şirket içi Hizmetleri'nden toplanan tüm verileri birlikte çözümlenebilir. Şirket içi kaynakları için yedekleme ve yüksek kullanılabilirlik için bulut yararlanmak için Azure Backup ve Azure Site Recovery kullanın.

![Yönetim Hizmetleri Azure panosunda](./media/governance-in-azure/security-governance-in-azure-fig8.png)

Runbook'ları bulutta genellikle, şirket içi kaynaklara erişemez, ancak runbook'ları, veri merkezinizdeki barındıracak bir veya daha çok bilgisayarda bir aracı yükleyebilirsiniz. Bir runbook'u başlattığınızda, bulutta veya yerel bir çalışan üzerinde çalışmasını isteyip istemediğinizi belirtin.

Farklı çözümler, Microsoft'un ve iş ortakları, günlük analizi yatırımınızı değerini artırmak için Azure aboneliğiniz ekleyebilirsiniz kullanılabilir. Örneğin, Azure'un sunduğu [yönetim çözümleri](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-solutions)--Yönetimi senaryosu bir veya daha fazla Yönetim Hizmetleri kullanarak uygulama mantığını kümesi paketlenmiş.

![Azure yönetim çözümlerine Galerisi](./media/governance-in-azure/security-governance-in-azure-fig10.png)

Bir iş ortağı olarak, uygulama ve hizmetlerinize desteklemek ve bunları Azure Marketi veya hızlı başlangıç şablonlarını aracılığıyla kullanıcılara sağlamak üzere kendi çözümleri oluşturabilirsiniz.

## <a name="performance-alerting-and-monitoring"></a>Performans uyarı verme ve izleme

### <a name="alerting"></a>Uyarı

Uyarıları izleme Azure kaynak ölçümleri, olayları veya günlükleri bir yöntemdir. Belirttiğiniz bir koşul karşılandığında bunlar size bildirir.

Uyarılar dahil olmak üzere Hizmetleri kullanılabilir:

- **Azure Application Insights**: web test ve ölçüm Uyarıları etkinleştirir. Daha fazla bilgi için bkz: [Application Insights'ta uyarıları ayarlama](https://docs.microsoft.com/azure/application-insights/app-insights-alerts) ve [izlemek kullanılabilirlik ve yanıt herhangi bir Web sitesinin](https://docs.microsoft.com/azure/application-insights/app-insights-monitor-web-app-availability).

- **Günlük analizi**: etkinlik ve günlük analizi için tanılama günlükleri yönlendirilmesini sağlar. Bu ölçüm, günlük ve diğer uyarı türleri sağlar. Daha fazla bilgi için bkz: [günlük analizi uyarılarını](https://docs.microsoft.com/azure/log-analytics/log-analytics-alerts).

- **Azure İzleyici**: ölçüm değerleri ve etkinlik günlüğü olaylarını göre uyarıları sağlar. Kullanabileceğiniz [Azure İzleyici REST API](https://msdn.microsoft.com/library/dn931943.aspx) Uyarıları yönetme. Daha fazla bilgi için bkz: [uyarıları oluşturmak için Azure portal, PowerShell veya komut satırı arabirimini kullanarak](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-alerts-portal).

### <a name="monitoring"></a>İzleme

Performans sorunlarını bulut uygulamanızda iş etkileyebilir. Birden çok birbirine bağlı bileşenleri ve sık sürümleri ile degradations herhangi bir zamanda oluşabilir. Ve kullanıcılarınızın bir uygulama geliştiriyorsanız, genellikle testinde bulamadı sorunları keşfedin. Bu sorunlar hakkında hemen bilmeniz ve tanılama ve bunları düzeltmek için Araçlar olması gerekir.

Çeşitli araçlar Azure uygulamaları ve Hizmetleri izleme yoktur. Bazı özelliklerine çakışıyor. Kısmen geliştirme ve bir uygulamanın işlemi Bulanıklaştırma nedeni budur.

Asıl araçlar şunlardır:

- **Azure İzleyici** Azure üzerinde çalışan hizmetleri izlemek için temel bir araçtır. Bir hizmet ve çevresindeki ortamı üretimini ilgili altyapı düzeyinde verileri sağlar. Tüm uygulamalarınızda Azure ve yukarı veya aşağı kaynakları ölçeklendirmek karar verme yönetiyorsanız, Azure İzleyici başlamanıza yardımcı olabilir.

- **Application Insights** geliştirme için ve bir üretim izleme çözümü olarak kullanılabilir. Bu paket, uygulamanızda neler olup bittiğini daha iç bir görünümünü sağlar, böylece yükleyerek çalışır. Verileri bağımlılıkları, özel durum izleme, anlık görüntüler ve yürütme profilleri hata ayıklama yanıt sürelerini içerir. Tüm bu telemetrinin bir uygulama hata ayıklama yardımcı olması ve kullanıcıların ne ile yaptıklarını anlamanıza yardımcı olması için çözümlemek için araçlar sağlar. Bir ani artış yanıt süreleri nedeniyle bir şeyler bir uygulama veya dış bazı resourcing sorunu olup anlayabilirsiniz. Kullanırsanız, Visual Studio ve uygulama hatalı, onu giderebilmemiz kod sorunu satıra sağ gidin.

- **Günlük analizi** performansı ayarlamak ve üretimde çalışan uygulamalara bakım planı için ihtiyacınız olanlar içindir. Toplar ve 10-15 dakikalık bir gecikmeyle pek çok kaynaktan veri toplar. Azure için bir bütünsel BT yönetim çözümü sunar şirket içi ve üçüncü taraf bulut tabanlı altyapı (örneğin, Amazon Web Hizmetleri). Veri kaynakları arasında çözümlemek için Araçlar tüm günlükleri karmaşık sorgular sağlar ve belirtilen koşullara proaktif olarak uyarabilir sağlar. Bile, merkezi bir depoya özel verileri toplamak ve ardından sorgu ve bu verileri görselleştirin.

- **System Center Operations Manager** yönetme ve büyük bulut yüklemeleri izleme içindir. Zaten kendisiyle tanıdık şirket içi Windows Server için bir yönetim aracı olarak olabilir ve Hyper-V bulut tabanlı, ancak ayrıca tümleştirileceğini ve Azure uygulamalarını yönetme. Bunun yanı sıra, Application Insights mevcut Canlı uygulamaları yükleyebilirsiniz. Bir uygulamayı devre dışı kalırsa, Operations Manager saniye cinsinden belirtir.


## <a name="next-steps"></a>Sonraki adımlar

- [Azure Resource Manager şablonları oluşturmaya yönelik en iyi uygulamalar](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices)

- [Azure aboneliği idare uygulamanın örnekleri](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-subscription-examples)

- [Microsoft Azure kamu](https://docs.microsoft.com/azure/azure-government/)
