---
title: "Azure API Management'te istek izlemeyi kullanma Apı'lerinizi hata ayıklama | Microsoft Docs"
description: "İstek işleme adımları Azure API Management'te incelemek öğrenmek için bu öğreticinin adımları izleyin."
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 11/19/2017
ms.author: apimpm
ms.openlocfilehash: 7b9bec7927169b9d820c095a7d11705264e7dcfe
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2017
---
# <a name="debug-your-apis-using-request-tracing"></a>İstek izleme kullanılarak Apı'lerinizi hata ayıklama

Bu öğreticide, API sorun giderme ve hata ayıklama ile yardımcı olmak için istek işleme incelemek açıklar. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir çağrı izleme

![API denetçisi](media/api-management-howto-api-inspector/api-inspector001.PNG)

## <a name="prerequisites"></a>Ön koşullar

+ Aşağıdaki Hızlı Başlangıç tamamlamak: [bir Azure API Management örneği oluşturma](get-started-create-service-instance.md).
+ Ayrıca, aşağıdaki öğreticiye: [alma ve ilk API'nizi yayımlama](import-and-publish.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="trace-a-call"></a>Bir çağrı izleme

1. Seçin **API'leri**.
2. Tıklatın **Demo konferans API** API listenizden.
3. Seçin **GetSpeakers** işlemi.
4. Geçiş **Test** sekmesi.
5. Adlı bir HTTP üstbilgisi eklediğinizden emin olun **Ocp Apim izleme** kümesine değerle **doğru**.
6. Tıklatın **"Gönderme"** bir API çağrısı yapma. 
7. Çağrının tamamlanmasını bekleyin. 
8. Git **izleme** sekmesinde **API konsol**. Ayrıntılı izleme bilgilerini atlamak için aşağıdaki bağlantılardan herhangi birine tıklayın: **gelen**, **arka uç**, **giden**.

    İçinde **gelen** bölümüne, API Management alınan çağıran ve 2. adımda eklediğimiz hız sınırı ve set-üstbilgisi ilkeleri de dahil olmak üzere isteği uygulanan tüm ilkeler özgün istek bakın.

    İçinde **arka uç** bölümüne, API Management API arka uç ve onu alınan yanıtta gönderilen istekleri bakın.
    
    İçinde **giden** bölümüne, yanıtını çağırana yeniden göndermeden önce uygulanan tüm ilkeler bakın.
 
    > [!TIP]
    > İstek API Management tarafından alınan bu yana her adım geçen süre de gösterir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Bir çağrı izleme

Sonraki öğretici ilerleyin:

> [!div class="nextstepaction"]
> [Düzeltmelerini kullanın](api-management-get-started-revise-api.md)
