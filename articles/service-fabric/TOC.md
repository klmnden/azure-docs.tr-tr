# [Service Fabric Belgeleri](/azure/service-fabric)
# Genel Bakış
## [Service Fabric nedir?](service-fabric-overview.md)

# Hızlı Başlangıçlar
## [.NET uygulaması oluşturma](service-fabric-quickstart-dotnet.md)
## [Bir Linux kapsayıcı uygulaması dağıtma](service-fabric-quickstart-containers-linux.md)
## [Bir Windows kapsayıcı uygulaması dağıtma](service-fabric-quickstart-containers.md)
## Java Hızlı Başlangıç Kılavuzları
### [Spring Boot uygulaması dağıtma](service-fabric-quickstart-java-spring-boot.md)
### [Reliable Services uygulaması dağıtma](service-fabric-quickstart-java-reliable-services.md)

# Öğreticiler
## .NET uygulaması dağıtma
### [1- .NET uygulaması oluşturma](service-fabric-tutorial-create-dotnet-app.md)
### [2- Uygulamayı dağıtma](service-fabric-tutorial-deploy-app-to-party-cluster.md)
### [3 - CI/CD yapılandırma](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)
### [4- İzleme ve Tanılama](service-fabric-tutorial-monitoring-aspnet.md)

## Mevcut bir .NET uygulamasını kapsayıcılı hale getirme
### [1- Docker Compose kullanarak bir .NET uygulaması dağıtma](service-fabric-host-app-in-a-container.md)
### [2- Kapsayıcınızı izleme](service-fabric-tutorial-monitoring-wincontainers.md)

## Linux kapsayıcı uygulaması oluşturma
### [1 - Kapsayıcı görüntüleri oluşturma](service-fabric-tutorial-create-container-images.md)
### [2 - Kapsayıcıları paketleme ve dağıtma](service-fabric-tutorial-package-containers.md)
### [3 - Yük devretme ve ölçeklendirme](service-fabric-tutorial-containers-failover.md)

## Kümeleri oluşturma ve yönetme
### 1 - Azure’da küme oluşturma
#### [1a- Windows kümesi oluşturma](service-fabric-tutorial-create-vnet-and-windows-cluster.md)
#### [1b- Linux kümesi oluşturma](service-fabric-tutorial-create-vnet-and-linux-cluster.md)
### [2- Kümeyi ölçeklendirme](service-fabric-tutorial-scale-cluster.md)
### [3- Çalışma zamanı kümesini yükseltme](service-fabric-tutorial-upgrade-cluster.md)
### [4- API Management’ı Service Fabric ile dağıtma](service-fabric-tutorial-deploy-api-management.md)

# Örnekler
## [Kod örnekleri](https://azure.microsoft.com/resources/samples/?service=service-fabric)
## [Azure PowerShell](service-fabric-powershell-samples.md)
## [Service Fabric CLI](samples-cli.md)

# Kavramlar
## [Mikro hizmetleri anlama](service-fabric-overview-microservices.md)
## [Büyük resim](service-fabric-content-roadmap.md)
## [Uygulama senaryoları](service-fabric-application-scenarios.md)
## [Modeller ve senaryolar](service-fabric-patterns-and-scenarios.md)
## [Mimari](service-fabric-architecture.md)
## [Terminoloji](service-fabric-technical-overview.md)

## [Desteklenen programlama modelleri](service-fabric-choose-framework.md)
### [Kapsayıcılar](service-fabric-containers-overview.md)
#### [Docker Compose (önizleme)](service-fabric-docker-compose.md)
#### [Kaynak idaresi](service-fabric-resource-governance.md)
### [Reliable Services](service-fabric-reliable-services-introduction.md)
#### [Reliable Services yaşam döngüsü - C#](service-fabric-reliable-services-lifecycle.md)
#### [Reliable Services yaşam döngüsü - Java](service-fabric-reliable-services-lifecycle-java.md)
#### [Güvenilir Koleksiyonlar](service-fabric-reliable-services-reliable-collections.md)
#### [Güvenilir Koleksiyon ile ilgili kılavuzlar ve öneriler](service-fabric-reliable-services-reliable-collections-guidelines.md)
#### [Güvenilir Koleksiyonlar ile çalışma](service-fabric-work-with-reliable-collections.md)
#### [İşlemler ve kilitler](service-fabric-reliable-services-reliable-collections-transactions-locks.md)
#### [Güvenilir Eşzamanlı Sıra](service-fabric-reliable-services-reliable-concurrent-queue.md)
#### [Güvenilir Koleksiyonu serileştirme](service-fabric-reliable-services-reliable-collections-serialization.md)
#### [Güvenilir Durum Yöneticisi ve Güvenilir Koleksiyon iç işlevleri](service-fabric-reliable-services-reliable-collections-internals.md)
#### [Gelişmiş kullanım](service-fabric-reliable-services-advanced-usage.md)

### [Reliable Actors](service-fabric-reliable-actors-introduction.md)
#### [Mimari](service-fabric-reliable-actors-platform.md)
#### [Yaşam döngüsü ve atık toplama](service-fabric-reliable-actors-lifecycle.md)
#### [Durum yönetimi](service-fabric-reliable-actors-state-management.md)
#### [Çok biçimlilik](service-fabric-reliable-actors-polymorphism.md)
#### [Yeniden giriş](service-fabric-reliable-actors-reentrancy.md)
#### [Türü seri hale getirme](service-fabric-reliable-actors-notes-on-actor-type-serialization.md)

## Uygulamalar ve hizmetler
### [Uygulama modeli](service-fabric-application-model.md)
### [Uygulama ve hizmet bildirimleri](service-fabric-application-and-service-manifests.md)
### [Barındırma modeli](service-fabric-hosting-model.md)

### [Hizmet durumu](service-fabric-concepts-state.md)
### [Hizmet bölümleme](service-fabric-concepts-partitioning.md)
### [Uygulamaların ölçeklenebilirliği](service-fabric-concepts-scalability.md)
### [Hizmetlerin kullanılabilirliği](service-fabric-availability-services.md)
### [Çoğaltma ve örnek yaşam döngüsü](service-fabric-concepts-replica-lifecycle.md)
### [Yeniden yapılandırma](service-fabric-concepts-reconfiguration.md)

### [Hizmet iletişimleri](service-fabric-connect-and-communicate-with-services.md)
#### [DNS hizmeti](service-fabric-dnsservice.md)
#### [Ters proxy](service-fabric-reverseproxy.md)
#### [Güvenli iletişim için ters proxy ayarlarını yapılandırma](service-fabric-reverseproxy-configure-secure-communication.md)
#### [Ters proxy tanılama](service-fabric-reverse-proxy-diagnostics.md)
#### [ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md)

### [Uygulama yaşam döngüsü](service-fabric-application-lifecycle.md)
#### [Uygulama yükseltme](service-fabric-application-upgrade.md)
##### [Yapılandırma](service-fabric-visualstudio-configure-upgrade.md)
##### [Uygulama yükseltme parametreleri](service-fabric-application-upgrade-parameters.md)
##### [Uygulama yükseltmede verileri serileştirme](service-fabric-application-upgrade-data-serialization.md)
##### [Uygulama yükseltme ile ilgili gelişmiş konular](service-fabric-application-upgrade-advanced.md)
#### [Birden çok ortam için uygulamaları yönetme](service-fabric-manage-multiple-environment-app-configuration.md)
#### [Hata analizi ile uygulamaları test etme](service-fabric-testability-overview.md)
#### [ImageStoreConnectionString ayarı](service-fabric-image-store-connection-string.md)

### [Hizmet kaynakları](service-fabric-service-manifest-resources.md)

## [Kümeler](service-fabric-deploy-anywhere.md)
### [Küme güvenliği](service-fabric-cluster-security.md)
### [Linux ve Windows arasındaki özellik farkları](service-fabric-linux-windows-differences.md)
### Azure’da kümeler
#### [Düğüm türleri ve VM Ölçek Kümeleri](service-fabric-cluster-nodetypes.md)
#### [Küme ağ desenleri](service-fabric-patterns-networking.md)
### [Küme kaynağı yöneticisi](service-fabric-cluster-resource-manager-introduction.md)
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
### [İzleme ve tanılama uygulamaları](service-fabric-diagnostics-overview.md)
### Olay oluşturma
#### [Platform düzeyinde etkinlikler oluşturma](service-fabric-diagnostics-event-generation-infra.md)
##### [İşlevsel kanal](service-fabric-diagnostics-event-generation-operational.md)
##### [Reliable Services olayları](service-fabric-reliable-services-diagnostics.md)
##### [Reliable Actors olayları](service-fabric-reliable-actors-diagnostics.md)
##### [Performans ölçümleri](service-fabric-diagnostics-event-generation-perf.md)
##### [Uzaktan Hizmet İletişimini izleme](service-fabric-reliable-serviceremoting-diagnostics.md)
#### [Uygulama düzeyinde olaylar oluşturma](service-fabric-diagnostics-event-generation-app.md)
### Uygulama ve küme sistem durumunu denetleme
#### [Service Fabric sistem durumunu izleme](service-fabric-health-introduction.md)
#### [Hizmet durumunu raporlama ve denetleme](service-fabric-diagnostics-how-to-report-and-check-service-health.md)
#### [Özel durum raporları ekleme](service-fabric-report-health.md)
#### [Sistem durum raporlarıyla ilgili sorunları giderme](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
#### [Sistem durumu raporlarını görüntüleme](service-fabric-view-entities-aggregated-health.md)
### Olayları toplama
#### [EventFlow ile olayları toplama](service-fabric-diagnostics-event-aggregation-eventflow.md)
#### Azure Tanılama ile olayları toplama
##### [Windows](service-fabric-diagnostics-event-aggregation-wad.md)
##### [Linux](service-fabric-diagnostics-event-aggregation-lad.md)
### Olayları çözümleme
#### [Application Insights ile olayları çözümleme](service-fabric-diagnostics-event-analysis-appinsights.md)
#### [OMS ile olayları çözümleme](service-fabric-diagnostics-event-analysis-oms.md)
### [Yerel kümenizde sorun giderme](service-fabric-troubleshoot-local-cluster-setup.md)

## [API Management ile tümleştirme](service-fabric-api-management-overview.md)

# Nasıl yapılır kılavuzları
## Geliştirme ortamınızı kurma
### [Windows](service-fabric-get-started.md)
### [Linux](service-fabric-get-started-linux.md)
### [Mac OS](service-fabric-get-started-mac.md)
### [Service Fabric CLI’sını ayarlama](service-fabric-cli.md)

## Planlama ve hazırlama
### [Küme kapasitesini planlama](service-fabric-cluster-capacity.md)
### [Tek başına küme dağıtımını planlama](service-fabric-cluster-standalone-deployment-preparation.md)
### [Olağanüstü durum kurtarmaya hazırlanma](service-fabric-disaster-recovery.md)
### [Uygulama kapasitesi planlama](service-fabric-capacity-planning.md)

## İlk kez oluşturma...
### [Visual Studio’da C# uygulaması](service-fabric-create-your-first-application-in-visual-studio.md)
### [Windows kapsayıcı uygulaması](service-fabric-get-started-containers.md)
### [Linux kapsayıcı uygulaması](service-fabric-get-started-containers-linux.md)
### [Windows’ta C# Reliable Services uygulaması](service-fabric-reliable-services-quick-start.md)
### [Linux’ta Java Reliable Services uygulaması](service-fabric-reliable-services-quick-start-java.md)
### [Linux’ta C# Reliable Services uygulaması](service-fabric-create-your-first-linux-application-with-csharp.md)
### [Windows’ta C# Reliable Actors uygulaması](service-fabric-reliable-actors-get-started.md)
### [Linux’ta Java Reliable Actors uygulaması](service-fabric-create-your-first-linux-application-with-java.md)
### [Windows’ta konuk tarafından yürütülebilir uygulama](quickstart-guest-app.md)
### [Tek başına küme](service-fabric-get-started-standalone-cluster.md)

## Uygulama oluşturma

### Konuk yürütülebilir hizmet oluşturma
#### [Konuk tarafından yürütülebilir bir uygulama dağıtma](service-fabric-deploy-existing-app.md)
#### [Konuk tarafından yürütülebilir birden çok uygulama dağıtma](service-fabric-deploy-multiple-apps.md)
### Kapsayıcı hizmeti oluşturma
#### [Kapsayıcı güvenliği](service-fabric-securing-containers.md)
#### [Docker Compose (önizleme)](service-fabric-docker-compose.md)
#### [Kapsayıcılar ve hizmetler için kaynak idaresi](service-fabric-resource-governance.md)
#### [Hacim ve günlüğe kaydetme sürücüleri](service-fabric-containers-volume-logging-drivers.md)
#### [Kapsayıcılara dahil hizmetler](service-fabric-services-inside-containers.md)
#### [Kapsayıcı ağ modları](service-fabric-networking-modes.md)

### Reliable Services hizmeti oluşturma
#### Güvenilir Koleksiyonlar
##### [Güvenilir Koleksiyonlar ile çalışma](service-fabric-work-with-reliable-collections.md)
##### [Güvenilir Eşzamanlı Sıra](service-fabric-reliable-services-reliable-concurrent-queue.md)
##### [Güvenilir Koleksiyonu serileştirme](service-fabric-reliable-services-reliable-collections-serialization.md)

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
#### [Bildirim gönderme](service-fabric-reliable-actors-events.md)
#### [Zamanlayıcı ve anımsatıcı ayarlama](service-fabric-reliable-actors-timers-reminders.md)
#### [KvsActorStateProvider’ı yapılandırma](service-fabric-reliable-actors-kvsactorstateprovider-configuration.md)
#### [İletişim ayarlarını yapılandırma](service-fabric-reliable-actors-fabrictransportsettings.md)
#### [ReliableDictionaryActorStateProvider’ı yapılandırma](service-fabric-reliable-actors-reliabledictionarystateprovider-configuration.md)

### [Maven’i desteklemek için eski Java Uygulamasını geçirme](service-fabric-migrate-old-javaapp-to-use-maven.md)

### [Güvenli iletişim için ters proxy ayarlarını yapılandırma](service-fabric-reverseproxy-configure-secure-communication.md)

### [Bir ASP.NET Core hizmeti oluşturma](service-fabric-add-a-web-frontend.md)

### Güvenliği yapılandırma
#### [Uygulama parolalarını yönetme](service-fabric-application-secret-management.md)  
#### [Uygulamanıza yönelik güvenlik ilkeleri yapılandırma](service-fabric-application-runas-security.md)

## Bir Windows/VS geliştirme ortamında çalışma
### [Visual Studio'da uygulamaları yönetme](service-fabric-manage-application-in-visual-studio.md)
### [Visual Studio'da güvenli bağlantı yapılandırma](service-fabric-visualstudio-configure-secure-connections.md)
### [Visual Studio’da .NET hizmetinde hata ayıklama](service-fabric-debugging-your-application.md)
### [Genel hatalar ve özel durumlar](service-fabric-errors-and-exceptions.md)
### [Yerel olarak izleme ve tanılama](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
### [Windows'ta Linux kümesi ayarlama](service-fabric-local-linux-cluster-windows.md)

## Bir Linux/Eclipse geliştirme ortamında çalışma
### [Java ile geliştirme için Eclipse eklentisini kullanmaya başlama](service-fabric-get-started-eclipse.md)
### [Eclipse’te Java hizmeti hata ayıklaması](service-fabric-debugging-your-application-java.md)
### [Yerel olarak izleme ve tanılama](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)

## Cloud Services’tan geçiş
### [Cloud Services ile Service Fabric karşılaştırması](service-fabric-cloud-services-migration-differences.md)
### [Service Fabric’e geçme](service-fabric-cloud-services-migration-worker-role-stateless-service.md)
### [Önerilen uygulamalar](/azure/architecture/service-fabric/migrate-from-cloud-services)

## Uygulama yaşam döngüsünü yönetme
### [Uygulamaları paketleme](service-fabric-package-apps.md)
### [Yapılandırma dosyalarıyla parametreleri kullanma](service-fabric-how-to-parameterize-configuration-files.md)
### [Parametreleri kullanarak bağlantı noktası numaralarını belirtme](service-fabric-how-to-specify-port-number-using-parameters.md)
### [Ortam değişkenlerini belirtme](service-fabric-how-to-specify-environment-variables.md)

### Uygulama dağıtma veya kaldırma
#### [Yerel bir kümede uygulamaları dağıtma](service-fabric-get-started-with-a-local-cluster.md)
#### [Azure Resource Manager](service-fabric-application-arm-resource.md)
#### [Azure PowerShell](service-fabric-deploy-remove-applications.md)
#### [Service Fabric CLI](service-fabric-application-lifecycle-sfctl.md)
#### [FabricClient API’leri](service-fabric-deploy-remove-applications-fabricclient.md)

### Uygulamaları yükseltme
#### [Azure PowerShell kullanarak yükseltme](service-fabric-application-upgrade-tutorial-powershell.md)
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

### Sürekli tümleştirme kurulumu
#### [VSTS ile sürekli tümleştirmeyi ayarlama](service-fabric-set-up-continuous-integration.md)
#### [Jenkins kullanarak Linux uygulamalarınızı dağıtma](service-fabric-cicd-your-linux-applications-with-jenkins.md)

## Küme oluşturma ve yönetme
### Azure’da kümeler
#### Oluştur
##### [Azure portalı](service-fabric-cluster-creation-via-portal.md)
##### [Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
#### Ölçek
##### [El ile](service-fabric-cluster-scale-up-down.md)
##### [Programlama yoluyla](service-fabric-cluster-programmatic-scaling.md)
#### [Yükseltme](service-fabric-cluster-upgrade.md)
#### [Erişim denetimini ayarlama](service-fabric-cluster-security-roles.md)
#### [Yapılandırma](service-fabric-cluster-fabric-settings.md)
#### [Yük dengeleyicide bağlantı noktası açma](create-load-balancer-rule.md)
#### [Küme sertifikalarını yönetme](service-fabric-cluster-security-update-certs-azure.md)
#### [Silme](service-fabric-cluster-delete.md)

### Tek başına kümeler
#### Oluştur
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
### [Küme düğümleri yamalama](service-fabric-patch-orchestration-application.md)

## İzleme ve tanılama
### OMS
#### [OMS Log Analytics’i ayarlama](service-fabric-diagnostics-oms-setup.md)
#### [OMS Aracısını ekleme](service-fabric-diagnostics-oms-agent.md)
#### [Kapsayıcıları izleme](service-fabric-diagnostics-oms-containers.md)
### Performans izleme
#### [WAD ile performans izleme](service-fabric-diagnostics-perf-wad.md)

# Başvuru
## [Azure PowerShell](/powershell/module/azurerm.servicefabric/)
## [PowerShell](/powershell/module/servicefabric/?view=azureservicefabricps)
## [Azure CLI (az sf)](/cli/azure/sf)
## [Service Fabric CLI (sfctl)](service-fabric-sfctl.md)
### [sfctl application](service-fabric-sfctl-application.md)
### [sfctl chaos](service-fabric-sfctl-chaos.md)
### [sfctl cluster](service-fabric-sfctl-cluster.md)
### [sfctl compose](service-fabric-sfctl-compose.md)
### [sfctl is](service-fabric-sfctl-is.md)
### [sfctl node](service-fabric-sfctl-node.md)
### [sfctl partition](service-fabric-sfctl-partition.md)
### [sfctl replica](service-fabric-sfctl-replica.md)
### [sfctl rpm](service-fabric-sfctl-rpm.md)
### [sfctl service](service-fabric-sfctl-service.md)
### [sfctl store](service-fabric-sfctl-store.md)
## [Java API’si](/java/api/overview/azure/servicefabric)
## [.NET](/dotnet/api/overview/azure/service-fabric?view=azure-dotnet)
## [REST](/rest/api/servicefabric)
## [Hizmet modeli XML şeması](service-fabric-service-model-schema.md)
## [Ortam değişkenleri](service-fabric-environment-variables-reference.md)

# Kaynaklar
## [Azure Yol Haritası](https://azure.microsoft.com/roadmap/)
## [Sık sorulan sorular](service-fabric-common-questions.md)
## [Öğrenme yolu](https://azure.microsoft.com/documentation/learning-paths/service-fabric/)
## [MSDN Forumu](https://social.msdn.microsoft.com/Forums/home?forum=AzureServiceFabric)
## [Fiyatlandırma](https://azure.microsoft.com/pricing/details/service-fabric/)
## [Fiyatlandırma hesaplayıcı](https://azure.microsoft.com/pricing/calculator/)
## [Örnek kod](http://aka.ms/servicefabricsamples)
## [Destek seçenekleri](service-fabric-support.md)
## [Hizmet Güncelleştirmeleri](https://azure.microsoft.com/updates/?product=service-fabric)
## [Videolar](https://azure.microsoft.com/documentation/videos/index/?services=service-fabric)