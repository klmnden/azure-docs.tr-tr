---
title: 'Öğretici: Azure Active Directory Tümleştirme kutusu | Microsoft Docs'
description: Azure Active Directory ve kutusu arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 3b565c8d-35e2-482a-b2f4-bf8fd7d8731f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/09/2018
ms.author: jeedes
ms.openlocfilehash: 535f59b1b0dc56b183c8a019d101b4fd4f1bfad6
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2018
ms.locfileid: "49116134"
---
# <a name="tutorial-azure-active-directory-integration-with-box"></a>Öğretici: Azure Active Directory Tümleştirme kutusu

Bu öğreticide, kutusu Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.

Kutusu Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Box'a erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan kutusuna (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi kutusuyla yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik kutusu çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden kutusu ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-box-from-the-gallery"></a>Galeriden kutusu ekleme
Azure AD'de kutusunun tümleştirmesini yapılandırmak için kutusu Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden kutusunu eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![image](./media/box-tutorial/selectazuread.png)

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![image](./media/box-tutorial/a_select_app.png)
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![image](./media/box-tutorial/a_new_app.png)

4. Arama kutusuna **kutusu**seçin **kutusu** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![image](./media/box-tutorial/tutorial_Box_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma kutusunu "Britta Simon" adlı bir test kullanıcısı üzerinde sınayın.

Tek iş için oturum açma için Azure AD kutusuna karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmesi gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının kutusunda ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma kutusu test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Kutusunu test kullanıcısı oluşturma](#create-a-box-test-user)**  - kullanıcı Azure AD gösterimini bağlı kutusunda Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve kutusu uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma kutusuyla yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde [Azure portalında](https://portal.azure.com/), **kutusu** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![image](./media/box-tutorial/b1_b2_select_sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunu tıklatın **seçin** için **SAML** modu, çoklu oturum açmayı etkinleştirmek için.

    ![image](./media/box-tutorial/b1_b2_saml_sso.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için düğmeyi **temel SAML yapılandırma** iletişim.

    ![image](./media/box-tutorial/b1-domains_and_urlsedit.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<SUBDOMAIN>.account.box.com`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL:`box.net`

    ![image](./media/box-tutorial/tutorial_box_url.png)

    > [!NOTE] 
    > Oturum açma URL değeri, gerçek değil. Değerini gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [kutusu Destek ekibine](https://community.box.com/t5/custom/page/page-id/submit_sso_questionaire) değeri alınamıyor.
 
5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  ve bilgisayarınıza kaydedin.

    ![image](./media/box-tutorial/tutorial_Box_certificate.png)

6. Uygulamanız için SSO'yu yapılandırmak için verilen yordamı izleyin [SSO'yu kendi ayarlamak](https://community.box.com/t5/How-to-Guides-for-Admins/Setting-Up-Single-Sign-On-SSO-for-your-Enterprise/ta-p/1263#ssoonyourown). 

>[!NOTE]
>Box hesabınıza için SSO ayarlarını yapılandırmaya bulamıyorsanız, indirilen göndermem gerekiyor **Federasyon meta verileri XML** için [kutusu Destek ekibine](https://community.box.com/t5/custom/page/page-id/submit_sso_questionaire). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    ![image](./media/box-tutorial/d_users_and_groups.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![image](./media/box-tutorial/d_adduser.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![image](./media/box-tutorial/d_userproperties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü **brittasimon@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **özellikleri**seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’u seçin.
 
### <a name="create-a-box-test-user"></a>Kutusunu test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon kutusunda adlı bir kullanıcı oluşturmaktır. Kutusunu tam zamanında sağlama, varsayılan olarak etkin olan destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Yeni bir kullanıcı, henüz yoksa kutusu erişme denemesi sırasında oluşturulur.
>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [kutusu Destek ekibine](https://community.box.com/t5/custom/page/page-id/submit_sso_questionaire).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Box'a erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**.

    ![image](./media/box-tutorial/d_all_applications.png)

2. Uygulamalar listesinde **kutusu**.

    ![image](./media/box-tutorial/tutorial_Box_app.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![image](./media/box-tutorial/d_leftpaneusers.png)

4. Seçin **Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![image](./media/box-tutorial/d_assign_user.png)

4. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

5. İçinde **atama Ekle** iletişim kutusunda **atama** düğmesi.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Seçtiğinizde, **kutusu** kutucuğuna erişim panelinde kutusunu uygulamanıza oturum açma için oturum açma sayfası alın.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
* [Kullanıcı sağlamayı yapılandırma](box-userprovisioning-tutorial.md)


