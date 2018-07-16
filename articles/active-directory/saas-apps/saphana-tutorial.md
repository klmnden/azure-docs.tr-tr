---
title: 'Öğretici: SAP HANA ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ile SAP HANA arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: cef4a146-f4b0-4e94-82de-f5227a4b462c
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: c466e811d868403c59d6615882422996442d792a
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39045836"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-hana"></a>Öğretici: SAP HANA ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile SAP HANA tümleştirme konusunda bilgi edinin.

SAP HANA Azure AD ile tümleştirdiğinizde, aşağıdaki avantajlardan yararlanabilirsiniz:

- SAP HANA erişimi, Azure AD'de kontrol edebilirsiniz.
- SAP HANA ile Azure AD hesaplarına oturumunuz otomatik olarak, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda--Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

SAP HANA ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Çoklu oturum etkin açma (SSO) bir SAP HANA abonelik
- Ortak bir Iaas üzerinde çalışan bir HANA örneği şirket içi, Azure VM veya Azure büyük örneklerde SAP
- HANA Studio HANA örneğinde yüklü yanı sıra XSA Yönetim web arabirimi

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamında SAP hana kullanarak önermiyoruz. Tümleşik geliştirme ya da hazırlık ortamı uygulamanın önce test ve üretim ortamına kullanın.

Bu öğreticideki adımları test etmek için aşağıdaki önerileri uygulayın:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- [Bir aylık ücretsiz deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/) Azure AD'ye deneme ortamı yoksa, Azure ad.

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide açıklanan senaryo iki temel yapı taşları oluşur:

1. SAP HANA galeri ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="add-sap-hana-from-the-gallery"></a>SAP HANA Galeriden Ekle
SAP HANA'ın Azure AD'ye tümleştirmesini yapılandırmak için SAP HANA Galeriden yönetilen SaaS uygulamaları listenize ekleyin.

**SAP HANA Galeriden eklemek için aşağıdaki adımları uygulayın:**

1. İçinde [Azure portalında](https://portal.azure.com), sol bölmede seçin **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Git **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni bir uygulama eklemek için seçin **yeni uygulama** iletişim kutusunun üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **SAP HANA**. Ardından **SAP HANA** sonuçları panelinden. Son olarak, seçin **Ekle** uygulama eklemek için Ekle düğmesine. 

    ![Yeni uygulama](./media/saphana-tutorial/tutorial_saphana_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı dayalı SAP HANA ile test etme

Tek iş için oturum açma için Azure AD SAP HANA karşılığı kullanıcının Azure AD'de bir kullanıcı için olan bilmesi gerekir. Diğer bir deyişle, SAP HANA'da bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı kurmak gerekir.

SAP HANA'da vermek **kullanıcıadı** aynı değeri değer **kullanıcı adı** Azure AD'de. Bu adım, iki kullanıcı arasında bağlantı kurar.

Yapılandırma ve Azure AD çoklu oturum açma SAP HANA ile test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. [Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on) kullanıcılarınız bu özelliği kullanmak etkinleştirmek için.
2. [Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user) Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. [SAP HANA test kullanıcısı oluşturma](#creating-a-sap-hana-test-user) SAP HANA, kullanıcının Azure AD gösterimini bağlı bir karşılığı Britta simon'un sağlamak için.
4. [Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user) Britta Simon, Azure AD çoklu oturum açma kullanmak üzere etkinleştirmek için.
5. [Çoklu oturum açmayı test](#testing-single-sign-on) yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve SAP HANA uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile SAP HANA yapılandırmak için aşağıdaki adımları uygulayın:**

1. Azure portalında, üzerinde **SAP HANA** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. İçinde **çoklu oturum açma** iletişim kutusunun **SAML tabanlı oturum açma**seçin **modu**.
 
    ![Çoklu oturum açma iletişim kutusu](./media/saphana-tutorial/tutorial_saphana_samlbase.png)

3. İçinde **SAP HANA etki alanı ve URL'ler** bölümünde, aşağıdaki adımları uygulayın:

    ![Etki alanı ve URL'ler tek oturum açma bilgileri](./media/saphana-tutorial/tutorial_saphana_url.png)

    a. İçinde **tanımlayıcı** kutusunda, aşağıdaki komutu yazın: `HA100` 

    b. İçinde **yanıt URL'si** kutusuna aşağıdaki desene sahip bir URL yazın: `https://<Customer-SAP-instance-url>/sap/hana/xs/saml/login.xscfunc`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısıyla güncelleştirin ve yanıt URL'si. İlgili kişi [SAP HANA istemci Destek ekibine](https://cloudplatform.sap.com/contact.html) bu değerleri almak için. 

4. İçinde **SAML imzalama sertifikası** bölümünden **meta veri XML**. Bilgisayarınızda meta verileri dosyayı kaydedin.

    ![Sertifika indirme bağlantısı](./media/saphana-tutorial/tutorial_saphana_certificate.png) 

    >[!Note]
    >Sertifika etkin değilse, ardından seçerek etkin hale getirin **yeni sertifikayı etkin hale getirin** Azure AD'de onay kutusu. 

5. SAP HANA uygulama belirli bir biçimde SAML onaylamalarını bekliyor. Aşağıdaki ekran görüntüsünde bu biçimin örneği gösterilmektedir. 

    Biz burada eşleştirdik **kullanıcı tanımlayıcısı** ile **ExtractMailPrefix()** işlevi **user.mail**. Bu benzersiz kullanıcı kimliğidir kullanıcının e-postanın bir ön ek değerini sağlar Bu kullanıcı kimliği, her başarılı yanıt SAP HANA uygulamaya gönderilir.

    ![Çoklu oturum açmayı yapılandırın](./media/saphana-tutorial/attribute.png)

6. İçinde **kullanıcı öznitelikleri** bölümünü **çoklu oturum açma** iletişim kutusunda, aşağıdaki adımları uygulayın:

    a. İçinde **kullanıcı tanımlayıcısı** aşağı açılan listesinden **ExtractMailPrefix**.
    
    b. İçinde **posta** aşağı açılan listesinden **user.mail**.

7. **Kaydet** düğmesini seçin.

    ![Çoklu oturum açma düğmesi kaydetme yapılandırın](./media/saphana-tutorial/tutorial_general_400.png)
    
8. Çoklu oturum açma SAP HANA tarafta yapılandırmak için oturum açın, **HANA XSA Web Konsolu** ilgili HTTPS uç noktasına giderek.

    > [!NOTE]
    > Varsayılan yapılandırmasında URL'sini, istek kimliği doğrulanmış bir SAP HANA veritabanı kullanıcının kimlik bilgileri gerektiren bir oturum açma ekranına yönlendirir. Kullanıcı oturum açtığında SAML yönetim görevlerini gerçekleştirmek için izinleri olmalıdır.

9. XSA Web arabiriminde Git **SAML kimlik sağlayıcısı**. Buradan seçin **+** düğmesini görüntülemek için ekranın alt kısmındaki **kimlik sağlayıcı bilgileri ekleme** bölmesi. Ardından aşağıdaki adımları uygulayın:

    ![Kimlik sağlayıcısı Ekle](./media/saphana-tutorial/sap1.png)

    a. İçinde **kimlik sağlayıcı bilgileri ekleyin** bölmesi (Bu, Azure portalından indirdiğiniz) meta verileri XML içeriğini yapıştırın **meta verileri** kutusu.

    ![Kimlik sağlayıcı ayarları Ekle](./media/saphana-tutorial/sap2.png)

    b. XML belgesinin içeriğini geçerliyse, ayrıştırma işlemi için gerekli olan bilgileri ayıklayan **konu, varlık kimliği ve sertifikayı veren** alanlarını **genel veri** ekran alanı. Ayrıca, URL alanları için gereken bilgileri ayıklar **hedef** ekran alanı, örneğin, **temel URL ve SingleSignOn URL (*)** alanları.

    ![Kimlik sağlayıcı ayarları Ekle](./media/saphana-tutorial/sap3.png)

    c. İçinde **adı** kutusunun **genel veri** ekran alanı, yeni SAML SSO kimlik sağlayıcısı için bir ad girin.

    > [!NOTE]
    > SAML IDP adı zorunludur ve benzersiz olmalıdır. Kullanılacak SAP HANA XS uygulamalar için kimlik doğrulama yöntemi olarak SAML seçeneğini belirlediğinizde görüntülenen kullanılabilir SAML Idp'yi listesinde görünür. Örneğin, bunu yapabilirsiniz **kimlik doğrulaması** ekran alanı XS Yapıt yönetim aracı.

10. Seçin **Kaydet** SAML kimlik sağlayıcısı ile ilgili ayrıntıları kaydetmeyi ve yeni SAML IDP bilinen SAML IDP listesine eklemek için.

    ![Kaydet düğmesi](./media/saphana-tutorial/sap4.png)

11. Sistem Özellikleri içinde HANA Studio **yapılandırma** sekmesinde, filtre tarafından ayarları **saml**. Ardından ayarlamak **assertion_timeout** gelen **10 sn** için **120 saniye**.

    ![assertion_timeout ayarı](./media/saphana-tutorial/sap7.png)

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com) uygulamasını ayarladığınız sırada! Bu uygulamadan ekledikten sonra **Active Directory** > **kurumsal uygulamalar** bölümünden **çoklu oturum açma** sekme ve katıştırılmış erişin belgelerin **yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği hakkında [katıştırılmış belgeleri Azure AD](https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları uygulayın:**

1. İçinde **Azure portalında**, sol bölmede seçin **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](./media/saphana-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**. Ardından **tüm kullanıcılar**.
    
    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/saphana-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda **Ekle** iletişim kutusunun üst.
 
    ![Ekle düğmesi](./media/saphana-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları uygulayın:
 
    ![Kullanıcı iletişim kutusu](./media/saphana-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola**ve ardından parolayı not alın.

    d. **Oluştur**’u seçin.
 
### <a name="create-a-sap-hana-test-user"></a>SAP HANA test kullanıcısı oluşturma

SAP HANA için oturum açmak Azure AD kullanıcılarının etkinleştirmek için SAP HANA'da hazırlamanız gerekir.
SAP HANA tam zamanında sağlama, varsayılan olarak etkin olduğu destekler.

Bir kullanıcı el ile oluşturmanız gerekiyorsa, aşağıdaki adımları uygulayın:

>[!NOTE]
>Kullanıcının kullandığı Dış kimlik doğrulama değiştirebilirsiniz. Kerberos gibi bir dış sistemle doğrulayabilir. Dış kimlikler hakkında ayrıntılı bilgi için kişi, [etki alanı yöneticisi](https://cloudplatform.sap.com/contact.html).

1. Açık [SAP HANA Studio](https://help.sap.com/viewer/a2a49126a5c546a9864aae22c05c3d0e/2.0.01/en-us) bir Yöneticiyseniz ve ardından etkinleştir DB kullanıcı SAML SSO olarak.

    ![Kullanıcı oluştur](./media/saphana-tutorial/sap5.png)

2. Görünmeyen sol tarafındaki onay kutusunu işaretleyin **SAML**ve ardından **yapılandırma** bağlantı.

3. Seçin **Ekle** SAML IDP eklemek için.  Uygun SAML IDP seçmek ve ardından **Tamam**.

4. Ekleme **Dış kimlik** (Bu durumda, BrittaSimon) ya da seçin **herhangi**. Sonra **Tamam**’ı seçin.

    >[!Note]
    >Varsa **herhangi** onay kutusu seçilmez ve HANA kullanıcı adı UPN etki alanı soneki önce kullanıcı adı tam olarak eşleşmesi gerekir. (Örneğin, BrittaSimon@contoso.com hana BrittaSimon olur.)

5. Test amacıyla, tüm Ata **XS** kullanıcı rolleri.

    ![Rol atama](./media/saphana-tutorial/sap6.png)

    > [!TIP]
    > Yalnızca, kullanım durumları için uygun izinleri vermeniz gerekir.

6. Kullanıcı kaydedin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, SAP HANA için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**SAP HANA Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümünü açın. Dizin görünümüne gidin ve Git **kurumsal uygulamalar**. Seçin **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **SAP HANA**.

    ![Kullanıcı Ata](./media/saphana-tutorial/tutorial_saphana_app.png) 

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202] 

4. Seçin **Ekle** düğmesi. İçinde **atama Ekle** iletişim kutusunda **kullanıcılar ve gruplar**.

    ![Atama Ekle bölmesi][203]

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** içinde **kullanıcılar** listesi.

6. Tıklayın **seçin** düğmesine **kullanıcılar ve gruplar** iletişim kutusu.

7. Seçin **atama** düğmesine **atama Ekle** iletişim kutusu.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test.

Erişim Paneli'nde SAP HANA kutucuğu seçtiğinizde, otomatik olarak imzalanmış SAP HANA uygulamanıza.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/saphana-tutorial/tutorial_general_01.png
[2]: ./media/saphana-tutorial/tutorial_general_02.png
[3]: ./media/saphana-tutorial/tutorial_general_03.png
[4]: ./media/saphana-tutorial/tutorial_general_04.png

[100]: ./media/saphana-tutorial/tutorial_general_100.png

[200]: ./media/saphana-tutorial/tutorial_general_200.png
[201]: ./media/saphana-tutorial/tutorial_general_201.png
[202]: ./media/saphana-tutorial/tutorial_general_202.png
[203]: ./media/saphana-tutorial/tutorial_general_203.png

