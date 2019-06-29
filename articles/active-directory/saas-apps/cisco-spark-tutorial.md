---
title: 'Öğretici: Cisco Webex ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Cisco Webex arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: c47894b1-f5df-4755-845d-f12f4c602dc4
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/24/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1dc537a631cd083da0f902fb4fcd44d47756eeba
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67471765"
---
# <a name="tutorial-integrate-cisco-webex-with-azure-active-directory"></a>Öğretici: Cisco Webex Azure Active Directory ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Cisco Webex tümleştirme öğreneceksiniz. Cisco Webex Azure AD ile tümleştirdiğinizde, şunları yapabilirsiniz:

* Cisco Webex erişimi, Azure AD'de denetler.
* Otomatik olarak Cisco Webex için kendi Azure AD hesapları ile oturum açmış olmasını sağlayın.
* Bir merkezi konumda - Azure portalı hesaplarınızı yönetin.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir aboneliğiniz yoksa, alabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/).
* Cisco Webex çoklu oturum açma (SSO) abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD SSO bir test ortamında test edin. Cisco Webex destekler **SP ve IDP** SSO ve destekler başlatılan **otomatik** kullanıcı sağlama.

## <a name="adding-cisco-webex-from-the-gallery"></a>Cisco Webex galeri ekleme

Azure AD'de Cisco Webex tümleştirmesini yapılandırmak için Cisco Webex Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde seçin **Azure Active Directory** hizmeti.
1. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları**.
1. Yeni bir uygulama eklemek için seçin **yeni uygulama**.
1. İçinde **Galeriden Ekle** bölümüne şunu yazın **Cisco Webex** arama kutusuna.
1. Seçin **Cisco Webex** gelen sonuçlar panelinde ve uygulama ekleyin. Uygulama, kiracınıza eklendiği sırada birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Yapılandırma ve Azure AD SSO Cisco adlı bir test kullanıcı kullanarak Webex ile test etme **B.Simon**. Çalışmak SSO için Cisco Webex içinde bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişki oluşturmanız gerekir.

Yapılandırma ve Azure AD SSO ile Cisco Webex sınamak için aşağıdaki yapı taşlarını tamamlayın:

1. **[Azure AD SSO'yu yapılandırma](#configure-azure-ad-sso)**  kullanıcılarınız bu özelliği kullanmak etkinleştirmek için.
2. **[Cisco Webex yapılandırma](#configure-cisco-webex)**  uygulama tarafında SSO ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  Azure AD çoklu oturum açma B.Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  B.Simon Azure AD çoklu oturum açma kullanmak üzere etkinleştirmek için.
5. **[Cisco Webex test kullanıcısı oluşturma](#create-cisco-webex-test-user)**  kullanıcı Azure AD gösterimini bağlı Cisco Webex B.Simon bir karşılığı sağlamak için.
6. **[Test SSO](#test-sso)**  yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO'yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek üzere aşağıdaki adımları izleyin.

1. İçinde [Azure portalında](https://portal.azure.com/), **Cisco Webex** uygulama tümleştirme sayfası, bulma **Yönet** bölümünde ve seçin **çoklu oturum açma**.
1. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** sayfasında **SAML**.
1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında, düzenleme/kalem simgesine tıklayıp **temel SAML yapılandırma** ayarlarını düzenlemek için.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, karşıya yüklenen **hizmet sağlayıcısı meta verileri** dosyasını açıp aşağıdaki adımları uygulayarak uygulamayı yapılandırın:

    >[!Note]
    >Hizmet sağlayıcısı meta veri dosyasından erişmenizi sağlayacak **Cisco Webex** portalı. 

    a. Tıklayın **meta veri dosyasını karşıya yükleme**.

    b. Tıklayarak **klasör logosu** meta veri dosyası seçin ve **karşıya**.

    c. Hizmet sağlayıcısı meta veri dosyasını karşıya yükleme işlemin başarıyla tamamlanmasından sonra **tanımlayıcı** ve **yanıt URL'si** değerlerini alma otomatik olarak doldurulmuş **temel SAML yapılandırma** Bölüm:

    İçinde **oturum açma URL'si** metin değerini yapıştırın **yanıt URL'si**, alır autofilled SP tarafından meta veri dosyasını karşıya yükleme.

5. Cisco Webex uygulama, özel öznitelik eşlemelerini SAML belirteci öznitelikleri yapılandırmanıza ekleyin gerektiren belirli bir biçimde SAML onaylamalarını bekler. Aşağıdaki ekran görüntüsünde, varsayılan öznitelikler listesinde gösterilmiştir. Tıklayarak **Düzenle** öznitelikleri eklemek için simge.

    ![image](common/edit-attribute.png)

6. Yukarıdaki için Ayrıca, Cisco Webex uygulama SAML yanıtta geçirilecek birkaç daha fazla öznitelik bekliyor. İçinde **kullanıcı taleplerini** bölümünde **kullanıcı öznitelikleri** iletişim kutusunda gösterildiği gibi SAML belirteci özniteliği eklemek için aşağıdaki adımları gerçekleştirin tablonun altındaki:
    
    | Ad |  Kaynak özniteliği|
    | ---------------|--------- |
    | Kullanıcı Kimliği | User.userPrincipalName |

    a. Tıklayın **Ekle yeni talep** açmak için **yönetmek, kullanıcı talepleri** iletişim.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. Bırakın **Namespace** boş.

    d. Kaynağı olarak **özniteliği**.

    e. Gelen **kaynak özniteliği** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    f. Tıklayın **Tamam**

    g. **Kaydet**’e tıklayın.

1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde, bulma **Federasyon meta verileri XML** seçip **indirin** sertifikayı indirin ve bilgisayarınıza kaydedin.

   ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. Üzerinde **Cisco Webex kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

   ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

### <a name="configure-cisco-webex"></a>Cisco Webex yapılandırın

1. Oturum [Cisco bulut işbirliği Yönetimi](https://admin.ciscospark.com/) tam yönetici kimlik bilgilerinizle.

2. Seçin **ayarları** altında **kimlik doğrulaması** bölümünde **Değiştir**.

    ![Çoklu oturum açmayı yapılandırın](./media/cisco-spark-tutorial/tutorial_cisco_spark_10.png)
  
3. Seçin **3. taraf kimlik sağlayıcısı tümleştirin. (Gelişmiş)**  ve sonraki ekrana gidin.

4. Üzerinde **IDP meta verileri içeri aktarma** sayfasında, ya da sürükle ve bırak sayfaya Azure AD'ye meta veri dosyası veya dosya tarayıcı seçeneğini bulun ve Azure AD meta veri dosyasını karşıya yüklemek için kullanın. Ardından, **meta verileri (daha güvenli) bir sertifika yetkilisi tarafından imzalanmış bir sertifika gerektir** tıklatıp **sonraki**.

    ![Çoklu oturum açmayı yapılandırın](./media/cisco-spark-tutorial/tutorial_cisco_spark_11.png)

5. Seçin **SSO Bağlantıyı Sına**ve yeni bir tarayıcı sekmesi oturum açtığında, oturum açarak Azure AD kimlik doğrulaması.

6. Geri dönüp **Cisco bulut işbirliği Yönetimi** tarayıcı sekmesinde. Test başarılı olursa seçin **bu testi başarılı. Çoklu oturum açma seçeneğini etkinleştirin** tıklatıp **sonraki**.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, bir test kullanıcısı B.Simon adlı Azure portalında oluşturacaksınız.

1. Azure Portalı'ndaki sol bölmeden seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.
1. Seçin **yeni kullanıcı** ekranın üstünde.
1. İçinde **kullanıcı** özellikleri, aşağıdaki adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. İçinde **kullanıcı adı** alanına username@companydomain.extension. Örneğin, `B.Simon@contoso.com`.
   1. Seçin **Show parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.
   1. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Cisco Webex erişim vererek B.Simon tıklatmalarını sağlarsınız.

1. Azure portalında **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.
1. Uygulamalar listesinde **Cisco Webex**.
1. Uygulamanın genel bakış sayfasında bulma **Yönet** seçin ve bölüm **kullanıcılar ve gruplar**.

   !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

1. Seçin **Kullanıcı Ekle**, ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Kullanıcı ekleme bağlantısı](common/add-assign-user.png)

1. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **B.Simon** kullanıcılar listesinden ardından **seçin** ekranın alt kısmındaki düğmesi.
1. İçinde **atama Ekle** iletişim kutusunda, tıklayın **atama** düğmesi.

### <a name="create-cisco-webex-test-user"></a>Cisco Webex test kullanıcısı oluşturma

Bu bölümde, Britta Simon Cisco Webex adlı bir kullanıcı oluşturun. Bu bölümde, Britta Simon Cisco Webex adlı bir kullanıcı oluşturun.

1. Git [Cisco bulut işbirliği Yönetimi](https://admin.ciscospark.com/) tam yönetici kimlik bilgilerinizle.

2. Tıklayın **kullanıcılar** ardından **kullanıcıları yönetme**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/cisco-spark-tutorial/tutorial_cisco_spark_12.png) 

3. İçinde **yönetme kullanıcı** penceresinde **el ile eklemek veya kullanıcıları değiştirin** tıklatıp **sonraki**.

4. Seçin **adlarını ve e-posta adresi**. Ardından, metin kutusu aşağıdaki gibi doldurun:

    ![Çoklu oturum açmayı yapılandırın](./media/cisco-spark-tutorial/tutorial_cisco_spark_13.png) 

    a. İçinde **ad** metin adı gibi kullanıcı türü **B**.

    b. İçinde **Soyadı** metin türü son gibi kullanıcı adını **Simon**.

    c. İçinde **e-posta adresi** metin kutusuna kullanıcı e-posta adresini yazın ister b.simon@contoso.com.

5. Britta Simon eklemek için artı işaretine tıklayın. Ardından **İleri**'ye tıklayın.

6. İçinde **Hizmetleri kullanıcıları ekleme** penceresinde tıklayın **Kaydet** ardından **son**.

### <a name="test-sso"></a>Test SSO

Erişim Paneli'nde Cisco Webex kutucuğu seçtiğinizde, otomatik olarak SSO'yu ayarlama Cisco Webex için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)