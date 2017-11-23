---
title: "Azure maliyeti yönetimi için sık sorulan sorular | Microsoft Docs"
description: "Bazı Azure Yönetimi maliyeti ilgili sık sorulan soruların yanıtlarını içerir."
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 11/21/2017
ms.topic: article
ms.service: cost-management
manager: carmonm
ms.custom: 
ms.openlocfilehash: 043aea81258d96fc6598903f9b523f29a5bf2c15
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2017
---
# <a name="frequently-asked-questions-for-azure-cost-management"></a>Azure maliyeti yönetimi için sık sorulan sorular

Bu makalede Azure maliyeti Yönetimi (Cloudyn olarak da bilinir) hakkında bazı sık sorulan soruları giderir. Maliyet yönetimi hakkında sorularınız varsa, onları sorabilirsiniz [SSS Azure maliyeti Cloudyn yönetimine](https://social.msdn.microsoft.com/Forums/en-US/231bf072-2c71-4121-8339-ac9d868137b9/faqs-for-azure-cost-management-by-cloudyn?forum=Cloudyn).

## <a name="how-can-i-resolve-common-indirect-enterprise-setup-problems"></a>Ortak dolaylı Kurumsal kurulum sorunlarını çözmek nasıl?

İlk Cloudyn Portalı'nı kullandığınızda, bir Kurumsal Anlaşma veya Bulut çözümü sağlayıcısı (CSP) kullanıcı iseniz aşağıdaki iletileri görebilirsiniz:

- "Belirtilen API anahtarını bir üst düzey kayıt anahtarı değil" görüntülenen **ayarlamak yukarı Azure Yönetimi maliyeti** Sihirbazı.
- "– Hayır Kurumsal Anlaşma portalında gösterilen doğrudan kayıt".
- "Kullanım verisi yok son 30 gün boyunca bulundu. Biçimlendirme Azure hesabınız için etkin emin olmak için dağıtımcı başvurun"Cloudyn Portalı'nda görüntülenir.

Bir Azure Kurumsal anlaşmasına bir satıcı aracılığıyla ya da CSP satın önceki iletileri gösterir. Satıcınıza veya CSP etkinleştirmek gereken _biçimlendirme_ Azure hesabı için verilerinizi Cloudyn içinde görüntüleyebilmesi için.

Sorunları gidermeye yönelik şöyledir:

1. Satıcınızla etkinleştirmek gereken _biçimlendirme_ hesabınız için. Yönergeler için bkz: [dolaylı müşteri ekleme Kılavuzu](https://ea.azure.com/api/v3Help/v2IndirectCustomerOnboardingGuide).

2. Cloudyn ile kullanmak için Azure Kurumsal anlaşmasına anahtar oluşturun. Yönergeler için bkz: [ekleme bilgisayarınızı Azure EA](https://support.cloudyn.com/hc/en-us/articles/210429585-Adding-Your-AZURE-EA) veya [bilgisayarınızı EA kayıt kimliği bulmak ve API anahtarını](https://youtu.be/u_phLs_udig).

Yalnızca bir Azure hizmeti yönetici maliyet yönetimini etkinleştirebilirsiniz. Ortak yönetici izinleri yeterli değil.

Cloudyn ayarlamak için Azure Enterprise sözleşmesi API anahtarı oluşturmadan önce yönergeleri izleyerek Azure faturalama API etkinleştirmeniz gerekir:

- [Kurumsal müşteriler için raporlama API'leri genel bakış](../billing/billing-enterprise-api.md)
- [Microsoft Azure enterprise portal raporlama API](https://ea.azure.com/helpdocs/reportingAPI) altında **API veri erişimini etkinleştirme**


Departman yöneticilerinin, hesap sahipleri ve kuruluş yöneticileri izinleri vermek gerekebilecek _görüntüleyin ücret_ faturalama API ile.

## <a name="how-do-i-enable-suspended-or-locked-out-users"></a>Askıya alındı veya kilitlenmiş kullanıcıların nasıl etkinleştirebilirim?

Bir kullanıcı için erişime izin vermek için bir istek olan bir uyarı alırsanız, kullanıcı hesabı etkinleştirmeniz gerekir.

Kullanıcı hesabını etkinleştirmek için:

1. Cloudyn için Cloudyn ayarlamak için kullanılan Azure yönetici kullanıcı hesabı kullanarak oturum açın. Veya yönetici erişim verildi bir kullanıcı hesabıyla oturum açın.

2. Sağ üst dişli sembolü seçin ve Seç **kullanıcı yönetimi**.

3. Kullanıcı Bul, Kurşun Kalem simgesini seçin ve ardından kullanıcı düzenleyin.

4. Altında **kullanıcı durumu**, durumu değiştirme **askıya** için **etkin**.

Cloudyn kullanıcı hesapları, çoklu oturum açma azure'dan kullanarak bağlanın. Bir kullanıcı parolasını mistypes, yine Azure erişebilirsiniz olsa da bunlar Cloudyn dışında kilitlenmesine neden.

Azure varsayılan adresinden e-posta adresinizi Cloudyn değiştirirseniz, hesabınızın kilitli. "Durum initiallySuspended." gösterebilir Kullanıcı hesabınız kilitlendi, hesabınızı sıfırlamak için alternatif bir yöneticiye başvurun.

Hesaplardan birini kilitli durumda en az iki Cloudyn yönetici hesabı oluşturmanızı öneririz.

Cloudyn portalda oturum açamaz, doğru Azure maliyeti yönetim URL'si için Cloudyn oturum açmak için kullandığınızdan emin olun. Aşağıdaki URL'lerden birini kullanın:

- https://Azure.cloudyn.com
- https://MS.Portal.Azure.com/#blade/Microsoft_Azure_CostManagement/CloudynMainBlade

Cloudyn doğrudan URL https://app.cloudyn.com kullanmaktan kaçının.

## <a name="how-do-i-activate-unactivated-accounts-with-azure-credentials"></a>Azure kimlik bilgilerine sahip etkinleştirilmemiş hesaplarla nasıl etkinleştirilsin mi?

Azure hesaplarınızı Cloudyn tarafından bulunan hemen maliyet verileri hemen maliyet tabanlı raporlara sağlanır. Ancak, kullanım ve performans verilerini sağlamak Cloudyn için hesapları için Azure kimlik bilgilerinizi kaydetmeniz gerekir. Yönergeler için bkz: [eklemek Azure Resource Manager](https://support.cloudyn.com/hc/en-us/articles/212784085-Adding-Azure-Resource-Manager).

Azure eklemek için hesap adı, abonelik sağındaki Düzenle simgesi Cloudyn portalında bir hesabın kimlik bilgilerini seçin.

Azure kimlik bilgilerinizi Cloudyn için eklenene kadar hesabı olarak görünür _beklemediğiniz etkinleştirilmiş_.

## <a name="how-do-i-add-multiple-accounts-and-entities-to-an-existing-subscription"></a>Mevcut bir aboneliğe birden çok hesap ve varlıkları nasıl ekleyebilirim?

Ek varlıklar ek Kurumsal Anlaşma Cloudyn aboneliği eklemek için kullanılır. Aşağıdaki bağlantılarda ek varlıkları nasıl eklendiği açıklanmaktadır:

- [Varlık ekleme](https://support.cloudyn.com/hc/en-us/articles/212016145-Adding-an-Entity) makale
- [Hiyerarşinizi maliyet varlıklarla tanımlama](https://support.cloudyn.com/hc/en-us/articles/115005142529-Video-Defining-your-hierarchy-with-Cost-Entities) video

CSP'ler için:

Bir varlık için ek CSP hesapları eklemek için seçin **MSP erişim** yerine **Kurumsal** oluşturduğunuzda Yeni varlık. Hesabınız bir Kurumsal Anlaşma kaydedilir ve CSP kimlik bilgilerini eklemek istediğiniz, Cloudyn destek personeli hesap ayarlarınızı değiştirmeniz gerekebilir. Ücretli bir Azure abone değilseniz, Azure portalında yeni bir destek isteği oluşturabilirsiniz. Seçin **Yardım + Destek**ve ardından **yeni destek isteği**.

## <a name="how-do-i-change-the-currency-symbol-used-in-cloudyn"></a>Cloudyn içinde kullanılan para birimi simgesini nasıl değişiyor?

Tek bir varlık içindeki tüm Azure hesaplar aynı para birimi kullandığınızda, kullandığınız para birimi otomatik olarak algılanır. Ancak, para birimi simgesini yanlışlıkla olarak gösterilen  **$**  herhangi biri aşağıdaki para birimi için:

- GBP İngiltere sterlin =
- EUR Avrupa euro =
- INR Hindistan Rupisi =
- NOK Norveç Kronu =

Para birimi simgesini gösterebilir rağmen  **$**  ABD doları için maliyet değerler doğru para birimi cinsinden gösterilir. Örneğin, tüm hesaplarınızdaki aynı varlıkta Euro kullanırsanız _değerleri_ Cloudyn içinde gösterilen Euro, rağmen olan  **$**  simgesi görünürse yanlışlıkla.

Bir Azure Kurumsal anlaşmasına müşteri değilseniz, Cloudyn destek personeli $ maliyet raporlarda gösterilen, para birimi simgesini değiştirebilirsiniz. Azure portalında yeni bir destek isteği oluşturabilirsiniz. Seçin **Yardım + Destek**ve ardından **yeni destek isteği**.

Bir CSP müşterisi değilseniz, para birimi simgesini değiştiremezsiniz. Cloudyn yalnızca ABD Doları kullanmak oranı kartları destekler. Cloudyn farklı para oranı kartlarını desteklemek üzere seçeneği araştırmaktadır.

## <a name="what-are-cloudyn-data-refresh-timelines"></a>Cloudyn veri nelerdir zaman çizelgelerini Yenile?

Aşağıdaki veri yenileme zamanlamaları Cloudyn sahiptir:

- **İlk**: ayarladıktan sonra Cloudyn içinde maliyet verileri görüntülemek için 24 saat sürebilir. Ayrıca boyutlandırma önerilerini görüntülemek için yeterli veri toplamak üzere Cloudyn 10 güne kadar devam edebilir.
- **Günlük**: her ay sonuna onuncu günden Cloudyn verilerinizin önceki günden sonra UTC + 3 hakkında sonraki gün güncel göstermesi gerekir.
- **Aylık**: her ayın on günü için ilk günden Cloudyn verilerinizi yalnızca önceki ayın sonuna üzerinden gösterebilir.

Cloudyn önceki günün önceki gün tam verileri kullanılabilir olduğunda verileri işler. Önceki günün verileri her gün genellikle UTC + 3 hakkında Cloudyn tarafından kullanılabilir. Etiketler gibi bazı veriler ilaveten 24 işlemek için saat sürebilir.

Geçerli ay için verileri her ayın başlangıcında koleksiyonu için kullanılamaz. Süresi boyunca, önceki ay için kendi fatura hizmet sağlayıcıları sonlandırın. Önceki ayın verilerini her ayın başlangıcını sonra 10 gün Cloudyn 5'te görünüyor. Bu süre boyunca yalnızca amortized maliyetleri önceki ayın görebilirsiniz. Günlük faturalama veya kullanım verileri göremeyebilirsiniz. Cloudyn veriler kullanılabilir duruma geldiğinde firmalarda geriye dönük işler. İşlemeden sonra tüm aylık verileri görüntülenir beşinci günün ve her ayın on günü arasında.

Verileri Azure'dan Cloudyn için gönderme gecikme olursa, verileri Azure içinde hala kaydedilir. Bağlantı kurulduğunda verileri için Cloudyn aktarılır.

## <a name="how-can-a-direct-csp-configure-cloudyn-access-for-indirect-csp-customers-or-partners"></a>Nasıl doğrudan bir CSP Cloudyn erişim dolaylı CSP müşterileri ve ortakları için yapılandırabilir miyim?

Yönergeler için bkz: [Cloudyn içinde dolaylı CSP erişimi yapılandırma](quick-register-csp.md#configure-indirect-csp-access-in-cloudyn).

## <a name="what-causes-the-optimizer-menu-item-to-appear"></a>İyileştirici menü öğesi görünmesine nedeni nedir?

Azure Resource Manager erişimi ve veri ekledikten sonra görmeniz gerekir, toplanır **iyileştirici** seçeneği. Azure Resource Manager erişimi etkinleştirmek için bkz: [nasıl Azure kimlik etkinleştirilmemiş hesaplarıyla etkinleştirebilirim?](#how-do-i-activate-unactivated-accounts-with-azure-credentials)

## <a name="is-cost-managementcloudyn-agent-based"></a>Tabanlı maliyet Yönetimi/Cloudyn aracı?

Hayır. Aracıları kullanılmaz. Azure sanal makine ölçüm verilerini VM'ler için Microsoft Öngörüler API'SİNDEN toplanır. Azure VM'lerin ölçüm verilerini toplamak istiyorsanız, tanılama ayarlarının etkin olması gerekir.

## <a name="do-cloudyn-reports-show-more-than-one-ad-tenant-per-report"></a>Cloudyn raporlarını, rapor başına birden fazla AD Kiracı Göster?

Evet. Yapabilecekleriniz [karşılık gelen bir bulut hesap varlık oluşturursanız](tutorial-user-access.md#create-entities) sahip olduğunuz her bir AD Kiracı için. Ardından, tüm Azure AD Kiracı verilerinizi ve Amazon Web Hizmetleri ve Google Cloud Platform'un gibi diğer bulut platformu sağlayıcılarını görüntüleyebilirsiniz.
