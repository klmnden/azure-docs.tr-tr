---
title: 'Öğretici: Azure Active Directory Tümleştirme bağdaştırıcısıyla Five9 artı (CTI, kişi Center aracıları) | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory Five9 artı bağdaştırıcı (CTI, kişi Center aracıları) arasındaki yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 88dc82ab-be0b-4017-8335-c47d00775d7b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: jeedes
ms.openlocfilehash: d5c6b2c658a899b3c4363803dc3858cc2b6bab46
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-five9-plus-adapter-cti-contact-center-agents"></a>Öğretici: Azure Active Directory Tümleştirme bağdaştırıcısıyla Five9 artı (CTI, kişi Center aracıları)

Bu öğreticide, Azure Active Directory (Azure AD) ile Five9 artı bağdaştırıcı (CTI, kişi Center aracıları) tümleştirme öğrenin.

Five9 artı bağdaştırıcı (CTI, kişi Center aracıları) Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Five9 artı bağdaştırıcı (CTI, kişi Center aracıları) erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak Five9 artı bağdaştırıcısına (CTI, kişi Center aracıları) açan kullanıcılarınıza etkinleştirebilirsiniz (çoklu oturum açma) Azure AD hesaplarına sahip
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Five9 artı bağdaştırıcıyı ile (CTI, kişi Center aracıları) yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Five9 artı bağdaştırıcı (CTI, kişi Center aracıları) çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, burada bir aylık deneme elde edebilirsiniz: [deneme teklifi](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Five9 artı bağdaştırıcısı (CTI, kişi Center aracıları) ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-five9-plus-adapter-cti-contact-center-agents-from-the-gallery"></a>Galeriden Five9 artı bağdaştırıcısı (CTI, kişi Center aracıları) ekleme
Azure AD ile tümleştirme Five9 artı bağdaştırıcısının (CTI, kişi Center aracıları) yapılandırmak için Five9 artı (CTI, kişi Center aracıları) eklemek Galeriden yönetilen SaaS uygulamaları listenize gerekir.

**Five9 artı (CTI, kişi Center aracıları) Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Five9 artı bağdaştırıcı (CTI, kişi Center aracıları)**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-five9-tutorial/tutorial_five9_search.png)

5. Sonuçlar panelinde seçin **Five9 artı bağdaştırıcı (CTI, kişi Center aracıları)** ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-five9-tutorial/tutorial_five9_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Five9 artı "Britta Simon" adlı bir test kullanıcıya bağlı bağdaştırıcı (CTI, kişi Center aracıları) ile test etme.

Tekli çalışmaya oturum için Azure AD Five9 artı bağdaştırıcı (CTI, kişi Center aracıları) karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı Five9 artı bağdaştırıcısında (CTI, kişi Center aracıları) arasında bir bağlantı ilişkisi kurulması gerekir.

Five9 artı bağdaştırıcısında (CTI, kişi Center aracıları) değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırmak ve Azure AD çoklu oturum açma bağdaştırıcısıyla Five9 artı (CTI, kişi Center aracıları) sınamak için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Five9 artı bağdaştırıcı (CTI, kişi Center aracıları) test kullanıcısı oluşturma](#creating-a-five9-plus-adapter-cti-contact-center-agents-test-user)**  - Britta Simon, karşılık gelen Five9 yanı sıra kullanıcı Azure AD gösterimini bağlı bağdaştırıcı (CTI, kişi Center aracıları) sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Five9 artı bağdaştırıcı (CTI, kişi Center aracıları) uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Five9 artı bağdaştırıcıyı ile (CTI, kişi Center aracıları) yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Five9 artı bağdaştırıcı (CTI, kişi Center aracıları)** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-five9-tutorial/tutorial_five9_samlbase.png)

3. Üzerinde **Five9 artı bağdaştırıcı (CTI, kişi Center aracıları) etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-five9-tutorial/tutorial_five9_url.png)
    
    a. İçinde **tanımlayıcısı** metin kutusuna, aşağıdaki desenleri kullanarak URL'sini yazın:

    |    Ortam      |       URL'si      |
    | :-- | :-- |
    | "Five9 Plus bağdaştırıcısı Microsoft Dynamics CRM için" | `https://app.five9.com/appsvcs/saml/metadata/alias/msdc` |
    | "Five9 Plus bağdaştırıcısı Zendesk için" | `https://app.five9.com/appsvcs/saml/metadata/alias/zd` |
    | "Five9 Plus bağdaştırıcısı Aracısı Masaüstü araç seti için" | `https://app.five9.com/appsvcs/saml/metadata/alias/adt` |

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:

    |      Ortam     |      URL'si      |
    | :--                  | :--           |
    | "Five9 Plus bağdaştırıcısı Microsoft Dynamics CRM için" | `https://app.five9.com/appsvcs/saml/SSO/alias/msdc` |
    | "Five9 Plus bağdaştırıcısı Zendesk için" | `https://app.five9.com/appsvcs/saml/SSO/alias/zd` |
    | "Five9 Plus bağdaştırıcısı Aracısı Masaüstü araç seti için" | `https://app.five9.com/appsvcs/saml/SSO/alias/adt` |

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-five9-tutorial/tutorial_five9_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-five9-tutorial/tutorial_general_400.png)

6. Üzerinde **Five9 artı bağdaştırıcı (CTI, kişi Center aracıları) yapılandırmasını** 'yi tıklatın **yapılandırma Five9 artı bağdaştırıcı (CTI, kişi Center aracıları)** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-five9-tutorial/tutorial_five9_configure.png) 

7. Çoklu oturum açma yapılandırmak için **Five9 artı bağdaştırıcı (CTI, kişi Center aracıları)** yan, indirilen göndermek için ihtiyacınız **Certificate(Base64), Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** için [Five9 artı bağdaştırıcı (CTI, kişi Center aracıları) destek ekibi](https://www.five9.com/about/contact). Ayrıca, SSO daha fazla yapılandırmak için lütfen izleyin aşağıdaki adımları bağdaştırıcısı göre:

    a. "Five9 artı bağdaştırıcısı Aracısı Masaüstü araç seti için" Yönetici Kılavuzu: [http://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf](http://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf)
    
    b. "Five9 artı bağdaştırıcısı Microsoft Dynamics CRM için" Yönetici Kılavuzu: [http://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf](http://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf)
    
    c. "Five9 artı bağdaştırıcısı Zendesk için" Yönetici Kılavuzu: [http://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf](http://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf)


> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-five9-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-five9-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-five9-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-five9-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-five9-plus-adapter-cti-contact-center-agents-test-user"></a>Five9 artı bağdaştırıcı (CTI, kişi Center aracıları) test kullanıcısı oluşturma

Bu bölümde, Britta Simon Five9 artı bağdaştırıcısı (CTI, kişi Center aracıları) adlı bir kullanıcı oluşturun. Çalışmak [Five9 artı bağdaştırıcı (CTI, kişi Center aracıları) destek ekibi](https://www.five9.com/about/contact) Five9 artı bağdaştırıcı (CTI, kişi Center aracıları) platform kullanıcıları eklemek için. Kullanıcıların oluşturulan ve çoklu oturum açma kullanmadan önce etkinleştirilmelidir.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Five9 artı bağdaştırıcısına (CTI, kişi Center aracıları) erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Britta Simon Five9 artı bağdaştırıcısına (CTI, kişi Center aracıları) atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Five9 artı bağdaştırıcı (CTI, kişi Center aracıları)**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-five9-tutorial/tutorial_five9_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Five9 artı bağdaştırıcı (CTI, kişi Center aracıları) parçasında tıklattığınızda, otomatik olarak Five9 artı bağdaştırıcı (CTI, kişi Center aracıları) uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/active-directory-saas-five9-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-five9-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-five9-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-five9-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-five9-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-five9-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-five9-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-five9-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-five9-tutorial/tutorial_general_203.png

