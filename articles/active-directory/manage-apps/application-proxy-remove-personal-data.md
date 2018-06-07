---
title: Kişisel verileri - Azure Active Directory Uygulama proxy'si kaldırma | Microsoft Docs
description: Kişisel veriler cihazlarda Azure Active Directory Uygulama proxy'si için yüklü bağlayıcıların kaldırın.
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/21/2018
ms.author: barbkess
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 94cf0464aca3c46e0c6425b0fb3e24fcd767f95c
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655333"
---
# <a name="remove-personal-data-for-azure-active-directory-application-proxy"></a>Azure Active Directory Uygulama proxy'si için kişisel verileri Kaldır  

Azure Active Directory Uygulama proxy'si bağlayıcılar aygıtlarınızda kişisel veriler olabileceğini anlamına gelir, cihazlarda yüklemenizi gerektirir. Bu makalede gizlilik artırmak için kişisel verileri silmek adımlar sağlar. 


## <a name="where-is-the-personal-data"></a>Kişisel verileri nerede?
Aşağıdaki Günlük türleri kişisel veri yazmak uygulama proxy'si mümkündür:

- Bağlayıcı olay günlükleri
- Windows olay günlükleri

## <a name="remove-personal-data-from-windows-event-logs"></a>Windows olay günlüklerini kişisel verileri Kaldır

Veri saklama için Windows olay günlüklerini yapılandırma hakkında daha fazla bilgi için bkz: [olay günlüğü ayarları](https://technet.microsoft.com/library/cc952132.aspx). Windows olay günlüklerini hakkında bilgi edinmek için [Windows Olay Günlüğü'ı kullanarak](https://msdn.microsoft.com/library/windows/desktop/aa385772.aspx).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-hybrid-note.md)]

## <a name="remove-personal-data-from-connector-event-logs"></a>Kişisel veri bağlayıcısı olay günlüklerini Kaldır

Uygulama proxy'si günlükler kişisel verileriniz yoksa emin olmak için şunlardan birini yapabilirsiniz:

- Silin veya gerekli olduğunda, verileri görüntüleyebilir veya
- Günlüğünü devre dışı bırakın

Bağlayıcı olay günlüklerini kişisel verileri kaldırmak için aşağıdaki bölümleri kullanın. Bağlayıcı yüklü olduğu tüm cihazlar için temizleme işlemini tamamlamalısınız.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

### <a name="view-or-export-specific-data"></a>Görüntülemek veya belirli verileri dışarı aktarma

Görüntülemek veya belirli veri vermek için ilgili girişlere her bağlayıcı olay günlükleri arayın. Günlükleri şu adreste bulunabilir `C:\ProgramData\Microsoft\Microsoft AAD Application Proxy Connector\Trace`. 

Günlükleri metin dosyaları olduğundan, kullanabileceğiniz [findstr](https://docs.microsoft.com/windows-server/administration/windows-commands/findstr) bir kullanıcıyla ilişkili metin girdilerini arayın.  

Kişisel verileri bulmak üzere UserID için günlük dosyalarını arayın. 

Kerberos Kısıtlı temsilci kullanan bir uygulama tarafından günlüğe kaydedilen kişisel veriler bulmak için bu kullanıcı adı türü bileşenleri için arama yapın:

- Şirket içi kullanıcı asıl adı
- Kullanıcı adı, kullanıcı asıl adının parçası
- Kullanıcı adı, şirket içi kullanıcı asıl adının parçası
- Şirket içi Güvenlik Hesapları Yöneticisi (SAM) hesap adı 


### <a name="delete-specific-data"></a>Belirli verilerini sil

Belirli veri silmek için:

1. Yeni bir günlük dosyası oluşturmak için Microsoft Azure AD uygulama ara sunucusu Bağlayıcısı hizmetini yeniden başlatın. Yeni bir günlük dosyası, silmek veya eski günlük dosyaları değiştirmenize olanak sağlar. 
2. İzleyin [görünüm veya dışarı aktarma belirli veri](#view-or-export-specific-data) işlemi açıklanmıştır daha önce silinmesi için gereken bilgileri bulunamıyor. Tüm bağlayıcı günlükler arayın.
3. İlgili günlük dosyalarını silin veya kişisel veriler içeren alanlar seçmeli olarak silme. Artık ihtiyaç duymuyorsanız, ayrıca tüm eski günlük dosyaları silebilirsiniz.

### <a name="turn-off-connector-logs"></a>Connector günlükleri devre dışı bırakma

Connector günlükleri kişisel veri içermeyen emin olmak için bir günlük oluşturma devre dışı bırakma seçenektir. Connector günlükleri oluşturulmasını durdurmak için aşağıdaki vurgulanan satırından kaldırmak `C:\Program Files\Microsoft AAD App Proxy Connector\ApplicationProxyConnectorService.exe.config`. 

![Yapılandırma](./media/application-proxy-remove-personal-data/01.png)


## <a name="next-steps"></a>Sonraki adımlar

Uygulama proxy'si genel bakış için bkz: [güvenli uzaktan erişim sağlamak şirket içi uygulamalar](application-proxy.md).

