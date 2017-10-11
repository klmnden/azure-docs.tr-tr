---
title: "Öğretici: Azure Active Directory Tümleştirme ile Syncplicity | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile Syncplicity arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 896a3211-f368-46d7-95b8-e4768c23be08
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 1321fa71bcd625d6ea754432bfb402d3919e38f3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-syncplicity"></a>Öğretici: Syncplicity Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Syncplicity tümleştirmek öğrenin.

Syncplicity Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Syncplicity erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için Syncplicity (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Syncplicity ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Syncplicity çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Syncplicity Galeriden ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-syncplicity-from-the-gallery"></a>Syncplicity Galeriden ekleme
Azure AD Syncplicity tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Syncplicity eklemeniz gerekir.

**Syncplicity Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Syncplicity**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_search.png)

5. Sonuçlar panelinde seçin **Syncplicity**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı Syncplicity ile test etme

Tekli çalışmaya oturum için Azure AD Syncplicity karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Syncplicity ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Syncplicity içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Syncplicity ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Syncplicity test kullanıcısı oluşturma](#creating-a-syncplicity-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Syncplicity sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Syncplicity uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Syncplicity yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Syncplicity** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_samlbase.png)

3. Üzerinde **Syncplicity etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<companyname>.syncplicity.com`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<companyname>.syncplicity.com/sp`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kişi [Syncplicity istemci destek ekibi](https://www.syncplicity.com/contact-us) bu değerleri almak için. 
 

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_certificate.png) 

  
5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-syncplicity-tutorial/tutorial_general_400.png)

6. Üzerinde **Syncplicity yapılandırma** 'yi tıklatın **yapılandırma Syncplicity** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_configure.png) 

7. Oturum açın, **Syncplicity** Kiracı.

8. Üstteki menüde tıklatın **yönetici**seçin **ayarları**ve ardından **özel etki alanı ve çoklu oturum açma**.
   
    ![Syncplicity](./media/active-directory-saas-syncplicity-tutorial/ic769545.png "Syncplicity")

9. Üzerinde **çoklu oturum açma (SSO)** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açma \(SSO\)](./media/active-directory-saas-syncplicity-tutorial/ic769550.png "Single Sign-On \\\(SSO\\\)")   

    a. İçinde **özel etki alanı** metin kutusuna, etki alanınızın adını yazın.
  
    b. Seçin **etkin** olarak **tek oturum açma durumu**.

    c. İçinde **varlık kimliği** metin değerini yapıştırın **SAML varlık kimliği** Azure portalından kopyalanan.

    d. İçinde **oturum açma sayfası URL'si** metin kutusuna, Yapıştır **SAML çoklu oturum açma hizmet URL'si** Azure portalından kopyalanan.

    e. İçinde **oturum kapatma sayfası URL'si** metin kutusuna, Yapıştır **Sign-Out URL** Azure portalından kopyalanan.

    f. İçinde **kimlik sağlayıcısı sertifikası**, tıklatın **Dosya Seç**ve Azure Portalı'ndan indirilen sertifika yükleyin. 

    g. Tıklatın **değişiklikleri kaydetme**.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-syncplicity-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-syncplicity-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-syncplicity-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-syncplicity-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="creating-a-syncplicity-test-user"></a>Syncplicity test kullanıcısı oluşturma
AAD kullanıcıların oturum açabilmesi için bunlar Syncplicity uygulama sağlanmalıdır. Bu bölümde, AAD kullanıcı hesaplarının Syncplicity içinde nasıl oluşturulacağı açıklanmaktadır.

**Bir kullanıcı hesabına Syncplicity sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum, **Syncplicity** Kiracı (örneğin: `https://company.Syncplicity.com`).

2. Tıklatın **yönetici** seçip **kullanıcı hesaplarını**.

3. Tıklatın **kullanıcı ekleme**.
   
    ![Kullanıcıları yönetme](./media/active-directory-saas-syncplicity-tutorial/ic769764.png "kullanıcıları yönetme")

4. Tür **e-posta addressess** sağlamak istediğiniz bir AAD hesabıyla seçin **kullanıcı** olarak **rol**ve ardından **sonraki**.
   
    ![Hesap bilgileri](./media/active-directory-saas-syncplicity-tutorial/ic769765.png "hesap bilgileri")
   
    >[!NOTE]
    >AAD hesap sahibi onaylayın ve hesabını etkinleştirmek için bir bağlantı içeren bir e-posta alır. 
    > 

5. Yeni kullanıcı bir üyesi haline gelir ve ardından, şirketinizde bir grup seçin **sonraki**.
   
    ![Grup üyelikleri](./media/active-directory-saas-syncplicity-tutorial/ic769772.png "grup üyelikleri")
   
    >[!NOTE]
    >Listelenen hiçbir grup varsa tıklatın **sonraki**. 
    > 

6. Kullanıcının bilgisayarında Syncplicity'nın denetimindeki yerleştirin ve ardından istediğiniz klasörleri seçin **sonraki**.
   
    ![Syncplicity klasörleri](./media/active-directory-saas-syncplicity-tutorial/ic769773.png "Syncplicity klasörleri")

>[!NOTE]
>API sağlama AAD kullanıcı hesaplarına Syncplicity tarafından sağlanan veya herhangi diğer Syncplicity kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz. 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Syncplicity için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Syncplicity için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Syncplicity**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümün amacı erişim paneli kullanılarak Azure AD çoklu oturum açma yapılandırmanızı test etmektir.

Erişim paneli Syncplicity parçasında tıklattığınızda, otomatik olarak Syncplicity uygulamanıza açan.
## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_203.png

