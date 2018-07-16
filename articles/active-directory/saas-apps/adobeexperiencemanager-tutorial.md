---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Adobe Experience Manager | Microsoft Docs'
description: Azure Active Directory ve Adobe Experience Manager arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 88a95bb5-c17c-474f-bb92-1f80f5344b5a
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: jeedes
ms.openlocfilehash: 56b392e57809cea0ae93800df39bb9dacd164ce2
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39054191"
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-experience-manager"></a>Öğretici: Azure Active Directory Tümleştirme ile Adobe Experience Manager

Bu öğreticide, Azure Active Directory (Azure AD) ile Adobe Experience Manager tümleştirme konusunda bilgi edinin.

Adobe Experience Manager'ı Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Adobe Experience Manager için erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak Adobe Experience Manager için kendi Azure AD hesapları ile oturum, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda--Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Adobe Experience Manager ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Bir Adobe Experience Manager çoklu oturum açma abonelik etkin.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamında kullanımı önerilmemektedir.

Bu öğreticideki adımları test etmek için aşağıdaki önerileri uygulayın:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Bir Azure AD deneme ortamı yoksa [ücretsiz bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide açıklanan senaryo iki temel yapı taşları oluşur:

1. Adobe Experience Manager galeri ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="add-adobe-experience-manager-from-the-gallery"></a>Adobe Experience Manager Galeriden Ekle
Azure AD'de, Adobe Experience Manager tümleştirme yapılandırmak için Adobe Experience Manager Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Adobe Experience Manager Galeriden eklemek için aşağıdaki adımları uygulayın:**

1. İçinde [Azure portalında](https://portal.azure.com), sol bölmede seçin **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Git **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni bir uygulama eklemek için seçin **yeni uygulama** iletişim kutusunun üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Adobe Experience Manager**. Seçin **Adobe Experience Manager** sonuçlar paneli ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Adobe Experience Manager sonuç listesinde](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma ile Adobe Experience "Britta Simon." adlı bir test kullanıcı tabanlı Manager test etme

Tek iş için oturum açma için Azure AD, Adobe Experience Manager karşılık gelen kullanıcı Azure AD'de bir kullanıcı için olan bilmesi gerekir. Diğer bir deyişle, Adobe Experience Manager içinde bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı kurmak gerekir.

Adobe Experience Manager içindeki değeri vermek **kullanıcıadı** aynı değeri **kullanıcı adı** Azure AD'de. Şimdi iki kullanıcı arasındaki bağlantı kurduğunuz. 

Yapılandırma ve Azure AD çoklu oturum açma ile Adobe Experience Manager sınamak için aşağıdaki yapı taşlarını tamamlayın:

1. [Azure AD çoklu oturum açmayı yapılandırma](#configure-azure-ad-single-sign-on) kullanıcılarınız bu özelliği kullanmak etkinleştirmek için.
2. [Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user) Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. [Bir Adobe Experience Manager test kullanıcısı oluşturma](#create-an-adobe-experience-manager-test-user) Adobe deneyimi kullanıcı Azure AD gösterimini bağlı Yöneticisi'nde bir karşılığı Britta simon'un sağlamak için.
4. [Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user) Britta Simon, Azure AD çoklu oturum açma kullanmak üzere etkinleştirmek için.
5. [Çoklu oturum açmayı test](#test-single-sign-on) yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Adobe Experience Manager uygulamanızda çoklu oturum açmayı yapılandırın.

**Adobe Experience Manager ile Azure AD çoklu oturum açmayı yapılandırmak için aşağıdaki adımları uygulayın:**

1. Azure portalında, üzerinde **Adobe Experience Manager** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Çoklu oturum açma, etkinleştirmek için **çoklu oturum açma** iletişim kutusundaki **modu** açılan menüsünde, select **SAML tabanlı oturum açma**.
 
    ![Çoklu oturum açma iletişim kutusu](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_samlbase.png)

3. İçinde **Adobe Experience Manager etki alanı ve URL'ler** bölümünde, istediğinizde uygulamada yapılandırmak aşağıdaki adımları uygulayın **IDP** modu:

    ![Adobe Experience Manager etki alanı ve URL'ler tek oturum açma bilgileri](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_url1.png)

    a. İçinde **tanımlayıcı** AEM sunucunuzda de tanımlayan benzersiz bir değer yazın. 

    b. İçinde **yanıt URL'si** kutusuna aşağıdaki desene sahip bir URL yazın: `https://<AEM Server Url>/saml_login`.

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısıyla güncelleştirin ve yanıt URL'si. Bu değerleri almak için iletişime geçin [Adobe Experience Manager destek ekip](https://helpx.adobe.com/support/experience-manager.html).
 
4. Denetleme **Gelişmiş URL ayarlarını göster**. Ardından, uygulamayı yapılandırmak istiyorsanız aşağıdaki adımları uygulayın **SP** başlatılan modu:

    ![Adobe Experience Manager etki alanı ve URL'ler tek oturum açma bilgileri](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_spconfigure.png)

    İçinde **işareti bulunan URL'si** kutusuna, Adobe Experience Manager sunucusunun URL'sini yazın. 

5. İçinde **SAML imzalama sertifikası** bölümünden **sertifika (Base64)**. Ardından, bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_certificate.png) 

6. Adobe Experience Manager yapılandırma bölümünde oturum açma yapılandırması penceresi açmak için seçmeniz **yapılandırma Adobe Experience Manager**. Kopyalama **SAML oturum açma hizmeti URL'si**, **SAML varlık kimliği**, ve **oturum kapatma kimliği** hızlı başvuru bölümünden.

    ![Yapılandırma bölümü bağlantısı](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_configure.png) 

7. **Kaydet**’i seçin.

    ![Kaydet düğmesi çoklu oturum açmayı yapılandırın](./media/adobeexperiencemanager-tutorial/tutorial_general_400.png)

8. Başka bir tarayıcı penceresinde açmak **Adobe Experience Manager** Yönetim Portalı.

9. Seçin **ayarları** > **güvenlik** > **kullanıcılar**.

    ![Çoklu oturum açma düğmesi kaydetme yapılandırın](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_user.png)

10. Seçin **yönetici** veya ilgili herhangi bir kullanıcı.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin6.png)

11. Seçin **hesap ayarları** > **yönetme TrustStore**.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_managetrust.png)

12. Altında **CER dosyası ekleme sertifikadan**, tıklayın **sertifika dosyası seçin**. Göz atın ve zaten Azure portalından indirdiğiniz sertifika dosyasını seçin.

    ![Kaydet düğmesi çoklu oturum açmayı yapılandırın](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_user2.png)

13. Sertifika için TrustStore eklenir. Diğer sertifika adını not edin.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin7.png)

14. Üzerinde **kullanıcılar** sayfasında **kimlik doğrulama hizmeti**.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin8.png)

15. Seçin **hesap ayarları** > **oluşturma/yönetme KeyStore**. KeyStore için bir parola sağlanarak oluşturun.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin9.png)

16. Yönetici ekranına dönün. Ardından **ayarları** > **işlemleri** > **Web Konsolu**.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin1.png)

    Bu yapılandırma sayfasını açar.

    ![Çoklu oturum açma düğmesi kaydetme yapılandırın](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin2.png)

17. Bulma **Adobe Granit SAML 2.0 kimlik doğrulama işleyicisi**. Ardından **Ekle** simgesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin3.png)

19. Bu sayfada aşağıdaki eylemleri gerçekleştirin.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin4.png)

    a. İçinde **yolu** kutusuna **/**.

    b. İçinde **IDP URL** kutusuna **SAML oturum açma hizmeti URL'si** Azure portaldan kopyaladığınız değeri.

    c. İçinde **IDP sertifika diğer adı** kutusuna **sertifika diğer adı** TrustStore içinde eklenen değer.

    d. İçinde **güvenlik sağlanan varlık kimliği** kutusunda, benzersiz bir değer girin **SAML varlık kimliği** Azure portalında yapılandırılmış değer.

    e. İçinde **onay belgesi tüketici hizmeti URL'si** kutusuna **yanıt URL'si** Azure portalında yapılandırılmış değer.

    f. İçinde **anahtarı parola Store** kutusuna **parola** , anahtar deposu ayarlayın.

    g. İçinde **kullanıcı öznitelik kimliği** kutusuna **ad kimliği** veya sizin durumunuzda ilgili başka bir kullanıcı kimliği.

    h. Seçin **otomatik oluştur CRX kullanıcılar**.

    i. İçinde **oturum kapatma URL'si** kutusunda, benzersiz bir değer girin **oturum kapatma URL'si** Azure portalından aldığınız değeri.

    j. **Kaydet**’i seçin.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com) uygulamasını ayarladığınız sırada. Bu uygulamadan ekledikten sonra **Active Directory** > **kurumsal uygulamalar** bölümünden **çoklu oturum açma** sekmesi. Ekli belgelerin sonra erişim **yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında [Azure AD belgeleri katıştırılmış]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları uygulayın:**

1. Azure portalında, sol bölmede seçin **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/adobeexperiencemanager-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/adobeexperiencemanager-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, üst kısmındaki **tüm kullanıcılar** iletişim kutusunda **Ekle**.

    ![Ekle düğmesi](./media/adobeexperiencemanager-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları uygulayın:

    ![Kullanıcı iletişim kutusu](./media/adobeexperiencemanager-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusu. Ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’u seçin.
  
### <a name="create-an-adobe-experience-manager-test-user"></a>Bir Adobe Experience Manager test kullanıcısı oluşturma

Bu bölümde, Britta Simon, Adobe Experience Manager adlı bir kullanıcı oluşturun. Seçtiyseniz **otomatik oluştur CRX kullanıcılar** seçeneği, kullanıcıların otomatik olarak oluşturulur başarılı kimlik doğrulamasından sonra. 

Kullanıcıları el ile oluşturmak istiyorsanız, çalışmak [Adobe Experience Manager destek ekip](https://helpx.adobe.com/support/experience-manager.html) Adobe Experience Manager platform kullanıcıları eklemek için. 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Adobe Experience Manager erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon için Adobe Experience Manager atamak için aşağıdaki adımları uygulayın:**

1. Azure portalında uygulama görünümünü açın. Ardından, dizin görünümüne gitmek **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **Adobe Experience Manager**.

    ![Uygulamalar listesinde Adobe Experience Manager bağlantısı](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Seçin **Ekle** düğmesi. Ardından **atama Ekle** iletişim kutusunda **kullanıcılar ve gruplar**.

    ![Atama Ekle bölmesi][203]

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. İçinde **kullanıcılar ve gruplar** iletişim kutusu, tıklayın **seçin** düğmesi.

7. İçinde **atama Ekle** iletişim kutusunda **atama** düğmesi.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test.

Erişim Paneli'nde Adobe Experience Manager kutucuğu seçtiğinizde, otomatik olarak Adobe Experience Manager uygulamanıza oturum açmış.

Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/adobeexperiencemanager-tutorial/tutorial_general_01.png
[2]: ./media/adobeexperiencemanager-tutorial/tutorial_general_02.png
[3]: ./media/adobeexperiencemanager-tutorial/tutorial_general_03.png
[4]: ./media/adobeexperiencemanager-tutorial/tutorial_general_04.png

[100]: ./media/adobeexperiencemanager-tutorial/tutorial_general_100.png

[200]: ./media/adobeexperiencemanager-tutorial/tutorial_general_200.png
[201]: ./media/adobeexperiencemanager-tutorial/tutorial_general_201.png
[202]: ./media/adobeexperiencemanager-tutorial/tutorial_general_202.png
[203]: ./media/adobeexperiencemanager-tutorial/tutorial_general_203.png

