---
title: Azure kaynakları için yerleşik roller | Microsoft Docs
description: Rol tabanlı erişim denetimi (RBAC) ve Azure kaynakları için yerleşik rolleri açıklanır. Eylemler, NotActions, DataActions ve NotDataActions listeler.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: reference
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 04/01/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: it-pro
ms.openlocfilehash: f505f922685cd192525814df25cca1a1401d2913
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59281225"
---
# <a name="built-in-roles-for-azure-resources"></a>Azure kaynakları için yerleşik roller

[Rol tabanlı erişim denetimi (RBAC)](overview.md) kullanıcıları, grupları, hizmet sorumluları ve yönetilen kimlikleri için atayabileceğiniz Azure kaynakları için birkaç yerleşik rol yok. Rol atamaları Azure kaynaklarına erişimi denetlemek yoludur. Yerleşik roller, kuruluşunuzun özel ihtiyaçlarını karşılamıyorsa, kendi oluşturabileceğiniz [Azure kaynakları için özel roller](custom-roles.md).

Bu makalede, her zaman gelişen Azure kaynakları için yerleşik rolleri listeler. En son rolleri almak için kullanın [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition) veya [az role definition listesini](/cli/azure/role/definition#az-role-definition-list). Azure Active Directory'de yönetici rolleri için arıyorsanız, bkz. [Azure Active Directory'de Yönetici rolü izinleri](../active-directory/users-groups-roles/directory-assign-admin-roles.md).

## <a name="built-in-role-descriptions"></a>Yerleşik rol tanımları

Aşağıdaki tabloda her yerleşik rol kısa bir açıklamasını sağlar. Rol adı listesini görmek için tıklayın `Actions`, `NotActions`, `DataActions`, ve `NotDataActions` her rol için. Bu eylemler ne anlama gelir ve bunlar için yönetim ve veri düzlemleri nasıl uygulandığını hakkında daha fazla bilgi için bkz. [Azure kaynakları için rol tanımlarını anlamak](role-definitions.md).


| Yerleşik rol | Açıklama |
| --- | --- |
| [Sahip](#owner) | Kaynaklara erişim dahil olmak üzere her şeyi yönetmenizi sağlar. |
| [Katılımcı](#contributor) | Kaynaklara erişim hariç olmak üzere her şeyi yönetmenizi sağlar. |
| [Okuyucu](#reader) | Her şeyi görüntülemenizi sağlar ancak değişiklik yapmanıza izin vermez. |
| [AcrDelete](#acrdelete) | ACR delete |
| [AcrImageSigner](#acrimagesigner) | acr görüntüsü imzalayan |
| [AcrPull](#acrpull) | ACR çekme |
| [AcrPush](#acrpush) | ACR gönderme |
| [AcrQuarantineReader](#acrquarantinereader) | acr karantina veri okuyucu |
| [AcrQuarantineWriter](#acrquarantinewriter) | acr karantina veri yazıcı |
| [API Yönetimi Hizmeti Katılımcısı](#api-management-service-contributor) | Hizmeti ve API'leri yönetebilir |
| [API Management Hizmet Operatörü Rolü](#api-management-service-operator-role) | Hizmeti yönetebilir, ancak API'leri yönetemez |
| [API Management Hizmet Okuyucusu Rolü](#api-management-service-reader-role) | Hizmet ve API'lere salt okunur erişim |
| [Application Insights Bileşeni Katılımcısı](#application-insights-component-contributor) | Application Insights bileşenlerini yönetebilir |
| [Application Insights Snapshot Debugger](#application-insights-snapshot-debugger) | Görüntülemenizi ve indirmenizi ile Application Insights Snapshot Debugger toplanan hata ayıklama anlık görüntülerini kullanıcı izin verilir. Bu izinleri dahil olmadığına dikkat edin [sahibi](#owner) veya [katkıda bulunan](#contributor) rolleri. |
| [Otomasyon İşi İşleci](#automation-job-operator) | Otomasyon Runbook'larını kullanarak İş oluşturun ve yönetin. |
| [Otomasyon Operatörü](#automation-operator) | Otomasyon Operatörleri, işleri başlatabilir, durdurabilir, askıya alabilir ve sürdürebilir |
| [Otomasyon Runbook'u İşleci](#automation-runbook-operator) | Runbook'un İşlerini oluşturabilmek için Runbook özelliklerini okuyun. |
| [Avere katkıda bulunan](#avere-contributor) | Oluşturabilir ve bir Avere vFXT kümesini yönetme. |
| [Avere işleci](#avere-operator) | Kümeyi yönetmek için Avere vFXT küme tarafından kullanılan |
| [Azure Kubernetes Hizmet Kümesi Yönetici Rolü](#azure-kubernetes-service-cluster-admin-role) | Küme yöneticisi kimlik bilgileri eylemini listele. |
| [Azure Kubernetes Hizmet Kümesi Kullanıcı Rolü](#azure-kubernetes-service-cluster-user-role) | Küme kullanıcısı kimlik bilgileri eylemini listele. |
| [Azure Stack Kayıt Sahibi](#azure-stack-registration-owner) | Azure Stack kayıtlarını yönetmenize imkan sağlar. |
| [Yedekleme Katılımcısı](#backup-contributor) | Yedekleme hizmetini yönetmenize olanak sağlar ancak kasa oluşturma ve diğer kullanıcılara erişim verme izni sağlamaz |
| [Yedekleme İşleci](#backup-operator) | Yedekleme kaldırma, kasa oluşturma ve diğer kullanıcılara erişim verme dışındaki yedekleme hizmetlerini yönetmenize olanak sağlar |
| [Yedekleme Okuyucusu](#backup-reader) | Yedekleme hizmetlerini görüntüleyebilir ancak değişiklik yapamaz |
| [Faturalama Okuyucusu](#billing-reader) | Faturalandırma verilerine okuma erişimi verir |
| [BizTalk Katılımcısı](#biztalk-contributor) | BizTalk hizmetlerini yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |
| [CDN Uç Noktası Katkıda Bulunanı](#cdn-endpoint-contributor) | CDN uç noktalarını yönetebilir, ancak diğer kullanıcılara erişim izni veremez. |
| [CDN Uç Nokta Okuyucusu](#cdn-endpoint-reader) | CDN uç noktalarını görüntüleyebilir, ancak değişiklik yapamaz. |
| [CDN Profili Katkıda Bulunanı](#cdn-profile-contributor) | CDN profillerini ve uç noktalarını yönetebilir, ancak diğer kullanıcılara erişim izni veremez. |
| [CDN Profil Okuyucusu](#cdn-profile-reader) | CDN profillerini ve uç noktalarını görüntüleyebilir, ancak değişiklik yapamaz. |
| [Klasik Ağ Katılımcısı](#classic-network-contributor) | Klasik ağları yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |
| [Klasik Depolama Hesabı Katılımcısı](#classic-storage-account-contributor) | Klasik depolama hesaplarını yönetmenize izin verir ancak bu hesaplara erişim hakkı vermez. |
| [Klasik Depolama Hesabı Anahtarı İşleci Hizmet Rolü](#classic-storage-account-key-operator-service-role) | Klasik Depolama Hesabı Anahtarı İşleçlerine, Klasik Depolama Hesaplarında anahtarları listeleme ve yeniden oluşturma izni verilir |
| [Klasik Sanal Makine Katılımcısı](#classic-virtual-machine-contributor) | Klasik sanal makineleri yönetmenizi sağlar ancak bunlara veya bağlı oldukları sanal ağ ya da depolama hesaplarına yönelik erişimi yönetme izni vermez. |
| [Bilişsel Hizmetler Katılımcısı](#cognitive-services-contributor) | Bilişsel Hizmetler anahtarlarını oluşturmanıza, okumanıza, güncelleştirmenize, silmenize ve yönetmenize olanak sağlar. |
| [Bilişsel Hizmetler Veri Okuyucusu (Önizleme)](#cognitive-services-data-reader-preview) | Bilişsel Hizmetler verilerini okumanıza olanak sağlar. |
| [Bilişsel Hizmetler Kullanıcısı](#cognitive-services-user) | Bilişsel Hizmetler anahtarlarını okumanıza ve listelemenize olanak sağlar. |
| [Cosmos DB Hesabı Okuyucusu Rolü](#cosmos-db-account-reader-role) | Azure Cosmos DB hesabı verileri okuyabilir. Bkz: [DocumentDB hesabı Katılımcısı](#documentdb-account-contributor) Azure Cosmos DB hesapları yönetme. |
| [CosmosBackupOperator](#cosmosbackupoperator) | Bir Cosmos DB veritabanı veya bir hesaba ilişkin bir kapsayıcı için geri yükleme isteği gönderebilir |
| [Maliyet Yönetimi Katkıda Bulunanı](#cost-management-contributor) | Maliyetleri görüntüleyebilir ve maliyet yapılandırmasını yönetebilir (örneğin bütçeler, dışarı aktarmalar) |
| [Maliyet Yönetimi Okuyucusu](#cost-management-reader) | Maliyet verileri ve yapılandırmasını (örneğin bütçeler, dışarı aktarmalar) görüntüleyebilir |
| [Veri Kutusu Katılımcısı](#data-box-contributor) | Başkalarına erişim izni verme hariç Veri Kutusu Hizmeti altındaki her şeyi yönetmenize olanak sağlar. |
| [Veri Kutusu Okuyucusu](#data-box-reader) | Sipariş oluşturma veya sipariş ayrıntılarını düzenleme ve başkalarına erişim izni verme hariç Veri Kutusu Hizmeti özelliklerini yönetmenize olanak sağlar. |
| [Data Factory Katılımcısı](#data-factory-contributor) | Veri fabrikaları ile birlikte bunların alt kaynaklarını oluşturup yönetin. |
| [Data Lake Analytics Geliştiricisi](#data-lake-analytics-developer) | İşlerinizi göndermenize, izlemenize ve yönetmenize izin verir, ancak Data Lake Analytics hesabı oluşturmanıza veya silmenize izin vermez. |
| [Veri Temizleyici](#data-purger) | Analiz verilerini temizleyebilir |
| [DevTest Labs Kullanıcısı](#devtest-labs-user) | Azure DevTest Labs'teki tüm sanal makinelerinize bağlanmanıza, bu makineleri başlatmanıza, yeniden başlatmanıza ve kapatmanıza izin verir. |
| [DNS Bölgesi Katkıda Bulunanı](#dns-zone-contributor) | Azure DNS'te, DNS bölgelerini ve kayıt kümelerini yönetmenize izin verir, ancak bunlara kimlerin erişebildiğini denetlemenize izin vermez. |
| [DocumentDB Hesabı Katılımcısı](#documentdb-account-contributor) | Azure Cosmos DB hesapları yönetebilirsiniz. Azure Cosmos DB, eski adıyla DocumentDB bilinir. |
| [EventGrid EventSubscription Katkıda Bulunanı](#eventgrid-eventsubscription-contributor) | EventGrid olay aboneliği işlemlerini yönetmenize olanak sağlar. |
| [EventGrid EventSubscription Okuyucusu](#eventgrid-eventsubscription-reader) | EventGrid olay aboneliklerini okumanıza olanak sağlar. |
| [HDInsight Etki Alanı Hizmetleri Katılımcısı](#hdinsight-domain-services-contributor) | Okuyabilir oluşturma, değiştirme ve etki alanı Hizmetleri Sil ilgili işlemler için HDInsight Kurumsal Güvenlik Paketi için gereken Etki Alanı Hizmetleriyle ilgili işlemleri Okuyabilir, Oluşturabilir, Değiştirebilir ve Silebilir |
| [Akıllı Sistemler Hesap Katılımcısı](#intelligent-systems-account-contributor) | Akıllı Sistemler hesaplarını yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |
| [Key Vault Katkıda Bulunanı](#key-vault-contributor) | Anahtar kasalarını yönetmenize izin verir, ancak bunlara erişmenize izin vermez. |
| [Laboratuvar Oluşturan](#lab-creator) | Azure Laboratuvar Hesaplarınız altında yönetilen laboratuvarları oluşturmanıza, yönetmenize ve silmenize olanak sağlar. |
| [Log Analytics Katkıda Bulunan](#log-analytics-contributor) | Log Analytics katkıda bulunan tüm izleme verilerini okuyabilir ve izleme ayarlarını düzenleyin. İzleme ayarlarını düzenleme Vm'lere VM uzantısı ekleme içerir; Azure Depolama'dan günlüklerin toplanmasını yapılandırma yapabilmek için depolama hesabı anahtarlarını okuma; oluşturma ve Otomasyon hesapları yapılandırma; çözümler eklenerek; ve tüm Azure kaynaklarında Azure tanılamayı yapılandırma. |
| [Log Analytics Okuyucusu](#log-analytics-reader) | Log Analytics Okuyucusu, tüm izleme verilerinin görüntüleme ve aramanın yanı sıra izleme ayarlarını da (tüm Azure kaynaklarındaki Azure tanılama yapılandırmalarını görüntüleme dahil) görüntüleyebilir. |
| [Mantıksal Uygulama Katkıda Bulunanı](#logic-app-contributor) | Mantıksal uygulamayı yönetmenize izin verir, ancak bunlara yönelik erişimi yönetmenize izin vermez. |
| [Mantıksal Uygulama Operatörü](#logic-app-operator) | Mantıksal uygulamayı okumanıza, etkinleştirmenize ve devre dışı bırakmanıza izin verir. |
| [Yönetilen Uygulama İşletmeni Rolü](#managed-application-operator-role) | Yönetilen Uygulama kaynakları üzerinde eylemleri okumanıza ve gerçekleştirmenize olanak sağlar |
| [Yönetilen Uygulamalar Okuyucusu](#managed-applications-reader) | Yönetilen bir uygulama içinde kaynakları okumanıza ve JIT erişimi isteğinde bulunmanıza olanak sağlar. |
| [Yönetilen Kimlik Katılımcısı](#managed-identity-contributor) | Kullanıcı Tarafından Atanan Kimliği Oluşturma, Okuma, Güncelleştirme ve Silme |
| [Yönetilen Kimlik İşleci](#managed-identity-operator) | Kullanıcı Tarafından Atanan Kimliği Oku ve Ata |
| [Yönetim Grubu Katkıda Bulunanı](#management-group-contributor) | Yönetim Grubu Katkıda Bulunanı Rolü |
| [Yönetim Grubu Okuyucusu](#management-group-reader) | Yönetim Grubu Okuyucusu Rolü |
| [İzleme Katkıda Bulunanı](#monitoring-contributor) | Tüm izleme verilerini okuyabilir ve izleme ayarlarını düzenleyin. Ayrıca bkz: [Azure İzleyici ile güvenlik rolleri ve izinleri ile çalışmaya başlama](../azure-monitor/platform/roles-permissions-security.md#built-in-monitoring-roles). |
| [İzleme Ölçüm Yayımcısı](#monitoring-metrics-publisher) | Azure kaynaklarına karşı ölçümleri yayımlamayı etkinleştirir |
| [İzleme Okuyucusu](#monitoring-reader) | (Ölçümler, günlükler, vb.) tüm izleme verilerini okuyabilir. Ayrıca bkz: [Azure İzleyici ile güvenlik rolleri ve izinleri ile çalışmaya başlama](../azure-monitor/platform/roles-permissions-security.md#built-in-monitoring-roles). |
| [Ağ Katılımcısı](#network-contributor) | Ağları yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |
| [New Relic APM Hesap Katılımcısı](#new-relic-apm-account-contributor) | New Relic Application Performance Management hesaplarını ve uygulamalarını yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |
| [Okuyucu ve Veri Erişimi](#reader-and-data-access) | Sağlayan her şeyi görüntüleyebilir, ancak bir depolama hesabı ya da kapsanan kaynak oluşturma veya silemezsiniz izin vermez. Depolama hesabı anahtarları erişimi üzerinden bir depolama hesabında kapsanan tüm verilere okuma/yazma erişimi de izin verir. |
| [Redis Cache Katılımcısı](#redis-cache-contributor) | Redis Cache'leri yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |
| [Kaynak İlkesine Katkıda Bulunan (Önizleme)](#resource-policy-contributor-preview) | (Önizleme) Kaynak ilkesi oluşturma/değiştirme, destek bileti oluşturma ve kaynakları/hiyerarşiyi okuma haklarıyla EA’dan kullanıcılar geri dolduruldu. |
| [Zamanlayıcı İş Koleksiyonları Katılımcısı](#scheduler-job-collections-contributor) | Zamanlayıcı iş koleksiyonlarını yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |
| [Search Hizmeti Katılımcısı](#search-service-contributor) | Search hizmetlerini yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |
| [Güvenlik Yöneticisi](#security-admin) | Güvenlik Merkezi'nde yalnızca: Güvenlik ilkelerini görüntüleyin, güvenlik durumlarını görüntülemek, güvenlik ilkeleri, uyarıları görüntüleme ve öneriler düzenleme, uyarıları ve öneriler Kapat |
| [Güvenlik Yöneticisi (Eski)](#security-manager-legacy) | Bu eski bir roldür. Lütfen bunun yerine Güvenlik Yöneticisi kullanın |
| [Güvenlik Okuyucusu](#security-reader) | Güvenlik Merkezi'nde yalnızca: Öneriler ve uyarılar, güvenlik ilkeleri, güvenlik durumlarını görüntüleyebilir ancak değişiklik yapamaz görünüm görüntüleyebilirsiniz. |
| [Site Recovery Katkıda Bulunanı](#site-recovery-contributor) | Kasa oluşturma ve rol atama işlemleri dışında Site Recovery hizmetini yönetmenize imkan sağlar |
| [Site Recovery Operatörü](#site-recovery-operator) | Yük devretme ve yeniden çalışma dışındaki Site Recovery yönetimi işlemlerini gerçekleştirmenize izin vermez |
| [Site Recovery Okuyucusu](#site-recovery-reader) | Site Recovery durumunu görüntülemenize izin verir, ancak diğer yönetim işlemlerini gerçekleştirmenize izin vermez |
| [Spatial Anchors Hesabı Katkıda Bulunanı](#spatial-anchors-account-contributor) | Hesabınızdaki uzamsal sabit noktaları yönetmenize olanak sağlar, ancak silmenize izin vermez |
| [Spatial Anchors Hesabı Sahibi](#spatial-anchors-account-owner) | Silme de dahil olmak üzere hesabınızdaki uzamsal sabit noktaları yönetmenize olanak sağlar |
| [Spatial Anchors Hesabı Okuyucusu](#spatial-anchors-account-reader) | Hesabınızdaki uzamsal sabit noktaları bulup bunların özelliklerini okumanıza olanak sağlar |
| [SQL DB Katılımcısı](#sql-db-contributor) | SQL veritabanları, ancak onlara yönelik erişimi yönetmenize olanak tanır. Ayrıca, güvenlikle ilgili ilkelerini veya üst SQL sunucularını yönetemezsiniz. |
| [SQL Güvenlik Yöneticisi](#sql-security-manager) | SQL sunucularının ve veritabanlarının güvenlikle ilgili ilkelerini yönetmenizi sağlar ancak onlara erişimi yönetme izni vermez. |
| [SQL Server Katılımcısı](#sql-server-contributor) | SQL sunucularını ve veritabanlarını yönetmenizi sağlar ancak güvenlikle ilgili ilkelerini yönetmenize izin vermez. |
| [Depolama Hesabı Katılımcısı](#storage-account-contributor) | Depolama hesaplarını yönetmenize izin verir ancak bunlara yönelik erişimi yönetmenize izin vermez. |
| [Depolama Hesabı Anahtarı İşleci Hizmet Rolü](#storage-account-key-operator-service-role) | Depolama Hesabı Anahtarı İşleçlerine, Depolama Hesaplarında anahtarları listeleme ve yeniden oluşturma izni verilir |
| [Depolama Blob Verileri Katkıda Bulunanı](#storage-blob-data-contributor) | Azure Depolama blob kapsayıcılarına ve verilerine yönelik okuma, yazma ve silme erişimi verir |
| [Depolama Blob Verileri Sahibi](#storage-blob-data-owner) | POSIX erişim denetimini atama dahil olmak üzere Azure Depolama blobu kapsayıcılarına ve verilerine tam erişim sağlar. |
| [Depolama Blob Verileri Okuyucusu](#storage-blob-data-reader) | Azure Depolama blob kapsayıcılarına ve verilerine yönelik okuma erişimi verir |
| [Depolama Kuyruk Verileri Katkıda Bulunanı](#storage-queue-data-contributor) | Azure Depolama kuyruklarına ve kuyruk iletilerine yönelik okuma, yazma ve silme erişimi verir |
| [Depolama Kuyruk Verileri İleti İşleyicisi](#storage-queue-data-message-processor) | Azure Depolama kuyruk iletilerine yönelik göz atma, alma ve silme erişimi verir |
| [Depolama Kuyruk Verileri İleti Göndericisi](#storage-queue-data-message-sender) | Azure Depolama kuyruk iletilerinin gönderilmesine izin verir |
| [Depolama Kuyruk Verileri Okuyucusu](#storage-queue-data-reader) | Azure Depolama kuyruklarına ve kuyruk iletilerine yönelik okuma erişimi verir |
| [Destek İsteğine Katkıda Bulunan](#support-request-contributor) | Destek istekleri oluşturmanıza ve bunları yönetmenize olanak sağlar |
| [Traffic Manager Katkıda Bulunanı](#traffic-manager-contributor) | Traffic Manager profillerini yönetmenize izin verir, ancak bunlara kimlerin erişebildiğini denetlemenize izin vermez. |
| [Kullanıcı Erişimi Yöneticisi](#user-access-administrator) | Azure kaynaklarına yönelik kullanıcı erişimini yönetmenizi sağlar. |
| [Sanal Makine Yöneticisi Oturum Açma](#virtual-machine-administrator-login) | Portalda Sanal Makineleri görüntüleyin ve yönetici olarak oturum açın |
| [Sanal Makine Katılımcısı](#virtual-machine-contributor) | Sanal makineleri yönetmenizi sağlar ancak bu sanal makinelere ve bağlı oldukları sanal ağ veya depolama hesaplarına yönelik erişimi yönetme izni vermez. |
| [Sanal Makine Kullanıcı Oturum Açma](#virtual-machine-user-login) | Portalda Sanal Makineleri görüntüleyin ve normal bir kullanıcı olarak oturum açın. |
| [Web Planı Katılımcısı](#web-plan-contributor) | Web siteleri için web planlarını yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |
| [Web Sitesi Katılımcısı](#website-contributor) | Web sitelerini (web planlarını değil) yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |


## <a name="owner"></a>Sahip
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Kaynaklara erişim dahil olmak üzere her şeyi yönetmenizi sağlar. |
> | **Kimlik** | 8e3af657-a8ff-443c-a75c-2fe8c4bcb635 |
> | **Eylemler** |  |
> | * | Tüm türlerin kaynak oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="contributor"></a>Katılımcı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Kaynaklara erişim hariç olmak üzere her şeyi yönetmenizi sağlar. |
> | **Kimlik** | b24988ac-6180-42a0-ab88-20f7382dd24c |
> | **Eylemler** |  |
> | * | Tüm türlerin kaynak oluşturma ve yönetme |
> | **NotActions** |  |
> | Microsoft.Authorization/*/Delete | Rolleri ve rol atamaları silin |
> | Microsoft.Authorization/*/Write | Rolleri ve rol ataması oluştur |
> | Microsoft.Authorization/elevateAccess/Action | Çağırana kiracı kapsamında Kullanıcı Erişim Yöneticisi erişimi verir |
> | Microsoft.Blueprint/blueprintAssignments/write | Herhangi bir şema yapıtını oluşturun veya güncelleştirin |
> | Microsoft.Blueprint/blueprintAssignments/delete | Herhangi bir şema yapıtını silin |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="reader"></a>Okuyucu
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Her şeyi görüntülemenizi sağlar ancak değişiklik yapmanıza izin vermez. |
> | **Kimlik** | acdd72a7-3385-48ef-bd42-f606fba81ae7 |
> | **Eylemler** |  |
> | * / Okuma | Gizli dizileri dışında tüm türler kaynakları okuyun. |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="acrdelete"></a>AcrDelete
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | ACR delete |
> | **Kimlik** | c2f4ef07-c644-48eb-af81-4b1b4947fb11 |
> | **Eylemler** |  |
> | Microsoft.ContainerRegistry/registries/artifacts/delete | Kapsayıcı kayıt defterindeki yapıt silin. |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="acrimagesigner"></a>AcrImageSigner
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | acr görüntüsü imzalayan |
> | **Kimlik** | 6cef56e8-d556-48e5-a04f-b8e64114680f |
> | **Eylemler** |  |
> | Microsoft.ContainerRegistry/registries/sign/write | Kapsayıcı kayıt defteri meta verilerini içeriğine güven İtme veya çekme. |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="acrpull"></a>AcrPull
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | ACR çekme |
> | **Kimlik** | 7f951dda-4ed3-4680-a7ca-43fe172d538d |
> | **Eylemler** |  |
> | Microsoft.ContainerRegistry/registries/pull/read | Çektiğinizde ya da kapsayıcı kayıt defterinden görüntüleri alın. |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="acrpush"></a>AcrPush
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | ACR gönderme |
> | **Kimlik** | 8311e382-0749-4cb8-b61a-304f252e45ec |
> | **Eylemler** |  |
> | Microsoft.ContainerRegistry/registries/pull/read | Çektiğinizde ya da kapsayıcı kayıt defterinden görüntüleri alın. |
> | Microsoft.ContainerRegistry/registries/push/write | Anında iletme veya görüntüleri bir kapsayıcı kayıt defterine yazma. |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="acrquarantinereader"></a>AcrQuarantineReader
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | acr karantina veri okuyucu |
> | **Kimlik** | cdda3590-29a3-44f6-95f2-9f980659eb04 |
> | **Eylemler** |  |
> | Microsoft.ContainerRegistry/registries/quarantineRead/read | Çektiğinizde ya da kapsayıcı kayıt defterinden karantinaya alınan görüntü alma |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="acrquarantinewriter"></a>AcrQuarantineWriter
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | acr karantina veri yazıcı |
> | **Kimlik** | c8d4ff99-41c3-41a8-9f60-21dfdad59608 |
> | **Eylemler** |  |
> | Microsoft.ContainerRegistry/registries/quarantineRead/read | Çektiğinizde ya da kapsayıcı kayıt defterinden karantinaya alınan görüntü alma |
> | Microsoft.ContainerRegistry/registries/quarantineWrite/write | Yazma/karantinaya alınan görüntüleri karantina durumunu değiştir |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="api-management-service-contributor"></a>API Yönetimi Hizmeti Katılımcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Hizmeti ve API'leri yönetebilir |
> | **Kimlik** | 312a565d-c81f-4fd8-895a-4e21e48d571c |
> | **Eylemler** |  |
> | Microsoft.ApiManagement/service/* | Oluşturma ve API Management hizmeti yönetme |
> | Microsoft.Authorization/*/read | Yetkilendirme okuyun |
> | Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönet |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="api-management-service-operator-role"></a>API Management Hizmet Operatörü Rolü
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Hizmeti yönetebilir, ancak API'leri yönetemez |
> | **Kimlik** | e022efe7-f5ba-4159-bbe4-b44f577e9b61 |
> | **Eylemler** |  |
> | Microsoft.ApiManagement/service/*/read | Okuma API Management hizmeti örnekleri |
> | Microsoft.ApiManagement/service/backup/action | Belirtilen kapsayıcıya bir kullanıcı için yedekleme API Management hizmeti sağlanan depolama hesabı |
> | Microsoft.ApiManagement/service/delete | API Management hizmet örneği silme |
> | Microsoft.ApiManagement/service/managedeployments/action | SKU/birimlerini, API Management hizmetinin bölgesel dağıtımlara ekleme/kaldırma değiştirme |
> | Microsoft.ApiManagement/service/read | Bir API Management hizmet örneği için meta veriler okuma |
> | Microsoft.ApiManagement/service/restore/action | API Management hizmeti bir kullanıcı tarafından sağlanan depolama hesabı belirtilen kapsayıcıda geri yükleme |
> | Microsoft.ApiManagement/service/updatecertificate/action | Bir API Management hizmeti için SSL sertifikasını karşıya yükle |
> | Microsoft.ApiManagement/service/updatehostname/action | Kurulum, güncelleştirmeniz ya da bir API Management hizmeti için özel etki alanı adlarını Kaldır |
> | Microsoft.ApiManagement/service/write | Yeni bir API Management hizmeti örneği oluşturma |
> | Microsoft.Authorization/*/read | Yetkilendirme okuyun |
> | Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönet |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | Microsoft.ApiManagement/service/users/keys/read | Kullanıcı anahtarları listesini alın |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="api-management-service-reader-role"></a>API Management Hizmet Okuyucusu Rolü
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Hizmet ve API'lere salt okunur erişim |
> | **Kimlik** | 71522526-b88f-4d52-b57f-d31fc3546d0d |
> | **Eylemler** |  |
> | Microsoft.ApiManagement/service/*/read | Okuma API Management hizmeti örnekleri |
> | Microsoft.ApiManagement/service/read | Bir API Management hizmet örneği için meta veriler okuma |
> | Microsoft.Authorization/*/read | Yetkilendirme okuyun |
> | Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönet |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | Microsoft.ApiManagement/service/users/keys/read | Kullanıcı anahtarları listesini alın |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="application-insights-component-contributor"></a>Application Insights Bileşeni Katılımcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Application Insights bileşenlerini yönetebilir |
> | **Kimlik** | ae349356-3a1b-4a5e-921d-050484c6347e |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönet |
> | Microsoft.Insights/components/* | Oluşturma ve Insights bileşenlerini yönetme |
> | Microsoft.Insights/webtests/* | Oluşturma ve yönetme web testleri |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="application-insights-snapshot-debugger"></a>Application Insights Snapshot Debugger
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Görüntülemenizi ve indirmenizi ile Application Insights Snapshot Debugger toplanan hata ayıklama anlık görüntülerini kullanıcı izin verilir. Bu izinleri dahil olmadığına dikkat edin [sahibi](#owner) veya [katkıda bulunan](#contributor) rolleri. |
> | **Kimlik** | 08954f03-6346-4c2e-81c0-ec3a5cfae23b |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Insights/alertRules/* | Oluşturma ve yönetme Insights uyarı kuralları |
> | Microsoft.Insights/components/*/read |  |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="automation-job-operator"></a>Otomasyon İşi İşleci
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Otomasyon Runbook'larını kullanarak İş oluşturun ve yönetin. |
> | **Kimlik** | 4fe576fe-1146-4730-92eb-48519fa6bf9f |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read | Karma Runbook çalışanı kaynaklarını okur |
> | Microsoft.Automation/automationAccounts/jobs/read | Bir Azure Otomasyonu işini alır |
> | Microsoft.Automation/automationAccounts/jobs/resume/action | Bir Azure Otomasyonu işini sürdürür |
> | Microsoft.Automation/automationAccounts/jobs/stop/action | Bir Azure Otomasyonu işini durdurur |
> | Microsoft.Automation/automationAccounts/jobs/streams/read | Bir Azure Otomasyonu iş akışı alır |
> | Microsoft.Automation/automationAccounts/jobs/suspend/action | Azure Otomasyonu işini askıya alır |
> | Microsoft.Automation/automationAccounts/jobs/write | Bir Azure Otomasyonu işi oluşturur |
> | Microsoft.Automation/automationAccounts/jobs/output/read | Bir işin çıktısını alır |
> | Microsoft.Insights/alertRules/* | Oluşturma ve yönetme Insights uyarı kuralları |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="automation-operator"></a>Otomasyon Operatörü
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Otomasyon Operatörleri, işleri başlatabilir, durdurabilir, askıya alabilir ve sürdürebilir |
> | **Kimlik** | d3881f73-407a-4167-8283-e981cbba0404 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read | Karma Runbook çalışanı kaynaklarını okur |
> | Microsoft.Automation/automationAccounts/jobs/read | Bir Azure Otomasyonu işini alır |
> | Microsoft.Automation/automationAccounts/jobs/resume/action | Bir Azure Otomasyonu işini sürdürür |
> | Microsoft.Automation/automationAccounts/jobs/stop/action | Bir Azure Otomasyonu işini durdurur |
> | Microsoft.Automation/automationAccounts/jobs/streams/read | Bir Azure Otomasyonu iş akışı alır |
> | Microsoft.Automation/automationAccounts/jobs/suspend/action | Azure Otomasyonu işini askıya alır |
> | Microsoft.Automation/automationAccounts/jobs/write | Bir Azure Otomasyonu işi oluşturur |
> | Microsoft.Automation/automationAccounts/jobSchedules/read | Bir Azure Otomasyonu iş zamanlaması alır |
> | Microsoft.Automation/automationAccounts/jobSchedules/write | Bir Azure Otomasyonu iş zamanlaması oluşturur |
> | Microsoft.Automation/automationAccounts/linkedWorkspace/read | Otomasyon hesabına bağlı çalışma alanını alır |
> | Microsoft.Automation/automationAccounts/read | Bir Azure Otomasyonu hesabını alır |
> | Microsoft.Automation/automationAccounts/runbooks/read | Bir Azure Otomasyonu runbook'unu alır |
> | Microsoft.Automation/automationAccounts/schedules/read | Bir Azure Otomasyonu zamanlama varlığını alır |
> | Microsoft.Automation/automationAccounts/schedules/write | Bir Azure Otomasyonu zamanlama varlığı oluşturur veya güncelleştirir |
> | Microsoft.Insights/alertRules/* | Oluşturma ve yönetme Insights uyarı kuralları |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Automation/automationAccounts/jobs/output/read | Bir işin çıktısını alır |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="automation-runbook-operator"></a>Otomasyon Runbook'u İşleci
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Runbook'un İşlerini oluşturabilmek için Runbook özelliklerini okuyun. |
> | **Kimlik** | 5fb5aef8-1081-4b8e-bb16-9d5d0385bab5 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Automation/automationAccounts/runbooks/read | Bir Azure Otomasyonu runbook'unu alır |
> | Microsoft.Insights/alertRules/* | Oluşturma ve yönetme Insights uyarı kuralları |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="avere-contributor"></a>Avere katkıda bulunan
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Oluşturabilir ve bir Avere vFXT kümesini yönetme. |
> | **Kimlik** | 4f8fab4f-1852-4a58-a46a-8eaf358af14a |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Compute/*/read |  |
> | Microsoft.Compute/availabilitySets/* |  |
> | Microsoft.Compute/virtualMachines/* |  |
> | Microsoft.Compute/disks/* |  |
> | Microsoft.Network/*/read |  |
> | Microsoft.Network/networkInterfaces/* |  |
> | Microsoft.Network/virtualNetworks/read | Sanal ağ tanımı Al |
> | Microsoft.Network/virtualNetworks/subnets/read | Bir sanal ağ alt ağı tanımı alır |
> | Microsoft.Network/virtualNetworks/subnets/join/action | Bir sanal ağa katılır. Alertable değil. |
> | Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Kaynak depolama hesabı veya SQL veritabanı gibi bir alt ağa katılır. Alertable değil. |
> | Microsoft.Network/networkSecurityGroups/join/action | Bir ağ güvenlik grubu birleştirir. Alertable değil. |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Insights/alertRules/* | Oluşturma ve yönetme Insights uyarı kuralları |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Storage/*/read |  |
> | Microsoft.Storage/storageAccounts/* |  |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/resources/read | Kaynak grubun kaynaklarını alır. |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete | Bir blobun silinmesinin sonucunu döndürür |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read | Bir blobu veya blob listesini döndürür |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write | Bir blobu yazmanın sonucunu döndürür |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="avere-operator"></a>Avere işleci
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Kümeyi yönetmek için Avere vFXT küme tarafından kullanılan |
> | **Kimlik** | c025889f-8102-4ebf-b32c-fc0c6f0c6bd9 |
> | **Eylemler** |  |
> | Microsoft.Compute/virtualMachines/read | Bir sanal makinenin özelliklerini alır |
> | Microsoft.Network/networkInterfaces/read | Bir ağ arabirimi tanımı alır.  |
> | Microsoft.Network/networkInterfaces/write | Bir ağ arabirimi oluşturur veya mevcut bir ağ arabirimi güncelleştirir.  |
> | Microsoft.Network/virtualNetworks/read | Sanal ağ tanımı Al |
> | Microsoft.Network/virtualNetworks/subnets/read | Bir sanal ağ alt ağı tanımı alır |
> | Microsoft.Network/virtualNetworks/subnets/join/action | Bir sanal ağa katılır. Alertable değil. |
> | Microsoft.Network/networkSecurityGroups/join/action | Bir ağ güvenlik grubu birleştirir. Alertable değil. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Storage/storageAccounts/blobServices/containers/delete | Bir kapsayıcının silinmesinin sonucunu döndürür |
> | Microsoft.Storage/storageAccounts/blobServices/containers/read | Kapsayıcı listesini döndürür |
> | Microsoft.Storage/storageAccounts/blobServices/containers/write | Put blob kapsayıcısı sonucunu döndürür |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete | Bir blobun silinmesinin sonucunu döndürür |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read | Bir blobu veya blob listesini döndürür |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write | Bir blobu yazmanın sonucunu döndürür |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="azure-kubernetes-service-cluster-admin-role"></a>Azure Kubernetes Hizmet Kümesi Yönetici Rolü
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Küme yöneticisi kimlik bilgileri eylemini listele. |
> | **Kimlik** | 0ab0b1a8-8aac-4efd-b8c2-3ee1fb270be8 |
> | **Eylemler** |  |
> | Microsoft.ContainerService/managedClusters/listClusterAdminCredential/action | Yönetilen bir kümenin clusterAdmin kimlik bilgilerini listele |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="azure-kubernetes-service-cluster-user-role"></a>Azure Kubernetes Hizmet Kümesi Kullanıcı Rolü
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Küme kullanıcısı kimlik bilgileri eylemini listele. |
> | **Kimlik** | 4abbcc35-e782-43d8-92c5-2d3f1bd2253f |
> | **Eylemler** |  |
> | Microsoft.ContainerService/managedClusters/listClusterUserCredential/action | Yönetilen bir kümenin clusterUser kimlik bilgilerini listele |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="azure-stack-registration-owner"></a>Azure Stack Kayıt Sahibi
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Azure Stack kayıtlarını yönetmenize imkan sağlar. |
> | **Kimlik** | 6f12a6df-dd06-4f3e-bcb1-ce8be600526a |
> | **Eylemler** |  |
> | Microsoft.AzureStack/registrations/products/listDetails/action | Azure Stack Marketini ürün ayrıntılarını genişletilmiş alır |
> | Microsoft.AzureStack/registrations/products/read | Azure Stack Marketini ürün özelliklerini alır |
> | Microsoft.AzureStack/registrations/read | Azure Stack kayıt özelliklerini alır |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="backup-contributor"></a>Yedekleme Katılımcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Yedekleme hizmetini yönetmenize olanak sağlar ancak kasa oluşturma ve diğer kullanıcılara erişim verme izni sağlamaz |
> | **Kimlik** | 5e467623-bb1f-42f4-a55d-6e525e11384b |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Network/virtualNetworks/read | Sanal ağ tanımı Al |
> | Microsoft.RecoveryServices/locations/* |  |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/* | Yedekleme Yönetimi işleminin sonuçlarını yönetme |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/* | Oluşturma ve yedekleme kurtarma Hizmetleri kasasına yedekleme yapılarında kapsayıcılarda yönetme |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/refreshContainers/action | Kapsayıcı listesini yeniler |
> | Microsoft.RecoveryServices/Vaults/backupJobs/* | Yedekleme işler oluşturma ve yönetme |
> | Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Dışarı aktarma işleri |
> | Microsoft.RecoveryServices/Vaults/backupJobsExport/operationResults/read |  |
> | Microsoft.RecoveryServices/Vaults/backupManagementMetaData/* | Oluşturma ve yedekleme yönetimi ile ilgili meta verileri yönetme |
> | Microsoft.RecoveryServices/Vaults/backupOperationResults/* | Oluşturma ve yedekleme yönetim işlemlerinin sonuçlarını yönetme |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/* | Yedekleme ilkeleri oluşturma ve yönetme |
> | Microsoft.RecoveryServices/Vaults/backupProtectableItems/* | Oluşturma ve yedeklenebilir öğeleri yönetme |
> | Microsoft.RecoveryServices/Vaults/backupProtectedItems/* | Oluşturma ve yedeklenen öğeleri yönetme |
> | Microsoft.RecoveryServices/Vaults/backupProtectionContainers/* | Oluşturma ve yedekleme öğeleri tutan kapsayıcılar'ı yönetme |
> | Microsoft.RecoveryServices/Vaults/backupSecurityPIN/* |  |
> | Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Özetleri için bir kurtarma Hizmetleri korumalı öğeler ve korumalı sunucuların döndürür. |
> | Microsoft.RecoveryServices/Vaults/certificates/* | Kurtarma Hizmetleri kasasında yedekleme ilgili sertifikaları oluşturmak ve yönetmek |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/* | Oluşturma ve yönetme kasaya ilgili genişletilmiş bilgileri |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Kurtarma Hizmetleri kasası için uyarıları alır. |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/* |  |
> | Microsoft.RecoveryServices/Vaults/read | Kasayı Al işlemi 'vault' türündeki Azure kaynağını temsil eden bir nesne alır. |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/* | Oluşturma ve kayıtlı kimlikleri yönetme |
> | Microsoft.RecoveryServices/Vaults/usages/* | Oluşturma ve kurtarma Hizmetleri kasası kullanımını yönetme |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Storage/storageAccounts/read | Depolama hesaplarının listesini döndürür veya belirtilen depolama hesabının özelliklerini alır. |
> | Microsoft.RecoveryServices/Vaults/backupstorageconfig/* |  |
> | Microsoft.RecoveryServices/Vaults/backupconfig/* |  |
> | Microsoft.RecoveryServices/Vaults/backupValidateOperation/action | Korumalı öğe üzerinde işlem doğrulama |
> | Microsoft.RecoveryServices/Vaults/write | Kasa Oluştur işlemi, 'vault' türünde bir Azure kaynağı oluşturur |
> | Microsoft.RecoveryServices/Vaults/backupOperations/read | Yedekleme işlemi döndürür durumu için kurtarma Hizmetleri kasası. |
> | Microsoft.RecoveryServices/Vaults/backupEngines/read | Kasaya kayıtlı tüm yedekleme yönetimi sunucularının listesini döndürür. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/* |  |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectableContainers/read | Korunabilir tüm kapsayıcıları alın |
> | Microsoft.RecoveryServices/locations/backupStatus/action | Kurtarma Hizmetleri kasaları için yedekleme durumunu denetle |
> | Microsoft.RecoveryServices/locations/backupPreValidateProtection/action |  |
> | Microsoft.RecoveryServices/locations/backupValidateFeatures/action | Özellikleri doğrulama |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/write | Uyarıyı çözümler. |
> | Microsoft.RecoveryServices/operations/read | İşlem, bir kaynak sağlayıcı için işlemlerin listesini döndürür. |
> | Microsoft.RecoveryServices/locations/operationStatus/read | Belirli bir işlemi için işlem durumunu alır |
> | Microsoft.RecoveryServices/Vaults/backupProtectionIntents/read | Tüm yedekleme koruma hedefleri listesinde |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="backup-operator"></a>Yedekleme İşleci
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Yedekleme kaldırma, kasa oluşturma ve diğer kullanıcılara erişim verme dışındaki yedekleme hizmetlerini yönetmenize olanak sağlar |
> | **Kimlik** | 00c29273-979b-4161-815c-10b084fb9324 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Network/virtualNetworks/read | Sanal ağ tanımı Al |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read | İşlemin durumunu döndürür |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/operationResults/read | Koruma Kapsayıcısı üzerinde gerçekleştirilen İşlemin sonuçlarını alır. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/backup/action | Korumalı Öğe için Yedekleme gerçekleştirir. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read | Korumalı Öğeler üzerinde Gerçekleştirilen İşlemin Sonuçlarını alın. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationsStatus/read | Korumalı Öğeler üzerinde Gerçekleştirilen İşlemin durumunu döndürür. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read | Korumalı öğenin nesne ayrıntılarını döndürür |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/provisionInstantItemRecovery/action | Korumalı öğe için anında öğe kurtarma sağla |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read | Korumalı Öğeler için Kurtarma Noktalarını alın. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/restore/action | Korumalı Öğeler için Kurtarma Noktalarını geri yükleyin. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/revokeInstantItemRecovery/action | Korumalı öğe için anında öğe kurtarmayı iptal et |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write | Yedekleme korumalı öğesi oluştur |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read | Tüm kayıtlı kapsayıcıları döndürür |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/refreshContainers/action | Kapsayıcı listesini yeniler |
> | Microsoft.RecoveryServices/Vaults/backupJobs/* | Yedekleme işler oluşturma ve yönetme |
> | Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Dışarı aktarma işleri |
> | Microsoft.RecoveryServices/Vaults/backupJobsExport/operationResults/read |  |
> | Microsoft.RecoveryServices/Vaults/backupManagementMetaData/read |  |
> | Microsoft.RecoveryServices/Vaults/backupOperationResults/* | Oluşturma ve yedekleme yönetim işlemlerinin sonuçlarını yönetme |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read | İlke İşleminin Sonuçlarını alın. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/read | Tüm koruma ilkelerini döndürür |
> | Microsoft.RecoveryServices/Vaults/backupProtectableItems/* | Oluşturma ve yedeklenebilir öğeleri yönetme |
> | Microsoft.RecoveryServices/Vaults/backupProtectedItems/read | Tüm Korumalı Öğelerin listesini döndürür. |
> | Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read | Aboneliğe ait tüm kapsayıcıları döndürür |
> | Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Özetleri için bir kurtarma Hizmetleri korumalı öğeler ve korumalı sunucuların döndürür. |
> | Microsoft.RecoveryServices/Vaults/certificates/write | Kaynak sertifikayı Güncelleştir işlemi, kaynak/kasa kimlik bilgisi sertifikasını güncelleştirir. |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/read | Daha Fazla Bilgi Al işlemi, ?vault? türündeki Azure kaynağını temsil eden nesnenin Daha Fazla Bilgi değerini alır. |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/write | Daha Fazla Bilgi Al işlemi, ?vault? türündeki Azure kaynağını temsil eden nesnenin Daha Fazla Bilgi değerini alır. |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Kurtarma Hizmetleri kasası için uyarıları alır. |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/* |  |
> | Microsoft.RecoveryServices/Vaults/read | Kasayı Al işlemi 'vault' türündeki Azure kaynağını temsil eden bir nesne alır. |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | Zaman uyumsuz olarak gönderilen işlemin sonucu ve işlem durumunu alma işlem işlemi kullanılabilir sonuçlarını Al |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Kapsayıcıları Al işlemi kullanılabilir bir kaynak için kayıtlı olan kapsayıcıları alın. |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/write | Hizmet kapsayıcısını Kaydet işlemi, bir kapsayıcıyı kurtarma Hizmeti'ne kaydolmak için kullanılabilir. |
> | Microsoft.RecoveryServices/Vaults/usages/read | Bir Kurtarma Hizmetleri Kasası için kullanım ayrıntılarını döndürür. |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Storage/storageAccounts/read | Depolama hesaplarının listesini döndürür veya belirtilen depolama hesabının özelliklerini alır. |
> | Microsoft.RecoveryServices/Vaults/backupstorageconfig/* |  |
> | Microsoft.RecoveryServices/Vaults/backupValidateOperation/action | Korumalı öğe üzerinde işlem doğrulama |
> | Microsoft.RecoveryServices/Vaults/backupOperations/read | Yedekleme işlemi döndürür durumu için kurtarma Hizmetleri kasası. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/operations/read | İlke işleminin durumunu alın. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/write | Kayıtlı bir kapsayıcı oluşturur |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/inquire/action | Bir kapsayıcı içindeki iş yükleri için sorgulama yapmak |
> | Microsoft.RecoveryServices/Vaults/backupEngines/read | Kasaya kayıtlı tüm yedekleme yönetimi sunucularının listesini döndürür. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write | Bir yedekleme koruma hedefi oluşturma |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/read | Bir yedekleme koruma hedefi Al |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectableContainers/read | Korunabilir tüm kapsayıcıları alın |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/items/read | Bir kapsayıcıdaki tüm öğeleri al |
> | Microsoft.RecoveryServices/locations/backupStatus/action | Kurtarma Hizmetleri kasaları için yedekleme durumunu denetle |
> | Microsoft.RecoveryServices/locations/backupPreValidateProtection/action |  |
> | Microsoft.RecoveryServices/locations/backupValidateFeatures/action | Özellikleri doğrulama |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/write | Uyarıyı çözümler. |
> | Microsoft.RecoveryServices/operations/read | İşlem, bir kaynak sağlayıcı için işlemlerin listesini döndürür. |
> | Microsoft.RecoveryServices/locations/operationStatus/read | Belirli bir işlemi için işlem durumunu alır |
> | Microsoft.RecoveryServices/Vaults/backupProtectionIntents/read | Tüm yedekleme koruma hedefleri listesinde |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="backup-reader"></a>Yedekleme Okuyucusu
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Yedekleme hizmetlerini görüntüleyebilir ancak değişiklik yapamaz |
> | **Kimlik** | a795c7a0-d4a2-40c1-ae25-d81f01202912 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp, hizmet tarafından kullanılan iç işlemdir. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read | İşlemin durumunu döndürür |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/operationResults/read | Koruma Kapsayıcısı üzerinde gerçekleştirilen İşlemin sonuçlarını alır. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read | Korumalı Öğeler üzerinde Gerçekleştirilen İşlemin Sonuçlarını alın. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationsStatus/read | Korumalı Öğeler üzerinde Gerçekleştirilen İşlemin durumunu döndürür. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read | Korumalı öğenin nesne ayrıntılarını döndürür |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read | Korumalı Öğeler için Kurtarma Noktalarını alın. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read | Tüm kayıtlı kapsayıcıları döndürür |
> | Microsoft.RecoveryServices/Vaults/backupJobs/operationResults/read | İş İşleminin Sonucunu döndürür. |
> | Microsoft.RecoveryServices/Vaults/backupJobs/read | Tüm iş nesnelerini döndürür |
> | Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Dışarı aktarma işleri |
> | Microsoft.RecoveryServices/Vaults/backupJobsExport/operationResults/read |  |
> | Microsoft.RecoveryServices/Vaults/backupManagementMetaData/read |  |
> | Microsoft.RecoveryServices/Vaults/backupOperationResults/read | Kurtarma Hizmetleri Kasası için Yedekleme işleminin Sonucunu döndürür. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read | İlke İşleminin Sonuçlarını alın. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/read | Tüm koruma ilkelerini döndürür |
> | Microsoft.RecoveryServices/Vaults/backupProtectedItems/read | Tüm Korumalı Öğelerin listesini döndürür. |
> | Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read | Aboneliğe ait tüm kapsayıcıları döndürür |
> | Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Özetleri için bir kurtarma Hizmetleri korumalı öğeler ve korumalı sunucuların döndürür. |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/read | Daha Fazla Bilgi Al işlemi, ?vault? türündeki Azure kaynağını temsil eden nesnenin Daha Fazla Bilgi değerini alır. |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Kurtarma Hizmetleri kasası için uyarıları alır. |
> | Microsoft.RecoveryServices/Vaults/read | Kasayı Al işlemi 'vault' türündeki Azure kaynağını temsil eden bir nesne alır. |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | Zaman uyumsuz olarak gönderilen işlemin sonucu ve işlem durumunu alma işlem işlemi kullanılabilir sonuçlarını Al |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Kapsayıcıları Al işlemi kullanılabilir bir kaynak için kayıtlı olan kapsayıcıları alın. |
> | Microsoft.RecoveryServices/Vaults/backupstorageconfig/read | Depolama yapılandırmasını döndürür kurtarma Hizmetleri kasası. |
> | Microsoft.RecoveryServices/Vaults/backupconfig/read | Yapılandırmayı döndürür kurtarma Hizmetleri kasası. |
> | Microsoft.RecoveryServices/Vaults/backupOperations/read | Yedekleme işlemi döndürür durumu için kurtarma Hizmetleri kasası. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/operations/read | İlke işleminin durumunu alın. |
> | Microsoft.RecoveryServices/Vaults/backupEngines/read | Kasaya kayıtlı tüm yedekleme yönetimi sunucularının listesini döndürür. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/read | Bir yedekleme koruma hedefi Al |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/items/read | Bir kapsayıcıdaki tüm öğeleri al |
> | Microsoft.RecoveryServices/locations/backupStatus/action | Kurtarma Hizmetleri kasaları için yedekleme durumunu denetle |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/* |  |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/write | Uyarıyı çözümler. |
> | Microsoft.RecoveryServices/operations/read | İşlem, bir kaynak sağlayıcı için işlemlerin listesini döndürür. |
> | Microsoft.RecoveryServices/locations/operationStatus/read | Belirli bir işlemi için işlem durumunu alır |
> | Microsoft.RecoveryServices/Vaults/backupProtectionIntents/read | Tüm yedekleme koruma hedefleri listesinde |
> | Microsoft.RecoveryServices/Vaults/usages/read | Bir Kurtarma Hizmetleri Kasası için kullanım ayrıntılarını döndürür. |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="billing-reader"></a>Faturalama Okuyucusu
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Faturalandırma verilerine okuma erişimi verir |
> | **Kimlik** | fa23ad8b-c56e-40d8-ac0c-ce449e1d2c64 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Billing/*/read | Faturalama bilgileri okuyun |
> | Microsoft.Commerce/*/read |  |
> | Microsoft.Consumption/*/read |  |
> | Microsoft.Management/managementGroups/read | Kimliği doğrulanmış kullanıcı için Yönetim grupları listesi. |
> | Microsoft.CostManagement/*/read |  |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="biztalk-contributor"></a>BizTalk Katılımcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | BizTalk hizmetlerini yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |
> | **Kimlik** | 5e3c6656-6cfa-4708-81fe-0de47ac73342 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.BizTalkServices/BizTalk/* | BizTalk Hizmetleri oluşturup yönetebilirsiniz |
> | Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönet |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="cdn-endpoint-contributor"></a>CDN Uç Noktası Katkıda Bulunanı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | CDN uç noktalarını yönetebilir, ancak diğer kullanıcılara erişim izni veremez. |
> | **Kimlik** | 426e0c7f-0c7e-4658-b36f-ff54d6c29b45 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Cdn/edgenodes/read |  |
> | Microsoft.Cdn/operationresults/* |  |
> | Microsoft.Cdn/profiles/endpoints/* |  |
> | Microsoft.Insights/alertRules/* | Oluşturma ve yönetme Insights uyarı kuralları |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="cdn-endpoint-reader"></a>CDN Uç Nokta Okuyucusu
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | CDN uç noktalarını görüntüleyebilir, ancak değişiklik yapamaz. |
> | **Kimlik** | 871e35f6-b5c1-49cc-a043-bde969a0f2cd |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Cdn/edgenodes/read |  |
> | Microsoft.Cdn/operationresults/* |  |
> | Microsoft.Cdn/profiles/endpoints/*/read |  |
> | Microsoft.Insights/alertRules/* | Oluşturma ve yönetme Insights uyarı kuralları |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="cdn-profile-contributor"></a>CDN Profili Katkıda Bulunanı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | CDN profillerini ve uç noktalarını yönetebilir, ancak diğer kullanıcılara erişim izni veremez. |
> | **Kimlik** | ec156ff8-a8d1-4d15-830c-5b80698ca432 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Cdn/edgenodes/read |  |
> | Microsoft.Cdn/operationresults/* |  |
> | Microsoft.Cdn/profiles/* |  |
> | Microsoft.Insights/alertRules/* | Oluşturma ve yönetme Insights uyarı kuralları |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="cdn-profile-reader"></a>CDN Profil Okuyucusu
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | CDN profillerini ve uç noktalarını görüntüleyebilir, ancak değişiklik yapamaz. |
> | **Kimlik** | 8f96442b-4075-438f-813d-ad51ab4019af |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Cdn/edgenodes/read |  |
> | Microsoft.Cdn/operationresults/* |  |
> | Microsoft.Cdn/profiles/*/read |  |
> | Microsoft.Insights/alertRules/* | Oluşturma ve yönetme Insights uyarı kuralları |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="classic-network-contributor"></a>Klasik Ağ Katılımcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Klasik ağları yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |
> | **Kimlik** | b34d265f-36f7-4a0d-a4d4-e158ca92e90f |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Yetkilendirme okuyun |
> | Microsoft.ClassicNetwork/* | Oluşturma ve klasik ağları yönetme |
> | Microsoft.Insights/alertRules/* | Oluşturma ve yönetme Insights uyarı kuralları |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="classic-storage-account-contributor"></a>Klasik Depolama Hesabı Katılımcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Klasik depolama hesaplarını yönetmenize izin verir ancak bu hesaplara erişim hakkı vermez. |
> | **Kimlik** | 86e8f5dc-a6e9-4c67-9d15-de283e8eac25 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Yetkilendirme okuyun |
> | Microsoft.ClassicStorage/storageAccounts/* | Depolama hesapları oluşturma ve yönetme |
> | Microsoft.Insights/alertRules/* | Oluşturma ve yönetme Insights uyarı kuralları |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="classic-storage-account-key-operator-service-role"></a>Klasik Depolama Hesabı Anahtarı İşleci Hizmet Rolü
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Klasik Depolama Hesabı Anahtarı İşleçlerine, Klasik Depolama Hesaplarında anahtarları listeleme ve yeniden oluşturma izni verilir |
> | **Kimlik** | 985d6b00-f706-48f5-a6fe-d0ca12fb668d |
> | **Eylemler** |  |
> | Microsoft.ClassicStorage/storageAccounts/listkeys/action | Depolama hesaplarının erişim anahtarlarını listeler. |
> | Microsoft.ClassicStorage/storageAccounts/regeneratekey/action | Depolama hesabı için var olan erişim anahtarlarını yeniden oluşturur. |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="classic-virtual-machine-contributor"></a>Klasik Sanal Makine Katılımcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Klasik sanal makineleri yönetmenizi sağlar ancak bunlara veya bağlı oldukları sanal ağ ya da depolama hesaplarına yönelik erişimi yönetme izni vermez. |
> | **Kimlik** | d73bb868-a0df-4d4d-bd69-98a00b01fccb |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Yetkilendirme okuyun |
> | Microsoft.ClassicCompute/domainNames/* | Oluşturma ve klasik işlem etki alanı adlarını yönetme |
> | Microsoft.ClassicCompute/virtualMachines/* | Sanal makineler oluşturun ve yönetin |
> | Microsoft.ClassicNetwork/networkSecurityGroups/join/action |  |
> | Microsoft.ClassicNetwork/reservedIps/link/action | Ayrılmış bir IP'ye bağlantı oluşturun |
> | Microsoft.ClassicNetwork/reservedIps/read | Ayrılmış IP'leri alır |
> | Microsoft.ClassicNetwork/virtualNetworks/join/action | Sanal ağa katılır. |
> | Microsoft.ClassicNetwork/virtualNetworks/read | Sanal ağı alın. |
> | Microsoft.ClassicStorage/storageAccounts/disks/read | Depolama hesabı diskini döndürür. |
> | Microsoft.ClassicStorage/storageAccounts/images/read | Depolama hesabı görüntüsünü döndürür. (Kullanım dışı. 'Microsoft.ClassicStorage/storageAccounts/vmImages' kullanın) |
> | Microsoft.ClassicStorage/storageAccounts/listKeys/action | Depolama hesaplarının erişim anahtarlarını listeler. |
> | Microsoft.ClassicStorage/storageAccounts/read | Belirli bir hesaba yönelik depolama hesabını döndürün. |
> | Microsoft.Insights/alertRules/* | Oluşturma ve yönetme Insights uyarı kuralları |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="cognitive-services-contributor"></a>Bilişsel Hizmetler Katılımcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Bilişsel Hizmetler anahtarlarını oluşturmanıza, okumanıza, güncelleştirmenize, silmenize ve yönetmenize olanak sağlar. |
> | **Kimlik** | 25fbc0a9-bd7c-42a3-aa1a-3b75d497ee68 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.CognitiveServices/* |  |
> | Microsoft.Features/features/read | Bir aboneliğin özelliklerini alır. |
> | Microsoft.Features/providers/features/read | Belirli bir kaynak sağlayıcısındaki bir abonelik özelliğini alır. |
> | Microsoft.Insights/alertRules/* | Oluşturma ve yönetme Insights uyarı kuralları |
> | Microsoft.Insights/diagnosticSettings/* | Analiz sunucusu için tanılama ayarını okur, güncelleştirir veya oluşturur |
> | Microsoft.Insights/logDefinitions/read | Günlük tanımlarını oku |
> | Microsoft.Insights/metricdefinitions/read | Ölçüm tanımlarını oku |
> | Microsoft.Insights/metrics/read | Ölçümleri okuma |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Resources/deployments/operations/read | Dağıtım işlemlerini alır veya listeler. |
> | Microsoft.Resources/subscriptions/operationresults/read | Abonelik işlem sonuçlarını alır. |
> | Microsoft.Resources/subscriptions/read | Aboneliklerin listesini alır. |
> | Microsoft.Resources/subscriptions/resourcegroups/deployments/* |  |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="cognitive-services-data-reader-preview"></a>Bilişsel Hizmetler Veri Okuyucusu (Önizleme)
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Bilişsel Hizmetler verilerini okumanıza olanak sağlar. |
> | **Kimlik** | b59867f0-fa02-499b-be73-45a86b5b3e1c |
> | **Eylemler** |  |
> | *yok* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | Microsoft.CognitiveServices/*/read |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="cognitive-services-user"></a>Bilişsel Hizmetler Kullanıcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Bilişsel Hizmetler anahtarlarını okumanıza ve listelemenize olanak sağlar. |
> | **Kimlik** | a97b65f3-24c7-4388-baec-2e87135dc908 |
> | **Eylemler** |  |
> | Microsoft.CognitiveServices/*/read |  |
> | Microsoft.CognitiveServices/accounts/listkeys/action | Anahtarları Listele |
> | Microsoft.Insights/alertRules/read | Bir klasik ölçüm uyarısını okuyun |
> | Microsoft.Insights/diagnosticSettings/read | Bir kaynak tanılama ayarını okuyun |
> | Microsoft.Insights/logDefinitions/read | Günlük tanımlarını oku |
> | Microsoft.Insights/metricdefinitions/read | Ölçüm tanımlarını oku |
> | Microsoft.Insights/metrics/read | Ölçümleri okuma |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/operations/read | Dağıtım işlemlerini alır veya listeler. |
> | Microsoft.Resources/subscriptions/operationresults/read | Abonelik işlem sonuçlarını alır. |
> | Microsoft.Resources/subscriptions/read | Aboneliklerin listesini alır. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | Microsoft.CognitiveServices/* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="cosmos-db-account-reader-role"></a>Cosmos DB Hesabı Okuyucusu Rolü
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Azure Cosmos DB hesabı verileri okuyabilir. Bkz: [DocumentDB hesabı Katılımcısı](#documentdb-account-contributor) Azure Cosmos DB hesapları yönetme. |
> | **Kimlik** | fbdf93bf-df7d-467e-a4d2-9458aa1360c8 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları, her kullanıcıya verilen izinler okuyabilir |
> | Microsoft.DocumentDB/*/read | Herhangi bir koleksiyon okuyun |
> | Microsoft.DocumentDB/databaseAccounts/readonlykeys/action | Veritabanı hesabı salt okunur anahtarları okur. |
> | Microsoft.Insights/MetricDefinitions/read | Ölçüm tanımlarını oku |
> | Microsoft.Insights/Metrics/read | Ölçümleri okuma |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="cosmosbackupoperator"></a>CosmosBackupOperator
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Bir Cosmos DB veritabanı veya bir hesaba ilişkin bir kapsayıcı için geri yükleme isteği gönderebilir |
> | **Kimlik** | db7b14f2-5adf-42da-9f96-f2ee17bab5cb |
> | **Eylemler** |  |
> | Microsoft.DocumentDB/databaseAccounts/backup/action | Yedeklemeyi yapılandırmak için bir istek gönderin |
> | Microsoft.DocumentDB/databaseAccounts/restore/action | Bir geri yükleme isteği gönderin |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="cost-management-contributor"></a>Maliyet Yönetimi Katkıda Bulunanı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Maliyetleri görüntüleyebilir ve maliyet yapılandırmasını yönetebilir (örneğin bütçeler, dışarı aktarmalar) |
> | **Kimlik** | 434105ed-43f6-45c7-a02f-909b2ba83430 |
> | **Eylemler** |  |
> | Microsoft.Consumption/* |  |
> | Microsoft.CostManagement/* |  |
> | Microsoft.Billing/billingPeriods/read | Kullanılabilir faturalama dönemleri listeler |
> | Microsoft.Resources/subscriptions/read | Aboneliklerin listesini alır. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="cost-management-reader"></a>Maliyet Yönetimi Okuyucusu
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Maliyet verileri ve yapılandırmasını (örneğin bütçeler, dışarı aktarmalar) görüntüleyebilir |
> | **Kimlik** | 72fafb9e-0641-4937-9268-a91bfd8191a3 |
> | **Eylemler** |  |
> | Microsoft.Consumption/*/read |  |
> | Microsoft.CostManagement/*/read |  |
> | Microsoft.Billing/billingPeriods/read | Kullanılabilir faturalama dönemleri listeler |
> | Microsoft.Resources/subscriptions/read | Aboneliklerin listesini alır. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="data-box-contributor"></a>Veri Kutusu Katılımcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Başkalarına erişim izni verme hariç Veri Kutusu Hizmeti altındaki her şeyi yönetmenize olanak sağlar. |
> | **Kimlik** | add466c9-e687-43fc-8d98-dfcf8d720be5 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | Microsoft.Databox/* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="data-box-reader"></a>Veri Kutusu Okuyucusu
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Sipariş oluşturma veya sipariş ayrıntılarını düzenleme ve başkalarına erişim izni verme hariç Veri Kutusu Hizmeti özelliklerini yönetmenize olanak sağlar. |
> | **Kimlik** | 028f4ed7-e2a9-465e-a8f4-9c0ffdfdc027 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Databox/*/read |  |
> | Microsoft.Databox/jobs/listsecrets/action |  |
> | Microsoft.Databox/jobs/listcredentials/action | Siparişle ilgili şifrelenmemiş kimlik bilgilerini listeler. |
> | Microsoft.Databox/locations/availableSkus/action | Bu yöntem, kullanılabilen SKU'ların listesini döndürür. |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="data-factory-contributor"></a>Data Factory Katılımcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Veri fabrikaları ile birlikte bunların alt kaynaklarını oluşturup yönetin. |
> | **Kimlik** | 673868aa-7521-48a0-acc6-0f60742d39f5 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.DataFactory/dataFactories/* | Oluşturun ve veri fabrikaları ve bunların alt kaynaklarını yönetin. |
> | Microsoft.DataFactory/factories/* | Oluşturun ve veri fabrikaları ve bunların alt kaynaklarını yönetin. |
> | Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönet |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="data-lake-analytics-developer"></a>Data Lake Analytics Geliştiricisi
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | İşlerinizi göndermenize, izlemenize ve yönetmenize izin verir, ancak Data Lake Analytics hesabı oluşturmanıza veya silmenize izin vermez. |
> | **Kimlik** | 47b7735b-770e-4598-a7da-8b91488b4c88 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.BigAnalytics/accounts/* |  |
> | Microsoft.DataLakeAnalytics/accounts/* |  |
> | Microsoft.Insights/alertRules/* | Oluşturma ve yönetme Insights uyarı kuralları |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | Microsoft.BigAnalytics/accounts/Delete |  |
> | Microsoft.BigAnalytics/accounts/TakeOwnership/action |  |
> | Microsoft.BigAnalytics/accounts/Write |  |
> | Microsoft.DataLakeAnalytics/accounts/Delete | DataLakeAnalytics hesabı silin. |
> | Microsoft.DataLakeAnalytics/accounts/TakeOwnership/action | Diğer kullanıcılar tarafından gönderilen bir iş iptal etmek için izinleri verin. |
> | Microsoft.DataLakeAnalytics/accounts/Write | Veya bir DataLakeAnalytics hesabı güncelleştirilemiyor. |
> | Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/Write | Veya DataLakeAnalytics hesabın bağlı DataLakeStore hesabı güncelleştirilemiyor. |
> | Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/Delete | Bir DataLakeAnalytics hesabından DataLakeStore hesabı bağlantısını. |
> | Microsoft.DataLakeAnalytics/accounts/storageAccounts/Write | Veya bağlı bir depolama hesabı DataLakeAnalytics hesabının güncelleştirilemiyor. |
> | Microsoft.DataLakeAnalytics/accounts/storageAccounts/Delete | Bir depolama hesabından bir DataLakeAnalytics hesabı bağlantısını. |
> | Microsoft.DataLakeAnalytics/accounts/firewallRules/Write | Veya bir güvenlik duvarı kuralı güncelleştirilemiyor. |
> | Microsoft.DataLakeAnalytics/accounts/firewallRules/Delete | Bir güvenlik duvarı kuralını silin. |
> | Microsoft.DataLakeAnalytics/accounts/computePolicies/Write | Veya bir işlem İlkesi güncelleştirilemiyor. |
> | Microsoft.DataLakeAnalytics/accounts/computePolicies/Delete | Bir işlem ilkeyi silin. |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="data-purger"></a>Veri Temizleyici
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Analiz verilerini temizleyebilir |
> | **Kimlik** | 150f5e0c-0603-4f03-8c7f-cf70034c4e90 |
> | **Eylemler** |  |
> | Microsoft.Insights/components/*/read |  |
> | Microsoft.Insights/components/purge/action | Application Insights verilerini temizleme |
> | Microsoft.OperationalInsights/workspaces/*/read |  |
> | Microsoft.OperationalInsights/workspaces/purge/action | Belirtilen veriyi çalışma alanından silin |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="devtest-labs-user"></a>DevTest Labs Kullanıcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Azure DevTest Labs'teki tüm sanal makinelerinize bağlanmanıza, bu makineleri başlatmanıza, yeniden başlatmanıza ve kapatmanıza izin verir. |
> | **Kimlik** | 76283e04-6283-4c54-8f91-bcf1374a3c64 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Compute/availabilitySets/read | Bir kullanılabilirlik kümesinin özelliklerini al |
> | Microsoft.Compute/virtualMachines/*/read | Bir sanal makine (VM boyutları, çalışma zamanı durumu, VM uzantıları, vb.) özelliklerini okuyun |
> | Microsoft.Compute/virtualMachines/deallocate/action | Sanal makineyi kapatır ve işlem kaynaklarını serbest bırakır |
> | Microsoft.Compute/virtualMachines/read | Bir sanal makinenin özelliklerini alır |
> | Microsoft.Compute/virtualMachines/restart/action | Sanal makineyi yeniden başlatır |
> | Microsoft.Compute/virtualMachines/start/action | Sanal makineyi başlatır |
> | Microsoft.DevTestLab/*/read | Bir laboratuvar özelliklerini okuyun |
> | Microsoft.DevTestLab/labs/claimAnyVm/action | Rastgele bir talep edilebilir sanal makine Laboratuvardaki talep edin. |
> | Microsoft.DevTestLab/labs/createEnvironment/action | Sanal makineleri bir laboratuar ortamında oluşturun. |
> | Microsoft.DevTestLab/labs/formulas/delete | Formülleri silin. |
> | Microsoft.DevTestLab/labs/formulas/read | Formülleri okuyun. |
> | Microsoft.DevTestLab/labs/formulas/write | Ekleyebilir veya formülleri değiştirebilirsiniz. |
> | Microsoft.DevTestLab/labs/policySets/evaluatePolicies/action | Laboratuvar ilkeyi değerlendirir. |
> | Microsoft.DevTestLab/labs/virtualMachines/claim/action | Mevcut bir sanal makinenin sahipliğini al |
> | Microsoft.DevTestLab/labs/virtualmachines/listApplicableSchedules/action | Varsa geçerli Başlat/Durdur zamanlamaları listeler. |
> | Microsoft.DevTestLab/labs/virtualMachines/getRdpFileContents/action | Sanal makine için RDP dosyasının içeriğini temsil eden bir dize alır |
> | Microsoft.Network/loadBalancers/backendAddressPools/join/action | Bir yük dengeleyici arka uç adres havuzu birleştirir. Alertable değil. |
> | Microsoft.Network/loadBalancers/inboundNatRules/join/action | Bir yük dengeleyici gelen nat kuralı birleştirir. Alertable değil. |
> | Microsoft.Network/networkInterfaces/*/read | Bir ağ arabirimi (ağ arabiriminin bir parçası olduğu gibi tüm yük Dengeleyiciler) özelliklerini okuyun |
> | Microsoft.Network/networkInterfaces/join/action | Bir sanal makineye bir ağ arabirimi ile birleştirir. Alertable değil. |
> | Microsoft.Network/networkInterfaces/read | Bir ağ arabirimi tanımı alır.  |
> | Microsoft.Network/networkInterfaces/write | Bir ağ arabirimi oluşturur veya mevcut bir ağ arabirimi güncelleştirir.  |
> | Microsoft.Network/publicIPAddresses/*/read | Genel bir IP adresinin özelliklerini okuyun |
> | Microsoft.Network/publicIPAddresses/join/action | Bir genel IP adresi birleştirir. Alertable değil. |
> | Microsoft.Network/publicIPAddresses/read | Bir genel IP adresi tanımı alır. |
> | Microsoft.Network/virtualNetworks/subnets/join/action | Bir sanal ağa katılır. Alertable değil. |
> | Microsoft.Resources/deployments/operations/read | Dağıtım işlemlerini alır veya listeler. |
> | Microsoft.Resources/deployments/read | Dağıtımları alır veya listeler. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Storage/storageAccounts/listKeys/action | Belirtilen depolama hesabının erişim anahtarlarını döndürür. |
> | **NotActions** |  |
> | Microsoft.Compute/virtualMachines/vmSizes/read | Sanal makineyi güncelleştirmek için kullanılabilir boyutları listeler |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="dns-zone-contributor"></a>DNS Bölgesi Katkıda Bulunanı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Azure DNS'te, DNS bölgelerini ve kayıt kümelerini yönetmenize izin verir, ancak bunlara kimlerin erişebildiğini denetlemenize izin vermez. |
> | **Kimlik** | befefa01-2a29-4197-83a8-272ff33ce314 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönet |
> | Microsoft.Network/dnsZones/* | DNS bölgeleri ve kayıtları oluşturma ve yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="documentdb-account-contributor"></a>DocumentDB Hesabı Katılımcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Azure Cosmos DB hesapları yönetebilirsiniz. Azure Cosmos DB, eski adıyla DocumentDB bilinir. |
> | **Kimlik** | 5bd9cd88-fe45-4216-938b-f97437e15450 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.DocumentDb/databaseAccounts/* | Azure Cosmos DB hesapları oluşturma ve yönetme |
> | Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönet |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="eventgrid-eventsubscription-contributor"></a>EventGrid EventSubscription Katkıda Bulunanı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | EventGrid olay aboneliği işlemlerini yönetmenize olanak sağlar. |
> | **Kimlik** | 428e0ff0-5e57-4d9c-a221-2c70d0e0a443 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.EventGrid/eventSubscriptions/* |  |
> | Microsoft.EventGrid/topicTypes/eventSubscriptions/read | Genel olay abonelikleri konu türüne göre listeleme |
> | Microsoft.EventGrid/locations/eventSubscriptions/read | Bölgesel olay abonelikleri listesi |
> | Microsoft.EventGrid/locations/topicTypes/eventSubscriptions/read | Tarafından topictype bölgesel olay abonelikleri listesi |
> | Microsoft.Insights/alertRules/* | Oluşturma ve yönetme Insights uyarı kuralları |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="eventgrid-eventsubscription-reader"></a>EventGrid EventSubscription Okuyucusu
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | EventGrid olay aboneliklerini okumanıza olanak sağlar. |
> | **Kimlik** | 2414bbcf-6497-4faf-8c65-045460748405 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.EventGrid/eventSubscriptions/read | Bir eventSubscription okuyun |
> | Microsoft.EventGrid/topicTypes/eventSubscriptions/read | Genel olay abonelikleri konu türüne göre listeleme |
> | Microsoft.EventGrid/locations/eventSubscriptions/read | Bölgesel olay abonelikleri listesi |
> | Microsoft.EventGrid/locations/topicTypes/eventSubscriptions/read | Tarafından topictype bölgesel olay abonelikleri listesi |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="hdinsight-domain-services-contributor"></a>HDInsight Etki Alanı Hizmetleri Katılımcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Okuyabilir oluşturma, değiştirme ve etki alanı Hizmetleri Sil ilgili işlemler için HDInsight Kurumsal Güvenlik Paketi için gereken Etki Alanı Hizmetleriyle ilgili işlemleri Okuyabilir, Oluşturabilir, Değiştirebilir ve Silebilir |
> | **Kimlik** | 8d8d5a11-05d3-4bda-a417-a08778121c7c |
> | **Eylemler** |  |
> | Microsoft.AAD/*/read |  |
> | Microsoft.AAD/domainServices/*/read |  |
> | Microsoft.AAD/domainServices/oucontainer/* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="intelligent-systems-account-contributor"></a>Akıllı Sistemler Hesap Katılımcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Akıllı Sistemler hesaplarını yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |
> | **Kimlik** | 03a6d094-3444-4b3d-88af-7477090a9e5e |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönet |
> | Microsoft.IntelligentSystems/accounts/* | Akıllı sistemler hesapları oluşturma ve yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="key-vault-contributor"></a>Key Vault Katkıda Bulunanı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Anahtar kasalarını yönetmenize izin verir, ancak bunlara erişmenize izin vermez. |
> | **Kimlik** | f25e0fa2-a7c8-4377-a976-54943a77a395 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Insights/alertRules/* | Oluşturma ve yönetme Insights uyarı kuralları |
> | Microsoft.KeyVault/* |  |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | Microsoft.KeyVault/locations/deletedVaults/purge/action | Geçici silinen bir anahtar kasasını temizleyin |
> | Microsoft.KeyVault/hsmPools/* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="lab-creator"></a>Laboratuvar Oluşturan
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Azure Laboratuvar Hesaplarınız altında yönetilen laboratuvarları oluşturmanıza, yönetmenize ve silmenize olanak sağlar. |
> | **Kimlik** | b97fb8bc-a8b2-4522-a38b-dd33c7e65ead |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.LabServices/labAccounts/*/read |  |
> | Microsoft.LabServices/labAccounts/createLab/action | Bir laboratuvar bir laboratuvar hesabı oluşturun. |
> | Microsoft.LabServices/labAccounts/sizes/getRegionalAvailability/action |  |
> | Microsoft.LabServices/labAccounts/getRegionalAvailability/action | Bir laboratuvar hesabı altında yapılandırılmış her boyutu kategori bölgesel kullanılabilirliği hakkında bilgi alın |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="log-analytics-contributor"></a>Log Analytics Katkıda Bulunan
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Log Analytics katkıda bulunan tüm izleme verilerini okuyabilir ve izleme ayarlarını düzenleyin. İzleme ayarlarını düzenleme Vm'lere VM uzantısı ekleme içerir; Azure Depolama'dan günlüklerin toplanmasını yapılandırma yapabilmek için depolama hesabı anahtarlarını okuma; oluşturma ve Otomasyon hesapları yapılandırma; çözümler eklenerek; ve tüm Azure kaynaklarında Azure tanılamayı yapılandırma. |
> | **Kimlik** | 92aaf0da-9dab-42b6-94a3-d43ce8d16293 |
> | **Eylemler** |  |
> | * / Okuma | Gizli dizileri dışında tüm türler kaynakları okuyun. |
> | Microsoft.Automation/automationAccounts/* |  |
> | Microsoft.ClassicCompute/virtualMachines/extensions/* |  |
> | Microsoft.ClassicStorage/storageAccounts/listKeys/action | Depolama hesaplarının erişim anahtarlarını listeler. |
> | Microsoft.Compute/virtualMachines/extensions/* |  |
> | Microsoft.Insights/alertRules/* | Oluşturma ve yönetme Insights uyarı kuralları |
> | Microsoft.Insights/diagnosticSettings/* | Analiz sunucusu için tanılama ayarını okur, güncelleştirir veya oluşturur |
> | Microsoft.OperationalInsights/* |  |
> | Microsoft.OperationsManagement/* |  |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Resources/subscriptions/resourcegroups/deployments/* |  |
> | Microsoft.Storage/storageAccounts/listKeys/action | Belirtilen depolama hesabının erişim anahtarlarını döndürür. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="log-analytics-reader"></a>Log Analytics Okuyucusu
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Log Analytics Okuyucusu, tüm izleme verilerinin görüntüleme ve aramanın yanı sıra izleme ayarlarını da (tüm Azure kaynaklarındaki Azure tanılama yapılandırmalarını görüntüleme dahil) görüntüleyebilir. |
> | **Kimlik** | 73c42c96-874c-492b-b04d-ab87d138a893 |
> | **Eylemler** |  |
> | * / Okuma | Gizli dizileri dışında tüm türler kaynakları okuyun. |
> | Microsoft.OperationalInsights/workspaces/analytics/query/action | Yeni altyapıyı kullanarak arama yapın. |
> | Microsoft.OperationalInsights/workspaces/search/action | Arama sorgusu yürütür |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | Microsoft.OperationalInsights/workspaces/sharedKeys/read | Çalışma alanı paylaşılan anahtarlarını alır. Bu anahtarlar, Microsoft operasyonel İçgörüler aracılarını çalışma alanına bağlamak için kullanılır. |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="logic-app-contributor"></a>Mantıksal Uygulama Katkıda Bulunanı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Mantıksal uygulamayı yönetmenize izin verir, ancak bunlara yönelik erişimi yönetmenize izin vermez. |
> | **Kimlik** | 87a39d53-fc1b-424a-814c-f7e04687dc9e |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.ClassicStorage/storageAccounts/listKeys/action | Depolama hesaplarının erişim anahtarlarını listeler. |
> | Microsoft.ClassicStorage/storageAccounts/read | Belirli bir hesaba yönelik depolama hesabını döndürün. |
> | Microsoft.Insights/alertRules/* | Oluşturma ve yönetme Insights uyarı kuralları |
> | Microsoft.Insights/diagnosticSettings/* | Analiz sunucusu için tanılama ayarını okur, güncelleştirir veya oluşturur |
> | Microsoft.Insights/logdefinitions/* | Bu izin, portal aracılığıyla etkinlik günlüklerine erişmek isteyen kullanıcılar için gereklidir. Etkinlik günlüğünde günlük kategorileri listesi. |
> | Microsoft.Insights/metricDefinitions/* | Okunan ölçüm tanımları (bir kaynak için ölçüm kullanılabilir türler listesi). |
> | Microsoft.Logic/* | Logic Apps kaynaklarını yönetir. |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Resources/subscriptions/operationresults/read | Abonelik işlem sonuçlarını alır. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Storage/storageAccounts/listkeys/action | Belirtilen depolama hesabının erişim anahtarlarını döndürür. |
> | Microsoft.Storage/storageAccounts/read | Depolama hesaplarının listesini döndürür veya belirtilen depolama hesabının özelliklerini alır. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | Microsoft.Web/connectionGateways/* | Oluşturma ve bir bağlantı ağ geçidi yönetir. |
> | Microsoft.Web/connections/* | Oluşturma ve bir bağlantı yönetir. |
> | Microsoft.Web/customApis/* | Oluşturur ve özel bir API yönetir. |
> | Microsoft.Web/serverFarms/join/action |  |
> | Microsoft.Web/serverFarms/read | Bir App Service planı üzerinde özelliklerini alma |
> | Microsoft.Web/sites/functions/listSecrets/action | Liste gizli dizileri Web Apps işlevleri. |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="logic-app-operator"></a>Mantıksal Uygulama Operatörü
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Mantıksal uygulamayı okumanıza, etkinleştirmenize ve devre dışı bırakmanıza izin verir. |
> | **Kimlik** | 515c2055-d9d4-4321-b1b9-bd0c9a0f79fe |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Insights/alertRules/*/read | Okuma Insights uyarı kuralları |
> | Microsoft.Insights/diagnosticSettings/*/read | Logic Apps için tanılama ayarları alır |
> | Microsoft.Insights/metricDefinitions/*/read | Logic Apps için kullanılabilir ölçümleri alır. |
> | Microsoft.Logic/*/read | Logic Apps kaynaklarını okur. |
> | Microsoft.Logic/workflows/disable/action | İş akışını devre dışı bırakır. |
> | Microsoft.Logic/workflows/enable/action | İş akışını etkinleştirir. |
> | Microsoft.Logic/workflows/validate/action | İş akışını doğrular. |
> | Microsoft.Resources/deployments/operations/read | Dağıtım işlemlerini alır veya listeler. |
> | Microsoft.Resources/subscriptions/operationresults/read | Abonelik işlem sonuçlarını alır. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | Microsoft.Web/connectionGateways/*/read | Bağlantı ağ geçidi'ni okuyun. |
> | Microsoft.Web/connections/*/read | Bağlantıları okuyun. |
> | Microsoft.Web/customApis/*/read | Özel API okuyun. |
> | Microsoft.Web/serverFarms/read | Bir App Service planı üzerinde özelliklerini alma |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="managed-application-operator-role"></a>Yönetilen Uygulama İşletmeni Rolü
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Yönetilen Uygulama kaynakları üzerinde eylemleri okumanıza ve gerçekleştirmenize olanak sağlar |
> | **Kimlik** | c7393b34-138c-406f-901b-d8cf2b17e6ae |
> | **Eylemler** |  |
> | * / Okuma | Gizli dizileri dışında tüm türler kaynakları okuyun. |
> | Microsoft.Solutions/applications/read | Uygulama listesi alır. |
> | Microsoft.Solutions/*/action |  |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="managed-applications-reader"></a>Yönetilen Uygulamalar Okuyucusu
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Yönetilen bir uygulama içinde kaynakları okumanıza ve JIT erişimi isteğinde bulunmanıza olanak sağlar. |
> | **Kimlik** | b9331d33-8a36-4f8c-b097-4f54124fdb44 |
> | **Eylemler** |  |
> | * / Okuma | Gizli dizileri dışında tüm türler kaynakları okuyun. |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Solutions/jitRequests/* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="managed-identity-contributor"></a>Yönetilen Kimlik Katılımcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Kullanıcı Tarafından Atanan Kimliği Oluşturma, Okuma, Güncelleştirme ve Silme |
> | **Kimlik** | e40ec5ca-96e0-45a2-b4ff-59039f2c2b59 |
> | **Eylemler** |  |
> | Microsoft.ManagedIdentity/userAssignedIdentities/*/read |  |
> | Microsoft.ManagedIdentity/userAssignedIdentities/*/write |  |
> | Microsoft.ManagedIdentity/userAssignedIdentities/*/delete |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Insights/alertRules/* | Oluşturma ve yönetme Insights uyarı kuralları |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="managed-identity-operator"></a>Yönetilen Kimlik İşleci
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Kullanıcı Tarafından Atanan Kimliği Oku ve Ata |
> | **Kimlik** | f1a07417-d97a-45cb-824c-7a7467783830 |
> | **Eylemler** |  |
> | Microsoft.ManagedIdentity/userAssignedIdentities/*/read |  |
> | Microsoft.ManagedIdentity/userAssignedIdentities/*/assign/action |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Insights/alertRules/* | Oluşturma ve yönetme Insights uyarı kuralları |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="management-group-contributor"></a>Yönetim Grubu Katkıda Bulunanı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Yönetim Grubu Katkıda Bulunanı Rolü |
> | **Kimlik** | 5d58bcaf-24a5-4b20-bdb6-eed9f69fbe4c |
> | **Eylemler** |  |
> | Microsoft.Management/managementGroups/delete | Yönetim grubunu silin. |
> | Microsoft.Management/managementGroups/read | Kimliği doğrulanmış kullanıcı için Yönetim grupları listesi. |
> | Microsoft.Management/managementGroups/subscriptions/delete | Abonelik yönetim grubundan serbest ilişkilendirir. |
> | Microsoft.Management/managementGroups/subscriptions/write | Mevcut yönetim grubuyla abonelik ilişkilendirir. |
> | Microsoft.Management/managementGroups/write | Veya bir yönetim grubu güncelleştirilemiyor. |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="management-group-reader"></a>Yönetim Grubu Okuyucusu
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Yönetim Grubu Okuyucusu Rolü |
> | **Kimlik** | ac63b705-f282-497d-ac71-919bf39d939d |
> | **Eylemler** |  |
> | Microsoft.Management/managementGroups/read | Kimliği doğrulanmış kullanıcı için Yönetim grupları listesi. |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="monitoring-contributor"></a>İzleme Katkıda Bulunanı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Tüm izleme verilerini okuyabilir ve izleme ayarlarını düzenleyin. Ayrıca bkz: [Azure İzleyici ile güvenlik rolleri ve izinleri ile çalışmaya başlama](../azure-monitor/platform/roles-permissions-security.md#built-in-monitoring-roles). |
> | **Kimlik** | 749f88d5-cbae-40b8-bcfc-e573ddc772fa |
> | **Eylemler** |  |
> | * / Okuma | Gizli dizileri dışında tüm türler kaynakları okuyun. |
> | Microsoft.AlertsManagement/alerts/* |  |
> | Microsoft.AlertsManagement/alertsSummary/* |  |
> | Microsoft.Insights/actiongroups/* |  |
> | Microsoft.Insights/activityLogAlerts/* |  |
> | Microsoft.Insights/AlertRules/* | Okuma/yazma/silme uyarı kuralları. |
> | Microsoft.Insights/components/* | Application Insights bileşenlerini okuma/yazma/silme. |
> | Microsoft.Insights/DiagnosticSettings/* | Tanılama ayarlarını okuma/yazma/silme. |
> | Microsoft.Insights/eventtypes/* | Bir abonelikte etkinlik günlüğü olayları (Yönetim olayları) listeler. Bu izin, Etkinlik günlüğünü programlı ve portal erişimi için geçerlidir. |
> | Microsoft.Insights/LogDefinitions/* | Bu izin, portal aracılığıyla etkinlik günlüklerine erişmek isteyen kullanıcılar için gereklidir. Etkinlik günlüğünde günlük kategorileri listesi. |
> | Microsoft.Insights/metricalerts/* |  |
> | Microsoft.Insights/MetricDefinitions/* | Okunan ölçüm tanımları (bir kaynak için ölçüm kullanılabilir türler listesi). |
> | Microsoft.Insights/Metrics/* | Bir kaynak için ölçüm okuyun. |
> | Microsoft.Insights/Register/Action | Microsoft Insights sağlayıcısını kaydedin |
> | Microsoft.Insights/scheduledqueryrules/* |  |
> | Microsoft.Insights/webtests/* | Web testleri okuma/yazma/silme Application Insights. |
> | Microsoft.OperationalInsights/workspaces/intelligencepacks/* | Okuma/yazma/silme log analytics çözüm paketleri. |
> | Microsoft.OperationalInsights/workspaces/savedSearches/* | Okuma/yazma/silme log analytics kayıtlı aramalar. |
> | Microsoft.OperationalInsights/workspaces/search/action | Arama sorgusu yürütür |
> | Microsoft.OperationalInsights/workspaces/sharedKeys/action | Çalışma alanı paylaşılan anahtarlarını alır. Bu anahtarlar, Microsoft operasyonel İçgörüler aracılarını çalışma alanına bağlamak için kullanılır. |
> | Microsoft.OperationalInsights/workspaces/storageinsightconfigs/* | Okuma/yazma/silme log analytics depolama Insight yapılandırmaları. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | Microsoft.WorkloadMonitor/monitors/* |  |
> | Microsoft.WorkloadMonitor/notificationSettings/* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="monitoring-metrics-publisher"></a>İzleme Ölçüm Yayımcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Azure kaynaklarına karşı ölçümleri yayımlamayı etkinleştirir |
> | **Kimlik** | 3913510d-42f4-4e42-8a64-420c390055eb |
> | **Eylemler** |  |
> | Microsoft.Insights/Register/Action | Microsoft Insights sağlayıcısını kaydedin |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | Microsoft.Insights/Metrics/Write | Ölçümleri yaz |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="monitoring-reader"></a>İzleme Okuyucusu
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | (Ölçümler, günlükler, vb.) tüm izleme verilerini okuyabilir. Ayrıca bkz: [Azure İzleyici ile güvenlik rolleri ve izinleri ile çalışmaya başlama](../azure-monitor/platform/roles-permissions-security.md#built-in-monitoring-roles). |
> | **Kimlik** | 43d0d8ad-25c7-4714-9337-8ba259a9fe05 |
> | **Eylemler** |  |
> | * / Okuma | Gizli dizileri dışında tüm türler kaynakları okuyun. |
> | Microsoft.OperationalInsights/workspaces/search/action | Arama sorgusu yürütür |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="network-contributor"></a>Ağ Katılımcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Ağları yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |
> | **Kimlik** | 4d97b98b-1d4f-4787-a291-c67834d212e7 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönet |
> | Microsoft.Network/* | Oluşturma ve ağları yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="new-relic-apm-account-contributor"></a>New Relic APM Hesap Katılımcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | New Relic Application Performance Management hesaplarını ve uygulamalarını yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |
> | **Kimlik** | 5d28c62d-5b37-4476-8438-e587778df237 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Insights/alertRules/* | Oluşturma ve yönetme Insights uyarı kuralları |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | NewRelic.APM/accounts/* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="reader-and-data-access"></a>Okuyucu ve Veri Erişimi
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Sağlayan her şeyi görüntüleyebilir, ancak bir depolama hesabı ya da kapsanan kaynak oluşturma veya silemezsiniz izin vermez. Depolama hesabı anahtarları erişimi üzerinden bir depolama hesabında kapsanan tüm verilere okuma/yazma erişimi de izin verir. |
> | **Kimlik** | c12c1c16-33a1-487b-954d-41c89c60f349 |
> | **Eylemler** |  |
> | Microsoft.Storage/storageAccounts/listKeys/action | Belirtilen depolama hesabının erişim anahtarlarını döndürür. |
> | Microsoft.Storage/storageAccounts/read | Depolama hesaplarının listesini döndürür veya belirtilen depolama hesabının özelliklerini alır. |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="redis-cache-contributor"></a>Redis Cache Katılımcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Redis Cache'leri yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |
> | **Kimlik** | e0f68234-74aa-48ed-b826-c38b57376e17 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Cache/redis/* | Oluşturma ve yönetme Redis önbellekleri |
> | Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönet |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="resource-policy-contributor-preview"></a>Kaynak İlkesine Katkıda Bulunan (Önizleme)
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | (Önizleme) Kaynak ilkesi oluşturma/değiştirme, destek bileti oluşturma ve kaynakları/hiyerarşiyi okuma haklarıyla EA’dan kullanıcılar geri dolduruldu. |
> | **Kimlik** | 36243c78-bf99-498c-9df9-86d9f8d28608 |
> | **Eylemler** |  |
> | * / Okuma | Gizli dizileri dışında tüm türler kaynakları okuyun. |
> | Microsoft.Authorization/policyassignments/* | Oluşturma ve ilke atamalarını yönetme |
> | Microsoft.Authorization/policydefinitions/* | Oluşturma ve yönetme ilke tanımları |
> | Microsoft.Authorization/policysetdefinitions/* | Oluşturma ve ilke kümelerini Yönet |
> | Microsoft.PolicyInsights/* |  |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="scheduler-job-collections-contributor"></a>Zamanlayıcı İş Koleksiyonları Katılımcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Zamanlayıcı iş koleksiyonlarını yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |
> | **Kimlik** | 188a0f2f-5c9e-469b-ae67-2aa5ce574b94 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönet |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Scheduler/jobcollections/* | Oluşturma ve iş koleksiyonları yönetme |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="search-service-contributor"></a>Search Hizmeti Katılımcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Search hizmetlerini yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |
> | **Kimlik** | 7ca78c08-252a-4471-8644-bb5ff32d4ba0 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönet |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Search/searchServices/* | Arama Hizmetleri oluşturup yönetebilirsiniz |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="security-admin"></a>Güvenlik Yöneticisi
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Güvenlik Merkezi'nde yalnızca: Güvenlik ilkelerini görüntüleyin, güvenlik durumlarını görüntülemek, güvenlik ilkeleri, uyarıları görüntüleme ve öneriler düzenleme, uyarıları ve öneriler Kapat |
> | **Kimlik** | fb1c8493-542b-48eb-b624-b4c8fea62acd |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Authorization/policyAssignments/* | Oluşturma ve ilke atamalarını yönetme |
> | Microsoft.Authorization/policyDefinitions/* | Oluşturma ve yönetme ilke tanımları |
> | Microsoft.Authorization/policySetDefinitions/* | Oluşturma ve ilke kümelerini Yönet |
> | Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönet |
> | Microsoft.Management/managementGroups/read | Kimliği doğrulanmış kullanıcı için Yönetim grupları listesi. |
> | Microsoft.operationalInsights/workspaces/*/read | Log analytics verilerini görüntüleme |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Security/* |  |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="security-manager-legacy"></a>Güvenlik Yöneticisi (Eski)
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Bu eski bir roldür. Lütfen bunun yerine Güvenlik Yöneticisi kullanın |
> | **Kimlik** | e3d13bf0-dd5a-482e-ba6b-9b8433878d10 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.ClassicCompute/*/read | Klasik sanal makineleri yapılandırma bilgilerini okuyun |
> | Microsoft.ClassicCompute/virtualMachines/*/write | Klasik sanal makine yapılandırmasını yazma |
> | Microsoft.ClassicNetwork/*/read | Klasik ağ hakkında yapılandırma bilgilerini okuyun |
> | Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönet |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Security/* | Güvenlik bileşenleri ve ilkeleri oluşturma ve yönetme |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="security-reader"></a>Güvenlik Okuyucusu
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Güvenlik Merkezi'nde yalnızca: Öneriler ve uyarılar, güvenlik ilkeleri, güvenlik durumlarını görüntüleyebilir ancak değişiklik yapamaz görünüm görüntüleyebilirsiniz. |
> | **Kimlik** | 39bc4728-0917-49c7-9d2c-d95423bc2eb4 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönet |
> | Microsoft.operationalInsights/workspaces/*/read | Log analytics verilerini görüntüleme |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Security/*/read | Okuma güvenlik bileşenleri ve ilkeleri |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | Microsoft.Management/managementGroups/read | Kimliği doğrulanmış kullanıcı için Yönetim grupları listesi. |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="site-recovery-contributor"></a>Site Recovery Katkıda Bulunanı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Kasa oluşturma ve rol atama işlemleri dışında Site Recovery hizmetini yönetmenize imkan sağlar |
> | **Kimlik** | 6670b86e-a3f7-4917-ac9b-5d6ab1be4567 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönet |
> | Microsoft.Network/virtualNetworks/read | Sanal ağ tanımı Al |
> | Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp, hizmet tarafından kullanılan iç işlemdir. |
> | Microsoft.RecoveryServices/locations/allocateStamp/action | AllocateStamp hizmeti tarafından kullanılan iç işlemdir. |
> | Microsoft.RecoveryServices/Vaults/certificates/write | Kaynak sertifikayı Güncelleştir işlemi, kaynak/kasa kimlik bilgisi sertifikasını güncelleştirir. |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/* | Oluşturma ve yönetme kasaya ilgili genişletilmiş bilgileri |
> | Microsoft.RecoveryServices/Vaults/read | Kasayı Al işlemi 'vault' türündeki Azure kaynağını temsil eden bir nesne alır. |
> | Microsoft.RecoveryServices/Vaults/refreshContainers/read |  |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/* | Oluşturma ve kayıtlı kimlikleri yönetme |
> | Microsoft.RecoveryServices/vaults/replicationAlertSettings/* | Oluşturma veya çoğaltma uyarı ayarlarını güncelleştirme |
> | Microsoft.RecoveryServices/vaults/replicationEvents/read | Tüm olayları okuyun |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/* | Oluşturma ve çoğaltma yapıları yönetme |
> | Microsoft.RecoveryServices/vaults/replicationJobs/* | Çoğaltma işler oluşturma ve yönetme |
> | Microsoft.RecoveryServices/vaults/replicationPolicies/* | Çoğaltma ilkeleri oluşturma ve yönetme |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/* | Oluşturma ve kurtarma planları yönetme |
> | Microsoft.RecoveryServices/Vaults/storageConfig/* | Kurtarma Hizmetleri kasasının depolama yapılandırması oluşturma ve yönetme |
> | Microsoft.RecoveryServices/Vaults/tokenInfo/read |  |
> | Microsoft.RecoveryServices/Vaults/usages/read | Bir Kurtarma Hizmetleri Kasası için kullanım ayrıntılarını döndürür. |
> | Microsoft.RecoveryServices/Vaults/vaultTokens/read | Kasa düzeyi arka uç işlemlerine ait kasa Belirteci'ni almak için kasa belirteci işlemi kullanılabilir. |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/* | Kurtarma Hizmetleri kasası için uyarıları okuma |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read |  |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Storage/storageAccounts/read | Depolama hesaplarının listesini döndürür veya belirtilen depolama hesabının özelliklerini alır. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="site-recovery-operator"></a>Site Recovery Operatörü
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Yük devretme ve yeniden çalışma dışındaki Site Recovery yönetimi işlemlerini gerçekleştirmenize izin vermez |
> | **Kimlik** | 494ae006-db33-4328-bf46-533a6560a3ca |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönet |
> | Microsoft.Network/virtualNetworks/read | Sanal ağ tanımı Al |
> | Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp, hizmet tarafından kullanılan iç işlemdir. |
> | Microsoft.RecoveryServices/locations/allocateStamp/action | AllocateStamp hizmeti tarafından kullanılan iç işlemdir. |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/read | Daha Fazla Bilgi Al işlemi, ?vault? türündeki Azure kaynağını temsil eden nesnenin Daha Fazla Bilgi değerini alır. |
> | Microsoft.RecoveryServices/Vaults/read | Kasayı Al işlemi 'vault' türündeki Azure kaynağını temsil eden bir nesne alır. |
> | Microsoft.RecoveryServices/Vaults/refreshContainers/read |  |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | Zaman uyumsuz olarak gönderilen işlemin sonucu ve işlem durumunu alma işlem işlemi kullanılabilir sonuçlarını Al |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Kapsayıcıları Al işlemi kullanılabilir bir kaynak için kayıtlı olan kapsayıcıları alın. |
> | Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Herhangi bir uyarı ayarlarını okuma |
> | Microsoft.RecoveryServices/vaults/replicationEvents/read | Tüm olayları okuyun |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/checkConsistency/action | Doku Tutarlılığını Denetler |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/read | Tüm yapıları okuyun |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/reassociateGateway/action | Ağ geçidini yeniden ilişkilendir |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/renewcertificate/action | Yapı sertifikasını Yenile |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read | Herhangi bir ağa okuyun |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/read | Tüm ağ eşlemeleri okuyun |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/read | Tüm koruma kapsayıcıları okuyun |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectableItems/read | Tüm korunabilir öğelerin okuyun |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/applyRecoveryPoint/action | Kurtarma noktası Uygula |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/failoverCommit/action | Yük devretme işlemesi |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/plannedFailover/action | Planlanan Yük Devretme |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/read | Tüm korumalı öğeleri okuma |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | Tüm çoğaltma kurtarma noktaları okuyun |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/repairReplication/action | Çoğaltmayı Onar |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/reProtect/action | Korumalı öğeyi yeniden koru |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailover/action | Test Yük Devretmesi |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailoverCleanup/action | Yük devretme temizlik testi |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/unplannedFailover/action | Yük devretme |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/updateMobilityService/action | Mobility hizmetini güncelleştirme |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/read | Herhangi bir koruma kapsayıcısı eşlemelerini okuyun |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/read | Bir kurtarma Hizmetleri Sağlayıcısı'nı okuyun |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/refreshProvider/action | Sağlayıcı Yenile |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/read | Tüm depolama sınıflandırmaları okuyun |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/read | Herhangi bir depolama sınıflandırması eşlemeleri okuyun |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read | Tüm vCenters okuyun |
> | Microsoft.RecoveryServices/vaults/replicationJobs/* | Çoğaltma işler oluşturma ve yönetme |
> | Microsoft.RecoveryServices/vaults/replicationPolicies/read | Tüm ilkeler okuyun |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/failoverCommit/action | Yük devretme işlemesi kurtarma planı |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/plannedFailover/action | Planlı yük devretme kurtarma planı |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read | Kurtarma planları okuyun |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/reProtect/action | Kurtarma planını yeniden koru |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailover/action | Test yük devretme kurtarma planı |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailoverCleanup/action | Test Yük Devretmesini temizleme kurtarma planı |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/unplannedFailover/action | Yük devretme kurtarma planı |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/* | Kurtarma Hizmetleri kasası için uyarıları okuma |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read |  |
> | Microsoft.RecoveryServices/Vaults/storageConfig/read |  |
> | Microsoft.RecoveryServices/Vaults/tokenInfo/read |  |
> | Microsoft.RecoveryServices/Vaults/usages/read | Bir Kurtarma Hizmetleri Kasası için kullanım ayrıntılarını döndürür. |
> | Microsoft.RecoveryServices/Vaults/vaultTokens/read | Kasa düzeyi arka uç işlemlerine ait kasa Belirteci'ni almak için kasa belirteci işlemi kullanılabilir. |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Storage/storageAccounts/read | Depolama hesaplarının listesini döndürür veya belirtilen depolama hesabının özelliklerini alır. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="site-recovery-reader"></a>Site Recovery Okuyucusu
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Site Recovery durumunu görüntülemenize izin verir, ancak diğer yönetim işlemlerini gerçekleştirmenize izin vermez |
> | **Kimlik** | dbaa88c4-0c30-4179-9fb3-46319faa6149 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp, hizmet tarafından kullanılan iç işlemdir. |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/read | Daha Fazla Bilgi Al işlemi, ?vault? türündeki Azure kaynağını temsil eden nesnenin Daha Fazla Bilgi değerini alır. |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Kurtarma Hizmetleri kasası için uyarıları alır. |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read |  |
> | Microsoft.RecoveryServices/Vaults/read | Kasayı Al işlemi 'vault' türündeki Azure kaynağını temsil eden bir nesne alır. |
> | Microsoft.RecoveryServices/Vaults/refreshContainers/read |  |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | Zaman uyumsuz olarak gönderilen işlemin sonucu ve işlem durumunu alma işlem işlemi kullanılabilir sonuçlarını Al |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Kapsayıcıları Al işlemi kullanılabilir bir kaynak için kayıtlı olan kapsayıcıları alın. |
> | Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Herhangi bir uyarı ayarlarını okuma |
> | Microsoft.RecoveryServices/vaults/replicationEvents/read | Tüm olayları okuyun |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/read | Tüm yapıları okuyun |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read | Herhangi bir ağa okuyun |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/read | Tüm ağ eşlemeleri okuyun |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/read | Tüm koruma kapsayıcıları okuyun |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectableItems/read | Tüm korunabilir öğelerin okuyun |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/read | Tüm korumalı öğeleri okuma |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | Tüm çoğaltma kurtarma noktaları okuyun |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/read | Herhangi bir koruma kapsayıcısı eşlemelerini okuyun |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/read | Bir kurtarma Hizmetleri Sağlayıcısı'nı okuyun |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/read | Tüm depolama sınıflandırmaları okuyun |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/read | Herhangi bir depolama sınıflandırması eşlemeleri okuyun |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read | Tüm vCenters okuyun |
> | Microsoft.RecoveryServices/vaults/replicationJobs/read | Herhangi bir işi okuma |
> | Microsoft.RecoveryServices/vaults/replicationPolicies/read | Tüm ilkeler okuyun |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read | Kurtarma planları okuyun |
> | Microsoft.RecoveryServices/Vaults/storageConfig/read |  |
> | Microsoft.RecoveryServices/Vaults/tokenInfo/read |  |
> | Microsoft.RecoveryServices/Vaults/usages/read | Bir Kurtarma Hizmetleri Kasası için kullanım ayrıntılarını döndürür. |
> | Microsoft.RecoveryServices/Vaults/vaultTokens/read | Kasa düzeyi arka uç işlemlerine ait kasa Belirteci'ni almak için kasa belirteci işlemi kullanılabilir. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="spatial-anchors-account-contributor"></a>Spatial Anchors Hesabı Katkıda Bulunanı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Hesabınızdaki uzamsal sabit noktaları yönetmenize olanak sağlar, ancak silmenize izin vermez |
> | **Kimlik** | 8bbe83f1-e2a6-4df7-8cb4-4e04d4e5c827 |
> | **Eylemler** |  |
> | *yok* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/create/action | Uzamsal yer işaretleri oluşturmanız |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/discovery/read | Uzamsal bağlayıcılarını bulma |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/properties/read | Uzamsal yer işaretleri özelliklerini alma |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/query/read | Uzamsal bağlayıcılarını bulun |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/submitdiag/read | Azure uzamsal bağlayıcılarını hizmet kalitesini geliştirmeye yardımcı olmak için Tanılama verileri gönder |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/write | Uzamsal yer işaretleri özelliklerini güncelleştir |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="spatial-anchors-account-owner"></a>Spatial Anchors Hesabı Sahibi
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Silme de dahil olmak üzere hesabınızdaki uzamsal sabit noktaları yönetmenize olanak sağlar |
> | **Kimlik** | 70bbe301-9835-447d-afdd-19eb3167307c |
> | **Eylemler** |  |
> | *yok* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/create/action | Uzamsal yer işaretleri oluşturmanız |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/delete | Uzamsal çıpalarını silme |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/discovery/read | Uzamsal bağlayıcılarını bulma |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/properties/read | Uzamsal yer işaretleri özelliklerini alma |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/query/read | Uzamsal bağlayıcılarını bulun |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/submitdiag/read | Azure uzamsal bağlayıcılarını hizmet kalitesini geliştirmeye yardımcı olmak için Tanılama verileri gönder |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/write | Uzamsal yer işaretleri özelliklerini güncelleştir |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="spatial-anchors-account-reader"></a>Spatial Anchors Hesabı Okuyucusu
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Hesabınızdaki uzamsal sabit noktaları bulup bunların özelliklerini okumanıza olanak sağlar |
> | **Kimlik** | 5d51204f-eb77-4b1c-b86a-2ec626c49413 |
> | **Eylemler** |  |
> | *yok* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/discovery/read | Uzamsal bağlayıcılarını bulma |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/properties/read | Uzamsal yer işaretleri özelliklerini alma |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/query/read | Uzamsal bağlayıcılarını bulun |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/submitdiag/read | Azure uzamsal bağlayıcılarını hizmet kalitesini geliştirmeye yardımcı olmak için Tanılama verileri gönder |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="sql-db-contributor"></a>SQL DB Katılımcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | SQL veritabanları, ancak onlara yönelik erişimi yönetmenize olanak tanır. Ayrıca, güvenlikle ilgili ilkelerini veya üst SQL sunucularını yönetemezsiniz. |
> | **Kimlik** | 9b7fa17d-e63e-47b0-bb0a-15c516ac86ec |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönet |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Sql/locations/*/read |  |
> | Microsoft.Sql/servers/databases/* | SQL veritabanlarını oluşturma ve yönetme |
> | Microsoft.Sql/servers/read | Sunucuları veya belirtilen sunucunun özelliklerini alır listesini döndürür. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | Microsoft.Insights/metrics/read | Ölçümleri okuma |
> | Microsoft.Insights/metricDefinitions/read | Ölçüm tanımlarını oku |
> | **NotActions** |  |
> | Microsoft.Sql/managedInstances/databases/currentSensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/recommendedSensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/securityAlertPolicies/* |  |
> | Microsoft.Sql/managedInstances/databases/sensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/managedInstances/securityAlertPolicies/* |  |
> | Microsoft.Sql/managedInstances/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/servers/databases/auditingPolicies/* | Denetim ilkeleri Düzenle |
> | Microsoft.Sql/servers/databases/auditingSettings/* | Denetim ayarlarını Düzenle |
> | Microsoft.Sql/servers/databases/auditRecords/read | Veritabanı blob Denetim kayıtlarını alma |
> | Microsoft.Sql/servers/databases/connectionPolicies/* | Bağlantı ilkeleri Düzenle |
> | Microsoft.Sql/servers/databases/currentSensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Veri maskeleme ilkeleri Düzenle |
> | Microsoft.Sql/servers/databases/extendedAuditingSettings/* |  |
> | Microsoft.Sql/servers/databases/recommendedSensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/securityAlertPolicies/* | Uyarı güvenlik ilkelerini düzenleme |
> | Microsoft.Sql/servers/databases/securityMetrics/* | Güvenlik ölçümlerinin Düzenle |
> | Microsoft.Sql/servers/databases/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/* |  |
> | Microsoft.Sql/servers/vulnerabilityAssessments/* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="sql-security-manager"></a>SQL Güvenlik Yöneticisi
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | SQL sunucularının ve veritabanlarının güvenlikle ilgili ilkelerini yönetmenizi sağlar ancak onlara erişimi yönetme izni vermez. |
> | **Kimlik** | 056cd41c-7e88-42e1-933e-88ba6a50c9c3 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma Microsoft yetkilendirmesi |
> | Microsoft.Insights/alertRules/* | Oluşturma ve yönetme Insights uyarı kuralları |
> | Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Kaynak depolama hesabı veya SQL veritabanı gibi bir alt ağa katılır. Alertable değil. |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Sql/managedInstances/databases/currentSensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/recommendedSensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/securityAlertPolicies/* |  |
> | Microsoft.Sql/managedInstances/databases/sensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/managedInstances/securityAlertPolicies/* |  |
> | Microsoft.Sql/managedInstances/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/servers/auditingPolicies/* | SQL server denetim ilkeleri oluşturma ve yönetme |
> | Microsoft.Sql/servers/auditingSettings/* | Oluşturma ve SQL server denetim ayarı yönetme |
> | Microsoft.Sql/servers/extendedAuditingSettings/read | Denetim İlkesi belirli bir sunucuda yapılandırılan genişletilmiş sunucusu blob ayrıntılarını alma |
> | Microsoft.Sql/servers/databases/auditingPolicies/* | SQL server veritabanı denetim ilkeleri oluşturma ve yönetme |
> | Microsoft.Sql/servers/databases/auditingSettings/* | Oluşturma ve SQL server veritabanı denetim ayarlarını yönetme |
> | Microsoft.Sql/servers/databases/auditRecords/read | Denetim kayıtlarını okuma |
> | Microsoft.Sql/servers/databases/connectionPolicies/* | SQL server veritabanı bağlantı ilkeleri oluşturma ve yönetme |
> | Microsoft.Sql/servers/databases/currentSensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Oluşturma ve SQL server veritabanı veri maskeleme ilkelerini yönetme |
> | Microsoft.Sql/servers/databases/extendedAuditingSettings/read | Belirli bir veritabanı üzerinde yapılandırılan genişletilmiş blob denetim ilkesi ayrıntılarını alma |
> | Microsoft.Sql/servers/databases/read | Veritabanları veya belirtilen veritabanı özelliklerini alır listesini döndürür. |
> | Microsoft.Sql/servers/databases/recommendedSensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/schemas/read | Veritabanı şeması alın. |
> | Microsoft.Sql/servers/databases/schemas/tables/columns/read | Bir veritabanı sütunu alın. |
> | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/schemas/tables/read | Bir veritabanı tablosuna alın. |
> | Microsoft.Sql/servers/databases/securityAlertPolicies/* | SQL server veritabanı güvenlik uyarı ilkeleri oluşturma ve yönetme |
> | Microsoft.Sql/servers/databases/securityMetrics/* | Oluşturma ve SQL server veritabanı güvenlik ölçümlerini yönetme |
> | Microsoft.Sql/servers/databases/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/* |  |
> | Microsoft.Sql/servers/firewallRules/* |  |
> | Microsoft.Sql/servers/read | Sunucuları veya belirtilen sunucunun özelliklerini alır listesini döndürür. |
> | Microsoft.Sql/servers/securityAlertPolicies/* | SQL server güvenlik uyarı ilkeleri oluşturma ve yönetme |
> | Microsoft.Sql/servers/vulnerabilityAssessments/* |  |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="sql-server-contributor"></a>SQL Server Katılımcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | SQL sunucularını ve veritabanlarını yönetmenizi sağlar ancak güvenlikle ilgili ilkelerini yönetmenize izin vermez. |
> | **Kimlik** | 6d8ee4ec-f05a-4a1d-8b00-a9b17e38b437 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Insights/alertRules/* | Oluşturma ve yönetme Insights uyarı kuralları |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Sql/locations/*/read |  |
> | Microsoft.Sql/servers/* | Oluşturma ve SQL sunucularını yönetme |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | Microsoft.Insights/metrics/read | Ölçümleri okuma |
> | Microsoft.Insights/metricDefinitions/read | Ölçüm tanımlarını oku |
> | **NotActions** |  |
> | Microsoft.Sql/managedInstances/databases/currentSensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/recommendedSensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/securityAlertPolicies/* |  |
> | Microsoft.Sql/managedInstances/databases/sensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/managedInstances/securityAlertPolicies/* |  |
> | Microsoft.Sql/managedInstances/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/servers/auditingPolicies/* | SQL server denetim ilkeleri Düzenle |
> | Microsoft.Sql/servers/auditingSettings/* | SQL sunucusunun denetim ayarlarını Düzenle |
> | Microsoft.Sql/servers/databases/auditingPolicies/* | SQL server veritabanı denetim ilkeleri Düzenle |
> | Microsoft.Sql/servers/databases/auditingSettings/* | SQL server veritabanı denetim ayarları Düzenle |
> | Microsoft.Sql/servers/databases/auditRecords/read | Denetim kayıtlarını okuma |
> | Microsoft.Sql/servers/databases/connectionPolicies/* | SQL server veritabanı bağlantı ilkeleri Düzenle |
> | Microsoft.Sql/servers/databases/currentSensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/dataMaskingPolicies/* | SQL server veritabanı veri maskeleme ilkeleri Düzenle |
> | Microsoft.Sql/servers/databases/extendedAuditingSettings/* |  |
> | Microsoft.Sql/servers/databases/recommendedSensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/securityAlertPolicies/* | SQL server veritabanı güvenlik uyarı ilkelerini düzenleme |
> | Microsoft.Sql/servers/databases/securityMetrics/* | SQL server veritabanı güvenlik ölçümlerini Düzenle |
> | Microsoft.Sql/servers/databases/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/* |  |
> | Microsoft.Sql/servers/extendedAuditingSettings/* |  |
> | Microsoft.Sql/servers/securityAlertPolicies/* | SQL server güvenlik uyarı ilkelerini düzenleme |
> | Microsoft.Sql/servers/vulnerabilityAssessments/* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="storage-account-contributor"></a>Depolama Hesabı Katılımcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Depolama hesaplarının yönetimini izin verir. Depolama hesabındaki verilere erişim sağlamaz. |
> | **Kimlik** | 17d1049b-9a84-46fb-8f53-869881c3d3ab |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Tüm Yetkilendirme okuyun |
> | Microsoft.Insights/alertRules/* | Oluşturma ve yönetme Insights uyarı kuralları |
> | Microsoft.Insights/diagnosticSettings/* | Tanılama ayarlarını yönetme |
> | Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Kaynak depolama hesabı veya SQL veritabanı gibi bir alt ağa katılır. Alertable değil. |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Storage/storageAccounts/* | Depolama hesapları oluşturma ve yönetme |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="storage-account-key-operator-service-role"></a>Depolama Hesabı Anahtarı İşleci Hizmet Rolü
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Listeleme ve depolama hesabı erişim anahtarlarını yeniden izin verir. |
> | **Kimlik** | 81a9662b-bebf-436f-a333-f67b29880f12 |
> | **Eylemler** |  |
> | Microsoft.Storage/storageAccounts/listkeys/action | Belirtilen depolama hesabının erişim anahtarlarını döndürür. |
> | Microsoft.Storage/storageAccounts/regeneratekey/action | Belirtilen depolama hesabının erişim anahtarlarını yeniden oluştur. |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="storage-blob-data-contributor"></a>Depolama Blob Verileri Katkıda Bulunanı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Okuma, yazma ve Azure depolama kapsayıcıları ve blobları silin. Verilen veri çalışması için gerekli eylemleri öğrenmek için bkz: [blob ve kuyruk veri işlemleri çağırmak için izinleri](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-rest-operations). |
> | **Kimlik** | ba92f5b4-2d11-453d-a403-e96b0029c9fe |
> | **Eylemler** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/delete | Bir kapsayıcı silin. |
> | Microsoft.Storage/storageAccounts/blobServices/containers/read | Bir kapsayıcı veya bir kapsayıcı listesi döndürür. |
> | Microsoft.Storage/storageAccounts/blobServices/containers/write | Bir kapsayıcının meta verileri veya özelliklerini değiştirin. |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete | Bir blobu silin. |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read | Bir blobu veya BLOB listesini döndürür. |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write | Bir blob olarak yazar. |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="storage-blob-data-owner"></a>Depolama Blob Verileri Sahibi
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Azure depolama blob kapsayıcılarına ve verilerine, POSIX erişim denetimi atama dahil olmak üzere tam erişim sağlar. Verilen veri çalışması için gerekli eylemleri öğrenmek için bkz: [blob ve kuyruk veri işlemleri çağırmak için izinleri](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-rest-operations). |
> | **Kimlik** | b7e6dc6d-f1e8-4753-8033-0f276bb0955b |
> | **Eylemler** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/* | Tam kapsayıcılarında.  |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/* | Blobları tam izinleri. |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="storage-blob-data-reader"></a>Depolama Blob Verileri Okuyucusu
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Okuma ve Azure depolama kapsayıcıları ve blobları listeleyin. Verilen veri çalışması için gerekli eylemleri öğrenmek için bkz: [blob ve kuyruk veri işlemleri çağırmak için izinleri](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-rest-operations). |
> | **Kimlik** | 2a2b9908-6ea1-4ae2-8e65-a410df84e7d1 |
> | **Eylemler** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/read | Bir kapsayıcı veya bir kapsayıcı listesi döndürür. |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read | Bir blobu veya BLOB listesini döndürür. |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="storage-queue-data-contributor"></a>Depolama Kuyruk Verileri Katkıda Bulunanı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Okuma, yazma ve Azure depolama kuyruklarına ve kuyruk iletilerine silin. Verilen veri çalışması için gerekli eylemleri öğrenmek için bkz: [blob ve kuyruk veri işlemleri çağırmak için izinleri](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-rest-operations). |
> | **Kimlik** | 974c5e8b-45b9-4653-ba55-5f855dd0fb88 |
> | **Eylemler** |  |
> | Microsoft.Storage/storageAccounts/queueServices/queues/delete | Bir kuyruk silme. |
> | Microsoft.Storage/storageAccounts/queueServices/queues/read | Bir kuyruğu veya kuyruk listesini döndürür. |
> | Microsoft.Storage/storageAccounts/queueServices/queues/write | Kuyruk meta verileri veya özelliklerini değiştirin. |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/queueServices/queues/messages/delete | Bir veya daha fazla ileti kuyruktan silin. |
> | Microsoft.Storage/storageAccounts/queueServices/queues/messages/read | Özet veya kuyruktan bir veya daha fazla ileti alır. |
> | Microsoft.Storage/storageAccounts/queueServices/queues/messages/write | Kuyruğa bir ileti ekleyin. |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="storage-queue-data-message-processor"></a>Depolama Kuyruk Verileri İleti İşleyicisi
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Özet, almak ve bir Azure depolama kuyruğuna iletiler silin. Verilen veri çalışması için gerekli eylemleri öğrenmek için bkz: [blob ve kuyruk veri işlemleri çağırmak için izinleri](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-rest-operations). |
> | **Kimlik** | 8a0f0c08-91a1-4084-bc3d-661d67233fed |
> | **Eylemler** |  |
> | *yok* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/queueServices/queues/messages/read | Bir iletiye Gözat. |
> | Microsoft.Storage/storageAccounts/queueServices/queues/messages/process/action | Almak ve bir ileti Sil. |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="storage-queue-data-message-sender"></a>Depolama Kuyruk Verileri İleti Göndericisi
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Bir Azure depolama kuyruğuna ileti ekleme. Verilen veri çalışması için gerekli eylemleri öğrenmek için bkz: [blob ve kuyruk veri işlemleri çağırmak için izinleri](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-rest-operations). |
> | **Kimlik** | c6a89b2d-59bc-44d0-9896-0f6e12d7b80a |
> | **Eylemler** |  |
> | *yok* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/queueServices/queues/messages/add/action | Kuyruğa bir ileti ekleyin. |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="storage-queue-data-reader"></a>Depolama Kuyruk Verileri Okuyucusu
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Okuma ve Azure depolama kuyruklarına ve kuyruk iletilerine listeleyin. Verilen veri çalışması için gerekli eylemleri öğrenmek için bkz: [blob ve kuyruk veri işlemleri çağırmak için izinleri](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-rest-operations). |
> | **Kimlik** | 19e7f393-937e-4f77-808e-94535e297925 |
> | **Eylemler** |  |
> | Microsoft.Storage/storageAccounts/queueServices/queues/read | Bir kuyruğu veya kuyruk listesini döndürür. |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/queueServices/queues/messages/read | Özet veya kuyruktan bir veya daha fazla ileti alır. |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="support-request-contributor"></a>Destek İsteğine Katkıda Bulunan
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Destek istekleri oluşturmanıza ve bunları yönetmenize olanak sağlar |
> | **Kimlik** | cfd33db0-3dd1-45e3-aa9d-cdbdf3b6f24e |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Yetkilendirme okuyun |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="traffic-manager-contributor"></a>Traffic Manager Katkıda Bulunanı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Traffic Manager profillerini yönetmenize izin verir, ancak bunlara kimlerin erişebildiğini denetlemenize izin vermez. |
> | **Kimlik** | a4b10055-b0c7-44c2-b00f-c7b5b3550cf7 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Insights/alertRules/* | Oluşturma ve yönetme Insights uyarı kuralları |
> | Microsoft.Network/trafficManagerProfiles/* |  |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="user-access-administrator"></a>Kullanıcı Erişimi Yöneticisi
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Azure kaynaklarına yönelik kullanıcı erişimini yönetmenizi sağlar. |
> | **Kimlik** | 18d7d88d-d35e-4fb5-a5c3-7773c20a72d9 |
> | **Eylemler** |  |
> | * / Okuma | Gizli dizileri dışında tüm türler kaynakları okuyun. |
> | Microsoft.Authorization/* | Yetkilendirme yönetme |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="virtual-machine-administrator-login"></a>Sanal Makine Yöneticisi Oturum Açma
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Portalda Sanal Makineleri görüntüleyin ve yönetici olarak oturum açın |
> | **Kimlik** | 1c0163c0-47e6-4577-8991-ea5c82e286e4 |
> | **Eylemler** |  |
> | Microsoft.Network/publicIPAddresses/read | Bir genel IP adresi tanımı alır. |
> | Microsoft.Network/virtualNetworks/read | Sanal ağ tanımı Al |
> | Microsoft.Network/loadBalancers/read | Bir yük dengeleyici tanımı alır |
> | Microsoft.Network/networkInterfaces/read | Bir ağ arabirimi tanımı alır.  |
> | Microsoft.Compute/virtualMachines/*/read |  |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | Microsoft.Compute/virtualMachines/login/action | Bir sanal makinede normal bir kullanıcı olarak oturum açın |
> | Microsoft.Compute/virtualMachines/loginAsAdmin/action | Windows yöneticisi veya Linux kök kullanıcı ayrıcalıklarıyla bir sanal makinede oturum açın |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="virtual-machine-contributor"></a>Sanal Makine Katılımcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Sanal makineleri yönetmenizi sağlar ancak bu sanal makinelere ve bağlı oldukları sanal ağ veya depolama hesaplarına yönelik erişimi yönetme izni vermez. |
> | **Kimlik** | 9980e02c-c2be-4d73-94e8-173b1dc7cf3c |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Yetkilendirme okuyun |
> | Microsoft.Compute/availabilitySets/* | Oluşturma ve işlem kullanılabilirlik kümelerini Yönet |
> | Microsoft.Compute/locations/* | Oluşturma ve işlem konumlarını Yönet |
> | Microsoft.Compute/virtualMachines/* | Sanal makineler oluşturun ve yönetin |
> | Microsoft.Compute/virtualMachineScaleSets/* | Oluşturma ve sanal makine ölçek kümeleri yönetme |
> | Microsoft.DevTestLab/schedules/* |  |
> | Microsoft.Insights/alertRules/* | Oluşturma ve yönetme Insights uyarı kuralları |
> | Microsoft.Network/applicationGateways/backendAddressPools/join/action | Bir uygulama ağ geçidi arka uç adres havuzu birleştirir. Alertable değil. |
> | Microsoft.Network/loadBalancers/backendAddressPools/join/action | Bir yük dengeleyici arka uç adres havuzu birleştirir. Alertable değil. |
> | Microsoft.Network/loadBalancers/inboundNatPools/join/action | Birleştiren bir yük dengeleyici gelen NAT havuzu. Alertable değil. |
> | Microsoft.Network/loadBalancers/inboundNatRules/join/action | Bir yük dengeleyici gelen nat kuralı birleştirir. Alertable değil. |
> | Microsoft.Network/loadBalancers/probes/join/action | Bir yük dengeleyici araştırmalarını kullanan sağlar. Örneğin, VM ölçek ile bu izni healthProbe özellik kümesi araştırma başvurabilirsiniz. Alertable değil. |
> | Microsoft.Network/loadBalancers/read | Bir yük dengeleyici tanımı alır |
> | Microsoft.Network/locations/* | Oluşturun ve ağ konumları yönetin |
> | Microsoft.Network/networkInterfaces/* | Oluşturun ve ağ ara birimlerini ayarlama |
> | Microsoft.Network/networkSecurityGroups/join/action | Bir ağ güvenlik grubu birleştirir. Alertable değil. |
> | Microsoft.Network/networkSecurityGroups/read | Bir ağ güvenlik grubu tanımı alır |
> | Microsoft.Network/publicIPAddresses/join/action | Bir genel IP adresi birleştirir. Alertable değil. |
> | Microsoft.Network/publicIPAddresses/read | Bir genel IP adresi tanımı alır. |
> | Microsoft.Network/virtualNetworks/read | Sanal ağ tanımı Al |
> | Microsoft.Network/virtualNetworks/subnets/join/action | Bir sanal ağa katılır. Alertable değil. |
> | Microsoft.RecoveryServices/locations/* |  |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write | Bir yedekleme koruma hedefi oluşturma |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/*/read |  |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read | Korumalı öğenin nesne ayrıntılarını döndürür |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write | Yedekleme korumalı öğesi oluştur |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/read | Tüm koruma ilkelerini döndürür |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/write | Koruma ilkesi oluşturur |
> | Microsoft.RecoveryServices/Vaults/read | Kasayı Al işlemi 'vault' türündeki Azure kaynağını temsil eden bir nesne alır. |
> | Microsoft.RecoveryServices/Vaults/usages/read | Bir Kurtarma Hizmetleri Kasası için kullanım ayrıntılarını döndürür. |
> | Microsoft.RecoveryServices/Vaults/write | Kasa Oluştur işlemi, 'vault' türünde bir Azure kaynağı oluşturur |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.SqlVirtualMachine/* |  |
> | Microsoft.Storage/storageAccounts/listKeys/action | Belirtilen depolama hesabının erişim anahtarlarını döndürür. |
> | Microsoft.Storage/storageAccounts/read | Depolama hesaplarının listesini döndürür veya belirtilen depolama hesabının özelliklerini alır. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="virtual-machine-user-login"></a>Sanal Makine Kullanıcı Oturum Açma
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Portalda Sanal Makineleri görüntüleyin ve normal bir kullanıcı olarak oturum açın. |
> | **Kimlik** | fb879df8-f326-4884-b1cf-06f3ad86be52 |
> | **Eylemler** |  |
> | Microsoft.Network/publicIPAddresses/read | Bir genel IP adresi tanımı alır. |
> | Microsoft.Network/virtualNetworks/read | Sanal ağ tanımı Al |
> | Microsoft.Network/loadBalancers/read | Bir yük dengeleyici tanımı alır |
> | Microsoft.Network/networkInterfaces/read | Bir ağ arabirimi tanımı alır.  |
> | Microsoft.Compute/virtualMachines/*/read |  |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | Microsoft.Compute/virtualMachines/login/action | Bir sanal makinede normal bir kullanıcı olarak oturum açın |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="web-plan-contributor"></a>Web Planı Katılımcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Web siteleri için web planlarını yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |
> | **Kimlik** | 2cc479cb-7b4d-49a8-b449-8c00fd0f0a4b |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Yetkilendirme okuyun |
> | Microsoft.Insights/alertRules/* | Oluşturma ve yönetme Insights uyarı kuralları |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | Microsoft.Web/serverFarms/* | Oluşturma ve sunucu grupları yönetme |
> | Microsoft.Web/hostingEnvironments/Join/Action | App Service ortamı birleştirir |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="website-contributor"></a>Web Sitesi Katılımcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Web sitelerini (web planlarını değil) yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |
> | **Kimlik** | de139f84-1756-47ae-9be6-808fbbe84772 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Yetkilendirme okuyun |
> | Microsoft.Insights/alertRules/* | Oluşturma ve yönetme Insights uyarı kuralları |
> | Microsoft.Insights/components/* | Oluşturma ve Insights bileşenlerini yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımlarında yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | Microsoft.Web/certificates/* | Web sitesi sertifikaları oluşturmak ve yönetmek |
> | Microsoft.Web/listSitesAssignedToHostName/read | Ana bilgisayar adı için atanan site adını alın. |
> | Microsoft.Web/serverFarms/join/action |  |
> | Microsoft.Web/serverFarms/read | Bir App Service planı üzerinde özelliklerini alma |
> | Microsoft.Web/sites/* | Oluşturma ve yönetme (site oluşturmayı ilişkili App Service planı için yazma izinleri de gerektirir) Web siteleri |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure kaynakları için özel roller](custom-roles.md)
- [RBAC ve Azure portalını kullanarak Azure kaynaklarına erişimi yönetme](role-assignments-portal.md)
- [Azure Güvenlik Merkezi'nde izinler](../security-center/security-center-permissions.md)
