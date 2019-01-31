---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 01/24/2019
ms.openlocfilehash: 08e6b5d109d6647f2a291f117f4993bae7598464
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55302333"
---
Önce tamamlamanız ve gönderme gerekir [Bilişsel hizmetler görüntü işleme kapsayıcıları istek formunu](https://aka.ms/VisionContainersPreview) kapsayıcıya erişim istemek için. Form, şirketiniz ve kapsayıcı kullanacağınız kullanıcı senaryosu hakkında bilgi ister. Azure Bilişsel hizmetler takım gönderilen sonra özel kapsayıcı kayıt defterine erişim için ölçütleri karşıladığından emin olmak üzere form gözden geçirir.

> [!IMPORTANT]
> Formu bir Microsoft hesabı (MSA) veya Azure Active Directory (Azure AD) hesabı ile ilişkili bir e-posta adresi kullanmanız gerekir.

İsteğiniz onaylandı, kimlik bilgilerinizi edinme ve özel kapsayıcı kayıt defterine erişim açıklayan yönergeler içeren bir e-posta alırsınız.

## <a name="log-in-to-the-private-container-registry"></a>Özel kapsayıcı kayıt defteri oturum açma

Bilişsel hizmetler kapsayıcılar için özel kapsayıcı kayıt defteri ile kimlik doğrulaması yapmak için birkaç yolu vardır, ancak önerilen yöntem komut satırından kullanmaktır [Docker CLI](https://docs.docker.com/engine/reference/commandline/cli/).

Kullanım [docker oturum açma](https://docs.docker.com/engine/reference/commandline/login/) komutu, oturum açmak için aşağıdaki örnekte gösterildiği gibi `containerpreview.azurecr.io`, Bilişsel hizmetler kapsayıcılar için özel kapsayıcı kayıt defteri. Değiştirin *\<kullanıcıadı\>* kullanıcı adıyla ve *\<parola\>* Azure'dan aldığınız kimlik bilgileri sağlanan parolayla Bilişsel Hizmetleri ekibi.

```docker
docker login containerpreview.azurecr.io -u <username> -p <password>
```

Bir metin dosyasındaki kimlik bilgilerinizi sağladıktan ise, metin içeriğini bitiştirebilirsiniz kullanarak dosya `cat` komutu için `docker login` komutu aşağıdaki örnekte gösterildiği gibi. Değiştirin *\<passwordFile\>* ile parolasını içeren bir metin dosyasının adını ve yolunu ve *\<kullanıcıadı\>* kullanıcı adı kimlik bilgilerinizi sağlanır.

```docker
cat <passwordFile> | docker login containerpreview.azurecr.io -u <username> --password-stdin
```

