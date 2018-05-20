---
title: 'Öğretici: Azure Active Directory Tümleştirme ile RunMyProcess | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile RunMyProcess arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: d31f7395-048b-4a61-9505-5acf9fc68d9b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 9232cbcef0bb445e8fc09475feaa2251afd3d3fb
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-runmyprocess"></a>Öğretici: Azure Active Directory Tümleştirme RunMyProcess ile

Bu öğreticide, Azure Active Directory (Azure AD) ile RunMyProcess tümleştirmek öğrenin.

RunMyProcess Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- RunMyProcess erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için RunMyProcess (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme RunMyProcess ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir RunMyProcess çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, burada bir aylık deneme elde edebilirsiniz:[deneme teklifi](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden RunMyProcess ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-runmyprocess-from-the-gallery"></a>Galeriden RunMyProcess ekleme
Azure AD RunMyProcess tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden RunMyProcess eklemeniz gerekir.

**Galeriden RunMyProcess eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **RunMyProcess**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_search.png)

5. Sonuçlar panelinde seçin **RunMyProcess**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı RunMyProcess sınayın.

Tekli çalışmaya oturum için Azure AD RunMyProcess karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının RunMyProcess ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

RunMyProcess içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma RunMyProcess ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[RunMyProcess test kullanıcısı oluşturma](#creating-a-runmyprocess-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı RunMyProcess sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma RunMyProcess uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile RunMyProcess yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **RunMyProcess** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_samlbase.png)

3. Üzerinde **RunMyProcess etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_url.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://live.runmyprocess.com/live/<tenant id>`

    > [!NOTE] 
    > Değer gerçek değil. Değerin gerçek oturum açma URL'si ile güncelleştirin. Kişi [RunMyProcess istemci destek ekibi](mailto:support@runmyprocess.com) değeri alınamıyor. 

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_400.png)

6. Üzerinde **RunMyProcess yapılandırma** 'yi tıklatın **yapılandırma RunMyProcess** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_configure.png) 

7. Farklı web tarayıcısı penceresinde RunMyProcess kiracınız yönetici olarak oturum.

8. Sol gezinti panelinde tıklatın **hesap** seçip **yapılandırma**.
   
    ![Çoklu oturum açma uygulama tarafında yapılandırma](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_001.png)

9. Git **kimlik doğrulama yöntemini** bölümünde ve aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açma uygulama tarafında yapılandırma](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_002.png)

    a. Olarak **yöntemi**seçin **Samlv2 SSO'su**. 

    b. İçinde **SSO yeniden yönlendirme** metin değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si**, Azure portalından kopyalanan.

    c. İçinde **oturumu kapatıp yeniden yönlendirme** metin değerini yapıştırın **Sign-Out URL**, Azure portalından kopyalanan.

    d. İçinde **ad kimliği biçimi** metin değerini yazın **ad tanımlayıcısı biçimi** olarak **urn: OASIS: adları: tc: SAML:1.1:nameid-biçimi: emailAddress**.

    e. İndirilen sertifika dosyasının içeriğini kopyalayın ve ardından yapıştırın **sertifika** metin kutusu. 
 
    f. Tıklatın **kaydetmek** simgesi.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-runmyprocess-test-user"></a>RunMyProcess test kullanıcısı oluşturma

Azure AD kullanıcıları için RunMyProcess oturum açmak etkinleştirmek için bunların RunMyProcess sağlanmalıdır. RunMyProcess söz konusu olduğunda, sağlama bir el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. RunMyProcess şirket sitenize yönetici olarak oturum açın.

2. Tıklatın **hesap** seçip **kullanıcılar** sol gezinti panelinde, ardından **yeni kullanıcı**.
   
    ![Yeni kullanıcı](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_003.png "yeni kullanıcı")

3. İçinde **kullanıcı ayarları** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Profil](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_004.png "profili") 
  
    a. Tür **adı** ve **e-posta** geçerli bir Azure AD hesabının istediğiniz ilgili metin kutularına sağlamayı. 

    b. Seçin bir **IDE dil**, **dil**, ve **profil**. 

    c. Seçin **Gönder hesap oluşturma e-posta bana**. 

    d. **Kaydet**’e tıklayın.
   
    >[!NOTE]
    >API tarafından RunMyProcess sağlamak için Azure Active Directory kullanıcı hesapları sağlanan veya herhangi diğer RunMyProcess kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz. 
    > 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta RunMyProcess için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**RunMyProcess için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **RunMyProcess**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümün amacı erişim paneli kullanılarak Azure AD SSO yapılandırmanızı test etmektir.

Erişim paneli RunMyProcess parçasında tıklattığınızda, otomatik olarak RunMyProcess uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_203.png

