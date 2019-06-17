---
title: Bir hizmetin Uzaktan izleme çözümü için kullanıcı Arabirimi - Azure | Microsoft Docs
description: Bu makalede Uzaktan izleme çözüm Hızlandırıcı web kullanıcı Arabirimi yeni bir hizmet eklemek gösterilmektedir.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/02/2018
ms.topic: conceptual
ms.openlocfilehash: e44aa8ade512a6005959e795cb1d4ad861da1338
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61447055"
---
# <a name="add-a-custom-service-to-the-remote-monitoring-solution-accelerator-web-ui"></a>Uzaktan izleme çözüm Hızlandırıcı web kullanıcı Arabirimine özel hizmet ekleme

Bu makalede Uzaktan izleme çözüm Hızlandırıcı web kullanıcı Arabirimi yeni bir hizmet eklemek gösterilmektedir. Bu makalede açıklanır:

- Yerel geliştirme ortamı hazırlamayı öğrenin.
- Yeni bir hizmet web kullanıcı Arabirimi ekleme.

Bu makaledeki örnek hizmeti verileri için bir kılavuz sağlayan [özel kılavuz Uzaktan izleme çözüm Hızlandırıcı web kullanıcı Arabirimine eklemek](iot-accelerators-remote-monitoring-customize-grid.md) nasıl yapılır makalesi nasıl ekleneceğini gösterir.

React uygulamada, bir hizmet genellikle bir arka uç hizmetiyle etkileşim kurar. Uzaktan izleme çözüm Hızlandırıcısını örneklerde, mikro hizmetler IOT hub Yöneticisi ve yapılandırma ile etkileşim hizmetleri içerir.

## <a name="prerequisites"></a>Önkoşullar

Nasıl yapılır bu kılavuzdaki adımları tamamlamak için aşağıdaki yazılımların yerel geliştirme makinenizde yüklü gerekir:

- [Git](https://git-scm.com/downloads)
- [Node.js](https://nodejs.org/download/)

## <a name="before-you-start"></a>Başlamadan önce

Bölümündeki adımları tamamlamanız [Uzaktan izleme çözüm Hızlandırıcı web kullanıcı Arabirimine özel sayfa ekleme](iot-accelerators-remote-monitoring-customize-page.md) devam etmeden önce nasıl yapılır makalesi.

## <a name="add-a-service"></a>Hizmet ekleme

Web kullanıcı Arabirimine bir hizmet eklemek, hizmeti tanımlayan kaynak dosyaları ekleyin ve web kullanıcı Arabirimi yeni hizmet haberdar olmak için bazı mevcut dosyaları değiştirmek gerekir.

### <a name="add-the-new-files-that-define-the-service"></a>Hizmeti tanımlayan yeni dosyalar Ekle

Başlamak, için **gözden geçirme/src/Hizmetleri** klasörü, basit bir hizmet tanımlama dosyaları içerir:

**exampleService.js**

[!code-javascript[Example service](~/remote-monitoring-webui/src/walkthrough/services/exampleService.js?name=service "Example service")]

Hizmetleri nasıl uygulandığı hakkında daha fazla bilgi için bkz: [giriş reaktif programlama, eksik kılavuzluğa odaklanmamı](https://gist.github.com/staltz/868e7e9bc2a7b8c1f754).

**model/exampleModels.js**

[!code-javascript[Example model](~/remote-monitoring-webui/src/walkthrough/services/models/exampleModels.js?name=models "Example model")]

Kopyalama **exampleService.js** için **src/Hizmetler** klasörü ve kopyalama **exampleModels.js** için **Hizmetleri/src/modelleri** klasör.

Güncelleştirme **index.js** dosyası **src/Hizmetler** yeni hizmet klasörü:

```js
export * from './exampleService';
```

Güncelleştirme **index.js** dosyası **Hizmetleri/src/modelleri** yeni modeli klasörü:

```js
export * from './exampleModels';
```

### <a name="set-up-the-calls-to-the-service-from-the-store"></a>Hizmete çağrı deposu ayarlama

Başlamak, için **src/gözden geçirme/deposu/genişletin** örnek Azaltıcı klasör içerir:

**exampleReducer.js**

[!code-javascript[Example reducer](~/remote-monitoring-webui/src/walkthrough/store/reducers/exampleReducer.js?name=reducer "Example reducer")]

Kopyalama **exampleReducer.js** için **mağazası/src/genişletin** klasör.

Azaltıcı hakkında daha fazla bilgi edinmek ve **Epic'ler**, bkz: [observable redux](https://redux-observable.js.org/).

### <a name="configure-the-middleware"></a>Ara yazılımını yapılandırma

İçin Azaltıcı ara yazılımını yapılandırma ekleyin **rootReducer.js** dosyası **src/deposu** klasörü:

```js
import { reducer as exampleReducer } from './reducers/exampleReducer';

const rootReducer = combineReducers({
  ...appReducer,
  ...devicesReducer,
  ...rulesReducer,
  ...simulationReducer,
  ...exampleReducer
});
```

Epic'ler ekleme **rootEpics.js** dosyası **src/deposu** klasörü:

```js
import { epics as exampleEpics } from './reducers/exampleReducer';

// Extract the epic function from each property object
const epics = [
  ...appEpics.getEpics(),
  ...devicesEpics.getEpics(),
  ...rulesEpics.getEpics(),
  ...simulationEpics.getEpics(),
  ...exampleEpics.getEpics()
];
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, ekleme ya da Hizmetleri Web kullanıcı Arabiriminde, Uzaktan izleme çözüm Hızlandırıcısını özelleştirme yardımcı olacak kaynaklar hakkında bilgi edindiniz.

Bir hizmet tanımladığınız artık sonraki adım olarak [özel kılavuz Uzaktan izleme çözüm Hızlandırıcı web kullanıcı Arabirimine eklemek](iot-accelerators-remote-monitoring-customize-grid.md) hizmet tarafından döndürülen verileri görüntüler.

Uzaktan izleme çözüm Hızlandırıcısını hakkında daha fazla kavramsal bilgi için bkz. [Uzaktan izleme mimarisi](iot-accelerators-remote-monitoring-sample-walkthrough.md).
