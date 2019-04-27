---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Veracode | Microsoft Docs'
description: Azure Active Directory ve Veracode arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 4fe78050-cb6d-4db9-96ec-58cc0779167f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e80a2e554d31ad85ddb1111f84a96e1814c3969f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60638908"
---
# <a name="tutorial-azure-active-directory-integration-with-veracode"></a>Öğretici: Veracode ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Veracode tümleştirme konusunda bilgi edinin.

Azure AD ile Veracode tümleştirme ile aşağıdaki avantajları sağlar:

- Veracode erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için Veracode (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Veracode yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik Veracode çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Veracode Ekle
1. Yapılandırma ve Azure AD çoklu oturum açmayı test etme

## <a name="add-veracode-from-the-gallery"></a>Galeriden Veracode Ekle
Azure AD'de Veracode tümleştirmesini yapılandırmak için Veracode Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Veracode eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **Veracode**seçin **Veracode** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Veracode](./media/veracode-tutorial/tutorial_veracode_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Veracode sınayın.

Tek iş için oturum açma için Azure AD ne Veracode karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Veracode ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Veracode içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Veracode ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Veracode test kullanıcısı oluşturma](#create-a-veracode-test-user)**  - kullanıcı Azure AD gösterimini bağlı Veracode Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Veracode uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Veracode yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Veracode** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/veracode-tutorial/tutorial_veracode_samlbase.png)

1. Üzerinde **Veracode etki alanı ve URL'ler** bölümü, kullanıcı gerekmez uygulama zaten Azure ile önceden tümleştirilmiştir gibi tüm adımları gerçekleştirin. 

    ![Çoklu oturum açmayı yapılandırın](./media/veracode-tutorial/tutorial_veracode_url.png)

1. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/veracode-tutorial/tutorial_veracode_certificate.png) 

1. Bu bölümün amacı, kullanıcıların Azure AD'de SAML protokolü temel alarak Federasyon kullanarak Veracode için kendi hesabıyla kimlik doğrulaması sağlamak nasıl anahat sağlamaktır.

    Özel öznitelik eşlemeleri eklemek gerektiren belirli bir biçimde SAML onaylamalarını Veracode uygulamanızı bekliyor, **saml belirteci öznitelikleri** yapılandırma. Aşağıdaki ekran görüntüsü bunun bir örneği gösterilmektedir.
    
    ![Öznitelikleri](./media/veracode-tutorial/tutorial_veracode_attr.png "öznitelikleri")

1. Gerekli öznitelik eşlemelerini eklemek için aşağıdaki adımları gerçekleştirin:

    | Öznitelik Adı | Öznitelik Değeri |
    |--- |--- |
    | firstName |User.givenName |
    | Soyadı |User.surname |
    | e-posta |User.Mail |
    
    a. Yukarıdaki tablodaki her veri satırı için tıklatın **kullanıcı özniteliğini eklemek**.
    
    ![Öznitelikleri](./media/veracode-tutorial/tutorial_veracode_addattr.png "öznitelikleri")
    
    ![Öznitelikleri](./media/veracode-tutorial/tutorial_veracode_addattr1.png "öznitelikleri")
    
    b. İçinde **öznitelik adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.
    
    c. İçinde **öznitelik değeri** metin kutusuna, bu satır için gösterilen öznitelik değerini seçin.
    
    d. **Tamam**’a tıklayın.

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/veracode-tutorial/tutorial_general_400.png)

1. Üzerinde **Veracode yapılandırma** bölümünde **yapılandırma Veracode** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML varlık kimliği** gelen **hızlı başvuru bölümü.**

    ![Veracode yapılandırma](./media/veracode-tutorial/tutorial_veracode_configure.png) 

1. Farklı bir web tarayıcı penceresinde Veracode şirket sitenize yönetici olarak oturum.

1. Üstteki menüden **ayarları**ve ardından **yönetici**.
   
    ![Yönetim](./media/veracode-tutorial/ic802911.png "Yönetim")

1. Tıklayın **SAML** sekmesi.

1. İçinde **kuruluş SAML ayarlarını** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Yönetim](./media/veracode-tutorial/ic802912.png "Yönetim")
   
    a.  İçinde **veren** metin değerini yapıştırın **SAML varlık kimliği** , Azure Portalı'ndan kopyaladığınız.
    
    b. Azure portalından indirilen sertifikanızı karşıya yüklemek için tıklayın **Dosya Seç**.
   
    c. Seçin **etkinleştirme kendi kendine kayıt**.

1. İçinde **kendi kendine kayıt ayarları** bölümünde aşağıdaki adımları uygulayın ve ardından **Kaydet**:
   
    ![Yönetim](./media/veracode-tutorial/ic802913.png "Yönetim")
   
    a. Olarak **yeni kullanıcı etkinleştirme**seçin **Hayır etkinleştirmesinin**.
   
    b. Olarak **kullanıcı veri güncelleştirmelerini**seçin **tercih Veracode kullanıcı verilerini**.
   
    c. İçin **SAML özniteliği ayrıntılarını**, aşağıdakileri seçin:
      * **Kullanıcı rolleri**
      * **İlke Yöneticisi**
      * **Gözden Geçiren**
      * **Güvenliği sağlama**
      * **Üst düzey**
      * **Gönderen**
      * **Oluşturan**
      * **Tüm taraması türleri**
      * **Takım üyeliklerinizi**
      * **Varsayılan takım**

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/veracode-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/veracode-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/veracode-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/veracode-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-veracode-test-user"></a>Veracode test kullanıcısı oluşturma
Veracode açarken Azure AD kullanıcılarının etkinleştirmek için bunların Veracode sağlanması gerekir. Veracode söz konusu olduğunda, sağlama, otomatik bir görev olur. Sizin için hiçbir eylem öğesini yoktur. Kullanıcıları otomatik olarak ilk çoklu oturum açma girişimi sırasında gerekirse oluşturulur.

> [!NOTE]
> Herhangi diğer Veracode kullanıcı hesabı oluşturma araçları kullanabilir veya API Azure AD'ye kullanıcı hesapları sağlamak için Veracode tarafından sağlanan.
> 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Veracode erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Veracode için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Veracode**.

    ![Uygulamalar listesinde Veracode bağlantı](./media/veracode-tutorial/tutorial_veracode_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Veracode kutucuğa tıkladığınızda, otomatik olarak Veracode uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/veracode-tutorial/tutorial_general_01.png
[2]: ./media/veracode-tutorial/tutorial_general_02.png
[3]: ./media/veracode-tutorial/tutorial_general_03.png
[4]: ./media/veracode-tutorial/tutorial_general_04.png

[100]: ./media/veracode-tutorial/tutorial_general_100.png

[200]: ./media/veracode-tutorial/tutorial_general_200.png
[201]: ./media/veracode-tutorial/tutorial_general_201.png
[202]: ./media/veracode-tutorial/tutorial_general_202.png
[203]: ./media/veracode-tutorial/tutorial_general_203.png

