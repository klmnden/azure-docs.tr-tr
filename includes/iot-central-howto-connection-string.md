---
title: include dosyası
description: include dosyası
services: iot-central
author: dominicbetts
ms.service: iot-central
ms.topic: include
ms.date: 04/09/2019
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: eae4b1e919270413d4ca6627964fad9c7f5bd9bf
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66151975"
---
1. Kullanım `dps-keygen` bir bağlantı dizesi oluşturmak için komut satırı yardımcı programı:

    Yüklenecek [tuşu Oluşturucu yardımcı programı](https://github.com/Azure/dps-keygen), aşağıdaki komutu çalıştırın:

    ```cmd/sh
    npm i -g dps-keygen
    ```

1. Bir bağlantı dizesi oluşturmak için daha önce not ettiğiniz ilişkin bağlantı bilgilerini kullanarak şu komutu çalıştırın:

    ```cmd/sh
    dps-keygen -di:<Device ID> -dk:<Primary or Secondary Key> -si:<Scope ID>
    ```

1. Bağlantı dizesini kopyalayın `dps-keygen` cihaz kodunuzda kullanmak için çıktı.