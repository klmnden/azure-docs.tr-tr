---
title: include dosyası
description: include dosyası
services: digital-twins
author: kingdomofends
ms.service: digital-twins
ms.topic: include
ms.date: 01/02/2019
ms.author: adgera
ms.custom: include file
ms.openlocfilehash: 1c6579776b86decb78c172578cbe55a66c05d78f
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54026562"
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

Çok bölümlü istekleri program aracılığıyla yapılabilir (aracılığıyla C#), bir REST istemcisi veya gibi aracı üzerinden [Postman](https://www.getpostman.com/). REST istemcisi araçları destek karmaşık çok bölümlü istekleri için değişen düzeylerde olabilir. Hangi aracı ihtiyaçlarınıza en uygun olduğunu doğrulayın.

> [!IMPORTANT]
> Azure dijital İkizlerini yönetim API'leri için çok bölümlü isteklerini iki bölümden oluşur:
> * Tarafından bildirilen blob meta verileri (örneğin, ilişkili bir MIME türü) **Content-Type** ve **Content-Disposition**
> * Karşıya yüklenecek bir dosya yapılandırılmamış içeriğini içeren blob içeriği
>
> İki bölümü hiçbiri için gerekli olan **düzeltme eki** istekleri. Her ikisi için gerekli olan **POST** veya oluşturma işlemleri.
