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
ms.date: 11/06/2018
ms.author: jeedes
ms.openlocfilehash: 4ed571d34e5df67f556f39b898e7ae5efc06a3e1
ms.sourcegitcommit: 02ce0fc22a71796f08a9aa20c76e2fa40eb2f10a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2018
ms.locfileid: "51288943"
---
# <a name="tutorial-azure-active-directory-integration-with-g-suite"></a>Öğretici: Azure Active Directory G Suite ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile G Suite tümleştirme konusunda bilgi edinin.

G Suite Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- G Suite erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) için G Suite açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

G Suite ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik bir G Suite çoklu oturum açma etkin
- Google Apps aboneliği veya Google Cloud Platform abonelik.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

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

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin.
Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. G Suite galeri ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-g-suite-from-the-gallery"></a>G Suite galeri ekleme

Azure AD'de G Suite tümleştirmesini yapılandırmak için G Suite Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**G Suite Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![image](./media/google-apps-tutorial/selectazuread.png)

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![image](./media/google-apps-tutorial/a_select_app.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![image](./media/google-apps-tutorial/a_new_app.png)

4. Arama kutusuna **G Suite**seçin **G Suite** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![image](./media/google-apps-tutorial/a_add_app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve G Suite "Britta Simon" adlı bir test kullanıcı tabanlı Azure AD çoklu oturum açmayı sınayın.

Tek iş için oturum açma için Azure AD ne G Suite karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve G Suite ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma G Suite ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[G Suite test kullanıcısı oluşturma](#create-a-g-suite-test-user)**  - kullanıcı Azure AD gösterimini bağlı G Suite Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve G Suite uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma G Suite ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde [Azure portalında](https://portal.azure.com/), **G Suite** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![image](./media/google-apps-tutorial/b1_b2_select_sso.png)

2. Tıklayın **değişiklik çoklu oturum açma modunu** seçmek için ekranın en üstünde **SAML** modu.

      ![image](./media/google-apps-tutorial/b1_b2_saml_ssso.png)

3. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunu tıklatın **seçin** için **SAML** modu, çoklu oturum açmayı etkinleştirmek için.

    ![image](./media/google-apps-tutorial/b1_b2_saml_sso.png)

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için düğmeyi **temel SAML yapılandırma** iletişim.

    ![image](./media/google-apps-tutorial/b1-domains_and_urlsedit.png)

5. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://www.google.com/a/<yourdomain.com>/ServiceLogin?continue=https://mail.google.com`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak:
    | |
    |--|
    | `google.com/a/<yourdomain.com>` |
    | `google.com` |
    | `http://google.com` |
    | `http://google.com/a/<yourdomain.com>` |

    ![image](./media/google-apps-tutorial/b1-domains_and_urls.png)

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [G Suite istemci Destek ekibine](https://www.google.com/contact/) bu değerleri almak için.

6. G Suite uygulama belirli bir biçimde SAML onaylamalarını bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** uygulama tümleştirme sayfasında bölümü. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için düğmeyi **kullanıcı öznitelikleri** iletişim.

    ![image](./media/google-apps-tutorial/i3-attribute.png)

7. İçinde **kullanıcı taleplerini** bölümünde **kullanıcı öznitelikleri** iletişim kutusunda, SAML belirteci özniteliği yukarıdaki görüntüde gösterilen şekilde yapılandırın ve aşağıdaki adımları gerçekleştirin:

    a. Tıklayın **Düzenle** açmak için düğmeyi **yönetmek, kullanıcı talepleri** iletişim.

    ![image](./media/google-apps-tutorial/i2-attribute.png)

    ![image](./media/google-apps-tutorial/i4-attribute.png)

    b. Gelen **kaynak özniteliği** selelct öznitelik değeri listesi.

    c. **Kaydet**’e tıklayın.

8. Üzerinde **yukarı çoklu oturum açma SAML ile Ayarla** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** olarak başına uygun sertifikayı indirmek için gereksinim ve bilgisayarınıza kaydedin.

    ![image](./media/google-apps-tutorial/certificatebase64.png)

9. Üzerinde **G Suite kümesi** bölümünde, ihtiyacınıza göre uygun URL'yi kopyalayın.

    Not: URL aşağıdaki bildirebilir

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

    ![image](./media/google-apps-tutorial/d1_saml.png) 

10. Tarayıcınızda yeni bir sekme açın ve oturum [G Suite Yönetici Konsolu](http://admin.google.com/) yönetici hesabınızı kullanarak.

11. Tıklayın **güvenlik**. Bağlantıyı görmüyorsanız, bunun altında gizlenebilir **diğer denetimler** ekranın alt kısmındaki menü.

    ![Güvenlik'e tıklayın.][10]

12. Üzerinde **güvenlik** sayfasında **çoklu oturum açmayı (SSO) ayarlayın.**

    ![SSO tıklayın.][11]

13. Şu yapılandırma değişiklikleri gerçekleştirin:

    ![SSO yapılandırma][12]

    a. Seçin **üçüncü taraf kimlik sağlayıcısı ile Kurulum SSO**.

    b. İçinde **oturum açma sayfası URL'si** alan G Suite'te, değerini yapıştırın **oturum açma URL'si** , Azure Portalı'ndan kopyaladığınız.

    c. İçinde **sayfa oturum kapatma URL'si** alan G Suite'te, değerini yapıştırın **oturum kapatma URL'si** , Azure Portalı'ndan kopyaladığınız.

    d. İçinde **değiştirme parola URL'si** alan G Suite'te, değerini yapıştırın **değiştirme parola URL'si** , Azure Portalı'ndan kopyaladığınız.

    e. G Suite'teki için **doğrulama sertifikası**, Azure portalından indirdiğiniz sertifikasını karşıya yükleyin.

    f. Seçin **belirli bir etki alanı gönderici kullanın**.

    g. Tıklayın **değişiklikleri kaydetmek**.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    ![image](./media/google-apps-tutorial/d_users_and_groups.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![image](./media/google-apps-tutorial/d_adduser.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![image](./media/google-apps-tutorial/d_userproperties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü **brittasimon@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **özellikleri**seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’u seçin.

### <a name="create-a-g-suite-test-user"></a>G Suite test kullanıcısı oluşturma

Bu bölümün amacı, G Suite yazılımı Britta Simon adlı bir kullanıcı oluşturmaktır. G Suite otomatik sağlamayı, varsayılan olarak etkin olan destekler. Bu bölümde hiçbir şey yoktur. Bir kullanıcı, G Suite yazılımı zaten yoksa, G Suite yazılımı erişmeye çalıştığında yeni bir tane oluşturulur.

> [!NOTE]
> Sağlama Azure AD'de çoklu oturum açmayı test etmeden önce açık durumda değil, kullanıcı G Suite içinde zaten mevcut olduğundan emin olun.

> [!NOTE]
> Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [Google Destek ekibine](https://www.google.com/contact/).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, G Suite erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**.

    ![image](./media/google-apps-tutorial/d_all_applications.png)

2. Uygulamalar listesinde **G Suite**.

    ![image](./media/google-apps-tutorial/d_all_proapplications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![image](./media/google-apps-tutorial/d_leftpaneusers.png)

4. Seçin **Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![image](./media/google-apps-tutorial/d_assign_user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. İçinde **atama Ekle** iletişim kutusunda **atama** düğmesi.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde G Suite kutucuğa tıkladığınızda, otomatik olarak G Suite uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[10]: ./media/google-apps-tutorial/gapps-security.png
[11]: ./media/google-apps-tutorial/security-gapps.png
[12]: ./media/google-apps-tutorial/gapps-sso-config.png