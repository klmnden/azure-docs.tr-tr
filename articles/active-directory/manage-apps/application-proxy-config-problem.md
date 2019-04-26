---
title: Uygulama proxy'si uygulaması oluşturulurken hata | Microsoft Docs
description: Azure AD Yönetim Portalı'nda Uygulama Proxy uygulamaları oluşturma sorunlarını giderme
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
ms.date: 05/21/2018
ms.author: celested
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: e7aad73331d06828a4bfe909dbb3b98350b1d0d6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60293606"
---
# <a name="problem-creating-an-application-proxy-application"></a>Uygulama proxy'si uygulaması oluşturulurken hata 

Yaygın sorunlardan bazılarını insanların yüz yeni bir uygulama proxy'si uygulamasını oluştururken aşağıdadır.

## <a name="recommended-documents"></a>Önerilen belgeler 

Yönetici portalı üzerinden bir uygulama proxy'si uygulaması oluşturma hakkında daha fazla bilgi edinmek için [Azure AD uygulama ara sunucusu kullanarak uygulama yayımlama](application-proxy-add-on-premises-application.md).

Bu belgedeki adımlarda aşağıdaki ve uygulama oluşturulurken bir hata alıyorsanız, bilgi için hata ayrıntılarına ve önerileri uygulama düzeltme öğrenmek için bkz. Çoğu hata iletileri, önerilen düzeltmeyi içerir. 

## <a name="specific-things-to-check"></a>Denetlenecek belirli öğeler

Sık karşılaşılan hataları önlemek için aşağıdakileri doğrulayın:

-   Uygulama proxy'si uygulaması oluşturmak için izne sahip bir yöneticisiniz

-   İç URL benzersizdir

-   Dış URL benzersizdir

-   URL'leri http veya https ile başlayamaz ve bitemez bir "/"

-   URL, bir etki alanı adı ve IP adresi olmalıdır.

Uygulamayı oluştururken sağ üst köşede hata iletisi görüntülenmelidir. Hata iletilerini görmek için bildirim simgesini de seçebilirsiniz.

   ![Uyarı istemi](./media/application-proxy-config-problem/error-message.png)

## <a name="next-steps"></a>Sonraki adımlar
[Azure portalında uygulama ara sunucusunu etkinleştirme](application-proxy-add-on-premises-application.md)
