---
title: "Erişim denetimi (Java) kullanma | Microsoft Docs"
description: "Geliştirme ve erişim denetimi Azure içinde Java ile kullanma hakkında bilgi edinin."
services: active-directory
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 247dfd59-0221-4193-97ec-4f3ebe01d3c7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.custom: aaddev
ms.openlocfilehash: 698403d181e1fee09bb4692290c92203ded97ba4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-authenticate-web-users-with-azure-access-control-service-using-eclipse"></a>Eclipse kullanarak Azure erişim denetimi hizmeti ile Web kullanıcıların kimliklerini nasıl
Bu kılavuz Eclipse için Azure erişim denetimi Hizmeti'nden (ACS) Azure Araç Seti içinde kullanmak nasıl yapacağınızı gösterir. ACS hakkında daha fazla bilgi için bkz: [sonraki adımlar](#next_steps) bölümü.

> [!NOTE]
> Azure Erişim Hizmetleri Denetim filtresi topluluk teknoloji önizlemesidir. Yayın öncesi yazılım olarak bunu resmi olarak Microsoft tarafından desteklenmiyor.
> 
> 

## <a name="what-is-acs"></a>ACS nedir?
Çoğu geliştirici kimlik uzmanlar değildir ve genellikle zaman geliştirme kimlik doğrulama ve yetkilendirme mekanizmaları kullanıcıların uygulamaları ve Hizmetleri için harcadıkları istemiyorsanız. ACS faktörü karmaşık kimlik doğrulaması mantığı kodunuzu içine gerek kalmadan web uygulamaları ve hizmetlerine erişmek isteyen kullanıcıların kimliğini doğrulamak için kolay bir yol sağlayan bir Azure hizmetidir.

ACS aşağıdaki özellikler mevcuttur:

* Windows Identity Foundation (WIF) ile tümleştirme.
* Windows Live ID, Google, Yahoo! ve Facebook dahil olmak üzere popüler web kimlik sağlayıcıları (IP) desteği.
* Active Directory Federasyon Hizmetleri (AD FS) için destek 2.0.
* Açık Veri Protokolü (OData)-tabanlı ACS ayarları için programlı erişim sağlayan yönetim hizmeti.
* ACS ayarları için yönetici erişimine izin veren bir Yönetim Portalı.

ACS hakkında daha fazla bilgi için bkz: [erişim denetimi hizmeti 2.0][Access Control Service 2.0].

## <a name="concepts"></a>Kavramlar
Azure ACS sorumluları talep tabanlı kimlik - bulutta veya şirket içi çalışan uygulamalar için kimlik doğrulama mekanizmaları oluşturma için tutarlı bir yaklaşım, yerleşik olarak bulunur. Talep tabanlı kimlik ihtiyaç duydukları diğer kuruluşlardaki ve Internet'te, kuruluş içindeki kullanıcıların kimlik bilgilerini almak üzere uygulama ve hizmetler için ortak bir yol sağlar.

Bu Kılavuzu'ndaki görevleri tamamlamak için aşağıdaki kavramları anlamanız gerekir:

**İstemci** -nasıl yapılır bu kılavuzun bağlamında, web uygulamanıza erişmek için deneyen bir tarayıcı budur.

**Bağlı olan taraf (RP) uygulama** -bir Web sitesi veya bir dış yetkilisi kimlik doğrulaması outsources hizmeti, bir RP uygulamasıdır. Kimlik terimler RP bu yetkilisine güvendiğinden söyleyin. Bu kılavuz, uygulamanızı güven ACS yapılandırma açıklanmaktadır.

**Belirteç** -genellikle bir kullanıcının başarılı kimlik doğrulama sırasında verilen güvenlik verilerinin bir koleksiyonunu bir belirteçtir. Bir dizi içeriyor *talep*, kimliği doğrulanmış kullanıcının öznitelikleri. Bir talep bir kullanıcının adını gösterebilir, bir kullanıcının yaşı vb. için bir kullanıcı rolü için bir tanımlayıcı ait. Bir belirteç genellikle dijital olarak imzalanmış, bu her zaman geri verenini sağlanmasına ve içeriği ile değiştirilmemesi anlamına gelir. Bir kullanıcı, RP uygulama güvendiği bir yetkili tarafından verilen geçerli bir belirteci sunarak RP uygulamaya erişim kazanır.

**Kimlik sağlayıcısı (IP)** -IP kullanıcı kimliklerini doğrular ve güvenlik belirteçleri bir yetkili değil. Belirteç, gerçek iş güvenlik belirteci hizmeti (STS) adlı bir özel hizmet olsa uygulanır. Windows Live ID, Facebook, iş kullanıcı depoları (örneğin, Active Directory) ve benzeri IP'leri tipik örnekleri içerir.
ACS IP güvenecek şekilde yapılandırıldığında, sistem, kabul etmek ve bu IP tarafından yayınlanan belirteçleri doğrulamak. ACS, uygulamanızın ACS güvendiğinde, anında uygulamanızdan tüm kimliği doğrulanmış kullanıcılara tüm IP'ler ACS güvendiği sizin adınıza sunabileceğiniz, yani birden çok IP aynı anda güvenebilirsiniz.

**Federasyon sağlayıcısı (FP)** -IP'leri kullanıcılar, doğrudan bilgiye sahip ve bunları kendi kimlik bilgilerini kullanarak kimlik doğrulaması ve sorun hakkında ne bunlarla ilgili bildikleri talep. Bir Federasyon sağlayıcı (FP) yetkilisi farklı türüdür: kullanıcıların doğrudan kimlik doğrulaması yerine, bir aracı ve aracıların arasındaki kimlik doğrulaması bir RP bir görür veya daha fazla IP. IP ve FPs belirteçleri güvenlik, dolayısıyla her ikisi de güvenlik belirteci Hizmetleri (STS) kullanın. ACS bir dp ' dir.

**ACS kuralı altyapısı** -güvenilen IP'leri gelen belirteçlerinden RP tarafından tüketilen yönelik olarak dönüştürmek için kullanılan mantığı basit talep dönüştürme kuralları formunda kod oluşturulmuş. ACS, RP için belirtilen dönüşüm mantığı uygulamak mvc'deki bir kuralı altyapısı sunar.

**ACS Namespace** -ayarlarınızı düzenlemek için kullandığınız ACS en üst düzey bölümünü bir ad alanıdır. Bir ad alanı güvendiğiniz IP'leri sunmak istiyorsanız RP uygulamalar listesini tutar, kural beklediğiniz kuralları ile gelen belirteçleri işlemek için altyapısı ve benzeri. Bir ad alanı, uygulama ve geliştirici tarafından kendi işlevini gerçekleştirmesi için ACS almak için kullanılacak çeşitli uç noktalarını kullanıma sunar.

Aşağıdaki şekilde ACS kimlik doğrulaması bir web uygulaması ile nasıl çalıştığı gösterilmektedir:

![ACS Akış Diyagramı][acs_flow]

1. (Bu durumda bir tarayıcıda) istemci bir sayfa RP ister.
2. İstek henüz kimlik doğrulaması, RP kullanıcı güvendiği yetkilisine yönlendirir olduğundan, ACS olduğu. ACS bu RP için belirtilmiş IP'leri seçimi ile kullanıcı sayısını gösterir. Kullanıcı, uygun IP seçer.
3. İstemci IP'ın kimlik doğrulama sayfasına gözatar ve kullanıcının oturum açmasını ister.
4. İstemci (örneğin, kimlik bilgilerini girdiği kimlik) doğrulandıktan sonra IP güvenlik belirteci verir.
5. Bir güvenlik belirteci gönderdikten, ACS için istemci IP yönlendirir ve ACS IP tarafından verilen güvenlik belirteci istemciye gönderir.
6. ACS kimlik ACS kuralları motoruna bu belirteç talep, çıktı kimlik talepleri hesaplar ve bu çıkış talepleri içeren yeni bir güvenlik belirteci verir girişleri IP tarafından verilen güvenlik belirteci doğrular.
7. ACS istemciyi RP yönlendirir. İstemci RP ACS tarafından verilen yeni güvenlik belirteci gönderir. RP ACS tarafından verilen güvenlik belirteci imzayı doğrular, bu belirtecinizdeki talepleri doğrular ve başlangıçta istenen sayfasını döndürür.

## <a name="prerequisites"></a>Ön koşullar
Bu Kılavuzu'ndaki görevleri tamamlamak için aşağıdakiler gerekir:

* Bir Java Geliştirme Seti (JDK), v 1.6 veya sonraki sürümleri.
* Java EE geliştiricileri için Eclipse IDE Indigo olarak biliniyordu veya üzeri. Bu adresten yüklenebilir <http://www.eclipse.org/downloads/>. 
* Java tabanlı web sunucusu ya da Apache Tomcat, GlassFish, JBoss uygulama sunucusu veya Jetty gibi uygulama sunucusu dağıtımı.
* öğesinden alınan bir Azure aboneliği <http://www.microsoft.com/windowsazure/offers/>.
* Eclipse, Azure Toolkit Nisan 2014 sürümünden veya sonraki bir sürümü. Daha fazla bilgi için bkz: [Azure araç setini Eclipse için yükleme](http://msdn.microsoft.com/library/windowsazure/hh690946.aspx).
* Uygulamanızla birlikte kullanmak için bir X.509 sertifikası. Bu sertifika ortak sertifika (.cer) ve kişisel bilgi değişimi gerekir (. PFX) biçimi. (Bu sertifikayı oluşturmak için seçenekler daha sonra Bu öğreticide açıklanacaktır).
* Azure ile benzerlik işlem sırasında ele alınan öykünücüsü ve dağıtım teknikleri [bir Hello World uygulamasının Azure için Eclipse'te oluşturma](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx).

## <a name="create-an-acs-namespace"></a>Bir ACS Namespace oluşturma
Azure'da erişim denetimi Hizmeti'nden (ACS) kullanmaya başlamak için bir ACS ad alanı oluşturmanız gerekir. Ad alanı, uygulamanızda ACS kaynaklardan adresleme için benzersiz bir kapsam sağlar.

1. İçine oturum [Azure Yönetim Portalı][Azure Management Portal].
2. Tıklatın **Active Directory**. 
3. Yeni bir erişim denetimi ad alanı oluşturmak için tıklatın **yeni**, tıklatın **uygulama hizmetleri**, tıklatın **erişim denetimi**ve ardından **hızlı Oluştur**. 
4. Ad alanı için bir ad girin. Azure adının benzersiz olduğunu doğrular.
5. Ad alanı kullanıldığı bölgeyi seçin. En iyi performans için uygulamanızın dağıttığınız bölge kullanın.
6. Birden fazla aboneliğiniz varsa, ACS ad alanı için kullanmak istediğiniz aboneliği seçin.
7. **Oluştur**'a tıklayın.

Azure oluşturur ve ad alanını etkinleştirir. Yeni ad alanı durumu olana kadar bekleyin **etkin** devam etmeden önce. 

## <a name="add-identity-providers"></a>Kimlik sağlayıcısı ekleyin
Bu görevde, RP uygulamanız ile kimlik doğrulaması için kullanılacak IP ekleyin. Tanıtım amacıyla, bu görev Windows Live bir IP eklemeyi gösterir, ancak ACS yönetim Portalı'nda listelenen IP'leri birini kullanabilirsiniz.

1. İçinde [Azure Yönetim Portalı][Azure Management Portal], tıklatın **Active Directory**, bir erişim denetimi ad alanını seçin ve ardından **Yönet**. ACS yönetim Portalı'nı açar.
2. ACS yönetim portalı sol gezinti bölmesinde tıklayın **kimlik sağlayıcıları**.
3. Windows Live ID, varsayılan olarak etkindir ve silinemez. Bu öğreticinin amaçları doğrultusunda, yalnızca Windows Live ID kullanılır. Bu, ancak diğer IP'leri tıklayarak eklediğiniz ekrandır **Ekle** düğmesi.

Windows Live ID ACS ad alanınız için bir IP olarak etkinleştirilmiştir. Ardından, Java web uygulamanıza (daha sonra oluşturulacak) belirttiğiniz bir RP olarak.

## <a name="add-a-relying-party-application"></a>Bir bağlı olan taraf uygulaması ekleyin
Bu görevde, Java web uygulamanıza geçerli RP uygulama olarak tanımak için ACS'yi yapılandırın.

1. ACS yönetim portalı tıklatın **bağlı olan taraf uygulamaları**.
2. Üzerinde **bağlı olan taraf uygulamaları** sayfasında, **Ekle**.
3. Üzerinde **bağlı olan taraf uygulaması Ekle** sayfasında, aşağıdakileri yapın:
   
   1. İçinde **adı**, RP adını yazın. Bu öğreticinin amaçları doğrultusunda yazın **Azure Web uygulaması**.
   2. İçinde **modu**seçin **ayarlarını girin el ile**.
   3. İçinde **bölge**, ACS tarafından verilen güvenlik belirtecin geçerli olduğu URI yazın. Bu görev için yazın **http://localhost: 8080 /**.
      ![İşlem öykünücüsü kullanmak için bağlı olan taraf bölgesi][relying_party_realm_emulator]
   4. İçinde **dönüş URL'si** ACS için güvenlik belirteci verir URL'sini yazın. Bu görev için yazın **http://localhost:8080/MyACSHelloWorld/index.jsp**
      ![bağlı olan taraf işlem öykünücüsü kullanmak için URL döndürür][relying_party_return_url_emulator]
   5. Geri kalan alanları varsayılan değerleri kabul edin.
4. **Kaydet** düğmesine tıklayın.

Azure işlem öykünücüsü çalıştırdığınızda artık başarıyla Java web uygulamanıza yapılandırdınız (http://localhost: 8080 adresindeki /) bir RP ACS ad alanı içinde olmalıdır. Ardından, RP talebini işlemek için ACS kullanan kurallar oluşturun.

## <a name="create-rules"></a>Kuralları oluşturma
Bu görevde, talepler, RP Ip'lerden nasıl geçirilir sürücü kuralları tanımlar. Bu kılavuz amacıyla, biz yalnızca giriş talep türlerini ve değerlerini doğrudan çıkış belirteçte filtreleme veya bunları değiştirme kopyalamak için ACS yapılandırır.

1. ACS yönetim portalı ana sayfasında, tıklatın **Kural gruplarını**.
2. Üzerinde **kuralı grupları** sayfasında, **Azure Web uygulaması için varsayılan kuralı grubu**.
3. Üzerinde **kural grubunu Düzenle** sayfasında, **Generate**.
4. Üzerinde **kuralları oluştur: Azure Web uygulaması için varsayılan kuralı grubu** sayfasında, Windows Live ID işaretli emin olun ve ardından **Generate**.    
5. Üzerinde **kural grubunu Düzenle** sayfasında, **kaydetmek**.

## <a name="upload-a-certificate-to-your-acs-namespace"></a>Bir sertifika için ACS ad alanınızı karşıya yükle
Bu görevde, karşıya bir. ACS ad alanınızı tarafından oluşturulan belirteç isteklerini imzalamak için kullanılan PFX sertifikası.

1. ACS yönetim portalı ana sayfasında, tıklatın **sertifikaları ve anahtarları**.
2. Üzerinde **sertifikaları ve anahtarları** sayfasında, **Ekle** yukarıda **belirteç imzalama**.
3. Üzerinde **eklemek belirteç imzalama sertifikası veya anahtar** sayfa:
   1. İçinde **için kullanılan** 'yi tıklatın **bağlı olan taraf uygulaması** seçip **Azure Web uygulaması** (Bu, daha önce bağlı olan taraf uygulamanızın adı ayarlayın).
   2. İçinde **türü** bölümünde, select **X.509 sertifikası**.
   3. İçinde **sertifika** bölümünde Gözat düğmesine tıklayın ve kullanmak istediğiniz X.509 sertifika dosyasına gidin. Bu bir. PFX dosyası. Dosyayı seçin, **açık**ve sertifika parolası enter **parola** metin kutusu. Test amacıyla, bir self-signed sertifika kullanmak unutmayın. Kendinden imzalı bir sertifika oluşturmak için kullanın **yeni** düğmesini **ACS filtre kitaplığı** iletişim (aşağıda açıklanmıştır) ya da kullanım **encutil.exe** yardımcı programı'ndan [proje Web sitesi] [ project website] Java için Azure Starter Kit.
   4. Emin **olun birincil** denetlenir. **Eklemek belirteç imzalama sertifikası veya anahtar** sayfa şuna benzer görünmelidir.
       ![Belirteç imzalama sertifikası ekleme][add_token_signing_cert]
   5. Tıklatın **kaydetmek** ayarlarınızı kaydedip kapatmak için **eklemek belirteç imzalama sertifikası veya anahtar** sayfası.

Ardından, uygulama tümleştirmesi sayfasındaki bilgileri gözden geçirin ve Java web uygulamanızı ACS kullanacak şekilde yapılandırmanız gerekir URI'yi kopyalayın.

## <a name="review-the-application-integration-page"></a>Uygulama Tümleştirme sayfasını inceleyin
Tüm bilgi ve kod Java web uygulamanıza (uygulama ACS yönetim Portalı'nın uygulama tümleştirmesi sayfasında ACS ile çalışmak için RP) yapılandırmak gerekli bulabilirsiniz. Java web uygulamanızı federe kimlik doğrulaması için yapılandırırken bu bilgileri gerekir.

1. ACS yönetim portalı tıklatın **uygulama tümleştirmesi**.  
2. İçinde **uygulama tümleştirmesi** sayfasında, **oturum açma sayfaları**.
3. İçinde **oturum açma sayfası tümleştirme** sayfasında, **Azure Web uygulaması**.

İçinde **oturum açma sayfası tümleştirme: Azure Web uygulaması** sayfasında, listelenen URL **seçenek 1: bir ACS barındırılan oturum açma sayfasına bağlantı** Java web uygulamanızda kullanılacak. Java uygulamanızı Azure erişim denetimi Hizmetleri filtre kitaplığı eklediğinizde, bu değer gerekir.

## <a name="create-a-java-web-application"></a>Bir Java web uygulaması oluşturma
1. Eclipse içinde menüsünde tıklatın **dosya**, tıklatın **yeni**ve ardından **dinamik Web projesi**. (Görmüyorsanız **dinamik Web projesi** tıkladıktan sonra kullanılabilir bir proje listelenen **dosya**, **yeni**, aşağıdakileri yapın: tıklatın **dosya**,'ı tıklatın **yeni**,'ı tıklatın **proje**, genişletin **Web**,'ı tıklatın **dinamik Web projesi**, tıklatıp **sonraki**.) Bu öğreticinin amaçları doğrultusunda, proje adı **MyACSHelloWorld**. (Emin bu adı kullanın, bu öğreticide sonraki adımlarda beklediğiniz MyACSHelloWorld adlandırılması, WAR dosyası). Ekranınızın aşağıdakine benzer görünür:
   
    ![ACS exampple için Hello World projesi oluşturma][create_acs_hello_world]
   
    **Son**'a tıklayın.
2. Eclipse'nın Proje Gezgini görünümü içinde genişletmek **MyACSHelloWorld**. **WebContent**'e sağ tıklayın, **Yeni**'ye tıklayın ve ardından **JSP Dosyası**'na tıklayın.
3. İçinde **yeni JSP dosyası** iletişim kutusunda, dosya adı **index.jsp**. Aşağıda gösterildiği gibi üst klasör MyACSHelloWorld/WebContent olarak tutun:
   
    ![ACS örneğin bir JSP dosyası ekleme][add_jsp_file_acs]
   
    **İleri**’ye tıklayın.
4. İçinde **JSP şablon seç** iletişim kutusunda **yeni JSP dosyası (html)** tıklatıp **son**.
5. İndex.jsp dosyası Eclipse'te açıldığında, görüntülenecek metni eklemek **Hello ACS World!** `<body>`Hello World! (Merhaba Dünya!) ifadesinin görüntülenmesi için metni ekleyin. Güncelleştirilmiş `<body>` içeriği aşağıdaki gibi görünmelidir:
   
        <body>
          <b><% out.println("Hello ACS World!"); %></b>
        </body>
   
    İndex.jsp kaydedin.

## <a name="add-the-acs-filter-library-to-your-application"></a>ACS filtre kitaplığı uygulamanıza ekleyin
1. Eclipse'nın Proje Gezgini'nde sağ **MyACSHelloWorld**, tıklatın **yapı yolu**ve ardından **oluşturma yolunu Yapılandır**.
2. İçinde **Java oluşturma yolu** iletişim kutusunda, tıklatın **kitaplıkları** sekmesi.
3. Tıklatın **Kitaplığı eklemek**.
4. Tıklatın **Azure erişim denetimi Hizmetleri filtresi (tarafından MS açık teknik)** ve ardından **sonraki**. **Azure erişim denetimi Hizmetleri filtre** iletişim kutusu gösterilir.  ( **Konumu** alan Eclipse, yüklediğiniz bağlı olarak farklı bir yol olabilir ve sürüm numarasını yazılım güncelleştirmelerini bağlı olarak farklı olabilir.)
   
    ![ACS filtre kitaplığı Ekle][add_acs_filter_lib]
5. Bir tarayıcı kullanılarak açılmasını için **oturum açma sayfası tümleştirme** yönetim portalının URL'sini listelenen kopyalama sayfasında **seçenek 1: bir ACS barındırılan oturum açma sayfasına bağlantı** alan ve yapıştırın **ACS kimlik doğrulama uç noktası** Eclipse iletişim alanı.
6. Bir tarayıcı kullanılarak açılmasını için **bağlı olan taraf uygulamayı Düzenle** yönetim portalının URL'sini listelenen kopyalama sayfasında **bölge** alan ve yapıştırın **bağlı olan taraf bölge** Eclipse iletişim alanı.
7. İçinde **güvenlik** varolan bir sertifikayı kullanmak istiyorsanız tıklatın Eclipse iletişim bölümünü **Gözat**, istediğiniz kullanın, onu seçin ve'sertifikaya gidin **açık**. Veya yeni bir sertifika oluşturmak istiyorsanız, tıklayın **yeni** görüntülemek için **yeni sertifika** iletişim kutusunda, parola, .cer dosyasının adını ve yeni sertifikayı .pfx dosyasının adını belirtin.
8. Denetleme **WAR dosyası sertifikanın katıştırmak**. Bu şekilde sertifikanın katıştırma el ile bir bileşeni olarak eklemek gerek kalmadan, dağıtımınızdaki içerir. (Bunun yerine, sertifikanızı dışarıdan WAR dosyanızdan depolamanız gerekir, rolü bileşen olarak sertifika Ekle ve seçeneğinin işaretini kaldırın **WAR dosyası sertifikanın katıştırmak**.)
9. [İsteğe bağlı] Tutmak **gerektiren HTTPS bağlantıları** işaretli. Bu seçeneği ayarlarsanız, HTTPS protokolünü kullanarak uygulamanızı erişim gerekir. HTTPS bağlantılarına ihtiyacınız istemiyorsanız bu seçeneği temizleyin.
10. Bir dağıtım için işlem öykünücüsü, **Azure ACS filtre** ayarları aşağıdakine benzer görünür.
    
    ![İşlem öykünücüsü için bir dağıtım için Azure ACS filtresi ayarları][add_acs_filter_lib_emulator]
11. **Son**'a tıklayın.
12. Tıklatın **Evet** bir iletişim kutusu web.xml dosyasını oluşturulacak belirten ile birlikte sunulan olduğunda.
13. Tıklatın **Tamam** kapatmak için **Java oluşturma yolu** iletişim.

## <a name="deploy-to-the-compute-emulator"></a>İşlem öykünücüsü dağıtma
1. Eclipse'nın Proje Gezgini'nde sağ **MyACSHelloWorld**, tıklatın **Azure**ve ardından **Azure için paket**.
2. İçin **proje adı**, türü **MyAzureACSProject** tıklatıp **sonraki**.
3. Bir JDK'yı seçin ve uygulama sunucusu. (Bu adımları içinde ayrıntılı olarak ele [bir Hello World uygulamasının Azure için Eclipse'te oluşturma](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx) öğretici).
4. **Son**'a tıklayın.
5. Tıklatın **Azure öykünücüsünde çalıştırın** düğmesi.
6. İşlem öykünücüsü, Java web uygulaması başladıktan sonra (geçerli bir tarayıcı oturumu ACS oturum açma testinizle karışmaması böylece) tarayıcınızın tüm örneklerini kapatın.
7. Açarak uygulamanızı çalıştırın <http://localhost: 8080/MyACSHelloWorld/> tarayıcınızda (veya <https://localhost:8080/MyACSHelloWorld/> , işaretlediyseniz **gerektiren HTTPS bağlantıları**). Bir Windows Live ID oturum açma için istemde ve ardından bağlı olan taraf uygulamanız için belirtilen dönüş URL'si için gerçekleştirilmelidir.
8. Uygulamanızı görüntülemeyi bitirdikten tıklatın **Azure öykünücüsünü sıfırlayın** düğmesi.

## <a name="deploy-to-azure"></a>Azure’a Dağıt
Azure'a dağıtmak için bağlı olan taraf bölge değiştirmek ve dönüş URL'si için ACS ad alanınızı gerekecektir.

1. Azure Yönetim Portalı içinde içinde **bağlı olan taraf uygulamayı Düzenle** sayfasında, Değiştir **bölge** dağıtılan sitenizin URL'si olmalıdır. Değiştir **örnek** dağıtımınız için belirtilen DNS adı.
   
    ![Üretimde kullanım için bağlı olan taraf bölgesi][relying_party_realm_production]
2. Değiştirme **dönüş URL'si** uygulamanızın URL'si olmalıdır. Değiştir **örnek** dağıtımınız için belirtilen DNS adı.
   
    ![Üretimde kullanım için bağlı olan taraf dönüş URL'si][relying_party_return_url_production]
3. Tıklatın **kaydetmek** , güncelleştirilmiş bölümünde, bağlı olan taraf bölge kaydetmek ve URL değişiklikleri dönmek için.
4. Tutmak **oturum açma sayfası tümleştirme** tarayıcınızda açık sayfasında, kısa süre içinde kopyalama gerekir.
5. Eclipse'nın Proje Gezgini'nde sağ **MyACSHelloWorld**, tıklatın **yapı yolu**ve ardından **oluşturma yolunu Yapılandır**.
6. Tıklatın **kitaplıkları** sekmesini tıklatın, **Azure erişim denetimi Hizmetleri filtre**ve ardından **Düzenle**.
7. Bir tarayıcı kullanılarak açılmasını için **oturum açma sayfası tümleştirme** yönetim portalının URL'sini listelenen kopyalama sayfasında **seçenek 1: bir ACS barındırılan oturum açma sayfasına bağlantı** alan ve yapıştırın **ACS kimlik doğrulama uç noktası** Eclipse iletişim alanı.
8. Bir tarayıcı kullanılarak açılmasını için **bağlı olan taraf uygulamayı Düzenle** yönetim portalının URL'sini listelenen kopyalama sayfasında **bölge** alan ve yapıştırın **bağlı olan taraf bölge** Eclipse iletişim alanı.
9. İçinde **güvenlik** varolan bir sertifikayı kullanmak istiyorsanız tıklatın Eclipse iletişim bölümünü **Gözat**, istediğiniz kullanın, onu seçin ve'sertifikaya gidin **açık**. Veya yeni bir sertifika oluşturmak istiyorsanız, tıklayın **yeni** görüntülemek için **yeni sertifika** iletişim kutusunda, parola, .cer dosyasının adını ve yeni sertifikayı .pfx dosyasının adını belirtin.
10. Tutmak **WAR dosyası sertifikanın katıştırmak** WAR dosyası sertifikanın eklemek istediğiniz olduğu varsayılarak, kontrol.
11. [İsteğe bağlı] Tutmak **gerektiren HTTPS bağlantıları** işaretli. Bu seçeneği ayarlarsanız, HTTPS protokolünü kullanarak uygulamanızı erişim gerekir. HTTPS bağlantılarına ihtiyacınız istemiyorsanız bu seçeneği temizleyin.
12. Azure dağıtımı için Azure ACS filtre ayarlarınızı aşağıdakine benzer görünecektir.
    
    ![Üretim dağıtımı için Azure ACS filtresi ayarları][add_acs_filter_lib_production]
13. Tıklatın **son** kapatmak için **düzenleme Kitaplığı** iletişim.
14. Tıklatın **Tamam** kapatmak için **MyACSHelloWorld özelliklerini** iletişim.
15. Eclipse'te, tıklatın **Azure bulut Yayımla** düğmesi. Komut istemlerini yapılmış gibi benzer yanıt **uygulamanızı azure'a dağıtmak için** bölümünü [bir Hello World uygulamasının Azure için Eclipse'te oluşturma](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx) konu. 

Web Uygulamanız dağıtıldıktan sonra tüm açık tarayıcı oturumları kapatmak, web uygulamanızı çalıştırın ve bağlı olan taraf uygulamanızı dönüş URL'sine gönderilen tarafından izlenen Windows Live ID kimlik bilgilerinizle oturum istenir.

İşiniz bittiğinde, ACS Hello World uygulamasını kullanarak unutmayın dağıtımını silmek (bir dağıtımda silmek nasıl öğrenebilirsiniz [bir Hello World uygulamasının Azure için Eclipse'te oluşturma](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx) konu).

## <a name="next_steps"></a>Sonraki adımlar
Bir inceleme, güvenlik onaylama işlemi biçimlendirme dili (uygulamanıza ACS tarafından döndürülen SAML) için bkz: [SAML görüntülemek nasıl Azure erişim denetimi hizmeti tarafından döndürülen][How to view SAML returned by the Azure Access Control Service]. Biraz daha ACS'ın işlevselliği inceleyin ve daha karmaşık senaryolarıyla denemeler yapmak için bkz: [erişim denetimi hizmeti 2.0][Access Control Service 2.0].

Ayrıca, bu örnekte kullanılan **WAR dosyası sertifikanın katıştırmak** seçeneği. Bu seçenek sertifika dağıtmayı kolaylaştırır. Bunun yerine imzalama sertifikanızın WAR dosyanızdan ayrı tutmak istiyorsanız, aşağıdaki yöntemi kullanabilirsiniz:

1. İçinde **güvenlik** bölümünü **Azure erişim denetimi Hizmetleri filtre** iletişim, türü **${env. JAVA_HOME}/mycert.cer** ve işaretini **WAR dosyası sertifikanın katıştırmak**. (Sertifika dosyası adı farklıysa Sertifikam.cer ayarlayın.) Tıklatın **son** iletişim kutusunu kapatmak için.
2. Dağıtımınızda bir bileşen olarak sertifikayı kopyalayın: Eclipse'nın Proje Gezgini'nde genişletin **MyAzureACSProject**, sağ tıklayın **WorkerRole1**, tıklatın **özellikleri**, genişletin **Azure rol**, tıklatıp **bileşenleri**.
3. **Ekle**'ye tıklayın.
4. İçinde **Bileşen Ekle** iletişim:
   
   1. İçinde **alma** bölümü:
      1. Kullanmak **dosya** düğmesi kullanmak istediğiniz sertifika gidin. 
      2. İçin **yöntemi**seçin **kopya**.
   2. İçin **olarak ad**, metin kutusuna tıklayın ve varsayılan adı kabul edin.
   3. İçinde **dağıtma** bölümü:
      1. İçin **yöntemi**seçin **kopya**.
      2. İçin **dizinine**, türü **JAVA_HOME %**.
   4. **Bileşen Ekle** iletişim aşağıdakine benzer görünmelidir.
      
       ![Sertifika Bileşen Ekle][add_cert_component]
   5. **Tamam** düğmesine tıklayın.

Bu noktada, sertifikanızı dağıtımınızda eklenmesi. WAR dosyası sertifikanın katıştırmak mı dağıtımınıza bir bileşeni olarak eklemek bağımsız olarak, sertifika açıklandığı gibi ad alanınızın karşıya gerektiğini unutmayın [ACS ad alanınızın bir sertifika karşıya] [ Upload a certificate to your ACS namespace] bölümü.

[What is ACS?]: #what-is
[Concepts]: #concepts
[Prerequisites]: #pre
[Create a Java web application]: #create-java-app
[Create an ACS Namespace]: #create-namespace
[Add Identity Providers]: #add-IP
[Add a Relying Party Application]: #add-RP
[Create Rules]: #create-rules
[Upload a certificate to your ACS namespace]: #upload-certificate
[Review the Application Integration Page]: #review-app-int
[Configure Trust between ACS and Your ASP.NET Web Application]: #config-trust
[Add the ACS Filter library to your application]: #add_acs_filter_library
[Deploy to the compute emulator]: #deploy_compute_emulator
[Deploy to Azure]: #deploy_azure
[Next steps]: #next_steps
[project website]: http://wastarterkit4java.codeplex.com/releases/view/61026
[How to view SAML returned by the Azure Access Control Service]: active-directory-java-view-saml-returned-by-access-control.md
[Access Control Service 2.0]: http://go.microsoft.com/fwlink/?LinkID=212360
[Windows Identity Foundation]: http://www.microsoft.com/download/en/details.aspx?id=17331
[Windows Identity Foundation SDK]: http://www.microsoft.com/download/en/details.aspx?id=4451
[Azure Management Portal]: https://manage.windowsazure.com
[acs_flow]: ./media/active-directory-java-authenticate-users-access-control-eclipse/ACSFlow.png

<!-- Eclipse-specific -->
[add_acs_filter_lib]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddACSFilterLibrary.png
[add_acs_filter_lib_emulator]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddACSFilterLibraryEmulator.png
[add_acs_filter_lib_production]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddACSFilterLibraryProduction.png

[relying_party_realm_emulator]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyRealmEmulator.png
[relying_party_return_url_emulator]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyReturnURLEmulator.png
[relying_party_realm_production]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyRealmProduction.png
[relying_party_return_url_production]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyReturnURLProduction.png
[add_cert_component]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddCertificateComponent.png
[add_jsp_file_acs]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddJSPFileACS.png
[create_acs_hello_world]: ./media/active-directory-java-authenticate-users-access-control-eclipse/CreateACSHelloWorld.png
[add_token_signing_cert]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddTokenSigningCertificate.png

