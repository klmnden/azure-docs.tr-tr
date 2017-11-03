---
title: "Uygulama proxy'si uygulama oluşturma sorunu | Microsoft Docs"
description: "Azure AD Yönetim Portalı'nda uygulama proxy'si uygulamaları oluşturma sorunlarını giderme"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: fe56f56162ba7186f1b660a5b44fcef38f1dee9d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="problem-creating-an-application-proxy-application"></a>Uygulama proxy'si uygulama oluşturma sorunu 

Aşağıda bazı yaygın sorunlar kişiler yüz yeni bir uygulama proxy'si uygulama oluştururken verilmektedir.

## <a name="recommended-documents"></a>Önerilen belgeler 

Yönetim Portalı üzerinden uygulama proxy'si uygulama oluşturma hakkında daha fazla bilgi için bkz: [Azure AD uygulama proxy'si ile uygulama yayımlama](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal).

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
[Azure portalında uygulama ara sunucusunu etkinleştirme](active-directory-application-proxy-enable.md)
