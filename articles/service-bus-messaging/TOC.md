# Genel Bakış
## [Service Bus Mesajlaşması nedir?](service-bus-messaging-overview.md)
## [Service Bus ile ilgili temel bilgiler](service-bus-fundamentals-hybrid-solutions.md)
## [Service Bus mimarisi](service-bus-architecture.md)
## [SSS](service-bus-faq.md)

# Başlarken
## [Ad alanı oluşturma](service-bus-create-namespace-portal.md)
## Kuyrukları kullanma
### [.NET](service-bus-dotnet-get-started-with-queues.md)
### [Java](service-bus-java-how-to-use-queues.md)
### [Node.js](service-bus-nodejs-how-to-use-queues.md)
### [PHP](service-bus-php-how-to-use-queues.md)
### [Python](service-bus-python-how-to-use-queues.md)
### [Ruby](service-bus-ruby-how-to-use-queues.md)
### [REST](/rest/api/servicebus/queues)
## Konuları ve abonelikleri kullanma
### [.NET](service-bus-dotnet-how-to-use-topics-subscriptions.md)
### [Java](service-bus-java-how-to-use-topics-subscriptions.md)
### [Node.js](service-bus-nodejs-how-to-use-topics-subscriptions.md)
### [PHP](service-bus-php-how-to-use-topics-subscriptions.md)
### [Python](service-bus-python-how-to-use-topics-subscriptions.md)
### [Ruby](service-bus-ruby-how-to-use-topics-subscriptions.md)

# Nasıl yapılır
## Planlama ve tasarım
### [Yönetilen Hizmet Kimliği (önizleme)](service-bus-managed-service-identity.md)
### [Rol Tabanlı Erişim Denetimi (önizleme)](service-bus-role-based-access-control.md)
### [Premium mesajlaşma](service-bus-premium-messaging.md)
### [Azure Kuyrukları ve Service Bus kuyruklarını karşılaştırması](service-bus-azure-and-service-bus-queues-compared-contrasted.md)
### [Çok katmanlı Service Bus uygulaması derleme](service-bus-dotnet-multi-tier-app-using-service-bus-queues.md)
### [Performansı iyileştirme](service-bus-performance-improvements.md)
### [Coğrafi olağanüstü durum kurtarma ve coğrafi çoğaltma](service-bus-geo-dr.md)
### [Zaman uyumsuz mesajlaşma ve yüksek kullanılabilirlik](service-bus-async-messaging.md)
### [Kesintileri ve olağanüstü durumları yönetme](service-bus-outages-disasters.md)

## Geliştirme
### İleti işleme
#### [Kuyruklar, konu başlıkları ve abonelikler](service-bus-queues-topics-subscriptions.md)
#### [İletiler, yükler ve serileştirme](service-bus-messages-payloads.md)
#### [İleti aktarımları, kilitler ve kapatma](message-transfers-locks-settlement.md)
#### [İleti sıralama ve zaman damgaları](message-sequencing.md)
#### [İleti süre sonu (Yaşam Süresi)](message-expiration.md)
### [Kimlik doğrulama ve yetkilendirme](service-bus-authentication-and-authorization.md)
#### [ACS’den SAS’ye geçiş](service-bus-migrate-acs-sas.md)
#### [Paylaşılan Erişim İmzaları ile kimlik doğrulaması](service-bus-sas.md)
### [Konu başlığı filtreleri ve eylemleri](topic-filters.md)
### [Bölümlenmiş kuyruklar ve konular](service-bus-partitioning.md)
### [İleti oturumları](message-sessions.md)
### AMQP
#### [AMQP’ye genel bakış](service-bus-amqp-overview.md)
#### [.NET](service-bus-amqp-dotnet.md)
#### [Java](service-bus-amqp-java.md)
#### [Java İleti Hizmeti ve AMQP](service-bus-java-how-to-use-jms-api-amqp.md)
#### [AMQP protokol kılavuzu](service-bus-amqp-protocol-guide.md)
#### [AMQP istek-yanıt tabanlı işlemler](service-bus-amqp-request-response.md)
### Gelişmiş özellikler
#### [Teslim edilemeyen iletiler sırası](service-bus-dead-letter-queues.md)
#### [İletileri önceden getirme](service-bus-prefetch.md)
#### [Yinelenen iletileri algılama](duplicate-detection.md)
#### [İleti sayaçları](message-counters.md)
#### [İleti erteleme](message-deferral.md)
#### [İletilere gözatma](message-browsing.md)
#### [Otomatik yönlendirme ile varlıklar arasında zincir oluşturma](service-bus-auto-forwarding.md)
#### [İşlem gerçekleştirme](service-bus-transactions.md)
#### [Eşleştirilmiş ad alanı uygulama](service-bus-paired-namespaces.md)
### [Uçtan uca izleme ve tanılama](service-bus-end-to-end-tracing.md)
## Yönetme
### [Azure İzleyici ile Service Bus’ı izleme](service-bus-metrics-azure-monitor.md)
### [Service Bus yönetim kitaplıkları](service-bus-management-libraries.md)
### [Tanılama günlükleri](service-bus-diagnostic-logs.md)
### [Mesajlaşma varlıklarını askıya alma ve yeniden etkinleştirme](entity-suspend.md)
### [Azure Resource Manager şablonlarını kullanma](service-bus-resource-manager-overview.md)
#### [Ad alanı oluşturma](service-bus-resource-manager-namespace.md)
#### [Bir ad alanı ve kuyruk oluşturma](service-bus-resource-manager-namespace-queue.md)
#### [Konusu ve aboneliği olan bir ad alanı oluşturma](service-bus-resource-manager-namespace-topic.md)
#### [Ad alanı ve kuyruğa yönelik bir yetkilendirme kuralı oluşturma](service-bus-resource-manager-namespace-auth-rule.md)
#### [Konusu, aboneliği ve kuralı olan bir ad alanı oluşturma](service-bus-resource-manager-namespace-topic-with-rule.md)
#### 
### [Varlıkları sağlamak için Azure PowerShell kullanma](service-bus-manage-with-ps.md)

# Başvuru
## .NET
### [Microsoft.ServiceBus.Messaging (.NET Framework)](/dotnet/api/microsoft.servicebus.messaging)
### [Microsoft.Azure.ServiceBus (.NET Standard)](/dotnet/api/microsoft.azure.servicebus)
## [Java](/java/api/overview/azure/servicebus)
## [Azure PowerShell](/powershell/module/azurerm.servicebus)
## [REST](/rest/api/servicebus)
## [Özel durumlar](service-bus-messaging-exceptions.md)
## [Kotalar](service-bus-quotas.md)
## [SQLFilter söz dizimi](service-bus-messaging-sql-filter.md)
## [SQLRuleAction söz dizimi](service-bus-messaging-sql-rule-action.md)

# Kaynaklar
## [Azure Yol Haritası](https://azure.microsoft.com/roadmap/?category=enterprise-integration)
## [Blog](https://blogs.msdn.microsoft.com/servicebus/)
## [MSDN forumları](https://social.msdn.microsoft.com/forums/home?forum=servbus)
## [Fiyatlandırma](https://azure.microsoft.com/pricing/details/service-bus/)
## [Fiyatlandırma hesaplayıcı](https://azure.microsoft.com/pricing/calculator/)
## [Fiyatlandırma ayrıntıları](service-bus-pricing-billing.md)
## [Örnekler](service-bus-samples.md)
## [ServiceBus360](https://www.servicebus360.com/)
## [Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer)
## [Hizmet güncelleştirmeleri](https://azure.microsoft.com/updates/?product=service-bus)
## [Stack Overflow](http://stackoverflow.com/questions/tagged/azureservicebus)
## [Videolar](https://azure.microsoft.com/documentation/videos/index/?services=service-bus)


