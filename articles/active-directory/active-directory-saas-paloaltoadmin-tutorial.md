---
title: 'Öğretici: Azure Active Directory Palo Alto Networks - Admin kullanıcı Arabirimi ile tümleştirin. | Microsoft Docs'
description: Çoklu oturum açma Palo Alto ağları - Admin kullanıcı Arabirimi ve Azure Active Directory arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: a826eaec-15af-4c85-8855-8a3374d1efb9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: jeedes
ms.openlocfilehash: aa3366810a40b004fe510cb2909f8da0f3513ddb
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="integrate-azure-active-directory-with-palo-alto-networks---admin-ui"></a>Yönetici UI Palo Alto ağlarla - Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) Palo Alto ağlarını - Admin kullanıcı Arabirimi ile tümleştirmeye öğrenin.

Palo Alto Networks - Admin kullanıcı Arabirimi ile tümleştirme Azure AD tarafından aşağıdaki yararları alın:

- Palo Alto Networks - Admin kullanıcı Arabirimi erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak Palo Alto Networks - yönetici kullanıcı Arabirimi (çoklu oturum açma veya SSO) ile Azure AD hesaplarına açan, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda, Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında bilgi edinmek için [uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Palo Alto ağlarla - yönetici UI yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Palo Alto ağları yeni nesil güvenlik duvarı veya Panorama (güvenlik duvarları için merkezi yönetim sistemi)

> [!NOTE]
> Bu öğreticide adımları test ettiğinizde, bunu yapmanızı öneririz *değil* bir üretim ortamında kullanın.

Bu öğreticide adımları test etmek için aşağıdaki önerileri uygulayın:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. 

Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

* Yönetici UI galerisinden Palo Alto ağlar - ekleme
* Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="add-palo-alto-networks---admin-ui-from-the-gallery"></a>Yönetici UI galerisinden Palo Alto ağları - ekleyin
Azure ad tümleştirme Palo Alto ağlarla - yönetici UI yapılandırmak için aşağıdakileri yaparak yönetilen SaaS uygulamaları listenize galerisinden yönetici UI Palo Alto Networks - ekleyin:

1. İçinde [Azure portal](https://portal.azure.com), sol bölmede seçin **Azure Active Directory**. 

    ![Azure Active Directory düğmesi][1]

2. Seçin **kurumsal uygulamalar** > **tüm uygulamaları**.

    !["Kurumsal uygulamalar" penceresi][2]
    
3. Yeni bir uygulama eklemek için seçin **yeni uygulama** pencerenin üstündeki düğmesi.

    !["Yeni uygulama" düğmesi][3]

4. Arama kutusuna **Palo Alto Networks - yönetici UI**seçin **Palo Alto Networks - yönetici UI** sonuçları listesi ve ardından **Ekle**.

    ![Palo Alto Networks - sonuçlar listesinde yönetim kullanıcı Arabirimi](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_step4-add-from-the-gallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırmanız ve Palo Alto Networks - Admin kullanıcı Arabirimi ile Azure AD çoklu oturum açmayı test "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı

Tekli çalışmaya oturum için - Admin kullanıcı Arabirimi kullanıcı ve kendisine karşılık gelen Azure AD'de Palo Alto ağları tanımlamak Azure AD gerekiyor. Diğer bir deyişle, bir Azure AD kullanıcısının Palo Alto Networks - Admin kullanıcı Arabirimi aynı kullanıcı arasında bir bağlantı ilişkisi kurulmalıdır.

Bağlantı ilişkisi kurmak için Palo Alto ağları - yönetici UI atama *kullanıcıadı* değerini *kullanıcı adı* Azure AD'de.

Yapılandırma ve Azure AD çoklu oturum açma Palo Alto Networks - Admin kullanıcı Arabirimi ile test etme için yapı taşları sonraki beş bölümlerde tamamlayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Palo Alto ağlarınızı - aşağıdakileri yaparak yönetici UI uygulama yapılandırın:

1. Azure portalında üzerinde **Palo Alto Networks - yönetici UI** uygulama tümleştirmesi sayfasında, **çoklu oturum açma**.

    !["Çoklu oturum açmayı" bağlantı][4]

2. İçinde **çoklu oturum açma** penceresi, **tek oturum açma modu** kutusunda **SAML tabanlı oturum açma**.
 
    !["Çoklu oturum açmayı" penceresi](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_samlbase.png)

3. Altında **Palo Alto Networks - yönetici UI etki alanı ve URL'leri**, aşağıdakileri yapın:

    !["Palo Alto Networks - yönetici UI etki alanı ve URL'ler" tek oturum açma bilgileri](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_show_advanced_url.png)
    
    a. İçinde **oturum açma URL'si** kutusuna bir URL aşağıdaki biçimde yazın: *https://\<müşteri güvenlik duvarı FQDN > /php/login.php*.

    b. İçinde **tanımlayıcısı** kutusuna bir URL aşağıdaki biçimde yazın: *https://\<müşteri güvenlik duvarı FQDN >: 443/SAML20/SP*.
    
    c. İçinde **yanıt URL'si** kutusuna onaylama tüketici Hizmeti'ni (ACS) URL aşağıdaki biçimde yazın: *https://\<müşteri güvenlik duvarı FQDN >: 443/SAML20/SP/ACS*.
    
    > [!NOTE] 
    > Yukarıdaki değerleri gerçek değildir. Bunları tanımlayıcısı ve gerçek oturum açma URL'si ile güncelleştirin. Değerleri almak için başvurun [Palo Alto Networks - yönetici UI istemci destek ekibi](https://support.paloaltonetworks.com/support). 
 
4. Palo Alto Networks - yönetici kullanıcı Arabirimi uygulaması SAML onaylar belirli bir biçimde beklediği talepler aşağıdaki görüntüde gösterildiği gibi yapılandırın. Öznitelik değerlerinin yönetmek **kullanıcı öznitelikleri** bölümünü **uygulama tümleştirmesi** aşağıdakileri yaparak sayfa:
    
    ![SAML belirteci öznitelikler listesi](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_attribute.png)
    
   > [!NOTE]
   > Öznitelik değerleri yalnızca örnek olduğundan, uygun değerleri için eşleme *kullanıcıadı* ve *adminrole*. Başka bir isteğe bağlı özniteliği yok *accessdomain*, Güvenlik Duvarı'nda belirli sanal sistemlere yönetici erişimi kısıtlamak için kullanılır.
   >
        
    | Öznitelik adı | Öznitelik değeri |
    | --- | --- |    
    | kullanıcı adı | User.userPrincipalName |
    | adminrole | customadmin |

    a. Seçin **Ekle özniteliği**.  
    
    !["Ekle özniteliği" düğmesi](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_attribute_04.png)

    **Özniteliği eklemek** penceresi açılır.

    !["Ekle özniteliği" penceresi](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_attribute_05.png)
    
    b. İçinde **adı** ilgili satır için gösterilen öznitelik adı yazın.
    
    c. İçinde **değeri** ilgili satır için gösterilen öznitelik değerini yazın.
    
    d. **Tamam**’ı seçin.

    > [!NOTE]
    > Öznitelikler hakkında daha fazla bilgi için aşağıdaki makalelere bakın:
    > * [Yönetici kullanıcı Arabirimi (adminrole) için Yönetici rolü profili](https://www.paloaltonetworks.com/documentation/80/pan-os/pan-os/firewall-administration/manage-firewall-administrators/configure-an-admin-role-profile)
    > * [Aygıt erişim etki alanı yönetici UI (accessdomain)](https://www.paloaltonetworks.com/documentation/80/pan-os/web-interface-help/device/device-access-domain)
    >

5. Altında **SAML imzalama sertifikası**seçin **meta veri XML**ve ardından **kaydetmek**.

    ![Meta veri XML indirme bağlantısı](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_certificate.png) 

    ![Kaydet düğmesi](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_400.png)

6. Palo Alto ağları güvenlik duvarı yönetim kullanıcı Arabirimi, yeni bir pencerede yönetici olarak açın.

7. Seçin **aygıt** sekmesi.

    ![Aygıt sekmesi](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_admin1.png)

8. Sol bölmede seçin **SAML kimlik sağlayıcısı**ve ardından **alma** meta veri dosya içeri aktarılamıyor.

    ![İçeri aktarma meta veri dosyası düğmesi](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_admin2.png)

9. İçinde **SAML tanımlamak sağlayıcısı sunucu Profil alma** penceresinde aşağıdakileri yapın:

    !["SAML tanımlamak sağlayıcısı sunucu Profil Al" penceresi](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_idp.png)

    a. İçinde **profil adı** kutusunda, bir ad sağlayın (örneğin, **Azuread'i yönetici UI**).
    
    b. Altında **kimlik sağlayıcısı meta verileri**seçin **Gözat**, daha önce Azure portalından indirdiğiniz metadata.xml dosyası seçin.
    
    c. Clear **kimlik sağlayıcısı sertifikası doğrulama** onay kutusu.
    
    d. **Tamam**’ı seçin.
    
    e. Güvenlik Duvarı'nda yapılandırmaları gerçekleştirmeyi seçin **tamamlama**.

10. Sol bölmede seçin **SAML kimlik sağlayıcısı**ve ardından SAML kimlik sağlayıcısı profilini seçin (örneğin, **Azuread'i yönetici UI**) önceki adımda oluşturduğunuz. 

    ![SAML kimlik sağlayıcısı profili](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_idp_select.png)

11. İçinde **SAML kimlik sağlayıcısı sunucu profilini** penceresinde aşağıdakileri yapın:

    !["SAML kimlik sağlayıcısı sunucu profilini" penceresi](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_slo.png)
  
    a. İçinde **kimlik sağlayıcısı SLO URL'si** kutusunda, önceden içe aktarılmış SLO URL aşağıdaki URL ile değiştirin: **https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0**.
  
    b. **Tamam**’ı seçin.

12. Palo Alto ağları güvenlik duvarının yönetim kullanıcı Arabirimi üzerindeki seçin **aygıt**ve ardından **yönetici rollerine**.

13. Seçin **Ekle** düğmesi. 

14. İçinde **Yönetici rolü profili** penceresi, **adı** kutusunda, Yönetici rolü için bir ad (örneğin, **fwadmin**).  
    Yönetici rolü adı kimlik sağlayıcısı tarafından gönderilen SAML Yönetici rolü öznitelik adı eşleşmelidir. Yönetici rolü adını ve değerini adım 4'te oluşturulmuştur.

    ![Palo Alto ağları yönetici rolünü yapılandırma](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_adminrole.png)
  
15. Güvenlik duvarının yönetim kullanıcı Arabirimi üzerindeki seçin **aygıt**ve ardından **kimlik doğrulama profili**.

16. Seçin **Ekle** düğmesi. 

17. İçinde **kimlik doğrulama profili** penceresinde aşağıdakileri yapın: 

    !["Kimlik doğrulama profili" penceresi](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_authentication_profile.png)

    a. İçinde **adı** kutusunda, bir ad sağlayın (örneğin, **AzureSAML_Admin_AuthProfile**).
    
    b. İçinde **türü** aşağı açılan listesinden, **SAML**. 
   
    c. İçinde **IDP sunucu profilini** aşağı açılan listesinde, uygun SAML kimlik sağlayıcısı sunucu profilini seçin (örneğin, **Azuread'i yönetici UI**).
   
    c. Seçin **etkinleştirmek tek oturum kapatma** onay kutusu.
    
    d. İçinde **Yönetici rolü özniteliği** kutusunda, öznitelik adını girin (örneğin, **adminrole**). 
    
    e. Seçin **Gelişmiş** sekmesini ve ardından, **izin listesi**seçin **Ekle**. 
    
    ![' Nin Gelişmiş sekmesinde Ekle düğmesi](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_allowlist.png)
    
    f. Seçin **tüm** onay kutusunu işaretleyin veya kullanıcıları ve bu profille doğrulanabilir grupları seçin.  
    Kullanıcı Kimliği doğruladığında, güvenlik duvarı ilişkili kullanıcı adı veya grubun bu listedeki girişleri karşı eşleşir. Girişleri eklemezseniz hiçbir kullanıcıların kimliklerini doğrulayabilirsiniz.

    g. **Tamam**’ı seçin.

18. SAML SSO Azure kullanarak yöneticiler etkinleştirmek için seçin **aygıt** > **Kurulum**. İçinde **Kurulum** bölmesinde, **Yönetim** sekmesini ve ardından, **kimlik doğrulama ayarlarını**seçin **ayarları** ("gear") düğmesine . 

 ![Ayarlar düğmesi](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_authsetup.png)

19. 17. adımda oluşturduğunuz SAML kimlik doğrulaması profilini seçin (örneğin, **AzureSAML_Admin_AuthProfile**).

 ![Kimlik doğrulama profili alanı](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_authsettings.png)

20. **Tamam**’ı seçin.

21. Yapılandırma kaydedilemedi seçin **tamamlama**.


> [!TIP]
> Uygulaması kuruluyor gibi önceki yönergeleri kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com). Uygulamada ekledikten sonra **Active Directory** > **kurumsal uygulamalar** bölümünde, select **çoklu oturum açma** sekmesini tıklatın ve ardından erişim belgelerde katıştırılmış **yapılandırma** alt bölüm. Embedded belgeler özelliği hakkında daha fazla bilgi için bkz: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, aşağıdakileri yaparak Azure portalında test kullanıcısı Britta Simon oluşturun:

![Bir Azure AD test kullanıcısı oluşturma][100]

1. Azure portalında sol bölmede seçin **Azure Active Directory**.

    ![Azure Active Directory bağlantısı](./media/active-directory-saas-paloaltoadmin-tutorial/create_aaduser_01.png)

2. Geçerli kullanıcıların bir listesini görüntülemek için seçin **kullanıcılar ve gruplar** > **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-paloaltoadmin-tutorial/create_aaduser_02.png)

3. Üstündeki **tüm kullanıcılar** penceresinde, seçin **Ekle**.

    ![Ekle düğmesi](./media/active-directory-saas-paloaltoadmin-tutorial/create_aaduser_03.png)
    
    **Kullanıcı** penceresi açılır.

4. İçinde **kullanıcı** penceresinde aşağıdakileri yapın:

    ![Kullanıcı penceresi](./media/active-directory-saas-paloaltoadmin-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’u seçin.
 
### <a name="create-a-palo-alto-networks---admin-ui-test-user"></a>Palo Alto Networks - yönetici UI test kullanıcısı oluşturma

Palo Alto Networks - yönetim kullanıcı Arabirimi yalnızca zaman kullanıcı sağlamayı destekler. Bir kullanıcı zaten yoksa, başarılı bir kimlik doğrulamasından sonra sistemde otomatik olarak oluşturulur. Hiçbir eylem sizden kullanıcı oluşturmak için gereklidir.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, kullanıcı Britta Palo Alto Networks - yönetici UI erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin. Bunu yapmak için aşağıdakileri yapın:

![Kullanıcı rolü atayın][200] 

1. Azure portalında açmak **uygulamaları** görünümü, Git **dizin** görüntülemek ve ardından **kurumsal uygulamalar** > **tüm uygulamaları**.

    !["Kurumsal uygulamalar" ve "Tüm uygulamaları" bağlantılar][201] 

2. İçinde **uygulamaları** listesinde **Palo Alto Networks - yönetici UI**.

    ![Palo Alto ağlar - Admin kullanıcı Arabirimi bağlantısı](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_app.png)  

3. Sol bölmede seçin **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Seçin **Ekle** , daha sonra **eklemek atama** bölmesinde, **kullanıcılar ve gruplar**.

    ![Ekleme atama bölmesi][203]

5. İçinde **kullanıcılar ve gruplar** penceresi, **kullanıcılar** listesinde **Britta Simon**.

6. Seçin **seçin** düğmesi.

7. İçinde **eklemek atama** penceresinde, seçin **atamak**.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test.

Palo Alto Networks - yönetici UI kutucuğu erişim Paneli'nde seçtiğinizde, otomatik olarak, Palo Alto Networks - yönetici kullanıcı Arabirimi uygulaması için oturum açmanız.

Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_203.png

