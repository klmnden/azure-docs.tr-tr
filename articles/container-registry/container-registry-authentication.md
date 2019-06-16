---
title: Azure container registry ile kimlik doğrulaması
description: Bir Azure Active Directory kimlik bilgilerinizle oturum, hizmet sorumlularını kullanma ve isteğe bağlı bir yönetici kimlik bilgileri de dahil olmak üzere Azure container registry, kimlik doğrulama seçenekleri.
services: container-registry
author: dlepow
manager: jeconnoc
ms.service: container-registry
ms.topic: article
ms.date: 12/21/2018
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9682b9b832a8fd7374cb84e8cc6faad69df15945
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61333875"
---
# <a name="authenticate-with-a-private-docker-container-registry"></a>Özel Docker kapsayıcı kayıt defteri ile kimlik doğrulaması

Her biri bir veya daha fazla kayıt defteri kullanım senaryoları için uygun bir Azure kapsayıcı kayıt defteri ile kimlik doğrulaması yapmak için birkaç yol vardır.

Bir kayıt defterine doğrudan aracılığıyla oturum [tek oturum açma](#individual-login-with-azure-ad), ya da uygulamaları ve kapsayıcı düzenleyicileri katılımsız veya "gözetimsiz" kimlik doğrulaması Azure Active Directory (Azure AD) kullanarak gerçekleştirebileceğiniz [ Hizmet sorumlusu](#service-principal).

## <a name="individual-login-with-azure-ad"></a>Azure AD ile tek tek oturum açma

Görüntü çeken ve bir geliştirme iş istasyonundan, görüntüleri gönderme gibi Defterinizle doğrudan çalışırken kullanarak kimlik doğrulaması [az acr oturum açma](/cli/azure/acr?view=azure-cli-latest#az-acr-login) komutunu [Azure CLI](/cli/azure/install-azure-cli):

```azurecli
az acr login --name <acrName>
```

İle oturum açtığınızda `az acr login`, CLI, yürütme sırasında oluşturulan belirteci kullanan [az login](/cli/azure/reference-index#az-login) sorunsuz bir şekilde oturumunuz, kayıt defteri ile kimlik doğrulaması için. Bu şekilde oturum açtığınız sonra kimlik bilgileriniz önbelleğe alınmış ve sonraki `docker` komutları oturumunuzda bir kullanıcı adı veya parola gerektirmez. 

Kayıt defteri erişimi için belirteci tarafından kullanılan `az acr login` bu nedenle her zaman çalıştırmadan önce kayıt defterinde oturum öneririz 1 saat için geçerli olan bir `docker` komutu. Belirtecinizin süresi dolarsa, onu kullanarak yenileyebilirsiniz `az acr login` komutu yeniden yeniden kimlik doğrulamaya zorlayabilir. 

Kullanarak `az acr login` Azure kimliklerle sağlar [rol tabanlı erişim](../role-based-access-control/role-assignments-portal.md). Bazı senaryolar için Azure AD'de bir kayıt defterine kendi bireysel kimlik ile oturum açmak isteyebilirsiniz. Çapraz servis senaryoları için ya da burada bireysel erişim yönetmek istemediğiniz bir çalışma grubunun gereksinimlerini ele almak için de içinde oturum açabileceğiniz bir [yönetilen Azure kaynakları için kimliği](container-registry-authentication-managed-identity.md).

## <a name="service-principal"></a>Hizmet sorumlusu

Atarsanız bir [hizmet sorumlusu](../active-directory/develop/app-objects-and-service-principals.md) defterinize, uygulamanızın veya hizmetinizin, gözetimsiz kimlik doğrulaması için kullanabilirsiniz. Hizmet sorumluları izin [rol tabanlı erişim](../role-based-access-control/role-assignments-portal.md) bir kayıt defterine ve birden çok hizmet sorumluları bir kayıt defterine atayabilir. Birden çok hizmet sorumluları farklı uygulamalar için farklı erişim tanımlamanızı sağlar.

Kapsayıcı kayıt defteri için kullanılabilir roller şunlardır:

* **AcrPull**: çekme

* **AcrPush**: çekme ve itme

* **Sahibi**: çekme, gönderme ve diğer kullanıcılara roller atama

Rolleri tam bir listesi için bkz. [Azure Container Registry rolleri ve izinleri](container-registry-roles.md).

CLI betiklerini bir hizmet sorumlusu uygulama kimliği ve parola kimlik doğrulaması ile Azure container registry oluşturun veya mevcut bir hizmet sorumlusu adını kullanmak için bkz: [hizmet sorumluları ile Azure Container Registry kimlik doğrulaması](container-registry-auth-service-principal.md).

Hizmet sorumluları, hem çekme ve itme senaryoları aşağıdaki gibi bir kayıt defteri gözetimsiz bağlantıyı etkinleştirmek:

  * *Çekme*: Kubernetes, DC/OS ve Docker Swarm dahil düzenleme sistemleri için bir kayıt defterinden kapsayıcıları dağıtın. Aynı zamanda kapsayıcı kayıt defterleri için ilgili Azure Hizmetleri gibi çekebilirsiniz [Azure Kubernetes hizmeti](container-registry-auth-aks.md), [Azure Container Instances](container-registry-auth-aci.md), [App Service](../app-service/index.yml), [Batch](../batch/index.yml), [Service Fabric](/azure/service-fabric/)ve diğerleri.

  * *Anında iletme*: Kapsayıcı görüntüleri oluşturun ve bunları Azure işlem hatları veya Jenkins gibi sürekli tümleştirme ve dağıtım çözümlerini kullanarak bir kayıt defterine gönderin.

Siz de doğrudan hizmet sorumlusu ile oturum açabilir. Etkileşimli olarak aşağıdaki komutu çalıştırdığınızda, hizmet sorumlusu uygulama kimliği (kullanıcı adı) ve istendiğinde parolayı belirtin. Oturum açma kimlik bilgilerini yönetmek en iyi yöntemler için bkz: [docker oturum açma](https://docs.docker.com/engine/reference/commandline/login/) komut başvurusuna:

```
docker login myregistry.azurecr.io
```

Uygulama Kimliği hatırlamak zorunda kalmazsınız oturum açtıktan sonra Docker kimlik bilgilerini önbelleğe alır

> [!TIP]
> Çalıştırarak bir hizmet sorumlusu parolası yeniden oluşturabilirsiniz [az ad sp reset-credentials](/cli/azure/ad/sp?view=azure-cli-latest) komutu.
>

## <a name="admin-account"></a>Yönetici hesabı

Her kapsayıcı kayıt defteri varsayılan olarak devre dışı bir yönetici kullanıcı hesabı içerir. Yönetici kullanıcıyı etkinleştirin ve kimlik bilgilerini Azure portalında veya Azure CLI veya diğer Azure araçları kullanarak yönetin.

> [!IMPORTANT]
> Yönetici hesabı, test etmek için çoğunlukla kayıt defterine erişmek tek bir kullanıcı için tasarlanmıştır. Yönetici hesabı kimlik bilgileri ile birden çok kullanıcı paylaşımı önermiyoruz. Yönetici hesabı ile yetkilendirilmiş tüm kullanıcılar, kayıt defterine gönderme ve çekme erişimi olan tek bir kullanıcı olarak görünür. Değiştirme veya bu hesap devre dışı bırakma kayıt defteri erişim kimlik bilgilerini kullanan tüm kullanıcılar için devre dışı bırakır. Bireysel kimlik, kullanıcı ve hizmet sorumluları gözetimsiz senaryoları için önerilir.
>

İki parola ile ikisi için de üretilebilir sağlanan yönetici hesabıdır. İki parola diğer yeniden oluşturmak, bir parola kullanarak kayıt defteri bağlantı sürdürmenizi sağlar. Yönetici hesabı etkinleştirilirse, kullanıcı adı ve parola ya da geçirebilirsiniz `docker login` temel kimlik doğrulaması kayıt için istem görüntülendiğinde komutu. Örneğin:

```
docker login myregistry.azurecr.io 
```


Mevcut bir kayıt defteri için yönetici kullanıcıyı etkinleştirmek için kullanabileceğiniz `--admin-enabled` parametresinin [az acr update](/cli/azure/acr?view=azure-cli-latest#az-acr-update) Azure CLI komutunu:

```azurecli
az acr update -n <acrName> --admin-enabled true
```

Kayıt defterinizin giderek Azure portalında yönetici kullanıcıyı etkinleştirebilirsiniz seçerek **erişim anahtarları** altında **ayarları**, ardından **etkinleştirme** altında **yönetici Kullanıcı**.

![Azure portalında yönetici kullanıcı UI'ı etkinleştir][auth-portal-01]

## <a name="next-steps"></a>Sonraki adımlar

* [Azure CLI kullanarak ilk görüntünüzü itme](container-registry-get-started-azure-cli.md)

<!-- IMAGES -->
[auth-portal-01]: ./media/container-registry-authentication/auth-portal-01.png
