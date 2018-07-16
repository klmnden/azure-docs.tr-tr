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
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: jeedes
ms.openlocfilehash: 79a2bc9d517e3c292268a4a70f08936cb0325fbd
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39053096"
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

- Azure AD aboneliğiniz
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

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **GitHub**seçin **GitHub** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde GitHub](./media/github-tutorial/tutorial_github_addfromgallery.png)

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

1. Azure portalında, üzerinde **GitHub** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/github-tutorial/tutorial_github_samlbase.png)

3. Üzerinde **GitHub etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![GitHub etki alanı ve URL'ler tek oturum açma bilgileri](./media/github-tutorial/tutorial_github_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://github.com/orgs/<entity-id>/sso`

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna bir URL şu biçimi kullanarak: `https://github.com/orgs/<entity-id>`

    > [!NOTE]
    > Bunlar gerçek değerleri olmadığına dikkat edin. Bu değerler gerçek oturum açma URL ve tanımlayıcıdır ile güncelleştirmeniz gerekiyor. Burada benzersiz dize değeri tanımlayıcıda kullanmanızı öneririz. Bu değerleri almak için GitHub yönetici bölümüne gidin.

4. Üzerinde **kullanıcı öznitelikleri** bölümünden **kullanıcı tanımlayıcısı** user.mail olarak.

    ![Çoklu oturum açmayı yapılandırın](./media/github-tutorial/tutorial_github_attribute_new01.png)

5. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/github-tutorial/tutorial_github_certificate.png) 

6. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/github-tutorial/tutorial_general_400.png)

7. Üzerinde **GitHub yapılandırma** bölümünde **GitHub yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![GitHub yapılandırma](./media/github-tutorial/tutorial_github_configure.png) 

8. Farklı bir web tarayıcı penceresinde GitHub kuruluş sitenize yönetici olarak oturum.

9. Gidin **ayarları** tıklatıp **güvenlik**

    ![Ayarlar](./media/github-tutorial/tutorial_github_config_github_03.png)

10. Kontrol **etkinleştirme SAML kimlik doğrulaması** kutusu, çoklu oturum açmayı yapılandırma alanları açıklayacak. Ardından, çoklu oturum açma URL'si Azure AD yapılandırmasını güncelleştirmek için tek oturum açma URL değeri kullanın.

    ![Ayarlar](./media/github-tutorial/tutorial_github_config_github_13.png)

11. Aşağıdaki alanları yapılandırın:

    a. İçinde **oturum açma URL'si** metin kutusu, yapıştırma **SAML çoklu oturum açma hizmeti URL'si** Azure portaldan kopyaladığınız değeri.

    b. İçinde **veren** metin kutusu, yapıştırma **SAML varlık kimliği** Azure portaldan kopyaladığınız değeri.

    c. Açık Not Defteri'nde, Azure portalından indirilen sertifikanın yapıştırın içeriğinize **ortak sertifika** metin.

    ![Ayarlar](./media/github-tutorial/tutorial_github_config_github_051.png)

12. Tıklayarak **Test SAML yapılandırma** onaylamak için hiçbir doğrulama hataları veya SSO sırasında hata.

    ![Ayarlar](./media/github-tutorial/tutorial_github_config_github_06.png)

13. **Kaydet**’e tıklayın

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/github-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/github-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/github-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/github-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
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

![Kullanıcı rolü atayın][200] 

**Britta Simon Github'a atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

2. Uygulamalar listesinde **GitHub**.

    ![Uygulamalar listesinde GitHub bağlantısı](./media/github-tutorial/tutorial_github_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde GitHub kutucuğa tıkladığınızda, otomatik olarak GitHub uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/github-tutorial/tutorial_general_01.png
[2]: ./media/github-tutorial/tutorial_general_02.png
[3]: ./media/github-tutorial/tutorial_general_03.png
[4]: ./media/github-tutorial/tutorial_general_04.png

[100]: ./media/github-tutorial/tutorial_general_100.png

[200]: ./media/github-tutorial/tutorial_general_200.png
[201]: ./media/github-tutorial/tutorial_general_201.png
[202]: ./media/github-tutorial/tutorial_general_202.png
[203]: ./media/github-tutorial/tutorial_general_203.png

