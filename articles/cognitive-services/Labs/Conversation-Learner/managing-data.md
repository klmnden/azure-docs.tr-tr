---
title: Konuşma öğrenen - Microsoft Bilişsel hizmetler ile kullanıcı verileri yönetme | Microsoft Docs
titleSuffix: Azure
description: Konuşma öğrenen kullanıcı verileriyle yönetmeyi öğrenin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 8d42f903559a1e07b42ded33972be4b552f21b5e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353975"
---
# <a name="managing-user-data"></a>Kullanıcı verileri yönetme

Bu sayfa, konuşma öğrenen bulut hizmeti iletişim kutuları sahip son kullanıcıların yürütülürken günlüklerini açıklar.  Ayrıca, böylece almak veya belirli bir kullanıcı ile ilişkili tüm günlükleri silme konuşma öğrenen günlükleri kullanıcı kimlikleri ile ilişkilendirmek nasıl açıklanır.

## <a name="overview-of-end-user-data-logging"></a>Son kullanıcı veri günlüğü'ne genel bakış

Varsayılan olarak, konuşma öğrenen bulut hizmeti, son kullanıcılar, bot arasındaki etkileşimler kaydeder.  Bu günlükler, bot yanlış varlık ayıklanan veya yanlış eylem seçili olduğu durumları tanımlamanıza olanak sağlayarak, bot iyileştirmek için önemlidir.  Bu hatalar daha sonra UI "Günlük iletişim kutularını" sayfasına giderek, düzeltme yapma ve yeni bir tren iletişim kutusu düzeltilmiş bu iletişim kutusunu depolayarak düzeltilebilir. Öğretici "Günlük iletişim kutuları." hakkında daha fazla bilgi için bkz

## <a name="how-to-disable-logging"></a>Günlüğünü devre dışı bırakma

Son kullanıcılar ile görüşmeler konuşma öğrenen uygulamanız için "Ayarlar" sayfasında olup olmadığını kontrol edebilirsiniz.  "Günlük görüşmeleri." için bir onay kutusu yok  Son kullanıcılar ile görüşmeler bu kutuyu işaretleyerek tarafından günlüğe kaydedilmez.

## <a name="what-is-logged"></a>Günlüğe kaydedilenler 

Günlük iletişim kutularında, konuşma öğrenen kullanıcı girişi, varlık değerler, seçilen eylemleri ve zaman damgaları için her bırakma depolar.  Bu günlükler bir süre için depolanır ve (Ayrıntılar için bkz: "varsayılan değer ve sınırlar" Yardım sayfasında) atılır.  

Konuşma öğrenen her oturum iletişim için benzersiz bir kimliği oluşturur.  Konuşma öğrenen mu *değil* oturum iletişim kutuları ile kullanıcı tanımlayıcısı depolamak.  

## <a name="associating-logged-dialogs-with-a-user-id"></a>İletişim kutuları bir kullanıcı Kimliğiyle oturum ilişkilendirme

Genellikle, örneğin--kullanıcı Kimliğiyle oturum iletişim kutuları ilişkilendirilecek, almak veya belirli bir kullanıcının oturum iletişim kutuları silmek için olması önemlidir.  Konuşma öğrenen kullanıcı tanımlayıcısı depolamaz olduğundan, bu ilişki geliştiricinin kod tarafından tutulması gerekir.  

Bu eşlemeyi oluşturmak için günlüğe kaydedilen iletişim kutusunda kimliği elde `EntityDetectionCallback`; ardından bot ait depolama biriminde, kullanıcı kimliği ve oturum bu iletişim kutusu arasındaki ilişkiyi saklayın.  

```
cl.EntityDetectionCallback(async (text: string, memoryManager: ClientMemoryManager): Promise<void> => {

    // Get user and session info
    var sessionData = memoryManager.SessionInfo();

    // sessionData.sessionId is the ID of this logged dialog.
    // In your bot-specific datastore, store an association
    // bewteen your user identifier and this session ID.
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

## <a name="headers-for-http-calls"></a>HTTP çağrıları için üstbilgileri

Her aşağıdaki HTTP çağrıları aşağıdaki üstbilgi ekleyin:

```
Ocp-Apim-Subscription-Key=<LUIS_AUTHORING_KEY>
```

Burada `<LUIS_AUTHORING_KEY>` olan konuşma öğrenen uygulamalarınızı erişmek için kullanılan anahtar yazma HALUK.

## <a name="how-to-obtain-raw-data-for-a-logged-dialog"></a>Günlüğe kaydedilmiş bir iletişim kutusu için ham verileri elde etme

Bir günlük iletişim kutusu için ham verileri elde etmek için bu HTTP çağrı kullanabilirsiniz:

```
GET https://westus.api.cognitive.microsoft.com/conversationlearner/v1.0/app/<appId>/logdialog/<logDialogId>
```

Burada `<appId>` Bu konuşma öğrenen uygulama için GUID'dir ve `<logDialgoId>` almak istediğiniz günlük iletişim kimliğidir.  

Günlük iletişim kutuları geliştirici tarafından düzenlenen ve iletişim kutuları eğitmek olarak depolanan unutmayın.  Bu yapıldığında, konuşma öğrenen tren iletişim kutusuyla "kaynak" günlük iletişim Kimliğini depolar.  Ayrıca, tren iletişim "kullanıcı Arabiriminde dal"; Tren iletişim bir ilişkili kaynak günlük iletişim kimliği varsa, sonra bu tren iletişim kutusundan dalları aynı günlük iletişim kimlikle işaretlenir

Günlük iletişim kutusundan türetilen tüm tren iletişim kutuları edinmek için şu adımları izleyin.

İlk olarak, tüm tren iletişim kutuları Al:

```
GET https://westus.api.cognitive.microsoft.com/conversationlearner/v1.0/app/<appId>/traindialogs
```

Burada `<appId>` Bu konuşma öğrenen uygulaması için GUID değeridir.  

Bu, tüm tren iletişim kutuları döndürür.  Bu listede ilişkili arama `sourceLogDialogId`ve ilişkili Not `trainDialogId`. 

Tek bir iletişim kutusu kimliği ile eğitmek:

```
GET https://westus.api.cognitive.microsoft.com/conversationlearner/v1.0/app/<appId>/traindialog/<trainDialogId>
```

Burada `<appId>` Bu konuşma öğrenen uygulama için GUID'dir ve `<trainDialogId>` almak istediğiniz tren iletişim kimliğidir.  

## <a name="how-to-delete-a-logged-dialog"></a>Günlüğe kaydedilmiş bir iletişim kutusu silme

Kimliğini verilen günlük iletişim silmek isterseniz, bu HTTP çağrı kullanabilirsiniz:

```
DELETE https://westus.api.cognitive.microsoft.com/conversationlearner/v1.0/app/<appId>/logdialog/<logDialogId>
```

Burada `<appId>` Bu konuşma öğrenen uygulama için GUID'dir ve `<logDialogId>` silmek istediğiniz günlük iletişim kimliğidir. 

Kimliğini verilen tren iletişim silmek isterseniz, bu HTTP çağrı kullanabilirsiniz:

```
DELETE https://westus.api.cognitive.microsoft.com/conversationlearner/v1.0/app/<appId>/traindialog/<trainDialogId>
```

Burada `<appId>` Bu konuşma öğrenen uygulama için GUID'dir ve `<trainDialogId>` silmek istediğinizden tren iletişim kimliğidir. 
