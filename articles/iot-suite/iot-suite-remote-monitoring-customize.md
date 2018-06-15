---
title: Uzaktan izleme çözümü UI - Azure özelleştirme | Microsoft Docs
description: Bu makalede Uzaktan izleme Çözüm Hızlandırıcısı UI için kaynak koduna erişim ve bazı özelleştirmeler nasıl hakkında bilgi sağlar.
services: iot-suite
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 01/17/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: be20d45b380f66208884f15f4644f36f2a403837
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33774059"
---
# <a name="customize-the-remote-monitoring-solution-accelerator"></a>Uzaktan izleme Çözüm Hızlandırıcısı özelleştirme

Bu makalede nasıl kaynak koduna erişim ve Uzaktan izleme Çözüm Hızlandırıcısı UI özelleştirme hakkında bilgi sağlar. Makalede açıklanır:

## <a name="prepare-a-local-development-environment-for-the-ui"></a>Kullanıcı Arabirimi yerel geliştirme ortamını hazırlayın

Uzaktan izleme Çözüm Hızlandırıcısı UI kod React.js framework kullanılarak uygulanır. Kaynak kodundaki bulabilirsiniz [azure-iot-pcs-remote-monitoring-webui](https://github.com/Azure/azure-iot-pcs-remote-monitoring-webui) GitHub depo.

UI için değişiklik yapmak için bir kopyası yerel olarak çalıştırabilirsiniz. Yerel kopyayı telemetri alma gibi eylemleri gerçekleştirmek için çözüm dağıtılan bir örneğine bağlanır.

UI geliştirme için yerel bir ortamı ayarlama işlemi aşağıdaki adımları özetlemektedir:

1. Dağıtma bir **temel** kullanarak Çözüm Hızlandırıcısı örneğini **bilgisayarları** CLI. Dağıtımınız ve sanal makine için sağlanan kimlik bilgileri adını not edin. Daha fazla bilgi için bkz: [CLI kullanarak dağıtmak](iot-suite-remote-monitoring-deploy-cli.md).

1. Azure Portalı'nı kullanın veya [az CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) çözümünüzdeki mikro barındıran sanal makinenin SSH erişimini sağlamak için. Örneğin:

    ```sh
    az network nsg rule update --name SSH --nsg-name {your solution name}-nsg --resource-group {your solution name} --access Allow
    ```

1. Azure Portalı'nı kullanın veya [az CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) adı ve sanal makinenizin ortak IP adresi bulunamıyor. Örneğin:

    ```sh
    az resource list --resource-group {your solution name} -o table
    az vm list-ip-addresses --name {your vm name from previous command} --resource-group {your solution name} -o table
    ```

1. Önceki adımda ve, çalıştırdığınızda sağladığınız kimlik bilgileri IP adresinden, sanal makineye bağlanmak için SSH kullanan **bilgisayarları** çözümü dağıtmak için.

1. Bağlanmak yerel UX izin vermek için sanal makine bash kabuğunda, aşağıdaki komutları çalıştırın:

    ```sh
    cd /app
    sudo ./start.sh --unsafe
    ```

1. Komut tamamlandıktan ve web sitesini başlatır gördükten sonra sanal makineden bağlantısını kesebilirsiniz.

1. Yerel kopyasını içinde [azure-iot-pcs-remote-monitoring-webui](https://github.com/Azure/azure-iot-pcs-remote-monitoring-webui) deposu, Düzenle **.env** dosya dağıtılan çözümünüzü URL'sini eklemek için:

    ```config
    NODE_PATH = src/
    REACT_APP_BASE_SERVICE_URL=https://{your solution name}.azurewebsites.net/
    ```

1. Bir komut isteminde yerel kopyasını `azure-iot-pcs-remote-monitoring-webui` klasörü, gerekli kitaplıkları yükleme ve kullanıcı Arabirimi yerel olarak çalıştırmak için aşağıdaki komutları çalıştırın:

    ```cmd/sh
    npm install
    npm start
    ```

1. Yerel olarak adresindeki UI önceki komutu çalıştırır http://localhost:3000/dashboard. Site çalışırken kodu düzenleme ve dinamik olarak güncelleştir görmek.

## <a name="customize-the-layout"></a>Düzenini özelleştirin

Uzaktan izleme çözümünde her bir sayfa olarak adlandırılan denetimleri kümesini oluşan *paneller* kaynak kodundaki. Örneğin, **Pano** sayfa oluşur beş bölmelerden: genel bakış, harita, uyarılar, Telemetri ve KPI'leri. Her bir sayfa ve onun paneller tanımlayan kaynak kodunu bulabilirsiniz [bilgisayarları-uzaktan-izleme-webui](https://github.com/Azure/pcs-remote-monitoring-webui) GitHub depo. Örneğin, tanımlayan kodu **Pano** sayfası, düzenini ve paneller sayfasında bulunan [src/bileşenleri/sayfalar/Pano](https://github.com/Azure/pcs-remote-monitoring-webui/tree/master/src/components/pages/dashboard) klasör.

Kendi düzeni ve boyutlandırma paneller yönetmek için kolay bir sayfanın düzenini değiştirebilirsiniz. Örneğin, için aşağıdaki değişiklikler **PageContent** öğesinde `src/components/pages/dashboard/dashboard.js` dosya Haritası ve telemetriyi panelleri konumlarını değiştirme ve KPI paneller ve harita göreli genişliğini değiştirin:

```nodejs
<PageContent className="dashboard-container" key="page-content">
  <Grid>
    <Cell className="col-1 devices-overview-cell">
      <OverviewPanel
        openWarningCount={openWarningCount}
        openCriticalCount={openCriticalCount}
        onlineDeviceCount={onlineDeviceCount}
        offlineDeviceCount={offlineDeviceCount}
        isPending={kpisIsPending || devicesIsPending}
        error={devicesError || kpisError}
        t={t} />
    </Cell>
    <Cell className="col-5">
      <TelemetryPanel
        telemetry={telemetry}
        isPending={telemetryIsPending}
        error={telemetryError}
        colors={chartColorObjects}
        t={t} />
    </Cell>
    <Cell className="col-3">
      <CustAlarmsPanel
        alarms={currentActiveAlarmsWithName}
        isPending={kpisIsPending || rulesIsPending}
        error={rulesError || kpisError}
        t={t} />
    </Cell>
    <Cell className="col-4">
    <PanelErrorBoundary msg={t('dashboard.panels.map.runtimeError')}>
        <MapPanel
          azureMapsKey={azureMapsKey}
          devices={devices}
          devicesInAlarm={devicesInAlarm}
          mapKeyIsPending={azureMapsKeyIsPending}
          isPending={devicesIsPending || kpisIsPending}
          error={azureMapsKeyError || devicesError || kpisError}
          t={t} />
      </PanelErrorBoundary>
    </Cell>
    <Cell className="col-6">
      <KpisPanel
        topAlarms={topAlarmsWithName}
        alarmsPerDeviceId={alarmsPerDeviceType}
        criticalAlarmsChange={criticalAlarmsChange}
        warningAlarmsChange={warningAlarmsChange}
        isPending={kpisIsPending || rulesIsPending || devicesIsPending}
        error={devicesError || rulesError || kpisError}
        colors={chartColorObjects}
        t={t} />
    </Cell>
  </Grid>
</PageContent>
```

![Değişiklik paneli düzeni](media/iot-suite-remote-monitoring-customize/layout.png)

> [!NOTE]
> Harita yerel dağıtımda yapılandırılmamış.

Da aynı paneli birden çok örneğini veya birden çok sürüm varsa ekleyebilirsiniz, [yinelenen ve bir panel özelleştirme](#duplicate-and-customize-an-existing-control). Aşağıdaki örnekte, iki örnek telemetri panelinin düzenleyerek eklemek gösterilmiştir `src/components/pages/dashboard/dashboard.js` dosyası:

```nodejs
<PageContent className="dashboard-container" key="page-content">
  <Grid>
    <Cell className="col-1 devices-overview-cell">
      <OverviewPanel
        openWarningCount={openWarningCount}
        openCriticalCount={openCriticalCount}
        onlineDeviceCount={onlineDeviceCount}
        offlineDeviceCount={offlineDeviceCount}
        isPending={kpisIsPending || devicesIsPending}
        error={devicesError || kpisError}
        t={t} />
    </Cell>
    <Cell className="col-3">
      <TelemetryPanel
        telemetry={telemetry}
        isPending={telemetryIsPending}
        error={telemetryError}
        colors={chartColorObjects}
        t={t} />
    </Cell>
    <Cell className="col-3">
      <TelemetryPanel
        telemetry={telemetry}
        isPending={telemetryIsPending}
        error={telemetryError}
        colors={chartColorObjects}
        t={t} />
    </Cell>
    <Cell className="col-2">
      <CustAlarmsPanel
        alarms={currentActiveAlarmsWithName}
        isPending={kpisIsPending || rulesIsPending}
        error={rulesError || kpisError}
        t={t} />
    </Cell>
    <Cell className="col-4">
    <PanelErrorBoundary msg={t('dashboard.panels.map.runtimeError')}>
        <MapPanel
          azureMapsKey={azureMapsKey}
          devices={devices}
          devicesInAlarm={devicesInAlarm}
          mapKeyIsPending={azureMapsKeyIsPending}
          isPending={devicesIsPending || kpisIsPending}
          error={azureMapsKeyError || devicesError || kpisError}
          t={t} />
      </PanelErrorBoundary>
    </Cell>
    <Cell className="col-6">
      <KpisPanel
        topAlarms={topAlarmsWithName}
        alarmsPerDeviceId={alarmsPerDeviceType}
        criticalAlarmsChange={criticalAlarmsChange}
        warningAlarmsChange={warningAlarmsChange}
        isPending={kpisIsPending || rulesIsPending || devicesIsPending}
        error={devicesError || rulesError || kpisError}
        colors={chartColorObjects}
        t={t} />
    </Cell>
  </Grid>
</PageContent>
```

Bu gibi durumlarda, farklı telemetri sonra her panelinde görüntüleyebilirsiniz:

![Birden fazla telemetri panel](media/iot-suite-remote-monitoring-customize/multiple-telemetry.png)

> [!NOTE]
> Harita yerel dağıtımda yapılandırılmamış.

## <a name="duplicate-and-customize-an-existing-control"></a>Yinelenen ve varolan bir denetim özelleştirme

Aşağıdaki adımları nasıl kullanılacağını anahat **alarmlar** Masası varolan paneli yinelenen, değiştirmek ve değiştirilmiş sürümünü kullanmak nasıl bir örnek olarak:

1. Depo, yerel kopyasında bir kopyasını **alarmlar** klasöründe `src/components/pages/dashboard/panels` klasörü. Yeni bir kopya adı **cust_alarms**.

1. İçinde **alarmsPanel.js** dosyasını **cust_alarms** klasörünü olması için sınıf adını düzenleyin **CustAlarmsPanel**:

    ```nodejs
    export class CustAlarmsPanel extends Component {
    ```

1. Aşağıdaki satırı ekleyin `src/components/pages/dashboard/panels/index.js` dosyası:

    ```nodejs
    export * from './cust_alarms';
    ```

1. Değiştir `AlarmsPanel` ile `CustAlarmsPanel` içinde `src/components/pages/dashboard/dashboard.js` dosyası:

    ```nodejs
    import {
      OverviewPanel,
      CustAlarmsPanel,
      TelemetryPanel,
      KpisPanel,
      MapPanel,
      transformTelemetryResponse,
      chartColors
    } from './panels';

    ...

    <Cell className="col-3">
      <CustAlarmsPanel
        alarms={currentActiveAlarmsWithName}
        isPending={kpisIsPending || rulesIsPending}
        error={rulesError || kpisError}
        t={t} />
    </Cell>
    ```

Özgün şimdi değiştirilen **alarmlar** adlı bir kopya paneliyle **CustAlarms**. Bu kopyalama asıl aynıdır. Şimdi kopyalama değiştirebilirsiniz. Örneğin, içinde sıralamasını değiştirmek için **alarmlar** paneli:

1. `src/components/pages/dashboard/panels/cust_alarms/alarmsPanel.js` dosyasını açın.

1. Sütun tanımları, aşağıdaki kod parçacığında gösterildiği gibi değiştirin:

    ```nodejs
    this.columnDefs = [
      rulesColumnDefs.severity,
      {
        headerName: 'rules.grid.count',
        field: 'count'
      },
      {
        ...rulesColumnDefs.ruleName,
        minWidth: 200
      },
      rulesColumnDefs.explore
    ];
    ```

Aşağıdaki ekran görüntüsü yeni sürümünü gösterir **alarmlar** paneli:

![Güncelleştirilmiş alarmlar paneli](media/iot-suite-remote-monitoring-customize/reorder-columns.png)

## <a name="customize-the-telemetry-chart"></a>Telemetri grafiği özelleştirme

Telemetri grafikte **Pano** sayfası dosyalarında tarafından tanımlanan `src/components/pages/dashboard/panels/telemtry` klasör. Kullanıcı arabirimini telemetri çözüm arka ucu alır `src/services/telemetryService.js` dosya. Aşağıdaki adımlar 5 dakika için 15 dakika telemetri grafikte görüntülenen zaman aralığını değiştirmek nasıl gösterir:

1. İçinde `src/services/telemetryService.js` dosya, çağrılan işlev bulun **getTelemetryByDeviceIdP15M**. Bu işlev bir kopyasını alın ve kopyalama gibi değiştirin:

    ```nodejs
    static getTelemetryByDeviceIdP5M(devices = []) {
      return TelemetryService.getTelemetryByMessages({
        from: 'NOW-PT5M',
        to: 'NOW',
        order: 'desc',
        devices
      });
    }
    ```

1. Bu yeni işlev telemetri grafik doldurmak için kullanılacak açmak `src/components/pages/dashboard/dashboard.js` dosya. Telemetri akışına başlatır satırı bulun ve aşağıdaki gibi değiştirin:

    ```node.js
    const getTelemetryStream = ({ deviceIds = [] }) => TelemetryService.getTelemetryByDeviceIdP5M(deviceIds)
    ```

Telemetri grafik artık telemetri verilerinin beş dakika gösterir:

![Bir gün gösteren telemetri grafiği](media/iot-suite-remote-monitoring-customize/telemetry-period.png)

## <a name="add-a-new-kpi"></a>Yeni bir KPI Ekle

**Pano** sayfasını görüntüler, KPI'ları **sistem KPI'ları** paneli. Bu KPI'ları hesaplanır `src/components/pages/dashboard/dashboard.js` dosya. KPI'ları tarafından işlenen `src/components/pages/dashboard/panels/kpis/kpisPanel.js` dosya. Aşağıdaki adımlar hesaplamak ve üzerinde yeni bir KPI değeri işlemek nasıl açıklamaktadır **Pano** sayfası. Yeni bir yüzde değişikliği uyarılar KPI eklemek için gösterilen örnek verilmiştir:

1. `src/components/pages/dashboard/dashboard.js` dosyasını açın. Değiştirme **ilk durum** eklenecek nesne bir **warningAlarmsChange** özelliğini aşağıdaki gibi:

    ```nodejs
    const initialState = {
      ...

      // Kpis data
      currentActiveAlarms: [],
      topAlarms: [],
      alarmsPerDeviceId: {},
      criticalAlarmsChange: 0,
      warningAlarmsChange: 0,
      kpisIsPending: true,
      kpisError: null,

      ...
    };
    ```

1. Değiştirme **currentAlarmsStats** eklenecek nesne **totalWarningCount** özelliği olarak:

    ```nodejs
    return {
      openWarningCount: (acc.openWarningCount || 0) + (isWarning && isOpen ? 1 : 0),
      openCriticalCount: (acc.openCriticalCount || 0) + (isCritical && isOpen ? 1 : 0),
      totalWarningCount: (acc.totalWarningCount || 0) + (isWarning ? 1 : 0),
      totalCriticalCount: (acc.totalCriticalCount || 0) + (isCritical ? 1 : 0),
      alarmsPerDeviceId: updatedAlarmsPerDeviceId
    };
    ```

1. Yeni KPI hesaplayın. Kritik uyarılar sayısı hesaplamasını bulun. Kod yinelenen ve kopyalama gibi değiştirin:

    ```nodejs
    // ================== Warning Alarms Count - START
    const currentWarningAlarms = currentAlarmsStats.totalWarningCount;
    const previousWarningAlarms = previousAlarms.reduce(
      (cnt, { severity }) => severity === 'warning' ? cnt + 1 : cnt,
      0
    );
    const warningAlarmsChange = ((currentWarningAlarms - previousWarningAlarms) / currentWarningAlarms * 100).toFixed(2);
    // ================== Warning Alarms Count - END
    ```

1. Yeni dahil **warningAlarmsChange** KPI KPI akış:

    ```nodejs
    return ({
      kpisIsPending: false,

      // Kpis data
      currentActiveAlarms,
      topAlarms,
      criticalAlarmsChange,
      warningAlarmsChange,
      alarmsPerDeviceId: currentAlarmsStats.alarmsPerDeviceId,

      ...
    });

1. Include the new **warningAlarmsChange** KPI in the state data used to render the UI:

    ```nodejs
    const {
      ...

      currentActiveAlarms,
      topAlarms,
      alarmsPerDeviceId,
      criticalAlarmsChange,
      warningAlarmsChange,
      kpisIsPending,
      kpisError,

      ...
    } = this.state;
    ```

1. KPI'ları paneline geçirilen verileri güncelleştirin:

    ```node.js
    <KpisPanel
      topAlarms={topAlarmsWithName}
      alarmsPerDeviceId={alarmsPerDeviceType}
      criticalAlarmsChange={criticalAlarmsChange}
      warningAlarmsChange={warningAlarmsChange}
      isPending={kpisIsPending || rulesIsPending || devicesIsPending}
      error={devicesError || rulesError || kpisError}
      colors={chartColorObjects}
      t={t} />
    ```

Değişiklikleri şimdi bitirdikten `src/components/pages/dashboard/dashboard.js` dosya. Aşağıdaki adımlar, yaptığınız değişiklikleri açıklamaktadır `src/components/pages/dashboard/panels/kpis/kpisPanel.js` dosya yeni KPI görüntülemek için:

1. Yeni KPI değerini şu şekilde almak için kod aşağıdaki satırı değiştirin:

    ```nodejs
    const { t, isPending, criticalAlarmsChange, warningAlarmsChange, error } = this.props;
    ```

1. Yeni KPI değeri gibi görüntülemek için biçimlendirme değiştirin:

    ```nodejs
    <div className="kpi-cell">
      <div className="kpi-header">{t('dashboard.panels.kpis.criticalAlarms')}</div>
      <div className="critical-alarms">
        {
          criticalAlarmsChange !== 0 &&
            <div className="kpi-percentage-container">
              <div className="kpi-value">{ criticalAlarmsChange }</div>
              <div className="kpi-percentage-sign">%</div>
            </div>
        }
      </div>
      <div className="kpi-header">{t('Warning alarms')}</div>
      <div className="critical-alarms">
        {
          warningAlarmsChange !== 0 &&
            <div className="kpi-percentage-container">
              <div className="kpi-value">{ warningAlarmsChange }</div>
              <div className="kpi-percentage-sign">%</div>
            </div>
        }
      </div>
    </div>
    ```

**Pano** sayfasında artık yeni KPI değeri görüntüler:

![Uyarı KPI](media/iot-suite-remote-monitoring-customize/new-kpi.png)

## <a name="customize-the-map"></a>Harita özelleştirme

Bkz: [Özelleştir harita](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide#upgrade-map-key-to-see-devices-on-a-dynamic-map) çözümündeki eşleme bileşenleri ayrıntılarını için github sayfası.

<!--
### Connect an external visualization tool

See the [Connect an external visualization tool](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/) page in GitHub for details of how to connect an external visualization tool.

-->

## <a name="other-customization-options"></a>Diğer özelleştirme seçenekleri

Daha fazla Uzaktan izleme çözümünde sunu ve görselleştirmeleri katmanını değiştirmek için kod düzenleyebilirsiniz. İlgili GitHub depolarının şunlardır:

* [Yapılandırma mikro hizmet Azure IOT çözümleri (.NET)](https://github.com/Azure/pcs-ui-config-dotnet/)
* [Azure IOT çözümleri (Java) için yapılandırma mikro hizmet](https://github.com/Azure/pcs-ui-config-java/)
* [Azure IOT bilgisayarları uzaktan Web kullanıcı Arabirimi izleme](https://github.com/Azure/pcs-remote-monitoring-webui)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede Uzaktan izleme Çözüm Hızlandırıcısı web kullanıcı Arabirimi özelleştirmenize yardımcı olacak kaynaklar hakkında öğrendiniz.

Uzaktan izleme Çözüm Hızlandırıcısı hakkında daha fazla kavramsal bilgi için bkz: [Uzaktan izleme mimarisi](iot-suite-remote-monitoring-sample-walkthrough.md)

Uzaktan izleme çözümü özelleştirme hakkında daha fazla bilgi için bkz: [özelleştirme ve yeniden dağıtın bir mikro hizmet](iot-suite-microservices-example.md)
<!-- Next tutorials in the sequence -->