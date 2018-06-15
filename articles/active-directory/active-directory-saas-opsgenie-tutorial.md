---
title: 'Öğretici: Azure Active Directory Tümleştirme ile OpsGenie | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile OpsGenie arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 41b59b22-a61d-4fe6-ab0d-6c3991d1375f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/16/2018
ms.author: jeedes
ms.openlocfilehash: b42320884f5478d05c305185334da42097f603ec
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34347552"
---
# <a name="tutorial-azure-active-directory-integration-with-opsgenie"></a>Öğretici: Azure Active Directory Tümleştirme OpsGenie ile

Bu öğreticide, Azure Active Directory (Azure AD) ile OpsGenie tümleştirmek öğrenin.

OpsGenie Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- OpsGenie erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için OpsGenie (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme OpsGenie ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir OpsGenie çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden OpsGenie ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-opsgenie-from-the-gallery"></a>Galeriden OpsGenie ekleme
Azure AD OpsGenie tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden OpsGenie eklemeniz gerekir.

**Galeriden OpsGenie eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **OpsGenie**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_search.png)

5. Sonuçlar panelinde seçin **OpsGenie**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı OpsGenie sınayın.

Tekli çalışmaya oturum için Azure AD OpsGenie karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının OpsGenie ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

OpsGenie içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma OpsGenie ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[OpsGenie test kullanıcısı oluşturma](#creating-a-opsgenie-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı OpsGenie sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma OpsGenie uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile OpsGenie yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **OpsGenie** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_samlbase.png)

3. Üzerinde **OpsGenie etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_url.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL'yi yazın: `https://app.opsgenie.com/auth/login`

4. Üzerinde **SAML imzalama sertifikası** bölümünde, kopyalamak için Kopyala düğmesini tıklatın **uygulama Federasyon meta veri URL'sini** ve Not Defteri'ne yapıştırın.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_certificate.png)

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-opsgenie-tutorial/tutorial_general_400.png)

6. Üzerinde **OpsGenie yapılandırma** 'yi tıklatın **yapılandırma OpsGenie** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML çoklu oturum açma hizmet URL'si** hızlı başvuru bölümünden.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_configure.png)

7. Başka bir tarayıcı örneği açın ve ardından günlük için yönetici olarak OpsGenie bileşenini.

8. Tıklatın **ayarları**ve ardından **çoklu oturum açma** sekmesi.
   
    ![OpsGenie çoklu oturum açma](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_06.png)

9. SSO'yu etkinleştirmek için seçin **etkin**.
   
    ![OpsGenie ayarları](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_07.png) 

10. İçinde **sağlayıcı** 'yi tıklatın **Azure Active Directory** sekmesi.
   
    ![OpsGenie ayarları](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_08.png) 

11. Azure Active Directory iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![OpsGenie ayarları](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_09.png)
    
    a. İçinde **SAML 2.0 Endpoint** metin kutusuna, Yapıştır **tek oturum üzerinde hizmet URL'si**Azure portalından kopyaladığınız değeri.
    
    b. İçinde **meta veri URL'sini:** metin kutusuna, Yapıştır **uygulama Federasyon meta veri URL'sini** Azure portalından kopyaladığınız değeri.
    
    c. Tıklatın **değişiklikleri kaydetmek**.

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-opsgenie-test-user"></a>OpsGenie test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon içinde OpsGenie adlı bir kullanıcı oluşturmaktır. 

1. Bir web tarayıcısı penceresinde OpsGenie Kiracı yönetici olarak oturum açın.

2. Tıklayarak kullanıcıları listesine gidin **kullanıcı** sol panelinde.
   
   ![OpsGenie ayarları](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_10.png) 

3. Tıklatın **kullanıcı ekleme**.

4. Üzerinde **Kullanıcı Ekle** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
   ![OpsGenie ayarları](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_11.png)
   
   a. İçinde **e-posta** metin kutusuna, e-posta adresi türü BrittaSimon ele Azure Active Directory'de.
   
   b. İçinde **tam adı** metin kutusuna, türü **Britta Simon**.
   
   c. **Kaydet**’e tıklayın. 

>[!NOTE]
>Britta kendi profili ayarlama yönergeleri içeren bir e-posta alır.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta OpsGenie için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**OpsGenie için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **OpsGenie**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümün amacı erişim paneli kullanılarak Azure AD SSO yapılandırmanızı test etmektir.

Erişim paneli OpsGenie parçasında tıklattığınızda, otomatik olarak OpsGenie uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_203.png

