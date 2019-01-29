---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Clarizen | Microsoft Docs'
description: Azure Active Directory ve Clarizen arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: jeedes
ms.openlocfilehash: a7280111856f9cb2a20ebb7b52be04818c4b43c9
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55195282"
---
# <a name="tutorial-azure-active-directory-integration-with-clarizen"></a>Öğretici: Clarizen ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) tümleştirmeyi Clarizen ile öğrenin. Bu tümleştirme, aşağıdaki avantajları sunar:

- Clarizen erişimi olan Azure AD'de, denetleyebilirsiniz.
- Kullanıcılarınız için Clarizen (çoklu oturum açma) ile Azure AD hesaplarına otomatik olarak oturum açmanız etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda, Azure portalında yönetebilir.

Bu öğreticide senaryo iki ana görevden oluşur:

1. Galeriden Clarizen ekleyin.
1. Yapılandırma ve Azure AD çoklu oturum açmayı test edin.

Azure AD ile bir hizmet (SaaS) uygulamasını tümleştirme olarak yazılım hakkında daha fazla bilgi istiyorsanız bkz [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar
Azure AD Tümleştirmesi ile Clarizen yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Çoklu oturum açma için etkinleştirilen Clarizen abonelik

Bu öğreticideki adımları test etmek için aşağıdaki önerileri uygulayın:

- Azure AD çoklu oturum açma bir test ortamında test edin. Bu gerekli olmadıkça, üretim ortamında kullanmayın.
- Bir Azure AD test ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="add-clarizen-from-the-gallery"></a>Galeriden Clarizen Ekle
Azure AD'de Clarizen tümleştirmesini yapılandırmak için Clarizen Galeriden yönetilen SaaS uygulamaları listenize ekleyin.

1. İçinde [Azure portalında](https://portal.azure.com), sol bölmede **Azure Active Directory** simgesi.

    ![Azure Active Directory simgesi][1]

1. Tıklayın **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    !["Kurumsal uygulamalar" ve "Tüm uygulamalar"][2]

1. Tıklayın **Ekle** iletişim kutusunun üstündeki düğmesi.

    !["Ekle" düğmesi][3]

1. Arama kutusuna **Clarizen**.

    ![Arama kutusuna "Clarizen"](./media/clarizen-tutorial/tutorial_clarizen_000.png)

1. Sonuçlar bölmesinde seçin **Clarizen**ve ardından **Ekle** uygulama eklemek için.

    ![Sonuçlar bölmesinde Clarizen seçme](./media/clarizen-tutorial/tutorial_clarizen_0001.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme
Aşağıdaki bölümlerde, yapılandırın ve Azure AD çoklu oturum açmayı test kullanıcıya Britta Simon bağlı Clarizen sınayın.

Tek iş için oturum açma için Azure AD ne Clarizen karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Clarizen ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir. Değerini atayarak bu bağlantı ilişki kurmak **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** Clarizen içinde.

Yapılandırma ve Azure AD çoklu oturum açma Clarizen ile test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configure-azure-ad-single-sign-on)**  kullanıcılarınız bu özelliği kullanmak etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Clarizen test kullanıcısı oluşturma](#create-a-clarizen-test-user)**  bir karşılığı Britta simon'un Azure AD'ye gösterimini her için bağlı Clarizen sağlamak için.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  Britta Simon, Azure AD çoklu oturum açma kullanmak üzere etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın
Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Clarizen uygulamanızda çoklu oturum açmayı yapılandırın.

1. Azure portalında, üzerinde **Clarizen** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    !["Çoklu oturum açma" tıklayarak][4]

1. İçinde **çoklu oturum açma** iletişim kutusu için **modu**seçin **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    !["SAML tabanlı oturum açma" seçme](./media/clarizen-tutorial/tutorial_clarizen_01.png)

1. İçinde **Clarizen etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Kutuları tanımlayıcı ve yanıt URL'si](./media/clarizen-tutorial/tutorial_clarizen_02.png)

    a. İçinde **tanımlayıcı** değeri olarak yazın: **Clarizen**

    b. İçinde **yanıt URL'si** şu biçimi kullanarak bir URL yazın: **https://<company name>.clarizen.com/Clarizen/Pages/Integrations/SAML/SamlResponse.aspx**

    > [!NOTE]
    > Bunlar gerçek değerlerin değildir. Gerçek tanımlayıcısı kullanın ve yanıt URL'si gerekir. Burada, benzersiz bir dize değerini tanımlayıcı olarak kullanmanızı öneririz. Gerçek değerlere ulaşmak için ilgili kişi [Clarizen Destek ekibine](https://success.clarizen.com/hc/en-us/requests/new).

1. Üzerinde **SAML imzalama sertifikası** bölümünde **yeni sertifika oluştur**.

    !["Yeni Sertifika Oluştur"'ı tıklatarak](./media/clarizen-tutorial/tutorial_clarizen_03.png)    

1. İçinde **yeni sertifika oluştur** iletişim kutusuna Takvim simgesine ve sona erme tarihini seçin. Daha sonra **Kaydet**'e tıklayın.

    ![Seçme ve sona erme tarihini kaydediliyor](./media/clarizen-tutorial/tutorial_general_300.png)

1. İçinde **SAML imzalama sertifikası** bölümünden **yeni sertifikayı etkin hale getirin**ve ardından **Kaydet**.

    ![Yeni sertifikayı etkin hale getirme için onay kutusunu işaretleyerek](./media/clarizen-tutorial/tutorial_clarizen_04.png)

1. İçinde **geçiş sertifikası** iletişim kutusu, tıklayın **Tamam**.

    ![Sertifikayı etkinleştirmek istediğinizi onaylamak için "Tamam" ı tıklayarak](./media/clarizen-tutorial/tutorial_general_400.png)

1. İçinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Tıklayıp "yüklemeyi başlatmak için sertifika (Base64)"](./media/clarizen-tutorial/tutorial_clarizen_05.png)

1. İçinde **Clarizen yapılandırma** bölümünde **yapılandırma Clarizen** açmak için **yapılandırma oturum açma** penceresi.

    !["Clarizen Yapılandır" ı tıklatarak](./media/clarizen-tutorial/tutorial_clarizen_06.png)

    ![""Oturum açma Yapılandır penceresinde, dosyaları ve URL'leri dahil](./media/clarizen-tutorial/tutorial_clarizen_07.png)

1. Farklı bir web tarayıcı penceresinde Clarizen şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Kullanıcı adınıza tıklayın ve ardından **ayarları**.

    ![Kullanıcı adınızın "Ayarlar" a tıklandığında](./media/clarizen-tutorial/tutorial_clarizen_001.png "ayarları")

1. Tıklayın **genel ayarları** sekmesi. Ardından yanındaki **şirket dışı kimlik doğrulaması**, tıklayın **Düzenle**.

    !["Genel ayarları" sekmesine](./media/clarizen-tutorial/tutorial_clarizen_002.png "genel ayarlar")

1. İçinde **şirket dışı kimlik doğrulaması** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![İletişim kutusu "Federe kimlik doğrulaması"](./media/clarizen-tutorial/tutorial_clarizen_003.png "şirket dışı kimlik doğrulaması")

    a. Seçin **etkinleştir federe kimlik doğrulaması**.

    b. Tıklayın **karşıya** indirilen sertifikanızı karşıya yüklemek için.

    c. İçinde **oturum açma URL'si** kutusunda, değeri girin **SAML çoklu oturum açma hizmeti URL'si** penceresinden Azure AD uygulama yapılandırması.

    d. İçinde **oturum kapatma URL'si** kutusunda, değeri girin **oturum kapatma URL'si** penceresinden Azure AD uygulama yapılandırması.

    e. Seçin **POST kullanın**.

    f. **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Azure portalında Britta Simon adlı bir test kullanıcısı oluşturun.

![Azure AD test kullanıcı adı ve e-posta adresi][100]

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory simgesi](./media/clarizen-tutorial/create_aaduser_01.png)

1. Tıklayın **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar** kullanıcılar listesini görüntüleyin.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar"](./media/clarizen-tutorial/create_aaduser_02.png)

1. İletişim kutusunun en üstünde tıklayın **Ekle** açmak için **kullanıcı** iletişim kutusu.

    !["Ekle" düğmesi](./media/clarizen-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    !["Kullanıcı" iletişim kutusu adı, e-posta adresi ve parola ile doldurulur](./media/clarizen-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon hesabın e-posta adresini yazın.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.

### <a name="create-a-clarizen-test-user"></a>Clarizen test kullanıcısı oluşturma

Bu bölümün amacı Clarizen Britta Simon adlı bir kullanıcı oluşturmaktır.

**Kullanıcı el ile oluşturmanız gerekiyorsa, lütfen aşağıdaki adımları uygulayın:**

Azure AD kullanıcıları için Clarizen oturum açmak etkinleştirmek için kullanıcı hesapları hazırlamanız gerekir. Clarizen söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

1. Clarizen şirketinizin sitesi için bir yönetici olarak oturum açın.

2. Tıklayın **kişiler**.

    !["Kişiler" tıklayarak](./media/clarizen-tutorial/create_aaduser_001.png "kişiler")

3. Tıklayın **kullanıcı davet**.

    !["Kullanıcı davet et" düğmesini](./media/clarizen-tutorial/create_aaduser_002.png "kullanıcılar davet edin")

1. İçinde **davet kişiler** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![İletişim kutusu "Kişileri davet edin"](./media/clarizen-tutorial/create_aaduser_003.png "kişileri davet edin")

    a. İçinde **e-posta** Britta Simon hesabın e-posta adresini yazın.

    b. Tıklayın **davet**.

    > [!NOTE]
    > Azure Active Directory hesap sahibinin e-posta alır ve bunu etkinleştirilmeden önce hesabını onaylamak için bağlantıyı izleyin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın
Britta Simon, Azure çoklu oturum açma için Clarizen erişim vererek kullanmak etkinleştirin.

![Atanan test kullanıcısı][200]

1. Azure uygulamaları görüntüleyin, portal Aç directory görünüme Gözat'a tıklayın **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.

    !["Kurumsal uygulamalar" ve "Tüm uygulamalar"][201]

1. Uygulamalar listesinde **Clarizen**.

    ![Clarizen listeden](./media/clarizen-tutorial/tutorial_clarizen_50.png)

1. Sol bölmede **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" tıklayarak][202]

1. **Ekle** düğmesine tıklayın. Ardından **atama Ekle** iletişim kutusunda **kullanıcılar ve gruplar**.

    !["Ekle" düğmesi ve "Atama Ekle" iletişim kutusu][203]

1. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcılar listesinde.

1. İçinde **kullanıcılar ve gruplar** iletişim kutusu, tıklayın **seçin** düğmesi.

1. İçinde **atama Ekle** iletişim kutusu, tıklayın **atama** düğmesi.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi
Azure AD çoklu oturum açma yapılandırmanızı erişim panelini kullanarak test edin.

Erişim paneli Clarizen kutucuğa tıkladığınızda, size otomatik olarak Clarizen uygulamanıza oturum açmanız.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/clarizen-tutorial/tutorial_general_01.png
[2]: ./media/clarizen-tutorial/tutorial_general_02.png
[3]: ./media/clarizen-tutorial/tutorial_general_03.png
[4]: ./media/clarizen-tutorial/tutorial_general_04.png

[100]: ./media/clarizen-tutorial/tutorial_general_100.png

[200]: ./media/clarizen-tutorial/tutorial_general_200.png
[201]: ./media/clarizen-tutorial/tutorial_general_201.png
[202]: ./media/clarizen-tutorial/tutorial_general_202.png
[203]: ./media/clarizen-tutorial/tutorial_general_203.png
