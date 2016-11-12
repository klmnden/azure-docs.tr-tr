# Genel Bakış
## [Service Fabric nedir?](service-fabric-overview.md)
## [Mikro hizmetleri anlama](service-fabric-overview-microservices.md)
## [Uygulama senaryoları](service-fabric-application-scenarios.md)
## [Mimari](service-fabric-architecture.md)

# Kullanmaya Başlama
## Geliştirme ortamınızı kurma
### [Windows](service-fabric-get-started.md)
### [Linux](service-fabric-get-started-linux.md)
### [Mac OS](service-fabric-get-started-mac.md)
## İlk uygulamanızı oluşturun
### [Windows üzerinde C#](service-fabric-create-your-first-application-in-visual-studio.md)
### [Linux üzerinde Java](service-fabric-create-your-first-linux-application-with-java.md)
### [Linux üzerinde C#](service-fabric-create-your-first-linux-application-with-csharp.md)
## [Yerel bir kümede uygulamaları dağıtma](service-fabric-get-started-with-a-local-cluster.md)

# Nasıl yapılır?
## Uygulama oluşturma
### Temel Bilgiler
#### [Programlama modeli](service-fabric-choose-framework.md)
#### [Uygulama modeli](service-fabric-application-model.md)
#### [Hizmet iletişimleri](service-fabric-connect-and-communicate-with-services.md)
#### [Araçlar](service-fabric-manage-application-in-visual-studio.md)
#### [Hata ayıklama](service-fabric-debugging-your-application.md)
#### İzleme ve tanılama
##### [Windows](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
##### [Linux](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)
#### [Uygulamanıza yönelik güvenlik ilkeleri yapılandırma](service-fabric-application-runas-security.md)
#### [Uygulamanızı birden çok ortam için yapılandırma](service-fabric-manage-multiple-environment-app-configuration.md)

### Reliable Service uygulaması
#### [Genel Bakış](service-fabric-reliable-services-introduction.md)
#### Başlarken
##### [Windows üzerinde C#](service-fabric-reliable-services-quick-start.md)
##### [Linux üzerinde Java](service-fabric-reliable-services-quick-start-java.md)
#### [Mimari](service-fabric-reliable-services-platform-architecture.md)
#### [Güvenilir Koleksiyonlar](service-fabric-reliable-services-reliable-collections.md)
#### [Reliable Collections kullanımı](service-fabric-work-with-reliable-collections.md)
#### [Yapılandırma](service-fabric-reliable-services-configuration.md)
#### [Bildirimler](service-fabric-reliable-services-notifications.md)
#### [Yedekleme ve geri yükleme](service-fabric-reliable-services-backup-restore.md)
#### [Reliable Services özelliğiyle iletişim kurma](service-fabric-reliable-services-communication.md)
##### [ASP.NET](service-fabric-reliable-services-communication-webapi.md)
##### [Uzaktan Hizmet Verme](service-fabric-reliable-services-communication-remoting.md)
##### [WCF](service-fabric-reliable-services-communication-wcf.md)
##### [Ters Proxy](service-fabric-reverseproxy.md)
#### [Gelişmiş kullanım](service-fabric-reliable-services-advanced-usage.md)

### Reliable Actor uygulaması
#### [Genel Bakış](service-fabric-reliable-actors-introduction.md)
#### Kullanmaya Başlama
##### [Windows üzerinde C#](service-fabric-reliable-actors-get-started.md)
##### [Linux üzerinde Java](service-fabric-reliable-actors-get-started-java.md)
#### [Mimari](service-fabric-reliable-actors-platform.md)
#### [Yaşam döngüsü ve atık toplama](service-fabric-reliable-actors-lifecycle.md)
#### [Çok biçimlilik](service-fabric-reliable-actors-polymorphism.md)
#### [Yeniden giriş](service-fabric-reliable-actors-reentrancy.md)
#### [Zamanlayıcılar ve anımsatıcılar](service-fabric-reliable-actors-timers-reminders.md)
#### [Olaylar](service-fabric-reliable-actors-events.md)
#### [Durum yönetimi](service-fabric-reliable-actors-state-management.md)
#### [Durum sağlayıcısı yapılandırma](service-fabric-reliable-actors-kvsactorstateprovider-configuration.md)
#### [Türü seri hale getirme](service-fabric-reliable-actors-notes-on-actor-type-serialization.md)

### Konuk tarafından yürütülebilir uygulama
#### [Konuk tarafından yürütülebilir bir uygulama dağıtma](service-fabric-deploy-existing-app.md)
#### [Konuk tarafından yürütülebilir birden çok uygulama dağıtma](service-fabric-deploy-multiple-apps.md)

### Kapsayıcı uygulaması
#### [Genel Bakış](service-fabric-containers-overview.md)
#### [Windows kapsayıcısı dağıtma](service-fabric-deploy-container.md)
#### [Docker kapsayıcısı dağıtma](service-fabric-deploy-container-linux.md)

## Cloud Services’tan geçiş
### [Cloud Services ile Service Fabric karşılaştırması](service-fabric-cloud-services-migration-differences.md)
### [Service Fabric’e geçme](service-fabric-cloud-services-migration-worker-role-stateless-service.md)

## Küme oluşturma ve yönetme

### Temel Bilgiler
#### [Genel Bakış](service-fabric-deploy-anywhere.md)
#### [Bir kümeyi tanımlama](service-fabric-cluster-resource-manager-cluster-description.md)
#### [Kapasite planlaması](service-fabric-cluster-capacity.md)
#### [Küme görselleştirme](service-fabric-visualizing-your-cluster.md)
#### [Güvenli bir kümeye bağlanma](service-fabric-connect-to-secure-cluster.md)
#### [Güvenlik](service-fabric-cluster-security.md)
#### [Olağanüstü durum kurtarma](service-fabric-disaster-recovery.md)

### Azure’da kümeler
#### Azure’da küme oluşturma
##### [Azure Portal](service-fabric-cluster-creation-via-portal.md)
##### [Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
#### [Düğüm türleri ve VM Ölçek Kümeleri](service-fabric-cluster-nodetypes.md)
#### [Küme ölçeklendirme](service-fabric-cluster-scale-up-down.md)
#### [Kümeyi yükseltme](service-fabric-cluster-upgrade.md)
#### [Küme silme](service-fabric-cluster-delete.md)
#### [Erişim denetimi](service-fabric-cluster-security-roles.md)
#### [Küme yapılandırma](service-fabric-cluster-fabric-settings.md)
#### [Bir Parti Kümesi’ni ücretsiz olarak deneme](http://aka.ms/tryservicefabric)

### Tek başına kümeler
#### [Tek başına küme oluşturma](service-fabric-cluster-creation-for-windows-server.md)
#### [Küme ölçeklendirme](service-fabric-cluster-windows-server-add-remove-nodes.md)
#### [Kümeyi yükseltme](service-fabric-cluster-upgrade-windows-server.md)
#### [Küme güvenliğini sağlama](service-fabric-windows-cluster-x509-security.md)
#### [Erişim denetimi](service-fabric-cluster-security-roles.md)
#### [Küme yapılandırma](service-fabric-cluster-manifest.md)

## Küme kaynaklarını yönetme ve düzenleme
### [Cluster Resource Manager’a genel bakış](service-fabric-cluster-resource-manager-introduction.md)
### [Cluster Resource Manager mimarisi](service-fabric-cluster-resource-manager-architecture.md)
### [Bir kümeyi tanımlama](service-fabric-cluster-resource-manager-cluster-description.md)
### [Uygulama gruplarına genel bakış](service-fabric-cluster-resource-manager-application-groups.md)
### [Cluster Resource Manager ayarlarını yapılandırma](service-fabric-cluster-resource-manager-configure-services.md)
### [Kaynak tüketimi ölçümleri](service-fabric-cluster-resource-manager-metrics.md)
### [Hizmet benzeşimi kullanımı](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md)
### [Hizmet yerleştirme ilkeleri](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md)
### [Küme yönetme](service-fabric-cluster-resource-manager-management-integration.md)
### [Küme birleştirme](service-fabric-cluster-resource-manager-defragmentation-metrics.md)
### [Küme Dengeleme](service-fabric-cluster-resource-manager-balancing.md)
### [Azaltma](service-fabric-cluster-resource-manager-advanced-throttling.md)
### [Hizmet taşıma](service-fabric-cluster-resource-manager-movement-cost.md)

## Uygulama yaşam döngüsünü yönetme
### [Genel Bakış](service-fabric-application-lifecycle.md)
### [Sürekli tümleştirme kurulumu](service-fabric-set-up-continuous-integration.md)
### Uygulama dağıtma veya kaldırma
#### [PowerShell](service-fabric-deploy-remove-applications.md)
#### [Visual Studio](service-fabric-publish-app-remote-cluster.md)
### [Uygulama yükseltmeye genel bakış](service-fabric-application-upgrade.md)
### [Uygulama yükseltmeyi yapılandırma](service-fabric-visualstudio-configure-upgrade.md)
### [Uygulama yükseltme parametreleri](service-fabric-application-upgrade-parameters.md)
### Uygulama yükseltme
#### [PowerShell](service-fabric-application-upgrade-tutorial-powershell.md)
#### [Visual Studio](service-fabric-application-upgrade-tutorial.md)
### [Uygulama yükseltme ile ilgili sorunları giderme](service-fabric-application-upgrade-troubleshooting.md)
### [Uygulama yükseltmede verileri serileştirme](service-fabric-application-upgrade-data-serialization.md)
### [Uygulama yükseltme ile ilgili gelişmiş konular](service-fabric-application-upgrade-advanced.md)
### [REST tabanlı uygulama yaşam döngüsü örneği](service-fabric-rest-based-application-lifecycle-sample.md)

## Uygulama ve küme sistem durumunu denetleme
### [Service Fabric sistem durumunu izleme](service-fabric-health-introduction.md)
### [Hizmet durumunu raporlama ve denetleme](service-fabric-diagnostics-how-to-report-and-check-service-health.md)
### [Özel durum raporları ekleme](service-fabric-report-health.md)
### [Sistem durum raporlarıyla ilgili sorunları giderme](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
### [Sistem durumu raporlarını görüntüleme](service-fabric-view-entities-aggregated-health.md)

## İzleme ve tanılama
### Hizmetleri yerel olarak izleme ve tanılama
#### [Windows](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
#### [Linux](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)
### Azure Tanılama günlükleri
#### [Windows](service-fabric-diagnostics-how-to-setup-wad.md)
#### [Linux](service-fabric-diagnostics-how-to-setup-lad.md)
### [Service Fabric uygulama izlemesi](service-fabric-diagnostic-how-to-use-elasticsearch.md)
### [Reliable Actors hizmetinde tanılama](service-fabric-reliable-actors-diagnostics.md)
### [Durum bilgisi olan Reliable Services özelliğinde tanılama](service-fabric-reliable-services-diagnostics.md)
### [Yerel kümenizde sorun giderme](service-fabric-troubleshoot-local-cluster-setup.md)
### [Yerel kümenizde sorun giderme](service-fabric-diagnostics-troubleshoot-common-scenarios.md)

## Uygulamaları ölçeklendirme
### [Reliable Services özelliğini bölümleme](service-fabric-concepts-partitioning.md)
### [Hizmetlerin kullanılabilirliği](service-fabric-availability-services.md)
### [Uygulamaları ölçeklendirme](service-fabric-concepts-scalability.md)
### [Uygulamalara yönelik kapasite planlama](service-fabric-capacity-planning.md)

## Uygulama ve hizmetleri test etme
### [Fault Analysis’e genel bakış](service-fabric-testability-overview.md)
### [Hizmetten hizmete iletişimi test etme](service-fabric-testability-scenarios-service-communication.md)
### Hata benzetimi yapma
#### [Denetimli Chaos kullanma](service-fabric-controlled-chaos.md)
#### [Test eylemlerini kullanma](service-fabric-testability-actions.md)
#### [İş yükleri sırasında](service-fabric-testability-workload-tests.md)
#### [Veri kaybı çağırarak](service-fabric-use-data-loss-api.md)
#### [Test senaryolarını kullanma](service-fabric-testability-scenarios.md)
### [Uygulamanıza yük testi yapma](service-fabric-vso-load-test.md)

# Başvuru
## [Terminoloji](service-fabric-technical-overview.md)
## [Reliable Actors](https://go.microsoft.com/fwlink/p/?linkid=833398)
## [Reliable Actors WCF](https://go.microsoft.com/fwlink/p/?linkid=833401)
## [Reliable Services](https://go.microsoft.com/fwlink/p/?linkid=833402)
## [Reliable Services WCF](https://go.microsoft.com/fwlink/p/?linkid=833403)
## [Veriler](https://go.microsoft.com/fwlink/p/?linkid=833404)
## [Veri Arabirimleri](https://go.microsoft.com/fwlink/p/?linkid=833406)
## [Sistem](https://go.microsoft.com/fwlink/p/?linkid=833407)
## [PowerShell](https://go.microsoft.com/fwlink/p/?linkid=833408)
## [REST API](https://go.microsoft.com/fwlink/p/?LinkID=532910)
## [Java API’si](https://go.microsoft.com/fwlink/p/?linkid=833410)
## [Örnek kod](http://aka.ms/servicefabricsamples)

# Kaynaklar
## [Öğrenme yolu](https://azure.microsoft.com/documentation/learning-paths/service-fabric/)
## [Hizmet Güncelleştirmeleri](https://azure.microsoft.com/updates/?product=service-fabric&updatetype=&platform=)
## [MSDN Forumu](https://social.msdn.microsoft.com/Forums/home?forum=AzureServiceFabric)


<!--HONumber=Nov16_HO2-->


