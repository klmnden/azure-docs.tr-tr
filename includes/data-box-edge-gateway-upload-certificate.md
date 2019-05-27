---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/23/2019
ms.author: alkohli
ms.openlocfilehash: d7a9923d5bd9e357bcd75fae6e0a7d1bcd437a53
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66161159"
---
Uygun bir SSL sertifikası, doğru sunucuya şifreli bilgiler göndererek sağlar. Şifreleme yanı sıra, sertifika kimlik doğrulaması için de sağlar. Cihazın PowerShell arabirimi üzerinden kendi güvenilen SSL sertifikanızı karşıya yükleyebilirsiniz.

1. [PowerShell arabirimine bağlanma](#connect-to-the-powershell-interface).
2. Kullanım `Set-HcsCertificate` sertifikayı karşıya yüklemek için cmdlet'i. İstendiğinde aşağıdaki parametreleri girin:

   - `CertificateFilePath` -Path, sertifika dosyasının bulunduğu paylaşıma *.pfx* biçimi.
   - `CertificatePassword` -Sertifikayı korumak için kullanılan bir parola.
   - `Credentials` -Kullanıcı adı ve parola sertifikasını içeren paylaşımına erişmek için.

     Aşağıdaki örnek, bu cmdlet kullanımını gösterir:

     ```
     Set-HcsCertificate -Scope LocalWebUI -CertificateFilePath "\\myfileshare\certificates\mycert.pfx" -CertificatePassword "mypassword" -Credential "Username/Password"
     ```

