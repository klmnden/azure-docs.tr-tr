---
title: Azure yığınında Ruby API sürümü profilleri kullanarak | Microsoft Docs
description: Azure yığınında Ruby ile API sürümü profillerini kullanma hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: B82E4979-FB78-4522-B9A1-84222D4F854B
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2018
ms.author: mabrigg
ms.reviewer: sijuman
ms.openlocfilehash: dd8130ac12f9c7c2095f9329dc4ce8a34187cf62
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="use-api-version-profiles-with-ruby-in-azure-stack"></a>Azure yığınında Ruby ile API sürümü profilleri kullanma

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

## <a name="ruby-and-api-version-profiles"></a>Ruby ve API sürümü profilleri

Ruby SDK'sı için Azure yığın Resource Manager yapı ve altyapınızı yönetmenize yardımcı olan araçlar sağlar. Kaynak sağlayıcıları SDK işlem, sanal ağlar ve depolama ile Söyleniş dil içerir. Ruby SDK API profillerinde karma bulut geliştirme genel Azure kaynakları ve Azure yığında kaynaklarına arasında geçiş yardımcı olarak etkinleştirin.

Bir API profili, kaynak sağlayıcıları ve hizmet sürümleri birleşimidir. Bir API profili, farklı kaynak türlerini birleştirmek için kullanın.

 - Tüm hizmetler en son sürümüne, kullanmanız yapmak için **son** Azure SDK paketi gem profili.
 - Azure yığın ile uyumlu hizmetleri kullanmak için **V2017_03_09** Azure SDK paketi gem profili.
 - Son API sürümü, bir hizmetin kullanmak için **son** belirli gem profili. Örneğin, son API sürümü işlem hizmetinin tek başına kullanmak istiyorsanız, kullanmak **son** profilini **işlem** gem.
 - Bir hizmet için belirli bir API sürümü kullanmak için gem içinde tanımlanmış özel API sürümü kullanın.

> [!Note]   
> Aynı uygulama seçeneklerin tümü birleştirebilirsiniz.

## <a name="install-the-azure-ruby-sdk"></a>Azure Söyleniş SDK'sını yükleyin

 - Yüklemek için resmi yönergeleri [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).
 - Yüklemek için resmi yönergeleri [Ruby](https://www.ruby-lang.org/en/documentation/installation/).
    - Yüklenirken seçin **yol değişkenine Ruby ekleme**
    - Geliştirme Seti istendiğinde Söyleniş yükleme sırasında yükleyin.
    - Ardından, aşağıdaki komutu kullanarak Paketleyici yükleyin:  
      `Gem install bundler`
 - Yoksa, bir abonelik oluşturun ve daha sonra kullanılmak üzere abonelik Kimliğini kaydedin. Bir aboneliği oluşturmak için yönergeler [burada](https://docs.microsoft.com/azure/azure-stack/azure-stack-subscribe-plan-provision-vm). 
 - Bir hizmet sorumlusu oluşturun ve kendi Kimliğini ve parolasını kaydedin. Azure yığını için hizmet sorumlusu oluşturmak için yönergeler [burada](https://docs.microsoft.com/azure/azure-stack/azure-stack-create-service-principals). 
 - Hizmet sorumlusu katılımcı/sahip rolü, aboneliğinizde olduğundan emin olun. Hizmet sorumlusuna rol atama hakkında yönergeler [burada](https://docs.microsoft.com/azure/azure-stack/azure-stack-create-service-principals).

## <a name="install-the-rubygem-packages"></a>Rubygem paket yüklemek için

Doğrudan azure rubygem paketlerini yükleyebilirsiniz.

````Ruby  
gem install azure_mgmt_compute
gem install azure_mgmt_storage
gem install azure_mgmt_resources
gem install azure_mgmt_network
Or use them in your Gemfile.
gem 'azure_mgmt_storage'
gem 'azure_mgmt_compute'
gem 'azure_mgmt_resources'
gem 'azure_mgmt_network'
````

Azure Resource Manager Ruby SDK'sını Önizleme aşamasındadır ve olasılıkla gelecek sürümlerde arabirimi değişiklikler vardır unutmayın. Artan bir ikincil sürüm numarası önemli değişiklikler gösterebilir.

## <a name="usage-of-the-azuresdk-gem"></a>Azure_sdk gem kullanımı

Gem azure_sdk, desteklenen tüm gems Ruby SDK'sındaki toplu paketidir. Bu gem oluşan bir **son** tüm hizmetleri en son sürümünü destekleyen profili. Sürümü tutulan profili tanıtır **V2017_03_09** Azure yığını için yerleşik profili.

Aşağıdaki komutla azure_sdk toplaması gem yükleyebilirsiniz:  

````Ruby  
  gem install 'azure_sdk
````

## <a name="prerequisite"></a>Önkoşul

Ruby Azure SDK'sı Azure yığın ile kullanmak için aşağıdaki değerleri sağlayın ve ortam değişkenleri değerlerle ayarlayın. Ortam değişkenlerini ayarlama bir işletim sistemi için tablodan sonra yönergelere bakın. 

| Değer | Ortam değişkenleri | Açıklama | 
| --- | --- | --- | --- |
| Kiracı Kimliği | AZURE_TENANT_ID | Azure yığın değerini [kimliği Kiracı](https://docs.microsoft.com/azure/azure-stack/azure-stack-identity-overview). |
| İstemci Kimliği | AZURE_CLIENT_ID | Hizmet sorumlusu, bu belgenin önceki bölümünde bulunan oluşturulduğunda asıl uygulama kimliği kaydedilmiş hizmeti.  |
| Abonelik Kimliği | AZURE_SUBSCRIPTION_ID | [Abonelik kimliği](https://docs.microsoft.com/azure/azure-stack/azure-stack-plan-offer-quota-overview#subscriptions) teklifleri nasıl eriştiği Azure yığınında. |
| İstemci Gizli Anahtarı | AZURE_CLIENT_SECRET | Hizmet sorumlusu oluşturulduğu sırada hizmet asıl uygulama gizli anahtarı kaydedildi. |
| Kaynak Yöneticisi uç noktası | ARM_ENDPOINT | Bkz: [Azure yığın Kaynak Yöneticisi endpoin](#The-azure-stack-resource-manager-endpoint).  |

### <a name="the-azure-stack-resource-manager-endpoint"></a>Azure yığın Kaynak Yöneticisi uç noktası

Microsoft Azure Resource Manager yöneticilerin dağıtmak, yönetmek ve Azure kaynaklarını izlemenize olanak sağlayan bir yönetim çerçevedir. Azure Resource Manager, bir grup olarak yerine tek tek tek bir işlemde bu görevleri işleyebilir.

Resource Manager uç noktasından meta veri bilgilerini alabilirsiniz. Uç nokta kodunuzu çalıştırmak için gerekli bilgileri bir JSON dosyası döndürür.

  > [!Note]  
  > **ResourceManagerUrl** Azure yığın Geliştirme Seti (ASDK) olan: `https://management.local.azurestack.external/`  
  > **ResourceManagerUrl** tümleşik sistemlerindeki: `https://management.<location>.ext-<machine-name>.masd.stbtest.microsoft.com/`  
  > Gerekli meta verilerini almak için: `<ResourceManagerUrl>/metadata/endpoints?api-version=1.0`
  
  Örnek JSON dosyası:

  ```json
  { "galleryEndpoint": "https://portal.local.azurestack.external:30015/",  
    "graphEndpoint": "https://graph.windows.net/",  
    "portal Endpoint": "https://portal.local.azurestack.external/", 
    "authentication": {
      "loginEndpoint": "https://login.windows.net/", 
      "audiences": ["https://management.<yourtenant>.onmicrosoft.com/3cc5febd-e4b7-4a85-a2ed-1d730e2f5928"]
    }
  }
  ```

### <a name="set-environmental-variables"></a>Ortam değişkenlerini ayarlama

**Microsoft Windows**  
Windows Komut İstemi'nde ortam değişkenlerini ayarlamak için aşağıdaki biçimi kullanın:  
`set AZURE_TENANT_ID=<YOUR_TENANT_ID>`

**macOS, Linux ve UNIX tabanlı sistemler**  
UNIX tabanlı sistemlerde gibi komutunu kullanabilirsiniz:  
`export AZURE_TENANT_ID=<YOUR_TENANT_ID>`

## <a name="existing-api-profiles"></a>Varolan API profilleri

Azure_sdk toplaması gem aşağıdaki iki profilleri vardır:

1. **V2017_03_09**  
  Azure yığını için yerleşik profili. Azure yığın ile en uyumlu olacak şekilde Hizmetleri için bu profili kullanın.
2. **en son**  
  Profil tüm hizmetleri en son sürümlerini oluşur. Tüm hizmetler en son sürümlerini kullanın.

Azure yığını ve API profilleri hakkında daha fazla bilgi için bir [API özeti profilleri](azure-stack-version-profiles.md#summary-of-api-profiles).

## <a name="azure-ruby-sdk-api-profile-usage"></a>Azure Ruby SDK API'si profili kullanımı

Aşağıdaki satırları, profil istemci örneği oluşturmak için kullanılmalıdır. Bu parametre yalnızca olan Azure yığın veya diğer özel Bulutlar için gerekli. Genel Azure, bu ayarlar varsayılan olarak zaten sahiptir.

````Ruby  
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
client = Azure::Resources::Profiles::V2017_03_09::Mgmt::Client.new(options)
````

Profil istemci işlem, depolama ve ağ gibi tek tek kaynak sağlayıcıları erişmek için kullanılabilir.

````Ruby  
# To access the operations associated with Compute
profile_client.compute.virtual_machines.get 'RESOURCE_GROUP_NAME', 'VIRTUAL_MACHINE_NAME'

# Option 1: To access the models associated with Compute
purchase_plan_obj = profile_client.compute.model_classes.purchase_plan.new

# Option 2: To access the models associated with Compute
# Notice Namespace: Azure::Profiles::<Profile Name>::<Service Name>::Mgmt::Models::<Model Name>
purchase_plan_obj = Azure::Profiles::V2017_03_09::Compute::Mgmt::Models::PurchasePlan.new
````

## <a name="define-azurestack-environment-setting-functions"></a>AzureStack ortamı ayarı işlevleri tanımlama

Azure yığın ortamına hizmet sorumlusunun kimliğini doğrulamak için kullanarak uç noktaları tanımlamak **get_active_directory_settings()**. Bu yöntem **ARM_Endpoint** ortam değişkenleri oluşturulurken ayarlanan ortam değişkeni.

````Ruby  
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
````

## <a name="samples-using-api-profiles"></a>API profillerini kullanma örnekleri

Kullanabileceğiniz aşağıdaki örnekler, Ruby ve Azure yığını API'si profilleriyle çözümleri oluşturma bir başvuru olarak GitHub repositoreis bulundu:

 - [Ruby ile Azure kaynaklarını ve kaynak gruplarını yönetme](https://github.com/Azure-Samples/resource-manager-ruby-resources-and-groups/tree/master/Hybrid)
 - [Ruby kullanarak sanal makineleri yönetme](https://github.com/Azure-Samples/compute-ruby-manage-vm/tree/master/Hybrid)
 - [Bir SSH dağıtmak Ruby bir şablon ile VM etkin](https://github.com/Azure-Samples/resource-manager-ruby-template-deployment/tree/master/Hybrid)

### <a name="sample-resource-manager-and-groups"></a>Örnek Resource Manager ve gruplar

Örneği çalıştırmak için Ruby yüklediğinizden emin olun. Visual Studio Code kullanıyorsanız, bir uzantı Ruby SDK'sını indirin. 

> [!Note]  
> Örnek: için depo elde edebilirsiniz "[yönetmek Azure kaynakları ve kaynak gruplarıyla Ruby](https://github.com/Azure-Samples/resource-manager-ruby-resources-and-groups/tree/master/Hybrid)".

1. Depoyu kopyalayın.

    ````Bash
    git clone https://github.com/Azure-Samples/resource-manager-ruby-resources-and-groups.git
    ````

2. Paket kullanarak bağımlılıkları yükler.

    ````Bash
    cd resource-manager-ruby-resources-and-groups\Hybrid\
    bundle install
    ````

3. PowerShell kullanarak bir Azure hizmet sorumlusu oluşturmak ve gerekli değerleri alabilirsiniz. 

  Bir hizmet sorumlusu oluşturma ile ilgili yönergeler için bkz: [bir sertifika ile bir hizmet sorumlusu oluşturmak için kullanım Azure PowerShell](https://docs.microsoft.com/azure/azure-stack/azure-stack-create-service-principals).

  Gerekli değerler şunlardır:
  - Kiracı Kimliği
  - İstemci Kimliği
  - İstemci Gizli Anahtarı
  - Abonelik Kimliği
  - Kaynak Yöneticisi uç noktası

  Oluşturduğunuz hizmet asıl öğesinden alınan bilgileri kullanarak aşağıdaki ortam değişkenleri ayarlayın.

  - AZURE_TENANT_ID ver = {Kiracı kimliğiniz}
  - AZURE_CLIENT_ID ver = {istemci kimliğiniz}
  - AZURE_CLIENT_SECRET verme {istemci parolanızı} =
  - AZURE_SUBSCRIPTION_ID ver = {abonelik kimliğiniz}
  - ARM_ENDPOINT verme {AzureStack Kaynak Yöneticisi URL'nizi} =

  > [!Note]  
  > Windows üzerinde yerine verme kümesi kullanın.

4. Konum değişkeni AzureStack konumunuza ayarlandığından emin olun. Örneğin yerel "yerel" =

5. Aşağıdaki kod satırını ekleyin sağ active directory uç noktaları hedeflemek için Azure yığınını veya diğer özel Bulutlar kullanarak.

  ````Ruby  
  active_directory_settings = get_active_directory_settings(ENV['ARM_ENDPOINT'])
  ````

6. Seçenekler değişkeni içinde active directory ayarları ve Azure yığın ile çalışmak için temel URL ekleyin. 

  ````Ruby  
  options = {
    credentials: credentials,
    subscription_id: subscription_id,
    active_directory_settings: active_directory_settings,
    base_url: ENV['ARM_ENDPOINT']
  }
  ````

7. Azure yığın profili hedefler profili istemci oluşturun:

  ````Ruby  
    client = Azure::Resources::Profiles::V2017_03_09::Mgmt::Client.new(options)
  ````

8. Azure yığını ile hizmet sorumlusunun kimliğini doğrulamak için uç noktalar kullanılarak tanımlanması gerekir **get_active_directory_settings()**. Bu yöntem **ARM_Endpoint** ortam değişkenleri oluşturulurken ayarlanan ortam değişkeni.

  ````Ruby  
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
  ````

9. Örnek uygulamayı çalıştırın.

  ````Ruby
    bundle exec ruby example.rb
  ````

## 

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stack için PowerShell yükleme](azure-stack-powershell-install.md)
* [Azure yığın kullanıcının PowerShell ortamını yapılandırma](azure-stack-powershell-configure-user.md)  
