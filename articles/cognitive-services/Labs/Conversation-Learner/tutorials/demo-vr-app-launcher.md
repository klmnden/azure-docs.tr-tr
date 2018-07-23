---
title: Konuşma Öğrenici model, sanal gerçeklik uygulama Başlatıcısı - Microsoft Bilişsel hizmetler deneme | Microsoft Docs
titleSuffix: Azure
description: Bir tanıtım konuşma Öğrenici modeli oluşturmayı öğrenin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 896ec007c03e30e5c20a5344430be040271bc00b
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39171159"
---
# <a name="demo-virtual-reality-app-launcher"></a>Demo: Sanal gerçeklik uygulama Başlatıcısı

Bu Tanıtım "nu duvara yansıtabilir yerleştirmenizi ve Skype'ı başlatın." gibi komutları destekleyen bir sanal gerçeklik modeli Başlatıcısı gösterilmektedir. Bir kullanıcının uygulamayı başlatmak için bir uygulama adı ve konumu söylemeniz gerekir. Model başlatılıyor işlenir bir API çağrısı. Bir uygulama adı kullanıcıdan tanındığında entityDetectionCallback istenen uygulamanın yüklü uygulamalar listesinde bir veya daha fazla uygulama eşleşip eşleşmediğini denetler. İstenen uygulama yüklü değil ve uygulama adı belirsiz olduğu durumu işler (yüklü birden fazla uygulama eşleşir).

## <a name="video"></a>Video

[![Tanıtım VR uygulama önizlemesi](http://aka.ms/cl-demo-vrapp-preview)](http://aka.ms/blis-demo-vrapp)

## <a name="requirements"></a>Gereksinimler

Bu öğreticide, VRAppLauncher bot çalışıyor olması gerekir:

    npm run demo-vrapp
    
### <a name="open-the-demo"></a>Tanıtım açın

Web kullanıcı Arabirimi Model listesi üzerinde VRAppLauncher tıklayın. 

## <a name="entities"></a>Varlıklar

Dört varlıkları oluşturduk:

- AppName: Örneğin Skype
- PlacementLocation: için örnek duvar
- UnknownAppName: çünkü yüklü olmayan bir varlık adı tanıyamadığı yükleyen sistem ayarlar programlı bir varlık kullanıcı, örneğin diyor.
- DisAmbigAppNames: bir dizi kullanıcının eşleşen iki veya daha fazla yüklü uygulama adını belirtti. 

![](../media/tutorial_vrapplauncher_entities.PNG)

### <a name="actions"></a>Eylemler

Bir uygulamayı başlatmak için işlev çağrısı başlayacak LaunchApp, adlı bir API'yi içeren bir dizi eylemi oluşturduk.

![](../media/tutorial_vrapplauncher_actions.PNG)

### <a name="training-dialogs"></a>Eğitim iletişim kutuları
Eğitim iletişim kutuları bir dizi tanımladığımız.

![](../media/tutorial_vrapplauncher_dialogs.PNG)

Örneğin, öğretim oturumu deneyelim.

1. Train iletişim kutuları, ardından yeni Train iletişim tıklayın.
1. 'Merhaba' girin.
2. Puan eylemini tıklatın.
3. 'Hangi uygulama, başlatmak istiyor musunuz?' seçmek için tıklayın
4. 'Outlook' girin.
    - LUIS bir varlık olarak tanır.
5. Puan eylemleri tıklayın.
3. 'Yerleştirilen istediğiniz?' seçmek için tıklayın
4. 'Nu duvara yansıtabilir' girin.
    - LUIS bir PlacementLocation tanır.
2. Puan Eylemler girin.
6. 'LaunchApp' seçin
7. Sistem: 'nu duvara outlook başlatılıyor'.
    - Bu API çağrısı tetiklenir. Bu çağrı sırasında C: kodudur\<\installedpath > \src\demos\demoVRAppLauncher.ts. Ancak, bu tanıtım için Outlook'u başlatmak için kodu gerçekten içermiyor.
    - Bu AppName ve PlacementLocation varlıkları temizler. Döndürür yanıt olarak yukarıdaki dize.
4. Öğretim Bitti'ye tıklayın.

![](../media/tutorial_vrapplauncher_callbackcode.PNG)

Bilinmeyen ve belirsiz varlıkları işlemek için başka bir eğitim oturumu başlayalım.

1. Yeni Train iletişim tıklayın.
1. 'OneNote start' girin. 
    - Model OneNote bir uygulama adı tanır. `EntityDetectionCallback` İşlevi kod içinde tanımlanan kod içinde tanımlanan uygulama listesini eşleştirerek için bir uygulama adı kullanıcı tarafından girilen ad giderir. Ardından, tüm eşleşen uygulamalar kümesini döndürür. 
    - Eşleşme listesini sıfır ise, bu uygulama yüklenmemiş anlamına gelir. Bunu unknownAppName yerleştirir.
    - Birden fazla uygulama bulur, bunlara kopyalar `DisambigAppNames` ve AppName varlık temizler.
2. Puan eylemini tıklatın.
3. 'Ne yazık ki uygulama $UknownAppName bilmiyorum.' seçmek için tıklayın
4. 'Amazon start' girin. Biz, başka bir yol denenir.
5. Puan eylemleri tıklayın.
    - Amazon Video ve Amazon müzik bulunduğunuz artık `DisambigAppNames` bellek ve OneNote temizlendi.
3. İçin seçin 'Olan birkaç uygulamalar gibi ses...' tıklayın
    - Puan biz yalnızca bu noktaya kadar birkaç eğitim iletişim kutuları tanımladığınız çok yüksek değil. Daha fazla eğitim iletişim kutularını tanımlayan modeli daha belirleyici hale getirir.
2. Puan Eylemler girin.
4. Öğretim Bitti'ye tıklayın.

Artık varlık çözümleme yapmak öğrendiniz. Tanıtım API geri çağırmaları gösterilen ve bilgi toplama, durum ve belirsizlik denetimi ve doğru buna göre eylemde için bir şablon gösterdi.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Konuşma Öğrenici bot dağıtma](../deploy-to-bf.md)
