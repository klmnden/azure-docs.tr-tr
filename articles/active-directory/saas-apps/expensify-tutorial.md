---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Expensify | Microsoft Docs'
description: Azure Active Directory ve Expensify arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 1e761484-7a2f-4321-91f4-6d5d0b69344e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/02/2018
ms.author: jeedes
ms.openlocfilehash: 1f69ad3045693e1ca826b6e04443f97ac30bc377
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55156947"
---
# <a name="tutorial-azure-active-directory-integration-with-expensify"></a>Öğretici: Expensify ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Expensify tümleştirme konusunda bilgi edinin.

Azure AD ile Expensify tümleştirme ile aşağıdaki avantajları sağlar:

- Expensify erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için Expensify (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Expensify yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik bir Expensify çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Expensify ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-expensify-from-the-gallery"></a>Galeriden Expensify ekleme

Azure AD'de Expensify tümleştirmesini yapılandırmak için Expensify Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Expensify eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![image](./media/expensify-tutorial/selectazuread.png)

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![image](./media/expensify-tutorial/a_select_app.png)
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![image](./media/expensify-tutorial/a_new_app.png)

4. Arama kutusuna **Expensify**seçin **Expensify** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![image](./media/expensify-tutorial/a_add_app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Expensify sınayın.

Tek iş için oturum açma için Azure AD ne Expensify karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Expensify ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Expensify içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Expensify ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Bir Expensify test kullanıcısı oluşturma](#create-an-expensify-test-user)**  - kullanıcı Azure AD gösterimini bağlı Expensify Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Expensify uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Expensify yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde [Azure portalında](https://portal.azure.com/), **Expensify** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![image](./media/expensify-tutorial/b1_b2_select_sso.png)

2. Tıklayın **değişiklik çoklu oturum açma modunu** seçmek için ekranın en üstünde **SAML** modu.

      ![image](./media/expensify-tutorial/b1_b2_saml_ssso.png)

3. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunu tıklatın **seçin** için **SAML** modu, çoklu oturum açmayı etkinleştirmek için.

    ![image](./media/expensify-tutorial/b1_b2_saml_sso.png)

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için düğmeyi **temel SAML yapılandırma** iletişim.

    ![image](./media/expensify-tutorial/b1-domains_and_urlsedit.png)

5. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL yazın: `https://www.expensify.com/authentication/saml/login`

    b. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://www.<companyname>.expensify.com`

    ![image](./media/expensify-tutorial/b1-domains_and_urls.png)

    > [!NOTE] 
    > Değiştirin <companyname> tanımlayıcı URL'si ile şirketinizin etki alanı bölümü. Örnek olarak bkz `https://contoso.expensify.com` yukarıda. Expensify etki alanınızın adını gelen gösterildiği gibi budur **ayarlar > etki alanı denetim**.

    ![Etki alanı bilgilerini expensify](./media/expensify-tutorial/tutorial_expensify_domain.png)

6. Üzerinde **yukarı çoklu oturum açma SAML ile Ayarla** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** olarak başına uygun sertifikayı indirmek için gereksinim ve bilgisayarınıza kaydedin.

    ![image](./media/expensify-tutorial/certificatebase64.png)

7. İçinde Expensify SSO'yu etkinleştirmek için önce etkinleştirmeniz gerekir **etki alanı denetim** uygulama. Listelenen adımları uygulama etki alanı denetim etkinleştirebilirsiniz [burada](https://help.expensify.com/domain-control). Çalışmak için ek destek, [Expensify istemci Destek ekibine](mailto:help@expensify.com). Etki alanı denetim etkin olduktan sonra aşağıdaki adımları izleyin:
   
    ![Çoklu oturum açmayı yapılandırın](./media/expensify-tutorial/tutorial_expensify_51.png)
    
    a. Expensify uygulamanız için oturum açın.
    
    b. Sol bölmede bulunan tıklayın **ayarları** gidin **SAML**.
    
    c. İki durumlu **SAML oturum açma** olarak seçeneğini **etkin**.
    
    d. İndirilen Federasyon meta verileri Azure AD'den Not Defteri'nde açın, içeriği kopyalayın ve ardından yapıştırın **kimlik sağlayıcısı meta verileri** metin.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    ![image](./media/expensify-tutorial/d_users_and_groups.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![image](./media/expensify-tutorial/d_adduser.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![image](./media/expensify-tutorial/d_userproperties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü **brittasimon@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **özellikleri**seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’u seçin.
 
### <a name="create-an-expensify-test-user"></a>Bir Expensify test kullanıcısı oluşturma

Bu bölümde, Britta Simon Expensify içinde adlı bir kullanıcı oluşturun. Çalışmak [Expensify istemci Destek ekibine](mailto:help@expensify.com) Expensify platform kullanıcıları eklemek için.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Expensify erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**.

    ![image](./media/expensify-tutorial/d_all_applications.png)

2. Uygulamalar listesinde **Expensify**.

    ![image](./media/expensify-tutorial/d_all_proapplications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![image](./media/expensify-tutorial/d_leftpaneusers.png)

4. Seçin **Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![image](./media/expensify-tutorial/d_assign_user.png)

4. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

5. İçinde **atama Ekle** iletişim kutusunda **atama** düğmesi.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Expensify kutucuğa tıkladığınızda, otomatik olarak Expensify uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)




