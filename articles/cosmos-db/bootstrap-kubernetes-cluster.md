---
title: Azure Cosmos DB ile Azure Kubernetes kullanma
description: Azure Cosmos DB (Önizleme) kullanan azure'da bir Kubernetes kümesinin bootstrap öğrenin
author: SnehaGunda
ms.service: cosmos-db
ms.topic: sample
ms.date: 05/06/2019
ms.author: sngun
ms.openlocfilehash: 2c6af53aeec5d40f603d65595d93527107c0d80a
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66427707"
---
# <a name="how-to-use-azure-kubernetes-with-azure-cosmos-db-preview"></a>Azure Kubernetes, Azure Cosmos DB (Önizleme) ile kullanma

Azure Cosmos DB'de API etcd Azure Kubernetes için arka uç deposu olarak Azure Cosmos DB kullanmanıza olanak tanır. Azure Cosmos DB ana düğümünün API sunucularının yerel olarak yüklenmiş etcd eriştiğiniz gibi Azure Cosmos DB kullanmayı sağlar etcd kablo protokolünü uygular. Azure Cosmos DB'de etcd API'si şu anda Önizleme aşamasındadır. Kubernetes için yedekleme deposu olarak Azure Cosmos etcd API kullandığınızda, aşağıdaki avantajlardan yararlanabilirsiniz: 

* El ile yapılandırın ve etcd yönetmenize gerek yoktur.
* Cosmos (tek bölge içinde % 99,99 oranında, birden çok bölgede % 99,999) tarafından garanti etcd, yüksek kullanılabilirliği.
* Esnek ölçeklendirilebilirliğinin sunduğu etcd.
* Kurumsal kullanıma hazır & varsayılan tarafından güvenli hale getirin.
* Sektör lideri ve kapsamlı SLA'lar.

Azure Cosmos DB'de etcd API hakkında daha fazla bilgi için bkz: [genel bakış](etcd-api-introduction.md) makalesi. Bu makalede nasıl kullanılacağını gösterir [Azure Kubernetes altyapısını](https://github.com/Azure/aks-engine/blob/master/docs/tutorials/quickstart.md) (kullanan azure'da bir Kubernetes kümesinin önyüklemenizi aks-altyapısı) [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/) yerel olarak yüklenmiş ve yapılandırılmış bir etcd yerine. 

## <a name="prerequisites"></a>Önkoşullar

1. En son sürümünü yükleyin [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest). Azure CLI belirli işletim sisteminize indirin ve yükleyin.

1. Yükleme [en son sürümü](https://github.com/Azure/aks-engine/releases) Azure Kubernetes altyapısı. Farklı işletim sistemleri için yükleme yönergelerini kullanılabilir [Azure Kubernetes altyapısını](https://github.com/Azure/aks-engine/blob/master/docs/tutorials/quickstart.md#install-aks-engine) sayfası. Yer alan adımları yeterlidir **yükleme AKS motoru** bağlı doc bölümü. İndirdikten sonra zip dosyasını ayıklayın.

   Azure Kubernetes altyapısı (**aks altyapısı**) Azure üzerinde Kubernetes kümeleri için Azure Resource Manager şablonları oluşturur. Aks altyapısı giriş orchestrator, özellikleri ve aracılar da dahil olmak üzere, istenen küme açıklayan bir küme tanımı dosyasıdır. Azure Kubernetes Service için genel API giriş dosyaları yapısını benzer.

1. Azure Cosmos DB'de etcd API'si şu anda Önizleme aşamasındadır. Önizleme sürümünde kullanmak kaydolun: https://aka.ms/cosmosetcdapi-signup. Formu gönderdikten sonra aboneliğinizi, Azure Cosmos etcd API kullanmak için izin verilenler listesinde olacaktır. 

## <a name="deploy-the-cluster-with-azure-cosmos-db"></a>Azure Cosmos DB ile küme dağıtma

1. Bir komut istemi penceresi açın ve aşağıdaki komutu kullanarak Azure'da oturum açın:

   ```azurecli-interactive
   az login 
   ```

1. Birden fazla aboneliğiniz varsa Azure Cosmos DB etcd API için Güvenilenler listesinde aboneliğe geçin. Aşağıdaki komutu kullanarak gerekli aboneliğine geçiş yapabilirsiniz:

   ```azurecli-interactive
   az account set --subscription "<Name of your subscription>"
   ```
1. Ardından, Azure Kubernetes kümesi tarafından gerekli kaynaklar dağıtacağınız yeni bir kaynak grubu oluşturun. "Centralus" bölgesinde bir kaynak grubu oluşturduğunuzdan emin olun. Kaynak grubu "centralus" bölgede ancak olması zorunlu değildir, Azure Cosmos etcd API yalnızca "centralus" bölgesinde dağıtmak şu anda kullanılabilir. Bu nedenle Cosmos etcd örneğiyle birlikte bulunması için Kubernetes kümesi idealdir:

   ```azurecli-interactive
   az group create --name <Name> --location "centralus"
   ```

1. Ardından aynı kaynak grubunun parçası olan kaynaklarla iletişim kurabilmesi için bir hizmet sorumlusu Azure Kubernetes kümesi için oluşturun. Oluşturmak için CLI olur, bu örnekte Azure CLI, PowerShell veya Azure portalını kullanarak bir hizmet sorumlusu oluşturabilirsiniz.

   ```azurecli-interactive
   az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/<Your_Azure_subscription_ID>/resourceGroups/<Your_resource_group_name>"
   ```
   Bu komut, örneğin bir hizmet sorumlusu ayrıntıları verir:
   
   ```cmd
   Retrying role assignment creation: 1/36
   {
     "appId": "8415a4e9-4f83-46ca-a704-107457b2e3ab",
     "displayName": "azure-cli-2019-04-19-19-01-46",
     "name": "http://azure-cli-2019-04-19-19-01-46",
     "password": "102aecd3-5e37-4f3d-8738-2ac348c2e6a7",
     "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db47"
   }
   ```
   
   Not **AppID** ve **parola** Bu parametreler sonraki adımlarda kullanacağınız alanları. 

1. Komut İstemi'nden Azure Kubernetes altyapısı yürütülebilir dosyanın bulunduğu klasöre gidin. Örneğin, bir komut istemi klasöre gidebilirsiniz:

   ```cmd
   cd "\aks-engine-v0.36.3-windows-amd64\aks-engine-v0.36.3-windows-amd64"
   ```

1. Tercih ettiğiniz bir metin düzenleyicisinde açın ve Azure Cosmos DB etcd API'si ile Azure Kubernetes kümesi dağıtır bir Resource Manager şablonu tanımlayın. Metin düzenleyicinize aşağıdaki JSON tanımını kopyalayın ve dosyayı farklı Kaydet `apiModel.json`:

   ```json

   {
    "apiVersion": "vlabs",
    "properties": {
        "orchestratorProfile": {
            "orchestratorType": "Kubernetes",
            "kubernetesConfig": {
                "useManagedIdentity": false
            }
        },
        "masterProfile": {
            "count": 1,
            "dnsPrefix": "",
            "vmSize": "Standard_D2_v3",
            "cosmosEtcd": true
        },
        "agentPoolProfiles": [
            {
                "name": "agent",
                "count": 1,
                "vmSize": "Standard_D2_v3",
                "availabilityProfile": "AvailabilitySet"
            }
        ],
        "linuxProfile": {
            "adminUsername": "azureuser",
            "ssh": {
                "publicKeys": [
                    {
                        "keyData": ""
                    }
                ]
            }
        }
    }
   }
   ```

   JSON/kümeyi tanım dosyasında Not anahtar parametredir **"cosmosEtcd": true**. Bu parametre "masterProfile" özellikleri ve dağıtım normal etcd yerine Azure Cosmos etcd API'sini kullanmayı gösterir. 

1. Aşağıdaki komutla Azure Cosmos DB kullanan Azure Kubernetes kümesi dağıtın:

   ```cmd
   aks-engine deploy \
     --subscription-id <Your_Azure_subscription_ID> \
     --client-id <Service_ principal_appId> \
     --client-secret <Service_ principal_password> \
     --dns-prefix <Region_unique_dns_name > \
     --location centralus \
     --resource-group <Resource_Group_Name> \
     --api-model <Fully_qualified_path_to_the_template_file>  \
     --force-overwrite
   ```

   Azure Kubernetes altyapısı istediğiniz şekle, boyutu ve yapılandırmasını Azure Kubernetes kümesi tanımıma tüketir. Küme tanımı etkinleştirilebilir çeşitli özellikler mevcuttur. Bu örnekte şu parametreleri kullanır:

   * **Abonelik kimliği:** Azure Cosmos DB etcd API'si etkin olduğunda azure abonelik kimliği.
   * **istemci kimliği:** Hizmet sorumlusunun uygulama kimliği. `appId` 4. adımdaki çıktı olarak döndürüldü.
   * **Gizli:** Hizmet sorumlusunun parolasını veya rastgele oluşturulmuş bir parola. Bu değer, 4. Adım 'password' parametresinde çıktı olarak döndürüldü. 
   * **dnsPrefix:** Bölge benzersiz bir DNS adı. Bu değer, ana bilgisayar adı (örnek değerleri olan myprod1, hazırlama) bir parçası oluşturacak.
   * **Konum:**  Küme için şu anda yalnızca dağıtılan "centralus" olması gereken yerde konumu desteklenir.

   > [!Note]
   > Azure Cosmos etcd API'si şu anda yalnızca "centralus" bölgesinde dağıtmak kullanılabilir. 
 
   * **API modeli:** Şablon dosyasının tam yolu.
   * **zorla-üzerine yaz:** Bu seçenek otomatik olarak çıkış dizinindeki varolan dosyaların üzerine yazmak için kullanılır.
 
   Aşağıdaki komut, örnek bir dağıtım gösterir:

   ```cmd
   aks-engine deploy \
     --subscription-id 1234fc61-1234-1234-1234-be1234d12c1b \
     --client-id 1234a4e9-4f83-46ca-a704-107457b2e3ab \
     --client-secret 123aecd3-5e37-4f3d-8738-2ac348c2e6a7 \
     --dns-prefix aks-sg-test \
     --location centralus \
     --api-model "C:\Users\demouser\Downloads\apiModel.json"  \
     --force-overwrite
   ```

## <a name="verify-the-deployment"></a>Dağıtımı doğrulama

Şablon dağıtımı tamamlanması birkaç dakika sürer. Dağıtım başarıyla tamamlandıktan sonra komut isteminde aşağıdaki çıktıyı görürsünüz:

```cmd
WARN[0006] apimodel: missing masterProfile.dnsPrefix will use "aks-sg-test"
WARN[0006] --resource-group was not specified. Using the DNS prefix from the apimodel as the resource group name: aks-sg-test
INFO[0025] Starting ARM Deployment (aks-sg-test-546247491). This will take some time...
INFO[0587] Finished ARM Deployment (aks-sg-test-546247491). Succeeded
```

Gibi artık içeren kaynakları kaynak grubu - sanal makine, Azure Cosmos hesap (etcd API), sanal ağ, kullanılabilirlik kümesi ve diğer kaynakları Kubernetes kümesi tarafından gerekli. 

Azure Cosmos hesabın adı ile k8s eklenmiş belirtilen DNS adı ön eki eşleşir. Azure Cosmos hesabınızı otomatik olarak adlı bir veritabanı ile sağlanacak **EtcdDB** ve adlı bir kapsayıcı **EtcdData**. Kapsayıcı etcd depolayacak ilgili verileri. Kapsayıcı belirli sayıda istek birimleri ile sağlanan ve yapabilecekleriniz [Ölçek (artırma/azaltma) aktarım hızı](scaling-throughput.md) , iş yüküne göre. 

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi edinmek için nasıl [Azure Cosmos veritabanı, kapsayıcılar ve öğeleri ile çalışma](databases-containers-items.md)
* Bilgi edinmek için nasıl [sağlanan aktarım hızı maliyetlerini iyileştirme](optimize-cost-throughput.md)

