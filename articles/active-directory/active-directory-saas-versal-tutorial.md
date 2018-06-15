---
title: 'Öğretici: Azure Active Directory Tümleştirme fasından yapılan ile | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile fasından yapılan arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 5b2e53c0-61a3-4954-ae46-8c28c6368bfd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: jeedes
ms.openlocfilehash: a2710c7fa0d035cee8a16d10edf3d603443ad520
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34352064"
---
# <a name="tutorial-azure-active-directory-integration-with-versal"></a>Öğretici: Azure Active Directory Tümleştirme fasından yapılan ile

Bu öğreticide, Azure Active Directory (Azure AD) ile fasından yapılan tümleştirmek öğrenin.

Fasından yapılan Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Fasından yapılan erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak için fasından yapılan (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme fasından yapılan ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Versal çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden fasından yapılan ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-versal-from-the-gallery"></a>Galeriden fasından yapılan ekleme
Azure AD'ye fasından yapılan tümleştirmesini yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden fasından yapılan eklemeniz gerekir.

**Galeriden fasından yapılan eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **fasından yapılan**seçin **fasından yapılan** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde fasından yapılan](./media/active-directory-saas-versal-tutorial/tutorial_versal_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırmak ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı fasından yapılan sınayın.

Tekli çalışmaya oturum için Azure AD fasından yapılan karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının fasından yapılan ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Değeri fasından yapılan içinde atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma fasından yapılan ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Versal test kullanıcısı oluşturma](#create-a-versal-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı fasından yapılan sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Versal uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile fasından yapılan yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **fasından yapılan** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-versal-tutorial/tutorial_versal_samlbase.png)

3. Üzerinde **fasından yapılan etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Oturum açma bilgileri Versal etki alanı ve URL'leri tek](./media/active-directory-saas-versal-tutorial/tutorial_versal_url.png)

    a. İçinde **tanımlayıcısı** metin değeri yazın: `VERSAL`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://versal.com/sso/saml/orgs/<organization_id>`

    > [!NOTE] 
    > Yanıt URL'si değeri gerçek değil. Bu değer ile gerçek yanıt URL'si güncelleştirin. Kişi [Versal destek ekibi](https://support.versal.com/hc/) bu değeri alınamıyor.
    
4. Uygulamanız için SAML belirteci öznitelikleri yapılandırmanızı özel öznitelik eşlemelerini eklemenizi gerektirir belirli bir biçimde SAML onaylar bekliyor. Aşağıdaki ekran görüntüsünde bunun bir örneği gösterir. Varsayılan değer olan **kullanıcı tanımlayıcısı** olan **user.userprincipalname** ancak **fasından yapılan** bu kullanıcının e-posta adresi ile eşlenecek bekliyor. Bunun için kullanabileceğiniz **user.mail** özniteliği listeden veya kuruluş yapılandırmanızı temel alarak uygun öznitelik değeri kullanın.
    
    ![Kullanıcı tanımlayıcısı açılır menüsü](./media/active-directory-saas-versal-tutorial/tutorial_versal_attribute.png)

5. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-versal-tutorial/tutorial_versal_certificate.png) 

6. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-versal-tutorial/tutorial_general_400.png)
    
7. Çoklu oturum açma yapılandırmak için **fasından yapılan** yan, indirilen göndermek için ihtiyacınız **meta veri XML** ve **SAML imzalama sertifikası** için [Versal destek ekibi ](https://support.versal.com/hc/). Bunlar Versal kuruluşunuz iki tarafta da ayarlamanızı SAML SSO bağlantınız için yapılandırır.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-versal-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-versal-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-versal-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-versal-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
  
### <a name="create-a-versal-test-user"></a>Versal test kullanıcısı oluşturma

Bu bölümde, içinde fasından yapılan Britta Simon adlı bir kullanıcı oluşturun. Lütfen izleyin [bir SAML oluşturmadan test kullanıcı](https://support.versal.com/hc/en-us/articles/115011672887-Creating-a-SAML-test-user) kuruluşunuzdaki Britta Simon kullanıcı oluşturmak için destek Kılavuzu. Kullanıcıların oluşturulan ve çoklu oturum açma kullanmadan önce fasından yapılan etkinleştirilmelidir. 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta fasından yapılan için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**İçin fasından yapılan Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **fasından yapılan**.

    ![Fasından yapılan bağlantı uygulamalar listesinde](./media/active-directory-saas-versal-tutorial/tutorial_versal_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Web sitenizi içinde katıştırılmış bir Versal indirmelere kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.
Lütfen bakın [katıştırma kuruluş kurslar](https://support.versal.com/hc/en-us/articles/203271866-Embedding-organizational-courses) **SAML çoklu oturum açma** Kılavuzu çoklu oturum açmayı Versal indirmelere Azure AD desteği ekleme hakkında yönergeler için destek. 

Bir indirmelere oluşturma, kuruluşunuzla paylaşın ve indirmelere katıştırma test etmek için yayımlama gerekecektir. Lütfen bakın [bir indirmelere oluşturma](https://support.versal.com/hc/en-us/articles/203722528-Create-a-course), [bir indirmelere yayımlama](https://support.versal.com/hc/en-us/articles/203753398-Publishing-a-course), ve [indirmelere ve öğrenen Yönetimi](https://support.versal.com/hc/en-us/articles/206029467-Course-and-learner-management) daha fazla bilgi için.  
                     

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/active-directory-saas-versal-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-versal-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-versal-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-versal-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-versal-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-versal-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-versal-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-versal-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-versal-tutorial/tutorial_general_203.png

