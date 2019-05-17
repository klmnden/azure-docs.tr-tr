---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 09/15/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 6882c46ec0e4925c42de86c87225e9509c84df84
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "65757941"
---
## <a name="set-up-your-development-environment"></a>Geliştirme ortamınızı kurma
Ardından, geliştirme ortamınızı Visual Studio’da ayarlayın; böylece bu kılavuzdaki kod örneklerini denemeye hazır olursunuz.

### <a name="create-a-windows-console-application-project"></a>Windows konsol uygulaması projesi oluşturma
Visual Studio'da yeni bir Windows konsol uygulaması oluşturun. Aşağıdaki adımlar Visual Studio 2017’de bir konsol uygulaması oluşturmayı gösterir. Adımlar Visual Studio’nun diğer sürümlerinde de benzerdir.

1. **Dosya** > **Yeni** > **Proje**’yi seçin.
2. **Yüklü** > **Şablonlar** > **Visual C#** > **Windows Klasik Masaüstü** öğesini seçin.
3. **Konsol Uygulaması (.NET Framework)** öğesini seçin.
4. **Ad** alanına uygulamanız için bir ad girin.
5. **Tamam**’ı seçin.

![Visual Studio'da Yeni Proje iletişim kutusunun ekran görüntüsü](./media/storage-development-environment-include/storage-development-environment-include-1.png)

Bu öğreticideki tüm kod örnekleri konsol uygulamanızın `Program.cs` dosyasındaki `Main()` yöntemine eklenebilir.

Azure bulut hizmeti veya web uygulaması ile masaüstü ve mobil uygulamaları dahil olmak üzere herhangi bir .NET uygulaması türünde Azure Depolama İstemcisi Kitaplığını kullanabilirsiniz. Bu kılavuzda, sadeleştirmek için konsol uygulaması kullanmaktayız.

### <a name="use-nuget-to-install-the-required-packages"></a>Gereken paketleri yüklemek için NuGet kullanma
Bu öğreticiyi tamamlamak için projenizde başvurmanız gereken iki paket vardır:

* [.NET için Microsoft Azure depolama istemci Kitaplığı](https://www.nuget.org/packages/WindowsAzure.Storage/): Bu paket depolama hesabınızdaki veri kaynaklarına programlı erişim sağlar.
* [.NET için Microsoft Azure Yapılandırma Yöneticisi Kitaplığı](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/): Bu paket, uygulamanızın nerede çalıştığına bakmaksızın yapılandırma dosyasındaki bağlantı dizesini ayrıştırmak için bir sınıf sağlar.

Her iki paketi de almak için NuGet kullanabilirsiniz. Şu adımları uygulayın:

1. **Çözüm Gezgini**'nde projenize sağ tıklayın ve **NuGet Paketlerini Yönet**’i seçin.
2. Çevrimiçi olarak "WindowsAzure.Storage" ifadesini arayın ve Depolama İstemci Kitaplığı’nı ve bağımlılıklarını yüklemek için **Yükle**’yi seçin.
3. Çevrimiçi olarak "WindowsAzure.ConfigurationManager" ifadesini arayın ve Azure Yapılandırma Yöneticisi’ni yüklemek için **Yükle**’yi seçin.

> [!NOTE]
> Depolama İstemcisi Kitaplığı paketi [.NET için Azure SDK](https://azure.microsoft.com/downloads/) uygulamasında da bulunur. Ancak, istemci kitaplığının her zaman en son sürümüne sahip olmanızı sağlamak için Depolama istemcisi Kitaplığını NuGet’ten yüklemenizi öneririz.
> 
> .NET için Depolama İstemci Kitaplığındaki ODataLib bağımlılıkları, WCF Veri Hizmetleri’nden değil, NuGet üzerindeki ODataLib paketleriyle çözümlenir. ODataLib kitaplıkları NuGet aracılığıyla doğrudan indirilebilir veya kod projenizle başvurulabilir. Depolama İstemcisi Kitaplığı tarafından kullanılan belirli ODataLib paketleri [OData](http://nuget.org/packages/Microsoft.Data.OData/), [Edm](http://nuget.org/packages/Microsoft.Data.Edm/) ve [Spatial](http://nuget.org/packages/System.Spatial/) paketleridir. Bu kitaplıklar, Azure Table Storage sınıfları tarafından kullanılırken Depolama İstemcisi Kitaplığı’yla programlama için gerekli bağımlılıklardır.
> 
> 

### <a name="determine-your-target-environment"></a>Hedef ortamınızı saptama
Bu kılavuzdaki örnekleri çalıştırmak için iki ortam seçeneğiniz vardır:

* Kodunuzu buluttaki bir Azure Storage hesabına karşı çalıştırabilirsiniz. 
* Kodunuzu Azure Storage öykünücüsüne karşı çalıştırabilirsiniz. Depolama öykünücüsü, buluttaki Azure Storage hesabına öykünen bir yerel ortamdır. Öykünücü, uygulamanız geliştirildiği sırada kodunuzu test etmek ve hatalarını ayıklamak için bağımsız bir seçenektir. Öykünücü iyi bilinen hesabı ve anahtarı kullanır. Daha fazla bilgi için bkz. [Geliştirme ve test için Azure depolama öykünücüsünü kullanma](../articles/storage/common/storage-use-emulator.md).

Buluttaki bir depolama hesabını hedefliyorsanız, depolama hesabınız için birincil erişim anahtarını Azure portalından kopyalayın. Daha fazla bilgi için bkz. [Erişim anahtarları](../articles/storage/common/storage-account-manage.md#access-keys)

> [!NOTE]
> Azure Storage ile ilişkili maliyetlerin oluşmasını önlemek için depolama öykünücüsünü hedefleyebilirsiniz. Ancak, buluttaki bir Azure Storage hesabını hedeflemeyi seçerseniz, bu öğreticiyi gerçekleştirme maliyetleri göz ardı edilecektir.
> 
> 

### <a name="configure-your-storage-connection-string"></a>Depolama bağlantı dizelerinizi yapılandırma
.NET için Azure Storage İstemcisi Kitaplığı,depolama hizmetlerine erişilmesi amacıyla uç noktaları ve kimlik bilgilerini yapılandıracak depolama bağlantı dizesinin kullanılmasını destekler. Depolama bağlantı dizenizi korumanın en iyi yolu bir yapılandırma dosyasında tutmaktır. 

Bağlantı dizeleri hakkında daha fazla bilgi için bkz. [Azure Depolama’da bir bağlantı dizesi yapılandırma](../articles/storage/common/storage-configure-connection-string.md).

> [!NOTE]
> Depolama hesabı anahtarınız depolama hesabınızın kök parolasına benzer. Depolama hesabı anahtarınızı korumak için her zaman özen gösterin. Diğer kullanıcılara dağıtmaktan, sabit kodlamaktan ve başkalarının erişebileceği düz metin dosyasına kaydetmekten kaçının. Anahtarınızın tehlikede olduğunu düşünüyorsanız, Azure portalını kullanarak hesap anahtarınızı yeniden oluşturun.
> 
> 

Bağlantı dizenizi yapılandırmak için, `app.config` dosyasını Visual Studio’daki Çözüm Gezgini'nden açın. `<appSettings>` öğesinin içeriğini aşağıda gösterildiği gibi ekleyin. `account-name` değerini depolama hesabınızın adıyla ve `account-key` değerini hesabınızın erişim anahtarıyla değiştirin:

```xml
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2" />
    </startup>
    <appSettings>
        <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key" />
    </appSettings>
</configuration>
```

Örneğin, yapılandırma ayarınız şunun gibi görünür:

```xml
<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=storagesample;AccountKey=GMuzNHjlB3S9itqZJHHCnRkrokLkcSyW7yK9BRbGp0ENePunLPwBgpxV1Z/pVo9zpem/2xSHXkMqTHHLcx8XRA==" />
```

Depolama öykünücüsünü hedeflemek için iyi bilinen hesap adıyla ve anahtarıyla eşleşen bir kısayolu kullanabilirsiniz. Bu durumda, bağlantı dizesi ayarı şöyle olur:

```xml
<add key="StorageConnectionString" value="UseDevelopmentStorage=true;" />
```

