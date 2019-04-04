---
title: Azure stack'teki Ruby ile API Sürüm profillerini kullanma | Microsoft Docs
description: Azure stack'teki Ruby ile API Sürüm profillerini kullanma hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: B82E4979-FB78-4522-B9A1-84222D4F854B
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2019
ms.author: sethm
ms.reviewer: sijuman
ms.lastreviewed: 01/09/2019
ms.openlocfilehash: 226416688fe4f2c48981718e7a2f40b0ba5b0685
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58649620"
---
# <a name="use-api-version-profiles-with-ruby-in-azure-stack"></a>Azure stack'teki Ruby ile API Sürüm profillerini kullanma

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

## <a name="ruby-and-api-version-profiles"></a>Ruby ve API sürümü profillerini

Ruby SDK'sı için Azure Stack Kaynak Yöneticisi'ni oluşturmanıza ve altyapınızı yönetmenize yardımcı olacak araçlar sağlar. İşlem, sanal ağlar ve depolama ile Ruby dil SDK kaynak sağlayıcılarını içerir. Ruby SDK'sı API profillerinde, hibrit bulut geliştirme genel Azure kaynakları ve Azure Stack'te kaynakları arasında geçiş yardımcı olarak etkinleştirin.

Bir API profili, kaynak sağlayıcıları ve hizmet sürümlerini birleşimidir. Farklı kaynak türleri birleştirmek için bir API profili kullanabilirsiniz.

- Tüm hizmetler en son sürümlerini kullanmak için **son** Azure SDK paketi gem profili.
- Azure Stack ile uyumlu hizmetleri kullanmak için **V2018_03_01** Azure SDK paketi gem profili.
- En son kullanılacak **api sürümü** kullanın, hizmetin **en son** profilini belirli gem. Örneğin, en son kullanmak istiyorsanız, **api sürümü** işlem hizmetini tek başına, **en son** profilini **işlem** gem.
- Belirli bir kullanılacak **api sürümü** bir hizmet için gem içinde tanımlanan belirli API sürümlerini kullanın.

> [!NOTE]
> Tüm seçeneklerin aynı uygulamada birleştirebilirsiniz.

## <a name="install-the-azure-ruby-sdk"></a>Azure Ruby SDK'sını yükleyin

- Yüklemek için resmi yönergeleri [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).
- Yüklemek için resmi yönergeleri [Ruby](https://www.ruby-lang.org/en/documentation/installation/).
  - Yükleme sırasında seçin **PATH değişkenine ekleyin Ruby**.
  - Geliştirme Seti sırasında istendiğinde Ruby yüklemesini yükleyin.
  - Ardından, aşağıdaki komutu kullanarak bundler yükleyin:  
    `Gem install bundler`
- Yoksa, bir abonelik oluşturur ve daha sonra kullanılmak üzere abonelik Kimliğini kaydedin. Bir abonelik oluşturmak için yönergeler [burada](../azure-stack-subscribe-plan-provision-vm.md).
- Hizmet sorumlusu oluşturma ve onun Kimliğini ve parolasını kaydedin. Azure Stack için hizmet sorumlusu oluşturmak için yönergeler [burada](../azure-stack-create-service-principals.md).
- Hizmet sorumlusu, aboneliğinizde katkıda bulunan/sahip rolü olduğundan emin olun. Hizmet sorumlusu için rol atama hakkında yönergeler [burada](../azure-stack-create-service-principals.md).

## <a name="install-the-rubygem-packages"></a>Rubygem paketleri yükleme

Azure Rubygem paketleri doğrudan yükleyebilirsiniz.

```Ruby  
gem install azure_mgmt_compute
gem install azure_mgmt_storage
gem install azure_mgmt_resources
gem install azure_mgmt_network
Or use them in your Gemfile.
gem 'azure_mgmt_storage'
gem 'azure_mgmt_compute'
gem 'azure_mgmt_resources'
gem 'azure_mgmt_network'
```

Azure Resource Manager Ruby SDK'sı, Önizleme aşamasındadır ve gelecek sürümlerde arabirimi değişiklikler olasılıkla vardır. Artan bir ikincil sürüm numarası bozucu değişiklik gösterebilir.

## <a name="use-the-azuresdk-gem"></a>Azure_sdk gem kullanın

Gem **azure_sdk**, tüm desteklenen toprağa değerli taşlar Ruby SDK'sındaki toplamıdır. Bu gem oluşan bir **son** profili hizmetlerinin en son sürümünü destekler. İki tutulan profili tanıtır **V2017_03_09** ve **V2018_03_01** Azure Stack için yerleşik profilleri.

Aşağıdaki komutla azure_sdk toplaması gem yükleyebilirsiniz:  

```Ruby  
gem install 'azure_sdk'
```

## <a name="prerequisites"></a>Önkoşullar

Azure Ruby SDK'sı, Azure Stack ile kullanmak için aşağıdaki değerleri girin ve ardından şu değerleri içeren ortam değişkenlerini ayarlayın. Tablodan sonra sağlanan işletim sistemi ortam değişkenlerini ayarlama konusunda yönergelere bakın.

| Değer | Ortam değişkenleri | Açıklama |
| --- | --- | --- |
| Kiracı Kimliği | AZURE_TENANT_ID | Azure Stack değerini [Kiracı kimliği](https://docs.microsoft.com/azure/azure-stack/azure-stack-identity-overview). |
| İstemci Kimliği | AZURE_CLIENT_ID | Hizmet sorumlusu uygulama kimliği bu belgenin önceki bölümde üzerinde hizmet sorumlusu oluşturulurken kaydedilen.  |
| Abonelik Kimliği | AZURE_SUBSCRIPTION_ID | [Abonelik kimliği](https://docs.microsoft.com/azure/azure-stack/azure-stack-plan-offer-quota-overview#subscriptions) nasıl, teklifler eriştiği Azure Stack'te. |
| İstemci Gizli Anahtarı | AZURE_CLIENT_SECRET | Hizmet sorumlusu oluştururken hizmet sorumlusu uygulama gizli anahtarı kaydedildi. |
| Resource Manager uç noktası | ARM_ENDPOINT | Bkz: [Azure Stack Kaynak Yöneticisi uç noktası](#the-azure-stack-resource-manager-endpoint).  |

### <a name="the-azure-stack-resource-manager-endpoint"></a>Azure Stack Kaynak Yöneticisi uç noktası

Microsoft Azure Resource Manager yöneticilerin dağıtmak, yönetmek ve Azure kaynaklarınızı izlemenize olanak sağlayan bir yönetim çerçevesidir. Azure Resource Manager, bir grup olarak yerine tek tek bir işlemde bu görevleri işleyebilir.

Resource Manager uç noktasından meta veri bilgilerini alabilirsiniz. Uç nokta, kodunuzu çalıştırmak için gereken bilgileri bir JSON dosyası döndürür.

 > [!NOTE]  
 > **ResourceManagerUrl** olan Azure Stack geliştirme Seti'ni (ASDK): `https://management.local.azurestack.external/` **ResourceManagerUrl** tümleşik sisteminde: `https://management.<location>.ext-<machine-name>.masd.stbtest.microsoft.com/`  
 > Gerekli meta verilerini almak için: `<ResourceManagerUrl>/metadata/endpoints?api-version=1.0`
  
 Örnek JSON dosyası:

 ```json
 {
   "galleryEndpoint": "https://portal.local.azurestack.external:30015/",  
   "graphEndpoint": "https://graph.windows.net/",  
   "portal Endpoint": "https://portal.local.azurestack.external/",
   "authentication": {
     "loginEndpoint": "https://login.windows.net/",
     "audiences": ["https://management.<yourtenant>.onmicrosoft.com/3cc5febd-e4b7-4a85-a2ed-1d730e2f5928"]
   }
 }
```

### <a name="set-environmental-variables"></a>Ortam değişkenleri ayarlama

**Microsoft Windows**  
Bir Windows komut isteminde ortam değişkenlerini ayarlamak için aşağıdaki biçimi kullanın:  
`set AZURE_TENANT_ID=<YOUR_TENANT_ID>`

**macOS, Linux ve UNIX tabanlı sistemlerde** , UNIX veya Linux tabanlı sistemleri, aşağıdaki komutu kullanabilirsiniz:  
`export AZURE_TENANT_ID=<YOUR_TENANT_ID>`

## <a name="existing-api-profiles"></a>Mevcut API profilleri

Azure_sdk toplaması gem aşağıdaki üç profil içerir:

1. **V2018_03_01** Azure Stack için yerleşik profili. En son sürümleri Azure Stack'te sunulan hizmetinden yararlanarak için bu profili kullanın.
2. **V2017_03_09**  
  Azure Stack için yerleşik profili. Azure Stack ile en uyumlu olacak şekilde hizmetler için bu profili kullanın.
3. **en son**  
  Profil hizmetlerinin en son sürümleri içerir. Tüm hizmetler en son sürümlerini kullanın.

Azure Stack ve API profilleri hakkında daha fazla bilgi için bkz. [API özeti profilleri](azure-stack-version-profiles.md#summary-of-api-profiles).

## <a name="azure-ruby-sdk-api-profile-usage"></a>Azure Ruby SDK API profili kullanımı

Bir profili istemci örneği oluşturmak için aşağıdaki kodu kullanabilirsiniz. Bu parametre yalnızca Azure Stack veya diğer özel bulut için gereklidir. Küresel Azure, bu ayarlar varsayılan olarak zaten sahiptir.

```Ruby  
active_directory_settings = get_active_directory_settings(ENV['ARM_ENDPOINT'])

provider = MsRestAzure::ApplicationTokenProvider.new(
  ENV['AZURE_TENANT_ID'],
  ENV['AZURE_CLIENT_ID'],
  ENV['AZURE_CLIENT_SECRET'],
  active_directory_settings
)
credentials = MsRest::TokenCredentials.new(provider)
options = {
  credentials: credentials,
  subscription_id: subscription_id,
  active_directory_settings: active_directory_settings,
  base_url: ENV['ARM_ENDPOINT']
}

# Target profile built for Azure Stack
client = Azure::Resources::Profiles::V2018_03_01::Mgmt::Client.new(options)
```

Profili istemci, işlem, depolama ve ağ gibi ayrı kaynak sağlayıcıları erişmek için kullanılabilir:

```Ruby  
# To access the operations associated with Compute
profile_client.compute.virtual_machines.get 'RESOURCE_GROUP_NAME', 'VIRTUAL_MACHINE_NAME'

# Option 1: To access the models associated with Compute
purchase_plan_obj = profile_client.compute.model_classes.purchase_plan.new

# Option 2: To access the models associated with Compute
# Notice Namespace: Azure::Profiles::<Profile Name>::<Service Name>::Mgmt::Models::<Model Name>
purchase_plan_obj = Azure::Profiles::V2018_03_01::Compute::Mgmt::Models::PurchasePlan.new
```

## <a name="define-azure-stack-environment-setting-functions"></a>Azure Stack ortamı ayarı işlevleri tanımlayın

Azure Stack ortamına hizmet sorumlusunun kimliğini doğrulamak için kullanarak uç noktaları tanımlamak `get_active_directory_settings()`. Bu yöntemde **ARM_Endpoint** ortam değişkenlerinizi oluşturulurken ayarlanan ortam değişkeni:

```Ruby  
# Get Authentication endpoints using Arm Metadata Endpoints
def get_active_directory_settings(armEndpoint)
  settings = MsRestAzure::ActiveDirectoryServiceSettings.new
  response = Net::HTTP.get_response(URI("#{armEndpoint}/metadata/endpoints?api-version=1.0"))
  status_code = response.code
  response_content = response.body
  unless status_code == "200"
    error_model = JSON.load(response_content)
    fail MsRestAzure::AzureOperationError.new("Getting Azure Stack Metadata Endpoints", response, error_model)
  end
  result = JSON.load(response_content)
  settings.authentication_endpoint = result['authentication']['loginEndpoint'] unless result['authentication']['loginEndpoint'].nil?
  settings.token_audience = result['authentication']['audiences'][0] unless result['authentication']['audiences'][0].nil?
  settings
end
```

## <a name="samples-using-api-profiles"></a>API profillerini kullanma örnekleri

GitHub üzerinde Ruby ve Azure Stack API profilleriyle çözümleri oluşturmak için başvuru olarak bulunan aşağıdaki örnekleri kullanabilirsiniz:

- [Ruby ile Azure kaynaklarını ve kaynak gruplarını yönetme](https://github.com/Azure-Samples/Hybrid-Resource-Manager-Ruby-Resources-And-Groups)
- [Ruby kullanarak sanal makineleri yönetme](https://github.com/Azure-Samples/Hybrid-Compute-Ruby-Manage-VM)
- [SSH dağıtma Ruby bir şablon ile VM etkin](https://github.com/Azure-Samples/Hybrid-Resource-Manager-Ruby-Template-Deployment)

### <a name="sample-resource-manager-and-groups"></a>Örnek Resource Manager ve gruplar

Örneği çalıştırmak için Ruby yüklediğinizden emin olun. Visual Studio Code kullanıyorsanız de Ruby SDK'sı uzantısını indirin.

> [!NOTE]  
> Örneğine depo alabilirsiniz "[Azure kaynaklarınızı yönetme ve kaynak gruplarıyla Ruby](https://github.com/Azure-Samples/Hybrid-Resource-Manager-Ruby-Resources-And-Groups)".

1. Deposunu kopyalayın:

   ```bash
   git clone https://github.com/Azure-Samples/Hybrid-Resource-Manager-Ruby-Resources-And-Groups.git
   ```

2. Paket kullanarak bağımlılıkları yükler:

   ```Bash
   cd Hybrid-Resource-Manager-Ruby-Resources-And-Groups
   bundle install
   ```

3. Azure PowerShell kullanarak hizmet sorumlusu oluşturma ve gerekli değerleri alır.

   Bir hizmet sorumlusu oluşturma hakkında yönergeler için bkz: [bir sertifika ile hizmet sorumlusu oluşturmak için Azure PowerShell kullanarak](../azure-stack-create-service-principals.md).

   Gerekli değerler şunlardır:
   - Kiracı Kimliği
   - İstemci Kimliği
   - İstemci Gizli Anahtarı
   - Abonelik Kimliği
   - Resource Manager uç noktası

   Oluşturduğunuz hizmet Sorumlusundan alınan bilgileri kullanarak aşağıdaki ortam değişkenlerini ayarlayın.

   - dışarı aktarma AZURE_TENANT_ID {Kiracı kimliğinizi} =
   - dışarı aktarma AZURE_CLIENT_ID {istemci kimliğiniz} =
   - dışarı aktarma AZURE_CLIENT_SECRET {istemci gizli anahtarı} =
   - dışarı aktarma AZURE_SUBSCRIPTION_ID {abonelik kimliğinizi} =
   - dışarı aktarma ARM_ENDPOINT {AzureStack Resource manager URL'nizi} =

   > [!NOTE]  
   > Windows üzerinde dışarı aktarma yerine kullanın.

4. Azure Stack konumunuza konumu değişkeninin ayarlandığından emin olun; Örneğin, `LOCAL="local"`.

5. Aşağıdaki kod satırını ekleyin, şu active directory uç noktalarına hedeflemek için Azure Stack veya diğer özel bulut kullanarak:

   ```Ruby  
   active_directory_settings = get_active_directory_settings(ENV['ARM_ENDPOINT'])
   ```

6. Seçenekleri değişkeninde, Active Directory ayarları ve Azure Stack ile çalışmak için temel URL Ekle:

   ```ruby  
   options = {
   credentials: credentials,
   subscription_id: subscription_id,
   active_directory_settings: active_directory_settings,
   base_url: ENV['ARM_ENDPOINT']
   }
   ```

7. Azure Stack profili hedefleyen bir profili istemci oluşturun:

   ```ruby  
   client = Azure::Resources::Profiles::V2018_03_01::Mgmt::Client.new(options)
   ```

8. Azure Stack ile hizmet sorumlusunun kimliğini doğrulamak için uç noktaları kullanarak tanımlanmalıdır **get_active_directory_settings()**. Bu yöntemde **ARM_Endpoint** ortam değişkenlerinizi oluşturulurken ayarlanan ortam değişkeni:

   ```ruby  
   def get_active_directory_settings(armEndpoint)
     settings = MsRestAzure::ActiveDirectoryServiceSettings.new
     response = Net::HTTP.get_response(URI("#{armEndpoint}/metadata/endpoints?api-version=1.0"))
     status_code = response.code
     response_content = response.body
     unless status_code == "200"
       error_model = JSON.load(response_content)
       fail MsRestAzure::AzureOperationError.new("Getting Azure Stack Metadata Endpoints", response, error_model)
     end
     result = JSON.load(response_content)
     settings.authentication_endpoint = result['authentication']['loginEndpoint'] unless result['authentication']['loginEndpoint'].nil?
     settings.token_audience = result['authentication']['audiences'][0] unless result['authentication']['audiences'][0].nil?
     settings
   end
   ```

9. Örnek uygulamayı çalıştırın.

   ```ruby
   bundle exec ruby example.rb
   ```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack için PowerShell yükleme](azure-stack-powershell-install.md)
- [Azure Stack kullanıcının PowerShell ortamını yapılandırma](azure-stack-powershell-configure-user.md)  
