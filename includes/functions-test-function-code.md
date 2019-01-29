---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: 914c006daf49e22ebec870a549bfdbc63f882647
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2019
ms.locfileid: "55148020"
---
## <a name="test"></a>İşlevi test etme

Dağıtılan işlevi bir Mac veya Linux bilgisayarının veya Windows üzerinde Powershell kullanarak test etmek için cURL kullanın. `<app_name>` yer tutucusunu işlev uygulamanızın adıyla değiştirerek aşağıdaki cURL komutunu yürütün. `&name=<yourname>` sorgu dizesini URL’ye ekleyin.

```powershell
Invoke-WebRequest -Uri "https://<app_name>.azurewebsites.net/api/MyHttpTrigger?name=<yourname>"
```

```bash
curl https://<app_name>.azurewebsites.net/api/MyHttpTrigger?name=<yourname>
```  

![Tarayıcıda gösterilen işlev yanıtı.](./media/functions-test-function-code/functions-azure-cli-function-test-curl.png)  

Öğeniz yoksa `cURL`veya `Invoke-WebRequest` kullanılabilir komut satırınızda web tarayıcınızın adres aynı URL'yi girin. Burada da `<app_name>` yer tutucusunu işlev uygulamanızın adıyla değiştirin ve `&name=<yourname>` sorgu dizesini URL’ye ekleyip isteği yürütün.

    https://<app_name>.azurewebsites.net/api/MyHttpTrigger?name=<yourname>
   
![Tarayıcıda gösterilen işlev yanıtı.](./media/functions-test-function-code/functions-azure-cli-function-test-browser.png)  
