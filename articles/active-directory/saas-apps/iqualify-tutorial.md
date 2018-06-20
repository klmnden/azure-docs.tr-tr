---
title: 'Öğretici: Azure Active Directory Tümleştirme iQualify LMS ile | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile iQualify LMS arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 8a3caaff-dd8d-4afd-badf-a0fd60db3d2c
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/04/2017
ms.author: jeedes
ms.openlocfilehash: 166dfa8c5b3456de59055e5f62a566064540db31
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36225033"
---
# <a name="tutorial-azure-active-directory-integration-with-iqualify-lms"></a>Öğretici: Azure Active Directory Tümleştirme iQualify LMS ile

Bu öğreticide, Azure Active Directory (Azure AD) ile iQualify LMS tümleştirmek öğrenin.

İQualify LMS Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- İQualify LMS erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak Azure AD hesaplarına sahip iQualify LMS (çoklu oturum açma) için açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme LMS iQualify ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir iQualify LMS çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden iQualify LMS ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-iqualify-lms-from-the-gallery"></a>Galeriden iQualify LMS ekleme
Azure AD iQualify LMS tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden iQualify LMS eklemeniz gerekir.

**Galeriden iQualify LMS eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **iQualify LMS**seçin **iQualify LMS** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde iQualify LMS](./media/iqualify-tutorial/tutorial_iqualify_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." olarak adlandırılan bir test kullanıcı LMS dayalı iQualify ile test etme

Tekli çalışmaya oturum için Azure AD ne karşılık gelen kullanıcı iQualify LMS Azure AD'de bir kullanıcıya olduğunu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının iQualify ilgili kullanıcı arasında bir bağlantı ilişkisi LMS kurulması gerekir.

Değeri iQualify LMS'da, Ata **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma iQualify LMS ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir iQualify LMS test kullanıcısı oluşturma](#create-an-iqualify-lms-test-user)**  - bir Britta Simon karşılık gelen iQualify kullanıcı Azure AD gösterimini bağlı LMS içinde olması.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma iQualify LMS uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma LMS iQualify ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **iQualify LMS** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/iqualify-tutorial/tutorial_iqualify_samlbase.png)

3. Üzerinde **iQualify LMS etki alanı ve URL'leri** bölümünde, uygulama tarafından başlatılan IDP modunda yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin:

    ![iQualify LMS etki alanı ve URL'ler çoklu oturum açma bilgileri](./media/iqualify-tutorial/tutorial_iqualify_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: 
    | |
    |--|--|
    | Üretim ortamı: `https://<yourorg>.iqualify.com/`|
    | Test ortamı: `https://<yourorg>.iqualify.io`|
    
    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: 
    | |
    |--|--|
    | Üretim ortamı: `https://<yourorg>.iqualify.com/auth/saml2/callback` |
    | Test ortamı: `https://<yourorg>.iqualify.io/auth/saml2/callback` |

4. Denetleme **Göster Gelişmiş URL ayarları** ve uygulamada yapılandırmak istiyorsanız aşağıdaki adımı gerçekleştirin **SP** modunda başlatılan:

    ![iQualify LMS etki alanı ve URL'ler çoklu oturum açma bilgileri](./media/iqualify-tutorial/tutorial_iqualify_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:
    | |
    |--|--|
    | Üretim ortamı: `https://<yourorg>.iqualify.com/login` |
    | Test ortamı: `https://<yourorg>.iqualify.io/login` |
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler, gerçek tanımlayıcı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Kişi [iQualify LMS istemci destek ekibi](https://www.iqualify.com) bu değerleri almak için. 

5. İQualify LMS uygulama belirli bir biçimde görüntülenmesi için güvenlik onaylama işlemi biçimlendirme dili (SAML) onaylar bekler. Talepleri yapılandırmak ve öznitelik değerlerini yönetmek **kullanıcı öznitelikleri** aşağıdaki ekran görüntüsünde gösterildiği gibi iQualify uygulama tümleştirmesi sayfanın bölümünde:
    
    ![Çoklu oturum açmayı yapılandırın](./media/iqualify-tutorial/atb.png)

6. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim tabloda gösterilen her satır için aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı | Öznitelik Değeri |
    | --- | --- |    
    | e-posta | User.userPrincipalName |
    | ilk_ad | User.givenName |
    | Soyadı | User.surname |
    | person_id | "özniteliği" | 

    a. Tıklatın **Ekle özniteliği** açmak için **özniteliği eklemek** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/iqualify-tutorial/atb2.png)

    ![Çoklu oturum açmayı yapılandırın](./media/iqualify-tutorial/atb3.png)
    
    b. İçinde **adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın.
    
    c. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.
    
    d. Tıklatın **Tamam**

    e. Adımları "a" ile "d" sonraki tablo satır için yineleyin. 

    > [!Note]
    > İçin adımları "a" ile "d" yinelenen **person_id** özniteliği **isteğe bağlı**

7. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base 64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/iqualify-tutorial/tutorial_iqualify_certificate.png) 

8. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/iqualify-tutorial/tutorial_general_400.png)
    
9. Üzerinde **iQualify LMS yapılandırma** 'yi tıklatın **iQualify LMS yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![iQualify LMS yapılandırma](./media/iqualify-tutorial/tutorial_iqualify_configure.png) 

10.  Yeni bir tarayıcı penceresi açın ve iQualify ortamınıza yönetici olarak oturum açın.

11. Oturum açtığında, avatar sağ üst köşedeki tıklayın ve ardından üzerinde **"Hesap ayarları."**

    ![Hesap ayarları](./media/iqualify-tutorial/setting1.png) 
12. Hesap ayarları alanında, sol taraftaki Şerit menüsünde tıklatın ve tıklayın **"TÜMLEŞTİRMELER."**
    
    ![TÜMLEŞTİRMELER](./media/iqualify-tutorial/setting2.png)

13. TÜMLEŞTİRMELER altında tıklayın **SAML** simgesi.

    ![SAML simgesi](./media/iqualify-tutorial/setting3.png)

14. İçinde **SAML kimlik doğrulaması ayarlarını** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![SAML kimlik doğrulama ayarları](./media/iqualify-tutorial/setting4.png)

    a. İçinde **SAML çoklu oturum açma hizmet URL'si** kutusunda, yapıştırma **SAML tek Sign‑On hizmeti URL'si** değeri Azure AD uygulama yapılandırma penceresinden kopyalar.
    
    b. İçinde **SAML oturum kapatma URL'si** kutusunda, yapıştırma **Sign‑Out URL** değeri Azure AD uygulama yapılandırma penceresinden kopyalar.
    
    c. İndirilen sertifika dosyasını Not Defteri'nde açın, içeriği Kopyala ve ardından yapıştırın **ortak sertifika** kutusu.
    
    d. İçinde **oturum açma düğmesi ETİKETİ** oturum açma sayfasında görüntülenecek düğmesi için bir ad girin.
    
    e. **KAYDET**'e tıklayın.

    f. Tıklatın **güncelleştirme**.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/iqualify-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/iqualify-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/iqualify-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/iqualify-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-an-iqualify-lms-test-user"></a>Bir iQualify LMS test kullanıcısı oluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı iQualify içinde oluşturulur. iQualify LMS just‑in‑time kullanıcı hazırlama, varsayılan olarak etkin olduğu destekler.

Bu bölümde, eylem öğe yok. Bir kullanıcı iQualify zaten yoksa, iQualify LMS erişmeyi denediğinde yeni bir tane oluşturulur.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta iQualify LMS erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**İQualify LMS Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **iQualify LMS**.

    ![Uygulamalar listesinde iQualify LMS bağlantı](./media/iqualify-tutorial/tutorial_iqualify_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde LMS döşeme iQualify tıkladığınızda, oturum açma sayfasına iQualify LMS uygulamanızın almanız gerekir. 

   ![oturum açma sayfası](./media/iqualify-tutorial/login.png) 

Tıklatın **oturum Azure AD'de oturum** düğmesini otomatik olarak oturum açma iQualify LMS uygulamanıza.

Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/iqualify-tutorial/tutorial_general_01.png
[2]: ./media/iqualify-tutorial/tutorial_general_02.png
[3]: ./media/iqualify-tutorial/tutorial_general_03.png
[4]: ./media/iqualify-tutorial/tutorial_general_04.png

[100]: ./media/iqualify-tutorial/tutorial_general_100.png

[200]: ./media/iqualify-tutorial/tutorial_general_200.png
[201]: ./media/iqualify-tutorial/tutorial_general_201.png
[202]: ./media/iqualify-tutorial/tutorial_general_202.png
[203]: ./media/iqualify-tutorial/tutorial_general_203.png

