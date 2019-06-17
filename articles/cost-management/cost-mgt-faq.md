---
title: Cloudyn Azure için sık sorulan sorular | Microsoft Docs
description: Bu makalede, Cloudyn hakkında genel soruların yanıtları sağlanır.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 05/21/2019
ms.topic: troubleshooting
ms.service: cost-management
manager: benshy
ms.custom: seodec18
ms.openlocfilehash: 02a03adb128c140343032075ec334cbd6d88729b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66002018"
---
# <a name="frequently-asked-questions-for-cloudyn"></a>Cloudyn için sık sorulan sorular

Bu makalede Cloudyn hakkında bazı yaygın sorular ele alınmıştır. Cloudyn hakkında sorularınız varsa, onları sorabilir [Cloudyn için sık sorulan sorular](https://social.msdn.microsoft.com/Forums/en-US/231bf072-2c71-4121-8339-ac9d868137b9/faqs-for-cloudyn-cost-management?forum=Cloudyn).

## <a name="how-can-i-resolve-common-indirect-enterprise-setup-problems"></a>Sık karşılaşılan dolaylı Kurumsal kurulum sorunlarını nasıl giderebilirim?

Cloudyn portalını ilk kullandığınızda, Kurumsal Anlaşma veya Bulut Çözümü Sağlayıcı (CSP) kullanıcısıysanız şu iletileri görebilirsiniz:

- "Belirtilen API anahtarı bir üst düzey kayıt anahtarı değil" görüntülenen **Cloudyn ayarlamak** Sihirbazı.
- "– Hayır Kurumsal Anlaşma portalında gösterilen doğrudan kayıt".
- "Son 30 güne ait kullanım verisi bulunamadı. Biçimlendirme Azure hesabınız için etkinleştirildi emin olmak için dağıtımcı başvurun", Cloudyn portalında görüntülenmez.

Önceki ileti, bir kurumsal bayi veya CSP aracılığıyla Azure Kurumsal Anlaşma satın aldığınızı belirtir. Cloudyn'de verilerinizi görüntüleyebilmeniz için satıcınızın veya CSP’nin Azure hesabınız için _işaretlemeyi_ etkinleştirmesi gerekir.

Sorunların çözümü:

1. Kurumsal bayinin hesabınız için _işaretlemeyi_ etkinleştirmesi gerekir. Yönergeler için bkz. [Dolaylı Müşteri Ekleme Kılavuzu](https://ea.azure.com/api/v3Help/v2IndirectCustomerOnboardingGuide).

2. Cloudyn ile kullanılmak için Azure Kurumsal Anlaşma anahtarını üretirsiniz. Yönergeler için [ekleme bilgisayarınızı Azure EA](quick-register-ea.md#register-with-cloudyn) veya [Bul EA kayıt Kimliğinizi ve API anahtarı](https://youtu.be/u_phLs_udig).

Yalnızca bir Azure hizmet yöneticisi Cloudyn'i etkinleştirebilir. Ortak yönetici izinleri yeterli değil.

Cloudyn'i kurmak için Azure Kurumsal Anlaşma API anahtarını oluşturabilmeniz için aşağıda belirtilen kaynaklarda yer alan yönergeleri izleyerek Azure Faturalama API’sini etkinleştirmeniz gerekir:

- [Kurumsal müşteriler için Raporlama API’lerine genel bakış](../billing/billing-enterprise-api.md)
- **API’lere veri erişimini etkinleştirme** bölümünde [Microsoft Azure kurumsal portal Raporlama API’si](https://ea.azure.com/helpdocs/reportingAPI)


Departman yöneticilerine, hesap sahiplerine ve kurumsal yöneticilere Faturalama API’si ile _ücretleri görüntüleme_ izni vermeniz de gerekebilir.

## <a name="why-dont-i-see-optimizer-recommendations"></a>İyileştirici önerileri neden göremiyorum?

Öneri bilgiler, yalnızca etkinleştirilen hesapları için kullanılabilir. Herhangi bir öneri bilgisi görmeyeceğiniz **iyileştirici** rapor hesapları için kategorileri *etkinleştirilmemiş*de dahil olmak üzere:

- En iyi duruma getirme Yöneticisi
- Boyutlandırma optimizasyonu
- Verimsizlikleri

İyileştirici öneri verileri görüntüleyemezsiniz sonra büyük olasılıkla etkinleştirilmemiş olan hesapların varsa. Bir hesabı etkinleştirmek için Azure kimlik bilgilerinizle kaydetmeniz gerekir.

Bir hesabı etkinleştirmek için:

1.  Cloudyn portalında, sağ üst kısımdaki **Ayarlar**’a tıklayın ve **Bulut Hesapları**’nı seçin.
2.  Microsoft Azure hesapları sekmesinde sahip hesaplar için Ara bir **etkinleştirilmemiş** abonelik.
3.  Sağa etkinleştirilmemiş bir hesabın tıklayın **Düzenle** benzer bir kalem simgesi.
4.  Kiracı kimliği ve oran Kimliğinizi otomatik olarak algılanır. **İleri**'ye tıklayın.
5.  Azure portalına yönlendirilirsiniz. Portalda oturum açın ve Azure verilerinize erişmek için Cloudyn Toplayıcı yetkisi verin.
6.  Ardından, Cloudyn hesapları Yönetim sayfasına yönlendirilirsiniz ve aboneliğiniz ile güncelleştirilir **etkin** hesap durumu. Bu, yeşil onay işareti simgesi gösterir.
7.  Bir veya daha fazla abonelik için bir yeşil onay işareti simgesini görmüyorsanız, (CloudynCollector) abonelik için bir okuyucu uygulaması oluşturmak için gerekli izinlere sahip değilsiniz demektir. Abonelik için daha yüksek izinlere sahip bir kullanıcı, 3 ve 4 gerekiyor.  

Yukarıdaki adımları tamamladıktan sonra bir veya iki gün içinde iyileştirici önerileri görüntüleyebilirsiniz. Ancak, uygulamanın tam iyileştirme veri kullanılabilir olmadan önce en fazla beş gün sürebilir.


## <a name="how-do-i-enable-suspended-or-locked-out-users"></a>Askıya alınmış veya kilitlenmiş kullanıcıları nasıl etkinleştirebilirim?

İlk olarak, kullanıcı hesaplarını almak neden en yaygın senaryo bakalım *initiallySuspended*.

> Microsoft bulut çözümü sağlayıcısı veya Kurumsal Anlaşma kullanıcı admin1 olabilir. Kuruluş, Cloudyn kullanmaya başlamak hazırdır.  Kendisi, Azure portalı üzerinden kaydeder ve Cloudyn portalında oturum açar. Cloudyn portalında oturum açtığında ve Cloudyn hizmet kaydeder kişi Admin1 olur *birincil yönetici*. Tüm kullanıcı hesaplarını admin1 oluşturmaz. Bununla birlikte, Cloudyn portalını kullanarak, bunlar Azure hesapları oluşturun ve bir varlık hiyerarşisi ayarlayın. Admin1 Admin2, Cloudyn portalında oturum açın ve Cloudyn ile kaydetmek için ihtiyaç duydukları bir kiracı Yöneticisi, sizi bilgilendirir.
>
> Azure portalı üzerinden Admin2 kaydeder. Cloudyn portalında oturum açmayı denediğinde, ancak bunların hesabı olduğunu söyleyen bir hata iletisi **askıya**. Admin1, birincil yönetici hesabı askıya alınması bildirilir. Admin1 Admin2'ın hesabı etkinleştirin ve vermek için gereksinim duyduğu *yönetici varlık erişimi* uygun varlıkların ve kullanıcı yönetim erişimini ve etkin kullanıcı hesabı sağlar.


Bir istek, bir kullanıcı için erişim izni olan bir uyarı alırsanız, kullanıcı hesabı'nı etkinleştirmeniz gerekir.

Kullanıcı hesabını etkinleştirmek için:

1. İçin Cloudyn, Cloudyn ' için kullandığınız Azure yönetici kullanıcı hesabıyla oturum açın. Veya yönetici erişim izni bir kullanıcı hesabıyla oturum açın.
2. Sağ üst köşedeki dişli simgesini seçin ve seçin **kullanıcı yönetimi**.
3. Kullanıcıyı bulun, kalem simgesini seçin ve ardından kullanıcı düzenleyin.
4. Altında **kullanıcı durumu**, durumu değiştirme **askıya alındı** için **etkin**.

Cloudyn kullanıcı hesapları, çoklu oturum açmayı azure'dan kullanarak bağlanın. Bir kullanıcı parolasını mistypes, Azure yine de erişebilmeleri olsa da bunlar Cloudyn dışında saldırdığında.

Azure'da varsayılan adresinden e-posta adresinizi Cloudyn değiştirirseniz, hesabınızın kilitli. "Durumu initiallySuspended" gösterebilir Kullanıcı hesabınız kilitlendi, hesabınızı sıfırlamak için başka bir yönetici ile iletişime geçin.

Hesaplardan birini kilitli durumunda en az iki Cloudyn yöneticisi hesabı oluşturmanızı öneririz.

Cloudyn portalında oturum açamıyorsanız Cloudyn'e oturum açmak için doğru URL'yi kullandığınızdan emin olun. Kullanım [ https://azure.cloudyn.com ](https://ms.portal.azure.com/#blade/Microsoft_Azure_CostManagement/CloudynMainBlade).

Cloudyn doğrudan URL kullanmaktan kaçının https://app.cloudyn.com.

## <a name="how-do-i-activate-unactivated-accounts-with-azure-credentials"></a>Azure kimlik etkinleştirilmemiş hesaplarıyla nasıl etkinleştirebilirim?

Hesaplarınız Cloudyn tarafından bulunduktan hemen sonra maliyet verilerini hemen maliyet tabanlı raporlara sağlanır. Bununla birlikte, Cloudyn kullanım ve performans verilerini sağlamak hesapları için Azure kimlik bilgilerinizi kaydetmeniz gerekir. Yönergeler için [bir hesap ekleyin veya aboneliği güncelleştirme](activate-subs-accounts.md#add-an-account-or-update-a-subscription).

Azure'ı eklemek için hesap adı, abonelik sağındaki düzenleme simgesi Cloudyn portalında, bir hesabın kimlik bilgilerini seçin.

Azure kimlik bilgilerinizi Cloudyn'e eklenene kadar hesabı olarak görünür _beklemediğiniz etkinleştirilmiş_.

## <a name="how-do-i-add-multiple-accounts-and-entities-to-an-existing-subscription"></a>Mevcut bir aboneliğe birden çok hesap ve varlıkları nasıl ekleyebilirim?

Ek varlıklar Cloudyn aboneliğine ek Kurumsal anlaşmalar eklemek için kullanılır. Daha fazla bilgi için [oluşturun ve varlıkları yönetme](tutorial-user-access.md#create-and-manage-entities).

CSP'ler için:

Bir varlığa ek CSP hesapları eklemek için seçin **MSP erişim** yerine **Kurumsal** yeni bir varlık oluşturduğunuzda. Hesabınızı bir Kurumsal Anlaşma kaydedilir ve CSP kimlik bilgileri eklemek istediğiniz, Cloudyn destek personeli, hesap ayarlarını değiştirmek gerekebilir. Ücretli bir Azure abonesi iseniz, Azure portalında yeni bir destek isteği oluşturabilirsiniz. Seçin **Yardım + Destek**ve ardından **yeni destek isteği**.

## <a name="currency-symbols-in-cloudyn-reports"></a>Cloudyn raporlarında para birimi sembolleri

Farklı para birimlerinin kullanarak birden çok Azure hesabında olabilir. Bununla birlikte, Cloudyn maliyet raporlarında, rapor başına birden fazla para birimi türü gösterme.

Farklı para birimlerinin kullanarak birden fazla aboneliğiniz varsa, bir üst varlık ve onun alt varlık para ABD Doları görüntülenir **$** . Bizim önerilen en iyi uygulama, aynı varlık hiyerarşideki farklı para birimlerinin kullanarak önlemek içindir. Diğer bir deyişle, bir varlık yapısında düzenlenmiş tüm abonelikler aynı para kullanmanız gerekir.

Cloudyn, otomatik olarak, Kurumsal Anlaşma abonelik para algılar ve düzgün şekilde raporlar sunar.  Bununla birlikte, Cloudyn ABD Doları yalnızca görüntüler **$** CSP ve doğrudan web Azure hesapları için.

## <a name="what-are-cloudyn-data-refresh-timelines"></a>Cloudyn veri nelerdir zaman çizelgeleri yenilensin mi?

Cloudyn, aşağıdaki veri yenileme zaman çizelgeleri sahiptir:

- **İlk**: Sonra ayarlamak uygulamanın Cloudyn'de maliyet verilerini görüntülemek için 24 saat sürebilir. Ayrıca, Cloudyn boyutlandırma önerileri görüntülemek için yeterli veri toplamak üzere 10 güne kadar da sürebilir.
- **Günlük**: On günün sonuna kadar her ay, Cloudyn verilerinizin önceki günden sonra UTC + 3 hakkında sonraki günün tarihi göstermelidir.
- **Aylık**: Her ayın on günü için ilk günden Cloudyn verilerinizi yalnızca önceki ayın sonuna üzerinden gösterebilir.

Cloudyn, önceki günün önceki gün tam veri kullanılabilir olduğunda verileri işler. Önceki günün verileri her gün genellikle UTC + 3 hakkında Cloudyn tarafından kullanılabilir. Etiketler gibi bazı veriler ilaveten 24 işlemek için saat sürebilir.

Geçerli ay için verileri, her ayın başında toplamalarında kullanılamaz. Dönemi boyunca, hizmet sağlayıcıları önceki ay için bunların faturalandırma sonlandırın. Önceki ayın verilerini 10 gün sonra her ay başı Cloudyn 5'te görünür. Bu süre boyunca, yalnızca önceki ayın amorti edilmiş maliyet görebilirsiniz. Günlük faturalandırma ya da kullanım verilerini göremeyebilirsiniz. Cloudyn veri kullanılabilir olduğunda, geriye dönük olarak işler. İşlemden sonra tüm aylık verileri görüntülenir her ayın on günü beşinci arasında.

Cloudyn'e Azure'dan veri gönderen bir gecikme varsa, verileri Azure'da hala kaydedilir. Bağlantı geri geldiğinde, veriler Cloudyn'e aktarılır.

## <a name="cost-fluctuations-in-cloudyn-cost-reports"></a>Cloudyn maliyet raporlarında maliyet dalgalanmaları

Her bulut hizmeti sağlayıcıları güncelleştirilmiş fatura dosyaları gönderdiğinizde maliyet raporlarında maliyet dalgalanmaları gösterebilirsiniz. Yeni dosyaları bir bulut hizmeti sağlayıcısından dışında normal günlük veya aylık zamanlama bildirimi alındığında dalgalı maliyetler oluşur. Maliyet değişiklikler Cloudyn yeniden hesaplanmasına neden yoktur.

Ay boyunca, bulut hizmet sağlayıcınız tarafından gönderilen tüm faturalandırma günlük maliyetlerinizi tahmini dosyalarıdır. Bazen verileri sık sık güncelleştirilir — birden çok kez günde bazen. AWS ile Azure sık güncelleştirmeler. Önceki aya ait fatura hesaplama ve son fatura dosya alındığında, maliyet toplamları kararlı kalmalıdır. Genellikle, 10 ay tarafından.

Değişiklikler, bulut hizmeti sağlayıcısından maliyet ayarlamaları aldığında gerçekleşir. Kredisi alma örnek verilebilir. İlgili ay kapatıldıktan sonra değişiklikleri ay ortaya çıkabilir. Değişiklikler yeniden hesaplama bulut hizmet sağlayıcınız tarafından yapılan her. Cloudyn, tüm ayarlarını hesaplanır emin olmak için geçmiş verilerini güncelleştirir. Ayrıca maliyetleri doğru içinde görüntülendiğini doğrular, bildirir.

## <a name="how-can-a-direct-csp-configure-cloudyn-access-for-indirect-csp-customers-or-partners"></a>Nasıl doğrudan bir CSP dolaylı CSP müşterileri veya iş ortakları için Cloudyn erişimi yapılandırabilirsiniz?

Yönergeler için [Cloudyn'de dolaylı CSP erişimini yapılandırma](quick-register-csp.md#configure-indirect-csp-access-in-cloudyn).

## <a name="what-causes-the-optimizer-menu-item-to-appear"></a>Görüntülenecek iyileştirici menü öğesi nedeni nedir?

Azure Resource Manager erişim ve veri ekledikten sonra görmelisiniz, toplanır **iyileştirici** seçeneği. Azure Resource Manager erişimi etkinleştirmek için bkz: [nasıl etkinleştirilmemiş hesapları Azure kimlik bilgileri ile etkinleştirebilirim?](#how-do-i-activate-unactivated-accounts-with-azure-credentials)

## <a name="is-cloudyn-agent-based"></a>Cloudyn Aracısı bağlıdır?

Hayır. Aracıları kullanılmaz. Microsoft Insights API'den VM'ler için Azure sanal makine ölçüm verileri toplanır. Azure sanal makinelerinden ölçüm verilerini toplamak istiyorsanız, tanılama ayarları etkin olması gerekir.

## <a name="do-cloudyn-reports-show-more-than-one-ad-tenant-per-report"></a>Cloudyn raporlarını, rapor başına birden fazla AD Kiracı göstermemesidir?

Evet. Yapabilecekleriniz [karşılık gelen bir bulut hesap varlığı oluşturma](tutorial-user-access.md#create-and-manage-entities) sahip olduğunuz her bir AD Kiracı için. Ardından, tüm Azure AD Kiracı verilerinizi ve Amazon Web Services ve Google Cloud Platform dahil diğer bulut platform sağlayıcıları görüntüleyebilirsiniz.
