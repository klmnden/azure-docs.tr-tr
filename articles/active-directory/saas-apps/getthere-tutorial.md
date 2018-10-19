---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle GetThere | Microsoft Docs'
description: Azure Active Directory ve GetThere arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0441087e-953f-4b51-9842-316da7b72392
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2018
ms.author: jeedes
ms.openlocfilehash: bcefa3966a6c854f02ce7b3a75306b3d1c888ecd
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49431762"
---
# <a name="tutorial-azure-active-directory-integration-with-getthere"></a>Öğretici: Azure Active Directory GetThere ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile GetThere tümleştirme konusunda bilgi edinin.

Azure AD ile GetThere tümleştirme ile aşağıdaki avantajları sağlar:

- GetThere erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için GetThere (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile GetThere yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik GetThere çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden GetThere ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-getthere-from-the-gallery"></a>Galeriden GetThere ekleme
Azure AD'de GetThere tümleştirmesini yapılandırmak için GetThere Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden GetThere eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![image](./media/getthere-tutorial/selectazuread.png)

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![image](./media/getthere-tutorial/a_select_app.png)
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![image](./media/getthere-tutorial/a_new_app.png)

4. Arama kutusuna **GetThere**seçin **GetThere** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![image](./media/getthere-tutorial/tutorial_getthere_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı GetThere sınayın.

Tek iş için oturum açma için Azure AD ne GetThere karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının GetThere ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma GetThere ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[GetThere test kullanıcısı oluşturma](#create-a-getthere-test-user)**  - kullanıcı Azure AD gösterimini bağlı GetThere Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve GetThere uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile GetThere yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde [Azure portalında](https://portal.azure.com/), **GetThere** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![image](./media/getthere-tutorial/B1_B2_Select_SSO.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML** modu, çoklu oturum açmayı etkinleştirmek için.

    ![image](./media/getthere-tutorial/b1_b2_saml_sso.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için düğmeyi **temel SAML yapılandırma** iletişim.

    ![image](./media/getthere-tutorial/b1-domains_and_urlsedit.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![image](./media/getthere-tutorial/tutorial_getthere_url.png)

    a. İçinde **tanımlayıcı** metin herhangi birini yazın URL'leri aşağıda:
    | |
    |--|
    | `getthere.com` |
    | `http://idp.getthere.com` |

    b. İçinde **yanıt URL'si** metin kutusuna, birini yazın URL'leri aşağıda:
    | |
    |--|
    | `https://wx1.getthere.net/login/saml/post.act` |
    | `https://gtx2-gcte2.getthere.net/login/saml/post.act` |
    | `https://gtx2-gcte2.getthere.net/login/saml/ssoaasvalidate.act` |
    | `https://wx1.getthere.net/login/saml/ssoaavalidate.act` |
    
5. GetThere uygulama bekliyor benzersiz **kullanıcıadı** kullanıcıadı talep değeri. Müşteri, kullanıcı talebi için doğru değeri eşleyebilirsiniz. Aşağıdaki örnekte, biz eşlenen **kullanıcıadı** için **user.mail**, ancak eşleme kuruluş ayarlarınıza göre değiştirebilirsiniz. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için düğmeyi **kullanıcı öznitelikleri** iletişim.

    ![image](./media/getthere-tutorial/i4-attribute.png)

6. İçinde **kullanıcı taleplerini** bölümünde **kullanıcı öznitelikleri** iletişim kutusunda, SAML belirteci özniteliği yukarıdaki görüntüde gösterilen şekilde yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Ad |  Kaynak özniteliği |  Ad Alanı |
    | ---------------| --------------- | --------------- |
    | Site adı | "Kuruluşunuz göre değeri sağla" | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/sitename |
    | Kullanıcı adı |  User.Mail | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/username |
    
    a. Tıklayın **Ekle yeni talep** açmak için **yönetmek, kullanıcı talepleri** iletişim.

    ![image](./media/getthere-tutorial/i2-attribute.png)

    ![image](./media/getthere-tutorial/i3-attribute.png)

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. İçinde **Namespace** metin kutusuna, bu satır için gösterilen öznitelik ad alanı yazın.

    d. Kaynağı olarak **özniteliği**.

    e. Gelen **kaynak özniteliği** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    f. **Kaydet**’e tıklayın.

7. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** ve bilgisayarınıza kaydedin.

    ![image](./media/getthere-tutorial/tutorial_getthere_certficate.png) 

8. Üzerinde **GetThere kümesi** bölümünde, ihtiyacınıza göre uygun URL'yi kopyalayın.

    Not: URL aşağıdaki bildirebilir

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

    ![image](./media/getthere-tutorial/d1_samlsonfigure.png) 

9. Çoklu oturum açmayı yapılandırma **GetThere** tarafı, indirilen göndermek için ihtiyacınız **sertifika (Base64)** ve kopyalanan **oturum kapatma URL'si oturum açma URL'si Azure Ad tanımlayıcısı** için[ GetThere istemci Destek ekibine](mailto:dataintegration@sabre.com). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    ![image](./media/getthere-tutorial/d_users_and_groups.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![image](./media/getthere-tutorial/d_adduser.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![image](./media/getthere-tutorial/d_userproperties.png)

    a. İçinde **adı** alanına **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü **brittasimon@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **özellikleri**seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’u seçin.
 
### <a name="create-a-getthere-test-user"></a>GetThere test kullanıcısı oluşturma

Bu bölümde, Britta Simon GetThere içinde adlı bir kullanıcı oluşturun. Çalışmak [GetThere istemci Destek ekibine](mailto:dataintegration@sabre.com) GetThere platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için GetThere erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**.

    ![image](./media/getthere-tutorial/d_all_applications.png)

2. Uygulamalar listesinde **GetThere**.

    ![image](./media/getthere-tutorial/tutorial_getthere_app.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![image](./media/getthere-tutorial/d_leftpaneusers.png)

4. Seçin **Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![image](./media/getthere-tutorial/d_assign_user.png)

4. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

5. İçinde **atama Ekle** iletişim kutusunda **atama** düğmesi.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde GetThere kutucuğa tıkladığınızda, otomatik olarak GetThere uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
