---
title: Uzaktan izleme çözümü için kullanıcı Arabirimi - Azure bir panel ekleme | Microsoft Docs
description: Bu makalede Uzaktan izleme çözüm Hızlandırıcı web kullanıcı Arabirimi Panoda yeni bir panel eklemek gösterilmektedir.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/05/2018
ms.topic: conceptual
ms.openlocfilehash: 656a2375b8b1033e9b698d5d8e1e2bbc32b32a79
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2018
ms.locfileid: "49094588"
---
# <a name="add-a-custom-panel-to-the-dashboard-in-the-remote-monitoring-solution-accelerator-web-ui"></a>Uzaktan izleme çözüm Hızlandırıcı Web kullanıcı Arabiriminde panoya özel panel ekleme

Bu makalede Uzaktan izleme çözüm Hızlandırıcı web kullanıcı Arabiriminde bir Pano sayfasına yeni bir panel ekleme gösterilmektedir. Bu makalede açıklanır:

- Yerel geliştirme ortamı hazırlamayı öğrenin.
- Web kullanıcı Arabiriminde bir Pano sayfası için yeni bir panel ekleme yapma.

Bu makaledeki örnek paneli var olan Pano sayfasında görüntüler.

## <a name="prerequisites"></a>Önkoşullar

Nasıl yapılır bu kılavuzdaki adımları tamamlamak için aşağıdaki yazılımların yerel geliştirme makinenizde yüklü gerekir:

- [Git](https://git-scm.com/downloads)
- [Node.js](https://nodejs.org/download/)

## <a name="before-you-start"></a>Başlamadan önce

Bölümündeki adımları tamamlamanız [Uzaktan izleme çözüm Hızlandırıcı web kullanıcı Arabirimine özel sayfa ekleme](iot-accelerators-remote-monitoring-customize-page.md) devam etmeden önce makalesi.

## <a name="add-a-panel"></a>Bir panel ekleme

Web kullanıcı Arabirimine bir panel eklemek için bölmenin tanımlayan kaynak dosyaları ekleyin ve ardından paneli görüntülemek için panoyu değiştirin gerekir.

### <a name="add-the-new-files-that-define-the-panel"></a>Panel tanımlayan yeni dosyalar Ekle

Başlamak, için **src/gözden geçirme/bileşenleri/sayfaları/Pano/panelleri/examplePanel** klasörde bir panel tanımlayan dosyalar dahil olmak üzere:

**examplePanel.js**

[!code-javascript[Example panel](~/remote-monitoring-webui/src/walkthrough/components/pages/dashboard/panels/examplePanel/examplePanel.js?name=panel "Example panel")]

Kopyalama **src/gözden geçirme/bileşenleri/sayfaları/Pano/panelleri/examplePanel** klasörüne **src/bileşenleri/sayfalar/Pano/panellerini** klasör.

İçin aşağıdaki dışa aktarım Ekle **src/walkthrough/components/pages/dashboard/panels/index.js** dosyası:

```js
export * from './examplePanel';
```

### <a name="add-the-panel-to-the-dashboard"></a>Panoya panel ekleme

Değiştirme **src/components/pages/dashboard/dashboard.js** panel eklemek için.

Örnek paneli panellerinden içeri aktarmalar listesine ekleyin:

```js
import {
  OverviewPanel,
  AlertsPanel,
  TelemetryPanel,
  AnalyticsPanel,
  MapPanel,
  transformTelemetryResponse,
  chartColorObjects,
  ExamplePanel
} from './panels';
```

Sayfa içeriğini kılavuzunda aşağıdaki hücre tanımına ekleyin:

```js
          <Cell className="col-2">
            <ExamplePanel t={t} />
          </Cell>
```

## <a name="test-the-fly-out"></a>Çıkış test

Web kullanıcı Arabirimi yerel olarak çalışır durumda değilse, yerel kopyanızı deponun kök dizininde aşağıdaki komutu çalıştırın:

```cmd/sh
npm start
```

Önceki komutta kullanıcı Arabiriminde, yerel olarak çalışan [ http://localhost:3000/dashboard ](http://localhost:3000/dashboard). Gidin **Pano** yeni paneli görüntülemek için sayfa.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, ekleme veya Uzaktan izleme çözüm Hızlandırıcısını Web kullanıcı Arabiriminde panoları özelleştirin yardımcı olacak kaynaklar hakkında bilgi edindiniz.

Uzaktan izleme çözüm Hızlandırıcısını hakkında daha fazla kavramsal bilgi için bkz. [Uzaktan izleme mimarisi](iot-accelerators-remote-monitoring-sample-walkthrough.md).
