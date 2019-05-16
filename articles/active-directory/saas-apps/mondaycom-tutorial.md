---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile monday.com | Microsoft Docs'
description: Azure Active Directory ve monday.com arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 9e8ad807-0664-4e31-91de-731097c768e2
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/15/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: fc142bf02a44ea85861f4cc648fd7ee8602c7520
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65780814"
---
# <a name="tutorial-azure-active-directory-integration-with-mondaycom"></a>Öğretici: Monday.com ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile monday.com tümleştirme konusunda bilgi edinin.

Monday.com Azure AD ile tümleştirme, aşağıdaki avantajları sunar:

* Azure AD monday.com erişimi denetlemek için kullanabilirsiniz.
* Kullanıcıları otomatik olarak Azure AD hesaplarına (çoklu oturum açma) ile monday.com için oturum açmanız.
* Hesaplarınız bir merkezi konumda, Azure portalında yönetebilir.

Azure AD ile bir hizmet (SaaS) uygulamasını tümleştirme olarak yazılım hakkında daha fazla bilgi için bkz. [Azure Active Directory'de uygulamalar için çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile monday.com yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD aboneliğiniz yoksa, oluşturun bir [ücretsiz bir hesap](https://azure.microsoft.com/free/) başlamadan önce.
* Çoklu oturum etkin açma monday.com abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin ve monday.com Azure AD ile tümleştirme yapılandırın.

Monday.com aşağıdaki özellikleri destekler:

* **SP tarafından başlatılan çoklu oturum açma**
* **IDP tarafından başlatılan çoklu oturum açma**
* **Just-ın-time kullanıcı sağlama**

## <a name="add-mondaycom-in-the-azure-portal"></a>Azure portalında Monday.com Ekle

Monday.com Azure AD ile tümleştirmek için monday.com yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Sol menüde **Azure Active Directory**.

    ![Azure Active Directory seçeneği](common/select-azuread.png)

1. Seçin **kurumsal uygulamalar** > **tüm uygulamaları**.

    ![Kurumsal uygulamalar bölmesi](common/enterprise-applications.png)

1. Bir uygulama eklemek için seçin **yeni uygulama**.

    ![Yeni uygulama seçeneği](common/add-new-app.png)

1. Arama kutusuna **monday.com**. Arama sonuçlarında seçin **monday.com**ve ardından **Ekle**.

    ![sonuç listesinde Monday.com](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma monday.com adlı bir test kullanıcı tabanlı test **Britta Simon**. Tek iş için oturum açma için bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bağlı bir ilişki içinde monday.com oluşturmanız gerekir.

Yapılandırma ve Azure AD çoklu oturum açma monday.com ile test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

| Görev | Açıklama |
| --- | --- |
| **[Azure AD çoklu oturum açmayı yapılandırın](#configure-azure-ad-single-sign-on)** | Bu özelliği kullanmak olanak sağlar. |
| **[Monday.com çoklu oturum açmayı yapılandırın](#configure-mondaycom-single-sign-on)** | Uygulamada çoklu oturum açma ayarları yapılandırır. |
| **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)** | Testleri Azure AD çoklu oturum açma kullanıcı Britta Simon adı. |
| **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)** | Azure AD çoklu oturum açmayı kullanmak Britta Simon sağlar. |
| **[Monday.com test kullanıcısı oluşturma](#create-a-mondaycom-test-user)** | Kullanıcı Azure AD gösterimini bağlı monday.com içinde bir karşılığı Britta simon'un oluşturur. |
| **[Çoklu oturum açma testi](#test-single-sign-on)** | Yapılandırma çalıştığını doğrular. |

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma monday.com Azure portalında yapılandırın.

1. İçinde [Azure portalında](https://portal.azure.com/), **monday.com** uygulama tümleştirme bölmesinde **çoklu oturum açma**.

    ![Çoklu oturum açma seçeneği yapılandırın](common/select-sso.png)

1. İçinde **tek bir oturum açma yönteminizi seçmeniz** bölmesinde seçin **SAML** veya **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

1. İçinde **yukarı çoklu oturum açma SAML ile ayarlayın** bölmesinde **Düzenle** (açmak için kalem simgesi) **temel SAML yapılandırma** bölmesi.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. İçinde **temel SAML yapılandırma** bölmesinde, bir hizmet sağlayıcısı meta veri dosyası varsa ve yapılandırmak istediğiniz *IDP tarafından başlatılan modu*, aşağıdaki adımları tamamlayın:

    1. Seçin **meta veri dosyasını karşıya yükleme**.

       ![Karşıya yükleme meta verileri dosyası seçeneği](common/upload-metadata.png)

    1. Meta veri dosyası seçmek için klasör simgesini seçin ve ardından **karşıya**.

       ![Meta veri dosyası seçin ve ardından karşıya yükleme düğmesini seçin.](common/browse-upload-metadata.png)

    1. Meta veri dosyası başarıyla karşıya yüklendikten sonra **tanımlayıcı** ve **yanıt URL'si** değerleri içinde otomatik olarak doldurulur **temel SAML yapılandırma** bölmesi:

       ![Temel bir SAML yapılandırma bölmesinde IDP değerleri](common/idp-intiated.png)

       > [!Note]
       > Varsa **tanımlayıcı** ve **yanıt URL'si** değerleri otomatik olarak doldurulmamış, değerleri el ile girin.

1. Uygulamayı yapılandırmak için *SP tarafından başlatılan modu*:

    1. Seçin **ek URL'lerini ayarlayın**.
    
    1. İçinde **oturum açma URL'si** kutusunda, aşağıdaki desenin bir URL girin: https:\//\<etki-alanınız >. monday.com. İlgili kişi [monday.com istemci Destek ekibine](mailto:support@monday.com) oturum açma URL'sini alabilirsiniz.

        ![Küme ek URL'ler seçeneği](common/metadata-upload-additional-signon.png)

1. Monday.com uygulama SAML onaylamalarını belirli bir biçimde olmasını bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelik değerleri yönetmek için **yukarı çoklu oturum açma SAML ile ayarlanmış** bölmesinde **Düzenle** açmak için **kullanıcı öznitelikleri** bölmesi.

    ![Kullanıcı öznitelikleri bölmesi](common/edit-attribute.png)

1. Altında **kullanıcı taleplerini**seçin **Düzenle** talepleri düzenlenecek. Bir talep eklemek için seçin **Ekle yeni talep**ve ardından SAML belirteci öznitelik önceki resimde gösterildiği gibi yapılandırın. Ardından, aşağıdaki adımları tamamlayın: 

    1. Seçin **Ekle yeni talep**.

        ![Yeni Ekle talep kullanıcı talepleri bölmesinde seçeneği](common/new-save-attribute.png)

    1. İçinde **yönetmek, kullanıcı talepleri** bölmesinde aşağıdaki değerleri ayarlayın:
        
       1. İçinde **adı** kutusunda, kullanıcı talep satır için gösterilen öznitelik adını girin.

       1. Bırakın **Namespace** boş.

       1. İçin **kaynak**seçin **özniteliği**.

       1. İçinde **kaynak özniteliği** listesinde, kullanıcı talep satır için gösterilen öznitelik değeri seçin.

       1. Seçin **Tamam**ve ardından **Kaydet**.

       ![Yönet kullanıcı talepleri](common/new-attribute-details.png)

1. İçinde **yukarı çoklu oturum açma SAML ile ayarlanmış** bölmesi altında **SAML imzalama sertifikası**seçin **indirme** yanındaki **sertifika (Base64)**. Gereksinimlerinize göre bir indirme seçeneğini seçin. Sertifika bilgisayarınıza kaydedin.

    ![Sertifika (Base64) yükleme seçeneği](common/certificatebase64.png)

1. İçinde **monday.com kümesi** bölümünde, gereksinimlerinize göre aşağıdaki URL'ler kopyalayın:

    * Oturum Açma URL'si:
    * Azure AD Tanımlayıcısı
    * Oturum Kapatma URL'si

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

### <a name="configure-mondaycom-single-sign-on"></a>Monday.com çoklu oturum açmayı yapılandırın

Çoklu oturum açma monday.com tarafında yapılandırmak için indirilen sertifika (Base64) dosya ve Azure için portaldan kopyaladığınız ilgili URL'leri Gönder [monday.com Destek ekibine](mailto:support@monday.com). Oturum açma SAML tek bağlantısı her iki kenarı da düzgün ayarlandığından emin olmak için bunları gönderdiğiniz bilgiler monday.com Destek ekibine kullanır.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında Britta Simon adlı bir test kullanıcısı oluşturun.

1. Azure portalında **Azure Active Directory** > **kullanıcılar** > **tüm kullanıcılar**.

    ![Kullanıcılar ve tüm kullanıcılar seçenekleri](common/users.png)

1. Seçin **yeni kullanıcı**.

    ![Yeni kullanıcı seçeneği](common/new-user.png)

1. İçinde **kullanıcı** bölmesinde, aşağıdaki adımları tamamlayın:

    1. İçinde **adı** kutusuna **BrittaSimon**.
  
    1. İçinde **kullanıcı adı** kutusuna **brittasimon\@\<your-şirket etki alanı >.\< Uzantı >**. Örneğin, **brittasimon\@contoso.com**.

    1. Seçin **Show parola** onay kutusu. Görüntülenen değer azaltma **parola** kutusu.

    1. **Oluştur**’u seçin.

    ![Kullanıcı bölmesi](common/user-properties.png)

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Filiz Azure çoklu oturum açma kullanabilmeniz için monday.com Britta Simon erişim izni.

1. Azure portalında **kurumsal uygulamalar** > **tüm uygulamaları** > **monday.com**.

    ![Kurumsal uygulamalar bölmesi](common/enterprise-applications.png)

1. Uygulamalar listesinde **monday.com**.

    ![uygulamalar listesinde Monday.com](common/all-applications.png)

1. Menüde **kullanıcılar ve gruplar**.

    ![Kullanıcılar ve gruplar seçeneği](common/users-groups-blade.png)

1. Seçin **Kullanıcı Ekle**. Ardından **ataması ekleme** bölmesinde **kullanıcılar ve gruplar**.

    ![Ekle atama bölmesi](common/add-assign-user.png)

1. İçinde **kullanıcılar ve gruplar** bölmesinde **Britta Simon** kullanıcılar listesinde. **Seç**’i seçin.

1. SAML onaylaması rol değeri de beklediğiniz varsa **rol seçme** bölmesinde, listeden kullanıcı için uygun rolü seçin. **Seç**’i seçin.

1. İçinde **atama Ekle** bölmesinde **atama**.

### <a name="create-a-mondaycom-test-user"></a>Monday.com test kullanıcısı oluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı monday.com uygulama oluşturulur. Monday.com just-ın-time kullanıcı hazırlama, varsayılan olarak etkin olduğu destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Bir kullanıcı monday.com içinde zaten mevcut değilse yeni bir kimlik doğrulamasından sonra oluşturulur.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, uygulamalarım portalını kullanarak Azure AD çoklu oturum açma yapılandırmanızı test.

Çoklu oturum açma, getirdiğinizde seçtiğinizde **monday.com** uygulamalarım portalında, otomatik olarak monday.com için oturum açtınız. Uygulamalarım portal hakkında daha fazla bilgi için bkz. [erişim ve kullanım uygulamaları uygulamalarım portalında](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bu makaleleri gözden geçirin:

- [İçin Azure Active Directory ile SaaS uygulamalarını tümleştirme konusundaki öğreticilerin listesine](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)
- [Azure Active Directory'de uygulamalar için çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)
- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
