---
title: "Öğretici: SAP HANA Azure Active Directory Tümleştirme | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ve SAP HANA arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: cef4a146-f4b0-4e94-82de-f5227a4b462c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: a7e73f6ee763d1005ad85935cf2d8f6b24ecf116
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-hana"></a>Öğretici: SAP HANA Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile SAP HANA tümleştirmek öğrenin.

SAP HANA Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- SAP HANA erişimi, Azure AD'de kontrol edebilirsiniz
- Azure AD hesaplarına otomatik olarak (çoklu oturum açma) için SAP HANA açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

SAP HANA ile Azure AD tümleştirme yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir SAP HANA çoklu oturum açma abonelik etkin
- Çalışan HANA örneği üzerinde herhangi bir ortak Iaas, şirket içi ya da Azure VM'ler veya Azure SAP büyük örnekleri
- XSA Yönetim Web arabirimi yanı sıra HANA Studio HANA örnek üzerinde yüklü.

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamında, SAP HANA kullanarak önermiyoruz. Geliştirme veya uygulamanın hazırlama ortamında tümleştirme önce test ve üretim ortamına kullanın.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. SAP HANA Galeriden ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-sap-hana-from-the-gallery"></a>SAP HANA Galeriden ekleme
SAP HANA tümleştirme Azure AD'ye yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden SAP HANA eklemeniz gerekir.

**SAP HANA Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **SAP HANA**seçin **SAP HANA** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi. 

    ![Yeni uygulama](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı SAP HANA ile test etme

Tekli çalışmaya oturum için Azure AD SAP HANA karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının SAP HANA ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

SAP HANA içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma SAP HANA ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[SAP HANA test kullanıcısı oluşturma](#creating-a-sap-hana-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı SAP HANA sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma, SAP HANA uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile SAP HANA yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **SAP HANA** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_samlbase.png)

3. Üzerinde **SAP HANA etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, türü olarak:`HA100` 

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<Customer-SAP-instance-url>/sap/hana/xs/saml/login.xscfunc`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı ve yanıt URL'si ile güncelleştirin. Kişi [SAP HANA istemci destek ekibi](https://cloudplatform.sap.com/contact.html) bu değerleri almak için. 

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_certificate.png) 

    >[!Note]
    >Sertifika etkin değilse, ardından onu Azure AD içinde "Yeni sertifikayı etkinleştirmek" onay kutusunu tıklatarak etkin hale getirin. 

5. SAP HANA uygulaması SAML onaylar belirli bir biçimde bekliyor. Aşağıdaki ekran görüntüsünde bunun bir örneği gösterir. Biz burada eşledikten **kullanıcı tanımlayıcısı** ile **ExtractMailPrefix()** işlevinin **user.mail**. Bu e-posta benzersiz kullanıcı kimliği olan kullanıcının önek değeri verir Bu, her başarılı yanıt SAP HANA uygulamaya gönderilir.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-saphana-tutorial/attribute.png)

6. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim:

    a. İçinde **kullanıcı tanımlayıcısı** açılır listesinden, **ExtractMailPrefix**.
    
    b. İçinde **posta** açılır listesinden, **user.mail**.

7. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-saphana-tutorial/tutorial_general_400.png)
    
8. Çoklu oturum açma yapılandırmak için **SAP HANA** tarafı, oturum açma için sizin **HANA XSA Web Konsolu** ilgili HTTPS uç noktasına giderek.

    > [!Note]
    > Varsayılan yapılandırmasında, istek oturum açma işlemini tamamlamak için kimliği doğrulanmış bir SAP HANA veritabanına kullanıcı kimlik bilgileri gerektiren bir oturum açma ekranına yeniden yönlendirildiği URL. Oturum açan kullanıcının SAML yönetim görevlerini gerçekleştirmek için gereken izinleri olmalıdır.

9. XSA Web arabirimi gidin **SAML kimlik sağlayıcısı** buradan tıklatıp **"+"** -kimlik sağlayıcısı bilgisi Ekle bölmesinde görüntülemek ve aşağıdakileri yapmak için ekranın düğmesine adımlar:

    ![Kimlik sağlayıcısı ekleyin](./media/active-directory-saas-saphana-tutorial/sap1.png)

    a. İçinde **kimlik sağlayıcısı bilgisi Ekle** bölmesinde, Azure portalından indirmiş meta veri XML içeriğini yapıştırın **meta veri** metin kutusu.

    ![Kimlik sağlayıcı ayarları ekleme](./media/active-directory-saas-saphana-tutorial/sap2.png)

    b. XML belgesinin içeriğini geçerliyse, ayrıştırma işlemi eklemek için gereken bilgileri ayıklar **konu, varlık kimliği ve sertifikayı veren** genel veri ekran alanının alanlar ve hedef ekranında URL alanlar alan, örneğin,  **taban URL ve SingleSignOn URL (*)**.

    ![Kimlik sağlayıcı ayarları ekleme](./media/active-directory-saas-saphana-tutorial/sap3.png)

    c. Genel veri ekran alanının adı kutusuna yeni SAML SSO kimlik sağlayıcısı için bir ad girin.

    > [!Note]
    > SAML IDP adı zorunludur ve benzersiz olmalıdır; Örneğin, kimlik doğrulama ekran bölümünde XS yapı Yönetim Aracı'nı kullanmak SAP HANA XS uygulamalar için kimlik doğrulama yöntemi olarak SAML seçerseniz görüntülenir, kullanılabilir SAML IDPs listesinde görüntülenir.

10. Yeni SAML kimlik sağlayıcısı ayrıntılarını kaydedin. Seçin **kaydetmek** SAML kimlik sağlayıcısı ayrıntılarını kaydetmek ve yeni SAML IDP bilinen SAML IDPs listesine eklemek için.

    ![Kaydet düğmesi](./media/active-directory-saas-saphana-tutorial/sap4.png)

11. Sistem özelliklerini içinde HANA Studio'da **yapılandırma** sekmesinde, yalnızca ayarlarına göre filtre **saml** ve ayarlama **assertion_timeout** gelen **10 sn**  için **120 saniye**.

    ![assertion_timeout ayarı](./media/active-directory-saas-saphana-tutorial/sap7.png)

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-saphana-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-saphana-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Ekle düğmesi](./media/active-directory-saas-saphana-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-saphana-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="creating-a-sap-hana-test-user"></a>SAP HANA test kullanıcısı oluşturma

SAP HANA oturum açmak Azure AD kullanıcıları etkinleştirmek için bunların SAP HANA sağlanmalıdır.
SAP HANA tam zamanı sağlama, varsayılan olarak etkin olduğu destekler.

Bir kullanıcı el ile oluşturmanız gerekiyorsa, aşağıdaki adımları gerçekleştirin:

>[!Note]
>Kullanıcı tarafından kullanılan dış kimlik doğrulama değiştirebilirsiniz.
Dış kullanıcılar, örneğin Kerberos sistem bir dış sistem kullanılarak doğrulanır. Dış kimlikler hakkında ayrıntılı bilgi için kişi, [etki alanı yöneticisi](https://cloudplatform.sap.com/contact.html).

1. Açık [SAP HANA Studio](https://help.sap.com/viewer/a2a49126a5c546a9864aae22c05c3d0e/2.0.01/en-us) bir Yöneticiyseniz ve etkinleştir DB kullanıcı SAML SSO olarak.

    ![Kullanıcı oluştur](./media/active-directory-saas-saphana-tutorial/sap5.png)

2. Görünmez onay kutusunun solundaki değer **SAML** ve aşağıdaki Yapılandır bağlantısını izleyin.

3. Tıklatın **Ekle** SAML IDP eklenecek ve tıklatın **Tamam** uygun SAML IDP seçme.

4. Ekleme **Dış kimlik** (örneğin. Burada BrittaSimon) veya seçin **"Tüm"** tıklatıp **Tamam**.

    >[!Note]
    >"Tüm" onay kutusu işaretli sonra HANA kullanıcı adı tam etki alanı soneki önce UPN kullanıcı adı ile eşleşmesi gerekir (yani BrittaSimon@contoso.com HANA BrittaSimon olur).

5. Test amacıyla, tüm Ata **"XS"** kullanıcı rolleri.

    ![rol atama](./media/active-directory-saas-saphana-tutorial/sap6.png)

    > [!TIP]
    > Bu, kullanım örnekleri için yalnızca uygun izinleri vermeniz gerekir.

6. Kullanıcı kaydedin.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, SAP HANA erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**SAP HANA Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **SAP HANA**.

    ![Kullanıcı atama](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli SAP HANA parçasında tıklattığınızda, otomatik olarak SAP HANA uygulamanıza açan.
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

