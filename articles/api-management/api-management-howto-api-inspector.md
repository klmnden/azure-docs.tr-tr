---
title: Azure API Yönetimi’nde istek izlemeyi kullanarak API’lerinizdeki hataları ayıklama | Microsoft Docs
description: Azure API Yönetimi’nde istek işleme adımlarını inceleme hakkında bilgi edinmek için bu öğreticinin adımlarını izleyin.
services: api-management
documentationcenter: ''
author: juliako
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 11/19/2017
ms.author: apimpm
ms.openlocfilehash: 4db89b52a3fa9585572322d546d87513da41c3f0
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="debug-your-apis-using-request-tracing"></a>İstek izleme özelliğini kullanarak API’lerinizdeki hataları ayıklama

Bu öğreticide, API’nizdeki hataları ayıklamanıza ve sorunları gidermenize yardımcı olması için istek işlemenin nasıl inceleneceği açıklanmaktadır. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Çağrı izleme

![API denetçisi](media/api-management-howto-api-inspector/api-inspector001.PNG)

## <a name="prerequisites"></a>Ön koşullar

+ Şu hızlı başlangıcı tamamlayın: [Azure API Management örneği oluşturma](get-started-create-service-instance.md).
+ Ayrıca, şu öğreticiyi tamamlayın: [İlk API'nizi içeri aktarma ve yayımlama](import-and-publish.md).

## <a name="trace-a-call"></a>Çağrı izleme

1. **API’ler** seçeneğini belirleyin.
2. API listenizden **Tanıtım Konferansı API’sine** tıklayın.
3. **GetSpeakers** işlemini seçin.
4. **Test** sekmesine geçin.
5. Değer **true** olarak ayarlanmış şekilde, **Ocp-Apim-Trace** adlı bir HTTP üst bilgisini eklediğinizden emin olun.
6. API çağrısı yapmak için **"Gönder"** seçeneğine tıklayın. 
7. Çağrının tamamlanmasını bekleyin. 
8. **API konsolu**’nda **İzleme** sekmesine gidin. Ayrıntılı izleme bilgilerine atlamak için aşağıdaki bağlantılardan herhangi birine tıklayın: **gelen**, **arka uç**, **giden**.

    **Gelen** bölümünde, çağırandan alınan özgün istek API Yönetimini ve 2. adımda eklediğimiz hız sınırı ve üst bilgi ayarlama ilkeleri de dahil isteğe uygulanan tüm ilkeleri görürsünüz.

    **Arka uç** bölümünde, API arka ucuna gönderilen isteklerin API Yönetimini ve aldığı yanıtı görürsünüz.
    
    **Giden** bölümünde, çağırana geri gönderilmeden önce yanıta uygulanan tüm ilkeleri görürsünüz.
 
    > [!TIP]
    > Her bir adım, isteğin API Yönetimi tarafından alınmasından bu yana geçen süreyi de gösterir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Çağrı izleme

Sonraki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
> [Düzeltmeleri kullanma](api-management-get-started-revise-api.md)
