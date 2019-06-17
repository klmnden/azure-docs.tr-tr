---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 01/24/2019
ms.openlocfilehash: 4cdcec850f32d7e94f33eb28e5bf7839e511f347
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66124578"
---
Doldurun ve gönderme [Bilişsel hizmetler görüntü işleme kapsayıcıları istek formunu](https://aka.ms/VisionContainersPreview) kapsayıcıya erişim istemek için. Form, şirketiniz ve kapsayıcı kullanacağınız kullanıcı senaryosu hakkında bilgi ister. Siz formu gönderdikten sonra Azure Bilişsel hizmetler takım özel kapsayıcı kayıt defterine erişim için ölçütleri karşıladığından emin olmak için gözden geçirir.

> [!IMPORTANT]
> Bir Microsoft hesabı (MSA) ya da biçiminde bir Azure Active Directory (Azure AD) hesabı ile ilişkili bir e-posta adresi kullanmanız gerekir.

İsteğiniz onaylandı, kimlik bilgilerinizi edinme ve özel kapsayıcı kayıt defterine erişim açıklayan yönergeler içeren bir e-posta alırsınız.

## <a name="log-in-to-the-private-container-registry"></a>Özel kapsayıcı kayıt defteri oturum açma

Bilişsel hizmetler, kapsayıcılar için özel kapsayıcı kayıt defteri ile kimlik doğrulaması yapmak için birkaç yol vardır. Komut satırı yöntemini kullanarak kullanmanızı öneririz [Docker CLI](https://docs.docker.com/engine/reference/commandline/cli/).

Kullanım [docker oturum açma](https://docs.docker.com/engine/reference/commandline/login/) komutu, oturum açmak için aşağıdaki örnekte gösterildiği gibi `containerpreview.azurecr.io`, Bilişsel hizmetler, kapsayıcılar için özel kapsayıcı kayıt defteri olduğu. Değiştirin *\<kullanıcıadı\>* kullanıcı adıyla ve *\<parola\>* Azure'dan aldığınız kimlik bilgileri sağlanan parolayla Bilişsel Hizmetleri ekibi.

```
docker login containerpreview.azurecr.io -u <username> -p <password>
```

Bir metin dosyasındaki kimlik bilgilerinizi güvenli, bu dosyanın içeriğini bitiştirebilirsiniz `docker login` komutu. Kullanım `cat` , aşağıdaki örnekte gösterildiği gibi komutu. Değiştirin *\<passwordFile\>* parola içeren metin dosyası adı ve yolu. Değiştirin *\<kullanıcıadı\>* kimlik bilgilerinizi sağlanan kullanıcı adı.

```
cat <passwordFile> | docker login containerpreview.azurecr.io -u <username> --password-stdin
```

