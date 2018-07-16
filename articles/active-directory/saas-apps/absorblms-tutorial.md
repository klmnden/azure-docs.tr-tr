---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle devralarak LMS | Microsoft Docs'
description: Azure Active Directory ve LMS devralarak arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: ba9f1b3d-a4a0-4ff7-b0e7-428e0ed92142
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: jeedes
ms.openlocfilehash: 066ae92056e4b80b6627b371d6ebeb3235b2781d
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39043787"
---
# <a name="tutorial-azure-active-directory-integration-with-absorb-lms"></a>Öğretici: Azure Active Directory devralarak LMS ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile devralarak LMS tümleştirme konusunda bilgi edinin.

Azure AD ile devralarak LMS tümleştirme ile aşağıdaki avantajları sağlar:

- Devralarak LMS erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak devralarak LMS (aracılığıyla çoklu oturum açma) ile Azure AD hesaplarına oturum açmak, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda, Azure portalında yönetebilir.

Azure AD ile bir hizmet (SaaS) uygulamasını tümleştirme olarak yazılım hakkında daha fazla bilgi edinmek istiyorsanız bkz [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile devralarak LMS yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik bir LMS devralarak çoklu oturum açma etkin

> [!NOTE]
> Bir üretim ortamında, Bu öğretici için kullanılmaması önerilir.

Bu öğreticideki adımları test etmek için aşağıdaki önerileri uygulayın:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

* Galeriden devralarak LMS ekleme
* Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="add-absorb-lms-from-the-gallery"></a>Devralarak LMS Galeriden Ekle
Azure AD'de devralarak LMS tümleştirmesini yapılandırmak için devralarak LMS Galeriden yönetilen SaaS uygulamaları listenize ekleyin.

Galeriden devralarak LMS eklemek için aşağıdakileri yapın:

1. İçinde [Azure portalında](https://portal.azure.com), sol bölmede seçin **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Git **kurumsal uygulamalar** > **tüm uygulamaları**.

    ![Kurumsal uygulamalar bölmesi][2]
    
3. Bir uygulama eklemek için seçin **yeni uygulama** düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **devralarak LMS**seçin **devralarak LMS** sonuç paneli ve ardından **Ekle** düğmesi.

    ![Sonuçlar listesinde LMS Al](./media/absorblms-tutorial/tutorial_absorblms_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma Britta Simon adlı bir test kullanıcı tabanlı LMS devralarak sınayın.

Tek iş için oturum açma için Azure AD devralarak LMS karşılığı kullanıcının Azure AD'de ne olduğunu bilmeniz gerekir. Diğer bir deyişle, bir kullanıcının Azure AD'de devralarak LMS karşılık gelen kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Atayarak bu bağlantı ilişki kurmak *kullanıcı adı* değeri Azure AD'de *kullanıcıadı* devralarak LMS değeri.

Yapılandırma ve Azure AD çoklu oturum açma devralarak LMS ile test etmek için sonraki beş bölümlerde yapı taşlarını tamamlayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve LMS devralarak uygulamanızda çoklu oturum açmayı yapılandırın.

Azure AD çoklu oturum açma devralarak LMS ile yapılandırmak için aşağıdakileri yapın:

1. Azure portalında, üzerinde **devralarak LMS** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. İçinde **çoklu oturum açma** iletişim kutusundaki **modu** kutusunda **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/absorblms-tutorial/tutorial_absorblms_samlbase.png)

3. İçinde **devralarak LMS etki alanı ve URL'ler** bölümünde, aşağıdakileri yapın:

    ![LMS etki alanı ve URL'ler tek oturum açma bilgilerini al](./media/absorblms-tutorial/tutorial_absorblms_url.png)

    a. İçinde **tanımlayıcı** aşağıdaki söz dizimini kullanan bir URL yazın: `https://<subdomain>.myabsorb.com/Account/SAML`.

    b. İçinde **yanıt URL'si** aşağıdaki söz dizimini kullanan bir URL yazın: `https://<subdomain>.myabsorb.com/Account/SAML`.
     
    > [!NOTE] 
    > Bu URL'ler, gerçek değerleri değildir. Bunları, gerçek tanımlayıcısı ve yanıt URL'lerini güncelleştirin. Bu değerleri almak için iletişime geçin [devralarak LMS istemci Destek ekibine](https://www.absorblms.com/support). 

4. İçinde **SAML imzalama sertifikası** bölümünde **indirme** sütunundaki **meta veri XML**ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![İmzalama sertifikası indirme bağlantısı](./media/absorblms-tutorial/tutorial_absorblms_certificate.png) 

5. **Kaydet**’i seçin.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/absorblms-tutorial/tutorial_general_400.png)
    
6. İçinde **devralarak LMS yapılandırma** bölümünden **devralarak LMS yapılandırma** açmak için **yapılandırma oturum açma** penceresi ve ardından kopyalama **oturumkapatmaURL'si** içinde **hızlı başvuru bölümü.**

    ![Devralarak LMS yapılandırma bölmesi](./media/absorblms-tutorial/tutorial_absorblms_configure.png) 

7. Yeni bir web tarayıcısı penceresinde devralarak LMS şirketinizin sitesi için bir yönetici olarak oturum açın.

8. Seçin **hesabı** sağ üst köşedeki düğmesi. 

    ![Hesap Ekle düğmesine](./media/absorblms-tutorial/1.png)

9. Hesap bölmesinde seçin **Portal ayarları**.

    ![Portal ayarları bağlantısı](./media/absorblms-tutorial/2.png)
    
10. **Users (Kullanıcılar)** sekmesini seçin.

    ![Kullanıcılar sekmesine](./media/absorblms-tutorial/3.png)

11. Çoklu oturum açma Yapılandırması sayfasında, aşağıdakileri yapın:

    ![Çoklu oturum açma Yapılandırması sayfası](./media/absorblms-tutorial/4.png)

    a. İçinde **modu** kutusunda **kimlik sağlayıcısı tarafından başlatılan**.

    b. Azure portalından indirdiğiniz sertifika Not Defteri'nde açın. Kaldırma **---BEGIN CERTIFICATE---** ve **---END CERTIFICATE---** etiketler. Ardından **anahtar** kutusunda, kalan içeriği yapıştırın.
    
    c. İçinde **ID özelliği** kutusunda, Azure AD'de kullanıcı tanımlayıcısı yapılandırdığınız öznitelik seçin. Örneğin, varsa *userPrincipalName* Azure AD'de seçin seçili **Username**.

    d. İçinde **oturum açma URL'si** kutusu, yapıştırma **kullanıcı erişim URL'SİNDEN** uygulamanın gelen **özellikleri** Azure Portalı'nın sayfasında.

    e. İçinde **oturum kapatma URL'si**, yapıştırın **oturum kapatma URL'si** kopyalama değeri **yapılandırma oturum açma** Azure portal'ın penceresi.

12. İki durumlu **yalnızca SSO oturum açma izin** için **üzerinde**.

    ![Yalnızca izin SSO Oturum Aç/Kapat](./media/absorblms-tutorial/5.png)

13. Seçin **kaydedin.**

> [!TIP]
> Bu yönergelerde kısa bir sürümünü edinebilirsiniz [Azure portalında](https://portal.azure.com) uygulamasını ayarladığınız sırada. Uygulamadan ekledikten sonra **Active Directory** > **kurumsal uygulamalar** bölümünden **çoklu oturum açma** sekme ve katıştırılmış erişin belgelerin **yapılandırma** alttaki bölümü. Daha fazla bilgi için [Azure AD belgeleri katıştırılmış]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında, Britta Simon test kullanıcısı oluşturun.

![Bir Azure AD test kullanıcısı oluşturma][100]

Azure AD'de bir test kullanıcısı oluşturmak için aşağıdakileri yapın:

1. Azure portalında, sol bölmede seçin **Azure Active Directory**.

    ![Azure Active Directory düğmesi](./media/absorblms-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için seçin **kullanıcılar ve gruplar** > **tüm kullanıcılar**.
    
    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/absorblms-tutorial/create_aaduser_02.png) 

3. İletişim kutusunun en üstünde seçin **Ekle**.
 
    ![Ekle düğmesi](./media/absorblms-tutorial/create_aaduser_03.png) 

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdakileri yapın:
 
    ![Kullanıcı iletişim kutusu](./media/absorblms-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından değeri Not **parola** kutusu.

    d. **Oluştur**’u seçin.

### <a name="create-an-absorb-lms-test-user"></a>Bir LMS devralarak test kullanıcısı oluşturma

Azure AD kullanıcılarının LMS etkisini azaltmak için oturum açmak bunlar devralarak LMS ayarlanması gerekir.  

LMS etkisini azaltmak için Kurulum el ile gerçekleştirilen bir görevdir.

Bir kullanıcı hesabı ayarlamamız için aşağıdakileri yapın:

1. Devralarak LMS şirketinizin sitesi için bir yönetici olarak oturum açın.

2. Sol bölmede seçin **kullanıcılar**.

    ![Devralarak LMS kullanıcıları bağlantısı](./media/absorblms-tutorial/absorblms_users.png)

3. İçinde **kullanıcılar** bölmesinde **kullanıcılar**.

    ![Kullanıcıları bağlantısı](./media/absorblms-tutorial/absorblms_userssub.png)

4. İçinde **yeni Ekle** aşağı açılan listesinden **kullanıcı**.

    ![Yeni Ekle aşağı açılan listesi](./media/absorblms-tutorial/absorblms_createuser.png)

5. Üzerinde **Kullanıcı Ekle** sayfasında, aşağıdakileri yapın:

    ![Kullanıcı Ekle sayfası](./media/absorblms-tutorial/user.png)

    a. İçinde **ad** ad gibi yazın **Britta**.

    b. İçinde **Soyadı** Soyadı gibi yazın **Simon**.
    
    c. İçinde **kullanıcıadı** gibi tam bir ad yazın **Britta Simon**.

    d. İçinde **parola** Britta Simon'ın parola yazın.

    e. İçinde **parolayı onayla** kutusuna, parolayı yeniden yazın.
    
    f. Ayarlama **etkindir** geç **etkin**.  

6. Seçin **kaydedin.**
 
### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, kullanıcı LMS etkisini azaltmak için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200]

Kullanıcı Britta Simon LMS etkisini azaltmak için atamak için aşağıdakileri yapın:

1. Azure portalında uygulama görünümünü açın, dizin görünümüne gidin ve ardından **kurumsal uygulamalar** > **tüm uygulamaları**.

    !["Tüm uygulamalar" bağlantısı][201] 

2. İçinde **uygulamaları** listesinden **devralarak LMS**.

    ![Uygulamalar listesinde devralarak LMS bağlantı](./media/absorblms-tutorial/tutorial_absorblms_app.png) 

3. Sol bölmede seçin **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202] 

4. Seçin **Ekle** ve daha sonra **atama Ekle** bölmesinde **kullanıcılar ve gruplar**.

    ![Atama Ekle bölmesi][203]

5. İçinde **kullanıcılar ve gruplar** iletişim kutusundaki **kullanıcılar** listesinden **Britta Simon**.

6. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **seçin** düğmesi.

7. İçinde **atama Ekle** iletişim kutusunda **atama** düğmesi.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test.

Erişim Paneli'nde seçerek **devralarak LMS** kutucuk otomatik olarak açarsa, devralarak LMS uygulamanıza. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/absorblms-tutorial/tutorial_general_01.png
[2]: ./media/absorblms-tutorial/tutorial_general_02.png
[3]: ./media/absorblms-tutorial/tutorial_general_03.png
[4]: ./media/absorblms-tutorial/tutorial_general_04.png

[100]: ./media/absorblms-tutorial/tutorial_general_100.png

[200]: ./media/absorblms-tutorial/tutorial_general_200.png
[201]: ./media/absorblms-tutorial/tutorial_general_201.png
[202]: ./media/absorblms-tutorial/tutorial_general_202.png
[203]: ./media/absorblms-tutorial/tutorial_general_203.png
