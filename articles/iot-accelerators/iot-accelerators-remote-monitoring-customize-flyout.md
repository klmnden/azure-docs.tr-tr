---
title: Uzaktan izleme çözümü için kullanıcı Arabirimi - Azure bir geçici açılır pencere ekleme | Microsoft Docs
description: Bu makalede Uzaktan izleme çözüm Hızlandırıcı Web kullanıcı Arabiriminde sayfasında yeni bir geçici açılır pencere ekleme gösterilmektedir.
author: dominicbetts
manager: timlt
ms.author: v-yiso
ms.service: iot-accelerators
services: iot-accelerators
origin.date: 10/05/2018
ms.date: 11/26/2018
ms.topic: conceptual
ms.openlocfilehash: ccb1a7ff6abbc68f42c7632a8ba7a392b2c48794
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61447123"
---
# <a name="add-a-custom-flyout-to-the-remote-monitoring-solution-accelerator-web-ui"></a>Uzaktan izleme çözüm Hızlandırıcı web kullanıcı Arabirimine özel bir açılır öğesi Ekle

Bu makalede Uzaktan izleme çözüm Hızlandırıcı web kullanıcı Arabiriminde bir sayfaya yeni bir geçici açılır pencere ekleme gösterilmektedir. Bu makalede açıklanır:

- Yerel geliştirme ortamı hazırlamayı öğrenin.
- Web kullanıcı Arabiriminde bir sayfaya yeni bir geçici açılır pencere ekleme yapma.

Bu makaledeki örnek çıkma görüntüler kılavuzuna sayfasında, [özel kılavuz Uzaktan izleme çözüm Hızlandırıcı web kullanıcı Arabirimine eklemek](iot-accelerators-remote-monitoring-customize-grid.md) nasıl yapılır makalesi nasıl ekleneceğini gösterir.

## <a name="prerequisites"></a>Önkoşullar

Nasıl yapılır bu kılavuzdaki adımları tamamlamak için aşağıdaki yazılımların yerel geliştirme makinenizde yüklü gerekir:

- [Git](https://git-scm.com/downloads)
- [Node.js](https://nodejs.org/download/)

## <a name="before-you-start"></a>Başlamadan önce

Devam etmeden önce aşağıdaki makaleleri adımları tamamlaması gerekir:

- [Uzaktan izleme çözüm Hızlandırıcı web kullanıcı Arabirimine özel sayfa ekleme](iot-accelerators-remote-monitoring-customize-page.md).
- [Uzaktan izleme çözüm Hızlandırıcı web kullanıcı Arabirimine özel hizmet ekleme](iot-accelerators-remote-monitoring-customize-service.md)
- [Uzaktan izleme çözüm Hızlandırıcı web kullanıcı Arabirimine özel kılavuz ekleme](iot-accelerators-remote-monitoring-customize-grid.md)

## <a name="add-a-flyout"></a>Açılır öğe ekleme

Web kullanıcı Arabirimine bir açılır öğesi eklemek için açılan menüyü tanımlayan kaynak dosyalarını ekleyin ve web kullanıcı Arabirimi yeni bileşen haberdar olmak için bazı mevcut dosyaları değiştirmek gerekir.

### <a name="add-the-new-files-that-define-the-flyout"></a>Açılan menüyü tanımlayan yeni dosyalar Ekle

Başlamak, için **src/gözden geçirme/bileşenleri/sayfaları/pageWithFlyout/çıkarmalar/exampleFlyout** klasörü açılır pencere tanımlayan dosyalarını içerir:

**exampleFlyout.container.js**



**exampleFlyout.js**



Kopyalama **gözden geçirme/src/bileşenleri/pageWithFlyout/sayfaları/çıkarmalar** klasörüne **src/bileşenleri/sayfaları/örnek** klasör.

### <a name="add-the-flyout-to-the-page"></a>Açılan menüyü sayfasına ekleme

Değiştirme **src/components/pages/example/basicPage.js** açılır öğesi eklemek için.

Ekleme **Btn** içeri aktarmalar için **/shared components** ve eklemek için içeri aktarmalar **svgs** ve **ExampleFlyoutContainer**:

```js
import {
  AjaxError,
  ContextMenu,
  PageContent,
  RefreshBar,
  Btn
} from 'components/shared';
import { ExampleGrid } from './exampleGrid';
import { svgs } from 'utilities';
import { ExampleFlyoutContainer } from './flyouts/exampleFlyout';
```

Ekleme bir **const** tanımı **closedFlyoutState** ve durumda bir oluşturucu ekleyin:

```js
const closedFlyoutState = { openFlyoutName: undefined };

export class BasicPage extends Component {
  constructor(props) {
    super(props);
    this.state = { contextBtns: null, closedFlyoutState };
  }
```

Aşağıdaki işlevleri'ne ekleyerek **BasicPage** sınıfı:

```js
  closeFlyout = () => this.setState(closedFlyoutState);

  openFlyout = (name) => () => this.setState({ openFlyoutName: name });
```

Aşağıdaki **const** tanımları **işleme** işlevi:

```js
    const { openFlyoutName } = this.state;

    const isExampleFlyoutOpen = openFlyoutName === 'example';
```

Açılan menüyü için bağlam menüsünü açmak için bir düğme ekleyin:

```js
      <ContextMenu key="context-menu">
        {this.state.contextBtns}
        <Btn svg={svgs.reconfigure} onClick={this.openFlyout('example')}>{t('walkthrough.pageWithFlyout.open')}</Btn>
      </ContextMenu>,
```

Metin ve çıkma kapsayıcı sayfası içeriği ekleyin:

```js
      <PageContent className="basic-page-container" key="page-content">
        {t('walkthrough.pageWithFlyout.pageBody')}
        { isExampleFlyoutOpen && <ExampleFlyoutContainer onClose={this.closeFlyout} /> }
        <RefreshBar refresh={fetchData} time={lastUpdated} isPending={isPending} t={t} />
        {!!error && <AjaxError t={t} error={error} />}
        {!error && <ExampleGrid {...gridProps} />}
      </PageContent>
```

## <a name="test-the-flyout"></a>Açılır öğesi test etme

Web kullanıcı Arabirimi yerel olarak çalışır durumda değilse, yerel kopyanızı deponun kök dizininde aşağıdaki komutu çalıştırın:

```cmd/sh
npm start
```

Önceki komutta kullanıcı Arabiriminde, yerel olarak çalışan [ http://localhost:3000/dashboard ](http://localhost:3000/dashboard). Gidin **örnek** sayfasında ve tıklayın **açık çıkma**.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, ekleme veya Uzaktan izleme çözüm Hızlandırıcısını Web kullanıcı Arabiriminde sayfalarını özelleştirme yardımcı olacak kaynaklar hakkında bilgi edindiniz.

Bir sayfada açılır pencere tanımladığınız artık sonraki adım olarak [panoya Uzaktan izleme çözüm Hızlandırıcı Web kullanıcı Arabiriminde bir panel ekleme](iot-accelerators-remote-monitoring-customize-panel.md).

Uzaktan izleme çözüm Hızlandırıcısını hakkında daha fazla kavramsal bilgi için bkz. [Uzaktan izleme mimarisi](iot-accelerators-remote-monitoring-sample-walkthrough.md).
