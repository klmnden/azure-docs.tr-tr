---
title: 'Öğretici: Azure Active Directory GitHub ile tümleştirme | Microsoft Docs'
description: Azure Active Directory ve GitHub arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8761f5ca-c57c-4a7e-bf14-ac0421bd3b5e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2018
ms.author: jeedes
ms.openlocfilehash: dbd4634c575fd4f1886d3e7714ef9ddabbde0f8a
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49341166"
---
# <a name="tutorial-azure-active-directory-integration-with-github"></a>Öğretici: Azure Active Directory GitHub ile tümleştirme

Bu öğreticide, GitHub, Azure Active Directory (Azure AD) ile tümleştirme konusunda bilgi edinin.

Github'da Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- GitHub erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak GitHub'ın (çoklu oturum açma) ile Azure AD hesaplarına açan, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

GitHub Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir GitHub çoklu oturum açma abonelik etkin.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. GitHub galeri ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-github-from-the-gallery"></a>GitHub galeri ekleme
Azure AD'de GitHub tümleştirmesini yapılandırmak için GitHub galerideki yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden GitHub eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![image](./media/github-tutorial/selectazuread.png)

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![image](./media/github-tutorial/a_select_app.png)
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![image](./media/github-tutorial/a_new_app.png)

4. Arama kutusuna **GitHub**seçin **GitHub** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![image](./media/github-tutorial/tutorial_github_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı GitHub ile test edin.

Tek çalışmak için oturum açma için Azure AD ne karşılık gelen kullanıcı github'da bir kullanıcının Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili GitHub kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma GitHub ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[GitHub test kullanıcısı oluşturma](#create-a-github-test-user)**  - kullanıcı Azure AD gösterimini bağlı github'da Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve GitHub uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile GitHub yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde [Azure portalında](https://portal.azure.com/), **GitHub** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![image](./media/github-tutorial/b1_b2_select_sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunu tıklatın **seçin** için **SAML** modu, çoklu oturum açmayı etkinleştirmek için.

    ![image](./media/github-tutorial/b1_b2_saml_sso.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için düğmeyi **temel SAML yapılandırma** iletişim.

    ![image](./media/github-tutorial/b1-domains_and_urlsedit.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![image](./media/github-tutorial/tutorial_github_url.png) 

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://github.com/orgs/<entity-id>/sso`

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna bir URL şu biçimi kullanarak: `https://github.com/orgs/<entity-id>`

    > [!NOTE]
    > Bunlar gerçek değerleri olmadığına dikkat edin. Bu değerler gerçek oturum açma URL ve tanımlayıcıdır ile güncelleştirmeniz gerekiyor. Burada benzersiz dize değeri tanımlayıcıda kullanmanızı öneririz. Bu değerleri almak için GitHub yönetici bölümüne gidin.

5. GitHub uygulama belirli bir biçimde SAML onaylamalarını bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** uygulama tümleştirme sayfasında bölümü. Tıklayın **Düzenle** açmak için düğmeyi **kullanıcı öznitelikleri** iletişim.

    ![image](./media/github-tutorial/i3-attribute.png)

6. İçinde **kullanıcı taleplerini** bölümünde **kullanıcı öznitelikleri** iletişim kutusunda, SAML belirteci özniteliği yukarıdaki görüntüde gösterilen şekilde yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    a. Tıklayın **Düzenle** açmak için düğmeyi **yönetmek, kullanıcı talepleri** iletişim.

    ![image](./media/github-tutorial/i2-attribute.png)

    ![image](./media/github-tutorial/i4-attribute.png)

    b. Gelen **kaynak özniteliği** listesinde, öznitelik değeri seçin.

    c. **Kaydet**’e tıklayın.
 
7. İçinde **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** ve bilgisayarınıza kaydedin.

    ![image](./media/github-tutorial/tutorial_github_certficate.png)

8. Üzerinde **GitHub'ı ayarlama** bölümünde, ihtiyacınıza göre uygun URL'yi kopyalayın.

    a. Oturum Açma URL'si:

    b. Azure AD tanımlayıcısı

    c. Oturum Kapatma URL'si

    ![image](./media/github-tutorial/d1_samlsonfigure.png) 

9. Farklı bir web tarayıcı penceresinde GitHub kuruluş sitenize yönetici olarak oturum.

10. Gidin **ayarları** tıklatıp **güvenlik**

    ![Ayarlar](./media/github-tutorial/tutorial_github_config_github_03.png)

11. Kontrol **etkinleştirme SAML kimlik doğrulaması** kutusu, çoklu oturum açmayı yapılandırma alanları açıklayacak. Ardından, çoklu oturum açma URL'si Azure AD yapılandırmasını güncelleştirmek için tek oturum açma URL değeri kullanın.

    ![Ayarlar](./media/github-tutorial/tutorial_github_config_github_13.png)

12. Aşağıdaki alanları yapılandırın:

    ![Ayarlar](./media/github-tutorial/tutorial_github_config_github_051.png)

    a. İçinde **oturum açma URL'si** metin kutusu, yapıştırma **oturum açma URL'si** Azure portaldan kopyaladığınız değeri.

    b. İçinde **veren** metin kutusu, yapıştırma **Azure AD tanımlayıcısı** Azure portaldan kopyaladığınız değeri.

    c. Açık Not Defteri'nde, Azure portalından indirilen sertifikanın yapıştırın içeriğinize **ortak sertifika** metin.

    d. Tıklayarak **Düzenle** düzenleme simgesi **imza yöntemi** ve **Özet yöntemi** gelen **RSA SHA1** ve **SHA1**için **RSA-SHA256** ve **SHA256** aşağıda gösterildiği gibi.

    ![image](./media/github-tutorial/tutorial_github_sha.png) 
    
13. Tıklayarak **Test SAML yapılandırma** onaylamak için hiçbir doğrulama hataları veya SSO sırasında hata.

    ![Ayarlar](./media/github-tutorial/tutorial_github_config_github_06.png)

14. **Kaydet**’e tıklayın

> [!NOTE]
> Çoklu oturum açmayı github'da github'da belirli bir kuruluş için doğrular ve Github'daki kimliğini değiştirmez. Bu nedenle, kullanıcının GitHub.com oturumunu dolmuşsa, GitHub'ın kimliği/parola ile çoklu oturum açma işlemi sırasında kimlik doğrulaması istenebilir.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    ![image](./media/github-tutorial/d_users_and_groups.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![image](./media/github-tutorial/d_adduser.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![image](./media/github-tutorial/d_userproperties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü **brittasimon@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **özellikleri**seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’u seçin.
 
### <a name="create-a-github-test-user"></a>GitHub test kullanıcısı oluşturma

Bu bölümün amacı, Github'da Britta Simon adlı bir kullanıcı oluşturmaktır. GitHub otomatik kullanıcı hazırlama, varsayılan olarak etkin olan destekler. Daha fazla ayrıntı bulabilirsiniz [burada](github-provisioning-tutorial.md) otomatik kullanıcı sağlamayı yapılandırma.

**Kullanıcı el ile oluşturmanız gerekiyorsa, aşağıdaki adımları uygulayın:**

1. GitHub şirketinizin sitesi için bir yönetici olarak oturum açın.

2. Tıklayın **kişiler**.

    ![Kişiler](./media/github-tutorial/tutorial_github_config_github_08.png "kişiler")

3. Tıklayın **davet üye**.

    ![Kullanıcıları davet](./media/github-tutorial/tutorial_github_config_github_09.png "kullanıcılar davet edin")

4. Üzerinde **davet üye** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    a. İçinde **e-posta** metin Britta Simon hesabı e-posta adresini yazın.

    ![Kişileri davet edin](./media/github-tutorial/tutorial_github_config_github_10.png "kişileri davet edin")

    b. Tıklayın **Davet Gönder**.

    ![Kişileri davet edin](./media/github-tutorial/tutorial_github_config_github_11.png "kişileri davet edin")

    > [!NOTE]
    > Azure Active Directory hesap sahibinin e-posta alır ve bunu etkinleştirilmeden önce hesabını onaylamak için bağlantıyı izleyin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, GitHub için erişimi vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**.

    ![image](./media/github-tutorial/d_all_applications.png)

2. Uygulamalar listesinde **GitHub**.

    ![image](./media/github-tutorial/tutorial_github_app.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![image](./media/github-tutorial/d_leftpaneusers.png)

4. Seçin **Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![image](./media/github-tutorial/d_assign_user.png)

4. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

5. İçinde **atama Ekle** iletişim kutusunda **atama** düğmesi.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde GitHub kutucuğa tıkladığınızda, otomatik olarak GitHub uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)


