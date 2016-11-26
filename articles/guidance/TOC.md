# Mimari

## Bulut ile ilgili Temel Bilgiler

### [Dayanıklı uygulamalar tasarlama](guidance-resiliency-overview.md)
#### [Dayanıklılık denetim listesi](guidance-resiliency-checklist.md)
#### [Hata modu analizi](guidance-resiliency-failure-mode-analysis.md)
#### [Olağanüstü durum kurtarma](..\resiliency\resiliency-disaster-recovery-azure-applications.md)
#### [Olağanüstü durum kurtarma ve yüksek kullanılabilirlik](..\resiliency\resiliency-disaster-recovery-high-availability-azure-applications.md)
#### [Yüksek kullanılabilirlik](..\resiliency\resiliency-high-availability-azure-applications.md)
#### [Yüksek kullanılabilirlik denetim listesi](..\resiliency\resiliency-high-availability-checklist.md)
#### [Azure hizmete özgü dayanıklılık kılavuzu](..\resiliency\resiliency-service-guidance-index.md)
#### [Verilerin bozulması veya yanlışlıkla silinmesi durumunda kurtarma](..\resiliency\resiliency-technical-guidance-recovery-data-corruption.md)
#### [Yerel hatalardan kurtarma](..\resiliency\resiliency-technical-guidance-recovery-local-failures.md)
#### [Bölge genelinde hizmet kesintisinden kurtarma](..\resiliency\resiliency-technical-guidance-recovery-loss-azure-region.md)
#### [Şirket içinden Azure’a kurtarma](..\resiliency\resiliency-technical-guidance-recovery-on-premises-azure.md)
#### [Azure dayanıklılık teknik kılavuzu](..\resiliency\resiliency-technical-guidance.md)


## Başvuru Mimarileri

### [Azure'da VM iş yüklerini çalıştırma](guidance-ra-compute.md)
#### [Azure’da Linux VM çalıştırma](guidance-compute-single-vm-linux.md)
#### [Azure’da Windows VM çalıştırma](guidance-compute-single-vm.md)
#### [Ölçeklenebilirlik ve kullanılabilirlik için Azure’da birden fazla VM çalıştırma](guidance-compute-multi-vm.md)
#### [N katmanlı mimari için Linux VM’leri çalıştırma](guidance-compute-n-tier-vm-linux.md)
#### [N katmanlı mimari için Windows VM’leri çalıştırma](guidance-compute-n-tier-vm.md)
#### [Yüksek kullanılabilirlik için birden fazla bölgede Linux VM’leri çalıştırma](guidance-compute-multiple-datacenters-linux.md)
#### [Yüksek kullanılabilirlik için birden fazla bölgede Windows VM’leri çalıştırma](guidance-compute-multiple-datacenters.md)

### [Şirket içi ağınızı Azure’a bağlama](guidance-ra-hybrid-networking.md)
#### [Azure ExpressRoute ile karma ağ mimarisi](guidance-hybrid-network-expressroute.md)
#### [Azure ve Şirket İçi VPN ile Karma ağ mimarisi](guidance-hybrid-network-vpn.md)
#### [Yüksek oranda kullanılabilir karma ağ mimarisi](guidance-hybrid-network-expressroute-vpn-failover.md)

### [Azure’da Bulut Sınırını Koruma](guidance-ra-network-security.md)
#### [Azure’da güvenli karma ağ mimarisi](guidance-iaas-ra-secure-vnet-hybrid.md)
#### [Azure ve İnternet arasında DMZ](guidance-iaas-ra-secure-vnet-dmz.md)

### [Azure’da kimlik yönetimi](guidance-ra-identity.md)
#### [Azure Active Directory'yi uygulama](guidance-identity-aad.md)
#### [ADDS’i Azure’a genişletme](guidance-identity-adds-extend-domain.md)
#### [Azure’da ADDS kaynak ormanı oluşturma](guidance-identity-adds-resource-forest.md)
#### [Azure’da ADFS’yi uygulama](guidance-identity-adfs.md)

### [Azure Uygulama Hizmeti için Web uygulaması mimarileri](guidance-ra-app-service.md)
#### [Temel web uygulaması](guidance-web-apps-basic.md)
#### [Yüksek kullanılabilirliğe sahip web uygulaması](guidance-web-apps-multi-region.md)
#### [Web uygulamasında ölçeklenebilirliği geliştirme](guidance-web-apps-scalability.md)


## Bulut Uygulamalarına yönelik En İyi Yöntemler

### [API tasarım kılavuzu](..\best-practices-api-design.md)
### [API uygulama kılavuzu](..\best-practices-api-implementation.md)
### [Otomatik ölçeklendirme kılavuzu](..\best-practices-auto-scaling.md)
### [Kullanılabilirlik denetim listesi](..\best-practices-availability-checklist.md)
### [Arka plan işleri kılavuzu](..\best-practices-background-jobs.md)
### [Azure Eşleştirilmiş Bölgeleri](..\best-practices-availability-paired-regions.md)
### [Önbelleğe alma kılavuzu](..\best-practices-caching.md)
### [İçerik Teslim Ağı kılavuzu](..\best-practices-cdn.md)
### [Veri bölümleme kılavuzu](..\best-practices-data-partitioning.md)
### [İzleme ve tanılama kılavuzu](..\best-practices-monitoring.md)
### [Microsoft bulut hizmetleri ve ağ güvenliği](..\best-practices-network-security.md)
### [Azure Resource Manager şablonları için tasarım desenleri](..\best-practices-resource-manager-design-templates.md)
### [Azure kaynakları için önerilen adlandırma kuralları](guidance-naming-conventions.md)
### [Azure Resource Manager’da güvenlikle ilgili dikkat edilmesi gerekenler](..\best-practices-resource-manager-security.md)
### [Azure Resource Manager şablonlarında durum paylaşımı](..\best-practices-resource-manager-state.md)
### [Yeniden deneme genel kılavuzu](..\best-practices-retry-general.md)
### [Yeniden deneme hizmeti özel kılavuzu](..\best-practices-retry-service-specific.md)
### [Ölçeklenebilirlik denetim listesi](..\best-practices-scalability-checklist.md)


## Senaryo Kılavuzları

### [Azure’da Elasticsearch Kılavuzu](guidance-elasticsearch.md)
#### [Azure’da Elasticsearch’ü çalıştırma](guidance-elasticsearch-running-on-azure.md)
#### [Veri alımı performansını ayarlama](guidance-elasticsearch-tuning-data-ingestion-performance.md)
#### [Veri toplama ve sorgu performansını ayarlama](guidance-elasticsearch-tuning-data-aggregation-and-query-performance.md)
#### [Esnekliği ve kurtarmayı yapılandırma](guidance-elasticsearch-configuring-resilience-and-recovery.md)
#### [Performans sınama ortamı oluşturma](guidance-elasticsearch-creating-performance-testing-environment.md)
#### [JMeter test planı uygulama](guidance-elasticsearch-implementing-jmeter-test-plan.md)
#### [JMeter JUnit örnekleyicisi dağıtma](guidance-elasticsearch-deploying-jmeter-junit-sampler.md)
#### [Otomatik dayanıklılık testleri çalıştırma](guidance-elasticsearch-running-automated-resilience-tests.md)
#### [Otomatik performans testleri çalıştırma](guidance-elasticsearch-running-automated-performance-tests.md)

### [Çok müşterili uygulamalar için kimlik yönetimi](guidance-multitenant-identity.md)
#### [Genel Bakış](guidance-multitenant-identity-intro.md)
#### [Tailspin Surveys uygulaması](guidance-multitenant-identity-tailspin.md)
#### [Azure AD ve OpenID Connect kullanarak kimlik doğrulama](guidance-multitenant-identity-authenticate.md)
#### [Beyana dayalı kimlikler](guidance-multitenant-identity-claims.md)
#### [Kaydolma ve kiracı ekleme](guidance-multitenant-identity-signup.md)
#### [Uygulama rolleri](guidance-multitenant-identity-app-roles.md)
#### [Rol tabanlı ve kaynak tabanlı yetkilendirme](guidance-multitenant-identity-authorize.md)
#### [Arka uç web API’si güvenliğini sağlama](guidance-multitenant-identity-web-api.md)
#### [Önbellek erişim belirteçleri](guidance-multitenant-identity-token-cache.md)
#### [Müşterinin AD FS’si ile federasyon](guidance-multitenant-identity-adfs.md)
#### [Erişim belirteçleri almak için istemci onaylamayı kullanma](guidance-multitenant-identity-client-assertion.md)
#### [Uygulama parolalarını korumak için Azure Anahtar Kasası’nı kullanma](guidance-multitenant-identity-keyvault.md)
#### [Yüksek kullanılabilirlik ortamında sanal gereçleri dağıtma](guidance-nva-ha.md)


<!--HONumber=Nov16_HO4-->


