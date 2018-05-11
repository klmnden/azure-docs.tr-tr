---
title: Azure Active Directory Uygulama proxy'si GDPR | Microsoft Docs
description: GDPR hakkında daha fazla Azure Active Directory Uygulama proxy'si öğrenin.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: c7186f98-dd80-4910-92a4-a7b8ff6272b9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/16/2018
ms.author: markvi
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: a3df15743b4918d72fac5f8769a2d3ee721a452f
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
---
# <a name="gdpr-in-the-azure-active-directory-application-proxy"></a>Azure Active Directory Uygulama proxy'si GDPR  

Azure Active Directory (Azure AD) uygulama proxy'si GDPR diğer tüm Microsoft hizmetlerini ve özellikleri ile birlikte uyumludur. Microsoft'un GDPR desteği hakkında daha fazla bilgi için bkz: [Lisans Koşulları'nı ve belgeleri](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31).

Bu özellik bilgisayarlarınızda bağlayıcılar içerdiğinden GDPR uyumlu kalmak için izlemeniz gereken birkaç olay vardır. Uygulama proxy'si EUII içerebilir aşağıdaki günlük türlerini oluşturur:

- Bağlayıcı olay günlükleri
- Windows olay günlükleri

GDPR uyumlu olmak iki yol vardır:

- Sil ve çıktıkları anda istekleri dışarı aktarma

- Günlüğünü devre dışı bırakın

İçin:

- Veri saklama için Windows olay günlüklerini yapılandırma hakkında bilgi [olay günlüğü ayarları](https://technet.microsoft.com/library/cc952132.aspx). 
- Windows olay günlüğü hakkında genel bilgi bkz [Windows Olay Günlüğü'ı kullanarak](https://msdn.microsoft.com/library/windows/desktop/aa385772.aspx).


Connect olay günlüklerine bulabilirsiniz `C:\ProgramData\Microsoft\Microsoft AAD Application Proxy Connector\Trace`. Aşağıdaki bölümler için bağlayıcı olay günlüklerini ile ilgili adımları sağlar. Tüm bağlayıcı bilgisayarlarda aşağıdaki adımları tamamlamanız gerekir.
 

## <a name="processing-requests"></a>İstek işleme

Üç farklı türde zararlardan sorumlu istekleri vardır: 

- Sil
- Görünüm
- Dışarı Aktarma
 
Görünümü işlemek / istekleri dışarı aktarma için her biri için ilgili girişlere aramak için günlük dosyalarını gitmesi gerekir. 

Günlükleri metin dosyaları olduğundan, tarafından Örneğin, kullanarak arayabilirsiniz `findstr` kullanıcınız için ilgili girişler bulmak için komutu. Günlüklerde olabileceğinden aşağıdaki alanlar için ara: 

- UserId
- Kerberos Kısıtlı temsilci kullanan tüm uygulamalar için yapılandırılmış kullanıcı adı yazın:
    - Kullanıcı asıl adı
    - Şirket içi kullanıcı asıl adı
    - Kullanıcı adı, kullanıcı asıl adının parçası
    - Kullanıcı adı, şirket içi kullanıcı asıl adının parçası
    - Şirket içi SAM hesabı adı 

 
Ardından, bu alanlar toplamak ve bunları kullanıcıyla paylaşabilirsiniz.
Delete isteklerini işlemek için ilgili günlükleri silmeniz gerekir. (Microsoft Azure AD uygulama ara sunucusu yeni bir günlük dosyası oluşturmak için Bağlayıcısı) bağlayıcı hizmetini yeniden başlatabilirsiniz. Yeni bir günlük dosyası, eski günlük dosyaları silmenize olanak sağlar. Görünüm için işlemi gerçekleştirin / tüm ilgili günlüklerini bulmak için verin ve bu alanları veya dosyaları seçmeli olarak silin. Artık ihtiyaç duymuyorsanız, ayrıca her zaman sadece tüm eski günlük dosyaları silebilirsiniz.


## <a name="turn-off-connector-logs"></a>Connector günlükleri devre dışı bırakma

Bağlayıcı oturumunu kapatıp açmak için ayarlamanız gerekir `C:\Program Files\Microsoft AAD App Proxy Connector\ApplicationProxyConnectorService.exe.config` vurgulanan satırı kaldırarak: 


![Yapılandırma](./media/active-directory-application-proxy-gdpr/01.png)


## <a name="next-steps"></a>Sonraki adımlar

Azure AD uygulama ara sunucusuna genel bakış için bkz: [güvenli uzaktan erişim sağlamak şirket içi uygulamalar](manage-apps/application-proxy.md).

