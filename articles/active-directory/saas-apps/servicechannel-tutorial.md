---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile ServiceChannel | Microsoft Docs'
description: Azure Active Directory ve ServiceChannel arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: c3546eab-96b5-489b-a309-b895eb428053
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/3/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: b4be5087af70e10e5a73ea2a183a25b326aea664
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60341272"
---
# <a name="tutorial-azure-active-directory-integration-with-servicechannel"></a>Öğretici: ServiceChannel ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile ServiceChannel tümleştirme konusunda bilgi edinin.

Azure AD ile ServiceChannel tümleştirme ile aşağıdaki avantajları sağlar:

- ServiceChannel erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için ServiceChannel (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Bir merkezi konumda - Azure Yönetim Portalı hesaplarınızı yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile ServiceChannel yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir ServiceChannel çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Bu gerekli olmadığı sürece üretim ortamınızı kullanmamanız gerekir.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden ServiceChannel ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-servicechannel-from-the-gallery"></a>Galeriden ServiceChannel ekleme
Azure AD'de ServiceChannel tümleştirmesini yapılandırmak için ServiceChannel Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden ServiceChannel eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure Yönetim Portalı](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Tıklayın **Ekle** iletişim kutusunun üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **ServiceChannel**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/servicechannel-tutorial/tutorial-servicechannel_000.png)

1. Sonuçlar panelinde seçin **ServiceChannel**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/servicechannel-tutorial/tutorial-servicechannel_2.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı ServiceChannel sınayın.

Tek iş için oturum açma için Azure AD ne ServiceChannel karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının ServiceChannel ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Değerini atayarak bu bağlantı ilişki kurulduktan **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** ServiceChannel içinde.

Yapılandırma ve Azure AD çoklu oturum açma ServiceChannel ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[ServiceChannel test kullanıcısı oluşturma](#creating-a-servicechannel-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure yönetim portalında etkinleştirin ve ServiceChannel uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile ServiceChannel yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure Yönetim Portalı'nda üzerinde **ServiceChannel** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda olarak **modu** seçin **SAML tabanlı oturum açma** için çoklu oturum açmayı etkinleştirme.
 
    ![Çoklu oturum açmayı yapılandırın](./media/servicechannel-tutorial/tutorial-servicechannel_01.png)

1. Üzerinde **ServiceChannel etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/servicechannel-tutorial/tutorial-servicechannel_urls.png)

    a. İçinde **tanımlayıcı** metin değeri olarak yazın: `http://adfs.<domain>.com/adfs/service/trust`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<customer domain>.servicechannel.com/saml/acs`

    > [!NOTE] 
    > Bunlar gerçek değerleri olmadığına dikkat edin. Bu değerler gerçek tanımlayıcısı ve yanıt URL'sini güncelleştirmeniz gerekiyor. Burada benzersiz dize değeri tanımlayıcıda kullanmanızı öneririz. İlgili kişi [ServiceChannel Destek ekibine](https://servicechannel.zendesk.com/hc/en-us) bu değerleri almak için.

1. ServiceChannel uygulamanız SAML onaylamalarını özel öznitelik eşlemelerini SAML belirteci öznitelikleri yapılandırmanıza ekleyin gerektiren belirli bir biçimde bekliyor. Aşağıdaki ekran görüntüsü bunun bir örneği gösterilmektedir. **(Kullanıcı tanımlayıcısı) NameIdentifier** yalnızca zorunlu talep ve varsayılan değer **user.userprincipalname** ServiceChannel bu ile eşlenmesini bekliyor, ancak **user.mail**. Kullanıcı tam zamanında sağlama etkinleştirmeyi planlıyorsanız, aşağıda gösterildiği gibi aşağıdaki talep eklemelisiniz. **Rol** talep eşlenmesi gerekiyor **user.assignedroles** kullanıcı rolünü içerir.  

    ServiceChannel Kılavuzu başvurabilir [burada](https://servicechannel.zendesk.com/hc/en-us/articles/217514326-Azure-AD-Configuration-Example) talepler hakkında daha fazla rehberlik için.
    
    ![Çoklu oturum açmayı yapılandırın](./media/servicechannel-tutorial/tutorial_servicechannel_attribute.png)

    > [!NOTE] 
    > Bkz: [RBAC ve Azure portalını kullanarak erişimini yönetme](../../role-based-access-control/role-assignments-portal.md) nasıl yapılandırılacağını öğrenmek için **rol** Azure AD'de.

1. İçinde **kullanıcı öznitelikleri** bölümünde **görünümü ve diğer tüm kullanıcı özniteliklerini düzenleyin** ve özniteliklerini ayarlayın.

    | Öznitelik Adı | Öznitelik Değeri |
    | --- | --- |    
    | Rol| User.assignedroles |

    a. Tıklayın **eklemek agentconfigutil** açmak için **öznitelik Ekle** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/servicechannel-tutorial/tutorial_servicechannel_04.png)

    ![Çoklu oturum açmayı yapılandırın](./media/servicechannel-tutorial/tutorial_servicechannel_05.png)
    
    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.
    
    c. Gelen **değer** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.
    
    d. Tıklayın **Tamam**
    
1. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/servicechannel-tutorial/tutorial-servicechannel_05.png) 

1. **Kaydet**’e tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/servicechannel-tutorial/tutorial_general_400.png)

1. Üzerinde **ServiceChannel yapılandırma** bölümünde **yapılandırma ServiceChannel** açmak için **yapılandırma oturum açma** penceresi. Lütfen unutmayın **SAML varlık kimliği** gelen **hızlı başvuru** bölümü.

1. Çoklu oturum açmayı yapılandırma **ServiceChannel** tarafı, indirilen göndermek için ihtiyacınız **sertifika (Base64)** ve **SAML varlık kimliği** için [ServiceChannel Destek](https://servicechannel.zendesk.com/hc/en-us). Bunlar bu için her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı kurar.

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, bir test kullanıcısı Britta Simon adlı Azure Yönetim Portalı'nda oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure Yönetim Portalı**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/servicechannel-tutorial/create_aaduser_01.png) 

1. Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar** kullanıcılar listesini görüntüleyin.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/servicechannel-tutorial/create_aaduser_02.png) 

1. İletişim kutusunun en üstünde tıklayın **Ekle** açmak için **kullanıcı** iletişim.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/servicechannel-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/servicechannel-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın. 

### <a name="creating-a-servicechannel-test-user"></a>ServiceChannel test kullanıcısı oluşturma

Uygulama, zaman kullanıcı sağlamayı ve kimlik doğrulaması kullanıcılar uygulamaya otomatik olarak oluşturulacak sonra sadece destekler. İçin tam kullanıcı hazırlama, temasa [ServiceChannel destek ekibi](https://servicechannel.zendesk.com/hc/en-us)

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için ServiceChannel erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon ServiceChannel için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure Yönetim Portalı'nda uygulamaları görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **ServiceChannel**.

    ![Çoklu oturum açmayı yapılandırın](./media/servicechannel-tutorial/tutorial-servicechannel_app01.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde ServiceChannel kutucuğa tıkladığınızda, otomatik olarak ServiceChannel uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)


<!--Image references-->

[1]: ./media/servicechannel-tutorial/tutorial_general_01.png
[2]: ./media/servicechannel-tutorial/tutorial_general_02.png
[3]: ./media/servicechannel-tutorial/tutorial_general_03.png
[4]: ./media/servicechannel-tutorial/tutorial_general_04.png

[100]: ./media/servicechannel-tutorial/tutorial_general_100.png

[200]: ./media/servicechannel-tutorial/tutorial_general_200.png
[201]: ./media/servicechannel-tutorial/tutorial_general_201.png
[202]: ./media/servicechannel-tutorial/tutorial_general_202.png
[203]: ./media/servicechannel-tutorial/tutorial_general_203.png
