## <a name="create-a-ruby-application"></a>Ruby uygulaması oluşturma
Yönergeler için bkz: [Azure üzerinde bir Ruby uygulaması oluşturmak](../articles/virtual-machines/linux/classic/ruby-rails-web-app.md).

## <a name="configure-your-application-to-use-service-bus"></a>Uygulamanızı kullanım hizmet veri yolu için yapılandırma
Service Bus hizmetini kullanmak için karşıdan yükle ve storage REST Hizmetleri ile iletişim kuran bir dizi kolaylık içeren Azure Ruby paketini kullanın.

### <a name="use-rubygems-to-obtain-the-package"></a>Paket elde etmek için RubyGems kullanın
1. Bir komut satırı arabirimi gibi kullandığınız **PowerShell** (Windows), **Terminal** (Mac) veya **Bash** (UNIX).
2. Gem ve bağımlılıklarını yüklemek için komut penceresinde "gem yükleme azure" yazın.

### <a name="import-the-package"></a>Paket alma
Sık kullandığınız metin Düzenleyicisi'ni kullanarak aşağıdaki depolama kullanmayı düşündüğünüz Söyleniş dosyasının üstüne ekleyin:

```ruby
require "azure"
```

## <a name="set-up-a-service-bus-connection"></a>Hizmet veri yolu bağlantı kurma
Ad alanı değerleri, anahtar, anahtar, imzalayan ve ana bilgisayar adını ayarlamak için aşağıdaki kodu kullanın:

```ruby
Azure.configure do |config|
  config.sb_namespace = '<your azure service bus namespace>'
  config.sb_sas_key_name = '<your azure service bus access keyname>'
  config.sb_sas_key = '<your azure service bus access key>'
end
signer = Azure::ServiceBus::Auth::SharedAccessSigner.new
sb_host = "https://#{Azure.sb_namespace}.servicebus.windows.net"
```

Ad alanı değeri URL'nin tamamı yerine oluşturulan değere ayarlayın. Örneğin, **"yourexamplenamespace"**, değil "yourexamplenamespace.servicebus.windows.net".
