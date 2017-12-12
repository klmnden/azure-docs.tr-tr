---
title: "Öğretici: Azure Active Directory Tümleştirme ile Asana | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile Asana arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 837e38fe-8f55-475c-87f4-6394dc1fee2b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: jeedes
ms.openlocfilehash: 8058dcd397e5f81f4a8c8cd1845353fd789f604b
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-asana"></a>Öğretici: Azure Active Directory Tümleştirme Asana ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Asana tümleştirmek öğrenin.

Asana Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Asana erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için Asana (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Asana ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Asana çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Asana ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-asana-from-the-gallery"></a>Galeriden Asana ekleme
Azure AD Asana tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Asana eklemeniz gerekir.

**Galeriden Asana eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Asana**seçin **Asana** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-asana-tutorial/tutorial_asana_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı Asana ile test etme

Tekli çalışmaya oturum için Azure AD Asana karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Asana ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Asana içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Asana ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir Asana test kullanıcısı oluşturma](#create-an-asana-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Asana sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Asana uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Asana yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Asana** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-asana-tutorial/tutorial_asana_samlbase.png)

3. Üzerinde **Asana etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Asana etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-asana-tutorial/tutorial_asana_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, türü URL'si:`https://app.asana.com/`

    b. İçinde **tanımlayıcısı** metin kutusuna, tür değeri:`https://app.asana.com/`
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-asana-tutorial/tutorial_asana_certificate.png)
    
5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-asana-tutorial/tutorial_general_400.png)

6. Üzerinde **Asana yapılandırma** 'yi tıklatın **yapılandırma Asana** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Asana yapılandırma](./media/active-directory-saas-asana-tutorial/tutorial_asana_configure.png) 

7. Farklı bir tarayıcı penceresinde Asana uygulamanıza oturum. SSO Asana yapılandırmak için ekranın sağ üst köşedeki çalışma adına tıklayarak çalışma alanı ayarları erişin. Ardından, tıklatın  **\<çalışma alanı adınız\> ayarları**. 
   
    ![Asana sso ayarları](./media/active-directory-saas-asana-tutorial/tutorial_asana_09.png)

8. Üzerinde **kuruluş ayarları** penceresinde tıklatın **Yönetim**. Ardından **üyeleri gerekir oturum SAML** SSO yapılandırmasını etkinleştirmek için. Aşağıdaki gerçekleştirme adımları:
   
    ![Çoklu oturum açma kuruluş ayarlarını yapılandırma](./media/active-directory-saas-asana-tutorial/tutorial_asana_10.png)  

     a. İçinde **oturum açma sayfası URL'si** metin kutusuna, Yapıştır **SAML çoklu oturum açma hizmet URL'si**.

     b. Azure portalından indirdiğiniz sertifikayı sağ tıklatın, sonra sertifika dosyasını Not Defteri'nde veya tercih edilen metin düzenleyiciyi kullanarak açın. Başlangıç ve bitiş sertifika başlık arasında içerik kopyalamak ve yapıştırmak **X.509 sertifikası** metin kutusu.

9. **Kaydet** düğmesine tıklayın. Git [SSO'yu ayarlamak için Asana Kılavuzu](https://asana.com/guide/help/premium/authentication#gl-saml) daha fazla yardıma ihtiyacınız varsa.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-asana-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-asana-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-asana-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Ekle düğmesi](./media/active-directory-saas-asana-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="create-an-asana-test-user"></a>Bir Asana test kullanıcısı oluşturma

Bu bölümde, Asana içinde Britta Simon adlı bir kullanıcı oluşturun.

1. Üzerinde **Asana**gidin **takımlar** Sol paneldeki bölümü. Artı düğmesini tıklatın. 
   
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-asana-tutorial/tutorial_asana_12.png) 

2. E-posta türü britta.simon@contoso.com metin kutusuna ve ardından **davet**.

3. Tıklatın **Gönder davet**. Yeni kullanıcı bir e-posta kendi e-posta dikkate alır. Aynen oluşturun ve hesabı doğrulamak gerekir.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Asana için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200]

**Asana için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Asana**.

    ![Uygulamalar listesinde Asana bağlantı](./media/active-directory-saas-asana-tutorial/tutorial_asana_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümün amacı, Azure AD çoklu oturum açmayı test etmektir.

Asana oturum açma sayfasına gidin. E-posta adresi metin kutusuna e-posta adresi eklemek britta.simon@contoso.com. Parola metin kutusunu boş bırakın ve ardından **oturum**. Azure AD oturum açma sayfasına yeniden yönlendirilir. Azure AD kimlik bilgilerinizi tamamlayın. Şimdi, Asana üzerinde oturum açtınız.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-asana-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-asana-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-asana-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-asana-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-asana-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-asana-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-asana-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-asana-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-asana-tutorial/tutorial_general_203.png
[10]: ./media/active-directory-saas-asana-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-asana-tutorial/tutorial_general_070.png
