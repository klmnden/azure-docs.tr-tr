---
title: Logic Apps ile Azure Analysis Services modellerinde yenileme | Microsoft Docs
description: Azure Logic Apps kullanarak zaman uyumsuz yenileme kod öğrenin.
author: chrislound
manager: kfile
ms.service: analysis-services
ms.topic: conceptual
ms.date: 04/26/2019
ms.author: chlound
ms.openlocfilehash: 6e1ac5dfd1972e406a1bd8dcd26e6aef2c4ea6d1
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64919876"
---
# <a name="refresh-with-logic-apps"></a>Logic Apps ile yenileme

Logic Apps ve REST çağrılarını kullanarak sorgu genişleme salt okunur çoğaltmalarında eşitleme dahil olmak üzere, Azure Analysis tablosal modeller üzerinde otomatik veri yenileme işlemlerini gerçekleştirebilirsiniz.

Azure Analysis Services ile REST API'lerini kullanma hakkında daha fazla bilgi edinmek için [REST API ile zaman uyumsuz yenileme](analysis-services-async-refresh.md).

## <a name="authentication"></a>Kimlik Doğrulaması

Tüm çağrıları ile geçerli bir Azure Active Directory (OAuth 2) belirteci kimlik doğrulaması gerekir.  Bu makaledeki örneklerde, bir hizmet sorumlusu (SPN), Azure Analysis Services için kimlik doğrulaması için kullanır. Daha fazla bilgi için bkz. [Azure portalını kullanarak bir hizmet sorumlusu oluşturma](../active-directory/develop/howto-create-service-principal-portal.md).

## <a name="design-the-logic-app"></a>Mantıksal uygulama tasarlama

> [!IMPORTANT]
> Aşağıdaki örnekler, Azure Analysis Services Güvenlik Duvarı'nı devre dışı olduğunu varsayalım.  Güvenlik Duvarı etkinse, isteği başlatanın genel IP adresini Azure Analysis Services Güvenlik Duvarı'nda izin verilenler listesinde olması gerekir. Bölge başına Logic App IP aralıkları hakkında daha fazla bilgi için bkz. [limitler ve yapılandırma bilgilerini Azure Logic Apps](../logic-apps/logic-apps-limits-and-config.md#firewall-configuration-ip-addresses).

### <a name="prerequisites"></a>Önkoşullar

#### <a name="create-a-service-principal-spn"></a>(SPN) hizmet sorumlusu oluşturma

Bir hizmet sorumlusu oluşturma hakkında bilgi edinmek için [Azure portalını kullanarak bir hizmet sorumlusu oluşturma](../active-directory/develop/howto-create-service-principal-portal.md).

#### <a name="configure-permissions-in-azure-analysis-services"></a>Azure Analysis Services'de izinlerini yapılandırma
 
Oluşturduğunuz hizmet asıl sunucuda Sunucu yönetici izinleri olmalıdır. Daha fazla bilgi için bkz. [sunucu yöneticisi rolüne hizmet sorumlusu ekleme](analysis-services-addservprinc-admins.md).

### <a name="configure-the-logic-app"></a>Mantıksal uygulamayı yapılandırma

Bu örnekte, mantıksal uygulama, bir HTTP isteği alındığında tetiklemek için tasarlanmıştır. Bu, Azure Analysis Services model yenileme tetiklemek için Azure Data Factory gibi bir düzenleme aracı kullanımını etkinleştirir.

Mantıksal uygulama oluşturduktan sonra:

1. Mantıksal uygulama tasarımcısında olarak ilk eylemin **olduğunda bir HTTP isteği alındığında**.

   ![Alınan HTTP etkinlik Ekle](./media/analysis-services-async-refresh-logic-app/1.png)

Mantıksal uygulama kaydedildikten sonra bu adımı HTTP POST URL'si ile doldurur.

2. Yeni adım ekleme ve arama **HTTP**.  

   ![HTTP etkinlik Ekle](./media/analysis-services-async-refresh-logic-app/9.png)

   ![HTTP etkinlik Ekle](./media/analysis-services-async-refresh-logic-app/10.png)

3. Seçin **HTTP** Bu eylem ekleme.

   ![HTTP etkinlik Ekle](./media/analysis-services-async-refresh-logic-app/2.png)

HTTP etkinliğini aşağıdaki gibi yapılandırın:

|Özellik  |Değer  |
|---------|---------|
|**Yöntem**     |POST         |
|**URI**     | https://*sunucu bölgenizi*/servers/*aas sunucu adı*/models/*veritabanı adınız*/ <br /> <br /> Örneğin: https://westus.asazure.windows.net/servers/myserver/models/AdventureWorks/|
|**Üst Bilgiler**     |   İçerik türü, uygulama/json <br /> <br />  ![Üst bilgiler](./media/analysis-services-async-refresh-logic-app/6.png)    |
|**Gövde**     |   İstek gövdesi oluşturma hakkında daha fazla bilgi için bkz: [zaman uyumsuz yenileme REST API - POST /refreshes](analysis-services-async-refresh.md#post-refreshes). |
|**Kimlik doğrulaması**     |Active Directory OAuth         |
|**Kiracı**     |Azure Active Directory Tenantıd'nizi doldurun         |
|**Hedef kitle**     |https://*.asazure.windows.net         |
|**İstemci kimliği**     |Hizmet sorumlusu adı ClientID girin         |
|**Kimlik bilgisi türü**     |Gizli dizi         |
|**Gizli dizi**     |Hizmet sorumlusu adı gizli anahtarı girin         |

Örnek:

![Tamamlanan HTTP etkinliği](./media/analysis-services-async-refresh-logic-app/7.png)

Şimdi mantıksal uygulamayı test edin.  Mantıksal Uygulama Tasarımcısı'nda tıklatın **çalıştırma**.

![Mantıksal uygulamayı test etme](./media/analysis-services-async-refresh-logic-app/8.png)

## <a name="consume-the-logic-app-with-azure-data-factory"></a>Mantıksal uygulama Azure Data Factory ile kullanma

Mantıksal uygulama kaydedildikten sonra gözden **olduğunda bir HTTP isteği alındığında** etkinliği ve ardından kopyalama **HTTP POST URL'si** , artık oluşturulur.  Bu mantıksal uygulama tetikleyicisi zaman uyumsuz çağrı yapmak için Azure Data Factory tarafından kullanılan URL'dir.

Azure veri fabrikası Web etkinliği Bu eylem gerçekleştiren bir örnek aşağıda verilmiştir.

![Data Factory Web etkinliği](./media/analysis-services-async-refresh-logic-app/11.png)

## <a name="use-a-self-contained-logic-app"></a>Kendi içinde bir mantıksal uygulama kullanma

Data Factory gibi bir düzenleme aracı kullanarak model yenileme tetiklemek için üzerinde planlamıyorsanız, mantıksal uygulama, bir zamanlamaya göre yenileme tetiklemek için ayarlayabilirsiniz.

Yukarıdaki örneği kullanarak, ilk etkinliği silin ve değiştirin bir **zamanlama** etkinlik.

![Zamanlama etkinliği](./media/analysis-services-async-refresh-logic-app/12.png)

![Zamanlama etkinliği](./media/analysis-services-async-refresh-logic-app/13.png)

Bu örnekte **yinelenme**.

Etkinlik eklendiğinde aralığı ve sıklığı Yapılandır sonra yeni bir parametre ekleyin ve seçin **bu saatlerde**.

![Zamanlama etkinliği](./media/analysis-services-async-refresh-logic-app/16.png)

İstenen saat seçin.

![Zamanlama etkinliği](./media/analysis-services-async-refresh-logic-app/15.png)

Mantıksal uygulamayı kaydedin.

## <a name="next-steps"></a>Sonraki adımlar

[Örnekler](analysis-services-samples.md)  
[REST API](https://docs.microsoft.com/rest/api/analysisservices/servers)
