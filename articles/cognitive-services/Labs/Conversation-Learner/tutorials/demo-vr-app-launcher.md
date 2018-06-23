---
title: Konuşma öğrenen uygulama, sanal gerçekte uygulama Başlatıcısı - Microsoft Bilişsel hizmetler gösteri | Microsoft Docs
titleSuffix: Azure
description: Bir tanıtım konuşma öğrenen uygulama oluşturmayı öğrenin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 3e41125bf7da9ee64d666d22cb275af01af54012
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354071"
---
# <a name="demo-virtual-reality-app-launcher"></a>Gösteri: Sanal gerçekte uygulama Başlatıcısı

Bu demo "Skype başlatın ve duvarında koyun."gibi komutları destekleyen bir sanal gerçekte uygulama Başlatıcısı gösterir Bir kullanıcı, uygulamayı başlatmak için bir uygulama adı ve konumu söyleyin gerekiyor. Uygulamasını başlatarak gerçekleştirilir bir API çağrısı. Kullanıcıdan bir uygulama adı tanınmıyor, entityDetectionCallback istenen uygulama yüklü uygulamalar listesinde bir veya daha fazla uygulama eşleşip eşleşmediğini denetler. İstenen uygulama yüklü değil ve uygulama adı belirsiz olduğu durum işleme (birden fazla yüklü uygulamayı eşleşir).

## <a name="requirements"></a>Gereksinimler

Bu öğretici VRAppLauncher bot çalışıyor olması gerekir

    npm run demo-vrapp

### <a name="open-the-demo"></a>Tanıtım açın

Web kullanıcı Arabirimi uygulama listesinde üzerinde VRAppLauncher tıklayın. 

## <a name="entities"></a>Varlıklar

Dört varlıklar oluşturduk:

- AppName: Skype Örneğin
- PlacementLocation: örnek duvar için
- UnknownAppName: yüklü kurmadı için bir varlık adı tanımıyor yükleyen sistem ayarlar programlı bir varlık kullanıcı, örneğin söyler.
- DisAmbigAppNames: kullanıcının eşleşen iki veya daha fazla yüklü uygulamayı adlarının dizisini belirtti. 

![](../media/tutorial_vrapplauncher_entities.PNG)

### <a name="actions"></a>Eylemler

Bir uygulamayı başlatmak için işlev çağrısı başlayacak LaunchApp adlı bir API içeren bir dizi eylemi oluşturduk.

![](../media/tutorial_vrapplauncher_actions.PNG)

### <a name="training-dialogs"></a>Eğitim iletişim kutuları
Biz eğitim iletişim kutuları sayısı tanımladınız.

![](../media/tutorial_vrapplauncher_dialogs.PNG)

Örnek olarak, bir öğretme oturumu deneyelim.

1. Tren iletişim kutuları, ardından yeni tren iletişim'ı tıklatın.
1. 'Merhaba' girin.
2. Puan eylemini tıklatın.
3. 'Hangi uygulamanın, başlatmak istiyor musunuz?' seçmek için tıklatın
4. 'Outlook' girin.
    - HALUK bir varlık olarak tanıdığı olduğunu unutmayın.
5. Puan Eylemler'i tıklatın.
3. 'Yerleştirilen istediğiniz yere?' seçmek için tıklatın
4. 'Duvarında' girin.
    - HALUK PlacementLocation tanıdığı olduğunu unutmayın.
2. Puan Eylemler girin.
6. 'LaunchApp' seçin
7. Sistem: 'outlook duvarında başlatılıyor'.
    - Bu API çağrısı tetiklenen unutmayın. Bu çağrı sırasında C: kodudur\<\installedpath > \src\demos\demoVRAppLauncher.ts. Bu Tanıtım için outlook ancak başlatmak için mantığı gerçekten yok.
    - AppName ve PlacementLocation varlıkları temizler. Döndürür yanıt olarak yukarıdaki dize.
4. Öğretme Bitti'yi tıklatın.

![](../media/tutorial_vrapplauncher_callbackcode.PNG)

Bilinmeyen ve belirsiz varlıkları işlemek için başka bir eğitim oturumu başlayalım.

1. Yeni tren iletişim'ı tıklatın.
1. 'OneNote start' girin. 
    - Ad kullanıcı geçtiğini ve kod içinde tanımlanan uygulama listesinde eşleştirerek bir uygulama adına çözümler kod alır EntityDetectionCallback tanımlı olduğundan, bir uygulama adı olarak tanır. Ardından, tüm eşleşen uygulamalar kümesini döndürür. 
    - Eşleşmeleri listesinde sıfır ise, bu uygulama yüklü değil anlamına gelir. Bu unknownAppName koyar.
    - Birden fazla uygulama bulursa DisambigAppNames kopyalayın ve AppName varlık temizleyin.
2. Puan eylemini tıklatın.
3. 'Ne yazık ki $UknownAppName uygulama bilmiyorsanız.' seçmek için tıklatın
4. 'Amazon start' girin. Biz başka bir yol deneyeceksiniz.
5. Puan Eylemler'i tıklatın.
    - Not Amazon Video ve Amazon müzik DisambigAppNames bellekte sunulmuştur. Ve OneNote temizlendi.
3. 'Gibi ses... birkaç uygulamalar vardır' Select için tıklatın
    - Biz yalnızca bu noktaya kadar birkaç eğitim iletişim kutuları olduğundan puanı çok yüksek olmadığını unutmayın. Biz bazı şuradan daha fazla model daha belirleyici yapmanız gibi görünüyor.
2. Puan Eylemler girin.
4. Öğretme Bitti'yi tıklatın.

Şimdi varlık çözümlemesi yapmak öğrendiniz. Tanıtım ayrıca API geri aramalar gösterilmiştir ve bilgilerini toplama, varlığı ve belirsizlik denetleniyor ve oturum tabanlı sağ eylemin gerçekleştirilmesi için bir şablon gösterdi.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Konuşma öğrenen bot dağıtma](../deploy-to-bf.md)
