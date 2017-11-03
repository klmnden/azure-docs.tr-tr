---
title: "Öğretici: Azure Active Directory Tümleştirme ile Kintone | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile Kintone arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c2b947dc-e1a8-4f5f-b40e-2c5180648e4f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jeedes
ms.openlocfilehash: e5e847c12cba3611ce7ea2c3e956dbd55b1e0cac
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-kintone"></a>Öğretici: Azure Active Directory Tümleştirme Kintone ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Kintone tümleştirmek öğrenin.

Kintone Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Kintone erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için Kintone (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Kintone ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Kintone çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Kintone ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-kintone-from-the-gallery"></a>Galeriden Kintone ekleme
Azure AD Kintone tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Kintone eklemeniz gerekir.

**Galeriden Kintone eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Kintone**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-kintone-tutorial/tutorial_kintone_search.png)

5. Sonuçlar panelinde seçin **Kintone**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-kintone-tutorial/tutorial_kintone_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Kintone sınayın.

Tekli çalışmaya oturum için Azure AD Kintone karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Kintone ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Kintone içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Kintone ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Kintone test kullanıcısı oluşturma](#creating-a-kintone-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Kintone sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Kintone uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Kintone yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Kintone** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-kintone-tutorial/tutorial_kintone_samlbase.png)

3. Üzerinde **Kintone etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-kintone-tutorial/tutorial_kintone_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<companyname>.kintone.com`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın:
    | |
    |--|
    | `https://<companyname>.cybozu.com`|
    | `https://<companyname>.kintone.com`|

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kişi [Kintone istemci destek ekibi](https://www.kintone.com/contact/) bu değerleri almak için. 
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-kintone-tutorial/tutorial_kintone_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-kintone-tutorial/tutorial_general_400.png)

6. Üzerinde **Kintone yapılandırma** 'yi tıklatın **yapılandırma Kintone** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-kintone-tutorial/tutorial_kintone_configure.png) 

7. Farklı web tarayıcısı penceresinde oturum açın, **Kintone** yönetici olarak şirket site.

8. Tıklatın **ayarları**.
   
    ![Ayarları](./media/active-directory-saas-kintone-tutorial/ic785879.png "ayarları")

9. Tıklatın **kullanıcılar ve sistem yönetimini**.
   
    ![Kullanıcılar ve sistem yönetimini](./media/active-directory-saas-kintone-tutorial/ic785880.png "kullanıcılar ve Sistem Yönetimi")

10. Altında **Sistem Yönetimi \> güvenlik** tıklatın **oturum açma**.
   
    ![Oturum açma](./media/active-directory-saas-kintone-tutorial/ic785881.png "oturum açma")

11. Tıklatın **etkinleştirmek SAML kimlik doğrulaması**.
   
    ![SAML kimlik doğrulaması](./media/active-directory-saas-kintone-tutorial/ic785882.png "SAML kimlik doğrulaması")

12. SAML kimlik doğrulaması bölümünde, aşağıdaki adımları gerçekleştirin:
    
    ![SAML kimlik doğrulaması](./media/active-directory-saas-kintone-tutorial/ic785883.png "SAML kimlik doğrulaması")
    
    a. İçinde **oturum açma URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si** Azure portalından kopyalanan.
   
    b. İçinde **oturum kapatma URL'si** metin değerini yapıştırın **Sign-Out URL** Azure portalından kopyalanan.
    
    c. Tıklatın **Gözat** indirilen sertifikanızı karşıya yüklemek için.
    
    d. **Kaydet** düğmesine tıklayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-kintone-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-kintone-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-kintone-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-kintone-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="creating-a-kintone-test-user"></a>Kintone test kullanıcısı oluşturma

Azure AD kullanıcıları için Kintone oturum açmak etkinleştirmek için bunların Kintone sağlanmalıdır.  
Kintone söz konusu olduğunda, sağlama bir el ile bir görevdir.

### <a name="to-provision-a-user-account-perform-the-following-steps"></a>Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:

1. Oturum, **Kintone** yönetici olarak şirket site.

2. Tıklatın **ayarı**.
   
    ![Ayarları](./media/active-directory-saas-kintone-tutorial/ic785879.png "ayarları")

3. Tıklatın **kullanıcılar ve sistem yönetimini**.
   
    ![Kullanıcı & Sistem Yönetimi](./media/active-directory-saas-kintone-tutorial/ic785880.png "kullanıcı & Sistem Yönetimi")

4. Altında **kullanıcı yönetimi**, tıklatın **Departmanlar k & ullanıcıların**.
   
    ![K & ullanıcıların departmanı](./media/active-directory-saas-kintone-tutorial/ic785888.png "departmanı ve kullanıcılar")

5. Tıklatın **yeni kullanıcı**.
   
    ![Yeni kullanıcılar](./media/active-directory-saas-kintone-tutorial/ic785889.png "yeni kullanıcılar")

6. İçinde **yeni kullanıcı** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Yeni kullanıcılar](./media/active-directory-saas-kintone-tutorial/ic785890.png "yeni kullanıcılar")
   
    a. Tür a **görünen adı**, **oturum açma adı**, **yeni parola**, **parolayı onaylayın**, **e-posta adresi**, ve diğer ayrıntılarını geçerli bir AAD hesabıyla ilgili metin kutularına sağlamayı istiyor.
 
    b. **Kaydet** düğmesine tıklayın.

> [!NOTE]
> API sağlama AAD kullanıcı hesaplarına Kintone tarafından sağlanan veya herhangi diğer Kintone kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Kintone için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Kintone için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Kintone**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-kintone-tutorial/tutorial_kintone_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümün amacı erişim paneli kullanılarak Azure AD çoklu oturum açma yapılandırmanızı test etmektir.

Erişim paneli Kintone parçasında tıklattığınızda, otomatik olarak Kintone uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_203.png

