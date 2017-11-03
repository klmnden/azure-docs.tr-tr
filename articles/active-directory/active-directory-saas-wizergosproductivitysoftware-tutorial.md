---
title: "Öğretici: Azure Active Directory Tümleştirme Wizergos üretkenlik ile | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile Wizergos üretkenlik arasında yapılandırmayı öğrenin."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: acc04396-13c5-4c24-ab9a-30fbc9234ebd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2017
ms.author: jeedes
ms.openlocfilehash: 73b3bc05aeb337c12acb7e47c0dbebe6d0196530
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-wizergos-productivity-software"></a>Öğretici: Azure Active Directory Tümleştirme ile Wizergos üretkenlik
Bu öğreticinin amacı Wizergos üretkenlik Azure Active Directory (Azure AD) ile tümleştirme Göster sağlamaktır.

Wizergos üretkenlik Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Wizergos üretkenlik erişimi, Azure AD'de kontrol edebilirsiniz
* Otomatik olarak Wizergos üretkenlik çoklu oturum açma (SSO) ile Azure AD hesaplarına için açan kullanıcılarınıza etkinleştirebilirsiniz
* Hesaplarınızı bir merkezi konumda - Klasik Azure portalı Yönet

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar
Azure AD tümleştirme Wizergos üretkenlik ile yapılandırmak için aşağıdaki öğeleri gerekir:

* Bir Azure AD aboneliği
* Abonelik Wizergos üretkenlik yazılım SSO etkin

>[!NOTE]
>Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz. 
> 

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

* Bu gerekli olmadığı sürece, üretim ortamınızın kullanmamanız gerekir.
* Bir Azure AD deneme ortam yoksa, alabileceğiniz bir [bir aylık deneme](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticinin amacı, Azure AD SSO bir test ortamında test etmenizi hale getirmektir.

Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Wizergos üretkenlik ekleme
2. Yapılandırma ve Azure AD SSO test etme

## <a name="adding-wizergos-productivity-software-from-the-gallery"></a>Galeriden Wizergos üretkenlik ekleme
Azure AD Wizergos üretkenlik tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Wizergos üretkenlik eklemeniz gerekir.

**Galeriden Wizergos üretkenlik eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Klasik Azure portalı**, sol gezinti bölmesinde tıklatın **Active Directory**. 
   
    ![Active Directory][1]
2. Gelen **Directory** listesinde, directory tümleştirmesini etkinleştirmek istediğiniz dizini seçin.
3. Dizin görünümünde uygulamaları görünümü açmak için **uygulamaları** üst menüde.
   
    ![Uygulamalar][2]
4. Tıklatın **Ekle** sayfanın sonundaki.
   
    ![Uygulamalar][3]
5. Üzerinde **ne yapmak istiyorsunuz** iletişim kutusunda, tıklatın **Galeriden bir uygulama eklemek**.
   
    ![Uygulamalar][4]
6. Arama kutusuna **Wizergos üretkenlik**.
   
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_01.png)
7. Sonuçlar panelinde seçin **Wizergos üretkenlik**ve ardından **tam** uygulama eklemek için.
   
    ![Uygulama galerisinde seçme](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_001.png)

## <a name="configure-and-test-azure-ad-sso"></a>Yapılandırma ve Azure AD SSO test etme
Bu bölümün amacı, size nasıl yapılandırılacağı ve Azure AD "Britta Simon" adlı bir test kullanıcı tabanlı Wizergos üretkenlik SSO'su test göstermektir.

Çalışmak SSO için Azure AD Wizergos üretkenlik Azure AD'de bir kullanıcıya karşılık gelen kullanıcı ne olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Wizergos üretkenlik ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Bu bağlantı değeri atayarak ilişkisi **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** Wizergos üretkenlik yazılım.

Yapılandırma ve Azure AD çoklu oturum açma BynWizergos üretkenlik Softwareder ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Wizergos üretkenlik test kullanıcısı oluşturma](#creating-a-wizergos-productivity-software-test-user)**  - Britta Simon, karşılık gelen Wizergos Azure AD gösterimini her için bağlantılı üretkenlik sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-sso"></a>Azure AD SSO yapılandırma
Bu bölümde, Azure AD çoklu oturum açma Klasik portalında etkinleştirin ve çoklu oturum açma Wizergos üretkenlik uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Wizergos üretkenlik ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Klasik Portalı'ndaki üzerinde **Wizergos üretkenlik** uygulama tümleştirme sayfası, tıklatın **çoklu oturum açma yapılandırmak** açmak için **yapılandırma çoklu oturum açma**  iletişim kutusu.
   
    ![Çoklu oturum açmayı yapılandırın][6] 
2. Üzerinde **Wizergos üretkenlik oturum açmasını nasıl istiyorsunuz** sayfasında, **Azure AD çoklu oturum açma**ve ardından **sonraki**:
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_03.png)
3. Üzerinde **uygulama ayarlarını yapılandır** iletişim sayfasında, tıklatın **sonraki**:
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_04.png)
4. Üzerinde **çoklu oturum açma sırasında Wizergos üretkenlik yapılandırma** sayfasında, **indirme sertifika**ve ardından dosyayı bilgisayarınıza kaydedin:
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_05.png)
5. Farklı web tarayıcısı penceresinde Wizergos üretkenlik kiracınız yönetici olarak oturum.
6. Hamburger menüsünden seçin **yönetici**.
   
    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_000.png)
7. Sol taraftaki menüsünde Yönetim sayfasında seçin **kimlik doğrulaması** ve tıklayın **Azure AD**.
   
    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_002.png)
8. Aşağıdaki adımları gerçekleştirin **kimlik doğrulaması** bölümü.
   
    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_003.png)
  1. Tıklatın **karşıya** Azure AD'den indirilen sertifikayı karşıya yüklemek için düğmeyi. 
  2. İçinde **veren URL'si** textbox değeri put **veren URL'si** Azure AD Uygulama Yapılandırma Sihirbazı'ndan.
  3. İçinde **çoklu oturum açma URL'si** textbox değeri put **çoklu oturum açma hizmet URL'si** Azure AD Uygulama Yapılandırma Sihirbazı'ndan.
  4. İçinde **tek Sign-Out URL** textbox değeri put **tek Sign-out hizmeti URL'si** Azure AD Uygulama Yapılandırma Sihirbazı'ndan.
  5. Tıklatın **kaydetmek** düğmesi.
9. Klasik Portalı'ndaki tek oturum açma yapılandırması onay seçin ve ardından **sonraki**.
   
    ![Azure AD çoklu oturum açma][10]
10. Üzerinde **tek oturum açma onay** sayfasında, **tam**.  
    
    ![Azure AD çoklu oturum açma][11]

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Klasik portalda bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][20]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Klasik Azure portalı**, sol gezinti bölmesinde tıklatın **Active Directory**.
   
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_09.png)
2. Gelen **Directory** listesinde, directory tümleştirmesini etkinleştirmek istediğiniz dizini seçin.
3. Üstteki menüde kullanıcıların listesini görüntülemek için tıklatın **kullanıcılar**.
   
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_03.png)
4. Açmak için **Kullanıcı Ekle** iletişim kutusunda, araç çubuğunda alt tıklatın **Kullanıcı Ekle**.
   
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_04.png)
5. Üzerinde **bu kullanıcı hakkında bize** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_05.png) 
  1. Kullanıcı türü olarak, kuruluşunuzdaki yeni kullanıcı seçin.
  2. Kullanıcı adı **textbox**, türü **BrittaSimon**.
  3. **İleri**’ye tıklayın.
6. Üzerinde **kullanıcı profili** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
   
   ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_06.png)
  1. İçinde **ad** metin kutusuna, türü **Britta**.  
  2. İçinde **Soyadı** metin kutusuna, türü, **Simon**.
  3. İçinde **görünen adı** metin kutusuna, türü **Britta Simon**.
  4. İçinde **rol** listesinde **kullanıcı**.
  5. **İleri**’ye tıklayın.
7. Üzerinde **Get geçici parola** iletişim sayfasında, tıklatın **oluşturma**.
   
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_07.png)
8. Üzerinde **Get geçici parola** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_08.png)
  1. Değerini yazmak **yeni parola**.
  2. **Tamamla**’ya tıklayın.   

### <a name="create-a-wizergos-productivity-software-test-user"></a>Wizergos üretkenlik test kullanıcısı oluşturma
Bu bölümde, Britta Simon Wizergos üretkenlik adlı bir kullanıcı oluşturun. Lütfen Wizergos üretkenlik destek ekibi ile çalışmak [ support@wizergos.com ](emailTo:support@wizergos.com) Wizergos üretkenlik platform kullanıcıları eklemek için.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın
Bu bölümün amacı Britta Wizergos üretkenlik her erişim vererek Azure SSO kullanılacak Simon için etkinleştirmektir.

  ![Kullanıcı atama][200]

**Britta Simon Wizergos üretkenlik atamak için aşağıdaki adımları gerçekleştirin:**

1. Klasik portalı üzerinde dizin görünümünde uygulamaları görünümü açmak için tıklatın **uygulamaları** üst menüde.
   
    ![Kullanıcı atama][201]
2. Uygulamalar listesinde **Wizergos üretkenlik**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_50.png)
3. Üstteki menüde tıklatın **kullanıcılar**.
   
    ![Kullanıcı atama][203]
4. Kullanıcılar listesinden seçin **Britta Simon**.
5. Araç çubuğunda alt tıklatın **atamak**.
   
    ![Kullanıcı atama][205]

### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin
Bu bölümün amacı erişim paneli kullanılarak Azure AD SSO yapılandırmanızı test etmektir.

Erişim paneli Wizergos üretkenlik parçasında tıklattığınızda, otomatik olarak Wizergos üretkenlik uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar
* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_205.png
