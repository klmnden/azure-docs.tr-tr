---
title: 'Öğretici: Azure Active Directory Palo Alto Networks - yönetici kullanıcı Arabirimi ile tümleştirme | Microsoft Docs'
description: Palo Alto Networks - yönetici kullanıcı Arabirimi ile Azure Active Directory arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: a826eaec-15af-4c85-8855-8a3374d1efb9
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2018
ms.author: jeedes
ms.openlocfilehash: fed368c0df265495d9fee764f86825957fae8bab
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39447433"
---
# <a name="integrate-azure-active-directory-with-palo-alto-networks---admin-ui"></a>Yönetici Arabirimine Palo Alto Networks - Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) Palo Alto Networks - yönetici kullanıcı Arabirimi ile tümleştirme konusunda bilgi edinin.

Palo Alto Networks - yönetici kullanıcı Arabirimi ile tümleştirme Azure AD tarafından aşağıdaki avantajlardan yararlanabilirsiniz:

- Palo Alto Networks - yönetici Arabirimine erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak Palo Alto Networks - yönetici kullanıcı Arabirimi (çoklu oturum açma veya SSO) ile Azure AD hesaplarına açan, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda, Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında bilgi edinmek için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Palo Alto Networks - yönetici kullanıcı Arabirimi, Azure AD tümleştirmesi yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Palo Alto ağları tasarlanan yeni nesil güvenlik duvarı veya Panorama (güvenlik duvarları için merkezi yönetim sistemi)

> [!NOTE]
> Bu öğreticideki adımları test ettiğinizde, bunu yapmanızı öneririz *değil* bir üretim ortamı kullanın.

Bu öğreticideki adımları test etmek için aşağıdaki önerileri uygulayın:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. 

Bu öğreticide açıklanan senaryo iki temel yapı taşları oluşur:

* Palo Alto Networks tarafından sağlanan - yönetici Arabirimine galerisinden ekleme
* Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="add-palo-alto-networks---admin-ui-from-the-gallery"></a>Palo Alto Networks tarafından sağlanan - yönetici Arabirimine Galerisi ekleyin
Palo Alto Networks - yönetici kullanıcı Arabirimi, Azure AD'nin tümleştirmesini yapılandırmak için aşağıdakileri yaparak yönetilen SaaS listenizden galerisinden yönetici Arabirimine Palo Alto Networks - ekleyin:

1. İçinde [Azure portalında](https://portal.azure.com), sol bölmede seçin **Azure Active Directory**. 

    ![Azure Active Directory düğmesi][1]

1. Seçin **kurumsal uygulamalar** > **tüm uygulamaları**.

    !["Kurumsal uygulamalar" penceresi][2]
    
1. Yeni bir uygulama eklemek için seçin **yeni uygulama** penceresinin üstünde düğme.

    !["Yeni uygulama" düğmesi][3]

1. Arama kutusuna **Palo Alto Networks - yönetici Arabirimine**seçin **Palo Alto Networks - yönetici Arabirimine** sonuç listesini ve ardından **Ekle**.

    ![Palo Alto Networks tarafından sağlanan-sonuç listesinde yönetici kullanıcı Arabirimi](./media/paloaltoadmin-tutorial/tutorial_step4-add-from-the-gallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırmanız ve Palo Alto Networks - yönetici kullanıcı Arabirimi ile Azure AD çoklu oturum açmayı test "Britta Simon." adlı bir test kullanıcı tabanlı

Tek iş için oturum açma için Azure AD Palo Alto Networks - yönetici kullanıcı Arabirimi kullanıcı ve kendisine karşılık gelen Azure AD'de tanımlaması gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Palo Alto Networks - yönetici kullanıcı Arabirimi aynı kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Bağlantı kurmak için Palo Alto Networks - yönetici kullanıcı Arabirimi atama *kullanıcıadı* değerini *kullanıcı adı* Azure AD'de.

Yapılandırma ve Azure AD çoklu oturum açma Palo Alto Networks - yönetici kullanıcı Arabirimi ile test etmek için sonraki beş bölümlerde yapı taşlarını tamamlayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Palo Alto Networks - aşağıdakileri yaparak yönetici kullanıcı Arabirimi uygulamasını yapılandırın:

1. Azure portalında, üzerinde **Palo Alto Networks - yönetici Arabirimine** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    !["Çoklu oturum açma" bağlantısı][4]

1. İçinde **çoklu oturum açma** penceresi içinde **çoklu oturum açma modunu** kutusunda **SAML tabanlı oturum açma**.
 
    !["Çoklu oturum açma" penceresi](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_samlbase.png)

1. Altında **Palo Alto Networks - yönetici kullanıcı Arabirimi etki alanı ve URL'ler**, aşağıdakileri yapın:

    !["Palo Alto Networks - yönetici kullanıcı Arabirimi etki alanı ve URL'ler" çoklu oturum açma bilgileri](./media/paloaltoadmin-tutorial/tutorial_general_show_advanced_url.png)
    
    a. İçinde **oturum açma URL'si** kutusuna bir URL aşağıdaki biçimde yazın: *https://\<müşteri güvenlik duvarı FQDN > /php/login.php*.

    b. İçinde **tanımlayıcı** kutusuna bir URL aşağıdaki biçimde yazın: *https://\<müşteri güvenlik duvarı FQDN >: 443/SAML20/SP*.
    
    c. İçinde **yanıt URL'si** kutusuna onaylama tüketici hizmeti (ACS) URL'yi şu biçimde yazın: *https://\<müşteri güvenlik duvarı FQDN >: 443/SAML20/SP/ACS*.
    
    > [!NOTE] 
    > Yukarıdaki değerleri gerçek değildir. Bunları tanımlayıcısı ve gerçek oturum açma URL'si ile güncelleştirin. Değerlerini almak için iletişime geçin [Palo Alto Networks - yönetici kullanıcı Arabirimi istemci Destek ekibine](https://support.paloaltonetworks.com/support). 
 
1. Palo Alto Networks - yönetici kullanıcı Arabirimi uygulama belirli bir biçimde SAML onaylamalarını beklediği, talepleri aşağıdaki görüntüde gösterildiği gibi yapılandırın. Öznitelik değerleri yönetme **kullanıcı öznitelikleri** bölümünü **uygulama tümleştirmesi** aşağıdakileri yaparak sayfası:
    
    ![SAML belirteci öznitelikleri listesi](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_attribute.png)
    
   > [!NOTE]
   > Öznitelik değerleri yalnızca örnek olduğundan için uygun değerleri harita *kullanıcıadı* ve *adminrole*. İsteğe bağlı başka bir özniteliği *accessdomain*, güvenlik duvarını belirli sanal sistemlerde yönetici erişimini kısıtlamak için kullanılır.
   >
        
    | Öznitelik adı | Öznitelik değeri |
    | --- | --- |    
    | kullanıcı adı | User.userPrincipalName |
    | adminrole | customadmin |

    a. Seçin **eklemek agentconfigutil**.  
    
    !["Öznitelik Ekle" düğmesi](./media/paloaltoadmin-tutorial/tutorial_attribute_04.png)

    **Öznitelik Ekle** penceresi açılır.

    !["Öznitelik Ekle" penceresi](./media/paloaltoadmin-tutorial/tutorial_attribute_05.png)
    
    b. İçinde **adı** ilgili satır için gösterilen öznitelik adı yazın.
    
    c. İçinde **değer** ilgili satır için gösterilen öznitelik değeri yazın.
    
    d. **Tamam**’ı seçin.

    > [!NOTE]
    > Öznitelikler hakkında daha fazla bilgi için aşağıdaki makalelere bakın:
    > * [Yönetici kullanıcı Arabirimi (adminrole) için Yönetici rolü profili](https://www.paloaltonetworks.com/documentation/80/pan-os/pan-os/firewall-administration/manage-firewall-administrators/configure-an-admin-role-profile)
    > * [Cihaz erişim etki alanı yöneticisi kullanıcı arabirimi (accessdomain)](https://www.paloaltonetworks.com/documentation/80/pan-os/web-interface-help/device/device-access-domain)
    >

1. Altında **SAML imzalama sertifikası**seçin **meta veri XML**ve ardından **Kaydet**.

    ![Meta veri XML indirme bağlantısı](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_certificate.png) 

    ![Kaydet düğmesi](./media/paloaltoadmin-tutorial/tutorial_general_400.png)

1. Palo Alto ağ güvenlik duvarı yönetici Arabirimine yeni bir pencerede yönetici olarak açın.

1. Seçin **cihaz** sekmesi.

    ![Cihaz sekmesi](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_admin1.png)

1. Sol bölmede seçin **SAML kimlik sağlayıcısı**ve ardından **alma** meta veri dosyası içeri aktarmak için.

    ![İçeri aktarma meta veri dosyası düğmesi](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_admin2.png)

1. İçinde **SAML tanımlamak sağlayıcısı sunucusu Profil alma** penceresinde aşağıdakileri yapın:

    !["SAML tanımlamak sağlayıcısı sunucu profili içeri aktar" penceresi](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_idp.png)

    a. İçinde **profil adı** kutusunda, bir ad sağlayın (örneğin, **AzureAD yönetici Arabirimine**).
    
    b. Altında **kimlik sağlayıcısı meta verileri**seçin **Gözat**, daha önce Azure portalından indirdiğiniz metadata.xml dosyası seçin.
    
    c. NET **kimlik sağlayıcısı sertifikası doğrulama** onay kutusu.
    
    d. **Tamam**’ı seçin.
    
    e. Güvenlik duvarı yapılandırmaları uygulamak için seçin **işleme**.

1. Sol bölmede seçin **SAML kimlik sağlayıcısı**ve ardından SAML kimlik sağlayıcısı profilini seçin (örneğin, **AzureAD yönetici Arabirimine**), önceki adımda oluşturduğunuz. 

    ![SAML kimlik sağlayıcısı profili](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_idp_select.png)

1. İçinde **SAML kimlik sağlayıcısı Server profili** penceresinde aşağıdakileri yapın:

    !["SAML kimlik sağlayıcısı sunucu profili" penceresi](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_slo.png)
  
    a. İçinde **kimlik sağlayıcısı SLO URL'si** kutusunda, daha önce içeri aktarılan SLO URL aşağıdaki URL ile değiştirin: **https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0**.
  
    b. **Tamam**’ı seçin.

1. Palo Alto ağları Firewall'un yönetici kullanıcı Arabirimi üzerindeki seçin **cihaz**ve ardından **yönetici rolleri**.

1. Seçin **Ekle** düğmesi. 

1. İçinde **Yönetici rolü profili** penceresi içinde **adı** kutusunda, Yönetici rolü için bir ad sağlayın (örneğin, **fwadmin**).  
    Yönetici rolü adı, kimlik sağlayıcısı tarafından gönderilen SAML Yönetici rolü öznitelik adı eşleşmelidir. Yönetici rolü adını ve değerini 4. adımda oluşturulan.

    ![Palo Alto ağları yönetici rolünü yapılandırma](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_adminrole.png)
  
1. Güvenlik duvarının yönetici kullanıcı Arabirimi üzerindeki seçin **cihaz**ve ardından **kimlik doğrulama profili**.

1. Seçin **Ekle** düğmesi. 

1. İçinde **kimlik doğrulama profili** penceresinde aşağıdakileri yapın: 

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

1. SAML SSO Azure kullanarak yöneticiler etkinleştirmek için seçin **cihaz** > **Kurulum**. İçinde **Kurulum** bölmesinde **Yönetim** sekmesini ve sonra **kimlik doğrulama ayarları**seçin **ayarları** ("dişli") düğmesi . 

 ![Ayarlar düğmesi](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_authsetup.png)

1. 17. adımda oluşturduğunuz SAML kimlik doğrulaması profilini seçin (örneğin, **AzureSAML_Admin_AuthProfile**).

 ![Kimlik doğrulama profili alanı](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_authsettings.png)

1. **Tamam**’ı seçin.

1. Yapılandırmayı uygulamak için seçin **işleme**.


> [!TIP]
> Uygulamayı hazırlama tutunun gibi önceki yönergeleri kısa bir sürümünü edinebilirsiniz [Azure portalında](https://portal.azure.com). Uygulamayı ekledikten sonra **Active Directory** > **kurumsal uygulamalar** bölümünden **çoklu oturum açma** sekmesine ve ardından erişim belgelerinde katıştırılmış **yapılandırma** alttaki bölümü. Ekli belge özelliği hakkında daha fazla bilgi için bkz. [Azure AD belgeleri katıştırılmış]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, aşağıdakileri yaparak Azure portalında, Britta Simon test kullanıcısı oluşturun:

![Bir Azure AD test kullanıcısı oluşturma][100]

1. Azure portalında, sol bölmede seçin **Azure Active Directory**.

    ![Azure Active Directory bağlantısı](./media/paloaltoadmin-tutorial/create_aaduser_01.png)

1. Geçerli kullanıcıların bir listesini görüntülemek için seçin **kullanıcılar ve gruplar** > **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/paloaltoadmin-tutorial/create_aaduser_02.png)

1. Üst kısmındaki **tüm kullanıcılar** penceresinde **Ekle**.

    ![Ekle düğmesi](./media/paloaltoadmin-tutorial/create_aaduser_03.png)
    
    **Kullanıcı** penceresi açılır.

1. İçinde **kullanıcı** penceresinde aşağıdakileri yapın:

    ![Kullanıcı penceresi](./media/paloaltoadmin-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’u seçin.
 
### <a name="create-a-palo-alto-networks---admin-ui-test-user"></a>Palo Alto Networks - yönetici kullanıcı Arabirimi test kullanıcısı oluşturma

Palo Alto Networks tarafından sağlanan - yönetici kullanıcı Arabirimi, just-ın-time kullanıcı sağlamayı destekler. Bir kullanıcı zaten mevcut değilse, sistemde başarılı bir kimlik doğrulamasından sonra otomatik olarak oluşturulur. Herhangi bir eylemi, kullanıcı oluşturmak için gerekli değildir.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, kullanıcı Palo Alto Networks - yönetici Arabirimine erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin. Bunu yapmak için aşağıdakileri yapın:

![Kullanıcı rolü atayın][200] 

1. Azure portalında açın **uygulamaları** görünümü, Git **dizin** görüntüleyin ve ardından **kurumsal uygulamalar** > **tüm uygulamaları**.

    !["Kurumsal uygulamalar" ve "Tüm uygulamalar" bağlantıları][201] 

1. İçinde **uygulamaları** listesinden **Palo Alto Networks - yönetici Arabirimine**.

    ![Palo Alto ağları - yönetici kullanıcı Arabirimi bağlantısı](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_app.png)  

1. Sol bölmede seçin **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Seçin **Ekle** ve daha sonra **atama Ekle** bölmesinde **kullanıcılar ve gruplar**.

    ![Atama Ekle bölmesi][203]

1. İçinde **kullanıcılar ve gruplar** penceresi içinde **kullanıcılar** listesinden **Britta Simon**.

1. Seçin **seçin** düğmesi.

1. İçinde **atama Ekle** penceresinde **atama**.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test.

Palo Alto Networks - erişim panelinde yönetici Arabirimine kutucuğu seçtiğinizde, otomatik olarak, Palo Alto Networks - yönetici kullanıcı Arabirimi uygulaması için oturum açmanız.

Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/paloaltoadmin-tutorial/tutorial_general_01.png
[2]: ./media/paloaltoadmin-tutorial/tutorial_general_02.png
[3]: ./media/paloaltoadmin-tutorial/tutorial_general_03.png
[4]: ./media/paloaltoadmin-tutorial/tutorial_general_04.png

[100]: ./media/paloaltoadmin-tutorial/tutorial_general_100.png

[200]: ./media/paloaltoadmin-tutorial/tutorial_general_200.png
[201]: ./media/paloaltoadmin-tutorial/tutorial_general_201.png
[202]: ./media/paloaltoadmin-tutorial/tutorial_general_202.png
[203]: ./media/paloaltoadmin-tutorial/tutorial_general_203.png

