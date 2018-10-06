---
title: 'Öğretici: Azure Active Directory ServiceNow ile tümleştirme | Microsoft Docs'
description: Azure Active Directory hem de Servicenow'a arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: a5a1a264-7497-47e7-b129-a1b5b1ebff5b
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/04/2018
ms.author: jeedes
ms.openlocfilehash: 470805b2bb77e367887767b95e0f1e04d79c8f9d
ms.sourcegitcommit: 26cc9a1feb03a00d92da6f022d34940192ef2c42
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/06/2018
ms.locfileid: "48830744"
---
# <a name="tutorial-azure-active-directory-integration-with-servicenow"></a>Öğretici: Azure Active Directory ServiceNow ile tümleştirme

Bu öğreticide, ServiceNow, Azure Active Directory (Azure AD) ile tümleştirme konusunda bilgi edinin.

ServiceNow'ı Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- ServiceNow erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan ServiceNow'ın (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi ServiceNow ile yapılandırma için aşağıdakiler gerekir:

- Azure AD aboneliği
- Servicenow'ı, bir örnek veya Kiracı, ServiceNow, Calgary sürümü veya üzeri
- Servicenow'ı hızlı, ServiceNow Express, Helsinki sürümü örneğini veya üzeri
- ServiceNow kiracısına sahip olmanız gerekir [birden çok sağlayıcı tek oturum üzerinde eklentisi](http://wiki.servicenow.com/index.php?title=Multiple_Provider_Single_Sign-On#gsc.tab=0) etkin. Bunu aşağıdaki yöntemlerle yapabilirsiniz [hizmet isteği göndererek](https://hi.service-now.com).
- Otomatik yapılandırma için için ServiceNow Çoklu Sağlayıcı eklentisini etkinleştirin.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin.
Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. ServiceNow galeri ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-servicenow-from-the-gallery"></a>ServiceNow galeri ekleme

Servicenow'ı Azure AD'ye tümleştirmesini yapılandırmak için ServiceNow Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**ServiceNow Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **ServiceNow**seçin **ServiceNow** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde ServiceNow](./media/servicenow-tutorial/tutorial_servicenow_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve "Britta Simon" adlı bir test kullanıcı tabanlı servicenow'ı Azure AD çoklu oturum açmayı sınayın.

Tek çalışmak için oturum açma için Azure AD ne ServiceNow karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili ServiceNow kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

ServiceNow'ın değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma ServiceNow ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açma için servicenow'ı yapılandırma](#configure-azure-ad-single-sign-on-for-servicenow)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Azure AD çoklu oturum açma için ServiceNow Express yapılandırma](#configure-azure-ad-single-sign-on-for-servicenow-express)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Servicenow'ı test kullanıcısı oluşturma](#create-a-servicenow-test-user)**  - kullanıcı Azure AD gösterimini bağlı ServiceNow Britta simon'un bir karşılığı vardır.
5. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on-for-servicenow"></a>Azure AD çoklu oturum açma için servicenow'ı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma ServiceNow uygulamanızı yapılandırın.

**Azure AD çoklu oturum açma ServiceNow ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **ServiceNow** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Tıklayın **değişiklik çoklu oturum açma modunu** seçmek için ekranın en üstünde **SAML** modu.

    ![Çoklu oturum açmayı yapılandırın](./media/servicenow-tutorial/tutorial_general_300.png)

3. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunu tıklatın **seçin** için **SAML** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açmayı yapılandırın](./media/servicenow-tutorial/tutorial_general_301.png)

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için düğmeyi **temel SAML yapılandırma** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/servicenow-tutorial/tutorial_general_302.png)

5. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![ServiceNow etki alanı ve URL'ler tek oturum açma bilgileri](./media/servicenow-tutorial/tutorial_servicenow_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<instance-name>.service-now.com/navpage.do`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<instance-name>.service-now.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve öğreticinin sonraki bölümlerinde açıklanan tanımlayıcısı güncelleştirmeniz gerekir.

6. Üzerinde **SAML imzalama sertifikası** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Sertifika indirme bağlantısı](./media/servicenow-tutorial/tutorial_servicenow_certificate.png)

    a. Kopyalamak için Kopyala düğmesine **uygulama Federasyon meta verileri URL'sini** bu uygulama Federasyon meta veri URL'si, öğreticinin ilerleyen bölümlerinde kullanılacak şekilde kopyalayıp Not Defteri'ne yapıştırın.

    b. Tıklayarak **indirme** indirmek için **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

7. ServiceNow uygulamanızı yönetici olarak oturum açın.

8. Etkinleştirme **tümleştirmesi - birden çok sağlayıcı tek oturum açma yükleyicisini** sonraki adımları izleyerek Eklentisi:

    a. Sol taraftaki gezinti bölmesinde, arama **sistem tanımı** arama bölümüne ve ardından **eklentileri**.

    ![Eklenti etkinleştirme](./media/servicenow-tutorial/tutorial_servicenow_03.png "eklentisini etkinleştirme")

     b. Arama **tümleştirmesi - birden çok sağlayıcı tek oturum açma yükleyicisini**.

     ![Eklenti etkinleştirme](./media/servicenow-tutorial/tutorial_servicenow_04.png "eklentisini etkinleştirme")

    c. Eklentiyi seçin. Sağ tıklatın ve seçin **etkinleştir/yükseltme**.

     ![Eklenti etkinleştirme](./media/servicenow-tutorial/tutorial_activate.png "eklentisini etkinleştirme")

    d. Tıklayın **etkinleştirme** düğmesi.

     ![Eklenti etkinleştirme](./media/servicenow-tutorial/tutorial_activate1.png "eklentisini etkinleştirme")

9. Sol taraftaki gezinti bölmesinde, arama **Çoklu Sağlayıcı SSO** arama bölümüne ve ardından **özellikleri**.

    ![Uygulama URL'sini Yapılandır](./media/servicenow-tutorial/tutorial_servicenow_06.png "uygulama URL'sini yapılandırın")

10. Üzerinde **Çoklu Sağlayıcı SSO özelliklerini** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Uygulama URL'sini Yapılandır](./media/servicenow-tutorial/ic7694981.png "uygulama URL'sini yapılandırın")

    * Olarak **Çoklu Sağlayıcı SSO etkinleştirme**seçin **Evet**.
  
    * Olarak **kullanıcıların ve kullanıcı tabloya tüm kimlik sağlayıcılardan gelen otomatik içeri aktarma**seçin **Evet**.

    * Olarak **etkin hata ayıklama için birden çok sağlayıcı SSO tümleştirme günlüğü**seçin **Evet**.

    * İçinde **kullanıcı alanı tablo...**  metin kutusuna **user_name**.
  
    * **Kaydet**’e tıklayın.

11. Burada iki yolla **ServiceNow** yapılandırılabilir - otomatik ve el ile.

12. Yapılandırma **ServiceNow** izleyin otomatik olarak aşağıdaki adımları:

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

        * Kopyalama **ServiceNow giriş sayfası** değeri, yapıştırın **oturum açma URL'si** metin kutusunda **ServiceNow etki alanı ve URL'ler** bölümü Azure portalı.

            > [!NOTE]
            > ServiceNow örneği giriş sayfası, bir yapıdır, **ServieNow Kiracı URL'si** ve **/navpage.do** (örneğin:`https://fabrikam.service-now.com/navpage.do`).

        * Kopyalama **varlık kimliği / veren** değeri, yapıştırın **tanımlayıcı** metin kutusunda **ServiceNow etki alanı ve URL'ler** bölümü Azure portalı.

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
  
13. Yapılandırma **ServiceNow** el ile izleyerek aşağıdaki adımları:

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

        * Kopyalama **ServiceNow giriş sayfası** değeri, yapıştırın **oturum açma URL'si** metin kutusunda **ServiceNow etki alanı ve URL'ler** bölümü Azure portalı.

            > [!NOTE]
            > ServiceNow örneği giriş sayfası, bir yapıdır, **ServiceNow Kiracı URL'si** ve **/navpage.do** (örneğin:`https://fabrikam.service-now.com/navpage.do`).

        * Kopyalama **varlık kimliği / veren** değeri, yapıştırın **tanımlayıcı** metin kutusunda **ServiceNow etki alanı ve URL'ler** bölümü Azure portalı.

        * Lütfen emin olun **Nameıd ilke** ayarlanır `urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified` değeri.

        * Tıklayın **Gelişmiş**. İçinde **kullanıcı alanı** metin kutusuna **e-posta** veya **user_name**bağlı olarak, hangi alanın ServiceNow Dağıtımınızdaki kullanıcıları benzersiz olarak tanımlanabilmesi için kullanılır.

            > [!NOTE]
            > Azure AD kullanıcı kimlik (kullanıcı asıl adı) veya e-posta adresi SAML belirtecindeki benzersiz tanımlayıcı olarak giderek yaymak için Azure AD'yi yapılandırabilirsiniz **ServiceNow > öznitelikleri > çoklu oturum açma** Azure portal'ın bölümü ve istenen alan için eşleme **NameIdentifier** özniteliği. Seçili öznitelik için (örneğin, kullanıcı asıl adı) Azure AD'de depolanan değer alanına girilen için (örneğin, user_name) ServiceNow içinde depolanan değeri eşleşmelidir

        * Tıklayarak **Test Bağlantısı** sayfanın üst sağ köşesindeki.

        * Tıklayıp sonra **Bağlantıyı Sına**, gerek duyduğunuz kimlik bilgilerini girmek için açılır pencere alır ve sonuçları sayfası aşağıda gösterilmektedir. **SSO oturum kapatma Test sonuçlarını** hata beklenmektedir Lütfen hatasını görmezden Gel ve tıklayın **etkinleştirme** düğmesi.

          ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/servicenowactivate.png "çoklu oturum açmayı yapılandırın")

### <a name="configure-azure-ad-single-sign-on-for-servicenow-express"></a>Azure AD çoklu oturum açma için ServiceNow Express yapılandırın.

1. Azure portalında, üzerinde **ServiceNow** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Tıklayın **değişiklik çoklu oturum açma modunu** seçmek için ekranın en üstünde **SAML** modu.

    ![Çoklu oturum açmayı yapılandırın](./media/servicenow-tutorial/tutorial_general_300.png)

3. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunu tıklatın **seçin** için **SAML** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açmayı yapılandırın](./media/servicenow-tutorial/tutorial_general_301.png)

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için düğmeyi **temel SAML yapılandırma** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/servicenow-tutorial/tutorial_general_302.png)

5. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/servicenow-tutorial/tutorial_servicenow_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak değeri yazın: `https://<instance-name>.service-now.com/navpage.do`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<instance-name>.service-now.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri tanımlayıcısı ve gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [ServiceNow istemci Destek ekibine](https://www.servicenow.com/support/contact-support.html) bu değerleri almak için.

6. Üzerinde **SAML imzalama sertifikası** bölümü tıklatın üzerinde **indirme** indirmek için **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/servicenow-tutorial/tutorial_servicenow_certificates.png)

7. Tek bir tıklamayla hizmeti yapılandırmak için ServiceNow diğer bir deyişle, Azure için AD otomatik olarak yapılandırma ServiceNow SAML tabanlı kimlik doğrulaması için sağlanır. Bu hizmet etkinleştirmek için Git **servicenow'ı kümesi** bölümünde **görüntülemek hakkında adım adım yönergeler** yapılandırma oturum açmak için.

    ![Çoklu oturum açmayı yapılandırın](./media/servicenow-tutorial/tutorial_servicenow_configure.png)

8. ServiceNow örnek adı, yönetici kullanıcı adı ve yönetici parola girin **yapılandırma oturum açma** tıklayın ve form **Şimdi Yapılandır**. Belirtilen yönetici kullanıcı adı olmalıdır Not **security_admin** çalışması için bu ServiceNow atanmış rol. Aksi takdirde, SAML kimlik sağlayıcısı Azure AD'yi kullanacak şekilde servicenow'ı el ile yapılandırmak için tıklayın **çoklu oturum açmayı el ile yapılandırmanız** ve kopyalama **SAML çoklu oturum açma hizmeti URL'sioturumkapatmaURL'siveSAMLvarlıkkimliği** hızlı başvuru bölümünden.

    ![Uygulama URL'sini Yapılandır](./media/servicenow-tutorial/configure.png "uygulama URL'sini yapılandırın")

9. ServiceNow Express uygulamanıza yönetici olarak oturum açın.

10. Sol taraftaki gezinti bölmesinde **çoklu oturum açma**.

    ![Uygulama URL'sini Yapılandır](./media/servicenow-tutorial/ic7694980ex.png "uygulama URL'sini yapılandırın")

11. Üzerinde **çoklu oturum açma** iletişim kutusunda sağ üst köşedeki yapılandırma simgesine tıklayın ve aşağıdaki özellikleri ayarlayın:

    ![Uygulama URL'sini Yapılandır](./media/servicenow-tutorial/ic7694981ex.png "uygulama URL'sini yapılandırın")

    a. İki durumlu **Çoklu Sağlayıcı SSO etkinleştirme** sağ.

    b. İki durumlu **etkin hata ayıklama için birden çok sağlayıcı SSO tümleştirme günlüğü** sağ.

    c. İçinde **kullanıcı alanı tablo...**  metin kutusuna **user_name**.

12. Üzerinde **çoklu oturum açma** iletişim kutusunda, tıklayın **yeni sertifika Ekle**.

    ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/ic7694973ex.png "çoklu oturum açmayı yapılandırın")

13. Üzerinde **X.509 sertifikaları** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/ic7694975.png "çoklu oturum açmayı yapılandırın")

    a. İçinde **adı** metin yapılandırmanız için bir ad yazın (örneğin: **TestSAML2.0**).

    b. Seçin **etkin**.

    c. Olarak **biçimi**seçin **PEM**.

    d. Olarak **türü**seçin **Store sertifika güven**.

    e. Not Defteri'nde Azure portalından indirilen, Base64 olarak kodlanmış sertifika açın, içeriğini, panoya kopyalayın ve yapıştırın için **PEM sertifikası** metin.

    f. Tıklayın **güncelleştirme**

14. Üzerinde **çoklu oturum açma** iletişim kutusunda, tıklayın **ekleme yeni IDP**.

    ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/ic7694976ex.png "çoklu oturum açmayı yapılandırın")

15. Üzerinde **yeni kimlik sağlayıcısı Ekle** iletişim altında **kimlik sağlayıcısı yapılandırmak**, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/ic7694982ex.png "çoklu oturum açmayı yapılandırın")

    a. İçinde **adı** metin yapılandırmanız için bir ad yazın (örneğin: **SAML 2.0**).

    b. İçinde **kimlik sağlayıcısı URL'si** alan, değerini yapıştırın **kimlik sağlayıcı kimliği**, hangi Azure Portalı'ndan kopyaladığınız.

    c. İçinde **kimlik sağlayıcısının AuthnRequest** alan, değerini yapıştırın **kimlik doğrulaması istek URL**, hangi Azure Portalı'ndan kopyaladığınız.

    d. İçinde **kimlik sağlayıcısının SingleLogoutRequest** alan, değerini yapıştırın **çoklu oturum kapatma hizmeti URL'si**, hangi Azure Portalı'ndan kopyaladığınız

    e. Olarak **kimlik sağlayıcısı sertifikası**, önceki adımda oluşturduğunuz sertifikayı seçin.

16. Tıklayın **Gelişmiş ayarlar**, altında **ek bir kimlik sağlayıcısı özellikleri**, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/ic7694983ex.png "çoklu oturum açmayı yapılandırın")

    a. İçinde **Protokolü bağlama IDP'nin SingleLogoutRequest için** metin kutusuna **urn: OASIS: adları: tc: SAML:2.0:bindings:HTTP-yeniden yönlendirme**.

    b. İçinde **Nameıd ilke** metin kutusuna **urn: OASIS: adları: tc: SAML:1.1:nameid-biçimi: belirtilmeyen**.

    c. İçinde **AuthnContextClassRef yöntemi**, türü `http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password`.

    d. Seçimini **bir AuthnContextClass oluşturma**.

17. Altında **ek hizmet sağlayıcısı özellikleri**, aşağıdaki adımları gerçekleştirin:

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

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    ![Azure AD kullanıcısı oluşturun][100]

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/servicenow-tutorial/create_aaduser_01.png) 

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/servicenow-tutorial/create_aaduser_02.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü **brittasimon@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **özellikleri**seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’u seçin.

### <a name="create-a-servicenow-test-user"></a>Servicenow'ı test kullanıcısı oluşturma

Bu bölümün amacı, ServiceNow Britta Simon adlı bir kullanıcı oluşturmaktır. ServiceNow otomatik kullanıcı hazırlama, varsayılan olarak etkin olan destekler. Daha fazla ayrıntı bulabilirsiniz [burada](servicenow-provisioning-tutorial.md) otomatik kullanıcı sağlamayı yapılandırma.

> [!NOTE]
> Bir kullanıcı el ile oluşturmanız gerekiyorsa, iletişime geçmeniz [ServiceNow istemci destek ekibi](https://www.servicenow.com/support/contact-support.html)

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için ServiceNow erişim vererek Britta Simon etkinleştirin.

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

2. Uygulamalar listesinde **ServiceNow**.

    ![Uygulamalar listesinde ServiceNow bağlantısı](./media/servicenow-tutorial/tutorial_servicenow_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. İçinde **atama Ekle** iletişim kutusunda **atama** düğmesi.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde ServiceNow kutucuğa tıkladığınızda, otomatik olarak ServiceNow uygulamanızı açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
* [Kullanıcı sağlamayı yapılandırma](servicenow-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/servicenow-tutorial/tutorial_general_01.png
[2]: ./media/servicenow-tutorial/tutorial_general_02.png
[3]: ./media/servicenow-tutorial/tutorial_general_03.png
[4]: ./media/servicenow-tutorial/tutorial_general_04.png

[100]: ./media/servicenow-tutorial/tutorial_general_100.png

[200]: ./media/servicenow-tutorial/tutorial_general_200.png
[201]: ./media/servicenow-tutorial/tutorial_general_201.png
[202]: ./media/servicenow-tutorial/tutorial_general_202.png
[203]: ./media/servicenow-tutorial/tutorial_general_203.png
