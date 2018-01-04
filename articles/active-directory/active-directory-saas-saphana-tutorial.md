---
title: "Öğretici: SAP HANA Azure Active Directory Tümleştirme | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ve SAP HANA arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: cef4a146-f4b0-4e94-82de-f5227a4b462c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: 855525e2c1d3c33cc7134bbc1cd9b53ca59e1a70
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-hana"></a>Öğretici: SAP HANA Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile SAP HANA tümleştirmek öğrenin.

SAP HANA Azure AD ile tümleştirdiğinizde, aşağıdaki faydaları alın:

- SAP HANA erişimi, Azure AD'de kontrol edebilirsiniz.
- SAP HANA Azure AD hesaplarına otomatik olarak imzalanmış, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda--Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz: [uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

SAP HANA ile Azure AD tümleştirme yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Çoklu oturum etkin açma (SSO) olan bir SAP HANA abonelik
- Ortak bir Iaas üzerinde çalışan bir HANA örneği şirket içi, Azure VM veya Azure SAP büyük örnekleri
- HANA HANA örnek üzerinde yüklü Studio yanı sıra XSA Yönetim web arabirimi

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamında, SAP HANA kullanarak önermiyoruz. Geliştirme veya uygulamanın hazırlama ortamında tümleştirme önce test ve üretim ortamına kullanın.

Bu öğreticide adımları test etmek için aşağıdaki önerileri uygulayın:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- [Bir aylık ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/) bir Azure AD deneme ortam zaten yoksa, Azure ad.

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. SAP HANA Galeriden ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="add-sap-hana-from-the-gallery"></a>SAP HANA Galerisi'nden ekleme
SAP HANA tümleştirme Azure AD'ye yapılandırmak için SAP HANA Galeriden yönetilen SaaS uygulamaları listenize ekleyin.

**SAP HANA Galeriden eklemek için aşağıdaki adımları uygulayın:**

1. İçinde [Azure portal](https://portal.azure.com), sol bölmede seçin **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Git **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni bir uygulama eklemek için seçin **yeni uygulama** iletişim kutusunun üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **SAP HANA**. Ardından **SAP HANA** sonuçları panelinden. Son olarak, seçin **Ekle** uygulama eklemek için düğmesi. 

    ![Yeni uygulama](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı SAP HANA ile test etme

Tekli çalışmaya oturum için Azure AD için bir kullanıcı Azure AD'de SAP HANA karşılık gelen kullanıcı olan bilmek ister. Diğer bir deyişle, SAP HANA bir Azure AD kullanıcısının ve ilgili kullanıcı arasında bir bağlantı oluşturmanız gerekir.

SAP HANA içinde vermek **kullanıcıadı** aynı değerini değer **kullanıcı adı** Azure AD'de. Bu adım, iki kullanıcı arasındaki bağlantı kurar.

Yapılandırma ve Azure AD çoklu oturum açma SAP HANA ile test etmek için aşağıdaki yapı taşları tamamlayın:

1. [Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on) bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. [Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user) Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. [SAP HANA test kullanıcısı oluşturma](#creating-a-sap-hana-test-user) karşılık gelen Britta Simon, kullanıcının Azure AD gösterimini bağlı SAP HANA sağlamak için.
4. [Azure AD test kullanıcısı atayın](#assigning-the-azure-ad-test-user) Britta Azure AD çoklu oturum açma kullanmak Simon etkinleştirmek için.
5. [Test çoklu oturum açma](#testing-single-sign-on) yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma, SAP HANA uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile SAP HANA yapılandırmak için aşağıdaki adımları uygulayın:**

1. Azure portalında üzerinde **SAP HANA** uygulama tümleştirmesi sayfasında, **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. İçinde **çoklu oturum açma** iletişim kutusunda **SAML tabanlı oturum açma**seçin **modu**.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_samlbase.png)

3. İçinde **SAP HANA etki alanı ve URL'leri** bölümünde, aşağıdaki adımları uygulayın:

    ![Etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_url.png)

    a. İçinde **tanımlayıcısı** kutusunda, aşağıdaki komutu yazın:`HA100` 

    b. İçinde **yanıt URL'si** kutusunda, aşağıdaki desende bir URL yazın:`https://<Customer-SAP-instance-url>/sap/hana/xs/saml/login.xscfunc`

    > [!NOTE] 
    > Bu değerleri gerçek değil. Gerçek tanımlayıcısı ile bu değerleri güncelleştirmek ve URL yanıt. Kişi [SAP HANA istemci destek ekibi](https://cloudplatform.sap.com/contact.html) bu değerleri almak için. 

4. İçinde **SAML imzalama sertifikası** bölümünde, select **meta veri XML**. Ardından meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_certificate.png) 

    >[!Note]
    >Sertifika etkin değilse, ardından seçerek etkin duruma getirmek **yeni sertifika etkin hale getirin** onay kutusuna Azure AD. 

5. SAP HANA uygulaması SAML onaylar belirli bir biçimde bekliyor. Aşağıdaki ekran görüntüsü, bu biçimi örneği gösterir. 

    Burada size eşlenen **kullanıcı tanımlayıcısı** ile **ExtractMailPrefix()** işlevinin **user.mail**. Bu benzersiz kullanıcı kimliğidir kullanıcının e-postanın önek değeri verir Bu kullanıcı kimliği, her başarılı yanıt SAP HANA uygulamaya gönderilir.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-saphana-tutorial/attribute.png)

6. İçinde **kullanıcı öznitelikleri** bölümünü **çoklu oturum açma** iletişim kutusunda, aşağıdaki adımları uygulayın:

    a. İçinde **kullanıcı tanımlayıcısı** aşağı açılan listesinden, **ExtractMailPrefix**.
    
    b. İçinde **posta** aşağı açılan listesinden, **user.mail**.

7. **Kaydet** düğmesini seçin.

    ![Çoklu oturum açma düğmesi kaydetme yapılandırın](./media/active-directory-saas-saphana-tutorial/tutorial_general_400.png)
    
8. Çoklu oturum açma SAP HANA tarafında yapılandırmak için oturum açın, **HANA XSA Web Konsolu** ilgili HTTPS uç noktasına giderek.

    > [!NOTE]
    > Varsayılan yapılandırmada istek kimliği doğrulanmış bir SAP HANA veritabanına kullanıcı kimlik bilgileri gerektiren bir oturum açma ekranına yeniden yönlendirildiği URL. Oturum açtığında kullanıcının SAML yönetim görevlerini gerçekleştirmek için izinleri olmalıdır.

9. XSA Web arabirimi Git **SAML kimlik sağlayıcısı**. Buradan, seçin  **+**  görüntülemek için ekranın düğmesinde **kimlik sağlayıcısı bilgisi Ekle** bölmesi. Ardından aşağıdaki adımları uygulayın:

    ![Kimlik sağlayıcısı ekleyin](./media/active-directory-saas-saphana-tutorial/sap1.png)

    a. İçinde **kimlik sağlayıcısı bilgisi Ekle** bölmesinde (olan Azure portalından indirdiğiniz) meta veri XML içeriğini yapıştırın **meta veri** kutusu.

    ![Kimlik sağlayıcı ayarları ekleme](./media/active-directory-saas-saphana-tutorial/sap2.png)

    b. XML belgesinin içeriğini geçerliyse, ayrıştırma işlemi için gerekli olan bilgileri ayıklar **konu, varlık kimliği ve sertifikayı veren** alanlarını **genel veri** ekran alanı. Ayrıca URL alanları için gerekli olan bilgileri ayıklar **hedef** ekran alanı, örneğin,  **taban URL ve SingleSignOn URL (*)** alanlar.

    ![Kimlik sağlayıcı ayarları ekleme](./media/active-directory-saas-saphana-tutorial/sap3.png)

    c. İçinde **adı** kutusunun **genel veri** ekran alanı, yeni SAML SSO kimlik sağlayıcısı için bir ad girin.

    > [!NOTE]
    > SAML IDP adı zorunludur ve benzersiz olması gerekir. SAP HANA XS uygulamaları kullanmak kimlik doğrulama yöntemi olarak SAML seçtiğinizde görüntülenen kullanılabilir SAML IDPs listesinde görünür. Örneğin, bunu yapabilirsiniz **kimlik doğrulaması** ekran alanı XS yapı yönetim aracı.

10. Seçin **kaydetmek** SAML kimlik sağlayıcısı ayrıntılarını kaydetmek ve yeni SAML IDP bilinen SAML IDPs listesine eklemek için.

    ![Kaydet düğmesi](./media/active-directory-saas-saphana-tutorial/sap4.png)

11. Sistem özelliklerini içinde HANA Studio'da **yapılandırma** sekmesinde, filtre tarafından ayarları **saml**. Daha sonra ayarlamak **assertion_timeout** gelen **10 sn** için **120 saniye**.

    ![assertion_timeout ayarı](./media/active-directory-saas-saphana-tutorial/sap7.png)

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com) uygulaması kuruluyor sırada! Bu uygulamadan ekledikten sonra **Active Directory** > **kurumsal uygulamalar** bölümünde, select **çoklu oturum açma** sekmesinde ve katıştırılmış erişim belgeleri etraflıca **yapılandırma** alt bölüm. Daha fazla bilgiyi embedded belgeler özelliği hakkında [Azure AD embedded belgeler](https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD kullanıcı oluştur][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları uygulayın:**

1. İçinde **Azure portal**, sol bölmede seçin **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-saphana-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**. Ardından **tüm kullanıcılar**.
    
    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-saphana-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda **Ekle** iletişim kutusunun üstündeki.
 
    ![Ekle düğmesi](./media/active-directory-saas-saphana-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları uygulayın:
 
    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-saphana-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola**ve ardından parolayı not alın.

    d. **Oluştur**’u seçin.
 
### <a name="create-a-sap-hana-test-user"></a>SAP HANA test kullanıcısı oluşturma

SAP HANA oturum açmak Azure AD kullanıcıları etkinleştirmek için SAP HANA hazırlamanız gerekir.
SAP HANA tam zamanı sağlama, varsayılan olarak etkin olduğu destekler.

Bir kullanıcı el ile oluşturmanız gerekiyorsa, aşağıdaki adımları uygulayın:

>[!NOTE]
>Kullanıcının kullandığı Dış kimlik doğrulama değiştirebilirsiniz. Kerberos gibi bir dış sistem ile doğrulanabilir. Dış kimlikler hakkında ayrıntılı bilgi için kişi, [etki alanı yöneticisi](https://cloudplatform.sap.com/contact.html).

1. Açık [SAP HANA Studio](https://help.sap.com/viewer/a2a49126a5c546a9864aae22c05c3d0e/2.0.01/en-us) bir Yöneticiyseniz ve ardından etkinleştir DB kullanıcı SAML SSO olarak.

    ![Kullanıcı oluştur](./media/active-directory-saas-saphana-tutorial/sap5.png)

2. Görünmeyen sol tarafındaki onay kutusunu işaretleyin **SAML**ve ardından **yapılandırma** bağlantı.

3. Seçin **Ekle** SAML IDP eklemek için.  Uygun SAML IDP seçin ve ardından **Tamam**.

4. Ekleme **Dış kimlik** (Bu durumda, BrittaSimon) veya seçin **herhangi**. Ardından **Tamam**.

    >[!Note]
    >Varsa **herhangi** onay kutusu seçilmez sonra HANA kullanıcı adı tam etki alanı soneki önce UPN kullanıcı adı ile eşleşmesi gerekir. (Örneğin, BrittaSimon@contoso.com HANA BrittaSimon olur.)

5. Test amacıyla, tüm Ata **XS** kullanıcı rolleri.

    ![rol atama](./media/active-directory-saas-saphana-tutorial/sap6.png)

    > [!TIP]
    > Yalnızca, kullanım durumları için uygun izinleri vermeniz gerekir.

6. Kullanıcı kaydedin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, SAP HANA erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**SAP HANA Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın. Dizin görünümüne gidin ve Git **kurumsal uygulamalar**. Seçin **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **SAP HANA**.

    ![Kullanıcı atama](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_app.png) 

3. Soldaki menüde seçin **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202] 

4. Seçin **Ekle** düğmesi. İçinde **eklemek atama** iletişim kutusunda **kullanıcılar ve gruplar**.

    ![Ekleme atama bölmesi][203]

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** içinde **kullanıcılar** listesi.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim kutusu.

7. Seçin **atamak** düğmesini **eklemek atama** iletişim kutusu.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test.

Erişim panelinde SAP HANA döşeme seçtiğinizde otomatik olarak imzalanmış SAP HANA uygulamanıza.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_203.png

