---
title: "Öğretici: Azure Active Directory Tümleştirme SciQuest harcamanız Director ile | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile SciQuest harcamanız Director arasında yapılandırmayı öğrenin."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 9fab641b-292e-4bef-91d1-8ccc4f3a0c1f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/17/2017
ms.author: jeedes
ms.openlocfilehash: 84b707668dc45e92e6151f422f1c919f638533b1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sciquest-spend-director"></a>Öğretici: Azure Active Directory Tümleştirme SciQuest harcamanız Director ile
Bu öğreticinin amacı SciQuest harcamanız Director Azure Active Directory (Azure AD) ile tümleştirme Göster sağlamaktır.  
SciQuest harcadığı Director Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar: 

* SciQuest harcadığı Director erişimi, Azure AD'de kontrol edebilirsiniz 
* Azure AD hesaplarına otomatik olarak SciQuest harcamanız Director (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz
* Hesaplarınızı bir merkezi konumda - Klasik Azure portalı Yönet

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar
Azure AD tümleştirme SciQuest harcamanız Director ile yapılandırmak için aşağıdaki öğeleri gerekir:

* Bir Azure AD aboneliği
* Bir SciQuest harcamanız Director çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.
> 
> 

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

* Bu gerekli olmadığı sürece, üretim ortamınızın kullanmamanız gerekir.
* Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/). 

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticinin amacı, Azure AD çoklu oturum açma bir test ortamında test etmenizi hale getirmektir.  
Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden SciQuest harcamanız Director ekleme 
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-sciquest-spend-director-from-the-gallery"></a>Galeriden SciQuest harcamanız Director ekleme
Azure AD SciQuest harcamanız Director tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden SciQuest harcamanız Director eklemeniz gerekir.

**Galeriden SciQuest harcamanız Director eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Klasik Azure portalı**, sol gezinti bölmesinde tıklatın **Active Directory**. 
   
    ![Active Directory][1]

2. Gelen **Directory** listesinde, directory tümleştirmesini etkinleştirmek istediğiniz dizini seçin.

3. Dizin görünümünde uygulamaları görünümü açmak için **uygulamaları** üst menüde.
   
    ![Uygulamalar][2]

4. Tıklatın **Ekle** sayfanın sonundaki.
   
    ![Uygulamalar][3]

5. Üzerinde **ne yapmak istiyorsunuz** iletişim kutusunda, tıklatın **Galeriden bir uygulama eklemek**.
   
    ![Uygulamalar][4]

6. Arama kutusuna **sciQuest harcamanız director**.
   
    ![Uygulamalar][5]

7. Sonuçlar bölmesinde seçin **SciQuest harcamanız Director**ve ardından **tam** uygulama eklemek için.
   
    ![Uygulamalar][6]

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümün amacı, size nasıl yapılandırılacağı ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı SciQuest harcamanız Director test göstermektir.

Tekli çalışmaya oturum için Azure AD Azure AD'de bir kullanıcıya karşılık gelen kullanıcı SciQuest harcamanız Director nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı SciQuest harcamanız Director arasında bir bağlantı ilişkisi kurulması gerekir.  
Bu bağlantı değeri atayarak ilişkisi **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** SciQuest harcamanız Director.

Yapılandırma ve Azure AD çoklu oturum açma SciQuest harcamanız Director ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD tek tek oturum açma yapılandırma](#configuring-azure-ad-single-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[SciQuest harcadığı Director test kullanıcısı oluşturma](#creating-a-halogen-software-test-user)**  - Britta Simon, karşılık gelen SciQuest harcamanız Director Azure AD gösterimini her için bağlantılı sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-single-sign-on"></a>Azure AD çoklu çoklu oturum açma yapılandırma
Azure AD çoklu oturum açma Klasik Azure portalındaki etkinleştirmek ve çoklu oturum açma SciQuest harcamanız Director Uygulamanızı yapılandırmak için bu bölümün amacı olur.

**Azure AD çoklu oturum açma SciQuest harcamanız Director ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure Klasik portalında üzerinde **SciQuest harcamanız Director** uygulama tümleştirme sayfası, tıklatın **çoklu oturum açma özelliğini yapılandırın** açmak için **yapılandırma çoklu oturum açma** iletişim.
   
    ![Çoklu oturum açmayı yapılandırın][8]

2. Üzerinde **SciQuest harcamanız Director oturum açmasını nasıl istiyorsunuz** sayfasında, **Azure AD çoklu oturum açma**ve ardından **sonraki**.
   
    ![Azure AD çoklu oturum açma][9]

3. Üzerinde **uygulama ayarlarını yapılandır** iletişim sayfasında, aşağıdaki adımları gerçekleştirin: 
   
    ![Uygulama ayarlarını yapılandırın][10]
   
     a. İçinde **oturum üzerinde URL'si** metin kutusuna, türü URL'nizi kullanıcılarınız tarafından şu biçimi kullanarak SciQuest harcamanız Director uygulamanıza oturum açma için kullanılan: *https://.* SciQuest.com/.**
   
     b. İçinde **yanıt URL'si** metin kutusuna, içine yazdığınız aynı değeri yazın **oturum üzerinde URL'si** metin kutusu. 
   
     c. **İleri**’ye tıklayın.

4. Üzerinde **çoklu oturum açma SciQuest harcamanız Director yapılandırma** sayfasında, **karşıdan meta veri**ve meta veri dosyası, bilgisayarınıza yerel olarak kaydedin.
   
    ![Azure AD Connect nedir?][11]

5. Yukarıdaki indirilen meta verileri kullanarak bu kimlik doğrulama yöntemini etkinleştirmek için kişi SciQuest destekler.

6. Klasik Azure portalı, çoklu oturum açma yapılandırması onay seçin ve ardından **tam** kapatmak için **tek oturum yapılandırma üzerinde aktar** iletişim. 
   
    ![Azure AD Connect nedir?][15]

7. Üzerinde **tek oturum açma onay** sayfasında, **tam**.  

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Klasik Azure portalında bir test kullanıcı oluşturmaktır.

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Klasik Azure portalı**, sol gezinti bölmesinde tıklatın **Active Directory**.
   
    ![Azure AD Connect nedir?][100] 

2. Gelen **Directory** listesinde, directory tümleştirmesini etkinleştirmek istediğiniz dizini seçin.

3. Üstteki menüde kullanıcıların listesini görüntülemek için tıklatın **kullanıcılar**.
   
    ![Azure AD Connect nedir?][101] 

4. Açmak için **Kullanıcı Ekle** iletişim kutusunda, araç çubuğunda alt tıklatın **Kullanıcı Ekle**. 
   
    ![Azure AD Connect nedir?][102] 

5. Üzerinde **bu kullanıcı hakkında bize** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Azure AD Connect nedir?][103] 
   
    a. Olarak **kullanıcı türü**seçin **kuruluşunuzdaki yeni kullanıcı**.
   
    b. Kullanıcı adı **textbox**, türü **BrittaSimon**.
   
    c. **İleri**’ye tıklayın.

6. Üzerinde **kullanıcı profili** iletişim sayfasında, aşağıdaki adımları gerçekleştirin: 
   
    ![Azure AD Connect nedir?][104] 
   
    a. İçinde **ad** metin kutusuna, türü **Britta**.  
   
    b. İçinde **Soyadı** txtbox, türü, **Simon**.
   
    c. İçinde **görünen adı** metin kutusuna, türü **Britta Simon**.
   
    d. İçinde **rol** listesinde **kullanıcı**.
   
    e. **İleri**’ye tıklayın.

7. Üzerinde **Get geçici parola** iletişim sayfasında, tıklatın **oluşturma**.
   
    ![Azure AD Connect nedir?][105]  

8. Üzerinde **Get geçici parola** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Azure AD Connect nedir?][106]   
   
    a. Değerini yazmak **yeni parola**.
   
    b. **Tamamla**’ya tıklayın.   

### <a name="creating-a-sciquest-spend-director-test-user"></a>SciQuest harcadığı Director test kullanıcısı oluşturma
Bu bölümün amacı Britta Simon SciQuest harcamanız Director uygulamasında adlı bir kullanıcı oluşturmaktır.

SciQuest harcadığı Director destek ekibinize başvurun ve bunları oluşturulan edinilir test hesabınız hakkındaki ayrıntıları sağlamak gerekir.

Alternatif olarak, yalnızca zaman da kullanabilirsiniz sağlama, SciQuest harcamanız Director tarafından desteklenen tek bir oturum açma özelliği.  
Yalnızca zaman sağlama etkinse, yoksa kullanıcılar otomatik olarak SciQuest harcamanız Director tarafından bir tek oturum açma girişimi sırasında oluşturulur. Bu özellik tek oturum açma karşılık gelen kullanıcıları el ile oluşturma ihtiyacını ortadan kaldırır.

Yalnızca zaman etkin sağlama almak için başvurmanız gerekir SciQuest harcamanız Director destek ekibinize demektir.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama
Bu bölümün amacı Britta SciQuest harcamanız Director kendi erişim vererek, Azure çoklu oturum açma kullanılacak Simon için etkinleştirmektir.

![Azure AD Connect nedir?][200]

**Britta Simon SciQuest harcamanız Director atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure Klasik portalı üzerinde dizin görünümünde uygulamaları görünümü açmak için tıklatın **uygulamaları** üst menüde.
   
    ![Azure AD Connect nedir?][201]

2. Uygulamalar listesinde **SciQuest harcamanız Director**.
   
    ![Azure AD Connect nedir?][202]

3. Üstteki menüde tıklatın **kullanıcılar**.
   
    ![Azure AD Connect nedir?][203]

4. Kullanıcılar listesinden seçin **Britta Simon**.
   
    ![Azure AD Connect nedir?][204]

5. Araç çubuğunda alt tıklatın **atamak**.
   
    ![Azure AD Connect nedir?][205]

### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme
Bu bölümün amacı erişim paneli kullanılarak Azure AD çoklu oturum açma yapılandırmanızı test etmektir.  
Erişim paneli SciQuest harcamanız Director parçasında tıklattığınızda, otomatik olarak SciQuest harcamanız Director uygulamanıza açan.

## <a name="additional-resources"></a>Ek Kaynaklar
* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_01.png
[6]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_05.png
[8]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_06.png
[9]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_07.png
[10]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_08.png
[11]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_03.png
[15]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_04.png

[100]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_09.png 
[101]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_10.png 
[102]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_11.png 
[103]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_12.png 
[104]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_13.png 
[105]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_14.png 
[106]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_15.png 
[200]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_16.png 
[201]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_17.png 
[202]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_06.png
[203]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_18.png
[204]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_19.png
[205]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_20.png

