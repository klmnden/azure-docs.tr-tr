---
title: "Oturum açma ve Erişim Paneli sayfalarınıza şirket markası ekleme"
description: "Azure oturum açma sayfasına ve erişim paneli sayfasına nasıl şirket markası ekleyeceğiniz hakkında bilgi edinin"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: f74621b4-4ef0-4899-8c0e-0c20347a8c31
ms.service: active-directory
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/30/2016
ms.author: curtand
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 98c8352152b6cd1817d32c6418597c566d94d44f


---
# <a name="add-company-branding-to-your-sign-in-and-access-panel-pages"></a>Oturum açma ve Erişim Paneli sayfalarınıza şirket markası ekleme
Birçok şirket, karışıklığı önlemek için yönettikleri hizmetlerde ve web sitelerinde tutarlı bir genel görünüm uygulamak ister. Azure Active Directory, aşağıdaki web sayfalarının görünümünü şirket logonuzla ve özel renk düzenleriyle özelleştirmenize olanak tanıyarak size bu özelliği sunar:

* **Oturum açma sayfası:** Office 365'te veya Azure AD'yi kimlik sağlayıcınız olarak kullanan diğer web tabanlı uygulamalarda oturum açtığınızda görüntülenen sayfadır. Ana Bölge Bulma işlemi sırasında veya kimlik bilgilerinizi girerken bu sayfa ile karşılaşırsınız. Ana Bölge Bulma işlemi, sistemin federasyon kullanıcılarını kendi şirket içi STS'lerine (örneğin, AD FS) yönlendirmesini sağlar.
* **Erişim Paneli Sayfası:** Erişim Paneli, Azure AD yöneticinizin erişim izni verdiği bulut tabanlı uygulamaları görüntülemenize ve başlatmanıza olanak sağlayan web tabanlı bir portaldır. Erişim Paneline erişmek için şu URL'yi kullanın: [https://myapps.microsoft.com](https://myapps.microsoft.com).

Bu konu başlığında, oturum açma sayfasını ve erişim paneli sayfasını nasıl özelleştireceğiniz açıklanmıştır.

> [!NOTE]
> * Şirket markası özelliğini, yalnızca Azure Active Directory Basic veya Premium sürümüne yükseltme yaptıysanız veya bir Office 365 kullanıcısıysanız kullanabilirsiniz. Daha fazla bilgi için bkz. [Azure Active Directory sürümleri](active-directory-editions.md).
> * Azure Active Directory Premium ve Basic sürümleri, Azure Active Directory'nin dünya çapındaki örneğini kullanan Çin'deki müşterilerin kullanımına sunulmuştur. Azure Active Directory Premium ve Basic sürümleri, şu anda Çin'de 21Vianet tarafından işletilen Microsoft Azure hizmeti kapsamında desteklenmemektedir. Daha fazla bilgi için [Azure Active Directory Forumu](https://feedback.azure.com/forums/169401-azure-active-directory/)'nda bizimle iletişime geçin.
> 
> 

## <a name="customizing-the-sign-in-page"></a>Oturum açma sayfasını özelleştirme
Genel olarak, bulut uygulamalarınıza ve kuruluşunuzun abonesi olduğu hizmetlere tarayıcı tabanlı olarak erişmeniz gerekiyorsa oturum açma sayfasını kullanırsınız.

Oturum açma sayfanızda değişiklikler yaptıysanız bu değişikliklerin görünmesi bir saate kadar sürebilir.

Bir hizmeti, https://outlook.com/**contoso**.com veya https://mail.**contoso**.com gibi yalnızca kiracıya özgü bir URL ile ziyaret ettiğinizde markalı oturum açma sayfası görüntülenir.

Hizmeti https://mail.office365.com gibi kiracıya özgü olmayan bir URL ile ziyaret ederseniz markasız oturum açma sayfası görüntülenir. Bu durumda markanız, kullanıcı kimliğinizi girdiğinizde veya kullanıcı kutucuğunu seçtiğinizde görüntülenir.

> [!NOTE]
> * Etki alanı adınız, markayı yapılandırdığınız klasik Azure portalının **Active Directory** > **Directory (Dizin)** > **Domains (Etki Alanları)** bölümünde "Enabled" ("Etkin") olarak görünmelidir.
> * Oturum açma sayfasında bulunan marka, Microsoft'un tüketici oturum açma sayfasına aktarılmaz. Kişisel bir Microsoft hesabıyla oturum açarsanız Azure AD tarafından işlenen kullanıcı kutucuklarının markalı bir listesini görebilirsiniz ancak kuruluşunuzun markası, Microsoft hesabı oturum açma sayfasına uygulanmaz.
> 
> 

Bu sayfada şirketinizin markasını, renklerini ve diğer özelleştirilebilir öğelerini göstermek istiyorsanız bu iki deneyim arasındaki farkı anlamak için aşağıdaki görüntülere bakın.

Aşağıdaki anlık görüntüde, masaüstü bilgisayarda Office 365 oturum açma sayfasının özelleştirmeden **önceki** hali gösterilmektedir:

![Özelleştirmeden önce Office 365 oturum açma sayfası][1]

Aşağıdaki anlık görüntüde, masaüstü bilgisayarda Office 365 oturum açma sayfasının özelleştirmeden **sonraki** hali gösterilmektedir:

![Özelleştirmeden sonra Office 365 oturum açma sayfası][2]

Aşağıdaki anlık görüntüde, mobil cihazda Office 365 oturum açma sayfasının özelleştirmeden **önceki** hali gösterilmektedir:

![Özelleştirmeden önce Office 365 oturum açma sayfası][3]

Aşağıdaki anlık görüntüde, mobil cihazda Office 365 oturum açma sayfasının özelleştirmeden **sonraki** hali gösterilmektedir:

![Özelleştirmeden sonra Office 365 oturum açma sayfası][4]

Daha önce de gösterildiği gibi, bir tarayıcı penceresini yeniden boyutlandırdığınızda büyük Çizim çoğunlukla farklı ekran en boy oranlarına uygun şekilde kırpılır. Bunu göz önünde bulundurarak, çizimdeki temel görsel öğeleri her zaman sol üst köşede görünecek (sağdan sola yazılan diller için sağ üst) şekilde ayarlamaya çalışmanız gerekir. Yeniden boyutlandırma genellikle sağ alt köşeden yukarı/sola veya aşağıdan yukarı doğru yapıldığı için bu nokta önemlidir.

Aşağıdaki resimde, tarayıcı sol tarafa doğru yeniden boyutlandırıldığında çizimin nasıl kırpıldığı gösterilmiştir:

![][6]

Tarayıcı yukarı doğru yeniden boyutlandırıldıktan sonra şu şekilde görünür:

![][7]

## <a name="what-elements-on-the-page-can-i-customize"></a>Sayfadaki hangi öğeleri özelleştirebilirim?
Oturum açma sayfasında şu öğeleri özelleştirebilirsiniz:

![][5]

| Sayfa öğesi | Sayfadaki konum |
|:--- | --- |
| Başlık Logosu |Sayfanın sağ üst tarafında gösterilir. Oturum açtığınız hedef sitede (örneğin, Office 365 veya Azure) görüntülenen logonun yerini alır. |
| Büyük Çizim/Arka Plan Rengi |Sayfanın solunda gösterilir. Oturum açtığınız hedef sitede görüntülenen görüntünün yerini alır. Düşük bant genişliğine sahip bağlantılarda veya dar ekranlarda Büyük Çizimlerin yerine Arka Plan Rengi gösterilebilir. |
| Oturumum açık kalsın |Parola metin kutusunun altında gösterilir. |
| Oturum Açma Sayfası Metni |İş veya okul hesabıyla oturum açmadan önce yardımcı olacak bilgiler sağlamanız gerektiğinde sayfa alt bilgisinde gösterilir. Örneğin, yardım masanıza bir telefon numarası veya yasal bildirim eklemek isteyebilirsiniz. |

> [!NOTE]
> Tüm öğeler isteğe bağlıdır. Örneğin, Büyük Çizim yerine Başlık Logosu belirtirseniz oturum açma sayfasında hedef siteye ilişkin çizim (bu örnekte Office 365'in California otoyol görüntüsü) ve logonuz gösterilir.
> 
> 

Oturum açma sayfanızdaki **Oturumum açık kalsın** onay kutusu, kullanıcının tarayıcıyı kapatıp yeniden açması durumunda oturumunun açık kalmasına olanak tanır. Bu durum oturum ömrünü etkilemez. Azure Active Directory oturum açma sayfasındaki onay kutusunu gizleyebilirsiniz.

Onay kutusunun görüntülenip görüntülenmeyeceği **KMSI Gizle** ayarına bağlıdır.

![][9]

Onay kutusunu gizlemek için bu ayarı **Gizli** olarak yapılandırın. 

> [!NOTE]
> SharePoint Online ve Office 2010’un bazı özellikleri kullanıcıların bu kutuyu işaretleyebilmesine bağlıdır. Bu ayarı gizli olarak yapılandırırsanız kullanıcılarınız oturum açmaya yönelik ek ve beklenmeyen istemler görebilir.
> 
> 

Ayrıca bu sayfadaki tüm öğeleri yerelleştirebilirsiniz. Özelleştirme öğelerine ilişkin "varsayılan" bir küme yapılandırdıktan sonra farklı yerel ayarlar için daha fazla sürüm yapılandırabilirsiniz. Ayrıca, çeşitli öğeleri karıştırabilir ve eşleştirebilirsiniz. Örneğin, şunları yapabilirsiniz:

* Tüm kültürler için kullanılabilecek "varsayılan" bir Büyük Çizim oluşturup İngilizce ve Fransızca için belirli sürümler oluşturabilirsiniz. Tarayıcılarınızı bu iki dilden birine ayarladığınızda belirlenen görüntü görünür, varsayılan çizim ise diğer tüm dillerde görünür.
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
3. Üstteki araç çubuğunda **Configure (Yapılandır)** düğmesine tıklayın.
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

## <a name="testing-and-examples"></a>Test ve örnekler
Üretim ortamınızda değişiklik yapmadan önce bir kiracı ile deneme yapmanızı öneririz.

**Markanızın uygulanıp uygulanmadığını doğrulamak için:**

1. InPrivate veya Incognito tarayıcı oturumu açın.
2. contoso.com adresini özelleştirdiğiniz etki alanıyla değiştirerek https://outlook.com/contoso.com adresini ziyaret edin.

Bu, contoso.onmicrosoft.com gibi görünen etki alanları için de geçerlidir.

Etkili özelleştirme kümeleri oluşturmanıza yardımcı olmak üzere şu iki kurgusal oturum açma sayfasını özelleştirdik:

* [http://aka.ms/aaddemo001](http://aka.ms/aaddemo001)
* [http://aka.ms/aaddemo002](http://aka.ms/aaddemo002)

Dile özgü ayarları test etmek için web tarayıcınızda varsayılan dil tercihlerini değiştirerek özelleştirme sırasında belirlediğiniz dili ayarlamanız gerekir. Internet Explorer'da bu yapılandırmayı **İnternet Seçenekleri** menüsünden gerçekleştirebilirsiniz.

## <a name="customizable-elements"></a>Özelleştirilebilir öğeler
Azure AD'deki bazı özelleştirilebilir öğelerin birden çok kullanım durumu vardır. Şirket logolarını her dizin için bir kere yapılandırabilirsiniz ve bu logolar hem oturum açma sayfasında hem de Erişim Paneli sayfasında kullanılabilir. Bazı özelleştirilebilir öğeler yalnızca oturum açma sayfasına özgüdür. Aşağıdaki tabloda farklı özelleştirilebilir öğelerle ilgili ayrıntılar verilmiştir.

| Ad | Açıklama | Kısıtlamalar | Öneriler |
| --- | --- | --- | --- |
| Başlık Logosu |Başlık Logosu, oturum açma sayfasında ve Erişim panelinde görüntülenir. |<p>JPG veya PNG</p><p>60 x 280 piksel</p><p>10 KB</p> |<p>Kuruluşunuzun tam logosunu (piktogram ve logo türü dahil) kullanın</p><p>Mobil cihazlarda kaydırma çubuklarının görünmemesi için logoyu 30 pikselden düşük tutun.</p><p>4 KB altında tutun</p><p>Saydam bir PNG kullanın (oturum açma sayfasının her zaman beyaz bir arka plana sahip olacağını varsaymayın)</p> |
| Kutucuk Logosu |(şu anda oturum açma sayfasında kullanılmıyor) Gelecekte bu metin, farklı deneyim alanlarındaki genel "iş veya okul hesabı" piktogramını değiştirmek için kullanılabilir. |<p>JPG veya PNG</p><p>120 x 120 piksel</p><p>10 KB</p> |<p>Bu görüntü % 50 olarak yeniden boyutlandırılabileceği için (küçük metin olmaksızın) basit olmasını sağlayın |
| </p> | | | |
| Oturum Açma Sayfası Kullanıcı Adı Etiketi |(şu anda oturum açma sayfasında kullanılmıyor) Gelecekte bu metin, farklı deneyim alanlarındaki genel "iş veya okul hesabı" dizesini değiştirmek için kullanılabilir. Bunu "Contoso hesabı" veya "Contoso kimliği" olarak ayarlayabilirsiniz. |<p>Maksimum 50 karakterlik Unicode metni</p><p>Yalnızca düz metin (bağlantılar veya HTML etiketleri olmadan)</p> |<p>Kısa ve basit tutun</p><p>Kullanıcılarınıza, sağlamış olduğunuz iş veya okul hesaplarını genel olarak nasıl adlandırdıklarını sorun.</p> |
| Oturum Açma Sayfası Metni |Bu "ortak" metin, oturum açma sayfası formunun altında görünür; ek yönergeler için veya yardım ya da desteğin nereden alınabileceğiyle ilgili bilgi vermek için kullanılabilir. |<p>Maksimum 256 karakterlik Unicode metni</p><p>Yalnızca düz metin (bağlantılar veya HTML etiketleri olmadan)</p> |250 karakterin altında tutun (yaklaşık 3 metin satırı)  |
| Oturum Açma Sayfası Çizimi |Çizim, oturum açma sayfasında oturum açma sayfası formunun solunda görüntülenen büyük görüntüdür. |<p>JPG veya PNG</p><p>1420 x 1200</p><p>500 KB</p> |<p>1420 x 1200 piksel</p><p>Önemli: Olabildiğince küçük tutun; 200 KB'ın altında olması idealdir. Bu görüntü, çok büyük olduğu için önbelleğe alınmadığında Oturum Açma sayfasının performansı etkilenir</p><p>Bu görüntü genellikle, farklı ekran oranları uyum sağlayacak şekilde kırpılır. Tarayıcı küçüldükçe yeniden boyutlandırma işlemi yukarı doğru sağ/alt köşeden sol/üst köşeye doğru gerçekleştiği için, birincil görsel öğeleri sol üst köşede (RTL dilleri için sağ üstte) tutun.</p> |
| Oturum Açma Sayfası Arka Plan Rengi |Oturum açma sayfası arka plan rengi, oturum açma sayfası formunun solundaki alanda kullanılır. |Arka plan rengi, onaltılık biçimde bir RGB rengi olmalıdır (örnek: #FFFFFF) |<p>Düşük bant genişliğine sahip bağlantılarda büyük Çizim yerine arka plan rengi gösterilebilir.</p><p>Başlık logosunun birincil rengini seçmenizi öneririz</p> |

## <a name="next-steps"></a>Sonraki Adımlar
* [Azure Active Directory Premium ile çalışmaya başlama](active-directory-get-started-premium.md)
* [Erişim ve kullanım raporlarınızı görüntüleme](active-directory-view-access-usage-reports.md)

<!--Image references-->
[1]: ./media/active-directory-add-company-branding/SignInPage_beforecustomization.png
[2]: ./media/active-directory-add-company-branding/SignInPage_aftercustomization.png
[3]: ./media/active-directory-add-company-branding/SignInPage_mobile_beforecustomization.png
[4]: ./media/active-directory-add-company-branding/SignInPage_mobile_aftercustomization.png
[5]: ./media/active-directory-add-company-branding/SignInPage_aftercustomization_elements.png
[6]: ./media/active-directory-add-company-branding/SignInPage_aftercustomization_croppedleft.png
[7]: ./media/active-directory-add-company-branding/SignInPage_aftercustomization_croppedtop.png
[8]: ./media/active-directory-add-company-branding/APBranding.png
[9]: ./media/active-directory-add-company-branding/hidekmsi.png



<!--HONumber=Dec16_HO1-->


