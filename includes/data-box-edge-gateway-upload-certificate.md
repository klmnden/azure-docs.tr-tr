---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/23/2019
ms.author: alkohli
ms.openlocfilehash: d7a9923d5bd9e357bcd75fae6e0a7d1bcd437a53
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64732651"
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

