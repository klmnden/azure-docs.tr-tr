---
title: 'Öğretici: Azure Active Directory Tümleştirme ile QuickHelp | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile QuickHelp arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 655c9ad3-2076-4e2c-8e47-9ed3bf04be56
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2017
ms.author: jeedes
ms.openlocfilehash: 970120b5bc3f3ccadf9f48b1a42e167b5998f1a7
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35982179"
---
# <a name="tutorial-azure-active-directory-integration-with-quickhelp"></a>Öğretici: Azure Active Directory Tümleştirme QuickHelp ile

Bu öğreticide, Azure Active Directory (Azure AD) ile QuickHelp tümleştirmek öğrenin.

QuickHelp Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- QuickHelp erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için QuickHelp (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme QuickHelp ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir QuickHelp çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden QuickHelp ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-quickhelp-from-the-gallery"></a>Galeriden QuickHelp ekleme
Azure AD QuickHelp tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden QuickHelp eklemeniz gerekir.

**Galeriden QuickHelp eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **QuickHelp**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/quickhelp-tutorial/tutorial_quickhelp_search.png)

5. Sonuçlar panelinde seçin **QuickHelp**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/quickhelp-tutorial/tutorial_quickhelp_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı QuickHelp sınayın.

Tekli çalışmaya oturum için Azure AD QuickHelp karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının QuickHelp ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

QuickHelp içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma QuickHelp ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[QuickHelp test kullanıcısı oluşturma](#creating-a-quickhelp-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı QuickHelp sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma QuickHelp uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile QuickHelp yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **QuickHelp** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/quickhelp-tutorial/tutorial_quickhelp_samlbase.png)

3. Üzerinde **QuickHelp etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/quickhelp-tutorial/tutorial_quickhelp_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://quickhelp.com/<ROUTEURL>`

    b. İçinde **tanımlayıcısı** metin kutusuna, bir URL yazın: `https://auth.quickhelp.com`

    > [!NOTE] 
    > Oturum açma URL'si değeri gerçek değil. Değerin gerçek oturum açma URL'si ile güncelleştirin. Değer almak için kuruluşunuzun QuickHelp yöneticinizle veya beyin fırtınasını istemci başarı yöneticinize başvurun.
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/quickhelp-tutorial/tutorial_quickhelp_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/quickhelp-tutorial/tutorial_general_400.png) 

6. QuickHelp şirket sitenize yönetici olarak oturum.

7. Üstteki menüde tıklatın **yönetici**.
   
    ![Çoklu oturum açmayı yapılandırın][21]

8. İçinde **QuickHelp yönetici** menüsünde tıklatın **ayarları**.
   
    ![Çoklu oturum açmayı yapılandırın][22]

9. Tıklatın **kimlik doğrulama ayarlarını**.

10. Üzerinde **kimlik doğrulama ayarlarını** sayfasında, aşağıdaki adımları uygulayın
   
    ![Çoklu oturum açmayı yapılandırın][23]
   
    a. Olarak **SSO türü**seçin **WSFederation**.
   
    b. İndirilen Azure meta veri dosyanızı karşıya yüklemek için tıklayın **Gözat**, dosyasına gidin, end'ye tıklayın **meta veriler karşıya**.
   
    c. İçinde **e-posta** metin kutusuna, türü `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.
   
    d. İçinde **ad** metin kutusuna, `type http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.
   
    e. İçinde **Soyadı** metin kutusuna, `type http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.
   
    f. İçinde **Eylem çubuğu**, tıklatın **kaydetmek**.

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/quickhelp-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/quickhelp-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/quickhelp-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/quickhelp-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-quickhelp-test-user"></a>QuickHelp test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon içinde QuickHelp adlı bir kullanıcı oluşturmaktır.
Tekli çalışmaya oturum için Azure AD ne QuickHelp Azure AD'de bir kullanıcıya karşılık gelen kullanıcı olduğunu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının QuickHelp ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yalnızca zaman sağlama QuickHelp destekler. Bunun anlamı, gerekirse, bir kullanıcı hesabı QuickHelp içinde otomatik olarak oluşturulan ve hesap Azure AD hesabına bağlıdır.

Bu bölümde, eylem öğe yok.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta QuickHelp için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**QuickHelp için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **QuickHelp**.

    ![Çoklu oturum açmayı yapılandırın](./media/quickhelp-tutorial/tutorial_quickhelp_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümün amacı erişim paneli kullanılarak Azure AD çoklu oturum açma yapılandırmanızı test etmektir.  

Erişim paneli QuickHelp parçasında tıklattığınızda, otomatik olarak QuickHelp uygulamanıza açan.


## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/quickhelp-tutorial/tutorial_general_01.png
[2]: ./media/quickhelp-tutorial/tutorial_general_02.png
[3]: ./media/quickhelp-tutorial/tutorial_general_03.png
[4]: ./media/quickhelp-tutorial/tutorial_general_04.png

[100]: ./media/quickhelp-tutorial/tutorial_general_100.png

[200]: ./media/quickhelp-tutorial/tutorial_general_200.png
[201]: ./media/quickhelp-tutorial/tutorial_general_201.png
[202]: ./media/quickhelp-tutorial/tutorial_general_202.png
[203]: ./media/quickhelp-tutorial/tutorial_general_203.png
[21]: ./media/quickhelp-tutorial/tutorial_quickhelp_05.png
[22]: ./media/quickhelp-tutorial/tutorial_quickhelp_06.png
[23]: ./media/quickhelp-tutorial/tutorial_quickhelp_07.png
