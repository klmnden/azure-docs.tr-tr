---
title: Azure önbelleği için Redis Java ile kullanma hakkında bilgi edinmek için hızlı başlangıç | Microsoft Docs
description: Bu hızlı başlangıçta, Azure önbelleği için Redis kullanan yeni bir Java uygulaması oluşturacaksınız.
services: cache
documentationcenter: ''
author: yegu-ms
manager: jhubbard
editor: ''
ms.assetid: 29275a5e-2e39-4ef2-804f-7ecc5161eab9
ms.service: cache
ms.devlang: java
ms.topic: quickstart
ms.tgt_pltfrm: cache
ms.workload: tbd
ms.date: 05/23/2018
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: 047350fa957d9ca1fdec50f97a590ba90a42e1b2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60830577"
---
# <a name="quickstart-how-to-use-azure-cache-for-redis-with-java"></a>Hızlı Başlangıç: Redis ile Java için Azure önbellek kullanma


Azure önbelleği için Redis, Redis, Microsoft tarafından yönetilen için adanmış bir Azure önbellek hizmetine erişmenizi sağlar. Önbelleğinize Microsoft Azure’daki her uygulamadan erişilebilir.

Bu makalede, Redis kullanmak için Azure önbelleği ile çalışmaya başlama işlemini göstermektedir [Jedis](https://github.com/xetorthio/jedis) Redis istemcisini Java için.

![Önbellek uygulaması tamamlandı](./media/cache-java-get-started/cache-app-complete.png)

Bu hızlı başlangıçtaki adımları tamamlamak için herhangi bir kod düzenleyicisini kullanabilirsiniz. Ancak, Windows, macOS ve Linux platformlarında sağlanan [Visual Studio Code](https://code.visualstudio.com/) mükemmel bir seçenektir.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Önkoşullar

[Apache Maven](https://maven.apache.org/)



## <a name="create-an-azure-cache-for-redis"></a>Bir Azure önbelleği için Redis oluşturma

[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

[!INCLUDE [redis-cache-access-keys](../../includes/redis-cache-access-keys.md)]

**ANA BİLGİSAYAR ADI** ve **Birincil** erişim anahtarınız için ortam değişkenleri ekleyin. Hassas bilgileri kodunuza doğrudan eklemek yerine kodunuzdaki bu değişkenleri kullanacaksınız.

```
set REDISCACHEHOSTNAME=contosoCache.redis.cache.windows.net
set REDISCACHEKEY=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```

## <a name="create-a-new-java-app"></a>Yeni bir Java uygulaması oluşturma

Maven kullanarak yeni bir quickstart uygulaması oluşturun:

```
mvn archetype:generate -DarchetypeGroupId=org.apache.maven.archetypes -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeVersion=1.3 -DgroupId=example.demo -DartifactId=redistest -Dversion=1.0
```

Yeni *redistest* proje dizinine geçin.

*Pom.xml* dosyasını açın ve [Jedis](https://github.com/xetorthio/jedis) için bağımlılık ekleyin:

```xml
    <dependency>
        <groupId>redis.clients</groupId>
        <artifactId>jedis</artifactId>
        <version>2.9.0</version>
        <type>jar</type>
        <scope>compile</scope>
    </dependency>
```

*Pom.xml* dosyasını kaydedin.

*App.java*'yı açın ve kodu aşağıdaki kodla değiştirin:

```java
package example.demo;

import redis.clients.jedis.Jedis;
import redis.clients.jedis.JedisShardInfo;

/**
 * Redis test
 *
 */
public class App 
{
    public static void main( String[] args )
    {

        boolean useSsl = true;
        String cacheHostname = System.getenv("REDISCACHEHOSTNAME");
        String cachekey = System.getenv("REDISCACHEKEY");

        // Connect to the Azure Cache for Redis over the SSL port using the key.
        JedisShardInfo shardInfo = new JedisShardInfo(cacheHostname, 6380, useSsl);
        shardInfo.setPassword(cachekey); /* Use your access key. */
        Jedis jedis = new Jedis(shardInfo);      

        // Perform cache operations using the cache connection object...

        // Simple PING command        
        System.out.println( "\nCache Command  : Ping" );
        System.out.println( "Cache Response : " + jedis.ping());

        // Simple get and put of integral data types into the cache
        System.out.println( "\nCache Command  : GET Message" );
        System.out.println( "Cache Response : " + jedis.get("Message"));

        System.out.println( "\nCache Command  : SET Message" );
        System.out.println( "Cache Response : " + jedis.set("Message", "Hello! The cache is working from Java!"));

        // Demonstrate "SET Message" executed as expected...
        System.out.println( "\nCache Command  : GET Message" );
        System.out.println( "Cache Response : " + jedis.get("Message"));

        // Get the client list, useful to see if connection list is growing...
        System.out.println( "\nCache Command  : CLIENT LIST" );
        System.out.println( "Cache Response : " + jedis.clientList());

        jedis.close();
    }
}
```

Bu kod, bir Azure önbelleği için Redis örneği önbellek ana bilgisayar adı ve anahtar ortam değişkenlerini kullanarak bağlanmak nasıl gösterir. Kod ayrıca önbellekte bir dize değeri depolar ve alır. Ayrıca `PING` ve `CLIENT LIST` komutları da yürütülür. 

*App.java*'yı kaydedin.

## <a name="build-and-run-the-app"></a>Uygulamayı derleme ve çalıştırma

Uygulamayı derlemek ve çalıştırmak için aşağıdaki Maven komutunu yürütün:

```
mvn compile
mvn exec:java -D exec.mainClass=example.demo.App
```

Aşağıdaki örnekte, `Message` anahtarının Azure portaldaki Redis Konsolu kullanılarak ayarlanan, önceden önbelleğe alınmış bir değer içerdiğini görebilirsiniz. Uygulama, önbelleğe alınan bu değeri güncelleştirdi. Ayrıca uygulama, `PING` ve `CLIENT LIST` komutlarını da yürüttü.

![Önbellek uygulaması tamamlandı](./media/cache-java-get-started/cache-app-complete.png)


## <a name="clean-up-resources"></a>Kaynakları temizleme

Sonraki öğreticiyle devam edecekseniz, bu hızlı başlangıçta oluşturulan kaynakları tutabilir ve sonraki öğreticide yeniden kullanabilirsiniz.

Aksi takdirde, hızlı başlangıç örnek uygulamasını tamamladıysanız ücret yansıtılmaması için bu hızlı başlangıçta oluşturulan Azure kaynaklarını silebilirsiniz. 

> [!IMPORTANT]
> Bir kaynak grubunu silme işlemi geri alınamaz ve kaynak grubunun ve içindeki tüm kaynaklar kalıcı olarak silinir. Yanlış kaynak grubunu veya kaynakları yanlışlıkla silmediğinizden emin olun. Bu örneği, tutmak istediğiniz kaynakları içeren mevcut bir kaynak grubunda barındırmak için kaynaklar oluşturduysanız, kaynak grubunu silmek yerine her kaynağı kendi ilgili dikey penceresinden tek tek silebilirsiniz.
>

[Azure portalında](https://portal.azure.com) oturum açın ve **Kaynak grupları**’na tıklayın.

**Ada göre filtrele...** metin kutusuna kaynak grubunuzun adını girin. Bu makaledeki yönergelerde *TestResources* adlı bir kaynak grubu kullanılmıştır. Sonuç listesindeki kaynak grubunuzda **...** ve sonra **Kaynak grubunu sil**’e tıklayın.

![Sil](./media/cache-java-get-started/cache-delete-resource-group.png)

Kaynak grubunun silinmesini onaylamanız istenir. Onaylamak için kaynak grubunuzun adını yazın ve **Sil**’e tıklayın.

Birkaç dakika sonra kaynak grubu ve içerdiği kaynakların tümü silinir.



## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure Cache, Redis için Java uygulamasından kullanma öğrendiniz. Azure önbelleği için Redis ile ASP.NET web uygulaması kullanmak sonraki hızlı başlangıca devam edin.

> [!div class="nextstepaction"]
> [Bir Azure önbelleği için Redis kullanan ASP.NET web uygulaması oluşturun.](./cache-web-app-howto.md)



