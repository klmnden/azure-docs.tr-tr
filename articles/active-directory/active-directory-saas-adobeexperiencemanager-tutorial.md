---
title: "Öğretici: Azure Active Directory Tümleştirme ile Adobe deneyimi Manager | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ve Adobe deneyimi Yöneticisi arasındaki yapılandırmayı öğrenin."
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
ms.openlocfilehash: 4a4fccc4210fd6cf0ddbe99089c84a1fd38d5b09
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-experience-manager"></a>Öğretici: Azure Active Directory Tümleştirme ile Adobe deneyimi Yöneticisi

Bu öğreticide, Azure Active Directory (Azure AD) ile Adobe deneyimi Manager tümleştirme öğrenin.

Adobe deneyimi Yöneticisi Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Adobe deneyimi Manager'a erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak (çoklu oturum açma) Adobe deneyimi Manager'a açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Adobe deneyimi Yöneticisi ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Adobe deneyimi Manager çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Adobe deneyimi Yöneticisi ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-adobe-experience-manager-from-the-gallery"></a>Galeriden Adobe deneyimi Yöneticisi ekleme
Azure AD tümleştirmeye Adobe deneyimi Yöneticisi'ni yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Adobe deneyimi Manager eklemeniz gerekir.

**Galeriden Adobe deneyimi Yöneticisi eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna yazın **Adobe deneyimi Manager**seçin **Adobe deneyimi Manager** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde Adobe deneyimi Yöneticisi](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Adobe deneyimi "Britta Simon" adlı bir test kullanıcı tabanlı Yöneticisi ile test etme.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen Adobe deneyimi Yöneticisi'nde bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı Adobe deneyimi Yöneticisi'nde arasında bir bağlantı ilişkisi kurulması gerekir.

Adobe deneyimi Yöneticisi'nde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Adobe deneyimi Yöneticisi ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Adobe deneyimi Manager test kullanıcısı oluşturma](#create-an-adobe-experience-manager-test-user)**  - Britta Simon, karşılık gelen Adobe deneyimi kullanıcı Azure AD gösterimini bağlantılı Yöneticisi'nde sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Adobe deneyimi Manager uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Adobe deneyimi Yöneticisi ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Adobe deneyimi Manager** uygulama tümleştirmesi sayfasında, tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_samlbase.png)

3. Üzerinde **Adobe deneyimi yöneticisi etki alanını ve URL'leri** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** modu:

    ![Adobe deneyimi yöneticisi etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_url1.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, AEM sunucunuz üzerinde de tanımlayan benzersiz bir değer yazın. 

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<AEM Server Url>/saml_login`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı ve yanıt URL'si ile güncelleştirin. Kişi [Adobe deneyimi Manager destek ekip](https://helpx.adobe.com/support/experience-manager.html) bu değerleri almak için.
 
4. Göster Gelişmiş URL ayarlarını denetleyin ve uygulamada yapılandırmak istiyorsanız aşağıdaki adımı gerçekleştirin **SP** modu tarafından başlatılan:

    ![Adobe deneyimi yöneticisi etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_spconfigure.png)

    İçinde **oturum üzerinde URL'si** metin kutusuna, Adobe deneyimi Manager sunucusunun URL'sini yazın. 

5. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_certificate.png) 

6. Adobe deneyimi Yöneticisi yapılandırma bölümünde yapılandırmak Adobe deneyimi Yöneticisi yapılandırma oturum açma penceresini açmak için tıklatın. Kopya **SAML oturum açma hizmet URL'si**, **SAML varlık kimliği** ve **Sign-Out kimliği** hızlı başvuru bölümünden.

    ![Yapılandırma bölümü bağlantısı](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_configure.png) 

7. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_general_400.png)

8. Açık **Adobe deneyimi Manager** başka bir tarayıcı penceresinde Yönetici portalı.

9. Seçin **ayarları** -> **güvenlik** -> **kullanıcılar**.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_user.png)

10. Seçin **yönetici** veya ilgili herhangi bir kullanıcı.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin6.png)

11. Seçin **hesap ayarları** -> **oluşturma/yönetmek TrustStore**.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_managetrust.png)

12. Tıklatın **sertifika dosyasını seçin** gelen **CER dosyasından sertifika Ekle** düğmesi. Göz atın ve Azure portalından indirdiğiniz sertifika dosyasını seçin.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_user2.png)

13. Sertifika için TrustStore eklenir. Diğer sertifika adını not edin.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin7.png)

14. Üzerinde **kullanıcılar** sayfasında, **kimlik doğrulama hizmeti**.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin8.png)

15. Seçin **hesap ayarları** -> **Oluştur/Yönet KeyStore**. Bir parola sağlayarak bir anahtar oluşturun.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin9.png)

16. Geri dönerek seçin ve yönetici ekran **ayarları** -> **Operations** -> **Web Konsolu**.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin1.png)

17. Bu yapılandırma sayfasını açar.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin2.png)

18. Bul **Adobe Granit SAML 2.0 kimlik doğrulama işleyicisi** ve tıklayın **Ekle** simgesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin3.png)

19. Bu sayfada Eylemler şu gerçekleştirin.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin4.png)

    a. İçinde **yolu** metin girin  **/** .

    b. İçinde **IDP URL** metin değeri girin **SAML oturum açma hizmet URL'si**, Azure Portalı'ndan kopyalanan.

    c. İçinde **IDP sertifika diğer adı** metin değeri girin **sertifika diğer adı**, TrustStore içinde eklenen.

    d. İçinde **güvenlik sağlanan varlık kimliği** metin kutusuna, benzersiz bir değer girin **SAML varlık kimliği**, Azure Portalı'nda yapılandırılmış.

    e. İçinde **onaylama tüketici hizmeti URL'si** metin değeri girin **yanıt URL'si**, Azure Portalı'nda yapılandırılmış.

    f. İçinde **anahtarı parola deposunun** metin girin **parola**, bir anahtar deposunda ayarlanmış.

    g. İçinde **kullanıcı öznitelik kimliği** metin girin **ad kimliği** veya sizin durumunuzda ilgili olan diğer kullanıcı kimliği.

    h. Seçin **otomatik oluştur CRX kullanıcılar.**

    ı. İçinde **oturum kapatma URL'si** metin kutusuna, benzersiz bir değer girin **Sign-Out URL** Azure Portalı'ndan sahip.

    j. Tıklatın **Kaydet**

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-adobeexperiencemanager-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-adobeexperiencemanager-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-adobeexperiencemanager-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-adobeexperiencemanager-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**'a tıklayın.
  
### <a name="create-an-adobe-experience-manager-test-user"></a>Adobe deneyimi Manager test kullanıcısı oluşturma

Bu bölümde, Adobe deneyimi Yöneticisi'nde Britta Simon adlı bir kullanıcı oluşturun. Seçtiyseniz **otomatik oluştur CRX kullanıcılar** seçeneği kullanıcılar başarılı kimlik doğrulamasından sonra otomatik olarak oluşturulur. 

Kullanıcılar el ile oluşturmak istiyorsanız, lütfen çalışmak [Adobe deneyimi Manager destek ekip](https://helpx.adobe.com/support/experience-manager.html) Adobe deneyimi Manager platform kullanıcıları eklemek için. 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Adobe deneyimi Yöneticisi için erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Adobe deneyimi Manager Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Adobe deneyimi Manager**.

    ![Uygulamalar listesinde Adobe deneyimi Manager bağlantısı](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Adobe deneyimi Manager parçasında tıklattığınızda, otomatik olarak Adobe deneyimi Manager uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_general_203.png

