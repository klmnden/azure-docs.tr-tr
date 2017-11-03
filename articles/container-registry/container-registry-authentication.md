---
title: "Azure kapsayıcı kayıt defteri ile kimlik doğrulaması | Microsoft Docs"
description: "Bir Azure Active Directory hizmet asıl veya bir yönetici hesabı kullanarak bir Azure kapsayıcı kayıt defteri oturum açmak nasıl"
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: cristyg
tags: 
keywords: 
ms.assetid: 128a937a-766a-415b-b9fc-35a6c2f27417
ms.service: container-registry
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2017
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9d7d2ae0e9b1f7850332d151d78a4a5fdb013777
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="authenticate-with-a-private-docker-container-registry"></a>Bir özel Docker kapsayıcısı kayıt defteri ile kimlik doğrulaması
Azure kapsayıcı kayıt defteri kapsayıcı görüntülerinde çalışmak için kullanarak oturum `docker login` komutu. Ya da kullanarak oturum bir  **[Azure Active Directory hizmet asıl](../active-directory/active-directory-application-objects.md)**  veya kayıt defteri özgü **yönetici hesabı**. Bu makalede bu kimlikleri hakkında daha fazla ayrıntı sağlar.

## <a name="service-principal"></a>Hizmet sorumlusu

Bir hizmet sorumlusu kayıt defterine atayın ve temel Docker kimlik doğrulaması için kullanın. Bir hizmet sorumlusunu kullanarak çoğu senaryolar için önerilir. Uygulama kimliği ve hizmet sorumlusu için parola sağlayın `docker login` aşağıdaki örnekte gösterildiği gibi komut:

```
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

Bir uygulama kimliği unutmayın gerek kalmaması oturum açtıktan sonra Docker kimlik bilgileri önbelleğe alır

> [!TIP]
> İsterseniz, çalıştırarak bir hizmet sorumlusu parolasını yeniden oluşturabilmeniz `az ad sp reset-credentials` komutu.
>

Hizmet sorumluları izin [rol tabanlı erişim](../active-directory/role-based-access-control-configure.md) bir kayıt defteri. Kullanılabilir roller şunlardır:
  * Okuyucu (çekme yalnızca erişimi).
  * Katkıda bulunan (çekme ve itme).
  * (Çekme, gönderim ve ata rolleri diğer kullanıcılara) sahip.

Anonim erişim Azure kapsayıcı kayıt defterleri üzerinde kullanılabilir değil. Ortak görüntüler için kullandığınız [Docker hub'a](https://docs.docker.com/docker-hub/).

Farklı kullanıcılar veya uygulamalar için erişim tanımlamanıza olanak sağlayan bir kayıt defteri, birden çok hizmet asıl adı atayabilirsiniz. Hizmet asıl adı da geliştirici veya DevOps senaryolar aşağıdaki örneklerde olduğu gibi bir kayıt defteri "gözetimsiz" bağlantısını etkinleştir:

  * DC/OS, Docker Swarm ve Kubernetes gibi orchestration sistemlere kapsayıcı dağıtımlarını bir kayıt defteri. Kapsayıcı kayıt defterleri ilgili Azure hizmetlerine gibi çekebilir [kapsayıcı hizmeti](../container-service/index.yml), [uygulama hizmeti](../app-service/index.yml), [toplu](../batch/index.md), [Service Fabric](/azure/service-fabric/)ve diğerleri.

  * Kapsayıcı görüntülerini oluşturmak ve bunları bir kayıt defterine gönderme sürekli tümleştirme ve dağıtım çözümleri (örneğin, Visual Studio Team Services veya Jenkins).


## <a name="admin-account"></a>Yönetici hesabı
Oluşturduğunuz her kayıt defteri ile bir yönetici hesabı otomatik olarak oluşturulan. Hesap varsayılan olarak devre dışıdır, ancak etkinleştirmek ve kimlik bilgilerini, örneğin aracılığıyla yönetmek [portal](container-registry-get-started-portal.md#create-a-container-registry) veya kullanarak [Azure CLI 2.0 komutları](container-registry-get-started-azure-cli.md#create-a-container-registry). İki parola ile her ikisi de üretilebilir sağlanan her yönetici hesabıdır. İki parola diğer parola yeniden sırada bir parola kullanarak kayıt defteri bağlantılar sağlamanıza olanak sağlar. Hesap etkinleştirilirse, kullanıcı adı ve ya da parola geçirebilirsiniz `docker login` temel kimlik doğrulaması yapmak için kayıt defteri komutu. Örneğin:

```
docker login myregistry.azurecr.io -u myAdminName -p myPassword1
```

> [!IMPORTANT]
> Yönetici hesabı çoğunlukla test amaçları için kayıt defterini erişmek tek bir kullanıcı için tasarlanmıştır. Yönetici kimlik bilgileriniz diğer kullanıcılarla paylaşmak için önerilmez. Tüm kullanıcılar kayıt defterine tek bir kullanıcı olarak görünür. Değiştirme veya bu hesap devre dışı bırakma kayıt defteri erişim kimlik bilgilerini kullanan tüm kullanıcılar için devre dışı bırakır.
>

### <a name="next-steps"></a>Sonraki adımlar
* [Docker CLI kullanarak ilk görüntünüzü anında](container-registry-get-started-docker-cli.md).
* Kapsayıcı kayıt defteri Önizleme'de kimlik doğrulaması hakkında daha fazla bilgi için bkz: [blog gönderisi](https://blogs.msdn.microsoft.com/stevelasker/2016/11/17/azure-container-registry-user-accounts/).
