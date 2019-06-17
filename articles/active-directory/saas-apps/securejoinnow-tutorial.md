---
title: 'Öğretici: SecureW2 JoinNow Bağlayıcısı ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: SecureW2 JoinNow Bağlayıcısı ile Azure Active Directory arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 2445b3af-f827-40de-9097-6f5c933d0f53
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5b367befb90ec28ece963d67b479749e1c8ad363
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60340018"
---
# <a name="tutorial-azure-active-directory-integration-with-securew2-joinnow-connector"></a>Öğretici: SecureW2 JoinNow Bağlayıcısı ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile SecureW2 JoinNow bağlayıcı tümleştirme konusunda bilgi edinin.

SecureW2 JoinNow Bağlayıcısı Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- SecureW2 JoinNow bağlayıcı erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan SecureW2 JoinNow bağlayıcıya (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi SecureW2 JoinNow Bağlayıcısı ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik SecureW2 JoinNow bağlayıcı çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden SecureW2 JoinNow bağlayıcı ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-securew2-joinnow-connector-from-the-gallery"></a>Galeriden SecureW2 JoinNow bağlayıcı ekleme
Azure AD'de SecureW2 JoinNow bağlayıcı tümleştirmesini yapılandırmak için SecureW2 JoinNow bağlayıcı Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden SecureW2 JoinNow bağlayıcı eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **SecureW2 JoinNow bağlayıcı**seçin **SecureW2 JoinNow bağlayıcı** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde SecureW2 JoinNow Bağlayıcısı](./media/securejoinnow-tutorial/tutorial_securejoinnow_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve SecureW2 JoinNow "Britta Simon" adlı bir test kullanıcı tabanlı Bağlayıcısı ile Azure AD çoklu oturum açma testi.

Tek iş için oturum açma için Azure AD ne karşılık gelen kullanıcı SecureW2 JoinNow bağlayıcısında bir kullanıcının Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve ilgili kullanıcı SecureW2 JoinNow bağlayıcısında arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma SecureW2 JoinNow Bağlayıcısı ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[SecureW2 JoinNow bağlayıcı test kullanıcısı oluşturma](#create-a-securew2-joinnow-connector-test-user)**  - kullanıcı Azure AD gösterimini bağlı SecureW2 JoinNow bağlayıcısında Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve SecureW2 JoinNow bağlayıcı uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma SecureW2 JoinNow Bağlayıcısı ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **SecureW2 JoinNow bağlayıcı** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/securejoinnow-tutorial/tutorial_securejoinnow_samlbase.png)

3. Üzerinde **SecureW2 JoinNow bağlayıcı etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![SecureW2 JoinNow bağlayıcı etki alanı ve URL'ler tek oturum açma bilgileri](./media/securejoinnow-tutorial/tutorial_securejoinnow_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<organization-identifier>-auth.securew2.com/auth/saml/SSO`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<organization-identifier>-auth.securew2.com/auth/saml`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [SecureW2 JoinNow bağlayıcı istemci Destek ekibine](mailto:support@securew2.com) bu değerleri almak için. 

4. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/securejoinnow-tutorial/tutorial_securejoinnow_certificate.png) 

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/securejoinnow-tutorial/tutorial_general_400.png)

6. Çoklu oturum açmayı yapılandırma **SecureW2 JoinNow bağlayıcı** tarafı, indirilen göndermek için ihtiyacınız **meta veri XML** için [SecureW2 JoinNow bağlayıcı Destek ekibine](mailto:support@securew2.com). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/securejoinnow-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/securejoinnow-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/securejoinnow-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/securejoinnow-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-securew2-joinnow-connector-test-user"></a>SecureW2 JoinNow bağlayıcı test kullanıcısı oluşturma

Bu bölümde, Britta Simon SecureW2 JoinNow bağlayıcısında adlı bir kullanıcı oluşturun. Çalışmak [SecureW2 JoinNow bağlayıcı istemci Destek ekibine](mailto:support@securew2.com) SecureW2 JoinNow bağlayıcı platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, SecureW2 JoinNow bağlayıcısına erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon SecureW2 JoinNow bağlayıcıya atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **SecureW2 JoinNow bağlayıcı**.

    ![Uygulamalar listesini SecureW2 JoinNow Bağlayıcısı bağlantıyı](./media/securejoinnow-tutorial/tutorial_securejoinnow_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

**Uygulamayı test etmek için aşağıdaki adımları gerçekleştirin:** 

a. SecureW2 JoinNow bağlayıcı İstemcisi'ni açmak için listeden uygun Cihazınızı seçin ve tıklayın **oturum** düğmesi.

b. Varsayılan tarayıcıyı ve kimlik doğrulaması için Azure portalına yeniden yönlendirilmiş olmalıdır.

c. Başarılı kimlik doğrulamasını SecureW2 JoinNow bağlayıcı ilk giriş sayfasına geri döndürmesi gerekir.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/securejoinnow-tutorial/tutorial_general_01.png
[2]: ./media/securejoinnow-tutorial/tutorial_general_02.png
[3]: ./media/securejoinnow-tutorial/tutorial_general_03.png
[4]: ./media/securejoinnow-tutorial/tutorial_general_04.png

[100]: ./media/securejoinnow-tutorial/tutorial_general_100.png

[200]: ./media/securejoinnow-tutorial/tutorial_general_200.png
[201]: ./media/securejoinnow-tutorial/tutorial_general_201.png
[202]: ./media/securejoinnow-tutorial/tutorial_general_202.png
[203]: ./media/securejoinnow-tutorial/tutorial_general_203.png

