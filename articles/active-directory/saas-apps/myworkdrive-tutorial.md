---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle MyWorkDrive | Microsoft Docs'
description: Azure Active Directory ve MyWorkDrive arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4d049778-3c7b-46c0-92a4-f2633a32334b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/25/2018
ms.author: jeedes
ms.openlocfilehash: 7310d300c68399c31d9580f070602aa3adbc75e3
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50094065"
---
# <a name="tutorial-azure-active-directory-integration-with-myworkdrive"></a>Öğretici: Azure Active Directory tümleştirmesiyle MyWorkDrive

Bu öğreticide, Azure Active Directory (Azure AD) ile MyWorkDrive tümleştirme konusunda bilgi edinin.

MyWorkDrive, Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- MyWorkDrive erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan MyWorkDrive'nın (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile MyWorkDrive yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- MyWorkDrive çoklu oturum açma abonelik etkin.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. MyWorkDrive galeri ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-myworkdrive-from-the-gallery"></a>MyWorkDrive galeri ekleme
Azure AD'de MyWorkDrive'nın tümleştirmesini yapılandırmak için MyWorkDrive Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden MyWorkDrive eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![image](./media/myworkdrive-tutorial/selectazuread.png)

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![image](./media/myworkdrive-tutorial/a_select_app.png)
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![image](./media/myworkdrive-tutorial/a_new_app.png)

4. Arama kutusuna **MyWorkDrive**seçin **MyWorkDrive** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![image](./media/myworkdrive-tutorial/tutorial_myworkdrive_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı MyWorkDrive sınayın.

Tek çalışmak için oturum açma için Azure AD ne MyWorkDrive karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının MyWorkDrive ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma MyWorkDrive ile'test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[MyWorkDrive test kullanıcısı oluşturma](#create-a-myworkdrive-test-user)**  - kullanıcı Azure AD gösterimini bağlı MyWorkDrive Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve MyWorkDrive uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile MyWorkDrive yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde [Azure portalında](https://portal.azure.com/), **MyWorkDrive** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![image](./media/myworkdrive-tutorial/B1_B2_Select_SSO.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML** modu, çoklu oturum açmayı etkinleştirmek için.

    ![image](./media/myworkdrive-tutorial/b1_b2_saml_sso.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için düğmeyi **temel SAML yapılandırma** iletişim.

    ![image](./media/myworkdrive-tutorial/b1-domains_and_urlsedit.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** başlatılan modu:

    ![image](./media/myworkdrive-tutorial/tutorial_myworkdrive_url.png)

    İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<SERVER.DOMAIN.COM>/SAML/AssertionConsumerService.aspx`

5. Tıklayarak **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![image](./media/myworkdrive-tutorial/tutorial_myworkdrive_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<SERVER.DOMAIN.COM>/Account/Login-saml` 

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek yanıt URL'si ve oturum açma URL'si ile güncelleştirin.  Kendi şirketin MyWorkDrive sunucu konak name:e.g girin.
    > 
    > Yanıt URL'si: `https://yourserver.yourdomain.com/SAML/AssertionConsumerService.aspx`
    > 
    > Oturum açma URL'si:`https://yourserver.yourdomain.com/Account/Login-saml`
    > 
    > Kendi ana bilgisayar adı ve bu değerler için SSL sertifikası ayarlama değilseniz MyWorkDrive istemci Destek ekibine başvurun.

6. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde, Kopyala **simgesi** kopyalamak için **uygulama Federasyon meta verileri URL'sini** tıklatıp **indirme** indirmek için **sertifika (Base64)** bilgisayarınıza kaydedin.

    ![image](./media/myworkdrive-tutorial/tutorial_myworkdrive_certficate.png) 

7. Üzerinde **MyWorkDrive ' ayarlamak** bölümünde, ihtiyacınıza göre uygun URL'yi kopyalayın.

    Not: URL aşağıdaki bildirebilir

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

    ![image](./media/myworkdrive-tutorial/d1_samlsonfigure.png) 

8. Çoklu oturum açma MyWorkDrive tarafında yapılandırmak için indirme **sertifika (Base64), oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** bunları MyWorkDrive veya kopyalama üzerinde el ile yapılandırın ve Azure yapıştırın **Uygulama Federasyon meta verileri URL'sini** MyWorkDrive sunucusu yönetim paneli SAML Azure AD yapılandırma ekranında. Ek bilgi kişi [MyWorkDrive Destek ekibine](mailto:support@myworkdrive.com).

    
### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    ![image](./media/myworkdrive-tutorial/d_users_and_groups.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![image](./media/myworkdrive-tutorial/d_adduser.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![image](./media/myworkdrive-tutorial/d_userproperties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü **brittasimon@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **özellikleri**seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’u seçin.
 
### <a name="create-a-myworkdrive-test-user"></a>MyWorkDrive test kullanıcısı oluşturma

Bu bölümde, Britta Simon MyWorkDrive adlı bir kullanıcı oluşturun. Çalışmak [MyWorkDrive Destek ekibine](mailto:support@myworkdrive.com) MyWorkDrive platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için MyWorkDrive erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**.

    ![image](./media/myworkdrive-tutorial/d_all_applications.png)

2. Uygulamalar listesinde **MyWorkDrive**.

    ![image](./media/myworkdrive-tutorial/tutorial_myworkdrive_app.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![image](./media/myworkdrive-tutorial/d_leftpaneusers.png)

4. Seçin **Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![image](./media/myworkdrive-tutorial/d_assign_user.png)

4. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

5. İçinde **atama Ekle** iletişim kutusunda **atama** düğmesi.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde MyWorkDrive kutucuğa tıkladığınızda, otomatik olarak MyWorkDrive uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
