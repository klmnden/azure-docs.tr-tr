---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Huddle | Microsoft Docs'
description: Azure Active Directory ve Huddle arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 8389ba4c-f5f8-4ede-b2f4-32eae844ceb0
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/08/2018
ms.author: jeedes
ms.openlocfilehash: d9d145aa5da636574426f1ff4ad978eb857ab252
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54827933"
---
# <a name="tutorial-azure-active-directory-integration-with-huddle"></a>Öğretici: Huddle ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Huddle tümleştirme konusunda bilgi edinin.

Azure AD ile Huddle tümleştirme ile aşağıdaki avantajları sağlar:

- Huddle erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için Huddle (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Huddle yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik Huddle çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Huddle ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-huddle-from-the-gallery"></a>Galeriden Huddle ekleme

Azure AD'de Huddle tümleştirmesini yapılandırmak için Huddle Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Huddle eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Huddle**. Seçin **Huddle** sonuçlar paneli ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/huddle-tutorial/tutorial_huddle_addfromgallery.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı Huddle ile test etme

Tek iş için oturum açma için Azure AD ne Huddle karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Huddle ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Huddle ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Huddle test kullanıcısı oluşturma](#creating-a-huddle-test-user)**  - kullanıcı Azure AD gösterimini bağlı Huddle Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Huddle uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Huddle yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Huddle** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Tıklayın **değişiklik çoklu oturum açma modunu** seçmek için ekranın en üstünde **SAML** modu.

    ![Çoklu oturum açmayı yapılandırın](./media/huddle-tutorial/tutorial_general_300.png)

3. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunu tıklatın **seçin** için **SAML** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açmayı yapılandırın](./media/huddle-tutorial/tutorial_general_301.png)

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/huddle-tutorial/tutorial_general_302.png)

5. Üzerinde **temel SAML yapılandırma** bölümü, uygulamada yapılandırmak istiyorsanız, aşağıdaki adımları gerçekleştirin **IDP** intiated modu:

    > [!NOTE]
    > Huddle örneğinizin aşağıdaki girdiğiniz etki alanından otomatik olarak algılanır.

    ![Etki alanı ve URL'ler tek oturum açma bilgileri huddle](./media/huddle-tutorial/tutorial_huddle_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL:

    | | |
    |--|--|
    | `https://login.huddle.net`|
    | `https://login.huddle.com`|
    | |

    b. İçinde **yanıt URL'si** metin kutusuna bir URL:

    | | |
    |--|--|
    | `https://login.huddle.net/saml/browser-sso`|
    | `https://login.huddle.com/saml/browser-sso`|
    | `https://login.huddle.com/saml/idp-initiated-sso`|
    | |

    c. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![Etki alanı ve URL'ler tek oturum açma bilgileri huddle](./media/huddle-tutorial/tutorial_huddle_url1.png)

    İçinde **oturum açma URL'si** metin herhangi biri şu biçimi kullanarak URL'yi yazın:

    | | |
    |--|--|
    | `https://<customsubdomain>.huddle.com`|
    | `https://us.huddle.com`|
    | |

    > [!NOTE]
    > Oturum açma URL değeri, gerçek değil. Bu değer, gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [Huddle istemci Destek ekibine](https://huddle.zendesk.com) bu değeri alınamıyor.

6. Üzerinde **yukarı çoklu oturum açma SAML ile Ayarla** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** olarak başına uygun sertifikayı indirmek için gereksinim ve bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/huddle-tutorial/tutorial_huddle_certificate.png)

7. Üzerinde **Huddle kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Çoklu oturum açmayı yapılandırın](./media/huddle-tutorial/tutorial_huddle_configure.png)

8. Çoklu oturum açmayı yapılandırma **Huddle** tarafı, yüklediğiniz sertifikanın ve kopyalanan URL'leri göndermek için ihtiyacınız **ayarlanan** **Huddle** bölümü için Azure portalından [Huddle istemci Destek ekibine](https://huddle.zendesk.com). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

    >[!NOTE]
    > Çoklu oturum açma Huddle destek ekibi tarafından etkinleştirmesi gerekir. Yapılandırma tamamlandıktan sonra bir bildirim alırsınız.

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    ![Azure AD kullanıcısı oluşturun][100]

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/huddle-tutorial/create_aaduser_01.png) 

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/huddle-tutorial/create_aaduser_02.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü **brittasimon@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **özellikleri**seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’u seçin.

### <a name="creating-a-huddle-test-user"></a>Huddle test kullanıcısı oluşturma

Huddle için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların Huddle sağlanması gerekir. Huddle söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **Huddle** şirketinizin sitesi yöneticisi olarak.

2. Tıklayın **çalışma**.

3. Tıklayın **kişiler \> kişileri davet edin**.

    ![Kişiler](./media/huddle-tutorial/IC787838.png "kişiler")

4. İçinde **yeni bir davet oluşturma** bölümünde, aşağıdaki adımları gerçekleştirin:
  
    ![Yeni davet](./media/huddle-tutorial/IC787839.png "yeni davet")
  
    a. İçinde **insanları katılmaya davet etmek için bir takım seçin** listesinden **takım**.

    b. Tür **e-posta adresi** geçerli bir Azure AD hesabı içinde sağlamak istediğiniz **davet etmek istediğiniz kişilerin e-posta adresi girin** metin.

    c. Tıklayın **davet**.

    >[!NOTE]
    > Azure AD hesap sahibinin etkin hale gelir önce hesabı onaylamak için bir bağlantı içeren bir e-posta alırsınız.

>[!NOTE]
>Azure AD kullanıcı hesapları sağlamak için herhangi bir Huddle kullanıcı hesabı oluşturma araçları veya Huddle tarafından sağlanan API'leri kullanabilirsiniz.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için Huddle erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

2. Uygulamalar listesinde **Huddle**.

    ![Çoklu oturum açmayı yapılandırın](./media/huddle-tutorial/tutorial_huddle_app.png) 

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. İçinde **atama Ekle** iletişim kutusunda **atama** düğmesi.

### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Huddle kutucuğa tıkladığınızda, otomatik olarak Huddle uygulamanın oturum açma sayfası almanız gerekir.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/huddle-tutorial/tutorial_general_01.png
[2]: ./media/huddle-tutorial/tutorial_general_02.png
[3]: ./media/huddle-tutorial/tutorial_general_03.png
[4]: ./media/huddle-tutorial/tutorial_general_04.png
[100]: ./media/huddle-tutorial/tutorial_general_100.png
[200]: ./media/huddle-tutorial/tutorial_general_200.png
[201]: ./media/huddle-tutorial/tutorial_general_201.png
[202]: ./media/huddle-tutorial/tutorial_general_202.png
[203]: ./media/huddle-tutorial/tutorial_general_203.png
