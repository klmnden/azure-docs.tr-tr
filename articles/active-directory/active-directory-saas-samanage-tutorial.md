---
title: "Öğretici: Azure Active Directory Tümleştirme ile Samanage | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile Samanage arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: f0db4fb0-7eec-48c2-9c7a-beab1ab49bc2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: 3236c12af214c7d27f1ac835b654b36819b5e80b
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-samanage"></a>Öğretici: Azure Active Directory Tümleştirme Samanage ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Samanage tümleştirmek öğrenin.

Samanage Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Samanage erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için Samanage (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Samanage ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Samanage çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Samanage ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-samanage-from-the-gallery"></a>Galeriden Samanage ekleme
Azure AD Samanage tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Samanage eklemeniz gerekir.

**Galeriden Samanage eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Samanage**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_search.png)

5. Sonuçlar panelinde seçin **Samanage**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Samanage sınayın.

Tekli çalışmaya oturum için Azure AD Samanage karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Samanage ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Samanage içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Samanage ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Samanage test kullanıcısı oluşturma](#creating-a-samanage-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Samanage sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Samanage uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Samanage yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Samanage** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_samlbase.png)

3. Üzerinde **Samanage etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<Company Name>.samanage.com/saml_login/<Company Name>`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<Company Name>.samanage.com`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Öğreticide daha sonra açıklanan tanımlayıcısı ve gerçek oturum açma URL'si ile bu değerleri güncelleştirin. Daha fazla ayrıntı başvurun [Samanage istemci destek ekibi](https://www.samanage.com/support).    
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-samanage-tutorial/tutorial_general_400.png)

6. Üzerinde **Samanage yapılandırma** 'yi tıklatın **yapılandırma Samanage** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL ve SAML varlık kimliği** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_configure.png) 

7. Farklı web tarayıcısı penceresinde Samanage şirket sitenize yönetici olarak oturum açın.

8. Tıklatın **Pano** seçip **Kurulum** sol gezinti bölmesindeki.
   
    ![Pano](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_001.png "Panosu")

9. Tıklatın **çoklu oturum açma**.
   
    ![Çoklu oturum açma](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_002.png "çoklu oturum açma")

10. Gidin **SAML kullanarak oturum açma** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![SAML kullanarak oturum açma](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_003.png "SAML ile oturum açın")
 
    a. Tıklatın **çoklu oturum açmayı etkinleştir SAML ile**.  
 
    b. İçinde **kimlik sağlayıcısı URL'si** metin değerini yapıştırın **SAML varlık kimliği** Azure portalından kopyalanan.    
 
    c. Onayla **oturum açma URL'si** eşleşen **oturum üzerinde URL'si** , **Samanage etki alanı ve URL'leri** Azure portalı bölümünde.
 
    d. İçinde **oturum kapatma URL'si** metin değeri girin **Sign-Out URL** Azure portalından kopyalanan.
 
    e. İçinde **SAML veren** uygulama kimliği URI metin kutusuna, türü, kimlik sağlayıcısı ayarlayın.
 
    f. Not Defteri'nde Azure portalından indirdiğiniz, base-64 kodlanmış sertifika açın, içeriğini, panoya kopyalayın ve yapıştırın kendisine **, kimlik sağlayıcısı x.509 sertifika aşağıdaki Yapıştır** metin kutusu.
 
    g. Tıklatın **Samanage içinde yoksa, kullanıcılar oluşturma**.
 
    h. Tıklatın **güncelleştirme**.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
 
### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-samanage-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-samanage-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-samanage-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-samanage-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="creating-a-samanage-test-user"></a>Samanage test kullanıcısı oluşturma

Azure AD kullanıcıları için Samanage oturum açmak etkinleştirmek için bunların Samanage sağlanmalıdır.  
Samanage söz konusu olduğunda, sağlama bir el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Samanage şirket sitenize yönetici olarak oturum açın.

2. Tıklatın **Pano** seçip **Kurulum** sol gezinti bölmesinde.
   
    ![Kurulum](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_001.png "Kurulumu")

3. Tıklatın **kullanıcılar** sekmesi
   
    ![Kullanıcıların](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_006.png "kullanıcılar")

4. Tıklatın **yeni kullanıcı**.
   
    ![Yeni kullanıcı](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_007.png "yeni kullanıcı")

5. Tür **adı** ve **e-posta adresi** , önce sağlamak istediğiniz bir Azure Active Directory hesabının **kullanıcı oluşturma**.
   
    ![Kullanıcı oluşturma](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_008.png "kullanıcı oluştur")
   
   >[!NOTE]
   >Azure Active Directory hesap sahibi bir e-posta alır ve onu etkinleştirilmeden önce kendi hesabı onaylamak için bağlantıyı izleyin. API tarafından Samanage sağlamak için Azure Active Directory kullanıcı hesapları sağlanan veya herhangi diğer Samanage kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Samanage için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Samanage için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Samanage**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Samanage parçasında tıklattığınızda, otomatik olarak Samanage uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_203.png

