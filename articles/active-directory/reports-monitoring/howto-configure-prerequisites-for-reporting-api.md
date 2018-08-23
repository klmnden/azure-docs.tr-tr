---
title: Azure Active Directory raporlama API'SİYLE erişmek için Önkoşullar | Microsoft Docs
description: Azure AD raporlama API'si erişmek için gereken önkoşullar hakkında bilgi edinin
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: ada19f69-665c-452a-8452-701029bf4252
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 05/07/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 8d610dc74b7e2ef10295bc0a3407cf7c3d781b51
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2018
ms.locfileid: "42057595"
---
# <a name="prerequisites-to-access-the-azure-active-directory-reporting-api"></a>Azure Active Directory raporlama API'SİYLE erişmek için Önkoşullar

[Azure Active Directory (Azure AD) raporlama API'leri](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview), bir dizi REST tabanlı API aracılığıyla verilere programlı erişim sağlar. Çeşitli programlama dilleri ve araçlarından bu API'leri çağırabilirsiniz.

Raporlama API'sini kullanan [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) web API'lerine erişim yetkisi vermek için.

Raporlama API'sini erişiminizi hazırlamak için şunları yapmanız:

1. Roller atama
2. Bir uygulamayı kaydetme
3. İzinleri verme
4. Yapılandırma ayarlarını toplayın



## <a name="assign-roles"></a>Roller atama

API aracılığıyla raporlama verilerine erişim almak için atanan aşağıdaki rollerden birine sahip olmanız gerekir:

- Güvenlik Okuyucu

- Güvenlik Yöneticisi

- Genel yönetici




## <a name="register-an-application"></a>Bir uygulamayı kaydetme

Betik kullanarak raporlama API'sini erişiyorsanız bile bir uygulamayı kaydetme gerekir. Bu size bir **uygulama kimliği**bir yetkilendirme çağrı için gerekli olan ve kodunuzu belirteçleri alabilmesini sağlar.

Azure AD raporlama API'si erişmek için dizininize yapılandırmak için Azure portalında da üyesi olan bir Azure yönetici hesabıyla oturum açmalısınız **genel yönetici** Azure AD kiracınızda dizin rolü.

> [!IMPORTANT]
> Bu gibi "Yönetici" ayrıcalıklarına sahip kimlik bilgileri altında çalışan uygulamalar, çok güçlü olabilir, bu nedenle Lütfen uygulamanın kimliği/parola kimlik bilgilerini güvenli tuttuğunuzdan emin olun.
> 


**Bir Azure Active Directory uygulaması kaydetmek için:**

1. İçinde [Azure portalında](https://portal.azure.com), sol gezinti bölmesinde **Azure Active Directory**.
   
    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2. Üzerinde **Azure Active Directory** sayfasında **uygulama kayıtları**.

    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/02.png) 

3. Üzerinde **uygulama kayıtları** sayfasında, üstteki araç çubuğunda tıklatın **yeni uygulama kaydı**.

    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/03.png)

4. Üzerinde **Oluştur** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/04.png)

    a. İçinde **adı** metin kutusuna `Reporting API application`.

    b. Olarak **uygulama türü**seçin **Web uygulaması / API**.

    c. İçinde **oturum açma URL'si** metin kutusuna `https://localhost`.

    d. **Oluştur**’a tıklayın. 


## <a name="grant-permissions"></a>İzinleri verme 

Erişmek istediğiniz API bağlı olarak, uygulamanızı aşağıdaki izinler gerekir:  

| API | İzin |
| --- | --- |
| Windows Azure Active Directory | Dizin verilerini okuyun |
| Microsoft Graph | Tüm denetim günlük verilerini okuyun |


![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/36.png)


Aşağıdaki bölümde, her iki API'leri için adımları listelenir. API'lerden birini erişmek istemiyorsanız, ilgili adımları atlayabilirsiniz.
 

**API'leri kullanmak için uygulama izinleri vermek için:**

1. Üzerinde **uygulama kayıtları** uygulamalar listesinde sayfasında **raporlama API'si uygulama**.

2. Üzerinde **raporlama API'si uygulama** sayfasında, üstteki araç çubuğunda tıklatın **ayarları**. 

    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/05.png)

3. Üzerinde **ayarları** sayfasında **gerekli izinler**. 

    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/06.png)

4. Üzerinde **gerekli izinler** sayfasında **API** listesinde **Windows Azure Active Directory**. 

    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/07.png)

5. Üzerinde **erişimini etkinleştir** sayfasında **dizin verilerini okuma** ve seçimini **oturum açın ve kullanıcı profilini okuma**. 

    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/08.png)

6. Üst araç çubuğunda tıklatın **Kaydet**.

    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/15.png)

7. Üzerinde **gerekli izinler** sayfasında, üstteki araç çubuğunda tıklatın **Ekle**.

    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/32.png)

8. Üzerinde **API erişimi Ekle** sayfasında **bir API seçin**.

    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/31.png)

9. Üzerinde **bir API seçin** sayfasında **Microsoft Graph**ve ardından **seçin**.

    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/33.png)

10. Üzerinde **erişimini etkinleştir** sayfasında **tüm denetim günlük verileri okuma**ve ardından **seçin**.  

    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/34.png)


11. Üzerinde **API erişimi Ekle** sayfasında **Bitti**.  

12. Üzerinde **gerekli izinler** sayfasının üst araç. tıklayın **izinler**ve ardından **Evet**.

    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/17.png)


## <a name="gather-configuration-settings"></a>Yapılandırma ayarlarını toplayın 

Bu bölümde, aşağıdaki ayarları dizininizden alma gösterir:

- Etki alanı adı
- İstemci Kimliği
- Gizli anahtar

Bu değerler, raporlama API çağrıları yapılandırırken gerekir. 

### <a name="get-your-domain-name"></a>Etki alanı adınızı alma

**Etki alanınızın adını almak için:**

1. İçinde [Azure portalında](https://portal.azure.com), sol gezinti bölmesinde **Azure Active Directory**.
   
    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2. Üzerinde **Azure Active Directory** sayfasında **özel etki alanı adları**.

    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/09.png) 

3. Etki alanı adınızı etki alanları listeden kopyalayın.


### <a name="get-your-applications-client-id"></a>Uygulamanızın istemci kimliği alın

**Uygulamanızın istemci kimliği almak için:**

1. İçinde [Azure portalında](https://portal.azure.com), sol gezinti bölmesinde **Azure Active Directory**.
   
    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2. Üzerinde **uygulama kayıtları** uygulamalar listesinde sayfasında **raporlama API'si uygulama**.

3. Üzerinde **raporlama API'si uygulama** sayfasında en **uygulama kimliği**, tıklayın **kopyalamak için tıklayın**.

    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/11.png) 



### <a name="get-your-applications-client-secret"></a>Uygulamanızda gizli anahtar alma
Uygulamanızın istemci gizli anahtarını almak için yeni bir anahtar oluşturun ve bu değer daha sonra artık almak mümkün olmadığından yeni anahtar kaydedildikten sonra değeri kaydetmek gerekir.

**Uygulamanızın istemci gizli anahtarını almak için:**

1. İçinde [Azure portalında](https://portal.azure.com), sol gezinti bölmesinde **Azure Active Directory**.
   
    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2. Üzerinde **uygulama kayıtları** uygulamalar listesinde sayfasında **raporlama API'si uygulama**.


3. Üzerinde **raporlama API'si uygulama** sayfasında, üstteki araç çubuğunda tıklatın **ayarları**. 

    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/05.png)

4. Üzerinde **ayarları** sayfasında **APIR erişim** bölümünde **anahtarları**. 

    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/12.png)


5. Üzerinde **anahtarları** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/14.png)

    a. İçinde **açıklama** metin kutusuna `Reporting API`.

    b. Olarak **Expires**seçin **2 yıl içinde**.

    c. **Kaydet**’e tıklayın.

    d. Anahtar değerini kopyalayın.


## <a name="next-steps"></a>Sonraki adımlar

* [Azure Active Directory'ı sertifikalarla raporlama API'sini kullanarak veri alma](tutorial-access-api-with-certificates.md)
* [Raporlama API'leriyle ilgili ilk izlenim elde edin](concept-reporting-api.md)
* [Denetim API'si başvurusu](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) 
* [Oturum açma etkinliği raporunu API Başvurusu](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)
