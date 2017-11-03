---
title: "Öğretici: Azure Active Directory Tümleştirme SkyDesk e-posta ile | Microsoft Docs"
description: "Çoklu oturum açma SkyDesk e-posta ile Azure Active Directory arasındaki yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a9d0bbcb-ddb5-473f-a4aa-028ae88ced1a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.openlocfilehash: 0ffcca4161fc836192fc9c9871a905f36ea76b32
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-skydesk-email"></a>Öğretici: Azure Active Directory Tümleştirme SkyDesk e-posta ile

Bu öğreticide, Azure Active Directory (Azure AD) ile SkyDesk e-posta tümleştirme öğrenin.

Azure AD ile SkyDesk e-posta tümleştirme ile aşağıdaki avantajları sağlar:

- SkyDesk e-posta erişimi, Azure AD'de kontrol edebilirsiniz
- Azure AD hesaplarına otomatik olarak SkyDesk e-posta (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme SkyDesk e-posta ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir SkyDesk e-posta çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, burada bir aylık deneme alabilirsiniz [deneme teklifi](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden SkyDesk e-posta ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-skydesk-email-from-the-gallery"></a>Galeriden SkyDesk e-posta ekleme
Azure AD SkyDesk e-posta tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden SkyDesk e-posta eklemeniz gerekir.

**Galeriden SkyDesk e-posta eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **SkyDesk e-posta**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_search.png)

5. Sonuçlar panelinde seçin **SkyDesk e-posta**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırmanız ve SkyDesk e-posta ile Azure AD çoklu oturum açmayı test "Britta Simon" adlı bir test kullanıcı tabanlı.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen SkyDesk e-posta içinde bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının SkyDesk e-posta ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Değeri SkyDesk postada atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma SkyDesk e-posta ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[SkyDesk e-posta test kullanıcısı oluşturma](#creating-a-skydesk-email-test-user)**  - Britta Simon, karşılık gelen SkyDesk kullanıcı Azure AD gösterimini bağlantılı e-posta olmayacağını.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma SkyDesk e-posta uygulamanızı yapılandırın.

**Azure AD çoklu oturum açma SkyDesk e-posta ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **SkyDesk e-posta** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_samlbase.png)

3. Üzerinde **SkyDesk e-posta etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_url.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://mail.skydesk.jp/portal/<companyname>`

    > [!NOTE] 
    > Değer gerçek değil. Değerin gerçek oturum açma URL'si ile güncelleştirin. Kişi [SkyDesk e-posta istemcisi destek ekibi](https://www.skydesk.sg/support/) değeri alınamıyor. 
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_400.png)

6. Üzerinde **SkyDesk e-posta Yapılandırması** 'yi tıklatın **SkyDesk e-posta Yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_configure.png) 

7. İçinde SSO'yu etkinleştirmek için **SkyDesk e-posta**, aşağıdaki adımları gerçekleştirin:

    a. SkyDesk e-posta hesabınız yönetici olarak oturum.

    b. Üstteki menüde tıklatın **Kurulum**seçip **Org**. 
    
      ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_51.png)
  
    c. Tıklayın **etki alanları** sol panelindeki.
    
      ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_53.png)

    d. Tıklayın **etki alanı ekleme**.
    
      ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_54.png)

    e. Etki alanı adınızı girin ve ardından etki alanını doğrulayın.
    
      ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_55.png)

    f. Tıklayın **SAML kimlik doğrulaması** sol panelindeki.
    
      ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_52.png)

8. Üzerinde **SAML kimlik doğrulaması** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
   
      ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_56.png)
   
    >[!NOTE]
    >SAML tabanlı kimlik doğrulaması kullanmak için ya da olmalıdır **etki alanını doğruladıysanız** veya **portalı URL'si** kurulumu. Portal ayarlayabilirsiniz URL benzersiz bir ad ile.
    > 
    > 
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_57.png)

    a. İçinde **oturum açma URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si**, Azure portalından kopyalanan.
   
    b. İçinde **oturum kapatma** URL textbox değeri yapıştırın **Sign-Out URL**, Azure portalından kopyalanan.

    c. **Değiştirme parola URL'si** isteğe bağlı olduğu kadar boş bırakın.

    d. Tıklayın **anahtarı dosyadan al** Azure Portalı'ndan indirilen sertifikanızı seçin ve ardından **açık** sertifikayı karşıya yüklemek için.

    e. Olarak **algoritması**seçin **RSA**.

    f. Tıklatın **Tamam** değişiklikleri kaydedin.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="creating-a-skydesk-email-test-user"></a>SkyDesk e-posta test kullanıcısı oluşturma

Bu bölümde, Britta Simon SkyDesk e-postayla adlı bir kullanıcı oluşturun.

1. Tıklayın **kullanıcı erişimini** sol panel SkyDesk e-posta ve kullanıcı adınızı girin. 

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_58.png)

>[!NOTE] 
>Toplu kullanıcılar oluşturmanız gerekiyorsa, başvurmanız gerekir [SkyDesk e-posta istemcisi destek ekibi](https://www.skydesk.sg/support/).


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta SkyDesk e-postasına erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**SkyDesk e-posta Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **SkyDesk e-posta**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümün amacı erişim paneli kullanılarak Azure AD SSO yapılandırmanızı test etmektir.

Erişim paneli SkyDesk e-posta parçasında tıklattığınızda, otomatik olarak SkyDesk e-posta uygulamanızı açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_203.png

