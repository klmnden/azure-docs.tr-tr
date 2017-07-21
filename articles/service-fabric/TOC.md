# [Service Fabric Belgeleri](/azure/service-fabric)
# Genel Bakış
## [Service Fabric nedir?](service-fabric-overview.md)

# Hızlı Başlangıçlar
## [.NET uygulaması oluşturma](service-fabric-quickstart-dotnet.md)

# Öğreticiler
## .NET uygulaması dağıtma
### [1- .NET uygulaması oluşturma](service-fabric-tutorial-create-dotnet-app.md)
### [2- Uygulamayı dağıtma](service-fabric-tutorial-deploy-app-to-party-cluster.md)
### [3 - CI/CD yapılandırma](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)

## Bir uygulamayı lift-and-shift ile taşıma
### [1- Azure’da güvenli bir küme oluşturma](service-fabric-tutorial-create-cluster-azure-ps.md)
### [2 - Docker Compose kullanarak bir .NET uygulaması dağıtma](service-fabric-host-app-in-a-container.md)

# Örnekler
## [PowerShell](service-fabric-powershell-samples.md) 
# Kavramlar
## [Mikro hizmetleri anlama](service-fabric-overview-microservices.md)
## [Büyük resim](service-fabric-content-roadmap.md)
## [Uygulama senaryoları](service-fabric-application-scenarios.md)
## [Modeller ve senaryolar](service-fabric-patterns-and-scenarios.md)
## [Mimari](service-fabric-architecture.md)
## [Terminoloji](service-fabric-technical-overview.md)

## Uygulama ve hizmet oluşturma
### Desteklenen programlama modelleri
#### [Genel Bakış](service-fabric-choose-framework.md)
#### Kapsayıcılar
##### [Genel Bakış](service-fabric-containers-overview.md)
##### [Docker Compose (önizleme)](service-fabric-docker-compose.md)
##### [Kaynak idaresi](service-fabric-resource-governance.md)
#### Reliable Services
##### [Genel Bakış](service-fabric-reliable-services-introduction.md)
##### [Reliable Services yaşam döngüsü - C#](service-fabric-reliable-services-lifecycle.md)
##### [Reliable Services yaşam döngüsü - Java](service-fabric-reliable-services-lifecycle-java.md)
##### [Güvenilir Koleksiyonlar](service-fabric-reliable-services-reliable-collections.md)
##### [Güvenilir Koleksiyon ile ilgili kılavuzlar ve öneriler](service-fabric-reliable-services-reliable-collections-guidelines.md)
##### [Güvenilir Koleksiyonlar ile çalışma](service-fabric-work-with-reliable-collections.md)
##### [İşlemler ve kilitler](service-fabric-reliable-services-reliable-collections-transactions-locks.md)
##### [Güvenilir Eşzamanlı Sıra](service-fabric-reliable-services-reliable-concurrent-queue.md)
##### [Güvenilir Koleksiyonu serileştirme](service-fabric-reliable-services-reliable-collections-serialization.md)
##### [Güvenilir Durum Yöneticisi ve Güvenilir Koleksiyon iç işlevleri](service-fabric-reliable-services-reliable-collections-internals.md)
##### [Gelişmiş kullanım](service-fabric-reliable-services-advanced-usage.md)

#### Reliable Actors
##### [Genel Bakış](service-fabric-reliable-actors-introduction.md)
##### [Mimari](service-fabric-reliable-actors-platform.md)
##### [Yaşam döngüsü ve atık toplama](service-fabric-reliable-actors-lifecycle.md)
##### [Durum yönetimi](service-fabric-reliable-actors-state-management.md)
##### [Çok biçimlilik](service-fabric-reliable-actors-polymorphism.md)
##### [Yeniden giriş](service-fabric-reliable-actors-reentrancy.md)
##### [Türü seri hale getirme](service-fabric-reliable-actors-notes-on-actor-type-serialization.md)

### [Uygulama modeli](service-fabric-application-model.md)
### [Barındırma modeli](service-fabric-hosting-model.md)

### Hizmetler
#### [Hizmet kaynakları](service-fabric-service-manifest-resources.md)
#### [Hizmet durumu](service-fabric-concepts-state.md)
#### [Hizmet bölümleme](service-fabric-concepts-partitioning.md)
#### [Hizmetlerin kullanılabilirliği](service-fabric-availability-services.md)
#### Hizmet iletişimleri
##### [Genel Bakış](service-fabric-connect-and-communicate-with-services.md)
##### [DNS hizmeti](service-fabric-dnsservice.md)
##### [Ters proxy](service-fabric-reverseproxy.md)
##### [Güvenli iletişim için ters proxy ayarlarını yapılandırma](service-fabric-reverseproxy-configure-secure-communication.md)
### [Uygulamaların ölçeklenebilirliği](service-fabric-concepts-scalability.md)
### [ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md)

### [Uygulama kapasitesi planlama](service-fabric-capacity-planning.md)

## Uygulamaları yönetme
### [Genel Bakış](service-fabric-application-lifecycle.md)
### [ImageStoreConnectionString ayarı](service-fabric-image-store-connection-string.md)
### Uygulama yükseltme
#### [Genel Bakış](service-fabric-application-upgrade.md)
#### [Yapılandırma](service-fabric-visualstudio-configure-upgrade.md)
#### [Uygulama yükseltme parametreleri](service-fabric-application-upgrade-parameters.md)
#### [Uygulama yükseltmede verileri serileştirme](service-fabric-application-upgrade-data-serialization.md)
#### [Uygulama yükseltme ile ilgili gelişmiş konular](service-fabric-application-upgrade-advanced.md)
### [Hata analizine genel bakış](service-fabric-testability-overview.md)

## Küme oluşturma ve yönetme
### [Genel Bakış](service-fabric-deploy-anywhere.md)
### [Linux’da Service Fabric](service-fabric-linux-overview.md)
### Planlama ve hazırlama
#### [Kapasite planlaması](service-fabric-cluster-capacity.md)
#### [Olağanüstü durum kurtarma](service-fabric-disaster-recovery.md)
### [Bir kümeyi açıklama](service-fabric-cluster-resource-manager-cluster-description.md)
### [Küme güvenliği](service-fabric-cluster-security.md)
### [Linux ve Windows arasındaki özellik farkları](service-fabric-linux-windows-differences.md)
### Azure’da kümeler
#### [Düğüm türleri ve VM Ölçek Kümeleri](service-fabric-cluster-nodetypes.md)
#### [Küme ağ desenleri](service-fabric-patterns-networking.md)
### Küme kaynağı yöneticisi
#### [Genel Bakış](service-fabric-cluster-resource-manager-introduction.md)
#### [Mimari](service-fabric-cluster-resource-manager-architecture.md)
#### [Bir kümeyi tanımlama](service-fabric-cluster-resource-manager-cluster-description.md)
#### [Uygulama gruplarına genel bakış](service-fabric-cluster-resource-manager-application-groups.md)
#### [Cluster Resource Manager ayarlarını yapılandırma](service-fabric-cluster-resource-manager-configure-services.md)
#### [Kaynak tüketimi ölçümleri](service-fabric-cluster-resource-manager-metrics.md)
#### [Hizmet benzeşimi kullanımı](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md)
#### [Hizmet yerleştirme ilkeleri](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md)
#### [Küme yönetme](service-fabric-cluster-resource-manager-management-integration.md)
#### [Küme birleştirme](service-fabric-cluster-resource-manager-defragmentation-metrics.md)
#### [Küme Dengeleme](service-fabric-cluster-resource-manager-balancing.md)
#### [Azaltma](service-fabric-cluster-resource-manager-advanced-throttling.md)
#### [Hizmet taşıma](service-fabric-cluster-resource-manager-movement-cost.md)

## İzleme ve tanılama
### [Genel Bakış](service-fabric-diagnostics-overview.md)
### [Sistem durumu modeli](service-fabric-health-introduction.md)
### [Durum bilgisi olan Reliable Services özelliğinde tanılama](service-fabric-reliable-services-diagnostics.md)
### [Reliable Actors hizmetinde tanılama](service-fabric-reliable-actors-diagnostics.md)

# Nasıl Yapılır Kılavuzları
## Geliştirme ortamınızı kurma
### [Windows](service-fabric-get-started.md)
### [Linux](service-fabric-get-started-linux.md)
### [Mac OS](service-fabric-get-started-mac.md)

## Uygulama oluşturma
### Konuk yürütülebilir hizmet oluşturma
#### [Windows’ta Node.js uygulaması barındırma](quickstart-guest-app.md)
#### [Konuk tarafından yürütülebilir bir uygulama dağıtma](service-fabric-deploy-existing-app.md)
#### [Konuk tarafından yürütülebilir birden çok uygulama dağıtma](service-fabric-deploy-multiple-apps.md)
### Kapsayıcı hizmeti oluşturma
#### [Bir Windows kapsayıcı uygulaması oluşturma](service-fabric-get-started-containers.md)
#### [Bir Linux kapsayıcı uygulaması oluşturma](service-fabric-get-started-containers-linux.md)
#### [Windows kapsayıcısı dağıtma](service-fabric-deploy-container.md)
#### [Linux kapsayıcısı dağıtma](service-fabric-deploy-container-linux.md)
#### [Docker Compose (önizleme)](service-fabric-docker-compose.md)
#### [Kapsayıcılar ve hizmetler için kaynak idaresi](service-fabric-resource-governance.md)
#### [Hacim ve günlüğe kaydetme sürücüleri](service-fabric-containers-volume-logging-drivers.md)

### Reliable Services hizmeti oluşturma
#### [Genel Bakış](service-fabric-reliable-services-introduction.md)
#### Kavramlar
##### [Reliable Services yaşam döngüsü - C#](service-fabric-reliable-services-lifecycle.md)
##### [Reliable Services yaşam döngüsü - Java](service-fabric-reliable-services-lifecycle-java.md)

#### Güvenilir Koleksiyonlar
##### [Güvenilir Koleksiyonlar](service-fabric-reliable-services-reliable-collections.md)
##### [Güvenilir Koleksiyon ile ilgili kılavuzlar ve öneriler](service-fabric-reliable-services-reliable-collections-guidelines.md)
##### [Güvenilir Koleksiyonlar ile çalışma](service-fabric-work-with-reliable-collections.md)
##### [İşlemler ve kilitler](service-fabric-reliable-services-reliable-collections-transactions-locks.md)
##### [Güvenilir Eşzamanlı Sıra](service-fabric-reliable-services-reliable-concurrent-queue.md)
##### [Güvenilir Koleksiyonu serileştirme](service-fabric-reliable-services-reliable-collections-serialization.md)
##### [Güvenilir Durum Yöneticisi ve Güvenilir Koleksiyon iç işlevleri](service-fabric-reliable-services-reliable-collections-internals.md)

#### başlarken
##### [Windows üzerinde C#](service-fabric-reliable-services-quick-start.md)
##### [Linux üzerinde Java](service-fabric-reliable-services-quick-start-java.md)
##### [Linux'ta C# uygulaması oluşturma](service-fabric-create-your-first-linux-application-with-csharp.md)
#### Hizmetlerle iletişim kurma
##### [Reliable Services özelliğiyle iletişim kurma](service-fabric-reliable-services-communication.md)

##### [Uzaktan Hizmet Verme - C#](service-fabric-reliable-services-communication-remoting.md)
##### [Uzaktan Hizmet Verme - Java](service-fabric-reliable-services-communication-remoting-java.md)
##### [WCF](service-fabric-reliable-services-communication-wcf.md)
##### [Güvenli iletişim - C#](service-fabric-reliable-services-secure-communication.md)
##### [Güvenli iletişim - Java](service-fabric-reliable-services-secure-communication-java.md)

#### [Yapılandırma](service-fabric-reliable-services-configuration.md)
#### [Bildirim gönderme](service-fabric-reliable-services-notifications.md)
#### [Yedekleme ve geri yükleme](service-fabric-reliable-services-backup-restore.md)

### Reliable Actors hizmeti oluşturma
#### başlarken
##### [Windows üzerinde C#](service-fabric-reliable-actors-get-started.md)
##### [Linux üzerinde Java](service-fabric-reliable-actors-get-started-java.md)
#### [Bildirim gönderme](service-fabric-reliable-actors-events.md) 
#### [Zamanlayıcı ve anımsatıcı ayarlama](service-fabric-reliable-actors-timers-reminders.md)
#### [KvsActorStateProvider’ı yapılandırma](service-fabric-reliable-actors-kvsactorstateprovider-configuration.md)
#### [İletişim ayarlarını yapılandırma](service-fabric-reliable-actors-fabrictransportsettings.md) 
#### [ReliableDictionaryActorStateProvider’ı yapılandırma](service-fabric-reliable-actors-reliabledictionarystateprovider-configuration.md)

### [Güvenli iletişim için ters proxy ayarlarını yapılandırma](service-fabric-reverseproxy-configure-secure-communication.md)

### [Bir ASP.NET Core hizmeti oluşturma](service-fabric-add-a-web-frontend.md)

### Güvenliği yapılandırma
#### [Uygulama parolalarını yönetme](service-fabric-application-secret-management.md)  
#### [Uygulamanıza yönelik güvenlik ilkeleri yapılandırma](service-fabric-application-runas-security.md)

## Bir Windows geliştirme ortamında çalışma
### [Visual Studio'da uygulamaları yönetme](service-fabric-manage-application-in-visual-studio.md)
### [Visual Studio'da güvenli bağlantı yapılandırma](service-fabric-visualstudio-configure-secure-connections.md)
### [Uygulamanızı birden çok ortam için yapılandırma](service-fabric-manage-multiple-environment-app-configuration.md)
### [Visual Studio’da .NET hizmetinde hata ayıklama](service-fabric-debugging-your-application.md)
### [Genel hatalar ve özel durumlar](service-fabric-errors-and-exceptions.md)
### [Yerel olarak izleme ve tanılama](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

## Bir Linux geliştirme ortamında çalışma
### [Java ile geliştirme için Eclipse eklentisini kullanmaya başlama](service-fabric-get-started-eclipse.md)
### [Eclipse’te Java hizmeti hata ayıklaması](service-fabric-debugging-your-application-java.md)
### [Yerel olarak izleme ve tanılama](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)

## API Yönetimi ile tümleştirme
### [Genel Bakış](service-fabric-api-management-overview.md)
### [Hızlı başlangıç](service-fabric-api-management-quick-start.md)

## Cloud Services’tan geçiş
### [Cloud Services ile Service Fabric karşılaştırması](service-fabric-cloud-services-migration-differences.md)
### [Service Fabric’e geçme](service-fabric-cloud-services-migration-worker-role-stateless-service.md)
### [Önerilen uygulamalar](/azure/architecture/service-fabric/migrate-from-cloud-services)

## Uygulama yaşam döngüsünü yönetme
### [Uygulamaları paketleme](service-fabric-package-apps.md)

### Uygulama dağıtma veya kaldırma
#### [Yerel bir kümede uygulamaları dağıtma](service-fabric-get-started-with-a-local-cluster.md)
#### [PowerShell](service-fabric-deploy-remove-applications.md)
#### [Azure CLI 2.0](service-fabric-application-lifecycle-azure-cli-2-0.md)
#### [Visual Studio](service-fabric-publish-app-remote-cluster.md)
#### [FabricClient API’leri](service-fabric-deploy-remove-applications-fabricclient.md)

### Uygulamaları yükseltme
#### [PowerShell’i kullanarak yükseltme](service-fabric-application-upgrade-tutorial-powershell.md)
#### [Visual Studio kullanarak yükseltme](service-fabric-application-upgrade-tutorial.md)
#### [Uygulama yükseltme ile ilgili sorunları giderme](service-fabric-application-upgrade-troubleshooting.md)

### Uygulama ve hizmetleri test etme
#### [Hizmetten hizmete iletişimi test etme](service-fabric-testability-scenarios-service-communication.md)
#### Hata benzetimi yapma
##### [Denetimli chaos kullanma](service-fabric-controlled-chaos.md)
##### [Test eylemlerini kullanma](service-fabric-testability-actions.md)
##### [İş yükleri sırasında](service-fabric-testability-workload-tests.md)
##### [Test senaryolarını kullanma](service-fabric-testability-scenarios.md)
##### [Düğüm geçişi API’lerini kullanma](service-fabric-node-transition-apis.md)
#### [Uygulamanıza yük testi yapma](service-fabric-vso-load-test.md)

### Sürekli tümleştirme kurulumu
#### [VSTS ile sürekli tümleştirmeyi ayarlama](service-fabric-set-up-continuous-integration.md)
#### [Jenkins kullanarak Linux Java uygulamanızı dağıtma](service-fabric-cicd-your-linux-java-application-with-jenkins.md)

## Küme oluşturma ve yönetme
### Azure’da kümeler
#### Oluştur 
##### [Azure’da ilk kümenizi oluşturma](service-fabric-get-started-azure-cluster.md)
##### [Azure portal](service-fabric-cluster-creation-via-portal.md)
##### [Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
#### Ölçek 
##### [El ile](service-fabric-cluster-scale-up-down.md)
##### [Programlama yoluyla](service-fabric-cluster-programmatic-scaling.md)
#### [Yükseltme](service-fabric-cluster-upgrade.md)
#### [Erişim denetimini ayarlama](service-fabric-cluster-security-roles.md)
#### [Yapılandırma](service-fabric-cluster-fabric-settings.md)
#### [Küme sertifikalarını yönetme](service-fabric-cluster-security-update-certs-azure.md) 
#### [Silme](service-fabric-cluster-delete.md)

### Tek başına kümeler
#### [Dağıtımınızı planlama ve dağıtım için hazırlanma](service-fabric-cluster-standalone-deployment-preparation.md)
#### Oluştur
##### [İlk tek başına kümenizi oluşturma](service-fabric-get-started-standalone-cluster.md)
##### [Şirket içi oluşturma](service-fabric-cluster-creation-for-windows-server.md)
##### [Sertifikaları kullanarak güvenli hale getirme](service-fabric-windows-cluster-x509-security.md)  
##### [Windows güvenliğini kullanarak güvenli hale getirme](service-fabric-windows-cluster-windows-security.md)
##### [Tek başına paketin içeriği](service-fabric-cluster-standalone-package-contents.md)
#### [Ölçeklendirme](service-fabric-cluster-windows-server-add-remove-nodes.md)
#### [Erişim denetimini ayarlama](service-fabric-cluster-security-roles.md)
#### [Yapılandırma](service-fabric-cluster-manifest.md)
#### [Yükseltme](service-fabric-cluster-upgrade-windows-server.md) 

### [Küme görselleştirme](service-fabric-visualizing-your-cluster.md)
### [Güvenli bir kümeye bağlanma](service-fabric-connect-to-secure-cluster.md)

### [XPlat CLI kullanarak bir kümeyi yönetme](service-fabric-azure-cli.md)
### [Azure CLI 2.0 komutlarını kullanarak bir kümeyi yönetme](service-fabric-azure-cli-2-0.md)
### [Küme düğümleri yamalama](service-fabric-patch-orchestration-application.md)

### Küme kaynaklarını yönetme ve düzenleme
#### [Cluster Resource Manager’a genel bakış](service-fabric-cluster-resource-manager-introduction.md)
#### [Cluster Resource Manager mimarisi](service-fabric-cluster-resource-manager-architecture.md)
#### [Bir kümeyi tanımlama](service-fabric-cluster-resource-manager-cluster-description.md)
#### [Uygulama gruplarına genel bakış](service-fabric-cluster-resource-manager-application-groups.md)
#### [Cluster Resource Manager ayarlarını yapılandırma](service-fabric-cluster-resource-manager-configure-services.md)
#### [Kaynak tüketimi ölçümleri](service-fabric-cluster-resource-manager-metrics.md)
#### [Hizmet benzeşimi kullanımı](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md)
#### [Hizmet yerleştirme ilkeleri](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md)
#### [Küme yönetme](service-fabric-cluster-resource-manager-management-integration.md)
#### [Küme birleştirme](service-fabric-cluster-resource-manager-defragmentation-metrics.md)
#### [Küme Dengeleme](service-fabric-cluster-resource-manager-balancing.md)
#### [Azaltma](service-fabric-cluster-resource-manager-advanced-throttling.md)
#### [Hizmet taşıma](service-fabric-cluster-resource-manager-movement-cost.md)

## İzleme ve tanılama
### [İzleme ve tanılama uygulamaları](service-fabric-diagnostics-overview.md)
### Olay oluşturma
#### [Altyapı düzeyinde olaylar oluşturma](service-fabric-diagnostics-event-generation-infra.md)
##### [Reliable Services olayları](service-fabric-reliable-services-diagnostics.md)
##### [Reliable Actors olayları](service-fabric-reliable-actors-diagnostics.md)
##### [Performans ölçümleri](service-fabric-diagnostics-event-generation-perf.md)
#### [Uygulama düzeyinde olaylar oluşturma](service-fabric-diagnostics-event-generation-app.md)
### Uygulama ve küme sistem durumunu denetleme
#### [Service Fabric sistem durumunu izleme](service-fabric-health-introduction.md)
#### [Hizmet durumunu raporlama ve denetleme](service-fabric-diagnostics-how-to-report-and-check-service-health.md)
#### [Özel durum raporları ekleme](service-fabric-report-health.md)
#### [Sistem durum raporlarıyla ilgili sorunları giderme](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
#### [Sistem durumu raporlarını görüntüleme](service-fabric-view-entities-aggregated-health.md)
#### [Windows Server kapsayıcılarını izleme](service-fabric-diagnostics-containers-windowsserver.md)
### Olayları toplama
#### [EventFlow ile olayları toplama](service-fabric-diagnostics-event-aggregation-eventflow.md)
#### Azure Tanılama ile olayları toplama
##### [Windows](service-fabric-diagnostics-event-aggregation-wad.md)
##### [Linux](service-fabric-diagnostics-event-aggregation-lad.md)
### Olayları çözümleme
#### [Application Insights ile olayları çözümleme](service-fabric-diagnostics-event-analysis-appinsights.md)
#### [OMS ile olayları çözümleme](service-fabric-diagnostics-event-analysis-oms.md)
### [Yerel kümenizde sorun giderme](service-fabric-troubleshoot-local-cluster-setup.md)

# Başvuru
## [PowerShell (Azure)](/powershell/module/azurerm.servicefabric/)
## [PowerShell](/powershell/module/servicefabric/?view=azureservicefabricps)
## [Azure CLI](/cli/azure/sf)
## [Java API’si](/java/api/overview/azure/servicefabric)
## [.NET](/dotnet/api/overview/azure/service-fabric?view=azure-dotnet)
## [REST](/rest/api/servicefabric)

# Kaynaklar
## [Azure Yol Haritası](https://azure.microsoft.com/roadmap/)
## [Sık sorulan sorular](service-fabric-common-questions.md)
## [Öğrenme yolu](https://azure.microsoft.com/documentation/learning-paths/service-fabric/)
## [MSDN Forumu](https://social.msdn.microsoft.com/Forums/home?forum=AzureServiceFabric)
## [Fiyatlandırma](https://azure.microsoft.com/pricing/details/service-fabric/)
## [Örnek kod](http://aka.ms/servicefabricsamples)
## [Destek seçenekleri](service-fabric-support.md)
## [Hizmet Güncelleştirmeleri](https://azure.microsoft.com/updates/?product=service-fabric)
## [Videolar](https://azure.microsoft.com/documentation/videos/index/?services=service-fabric)

