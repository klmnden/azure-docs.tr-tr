---
title: "Öğretici: Azure Active Directory Tümleştirme ile NetDocuments | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile NetDocuments arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 1a47dc42-1a17-48a2-965e-eca4cfb2f197
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: ec0010eb3676a5a446bb88ade643553aa62f9aa4
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-netdocuments"></a>Öğretici: Azure Active Directory Tümleştirme NetDocuments ile

Bu öğreticide, Azure Active Directory (Azure AD) ile NetDocuments tümleştirmek öğrenin.

NetDocuments Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- NetDocuments erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için NetDocuments (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme NetDocuments ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir NetDocuments çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden NetDocuments ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-netdocuments-from-the-gallery"></a>Galeriden NetDocuments ekleme
Azure AD NetDocuments tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden NetDocuments eklemeniz gerekir.

**Galeriden NetDocuments eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **NetDocuments**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-netdocuments-tutorial/tutorial_netdocuments_search.png)

5. Sonuçlar panelinde seçin **NetDocuments**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-netdocuments-tutorial/tutorial_netdocuments_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı NetDocuments sınayın.

Tekli çalışmaya oturum için Azure AD NetDocuments karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının NetDocuments ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

NetDocuments içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma NetDocuments ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[NetDocuments test kullanıcısı oluşturma](#creating-a-netdocuments-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı NetDocuments sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma NetDocuments uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile NetDocuments yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **NetDocuments** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-netdocuments-tutorial/tutorial_netdocuments_samlbase.png)

3. Üzerinde **NetDocuments etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-netdocuments-tutorial/tutorial_netdocuments_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=<user identifier>`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=<user identifier>`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve yanıt URL'si ile güncelleştirin. Kişi [NetDocuments destek ekibi](https://support.netdocuments.com/hc/) bu değerleri almak için.
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-netdocuments-tutorial/tutorial_netdocuments_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-netdocuments-tutorial/tutorial_general_400.png)

6. Farklı web tarayıcısı penceresinde NetDocuments şirket sitenize yönetici olarak oturum açın.

7. Git **yönetici**.

8. Tıklatın **ekleme ve kaldırma kullanıcılar ve gruplar**.
   
    ![Depo](./media/active-directory-saas-netdocuments-tutorial/ic795047.png "deposu")

9. Tıklatın **Gelişmiş kimlik doğrulama Seçenekleri Yapılandır**.
    
    ![Gelişmiş kimlik doğrulama Seçenekleri Yapılandır](./media/active-directory-saas-netdocuments-tutorial/ic795048.png "Gelişmiş kimlik doğrulama seçeneklerini yapılandırma")

10. Üzerinde **Federal Kimlik** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
    ![Identitty Federasyon](./media/active-directory-saas-netdocuments-tutorial/ic795049.png "Identitty Federasyon")
   
    a. Olarak **kimlik sunucu türü Federasyon**seçin **Active Directory Federasyon Hizmetleri**.
   
    b. Tıklatın **dosya**, Azure Portalı'ndan indirilen indirilen meta veri dosyasını karşıya yüklemek için.
   
    c. **Tamam** düğmesine tıklayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-netdocuments-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-netdocuments-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-netdocuments-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-netdocuments-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="creating-a-netdocuments-test-user"></a>NetDocuments test kullanıcısı oluşturma

Azure AD kullanıcıları için NetDocuments oturum açmak etkinleştirmek için bunların NetDocuments sağlanmalıdır.  
NetDocuments söz konusu olduğunda, sağlama bir el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum SING, **NetDocuments** yönetici olarak şirket site.

2. Üstteki menüde tıklatın **yönetici**.
   
    ![Yönetici](./media/active-directory-saas-netdocuments-tutorial/ic795051.png "yönetici")

3. Tıklatın **ekleme ve kaldırma kullanıcılar ve gruplar**.
   
    ![Depo](./media/active-directory-saas-netdocuments-tutorial/ic795047.png "deposu")

4. İçinde **e-posta adresi** metin kutusu, geçerli bir Azure Active Directory hesabı sağlamak istediğiniz ve ardından e-posta adresini yazın **Kullanıcı Ekle**.
   
    ![E-posta adresi](./media/active-directory-saas-netdocuments-tutorial/ic795053.png "e-posta adresi")
   
   >[!NOTE]
   >Azure Active Directory hesap sahibi etkin duruma gelmesi hesabı onaylamak için bir bağlantı içeren bir e-posta alırsınız. API tarafından NetDocuments sağlamak için Azure Active Directory kullanıcı hesapları sağlanan veya herhangi diğer NetDocuments kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta NetDocuments için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**NetDocuments için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **NetDocuments**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-netdocuments-tutorial/tutorial_netdocuments_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli NetDocuments parçasında tıklattığınızda, otomatik olarak NetDocuments uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_203.png

