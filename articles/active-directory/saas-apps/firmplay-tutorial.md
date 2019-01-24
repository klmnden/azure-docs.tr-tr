---
title: 'Öğretici: FirmPlay - çalışan danışmanlık işe alma için Azure Active Directory tümleştirmesiyle | Microsoft Docs'
description: Azure Active Directory ve FirmPlay - işe alma için çalışan danışmanlık arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: a6799629-7546-43f8-a966-956db32864b1
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/15/2017
ms.author: jeedes
ms.openlocfilehash: 929494d5d802dbc545c750386a286029c4bf962d
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54809811"
---
# <a name="tutorial-azure-active-directory-integration-with-firmplay---employee-advocacy-for-recruiting"></a>Öğretici: Azure Active Directory tümleştirmesiyle FirmPlay - çalışan danışmanlık için işe alma

Bu öğreticide, Azure Active Directory (Azure AD) ile işe alma için çalışan danışmanlık FirmPlay - tümleştirme konusunda bilgi edinin.

FirmPlay - Azure AD ile işe alma için çalışan danışmanlık tümleştirme ile aşağıdaki avantajları sağlar:

- FirmPlay - çalışan danışmanlık işe alma için erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için FirmPlay - çalışan danışmanlık işe alma (çoklu oturum açma) ile kendi Azure AD hesapları için açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Bir merkezi konumda - Azure Yönetim Portalı hesaplarınızı yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi FirmPlay - için işe alma, çalışan danışmanlık ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- FirmPlay - çalışan danışmanlık için çoklu oturum açma etkin abonelik işe alma


> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.


Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Bu gerekli olmadığı sürece üretim ortamınızı kullanmamanız gerekir.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. İşe alma Galerisi için çalışan danışmanlık FirmPlay - ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma


## <a name="adding-firmplay---employee-advocacy-for-recruiting-from-the-gallery"></a>İşe alma Galerisi için çalışan danışmanlık FirmPlay - ekleme
FirmPlay - Azure AD'ye işe alma için çalışan danışmanlık tümleştirmesini yapılandırmak için işe alma galerisinden listenize yönetilen SaaS uygulamaları için çalışan danışmanlık FirmPlay - eklemeniz gerekir.

**İşe alma Galerisi için çalışan danışmanlık FirmPlay - eklemek için aşağıdaki adımları uygulayın:**

1. İçinde  **[Azure Yönetim Portalı](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Tıklayın **Ekle** iletişim kutusunun üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **FirmPlay - işe alma için çalışan danışmanlık**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/firmplay-tutorial/tutorial_firmplay_001.png)

1. Sonuçlar panelinde seçin **FirmPlay - işe alma için çalışan danışmanlık**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/firmplay-tutorial/tutorial_firmplay_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma FirmPlay - için işe alma "Britta Simon" adlı bir test kullanıcıya bağlı çalışanların danışmanlık ile test edin.

Tek çalışmak, oturum için Azure AD içinde FirmPlay karşılığı kullanıcının bilmesi gerekir - çalışan danışmanlık işe alma için bir kullanıcı için Azure AD'de. Diğer bir deyişle, bir Azure AD kullanıcısının FirmPlay - çalışan danışmanlık işe alma için ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Değerini atayarak bu bağlantı ilişki kurulduktan **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** FirmPlay - işe alma için çalışan danışmanlık içinde.

Yapılandırma ve Azure AD çoklu oturum açma FirmPlay ile-test etmek için çalışan danışmanlık işe alma için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Çalışan danışmanlık işe alma test kullanıcısı için bir FirmPlay - oluşturma](#creating-a-firmplay---employee-advocacy-for-recruiting-test-user)**  - FirmPlay içinde bir karşılığı Britta simon'un sağlamak için: Çalışan danışmanlık, yani işe alma için Azure AD gösterimini her için bağlı.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure yönetim portalında etkinleştirin ve FirmPlay - çalışan danışmanlık işe alma uygulaması için çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma FirmPlay - çalışan danışmanlık işe alma için yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure Yönetim Portalı'nda üzerinde **FirmPlay - işe alma için çalışan danışmanlık** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda olarak **modu** seçin **SAML tabanlı oturum açma** için çoklu oturum açmayı etkinleştirme.
 
    ![Çoklu oturum açmayı yapılandırın](./media/firmplay-tutorial/tutorial_firmplay_01.png)

1. Üzerinde **FirmPlay - işe etki alanı ve URL'ler için çalışan danışmanlık** bölümünde **işareti bulunan URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<your-subdomain>.firmplay.com/`

    ![Çoklu oturum açmayı yapılandırın](./media/firmplay-tutorial/tutorial_firmplay_02.png)

    > [!NOTE] 
    > Bu gerçek değer olmadığını unutmayın. Bu değer gerçek işareti bulunan URL'si ile güncelleştirmeniz gerekiyor. İlgili kişi [FirmPlay - işe alma destek ekibi için çalışan danışmanlık](mailto:engineering@firmplay.com) bu değeri alınamıyor. 

1. Üzerinde **SAML imzalama sertifikası** bölümünde **yeni sertifika oluştur**.

    ![Çoklu oturum açmayı yapılandırın](./media/firmplay-tutorial/tutorial_firmplay_03.png)     

1. Üzerinde **yeni sertifika oluştur** iletişim kutusunda, Takvim simgesine tıklayıp bir **sona erme tarihi**. Ardından **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/firmplay-tutorial/tutorial_general_300.png)

1. Üzerinde **SAML imzalama sertifikası** bölümünden **yeni sertifikayı etkin hale getirin** tıklatıp **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/firmplay-tutorial/tutorial_firmplay_04.png)

1. Açılır pencere üzerinde **geçiş sertifikası** penceresinde tıklayın **Tamam**.

    ![Çoklu oturum açmayı yapılandırın](./media/firmplay-tutorial/tutorial_general_400.png)

1. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin. 

    ![Çoklu oturum açmayı yapılandırın](./media/firmplay-tutorial/tutorial_firmplay_05.png) 

1. Üzerinde **FirmPlay - işe yapılandırması için çalışan danışmanlık** bölümünde **FirmPlay - çalışan danışmanlık işe alma için yapılandırma** açmak için **yapılandırma oturum açma** iletişim kutusu.

    ![Çoklu oturum açmayı yapılandırın](./media/firmplay-tutorial/tutorial_firmplay_06.png) 

    ![Çoklu oturum açmayı yapılandırın](./media/firmplay-tutorial/tutorial_firmplay_07.png)

1. Uygulamanız için yapılandırılmış SSO almak için iletişime geçin [FirmPlay - işe alma destek ekibi için çalışan danışmanlık](mailto:engineering@firmplay.com) ve bunları ile şu bilgileri sağlayın: 

    • İndirilen **sertifika dosyası**

    • **SAML çoklu oturum açma hizmeti URL'si**

    • **SAML varlık kimliği**

    • **Oturum kapatma URL'si**
  

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, bir test kullanıcısı Britta Simon adlı Azure Yönetim Portalı'nda oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure Yönetim Portalı**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/firmplay-tutorial/create_aaduser_01.png) 

1. Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar** kullanıcılar listesini görüntüleyin.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/firmplay-tutorial/create_aaduser_02.png) 

1. İletişim kutusunun en üstünde tıklayın **Ekle** açmak için **kullanıcı** iletişim.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/firmplay-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/firmplay-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın. 



### <a name="creating-a-firmplay---employee-advocacy-for-recruiting-test-user"></a>Bir FirmPlay - çalışan danışmanlık için işe alma test kullanıcısı oluşturma

Bu bölümde, Britta Simon FirmPlay - işe alma için çalışan danışmanlık adlı bir kullanıcı oluşturun. Lütfen birlikte çalışarak [FirmPlay - işe alma destek ekibi için çalışan danışmanlık](mailto:engineering@firmplay.com) FirmPlay - çalışan danışmanlık işe alma platform için kullanıcıları eklemek için.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma FirmPlay - çalışan danışmanlık işe alma için erişim vermeye tarafından kullanmak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon FirmPlay - çalışan danışmanlık için işe alma, atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure Yönetim Portalı'nda uygulamaları görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **FirmPlay - işe alma için çalışan danışmanlık**.

    ![Çoklu oturum açmayı yapılandırın](./media/firmplay-tutorial/tutorial_firmplay_50.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    


### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

FirmPlay - çalışan danışmanlık erişim Paneli'nde işe alma kutucuk için tıkladığınızda, otomatik olarak imzalanmış için FirmPlay - çalışan danışmanlık işe alma uygulaması için açma.


## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/firmplay-tutorial/tutorial_general_01.png
[2]: ./media/firmplay-tutorial/tutorial_general_02.png
[3]: ./media/firmplay-tutorial/tutorial_general_03.png
[4]: ./media/firmplay-tutorial/tutorial_general_04.png

[100]: ./media/firmplay-tutorial/tutorial_general_100.png

[200]: ./media/firmplay-tutorial/tutorial_general_200.png
[201]: ./media/firmplay-tutorial/tutorial_general_201.png
[202]: ./media/firmplay-tutorial/tutorial_general_202.png
[203]: ./media/firmplay-tutorial/tutorial_general_203.png