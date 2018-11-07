---
title: API'SİNİN nasıl kullanılacağı çağıran bir konuşma Öğrenici modeliyle - Microsoft Bilişsel hizmetler | Microsoft Docs
titleSuffix: Azure
description: Konuşma Öğrenici modeliyle API çağrıları kullanmayı öğrenin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 815d1e9f6d1e4b9937647d55b67665e1b27f501e
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51240775"
---
# <a name="how-to-add-api-calls-to-a-conversation-learner-model"></a>Konuşma Öğrenici modeli yapılan API çağrılarının ekleme

Bu öğreticide, API çağrıları, bir Modeli'ne Ekle gösterilmektedir. API çağrılarıdır tanımlayın ve yazma botunuzun, İşlevler ve konuşma Öğrenici çağırabilirsiniz.

## <a name="video"></a>Video

[![Öğretici 12 Önizleme](https://aka.ms/cl-tutorial-12-preview)](https://aka.ms/blis-tutorial-12)

## <a name="requirements"></a>Gereksinimler
Bu öğreticide, "tutorialAPICalls.ts" bot çalışıyor olması gerekir.

    npm run tutorial-api-calls

## <a name="details"></a>Ayrıntılar

- API çağrıları, okuma ve varlıkları işlemek.
- API çağrıları, bellek yöneticisi nesnesine erişebilir.
- API çağrıları bağımsız değişkenler de alabilir; bu farklı amaçlara hizmet için aynı API çağrısı yeniden kullanımına izin verir.

### <a name="open-the-demo"></a>Tanıtım açın

Web kullanıcı Arabirimi modeli listesinde Öğreticisi-12-APICalls tıklayın. 

### <a name="entities"></a>Varlıklar

Bir varlık numarası adlı modelde tanımladığımız.

![](../media/tutorial12_entities.PNG)

### <a name="api-calls"></a>API çağrıları
Bu API çağrısı için kod tanımlanan dosya: C:\<installedpath\>\src\demos\tutorialAPICalls.ts.

![](../media/tutorial12_apicalls.PNG)

- İlk API RandomGreeting aramasıdır. Karşılama değişkeninde tanımlanan rastgele bir karşılama döndürür.
- Çarp API geri çağırma: kullanıcı tarafından sağlanan iki sayının Çarp. Ardından, iki sayının Çarpım sonucunu döndürür. Bu API geri çağırmaları girişleri sürebilir gösterir. Bu bellek yöneticisi ilk bağımsız değişken olduğuna dikkat edin. 
- ClearEntities API geri çağırma: sonraki girin izin vermek için sayı varlık temizler. Bunu, API çağrıları varlıkları nasıl işleyebileceğiniz göstermektedir.

### <a name="actions"></a>Eylemler

Dört eylem oluşturduk. 

![](../media/tutorial12_actions.PNG)

- 'Hangi numarasını, 12 ile çarp istiyorsunuz yanı sıra' hangi communicative eylemi, tipik API çağrısı desenleri göstermek üç farklı API çağrıları vardır.

- RandomGreeting: bekleme olmayan bir işlemdir. Eylem oluşturma iletişim kutusunda, bunu için biz, eylem türü API_LOCAL seçili ardından RandomGreeting seçildi. 

![](../media/tutorial12_setupapicall.PNG)

Bot durdurup herhangi bir değişiklik için API'lerini olsaydık yenile düğmesinin yanındaki API kullanılır. Yenileme sırasında tıklayarak en son değişiklikleri seçiyordu.

İşte oluşturmamıza eylem Çarp: API_Local ve API seçtikten sonra bir varlık ($number) ilk giriş değeri (num1string) ve ikinci giriş değeri (num2string) için bir değer (12) girdiğimiz. Böylece aynı geri çağırma sistemde bazı eylemleri eşlenebilir ve eylemleri nasıl atanacağını farklı API'yi çağıran ve bu bot arasında bir yöneltme düzeyi sağlar.

![](../media/tutorial12_actionmultiply.PNG)

### <a name="train-dialog"></a>Train iletişim

Öğretim iletişim kutusundan atalım.

1. Train iletişim kutuları, ardından yeni Train iletişim tıklayın.
1. 'Merhaba' girin.
2. Puan eylemini tıklatın.
3. RandomGreeting seçmek için tıklayın. Bu rastgele Karşılama API çağrısı yürütülür.
3. 'Hangi sayıya, 12 ile çarp istiyor musunuz?' seçmek için tıklayın
4. '8' girin. Puan Eylemler'ye tıklayın.
4. Seç ' $number Çarp 12'. Çarpma işleminin sonucunu unutmayın.
5. 'Temizle varlıkları' seçin.
    - `number` Varlığın değeri temizlendi.
3. 'Hangi sayıya, 12 ile çarp istiyor musunuz?' seçmek için tıklayın
4. Yapılan test tıklayın.

![](../media/tutorial12_dialog.PNG)

Artık API geri aramaları, kaydetme gördünüz kendi ortak desenler ve bağımsız değişkenleri tanımlayın ve değerleri ve bunları varlıklarda ilişkilendirin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Kartları bölüm 1](./13-cards-1.md)
