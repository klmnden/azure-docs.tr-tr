---
title: Ticari Market'te yeni SaaS teklifi oluşturma
description: Liste veya Azure Market, AppSource, ya da Microsoft Partner Center üzerinde ticari Market portal'ı kullanarak bulut çözümü sağlayıcısı (CSP) programı aracılığıyla satış için bir hizmet (SaaS) teklifi olarak yeni bir yazılım oluşturma
author: mattwojo
manager: evansma
ms.author: mattwoj
ms.service: marketplace
ms.topic: conceptual
ms.date: 05/30/2019
ms.openlocfilehash: f2787cd74525e7676befb133a6106ce83d9c2a20
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67072627"
---
# <a name="create-a-new-saas-offer"></a>Yeni bir SaaS teklifi oluşturma

Hizmet (SaaS) sunduğu yazılım oluşturmaya başlamak için olun, ilk [bir iş ortağı merkezi hesabınız oluşturma](./create-account.md) açın [ticari Marketplace panosunu](https://partner.microsoft.com/dashboard/commercial-marketplace/offers), ile **genelbakış** sekmesi seçili.

![Market iş ortağı merkezi Panoda ticari](./media/new-offer-overview.png)

Seçin + **yeni oluştur...** düğmesini ve ardından **hizmet olarak yazılım** menü öğesi. 

Bir teklif türlerinden birini seçerseniz, eski yönlendirilecek [bulut iş ortağı portalı](https://cloudpartner.azure.com/).  SaaS teklifleri yalnızca şu anda ticari Market iş ortağı merkezi portalında kullanılabilir. 

![İş ortağı Merkezi'nde teklif penceresi oluştur](./media/new-offer-click.png)

**Yeni teklif** iletişim kutusu görüntülenir. 

![Yeni Teklif iletişim kutusu](./media/new-offer-popup.png)


## <a name="offer-id-and-name"></a>Teklif kimliği ve adı

- **Teklif kimliği**: Her teklif için benzersiz bir tanımlayıcı hesabınızı oluşturun. Bu kimliği, URL adresinin Market teklifi ve Azure Resource Manager şablonları (varsa) müşteriler için görünmez. Teklif kimliği, küçük harf, alfasayısal (kısa çizgi ve alt çizgi, ancak hiçbir boşluk dahil) olmalıdır. 50 karakterle sınırlıdır ve seçtikten sonra güncelleştirilemiyor oluşturun.  
Örnek: test-Teklif-1
<br>URL'de ortaya çıkarıyordu: `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1`

- **Teklif adı**: SaaS uygulama teklifinizi, yayın, reklamları ve web siteleri arasında tutarlı resmi adı.  Bu ad trademarked.  (Simge ticari marka veya telif hakkı olmadıkları sürece) adı boşluk, emoji içermemelidir sunar ve 50 karakter uzunluğunda olmalıdır.
<br>Örnek: Test teklifini 1&#8482;

**Oluştur**’u seçin.  Bir **genel bakış sunan** sayfası, bu teklif için oluşturulur.  

![İş ortağı merkezine genel bakış sunar.](./media/commercial-marketplace-offer-overview.png)

## <a name="offer-overview"></a>Teklif genel bakış

**Genel bakış sunan** sayfa içerir: 

- **Yayımlama durumu** Bu teklif ve her bir adımın tamamlanması ne kadar sürer yayımlamak için gerekli adımları görsel bir temsilini görüntüler. Tamamlanmamış yayımlama adım simgeler gri görünür. 

- **Genel bakış sunan** menüsü, bu teklif işlemleri gerçekleştirmek için bağlantılar listesini içerir. Bu işlemlerin listesini teklifiniz için yaptığınız seçime göre değişir.  
    - Teklif taslağı – silme taslak ise 
    - Teklif Canlı – ise durdurma satış teklifi 
    - Teklif Go-live – Önizleme aşamasındaysa 
    - Yayımcı oturumu kapatma – tamamlamadıysanız yayımlamayı iptal et

## <a name="offer-setup"></a>Teklif kurulumu

**Teklif Kurulum** sekmesine aşağıdaki bilgileri ister. Seçin **Kaydet** bu alanlar tamamladıktan sonra.

- **Microsoft satış yapmak ister misiniz?** (Evet/Hayır)
    - **Evet**, teklifinizi Microsoft, Microsoft sizin adınıza; Market işlem barındırma aracılığıyla satış yapmak ister misiniz veya 
    - **Hayır**, sadece teklifinizi marketleri aracılığıyla listelemek Microsoft bağımsız olarak tüm parasal işlemleri tercih.    

### <a name="sell-through-microsoft"></a>Microsoft satış yapın

Daha iyi müşteri bulma ve edinme, sağlar, Microsoft satış verir Microsoft sizin adınıza konak Market işlemleri ve Microsoft'un küresel olarak kullanılabilir ticaret özelliklerinden yararlanır.

#### <a name="saas-offer-requirements"></a>SaaS teklifi gereksinimleri

Ticari Market iş ortağı Merkezi'nde ile hizmet (SaaS) sunduğu yazılım listelemek için aşağıdaki ölçütleri karşılanmalıdır:

- Teklifinizi kullanmalısınız [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/) kimlik yönetimi ve kimlik doğrulaması için.
- Teklifinizi kullanmalısınız [SaaS yerine getirme API'leri](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-fulfillment-api-v2) Azure Marketi ile tümleştirme.
- Daha kapsamlı gereksinimleri için bkz [SaaS teklifi yayımlama Kılavuzu](https://docs.microsoft.com/azure/marketplace/marketplace-saas-applications-technical-publishing-guide).

#### <a name="saas-on-azure-billing-infrastructure-costs"></a>SaaS altyapı maliyetleri Azure faturalama
SaaS teklifi Azure'da barındırılıyorsa, yayımcı olarak kullanım ücretleri Azure altyapı ve yazılım lisans ücretleri için tek maliyeti öğesi olarak dikkate alması gerekir. Bu maliyet müşteri için aylık sabit ücret gösterilir. Azure altyapı kullanım yönetilir ve doğrudan sizin için iş ortağı üzerinden faturalandırılırsınız. Gerçek altyapı kullanım ücretleri müşteri tarafından görülmez. Yayımcılar Azure altyapı kullanım ücretleri, yazılım lisans fiyatına paket genellikle tercih etme. 

Yazılım Lisans ücretleri aylık, yinelenen site tabanlı abonelik sabit fiyat ücret gösterilir ve ölçülmez veya tüketim temelli.

|**Lisansınızı maliyeti**|**Aylık 100 ABD Doları**|
|:---|:---|
|Azure kullanım maliyeti (D1/1-çekirdek)|Doğrudan yayımcıya, müşteri faturalandırılır|
|Müşteri, Microsoft tarafından faturalandırılır|100,00 $ / ay (yayımcı hesabı tahakkuk veya doğrudan altyapısı maliyetlerin lisans ücreti için gerekir)|

- Bu senaryoda, Microsoft yazılım lisansı için 100,00 $ düzenler ve 80.00 out yayımcıya öder.
- İçin iş ortaklarıyla **sınırlı bir Market hizmeti ücreti** Haziran 2020 kadar Mayıs 2019'nden bir SaaS azaltılmış işlem ücreti sunar görürsünüz. Bu senaryoda, Microsoft yazılım lisansı için 100,00 $ düzenler ve 90.00 out yayımcıya öder.

> [!NOTE]
> **Bir Market hizmeti ücreti azaltıldı**: Belirli SaaS ticari Marketimizden yayımladığınız, Microsoft, Market hizmeti ücreti %20 değerinden (Microsoft yayımcı anlaşması'nda açıklandığı gibi) % 10 azaltacak sunar. Teklifiniz nitelemek için sırada tekliflerinizi en az biri Microsoft tarafından IP ortak satışa hazır ya da IP ortak satış öncelikli olacak şekilde atanmış olmalıdır.  Bu sınırlı bir Market hizmeti ücreti ay için almak için uygunluk en az beş (5) iş günü her takvim ayının sonundan önce karşılanması gerekir.  VM'ler, yönetilen uygulamalar veya ticari Marketimizden kullanıma sunulan diğer ürünler için sınırlı bir Market hizmeti ücreti uygulanmaz.  Sınırlı bir Market hizmeti ücreti yalnızca tam teklifleri 1 Mayıs 2019 ve 30 Haziran 2020 arasındaki Microsoft tarafından toplanan lisans ücretleri için kullanılabilir.  Bu tarihten sonra normal tutarı için bir Market hizmeti ücreti döndürür. 

|**Microsoft faturalar**|**Aylık 100 ABD Doları**|
|:---|:---|
|Microsoft, lisans maliyeti 80 oranında öder <br>**Tam SaaS uygulamaları için Microsoft lisans maliyetinizin %90 öder.*|80\.00 başına aylık <br>*$* her ay * 90.00|


#### <a name="csp-program-opt-in"></a>CSP programı kabul etme
[Bulut çözümü sağlayıcısı (CSP)](https://docs.microsoft.com/azure/marketplace/cloud-solution-providers) program sağlayan yazılım teklifleri değişikliğiyle uygun Microsoft müşterileri milyonlarca ulaşmak pazarlama ve satış yatırım.

- **Kanallar: My teklif CSP programı kullanılabilmesini** (onay kutusu)

CSP programı teklifinizi kullanılabilmesi seçme, bulut çözümü sağlayıcıları, ürün ile birlikte gelen bir çözümün bir parçası, müşterilerine satmak sağlar. 

### <a name="list-through-microsoft"></a>Microsoft eşlemeleri

Market kaydı oluşturarak Microsoft ile işletmenizi yükseltin. Seçme yalnızca, teklif listesi ve Microsoft yazılım lisans işlemlere doğrudan katılmasına değil Microsoft bulunamazsınız transact değil. İlişkili işlem ücreti yok yoktur ve 100 %'lisans ücretleri müşteriden toplanan herhangi bir yazılımın yayımcısına tutar. Ancak, yayımcı dahil ancak bunlarla sınırlı olmamak üzere yazılım lisans işlemin tüm yönlerini desteklemek için sorumludur: siparişi yerine getirme, kullanım ölçümü, faturalama, faturalama, ödeme ve koleksiyonu. 

- **Bu liste teklif ile etkileşim kurmak için potansiyel müşteriye nasıl istiyorsunuz?**

##### <a name="get-it-now-free"></a>Bunu şimdi (ücretsiz)
Teklifinizin müşteriler ücretsiz, uygulamanızın erişebildiği geçerli bir URL (http veya https ile başlayan) sağlayarak listeleyin.  Örneğin, `https://contoso.com/saas-app`

##### <a name="free-trial"></a>Ücretsiz deneme sürümü
Teklifinizin müşteriler ücretsiz bir deneme süresi boyunca, uygulamanızın erişebildiği geçerli bir URL (http veya https ile başlayan) sağlayarak listeleyin.  Örneğin, `https://contoso.com/trial/saas-app`

##### <a name="contact-me"></a>Benimle iletişim kurun
Müşteri İlişkileri Yönetimi (CRM) sisteminizi bağlanarak müşteri iletişim bilgilerini toplayın. Müşteri bilgilerini paylaşmak için izin istenecektir. Teklif adı, Kimliğini ve teklifinizi bulunduğu Market kaynağı yanı sıra bu müşteri ayrıntıları, yapılandırdığınız CRM sistemine gönderilir. CRM'İNİZE yapılandırma hakkında daha fazla bilgi için bkz. [Connect sağlama Yönetimi](#connect-lead-management). 

## <a name="example-marketplace-offer-listing"></a>Örnek Market teklifi listeleme

![Örnek Marketi teklif notları](./media/marketplace-offer.svg)

## <a name="enable-a-test-drive"></a>Bir test sürüşüne etkinleştir

Bir test sürüşüne 'satın almadan önce denemek için ' seçeneği vermeden, artan dönüştürme ve yüksek oranda nitelikli potansiyel müşteriler edinin nesil kaynaklanan olası müşterilere, teklifiniz göstermek için harika bir yoludur. [Test sürüşleri hakkında daha fazla bilgi edinin.](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/what-is-test-drive)

- **Bir test sürüşüne etkinleştirme** (onay) 

Test sürüşü sağlayarak, müşterilerin deneyebilmesi teklifiniz için sabit bir süre bir tanıtım ortamı yapılandırma istenir. 

### <a name="type-of-test-drive"></a>Test sürüşü türü

- **[Azure Resource Manager](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/azure-resource-manager-test-drive)** : Çözümünüzü oluşturan tüm Azure kaynaklarını içeren bir dağıtım şablonu. Bu senaryo uygun ürünler, yalnızca Azure kaynakları kullanır.
- **[Dynamics 365 Business Central için](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cpp-business-central-offer)** : Microsoft barındırır ve sistem bir iş merkezi kurumsal kaynak planlama için (sağlanmasına ve dağıtımına dahil) test sürücü hizmeti tutar (Finans, işlemleri, tedarik zinciri, CRM, vs.).  
- **[Dynamics 365 müşteri katılımı için](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/dyn365ce/cpp-customer-engagement-offer)** : Microsoft barındırır ve (sağlanmasına ve dağıtımına dahil) test sürücü hizmeti Customer Engagement Sistemi'nde (satış, servis, proje hizmeti, saha hizmeti, vb.) korur.  
- **[Operasyonlar için Dynamics 365](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cpp-dynamics-365-operations-offer)** : Microsoft barındırır ve sistem (Finans, işlemler, üretim, tedarik zinciri, vb.) bir finans ve operasyon kurumsal kaynak planlama için (sağlanmasına ve dağıtımına dahil) test sürücü hizmeti tutar. 
- **[Mantıksal uygulama](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/logic-app-test-drive)** : Tüm karmaşık çözüm mimarileri kapsayan bir dağıtım şablonu. Herhangi bir özel ürün, bu tür bir Test Sürüşü kullanmanız gerekir.
- **[Power BI](https://docs.microsoft.com/power-bi/service-template-apps-overview)** : Özel olarak geliştirilmiş bir panoya katıştırılmış bir bağlantı. Test Sürüşü bu tür bir etkileşimli Power BI görsel kullanması gereken göstermek istediğiniz ürünleri. Burada karşıya yüklemek için ihtiyacınız olan, katıştırılmış Power BI URL'si.

#### <a name="additional-test-drive-resources"></a>Ek test sürücü kaynakları
- [Test sürücü teknik en iyi uygulamalar](https://github.com/Azure/AzureTestDrive/wiki/Test-Drive-Best-Practices)
- [Test Sürüşü en iyi pazarlama](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/marketing-and-best-practices)
- [Bir genel bakış çağrı, sürücü test](https://assetsprod.microsoft.com/mpn/azure-marketplace-appsource-test-drives.pdf)

## <a name="connect-lead-management"></a>Müşteri adayı Yönetimi bağlama

Doğrudan marketlerden teklife listeleme ve müşteri vade farkı ifade eder veya dağıtır hemen sonra müşteri iletişim bilgilerini alabilir. böylece, müşteri ilişkileri yönetimi (CRM) sisteminizi takma müşteriler ile bağlantı, Ürün.

- **Bir müşteri adayı hedef seçin** (açılan menü): Nereye müşteri adayları göndermemizi istersiniz CRM sistemine bağlantı ayrıntılarını sağlayın. 

İş ortağı Merkezi sağlama yönetimi için aşağıdaki CRM sistemlerini destekler. Kurulum yönergeleri için bağlantıyı seçin.

- Azure Blob – sağlayan kişi e-posta, kapsayıcı adı ve depolama hesabı bağlantı dizesi. 
- [Azure tablo](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-azure-table) – iletişim e-posta ve depolama hesabı bağlantı dizesi sağlayın. 
- [Dynamics CRM Online](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-dynamics) – iletişim e-posta, URL ve kimlik doğrulaması modu (Office 365 veya Azure Active Directory) sağlar.
- [HTTPS uç noktası](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-https) – iletişim e-posta ve HTTPS uç nokta URL'sini sağlayın. 
- [Marketo](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-marketo) – iletişim e-posta, form kimlik bilgisi, Munchkin hesap kimliği ve sunucu kimliğini girin
- [Salesforce](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-salesforce) -ilgili kişi e-posta ve kuruluş kimliğini girin 

#### <a name="additional-lead-management-resources"></a>Ek müşteri adayı yönetim kaynakları
- [Müşteri adayı Yönetimi SSS](https://docs.microsoft.com/azure/marketplace/lead-management-for-cloud-marketplace#frequently-asked-questions)
- [Sık karşılaşılan müşteri adayı yapılandırma hataları](https://docs.microsoft.com/azure/marketplace/lead-management-for-cloud-marketplace#common-lead-configuration-errors-during-publishing-on-cloud-partner-portal)
- [Yönetim genel bakış bir çağrı cihazı sağlama](https://assetsprod.microsoft.com/mpn/cloud-marketplace-lead-management.pdf)

Unutmayın **Kaydet** sonraki bölüme geçmeden önce!

## <a name="properties"></a>Özellikler
**Özellikleri** sekmesi kategoriler ve teklifinizi marketleri teklifinizi ve uygulamanızı uygulama sürümünü destekleyen yasal sözleşmeleri gruplandırmak için kullanılan sektörler tanımlamak için sorar. 

Seçin **Kaydet** bu alanlar tamamladıktan sonra. 

### <a name="category"></a>Kategori
En az biri bir (1) ve uygun Market arama alanlarına teklifinizi gruplandırmak için kullanılan üç (3) kategorilerin en fazla'ı seçin. Nasıl teklifinizi kategorilerine teklif açıklamasında destekleyip çağırın. 

### <a name="industry"></a>Sektör
Teklifiniz uygun Market arama alanlarına gruplandırmak için kullanılan en fazla iki (2) sektörler seçin. Teklifinizi sektör için belirli durumda değilse, bir seçmeyin. Nasıl teklifinizi seçili sektörler teklif açıklamasında destekleyip çağırın. 

### <a name="app-version"></a>Uygulama sürümü
Bu, AppSource Market'te teklifinizi uygulamanın sürüm sayısını tanımlamak için kullanılan, isteğe bağlı bir alandır. 

### <a name="standard-contract"></a>Standart Sözleşme

- **Standart sözleşme kullanılsın mı?**

Müşteriler için satın alma işlemini basitleştirmek ve yazılım satıcıları için yasal karmaşıklığını azaltmak için Microsoft Market bir işlemde kolaylaştırmaya yardımcı olması için standart Sözleşme şablonunu sunar. 

Özel hüküm ve koşullar oluşturmak yerine, Azure Marketi yayımcılarının yazılımlarını sözleşmesindeki standart Kıdemli ve bir kez kabul etmek için müşterilerin yalnızca ihtiyaç duyan, teklif seçebilirsiniz. 

Standart sözleşme burada bulunabilir: https://go.microsoft.com/fwlink/?linkid=2041178.

#### <a name="terms-of-use"></a>Kullanım koşulları

Lisans koşullarına standart sözleşmeden farklıysa, kendi yasal koşulları kullanım buraya girin seçebilirsiniz. Bu alandaki metin en fazla 10.000 karakter da girebilirsiniz. Kullanım koşullarınızın daha uzun bir açıklama gerekiyorsa, tek bir URL bağlantısı ek lisans koşullarına bulunabileceği bu alana girin. Bu, müşterilere bir etkin bağlantı olarak görüntülenir.

Müşterilerin uygulamanızı deneyebilmeniz için önce bu koşulları kabul etmek için gereklidir. 

Unutmayın **Kaydet** sonraki bölüme geçmeden önce!

## <a name="offer-listing"></a>Teklif Listesi

Teklif sekmesini görüntüler teklifinizi kullanılabilir olduğu dilleri (ve pazarlara) listesi, şu anda Turkish (Turkey) yalnızca mevcut konumdur. Ayrıca, bu sayfa, dile özgü listeleme ve eklendiği tarih/saat durumunu görüntüler. Her dil için (teklif adı, açıklama, arama terimleri, vb.) Market ayrıntıları tanımlamak ihtiyacınız olacak / pazarlayın.

### <a name="offer-listings"></a>Teklif listeleri

Market'te teklifinizi açıklamaları da dahil olmak üzere ve varlıkları pazarlama görüntülenecek ayrıntılarını sağlayın.

- **Ad** (gerekli): Burada tanımlanan adı seçmiş olduğunuz marketplace(s) hakkında teklif başlığı olarak görüntülenir. Ad, bir önceki göre doldurulur **yeni teklif** girişi.  Bu trademarked.  (Semboller ticari marka ve telif hakkı olmadıkları sürece) bu boşluk, emoji içermemelidir ve 50 karakter uzunluğunda olmalıdır.
- **Özet** (gerekli): Market listing(s) arama sonuçlarında kullanılacak teklifinizin kısa bir açıklama sağlayın. Bu alandaki metin en fazla 100 karakter girilebilir.
- **Açıklama** (gerekli): Market listing(s) genel görünümde görüntülenecek teklifinizin bir açıklama sağlayın. Değer teklifi, temel avantajlar, kategori veya endüstri ilişkilendirmeleri, dahil etmeyi düşünün uygulama içi satın alma Fırsatlar, gerekli bildirilmesi ve daha fazla bilgi için bir bağlantı.
Bu alandaki metin en fazla 3000 karakter girilebilir. Ek ipuçları için bkz [harika uygulama açıklaması yazma](https://docs.microsoft.com/windows/uwp/publish/write-a-great-app-description).
- **Arama anahtar sözcükleri**: Müşterilerin teklifinizi marketplace(s) içinde bulmak için kullanabileceğiniz en fazla üç arama anahtar sözcükleri girin.
- **Başlangıç kılavuzuna** (gerekli): Yapılandırma ve potansiyel müşteriler için uygulamanızı kullanmaya başlama işlemi açıklanmaktadır.  Bu hızlı başlangıçta daha ayrıntılı çevrimiçi belgelere bağlantılar içerebilir. Bu alandaki metin en fazla 3000 karakter girilebilir. 

#### <a name="links"></a>Bağlantılar

- **Gizlilik İlkesi** (gerekli): Kuruluşunuzun gizlilik ilkesi bağlantısı. Uygulamanızın gizlilik yasa ve düzenlemelere uygun sağlamaya yönelik ve geçerli gizlilik ilkesi sağlama sorumluluğu size aittir
- **CSP programı pazarlama malzemeleri** (isteğe bağlı): Teklifinizin genişletmek isterseniz, pazarlama malzemeleri için bir bağlantı sağlamanız gereken [bulut çözümü sağlayıcısı (CSP)](https://docs.microsoft.com/azure/marketplace/cloud-solution-providers) program. CSP, Pazar, gruplandırma ve teklifinizi satmak CSP iş ortaklarının geniş bir yetkili müşterilerin teklifinizi genişletir. Bu satıcıları teklifinizi pazarlama malzemeleri için erişime ihtiyacınız olacaktır. Daha fazla bilgi için [Go-To-Market Hizmetleri](https://partner.microsoft.com/reach-customers/gtm).
- **Faydalı bağlantılar** (isteğe bağlı): Uygulamanızı veya sağlayarak listelenen ilgili hizmetler hakkında ek isteğe bağlı çevrimiçi belgeleri bir **başlık** ve **URL**. Tıklayarak ek faydalı bağlantılar ekleme **+ bir URL Ekle**.

#### <a name="contact-information"></a>İletişim bilgileri

- **Kişiler**: Her müşteri iletişim bilgileri için bir çalışan sağlamak **adı** , **telefon numarası**, ve **e-posta** adresi.  (Bu *yapmamayı* görüntülenecek herkese açık şekilde). A **destek URL'si** için de gerekli **destek ilgili kişisi** grubu.  (Bu bilgiyi *olacak* görüntülenecek herkese açık şekilde).

**Destek kişi** (gerekli): Genel destek soruları için.

**İlgili kişi mühendislik** (gerekli): Teknik sorular için.

**Kişi Yöneticisi kanal** (gerekli): CSP programı için satıcı hakkındaki sorularınız için.

#### <a name="files-and-images"></a>Dosya ve görüntüleri

- **Belgeler** (gerekli): Pazarlama ilgili belgeler, teklifiniz için PDF biçiminde, en az biri bir (1) ve üç (3) belge Teklif başına en fazla sağlama ekleyin.
- **Görüntüleri** (isteğe bağlı): Birden fazla yerde ürününüzün logosu görüntüleri aşağıdaki boyutları--küçük gerektiren marketplace(s) görüntülendiği vardır: 48 x 48 piksel _(gerekli)_ Orta: 90 x 90 piksel cinsinden büyük: 216 x 216 piksel _(gerekli)_ geniş: 255 x 115 piksel ve Hero: 815 x 290 piksel. Tüm görüntüleri olması gerekir. PNG biçimi.
- **Ekran görüntüleri** (gerekli): Teklifinizi gösteren ekran görüntüleri ekleyin. En fazla beş (5) ekran görüntüleri eklenebilir ve 1280 x 720 piksel boyutlandırılmalıdır. Tüm görüntüleri olması gerekir. PNG biçimi.
- **Videoları** (isteğe bağlı): Bağlantılar için teklifinizi gösteren video ekleyin. Teklifinizle birlikte müşterilere gösterilir, YouTube ve/veya Vimeo video bağlantıları kullanabilirsiniz. Ayrıca bir küçük resim, video girmeniz gerekir PNG biçiminde 1280 x 720 piksel boyutlu. Teklif başına dört video en fazla görüntüleyebilirsiniz.

Unutmayın **Kaydet** sonraki bölüme geçmeden önce!

#### <a name="additional-marketplace-listing-resources"></a>Ek Market kaynakları listeleme

- [Market için en iyi listelerini sunar.](https://docs.microsoft.com/azure/marketplace/gtm-offer-listing-best-practices)


## <a name="preview"></a>Önizleme

**Önizleme** sekmesi, sınırlı tanımlamanıza imkan tanır **Önizleme İzleyici** teklifinizi daha geniş Market hedef kitlesi için Canlı teklifinizi yayımlamadan önce serbest bırakmak.

> [!IMPORTANT]
> Seçmelisiniz **yayınlayın** teklifinizi Canlı Market genel kitle için Önizleme aşamasında teklifinizi denetledikten sonra yayımlanacak önce.

- **Bir önizleme İzleyici tanımlayın: İsteğe bağlı bir açıklama ile birlikte bir satır başına tek bir AAD/MSA hesabı e-ekleyin.**

En çok on (10) e-posta adreslerini el ile ekleyin veya canlı yirmi (var olan Microsoft hesabı (MSA) için bir CSV dosyasını karşıya yükleme, 20) veya Azure Active Directory (AAD) hesaplarını teklifinizi yayımlamadan önce doğrulama ile yardımcı olmak için. Bu hesaplar ekleyerek için marketplace(s) yayımlanmadan önce önizleme erişimi kullanıcınıza teklifiniz için izin verilecek bir kitleye tanımlıyorsunuz. Teklifinizi zaten Canlı ise, tüm değişiklikler ve güncelleştirmeler kullanıcınıza teklifiniz için test etmek için bir önizleme İzleyici tanımlayabilir.

> [!NOTE]
> Önizleme İzleyici özel bir hedef kitleye farklıdır. Bir önizleme İzleyici teklifinizi erişimine izin verilmesini _önceki_ için Canlı marketlerden yayımlanıyor. Bir plan oluşturur ve yalnızca özel bir kitle için kullanılabilir hale getirmek seçebilirsiniz. İçinde **listeleme planlama** sekmesinde, özel bir hedef kitleyle tanımlayabilirsiniz **özel bir plan budur** onay kutusu. Ardından, 20000 müşterilerin Azure Kiracı kimlikleri kullanarak özel bir hedef kitleye tanımlayabilirsiniz.

## <a name="technical-configuration"></a>Teknik yapılandırma

**Teknik yapılandırma** sekmesini teklifinizin bağlanmak için kullanılan teknik ayrıntılar (URL yolu, Web kancası, Kiracı kimliği ve uygulama kimliği) tanımlar. Bu bağlantı, elde etmeyi seçerlerse müşterinin Azure aboneliğindeki bir kaynak olarak teklifinizi sağlama sağlıyor.

- **Giriş sayfası URL'si** (gerekli): Teklifinizi Market'ten aldıktan sonra yerleşmesi müşteriler URL yönlendirilmiş sitesini tanımlayın. Bu URL, aynı zamanda Microsoft ticaret kolaylaştırmak için API bağlantı alma uç noktası olacaktır.

- **Bağlantı Web kancası** (gerekli): Microsoft, müşteri adına göndermek için gereken tüm zaman uyumsuz olaylar için (örnek: Azure aboneliği geçmiş geçersiz), bağlantı Web kancası girmenizi isteriz. Yerinde bir Web kancası sistemine sahip değilseniz, en basit yapılandırmadır, kendisine gönderilmesini meydana gelen olayları dinler ve ardından uygun şekilde işlemesine bir HTTP uç noktası mantıksal uygulama sağlamaktır (örneğin, https:\//prod-1westus.logic.azure.com:443/work). Daha fazla bilgi için [çağrı, tetikleyici veya iç içe iş akışları ile HTTP uç noktalarını logic apps'teki](https://docs.microsoft.com/azure/logic-apps/logic-apps-http-endpoint).

- **Azure AD Kiracı kimliği** (gerekli): Azure portalı içinde biz gerektiren, [Azure Active Directory (AD) uygulama oluşturma](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal) bir kimliği doğrulanmış iletişim, böylelikle bağlantı iki hizmetlerimiz arasında doğrulayabilirsiniz. Bulmak için [Kiracı kimliği](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-tenant-id), Azure Active Directory'ye gidip seçin **özellikleri**, ardından Ara **dizin kimliği** numarası listelenen (örn.) 50c464d3-4930-494c-963c-1e951d15360e).

- **Azure AD uygulama kimliği** (gerekli): Ayrıca gerekir, [uygulama kimliği](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-application-id-and-authentication-key) ve kimlik doğrulama anahtarı. Bu değerleri almak için Azure Active Directory'ye gidin ve seçin **uygulama kayıtları**, ardından Ara **uygulama kimliği** numarasını (ör. 50c464d3-4930-494c-963c-1e951d15360e) listelenir. Kimlik doğrulama anahtarı bulmak için Git **ayarları** seçip **anahtarları**. Bir açıklama ve süre ve ardından olacak bir sayı değeri sağlanmalıdır sağlamanız gerekir.

 Azure uygulama kimliği yayımcı Kimliğiniz ile ilişkili olduğunu unutmayın; bu nedenle, aynı uygulama kimliği tüm tekliflerin kullanıldığından emin olun.

## <a name="plan-overview"></a>Plan genel bakış

**Planı genel bakış** sekmesi içinde aynı Teklif planı seçeneklerini çeşitli vermenizi sağlar. Bu planları (bazen SKU'ları da adlandırılır), sürüm, kazanç veya hizmet katmanı açısından farklı. Teklifinizi Marketi'nde satmak için en az bir plan ayarlamanız gerekir.

Oluşturulduktan sonra fiyatlandırma modelleriyle, kullanılabilirlik (genel veya özel), planı adları, kimlikleri, görürsünüz geçerli durumu ve tüm kullanılabilir eylemler yayımlama.

**Eylemler** bulunan **planı genel bakış** geçerli durumunu planınıza bağlı olarak değişir ve şunları içerebilir:

- Plan durum ise **taslak** – taslağı silin
- Plan durum ise **canlı** – durdurma satmak plan veya eşitleme özel hedef kitle

**Yeni plan oluşturma** (kullanıcılar yoluyla Microsoft'a satmak üzere seçmek için bir plan en az)

- **Plan kimliği:** Bu teklifte her plan için bir benzersiz plan kimliği oluşturun. Bu kimliği, müşterilere ürün URL'si ve Azure Resource Manager şablonlarında (varsa) görülebilir. Yalnızca küçük harf, alfasayısal karakter, kısa çizgi veya alt çizgi kullanın. Bu plan kimliği için en çok 50 karakter izin verilir Seçme oluşturduktan sonra kimliği değiştirilemez unutmayın.
- **Plan adı:** Müşteriler, teklifinizi içinde seçmek planlama karar verirken bu adı görür. Her plan için bir teklif benzersiz ad, bu teklifte oluşturun. Plan adı aynı Teklif (örneğin bir parçası olabilecek yazılım planlarını ayırt etmek için kullanılır Teklif adı: Windows Server; planlar: Windows Server 2016, Windows Server 2019).

### <a name="plan-listing"></a>Plan listeleme

**Listeleme planlama** sekmesini görüntüler planınızı kullanılabildiği, dil (ve pazarlara) şu anda İngilizce (Amerika Birleşik Devletleri) yalnızca mevcut konumdur. Ayrıca, bu sayfa, dile özgü listeleme ve eklendiği tarih/saat durumunu görüntüler. Her dil için (teklif adı, açıklama, arama terimleri, vb.) Market ayrıntıları tanımlamak ihtiyacınız olacak / pazarlayın.

#### <a name="plan-listing-details"></a>Plan ayrıntıları listeleme

Plan dillerden biri seçilmesi **listeleme planlama** hakkında bilgi dahil olmak üzere **adı** ve **açıklaması.**

- **Ad**: Önceden doldurulmuş üzerinde Önizleme tabanlı **yeni plan** girişi ve başlığı "Market'te görüntülenen teklifin yazılım planınızı" olarak görünür.
- **Açıklama:** Bu, bu yazılım planı benzersiz kılan açıklamak için bir fırsat ve tüm diğer yazılım planlarını teklifinizi içinde farklılıklarını açıklamasıdır. En fazla 500 karakter içerebilir.

Seçin **Kaydet** bu alanlar tamamladıktan sonra.

#### <a name="plan-pricing-and-availability"></a>Fiyatlandırma ve kullanılabilirlik planlama

**Fiyatlandırma ve kullanılabilirlik** sekmesinde Bu plan, kullanılabilir pazarlara yapılandırmanıza olanak sağlar istenen kazanç elde etme modeli, fiyat ve fatura dönemi. Ayrıca, herkesin veya yalnızca belirli müşterilere (özel bir hedef kitle) planı görünür yapmak belirtebilirsiniz.

#### <a name="markets"></a>Pazarlar

- **Pazarlara Düzenle** (isteğe bağlı)

Her plan en az bir pazarda kullanılabilir olması gerekir. Bu planı kullanılabilir olmasını istediğiniz herhangi bir market konuma ait onay kutusunu seçin. Bir arama kutusu ve "Vergi havale" ülkelerde, Microsoft satış ve sizin adınıza vergi kullanımı remits seçme düğmesi yardımcı olmak için eklenmiştir. 


Fiyatlar ABD doları (USD) planınız için zaten ayarladıysanız ve başka bir pazar konumu ekle, yeni Pazar fiyatı geçerli döviz kurları göre hesaplanır. Fiyat için her Pazar yayımlamadan önce her zaman gözden geçirmeniz gerekir. Fiyatlandırma, yaptığınız değişiklikleri kaydettikten sonra "fiyatlar (xlsx) dışarı aktarma" bağlantısını kullanarak incelenebilir.

#### <a name="pricing"></a>Fiyatlandırma

- **Fiyatlandırma modeli**: Sabit fiyat veya tabanlı lisans

**Sabit fiyat:** Teklifinizle tek aylık veya yıllık fiyat düz tarife fiyatı erişimini etkinleştirir. Bu bazen site tabanlı fiyatlandırma olarak adlandırılır.

**Temel lisans:** Teklife erişme veya yer alan kullanıcı sayısına göre fiyat teklifinizi erişimini sağlama *kişilik*. Maksimum izin verilen bilgisayar lisansı sayısına fiyatını temel alan ve en düşük ayarlamak bu lisans tabanlı modeli sağlar. Bu şekilde, birden çok planları yapılandırarak kullanıcıların sayısına göre farklı fiyat noktaları yapılandırılabilir.  Bu alanlar isteğe bağlıdır. Boş bırakılırsa, bilgisayar lisansı sayısını (minimum 1) ve en çok sistemin destekleyebileceği gibi birçok ilgili bir sınır olmaması olarak yorumlanacaktır. Bu alanlar, planınız için bir güncelleştirme bir parçası olarak düzenlenebilir.

Bir kez yayımlandıktan sonra ödeme fiyatlandırma modeli seçimi değiştirilemez. Ayrıca, aynı fiyatlandırma modeli aynı teklifi için tüm planlar paylaşmanız gerekir.

- **Faturalama dönemi**: Aylık veya yıllık

Müşteriler, listelenen fiyat ödeme yapması gereken sıklığı seçin. En az bir aylık veya yıllık fiyat sağlanmalıdır ve seçeneklerin müşteriler için kullanılabilir hale getirilebilir.

- **Fiyat**: ABD Doları / ay veya yıl başına ABD Doları

Fiyatlar kümesi yerel para biriminde (ABD Doları = ABD Doları) yerel para kurulumu sırasında kullanılabilir geçerli döviz kurları kullanarak seçilen tüm pazarlar dönüştürülür. Elektronik Tablo fiyatlandırması ve her Pazar fiyatına gözden geçirme vererek yayımlamadan önce bu fiyatları doğrulayın. Özel ayarlamak istiyorsanız, tek bir pazar fiyatına değiştirebilir ve fiyatlandırma elektronik tabloyu içeri aktarın. Bu fiyatlandırma doğrulamak için sorumludur ve bu ayarları sahip.
**İlk veri fiyatlandırma dışarı aktarma etkinleştirmek için fiyatlandırma değişiklikleri kaydetmeniz gerekir.*

Bir plan yayımlandıktan sonra bazı kısıtlamalar ne değiştirebilirsiniz olduğundan fiyatlarınızı yayımlamadan önce gözden geçirin:

- Fiyatlandırma modeli, bir plan yayımlandıktan sonra değiştirilemez.
- Daha sonra bir fatura dönemi için bir plan yayımlandıktan sonra kaldırılamaz.
- Bir pazarda planınız için bir fiyat yayımlandığında, daha sonra değiştirilemez.

### <a name="plan-audience"></a>Hedef kitle planlama

Her plan herkese veya seçtiğiniz yalnızca belirli bir hedef kitle için görünür olacak şekilde yapılandırma seçeneğiniz vardır. Azure AD Kiracı kimlikleri kullanarak bu kısıtlı izleyiciye üyelik atayabilirsiniz.

#### <a name="privacy"></a>Gizlilik

- **Bu, özel bir plan** (onay kutusu isteğe bağlı)

Planınız özel ve yalnızca seçtiğiniz kısıtlı kitleye görünür hale getirmek için bu kutuyu işaretleyin. Özel bir plan yayımlandıktan sonra hedef kitleyi güncelleştirme ya da plan herkesin kullanımına açmak seçin. Bir plan herkese görünür olarak yayımlandıktan sonra herkese görünür olmalıdır. (Plan özel bir plan yeniden yapılandırılamaz).

- **Kısıtlı İzleyici (Kiracı kimliği)**

Bu özel plan erişebilir İzleyici atayın. Erişim bir atanan her Kiracı kimliği eklemek için seçeneğiyle Kiracı kimlikleri kullanılarak atanır. Bir .csv elektronik tablo dosyasını içe aktarılıyorsa 20.000 müşterilerin kimliklerini Kiracı veya en fazla 10 Kiracı kimlikleri eklenebilir.

Bir kiracı kimliği bir GUID (genel benzersiz tanımlayıcı, kaynakları tanımlamak için kullanılan bir 128-bit tamsayı) temsil edilen bir kuruluş bir gösterimidir. Kuruluş veya uygulama geliştirici Microsoft'la bir ilişki oluşturduğunda, örneğin Azure'a, Microsoft Intune'a veya Microsoft 365'e kaydolduğunda kuruluşun veya uygulama geliştiricinin aldığı özel bir Azure AD örneğidir. Her Azure AD kiracısı benzersizdir ve diğer Azure AD kiracılarından ayrıdır. Kiracı denetlemek için Azure portalı uygulamanızı yönetmek için kullanmak istediğiniz hesabı ile oturum açın. Bir kiracınız varsa, otomatik olarak bu kiracıda oturum açar ve kiracı adını doğrudan hesap adınızın altında görebilirsiniz. Adınızı, e-postanızı, dizininizi/kiracı kimliğinizi (GUID) ve etki alanınızı görmek için, Azure portalının sağ üst kısmında bulunan hesap adınızın üzerine gelin. Hesabınız birden çok kiracıyla ilişkiliyse, kiracılar arasında geçiş yapabileceğiniz bir menüyü açmak için hesap adınızı seçebilirsiniz. Her kiracının kendi kiracı kimliği vardır. Bir etki alanı adı URL'de kullanarak kuruluşunuzun Kiracı kimliği de arayabilirsiniz: [ https://www.whatismytenantid.com ](https://www.whatismytenantid.com).

SaaS teklifleri, özel bir hedef kitleye tanımlamak için Kiracı kimlikleri kullanırken, diğer teklif türleri (Bu da GUID'leri temsil edilir) Azure abonelik kimlikleri kullanabilir.

> [!NOTE]
> Özel İzleyici (veya sınırlı bir kitle) bir önizleme İzleyici farklıdır. İçinde **[Önizleme](#preview)** sekmesi, Önizleme İzleyici tanımlayabilirsiniz. Bir önizleme İzleyici teklifinizi erişimine izin verilmesini *önceki* Canlı Market'te yayımlanan teklif. Özel İzleyici atamasını yalnızca belirli bir plana uygularken Önizleme İzleyici tüm planlar görüntüleyebilirsiniz (özel veya değil), ancak planı test ve doğrulanmış yalnızca sınırlı Önizleme dönemi boyunca.

## <a name="example-list-of-plans-within-a-marketplace-offer"></a>Örnek bir Market teklifi içinde planları listesi

![Örnek Market planında notları ile listeleme](./media/marketplace-plan.svg)

## <a name="test-drive"></a>Test sürüşü

**Test sürücü** sekmesini Tanıtımı (veya "sürücü test") olanak tanır, müşterilerin satın alma için gerçekleştirmeden önce teklifiniz deneyebilmesi için etkinleştirilir. Bu makalede daha fazla bilgi edinin [Test Sürüşü nedir?](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/what-is-test-drive). Artık teklifiniz için bir test sürüşüne sağlamak istiyorsanız, geri dönün **[teklif Kurulum](#offer-setup)** işaretini kaldırın ve sayfa **etkinleştir test sürüşü**.

### <a name="technical-configuration"></a>Teknik yapılandırma
Test sürüşleri aşağıdaki türde kullanılabilir her biri kendi teknik yapılandırma gereksinimleri.

- [Azure Resource Manager](#technical-configuration-for-azure-resource-manager-test-drive)
- [Dynamics 365](#technical-configuration-for-dynamics-365-test-drive)
- [Mantıksal uygulama](#technical-configuration-for-logic-app-test-drive)
- [Power BI](#technical-configuration-not-required-for-power-bi-test-drives) (teknik yapılandırması gerekli değildir)

#### <a name="technical-configuration-for-azure-resource-manager-test-drive"></a>Sürücü test teknik yapılandırması için Azure Resource Manager

Çözümünüzü oluşturan tüm Azure kaynaklarını içeren bir dağıtım şablonu. Bu senaryo uygun ürünler, yalnızca Azure kaynakları kullanır. Ayarlama hakkında daha fazla bilgi bir [Azure Resource Manager test sürüşü](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/azure-resource-manager-test-drive).

- **Bölgeleri** (gerekli): Şu anda 26 desteklediği bölgelerin nereden test sürüşünüz kullanılabilir hale getirilebilir vardır. Genellikle, en iyi performans için en yakın bölgeyi seçebilmeniz için test sürüşünüz müşteriler, en büyük sayısını tahmin burada bölgelerde kullanılabilmesini isteyebilirsiniz. Aboneliğiniz her seçmiş olursunuz bölgelerin gerekli tüm kaynakları dağıtmak için izin verildiğinden emin olmanız gerekir.

- **Örnekleri**: (Sık erişimli veya Durgun) türünü seçin ve sayı teklifinizi kullanılabildiği bölgeler sayısıyla çarpımı kullanılabilir örnekleri.

**Sık erişimli**: Bu tür bir örnek, seçili bölge başına dağıtılan ve bekleniyor erişimdir. Müşteriler anında erişebilir *etkin* bir dağıtım için beklemek zorunda yerine bir test sürüşüne örnekleri. Artırabilen maliyeti daha büyük bir çalışma süresi tabi şekilde, bu örneklerin her zaman Azure aboneliğinize göre çalıştığını ' dir. En az bir sahip için önemle tavsiye edilir *etkin* çoğu müşteri tam dağıtımları için beklemek Hayır ise, müşteri kullanımı bir bırakma kaynaklanan istemiyorsanız gibi örnek *etkin* örneği kullanılabilir.

**Soğuk**: Bu tür bir örneği, bölge başına büyük olasılıkla dağıtılabilir örneklerinin toplam sayısını temsil eder. Soğuk örnekleri gerektiren bir müşteri test sürüşü, bu nedenle istediğinde dağıtmak için tüm Test sürücü Resource Manager şablonu *soğuk* örnekleridir daha çok yavaş *etkin* örnekleri. Yalnızca test sürüşü süresi için ödeme gerekir, bu düşüş olduğu *değil* her zaman Azure aboneliğinizde çalışan bir *etkin* örneği.

- **Test sürücü Azure Resource Manager şablonu**: Azure Resource Manager şablonunuzu içeren .zip karşıya yükleyin.  Hızlı Başlangıç makalesinde bir Azure Resource Manager şablonu oluşturma hakkında daha fazla bilgi edinin [oluşturun ve Azure portalını kullanarak Azure Resource Manager şablonlarını dağıtma](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal).

- **Test sürüşü süresi** (gerekli): Test Sürüşü saat sayısı etkin kalır. bir süre girin. Bu süre sona erdikten sonra Test Sürüşü otomatik olarak sona erer. Bu süre yalnızca kümesi saatlerin toplamına göre bir tam sayı (örn. sonuç "2" saat "1.5" geçerli değildir).

#### <a name="technical-configuration-for-dynamics-365-test-drive"></a>Sürücü test Dynamics 365 için teknik yapılandırması

Microsoft, barındırma ve sağlama hizmeti ve bu tür bir test sürüşü kullanarak dağıtımı bir test sürüşüne ayarlamanın karmaşıklığını kaldırabilirsiniz. Bu tür bir barındırılan test sürüşü için yapılandırmayı test sürüşü Business Central, müşteri katılımı veya işlemleri İzleyici mı hedeflediği bağımsız olarak aynıdır.

- **Maks. eş zamanlı test sürüşleri** (gerekli): Test sürüşünüz aynı anda kullanabileceğiniz müşteriler maksimum sayısını ayarlayın. Yeterli lisans sınırı kümesi desteklemek kullanılabilir olmasını sağlamak ihtiyacınız olacak şekilde test sürüşü etkin olduğu sürece her eş zamanlı kullanıcı bir Dynamics 365 lisansı kullanacaktır. Önerilen değeri 3-5.

- **Test sürüşü süresi** (gerekli): Test Sürüşü saat tanımlayarak etkin kalacak süreyi girin. Bu kadar çok saat sonra oturumu sonlandırmak ve artık lisanslarınızdan birini kullanır. Teklifinizi karmaşıklığına bağlı olarak 2-24 saat değerini öneririz. Bu süre yalnızca kümesi saatlerin toplamına göre bir tam sayı (örn. sonuç "2" saat "1.5" geçerli değildir).  Çalışma zamanı dışında ve test sürüşü yeniden erişmek istediğiniz kullanıcı yeni bir oturum isteyebilir.

- **Örnek URL** (gerekli): URL müşterinin kendi test sürüşü burada başlar. Genellikle, uygulamanızı çalıştıran yüklü örnek verilerle Dynamics 365 örneğinizin URL'sini (örneğin https://testdrive.crm.dynamics.com).

- **Örnek Web API'si URL** (gerekli): Microsoft 365 hesabınızda oturum ve giderek, Dynamics 365 örneği için Web API URL'si almak **ayarları** \&gt; **Özelleştirme** \&gt; **Geliştirici Kaynakları** \&gt; **Örnek Web API'si (hizmet kök URL'si)** , burada bulunan URL'yi kopyalayın (örneğin https://testdrive.crm.dynamics.com/api/data/v9.0).

- **Rol adı** (gerekli): İçinde özel Dynamics 365 test sürüşünüz tanımladığınız güvenlik rolü adını sağlayın. Bu kullanıcı için test sürüşü (örneğin test-drive rolü) sırasında atanır.

#### <a name="technical-configuration-for-logic-app-test-drive"></a>Mantıksal uygulamayı test sürüşü için teknik yapılandırma

Herhangi bir özel ürün, bu tür bir kapsayan karmaşık çözüm mimarileri çeşitli test sürücü dağıtım şablonu kullanmanız gerekir. Test sürüşleri mantıksal uygulamayı hazırlama hakkında daha fazla bilgi için ziyaret [işlemleri](https://github.com/Microsoft/AppSource/blob/master/Setup-your-Azure-subscription-for-Dynamics365-Operations-Test-Drives.md) ve [Customer Engagement](https://github.com/Microsoft/AppSource/wiki/Setting-up-Test-Drives-for-Dynamics-365-app) GitHub üzerinde.

- **Bölge** (gerekli, tek seçimli bir açılır liste): Şu anda 26 desteklediği bölgelerin nereden test sürüşünüz kullanılabilir hale getirilebilir vardır. Kaynaklar mantıksal uygulamanız için seçtiğiniz bölgede dağıtılır. Mantıksal uygulamanızı belirli bir bölgede depolanan tüm özel kaynaklar varsa, bu bölgeye burada seçili olduğundan emin olun. Bunu yapmanın en iyi yolu, tam olarak mantıksal uygulamanızı yerel olarak Şirket portalı, Azure aboneliğinize dağıtmak ve bunu doğru şekilde bu seçim yapmadan önce işlediğini doğrulayın sağlamaktır.

- **Maks. eş zamanlı test sürüşleri** (gerekli): Test sürüşünüz aynı anda kullanabileceğiniz müşteriler maksimum sayısını ayarlayın. Bu test sürücüleri zaten dağıtıldıysa, müşterilerin onlara bir dağıtım için beklemenize gerek kalmadan anında erişim sağlar.

- **Test sürüşü süresi** (gerekli): Test Sürüşü saat sayısı etkin kalır. bir süre girin. Bu süre sona erdikten sonra test sürüşü otomatik olarak sona erer.

- **Azure kaynak grubu adı** (gerekli): Girin [Azure kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups) mantıksal uygulamayı test sürüşünüz kaydedildiği adı.

- **Azure logic app name** (gerekli): Test sürüşü kullanıcıya atar mantıksal uygulama adını girin. Bu mantıksal uygulama, Azure kaynakları grubu yukarıdaki kaydedilmesi gerekir.

- **Sağlamayı kaldırma mantıksal uygulama adı** (gerekli): Müşteri tamamlandıktan sonra test sürüşü deprovisions mantıksal uygulama adını girin. Bu mantıksal uygulama, Azure kaynakları grubu yukarıdaki kaydedilmesi gerekir.

#### <a name="technical-configuration-not-required-for-power-bi-test-drives"></a>Teknik yapılandırma için Power BI test sürüşleri gerekli değildir

Görsel bir etkileşimli Power BI embedded bir bağlantı kendi test sürüşü olarak gerekli başka teknik yapılandırma özel olarak geliştirilmiş bir panoyu paylaşmak için kullanabileceğini göstermeyi istediğiniz ürünleri. Ayarlama hakkında daha fazla bilgi[Power BI](https://docs.microsoft.com/power-bi/service-template-apps-overview) şablon uygulamalar.

### <a name="deployment-subscription-details"></a>Dağıtım Abonelik Ayrıntıları

Test Sürüşü sizin adınıza dağıtmak için lütfen oluşturun ve ayrı ve benzersiz bir Azure aboneliği belirtin. (Power BI test sürüşleri için gerekli değildir).

- **Azure abonelik kimliği** (Azure Resource Manager ve Logic apps için gerekli): Raporlama ve faturalama kaynak kullanımı, bir Azure hesabı hizmetlerini erişim vermek için abonelik Kimliğini girin. Ele almanızı öneririz [ayrı bir Azure aboneliği oluşturma](https://docs.microsoft.com/azure/billing/billing-create-subscription) zaten yoksa, test sürüşleri için kullanılacak. Azure abonelik Kimliğinizi açarak bulabilirsiniz [Azure portalında](https://portal.azure.com/) giderek **abonelikleri** sol taraftaki menüyü sekmesi. Sekmeyi seçerek, abonelik kimliği (örneğin "a83645ac-1234-5ab6-6789-1h234g764ghty") görüntüler.

- **Azure AD Kiracı kimliği** (gerekli): Azure Active Directory (AD) girin [Kiracı kimliği](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-tenant-id). Bu Kimliğini bulmak için oturum açın [Azure portalında](https://portal.azure.com/), soldaki menüde Active Directory sekmesini seçin, **özellikleri** , ardından Ara **dizin kimliği** numarası listelenen (örn.) 50c464d3-4930-494c-963c-1e951d15360e). Kuruluşunuzun Kiracı kimliği, etki alanı adı URL'de kullanarak da arayabilirsiniz: [ https://www.whatismytenantid.com ](https://www.whatismytenantid.com).

- **Azure AD Kiracı adı** (için Dynamic 365 gerekli): Azure Active Directory (AD) adınızı girin. Bu adı bulmak için oturum açın [Azure portalında](https://portal.azure.com/), sağ üst köşedeki Kiracı adınızın hesap adınızın altında listelenir.

- **Azure AD uygulama kimliği** (gerekli): Azure Active Directory (AD) girin [uygulama kimliği](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-application-id-and-authentication-key). Bu Kimliğini bulmak için oturum açın [Azure portalında](https://portal.azure.com/), soldaki menüde Active Directory sekmesini seçin, **uygulama kayıtları**, ardından Ara **uygulama kimliği** numarası (örneğin 50c464d3-4930-494c-963c-1e951d15360e) listelenir.

- **Azure AD uygulama anahtarı** (gerekli): Azure Active Directory (AD) girin [uygulama anahtarı](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-application-id-and-authentication-key). Bu Kimliğini bulmak için oturum açın [Azure portalında](https://portal.azure.com/), soldaki menüde Active Directory sekmesini seçin, **uygulama kayıtları** , ardından **ayarları**  >  **Anahtarları**.

Unutmayın **Kaydet** sonraki bölüme geçmeden önce!

### <a name="test-drive-listings-optional"></a>Test listelerini sürücüye (isteğe bağlı)

**Test Sürüşü listelerini** seçeneği bulunan altında **Test sürücü** sekmesini görüntüler test sürüşünüz kullanılabildiği, dil (ve pazarlara) şu anda İngilizce (Amerika Birleşik Devletleri) yalnızca mevcut konumdur . Ayrıca, bu sayfa, dile özgü listeleme ve eklendiği tarih/saat durumunu görüntüler. Her dil/pazar için test sürüşü ayrıntıları (Açıklama, kullanıcı kılavuzuna, videolar vb.) tanımlamak gerekir.

- **Açıklama** (gerekli): Ne açıklanacaktır, test sürüşünüz açıklayan hedefleri denemek için kullanıcı özelliklerini keşfetmek için ve teklifinizi almaya karar kullanıcının yardımcı olmak için ilgili tüm bilgileri. Bu alandaki metin en fazla 3000 karakter girilebilir. 

- **Erişim bilgileri** (Azure Resource Manager ve mantıksal test sürüşleri için gerekli): Bir müşteri erişmek ve bu test sürüşü kullanmak için bilmeniz gerekenler açıklanmaktadır. Teklifinizi ve tam olarak hangi müşterinin test sürüşü boyunca özelliklerine erişmek için bilmeniz gereken kullanarak bir senaryo yol. Bu alandaki metin en fazla 10.000 karakter girilebilir.

- **Kullanıcı el kitabına** (gerekli): Test sürücü deneyiminizi ilişkin ayrıntılı bir kılavuz. Kullanıcı el ile test sürüşü yaşamasını elde edin ve sahip olabilecek herhangi bir sorunuz için başvuru olarak hizmet müşterisi istediğinizi tam olarak kapsamalıdır. Dosyanın PDF biçiminde olmalıdır ve karşıya yükledikten sonra (255 karakter Maks) adlandırılır.

- **Videolar: Video ekleme** (isteğe bağlı): Videoları için YouTube veya Vimeo karşıya ve bir müşteri bir kılavuzda başarıyla özelliklerini kullanma dahil olmak üzere test sürüşü daha iyi anlamalarına yardımcı olmak için bilgi görüntüleyebilmesi için bir bağlantı ve küçük resim görüntüsü ile (533 x 324 piksel cinsinden) burada başvurulan, Teklif ve avantajları vurgulayın senaryoları anlayın.
  - **Ad** (gerekli)
  - **URL'si (YouTube veya Vimeo yalnızca)** (gerekli)
  - **Küçük resim (533 x 324px)** : PNG biçiminde resim dosyası olmalıdır.

Seçin **Kaydet** bu alanlar tamamladıktan sonra.

## <a name="publish"></a>Yayımlama

#### <a name="submit-offer-to-preview"></a>Gönderme Önizleme sunma

Teklifin tüm gerekli bölümleri tamamladıktan sonra seçin **yayımlama** portalının sağ üst köşedeki içinde. İçin yeniden yönlendirilmiş olacaktır **gözden geçirin ve yayımlama** sayfası. 

Bu teklif yayımlama ilk kez buysa, şunları yapabilirsiniz:

- Bu teklif, her bölüm için tamamlanma durumunu görürsünüz.
    - *Başlatılmamış* – bölümüne dokunmaz anlamına gelir ve tamamlanması gerekir.
    - *Tamamlanmamış* – bölüm düzeltilmesi gereken hatalar olduğu anlamına gelir veya daha fazla bilgi sağlanması gerekir. Lütfen bölüm için geri dönün ve güncelleştirin.
    - *Tam* – yol bölümü tamamlandığında gerekli tüm verileri sağlanan ve hata yok. Teklif gönderebilmeniz için önce tüm bölümleri teklif eksiksiz bir durumda olması gerekir.
- Uygulamanız doğru bir şekilde uygulamanızı anlamak için yararlı tamamlayıcı notları yanı sıra test edildiğinden emin olmak için sertifika ekibine test yönergeler sağlar.
- Seçerek teklif yayımlama için gönderme **Gönder**. Teklifin Önizleme sürümü sırasında bildiren bir e-posta gözden geçirmek ve onaylamak için kullanılabilir size göndereceğiz. İş ortağı Merkezi'ne dönmek ve seçin **Go-live** genel (veya bir özel, özel dinleyicilere teklif varsa) teklifiniz yayımlamak teklif için.

## <a name="next-steps"></a>Sonraki adımlar

- [Ticari Market'te bir mevcut teklifi güncelleştirme](./update-existing-offer.md)
