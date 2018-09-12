---
title: Azure'da idare | Microsoft Docs
description: Uygulamanızı veya Kurumsal ihtiyaçlarını karşılamak üzere yukarı ve aşağı ölçeklendirilebilir bulut tabanlı bilgi işlem hizmetleri hakkında bilgi edinin.
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
ms.openlocfilehash: 6e5b6fac25c8c7f76991a58fbcab363c6fc20f12
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2018
ms.locfileid: "44380361"
---
# <a name="governance-in-azure"></a>Azure’da idare

Azure, birçok güvenlik seçenekleri ve böylece kuruluşunuzun dağıtımları benzersiz gereksinimlerini karşılamak bunlar üzerinde denetim olanağı sunar.

Karar verme süreçlerinde, ölçütleri ve ilkeleri planlama dahil Azure bulut idare başvurduğu, mimarisi, alım, dağıtım, işlemi ve yönetimi, bulut bilgi işlem. Azure bulut idare bir tümleşik denetim ve gözden geçirme ve bunların kullanımını üzerindeki kuruluşlar Azure platformunun bildiren danışmanlık yaklaşım sağlar. 

Azure bulut yönetimi için bir plan oluşturmak için kişileri, süreçleri ve teknolojileri ayrıntılı bakışın artık gerçekleşmesi gerekir. Ardından kolaylaştırır çerçeveleri oluşturabilirsiniz BT'nin tutarlı bir şekilde Azure özelliklerini kullanmak için kullanıcıların esnekliği sağlarken iş gereksinimlerini destekleyin.

## <a name="implementation-of-policies-processes-and-standards"></a>Uygulama ilkeleri, süreçleri ve standartları 

Yönetim rolleri ve sorumlulukları, bilgi güvenlik ilkeleri ve operasyonel devamlılığı uygulaması Azure genelinde denetleyecek oluşturmuştur. Azure yönetimi için güvenlik ve sürekliliği uygulamaları (üçüncü taraflar dahil) kendi ilgili takımlar içinde gözlemledikten sorumludur. Ayrıca, güvenlik ilkeleri, süreçleri ve standartları ile uyum kolaylaştırır.

### <a name="account-provisioning"></a>Hesap sağlama

Hesap hiyerarşi tanımlama kullanırsanız ve bir şirket içinde Azure Hizmetleri için önemli bir adımdır. Bu çekirdek idare yapısıdır. Sahip bir Kurumsal Anlaşma (EA) müşterileri, Departmanlar, hesaplar ve abonelikler ortamına alt bölümlere ayırmak.

![Hesap sağlama](./media/governance-in-azure/security-governance-in-azure-fig1.png)

Kurumsal Sözleşme yoksa kullanmayı [Azure etiketleri](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) hiyerarşi tanımlamak için abonelik düzeyinde. Azure aboneliğiniz tüm kaynakları içeren temel bir birimdir. Ayrıca, Azure, çekirdek sayısı ve kaynakları gibi çeşitli sınırlarda tanımlar. Abonelikleri içerebilir [kaynak grupları](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview), kaynakları içerebilir. [Rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview) bu üç düzeyde ilkeler geçerlidir.

Her Kurumsal farklıdır. Kurumsal olmayan şirketler için Azure etiketleri kullanarak hiyerarşi Azure nasıl düzenlendiğini esneklik sağlar. Azure'daki kaynakları dağıtmadan önce bir hiyerarşi modeli ve faturalama, kaynak erişimi ve karmaşıklık üzerindeki etkisini anlamak gerekir.

### <a name="subscription-controls"></a>Abonelik denetimleri

Nasıl kaynak kullanımı bildirilen faturalandırılır ve abonelikleri belirleyin. Abonelikler için ayrı faturalandırma ve ödeme ayarlayabilirsiniz. Bir Azure hesabı, birden fazla aboneliğiniz olabilir. Abonelikler, birden çok şirket departmanları Azure kaynak kullanımını belirlemek için kullanılabilir.

Örneğin, bir şirket olan ik, BT ve pazarlama departmanları ve bu bölümlerin farklı projelerde çalışıyor. Şirket, sanal makineler gibi Azure kaynaklarının kullanımını her departmanın, faturalandırmayla temel alabilir. Şirket, Finans departmanı her sonra kontrol edebilirsiniz.

Azure Abonelikleri, üç parametre kurar:

- Benzersiz abone kimliği

- Faturalandırma konumu

- Kullanılabilir kaynak kümesi

Bir kişi için bu parametreleri bir Microsoft hesabı kimliği, bir kredi kartı numarası ve Azure hizmetlerini içeren tam paketini içerir. Microsoft kullanım sınırları, abonelik türüne bağlı olarak zorlar.

Azure kayıt hiyerarşileri, Hizmetleri içinde bir Kurumsal Anlaşma nasıl yapılandırılmıştır tanımlayın. Kurumsal Anlaşma portalı, bir kuruluşun gereksinimleri için özelleştirilebilen esnek Hiyerarşiler dayalı bir kurumsal anlaşma ile ilişkili Azure kaynaklarına erişimini bölmek müşterilerin sağlar. Hiyerarşi desen, bir kuruluşun yönetim ve için ilişkili faturalandırma ve kaynak erişim hesabı için coğrafi yapısı eşleşmelidir.

İşlev, iş birimi, üç üst düzey hiyerarşi desenleri olan ve coğrafi. Departmanlar hesabı gruplandırmaları için yönetimsel bir yapısı var. Her bölüm içinde siloları faturalama ve birkaç anahtar sınırlar oluşturacak (örneğin, VM'ler ve depolama hesapları süre) Abonelikleri, hesapları atanabilir.

![Abonelik denetimleri](./media/governance-in-azure/security-governance-in-azure-fig2.png)


Bir kurumsal anlaşma kapsamında olan kuruluşlar için Azure abonelikleri dört düzeyli bir hiyerarşi izleyin:

1. Kuruluş kayıt Yöneticisi

2. Bölüm yöneticisi

3. Hesap sahibi

4. Hizmet yöneticisi

Bu hiyerarşi aşağıdaki yönetir:

- Faturalandırma ilişki.

- Hesap yönetimi.

- RBAC üzerinden erişim.

- Sınırları:

  - Kullanım ve faturalandırma (teklif rakamlara dayanan oranı kartı)

  - Sınırlar

  - Sanal ağ

- Ek Azure Active Directory (Azure AD). Bir Azure AD örneği birçok aboneliği ile ilişkili olabilir.

- Bir kurumsal kayıt hesabı ile ilişkilendirme.

### <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi

[RBAC](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) Azure kaynaklarının ayrıntılı erişim yönetimi sağlar. RBAC kullanarak, kullanıcıların işlerini yapması için gereken sadece erişim miktarını verebilirsiniz. Şirket, çalışanlar gereksinim duydukları tam izinleri vererek üzerinde odaklanmanız gerekir. Çok fazla İznim saldırganların bir hesap kullanıma sunar. Çok az izinleri çalışanlar işlerini verimli bir şekilde alınamıyor anlamına gelir. 

Herkes vermek yerine Kısıtlanmamış izinler, Azure aboneliği veya kaynakları yalnızca belirli eylemleri izin verebilirsiniz. Örneğin, bir çalışan aynı Abonelikteki SQL veritabanlarını başka bir çalışanın yönetirken siz bir Abonelikteki sanal makineleri yönetmenize izin vermek için RBAC kullanabilirsiniz.

Erişim vermek için kullanıcıları, grupları veya belirli bir kapsamda uygulamalar için roller atayın. Rol atamasının kapsamı, bir abonelik, kaynak grubu veya tek bir kaynak olabilir. Atanmış bir üst kapsamda bir rol, ayrıca içerdiği alt öğeleri için erişim verir. Örneğin, bir kaynak grubu erişimi olan bir kullanıcı, Web siteleri, sanal makineler ve alt ağları gibi içerdiği tüm kaynakları yönetebilir. Her abonelikte en fazla 2000 rol ataması oluşturabilirsiniz.

Rol izinleri koleksiyonudur ve RBAC birkaç yerleşik rol vardır. Aşağıdaki yerleşik roller, tüm kaynak türleri için geçerlidir:

- **Sahibi** tüm kaynaklara temsilci erişimi başkalarına hakkı dahil olmak üzere, tam erişimi vardır.

- **Katkıda bulunan** olabilir oluşturun ve tüm Azure kaynakları türlerini yönetmek ancak diğerleri için erişim izni veremiyor.

- **Okuyucu** tüm Azure kaynaklarını görebilirsiniz.

Azure'da yerleşik rollerin geri kalanı, belirli bir Azure kaynak yönetimi sağlar. Örneğin, sanal makine Katılımcısı rolü oluşturmak ve sanal makineleri yönetmek kullanıcı sağlar. Tüm yerleşik roller ve işlemlerini listesi için bkz: [RBAC yerleşik rolleri](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles).

RBAC, Azure portalı ve Azure Resource Manager API'leri Azure kaynaklarının yönetim işlemlerini destekler. Çoğu durumda, RBAC, Azure kaynakları için veri düzeyinde işlemlerine yetkisi kaldırılamıyor. RBAC veri işlemleri nasıl Genişletilmekte olan hakkında daha fazla bilgi için bkz: [rol tanımlarını anlamak](https://docs.microsoft.com/azure/role-based-access-control/role-definitions).

Yerleşik roller, belirli erişim gereksinimlerinizi karşılamıyorsa, aşağıdakileri yapabilirsiniz [özel bir rol oluşturun](https://docs.microsoft.com/azure/role-based-access-control/custom-roles). Yalnızca yerleşik roller gibi kullanıcılara, gruplara ve uygulamalara abonelik, kaynak grubu ve kaynak kapsamı için özel roller atanabilir. Kullanarak özel roller oluşturabilirsiniz [Azure PowerShell](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-powershell), [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli)ve [REST API](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-rest).

### <a name="resource-management"></a>Kaynak yönetimi

Azure, iki dağıtım modeli sağlar: [Klasik](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model) ve Azure Kaynak Yöneticisi.

Klasik modeldeki her bir kaynağın bağımsız olarak mevcut. İlgili kaynakları bir araya gruplamak için hiçbir yolu yoktur. Hangi kaynakların, çözüm veya uygulama oluşturan ve eşgüdümlü bir yaklaşım yönetilecek unutmayın el ile izlemek, sahip. Temel yönetim birimidir aboneliktir. Çok sayıda abonelikleri oluşturulmasını müşteri adayları bir Abonelikteki kaynakları ayırmanız zordur.

Resource Manager dağıtım modeli kavramını içerir bir [kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview). Kaynak grubu, ortak bir yaşam döngüsünü paylaşan kaynaklara yönelik bir kapsayıcıdır. Çözümün tüm kaynaklarını veya yalnızca bir grup olarak yönetmek istediğiniz kaynakları içerebilir. Kuruluş için önemli olan faktörleri temel alarak kaynakları kaynak gruplarına nasıl ayıracağınıza siz karar verirsiniz.

Resource Manager dağıtım modeli çeşitli avantajlar sunar:

- Çözümünüze yönelik tüm hizmetleri ayrı ayrı ele almak yerine grup halinde dağıtabilir, yönetebilir ve izleyebilirsiniz.

- Art arda çözümünüzü yaşam döngüsü boyunca dağıtabilir ve kaynaklarınızın tutarlı bir durumda dağıtıldığından emin olabilirsiniz.

- Erişim denetimini kaynak grubunuzdaki tüm kaynaklara uygulayabilirsiniz. Bu ilkeler, yeni kaynaklar kaynak grubuna eklendiğinde otomatik olarak uygulanır.

- Aboneliğinizdeki tüm kaynakları mantıksal olarak düzenlemek için kaynaklarınıza etiketler ekleyebilirsiniz.

- Çözümünüzün altyapısını tanımlamak için JavaScript Nesne Gösterimi (JSON) kullanabilirsiniz. JSON dosyası bir Resource Manager şablonu olarak bilinir.

- Doğru sırayla dağıtıldıkları için kaynaklarınız arasındaki bağımlılıkları tanımlayabilirsiniz.

![Resource Manager](./media/governance-in-azure/security-governance-in-azure-fig4.png)

Şablonlar hakkında öneriler için bkz. [Azure Resource Manager şablonları oluşturmaya yönelik en iyi uygulamalar](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices).

Azure Resource Manager kaynakların doğru sırada oluşturulmasını emin olmak için bağımlılıkları analiz eder. Bir kaynak (bir sanal diskler için depolama hesabına ihtiyaç duyan makine gibi), başka bir kaynaktan bir değer kullanır, size [bir bağımlılık ayarlayın](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-define-dependencies) şablondaki.

Ayrıca, şablonu altyapıda yapılan güncelleştirmeler için kullanabilirsiniz. Örneğin, çözümünüze bir kaynak ekleyebilir ve zaten dağıtılmış kaynaklar için yapılandırma kuralları ekleyebilirsiniz. Şablon kaynağı zaten var olan bir kaynak oluşturulmasını belirtiyorsa, Resource Manager yeni bir varlık oluşturmak yerine bir güncelleştirme gerçekleştirir. Yeni oluşturulacak şekilde Kaynak Yöneticisi'ni mevcut varlık aynı durumuna güncelleştirir.

Kurulum'a dahil olmayan yazılım yükleme gibi daha fazla işlem, ihtiyacınız olduğunda resource Manager senaryoları için uzantılar sağlar.

### <a name="resource-tracking"></a>Kaynak İzleme

Kuruluşunuzdaki kullanıcılar için abonelik kaynakları eklediğinizde, kaynakları uygun departmanı, müşteri ve ortam ile ilişkilendirilecek daha önemli hale gelir. Meta veri kaynaklarında ekleyebileceğiniz [etiketleri](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags). Kaynak veya sahibi hakkında bilgi sağlamak için etiketleri kullanın. Etiketleri yalnızca toplama ve kaynakları farklı şekillerde gruplamak, ancak geri ödeme amacıyla verilerin de olanak sağlar.

Kaynak grubu ve kaynakları ve karmaşık bir koleksiyon olduğunda etiketler kullanın. Bu varlıkları sizin için anlamlı bir şekilde görselleştirmeniz gerekir. Örneğin, kuruluşunuzda benzer bir rolü hizmet veya aynı departmana ait olan kaynakları etiketleyebilirsiniz.

Etiket, kuruluşunuzdaki kullanıcılar daha sonra tanımlamak ve yönetmek zor olabilecek birden çok kaynaklar oluşturabilir. Örneğin, bir projenin tüm kaynaklarını silmek isteyebilirsiniz. Bu kaynaklar proje için etiketlenmemişse bunları el ile bulmalıdır. Etiketleme, aboneliğinizden doğan gereksiz maliyetleri azaltmanın önemli bir yoludur.

Kaynakların bir etiketi paylaşması için aynı kaynak grubunda bulunmaları gerekmez. Kuruluşunuzdaki tüm kullanıcılara uygulanan yanlışlıkla birbirinden kısmen farklı etiketler (örneğin "departman" yerine "Depart") yerine genel etiketler kullanmasını sağlamak için kendi etiket sınıflandırmanızı oluşturabilirsiniz.

Kaynak ilkeleri kuruluşunuz için standart kurallar oluşturmanıza olanak sağlar. Kaynakları uygun değerlerle etiketlendiğinden emin olmak için ilkeleri oluşturabilirsiniz.

Azure portalından etiketli kaynakları da görüntüleyebilirsiniz. [Kullanım raporu](https://docs.microsoft.com/azure/billing/billing-understand-your-bill) için aboneliğinizi etiket adları ve değerleri içerir, bu nedenle, bozabilir maliyetleri etiketlere göre.

Etiketler hakkında daha fazla bilgi için bkz. [fatura etiketleri İlkesi girişimi](../azure-policy/scripts/billing-tags-policy-init.md).

Etiketler için aşağıdaki sınırlamalar geçerlidir:

- Her kaynak veya kaynak grubu en fazla 15 etiket anahtar/değer çiftleri olabilir. Bu sınırlama yalnızca kaynak grubu veya kaynağa doğrudan uygulanan etiketler için geçerlidir. Bir kaynak grubu, her 15 etiket anahtarı/değer çiftine sahip çok sayıda kaynak içerebilir.

- Etiket adı 512 karakterle sınırlıdır.

- Etiket değeri 256 karakterle sınırlıdır.

- Kaynak grubuna uygulanan etiketler, bu kaynak grubundaki kaynaklar tarafından devralınmaz.

Bir kaynak ile ilişkilendirmeniz gereken 15'ten fazla değer varsa, etiket değeri için JSON dizesi kullanın. JSON dizesi, tek etiket anahtarı uygulanan birden fazla değer içerebilir.

#### <a name="tags-for-billing"></a>Faturalandırma etiketleri

Etiketler fatura verilerinizi gruplandırmak etkinleştirin. Örneğin, birden çok VM farklı kuruluşlarda çalıştırıyorsanız, maliyet merkezi tarafından grubu kullanımı için etiketleri kullanın. Etiketleri, maliyetler üretim ortamında çalışan VM'ler için fatura kullanımı gibi çalışma zamanı ortamı tarafından kategorilere ayırmak için de kullanabilirsiniz.

Etiketler hakkında bilgi alabilirsiniz [Azure kaynak kullanım ve RateCard API'leri](https://docs.microsoft.com/azure/billing/billing-usage-rate-card-overview) veya kullanım virgülle ayrılmış değerler (CSV) dosyası. Kullanım dosyası indirin [Azure hesap portalını](https://account.windowsazure.com/) veya [EA portal](https://ea.azure.com/).

Fatura bilgileri programlı erişim hakkında daha fazla bilgi için bkz. [Microsoft Azure kaynak tüketiminize öngörü](https://docs.microsoft.com/azure/billing/billing-usage-rate-card-overview). REST API işlemleri için bkz: [Azure faturalandırma REST API Başvurusu](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c).

Etiketler fatura Destek Hizmetleri için kullanım CSV indirdiğinizde, etiketler etiketleri sütununda görünür.

### <a name="critical-resource-controls"></a>Kritik kaynak denetimleri

Kuruluşunuz için abonelik Çekirdek Hizmetleri ekler gibi bu hizmetleri iş kesintileri önlemek kullanılabilir olmasını sağlamak daha önemli hale gelir. [Kaynak kilitleri](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-lock-resources) burada değiştirilmesini veya silinmesini haritamın uygulamalarınızı veya Bulut altyapısı üzerinde önemli bir etkisi yüksek değerli kaynaklar üzerinde işlemleri kısıtlamak olanak sağlar. Kilitleri, bir aboneliğe, kaynak grubu ya da kaynağa uygulayabilirsiniz. Genellikle, sanal ağlar, ağ geçitleri ve depolama hesapları gibi temel kaynaklara kilitleri uygulayın.

Kaynak kilitleri iki değer desteklenmektedir: **CanNotDelete** ve **salt okunur**. **CanNotDelete** anlamına gelir (uygun hakları) sahip kullanıcılar yine de okuyun veya bir kaynağı değiştirmek ancak bunu silemezsiniz. **Salt okunur** silemez veya bir kaynağı değiştirme yetkili kullanıcıların anlamına gelir.

Kaynak kilitleri uygulamak gönderilen operations oluşan yönetim düzlemi gerçekleşen işlemlere <https://management.azure.com>. Kilitler nasıl kaynakları kendi işlevleri gerçekleştiren kısıtlama. Kaynak değişiklikleri kısıtlıdır, ancak kaynak işlemleri sınırlı değildir. Örneğin, bir **salt okunur** kilit üzerinde bir SQL veritabanı, veritabanı silmesini veya engeller ancak oluşturma, güncelleştirme ya da veritabanı verilerini silme engellemez.

Uygulama **salt okunur** gibi görünen bazı işlemleri operations gerektiren ek eylemler okunur beklenmeyen sonuçlara neden olabilir. Örneğin, yerleştirme bir **salt okunur** bir depolama hesabı üzerindeki kilidi anahtarları listeleme gelen tüm kullanıcıları engeller. Döndürülen anahtarları yazma işlemleri için kullanılabilir olmadığından anahtarları listeleme işlemi bir POST isteği gerçekleştirilir.

![Kritik kaynak denetimleri](./media/governance-in-azure/security-governance-in-azure-fig5.png)

Başka bir örnek için yerleştirme bir **salt okunur** bir Azure App Service kaynak kilidi, o etkileşime yazma erişim gerektirdiğinden kaynak dosyalarını görüntüleme Visual Studio sunucu Gezgini'nde engeller.

Rol tabanlı erişim denetimi, tüm kullanıcılar ve roller bir kısıtlama uygulamak yönetim kilitleri kullanın. İzinleri ayarlama bilgi edinmek için [RBAC ve Azure portalını kullanarak erişimini yönetme](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal).

Bir üst kapsamda bir kilit uyguladığınızda, ilgili kapsam içindeki tüm kaynaklar aynı kilit devralır. Daha sonra eklenen kaynaklar kilit üst öğeden devralır. Devralmada en kısıtlayıcı kilit önceliklidir.

Yönetim kilitlerini Sil ya da oluşturmak için Microsoft.Authorization/ veya Microsoft.Authorization/locks/ eylem erişiminiz olmalıdır. Yerleşik rolleri, yalnızca sahip ve kullanıcı erişimi Yöneticisi, bu eylemlerin verilir.

### <a name="api-access-to-billing-information"></a>Fatura bilgileri için API erişimi

Azure faturalandırma API'lerini kullanımı ve kaynak veri çekmek üzere, tercih edilen veri analizi araçları kullanın. Azure kaynak kullanım ve RateCard API'leri, doğru şekilde tahmin edip, maliyetleri yönetmenize yardımcı olabilir. API'ler, bir kaynak sağlayıcısı ve Azure Resource Manager tarafından kullanıma sunulan API'ler ailesinin bir parçası olarak uygulanır.

#### <a name="resource-usage-api-preview"></a>Kaynak kullanım API'si (Önizleme)

Azure kullanan [kaynak kullanım API'si](https://msdn.microsoft.com/library/azure/mt219003) tahmini Azure kullanım verilerinizi almak için. API içerir:

- **RBAC**: yapılandırma erişim ilkeleri [Azure portalında](https://portal.azure.com/) aracılığıyla veya [Azure PowerShell cmdlet'lerini](https://docs.microsoft.com/powershell/azure/overview) hangi kullanıcıların veya uygulamaların aboneliğin kullanım verilerine erişim elde edebilirsiniz belirtmek için. Çağıranlar, kimlik doğrulaması için standart Azure Active Directory belirteçleri kullanmanız gerekir. Arayanın belirli bir Azure aboneliği için kullanım verilerine erişim elde etmek için faturalandırma okuyucusu, okuyucu, sahibi veya katkıda bulunan rolüne ekleyin.

- **Günlük veya saatlik toplamalar**: çağıranlar, günlük veya saatlik artışlarla Azure kullanım verilerini isteyip istemediğinizi belirtebilirsiniz. Varsayılan günlük.

- **Örnek meta veri (kaynak etiketleri içerir)**: alma tam Kaynak URI gibi örnek düzeyi ayrıntısı (/subscriptions/ {abonelik-kimliği} /..), kaynak grubu bilgileri ve kaynak etiketleri. Bu meta veriler belirleyici hem de programsal olarak kullanım için çapraz ücretlendirme gibi durumlarda etiketlere göre ayırmanıza yardımcı olur.

- **Kaynak meta verileri**: Kaynak Ayrıntıları ölçüm adı, ölçüm kategorisi, ölçüm alt kategorisi, birim ve bölge gibi daha iyi bir ne tüketildiğinin'ın anlayış çağıran verin. Ayrıca Azure portalı kaynak meta verileri terminolojisi hizalama çalışıyoruz Azure kullanım CSV, CSV ve diğer genel kullanıma yönelik deneyimler deneyimler verilerin bağıntısını yardımcı olması için faturalama EA.

- **Tüm kullanım teklif türleri**: kullanım verileri, tüm türleri, Kullandıkça Öde, MSDN, parasal taahhüt, parasal kredi ve EA teklifi için kullanılabilir.

#### <a name="resource-ratecard-api"></a>Kaynak RateCard API'si

Kullanılabilir Azure kaynaklarını ve tahmini fiyatlandırma bilgileri için her bir listesini almak için Azure kaynak RateCard API'si kullanın. API içerir:

- **RBAC**: Azure portalında veya hangi kullanıcıların veya uygulamaların RateCard verilere erişim elde edebilirsiniz belirtmek için Azure PowerShell cmdlet'leri aracılığıyla erişim ilkelerinizi yapılandırın. Çağıranlar, kimlik doğrulaması için standart Azure Active Directory belirteçleri kullanmanız gerekir. Arayanın belirli bir Azure aboneliği için kullanım verilerine erişim elde etmek için okuyucu, sahibi veya katkıda bulunan rolüne ekleyin.

- **Kullandıkça Öde, MSDN, parasal taahhüt ve parasal kredi teklifleri (ancak değil EA) için destek**: Bu API, Azure teklifi düzeyinde fiyat bilgileri sağlar. Bu API'yi çağıran kaynak ayrıntıları ve fiyatları almak için teklif bilgilerini geçmesi gerekir. EA kayıt tarifelerine EA teklifler özelleştirdiğiniz olduğundan şu anda desteklenmiyor. 

#### <a name="scenarios"></a>Senaryolar

Kullanım ve RateCard API'leri birleşimi bu senaryoları mümkün kılar:

- **Azure faturalandırmanızı anlamanın ay boyunca harcama**: kullanım bileşimini kullanın ve RateCard API'leri, bulutunuzu daha iyi içgörüler edinmek için ay boyunca ayırabilirsiniz. Saatlik ve günlük kullanım ve Ücret tahminleri çözümleyebilirsiniz.

- **Uyarılar ayarlayın**: tahmini bulut tüketimi ve ücretleri alın ve kaynak veya parasal tabanlı uyarılar ayarlayın kullanım ve RateCard API'lerini kullanın.

- **Fatura tahmin**: Get tahmini tüketim ve bulut harcamalarını ve fatura bir faturalama döneminin sonunda ne olacağını tahmin etmek için makine öğrenimi algoritmaları uygulayın.

- **Maliyet analizi öncesi bir tüketim gerçekleştirmek**: iş yüklerinizi Azure'a taşıyarak ne kadar faturanız için beklenen kullanım olacaktır tahmin etmek için RateCard API kullanın. Ayrıca, diğer bulut ya da özel Bulutlar mevcut iş yüklerini varsa, Azure ile kullanım eşleyebilirsiniz ücretler Azure daha iyi bir tahminini almak için harcadığınız. Bu tahmin, teklife Özet ve parasal taahhüt ve para kredisi gibi Kullandıkça Öde ötesinde farklı teklif türleri arasında karşılaştırma olanağı sağlar.

- **What-if çözümlemesi yapma**: başka bir bölgede veya başka bir Azure kaynak yapılandırması iş yüklerini çalıştırmak için daha uygun maliyetli olup olmadığını belirleyebilir. Azure kaynak maliyetleri kullanmakta olduğunuz Azure bölgesine göre değişebilir. Başka bir Azure Teklif türü bir Azure kaynağında daha iyi bir fiyat sağlar, ayrıca belirleyebilirsiniz.

### <a name="networking-controls"></a>Ağ denetimleri

Kaynaklara erişimi (corporation'ın ağ içinde) iç veya dış (Internet) aracılığıyla olabilir. Kuruluşunuzdaki kullanıcıların yanlışlıkla kaynaklar içinde yanlış yere yerleştirin ve olası kötü amaçlı erişimi açabilirsiniz kolaydır. İle şirket içi cihazlar gibi kuruluşların uygun denetimleri, Azure kullanıcılarının doğru kararlar emin olmak için eklemeniz gerekir.

![Ağ denetimleri](./media/governance-in-azure/security-governance-in-azure-fig6.png)

Abonelik İdaresi için temel erişim denetimi aşağıdaki çekirdek kaynaklar sağlar.

#### <a name="network-connectivity"></a>Ağ bağlantısı

[Sanal ağlar](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) alt ağlar için kapsayıcı nesneleridir. Kesinlikle gerekli'da, bir sanal ağ genellikle uygulamalara iç şirket ağlarına bağlanmak için kullanılır. Azure sanal ağ hizmeti, güvenli bir şekilde Azure kaynaklarını sanal ağlarla birbirine olanak sağlar.

Bir sanal ağ, buluttaki kendi ağınızın bir gösterimidir. Azure bulutunun aboneliğinize adanmış mantıksal bir ayırma bir sanal ağdır. Bu gibi durumlarda, sanal ağlar aynı zamanda şirket içi ağınıza bağlanabilirsiniz.

Azure Sanal Ağları için özellikleri aşağıda verilmiştir:

- **Yalıtım**: sanal ağlar birbirlerinden. Geliştirme, test ve üretim için aynı CIDR adres bloklarını kullanan ayrı sanal ağlar oluşturabilirsiniz. Buna karşılık, farklı CIDR adres bloklarını kullanan ve ağları birbirine bağlama, birden fazla sanal ağ oluşturabilirsiniz. Bir sanal ağ, birden çok alt ağa bölebilirsiniz. Azure, bir sanal ağa bağlı olan Vm'leri ve Azure Cloud Services rol örnekleri için iç ad çözümleme sağlar. İsteğe bağlı olarak, kendi DNS sunucularınızı Azure dahili ad çözümlemesi yerine kullanılacak sanal ağ da yapılandırabilirsiniz.

- **Internet bağlantısı**: tüm Azure sanal makineler ve sanal bir ağa bağlı Cloud Services rol örneklerini varsayılan olarak, internet erişimi vardır. Gelen erişimi belirli kaynaklara, gerektiğinde de etkinleştirebilirsiniz.

- **Azure kaynak bağlantısı**: Bulut Hizmetleri ve sanal makineleri gibi Azure kaynakları aynı sanal ağa bağlanabilir. Bunlar farklı alt ağlarda olsa bile, kaynakları özel IP adresleri aracılığıyla birbirine bağlanabilir. Azure, varsayılan yapılandırma ve rotaları yönetme zorunluluğunu alt ağlar, sanal ağlar ve şirket içi ağlar arasında yönlendirme sağlar.

- **Sanal ağ bağlantısı**: Sanal ağları birbirine bağlanabilir. Herhangi bir sanal ağa bağlı kaynaklara herhangi bir sanal ağ üzerindeki herhangi bir kaynağa ile iletişim kurabilir.

- **Şirket içi bağlantı**: internet üzerinden şirket içi ağlara, ağ ile Azure arasında özel ağ bağlantıları üzerinden veya bir siteden siteye sanal özel ağ (VPN) bağlantısı üzerinden sanal ağlara bağlayabilirsiniz.

- **Trafik filtreleme**: ağ trafiği (gelen ve giden) sanal makineler ve bulut Hizmetleri için kaynak IP adresi ve bağlantı noktası, hedef IP adresi ve bağlantı noktası ve protokol göre filtreleyebilirsiniz.

- **Yönlendirme**: isteğe bağlı olarak kendi yönlendirmelerinizi yapılandırma veya bir ağ geçidi arasında BGP yolları kullanarak Azure yönlendirme varsayılan ayarlarını geçersiz kılabilir.

#### <a name="network-access-controls"></a>Ağ erişim denetimleri

[Ağ güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) (Nsg'ler), ister bir güvenlik duvarı ve nasıl bir kaynak "ağ üzerinden konuşabilirsiniz için" kuralları sağlar. Bunlar, bir alt ağ (veya sanal makine) İnternet'e ya da aynı sanal ağdaki diğer alt ağlara nasıl bağlayabileceğini üzerinde denetim sağlar.

Bir ağ güvenlik grubu izin veren veya Azure sanal ağlarınıza bağlı kaynaklara ağ trafiği reddeden güvenlik kurallarının bir listesini içerir. Nsg'ler alt ağlar, tek VM'ler (Klasik) veya Vm'lere (Resource Manager) bağlı ağ arabirimleri (NIC'ler) ile ilişkili olabilir.

Bir NSG bir alt ağ ile ilişkilendirildiğinde kurallar alt ağa bağlı tüm kaynaklara uygulanır. Bir NSG'yi bir VM veya NIC ile ilişkilendirilmesi yoluyla trafiği daha fazla kısıtlayabilirsiniz

## <a name="security-and-compliance-with-organizational-standards"></a>Güvenlik ve kuruluş standartlarıyla uyumluluğu

Her iş farklı gereksinimleri vardır ve bulut çözümlerinden belirgin avantaj yiyelim. Yine de her tür müşterilerin buluta taşıma konusunda aynı temel konuları var. Müşterilerin bulut sağlayıcılarından istediklerinizi aşağıdaki gibidir:

- **Verilerimizi güvenli**: BT Liderleri için kabul bulut artan veri güvenliği ve yönetim denetimini sağlayabilirsiniz. Ancak bunlar buluta geçirme bunları geçerli şirket içi çözümleri daha saldırganlar için daha savunmasız bırakır, hala oluşabileceklerden endişelisiniz.

- **Verilerimizi özel kalmasını**: Bulut Hizmetleri benzersiz zorluklar Yükselt. Şirket altyapı maliyetlerinden tasarruf edin ve kendi esnekliğinde geliştirmek için buluta aramak gibi bunlar ayrıca Denetim verilerinin depolandığı, kimin erişmekte olduğunu ve nasıl kullanıldıkları kaybetme endişesi.

- **Denetim bulunun**: şirketlerin daha fazla yenilikçi çözümler dağıtmak için bulutun avantajından yararlanın gibi bunlar denetim verilerinin kaybetmekten oluşabileceklerden endişelisiniz. Devlet kurumlarının müşteri verilerini yasal ve extralegal yollarla erişen son kavuşturma bazı Cıo'lar verilerini buluta depolamaya çok dikkatli olun.

- **Saydamlık Yükselt**: iş karar verenleri güvenlik, gizlilik ve denetim önemini anlayın. Ancak bunlar bağımsız olarak kendi verilerini ne nasıl depolandığını, erişilen ve güvenli doğrulama yeteneği de istersiniz.

- **Uyumluluğu**: şirketler bulut teknolojileri kullanımları genişlettikçe standartları ve düzenlemeleriyle kapsamı ve karmaşıklık iyileşmeye devam etmektedir. Şirketler, kendi uyumluluk standartlarını karşılanması bilmeniz gerekir.

## <a name="security-configuration-for-monitoring-logging-and-auditing"></a>İzleme, günlüğe kaydetme ve denetim için güvenlik yapılandırması

Azure aboneleri, birden fazla cihazda kendi bulut ortamlarını yönetebilir. Bu cihazları yönetim iş istasyonları, geliştirici PC'leri ve göreve özel izinleri bulunan ayrıcalıklı bile son kullanıcı cihazları içerebilir. 

Bazı durumlarda, yönetim işlevleri, Azure portalı gibi web tabanlı konsollar aracılığıyla gerçekleştirilir. Diğer durumlarda olabilir doğrudan bağlantılar Azure'da şirket içi sistemlerden VPN'ler, Terminal Hizmetleri, istemci uygulaması protokolleri veya Azure Hizmet Yönetimi API'si (SMAPI) (programlı olarak). Ayrıca, istemci uç noktaları ya da etki alanına katılmış veya yalıtılmış ve yönetilmeyen, tabletler veya akıllı telefonlar gibi olabilir.

Bu değişkenlik bulut dağıtımına önemli bir risk ekleyebilir. Yönetmenize, izlemenize ve yönetim eylemlerini denetlemek zor olabilir. Ayrıca bu değişkenlik bulut hizmetlerini yönetmek için kullanılan istemci uç noktalarına düzenlenmemiş erişim aracılığıyla güvenlik tehditlerine neden olabilir. Altyapı geliştirme ve yönetme amacıyla genel ya da kişisel iş istasyonlarını kullanmak, web’e gözatma (örneğin, su kaynağı saldırıları) ya da e-posta (örneğin, sosyal mühendislik ve kimlik avı) gibi öngörülemeyen tehdit vektörlerini açar.

İzleme, günlüğe kaydetme ve denetim, izleme ve yönetim etkinlikleri anlamak için bir temel sağlar. Tüm eylemlerin denetimi her zaman oluşturulan veri miktarı nedeniyle uygun olmayabilir. Ancak yönetim ilkelerinin verimliliğini denetimi en iyi bir uygulamadır.

Azure güvenlik yönetimi Azure Active Directory etki alanı Hizmetleri (AD DS) GPO'ları gelen dosya paylaşımı gibi tüm yöneticilerin Windows arabirimleri denetlemenize yardımcı olabilir. Yönetim iş istasyonları, izleme, günlüğe kaydetme ve denetim işlemleri içerir. Tüm yönetici ve geliştirici erişimini ve kullanımını izleyin.

### <a name="azure-security-center"></a>Azure Güvenlik Merkezi

[Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro) Abonelikteki kaynakların güvenlik durumu merkezi bir görünümünü sağlar. Bu, tehlike giren kaynakları önlemeye yardımcı öneriler sağlar. Kendi duruşunu adresleme riskine uygun hale getirmek Kurumsal izin belirli kaynak gruplarına sağlamaktan ayrıntılı ilkeleri--örneğin etkinleştirebilirsiniz.

![Azure Güvenlik Merkezi](./media/governance-in-azure/security-governance-in-azure-fig7.png)

Güvenlik Merkezi, Azure aboneliklerinizde tümleşik güvenlik izleme ve ilke yönetimi sağlar, aksi takdirde gözden kaçan geçebilir tehditleri ve güvenlik çözümlerinin geniş ekosistemiyle çalışır algılamaya yardımcı olur. Seçeneğini etkinleştirdikten sonra [güvenlik ilkeleri](https://docs.microsoft.com/azure/security-center/security-center-policies) bir aboneliğin kaynakları için Güvenlik Merkezi olası güvenlik açıklarını tanımlamak amacıyla kaynaklarınızın güvenliğini analiz eder. Ağ yapılandırmanız ile ilgili bilgiler hemen kullanımınıza sunulur.

Azure Güvenlik Merkezi, en iyi uygulama analizi ve Güvenlik İlkesi Yönetimi için bir Azure aboneliği içindeki tüm kaynakların bileşimini temsil eder. Ancak, güvenlik takımlar ve risk görevlileri önleyin, algılayın ve otomatik olarak toplar ve Azure kaynaklarınızı, ağ ve kötü amaçlı yazılımdan koruma programları ve güvenlik duvarları gibi iş ortağı çözümlerinden güvenlik verilerini analiz eden güvenlik tehditleri için etkinleştirir.

Ayrıca, Azure Güvenlik Merkezi Makine öğrenimi ve davranış analizi dahil olmak üzere gelişmiş analizler uygular. Microsoft ürünleri ve Hizmetleri, Microsoft dijital Suçlar birimi (DCU), Microsoft Güvenlik Yanıt Merkezi (MSRC) gelen genel tehdit bilgilerini kullanır ve dış akışları. Uygulayabileceğiniz [güvenlik idare](https://www.credera.com/blog/credera-site/azure-governance-part-4-other-tools-in-the-toolbox/) problem abonelik düzeyinde. Veya belirli gereksinimleri aşağı daraltmak ve bunları tek tek ilke tanımı kaynaklarında uygulayabilirsiniz.

Son olarak, Azure Güvenlik Merkezi kaynak güvenlik durumu bu ilkelere dayalı analiz eder ve panolarla ve olaylar gibi kötü amaçlı yazılım algılama veya kötü amaçlı IP bağlantı denemeleri uyarı sağlamak için bu bilgileri kullanır. Önerileri uygulama hakkında daha fazla bilgi için bkz. [Azure Güvenlik Merkezi'nde güvenlik önerilerini uygulama](https://docs.microsoft.com/azure/security-center/security-center-recommendations).

Azure Güvenlik Merkezi, aşağıdaki Azure kaynakları izler:

- Sanal makineler (VM'ler) (bulut hizmetleri de dahil)

- Sanal ağlar

- SQL veritabanları

- İş ortağı çözümleri gibi bir web uygulaması güvenlik duvarı vm'lerde ve Azure aboneliğiniz tümleşik [App Service ortamı](https://docs.microsoft.com/azure/app-service/app-service-app-service-environments-readme)

Güvenlik Merkezi'ne ilk kez eriştiğinizde aboneliğinizdeki tüm sanal makinelerde veri toplama etkinleştirilir. Veri toplama etkin tutmak, ancak yapabilecekleriniz öneririz [devre dışı](https://docs.microsoft.com/azure/security-center/security-center-faq) Güvenlik Merkezi ilkesinde.

### <a name="log-analytics"></a>Log Analytics

Azure Log Analytics yazılım geliştirme ve hizmet takımın bilgi güvenliği ve [idare program](https://github.com/Microsoft/azure-docs/blob/master/articles/log-analytics/log-analytics-security.md) kendi iş gereksinimlerini destekler. Bu yasa ve düzenlemeler çerçevesinde için açıklandığı aynılarını [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/) ve [Microsoft Güven Merkezi uyumluluğu](https://www.microsoft.com/TrustCenter/Compliance/default.aspx). Log Analytics, güvenlik gereksinimleri nasıl kurar ve güvenlik denetimleri tanımlar ve yönetir ve izleyiciler riskleri de açıklanmıştır vardır. Yıllık, takım ilkeler, standartlar, yordamları ve yönergeleri gözden geçirir.

Her bir Log Analytics geliştirme takım üyesine biçimsel uygulama güvenlik eğitim alır. Bir sürüm denetimi sistemi, her bir yazılım projesi geliştirme ile korumanızı sağlar.

Microsoft, Microsoft tüm hizmetleri değerlendirir ve denetleyen güvenlik ve uyumluluk bir ekibe sahiptir. Güvenlik müdürleri takım yapın ve Log Analytics geliştiren mühendislik bölümlerine ilişkili değilsiniz. Güvenlik sorumlularını, kendi yönetim zinciri var. Bunlar, güvenlik ve uyumluluk sağlamak için ürün ve Hizmetleri bağımsız değerlendirmeleri yürütün.

Log analytics'in temel işlevleri Azure'da çalışan hizmetleri kümesi tarafından sağlanır. Her hizmet belirli bir yönetim işlevi sağlar ve farklı hizmetleri birleştirerek farklı yönetim senaryoları elde edebilirsiniz.

![Yönetim için Azure Hizmetleri](./media/governance-in-azure/security-governance-in-azure-fig9.JPG)

Bu Yönetim Hizmetleri, bulutta tasarlanmıştır. Şirket içi kaynaklarınızı dağıtıp içermeyen şekilde tamamen Azure'da barındırılan. Minimum yapılandırmaya sahip olacak ve birkaç dakika içinde çalışır duruma olabilir.

Herhangi bir Windows veya Linux bilgisayarda, veri merkezinizdeki ekleyeceğiniz bir aracı ve Log Analytics'e veri gönderir. Burada, bulutta veya şirket içi hizmetlerden toplanan tüm verilerle birlikte çözümlenebilir. Azure Backup ve Azure Site Recovery, şirket içi kaynaklar için yedekleme ve yüksek kullanılabilirlik için bulutun avantajından yararlanmak için kullanın.

![Azure Panosu Yönetim Hizmetleri](./media/governance-in-azure/security-governance-in-azure-fig8.png)

Buluttaki Runbook'lar genellikle şirket içi kaynaklarınıza erişemez ancak veri merkezinizde runbook'ları barındıracak bir veya daha çok bilgisayarda bir aracı yükleyebilirsiniz. Bir runbook'u başlattığınızda yalnızca bulutta veya yerel bir çalışan çalışmasını isteyip istemediğinizi belirtin.

Microsoft ve iş ortaklarının Log analytics'te yatırımınızın değerini artırmak için Azure aboneliğinize eklemek farklı çözümler vardır. Örneğin, Azure'un sunduğu [yönetim çözümleri](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-solutions)--kümeleri mantığı bir veya daha fazla yönetim hizmeti kullanarak bir yönetim senaryosunu uygulayan önceden paketlenmiş.

![Azure yönetim çözümlerine Galerisi](./media/governance-in-azure/security-governance-in-azure-fig10.png)

Bir iş ortağı olarak, uygulamalarınızı ve hizmetlerinizi desteklemek ve bunları Azure Market ya da hızlı başlangıç şablonları aracılığıyla kullanıcılara sağlamak için kendi çözümlerinizi oluşturabilir.

## <a name="performance-alerting-and-monitoring"></a>Performans uyarı ve izleme

### <a name="alerting"></a>Uyarı

Uyarıları izleme Azure kaynak ölçümleri, olayları ve günlükleri bir yöntemdir. Belirlediğiniz bir koşulu karşılandığında, size bildirir.

Uyarıları gibi hizmetler kullanılabilir:

- **Azure Application Insights**: web test ve ölçüm uyarılarını etkinleştirir. Daha fazla bilgi için [Application Insights uyarıları ayarlamak](https://docs.microsoft.com/azure/application-insights/app-insights-alerts) ve [kullanılabilirlik ve yanıt herhangi bir Web sitesi izleme](https://docs.microsoft.com/azure/application-insights/app-insights-monitor-web-app-availability).

- **Log Analytics**: etkinlik ve tanılama günlüklerini Log analytics'e yönlendirilmesini sağlar. Bu ölçüm, günlük ve diğer uyarı türleri sağlar. Daha fazla bilgi için [Log analytics'teki uyarılar](https://docs.microsoft.com/azure/log-analytics/log-analytics-alerts).

- **Azure İzleyici**: ölçüm değerleri ve etkinlik günlüğü olayları göre uyarılar sağlar. Kullanabileceğiniz [Azure İzleyici REST API](https://msdn.microsoft.com/library/dn931943.aspx) Uyarıları yönetmek için. Daha fazla bilgi için [uyarılar oluşturmak için Azure portalı, PowerShell veya komut satırı arabirimini kullanarak](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-alerts-portal).

### <a name="monitoring"></a>İzleme

Bulut uygulamanızdaki performans sorunlarını işletmenizi etkileyebilir. Birden çok birbirine bağlı bileşenleri ve sık sık sürümleri ile performans düşüşü yaşanması herhangi bir zamanda gerçekleşebilir. Ve bir uygulama geliştiriyorsanız, kullanıcılarınız genellikle test bulamadınız sorunları keşfedin. Bu sorunlar hakkında hemen bilmek ve tanılama ve bunları düzeltmek için gereken araçları elde edersiniz.

Çeşitli Azure uygulamaları ve Hizmetleri izlemek için Araçlar yoktur. Bazı kendi özellikleri çakışıyor. Kısmen geliştirme ve bir uygulamanın işlem arasında Bulanıklaştırma nedeniyle budur.

Asıl araçlar şunlardır:

- **Azure İzleyici** Azure üzerinde çalışan hizmetleri izlemek için temel bir araçtır. Aktarım hızı bir hizmet ve bulunduğu ortam hakkında altyapı düzeyinde veriler sağlar. Azure İzleyici, tüm uygulamalarınızı Azure'da ve ölçek büyütme veya küçültme kaynakları karar yönetiyorsanız, başlamanıza yardımcı olabilir.

- **Application Insights** geliştirme için ve bir üretim izleme çözümü olarak kullanılabilir. Bu paket, uygulamanızda neler olup bittiğini daha iç bir görünümünü sağlar, böylece yükleyerek çalışır. Yanıt süreleri, bağımlılıklar, özel durum izleme, anlık görüntüler ve yürütme profillerini hata ayıklama verilerini içerir. Tüm bu telemetri uygulama hatalarını ayıklamanıza yardımcı olması için hem kullanıcıların ile yaptığı anlamanıza yardımcı olması için çözümlemek için araçlar sağlar. Yanıt süreleri bir ani değişiklik nedeniyle bir şey bir uygulama veya dış oranlarınız sorun olup olmadığını söyleyebilir. Kullanırsanız Visual Studio ve uygulama hatalı olduğundan, bunu düzeltmek için sorun kod satırına sağ gidin.

- **Log Analytics** performansını ayarlamak ve üretimde çalışan uygulamalara bakım planı için ihtiyacınız olanlar içindir. Toplar ve 10-15 dakikalık bir gecikmeyle birçok kaynaktan verileri toplar. Azure için bütünsel bir BT yönetimi çözümü sağlar şirket içi ve üçüncü taraf bulut tabanlı altyapınızı (örneğin, Amazon Web Hizmetleri). Bu, veri kaynaklarında, analiz etmeye tüm günlükleri arasında karmaşık sorgular sağlar ve belirli koşullara göre proaktif olarak uyarabilir sağlar. Bile, merkezi bir depoda özel veri toplamak ve ardından sorgu ve bu verileri görselleştirin.

- **System Center Operations Manager** yönetme ve büyük bulut kurulumları izlemek içindir. İle şirket içi Windows Server için bir yönetim aracı olarak olmayabilir ve bulutların Hyper-V tabanlı, ancak ayrıca tümleştirin ve Azure uygulamalarını yönetme. Diğerlerinin yanı sıra Application Insights mevcut Canlı uygulamaları yükleyebilirsiniz. Operations Manager uygulama kalırsa, saniyeler içinde bildirir.


## <a name="next-steps"></a>Sonraki adımlar

- [Azure Resource Manager şablonları oluşturmaya yönelik en iyi uygulamalar](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices)

- [Azure abonelik İdaresi uygulama örnekleri](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-subscription-examples)

- [Microsoft Azure kamu](https://docs.microsoft.com/azure/azure-government/)
