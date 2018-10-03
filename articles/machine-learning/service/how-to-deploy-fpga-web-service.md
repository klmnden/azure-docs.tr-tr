---
title: Model bir FPGA Azure Machine Learning ile bir web hizmeti olarak dağıtma
description: Azure Machine Learning ile bir FPGA üzerinde çalışan bir modeli ile bir web hizmetini dağıtma konusunda bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: tedway
author: tedway
ms.date: 10/01/2018
ms.openlocfilehash: df6637f1a52b679ba9ad0a49fb37d4e4b72f35e4
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2018
ms.locfileid: "48237832"
---
# <a name="deploy-a-model-as-a-web-service-on-an-fpga-with-azure-machine-learning"></a>Model bir FPGA Azure Machine Learning ile bir web hizmeti olarak dağıtma

Bir model üzerinde bir web hizmeti olarak dağıtabilirsiniz [alan programlanabilir kapı dizileri (FPGA)](concept-accelerate-with-fpgas.md).  FPGA kullanarak, bir tek bir toplu iş boyutu ile bile son derece düşük gecikme süresi çıkarım sağlar.   

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

- İster gerekir ve FPGA kotasının onaylanması gerekiyor. Erişim istemek için kota istek formunu doldurun: https://aka.ms/aml-real-time-ai

- Bir Azure Machine Learning hizmeti çalışma alanında ve yüklü Python için Azure Machine Learning SDK'sı. Kullanarak şu önkoşul olarak gerekenleri edinin öğrenin [bir geliştirme ortamı yapılandırma](how-to-configure-environment.md) belge.
 
  - Çalışma alanınızda yer alması gerekir *Doğu ABD 2* bölge.

  - Contrib ek özellikler yükleyin:

    ```shell
    pip install --upgrade azureml-sdk[contrib]
    ```  

## <a name="create-and-deploy-your-model"></a>Modelinizi oluşturup
Girdi görüntüsünün, özellik kazandırın ResNet-50 üzerinde bir FPGA kullanarak önceden işleme için işlem hattı oluşturma ve Imagenet veri kümesinde eğitilmiş bir classifer aracılığıyla Özellikler'i çalıştırın.

Yönergeleri izleyin:

* Model işlem hattı tanımlayın
* Modeli dağıtma
* Dağıtılan modeli kullanma
* Dağıtılan Hizmetleri Sil

> [!IMPORTANT]
> Gecikme süresi ve aktarım hızını iyileştirmek için istemci uç noktası ile aynı Azure bölgesinde olmalıdır.  Şu anda API'leri, Doğu ABD Azure bölgesi oluşturulur.



### <a name="preprocess-image"></a>Görüntü önceden işlenir
İlk aşamada işlem hattının görüntüleri önişle sağlamaktır.

```python
import os
import tensorflow as tf

# Input images as a two-dimensional tensor containing an arbitrary number of images represented a strings
import azureml.contrib.brainwave.models.utils as utils
in_images = tf.placeholder(tf.string)
image_tensors = utils.preprocess_array(in_images)
print(image_tensors.shape)
```

### <a name="add-featurizer"></a>Özelliği Oluşturucu Ekle
Model başlatın ve bir TensorFlow denetim noktası bir özelliği Oluşturucu kullanılacak ResNet50 quantized sürümünü indirin.

```python
from azureml.contrib.brainwave.models import QuantizedResnet50, Resnet50
model_path = os.path.expanduser('~/models')
model = QuantizedResnet50(model_path, is_frozen = True)
feature_tensor = model.import_graph_def(image_tensors)
print(model.version)
print(feature_tensor.name)
print(feature_tensor.shape)
```

### <a name="add-classifier"></a>Sınıflandırıcı Ekle
Bu sınıflandırıcı Imagenet veri kümesinde eğitim.

```python
classifier_input, classifier_output = Resnet50.get_default_classifier(feature_tensor, model_path)
```

### <a name="create-service-definition"></a>Hizmet tanımı oluşturma
Ön işleme görüntü özelliği oluşturucu ve hizmetin üzerinde çalıştığı sınıflandırıcı el edindikten sonra bir hizmet tanımı oluşturabilirsiniz. Hizmet tanımı FPGA hizmete dağıtılan modelinden oluşturulan dosyalar kümesidir. Hizmet tanımı, bir işlem hattı oluşur. İşlem hattı, sırayla çalıştırılır aşamaları dizisidir.  Aşamaları TensorFlow, Keras aşamaları ve BrainWave aşamaları desteklenir.  Aşamalar sırayla her sonraki aşama aşama giriş çıktısı ile hizmet üzerinde çalışır.

TensorFlow aşama oluşturun, grafiğin (Bu durumda varsayılan grafik kullanılır) ve giriş içeren bir oturum belirtin ve bu aşamaya tensors çıkış için.  Bu bilgiler, hizmette çalıştırmak üzere grafiği kaydetmek için kullanılır.

```python
from azureml.contrib.brainwave.pipeline import ModelDefinition, TensorflowStage, BrainWaveStage

save_path = os.path.expanduser('~/models/save')
model_def_path = os.path.join(save_path, 'service_def.zip')

model_def = ModelDefinition()
with tf.Session() as sess:
    model_def.pipeline.append(TensorflowStage(sess, in_images, image_tensors))
    model_def.pipeline.append(BrainWaveStage(sess, model))
    model_def.pipeline.append(TensorflowStage(sess, classifier_input, classifier_output))
    model_def.save(model_def_path)
    print(model_def_path)
```

### <a name="deploy-model"></a>Model dağıtma
Bir hizmeti hizmet tanımını oluşturun.  Çalışma alanınız, Doğu ABD 2 konumunda olması gerekiyor.

```python
from azureml.core import Workspace

ws = Workspace.from_config()
print(ws.name, ws.resource_group, ws.location, ws.subscription_id, sep = '\n')

from azureml.core.model import Model
model_name = "resnet-50-rtai"
registered_model = Model.register(ws, model_def_path, model_name)

from azureml.core.webservice import Webservice
from azureml.exceptions import WebserviceException
from azureml.contrib.brainwave import BrainwaveWebservice, BrainwaveImage
service_name = "imagenet-infer"
service = None
try:
    service = Webservice(ws, service_name)
except WebserviceException:
    image_config = BrainwaveImage.image_configuration()
    deployment_config = BrainwaveWebservice.deploy_configuration()
    service = Webservice.deploy_from_model(ws, service_name, [registered_model], image_config, deployment_config)
    service.wait_for_deployment(true)
```

### <a name="test-the-service"></a>Test hizmeti
API için bir görüntü gönderme ve yanıt sınamak için bir eşleme Imagenet sınıf adı çıkış sınıfı kimliği ekleyin.

```python
import requests
classes_entries = requests.get("https://raw.githubusercontent.com/Lasagne/Recipes/master/examples/resnet50/imagenet_classes.txt").text.splitlines()
```

Hizmetinizi arayın ve aşağıda "image.jpg bilgisayarınızı" dosya adı, makineden bir görüntü ile değiştirin. 

```python
with open('your-image.jpg') as f:
    results = service.run(f)
# map results [class_id] => [confidence]
results = enumerate(results)
# sort results by confidence
sorted_results = sorted(results, key=lambda x: x[1], reverse=True)
# print top 5 results
for top in sorted_results[:5]:
    print(classes_entries[top[0]], 'confidence:', top[1])
``` 

### <a name="clean-up-service"></a>Hizmetini kurma Temizle
Hizmeti silin.

```python
service.delete()
    
registered_model.delete()
```

## <a name="secure-fpga-web-services"></a>FPGA web hizmetlerini güvence altına alma

Azure Machine Learning modellerini FPGA üzerinde çalışan, SSL desteği ve anahtar tabanlı kimlik doğrulaması sağlar. Bu, hizmet ve istemcileri tarafından gönderilen güvenli veri erişimi sınırlamak sağlar.

> [!IMPORTANT]
> Kimlik doğrulaması, yalnızca bir SSL sertifikası ve anahtar sağladığınız Hizmetleri için etkindir. 
>
> SSL etkinleştirmezseniz, internet üzerindeki herhangi bir kullanıcı hizmete çağrı yapmak mümkün olacaktır.
>
> Hizmet erişirken etkinleştirirseniz SSL ve kimlik doğrulama anahtarı gereklidir.

SSL istemci ve hizmet arasında gönderilen verileri şifreler. Bu da sunucunun kimliğini doğrulamak için istemci tarafından kullanılmış.

SSL etkin bir hizmet dağıtmak veya etkinleştirmek için zaten dağıtılmış bir hizmeti güncelleştirin. Aynı adımlar şunlardır:

1. Bir etki alanı adı satın alır.

2. Bir SSL sertifikası alın.

3. Dağıtın veya hizmeti SSL etkin ile güncelleştirin.

4. Hizmete işaret edecek şekilde güncelleştirin.

### <a name="acquire-a-domain-name"></a>Bir etki alanı adını alma

Zaten bir etki alanı adına sahip olmadığınız, birinden, satın alabileceğiniz bir __etki alanı adı kayıt şirketi__. Maliyeti gibi işlemi kaydedicilerin arasında farklılık gösterir. Kayıt şirketi, araçlar da ile etki alanı adı yönetmek için sağlar. Bu araçlar, barındırılan hizmetinizin IP adresi için bir tam etki alanı adı (örneğin, www.contoso.com) eşlemek için kullanılır.

### <a name="acquire-an-ssl-certificate"></a>Bir SSL sertifikası alın

Bir SSL sertifikası almak için birçok yolu vardır. En sık karşılaşılan bir satın almaktır bir __sertifika yetkilisi__ (CA). Sertifika burada elde bağımsız olarak, aşağıdaki dosyaları gerekir:

* A __sertifika__. Sertifikayı tüm sertifika zincirine içermelidir ve PEM kodlu olmalıdır.
* A __anahtar__. Bu anahtar, PEM kodlu olmalıdır.

> [!TIP]
> Sertifika yetkilisi sertifika ve anahtarı sağlayamazsa ve PEM kodlu dosyaları olarak gibi bir yardımcı programını kullanın [OpenSSL](https://www.openssl.org/) biçimini değiştirmek için.

> [!IMPORTANT]
> Otomatik olarak imzalanan sertifikalar yalnızca geliştirme için kullanılması gerekir. Bunlar üretim ortamında kullanılmamalıdır.
>
> Kendinden imzalı bir sertifika kullanıyorsanız bkz [otomatik olarak imzalanan sertifikalarla tüketen](#self-signed) bölümüne ilişkin özel yönergeler.

> [!WARNING]
> Sertifika isterken, hizmet için kullanmayı planladığınız adresinin tam etki alanı adı (FQDN) sağlamalısınız. Örneğin, www.contoso.com. Sertifikayı damgalı adresi ve istemciler tarafından kullanılan adres, hizmetin kimliğini doğrularken karşılaştırılır.
>
> Adresleri eşleşmiyorsa, istemcilerin bir hata alırsınız. 

### <a name="deploy-or-update-the-service-with-ssl-enabled"></a>Dağıtma veya SSL ile etkin hizmetini güncelleştir

SSL etkin hizmeti dağıtmak için ayarlanmış `ssl_enabled` parametresi `True`. Ayarlama `ssl_certificate` parametre değerine __sertifika__ dosya ve `ssl_key` değerine __anahtarı__ dosya. Aşağıdaki örnek, SSL etkin bir hizmeti dağıtmak gösterir:

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

Yanıtın `create_service` işlem hizmetinin IP adresini içeriyor. Hizmetin IP adresine DNS adı eşleme IP adresi kullanılır.

Yanıtını da içeren bir __birincil anahtar__ ve __ikincil anahtar__ hizmeti kullanmak için kullanılır.

### <a name="update-your-dns-to-point-to-the-service"></a>Hizmete işaret edecek şekilde güncelleştirin

Etki alanı adınız için DNS kaydı güncelleştirmek için etki alanı adı kayıt şirketi tarafından sağlanan araçları kullanın. Kayıt hizmetinin IP adresine işaret etmelidir.

> [!NOTE]
> Bağlı olarak kayıt şirketi ve süresi (TTL) canlı etki alanı adı için yapılandırılmış, istemciler etki alanı adı çözümleyebilir önce birkaç saate kadar birkaç dakika sürebilir.

### <a name="consume-authenticated-services"></a>Kimliği doğrulanmış hizmetlerini kullanma

Aşağıdaki örnekler, Python ve C# kullanarak bir kimliği doğrulanmış hizmetinin nasıl kullanılacağı hakkında göstermektedir:

> [!NOTE]
> Değiştirin `authkey` birincil veya ikincil anahtarı ile hizmet oluşturma sırasında döndürdü.

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

Diğer gRPC istemciler bir yetkilendirme üst bilgisi ayarlayarak isteklerinin kimliğini doğrulayabilir. Genel yaklaşım oluşturmaktır bir `ChannelCredentials` birleştiren nesne `SslCredentials` ile `CallCredentials`. Bu isteğin yetkilendirme üst bilgisi eklenir. Belirli üstbilgilerinizin desteği uygulama konusunda daha fazla bilgi için bkz. [ https://grpc.io/docs/guides/auth.html ](https://grpc.io/docs/guides/auth.html).

Aşağıdaki örnekler, C# ve Git üst bilgi ayarlama işlemini göstermektedir:

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

### <a id="self-signed"></a>Otomatik olarak imzalanan sertifikalarla hizmetlerini kullanma

Otomatik olarak imzalanan bir sertifika ile güvenli bir sunucu kimlik doğrulaması istemci etkinleştirmek için iki yolu vardır:

* İstemci sisteminde `GRPC_DEFAULT_SSL_ROOTS_FILE_PATH` sertifika dosyasına işaret edecek şekilde İstemci sisteminde ortam değişkeni.

* Oluştururken bir `SslCredentials` nesnesine, yapıcısına sertifika dosyasının içeriğini geçirin.

Her iki yöntemi kullanarak sertifikayı kök sertifika kullanmak üzere gRPC neden olur.

> [!IMPORTANT]
> gRPC güvenilmeyen sertifikaları kabul etmiyor. Güvenilmeyen bir sertifika kullanarak başarısız olacak olan bir `Unavailable` durum kodu. Hata ayrıntılarını içeren `Connection Failed`.

## <a name="sample-notebook"></a>Örnek Not Defteri

Bu makaledeki kavramları içinde gösterilen `project-brainwave/project-brainwave-quickstart.ipynb` dizüstü bilgisayar.

Bu not alın:

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]
