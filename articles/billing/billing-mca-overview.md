---
title: Microsoft Müşteri anlaşması fatura hesabınıza - Azure kullanmaya başlayın
description: Microsoft müşteri hesabı faturalama sözleşmenizi anlama
author: bandersmsft
manager: amberbhargava
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/01/2019
ms.author: banders
ms.openlocfilehash: 87483c967641489e9548f38c99eebbf121d0d252
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67490743"
---
# <a name="get-started-with-your-microsoft-customer-agreement-billing-account"></a>Microsoft Müşteri anlaşması fatura hesabınıza ile çalışmaya başlama

Azure'u kullanmak kaydolduğunuzda, bir faturalama hesabı oluşturulur. Maliyetleri izleyin ve fatura hesabınıza ödemeler, faturaları yönetmek için kullanın. Faturalandırma birden çok hesaba erişim sağlayabilirsiniz. Örneğin, Azure için kişisel projeleriniz için kaydolmanız. Ayrıca, kuruluşunuzun Kurumsal Anlaşma veya Microsoft Müşteri sözleşmesi aracılığıyla azure'a erişimi olabilir. Bu senaryoların her biri için ayrı bir fatura hesap gerekir.

Bu makale, bir faturalama hesabı için bir Microsoft Müşteri sözleşmesi için geçerlidir. [Microsoft Müşteri sözleşmesi erişimi olup olmadığını denetlemek](#check-access-to-a-microsoft-customer-agreement).

## <a name="your-billing-account"></a>Fatura hesabınıza

Microsoft Müşteri sözleşmesi için fatura hesabınıza faturaları ve ödeme yöntemlerini yönetmenize olanak sağlayan bir veya daha fazla fatura profillerini içerir. Her fatura profili fatura profilin fatura maliyetlerini düzenlemenize olanak sağlayan bir veya daha fazla fatura bölümleri içerir.

Aşağıdaki diyagramda, bir faturalama hesabı, fatura profilleri ve fatura bölümleri arasındaki ilişkiyi gösterir.

![Microsoft Müşteri sözleşmesi hiyerarşi faturalama gösteren diyagram](./media/billing-mca-overview/mca-billing-hierarchy.png)

Fatura hesabındaki rolleri, yüksek düzeyde izinlere sahiptir. Varsayılan olarak, yalnızca Azure alır hesabına erişim için fatura kaydolan kullanıcı. Bu rolleri, faturaları görüntülemek ve Finans veya BT yöneticileri gibi kuruluşunuzun tamamı için maliyetleri izlemek için gereken kullanıcılara atanmalıdır. Daha fazla bilgi için [Faturalama hesabı rolleri ve görevleri](billing-understand-mca-roles.md#billing-account-roles-and-tasks).

## <a name="billing-profiles"></a>Faturalandırma profilleri

Fatura ve ödeme yöntemlerinizi yönetme için bir faturalandırma profili kullanın. Bir aylık Fatura Hesabınızdaki her bir faturalama profili için ayın başında oluşturulur. Fatura ilgili tüm Azure abonelikleri ve diğer satın alma önceki ayın ücretini içerir.

Faturalandırma profili için fatura hesabınıza otomatik olarak oluşturulur. Bu, varsayılan olarak tek bir fatura bölüm içerir. Kolayca izleyin ve maliyetleri gereksinimlerinize göre düzenlemek için ek bölümler oluşturabilir proje, bölüme veya geliştirme ortamında olup. Bu bölümler, her bir abonelik ve satın alma işlemleri, kendisine atadığınız kullanımını yansıtan fatura profilin faturasında görürsünüz.

Fatura profilleri rollerinde faturaları ve ödeme yöntemlerini görüntülemek ve yönetmek için izinlere sahip. Bu roller, faturalar gibi muhasebe ekibi üyelerinin, kuruluşunuzda ödeme kullanıcılar atayın. Daha fazla bilgi için [faturalama profili rolleri ve görevleri](billing-understand-mca-roles.md#billing-profile-roles-and-tasks).

### <a name="each-billing-profile-gets-a-monthly-invoice"></a>Bir aylık fatura her fatura profilini alır

Her fatura profili için ayın başında aylık fatura oluşturulur. Faturanın önceki aydan itibaren tüm ücretleri içerir.

Faturayı görüntülemek, belgeleri ve e-posta, Azure portalında, gelecek fatura almak için ayarını değiştirin. Daha fazla bilgi için [bir Microsoft Müşteri sözleşmesi faturaları indirmesine](billing-download-azure-invoice-daily-usage-date.md#download-invoices-for-a-microsoft-customer-agreement).

### <a name="invoice-payment-methods"></a>Fatura ödeme yöntemleri

Her fatura profili, fatura ödemek için kullanılan, kendi ödeme yöntemleri vardır. Aşağıdaki ödeme yöntemleri desteklenir:

| Tür             | Tanım  |
|------------------|-------------|
|Azure KREDİLERİ    |  KREDİLERİ uygun ücretleri ödemesi gereken kod miktarını azaltır faturanızla ilgili otomatik olarak uygulanır. Daha fazla bilgi için [fatura profiliniz için Azure kredi bakiyesi izlemek](billing-mca-check-azure-credits-balance.md). |
|Onay/aktarım kablo | Hesabınız için onay/havale ile ödeme onaylanırsa. Miktar için ödeme yapabilir onay/havale aracılığıyla faturası son. Fatura ödeme yönergeleri verilir. |
|Kredi kartı | Müşteriler, Azure Web sitesi üzerinden Azure'a kaydolun, kredi kartı ile ödeme yapabilirsiniz. |

### <a name="apply-policies-to-control-purchases"></a>Satın alma işlemleri denetlemek için ilkeler uygulayın

İlkeler, Denetim Azure Market ve faturalandırma profili kullanarak rezervasyon satın alma işlemleri için geçerlidir. Azure ayırmaları ve Market ürünleri satın devre dışı bırakmak için ilkeler ayarlayabilir. İlkeleri uygulandığında, bu satın alma işlemleri yapmak için faturalandırma profili faturalandırılır abonelikleri kullanılamaz.

### <a name="azure-plans-determine-pricing-and-service-level-agreement-for-subscriptions"></a>Azure planları, abonelikler için fiyatlandırma ve hizmet düzeyi sözleşmesi belirleme

Azure planları fiyatlandırma belirlemek ve hizmet düzeyi sözleşmeleri Azure abonelikleri için. Faturalandırma profili oluşturduğunuzda otomatik olarak etkinleştirilir. Faturalandırma profili ile ilişkili tüm fatura bölümleri, bu planları kullanabilirsiniz. Fatura bölümüne erişimi olan kullanıcılar, Azure abonelikleri oluşturabilmesi için planlar kullanın. Aşağıdaki Azure planları, Microsoft Müşteri sözleşmesi için hesapları faturalama desteklenir:

| Planlama             | Tanım  |
|------------------|-------------|
|Microsoft Azure-planı   | Kullanıcıların herhangi bir iş yükünü çalıştırmak üzere abonelik oluşturmasına izin verin. Daha fazla bilgi için [Microsoft Azure planlama](https://azure.microsoft.com/offers/ms-azr-0017g/) |
|Microsoft Azure geliştirme ve Test planlama | Geliştirme için kısıtlı abonelikler oluşturmak Visual Studio aboneleri izin verin veya test iş yükleri. Bu abonelik, Azure portalında daha düşük fiyatlar ve özel sanal makine görüntülerine erişim gibi avantajlardan yararlanabilirsiniz. Daha fazla bilgi için [Microsoft Azure geliştirme ve test için planlama](https://azure.microsoft.com/offers/ms-azr-0148g/)|

## <a name="invoice-sections"></a>Fatura bölümleri

Faturanızla ilgili maliyetleri düzenlemek için fatura bölümler oluşturun. Örneğin, departman, takım veya proje maliyetlerini düzenlemek istiyorsanız ancak kuruluşunuz için tek bir fatura gerekebilir. Bu senaryoda, her bölüm, takım veya proje için fatura bölümü oluşturduğunuz tek bir fatura profili sahip.

Bir fatura bölümü oluşturulduğunda, diğerleri bölümüne faturalandırılır Azure abonelikleri oluşturabilmesi için izin verebilirsiniz. Ardından tüm kullanım ücretleri ve abonelik satın alma işlemleri bölümüne faturalandırılır.

Fatura bölümünde rolleri Azure abonelikleri oluşturan Denetim izinlerine sahip. Bu roller, takımların mühendislik müşteri adayları ve teknik mimarlar gibi kuruluşumuz için Azure ortamı ayarlama kullanıcılar atayın. Daha fazla bilgi için [fatura bölüm rolleri ve görevleri](billing-understand-mca-roles.md#invoice-section-roles-and-tasks).

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Microsoft Müşteri sözleşmesi için erişim denetimi
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun.

Yardıma ihtiyacınız varsa [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için.

## <a name="next-steps"></a>Sonraki adımlar

Fatura hesabınıza hakkında bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure'da Microsoft Müşteri sözleşmesi yönetici rollerini anlama](billing-understand-mca-roles.md)
- [Microsoft Müşteri sözleşmesi için ek bir Azure aboneliği oluşturun](billing-mca-create-subscription.md)
- [Maliyetlerinizi düzenlemek için fatura bölümler oluşturma](billing-mca-section-invoice.md)
