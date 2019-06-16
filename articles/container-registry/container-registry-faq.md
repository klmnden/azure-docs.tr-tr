---
title: Azure Container Registry - sık sorulan sorular
description: Azure Container Registry hizmeti ile ilgili sık sorulan soruların yanıtları
services: container-registry
author: sajayantony
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 5/13/2019
ms.author: sajaya
ms.openlocfilehash: 1400c023e43179a9c8490334e262711486c75a2d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66417929"
---
# <a name="frequently-asked-questions-about-azure-container-registry"></a>Azure Container Registry hakkında sık sorulan sorular

Bu makalede, sık sorulan sorular ve Azure Container Registry hakkında bilinen sorunları giderir.

## <a name="resource-management"></a>Kaynak yönetimi

- [Resource Manager şablonu kullanarak bir Azure container registry oluşturabilir miyim?](#can-i-create-an-azure-container-registry-using-a-resource-manager-template)
- [ACR görüntü tarama güvenlik açığı var mı?](#is-there-security-vulnerability-scanning-for-images-in-acr)
- [Kubernetes ile Azure Container Registry nasıl yapılandırabilirim?](#how-do-i-configure-kubernetes-with-azure-container-registry)
- [Bir kapsayıcı kayıt defteri için yönetici kimlik bilgileri nasıl alabilirim?](#how-do-i-get-admin-credentials-for-a-container-registry)
- [Yönetici kimlik bilgileri bir Resource Manager şablonunun nasıl alabilirim?](#how-do-i-get-admin-credentials-in-a-resource-manager-template)
- [Azure CLI veya Azure PowerShell kullanarak çoğaltma silindiğinde olsa da, Yasak durumu ile çoğaltma silme başarısız](#delete-of-replication-fails-with-forbidden-status-although-the-replication-gets-deleted-using-the-azure-cli-or-azure-powershell)

### <a name="can-i-create-an-azure-container-registry-using-a-resource-manager-template"></a>Resource Manager şablonu kullanarak bir Azure Container Registry oluşturabilir miyim?

Evet. İşte [şablon](https://github.com/Azure/azure-cli/blob/master/src/command_modules/azure-cli-acr/azure/cli/command_modules/acr/template.json) bir kayıt defteri oluşturmak için kullanabilirsiniz.

### <a name="is-there-security-vulnerability-scanning-for-images-in-acr"></a>ACR görüntü tarama güvenlik açığı var mı?

Evet. Belgelerden bkz [Twistlock](https://www.twistlock.com/2016/11/07/twistlock-supports-azure-container-registry/) ve [Açık Deniz Mavisi](http://blog.aquasec.com/image-vulnerability-scanning-in-azure-container-registry).

### <a name="how-do-i-configure-kubernetes-with-azure-container-registry"></a>Kubernetes ile Azure Container Registry nasıl yapılandırabilirim?

Belgelerine bakın [Kubernetes](http://kubernetes.io/docs/user-guide/images/#using-azure-container-registry-acr) ve için adımları [Azure Kubernetes hizmeti](container-registry-auth-aks.md).

### <a name="how-do-i-get-admin-credentials-for-a-container-registry"></a>Bir kapsayıcı kayıt defteri için yönetici kimlik bilgileri nasıl alabilirim?

> [!IMPORTANT]
> Yönetici kullanıcı hesabının, test etmek için çoğunlukla kayıt defterine erişmek tek bir kullanıcı için tasarlanmıştır. Yönetici hesabı kimlik bilgileri ile birden çok kullanıcı paylaşımı önermiyoruz. Bireysel kimlik, kullanıcı ve hizmet sorumluları gözetimsiz senaryoları için önerilir. Bkz: [kimlik doğrulamasına genel bakış](container-registry-authentication.md).

Yönetici kimlik bilgileri almadan önce kayıt defterinin yönetici kullanıcı etkin olduğundan emin olun.

Azure CLI kullanarak kimlik bilgilerini almak için:

```azurecli
az acr credential show -n myRegistry
```

Azure Powershell kullanarak:

```powershell
Invoke-AzureRmResourceAction -Action listCredentials -ResourceType Microsoft.ContainerRegistry/registries -ResourceGroupName myResourceGroup -ResourceName myRegistry
```

### <a name="how-do-i-get-admin-credentials-in-a-resource-manager-template"></a>Yönetici kimlik bilgileri bir Resource Manager şablonunun nasıl alabilirim?

> [!IMPORTANT]
> Yönetici kullanıcı hesabının, test etmek için çoğunlukla kayıt defterine erişmek tek bir kullanıcı için tasarlanmıştır. Yönetici hesabı kimlik bilgileri ile birden çok kullanıcı paylaşımı önermiyoruz. Bireysel kimlik, kullanıcı ve hizmet sorumluları gözetimsiz senaryoları için önerilir. Bkz: [kimlik doğrulamasına genel bakış](container-registry-authentication.md).

Yönetici kimlik bilgileri almadan önce kayıt defterinin yönetici kullanıcı etkin olduğundan emin olun.

İlk parolayı almak için:

```json
{
    "password": "[listCredentials(resourceId('Microsoft.ContainerRegistry/registries', 'myRegistry'), '2017-10-01').passwords[0].value]"
}
```

İkinci parolasını almak için:

```json
{
    "password": "[listCredentials(resourceId('Microsoft.ContainerRegistry/registries', 'myRegistry'), '2017-10-01').passwords[1].value]"
}
```

### <a name="delete-of-replication-fails-with-forbidden-status-although-the-replication-gets-deleted-using-the-azure-cli-or-azure-powershell"></a>Azure CLI veya Azure PowerShell kullanarak çoğaltma silindiğinde olsa da, Yasak durumu ile çoğaltma silme başarısız

Kullanıcı bir kayıt defterini izinlere sahip ancak abonelikte okuyucu düzeyinde izinlere sahip değil, hata görülür. Bu sorunu çözmek için abonelik okuyucusu izinleriyle kullanıcıya atayın:


```azurecli  
az role assignment create --role "Reader" --assignee user@contoso.com --scope /subscriptions/<subscription_id> 
```

## <a name="registry-operations"></a>Kayıt defteri işlemleri

- [Docker kayıt defteri HTTP API V2 nasıl erişim sağlanır?](#how-do-i-access-docker-registry-http-api-v2)
- [Bir depodaki herhangi bir etiket tarafından başvurulmuyor tüm bildirimleri nasıl silebilirim?](#how-do-i-delete-all-manifests-that-are-not-referenced-by-any-tag-in-a-repository)
- [Neden kayıt defteri kota kullanımını görüntüleri sildikten sonra azaltmaz?](#why-does-the-registry-quota-usage-not-reduce-after-deleting-images)
- [Depolama kota değişiklikleri nasıl doğrulamak?](#how-do-i-validate-storage-quota-changes)
- [My kayıt defteriyle nasıl CLI bir kapsayıcıda çalıştırırken kimliğini?](#how-do-i-authenticate-with-my-registry-when-running-the-cli-in-a-container)
- [Azure Container Registry TLS 1.2 sürümü yalnızca yapılandırması ve TLS 1.2 etkinleştirmek nasıl sunar?](#does-azure-container-registry-offer-tls-v12-only-configuration-and-how-to-enable-tls-v12)
- [Azure Container Registry, içerik güven destekliyor mu?](#does-azure-container-registry-support-content-trust)
- [Nasıl kayıt defteri kaynak yönetme izni olmadan çekme veya anında iletme görüntülerine erişim verilsin mi?](#how-do-i-grant-access-to-pull-or-push-images-without-permission-to-manage-the-registry-resource)
- [Otomatik görüntü karantina için bir kayıt defteri hizmetini nasıl etkinleştirebilirim](#how-do-i-enable-automatic-image-quarantine-for-a-registry)

### <a name="how-do-i-access-docker-registry-http-api-v2"></a>Docker kayıt defteri HTTP API V2 nasıl erişim sağlanır?

ACR Docker kayıt defteri HTTP API V2 destekler. API'leri adresinden erişilebilen `https://<your registry login server>/v2/`. Örnek: `https://mycontainerregistry.azurecr.io/v2/`

### <a name="how-do-i-delete-all-manifests-that-are-not-referenced-by-any-tag-in-a-repository"></a>Bir depodaki herhangi bir etiket tarafından başvurulmuyor tüm bildirimleri nasıl silebilirim?

Üzerinde bash kullanıyorsanız:

```bash
az acr repository show-manifests -n myRegistry --repository myRepository --query "[?tags[0]==null].digest" -o tsv  | xargs -I% az acr repository delete -n myRegistry -t myRepository@%
```

PowerShell için:

```powershell
az acr repository show-manifests -n myRegistry --repository myRepository --query "[?tags[0]==null].digest" -o tsv | %{ az acr repository delete -n myRegistry -t myRepository@$_ }
```

Not: Ekleyebileceğiniz `-y` onayı atlamak için delete komutu.

Daha fazla bilgi için [Sil kapsayıcı görüntülerini Azure Container Registry'de](container-registry-delete.md).

### <a name="why-does-the-registry-quota-usage-not-reduce-after-deleting-images"></a>Neden kayıt defteri kota kullanımını görüntüleri sildikten sonra azaltmaz?

Katmanlarda hala diğer kapsayıcı görüntülerini tarafından başvurulmayan bu durum oluşabilir. Başvuru içeren bir görüntüyü silmeniz halinde, kayıt defteri kullanım birkaç dakika içinde güncelleştirir.

### <a name="how-do-i-validate-storage-quota-changes"></a>Depolama kota değişiklikleri nasıl doğrulamak?

Aşağıdaki docker dosyası kullanarak bir 1 GB katman ile bir görüntü oluşturun. Bu, görüntünün kayıt defterinde herhangi bir görüntü tarafından paylaşılmayan bir katman sahip olmasını sağlar.

```dockerfile
FROM alpine
RUN dd if=/dev/urandom of=1GB.bin  bs=32M  count=32
RUN ls -lh 1GB.bin
```

Oluşturun ve görüntüyü docker CLI kullanarak kayıt defterine gönderin.

```bash
docker build -t myregistry.azurecr.io/1gb:latest .
docker push myregistry.azurecr.io/1gb:latest
```

Azure portalında depolama kullanım arttı veya CLI kullanarak kullanım sorgulayabilirsiniz görüyor olması gerekir.

```bash
az acr show-usage -n myregistry
```

Azure CLI veya portalı kullanarak görüntüyü silin ve birkaç dakika sonra güncelleştirilmiş'ın kullanımını denetleyin.

```bash
az acr repository delete -n myregistry --image 1gb
```

### <a name="how-do-i-authenticate-with-my-registry-when-running-the-cli-in-a-container"></a>My kayıt defteriyle nasıl CLI bir kapsayıcıda çalıştırırken kimliğini?

Azure CLI kapsayıcı Docker yuva bağlayarak çalıştırmanız gerekir:

```bash
docker run -it -v /var/run/docker.sock:/var/run/docker.sock azuresdk/azure-cli-python:dev
```

Kapsayıcıda yükleme `docker`:

```bash
apk --update add docker
```

Ardından, kayıt defteri ile kimlik doğrulaması:

```azurecli
az acr login -n MyRegistry
```

### <a name="does-azure-container-registry-offer-tls-v12-only-configuration-and-how-to-enable-tls-v12"></a>Azure Container Registry TLS 1.2 sürümü yalnızca yapılandırması ve TLS 1.2 etkinleştirmek nasıl sunar?

Evet. TLS herhangi yeni bir docker İstemcisi'ni kullanarak etkinleştirin (18.03.0 sürümü ve üzeri). 

### <a name="does-azure-container-registry-support-content-trust"></a>Azure Container Registry, içerik güven destekliyor mu?

Evet, Azure Container Registry'de güvenilen görüntüleri beri kullanabilirsiniz [Docker Notary](https://docs.docker.com/notary/getting_started/) tümleştirilmiştir ve etkin hale getirilebilir. Ayrıntılar için bkz [Azure Container Registry içerik güven](container-registry-content-trust.md).


####  <a name="where-is-the-file-for-the-thumbprint-located"></a>Parmak izi bulunan dosyayı nerede?

Altında `~/.docker/trust/tuf/myregistry.azurecr.io/myrepository/metadata`:

* Ortak anahtarlar ve sertifikalar (temsilcisi rolleri) hariç tüm rollerin depolanır `root.json`.
* Ortak anahtarlar ve sertifikalar temsilcisi rolünün üst rolüne JSON dosyasında depolanır (örneğin `targets.json` için `targets/releases` rol).

Bu ortak anahtar ve sertifikalar, Docker ve Notary istemci tarafından yapılan genel TUF doğrulama sonra doğrulamak için önerilir.

### <a name="how-do-i-grant-access-to-pull-or-push-images-without-permission-to-manage-the-registry-resource"></a>Nasıl kayıt defteri kaynak yönetme izni olmadan çekme veya anında iletme görüntülerine erişim verilsin mi?

ACR destekler [özel roller](container-registry-roles.md) farklı izin düzeyleri sağlar. Özellikle, `AcrPull` ve `AcrPush` rollerinin izin verdiği Azure kayıt defteri kaynak yönetme izni olmadan herhangi bir çekme ve/veya anında iletme görüntülere kullanıcılar.

* Azure portalı: Kayıt defterinizin erişim denetimi (IAM) -> Ekle -> (seçin `AcrPull` veya `AcrPush` rolü için).
* Azure CLI: Aşağıdaki komutu çalıştırarak kayıt defteri kaynak Kimliğini bulun:

  ```azurecli
  az acr show -n myRegistry
  ```
  
  Ardından, atayabilirsiniz `AcrPull` veya `AcrPush` bir kullanıcı rolüne (aşağıdaki örnekte `AcrPull`):

  ```azurecli
    az role assignment create --scope resource_id --role AcrPull --assignee user@example.com
    ```

  Veya uygulama Kimliğine göre tanımlanan bir hizmet ilkesi rol atayın:

  ```
  az role assignment create --scope resource_id --role AcrPull --assignee 00000000-0000-0000-0000-000000000000
  ```

Atanan kimlik doğrulaması ve erişim kayıt defterindeki görüntüleri ise.

* Bir kayıt defterine kimliğini doğrulamak için:
    
  ```azurecli
  az acr login -n myRegistry 
  ```

* Depoları listeleme için:

  ```azurecli
  az acr repository list -n myRegistry
  ```

 Görüntü çekmek için:
    
  ```azurecli
  docker pull myregistry.azurecr.io/hello-world
  ```

Yalnızca kullanarak `AcrPull` veya `AcrPush` rolü, atanan Azure kayıt defteri kaynak yönetme izni yok. Örneğin, `az acr list` veya `az acr show -n myRegistry` kayıt gösterilmez.

### <a name="how-do-i-enable-automatic-image-quarantine-for-a-registry"></a>Otomatik görüntü karantina için bir kayıt defteri hizmetini nasıl etkinleştirebilirim?

Görüntü karantina şu anda Önizleme ACR özelliğidir. Güvenlik taraması başarıyla geçirilen görüntüleri normal kullanıcılara görünür olacak şekilde bir kayıt defteri karantina modunu etkinleştirebilirsiniz. Ayrıntılar için bkz [ACR GitHub deposunu](https://github.com/Azure/acr/tree/master/docs/preview/quarantine).

## <a name="diagnostics"></a>Tanılama

- [docker isteği hatası ile başarısız oluyor: net/http: istek iptal bağlantı (Client.Timeout üstbilgileri beklerken aşıldı) beklenirken](#docker-pull-fails-with-error-nethttp-request-canceled-while-waiting-for-connection-clienttimeout-exceeded-while-awaiting-headers)
- [docker push başarılı olur, ancak docker isteği başarısız olursa, hata: Yetkisiz: kimlik doğrulaması gerekli](#docker-push-succeeds-but-docker-pull-fails-with-error-unauthorized-authentication-required)
- [Etkinleştirme ve docker Daemon programını, hata ayıklama günlükleri alın](#enable-and-get-the-debug-logs-of-the-docker-daemon) 
- [Yeni kullanıcı izinleri güncelleştirdikten sonra hemen etkili olmayabilir](#new-user-permissions-may-not-be-effective-immediately-after-updating)
- [Kimlik doğrulama bilgilerini doğrudan REST API çağrıları üzerinde doğru biçimde verilmez](#authentication-information-is-not-given-in-the-correct-format-on-direct-rest-api-calls)
- [Neden Azure portalında tüm depoları veya etiketleri listelemez?](#why-does-the-azure-portal-not-list-all-my-repositories-or-tags)
- [Windows üzerinde nasıl http izlemeleri toplamak?](#how-do-i-collect-http-traces-on-windows)

### <a name="docker-pull-fails-with-error-nethttp-request-canceled-while-waiting-for-connection-clienttimeout-exceeded-while-awaiting-headers"></a>docker isteği hatası ile başarısız oluyor: net/http: istek iptal bağlantı (Client.Timeout üstbilgileri beklerken aşıldı) beklenirken

 - Bu hata geçici bir sorun varsa, yeniden deneme başarılı olur.
 - Varsa `docker pull` docker daemon ile ilgili bir sorun olabilir sonra sürekli olarak başarısız olur. Sorun genellikle docker Daemon programını yeniden başlatarak azaltılabilir. 
 - Ardından docker Daemon programını yeniden başlattıktan sonra bu sorunla karşılaşmaya devam ederseniz sorun makinenin bazı ağ bağlantı sorunları olabilir. Genel ağdaki makinenin iyi durumda olup olmadığını denetlemek için bir komutu gibi deneyin `ping www.bing.com`.
 - Bu gibi durumlarda, bir yeniden deneme mekanizması her zaman tüm docker istemci işlemleri olmalıdır.

### <a name="docker-push-succeeds-but-docker-pull-fails-with-error-unauthorized-authentication-required"></a>docker push başarılı olur, ancak docker isteği başarısız olursa, hata: Yetkisiz: kimlik doğrulaması gerekli

Docker daemon, Red Hat sürümüyle bu hata oluşabilir burada `--signature-verification` varsayılan olarak etkindir. Aşağıdaki komutu çalıştırarak, Red Hat Enterprise Linux (RHEL) veya Fedora docker daemon seçeneklerini denetleyebilirsiniz:

```bash
grep OPTIONS /etc/sysconfig/docker
```

Örneğin, Fedora 28 sunucusu aşağıdaki docker daemon seçenekleri vardır:

```
OPTIONS='--selinux-enabled --log-driver=journald --live-restore'
```

İle `--signature-verification=false` eksik `docker pull` benzeri bir hata ile başarısız oluyor:

```bash
Trying to pull repository myregistry.azurecr.io/myimage ...
unauthorized: authentication required
```

Hatayı gidermek için:
1. Seçeneğini ekleyin `--signature-verification=false` docker daemon yapılandırma dosyasına `/etc/sysconfig/docker`. Örneğin:

  ```
  OPTIONS='--selinux-enabled --log-driver=journald --live-restore --signature-verification=false'
  ```
2. Docker daemon hizmeti, aşağıdaki komutu çalıştırarak yeniden başlatın:

  ```bash
  sudo systemctl restart docker.service
  ```

Ayrıntılarını `--signature-verification` çalıştırarak bulunabilir `man dockerd`.

### <a name="enable-and-get-the-debug-logs-of-the-docker-daemon"></a>Etkinleştirme ve docker Daemon programını, hata ayıklama günlükleri alın  

Başlangıç `dockerd` ile `debug` seçeneği. İlk olarak, docker daemon yapılandırma dosyası oluşturun (`/etc/docker/daemon.json`), varsa ekleyin ve değil `debug` seçeneği:

```json
{   
    "debug": true   
}
```

Ardından, arka plan programını yeniden başlatın. Örneğin, Ubuntu 14.04 ile:

```bash
sudo service docker restart
```

Ayrıntıları bulunabilir [Docker belgeleri](https://docs.docker.com/engine/admin/#enable-debugging). 

 * Günlükler, sisteminize bağlı olarak farklı konumlarda oluşturulabilir. Örneğin, Ubuntu 14.04 sahip `/var/log/upstart/docker.log`.   
Bkz: [Docker belgeleri](https://docs.docker.com/engine/admin/#read-the-logs) Ayrıntılar için.    

 * Docker Windows'ın için % LOCALAPPDATA%/docker/ altında günlükleri üretilir. Ancak tüm hata ayıklama bilgileri henüz içeremez.   

   Tam arka plan programı günlüğe erişmek için bazı ek adımlar gerekebilir:

    ```console
    docker run --privileged -it --rm -v /var/run/docker.sock:/var/run/docker.sock -v /usr/local/bin/docker:/usr/local/bin/docker alpine sh

    docker run --net=host --ipc=host --uts=host --pid=host -it --security-opt=seccomp=unconfined --privileged --rm -v /:/host alpine /bin/sh
    chroot /host
    ```
    VM çalışan tüm dosyalara erişimi şimdi `dockerd`. Günlük altındadır `/var/log/docker.log`.

### <a name="new-user-permissions-may-not-be-effective-immediately-after-updating"></a>Yeni kullanıcı izinleri güncelleştirdikten sonra hemen etkili olmayabilir

Yeni izin verdiğinizde izinleri (yeni rolleri) için hizmet sorumlusu, değişikliği hemen etkili. İki olası nedeni vardır:

* Azure Active Directory rol atama gecikmesi. Normalde hızlıdır, ancak yayma gecikme nedeniyle dakika sürebilir.
* ACR belirteci sunucusu üzerinde izni gecikmesi. Bu, 10 dakika kadar sürebilir. Azaltmak için `docker logout` ve aynı kullanıcı 1 dakika sonra tekrar kimliğini:

  ```bash
  docker logout myregistry.azurecr.io
  docker login myregistry.azurecr.io
  ```

ACR, kullanıcılar tarafından giriş çoğaltma silme işlemi şu anda desteklemiyor. Geçici çözüm giriş çoğaltma eklemektir şablon oluşturur, ancak ekleyerek oluşturulduktan atla `"condition": false` aşağıda gösterildiği gibi:

```json
{
    "name": "[concat(parameters('acrName'), '/', parameters('location'))]",
    "condition": false,
    "type": "Microsoft.ContainerRegistry/registries/replications",
    "apiVersion": "2017-10-01",
    "location": "[parameters('location')]",
    "properties": {},
    "dependsOn": [
        "[concat('Microsoft.ContainerRegistry/registries/', parameters('acrName'))]"
     ]
},
```

### <a name="authentication-information-is-not-given-in-the-correct-format-on-direct-rest-api-calls"></a>Kimlik doğrulama bilgilerini doğrudan REST API çağrıları üzerinde doğru biçimde verilmez

Karşılaşabileceğiniz bir `InvalidAuthenticationInfo` hata, özellikle kullanımı `curl` aracı seçeneği ile `-L`, `--location` (yeniden yönlendirmeleri izlemek için).
Örneğin, blob kullanarak getirilirken `curl` ile `-L` seçeneği ve temel kimlik doğrulaması:

```bash
curl -L -H "Authorization: basic $credential" https://$registry.azurecr.io/v2/$repository/blobs/$digest
```

Aşağıdaki yanıta neden olabilir:

```xml
<?xml version="1.0" encoding="utf-8"?>
<Error><Code>InvalidAuthenticationInfo</Code><Message>Authentication information is not given in the correct format. Check the value of Authorization header.
RequestId:00000000-0000-0000-0000-000000000000
Time:2019-01-01T00:00:00.0000000Z</Message></Error>
```

Kök neden olan bazı `curl` uygulamaları özgün istekteki üstbilgi yeniden yönlendirmeleri izleyin.

Sorunu gidermek için üst bilgiler olmadan el ile yeniden yönlendirmeleri İzle gerekir. Yanıt üstbilgileri ile yazdırma `-D -` seçeneği `curl` ve ardından ayıklayın: `Location` üst bilgi:

```bash
redirect_url=$(curl -s -D - -H "Authorization: basic $credential" https://$registry.azurecr.io/v2/$repository/blobs/$digest | grep "^Location: " | cut -d " " -f2 | tr -d '\r')
curl $redirect_url
```

### <a name="why-does-the-azure-portal-not-list-all-my-repositories-or-tags"></a>Neden Azure portalında tüm depoları veya etiketleri listelemez? 

Microsoft Edge tarayıcısını kullanıyorsanız, en fazla 100 depoları veya listelenen etiketler görebilirsiniz. Kayıt defterinizin 100'den fazla depolar ve etiketleri varsa, Firefox veya Chrome'un tarayıcı tamamı Listelenemeyecek kadar kullanmanızı öneririz.

### <a name="how-do-i-collect-http-traces-on-windows"></a>Windows üzerinde nasıl http izlemeleri toplamak?

#### <a name="prerequisites"></a>Önkoşullar

- Fiddler https şifresini çözmeyi etkinleştirin:  <https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/DecryptHTTPS>
- Docker kullanıcı arabirimi aracılığıyla bir ara sunucu kullanmak Docker'ı etkinleştirin: <https://docs.docker.com/docker-for-windows/#proxies>
- İşlem tamamlandığında geri emin olun.  Docker etkinleştirildiğinde çalışmaz ve fiddler çalışmıyor.

#### <a name="windows-containers"></a>Windows kapsayıcıları

127\.0.0.1:8888 Docker proxy yapılandırma

#### <a name="linux-containers"></a>Linux kapsayıcıları

Docker'ın IP vm sanal anahtarını bulun:

```powershell
(Get-NetIPAddress -InterfaceAlias "*Docker*" -AddressFamily IPv4).IPAddress
```

(Örneğin 10.0.75.1:8888) 8888 bağlantı noktası ve önceki komut ve çıktı için Docker proxy ayarlarını yapılandırma

## <a name="tasks"></a>Görevler

- [Çalıştırmaları iptal nasıl toplu?](#how-do-i-batch-cancel-runs)
- [Az acr oluşturma komutu içinde nasıl .git klasörü dahil edilsin mi?](#how-do-i-include-the-git-folder-in-az-acr-build-command)

### <a name="how-do-i-batch-cancel-runs"></a>Çalıştırmaları iptal nasıl toplu?

Aşağıdaki komutları belirtilen kayıt defteri çalışan tüm görevler iptal edin.

```azurecli
az acr task list-runs -r $myregistry --run-status Running --query '[].runId' -o tsv \
| xargs -I% az acr task cancel-run -r $myregistry --run-id %
```

### <a name="how-do-i-include-the-git-folder-in-az-acr-build-command"></a>Az acr oluşturma komutu içinde nasıl .git klasörü dahil edilsin mi?

Yerel kaynak klasörüne geçirirseniz `az acr build` komutu `.git` klasörü varsayılan olarak karşıya yüklenen paketinden çıkarılır. Oluşturabileceğiniz bir `.dockerignore` aşağıdaki ayar dosyası. Altındaki tüm dosyaları geri yükleme komutunu belirten `.git` karşıya yüklenen paketteki. 

```
!.git/**
```

Bu ayar için de geçerlidir. `az acr run` komutu.

## <a name="cicd-integration"></a>CI/CD tümleştirmesi

- [CircleCI](https://github.com/Azure/acr/blob/master/docs/integration/CircleCI.md)
- [GitHub eylemleri](https://github.com/Azure/acr/blob/master/docs/integration/github-actions/github-actions.md)


## <a name="next-steps"></a>Sonraki adımlar

* [Daha fazla bilgi edinin](container-registry-intro.md) Azure Container Registry hakkında.
