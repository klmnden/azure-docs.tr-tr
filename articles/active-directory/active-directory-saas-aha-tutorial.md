---
title: "Öğretici: Azure Active Directory Tümleştirme ile Aha! | Microsoft Belgeleri"
description: "Çoklu oturum açma Azure Active Directory ile Aha arasında yapılandırmayı öğrenin!."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ad955d3d-896a-41bb-800d-68e8cb5ff48d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: jeedes
ms.openlocfilehash: 7723864b2e1ab2d5b69d86f0fa18416b9d3f9aa3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-aha"></a>Öğretici: Azure Active Directory Tümleştirme ile Aha!

Bu öğreticide, Aha tümleştirmek öğrenin! Azure ile Active Directory (Azure AD).

AHA tümleştirme! Azure AD ile aşağıdaki faydaları sağlar:

- Aha erişimi, Azure AD'de kontrol edebilirsiniz!
- Otomatik olarak Aha için açan kullanıcılarınıza etkinleştirebilirsiniz! (Çoklu oturum açma) Azure AD hesaplarına sahip
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Aha ile yapılandırmak için!, aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Aha! Çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. AHA ekleme! Galeriden
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-aha-from-the-gallery"></a>AHA ekleme! Galeriden
Aha tümleştirmesini yapılandırmak için! Azure AD ile Aha eklemeniz gerekir! Yönetilen SaaS uygulamaları listenize Galeriden.

**AHA eklemek için! Galeriden, aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Aha!**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-aha-tutorial/tutorial_aha_search.png)

5. Sonuçlar panelinde seçin **Aha!**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-aha-tutorial/tutorial_aha_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Aha ile test etme! "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı

Tekli çalışmaya oturum için Azure AD Aha ne karşılık gelen kullanıcı bilmesi gerekir! bir kullanıcı için Azure AD içinde değil. Diğer bir deyişle, bir bağlantı arasındaki ilişki bir Azure AD kullanıcısının Aha ilgili kullanıcı! kurulması gerekir.

Aha,!, değeri atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Aha ile test!, aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Oluşturma bir Aha! test kullanıcısı](#creating-an-aha-test-user)**  - Aha içinde karşılık gelen Britta Simon biri için! Bu kullanıcı Azure AD gösterimini bağlanır.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirme ve çoklu oturum açma, Aha yapılandırma! Uygulama.

**Azure AD çoklu oturum açma ile Aha yapılandırmak için!, aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Aha!** Uygulama Tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-aha-tutorial/tutorial_aha_samlbase.png)

3. Üzerinde **Aha! Etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-aha-tutorial/tutorial_aha_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<companyname>.aha.io/session/new`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<companyname>.aha.io`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kişi [Aha! İstemci destek ekibi](https://www.aha.io/company/contact) bu değerleri almak için. 
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-aha-tutorial/tutorial_aha_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-aha-tutorial/tutorial_general_400.png)

6. Farklı web tarayıcısı penceresinde, Aha oturum! Yönetici olarak şirket sitesi.

7. Üstteki menüde tıklatın **ayarları**.

    ![Ayarları](./media/active-directory-saas-aha-tutorial/IC798950.png "ayarları")

8. Tıklatın **hesap**.
   
    ![Profil](./media/active-directory-saas-aha-tutorial/IC798951.png "profili")

9. Tıklatın **güvenlik ve çoklu oturum açma**.
   
    ![Güvenlik ve çoklu oturum açma](./media/active-directory-saas-aha-tutorial/IC798952.png "güvenlik ve çoklu oturum açma")

10. İçinde **çoklu oturum açma** bölümünde olarak **kimlik sağlayıcısı**seçin **SAML2.0**.
   
    ![Güvenlik ve çoklu oturum açma](./media/active-directory-saas-aha-tutorial/IC798953.png "güvenlik ve çoklu oturum açma")

11. Üzerinde **çoklu oturum açma** yapılandırma sayfasında, aşağıdaki adımları gerçekleştirin:
    
    ![Çoklu oturum açma](./media/active-directory-saas-aha-tutorial/IC798954.png "çoklu oturum açma")
    
       a. İçinde **adı** metin kutusuna, yapılandırmanız için bir ad yazın.

       b. İçin **kullanarak yapılandırma**seçin **meta veri dosyası**.
   
       c. İndirilen meta veri dosyanızı karşıya yüklemek için tıklayın **Gözat**.
   
       d. Tıklatın **güncelleştirme**.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-aha-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-aha-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-aha-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-aha-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="creating-an-aha-test-user"></a>Oluşturma bir Aha! Test kullanıcısı

Azure AD kullanıcıların AHA için oturum açma!, Aha sağlanmalıdır!.  

Aha durumunda!, otomatik bir görev olduğundan sağlama. Sizin için eylem öğe yok.

Kullanıcıları otomatik olarak ilk tek oturum açma girişimi sırasında gerekirse oluşturulur.

>[!NOTE]
>Diğer bir Aha kullanabilirsiniz! kullanıcı hesabı oluşturma araçlarını veya Aha tarafından sağlanan API'leri! AAD kullanıcı hesaplarını sağlamak için.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Aha için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirmek!.

![Kullanıcı atama][200] 

**İçin Aha Britta Simon atamak!, aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Aha!**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-aha-tutorial/tutorial_aha_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Çoklu oturum açma ayarlarınızı test etmek isterseniz, erişim paneli açın. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-aha-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-aha-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-aha-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-aha-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-aha-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-aha-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-aha-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-aha-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-aha-tutorial/tutorial_general_203.png

