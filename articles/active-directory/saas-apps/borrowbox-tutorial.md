---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile BorrowBox | Microsoft Docs'
description: Azure Active Directory ve BorrowBox arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: dd8e4178-9a63-492a-bd48-782e94e404af
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/26/2018
ms.author: jeedes
ms.openlocfilehash: efb905364e5ae4736b058472e66b3a8850a99fe3
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55167853"
---
# <a name="tutorial-azure-active-directory-integration-with-borrowbox"></a>Öğretici: BorrowBox ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile BorrowBox tümleştirme konusunda bilgi edinin.

Azure AD ile BorrowBox tümleştirme ile aşağıdaki avantajları sağlar:

- BorrowBox erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için BorrowBox (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile BorrowBox yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik BorrowBox çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden BorrowBox ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-borrowbox-from-the-gallery"></a>Galeriden BorrowBox ekleme
Azure AD'de BorrowBox tümleştirmesini yapılandırmak için BorrowBox Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden BorrowBox eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![image](./common/selectazuread.png)

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![image](./common/a_select_app.png)
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![image](./common/a_new_app.png)

4. Arama kutusuna **BorrowBox**seçin **BorrowBox** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![image](./media/borrowbox-tutorial/tutorial_borrowbox_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı BorrowBox sınayın.

Tek iş için oturum açma için Azure AD ne BorrowBox karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının BorrowBox ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma BorrowBox ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[BorrowBox test kullanıcısı oluşturma](#create-a-borrowbox-test-user)**  - kullanıcı Azure AD gösterimini bağlı BorrowBox Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve BorrowBox uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile BorrowBox yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde [Azure portalında](https://portal.azure.com/), **BorrowBox** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![image](./common/B1_B2_Select_SSO.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML** modu, çoklu oturum açmayı etkinleştirmek için.

    ![image](./common/b1_b2_saml_sso.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için düğmeyi **temel SAML yapılandırma** iletişim.

    ![image](./common/b1-domains_and_urlsedit.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde kullanıcısının clonedatabase'i uygulama zaten Azure ile önceden tümleşik olarak herhangi bir adımı gerçekleştirmek.

    ![image](./media/borrowbox-tutorial/tutorial_borrowbox_url.png)

    a. Tıklayın **ek URL'lerini ayarlayın**.

    b. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://fe.bolindadigital.com/wldcs_bol_fo/b2i/mainPage.html?b2bSite=<ID>`

    ![image](./media/borrowbox-tutorial/tutorial_borrowbox_url1.png)

    > [!NOTE]
    > Oturum açma URL değeri, gerçek değil. Değerini gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [BorrowBox istemci Destek ekibine](mailto:borrowbox@bolinda.com) değeri alınamıyor.

5. BorrowBox uygulama belirli bir biçimde SAML onaylamalarını bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz **kullanıcı öznitelikleri ve talepler** uygulama tümleştirme sayfasında bölümü. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için düğmeyi **kullanıcı öznitelikleri ve talepler** iletişim.

    ![image](./media/borrowbox-tutorial/i4-attribute.png)

6. İçinde **kullanıcı taleplerini** bölümünde **kullanıcı öznitelikleri ve talepler** iletişim kutusunda, SAML belirteci özniteliği yukarıdaki görüntüde gösterilen şekilde yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    a. Tıklayarak **düzenleme simgesi** açmak için **yönetmek, kullanıcı talepleri** iletişim.

    ![image](./media/borrowbox-tutorial/i2-attribute.png)

    ![image](./media/borrowbox-tutorial/i3-attribute.png)

    b. Gelen **kaynak özniteliği** listesinden **user.mail**.

    c. **Kaydet**’e tıklayın. 

7. Üzerinde **yukarı çoklu oturum açma SAML ile Ayarla** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** olarak başına uygun sertifikayı indirmek için gereksinim ve bilgisayarınıza kaydedin.

    ![image](./media/borrowbox-tutorial/tutorial_borrowbox_certificate.png) 

8. Çoklu oturum açmayı yapılandırma **BorrowBox** tarafı, gereken göndermek için Azure portalından indirilen sertifika/metaveri [BorrowBox Destek ekibine](mailto:borrowbox@bolinda.com). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    ![image](./common/d_users_and_groups.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![image](./common/d_adduser.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![image](./common/d_userproperties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü **brittasimon@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **özellikleri**seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’u seçin.
 
### <a name="create-a-borrowbox-test-user"></a>BorrowBox test kullanıcısı oluşturma

Bu bölümün amacı BorrowBox Britta Simon adlı bir kullanıcı oluşturmaktır. BorrowBox tam zamanında sağlama, varsayılan olarak etkin olan destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Yeni bir kullanıcı, henüz yoksa BorrowBox erişme denemesi sırasında oluşturulur.
>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [BorrowBox Destek ekibine](mailto:borrowbox@bolinda.com).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için BorrowBox erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**.

    ![image](./common/d_all_applications.png)

2. Uygulamalar listesinde **BorrowBox**.

    ![image](./media/borrowbox-tutorial/tutorial_borrowbox_app.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![image](./common/d_leftpaneusers.png)

4. Seçin **Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![image](./common/d_assign_user.png)

4. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

5. İçinde **atama Ekle** iletişim kutusunda **atama** düğmesi.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde BorrowBox kutucuğa tıkladığınızda, otomatik olarak BorrowBox uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
