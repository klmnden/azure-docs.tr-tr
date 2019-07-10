---
title: 'Öğretici: ServiceNow ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory hem de Servicenow'a arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: a5a1a264-7497-47e7-b129-a1b5b1ebff5b
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/27/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8ba516aa2c3d2decaa4962f1ccd0394ebe9a4a62
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67706120"
---
# <a name="tutorial-integrate-servicenow-with-azure-active-directory"></a>Öğretici: ServiceNow'ı Azure Active Directory ile tümleştirme

Bu öğreticide, ServiceNow, Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. ServiceNow'ı Azure AD ile tümleştirdiğinizde, şunları yapabilirsiniz:

* ServiceNow erişimi, Azure AD'de denetler.
* Otomatik olarak ServiceNow için kendi Azure AD hesapları ile oturum açmış olmasını sağlayın.
* Bir merkezi konumda - Azure portalı hesaplarınızı yönetin.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir aboneliğiniz yoksa, alabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/).
* ServiceNow çoklu oturum açma (SSO) abonelik etkin.
* Servicenow'ı, bir örnek veya Kiracı, ServiceNow, Calgary sürümü veya üzeri
* Servicenow'ı hızlı, ServiceNow Express, Helsinki sürümü örneğini veya üzeri
* ServiceNow kiracısına sahip olmanız gerekir [birden çok sağlayıcı tek oturum üzerinde eklentisi](https://wiki.servicenow.com/index.php?title=Multiple_Provider_Single_Sign-On#gsc.tab=0) etkin. Bunu aşağıdaki yöntemlerle yapabilirsiniz [hizmet isteği göndererek](https://hi.service-now.com).
* Otomatik yapılandırma için için ServiceNow Çoklu Sağlayıcı eklentisini etkinleştirin.
* Yüklemek için uygun depolama ve ServiceNow Klasik uygulama için arama yapın ve tıklayın için ihtiyaç duyduğunuz ServiceNow Klasik (mobil) uygulamayı indirin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD SSO bir test ortamında test edin. ServiceNow'ı destekleyen **SP** SSO ve destekler başlatılan [ **otomatik** kullanıcı sağlamayı](servicenow-provisioning-tutorial.md).

Servicenow'ı Klasik (mobil) uygulama artık yapılandırılabilir SSO'yu etkinleştirme için Azure AD ile ve her ikisini de destekler **Android** ve **IOS** kullanıcılar. Bu öğreticide, yapılandırma ve Azure AD SSO bir test ortamında test edin.

## <a name="adding-servicenow-from-the-gallery"></a>ServiceNow galeri ekleme

Servicenow'ı Azure AD'ye tümleştirmesini yapılandırmak için ServiceNow Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde seçin **Azure Active Directory** hizmeti.
1. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları**.
1. Yeni bir uygulama eklemek için seçin **yeni uygulama**.
1. İçinde **Galeriden Ekle** bölümüne şunu yazın **ServiceNow** arama kutusuna.
1. Seçin **ServiceNow** gelen sonuçlar panelinde ve uygulama ekleyin. Uygulama, kiracınıza eklendiği sırada birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açma testi

Yapılandırma ve Azure AD SSO adlı bir test kullanıcı kullanarak ServiceNow ile test etme **B.Simon**. Çalışmak SSO için servicenow'ı bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişki oluşturmanız gerekir.

Yapılandırma ve Azure AD SSO ServiceNow ile test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. **[Azure AD SSO'yu yapılandırma](#configure-azure-ad-sso)**  kullanıcılarınız bu özelliği kullanmak etkinleştirmek için.
2. **[ServiceNow'ı yapılandırma](#configure-servicenow)**  uygulama tarafında SSO ayarlarını yapılandırmak için.
3. **[Azure AD SSO'yu yapılandırma için ServiceNow Express](#configure-azure-ad-sso-for-servicenow-express)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
4. **[ServiceNow Express SSO'yu yapılandırarak](#configure-servicenow-express-sso)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
5. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  Azure AD çoklu oturum açma B.Simon ile test etmek için.
6. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  B.Simon Azure AD çoklu oturum açma kullanmak üzere etkinleştirmek için.
7. **[Servicenow'ı test kullanıcısı oluşturma](#create-servicenow-test-user)**  kullanıcı Azure AD gösterimini bağlı ServiceNow B.Simon bir karşılığı sağlamak için.
8. **[Test SSO](#test-sso)**  yapılandırma çalışıp çalışmadığını doğrulayın.
9. **[Servicenow'ı Klasik (mobil) için test SSO](#test-sso-for-servicenow-classic-mobile)**  yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO'yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek üzere aşağıdaki adımları izleyin.

1. İçinde [Azure portalında](https://portal.azure.com/), **ServiceNow** uygulama tümleştirme sayfası, bulma **Yönet** bölümünde ve seçin **çoklu oturum açma**.
1. Üzerinde **çoklu oturum açma yöntemini seçin** sayfasında **SAML**.
1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında, düzenleme/kalem simgesine tıklayıp **temel SAML yapılandırma** ayarlarını düzenlemek için.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<instance-name>.service-now.com/navpage.do`

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<instance-name>.service-now.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Öğreticinin ilerleyen bölümlerinde açıklanan tanımlayıcısı ve gerçek oturum açma URL'si ile bu değerleri güncelleştirmeniz gerekir. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde, bulma **sertifika (Base64)** seçip **indirin** sertifikayı indirin ve bilgisayarınıza kaydedin.

   ![Sertifika indirme bağlantısı](common/certificatebase64.png)

   a. Kopyalamak için Kopyala düğmesine **uygulama Federasyon meta verileri URL'sini** bu uygulama Federasyon meta veri URL'si, öğreticinin ilerleyen bölümlerinde kullanılacak şekilde kopyalayıp Not Defteri'ne yapıştırın.

    b. Tıklayarak **indirme** indirmek için **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

1. Üzerinde **servicenow'ı kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

   ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

### <a name="configure-servicenow"></a>ServiceNow'ı yapılandırma

1. ServiceNow uygulamanızı yönetici olarak oturum açın.

2. Etkinleştirme **tümleştirmesi - birden çok sağlayıcı tek oturum açma yükleyicisini** sonraki adımları izleyerek Eklentisi:

    a. Sol taraftaki gezinti bölmesinde, arama **sistem tanımı** arama bölümüne ve ardından **eklentileri**.

    ![Eklenti etkinleştirme](./media/servicenow-tutorial/tutorial_servicenow_03.png "eklentisini etkinleştirme")

    b. Arama **tümleştirmesi - birden çok sağlayıcı tek oturum açma yükleyicisini**.

     ![Eklenti etkinleştirme](./media/servicenow-tutorial/tutorial_servicenow_04.png "eklentisini etkinleştirme")

    c. Eklentiyi seçin. Sağ tıklatın ve seçin **etkinleştir/yükseltme**.

     ![Eklenti etkinleştirme](./media/servicenow-tutorial/tutorial_activate.png "eklentisini etkinleştirme")

    d. Tıklayın **etkinleştirme** düğmesi.

     ![Eklenti etkinleştirme](./media/servicenow-tutorial/tutorial_activate1.png "eklentisini etkinleştirme")

3. Sol taraftaki gezinti bölmesinde, arama **Çoklu Sağlayıcı SSO** arama bölümüne ve ardından **özellikleri**.

    ![Uygulama URL'sini Yapılandır](./media/servicenow-tutorial/tutorial_servicenow_06.png "uygulama URL'sini yapılandırın")

4. Üzerinde **Çoklu Sağlayıcı SSO özelliklerini** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Uygulama URL'sini Yapılandır](./media/servicenow-tutorial/ic7694981.png "uygulama URL'sini yapılandırın")

    * Olarak **Çoklu Sağlayıcı SSO etkinleştirme**seçin **Evet**.
  
    * Olarak **kullanıcıların ve kullanıcı tabloya tüm kimlik sağlayıcılardan gelen otomatik içeri aktarma**seçin **Evet**.

    * Olarak **etkin hata ayıklama için birden çok sağlayıcı SSO tümleştirme günlüğü**seçin **Evet**.

    * İçinde **kullanıcı alanı tablo...**  metin kutusuna **user_name**.
  
    * **Kaydet**’e tıklayın.

5. Burada iki yolla **ServiceNow** yapılandırılabilir - otomatik ve el ile.

6. Yapılandırma **ServiceNow** izleyin otomatik olarak aşağıdaki adımları:

    * Geri dönüp **ServiceNow** çoklu oturum açma sayfasında Azure Portalı'nda.

    * Tek bir tıklamayla hizmeti yapılandırmak için ServiceNow diğer bir deyişle, Azure için AD otomatik olarak yapılandırma ServiceNow SAML tabanlı kimlik doğrulaması için sağlanır. Bu hizmet etkinleştirmek için Git **servicenow'ı yapılandırma** bölümünde **yapılandırma ServiceNow** yapılandırma oturum açmak için.

        ![Çoklu oturum açmayı yapılandırın](./media/servicenow-tutorial/tutorial_servicenow_configure.png)

    * ServiceNow örnek adı, yönetici kullanıcı adı ve yönetici parola girin **yapılandırma oturum açma** tıklayın ve form **Şimdi Yapılandır**. Belirtilen yönetici kullanıcı adı olmalıdır Not **security_admin** çalışması için bu ServiceNow atanmış rol. Aksi takdirde, SAML kimlik sağlayıcısı Azure AD'yi kullanacak şekilde servicenow'ı el ile yapılandırmak için tıklayın **çoklu oturum açmayı el ile yapılandırmanız** ve kopyalama **SAML çoklu oturum açma hizmeti URL'sioturumkapatmaURL'siveSAMLvarlıkkimliği** hızlı başvuru bölümünden.

        ![Uygulama URL'sini Yapılandır](./media/servicenow-tutorial/configure.png "uygulama URL'sini yapılandırın")

    * ServiceNow uygulamanızı yönetici olarak oturum açın.

    * Gerekli tüm ayarları yapılandırılır otomatik yapılandırmayı **ServiceNow** yan ancak **X.509 sertifikası** varsayılan olarak etkin değildir. El ile kimlik sağlayıcınızda ServiceNow için eşlemeniz gerekir. izleyin aynı için aşağıdaki adımları:

    * Sol taraftaki gezinti bölmesinde, arama **Çoklu Sağlayıcı SSO** arama bölümüne ve ardından **kimlik sağlayıcıları**.

        ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/tutorial_servicenow_07.png "çoklu oturum açmayı yapılandırın")

    * Otomatik olarak oluşturulan kimlik sağlayıcısına tıklayın

        ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/tutorial_servicenow_08.png "çoklu oturum açmayı yapılandırın")

    *  Üzerinde **kimlik sağlayıcısı** bölümünde, aşağıdaki adımları gerçekleştirin:

        ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/automatic_config.png "çoklu oturum açmayı yapılandırın")

        * İçinde **adı** metin yapılandırmanız için bir ad yazın (örneğin, **Microsoft Azure Federasyon çoklu oturum açma**).

        * Lütfen doldurulmuş kaldırın **kimlik sağlayıcısının SingleLogoutRequest** metin değeri.

        * Kopyalama **ServiceNow giriş sayfası** değeri, yapıştırın **oturum açma URL'si** metin kutusunda **ServiceNow temel SAML yapılandırma** bölümü Azure portalı.

            > [!NOTE]
            > ServiceNow örneği giriş sayfası, bir yapıdır, **ServiceNow Kiracı URL'si** ve **/navpage.do** (örneğin:`https://fabrikam.service-now.com/navpage.do`).

        * Kopyalama **varlık kimliği / veren** değeri, yapıştırın **tanımlayıcı** metin kutusunda **ServiceNow temel SAML yapılandırma** bölümü Azure portalı.

        * Lütfen emin olun **Nameıd ilke** ayarlanır `urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified` değeri. 

    * Ekranı aşağı kaydırarak **X.509 sertifikası** bölümünden **Düzenle**.

        ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/tutorial_servicenow_09.png "çoklu oturum açmayı yapılandırın")

    * Sertifikayı seçin ve sertifika eklemek için sağ ok simgesine tıklayın

        ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/tutorial_servicenow_11.png "çoklu oturum açmayı yapılandırın")

    * **Kaydet**’e tıklayın.

    * Tıklayarak **Test Bağlantısı** sayfanın üst sağ köşesindeki.

        ![Eklenti etkinleştirme](./media/servicenow-tutorial/tutorial_activate2.png "eklentisini etkinleştirme")

    * Tıklayıp sonra **Bağlantıyı Sına**, gerek duyduğunuz kimlik bilgilerini girmek için açılır pencere alır ve sonuçları sayfası aşağıda gösterilmektedir. **SSO oturum kapatma Test sonuçlarını** hata beklenmektedir Lütfen hatasını görmezden Gel ve tıklayın **etkinleştirme** düğmesi.

        ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/servicenowactivate.png "çoklu oturum açmayı yapılandırın")
  
7. Yapılandırma **ServiceNow** el ile izleyerek aşağıdaki adımları:

    * ServiceNow uygulamanızı yönetici olarak oturum açın.

    * Sol taraftaki gezinti bölmesinde **kimlik sağlayıcıları**.

        ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/tutorial_servicenow_07.png "çoklu oturum açmayı yapılandırın")

    * Üzerinde **kimlik sağlayıcıları** iletişim kutusunda, tıklayın **yeni**.

        ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/ic7694977.png "çoklu oturum açmayı yapılandırın")

    * Üzerinde **kimlik sağlayıcıları** iletişim kutusunda, tıklayın **SAML**.

        ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/ic7694978.png "çoklu oturum açmayı yapılandırın")

    * Üzerinde **kimlik sağlayıcısı meta verileri içeri aktarma** açılan, aşağıdaki adımları gerçekleştirin:

        ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/idp.png "çoklu oturum açmayı yapılandırın")

        * Girin **uygulama Federasyon meta verileri URL'sini** , Azure Portalı'ndan kopyaladığınız.

        * **İçeri Aktar**’a tıklayın.

    * IDP meta veri URL'sini okur ve tüm alanları bilgileri doldurur.

        ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/ic7694982.png "çoklu oturum açmayı yapılandırın")

        * İçinde **adı** metin yapılandırmanız için bir ad yazın (örneğin, **Microsoft Azure Federasyon çoklu oturum açma**).

        * Lütfen doldurulmuş kaldırın **kimlik sağlayıcısının SingleLogoutRequest** metin değeri.

        * Kopyalama **ServiceNow giriş sayfası** değeri, yapıştırın **oturum açma URL'si** metin kutusunda **ServiceNow temel SAML yapılandırma** bölümü Azure portalı.

            > [!NOTE]
            > ServiceNow örneği giriş sayfası, bir yapıdır, **ServiceNow Kiracı URL'si** ve **/navpage.do** (örneğin:`https://fabrikam.service-now.com/navpage.do`).

        * Kopyalama **varlık kimliği / veren** değeri, yapıştırın **tanımlayıcı** metin kutusunda **ServiceNow temel SAML yapılandırma** bölümü Azure portalı.

        * Lütfen emin olun **Nameıd ilke** ayarlanır `urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified` değeri.

        * **Gelişmiş**'e tıklayın. İçinde **kullanıcı alanı** metin kutusuna **e-posta** veya **user_name**bağlı olarak, hangi alanın ServiceNow Dağıtımınızdaki kullanıcıları benzersiz olarak tanımlanabilmesi için kullanılır.

            > [!NOTE]
            > Azure AD kullanıcı kimlik (kullanıcı asıl adı) veya e-posta adresi SAML belirtecindeki benzersiz tanımlayıcı olarak giderek yaymak için Azure AD'yi yapılandırabilirsiniz **ServiceNow > öznitelikleri > çoklu oturum açma** Azure portal'ın bölümü ve istenen alan için eşleme **NameIdentifier** özniteliği. Seçili öznitelik için (örneğin, kullanıcı asıl adı) Azure AD'de depolanan değer alanına girilen için (örneğin, user_name) ServiceNow içinde depolanan değeri eşleşmelidir

        * Tıklayarak **Test Bağlantısı** sayfanın üst sağ köşesindeki.

        * Tıklayıp sonra **Bağlantıyı Sına**, gerek duyduğunuz kimlik bilgilerini girmek için açılır pencere alır ve sonuçları sayfası aşağıda gösterilmektedir. **SSO oturum kapatma Test sonuçlarını** hata beklenmektedir Lütfen hatasını görmezden Gel ve tıklayın **etkinleştirme** düğmesi.

          ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/servicenowactivate.png "çoklu oturum açmayı yapılandırın")

### <a name="configure-azure-ad-sso-for-servicenow-express"></a>ServiceNow Express için Azure AD SSO'yu yapılandırma

1. İçinde [Azure portalında](https://portal.azure.com/), **ServiceNow** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **çoklu oturum açma yöntemini seçin** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<instance-name>.service-now.com/navpage.do`

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<instance-name>.service-now.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Öğreticinin ilerleyen bölümlerinde açıklanan tanımlayıcısı ve gerçek oturum açma URL'si ile bu değerleri güncelleştirmeniz gerekir. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Tek bir tıklamayla hizmeti yapılandırmak için ServiceNow diğer bir deyişle, Azure için AD otomatik olarak yapılandırma ServiceNow SAML tabanlı kimlik doğrulaması için sağlanır. Bu hizmet etkinleştirmek için Git **servicenow'ı kümesi** bölümünde **görüntülemek hakkında adım adım yönergeler** yapılandırma oturum açmak için.

    ![Çoklu oturum açmayı yapılandırın](./media/servicenow-tutorial/tutorial_servicenow_configure.png)

7. ServiceNow örnek adı, yönetici kullanıcı adı ve yönetici parola girin **yapılandırma oturum açma** tıklayın ve form **Şimdi Yapılandır**. Belirtilen yönetici kullanıcı adı olmalıdır Not **security_admin** çalışması için bu ServiceNow atanmış rol. Aksi takdirde, SAML kimlik sağlayıcısı Azure AD'yi kullanacak şekilde servicenow'ı el ile yapılandırmak için tıklayın **çoklu oturum açmayı el ile yapılandırmanız** ve kopyalama **oturum kapatma URL'si, Azure AD tanımlayıcısı ve oturum açma URL'si** gelen Hızlı Başvuru bölümü.

    ![Uygulama URL'sini Yapılandır](./media/servicenow-tutorial/configure.png "uygulama URL'sini yapılandırın")

### <a name="configure-servicenow-express-sso"></a>ServiceNow Express SSO yapılandırma

1. ServiceNow Express uygulamanıza yönetici olarak oturum açın.

2. Sol taraftaki gezinti bölmesinde **çoklu oturum açma**.

    ![Uygulama URL'sini Yapılandır](./media/servicenow-tutorial/ic7694980ex.png "uygulama URL'sini yapılandırın")

3. Üzerinde **çoklu oturum açma** iletişim kutusunda sağ üst köşedeki yapılandırma simgesine tıklayın ve aşağıdaki özellikleri ayarlayın:

    ![Uygulama URL'sini Yapılandır](./media/servicenow-tutorial/ic7694981ex.png "uygulama URL'sini yapılandırın")

    a. İki durumlu **Çoklu Sağlayıcı SSO etkinleştirme** sağ.

    b. İki durumlu **etkin hata ayıklama için birden çok sağlayıcı SSO tümleştirme günlüğü** sağ.

    c. İçinde **kullanıcı alanı tablo...**  metin kutusuna **user_name**.

4. Üzerinde **çoklu oturum açma** iletişim kutusunda, tıklayın **yeni sertifika Ekle**.

    ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/ic7694973ex.png "çoklu oturum açmayı yapılandırın")

5. Üzerinde **X.509 sertifikaları** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/ic7694975.png "çoklu oturum açmayı yapılandırın")

    a. İçinde **adı** metin yapılandırmanız için bir ad yazın (örneğin: **TestSAML2.0**).

    b. Seçin **etkin**.

    c. Olarak **biçimi**seçin **PEM**.

    d. Olarak **türü**seçin **Store sertifika güven**.

    e. Not Defteri'nde Azure portalından indirilen, Base64 olarak kodlanmış sertifika açın, içeriğini, panoya kopyalayın ve yapıştırın için **PEM sertifikası** metin.

    f. Tıklayın **güncelleştirme**

6. Üzerinde **çoklu oturum açma** iletişim kutusunda, tıklayın **ekleme yeni IDP**.

    ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/ic7694976ex.png "çoklu oturum açmayı yapılandırın")

7. Üzerinde **yeni kimlik sağlayıcısı Ekle** iletişim altında **kimlik sağlayıcısı yapılandırmak**, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/ic7694982ex.png "çoklu oturum açmayı yapılandırın")

    a. İçinde **adı** metin yapılandırmanız için bir ad yazın (örneğin: **SAML 2.0**).

    b. İçinde **kimlik sağlayıcısı URL'si** alan, değerini yapıştırın **kimlik sağlayıcı kimliği**, hangi Azure Portalı'ndan kopyaladığınız.

    c. İçinde **kimlik sağlayıcısının AuthnRequest** alan, değerini yapıştırın **kimlik doğrulaması istek URL**, hangi Azure Portalı'ndan kopyaladığınız.

    d. İçinde **kimlik sağlayıcısının SingleLogoutRequest** alan, değerini yapıştırın **oturum kapatma URL'si**, hangi Azure Portalı'ndan kopyaladığınız

    e. Olarak **kimlik sağlayıcısı sertifikası**, önceki adımda oluşturduğunuz sertifikayı seçin.

8. Tıklayın **Gelişmiş ayarlar**, altında **ek bir kimlik sağlayıcısı özellikleri**, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/ic7694983ex.png "çoklu oturum açmayı yapılandırın")

    a. İçinde **Protokolü bağlama IDP'nin SingleLogoutRequest için** metin kutusuna **urn: OASIS: adları: tc: SAML:2.0:bindings:HTTP-yeniden yönlendirme**.

    b. İçinde **Nameıd ilke** metin kutusuna **urn: OASIS: adları: tc: SAML:1.1:nameid-biçimi: belirtilmeyen**.

    c. İçinde **AuthnContextClassRef yöntemi**, türü `http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password`.

    d. Seçimini **bir AuthnContextClass oluşturma**.

9. Altında **ek hizmet sağlayıcısı özellikleri**, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/ic7694984ex.png "çoklu oturum açmayı yapılandırın")

    a. İçinde **ServiceNow giriş sayfası** metin, ServiceNow örneği giriş sayfası URL'sini yazın.

    > [!NOTE]
    > ServiceNow örneği giriş sayfası, bir yapıdır, **ServiceNow Kiracı URL'si** ve **/navpage.do** (örneğin: `https://fabrikam.service-now.com/navpage.do`).

    b. İçinde **varlık kimliği / veren** metin ServiceNow kiracınızın URL'sini yazın.

    c. İçinde **hedef kitle URİ'si** metin ServiceNow kiracınızın URL'sini yazın.

    d. İçinde **saat eğriltme** metin kutusuna **60**.

    e. İçinde **kullanıcı alanı** metin kutusuna **e-posta** veya **user_name**bağlı olarak, hangi alanın ServiceNow Dağıtımınızdaki kullanıcıları benzersiz olarak tanımlanabilmesi için kullanılır.

    > [!NOTE]
    > Azure AD kullanıcı kimlik (kullanıcı asıl adı) veya e-posta adresi SAML belirtecindeki benzersiz tanımlayıcı olarak giderek yaymak için Azure AD'yi yapılandırabilirsiniz **ServiceNow > öznitelikleri > çoklu oturum açma** Azure portal'ın bölümü ve istenen alan için eşleme **NameIdentifier** özniteliği. Seçili öznitelik için (örneğin, kullanıcı asıl adı) Azure AD'de depolanan değer alanına girilen için (örneğin, user_name) ServiceNow içinde depolanan değeri eşleşmelidir

    f. **Kaydet**’e tıklayın.

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

Bu bölümde, Azure çoklu oturum açma kullanmak için ServiceNow erişim vererek B.Simon tıklatmalarını sağlarsınız.

1. Azure portalında **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.
1. Uygulamalar listesinde **ServiceNow**.
1. Uygulamanın genel bakış sayfasında bulma **Yönet** seçin ve bölüm **kullanıcılar ve gruplar**.

   !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

1. Seçin **Kullanıcı Ekle**, ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Kullanıcı ekleme bağlantısı](common/add-assign-user.png)

1. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **B.Simon** kullanıcılar listesinden ardından **seçin** ekranın alt kısmındaki düğmesi.
1. SAML onaylama işlemi herhangi bir rolü değer de beklediğiniz varsa **rolü Seç** iletişim kutusunda, listeden bir kullanıcı için uygun rolü seçin ve ardından **seçin** ekranın alt kısmındaki düğmesi.
1. İçinde **atama Ekle** iletişim kutusunda, tıklayın **atama** düğmesi.

### <a name="create-servicenow-test-user"></a>Servicenow'ı test kullanıcısı oluşturma

Bu bölümün amacı, ServiceNow Britta Simon adlı bir kullanıcı oluşturmaktır. ServiceNow otomatik kullanıcı hazırlama, varsayılan olarak etkin olan destekler. Daha fazla ayrıntı bulabilirsiniz [burada](servicenow-provisioning-tutorial.md) otomatik kullanıcı sağlamayı yapılandırma.

> [!NOTE]
> Bir kullanıcı el ile oluşturmanız gerekiyorsa, iletişime geçmeniz [ServiceNow istemci destek ekibi](https://www.servicenow.com/support/contact-support.html)

### <a name="test-sso"></a>Test SSO

Erişim Paneli'nde ServiceNow kutucuğu seçtiğinizde, otomatik olarak SSO'yu ayarlama ServiceNow için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="test-sso-for-servicenow-classic-mobile"></a>Test SSO için ServiceNow Klasik (mobil)

1. Açık, **ServiceNow Klasik (mobil)** uygulama ve aşağıdaki adımları gerçekleştirin:

    a. Tıklayarak **ekleme** ekranın altında sembol.

    ![Oturum açma](./media/servicenow-tutorial/test03.png)

    b. ServiceNow örneği adınızı yazın ve tıklayın **devam**.

    ![Oturum açma](./media/servicenow-tutorial/test04.png)

    c. Üzerinde **oturum** ekranında, aşağıdaki adımları gerçekleştirin:

    ![Oturum açma](./media/servicenow-tutorial/test01.png)

    *  Tür **kullanıcıadı** gibi B.simon@contoso.com.

    *  Tıklayarak **kullanım dış oturum açma** ve Azure AD oturum açma sayfasına yönlendirilir.
    
    *  Kimlik bilgilerinizi girin ve varsa herhangi bir üçüncü taraf kimlik doğrulaması veya kullanıcı uygun şekilde yanıt vermesi gerekir etkinleştirilirse başka bir güvenlik özelliği ve uygulama **giriş sayfası** aşağıda gösterildiği gibi görüntülenir:

        ![Giriş sayfası](./media/servicenow-tutorial/test02.png)

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Kullanıcı sağlamayı yapılandırma](servicenow-provisioning-tutorial.md)