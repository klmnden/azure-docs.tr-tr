---
title: "Azure kapsayıcı kayıt defteri ile kimlik doğrulaması"
description: "Azure Active Directory dahil olmak üzere bir Azure kapsayıcı kayıt defteri için kimlik doğrulama seçenekleri ilkeleri doğrudan ve kayıt defteri oturum açma hizmeti."
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: mmacy
tags: 
keywords: 
ms.assetid: 128a937a-766a-415b-b9fc-35a6c2f27417
ms.service: container-registry
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/05/2017
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 51fb72fc3c0e9b9e261f19883820f5d7399a57ab
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
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

İle oturum açtığınızda `az acr login`, CLI, çalıştırıldığında oluşturulan belirteç kullanan `az login` sorunsuz bir şekilde kaydınız oturumunuzun kimliğini doğrulamak için. Bu şekilde oturum açtığınız sonra kimlik bilgilerinizi önbelleğe alınmış ve sonraki `docker` komutları bir kullanıcı adı veya parola gerektirmez. Belirtecinizin süresi dolarsa, bunu kullanarak yenileyebilirsiniz `az acr login` yeniden kimlik doğrulamaya komutu.

## <a name="service-principal"></a>Hizmet sorumlusu

Atamak için bir [hizmet sorumlusu](../active-directory/develop/active-directory-application-objects.md) , kayıt defterine ve uygulama veya hizmet gözetimsiz kimlik doğrulaması için kullanabilirsiniz. Hizmet sorumluları izin [rol tabanlı erişim](../active-directory/role-based-access-control-configure.md) bir kayıt defteri ve birden çok hizmet asıl adı için bir kayıt defteri atayabilirsiniz. Birden çok hizmet asıl adı, farklı uygulamalar için farklı erişim tanımlamanıza olanak sağlar.

Kullanılabilir roller şunlardır:

  * **Okuyucu**: çekme
  * **Katkıda bulunan**: çekme ve itme
  * **Sahibi**: diğer kullanıcılara roller atama çekme ve itme

Hizmet sorumluları hem İtme hem de çekme senaryoları aşağıdaki gibi bir kayıt defteri gözetimsiz bağlantısını etkinleştir:

  * *Okuyucu*: Kubernetes, DC/OS ve Docker Swarm gibi orchestration sistemleri için bir kayıt defterinden kapsayıcı dağıtımlarını. Ayrıca kapsayıcı defterlerinden ilgili Azure hizmetlerine gibi çekebilir [AKS](../aks/index.yml), [uygulama hizmeti](../app-service/index.yml), [toplu](../batch/index.md), [Service Fabric](/azure/service-fabric/), ve Başkalarının.

  * *Katkıda bulunan*: Visual Studio Team Services (VSTS) veya kapsayıcı görüntülerini oluşturmak ve bunları bir kayıt defterine gönderme Jenkins sürekli tümleştirme ve dağıtım çözümleri gibi.

> [!TIP]
> Bir hizmet sorumlusu parola çalıştırarak yeniden [az ad sp sıfırlama-kimlik](/cli/azure/ad/sp?view=azure-cli-latest#az_ad_sp_reset_credentials) komutu.
>

Ayrıca doğrudan hizmet sorumlusu ile da kaydedebilirsiniz. Uygulama kimliği ve hizmet sorumlusu için parola sağlayın `docker login` komutu:

```
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

Bir uygulama kimliği unutmayın gerek kalmaması oturum açtıktan sonra Docker kimlik bilgileri önbelleğe alır

Yüklü olan Docker sürümüne bağlı olarak, kullanımını öneren bir güvenlik uyarısı görebilirsiniz `--password-stdin` parametresi. Kullanımı, bu makalenin kapsamı dışında olsa da, bu en iyi yöntem öneririz. Daha fazla bilgi için bkz: [docker oturum açma](https://docs.docker.com/engine/reference/commandline/login/) komut başvurusu.

## <a name="admin-account"></a>Yönetici hesabı

Her kapsayıcı kayıt defteri varsayılan olarak devre dışı bir yönetici kullanıcı hesabını içerir. Yönetici kullanıcı etkinleştirmek ve kendi kimlik bilgilerini yönetme [Azure portal](container-registry-get-started-portal.md#create-a-container-registry), veya Azure CLI kullanarak.

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

> [!IMPORTANT]
> Yönetici hesabı, test etmek için çoğunlukla kayıt defterine erişmek tek bir kullanıcı için tasarlanmıştır. Yönetici kimlik bilgileriniz ile birden çok kullanıcı paylaşımı önermiyoruz. Yönetici hesabıyla kimlik doğrulaması yapan tüm kullanıcıların kayıt defterine tek bir kullanıcı olarak görünür. Değiştirme veya bu hesap devre dışı bırakma kayıt defteri erişim kimlik bilgilerini kullanan tüm kullanıcılar için devre dışı bırakır.
>

## <a name="next-steps"></a>Sonraki adımlar

* [Azure CLI kullanarak ilk görüntünüzü bildirme](container-registry-get-started-azure-cli.md)

<!-- IMAGES -->
[auth-portal-01]: ./media/container-registry-authentication/auth-portal-01.png