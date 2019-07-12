---
title: Kiracılar arası yönetim deneyimleriyle Azure Deniz
description: Temsil edilen Azure kaynak yönetimi, kiracılar arası yönetim deneyimi sağlar.
author: JnHs
ms.service: lighthouse
ms.author: jenhayes
ms.date: 07/11/2019
ms.topic: overview
manager: carmonm
ms.openlocfilehash: 15454d4b3f0abad6166c4b163df6c8652669d649
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67809907"
---
# <a name="cross-tenant-management-experiences"></a>Kiracılar arası yönetim deneyimleri

Bu makalede, bir hizmet sağlayıcısı olarak ile kullanabileceğiniz senaryoları [Azure kaynak yönetimi temsilcisi](../concepts/azure-delegated-resource-management.md) kendi kiracısında birden çok müşteriler için Azure kaynaklarını yönetmek için [Azure portalı ](https://portal.azure.com).

> [!NOTE]
> Temsil edilen Azure kaynak yönetimi, kiracılar arası yönetimini basitleştirmek için birden çok kiracının kendi olan bir kuruluş içinde de kullanılabilir.

## <a name="understanding-customer-tenants"></a>Müşteri kiracılar anlama

Azure Active Directory (Azure AD) kiracısı, bir kuruluşun gösterimidir. Bir kuruluş bir ilişki ile Microsoft kaydolarak Azure, Microsoft 365 veya diğer hizmetler için oluşturduklarında aldığı Azure AD ayrılmış bir örneğidir. Her Azure AD kiracısı diğer Azure AD kiracılarıyla ayrıdır ve kendi Kiracı kimliği (GUID) vardır. Daha fazla bilgi için bkz. [Azure Active Directory nedir?](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis)

Genellikle, bir müşterinin Azure kaynaklarını yönetmek için hizmet sağlayıcılarının gerektiren kullanıcı hesaplarını oluşturmak ve yönetmek için müşterinin Kiracı yönetici, müşterinin Kiracı ile ilişkili bir hesabı kullanarak Azure portalında oturum açmak gerekir hizmet sağlayıcısı.

İle temsil edilen Azure kaynak yönetimi, abonelik, kaynak grupları ve müşterinin Kiracı kaynaklara erişmek ve yönetmeden hizmet sağlayıcısının Kiracı içindeki kullanıcılar ekleme işlemini belirtir. Bu kullanıcılar daha sonra kendi kimlik bilgilerini kullanarak Azure portalında oturum açabilir. Azure portalında bunlar erişime sahip oldukları tüm müşterilere ait kaynakları yönetebilir. Bu adresini ziyaret ederek yapılabilir [müşterilerimin](../how-to/view-manage-customers.md) Azure portalında veya müşterinin hizmet aboneliği, Azure portalında veya API'ler aracılığıyla doğrudan bağlamında çalışma sayfası.

Temsil edilen Azure kaynak yönetimi, farklı kiracıların farklı hesaplarında oturum açmak zorunda kalmadan birden çok müşteri için kaynakları yönetmek büyük esneklik sağlar. Örneğin, bir hizmet sağlayıcısı farklı sorumluluklara ve erişim düzeyleri ile üç müşteriler burada gösterildiği gibi olabilir:

![Hizmet sağlayıcısı sorumlulukları gösteren üç müşteri Kiracı](../media/azure-delegated-resource-management-customer-tenants.jpg)

Temsil edilen Azure kaynak Yönetimi'ni kullanarak, yetkili kullanıcılar hizmet sağlayıcısının Kiracı için bu kaynaklara erişmek için burada gösterildiği şekilde oturum açabilir:

![Yönetilen bir hizmet sağlayıcısı Kiracı müşteri kaynakları](../media/azure-delegated-resource-management-service-provider-tenant.jpg)

## <a name="supported-services-and-scenarios"></a>Desteklenen hizmetler ve senaryolar

Şu anda, kiracılar arası yönetim deneyimi temsilci müşteri kaynaklarıyla aşağıdaki senaryoları destekler:

[Azure Otomasyonu](https://docs.microsoft.com/azure/automation/):

- Erişim ve temsilci müşteri kaynaklarıyla çalışmak için Otomasyon hesaplarını kullanma

[Azure yedekleme](https://docs.microsoft.com/azure/backup/):

- Yedekleme ve müşteri kiracılarındaki müşteri verilerini geri yükleme

[Azure Kubernetes Service'i (AKS)](https://docs.microsoft.com//azure/aks/):

- Barındırılan Kubernetes ortamlarını yönetin, dağıtın ve müşteri kiracılar içinde kapsayıcılı uygulamaları yönetme

[Azure İzleyici](https://docs.microsoft.com/azure/azure-monitor/):

- Azure portalında veya programlama aracılığıyla REST API çağrısı, tüm abonelikler arasında uyarıları görüntüleme olanağı ile temsil edilen abonelikler için uyarıları görüntüle
- Temsilci abonelikler için etkinlik günlüğü ayrıntılarını görüntüle
- Log analytics: Birden çok kiracının uzaktan müşteri çalışma alanlarını sorgu verileri

[Azure İlkesi](https://docs.microsoft.com/azure/governance/policy/):

- Temsilci abonelikleri içinde atanan ilkeler için ayrıntıları uyumluluk anlık görüntüleri Göster
- Oluşturma ve yetkilendirilmiş bir abonelik dahilinde ilke tanımlarının düzenleme
- Temsilci aboneliğinde müşteri tarafından tanımlanan ilke tanımları atayın
- Müşteriler kendilerini yazılan tüm ilkeler yanı sıra hizmet sağlayıcısı tarafından yazılan ilkeleri bakın
- Yönetilen kimlik müşteri yapılandırdıysa müşteri kiracılar içindeki Deployıfnotexists atamalar düzeltebilir ve *roleDefinitionIds* , ilke ataması

[Azure Kaynak Grafiği](https://docs.microsoft.com/azure/governance/resource-graph/):

- Müşteri Kiracı veya hizmet sağlayıcısının Kiracı için bir aboneliğe ait olup olmadığını belirlemenize izin verilen sorgu sonuçlarında artık Kiracı Kimliğini içerir.

[Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/):

- Kiracılar arası görünürlük
  - Güvenlik ilkeleriyle uyumluluğu izlemek ve güvenlik kapsamı tüm kiracıların kaynakları genelinde emin olun.
  - Tek bir görünümde birden çok müşteriyi arasında sürekli Mevzuat uyumluluğu izleme
  - İzleme, Önceliklendirme ve öncelik eyleme dönüştürülebilir güvenlik önerileri ile güvenli puanı hesaplamaya
- Kiracılar arası güvenlik duruşunu Yönetimi
  - Güvenlik ilkelerini yönetme
  - Eyleme dönüştürülebilir güvenlik önerileri ile uyumsuz olan kaynaklar üzerinde eylem yapın
  - Güvenlikle ilgili veri toplamak ve depolamak
- Kiracılar arası tehdit algılama ve koruma
  - Kiracıların kaynakları genelinde tehditleri algılayın
  - Just-ın-time (JIT) VM erişimi gibi gelişmiş tehdit koruması denetimleri uygulayın
  - Ağ güvenlik grubu yapılandırması Uyarlamalı ağ sağlamlaştırma ile sağlamlaştırma
  - Sunucular, yalnızca uygulamalar ve işlemler Uyarlamalı uygulama denetimleri ile olmaları gerektiği çalıştırdığınızdan emin olun
  - Önemli dosya ve kayıt defteri girdileri dosya bütünlüğünü izleme (FIM) ile değişiklikleri izle

[Azure hizmet durumu](https://docs.microsoft.com/azure/service-health/):

- Müşteri kaynaklarla Azure kaynak durumu izlemesi
- Müşteriler tarafından kullanılan Azure hizmetlerinin sistem durumunu izleme

[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/):

- Azure sanal makinelerinde (VM uzantıları kopyalamak için farklı çalıştır hesaplarını kullanamazsınız. Not) müşteri kiracılar için olağanüstü durum kurtarma seçeneklerini Yönet

[Azure sanal makineleri](https://docs.microsoft.com/azure/virtual-machines/):

- Dağıtım sonrası yapılandırma ve Otomasyon görevlerini Azure Vm'lerinde müşteri kiracıda sağlamak için sanal makine uzantıları kullanma
- Azure Vm'leri müşteri kiracıda sorunlarını gidermek için önyükleme Tanılama'yı kullanma
- Seri konsol müşteri kiracılarındaki Vm'lerle erişim
- Bir VM'ye uzaktan oturum açma için Azure Active Directory kullanamazsınız ve bir VM, parolalar, gizli dizileri veya disk şifreleme için şifreleme anahtarları için bir anahtar kasası ile tümleştiremezsiniz unutmayın

[Azure sanal ağı](https://docs.microsoft.com/azure/virtual-network/):

- Sanal ağlar ve sanal ağ arabirim kartları (Vnıc'ler) müşteri kiracılar içinde dağıtın ve yönetin

Destek istekleri:

- Açık temsilci kaynaklardan destek isteklerini **Yardım + Destek** (temsilci kapsamadaki mevcut destek planı seçme) Azure portalında dikey penceresi

Tüm senaryoları sayesinde, lütfen aşağıdaki geçerli sınırlamaları unutmayın:

- Azure Resource Manager tarafından işlenen isteklerin temsil edilen Azure kaynak Yönetimi'ni kullanarak gerçekleştirilebilir. Bu istekler için URI işlemi ile başlayıp `https://management.azure.com`. Ancak, bir kaynak türü (örneğin, KeyVault gizli dizileri erişim veya depolama veri erişimi) örneği tarafından işlenen isteklerin ile temsil edilen Azure kaynak yönetimi desteklenmez. Bu istekler için URI işlemi genellikle başlangıç gibi Örneğiniz için benzersiz olan bir adresi ile `https://myaccount.blob.core.windows.net` veya `https://mykeyvault.vault.azure.net/`. İkincisi de genellikle yönetim işlemleri yerine veri işlemleri altındadır. 
- Rol atamaları, rol tabanlı erişim denetimi (RBAC) kullanmalıdır [yerleşik roller](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles). Tüm yerleşik roller dışında sahibi, kullanıcı erişimi Yöneticisi veya herhangi bir yerleşik rolü ile temsil edilen Azure kaynak yönetimi ile şu anda desteklenen [DataActions](https://docs.microsoft.com/azure/role-based-access-control/role-definitions#dataactions) izni. Özel roller ve [Klasik Abonelik Yöneticisi rolleri](https://docs.microsoft.com/azure/role-based-access-control/classic-administrators) da desteklenmez.
- Şu anda ekleme yapamazsınız bir aboneliği (veya bir abonelik içindeki kaynak grubu) için Azure aboneliği, Azure Databricks kullanıyorsa kaynak yönetimi temsilcisi. Benzer şekilde, bir abonelik ile yetiştirme kaydedilmişse **Microsoft.ManagedServices** kaynak sağlayıcısı, olmayacaktır şu anda bu abonelik için bir Databricks çalışma alanı oluşturabilirsiniz.

## <a name="using-apis-and-management-tools-with-cross-tenant-management"></a>Kiracılar arası yönetimi API'leri ve yönetim araçlarını kullanarak

Desteklenen hizmetler ve yukarıda listelenen senaryolar için doğrudan portalında veya API'ler ve Yönetim Araçları (örneğin, Azure CLI ve Azure PowerShell'i) kullanarak yönetim görevlerini gerçekleştirebilirsiniz. Tüm mevcut API'lere (desteklenen hizmetler için) temsilci kaynaklarla çalışırken kullanılabilir.

Ayrıca vardır API'leri Azure Temsilcili kaynağa yönetim görevlerini gerçekleştirmek için belirli. Daha fazla bilgi için bkz. **başvuru** bölümü.


## <a name="next-steps"></a>Sonraki adımlar

- Yerleşik kaynak yönetimi, ya da temsilci müşterilerinizi azure'a [Azure Resource Manager şablonlarını kullanarak](../how-to/onboard-customer.md) ya da [bir özel veya genel yönetilen Hizmetleri teklifi Azure Marketinde yayımlama](../how-to/publish-managed-services-offers.md).
- [Görüntüleme ve yönetme müşteriler](../how-to/view-manage-customers.md) giderek **müşterilerimin** Azure portalında.