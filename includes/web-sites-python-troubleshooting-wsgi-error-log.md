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
ms.openlocfilehash: 5169432af65a046465c9d1ced349687d6fdc8bb7
ms.sourcegitcommit: caebf2bb2fc6574aeee1b46d694a61f8b9243198
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/12/2018
ms.locfileid: "35394035"
---
Python uygulamanız başlatılırken bir hatayla karşılaştığında, yalnızca basit bir hata sayfası (örn. döndürülür "Bir iç sunucu hatası oluştuğundan sayfası görüntülenemiyor.").

Python uygulama hatalarını yakalamak için:

1. Azure portalında, web uygulamanızı seçin **ayarları**.
2. Üzerinde **ayarları** sekmesinde **uygulama ayarları**.
3. Altında **uygulama ayarları**, aşağıdaki anahtar/değer çifti girin:
    * Anahtar: WSGI_LOG
    * Değer: D:\home\site\wwwroot\logs.txt (dosya adı tercih ettiğiniz girin)

Artık wwwroot klasörü logs.txt dosyasında hatalar görmeniz gerekir.
