---
title: Azure stack'teki Java ile API Sürüm profillerini kullanma | Microsoft Docs
description: Azure stack'teki Java ile API Sürüm profillerini kullanma hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/02/2019
ms.author: sethm
ms.reviewer: sijuman
ms.lastreviewed: 09/28/2018
ms.openlocfilehash: 0a2a42860ad4487f470aea9c4d2be8eba1fbe8ab
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58802856"
---
# <a name="use-api-version-profiles-with-java-in-azure-stack"></a>Java'da Azure Stack ile API Sürüm profillerini kullanma

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Java SDK'sı için Azure Stack Kaynak Yöneticisi'ni oluşturmanıza ve altyapınızı yönetmenize yardımcı olacak araçlar sağlar. İşlem, ağ, depolama, uygulama hizmetleri, SDK kaynak sağlayıcılarını içerir ve [KeyVault](../../key-vault/key-vault-whatis.md). Java SDK API profillerini .java dosyasının doğru modülleri yükler Pom.xml dosyasında bağımlılıklar dahil ederek içerir. Ancak, bağımlılık birden çok profilleri gibi ekleyebilirsiniz **2018-03-01-karma**, veya **son**, Azure profil olarak. Kaynak türünüzü oluşturduğunuzda, kullanmak istediğiniz bu profillerden API sürümünü seçebilir ve böylelikle kullanarak bu bağımlılıklar doğru modülü yükler. Bu Azure Stack için en güncel API sürümlerine karşı geliştirirken en son sürümlerini Azure'da kullanmanıza olanak sağlar. Java SDK'sını kullanarak bir gerçek hibrit bulut geliştirici deneyimi sağlar. Java SDK API profillerini, hibrit bulut geliştirme genel Azure kaynakları ve Azure Stack'te kaynakları arasında geçiş yardımcı olarak etkinleştirin.

## <a name="java-and-api-version-profiles"></a>Java ve API sürümü profillerini

Bir API profili, kaynak sağlayıcıları ve API sürümlerini birleşimidir. Bir API profili, bir kaynak sağlayıcısı paketindeki her kaynak türünün en son ve en çok kararlı sürümünü almak için kullanabilirsiniz.

- Tüm hizmetler en son sürümlerini kullanmak için **son** profili bağımlılık olarak.

  - En son profili kullanmak için bir bağımlılıktır **com.microsoft.azure**.

  - Azure Stack ile uyumlu hizmetleri kullanmak için **com.microsoft.azure.profile\_2018\_03\_01\_karma** profili.

    - .NET ile olduğu gibi doğru sınıf açılır listeden seçerseniz Pom.xml dosyasında modüller otomatik olarak yükleyen bir bağımlılık olarak belirtilmesi budur.

    - Her modülün en üstüne aşağıdaki gibi görünür:      `Import com.microsoft.azure.management.resources.v2018_03_01.ResourceGroup`

  - Bağımlılıklar aşağıdaki gibi görünür:

     ```xml
     <dependency>
     <groupId>com.microsoft.azure.profile_2018_03_01_hybrid</groupId>
     <artifactId>azure</artifactId>
     <version>1.0.0-beta</version>
     </dependency>
     ```

  - Belirli bir kaynak sağlayıcısındaki bir kaynak türü için belirli API sürümlerini kullanmak için IntelliSense ile tanımlanan belirli API sürümlerini kullanın.

Tüm seçeneklerin aynı uygulamada birleştirebilirsiniz unutmayın.

## <a name="install-the-azure-java-sdk"></a>Azure Java'yı yükleme SDK'sı

Java SDK'sını yüklemek için aşağıdaki adımları kullanın:

1. Git'i yüklemek için resmi yönergeleri izleyin. Yönergeler için [Git'i yükledikten Başlarken -](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).

2. Yüklemek için yönergeleri izleyin [Java SDK'sı](https://zulu.org/download/) ve [Maven](https://maven.apache.org/). Java Developer Kit 8 sürümünü doğru sürümüdür. Doğru Apache Maven sürümüdür 3.0 veya üstü. JAVA_HOME ortam değişkenini Java Development Kit yükleme konumuna hızlı başlangıcı tamamlamak için ayarlamanız gerekir. Daha fazla bilgi için [Java ve Maven ile ilk işlevinizi oluşturma](../../azure-functions/functions-create-first-java-maven.md).

3. Doğru bağımlılık paketlerini yüklemek için Java uygulamanızı Pom.xml dosyasını açın. Bir bağımlılık, aşağıdaki kodda gösterildiği gibi ekleyin:

   ```xml  
   <dependency>
   <groupId>com.microsoft.azure.profile_2018_03_01_hybrid</groupId>
   <artifactId>azure</artifactId>
   <version>1.0.0-beta</version>
   </dependency>
   ```

4. Yüklenmesi gereken paketleri kümesini kullanmak istediğiniz profili sürümüne bağlıdır. Paket adları profil sürümleri şunlardır:

   - **com.microsoft.azure.profile\_2018\_03\_01\_hybrid**
   - **com.microsoft.azure**
     - **en son**

5. Yoksa, bir abonelik oluşturur ve daha sonra kullanmak için abonelik Kimliğini kaydedin. Abonelik oluşturma hakkında yönergeler için bkz: [Azure Stack'te teklifleri abonelikleri oluşturma](../azure-stack-subscribe-plan-provision-vm.md).

6. Hizmet sorumlusu oluşturma ve istemci Kimliğini ve istemci gizli anahtarını kaydedin. Azure Stack için hizmet sorumlusu oluşturma hakkında yönergeler için bkz: [uygulamalar erişim sağlamak için Azure Stack](../azure-stack-create-service-principals.md). İstemci kimliği olarak da bilinen uygulama kimliği bir hizmet sorumlusu oluştururken olduğuna dikkat edin.

7. Hizmet sorumlunuzu aboneliğinizde katkıda bulunan/sahip rolü olduğundan emin olun. Hizmet sorumlusuna bir rol atamak yönergeler için bkz: [uygulamalar erişim sağlamak için Azure Stack](../azure-stack-create-service-principals.md).

## <a name="prerequisites"></a>Önkoşullar

Azure Java SDK'sı, Azure Stack ile kullanmak için aşağıdaki değerleri girin ve ardından ortam değişkenleriyle değerleri ayarlayın. Ortam değişkenlerini ayarlamak için tablonun işletim sisteminiz için aşağıdaki yönergelere bakın.

| Değer                     | Ortam değişkenleri | Açıklama                                                                                                                                                                                                          |
| ------------------------- | --------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Kiracı Kimliği                 | AZURE_TENANT_ID            | Azure Stack değerini [Kiracı kimliği](../azure-stack-identity-overview.md).                                                          |
| İstemci Kimliği                 | AZURE_CLIENT_ID             | Hizmet sorumlusu uygulama kimliği önceki bölümde hizmet sorumlusu oluşturulurken kaydedilen.                                                                                              |
| Abonelik Kimliği           | AZURE_SUBSCRIPTION_ID      | [Abonelik kimliği](../azure-stack-plan-offer-quota-overview.md#subscriptions) nasıl, teklifler eriştiği Azure Stack'te.                |
| İstemci Gizli Anahtarı             | AZURE_CLIENT_SECRET        | Hizmet sorumlusu oluşturulurken kaydedilen hizmet sorumlusu uygulama gizli anahtarı.                                                                                                                                   |
| Resource Manager uç noktası | ARM_ENDPOINT              | Bkz: [Azure Stack Kaynak Yöneticisi uç noktası](../user/azure-stack-version-profiles-ruby.md#the-azure-stack-resource-manager-endpoint). |
| Konum                  | RESOURCE_LOCATION    | **Yerel** Azure Stack için.                                                                                                                                                                                                |

Azure Stack için Kiracı Kimliğini bulmak için yönergelere bakın [burada](../azure-stack-csp-ref-operations.md). Ortam değişkenlerini ayarlamak için aşağıdakileri yapın:

### <a name="microsoft-windows"></a>Microsoft Windows

Bir Windows Komut İstemi'nde ortam değişkenlerini ayarlamak için aşağıdaki biçimi kullanın:

```shell
Set AZURE_TENANT_ID=<Your_Tenant_ID>
```

### <a name="macos-linux-and-unix-based-systems"></a>macOS, Linux ve UNIX tabanlı sistemlerde

UNIX tabanlı sistemlerde, aşağıdaki komutu kullanın:

```shell
Export AZURE_TENANT_ID=<Your_Tenant_ID>
```

### <a name="trust-the-azure-stack-ca-root-certificate"></a>Azure Stack CA kök sertifikasını güven

ASDK kullanıyorsanız, CA kök sertifikasını, uzak makinede güvenmesi gerekir. Tümleşik sistemlerle bunu gerekmez.

#### <a name="windows"></a>Windows

1. Azure Stack otomatik olarak imzalanan sertifikayı masaüstünüze dışarı aktarın.

1. Bir komut isteminde dizini JAVA_HOME%\bin % olarak değiştirin.

1. Şu komutu çalıştırın:

   ```shell
   .\keytool.exe -importcert -noprompt -file <location of the exported certificate here> -alias root -keystore %JAVA_HOME%\lib\security\cacerts -trustcacerts -storepass changeit
   ```

### <a name="the-azure-stack-resource-manager-endpoint"></a>Azure Stack Kaynak Yöneticisi uç noktası

Microsoft Azure Resource Manager dağıtma, yönetme ve Azure kaynaklarını izleme olanağı tanıyan bir yönetim çerçevesidir. Azure Resource Manager, bir grup olarak yerine tek tek bir işlemde bu görevleri işleyebilir.

Resource Manager uç noktasından meta veri bilgilerini alabilirsiniz. Uç nokta, kodunuzu çalıştırmak için gereken bilgileri bir JSON dosyası döndürür.

Aşağıdaki konuları göz önünde bulundurun:

- **ResourceManagerUrl** Azure Stack geliştirme Seti'ni (ASDK) olan: https://management.local.azurestack.external/.

- **ResourceManagerUrl** tümleşik sistemlerindeki: `https://management.<location>.ext-<machine-name>.masd.stbtest.microsoft.com/`.

Gerekli meta verilerini almak için: `<ResourceManagerUrl>/metadata/endpoints?api-version=1.0`.

Örnek JSON dosyası:

```json
{
   "galleryEndpoint": "https://portal.local.azurestack.external:30015/",
   "graphEndpoint": "https://graph.windows.net/",
   "portal Endpoint": "https://portal.local.azurestack.external/",
   "authentication":
      {
      "loginEndpoint": "https://login.windows.net/",
      "audiences": ["https://management.<yourtenant>.onmicrosoft.com/3cc5febd-e4b7-4a85-a2ed-1d730e2f5928"]
      }
}
```

## <a name="existing-api-profiles"></a>Mevcut API profilleri

- **com.microsoft.Azure.Profile\_2018\_03\_01\_karma**: Azure Stack için yerleşik son profili. En Azure Stack ile uyumlu 1808 damgada olduğu sürece veya diğer hizmetler için bu profili kullanın.

- **com.microsoft.azure**: Tüm hizmetler en son sürümlerine oluşan profili. Tüm hizmetler en son sürümlerini kullanın.

Azure Stack ve API profilleri hakkında daha fazla bilgi için bkz. [API özeti profilleri](../user/azure-stack-version-profiles.md#summary-of-api-profiles).

## <a name="azure-java-sdk-api-profile-usage"></a>Azure Java SDK API profili kullanımı

Aşağıdaki kod, Azure Stack hizmet sorumlusu kimliğini doğrular. Kiracı kimliği ile Azure Stack'e özel kimlik doğrulaması temel bir belirteç oluşturur:

```java
AzureTokenCredentials credentials = new ApplicationTokenCredentials(client, tenant, key, AZURE_STACK)
                    .withDefaultSubscriptionID(subscriptionID);
Azure azureStack = Azure.configure()
                    .withLogLevel(com.microsoft.rest.LogLevel.BASIC)
                    .authenticate(credentials, credentials.defaultSubscriptionID());
```

Bu, uygulamanızı Azure Stack başarıyla dağıtmak için API profili bağımlılıkları kullanmanıza olanak sağlar.

## <a name="define-azure-stack-environment-setting-functions"></a>Azure Stack ortamı ayarı işlevleri tanımlayın

Azure Stack bulut doğru uç noktaları ile kaydetmek için aşağıdaki kodu kullanın:

```java
AzureEnvironment AZURE_STACK = new AzureEnvironment(new HashMap<String, String>() {
                {
                    put("managementEndpointUrl", settings.get("audience"));
                    put("resourceManagerEndpointUrl", armEndpoint);
                    put("galleryEndpointUrl", settings.get("galleryEndpoint"));
                    put("activeDirectoryEndpointUrl", settings.get("login_endpoint"));
                    put("activeDirectoryResourceID", settings.get("audience"));
                    put("activeDirectoryGraphResourceID", settings.get("graphEndpoint"));
                    put("storageEndpointSuffix", armEndpoint.substring(armEndpoint.indexOf('.')));
                    put("keyVaultDnsSuffix", ".vault" + armEndpoint.substring(armEndpoint.indexOf('.')));
                }
            });
```

`getActiveDirectorySettings` Çağrı aşağıdaki kodda, meta veri uç noktalardan gelen uç noktaları alır. Bu, yapılan çağrı ortam değişkenlerinden durumları:

```java
public static HashMap<String, String>
getActiveDirectorySettings(String armEndpoint) {

HashMap<String, String> adSettings = new HashMap<String, String>();

try {

// create HTTP Client
HttpClient httpClient = HttpClientBuilder.create().build();

// Create new getRequest with below mentioned URL
HttpGet getRequest = new
HttpGet(String.format("%s/metadata/endpoints?api-version=1.0",
armEndpoint));

// Add additional header to getRequest which accepts application/xml data
getRequest.addHeader("accept", "application/xml");

// Execute request and catch response
HttpResponse response = httpClient.execute(getRequest);
```

## <a name="samples-using-api-profiles"></a>API profillerini kullanma örnekleri

.NET ve Azure Stack API profilleriyle çözümleri oluşturmak için aşağıdaki GitHub örneklerine başvuru olarak kullanabilirsiniz:

- [Kaynak gruplarını yönetme](https://github.com/Azure-Samples/Hybrid-resources-java-manage-resource-group)

- [Depolama hesaplarını yönetme](https://github.com/Azure-Samples/hybrid-storage-java-manage-storage-accounts)

- [Bir sanal makineyi yönetin](https://github.com/Azure-Samples/hybrid-compute-java-manage-vm)

### <a name="sample-unit-test-project"></a>Örnek birim testi projesi

1. Aşağıdaki komutu kullanarak depoyu kopyalayın:

   `git clone https://github.com/Azure-Samples/Hybrid-resources-java-manage-resource-group.git`

2. Bir Azure hizmet sorumlusu oluşturma ve aboneliğe erişmek için bir rol atayın. Bir hizmet sorumlusu oluşturma hakkında yönergeler için bkz: [bir sertifika ile hizmet sorumlusu oluşturmak için Azure PowerShell kullanarak](../azure-stack-create-service-principals.md).

3. Aşağıdaki gerekli ortam değişken değerleri alın:

   - AZURE_TENANT_ID
   - AZURE_CLIENT_ID
   - AZURE_CLIENT_SECRET
   - AZURE_SUBSCRIPTION_ID
   - ARM_ENDPOINT
   - RESOURCE_LOCATION

4. Komut istemi kullanarak oluşturduğunuz asıl hizmetinden alınan bilgileri kullanarak, aşağıdaki ortam değişkenlerini ayarlayın:

   - dışarı aktarma AZURE_TENANT_ID {Kiracı Kimliğinizi} =
   - dışarı aktarma AZURE_CLIENT_ID {istemci Kimliğiniz} =
   - dışarı aktarma AZURE_CLIENT_SECRET {istemci gizli anahtarı} =
   - dışarı aktarma AZURE_SUBSCRIPTION_ID {abonelik Kimliğinizi} =
   - dışarı aktarma ARM_ENDPOINT {, Azure Stack Kaynak Yöneticisi URL'si} =
   - dışarı aktarma RESOURCE_LOCATION {location Azure Stack} =

   Windows içinde kullanmak **ayarlamak** yerine **dışarı**.

5. Kullanma `getactivedirectorysettings` kod arm meta veri uç noktası almak ve uç nokta bilgileri ayarlamak için HTTP İstemcisi'ni kullanın.

   ```java
   public static HashMap<String, String> getActiveDirectorySettings(String armEndpoint) {
   HashMap<String, String> adSettings = new HashMap<String,> String>();

   try {

   // create HTTP Client
   HttpClient httpClient = HttpClientBuilder.create().build();

   // Create new getRequest with below mentioned URL
   HttpGet getRequest = new
   HttpGet(String.format("%s/metadata/endpoints?api-version=1.0", armEndpoint));

   // Add additional header to getRequest which accepts application/xml data
   getRequest.addHeader("accept", "application/xml");

   // Execute request and catch response
   HttpResponse response = httpClient.execute(getRequest);
   ```

6. Pom.xml dosyasında kullanmak için aşağıdaki bağımlılığı ekleyin **2018-03-01-karma** Azure Stack için profili. Bu bağımlılık, bu profil ile ilişkili aşağıdaki işlem, ağ, depolama, KeyVault ve uygulama hizmetleri kaynak sağlayıcı için modülleri yükler:

   ```xml
   <dependency>
   <groupId>com.microsoft.azure.profile_2018_03_01_hybrid</groupId>
   <artifactId>azure</artifactId>
   <vers1s.0.0-beta</version>
   </dependency>
   ```

7. Ortam değişkenlerini ayarlamak için açık bir komut isteminde aşağıdaki komutu girin:

   ```shell
   mvn clean compile exec:java
   ```

## <a name="next-steps"></a>Sonraki adımlar

API profilleri hakkında daha fazla bilgi için bkz:

- Azure Stack](azure-stack-version-profiles.md) profillerinde sürümü
- [Profilleri tarafından desteklenen kaynak sağlayıcısı API sürümleri](azure-stack-profiles-azure-resource-manager-versions.md)
