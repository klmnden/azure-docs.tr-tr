Depolama öykünücüsü paylaşılan anahtar kimlik doğrulaması için tek bir sabit hesap ve bilinen bir kimlik doğrulama anahtarı destekler. Bu hesabı ve anahtarı depolama öykünücüsünü ile kullanmak için izin verilen yalnızca paylaşılan anahtar kimlik bilgileridir. Bunlar:

```
Account name: devstoreaccount1
Account key: Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==
```

> [!NOTE]
> Depolama öykünücüsü tarafından desteklenen kimlik doğrulama anahtarı, yalnızca istemci kimlik doğrulama kodunuzu işlevselliğini test etmek için tasarlanmıştır. Herhangi bir güvenlik amaçla sunmuyor. Üretim depolama hesabı ve anahtarı ile depolama öykünücüsünü kullanamazsınız. Üretim verileri ile geliştirme hesap kullanmamalısınız.
> 
> Depolama öykünücüsü yalnızca bağlantı HTTP üzerinden destekler. Ancak, HTTPS üretimde Azure depolama hesabı kaynaklarına erişim için önerilen protokolüdür.
> 

#### <a name="connect-to-the-emulator-account-using-a-shortcut"></a>Kısayol kullanarak öykünücüsü hesabına bağlanma
Kısayol başvuruyor, uygulamanızın yapılandırma dosyasında bağlantı dizesini yapılandırmak için depolama öykünücüsünü uygulamanızdan bağlanmak için en kolay yolu olan `UseDevelopmentStorage=true`. Depolama öykünücüsünde için bir bağlantı dizesi örneği bir *app.config* dosyası: 

```xml
<appSettings>
  <add key="StorageConnectionString" value="UseDevelopmentStorage=true" />
</appSettings>
```

#### <a name="connect-to-the-emulator-account-using-the-well-known-account-name-and-key"></a>İyi bilinen hesap adı ve anahtarı kullanarak öykünücüsü hesabına bağlanma
Öykünücü hesap adı ve anahtar başvuruda bulunan bir bağlantı dizesi oluşturmak için bağlantı dizesindeki öykücüsünden kullanmak istediğiniz hizmetlerinin her biri için uç noktalar belirtmeniz gerekir. Bağlantı dizesi bir üretim depolama hesabı için farklı öykünücüsü uç noktaları başvurur böylece, bu gereklidir. Örneğin, bağlantı dizenizi değerini şuna benzeyecektir:

```
DefaultEndpointsProtocol=http;AccountName=devstoreaccount1;
AccountKey=Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==;
BlobEndpoint=http://127.0.0.1:10000/devstoreaccount1;
TableEndpoint=http://127.0.0.1:10002/devstoreaccount1;
QueueEndpoint=http://127.0.0.1:10001/devstoreaccount1;
```

Bu değer, yukarıda gösterilen kısayol özdeş olur `UseDevelopmentStorage=true`.

#### <a name="specify-an-http-proxy"></a>Bir HTTP Ara sunucusunu belirtin
Depolama öykünücüsü karşı hizmet sınarken kullanmak için bir HTTP proxy de belirtebilirsiniz. Bu işlem depolama hizmetleri karşı hatalarını ayıkladığınız sırada HTTP istekleri ve yanıtları Gözlemleme için yararlı olabilir. Bir proxy belirtmek için add `DevelopmentStorageProxyUri` seçeneği ile bağlantı dizesi ve bir proxy URI'si değerini ayarlayın. Örneğin, depolama öykünücüsünü işaret eder ve bir HTTP proxy yapılandıran bir bağlantı dizesi şöyledir:

```
UseDevelopmentStorage=true;DevelopmentStorageProxyUri=http://myProxyUri
```

