---
title: Uzaktan izleme çözümü için kullanıcı Arabirimi - Azure bir çıkış Ekle | Microsoft Docs
description: Bu makalede Uzaktan izleme çözüm Hızlandırıcı web kullanıcı Arabirimi sayfasındaki bir yeni çıkış ekleme gösterilmektedir.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/05/2018
ms.topic: conceptual
ms.openlocfilehash: 9ba58ca887332d2ea224320951b25031cacbef0d
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2018
ms.locfileid: "49094622"
---
# <a name="add-a-custom-fly-out-to-the-remote-monitoring-solution-accelerator-web-ui"></a>Bir özel çıkış Uzaktan izleme çözüm Hızlandırıcı web kullanıcı Arabirimi ekleme

Bu makalede Uzaktan izleme çözüm Hızlandırıcı web kullanıcı Arabiriminde bir yeni çıkış bir sayfaya ekleyin gösterilmektedir. Bu makalede açıklanır:

- Yerel geliştirme ortamı hazırlamayı öğrenin.
- Web kullanıcı Arabiriminde bir sayfaya bir yeni çıkış ekleme yapma.

Örnek çıkış bu makalede kılavuz sayfasıyla görüntüler, [özel kılavuz Uzaktan izleme çözüm Hızlandırıcı web kullanıcı Arabirimine eklemek](iot-accelerators-remote-monitoring-customize-grid.md) nasıl yapılır makalesi nasıl ekleneceğini gösterir.

## <a name="prerequisites"></a>Önkoşullar

Nasıl yapılır bu kılavuzdaki adımları tamamlamak için aşağıdaki yazılımların yerel geliştirme makinenizde yüklü gerekir:

- [Git](https://git-scm.com/downloads)
- [Node.js](https://nodejs.org/download/)

## <a name="before-you-start"></a>Başlamadan önce

Devam etmeden önce aşağıdaki makaleleri adımları tamamlaması gerekir:

- [Uzaktan izleme çözüm Hızlandırıcı web kullanıcı Arabirimine özel sayfa ekleme](iot-accelerators-remote-monitoring-customize-page.md).
- [Uzaktan izleme çözüm Hızlandırıcı web kullanıcı Arabirimine özel hizmet ekleme](iot-accelerators-remote-monitoring-customize-service.md)
- [Uzaktan izleme çözüm Hızlandırıcı web kullanıcı Arabirimine özel kılavuz ekleme](iot-accelerators-remote-monitoring-customize-grid.md)

## <a name="add-a-fly-out"></a>Bir çıkış Ekle

Web kullanıcı Arabirimi bir çıkış ekleme için çıkış tanımlayan kaynak dosyaları ekleyin ve web kullanıcı Arabirimi yeni bileşen haberdar olmak için bazı mevcut dosyaları değiştirmek gerekir.

### <a name="add-the-new-files-that-define-the-fly-out"></a>Çıkış tanımlayan yeni dosyalar Ekle

Başlamak, için **src/gözden geçirme/bileşenleri/sayfaları/pageWithFlyout/çıkarmalar/exampleFlyout** klasörü tanımlayan bir çıkış dosyalarını içerir:

**exampleFlyout.container.js**

[!code-javascript[Example fly-out container](~/remote-monitoring-webui/src/walkthrough/components/pages/pageWithFlyout/flyouts/exampleFlyout/exampleFlyout.container.js?name=flyoutcontainer "Example fly-out container")]

**exampleFlyout.js**

[!code-javascript[Example fly-out](~/remote-monitoring-webui/src/walkthrough/components/pages/pageWithFlyout/flyouts/exampleFlyout/exampleFlyout.js?name=flyout "Example fly-out")]

Kopyalama **gözden geçirme/src/bileşenleri/pageWithFlyout/sayfaları/çıkarmalar** klasörüne **src/bileşenleri/sayfaları/örnek** klasör.

### <a name="add-the-fly-out-to-the-page"></a>Çıkış sayfasına ekleme

Değiştirme **src/components/pages/example/basicPage.js** çıkış eklemek için.

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

Çıkış için bağlam menüsünü açmak için bir düğme ekleyin:

```js
      <ContextMenu key="context-menu">
        {this.state.contextBtns}
        <Btn svg={svgs.reconfigure} onClick={this.openFlyout('example')}>{t('walkthrough.pageWithFlyout.open')}</Btn>
      </ContextMenu>,
```

Metin ve çıkış kapsayıcısı için sayfa içeriği ekleyin:

```js
      <PageContent className="basic-page-container" key="page-content">
        {t('walkthrough.pageWithFlyout.pageBody')}
        { isExampleFlyoutOpen && <ExampleFlyoutContainer onClose={this.closeFlyout} /> }
        <RefreshBar refresh={fetchData} time={lastUpdated} isPending={isPending} t={t} />
        {!!error && <AjaxError t={t} error={error} />}
        {!error && <ExampleGrid {...gridProps} />}
      </PageContent>
```

## <a name="test-the-fly-out"></a>Çıkış test

Web kullanıcı Arabirimi yerel olarak çalışır durumda değilse, yerel kopyanızı deponun kök dizininde aşağıdaki komutu çalıştırın:

```cmd/sh
npm start
```

Önceki komutta kullanıcı Arabiriminde, yerel olarak çalışan [ http://localhost:3000/dashboard ](http://localhost:3000/dashboard). Gidin **örnek** sayfasında ve tıklayın **açık çıkma**.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, ekleme veya Uzaktan izleme çözüm Hızlandırıcısını Web kullanıcı Arabiriminde sayfalarını özelleştirme yardımcı olacak kaynaklar hakkında bilgi edindiniz.

Bir sayfada bir çıkış tanımlanmış artık sonraki adım olarak [panoya Uzaktan izleme çözüm Hızlandırıcı Web kullanıcı Arabiriminde bir panel ekleme](iot-accelerators-remote-monitoring-customize-panel.md).

Uzaktan izleme çözüm Hızlandırıcısını hakkında daha fazla kavramsal bilgi için bkz. [Uzaktan izleme mimarisi](iot-accelerators-remote-monitoring-sample-walkthrough.md).
