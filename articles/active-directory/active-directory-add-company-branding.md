---
title: "Azure Active Directory’de oturum açma ve Erişim Paneli sayfalarınıza şirket markası ekleme"
description: "Azure oturum açma sayfasına ve erişim paneli sayfasına nasıl şirket markası ekleyeceğiniz hakkında bilgi edinin"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: f74621b4-4ef0-4899-8c0e-0c20347a8c31
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/07/2017
ms.author: curtand
translationtype: Human Translation
ms.sourcegitcommit: 538f282b28e5f43f43bf6ef28af20a4d8daea369
ms.openlocfilehash: 6d4fbfe97288fcb76628b45649b8b678152198a9
ms.lasthandoff: 04/07/2017


---
# <a name="add-company-branding-to-sign-in-and-access-panel-pages"></a>Oturum açma ve Erişim Paneli sayfalarına şirket markası ekleme
Birçok şirket, yönettikleri hizmetlerde ve web sitelerinde tutarlı bir genel görünüm uygulamak ister. Azure Active Directory, BT uzmanlarının, aşağıdaki web sayfalarının görünümünü şirket logosuyla ve görüntülerle özelleştirmesine olanak tanıyarak bu özelliği sunar:

* **Oturum açma sayfası:** Çalışanlarınız ve şirket konuklarınız Office 365'te veya Azure AD'yi kullanan diğer uygulamalarda oturum açtığında görüntülenen sayfadır.
* **Erişim Paneli Sayfası:** Erişim Paneli, Azure AD yöneticinizin erişim izni verdiği bulut tabanlı uygulamaları görüntülemenize ve başlatmanıza olanak sağlayan web tabanlı bir portaldır. Erişim Paneli şu adreste bulunabilir: [https://myapps.microsoft.com](https://myapps.microsoft.com).

Bu konu başlığında, oturum açma sayfasını ve erişim paneli sayfasını nasıl özelleştireceğiniz açıklanmıştır.

> [!NOTE]
> * Şirket markası, yalnızca Azure Active Directory Basic veya Premium sürümüne yükseltme yaptıysanız veya bir Office 365 lisansınız varsa kullanılabilir. Daha fazla bilgi için bkz. Azure Active Directory sürümleri.
> 
> * Azure Active Directory Premium ve Basic sürümleri, Azure Active Directory'nin dünya çapındaki örneğini kullanan Çin'deki müşterilerin kullanımına sunulmuştur. Azure Active Directory Premium ve Basic sürümleri, şu anda Çin'de 21Vianet tarafından işletilen Microsoft Azure hizmeti kapsamında desteklenmemektedir. Daha fazla bilgi için Azure Active Directory Forumu'nda bizimle iletişime geçin.


## <a name="customizing-the-sign-in-page"></a>Oturum açma sayfasını özelleştirme
Kullanıcılarınız, kuruluşunuzun abone olduğu bulut uygulama ve hizmetlerine erişmeye çalışırken genellikle Azure AD oturum açma sayfasıyla etkileşimde bulunur.

Oturum açma sayfanızda marka değişiklikleri yaptıysanız bu değişikliklerin son kullanıcılara görünmesi bir saate kadar sürebilir.

Şirket markası öğeleri, kullanıcılar https://outlook.com/contoso.com gibi kiracıya özel bir URL’ye eriştiğinde Azure AD oturum açma sayfasında görünür.

Kullanıcılar www.office.com gibi genel bir URL’deki hizmeti ziyaret ettiğinde, sistem kullanıcının kim olduğunu bilmediği için oturum açma sayfasında şirket markası bilgileri gösterilmez. Ancak, kullanıcı kimliği girildikten veya bir kullanıcı kutucuğu seçildikten sonra şirket markası görünür.

> [!NOTE]
> * Etki alanı adınız, markayı yapılandırdığınız klasik Azure portalının **Active Directory** > **Directory (Dizin)** > **Domains (Etki Alanları)** bölümünde "Enabled" ("Etkin") olarak görünmelidir.
> * Oturum açma sayfasında bulunan marka, Microsoft'un kişisel hesaplara yönelik oturum açma sayfasına aktarılmaz. Çalışanlarınız veya şirket konuklarınız kişisel bir Microsoft hesabıyla oturum açtığında, oturum açma sayfaları kuruluşunuzun markasını yansıtmaz.
>

Aşağıdaki ekran görüntüleri, oturum açma sayfalarının nasıl özelleştirildiğini açıklamaktadır.

### <a name="scenario-1-contoso-employee-goes-to-a-generic-app-url-for-example-wwwofficecom"></a>Senaryo 1: Contoso çalışanı genel bir uygulama URL'sine gider (örneğin, www.office.com)

Bu örnekte, Contoso kullanıcısı bir mobil uygulamada veya genel bir URL kullanarak bir web uygulamasında oturum açar. Sol taraftaki çizim her zaman uygulamayı temsil eder ve sağ taraftaki etkileşim bölmesi, uygun olduğunda Contoso marka öğelerini gösterecek şekilde güncelleştirilir.

![Özelleştirmeden önce ve sonra Office 365 oturum açma sayfası][1]

### <a name="scenario-2-contoso-employee-goes-to-contoso-app-thats-restricted-to-internal-users"></a>Senaryo 2: Contoso çalışanı şirket içi kullanıcılarla sınırlı Contoso uygulamasına gider

Bu örnekte bir Contoso kullanıcısı, şirkete özel bir URL kullanarak bir iç uygulamada oturum açar. Sol taraftaki çizim şirket markasını (Contoso) temsil eder. Sağdaki etkileşim bölmesi ise Contoso için kilitlidir ve çalışanların oturum açmasına yardımcı olur.

![kısıtlı uygulama oturum açma sayfası][2]

### <a name="scenario-3-contoso-employee-goes-to-a-contoso-app-thats-open-to-external-users"></a>Senaryo 3: Contoso çalışanı şirket dışı kullanıcılara açık Contoso uygulamasına gider

Bu örnekte, kullanıcılar Contoso’dan bir iş kolu uygulamasında oturum açar, ancak kullanıcı bir Contoso çalışanı olabilir veya olmayabilir. Soldaki çizim, yukarıdaki senaryo \#2’de olduğu gibi kaynak sahibini (Contoso) temsil eder. Ancak bu kez, sağdaki etkileşim bölmesi dış kullanıcıların oturum açabileceğini göstermek üzere Contoso için kilitli değildir.

![açık erişimle oturum açma][3]

### <a name="scenario-4-fabrikam-business-guest-goes-to-contoso-app-thats-open-to-external-users"></a>Senaryo 4: Fabrikam şirket konuğu, dış kullanıcılara açık olan Contoso uygulamasına gider

Bu örnekte bir Contoso kullanıcısı, şirkete özel bir URL kullanarak bir iç uygulamada oturum açar. Sol taraftaki çizim şirket markasını (Contoso) temsil eder. Sağdaki etkileşim bölmesi ise Contoso için kilitlidir ve çalışanların oturum açmasına yardımcı olur.

![dış kullanıcı olarak oturum açma][4]


## <a name="what-elements-on-the-page-can-i-customize"></a>Sayfadaki hangi öğeleri özelleştirebilirim?

Oturum açma sayfasında şu öğeleri özelleştirebilirsiniz:

![][5]

| Sayfa öğesi | Sayfadaki konum |
|:--- | --- |
| Başlık Logosu | Sayfanın sağ üst tarafında gösterilir. Kullanıcının kuruluşu belirlendikten sonra (genellikle kullanıcı adı girildikten sonra) uygulama logosunu değiştirir. |
| Arka plan çizimi | Oturum açma sayfasının sol tarafında tam boyutlu görüntü olarak gösterilir. Kiracılı oturum açma senaryoları için uygulamanın çizimini değiştirir (kullanıcılar kendi kuruluşları veya şirket konuğu oldukları bir kuruluş tarafından yayımlanan bir uygulamaya eriştiğinde).<br>Düşük bant genişliğine sahip bağlantılarda arka plan çizimi bir arka plan rengiyle değiştirilir. Telefon gibi dar ekranlarda çizim gösterilmez.<br>Kullanıcılar tarayıcılarını yeniden boyutlandırdığınızda arka plan çizimi kırpılır. Çiziminizi tasarladığınızda kırpılmaması için lütfen temel görsel öğeleri sol üst köşede tutun. | 
| “Oturumumu açık bırak” onay kutusu | **Parola** kutusunun altında gösterilir. |
| Oturum açma sayfası metni | Sayfa alt bilgisinin üzerinde gösterilecek ortak metin. Kullanıcılarınıza yardım masanızın telefon numarası ya da yasal bildirim gibi yararlı bilgiler iletmek için kullanılabilir. |

> [!NOTE]
> Tüm öğeler isteğe bağlıdır. Örneğin, Arka Plan çizimi yerine Başlık Logosu belirtirseniz oturum açma sayfasında hedef siteye ilişkin çizim (bu örnekte Office 365'in California otoyol görüntüsü) ve logonuz gösterilir.
>

Oturum açma sayfanızdaki **Oturumumu açık bırak** onay kutusu, kullanıcının tarayıcıyı kapatıp yeniden açması durumunda oturumunun açık kalmasına olanak tanır ve oturum süresini etkilemez.

Onay kutusunun görüntülenip görüntülenmeyeceği **KMSI’yi Gizle** ayarına bağlıdır.

![KMSI’yi Gizle ayarı][6]

Onay kutusunu gizlemek için bu ayarı **Gizli** olarak yapılandırın.

> [!NOTE]
> SharePoint Online ve Office 2010’un bazı özellikleri kullanıcıların bu onay kutusunu işaretleyebilmesine bağlıdır. Bu ayarı gizli olarak yapılandırırsanız kullanıcılarınız oturum açmaya yönelik ek ve beklenmeyen istemler görebilir.
>
>

Ayrıca bu sayfadaki tüm öğeleri yerelleştirebilirsiniz. Özelleştirme öğelerine ilişkin "varsayılan" bir küme yapılandırdıktan sonra farklı yerel ayarlar için daha fazla sürüm yapılandırabilirsiniz. Ayrıca, çeşitli öğeleri karıştırabilir ve eşleştirebilirsiniz. Örneğin, şunları yapabilirsiniz:

* Tüm kültürler için kullanılabilecek "varsayılan" bir çizim oluşturup İngilizce ve Fransızca için belirli sürümler oluşturabilirsiniz. Tarayıcılarınızı bu iki dilden birine ayarladığınızda belirlenen görüntü görünür, varsayılan çizim ise diğer tüm dillerde görünür.
* Kuruluşunuz için farklı logolar (örneğin, Japonca veya İbranice sürümler) yapılandırabilirsiniz.

## <a name="access-panel-page-customization"></a>Erişim paneli sayfa özelleştirmesi
Erişim Paneli sayfası, yöneticiniz tarafından erişim hakkı verilen bulut uygulamalarına hızlı erişim için kullanabileceğiniz bir portal sayfasıdır. Uygulamalarınız, bu sayfada tıklanabilir uygulama kutucukları olarak görünür.

Aşağıdaki anlık görüntüde, özelleştirme işlemi uygulanan bir erişim paneli sayfası gösterilmektedir.

![][8]

## <a name="configure-your-directory-with-company-branding"></a>Dizininizi şirket markasıyla yapılandırma
Klasik Azure portalında her dizin için özelleştirilebilir öğelere ilişkin bir varsayılan küme yapılandırabilirsiniz. Yönetici, varsayılanlar kaydedildikten sonra farklı diller/yerel ayarlar için her öğenin yerelleştirilmiş sürümlerini ekleyebilir. Tüm özelleştirilebilir öğeler isteğe bağlıdır.

Örneğin, Büyük Çizim yerine varsayılan Başlık Logosunu yapılandırırsanız logonuz oturum açma sayfasının sağ üst köşesinde görüntülenir. Ancak sitenin varsayılan çizimi görüntülenir.

Şunun gibi bir yapılandırma olduğunu düşünün:

* İngilizce dilinde varsayılan bir Başlık Logosu ve Oturum Açma Sayfası Metni
* Almanca için dile özgü Oturum Açma Sayfası Metni 

Dil tercihiniz Almanca ise varsayılan Başlık Logosunu ve Almanca metni alırsınız.

Teknik olarak, Azure AD tarafından desteklenen her dil için farklı bir küme yapılandırabilirsiniz ancak bakım ve performans nedeniyle varyasyon sayısını düşük tutmanızı öneririz.
 
**Dizininize şirket markası eklemek için şu adımları uygulayın:**

1. Özelleştirmek istediğiniz dizinin yöneticisi olarak [Klasik Azure portalında](https://manage.windowsazure.com) oturum açın.
2. Özelleştirmek istediğiniz dizini seçin.
3. Üstteki araç çubuğunda **Configure (Yapılandır)** düğmesine tıklayın.
4. **Customize Branding (Markayı Özelleştir)**düğmesine tıklayın.
5. Özelleştirmek istediğiniz öğeleri değiştirin. Tüm alanlar isteğe bağlıdır.
6. **Kaydet** düğmesine tıklayın.

Oturum açma sayfası markasında yaptığınız yeni değişikliğin görünmesi bir saate kadar sürebilir.

**Dile özgü şirket markası eklemek için şu adımları uygulayın:**

1. Özelleştirmek istediğiniz dizinin yöneticisi olarak [Klasik Azure portalında](https://manage.windowsazure.com) oturum açın.
2. Özelleştirmek istediğiniz dizini seçin.
fs3. Üstteki araç çubuğunda **Configure (Yapılandır)** düğmesine tıklayın.
4. **Customize Branding (Markayı Özelleştir)**düğmesine tıklayın.
5. **Add branding for a specific language (Belirli bir dil için marka ekle)** düğmesine tıklayın.
6. Logoyu özelleştirmek istediğiniz dili seçip **Next (İleri)** düğmesine tıklayın.
7. Yalnızca dile özgü geçersiz kılma işlemlerini yapılandırmak istediğiniz öğeleri düzenleyin. Tüm alanlar isteğe bağlıdır. Bir alan boş bırakılırsa özel varsayılan değer (veya özel varsayılan yapılandırılmadıysa Microsoft varsayılanı) görüntülenir.
8. **Kaydet** düğmesine tıklayın.

**Dizininizden şirket markası kaldırmak için şu adımları uygulayın:**

1. Özelleştirmek istediğiniz dizinin yöneticisi olarak [Klasik Azure portalında](https://manage.windowsazure.com) oturum açın.
2. Özelleştirmek istediğiniz dizini seçin.
3. Üstteki araç çubuğunda **Configure (Yapılandır)** düğmesine tıklayın.
4. **Customize Branding (Markayı Özelleştir)**düğmesine tıklayın.
5. Marka Özelleştirme sayfasında **Edit Existing Branding Settings (Var Olan Marka Ayarlarını Düzenle)** seçeneğini belirleyip sonraki sayfaya gidin.
6. Kaldırmak istediğiniz öğelere bağlı olarak, şunlardan birini veya birkaçını yapın:

    a. **Banner Logo (Başlık Logosu)** altında **Remove uploaded logo (Karşıya yüklenen logoyu kaldır)** seçeneğini belirleyin.

    b. **Tile Logo (Kutucuk Logosu)** altında **Remove uploaded logo (Karşıya yüklenen logoyu kaldır)** seçeneğini belirleyin.

    c. Tüm metin kutularındaki metinleri kaldırın.

    d. **İleri**’ye tıklayın.

    e. Tüm metin kutularındaki metinleri kaldırın.
7. Öğeleri kaldırmak için **Save (Kaydet)** düğmesine tıklayın.
8. Gerekirse **Customize Branding (Markayı Özelleştir)** seçeneğine yeniden tıklayın ve kaldırılması gereken tüm dile özgü markalar için bu adımları tekrar edin.
    **Customize Branding (Markayı Özelleştir)** seçeneğine tıkladığınızda **Customize Default Branding (Varsayılan Markayı Özelleştir)** formunda yapılandırılmış bir ayar görmüyorsanız tüm marka ayarları kaldırılmıştır.


## <a name="customizable-elements"></a>Özelleştirilebilir öğeler
Şirket logoları, oturum açma ve Erişim Paneli sayfaları için kullanılırken, diğer öğeler yalnızca oturum açma sayfasında kullanılır. Aşağıdaki tabloda farklı özelleştirilebilir öğelerle ilgili ayrıntılar verilmiştir.

| Ad | Açıklama | Kısıtlamalar | Öneriler |
| --- | --- | --- | --- |
| Başlık logosu |Başlık logosu, oturum açma sayfasında ve Erişim panelinde görüntülenir. |<p>JPG veya PNG</p><p>60 x 280 piksel</p><p>10 KB</p> |<p>Kuruluşunuzun tam logosunu (piktogram ve logo türü dahil) kullanın</p><p>Mobil cihazlarda kaydırma çubuklarının görünmemesi için logoyu 30 pikselden düşük tutun.</p><p>4 KB altında tutun</p><p>Saydam bir PNG kullanın (oturum açma sayfasının her zaman beyaz bir arka plana sahip olacağını varsaymayın)</p> |
| Kutucuk logosu | Şu anda kullanılmıyor |<p>JPG veya PNG</p><p>120 x 120 piksel</p><p>10 KB</p> |<p>Bu görüntü % 50 olarak yeniden boyutlandırılabileceği için (küçük metin olmaksızın) basit olmasını sağlayın |
| </p> | | | |
| Oturum açma kullanıcı adı etiketi | Şu anda kullanılmıyor |<p>Maksimum 50 karakterlik Unicode metni</p><p>Yalnızca düz metin (bağlantılar veya HTML etiketleri olmadan)</p> |<p>Kısa ve basit tutun</p><p>Kullanıcılarınıza, sağlamış olduğunuz iş veya okul hesaplarını genel olarak nasıl adlandırdıklarını sorun.</p> |
| Oturum açma sayfası ortak metni |Bu ortak metin, oturum açma sayfası formunun altında görünür; ek yönergeler için veya yardım ya da desteğin nereden alınabileceğiyle ilgili bilgi vermek için kullanılabilir. |<p>Maksimum 256 karakterlik Unicode metni</p><p>Yalnızca düz metin (bağlantılar veya HTML etiketleri olmadan)</p> |250 karakterin altında tutun (yaklaşık 3 metin satırı)  |
| Oturum açma sayfası arka plan çizimi | Kullanıcılar kiracıya özel URL’lere eriştiğinde oturum açma sayfasının solunda (RtL dilleri için sağda) gösterilen büyük resim. |<p>JPG veya PNG</p><p>1420 x 1200</p><p>500 KB</p> |<p>1420 x 1200 piksel</p><p>Önemli: Olabildiğince küçük tutun; 200 KB'ın altında olması idealdir. Bu görüntü, çok büyük olduğu için önbelleğe alınmadığında Oturum Açma sayfasının performansı etkilenir</p><p>Bu resim, farklı ekran en boy oranlarına uyması için hemen her zaman kırpılır. Birincil görsel öğeleri sol üst köşede tutun.</p> |
| Oturum açma sayfası arka plan rengi | Düşük bant genişliğine sahip bağlantılarda arka plan çiziminin yerine bu düz renk kullanılır. | Onaltılık biçimde bir RGB rengi olmalıdır (örnek: \#FFFFFF) | Başlık logosunun birincil rengini seçmeniz önerilir. |

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Active Directory Premium ile çalışmaya başlama](active-directory-get-started-premium.md)
* [Erişim ve kullanım raporlarınızı görüntüleme](active-directory-view-access-usage-reports.md)

<!--Image references-->
[1]: ./media/active-directory-add-company-branding/signin-page_before-customization.png
[2]: ./media/active-directory-add-company-branding/signin-page-restricted-app.png
[3]: ./media/active-directory-add-company-branding/signin-page-open-access.png
[4]: ./media/active-directory-add-company-branding/signin-page-external-guest.png
[5]: ./media/active-directory-add-company-branding/which-elements-can-i-customize.png
[6]: ./media/active-directory-add-company-branding/hide-kmsi.png
[8]: ./media/active-directory-add-company-branding/APBranding.png
[9]: ./media/active-directory-add-company-branding/hidekmsi.png

