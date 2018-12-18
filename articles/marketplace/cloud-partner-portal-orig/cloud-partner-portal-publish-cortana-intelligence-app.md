---
title: Cortana Intelligence | Microsoft Docs
description: Cortana Intelligence teklifi yayımlama.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/18/2018
ms.author: pbutlerm
ms.openlocfilehash: 74cde720af012b3355b813cf6da2b2bdf10b9b8e
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51257216"
---
# <a name="publish-a-cortana-intelligence-offer-using-the-cloud-partner-portal"></a>Bulut iş ortağı portalını kullanarak bir Cortana Intelligence teklifi yayımlama

Bu makalede bulut iş ortağı portalını kullanarak bir Cortana Intelligence teklifi yayımlama açıklar.

## <a name="prerequisites"></a>Önkoşullar

Bulut iş ortağı portalı rol tabanlı erişim sağlayan bir teklifi yayımlama üzerinde işbirliği yapmak katkıda bulunanlar portalına destekler. Daha fazla bilgi için [bulut portalı kullanıcıları Yönet](./cloud-partner-portal-manage-users.md).

Teklif, yayımcı adına yayımlanmadan önce hesabı, bir kişi ile \"sahibi\" rol uyacağınızı kabul edersiniz gerek [kullanım](https://azure.microsoft.com/support/legal/website-terms-of-use/), [Microsoft gizlilik bildirimi](https://www.microsoft.com/privacystatement/default.aspx), ve [Microsoft Azure sertifikası Program sözleşmesi](https://azure.microsoft.com/support/legal/marketplace/certified-program-agreement/).

## <a name="to-start-creating-a-cortana-inteligence-offer"></a>Cortana Inteligence teklif oluşturmaya başlamak için

Tüm önkoşulların karşılandığını sonra Cortana Inteligence teklifinizi yazma başlamaya hazır olursunuz.

1.  Oturum [bulut iş ortağı portalı](http://cloudpartner.azure.com/).
2.  Sol gezinti çubuğunda **+ yeni teklif**.
3. Seçin **Cortana Intelligence**.
4. Seçin **Düzenleyicisi** sekmesinde **yeni teklif** görünüm aşağıdaki seçenekleri yapılandırın:
    -   Teklif ayarları
    -   Teknik bilgileri
    -   StoreFront ayrıntıları
    -   Kişiler

    Yukarıdaki seçeneklerin her biri, doldurmanız forma görüntüler.  Her form alanlarını, kırmızı bir yıldızla işaretlenir gerekli (\*).

## <a name="to-configure-offer-settings"></a>Teklif ayarları yapılandırmak için

Teklif ayarları bir teklif kimliği, yayımcı kimliği ve teklif adı gibi teklif hakkında temel bilgiler sağlar.

### <a name="offer-id"></a>Teklif Kimliği

Bu teklif içinde bir yayımcı profili için benzersiz bir tanımlayıcıdır.
Bu kimliği, ürün URL'lerinde görülebilir. Yalnızca küçük harfli alfasayısal karakterler veya tirelerden (-) oluşabilir. Kimliği tire bitemez ve en çok 50 karakter olabilir. 
>[!Note]
>Bir teklif etkin durumda olduğunda bu alan kilitli.

**Örnek:**

Varsa yayımcı **contoso** Teklif kimliği ile bir teklif oluşturur *örnek Cortana Intelligence*, appsource'ta görünür "https://appsource.microsoft.com/marketplace/apps/**contoso**.\*Örnek-Cortana Intelligence\*? sekmesi genel bakış = ".

### <a name="publisher-id"></a>Yayımcı kimliği

Yayımcı profil açılır listesi. Bu teklif altında yayımlamak istediğiniz yayımcı profilini seçmek için kullanın.

>[!Note]
>Bir teklif etkin durumda olduğunda bu alan kilitli.

### <a name="name"></a>Ad

Teklifiniz için görünen adı. Bu ad görüntülenen [AppSource](https://appsource.microsoft.com). En fazla 50 karakter olabilir.

Teklif Ayarları'nda gerekli bilgileri seçin sağlayan tamamladığınızda **Kaydet** teklif teknik bilgi kısımdan taşımak için. seçeneği.

## <a name="to-configure-technical-info"></a>Teknik bilgi yapılandırmak için

Teklif sayfanızın görüntülenecek teknik bilgiler sağlamak için bu görünümü kullanın. Aşağıdaki alanlar, bu görünüm için geçerlidir.

### <a name="partner-mpn-id"></a>İş ortağı MPN kimliği

Kayıtlı bir Microsoft iş ortağı olduğunuzdan, Git [iş ortakları Web sitesine](https://partners.microsoft.com/) ve kuruluş iş ortağı kimliğinizi almak oturum açın Bu kimliği girin **MPN kimliği için iş ortağı**.

### <a name="analytics-components-used"></a>Kullanılan analytics bileşenleri

Teklifinizi kullanan tüm bileşenleri'ni seçin. En az 1 bileşeni seçin. Seçin **Microsoft R** yalnızca SQL 2016 R services, HDInsight Premium MRS lisanslarını herhangi birini kullanıyorsanız veya Vm'lerde çalışan MRS.

### <a name="additional-components-used"></a>Kullanılan ek bileşenleri

Çözümünüzü kullanan tüm diğer bileşenleri seçin.

### <a name="customer-name-using-solution"></a>Çözümü kullanarak müşteri adı

Bu çözüm, üretim ortamında kullanarak müşteri adı sağlayın. 

>[!Note]
>Bu bilgiler Appsource'ta yayımlanan değil. Yalnızca çözüm değerlendirme için de kullanılır.

### <a name="azure-consumption-requirement-met"></a>Azure tüketim gereksinim karşılanır?

Olup olmadığını en az $1 K müşteri başına aylık Azure paketi bileşenlerinin çözümü oluşturur veya çözüm Microsoft R. kullanıp kullanmadığını belirtir

>[!Note]
>Bu bilgiler Appsource'ta yayımlanan değil. Yalnızca çözüm değerlendirme için de kullanılır.

### <a name="demo-video-url"></a>Tanıtım Video URL'si

Satış öncesi takımınızın müşteriler gösterebilirsiniz video çözümü/uygulama için bir tanıtım için bir URL sağlayın. 

>[!Note]
>Bu bilgiler Appsource'ta yayımlanan değil. Yalnızca çözüm değerlendirme için de kullanılır.

#### <a name="demo-video-guidelines"></a>Tanıtım video yönergeleri

- Video uzunluğu 15 dakikadan daha kısa olmalıdır.
- Video, çözümün işlevselliğini kaydı en düşük düzeyde düzenlenmelidir. 
- Video, kullanıcıya yönelik işlevselliği önemli noktaları vurgulayan ve Gelişmiş analiz tümleştirmesini üzerinde odaklanır. 
- Tanıtım videoları **yapmamayı** herkese açık şekilde müşterilerin kullanımına ancak nasıl olması beklenir çözüm *olacaktır* müşteri adayına gösterilir. Bu nedenle, bunlar betikleştirilmiş ve tekrarlanabilir olmalıdır.
- Videonuzu oluşturmak için standart görüntü biçimi (örneğin, MPEG) aktarır herhangi bir ekran kaydı aracını kullanın. 

Aşağıdaki yönergeler, işletme için Skype'ı kullanarak video oluşturma işlemi gösterilmektedir. 

1. [Toplantı başlayın](https://support.office.com/article/Start-a-Skype-for-Business-conference-call-8dc8ac52-91ac-4db9-8672-11551fdaf997)
2. [Masaüstünüzü paylaşmak](https://support.office.com/article/Share-your-screen-in-Skype-for-Business-2d436dc9-d092-4ef1-83f1-dd9f7a7cd3fc)
3. [Kaydı başlatmak](https://support.office.com/article/Share-sites-or-documents-with-people-outside-your-organization-80e49744-e30f-44db-8d51-16661b1d4232)
4. Kaydı durdurduktan sonra [kaydınız yayımlamak için kayıt Yöneticisi'ni kullanın](https://support.office.com/article/Recording-Manager-save-and-publish-59a3beb7-c700-40cf-ab21-bc82a2b06351)

Kaydedilen videonuzu karşıya yükleyin, paylaşılan bir URL oluşturmanıza olanak sağlayan bir hizmet. Örneğin, bir [OneDrive veya Sharepoint Konuk bağlantıyı](https://support.office.com/article/Share-sites-or-documents-with-people-outside-your-organization-80e49744-e30f-44db-8d51-16661b1d4232).

### <a name="supported-regions"></a>Desteklenen bölgeler

Çözümünüzü desteklediği tüm bölgeleri seçin. En az 1, bölge seçin.

### <a name="power-bi-desktop-file-pbix"></a>Power BI Masaüstü dosyasını (.pbix)

Varsa, bir .pbix dosyasını karşıya yükleyin. Harici olarak başvurulmayan veri dosyasına alındığından emin olun. Yerleşik rapor sizin için oluşturacağız.

### <a name="solution-architecture"></a>Çözüm mimarisi

Çözümünüzün mimarisini ayrıntılı bir belge karşıya yükleyin. Çözüm mimarisi diyagramı Kılavuzu yüklemek için bkz [Gelişmiş analiz çözümü iş akışı şablonu](https://partnersprofilesint.blob.core.windows.net/partner-assets/documents/AppSource%20Solution%20Publishing%20Guide%20Docs/Advanced%20Analytics%20Solution%20Workflow%20Template%20V.2017.3.23.pptx).

>[!Note]
>Bu bilgiler Appsource'ta yayımlanan değil. Yalnızca çözüm değerlendirme için de kullanılır.

### <a name="select-segments"></a>Segment seçin

Tüm ilgili sektör segmentine seçin. Uygulamanız için listelenmemiş bir segment ise, segment adı girin **başkalarının** alan. Kesimin üç sözcük sınırlı olmalıdır.

### <a name="select-business-processes"></a>İş süreçlerini seçin

Çözümünüzü en iyi şekilde açıklayan ve iş süreçleri seçin. Uygulamanız için listelenmemiş tüm işlemleri, işlem adı girin **başkalarının** alan. İşlem için üç sözcük sınırlı olmalıdır.

### <a name="trial-info"></a>Deneme bilgileri

-   **SaaS deneme URL'si:** için deneme sürümü deneyimi uygulamanızın URL'sini girin.
-   **Test sürücü deneme URL'si:** için Test Sürüşü deneyimi uygulamanızın URL'sini girin.

Deneme sürümleri hakkında daha fazla bilgi için bkz. **uygulama türü** bu makalenin sonraki bölümde.

Teknik bilgileri, gerekli bilgileri seçin sağlayan tamamladığınızda **Kaydet** teklif mağaza ayrıntıları kısımdan taşımak için. 

## <a name="to-configure-storefront-details"></a>Mağaza ayrıntıları yapılandırmak için

### <a name="offer-summary"></a>Teklif özeti

Bu teklife ilişkin değer önerisi bir özetidir. Bu teklife ilişkin arama sayfasında görünür. Özet uzunluğu en fazla 100 karakterdir.

### <a name="offer-description"></a>Teklif açıklaması

Bu, uygulamanızın ayrıntı sayfasında görünecek açıklamasıdır.
Açıklama uzunluğu en fazla 1300 karakterdir.

Bu alan html etiketleri gibi ile içerik aldığını unutmayın \<p\>, \<h1\>, \<h2\>, \<li\>, içerik çok daha fazlasını yapmanıza olanak tanıyan, vb. edileni. Yayımlama Portalı takım çalışıp çalışmadığını çalıştırmalarınızı içeriği daha edileni yapmak mağaza ayrıntılarının önizlemesini görmek için etkinleştirmek için bir özellik ekleme - aşağıdakilerden herhangi bir gerçek zamanlı çevrimiçi html araçları gibi kullanabileceğiniz kullanın ancak bu arada, [ http://htmledit.squarefree.com ](http://htmledit.squarefree.com/) açıklamanızı nasıl görüneceğini görmek için.

Önerilen açıklama metni değerine göre alt bölümlere sahip bölümlere ayırmak için propositions, her bir alt başlık ile vurgulanmış biçimidir. Ziyaretçileri genellikle bakış gist uygulama adreslerini almak için alt başlıklar ve "Teklif Özeti" alanı ve neden, göz önünde bulundurmalıdır hızlı bir bakış uygulamada. Bu nedenle, kullanıcı almak önemli olan\'s dikkat vermek bunları bir neden ayrıntıları almak için okumaya devam edin.

#### <a name="partner-examples"></a>İş ortağı örnekleri

- [Neal Analytics Envanter iyileştirme](https://appsource.microsoft.com/product/web-apps/neal_analytics.8066ad01-1e61-40cd-bd33-9b86c65fa73a?tab=Overview)
- [Plexure perakende kişiselleştirme](https://appsource.microsoft.com/product/web-apps/plexure.c82dc2fc-817b-487e-ae83-1658c1bc8ff2?tab=Overview)
- [AvePoint Vatandaşlık Hizmetleri](https://appsource.microsoft.com/product/web-apps/avepoint.7738ac97-fd40-4ed3-aaab-327c3e0fe0b3?tab=Overview)

### <a name="industries"></a>Sektörler

Uygulamanızı en iyi hizalanır sektör seçin. Uygulamanızı birden çok sektörler için ile ilişkili ise bu alanı boş bırakın.

### <a name="categories"></a>Kategoriler

Uygulamanıza uygun olan kategorilerini seçin. En fazla iki kategorisi seçin.

### <a name="app-type"></a>Uygulama türü

Uygulamanızı Appsource'ta destekleyecek deneme türünü seçin. Aşağıdaki denemeleriyle seçin:
- **Ücretsiz** uygulamanızı ücretsiz olduğu anlamına gelir.
- **Deneme** müşteriler deneyebileceğinizi anlamına gelir, belirli bir süre için.
- **İstek için deneme** müşteriler, uygulamanın bir deneme sürümü isteyebilir anlamına gelir.

İş ortaklarına deneme deneyimleri iki tür Appsource'ta verebilirsiniz.

- **Deneme** seçeneği olarak da bilinir, **müşteri LED'i deneme (CLT)** şu türlerden biri olabilir: 
    - SaaS denemesi
        - Müşteri, siz veya iş ortağınız sağlayan bir URL'ye gidebilir. Müşteri, bir süre için SSO için deneme sürümü deneyimi Kutulu AAD Federasyon gider.
        - Kullanıcının yapılandırma/diğer kullanıcılara verilerinden ayırır çok kiracılı SaaS uygulamasıyla budur. Veya bu deneyimi yalnızca salt okunur işlemler kullanan bir alt kümesi sağlar.

        **Örnekler:**

        - [AFS POP perakende yürütme](https://appsource.microsoft.com/product/web-apps/afs.fa9fc926-3bc3-43dd-becd-3ef41b52c10b?tab=Overview)
        - [AvePoint Vatandaşlık Hizmetleri](https://appsource.microsoft.com/product/web-apps/avepoint.7738ac97-fd40-4ed3-aaab-327c3e0fe0b3?tab=Overview). (Bu uygulama, seçili bir kullanıcı kişiler kümesi için Temizle yolları seçkin bir deneyim sağlar.)

     - Test Sürüşü
        - Sizin (veya ortağınızın) çözümü veya bir kısmı paketlenmiş AppSource oluşturabileceğiniz Azure Resource Manager şablonlarını kullanarak ve. Appsource'ta uygulama bir iş ortağı abonelikte kutulama ve örnekleri vb. sıcak/soğuk havuz bakımını yapma zamanı ile yönetebilirsiniz.
        - Biz, şablonları ve bu seçeneği belirlerseniz yardımcı olması için örnek kodu sağlayabilir.

        **Örnekler:**

        - [Neal Analytics Envanter iyileştirme](https://appsource.microsoft.com/product/web-apps/neal_analytics.8066ad01-1e61-40cd-bd33-9b86c65fa73a?tab=Overview&tag=CISHome)

- **Deneme sürümü için istek** (**iş ortağı LED'i denemeler /. sys**) seçeneği, müşterilerin iş ortağı izleme için bir kişi bilgilerini formu doldurun gerektirir. İş ortağı izler ve sunum ya da uygulamanın deneme verir. Daha fazla bilgi için [AppSource deneme sürümü deneyimi Kılavuzu](https://aka.ms/trialexperienceforwebapps) video üst düzey bir genel bakış.

>[!Note]
>Verileri gösteren **müşteri LED'i denemeler** daha yüksek bir müşteri adayı oluşturma daha olası sahip **iş ortağı LED'i denemeler**.


### <a name="help-link-for-your-app"></a>Uygulamanız için Yardım bağlantısına

Uygulamanıza yönelik yardım bilgilerini içeren bir sayfasına bir URL sağlayın.

### <a name="supported-countriesregions"></a>Desteklenen ülkeler/bölgeler

Bu alan, burada teklifinizi deneme sürümü için kullanılabilir olacak ülkeler/bölgeler belirler.

![Desteklenen ülkeler/bölgeler](./media/cloud-partner-portal-publish-cortana-intelligence-app/CISScreenshot3.png)

### <a name="supported-languages"></a>Desteklenen diller

Uygulamanızın desteklediği dilleri seçin. Bu listede yer almayan dilleri uygulamanız destekliyorsa, teklifinizi yayımlayın ve adresinden bize e-posta: <appsource@microsoft.com> diğer dil desteği hakkında bize bildirin.

### <a name="app-version"></a>Uygulama sürümü

Uygulamanız için sürüm numarası girin.

### <a name="products-your-app-works-with"></a>Ürünleri ile uygulamanızı çalışır.

Uygulamanızı çalışır belirli ürünleri listeler. En fazla 3 ürünleri listeleyebilirsiniz. Bir ürün listelemek için **artı** (yanında yeni) ve açık metin alanı sizin için oluşturulur. Uygulamanızı çalışır bir ürün adını girin.

### <a name="hide-key"></a>Anahtar Gizle

Bu teklif genel görünümden gizlemek için teklif Önizleme URL'si ile birleştirilmiş bir anahtardır. Bir parola değil. Herhangi bir alfasayısal dize girebilirsiniz.

### <a name="offer-logo-small"></a>Teklif logo (küçük)

Bu logo, uygulamanızın arama sayfasında görünür. Png resim biçimi izin verilen tek bir biçimidir. 48 x 48 pikseldir görüntü boyutudur.

### <a name="offer-logo-large"></a>Teklif logo (büyük)

Bu logo, uygulamanızın ayrıntı sayfasında görünür. Png resim biçimi izin verilen tek bir biçimidir. Görüntü boyutu, yalnızca 48 piksel x 48 pikseldir.

### <a name="video"></a>Video

En fazla dört videoları karşıya yükleyebilirsiniz. Karşıya yüklemek istediğiniz her video için:
- Video adı sağlayın
- Bir URL'si (YouTube veya Vimeo yalnızca) sağlayın
- Bir küçük resim, video ile ilişkilendirilecek sağlar. Küçük resim png resim biçimi kullanmalıdır ve onun boyutu 1280 piksel olarak X 720 piksel olmalıdır. 

Yeni video eklemek için seçin **+ yeni**sonraki ekran görüntüsünde gösterilmiştir.

![Yeni video ekleme](./media/cloud-partner-portal-publish-cortana-intelligence-app/CISScreenshot4.png)

### <a name="document"></a>Belge

En fazla üç belgeler karşıya yükleyebilirsiniz. Her belgenin PDF dosya biçimi kullanmanız gerekir. Yeni bir belge eklemek için:

- Seçin **+ yeni**
- Bir belge adı girin
- Seçin **karşıya** bir belge yüklemek için.

![Yeni Belge Ekle](./media/cloud-partner-portal-publish-cortana-intelligence-app/CISScreenshot5.png)

### <a name="privacy-policy"></a>Gizlilik İlkesi

Uygulamanızın gizlilik ilkesi için URL girin

### <a name="terms-of-use"></a>Kullanım koşulları

Uygulamanızın kullanım koşulları girin. Appsource'ta müşterilerin uygulamanızı deneyebilmeniz için önce bu koşulları kabul etmesi gerekir.

>[!Note]
>Bu alanın içeriğini kabul kullanarak HTML etiketleri gibi html içeriğini < p\>, < h1\>, ve < li\>. Bu etiketleri, içeriği biçimlendirmek için kullanabilirsiniz. 

### <a name="lead-destination"></a>Hedef yol

Bir CRM sistemine adaylarınızı depolanacağı yeri seçin. 

Seçin **Azure tablo** bu CRM sistemleri birini kullanıyorsanız,:, Salesforce, Marketo veya Microsoft Dynamics CRM. 

CRM sistemine hakkında daha fazla bilgi için istediğiniz kullanmak için desteklenen sistemler için aşağıdaki bağlantılardan birini seçin.

-   [Azure tablosu](./cloud-partner-portal-lead-management-instructions-azure-table.md)
-   [Marketo](./cloud-partner-portal-lead-management-instructions-marketo.md)
-   [Microsoft Dynamics CRM](./cloud-partner-portal-lead-management-instructions-dynamics.md)
-   [Salesforce](./cloud-partner-portal-lead-management-instructions-salesforce.md)
