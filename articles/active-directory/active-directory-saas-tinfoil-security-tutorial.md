---
title: 'Öğretici: TINFOIL SECURITY Azure Active Directory Tümleştirme | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile TINFOIL SECURITY arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: da02da92-e3b0-4c09-ad6c-180882b0f9f8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 96d7b75078fd1075d17d70ee677f28ba1bbb1576
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-tinfoil-security"></a>Öğretici: TINFOIL SECURITY Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile TINFOIL SECURITY tümleştirmek öğrenin.

TINFOIL SECURITY Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- TINFOIL güvenlik erişimi, Azure AD'de kontrol edebilirsiniz
- Azure AD hesaplarına otomatik olarak (çoklu oturum açma) için TINFOIL SECURITY açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

TINFOIL SECURITY ile Azure AD tümleştirme yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir TINFOIL SECURITY çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. TINFOIL SECURITY Galerisi'nden ekleme
2. Yapılandırma ve Azure AD çoklu oturum açmayı test etme

## <a name="add-tinfoil-security-from-the-gallery"></a>TINFOIL SECURITY Galerisi'nden ekleme
Azure AD TINFOIL SECURITY tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden TINFOIL SECURITY eklemeniz gerekir.

**TINFOIL SECURITY Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **TINFOIL SECURITY**seçin **TINFOIL SECURITY** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![TINFOIL SECURITY Galeriden](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma TINFOIL "Britta Simon" adlı bir test kullanıcı tabanlı güvenliği ile test etme.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen TINFOIL Security bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının TINFOIL SECURITY ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

TINFOIL güvenlik değeri atamak **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma TINFOIL SECURITY ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[TINFOIL SECURITY test kullanıcısı oluşturma](#create-a-tinfoil-security-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı TINFOIL SECURITY sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma TINFOIL SECURITY uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma TINFOIL SECURITY ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **TINFOIL SECURITY** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![SAML oturum açma tabanlı](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_samlbase.png)

3. Üzerinde **TINFOIL güvenlik etki alanı ve URL'leri** bölümü, kullanıcı gerekmez uygulama zaten Azure ile önceden tümleştirilmiş gibi tüm adımları gerçekleştirin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_url.png)


4. Üzerinde **SAML imzalama sertifikası** bölümünde, kopyalama **parmak İZİ** değeri.

    ![SAML imzalama sertifikası bölümü](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_certificate.png) 

5. Gerekli öznitelik eşlemelerini eklemek için aşağıdaki adımları gerçekleştirin:
    
    ![Öznitelikleri](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_attribute1.png "öznitelikleri")
    
    | Öznitelik Adı    |   Öznitelik Değeri |
    | ------------------- | -------------------- |
    | Hesap Kimliği | UXXXXXXXXXXXXX |
    
    a. Tıklatın **kullanıcı özniteliği eklemek**.
    
    ![Ekle özniteliği](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_attribute.png "öznitelikleri")
    
    ![Ekle özniteliği](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_addatt.png "öznitelikleri")
    
    b. İçinde **öznitelik adı** metin kutusuna, türü **AccountID**.
    
    c. İçinde **öznitelik değeri** metin kutusuna, daha sonra öğreticide alacak Yapıştır hesap kimliği değeri.
    
    d. **Tamam**’a tıklayın.    

6. Tıklatın **kaydetmek** düğmesi.

    ![Kaydet düğmesi](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_400.png)

7. Üzerinde **TINFOIL Güvenlik Yapılandırması** 'yi tıklatın **TINFOIL Güvenlik Yapılandırması** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![TINFOIL güvenlik yapılandırması](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_configure.png) 

8. Farklı web tarayıcısı penceresinde TINFOIL SECURITY şirket sitenize yönetici olarak oturum açın.

9. Üstteki araç çubuğunda tıklatın **hesabım**.
   
    ![Pano](./media/active-directory-saas-tinfoil-security-tutorial/ic798971.png "Panosu")

10. Tıklatın **güvenlik**.
   
    ![Güvenlik](./media/active-directory-saas-tinfoil-security-tutorial/ic798972.png "güvenlik")

11. Üzerinde **çoklu oturum açma** yapılandırma sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açma](./media/active-directory-saas-tinfoil-security-tutorial/ic798973.png "çoklu oturum açma")
   
    a. Seçin **etkinleştirmek SAML**.
   
    b. Tıklatın **el ile yapılandırma**.
   
    c. İçinde **SAML gönderme URL'sini** metin değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si** Azure portalından kopyalanan
   
    d. İçinde **SAML sertifika parmak izi** metin değerini yapıştırın **parmak izi** kopyalanan **SAML imzalama sertifikası** bölümü.
  
    e. Kopya **bilgisayarınızı hesap kimliği** değer ve değeri yapıştırın **öznitelik değeri** metin kutusu altında **özniteliği eklemek** Azure portalı bölümünde.
   
    f. **Kaydet**’e tıklayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-tinfoil-security-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Kullanıcıların ve grupların tüm kullanıcılar -> ](./media/active-directory-saas-tinfoil-security-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Kullanıcı](./media/active-directory-saas-tinfoil-security-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-tinfoil-security-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-tinfoil-security-test-user"></a>TINFOIL SECURITY test kullanıcısı oluşturma

Azure AD kullanıcıların TINFOIL SECURITY oturum etkinleştirmek için bunların TINFOIL SECURITY sağlanmalıdır. TINFOIL SECURITY söz konusu olduğunda, sağlama bir el ile bir görevdir.

**Sağlanan kullanıcı almak için aşağıdaki adımları gerçekleştirin:**

1. Kullanıcı bir kurumsal hesap parçasıysa, gerek [TINFOIL SECURITY Destek ekibine başvurun](https://www.tinfoilsecurity.com/contact) oluşturulmuş olan kullanıcı hesabı alınamadı.

2. Kullanıcı normal bir TINFOIL güvenlik SaaS kullanıcı ise, daha sonra kullanıcı bir ortak çalışanı herhangi bir kullanıcının siteleri için ekleyebilirsiniz. Bu yeni bir TINFOIL SECURITY kullanıcı hesabı oluşturmak için bir davet belirtilen e-posta göndermek için bir işlemi tetikler.

> [!NOTE]
> Azure AD kullanıcı hesaplarını sağlamak için herhangi bir TINFOIL SECURITY kullanıcı hesabı oluşturma araçlarını veya TINFOIL SECURITY tarafından sağlanan API'leri kullanabilirsiniz.
> 
> 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta TINFOIL SECURITY erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**TINFOIL SECURITY Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **TINFOIL SECURITY**.

    ![TINFOIL SECURITY seçin](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli TINFOIL SECURITY parçasında tıklattığınızda, otomatik olarak TINFOIL SECURITY uygulamanıza açan. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_203.png

