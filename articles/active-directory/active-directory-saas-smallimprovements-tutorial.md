---
title: 'Öğretici: Azure Active Directory Tümleştirme küçük geliştirmelerle | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ve küçük geliştirmeler arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 59c8a112-41e1-4337-9ef3-3d7029780d61
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 2f6cab5dd7c10e4036cdd2013c809142bf7ec846
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-small-improvements"></a>Öğretici: Azure Active Directory Tümleştirme küçük geliştirmelerle

Bu öğreticide, küçük geliştirmeler Azure Active Directory (Azure AD) ile tümleştirme öğrenin.

Küçük geliştirmeler Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Küçük geliştirmeleri erişimi, Azure AD'de kontrol edebilirsiniz
- Azure AD hesaplarına otomatik olarak küçük geliştirmeleri (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme küçük geliştirmelerle yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir küçük geliştirmeleri çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, burada bir aylık deneme alabilirsiniz [deneme teklifi](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden küçük geliştirmeler ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-small-improvements-from-the-gallery"></a>Galeriden küçük geliştirmeler ekleme
Azure AD Küçük geliştirmeleri tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden küçük geliştirmeleri eklemeniz gerekir.

**Galeriden küçük geliştirmeleri eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **küçük geliştirmeleri**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_search.png)

5. Sonuçlar panelinde seçin **küçük geliştirmeleri**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırmak ve küçük "Britta Simon" adlı bir test kullanıcı doğrultusunda geliştirmeler Azure AD çoklu oturum açmayı sınayın.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen küçük geliştirmeleri bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı küçük geliştirmeleri arasında bir bağlantı ilişkisi kurulması gerekir.

Küçük yenilikleri değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırmak ve Azure AD çoklu oturum açma küçük geliştirmelerle sınamak için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Küçük geliştirmeleri test kullanıcısı oluşturma](#creating-a-small-improvements-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı küçük geliştirmeleri sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma küçük geliştirmeleri uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma küçük geliştirmelerle yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **küçük geliştirmeleri** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_samlbase.png)

3. Üzerinde **küçük geliştirmeler etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<subdomain>.small-improvements.com`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<subdomain>.small-improvements.com`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kişi [küçük geliştirmeleri istemci destek ekibi](mailto:support@small-improvements.com) bu değerleri almak için. 
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_400.png)

6. Üzerinde **küçük geliştirmeleri yapılandırma** 'yi tıklatın **yapılandırma küçük geliştirmeleri** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_configure.png) 

7. Başka bir tarayıcı penceresinde küçük geliştirmeleri şirket sitenize yönetici olarak oturum açma.

8. Ana Panodaki sayfasından tıklatın **Yönetim** sol düğmesi.
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_06.png) 

9. Tıklatın **SAML SSO** gelen düğmesini **tümleştirmeler** bölümü.
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_07.png) 

10. SSO Kurulumu sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_08.png)  

    a. İçinde **HTTP uç noktası** metin değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si**, Azure portalından kopyalanan.

    b. İndirilen sertifikanızı Not Defteri'nde açın, içeriği Kopyala ve ardından yapıştırın **x509 sertifika** metin kutusu. 

    c. SSO ve oturum açma form kimlik doğrulaması seçeneği kullanıcılar için kullanılabilir olmasını istiyorsanız, ardından denetleyin **oturum açma/parola ile çok erişmesini** seçeneği.  

    d. SSO oturum açma düğmesini adlandırmak için uygun değeri girin **SAML komut istemi** metin kutusu.  

    e. **Kaydet**’e tıklayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-smallimprovements-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-smallimprovements-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-smallimprovements-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-smallimprovements-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-small-improvements-test-user"></a>Küçük geliştirmeleri test kullanıcısı oluşturma

Küçük geliştirmeleri oturum açmak Azure AD kullanıcıları etkinleştirmek için bunlar küçük artışlarını sağlanmalıdır. Küçük geliştirmeleri söz konusu olduğunda, sağlama bir el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Küçük geliştirmeleri şirket sitenize yönetici olarak oturum.

2. Giriş sayfasından sol,'ı tıklatın menüsüne gidin **Yönetim**.

3. Tıklatın **kullanıcı dizini** kullanıcı yönetimi bölümünden düğmesi. 
   
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_10.png) 

4. Tıklatın **kullanıcıları eklemek**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_11.png) 

5. Üzerinde **Kullanıcı Ekle** iletişim kutusunda, aşağıdaki adımları gerçekleştirin: 

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_12.png)
    
    a. Girin **ad** gibi kullanıcının **Britta**.

    b. Girin **Soyadı** gibi kullanıcının **Simon**.

    c. Girin **e-posta** gibi kullanıcının **brittasimon@contoso.com**. 

    d. Ayrıca kişisel iletisinde girmeyi seçebilirsiniz **bildirim e-posta Gönder** kutusu. Bildirim göndermek istemiyorsanız bu onay kutusunun işaretini kaldırın.

    e. Tıklatın **kullanıcılar oluşturma**.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta küçük geliştirmeleri erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Küçük geliştirmeleri Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **küçük geliştirmeleri**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümün amacı erişim paneli kullanılarak Azure AD SSO yapılandırmanızı test etmektir.  

Erişim paneli küçük geliştirmeleri parçasında tıklattığınızda, otomatik olarak küçük geliştirmeleri uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_203.png

