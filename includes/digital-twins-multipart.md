---
title: include dosyası
description: include dosyası
services: digital-twins
author: kingdomofends
ms.service: digital-twins
ms.topic: include
ms.date: 01/11/2019
ms.author: adgera
ms.custom: include file
ms.openlocfilehash: ac6b008597b6d6e557a0cc412c00c2202231bc3d
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67189011"
---
> [!NOTE]
> Çok parçalı istek, genellikle üç parça gerektirir:
> * A **Content-Type** üst bilgi:
>   * `application/json; charset=utf-8`
>   * `multipart/form-data; boundary="USER_DEFINED_BOUNDARY"`
> * A **içerik düzeni**:
>   * `form-data; name="metadata"`
> * Karşıya yüklenecek dosya içeriği
>
> **İçerik türü** ve **Content-Disposition** kullanım senaryosuna bağlı olarak farklılık gösterir.

Çok bölümlü istekleri program aracılığıyla yapılabilir (aracılığıyla C#), bir REST istemcisi veya gibi aracı üzerinden [Postman](https://docs.microsoft.com/azure/digital-twins/how-to-configure-postman#multi). REST istemcisi araçları destek karmaşık çok bölümlü istekleri için değişen düzeylerde olabilir. Yapılandırma ayarları da biraz gelen için aracı farklılık gösterebilir. Hangi aracı ihtiyaçlarınız için en uygun olduğunu doğrulayın.

> [!IMPORTANT]
> Azure dijital İkizlerini yönetim API'leri için genellikle yaptığınız çok parçalı istek, iki bölümden oluşur:
> * Tarafından bildirilen blob meta verileri (örneğin, ilişkili bir MIME türü) **Content-Type** ve/veya **Content-Disposition**
> * Karşıya yüklenecek bir dosya yapılandırılmamış içeriğini içeren blob içeriği
>
> İki bölümü hiçbiri için gerekli olan **düzeltme eki** istekleri. Her ikisi için gerekli olan **POST** veya oluşturma işlemleri.

[Doluluk Hızlı Başlangıç kaynak kodu](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/api/update.cs) tamamını içerir C# Azure dijital İkizlerini yönetim API'leri çok bölümlü istekler yapma gösteren örnekler.