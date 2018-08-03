---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Splunk Enterprise ve Splunk bulut | Microsoft Docs'
description: Azure Active Directory ve Splunk Enterprise ve Splunk bulut arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: b3e2b4a9-749c-4895-813d-db46f8dfdbf8
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/02/2017
ms.author: jeedes
ms.openlocfilehash: b1eb8f198b0d9e25a6514e19794c9b1d54a4cd3e
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39436092"
---
# <a name="tutorial-azure-active-directory-integration-with-splunk-enterprise-and-splunk-cloud"></a>Öğretici: Azure Active Directory Splunk Enterprise ve Splunk bulut ile tümleştirme

Bu öğreticide, Splunk Enterprise ve Splunk bulut Azure Active Directory (Azure AD) ile tümleştirme konusunda bilgi edinin.

Splunk Enterprise ve Splunk bulut Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Splunk Enterprise ve Splunk bulut erişimi, Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan Splunk Enterprise ve Splunk bulut (çoklu oturum açma) için açma, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Splunk bulut Splunk Enterprise ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Bir Splunk Enterprise ve Splunk bulut çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Splunk Enterprise ve Splunk bulut galeri ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-splunk-enterprise-and-splunk-cloud-from-the-gallery"></a>Splunk Enterprise ve Splunk bulut galeri ekleme
Azure AD'de Splunk Enterprise ve Splunk bulut tümleştirmesini yapılandırmak için Splunk Enterprise ve Splunk bulut Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Splunk Enterprise ve Splunk bulut Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **Splunk Enterprise ve Splunk bulut**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_splunkenterpriseandsplunkcloud_search.png)

1. Sonuçlar panelinde seçin **Splunk Enterprise ve Splunk bulut**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_splunkenterpriseandsplunkcloud_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Splunk Enterprise ile test etme ve "Britta Simon." adlı bir test kullanıcı Splunk bulut tabanlı

Tek iş için oturum açma için Azure AD ne karşılık gelen kullanıcı Splunk Enterprise hem de Splunk Bulutundaki bir kullanıcının Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili kullanıcı Splunk Enterprise ve Splunk bulut arasında bir bağlantı ilişki kurulması gerekir.

Splunk Enterprise ve Splunk bulut, değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Splunk Enterprise ve Splunk Bulutu ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Splunk Enterprise ve Splunk bulut test kullanıcısı oluşturma](#creating-a-splunk-enterprise-and-splunk-cloud-test-user)**  - Splunk Enterprise, Britta Simon ve kullanıcı Azure AD gösterimini bağlı Splunk bulut bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Splunk Enterprise ve Splunk bulut uygulamanızı yapılandırın.

**Azure AD çoklu oturum açma Splunk bulut Splunk Enterprise ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Splunk Enterprise ve Splunk bulut** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_splunkenterpriseandsplunkcloud_samlbase.png)

1. Üzerinde **Splunk Enterprise ve Splunk bulut etki alanı ve URL'ler** uygulamada yapılandırmak isterseniz, bölümü **IDP** başlatılan modu:

    ![Çoklu oturum açmayı yapılandırın](./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_splunkenterpriseandsplunkcloud_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<splunkserverUrl>/en-US/app/launcher/home`
    
    b. İçinde **tanımlayıcı** metin Splunk sunucunuzun URL'sini yazın.

    c. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<splunkserver>/saml/acs`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Burada benzersiz dize değeri tanımlayıcıda kullanmanızı öneririz. İlgili kişi [Splunk Enterprise ve Splunk bulut istemci Destek ekibine](https://www.splunk.com/content/splunkcom/en_us/about-us/contact.html#tabs/customer-support) bu değerleri almak için. 

1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_splunkenterpriseandsplunkcloud_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_general_400.png)

1. Çoklu oturum açmayı yapılandırma **Splunk Enterprise ve Splunk bulut** tarafı, indirilen göndermek için ihtiyacınız **meta veri XML** için [Splunk Enterprise ve Splunk bulut Destek ekibine](https://www.splunk.com/content/splunkcom/en_us/about-us/contact.html#tabs/customer-support).

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/splunkenterpriseandsplunkcloud-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/splunkenterpriseandsplunkcloud-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/splunkenterpriseandsplunkcloud-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/splunkenterpriseandsplunkcloud-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-splunk-enterprise-and-splunk-cloud-test-user"></a>Splunk Enterprise ve Splunk bulut test kullanıcısı oluşturma

Bu bölümde, Splunk Enterprise, Britta Simon ve Splunk bulut adlı bir kullanıcı oluşturun. Çalışmak [Splunk Enterprise ve Splunk bulut Destek ekibine](https://www.splunk.com/content/splunkcom/en_us/about-us/contact.html#tabs/customer-support) Splunk Enterprise ve Splunk bulut platform kullanıcıları eklemek için.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma Splunk Enterprise hem de Splunk bulut uygulamalarına erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Splunk Enterprise ve Splunk bulut atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Splunk Enterprise ve Splunk bulut**.

    ![Çoklu oturum açmayı yapılandırın](./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_splunkenterpriseandsplunkcloud_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Azure AD erişim panelini kullanarak SSOonfiguration test edin.

Splunk Enterprise'ı tıklatın ve Splunk bulut erişim Paneli'nde kutucuk, size otomatik olarak Splunk Enterprise ve Splunk bulut uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)


<!--Image references-->

[1]: ./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_general_01.png
[2]: ./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_general_02.png
[3]: ./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_general_03.png
[4]: ./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_general_04.png

[100]: ./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_general_100.png

[200]: ./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_general_200.png
[201]: ./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_general_201.png
[202]: ./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_general_202.png
[203]: ./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_general_203.png

