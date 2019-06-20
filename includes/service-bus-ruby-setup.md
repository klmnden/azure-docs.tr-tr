---
author: spelluru
ms.service: service-bus
ms.topic: include
ms.date: 11/09/2018
ms.author: spelluru
ms.openlocfilehash: 16ce537a54fc77fc0f72b859d6d193501d86c1fc
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188604"
---
## <a name="create-a-ruby-application"></a>Ruby uygulaması oluşturma
Yönergeler için [Azure'da Ruby uygulaması oluşturma](../articles/virtual-machines/linux/classic/ruby-rails-web-app.md).

## <a name="configure-your-application-to-use-service-bus"></a>Service Bus kullanmak için uygulamanızı yapılandırma
Service Bus hizmetini kullanmak için indirin ve depolama REST Hizmetleri ile iletişim kuran bir dizi kolaylık içeren Azure Ruby paketi kullanın.

### <a name="use-rubygems-to-obtain-the-package"></a>Paketi edinmek için RubyGems kullanma
1. **PowerShell** (Windows), **Terminal** (Mac) veya **Bash** (Unix) gibi bir komut satırı arabirimi kullanın.
2. Gem ve bağımlılıklarını yüklemek için komut penceresinde "gem yükleme azure" yazın.

### <a name="import-the-package"></a>Paketi içeri aktarma
Metin düzenleyiciyi kullanarak, depolama alanı kullanmayı düşündüğünüz Ruby dosyasının en üstüne aşağıdakileri ekleyin:

```ruby
require "azure"
```

## <a name="set-up-a-service-bus-connection"></a>Service Bus bağlantı kurma
Ad alanı değerlerini, anahtar, anahtar, imzalayan ve ana bilgisayar adını ayarlamak için aşağıdaki kodu kullanın:

```ruby
Azure.configure do |config|
  config.sb_namespace = '<your azure service bus namespace>'
  config.sb_sas_key_name = '<your azure service bus access keyname>'
  config.sb_sas_key = '<your azure service bus access key>'
end
signer = Azure::ServiceBus::Auth::SharedAccessSigner.new
sb_host = "https://#{Azure.sb_namespace}.servicebus.windows.net"
```

Ad alanı değeri, URL'nin tamamı yerine oluşturduğunuz değere ayarlayın. Örneğin, **"yourexamplenamespace"** , olmayan "yourexamplenamespace.servicebus.windows.net".
