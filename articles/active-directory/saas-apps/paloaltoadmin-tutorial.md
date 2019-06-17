---
title: 'Öğretici: Palo Alto Networks - yönetici kullanıcı Arabirimi ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Palo Alto Networks - yönetici kullanıcı Arabirimi ile Azure Active Directory arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: a826eaec-15af-4c85-8855-8a3374d1efb9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/02/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: dd0ca3bb356319f4661e24b192a5f7e776d14cd0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67095079"
---
# <a name="tutorial-azure-active-directory-integration-with-palo-alto-networks---admin-ui"></a>Öğretici: Palo Alto Networks - yönetici kullanıcı Arabirimi ile Azure Active Directory Tümleştirme

Bu öğreticide, Palo Alto Networks - yönetici Arabirimine Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.
Palo Alto Networks tarafından sağlanan-yönetici Arabirimine Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Palo Alto Networks - yönetici Arabirimine erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak Palo Alto Networks - yönetici Arabirimine (çoklu oturum açma) ile kendi Azure AD hesapları için oturum açmış, kullanıcılarınızın etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Palo Alto Networks - yönetici kullanıcı Arabirimi, Azure AD tümleştirmesi yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Abonelik Palo Alto ağları - yönetici Arabirimine çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Palo Alto Networks tarafından sağlanan - yönetici kullanıcı Arabirimi destekler **SP** tarafından başlatılan
* Palo Alto Networks tarafından sağlanan - yönetici kullanıcı Arabirimi destekler **zamanında** kullanıcı sağlama

## <a name="adding-palo-alto-networks---admin-ui-from-the-gallery"></a>Palo Alto Networks tarafından sağlanan - yönetici Arabirimine galerisinden ekleme

Palo Alto Networks - Azure AD'de yönetici Arabirimine tümleştirmesini yapılandırmak için yönetici Arabirimine galerisinden yönetilen SaaS listenizden Palo Alto Networks - eklemeniz gerekir.

**Galeri, yönetici kullanıcı Arabiriminden Palo Alto Networks - eklemek için aşağıdaki adımları uygulayın:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Palo Alto Networks - yönetici Arabirimine**seçin **Palo Alto Networks - yönetici Arabirimine** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Palo Alto Networks tarafından sağlanan-sonuç listesinde yönetici kullanıcı Arabirimi](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Palo Alto Networks ile test etme - yönetici kullanıcı Arabirimi tabanlı adlı bir test kullanıcı **Britta Simon**.
İş, bir Azure AD kullanıcısının Palo Alto Networks - ilgili kullanıcı arasında bir bağlantı ilişki için çoklu oturum açma için yönetici kullanıcı Arabirimi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Palo Alto Networks ile-test etmek için yönetici Arabirimine, aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Palo Alto Networks tarafından sağlanan - yönetici kullanıcı Arabirimi çoklu oturum açmayı yapılandırma](#configure-palo-alto-networks---admin-ui-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Palo Alto Networks tarafından sağlanan-yönetici kullanıcı Arabirimi test kullanıcısı oluşturma](#create-palo-alto-networks---admin-ui-test-user)**  - Palo Alto Networks - kullanıcı Azure AD gösterimini bağlı yönetici Arabirimine Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Palo Alto Networks - yönetici kullanıcı Arabirimi, Azure AD çoklu oturum açmayı yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Palo Alto Networks - yönetici Arabirimine** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Palo Alto ağ - yönetici kullanıcı Arabirimi etki alanı ve URL'ler tek bilgi'oturum açma](common/sp-identifier-reply.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<Customer Firewall FQDN>/php/login.php`

    b. İçinde **tanımlayıcı** kutusuna şu biçimi kullanarak bir URL yazın: `https://<Customer Firewall FQDN>:443/SAML20/SP`

    c. İçinde **yanıt URL'si** metin kutusuna onaylama tüketici hizmeti (ACS) URL'yi şu biçimde yazın: `https://<Customer Firewall FQDN>:443/SAML20/SP/ACS`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si, tanımlayıcı ve yanıt URL'si ile güncelleştirin. İlgili kişi [Palo Alto Networks - yönetici kullanıcı Arabirimi istemci Destek ekibine](https://support.paloaltonetworks.com/support) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Palo Alto Networks tarafından sağlanan - yönetici kullanıcı Arabirimi uygulama belirli bir biçimde SAML onaylamalarını bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** uygulama tümleştirme sayfasında bölümü. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için düğmeyi **kullanıcı öznitelikleri** iletişim.

    ![image](common/edit-attribute.png)

   > [!NOTE]
   > Öznitelik değerleri yalnızca örnek olduğundan için uygun değerleri harita *kullanıcıadı* ve *adminrole*. İsteğe bağlı başka bir özniteliği *accessdomain*, güvenlik duvarını belirli sanal sistemlerde yönetici erişimini kısıtlamak için kullanılır.
   >

6. İçinde **kullanıcı taleplerini** bölümünde **kullanıcı öznitelikleri** iletişim kutusunda, SAML belirteci özniteliği yukarıdaki görüntüde gösterilen şekilde yapılandırın ve aşağıdaki adımları gerçekleştirin:

    | Ad |  Kaynak özniteliği|
    | --- | --- |
    | username | User.userPrincipalName |
    | adminrole | customadmin |
    | | |

    a. Tıklayın **Ekle yeni talep** açmak için **yönetmek, kullanıcı talepleri** iletişim.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. Bırakın **Namespace** boş.

    d. Kaynağı olarak **özniteliği**.

    e. Gelen **kaynak özniteliği** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    f. Tıklayın **Tamam**

    g. **Kaydet**’e tıklayın.

    > [!NOTE]
    > Öznitelikler hakkında daha fazla bilgi için aşağıdaki makalelere bakın:
    > * [Yönetici kullanıcı Arabirimi (adminrole) için Yönetici rolü profili](https://www.paloaltonetworks.com/documentation/80/pan-os/pan-os/firewall-administration/manage-firewall-administrators/configure-an-admin-role-profile)
    > * [Cihaz erişim etki alanı yöneticisi kullanıcı arabirimi (accessdomain)](https://www.paloaltonetworks.com/documentation/80/pan-os/web-interface-help/device/device-access-domain)

7. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

8. Üzerinde **Palo Alto Networks - yönetici kullanıcı Arabirimi ayarlama** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-palo-alto-networks---admin-ui-single-sign-on"></a>Palo Alto Networks tarafından sağlanan - yönetici Arabirimine çoklu oturum açmayı yapılandırın

1. Palo Alto ağ güvenlik duvarı yönetici Arabirimine yeni bir pencerede yönetici olarak açın.

2. Seçin **cihaz** sekmesi.

    ![Cihaz sekmesi](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_admin1.png)

3. Sol bölmede seçin **SAML kimlik sağlayıcısı**ve ardından **alma** meta veri dosyası içeri aktarmak için.

    ![İçeri aktarma meta veri dosyası düğmesi](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_admin2.png)

4. İçinde **SAML tanımlamak sağlayıcısı sunucusu Profil alma** penceresinde aşağıdakileri yapın:

    !["SAML tanımlamak sağlayıcısı sunucu profili içeri aktar" penceresi](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_idp.png)

    a. İçinde **profil adı** kutusunda, bir ad sağlayın (örneğin, **AzureAD yönetici Arabirimine**).
    
    b. Altında **kimlik sağlayıcısı meta verileri**seçin **Gözat**, daha önce Azure portalından indirdiğiniz metadata.xml dosyası seçin.
    
    c. NET **kimlik sağlayıcısı sertifikası doğrulama** onay kutusu.
    
    d. **Tamam**’ı seçin.
    
    e. Güvenlik duvarı yapılandırmaları uygulamak için seçin **işleme**.

5. Sol bölmede seçin **SAML kimlik sağlayıcısı**ve ardından SAML kimlik sağlayıcısı profilini seçin (örneğin, **AzureAD yönetici Arabirimine**), önceki adımda oluşturduğunuz.

    ![SAML kimlik sağlayıcısı profili](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_idp_select.png)

6. İçinde **SAML kimlik sağlayıcısı Server profili** penceresinde aşağıdakileri yapın:

    !["SAML kimlik sağlayıcısı sunucu profili" penceresi](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_slo.png)
  
    a. İçinde **kimlik sağlayıcısı SLO URL'si** kutusunda, daha önce içeri aktarılan SLO URL aşağıdaki URL ile değiştirin: `https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0`
  
    b. **Tamam**’ı seçin.

7. Palo Alto ağları Firewall'un yönetici kullanıcı Arabirimi üzerindeki seçin **cihaz**ve ardından **yönetici rolleri**.

8. Seçin **Ekle** düğmesi.

9. İçinde **Yönetici rolü profili** penceresi içinde **adı** kutusunda, Yönetici rolü için bir ad sağlayın (örneğin, **fwadmin**). Yönetici rolü adı, kimlik sağlayıcısı tarafından gönderilen SAML Yönetici rolü öznitelik adı eşleşmelidir. Yönetici rolü adını ve değerini oluşturulmuş **kullanıcı öznitelikleri** bölümünde Azure portalında.

    ![Palo Alto ağları yönetici rolünü yapılandırma](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_adminrole.png)
  
10. Güvenlik duvarının yönetici kullanıcı Arabirimi üzerindeki seçin **cihaz**ve ardından **kimlik doğrulama profili**.

11. Seçin **Ekle** düğmesi.

12. İçinde **kimlik doğrulama profili** penceresinde aşağıdakileri yapın: 

    !["Kimlik doğrulama profili" penceresi](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_authentication_profile.png)

    a. İçinde **adı** kutusunda, bir ad sağlayın (örneğin, **AzureSAML_Admin_AuthProfile**).

    b. İçinde **türü** aşağı açılan listesinden **SAML**. 

    c. İçinde **IDP Server profili** aşağı açılan listesinde, uygun SAML kimlik sağlayıcısı sunucusunu profilini seçin (örneğin, **AzureAD yönetici Arabirimine**).

    c. Seçin **etkinleştirme çoklu oturum kapatma** onay kutusu.

    d. İçinde **Yönetici rolü özniteliği** kutusunda, öznitelik adını girin (örneğin, **adminrole**).

    e. Seçin **Gelişmiş** sekmesini ve sonra **izin verilenler listesi**seçin **Ekle**.

    ![Gelişmiş sekmesinde Ekle düğmesine](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_allowlist.png)

    f. Seçin **tüm** onay kutusunu işaretleyin veya kullanıcıları ve bu profille doğrulanabilir grupları seçin.  
    Kullanıcı Kimliği doğruladığında, güvenlik duvarı ilişkili kullanıcı adı veya grup bu listedeki girdiler karşı eşleşir. Girdileri eklemezseniz, hiçbir kullanıcı kimlik doğrulaması yapabilir.

    g. **Tamam**’ı seçin.

13. SAML SSO Azure kullanarak yöneticiler etkinleştirmek için seçin **cihaz** > **Kurulum**. İçinde **Kurulum** bölmesinde **Yönetim** sekmesini ve sonra **kimlik doğrulama ayarları**seçin **ayarları** ("dişli") düğmesi .

    ![Ayarlar düğmesi](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_authsetup.png)

14. Kimlik doğrulama profili penceresinde oluşturulan SAML kimlik doğrulaması profilini seçin (örneğin, **AzureSAML_Admin_AuthProfile**).

    ![Kimlik doğrulama profili alanı](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_authsettings.png)

15. **Tamam**’ı seçin.

16. Yapılandırmayı uygulamak için seçin **işleme**.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü **brittasimon\@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Palo Alto Networks - yönetici Arabirimine erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Palo Alto Networks - yönetici Arabirimine**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Palo Alto Networks - yönetici Arabirimine**.

    ![-Yönetici kullanıcı Arabirimi bağlantısı uygulamalar listesinde Palo Alto ağlar](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-palo-alto-networks---admin-ui-test-user"></a>Palo Alto Networks tarafından sağlanan-yönetici kullanıcı Arabirimi test kullanıcısı oluşturma

Palo Alto Networks tarafından sağlanan - yönetici kullanıcı Arabirimi, just-ın-time kullanıcı sağlamayı destekler. Bir kullanıcı zaten mevcut değilse, sistemde başarılı bir kimlik doğrulamasından sonra otomatik olarak oluşturulur. Herhangi bir eylemi, kullanıcı oluşturmak için gerekli değildir.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Palo Alto Networks - yönetici Arabirimine kutucuk erişim Paneli'nde tıklattığınızda, otomatik olarak Palo Alto Networks - yönetici Arabirimine SSO'yu ayarlamak için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
