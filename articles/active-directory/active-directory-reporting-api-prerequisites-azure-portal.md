---
title: "Azure AD raporlama API'si erişmek için Önkoşullar | Microsoft Docs"
description: "Azure AD raporlama API'si erişmek için önkoşullar hakkında bilgi edinin"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: ada19f69-665c-452a-8452-701029bf4252
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/14/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 9fbcac4bd6240aed607c3da6c6535d207a782448
ms.sourcegitcommit: a036a565bca3e47187eefcaf3cc54e3b5af5b369
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2017
---
# <a name="prerequisites-to-access-the-azure-ad-reporting-api"></a>Azure AD raporlama API'si erişmek için Önkoşullar

[API'leri raporlama Azure AD](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) verilere bir dizi REST tabanlı API'ler aracılığıyla programlı erişim sağlar. Çeşitli programlama dilleri ve araçlarından bu API'leri çağırabilirsiniz.

Raporlama API kullandığı [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) web API'leri erişim yetkisi vermek için. 

Raporlama verilerini API aracılığıyla erişmek için atanan aşağıdaki rolleri birine sahip olması gerekir:

- Güvenlik okuyucusu
- Güvenlik Yöneticisi
- Genel yönetici


Raporlama API erişiminizi hazırlamak için şunları yapmanız gerekir:

1. Bir uygulamayı kaydetme 
2. İzin ver 
3. Yapılandırma ayarlarını toplayın 

Sorularınız, sorunları veya Geri bildiriminiz için lütfen [bir destek bileti dosya](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-troubleshooting-support-howto).

## <a name="register-an-azure-active-directory-application"></a>Bir Azure Active Directory uygulamayı kaydetme

Bir komut dosyası kullanarak raporlama API eriştiğiniz olsa bile bir uygulama kaydetmeniz gerekir. Bu size verir bir **uygulama kimliği**, yetkilendirme çağrısı için gerekli olduğu ve belirteçleri almak kodunuzu sağlar.

Azure AD raporlama API'si erişmek için dizininize yapılandırmak için Azure portalı, aynı zamanda bir üyesi olan Azure yönetici hesabı ile oturum gerekir **genel yönetici** Azure AD kiracınızda dizin rolü.

> [!IMPORTANT]
> Bu gibi "Yönetici" ayrıcalıkları olan kimlik bilgileri altında çalışan uygulamalar çok güçlü olabilir, bu nedenle Lütfen uygulamanın kimliği/parola kimlik bilgileri güvenli tutmak emin olun.
> 


**Bir Azure Active Directory uygulaması kaydetmek için:**

1. İçinde [Azure portal](https://portal.azure.com), sol gezinti bölmesinde tıklatın **Azure Active Directory**.
   
    ![Uygulamayı Kaydet](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. Üzerinde **Azure Active Directory** dikey penceresinde tıklatın **uygulama kayıtlar**.

    ![Uygulamayı Kaydet](./media/active-directory-reporting-api-prerequisites-azure-portal/02.png) 

3. Üzerinde **uygulama kayıtlar** üstteki araç çubuğunda dikey tıklayın **yeni uygulama kaydı**.

    ![Uygulamayı Kaydet](./media/active-directory-reporting-api-prerequisites-azure-portal/03.png)

4. Üzerinde **oluşturma** dikey penceresinde, aşağıdaki adımları gerçekleştirin:

    ![Uygulamayı Kaydet](./media/active-directory-reporting-api-prerequisites-azure-portal/04.png)

    a. İçinde **adı** metin kutusuna, türü `Reporting API application`.

    b. Olarak **uygulama türü**seçin **Web uygulaması / API**.

    c. İçinde **oturum açma URL'si** metin kutusuna, türü `https://localhost`.

    d. **Oluştur**'a tıklayın. 


## <a name="grant-permissions"></a>İzin ver 

Bu adımın amacı, uygulamanızın vermektir **dizin verilerini okuma** izinleri **Windows Azure Active Directory** API.

![Uygulamayı Kaydet](./media/active-directory-reporting-api-prerequisites-azure-portal/16.png)
 

**Uygulama API kullanma izni vermek için:**

1. Üzerinde **uygulama kayıtlar** uygulamalar listesinde dikey tıklayın **raporlama API'si uygulama**.

2. Üzerinde **raporlama API'si uygulama** üstteki araç çubuğunda dikey tıklayın **ayarları**. 

    ![Uygulamayı Kaydet](./media/active-directory-reporting-api-prerequisites-azure-portal/05.png)

3. Üzerinde **ayarları** dikey penceresinde tıklatın **gerekli izinleri**. 

    ![Uygulamayı Kaydet](./media/active-directory-reporting-api-prerequisites-azure-portal/06.png)

4. Üzerinde **gerekli izinleri** dikey penceresinde, **API** tıklatın **Windows Azure Active Directory**. 

    ![Uygulamayı Kaydet](./media/active-directory-reporting-api-prerequisites-azure-portal/07.png)

5. Üzerinde **erişimi etkinleştir** dikey penceresinde, select **dizin verilerini okuma**. 

    ![Uygulamayı Kaydet](./media/active-directory-reporting-api-prerequisites-azure-portal/08.png)

6. Üstteki araç çubuğunda tıklatın **kaydetmek**.

    ![Uygulamayı Kaydet](./media/active-directory-reporting-api-prerequisites-azure-portal/15.png)

7. Tıklatın **izinler**ve ardından **Evet**.

    ![Uygulamayı Kaydet](./media/active-directory-reporting-api-prerequisites-azure-portal/17.png)


## <a name="gather-configuration-settings"></a>Yapılandırma ayarlarını toplayın 
Bu bölüm, aşağıdaki ayarları dizininizden alma gösterir:

* Etki alanı adı
* İstemci Kimliği
* Gizli anahtar

Raporlama API çağrıları yapılandırırken bu değerleri gerekir. 

### <a name="get-your-domain-name"></a>Etki alanı adınızı alma

**Etki alanınızın adını almak için:**

1. İçinde [Azure portal](https://portal.azure.com), sol gezinti bölmesinde tıklatın **Azure Active Directory**.
   
    ![Uygulamayı Kaydet](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. Üzerinde **Azure Active Directory** dikey penceresinde tıklatın **özel etki alanı adları**.

    ![Uygulamayı Kaydet](./media/active-directory-reporting-api-prerequisites-azure-portal/09.png) 

3. Etki alanı adınızı etki alanları listesinden kopyalayın.


### <a name="get-your-applications-client-id"></a>Uygulamanızın istemci kimliği alın

**Uygulamanızın istemci Kimliğini almak için:**

1. İçinde [Azure portal](https://portal.azure.com), sol gezinti bölmesinde tıklatın **Azure Active Directory**.
   
    ![Uygulamayı Kaydet](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. Üzerinde **uygulama kayıtlar** uygulamalar listesinde dikey tıklayın **raporlama API'si uygulama**.

3. Üzerinde **raporlama API'si uygulama** dikey penceresinde, **uygulama kimliği**, tıklatın **kopyalamak için tıklayın**.

    ![Uygulamayı Kaydet](./media/active-directory-reporting-api-prerequisites-azure-portal/11.png) 



### <a name="get-your-applications-client-secret"></a>Uygulamanızın istemci parolası mı almak
Uygulamanızın istemci parolası mı almak için yeni bir anahtar oluşturun ve bu değer daha sonra artık almak mümkün olmadığı için yeni anahtarı kaydetme sırasında değerini kaydedin gerekir.

**Uygulamanızın istemci parolası mı almak için:**

1. İçinde [Azure portal](https://portal.azure.com), sol gezinti bölmesinde tıklatın **Azure Active Directory**.
   
    ![Uygulamayı Kaydet](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. Üzerinde **uygulama kayıtlar** uygulamalar listesinde dikey tıklayın **raporlama API'si uygulama**.


3. Üzerinde **raporlama API'si uygulama** üstteki araç çubuğunda dikey tıklayın **ayarları**. 

    ![Uygulamayı Kaydet](./media/active-directory-reporting-api-prerequisites-azure-portal/05.png)

4. Üzerinde **ayarları** dikey penceresindeki **APIR erişim** 'yi tıklatın **anahtarları**. 

    ![Uygulamayı Kaydet](./media/active-directory-reporting-api-prerequisites-azure-portal/12.png)


5. Üzerinde **anahtarları** dikey penceresinde, aşağıdaki adımları gerçekleştirin:

    ![Uygulamayı Kaydet](./media/active-directory-reporting-api-prerequisites-azure-portal/14.png)

    a. İçinde **açıklama** metin kutusuna, türü `Reporting API`.

    b. Olarak **Expires**seçin **2 yıl içinde**.

    c. **Kaydet** düğmesine tıklayın.

    d. Anahtar değerini kopyalayın.


## <a name="next-steps"></a>Sonraki Adımlar
* Verileri Azure AD raporlama API'si programlı bir şekilde erişmek ister misiniz? Kullanıma [Azure Active Directory raporlama API'si ile çalışmaya başlama](active-directory-reporting-api-getting-started.md).
* Azure Active Directory raporlama hakkında daha fazla bilgi edinmek istiyorsanız, bkz: [Azure Active Directory raporlama Kılavuzu](active-directory-reporting-guide.md).  

