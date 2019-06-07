---
title: İş ortağı Merkezi'nde bir ticari Market hesabı yönetme
description: Bir ticari Market hesabı iş ortağı Merkezi'nde yönetmeyi öğrenin.
author: mattwojo
manager: evansma
ms.author: parthp
ms.service: marketplace
ms.topic: how-to
ms.date: 05/30/2019
ms.openlocfilehash: 5cb4caa6f0f8098e68d693be6cc2f33b5ccbeb32
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66752836"
---
# <a name="how-to-manage-your-commercial-marketplace-account-in-partner-center"></a>İş ortağı merkezi ticari Market hesabınızı yönetme 

Kaydederler [oluşturulan bir iş ortağı merkezi hesabınız](./create-account.md), hesabınızı ve teklifler kullanarak yönetebileceğiniz [ticari Marketplace panosunu](https://partner.microsoft.com/dashboard/commercial-marketplace/overview).

Bu makalede, biz iş ortağı merkezi hesabınız yönetmek nasıl içine ele alacağız da dahil olmak üzere: 

- [İş ortağı merkezi hesap ayarlarınızı erişim](#access-your-account-settings)
- [Yayımcı kimliği, satıcı kimliği, kullanıcı kimliği ve Azure AD kiracılarıyla bulun](#account-details)
- [Kişi bilgilerini güncelleştirin](#contact-info)
- [Finansal ayrıntılarını (ödeme hesabı, vergi profili ödeme Beklemede Durumu) yönetme](#financial-details)
- [Müşteri kullanımını izlemek için GUID'ler izleme ayarlayın](#tracking-guids)
- [Yönetici kullanıcılar](#manage-users)
- [Yöneticisi grupları](#manage-groups)
- [Yönetici Azure AD uygulamaları](#manage-azure-ad-applications)
- [Kullanıcı rolleri ve izinleri tanımlama](#define-user-roles-and-permissions)
- [Azure AD kiracılarıyla (iş hesapları) yönetme](#manage-tenants)
- [Yöneticisi iş ortağı merkezi sözleşmeleri](#agreements)


## <a name="access-your-account-settings"></a>Hesap ayarlarınızı erişim

Zaten yapmadıysanız, sizin (veya kuruluşunuzun Yöneticisi) erişmeli [hesap ayarları](https://partner.microsoft.com/dashboard/account/management) için iş ortağı merkezi hesabınız için:
- Şirketinizin hesabınızın doğrulama durumunu denetleyin
- Satıcı Kimliği, MPN kimliği, yayımcı kimliği onaylayın ve şirket onaylayanı ve satıcı kişi bilgilerini başvurun
- Uygunsa, vergi muafiyetleri dahil şirketinizin finansal ayrıntılarını ayarlayın
- İş hesabınızın iş ortağı Merkezi'nde kullanacak olan herkes için kullanıcı hesapları oluşturun

### <a name="open-developer-settings"></a>Geliştirici ayarları

Sağ üst köşesinde hesap ayarlarını bulunduğu, [ticari Marketplace panosunu](https://partner.microsoft.com/dashboard/commercial-marketplace) iş ortağı Merkezi'nde. (Ekranın sağ üst köşesinde Pano) dişli simgesini seçin ve ardından **Geliştirici ayarları**. 

![İş ortağı Merkezi'nde hesap ayarları menüsü](./media/dashboard-developer-settings.png)

İçinde **hesap ayarları**, görüntüleme olanağınız olacaktır:
- **Hesap ayrıntıları**: Hesap türü ve hesap durumu
- **Yayımcı kimlikleri**: Satıcı Kimliği, kullanıcı kimliği, yayımcı kimliği, Azure AD kiracıları, vs.
- **Kişi bilgileri**: Yayımcı görünen adı, Satıcı ilgili kişi adı, e-posta, telefon ve adresi
- **Finans bilgilerini**: Ödeme hesap, vergi profili ve ödeme Beklemede durumu
- **cihazları**: Hesabınızla ilişkili herhangi bir test cihazları
- **GUID'ler izleme**: Tüm izleme hesabınızla GUID'leri ilişkilendirme

### <a name="account-details"></a>Hesap ayrıntıları

Hesap Ayrıntıları bölümünde gibi temel bilgileri görebilir, **hesap türü** (şirket veya bireysel) ve **doğrulama durumu** hesabınızın. Hesap doğrulama işlemi sırasında bu ayarları e-posta doğrulama, İstihdam doğrulama ve iş doğrulama dahil olmak üzere gerekli her adımın görüntüler. Ayrıca, e-posta burada güncelleştirin ve gerekirse doğrulamasını yeniden gönder. 

### <a name="publisher-ids"></a>Yayımcı kimlikleri

Yayımcı kimlikleri bölümünde görebilirsiniz, **satıcı kimliği**, **MPN kimliği**, ve **yayımcı kimliği**. Bu değerler, geliştirici hesabınıza benzersiz olarak tanımlanabilmesi için Microsoft tarafından atanan ve düzenlenemez.

### <a name="contact-info"></a>İletişim bilgileri

Kişi bilgileri bölümünde görebilirsiniz, **yayımcı görünen adı**, **satıcı kişi bilgilerini** (ilgili kişi adı, e-posta, telefon numarası ve şirket satıcı adresi) ve **şirket Onaylayan** (adı, e-posta ve telefon numarası ile şirket için kararları onaylama yetkisi kişinin). 

### <a name="financial-details"></a>Finansal ayrıntıları

Finansal Ayrıntılar bölümünde sağlayın veya Ücretli uygulamalarını, eklentileri veya hizmetler yayımlarsanız, finansal bilgilerinizi güncelleştirin. 

Yalnızca ücretsiz teklifleri listesinde planlıyorsanız, hiçbir vergi formu doldurun veya bir ödeme hesabı ayarlamanız gerekmez. Fikrinizi değiştirirseniz ve Microsoft satmak istediğinize karar verin ödeme hesabınızı ayarlama ve o anda vergi formları doldurun. 

#### <a name="payout-account"></a>Ödeme hesabı

Ödeme işlemi devam eder, satış rakamlarını gönderildiği banka hesabı hesabıdır. Bu banka hesabı iş ortağı merkezi hesabınız kayıtlı olduğu aynı ülkede olması gerekir.

Ödeme hesabınızı kurmak için şunları yapmanız **Microsoft Account ilişkilendirin**:
1. İçinde **hesap ayarları**altında **Finans bilgilerini** bölümünden **Microsoft Account ilişkilendirin**. 
2. İstendiğinde, Microsoft hesabı (MSA) ile oturum açın. Bu hesap zaten başka bir iş ortağı merkezi hesabınız ile ilişkilendirilemez. 
3. Ödeme hesabınızın, iş ortağı Merkezi, oturumunuzu tamamen Kurulumu tamamlamak için daha sonra geri Microsoft Account (yerine açın iş hesabınızda) oturum. 

Microsoft Account ilişkili olduğundan, bir ödeme hesabı eklemek için gerekir:
- **Bir ödeme yöntemi seçin**: Banka hesabı veya PayPal
- **Ödeme bilgileri ekleyin**: Bu hesap türü (denetimi veya tasarruf) seçerek, hesap numarası, hesap sahibi adı girerek ve yönlendirme içerebilir numarası, fatura adresi, telefon numarası veya PayPal e-posta adresi. * PayPal hesabı ödeme yönteminiz olarak ve olup, Pazar bölgenizde desteklenip desteklenmediğini öğrenmek için kullanma hakkında daha fazla bilgi için [PayPal bilgisi](https://docs.microsoft.com/windows/uwp/publish/setting-up-your-payout-account-and-tax-forms#paypal-info).

> [!IMPORTANT]
> Ödeme hesabınızı değiştirme, ödemeler tarafından en çok bir ödeme dönemi gecikmeye yol açabilir. Bu gecikme, ilk ödeme hesabınızı ayarlarken az önce yaptığımız gibi hesap değişikliğini doğrulamak ihtiyacımız oluşur. Hesabınızı doğrulandıktan sonra hala tam miktar için ücretli; son ödeme için geçerli ödeme döngüsü sonraki teste eklenir.  

#### <a name="tax-profile"></a>Vergi profili

Geçerli, vergi profili durumu doğru onaylayan gözden **varlık türü** ve **vergi sertifika bilgilerini** görüntülenir. Seçin **Düzenle** formları tamamlayın veya güncelleştirmek için gerekli.

Vergi durumunuzu kurulabilmesi için ikametgahınızda Vatandaşlık ve ülkenizi belirtin ve ülke/bölge ile ilişkili uygun vergi formları tamamlayın.

İkamet veya Vatandaşlık ülke bağımsız olarak, Microsoft herhangi bir teklif satmak, Amerika Birleşik Devletleri vergi formları doldurmanız gerekir. Amerika Birleşik Devletleri yerleşimi gereksinimleri karşılaması iş ortaklarının bir IRS W-9 formu doldurmanız gerekir. Diğer iş ortaklarıyla Amerika Birleşik Devletleri dışında bir IRS w-8 formu doldurmanız gerekir. Vergi profilinizi tamamlamanız gibi bu formları çevrimiçi doldurabilirsiniz.

Amerika Birleşik Devletleri ayrı vergi mükellefi kimlik numarası (veya Yazmadınız) ödemeler, Microsoft'tan veya vergi anlaşma avantajları talep için gerekli değildir.

Tamamlayın ve iş ortağı Merkezi'nde Elektronik vergi formlarınızı gönderin; Çoğu durumda, yazdırma ve herhangi bir form posta gerekmez.

Farklı ülke ve bölgelerden vergi farklı gereksinimleri vardır. Vergiler ödeme yapması gereken tam ülke ve bölgelerden tekliflerinizi satın burada üzerinde bağlıdır. Microsoft, satış ve sizin adınıza bazı ülkelerde vergi kullanımı remits. Bu, ülkede teklifinizin listeleme aşamasında tanımlanır. Burada kaydoldunuz, bağlı olarak diğer ülkelerde havale satışları ve vergi makamına yerel için doğrudan satış kullanmak gerekebilir. Ayrıca, aldığınız satış tutarını gelir Vergiye tabi olabilir. Ülkenizi veya bölgenizi en iyi şekilde, Microsoft satış işlem için doğru vergi bilgileri belirlemenize yardımcı olabilecek ilgili yetkilisi başvurup önemle öneririz.

##### <a name="withholding-rates"></a>Tutma oranları
Uygun vergi oranı stopaj vergisi formlarınızı gönderdiğiniz bilgileri belirler. Amerika Birleşik Devletleri yaptığınız satış stopaj oranı uygulanır; ABD dışındaki konumlara yapılan satışlar stopaj tabi değildir. Stopaj fiyatları farklılık gösterir, ancak çoğu geliştirici, Amerika Birleşik Devletleri dışında kaydetme varsayılan oran % 30'dur. Ülkeniz için bir gelir vergi anlaşma ile Amerika Birleşik Devletleri kabul varsa bu hızının düşürülmesi seçeneğiniz vardır.

##### <a name="tax-treaty-benefits"></a>Vergi anlaşma avantajları
Amerika Birleşik Devletleri dışında olması durumunda, anlaşma avantajları vergi yararlanmak mümkün olabilir. Bu avantajlar ülkeden ülkeye değişir ve Microsoft alır. vergiler miktarını azaltmak izin verebilir. Vergi anlaşma avantajları, Kısım II W 8BEN formun tamamlayarak ortamı talep edebilir. Ülke veya bölgenizde yararlar sizin için geçerli olup olmadığını belirlemek için uygun kaynaklarla iletişim öneririz.

[Vergi ayrıntıları hakkında daha fazla Windows uygulama/oyun geliştiricileri ve Azure Marketi yayımcılarının için bilgi](https://docs.microsoft.com/windows/uwp/publish/tax-details-for-paid-apps).

#### <a name="payout-hold-status"></a>Ödeme Beklemede durumu

Varsayılan olarak, Microsoft ödeme aylık olarak gönderir. Ancak, ödemeler önleyecek, beklemeye seçeneğine sahip ödemeler hesabınıza gönderme. Ödemeler beklemeye kullanmayı tercih ederseniz kazanın ve Ayrıntılar sağlayan gelir kaydetmek devam, **ödeme özeti**. Ancak tutma kaldırana kadar biz hesabınıza ödeme göndermez. 

Ödemeler beklemeye almak için Git **hesap ayarları**. Altında **Finans bilgilerini**, **ödeme basılı durumu** bölümünde, kaydırıcısını **üzerinde**. Herhangi bir zamanda, ödeme Beklemede Durumu değiştirmek, ancak kararınız sonraki aylık ödeme etkiler unutmayın. Nisan'ın ödeme tutmak istiyorsanız, örneğin, ödeme Beklemede durumu ayarlamak için emin olun **üzerinde** Mart bitmeden önce.

Ayarladıktan sonra ödeme tutmak için durum **üzerinde**, kaydırıcıyı geçiş kadar tüm ödemeler tutmada olacak geri **kapalı**. Bunu yaptığınızda (herhangi bir geçerli ödeme eşik karşılanmış sağlanan), sonraki aylık ödeme döngüsü sırasında dahil olacaktır. Örneğin, beklemede, ödemeler vardı ancak Haziran ayında oluşturulan bir ödeme ister ve ardından değiştirilecek emin olun, ödeme tutun durumuna **kapalı** Mayıs bitmeden önce.

> [!NOTE]
> **Ödeme basılı durumu** seçimi uygulandığı **tüm** Azure Market, AppSource, Microsoft Store reklam vb., gibi Microsoft Partner Center Ücretli gelir kaynaklarını.). Her bir gelir kaynağı için farklı bir tutma durumları seçemezsiniz.

### <a name="devices"></a>Cihazlar

Cihaz yönetim ayarlarını yalnızca UWP yayımlama için geçerlidir. [Daha fazla bilgi edinin](https://docs.microsoft.com/windows/uwp/publish/manage-account-settings-and-profile#additional-settings-and-info).

### <a name="tracking-guids"></a>İzleme GUID'leri

Genel olarak benzersiz tanımlayıcıları (GUID'ler) Azure kullanımınızı izlemek için kullanılabilecek benzersiz bir başvuru (32 onaltılık basamak ile) sayılardır. 

İzleme için GUID'leri oluşturmak için bir GUID Oluşturucu kullanmanız gerekir. Azure depolama ekibi oluşturan bir [GUID generator form](https://aka.ms/StoragePartners) bir GUID biçimi, e-posta gönderilir ve farklı izleme sistemleri arasında yeniden kullanılabilir.

Her bir teklif ve dağıtım kanalı her ürün için benzersiz bir GUID oluşturun öneririz. Bölünecek raporlama istemiyorsanız ürünün birden çok dağıtım kanalları için tek bir GUID kullanmayı tercih edebilirsiniz.

Bir şablonu kullanarak bir ürünün dağıtılmasını ve Azure Marketi'nde ve GitHub üzerinde kullanılabilir, oluşturabilir ve 2 ayrı GUID'ler kaydedin:

*   Azure Marketi'nde bir ürün
*   Github'da bir ürün

Raporlama GUID'leri ve iş ortağı değeri (Microsoft iş ortağı kimliği) ile gerçekleştirilir. Ayrıca, GUID'leri teklifinizi içindeki her bir planı çizerek daha ayrıntılı bir düzeyde izleyebilirsiniz.

Daha fazla bilgi için [GUID'leri SSS ile Azure izleme müşteri kullanımını](https://docs.microsoft.com/azure/marketplace/azure-partner-customer-usage-attribution#faq).



## <a name="multi-user-account-management"></a>Birden çok kullanıcı hesabı Yönetimi

İş ortağı merkezi yararlanır [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis) (Azure AD) birden çok kullanıcı hesabı erişim ve yönetim. Kuruluşunuzun Azure AD, iş ortağı merkezi hesabınızla otomatik olarak ilişkili kayıt işleminin bir parçası olarak. 

## <a name="manage-users"></a>Kullanıcıları yönetme

**Kullanıcılar** iş ortağı merkezi bir bölümünü (altında **hesap ayarları**) kullanıcıları, grupları ve iş ortağı merkezi hesabınız erişimi olan bir Azure AD uygulamaları yönetmek için Azure AD kullanalım. Kullanıcıları yönetme, ile imzalanmış olması gerektiğini unutmayın, [iş hesabı](./company-work-accounts.md) (ilişkili Azure AD kiracısı). Farklı bir iş hesabı içinde kullanıcıları yönetmek için / Kiracı, ihtiyaç duyacağınız sahip bir kullanıcı olarak yeniden oturum açın veya oturumu kapatıp açmanız **Manager** izinleri, iş hesabı / Kiracı. 

İş hesabınızla (Azure AD kiracısı) imzalandıktan sonra şunları yapabilirsiniz:
- [Kullanıcı eklemek veya kaldırmak](#add-or-remove-users)
- [Bir kullanıcı parolasını değiştirme](#change-a-user-password)
- [Grupları ekleyebilir veya kaldırabilirsiniz](#add-or-remove-users)
- [Azure AD uygulamaları Ekle Kaldır](#add-new-azure-ad-applications)
- [Azure AD uygulaması için anahtarları yönetme](#manage-keys-for-an-azure-ad-application)
- [Kullanıcı rolleri ve izinleri tanımlama](#define-user-roles-and-permissions)


(Grupları ve Azure AD uygulamaları dahil) tüm iş ortağı Merkezi kullanıcıların etkin iş hesabı olması gerektiğini aklınızda bulundurun bir [Azure AD kiracısı](#manage-tenants) iş ortağı merkezi hesabınız ile ilişkili. 

### <a name="add-or-remove-users"></a>Kullanıcı eklemek veya kaldırmak

Hesabınızın olması gerekir [ **yönetici düzeyinde** ](#define-user-roles-and-permissions) izinlerini [iş hesabı (Azure AD kiracısı)](./company-work-accounts.md) eklemek veya kullanıcıları düzenlemek istediğiniz.

#### <a name="add-existing-users"></a>Mevcut kullanıcıları ekleme

Şirket içinde zaten mevcut iş ortağı merkezi hesabınız kullanıcılar eklemek için [iş hesabı (Azure AD kiracısı)](./company-work-accounts.md):

1. Git **kullanıcılar** (altında **hesap ayarları**) seçip **kullanıcı ekleme**.
2. Açılan listeden bir veya daha fazla kullanıcı seçin. Belirli kullanıcıları aramak için arama kutusunu kullanabilirsiniz.
* İş ortağı merkezi hesabınıza eklemek için birden fazla kullanıcı seçerseniz, bunları aynı rol veya özel izinler kümesini atamanız gerekir. Farklı roller/izinlerine sahip birden çok kullanıcı eklemek için her rol ya da özel bir izin kümesi için bu adımları yineleyin.
3.  Kullanıcı seçme işlemini tamamladığınızda, tıklayın **seçili**.
5.  İçinde **rolleri** bölümünde, seçili kullanıcılar için özelleştirilmiş izinleri ve rolleri belirtin.
6.  **Kaydet**’i seçin.

#### <a name="create-new-users"></a>Yeni kullanıcı oluşturma

Yeni kullanıcı hesapları oluşturmak için bir hesapla sahip [ **genel yönetici** ](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) izinleri. 

1. Git **kullanıcılar** (altında **hesap ayarları**) seçin **kullanıcı ekleme**, ardından **Create new users**.
1. Her yeni bir kullanıcı için bir ad, Soyadı ve kullanıcı adı girin. 
1. Yeni bir kullanıcı, kuruluşunuzun dizininde bir genel yönetici hesabına sahip olmasını istiyorsanız, başlıklı kutuyu işaretleyin **bu kullanıcının genel yönetici Azure ad, tüm dizin kaynakları üzerinde tam denetime sahip olun.** . Bu, tüm yönetim özelliklerine şirketinizde kullanıcının Azure AD kullanıcı tam erişim sağlar. İçinde olmayan bir iş ortağı merkezi de, hesap uygun rol/izin vermediği sürece kullanıcılar eklemek ve kuruluşunuzun iş hesabı (Azure AD kiracısı) kullanıcıları yönetmek mümkün olacaktır. 
1. İçin kutusunu işaretlediyseniz **bu kullanıcının genel yönetici olun**, sağlamak ihtiyacınız olacak bir **parola kurtarma e-posta** kullanıcının parolasını gerekiyorsa, kurtarılır.
1. İçinde **grup üyeliği** bölümünde, yeni bir kullanıcının ait olmasını istediğiniz grupları seçin.
1. İçinde **rolleri** bölümünde, rolleri veya özelleştirilmiş kullanıcı izinlerini belirtin.
1. **Kaydet**’i seçin.

İş ortağı Merkezi'nde yeni bir kullanıcı oluşturma da söz konusu kullanıcı için bir hesap için oturum iş hesabı (Azure AD kiracısı) oluşturur. İş ortağı Merkezi'nde bir kullanıcı adı için değişiklik yapmadan kuruluşunuzun iş hesabı (Azure AD kiracısı) aynı değişiklikleri yapar.

#### <a name="invite-new-users-by-email"></a>Yeni e-posta ile davet

Şirket iş hesabınızı (Azure AD kiracısı) e-posta yoluyla bir parçası olmayan kullanıcıları davet etmek için bir hesapla sahip [ **genel yönetici** ](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) izinleri. 

1. Gidin **kullanıcılar** (altında **hesap ayarları**) seçin **kullanıcı ekleme**, ardından **kullanıcıların e-posta ile davet**.
2. Bir veya daha fazla e-posta girin (en fazla on), adresi, virgül veya noktalı virgülle ayrılmış.
3. İçinde **rolleri** bölümünde, rolleri veya özelleştirilmiş kullanıcı izinlerini belirtin.
4. **Kaydet**’i seçin.

Davet ettiğiniz kullanıcılar, iş ortağı merkezi hesabınız katılmak için bir e-posta davetiyesi alırsınız. İş hesabınızı (Azure AD kiracısı) yeni Konuk kullanıcı hesabı oluşturulur. Her kullanıcı hesabınızı erişebilmeniz için önce kendi kendisinden daveti kabul etmesi gerekir.

Davetiye yeniden göndermeniz gerekiyorsa, ziyaret **kullanıcılar** sayfasında kullanıcı listesinin davet bulur, e-posta adresi seçin (veya yazılı metni *bekleyen davet*). Ardından, sayfanın en altında **daveti yeniden gönder**.
 

> [!NOTE]
> Kuruluşunuz kullanıyorsa [dizin tümleştirme](https://go.microsoft.com/fwlink/p/?LinkID=724033) şirket içi dizininizde Azure AD ile eşitlemek için iş ortağı Merkezi'nde yeni kullanıcıları, grupları veya Azure AD uygulamaları oluşturmak mümkün olmayacaktır. (Veya şirket içi dizininizdeki başka bir yönetici) görebilir ve bunları iş ortağı Merkezi'nde ekleme olacaksınız önce bunları doğrudan şirket içi dizin oluşturmanız gerekir.

#### <a name="remove-a-user"></a>Kullanıcıyı kaldırma

İş hesabınızı (Azure AD kiracısı) bir kullanıcıyı kaldırmak için şu adrese gidin **kullanıcılar** (altında **hesap ayarları**) seçeneğini en sağdaki sütunda onay kutularını kullanarak kaldırmak istediğiniz kullanıcı seçin **Kaldırma** kullanılabilir eylemlerine. Bir açılır pencere Seçili kullanıcıları silmek istediğinizi onaylamak için görünür.

#### <a name="change-a-user-password"></a>Bir kullanıcı parolasını değiştirme

Kullanıcılarınız parolalarını değiştirmek gerekiyorsa, bunu kendilerini sağlandıysa bir **parola kurtarma e-posta** kullanıcı hesabını oluştururken. Aşağıdaki adımları izleyerek bir kullanıcının parolasını da güncelleştirebilirsiniz. Şirket iş hesabınızı (Azure AD kiracısı) bir kullanıcının parolasını değiştirmek için hesap ile oturum açmanız gerekir [ **genel yönetici** ](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) izinleri. Bu Azure AD kiracınızda kullanıcının parolasını değiştirir, iş ortağı merkezine erişmek için kullandıkları parolanın yanı sıra dikkat edin.

1.  Gelen **kullanıcılar** sayfa (altında **hesap ayarları**), düzenlemek istediğiniz kullanıcı hesabının adını seçin.
2.  Seçin **parolayı Sıfırla** sayfanın alt kısmındaki düğmesi.
3.  Geçici bir parola da dahil olmak üzere, kullanıcı için oturum açma bilgilerini gösteren bir onay sayfası görüntülenir. Bu sayfadan çıktıktan sonra geçici parolayı erişmek mümkün olmayacaktır olarak yazdırma veya bu bilgileri kopyalayıp kullanıcıya sağlamak emin olun.


## <a name="manage-groups"></a>Grupları yönetme

Grupları, birden çok kullanıcı rolleri ve izinleri hep birlikte denetlemenizi sağlar.

#### <a name="add-an-existing-group"></a>Varolan bir grubu Ekle

Bir grup zaten kuruluşunuzun iş hesabı (Azure AD kiracısı) iş ortağı merkezi hesabınız var olan eklemek için: 

1.  Gelen **kullanıcılar** sayfa (altında **hesap ayarları**) seçeneğini **grupları Ekle**.
2.  Açılan listeden bir veya daha fazla grup seçin. Belirli gruplar için aramak için arama kutusunu kullanabilirsiniz.
İş ortağı merkezi hesabınıza eklemek için birden fazla grup seçin, bunları aynı rol veya özel izinler kümesini atamanız gerekir. Farklı roller/izinlerine sahip birden çok grubu eklemek için her rol ya da özel bir izin kümesi için bu adımları yineleyin.
3.  Grupları seçme işiniz bittiğinde, tıklayın **seçili**.
4.  İçinde **rolleri** bölümünde, rolleri veya seçilen gruplar için özelleştirilmiş izinleri belirtin. Grubun tüm üyeleri gruba, tek tek hesaplarıyla ilişkili izinleri ve rolleri bakılmaksızın uygulama iş ortağı merkezi hesabınız izinlerle erişmek mümkün olacaktır.
5.  **Kaydet**’i seçin.

Varolan bir grup eklediğinizde, bu grubun bir üyesi olan her bir kullanıcı grubun atanmış rolüyle ilişkilendirilmiş izinleri ile iş ortağı merkezi hesabınız erişmek mümkün olacaktır.

#### <a name="add-a-new-group"></a>Yeni grubu eklemek

İş ortağı merkezi hesabınız için yeni bir grup eklemek için: 

1.  Gelen **kullanıcılar** sayfa (altında **hesap ayarları**) seçeneğini **grupları Ekle**.
2.  Sonraki sayfada seçin **yeni grup**.
3.  Yeni grup için görünen ad girin.
4.  Rolleri veya özelleştirilmiş grup izinlerini belirtin. Grubun tüm üyelerinin burada bireysel hesaplarıyla ilişkili roller/izinlere bakılmaksızın uygulama iş ortağı merkezi hesabınız izinlerle erişmek mümkün olacaktır.
5.  Kullanıcılar yeni grup için görünen listeden seçin. Belirli kullanıcıları aramak için arama kutusunu kullanabilirsiniz.
6.  Kullanıcı seçme işlemini tamamladığınızda, tıklayın **seçili** yeni grubuna eklenecek.
7.  **Kaydet**’i seçin.

Bu yeni grubu, kuruluşunuzun iş hesabı (Azure AD kiracısı) de yalnızca iş ortağı merkezi hesabınız oluşturulacağını unutmayın.

#### <a name="remove-a-group"></a>Bir grubu Kaldır

İş hesabınızı (Azure AD kiracısı) bir grubu kaldırmak için şu adrese gidin **kullanıcılar** (altında **hesap ayarları**) seçeneğini en sağdaki sütunda onay kutularını kullanarak kaldırmak istediğiniz grup seçin **Kaldırma** kullanılabilir eylemlerine. Bir açılır pencere sizin için seçilen gruplar kaldırmak istediğinizi onaylamak görünür.

## <a name="manage-azure-ad-applications"></a>Azure AD uygulamaları yönetme

Şirketinizin Azure parçası olan uygulamaları veya hizmetleri izin iş ortağı merkezi hesabınıza erişmek için AD. 

#### <a name="add-existing-azure-ad-applications"></a>Mevcut Azure AD uygulamaları ekleme 

Uygulamaları, eklemek için şirketinizin Azure Active Directory'de zaten mevcut: 

1.  Gelen **kullanıcılar** sayfa (altında **hesap ayarları**) seçeneğini **Azure AD uygulamaları ekleme**.
2.  Bir veya daha fazla Azure AD uygulamaları görünen listeden seçin. Özel aramak için arama kutusunu kullanın Azure AD uygulamaları. İş ortağı merkezi hesabınıza eklemek için birden fazla Azure AD uygulaması seçerseniz, bunları aynı rol veya özel izinler kümesini atamanız gerekir. Farklı roller/izinlerine sahip birden çok Azure AD uygulama eklemek için her rol ya da özel bir izin kümesi için bu adımları yineleyin.
3.  Azure AD uygulamaları seçerek işiniz bittiğinde, tıklayın **seçili**.
5.  İçinde **rolleri** bölümünde, rolleri veya seçili özelleştirilmiş izinlerini belirtin. Azure AD uygulamaları.
6.  **Kaydet**’i seçin.

#### <a name="add-new-azure-ad-applications"></a>Ekleme yeni Azure AD uygulamaları 

İçin yeni bir Azure iş ortağı Merkezi erişim vermek istiyorsanız AD uygulama hesabı oluşturabilirsiniz birinde **kullanıcılar** bölümü. Bu şirket iş hesabınızı (Azure AD kiracısı) yeni bir hesap iş ortağı merkezi hesabınızda yalnızca oluşturur unutmayın. Bu Azure AD uygulaması iş ortağı merkezi kimlik doğrulaması için öncelikle kullanıyorsanız ve doğrudan erişmek için kullanıcıların gerekmez, herhangi bir geçerli adresi girebilirsiniz **yanıt URL'si** ve **uygulama kimliği URI'si**, uzun Bu değer olarak, dizininizdeki başka bir Azure AD uygulama tarafından kullanılmaz.

1.  Gelen **kullanıcılar** sayfa (altında **hesap ayarları**) seçeneğini **Azure AD uygulamaları ekleme**.
2.  Sonraki sayfada seçin **yeni Azure AD uygulama**.
3.  Girin **yanıt URL'si** yeni Azure AD uygulaması. Bu kullanıcılar burada oturum açabilir ve (uygulama URL'si veya oturum açma URL'si bazen olarak da bilinir), Azure AD uygulamasını kullanın URL'dir. **Yanıt URL'si** 256 karakterden uzun olamaz ve dizininizde benzersiz olmalıdır.
4.  Girin **uygulama kimliği URI'si** yeni Azure AD uygulaması. Bu, Azure AD'ye tek bir oturum açma isteği gönderildiğinde, sunulan Azure AD uygulaması için mantıksal bir tanımlayıcıdır. Unutmayın **uygulama kimliği URI'si** dizininizdeki her bir Azure AD uygulama için benzersiz olmalıdır. Bu kimliği, 256 karakterden uzun olamaz. Uygulama Kimliği URI'si hakkında daha fazla bilgi için bkz. [uygulamaları Azure Active Directory ile tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/quickstart-modify-supported-accounts#change-the-application-registration-to-support-different-accounts).
5.  İçinde **rolleri** bölümünde, Azure AD uygulaması için özelleştirilmiş izinleri ve rolleri belirtin.
6.  **Kaydet**’i seçin.

Ekleme veya bir Azure AD uygulaması oluşturma sonra dönebilirsiniz **kullanıcılar** bölümünde ve Kiracı kimliği, istemci kimliği, yanıt URL'si ve uygulama kimliği URI'Sİ'dahil olmak üzere, uygulamanın ayarlarını gözden geçirmek için uygulama adını seçin.

#### <a name="remove-an-application"></a>Uygulamayı kaldırma

İş hesabınızı (Azure AD kiracısı) bir uygulamayı kaldırmak için şu adrese gidin **kullanıcılar** (altında **hesap ayarları**), sağdaki sütundaki onay kutularını kullanarak kaldırmak istediğiniz uygulamayı seçin ardından **Kaldır** kullanılabilir eylemlerine. Bir açılır pencere sizin için seçilen uygulamaları kaldırmak istediğinizi onaylamak görünür.

#### <a name="manage-keys-for-an-azure-ad-application"></a>Azure AD uygulaması için anahtarları yönetme

Azure AD uygulamanız okur ve verileri Microsoft Azure AD yazar, bunu bir anahtar gerekir. İş ortağı Merkezi'nde bilgilerini düzenleyerek, bir Azure AD uygulaması için anahtarları oluşturabilirsiniz. Ayrıca, artık gerekli anahtarları kaldırabilirsiniz.

1.  Gelen **kullanıcılar** sayfa (altında **hesap ayarları**), Azure AD uygulama adını seçin. Tüm anahtar oluşturulduğu ve ne zaman dolacağını tarihi dahil olmak üzere Azure AD uygulaması için etkin anahtarlar görürsünüz. 
2. Artık gerekli bir anahtarı kaldırmayı seçin **Kaldır**.
3.  Yeni bir anahtar eklemek için seçin **yeni anahtar Ekle**.
4.  Bir ekran gösterme göreceğiniz **istemci kimliği** ve **anahtar değerlerini**. Bu sayfadan çıktıktan sonra yeniden erişmek mümkün olmayacaktır olarak yazdırma veya kopyalama bu bilgileri emin olun.
4.  Daha fazla anahtarları oluşturmak istiyorsanız seçin **başka bir anahtar ekleyin**.


### <a name="define-user-roles-and-permissions"></a>Kullanıcı rolleri ve izinleri tanımlama

Aşağıdaki rol ve iş ortağı merkezi ticari Market program izinlerin şirketinizin kullanıcılara atanabilir. 

Azure Active Directory (AAD) Kiracı roller genel yönetici, yönetici kullanıcı ve CSP rolleri dahil unutmayın. Olmayan AAD rollerini kiracıyı yönetme olmayan roller ve MPN yönetim, iş profili yönetici, başvuru Yöneticisi, no'lu tebliğ kapsamında teşvik yönetici ve no'lu tebliğ kapsamında teşvik kullanıcı içerirler.


|**Rol**|**İzinler**|
|----------------------------------|:---------------------------------|
|Genel yönetici|• Tam ayrıcalıklarına sahip tüm Microsoft hesabı/hizmetlerine erişebilir.
|      |• İş ortağı merkezi destek bileti oluşturma
||• Görünümü sözleşmeleri, fiyat listeleri ve teklifler
||• Görüntüleme, oluşturma ve iş ortağı kullanıcıları yönetme|
|Yöneticisi|• Haricinde vergi ve ödeme ayarları tüm Microsoft hesabı özelliklere erişebilirsiniz
|      |• Kullanıcıları ve rolleri yönetme ve iş hesaplarını (kiracılar)|
|Geliştirici|• Paketleri yükle, uygulamalar ve eklentiler gönderin ve telemetri Ayrıntılar için kullanım raporunu görüntüle
|      |• Finansal bilgi veya hesap ayarları erişemiyor|
|İş katkıda bulunan|• Finansal bilgilere erişebilir ve fiyatlandırma ayrıntılarını ayarlayın
|      |• Oluşturamaz veya yeni uygulamalar ve eklentiler gönderin|
|Finansal katkıda bulunan|• Ödeme raporları görüntüleyebilirsiniz.
|      |• Uygulamaları veya ayarlarında değişiklik yapamaz|
|Pazarlamacılar|• Finansal olmayan raporlar ve müşteri incelemeleri ile yanıtlayabilir.
|      |• Uygulamaları veya ayarlarında değişiklik yapamaz|

Bulut çözümü sağlayıcısı (CSP), Denetim Masası satıcı (CPV), Konuk kullanıcılar veya Microsoft iş ortağı ağı (MPN), rolleri ve izinleri gibi Azure Active Directory (AD), iş ortağı Merkezi, diğer alanlarında yönetme hakkında daha fazla bilgi için bkz [atayın Kullanıcı rolleri ve izinleri iş ortağı Merkezi'nde](https://docs.microsoft.com/partner-center/permissions-overview).


## <a name="manage-tenants"></a>Kiracılar'ı yönetme

Bu belge boyunca "iş hesabınıza" olarak da adlandırılan bir Azure Active Directory (AD) kiracısı bir gösterimi, kuruluşunuzun Azure portalında ayarlama ve Microsoft bulut Hizmetleri için dahili belirli bir örneğini yönetmenize yardımcı olur olduğu ve dış kullanıcılar. Kuruluşunuzun Azure, Microsoft Intune veya Office 365 gibi bir Microsoft bulut hizmetine abone, Azure AD kiracısı için kuruldu. 

İş ortağı Merkezi ile kullanmak için birden çok kiracılar ayarlayabilirsiniz. Herhangi bir kullanıcıyla **Manager** iş ortağı merkezi hesabınız rolü ekleme ve Azure AD kiracılarıyla hesaptan Kaldır seçeneğine sahip olur.  

### <a name="add-an-existing-tenant"></a>Mevcut bir kiracısı ekleme

Başka bir Azure AD kiracısı, iş ortağı merkezi hesabınızla ilişkilendirmek için:

1.  Gelen **kiracılar** sayfa (altında **hesap ayarları**) seçeneğini **başka bir Azure AD kiracısı ilişkilendirme**.
2. İlişkilendirmek istediğiniz kiracısı için Azure AD kimlik bilgilerinizi girin.
3. Azure AD kiracınız için kuruluş ve etki alanı adını gözden geçirin. İlişkilendirme tamamlanması seçin **Onayla**.

İlişkilendirme başarılı olursa, ardından ekleyin ve hesap kullanıcıları yönetmek hazır olmayacak **kullanıcılar** iş ortağı Merkezi bölümünde.

### <a name="create-a-new-tenant"></a>Yeni Kiracı oluşturma

İş ortağı merkezi hesabınız ile bir marka yeni Azure AD kiracısı oluşturmak için:

1.  Gelen **kiracılar** sayfa (altında **hesap ayarları**) seçeneğini **oluştur yeni bir Azure AD kiracısı**.
2. Yeni Azure AD dizini bilgilerini girin:
    - **Etki alanı adı**: Azure AD etki alanınız için birlikte kullanacağız benzersiz bir ad ". onmicrosoft.com". Örneğin, "örnek" girdiyseniz, Azure AD etki alanınızı "example.onmicrosoft.com" olacaktır.
    - **İlgili kişi e-posta**: Sizinle nereden hesabınızda gerekirse iletişim bir e-posta adresi.
    - **Genel yönetici kullanıcı hesabı bilgileri**: Ad, son adı, kullanıcı adı ve yeni genel yönetici hesabı için kullanmak istediğiniz parola.
3. Seçin **Oluştur** yeni etki alanı ve hesabı bilgileri onaylamak için.
4. Oturum açın, yeni Azure AD genel yönetici kullanıcı adı ve parola başlamak için [kullanıcıları ekleme ve yönetme](#manage-users).

İş ortağı merkezi portalında aracılığıyla değil, Azure portal'ınızın içinde yeni kiracıları oluşturma hakkında daha fazla bilgi için bkz [Azure Active Directory'de yeni bir kiracı oluşturmak](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-access-create-new-tenant).

### <a name="remove-a-tenant"></a>Bir kiracı Kaldır

Bir Kiracı İş ortağı merkezi hesabınızdan kaldırmak için şirket adını bulun **kiracılar** sayfa (içinde **hesap ayarları**), ardından **Kaldır**. Kiracıyı kaldırmak istediğinizi onaylamanız istenir. Bunu yaptıktan sonra hiçbir kullanıcı, kiracıda iş ortağı merkezi hesabınız ile oturum açabilir ve bu kullanıcılar için yapılandırmış olduğunuz herhangi bir izni kaldırılacak.

Bir kiracı kaldırdığınızda, iş ortağı merkezi hesabınız için o kiracıdan eklenen tüm kullanıcılar artık hesaplarında oturum açabilir mümkün olacaktır.

> [!TIP]
> İş ortağı Merkezi ile aynı kiracıda bir hesabı kullanarak şu anda oturum açmış bir kiracı kaldıramazsınız. Bir kiracı kaldırmak için iş ortağı merkezi oturum açmalısınız bir **Manager** hesapla ilişkilendirilmiştir: başka bir kiracı için. Yalnızca tek bir kiracı hesabı ile ilişkili ise, söz konusu kiracıyı yalnızca Microsoft hesabıyla oturum açılan hesap imzaladıktan sonra kaldırılabilir.


## <a name="agreements"></a>Sözleşmeler

**Sözleşmeleri** iş ortağı merkezi bir bölümünü (altında **hesap ayarları**) yetki verdiğiniz yayımlama anlaşmaları listesi şimdi görebilirsiniz. Bu sözleşmeler, adını, sürüm numarası, kabul edildi tarihi gibi ve anlaşma kabul kullanıcının adını göre listelenir. 

**Gerekli eylemleri** dikkat etmeniz gereken sözleşmesi güncellemeleri varsa bu sayfanın en üstünde şu şekilde görünebilir. Güncelleştirilmiş bir sözleşmeyi kabul etmek için önce bağlı sözleşmesi sürümünü okuyun ve ardından **sözleşmesini kabul**. 

Bulut çözümü sağlayıcısı (CSP) iş ortağı merkezi sözleşmeleri hakkında daha fazla bilgi için ziyaret [bölge ve dil tarafından Microsoft bulut anlaşmaları](https://docs.microsoft.com/partner-center/agreements).

## <a name="next-steps"></a>Sonraki adımlar

- [Yeni bir SaaS teklifi oluşturma](./create-new-saas-offer.md)
