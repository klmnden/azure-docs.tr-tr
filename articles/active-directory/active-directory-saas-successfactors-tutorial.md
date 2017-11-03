---
title: "Öğretici: Azure Active Directory Tümleştirme ile SuccessFactors | Microsoft Docs"
description: "Çoklu oturum açma, otomatik sağlama ve daha fazla etkinleştirmek için Azure Active Directory ile SuccessFactors kullanmayı öğrenin!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 32bd8898-c2d2-4aa7-8c46-f1f5c2aa05f1
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/21/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: e85a38ccbe25263ac42bc76351416b023fb77c87
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-successfactors"></a>Öğretici: Azure Active Directory Tümleştirme SuccessFactors ile
Bu öğreticinin amacı SuccessFactors Azure Active Directory (Azure AD) ile tümleştirme Göster sağlamaktır.

SuccessFactors Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* SuccessFactors erişimi, Azure AD'de kontrol edebilirsiniz
* Otomatik olarak için SuccessFactors (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
* Hesaplarınızı bir merkezi konumda - Klasik Azure portalı Yönet

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar
Azure AD tümleştirme SuccessFactors ile yapılandırmak için aşağıdaki öğeleri gerekir:

* Geçerli bir Azure aboneliği
* Bir kiracı SuccessFactors içinde

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

1. Galeriden SuccessFactors ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-successfactors-from-the-gallery"></a>Galeriden SuccessFactors ekleme
Azure AD SuccessFactors tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden SuccessFactors eklemeniz gerekir.

**Galeriden SuccessFactors eklemek için aşağıdaki adımları gerçekleştirin:**

1. Azure Klasik portalında, sol gezinti bölmesinde, tıklatın **Active Directory**.
   
    ![Çoklu oturum açmayı yapılandırma][1]
2. Gelen **Directory** listesinde, directory tümleştirmesini etkinleştirmek istediğiniz dizini seçin.
3. Dizin görünümünde uygulamaları görünümü açmak için **uygulamaları** üst menüde.
   
    ![Çoklu oturum açmayı yapılandırma][2]
4. Tıklatın **Ekle** sayfanın sonundaki.
   
    ![Uygulamalar][3]
5. Üzerinde **ne yapmak istiyorsunuz** iletişim kutusunda, tıklatın **Galeriden bir uygulama eklemek**.
   
    ![Çoklu oturum açmayı yapılandırma][4]
6. İçinde **arama kutusu**, türü **SuccessFactors**.
   
    ![Çoklu oturum açmayı yapılandırma][5]
7. Sonuçlar panelinde seçin **SuccessFactors**ve ardından **tam** uygulama eklemek için.
   
    ![Çoklu oturum açmayı yapılandırma][6]

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümün amacı, size nasıl yapılandırılacağı ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı SuccessFactors test göstermektir.

Tekli çalışmaya oturum için Azure AD ne SuccessFactors Azure AD'de bir kullanıcıya karşılık gelen kullanıcı olduğunu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının SuccessFactors ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Bu bağlantı değeri atayarak ilişkisi **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** SuccessFactors içinde.

Yapılandırma ve Azure AD çoklu oturum açma SuccessFactors ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[SuccessFactors test kullanıcısı oluşturma](#creating-a-successfactors-test-user)**  - Britta Simon, karşılık gelen her, Azure AD gösterimine bağlı SuccessFactors sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma
Bu bölümde, Azure AD çoklu oturum açma Klasik portalında etkinleştirin ve çoklu oturum açma SuccessFactors uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile SuccessFactors yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure Klasik portalında üzerinde **SuccessFactors** uygulama tümleştirmesi sayfasında, tıklatın **çoklu oturum açma özelliğini yapılandırın** açmak için **tek oturum yapılandırma üzerinde aktar** iletişim.
   
    ![Çoklu oturum açmayı yapılandırma][7]
2. Üzerinde **SuccessFactors için oturum açmasını nasıl istiyorsunuz** sayfasında, **Microsoft Azure AD çoklu oturum açma**ve ardından **sonraki**.
   
    ![Çoklu oturum açmayı yapılandırma][8]
3. Üzerinde **uygulama URL'sini Yapılandır** sayfasında, aşağıdaki adımları uygulayın ve ardından **sonraki**.
   
    ![Çoklu oturum açmayı yapılandırma][9]
   
    a. İçinde **oturum üzerinde URL'si** metin kutusuna, aşağıdaki desenleri birini kullanarak URL yazın: 
   
    |  |
    | --- |
    | `https://<company name>.successfactors.com/<company name>` |
    | `https://<company name>.sapsf.com/<company name>` |
    | `https://<company name>.successfactors.eu/<company name>` |
    | `https://<company name>.sapsf.eu` |
   
    b. İçinde **yanıt URL'si** metin kutusuna, aşağıdaki desenleri birini kullanarak URL yazın: 
   
    |  |
    | --- |
    | `https://<company name>.successfactors.com/<company name>` |
    | `https://<company name>.sapsf.com/<company name>` |
    | `https://<company name>.successfactors.eu/<company name>` |
    | `https://<company name>.sapsf.eu` |
    | `https://<company name>.sapsf.eu/<company name>` |
   
    c. **İleri**’ye tıklayın. 

    > [!NOTE]
    > Lütfen bu gerçek değerlerin olmadığına dikkat edin. Gerçek oturum üzerinde URL'si ve yanıt URL'si bu değerleri güncelleştirmeniz gerekir. Bu değerleri almak için başvurun [SuccessFactors destek ekibi](https://www.successfactors.com/en_us/support.html).

1. Üzerinde **çoklu oturum açma sırasında SuccessFactors yapılandırma** sayfasında, **indirme sertifika**ve yerel olarak bilgisayarınızda sertifika dosyasını kaydedin.
   
    ![Çoklu oturum açmayı yapılandırma][10]

2. Farklı web tarayıcısı penceresinde oturum açın, **SuccessFactors Yönetici portalı** yönetici olarak.

3. Ziyaret **uygulama güvenliği** ve yerel **tek oturum açma özelliğini**. 

4. Herhangi bir değer yerleştirin **sıfırlama belirteci** tıklatıp **Kaydet belirteci** SAML SSO'yu etkinleştirmek için.
   
    ![Çoklu oturum açma uygulama tarafında yapılandırma][11]

    > [!NOTE] 
    > Bu değer yalnızca açık/kapalı anahtar kullanılır. Herhangi bir değer kaydettiyseniz, SAML SSO açık'tır. Boş bir değer kaydettiyseniz SAML SSO Kapalı'dır.

1. Yerel ekran görüntüsü aşağıda ve aşağıdaki eylemleri gerçekleştirin.
   
    ![Çoklu oturum açma uygulama tarafında yapılandırma][12]
   
    a. Seçin **SAML v2 SSO** radyo düğmesi
   
    b. SAML sunduğundan taraf Name(e.g. SAml issuer + company name) ayarlayın.
   
    c. İçinde **SAML veren** textbox değeri put **veren URL'si** Azure AD Uygulama Yapılandırma Sihirbazı'ndan.
   
    d. Seçin **yanıt (müşteri oluşturulan/IDP/AP)** olarak **zorunlu imza gerektir**.
   
    e. Seçin **etkin** olarak **etkinleştirmek SAML bayrağı**.
   
    f. Seçin **Hayır** olarak **oturum açma isteği imza (BT oluşturulan/SP/RP)**.
   
    g. Seçin **tarayıcı/Post profili** olarak **SAML profili**.
   
    h. Seçin **Hayır** olarak **sertifika geçerli süresi zorunlu**.
   
    ı. İndirilen sertifika dosyasının içeriğini kopyalayın ve ardından yapıştırın **SAML doğrulama sertifikası** metin kutusu.

    > [!NOTE] 
    > Sertifika içeriği sahip başlamalıdır sertifika ve bitiş sertifika etiketler.

1. SAML V2'ye gidin ve ardından aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açma uygulama tarafında yapılandırma][13]
   
    a. Seçin **Evet** olarak **destek SP tarafından başlatılan genel oturum kapatma**.
   
    b. İçinde **genel oturum kapatma hizmeti URL'si (LogoutRequest hedef)** textbox değeri put **uzak oturum kapatma URL'si** Azure AD Uygulama Yapılandırma Sihirbazı'ndan.
   
    c. Seçin **Hayır** olarak **sp tüm NameID öğesi şifrelemek gerekir gerektiren**.
   
    d. Seçin **belirtilmeyen** olarak **NameID biçimi**.
   
    e. Seçin **Evet** olarak **etkinleştir sp tarafından başlatılan oturum açma (AuthnRequest)**.
   
    f. İçinde **şirket çapında veren olarak gönderme isteği** textbox değeri put **uzaktan oturum açma URL'si** Azure AD Uygulama Yapılandırma Sihirbazı'ndan.
2. Oturum açma kullanıcı adları yapmak istiyorsanız bu adımları uygulamadan büyük küçük harf duyarsız.
   
    a. Ziyaret **şirket ayarları**(yakınında alt).
   
    b. yakın onay kutusunu seçin **etkinleştirmek olmayan durumda duyarlı kullanıcıadı**.
   
    c.Click **kaydetmek**.
   
    ![Çoklu oturum açmayı yapılandırın][29]

    > [!NOTE] 
    > Bu ayarı etkinleştirmek çalışırsanız, yinelenen bir SAML oturum açma adı oluşturur, sistem denetler. Örneğin, müşterinin kullanıcı adları Kullanıcı1 hem Kullanıcı1 varsa. Büyük küçük harfe duyarlılığın hemen alma bu yinelemeleri yapar. Sistem, bir hata iletisi alır ve özelliği izin vermez. Müşteri, gerçekte farklı yazıldığından için kullanıcı adları birini değiştirmeniz gerekir. 

1. Klasik Azure portalı, çoklu oturum açma yapılandırması onay seçin ve ardından **tam** kapatmak için **tek oturum yapılandırma üzerinde aktar** iletişim.
   
    ![Uygulamalar][14]
2. Üzerinde **tek oturum açma onay** sayfasında, **tam**.
   
    ![Uygulamalar][15]

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Klasik portalda bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][16]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Klasik Azure portalı**, sol gezinti bölmesinde tıklatın **Active Directory**.
   
    ![Bir Azure AD test kullanıcısı oluşturma][17]
2. Gelen **Directory** listesinde, directory tümleştirmesini etkinleştirmek istediğiniz dizini seçin.
3. Üstteki menüde kullanıcıların listesini görüntülemek için tıklatın **kullanıcılar**.
   
    ![Bir Azure AD test kullanıcısı oluşturma][18]
4. Açmak için **Kullanıcı Ekle** iletişim kutusunda, araç çubuğunda alt tıklatın **Kullanıcı Ekle**.
   
    ![Bir Azure AD test kullanıcısı oluşturma][19]
5. Üzerinde **bu kullanıcı hakkında bize** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Bir Azure AD test kullanıcısı oluşturma][20]
   
    a. Kullanıcı türü olarak, kuruluşunuzdaki yeni kullanıcı seçin.
   
    b. Kullanıcı adı **textbox**, türü **BrittaSimon**.
   
    c. **İleri**’ye tıklayın.
6. Üzerinde **kullanıcı profili** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Bir Azure AD test kullanıcısı oluşturma][21]
   
    a. İçinde **ad** metin kutusuna, türü **Britta**.  
   
    b. İçinde **Soyadı** metin kutusuna, türü, **Simon**.
   
    c. İçinde **görünen adı** metin kutusuna, türü **Britta Simon**.
   
    d. İçinde **rol** listesinde **kullanıcı**.
   
    e. **İleri**’ye tıklayın.
7. Üzerinde **Get geçici parola** iletişim sayfasında, tıklatın **oluşturma**.
   
    ![Bir Azure AD test kullanıcısı oluşturma][22]
8. Üzerinde **Get geçici parola** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Bir Azure AD test kullanıcısı oluşturma][23]
   
    a. Değerini yazmak **yeni parola**.
   
    b. **Tamamla**’ya tıklayın.  

### <a name="creating-a-successfactors-test-user"></a>SuccessFactors test kullanıcısı oluşturma
Azure AD kullanıcıların SuccessFactors oturum etkinleştirmek için bunların SuccessFactors sağlanmalıdır.  
SuccessFactors söz konusu olduğunda, sağlama bir el ile bir görevdir.

SuccessFactors içinde oluşturulan kullanıcıların almak için başvurmanız gerekir [SuccessFactors destek ekibi](https://www.successfactors.com/en_us/support.html).

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama
Bu bölümün amacı Britta SuccessFactors için kendi erişim vererek, Azure çoklu oturum açma kullanılacak Simon için etkinleştirmektir.

![Kullanıcı atama][24]

**SuccessFactors için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Klasik portalı üzerinde dizin görünümünde uygulamaları görünümü açmak için tıklatın **uygulamaları** üst menüde.
   
    ![Kullanıcı atama][25]
2. Uygulamalar listesinde **SuccessFactors**.
   
    ![Çoklu oturum açmayı yapılandırın][26]
3. Üstteki menüde tıklatın **kullanıcılar**.
   
    ![Kullanıcı atama][27]
4. Kullanıcılar listesinden seçin **Britta Simon**.
5. Araç çubuğunda alt tıklatın **atamak**.
   
    ![Kullanıcı atama][28]

### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme
Bu bölümün amacı erişim paneli kullanılarak Azure AD çoklu oturum açma yapılandırmanızı test etmektir.

Erişim paneli SuccessFactors parçasında tıklattığınızda, otomatik olarak SuccessFactors uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar
* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[0]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_00.png
[1]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_01.png
[6]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_02.png
[7]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_03.png
[8]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_04.png
[9]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_05.png
[10]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_06.png

[11]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_07.png
[12]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_08.png
[13]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_09.png
[14]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_05.png
[15]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_06.png

[16]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_00.png
[17]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_01.png
[18]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_02.png
[19]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_03.png
[20]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_04.png
[21]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_05.png
[22]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_06.png
[23]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_07.png

[24]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_07.png
[25]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_08.png
[26]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_11.png
[27]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_09.png
[28]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_10.png
[29]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_10.png
