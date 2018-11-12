---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: ae88a6132c79d9d9f930f6445e53b0f0452c9f2a
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51262616"
---
## <a name="test"></a>İşlevi test etme

Dağıtılan işlevi bir Mac veya Linux bilgisayarda ya da Windows üzerinde Bash kullanarak test etmek için cURL kullanın. `<app_name>` yer tutucusunu işlev uygulamanızın adıyla değiştirerek aşağıdaki cURL komutunu yürütün. `&name=<yourname>` sorgu dizesini URL’ye ekleyin.

```bash
curl https://<app_name>.azurewebsites.net/api/MyHttpTrigger?name=<yourname>
```  

![Tarayıcıda gösterilen işlev yanıtı.](./media/functions-test-function-code/functions-azure-cli-function-test-curl.png)  

Komut satırınızda cURL yoksa, web tarayıcınızın adres çubuğuna aynı URL'yi girin. Burada da `<app_name>` yer tutucusunu işlev uygulamanızın adıyla değiştirin ve `&name=<yourname>` sorgu dizesini URL’ye ekleyip isteği yürütün.

    https://<app_name>.azurewebsites.net/api/MyHttpTrigger?name=<yourname>
   
![Tarayıcıda gösterilen işlev yanıtı.](./media/functions-test-function-code/functions-azure-cli-function-test-browser.png)  
