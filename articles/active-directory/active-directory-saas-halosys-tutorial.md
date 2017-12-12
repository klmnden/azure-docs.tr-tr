---
title: "Öğretici: Azure Active Directory Tümleştirme ile Halosys | Microsoft Docs"
description: "Çoklu oturum açma, otomatik sağlama ve daha fazla etkinleştirmek için Azure Active Directory ile Halosys kullanmayı öğrenin!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: mtillman
ms.assetid: 42a0eb7c-5cb7-44a9-b00b-b0e7df4b63e8
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/22/2017
ms.author: jeedes
ms.openlocfilehash: cfd932fa87ffd40ffc6ac96ad72ae7eac31e0b98
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-halosys"></a>Öğretici: Azure Active Directory Tümleştirme Halosys ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Halosys tümleştirmek öğrenin.

Halosys Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Halosys erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için Halosys (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Klasik Azure portalı Yönet

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Halosys ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Halosys çoklu oturum açma etkin abonelik


> [!NOTE] 
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.


Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Bu gerekli olmadığı sürece, üretim ortamınızın kullanmamanız gerekir.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin.

Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Halosys ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama


## <a name="adding-halosys-from-the-gallery"></a>Galeriden Halosys ekleme
Azure AD Halosys tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Halosys eklemeniz gerekir.

**Galeriden Halosys eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Klasik Azure portalı**, sol gezinti bölmesinde tıklatın **Active Directory**.

    ![Active Directory][1]
2. Gelen **Directory** listesinde, directory tümleştirmesini etkinleştirmek istediğiniz dizini seçin.

3. Dizin görünümünde uygulamaları görünümü açmak için **uygulamaları** üst menüde.

    ![Uygulamalar][2]

4. Tıklatın **Ekle** sayfanın sonundaki.

    ![Uygulamalar][3]

5. Üzerinde **ne yapmak istiyorsunuz** iletişim kutusunda, tıklatın **Galeriden bir uygulama eklemek**.

    ![Uygulamalar][4]

6. Arama kutusuna **Halosys**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_01.png)
    
7. Sonuçlar bölmesinde seçin **Halosys**ve ardından **tam** uygulama eklemek için.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_011.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Halosys sınayın.

Tekli çalışmaya oturum için Azure AD Halosys karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Halosys ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Bu bağlantı değeri atayarak ilişkisi **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** Halosys içinde.

Yapılandırma ve Azure AD çoklu oturum açma Halosys ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Halosys test kullanıcısı oluşturma](#creating-a-halosys-test-user)**  - Britta Simon, karşılık gelen her, Azure AD gösterimine bağlı Halosys sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Klasik portalında etkinleştirin ve çoklu oturum açma Halosys uygulamanızda yapılandırın.


**Azure AD çoklu oturum açma ile Halosys yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Klasik Portalı'ndaki üzerinde **Halosys** uygulama tümleştirme sayfası, tıklatın **çoklu oturum açma yapılandırmak** açmak için **yapılandırma çoklu oturum açma** iletişim.
     
    ![Çoklu oturum açmayı yapılandırın][6] 

2. Üzerinde **Halosys için oturum açmasını nasıl istiyorsunuz** sayfasında, **Azure AD çoklu oturum açma**ve ardından **sonraki**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_03.png) 

3. Üzerinde **uygulama ayarlarını yapılandır** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_04.png) 

    a. İçinde **oturum üzerinde URL'si** metin kutusuna, türü URL kullanıcılarınıza oturum açma şu biçimi kullanarak Halosys uygulamanız tarafından kullanılan: `https://<company-name>.Halosys.com/client-api/api`.

    b.In **tanımlayıcı URL'si** metin kutusuna, aşağıdaki desende URL'yi yazın: `https://<company-name>.Halosys.com`.   
         
4. Üzerinde **çoklu oturum açma sırasında Halosys yapılandırma** sayfasında, **karşıdan meta veri**ve ardından dosyayı bilgisayarınıza kaydedin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_05.png)
   
5. Uygulamanız için yapılandırılmış SSO almak için Halosys Destek ekibine başvurun ve aşağıdaki verin:

    • İndirilen **meta veri dosyası**
    
    • **SAML SSO URL'si**
    

6. Klasik Portalı'ndaki tek oturum açma yapılandırması onay seçin ve ardından **sonraki**.
    
    ![Azure AD çoklu oturum açma][10]

7. Üzerinde **tek oturum açma onay** sayfasında, **tam**.  
 
    ![Azure AD çoklu oturum açma][11]


### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümde, bir test kullanıcı Britta Simon adlı Klasik portalda oluşturun.


![Azure AD Kullanıcı oluşturma][20]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Klasik Azure portalı**, sol gezinti bölmesinde tıklatın **Active Directory**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-Halosys-tutorial/create_aaduser_09.png) 

2. Gelen **Directory** listesinde, directory tümleştirmesini etkinleştirmek istediğiniz dizini seçin.

3. Üstteki menüde kullanıcıların listesini görüntülemek için tıklatın **kullanıcılar**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-Halosys-tutorial/create_aaduser_03.png) 

4. Açmak için **Kullanıcı Ekle** iletişim kutusunda, araç çubuğunda alt tıklatın **Kullanıcı Ekle**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-Halosys-tutorial/create_aaduser_04.png) 

5. Üzerinde **bu kullanıcı hakkında bize** iletişim sayfasında, aşağıdaki adımları gerçekleştirin: ![bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-Halosys-tutorial/create_aaduser_05.png) 

    a. Kullanıcı türü olarak, kuruluşunuzdaki yeni kullanıcı seçin.

    b. Kullanıcı adı **textbox**, türü **BrittaSimon**.

    c. **İleri**’ye tıklayın.

6.  Üzerinde **kullanıcı profili** iletişim sayfasında, aşağıdaki adımları gerçekleştirin: ![bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-Halosys-tutorial/create_aaduser_06.png) 

    a. İçinde **ad** metin kutusuna, türü **Britta**.  

    b. İçinde **Soyadı** metin kutusuna, türü, **Simon**.

    c. İçinde **görünen adı** metin kutusuna, türü **Britta Simon**.

    d. İçinde **rol** listesinde **kullanıcı**.

    e. **İleri**’ye tıklayın.

7. Üzerinde **Get geçici parola** iletişim sayfasında, tıklatın **oluşturma**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-Halosys-tutorial/create_aaduser_07.png) 

8. Üzerinde **Get geçici parola** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-Halosys-tutorial/create_aaduser_08.png) 

    a. Değerini yazmak **yeni parola**.

    b. **Tamamla**’ya tıklayın.   



### <a name="creating-a-halosys-test-user"></a>Halosys test kullanıcısı oluşturma

Bu bölümde, Halosys içinde Britta Simon adlı bir kullanıcı oluşturun. Lütfen Halosys platform kullanıcıları eklemek için Halosys destek ekibi ile çalışın.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Halosys için kendi erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Halosys için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Klasik portalı üzerinde dizin görünümünde uygulamaları görünümü açmak için tıklatın **uygulamaları** üst menüde.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Halosys**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_50.png) 

3. Üstteki menüde tıklatın **kullanıcılar**.

    ![Kullanıcı atama][203]

4. Kullanıcılar listesinden seçin **Britta Simon**.

5. Araç çubuğunda alt tıklatın **atamak**.

    ![Kullanıcı atama][205]


### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Halosys parçasında tıklattığınızda, otomatik olarak Halosys uygulamanıza açan.


## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_205.png
