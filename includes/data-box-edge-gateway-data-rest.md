---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/16/2019
ms.author: alkohli
ms.openlocfilehash: 8a6ceee0f7db4b7b368dce1a28c8edc0b2b11180
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60754330"
---
Bekleyen veriler için:

- BitLocker XTS-AES 256 bit şifrelemeyi, yerel verileri korumak için kullanılır.
- Paylaşımlarında depolanan verilere erişimi sınırlıdır.

    - Paylaşım verilere erişen SMB istemcileri paylaşımıyla ilişkili kullanıcı kimlik bilgileri gerekir. Bu kimlik bilgileri, paylaşımı oluşturulurken tanımlanır.
    - Bir paylaşıma erişim NFS istemcilerinin IP adreslerini paylaşım oluşturulduğunda eklenmesi gerekir.