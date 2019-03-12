---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/05/2019
ms.author: alkohli
ms.openlocfilehash: c689dc4f7e0957218d6504532c4b481d7673def8
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2019
ms.locfileid: "57555278"
---
Uygun bir SSL sertifikası, doğru sunucuya şifreli bilgiler göndererek sağlar. Şifreleme yanı sıra, sertifika kimlik doğrulaması için de sağlar. Cihazın PowerShell arabirimi üzerinden kendi güvenilen SSL sertifikanızı karşıya yükleyebilirsiniz.

1. [PowerShell arabirimine bağlanma](#connect-to-the-powershell-interface).
2. Kullanım `Set-HcsCertificate` sertifikayı karşıya yüklemek için cmdlet'i. İstendiğinde aşağıdaki parametreleri girin:

    - `CertificateFilePath` -Path, sertifika dosyasının bulunduğu paylaşıma *.pfx* biçimi.
    - `CertificatePassword` -Sertifikayı korumak için kullanılan bir parola.
    - `Credentials` -Kullanıcı adı ve parola sertifikasını içeren paylaşımına erişmek için.

    Aşağıdaki örnek, bu cmdlet kullanımını gösterir:

    ```
    Set-HcsCertificate -Scope LocalWebUI -CertificateFilePath "\\myfileshare\certificates\mycert.pfx" -CertificatePassword "mypassword" -Credentials "Username/Password"
    ```

