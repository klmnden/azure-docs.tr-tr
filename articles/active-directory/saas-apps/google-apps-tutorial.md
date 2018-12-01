---
title: 'Öğretici: Azure Active Directory Tümleştirme ile G Suite | Microsoft Docs'
description: G Suite ile Azure Active Directory arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 38a6ca75-7fd0-4cdc-9b9f-fae080c5a016
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/30/2018
ms.author: jeedes
ms.openlocfilehash: b8f6e69169cd146ec9dd20d8dad43b74ddb59228
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2018
ms.locfileid: "52726891"
---
# <a name="tutorial-azure-active-directory-integration-with-g-suite"></a>Öğretici: Azure Active Directory G Suite ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile G Suite tümleştirme konusunda bilgi edinin.

G Suite Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- G Suite erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) için G Suite açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Önkoşullar

G Suite ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik bir G Suite çoklu oturum açma etkin
- Google Apps aboneliği veya Google Cloud Platform abonelik.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz. Bu belge, yeni kullanıcı çoklu oturum açma deneyimini kullanarak oluşturuldu. Kurulum, hala eski kullanıyorsanız, farklı görünecektir. G Suite uygulamanın çoklu oturum açma ayarlarını yeni deneyim etkinleştirebilirsiniz. Git **Azure AD, kurumsal uygulamalar**seçin **G Suite**seçin **çoklu oturum açma** ve ardından **yeni deneyimimizi deneyin**.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="frequently-asked-questions"></a>Sık Sorulan Sorular

1. **S: Azure AD ile tümleştirme Google Cloud Platform SSO Bu tümleştirme desteği mu?**

    C: Evet. Google Cloud Platform ve Google Apps, aynı kimlik doğrulaması platformunun paylaşın. Bu nedenle GCP tümleştirme yapmak için Google Apps ile SSO yapılandırmanız gerekir.

2. **S: Azure AD çoklu oturum açma ile uyumlu Chromebooks ve diğer Chrome cihazları mı?**
  
    C: Evet, kullanıcılar, Azure AD kimlik bilgilerini kullanarak Chromebook cihazlarını oturum açabilir. Bkz. Bu [G Suite Destek makalesi](https://support.google.com/chrome/a/answer/6060880) neden hakkında bilgi için kimlik bilgilerini iki kez kullanıcılardan.

3. **S: çoklu oturum açma etkinleştirebilirim, kullanıcıların Google Classroom, GMail, Google Drive, YouTube ve benzeri gibi tüm Google ürün oturum açmak için Azure AD kimlik bilgilerini kullanmanız mümkün olacak mı?**

    C: Evet, bağlı olarak [hangi G Suite](https://support.google.com/a/answer/182442?hl=en&ref_topic=1227583) etkinleştirmek veya kuruluşunuz için devre dışı bırakmak seçin.

4. **Yalnızca bir alt kümesini G Suite Kullanıcılarım için çoklu oturum açmayı etkinleştirme miyim?**

    C: Hayır, çoklu oturum açmayı üzerinde hemen kapatma kendi Azure AD kimlik bilgileriyle kimlik doğrulaması tüm G Suite kullanıcılarınız gerektirir. G Suite sahip birden çok kimlik sağlayıcı desteklemediğinden, G Suite ortamınız için kimlik sağlayıcısı ya da Azure AD olabilir veya Google--ikisi aynı anda.

5. **S: bir kullanıcının Windows oturum açtığı, otomatik olarak G Suite için bir parola girmeniz istenir alma olmadan kimlik doğrulaması misiniz?**

    Y: Bu senaryoyu etkinleştirmek için iki seçenek vardır. İlk olarak, kullanıcılar Windows 10 cihazları oturum [Azure Active Directory Join](../device-management-introduction.md). Alternatif olarak, kullanıcıların etki alanına katılmış bir şirket içi Active Directory'ye Azure ad çoklu oturum açma için etkinleştirilmiş olan Windows cihazları oturum bir [Active Directory Federasyon Hizmetleri (AD FS)](../hybrid/plan-connect-user-signin.md) dağıtım. Azure AD arasında çoklu oturum açmayı etkinleştirmek için aşağıdaki öğreticiye adımları gerçekleştirmek iki seçenek de gerektirir ve G Suite.

6. **Hata: Geçersiz e-posta**

    Bu kurulumu için e-posta özniteliği, kullanıcıların oturum açabilmesi için gereklidir. Bu öznitelik el ile ayarlanamaz.

    E-posta özniteliği geçerli bir Exchange lisansına sahip herhangi bir kullanıcı için otomatik doldurulur. Kullanıcı e-posta etkin değilse, erişim vermek için bu öznitelik almak uygulama gereksinimleriniz değiştikçe bu hata alınır.

    Bir yönetici hesabıyla portal.office.com tıklayarak Yönetim merkezinde, abonelikler, faturalama, Office 365 aboneliğinizi seçin ve ardından Ata seçeneğine tıklayın, kullanıcılara seçin aboneliğini denetleyin ve sağ bölmede, tıklayarak istediğiniz kullanıcıların gidebilirsiniz lisansları düzenleyin.

    O365 lisansı atandıktan sonra uygulanması bazı dakika sürebilir. Bundan sonra user.mail öznitelik girdiğinizde olur ve sorunun çözülmesi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. G Suite galeri ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-g-suite-from-the-gallery"></a>G Suite galeri ekleme

Azure AD'de G Suite tümleştirmesini yapılandırmak için G Suite Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**G Suite Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **G Suite**seçin **G Suite** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde G Suite](./media/google-apps-tutorial/tutorial_gsuite_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve G Suite "Britta Simon" adlı bir test kullanıcı tabanlı Azure AD çoklu oturum açmayı sınayın.

Tek iş için oturum açma için Azure AD ne G Suite karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve G Suite ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma G Suite ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir G Suite test kullanıcısı oluşturma](#creating-a-g-suite-test-user)**  - kullanıcı Azure AD gösterimini bağlı G Suite Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve G Suite uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma G Suite ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **G Suite** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunu tıklatın **seçin** için **SAML** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açmayı yapılandırın](common/tutorial_general_301.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Çoklu oturum açmayı yapılandırın](common/editconfigure.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![G Suite etki alanı ve URL'ler tek oturum açma bilgileri](./media/google-apps-tutorial/tutorial_gsuite_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://www.google.com/a/<yourdomain.com>/ServiceLogin?continue=https://mail.google.com`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak:
    | |
    |--|
    | `google.com/a/<yourdomain.com>` |
    | `google.com` |
    | `http://google.com` |
    | `http://google.com/a/<yourdomain.com>` |

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [G Suite istemci Destek ekibine](https://www.google.com/contact/) bu değerleri almak için.

5. G Suite uygulama belirli bir biçimde SAML onaylamalarını bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** uygulama tümleştirme sayfasında bölümü. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için düğmeyi **kullanıcı öznitelikleri** iletişim.

    ![image](./media/google-apps-tutorial/i3-attribute.png)

6. İçinde **kullanıcı taleplerini** bölümünde **kullanıcı öznitelikleri** iletişim kutusunda, SAML belirteci özniteliği yukarıdaki görüntüde gösterilen şekilde yapılandırın ve aşağıdaki adımları gerçekleştirin:

    a. Tıklayın **Düzenle** açmak için düğmeyi **yönetmek, kullanıcı talepleri** iletişim.

    ![image](./media/google-apps-tutorial/i2-attribute.png)

    ![image](./media/google-apps-tutorial/i4-attribute.png)

    b. Gelen **kaynak özniteliği** selelct öznitelik değeri listesi.

    c. **Kaydet**’e tıklayın.

5. Üzerinde **SAML imzalama sertifikası** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/google-apps-tutorial/tutorial_gsuite_certificate.png) 

6. Üzerinde **G Suite kümesi** bölümünde, ihtiyacınıza göre uygun URL'yi kopyalayın.

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

    ![G Suite yapılandırması](common/configuresection.png)

9. Tarayıcınızda yeni bir sekme açın ve oturum [G Suite Yönetici Konsolu](http://admin.google.com/) yönetici hesabınızı kullanarak.

10. Tıklayın **güvenlik**. Bağlantıyı görmüyorsanız, bunun altında gizlenebilir **diğer denetimler** ekranın alt kısmındaki menü.

    ![Güvenlik'e tıklayın.][10]

11. Üzerinde **güvenlik** sayfasında **çoklu oturum açmayı (SSO) ayarlayın.**

    ![SSO tıklayın.][11]

12. Şu yapılandırma değişiklikleri gerçekleştirin:

    ![SSO yapılandırma][12]

    a. Seçin **üçüncü taraf kimlik sağlayıcısı ile Kurulum SSO**.

    b. İçinde **oturum açma sayfası URL'si** alan G Suite'te, değerini yapıştırın **oturum açma URL'si** , Azure Portalı'ndan kopyaladığınız.

    c. İçinde **sayfa oturum kapatma URL'si** alan G Suite'te, değerini yapıştırın **oturum kapatma URL'si** , Azure Portalı'ndan kopyaladığınız.

    d. İçinde **değiştirme parola URL'si** alan G Suite'te, değerini yapıştırın **değiştirme parola URL'si** , Azure Portalı'ndan kopyaladığınız.

    e. G Suite'teki için **doğrulama sertifikası**, Azure portalından indirdiğiniz sertifikasını karşıya yükleyin.

    f. Seçin **belirli bir etki alanı gönderici kullanın**.

    g. Tıklayın **değişiklikleri kaydetmek**.

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    ![Azure AD kullanıcısı oluşturun][100]

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Bir Azure AD test kullanıcısı oluşturma](common/create_aaduser_01.png) 

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Bir Azure AD test kullanıcısı oluşturma](common/create_aaduser_02.png)

    a. İçinde **adı** alanına **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alanına **brittasimon@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **özellikleri**seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’u seçin.

### <a name="creating-a-g-suite-test-user"></a>Bir G Suite test kullanıcısı oluşturma

Bu bölümün amacı, G Suite yazılımı Britta Simon adlı bir kullanıcı oluşturmaktır. G Suite otomatik sağlamayı, varsayılan olarak etkin olan destekler. Bu bölümde hiçbir şey yoktur. Bir kullanıcı, G Suite yazılımı zaten yoksa, G Suite yazılımı erişmeye çalıştığında yeni bir tane oluşturulur.

> [!NOTE]
> Sağlama Azure AD'de çoklu oturum açmayı test etmeden önce açık durumda değil, kullanıcı G Suite içinde zaten mevcut olduğundan emin olun.

> [!NOTE]
> Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [Google Destek ekibine](https://www.google.com/contact/).

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, G Suite erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

2. Uygulamalar listesinde **G Suite**.

    ![Çoklu oturum açmayı yapılandırın](./media/google-apps-tutorial/tutorial_gsuite_app.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. İçinde **atama Ekle** iletişim kutusunda **atama** düğmesi.

### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde G Suite kutucuğa tıkladığınızda, otomatik olarak G Suite uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: common/tutorial_general_01.png
[2]: common/tutorial_general_02.png
[3]: common/tutorial_general_03.png
[4]: common/tutorial_general_04.png

[100]: common/tutorial_general_100.png

[201]: common/tutorial_general_201.png
[202]: common/tutorial_general_202.png
[203]: common/tutorial_general_203.png
<!--Image references-->

[10]: ./media/google-apps-tutorial/gapps-security.png
[11]: ./media/google-apps-tutorial/security-gapps.png
[12]: ./media/google-apps-tutorial/gapps-sso-config.png