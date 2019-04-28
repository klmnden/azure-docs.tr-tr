---
title: Uzaktan izleme çözümü için kullanıcı Arabirimi - Azure bir sayfa ekleyin | Microsoft Docs
description: Bu makalede, Uzaktan izleme çözüm Hızlandırıcı web kullanıcı Arabirimi yeni bir sayfa ekleme işlemini göstermektedir.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/02/2018
ms.topic: conceptual
ms.openlocfilehash: 95830cdffb232e16f9fbae51cfa11fbd18172c3c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61447089"
---
# <a name="add-a-custom-page-to-the-remote-monitoring-solution-accelerator-web-ui"></a>Uzaktan izleme çözüm Hızlandırıcı web kullanıcı Arabirimine özel sayfa ekleme

Bu makalede, Uzaktan izleme çözüm Hızlandırıcı web kullanıcı Arabirimi yeni bir sayfa ekleme işlemini göstermektedir. Bu makalede açıklanır:

- Yerel geliştirme ortamı hazırlamayı öğrenin.
- Yeni bir sayfa web kullanıcı Arabirimi ekleme.

Diğer nasıl yapılır kılavuzları, daha fazla özellik eklediğiniz sayfasına eklemek için bu senaryo genişletin.

## <a name="prerequisites"></a>Önkoşullar

Nasıl yapılır bu kılavuzdaki adımları tamamlamak için aşağıdaki yazılımların yerel geliştirme makinenizde yüklü gerekir:

- [Git](https://git-scm.com/downloads)
- [Node.js](https://nodejs.org/download/)

## <a name="prepare-a-local-development-environment-for-the-ui"></a>Kullanıcı Arabirimi için bir yerel geliştirme ortamınızı hazırlama

Uzaktan izleme çözüm Hızlandırıcısını UI kod kullanılarak uygulanan [React](https://reactjs.org/) JavaScript çerçevesini. Kaynak kodunda bulabilirsiniz [Uzaktan izleme WebUI](https://github.com/Azure/pcs-remote-monitoring-webui) GitHub deposu.

Ve kullanıcı arabirimi değişiklikleri test etmek için yerel geliştirme makinenizde çalıştırabilirsiniz. İsteğe bağlı olarak yerel kopya gerçek veya sanal cihazlarınızı etkileşim sağlamak için çözüm Hızlandırıcısını dağıtılan bir örneğini bağlanabilirsiniz.

Yerel geliştirme ortamınızı hazırlama için kopyalamak için Git kullanın [Uzaktan izleme WebUI](https://github.com/Azure/pcs-remote-monitoring-webui) deposunu yerel makinenize:

```cmd/sh
git clone https://github.com/Azure/pcs-remote-monitoring-webui.git
```

## <a name="add-a-page"></a>Sayfa ekleme

Web kullanıcı Arabirimine bir sayfa eklemek için sayfanın tanımlayan kaynak dosyaları ekleyin ve web kullanıcı Arabirimi yeni sayfa haberdar olmak için bazı mevcut dosyaları değiştirmek gerekir.

### <a name="add-the-new-files-that-define-the-page"></a>Tanımlama sayfası yeni dosyaları Ekle

Başlamak, için **src/gözden geçirme/bileşenleri/sayfaları/basicPage** klasörü, basit bir sayfa tanımlama dört dosyaları içerir:

**basicPage.container.js**

[!code-javascript[Page container source](~/remote-monitoring-webui/src/walkthrough/components/pages/basicPage/basicPage.container.js?name=container "Page container source")]

**basicPage.js**

[!code-javascript[Basic page](~/remote-monitoring-webui/src/walkthrough/components/pages/basicPage/basicPage.js?name=page "Basic page")]

**basicPage.scss**

[!code-javascript[Page styling](~/remote-monitoring-webui/src/walkthrough/components/pages/basicPage/basicPage.scss?name=styles "Page styling")]

**basicPage.test.js**

[!code-javascript[Test code for basic page](~/remote-monitoring-webui/src/walkthrough/components/pages/basicPage/basicPage.test.js?name=test "Test code for basic page")]

Yeni bir klasör oluşturun **src/bileşenleri/sayfaları/örnek** ve şu dört dosyaları içine kopyalayın.

### <a name="add-the-new-page-to-the-web-ui"></a>Web kullanıcı Arabirimine yeni sayfa Ekle

Web kullanıcı Arabirimi yeni sayfa eklemek, var olan dosyaları için aşağıdaki değişiklikleri yapın:

1. Yeni sayfa kapsayıcıya Ekle **src/components/pages/index.js** dosyası:

    ```js
    export * from './example/basicPage.container';
    ```

1. (İsteğe bağlı)  Yeni sayfa için bir SVG simgesi ekleyin. Daha fazla bilgi için [webui/src/utilities/README.md](https://github.com/Azure/pcs-remote-monitoring-webui/blob/master/src/utilities/README.md). Mevcut bir SVG dosyasının kullanabilirsiniz.

1. Sayfa adı çevirileri dosyasına ekleme **public/locales/en/translations.json**. Kullanıcı Arabirimi kullanan web [i18next](https://www.i18next.com/) uluslararası duruma getirme için.

    ```json
    "tabs": {
      "basicPage": "Example",
    },
    ```

1. Açık **src/components/app.js** en üst düzey uygulama sayfası tanımlayan dosya. İçeri aktarmalar listesine yeni bir sayfa ekleyin:

    ```javascript
    // Page Components
    import  {
      //...
      BasicPageContainer
    } from './pages';
    ```

1. Aynı dosyada, yeni sayfaya ekleyin `pagesConfig` dizisi. Ayarlama `to` adres için rota, SVG simgesi ve daha önce eklediğiniz çevirileri başvurmak ve ayarlama `component` sayfanın kapsayıcıya:

    ```js
    const pagesConfig = [
      //...
      {
        to: '/basicpage',
        exact: true,
        svg: svgs.tabs.example,
        labelId: 'tabs.basicPage',
        component: BasicPageContainer
      },
      //...
    ];
    ```

1. Eklemek istediğiniz yeni içerik haritaları için `crumbsConfig` dizisi:

    ```js
    const crumbsConfig = [
      //...
      {
        path: '/basicpage', crumbs: [
          { to: '/basicpage', labelId: 'tabs.basicPage' }
        ]
      },
      //...
    ];
    ```

    Bu örnek sayfası, yalnızca bir içerik haritası içerir, ancak bazı sayfalar daha fazla olabilir.

Yaptığınız tüm değişiklikleri kaydedin. Web kullanıcı Arabirimi eklenen yeni sayfanız ile çalışmaya hazır.

### <a name="test-the-new-page"></a>Yeni sayfa test

Bir komut isteminde yerel kopyanızı deposunun kök dizinine gidin ve gerekli kitaplıkları yükleyin ve web kullanıcı Arabirimi yerel olarak çalıştırmak için aşağıdaki komutları çalıştırın:

```cmd/sh
npm install
npm start
```

Önceki komutta kullanıcı Arabiriminde, yerel olarak çalışan [ http://localhost:3000/dashboard ](http://localhost:3000/dashboard).

Çözüm Hızlandırıcısını dağıtılan bir örneğine yerel web kullanıcı Arabirimi örneğinizin bağlanmadan Panoda görüyorsunuz. Yeni sayfanız test yeteneği bu hataları etkilemez.

Şimdi, sitenin yerel olarak çalışırken kod düzenleme ve UI'yi güncellemeye dinamik olarak web bakın.

## <a name="optional-connect-to-deployed-instance"></a>[İsteğe bağlı] Dağıtılan bir sunucuya bağlanın

İsteğe bağlı olarak, Uzaktan izleme çözüm Hızlandırıcısını bulutta çalışan yerel web kullanıcı Arabirimi kopyası bağlanabilirsiniz:

1. Dağıtım bir **temel** çözüm Hızlandırıcı kullanarak örneğini **bilgisayarları** CLI. Dağıtımınız ve sanal makine için sağlanan kimlik bilgileri adını not edin. Daha fazla bilgi için [CLI kullanarak dağıtma](iot-accelerators-remote-monitoring-deploy-cli.md).

1. Azure portalını kullanın veya [az CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) çözümünüzdeki mikro hizmetleri barındıran sanal makineye SSH erişimini etkinleştirmek için. Örneğin:

    ```sh
    az network nsg rule update --name SSH --nsg-name {your solution name}-nsg --resource-group {your solution name} --access Allow
    ```

    Yalnızca, test ve geliştirme sırasında SSH erişimini etkinleştirmeniz gerekir. SSH, etkinleştirirseniz [yeniden olabildiğince çabuk devre dışı](../security/azure-security-network-security-best-practices.md).

1. Azure portalını kullanın veya [az CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) adı ve sanal makinenizin genel IP adresini bulmak için. Örneğin:

    ```sh
    az resource list --resource-group {your solution name} -o table
    az vm list-ip-addresses --name {your vm name from previous command} --resource-group {your solution name} -o table
    ```

1. IP adresi önceki adımda ve çalıştırdığınızda sağladığınız kimlik bilgileri kullanarak sanal makinenize bağlanmak için SSH kullanın **bilgisayarları** çözüm dağıtın.

1. Bağlanmak yerel kullanıcı Deneyimini izin vermek için sanal makine bash kabuğunda aşağıdaki komutları çalıştırın:

    ```sh
    cd /app
    sudo ./start.sh --unsafe
    ```

1. Komut tamamlandıktan ve web sitesini başlatır gördükten sonra sanal makineden kesebilirsiniz.

1. Yerel kopyanızı içinde [Uzaktan izleme WebUI](https://github.com/Azure/pcs-remote-monitoring-webui) havuzu Düzenle **.env** dosya dağıtılan çözümünüzün URL'si eklemek için:

    ```config
    NODE_PATH = src/
    REACT_APP_BASE_SERVICE_URL=https://{your solution name}.azurewebsites.net/
    ```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Uzaktan izleme çözüm Hızlandırıcısını web kullanıcı Arabirimi özelleştirmenize yardımcı olacak kaynaklar hakkında bilgi edindiniz.

Tanımladığınız bir sayfa artık sonraki adım olarak [özel hizmet Uzaktan izleme çözüm Hızlandırıcı web kullanıcı Arabirimine ekleme](iot-accelerators-remote-monitoring-customize-service.md) kullanıcı Arabiriminde görüntülenecek veri alır.

Uzaktan izleme çözüm Hızlandırıcısını hakkında daha fazla kavramsal bilgi için bkz. [Uzaktan izleme mimarisi](iot-accelerators-remote-monitoring-sample-walkthrough.md).
