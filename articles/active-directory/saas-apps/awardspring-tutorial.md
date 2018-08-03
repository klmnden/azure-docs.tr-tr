---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle AwardSpring | Microsoft Docs'
description: Azure Active Directory ve AwardSpring arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 2f115be6-4fbe-42aa-9319-7462e7a75736
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2018
ms.author: jeedes
ms.openlocfilehash: f75c14989b46721e7043c06086cac02222f452a5
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39431326"
---
# <a name="tutorial-azure-active-directory-integration-with-awardspring"></a>Öğretici: Azure Active Directory AwardSpring ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile AwardSpring tümleştirme konusunda bilgi edinin.

Azure AD ile AwardSpring tümleştirme ile aşağıdaki avantajları sağlar:

- AwardSpring erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için AwardSpring (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile AwardSpring yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik bir AwardSpring çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden AwardSpring ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-awardspring-from-the-gallery"></a>Galeriden AwardSpring ekleme
Azure AD'de AwardSpring tümleştirmesini yapılandırmak için AwardSpring Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden AwardSpring eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **AwardSpring**seçin **AwardSpring** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde AwardSpring](./media/awardspring-tutorial/tutorial_awardspring_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı AwardSpring sınayın.

Tek iş için oturum açma için Azure AD ne AwardSpring karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının AwardSpring ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma AwardSpring ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Bir AwardSpring test kullanıcısı oluşturma](#create-an-awardspring-test-user)**  - kullanıcı Azure AD gösterimini bağlı AwardSpring Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve AwardSpring uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile AwardSpring yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **AwardSpring** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/awardspring-tutorial/tutorial_awardspring_samlbase.png)

1. Üzerinde **AwardSpring etki alanı ve URL'ler** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** başlatılan modu:

    ![AwardSpring etki alanı ve URL'ler tek oturum açma bilgileri](./media/awardspring-tutorial/tutorial_awardspring_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.awardspring.com/SignIn/SamlMetaData`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.awardspring.com/SignIn/SamlAcs`

1. Denetleme **Gelişmiş URL ayarlarını göster** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![AwardSpring etki alanı ve URL'ler tek oturum açma bilgileri](./media/awardspring-tutorial/tutorial_awardspring_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.awardspring.com/`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [AwardSpring istemci Destek ekibine](mailto:support@awardspring.com) bu değerleri almak için. 

1. AwardSpring uygulama belirli bir biçimde SAML onaylamalarını bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** uygulama tümleştirme sayfasında bölümü. Aşağıdaki ekran görüntüsü bunun bir örneği gösterilmektedir.
    
    ![Çoklu oturum açmayı yapılandırın](./media/awardspring-tutorial/tutorial_awardSpring_attribute.png)

1. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, SAML belirteci özniteliği yukarıdaki görüntüde gösterilen şekilde yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı | Öznitelik Değeri |
    | ---------------| --------------- |    
    | Ad | User.givenName |
    | Soyadı | User.surname |
    | Email | User.Mail |
    | Kullanıcı adı | User.userprinicipalname |
    | StudentID | < Öğrenci Kimliği > |

    > [!NOTE]
    > StudentID öznitelik geri Taleplerde geçilmesi gereken gerçek Öğrenci Kimliği ile eşlenir. İlgili kişi [AwardSpring istemci Destek ekibine](mailto:support@awardspring.com) bu değeri alınamıyor.

    a. Mevcut öznitelikleri kaldırın ve yeni özellikler ekleyin. Tıklayın **eklemek agentconfigutil** açmak için **öznitelik Ekle** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/awardspring-tutorial/tutorial_attribute_04.png)

    ![Çoklu oturum açmayı yapılandırın](./media/awardspring-tutorial/tutorial_attribute_05.png)
    
    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.
    
    c. Gelen **değer** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    d. Bırakın **Namespace** boş.
    
    d. Tıklayın **Tamam**

1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/awardspring-tutorial/tutorial_awardspring_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/awardspring-tutorial/tutorial_general_400.png)
    
1. Çoklu oturum açmayı yapılandırma **AwardSpring** tarafı, indirilen göndermek için ihtiyacınız **meta veri XML** için [AwardSpring Destek ekibine](mailto:support@awardspring.com). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/awardspring-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/awardspring-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/awardspring-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/awardspring-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-an-awardspring-test-user"></a>Bir AwardSpring test kullanıcısı oluşturma

Bu bölümün amacı AwardSpring Britta Simon adlı bir kullanıcı oluşturmaktır. AwardSpring tam zamanında sağlama, varsayılan olarak etkin olan destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Yeni bir kullanıcı, henüz yoksa AwardSpring erişme denemesi sırasında oluşturulur.

>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [AwardSpring Destek ekibine](maito:support@awardspring.com).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için AwardSpring erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon AwardSpring için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **AwardSpring**.

    ![Uygulamalar listesinde AwardSpring bağlantı](./media/awardspring-tutorial/tutorial_awardspring_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde AwardSpring kutucuğa tıkladığınızda, otomatik olarak AwardSpring uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/awardspring-tutorial/tutorial_general_01.png
[2]: ./media/awardspring-tutorial/tutorial_general_02.png
[3]: ./media/awardspring-tutorial/tutorial_general_03.png
[4]: ./media/awardspring-tutorial/tutorial_general_04.png

[100]: ./media/awardspring-tutorial/tutorial_general_100.png

[200]: ./media/awardspring-tutorial/tutorial_general_200.png
[201]: ./media/awardspring-tutorial/tutorial_general_201.png
[202]: ./media/awardspring-tutorial/tutorial_general_202.png
[203]: ./media/awardspring-tutorial/tutorial_general_203.png

