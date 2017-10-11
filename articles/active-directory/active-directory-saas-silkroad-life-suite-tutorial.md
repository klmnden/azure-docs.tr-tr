---
title: "Öğretici: Azure Active Directory Tümleştirme SilkRoad yaşam Suite ile | Microsoft Docs"
description: "Çoklu oturum açma SilkRoad yaşam Suite ile Azure Active Directory arasındaki yapılandırmayı öğrenin."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 3cd92319-7964-41eb-8712-444f5c8b4d15
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/10/2017
ms.author: jeedes
ms.openlocfilehash: ecf4e31ecea00d003fc47ea4cebb781ca58957f7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-silkroad-life-suite"></a>Öğretici: Azure Active Directory Tümleştirme SilkRoad yaşam Suite ile
Bu öğreticinin amacı SilkRoad yaşam Suite Azure Active Directory (Azure AD) ile tümleştirme Göster sağlamaktır. 

SilkRoad yaşam Suite Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar: 

* SilkRoad yaşam Suite erişimi, Azure AD'de kontrol edebilirsiniz 
* Otomatik olarak SilkRoad yaşam Suite çoklu oturum açma (SSO) ile Azure AD hesaplarına için açan kullanıcılarınıza etkinleştirebilirsiniz

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar
Azure AD tümleştirme SilkRoad yaşam paketiyle yapılandırmak için aşağıdaki öğeleri gerekir:

* Bir Azure AD aboneliği
* Abonelik SilkRoad yaşam Suite SSO etkin

>[!NOTE]
>Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz. 
> 

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

* Bu gerekli olmadığı sürece, üretim ortamınızın kullanmamanız gerekir.
* Bir Azure AD deneme ortam yoksa, alabileceğiniz bir [bir aylık deneme](https://azure.microsoft.com/pricing/free-trial/). 

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticinin amacı, Azure AD SSO bir test ortamında test etmenizi hale getirmektir.

Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden SilkRoad yaşam paketi ekleme 
2. Yapılandırma ve Azure AD SSO test etme

## <a name="add-silkroad-life-suite-from-the-gallery"></a>Galeriden SilkRoad yaşam paketi ekleme
Azure AD SilkRoad yaşam Suite tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden SilkRoad yaşam paketi eklemeniz gerekir.

**Galeriden SilkRoad yaşam paketi eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Klasik Azure portalı**, sol gezinti bölmesinde tıklatın **Active Directory**. 
   
    ![Active Directory][1]

2. Gelen **Directory** listesinde, directory tümleştirmesini etkinleştirmek istediğiniz dizini seçin.

3. Dizin görünümünde uygulamaları görünümü açmak için **uygulamaları** üst menüde.
   
    ![Uygulamalar][2]

4. Tıklatın **Ekle** sayfanın sonundaki.
   
    ![Uygulamalar][3]

5. Üzerinde **ne yapmak istiyorsunuz** iletişim kutusunda, tıklatın **Galeriden bir uygulama eklemek**.
   
    ![Uygulamalar][4]

6. Arama kutusuna **SilkRoad yaşam Suite**.
   
    ![Uygulamalar][5]

7. Sonuçlar bölmesinde seçin **SilkRoad yaşam Suite**ve ardından **tam** uygulama eklemek için.
   
    ![Uygulamalar][50]

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme
Bu bölümün amacı, size nasıl yapılandırılacağı ve SilkRoad yaşam "Britta Simon" adlı bir test kullanıcı tabanlı Suite ile Azure AD SSO test göstermektir.

Çalışmak SSO için Azure AD Azure AD'de bir kullanıcıya karşılık gelen kullanıcı SilkRoad yaşam paketindeki nedir bilmesi gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının SilkRoad yaşam paketindeki ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Bu bağlantı değeri atayarak ilişkisi **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** SilkRoad yaşam paketindeki.

Yapılandırma ve Azure AD çoklu oturum açma SilkRoad yaşam Suite ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[SilkRoad yaşam Suite test kullanıcısı oluşturma](#creating-a-silkroad-life-suite-test-user)**  - Britta Simon, karşılık gelen her, Azure AD gösterimine bağlı SilkRoad yaşam Suite sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın
Bu bölümün amacı, Klasik Azure portalında Azure AD SSO'yu etkinleştirmek ve SSO SilkRoad yaşam Suite Uygulamanızı yapılandırmak için ' dir.

**Azure AD çoklu oturum açma SilkRoad yaşam paketiyle yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. SilkRoad şirket sitenize yönetici olarak oturum. 

  >[!NOTE] 
  > Microsoft Azure AD ile Federasyon yapılandırma SilkRoad yaşam Suite kimlik doğrulaması uygulamaya erişim elde etmek için lütfen SilkRoad desteği veya SilkRoad Hizmetleri temsilcinize başvurun.
  > 

2. Git **hizmet sağlayıcısı**ve ardından **Federasyon ayrıntıları**. 
   
    ![Azure AD çoklu oturum açma][10] 

3. Tıklatın **karşıdan Federasyon meta verileri**ve meta veri dosyası, bilgisayarınıza kaydedin.
   
    ![Azure AD çoklu oturum açma][11] 

4. Azure Klasik portalında üzerinde **SilkRoad yaşam Suite** uygulama tümleştirme sayfası, tıklatın **çoklu oturum açma özelliğini yapılandırın** açmak için **yapılandırma çoklu oturum açma** iletişim.
   
    ![Çoklu oturum açmayı yapılandırın][6] 

5. Üzerinde **SilkRoad yaşam paketine oturum açmasını nasıl istiyorsunuz** sayfasında, **Azure AD çoklu oturum açma**ve ardından **sonraki**.
   
    ![Azure AD çoklu oturum açma][7] 

6. Üzerinde **uygulama ayarlarını yapılandır** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Azure AD çoklu oturum açma][8]   
 1. İçinde **oturum üzerinde URL'si** metin kutusuna, türü URL'yi, kullanıcılarınıza oturum açma SilkRoad yaşam Suite sitenize tarafından kullanılan (örn: *https://defcompanytest-test-redcarpet.silkroad-eng.com/Authentication/*).  
 2. İndirilen açmak **Silkroad** meta veri dosyası. 
 3. Bulun **AssertionConsumerService** etiketi ve ardından kopyalama **konumu** özniteliği.         
   
    ![Azure AD çoklu oturum açma][21] 
 4. Değeri içine yapıştırabilirsiniz **yanıt URL'si** metin kutusu.  
 5. **İleri**’ye tıklayın.

6. Üzerinde **çoklu oturum açma SilkRoad yaşam Suite yapılandırma** sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Azure AD çoklu oturum açma][9]  
 1. İndirme sertifikası'nı tıklatın ve ardından dosyayı bilgisayarınıza kaydedin.  
 2. **İleri**’ye tıklayın.

7. İçinde **SilkRoad** uygulama tıklatın **kimlik doğrulaması kaynakları**.
   
    ![Azure AD çoklu oturum açma][12] 

8. Tıklatın **kimlik doğrulaması kaynağı Ekle**. 
   
    ![Azure AD çoklu oturum açma][13] 

9. İçinde **kimlik doğrulaması kaynağı Ekle** bölümünde, aşağıdaki adımları gerçekleştirin: 
   
    ![Azure AD çoklu oturum açma][14]  
 1. Altında **seçeneği 2 - meta veri dosyası**, tıklatın **Gözat** indirilen meta veri dosyasını karşıya yüklemek için.  
 2. Tıklatın **kimlik dosya verilerini kullanarak sağlayıcısı oluşturma**.

10. İçinde **kimlik doğrulaması kaynakları** 'yi tıklatın **Düzenle**. 
    
     ![Azure AD çoklu oturum açma][15] 

11. Üzerinde **kimlik doğrulaması kaynağını Düzenle** iletişim kutusunda, aşağıdaki adımları gerçekleştirin: 
    
     ![Azure AD çoklu oturum açma][16] 
 1. Olarak **etkin**seçin **Evet**.   
 2. İçinde **IDP açıklama** metin kutusuna, yapılandırmanız için bir açıklama yazın (örneğin: *Azure AD SSO*).  
 3. İçinde **IDP adı** metin kutusuna, yapılandırmanızda özgü adını yazın (örneğin: *Azure SP*).  
 4. **Kaydet** düğmesine tıklayın.

12. Diğer tüm kimlik doğrulaması kaynakları devre dışı bırakın. 
    
     ![Azure AD çoklu oturum açma][17]

13. Azure Klasik portalında üzerinde **tek oturum açma onay** sayfasında, **sonraki**.  
    
     ![Azure AD çoklu oturum açma][18]

14. Üzerinde **tek oturum açma onay** sayfasında, **tam**.
    
     ![Azure AD çoklu oturum açma][19]

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Klasik Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][20]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Klasik Azure portalı**, sol gezinti bölmesinde tıklatın **Active Directory**.
   
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_09.png)  

2. Gelen **Directory** listesinde, directory tümleştirmesini etkinleştirmek istediğiniz dizini seçin.

3. Üstteki menüde kullanıcıların listesini görüntülemek için tıklatın **kullanıcılar**.
   
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_03.png) 

4. Açmak için **Kullanıcı Ekle** iletişim kutusunda, araç çubuğunda alt tıklatın **Kullanıcı Ekle**. 
   
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_04.png) 

5. Üzerinde **bu kullanıcı hakkında bize** iletişim sayfasında, aşağıdaki adımları gerçekleştirin: 
   
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_05.png)  
 1. Kullanıcı türü olarak, kuruluşunuzdaki yeni kullanıcı seçin.  
 2. Kullanıcı adı **textbox**, türü **BrittaSimon**. 
 3. **İleri**’ye tıklayın.

6. Üzerinde **kullanıcı profili** iletişim sayfasında, aşağıdaki adımları gerçekleştirin: 
   
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_06.png)  
 1. İçinde **ad** metin kutusuna, türü **Britta**.    
 2. İçinde **Soyadı** metin kutusuna, türü, **Simon**. 
 3. İçinde **görünen adı** metin kutusuna, türü **Britta Simon**. 
 4. İçinde **rol** listesinde **kullanıcı**.
 5. **İleri**’ye tıklayın.

7. Üzerinde **Get geçici parola** iletişim sayfasında, tıklatın **oluşturma**.
   
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_07.png) 

8. Üzerinde **Get geçici parola** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_08.png)  
 1. Değerini yazmak **yeni parola**. 
 2. **Tamamla**’ya tıklayın.   

### <a name="create-a-silkroad-life-suite-test-user"></a>SilkRoad yaşam Suite test kullanıcısı oluşturma
Bu bölümün amacı Britta Simon SilkRoad yaşam paketindeki adlı bir kullanıcı oluşturmaktır. Britta'nın bir SSO kimliği olmalıdır (bazen denir bir *AuthParam*) Britta'nın eşleşen **emailaddress** Azure AD'de.

**Britta Simon SilkRoad yaşam paketindeki adlı bir kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

- Sahip bir kullanıcı oluşturmak için SilkRoad yaşam Suite Destek ekibinize başvurun **SSO kimliği** aynı değere özniteliği **emailaddress** Britta Simon Azure AD'de.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın
Bu bölümün amacı Britta SilkRoad yaşam paketine kendi erişim vererek Azure SSO kullanılacak Simon sağlamaktır.

![Kullanıcı atama][200] 

**Britta Simon SilkRoad yaşam paketine atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure Klasik portalı üzerinde dizin görünümünde uygulamaları görünümü açmak için tıklatın **uygulamaları** üst menüde.
   
    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **SilkRoad yaşam Suite**.
   
    ![Kullanıcı atama][202] 

3. Üstteki menüde tıklatın **kullanıcılar**.
   
    ![Kullanıcı atama][203] 

4. Kullanıcılar listesinden seçin **Britta Simon**.

5. Araç çubuğunda alt tıklatın **atamak**.
   
    ![Kullanıcı atama][205]

### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin
Bu bölümün amacı erişim paneli kullanılarak Azure AD SSO yapılandırmanızı test etmektir.  

Erişim paneli SilkRoad yaşam Suite parçasında tıklattığınızda, otomatik olarak SilkRoad yaşam Suite uygulamanıza açan.

## <a name="additional-resources"></a>Ek Kaynaklar
* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_01.png
[50]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_02.png

[6]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_03.png
[8]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_04.png
[9]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_05.png
[10]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_06.png
[11]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_07.png
[12]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_08.png
[13]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_09.png
[14]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_10.png
[15]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_11.png
[16]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_12.png
[17]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_13.png
[18]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_06.png
[19]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_07.png


[20]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_15.png


[200]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_14.png
[203]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_205.png





