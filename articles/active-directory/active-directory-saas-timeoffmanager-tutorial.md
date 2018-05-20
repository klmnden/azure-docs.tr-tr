---
title: 'Öğretici: Azure Active Directory Tümleştirme ile TimeOffManager | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile TimeOffManager arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 3685912f-d5aa-4730-ab58-35a088fc1cc3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: 462a77e56f2dc28a3a3258ab44a1486a4dd257e4
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-timeoffmanager"></a>Öğretici: Azure Active Directory Tümleştirme TimeOffManager ile

Bu öğreticide, Azure Active Directory (Azure AD) ile TimeOffManager tümleştirmek öğrenin.

TimeOffManager Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- TimeOffManager erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için TimeOffManager (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme TimeOffManager ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir TimeOffManager çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden TimeOffManager Ekle
2. Yapılandırma ve Azure AD çoklu oturum açmayı test etme

## <a name="add-timeoffmanager-from-the-gallery"></a>Galeriden TimeOffManager Ekle
Azure AD TimeOffManager tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden TimeOffManager eklemeniz gerekir.

**Galeriden TimeOffManager eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **TimeOffManager**seçin **TimeOffManager** sonuç paneli ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Galerisi'nden ekleme](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı TimeOffManager sınayın.

Tekli çalışmaya oturum için Azure AD TimeOffManager karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının TimeOffManager ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

TimeOffManager içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma TimeOffManager ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[TimeOffManager test kullanıcısı oluşturma](#create-a-timeoffmanager-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı TimeOffManager sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma TimeOffManager uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile TimeOffManager yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **TimeOffManager** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![SAML oturum açma tabanlı](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_samlbase.png)

3. Üzerinde **TimeOffManager etki alanı ve URL'leri** bölümünde, aşağıdaki işlemi gerçekleştirin:

     ![TimeOffManager etki alanı ve URL'ler bölümü](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_url.png)

    İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://www.timeoffmanager.com/cpanel/sso/consume.aspx?company_id=<companyid>`

    > [!NOTE] 
    > Bu değer gerçek değil. Bu değer ile gerçek yanıt URL'si güncelleştirin. Bu değerden alabilirsiniz **çoklu oturum açma ayarları sayfasında** daha sonra öğreticide veya kişi içinde açıklanan [TimeOffManager destek ekibi](http://www.timeoffmanager.com/contact-us.aspx).
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![SAML imzalama sertifikası bölümü](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_certificate.png) 

5. Bu bölümün amacı kullanıcıların TimeOffManger için kendi hesabıyla SAML protokolünü temel Federasyon kullanarak Azure AD kimlik doğrulaması sağlamak nasıl anahat sağlamaktır.
    
    SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemelerini ekleyin gerektiren belirli bir biçimde SAML onaylar TimeOffManger uygulamanızı bekliyor. Aşağıdaki ekran görüntüsünde bunun bir örneği gösterir.

    ![SAML belirteci öznitelikleri](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_attrb.png "saml belirteci öznitelikleri")
    
    | Öznitelik Adı | Öznitelik Değeri |
    | --- | --- |
    | FirstName |User.givenName |
    | Soyadı |User.surname |
    | Email |User.Mail |
    
    a.  Her veri satırının için yukarıdaki tabloda **kullanıcı özniteliği eklemek**.
    
    ![SAML belirteci öznitelikleri](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_addattrb.png "saml belirteci öznitelikleri")
    
    ![SAML belirteci öznitelikleri](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_addattrb1.png "saml belirteci öznitelikleri")
    
    b.  İçinde **öznitelik adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın.
    
    c.  İçinde **öznitelik değeri** metin kutusuna, ilgili satır için gösterilen öznitelik değerini seçin.
    
    d.  **Tamam**’a tıklayın.
    
6. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_400.png)

7. Üzerinde **TimeOffManager yapılandırma** 'yi tıklatın **yapılandırma TimeOffManager** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![TimeOffManager yapılandırma bölümü](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_configure.png) 

8. Farklı web tarayıcısı penceresinde TimeOffManager şirket sitenize yönetici olarak oturum açın.

9. Git **hesap \> Hesap seçenekleri \> tek oturum açma ayarları**.
   
   ![Çoklu oturum açma ayarları](./media/active-directory-saas-timeoffmanager-tutorial/ic795917.png "tek oturum açma ayarları")
7. İçinde **çoklu oturum açma ayarları** bölümünde, aşağıdaki adımları gerçekleştirin:
   
   ![Çoklu oturum açma ayarları](./media/active-directory-saas-timeoffmanager-tutorial/ic795918.png "tek oturum açma ayarları")
   
   a. Base-64 kodlanmış sertifikanızı Not Defteri'nde açın, içeriğini, panoya kopyalayın ve sonra tüm sertifika içine yapıştırabilirsiniz **X.509 sertifikası** metin kutusu.
   
   b. İçinde **IDP veren** metin değerini yapıştırın **SAML varlık kimliği** Azure portalından kopyalanan.
   
   c. İçinde **IDP uç nokta URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si** Azure portalından kopyalanan.
   
   d. Olarak **zorunlu SAML**seçin **Hayır**.
   
   e. Olarak **otomatik olarak oluşturma kullanıcıların**seçin **Evet**.
   
   f. İçinde **oturum kapatma URL'si** metin değerini yapıştırın **Sign-Out URL** Azure portalından kopyalanan.
   
   g. Tıklatın **değişiklikleri kaydetmek**.

11. İçinde **çoklu oturum açma ayarları** sayfasında, değerini kopyalayın **onaylama tüketici hizmeti URL'si** ve yapıştırın **yanıt URL'si** metin kutusu altında **TimeOffManager Etki alanı ve URL'leri** Azure portalı bölümünde. 

      ![Çoklu oturum açma ayarları](./media/active-directory-saas-timeoffmanager-tutorial/ic795915.png "tek oturum açma ayarları")

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-timeoffmanager-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Kullanıcıların ve grupların tüm kullanıcılar-->](./media/active-directory-saas-timeoffmanager-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Ekle düğmesi](./media/active-directory-saas-timeoffmanager-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Kullanıcı iletişim sayfası](./media/active-directory-saas-timeoffmanager-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-timeoffmanager-test-user"></a>TimeOffManager test kullanıcısı oluşturma

Azure AD kullanıcıların TimeOffManager oturum etkinleştirmek için bunlar için TimeOffManager sağlanmalıdır.  

Yalnızca zaman sağlama kullanıcı TimeOffManager destekler. Sizin için eylem öğe yok.  

Kullanıcılar, çoklu oturum açma kullanarak ilk oturum açma sırasında otomatik olarak eklenir.

>[!NOTE]
>API Azure AD kullanıcı hesaplarını sağlamak için TimeOffManager tarafından sağlanan veya herhangi diğer TimeOffManager kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz.
> 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta TimeOffManager için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**TimeOffManager için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **TimeOffManager**.

    ![Uygulama listesinde TimeOffManager](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli TimeOffManager parçasında tıklattığınızda, otomatik olarak TimeOffManager uygulamanıza açan. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_203.png

