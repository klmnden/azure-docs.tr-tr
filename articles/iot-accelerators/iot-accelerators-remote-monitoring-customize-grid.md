---
title: Uzaktan izleme çözümü için kullanıcı Arabirimi - Azure Kılavuz ekleme | Microsoft Docs
description: Bu makalede Uzaktan izleme çözüm Hızlandırıcı web kullanıcı Arabirimi sayfasındaki yeni GID ekleme gösterilmektedir.
author: dominicbetts
manager: timlt
ms.author: v-yiso
ms.service: iot-accelerators
services: iot-accelerators
origin.date: 10/04/2018
ms.date: 11/26/2018
ms.topic: conceptual
ms.openlocfilehash: a24cb7f39ccb8ea07d4dde2869dc7c924b91983a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61447106"
---
# <a name="add-a-custom-grid-to-the-remote-monitoring-solution-accelerator-web-ui"></a>Uzaktan izleme çözüm Hızlandırıcı web kullanıcı Arabirimine özel kılavuz ekleme

Bu makalede Uzaktan izleme çözüm Hızlandırıcı web kullanıcı Arabiriminde bir sayfaya yeni bir kılavuz ekleme gösterilmektedir. Bu makalede açıklanır:

- Yerel geliştirme ortamı hazırlamayı öğrenin.
- Yeni bir kılavuz Web kullanıcı Arabiriminde bir sayfa ekleme.

Bu makaledeki örnek grid hizmetinden verileri görüntüler, [özel hizmet Uzaktan izleme çözüm Hızlandırıcı web kullanıcı Arabirimine ekleme](iot-accelerators-remote-monitoring-customize-service.md) nasıl yapılır makalesi nasıl ekleneceğini gösterir.

## <a name="prerequisites"></a>Önkoşullar

Nasıl yapılır bu kılavuzdaki adımları tamamlamak için aşağıdaki yazılımların yerel geliştirme makinenizde yüklü gerekir:

- [Git](https://git-scm.com/downloads)
- [Node.js](https://nodejs.org/download/)

## <a name="before-you-start"></a>Başlamadan önce

Devam etmeden önce aşağıdaki makaleleri adımları tamamlaması gerekir:

- [Uzaktan izleme çözüm Hızlandırıcı web kullanıcı Arabirimine özel sayfa ekleme](iot-accelerators-remote-monitoring-customize-page.md).
- [Uzaktan izleme çözüm Hızlandırıcı web kullanıcı Arabirimine özel hizmet ekleme](iot-accelerators-remote-monitoring-customize-service.md)

## <a name="add-a-grid"></a>Kılavuz ekleme

Web kullanıcı Arabirimine bir kılavuz eklemek için kılavuz tanımlayan kaynak dosyaları ekleyin ve web kullanıcı Arabirimi yeni bileşen haberdar olmak için bazı mevcut dosyaları değiştirmek gerekir.

### <a name="add-the-new-files-that-define-the-grid"></a>Kılavuz tanımlayan yeni dosyalar Ekle

Başlamak, için **gözden geçirme/src/bileşenleri/pageWithGrid/sayfaları/exampleGrid** klasörü tanımlayan bir kılavuz dosyalarını içerir:

**exampleGrid.js**



**exampleGridConfig.js**



Kopyalama **gözden geçirme/src/bileşenleri/pageWithGrid/sayfaları/exampleGrid** klasörüne **src/bileşenleri/sayfaları/örnek** klasör.

### <a name="add-the-grid-to-the-page"></a>Kılavuz sayfasına ekleme

Değiştirme **src/components/pages/example/basicPage.container.js** gibi hizmet tanımı içeri aktarmak için:

```js
import { connect } from 'react-redux';
import { translate } from 'react-i18next';

import {
  epics as exampleEpics,
  getExamples,
  getExamplesError,
  getExamplesLastUpdated,
  getExamplesPendingStatus
} from 'store/reducers/exampleReducer';
import { BasicPage } from './basicPage';

// Pass the data
const mapStateToProps = state => ({
  data: getExamples(state),
  error: getExamplesError(state),
  isPending: getExamplesPendingStatus(state),
  lastUpdated: getExamplesLastUpdated(state)
});

// Wrap the dispatch method
const mapDispatchToProps = dispatch => ({
  fetchData: () => dispatch(exampleEpics.actions.fetchExamples())
});

export const BasicPageContainer = translate()(connect(mapStateToProps, mapDispatchToProps)(BasicPage));
```

Değiştirme **src/components/pages/example/basicPage.js** gibi kılavuz eklemek için:

```js
// Copyright (c) Microsoft. All rights reserved.

import React, { Component } from 'react';

import {
  AjaxError,
  ContextMenu,
  PageContent,
  RefreshBar
} from 'components/shared';
import { ExampleGrid } from './exampleGrid';

import './basicPage.css';

export class BasicPage extends Component {
  constructor(props) {
    super(props);
    this.state = { contextBtns: null };
  }

  componentDidMount() {
    const { isPending, lastUpdated, fetchData } = this.props;
    if (!lastUpdated && !isPending) fetchData();
  }

  onGridReady = gridReadyEvent => this.gridApi = gridReadyEvent.api;

  onContextMenuChange = contextBtns => this.setState({ contextBtns });

  render() {
    const { t, data, error, isPending, lastUpdated, fetchData } = this.props;
    const gridProps = {
      onGridReady: this.onGridReady,
      rowData: isPending ? undefined : data || [],
      onContextMenuChange: this.onContextMenuChange,
      t: this.props.t
    };

    return [
      <ContextMenu key="context-menu">
        {this.state.contextBtns}
      </ContextMenu>,
      <PageContent className="basic-page-container" key="page-content">
        <RefreshBar refresh={fetchData} time={lastUpdated} isPending={isPending} t={t} />
        {!!error && <AjaxError t={t} error={error} />}
        {!error && <ExampleGrid {...gridProps} />}
      </PageContent>
    ];
  }
}
```

Değiştirme **src/components/pages/example/basicPage.test.js** testler şu şekilde güncelleştirmek için:

```js
// Copyright (c) Microsoft. All rights reserved.

import React from 'react';
import { shallow } from 'enzyme';
import 'polyfills';

import { BasicPage } from './basicPage';

describe('BasicPage Component', () => {
  it('Renders without crashing', () => {

    const fakeProps = {
      data: undefined,
      error: undefined,
      isPending: false,
      lastUpdated: undefined,
      fetchData: () => { },
      t: () => { },
    };

    const wrapper = shallow(
      <BasicPage {...fakeProps} />
    );
  });
});
```

## <a name="test-the-grid"></a>Test Kılavuzu

Web kullanıcı Arabirimi yerel olarak çalışır durumda değilse, yerel kopyanızı deponun kök dizininde aşağıdaki komutu çalıştırın:

```cmd/sh
npm start
```

Önceki komutta kullanıcı Arabiriminde, yerel olarak çalışan [ http://localhost:3000/dashboard ](http://localhost:3000/dashboard). Gidin **örnek** hizmetinden verileri görüntülemek kılavuz görmek için sayfayı.

## <a name="select-rows"></a>Satırları Seç

Kılavuz satırları seçmek için bir kullanıcı etkinleştirme iki seçenek vardır:

### <a name="hard-select-rows"></a>Select sabit satır

Bir kullanıcı aynı anda birden çok satırda yapması gerekiyorsa satırlarda onay kutularını kullanın:

1. Seçimi satır sabit etkinleştirmek bir **checkboxColumn** için **columnDefs** kılavuza sağlanan. **checkboxColumn** tanımlanan **/src/components/shared/pcsGrid/pcsGrid.js**:

    ```js
    this.columnDefs = [
      checkboxColumn,
      exampleColumnDefs.id,
      exampleColumnDefs.description
    ];
    ```

1. Seçilen öğelere erişmek için dahili kılavuz API başvuru alın:

    ```js
    onGridReady = gridReadyEvent => {
      this.gridApi = gridReadyEvent.api;
      // Call the onReady props if it exists
      if (isFunc(this.props.onGridReady)) {
        this.props.onGridReady(gridReadyEvent);
      }
    };
    ```

1. Kılavuzundaki bir satır seçili sabit olduğunda sayfa bağlamı düğmeleri sağlar:

    ```js
    this.contextBtns = [
      <Btn key="context-btn-1" svg={svgs.reconfigure} onClick={this.doSomething()}>Button 1</Btn>,
      <Btn key="context-btn-2" svg={svgs.trash} onClick={this.doSomethingElse()}>Button 2</Btn>
    ];
    ```

    ```js
    onHardSelectChange = (selectedObjs) => {
      const { onContextMenuChange, onHardSelectChange } = this.props;
      // Show the context buttons when there are rows checked.
      if (isFunc(onContextMenuChange)) {
        onContextMenuChange(selectedObjs.length > 0 ? this.contextBtns : null);
      }
      //...
    }
    ```

1. Bir bağlam düğmesine tıklandığında, işlerinizi yapmak için sabit seçili öğeleri alın:

    ```js
    doSomething = () => {
      //Just for demo purposes. Don't console log in a real grid.
      console.log('hard selected rows', this.gridApi.getSelectedRows());
    };
    ```

### <a name="soft-select-rows"></a>Satırları geçici seçin

Kullanıcı yalnızca tek bir satırda yapması gerekiyorsa, bir veya daha fazla sütun için bir yumuşak seçin bağlantısını yapılandırmak **columnDefs**.

1. İçinde **exampleGridConfig.js**, ekleme **SoftSelectLinkRenderer** olarak **cellRendererFramework** için bir **columnDef**.

    ```js
    export const exampleColumnDefs = {
      id: {
        headerName: 'examples.grid.name',
        field: 'id',
        sort: 'asc',
        cellRendererFramework: SoftSelectLinkRenderer
      }
    };
    ```

1. Bir yazılım Seç bağlantısına tıklandığında tetikler **onSoftSelectChange** olay. Hangi eylemi ayrıntıları açılan menüyü açarak gibi ilgili satır için istenen gerçekleştirin. Bu örnek yalnızca konsola yazar:

    ```js
    onSoftSelectChange = (rowId, rowEvent) => {
      const { onSoftSelectChange } = this.props;
      const obj = (this.gridApi.getDisplayedRowAtIndex(rowId) || {}).data;
      if (obj) {
        //Just for demo purposes. Don't console log a real grid.
        console.log('Soft selected', obj);
        this.setState({ softSelectedObj: obj });
      }
      if (isFunc(onSoftSelectChange)) {
        onSoftSelectChange(obj, rowEvent);
      }
    }
    ```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, ekleme veya Uzaktan izleme çözüm Hızlandırıcısını Web kullanıcı Arabiriminde sayfalarını özelleştirme yardımcı olacak kaynaklar hakkında bilgi edindiniz.

Sonraki adım bir kılavuz tanımladığınız artık içerir [Uzaktan izleme çözüm Hızlandırıcı web kullanıcı Arabirimine özel bir geçici açılır pencere ekleme](iot-accelerators-remote-monitoring-customize-flyout.md) örnek sayfasında görüntüler.

Uzaktan izleme çözüm Hızlandırıcısını hakkında daha fazla kavramsal bilgi için bkz. [Uzaktan izleme mimarisi](iot-accelerators-remote-monitoring-sample-walkthrough.md).
