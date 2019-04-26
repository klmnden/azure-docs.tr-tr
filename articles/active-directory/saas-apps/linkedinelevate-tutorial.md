---
title: 'Öğretici: LinkedIn yükseltmesine ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve LinkedIn yükseltmesine arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 2ad9941b-c574-42c3-bd0f-5d6ec68537ef
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6ca8e537f261b59fb4e069d47d24e21abbdeca46
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60259687"
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-elevate"></a>Öğretici: Azure Active Directory tümleştirmesiyle LinkedIn Yükselt

Bu öğreticide, LinkedIn yükseltmesine Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.

Azure AD ile tümleştirme yükseltmesine LinkedIn ile aşağıdaki avantajları sağlar:

- LinkedIn yükseltmesine erişimi, Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) için LinkedIn yükseltmesine açma, kullanıcılarınızın etkinleştirebilirsiniz
- Bir merkezi konumda - Azure Yönetim Portalı hesaplarınızı yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

LinkedIn yükseltmesine ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir LinkedIn yükseltmesine çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Bu gerekli olmadığı sürece üretim ortamınızı kullanmamanız gerekir.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin.
Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. LinkedIn yükseltmesine galeri ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-linkedin-elevate-from-the-gallery"></a>LinkedIn yükseltmesine galeri ekleme
LinkedIn yükseltmesine tümleştirmesini Azure AD'de yapılandırmak için LinkedIn yükseltmesine Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**LinkedIn yükseltmesine Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure Yönetim Portalı](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]

1. Tıklayın **Ekle** iletişim kutusunun üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **LinkedIn yükseltmesine**. Sonuçlar panelinde tıklayın **LinkedIn yükseltmesine** uygulama eklemek için.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/linkedinelevate-tutorial/tutorial-linkedinElevate_000.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı LinkedIn yükseltmesine ile test edin.

Tek çalışmak için oturum açma için Azure AD ne karşılık gelen kullanıcı LinkedIn yükseltmek için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili LinkedIn yükseltmesine kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Değerini atayarak bu bağlantı ilişki kurulduktan **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** içinde LinkedIn yükseltebilir.

Yapılandırma ve Azure AD çoklu oturum açma yükseltmesine LinkedIn ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Bir LinkedIn yükseltmesine test kullanıcısı oluşturma](#creating-a-linkedin-elevate-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure yönetim portalında etkinleştirin ve LinkedIn yükseltmesine uygulamanızda çoklu oturum açmayı yapılandırın.

**LinkedIn yükseltmesine ile Azure AD çoklu oturum açmayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure Yönetim Portalı'nda üzerinde **LinkedIn yükseltmesine** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda olarak **modu** seçin **SAML tabanlı oturum açma** için çoklu oturum açmayı etkinleştirme.

    ![Çoklu oturum açmayı yapılandırın](./media/linkedinelevate-tutorial/tutorial-linkedin_01.png)

1. Farklı bir web tarayıcı penceresinde LinkedIn yükseltmesine kiracınıza yönetici olarak oturum.

1. İçinde **hesap Merkezi**, tıklayın **genel ayarları** altında **ayarları**. Ayrıca, seçin **yükseltme - AAD Test yükseltmesine** aşağı açılan listeden.

    ![Çoklu oturum açmayı yapılandırın](./media/linkedinelevate-tutorial/tutorial_linkedin_admin_01.png)

1. Tıklayarak **veya yüklemek ve tek tek alanları formdan kopyalamak için burayı tıklatın** kopyalayıp **varlık kimliği** ve **onaylama tüketici erişim (ACS) URL'si**

    ![Çoklu oturum açmayı yapılandırın](./media/linkedinelevate-tutorial/tutorial_linkedin_admin_03.png)

1. Azure portalında altında **LinkedIn yükseltme etki alanı ve URL'ler**, çoklu oturum AÇMAYA yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP tarafından başlatılan** modu

    ![Çoklu oturum açmayı yapılandırın](./media/linkedinelevate-tutorial/tutorial_linkedin_signon_01.png)

    a. İçinde **tanımlayıcı** metin girin **varlık kimliği** LinkedIn portaldan kopyaladığınız 

    b. İçinde **yanıt URL'si** metin girin **onaylama tüketici erişim (ACS) URL'si** LinkedIn portaldan kopyaladığınız

1. Çoklu oturum AÇMAYA yapılandırmak istiyorsanız **SP tarafından başlatılan**, ardından yapılandırma bölümü Göster Gelişmiş URL ayarını seçeneğe tıklayın ve oturum açma URL aşağıdaki desenle yapılandırın:

    `https://www.linkedin.com/checkpoint/enterprise/login/<AccountId>?application=elevate&applicationInstanceId=<InstanceId>` 

    ![Çoklu oturum açmayı yapılandırın](./media/linkedinelevate-tutorial/tutorial_linkedin_signon_02.png) 

1. LinkedIn yükseltmesine uygulamanız SAML onaylamalarını özel öznitelik eşlemelerini SAML belirteci öznitelikleri yapılandırmanıza ekleyin gerektiren belirli bir biçimde bekliyor. Aşağıdaki ekran görüntüsü bunun bir örneği gösterilmektedir. Varsayılan değer olan **kullanıcı tanımlayıcısı** olduğu **user.userprincipalname** ancak LinkedIn yükseltebilir, bekliyor bu kullanıcının e-posta adresi ile eşlenmiş. Bunun için kullanabileceğiniz **user.mail** listeden öznitelik veya kuruluş yapılandırmanıza göre uygun öznitelik değeri kullanın.

    ![Çoklu oturum açmayı yapılandırın](./media/linkedinelevate-tutorial/updateusermail.png)

1. İçinde **kullanıcı öznitelikleri** bölümünde **görünümü ve diğer tüm kullanıcı özniteliklerini düzenleyin** ve özniteliklerini ayarlayın. Adlı başka bir talep eklemeniz **departmanı** ve değer için eşlenmesi gereken **user.department**.

    | Öznitelik Adı | Öznitelik Değeri |
    | --- | --- |
    | Bölüm| User.Department |

      ![Bir Azure AD test kullanıcısı oluşturma](./media/linkedinelevate-tutorial/userattribute.png)

      a. Öznitelik öznitelik Ayrıntılar sayfasını açmak için tıklayın, aşağıdaki - gösterildiği departmanı öznitelik Ekle

      ![Bir Azure AD test kullanıcısı oluşturma](./media/linkedinelevate-tutorial/adduserattribute.png)

      b. Tıklayarak **Tamam** öznitelik kaydetmek için.

      c. Özniteliğin adını değiştirmek **emailaddress** için **e-posta**.

1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda XML dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/linkedinelevate-tutorial/tutorial-linkedinElevate_certificate.png) 

1. **Kaydet**’e tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/linkedinelevate-tutorial/tutorial_general_400.png)

1. Git **LinkedIn yönetici ayarları** bölümü. Yalnızca karşıya yükleme XML dosyası seçeneğine tıklayıp Azure portalından indirilen XML dosyasını karşıya yükleyin.

    ![Çoklu oturum açmayı yapılandırın](./media/linkedinelevate-tutorial/tutorial_linkedin_metadata_03.png)

1. Tıklayın **üzerinde** SSO'yu etkinleştirmek üzere. SSO durumu değişir **bağlı** için **bağlandı**

    ![Çoklu oturum açmayı yapılandırın](./media/linkedinelevate-tutorial/tutorial_linkedin_admin_05.png)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, bir test kullanıcısı Britta Simon adlı Azure Yönetim Portalı'nda oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure Yönetim Portalı**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/linkedinelevate-tutorial/create_aaduser_01.png) 

1. Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar** kullanıcılar listesini görüntüleyin.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/linkedinelevate-tutorial/create_aaduser_02.png) 

1. İletişim kutusunun en üstünde tıklayın **Ekle** açmak için **kullanıcı** iletişim.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/linkedinelevate-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Bir Azure AD test kullanıcısı oluşturma](./media/linkedinelevate-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.

### <a name="creating-a-linkedin-elevate-test-user"></a>Bir LinkedIn yükseltmesine test kullanıcısı oluşturma

LinkedIn yükseltmesine uygulama zamanı kullanıcı sağlamayı ve kimlik doğrulaması kullanıcılar uygulamaya otomatik olarak oluşturulacak sonra sadece destekler. Yönetici ayarları LinkedIn yükseltmesine portal Çevir ' anahtar sayfasında **lisansları otomatik olarak ata** etkin tam zamanında sağlama ve bu da bir lisans kullanıcıya atar. LinkedIn yükseltmesine de destekler otomatik kullanıcı hazırlama, daha fazla ayrıntı bulabilirsiniz [burada](linkedinelevate-provisioning-tutorial.md) otomatik kullanıcı sağlamayı yapılandırma.

   ![Bir Azure AD test kullanıcısı oluşturma](./media/linkedinelevate-tutorial/LinkedinUserprovswitch.png)

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, LinkedIn yükseltmek için erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon LinkedIn yükseltmek için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure Yönetim Portalı'nda uygulamaları görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

1. Uygulamalar listesinde **LinkedIn yükseltmesine**.

    ![Çoklu oturum açmayı yapılandırın](./media/linkedinelevate-tutorial/tutorial-linkedinElevate_0001.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.

### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli LinkedIn yükseltmesine kutucuğa tıkladığınızda, Azure oturum açma sayfası almanız gerekir ve üzerinde başarılı oturum açma işleminden sonra, LinkedIn yükseltmesine uygulamanıza almanız gerekir.

## <a name="additional-resources"></a>Ek kaynaklar

* [Öğretici: Azure Active Directory ile otomatik kullanıcı hazırlama için LinkedIn yükseltmesine yapılandırma](linkedinelevate-provisioning-tutorial.md)
* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
* [Kullanıcı sağlamayı yapılandırma](linkedinelevate-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/linkedinElevate-tutorial/tutorial_general_01.png
[2]: ./media/linkedinElevate-tutorial/tutorial_general_02.png
[3]: ./media/linkedinElevate-tutorial/tutorial_general_03.png
[4]: ./media/linkedinElevate-tutorial/tutorial_general_04.png

[100]: ./media/linkedinElevate-tutorial/tutorial_general_100.png

[200]: ./media/linkedinElevate-tutorial/tutorial_general_200.png
[201]: ./media/linkedinElevate-tutorial/tutorial_general_201.png
[202]: ./media/linkedinElevate-tutorial/tutorial_general_202.png
[203]: ./media/linkedinElevate-tutorial/tutorial_general_203.png
