---
title: Anlama ve Azure maliyet Yönetimi kapsamlar ile çalışma | Microsoft Docs
description: Bu makalede faturalama ve kaynak yönetimi kapsamları Azure'da ve maliyet yönetimi ve API kapsamlarını kullanma kullanılabilir anlamanıza yardımcı olur.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 03/13/2019
ms.topic: conceptual
ms.service: cost-management
manager: micflan
ms.custom: ''
ms.openlocfilehash: 4e7956e8873b552fcd73c51a51f51d99f21af324
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61003021"
---
# <a name="understand-and-work-with-scopes"></a>Kapsamları anlama ve bunlarla çalışma

Bu makalede faturalama ve kaynak yönetimi kapsamları Azure'da ve maliyet yönetimi ve API kapsamlarını kullanma kullanılabilir anlamanıza yardımcı olur.

## <a name="scopes"></a>Kapsamlar

A _kapsam_ Burada, Azure AD kullanıcılarının erişmek ve hizmetleri yönetmek Azure kaynak hiyerarşisindeki düğüm. Çoğu Azure kaynakları oluşturulur ve abonelikleri parçası olan kaynak grupları dağıtılır. Microsoft ayrıca, faturalama verileri yönetmek için rol özelleştirilmiş Azure abonelikleri Yukarıdaki iki hiyerarşi sunar:
- Ödemeler ve faturalar gibi faturalama verileri
- Maliyet ve ilke yönetimi gibi bulut Hizmetleri

Burada, faturalama verileri yönetmek, ödemeler belirli roller sahip, faturaları görüntülemek ve genel hesap yönetimi kuralları kapsamlardır. Faturalama ve hesap rolleri yönetilen ayrı olarak kullanan kaynak yönetimi için kullanılan olanlardan [Azure RBAC](../role-based-access-control/overview.md). Erişim denetimi farklar dahil olmak üzere ayrı kapsamların amacı açıkça ayırt etmek için bu denir _kapsamları faturalama_ ve _RBAC kapsamları_sırasıyla.

## <a name="how-cost-management-uses-scopes"></a>Maliyet Yönetimi kapsamları kullanma

Maliyet Yönetimi works hiç üzerinde kaynak kuruluşların tüm Faturalama hesabı ya da tek bir kaynak grubu olup olmadığını, erişimleri düzeyinde maliyetlerini yönetmek izin kapsamları. Fatura kapsam (abonelik türü) Microsoft sözleşmenize göre farklılık gösterse de RBAC kapsamları yoktur.

## <a name="azure-rbac-scopes"></a>Azure RBAC kapsamları

Azure kaynak yönetimi için üç kapsamları destekler. Her kapsam yönetme erişimi ve idare dahil olmak üzere, destekler, ancak bunlarla sınırlı olmamak üzere, maliyet yönetimi.

- [**Yönetim grupları** ](../governance/management-groups/index.md) -hiyerarşik kapsayıcılar, Azure abonelikleri düzenlemek için sekiz düzeylerini kadar.

    Kaynak türü: [Microsoft.Management/managementGroups](/rest/api/resources/managementgroups)

- **Abonelikleri** -Azure kaynakları için birincil kapsayıcıları.

    Kaynak türü: [Microsoft.Resources/subscriptions](/rest/api/resources/subscriptions)

- [**Kaynak grupları** ](../azure-resource-manager/resource-group-overview.md#resource-groups) -aynı yaşam döngüsünü paylaşan bir Azure çözümü için ilgili kaynakları mantıksal gruplandırmaları. Dağıtılan ve birlikte silindi örnek kaynaklar için.

    Kaynak türü: [Microsoft.Resources/subscriptions/resourceGroups](/rest/api/resources/resourcegroups)

Yönetim grupları abonelikler bir hiyerarşi halinde düzenlemenize olanak sağlar. Örneğin, Yönetim gruplarını kullanarak bir mantıksal bir kuruluş hiyerarşisi oluşturabilirsiniz. Ardından, takımlar Abonelikleri, üretim ve geliştirme/test iş yükleri için de sağlar. Ve ardından her bir alt sistemi veya bileşeni yönetmek için abonelikleri kaynak grupları oluşturun.

Bir kuruluş hiyerarşisi sağlar, maliyet oluşturma ve ilke uyumluluğunu kuruluş dökümü. Ardından, her bir öncü görüntüleyebilir ve geçerli maliyetlerini analiz edin. Ve hatalı harcama desenleri kaldırım kenarı ve en düşük düzeyinde Danışmanı önerileri ile maliyetleri iyileştirmek için bütçe oluşturabilirsiniz.

Maliyetleri görüntülemek ve isteğe bağlı olarak Maliyet yapılandırması, bütçe ve dışarı aktarma, gibi yönetmek için erişim verme İdaresi kapsamları Azure RBAC kullanarak gerçekleştirilir. Önceden tanımlanmış bir dizi bir rolü belirli bir kapsamda ve aşağıda tanımlanan eylemi gerçekleştirmek için grupları ve Azure AD kullanıcıları için Azure RBAC kullanın. Örneğin, bir yönetim grubu kapsama atanmış bir rol, ayrıca iç içe geçmiş Abonelikleriniz ve kaynak gruplarınız için aynı izinleri verir.

Aşağıdaki yerleşik rolleri her biri aşağıdaki kapsamlar için maliyet Yönetimi destekler:

- [**Sahibi** ](../role-based-access-control/built-in-roles.md#owner) – maliyetleri görüntüleyebilir ve maliyet yapılandırma dahil her şeyi yönetme.
- [**Katkıda bulunan** ](../role-based-access-control/built-in-roles.md#contributor) – maliyetleri görüntüleyebilir ve her şeyi, maliyet yapılandırılması dahil olmak üzere, ancak erişim denetimi dışında yönetin.
- [**Okuyucu** ](../role-based-access-control/built-in-roles.md#reader) – maliyet verileri ve yapılandırma dahil olmak üzere her şeyi görüntüleyebilir ancak değişiklik yapamazsınız.
- [**Maliyet Yönetimi katkıda bulunan** ](../role-based-access-control/built-in-roles.md#cost-management-contributor) – maliyetleri görüntüleyebilir ve maliyet yapılandırmasını yönetin.
- [**Maliyet Yönetimi okuyucu** ](../role-based-access-control/built-in-roles.md#cost-management-reader) – maliyet verileri ve yapılandırma görüntüleyebilirsiniz.

Maliyet Yönetimi katkıda önerilen en az ayrıcalıklı rol bulunur. Bu, oluşturmak ve bütçelerini yönetmek erişim veriyor ve daha etkili bir şekilde izlemek ve maliyetleri rapor verir. Maliyet Yönetimi Katkıda Bulunanlar, uçtan uca maliyet yönetim senaryolarını desteklemek için ek rolleri de gerektirebilir. Aşağıdaki senaryoları düşünün:

- **Hareket bütçelerini aşıldığında** – maliyet Yönetimi Katkıda Bulunanlar, oluşturma ve/veya fazla kullanımlar için otomatik olarak tepki vermek için Eylem grupları yönetmek için erişim da gerekir. Vermeyi düşünün [izleme katılımcı](../role-based-access-control/built-in-roles.md#monitoring-contributor) kullanmak üzere eylem grubu içeren bir kaynak grubu için bütçe eşikleri aşıldığında. Ek roller, belirli eylemleri otomatikleştirme, otomasyon ve Azure işlevleri gibi kullanılan belirli hizmetler için gerektirir.
- **Verileri dışarı aktarma maliyet zamanlama** – maliyet Yönetimi Katkıda Bulunanlar, verileri bir depolama hesabına kopyalamak için bir verme zamanlamak için depolama hesaplarını yönetme erişimi de gerekir. Vermeyi düşünün [depolama hesabı Katılımcısı](../role-based-access-control/built-in-roles.md#storage-account-contributor) depolama içeren bir kaynak grubu hesabı nerede maliyet verilerini dışarı aktarılır.
- **Maliyet tasarrufu önerileri görüntüleme** – maliyet Yönetimi okuyucu ve katkıda bulunanlar yoksa erişiminiz önerileri varsayılan olarak. Öneriler erişim kaynakların okuma erişimi gerektirir. Vermeyi düşünün [okuyucu](../role-based-access-control/built-in-roles.md#reader) veya [hizmete özgü rol](../role-based-access-control/built-in-roles.md#built-in-role-descriptions).

## <a name="enterprise-agreement-scopes"></a>Kurumsal Anlaşma kapsamları

Kurumsal Anlaşma (EA) faturalandırma hesapları kayıtlarına olarak da bilinir, aşağıdaki kapsamlar vardır:

- [**Fatura hesabı** ](../billing/billing-view-all-accounts.md) -bir EA kaydına temsil eder. Faturalar bu kapsamda oluşturulur. Market ve ayırmalar gibi kullanım tabanlı, bulunmayan bir satın alma işlemleri, yalnızca bu kapsamda kullanılabilir. Bunlar, Departmanlar veya kayıt hesapları temsil değildir.

    Kaynak türü: `Microsoft.Billing/billingAccounts (accountType = Enrollment)`
- **Departman** : isteğe bağlı kayıt hesaplarını gruplandırma.

    Kaynak türü: `Billing/billingAccounts/departments`

- **Kayıt hesabı** -tek bir hesap sahibi temsil eder. Birden çok kişilere erişim izni verme desteklemiyor.

    Kaynak türü: `Microsoft.Billing/billingAccounts/enrollmentAccounts`

İdare kapsamları tek bir dizine bağlı olsa da, Kurumsal Anlaşma fatura kapsamları değildir. Bir kurumsal Anlaşma fatura hesap, Azure AD dizini herhangi bir sayıda arasında aboneliğiniz olabilir.

Kurumsal Anlaşma fatura kapsamları aşağıdaki rollerini destekler:

- **Kuruluş Yöneticisi** – fatura hesap ayarlarına ve erişimi yönetebilir, tüm maliyetleri görüntüleyebilirsiniz ve maliyet yapılandırmasını yönetebilirsiniz. Örneğin, bütçesinin ve aktarır. İşlevinde, kapsam faturalama EA aynıdır [maliyet Yönetimi katkıda bulunan Azure RBAC rolü](../role-based-access-control/built-in-roles.md#cost-management-contributor).
- **Kurumsal salt okunur kullanıcı** – maliyet yapılandırma fatura hesap ayarları ve maliyet verilerini görüntüleyebilirsiniz. Örneğin, bütçesinin ve aktarır. İşlevinde, kapsam faturalama EA aynıdır [maliyet Yönetimi okuyucu Azure RBAC rolü](../role-based-access-control/built-in-roles.md#cost-management-reader).
- **Bölüm Yöneticisi** – maliyet merkezi gibi departmanı ayarlarını yönetmek ve erişim, tüm maliyetler görüntüleyebilir ve maliyet yapılandırmasını yönetin. Örneğin, bütçesinin ve aktarır.  **DA ücretleri görüntüle** hesap ayarını faturalama bölüm Yöneticiler ve salt okunur kullanıcılar için maliyetleri görmek için etkinleştirilmelidir. Varsa **DA ücretleri görüntüle** olan bir hesap veya aboneliğe sahip olsalar bile devre dışı, departman kullanıcılar herhangi bir düzeyde maliyetleri göremez.
- **Departman salt okunur kullanıcı** – bölüm ayarları, maliyet verilerini ve maliyet yapılandırma görüntüleyebilirsiniz. Örneğin, bütçesinin ve aktarır. Varsa **DA ücretleri görüntüle** olan bir hesap veya aboneliğe sahip olsalar bile devre dışı, departman kullanıcılar herhangi bir düzeyde maliyetleri göremez.
- **Hesap sahibi** – kayıt hesabı ayarları (örneğin, maliyet merkezi) yönetme, tüm maliyetler görüntüleyebilir ve kayıt hesabı için maliyet yapılandırmayı (örneğin, bütçe ve dışarı aktarma gibi) yönetme. **AO ücretleri görüntüle** hesap ayarını fatura hesap sahipleri ve RBAC kullanıcılar için maliyetleri görmek için etkinleştirilmelidir.

Kurumsal Anlaşma fatura hesap kullanıcıları doğrudan faturalar için erişiminiz yok. Faturalar sistem lisanslama bir dış biriminden kullanılabilir.

Azure Abonelikleri, kayıt hesapları altında iç içe geçirilmiştir. Faturalandırma, Abonelikleriniz ve ilgili kendi kapsamları olan kaynak grupları için maliyet verilerine erişim kullanıcınız. Bunlar, görmek veya Azure portalında kaynakları yönetmek için erişimi yoktur. Faturalandırma kullanıcılar giderek maliyetleri görüntüleyebilirsiniz **maliyet Yönetimi + faturalandırma** Hizmetleri Azure portal listesinde. Ardından, maliyetleri belirli abonelikler ve kaynak grupları hakkında rapor oluşturmak için ihtiyaç duydukları filtre uygulayabilirsiniz.

Belirli bir fatura hesap altında açıkça kalan yoktur çünkü faturalandırma kullanıcıları yönetim gruplarına erişiminiz yok. Erişim yönetim gruplarına açıkça verilmelidir. Yönetim, tüm iç içe aboneliklerden döküm maliyetleri gruplandırır. Ancak, yalnızca kullanım tabanlı satın alma işlemleri içerirler. Bunlar gibi ayırmaları ve üçüncü taraf Market teklifleri satın alma işlemleri içermez. Bu maliyetler görüntülemek için EA Faturalama hesabı kullanın.

## <a name="individual-agreement-pay-as-you-go-scopes"></a>Tek tek anlaşma (Kullandıkça Öde) kapsamları

Kullandıkça Öde (PAYG) Abonelikleri, ilgili türleri dahil olmak üzere ücretsiz deneme ister ve geliştirme/test teklifleri, açık bir faturalama hesabı kapsam yok. Bunun yerine, her abonelik hesap sahibi veya EA hesap sahibi gibi hesap yöneticisi vardır.

- [**Fatura hesabı** ](../billing/billing-view-all-accounts.md) -tek bir hesap sahibi için bir veya daha fazla Azure aboneliği temsil eder. Birden çok kişi ya da toplam maliyeti görünümlere erişim için erişim verme şu anda desteklemiyor.

    Kaynak türü: Uygulanamaz

PAYG abonelik hesabı yöneticileri görüntülemek ve fatura ve ödemeleri, gibi fatura verilerini yönetmenize [Azure hesap Merkezi](https://account.azure.com/subscriptions). Ancak, bunlar maliyet verilerini görüntüleyemez veya Azure portalında kaynakları yönetin. Hesap Yöneticisi erişim vermek için daha önce bahsedilen maliyet yönetim rollerini kullanın.

EA, Azure portalında faturalarını PAYG abonelik hesabı yöneticileri görebilirsiniz. Maliyet Yönetimi okuyucu ve maliyet Yönetimi katkıda bulunan rollerinin faturalar erişim sağlamıyorsa aklınızda bulundurun. Daha fazla bilgi için [PAYG faturalar için erişimi nasıl](../billing/billing-manage-access.md#give-access-to-billing).

## <a name="customer-agreement-scopes"></a>Müşteri sözleşmesi kapsamları

Microsoft Müşteri sözleşmesi fatura hesapları aşağıdaki kapsamlar vardır:

- **Fatura hesabı** -birden çok Microsoft ürünleri ve Hizmetleri için bir müşteri sözleşmesi temsil eder. Müşteri sözleşmesi faturalama hesaplarının işlevsel olarak EA kaydınız ile aynı değildir. EA kaydınız daha yakından fatura profillere hizalanır.

    Kaynak türü: `Microsoft.Billing/billingAccounts (accountType = Organization)`

- **Faturalandırma profili** -bir faturaya dahil abonelikleri tanımlar. Faturalandırma profilleri, faturalar, oluşturulan kapsamı olduğundan bir EA kaydına işlevsel karşılığıdır. Benzer şekilde, kullanım tabanlı (Market ve ayırmaları gibi) değil satın alma işlemleri yalnızca bu kapsamda kullanılabilir. Bunlar, fatura bölümlerde dahil edilmez.

    Kaynak türü: `Microsoft.Billing/billingAccounts/billingProfiles`

- **Fatura bölüm** -aboneliğinin bir fatura veya ödeme profilinde bir grubu temsil eder. Fatura bölümleridir Departmanlar gibi — birden çok kişinin bir fatura bölümüne erişebilir.

    Kaynak türü: `Microsoft.Billing/billingAccounts/invoiceSections`

EA kapsamlar, müşteri hesaplarını faturalama sözleşmesi faturalama _olan_ tek bir dizine bağlı ve abonelikler arasında birden çok Azure AD dizini sahip olamaz.

Müşteri anlaşma fatura kapsamları aşağıdaki rolleri destekler:

- **Sahibi** – fatura ayarları ve erişimi yönetin, tüm maliyetler görüntüleyebilir ve maliyet yapılandırmasını yönetin. Örneğin, bütçesinin ve aktarır. İşlevinde, kapsam faturalama bu müşteri sözleşmesi aynıdır [maliyet Yönetimi katkıda bulunan Azure RBAC rolü](../role-based-access-control/built-in-roles.md#cost-management-contributor).
- **Katkıda bulunan** -erişim dışında fatura ayarlarını yönetme, tüm maliyetler görüntüleyebilir ve maliyet yapılandırmasını yönetin. Örneğin, bütçesinin ve aktarır. İşlevinde, kapsam faturalama bu müşteri sözleşmesi aynıdır [maliyet Yönetimi katkıda bulunan Azure RBAC rolü](../role-based-access-control/built-in-roles.md#cost-management-contributor).
- **Okuyucu** – fatura ayarları, maliyet verilerini ve maliyet yapılandırma görüntüleyebilirsiniz. Örneğin, bütçesinin ve aktarır. İşlevinde, kapsam faturalama bu müşteri sözleşmesi aynıdır [maliyet Yönetimi okuyucu Azure RBAC rolü](../role-based-access-control/built-in-roles.md#cost-management-reader).
- **Fatura Yöneticisi** – görüntüleyebilir ve fatura ödemek ve için verileri ve yapılandırma maliyet görüntüleme. Örneğin, bütçesinin ve aktarır. İşlevinde, kapsam faturalama bu müşteri sözleşmesi aynıdır [maliyet Yönetimi okuyucu Azure RBAC rolü](../role-based-access-control/built-in-roles.md#cost-management-reader).
- **Azure aboneliği Oluşturucusu** – Azure abonelikleri oluşturabilir, görüntüleyebilir maliyetleri ve maliyet yapılandırmasını yönetin. Örneğin, bütçesinin ve aktarır. İşlevinde, bu müşteri sözleşmesi fatura kapsamı EA kayıt hesabı sahip rolü ile aynıdır.

Azure abonelikleri EA kayıt hesaplarla nasıl oldukları gibi fatura bölümler altında iç içe geçirilmiştir. Faturalandırma, Abonelikleriniz ve ilgili kendi kapsamları misiniz kaynak gruplarınız için maliyet verilerine erişim kullanıcınız. Ancak, bunlar görmek veya Azure portalında kaynakları yönetmek için erişimi yoktur. Faturalandırma kullanıcılar giderek maliyetleri görüntüleyebilirsiniz **maliyet Yönetimi + faturalandırma** Hizmetleri Azure portal listesinde. Ardından, maliyetleri belirli abonelikler ve kaynak grupları hakkında rapor oluşturmak için ihtiyaç duydukları filtreleyin.

Faturalama hesap altında açıkça kalan yoktur çünkü faturalandırma kullanıcıları yönetim gruplarına erişiminiz yok. Ancak, Yönetim grupları kuruluş için etkinleştirildiğinde, tüm abonelik maliyetleri fatura hesap ve kök yönetim grubuna hem de tek bir dizin için sınırlı olduğu için aktarılmış. Yönetim grupları yalnızca kullanım tabanlı satın alma işlemleri içerir. Satın alma işlemleri ayırmaları ve üçüncü taraf Market teklifleri gibi yönetim gruplarında dahil edilmez. Bu nedenle, fatura hesap ve kök yönetim grubu farklı toplamları bildirebilir. Bu maliyetler görüntülemek için fatura hesap veya ilgili faturalandırma profili kullanın.

## <a name="cloud-solution-provider-csp-scopes"></a>Bulut çözümü sağlayıcısı (CSP) kapsamları

Bulut çözümü sağlayıcısı (CSP) iş ortakları maliyet Yönetimi'nde bugün desteklenmez. Bunun yerine kullanabileceğiniz [iş ortağı Merkezi](https://docs.microsoft.com/azure/cloud-solution-provider/overview/partner-center-overview).

## <a name="switch-between-scopes-in-cost-management"></a>Maliyet Yönetimi'nde kapsamları arasında geçiş yapma

Azure portalında tüm maliyet Yönetimi görünümlerini içeren bir **kapsam** zehirli görünümü sol üst. Hızlı bir şekilde kapsamını değiştirmek için kullanın. Tıklayın **kapsam** zehirli Kapsam Seçici'yi açın. Bu, fatura hesapları, kök yönetim grubu ve kök yönetim grubu altında iç içe olmayan tüm abonelikleri gösterir. Bir kapsam seçin için arka plan vurgulayın ve ardından'ı **seçin** altındaki. Ayrıntıya bir Abonelikteki kaynak grupları gibi iç içe geçmiş kapsamlar için açma kapsam adı bağlantısına tıklayın. Tüm iç içe geçme düzeyi üst kapsamda seçmek için tıklatın **seçin &lt;kapsam&gt;**  üst kapsam Seçici.

## <a name="identify-the-resource-id-for-a-scope"></a>Kaynak kimliği için bir kapsam tanımlama

Maliyet Yönetimi API'leri ile çalışırken, bilmenin verdiği kapsamı önemlidir. Maliyet Yönetimi API'leri için uygun kapsamı URI oluşturmak için aşağıdaki bilgileri kullanın.

### <a name="billing-accounts"></a>Faturalandırma hesapları

1. Azure portalını açın ve gidin **maliyet Yönetimi + faturalandırma** Hizmetler listesinde.
2. Seçin **özellikleri** Faturalama hesabı menüsünde.
3. Fatura hesabı kimliği kopyalayın.
4. Kapsamınızı şöyledir: `"/providers/Microsoft.Billing/billingAccounts/{billingAccountId}"`

### <a name="billing-profiles"></a>Faturalama profilleri

1. Azure portalını açın ve gidin **maliyet Yönetimi + faturalandırma** Hizmetler listesinde.
2. Seçin **faturalandırma profilleri** Faturalama hesabı menüsünde.
3. İstenen fatura profil adına tıklayın.
4. Seçin **özellikleri** fatura profil menüsünde.
5. Fatura hesap ve faturalandırma profili kimlikleri kopyalayın.
6. Kapsamınızı şöyledir: `"/providers/Microsoft.Billing/billingAccounts/{billingAccountId}/billingProfiles/{billingProfileId}"`

### <a name="invoice-sections"></a>Fatura bölümleri

1. Azure portalını açın ve gidin **maliyet Yönetimi + faturalandırma** Hizmetler listesinde.
2. Seçin **fatura bölümleri** Faturalama hesabı menüsünde.
3. İstenen fatura bölümünün adına tıklayın.
4. Seçin **özellikleri** fatura bölüm menüsünde.
5. Fatura hesabı kopyalayın ve fatura bölüm kimlikleri.
6. Kapsamınızı şöyledir: `"/providers/Microsoft.Billing/billingAccounts/{billingAccountId}/invoiceSections/{invoiceSectionId}"`

### <a name="ea-departments"></a>EA Departmanlar

1. Azure portalını açın ve gidin **maliyet Yönetimi + faturalandırma** Hizmetler listesinde.
2. Seçin **Departmanlar** Faturalama hesabı menüsünde.
3. İstenen bölüm adına tıklayın.
4. Seçin **özellikleri** departmanı menüsünde.
5. Fatura hesabı ve bölüm kimlikleri kopyalayın.
6. Kapsamınızı şöyledir: `"/providers/Microsoft.Billing/billingAccounts/{billingAccountId}/departments/{departmentId}"`

### <a name="ea-enrollment-account"></a>EA kayıt hesabı

1. Azure portalını açın ve gidin **maliyet Yönetimi + faturalandırma** Hizmetler listesinde.
2. Seçin **kayıt hesapları** Faturalama hesabı menüsünde.
3. İstenen kayıt hesabı adına tıklayın.
4. Seçin **özellikleri** kayıt hesabı menüsünde.
5. Kayıt hesabı kimlikleri ve fatura hesap kopyalayın.
6. Kapsamınızı şöyledir: `"/providers/Microsoft.Billing/billingAccounts/{billingAccountId}/enrollmentAccounts/{enrollmentAccountId}"`

### <a name="management-group"></a>Yönetim grubu

1. Azure portalını açın ve gidin **Yönetim grupları** Hizmetler listesinde.
2. İstenen yönetim grubuna gidin.
3. Yönetim grubu Tanıtıcısı tablodan kopyalayın.
4. Kapsamınızı şöyledir: `"/providers/Microsoft.Management/managementGroups/{id}"`

### <a name="subscription"></a>Abonelik

1. Azure portalını açın ve gidin **abonelikleri** Hizmetler listesinde.
2. Abonelik kimliği, tablodan kopyalayın.
3. Kapsamınızı şöyledir: `"/subscriptions/{id}"`

### <a name="resource-groups"></a>Kaynak grupları

1. Azure portalını açın ve gidin **kaynak grupları** Hizmetler listesinde.
2. İstenen kaynak grubunun adına tıklayın.
3. Seçin **özellikleri** ve kaynak grubu menüsünde.
4. Kaynak Kimliği alan değerini kopyalayın.
5. Kapsamınızı şöyledir: `"/subscriptions/{id}/resourceGroups/{name}"`

Maliyet Yönetimi desteklenen şu anda [Azure genel](https://management.azure.com) ve [Azure kamu](https://management.usgovcloudapi.net). Azure kamu hakkında daha fazla bilgi için bkz: [Azure genel ve kamu API uç noktaları](../azure-government/documentation-government-developer-guide.md#endpoint-mapping)_._

## <a name="next-steps"></a>Sonraki adımlar

- Maliyet yönetimi için zaten ilk hızlı tamamlamadıysanız, hem okuma [maliyetleri başlamanızı](quick-acm-cost-analysis.md).
