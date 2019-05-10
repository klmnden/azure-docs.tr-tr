---
title: 'Öğretici: SAML SSO için çözüm GmbH tarafından Jıra ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Çoklu oturum açma SAML SSO Jıra için Azure Active Directory arasındaki GmbH çözünürlüğün yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 20e18819-e330-4e40-bd8d-2ff3b98e035f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/03/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 943131bc746b5d2a1fd95a26a6a6c9f3bb6b9e57
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65509949"
---
# <a name="tutorial-azure-active-directory-integration-with-saml-sso-for-jira-by-resolution-gmbh"></a>Öğretici: GmbH çözünürlüğün Jıra için SAML SSO ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile GmbH çözünürlüğün SAML SSO Jıra için ayarlama konusunda bilgi edinin.
SAML SSO için Jıra çözünürlüğün GmbH Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Jıra'ya SAML SSO eklentisi ile GmbH çözünürlüğüyle oturum açabilir, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak Jıra'ya ile Azure AD hesaplarına (çoklu oturum açma) GmbH çözünürlüğüyle Jıra için SAML SSO kullanarak oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi ve SAML SSO için Jıra GmbH çözünürlüğüyle yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Abonelik Jıra çözünürlüğün GmbH çoklu oturum açma için SAML SSO etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* SAML SSO için Jıra GmbH destekler çözünürlüğün **SP** ve **IDP** tarafından başlatılan

## <a name="adding-an-enterprise-application-for-single-sign-on"></a>Bir kurumsal uygulama için çoklu oturum açma ekleme

Azure AD'de çoklu oturum açmayı ayarlamak için yeni bir kuruluş uygulaması eklemeniz gerekir. Galeride, bunun için önceden önceden yapılandırılmış bir uygulama olduğundan **GmbH çözünürlüğün Jıra için SAML SSO**.

**SAML SSO için Jıra GmbH çözünürlüğüyle Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **GmbH çözünürlüğün Jıra için SAML SSO**seçin **GmbH çözünürlüğün Jıra için SAML SSO** sonuç paneli ve ardından **Ekle** düğme eklemek için uygulama. Ayrıca, kuruluş adını da değiştirebilirsiniz.

     ![SAML SSO için sonuç listesinde GmbH çözünürlüğün Jıra](common/search-new-app.png)

## <a name="configure-and-test-single-sign-on-with-the-saml-sso-plugin-and-azure-ad"></a>Yapılandırma ve Azure AD ve SAML SSO eklentisi ile çoklu oturum açmayı test etme

Bu bölümde, test ve çoklu oturum açma Jıra için bir Azure AD kullanıcısı için yapılandırın. Bu adlı bir test kullanıcısı için gerçekleştirilir **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ve SAML SSO Jıra için ilgili kullanıcı çözünürlüğün arasında bir bağlantı ilişki GmbH kurulması gerekir.

Yapılandırma ve çoklu oturum açmayı test etmek için aşağıdaki adımları tamamlamanız gerekir:

1. **[Azure AD Kurumsal uygulama için çoklu oturum açmayı yapılandırma](#configure-the-azure-ad-enterprise-application-for-single-sign-on)**  -Azure AD Kurumsal uygulama için çoklu oturum açmayı yapılandırın
2. **[SAML SSO eklentisi Jıra örneğinizin yapılandırma](#configure-the-saml-sso-plugin-of-your-jira-instance)**  -uygulama tarafında çoklu oturum açma ayarlarını yapılandırın.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  -Azure AD'de bir test kullanıcısı oluşturun.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  -etkinleştirme tek kullanmak üzere test kullanıcı oturum açma Azure tarafında.
1. **[Test kullanıcısı oluşturma Jıra'da](#create-the-test-user-also-in-jira)**  -karşılık gelen bir test kullanıcısı Jıra'da için Azure AD test kullanıcısı oluşturun.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  -yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-the-azure-ad-enterprise-application-for-single-sign-on"></a>Azure AD Kurumsal uygulama için çoklu oturum açmayı yapılandırın

Bu bölümde, çoklu oturum açma Azure portalındaki ayarlarsınız.

Çoklu oturum açma SAML SSO için Jıra ile GmbH çözünürlüğüyle yapılandırmak için aşağıdaki adımları uygulayın:

1. İçinde [Azure portalında](https://portal.azure.com/), yeni oluşturduğunuz içinde **GmbH çözünürlüğün Jıra için SAML SSO** Kurumsal uygulama, select **çoklu oturum açma** sol bölmesinde.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. İçin **tek bir oturum açma yönteminizi seçmeniz**seçin **SAML** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Daha sonra tıklayın **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. İçinde **temel SAML yapılandırma** uygulamada yapılandırmak isterseniz, bölümü **IDP** modu başlatılan ve ardından aşağıdaki adımları gerçekleştirin:

    ![Oturum açma bilgileri tek bir SAML SSO için Jıra çözünürlüğün GmbH etki alanı ve URL'ler](common/idp-intiated.png)

    a. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<server-base-url>/plugins/servlet/samlsso`

    b. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<server-base-url>/plugins/servlet/samlsso`

    c. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız aşağıdaki adımı gerçekleştirmeniz **SP** başlatılan modu:

    ![Oturum açma bilgileri tek bir SAML SSO için Jıra çözünürlüğün GmbH etki alanı ve URL'ler](common/metadata-upload-additional-signon.png)

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://<server-base-url>/plugins/servlet/samlsso`

    > [!NOTE]
    > Tanımlayıcı, yanıt URL'si ve oturum açma URL'si için alternatif  **\<sunucu temel url >** Jıra örneğinizin temel URL'si. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında. Bir sorun varsa, adresinden bize başvurun [SAML SSO Jıra çözünürlüğün GmbH istemci için Destek ekibine](https://www.resolution.de/go/support).

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde, indirme **Federasyon meta verileri XML** ve bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

### <a name="configure-the-saml-sso-plugin-of-your-jira-instance"></a>SAML SSO eklentisi Jıra örneğinizin yapılandırın 

1. Farklı bir web tarayıcı penceresinde bir Jıra Örneğiniz için bir yönetici olarak oturum açın.

2. Sağ tarafındaki dişli üzerine gelin ve tıklayın **uygulamaları yönetme**.
    
    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/addon1.png)

3. Yönetici erişimi sayfasına yönlendirilirsiniz, girin **parola** tıklatıp **Onayla** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/addon2.png)

4. Jıra normalde Atlassian Market'te yönlendirir. Aksi takdirde, tıklayarak **yeni uygulamalar Bul** sol bölmesinde. Arama **SAML çoklu oturum açma (SSO) JIRA için** tıklatıp **yükleme** SAML eklentisini yüklemek için düğmesine.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/store.png)

5. Eklenti yüklemesi başlatılır. İşlem tamamlandığında tıklayın **Kapat** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/store-2.png)

    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/store-3.png)

6. ' A tıklayarak **Yönet**.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/store-4.png)
    
8. Daha sonra tıklayın **yapılandırma** yalnızca yüklü eklentiyi yapılandırmak için.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/store-5.png)

9. İçinde **SAML SingleSignOn eklentisi yapılandırma** Sihirbazı'nı tıklatın **yeni IDP ekleme** yeni bir kimlik sağlayıcısı olarak Azure AD'ye yapılandırmak için.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/addon4.png) 

10. Üzerinde **SAML kimlik sağlayıcınızı seçin** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/addon5a.png)
 
    a. Ayarlama **Azure AD'ye** IDP türü.
    
    b. Ekleme **adı** kimlik sağlayıcısının (ör. Azure AD).
    
    c. (İsteğe bağlı) ekleme **açıklama** kimlik sağlayıcısının (ör. Azure AD).
    
    d. **İleri**’ye tıklayın.
    
11. Üzerinde **kimlik sağlayıcı Yapılandırması** sayfasında **sonraki**.
 
    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/addon5b.png)

12. Üzerinde **meta verileri içeri aktarma SAML IDP** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/addon5c.png)

    a. Tıklayın **meta veri XML dosyasını seçin** düğmesi ve çekme **Federasyon meta verileri XML** önce indirmiş olduğunuz dosya.

    b. Tıklayın **alma** düğmesi.
     
    c. İçeri aktarma başarılı olana kadar kısa bir süre bekleyin.  
     
    d. Tıklayın **sonraki** düğmesi.
    
13. Üzerinde **kullanıcı kimliği öznitelik ve dönüştürme** sayfasında **sonraki** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/addon5d.png)
    
14. Üzerinde **kullanıcı oluşturma ve güncelleştirme** sayfasında **sonraki & Kaydet** ayarları kaydetmek için.
    
    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/addon6a.png)
    
15. Üzerinde **ayarlarınızı test etme** sayfasında **test atlayın ve el ile yapılandırma** kullanıcı test şimdilik atlamak için. Bu, sonraki bölümde gerçekleştirilir ve bazı ayarlar Azure portalında gerektirir.
    
    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/addon6b.png)
    
16. Tıklayın **Tamam** uyarı atlanacak.
    
    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/addon6c.png)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır. Kullanıcı ile çoklu oturum açmayı test eder.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. İçinde **kullanıcı özelliklerini**, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alanına **Britta Simon**.
  
    b. İçinde **kullanıcı adı** alanına <b> BrittaSimon@contoso.com </b>.

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Simon her çoklu oturum açma kullanacak şekilde sağlayan kurumsal uygulamaya ekleyin.

1. Azure portalında **kurumsal uygulamalar**ve ardından **tüm uygulamaları**. 

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Bu öğreticinin başlangıçta oluşturduğunuz Kurumsal uygulama uygulamalar listesinde arayın. Bu öğreticideki adımları takip ediyorsanız adlı **GmbH çözünürlüğün Jıra için SAML SSO**. Başka bir ad verdiyseniz, bu adını aratın.

    ![SAML SSO uygulamaları listesinde çözümleme GmbH bağlantısıyla Jıra için](common/all-applications.png)

3. Sol bölmede bulunan tıklayın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Seçin **Kullanıcı Ekle**ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıların listesini ve ardından **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylama işlemi herhangi bir rolü değer daha sonra beklediğiniz varsa **rolü Seç** iletişim kutusunda, listeden bir kullanıcı için uygun rolü seçin ve ardından **seçin** ekranın alt kısmındaki düğmesi .

7. İçinde **atama Ekle** iletişim kutusunda, tıklayın **atama** düğmesi.

### <a name="create-the-test-user-also-in-jira"></a>Test kullanıcısı Jıra'da oluşturabilir

SAML SSO için Jıra GmbH çözünürlüğüyle oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunlar Jıra için SAML SSO içine GmbH çözüm tarafından sağlanması gerekir. Bu öğreticinin çalışması için el ile sağlama yapmanız gerekir. Ancak, aynı zamanda diğer sağlama modeli vardır, çözünürlüğün SAML SSO eklentisi için kullanılabilir örneğin **zamanında** sağlama. Kendi belgelerine başvurmak [SAML SSO GmbH çözünürlüğün](https://wiki.resolution.de/doc/saml-sso/latest/all). İlgili bir sorunuz varsa, desteğe başvurun [çözünürlük desteği](https://www.resolution.de/go/support).

**Bir kullanıcı hesabını el ile sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Jıra örneğine yönetici olarak oturum açın.

2. Dişli ve select üzerine geldiğinizde **kullanıcı yönetimi**.

   ![Çalışan Ekle](./media/samlssojira-tutorial/user1.png)

3. Yönetici erişimi sayfasına yönlendirilirsiniz, enter **parola** tıklatıp **Onayla** düğmesi.

    ![Çalışan Ekle](./media/samlssojira-tutorial/user2.png) 

4. Altında **kullanıcı yönetimi** sekmesinde bölüm **oluşturacağı**.

    ![Çalışan Ekle](./media/samlssojira-tutorial/user3-new.png) 

5. Üzerinde **"Yeni kullanıcı oluşturma"** iletişim sayfasında, aşağıdaki adımları gerçekleştirin. Kullanıcı tam olarak oluşturmak sahip olduğunuz Azure AD'de ister:

    ![Çalışan Ekle](./media/samlssojira-tutorial/user4-new.png) 

    a. İçinde **e-posta adresi** metin kutusuna kullanıcı e-posta adresini yazın: <b> BrittaSimon@contoso.com </b>.

    b. İçinde **tam adı** metin kutusuna kullanıcının tam adını yazın: **Britta Simon**.

    c. İçinde **kullanıcıadı** metin kutusuna kullanıcı e-posta adresini yazın: <b> BrittaSimon@contoso.com </b>. 

    d. İçinde **parola** metin kutusu, kullanıcının parolasını girin.

    e. Tıklayın **kullanıcı oluşturma** kullanıcı oluşturmayı tamamlamak için.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Çözüm GmbH kutucuk erişim Paneli'nde tarafından Jıra için SAML SSO'ye tıkladığınızda, otomatik olarak Jıra için SAML SSO için SSO'yu ayarlama çözümleme GmbH tarafından oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

İçin giderseniz, aynı zamanda çoklu oturum açma, test edebilirsiniz [https://\<sunucu temel URL'si >/servlet/plugins/samlsso](https://\<server-base-url>/plugins/servlet/samlsso). Yedek  **\<sunucu temel url >** Jıra örneğinizin temel URL'si.


## <a name="enable-single-sign-on-redirection-for-jira"></a>Çoklu oturum açma için yeniden yönlendirmeyi Jıra etkinleştir

Önce bölümünde belirtildiği gibi çoklu oturum açma tetikleyicisi için şu anda iki yolu vardır. Kullanılarak **Azure portalında** veya bu adı kullanıyor **Jıra Örneğinize özel bir bağlantıyı**. SAML SSO eklentisi GmbH çözünürlüğün Ayrıca, çoklu oturum açma tarafından yalnızca tetiklemek sağlar **Jıra Örneğinize işaret eden herhangi bir URL'ye erişme**.

Esas olarak, Jıra erişen tüm kullanıcılar için tek bir seçenek eklenti etkinleştirme sonra oturum yönlendirilirsiniz.

SSO yeniden yönlendirme etkinleştirmek için aşağıdakileri yapabilirsiniz **Jıra örneğinizin**:

1. SAML SSO eklentisi jıra'da yapılandırma sayfasında erişin.
1. Tıklayarak **yeniden yönlendirme** sol bölmesinde.
![](./media/samlssojira-tutorial/ssore1.png)

1. Değer çizgisi **etkinleştirme SSO yeniden yönlendirme**.
![](./media/samlssojira-tutorial/ssore2.png) 

1. Tuşuna **Ayarları Kaydet** sağ üst köşedeki düğmesi.

Seçeneğini etkinleştirdikten sonra kullanıcı adı/parola istemi if ulaşmaya devam edebilirsiniz **etkinleştirme nosso** seçeneği giderek işaretlendiğinden [https://\<sunucu temel url > /login.jsp?nosso](https://\<server-base-url>/login.jsp?nosso). Her zaman alternatif  **\<sunucu temel url >** temel URL'niz ile.


## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

