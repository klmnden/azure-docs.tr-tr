---
title: include dosyası
description: include dosyası
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 06/11/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 020e59f029b09f3c7656f67039731e4141e68d31
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66137026"
---
Python uygulamanız başlatılırken bir hatayla karşılaştığında, yalnızca basit bir hata sayfası (örn. döndürülür "Bir iç sunucu hatası oluştuğundan sayfası görüntülenemiyor.").

Python uygulama hatalarını yakalamak için:

1. Azure portalında, web uygulamanızı seçin **ayarları**.
2. Üzerinde **ayarları** sekmesinde **uygulama ayarları**.
3. Altında **uygulama ayarları**, aşağıdaki anahtar/değer çifti girin:
    * Anahtar: WSGI_LOG
    * Değer: D:\home\site\wwwroot\logs.txt (dosya adı tercih ettiğiniz girin)

Artık wwwroot klasörü logs.txt dosyasında hatalar görmeniz gerekir.
