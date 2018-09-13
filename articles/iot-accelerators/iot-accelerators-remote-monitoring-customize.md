---
title: Uzaktan izleme çözümü UI - Azure özelleştirme | Microsoft Docs
description: Bu makalede Uzaktan izleme çözüm Hızlandırıcısını UI için kaynak koduna erişim ve bazı özelleştirmeler nasıl hakkında bilgi sağlar.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 01/17/2018
ms.topic: conceptual
ms.openlocfilehash: 01ef5fd70b1c919c5aa2a7afbb6e46558a80b1f3
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "44717346"
---
# <a name="customize-the-remote-monitoring-solution-accelerator"></a>Uzaktan izleme çözüm Hızlandırıcısını özelleştirme

Bu makalede nasıl kaynak koduna erişim ve UI Uzaktan izleme çözüm Hızlandırıcısını özelleştirme hakkında bilgi sağlar. Bu makalede açıklanır:

## <a name="prepare-a-local-development-environment-for-the-ui"></a>Kullanıcı Arabirimi için bir yerel geliştirme ortamınızı hazırlama

Uzaktan izleme çözüm Hızlandırıcısını UI kodunu React.js framework kullanılarak uygulanır. Kaynak kodunda bulabilirsiniz [azure-iot-pcs-remote-monitoring-webui](https://github.com/Azure/azure-iot-pcs-remote-monitoring-webui) GitHub deposu.

Kullanıcı Arabirimi için değişiklik yapmak için bir kopyasını yerel olarak çalıştırabilirsiniz. Yerel kopya telemetri alma gibi eylemleri gerçekleştirmek için çözüm dağıtılan bir örneğine bağlanır.

Aşağıdaki adımlar, kullanıcı Arabirimi geliştirme için yerel bir ortamı ayarlama işlemini özetlemektedir:

1. Dağıtım bir **temel** çözüm Hızlandırıcı kullanarak örneğini **bilgisayarları** CLI. Dağıtımınız ve sanal makine için sağlanan kimlik bilgileri adını not edin. Daha fazla bilgi için [CLI kullanarak dağıtma](iot-accelerators-remote-monitoring-deploy-cli.md).

1. Azure portalını kullanın veya [az CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) çözümünüzdeki mikro hizmetleri barındıran sanal makineye SSH erişimini etkinleştirmek için. Örneğin:

    ```sh
    az network nsg rule update --name SSH --nsg-name {your solution name}-nsg --resource-group {your solution name} --access Allow
    ```

    Yalnızca, test ve geliştirme sırasında SSH erişimini etkinleştirmeniz gerekir. SSH, etkinleştirirseniz [yeniden olabildiğince çabuk devre dışı](../security/azure-security-network-security-best-practices.md#disable-rdpssh-access-to-azure-virtual-machines).

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

1. Yerel kopyanızı içinde [azure-iot-pcs-remote-monitoring-webui](https://github.com/Azure/azure-iot-pcs-remote-monitoring-webui) havuzu Düzenle **.env** dosya dağıtılan çözümünüzün URL'si eklemek için:

    ```config
    NODE_PATH = src/
    REACT_APP_BASE_SERVICE_URL=https://{your solution name}.azurewebsites.net/
    ```

1. Bir komut isteminde yerel kopyanızı `azure-iot-pcs-remote-monitoring-webui` klasörü, gerekli kitaplıkları yükleyin ve kullanıcı Arabirimi yerel olarak çalıştırmak için aşağıdaki komutları çalıştırın:

    ```cmd/sh
    npm install
    npm start
    ```

1. Önceki komutta kullanıcı Arabiriminde, yerel olarak çalışan http://localhost:3000/dashboard. Site çalışırken kod düzenleme ve dinamik olarak güncelleştir çıksın.

## <a name="customize-the-layout"></a>Düzenleri özelleştirme

Uzaktan izleme çözümünde her sayfa, bir dizi denetimi olarak adlandırılır, oluşan *panelleri* kaynak kodunda. Örneğin, **Pano** sayfa oluşur beş bölmelerden: genel bakış, harita, alarmlar, Telemetri ve KPI'ler. Her sayfanın ve kendi panellerinde tanımlayan kaynak kodunu bulabilirsiniz [bilgisayarları-remote-monitoring-webui](https://github.com/Azure/pcs-remote-monitoring-webui) GitHub deposu. Örneğin, tanımlar kod **Pano** sayfa düzenini ve panel sayfasında bulunan [src/bileşenleri/sayfaları/dashboard](https://github.com/Azure/pcs-remote-monitoring-webui/tree/master/src/components/pages/dashboard) klasör.

Çünkü kendi düzen panelleri yönetme ve boyutlandırma, bir sayfanın düzenini kolayca değiştirebilirsiniz. Aşağıdaki gibi değişir **PageContent** öğesinde `src/components/pages/dashboard/dashboard.js` dosya Haritası ve telemetriyi panellerin konumlarını değiştirme ve harita ve KPI panelleri göreli genişliğini değiştirin:

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

![Panel düzenini değiştir](./media/iot-accelerators-remote-monitoring-customize/layout.png)

> [!NOTE]
> Yerel dağıtımda eşleme yapılandırılmadı.

De aynı paneli birden çok örneğini veya birden çok sürüm varsa ekleyebilirsiniz, [yinelenen ve bir paneli özelleştirme](#duplicate-and-customize-an-existing-control). Aşağıdaki örnek, iki örnek telemetri panel düzenleyerek ekleme işlemi gösterilmektedir `src/components/pages/dashboard/dashboard.js` dosyası:

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

Ardından, farklı telemetri her panelinde görüntüleyebilirsiniz:

![Birden fazla telemetri panel](./media/iot-accelerators-remote-monitoring-customize/multiple-telemetry.png)

> [!NOTE]
> Yerel dağıtımda eşleme yapılandırılmadı.

## <a name="duplicate-and-customize-an-existing-control"></a>Yinelenen ve varolan bir denetimi özelleştirme

Aşağıdaki adımlar, nasıl kullanılacağını özetlemektedir **alarmlar** paneli var olan bir panel yinelenen, değiştirmek ve değiştirilmiş sürümünü kullanmak nasıl bir örnek olarak:

1. Depo, yerel kopyasında bir kopyasını **alarmlar** klasöründe `src/components/pages/dashboard/panels` klasör. Yeni kopya adı **cust_alarms**.

1. İçinde **alarmsPanel.js** dosyası **cust_alarms** klasöründe olması için sınıfın adını Düzenle **CustAlarmsPanel**:

    ```nodejs
    export class CustAlarmsPanel extends Component {
    ```

1. Aşağıdaki satırı ekleyin `src/components/pages/dashboard/panels/index.js` dosyası:

    ```nodejs
    export * from './cust_alarms';
    ```

1. Değiştirin `AlarmsPanel` ile `CustAlarmsPanel` içinde `src/components/pages/dashboard/dashboard.js` dosyası:

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

Artık özgün almıştır **alarmlar** adlı bir kopya paneliyle **CustAlarms**. Bu kopyası, özgün aynıdır. Artık, kopya değiştirebilirsiniz. Örneğin, sıralama sütunu değiştirme **alarmlar** paneli:

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

Yeni sürümünü aşağıdaki ekran görüntüsünde gösterilmektedir **alarmlar** paneli:

![Alarmlar panelinde güncelleştirildi](./media/iot-accelerators-remote-monitoring-customize/reorder-columns.png)

## <a name="customize-the-telemetry-chart"></a>Telemetri grafiği özelleştirme

Telemetri grafikte **Pano** sayfası dosyalarında tanımlanan `src/components/pages/dashboard/panels/telemtry` klasör. Çözüm arka ucu telemetri kullanıcı arabirimini alır `src/services/telemetryService.js` dosya. Aşağıdaki adımlarda, 5 dakika 15 dakika telemetri grafiğe görüntülenecek zaman aralığını değiştirmek gösterilmektedir:

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

1. Telemetri grafik doldurmak için bu yeni işlevi kullanmak için açık `src/components/pages/dashboard/dashboard.js` dosya. Telemetri akışına başlatır satırını bulun ve şu şekilde değiştirin:

    ```node.js
    const getTelemetryStream = ({ deviceIds = [] }) => TelemetryService.getTelemetryByDeviceIdP5M(deviceIds)
    ```

Telemetri grafik artık beş dakika telemetri verilerini gösterir:

![Bir gün gösteren telemetri grafiği](./media/iot-accelerators-remote-monitoring-customize/telemetry-period.png)

## <a name="add-a-new-kpi"></a>Yeni KPI Ekle

**Pano** sayfası görüntüler KPI'ler **sistem KPI'ları** paneli. Bu KPI'ları hesaplanır `src/components/pages/dashboard/dashboard.js` dosya. KPI'ları tarafından işlenen `src/components/pages/dashboard/panels/kpis/kpisPanel.js` dosya. Aşağıdaki adımlarda açıklanmıştır hesaplamak ve üzerinde yeni bir KPI değeri işlemek nasıl **Pano** sayfası. Yeni bir yüzde değişikliği uyarılar KPI eklemek için gösterilen örnekte bulunur:

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

1. Değiştirme **currentAlarmsStats** eklenecek nesnenin **totalWarningCount** bir özellik olarak:

    ```nodejs
    return {
      openWarningCount: (acc.openWarningCount || 0) + (isWarning && isOpen ? 1 : 0),
      openCriticalCount: (acc.openCriticalCount || 0) + (isCritical && isOpen ? 1 : 0),
      totalWarningCount: (acc.totalWarningCount || 0) + (isWarning ? 1 : 0),
      totalCriticalCount: (acc.totalCriticalCount || 0) + (isCritical ? 1 : 0),
      alarmsPerDeviceId: updatedAlarmsPerDeviceId
    };
    ```

1. Yeni KPI hesaplayın. Kritik uyarılar sayısı için hesaplama bulun. Yinelenen kodu ve kopyalama gibi değiştirin:

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

1. Yeni dahil **warningAlarmsChange** KPI stream'de KPI:

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
    ```

1. Yeni dahil **warningAlarmsChange** KPI kullanıcı arabirimini oluşturmak için kullanılan durum veri:

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

1. KPI'ler panele geçirilen verileri güncelleştirin:

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

Yapılan değişiklikler artık tamamlandı `src/components/pages/dashboard/dashboard.js` dosya. Yaptığınız değişiklik aşağıdaki adımlarda açıklanmıştır `src/components/pages/dashboard/panels/kpis/kpisPanel.js` dosyayı yeni KPI görüntülemek için:

1. Yeni KPI değeri şu şekilde almak için kod aşağıdaki satırı değiştirin:

    ```nodejs
    const { t, isPending, criticalAlarmsChange, warningAlarmsChange, error } = this.props;
    ```

1. Yeni KPI değeri şu şekilde görüntülenecek biçimlendirme değiştirin:

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

Bu makalede, Uzaktan izleme çözüm Hızlandırıcısını web kullanıcı Arabirimi özelleştirmenize yardımcı olacak kaynaklar hakkında bilgi edindiniz.

Uzaktan izleme çözüm Hızlandırıcısını hakkında daha fazla kavramsal bilgi için bkz. [Uzaktan izleme mimarisi](iot-accelerators-remote-monitoring-sample-walkthrough.md)

Uzaktan izleme çözümü özelleştirme hakkında daha fazla bilgi için bkz. [özelleştirme ve yeniden dağıtma bir mikro hizmet](iot-accelerators-microservices-example.md)
<!-- Next tutorials in the sequence -->
