---
title: Azure Container Instance tarif
titleSuffix: Azure Cognitive Services
description: Azure Container Instance üzerinde Bilişsel Hizmetleri kapsayıcıları dağıtma hakkında bilgi edinin
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 06/19/2019
ms.author: dapine
ms.openlocfilehash: db73d4e30c960eb09e6b5fbc9411901c69c28b01
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67272978"
---
# <a name="deploy-and-run-container-on-azure-container-instance-aci"></a>Dağıtma ve Azure Container örneği (ACI) kapsayıcı çalıştırma

Azure ile kolayca bulut uygulamalarında Azure Bilişsel hizmetler aşağıdaki adımlarla ölçeklendirme [kapsayıcı örneği](https://docs.microsoft.com/azure/container-instances/) (ACI). Bu altyapıyı yönetmek yerine uygulamalarınıza oluşturmaya odaklanmanıza yardımcı olur.

## <a name="prerequisites"></a>Önkoşullar

Bu çözüm, tüm Bilişsel hizmetler kapsayıcısı ile çalışır. Bilişsel Hizmet kaynağı Azure portalında bu tarif kullanmadan önce oluşturulması gerekir. Yükleme ve yapılandırma için bir kapsayıcı hizmeti için özel bir "nasıl yükleneceği" Belge kapsayıcıları destekleyen her Bilişsel hizmet içerir. Bazı hizmetler kapsayıcısı için giriş olarak bir dosya veya dosyalar kümesi gerektirdiği için anlamak ve bu çözümü kullanarak önce kapsayıcı başarıyla kullandınız önemlidir.

* Azure portalında oluşturulan bir Bilişsel Hizmet kaynağı.
* Bilişsel hizmet **uç nokta URL'si** -", belirli hizmetin nasıl yükleneceği" için kapsayıcı gözden geçirin, uç nokta URL'sini gelen Azure portalı ve hangi içinde olduğu bulmak için bir doğru örnek URL'sini benzer. Hizmetten hizmete tam biçimini değiştirebilirsiniz.
* Bilişsel hizmet **anahtarı** -anahtarları bulunan **anahtarları** Azure kaynak sayfası. Yalnızca iki anahtarlarından biri gerekir. Anahtar 32 alfasayısal karakter, bir dizedir.
* Tek bir Bilişsel hizmetler kapsayıcısı yerel ana bilgisayarınızda (bilgisayar). Emin olun, şunları yapabilirsiniz:
  * Görüntüyü aşağı çekmek bir `docker pull` komutu.
  * Yerel kapsayıcı ile tüm gerekli yapılandırma ayarlarıyla başarıyla çalıştırılmış bir `docker run` komutu.
  * Kapsayıcının uç noktası, 2xx yanıtı ve bir JSON yanıtı geri alma çağırın.

Açılı ayraçlar içinde tüm değişkenlerinin `<>`, kendi değerlerinizle değiştirilmesi gerekebilir. Bu değişiklik, açılı ayraçlar içerir.

## <a name="step-2-launch-your-container-on-azure-container-instances-aci"></a>2\. adım: Kapsayıcınızı Azure Container Instances'a (ACI) üzerinde Başlat

**Azure Container örneği (ACI) kaynak oluşturuluyor.**

1. Git [Oluştur](https://ms.portal.azure.com/#create/Microsoft.ContainerInstances) Container Instances için sayfa.

1. Üzerinde **Temelleri** sekmesinde, aşağıdaki bilgileri girin:

    |Sayfa|Ayar|Değer|
    |--|--|--|
    |Temel Bilgiler|Abonelik|Aboneliğinizi seçin.|
    |Temel Bilgiler|Kaynak grubu|Kullanılabilir kaynak grubu seçin veya yeni bir tane oluşturmanız `cognitive-services`.|
    |Temel Bilgiler|Kapsayıcı adı|Gibi bir ad girin `cognitive-container-instance`. Bu ad, daha düşük bir büyük harf olarak olmalıdır.|
    |Temel Bilgiler|Location|Dağıtım için bir bölge seçin.|
    |Temel Bilgiler|Görüntü türü|`Public`|
    |Temel Bilgiler|Görüntü adı|Bilişsel hizmetler kapsayıcı konumunu girin. Bu, kullandığınız konumun aynısını olabilir `docker pull` komutu _örneğin_: <br>`mcr.microsoft.com/azure-cognitive-services/sentiment`|
    |Temel Bilgiler|İşletim sistemi türü|`Linux`|
    |Temel Bilgiler|Boyut|Belirli bir Bilişsel hizmet kapsayıcınız için önerilen önerileri boyutu değiştirin.:<br>2 Çekirdek<br>4 GB
    ||||
  
1. Üzerinde **ağ** sekmesinde, aşağıdaki bilgileri girin:

    |Sayfa|Ayar|Değer|
    |--|--|--|
    |Ağ|Bağlantı Noktaları|Varolan bir bağlantı noktasını TCP düzenlemek `80` için `5000`. Bu, kapsayıcıyı 5000 numaralı bağlantı noktasında bırakıyorsunuz anlamına gelir.|
    ||||

1. Üzerinde **Gelişmiş** sekmesinde, kapsayıcı örneğinin kaynak ayarları faturalama gerekli kapsayıcı geçirmek için aşağıdaki ayrıntıları girin:

    |Gelişmiş sayfa anahtarı|Gelişmiş sayfa değeri|
    |--|--|
    |`apikey`|Öğesinden kopyalanan **anahtarları** kaynak sayfası. İki anahtar yalnızca biri gerekir. Boşluk veya tire ile 32 bir alfasayısal karakter dizesi olduğu `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`.|
    |`billing`|Öğesinden kopyalanan **genel bakış** kaynak sayfası. |
    |`eula`|`accept`|

    Kapsayıcınızı giriş başlatmalar, çıkış başlatmalar veya günlüğe kaydetme gibi diğer yapılandırma ayarları varsa, söz konusu ayarların aynı zamanda eklenmesi gerekir.

1. Seçin **gözden geçir ve Oluştur**.
1. Doğrulama denetimini geçtikten seçin **Oluştur** oluşturma işlemini tamamlamak için.
1. Üst gezintideki zil simgesini seçin. Bildirim penceresini budur. Mavi görüntüler **kaynağa Git** kaynak oluşturulduğunda düğmesi. Yeni kaynak gitmek için bu düğmeyi seçin.

## <a name="use-the-container-instance"></a>Kapsayıcı örneğini kullan

1. Seçin **genel bakış** ve IP adresini kopyalayın. Sayısal bir IP adresi gibi olacaktır `55.55.55.55`.
1. Örneğin, IP adresi kullanın ve yeni bir tarayıcı sekmesi açın `http://<IP-address>:5000 (http://55.55.55.55:5000`). Kapsayıcıyı çalıştıran tamamlanamayacağını kapsayıcının giriş sayfasını görürsünüz.

1. Seçin **hizmet API açıklaması** kapsayıcısı için swagger sayfasını görüntülemek için.

1. Herhangi bir **POST** API'ler ve select **denemek**.  Giriş dahil olmak üzere parametreler görüntülenir. Parametrelerini doldurun.

1. Seçin **yürütme** kapsayıcı Örneğinize isteği göndermek için.

    Başarıyla oluşturulan ve Bilişsel hizmetler, kapsayıcılar, Azure Container Instance üzerinde kullanılır.
