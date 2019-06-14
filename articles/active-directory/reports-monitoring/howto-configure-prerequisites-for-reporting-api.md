---
title: Azure Active Directory raporlama API'SİYLE erişmek için Önkoşullar | Microsoft Docs
description: Azure AD raporlama API'si erişmek için gereken önkoşullar hakkında bilgi edinin
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: ada19f69-665c-452a-8452-701029bf4252
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8ab30dfeccc4ae1c7b09a8f48846ffb9e71cfc23
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60439170"
---
# <a name="prerequisites-to-access-the-azure-active-directory-reporting-api"></a>Azure Active Directory raporlama API'SİYLE erişmek için Önkoşullar

[Azure Active Directory (Azure AD) raporlama API'leri](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview), bir dizi REST tabanlı API aracılığıyla verilere programlı erişim sağlar. Çeşitli programlama dilleri ve araçlarından bu API'leri çağırabilirsiniz.

Raporlama API'sini kullanan [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) web API'lerine erişim yetkisi vermek için.

Raporlama API'sini erişiminizi hazırlamak için şunları yapmanız:

1. [Rol Atama](#assign-roles)
2. [Uygulamayı kaydetme](#register-an-application)
3. [İzin ver](#grant-permissions)
4. [Yapılandırma ayarlarını toplayın](#gather-configuration-settings)

## <a name="assign-roles"></a>Rol atama

API aracılığıyla raporlama verilerine erişim almak için atanan aşağıdaki rollerden birine sahip olmanız gerekir:

- Güvenlik okuyucusu

- Güvenlik Yöneticisi

- Genel Yönetici


## <a name="register-an-application"></a>Bir uygulamayı kaydetme

Betik kullanarak raporlama API'sini erişiyorsanız bile bir uygulamayı kaydetmeniz gerekir. Bu size bir **uygulama kimliği**, yetkilendirme çağırır ve kodunuzu belirteçleri alabilmesini sağlar için gerekli olan.

Azure AD raporlama API'si erişmek için dizininize yapılandırmak için oturum gerekir [Azure portalında](https://portal.azure.com) da üyesi olan bir Azure yönetici hesabıyla **genel yönetici** dizin rolü Azure AD kiracınızda.

> [!IMPORTANT]
> Yönetici ayrıcalıklarına sahip kimlik bilgileri altında çalışan uygulamalar, çok güçlü olabilir, bu nedenle Lütfen uygulama kimliği ve gizli kimlik bilgilerini güvenli bir yerde saklamayı unutmayın.
> 

**Azure AD uygulaması kaydetmek için:**

1. İçinde [Azure portalında](https://portal.azure.com)seçin **Azure Active Directory** sol gezinti bölmesinden.
   
    ![Uygulamayı kaydetme](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2. İçinde **Azure Active Directory** sayfasında **uygulama kayıtları**.

    ![Uygulamayı kaydetme](./media/howto-configure-prerequisites-for-reporting-api/02.png) 

3. Gelen **uygulama kayıtları** sayfasında **yeni uygulama kaydı**.

    ![Uygulamayı kaydetme](./media/howto-configure-prerequisites-for-reporting-api/03.png)

4. İçinde **Oluştur** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Uygulamayı kaydetme](./media/howto-configure-prerequisites-for-reporting-api/04.png)

    a. İçinde **adı** metin kutusuna `Reporting API application`.

    b. Olarak **uygulama türü**seçin **Web uygulaması / API**.

    c. İçinde **oturum açma URL'si** metin kutusuna `https://localhost`.

    d. **Oluştur**’u seçin. 


## <a name="grant-permissions"></a>İzinleri verme 

Erişmek istediğiniz API bağlı olarak, uygulamanızı aşağıdaki izinler gerekir:  

| API | İzin |
| --- | --- |
| Windows Azure Active Directory | Dizin verilerini okuma |
| Microsoft Graph | Tüm denetim günlük verilerini okuyun |


![Uygulamayı kaydetme](./media/howto-configure-prerequisites-for-reporting-api/36.png)

Aşağıdaki bölümde, her iki API'leri için adımları listelenir. API'lerden birini erişmek istemiyorsanız, ilgili adımları atlayabilirsiniz.

**API'leri kullanmak için uygulama izinleri vermek için:**

1. Uygulamanızdan seçin **uygulama kayıtları** sayfasından seçim yapıp **ayarları**. 

    ![Uygulamayı kaydetme](./media/howto-configure-prerequisites-for-reporting-api/05.png)

2. Üzerinde **ayarları** sayfasında **gerekli izinler**. 

    ![Uygulamayı kaydetme](./media/howto-configure-prerequisites-for-reporting-api/06.png)

3. Üzerinde **gerekli izinler** sayfasında **API** listesinde **Windows Azure Active Directory**. 

    ![Uygulamayı kaydetme](./media/howto-configure-prerequisites-for-reporting-api/07.png)

4. Üzerinde **erişimini etkinleştir** sayfasında **dizin verilerini okuma** ve seçimini **oturum açın ve kullanıcı profilini okuma**. 

    ![Uygulamayı kaydetme](./media/howto-configure-prerequisites-for-reporting-api/08.png)

5. Üst araç çubuğunda tıklatın **Kaydet**.

    ![Uygulamayı kaydetme](./media/howto-configure-prerequisites-for-reporting-api/15.png)

6. Üzerinde **gerekli izinler** sayfasında, üstteki araç çubuğunda tıklatın **Ekle**.

    ![Uygulamayı kaydetme](./media/howto-configure-prerequisites-for-reporting-api/32.png)

7. Üzerinde **API erişimi Ekle** sayfasında **bir API seçin**.

    ![Uygulamayı kaydetme](./media/howto-configure-prerequisites-for-reporting-api/31.png)

8. Üzerinde **bir API seçin** sayfasında **Microsoft Graph**ve ardından **seçin**.

    ![Uygulamayı kaydetme](./media/howto-configure-prerequisites-for-reporting-api/33.png)

9. Üzerinde **erişimini etkinleştir** sayfasında **tüm denetim günlük verileri okuma**ve ardından **seçin**.  

    ![Uygulamayı kaydetme](./media/howto-configure-prerequisites-for-reporting-api/34.png)

10. Üzerinde **API erişimi Ekle** sayfasında **Bitti**.  

11. Üzerinde **gerekli izinler** sayfasının üst araç. tıklayın **izinler**ve ardından **Evet**.

    ![Uygulamayı kaydetme](./media/howto-configure-prerequisites-for-reporting-api/17.png)


## <a name="gather-configuration-settings"></a>Yapılandırma ayarlarını toplayın 

Bu bölümde, aşağıdaki ayarları dizininizden alma gösterir:

- Etki alanı adı
- İstemci Kimliği
- Gizli anahtar

Bu değerler, raporlama API çağrıları yapılandırırken gerekir. 

### <a name="get-your-domain-name"></a>Etki alanı adınızı alma

**Etki alanınızın adını almak için:**

1. İçinde [Azure portalında](https://portal.azure.com), sol gezinti bölmesinde seçin **Azure Active Directory**.
   
    ![Uygulamayı kaydetme](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2. Üzerinde **Azure Active Directory** sayfasında **özel etki alanı adları**.

    ![Uygulamayı kaydetme](./media/howto-configure-prerequisites-for-reporting-api/09.png) 

3. Etki alanı adınızı etki alanları listeden kopyalayın.


### <a name="get-your-applications-client-id"></a>Uygulamanızın istemci kimliği alın

**Uygulamanızın istemci kimliği almak için:**

1. İçinde [Azure portalında](https://portal.azure.com), sol gezinti bölmesinde **Azure Active Directory**.
   
    ![Uygulamayı kaydetme](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2. Uygulamanızdan seçin **uygulama kayıtları** sayfası.

3. Uygulama sayfasından gidin **uygulama kimliği** seçip **kopyalamak için tıklayın**.

    ![Uygulamayı kaydetme](./media/howto-configure-prerequisites-for-reporting-api/11.png) 


### <a name="get-your-applications-client-secret"></a>Uygulamanızda gizli anahtar alma
Uygulamanızın istemci gizli anahtarını almak için yeni bir anahtar oluşturun ve bu değer daha sonra artık almak mümkün olmadığından yeni anahtar kaydedildikten sonra değeri kaydetmek gerekir.

**Uygulamanızın istemci gizli anahtarını almak için:**

1. İçinde [Azure portalında](https://portal.azure.com), sol gezinti bölmesinde **Azure Active Directory**.
   
    ![Uygulamayı kaydetme](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2.  Uygulamanızdan seçin **uygulama kayıtları** sayfası.

3. Uygulama sayfasında, araç çubuğunda üstte **ayarları**. 

    ![Uygulamayı kaydetme](./media/howto-configure-prerequisites-for-reporting-api/05.png)

4. Üzerinde **ayarları** sayfasında **API erişimi** bölümünde **anahtarları**. 

    ![Uygulamayı kaydetme](./media/howto-configure-prerequisites-for-reporting-api/12.png)

5. Üzerinde **anahtarları** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Uygulamayı kaydetme](./media/howto-configure-prerequisites-for-reporting-api/14.png)

    a. İçinde **açıklama** metin kutusuna `Reporting API`.

    b. Olarak **Expires**seçin **2 yıl içinde**.

    c. **Kaydet**’e tıklayın.

    d. Anahtar değerini kopyalayın.

## <a name="troubleshoot-errors-in-the-reporting-api"></a>Raporlama API'sini hatalarını giderme

Bu bölümde, etkinlik raporları kendi çözümlemesi için adımları ve MS Graph API'sini kullanarak erişirken içine çalışabilir genel hata iletileri listelenir.

### <a name="500-http-internal-server-error-while-accessing-microsoft-graph-v2-endpoint"></a>Microsoft Graph V2 uç noktası erişirken 500 HTTP iç sunucu hatası

Şu anda Microsoft Graph v2 uç noktası desteklemiyoruz - Microsoft Graph v1 uç noktayı kullanarak etkinlik günlüklerini emin olun.

### <a name="error-failed-to-get-user-roles-from-ad-graph"></a>Hata: AD grafikten kullanıcı rolleri alınamadı

Oturum açma erişmeye çalışırken bu hata iletisini alabilirsiniz Graph Gezgini kullanarak. Oturum açma düğmelerinin her ikisi de Graph Gezgini Arabiriminde kullanarak hesabınızda aşağıdaki görüntüde gösterildiği gibi oturumunuz emin olun. 

![Graph Gezgini](./media/troubleshoot-graph-api/graph-explorer.png)

### <a name="error-failed-to-do-premium-license-check-from-ad-graph"></a>Hata: AD grafikten Premium lisansı denetimi yapmak başarısız oldu 

Oturum açma erişmeye çalışırken bu hatayı çalıştırırsanız Graph Gezgini kullanma, seçim **değiştirme izinlerini** hesabınızın sol gezinti ve seçim altında **Tasks.ReadWrite** ve **Directory.Read.All**. 

![UI izinleri değiştirme](./media/troubleshoot-graph-api/modify-permissions.png)


### <a name="error-neither-tenant-is-b2c-or-tenant-doesnt-have-premium-license"></a>Hata: Ne B2C kiracısının olduğu veya kiracının premium lisansı yok

Oturum açma raporları erişim gerektiren bir Azure Active Directory premium 1 (P1) lisans. Oturum açma erişirken bu hata iletisini görürseniz, bir Azure AD P1 lisansı ile kiracınıza lisansının olduğunu doğrulayın.

### <a name="error-user-is-not-in-the-allowed-roles"></a>Hata: Kullanıcı, izin verilen rollerinde değil 

Denetim günlükleri veya oturum açma API'sini kullanarak erişmeye çalışırken bu hatayı görürseniz, hesabınızın bir parçası olduğundan emin olun **güvenlik okuyucusu** veya **rapor okuyucu** Azure Active Directory'niz içindeki rolü Kiracı. 

### <a name="error-application-missing-aad-read-directory-data-permission"></a>Hata: Uygulamanın AAD 'Dizin verilerini okuma' izni yok 

Lütfen adımları [Azure Active Directory raporlama API'SİYLE erişmek için Önkoşullar](howto-configure-prerequisites-for-reporting-api.md) için uygulamanızın doğru izin kümesi ile çalıştığından emin olun. 

### <a name="error-application-missing-msgraph-api-read-all-audit-log-data-permission"></a>Hata: Uygulama MSGraph API 'tüm denetim günlük verileri okuma' izni yok

Lütfen adımları [Azure Active Directory raporlama API'SİYLE erişmek için Önkoşullar](howto-configure-prerequisites-for-reporting-api.md) için uygulamanızın doğru izin kümesi ile çalıştığından emin olun. 

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Active Directory'ı sertifikalarla raporlama API'sini kullanarak veri alma](tutorial-access-api-with-certificates.md)
* [Denetim API'si başvurusu](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) 
* [Oturum açma etkinliği raporunu API Başvurusu](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)
