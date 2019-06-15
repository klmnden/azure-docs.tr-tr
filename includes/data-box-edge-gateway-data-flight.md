---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/16/2019
ms.author: alkohli
ms.openlocfilehash: acf1195616d45b155445604ef0204528e5f7ca03
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "67120542"
---
Uçuş verileri için:

- Standart TLS 1.2 cihaz ve Azure arasında geçen verileri için kullanılır. TLS 1.1 ve önceki sürümleri geri dönüş yoktur. Aracı iletişimi, TLS 1.2 desteklenmiyorsa engellenir. TLS 1.2 portal ve SDK yönetimi için de gereklidir.
- İstemciler, cihazınızın yerel web kullanıcı Arabiriminde bir tarayıcı eriştiğinizde, standart TLS 1.2 varsayılan güvenli protokol olarak kullanılır.

    - TLS 1.2 kullanmak için tarayıcınızı yapılandırmak için en iyi yöntem olacaktır.
    - TLS 1.2 tarayıcı desteklemiyorsa, TLS 1.1 veya TLS 1.0 kullanabilirsiniz.
- Veri sunucularınızdan kopyalarken verilerini korumak için şifreleme ile SMB 3.0 kullanmanızı öneririz.
