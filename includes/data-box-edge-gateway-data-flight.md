---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/16/2019
ms.author: alkohli
ms.openlocfilehash: bea14544949f90290bf3878295b49f1e08be9eea
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59684483"
---
Veri-de-uçuş için:

- Cihaz ve Azure arasında veri standart TLS 1.2 kullanılır. TLS 1.1 ve önceki sürümleri geri dönüş yoktur. Aracı iletişimi, TLS 1.2 desteklenmiyorsa engellenir. TLS 1.2 portal ve SDK'sı yönetim işlemleri için de gereklidir.
- İstemciler bir tarayıcıda yerel web kullanıcı Arabirimi üzerinden Cihazınızı eriştiğinizde, standart TLS 1.2 varsayılan güvenli protokol olarak kullanılır.

    - TLS 1.2 kullanmak için tarayıcınızı yapılandırmak için en iyi yöntem olacaktır.
    - Tarayıcı TLS 1.2 desteklemiyorsa, TLS 1.1 veya TLS 1.0 kullanabilirsiniz.
- Veri sunucularınızdan kopyaladığınızda verilerini korumak için SMB 3.0 şifreleme ile kullanmanızı öneririz.
