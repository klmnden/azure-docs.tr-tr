---
title: Faturalandırma sahipliğini Azure aboneliği edinin
description: Diğer kullanıcıların Azure abonelik faturalandırma sahipliğini istek öğrenin.
author: amberbhargava
manager: amberb
editor: banders
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/01/2019
ms.author: banders
ms.openlocfilehash: 4c9a8c866a157757121e6a9dd07a0a8559937c5e
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67490890"
---
# <a name="get-billing-ownership-of-azure-subscriptions-from-other-accounts"></a>Faturalandırma sahipliğini diğer hesaplardan gelen Azure aboneliği edinin

Mevcut fatura sahibi kuruluşunuz ayrılıyor ya da fatura hesabınıza aboneliği için ödeme yapmak istediğiniz Azure aboneliği sahipliği devralmanız isteyebilirsiniz. Sahipliği alma aboneliğin faturalama sorumlulukları hesabınıza aktarır.

Bu makale, bir faturalama hesabı için bir Microsoft Müşteri sözleşmesi için geçerlidir. [Microsoft Müşteri sözleşmesi erişimi olup olmadığını denetlemek](#check-for-access).

Faturalandırma sahipliğini istemek için olmanız bir **fatura bölümüne sahip** veya **fatura bölümüne katkıda bulunan**. Daha fazla bilgi için bkz. [fatura bölüm rolleri görevleri](billing-understand-mca-roles.md#invoice-section-roles-and-tasks).

## <a name="request-billing-ownership"></a>Faturalandırma sahipliğini iste

1. Oturum [Azure portalında](http://portal.azure.com) bir fatura bölüm sahibi veya katkıda bulunan için Microsoft Müşteri sözleşmesi için bir faturalama hesabı olarak.

2. Arama **maliyet Yönetimi + faturalandırma**.

   ![Maliyet Yönetimi + faturalandırma için Azure portalı arama gösteren ekran görüntüsü](./media/billing-mca-request-billing-ownership/billing-search-cost-management-billing.png)

3. Seçin **fatura bölümleri** sol taraftaki. Erişiminizi bağlı olarak, bir faturalama hesabı veya faturalandırma profili seçmeniz gerekebilir. Fatura hesabı veya profili makinesinden seçin **fatura bölümleri**.
   
   ![Seçme gösteren ekran görüntüsü fatura bölümleri](./media/billing-mca-request-billing-ownership/mca-select-invoice-sections.png)        

4. Bir fatura bölümü listeden seçin. Aboneliğinin sahipliği devralmanız sonra bu fatura bölümüne faturalandırılır.

5. Seçin **aktarım istekleri** seçin ve sol alt tarafında **Ekle**.
 
   ![Aktarım İsteği seçerek gösteren ekran görüntüsü](./media/billing-mca-request-billing-ownership/mca-select-transfer-requests.png)

6. Faturalandırma sahipliğini gelen istediği kullanıcının e-posta adresi girin. Kullanıcı hesabı yönetici bir Microsoft çevrimiçi hizmet programı Faturalama hesabı veya Kurumsal Anlaşma bir hesap sahibi olması gerekir. Daha fazla bilgi için [fatura hesaplarınızı Azure portalında görüntüleme](billing-view-all-accounts.md). Seçin **gönderme aktarım isteği**.

   ![Aktarım isteği gönderme gösteren ekran görüntüsü](./media/billing-mca-request-billing-ownership/mca-send-transfer-requests.png)

7. Kullanıcı, aktarım isteğini gözden geçirmek için yönergeler içeren bir e-posta alır.

   ![Bu programları gözden geçirme aktarım isteği eposta ekran görüntüsü](./media/billing-mca-request-billing-ownership/mca-review-transfer-request-email.png)

8. Aktarım İsteği onaylamak için kullanıcı e-postada bağlantısını seçer ve yönergeleri izler.

    ![Bu programları gözden geçirme aktarım isteği eposta ekran görüntüsü](./media/billing-mca-request-billing-ownership/mca-review-transfer-request.png)

## <a name="check-the-transfer-request-status"></a>Aktarım İsteği durumunu denetle

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Arama **maliyet Yönetimi + faturalandırma**.

   ![Maliyet Yönetimi + faturalandırma için Azure portalı arama gösteren ekran görüntüsü](./media/billing-mca-request-billing-ownership/billing-search-cost-management-billing.png)


3. Seçin **fatura bölümleri** sol taraftaki. Erişiminizi bağlı olarak, bir faturalama hesabı veya faturalandırma profili seçmeniz gerekebilir. Fatura hesabı veya profili makinesinden seçin **fatura bölümleri**.
   
   ![Seçme gösteren ekran görüntüsü fatura bölümleri](./media/billing-mca-request-billing-ownership/mca-select-invoice-sections.png)        

4. Fatura bölümü aktarım isteği gönderilen listeden seçin.

5. Seçin **aktarım istekleri** sol alt taraftaki. Aktarım İsteği sayfasında aşağıdaki bilgileri görüntüler:

    ![Aktarım İsteği listesini gösteren ekran görüntüsü](./media/billing-mca-request-billing-ownership/mca-select-transfer-requests-for-status.png)

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

7. Ayrıntılarını görüntülemek için bir aktarım isteği seçin. Aktarım Ayrıntıları sayfasında aşağıdaki bilgileri görüntüler:
   
   ![Aktarılan Aboneliklerin listesini gösteren ekran görüntüsü](./media/billing-mca-request-billing-ownership/mca-transfer-completed.png)
    
   |Sütun  |Tanım|
   |---------|---------|
   |Aktarım İsteği kimliği|Aktarım isteğinizin benzersiz kimliği. Bir destek isteği gönderirseniz kimliği destek talebinizi hızlandırmak için Azure destek ekibi ile paylaşabilirsiniz.|
   |Aktarım istenme tarihi|Aktarım isteği zaman gönderildiği tarih|
   |Tarafından istenen Aktarım|Aktarım İsteği gönderen kullanıcının e-posta adresi|
   |Aktarım isteğinin son kullanma tarihi| Aktarım isteğinin süresinin dolduğu tarih|
   |Alıcının e-posta adresi|Faturalandırma sahipliğini aktarma isteği gönderildi kullanıcının e-posta adresi|
   |Alıcılara gönderilen Aktarım bağlantısı|Aktarım İsteği gözden geçirmek için kullanıcı için gönderilen URL'yi|

## <a name="supported-subscription-types"></a>Desteklenen abonelik türleri

Fatura sahipliğini abonelik türleri aşağıda listelenen isteyebilir.

- [Eylem paketi](https://azure.microsoft.com/offers/ms-azr-0025p/)\* 
- [Açık lisansta azure](https://azure.microsoft.com/offers/ms-azr-0111p/)\*
- [Azure Pass Sponsorluğu](https://azure.microsoft.com/offers/azure-pass/)\*
- [Kurumsal geliştirme ve Test](https://azure.microsoft.com/offers/ms-azr-0148p/)
- [Ücretsiz deneme](https://azure.microsoft.com/offers/ms-azr-0044p/)\*
- [Kullandıkça Öde](https://azure.microsoft.com/offers/ms-azr-0003p/) 
- [Kullandıkça Öde geliştirme ve Test](https://azure.microsoft.com/offers/ms-azr-0023p/)
- [Microsoft Azure-planı](https://azure.microsoft.com/offers/ms-azr-0017g/)\*\*
- [Microsoft Azure sponsorluk teklifi](https://azure.microsoft.com/offers/ms-azr-0036p/)\*
- [Microsoft Kurumsal Anlaşma](https://azure.microsoft.com/pricing/enterprise-agreement/)
- [Microsoft iş ortağı ağı](https://azure.microsoft.com/offers/ms-azr-0025p/)\*
- [MSDN platformları](https://azure.microsoft.com/offers/ms-azr-0062p/)\*
- [Visual Studio Enterprise (BizSpark) aboneleri](https://azure.microsoft.com/offers/ms-azr-0064p/)\*
- [Visual Studio Enterprise (MPN) aboneleri](https://azure.microsoft.com/offers/ms-azr-0029p/)\*
- [Visual Studio Enterprise aboneleri](https://azure.microsoft.com/offers/ms-azr-0063p/)\*
- [Visual Studio Professional](https://azure.microsoft.com/offers/ms-azr-0059p/)\*
- [Visual Studio Test Professional aboneleri](https://azure.microsoft.com/offers/ms-azr-0060p/)\*

\* Abonelikte mevcut olan herhangi bir kredi aktarma yeni hesap kullanılamaz.

\*\* Yalnızca abonelikleri sırasında oluşturulan hesaplarında desteklenen Azure Web sitesinde kaydolun.


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

Mevcut kullanıcıları, grupları veya hizmet sorumluları için (Azure RBAC (rol tabanlı erişim denetimi)) kullanılarak atanmış erişim [… /Role-based-Access-Control/Overview.MD], geçiş sırasında etkilenmez.

### <a name="azure-support-plan"></a>Azure destek planınızı

Azure destek abonelikleri ile aktarmaz. Kullanıcı tüm Azure abonelikleri aktarırsa, kendi destek planı iptal isteyin.

### <a name="charges-for-transferred-subscription"></a>Aktarılan Abonelik ücretleri

Abonelik faturalama özgün sahibinin, transfer tamamlandıktan noktaya kadar bildirilen herhangi bir ücreti sorumludur. Fatura bölümünüzü, aktarımı ve sonraki sürümlerde zamandan bildirilen ücretleri sorumludur. Aktarım önce gerçekleşen ancak daha sonra bildirilen bazı ücretlerini olabilir. Bu ücretler, fatura bölümünde gösterilir.

### <a name="cancel-a-transfer-request"></a>Aktarım isteğini iptal et

İstek Onaylandı veya reddedildi kadar aktarım isteğini iptal edebilirsiniz. Aktarım İsteği iptal etmek için Git [aktarım Ayrıntıları sayfası](#check-the-transfer-request-status) ve iptal sayfanın altından seçin.

### <a name="software-as-a-service-saas-transfer"></a>Yazılım olarak hizmet (SaaS) aktarım

SaaS ürünlerinde aboneliklerle aktarılmıyor. Kullanıcıya sor [Azure desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) SaaS ürünlerinde faturalandırma sahipliğini aktarmak için. Faturalandırma sahipliğini yanı sıra kullanıcı ayrıca kaynak sahipliğini aktarabilir. Kaynak sahipliği ürün ayrıntılarını görüntüleme ve silme gibi yönetim işlemlerini gerçekleştirmenize olanak tanır. Kullanıcının kaynak sahipliğini devretmek için SaaS üründe bir kaynak sahibi olması gerekir.

## <a name="check-for-access"></a>Erişimi denetle
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun

Yardıma ihtiyacınız varsa [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için.

## <a name="next-steps"></a>Sonraki adımlar

- Azure abonelik faturalandırma sahipliğini, fatura bölümüne aktarılır. İçinde bu aboneliklerinin izlemek [Azure portalında](https://portal.azure.com).
- Diğerleri, görüntüleme ve bu abonelikler için fatura bilgilerini yönetme izinleri verin. Daha fazla bilgi için [fatura bölüm rolleri ve görevleri](billing-understand-mca-roles.md#invoice-section-roles-and-tasks).
