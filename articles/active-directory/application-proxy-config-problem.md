---
title: Uygulama proxy'si uygulama oluşturma sorunu | Microsoft Docs
description: Azure AD Yönetim Portalı'nda uygulama proxy'si uygulamaları oluşturma sorunlarını giderme
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
ms.openlocfilehash: 376925715e954904cfdadccb060d0ca242bbec4a
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
---
# <a name="problem-creating-an-application-proxy-application"></a>Uygulama proxy'si uygulama oluşturma sorunu 

Aşağıda bazı yaygın sorunlar kişiler yüz yeni bir uygulama proxy'si uygulama oluştururken verilmektedir.

## <a name="recommended-documents"></a>Önerilen belgeler 

Yönetim Portalı üzerinden uygulama proxy'si uygulama oluşturma hakkında daha fazla bilgi için bkz: [Azure AD uygulama proxy'si ile uygulama yayımlama](manage-apps/application-proxy-publish-azure-portal.md).

Bu belgede yer alan adımlar aşağıdaki ve uygulama oluşturulurken bir hata alıyor, bilgi için hata ayrıntıları ve uygulama gidermeye yönelik öneriler bakın. Çoğu hata iletileri önerilen bir düzeltmeyi içerir. 

## <a name="specific-things-to-check"></a>Denetlenecek belirli öğeler

Sık karşılaşılan hataları önlemek için doğrulayın:

-   Uygulama proxy'si uygulama oluşturmak için izne sahip bir yönetici

-   İç URL benzersizdir

-   Dış URL benzersizdir

-   URL'leri http veya https ile başlamalı ve bitmelidir bir "/"

-   URL bir etki alanı adı ve IP adresi olmalıdır

Uygulama oluşturduğunuzda, hata iletisi sağ üst köşedeki görüntülemelidir. Hata iletilerini görmek için bildirim simgesine de seçebilirsiniz.

   ![Bildirim istemi](./media/application-proxy-config-problem/error-message.png)

## <a name="next-steps"></a>Sonraki adımlar
[Azure portalında uygulama ara sunucusunu etkinleştirme](manage-apps/application-proxy-enable.md)
