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
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61270210"
---
Python uygulamanız başlatılırken bir hatayla karşılaştığında, yalnızca basit bir hata sayfası (örn. döndürülür "Bir iç sunucu hatası oluştuğundan sayfası görüntülenemiyor.").

Python uygulama hatalarını yakalamak için:

1. Azure portalında, web uygulamanızı seçin **ayarları**.
2. Üzerinde **ayarları** sekmesinde **uygulama ayarları**.
3. Altında **uygulama ayarları**, aşağıdaki anahtar/değer çifti girin:
    * Anahtar: WSGI_LOG
    * Değer: D:\home\site\wwwroot\logs.txt (dosya adı tercih ettiğiniz girin)

Artık wwwroot klasörü logs.txt dosyasında hatalar görmeniz gerekir.
