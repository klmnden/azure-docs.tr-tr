---
title: Liste türü tarafından gereksinimleri | Azure
description: Bu makalede uygunluk ölçütleri ve uygulamalarını Azure Marketi'nde yayımlama anlayın çalışılırken yayımlama gereksinimleri ortakları açıklanmaktadır.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
documentationcenter: ''
author: jm-aditi-ms
manager: pabutler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 06/05/2018
ms.author: ellacroi
ms.openlocfilehash: 07a62dfa2d7e1c71daf547c5aa7c8c7d15830bfd
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36309325"
---
# <a name="requirements-by-listing-type"></a>Liste türü tarafından gereksinimleri  
Teknik ve pazarlama içerik gereksinimleri mağaza, Teklif türü ve liste türüne göre değişir. Uyumluluğunu doğrulamak için aşağıdaki özellikleri gözden geçirin.  
1. StoreFront gereksinimleri:  
    *   [AppSource](#storefront-requirements-appSource)  
    *   [Azure Market](#storefront-requirements-azure-marketplace)  
2. Liste türü ve Teklif türü gereksinimleri:  
    *   Çözümünüzü sayfa konumunda bulunan için liste türleri ve teklif türleri hakkında daha fazla bilgi için listeleme türünü belirleme ziyaret [docs.microsoft.com/azure/marketplace/determine-your-listing-type](./determine-your-listing-type.md).  

## <a name="storefront-requirements-appsource"></a>StoreFront gereksinimleri: AppSource  
Aşağıdaki tabloda AppSource üzerinde yayımlama için önkoşul gereksinimleri açıklanmaktadır.  
| Gereksinim | Ayrıntılar | Gerekli ya da Önerilen |  
|:--- |:--- |:--- |  
| ***Azure Active Directory (Azure AD)*** | Uygulamanız Azure Active Directory Federasyon çoklu oturum açma (SSO Azure AD federe) özelliğini etkin onay ile izin vermeniz gerekir.<ul> <li>Azure AD etkinleştirme hakkında daha fazla bilgi SSO Federasyon için yapılandırma çoklu oturum açma için Azure Active Directory'de uygulama Galerisi sayfasında bulunan olmayan uygulamalar ziyaret edin [docs.microsoft.com/azure/active-directory/ Active-directory-saas-özel-apps](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps).</li> </ul> | Gerekli |   
| ***Microsoft bulut hizmetleriyle tümleştirme*** | Uygulamanızı Microsoft Power BI, Cortana Intelligence ya da Microsoft Azure Hizmetleri gibi diğer Microsoft Cloud services ile tümleştirmeniz.<ul> <li>Nesnelerin interneti Microsoft Cloud hizmet örneğidir.</li> </ul> | Önerilen |  
| ***Hedef kitle*** | Uygulamanızı iş kullanıcılar ve işletme sahipleri için olması gerekir. | Gerekli | 
| ***İş için bir hizmet (SaaS) uygulaması olarak yazılım*** | Uygulamanızı aşağıdaki gereksinimleri karşılaması gerekir.<ul> <li>Bir iş kolu satır SaaS uygulaması</li> <li>İş sürecini odaklanmış</li> <li>İşletme müşterileri için hedeflenmiş</li> <li>Kullanıcı adı ve parola gibi oturum açmak için iş kimlik bilgilerini kullanmak kullanıcıları etkinleştir</li> </ul> | Gerekli |  
| ***Ücretsiz deneme süresi ve deneme sürümü deneyimi*** | Uygulamanızı bir sırayla uygulamanız sınırlı bir süre için ücretsiz kullanmak bir müşteri için aşağıdaki seçenekleri içermelidir.<ul> <li>Sağlayan bir `try` müşteriler bir deneme sürümü AppSource uygulamanızda başlayabilir şekilde yöntemi</li> <li>Sağlayan bir `request trial` müşteriler bir deneme sürümü, uygulamanızın isteyebilir şekilde AppSource seçeneği</li> </ul>Sağladığınız ücretsiz deneme müşteri ek ücret ödemeden uygulamanızla denemek için önceden ayarlanmış bir süre sağlaması gerekir. | Gerekli |  
| ***Kolayca yapılandırılabilir ve kullanıma hazır çözümü*** | Uygulamanız, kolay ve hızlı yapılandırma ve özelleştirme gerekli yok ayarlanmış olması gerekir. | Gerekli |  
| ***Sağlama Yönetimi*** | Aldığınız önce sağlama verileri mağaza müşteri adayları kabul etmek, CRM etkinleştirmeniz gerekir.<ul> <li>Katılımlarını Marketo, Microsoft Dynamics veya Salesforce gösterilebilir</li> </ul> | Gerekli |  
| ***Gizlilik ilkesini ve kullanım koşulları*** | Uygulamanızı genel bir URL kullanarak gizlilik ilkesi sayfanızı bağlantısını sağlamanız gerekir. Metin olarak yayımlama sırasında kullanım koşullarını sağlanması gerekir. | Gerekli |  
| ***Destek*** | Uygulamanızı genel bir URL kullanarak, Müşteri Destek sayfasına bir bağlantı sağlamanız gerekir. Uygulamanızı bir deneme ise, deneme süresi boyunca herhangi bir ek ücret ödemeden desteklemesi gerekir. | Gerekli |  

## <a name="storefront-requirements-azure-marketplace"></a>StoreFront gereksinimleri: Azure Market  
Azure Market türlerinde listeleme önkoşul gereksinimleri şunlardır:  
| Gereksinim | Ayrıntılar | Liste türü |  
|:--- |:--- |:--- |  
| ***Katılım ilkeleri*** | Uygulamanızı Azure Marketi katılım ilkeleri izlemeniz gerekir.<ul> <li>Katılım ilkeleri hakkında daha fazla bilgi için Azure Market katılım sayfasında bulunan ilkeleri ziyaret [azure.microsoft.com/support/legal/marketplace/participation-policies](https://azure.microsoft.com/support/legal/marketplace/participation-policies).</li></ul> | liste<br />Transact<br />deneme |  
| ***Microsoft ile tümleştirme*** | Teklifiniz kullanın veya Microsoft Azure hizmet türleri işlem, ağ veya depolama gibi genişletir. Teklifiniz, var olan bir Azure Marketi kategori veritabanları, güvenlik veya ağ gibi uyumlu olmalıdır.<ul> <li>Market teklifleri hakkında daha fazla bilgi için Market sayfasında bulunan uygulamalar ziyaret [azuremarketplace.microsoft.com/marketplace/apps](https://azuremarketplace.microsoft.com/marketplace/apps).</li> </ul> | liste<br />Transact<br />deneme |  
| ***Hedef kitle*** | Teklifiniz, BT uzmanları, bulut geliştiriciler veya diğer teknik müşteri rolleri olması gerekir. | liste<br />Transact<br />deneme |  
| ***Sağlama Yönetimi*** | Aldığınız önce sağlama verileri mağaza müşteri adayları kabul etmek, CRM (Marketo, Microsoft Dynamics veya Salesforce) etkinleştirmeniz gerekir. | liste<br />Transact<br />deneme |  
| ***Gizlilik ilkesini ve kullanım koşulları*** | Uygulamanızı genel bir URL kullanarak gizlilik ilkesi sayfanızı bağlantısını sağlamanız gerekir. Metin olarak yayımlama sırasında kullanım koşullarını sağlanması gerekir. | liste<br />Transact<br />deneme |  
| ***Destek*** | Teklifiniz, genel bir URL kullanarak, Müşteri Destek sayfasına bir bağlantı sağlamanız gerekir. Teklifiniz deneme ise, deneme süresi boyunca herhangi bir ek ücret ödemeden desteklemesi gerekir. | Transact<br />deneme |    

## <a name="non-transact-listings"></a>Transact listeleri  
Bu bölümde, liste türü Transact kullanmayın tüm teklif türleri açıklanmaktadır. 

### <a name="list"></a>Liste  
Liste türü listeleme markette aşağıdaki veriş teklif türleri içerir.  

| Teklif türü | Vitrin | Ayrıntılar |  
|:---        |:---        |:---     |  
| Danışmanlık Hizmetleri | AppSource | [Gereksinimleri: AppSource: listesi: Danışmanlık Hizmetleri](#requirements-appsource-list-consulting-services) |  
| Danışmanlık Hizmetleri | Azure Market | [Gereksinimleri: Azure Market: listesi: Danışmanlık Hizmetleri](#requirements-azure-marketplace-list-consulting-services) |  
| Benimle iletişime geçin | AppSource | [](#) |  
| Benimle iletişime geçin | Azure Market | [Gereksinimleri: AppSource: listesi: benimle iletişim](#requirements-azure-marketplace-list-contact-me) |  

#### <a name="requirements-appsource-list-consulting-service"></a>Gereksinimleri: AppSource: listesi: hizmet danışmanlık  

| Gereksinimler | Ayrıntılar |  
|:--- |:--- |  
| Hizmet teklif özelliklerini | Danışmanlık hizmetiniz aşağıdaki ölçütleri karşılaması gerekir.<ul> <li>Sabit kapsam, sabit süre, sabit fiyat (veya boş) katılım sunar.</li> <li>Satış öncesi için öncelikle yönlendir.</li> <li>Tek bir müşteri sınırlayın.</li> <li>Sitesinde yürütün.</li> </ul> |  
| Danışmanlık Hizmetleri için iş ortağı gereksinimleri | Hizmetiniz için ilgili alanında ölçütlere uyan.<table><tr><th>Çözüm alanı</th><th>Ölçütler</th></tr><tr><td>Dynamics 365 müşteri katılım için</td><td>Gümüş veya Bulut müşteri ilişkileri yönetimi Gold uzmanlığına sahip.</td></tr><tr><td>Dynamics 365 Finans ve işlemleri, Enterprise edition</td><td>Gümüş veya Altın Kurumsal Kaynak planlama uzmanlığı ve bulut işlemlerinizin geliri 25000 veya daha fazla sonunda 12 ay içinde vardır.</td></tr><tr><td>Dynamics 365 Finans ve işlemleri, Business edition</td><td>Bulut Hizmetleri Sağlayıcısı (CSP) veya dijital iş ortağı, kaydı (DPOR) olarak bir veya daha fazla müşterileri için hizmet.</td></tr><tr><td>Power BI</td><td>Çözüm Partner ölçütleri karşılayan.</td></tr><tr><td>PowerApps</td><td>Bir iş ortağı gösterimi çözümü vardır.</td></tr></table><ul> <li>Müşteri İlişkileri Yönetimi hakkında daha fazla bilgi için bulut Müşteri İlişkileri Yönetimi sayfasında bulunan ziyaret [partner.microsoft.com/membership/cloud-customer-relationship-management-competency](https://partner.microsoft.com/membership/cloud-customer-relationship-management-competency).</li> <li>Kurumsal Kaynak Planlaması sayfasında bulunan kaynak planlama hakkında daha fazla bilgi için ziyaret [partner.microsoft.com/membership/enterprise-resource-planning-competency](https://partner.microsoft.com/membership/enterprise-resource-planning-competency).</li> <li>Bulut Hizmetleri sayfasında bulunan sağlayıcısı CSP hakkında daha fazla bilgi için ziyaret [partner.microsoft.com/cloud-solution-provider](https://partner.microsoft.com/cloud-solution-provider).</li> <li>DPOR hakkında daha fazla bilgi için dijital iş ortağını ziyaret edin ve iş ortağı ilişkilendirme sayfasında bulunan [partner.microsoft.com/membership/digital-partner-of-record](https://partner.microsoft.com/membership/digital-partner-of-record).</li> <li>Çözüm partner ölçütleri hakkında daha fazla bilgi için çözüm iş ortağı genel bakış ziyaret ederek özendirme belge bulunan [www.microsoftpartnerserverandcloud.com/_layouts/download.aspx? SourceUrl=Hosted%20Documents/Power%20BI%20Program%20Overview%20%26%20Incentives.pdf](https://www.microsoftpartnerserverandcloud.com/_layouts/download.aspx?SourceUrl=Hosted%20Documents/Power%20BI%20Program%20Overview%20%26%20Incentives.pdf).</li> <li>İş ortağı gösterimi hakkında daha fazla bilgi için sayfa bulunan iş ortağı gösterimi ziyaret [powerapps.microsoft.com/partner-showcase](https://powerapps.microsoft.com/partner-showcase).</li> </ul> |  

#### <a name="requirements-azure-marketplace-list-consulting-service"></a>Gereksinimleri: Azure Market: listesi: hizmet danışmanlık  

| Gereksinimler | Ayrıntılar |  
|:--- |:--- |  
| Hizmet teklif özelliklerini | Danışmanlık hizmetiniz aşağıdaki ölçütleri karşılaması gerekir.<ul> <li>Sabit kapsam, sabit süre, sabit fiyat (veya boş) katılım sunar.</li> <li>Satış öncesi için öncelikle yönlendir.</li> <li>Tek bir müşteri sınırlayın.</li> <li>Sitesinde yürütün.</li> </ul> |  
| Danışmanlık Hizmetleri için iş ortağı gereksinimleri | Gümüş veya altın ilgili alanında aşağıdaki yetkinlikleri birinde hizmetiniz için olmalıdır. <table><tr><th>Çözüm alanı</th><th>Uzmanlığı</th></tr><td>Bulut platformu ve altyapısı</td><td>Bulut platformu<br />Veri Merkezi</td><tr><td>Uygulama geliştirme ve ISV</td><td>Uygulama Geliştirme<br />Uygulama Tümleştirme<br />DevOps</td></tr><tr><td>Veri Yönetimi ve analizi</td><td>Veri Analizi<br />Veri Platformu</td></tr></table><ul> <li>Yetkinlikleri aracılığıyla Microsoft iş ortağı sayfasında bulunan ağ yetkinlikleri hakkında daha fazla bilgi için ziyaret [partner.microsoft.com/membership/competencies](https://partner.microsoft.com/membership/competencies).</li> <li>Azure Market danışmanlık sayfasında bulunan hizmetleri listeleme hakkında daha fazla bilgi için ziyaret [docs.microsoft.com/azure/marketplace/consulting-services](https://docs.microsoft.com/azure/marketplace/consulting-services).</li></ul> |  

<!-- #### Requirements: Azure Marketplace: List: Contact Me -->

---   

### <a name="trial"></a>Deneme  

| Teklif türü | Vitrin | Ayrıntılar |  
|:---        |:---        |:---     |  
| Ücretsiz / SaaS deneme | AppSource | [Türü gereksinimlerini listeleyen: deneme](#listing-type-requirements-trial) |  
| Ücretsiz / SaaS deneme | Azure Market | [Gereksinimleri: Azure Market: deneme: ücretsiz deneme sürümü / SaaS deneme](#requirements-azure-marketplace-trial-free-trial-/-saas-trial) |  
| Etkileşimli tanıtım | AppSource | [Türü gereksinimlerini listeleyen: deneme](#listing-type-requirements-trial) |  
| Etkileşimli tanıtım | Azure Market | [Gereksinimleri: Azure Market: deneme: etkileşimli Tanıtımı](#requirements-azure-marketplace-trial-interactive-demo) |  
| Test sürüşü | AppSource | [Türü gereksinimlerini listeleyen: deneme](#listing-type-requirements-trial) |  
| Test sürüşü | Azure Market | [Gereksinimleri: Azure Market: deneme: Test sürücü](#requirements-azure-marketplace-trial-test-drive) |  

#### <a name="requirements-azure-marketplace-trial"></a>Gereksinimleri: Azure Market: deneme  

| Gereksinim | Ayrıntılar |  
|:--- |:--- |  
| Ücretsiz deneme süresi ve deneme sürümü deneyimi | Müşterinizin, uygulamanızı ücretsiz sınırlı bir süre için kullanabilir.<br /><br />Müşteri Lisans veya abonelik ücretler teklif veya uygulama için ödeme yapmak için gerekli değildir. Müşteri temel alınan Microsoft birinci taraf ürün veya hizmet için ödeme yapmak için gerekli değildir. Tüm deneme seçenekleri Azure aboneliğinize dağıtılır. Yönetim ve maliyet iyileştirmesi tek denetim deneme var.<br /><br />Ücretsiz deneme sürümü, etkileşimli demo seçin veya sürücü test. Seçtiğiniz öğeye olsun, ücretsiz deneme sürümünüzü müşteri hiçbir ek ücret ödemeden uygulama denemek için önceden ayarlanmış bir süre sağlaması gerekir.<ul> <li>Sınamayı oluşturma işlemine başlamak için bir e-posta Gönder [ amp-testdrive@microsoft.com ](mailto:amp-testdrive@microsoft.com).</li> </ul>Not: Azure Market SaaS deneme deneyimleri oturum açmak için iş kimlik bilgilerini kullanmak üzere müşteriler izin vermeniz gerekir.<ul> <li>Daha fazla bilgi için deneme deneyimleri bölümünde bulunan AppSource ziyaret [docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-appsource-certified#appsource-trial-experiences](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-appsource-certified#appsource-trial-experiences).</li> </ul> |  
| Kolayca yapılandırılabilir ve kullanıma hazır çözümü | Uygulamanızı yapılandırmak ve ayarlamak hızlı ve kolay olmalıdır. |  
| Kullanılabilirlik / açık kalma süresi | SaaS uygulaması veya platform en az % 99,9 çalışma süresi olması gerekir. |  
| Azure Active Directory | Teklifiniz etkin izin ile çoklu oturum açma (SSO) (Azure AD SSO federe) Azure Active Directory (Azure AD) federe izin vermeniz gerekir. |  

#### <a name="requirements-azure-marketplace-trial-free-trial--saas-trial"></a>Gereksinimleri: Azure Market: deneme: ücretsiz deneme sürümü / SaaS deneme  

| Avantaj | Gereksinim |  
|:--- |:--- |  
| Dönüştürme için otomatik olarak yöntemi ile satın alma kullanılması ödeme önce ürününüzü denemek bir müşteri sağlar. Ayrıca müşteri ve Microsoft satış ekipleri ile birleşik katılım için prototip sağlamaları sağlar. | Bir sanal makine ya da çözüm şablonu çözümüdür.<br /><br />Çok kiracılı bir SaaS ürün sunar ve sunan bir SaaS, çözümüdür.<br /><br />Bir müşteri hale getirmek için ilk çalıştırma deneyimi ve hızlı bir şekilde çalışan var.<br /><br />Tek bir kiracı sahip ancak müşteriler Konuk kullanıcı olarak eklemekte olduğunuz. |  

#### <a name="requirements-azure-marketplace-trial-interactive-demo"></a>Gereksinimleri: Azure Market: deneme: etkileşimli Tanıtımı  

| Avantaj | Gereksinim |  
|:--- |:--- |  
| Çözümünüzü kurma eylem KARMAŞASIZ görmek, müşterilerin sağlar. | Çözümünüzü deneme süresi içinde elde etmek için zor olabilir karmaşık ayarları gerektirir. |  

#### <a name="requirements-azure-marketplace-trial-test-drive"></a>Gereksinimleri: Azure Market: deneme: Test sürücü  

| Avantaj | Gereksinim |  
|:--- |:--- |  
| Bunlar satın almadan önce ürününüzü denemek bir müşteri sağlar.<br /><br />Önceden yapılandırılmış ayarları kullanarak çözümünüzün Kılavuzlu bir deneyim sağlar.<br /><br />Aşağıdaki ek avantajları sınamayı kullanılırken geçerlidir.<ul> <li>Kullanıcı aramalarını Market'te % 27, kullanıcılar tarafından yalnızca Göster teklifleri test sürücülerle için iyileştirilmiştir.</li> <li>Test sürücülerle teklifleri %38 daha fazla müşteri adayları teklifleri daha oluşturun.</li> <li>Yeni müşteri edinme Market'te % 36 sınamayı sürdü müşterilerden gelen.</li> <li>Test sürücüleri ürününüz ortak satış çalışmaları için daha iyi anlamak Microsoft alan satıcılar sağlar.</li> </ul> | Çözümünüzü bir sanal makine, çözüm şablonu ya da tek bir kiracı SaaS uygulamayla veya sağlamak için karmaşıktır. <br /><br />Ücretli bir teklife deneme sürümünüzü dönüştürmek için bir yöntem yok. |  

---

## <a name="transact-specific-listings"></a>Transact-özel listeleri

### <a name="transact"></a>İşlem  

| Teklif türü | Vitrin | Ayrıntılar |   
|:---        |:---        | :--- |  
| Azure uygulamaları: yönetilen bir uygulama | Azure Market | [Gereksinimleri: Azure Market: Transact: Azure uygulamaları: yönetilen bir uygulama](#requirements-azure-marketplace-transact-azure-apps-managed-app) |  
| Azure uygulamaları: Çözüm şablonu | Azure Market | [Gereksinimleri: Azure Market: Transact: Azure uygulamaları: Çözüm şablonu](#requirements-azure-marketplace-transact-azure-apps-solution-template) |  
| Kapsayıcılar | Azure Market | [Gereksinimleri: Azure Market: Transact: kapsayıcı](#requirements-azure-marketplace-transact-container) |  
| SaaS uygulama  | Azure Market | [Gereksinimleri: Azure Market: Transact: SaaS uygulama](#requirements-azure-marketplace-transact-saas-app) |  
| Sanal makine | Azure Market | [Gereksinimleri: Azure Market: Transact: sanal makine](#requirements-azure-marketplace-transact-virtual-machine) |  

<!-- #### Requirements: Azure Marketplace: Transact: Azure apps: Managed app  

#### Requirements: Azure Marketplace: Transact: Azure apps: Solution template   -->

#### <a name="requirements-azure-marketplace-transact-container"></a>Gereksinimleri: Azure Market: Transact: kapsayıcı  

| Gereksinim | Ayrıntılar |  
|:--- |:--- |  
| Faturalama ve ölçümü | Ya da ücretsiz destek veya KLG faturalama modeli. |  
| Docker tabanlı görüntü | Kapsayıcı görüntünüzü Docker görüntü biçimi dayalı ve Azure kapsayıcı defterlerinden çekilen gerekir. |  

#### <a name="requirements-azure-marketplace-transact-saas-app"></a>Gereksinimleri: Azure Market: Transact: SaaS uygulama  

| Gereksinim | Ayrıntılar |  
|:--- |:--- |  
| Faturalama ve ölçümü | Teklifiniz aylık bir düz hızında fiyatlandırılır. Kullanım tabanlı fiyatlandırma ve kullanım tabanlı *true yukarı* seçenekleri şu anda desteklenmiyor. |  
| İptali | Herhangi bir zamanda müşteri tarafından iptal edilebilen teklifidir. |  
| İşlem giriş sayfası | Azure ortak markalı işlem giriş sayfası ana bilgisayar. Giriş sayfası oluşturmak ve SaaS Hizmet hesabınızı yönetmek, müşterilerin sağlar. |  
| SaaS abonelik API | Etkileşimde bulunan bir hizmet oluşturmak, güncelleştirmek ve bir kullanıcı hesabı ve hizmet planını silmek için SaaS abonelikle sağlar. 24 saat içinde desteklenen tüm kritik API değişiklikleri gerekir. Kritik olmayan API değişiklikleri düzenli olarak güncelleştirilir. |  

#### <a name="requirements-azure-marketplace-transact-virtual-machine"></a>Gereksinimleri: Azure Market: Transact: sanal makine  

| Gereksinim | Ayrıntılar |  
|:--- |:--- | 
| Faturalama ve ölçümü | VM KLG veya Kullandıkça Öde aylık faturalama desteklemesi gerekir. |  
| Azure ile uyumlu sanal sabit disk (VHD) | Sanal makineleri, Windows veya Linux'ta oluşturulmalıdır.<ul> <li>Linux VHD oluşturma hakkında daha fazla bilgi için bir Azure uyumlu VHD (Linux tabanlı) bölümünde bulunan Oluştur ziyaret [docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation#2- Create-an-Azure-Compatible-VHD-Linux-based](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation#2-create-an-azure-compatible-vhd-linux-based).</li> <li>Windows VHD oluşturma hakkında daha fazla bilgi için bir Azure uyumlu VHD (Windows tabanlı) bölümünde bulunan Oluştur ziyaret [docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation#3- Create-an-Azure-Compatible-VHD-Windows-based](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation#3-create-an-azure-compatible-vhd-windows-based).</li> </ul> |  

## <a name="next-steps"></a>Sonraki adımlar
*   Ziyaret [Azure Marketi ve AppSource yayımcı Kılavuzu](./marketplace-publishers-guide.md) sayfası.  
 
---  
