---
title: 'Öğretici: Azure Active Directory Tümleştirme ile OpenAthens | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile OpenAthens arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: dd4adfc7-e238-41d5-8b25-1811f08078b6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2017
ms.author: jeedes
ms.openlocfilehash: 3b19f3c7ed84a63f48320a2c7af8d06a9cc5deb4
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-openathens"></a>Öğretici: Azure Active Directory Tümleştirme OpenAthens ile

Bu öğreticide, Azure Active Directory (Azure AD) ile OpenAthens tümleştirmek öğrenin.

OpenAthens Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- OpenAthens erişimi, Azure AD'de kontrol edebilirsiniz.
- Kullanıcılarınız için OpenAthens (çoklu oturum açma) ile Azure AD hesaplarına otomatik olarak oturum açmaya etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda--Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz: [uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme OpenAthens ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir OpenAthens çoklu oturum açma abonelik etkin

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden OpenAthens ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-openathens-from-the-gallery"></a>Galeriden OpenAthens ekleme
Azure AD OpenAthens tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden OpenAthens eklemeniz gerekir.

**Galeriden OpenAthens eklemek için**

1. İçinde [Azure portal](https://portal.azure.com), sol bölmede seçin **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gözat **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.

    ![Kuruluş uygulamaları bölmesi][2]
    
3. Yeni bir uygulama eklemek için seçin **yeni uygulama** iletişim kutusunun üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **OpenAthens**seçin **OpenAthens** sonuçlar paneli ve ardından **Ekle** düğmesi.

    ![Sonuçlar listesinde OpenAthens](./media/active-directory-saas-openathens-tutorial/tutorial_openathens_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı OpenAthens ile test etme

Tekli çalışmaya oturum için Azure AD OpenAthens karşılık gelen kullanıcı kullanıcıya Azure AD'de nedir bilmek ister. Diğer bir deyişle, OpenAthens içinde bir Azure AD kullanıcısının ve ilgili kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

OpenAthens içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma OpenAthens ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. [Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on), bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. [Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user), Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. [Bir OpenAthens test kullanıcısı oluşturma](#create-a-openathens-test-user), karşılık gelen Britta Simon, kullanıcının Azure AD gösterimini bağlı OpenAthens sağlamak için.
4. [Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user), Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. [Test çoklu oturum açma](#test-single-sign-on), yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma OpenAthens uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile OpenAthens yapılandırmak için**

1. Azure portalında üzerinde **OpenAthens** uygulama tümleştirmesi sayfasında, **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantıyı yapılandırın][4]

2. Çoklu oturum açma, etkinleştirmek için **çoklu oturum açma** iletişim kutusunda **SAML tabanlı oturum açma** olarak **modu**.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-openathens-tutorial/tutorial_openathens_samlbase.png)

3. İçinde **OpenAthens etki alanı ve URL'leri** bölümünde, bir değer girin `https://login.openathens.net/saml/2/metadata-sp` içinde **tanımlayıcısı** metin kutusu.

    ![OpenAthens etki alanı ve oturum açma URL'leri tek bilgi](./media/active-directory-saas-openathens-tutorial/tutorial_openathens_url.png)

4. İçinde **SAML imzalama sertifikası** bölümünde, select **meta veri XML**ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Bağlantı AMSL imzalama sertifikası yükle](./media/active-directory-saas-openathens-tutorial/tutorial_openathens_certificate.png) 

5. **Kaydet** düğmesini seçin.

    ![Çoklu oturum açma düğmesi Kaydet](./media/active-directory-saas-openathens-tutorial/tutorial_general_400.png)

6. Farklı web tarayıcısı penceresinde OpenAthens şirket sitenize yönetici olarak oturum açın.

7. Seçin **bağlantıları** altındaki listeden **Yönetim** sekmesi. 

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-openathens-tutorial/tutorial_openathens_application1.png)

8. Seçin **SAML 1.1/2.0**ve ardından **yapılandırma** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-openathens-tutorial/tutorial_openathens_application2.png)
    
9. Yapılandırması eklemek için seçin **Gözat** Azure portalından indirdiğiniz meta veri .xml dosyasını karşıya yükle düğmesine ve ardından **Ekle**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-openathens-tutorial/tutorial_openathens_application3.png)

10. Altında aşağıdaki adımları gerçekleştirin **ayrıntıları** sekmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-openathens-tutorial/tutorial_openathens_application4.png)

    a. İçinde **görünen adı eşlemesi**seçin **kullanım özniteliği**.

    b. İçinde **görünen adı özniteliği** metin kutusunda, değeri girin `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.
    
    c. İçinde **benzersiz kullanıcı eşlemesi**seçin **kullanım özniteliği**.

    d. İçinde **benzersiz kullanıcı özniteliği** metin kutusunda, değeri girin `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.

    e. İçinde **durum**, üç onay kutusunu seçin.

    f. İçinde **yerel hesaplar oluşturma**seçin **otomatik olarak**.

    g. Seçin **değişiklikleri kaydetmek**.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken. Bu uygulamadan ekledikten sonra **Active Directory** > **kurumsal uygulamalar** bölümünde, select **çoklu oturum açma** sekmesinde ve katıştırılmış erişim belgeleri etraflıca **yapılandırma** alt bölüm. Embedded belgeler özelliği hakkında daha fazla bilgi için bkz: [Azure AD embedded belgeler](https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı "Britta Simon." olarak adlandırılan Azure portalında bir test kullanıcı oluşturmaktır

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD bir test kullanıcısı oluşturma**

1. Azure portalında sol bölmede seçin **Azure Active Directory**.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-openathens-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-openathens-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusunda **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-openathens-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-openathens-tutorial/create_aaduser_04.png)

    a. İçinde **adı** metin kutusunda, **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, Britta Simon için e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** metin kutusu.

    d. **Oluştur**’u seçin.
  
### <a name="create-an-openathens-test-user"></a>Bir OpenAthens test kullanıcısı oluşturma

Yalnızca zaman sağlama OpenAthens destekler ve kullanıcıların başarılı kimlik doğrulamasından sonra otomatik olarak oluşturulur. Herhangi bir eylem gerçekleştirmeniz gerekmez.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta OpenAthens için kendi erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**OpenAthens için Britta Simon atamak için**

1. Uygulamaları görüntülemek, Azure portalında, açık dizin görünümüne gidin ve Git **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. İçinde **uygulamaları** listesinde **OpenAthens**.

    ![Uygulamalar listesinde OpenAthens bağlantı](./media/active-directory-saas-openathens-tutorial/tutorial_openathens_app.png)  

3. Soldaki menüde seçin **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Seçin **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** içinde **eklemek atama** bölmesi.

    ![Ekleme atama bölmesi][203]

5. İçinde **kullanıcılar ve gruplar** listesinde **Britta Simon**.

6. Seçin **seçin** düğmesini **kullanıcılar ve gruplar** listesi.

7. Seçin **atamak** düğmesini **eklemek atama** bölmesi.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test.

Seçtiğinizde, **OpenAthens** döşeme erişim panelinde oturumunuz otomatik olarak OpenAthens uygulamanız.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticilerin bir listesi için bkz: [Azure AD ile kullanmak için SaaS uygulama tümleştirmesi öğreticileri](active-directory-saas-tutorial-list.md).
* Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma hakkında daha fazla bilgi için bkz: [uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md).

<!--Image references-->

[1]: ./media/active-directory-saas-openathens-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-openathens-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-openathens-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-openathens-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-openathens-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-openathens-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-openathens-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-openathens-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-openathens-tutorial/tutorial_general_203.png

