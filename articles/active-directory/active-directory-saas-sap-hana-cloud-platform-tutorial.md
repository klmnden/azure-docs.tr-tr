---
title: 'Öğretici: Azure Active Directory Tümleştirme SAP bulut platformu ile | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ve SAP bulut platformu arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: bd398225-8bd8-4697-9a44-af6e6679113a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/15/2017
ms.author: jeedes
ms.openlocfilehash: 4d39c8437cbed497e2a2a7e64caa05f8e3d04869
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-sap-cloud-platform"></a>Öğretici: SAP bulut platformu Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile SAP bulut platformu tümleştirmek öğrenin.

SAP bulut platformu Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- SAP bulut platformu erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak (çoklu oturum açma) SAP bulut platformuna açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme SAP bulut platformu ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir SAP bulut platformu çoklu oturum açma abonelik etkin

Bu öğreticiyi tamamladıktan sonra SAP bulut platformu için atanmış Azure AD kullanıcılarının uygulama kullanarak çoklu oturum açabilirsiniz [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

>[!IMPORTANT]
>Kendi uygulamanızı dağıtmak veya çoklu oturum açma üzerinde test etmek için bir uygulama SAP bulut platformu hesabınızda abone olmak gerekir. Bu öğreticide, bir uygulama hesabında dağıtılır.
> 

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden SAP bulut platformu ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-sap-cloud-platform-from-the-gallery"></a>Galeriden SAP bulut platformu ekleme
Azure AD SAP bulut platformu tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden SAP bulut Platform eklemeniz gerekir.

**SAP bulut platformu Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **SAP bulut platformu**seçin **SAP bulut platformu** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde SAP bulut platformu](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/tutorial_sapcloudplatform_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı SAP bulut platformu ile test etme.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen SAP bulut Platform bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının SAP bulut platformu ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

SAP bulut platformunda değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma SAP bulut platformu ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[SAP bulut platformu test kullanıcısı oluşturma](#create-a-sap-cloud-platform-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı SAP bulut platformu sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma SAP bulut platformu uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma SAP bulut platformu ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **SAP bulut platformu** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/tutorial_sapcloudplatform_samlbase.png)

3. Üzerinde **SAP bulut platformu etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![SAP bulut platformu etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/tutorial_sapcloudplatform_url.png)

    a. İçinde **oturum üzerinde URL'si** metin kutusuna, türü URL kullanılan kullanıcılarınız tarafından oturumu açmak için **SAP bulut platformu** uygulama. Korunan bir kaynağa SAP bulut platformu uygulamanızda hesaplarına özgü URL'sidir. URL aşağıdaki desenine dayanır: `https://<applicationName><accountName>.<landscape host>.ondemand.com/<path_to_protected_resource>`
      
     >[!NOTE]
     >Bu kullanıcının kimlik doğrulamasını gerektirir, SAP bulut platformu uygulamanızda URL'dir.
     > 

    | |
    |--|
    | `https://<subdomain>.hanatrial.ondemand.com/<instancename>` |
    | `https://<subdomain>.hana.ondemand.com/<instancename>` |

    b. İçinde **tanımlayıcısı** textbox sağlayacağınızı SAP bulut platformun aşağıdaki desenleri birini kullanarak bir URL yazın: 

    | |
    |--|
    | `https://hanatrial.ondemand.com/<instancename>` |
    | `https://hana.ondemand.com/<instancename>` |
    | `https://us1.hana.ondemand.com/<instancename>` |
    | `https://ap1.hana.ondemand.com/<instancename>` |

    c. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:

    | |
    |--|
    | `https://<subdomain>.hanatrial.ondemand.com/<instancename>` |
    | `https://<subdomain>.hana.ondemand.com/<instancename>` |
    | `https://<subdomain>.us1.hana.ondemand.com/<instancename>` |
    | `https://<subdomain>.dispatcher.us1.hana.ondemand.com/<instancename>` |
    | `https://<subdomain>.ap1.hana.ondemand.com/<instancename>` |
    | `https://<subdomain>.dispatcher.ap1.hana.ondemand.com/<instancename>` |
    | `https://<subdomain>.dispatcher.hana.ondemand.com/<instancename>` |

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler, gerçek oturum açma URL'si tanımlayıcısı ve yanıt URL'si ile güncelleştirin. Kişi [SAP bulut platformu istemci destek ekibi](https://help.sap.com/viewer/65de2977205c403bbc107264b8eccf4b/Cloud/5dd739823b824b539eee47b7860a00be.html) oturum açma URL'si ve tanımlayıcısı alınamıyor. Yanıt URL'si, bu öğreticinin ilerleyen bölümlerinde açıklanan güven yönetim bölümünden edinebilirsiniz.
    > 
     
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/tutorial_sapcloudplatform_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/tutorial_general_400.png)

6. Farklı web tarayıcısı penceresinde SAP bulut platformu Pilot oturum `https://account.<landscape host>.ondemand.com/cockpit`(örneğin: https://account.hanatrial.ondemand.com/cockpit).

7. Tıklatın **güven** sekmesi.
   
    ![Güven](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/ic790800.png "güven")

8. Güven Yönetimi bölümünde altında **yerel hizmet sağlayıcısı**, aşağıdaki adımları gerçekleştirin:

    ![Yönetim güven](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/ic793931.png "güven Yönetimi")
   
    a. **Düzenle**’ye tıklayın.

    b. Olarak **yapılandırma türü**seçin **özel**.

    c. Olarak **yerel sağlayıcı adı**, varsayılan değeri bırakın. Bu değer kopyalayın ve yapıştırın **tanımlayıcısı** SAP bulut platformu için Azure AD yapılandırmasında alan.

    d. Oluşturmak için bir **imzalama anahtarı** ve **imzalama sertifikası** anahtar çifti, tıklatın **anahtar çifti oluşturma**.

    e. Olarak **asıl yayma**seçin **devre dışı**.

    f. Olarak **zorla kimlik doğrulama**seçin **devre dışı**.

    g. **Kaydet**’e tıklayın.

9. Kaydettikten sonra **yerel hizmet sağlayıcısı** ayarlarını, yanıt URL'si edinmek için aşağıdakileri gerçekleştirin:
   
    ![Meta veri alma](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/ic793930.png "meta verileri alma")

    a. SAP bulut platformu meta veri dosyası tıklatarak karşıdan **meta verileri alma**.

    b. İndirilen SAP bulut platformu meta veri XML dosyasını açın ve ardından bulun **ns3:AssertionConsumerService** etiketi.
 
    c. Değerini kopyalayın **konumu** özniteliği ve ardından yapıştırın **yanıt URL'si** SAP bulut platformu için Azure AD yapılandırmasında alan.

10. Tıklatın **güvenilen kimlik sağlayıcısı** sekmesini ve ardından **güvenilen kimlik sağlayıcısı ekleyin**.
   
    ![Yönetim güven](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/ic790802.png "güven Yönetimi")
   
    >[!NOTE]
    >Güvenilen kimlik sağlayıcıları listesini yönetmek için özel yapılandırma türü yerel hizmet sağlayıcısı bölümünde seçmiş olmanız gerekir. Varsayılan yapılandırma türü için düzenlenemeyen ve örtük güven SAP kimliği hizmetine sahip. Hiçbiri için herhangi bir güven ayarı yok.
    > 
    > 

11. Tıklatın **genel** sekmesini ve ardından **Gözat** indirilen meta veri dosyasını karşıya yüklemek için.
    
    ![Yönetim güven](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/ic793932.png "güven Yönetimi")
    
    >[!NOTE]
    >Meta veri dosyası, değerlerini karşıya sonra **çoklu oturum açma URL'si**, **çoklu oturum kapatma URL'si**, ve **imzalama sertifikası** otomatik olarak doldurulur.
    > 
     
12. **Öznitelikler** sekmesine tıklayın.

13. Üzerinde **öznitelikleri** sekmesinde, aşağıdaki adımı gerçekleştirin:
    
    ![Öznitelikleri](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/ic790804.png "öznitelikleri") 

    a. Tıklatın **Add Assertion-Based özniteliği**ve ardından aşağıdaki onaylama tabanlı öznitelikleri ekleyin:
       
    | Onaylama işlemi özniteliği | Asıl özniteliği |
    | --- | --- |
    | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname` |FirstName |
    | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname` |Soyadı |
    | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` |e-posta |
   
     >[!NOTE]
     >Nasıl SCP üzerinde uygulamaları, diğer bir deyişle, SAML yanıtta bekledikleri ve hangi adı (asıl özniteliği) altında kod bu öznitelikte eriştiklerinde hangi öznitelikleri geliştirilen üzerinde öznitelikleri yapılandırmasına bağlıdır.
     > 
    
    b. **Varsayılan özniteliği** ekran görüntüsünde yalnızca gösterim amacıyla kullanılır. İş senaryosu yapmak için gerekli değildir.  
 
    c. Adları ve değerleri için **asıl özniteliği** ekran görüntüsünde gösterilen uygulama nasıl geliştirilen bağlıdır. Uygulamanızın farklı eşlemeleri gerektirdiği mümkündür.

### <a name="assertion-based-groups"></a>Onaylama işlemi tabanlı grupları

İsteğe bağlı bir adım, Azure Active Directory kimlik sağlayıcısı için onaylama tabanlı grupları yapılandırabilirsiniz.

SAP bulut platformunda gruplarını kullanarak dinamik olarak bir veya daha fazla kullanıcı, SAP bulut platformu uygulamalarınızda SAML 2.0 onaylama özniteliklerinin değerlerini tarafından belirlenen bir veya daha fazla rol atamanıza olanak sağlar. 

Onaylama işlemi öznitelik içeriyorsa, örneğin, "*sözleşme geçici =*", etkilenen tüm kullanıcılar grubuna eklenecek isteyebilirler"*geçici*". Grup "*geçici*" SAP bulut platformu hesabınızda dağıtılan bir veya daha fazla uygulamalardan bir veya daha fazla rol içerebilir.
 
Aynı anda çok sayıda kullanıcı uygulamalarının SAP bulut platformu hesabınızda bir veya daha fazla rol atamak istiyorsanız onaylama tabanlı grupları kullanın. Yalnızca tek veya küçük kullanıcı sayısı çok belirli rollere atamak istiyorsanız, bunları doğrudan atama öneririz "**yetkilerini**" SAP bulut platformu Pilot sekmesinde.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-sap-cloud-platform-test-user"></a>SAP bulut platformu test kullanıcısı oluşturma

SAP bulut platformu oturum açmak Azure AD kullanıcıları etkinleştirmek için SAP bulut platformu rollerinde onlara atamanız gerekir.

**Bir kullanıcıya rol atamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum, **SAP bulut platformu** Pilot.

2. Aşağıdakileri gerçekleştirin:
   
    ![Yetkilerini](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/ic790805.png "yetkileri değiştirme")
   
    a. Tıklatın **yetkilendirme**.

    b. Tıklatın **kullanıcılar** sekmesi.

    c. İçinde **kullanıcı** metin kutusuna, kullanıcının e-posta adresini yazın.

    d. Tıklatın **atamak** kullanıcı role atamak için.

    e. **Kaydet**’e tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, SAP bulut platformu için erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**SAP bulut platformu Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **SAP bulut platformu**.

    ![Uygulamalar listesinde SAP bulut platformu bağlantı](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/tutorial_sapcloudplatform_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı erişim paneli kullanılarak Azure AD çoklu oturum açma yapılandırmanızı test etmektir.

Erişim paneli SAP bulut platformu parçasında tıklattığınızda, otomatik olarak SAP bulut platformu uygulamanıza açan.


## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sap-hana-cloud-platform-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sap-hana-cloud-platform-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sap-hana-cloud-platform-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sap-hana-cloud-platform-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sap-hana-cloud-platform-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sap-hana-cloud-platform-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sap-hana-cloud-platform-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sap-hana-cloud-platform-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sap-hana-cloud-platform-tutorial/tutorial_general_203.png

