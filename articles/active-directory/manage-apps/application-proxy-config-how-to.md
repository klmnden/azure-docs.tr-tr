---
title: Uygulama proxy'si uygulaması yapılandırma | Microsoft Docs
description: Bir yapılandırma, birkaç basit adımda uygulama proxy'si uygulaması oluşturmayı öğrenin
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/18/2018
ms.author: celested
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 95f22f064043467bf52c23cab547a7e6c8ba2205
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60443207"
---
# <a name="how-to-configure-an-application-proxy-application"></a>Uygulama proxy'si uygulaması yapılandırma

Bu makalede, şirket içi uygulamaları bulutta kullanıma sunmak için bir uygulama proxy'si uygulamasını Azure AD'ye nasıl anlamanıza yardımcı olur.

## <a name="recommended-documents"></a>Önerilen belgeler 

İlk Yapılandırma ve Yönetim Portalı üzerinden bir uygulama proxy'si uygulaması oluşturma hakkında bilgi edinmek için izleyin [Azure AD uygulama ara sunucusu kullanarak uygulama yayımlama](application-proxy-add-on-premises-application.md).

Bağlayıcı yapılandırma hakkında daha fazla bilgi için bkz [Azure portalında uygulama ara sunucusunu etkinleştirme](application-proxy-add-on-premises-application.md).

Sertifikaları karşıya yükleme ve özel etki alanlarını kullanma hakkında daha fazla bilgi için bkz: [Azure AD uygulama proxy'sinde özel etki alanları ile çalışma](application-proxy-configure-custom-domain.md).

## <a name="create-the-applicationsetting-the-urls"></a>Uygulama/ayarı URL'ler oluşturma

Adımları takip ediyorsanız [Azure AD uygulama ara sunucusu kullanarak uygulama yayımlama](application-proxy-add-on-premises-application.md) belgeleri ve bu bilgi için hata ayrıntılarına ve düzeltmeye ilişkin öneriler görürsünüz alınırken bir hata uygulaması oluşturma uygulama. Çoğu hata iletileri, önerilen düzeltmeyi içerir. Sık karşılaşılan hataları önlemek için aşağıdakileri doğrulayın:

-   Uygulama proxy'si uygulaması oluşturmak için izne sahip bir yöneticisiniz

-   İç URL benzersizdir

-   Dış URL benzersizdir

-   URL'leri http veya https ile başlayamaz ve bitemez bir "/"

-   URL bir etki alanı adı veya IP adresi olmalıdır

Uygulamayı oluştururken sağ üst köşede hata iletisi görüntülenmelidir. Hata iletilerini görmek için bildirim simgesini de seçebilirsiniz.

   ![Uyarı istemi](./media/application-proxy-config-how-to/error-message.png)

## <a name="configure-connectorsconnector-groups"></a>Bağlayıcıların/bağlayıcı gruplarını yapılandırma

Bağlayıcı grupları ve bağlayıcılar hakkında uyarı nedeniyle uygulamanızı yapılandırma ile ilgili sorunlar yaşıyorsanız, bağlayıcı indirme hakkında ayrıntılar için uygulama proxy'sini etkinleştirme hakkında yönergeler bakın. Bağlayıcılar hakkında daha fazla bilgi edinmek istiyorsanız bkz [bağlayıcılar belgelerine](application-proxy-connectors.md).

Bağlayıcılarınızı devre dışı ise bu hizmete bağlanamıyoruz anlamına gelir. Bu durum, genellikle tüm gereken bağlantı noktalarını açık olmadığından olur. Gerekli bağlantı noktalarının bir listesini görmek için etkinleştirme uygulama proxy'si belgelerin Önkoşullar bölümüne bakın.

## <a name="upload-certificates-for-custom-domains"></a>Özel etki alanları için sertifikaları karşıya yükle

Özel etki alanlarına etki alanı, dış URL'leri belirtmenizi sağlar. Özel etki alanlarını kullanmak için bu etki alanı için sertifika karşıya gerekir. Özel etki alanları ve sertifikalar hakkında daha fazla bilgi için bkz: [Azure AD uygulama proxy'sinde özel etki alanları ile çalışma](application-proxy-configure-custom-domain.md). 

Sertifikanızı karşıya yüklemeyi sorunlarla karşılaşıyorsanız, sertifikayla ilgili bir sorun hakkında daha fazla bilgi için portaldaki hata iletilerini arayın. Ortak sertifika sorunları şunlardır:

-   Süresi dolan sertifika

-   Otomatik olarak imzalanan sertifika

-   Sertifika özel anahtar eksik

Sertifikayı karşıya yüklemek çalışırken sağ üst köşede hata iletisini görüntüler. Hata iletilerini görmek için bildirim simgesini de seçebilirsiniz.

   ![Uyarı istemi](./media/application-proxy-config-how-to/error-message2.png)

## <a name="next-steps"></a>Sonraki adımlar
[Azure AD uygulama ara sunucusu kullanarak uygulama yayımlama](application-proxy-add-on-premises-application.md)
