---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Adobe deneyimi Manager | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ve Adobe deneyimi Yöneticisi arasındaki yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 88a95bb5-c17c-474f-bb92-1f80f5344b5a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: jeedes
ms.openlocfilehash: 4904d9b0fc0746bc915477bb0ac83a0083ca198f
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35980662"
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-experience-manager"></a>Öğretici: Azure Active Directory Tümleştirme ile Adobe deneyimi Yöneticisi

Bu öğreticide, Azure Active Directory (Azure AD) ile Adobe deneyimi Manager tümleştirme öğrenin.

Adobe deneyimi Yöneticisi Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Adobe deneyimi Manager'a erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak Adobe deneyimi Manager ile Azure AD hesaplarına oturum, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda--Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz: [uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Adobe deneyimi Yöneticisi ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Adobe deneyimi Manager çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamında kullanmanızı öneririz yok.

Bu öğreticide adımları test etmek için aşağıdaki önerileri uygulayın:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa [ücretsiz bir aylık deneme almak](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Adobe deneyimi Yöneticisi ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="add-adobe-experience-manager-from-the-gallery"></a>Galeriden Adobe deneyimi Yöneticisi ekleme
Azure AD tümleştirmeye Adobe deneyimi Yöneticisi'ni yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Adobe deneyimi Manager eklemeniz gerekir.

**Galeriden Adobe deneyimi Yöneticisi eklemek için aşağıdaki adımları uygulayın:**

1. İçinde [Azure portal](https://portal.azure.com), sol bölmede seçin **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Git **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni bir uygulama eklemek için seçin **yeni uygulama** iletişim kutusunun üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Adobe deneyimi Manager**. Seçin **Adobe deneyimi Manager** sonuçlar paneli ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Sonuçlar listesinde Adobe deneyimi Yöneticisi](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Adobe deneyimi "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı Yöneticisi ile test etme

Tekli çalışmaya oturum için Azure AD için bir kullanıcı Azure AD içinde karşılık gelen kullanıcı Adobe deneyimi Yöneticisi'nde olan bilmek ister. Diğer bir deyişle, Adobe deneyimi Yöneticisi'nde bir Azure AD kullanıcısının ve ilgili kullanıcı arasında bir bağlantı oluşturmanız gerekir.

Adobe deneyimi Yöneticisi'nde değere vermek **kullanıcıadı** aynı değerini **kullanıcı adı** Azure AD'de. Şimdi iki kullanıcılar arasında bağlantı kurulduktan. 

Yapılandırma ve Azure AD çoklu oturum açma Adobe deneyimi Yöneticisi ile test etmek için aşağıdaki yapı taşları tamamlayın:

1. [Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on) bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. [Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user) Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. [Adobe deneyimi Manager test kullanıcısı oluşturma](#create-an-adobe-experience-manager-test-user) karşılık gelen Britta Simon, Adobe deneyimi kullanıcı Azure AD gösterimini bağlantılı Yöneticisi'nde sağlamak için.
4. [Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user) Britta Azure AD çoklu oturum açma kullanmak Simon etkinleştirmek için.
5. [Test çoklu oturum açma](#test-single-sign-on) yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Adobe deneyimi Manager uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Adobe deneyimi Yöneticisi ile yapılandırmak için aşağıdaki adımları uygulayın:**

1. Azure portalında üzerinde **Adobe deneyimi Manager** uygulama tümleştirmesi sayfasında, **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Çoklu oturum açma, etkinleştirmek için **çoklu oturum açma** iletişim kutusunda **modu** açılır menüsünde, select **SAML tabanlı oturum açma**.
 
    ![Çoklu oturum açma iletişim kutusu](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_samlbase.png)

3. İçinde **Adobe deneyimi yöneticisi etki alanını ve URL'leri** bölümünde, uygulamada yapılandırmak istiyorsanız, aşağıdaki adımlar **IDP** modu:

    ![Adobe deneyimi yöneticisi etki alanı ve URL'leri tek oturum açma bilgileri](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_url1.png)

    a. İçinde **tanımlayıcısı** AEM sunucunuzda da tanımladığınız benzersiz bir değer yazın. 

    b. İçinde **yanıt URL'si** kutusunda, aşağıdaki desende bir URL yazın: `https://<AEM Server Url>/saml_login`.

    > [!NOTE] 
    > Bu değerler gerçek değildir. Gerçek tanımlayıcısı ile bu değerleri güncelleştirmek ve URL yanıt. Bu değerleri almak için başvurun [Adobe deneyimi Manager destek ekip](https://helpx.adobe.com/support/experience-manager.html).
 
4. Denetleme **Göster Gelişmiş URL ayarları**. Uygulamada yapılandırmak istiyorsanız aşağıdaki adımları uygulayın **SP** modu tarafından başlatılan:

    ![Adobe deneyimi yöneticisi etki alanı ve URL'leri tek oturum açma bilgileri](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_spconfigure.png)

    İçinde **oturum üzerinde URL'si** kutusunda, Adobe deneyimi Manager sunucusunun URL'sini yazın. 

5. İçinde **SAML imzalama sertifikası** bölümünde, select **sertifika (Base64)**. Ardından, bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_certificate.png) 

6. Adobe deneyimi Yöneticisi yapılandırma bölümünde oturum açma yapılandırması penceresini açmak için seçin **Adobe deneyimi Yapılandırma Yöneticisi'ni**. Kopya **SAML oturum açma hizmet URL'si**, **SAML varlık kimliği**, ve **Sign-Out kimliği** hızlı başvuru bölümünden.

    ![Yapılandırma bölümü bağlantısı](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_configure.png) 

7. **Kaydet**’i seçin.

    ![Çoklu oturum açma düğmesi kaydetme yapılandırın](./media/adobeexperiencemanager-tutorial/tutorial_general_400.png)

8. Başka bir tarayıcı penceresinde açmak **Adobe deneyimi Manager** Yönetici portalı.

9. Seçin **ayarları** > **güvenlik** > **kullanıcılar**.

    ![Çoklu oturum açma düğmesi kaydetme yapılandırın](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_user.png)

10. Seçin **yönetici** veya ilgili herhangi bir kullanıcı.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin6.png)

11. Seçin **hesap ayarları** > **yönetmek TrustStore**.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_managetrust.png)

12. Altında **CER dosyasından sertifika Ekle**, tıklatın **sertifika dosyasını seçin**. Gidin ve zaten Azure portalından indirdiğiniz sertifika dosyasını seçin.

    ![Çoklu oturum açma düğmesi kaydetme yapılandırın](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_user2.png)

13. Sertifika için TrustStore eklenir. Diğer sertifika adını not edin.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin7.png)

14. Üzerinde **kullanıcılar** sayfasında, **kimlik doğrulama hizmeti**.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin8.png)

15. Seçin **hesap ayarları** > **Oluştur/Yönet KeyStore**. Bir parola sağlayarak bir anahtar oluşturun.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin9.png)

16. Yönetici ekrana dönün. Ardından **ayarları** > **Operations** > **Web Konsolu**.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin1.png)

    Bu yapılandırma sayfasını açar.

    ![Çoklu oturum açma düğmesi kaydetme yapılandırın](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin2.png)

17. Bul **Adobe Granit SAML 2.0 kimlik doğrulama işleyicisi**. Ardından **Ekle** simgesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin3.png)

19. Bu sayfada aşağıdaki eylemleri gerçekleştirin.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin4.png)

    a. İçinde **yolu** kutusuna **/**.

    b. İçinde **IDP URL** kutusuna **SAML oturum açma hizmet URL'si** Azure portalından kopyaladığınız değeri.

    c. İçinde **IDP sertifika diğer adı** kutusuna **sertifika diğer adı** TrustStore içinde eklenen değer.

    d. İçinde **güvenlik sağlanan varlık kimliği** kutusunda, benzersiz bir değer girin **SAML varlık kimliği** Azure Portalı'nda yapılandırılmış değeri.

    e. İçinde **onaylama tüketici hizmeti URL'si** kutusuna **yanıt URL'si** Azure Portalı'nda yapılandırılmış değeri.

    f. İçinde **anahtarı parola deposunun** kutusuna **parola** , bir anahtar deposunda ayarlayın.

    g. İçinde **kullanıcı öznitelik kimliği** kutusuna **ad kimliği** veya sizin durumunuzda ilgili başka bir kullanıcı kimliği.

    h. Seçin **otomatik oluştur CRX kullanıcılar**.

    i. İçinde **oturum kapatma URL'si** kutusunda, benzersiz bir değer girin **Sign-Out URL** Azure portalından aldığınız değeri.

    j. **Kaydet**’i seçin.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com) uygulaması kuruluyor sırada. Bu uygulamadan ekledikten sonra **Active Directory** > **kurumsal uygulamalar** bölümünde, select **çoklu oturum açma** sekmesi. Katıştırılmış belgeleri aracılığıyla erişim **yapılandırma** alt bölüm. Daha fazla bilgiyi embedded belgeler özelliği hakkında [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları uygulayın:**

1. Azure portalında sol bölmede seçin **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/adobeexperiencemanager-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/adobeexperiencemanager-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** en üstündeki iletişim kutusu **tüm kullanıcılar** iletişim kutusunda **Ekle**.

    ![Ekle düğmesi](./media/adobeexperiencemanager-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları uygulayın:

    ![Kullanıcı iletişim kutusu](./media/adobeexperiencemanager-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusu. Görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’u seçin.
  
### <a name="create-an-adobe-experience-manager-test-user"></a>Adobe deneyimi Manager test kullanıcısı oluşturma

Bu bölümde, Adobe deneyimi Yöneticisi'nde Britta Simon adlı bir kullanıcı oluşturun. Seçtiyseniz **otomatik oluştur CRX kullanıcılar** seçeneği, kullanıcıların otomatik olarak oluşturulur başarılı kimlik doğrulamasından sonra. 

Kullanıcılar el ile oluşturmak istiyorsanız, çalışmak [Adobe deneyimi Manager destek ekip](https://helpx.adobe.com/support/experience-manager.html) Adobe deneyimi Manager platform kullanıcıları eklemek için. 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Adobe deneyimi Yöneticisi erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Adobe deneyimi Manager Britta Simon atamak için aşağıdaki adımları uygulayın:**

1. Azure portalında uygulamaları görünümünü açın. Ardından, dizin görünümüne gidin **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Adobe deneyimi Manager**.

    ![Uygulamalar listesinde Adobe deneyimi Manager bağlantısı](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_app.png)  

3. Soldaki menüde seçin **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Seçin **Ekle** düğmesi. Ardından **eklemek atama** iletişim kutusunda **kullanıcılar ve gruplar**.

    ![Ekleme atama bölmesi][203]

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. İçinde **kullanıcılar ve gruplar** iletişim kutusu, tıklatın **seçin** düğmesi.

7. İçinde **eklemek atama** iletişim kutusunda **atamak** düğmesi.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test.

Erişim panelinde Adobe deneyimi Yöneticisi kutucuğu seçtiğinizde, otomatik olarak Adobe deneyimi Manager uygulamanıza oturum.

Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)



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

