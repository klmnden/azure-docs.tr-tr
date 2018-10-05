---
title: SaaS uygulaması teknik yayımlama Kılavuzu'nu | Microsoft Docs
description: SaaS uygulamaları teklifler uygun markette yayımlama açıklanmaktadır.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/14/2018
ms.author: pbutlerm
ms.openlocfilehash: 9d73040f11549741643d96913c42df49594b8d41
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48811315"
---
<a name="saas-application-technical-publishing-guide"></a>SaaS uygulaması teknik yayımlama Kılavuzu
===========================================

SaaS uygulamaları teknik yayımlama Kılavuzu'na hoşgeldiniz! Bu kılavuz, aday ve mevcut yayımcıların uygulamalarını ve hizmetlerini AppSource ya da sunarak SaaS uygulamalarını kullanarak Azure Marketi'nde listelemek için yardımcı olmak için tasarlanmıştır.

Diğer tüm Market teklifleri genel bakış için bkz [Market yayımcı rehberi](https://aka.ms/sellerguide).


<a name="what-are-pre-requisites-for-publishing-a-saas-app"></a>Bir SaaS uygulaması yayımlamak için ön koşullar nelerdir?
-------------------------------------------------

Yayımlama Portalı izin vererek bir teklifi yayımlama doğru işbirliği yapmak birden çok kişiye portalında rol tabanlı erişim sağlar. Daha fazla bilgi için [Kullanıcıları Yönet](./cloud-partner-portal-manage-users.md). 

Teklif, yayımcı adına yayımlanmadan önce hesabı, bir kişi ile *sahibi* rol uyacağınızı kabul edersiniz gerek [kullanım](https://azure.microsoft.com/support/legal/website-terms-of-use/), [Microsoft PrivacyStatement](http://www.microsoft.com/privacystatement/default.aspx), ve [Microsoft Azure sertifikalı ProgramAgreement](https://azure.microsoft.com/support/legal/marketplace/certified-program-agreement/).


<a name="creating-an-offer"></a>Teklif oluşturma
-----------------

Bu bölümde, yeni bir SaaS uygulaması teklif oluşturma işlemi açıklanmaktadır.

![SaaS genel bakış sunar.](media/cpp-creating-saas-offers/saas-offer-overview.png)

SaaS uygulama teklifini aşağıdaki tabloda açıklanan beş bölümlerden oluşur:

| **Teklif bölümü**  | **Açıklama**                                                                                                     |
|--------------------|-------------------------------------------------------------------------------------------------------------------------------|
| Teklif ayarları     | SaaS uygulaması için benzersiz bir ad ve kimliği tanımlamak için yardımcı olur.                                                                         |
| Teknik bilgileri     | SaaS çözümünün türünü yapılandırmak ve uygulamanız için bağlantı ayrıntılarını sağlamak için yardımcı olur.                            |
| Test Sürüşü         | Teklifiniz, satın almadan önce test etmek müşterilerin olanak tanıyan bir hizmet tanımlamanızı sağlayan isteğe bağlı bir bölüm. |
| StoreFront ayrıntıları | Pazarlama, yasal, sağlama yönetimi ve listeleme bölümleri içerir:   <br/> -Pazarlama bölüm teklifini Market Kullanıcı Portalı'nda düzgün görüntülenmesi için gerekli logoları ve açıklama girmenize olanak sağlar.  <br/> -Sağlama Yönetimi Azure Market Son Kullanıcı Portalı'nda oluşturulan son kullanıcı müşteri adaylarını yeniden yönlendirileceği bir yerde tanımlamanızı sağlar.  <br/> -Yasal bölüm gizlilik ilkesi ve kullanım dönemi yasal belgeler girmenizi sağlar.  |
| İletişim            | Teklife ilişkin destek iletişim bilgileri girmenize olanak sağlar.                                                                  |
|  |  |


### <a name="creating-a-new-offer"></a>Yeni bir teklif oluşturma

Bulut iş ortağı portalında oturum açtıktan sonra seçin **yeni teklif** üzerinde mevcut tekliflerin bir menü görüntüler sol menü öğesi. Aşağıdaki görüntüde, bu tekliflerin bir örnek gösterilmektedir:

![SaaS yeni teklif](media/cpp-creating-saas-offers/saas-new-offer.png)

Listeden onaylanmış olan ve yeni bir teklif formu açılır bir teklif seçin.

![SaaS yeni teklif formu](media/cpp-creating-saas-offers/saas-new-offer-2.png)

Aşağıdaki tabloda, teklif alanlar açıklanır:

| **Teklif alanları** | **Açıklama**                                                                                            |
|------------------|----------------------------------------------------------------------------------------------------------- |
| Teklif Kimliği         | Bir yayımcı profilinde teklif için benzersiz bir tanımlayıcı. Bu kimliği, ürün URL'leri ve fatura raporlarda görünmez. Yalnızca küçük harfli alfasayısal karakterler veya tirelerden (-) oluşabilir. Kimliği tire bitemez ve en çok 50 karakter olabilir. Bir teklif etkin hale gelir, sonra bu alan kilitli olduğunu unutmayın. Contoso, yayımcı, Teklif kimliği örnek-vm ile bir teklif yayımlar, örneğin, bu Azure Marketi'nde görünür: [https://azuremarketplace.microsoft.com/marketplace/apps/contoso.sample-vm?tab=Overview](https://azuremarketplace.microsoft.com/) |
| Yayımcı kimliği     | Yayımcı kimliği, Market'te kendi benzersiz tanımlayıcısıdır. Yayımcı kimliğiniz bağlı tüm tekliflerinizin olmalıdır Yayımcı kimliği, teklif kaydedildikten sonra değiştirilemez.                                                                                       |
| Ad             | Bu, teklifiniz için görünen addır. Bu, Azure Marketi'nde hem de Azure portalında görünür addır. En fazla 50 karakter olabilir. Bir kılavuz ürününüzün tanınabilir bir marka adı eklemektir. Nasıl pazarlanmadığından olmadığı sürece, şirketinizin adını buraya dahil değildir. Bu teklif kendi Web pazarlama, adı tam olarak nasıl siteniz görünür olduğundan emin olun.             |
|  |  |

Tıklayın **Kaydet** ilerlemenizi kaydetmek için. Kullanıcınıza teklifiniz için ekleme planları sonraki bölümde açıklanmaktadır.


### <a name="technical-information"></a>Teknik bilgiler

Teknik bilgi bölümüne aşağıdaki bilgileri girmenizi sağlar:

![SaaS teknik bilgi sağlar.](media/cpp-creating-saas-offers/saas-new-offer-technical-info.png)

SaaS listesini veya ticari etkin olmasını önerilere şu en önemli kararıdır. SaaS listesini ise arasında seçmeniz gerekir:

-   Ücretsiz - SaaS uygulamasının müşterilerin uygulamanızı edinebileceğiniz bir URL sağlayın.
-   Ücretsiz deneme - Burada, satın alma teklifini önce müşteriler deneme çalıştırabilirsiniz SaaS uygulamasının bir URL sağlayın.
-   Bana - yalnızca ilgili müşteri adayı, bağlı bir yönetim sistemine sahip, istemek irtibat kurulmasını müşteriler için bu seçeneği sağlar ve bir müşteri adayı sizinle iletişime geçin.

Azure Marketi'nde ve Microsoft işlemler, select üzerinden ticaret etkinleştirmek istiyorsa bir SaaS uygulaması varsa **Azure üzerinden satın**.  
SaaS uygulamanızı bağlama hakkında daha fazla bilgi için bkz. [SaaS - Azure üzerinden satın](./cloud-partner-portal-saas-offer-subscriptions.md).


### <a name="test-drive"></a>Test Sürüşü

Müşterileriniz için bir deneme sürümü deneyimi oluşturma güvenle satın alabileceğiniz emin olmak için iyi bir uygulamadır. Deneme seçenekleri, Test Sürüşü en verimli oluşturma yüksek kaliteli bir müşteri adayları ve artan dönüştürme bu müşteri adayları var.

Müşterilere ürün uygulamasının temel özellikler ve avantajları, gerçek uygulama senaryoda gösterildiği uygulamalı, kendi kendine yol gösteren denemesini sağlar.

![SaaS teklifi Test Sürüşü](media/cpp-creating-saas-offers/saas-new-offer-test-drive.png)


#### <a name="how-a-test-drive-works"></a>Bir Test sürüşüne nasıl çalışır?

Bir müşteri adayını arar ve uygulamanızı Marketi'nde bulur. Müşteri açar ve kullanım koşullarını kabul eder. Bu noktada, müşteri ile takip etmek için yüksek oranda tam bir müşteri adayı alırken saat için sabit sayıda denemek için önceden yapılandırılmış ortamınıza alır.

![SaaS teklifi test sürücü 2](media/cpp-creating-saas-offers/saas-new-offer-test-drive-2.png)

Uygulamanızı nasıl karmaşık olursa olsun, Microsoft Test Sürüşü, müşteri için ürününüzün hayata yardımcı olur. Her ürün, senaryo ve Market olan türüne göre kullanılabilir Test Sürüşleri, üç farklı türü vardır.

-   **Azure Resource Manager** -bir Azure ARM Test Sürüşü olan yayımcı tarafından oluşturulan bir çözümünü oluşturan tüm Azure kaynakları içeren bir dağıtım şablonu. Bu tür bir Test Sürüşü uygun ürünler, yalnızca Azure kaynakları şunlardır.
-   **Mantıksal uygulama** -bir mantıksal uygulamayı Test Sürüşü olan bir dağıtım şablonu tüm karmaşık çözüm mimarileri kapsayacak şekilde tasarlanmıştır. Tüm Dynamics uygulamaları ve özel ürünler bu tür bir Test Sürüşü kullanmanız gerekir.
-   **Power BI** -bir Power BI Test Sürüşü özel oluşturulan bir panoya katıştırılmış bir bağlantı oluşur. Test Sürüşü bu tür bir etkileşimli Power BI görsel kullanması gereken göstermek istediği herhangi bir ürünü. Karşıya yüklemek için ihtiyacınız olan, katıştırılmış Power BI URL'si.

Bir test sürücü eklemek için ana yayımlama adımlar şunlardır:

1.  Test Sürüşü senaryonuzu tanımlama
2.  Derleme ve/veya Resource Manager şablonunuzu değiştirme
3.  Test Sürüşü hakkında adım adım el ile oluşturma
4.  Teklifinizi yeniden yayımlayın

Daha fazla bilgi için [Test Sürüşü](./what-is-test-drive.md).


### <a name="storefront-details"></a>StoreFront ayrıntıları

SaaS uygulamaları, ilk iki bölümlerini bir özeti ve uygulamanızla ilgili olarak sağlanacak açıklama gerekir.

![SaaS teklif mağaza ayrıntıları](media/cpp-creating-saas-offers/saas-new-offer-storefront-details.png)

Bu teklif aşağıdaki tabloda açıklanmıştır\'s mağaza ayrıntıları:

| **Teklif alanları**        | **Açıklama**                                                                                          |
|-------------------------| ---------------------------------------------------------------------------------------------------------|
| Teklif özeti           | Ürününüzün değer önerisi özeti. Bu teklife ilişkin arama sayfasında görünür. En fazla 100 karakter olmalıdır.   |
| Teklif açıklaması       | Uygulama Ayrıntıları sayfasında görünür açıklaması. İzin verilen en büyük 1300 karakter *Not* Bu alan HTML etiketleri gibi & ltp içerik kabul\>, & lth1\>, & lth2\>, & ltli\>, olanak tanıyan, vb. içeriği daha edileni olun. Yayımlama Portalı takım çalışıp çalışmadığını çalıştırmalarınızı içeriği daha edileni yapmak mağaza ayrıntılarının önizlemesini görmek için etkinleştirmek için bir özellik ekleme - çevrimiçi bir gerçek zamanlı HTML araçları gibi kullanabileceğiniz herhangi kullanın ancak bu arada, http://htmledit.squarefree.com görmek için açıklamanızı nasıl görüneceği. |
| Sektörler              | Teklifiniz için en iyi hizalanır sektörler seçin. Uygulamanızı birden çok sektörler için ile ilişkili ise, en fazla iki seçebilirsiniz. |
| Önerilen kategorileri    | Teklifiniz için en iyi hizalanır kategorilerini seçin. En fazla üç kategoriden seçebilirsiniz.     |
| Uygulama sürümü     | Uygulamanızı sürüm numarasını girin                                                                |
| Arama anahtar sözcükleri (en fazla 3) | Müşterilerin uygulamanızı Marketi vitrini Web sitesinde bulmak için kullanabileceğiniz en fazla üç arama anahtar sözcükleri girin. |
|  |  |


### <a name="marketing-artifacts"></a>Pazarlama Yapıtları

Pazarlama yapıtları bölümü, logolar, videoları, ekran görüntüleri ve belgeler gibi varlıkları pazarlama Azure Marketi'nde tanımlamanıza olanak verir:

![Pazarlama yapıtları SaaS teklifi](media/cpp-creating-saas-offers/saas-new-offer-marketing-artifacts.png)

Aşağıdaki tabloda, pazarlama alanlar açıklanır:

| **Teklif alanları** | **Açıklama**                                                                                                          |
|------------------| ------------------------------------------------------------------------------------------------------------------------ |
| Logo            | Eğer bir **Azure üzerinden satın** SaaS uygulamasının tüm logosu görüntüleri sağlamalıdır. Bir liste olması durumunda, yalnızca 2 logo gerekli değildir. Bulut iş ortağı Portalı'nda karşıya logo aşağıdaki yönergeleri kullanmanız gerekir:     <br/> -Sayısı birincil ve ikincil renk logonuzu düşük tutun. Azure tasarımının basit bir renk paleti vardır.     <br/> -Sorunlarını giderme siyah veya beyaz arka plan rengi logonuz olarak kullanmaktan kaçının. Azure Portal'ın Tema renkleri siyah beyaz ' dir. Bunun yerine, logonuzu Azure portalında belirgin hale getirir bazı renk kullanın. Basit birincil renkleri öneririz. Saydam arka plan kullanıyorsanız logosu ve metin siyah, beyaz veya mavi olmadığından emin olun.     <br/> -Arka plan gradyan logosunu işaret kullanmayın.     <br/> -Metin, hatta şirket veya marka adı logosunu yerleştirmekten kaçının. Logonuzu Görünüm ve yapısını 'düz' olmalıdır ve gradyanları kaçınmalıdır.    <br/> -Logo resmi uzatılması gerektiğini değil.                   |
| Videolar           | Teklifinizin videoların bağlantılarını eklemenizi sağlar. Teklifinizle birlikte müşterilere gösterilir, YouTube ve/veya Vimeo video bağlantıları kullanabilirsiniz. Bir küçük resim, video 1280 x 720 piksel png görüntüsü ile girmeniz gerekir. Teklif başına dört video en fazla olabilir. |
| Belgeler        | Teklifinizin pazarlama belgeleri eklemenizi sağlar. Tüm belgeleri, PDF biçiminde olmalıdır ve en fazla üç belgeler Teklif başına olabilir.                                                                                                                                                      |
| Ekran görüntüleri      | Teklifinizin ekran görüntüleri eklemenizi sağlar. Teklif eklenebilecek beş ekran görüntüleri en fazla yoktur. En yüksek görüntü boyutu 1280 x 720 pikseldir.                                                                                                                                             |
| Faydalı Bağlantılar     | Mimari diyagramları noktasına veya bir müşteri görmek istediğiniz diğer Web sitelerinin Yardım teklifiniz için dış URL eklemenize olanak sağlar.                                                                                                                                                              |
|  |  |

Aşağıdaki görüntüde, bilgileri bir Market arama sonucunda nasıl görüntüleneceğini gösterir:

![SaaS pazarlama yayımcı bilgileri sunar.](media/cpp-creating-saas-offers/saas-new-offer-marketing-publisher-info.png)

Aşağıdaki görüntüde, daha küçük teklif kutucuğa müşteri tıkladıktan sonra teklifi Market'te nasıl görüntüleneceğini gösterir:

![Pazarlama yayımcı info2 SaaS teklifi](media/cpp-creating-saas-offers/saas-new-offer-marketing-publisher-info-2.png)


### <a name="lead-management-and-legal-information"></a>Tedarik yönetimi ve yasal bilgiler

Yasal bölümü teklife ilişkin yasal belgeler ayarlamanıza olanak sağlar.
Her SaaS uygulaması teklif için gereken iki yasal belgeler: gizlilik ilkesini ve kullanım koşulları. Daha fazla bilgi için bkz.  
[Müşteri adayları yapılandırma](./cloud-partner-portal-get-customer-leads.md).

![SaaS pazarlama yasal bilgileri sunar.](media/cpp-creating-saas-offers/saas-new-offer-legal-info.png)

Aşağıdaki tabloda, yasal bölüm özellikleri açıklanmaktadır:

| **Teklif alanları**   | **Açıklama**                                                                                 |
|--------------------| ----------------------------------------------------------------------------------------------- |
| Gizlilik İlkesi URL'si | Şirketinizin gizlilik ilkesi URL'si.                                                       |
| Kullanım Koşulları       | Kullanım koşullarını teklifiniz için. Yazın veya kopyalayıp buraya kullanım koşulları. Temel HTML kullanım verilir, gibi bu alanın alan html etiketleri gibi & ltp içerik\>, & lth1\>, & lth2\>, & ltli\>vb. *Önemli* gözden geçirin ve oluşturduğunuz teklif göndermeden önce bir tarayıcıda HTML Önizleme öneririz. |
|  |  |


### <a name="contact-information"></a>İletişim Bilgileri

Bu bölümde, müşteriler bu tekliften yararlanmaya desteklemek için sorumlu olacağını şirketinizdeki desteği iletişim bilgilerini girer.
Üç ana alan vardır: mühendislik birimi ilgili kişisi, destek ilgili kişisi ve destek URL'si:

![SaaS pazarlama iletişim bilgileri sunar.](media/cpp-creating-saas-offers/saas-new-offer-contact-info.png)


| **lgili kişi**         | **Açıklama**                                                                                                                          |
|---------------------|------------------------------------------------------------------------------------------------------------------------------------------|
| Mühendislik birimi ilgili kişisi | Ad, e-posta ve Microsoft, tüm destek ve iş sorunlarını başvurabilirsiniz bir mühendislik iletişim telefon numarası sağlayın. |
| Destek ekibiyle iletişime geçin     | Ad, e-posta, telefon numarası sağlayın ve Destek, destek istekleri varsa, müşterilerinizin başvurabilirsiniz URL'si.                  |
|  |  |


Teklifinizi hazırdır ve yayımlama isabet sonra teklif sertifika gider. SaaS uygulamanızı geçici bir liste veya seçili sahip satma, Azure üzerinden uç noktaları varsa, URL'sini de test el ile bir doğrulama test ederiz. Bu elle onay sırasında uygun mağaza uygulamanızı (AppSource, Azure Market veya her ikisi de) gösterilmesi gerekir ayrıca karar.

<a name="updating-the-offer"></a>Teklif güncelleştiriliyor
------------------

Yayımlandı ve canlı sonra teklifinizi yapmak isteyebilirsiniz güncelleştirmeleri çeşitli tür vardır. Teklifinizi yeni sürümü için yaptığınız tüm değişiklikler kaydedilir ve Market'te yansıtacak biçimde yeniden yayımlandı.

<a name="deleting-an-existing-offer"></a>Var olan bir teklif siliniyor
--------------------------

Teklifinizi Market'ten kaldırmaya karar verebilirsiniz. Teklif kaldırma işlemi, yeni müşteriler artık satın alabilir veya teklifiniz, dağıtma, var olan müşterileri, herhangi bir etkisi yoktur ancak sağlar. Teklif Sonlandırma, sizinle mevcut müşterileriniz arasındaki hizmet ve/veya lisans sözleşmesini iptal etme işlemidir.

Yönergeler ve teklif kaldırma ve sonlandırmayla ilgili ilkeler Microsoft Market yayımcı anlaşması tarafından yönetilir (bkz. Bölüm 7) ve katılım ilkeleri (bkz. Bölüm 6.2). Bu bölümde farklı desteklenen silme senaryolar ve adımlar hakkında bilgi için alabilir.

### <a name="delete-the-live-offer"></a>Canlı teklifi Sil

Canlı bir teklifin kaldırılması için istekte varsa dikkate edilmesi gereken çeşitli unsur vardır. Destek ekibinden Canlı teklifini Azure Marketi'nden kaldırma yönergeleri almak için aşağıdaki adımları izleyin:

1.  Bu bağlantıyı kullanarak bir destek bileti oluşturun
2.  Sorun türü listesinde **tekliflerini yönetmenize**ve kategori listede **bir teklif ve/veya SKU üretimde zaten değiştirme**.
3.  İsteği Gönder

Destek ekibine teklif silme işleminde size yol gösterecektir.

> [!NOTE] 
> Teklif siliniyor, bu teklifi satın alımlarında geçerli etkilemez. Bu müşteri satın alma işlemleri, önceki gibi çalışmaya devam eder.
Silme işlemi tamamlandıktan sonra ancak teklif herhangi yeni satın alma işlemleri için kullanılabilir değil.
