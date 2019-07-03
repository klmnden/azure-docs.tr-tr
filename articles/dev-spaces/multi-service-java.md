---
title: Java ve VS Code kullanarak birden çok bağımlı hizmetleri çalıştırma
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
author: zr-msft
ms.author: zarhoads
ms.date: 11/21/2018
ms.topic: tutorial
description: Azure’da kapsayıcılar ve mikro hizmetlerle hızlı Kubernetes geliştirme
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, kapsayıcılar, Helm, hizmet kafes, ağ hizmeti Yönlendirme, kubectl, k8s
ms.openlocfilehash: a93bda3392962a1c35e2bb2433d285ed497075d2
ms.sourcegitcommit: 837dfd2c84a810c75b009d5813ecb67237aaf6b8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67503119"
---
# <a name="multi-service-development-with-azure-dev-spaces"></a>Azure geliştirme alanları ile birden çok hizmet geliştirme

Bu öğreticide, bazı geliştirme alanları sağlayan sağlamasının yanı sıra Azure geliştirme alanları kullanarak, çoklu hizmet uygulamalarını nasıl geliştirebileceğinizi öğreneceksiniz.

## <a name="call-a-service-running-in-a-separate-container"></a>Ayrı kapsayıcıda çalıştırılan hizmeti çağırma

Bu bölümde `mywebapi` adlı ikinci bir hizmet oluşturacak ve bu hizmetin `webfrontend` tarafından çağrılmasını sağlayacaksınız. Her hizmet ayrı kapsayıcılarda çalışır. Ardından her iki kapsayıcıda da hata ayıklayacaksınız.

![Birden çok kapsayıcı](media/common/multi-container.png)

### <a name="download-sample-code-for-mywebapi"></a>*mywebapi* için örnek kod indirme
Zamandan kazanmak adına örnek kodu bir GitHub deposundan indirelim. [https://github.com/Azure/dev-spaces](https://github.com/Azure/dev-spaces ) adresine gidip **Kopyala veya İndir**’i seçerek GitHub deposunu indirin. Bu bölümün kodu `samples/java/getting-started/mywebapi` konumundadır.

### <a name="run-mywebapi"></a>*mywebapi* hizmetini çalıştırın
1. *Ayrı bir VS Code penceresinde* `mywebapi` klasörünü açın.
1. **Komut Paleti**'ni açın (**Görünüm | Komut Paleti** menüsünü kullanarak) ve otomatik tamamlama özelliğini kullanarak komutu yazın ve seçin: `Azure Dev Spaces: Prepare configuration files for Azure Dev Spaces`.
1. F5'e bastıktan sonra hizmetin oluşturulup dağıtılmasını bekleyin. Bunun bir ileti olduğunda benzer hazır olduğunu bilmeniz hata ayıklama konsolunda aşağıda görünür:

    ```cmd
    2019-03-11 17:02:35.935  INFO 216 --- [           main] com.ms.sample.mywebapi.Application       : Started Application in 8.164 seconds (JVM running for 9.272)
    ```

1. Uç nokta URL'si `http://localhost:<portnumber>` benzeri bir URL olacaktır. **İpucu: VS Code durum çubuğunda turuncu açmak ve tıklanabilir URL'sini görüntüler.** Kapsayıcı yerel olarak çalışıyor gibi görünebilir, ancak gerçekte Azure’daki geliştirme ortamımızda çalışıyordur. Bunun localhost adresi olmasının nedeni `mywebapi` hizmetinin hiçbir genel uç nokta tanımlamamış olması ve buna yalnızca Kubernetes örneğinin içinden erişilebilmesidir. Size rahatlık sağlamak ve yerel makinenizden özel hizmetle etkileşimi kolaylaştırmak için, Azure Dev Spaces Azure'da çalıştırılan kapsayıcıya geçici bir SSH tüneli oluşturur.
1. `mywebapi` hazır olduğunda, tarayıcınızda localhost adresini açın.
1. Tüm adımları başarılı olduysa, `mywebapi` hizmetinden bir yanıt alındığını görebilmelisiniz.

### <a name="make-a-request-from-webfrontend-to-mywebapi"></a>*webfrontend*’den *mywebapi*’ye istek gönderme
Şimdi `webfrontend` uygulamasında `mywebapi` hizmetine istek gönderen bir kod yazalım.
1. `webfrontend` için VS Code penceresine geçin.
1. Aşağıdaki `import` deyimlerini `package` deyimi altına *ekleyin*:

   ```java
   import java.io.*;
   import java.net.*;
   ```
1. Karşılama yönteminin kodunu *değiştirin*:

    ```java
    @RequestMapping(value = "/greeting", produces = "text/plain")
    public String greeting(@RequestHeader(value = "azds-route-as", required = false) String azdsRouteAs) throws Exception {
        URLConnection conn = new URL("http://mywebapi/").openConnection();
        conn.setRequestProperty("azds-route-as", azdsRouteAs); // propagate dev space routing header
        try (BufferedReader reader = new BufferedReader(new InputStreamReader(conn.getInputStream())))
        {
            return "Hello from webfrontend and " + reader.lines().reduce("\n", String::concat);
        }
    }
    ```

Önceki kod örneğinde `azds-route-as` üst bilgisi gelen istekten giden isteğe iletilmektedir. Daha sonra bunun takımlara işbirliğine dayalı geliştirme açısından nasıl yardımcı olduğunu göreceksiniz.

### <a name="debug-across-multiple-services"></a>Birden çok hizmette hata ayıklama
1. Bu noktada, `mywebapi` hizmetinin hata ayıklayıcısı ekli bir şekilde çalışmaya devam ediyor olması gerekir. Devam etmiyorsa, `mywebapi` projesinde F5'e basın.
1. Bir kesim noktası `index()` yöntemi `mywebapi` projesi [satır 19 `Application.java`](https://github.com/Azure/dev-spaces/blob/master/samples/java/getting-started/mywebapi/src/main/java/com/ms/sample/mywebapi/Application.java#L19)
1. `webfrontend` projesinde, `mywebapi` konumuna GET isteği göndermeden hemen önce, `try` ile başlayan satırda bir kesme noktası ayarlayın.
1. `webfrontend` projesinde F5’e basın (veya o sırada çalışıyorsa, hata ayıklayıcıyı yeniden başlatın).
1. Web uygulamasını çağırın ve her iki hizmette de kodun üzerinden geçin.
1. Web uygulamasında hakkında sayfasında iki hizmet tarafından birleştirilmiş bir ileti görüntülenir: "Hello webfrontend ve Hello mywebapi gelen."

### <a name="well-done"></a>Bravo!
Artık her kapsayıcının ayrı ayrı geliştirilip dağıtılabileceği çok kapsayıcılı bir uygulamanız var.


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Takım geliştirme geliştirme alanları hakkında bilgi edinin](team-development-java.md)
