---
title: Konuşma Öğrenici - Microsoft Bilişsel hizmetler ile kullanıcı verileri yönetme | Microsoft Docs
titleSuffix: Azure
description: Konuşma Öğrenici kullanıcı verileriyle yönetmeyi öğrenin.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ms.openlocfilehash: 7ea0b246a16ff196a4160d9822b5db15cd39a4a6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66385192"
---
# <a name="managing-user-data"></a>Kullanıcı verilerini yönetme

Bu sayfa, konuşma Öğrenici bulut hizmeti iletişim kutuları olan son kullanıcılar yapılırken günlükleri açıklar.  Ayrıca, böylece belirli bir kullanıcıyla ilişkili tüm günlükler sileceğinizi konuşma Öğrenici günlükleri kullanıcı kimlikleri ile ilişkilendirmek nasıl açıklar.

## <a name="overview-of-end-user-data-logging"></a>Son kullanıcı veri günlüğü'ne genel bakış

Varsayılan olarak, konuşma Öğrenici bulut hizmeti botunuza son kullanıcılar arasındaki etkileşimleri kaydeder.  Bu günlükler, burada botunuzun yanlış varlık ayıklanan veya hatalı eylem seçili durumları tanımlamanıza izin etkinleştirme botunuzun iyileştirmek için önemlidir.  Bu hatalar, kullanıcı arabiriminin "Günlük iletişim kutuları" sayfasına giderek, düzeltme yapma ve düzeltilmiş bu iletişim kutusunda yeni train iletişim kutusu olarak depolama ardından düzeltilebilir. "Günlük iletişim kutuları." hakkında daha fazla bilgi için bkz

## <a name="how-to-disable-logging"></a>Günlüğe kaydetme devre dışı bırakma

Konuşma Öğrenici modeliniz için "Ayarlar" sayfasında son kullanıcılarla yapılan görüşmeler olup olmadığını kontrol edebilirsiniz.  "Günlük konuşmaları" için bir onay kutusu yok  Son kullanıcılarla yapılan görüşmeler bu kutusunun işaretini kaldırarak, günlüğe kaydedilmez.

## <a name="what-is-logged"></a>Günlüğe kaydedilir 

Günlük iletişim kutularında, konuşma Öğrenici kullanıcı girişi, varlık değerlerini, seçili eylem ve zaman damgaları için her bir bırakma depolar.  Bu günlükler bir süre için depolanır ve atılır (Ayrıntılar için "varsayılan değer ve sınırları" Yardım sayfasında bakın).  

Konuşma Öğrenici her oturum iletişim için benzersiz bir kimliği oluşturur.  Konuşma Öğrenici mu *değil* oturum iletişim kutuları bir kullanıcı tanımlayıcısı depolayın.  

## <a name="associating-logged-dialogs-with-a-user-id"></a>İletişim kutuları bir kullanıcı Kimliğiyle oturum ilişkilendirme

Genellikle, günlüğe kaydedilen iletişim kutuları örneğin--kullanıcı kimliği ile ilişkilendirmek için oturum açmış iletişim kutuları belirli bir kullanıcıdan sileceğinizi yapabilmek için olması önemlidir.  Konuşma Öğrenici kullanıcı tanımlayıcısı depolamaz olduğundan, bu ilişkiyi geliştiricinin kod tarafından tutulması gerekir.  

Bu eşlemeyi oluşturmak için oturum açmış iletişim kutusunda kimliği elde `EntityDetectionCallback`; ardından kullanıcı Kimliğini ve bu oturum iletişim arasındaki ilişki, botun depolamada depolamak.  

```
cl.EntityDetectionCallback(async (text: string, memoryManager: ClientMemoryManager): Promise<void> => {

    // Get user and session info
    var sessionData = memoryManager.SessionInfo();

    // sessionData.sessionId is the ID of this logged dialog.
    // In your bot-specific data store, store an association
    // between your user identifier and this session ID.
    console.log(sessionData.logDialogId)

    // sessionData.userId and sessionData.userName are the 
    // user ID and user name as defined by the channel the user
    // is on (eg, Skype, Teams, Facebook Messenger, etc.).
    // For some channels, this will be undefined.
    // This information is NOT stored with the logged dialog.
    console.log(sessionData.userId);
    console.log(sessionData.userName);

})
```

## <a name="headers-for-http-calls"></a>Üst bilgiler HTTP çağrıları için

Her HTTP çağrıları, aşağıdaki üst bilgi ekleyin:

```
Ocp-Apim-Subscription-Key=<LUIS_AUTHORING_KEY>
```

Burada `<LUIS_AUTHORING_KEY>` olduğu konuşma Öğrenici uygulamalarınıza erişmek için kullanılan anahtarı yazma LUIS.

## <a name="how-to-obtain-raw-data-for-a-logged-dialog"></a>Günlüğe kaydedilen bir iletişim kutusu için ham verileri elde etme

Günlük iletişim kutusu için ham verileri almak için bu HTTP çağrısı kullanabilirsiniz:

```
GET https://westus.api.cognitive.microsoft.com/conversationlearner/v1.0/app/<appId>/logdialog/<logDialogId>
```

Burada `<appId>` Bu konuşma Öğrenici modeli için GUID'dir ve `<logDialgoId>` almak istediğiniz günlük iletişim kimliğidir.  

> [!NOTE]
> Günlük iletişim kutuları geliştirici tarafından düzenlenebilir ve ardından iletişim kutularını eğitme olarak depolanan.  Bu yapıldığında, konuşma Öğrenici train iletişim kutusu "kaynak" günlük iletişim Kimliğini depolar.  Daha fazla train iletişim "kullanıcı Arabiriminde dal"; train iletişim bir ilişkili kaynak günlük iletişim kimliği varsa, ardından dallar Bu eğitimi iletişim kutusundan aynı günlük iletişim kimlikle işaretlenir

Günlük iletişim kutusundan türetilen tüm train iletişim kutuları edinmek için şu adımları izleyin.

İlk olarak, tüm train iletişim kutuları alın:

```
GET https://westus.api.cognitive.microsoft.com/conversationlearner/v1.0/app/<appId>/traindialogs
```

Burada `<appId>` Bu konuşma Öğrenici model GUID'dir.  

Bu, tüm train iletişim kutuları döndürür.  Bu listede ilişkili arama `sourceLogDialogId`ve ilişkili Not `trainDialogId`. 

Tek bir iletişim kutusu kimliği ile Eğitimi:

```
GET https://westus.api.cognitive.microsoft.com/conversationlearner/v1.0/app/<appId>/traindialog/<trainDialogId>
```

Burada `<appId>` Bu konuşma Öğrenici modeli için GUID'dir ve `<trainDialogId>` almak istediğiniz train iletişim kimliğidir.  

## <a name="how-to-delete-a-logged-dialog"></a>Günlüğe kaydedilen bir iletişim kutusu silme

Kendi Kimliğini belirtilen bir günlük iletişim silmek isterseniz, bu HTTP çağrısı kullanabilirsiniz:

```
DELETE https://westus.api.cognitive.microsoft.com/conversationlearner/v1.0/app/<appId>/logdialog/<logDialogId>
```

Burada `<appId>` Bu konuşma Öğrenici modeli için GUID'dir ve `<logDialogId>` silmek istediğiniz günlük iletişim kimliğidir. 

Kendi Kimliğini belirtilen bir eğitimi iletişim silmek isterseniz, bu HTTP çağrısı kullanabilirsiniz:

```
DELETE https://westus.api.cognitive.microsoft.com/conversationlearner/v1.0/app/<appId>/traindialog/<trainDialogId>
```

Burada `<appId>` Bu konuşma Öğrenici modeli için GUID'dir ve `<trainDialogId>` silmek istediğiniz eğitimi iletişim kimliğidir. 
