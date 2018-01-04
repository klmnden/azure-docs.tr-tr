---
title: "Öğretici: Azure Active Directory Tümleştirme ile Clarizen | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile Clarizen arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2017
ms.author: jeedes
ms.openlocfilehash: 2925f0a9f582d0dfeca9832ca032b0d847f23f6b
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-clarizen"></a>Öğretici: Azure Active Directory Tümleştirme Clarizen ile

Bu öğreticide, Azure Active Directory (Azure AD) tümleştirme ile Clarizen öğrenin. Bu tümleştirme, aşağıdaki avantajları sunar:

- Clarizen erişebilen Azure AD'de, denetleyebilirsiniz.
- Kullanıcılarınız için Clarizen (çoklu oturum açma) ile Azure AD hesaplarına otomatik olarak oturum açmanız etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda, Azure portalında yönetebilir.

Bu öğretici senaryoda iki ana görevden oluşur:

1. Clarizen Galeriden ekleyin.
2. Yapılandırma ve Azure AD çoklu oturum açmayı sınayın.

Azure AD ile hizmet (SaaS) uygulaması tümleştirme olarak yazılım hakkında daha fazla bilgi istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar
Azure AD tümleştirme Clarizen ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Çoklu oturum açma için etkinleştirilen Clarizen abonelik

Bu öğreticide adımları test etmek için aşağıdaki önerileri uygulayın:

- Azure AD çoklu oturum açma bir test ortamında test edin. Bu gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD test ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="add-clarizen-from-the-gallery"></a>Galeriden Clarizen Ekle
Azure AD Clarizen tümleştirilmesi yapılandırmak için Clarizen Galeriden yönetilen SaaS uygulamaları listenize ekleyin.

1. İçinde [Azure portal](https://portal.azure.com), sol bölmede **Azure Active Directory** simgesi.

    ![Azure Active Directory simgesi][1]

2. Tıklatın **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    !["Kurumsal uygulamalar" ve "Tüm uygulamalar" ı tıklatarak][2]

3. Tıklatın **Ekle** iletişim kutusunun üstündeki düğmesi.

    !["Ekle" düğmesi][3]

4. Arama kutusuna **Clarizen**.

    ![Arama kutusuna "Clarizen" yazın](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_000.png)

5. Sonuçlar bölmesinde seçin **Clarizen**ve ardından **Ekle** uygulama eklemek için.

    ![Sonuçlar bölmesinde Clarizen seçme](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_0001.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme
Aşağıdaki bölümlerde, yapılandırma ve test kullanıcı Britta Simon tabanlı Clarizen ile Azure AD çoklu oturum açma sınayın.

Tekli çalışmaya oturum için Azure AD Clarizen karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Clarizen ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir. Değerini atayarak bu bağlantı ilişkisini kurmak **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** Clarizen içinde.

Yapılandırma ve Azure AD çoklu oturum açma Clarizen ile test etmek için aşağıdaki yapı taşları tamamlayın:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Clarizen test kullanıcısı oluşturma](#create-a-clarizen-test-user)**  Britta Simon, karşılık gelen her, Azure AD gösterimine bağlı Clarizen sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  Britta Azure AD çoklu oturum açma kullanmak Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın
Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Clarizen uygulamanızda yapılandırın.

1. Azure portalında üzerinde **Clarizen** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    !["Çoklu oturum açma" a tıklayarak][4]

2. İçinde **çoklu oturum açma** iletişim kutusu için **modu**seçin **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    !["SAML tabanlı oturum açma" seçme](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_01.png)

3. İçinde **Clarizen etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Tanımlayıcı ve yanıt URL'si için kutuları](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_02.png)

    a. İçinde **tanımlayıcısı** değeri olarak yazın: **Clarizen**

    b. İçinde **yanıt URL'si** kutusunda, bir URL şu biçimi kullanarak girin: **https://<company name>.clarizen.com/Clarizen/Pages/Integrations/SAML/SamlResponse.aspx**

    > [!NOTE]
    > Bunlar gerçek değerleri değildir. Gerçek tanımlayıcı kullanın ve URL yanıt gerekmez. Burada tanımlayıcı olarak benzersiz bir dize değerini kullanmanızı öneririz. Gerçek değerleri almak için başvurun [Clarizen destek ekibi](https://success.clarizen.com/hc/en-us/requests/new).

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **yeni sertifika oluştur**.

    !["Yeni Sertifika Oluştur" a tıklayarak](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_03.png)  

5. İçinde **yeni sertifika oluştur** iletişim kutusunda, takvim simgesini tıklatın ve bir süre sonu tarihi seçin. Daha sonra **Kaydet**'e tıklayın.

    ![Seçme ve sona erme tarihi kaydetme](./media/active-directory-saas-clarizen-tutorial/tutorial_general_300.png)

6. İçinde **SAML imzalama sertifikası** bölümünde, select **yeni sertifika etkin hale getirin**ve ardından **kaydetmek**.

    ![Yeni sertifika etkin hale getirme için onay kutusunu işaretleyerek](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_04.png)

7. İçinde **geçiş sertifikası** iletişim kutusu, tıklatın **Tamam**.

    ![Sertifikayı etkinleştirmek istediğinizi onaylamak için "Tamam" a tıklayarak](./media/active-directory-saas-clarizen-tutorial/tutorial_general_400.png)

8. İçinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    !["Yüklemeyi başlatmak için sertifika (Base64)" a tıklayarak](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_05.png)

9. İçinde **Clarizen yapılandırma** 'yi tıklatın **yapılandırma Clarizen** açmak için **yapılandırma oturum açma** penceresi.

    !["Clarizen Yapılandır" a tıklayarak](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_06.png)

    !["Oturum açma yapılandırma" penceresinde, dosyaları ve URL'leri dahil](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_07.png)

10. Farklı web tarayıcısı penceresinde Clarizen şirket sitenize yönetici olarak oturum açın.

11. Kullanıcı adınıza tıklayın ve ardından **ayarları**.

    ![Kullanıcı adınızı "Ayarlar"'ı tıklatarak](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_001.png "ayarları")

12. Tıklatın **genel ayarları** sekmesi. Ardından yanına **şirket dışı kimlik doğrulaması**, tıklatın **Düzenle**.

    !["Genel ayarları" sekmesini](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_002.png "genel ayarları")

13. İçinde **şirket dışı kimlik doğrulaması** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    !["Federe kimlik doğrulaması" iletişim kutusu](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_003.png "şirket dışı kimlik doğrulaması")

    a. Seçin **etkinleştir federe kimlik doğrulaması**.

    b. Tıklatın **karşıya** indirilen sertifikanızı karşıya yüklemek için.

    c. İçinde **oturum açma URL'si** kutusunda, değeri girin **SAML çoklu oturum açma hizmet URL'si** Azure AD uygulama yapılandırma penceresinden.

    d. İçinde **Sign-out URL** kutusunda, değeri girin **Sign-Out URL** Azure AD uygulama yapılandırma penceresinden.

    e. Seçin **POST kullanmak**.

    f. **Kaydet** düğmesine tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Azure portalında Britta Simon adlı bir test kullanıcısı oluşturun.

![Azure AD test kullanıcı adı ve e-posta adresi][100]

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** simgesi.

    ![Azure Active Directory simgesi](./media/active-directory-saas-clarizen-tutorial/create_aaduser_01.png)

2. Tıklatın **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar** kullanıcıların listesini görüntülemek için.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar"](./media/active-directory-saas-clarizen-tutorial/create_aaduser_02.png)

3. İletişim kutusunun üstündeki **Ekle** açmak için **kullanıcı** iletişim kutusu.

    !["Ekle" düğmesi](./media/active-directory-saas-clarizen-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    !["Kullanıcı" iletişim kutusu adı, e-posta adresi ve parola ile doldurulur](./media/active-directory-saas-clarizen-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon hesabın e-posta adresini yazın.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.

### <a name="create-a-clarizen-test-user"></a>Clarizen test kullanıcısı oluşturma
Azure AD için Clarizen oturum açmalarını etkinleştirmek için kullanıcı hesapları hazırlamanız gerekir. Clarizen söz konusu olduğunda, sağlama bir el ile bir görevdir.

1. Clarizen şirket sitenize yönetici olarak oturum açın.

2. Tıklatın **kişiler**.

    !["Kişiler" tıklatarak](./media/active-directory-saas-clarizen-tutorial/create_aaduser_001.png "kişiler")

3. Tıklatın **davet kullanıcı**.

    !["Kullanıcı davet" düğmesine](./media/active-directory-saas-clarizen-tutorial/create_aaduser_002.png "kullanıcıları davet et")

4. İçinde **kişileri davet** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    !["Kişileri davet edin" iletişim kutusu](./media/active-directory-saas-clarizen-tutorial/create_aaduser_003.png "kişileri davet edin")

    a. İçinde **e-posta** Britta Simon hesabın e-posta adresini yazın.

    b. Tıklatın **davet**.

    > [!NOTE]
    > Azure Active Directory hesap sahibi bir e-posta alır ve onu etkinleştirilmeden önce kendi hesabı onaylamak için bağlantıyı izleyin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın
Britta Clarizen için kendi erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Atanan test kullanıcısı][200]

1. Azure portal, Aç uygulamaları görüntülemek, dizin görüntülemek için Gözat'ı tıklatın **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.

    !["Kurumsal uygulamalar" ve "Tüm uygulamalar" ı tıklatarak][201]

2. Uygulamalar listesinde **Clarizen**.

    ![Listede Clarizen seçme](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_50.png)

3. Sol bölmede **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" a tıklayarak][202]

4. Tıklatın **Ekle** düğmesi. Ardından **eklemek atama** iletişim kutusunda **kullanıcılar ve gruplar**.

    !["Ekle" düğmesi ve "Atama Ekle" iletişim kutusu][203]

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcılar listesinde.

6. İçinde **kullanıcılar ve gruplar** iletişim kutusu, tıklatın **seçin** düğmesi.

7. İçinde **eklemek atama** iletişim kutusu, tıklatın **atamak** düğmesi.

### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin
Azure AD çoklu oturum açma yapılandırmanızı erişim paneli kullanarak sınayın.

Erişim paneli Clarizen parçasında tıklattığınızda, otomatik olarak Clarizen uygulamanıza oturum açmanız.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_203.png
