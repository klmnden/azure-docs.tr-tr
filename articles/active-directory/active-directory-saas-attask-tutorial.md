---
title: "Öğretici: Azure Active Directory Tümleştirme ile @Task| Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory arasında yapılandırmayı öğrenin ve @Task."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: aab8bd2f-f9dd-42da-a18e-d707865687d7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2017
ms.author: jeedes
ms.openlocfilehash: ebb19ca6cbaf04106fbce937d95651e709854cfd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-task"></a>Öğretici: Azure Active Directory ile tümleştirme@Task
Bu öğreticinin amacı nasıl tümleştirileceği Göster sağlamaktır @Task Azure Active Directory'ye (Azure AD).  
Tümleştirme @Task Azure AD ile aşağıdaki faydaları sağlar: 

* Erişebilen Azure AD'de kontrol edebilirsiniz@Task
* Oturum açma için otomatik olarak almak, kullanıcılarınızın etkinleştirebilirsiniz @Task (çoklu oturum açma) Azure AD hesaplarına sahip
* Hesaplarınızı bir merkezi konumda - Klasik Azure portalı Yönet

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar
Azure AD ile tümleştirme yapılandırmak için @Task, aşağıdaki öğeleri gerekir:

* Bir Azure AD aboneliği
* Bir @Task çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.
> 
> 

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

* Bu gerekli olmadığı sürece, üretim ortamınızın kullanmamanız gerekir.
* Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/). 

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticinin amacı, Azure AD çoklu oturum açma bir test ortamında test etmenizi hale getirmektir.  
Bu öğreticide verilen senaryoda üç ana yapı taşlarını oluşur:

1. Ekleme @Task galerisinden 
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-task-from-the-gallery"></a>Ekleme @Task galerisinden
Tümleştirmesini yapılandırmak için @Task eklemenize gerek Azure AD ile @Task yönetilen SaaS uygulamaları listenize galerisinden.

**Eklenecek @Task Galeriden, aşağıdaki adımları gerçekleştirin:**

1. İçinde **Klasik Azure portalı**, sol gezinti bölmesinde tıklatın **Active Directory**. 
   
    ![Active Directory][1] 
2. Gelen **Directory** listesinde, directory tümleştirmesini etkinleştirmek istediğiniz dizini seçin.
3. Dizin görünümünde uygulamaları görünümü açmak için **uygulamaları** üst menüde.
   
    ![Uygulamalar][2] 
4. Tıklatın **Ekle** sayfanın sonundaki.
   
    ![Uygulamalar][3] 
5. Üzerinde **ne yapmak istiyorsunuz** iletişim kutusunda, tıklatın **Galeriden bir uygulama eklemek**.
   
    ![Uygulamalar][4] 
6. Arama kutusuna  **@Task** .
   
    ![Uygulamalar][5] 
7. Sonuçlar bölmesinde seçin  **@Task** ve ardından **tam** uygulama eklemek için.
   
    ![Uygulamalar][30] 

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Nasıl yapılandırılacağı ve Azure AD çoklu oturum açma ile test göstermek için bu bölümün amacı olan @Task "Britta Simon" adlı bir test kullanıcı tabanlı.

Tekli çalışmaya oturum için Azure AD içinde karşılık gelen kullanıcının bilmek ister @Task bir kullanıcı Azure AD'de sağlamaktır. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı arasında bir bağlantı ilişki @Task kurulması gerekir.   
Bu bağlantı değeri atayarak ilişkisi **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** içinde @Task.

Yapılandırma ve Azure AD çoklu oturum açma ile test etmek için @Task, aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Oluşturma bir @Tasktest kullanıcı](#creating-a-halogen-software-test-user)**  - Britta Simon, karşılık gelen olmasını @Taskthat her, Azure AD gösterimine bağlandı.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma
Azure AD çoklu oturum açma Klasik Azure portalındaki etkinleştirmek ve çoklu oturum açma içinde yapılandırmak için bu bölümün amacı olan, @Task uygulama.

**Azure AD çoklu oturum açma ile yapılandırmak için @Task, aşağıdaki adımları gerçekleştirin:**

1. Azure Klasik portalında üzerinde  **@Task**  uygulama tümleştirmesi sayfasında, tıklatın **çoklu oturum açma özelliğini yapılandırın** açmak için **yapılandırma çoklu oturum açma** iletişim.
   
    ![Çoklu oturum açmayı yapılandırın][6] 
2. Üzerinde **oturum kullanıcılara nasıl istiyorsunuz @Task**  sayfasında, **Azure AD çoklu oturum açma**ve ardından **sonraki**.
   
    ![Azure AD çoklu oturum açma][7] 
3. Üzerinde **uygulama ayarlarını yapılandır** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Uygulama ayarlarını yapılandırın][8] 
   
     a. İçinde **oturum üzerinde URL'si** metin kutusuna, türü URL'yi, kullanıcılarınıza oturum açma tarafından kullanılan, @Task uygulama (örn:*https://<Tenant name>.attask ondemand.com*).
   
     b. **İleri**’ye tıklayın.
4. Üzerinde **çoklu oturum açma sırasında yapılandırma @Task**  sayfasında, **karşıdan meta veri**, yerel olarak bilgisayarınızda meta veri dosyasını kaydedin ve ardından **sonraki**.
   
    ![Azure AD Connect nedir?][9] 
5. Oturum açma için sizin @Task yönetici olarak şirket site.
6. Git **tek oturum açma yapılandırması**.
7. Üzerinde **çoklu oturum açma** iletişim kutusunda, aşağıdaki adımları uygulayın
   
    ![Çoklu oturum açmayı yapılandırın][23]
   
    a. Olarak **türü**seçin **SAML 2.0**.
   
    b. Seçin **hizmet sağlayıcı kimliği**.
   
    c. Azure Klasik portalı üzerinde kopyalama **uzaktan oturum açma URL'si**ve ardından yapıştırın **oturum açma portalı URL'si** metin kutusu.
   
    d. Azure Klasik portalı üzerinde kopyalama **tek Sign-Out hizmeti URL'si**ve ardından yapıştırın **Sign-Out URL** metin kutusu.
   
    e. Azure Klasik portalı üzerinde kopyalama **değişiklik parola URL'si**ve ardından yapıştırın **değişiklik parola URL'si** metin kutusu.
   
    f. **Kaydet** düğmesine tıklayın.
8. Klasik Azure portalı, çoklu oturum açma yapılandırması onay seçin ve ardından **sonraki**. 
   
    ![Azure AD Connect nedir?][10]
9. Üzerinde **tek oturum açma onay** sayfasında, **tam**.  
   
    ![Azure AD Connect nedir?][11]

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Klasik Azure portalında bir test kullanıcı oluşturmaktır.  

![Azure AD Kullanıcı oluşturma][20]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Klasik Azure portalı**, sol gezinti bölmesinde tıklatın **Active Directory**.
   
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-attask-tutorial/create_aaduser_02.png) 
2. Gelen **Directory** listesinde, directory tümleştirmesini etkinleştirmek istediğiniz dizini seçin.
3. Üstteki menüde kullanıcıların listesini görüntülemek için tıklatın **kullanıcılar**.
   
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-attask-tutorial/create_aaduser_03.png) 
4. Açmak için **Kullanıcı Ekle** iletişim kutusunda, araç çubuğunda alt tıklatın **Kullanıcı Ekle**. 
   
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-attask-tutorial/create_aaduser_04.png) 
5. Üzerinde **bu kullanıcı hakkında bize** iletişim sayfasında, aşağıdaki adımları gerçekleştirin: 
   
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-attask-tutorial/create_aaduser_05.png) 
   
    a. Kullanıcı türü olarak, kuruluşunuzdaki yeni kullanıcı seçin.
   
    b. Kullanıcı adı **textbox**, türü **BrittaSimon**.
   
    c. **İleri**’ye tıklayın.
6. Üzerinde **kullanıcı profili** iletişim sayfasında, aşağıdaki adımları gerçekleştirin: 
   
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-attask-tutorial/create_aaduser_06.png) 
   
    a. İçinde **ad** metin kutusuna, türü **Britta**.  
   
    b. İçinde **Soyadı** metin kutusuna, türü, **Simon**.
   
    c. İçinde **görünen adı** metin kutusuna, türü **Britta Simon**.
   
    d. İçinde **rol** listesinde **kullanıcı**.

    e. **İleri**’ye tıklayın.

7. Üzerinde **Get geçici parola** iletişim sayfasında, tıklatın **oluşturma**.
   
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-attask-tutorial/create_aaduser_07.png) 
8. Üzerinde **Get geçici parola** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-attask-tutorial/create_aaduser_08.png) 
   
    a. Değerini yazmak **yeni parola**.
   
    b. **Tamamla**’ya tıklayın.   

### <a name="creating-an-task-test-user"></a>Oluşturma bir @Task test kullanıcısı
Bu bölümün amacı Britta Simon adlı bir kullanıcı oluşturmaktır @Task.

**Britta Simon adlı bir kullanıcı oluşturmak için @Task, aşağıdaki adımları gerçekleştirin:**

1. Oturum, @Task Yöneticisi olarak şirket site.
2. Üstteki menüde tıklatın **kişiler**.
3. Tıklatın **yeni bir kişiye**. 
4. Yeni bir kişiye iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
    ![Oluşturma bir @Task test kullanıcısı][21] 
   
    a. İçinde **ad** metin kutusuna, "Britta" yazın.
   
    b. İçinde **Soyadı** metin kutusuna, "Simon" yazın.
   
    c. İçinde **e-posta adresi** metin kutusuna, Azure Active Directory'de Britta Simon'ın e-posta adresini yazın.
   
    d. Tıklatın **kişiyi ekler**.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama
Her erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirmek için bu bölümün amacı olan @Task.

![Kullanıcı atama][200] 

**Britta Simon atamak için @Task, aşağıdaki adımları gerçekleştirin:**

1. Azure Klasik portalı üzerinde dizin görünümünde uygulamaları görünümü açmak için tıklatın **uygulamaları** üst menüde.
   
    ![Kullanıcı atama][201] 
2. Uygulamalar listesinde  **@Task** .
   
    ![Kullanıcı atama][202] 
3. Üstteki menüde tıklatın **kullanıcılar**.
   
    ![Kullanıcı atama][203] 
4. Kullanıcılar listesinden seçin **Britta Simon**.
5. Araç çubuğunda alt tıklatın **atamak**.
   
    ![Kullanıcı atama][205]

### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme
Bu bölümün amacı erişim paneli kullanılarak Azure AD çoklu oturum açma yapılandırmanızı test etmektir.  
Tıkladığınızda @Task döşeme erişim panelinde otomatik olarak oturum açmayı almanız gerekir, @Task uygulama.

## <a name="additional-resources"></a>Ek Kaynaklar
* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-attask-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-attask-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-attask-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-attask-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_01.png
[30]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_02.png


[6]: ./media/active-directory-saas-attask-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_03.png
[8]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_04.png
[9]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_05.png
[10]: ./media/active-directory-saas-attask-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-attask-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-attask-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_08.png


[23]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_06.png

[200]: ./media/active-directory-saas-attask-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-attask-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_09.png
[203]: ./media/active-directory-saas-attask-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-attask-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-attask-tutorial/tutorial_general_205.png






