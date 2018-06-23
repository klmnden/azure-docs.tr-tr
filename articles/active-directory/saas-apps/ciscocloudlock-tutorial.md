---
title: 'Öğretici: Azure Active Directory ile tümleştirme bulut güvenlik doku ile | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ve bulut güvenlik doku arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 549e8810-1b3b-4351-bf4b-f07de98980d1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: jeedes
ms.openlocfilehash: 5ec729c6f82cec503cae2fa057f5842849004ac7
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36318254"
---
# <a name="tutorial-azure-active-directory-integration-with-the-cloud-security-fabric"></a>Öğretici: Azure Active Directory ile tümleştirme bulut güvenlik doku ile

Bu öğreticide, Azure Active Directory (Azure AD) ile bulut güvenlik doku tümleştirmek öğrenin.

Bulut güvenlik doku Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Bulut güvenlik doku erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak bulut güvenlik dokuya (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme bulut güvenlik doku ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir bulut güvenlik doku çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden bulut güvenlik doku ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-the-cloud-security-fabric-from-the-gallery"></a>Galeriden bulut güvenlik doku ekleme
Azure AD bulut güvenlik doku tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden bulut güvenlik doku eklemeniz gerekir.

**Galeriden bulut güvenlik doku eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **bulut güvenlik doku**seçin **bulut güvenlik doku** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde bulut güvenlik yapı](./media/ciscocloudlock-tutorial/tutorial_ciscocloudlock_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı bulut güvenlik doku ile test etme.

Tekli çalışmaya oturum için Azure AD bulut güvenlik doku karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının bulut güvenlik doku ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma bulut güvenlik doku ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bulut güvenlik doku test kullanıcısı oluşturma](#create-a-the-cloud-security-fabric-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlantılı bulut güvenlik doku sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma, bulut güvenlik doku uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile bulut güvenlik doku yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **bulut güvenlik doku** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma iletişim kutusu](./media/ciscocloudlock-tutorial/tutorial_ciscocloudlock_samlbase.png)

3. Üzerinde **bulut güvenlik doku etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Bulut güvenlik doku etki alanı ve URL'ler tek oturum açma bilgileri](./media/ciscocloudlock-tutorial/tutorial_ciscocloudlock_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, bir URL yazın:
    | |
    |--|
    | `https://platform.cloudlock.com` |
    | `https://app.cloudlock.com` |

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın:
    | |
    |--|
    | `https://platform.cloudlock.com/gate/saml/sso/<subdomain>` |
    | `https://app.cloudlock.com/gate/saml/sso/<subdomain>` |

    > [!NOTE]
    > Tanımlayıcı değeri gerçek değil. Değerin gerçek tanımlayıcısı ile güncelleştirin. Kişi [bulut güvenlik doku istemci destek ekibi](mailto:support@cloudlock.com) değeri alınamıyor. 

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/ciscocloudlock-tutorial/tutorial_ciscocloudlock_certificate.png)

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/ciscocloudlock-tutorial/tutorial_general_400.png)

6. Çoklu oturum açma yapılandırmak için **bulut güvenlik doku** yan, indirilen göndermek için ihtiyacınız **meta veri XML** için [bulut güvenlik doku destek ekibi](mailto:support@cloudlock.com). Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/ciscocloudlock-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/ciscocloudlock-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/ciscocloudlock-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/ciscocloudlock-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.

### <a name="create-a-the-cloud-security-fabric-test-user"></a>Bulut güvenlik doku test kullanıcısı oluşturma

Bu bölümde, bulut güvenlik yapıda Britta Simon adlı bir kullanıcı oluşturun. Çalışmak [bulut güvenlik doku destek ekibi](mailto:support@cloudlock.com) bulut güvenlik doku platform kullanıcıları eklemek için. Kullanıcıların oluşturulan ve çoklu oturum açma kullanmadan önce etkinleştirilmelidir. 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, bulut güvenlik doku erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200]

**Britta Simon bulut güvenlik dokuya atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201]

2. Uygulamalar listesinde **bulut güvenlik doku**.

    ![Uygulamalar listesinde bulut güvenlik doku bağlantı](./media/ciscocloudlock-tutorial/tutorial_ciscocloudlock_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli bulut güvenlik doku parçasında tıklattığınızda, otomatik olarak bulut güvenlik Fabric uygulamanızı açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/ciscocloudlock-tutorial/tutorial_general_01.png
[2]: ./media/ciscocloudlock-tutorial/tutorial_general_02.png
[3]: ./media/ciscocloudlock-tutorial/tutorial_general_03.png
[4]: ./media/ciscocloudlock-tutorial/tutorial_general_04.png

[100]: ./media/ciscocloudlock-tutorial/tutorial_general_100.png

[200]: ./media/ciscocloudlock-tutorial/tutorial_general_200.png
[201]: ./media/ciscocloudlock-tutorial/tutorial_general_201.png
[202]: ./media/ciscocloudlock-tutorial/tutorial_general_202.png
[203]: ./media/ciscocloudlock-tutorial/tutorial_general_203.png
