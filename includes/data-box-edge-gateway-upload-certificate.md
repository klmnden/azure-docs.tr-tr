---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/05/2019
ms.author: alkohli
ms.openlocfilehash: 216380cf7069468d13c4e533fc90b2596aa211c4
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58125271"
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

