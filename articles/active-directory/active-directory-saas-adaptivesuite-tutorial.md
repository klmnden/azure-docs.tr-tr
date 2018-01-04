---
title: "Öğretici: Azure Active Directory Tümleştirme Uyarlamalı Suite ile | Microsoft Docs"
description: "Çoklu oturum açma Uyarlamalı Suite ile Azure Active Directory arasındaki yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 13af9d00-116a-41b8-8ca0-4870b31e224c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: 8e128ddf53a93fe30350d8e914657f3539701603
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adaptive-suite"></a>Öğretici: Azure Active Directory Tümleştirme Uyarlamalı Suite ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Uyarlamalı Suite tümleştirmek öğrenin.

Uyarlamalı Suite Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Uyarlamalı Suite erişimi, Azure AD'de kontrol edebilirsiniz
- Azure AD hesaplarına otomatik olarak Uyarlamalı paketine (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Uyarlamalı paketiyle yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Uyarlamalı Suite çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Uyarlamalı paketi ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-adaptive-suite-from-the-gallery"></a>Galeriden Uyarlamalı paketi ekleme
Azure AD Uyarlamalı Suite tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Uyarlamalı paketi eklemeniz gerekir.

**Galeriden Uyarlamalı paketi eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Uyarlamalı Suite**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_search.png)

5. Sonuçlar panelinde seçin **Uyarlamalı Suite**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Uyarlamalı "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı Suite ile test etme

Tekli çalışmaya oturum için Azure AD ne karşılık gelen Uyarlamalı grubundaki bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Uyarlamalı paketindeki ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Uyarlamalı paketindeki değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Uyarlamalı Suite ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Uyarlamalı Suite test kullanıcısı oluşturma](#creating-an-adaptive-suite-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Uyarlamalı Suite sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Uyarlamalı Suite uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Uyarlamalı paketiyle yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Uyarlamalı Suite** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_samlbase.png)

3. Üzerinde **Uyarlamalı Suite etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_url.png)

    İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://login.adaptiveinsights.com:443/samlsso/<unique-id>`

    >[!NOTE]
    > Bu değer Uyarlamalı paketinden 's alabilir **SAML SSO ayarları** sayfası.
    >  

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_400.png)

6. Üzerinde **Uyarlamalı paketi yapılandırma** 'yi tıklatın **yapılandırma Uyarlamalı Suite** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_configure.png) 

7. Farklı web tarayıcısı penceresinde Uyarlamalı Suite şirket sitenize yönetici olarak oturum açın.

8. Git **yönetici**.
   
    ![Yönetici](./media/active-directory-saas-adaptivesuite-tutorial/IC805644.png "yönetici")

9. İçinde **kullanıcılar ve roller** 'yi tıklatın **SAML SSO ayarları yönetme**.
   
    ![SAML SSO ayarlarını Yönet](./media/active-directory-saas-adaptivesuite-tutorial/IC805645.png "SAML SSO ayarlarını yönet")

10. Üzerinde **SAML SSO ayarları** sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![SAML SSO ayarları](./media/active-directory-saas-adaptivesuite-tutorial/IC805646.png "SAML SSO ayarları")

    a. İçinde **kimlik sağlayıcı adı** metin kutusuna, yapılandırmanız için bir ad yazın.
    
    b. Yapıştır **SAML varlık kimliği** değeri kopyalanan Azure portalından **kimlik sağlayıcısı varlık kimliği** metin kutusu.
  
    c. Yapıştır **SAML çoklu oturum açma hizmet URL'si** değeri kopyalanan Azure portalından **kimlik sağlayıcısı SSO URL** metin kutusu.
  
    d. Yapıştır **SAML çoklu oturum açma hizmet URL'si** değeri kopyalanan Azure portalından **özel oturum kapatma URL'si** metin kutusu.
  
    e. İndirilen sertifikanızı karşıya yüklemek için tıklayın **dosya**.
  
    f. İçin aşağıdakileri seçin:
    * **SAML kullanıcı kimliği**seçin **Uyarlamalı Öngörüler kullanıcının adını**.
    * **SAML kullanıcı kimliği konumu**seçin **NameID, konu kullanıcı kimliği**.
    * **SAML NameID biçimi**seçin **e-posta adresi**.
    * **SAML'yi etkinleştir**seçin **izin SAML SSO ve doğrudan Uyarlamalı Öngörüler oturum açma**.
    
    g. **Kaydet** düğmesine tıklayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-adaptivesuite-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-adaptivesuite-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-adaptivesuite-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-adaptivesuite-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="creating-an-adaptive-suite-test-user"></a>Uyarlamalı Suite test kullanıcısı oluşturma

Uyarlamalı paketine oturum açmak Azure AD kullanıcıları etkinleştirmek için bunlar Uyarlamalı paketine sağlanmalıdır.  

* Uyarlamalı söz konusu olduğunda, sağlama el ile bir görev paketidir.

**Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:** 

1. Oturum, **Uyarlamalı Suite** yönetici olarak şirket site.
2. Git **yönetici**.
   
   ![Yönetici](./media/active-directory-saas-adaptivesuite-tutorial/IC805644.png "yönetici")
3. İçinde **kullanıcılar ve roller** 'yi tıklatın **Kullanıcı Ekle**.
   
   ![Kullanıcı ekleme](./media/active-directory-saas-adaptivesuite-tutorial/IC805648.png "kullanıcı ekleme")
4. İçinde **yeni kullanıcı** bölümünde, aşağıdaki adımları gerçekleştirin:
   
   ![Gönderme](./media/active-directory-saas-adaptivesuite-tutorial/IC805649.png "Gönder")   

   a. Tür **adı**, **oturum açma**, **e-posta**, **parola** istediğiniz ilgili metin kutularına sağlamayı geçerli bir Azure Active Directory kullanıcı.
  
   b. Seçin bir **rol**.
  
   c. Tıklatın **gönderme**.

>[!NOTE]
>API sağlama AAD kullanıcı hesaplarına Uyarlamalı paketi tarafından sağlanan veya herhangi diğer Uyarlamalı Suite kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz.
>  

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Uyarlamalı paketine erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı atama][200] 

**Uyarlamalı paketine Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Uyarlamalı Suite**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümün amacı erişim paneli kullanılarak Microsoft Azure AD çoklu oturum açma yapılandırmanızı test sağlamaktır.

Erişim paneli Uyarlamalı Suite parçasında tıklattığınızda, otomatik olarak Uyarlamalı Suite uygulamanıza açan.


## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_203.png

