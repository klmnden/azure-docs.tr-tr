---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 05/07/2019
ms.openlocfilehash: 02beb6bdce096c35d8948cc8aefab6cc97be907c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67063977"
---
## <a name="use-the-docker-cli-to-authenticate-the-private-container-registry"></a>Özel kapsayıcı kayıt defteri kimlik doğrulaması için Docker CLI'yı kullanın

Bilişsel hizmetler kapsayıcılarla birkaç şekilde özel kapsayıcı kayıt defteri ile doğrulayabilir, ancak önerilen yöntem komut satırından [Docker CLI](https://docs.docker.com/engine/reference/commandline/cli/).

Kullanım [ `docker login` komut](https://docs.docker.com/engine/reference/commandline/login/)oturum açmak için aşağıdaki örnekte gösterildiği gibi `containerpreview.azurecr.io`, Bilişsel hizmetler kapsayıcılar için özel kapsayıcı kayıt defteri. Değiştirin *\<kullanıcıadı\>* kullanıcı adıyla ve *\<parola\>* kimlik bilgilerinde aldığınız sağlanan parola Azure Bilişsel hizmetler ekibi.

```
docker login containerpreview.azurecr.io -u <username> -p <password>
```

Bir metin dosyasındaki kimlik bilgilerinizi güvenli hale getirdiniz ise, metin dosyasının içeriğini kullanarak bitiştirebilirsiniz `cat` komutu için `docker login` , aşağıdaki örnekte gösterildiği gibi komutu. Değiştirin *\<passwordFile\>* ile parolasını içeren bir metin dosyasının adını ve yolunu ve *\<kullanıcıadı\>* kullanıcı adı kimlik bilgilerinizi sağlanır.

```
cat <passwordFile> | docker login containerpreview.azurecr.io -u <username> --password-stdin
```
