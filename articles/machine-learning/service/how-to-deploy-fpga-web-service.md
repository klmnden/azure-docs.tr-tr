---
title: Azure Machine Learning ile bir FPGA üzerinde bir web hizmeti olarak modeli dağıtma
description: Azure Machine Learning ile bir FPGA üzerinde çalışan bir modelle bir web hizmeti dağıtmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: tedway
author: tedway
ms.date: 05/07/2018
ms.openlocfilehash: f3237980a1ad1969b5cf8d42d547ddf96608dd97
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="deploy-a-model-as-a-web-service-on-an-fpga-with-azure-machine-learning"></a>Azure Machine Learning ile bir FPGA üzerinde bir web hizmeti olarak modeli dağıtma

Bu belgede, iş istasyonu ortamınızı ayarlayın ve bir model üzerinde bir web hizmeti olarak dağıtma hakkında bilgi edinin [alan programlanabilir kapısı diziler (FPGA)](concept-accelerate-with-fpgas.md). Web hizmeti proje Brainwave model üzerinde FPGA çalıştırmak için kullanır.

FPGAs kullanarak tek bir toplu iş boyutu ile bile son derece düşük gecikme süresi inferencing sağlar.

## <a name="create-an-azure-machine-learning-model-management-account"></a>Bir Azure Machine Learning modeli yönetim hesabı oluşturun

1. Model yönetim hesabı oluşturma sayfaya gidin [Azure Portal](https://aka.ms/aml-create-mma).

2. Portalda, bir Model yönetim hesabı oluşturun **Doğu ABD 2** bölge.

   ![Model yönetim hesabı oluştur ekran görüntüsü](media/how-to-deploy-fpga-web-service/azure-portal-create-mma.PNG)

3. Model yönetim hesabınız bir ad verin, bir abonelik seçin ve bir kaynak grubu seçin.

   >[!IMPORTANT]
   >Konum için seçmelisiniz **Doğu ABD 2** bölge olarak.  Şu anda başka bir bölge yok.

4. Bir fiyatlandırma katmanı seçin (S1 yeterli bağlıdır, ancak aynı zamanda iş S2 ve S3).  DevTest katmanı desteklenmiyor.  

5. Tıklatın **seçin** fiyatlandırma katmanı onaylamak için.

6. Tıklatın **oluşturma** soldaki ML Model yönetim.

## <a name="get-model-management-account-information"></a>Model yönetim hesabı bilgileri alma

Model yönetim hesabı (MMA) hakkında bilgi edinmek için tıklayın __Model yönetim hesabı__ Azure portalındaki.

Değerlerin aşağıdaki öğelerden birini kopyalayın:

+ Model yönetim hesabı adı (içinde sol üst köşedeki üzerinde)
+ Kaynak grubu adı
+ Abonelik Kimliği
+ Konum (kullan "eastus2")

![Model yönetim hesabı bilgileri](media/how-to-deploy-fpga-web-service/azure-portal-mma-info.PNG)

## <a name="set-up-your-machine"></a>Makine kurun

İş istasyonunuzu FPGA dağıtımı için ayarlamak için aşağıdaki adımları kullanın:

1. En son sürümünü karşıdan yükleyip [Git](https://git-scm.com/downloads).

2. Yükleme [Anaconda (Python 3.6)](https://conda.io/miniconda.html).

3. Anaconda ortam indirmek için Git komut isteminden aşağıdaki komutu kullanın:

    ```
    git clone https://aka.ms/aml-real-time-ai
    ```

4. Ortamı oluşturmak için açık bir **Anaconda istemi** (Azure Machine Learning çalışma ekranı istemi değil) ve aşağıdaki komutu çalıştırın:

    > [!IMPORTANT]
    > `environment.yml` Önceki adımda kopyaladığınız git deposunda dosyasıdır. Yolun istasyonunuzda dosyasına işaret etmek için gerektiği gibi değiştirin.

    ```
    conda env create -f environment.yml
    ```

5. Ortam etkinleştirmek için aşağıdaki komutu kullanın:

    ```
    conda activate amlrealtimeai
    ```

6. Jupyter not defteri sunucu başlatmak için aşağıdaki komutu kullanın:

    ```
    jupyter notebook
    ```

    Bu komutun çıktısı aşağıdakine benzer:

    ```text
    Copy/paste this URL into your browser when you connect for the first time, to login with a token:
        http://localhost:8888/?token=bb2ce89cc8ae931f5df50f96e3a6badfc826ff4100e78075
    ```

    > [!TIP]
    > Farklı bir belirteç komutu her çalıştırdığınızda alırsınız.

    Tarayıcınız Jupyter not defteri için otomatik olarak açılmazsa sayfasını açmak için önceki komutu tarafından döndürülen HTTP URL'sini kullanın.

    ![Jupyter not defteri web sayfasının görüntüsü](./media/how-to-deploy-fpga-web-service/jupyter-notebook.png)

## <a name="deploy-your-model"></a>Model dağıtma

Jupyter Not Defteri ' açmak `00_QuickStart.ipynb` dizüstü bilgisayarınızı `notebooks/resnet50` dizin. Not defterini'ndaki yönergeleri izleyin:

* Hizmeti tanımlayın
* Modeli dağıtma
* Dağıtılan model kullanma
* Dağıtılan Hizmetleri Sil

> [!IMPORTANT]
> Gecikme süresi ve verimlilik en iyi duruma getirme iş istasyonunuzu uç nokta ile aynı Azure bölgesinde olmalıdır.  Şu anda API'leri Doğu ABD Azure bölgesinde oluşturulur.

## <a name="ssltls-and-authentication"></a>SSL/TLS ve kimlik doğrulaması

Azure Machine Learning SSL desteği ve anahtar tabanlı kimlik doğrulaması sağlar. Bu, hizmet ve istemcileri tarafından gönderilen güvenli veri erişimini kısıtlama olanak sağlar.

> [!NOTE]
> Bu bölümdeki adımları yalnızca Azure Machine Learning donanım hızlandırılmış modelleri için geçerlidir. Standart Azure Machine Learning hizmetler için bkz: [Azure Machine Learning işlem SSL ayarlamak nasıl](https://docs.microsoft.com/azure/machine-learning/preview/how-to-setup-ssl-on-mlc) belge.

> [!IMPORTANT]
> Kimlik doğrulaması, yalnızca bir SSL sertifikası ve anahtarı sağlamış Hizmetleri için etkinleştirilir. 
>
> SSL etkinleştirmezseniz, internet üzerindeki herhangi bir kullanıcı hizmete çağrı yapmak mümkün olacaktır.
>
> SSL ve kimlik doğrulama anahtarı etkinleştirirseniz, hizmet erişirken gereklidir.

SSL istemci ile hizmet arasında gönderilen verileri şifreler. Bu da sunucunun kimliğini doğrulamak için istemci tarafından kullanılmış.

SSL özellikli bir hizmet dağıtmak veya etkinleştirmek için zaten dağıtılmış bir hizmeti güncelleştirin. Aynı adımlar şunlardır:

1. Bir etki alanı adı satın alır.

2. Bir SSL sertifikası alın.

3. Dağıtın veya hizmet etkin SSL ile güncelleştirin.

4. DNS hizmete işaret edecek şekilde güncelleştirin.

### <a name="acquire-a-domain-name"></a>Bir etki alanı adını Al

Zaten bir etki alanı adına sahip olmadığınız takdirde birinden satın alabilirsiniz bir __etki alanı adı kayıt__. Maliyet yaptığı gibi işlem kaydedicilerin arasında farklılık gösterir. Kayıt şirketi Ayrıca, araçları ile etki alanı adı yönetmek için sağlar. Bu araçlar, hizmetiniz konumunda barındırılan IP adresi için bir tam etki alanı adını (örneğin, www.contoso.com) eşleştirmek için kullanılır.

### <a name="acquire-an-ssl-certificate"></a>Bir SSL sertifikası alın

Bir SSL sertifikası almak için birçok yolu vardır. En sık karşılaşılan bir satın almaktır bir __sertifika yetkilisi__ (CA). Sertifikayı nerede alma bağımsız olarak, aşağıdaki dosyaları gerekir:

* A __sertifika__. Sertifika tam sertifika zinciri içermelidir ve PEM kodlanmış olmalıdır.
* A __anahtar__. Anahtar PEM kodlanmış olmalıdır.

> [!TIP]
> Sertifika yetkilisi sertifika ve anahtar PEM kodlu dosyaları olarak sağlayamaz, bir yardımcı programı gibi kullanabilir [OpenSSL](https://www.openssl.org/) biçimini değiştirmek için.

> [!IMPORTANT]
> Otomatik olarak imzalanan sertifikalar yalnızca geliştirme için kullanılmalıdır. Bunlar üretimde kullanılmamalıdır.
>
> Kendinden imzalı bir sertifika kullanıyorsanız bkz [hizmetleri otomatik olarak imzalanan sertifikalarla kullanma](#self-signed) bölüm özel yönergeler için.

> [!WARNING]
> Sertifika isterken, hizmet için kullanmayı planladığınız adresinin tam etki alanı adı (FQDN) sağlamalısınız. Örneğin, www.contoso.com. Sertifika damgalı adresi ve istemciler tarafından kullanılan adres hizmetin kimliğini doğrularken karşılaştırılır.
>
> Adresleri eşleşmiyorsa, istemcilerin bir hata alırsınız. 

### <a name="deploy-or-update-the-service-with-ssl-enabled"></a>Dağıtın veya hizmet etkin SSL ile güncelleştirin

SSL özellikli hizmeti dağıtmak için ayarlanmış `ssl_enabled` parametresi `True`. Ayarlama `ssl_certificate` parametre değerine __sertifika__ dosya ve `ssl_key` değerine __anahtar__ dosya. Aşağıdaki örnek, hizmet etkin SSL ile dağıtma gösterir:

```python
from amlrealtimeai import DeploymentClient

subscription_id = "<Your Azure Subscription ID>"
resource_group = "<Your Azure Resource Group Name>"
model_management_account = "<Your AzureML Model Management Account Name>"
location = "eastus2"

model_name = "resnet50-model"
service_name = "quickstart-service"

deployment_client = DeploymentClient(subscription_id, resource_group, model_management_account, location)

with open('cert.pem','r') as cert_file:
    with open('key.pem','r') as key_file:
        cert = cert_file.read()
        key = key_file.read()
        service = deployment_client.create_service(service_name, model_id, ssl_enabled=True, ssl_certificate=cert, ssl_key=key)
```

Yanıtın `create_service` işlemi hizmetinin IP adresini içerir. DNS adı IP adresine hizmetinin eşlemek için IP adresi kullanılır.

Yanıt de içeren bir __birincil anahtar__ ve __ikincil anahtar__ hizmeti kullanmak için kullanılır.

### <a name="update-your-dns-to-point-to-the-service"></a>Hizmete işaret edecek şekilde, DNS güncelleştirme

Etki alanı adınız için DNS kaydını güncelleştirmek için etki alanı adı kayıt şirketiniz tarafından sağlanan araçları kullanın. Kayıt hizmeti IP adresine işaret etmesi gerekir.

> [!NOTE]
> Kayıt şirketi ve zaman bağlı olarak (TTL) canlı etki alanı adı için yapılandırılmış, istemciler etki alanı adı çözebilmek için önce birkaç saate kadar birkaç dakika sürebilir.

### <a name="consuming-authenticated-services"></a>Kimliği doğrulanmış Hizmetleri kullanma

Aşağıdaki örnekler, Python ve C# kullanarak bir kimliği doğrulanmış servisini göstermektedir:

> [!NOTE]
> Değiştir `authkey` birincil veya ikincil anahtarı ile hizmet oluşturulurken döndürdü.

```python
from amlrealtimeai import PredictionClient
client = PredictionClient(service.ipAddress, service.port, use_ssl=True, access_token="authKey")
image_file = R'C:\path_to_file\image.jpg'
results = client.score_image(image_file)
```

```csharp
var client = new ScoringClient(host, 50051, useSSL, "authKey");
float[,] result;
using (var content = File.OpenRead(image))
    {
        IScoringRequest request = new ImageRequest(content);
        result = client.Score<float[,]>(request);
    }
```

Diğer gRPC istemciler bir authorization üstbilgisi ayarlayarak isteklerinin kimliğini doğrulayabilir. Genel yaklaşım oluşturmaktır bir `ChannelCredentials` birleştirir nesne `SslCredentials` ile `CallCredentials`. Bu isteğin yetkilendirme üst bilgisi eklenir. Belirli üstbilgileri desteği uygulama ile ilgili daha fazla bilgi için bkz: [ https://grpc.io/docs/guides/auth.html ](https://grpc.io/docs/guides/auth.html).

Aşağıdaki örnekler üstbilgi C# ve Git içinde nasıl ayarlanacağını göstermektedir:

```csharp
creds = ChannelCredentials.Create(baseCreds, CallCredentials.FromInterceptor(
                      async (context, metadata) =>
                      {
                          metadata.Add(new Metadata.Entry("authorization", "authKey"));
                          await Task.CompletedTask;
                      }));

```

```go
conn, err := grpc.Dial(serverAddr, 
    grpc.WithTransportCredentials(credentials.NewClientTLSFromCert(nil, "")),
    grpc.WithPerRPCCredentials(&authCreds{
    Key: "authKey"}))

type authCreds struct {
    Key string
}

func (c *authCreds) GetRequestMetadata(context.Context, uri ...string) (map[string]string, error) {
    return map[string]string{
        "authorization": c.Key,
    }, nil
}

func (c *authCreds) RequireTransportSecurity() bool {
    return true
}
```

### <a id="self-signed"></a>Hizmetleri otomatik olarak imzalanan sertifikalar ile kullanma

Kendinden imzalı bir sertifikayla güvenli bir sunucu kimlik doğrulaması istemci etkinleştirmenin iki yolu vardır:

* İstemci sisteminde `GRPC_DEFAULT_SSL_ROOTS_FILE_PATH` sertifika dosyasına işaret edecek şekilde İstemci sisteminde ortam değişkeni.

* Oluşturulurken bir `SslCredentials` nesnesi, sertifika dosyasının içeriğini oluşturucuya geçirin.

Her iki yöntemi kullanarak sertifikayı kök sertifikası kullanmak üzere gRPC neden olur.

> [!IMPORTANT]
> Güvenilmeyen Sertifikalar gRPC kabul etmez. Güvenilmeyen bir sertifika kullanarak başarısız olur ile bir `Unavailable` durum kodu. Hata ayrıntıları içeren `Connection Failed`.
