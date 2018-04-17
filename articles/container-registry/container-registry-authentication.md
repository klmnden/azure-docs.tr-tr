---
title: Azure kapsayıcı kayıt defteri ile kimlik doğrulaması
description: Azure Active Directory dahil olmak üzere bir Azure kapsayıcı kayıt defteri için kimlik doğrulama seçenekleri ilkeleri doğrudan ve kayıt defteri oturum açma hizmeti.
services: container-registry
author: stevelas
manager: timlt
ms.service: container-registry
ms.topic: article
ms.date: 01/23/2018
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 349d4f8cba2967edcedb202979695d271283fa8b
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="authenticate-with-a-private-docker-container-registry"></a>Bir özel Docker kapsayıcısı kayıt defteri ile kimlik doğrulaması

Azure kapsayıcı kayıt defteri her biri bir veya daha fazla kayıt defteri kullanım senaryoları için geçerli bir kimlik doğrulaması yapmak için birkaç yolu vardır.

Bir kayıt defteri aracılığıyla doğrudan oturum [tek tek oturum açma](#individual-login-with-azure-ad), ve uygulamaları ve kapsayıcı orchestrators katılımsız ya da "gözetimsiz," kimlik doğrulaması Azure Active Directory (Azure AD) kullanarak gerçekleştirebilir [ Hizmet sorumlusu](#service-principal).

Azure kapsayıcı kayıt defteri veya anonim erişim kimliği doğrulanmamış Docker işlemlerini desteklemiyor. Ortak görüntüler için kullandığınız [Docker hub'a](https://docs.docker.com/docker-hub/).

## <a name="individual-login-with-azure-ad"></a>Azure AD ile tek tek oturum açma

Görüntüleri çekme ve görüntüleri geliştirme istasyonunuzdan gönderilmesi gibi kayıt defteri ile doğrudan çalışırken kullanarak kimlik doğrulaması [az acr oturum açma](/cli/azure/acr?view=azure-cli-latest#az_acr_login) komutunu [Azure CLI](/cli/azure/install-azure-cli):

```azurecli
az acr login --name <acrName>
```

İle oturum açtığınızda `az acr login`, CLI, çalıştırıldığında oluşturulan belirteç kullanan `az login` sorunsuz bir şekilde kaydınız oturumunuzun kimliğini doğrulamak için. Bu şekilde oturum açtığınız sonra kimlik bilgilerinizi önbelleğe alınmış ve sonraki `docker` komutları bir kullanıcı adı veya parola gerektirmez. Belirtecinizin süresi dolarsa, bunu kullanarak yenileyebilirsiniz `az acr login` yeniden kimlik doğrulamaya komutu. Kullanarak `az acr login` Azure kimliklerle sağlar [rol tabanlı erişim](../role-based-access-control/role-assignments-portal.md).

## <a name="service-principal"></a>Hizmet sorumlusu

Atamak için bir [hizmet sorumlusu](../active-directory/develop/active-directory-application-objects.md) , kayıt defterine ve uygulama veya hizmet gözetimsiz kimlik doğrulaması için kullanabilirsiniz. Hizmet sorumluları izin [rol tabanlı erişim](../role-based-access-control/role-assignments-portal.md) bir kayıt defteri ve birden çok hizmet asıl adı için bir kayıt defteri atayabilirsiniz. Birden çok hizmet asıl adı, farklı uygulamalar için farklı erişim tanımlamanıza olanak sağlar.

Kullanılabilir roller şunlardır:

  * **Okuyucu**: çekme
  * **Katkıda bulunan**: çekme ve itme
  * **Sahibi**: diğer kullanıcılara roller atama çekme ve itme

Hizmet sorumluları hem İtme hem de çekme senaryoları aşağıdaki gibi bir kayıt defteri gözetimsiz bağlantısını etkinleştir:

  * *Okuyucu*: Kubernetes, DC/OS ve Docker Swarm gibi orchestration sistemleri için bir kayıt defterinden kapsayıcı dağıtımlarını. Ayrıca kapsayıcı defterlerinden ilgili Azure hizmetlerine gibi çekebilir [AKS](../aks/index.yml), [uygulama hizmeti](../app-service/index.yml), [toplu](../batch/index.yml), [Service Fabric](/azure/service-fabric/), ve Başkalarının.

  * *Katkıda bulunan*: Visual Studio Team Services (VSTS) veya kapsayıcı görüntülerini oluşturmak ve bunları bir kayıt defterine gönderme Jenkins sürekli tümleştirme ve dağıtım çözümleri gibi.

> [!TIP]
> Bir hizmet sorumlusu parola çalıştırarak yeniden [az ad sp sıfırlama-kimlik](/cli/azure/ad/sp?view=azure-cli-latest#az_ad_sp_reset_credentials) komutu.
>

Ayrıca doğrudan hizmet sorumlusu ile da kaydedebilirsiniz. Uygulama kimliği ve hizmet sorumlusu için parola sağlayın `docker login` komutu:

```
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

Bir uygulama kimliği unutmayın gerek kalmaması oturum açtıktan sonra Docker kimlik bilgileri önbelleğe alır

Yüklü olan Docker sürümüne bağlı olarak, kullanımını öneren bir güvenlik uyarısı görebilirsiniz `--password-stdin` parametresi. Bunun kullanımı bu makalenin kapsamında olmasa da bu en iyi yöntemin izlenmesi önerilir. Daha fazla bilgi için bkz: [docker oturum açma](https://docs.docker.com/engine/reference/commandline/login/) komut başvurusu.

ACR gözetimsiz kimlik doğrulaması için bir hizmet sorumlusu kullanma hakkında daha fazla bilgi için bkz: [hizmet asıl adı ile Azure kapsayıcı kayıt defteri kimlik doğrulaması](container-registry-auth-service-principal.md).

## <a name="admin-account"></a>Yönetici hesabı

Her kapsayıcı kayıt defteri varsayılan olarak devre dışı bir yönetici kullanıcı hesabını içerir. Yönetici kullanıcı etkinleştirmek ve kendi kimlik bilgilerini yönetme [Azure portal](container-registry-get-started-portal.md#create-a-container-registry), veya Azure CLI kullanarak.

> [!IMPORTANT]
> Yönetici hesabı, test etmek için çoğunlukla kayıt defterine erişmek tek bir kullanıcı için tasarlanmıştır. Yönetici kimlik bilgileriniz ile birden çok kullanıcı paylaşımı önermiyoruz. Yönetici hesabıyla kimlik doğrulaması yapan tüm kullanıcıların, kayıt defteri anında iletme ve çekme erişimi olan tek bir kullanıcı olarak görünür. Değiştirme veya bu hesap devre dışı bırakma kayıt defteri erişim kimlik bilgilerini kullanan tüm kullanıcılar için devre dışı bırakır. Tek tek kimlik, kullanıcı ve hizmet asıl adı gözetimsiz senaryolar için önerilir.
>

İki parola ile her ikisi de üretilebilir sağlanan yönetici hesabıdır. İki parola diğer yeniden oluşturmak, bir parola kullanarak kayıt defterine bağlanma korumanıza olanak sağlar. Yönetici hesabı etkinleştirilirse, kullanıcı adı ve ya da parola geçirebilirsiniz `docker login` temel kimlik doğrulaması yapmak için kayıt defteri komutu. Örneğin:

```
docker login myregistry.azurecr.io -u myAdminName -p myPassword1
```

Yeniden, Docker, kullanmanızı önerir `--password-stdin` güvenliği artırmak için komut satırında sağladığını yerine parametre. Yalnızca kullanıcı adınızı, olmadan belirtebilirsiniz `-p`ve istendiğinde parolanızı girin.

Yönetici kullanıcı için varolan bir kayıt etkinleştirmek için kullanabileceğiniz `--admin-enabled` parametresinin [az acr güncelleştirme](/cli/azure/acr?view=azure-cli-latest#az_acr_update) Azure CLI komutunu:

```azurecli
az acr update -n <acrName> --admin-enabled true
```

Azure portalında yönetici kullanıcı, kayıt defteri giderek etkinleştirebilirsiniz seçme **erişim anahtarları** altında **ayarları**, ardından **etkinleştirmek** altında **yönetici Kullanıcı**.

![Azure portalında yönetici kullanıcı UI etkinleştir][auth-portal-01]

## <a name="next-steps"></a>Sonraki adımlar

* [Azure CLI kullanarak ilk görüntünüzü bildirme](container-registry-get-started-azure-cli.md)

<!-- IMAGES -->
[auth-portal-01]: ./media/container-registry-authentication/auth-portal-01.png
