---
title: "Azure işlevleri kullanarak sunucusuz bir API oluşturma | Microsoft Docs"
description: "Azure işlevleri kullanarak sunucusuz bir API oluşturma"
services: functions
author: mattchenderson
manager: cfowler
ms.service: functions
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: tutorial
ms.date: 05/04/2017
ms.author: mahender
ms.custom: mvc
ms.openlocfilehash: 630d9022da0d51e533534ea43f50f27e8eb09a78
ms.sourcegitcommit: ce934aca02072bdd2ec8d01dcbdca39134436359
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2017
---
# <a name="create-a-serverless-api-using-azure-functions"></a>Azure işlevleri kullanarak sunucusuz bir API oluşturma

Bu öğreticide, Azure işlevlerinin nasıl yüksek oranda ölçeklenebilir API'leri oluşturmanıza olanak sağlayan bilgi edineceksiniz. Azure işlevleri ile birlikte gelen yerleşik HTTP tetikleyiciler ve bir uç nokta diller, Node.JS, C# ve daha fazlası da dahil olmak üzere çeşitli Yazar kolaylaştırmak bağlamaları koleksiyonu. Bu öğreticide, API tasarımınızda belirli eylemleri işlemek için bir HTTP tetikleyicisi özelleştirin. Ayrıca, Azure işlevleri proxy'leri ile tümleştirme ve sahte API'leri ayarlayarak API'nizi artan için hazırlarsınız. Tüm bunlar gerçekleştirilir işlevleri sunucusuz bilgi işlem ortamı üstünde kaynaklarını ölçeklendirme hakkında endişelenmeniz gerekmez - yalnızca, API mantığına odaklanabilir şekilde.

## <a name="prerequisites"></a>Ön koşullar 

[!INCLUDE [Previous quickstart note](../../includes/functions-quickstart-previous-topics.md)]

Sonuçta elde edilen işlevi bu öğreticinin geri kalanını için kullanılır.

### <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure Portalı'nı açın. Bunu yapmak için oturumu [https://portal.azure.com](https://portal.azure.com) Azure hesabınızla.

## <a name="customize-your-http-function"></a>HTTP işlevinizi özelleştirme

Varsayılan olarak, HTTP tetiklemeli işlevin herhangi bir HTTP yöntemini kabul edecek şekilde yapılandırılır. Ayrıca varsayılan URL biçiminde olduğunu `http://<yourapp>.azurewebsites.net/api/<funcname>?code=<functionkey>`. Hızlı Başlangıç sonra izlediyseniz `<funcname>` büyük olasılıkla "HttpTriggerJS1" şöyle görünür. Bu bölümde, yalnızca GET isteklerini yanıtlamak için işlevi değiştirecek `/api/hello` yerine rota. 

1. Azure portalında işlevinizi gidin. Seçin **tümleştir** sol gezinti bölmesinde.

    ![Bir HTTP işlevi özelleştirme](./media/functions-create-serverless-api/customizing-http.png)

1. Tabloda belirtildiği gibi HTTP tetikleyicisi ayarlarını kullanın.

    | Alan | Örnek değer | Açıklama |
    |---|---|---|
    | İzin verilen HTTP yöntemleri | Seçili yöntemleri | Bu işlevi çağırmak için hangi HTTP yöntemlerini kullanılabilir belirler |
    | Seçili HTTP yöntemleri | AL | Bu işlevi çağırmak için kullanılacak yalnızca seçili HTTP yöntemlerini sağlar |
    | Rota şablonu | / Merhaba | Hangi rota bu işlevi çağırmak için kullanılan belirler |
    | Yetkilendirme düzeyi | Anonim | İsteğe bağlı: işlevinizde bir API anahtarı olmadan erişilebilir hale getirir |

    > [!NOTE] 
    > İçermediği Not `/api` bu genel bir ayar tarafından gerçekleştirilir gibi temel yol önekini rota şablonu.

1. **Kaydet** düğmesine tıklayın.

HTTP işlevlerini özelleştirme hakkında daha fazla bilgiyi [Azure işlevleri HTTP ve Web kancası bağlamaları](https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook#customizing-the-http-endpoint).

### <a name="test-your-api"></a>API'nizi test

Ardından, yeni API yüzeyi ile çalışma görmek için işlevinizi test.
1. Sol gezinti bölmesinde işlevin adına tıklayarak geliştirme sayfasına geri gidin.
1. Tıklatın **Al işlevi URL** ve URL'yi kopyalayın. Bunu kullandığını görürsünüz `/api/hello` şimdi rota.
1. Yeni bir tarayıcı sekmesi veya tercih edilen REST istemciniz URL'yi kopyalayın. Tarayıcılar GET varsayılan olarak kullanır.
1. Parametreleri URL'nizi sorgu dizesinde örneğin ekleyin`/api/hello/?name=John`
1. ', Çalışmakta olduğunu onaylamak için Enter' a basın. Yanıt görmeniz gerekir "*Hello John*"
1. İşlev yürütülmez onaylamak için başka bir HTTP yöntemi ile uç noktası çağrılmadan da deneyebilirsiniz. Bunun için cURL, Postman veya Fiddler gibi bir REST istemcisi kullanmanız gerekecektir.

## <a name="proxies-overview"></a>Proxy'leri genel bakış

Sonraki bölümde, bir proxy üzerinden API'nizi belirir. Azure işlevleri proxy'leri istekleri iletmek için diğer kaynakları sağlayan bir önizleme özelliğidir. Yalnızca Bu uç HTTP tetikleyicisi, ancak ne zaman yürütmek için kodu yazmak yerine adlandırılan gibi bir HTTP uç noktası tanımlamak, uzak bir uygulama için bir URL girin. Bu, istemcilerin kullanmak için kolay olan tek bir API yüzeyi içine birden çok API kaynağı oluşturmak sağlar. API'nizi mikro olarak oluşturmak isterseniz, bu özellikle yararlıdır.

Bir proxy gibi tüm HTTP kaynağa işaret edebilir:
- Azure İşlevleri 
- API uygulamaları [Azure uygulama hizmeti](https://docs.microsoft.com/azure/app-service/app-service-web-overview)
- Docker kapsayıcılarında [Linux uygulama hizmeti](https://docs.microsoft.com/azure/app-service/containers/app-service-linux-intro)
- Barındırılan herhangi bir API'yi

Proxy'leri hakkında daha fazla bilgi için bkz: [Azure işlevleri proxy'leri (Önizleme) çalışmaya].

## <a name="create-your-first-proxy"></a>İlk proxy oluşturma

Bu bölümde, ön uç genel apı'nize olarak hizmet veren yeni bir proxy oluşturur. 

### <a name="setting-up-the-frontend-environment"></a>Ön uç ortamını ayarlama

Adımlarını yineleyin [bir işlev uygulaması oluşturma](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function#create-a-function-app) proxy oluşturursunuz yeni bir işlev uygulaması oluşturmak için. Bu yeni uygulamanın URL bizim API için ön uç hizmet verir ve daha önce düzenlemekte olduğunuz işlev uygulaması arka ucu olarak hizmet verecektir.

1. Portalı'nda yeni ön uç işlevi uygulamanıza gidin.
1. Seçin **Platform özellikleri** ve **uygulama ayarları**.
1. Ekranı aşağı kaydırarak **uygulama ayarları** burada anahtar/değer çiftlerinin depolanır ve yeni bir "HELLO_HOST" anahtarla ayarlama. Değerini, arka uç işlevi uygulamanızın konağa gibi ayarlama `<YourBackendApp>.azurewebsites.net`. Bu HTTP işlevinizi test edilirken önceden kopyaladığınız URL'yi bir parçasıdır. Bu ayar daha sonra yapılandırma başvuru.

    > [!NOTE] 
    > Proxy için bir sabit kodlanmış ortamı bağımlılığı önlemek ana bilgisayar yapılandırması için önerilen uygulama ayarları. Uygulama ayarları kullanarak, proxy yapılandırması ortamlar arasında taşıyabilirsiniz ve ortama özgü uygulama ayarları uygulanacak anlamına gelir.

1. **Kaydet** düğmesine tıklayın.

### <a name="creating-a-proxy-on-the-frontend"></a>Ön uç üzerinde bir proxy sunucu oluşturma

1. Ön uç işlevi uygulamanızı portalında için geri gidin.
1. Sol taraftaki gezinti artı işaretini tıklatın '+' yanındaki "Proxy'leri (Önizleme)".
    ![Bir proxy sunucu oluşturma](./media/functions-create-serverless-api/creating-proxy.png)
1. Tabloda belirtildiği gibi proxy ayarlarını kullanın. 

    | Alan | Örnek değer | Açıklama |
    |---|---|---|
    | Ad | HelloProxy | Yalnızca yönetim için kullanılan bir kolay ad |
    | Rota şablonu | / api/hello | Hangi rota bu proxy çağırmak için kullanılan belirler |
    | Arka uç URL'si | https://%HELLO_HOST%/api/Hello | İçin istek yönlendirilirken olacağı uç nokta belirtir |
    
1. Proxy'leri sağlamaz Not `/api` taban yol öneki ve rota şablona dahil bu gerekir.
1. `%HELLO_HOST%` Sözdizimi, daha önce oluşturduğunuz uygulama ayarı başvuru. Çözümlenen URL özgün işlevinizi işaret edecek.
1. **Oluştur**'a tıklayın.
1. Proxy URL kopyalanması ve tarayıcıda veya test ile sık kullanılan HTTP istemci tarafından yeni proxy deneyebilirsiniz.
    1. Anonim işlevi kullanmak için:
        1. `https://YOURPROXYAPP.azurewebsites.net/api/hello?name="Proxies"`
    1. Yetkilendirme kullanımıyla bir işlev için:
        1. `https://YOURPROXYAPP.azurewebsites.net/api/hello?code=YOURCODE&name="Proxies"`

## <a name="create-a-mock-api"></a>Sahte bir API oluşturma

Ardından, çözümünüz için sahte bir API oluşturmak için bir proxy kullanır. Bu, istemci geliştirme ilerleme için tam olarak uygulanan arka uç gerek kalmadan sağlar. Geliştirme, daha sonra bu mantığı destekleyen yeni bir işlev uygulaması oluşturmak ve proxy yönlendirin.

Bu sahte API oluşturmak için yeni bir proxy oluşturacağız kullanarak bu zaman [App Service Düzenleyicisi](https://github.com/projectkudu/kudu/wiki/App-Service-Editor). Başlamak için portal işlevi uygulamanızda gidin. Seçin **Platform özellikleri** ve altında **geliştirme araçları** Bul **App Service Düzenleyicisi**. Bu tıklandığında App Service Düzenleyicisi'ni yeni sekmede açılır.

Seçin `proxies.json` sol gezinti bölmesinde. Bu yapılandırma, proxy'leri tamamı için depolayan dosyasıdır. Birini kullanırsanız [işlevleri dağıtım yöntemleri](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment), bu kaynak denetiminde koruyacaktır dosyasıdır. Bu dosya hakkında daha fazla bilgi için bkz: [proxy'leri Gelişmiş Yapılandırma](https://docs.microsoft.com/azure/azure-functions/functions-proxies#advanced-configuration).

O ana kadarki izlediyseniz, proxies.json aşağıdaki gibi görünmelidir:

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "HelloProxy": {
            "matchCondition": {
                "route": "/api/hello"
            },
            "backendUri": "https://%HELLO_HOST%/api/hello"
        }
    }
}
```

Sonraki sahte API'nizi ekleyeceksiniz. Proxies.json dosyanızı aşağıdaki satırla değiştirin:

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "HelloProxy": {
            "matchCondition": {
                "route": "/api/hello"
            },
            "backendUri": "https://%HELLO_HOST%/api/hello"
        },
        "GetUserByName" : {
            "matchCondition": {
                "methods": [ "GET" ],
                "route": "/api/users/{username}"
            },
            "responseOverrides": {
                "response.statusCode": "200",
                "response.headers.Content-Type" : "application/json",
                "response.body": {
                    "name": "{username}",
                    "description": "Awesome developer and master of serverless APIs",
                    "skills": [
                        "Serverless",
                        "APIs",
                        "Azure",
                        "Cloud"
                    ]
                }
            }
        }
    }
}
```

Bu yeni olmadan bir proxy, "GetUserByName" backendUri özelliği ekler. Başka bir kaynak çağırmak yerine, bir yanıt geçersiz kılma kullanılarak proxy'leri varsayılan yanıttan değiştirir. İstek ve yanıt geçersiz kılmaları da arka uç URL'si ile birlikte kullanılabilir. Bu, özellikle eski bir sistemi için proxy sunucusunu nereye üst bilgiler, değiştirmeniz gerekebilir sorguladığınızda parametreleri, vb. kullanışlıdır. İstek ve yanıt geçersiz kılmalar hakkında daha fazla bilgi için bkz: [istekleri ve yanıtları proxy'leri değiştirme](https://docs.microsoft.com/azure/azure-functions/functions-proxies#a-namemodify-requests-responsesamodifying-requests-and-responses).

Çağırarak sahte API'nizi test `<YourProxyApp>.azurewebsites.net/api/users/{username}` bir tarayıcı veya sık kullandığınız REST istemcisinden kullanarak uç nokta. Değiştirdiğinizden emin olun _{username}_ olan bir kullanıcı adı temsil eden bir dize değeri.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, derleme ve Azure işlevleri bir API özelleştirme öğrendiniz. Ayrıca mocks, birleşik bir API yüzeyi olarak birlikte de dahil olmak üzere çok sayıda API getirmek öğrendiniz. Bu teknikler, tüm sunucusuz işlem modeli üzerinde çalışırken Azure işlevleri tarafından sağlanan tüm karmaşıklık API'leri oluşturmak için kullanabilirsiniz.

Daha fazla API'nizi geliştirdikçe aşağıdaki başvurular yararlı olabilir:

- [Azure işlevleri HTTP ve Web kancası bağlamaları](https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook)
- [Azure işlevleri proxy'leri (Önizleme) çalışmaya]
- [Azure işlevleri API (Önizleme) belgeleme](https://docs.microsoft.com/azure/azure-functions/functions-api-definition-getting-started)


[Create your first function]: https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function
[Azure işlevleri proxy'leri (Önizleme) çalışmaya]: https://docs.microsoft.com/azure/azure-functions/functions-proxies
