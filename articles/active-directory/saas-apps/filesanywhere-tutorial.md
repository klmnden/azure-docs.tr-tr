---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile FilesAnywhere | Microsoft Docs'
description: Azure Active Directory ve FilesAnywhere arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/17/2017
ms.author: jeedes
ms.openlocfilehash: 2dab43499d5f55474117f88cfaa1cecd1a50bf3e
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54822051"
---
# <a name="tutorial-azure-active-directory-integration-with-filesanywhere"></a>Öğretici: FilesAnywhere ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile FilesAnywhere tümleştirme konusunda bilgi edinin.

Azure AD ile FilesAnywhere tümleştirme ile aşağıdaki avantajları sağlar:

- FilesAnywhere erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için FilesAnywhere (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Bir merkezi konumda - Azure Yönetim Portalı hesaplarınızı yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile FilesAnywhere yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir FilesAnywhere çoklu oturum açma etkin aboneliği


> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.


Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Bu gerekli olmadığı sürece üretim ortamınızı kullanmamanız gerekir.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden FilesAnywhere ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma


## <a name="adding-filesanywhere-from-the-gallery"></a>Galeriden FilesAnywhere ekleme
Azure AD'de FilesAnywhere tümleştirmesini yapılandırmak için FilesAnywhere Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden FilesAnywhere eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure Yönetim Portalı](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Tıklayın **Ekle** iletişim kutusunun üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **FilesAnywhere**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/filesanywhere-tutorial/tutorial_FilesAnywhere_search.png)

1. Sonuçlar panelinde seçin **FilesAnywhere**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/filesanywhere-tutorial/tutorial_FilesAnywhere_addfromgallery.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı FilesAnywhere sınayın.

Tek iş için oturum açma için Azure AD ne FilesAnywhere karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının FilesAnywhere ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Değerini atayarak bu bağlantı ilişki kurulduktan **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** FilesAnywhere içinde.

Yapılandırma ve Azure AD çoklu oturum açma FilesAnywhere ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[FilesAnywhere test kullanıcısı oluşturma](#creating-a-filesanywhere-test-user)**  - Azure AD gösterimini her için bağlı FilesAnywhere Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure yönetim portalında etkinleştirin ve FilesAnywhere uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile FilesAnywhere yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure Yönetim Portalı'nda üzerinde **FilesAnywhere** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda olarak **modu** seçin **SAML tabanlı oturum açma** için çoklu oturum açmayı etkinleştirme.
 
    ![Çoklu oturum açmayı yapılandırın](./media/filesanywhere-tutorial/tutorial_FilesAnywhere_samlbase.png)

1. Üzerinde **FilesAnywhere etki alanı ve URL'ler** uygulamada yapılandırmak isterseniz, bölümü **IDP tarafından başlatılan modu**:

    ![Çoklu oturum açmayı yapılandırın](./media/filesanywhere-tutorial/tutorial_filesanywhere_url.png)
    
    a. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<company name>.filesanywhere.com/saml20.aspx?c=215`
> [!NOTE]
> Lütfen unutmayın değeri **215** olduğu bir **ClientID** ve yalnızca bir örnektir. Gerçek ClientID değeriyle gerekir.

1. Üzerinde **FilesAnywhere etki alanı ve URL'ler** uygulamada yapılandırmak isterseniz, bölümü **SP tarafından başlatılan modu**, aşağıdaki adımları gerçekleştirin:
    
    ![Çoklu oturum açmayı yapılandırın](./media/filesanywhere-tutorial/tutorial_filesanywhere_url1.png)

    a. Tıklayarak **Gelişmiş URL ayarlarını göster** seçeneği

    b. İçinde **işareti bulunan URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<sub domain>.filesanywhere.com/`

    > [!NOTE] 
    > Bunlar gerçek değerleri olmadığına dikkat edin. Bu değerler gerçek oturum üzerinde URL'si ve yanıt URL'sini güncelleştirmeniz gerekiyor. İlgili kişi [FilesAnywhere Destek ekibine](mailto:support@FilesAnywhere.com) bu değerleri almak için. 

1. FilesAnywhere yazılım uygulama belirli bir biçimde SAML onaylamalarını bekler. Lütfen bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz "**kullanıcı öznitelikleri**" uygulama tümleştirme sayfasında bölümü. Aşağıdaki ekran görüntüsü bunun bir örneği gösterilmektedir.
    
    ![Çoklu oturum açmayı yapılandırın](./media/filesanywhere-tutorial/tutorial_filesanywhere_attribute.png)
    
    Kullanıcılar oturum açtığında FilesAnywhere ile değerini aldıkları **ClientID** özniteliğini [FilesAnywhere takım](mailto:support@FilesAnywhere.com). FilesAnywhere tarafından sağlanan benzersiz değere sahip "İstemci kimliği" öznitelik eklemeniz gerekir. Yukarıda gösterilen bu öznitelikleri gereklidir.
    > [!NOTE] 
    > Lütfen unutmayın değeri **2331** , **ClientID** yalnızca bir örnektir. Gerçek değer sağlanması gerekiyor.


1. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, SAML belirteci özniteliği yukarıdaki görüntüde gösterilen şekilde yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı | Öznitelik Değeri |
    | ---------------| --------------- |    
    | ClientID | *"uniquevalue"* |

    a. Tıklayın **eklemek agentconfigutil** açmak için **öznitelik Ekle** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/filesanywhere-tutorial/tutorial_FilesAnywhere_04.png)

    ![Çoklu oturum açmayı yapılandırın](./media/filesanywhere-tutorial/tutorial_FilesAnywhere_05.png)
    
    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.
    
    c. Gelen **değer** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.
    
    d. Tıklayın **Tamam**

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/filesanywhere-tutorial/tutorial_general_400.png)

1. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/filesanywhere-tutorial/tutorial_FilesAnywhere_certificate.png) 

1. Üzerinde **FilesAnywhere yapılandırma** bölümünde **yapılandırma FilesAnywhere** açmak için **yapılandırma oturum açma** penceresi.

    ![Çoklu oturum açmayı yapılandırın](./media/filesanywhere-tutorial/tutorial_FilesAnywhere_configure.png) 

    ![Çoklu oturum açmayı yapılandırın](./media/filesanywhere-tutorial/tutorial_FilesAnywhere_configuresignon.png)

1.  SSO yapılandırma FilesAnywhere sonunda uygulamanız için tam almak için iletişime geçin [FilesAnywhere Destek ekibine](mailto:support@FilesAnywhere.com) ve imzalama sertifikası ve çoklu oturum açma (SSO) URL'si indirilen SAML belirteci verin.

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, bir test kullanıcısı Britta Simon adlı Azure Yönetim Portalı'nda oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure Yönetim Portalı**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/filesanywhere-tutorial/create_aaduser_01.png) 

1. Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar** kullanıcılar listesini görüntüleyin.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/filesanywhere-tutorial/create_aaduser_02.png) 

1. İletişim kutusunun en üstünde tıklayın **Ekle** açmak için **kullanıcı** iletişim.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/filesanywhere-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/filesanywhere-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın. 



### <a name="creating-a-filesanywhere-test-user"></a>FilesAnywhere test kullanıcısı oluşturma

Uygulama, zaman kullanıcı sağlamayı ve kimlik doğrulaması kullanıcılar uygulamaya otomatik olarak oluşturulacak sonra sadece destekler. 


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için FilesAnywhere erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon FilesAnywhere için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure Yönetim Portalı'nda uygulamaları görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **FilesAnywhere**.

    ![Çoklu oturum açmayı yapılandırın](./media/filesanywhere-tutorial/tutorial_FilesAnywhere_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    


### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde FilesAnywhere kutucuğa tıkladığınızda, otomatik olarak FilesAnywhere uygulamanıza açan.


## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/FilesAnywhere-tutorial/tutorial_general_01.png
[2]: ./media/FilesAnywhere-tutorial/tutorial_general_02.png
[3]: ./media/FilesAnywhere-tutorial/tutorial_general_03.png
[4]: ./media/FilesAnywhere-tutorial/tutorial_general_04.png

[100]: ./media/FilesAnywhere-tutorial/tutorial_general_100.png

[200]: ./media/FilesAnywhere-tutorial/tutorial_general_200.png
[201]: ./media/FilesAnywhere-tutorial/tutorial_general_201.png
[202]: ./media/FilesAnywhere-tutorial/tutorial_general_202.png
[203]: ./media/FilesAnywhere-tutorial/tutorial_general_203.png
