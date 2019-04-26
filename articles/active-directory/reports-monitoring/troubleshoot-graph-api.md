---
title: Azure Active Directory raporlama API'SİYLE hatalarını giderme | Microsoft Docs
description: Bir çözüm hataları için Azure Active Directory raporlama API'lerini çağrılırken sağlar.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 0030c5a4-16f0-46f4-ad30-782e7fea7e40
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
ms.openlocfilehash: 8b517204fb650020bdebf8172186f30fff58f722
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60285001"
---
# <a name="troubleshoot-errors-in-azure-active-directory-reporting-api"></a>Azure Active Directory raporlama API'SİYLE hatalarını giderme

Bu makalede, bunların çözümü için adımları ve MS Graph API'sini kullanarak Etkinlik raporlarını erişirken içine çalışabilir genel hata iletileri listelenir.

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

## <a name="next-steps"></a>Sonraki Adımlar

[API Başvurusu denetim kullanmak](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit)
[oturum açma etkinliği raporunu API başvuru kullanın](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)
