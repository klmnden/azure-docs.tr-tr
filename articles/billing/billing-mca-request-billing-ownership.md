---
title: Azure aboneliği sahipliğini fatura | Microsoft Docs
description: Diğer kullanıcıların Azure abonelik faturalandırma sahipliğini istek öğrenin.
services: ''
documentationcenter: ''
author: amberbhargava
manager: amberb
editor: banders
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/28/2019
ms.author: banders
ms.openlocfilehash: be8c7fcebca224196d9eac7d22387989b1bdfd46
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60371958"
---
# <a name="get-billing-ownership-of-azure-subscriptions-from-other-users"></a>Faturalandırma sahipliğini diğer kullanıcıların Azure aboneliği edinin

Mevcut fatura sahibi kuruluşunuz ayrılıyor ya da fatura hesabınıza aboneliği için ödeme yapmak istediğiniz Azure aboneliği sahipliğini alın isteyebilirsiniz.

Azure aboneliği sahipliğini fatura diğer hesaplarında mevcut sahiplerini gerçekleştirilecek bir istek gönderebilirsiniz. Abonelikler, faturalama sorumlulukları sahipliği alma, fatura bölümüne aktarır.

Faturalandırma sahipliğini istemek için olmanız bir **fatura bölümüne sahip** veya **fatura bölümüne katkıda bulunan**. Daha fazla bilgi için bkz. [fatura bölüm rolleri görevleri](billing-understand-mca-roles.md#invoice-section-roles-and-tasks).

Bu makale, bir faturalama hesabı için bir Microsoft Müşteri sözleşmesi için geçerlidir. [Microsoft Müşteri sözleşmesi erişimi olup olmadığını denetlemek](#check-access-to-a-microsoft-customer-agreement).

## <a name="request-billing-ownership-in-the-azure-portal"></a>Azure portalındaki faturalandırma sahipliğini iste

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Arama **maliyet Yönetimi + faturalandırma**.

   ![Maliyet Yönetimi + faturalandırma için Azure portalı arama gösteren ekran görüntüsü](./media/billing-mca-request-billing-ownership/billing-search-cost-management-billing.png)

3. Fatura bölümüne gidin. Erişiminizi bağlı olarak, bir faturalama hesabı veya faturalandırma profili seçmeniz gerekebilir. Fatura hesabı veya profili makinesinden seçin **fatura bölümleri** ve ardından bir fatura bölümü.
   <!-- Todo - Add a screenshot -->

4. Seçin **aktarım istekleri** sol alt taraftaki.

5. Sayfanın üst kısmında **Ekle**'yi seçin.

6. Faturalandırma sahipliğini gelen istediği kullanıcının e-posta adresi girin. Kullanıcı hesabı yönetici bir Microsoft çevrimiçi hizmet programı Faturalama hesabı veya Kurumsal Anlaşma bir hesap sahibi olması gerekir. Daha fazla bilgi için [fatura hesaplarınızı Azure portalında görüntüleme](billing-view-all-accounts.md).

   ![Yeni bir aktarım isteği ekleme gösteren ekran görüntüsü](./media/billing-mca-request-billing-ownership/mca-new-transfer-request.png)

7. Seçin **gönderme aktarım isteği**.

8. Kullanıcı, aktarım isteğini gözden geçirmek için yönergeler içeren bir e-posta alır.

   ![Bu programları gözden geçirme aktarım isteği eposta ekran görüntüsü](./media/billing-mca-request-billing-ownership/mca-review-transfer-request-email.png)

9. Aktarım İsteği onaylamak için kullanıcı e-postada bağlantısını seçer ve yönergeleri izler.

    ![Bu programları gözden geçirme aktarım isteği eposta ekran görüntüsü](./media/billing-mca-request-billing-ownership/mca-review-transfer-request.png)

## <a name="check-the-status-of-your-transfer-request-in-the-azure-portal"></a>Azure portalında aktarım isteğinizin durumunu denetleyin

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Arama **maliyet Yönetimi + faturalandırma**.

   ![Maliyet Yönetimi + faturalandırma için Azure portalı arama gösteren ekran görüntüsü](./media/billing-mca-request-billing-ownership/billing-search-cost-management-billing.png)

3. Fatura bölümüne gidin. Erişiminizi bağlı olarak, bir faturalama hesabı veya faturalandırma profili seçmeniz gerekebilir. Fatura hesabı veya profili makinesinden seçin **fatura bölümleri** ve ardından bir fatura bölümü.
   <!-- Todo - Add a screenshot -->

4. Seçin **aktarım istekleri** sol alt taraftaki.

5. Aktarım İsteği sayfasında aşağıdaki bilgileri görüntüler:

    ![Aktarım İsteği listesini gösteren ekran görüntüsü](./media/billing-mca-request-billing-ownership/mca-view-transfer-requests.png)

   |Sütun|Tanım|
   |---------|---------|
   |İstek tarihi|Aktarım isteği zaman gönderildiği tarih|
   |Alıcı|Faturalandırma sahipliğini aktarma isteği gönderildi kullanıcının e-posta adresi|
   |Sona erme tarihi|İsteğin süresinin dolduğu tarih|
   |Durum|Aktarım isteğinin durumu|

    Aktarım isteği, aşağıdaki durumlardan biri olabilir:

   |Durum|Tanım|
   |---------|---------|
   |Sürüyor|Kullanıcı, aktarım isteğini kabul edilmemiş|
   |İşleniyor|Kullanıcı, aktarım isteğini onayladı. Kullanıcının seçili abonelikler için faturalama, fatura bölümüne aktarılır|
   |Tamamlandı| Kullanıcının seçili abonelikler için faturalama, fatura bölümüne aktarılır.|
   |Hatalarla tamamlandı|İsteği tamamlandı ancak kullanıcının seçtiği bazı abonelikler için faturalama aktarılan uygulanamadı|
   |Süresi dolmuş|Kullanıcı isteği zamanında kabul etmedi ve ve süresi doldu|
   |İptal edildi|Aktarım İsteği erişimi olan istek iptal edildi|
   |Reddetti|Kullanıcı, aktarım isteğini reddetti.|

6. Ayrıntılarını görüntülemek için bir aktarım isteği seçin. Aktarım Ayrıntıları sayfasında aşağıdaki bilgileri görüntüler:
   <!-- Todo - Add a screenshot -->

   |Sütun  |Tanım|
   |---------|---------|
   |Aktarım İsteği kimliği|Aktarım isteğinizin benzersiz kimliği. Bir destek isteği gönderirseniz kimliği destek talebinizi hızlandırmak için Azure destek ekibi ile paylaşabilirsiniz.|
   |Aktarım istenme tarihi|Aktarım isteği zaman gönderildiği tarih|
   |Tarafından istenen Aktarım|Aktarım İsteği gönderen kullanıcının e-posta adresi|
   |Aktarım isteğinin son kullanma tarihi| Aktarım isteğinin süresinin dolduğu tarih|
   |Alıcının e-posta adresi|Faturalandırma sahipliğini aktarma isteği gönderildi kullanıcının e-posta adresi|
   |Alıcılara gönderilen Aktarım bağlantısı|Aktarım İsteği gözden geçirmek için kullanıcı için gönderilen URL'yi|

## <a name="additional-information"></a>Ek bilgiler

Aşağıdaki bölümde, abonelik aktarma hakkında ek bilgi sağlar.

### <a name="no-service-downtime"></a>Hizmet kapalı kalma süresi olmadan

Abonelikte bulunan Azure Hizmetleri, herhangi bir kesinti olmadan çalışmaya devam. Biz yalnızca aktarmak için kullanıcının seçtiği Azure abonelikleri için fatura ilişkiyi geçiş.

### <a name="disabled-subscriptions"></a>Devre dışı bırakılan abonelikler

Devre dışı bırakılmış abonelik aktarılamaz. Abonelik, fatura sahipliğini devretmek için etkin durumda olması gerekir.

### <a name="azure-resources-transfer"></a>Azure kaynaklarını aktarımı

Abonelikteki tüm kaynaklara, Web siteleri aktarımı, disk ve VM'lerin ister.

### <a name="azure-marketplace-products-transfer"></a>Azure Market ürünleri Aktarım

Azure Market ürünleri birlikte kendi aboneliklerini aktarın.

### <a name="azure-reservations-transfer"></a>Azure rezervasyon aktarımı

Azure ayırmaları abonelikleri ile otomatik olarak taşıma. [Azure Destek ekibiyle iletişime geçin](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ayırmaları taşınır.

### <a name="access-to-azure-services"></a>Azure hizmetlerine erişim

Azure RBAC (rol tabanlı erişim denetimi) ile ayarlanmış olan Azure kaynaklarına erişimi, geçiş sırasında etkilenmez.

### <a name="azure-support-plan"></a>Azure destek planınızı

Azure destek abonelikleri ile aktarmaz. Kullanıcı tüm Azure abonelikleri aktarırsa, kendi destek planı iptal isteyin.

### <a name="charges-for-transferred-subscription"></a>Aktarılan Abonelik ücretleri

Abonelik faturalama özgün sahibinin, transfer tamamlandıktan noktaya kadar bildirilen herhangi bir ücreti sorumludur. Fatura bölümünüzü, aktarımı ve sonraki sürümlerde zamandan bildirilen ücretleri sorumludur. Aktarım önce gerçekleşen ancak daha sonra bildirilen bazı ücretlerini olabilir. Bu ücretler, fatura bölümünde gösterilir.

### <a name="supported-offers"></a>Desteklenen teklifler

Herhangi bir türü veya CSP sunar dışında teklifler, abonelik aktarılabilir.

### <a name="cancel-a-transfer-request"></a>Aktarım isteğini iptal et

İstek Onaylandı veya reddedildi kadar aktarım isteğini iptal edebilirsiniz. Aktarım İsteği iptal etmek için İptal'i seçin ve aktarım Ayrıntıları sayfası için sayfanın alt gidin.

### <a name="software-as-a-service-saas-transfer"></a>Yazılım olarak hizmet (SaaS) aktarım

SaaS ürünlerinde aboneliklerle aktarılmıyor. Kullanıcıya sor [Azure desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) SaaS ürünlerinde faturalandırma sahipliğini aktarmak için. Faturalandırma sahipliğini yanı sıra kullanıcı ayrıca kaynak sahipliğini aktarabilir. Kaynak sahipliği ürün ayrıntılarını görüntüleme ve silme gibi yönetim işlemlerini gerçekleştirmenize olanak tanır. Kullanıcının kaynak sahipliğini devretmek için SaaS üründe bir kaynak sahibi olması gerekir.

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Microsoft Müşteri sözleşmesi için erişim denetimi
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun

Yardıma ihtiyacınız varsa [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için.

## <a name="next-steps"></a>Sonraki adımlar

- Azure abonelik faturalandırma sahipliğini, fatura bölümüne aktarılır. İçinde bu aboneliklerinin izlemek [Azure portalında](https://portal.azure.com).
- Diğerleri, görüntüleme ve bu abonelikler için fatura bilgilerini yönetme izinleri verin. Daha fazla bilgi için [fatura bölüm rolleri ve görevleri](billing-understand-mca-roles.md#invoice-section-roles-and-tasks).
