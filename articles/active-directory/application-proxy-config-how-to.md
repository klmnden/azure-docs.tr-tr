---
title: Bir uygulama proxy'si uygulamasının nasıl yapılandırılacağını | Microsoft Docs
description: Bir yapılandırma birkaç basit adımda bir uygulama proxy'si uygulama oluşturmayı öğrenin
services: active-directory
documentationcenter: ''
author: ajamess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 816f2c10631de3809c6679c1e2715174f072f56d
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
---
# <a name="how-to-configure-an-application-proxy-application"></a>Uygulama proxy'si uygulama yapılandırma

Bu makalede, şirket içi uygulamaları bulutta kullanıma sunmak için bir uygulama proxy'si uygulamasının Azure AD içinde nasıl yapılandırıldığı anlamanıza yardımcı olur.

## <a name="recommended-documents"></a>Önerilen belgeler 

Başlangıç yapılandırmasını ve Yönetim Portalı aracılığıyla bir uygulama proxy'si uygulama oluşturma hakkında bilgi edinmek için izleyin [Azure AD uygulama proxy'si ile uygulama yayımlama](manage-apps/application-proxy-publish-azure-portal.md).

Bağlayıcılar yapılandırma hakkında daha fazla bilgi için bkz [Azure portalında uygulama ara sunucusunu etkinleştirme](manage-apps/application-proxy-enable.md).

Sertifikaları karşıya yükleniyor ve özel etki alanlarını kullanma hakkında daha fazla bilgi için bkz: [Azure AD uygulama proxy'si özel etki alanları ile çalışma](manage-apps/application-proxy-configure-custom-domain.md).

## <a name="create-the-applicationsetting-the-urls"></a>Uygulama/ayarı URL'leri oluştur

İçindeki adımları takip ediyorsanız [Azure AD uygulama proxy'si ile uygulama yayımlama](manage-apps/application-proxy-publish-azure-portal.md) belgelerine ve bu uygulama oluşturulurken bir hata alma Bkz. bilgi için hata ayrıntıları ve uygulama gidermeye yönelik öneriler. Çoğu hata iletileri önerilen bir düzeltmeyi içerir. Sık karşılaşılan hataları önlemek için doğrulayın:

-   Uygulama proxy'si uygulama oluşturmak için izne sahip bir yönetici

-   İç URL benzersizdir

-   Dış URL benzersizdir

-   URL'leri http veya https ile başlamalı ve bitmelidir bir "/"

-   Bir etki alanı adı, IP adresi olmalıdır

Uygulama oluşturduğunuzda, hata iletisi sağ üst köşedeki görüntülemelidir. Hata iletilerini görmek için bildirim simgesine de seçebilirsiniz.

   ![Bildirim istemi](./media/application-proxy-config-how-to/error-message.png)

## <a name="configure-connectorsconnector-groups"></a>Bağlayıcılar/bağlayıcı gruplarını yapılandırma

Bağlayıcılar ve bağlayıcı grupları hakkında uyarı nedeniyle, uygulamanızın yapılandırma zorluk yaşıyorsanız, bağlayıcılar indirme hakkında ayrıntılar için uygulama proxy'si etkinleştirme hakkında yönergeler bakın. Bağlayıcılar hakkında daha fazla bilgi edinmek istiyorsanız bkz [bağlayıcılar belgelerine](application-proxy-understand-connectors.md).

Bağlayıcılar etkin olmayan varsa, bu hizmete erişmek sorunu yaşıyor anlamına gelir. Bu, genellikle tüm gerekli bağlantı noktalarının açık olmadığından olur. Gerekli bağlantı noktalarının bir listesini görmek için etkinleştirme uygulama proxy'si belgelerin Önkoşullar bölümüne bakın.

## <a name="upload-certificates-for-custom-domains"></a>Özel etki alanları için sertifikalarını karşıya yükleme

Özel etki alanlarını etki alanı, dış URL'ler belirtmenizi sağlar. Özel etki alanlarını kullanmak için bu etki alanı için sertifika karşıya gerekir. Özel etki alanları ve sertifikaları kullanma hakkında daha fazla bilgi için bkz: [Azure AD uygulama proxy'si özel etki alanları ile çalışma](manage-apps/application-proxy-configure-custom-domain.md). 

Sertifikanız karşıya sorunlarla karşılaşıyorsanız, portal sertifikayla ilgili sorun hakkında daha fazla bilgi için hata iletilerinde arayın. Ortak sertifika sorunları şunları içerir:

-   Süresi dolan sertifika

-   Otomatik olarak imzalanan sertifika

-   Sertifika özel anahtarı eksik

Hata iletisini sertifikayı karşıya yüklemeye gibi sağ üst köşedeki görüntüler. Hata iletilerini görmek için bildirim simgesine de seçebilirsiniz.

   ![Bildirim istemi](./media/application-proxy-config-how-to/error-message2.png)

## <a name="next-steps"></a>Sonraki adımlar
[Azure AD uygulama proxy'si ile uygulama yayımlama](manage-apps/application-proxy-publish-azure-portal.md)
