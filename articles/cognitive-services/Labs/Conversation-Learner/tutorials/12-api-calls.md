---
title: API kullanın kullanma çağıran bir konuşma öğrenen uygulamayla - Microsoft Bilişsel hizmetler | Microsoft Docs
titleSuffix: Azure
description: Kullanım API çağrılarının bir konuşma öğrenen uygulaması ile nasıl kullanılacağını öğrenin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: ec752cbadfac7a47e08ed7b0ffe8bb475969fac5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354035"
---
# <a name="how-to-add-api-calls-to-a-conversation-learner-application"></a>API çağrıları bir konuşma öğrenen uygulamasına ekleme

Bu öğreticide, API çağrıları, uygulamanıza eklemek gösterilmiştir. API çağrıları olan tanımlayan ve yazma, bot, İşlevler ve konuşma öğrenen çağırabilirsiniz.

## <a name="requirements"></a>Gereksinimler
Bu öğretici "tutorialAPICalls.ts" bot çalışıyor olması gerekir.

    npm run tutorial-api-calls

## <a name="details"></a>Ayrıntılar

- API çağrıları, okuma ve varlıkları arabirimidir.
- API çağrıları Bellek Yöneticisi nesnesi erişimi.
- API çağrıları da bağımsız değişkenler almayan--bu yeniden farklı amaçlara hizmet için aynı API çağrısı kullanarak sağlar.

### <a name="open-the-demo"></a>Tanıtım açın

Web kullanıcı Arabirimi uygulama listesinde Öğreticisi-12-APICalls tıklayın. 

### <a name="entities"></a>Varlıklar

Biz, tek bir varlık numarası olarak adlandırılan uygulamada tanımladınız.

![](../media/tutorial12_entities.PNG)

### <a name="api-calls"></a>API çağrıları
Bu kod API çağrıları için tanımlanan dosya: C:\<installedpath\>\src\demos\tutorialAPICalls.ts.

![](../media/tutorial12_apicalls.PNG)

- İlk API RandomGreeting aramasıdır. Karşılama değişkeninde tanımlanan rastgele bir karşılama döndürür.
- Çarp API geri çağırma: kullanıcı tarafından sağlanan iki sayının Çarp. Ardından, iki sayının Çarpım sonucunu döndürür. Bu API geri aramalar girişleri sürebilir gösterir. Bu bellek yöneticisi ilk bağımsız değişken olduğuna dikkat edin. 
- ClearEntities API geri çağırma: sonraki sayıyı girin kullanıcı izin vermek için sayı varlık temizler. Bu API çağrıları varlıkları nasıl işleyebileceğiniz gösterilmektedir.

### <a name="actions"></a>Eylemler

Dört eylem oluşturduk. 

![](../media/tutorial12_actions.PNG)

- 'Hangi numarasını 12 tarafından çarpılacağı istiyorsunuz yanı sıra?' communicative eylem olduğu, tipik API çağrısı desenlerini göstermek üç farklı API çağrıları vardır.

- RandomGreeting: bekleme olmayan bir eylemdir. Bu, oluşturma eylem iletişim kutusunda ayarlamak için biz, eylem türü API_LOCAL seçili sonra RandomGreeting seçili. 

![](../media/tutorial12_setupapicall.PNG)

Bot durdurmak ve herhangi bir değişiklik için API'ları için olsaydı API yanındaki Yenile düğmesini kullanılır. Üzerinde yenileme tıklatarak en son değişiklikleri almak.

İşte oluşturmamıza eylem Çarp: API_Local ve API seçtikten sonra bir varlık ($number) ilk giriş değerini (num1string) ve ikinci giriş değeri (num2string) için bir değer (12) girdiğimiz. Bu düzeyi bot arasında yönlendirme sağlar ve böylece aynı geri çağırma sistemde birkaç Eylemler eşlenebilir ve bunların eylemleri nasıl atanacağını farklı API çağırır.

![](../media/tutorial12_actionmultiply.PNG)

### <a name="train-dialog"></a>Tren iletişim

Şimdi öğretme iletişim kutusundan yol.

1. Tren iletişim kutuları, ardından yeni tren iletişim'ı tıklatın.
1. 'Merhaba' girin.
2. Puan eylemini tıklatın.
3. RandomGreeting seçmek için tıklatın. Bu rastgele selamlama API çağrısı yürütülür.
3. 'Hangi numarasını, 12 ile çarpın istiyor musunuz?' seçmek için tıklatın
4. '8' girin. Puan eylemleri'ye tıklayın.
4. Seç ' $number Çarp 12'. Çarpım sonucunu unutmayın.
5. 'NET varlıkları' seçin.
    - Sayı varlığın değeri temizlenmiş unutmayın.
3. 'Hangi numarasını, 12 ile çarpın istiyor musunuz?' seçmek için tıklatın
4. Done sınama'ı tıklatın.

![](../media/tutorial12_dialog.PNG)

API geri aramaları kaydetme şimdi gördünüz kendi ortak desenler ve bağımsız değişkenler ve değerleri ve bunlara varlıkları ilişkilendirme nasıl tanımlanacağı.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Kartlar bölüm 1](./13-cards-1.md)
