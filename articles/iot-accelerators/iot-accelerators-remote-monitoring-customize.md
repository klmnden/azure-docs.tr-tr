---
title: Uzaktan izleme çözümü UI - Azure özelleştirme | Microsoft Docs
description: Bu makalede Uzaktan izleme çözüm Hızlandırıcısını UI için kaynak koduna erişim ve bazı özelleştirmeler nasıl hakkında bilgi sağlar.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 11/09/2018
ms.topic: conceptual
ms.openlocfilehash: aed63e332375be4f8ed939cf162545c9f366f329
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66143464"
---
# <a name="customize-the-remote-monitoring-solution-accelerator"></a>Uzaktan izleme çözüm Hızlandırıcısını özelleştirme

Bu makalede nasıl kaynak koduna erişim ve UI Uzaktan izleme çözüm Hızlandırıcısını özelleştirme hakkında bilgi sağlar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prepare-a-local-development-environment-for-the-ui"></a>Kullanıcı Arabirimi için bir yerel geliştirme ortamınızı hazırlama

Uzaktan izleme çözüm Hızlandırıcısını UI kodunu React.js framework kullanılarak uygulanır. Kaynak kodunda bulabilirsiniz [azure-iot-pcs-remote-monitoring-webui](https://github.com/Azure/azure-iot-pcs-remote-monitoring-webui) GitHub deposu.

Kullanıcı Arabirimi için değişiklik yapmak için bir kopyasını yerel olarak çalıştırabilirsiniz. Telemetri alma gibi eylemleri tamamlamak için yerel kopyayı çözüm dağıtılan bir örneğine bağlanır.

Aşağıdaki adımlar, kullanıcı Arabirimi geliştirme için yerel bir ortamı ayarlama işlemini özetlemektedir:

1. Dağıtım bir **temel** çözüm Hızlandırıcı kullanarak örneğini **bilgisayarları** CLI. Dağıtımınız ve sanal makine için sağlanan kimlik bilgileri adını not edin. Daha fazla bilgi için [CLI kullanarak dağıtma](iot-accelerators-remote-monitoring-deploy-cli.md).

1. Mikro hizmetler, çözümünüzdeki barındıran sanal makineye SSH erişimini etkinleştirmek için Azure portal veya Azure Cloud Shell kullanın. Örneğin:

    ```azurecli-interactive
    az network nsg rule update --name SSH --nsg-name {your solution name}-nsg --resource-group {your solution name} --access Allow
    ```

    Yalnızca test ve geliştirme sırasında SSH erişimini etkinleştirin. SSH, etkinleştirirseniz [kullanmayı bitirdikten hemen sonra bunu devre dışı bırakmanız gerekir](../security/azure-security-network-security-best-practices.md#disable-rdpssh-access-to-virtual-machines).

1. Azure portal veya Azure Cloud Shell, sanal makinenizin genel IP adresi ve adını bulmak için kullanın. Örneğin:

    ```azurecli-interactive
    az resource list --resource-group {your solution name} -o table
    az vm list-ip-addresses --name {your vm name from previous command} --resource-group {your solution name} -o table
    ```

1. Sanal makinenize bağlanmak için SSH kullanın. IP adresi önceki adımda ve çalıştırdığınızda sağladığınız kimlik bilgileri kullanmak **bilgisayarları** çözümü dağıtmak için. `ssh` Komutu Azure Cloud Shell'de kullanılabilir.

1. Bağlanmak yerel kullanıcı Deneyimini izin vermek için sanal makine bash kabuğunda aşağıdaki komutları çalıştırın:

    ```sh
    cd /app
    sudo ./start.sh --unsafe
    ```

1. Komut tamamlandıktan ve web sitesini başlatır gördükten sonra sanal makineden kesebilirsiniz.

1. Yerel kopyanızı içinde [azure-iot-pcs-remote-monitoring-webui](https://github.com/Azure/azure-iot-pcs-remote-monitoring-webui) havuzu Düzenle **.env** dosya dağıtılan çözümünüzün URL'si eklemek için:

    ```config
    NODE_PATH = src/
    REACT_APP_BASE_SERVICE_URL=https://{your solution name}.azurewebsites.net/
    ```

1. Bir komut isteminde, yerel kopyasına gidin `azure-iot-pcs-remote-monitoring-webui` klasör.

1. Gerekli kitaplıkları yükleyin ve kullanıcı Arabirimi yerel olarak çalıştırmak için aşağıdaki komutları çalıştırın:

    ```cmd/sh
    npm install
    npm start
    ```

1. Önceki komutu kullanıcı Arabirimi yerel olarak http çalıştırır:\//localhost:3000 / Pano. Site çalışırken kod düzenleme ve dinamik olarak güncelleştir çıksın.

## <a name="customize-the-layout"></a>Düzenleri özelleştirme

Uzaktan izleme çözümünde her sayfa, bir dizi denetimi olarak adlandırılır, oluşan *panelleri* kaynak kodunda. **Pano** sayfa oluşur beş bölmelerden: Genel bakış, harita, uyarılar, Telemetri ve analiz. Her sayfanın ve kendi panellerinde tanımlayan kaynak kodunu bulabilirsiniz [bilgisayarları-remote-monitoring-webui](https://github.com/Azure/pcs-remote-monitoring-webui) GitHub deposu. Örneğin, tanımlar kod **Pano** sayfa düzenini ve panel sayfasında bulunan [src/bileşenleri/sayfaları/dashboard](https://github.com/Azure/pcs-remote-monitoring-webui/tree/master/src/components/pages/dashboard) klasör.

Çünkü kendi düzen panelleri yönetme ve boyutlandırma, bir sayfanın düzenini kolayca değiştirebilirsiniz. Aşağıdaki değişiklikleri yapın **PageContent** öğesinde `src/components/pages/dashboard/dashboard.js` dosya:

* Haritası ve telemetriyi panellerin konumlarını değiştirme.
* Harita ve analiz paneller göreli genişliğini değiştirin.

```javascript
<PageContent className="dashboard-container">
  <Grid>
    <Cell className="col-1 devices-overview-cell">
      <OverviewPanel
        activeDeviceGroup={activeDeviceGroup}
        openWarningCount={openWarningCount}
        openCriticalCount={openCriticalCount}
        onlineDeviceCount={onlineDeviceCount}
        offlineDeviceCount={offlineDeviceCount}
        isPending={analyticsIsPending || devicesIsPending}
        error={deviceGroupError || devicesError || analyticsError}
        t={t} />
    </Cell>
    <Cell className="col-6">
      <TelemetryPanel
        timeSeriesExplorerUrl={timeSeriesParamUrl}
        telemetry={telemetry}
        isPending={telemetryIsPending}
        lastRefreshed={lastRefreshed}
        error={deviceGroupError || telemetryError}
        theme={theme}
        colors={chartColorObjects}
        t={t} />
    </Cell>
    <Cell className="col-3">
      <AlertsPanel
        alerts={currentActiveAlertsWithName}
        isPending={analyticsIsPending || rulesIsPending}
        error={rulesError || analyticsError}
        t={t}
        deviceGroups={deviceGroups} />
    </Cell>
    <Cell className="col-4">
      <PanelErrorBoundary msg={t('dashboard.panels.map.runtimeError')}>
        <MapPanel
          analyticsVersion={analyticsVersion}
          azureMapsKey={azureMapsKey}
          devices={devices}
          devicesInAlert={devicesInAlert}
          mapKeyIsPending={azureMapsKeyIsPending}
          isPending={devicesIsPending || analyticsIsPending}
          error={azureMapsKeyError || devicesError || analyticsError}
          t={t} />
      </PanelErrorBoundary>
    </Cell>
    <Cell className="col-6">
      <AnalyticsPanel
        timeSeriesExplorerUrl={timeSeriesParamUrl}
        topAlerts={topAlertsWithName}
        alertsPerDeviceId={alertsPerDeviceType}
        criticalAlertsChange={criticalAlertsChange}
        isPending={analyticsIsPending || rulesIsPending || devicesIsPending}
        error={devicesError || rulesError || analyticsError}
        theme={theme}
        colors={chartColorObjects}
        t={t} />
    </Cell>
    {
      Config.showWalkthroughExamples &&
      <Cell className="col-4">
        <ExamplePanel t={t} />
      </Cell>
    }
  </Grid>
</PageContent>
```

![Panel düzenini değiştir](./media/iot-accelerators-remote-monitoring-customize/layout.png)

De aynı paneli birkaç örnek veya çeşitli sürümleri varsa ekleyebilirsiniz, [yinelenen ve bir paneli özelleştirme](#duplicate-and-customize-an-existing-control). Aşağıdaki örnek iki örnek telemetri panel Ekle gösterilmektedir. Bu değişiklikleri yapmak için Düzenle `src/components/pages/dashboard/dashboard.js` dosyası:

```javascript
<PageContent className="dashboard-container">
  <Grid>
    <Cell className="col-1 devices-overview-cell">
      <OverviewPanel
        activeDeviceGroup={activeDeviceGroup}
        openWarningCount={openWarningCount}
        openCriticalCount={openCriticalCount}
        onlineDeviceCount={onlineDeviceCount}
        offlineDeviceCount={offlineDeviceCount}
        isPending={analyticsIsPending || devicesIsPending}
        error={deviceGroupError || devicesError || analyticsError}
        t={t} />
    </Cell>
    <Cell className="col-3">
      <TelemetryPanel
        timeSeriesExplorerUrl={timeSeriesParamUrl}
        telemetry={telemetry}
        isPending={telemetryIsPending}
        lastRefreshed={lastRefreshed}
        error={deviceGroupError || telemetryError}
        theme={theme}
        colors={chartColorObjects}
        t={t} />
    </Cell>
    <Cell className="col-3">
      <TelemetryPanel
        timeSeriesExplorerUrl={timeSeriesParamUrl}
        telemetry={telemetry}
        isPending={telemetryIsPending}
        lastRefreshed={lastRefreshed}
        error={deviceGroupError || telemetryError}
        theme={theme}
        colors={chartColorObjects}
        t={t} />
    </Cell>
    <Cell className="col-3">
      <AlertsPanel
        alerts={currentActiveAlertsWithName}
        isPending={analyticsIsPending || rulesIsPending}
        error={rulesError || analyticsError}
        t={t}
        deviceGroups={deviceGroups} />
    </Cell>
    <Cell className="col-4">
      <PanelErrorBoundary msg={t('dashboard.panels.map.runtimeError')}>
        <MapPanel
          analyticsVersion={analyticsVersion}
          azureMapsKey={azureMapsKey}
          devices={devices}
          devicesInAlert={devicesInAlert}
          mapKeyIsPending={azureMapsKeyIsPending}
          isPending={devicesIsPending || analyticsIsPending}
          error={azureMapsKeyError || devicesError || analyticsError}
          t={t} />
      </PanelErrorBoundary>
    </Cell>
    <Cell className="col-6">
      <AnalyticsPanel
        timeSeriesExplorerUrl={timeSeriesParamUrl}
        topAlerts={topAlertsWithName}
        alertsPerDeviceId={alertsPerDeviceType}
        criticalAlertsChange={criticalAlertsChange}
        isPending={analyticsIsPending || rulesIsPending || devicesIsPending}
        error={devicesError || rulesError || analyticsError}
        theme={theme}
        colors={chartColorObjects}
        t={t} />
    </Cell>
    {
      Config.showWalkthroughExamples &&
      <Cell className="col-4">
        <ExamplePanel t={t} />
      </Cell>
    }
  </Grid>
</PageContent>
```

Ardından, farklı telemetri her panelinde görüntüleyebilirsiniz:

![Birden fazla telemetri panel](./media/iot-accelerators-remote-monitoring-customize/multiple-telemetry.png)

## <a name="duplicate-and-customize-an-existing-control"></a>Yinelenen ve varolan bir denetimi özelleştirme

Aşağıdaki adımlar, var olan bir panel yinelenen, değiştirin ve ardından değiştirilmiş sürümünü nasıl özetlemektedir. Adımları **uyarılar** paneli örnek olarak:

1. Depo, yerel kopyasında bir kopyasını **uyarılar** klasöründe `src/components/pages/dashboard/panels` klasör. Yeni kopya adı **cust_alerts**.

1. İçinde **alertsPanel.js** dosyası **cust_alerts** klasöründe olması için sınıfın adını Düzenle **CustAlertsPanel**:

    ```javascript
    export class CustAlertsPanel extends Component {
    ```

1. Aşağıdaki satırı ekleyin `src/components/pages/dashboard/panels/index.js` dosyası:

    ```javascript
    export * from './cust_alerts';
    ```

1. Değiştirin `alertsPanel` ile `CustAlertsPanel` içinde `src/components/pages/dashboard/dashboard.js` dosyası:

    ```javascript
    import {
      OverviewPanel,
      CustAlertsPanel,
      TelemetryPanel,
      KpisPanel,
      MapPanel,
      transformTelemetryResponse,
      chartColors
    } from './panels';

    ...

    <Cell className="col-3">
      <CustAlertsPanel
        alerts={currentActivealertsWithName}
        isPending={kpisIsPending || rulesIsPending}
        error={rulesError || kpisError}
        t={t} />
    </Cell>
    ```

Artık özgün yerine **uyarılar** adlı bir kopya paneliyle **CustAlerts**. Bu kopyası, özgün ile aynıdır. Artık, kopya değiştirebilirsiniz. Örneğin, sıralama sütunu değiştirme **uyarılar** paneli:

1. `src/components/pages/dashboard/panels/cust_alerts/alertsPanel.js` dosyasını açın.

1. Sütun tanımları, aşağıdaki kod parçacığında gösterildiği gibi değiştirin:

    ```javascript
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

Yeni sürümünü aşağıdaki ekran görüntüsünde gösterilmektedir **uyarılar** paneli:

![Uyarılar panelinde güncelleştirildi](./media/iot-accelerators-remote-monitoring-customize/reorder-columns.png)

## <a name="customize-the-telemetry-chart"></a>Telemetri grafiği özelleştirme

Dosyaları `src/components/pages/dashboard/panels/telemtry` klasör üzerinde telemetri grafik tanımlayın **Pano** sayfası. Çözüm arka ucu telemetri kullanıcı arabirimini alır `src/services/telemetryService.js` dosya. Aşağıdaki adımlarda, 5 dakika 15 telemetri grafiğe görüntülenecek zaman aralığını değiştirmek gösterilmektedir:

1. İçinde `src/services/telemetryService.js` dosya, çağrılan işlev bulun **getTelemetryByDeviceIdP15M**. Bu işlev bir kopyasını alın ve kopyalama gibi değiştirin:

    ```javascript
    static getTelemetryByDeviceIdP5M(devices = []) {
      return TelemetryService.getTelemetryByMessages({
        from: 'NOW-PT5M',
        to: 'NOW',
        order: 'desc',
        devices
      });
    }
    ```

1. Telemetri grafik doldurmak için bu yeni işlevi kullanmak için açık `src/components/pages/dashboard/dashboard.js` dosya. Telemetri akışına başlatır satırını bulun ve şu şekilde değiştirin:

    ```javascript
    const getTelemetryStream = ({ deviceIds = [] }) => TelemetryService.getTelemetryByDeviceIdP5M(deviceIds)
    ```

Telemetri grafik artık beş dakika telemetri verilerini gösterir:

![Bir gün gösteren telemetri grafiği](./media/iot-accelerators-remote-monitoring-customize/telemetry-period.png)

## <a name="add-a-new-kpi"></a>Yeni KPI Ekle

**Pano** sayfası görüntüler KPI'ler **Analytics** paneli. Bu KPI'ları hesaplanır `src/components/pages/dashboard/dashboard.js` dosya. KPI'ları tarafından işlenen `src/components/pages/dashboard/panels/analytics/analyticsPanel.js` dosya. Aşağıdaki adımlarda açıklanmıştır hesaplamak ve üzerinde yeni bir KPI değeri işlemek nasıl **Pano** sayfası. Yeni bir yüzde değişikliği uyarı bildirimleri KPI eklemek için gösterilen örnekte bulunur:

1. `src/components/pages/dashboard/dashboard.js` dosyasını açın. Değiştirme **ilk durum** eklenecek nesne bir **warningAlertsChange** özelliğini aşağıdaki gibi:

    ```javascript
    const initialState = {
      ...

      // Analytics data
      analyticsVersion: 0,
      currentActiveAlerts: [],
      topAlerts: [],
      alertsPerDeviceId: {},
      criticalAlertsChange: 0,
      warningAlertsChange: 0,
      analyticsIsPending: true,
      analyticsError: null

      ...
    };
    ```

1. Değiştirme **currentAlertsStats** eklenecek nesnenin **totalWarningCount** bir özellik olarak:

    ```javascript
    return {
      openWarningCount: (acc.openWarningCount || 0) + (isWarning && isOpen ? 1 : 0),
      openCriticalCount: (acc.openCriticalCount || 0) + (isCritical && isOpen ? 1 : 0),
      totalWarningCount: (acc.totalWarningCount || 0) + (isWarning ? 1 : 0),
      totalCriticalCount: (acc.totalCriticalCount || 0) + (isCritical ? 1 : 0),
      alertsPerDeviceId: updatedAlertsPerDeviceId
    };
    ```

1. Yeni KPI hesaplayın. Hesaplama için kritik uyarı sayısını bulur. Yinelenen kodu ve kopyalama gibi değiştirin:

    ```javascript
    // ================== Warning Alerts Count - START
    const currentWarningAlerts = currentAlertsStats.totalWarningCount;
    const previousWarningAlerts = previousAlerts.reduce(
      (cnt, { severity }) => severity === Config.ruleSeverity.warning ? cnt + 1 : cnt,
      0
    );
    const warningAlertsChange = ((currentWarningAlerts - previousWarningAlerts) / currentWarningAlerts * 100).toFixed(2);
    // ================== Warning Alerts Count - END
    ```

1. Yeni dahil **warningAlertsChange** KPI stream'de KPI:

    ```javascript
    return ({
      analyticsIsPending: false,
      analyticsVersion: this.state.analyticsVersion + 1,

      // Analytics data
      currentActiveAlerts,
      topAlerts,
      criticalAlertsChange,
      warningAlertsChange,
      alertsPerDeviceId: currentAlertsStats.alertsPerDeviceId,

      ...
    });
    ```

1. Yeni dahil **warningAlertsChange** KPI kullanıcı arabirimini oluşturmak için kullanılan durum veri:

    ```javascript
    const {
      ...

      analyticsVersion,
      currentActiveAlerts,
      topAlerts,
      alertsPerDeviceId,
      criticalAlertsChange,
      warningAlertsChange,
      analyticsIsPending,
      analyticsError,

      ...
    } = this.state;
    ```

1. KPI'ler panele geçirilen verileri güncelleştirin:

    ```javascript
    <AnalyticsPanel
      timeSeriesExplorerUrl={timeSeriesParamUrl}
      topAlerts={topAlertsWithName}
      alertsPerDeviceId={alertsPerDeviceType}
      criticalAlertsChange={criticalAlertsChange}
      warningAlertsChange={warningAlertsChange}
      isPending={analyticsIsPending || rulesIsPending || devicesIsPending}
      error={devicesError || rulesError || analyticsError}
      theme={theme}
      colors={chartColorObjects}
      t={t} />
    ```

Yapılan değişiklikler artık bitirdikten `src/components/pages/dashboard/dashboard.js` dosya. Yaptığınız değişiklik aşağıdaki adımlarda açıklanmıştır `src/components/pages/dashboard/panels/analytics/analyticsPanel.js` dosyayı yeni KPI görüntülemek için:

1. Yeni KPI değeri şu şekilde almak için kod aşağıdaki satırı değiştirin:

    ```javascript
    const { t, isPending, criticalAlertsChange, warningAlertsChange, alertsPerDeviceId, topAlerts, timeSeriesExplorerUrl, error } = this.props;
    ```

1. Yeni KPI değeri şu şekilde görüntülenecek biçimlendirme değiştirin:

    ```javascript
    <div className="analytics-cell">
      <div className="analytics-header">{t('dashboard.panels.analytics.criticalAlerts')}</div>
      <div className="critical-alerts">
        {
          !showOverlay &&
            <div className="analytics-percentage-container">
              <div className="analytics-value">{ !isNaN(criticalAlertsChange) ? criticalAlertsChange : 0 }</div>
              <div className="analytics-percentage-sign">%</div>
            </div>
        }
      </div>
      <div className="critical-alerts">
        {
          !showOverlay &&
            <div className="analytics-percentage-container">
              <div className="analytics-value">{ !isNaN(warningAlertsChange) ? warningAlertsChange : 0 }</div>
              <div className="analytics-percentage-sign">%</div>
            </div>
        }
      </div>
    </div>
    ```

**Pano** sayfasında artık yeni KPI değeri görüntüler:

![Uyarı KPI](./media/iot-accelerators-remote-monitoring-customize/new-kpi.png)

## <a name="customize-the-map"></a>Haritayı özelleştirme

Bkz: [Özelleştir harita](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide#upgrade-map-key-to-see-devices-on-a-dynamic-map) GitHub sayfasında çözüm içinde eşleme bileşenlerini hakkında ayrıntılar için.

<!--
### Connect an external visualization tool

See the [Connect an external visualization tool](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/) page in GitHub for details of how to connect an external visualization tool.

-->

## <a name="other-customization-options"></a>Diğer özelleştirme seçeneklerinin

Daha fazla Uzaktan izleme çözümünde sunu ve görselleştirmeler katmanını değiştirmek için kodu yeniden düzenleyebilirsiniz. İlgili GitHub depoları şunlardır:

* [Yapılandırma mikro hizmet için Azure IOT çözümleri (.NET)](https://github.com/Azure/remote-monitoring-services-dotnet/tree/master/config)
* [Yapılandırma mikro hizmet Azure IOT çözümleri (Java)](https://github.com/Azure/remote-monitoring-services-java/tree/master/config)
* [Azure IOT bilgisayarları uzaktan Web UI'ı izleme](https://github.com/Azure/pcs-remote-monitoring-webui)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Uzaktan izleme çözüm Hızlandırıcısını web kullanıcı Arabirimi özelleştirmenize yardımcı olacak kaynaklar hakkında bilgi edindiniz. Kullanıcı arabirimini özelleştirme hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Uzaktan izleme çözüm Hızlandırıcı web kullanıcı Arabirimine özel sayfa ekleme](iot-accelerators-remote-monitoring-customize-page.md)
* [Uzaktan izleme çözüm Hızlandırıcı web kullanıcı Arabirimine özel hizmet ekleme](iot-accelerators-remote-monitoring-customize-service.md)
* [Uzaktan izleme çözüm Hızlandırıcı web kullanıcı Arabirimine özel kılavuz ekleme](iot-accelerators-remote-monitoring-customize-grid.md)
* [Uzaktan izleme çözüm Hızlandırıcı web kullanıcı Arabirimine özel bir açılır öğesi Ekle](iot-accelerators-remote-monitoring-customize-flyout.md)
* [Uzaktan izleme çözüm Hızlandırıcı Web kullanıcı Arabiriminde panoya özel panel ekleme](iot-accelerators-remote-monitoring-customize-panel.md)

Uzaktan izleme çözüm Hızlandırıcısını hakkında daha fazla kavramsal bilgi için bkz. [Uzaktan izleme mimarisi](iot-accelerators-remote-monitoring-sample-walkthrough.md)

Uzaktan izleme çözümü mikro hizmetler özelleştirme hakkında daha fazla bilgi için bkz. [özelleştirme ve yeniden dağıtma bir mikro hizmet](iot-accelerators-microservices-example.md).
<!-- Next tutorials in the sequence -->
