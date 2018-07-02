---
title: Azure Resource Manager kaynak sağlayıcısı işlemleri | Microsoft Docs
description: Microsoft Azure Resource Manager kaynak sağlayıcılarına ilişkin işlemleri listeler
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/28/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: df999ce08785ce2c3954d18f3b05eb82bbbc5c97
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37116413"
---
# <a name="azure-resource-manager-resource-provider-operations"></a>Azure Resource Manager kaynak sağlayıcısı işlemleri

Bu makalede her Azure Resource Manager kaynak sağlayıcısı için kullanılabilir olan işlemleri listelenmektedir. Bu işlemler, kullanılabilir [özel roller](custom-roles.md) ayrıntılı sağlamak için [rol tabanlı erişim denetimi (RBAC)](overview.md) kaynaklara. İşlemi dizeleri aşağıdaki biçime sahiptir: `{Company}.{ProviderName}/{resourceType}/{action}`

Kaynak sağlayıcısı işlemleri her zaman artmaktadır. En son işlemleri almak için [Get-AzureRmProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) veya [az sağlayıcı işlemi listesi](/cli/azure/provider/operation#az-provider-operation-list).

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="microsoftaad"></a>Microsoft.AAD

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.AAD/domainServices/delete | Etki Alanı Hizmetleri siler. |
> | Eylem | Microsoft.AAD/domainServices/read | Etki Alanı Hizmetleri okur. |
> | Eylem | Microsoft.AAD/domainServices/write | Etki Alanı Hizmetleri yazma |
> | Eylem | Microsoft.AAD/locations/operationresults/read |  |
> | Eylem | Microsoft.AAD/Operations/read |  |

## <a name="microsoftaadiam"></a>microsoft.aadiam

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.aadiam/diagnosticsettings/DELETE | Tanılama ayarını silme |
> | Eylem | Microsoft.aadiam/diagnosticsettings/Read | Tanılama ayarını okuma |
> | Eylem | Microsoft.aadiam/diagnosticsettings/Write | Tanılama ayarı yazılıyor |
> | Eylem | Microsoft.aadiam/diagnosticsettingscategories/Read | Tanılama ayarını kategorileri okuma |
> | Eylem | microsoft.aadiam/tenants/providers/Microsoft.Insights/diagnosticSettings/read | Kaynak için tanılama ayarını alır |
> | Eylem | microsoft.aadiam/tenants/providers/Microsoft.Insights/diagnosticSettings/write | Kaynak için tanılama ayarını oluşturur veya güncelleştirir |
> | Eylem | microsoft.aadiam/tenants/providers/Microsoft.Insights/logDefinitions/read | Kiracılar için kullanılabilir günlüklerini alır |

## <a name="microsoftaddons"></a>Microsoft.Addons

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Addons/operations/read | Alır RP işlemleri desteklenmiyor. |
> | Eylem | Microsoft.Addons/register/action | Belirtilen abonelik Microsoft.Addons ile kaydetme |
> | Eylem | Microsoft.Addons/supportProviders/listsupportplaninfo/action | Belirtilen abonelik için geçerli destek planı bilgilerini listeler. |
> | Eylem | Microsoft.Addons/supportProviders/supportPlanTypes/delete | Belirtilen kurallı destek planı kaldırır |
> | Eylem | Microsoft.Addons/supportProviders/supportPlanTypes/read | Belirtilen kurallı destek planı duruma getirin. |
> | Eylem | Microsoft.Addons/supportProviders/supportPlanTypes/write | Belirtilen kurallı destek planı türü ekler. |

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.ADHybridHealthService/addsservices/action | Kiracı için yeni bir orman oluşturun. |
> | Eylem | Microsoft.ADHybridHealthService/addsservices/addomainservicemembers/read | Tüm sunucular için belirtilen hizmet adını alır. |
> | Eylem | Microsoft.ADHybridHealthService/addsservices/alerts/read | Uyarıları ayrıntıları yükseltilmiş orman alertıd, uyarı gibi tarih, uyarı son algılanan, uyarı açıklaması, son güncelleştirilmiş, uyarı düzeyi, uyarı durumu, sorun giderme bağlantılarını vb. uyarı alır. |
> | Eylem | Microsoft.ADHybridHealthService/addsservices/configuration/read | Hizmet yapılandırması için orman alır. Örnek - orman adı, etki alanı adlandırma Yöneticisi FSMO rolü, Şema Yöneticisi FSMO rolü vb. Functionla düzeyi. |
> | Eylem | Microsoft.ADHybridHealthService/addsservices/delete | Bir hizmet siler ve onu sunucularının yanı sıra sistem durumu verileri. |
> | Eylem | Microsoft.ADHybridHealthService/addsservices/dimensions/read | Orman için etki alanı ve site ayrıntıları alır. Örnek sistem durumu, etkin uyarıları, çözümlenen uyarıları, etki alanı işlev düzeyi orman, altyapı yöneticisi, PDC, gibi özellikleri RID master vs.  |
> | Eylem | Microsoft.ADHybridHealthService/addsservices/features/userpreference/read | Orman için kullanıcı tercihi ayarını alır.<br>Örnek - MetricCounterName ldapsuccessfulbinds, ntlmauthentications, kerberosauthentications, addsinsightsagentprivatebytes, ldapsearches gibi.<br>UI grafikleri vb. için ayarları. |
> | Eylem | Microsoft.ADHybridHealthService/addsservices/forestsummary/read | Orman orman adı, bu ormanda altındaki etki alanları sayısı, siteler ve site ayrıntıları vb. sayısı gibi belirli bir orman için Özet alır. |
> | Eylem | Microsoft.ADHybridHealthService/addsservices/metricmetadata/read | Belirli bir hizmeti için desteklenen ölçümleri listesini alır.<br>Örnek Extranet hesap kilitlemeleri, toplam başarısız istekler, bekleyen belirteç isteklerini (Proxy), belirteç isteklerini/sec ADFS hizmetinin vb. için.<br>NTLM kimlik doğrulamaları/sn, LDAP başarılı bağlantısı sayısı/sn, LDAP bağlama süresi, LDAP Etkin iş parçacığı, Kerberos kimlik doğrulamaları/sn vb. ATQ iş parçacıkları toplam ADDomainService için.<br>ADSync hizmeti için Azure AD için profil gecikme, TCP bağlantı kuran, Öngörüler Aracısı özel bayt sayısı, istatistikleri Dışarı Aktar'ı çalıştırın. |
> | Eylem | Microsoft.ADHybridHealthService/addsservices/metrics/groups/read | Hizmet verilen, bu API ölçümleri bilgilerini alır.<br>Örneğin, bu API ile ilgili bilgi almak için kullanılabilir: Extranet hesap kilitlemeleri, toplam başarısız istekler, bekleyen belirteç isteklerini (Proxy), belirteç isteklerini/sec vb. ADFederation hizmeti.<br>NTLM kimlik doğrulamaları/sn, LDAP başarılı bağlantısı sayısı/sn, LDAP bağlama süresi, LDAP Etkin iş parçacığı, Kerberos kimlik doğrulamaları/sn vb. ATQ iş parçacıkları toplam ADDomain hizmeti.<br>Profil gecikme Çalıştır TCP bağlantı kuran, Öngörüler Aracısı özel bayt sayısı, istatistikleri için Azure AD eşitleme hizmeti için dışarı aktarın. |
> | Eylem | Microsoft.ADHybridHealthService/addsservices/premiumcheck/read | Bu API için premium Kiracı tüm edildi ADDomainServices listesini alır. |
> | Eylem | Microsoft.ADHybridHealthService/addsservices/read | Hizmet Ayrıntıları belirtilen hizmet adını alır. |
> | Eylem | Microsoft.ADHybridHealthService/addsservices/replicationdetails/read | Belirtilen hizmet adı için tüm sunucular için çoğaltma ayrıntılarını alır. |
> | Eylem | Microsoft.ADHybridHealthService/addsservices/replicationstatus/read | Etki alanı denetleyicileri ve bunların çoğaltma hataları varsa sayısını alır. |
> | Eylem | Microsoft.ADHybridHealthService/addsservices/replicationsummary/read | Belirtilen orman için çoğaltma ayrıntılarını yanı sıra etki alanı denetleyicisi listesi alır tamamlayın. |
> | Eylem | Microsoft.ADHybridHealthService/addsservices/servicemembers/action | Bir sunucu örneği hizmete ekleyin. |
> | Eylem | Microsoft.ADHybridHealthService/addsservices/servicemembers/credentials/read | ADDomainService sunucu kaydı sırasında bu API yeni sunucuları hazırlanması için kimlik bilgilerini almak için çağrılır. |
> | Eylem | Microsoft.ADHybridHealthService/addsservices/servicemembers/delete | Verili hizmet ve Kiracı için bir sunucu siler. |
> | Eylem | Microsoft.ADHybridHealthService/addsservices/write | Oluşturur veya Kiracı için ADDomainService örneğini güncelleştirir. |
> | Eylem | Microsoft.ADHybridHealthService/configuration/action | Güncelleştirmeler Kiracı yapılandırması. |
> | Eylem | Microsoft.ADHybridHealthService/configuration/read | Kiracı yapılandırması okur. |
> | Eylem | Microsoft.ADHybridHealthService/configuration/write | Bir kiracı yapılandırma oluşturur. |
> | Eylem | Microsoft.ADHybridHealthService/logs/contents/read | Aracı yükleme ve kayıt günlüklerini blob içinde depolanan içeriğini alır. |
> | Eylem | Microsoft.ADHybridHealthService/logs/read | Aracı yükleme ve kayıt günlüklerini Kiracı için alır. |
> | Eylem | Microsoft.ADHybridHealthService/operations/read | Sistem tarafından desteklenen işlemler listesini alır. |
> | Eylem | Microsoft.ADHybridHealthService/register/action | ADHybrid sistem durumu Hizmet kaynağı sağlayıcısını kaydeder ve ADHybrid sistem durumu Hizmet kaynağı oluşturulmasını sağlar. |
> | Eylem | Microsoft.ADHybridHealthService/reports/availabledeployments/read | Müşteri olaylar desteklemek için DevOps tarafından kullanılan kullanılabilir bölgelerin listesini alır. |
> | Eylem | Microsoft.ADHybridHealthService/reports/badpassword/read | Active Directory Federasyon Hizmeti içindeki tüm kullanıcılar için hatalı parola denemesi listesini alır. |
> | Eylem | Microsoft.ADHybridHealthService/reports/badpassworduseridipfrequency/read | BLOB SAS URİ'si durumu içeren alır ve belirli bir kiracı için günlük IPADDRESS başına UserID başına nihai sonucu yeni sıraya alınan rapor işi sıklığı, hatalı kullanıcı adı/parola için çalışır. |
> | Eylem | Microsoft.ADHybridHealthService/reports/consentedtodevopstenants/read | DevOps listesini alır rıza kiracılar. Genellikle, müşteri desteği için kullanılır. |
> | Eylem | Microsoft.ADHybridHealthService/reports/isdevops/read | Wheather teannt DevOps rıza olup olmadığını belirten bir değer alır. |
> | Eylem | Microsoft.ADHybridHealthService/reports/selectdevopstenant/read | Userid(objectid) seçili geliştirme ops Kiracı için güncelleştirir. |
> | Eylem | Microsoft.ADHybridHealthService/reports/selecteddeployment/read | Seçilen dağıtımı verilen Kiracı için alır. |
> | Eylem | Microsoft.ADHybridHealthService/reports/tenantassigneddeployment/read | Kiracı kimliği alır verilen Kiracı depolama konumu. |
> | Eylem | Microsoft.ADHybridHealthService/reports/updateselecteddeployment/read | Veri erişileceği coğrafi konumunu alır. |
> | Eylem | Microsoft.ADHybridHealthService/services/action | Kiracı içinde bir hizmet örneği güncelleştirir. |
> | Eylem | Microsoft.ADHybridHealthService/services/alerts/read | Bir hizmet uyarılarını okur. |
> | Eylem | Microsoft.ADHybridHealthService/services/alerts/read | Bir hizmet uyarılarını okur. |
> | Eylem | Microsoft.ADHybridHealthService/services/checkservicefeatureavailibility/read | Bir özellik adı bir hizmet bu özelliği kullanmak için gereken her şeyi olup olmadığını doğrular. |
> | Eylem | Microsoft.ADHybridHealthService/services/delete | Kiracı içinde bir hizmet örneği siler. |
> | Eylem | Microsoft.ADHybridHealthService/services/exporterrors/read | Dışarı Aktarma hataları için verilen eşitleme hizmeti alır. |
> | Eylem | Microsoft.ADHybridHealthService/services/exportstatus/read | Verme durumu için belirli bir hizmeti alır. |
> | Eylem | Microsoft.ADHybridHealthService/services/feedbacktype/feedback/read | Verili hizmet ve sunucu için uyarıları geri bildirim alır. |
> | Eylem | Microsoft.ADHybridHealthService/services/metricmetadata/read | Belirli bir hizmeti için desteklenen ölçümleri listesini alır.<br>Örnek Extranet hesap kilitlemeleri, toplam başarısız istekler, bekleyen belirteç isteklerini (Proxy), belirteç isteklerini/sec ADFS hizmetinin vb. için.<br>NTLM kimlik doğrulamaları/sn, LDAP başarılı bağlantısı sayısı/sn, LDAP bağlama süresi, LDAP Etkin iş parçacığı, Kerberos kimlik doğrulamaları/sn vb. ATQ iş parçacıkları toplam ADDomainService için.<br>ADSync hizmeti için Azure AD için profil gecikme, TCP bağlantı kuran, Öngörüler Aracısı özel bayt sayısı, istatistikleri Dışarı Aktar'ı çalıştırın. |
> | Eylem | Microsoft.ADHybridHealthService/services/metrics/groups/average/read | Hizmet verilen, bu API belirli bir hizmeti için ölçümler için ortalama alır.<br>Örneğin, bu API ile ilgili bilgi almak için kullanılabilir: Extranet hesap kilitlemeleri, toplam başarısız istekler, bekleyen belirteç isteklerini (Proxy), belirteç isteklerini/sec vb. ADFederation hizmeti.<br>NTLM kimlik doğrulamaları/sn, LDAP başarılı bağlantısı sayısı/sn, LDAP bağlama süresi, LDAP Etkin iş parçacığı, Kerberos kimlik doğrulamaları/sn vb. ATQ iş parçacıkları toplam ADDomain hizmeti.<br>Profil gecikme Çalıştır TCP bağlantı kuran, Öngörüler Aracısı özel bayt sayısı, istatistikleri için Azure AD eşitleme hizmeti için dışarı aktarın. |
> | Eylem | Microsoft.ADHybridHealthService/services/metrics/groups/read | Hizmet verilen, bu API ölçümleri bilgilerini alır.<br>Örneğin, bu API ile ilgili bilgi almak için kullanılabilir: Extranet hesap kilitlemeleri, toplam başarısız istekler, bekleyen belirteç isteklerini (Proxy), belirteç isteklerini/sec vb. ADFederation hizmeti.<br>NTLM kimlik doğrulamaları/sn, LDAP başarılı bağlantısı sayısı/sn, LDAP bağlama süresi, LDAP Etkin iş parçacığı, Kerberos kimlik doğrulamaları/sn vb. ATQ iş parçacıkları toplam ADDomain hizmeti.<br>Profil gecikme Çalıştır TCP bağlantı kuran, Öngörüler Aracısı özel bayt sayısı, istatistikleri için Azure AD eşitleme hizmeti için dışarı aktarın. |
> | Eylem | Microsoft.ADHybridHealthService/services/metrics/groups/sum/read | Hizmet verilen, bu API için belirli bir hizmeti ölçümleri toplanmış görünümünü alır.<br>Örneğin, bu API ile ilgili bilgi almak için kullanılabilir: Extranet hesap kilitlemeleri, toplam başarısız istekler, bekleyen belirteç isteklerini (Proxy), belirteç isteklerini/sec vb. ADFederation hizmeti.<br>NTLM kimlik doğrulamaları/sn, LDAP başarılı bağlantısı sayısı/sn, LDAP bağlama süresi, LDAP Etkin iş parçacığı, Kerberos kimlik doğrulamaları/sn vb. ATQ iş parçacıkları toplam ADDomain hizmeti.<br>Profil gecikme Çalıştır TCP bağlantı kuran, Öngörüler Aracısı özel bayt sayısı, istatistikleri için Azure AD eşitleme hizmeti için dışarı aktarın. |
> | Eylem | Microsoft.ADHybridHealthService/services/monitoringconfiguration/write | Eklemek veya bir hizmet için izleme yapılandırmasını güncelleştirir. |
> | Eylem | Microsoft.ADHybridHealthService/services/monitoringconfigurations/read | Belirli bir hizmeti İzleme yapılandırmalarını alır. |
> | Eylem | Microsoft.ADHybridHealthService/services/monitoringconfigurations/write | Eklemek veya bir hizmet için izleme yapılandırmalarını güncelleştirir. |
> | Eylem | Microsoft.ADHybridHealthService/services/premiumcheck/read | Bu API premium kiracınız için tüm edildi hizmetlerin listesini alır. |
> | Eylem | Microsoft.ADHybridHealthService/services/read | Hizmet örnekleri kiracısında okur. |
> | Eylem | Microsoft.ADHybridHealthService/services/reports/details/read | Son 7 gün için hatalı parola hataları ile ilk 50 kullanıcı alır raporu |
> | Eylem | Microsoft.ADHybridHealthService/services/servicemembers/action | Bir sunucu örneği hizmetinde oluşturur. |
> | Eylem | Microsoft.ADHybridHealthService/services/servicemembers/alerts/read | Uyarılar için bir sunucu okur. |
> | Eylem | Microsoft.ADHybridHealthService/services/servicemembers/credentials/read | Sunucu kaydı sırasında bu API yeni sunucuları hazırlanması için kimlik bilgilerini almak için çağrılır. |
> | Eylem | Microsoft.ADHybridHealthService/services/servicemembers/datafreshness/read | Belli bir sunucu için bu API sunucuları ve her karşıya yükleme için en son saat olarak karşıya veri türleri listesini alır. |
> | Eylem | Microsoft.ADHybridHealthService/services/servicemembers/delete | Hizmet bir sunucu örneği siler. |
> | Eylem | Microsoft.ADHybridHealthService/services/servicemembers/exportstatus/read | Belirli bir eşitleme hizmeti için eşitleme verme hata ayrıntılarını alır. |
> | Eylem | Microsoft.ADHybridHealthService/services/servicemembers/metrics/groups/read | Hizmet verilen, bu API ölçümleri bilgilerini alır.<br>Örneğin, bu API ile ilgili bilgi almak için kullanılabilir: Extranet hesap kilitlemeleri, toplam başarısız istekler, bekleyen belirteç isteklerini (Proxy), belirteç isteklerini/sec vb. ADFederation hizmeti.<br>NTLM kimlik doğrulamaları/sn, LDAP başarılı bağlantısı sayısı/sn, LDAP bağlama süresi, LDAP Etkin iş parçacığı, Kerberos kimlik doğrulamaları/sn vb. ATQ iş parçacıkları toplam ADDomain hizmeti.<br>Profil gecikme Çalıştır TCP bağlantı kuran, Öngörüler Aracısı özel bayt sayısı, istatistikleri için Azure AD eşitleme hizmeti için dışarı aktarın. |
> | Eylem | Microsoft.ADHybridHealthService/services/servicemembers/read | Hizmet sunucu örneğinde okur. |
> | Eylem | Microsoft.ADHybridHealthService/services/servicemembers/serviceconfiguration/read | Belirli bir kiracı için hizmet yapılandırmasını alır. |
> | Eylem | Microsoft.ADHybridHealthService/services/tenantwhitelisting/read | Belirli bir kiracı için özellik uygulamaları güvenilir listeye almayı durumunu alır. |
> | Eylem | Microsoft.ADHybridHealthService/services/write | Bir hizmet örneği Kiracı oluşturur. |
> | Eylem | Microsoft.ADHybridHealthService/unregister/action | Abonelik ADHybrid sistem durumu hizmeti kaynak sağlayıcısı için kaydını siler. |

## <a name="microsoftadvisor"></a>Microsoft.Advisor

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Advisor/configurations/read | Yapılandırmalarını alma |
> | Eylem | Microsoft.Advisor/configurations/write | / Güncelleştirme yapılandırma |
> | Eylem | Microsoft.Advisor/generateRecommendations/action | Öneriler oluşturur |
> | Eylem | Microsoft.Advisor/generateRecommendations/read | Önerileri durumuna alır oluştur |
> | Eylem | Microsoft.Advisor/operations/read | Microsoft Advisor işlemlerinde alır |
> | Eylem | Microsoft.Advisor/recommendations/read | Öneriler okur |
> | Eylem | Microsoft.Advisor/recommendations/suppressions/delete | Gizleme siler |
> | Eylem | Microsoft.Advisor/recommendations/suppressions/read | Suppressions alır |
> | Eylem | Microsoft.Advisor/recommendations/suppressions/write | / Suppressions güncelleştirme |
> | Eylem | Microsoft.Advisor/register/action | Microsoft Advisor için aboneliği kaydeder |
> | Eylem | Microsoft.Advisor/suppressions/delete | Gizleme siler |
> | Eylem | Microsoft.Advisor/suppressions/read | Suppressions alır |
> | Eylem | Microsoft.Advisor/suppressions/write | / Suppressions güncelleştirme |
> | Eylem | Microsoft.Advisor/unregister/action | Abonelik için Microsoft Advisor kaydını siler |

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.AlertsManagement/alerts/changestate/action | Uyarı durumunu değiştirin. |
> | Eylem | Microsoft.AlertsManagement/alerts/read | Tüm uyarılar için giriş filtrelerini alır. |
> | Eylem | Microsoft.AlertsManagement/alertsSummary/read | Uyarıların özetini alın |
> | Eylem | Microsoft.AlertsManagement/Operations/read | Sağlanan işlemleri okur |
> | Eylem | Microsoft.AlertsManagement/smartGroups/changestate/action | Akıllı grup durumunu değiştirme |
> | Eylem | Microsoft.AlertsManagement/smartGroups/read | Tüm akıllı grupları giriş filtrelerini alma |

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.AnalysisServices/locations/checkNameAvailability/action | Analysis Server adının geçerli olup olmadığını denetler ve içinde değil kullanın. |
> | Eylem | Microsoft.AnalysisServices/locations/operationresults/read | Belirtilen işlem sonucu bilgileri alır. |
> | Eylem | Microsoft.AnalysisServices/locations/operationstatuses/read | Belirtilen işlem durumu bilgileri alır. |
> | Eylem | Microsoft.AnalysisServices/operations/read | İşlem bilgileri alır. |
> | Eylem | Microsoft.AnalysisServices/register/action | Analysis Services kaynağı sağlayıcısını kaydeder. |
> | Eylem | Microsoft.AnalysisServices/servers/delete | Analiz sunucusu siler. |
> | Eylem | Microsoft.AnalysisServices/servers/listGatewayStatus/action | Sunucuyla ilişkili ağ geçidi durumunu listeler. |
> | Eylem | Microsoft.AnalysisServices/servers/providers/Microsoft.Insights/diagnosticSettings/read | Analysis Server tanılama ayarını alır |
> | Eylem | Microsoft.AnalysisServices/servers/providers/Microsoft.Insights/diagnosticSettings/write | Oluşturur veya Analysis Server tanılama ayarını güncelleştirir |
> | Eylem | Microsoft.AnalysisServices/servers/providers/Microsoft.Insights/logDefinitions/read | Sunucuları için kullanılabilir günlüklerini alır |
> | Eylem | Microsoft.AnalysisServices/servers/providers/Microsoft.Insights/metricDefinitions/read | Analiz sunucusu için kullanılabilir ölçümleri alır |
> | Eylem | Microsoft.AnalysisServices/servers/read | Belirtilen Analysis Server'ın bilgilerini alır. |
> | Eylem | Microsoft.AnalysisServices/servers/resume/action | Analiz sunucusu sürdürür. |
> | Eylem | Microsoft.AnalysisServices/servers/skus/read | Sunucu için kullanılabilir SKU bilgilerini alma |
> | Eylem | Microsoft.AnalysisServices/servers/suspend/action | Analiz sunucusu askıya alır. |
> | Eylem | Microsoft.AnalysisServices/servers/write | Oluşturur veya belirtilen Analysis Server güncelleştirir. |
> | Eylem | Microsoft.AnalysisServices/skus/read | SKU'ları bilgileri alır. |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.ApiManagement/checkNameAvailability/read | Hizmet adı kullanılabilir sağladıysanız denetler |
> | Eylem | Microsoft.ApiManagement/operations/read | Tüm API kullanılabilir Microsoft.ApiManagement kaynak için okuma işlemleri |
> | Eylem | Microsoft.ApiManagement/register/action | Microsoft.ApiManagement kaynak sağlayıcısı için abonelik kaydı |
> | Eylem | Microsoft.ApiManagement/reports/read | Dönemleri, coğrafi bölge, geliştiriciler, ürünler, API'leri, işlemleri, abonelik ve byRequest tarafından toplanan raporlar alın. |
> | Eylem | Microsoft.ApiManagement/service/apis/delete | Varolan API Kaldır |
> | Eylem | Microsoft.ApiManagement/service/apis/diagnostics/delete | Varolan tanılama Kaldır |
> | Eylem | Microsoft.ApiManagement/service/apis/diagnostics/read | Tanılama veya tanılama Ayrıntıları Al listesini al |
> | Eylem | Microsoft.ApiManagement/service/apis/diagnostics/write | Yeni tanılama ekleme veya güncelleştirme mevcut tanılama ayrıntıları |
> | Eylem | Microsoft.ApiManagement/service/apis/issues/attachments/delete | Ek varolan kaldırır |
> | Eylem | Microsoft.ApiManagement/service/apis/issues/attachments/read | Sorunu ekler veya API Management alır sorunu ek ayrıntıları |
> | Eylem | Microsoft.ApiManagement/service/apis/issues/attachments/write | API sorunu eki ekleyin |
> | Eylem | Microsoft.ApiManagement/service/apis/issues/comments/delete | Açıklama varolan kaldırır |
> | Eylem | Microsoft.ApiManagement/service/apis/issues/comments/read | Açıklama ayrıntıları alır açıklamaları veya API Management alır sorunu sorun |
> | Eylem | Microsoft.ApiManagement/service/apis/issues/comments/write | API sorunu açıklama ekleme |
> | Eylem | Microsoft.ApiManagement/service/apis/issues/delete | Sorunu varolan kaldırır |
> | Eylem | Microsoft.ApiManagement/service/apis/issues/read | API veya API Management alır sorun ayrıntıları ile ilgili sorunlar Al |
> | Eylem | Microsoft.ApiManagement/service/apis/issues/write | API sorunu ekleyin veya API sorunu güncelleştirin |
> | Eylem | Microsoft.ApiManagement/service/apis/operations/delete | Varolan API işlemi Kaldır |
> | Eylem | Microsoft.ApiManagement/service/apis/operations/policies/delete | API işlemi ilkelerden ilke yapılandırmasını Kaldır |
> | Eylem | Microsoft.ApiManagement/service/apis/operations/policies/read | İlkeleri API işlemi veya alma ilkesi yapılandırma ayrıntıları alma için API işlemi |
> | Eylem | Microsoft.ApiManagement/service/apis/operations/policies/write | API işlem için ilke yapılandırma ayrıntılarını ayarlayın |
> | Eylem | Microsoft.ApiManagement/service/apis/operations/policy/delete | İlke yapılandırma işlemi Kaldır |
> | Eylem | Microsoft.ApiManagement/service/apis/operations/policy/read | İlke yapılandırma ayrıntılarını alma işlemi için |
> | Eylem | Microsoft.ApiManagement/service/apis/operations/policy/write | İşlem için ilke yapılandırma ayrıntılarını ayarlayın |
> | Eylem | Microsoft.ApiManagement/service/apis/operations/read | Mevcut API işlemleri listesini almak veya API işlemi ayrıntılarını alma |
> | Eylem | Microsoft.ApiManagement/service/apis/operations/tags/delete | Varolan bir etiketi mevcut işlemi ile ilişkisini silme |
> | Eylem | Microsoft.ApiManagement/service/apis/operations/tags/read | İşlem ya da almak etiketi ayrıntılarla ilişkili etiketleri Al |
> | Eylem | Microsoft.ApiManagement/service/apis/operations/tags/write | Varolan etiketi işlemle ilişkilendirme |
> | Eylem | Microsoft.ApiManagement/service/apis/operations/write | Yeni API işlemi oluşturma veya güncelleştirme mevcut API işlemi |
> | Eylem | Microsoft.ApiManagement/service/apis/operationsByTags/read | İşlem/etiketi ilişkilendirmeleri listesini al |
> | Eylem | Microsoft.ApiManagement/service/apis/policies/delete | API ilkelerden ilke yapılandırmasını Kaldır |
> | Eylem | Microsoft.ApiManagement/service/apis/policies/read | İlkeleri için API API veya alma ilkesi yapılandırma ayrıntılarını alma |
> | Eylem | Microsoft.ApiManagement/service/apis/policies/write | İçin API ilkesi yapılandırma ayrıntılarını ayarlayın |
> | Eylem | Microsoft.ApiManagement/service/apis/policy/delete | İlke Yapılandırması API öğesinden Kaldır |
> | Eylem | Microsoft.ApiManagement/service/apis/policy/read | İçin API ilkesi yapılandırma ayrıntılarını alma |
> | Eylem | Microsoft.ApiManagement/service/apis/policy/write | İçin API ilkesi yapılandırma ayrıntılarını ayarlayın |
> | Eylem | Microsoft.ApiManagement/service/apis/products/read | API parçası olan tüm ürünleri alma |
> | Eylem | Microsoft.ApiManagement/service/apis/read | Tüm kayıtlı API veya Get ayrıntılarını API listesini al |
> | Eylem | Microsoft.ApiManagement/service/apis/releases/delete | API ya da kaldırma API yayın tüm sürümleri kaldırır |
> | Eylem | Microsoft.ApiManagement/service/apis/releases/read | Bir API reelase API veya Get ayrıntılarını Get sürümleri |
> | Eylem | Microsoft.ApiManagement/service/apis/releases/write | Yeni API sürüm oluşturma veya varolan API sürümü güncelleştirme |
> | Eylem | Microsoft.ApiManagement/service/apis/revisions/delete | Bir API'nin tüm düzeltmelerini kaldırır |
> | Eylem | Microsoft.ApiManagement/service/apis/revisions/read | API'ye ait düzeltmelerini Al |
> | Eylem | Microsoft.ApiManagement/service/apis/schemas/delete | Şema varolan kaldırır |
> | Eylem | Microsoft.ApiManagement/service/apis/schemas/document/read | Şema açıklayan belge Al |
> | Eylem | Microsoft.ApiManagement/service/apis/schemas/document/write | Şema açıklayan belge güncelleştir |
> | Eylem | Microsoft.ApiManagement/service/apis/schemas/read | Belirli bir API için tüm şemaları alır veya API tarafından kullanılan şemaları alır |
> | Eylem | Microsoft.ApiManagement/service/apis/schemas/write | API tarafından kullanılan şemalar ayarlar |
> | Eylem | Microsoft.ApiManagement/service/apis/tagDescriptions/delete | Etiket açıklama API öğesinden Kaldır |
> | Eylem | Microsoft.ApiManagement/service/apis/tagDescriptions/read | Etiketler açıklamaları API veya alma etiketi açıklama kapsamında API kapsamında alın. |
> | Eylem | Microsoft.ApiManagement/service/apis/tagDescriptions/write | Oluştur/Değiştir etiketi açıklaması kapsamında API |
> | Eylem | Microsoft.ApiManagement/service/apis/tags/delete | Varolan API/etiketi ilişkilendirmesini Kaldır |
> | Eylem | Microsoft.ApiManagement/service/apis/tags/read | Tüm API/etiketi ilişkilendirme için API/etiketi ilişkilendirme API veya Get ayrıntılarını alma |
> | Eylem | Microsoft.ApiManagement/service/apis/tags/write | Yeni API/etiketi ilişkilendirmesini Ekle |
> | Eylem | Microsoft.ApiManagement/service/apis/write | Yeni API oluşturma veya güncelleştirme mevcut API ayrıntıları |
> | Eylem | Microsoft.ApiManagement/service/apisByTags/read | API/etiketi ilişkilendirmeleri listesini al |
> | Eylem | Microsoft.ApiManagement/service/api-version-sets/delete | Varolan VersionSet Kaldır |
> | Eylem | Microsoft.ApiManagement/service/api-version-sets/read | Sürüm grubu varlıkları veya bir VersionSet alır ayrıntılarını listesini al |
> | Eylem | Microsoft.ApiManagement/service/api-version-sets/versions/read | Sürüm varlıkların listesini al |
> | Eylem | Microsoft.ApiManagement/service/api-version-sets/write | Yeni VersionSet oluşturma veya güncelleştirme mevcut VersionSet ayrıntıları |
> | Eylem | Microsoft.ApiManagement/service/applynetworkconfigurationupdates/action | Güncelleştirilmiş ağ ayarları seçmek üzere sanal ağda çalışan Microsoft.ApiManagement kaynakları güncelleştirir. |
> | Eylem | Microsoft.ApiManagement/service/authorizationServers/delete | Varolan bir yetkilendirme sunucusu Kaldır |
> | Eylem | Microsoft.ApiManagement/service/authorizationServers/read | Yetkilendirme sunucularının listesini alın veya yetkilendirme sunucusu ayrıntılarını alma |
> | Eylem | Microsoft.ApiManagement/service/authorizationServers/write | Var olan bir yetkilendirme sunucusu Güncelleştirme ayrıntıları veya yeni bir yetkilendirme sunucusu oluşturma |
> | Eylem | Microsoft.ApiManagement/service/backends/delete | Varolan arka uç Kaldır |
> | Eylem | Microsoft.ApiManagement/service/backends/read | Arka uçlarını listesini almak veya arka uç ayrıntılarını alma |
> | Eylem | Microsoft.ApiManagement/service/backends/reconnect/action | Yeniden bağlanma isteği oluşturma |
> | Eylem | Microsoft.ApiManagement/service/backends/write | Yeni bir arka uç ekleme veya güncelleştirme mevcut arka uç ayrıntıları |
> | Eylem | Microsoft.ApiManagement/service/backup/action | Bir kullanıcı belirtilen kapsayıcıda yedekleme API Management hizmetine sağlanan depolama hesabı |
> | Eylem | Microsoft.ApiManagement/service/certificates/delete | Varolan bir sertifikayı Kaldır |
> | Eylem | Microsoft.ApiManagement/service/certificates/read | Sertifikaların listesini alın veya sertifika ayrıntılarını alma |
> | Eylem | Microsoft.ApiManagement/service/certificates/write | Yeni sertifika Ekle |
> | Eylem | Microsoft.ApiManagement/service/delete | API Management hizmeti örneği Sil |
> | Eylem | Microsoft.ApiManagement/service/diagnostics/delete | Varolan tanılama Kaldır |
> | Eylem | Microsoft.ApiManagement/service/diagnostics/read | Tanılama veya tanılama Ayrıntıları Al listesini al |
> | Eylem | Microsoft.ApiManagement/service/diagnostics/write | Yeni tanılama ekleme veya güncelleştirme mevcut tanılama ayrıntıları |
> | Eylem | Microsoft.ApiManagement/service/getssotoken/action | Kullanılabilir alır SSO belirteci için API Management hizmet eski portalı yönetici olarak oturum aç |
> | Eylem | Microsoft.ApiManagement/service/groups/delete | Varolan bir grubu Kaldır |
> | Eylem | Microsoft.ApiManagement/service/groups/read | Grupları veya bir grup alır ayrıntılarını listesini al |
> | Eylem | Microsoft.ApiManagement/service/groups/users/delete | Varolan bir kullanıcı var olan grubundan Kaldır |
> | Eylem | Microsoft.ApiManagement/service/groups/users/read | Grup kullanıcıların listesini al |
> | Eylem | Microsoft.ApiManagement/service/groups/users/write | Varolan bir gruba varolan kullanıcı Ekle |
> | Eylem | Microsoft.ApiManagement/service/groups/write | Yeni grubu oluşturmak veya var olan Grup ayrıntılarını güncelleştir |
> | Eylem | Microsoft.ApiManagement/service/identityProviders/delete | Varolan kimlik sağlayıcısı Kaldır |
> | Eylem | Microsoft.ApiManagement/service/identityProviders/read | Kimlik sağlayıcısı Get ayrıntıları veya kimlik sağlayıcıları listesini al |
> | Eylem | Microsoft.ApiManagement/service/identityProviders/write | Varolan bir kimlik sağlayıcısı'nın yeni bir kimlik sağlayıcısı veya Güncelleştirme ayrıntıları oluşturun |
> | Eylem | Microsoft.ApiManagement/service/locations/networkstatus/read | Ağ erişim durumu, hizmetin bağımlı olduğu kaynaklar konumu alır. |
> | Eylem | Microsoft.ApiManagement/service/loggers/delete | Varolan Günlükçü Kaldır |
> | Eylem | Microsoft.ApiManagement/service/loggers/read | Günlükçüleri listesini almak veya Günlükçü ayrıntılarını alma |
> | Eylem | Microsoft.ApiManagement/service/loggers/write | Yeni Günlükçü ekleme veya güncelleştirme mevcut Günlükçü ayrıntıları |
> | Eylem | Microsoft.ApiManagement/service/managedeployments/action | SKU/birimlerini, API Management Hizmet Ekle/Kaldır bölgesel dağıtımları değiştirme |
> | Eylem | Microsoft.ApiManagement/service/networkstatus/read | Hizmetin bağımlı olduğu kaynaklar ağ erişim durumunu alır. |
> | Eylem | Microsoft.ApiManagement/service/notifications/action | Belirtilen bir kullanıcıya bildirim gönderir |
> | Eylem | Microsoft.ApiManagement/service/notifications/read | Tüm API Management yayımcı bildirimleri veya alma API Management yayımcı bildirim ayrıntılarını alır |
> | Eylem | Microsoft.ApiManagement/service/notifications/recipientEmails/delete | Var olan e-posta bildirim ile ilişkilendirilmiş kaldırır |
> | Eylem | Microsoft.ApiManagement/service/notifications/recipientEmails/read | E-posta alıcılarını API Management yayımcı bildirim ile ilişkilendirilmiş Al |
> | Eylem | Microsoft.ApiManagement/service/notifications/recipientEmails/write | Yeni e-posta alıcısının bildirim oluşturma |
> | Eylem | Microsoft.ApiManagement/service/notifications/recipientUsers/delete | Bildirim alıcılarını ilişkili kullanıcıyı kaldırır |
> | Eylem | Microsoft.ApiManagement/service/notifications/recipientUsers/read | Alıcı bildirim ile ilişkilendirilmiş kullanıcılar |
> | Eylem | Microsoft.ApiManagement/service/notifications/recipientUsers/write | Kullanıcı için bildirim alıcıları ekleyin |
> | Eylem | Microsoft.ApiManagement/service/notifications/write | Oluşturma veya güncelleştirme API Management yayımcı bildirim |
> | Eylem | Microsoft.ApiManagement/service/openidConnectProviders/delete | Varolan Openıd Connect sağlayıcısını Kaldır |
> | Eylem | Microsoft.ApiManagement/service/openidConnectProviders/read | Openıd Connect sağlayıcısı Get ayrıntıları veya Openıd Connect sağlayıcıları listesini al |
> | Eylem | Microsoft.ApiManagement/service/openidConnectProviders/write | Bağlantı varolan bir Openıd sağlayıcısının yeni bir Openıd Connect sağlayıcısı veya Güncelleştirme ayrıntıları oluşturun |
> | Eylem | Microsoft.ApiManagement/service/operationresults/read | Uzun süre çalışan işlemin geçerli durumunu alır |
> | Eylem | Microsoft.ApiManagement/service/policies/delete | Kiracı ilkelerden ilke yapılandırmasını Kaldır |
> | Eylem | Microsoft.ApiManagement/service/policies/read | İlkeleri Kiracısı için Kiracı veya alma ilkesi yapılandırma ayrıntılarını alma |
> | Eylem | Microsoft.ApiManagement/service/policies/write | Kiracı için ilke yapılandırma ayrıntılarını ayarlayın |
> | Eylem | Microsoft.ApiManagement/service/policySnippets/read | Tüm ilke parçacıkları Al |
> | Eylem | Microsoft.ApiManagement/service/portalsettings/read | Portalı veya Get portalı için ayarları'nda oturum açın veya Portal için temsilci ayarlarını almak için oturum ayarlarını al |
> | Eylem | Microsoft.ApiManagement/service/portalsettings/write | Kaydol ayarları veya güncelleştirme Kaydol ayarları veya güncelleştirme oturum ayarları veya güncelleştirme oturum ayarları veya güncelleştirme temsilci ayarları veya güncelleştirme temsilci ayarlarını güncelleştirme |
> | Eylem | Microsoft.ApiManagement/service/products/apis/delete | Var olan ürün varolan API Kaldır |
> | Eylem | Microsoft.ApiManagement/service/products/apis/read | Varolan bir ürüne eklenmiş API'leri listesini al |
> | Eylem | Microsoft.ApiManagement/service/products/apis/write | Varolan bir ürüne varolan API ekleme |
> | Eylem | Microsoft.ApiManagement/service/products/delete | Varolan ürünü kaldırın |
> | Eylem | Microsoft.ApiManagement/service/products/groups/delete | Varolan Ürün mevcut Geliştirici grubunun ilişkilendirmesini Sil |
> | Eylem | Microsoft.ApiManagement/service/products/groups/read | Ürünle ilişkili Geliştirici gruplarının listesini alma |
> | Eylem | Microsoft.ApiManagement/service/products/groups/write | Varolan bir geliştirici Grup varolan ürünle ilişkilendirin |
> | Eylem | Microsoft.ApiManagement/service/products/policies/delete | Ürün ilkelerden ilke yapılandırmasını Kaldır |
> | Eylem | Microsoft.ApiManagement/service/products/policies/read | İlkeleri ürün veya alma ilkesi yapılandırma ayrıntıları için ürün alın. |
> | Eylem | Microsoft.ApiManagement/service/products/policies/write | İlke yapılandırma ayrıntıları için ürün ayarlayın |
> | Eylem | Microsoft.ApiManagement/service/products/policy/delete | Var olan ürün ilkesi yapılandırmasını Kaldır |
> | Eylem | Microsoft.ApiManagement/service/products/policy/read | Varolan ürünün ilke yapılandırmasını alma |
> | Eylem | Microsoft.ApiManagement/service/products/policy/write | Varolan bir ürün için ilke yapılandırma kümesi |
> | Eylem | Microsoft.ApiManagement/service/products/read | Ürünlerin listesini al veya ürün ayrıntılarını Al |
> | Eylem | Microsoft.ApiManagement/service/products/subscriptions/read | Ürün Aboneliklerin listesini alın |
> | Eylem | Microsoft.ApiManagement/service/products/tags/delete | Varolan bir etiketi mevcut ürün ile ilişkisini silme |
> | Eylem | Microsoft.ApiManagement/service/products/tags/read | Ürün veya alma etiketi ayrıntılarla ilişkili etiketleri Al |
> | Eylem | Microsoft.ApiManagement/service/products/tags/write | Varolan bir etiketi mevcut ürünle ilişkilendirin |
> | Eylem | Microsoft.ApiManagement/service/products/write | Yeni ürün oluşturmak veya mevcut ürün ayrıntıları güncelleştir |
> | Eylem | Microsoft.ApiManagement/service/productsByTags/read | Ürün/etiketi ilişkilendirmeleri listesini al |
> | Eylem | Microsoft.ApiManagement/service/properties/delete | Özelliği varolan kaldırır |
> | Eylem | Microsoft.ApiManagement/service/properties/read | Belirtilen özellik ayrıntılarını alır veya tüm özelliklerinin listesini alır |
> | Eylem | Microsoft.ApiManagement/service/properties/write | Yeni bir özellik oluşturur veya belirtilen özellik için değer güncelleştirir |
> | Eylem | Microsoft.ApiManagement/service/providers/Microsoft.Insights/diagnosticSettings/read | API Management hizmeti tanılama ayarını alır |
> | Eylem | Microsoft.ApiManagement/service/providers/Microsoft.Insights/diagnosticSettings/write | Oluşturur veya API Management hizmeti tanılama ayarını güncelleştirir |
> | Eylem | Microsoft.ApiManagement/service/providers/Microsoft.Insights/logDefinitions/read | API Management hizmeti için kullanılabilir günlüklerini alır |
> | Eylem | Microsoft.ApiManagement/service/providers/Microsoft.Insights/metricDefinitions/read | API Management hizmeti için kullanılabilir ölçümleri alır |
> | Eylem | Microsoft.ApiManagement/service/quotas/periods/read | Dönem için kota sayaç değeri Al |
> | Eylem | Microsoft.ApiManagement/service/quotas/periods/write | Kota sayaç geçerli değeri ayarlayın |
> | Eylem | Microsoft.ApiManagement/service/quotas/read | İçin kota değerlerini alma |
> | Eylem | Microsoft.ApiManagement/service/quotas/write | Kota sayaç geçerli değeri ayarlayın |
> | Eylem | Microsoft.ApiManagement/service/read | API Management hizmeti örneği ile ilgili meta verilerini okuma |
> | Eylem | Microsoft.ApiManagement/service/reports/read | Dönemleri ya da coğrafi bölge veya geliştiriciler tarafından toplanan Get rapor ile toplanmış Get raporu tarafından toplanan rapor alın.<br>veya ürünü tarafından toplanan rapor alın.<br>veya işlemleri veya abonelik tarafından toplanan Get raporu tarafından toplanan API'leri veya Get raporu tarafından toplanan rapor alın.<br>veya istekleri raporlama verilerini alma |
> | Eylem | Microsoft.ApiManagement/service/restore/action | API Management hizmeti belirtilen bir kullanıcı tarafından sağlanan depolama hesabı kapsayıcısında geri yükleme |
> | Eylem | Microsoft.ApiManagement/service/subscriptions/delete | Aboneliği silin. Bu işlem aboneliğini silmek için kullanılabilir |
> | Eylem | Microsoft.ApiManagement/service/subscriptions/read | Ürün Aboneliklerin listesini alın veya ürün aboneliği ayrıntılarını alma |
> | Eylem | Microsoft.ApiManagement/service/subscriptions/regeneratePrimaryKey/action | Abonelik birincil anahtarını yeniden oluşturma |
> | Eylem | Microsoft.ApiManagement/service/subscriptions/regenerateSecondaryKey/action | Abonelik ikincil anahtarını yeniden oluşturma |
> | Eylem | Microsoft.ApiManagement/service/subscriptions/write | Mevcut bir kullanıcının var olan bir ürüne abone veya var olan abonelik ayrıntıları güncelleştirin. Bu işlem, aboneliğinizi yenilemek için kullanılabilir |
> | Eylem | Microsoft.ApiManagement/service/tagResources/read | İlişkilendirilmiş kaynakları olan etiketleri listesini al |
> | Eylem | Microsoft.ApiManagement/service/tags/delete | Varolan etiketi Kaldır |
> | Eylem | Microsoft.ApiManagement/service/tags/read | Etiketler listesini almak veya etiket ayrıntılarını alma |
> | Eylem | Microsoft.ApiManagement/service/tags/write | Yeni etiket eklemek veya mevcut etiket ayrıntılarını güncelleştir |
> | Eylem | Microsoft.ApiManagement/service/templates/delete | Varsayılan API Management e-posta şablonu Sıfırla |
> | Eylem | Microsoft.ApiManagement/service/templates/read | Tüm e-posta şablonlarını veya API Management alır e-posta şablonu ayrıntılarını alır |
> | Eylem | Microsoft.ApiManagement/service/templates/write | API Management e-posta şablonu ya güncelleştirmeleri API Management e-posta şablonu oluştur veya güncelleştir |
> | Eylem | Microsoft.ApiManagement/service/tenant/delete | Kiracı için ilke yapılandırmasını kaldırın |
> | Eylem | Microsoft.ApiManagement/service/tenant/deploy/action | Değişiklikler veritabanına yapılandırmasında belirtilen git daldan uygulamak için bir dağıtım görev çalıştırır. |
> | Eylem | Microsoft.ApiManagement/service/tenant/operationResults/read | İşlem sonuçları listesini al veya belirli bir işlemin sonucunu Al |
> | Eylem | Microsoft.ApiManagement/service/tenant/read | İlke Yapılandırması Kiracı veya Get Kiracı için erişim bilgileri ayrıntıları |
> | Eylem | Microsoft.ApiManagement/service/tenant/regeneratePrimaryKey/action | Birincil erişim anahtarını yeniden oluşturma |
> | Eylem | Microsoft.ApiManagement/service/tenant/regenerateSecondaryKey/action | İkincil erişim anahtarını yeniden oluşturma |
> | Eylem | Microsoft.ApiManagement/service/tenant/save/action | Yürütme deposunda belirtilen dal yapılandırma anlık görüntü oluşturur |
> | Eylem | Microsoft.ApiManagement/service/tenant/syncState/read | En son git eşitleme durumunu Al |
> | Eylem | Microsoft.ApiManagement/service/tenant/validate/action | Belirtilen git şube değişikliklerden doğrular |
> | Eylem | Microsoft.ApiManagement/service/tenant/write | Kiracı veya güncelleştirme Kiracı erişim bilgileri ayrıntıları ilke yapılandırmasını ayarlayın |
> | Eylem | Microsoft.ApiManagement/service/updatecertificate/action | Bir API Management hizmeti için SSL sertifikasını karşıya yükle |
> | Eylem | Microsoft.ApiManagement/service/updatehostname/action | Kurulum, güncelleştirmeniz ya da bir API Management hizmeti için özel etki alanı adlarını kaldırın |
> | Eylem | Microsoft.ApiManagement/service/users/action | Yeni bir kullanıcı kaydı |
> | Eylem | Microsoft.ApiManagement/service/users/applications/attachments/delete | Bir ek kaldırır |
> | Eylem | Microsoft.ApiManagement/service/users/applications/attachments/read | Uygulama ekler veya alır eki alır |
> | Eylem | Microsoft.ApiManagement/service/users/applications/attachments/write | Uygulama eki ekleyin |
> | Eylem | Microsoft.ApiManagement/service/users/applications/delete | Uygulama varolan kaldırır |
> | Eylem | Microsoft.ApiManagement/service/users/applications/read | Tüm kullanıcı uygulamaları veya API Management alır uygulama ayrıntıları listesini al |
> | Eylem | Microsoft.ApiManagement/service/users/applications/write | API Management veya güncelleştirmeler uygulama ayrıntıları uygulamaya kaydeder |
> | Eylem | Microsoft.ApiManagement/service/users/delete | Kullanıcı hesabını kaldır |
> | Eylem | Microsoft.ApiManagement/service/users/generateSsoUrl/action | SSO URL oluşturur. URL yönetim portalına erişmek için kullanılabilir |
> | Eylem | Microsoft.ApiManagement/service/users/groups/read | Kullanıcı gruplarının listesini alma |
> | Eylem | Microsoft.ApiManagement/service/users/keys/read | Kullanıcı anahtarları listesini al |
> | Eylem | Microsoft.ApiManagement/service/users/read | Kayıtlı kullanıcıların listesini almak veya bir kullanıcı hesabı ayrıntılarını alma |
> | Eylem | Microsoft.ApiManagement/service/users/subscriptions/read | Kullanıcı abonelikleri listesini al |
> | Eylem | Microsoft.ApiManagement/service/users/token/action | Bir kullanıcı için belirteç erişim belirteci alma |
> | Eylem | Microsoft.ApiManagement/service/users/write | Yeni bir kullanıcı veya varolan bir kullanıcı hesabı ayrıntılarını güncelleştirme kaydetme |
> | Eylem | Microsoft.ApiManagement/service/write | API Management hizmeti yeni bir örneğini oluşturma |
> | Eylem | Microsoft.ApiManagement/unregister/action | Microsoft.ApiManagement kaynak sağlayıcısı için abonelik kaydı |

## <a name="microsoftauthorization"></a>Microsoft.Authorization

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Authorization/checkAccess/action | Arayanın belirli bir eylemi gerçekleştirmek için yetkili olup olmadığını denetler |
> | Eylem | Microsoft.Authorization/classicAdministrators/delete | Yöneticiyi abonelikten çıkarır. |
> | Eylem | Microsoft.Authorization/classicAdministrators/read | Abonelik için yöneticileri okur. |
> | Eylem | Microsoft.Authorization/classicAdministrators/write | Abonelik için yönetici ekleyin veya değiştirin. |
> | Eylem | Microsoft.Authorization/elevateAccess/action | Çağırana kiracı kapsamında Kullanıcı Erişim Yöneticisi erişimi verir |
> | Eylem | Microsoft.Authorization/locks/delete | Belirtilen kapsamdaki kilitleri silin. |
> | Eylem | Microsoft.Authorization/locks/read | Belirtilen kapsamdaki kilitleri alın. |
> | Eylem | Microsoft.Authorization/locks/write | Belirtilen kapsamdaki kilitleri ekleyin. |
> | Eylem | Microsoft.Authorization/permissions/read | Arayanın, belirtilen bir kapsamda sahip olduğu tüm izinleri listeler. |
> | Eylem | Microsoft.Authorization/policyAssignments/delete | Belirtilen kapsamdaki bir ilke atamasını silin. |
> | Eylem | Microsoft.Authorization/policyAssignments/read | İlke ataması hakkında bilgi edinin. |
> | Eylem | Microsoft.Authorization/policyAssignments/write | Belirtilen kapsamda bir ilke ataması oluşturun. |
> | Eylem | Microsoft.Authorization/policyDefinitions/delete | İlke tanımını silin. |
> | Eylem | Microsoft.Authorization/policyDefinitions/read | İlke tanımı hakkında bilgi edinin. |
> | Eylem | Microsoft.Authorization/policyDefinitions/write | Özel bir ilke tanımı oluşturun. |
> | Eylem | Microsoft.Authorization/policySetDefinitions/delete | Bir ilke kümesi tanımını silin. |
> | Eylem | Microsoft.Authorization/policySetDefinitions/read | Bir ilke kümesi tanımı hakkında bilgi alın. |
> | Eylem | Microsoft.Authorization/policySetDefinitions/write | Özel bir ilke kümesi tanımı oluşturun. |
> | Eylem | Microsoft.Authorization/providerOperations/read | Tüm kaynak sağlayıcılarına ilişkin işlemleri alın. Bunlar rol tanımlarında kullanılabilir. |
> | Eylem | Microsoft.Authorization/roleAssignments/delete | Belirtilen kapsamdaki rol atamasını silin. |
> | Eylem | Microsoft.Authorization/roleAssignments/read | Bir rol ataması hakkında bilgi alın. |
> | Eylem | Microsoft.Authorization/roleAssignments/write | Belirtilen kapsamda bir rol ataması oluşturun. |
> | Eylem | Microsoft.Authorization/roleDefinitions/delete | Belirtilen özel rol tanımını silin. |
> | Eylem | Microsoft.Authorization/roleDefinitions/read | Rol tanımı hakkında bilgi alın. |
> | Eylem | Microsoft.Authorization/roleDefinitions/write | Belirtilen izinlerle ve atanabilir kapsamlarla birlikte özel bir rol tanımı oluşturun veya güncelleştirin. |

## <a name="microsoftautomation"></a>Microsoft.Automation

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Automation/automationAccounts/agentRegistrationInformation/read | Bir Azure Otomasyonu DSC'SİNİN kayıt bilgileri okuyun |
> | Eylem | Microsoft.Automation/automationAccounts/agentRegistrationInformation/regenerateKey/action | Azure Otomasyonu DSC anahtarları yeniden isteği Yazar |
> | Eylem | Microsoft.Automation/automationAccounts/certificates/delete | Bir Azure Otomasyonu sertifika varlığını siler |
> | Eylem | Microsoft.Automation/automationAccounts/certificates/getCount/action | Sertifikaları sayısını okur |
> | Eylem | Microsoft.Automation/automationAccounts/certificates/read | Bir Azure Otomasyonu sertifika varlığı alır |
> | Eylem | Microsoft.Automation/automationAccounts/certificates/write | Bir Azure Otomasyonu sertifika varlığı oluşturur veya güncelleştirir |
> | Eylem | Microsoft.Automation/automationAccounts/compilationjobs/read | Bir Azure Otomasyonu DSC'SİNİN derleme okur |
> | Eylem | Microsoft.Automation/automationAccounts/compilationjobs/read | Bir Azure Otomasyonu DSC'SİNİN derleme okur |
> | Eylem | Microsoft.Automation/automationAccounts/compilationjobs/write | Bir Azure Otomasyonu DSC'SİNİN derleme Yazar |
> | Eylem | Microsoft.Automation/automationAccounts/compilationjobs/write | Bir Azure Otomasyonu DSC'SİNİN derleme Yazar |
> | Eylem | Microsoft.Automation/automationAccounts/configurations/content/read | Yapılandırma medya içeriği okur |
> | Eylem | Microsoft.Automation/automationAccounts/configurations/delete | Bir Azure Otomasyonu DSC'SİNİN içeriğini siler |
> | Eylem | Microsoft.Automation/automationAccounts/configurations/getCount/action | Bir Azure Otomasyonu DSC'SİNİN içeriğini sayısını okur |
> | Eylem | Microsoft.Automation/automationAccounts/configurations/read | Bir Azure Otomasyonu DSC'SİNİN içeriğini alır |
> | Eylem | Microsoft.Automation/automationAccounts/configurations/write | Bir Azure Otomasyonu DSC'SİNİN içeriğini yazar |
> | Eylem | Microsoft.Automation/automationAccounts/connections/delete | Bir Azure Otomasyonu bağlantı varlığını siler |
> | Eylem | Microsoft.Automation/automationAccounts/connections/getCount/action | Bağlantı sayısını okur |
> | Eylem | Microsoft.Automation/automationAccounts/connections/read | Bir Azure Otomasyonu bağlantı varlığı alır |
> | Eylem | Microsoft.Automation/automationAccounts/connections/write | Bir Azure Otomasyonu bağlantı varlığı oluşturur veya güncelleştirir |
> | Eylem | Microsoft.Automation/automationAccounts/connectionTypes/delete | Bir Azure Otomasyonu bağlantı türü varlığını siler |
> | Eylem | Microsoft.Automation/automationAccounts/connectionTypes/read | Bir Azure Otomasyonu bağlantı türü varlığını alır |
> | Eylem | Microsoft.Automation/automationAccounts/connectionTypes/write | Bir Azure Otomasyonu bağlantı türü varlığı oluşturur |
> | Eylem | Microsoft.Automation/automationAccounts/credentials/delete | Bir Azure Otomasyonu kimlik bilgisi varlığını siler |
> | Eylem | Microsoft.Automation/automationAccounts/credentials/getCount/action | Kimlik bilgileri sayısını okur |
> | Eylem | Microsoft.Automation/automationAccounts/credentials/read | Bir Azure Otomasyonu kimlik bilgisi varlığı alır |
> | Eylem | Microsoft.Automation/automationAccounts/credentials/write | Bir Azure Otomasyonu kimlik bilgisi varlığı oluşturur veya güncelleştirir |
> | Eylem | Microsoft.Automation/automationAccounts/delete | Bir Azure Otomasyonu hesabını siler |
> | Eylem | Microsoft.Automation/automationAccounts/diagnosticSettings/read | Kaynak için tanılama ayarını alır |
> | Eylem | Microsoft.Automation/automationAccounts/diagnosticSettings/write | Kaynağın tanılama ayarını ayarlar |
> | Eylem | Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/delete | Karma Runbook çalışanı kaynaklarını siler |
> | Eylem | Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read | Karma Runbook çalışanı kaynaklarını okur |
> | Eylem | Microsoft.Automation/automationAccounts/jobs/output/read | Bir işin çıktısını alır |
> | Eylem | Microsoft.Automation/automationAccounts/jobs/read | Bir Azure Otomasyonu işini alır |
> | Eylem | Microsoft.Automation/automationAccounts/jobs/resume/action | Bir Azure Otomasyonu işini sürdürür |
> | Eylem | Microsoft.Automation/automationAccounts/jobs/runbookContent/action | İş yürütme sırasında Azure Otomasyonu runbook içeriğini alır |
> | Eylem | Microsoft.Automation/automationAccounts/jobs/stop/action | Bir Azure Otomasyonu işini durdurur |
> | Eylem | Microsoft.Automation/automationAccounts/jobs/streams/read | Bir Azure Otomasyonu iş akışını alır |
> | Eylem | Microsoft.Automation/automationAccounts/jobs/streams/read | Bir Azure Otomasyonu iş akışını alır |
> | Eylem | Microsoft.Automation/automationAccounts/jobs/suspend/action | Bir Azure Otomasyonu işini askıya alır |
> | Eylem | Microsoft.Automation/automationAccounts/jobs/write | Bir Azure Otomasyonu işi oluşturur |
> | Eylem | Microsoft.Automation/automationAccounts/jobSchedules/delete | Bir Azure Otomasyonu iş zamanlaması silme |
> | Eylem | Microsoft.Automation/automationAccounts/jobSchedules/read | Bir Azure Otomasyonu iş zamanlaması alır |
> | Eylem | Microsoft.Automation/automationAccounts/jobSchedules/write | Bir Azure Otomasyonu iş zamanlaması oluşturur |
> | Eylem | Microsoft.Automation/automationAccounts/linkedWorkspace/read | Otomasyon hesabı bağlı çalışma alır |
> | Eylem | Microsoft.Automation/automationAccounts/listKeys/action | Otomasyon hesabı için anahtarlar okur |
> | Eylem | Microsoft.Automation/automationAccounts/logDefinitions/read | Otomasyon hesabının kullanılabilir günlüklerini alır |
> | Eylem | Microsoft.Automation/automationAccounts/modules/activities/read | Azure Otomasyon etkinliklerini alır |
> | Eylem | Microsoft.Automation/automationAccounts/modules/delete | Bir Azure Otomasyonu modülünü siler |
> | Eylem | Microsoft.Automation/automationAccounts/modules/getCount/action | Otomasyon hesabı içinde modüllerin sayısını alır |
> | Eylem | Microsoft.Automation/automationAccounts/modules/read | Bir Azure Otomasyonu modülünü alır |
> | Eylem | Microsoft.Automation/automationAccounts/modules/write | Oluşturur veya bir Azure Otomasyonu modülü güncelleştirir |
> | Eylem | Microsoft.Automation/automationAccounts/nodeConfigurations/delete | Bir Azure Otomasyonu DSC'SİNİN düğüm yapılandırması siler |
> | Eylem | Microsoft.Automation/automationAccounts/nodeConfigurations/rawContent/action | Bir Azure Otomasyonu DSC'SİNİN düğüm yapılandırması içeriğini okur |
> | Eylem | Microsoft.Automation/automationAccounts/nodeConfigurations/read | Bir Azure Otomasyonu DSC'SİNİN düğüm yapılandırması okur |
> | Eylem | Microsoft.Automation/automationAccounts/nodeConfigurations/write | Bir Azure Otomasyonu DSC'SİNİN düğüm yapılandırması Yazar |
> | Eylem | Microsoft.Automation/automationAccounts/nodecounts/read | Belirtilen tür için Özet düğüm sayısını okur |
> | Eylem | Microsoft.Automation/automationAccounts/nodes/delete | Azure Otomasyonu DSC düğümleri siler |
> | Eylem | Microsoft.Automation/automationAccounts/nodes/read | Azure Otomasyonu DSC düğümleri okur |
> | Eylem | Microsoft.Automation/automationAccounts/nodes/reports/content/read | Azure Otomasyonu DSC rapor içeriğini okur |
> | Eylem | Microsoft.Automation/automationAccounts/nodes/reports/read | Azure Otomasyonu DSC raporları okur |
> | Eylem | Microsoft.Automation/automationAccounts/nodes/write | Oluşturur veya Azure Otomasyonu DSC düğümleri güncelleştirir |
> | Eylem | Microsoft.Automation/automationAccounts/objectDataTypes/fields/read | Azure Otomasyonu TypeFields alır |
> | Eylem | Microsoft.Automation/automationAccounts/providers/Microsoft.Insights/metricDefinitions/read | Otomasyon ölçüm tanımlarını alır |
> | Eylem | Microsoft.Automation/automationAccounts/read | Bir Azure Otomasyonu hesabını alır |
> | Eylem | Microsoft.Automation/automationAccounts/runbooks/content/read | Bir Azure Otomasyonu runbook içeriğini alır |
> | Eylem | Microsoft.Automation/automationAccounts/runbooks/delete | Bir Azure Otomasyonu runbook'unu siler |
> | Eylem | Microsoft.Automation/automationAccounts/runbooks/draft/content/write | Bir Azure Otomasyonu runbook taslağının içeriğini oluşturur |
> | Eylem | Microsoft.Automation/automationAccounts/runbooks/draft/operationResults/read | Azure Otomasyonu runbook taslağı İşlem sonuçlarını alır |
> | Eylem | Microsoft.Automation/automationAccounts/runbooks/draft/read | Bir Azure Otomasyonu runbook taslağı alır |
> | Eylem | Microsoft.Automation/automationAccounts/runbooks/draft/testJob/read | Bir Azure Otomasyonu runbook taslağı test işini alır |
> | Eylem | Microsoft.Automation/automationAccounts/runbooks/draft/testJob/resume/action | Bir Azure Otomasyonu runbook taslağı test işini sürdürür |
> | Eylem | Microsoft.Automation/automationAccounts/runbooks/draft/testJob/stop/action | Bir Azure Otomasyonu runbook taslağı test işini durdurur |
> | Eylem | Microsoft.Automation/automationAccounts/runbooks/draft/testJob/suspend/action | Bir Azure Otomasyonu runbook taslağı test işini askıya alır |
> | Eylem | Microsoft.Automation/automationAccounts/runbooks/draft/testJob/write | Bir Azure Otomasyonu runbook taslağı test işi oluşturur |
> | Eylem | Microsoft.Automation/automationAccounts/runbooks/draft/undoEdit/action | Bir Azure Otomasyonu runbook taslağı düzenlemeleri geri alma |
> | Eylem | Microsoft.Automation/automationAccounts/runbooks/getCount/action | Azure Otomasyon çalışma kitabı sayısını alır |
> | Eylem | Microsoft.Automation/automationAccounts/runbooks/publish/action | Bir Azure Otomasyonu runbook taslağı yayımlar |
> | Eylem | Microsoft.Automation/automationAccounts/runbooks/read | Bir Azure Otomasyonu runbook'unu alır |
> | Eylem | Microsoft.Automation/automationAccounts/runbooks/write | Oluşturur veya bir Azure Otomasyonu runbook'u güncelleştirir |
> | Eylem | Microsoft.Automation/automationAccounts/schedules/delete | Bir Azure Otomasyonu zamanlama varlığını siler |
> | Eylem | Microsoft.Automation/automationAccounts/schedules/getCount/action | Azure Otomasyonu zamanlama sayısı alır |
> | Eylem | Microsoft.Automation/automationAccounts/schedules/read | Bir Azure Otomasyonu zamanlama varlığını alır |
> | Eylem | Microsoft.Automation/automationAccounts/schedules/write | Bir Azure Otomasyonu zamanlama varlığı oluşturur veya güncelleştirir |
> | Eylem | Microsoft.Automation/automationAccounts/softwareUpdateConfigurations/delete | Bir Azure Otomasyonu yazılım güncelleştirme yapılandırmasını siler |
> | Eylem | Microsoft.Automation/automationAccounts/softwareUpdateConfigurations/read | Bir Azure Otomasyonu yazılım güncelleştirme yapılandırmasını alır |
> | Eylem | Microsoft.Automation/automationAccounts/softwareUpdateConfigurations/write | Oluşturur veya Azure Automation yazılım güncelleştirme yapılandırmasını güncelleştirir |
> | Eylem | Microsoft.Automation/automationAccounts/statistics/read | Azure Otomasyonu istatistiklerini alır |
> | Eylem | Microsoft.Automation/automationAccounts/updateDeploymentMachineRuns/read | Bir Azure Otomasyonu güncelleştirme dağıtım makine Al |
> | Eylem | Microsoft.Automation/automationAccounts/updateManagementPatchJob/read | Bir Azure Otomasyonu güncelleştirme yönetimi düzeltme eki işini alır |
> | Eylem | Microsoft.Automation/automationAccounts/usages/read | Azure Otomasyonu kullanım alır |
> | Eylem | Microsoft.Automation/automationAccounts/variables/delete | Bir Azure Otomasyonu değişken varlığını siler |
> | Eylem | Microsoft.Automation/automationAccounts/variables/read | Bir Azure Otomasyonu değişken varlığını okur |
> | Eylem | Microsoft.Automation/automationAccounts/variables/write | Bir Azure Otomasyonu değişken varlığı oluşturur veya güncelleştirir |
> | Eylem | Microsoft.Automation/automationAccounts/watchers/delete | Bir Azure Otomasyonu İzleyici işi Sil |
> | Eylem | Microsoft.Automation/automationAccounts/watchers/read | Bir Azure Otomasyonu İzleyici işini alır |
> | Eylem | Microsoft.Automation/automationAccounts/watchers/start/action | Bir Azure Otomasyonu İzleyici işi Başlat |
> | Eylem | Microsoft.Automation/automationAccounts/watchers/stop/action | Bir Azure Otomasyonu İzleyici işini durdurma |
> | Eylem | Microsoft.Automation/automationAccounts/watchers/streams/read | Bir Azure Otomasyonu İzleyicisi iş akışını alır |
> | Eylem | Microsoft.Automation/automationAccounts/watchers/watcherActions/delete | Bir Azure Otomasyonu İzleyici işi eylemlerini silme |
> | Eylem | Microsoft.Automation/automationAccounts/watchers/watcherActions/read | Bir Azure Otomasyonu izleyicisini proje eylemlerini alır |
> | Eylem | Microsoft.Automation/automationAccounts/watchers/watcherActions/write | Bir Azure Otomasyonu İzleyici işi eylemleri oluşturma |
> | Eylem | Microsoft.Automation/automationAccounts/watchers/write | Bir Azure Otomasyonu İzleyici işi oluşturur |
> | Eylem | Microsoft.Automation/automationAccounts/webhooks/action | Bir Azure Otomasyonu Web kancası için URI oluşturur |
> | Eylem | Microsoft.Automation/automationAccounts/webhooks/delete | Bir Azure Otomasyonu Web kancasını siler  |
> | Eylem | Microsoft.Automation/automationAccounts/webhooks/read | Bir Azure Otomasyonu Web kancasını okur |
> | Eylem | Microsoft.Automation/automationAccounts/webhooks/write | Oluşturur veya bir Azure Otomasyonu Web kancası güncelleştirir |
> | Eylem | Microsoft.Automation/automationAccounts/write | Bir Azure Otomasyonu hesabı oluşturur veya güncelleştirir |
> | Eylem | Microsoft.Automation/operations/read | Azure Automation kaynaklarınız için kullanılabilir işlemleri alır |
> | Eylem | Microsoft.Automation/register/action | Azure otomasyonu için aboneliği kaydeder |

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.AzureActiveDirectory/b2cDirectories/delete | B2C Dizini kaynağını sil |
> | Eylem | Microsoft.AzureActiveDirectory/b2cDirectories/read | B2C Dizini kaynağını görüntüle |
> | Eylem | Microsoft.AzureActiveDirectory/b2cDirectories/write | B2C Dizini kaynağı oluştur veya güncelleştir |
> | Eylem | Microsoft.AzureActiveDirectory/operations/read | Microsoft.AzureActiveDirectory kaynak sağlayıcısı için tüm kullanılabilir API işlemlerini okuyun |
> | Eylem | Microsoft.AzureActiveDirectory/register/action | Microsoft.AzureActiveDirectory kaynak sağlayıcısı için aboneliği kaydedin |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.AzureStack/Operations/read | Bir kaynak sağlayıcısı işlemi özelliklerini alır |
> | Eylem | Microsoft.AzureStack/register/action | Abonelik Microsoft.AzureStack kaynak sağlayıcısına kaydeder |
> | Eylem | Microsoft.AzureStack/registrations/customerSubscriptions/delete | Bir Azure yığın müşteri aboneliğe siler |
> | Eylem | Microsoft.AzureStack/registrations/customerSubscriptions/read | Bir Azure yığın müşteri aboneliğin özelliklerini alır |
> | Eylem | Microsoft.AzureStack/registrations/customerSubscriptions/write | Oluşturur veya bir Azure yığın müşteri aboneliğe güncelleştirir |
> | Eylem | Microsoft.AzureStack/registrations/delete | Bir Azure yığın kaydını siler |
> | Eylem | Microsoft.AzureStack/registrations/getActivationKey/action | En son Azure yığın etkinleştirme anahtarı alır |
> | Eylem | Microsoft.AzureStack/registrations/products/listDetails/action | Bir Azure yığın Market ürün ayrıntılarını genişletilmiş alır |
> | Eylem | Microsoft.AzureStack/registrations/products/read | Bir Azure yığın Market ürün özelliklerini alır |
> | Eylem | Microsoft.AzureStack/registrations/read | Bir Azure yığın kaydı özelliklerini alır |
> | Eylem | Microsoft.AzureStack/registrations/write | Oluşturur veya bir Azure yığın kaydı güncelleştirir |

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Batch/batchAccounts/applications/delete | Bir uygulamayı siler |
> | Eylem | Microsoft.Batch/batchAccounts/applications/read | Uygulamaları listeler veya bir uygulamanın özelliklerini alır |
> | Eylem | Microsoft.Batch/batchAccounts/applications/versions/activate/action | Bir uygulama paketi etkinleştirir |
> | Eylem | Microsoft.Batch/batchAccounts/applications/versions/delete | Bir uygulama paketini siler |
> | Eylem | Microsoft.Batch/batchAccounts/applications/versions/read | Bir uygulama paketi özelliklerini alır |
> | Eylem | Microsoft.Batch/batchAccounts/applications/versions/write | Yeni bir uygulama paketi oluşturur veya var olan uygulama paketini güncelleştirir |
> | Eylem | Microsoft.Batch/batchAccounts/applications/write | Yeni bir uygulama oluşturur veya varolan bir uygulamayı güncelleştirir |
> | Eylem | Microsoft.Batch/batchAccounts/certificateOperationResults/read | Bir Batch hesabında uzun süre çalışan bir işlem sonuçlarını alır |
> | Eylem | Microsoft.Batch/batchAccounts/certificates/cancelDelete/action | Silme işlemi başarısız bir Batch hesabında bir sertifika iptal eder |
> | Eylem | Microsoft.Batch/batchAccounts/certificates/delete | Sertifika toplu işlem hesabından siler |
> | Eylem | Microsoft.Batch/batchAccounts/certificates/read | Bir Batch hesabında sertifikaları listeleyen veya bir sertifikanın özelliklerini alır |
> | Eylem | Microsoft.Batch/batchAccounts/certificates/write | Bir Batch hesabında yeni bir sertifika oluşturur veya varolan bir sertifikayı güncelleştirir |
> | Eylem | Microsoft.Batch/batchAccounts/delete | Batch hesabını siler |
> | Eylem | Microsoft.Batch/batchAccounts/listkeys/action | Bir toplu işlem hesabı için anahtarlar erişim listeleri |
> | Eylem | Microsoft.Batch/batchAccounts/operationResults/read | Uzun süren bir Batch hesabı İşlem sonuçlarını alır |
> | Eylem | Microsoft.Batch/batchAccounts/poolOperationResults/read | Bir Batch hesabında uzun süren bir havuz İşlem sonuçlarını alır |
> | Eylem | Microsoft.Batch/batchAccounts/pools/delete | Bir toplu işlem hesabından bir havuzu siler |
> | Eylem | Microsoft.Batch/batchAccounts/pools/disableAutoscale/action | Bir toplu işlem hesabı havuzu için otomatik ölçeklendirme devre dışı bırakır |
> | Eylem | Microsoft.Batch/batchAccounts/pools/read | Bir toplu işlem hesabı havuzlarını listeler veya bir havuz özelliklerini alır |
> | Eylem | Microsoft.Batch/batchAccounts/pools/stopResize/action | Bir Batch hesabı havuzu işlemi durdurur devam eden bir yeniden boyutlandırma |
> | Eylem | Microsoft.Batch/batchAccounts/pools/upgradeOs/action | Bir toplu işlem hesabı havuzunun işletim sistemi yükseltmeleri |
> | Eylem | Microsoft.Batch/batchAccounts/pools/write | Bir Batch hesabında yeni bir havuz oluşturur veya mevcut bir havuzu güncelleştirir |
> | Eylem | Microsoft.Batch/batchAccounts/providers/Microsoft.Insights/diagnosticSettings/read | Kaynak için tanılama ayarını alır |
> | Eylem | Microsoft.Batch/batchAccounts/providers/Microsoft.Insights/diagnosticSettings/write | Kaynak için tanılama ayarını oluşturur veya güncelleştirir |
> | Eylem | Microsoft.Batch/batchAccounts/providers/Microsoft.Insights/logDefinitions/read | Batch hizmeti için kullanılabilir günlüklerini alır |
> | Eylem | Microsoft.Batch/batchAccounts/providers/Microsoft.Insights/metricDefinitions/read | Batch hizmeti için kullanılabilir ölçümleri alır |
> | Eylem | Microsoft.Batch/batchAccounts/read | Bir toplu işlem hesabının özelliklerini alır veya toplu işlem hesaplarını listeler |
> | Eylem | Microsoft.Batch/batchAccounts/regeneratekeys/action | Erişim anahtarları bir toplu işlem hesabı için yeniden oluşturur |
> | Eylem | Microsoft.Batch/batchAccounts/syncAutoStorageKeys/action | Toplu işlem hesabı için yapılandırılmış otomatik depolama hesabının erişim anahtarlarını eşitler |
> | Eylem | Microsoft.Batch/batchAccounts/write | Yeni bir Batch hesabı oluşturur veya var olan bir toplu işlem hesabını güncelleştirir |
> | Eylem | Microsoft.Batch/locations/checkNameAvailability/action | Hesap adı geçerli ve kullanımda olup olmadığını denetler. |
> | Eylem | Microsoft.Batch/locations/quotas/read | Belirtilen Azure bölge belirtilen abonelik toplu kotaları alır |
> | Eylem | Microsoft.Batch/register/action | Batch kaynak sağlayıcısı için aboneliği kaydeder ve Batch hesaplarının oluşturulmasını etkinleştirir |
> | Eylem | Microsoft.Batch/unregister/action | Abonelik toplu işlem hesaplarını oluşturulmasını önleme toplu kaynak sağlayıcısı için kaydını siler |

## <a name="microsoftbatchai"></a>Microsoft.BatchAI

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.BatchAI/clusters/read | Toplu AI kümeleri listeler veya toplu AI küme özelliklerini alır |
> | Eylem | Microsoft.BatchAI/fileservers/read | Toplu AI fileservers listeler veya toplu AI DosyaSunucusu özelliklerini alır |
> | Eylem | Microsoft.BatchAI/locations/operationresults/read | Belirtilen Azure bölge toplu AI zaman uyumsuz işlem sonucunu alır |
> | Eylem | Microsoft.BatchAI/locations/operationstatuses/read | Belirtilen Azure bölge toplu AI zaman uyumsuz işlem durumunu alır |
> | Eylem | Microsoft.BatchAI/locations/usages/read | Belirtilen abonelik toplu AI kullanımları sırasında belirtilen Azure bölgesini alır |
> | Eylem | Microsoft.BatchAI/register/action | Toplu AI kaynak sağlayıcısı için aboneliği kaydeder ve toplu AI kaynakların oluşturulmasını sağlar |
> | Eylem | Microsoft.BatchAI/unregister/action | Abonelik toplu AI kaynakların oluşturulmasını önleme toplu AI kaynak sağlayıcısı için kaydını siler |
> | Eylem | Microsoft.BatchAI/workspaces/clusters/delete | Bir toplu AI küme siler |
> | Eylem | Microsoft.BatchAI/workspaces/clusters/read | Toplu AI kümeleri listeler veya toplu AI küme özelliklerini alır |
> | Eylem | Microsoft.BatchAI/workspaces/clusters/remoteLoginInformation/action | Bir toplu AI küme uzaktan oturum açma bilgilerini listeler |
> | Eylem | Microsoft.BatchAI/workspaces/clusters/write | Yeni bir toplu AI kümesi oluşturur veya var olan toplu AI kümeyi güncelleştirir |
> | Eylem | Microsoft.BatchAI/workspaces/delete | Bir toplu AI Çalışma siler |
> | Eylem | Microsoft.BatchAI/workspaces/experiments/delete | Bir toplu AI deneme siler |
> | Eylem | Microsoft.BatchAI/workspaces/experiments/jobs/delete | Bir toplu AI işi siler |
> | Eylem | Microsoft.BatchAI/workspaces/experiments/jobs/listoutputfiles/action | Çıkış dosyaları toplu AI işi için listeler |
> | Eylem | Microsoft.BatchAI/workspaces/experiments/jobs/read | Bir toplu AI iş özelliklerini alır veya toplu AI işlerini listeler |
> | Eylem | Microsoft.BatchAI/workspaces/experiments/jobs/remoteLoginInformation/action | Bir toplu AI işi uzaktan oturum açma bilgilerini listeler |
> | Eylem | Microsoft.BatchAI/workspaces/experiments/jobs/terminate/action | Bir toplu AI işi sonlandırır |
> | Eylem | Microsoft.BatchAI/workspaces/experiments/jobs/write | Yeni bir toplu AI işi oluşturur veya mevcut bir toplu AI proje güncelleştirir |
> | Eylem | Microsoft.BatchAI/workspaces/experiments/read | Listeleri toplu AI denemelerini veya toplu AI deneme özelliklerini alır |
> | Eylem | Microsoft.BatchAI/workspaces/experiments/write | Yeni bir toplu AI deneme oluşturur veya mevcut bir toplu AI deneme güncelleştirir |
> | Eylem | Microsoft.BatchAI/workspaces/fileservers/delete | Bir toplu AI DosyaSunucusu siler |
> | Eylem | Microsoft.BatchAI/workspaces/fileservers/read | Toplu AI fileservers listeler veya toplu AI DosyaSunucusu özelliklerini alır |
> | Eylem | Microsoft.BatchAI/workspaces/fileservers/write | Yeni bir toplu AI DosyaSunucusu oluşturur veya mevcut bir toplu AI DosyaSunucusu güncelleştirir |
> | Eylem | Microsoft.BatchAI/workspaces/read | Toplu AI çalışma alanları listeler veya toplu AI çalışma özelliklerini alır |
> | Eylem | Microsoft.BatchAI/workspaces/write | Yeni bir toplu AI Çalışma alanı oluşturur veya var olan bir toplu AI Çalışma güncelleştirir |

## <a name="microsoftbilling"></a>Microsoft.Billing

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Billing/billingPeriods/read | Kullanılabilir fatura aralıklarını listeler |
> | Eylem | Microsoft.Billing/invoices/read | Listeleri kullanılabilir faturalar |

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.BingMaps/mapApis/Delete | İşlemi Siler |
> | Eylem | Microsoft.BingMaps/mapApis/listSecrets/action | Parolaları Listeler |
> | Eylem | Microsoft.BingMaps/mapApis/listSingleSignOnToken/action | Kaynak İçin Çoklu Oturum Açma Yetkilendirme Belirtecini Okuyun |
> | Eylem | Microsoft.BingMaps/mapApis/Read | İşlemi Okur |
> | Eylem | Microsoft.BingMaps/mapApis/regenerateKey/action | Anahtarı Yeniden Oluşturur |
> | Eylem | Microsoft.BingMaps/mapApis/Write | İşlemi Yazar |
> | Eylem | Microsoft.BingMaps/Operations/read | İşlem açıklaması. |

## <a name="microsoftcache"></a>Microsoft.Cache

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Cache/checknameavailability/action | Bir adın yeni bir Redis Cache için kullanılabilir olup olmadığını denetler |
> | Eylem | Microsoft.Cache/locations/operationresults/read | 'Konum' üst bilgisi önceden istemciye döndürülen uzun süredir çalışan işlemin sonucunu alır |
> | Eylem | Microsoft.Cache/operations/read | 'Microsoft.Cache' sağlayıcısının desteklediği işlemleri listeler. |
> | Eylem | Microsoft.Cache/redis/delete | Redis Cache'nin tamamını sil |
> | Eylem | Microsoft.Cache/redis/export/action | Redis verilerini belirtilen biçimde ön ekli depolama blob'larına aktar |
> | Eylem | Microsoft.Cache/redis/firewallRules/delete | Bir Redis Cache'in IP güvenlik duvarı kurallarını siler |
> | Eylem | Microsoft.Cache/redis/firewallRules/read | Bir Redis Cache'in IP güvenlik duvarı kurallarını alır |
> | Eylem | Microsoft.Cache/redis/firewallRules/write | Bir Redis Cache'in IP güvenlik duvarı kurallarını düzenler |
> | Eylem | Microsoft.Cache/redis/forceReboot/action | Veri kaybı olasılığı olan bir önbellek örneği yeniden başlatmayı zorlayın. |
> | Eylem | Microsoft.Cache/redis/import/action | Birden çok blob'dan belirli bir biçimdeki verileri Redis'e aktar |
> | Eylem | Microsoft.Cache/redis/linkedservers/delete | Redis Cache'ten Bağlı Sunucuyu Sil |
> | Eylem | Microsoft.Cache/redis/linkedservers/read | Bir Redis Cache ile ilişkili Bağlı Sunucuları alın. |
> | Eylem | Microsoft.Cache/redis/linkedservers/write | Redis Cache'e Bağlı Sunucu Ekle |
> | Eylem | Microsoft.Cache/redis/listKeys/action | Redis Cache erişim anahtarlarının değerini yönetim portalında görüntüleyin |
> | Eylem | Microsoft.Cache/redis/listUpgradeNotifications/read | Önbellek kiracısı için en son Yükseltme Bildirimlerini listeleyin. |
> | Eylem | Microsoft.Cache/redis/metricDefinitions/read | Bir Redis Cache için kullanılabilir ölçümleri alır |
> | Eylem | Microsoft.Cache/redis/patchSchedules/delete | Bir Redis Cache'in düzeltme eki zamanlamasını siler |
> | Eylem | Microsoft.Cache/redis/patchSchedules/read | Redis Cache'in düzeltme eki uygulama zamanlamasını alır |
> | Eylem | Microsoft.Cache/redis/patchSchedules/write | Redis Cache'in düzeltme eki uygulama zamanlamasını değiştirir |
> | Eylem | Microsoft.Cache/redis/read | Redis Cache'nin ayarlarını ve yapılandırmasını yönetim portalında görüntüleyin |
> | Eylem | Microsoft.Cache/redis/regenerateKey/action | Redis Cache erişim anahtarlarının değerini yönetim portalında değiştirin |
> | Eylem | Microsoft.Cache/redis/start/action | Bir önbellek örneği başlatın. |
> | Eylem | Microsoft.Cache/redis/stop/action | Bir önbellek örneğini durdurun. |
> | Eylem | Microsoft.Cache/redis/write | Redis Cache'in ayarlarını ve yapılandırmasını yönetim portalında değiştirin |
> | Eylem | Microsoft.Cache/register/action | 'Microsoft.Cache' kaynak sağlayıcısını bir aboneliğe kaydeder |
> | Eylem | Microsoft.Cache/unregister/action | 'Microsoft.Cache' kaynak sağlayıcısının kaydını bir abonelikten kaldırır |

## <a name="microsoftcapacity"></a>Microsoft.Capacity

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Capacity/register/action | Kapasite kaynağı sağlayıcısını kaydeder ve kapasite kaynakların oluşturulmasını sağlar. |
> | Eylem | Microsoft.Capacity/reservationorders/action | Tüm Rezervasyonu Güncelleştir |
> | Eylem | Microsoft.Capacity/reservationorders/delete | Tüm ayırmayı silmek |
> | Eylem | Microsoft.Capacity/reservationorders/read | Tüm ayırmaları okuma |
> | Eylem | Microsoft.Capacity/reservationorders/reservations/action | Tüm Rezervasyonu Güncelleştir |
> | Eylem | Microsoft.Capacity/reservationorders/reservations/delete | Tüm ayırmayı silmek |
> | Eylem | Microsoft.Capacity/reservationorders/reservations/read | Tüm ayırmaları okuma |
> | Eylem | Microsoft.Capacity/reservationorders/reservations/revisions/read | Tüm ayırmaları okuma |
> | Eylem | Microsoft.Capacity/reservationorders/reservations/write | Tüm ayırma oluştur |
> | Eylem | Microsoft.Capacity/reservationorders/write | Tüm ayırma oluştur |

## <a name="microsoftcdn"></a>Microsoft.Cdn

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Cdn/CheckNameAvailability/action |  |
> | Eylem | Microsoft.Cdn/CheckResourceUsage/action |  |
> | Eylem | Microsoft.Cdn/edgenodes/delete |  |
> | Eylem | Microsoft.Cdn/edgenodes/read |  |
> | Eylem | Microsoft.Cdn/edgenodes/write |  |
> | Eylem | Microsoft.Cdn/operationresults/delete |  |
> | Eylem | Microsoft.Cdn/operationresults/profileresults/CheckResourceUsage/action |  |
> | Eylem | Microsoft.Cdn/operationresults/profileresults/delete |  |
> | Eylem | Microsoft.Cdn/operationresults/profileresults/endpointresults/CheckResourceUsage/action |  |
> | Eylem | Microsoft.Cdn/operationresults/profileresults/endpointresults/customdomainresults/delete |  |
> | Eylem | Microsoft.Cdn/operationresults/profileresults/endpointresults/customdomainresults/DisableCustomHttps/action |  |
> | Eylem | Microsoft.Cdn/operationresults/profileresults/endpointresults/customdomainresults/EnableCustomHttps/action |  |
> | Eylem | Microsoft.Cdn/operationresults/profileresults/endpointresults/customdomainresults/read |  |
> | Eylem | Microsoft.Cdn/operationresults/profileresults/endpointresults/customdomainresults/write |  |
> | Eylem | Microsoft.Cdn/operationresults/profileresults/endpointresults/delete |  |
> | Eylem | Microsoft.Cdn/operationresults/profileresults/endpointresults/Load/action |  |
> | Eylem | Microsoft.Cdn/operationresults/profileresults/endpointresults/originresults/delete |  |
> | Eylem | Microsoft.Cdn/operationresults/profileresults/endpointresults/originresults/read |  |
> | Eylem | Microsoft.Cdn/operationresults/profileresults/endpointresults/originresults/write |  |
> | Eylem | Microsoft.Cdn/operationresults/profileresults/endpointresults/Purge/action |  |
> | Eylem | Microsoft.Cdn/operationresults/profileresults/endpointresults/read |  |
> | Eylem | Microsoft.Cdn/operationresults/profileresults/endpointresults/Start/action |  |
> | Eylem | Microsoft.Cdn/operationresults/profileresults/endpointresults/Stop/action |  |
> | Eylem | Microsoft.Cdn/operationresults/profileresults/endpointresults/ValidateCustomDomain/action |  |
> | Eylem | Microsoft.Cdn/operationresults/profileresults/endpointresults/write |  |
> | Eylem | Microsoft.Cdn/operationresults/profileresults/GenerateSsoUri/action |  |
> | Eylem | Microsoft.Cdn/operationresults/profileresults/GetSupportedOptimizationTypes/action |  |
> | Eylem | Microsoft.Cdn/operationresults/profileresults/read |  |
> | Eylem | Microsoft.Cdn/operationresults/profileresults/write |  |
> | Eylem | Microsoft.Cdn/operationresults/read |  |
> | Eylem | Microsoft.Cdn/operationresults/write |  |
> | Eylem | Microsoft.Cdn/operations/read |  |
> | Eylem | Microsoft.Cdn/profiles/CheckResourceUsage/action |  |
> | Eylem | Microsoft.Cdn/profiles/delete |  |
> | Eylem | Microsoft.Cdn/profiles/endpoints/CheckResourceUsage/action |  |
> | Eylem | Microsoft.Cdn/profiles/endpoints/customdomains/delete |  |
> | Eylem | Microsoft.Cdn/profiles/endpoints/customdomains/DisableCustomHttps/action |  |
> | Eylem | Microsoft.Cdn/profiles/endpoints/customdomains/EnableCustomHttps/action |  |
> | Eylem | Microsoft.Cdn/profiles/endpoints/customdomains/read |  |
> | Eylem | Microsoft.Cdn/profiles/endpoints/customdomains/write |  |
> | Eylem | Microsoft.Cdn/profiles/endpoints/delete |  |
> | Eylem | Microsoft.Cdn/profiles/endpoints/Load/action |  |
> | Eylem | Microsoft.Cdn/profiles/endpoints/origins/delete |  |
> | Eylem | Microsoft.Cdn/profiles/endpoints/origins/read |  |
> | Eylem | Microsoft.Cdn/profiles/endpoints/origins/write |  |
> | Eylem | Microsoft.Cdn/profiles/endpoints/providers/Microsoft.Insights/diagnosticSettings/read | Kaynağın tanılama ayarlarını alır |
> | Eylem | Microsoft.Cdn/profiles/endpoints/providers/Microsoft.Insights/diagnosticSettings/write | Oluşturur veya kaynağın tanılama ayarlarını güncelleştirir |
> | Eylem | Microsoft.Cdn/profiles/endpoints/providers/Microsoft.Insights/logDefinitions/read | Microsoft.Cdn için kullanılabilir günlüklerini alır |
> | Eylem | Microsoft.Cdn/profiles/endpoints/Purge/action |  |
> | Eylem | Microsoft.Cdn/profiles/endpoints/read |  |
> | Eylem | Microsoft.Cdn/profiles/endpoints/Start/action |  |
> | Eylem | Microsoft.Cdn/profiles/endpoints/Stop/action |  |
> | Eylem | Microsoft.Cdn/profiles/endpoints/ValidateCustomDomain/action |  |
> | Eylem | Microsoft.Cdn/profiles/endpoints/write |  |
> | Eylem | Microsoft.Cdn/profiles/GenerateSsoUri/action |  |
> | Eylem | Microsoft.Cdn/profiles/GetSupportedOptimizationTypes/action |  |
> | Eylem | Microsoft.Cdn/profiles/read |  |
> | Eylem | Microsoft.Cdn/profiles/write |  |
> | Eylem | Microsoft.Cdn/register/action | CDN kaynak sağlayıcısı için aboneliği kaydeder ve CDN profillerinin oluşturulmasını sağlar. |
> | Eylem | Microsoft.Cdn/ValidateProbe/action |  |

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.CertificateRegistration/certificateOrders/certificates/Delete | Varolan bir sertifikayı Sil |
> | Eylem | Microsoft.CertificateRegistration/certificateOrders/certificates/Read | Sertifikaların listesini al |
> | Eylem | Microsoft.CertificateRegistration/certificateOrders/certificates/Write | Yeni bir sertifika ekleyin veya mevcut bir güncelleştirme |
> | Eylem | Microsoft.CertificateRegistration/certificateOrders/Delete | Varolan bir AppServiceCertificate Sil |
> | Eylem | Microsoft.CertificateRegistration/certificateOrders/Read | CertificateOrders listesini al |
> | Eylem | Microsoft.CertificateRegistration/certificateOrders/reissue/Action | Varolan bir certificateorder yeniden gönderin |
> | Eylem | Microsoft.CertificateRegistration/certificateOrders/renew/Action | Varolan bir certificateorder yenileme |
> | Eylem | Microsoft.CertificateRegistration/certificateOrders/resendEmail/Action | Yeniden Gönder sertifika e-posta |
> | Eylem | Microsoft.CertificateRegistration/certificateOrders/resendRequestEmails/Action | Başka bir e-posta adresine e-postalar isteği yeniden gönderin |
> | Eylem | Microsoft.CertificateRegistration/certificateOrders/resendRequestEmails/Action | Site korumalı için verilen bir uygulama hizmet sertifikası alma |
> | Eylem | Microsoft.CertificateRegistration/certificateOrders/retrieveCertificateActions/Action | Sertifika eylemlerin listesini alma |
> | Eylem | Microsoft.CertificateRegistration/certificateOrders/retrieveEmailHistory/Action | Sertifika e-posta geçmişini alma |
> | Eylem | Microsoft.CertificateRegistration/certificateOrders/verifyDomainOwnership/Action | Etki alanı sahipliğini doğrulama |
> | Eylem | Microsoft.CertificateRegistration/certificateOrders/Write | Yeni bir certificateOrder eklemek veya mevcut bir güncelleştirme |
> | Eylem | Microsoft.CertificateRegistration/operations/Read | Uygulama hizmeti sertifika kaydı tüm işlemler listesi |
> | Eylem | Microsoft.CertificateRegistration/provisionGlobalAppServicePrincipalInUserTenant/Action | Hizmet uygulaması sorumlusu için hizmet asıl sağlama |
> | Eylem | Microsoft.CertificateRegistration/register/action | Aboneliği için Microsoft Certificates kaynak sağlayıcısı kaydetme |
> | Eylem | Microsoft.CertificateRegistration/validateCertificateRegistrationInformation/Action | Sertifika satın alma nesnesi gönderme olmadan doğrula |

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.ClassicCompute/capabilities/read | Özellikleri gösterir |
> | Eylem | Microsoft.ClassicCompute/checkDomainNameAvailability/action | Bir etki alanı adının kullanılabilirliğini denetler. |
> | Eylem | Microsoft.ClassicCompute/checkDomainNameAvailability/read | Bir etki alanı adının kullanılabilirliğini alır. |
> | Eylem | Microsoft.ClassicCompute/domainNames/active/write | Etkin etki alanı adını ayarlar. |
> | Eylem | Microsoft.ClassicCompute/domainNames/availabilitySets/read | Kaynak için kullanılabilirlik kümesini görüntüleyin. |
> | Eylem | Microsoft.ClassicCompute/domainNames/capabilities/read | Etki alanı adı özelliklerini gösterir |
> | Eylem | Microsoft.ClassicCompute/domainNames/delete | Kaynaklar için etki alanı adlarını kaldırın. |
> | Eylem | Microsoft.ClassicCompute/domainNames/deploymentslots/read | Dağıtım yuvalarını gösterir. |
> | Eylem | Microsoft.ClassicCompute/domainNames/deploymentslots/state/read | Dağıtım yuvası durumu alır. |
> | Eylem | Microsoft.ClassicCompute/domainNames/deploymentslots/state/write | Dağıtım yuvası durumunu ekleyin. |
> | Eylem | Microsoft.ClassicCompute/domainNames/deploymentslots/write | Dağıtımı oluşturur veya güncelleştirir. |
> | Eylem | Microsoft.ClassicCompute/domainNames/extensions/delete | Etki alanı adı uzantılarını kaldırın. |
> | Eylem | Microsoft.ClassicCompute/domainNames/extensions/operationStatuses/read | Etki alanı uzantıları için işlem durumunu okur. |
> | Eylem | Microsoft.ClassicCompute/domainNames/extensions/read | Etki alanı adı uzantılarını döndürür. |
> | Eylem | Microsoft.ClassicCompute/domainNames/extensions/write | Etki alanı adı uzantıları ekleyin. |
> | Eylem | Microsoft.ClassicCompute/domainNames/internalLoadBalancers/delete | Yeni bir iç yük dengelemeyi kaldırın. |
> | Eylem | Microsoft.ClassicCompute/domainNames/internalLoadBalancers/operationStatuses/read | Etki alanı adları iç load balancer'ları için işlem durumunu okur. |
> | Eylem | Microsoft.ClassicCompute/domainNames/internalLoadBalancers/read | İç load balancer'ları alır. |
> | Eylem | Microsoft.ClassicCompute/domainNames/internalLoadBalancers/write | Yeni bir iç yük dengeleme oluşturur. |
> | Eylem | Microsoft.ClassicCompute/domainNames/loadBalancedEndpointSets/operationStatuses/read | Etki alanı adları yük dengeli uç nokta kümeleri için işlem durumunu okur. |
> | Eylem | Microsoft.ClassicCompute/domainNames/loadBalancedEndpointSets/read | Yük dengeli uç nokta kümelerini alır. |
> | Eylem | Microsoft.ClassicCompute/domainNames/loadBalancedEndpointSets/write | Yük dengeli uç nokta kümesini ekleyin. |
> | Eylem | Microsoft.ClassicCompute/domainNames/operationstatuses/read | Etki alanı adının işlem durumunu alır. |
> | Eylem | Microsoft.ClassicCompute/domainNames/operationStatuses/read | Etki alanı uzantıları için işlem durumunu okur. |
> | Eylem | Microsoft.ClassicCompute/domainNames/read | Kaynaklar için etki alanı adlarını döndürün. |
> | Eylem | Microsoft.ClassicCompute/domainNames/serviceCertificates/delete | Kullanılan hizmet sertifikalarını silin. |
> | Eylem | Microsoft.ClassicCompute/domainNames/serviceCertificates/operationStatuses/read | Etki alanı adları hizmet sertifikaları için işlem durumunu okur. |
> | Eylem | Microsoft.ClassicCompute/domainNames/serviceCertificates/read | Kullanılan hizmet sertifikalarını döndürür. |
> | Eylem | Microsoft.ClassicCompute/domainNames/serviceCertificates/write | Kullanılan hizmet sertifikalarını ekleyin veya değiştirin. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/abortMigration/action | Bir dağıtım yuvasının geçişini durdurur. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/commitMigration/action | Bir dağıtım yuvasının geçişini yürütür. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/delete | Bir dağıtım yuvasını siler. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/operationStatuses/read | Etki alanı adı yuvaları için işlem durumunu okur. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/prepareMigration/action | Bir dağıtım yuvasının geçişini hazırlar. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/read | Dağıtım yuvalarını gösterir. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/roles/extensionReferences/delete | Dağıtım yuvası rolüne ait uzantı başvurusunu kaldırın. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/roles/extensionReferences/operationStatuses/read | Etki alanı adı yuvaları uzantı başvuruları için işlem durumunu okur. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/roles/extensionReferences/read | Dağıtım yuvası rolüne ait uzantı başvurusunu döndürür. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/roles/extensionReferences/write | Dağıtım yuvası rolüne ait uzantı başvurusunu ekleyin veya değiştirin. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/roles/metricdefinitions/read | Etki alanı adı için rol ölçümü tanımını alır. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/roles/metrics/read | Etki alanı adı için rol ölçümünü alır. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/roles/operationstatuses/read | Etki alanı adları yuva rolü için işlem durumunu alır. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/roles/providers/Microsoft.Insights/diagnosticSettings/read | Tanılama ayarlarını alın. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/roles/providers/Microsoft.Insights/diagnosticSettings/write | Tanılama ayarlarını ekleyin veya değiştirin. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/roles/providers/Microsoft.Insights/metricDefinitions/read | Ölçüm tanımlarını alır. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/roles/read | Dağıtım yuvası için rolü alın. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/downloadremotedesktopconnectionfile/action | Etki alanı adı yuvası rolü üzerindeki rol örneği için uzak masaüstü bağlantısı dosyasını indirir. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/operationStatuses/read | Etki alanı adları yuva rolü üzerindeki rol örneği için işlem durumunu alır. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/read | Rol örneğini alın. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/rebuild/action | Rol örneğini yeniden derler. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/reimage/action | Rol örneğinin yeniden görüntüsünü oluşturur. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/restart/action | Rol örneklerini yeniden başlatır. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/roles/skus/read | Dağıtım yuvası için rol SKU'sunu alır. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/roles/write | Dağıtım yuvası için rol ekleyin. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/start/action | Bir dağıtım yuvası başlatır. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/state/start/write | Dağıtım yuvası durumunu durduruldu olarak değiştirir. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/state/stop/write | Dağıtım yuvası durumunu başlatıldı olarak değiştirir. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/stop/action | Dağıtım yuvasını askıya alır. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/upgradeDomain/write | Etki alanını yükseltir. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/validateMigration/action | Bir dağıtım yuvasının geçişini doğrular. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/write | Dağıtımı oluşturur veya güncelleştirir. |
> | Eylem | Microsoft.ClassicCompute/domainNames/swap/action | Hazırlama yuvasını üretim yuvasıyla değiştirir. |
> | Eylem | Microsoft.ClassicCompute/domainNames/write | Kaynaklar için etki alanı adlarını ekleyin veya değiştirin. |
> | Eylem | Microsoft.ClassicCompute/moveSubscriptionResources/action | Tüm klasik kaynakları farklı bir aboneliğe taşıyın. |
> | Eylem | Microsoft.ClassicCompute/operatingSystemFamilies/read | Microsoft Azure'da kullanılabilen konuk işletim sistemi ailelerini ve her aile için kullanılabilen işletim sistemi sürümlerini listeler. |
> | Eylem | Microsoft.ClassicCompute/operatingSystems/read | Microsoft Azure'da kullanılabilen geçerli konuk işletim sistemi sürümlerini listeler. |
> | Eylem | Microsoft.ClassicCompute/operations/read | İşlem listesini alır. |
> | Eylem | Microsoft.ClassicCompute/operationStatuses/read | Kaynağın işlem durumunu okur. |
> | Eylem | Microsoft.ClassicCompute/quotas/read | Abonelik için kotayı alın. |
> | Eylem | Microsoft.ClassicCompute/register/action | Classic Compute'a Kaydol |
> | Eylem | Microsoft.ClassicCompute/resourceTypes/skus/read | Desteklenen kaynak türleri için Sku listesini alır. |
> | Eylem | Microsoft.ClassicCompute/validateSubscriptionMoveAvailability/action | Klasik taşıma işlemi için aboneliğin uygunluk durumunu doğrulayın. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/associatedNetworkSecurityGroups/delete | Sanal makineyle ilişkilendirilmiş ağ güvenlik grubunu siler. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/associatedNetworkSecurityGroups/operationStatuses/read | Ağ güvenlik gruplarıyla ilişkili sanal makineler için işlem durumunu okur. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/associatedNetworkSecurityGroups/read | Sanal makineyle ilişkilendirilmiş ağ güvenlik grubunu alır. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/associatedNetworkSecurityGroups/write | Sanal makineyle ilişkilendirilmiş bir ağ güvenlik grubu ekler. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/asyncOperations/read | Olası zaman uyumsuz işlemleri alır |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/attachDisk/action | Sanal makineye bir veri diski ekler. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/capture/action | Bir sanal makineyi yakalar. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/delete | Sanal makineleri kaldırır. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/detachDisk/action | Bir veri diskini sanal makineden ayırır. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/diagnosticsettings/read | Sanal makine tanılama ayarlarını alır. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/disks/read | Veri disklerinin listesini alır |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/downloadRemoteDesktopConnectionFile/action | Sanal makine için RDP dosyasını indirir. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/extensions/operationStatuses/read | Sanal makine uzantıları için işlem durumunu okur. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/extensions/read | Sanal makine uzantısını alır. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/extensions/write | Sanal makine uzantısını yerleştirir. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/metricdefinitions/read | Sanal makine ölçüm tanımını alır. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/metrics/read | Ölçümleri alır. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/delete | Ağ arabirimiyle ilişkilendirilmiş ağ güvenlik grubunu siler. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/operationStatuses/read | Ağ güvenlik gruplarıyla ilişkili sanal makineler için işlem durumunu okur. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/read | Ağ arabirimiyle ilişkilendirilmiş ağ güvenlik grubunu alır. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/write | Ağ arabirimiyle ilişkilendirilmiş bir ağ güvenlik grubunu ekler. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/operationStatuses/read | Sanal makineler için işlem durumunu okur. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/performMaintenance/action | Sanal makinede bakım işlemi gerçekleştirir. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/providers/Microsoft.Insights/diagnosticSettings/read | Tanılama ayarlarını alın. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/providers/Microsoft.Insights/diagnosticSettings/write | Tanılama ayarlarını ekleyin veya değiştirin. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/providers/Microsoft.Insights/metricDefinitions/read | Ölçüm tanımlarını alır. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/read | Sanal makinelerin listesini alır. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/redeploy/action | Sanal makineyi yeniden dağıtır. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/restart/action | Sanal makineleri yeniden başlatır. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/shutdown/action | Sanal makineyi kapatın. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/start/action | Sanal makineyi başlatın. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/stop/action | Sanal makineyi durdurur. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/write | Sanal makineleri ekleyin veya değiştirin. |

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.ClassicNetwork/gatewaySupportedDevices/read | Desteklenen cihazların listesini alır. |
> | Eylem | Microsoft.ClassicNetwork/networkSecurityGroups/delete | Ağ güvenlik grubunu siler. |
> | Eylem | Microsoft.ClassicNetwork/networkSecurityGroups/operationStatuses/read | Ağ güvenlik grubu için işlem durumunu okur. |
> | Eylem | Microsoft.ClassicNetwork/networkSecurityGroups/read | Ağ güvenlik grubunu alır. |
> | Eylem | Microsoft.ClassicNetwork/networkSecurityGroups/securityRules/delete | Güvenlik kuralını siler. |
> | Eylem | Microsoft.ClassicNetwork/networkSecurityGroups/securityRules/operationStatuses/read | Aü güvenlik grubu için işlem durumunu okur. |
> | Eylem | Microsoft.ClassicNetwork/networkSecurityGroups/securityRules/read | Güvenlik kuralını alır. |
> | Eylem | Microsoft.ClassicNetwork/networkSecurityGroups/securityRules/write | Güvenlik kuralı alır veya güncelleştirir. |
> | Eylem | Microsoft.ClassicNetwork/networkSecurityGroups/write | Yeni bir ağ güvenlik grubu ekler. |
> | Eylem | Microsoft.ClassicNetwork/quotas/read | Abonelik için kotayı alın. |
> | Eylem | Microsoft.ClassicNetwork/register/action | Classic Network'e Kaydol |
> | Eylem | Microsoft.ClassicNetwork/reservedIps/delete | Ayrılmış bir IP'yi silin. |
> | Eylem | Microsoft.ClassicNetwork/reservedIps/join/action | Ayrılmış bir IP'ye katılın |
> | Eylem | Microsoft.ClassicNetwork/reservedIps/link/action | Ayrılmış bir IP'ye bağlantı oluşturun |
> | Eylem | Microsoft.ClassicNetwork/reservedIps/operationStatuses/read | Ayrılmış IP'ler için işlem durumunu okur |
> | Eylem | Microsoft.ClassicNetwork/reservedIps/read | Ayrılmış IP'leri alır |
> | Eylem | Microsoft.ClassicNetwork/reservedIps/write | Yeni bir ayrılmış IP ekleyin |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/capabilities/read | Özellikleri gösterir |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/checkIPAddressAvailability/action | Bir sanal ağdaki belirli bir IP adresinin kullanılabilirliğini denetler. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/delete | Sanal ağı siler. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRevokedCertificates/delete | Bir istemci sertifikası iptalini geri alır. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRevokedCertificates/read | İptal edilen istemci sertifikalarını okuyun. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRevokedCertificates/write | Bir istemci sertifikasını iptal eder. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates/delete | Sanal ağın ağ geçidi istemci sertifikasını siler. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates/download/action | Parmak izine göre sertifika indirir. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates/listPackage/action | Sanal ağ geçidi sertifika paketini listeler. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates/read | İstemci kök sertifikalarını bulun. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates/write | Yeni bir istemci kök sertifikasını karşıya yükler. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/gateways/connections/connect/action | Bir siteden siteye ağ geçidi bağlantısına bağlanır. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/gateways/connections/disconnect/action | Bir siteden siteye ağ geçidi bağlantısını keser. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/gateways/connections/read | Bağlantıların listesini alır. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/gateways/connections/test/action | Bir siteden siteye ağ geçidi bağlantısını test eder. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/gateways/delete | Sanal ağ geçidini siler. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/gateways/downloadDeviceConfigurationScript/action | Cihaz yapılandırma betiğini indirir. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/gateways/downloadDiagnostics/action | Ağ geçidi tanılamasını indirir. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/gateways/listCircuitServiceKey/action | Bağlantı hattı hizmet anahtarını alır. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/gateways/listPackage/action | Sanal ağ geçidi paketini listeler. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/gateways/operationStatuses/read | Sanal ağ geçitleri için işlem durumunu okur. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/gateways/packages/read | Sanal ağ geçidi paketini alır. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/gateways/read | Sanal ağ geçitlerini alır. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/gateways/startDiagnostics/action | Sanal ağ geçidi için tanılamayı başlatır. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/gateways/stopDiagnostics/action | Sanal ağ geçidi için tanılamayı durdurur. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/gateways/write | Bir sanal ağ geçidi ekler. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/join/action | Sanal ağa katılır. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/operationStatuses/read | Sanal ağlar için işlem durumunu okur. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/peer/action | Bir sanal ağı başka bir sanal ağ ile eşler. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/read | Sanal ağı alın. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/subnets/associatedNetworkSecurityGroups/delete | Alt ağ ile ilişkilendirilmiş ağ güvenlik grubunu siler. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/subnets/associatedNetworkSecurityGroups/operationStatuses/read | Ağ güvenlik grubuyla ilişkili sanal ağ alt ağı için işlem durumunu okur. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/subnets/associatedNetworkSecurityGroups/read | Alt ağ ile ilişkilendirilmiş ağ güvenlik grubunu alır. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/subnets/associatedNetworkSecurityGroups/write | Alt ağ ile ilişkilendirilmiş bir ağ güvenlik grubu ekler. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/write | Yeni bir sanal ağ ekleyin. |

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.ClassicStorage/capabilities/read | Özellikleri gösterir |
> | Eylem | Microsoft.ClassicStorage/checkStorageAccountAvailability/action | Bir depolama hesabının kullanılabilirliğini denetler. |
> | Eylem | Microsoft.ClassicStorage/disks/read | Depolama hesabı diskini döndürür. |
> | Eylem | Microsoft.ClassicStorage/images/read | Görüntüyü döndürür. |
> | Eylem | Microsoft.ClassicStorage/osImages/read | İşletim sistemi görüntüsünü döndürür. |
> | Eylem | Microsoft.ClassicStorage/publicImages/read | Ortak sanal makine görüntüsünü alır. |
> | Eylem | Microsoft.ClassicStorage/quotas/read | Abonelik için kotayı alın. |
> | Eylem | Microsoft.ClassicStorage/register/action | Klasik Depolamaya Kaydeder |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/delete | Depolama hesabını silin. |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/disks/delete | Verilen depolama hesabı diskini siler. |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/disks/operationStatuses/read | Kaynağın işlem durumunu okur. |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/disks/read | Depolama hesabı diskini döndürür. |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/disks/write | Depolama hesabı diski ekler. |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/images/delete | Belirtilen depolama hesabı görüntüsünü siler. |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/images/read | Depolama hesabı görüntüsünü döndürür. |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/listKeys/action | Depolama hesaplarının erişim anahtarlarını listeler. |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/operationStatuses/read | Kaynağın işlem durumunu okur. |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/osImages/delete | Belirtilen bir depolama hesabı işletim sistemi görüntüsünü siler. |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/osImages/read | Depolama hesabı işletim sistemi görüntüsünü döndürür. |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/read | Belirli bir hesaba yönelik depolama hesabını döndürün. |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/regenerateKey/action | Depolama hesabı için var olan erişim anahtarlarını yeniden oluşturur. |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/services/diagnosticSettings/read | Tanılama ayarlarını alın. |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/services/diagnosticSettings/write | Tanılama ayarlarını ekleyin veya değiştirin. |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/services/metricDefinitions/read | Ölçüm tanımlarını alır. |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/services/metrics/read | Ölçümleri alır. |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/services/read | Kullanılabilir hizmetleri alın. |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/write | Yeni bir depolama hesabı ekler. |

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.CognitiveServices/accounts/delete | API hesaplarını siler |
> | Eylem | Microsoft.CognitiveServices/accounts/listKeys/action | Anahtarları Listele |
> | Eylem | Microsoft.CognitiveServices/accounts/providers/Microsoft.Insights/diagnosticSettings/read | Kaynağın tanılama ayarını alır. |
> | Eylem | Microsoft.CognitiveServices/accounts/providers/Microsoft.Insights/diagnosticSettings/write | Kaynağın tanılama ayarını oluşturur veya güncelleştirir. |
> | Eylem | Microsoft.CognitiveServices/accounts/providers/Microsoft.Insights/logDefinitions/read | Bilişsel Hizmetler hesabı için kullanılabilir günlükleri alır |
> | Eylem | Microsoft.CognitiveServices/accounts/providers/Microsoft.Insights/metricDefinitions/read | Bilişsel Hizmetler için kullanılabilir ölçümleri alır. |
> | Eylem | Microsoft.CognitiveServices/accounts/read | API hesaplarını okur. |
> | Eylem | Microsoft.CognitiveServices/accounts/regenerateKey/action | Anahtarı Yeniden Oluştur |
> | Eylem | Microsoft.CognitiveServices/accounts/skus/read | Mevcut kaynağa ait kullanılabilen SKU'ları okur. |
> | Eylem | Microsoft.CognitiveServices/accounts/usages/read | Mevcut bir kaynak için kota kullanımını alın. |
> | Eylem | Microsoft.CognitiveServices/accounts/write | API Hesaplarını yazar. |
> | Eylem | Microsoft.CognitiveServices/locations/checkSkuAvailability/action | Bir abonelik için kullanılabilir SKU'ları okur. |
> | Eylem | Microsoft.CognitiveServices/Operations/read | Tüm kullanılabilir işlemleri listeleyin |
> | Eylem | Microsoft.CognitiveServices/register/action | Bilişsel Hizmetler için Abonelik kaydeder |
> | Eylem | Microsoft.CognitiveServices/skus/read | Bilişsel Hizmetler için kullanılabilir SKU'ları okur. |

## <a name="microsoftcommerce"></a>Microsoft.Commerce

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Commerce/RateCard/read | Veri, kaynak/ölçer meta verileri ve belirtilen abonelik için hızlarını döndürür sunar. |
> | Eylem | Microsoft.Commerce/UsageAggregates/read | Microsoft Azure'nın tüketim abonelik tarafından alır. Sonuç toplamalar içeren kullanım verileri, abonelik ve kaynak ilgili bilgi için belirli bir zaman aralığı. |

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Compute/availabilitySets/delete | Kullanılabilirlik kümesini siler |
> | Eylem | Microsoft.Compute/availabilitySets/read | Bir kullanılabilirlik kümesinin özelliklerini al |
> | Eylem | Microsoft.Compute/availabilitySets/vmSizes/read | Kullanılabilirlik kümesinde bir sanal makine oluşturmak veya güncelleştirmek için kullanılabilir boyutları listele |
> | Eylem | Microsoft.Compute/availabilitySets/write | Yeni bir kullanılabilirlik kümesi oluşturur veya mevcut kümeyi güncelleştirir |
> | Eylem | Microsoft.Compute/disks/beginGetAccess/action | Blob erişimi için Diskin SAS URI'sini alır |
> | Eylem | Microsoft.Compute/disks/delete | Diski siler |
> | Eylem | Microsoft.Compute/disks/endGetAccess/action | Diskin SAS URI'sini iptal eder |
> | Eylem | Microsoft.Compute/disks/read | Diskin özelliklerini alır |
> | Eylem | Microsoft.Compute/disks/write | Yeni bir Disk oluşturur veya mevcut Diski güncelleştirir |
> | Eylem | Microsoft.Compute/images/delete | Görüntüyü siler |
> | Eylem | Microsoft.Compute/images/read | Görüntü özelliklerini alır |
> | Eylem | Microsoft.Compute/images/write | Yeni bir Görüntü oluşturur veya mevcut bir Görüntüyü güncelleştirir |
> | Eylem | Microsoft.Compute/locations/capsOperations/read | Zaman uyumsuz bir Caps işleminin durumunu alır |
> | Eylem | Microsoft.Compute/locations/diskOperations/read | Zaman uyumsuz bir Disk işlem durumunu alır |
> | Eylem | Microsoft.Compute/locations/operations/read | Bir zaman uyumsuz işlemin durumunu alır |
> | Eylem | Microsoft.Compute/locations/publishers/artifacttypes/offers/read | Platform Görüntüsü Teklifi'nin özelliklerini alma |
> | Eylem | Microsoft.Compute/locations/publishers/artifacttypes/offers/skus/read | Platform Görüntüsü SKU'sunun özelliklerini alma |
> | Eylem | Microsoft.Compute/locations/publishers/artifacttypes/offers/skus/versions/read | Platform Görüntüsü Sürümü özelliklerini alma |
> | Eylem | Microsoft.Compute/locations/publishers/artifacttypes/types/read | VMExtension Türü özelliklerini alır |
> | Eylem | Microsoft.Compute/locations/publishers/artifacttypes/types/versions/read | VMExtension Sürümü özelliklerini alır |
> | Eylem | Microsoft.Compute/locations/publishers/read | Yayımcının özelliklerini alır |
> | Eylem | Microsoft.Compute/locations/runCommands/read | Konumda kullanılabilir çalıştırma komutlarını listeler |
> | Eylem | Microsoft.Compute/locations/usages/read | Aboneliğin bir konumdaki işlem kaynaklarında bulunan hizmet sınırlarını ve geçerli kullanım miktarlarını alır |
> | Eylem | Microsoft.Compute/locations/vmSizes/read | Bir konumdaki kullanılabilir sanal makine boyutlarını listeler |
> | Eylem | Microsoft.Compute/operations/read | Microsoft.Compute kaynak sağlayıcısındaki kullanılabilir işlemleri listele |
> | Eylem | Microsoft.Compute/register/action | Aboneliği Microsoft.Compute kaynak sağlayıcısına kaydeder |
> | Eylem | Microsoft.Compute/restorePointCollections/delete | Geri yükleme noktası koleksiyonu ve bağımsız geri yükleme noktalarını siler |
> | Eylem | Microsoft.Compute/restorePointCollections/read | Geri yükleme noktası koleksiyonunun özelliklerini alır |
> | Eylem | Microsoft.Compute/restorePointCollections/restorePoints/delete | Geri yükleme noktasını siler |
> | Eylem | Microsoft.Compute/restorePointCollections/restorePoints/read | Geri yükleme noktasının özelliklerini alır |
> | Eylem | Microsoft.Compute/restorePointCollections/restorePoints/retrieveSasUris/action | Blob SAS URI'leriyle birlikte geri yükleme noktasının özelliklerini alır |
> | Eylem | Microsoft.Compute/restorePointCollections/restorePoints/write | Yeni bir geri yükleme noktası oluşturur |
> | Eylem | Microsoft.Compute/restorePointCollections/write | Yeni bir geri yükleme noktası koleksiyonu oluşturur veya mevcut olanı güncelleştirir |
> | Eylem | Microsoft.Compute/sharedVMImages/delete | SharedVMImage'i siler |
> | Eylem | Microsoft.Compute/sharedVMImages/read | SharedVMImage özelliklerini al |
> | Eylem | Microsoft.Compute/sharedVMImages/versions/delete | SharedVMImageVersion Sil |
> | Eylem | Microsoft.Compute/sharedVMImages/versions/read | SharedVMImageVersion özelliklerini alma |
> | Eylem | Microsoft.Compute/sharedVMImages/versions/replicate/action | SharedVMImageVersion'ı hedef bölgelere çoğaltma |
> | Eylem | Microsoft.Compute/sharedVMImages/versions/write | Yeni bir SharedVMImageVersion oluşturur veya mevcut bir SharedVMImageVersion'ı güncelleştirir |
> | Eylem | Microsoft.Compute/sharedVMImages/write | Yeni bir SharedVMImage oluşturur veya mevcut bir SharedVMImage'i güncelleştirir |
> | Eylem | Microsoft.Compute/skus/read | Aboneliğiniz için bulunan Microsoft.Compute SKU listesini alır |
> | Eylem | Microsoft.Compute/snapshots/beginGetAccess/action | Blob erişimi için anlık görüntü SAS URI'sini Al |
> | Eylem | Microsoft.Compute/snapshots/delete | Anlık Görüntüyü siler |
> | Eylem | Microsoft.Compute/snapshots/endGetAccess/action | Anlık görüntü SAS URI'sini iptal et |
> | Eylem | Microsoft.Compute/snapshots/read | Anlık Görüntünün özelliklerin alır |
> | Eylem | Microsoft.Compute/snapshots/write | Yeni bir Anlık Görüntü oluşturur veya mevcut Anlık Görüntüyü güncelleştirir |
> | Eylem | Microsoft.Compute/virtualMachines/capture/action | Sanal sabit diskleri kopyalayarak sanal makineyi yakalar ve benzer sanal makineler oluşturmak için kullanılabilecek bir şablon oluşturur |
> | Eylem | Microsoft.Compute/virtualMachines/convertToManagedDisks/action | Sanal makinenin blob tabanlı disklerini yönetilen disklere dönüştürür |
> | Eylem | Microsoft.Compute/virtualMachines/deallocate/action | Sanal makineyi kapatır ve işlem kaynaklarını serbest bırakır |
> | Eylem | Microsoft.Compute/virtualMachines/delete | Sanal makineyi siler |
> | Eylem | Microsoft.Compute/virtualMachines/extensions/delete | Sanal makine uzantısını siler |
> | Eylem | Microsoft.Compute/virtualMachines/extensions/read | Bir sanal makine uzantısının özelliklerini alır |
> | Eylem | Microsoft.Compute/virtualMachines/extensions/write | Yeni bir sanal makine uzantısı oluşturur veya mevcut uzantıyı güncelleştirir |
> | Eylem | Microsoft.Compute/virtualMachines/generalize/action | Sanal makine durumunu Genelleştirmiş olarak ayarlar ve sanal makineyi yakalama için hazırlar |
> | Eylem | Microsoft.Compute/virtualMachines/instanceView/read | Sanal makine ve kaynaklarının ayrıntılı çalışma zamanı durumunu alır |
> | DataAction | Microsoft.Compute/virtualMachines/login/action | Bir sanal makinede normal bir kullanıcı olarak oturum açın |
> | DataAction | Microsoft.Compute/virtualMachines/loginAsAdmin/action | Windows yöneticisi veya Linux kök kullanıcı ayrıcalıklarıyla bir sanal makinede oturum açın |
> | Eylem | Microsoft.Compute/virtualMachines/performMaintenance/action | VM'de Bakım İşlemini gerçekleştirir. |
> | Eylem | Microsoft.Compute/virtualMachines/powerOff/action | Sanal makineyi kapatır. Sanal makinenin faturalandırılmaya devam edecek unutmayın. |
> | Eylem | Microsoft.Compute/virtualMachines/providers/Microsoft.Insights/metricDefinitions/read | Sanal Makine Ölçüm Tanımlarını Okur |
> | Eylem | Microsoft.Compute/virtualMachines/read | Bir sanal makinenin özelliklerini alır |
> | Eylem | Microsoft.Compute/virtualMachines/redeploy/action | Sanal makineyi yeniden dağıtır |
> | Eylem | Microsoft.Compute/virtualMachines/reimage/action | Fark kayıt diski kullanarak sanal makine görüntüsünü yeniden oluşturur. |
> | Eylem | Microsoft.Compute/virtualMachines/restart/action | Sanal makineyi yeniden başlatır |
> | Eylem | Microsoft.Compute/virtualMachines/runCommand/action | Sanal makinede önceden tanımlanmış bir betik yürütür |
> | Eylem | Microsoft.Compute/virtualMachines/start/action | Sanal makineyi başlatır |
> | Eylem | Microsoft.Compute/virtualMachines/vmSizes/read | Sanal makineyi güncelleştirmek için kullanılabilir boyutları listeler |
> | Eylem | Microsoft.Compute/virtualMachines/write | Yeni bir sanal makine oluşturur veya mevcut sanal makineyi güncelleştirir |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/deallocate/action | Sanal makineyi kapatır ve Sanal Makine Ölçek Kümesi örneklerinin işlem kaynaklarını serbest bırakır  |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/delete | Sanal Makine Ölçek Kümesini siler |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/delete/action | Sanal Makine Ölçek Kümesinin örneklerini siler |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/extensions/delete | Sanal Makine Ölçek Kümesi Uzantısını siler |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/extensions/read | Sanal Makine Ölçek Kümesi Uzantısının özelliklerini alır |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/extensions/write | Yeni bir Sanal Makine Ölçek Kümesi Uzantısı oluşturur veya mevcut olanı güncelleştirir |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/forceRecoveryServiceFabricPlatformUpdateDomainWalk/action | Takılmış bekleyen bir güncelleştirmeyi tamamlamak için Service Fabric Sanal Makine Ölçek Kümesinin platform güncelleştirme etki alanlarını el ile gezin |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/instanceView/read | Sanal Makine Ölçek Kümesinin örnek görünümünü alır |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/manualUpgrade/action | Örnekleri, Sanal Makine Ölçek Kümesinin son modeline el ile güncelleştirir |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/networkInterfaces/read | Bir Sanal Makine Ölçek Kümesinin tüm ağ arabirimlerinin özelliklerini alma |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/osUpgradeHistory/read | Bir Sanal Makine Ölçek Kümesi için işletim sistemi yükseltme geçmişini alır |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/performMaintenance/action | Sanal Makine Ölçek Kümesi'nin örneklerinde planlanmış bakım gerçekleştirir |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/powerOff/action | Sanal Makine Ölçek Kümesinin örneklerini kapatır |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/providers/Microsoft.Insights/metricDefinitions/read | Sanal Makine Ölçek Kümesi Ölçüm Tanımlarını Okur |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/publicIPAddresses/read | Bir Sanal Makine Ölçek Kümesinin tüm genel IP adreslerinin özelliklerini alma |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/read | Bir Sanal Makine Ölçek Kümesinin özelliklerini alın |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/redeploy/action | Sanal Makine Ölçek Kümesi'nin örneklerini yeniden dağıtın |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/reimage/action | Sanal Makine Ölçek Kümesi örneklerinin görüntülerini yeniden oluşturur |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/restart/action | Sanal Makine Ölçek Kümesinin örneklerini yeniden başlatır |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/rollingUpgrades/cancel/action | Sanal Makine Ölçek Kümesinin sıralı yükseltmesini iptal eder |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/rollingUpgrades/read | Bir Sanal Makine Ölçek Kümesi için son Sıralı Yükseltme durumunu alın |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/scale/action | Mevcut bir Sanal Makine Ölçek Kümesinin belirtilen örnek sayısında Ölçeği Daraltma/Ölçeği Genişletme işlemlerini gerçekleştirebildiğini doğrulayın |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/skus/read | Mevcut bir Sanal Makine Ölçek Kümesi için geçerli SKU'ları listeler |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/start/action | Sanal Makine Ölçek Kümesinin örneklerini başlatır |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/deallocate/action | VM Ölçek Kümesindeki bir Sanal Makineyi kapatır ve işlem kaynaklarını serbest bırakır. |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/delete | VM Ölçek Kümesindeki belirli bir Sanal Makineyi silin. |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/instanceView/read | VM Ölçek Kümesindeki bir Sanal Makinenin örnek görünümünü alır. |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/networkInterfaces/ipConfigurations/publicIPAddresses/read | Sanal makine ölçek kümesi kullanılarak oluşturulan genel IP adresi özelliklerini alır. Sanal makine ölçek kümesi en fazla ipconfiguration (özel IP) başına bir genel IP oluşturun |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/networkInterfaces/ipConfigurations/read | Bir veya tüm IP yapılandırmasında sanal makine ölçek kümesi kullanılarak oluşturulan ağ arabirimi özelliklerini alır. Özel IP IP yapılandırmaları temsil eder |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/networkInterfaces/read | Sanal Makine Ölçek Kümesi kullanılarak oluşturulan bir sanal makinenin ağ arabirimlerinden birinin veya tümünün özelliklerini alma |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/performMaintenance/action | Bir Sanal Makine Ölçek Kümesi'ndeki Sanal Makine örneğinde planlanmış bakım gerçekleştirir |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/powerOff/action | VM Ölçek Kümesindeki bir Sanal Makine örneğini kapatır. |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/providers/Microsoft.Insights/metricDefinitions/read | Ölçek Kümesindeki Sanal Makine Ölçüm Tanımlarını Okur |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/read | VM Ölçek Kümesindeki bir Sanal Makinenin özelliklerini alır |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/redeploy/action | Bir Sanal Makine Ölçek Kümesi'ndeki Sanal Makine örneğini yeniden dağıtır |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/reimage/action | Sanal Makine Ölçek Kümesindeki Sanal Makine örneğinin görüntüsünü yeniden oluşturur. |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/restart/action | VM Ölçek Kümesindeki bir Sanal Makine örneğini yeniden başlatır. |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/start/action | VM Ölçek Kümesindeki bir Sanal Makine örneğini başlatır. |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/write | VM Ölçek Kümesindeki bir Sanal Makinenin özelliklerini güncelleştirir |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/write | Yeni bir Sanal Makine Ölçek Kümesi oluşturur veya mevcut kümeyi güncelleştirir |

## <a name="microsoftconsumption"></a>Microsoft.Consumption

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Consumption/balances/read | Bir yönetim grubu için fatura dönemi için Özet kullanımı listeleyin. |
> | Eylem | Microsoft.Consumption/budgets/read | Bir abonelik veya bir yönetim grubu tarafından bütçeleri listeleyin. |
> | Eylem | Microsoft.Consumption/budgets/write | Oluşturur, güncelleştirme ve bir abonelik veya bir yönetim grubu tarafından bütçeleri silme. |
> | Eylem | Microsoft.Consumption/marketplaces/read | EA ve WebDirect abonelikler için bir kapsam Market kaynak kullanım ayrıntılarını listeler. |
> | Eylem | Microsoft.Consumption/operations/read | Microsoft.Consumption kaynak sağlayıcısı tarafından desteklenen tüm işlemleri listele. |
> | Eylem | Microsoft.Consumption/pricesheets/read | Bir abonelik veya bir yönetim grubu için Pricesheets veri listeleyin. |
> | Eylem | Microsoft.Consumption/reservationDetails/read | Ayırma sipariş veya yönetim gruplarına göre ayrılmış örnekler kullanımı ayrıntılarını listeler. Günlük düzeyi başına örnek başına ayrıntıları verilerdir. |
> | Eylem | Microsoft.Consumption/reservationRecommendations/read | Bir abonelik için ayrılmış örnekler için tek veya paylaşılan önerileri listeler. |
> | Eylem | Microsoft.Consumption/reservationSummaries/read | Ayırma sipariş veya yönetim gruplarına göre ayrılmış örnekler için Özet kullanımı listeleyin. Özet günlük veya aylık düzeyinde ya da veridir. |
> | Eylem | Microsoft.Consumption/reservationTransactions/read | Yönetim grubu tarafından ayrılmış örnekler için işlem geçmişini listeleyin. |
> | Eylem | Microsoft.Consumption/tenants/register/action | Kapsam için bir kiracı tarafından Microsoft.Consumption eylemi kaydedin. |
> | Eylem | Microsoft.Consumption/terms/read | Bir abonelik veya bir yönetim grubu koşulları listeler. |
> | Eylem | Microsoft.Consumption/usageDetails/read | EA ve WebDirect abonelikler için bir kapsam kullanım ayrıntılarını listeler. |

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.ContainerInstance/containerGroups/containers/logs/read | Belirli bir kapsayıcı için günlükleri alın. |
> | Eylem | Microsoft.ContainerInstance/containerGroups/delete | Belirli kapsayıcı grubunu silin. |
> | Eylem | Microsoft.ContainerInstance/containerGroups/providers/Microsoft.Insights/diagnosticSettings/read | Kapsayıcı grubun tanılama ayarını alır. |
> | Eylem | Microsoft.ContainerInstance/containerGroups/providers/Microsoft.Insights/diagnosticSettings/write | Kapsayıcı grubun tanılama ayarını oluşturur veya güncelleştirir. |
> | Eylem | Microsoft.ContainerInstance/containerGroups/providers/Microsoft.Insights/metricDefinitions/read | Kapsayıcı grup için mevcut ölçümleri alır. |
> | Eylem | Microsoft.ContainerInstance/containerGroups/read | Tüm kapsayıcı gruplarını alın. |
> | Eylem | Microsoft.ContainerInstance/containerGroups/write | Belirli bir kapsayıcı grubunu oluşturun veya güncelleştirin. |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.ContainerRegistry/checkNameAvailability/read | Kapsayıcı kayıt defteri adı kullanılabilir olup olmadığını denetler. |
> | Eylem | Microsoft.ContainerRegistry/locations/operationResults/read | Zaman uyumsuz işlem sonucunu alır |
> | Eylem | Microsoft.ContainerRegistry/operations/read | Tüm kullanılabilir Azure kapsayıcı kayıt defteri REST API işlemlerinin listeler |
> | Eylem | Microsoft.ContainerRegistry/register/action | Kapsayıcı kayıt kaynak sağlayıcısı için aboneliği kaydeder ve kapsayıcı kayıt defterleri oluşturulmasını sağlar. |
> | Eylem | Microsoft.ContainerRegistry/registries/delete | Bir kapsayıcı kayıt siler. |
> | Eylem | Microsoft.ContainerRegistry/registries/eventGridFilters/delete | Olay kılavuz filtresi kapsayıcı kayıt defterinden siler. |
> | Eylem | Microsoft.ContainerRegistry/registries/eventGridFilters/read | Belirtilen olay kılavuz filtresi özelliklerini alır veya belirtilen kapsayıcı kayıt defteri için tüm olay kılavuz filtreler listelenmiştir. |
> | Eylem | Microsoft.ContainerRegistry/registries/eventGridFilters/write | Belirtilen parametrelerle bir kapsayıcı kayıt defteri olay kılavuz filtresi güncelleştirir veya oluşturur. |
> | Eylem | Microsoft.ContainerRegistry/registries/importImage/action | Kapsayıcı kayıt defterinde belirtilen parametrelerle görüntü alın. |
> | Eylem | Microsoft.ContainerRegistry/registries/listCredentials/action | Belirtilen kapsayıcı kayıt defteri için oturum açma kimlik bilgilerini listeler. |
> | Eylem | Microsoft.ContainerRegistry/registries/listUsages/read | Belirtilen kapsayıcı kayıt defteri için kota kullanımları listeler. |
> | Eylem | Microsoft.ContainerRegistry/registries/operationStatuses/read | Bir kayıt defteri zaman uyumsuz işlem durumunu alır |
> | Eylem | Microsoft.ContainerRegistry/registries/providers/Microsoft.Insights/diagnosticSettings/read | Kaynak için tanılama ayarını alır |
> | Eylem | Microsoft.ContainerRegistry/registries/providers/Microsoft.Insights/diagnosticSettings/write | Kaynak için tanılama ayarını oluşturur veya güncelleştirir |
> | Eylem | Microsoft.ContainerRegistry/registries/providers/Microsoft.Insights/metricDefinitions/read | Microsoft ContainerRegistry için kullanılabilir ölçümleri alır |
> | Eylem | Microsoft.ContainerRegistry/registries/read | Belirtilen kapsayıcı kayıt defteri özelliklerini alır veya tüm kapsayıcı kayıt defterleri belirtilen kaynak grubu veya abonelik altında listelenir. |
> | Eylem | Microsoft.ContainerRegistry/registries/regenerateCredential/action | Belirtilen kapsayıcı kayıt defteri için oturum açma kimlik bilgilerini yeniden oluşturur. |
> | Eylem | Microsoft.ContainerRegistry/registries/replications/delete | Bir çoğaltma kapsayıcı kayıt defterinden siler. |
> | Eylem | Microsoft.ContainerRegistry/registries/replications/operationStatuses/read | Bir çoğaltma zaman uyumsuz işlem durumunu alır |
> | Eylem | Microsoft.ContainerRegistry/registries/replications/read | Belirtilen çoğaltma özelliklerini alır veya belirtilen kapsayıcı kayıt defteri için tüm çoğaltmalar listeler. |
> | Eylem | Microsoft.ContainerRegistry/registries/replications/write | Belirtilen parametrelerle bir çoğaltma için bir kapsayıcı kayıt defterini güncelleştirir veya oluşturur. |
> | Eylem | Microsoft.ContainerRegistry/registries/webhooks/delete | Bir Web kancası kapsayıcı kayıt defterinden siler. |
> | Eylem | Microsoft.ContainerRegistry/registries/webhooks/getCallbackConfig/action | Web kancası için URI hizmetinin yapılandırmasını ve özel üstbilgi alır. |
> | Eylem | Microsoft.ContainerRegistry/registries/webhooks/listEvents/action | Belirtilen Web kancası için son olayları listeler. |
> | Eylem | Microsoft.ContainerRegistry/registries/webhooks/operationStatuses/read | Bir Web kancası zaman uyumsuz işlem durumunu alır |
> | Eylem | Microsoft.ContainerRegistry/registries/webhooks/ping/action | Web kancası için gönderilecek bir ping olay tetiklenir. |
> | Eylem | Microsoft.ContainerRegistry/registries/webhooks/read | Belirtilen Web kancası özelliklerini alır veya belirtilen kapsayıcı kayıt defteri için tüm Web kancalarını listeler. |
> | Eylem | Microsoft.ContainerRegistry/registries/webhooks/write | Belirtilen parametrelerle bir Web kancası için bir kapsayıcı kayıt defterini güncelleştirir veya oluşturur. |
> | Eylem | Microsoft.ContainerRegistry/registries/write | Belirtilen parametrelerle bir kapsayıcı kayıt defterini güncelleştirir veya oluşturur. |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.ContainerService/containerServices/delete | Bir kapsayıcı hizmetini siler |
> | Eylem | Microsoft.ContainerService/containerServices/read | Bir kapsayıcı hizmeti Al |
> | Eylem | Microsoft.ContainerService/containerServices/write | Yeni bir kapsayıcı hizmeti oluşturur veya mevcut kümeyi güncelleştirir |
> | Eylem | Microsoft.ContainerService/managedClusters/accessProfiles/listCredential/action | Bir yönetilen küme erişim profili listesi kimlik bilgilerini kullanarak rol adına göre alma |
> | Eylem | Microsoft.ContainerService/managedClusters/accessProfiles/read | Rol adı tarafından yönetilen küme erişim profili Al |
> | Eylem | Microsoft.ContainerService/managedClusters/delete | Yönetilen bir küme siler |
> | Eylem | Microsoft.ContainerService/managedClusters/providers/Microsoft.Insights/diagnosticSettings/read | Yönetilen küme kaynağı tanılama ayarını alma |
> | Eylem | Microsoft.ContainerService/managedClusters/providers/Microsoft.Insights/diagnosticSettings/write | Oluşturur veya yönetilen küme kaynağı tanılama ayarını güncelleştirir |
> | Eylem | Microsoft.ContainerService/managedClusters/providers/Microsoft.Insights/logDefinitions/read | Yönetilen bir küme için kullanılabilir günlüklerini alır |
> | Eylem | Microsoft.ContainerService/managedClusters/providers/Microsoft.Insights/metricDefinitions/read | Yönetilen bir küme için kullanılabilir ölçümleri alır |
> | Eylem | Microsoft.ContainerService/managedClusters/read | Yönetilen bir küme Al |
> | Eylem | Microsoft.ContainerService/managedClusters/write | Yeni bir yönetilen kümesi oluşturur veya mevcut kümeyi güncelleştirir |

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.ContentModerator/applications/delete | İşlemi Siler |
> | Eylem | Microsoft.ContentModerator/applications/listSecrets/action | Parolaları Listele |
> | Eylem | Microsoft.ContentModerator/applications/listSingleSignOnToken/action | Çoklu oturum açma belirteçleri okuma |
> | Eylem | Microsoft.ContentModerator/applications/read | İşlemi Okur |
> | Eylem | Microsoft.ContentModerator/applications/write | İşlemi Yazar |
> | Eylem | Microsoft.ContentModerator/applications/write | İşlemi Yazar |
> | Eylem | Microsoft.ContentModerator/listCommunicationPreference/action | Liste iletişim tercihi |
> | Eylem | Microsoft.ContentModerator/operations/read | okuma işlemleri |
> | Eylem | Microsoft.ContentModerator/updateCommunicationPreference/action | Güncelleştirme iletişim tercihi |

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.CustomerInsights/hubs/adobemetadata/action | Azure müşteri Öngörüler Adobe meta verileri güncelle |
> | Eylem | Microsoft.CustomerInsights/hubs/adobemetadata/read | Tüm Azure müşteri Öngörüler Adobe meta verilerini okuma |
> | Eylem | Microsoft.CustomerInsights/hubs/authorizationPolicies/delete | Tüm Azure müşteri Öngörüler paylaşılan erişim imzası ilkelerini Sil |
> | Eylem | Microsoft.CustomerInsights/hubs/authorizationPolicies/read | Herhangi bir Azure müşteri öngörü paylaşılan erişim imzası İlkesi okuma |
> | Eylem | Microsoft.CustomerInsights/hubs/authorizationPolicies/regeneratePrimaryKey/action | Azure müşteri Öngörüler paylaşılan erişim imzası ilkesinin birincil anahtarını yeniden oluşturma |
> | Eylem | Microsoft.CustomerInsights/hubs/authorizationPolicies/regenerateSecondaryKey/action | Azure müşteri Öngörüler paylaşılan erişim imzası İlkesi ikincil anahtarını yeniden oluşturma |
> | Eylem | Microsoft.CustomerInsights/hubs/authorizationPolicies/write | Herhangi bir Azure müşteri Öngörüler paylaşılan erişim imzası ilke güncelle |
> | Eylem | Microsoft.CustomerInsights/hubs/connectors/activate/action | Tüm Azure müşteri Öngörüler Bağlayıcısı etkinleştir |
> | Eylem | Microsoft.CustomerInsights/hubs/connectors/activate/action | Tüm Azure müşteri Öngörüler Bağlayıcısı etkinleştir |
> | Eylem | Microsoft.CustomerInsights/hubs/connectors/delete | Tüm Azure müşteri Öngörüler bağlayıcısını silin |
> | Eylem | Microsoft.CustomerInsights/hubs/connectors/getruntimestatus/action | Tüm Azure müşteri Öngörüler Bağlayıcısı çalışma zamanı durumunu Al |
> | Eylem | Microsoft.CustomerInsights/hubs/connectors/mappings/activate/action | Tüm Azure müşteri Öngörüler Bağlayıcısı eşleme etkinleştir |
> | Eylem | Microsoft.CustomerInsights/hubs/connectors/mappings/delete | Tüm Azure müşteri Öngörüler Bağlayıcısı eşleme Sil |
> | Eylem | Microsoft.CustomerInsights/hubs/connectors/mappings/operations/read | Bir Azure müşteri Öngörüler Bağlayıcısı eşleme işlem sonucunu oku |
> | Eylem | Microsoft.CustomerInsights/hubs/connectors/mappings/read | Tüm Azure müşteri Öngörüler Bağlayıcısı eşleme okuma |
> | Eylem | Microsoft.CustomerInsights/hubs/connectors/mappings/write | Oluşturma veya güncelleştirme tüm Azure müşteri Öngörüler Bağlayıcısı eşlemesi |
> | Eylem | Microsoft.CustomerInsights/hubs/connectors/operations/read | Bir Azure müşteri Öngörüler Bağlayıcısı işlem sonucunu oku |
> | Eylem | Microsoft.CustomerInsights/hubs/connectors/read | Tüm Azure müşteri Öngörüler Bağlayıcısı okuma |
> | Eylem | Microsoft.CustomerInsights/hubs/connectors/saveauthinfo/action | Tüm Azure müşteri Öngörüler Bağlayıcısı kimlik doğrulama bilgileri güncelle |
> | Eylem | Microsoft.CustomerInsights/hubs/connectors/update/action | Tüm Azure müşteri Öngörüler Bağlayıcısı güncelleştir |
> | Eylem | Microsoft.CustomerInsights/hubs/connectors/write | Tüm Azure müşteri Öngörüler Bağlayıcısı güncelle |
> | Eylem | Microsoft.CustomerInsights/hubs/crmmetadata/action | Azure müşteri Öngörüler Crm meta verileri güncelle |
> | Eylem | Microsoft.CustomerInsights/hubs/crmmetadata/read | Tüm Azure müşteri Öngörüler Crm meta verilerini okuma |
> | Eylem | Microsoft.CustomerInsights/hubs/delete | Tüm Azure müşteri Öngörüler hub'ını silmek |
> | Eylem | Microsoft.CustomerInsights/hubs/gdpr/delete | Tüm Azure müşteri Öngörüler Gdpr Sil |
> | Eylem | Microsoft.CustomerInsights/hubs/gdpr/read | Tüm Azure müşteri Öngörüler Gdpr okuma |
> | Eylem | Microsoft.CustomerInsights/hubs/gdpr/write | Tüm Azure müşteri Öngörüler Gdpr güncelle |
> | Eylem | Microsoft.CustomerInsights/hubs/getbillingcredits/read | Azure müşteri Öngörüler Hub faturalama krediler alırsınız |
> | Eylem | Microsoft.CustomerInsights/hubs/getbillinghistory/read | Azure müşteri Öngörüler Hub fatura geçmişini alma |
> | Eylem | Microsoft.CustomerInsights/hubs/images/delete | Herhangi bir Azure müşteri Öngörüler görüntü Sil |
> | Eylem | Microsoft.CustomerInsights/hubs/images/read | Herhangi bir Azure müşteri Öngörüler görüntü okuma |
> | Eylem | Microsoft.CustomerInsights/hubs/images/write | Herhangi bir Azure müşteri Öngörüler görüntü güncelle |
> | Eylem | Microsoft.CustomerInsights/hubs/interactions/delete | Tüm azure müşteri Öngörüler etkileşimleri Sil |
> | Eylem | Microsoft.CustomerInsights/hubs/interactions/operations/read | Bir Azure müşteri Öngörüler etkileşim işlem sonucunu oku |
> | Eylem | Microsoft.CustomerInsights/hubs/interactions/read | Azure müşteri Öngörüler etkileşimi okuma |
> | Eylem | Microsoft.CustomerInsights/hubs/interactions/suggestrelationshiplinks/action | İlişki bağlantılar için herhangi bir Azure müşteri Öngörüler etkileşimi önerisi |
> | Eylem | Microsoft.CustomerInsights/hubs/interactions/write | Azure müşteri Öngörüler etkileşimi güncelle |
> | Eylem | Microsoft.CustomerInsights/hubs/kpi/delete | Tüm Azure müşteri Öngörüler ana performans göstergesinin Sil |
> | Eylem | Microsoft.CustomerInsights/hubs/kpi/operations/read | Bir Azure müşteri Öngörüler anahtar performans göstergelerini işlem sonucunu oku |
> | Eylem | Microsoft.CustomerInsights/hubs/kpi/read | Tüm Azure müşteri Öngörüler ana performans göstergesinin okuma |
> | Eylem | Microsoft.CustomerInsights/hubs/kpi/reprocess/action | Tüm Azure müşteri Öngörüler ana performans göstergelerini yeniden işleyin |
> | Eylem | Microsoft.CustomerInsights/hubs/kpi/write | Tüm Azure müşteri Öngörüler ana performans göstergesinin güncelle |
> | Eylem | Microsoft.CustomerInsights/hubs/links/delete | Tüm Azure müşteri Öngörüler bağlantılar Sil |
> | Eylem | Microsoft.CustomerInsights/hubs/links/operations/read | Bir Azure müşteri Öngörüler bağlantılar işlem sonucunu oku |
> | Eylem | Microsoft.CustomerInsights/hubs/links/read | Tüm Azure müşteri Öngörüler bağlantılar okuma |
> | Eylem | Microsoft.CustomerInsights/hubs/links/write | Tüm Azure müşteri bağlantılar güncelle |
> | Eylem | Microsoft.CustomerInsights/hubs/msemetadata/action | Azure müşteri Öngörüler Mse meta verileri güncelle |
> | Eylem | Microsoft.CustomerInsights/hubs/msemetadata/read | Tüm Azure müşteri Öngörüler Mse meta verilerini okuma |
> | Eylem | Microsoft.CustomerInsights/hubs/operationresults/read | Azure müşteri Öngörüler Hub İşlem sonuçlarını alır |
> | Eylem | Microsoft.CustomerInsights/hubs/predictions/delete | Tüm Azure müşteri Öngörüler tahminleri Sil |
> | Eylem | Microsoft.CustomerInsights/hubs/predictions/operations/read | Bir Azure müşteri Öngörüler tahminleri işlem sonucunu oku |
> | Eylem | Microsoft.CustomerInsights/hubs/predictions/read | Tüm Azure müşteri Öngörüler tahminleri okuma |
> | Eylem | Microsoft.CustomerInsights/hubs/predictions/write | Tüm Azure müşteri tahminleri güncelle |
> | Eylem | Microsoft.CustomerInsights/hubs/predictivematchpolicies/delete | Tüm Azure müşteri Öngörüler Tahmine dayalı eşleşme ilkelerini Sil |
> | Eylem | Microsoft.CustomerInsights/hubs/predictivematchpolicies/operations/read | Bir Azure müşteri Öngörüler Tahmine dayalı eşleşme ilkeleri işlem sonucunu oku |
> | Eylem | Microsoft.CustomerInsights/hubs/predictivematchpolicies/read | Tüm Azure müşteri Öngörüler Tahmine dayalı eşleşme ilkeleri okuma |
> | Eylem | Microsoft.CustomerInsights/hubs/predictivematchpolicies/write | Tüm Azure müşteri Öngörüler Tahmine dayalı eşleşme ilkeleri güncelle |
> | Eylem | Microsoft.CustomerInsights/hubs/profiles/delete | Herhangi bir Azure müşteri Öngörüler profil Sil |
> | Eylem | Microsoft.CustomerInsights/hubs/profiles/operations/read | Bir Azure müşteri Öngörüler profili işlem sonucunu oku |
> | Eylem | Microsoft.CustomerInsights/hubs/profiles/read | Tüm Azure müşteri Öngörüler profilini okuma |
> | Eylem | Microsoft.CustomerInsights/hubs/profiles/write | Herhangi bir Azure müşteri Öngörüler profil yazma |
> | Eylem | Microsoft.CustomerInsights/hubs/providers/Microsoft.Insights/diagnosticSettings/read | Kaynak için tanılama ayarını alır |
> | Eylem | Microsoft.CustomerInsights/hubs/providers/Microsoft.Insights/diagnosticSettings/write | Kaynak için tanılama ayarını oluşturur veya güncelleştirir |
> | Eylem | Microsoft.CustomerInsights/hubs/providers/Microsoft.Insights/logDefinitions/read | Kaynak için kullanılabilir günlüklerini alır |
> | Eylem | Microsoft.CustomerInsights/hubs/providers/Microsoft.Insights/metricDefinitions/read | Kaynak için kullanılabilir ölçümleri alır |
> | Eylem | Microsoft.CustomerInsights/hubs/read | Tüm Azure müşteri Öngörüler hub'ı okuyun |
> | Eylem | Microsoft.CustomerInsights/hubs/relationshiplinks/delete | Tüm Azure müşteri Öngörüler ilişki bağlantılar Sil |
> | Eylem | Microsoft.CustomerInsights/hubs/relationshiplinks/operations/read | Bir Azure müşteri Öngörüler ilişki bağlantılar işlem sonucunu oku |
> | Eylem | Microsoft.CustomerInsights/hubs/relationshiplinks/read | Tüm Azure müşteri Öngörüler ilişki bağlantılar okuma |
> | Eylem | Microsoft.CustomerInsights/hubs/relationshiplinks/write | Tüm Azure müşteri Öngörüler ilişki bağlantılar güncelle |
> | Eylem | Microsoft.CustomerInsights/hubs/relationships/delete | Tüm Azure müşteri Öngörüler ilişkileri silin |
> | Eylem | Microsoft.CustomerInsights/hubs/relationships/operations/read | Bir Azure müşteri Öngörüler ilişkileri işlem sonucunu oku |
> | Eylem | Microsoft.CustomerInsights/hubs/relationships/read | Tüm Azure müşteri Öngörüler ilişkiler okuma |
> | Eylem | Microsoft.CustomerInsights/hubs/relationships/write | Tüm Azure müşteri Öngörüler ilişkiler güncelle |
> | Eylem | Microsoft.CustomerInsights/hubs/roleAssignments/delete | Tüm Azure müşteri Öngörüler Rbac atamasını silin |
> | Eylem | Microsoft.CustomerInsights/hubs/roleAssignments/operations/read | Bir Azure müşteri Öngörüler Rbac atama işlem sonucunu oku |
> | Eylem | Microsoft.CustomerInsights/hubs/roleAssignments/read | Herhangi bir Azure müşteri Öngörüler Rbac atama okuma |
> | Eylem | Microsoft.CustomerInsights/hubs/roleAssignments/write | Herhangi bir Azure müşteri Öngörüler Rbac atama güncelle |
> | Eylem | Microsoft.CustomerInsights/hubs/roles/read | Herhangi bir Azure müşteri Öngörüler Rbac rolü okuma |
> | Eylem | Microsoft.CustomerInsights/hubs/salesforcemetadata/action | Azure müşteri Öngörüler SalesForce meta verileri güncelle |
> | Eylem | Microsoft.CustomerInsights/hubs/salesforcemetadata/read | Tüm Azure müşteri Öngörüler SalesForce meta verilerini okuma |
> | Eylem | Microsoft.CustomerInsights/hubs/segments/delete | Tüm Azure müşteri Öngörüler segmentleri silin |
> | Eylem | Microsoft.CustomerInsights/hubs/segments/dynamic/action | Tüm Azure müşteri Insight dinamik yönetim kesim |
> | Eylem | Microsoft.CustomerInsights/hubs/segments/read | Tüm Azure müşteri Öngörüler kesimleri okuma |
> | Eylem | Microsoft.CustomerInsights/hubs/segments/static/action | Herhangi bir Azure müşteri Insight statik yönetim kesim |
> | Eylem | Microsoft.CustomerInsights/hubs/segments/write | Tüm Azure müşteri Öngörüler kesimleri güncelle |
> | Eylem | Microsoft.CustomerInsights/hubs/sqlconnectionstrings/delete | Tüm Azure müşteri Öngörüler SqlConnectionStrings Sil |
> | Eylem | Microsoft.CustomerInsights/hubs/sqlconnectionstrings/read | Tüm Azure müşteri Öngörüler SqlConnectionStrings okuma |
> | Eylem | Microsoft.CustomerInsights/hubs/sqlconnectionstrings/write | Tüm Azure müşteri Öngörüler SqlConnectionStrings güncelle |
> | Eylem | Microsoft.CustomerInsights/hubs/suggesttypeschema/action | Örnek verileri önermek türü şeması oluştur |
> | Eylem | Microsoft.CustomerInsights/hubs/tenantmanagement/read | Tüm Azure müşteri Öngörüler hub ayarlarını yönet |
> | Eylem | Microsoft.CustomerInsights/hubs/views/delete | Herhangi bir Azure müşteri Öngörüler uygulama Görünümü Sil |
> | Eylem | Microsoft.CustomerInsights/hubs/views/read | Herhangi bir Azure müşteri Öngörüler uygulama görünümü okuma |
> | Eylem | Microsoft.CustomerInsights/hubs/views/write | Herhangi bir Azure müşteri Öngörüler uygulama Görünümü güncelle |
> | Eylem | Microsoft.CustomerInsights/hubs/widgettypes/read | Tüm Azure müşteri Öngörüler uygulama pencere öğesi türlerine okuma |
> | Eylem | Microsoft.CustomerInsights/hubs/write | Tüm Azure müşteri Öngörüler Hub güncelle |
> | Eylem | Microsoft.CustomerInsights/operations/read | Azure müşteri Öngörüler API Metadatas okuma |
> | Eylem | Microsoft.CustomerInsights/register/action | Müşteri Öngörüler kaynak sağlayıcısı için aboneliği kaydeder ve müşteri Öngörüler kaynakların oluşturulmasını sağlar |
> | Eylem | Microsoft.CustomerInsights/unregister/action | Müşteri Öngörüler kaynak sağlayıcısı için abonelik kaydını siler |

## <a name="microsoftdatabricks"></a>Microsoft.Databricks

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Databricks/workspaces/delete | Bir çalışma alanı kaldırır. |
> | Eylem | Microsoft.Databricks/workspaces/read | Çalışma alanlarının bir listesini alır. |
> | Eylem | Microsoft.Databricks/workspaces/write | Bir çalışma alanı oluşturur. |

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.DataCatalog/catalogs/delete | Katalog siler. |
> | Eylem | Microsoft.DataCatalog/catalogs/read | Katalog ya da kataloglar abonelik veya kaynak grubu altında için özellikleri alır. |
> | Eylem | Microsoft.DataCatalog/catalogs/write | Katalog oluşturur veya etiketleri ve Katalog özelliklerini güncelleştirir. |
> | Eylem | Microsoft.DataCatalog/checkNameAvailability/action | Kiracı için katalog adı kullanılabilirliğini denetler. |
> | Eylem | Microsoft.DataCatalog/operations/read | Microsoft.DataCatalog kaynak sağlayıcısı kullanılabilir işlemleri listeler. |
> | Eylem | Microsoft.DataCatalog/register/action | Abonelik Microsoft.DataCatalog kaynak sağlayıcısına kaydeder. |
> | Eylem | Microsoft.DataCatalog/unregister/action | Abonelik Microsoft.DataCatalog kaynak sağlayıcısından gelen kaydını siler. |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.DataFactory/datafactories/activitywindows/read | Belirtilen parametrelerle veri fabrikasında etkinlik Windows okur. |
> | Eylem | Microsoft.DataFactory/datafactories/datapipelines/activities/activitywindows/read | Belirtilen parametrelerle ardışık düzen etkinliği için etkinlik Windows okur. |
> | Eylem | Microsoft.DataFactory/datafactories/datapipelines/activitywindows/read | Belirtilen parametrelerle ardışık düzeni için etkinlik Windows okur. |
> | Eylem | Microsoft.DataFactory/datafactories/datapipelines/delete | Hiçbir ardışık düzen siler. |
> | Eylem | Microsoft.DataFactory/datafactories/datapipelines/pause/action | Hiçbir ardışık düzen duraklatır. |
> | Eylem | Microsoft.DataFactory/datafactories/datapipelines/read | Hiçbir ardışık düzen okur. |
> | Eylem | Microsoft.DataFactory/datafactories/datapipelines/resume/action | Hiçbir ardışık düzen sürdürür. |
> | Eylem | Microsoft.DataFactory/datafactories/datapipelines/update/action | Hiçbir ardışık düzen güncelleştirir. |
> | Eylem | Microsoft.DataFactory/datafactories/datapipelines/write | Hiçbir ardışık düzen güncelleştirir veya oluşturur. |
> | Eylem | Microsoft.DataFactory/datafactories/datasets/activitywindows/read | Belirtilen parametrelerle veri kümesi için etkinlik Windows okur. |
> | Eylem | Microsoft.DataFactory/datafactories/datasets/delete | Herhangi bir veri kümesini siler. |
> | Eylem | Microsoft.DataFactory/datafactories/datasets/read | Herhangi bir veri kümesi okur. |
> | Eylem | Microsoft.DataFactory/datafactories/datasets/sliceruns/read | Veri dilimi çalıştırmak için belirtilen başlangıç saati ile sağlanan veri kümesinde okur. |
> | Eylem | Microsoft.DataFactory/datafactories/datasets/slices/read | Veri dilimi belirli dönemde alır. |
> | Eylem | Microsoft.DataFactory/datafactories/datasets/slices/write | Veri dilimi durumunu güncelleştirin. |
> | Eylem | Microsoft.DataFactory/datafactories/datasets/write | Oluşturur veya herhangi bir veri kümesi güncelleştirir. |
> | Eylem | Microsoft.DataFactory/datafactories/delete | Veri Fabrikası siler. |
> | Eylem | Microsoft.DataFactory/datafactories/gateways/connectioninfo/action | Tüm ağ geçidi için bağlantı bilgileri okur. |
> | Eylem | Microsoft.DataFactory/datafactories/gateways/delete | Herhangi bir ağ geçidini siler. |
> | Eylem | Microsoft.DataFactory/datafactories/gateways/listauthkeys/action | Kimlik doğrulama anahtarları için herhangi bir ağ geçidi listeler. |
> | Eylem | Microsoft.DataFactory/datafactories/gateways/read | Tüm ağ geçidi okur. |
> | Eylem | Microsoft.DataFactory/datafactories/gateways/regenerateauthkey/action | Kimlik doğrulama anahtarları için herhangi bir ağ geçidini yeniden oluşturur. |
> | Eylem | Microsoft.DataFactory/datafactories/gateways/write | Oluşturur veya herhangi bir ağ geçidini güncelleştirir. |
> | Eylem | Microsoft.DataFactory/datafactories/linkedServices/delete | Herhangi bir bağlantılı hizmeti siler. |
> | Eylem | Microsoft.DataFactory/datafactories/linkedServices/read | Herhangi bir bağlantılı hizmeti okur. |
> | Eylem | Microsoft.DataFactory/datafactories/linkedServices/write | Oluşturur veya herhangi bir bağlantılı hizmeti güncelleştirir. |
> | Eylem | Microsoft.DataFactory/datafactories/providers/Microsoft.Insights/diagnosticSettings/read | Kaynak için tanılama ayarını alır |
> | Eylem | Microsoft.DataFactory/datafactories/providers/Microsoft.Insights/diagnosticSettings/write | Kaynak için tanılama ayarını oluşturur veya güncelleştirir |
> | Eylem | Microsoft.DataFactory/datafactories/providers/Microsoft.Insights/metricDefinitions/read | Datafactories için kullanılabilir ölçümleri alır |
> | Eylem | Microsoft.DataFactory/datafactories/read | Veri Fabrikası okur. |
> | Eylem | Microsoft.DataFactory/datafactories/runs/loginfo/read | SAS URI'sini günlükleri içeren bir blob kapsayıcısını okur. |
> | Eylem | Microsoft.DataFactory/datafactories/tables/delete | Herhangi bir veri kümesini siler. |
> | Eylem | Microsoft.DataFactory/datafactories/tables/read | Herhangi bir veri kümesi okur. |
> | Eylem | Microsoft.DataFactory/datafactories/tables/write | Oluşturur veya herhangi bir veri kümesi güncelleştirir. |
> | Eylem | Microsoft.DataFactory/datafactories/write | Veri Fabrikası güncelleştirir veya oluşturur. |
> | Eylem | Microsoft.DataFactory/factories/cancelpipelinerun/action | Çalışma kimliği tarafından belirtilen çalışma ardışık düzen iptal eder |
> | Eylem | Microsoft.DataFactory/factories/datasets/delete | Herhangi bir veri kümesini siler. |
> | Eylem | Microsoft.DataFactory/factories/datasets/read | Herhangi bir veri kümesi okur. |
> | Eylem | Microsoft.DataFactory/factories/datasets/write | Oluşturur veya herhangi bir veri kümesi güncelleştirir. |
> | Eylem | Microsoft.DataFactory/factories/delete | Veri Fabrikası siler. |
> | Eylem | Microsoft.DataFactory/factories/integrationruntimes/delete | Tüm tümleştirme çalışma zamanı siler. |
> | Eylem | Microsoft.DataFactory/factories/integrationruntimes/getconnectioninfo/read | Tümleştirme çalışma zamanı bağlantı bilgileri okur. |
> | Eylem | Microsoft.DataFactory/factories/integrationruntimes/getstatus/read | Tümleştirme çalışma zamanı durumunu okur. |
> | Eylem | Microsoft.DataFactory/factories/integrationruntimes/listauthkeys/read | Kimlik doğrulama anahtarları herhangi tümleştirmesi çalışma zamanı için listeler. |
> | Eylem | Microsoft.DataFactory/factories/integrationruntimes/monitoringdata/read | Tüm tümleştirme çalışma zamanı için izleme verilerini alır. |
> | Eylem | Microsoft.DataFactory/factories/integrationruntimes/nodes/delete | Düğümü için belirtilen tümleştirmesi çalışma zamanı siler. |
> | Eylem | Microsoft.DataFactory/factories/integrationruntimes/nodes/ipAddress/action | Belirtilen düğüm Integration zamanının IP adresini döndürür. |
> | Eylem | Microsoft.DataFactory/factories/integrationruntimes/nodes/write | Kendini barındıran tümleştirmesi çalışma zamanı düğüm güncelleştirir. |
> | Eylem | Microsoft.DataFactory/factories/integrationruntimes/read | Tüm tümleştirme çalışma zamanı okur. |
> | Eylem | Microsoft.DataFactory/factories/integrationruntimes/regenerateauthkey/action | Kimlik doğrulama anahtarları için belirtilen tümleştirmesi çalışma zamanı yeniden oluşturur. |
> | Eylem | Microsoft.DataFactory/factories/integrationruntimes/start/action | Tüm tümleştirme çalışma zamanı başlatır. |
> | Eylem | Microsoft.DataFactory/factories/integrationruntimes/stop/action | Tüm tümleştirme çalışma zamanı durdurur. |
> | Eylem | Microsoft.DataFactory/factories/integrationruntimes/synccredentials/action | Kimlik bilgileri için belirtilen tümleştirmesi çalışma zamanı eşitlenir. |
> | Eylem | Microsoft.DataFactory/factories/integrationruntimes/upgrade/action | Belirtilen tümleştirmesi çalışma zamanı yükseltir. |
> | Eylem | Microsoft.DataFactory/factories/integrationruntimes/write | Oluşturur veya hiçbir tümleştirme çalışma zamanı güncelleştirir. |
> | Eylem | Microsoft.DataFactory/factories/linkedServices/delete | Bağlantılı hizmetinin siler. |
> | Eylem | Microsoft.DataFactory/factories/linkedServices/read | Bağlantılı hizmetinin okur. |
> | Eylem | Microsoft.DataFactory/factories/linkedServices/write | Bağlantılı hizmet güncelle |
> | Eylem | Microsoft.DataFactory/factories/pipelineruns/activityruns/read | Etkinlik Kimliği çalıştırmak için belirtilen ardışık düzenini çalışır okur |
> | Eylem | Microsoft.DataFactory/factories/pipelineruns/read | Ardışık Düzen çalıştırır okur. |
> | Eylem | Microsoft.DataFactory/factories/pipelines/createrun/action | Bir farklı çalıştır ardışık düzeni için oluşturur. |
> | Eylem | Microsoft.DataFactory/factories/pipelines/delete | Ardışık Düzen siler. |
> | Eylem | Microsoft.DataFactory/factories/pipelines/read | Ardışık Düzen okur. |
> | Eylem | Microsoft.DataFactory/factories/pipelines/write | Ardışık Düzen güncelle |
> | Eylem | Microsoft.DataFactory/factories/providers/Microsoft.Insights/diagnosticSettings/read | Kaynak için tanılama ayarını alır |
> | Eylem | Microsoft.DataFactory/factories/providers/Microsoft.Insights/diagnosticSettings/write | Kaynak için tanılama ayarını oluşturur veya güncelleştirir |
> | Eylem | Microsoft.DataFactory/factories/providers/Microsoft.Insights/logDefinitions/read | Oluşturucuları için kullanılabilir günlüklerini alır |
> | Eylem | Microsoft.DataFactory/factories/providers/Microsoft.Insights/metricDefinitions/read | Oluşturucuları için kullanılabilir ölçümleri alır |
> | Eylem | Microsoft.DataFactory/factories/read | Veri Fabrikası okur. |
> | Eylem | Microsoft.DataFactory/factories/triggers/delete | Hiçbir tetikleyici siler. |
> | Eylem | Microsoft.DataFactory/factories/triggers/read | Hiçbir tetikleyici okur. |
> | Eylem | Microsoft.DataFactory/factories/triggers/start/action | Hiçbir tetikleyici başlatır. |
> | Eylem | Microsoft.DataFactory/factories/triggers/stop/action | Hiçbir tetikleyici durdurur. |
> | Eylem | Microsoft.DataFactory/factories/triggers/triggerruns/read | Tetikleyici çalıştırır okur. |
> | Eylem | Microsoft.DataFactory/factories/triggers/write | Oluşturur veya hiçbir tetikleyici güncelleştirir. |
> | Eylem | Microsoft.DataFactory/factories/write | Veri Fabrikası güncelle |
> | Eylem | Microsoft.DataFactory/locations/configureFactoryRepo/action | Depo üreteci için yapılandırır. |
> | Eylem | Microsoft.DataFactory/register/action | Veri Fabrikası kaynak sağlayıcısı için aboneliği kaydeder. |
> | Eylem | Microsoft.DataFactory/unregister/action | Veri Fabrikası kaynak sağlayıcısı için abonelik kaydını siler. |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.DataLakeAnalytics/accounts/computePolicies/delete | Bir işlem ilkeyi silin. |
> | Eylem | Microsoft.DataLakeAnalytics/accounts/computePolicies/read | Bir işlem İlkesi hakkında bilgi alın. |
> | Eylem | Microsoft.DataLakeAnalytics/accounts/computePolicies/write | Oluşturun veya bir işlem İlkesi güncelleştirin. |
> | Eylem | Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/delete | DataLakeAnalytics hesap DataLakeStore hesabından bağlantısını. |
> | Eylem | Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/read | DataLakeAnalytics hesabı bağlı DataLakeStore hesabı hakkında bilgi alın. |
> | Eylem | Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/write | Oluşturun veya bir bağlantılı DataLakeStore hesabı DataLakeAnalytics hesabının güncelleştirin. |
> | Eylem | Microsoft.DataLakeAnalytics/accounts/delete | Bir DataLakeAnalytics hesabı silin. |
> | Eylem | Microsoft.DataLakeAnalytics/accounts/firewallRules/delete | Bir güvenlik duvarı kuralını siler. |
> | Eylem | Microsoft.DataLakeAnalytics/accounts/firewallRules/read | Bir güvenlik duvarı kuralı hakkında bilgi alın. |
> | Eylem | Microsoft.DataLakeAnalytics/accounts/firewallRules/write | Oluşturun veya bir güvenlik duvarı kuralı güncelleştirin. |
> | Eylem | Microsoft.DataLakeAnalytics/accounts/operationResults/read | DataLakeAnalytics hesabı işleminin sonucu alın. |
> | Eylem | Microsoft.DataLakeAnalytics/accounts/providers/Microsoft.Insights/diagnosticSettings/read | DataLakeAnalytics hesabı için tanılama ayarlarını al. |
> | Eylem | Microsoft.DataLakeAnalytics/accounts/providers/Microsoft.Insights/diagnosticSettings/write | Oluşturun veya DataLakeAnalytics hesabının tanılama ayarlarını güncelleştirin. |
> | Eylem | Microsoft.DataLakeAnalytics/accounts/providers/Microsoft.Insights/logDefinitions/read | Kullanılabilir günlüklerini DataLakeAnalytics hesabı alamıyor. |
> | Eylem | Microsoft.DataLakeAnalytics/accounts/providers/Microsoft.Insights/metricDefinitions/read | Kullanılabilir ölçümler için DataLakeAnalytics hesabı edinin. |
> | Eylem | Microsoft.DataLakeAnalytics/accounts/read | DataLakeAnalytics hesabınız hakkında bilgi alın. |
> | Eylem | Microsoft.DataLakeAnalytics/accounts/storageAccounts/Containers/listSasTokens/action | DataLakeAnalytics hesabın bağlantılı bir depolama hesabının depolama kapsayıcıları için SAS belirteci listeleyin. |
> | Eylem | Microsoft.DataLakeAnalytics/accounts/storageAccounts/Containers/read | DataLakeAnalytics hesabının bağlı bir depolama hesabında kapsayıcıları alın. |
> | Eylem | Microsoft.DataLakeAnalytics/accounts/storageAccounts/delete | Bir depolama hesabı DataLakeAnalytics hesabından bağlantısını. |
> | Eylem | Microsoft.DataLakeAnalytics/accounts/storageAccounts/read | DataLakeAnalytics hesabın bağlantılı bir depolama hesabı hakkında bilgi alın. |
> | Eylem | Microsoft.DataLakeAnalytics/accounts/storageAccounts/write | Oluşturun veya bağlı bir depolama hesabında DataLakeAnalytics hesabının güncelleştirin. |
> | Eylem | Microsoft.DataLakeAnalytics/accounts/TakeOwnership/action | Diğer kullanıcılar tarafından gönderilen işleri iptal etme izni verin. |
> | Eylem | Microsoft.DataLakeAnalytics/accounts/write | Oluşturun veya bir DataLakeAnalytics hesabı güncelleştirin. |
> | Eylem | Microsoft.DataLakeAnalytics/locations/capability/read | Bir abonelik Yetenek bilgilerini al DataLakeAnalytics kullanma ile ilgili. |
> | Eylem | Microsoft.DataLakeAnalytics/locations/checkNameAvailability/action | DataLakeAnalytics hesap adı kullanılabilirliğini denetleyin. |
> | Eylem | Microsoft.DataLakeAnalytics/locations/operationResults/read | DataLakeAnalytics hesabı işleminin sonucu alın. |
> | Eylem | Microsoft.DataLakeAnalytics/operations/read | DataLakeAnalytics kullanılabilir işlemleri alın. |
> | Eylem | Microsoft.DataLakeAnalytics/register/action | DataLakeAnalytics aboneliğine kaydolun. |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.DataLakeStore/accounts/delete | Bir DataLakeStore hesabı silin. |
> | Eylem | Microsoft.DataLakeStore/accounts/enableKeyVault/action | KeyVault DataLakeStore hesabı için etkinleştirin. |
> | Eylem | Microsoft.DataLakeStore/accounts/eventGridFilters/delete | Bir EventGrid filtresi silin. |
> | Eylem | Microsoft.DataLakeStore/accounts/eventGridFilters/read | Bir EventGrid filtresi alın. |
> | Eylem | Microsoft.DataLakeStore/accounts/eventGridFilters/write | Oluşturun veya bir EventGrid filtresi güncelleştirin. |
> | Eylem | Microsoft.DataLakeStore/accounts/firewallRules/delete | Bir güvenlik duvarı kuralını siler. |
> | Eylem | Microsoft.DataLakeStore/accounts/firewallRules/read | Bir güvenlik duvarı kuralı hakkında bilgi alın. |
> | Eylem | Microsoft.DataLakeStore/accounts/firewallRules/write | Oluşturun veya bir güvenlik duvarı kuralı güncelleştirin. |
> | Eylem | Microsoft.DataLakeStore/accounts/operationResults/read | DataLakeStore hesabı işleminin sonucu alın. |
> | Eylem | Microsoft.DataLakeStore/accounts/providers/Microsoft.Insights/diagnosticSettings/read | DataLakeStore hesabı için tanılama ayarlarını al. |
> | Eylem | Microsoft.DataLakeStore/accounts/providers/Microsoft.Insights/diagnosticSettings/write | Oluşturun veya DataLakeStore hesabının tanılama ayarlarını güncelleştirin. |
> | Eylem | Microsoft.DataLakeStore/accounts/providers/Microsoft.Insights/logDefinitions/read | Kullanılabilir günlüklerini DataLakeStore hesabı alamıyor. |
> | Eylem | Microsoft.DataLakeStore/accounts/providers/Microsoft.Insights/metricDefinitions/read | Kullanılabilir ölçümler için DataLakeStore hesabı edinin. |
> | Eylem | Microsoft.DataLakeStore/accounts/read | DataLakeStore hesabınız hakkında bilgi alın. |
> | Eylem | Microsoft.DataLakeStore/accounts/Superuser/action | Süper kullanıcı ile Microsoft.Authorization/roleAssignments/write verilmişse Data Lake Store üzerinde verin. |
> | Eylem | Microsoft.DataLakeStore/accounts/trustedIdProviders/delete | Güvenilen kimlik sağlayıcısı silin. |
> | Eylem | Microsoft.DataLakeStore/accounts/trustedIdProviders/read | Güvenilen kimlik sağlayıcısı hakkında bilgi alın. |
> | Eylem | Microsoft.DataLakeStore/accounts/trustedIdProviders/write | Oluşturun veya bir güvenilen kimlik sağlayıcısı güncelleştirin. |
> | Eylem | Microsoft.DataLakeStore/accounts/write | Oluşturun veya bir DataLakeStore hesabı güncelleştirin. |
> | Eylem | Microsoft.DataLakeStore/locations/capability/read | Bir abonelik Yetenek bilgilerini al DataLakeStore kullanma ile ilgili. |
> | Eylem | Microsoft.DataLakeStore/locations/checkNameAvailability/action | DataLakeStore hesap adı kullanılabilirliğini denetleyin. |
> | Eylem | Microsoft.DataLakeStore/locations/operationResults/read | DataLakeStore hesabı işleminin sonucu alın. |
> | Eylem | Microsoft.DataLakeStore/operations/read | DataLakeStore kullanılabilir işlemleri alın. |
> | Eylem | Microsoft.DataLakeStore/register/action | DataLakeStore aboneliğine kaydolun. |

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.DataMigration/locations/operationResults/read | 202 Kabul Edildi yanıtı ile ilgili uzun süreli bir işlemin durumunu alın |
> | Eylem | Microsoft.DataMigration/locations/operationStatuses/read | 202 Kabul Edildi yanıtı ile ilgili uzun süreli bir işlemin durumunu alın |
> | Eylem | Microsoft.DataMigration/register/action | Aboneliği Azure Veritabanı Geçiş Hizmet sağlayıcısına kaydeder |
> | Eylem | Microsoft.DataMigration/services/checkStatus/action | Hizmetin dağıtılmış ve çalışır durumda olup olmadığını denetleyin |
> | Eylem | Microsoft.DataMigration/services/delete | Bir kaynağı ve tüm alt öğelerini siler |
> | Eylem | Microsoft.DataMigration/services/projects/accessArtifacts/action | GET veya PUT proje yapıtları için kullanılabilen bir URL oluşturun |
> | Eylem | Microsoft.DataMigration/services/projects/delete | Bir kaynağı ve tüm alt öğelerini siler |
> | Eylem | Microsoft.DataMigration/services/projects/read | Kaynaklar hakkındaki bilgileri okuyun |
> | Eylem | Microsoft.DataMigration/services/projects/tasks/cancel/action | O anda çalışıyorsa görevi iptal edin |
> | Eylem | Microsoft.DataMigration/services/projects/tasks/delete | Bir kaynağı ve tüm alt öğelerini siler |
> | Eylem | Microsoft.DataMigration/services/projects/tasks/read | Kaynaklar hakkındaki bilgileri okuyun |
> | Eylem | Microsoft.DataMigration/services/projects/tasks/write | Azure Veritabanı Geçiş Hizmeti görevlerini çalıştır |
> | Eylem | Microsoft.DataMigration/services/projects/write | Azure Veritabanı Geçiş Hizmeti görevlerini çalıştır |
> | Eylem | Microsoft.DataMigration/services/read | Kaynaklar hakkındaki bilgileri okuyun |
> | Eylem | Microsoft.DataMigration/services/slots/delete | Bir kaynağı ve tüm alt öğelerini siler |
> | Eylem | Microsoft.DataMigration/services/slots/read | Kaynaklar hakkındaki bilgileri okuyun |
> | Eylem | Microsoft.DataMigration/services/slots/write | Kaynakları ve özelliklerini oluşturun ya da güncelleştirin |
> | Eylem | Microsoft.DataMigration/services/start/action | Geçişleri yeniden işlemesine izin vermek için DMS hizmetini başlatın |
> | Eylem | Microsoft.DataMigration/services/stop/action | Maliyetlerini en aza indirmek için DMS hizmetini durdurun |
> | Eylem | Microsoft.DataMigration/services/write | Kaynakları ve özelliklerini oluşturun ya da güncelleştirin |
> | Eylem | Microsoft.DataMigration/skus/read | DMS kaynakları tarafından desteklenen SKU'ların bir listesini alın. |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.DBforMySQL/locations/performanceTiers/read | Performans katmanı kullanılabilir listesini döndürür. |
> | Eylem | Microsoft.DBforMySQL/performanceTiers/read | Performans katmanı kullanılabilir listesini döndürür. |
> | Eylem | Microsoft.DBforMySQL/servers/delete | Var olan bir sunucuyu siler. |
> | Eylem | Microsoft.DBforMySQL/servers/firewallRules/delete | Mevcut bir güvenlik duvarı kuralını siler. |
> | Eylem | Microsoft.DBforMySQL/servers/firewallRules/read | Güvenlik Duvarı listesi için belirtilen güvenlik duvarı kuralı özellikleri alır veya bir sunucu için kuralları döndür. |
> | Eylem | Microsoft.DBforMySQL/servers/firewallRules/write | Bir güvenlik duvarı kuralı belirtilen parametreleri veya güncelleştirme mevcut bir kuralı oluşturur. |
> | Eylem | Microsoft.DBforMySQL/servers/providers/Microsoft.Insights/diagnosticSettings/read | Kaynak disagnostic ayarını alır |
> | Eylem | Microsoft.DBforMySQL/servers/providers/Microsoft.Insights/diagnosticSettings/write | Kaynak için tanılama ayarını oluşturur veya güncelleştirir |
> | Eylem | Microsoft.DBforMySQL/servers/providers/Microsoft.Insights/metricDefinitions/read | Dönüş türleri veritabanları için kullanılabilir ölçümleri |
> | Eylem | Microsoft.DBforMySQL/servers/read | Sunucuları veya alır belirtilen sunucu için özellikler listesini döndürür. |
> | Eylem | Microsoft.DBforMySQL/servers/recoverableServers/read | Kurtarılabilir MySQL sunucu bilgilerini döndürür |
> | Eylem | Microsoft.DBforMySQL/servers/virtualNetworkRules/delete | Mevcut bir sanal ağ kuralını siler |
> | Eylem | Microsoft.DBforMySQL/servers/virtualNetworkRules/read | Dönüş sanal ağ listesi kuralları veya belirtilen sanal ağ kuralı özellikleri alır. |
> | Eylem | Microsoft.DBforMySQL/servers/virtualNetworkRules/write | Belirtilen parametrelerle bir sanal ağ kuralı oluşturur veya özellikleri veya etiketleri belirtilen sanal ağ kuralı için güncelleştirin. |
> | Eylem | Microsoft.DBforMySQL/servers/write | Sunucu belirtilen parametrelerle oluşturur veya özellikleri veya etiketleri belirtilen sunucu güncelleştirme. |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.DBforPostgreSQL/locations/performanceTiers/read | Performans katmanı kullanılabilir listesini döndürür. |
> | Eylem | Microsoft.DBforPostgreSQL/performanceTiers/read | Performans katmanı kullanılabilir listesini döndürür. |
> | Eylem | Microsoft.DBforPostgreSQL/servers/delete | Var olan bir sunucuyu siler. |
> | Eylem | Microsoft.DBforPostgreSQL/servers/firewallRules/delete | Mevcut bir güvenlik duvarı kuralını siler. |
> | Eylem | Microsoft.DBforPostgreSQL/servers/firewallRules/read | Güvenlik Duvarı listesi için belirtilen güvenlik duvarı kuralı özellikleri alır veya bir sunucu için kuralları döndür. |
> | Eylem | Microsoft.DBforPostgreSQL/servers/firewallRules/write | Bir güvenlik duvarı kuralı belirtilen parametreleri veya güncelleştirme mevcut bir kuralı oluşturur. |
> | Eylem | Microsoft.DBforPostgreSQL/servers/providers/Microsoft.Insights/diagnosticSettings/read | Kaynak disagnostic ayarını alır |
> | Eylem | Microsoft.DBforPostgreSQL/servers/providers/Microsoft.Insights/diagnosticSettings/write | Kaynak için tanılama ayarını oluşturur veya güncelleştirir |
> | Eylem | Microsoft.DBforPostgreSQL/servers/providers/Microsoft.Insights/logDefinitions/read | Postgres sunucuları için kullanılabilir günlüklerini alır |
> | Eylem | Microsoft.DBforPostgreSQL/servers/providers/Microsoft.Insights/metricDefinitions/read | Dönüş türleri veritabanları için kullanılabilir ölçümleri |
> | Eylem | Microsoft.DBforPostgreSQL/servers/read | Sunucuları veya alır belirtilen sunucu için özellikler listesini döndürür. |
> | Eylem | Microsoft.DBforPostgreSQL/servers/recoverableServers/read | Kurtarılabilir PostgreSQL sunucu bilgilerini döndürür |
> | Eylem | Microsoft.DBforPostgreSQL/servers/securityAlertPolicies/read | Verilen bir sunucu üzerinde yapılandırılmış sunucu tehdit algılama ilkesi ayrıntılarını alma |
> | Eylem | Microsoft.DBforPostgreSQL/servers/securityAlertPolicies/write | Belirli bir sunucuda Sunucu tehdit algılama ilkesini değiştirme |
> | Eylem | Microsoft.DBforPostgreSQL/servers/virtualNetworkRules/delete | Mevcut bir sanal ağ kuralını siler |
> | Eylem | Microsoft.DBforPostgreSQL/servers/virtualNetworkRules/read | Dönüş sanal ağ listesi kuralları veya belirtilen sanal ağ kuralı özellikleri alır. |
> | Eylem | Microsoft.DBforPostgreSQL/servers/virtualNetworkRules/write | Belirtilen parametrelerle bir sanal ağ kuralı oluşturur veya özellikleri veya etiketleri belirtilen sanal ağ kuralı için güncelleştirin. |
> | Eylem | Microsoft.DBforPostgreSQL/servers/write | Sunucu belirtilen parametrelerle oluşturur veya özellikleri veya etiketleri belirtilen sunucu güncelleştirme. |

## <a name="microsoftdevices"></a>Microsoft.Devices

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Devices/canaryoperationresults/Read | İşlem sonucu için Canary Al |
> | Eylem | Microsoft.Devices/checkNameAvailability/Action | Onay, Iothub adı kullanılamıyor |
> | Eylem | Microsoft.Devices/checkProvisioningServiceNameAvailability/Action | Onay, Iothub adı kullanılamıyor |
> | Eylem | Microsoft.Devices/ElasticPools/diagnosticSettings/read | Kaynak için tanılama ayarını alır |
> | Eylem | Microsoft.Devices/ElasticPools/diagnosticSettings/write | Kaynak için tanılama ayarını oluşturur veya güncelleştirir |
> | Eylem | Microsoft.Devices/elasticPools/iotHubTenants/certificates/Delete | Sertifika siler |
> | Eylem | Microsoft.Devices/elasticPools/iotHubTenants/certificates/generateVerificationCode/Action | Doğrulama kodu oluştur |
> | Eylem | Microsoft.Devices/elasticPools/iotHubTenants/certificates/Read | Sertifikayı alır |
> | Eylem | Microsoft.Devices/elasticPools/iotHubTenants/certificates/verify/Action | Sertifika kaynak doğrulayın |
> | Eylem | Microsoft.Devices/elasticPools/iotHubTenants/certificates/Write | Sertifika güncelle |
> | Eylem | Microsoft.Devices/elasticPools/iotHubTenants/Delete | Iothub Kiracı kaynağı silme |
> | Eylem | Microsoft.Devices/ElasticPools/IotHubTenants/diagnosticSettings/read | Kaynak için tanılama ayarını alır |
> | Eylem | Microsoft.Devices/ElasticPools/IotHubTenants/diagnosticSettings/write | Kaynak için tanılama ayarını oluşturur veya güncelleştirir |
> | Eylem | Microsoft.Devices/elasticPools/iotHubTenants/eventHubEndpoints/consumerGroups/Delete | EventHub tüketici grubu Sil |
> | Eylem | Microsoft.Devices/elasticPools/iotHubTenants/eventHubEndpoints/consumerGroups/Read | EventHub tüketici grubu Al |
> | Eylem | Microsoft.Devices/elasticPools/iotHubTenants/eventHubEndpoints/consumerGroups/Write | EventHub tüketici grubu oluşturma |
> | Eylem | Microsoft.Devices/elasticPools/iotHubTenants/exportDevices/Action | Dışarı aktarma cihazları |
> | Eylem | Microsoft.Devices/elasticPools/iotHubTenants/getStats/Read | Iothub Kiracı istatistikleri kaynak alır |
> | Eylem | Microsoft.Devices/elasticPools/iotHubTenants/importDevices/Action | Cihazları içeri aktarma |
> | Eylem | Microsoft.Devices/elasticPools/iotHubTenants/iotHubKeys/listkeys/Action | Iothub Kiracı anahtarını alır |
> | Eylem | Microsoft.Devices/elasticPools/iotHubTenants/jobs/Read | İş ayrıntıları Iothub gönderme |
> | Eylem | Microsoft.Devices/elasticPools/iotHubTenants/listKeys/Action | Iothub Kiracı anahtarları alır |
> | Eylem | Microsoft.Devices/ElasticPools/IotHubTenants/logDefinitions/read | Iothub hizmeti için kullanılabilir günlük tanımları alır |
> | Eylem | Microsoft.Devices/ElasticPools/IotHubTenants/metricDefinitions/read | Iothub hizmeti için kullanılabilir ölçümleri alır |
> | Eylem | Microsoft.Devices/elasticPools/iotHubTenants/quotaMetrics/Read | Kota ölçümleri alma |
> | Eylem | Microsoft.Devices/elasticPools/iotHubTenants/Read | Iothub Kiracı kaynak alır |
> | Eylem | Microsoft.Devices/elasticPools/iotHubTenants/routing/routes/$testall/Action | Bir ileti tüm var olan yollar karşı test etme |
> | Eylem | Microsoft.Devices/elasticPools/iotHubTenants/routing/routes/$testnew/Action | Bir ileti sağlanan test rota karşı test etme |
> | Eylem | Microsoft.Devices/elasticPools/iotHubTenants/routingEndpointsHealth/Read | Bir Iothub için tüm yönlendirme uç noktaları durumunu alır |
> | Eylem | Microsoft.Devices/elasticPools/iotHubTenants/Write | Iothub Kiracı kaynak güncelle |
> | Eylem | Microsoft.Devices/ElasticPools/metricDefinitions/read | Iothub hizmeti için kullanılabilir ölçümleri alır |
> | Eylem | Microsoft.Devices/iotHubs/canaryoperationresults/Read | İşlem sonucu Canary (eski API) Al |
> | Eylem | Microsoft.Devices/iotHubs/certificates/Delete | Sertifika siler |
> | Eylem | Microsoft.Devices/iotHubs/certificates/generateVerificationCode/Action | Doğrulama kodu oluştur |
> | Eylem | Microsoft.Devices/iotHubs/certificates/Read | Sertifikayı alır |
> | Eylem | Microsoft.Devices/iotHubs/certificates/verify/Action | Sertifika kaynak doğrulayın |
> | Eylem | Microsoft.Devices/iotHubs/certificates/Write | Sertifika güncelle |
> | Eylem | Microsoft.Devices/iotHubs/Delete | Iothub kaynağı silme |
> | Eylem | Microsoft.Devices/IotHubs/diagnosticSettings/read | Kaynak için tanılama ayarını alır |
> | Eylem | Microsoft.Devices/IotHubs/diagnosticSettings/write | Kaynak için tanılama ayarını oluşturur veya güncelleştirir |
> | Eylem | Microsoft.Devices/iotHubs/eventGridFilters/Delete | Olay kılavuz filtre siler |
> | Eylem | Microsoft.Devices/iotHubs/eventGridFilters/Read | Olay kılavuz filtreyi alır |
> | Eylem | Microsoft.Devices/iotHubs/eventGridFilters/Write | Yeni oluşturun veya varolan bir olay kılavuz filtreyi güncelleştirin |
> | Eylem | Microsoft.Devices/iotHubs/eventHubEndpoints/consumerGroups/Delete | EventHub tüketici grubu Sil |
> | Eylem | Microsoft.Devices/iotHubs/eventHubEndpoints/consumerGroups/Read | EventHub tüketici grubu Al |
> | Eylem | Microsoft.Devices/iotHubs/eventHubEndpoints/consumerGroups/Write | EventHub tüketici grubu oluşturma |
> | Eylem | Microsoft.Devices/iotHubs/exportDevices/Action | Dışarı aktarma cihazları |
> | Eylem | Microsoft.Devices/iotHubs/importDevices/Action | Cihazları içeri aktarma |
> | Eylem | Microsoft.Devices/iotHubs/iotHubKeys/listkeys/Action | Verilen ada Iothub anahtarı alma |
> | Eylem | Microsoft.Devices/iotHubs/iotHubStats/Read | Get IotHub Statistics |
> | Eylem | Microsoft.Devices/iotHubs/jobs/Read | İş ayrıntıları Iothub gönderme |
> | Eylem | Microsoft.Devices/iotHubs/listkeys/Action | Tüm Iothub anahtarları alma |
> | Eylem | Microsoft.Devices/IotHubs/logDefinitions/read | Iothub hizmeti için kullanılabilir günlük tanımları alır |
> | Eylem | Microsoft.Devices/IotHubs/metricDefinitions/read | Iothub hizmeti için kullanılabilir ölçümleri alır |
> | Eylem | Microsoft.Devices/iotHubs/operationresults/Read | İşlem sonucu (eski API) Al |
> | Eylem | Microsoft.Devices/iotHubs/quotaMetrics/Read | Kota ölçümleri alma |
> | Eylem | Microsoft.Devices/iotHubs/Read | Iothub kaynaklar alır |
> | Eylem | Microsoft.Devices/iotHubs/routing/$testall/Action | Bir ileti tüm var olan yollar karşı test etme |
> | Eylem | Microsoft.Devices/iotHubs/routing/$testnew/Action | Bir ileti sağlanan test rota karşı test etme |
> | Eylem | Microsoft.Devices/iotHubs/routingEndpointsHealth/Read | Bir Iothub için tüm yönlendirme uç noktaları durumunu alır |
> | Eylem | Microsoft.Devices/iotHubs/skus/Read | Geçerli Iothub SKU'ları alma |
> | Eylem | Microsoft.Devices/iotHubs/Write | Iothub kaynak güncelle |
> | Eylem | Microsoft.Devices/operationresults/Read | İşlem Sonucunu Al |
> | Eylem | Microsoft.Devices/operations/Read | Tüm ResourceProvider işlemleri Al |
> | Eylem | Microsoft.Devices/provisioningServices/certificates/Delete | Sertifika siler |
> | Eylem | Microsoft.Devices/provisioningServices/certificates/generateVerificationCode/Action | Doğrulama kodu oluştur |
> | Eylem | Microsoft.Devices/provisioningServices/certificates/Read | Sertifikayı alır |
> | Eylem | Microsoft.Devices/provisioningServices/certificates/verify/Action | Sertifika kaynak doğrulayın |
> | Eylem | Microsoft.Devices/provisioningServices/certificates/Write | Sertifika güncelle |
> | Eylem | Microsoft.Devices/provisioningServices/Delete | IotDps kaynağı silme |
> | Eylem | Microsoft.Devices/provisioningServices/diagnosticSettings/read | Kaynak için tanılama ayarını alır |
> | Eylem | Microsoft.Devices/provisioningServices/diagnosticSettings/write | Kaynak için tanılama ayarını oluşturur veya güncelleştirir |
> | Eylem | Microsoft.Devices/provisioningServices/keys/listkeys/Action | Anahtar adı için IotDps anahtarları alma |
> | Eylem | Microsoft.Devices/provisioningServices/listkeys/Action | Tüm IotDps anahtarları alma |
> | Eylem | Microsoft.Devices/provisioningServices/logDefinitions/read | Sağlama hizmeti için kullanılabilir günlük tanımları alır |
> | Eylem | Microsoft.Devices/provisioningServices/metricDefinitions/read | Sağlama hizmeti için kullanılabilir ölçümleri alır |
> | Eylem | Microsoft.Devices/provisioningServices/operationresults/Read | DPS işlem sonucu alın |
> | Eylem | Microsoft.Devices/provisioningServices/Read | IotDps kaynak alma |
> | Eylem | Microsoft.Devices/provisioningServices/skus/Read | Geçerli IotDps SKU'ları alma |
> | Eylem | Microsoft.Devices/provisioningServices/Write | IotDps kaynağı oluşturma |
> | Eylem | Microsoft.Devices/register/action | Iothub kaynak sağlayıcısı için abonelik kaydı ve Iothub kaynakların oluşturulmasını sağlar |
> | Eylem | Microsoft.Devices/register/action | Iothub kaynak sağlayıcısı için abonelik kaydı ve Iothub kaynakların oluşturulmasını sağlar |
> | Eylem | Microsoft.Devices/usages/Read | Abonelik bu sağlayıcı için kullanım ayrıntıları alın. |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.DevTestLab/labCenters/delete | Laboratuvar merkezleri silin. |
> | Eylem | Microsoft.DevTestLab/labCenters/read | Laboratuvar merkezleri okuyun. |
> | Eylem | Microsoft.DevTestLab/labCenters/write | Ekleyin veya Laboratuvar merkezleri değiştirin. |
> | Eylem | Microsoft.DevTestLab/labs/artifactSources/armTemplates/read | Azure resource manager şablonları okuyun. |
> | Eylem | Microsoft.DevTestLab/labs/artifactSources/artifacts/GenerateArmTemplate/action | Belirtilen yapı için bir ARM şablonu oluşturur, bir depolama hesabı için gereken dosyaları yükler ve oluşturulan yapı doğrular. |
> | Eylem | Microsoft.DevTestLab/labs/artifactSources/artifacts/read | Yapıları okuyun. |
> | Eylem | Microsoft.DevTestLab/labs/artifactSources/delete | Yapı kaynaklarını silin. |
> | Eylem | Microsoft.DevTestLab/labs/artifactSources/read | Yapı kaynaklarını okuyun. |
> | Eylem | Microsoft.DevTestLab/labs/artifactSources/write | Ekleyin veya Yapı kaynaklarını değiştirin. |
> | Eylem | Microsoft.DevTestLab/labs/ClaimAnyVm/action | Laboratuvar rastgele claimable sanal makinede talep. |
> | Eylem | Microsoft.DevTestLab/labs/costs/read | Maliyetleri okuyun. |
> | Eylem | Microsoft.DevTestLab/labs/costs/write | Ekleyin veya maliyetleri değiştirin. |
> | Eylem | Microsoft.DevTestLab/labs/CreateEnvironment/action | Sanal makineler bir laboratuar ortamında oluşturun. |
> | Eylem | Microsoft.DevTestLab/labs/customImages/delete | Özel resimler silin. |
> | Eylem | Microsoft.DevTestLab/labs/customImages/read | Özel resimler okuyun. |
> | Eylem | Microsoft.DevTestLab/labs/customImages/write | Ekleyin veya özel resimler değiştirin. |
> | Eylem | Microsoft.DevTestLab/labs/delete | Labs silin. |
> | Eylem | Microsoft.DevTestLab/labs/ExportResourceUsage/action | Laboratuvar kaynak kullanımı bir depolama hesabına dışa aktarır. |
> | Eylem | Microsoft.DevTestLab/labs/formulas/delete | Formülleri silin. |
> | Eylem | Microsoft.DevTestLab/labs/formulas/read | Formülleri okuyun. |
> | Eylem | Microsoft.DevTestLab/labs/formulas/write | Ekleyin veya formüller değiştirin. |
> | Eylem | Microsoft.DevTestLab/labs/galleryImages/read | Galeri görüntüleri okuyun. |
> | Eylem | Microsoft.DevTestLab/labs/GenerateUploadUri/action | Özel disk görüntülerini laboratuvara yüklemek için bir URI oluşturur. |
> | Eylem | Microsoft.DevTestLab/labs/ImportVirtualMachine/action | Bir sanal makine farklı bir laboratuvar alın. |
> | Eylem | Microsoft.DevTestLab/labs/ListVhds/action | Liste disk görüntülerini özel görüntü oluşturma için kullanılabilir. |
> | Eylem | Microsoft.DevTestLab/labs/notificationChannels/delete | Notificationchannels silin. |
> | Eylem | Microsoft.DevTestLab/labs/notificationChannels/Notify/action | Bildirim sağlanan kanala göndermek. |
> | Eylem | Microsoft.DevTestLab/labs/notificationChannels/read | Notificationchannels okuyun. |
> | Eylem | Microsoft.DevTestLab/labs/notificationChannels/write | Ekleyin veya notificationchannels değiştirin. |
> | Eylem | Microsoft.DevTestLab/labs/policySets/EvaluatePolicies/action | Laboratuvar ilkeyi değerlendirir. |
> | Eylem | Microsoft.DevTestLab/labs/policySets/policies/delete | İlkeleri silin. |
> | Eylem | Microsoft.DevTestLab/labs/policySets/policies/read | İlkeleri okuyun. |
> | Eylem | Microsoft.DevTestLab/labs/policySets/policies/write | Ekleyin veya ilkeleri değiştirin. |
> | Eylem | Microsoft.DevTestLab/labs/read | Labs okuyun. |
> | Eylem | Microsoft.DevTestLab/labs/schedules/delete | Zamanlamalar silin. |
> | Eylem | Microsoft.DevTestLab/labs/schedules/Execute/action | Bir zamanlama yürütün. |
> | Eylem | Microsoft.DevTestLab/labs/schedules/ListApplicable/action | Tüm geçerli zamanlamaları listeler |
> | Eylem | Microsoft.DevTestLab/labs/schedules/read | Zamanlamalar okuyun. |
> | Eylem | Microsoft.DevTestLab/labs/schedules/write | Ekleyin veya zamanlamalarını değiştirin. |
> | Eylem | Microsoft.DevTestLab/labs/serviceRunners/delete | Hizmet koşucular silin. |
> | Eylem | Microsoft.DevTestLab/labs/serviceRunners/read | Hizmet koşucular okuyun. |
> | Eylem | Microsoft.DevTestLab/labs/serviceRunners/write | Ekleyin veya hizmet koşucular değiştirin. |
> | Eylem | Microsoft.DevTestLab/labs/users/delete | Kullanıcı profillerini silin. |
> | Eylem | Microsoft.DevTestLab/labs/users/disks/Attach/action | Ekleme ve disk sanal makineye kira oluşturun. |
> | Eylem | Microsoft.DevTestLab/labs/users/disks/delete | Diskleri silin. |
> | Eylem | Microsoft.DevTestLab/labs/users/disks/Detach/action | Ayırma ve sanal makineye bağlı disk kira bölün. |
> | Eylem | Microsoft.DevTestLab/labs/users/disks/read | Diskleri okuyun. |
> | Eylem | Microsoft.DevTestLab/labs/users/disks/write | Ekleyin veya diskleri değiştirin. |
> | Eylem | Microsoft.DevTestLab/labs/users/environments/delete | Ortamlar silin. |
> | Eylem | Microsoft.DevTestLab/labs/users/environments/read | Ortamlar okuyun. |
> | Eylem | Microsoft.DevTestLab/labs/users/environments/write | Ekleyin veya ortamlar değiştirin. |
> | Eylem | Microsoft.DevTestLab/labs/users/read | Kullanıcı profillerini okuyun. |
> | Eylem | Microsoft.DevTestLab/labs/users/secrets/delete | Gizli silin. |
> | Eylem | Microsoft.DevTestLab/labs/users/secrets/read | Gizli okuyun. |
> | Eylem | Microsoft.DevTestLab/labs/users/secrets/write | Ekleyin veya gizli değiştirin. |
> | Eylem | Microsoft.DevTestLab/labs/users/serviceFabrics/delete | Hizmet dokuları silin. |
> | Eylem | Microsoft.DevTestLab/labs/users/serviceFabrics/ListApplicableSchedules/action | Tüm geçerli zamanlamaları listeler |
> | Eylem | Microsoft.DevTestLab/labs/users/serviceFabrics/read | Hizmet dokuları okuyun. |
> | Eylem | Microsoft.DevTestLab/labs/users/serviceFabrics/schedules/delete | Zamanlamalar silin. |
> | Eylem | Microsoft.DevTestLab/labs/users/serviceFabrics/schedules/Execute/action | Bir zamanlama yürütün. |
> | Eylem | Microsoft.DevTestLab/labs/users/serviceFabrics/schedules/read | Zamanlamalar okuyun. |
> | Eylem | Microsoft.DevTestLab/labs/users/serviceFabrics/schedules/write | Ekleyin veya zamanlamalarını değiştirin. |
> | Eylem | Microsoft.DevTestLab/labs/users/serviceFabrics/Start/action | Service fabric başlatın. |
> | Eylem | Microsoft.DevTestLab/labs/users/serviceFabrics/Stop/action | Service fabric Durdur |
> | Eylem | Microsoft.DevTestLab/labs/users/serviceFabrics/write | Ekleyin veya hizmet dokuları değiştirin. |
> | Eylem | Microsoft.DevTestLab/labs/users/write | Ekleyin veya kullanıcı profillerini değiştirin. |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/AddDataDisk/action | Yeni veya var olan veri diski sanal makineye Ekle. |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/ApplyArtifacts/action | Yapılar, sanal makine için geçerlidir. |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/Claim/action | Var olan bir sanal makine sahipliğini alın |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/delete | Sanal makineleri silin. |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/DetachDataDisk/action | Belirtilen sanal makinede diski kullanımdan çıkarın. |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/ListApplicableSchedules/action | Tüm geçerli zamanlamaları listeler |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/read | Sanal makineler okuyun. |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/Redeploy/action | Bir sanal makineyi yeniden dağıtın |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/Resize/action | Sanal makine yeniden boyutlandırın. |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/Restart/action | Bir sanal makineyi yeniden başlatın. |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/schedules/delete | Zamanlamalar silin. |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/schedules/Execute/action | Bir zamanlama yürütün. |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/schedules/read | Zamanlamalar okuyun. |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/schedules/write | Ekleyin veya zamanlamalarını değiştirin. |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/Start/action | Bir sanal makineyi başlatın. |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/Stop/action | Bir sanal makineyi durdurma |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/TransferDisks/action | Sanal makine veri diski kendinize sahipliğini aktarma |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/UnClaim/action | Varolan bir sanal makinenin sahipliğini sürüm |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/write | Sanal makineleri ekleyin veya değiştirin. |
> | Eylem | Microsoft.DevTestLab/labs/virtualNetworks/delete | Sanal ağlar silin. |
> | Eylem | Microsoft.DevTestLab/labs/virtualNetworks/read | Sanal ağlar okuyun. |
> | Eylem | Microsoft.DevTestLab/labs/virtualNetworks/write | Ekleyin veya sanal ağları değiştirin. |
> | Eylem | Microsoft.DevTestLab/labs/vmPools/delete | Sanal makine havuzları silin. |
> | Eylem | Microsoft.DevTestLab/labs/vmPools/read | Sanal makine havuzları okuyun. |
> | Eylem | Microsoft.DevTestLab/labs/vmPools/write | Ekleyin veya sanal makine havuzları değiştirin. |
> | Eylem | Microsoft.DevTestLab/labs/write | Ekleyin veya labs değiştirin. |
> | Eylem | Microsoft.DevTestLab/locations/operations/read | Okuma işlemleri. |
> | Eylem | Microsoft.DevTestLab/register/action | Aboneliği kaydeder |
> | Eylem | Microsoft.DevTestLab/schedules/delete | Zamanlamalar silin. |
> | Eylem | Microsoft.DevTestLab/schedules/Execute/action | Bir zamanlama yürütün. |
> | Eylem | Microsoft.DevTestLab/schedules/read | Zamanlamalar okuyun. |
> | Eylem | Microsoft.DevTestLab/schedules/Retarget/action | Bir zamanlama ait hedef kaynak kimliği güncelleştirir |
> | Eylem | Microsoft.DevTestLab/schedules/write | Ekleyin veya zamanlamalarını değiştirin. |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.DocumentDB/databaseAccountNames/read | Ad kullanılabilirliğini denetler. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/changeResourceGroup/action | Veritabanı hesabı, kaynak grubu Değiştir |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/databases/collections/metricDefinitions/read | Koleksiyon ölçüm tanımlarını okur. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/databases/collections/metrics/read | Koleksiyon ölçümleri okur. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/databases/collections/partitionKeyRangeId/metrics/read | Veritabanı hesabı bölüm anahtar düzeyindeki ölçümlerini okuma |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/databases/collections/partitions/metrics/read | Veritabanı hesabı bölüm düzeyindeki ölçümlerini okuma |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/databases/collections/partitions/read | Bir koleksiyondaki veritabanı hesabı bölümleri okuyun |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/databases/collections/partitions/usages/read | Veritabanı hesabı bölüm düzeyi kullanımları okuma |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/databases/collections/usages/read | Koleksiyon kullanımları okur. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/databases/metricDefinitions/read | Ölçüm tanımlarını veritabanını okur |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/databases/metrics/read | Veritabanı ölçümleri okur. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/databases/usages/read | Veritabanı kullanımları okur. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/delete | Veritabanı hesaplarını siler. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/failoverPriorityChange/action | Veritabanı hesabı bölgelerinin yük devretme önceliklerini değiştirin. Bu el ile yük devretme işlemi gerçekleştirmek için kullanılır |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/listConnectionStrings/action | Bağlantı dizeleri için bir veritabanı hesabı edinin |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/listKeys/action | Veritabanı hesabı listesi anahtarları |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/metricDefinitions/read | Veritabanı hesabı ölçümleri tanımları okur. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/metrics/read | Veritabanı hesabı ölçümleri okur. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/offlineRegion/action | Çevrimdışı bir veritabanı hesabı bölgesi.  |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/onlineRegion/action | Çevrimiçi bir veritabanı hesabı bölgesi. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/operationResults/read | Zaman uyumsuz işlemin durumunu okuma |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/percentile/metrics/read | Gecikme ölçümleri |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/percentile/read | Yüzdebirlik değeri çoğaltma gecikmeleri, okuma |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/percentile/sourceRegion/targetRegion/metrics/read | Belirli bir kaynak ve hedef bölge gecikme ölçümleri |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/percentile/targetRegion/metrics/read | Belirli hedef bölge için gecikme ölçümleri |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/providers/Microsoft.Insights/diagnosticSettings/read | Kaynak için tanılama ayarını alır |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/providers/Microsoft.Insights/diagnosticSettings/write | Kaynak için tanılama ayarını oluşturur veya güncelleştirir |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/providers/Microsoft.Insights/logDefinitions/read | Veritabanı hesabı için kullanılabilir günlük catageries alır |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/providers/Microsoft.Insights/metricDefinitions/read | Veritabanı hesabı için kullanılabilir ölçümleri alır |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/read | Veritabanı hesabı okur. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/readonlykeys/action | Veritabanı hesabı readonly anahtarları okur. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/readonlykeys/read | Veritabanı hesabı readonly anahtarları okur. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/regenerateKey/action | Veritabanı hesabı anahtarlarını döndürün |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/region/databases/collections/metrics/read | Bölgesel koleksiyonu ölçümleri okur. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/region/databases/collections/partitionKeyRangeId/metrics/read | Bölgesel veritabanı hesabı bölüm anahtar düzeyindeki ölçümlerini okuma |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/region/databases/collections/partitions/metrics/read | Bölgesel veritabanı hesabı bölüm düzeyindeki ölçümlerini okuma |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/region/databases/collections/partitions/read | Bir koleksiyondaki bölgesel veritabanı hesabı bölümleri okuyun |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/region/metrics/read | Bölge ve veritabanı hesabı ölçümleri okur. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/usages/read | Veritabanı hesabı kullanımları okur. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/write | Bir veritabanı hesaplarını güncelleştirin. |
> | Eylem | Microsoft.DocumentDB/locations/deleteVirtualNetworkOrSubnets/action | Microsoft.DocumentDB VirtualNetwork veya alt ağı siliniyor olduğunu bildirir. |
> | Eylem | Microsoft.DocumentDB/locations/deleteVirtualNetworkOrSubnets/operationResults/read | Zaman uyumsuz işlem deleteVirtualNetworkOrSubnets durumunu okuma |
> | Eylem | Microsoft.DocumentDB/operationResults/read | Zaman uyumsuz işlemin durumunu okuma |
> | Eylem | Microsoft.DocumentDB/operations/read | Microsoft DocumentDB için kullanılabilir okuma işlemleri  |
> | Eylem | Microsoft.DocumentDB/register/action |  Aboneliği için Microsoft DocumentDB kaynak sağlayıcısı kaydetme |

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.DomainRegistration/checkDomainAvailability/Action | Bir etki alanı satın almak için uygun olup olmadığını denetleyin |
> | Eylem | Microsoft.DomainRegistration/domains/Delete | Varolan bir etki alanını silin. |
> | Eylem | Microsoft.DomainRegistration/domains/domainownershipidentifiers/Delete | Sahipliği tanımlayıcısını sil |
> | Eylem | Microsoft.DomainRegistration/domains/domainownershipidentifiers/Read | Liste sahipliği tanımlayıcıları |
> | Eylem | Microsoft.DomainRegistration/domains/domainownershipidentifiers/Read | Sahipliği tanımlayıcısını alın |
> | Eylem | Microsoft.DomainRegistration/domains/domainownershipidentifiers/Write | Tanımlayıcı güncelle |
> | Eylem | Microsoft.DomainRegistration/domains/operationresults/Read | Bir etki alanı işlemi Al |
> | Eylem | Microsoft.DomainRegistration/domains/Read | Etki alanlarının listesini alın |
> | Eylem | Microsoft.DomainRegistration/domains/Read | Etki alanı alma |
> | Eylem | Microsoft.DomainRegistration/domains/renew/Action | Mevcut bir etki alanına yenileyin. |
> | Eylem | Microsoft.DomainRegistration/domains/Write | Yeni bir etki alanı eklemek veya mevcut bir güncelleştirme |
> | Eylem | Microsoft.DomainRegistration/generateSsoRequest/Action | Etki alanı denetim Merkezi'nde oturum için bir istek oluşturun. |
> | Eylem | Microsoft.DomainRegistration/listDomainRecommendations/Action | Anahtar sözcüklere dayalı liste etki alanı önerileri Al |
> | Eylem | Microsoft.DomainRegistration/operations/Read | Uygulama hizmeti etki alanı kaydı tüm işlemler listesi |
> | Eylem | Microsoft.DomainRegistration/register/action | Aboneliği için Microsoft Domains kaynak sağlayıcısı kaydetme |
> | Eylem | Microsoft.DomainRegistration/topLevelDomains/listAgreements/Action | Anlaşma eylem listesi |
> | Eylem | Microsoft.DomainRegistration/topLevelDomains/Read | TopLevel etki alanları Al |
> | Eylem | Microsoft.DomainRegistration/topLevelDomains/Read | TopLevel etki alanı alma |
> | Eylem | Microsoft.DomainRegistration/validateDomainRegistrationInformation/Action | Etki alanı satın alma nesnesi gönderme olmadan doğrula |

## <a name="microsoftdynamicslcs"></a>Microsoft.DynamicsLcs

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.DynamicsLcs/lcsprojects/clouddeployments/read | Microsoft Dynamics AX 2012 R3 değerlendirme dağıtımları bir kullanıcıya ait bir Microsoft Dynamics yaşam döngüsü Hizmetleri Proje görüntüler. |
> | Eylem | Microsoft.DynamicsLcs/lcsprojects/clouddeployments/write | Microsoft Dynamics AX 2012 R3 değerlendirme dağıtım bir kullanıcıya ait bir Microsoft Dynamics yaşam döngüsü Hizmetleri projesi oluşturun. Azure Yönetim Portalı'ndan dağıtımları yönetilebilir. |
> | Eylem | Microsoft.DynamicsLcs/lcsprojects/connectors/read | Microsoft Dynamics yaşam döngüsü Hizmetleri projeye ait okuma bağlayıcılar |
> | Eylem | Microsoft.DynamicsLcs/lcsprojects/connectors/write | Oluşturma ve Microsoft Dynamics yaşam döngüsü Hizmetleri projeye ait bağlayıcılar güncelleştirme |
> | Eylem | Microsoft.DynamicsLcs/lcsprojects/delete | Kullanıcıya ait Microsoft Dynamics yaşam döngüsü Hizmetleri projeleri sil |
> | Eylem | Microsoft.DynamicsLcs/lcsprojects/read | Bir kullanıcıya ait görüntü Microsoft Dynamics yaşam döngüsü Hizmetleri projeleri |
> | Eylem | Microsoft.DynamicsLcs/lcsprojects/write | Oluşturun ve kullanıcıya ait Microsoft Dynamics yaşam döngüsü Hizmetleri projeleri güncelleştirin. Yalnızca ad ve açıklama özellikleri güncelleştirilebilir. Oluşturulduktan sonra projeyle ilişkili konumu ve abonelik güncelleştirilemiyor |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.EventGrid/eventSubscriptions/delete | Bir eventSubscription Sil |
> | Eylem | Microsoft.EventGrid/eventSubscriptions/getFullUrl/action | Olay aboneliği için tam URL'sini alma |
> | Eylem | Microsoft.EventGrid/eventSubscriptions/providers/Microsoft.Insights/diagnosticSettings/read | Olay abonelikleri tanılama ayarını alır |
> | Eylem | Microsoft.EventGrid/eventSubscriptions/providers/Microsoft.Insights/diagnosticSettings/write | Oluşturur veya olay abonelikleri tanılama ayarını güncelleştirir |
> | Eylem | Microsoft.EventGrid/eventSubscriptions/providers/Microsoft.Insights/metricDefinitions/read | EventSubscriptions için kullanılabilir ölçümleri alır |
> | Eylem | Microsoft.EventGrid/eventSubscriptions/read | Bir eventSubscription okuma |
> | Eylem | Microsoft.EventGrid/eventSubscriptions/write | Bir eventSubscription güncelle |
> | Eylem | Microsoft.EventGrid/extensionTopics/providers/Microsoft.Insights/diagnosticSettings/read | Konular tanılama ayarını alır |
> | Eylem | Microsoft.EventGrid/extensionTopics/providers/Microsoft.Insights/diagnosticSettings/write | Oluşturur veya konuları tanılama ayarını güncelleştirir |
> | Eylem | Microsoft.EventGrid/extensionTopics/providers/Microsoft.Insights/metricDefinitions/read | Konular için kullanılabilir ölçümleri alır |
> | Eylem | Microsoft.EventGrid/register/action | EventSubscription EventGrid kaynak sağlayıcısı için kaydeder ve olay kılavuz abonelikleri oluşturulmasını sağlar. |
> | Eylem | Microsoft.EventGrid/topics/delete | Bir konu Sil |
> | Eylem | Microsoft.EventGrid/topics/listKeys/action | Konu listesi tuşları |
> | Eylem | Microsoft.EventGrid/topics/providers/Microsoft.Insights/diagnosticSettings/read | Konular tanılama ayarını alır |
> | Eylem | Microsoft.EventGrid/topics/providers/Microsoft.Insights/diagnosticSettings/write | Oluşturur veya konuları tanılama ayarını güncelleştirir |
> | Eylem | Microsoft.EventGrid/topics/providers/Microsoft.Insights/metricDefinitions/read | Konular için kullanılabilir ölçümleri alır |
> | Eylem | Microsoft.EventGrid/topics/read | Bir konuyu okuyun |
> | Eylem | Microsoft.EventGrid/topics/regenerateKey/action | Konu için yeniden oluşturma anahtarı |
> | Eylem | Microsoft.EventGrid/topics/write | Bir konu güncelle |

## <a name="microsofteventhub"></a>Microsoft.EventHub

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.EventHub/checkNameAvailability/action | İlgili abonelikte ad alanının kullanılabilirliğini denetler. |
> | Eylem | Microsoft.EventHub/checkNamespaceAvailability/action | İlgili abonelikte ad alanının kullanılabilirliğini denetler. Bu API kullanım Lütfen CheckNameAvailabiltiy kullanın. |
> | Eylem | Microsoft.EventHub/clusters/providers/Microsoft.Insights/metricDefinitions/read | Küme ölçümlerini kaynak açıklamaları listesini al |
> | Eylem | Microsoft.EventHub/clusters/read | Küme Kaynağı Açıklamasını alır |
> | Eylem | Microsoft.EventHub/clusters/write | Küme Kaynağı Açıklamasını alır |
> | Eylem | Microsoft.EventHub/namespaces/authorizationRules/action | Güncelleştirmeleri Namespace yetkilendirme kuralı. Depricated API'dir. Namespace yetkilendirme kuralı yerine güncelleştirmek için lütfen PUT çağrı kullanın... API sürümü 2017-04-01 üzerinde bu işlem desteklenmiyor. |
> | Eylem | Microsoft.EventHub/namespaces/authorizationRules/delete | Namespace yetkilendirme kuralını silin. Namespace yetkilendirme varsayılan kural silinemiyor.  |
> | Eylem | Microsoft.EventHub/namespaces/authorizationRules/listkeys/action | Ad Alanı için Bağlantı Dizesini alın |
> | Eylem | Microsoft.EventHub/namespaces/authorizationRules/read | Ad Alanı Yetkilendirme Kuralları açıklamasının listesini alır. |
> | Eylem | Microsoft.EventHub/namespaces/authorizationRules/regenerateKeys/action | Kaynağın Birincil veya İkincil anahtarını yeniden oluşturun |
> | Eylem | Microsoft.EventHub/namespaces/authorizationRules/write | Namespace düzeyi yetkilendirme kuralları oluşturun ve özelliklerini güncelleştirin. Yetkilendirme kuralları erişim hakları, birincil ve ikincil anahtarları güncelleştirilebilir. |
> | Eylem | Microsoft.EventHub/namespaces/Delete | Ad Alanı Kaynağını silin |
> | Eylem | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/authorizationRules/listkeys/action | Olağanüstü Durum Kurtarma birincil ad alanı için yetkilendirme kuralları anahtarlarını alır |
> | Eylem | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/authorizationRules/read | Olağanüstü Durum Kurtarma Birincil Ad Alanının Yetkilendirme Kurallarını Al |
> | Eylem | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/breakPairing/action | Olağanüstü Durum Kurtarma'yı devre dışı bırakır ve birincil ad alanındaki değişiklikleri ikincil ad alanına çoğaltmayı durdurur. |
> | Eylem | Microsoft.EventHub/namespaces/disasterrecoveryconfigs/checkNameAvailability/action | İlgili abonelikte ad alanının diğer ad kullanılabilirliğini denetler. |
> | Eylem | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/delete | Ad alanıyla ilişkili olağanüstü durum kurtarma yapılandırması siler. Bu işlem yalnızca birincil ad alanı çağrılabilir. |
> | Eylem | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/failover/action | Bir GEO DR yük devretmesini çağırır ve ad alanı diğer adını ikincil ad alanına işaret edecek şekilde yeniden yapılandırır. |
> | Eylem | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/read | Ad alanıyla ilişkili Olağanüstü Durum Kurtarma yapılandırmasını alır. |
> | Eylem | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/write | Ad alanıyla ilişkili Olağanüstü Durum Kurtarma yapılandırmasını oluşturur veya güncelleştirir. |
> | Eylem | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/action | EventHub güncelleştirmek için güncelleştirme işlemi. API sürümü 2017-04-01 üzerinde bu işlem desteklenmiyor. Yetkilendirme kuralları. Yetkilendirme kuralı güncelleştirmek için lütfen PUT çağrı kullanın. |
> | Eylem | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/delete | EventHub yetkilendirme kuralları silmek için işlemi |
> | Eylem | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/listkeys/action | EventHub bağlantı dizesi alma |
> | Eylem | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/read |  EventHub yetkilendirme kuralları listesini al |
> | Eylem | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/regenerateKeys/action | Kaynağın Birincil veya İkincil anahtarını yeniden oluşturun |
> | Eylem | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/write | EventHub yetkilendirme kuralları oluşturun ve özelliklerini güncelleştirin. Yetkilendirme kuralları erişim hakları güncelleştirilebilir. |
> | Eylem | Microsoft.EventHub/namespaces/eventHubs/consumergroups/Delete | ConsumerGroup kaynağı silme işlemi |
> | Eylem | Microsoft.EventHub/namespaces/eventHubs/consumergroups/read | ConsumerGroup kaynak açıklamaları listesini al |
> | Eylem | Microsoft.EventHub/namespaces/eventHubs/consumergroups/write | Oluşturma veya güncelleştirme ConsumerGroup özellikleri. |
> | Eylem | Microsoft.EventHub/namespaces/eventhubs/Delete | EventHub kaynağı silme işlemi |
> | Eylem | Microsoft.EventHub/namespaces/eventhubs/read | EventHub kaynak açıklamaları listesini al |
> | Eylem | Microsoft.EventHub/namespaces/eventhubs/write | Oluşturma veya güncelleştirme EventHub özellikleri. |
> | Eylem | Microsoft.EventHub/namespaces/messagingPlan/read | Mesajlaşma planı için bir ad alır.<br>Bu API kullanım dışıdır.<br>MessagingPlan kaynak kullanıma sunulan özellikler (üst) sonraki API sürümleri Namespace kaynağın taşınması...<br>API sürümü 2017-04-01 üzerinde bu işlem desteklenmiyor. |
> | Eylem | Microsoft.EventHub/namespaces/messagingPlan/write | Mesajlaşma Plan için bir ad alanı güncelleştirir.<br>Bu API kullanım dışıdır.<br>MessagingPlan kaynak kullanıma sunulan özellikler (üst) sonraki API sürümleri Namespace kaynağın taşınması...<br>API sürümü 2017-04-01 üzerinde bu işlem desteklenmiyor. |
> | Eylem | Microsoft.EventHub/namespaces/operationresults/read | Ad alanı işleminin durumunu al |
> | Eylem | Microsoft.EventHub/namespaces/providers/Microsoft.Insights/diagnosticSettings/read | Namespace tanılama ayarları kaynak açıklamaları listesini al |
> | Eylem | Microsoft.EventHub/namespaces/providers/Microsoft.Insights/diagnosticSettings/write | Namespace tanılama ayarları kaynak açıklamaları listesini al |
> | Eylem | Microsoft.EventHub/namespaces/providers/Microsoft.Insights/logDefinitions/read | Namespace günlükleri kaynak açıklamaları listesini al |
> | Eylem | Microsoft.EventHub/namespaces/providers/Microsoft.Insights/metricDefinitions/read | Namespace ölçümleri kaynak açıklamaları listesini al |
> | Eylem | Microsoft.EventHub/namespaces/read | Ad Alanı Kaynak Açıklamasının listesini alır |
> | Eylem | Microsoft.EventHub/namespaces/removeAcsNamepsace/action | ACS ad alanını kaldırın |
> | Eylem | Microsoft.EventHub/namespaces/write | Namespace kaynak oluşturun ve özelliklerini güncelleştirin. Etiketleri ve Namespace kapasitesini hangi güncelleştirilebilir özelliklerdir. |
> | Eylem | Microsoft.EventHub/operations/read | İşlemleri Al |
> | Eylem | Microsoft.EventHub/register/action | Aboneliği EventHub kaynak sağlayıcısı için kaydeder ve EventHub kaynaklarının oluşturulmasını sağlar |
> | Eylem | Microsoft.EventHub/sku/read | Sku kaynak açıklamaları listesini al |
> | Eylem | Microsoft.EventHub/sku/regions/read | SkuRegions kaynak açıklamaları listesini al |
> | Eylem | Microsoft.EventHub/unregister/action | EventHub Kaynak Sağlayıcısını kaydet |

## <a name="microsoftfeatures"></a>Microsoft.Features

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Features/features/read | Bir aboneliğin özelliklerini alır. |
> | Eylem | Microsoft.Features/operations/read | İşlem listesini alır. |
> | Eylem | Microsoft.Features/providers/features/read | Belirli bir kaynak sağlayıcısındaki bir abonelik özelliğini alır. |
> | Eylem | Microsoft.Features/providers/features/register/action | Belirli bir kaynak sağlayıcısındaki bir abonelik özelliğini kaydeder. |
> | Eylem | Microsoft.Features/providers/features/unregister/action | Belirtilen bir kaynak sağlayıcısındaki bir abonelik özelliğinin kaydını siler. |
> | Eylem | Microsoft.Features/register/action | Bir aboneliğin özelliğini kaydeder. |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.HDInsight/clusters/changerdpsetting/action | Hdınsight kümesi RDP ayarını değiştirme |
> | Eylem | Microsoft.HDInsight/clusters/configurations/action | Hdınsight küme yapılandırmasını güncelleştirme |
> | Eylem | Microsoft.HDInsight/clusters/configurations/read | Hdınsight küme yapılandırmalarını alma |
> | Eylem | Microsoft.HDInsight/clusters/delete | Bir Hdınsight kümesi Sil |
> | Eylem | Microsoft.HDInsight/clusters/providers/Microsoft.Insights/diagnosticSettings/read | Hdınsight kümesi kaynağın tanılama ayarını alır |
> | Eylem | Microsoft.HDInsight/clusters/providers/Microsoft.Insights/diagnosticSettings/write | Oluşturur veya Hdınsight kümesi kaynağın tanılama ayarını güncelleştirir |
> | Eylem | Microsoft.HDInsight/clusters/providers/Microsoft.Insights/metricDefinitions/read | Hdınsight kümesi için kullanılabilir ölçümleri alır |
> | Eylem | Microsoft.HDInsight/clusters/read | Hdınsight kümesi hakkında bilgi almak |
> | Eylem | Microsoft.HDInsight/clusters/roles/resize/action | Bir Hdınsight kümesi yeniden boyutlandırma |
> | Eylem | Microsoft.HDInsight/clusters/write | Hdınsight kümesi güncelle |
> | Eylem | Microsoft.HDInsight/locations/capabilities/read | Abonelik özelliklerini al |
> | Eylem | Microsoft.HDInsight/locations/checkNameAvailability/read | Ad Kullanılabilirliğini Denetle |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.ImportExport/jobs/delete | Var olan bir işi siler. |
> | Eylem | Microsoft.ImportExport/jobs/listBitLockerKeys/action | Belirtilen iş için BitLocker anahtarları alır. |
> | Eylem | Microsoft.ImportExport/jobs/read | Belirtilen iş için özellikleri alır veya işlerin listesini döndürür. |
> | Eylem | Microsoft.ImportExport/jobs/write | Belirtilen parametrelerle bir işi oluşturur veya özellikleri veya etiketleri belirtilen iş için güncelleştirin. |
> | Eylem | Microsoft.ImportExport/locations/read | Belirtilen konum için özellikleri alır veya konumların listesini döndürür. |
> | Eylem | Microsoft.ImportExport/register/action | İçeri/dışarı aktarma kaynak sağlayıcısı için aboneliği kaydeder ve içeri/dışarı aktarma işleri oluşturulmasını sağlar. |

## <a name="microsoftinsights"></a>Microsoft.Insights

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Insights/ActionGroups/Delete | Bir eylem grubu siliniyor |
> | Eylem | Microsoft.Insights/ActionGroups/Read | Bir eylem grubu okunuyor |
> | Eylem | Microsoft.Insights/ActionGroups/Write | Bir eylem grubu yazılıyor |
> | Eylem | Microsoft.Insights/ActivityLogAlerts/Activated/Action | Etkinlik Günlüğü Uyarısı tetiklendi |
> | Eylem | Microsoft.Insights/ActivityLogAlerts/Delete | Bir etkinlik günlüğü uyarısı siliniyor |
> | Eylem | Microsoft.Insights/ActivityLogAlerts/Read | Bir etkinlik günlüğü uyarısı okunuyor |
> | Eylem | Microsoft.Insights/ActivityLogAlerts/Write | Bir etkinlik günlüğü uyarısı okunuyor |
> | Eylem | Microsoft.Insights/AlertRules/Activated/Action | Uyarı Kuralı etkinleştirildi |
> | Eylem | Microsoft.Insights/AlertRules/Delete | Uyarı kuralı yapılandırması siliniyor |
> | Eylem | Microsoft.Insights/AlertRules/Incidents/Read | Uyarı kuralı olayı yapılandırması okunuyor |
> | Eylem | Microsoft.Insights/AlertRules/Read | Uyarı kuralı yapılandırması okunuyor |
> | Eylem | Microsoft.Insights/AlertRules/Resolved/Action | Uyarı Kuralı çözümlendi |
> | Eylem | Microsoft.Insights/AlertRules/Throttled/Action | Uyarı kuralı kısıtlanmış |
> | Eylem | Microsoft.Insights/AlertRules/Write | Bir uyarı kuralı yapılandırmasına yazılıyor |
> | Eylem | Microsoft.Insights/AutoscaleSettings/Delete | Otomatik ölçek ayarı yapılandırması siliniyor |
> | Eylem | Microsoft.Insights/AutoscaleSettings/providers/Microsoft.Insights/MetricDefinitions/Read | Ölçüm tanımlarını oku |
> | Eylem | Microsoft.Insights/AutoscaleSettings/Read | Otomatik ölçek ayarı yapılandırması okunuyor |
> | Eylem | Microsoft.Insights/AutoscaleSettings/Scaledown/Action | Otomatik Ölçek ölçeği azaltma işlemi |
> | Eylem | Microsoft.Insights/AutoscaleSettings/ScaledownResult/Action | Otomatik Ölçek sonuç ölçeği azaltma işlemi |
> | Eylem | Microsoft.Insights/AutoscaleSettings/Scaleup/Action | Otomatik Ölçek ölçeği artırma işlemi |
> | Eylem | Microsoft.Insights/AutoscaleSettings/ScaleupResult/Action | Otomatik ölçek sonuç ölçeği artırma işlemi |
> | Eylem | Microsoft.Insights/AutoscaleSettings/Write | Otomatik ölçek ayarı yapılandırması yazılıyor |
> | Eylem | Microsoft.Insights/Components/AnalyticsItems/Delete | Bir Application Insights analitik öğesini silme |
> | Eylem | Microsoft.Insights/Components/AnalyticsItems/Read | Bir Application Insights analitik öğesini okuma |
> | Eylem | Microsoft.Insights/Components/AnalyticsItems/Write | Bir Application Insights analitik öğesinin yazma |
> | Eylem | Microsoft.Insights/Components/AnalyticsTables/Action | Application Insights analitik tablosu eylemi |
> | Eylem | Microsoft.Insights/Components/AnalyticsTables/Delete | Bir Application Insights analitik tablo şemasını silme |
> | Eylem | Microsoft.Insights/Components/AnalyticsTables/Read | Bir Application Insights analitik tablo şemasını okuma |
> | Eylem | Microsoft.Insights/Components/AnalyticsTables/Write | Bir Application Insights analitik tablo şemasını yazma |
> | Eylem | Microsoft.Insights/Components/Annotations/Delete | Bir Application Insights ek açıklamasını silme |
> | Eylem | Microsoft.Insights/Components/Annotations/Read | Bir Application Insights ek açıklamasını okuma |
> | Eylem | Microsoft.Insights/Components/Annotations/Write | Bir Application Insights ek açıklamasını yazma |
> | Eylem | Microsoft.Insights/Components/Api/Read | Application Insights bileşen verileri API'sini okuma |
> | Eylem | Microsoft.Insights/Components/ApiKeys/Action | Bir Application Insights API anahtarını oluşturma |
> | Eylem | Microsoft.Insights/Components/ApiKeys/Delete | Bir Application Insights API anahtarını silme |
> | Eylem | Microsoft.Insights/Components/ApiKeys/Read | Bir Application Insights API anahtarını okuma |
> | Eylem | Microsoft.Insights/Components/BillingPlanForComponent/Read | Application Insights bileşeni için bir faturalama planını okuma |
> | Eylem | Microsoft.Insights/Components/CurrentBillingFeatures/Read | Application Insights bileşeninin geçerli faturalama özelliklerini okuma |
> | Eylem | Microsoft.Insights/Components/CurrentBillingFeatures/Write | Application Insights bileşeninin geçerli faturalama özelliklerini yazma |
> | Eylem | Microsoft.Insights/Components/DefaultWorkItemConfig/Read | Application Insights varsayılan ALM tümleştirmesinin yapılandırmasını okuma |
> | Eylem | Microsoft.Insights/Components/Delete | Bir Application Insights bileşen yapılandırmasını silme |
> | Eylem | Microsoft.Insights/Components/Events/Read | OData sorgu biçimini kullanarak Application Insights'tan günlük al |
> | Eylem | Microsoft.Insights/Components/ExportConfiguration/Action | Application Insights dışa aktarım ayarları eylemi |
> | Eylem | Microsoft.Insights/Components/ExportConfiguration/Delete | Application Insights dışa aktarma ayarlarını silme |
> | Eylem | Microsoft.Insights/Components/ExportConfiguration/Read | Application Insights dışa aktarma ayarlarını okuma |
> | Eylem | Microsoft.Insights/Components/ExportConfiguration/Write | Application Insights dışa aktarma ayarlarını yazma |
> | Eylem | Microsoft.Insights/Components/ExtendQueries/Read | Application Insights bileşeninin genişletilmiş sorgu sonuçlarını okuma |
> | Eylem | Microsoft.Insights/Components/Favorites/Delete | Application Insights sık kullanılanını silme |
> | Eylem | Microsoft.Insights/Components/Favorites/Read | Application Insights sık kullanılanını okuma |
> | Eylem | Microsoft.Insights/Components/Favorites/Write | Application Insights sık kullanılanını yazma |
> | Eylem | Microsoft.Insights/Components/FeatureCapabilities/Read | Application Insights bileşeni özellik yeteneklerini okuma |
> | Eylem | Microsoft.Insights/Components/GetAvailableBillingFeatures/Read | Application Insights bileşeni kullanılabilir faturalama özelliklerini okuma |
> | Eylem | Microsoft.Insights/Components/GetToken/Read | Bir Application Insights bileşeni belirteci |
> | Eylem | Microsoft.Insights/Components/MetricDefinitions/Read | Application Insights bileşen ölçüm tanımlarını okuma |
> | Eylem | Microsoft.Insights/Components/Metrics/Read | Application Insights bileşen ölçümlerini okuma |
> | Eylem | Microsoft.Insights/Components/Move/Action | Bir Application Insights Bileşenini başka bir kaynak grubu veya aboneliğe taşıyın |
> | Eylem | Microsoft.Insights/Components/MyAnalyticsItems/Delete | Bir Application Insights kişisel analitik öğesini silme |
> | Eylem | Microsoft.Insights/Components/MyAnalyticsItems/Read | Bir Application Insights kişisel analitik öğesini okuma |
> | Eylem | Microsoft.Insights/Components/MyAnalyticsItems/Write | Bir Application Insights kişisel analitik öğesini yazma |
> | Eylem | Microsoft.Insights/Components/MyFavorites/Read | Bir Application Insights kişisel sık kullanılanını okuma |
> | Eylem | Microsoft.Insights/Components/Operations/Read | Application Insights'ta uzun süren işlemlerin durumunu al |
> | Eylem | Microsoft.Insights/Components/PricingPlans/Read | Bir Application Insights bileşen fiyatlandırma planını okuma |
> | Eylem | Microsoft.Insights/Components/PricingPlans/Write | Bir Application Insights bileşen fiyatlandırma planını yazma |
> | Eylem | Microsoft.Insights/Components/ProactiveDetectionConfigs/Read | Bir Application Insights proaktif algılama yapılandırmasını okuma |
> | Eylem | Microsoft.Insights/Components/ProactiveDetectionConfigs/Write | Bir Application Insights proaktif algılama yapılandırmasını yazma |
> | Eylem | Microsoft.Insights/Components/providers/Microsoft.Insights/MetricDefinitions/Read | Ölçüm tanımlarını oku |
> | Eylem | Microsoft.Insights/Components/Purge/Action | Application Insights verilerini temizleme |
> | Eylem | Microsoft.Insights/Components/Query/Read | Application Insights günlüklerine karşı sorgu çalıştır |
> | Eylem | Microsoft.Insights/Components/QuotaStatus/Read | Application Insights bileşen kotasının durumunu okuma |
> | Eylem | Microsoft.Insights/Components/Read | Bir Application Insights bileşen yapılandırmasını okuma |
> | Eylem | Microsoft.Insights/Components/SyntheticMonitorLocations/Read | Application Insights web testi konumlarını okuma |
> | Eylem | Microsoft.Insights/Components/Webtests/Read | Bir web testi yapılandırmasını okuma |
> | Eylem | Microsoft.Insights/Components/WorkItemConfigs/Delete | Bir Application Insights ALM tümleştirmesinin yapılandırmasını silme |
> | Eylem | Microsoft.Insights/Components/WorkItemConfigs/Read | Application Insights ALM tümleştirme yapılandırması |
> | Eylem | Microsoft.Insights/Components/WorkItemConfigs/Write | Bir Application Insights ALM tümleştirmesinin yapılandırmasını yazma |
> | Eylem | Microsoft.Insights/Components/Write | Bir Application Insights bileşen yapılandırmasına yazma |
> | Eylem | Microsoft.Insights/DiagnosticSettings/Delete | Tanılama ayarları yapılandırmasını silme |
> | Eylem | Microsoft.Insights/DiagnosticSettings/Read | Tanılama ayarları yapılandırmasını okuma |
> | Eylem | Microsoft.Insights/DiagnosticSettings/Write | Tanılama ayarları yapılandırmasına yazma |
> | Eylem | Microsoft.Insights/EventCategories/Read | Bir etkinlik kategorisi okunuyor |
> | Eylem | Microsoft.Insights/eventtypes/digestevents/Read | Yönetim olayı türü özetini oku |
> | Eylem | Microsoft.Insights/eventtypes/values/Read | Yönetim olayı tür değerlerini oku |
> | Eylem | Microsoft.Insights/ExtendedDiagnosticSettings/Delete | Genişletilmiş tanı ayarları yapılandırmasını silme |
> | Eylem | Microsoft.Insights/ExtendedDiagnosticSettings/Read | Genişletilmiş tanı ayarları yapılandırmasını okuma |
> | Eylem | Microsoft.Insights/ExtendedDiagnosticSettings/Write | Genişletilmiş tanı ayarları yapılandırmasına yazma |
> | Eylem | Microsoft.Insights/ListMigrationDate/Action | Abonelik geçiş tarihini geri alın |
> | Eylem | Microsoft.Insights/ListMigrationDate/Read | Abonelik geçiş tarihini geri alın |
> | Eylem | Microsoft.Insights/LogDefinitions/Read | Günlük tanımlarını oku |
> | Eylem | Microsoft.Insights/LogProfiles/Delete | Günlük profilleri yapılandırmasını silme |
> | Eylem | Microsoft.Insights/LogProfiles/Read | Günlük profillerini okuma |
> | Eylem | Microsoft.Insights/LogProfiles/Write | Bir günlük profili yapılandırmasına yazma |
> | Eylem | Microsoft.Insights/MetricAlerts/Delete | Bir ölçüm uyarısı siliniyor |
> | Eylem | Microsoft.Insights/MetricAlerts/Read | Bir ölçüm uyarısı okunuyor |
> | Eylem | Microsoft.Insights/MetricAlerts/Status/Read | Ölçüm uyarı durumu okunuyor |
> | Eylem | Microsoft.Insights/MetricAlerts/Write | Bir ölçüm uyarısı yazılıyor |
> | Eylem | Microsoft.Insights/MetricDefinitions/Microsoft.Insights/Read | Ölçüm tanımlarını oku |
> | Eylem | Microsoft.Insights/MetricDefinitions/providers/Microsoft.Insights/Read | Ölçüm tanımlarını oku |
> | Eylem | Microsoft.Insights/MetricDefinitions/Read | Ölçüm tanımlarını oku |
> | Eylem | Microsoft.Insights/Metrics/providers/Metrics/Read | Ölçümleri okuma |
> | Eylem | Microsoft.Insights/Metrics/Read | Ölçümleri okuma |
> | Eylem | Microsoft.Insights/Metrics/Write | Ölçümleri yaz |
> | Eylem | Microsoft.Insights/MigrateToNewpricingModel/Action | Aboneliği yeni fiyatlandırma modeline geçirin |
> | Eylem | Microsoft.Insights/Operations/Read | İşlemler okunuyor |
> | Eylem | Microsoft.Insights/Register/Action | Microsoft Insights sağlayıcısını kaydedin |
> | Eylem | Microsoft.Insights/RollbackToLegacyPricingModel/Action | Aboneliği eski fiyatlandırma modeline geri alın |
> | Eylem | Microsoft.Insights/ScheduledQueryRules/Delete | Zamanlanmış bir sorgu kuralı siliniyor |
> | Eylem | Microsoft.Insights/ScheduledQueryRules/Read | Zamanlanmış bir sorgu kuralı okunuyor |
> | Eylem | Microsoft.Insights/ScheduledQueryRules/Write | Zamanlanmış bir sorgu kuralı yazılıyor |
> | Eylem | Microsoft.Insights/Tenants/Register/Action | Microsoft Insights sağlayıcısını başlatır |
> | Eylem | Microsoft.Insights/Unregister/Action | Microsoft Insights sağlayıcısını kaydedin |
> | Eylem | Microsoft.Insights/Webtests/Delete | Bir web testi yapılandırmasını silme |
> | Eylem | Microsoft.Insights/Webtests/GetToken/Read | Bir web testi belirtecini okuma |
> | Eylem | Microsoft.Insights/Webtests/MetricDefinitions/Read | Bir web testi ölçümünün tanımlarını okuma |
> | Eylem | Microsoft.Insights/Webtests/Metrics/Read | Bir web testinin ölçümlerini okuma |
> | Eylem | Microsoft.Insights/Webtests/Read | Bir web testi yapılandırmasını okuma |
> | Eylem | Microsoft.Insights/Webtests/Write | Bir web testi yapılandırmasına yazma |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.KeyVault/checkNameAvailability/read | Bir anahtar kasası adının geçerli ve kullanımda olup olmadığını denetler |
> | Eylem | Microsoft.KeyVault/deletedVaults/read | Geçici silinen anahtar kasalarının özelliklerini görüntüleyin |
> | Eylem | Microsoft.KeyVault/hsmPools/delete | Bir HSM havuzunu silin |
> | Eylem | Microsoft.KeyVault/hsmPools/joinVault/action | Bir HSM havuzuna anahtar kasası ekleyin |
> | Eylem | Microsoft.KeyVault/hsmPools/read | Bir HSM havuzunun özelliklerini görüntüleyin |
> | Eylem | Microsoft.KeyVault/hsmPools/write | Yeni bir HSM havuzu oluşturun veya mevcut bir HSM havuzunun özelliklerini güncelleştirin |
> | Eylem | Microsoft.KeyVault/locations/deletedVaults/purge/action | Geçici silinen bir anahtar kasasını temizleyin |
> | Eylem | Microsoft.KeyVault/locations/deletedVaults/read | Geçici silinen bir anahtar kasasının özelliklerini görüntüleyin |
> | Eylem | Microsoft.KeyVault/locations/deleteVirtualNetworkOrSubnets/action | Microsoft.KeyVault'a bir sanal ağın veya alt ağın silindiğini bildirir |
> | Eylem | Microsoft.KeyVault/locations/operationResults/read | Uzun süre çalışan bir işlemin sonucunu denetleyin |
> | Eylem | Microsoft.KeyVault/operations/read | Microsoft.KeyVault kaynak sağlayıcısındaki kullanılabilir işlemleri listele |
> | Eylem | Microsoft.KeyVault/register/action | Bir aboneliği kaydeder |
> | Eylem | Microsoft.KeyVault/unregister/action | Abonelik kaydını siler |
> | Eylem | Microsoft.KeyVault/vaults/accessPolicies/write | Var olan bir erişim ilkesini, birleştirme veya değiştirme yoluyla güncelleştirin ya da kasaya yeni bir erişim ilkesi ekleyin. |
> | Eylem | Microsoft.KeyVault/vaults/delete | Bir anahtar kasasını silme |
> | Eylem | Microsoft.KeyVault/vaults/deploy/action | Azure kaynakları dağıtılırken bir anahtar kasasındaki gizli dizilere erişim sağlar |
> | Eylem | Microsoft.KeyVault/vaults/providers/Microsoft.Insights/diagnosticSettings/Read | Kaynak için tanılama ayarını alır |
> | Eylem | Microsoft.KeyVault/vaults/providers/Microsoft.Insights/diagnosticSettings/Write | Kaynak için tanılama ayarını oluşturur veya güncelleştirir |
> | Eylem | Microsoft.KeyVault/vaults/providers/Microsoft.Insights/logDefinitions/read | Bir anahtar kasası için kullanılabilir günlükleri alır |
> | Eylem | Microsoft.KeyVault/vaults/providers/Microsoft.Insights/metricDefinitions/read | Bir anahtar kasası için kullanılabilir ölçümleri alır |
> | Eylem | Microsoft.KeyVault/vaults/read | Anahtar kasasının özelliklerini görüntüleyin |
> | Eylem | Microsoft.KeyVault/vaults/secrets/read | Bir gizli dizinin özelliklerini (değeri hariç) görüntüleyin |
> | Eylem | Microsoft.KeyVault/vaults/secrets/write | Yeni bir gizli dizi oluşturun veya var olan bir gizli dizinin değerini güncelleştirin |
> | Eylem | Microsoft.KeyVault/vaults/write | Yeni bir anahtar kasası oluşturun veya var olan bir anahtar kasasının özelliklerini güncelleştirin |

## <a name="microsoftkusto"></a>Microsoft.Kusto

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Kusto/Clusters/Databases/delete | Bir veritabanı kaynağı siler. |
> | Eylem | Microsoft.Kusto/Clusters/Databases/read | Bir veritabanı kaynak okur. |
> | Eylem | Microsoft.Kusto/Clusters/Databases/write | Bir veritabanı kaynak yazar. |
> | Eylem | Microsoft.Kusto/Clusters/delete | Bir küme kaynağı siler. |
> | Eylem | Microsoft.Kusto/Clusters/providers/Microsoft.Insights/diagnosticSettings/read | Kaynağın tanılama ayarlarını alır |
> | Eylem | Microsoft.Kusto/Clusters/providers/Microsoft.Insights/diagnosticSettings/write | Kaynak için tanılama ayarını oluşturur veya güncelleştirir |
> | Eylem | Microsoft.Kusto/Clusters/providers/Microsoft.Insights/metricDefinitions/read | Kaynak ölçüm tanımlarını alır |
> | Eylem | Microsoft.Kusto/Clusters/read | Bir küme kaynağı okur. |
> | Eylem | Microsoft.Kusto/Clusters/write | Bir küme kaynağı yazar. |
> | Eylem | Microsoft.Kusto/Locations/CheckNameAvailability/write | Ad kullanılabilirliğini kaynak okuma denetleyin |
> | Eylem | Microsoft.Kusto/locations/operationresults/read | İşlem kaynaklarını okur |
> | Eylem | Microsoft.Kusto/Operations/read | İşlem kaynaklarını okur |

## <a name="microsoftlabservices"></a>Microsoft.LabServices

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.LabServices/labAccounts/CreateLab/action | Bir laboratuvar bir laboratuvar hesabı oluşturun. |
> | Eylem | Microsoft.LabServices/labAccounts/delete | Laboratuvar hesapları silin. |
> | Eylem | Microsoft.LabServices/labAccounts/galleryImages/read | Galeri görüntüleri okuyun. |
> | Eylem | Microsoft.LabServices/labAccounts/galleryImages/write | Ekleyin veya galeri görüntüleri değiştirin. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/delete | Labs silin. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/environmentSettings/ClaimAny/action | Rastgele bir ortamı bir ortam ayarları bir kullanıcı için talepleri |
> | Eylem | Microsoft.LabServices/labAccounts/labs/environmentSettings/delete | Ortam ayarı silin. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/Claim/action | Ortam talep ve kullanıcıya atar |
> | Eylem | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/delete | Ortamlar silin. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/read | Ortamlar okuyun. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/Start/action | Bir ortam ortamı içindeki tüm kaynaklar başlatarak başlatır. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/Stop/action | Bir ortam ortamı içindeki tüm kaynaklar durdurarak durdurur |
> | Eylem | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/write | Ekleyin veya ortamlar değiştirin. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/environmentSettings/Publish/action | Hükümler/deprovisions kaynakları ayarlama ortamı için laboratuvar/ortam ayarı geçerli durumuna göre gereklidir. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/environmentSettings/read | Ortam ayarı okuyun. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/environmentSettings/Start/action | Bir şablon şablonu içindeki tüm kaynakların başlatarak başlatır. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/environmentSettings/Stop/action | Bir şablon şablonu içindeki tüm kaynakların başlatarak başlatır. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/environmentSettings/write | Ekleyin veya ortam ayarını değiştirin. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/read | Labs okuyun. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/Register/action | Yönetilen Laboratuvar kaydedin. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/users/delete | Kullanıcıların silin. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/users/read | Kullanıcılar okuyun. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/users/write | Ekleyin veya kullanıcıları değiştirin. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/write | Ekleyin veya labs değiştirin. |
> | Eylem | Microsoft.LabServices/labAccounts/read | Laboratuvar hesapları okuyun. |
> | Eylem | Microsoft.LabServices/labAccounts/sizes/ListAvailableSkus/action | Bir laboratuvar hesabındaki her boyutu türü için liste kullanılabilir SKU'lar |
> | Eylem | Microsoft.LabServices/labAccounts/sizes/read | Boyutları okuyun. |
> | Eylem | Microsoft.LabServices/labAccounts/write | Ekleyin veya Laboratuvar hesapları değiştirin. |
> | Eylem | Microsoft.LabServices/locations/operations/read | Okuma işlemleri. |
> | Eylem | Microsoft.LabServices/register/action | Aboneliği kaydeder |
> | Eylem | Microsoft.LabServices/users/GetEnvironment/action | Sanal makine ayrıntılarını alır |
> | Eylem | Microsoft.LabServices/users/GetOperationStatus/action | Uzun süre çalışan işlemin durumunu alır |
> | Eylem | Microsoft.LabServices/users/ListEnvironments/action | Kullanıcı için liste ortamları |
> | Eylem | Microsoft.LabServices/users/ListLabs/action | Kullanıcı için Labs listeleyin. |
> | Eylem | Microsoft.LabServices/users/Register/action | Yönetilen bir laboratuvar için bir kullanıcı kaydı |
> | Eylem | Microsoft.LabServices/users/StartEnvironment/action | Bir ortam ortamı içindeki tüm kaynaklar başlatarak başlatır. |
> | Eylem | Microsoft.LabServices/users/StopEnvironment/action | Bir ortam ortamı içindeki tüm kaynaklar durdurarak durdurur |

## <a name="microsoftlocationbasedservices"></a>Microsoft.LocationBasedServices

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.LocationBasedServices/accounts/delete | (Kullanım dışı: Lütfen /providers/Microsoft.Maps kullanın) Temel bir konumu silmek hizmetleri hesabı. |
> | Eylem | Microsoft.LocationBasedServices/accounts/listKeys/action | (Kullanım dışı: Lütfen /providers/Microsoft.Maps kullanın) Konum tabanlı hizmetleri hesabı anahtarlarını Listele |
> | Eylem | Microsoft.LocationBasedServices/accounts/providers/Microsoft.Insights/diagnosticSettings/read | (Kullanım dışı: Lütfen /providers/Microsoft.Maps kullanın) Kaynağın tanılama ayarını alır |
> | Eylem | Microsoft.LocationBasedServices/accounts/providers/Microsoft.Insights/diagnosticSettings/write | (Kullanım dışı: Lütfen /providers/Microsoft.Maps kullanın) Oluşturur veya kaynağın tanılama ayarını güncelleştirir |
> | Eylem | Microsoft.LocationBasedServices/accounts/providers/Microsoft.Insights/metricDefinitions/read | (Kullanım dışı: Lütfen /providers/Microsoft.Maps kullanın) Konum tabanlı Hizmetleri hesapları için kullanılabilir ölçümleri alır |
> | Eylem | Microsoft.LocationBasedServices/accounts/read | (Kullanım dışı: Lütfen /providers/Microsoft.Maps kullanın) Bağlı bir konum alma hizmetleri hesabı. |
> | Eylem | Microsoft.LocationBasedServices/accounts/regenerateKey/action | (Kullanım dışı: Lütfen /providers/Microsoft.Maps kullanın) Yeni konum tabanlı hizmetleri hesabı birincil veya ikincil anahtarı oluştur |
> | Eylem | Microsoft.LocationBasedServices/accounts/write | (Kullanım dışı: Lütfen /providers/Microsoft.Maps kullanın) Oluşturun veya bir konum tabanlı hizmetleri hesabı güncelleştirin. |
> | Eylem | Microsoft.LocationBasedServices/register/action | (Kullanım dışı: Lütfen /providers/Microsoft.Maps kullanın) Sağlayıcıyı kaydettirin |

## <a name="microsoftloganalytics"></a>Microsoft.LogAnalytics

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | DataAction | Microsoft.LogAnalytics/logs/ADAssessmentRecommendation/read | ADAssessmentRecommendation tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/ADReplicationResult/read | ADReplicationResult tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/ADSecurityAssessmentRecommendation/read | ADSecurityAssessmentRecommendation tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/Alert/read | Uyarı tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/AlertHistory/read | AlertHistory tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/ApplicationInsights/read | Applicationınsights tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/AzureActivity/read | AzureActivity tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/AzureMetrics/read | AzureMetrics tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/BoundPort/read | BoundPort tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/CommonSecurityLog/read | CommonSecurityLog tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/ComputerGroup/read | ComputerGroup tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/ConfigurationChange/read | ConfigurationChange tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/ConfigurationData/read | ConfigurationData tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/ContainerImageInventory/read | ContainerImageInventory tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/ContainerInventory/read | ContainerInventory tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/ContainerLog/read | ContainerLog tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/ContainerServiceLog/read | ContainerServiceLog tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/CustomLogs/read | Herhangi bir özel günlüğü verilerini okuma |
> | DataAction | Microsoft.LogAnalytics/logs/DeviceAppCrash/read | DeviceAppCrash tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/DeviceAppLaunch/read | DeviceAppLaunch tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/DeviceCalendar/read | DeviceCalendar tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/DeviceCleanup/read | DeviceCleanup tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/DeviceConnectSession/read | DeviceConnectSession tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/DeviceEtw/read | DeviceEtw tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/DeviceHardwareHealth/read | DeviceHardwareHealth tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/DeviceHealth/read | DeviceHealth tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/DeviceHeartbeat/read | DeviceHeartbeat tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/DeviceSkypeHeartbeat/read | DeviceSkypeHeartbeat tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/DeviceSkypeSignIn/read | DeviceSkypeSignIn tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/DeviceSleepState/read | DeviceSleepState tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/DHAppFailure/read | DHAppFailure tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/DHAppReliability/read | DHAppReliability tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/DHDriverReliability/read | DHDriverReliability tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/DHLogonFailures/read | DHLogonFailures tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/DHLogonMetrics/read | DHLogonMetrics tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/DHOSCrashData/read | DHOSCrashData tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/DHOSReliability/read | DHOSReliability tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/DHWipAppLearning/read | DHWipAppLearning tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/DnsEvents/read | DnsEvents tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/DnsInventory/read | DnsInventory tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/ETWEvent/read | ETWEvent tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/Event/read | Olay tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/ExchangeAssessmentRecommendation/read | ExchangeAssessmentRecommendation tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/Heartbeat/read | Sinyal tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/IISAssessmentRecommendation/read | IISAssessmentRecommendation tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/InboundConnection/read | InboundConnection tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/KubeNodeInventory/read | KubeNodeInventory tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/KubePodInventory/read | KubePodInventory tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/LinuxAuditLog/read | LinuxAuditLog tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/MAApplication/read | MAApplication tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/MAApplicationHealth/read | MAApplicationHealth tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/MAApplicationInstance/read | MAApplicationInstance tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/MAApplicationReadiness/read | MAApplicationReadiness tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/MADeploymentPlan/read | MADeploymentPlan tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/MADevice/read | MADevice tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/MADeviceReadiness/read | MADeviceReadiness tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/MADriverReadiness/read | MADriverReadiness tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeAddin/read | MAOfficeAddin tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeAddinHealth/read | MAOfficeAddinHealth tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeAddinInstance/read | MAOfficeAddinInstance tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeAddinReadiness/read | MAOfficeAddinReadiness tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeApp/read | MAOfficeApp tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeAppHealth/read | MAOfficeAppHealth tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeAppInstance/read | MAOfficeAppInstance tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeAppReadiness/read | MAOfficeAppReadiness tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeBuildInfo/read | MAOfficeBuildInfo tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeCurrencyAssessment/read | MAOfficeCurrencyAssessment tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeCurrencyAssessmentDailyCounts/read | MAOfficeCurrencyAssessmentDailyCounts tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeDeploymentStatus/read | MAOfficeDeploymentStatus tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeMacroIssueReadiness/read | MAOfficeMacroIssueReadiness tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeMacroSummary/read | MAOfficeMacroSummary tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeSuite/read | MAOfficeSuite tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeSuiteInstance/read | MAOfficeSuiteInstance tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/MAProposedPilotDevices/read | MAProposedPilotDevices tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/MAWindowsBuildInfo/read | MAWindowsBuildInfo tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/MAWindowsCurrencyAssessment/read | MAWindowsCurrencyAssessment tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/MAWindowsCurrencyAssessmentDailyCounts/read | MAWindowsCurrencyAssessmentDailyCounts tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/MAWindowsDeploymentStatus/read | MAWindowsDeploymentStatus tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/NetworkMonitoring/read | NetworkMonitoring tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/OfficeActivity/read | OfficeActivity tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/Operation/read | İşlemi tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/OutboundConnection/read | OutboundConnection tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/Perf/read | Perf tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/ProtectionStatus/read | ProtectionStatus tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/ReservedAzureCommonFields/read | ReservedAzureCommonFields tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/ReservedCommonFields/read | ReservedCommonFields tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/SCCMAssessmentRecommendation/read | SCCMAssessmentRecommendation tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/SCOMAssessmentRecommendation/read | SCOMAssessmentRecommendation tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/SecurityAlert/read | SecurityAlert tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/SecurityBaseline/read | SecurityBaseline tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/SecurityBaselineSummary/read | SecurityBaselineSummary tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/SecurityDetection/read | SecurityDetection tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/SecurityEvent/read | SecurityEvent tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/ServiceFabricOperationalEvent/read | ServiceFabricOperationalEvent tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/ServiceFabricReliableActorEvent/read | ServiceFabricReliableActorEvent tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/ServiceFabricReliableServiceEvent/read | ServiceFabricReliableServiceEvent tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/SfBAssessmentRecommendation/read | SfBAssessmentRecommendation tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/SPAssessmentRecommendation/read | SPAssessmentRecommendation tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/SQLAssessmentRecommendation/read | SQLAssessmentRecommendation tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/SQLQueryPerformance/read | SQLQueryPerformance tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/Syslog/read | Syslog tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/SysmonEvent/read | SysmonEvent tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/UAApp/read | UAApp tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/UAComputer/read | UAComputer tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/UAComputerRank/read | UAComputerRank tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/UADriver/read | UADriver tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/UADriverProblemCodes/read | UADriverProblemCodes tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/UAFeedback/read | UAFeedback tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/UAHardwareSecurity/read | UAHardwareSecurity tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/UAIESiteDiscovery/read | UAIESiteDiscovery tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/UAOfficeAddIn/read | UAOfficeAddIn tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/UAProposedActionPlan/read | UAProposedActionPlan tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/UASysReqIssue/read | UASysReqIssue tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/UAUpgradedComputer/read | UAUpgradedComputer tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/Update/read | Güncelleştirme tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/UpdateRunProgress/read | UpdateRunProgress tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/UpdateSummary/read | UpdateSummary tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/Usage/read | Kullanım tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/W3CIISLog/read | W3CIISLog tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/WaaSDeploymentStatus/read | WaaSDeploymentStatus tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/WaaSInsiderStatus/read | WaaSInsiderStatus tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/WaaSUpdateStatus/read | WaaSUpdateStatus tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/WDAVStatus/read | WDAVStatus tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/WDAVThreat/read | WDAVThreat tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/WindowsClientAssessmentRecommendation/read | WindowsClientAssessmentRecommendation tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/WindowsFirewall/read | WindowsFirewall tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/WindowsServerAssessmentRecommendation/read | WindowsServerAssessmentRecommendation tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/WireData/read | WireData tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/WUDOAggregatedStatus/read | WUDOAggregatedStatus tablodan veri okuma |
> | DataAction | Microsoft.LogAnalytics/logs/WUDOStatus/read | WUDOStatus tablodan veri okuma |

## <a name="microsoftlogic"></a>Microsoft.Logic

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Logic/integrationAccounts/agreements/delete | Tümleştirme hesabındaki sözleşmeyi siler. |
> | Eylem | Microsoft.Logic/integrationAccounts/agreements/listContentCallbackUrl/action | Tümleştirme hesabındaki sözleşme içeriği için geri çağırma URL'sini alır. |
> | Eylem | Microsoft.Logic/integrationAccounts/agreements/read | Tümleştirme hesabındaki sözleşmeyi okur. |
> | Eylem | Microsoft.Logic/integrationAccounts/agreements/write | Tümleştirme hesabındaki sözleşmeyi oluşturur veya güncelleştirir. |
> | Eylem | Microsoft.Logic/integrationAccounts/assemblies/delete | Tümleştirme hesabındaki bütünleştirilmiş kodu siler. |
> | Eylem | Microsoft.Logic/integrationAccounts/assemblies/listContentCallbackUrl/action | Tümleştirme hesabındaki bütünleştirilmiş kod içeriği için geri çağırma URL'sini alır. |
> | Eylem | Microsoft.Logic/integrationAccounts/assemblies/read | Tümleştirme hesabındaki bütünleştirilmiş kodu okur. |
> | Eylem | Microsoft.Logic/integrationAccounts/assemblies/write | Tümleştirme hesabındaki bütünleştirilmiş kodu oluşturur veya güncelleştirir. |
> | Eylem | Microsoft.Logic/integrationAccounts/batchConfigurations/delete | Tümleştirme hesabındaki toplu yapılandırmayı siler. |
> | Eylem | Microsoft.Logic/integrationAccounts/batchConfigurations/read | Tümleştirme hesabındaki toplu yapılandırmayı okur. |
> | Eylem | Microsoft.Logic/integrationAccounts/batchConfigurations/write | Tümleştirme hesabındaki toplu yapılandırmayı oluşturur veya güncelleştirir. |
> | Eylem | Microsoft.Logic/integrationAccounts/certificates/delete | Tümleştirme hesabındaki sertifikayı siler. |
> | Eylem | Microsoft.Logic/integrationAccounts/certificates/read | Tümleştirme hesabındaki sertifikayı okur. |
> | Eylem | Microsoft.Logic/integrationAccounts/certificates/write | Tümleştirme hesabındaki sertifikayı oluşturur veya güncelleştirir. |
> | Eylem | Microsoft.Logic/integrationAccounts/delete | Tümleştirme hesabını siler. |
> | Eylem | Microsoft.Logic/integrationAccounts/join/action | Tümleştirme Hesabına katılır. |
> | Eylem | Microsoft.Logic/integrationAccounts/listCallbackUrl/action | Tümleştirme hesabı için geri çağırma URL'sini alır. |
> | Eylem | Microsoft.Logic/integrationAccounts/listKeyVaultKeys/action | Anahtar kasasındaki anahtarları alır. |
> | Eylem | Microsoft.Logic/integrationAccounts/logTrackingEvents/action | Tümleştirme hesabındaki izleme olaylarını günlüğe kaydeder. |
> | Eylem | Microsoft.Logic/integrationAccounts/maps/delete | Tümleştirme hesabındaki eşlemeyi siler. |
> | Eylem | Microsoft.Logic/integrationAccounts/maps/listContentCallbackUrl/action | Tümleştirme hesabındaki eşleme içeriği için geri çağırma URL'sini alır. |
> | Eylem | Microsoft.Logic/integrationAccounts/maps/read | Tümleştirme hesabındaki eşlemeyi okur. |
> | Eylem | Microsoft.Logic/integrationAccounts/maps/write | Tümleştirme hesabındaki eşlemeyi oluşturur veya güncelleştirir. |
> | Eylem | Microsoft.Logic/integrationAccounts/partners/delete | Tümleştirme hesabındaki iş ortağını siler. |
> | Eylem | Microsoft.Logic/integrationAccounts/partners/listContentCallbackUrl/action | Tümleştirme hesabındaki iş ortağı içeriği için geri çağırma URL'sini alır. |
> | Eylem | Microsoft.Logic/integrationAccounts/partners/read | Tümleştirme hesabındaki iş ortağını okur. |
> | Eylem | Microsoft.Logic/integrationAccounts/partners/write | Tümleştirme hesabındaki iş ortağını oluşturur veya güncelleştirir. |
> | Eylem | Microsoft.Logic/integrationAccounts/providers/Microsoft.Insights/logDefinitions/read | Tümleştirme Hesabı günlük tanımlarını okur. |
> | Eylem | Microsoft.Logic/integrationAccounts/read | Tümleştirme hesabını okur. |
> | Eylem | Microsoft.Logic/integrationAccounts/regenerateAccessKey/action | Erişim anahtarı parolalarını yeniden oluşturur. |
> | Eylem | Microsoft.Logic/integrationAccounts/schemas/delete | Tümleştirme hesabındaki şemayı siler. |
> | Eylem | Microsoft.Logic/integrationAccounts/schemas/listContentCallbackUrl/action | Tümleştirme hesabındaki şema içeriği için geri çağırma URL'sini alır. |
> | Eylem | Microsoft.Logic/integrationAccounts/schemas/read | Tümleştirme hesabındaki şemayı okur. |
> | Eylem | Microsoft.Logic/integrationAccounts/schemas/write | Tümleştirme hesabındaki şemayı oluşturur veya güncelleştirir. |
> | Eylem | Microsoft.Logic/integrationAccounts/sessions/delete | Tümleştirme hesabındaki oturumu siler. |
> | Eylem | Microsoft.Logic/integrationAccounts/sessions/read | Tümleştirme hesabındaki toplu yapılandırmayı okur. |
> | Eylem | Microsoft.Logic/integrationAccounts/sessions/write | Tümleştirme hesabındaki oturumu oluşturur veya güncelleştirir. |
> | Eylem | Microsoft.Logic/integrationAccounts/write | Tümleştirme hesabını oluşturur veya güncelleştirir. |
> | Eylem | Microsoft.Logic/isolatedEnvironments/delete | Yalıtılmış ortamı siler. |
> | Eylem | Microsoft.Logic/isolatedEnvironments/join/action | Yalıtılmış Ortama katılır. |
> | Eylem | Microsoft.Logic/isolatedEnvironments/read | Yalıtılmış ortamı okur. |
> | Eylem | Microsoft.Logic/isolatedEnvironments/write | Yalıtımış ortamı oluşturur veya güncelleştirir. |
> | Eylem | Microsoft.Logic/locations/workflows/validate/action | İş akışını doğrular. |
> | Eylem | Microsoft.Logic/operations/read | İşlemi alır. |
> | Eylem | Microsoft.Logic/register/action | Belirli bir abonelik için Microsoft.Logic kaynak sağlayıcısını kaydeder. |
> | Eylem | Microsoft.Logic/workflows/accessKeys/delete | Erişim anahtarını siler. |
> | Eylem | Microsoft.Logic/workflows/accessKeys/list/action | Erişim anahtarı parolalarını listeler. |
> | Eylem | Microsoft.Logic/workflows/accessKeys/read | Erişim anahtarını okur. |
> | Eylem | Microsoft.Logic/workflows/accessKeys/regenerate/action | Erişim anahtarı parolalarını yeniden oluşturur. |
> | Eylem | Microsoft.Logic/workflows/accessKeys/write | Erişim anahtarı oluşturur veya güncelleştirir. |
> | Eylem | Microsoft.Logic/workflows/delete | İş akışını siler. |
> | Eylem | Microsoft.Logic/workflows/disable/action | İş akışını devre dışı bırakır. |
> | Eylem | Microsoft.Logic/workflows/enable/action | İş akışını etkinleştirir. |
> | Eylem | Microsoft.Logic/workflows/listCallbackUrl/action | İş akışı için geri çağrıma URL'sini alır. |
> | Eylem | Microsoft.Logic/workflows/listSwagger/action | İş akışı swagger tanımlarını alır. |
> | Eylem | Microsoft.Logic/workflows/move/action | İş Akışını mevcut abonelik kimliği, kaynak grubu ve/veya adından farklı bir abonelik kimliğine, kaynak grubuna ve/veya ada taşır. |
> | Eylem | Microsoft.Logic/workflows/providers/Microsoft.Insights/diagnosticSettings/read | İş akışı tanılama ayarlarını okur. |
> | Eylem | Microsoft.Logic/workflows/providers/Microsoft.Insights/diagnosticSettings/write | İş akışı tanılama ayarını oluşturur veya güncelleştirir. |
> | Eylem | Microsoft.Logic/workflows/providers/Microsoft.Insights/logDefinitions/read | İş akışı günlüğü tanımlarını okur. |
> | Eylem | Microsoft.Logic/workflows/providers/Microsoft.Insights/metricDefinitions/read | İş akışı ölçüm tanımlarını okur. |
> | Eylem | Microsoft.Logic/workflows/read | İş akışını okur. |
> | Eylem | Microsoft.Logic/workflows/regenerateAccessKey/action | Erişim anahtarı parolalarını yeniden oluşturur. |
> | Eylem | Microsoft.Logic/workflows/run/action | İş akışının çalıştırılmasını başlatır. |
> | Eylem | Microsoft.Logic/workflows/runs/actions/listExpressionTraces/action | İş akışı çalıştırma eylemi ifade izlemelerini alır. |
> | Eylem | Microsoft.Logic/workflows/runs/actions/read | İş akışı çalıştırma eylemini okur. |
> | Eylem | Microsoft.Logic/workflows/runs/actions/repetitions/listExpressionTraces/action | İş akışı çalıştırma eylemi yineleme ifadesi izlemelerini alır. |
> | Eylem | Microsoft.Logic/workflows/runs/actions/repetitions/read | İş akışı çalıştırma eylemi yinelemesini okur. |
> | Eylem | Microsoft.Logic/workflows/runs/actions/repetitions/requestHistories/read | İş akışı çalıştırma yineleme eylemi istek geçmişini okur. |
> | Eylem | Microsoft.Logic/workflows/runs/actions/requestHistories/read | İş akışı çalıştırma eylemi istek geçmişini okur. |
> | Eylem | Microsoft.Logic/workflows/runs/actions/scoperepetitions/read | İş akışı çalıştırma eylemi yineleme kapsamını okur. |
> | Eylem | Microsoft.Logic/workflows/runs/cancel/action | İş akışının çalıştırılmasını iptal eder. |
> | Eylem | Microsoft.Logic/workflows/runs/operations/read | İş akışı çalıştırma işlemi durumunu okur. |
> | Eylem | Microsoft.Logic/workflows/runs/read | İş akışı çalıştırmasını okur. |
> | Eylem | Microsoft.Logic/workflows/suspend/action | İş akışını askıya alır. |
> | Eylem | Microsoft.Logic/workflows/triggers/histories/read | Tetikleyici geçmişlerini okur. |
> | Eylem | Microsoft.Logic/workflows/triggers/histories/resubmit/action | İş akışı tetikleyicisini yeniden gönderir. |
> | Eylem | Microsoft.Logic/workflows/triggers/listCallbackUrl/action | Tetikleyici için geri arama URL'sini alır. |
> | Eylem | Microsoft.Logic/workflows/triggers/read | Tetikleyiciyi okur. |
> | Eylem | Microsoft.Logic/workflows/triggers/reset/action | Tetikleyiciyi sıfırlar. |
> | Eylem | Microsoft.Logic/workflows/triggers/run/action | Tetikleyiciyi yürütür. |
> | Eylem | Microsoft.Logic/workflows/triggers/setState/action | Tetikleyici durumunu ayarlar. |
> | Eylem | Microsoft.Logic/workflows/validate/action | İş akışını doğrular. |
> | Eylem | Microsoft.Logic/workflows/versions/read | İş akışı sürümünü okur. |
> | Eylem | Microsoft.Logic/workflows/versions/triggers/listCallbackUrl/action | Tetikleyici için geri arama URL'sini alır. |
> | Eylem | Microsoft.Logic/workflows/write | İş akışını oluşturur veya güncelleştirir. |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.MachineLearning/commitmentPlans/commitmentAssociations/move/action | Taahhüt Plan ilişkilendirme öğrenme herhangi bir makineye taşıma |
> | Eylem | Microsoft.MachineLearning/commitmentPlans/commitmentAssociations/read | Taahhüt Plan ilişkilendirme öğrenme herhangi bir makineye okuma |
> | Eylem | Microsoft.MachineLearning/commitmentPlans/delete | Taahhüt Plan öğrenme herhangi bir makineye Sil |
> | Eylem | Microsoft.MachineLearning/commitmentPlans/join/action | Taahhüt Plan öğrenme herhangi bir makineye katılma |
> | Eylem | Microsoft.MachineLearning/commitmentPlans/read | Taahhüt Plan öğrenme herhangi bir makineye okuma |
> | Eylem | Microsoft.MachineLearning/commitmentPlans/write | Herhangi bir Machine Learning taahhüt planının güncelle |
> | Eylem | Microsoft.MachineLearning/locations/operationresults/read | Machine Learning işleminin sonucu alın |
> | Eylem | Microsoft.MachineLearning/locations/operationsstatus/read | Devam eden bir Machine Learning işlem durumunu Al |
> | Eylem | Microsoft.MachineLearning/operations/read | Machine Learning işlemleri Al |
> | Eylem | Microsoft.MachineLearning/register/action | Machine learning web hizmeti kaynak sağlayıcısı için aboneliği kaydeder ve web hizmetleri oluşturulmasını sağlar. |
> | Eylem | Microsoft.MachineLearning/skus/read | Machine Learning taahhüt Plan SKU'ları alma |
> | Eylem | Microsoft.MachineLearning/webServices/action | Desteklenen bölgeler için bölgesel Web hizmeti özellikleri oluşturma |
> | Eylem | Microsoft.MachineLearning/webServices/delete | Bir Machine Learning Web hizmeti Sil |
> | Eylem | Microsoft.MachineLearning/webServices/listkeys/read | Machine Learning Web hizmeti anahtarları alma |
> | Eylem | Microsoft.MachineLearning/webServices/read | Bir Machine Learning Web hizmeti okuma |
> | Eylem | Microsoft.MachineLearning/webServices/write | Bir Machine Learning Web hizmeti güncelle |
> | Eylem | Microsoft.MachineLearning/Workspaces/delete | Bir Machine Learning çalışma alanı silme |
> | Eylem | Microsoft.MachineLearning/Workspaces/listworkspacekeys/action | Bir Machine Learning çalışma alanı listesi tuşları |
> | Eylem | Microsoft.MachineLearning/Workspaces/read | Çalışma alanı öğrenme herhangi bir makineye okuma |
> | Eylem | Microsoft.MachineLearning/Workspaces/resyncstoragekeys/action | Machine Learning çalışma alanı için yapılandırılan depolama hesabı anahtarlarını yeniden eşitleme |
> | Eylem | Microsoft.MachineLearning/Workspaces/write | Çalışma alanı öğrenme makine oluşturulamıyor veya güncelleştirilemiyor |

## <a name="microsoftmachinelearningcompute"></a>Microsoft.MachineLearningCompute

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.MachineLearningCompute/operationalizationClusters/checkUpdate/action | Güncelleştirmeleri Sistem Hizmetleri operationalization küme için kullanılabilir olup olmadığını denetleyin |
> | Eylem | Microsoft.MachineLearningCompute/operationalizationClusters/delete | Herhangi bir barındırma hesabı silme |
> | Eylem | Microsoft.MachineLearningCompute/operationalizationClusters/listKeys/action | Operationalization kümesi ile ilişkili tuşlarını listesi |
> | Eylem | Microsoft.MachineLearningCompute/operationalizationClusters/read | Herhangi bir barındırma hesabı okuma |
> | Eylem | Microsoft.MachineLearningCompute/operationalizationClusters/updateSystem/action | Güncelleştirme operationalization kümedeki Sistem Hizmetleri |
> | Eylem | Microsoft.MachineLearningCompute/operationalizationClusters/write | Herhangi bir barındırma hesabı güncelle |
> | Eylem | Microsoft.MachineLearningCompute/register/action | Abonelik kimliği kaynak sağlayıcısına kaydeder ve işlem kaynaklarını öğrenme bir makineye oluşturulmasını sağlar |

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.MachineLearningModelManagement/accounts/delete | Herhangi bir barındırma hesabı silme |
> | Eylem | Microsoft.MachineLearningModelManagement/accounts/read | Herhangi bir barındırma hesabı okuma |
> | Eylem | Microsoft.MachineLearningModelManagement/accounts/write | Herhangi bir barındırma hesabı güncelle |
> | Eylem | Microsoft.MachineLearningModelManagement/register/action | Abonelik kimliği kaynak sağlayıcısına kaydeder ve barındırma hesabı oluşturma sağlar |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.MachineLearningServices/register/action | Machine Learning Hizmetleri kaynak sağlayıcısı için aboneliği kaydeder |
> | Eylem | Microsoft.MachineLearningServices/workspaces/computes/delete | Machine Learning Hizmetleri çalışma alanları işlem kaynakları siler |
> | Eylem | Microsoft.MachineLearningServices/workspaces/computes/listKeys/action | Machine Learning hizmetler çalışma alanında işlem kaynakları gizli listesi |
> | Eylem | Microsoft.MachineLearningServices/workspaces/computes/read | Machine Learning Hizmetleri çalışma alanları işlem kaynaklarını alır |
> | Eylem | Microsoft.MachineLearningServices/workspaces/computes/write | Oluşturur veya Machine Learning Hizmetleri çalışma alanları işlem kaynaklarında güncelleştirir |
> | Eylem | Microsoft.MachineLearningServices/workspaces/delete | Makine öğrenimi Hizmetleri çalışma alanları siler |
> | Eylem | Microsoft.MachineLearningServices/workspaces/listKeys/action | Bir Machine Learning Services çalışma alanı için gizli anahtarları listeleme |
> | Eylem | Microsoft.MachineLearningServices/workspaces/read | Makine öğrenimi Hizmetleri çalışma alanları alır |
> | Eylem | Microsoft.MachineLearningServices/workspaces/write | Oluşturur veya bir makine öğrenme Hizmetleri çalışma alanları güncelleştirir |

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.ManagedIdentity/userAssignedIdentities/assign/action | Varolan bir kullanıcı atamak için RBAC eylem kimliği için bir kaynak atanır. |
> | Eylem | Microsoft.ManagedIdentity/userAssignedIdentities/delete | Mevcut bir kullanıcı kimliği atanır siler |
> | Eylem | Microsoft.ManagedIdentity/userAssignedIdentities/read | Mevcut bir kullanıcı kimliği atanır alır |
> | Eylem | Microsoft.ManagedIdentity/userAssignedIdentities/write | Yeni bir kullanıcı kimliği atanır oluşturur veya mevcut bir kullanıcı kimliği atanır ile ilişkili etiketleri güncelleştirir |

## <a name="microsoftmanagedlab"></a>Microsoft.ManagedLab

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.ManagedLab/labAccounts/CreateLab/action | Bir laboratuvar bir laboratuvar hesabı oluşturun. |
> | Eylem | Microsoft.ManagedLab/labAccounts/delete | Laboratuvar hesapları silin. |
> | Eylem | Microsoft.ManagedLab/labAccounts/labs/delete | Labs silin. |
> | Eylem | Microsoft.ManagedLab/labAccounts/labs/environmentSettings/delete | Ortam ayarı silin. |
> | Eylem | Microsoft.ManagedLab/labAccounts/labs/environmentSettings/environments/delete | Ortamlar silin. |
> | Eylem | Microsoft.ManagedLab/labAccounts/labs/environmentSettings/environments/read | Ortamlar okuyun. |
> | Eylem | Microsoft.ManagedLab/labAccounts/labs/environmentSettings/environments/write | Ekleyin veya ortamlar değiştirin. |
> | Eylem | Microsoft.ManagedLab/labAccounts/labs/environmentSettings/read | Ortam ayarı okuyun. |
> | Eylem | Microsoft.ManagedLab/labAccounts/labs/environmentSettings/write | Ekleyin veya ortam ayarını değiştirin. |
> | Eylem | Microsoft.ManagedLab/labAccounts/labs/labVms/delete | Laboratuvar sanal makineleri silin. |
> | Eylem | Microsoft.ManagedLab/labAccounts/labs/labVms/read | Laboratuvar sanal makineleri okuyun. |
> | Eylem | Microsoft.ManagedLab/labAccounts/labs/labVms/write | Ekleyin veya Laboratuvar sanal makineleri değiştirin. |
> | Eylem | Microsoft.ManagedLab/labAccounts/labs/read | Labs okuyun. |
> | Eylem | Microsoft.ManagedLab/labAccounts/labs/write | Ekleyin veya labs değiştirin. |
> | Eylem | Microsoft.ManagedLab/labAccounts/read | Laboratuvar hesapları okuyun. |
> | Eylem | Microsoft.ManagedLab/labAccounts/write | Ekleyin veya Laboratuvar hesapları değiştirin. |
> | Eylem | Microsoft.ManagedLab/locations/operations/read | Okuma işlemleri. |
> | Eylem | Microsoft.ManagedLab/register/action | Aboneliği kaydeder |

## <a name="microsoftmanagement"></a>Microsoft.Management

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Management/checkNameAvailability/action | Belirtilen yönetim grubu adı geçerli ve benzersiz olup olmadığını denetler. |
> | Eylem | Microsoft.Management/getEntities/action | Kimliği doğrulanmış kullanıcı için tüm varlıkları (Yönetim grupları, abonelikler, vb.) listeler. |
> | Eylem | Microsoft.Management/managementGroups/delete | Yönetim grubunu silin. |
> | Eylem | Microsoft.Management/managementGroups/read | Kimliği doğrulanmış kullanıcı için Yönetim grupları listesi. |
> | Eylem | Microsoft.Management/managementGroups/subscriptions/delete | Yönetim grubu abonelikten XML'deki ilişkilendirir. |
> | Eylem | Microsoft.Management/managementGroups/subscriptions/write | Yönetim grubu abonelikle varolan ilişkilendirir. |
> | Eylem | Microsoft.Management/managementGroups/write | Oluşturun veya bir yönetim grubu güncelleştirin. |
> | Eylem | Microsoft.Management/register/action | Belirtilen abonelik Microsoft.Management ile kaydetme |

## <a name="microsoftmaps"></a>Microsoft.Maps

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Maps/accounts/delete | Bir Maps hesabı silin. |
> | Eylem | Microsoft.Maps/accounts/listKeys/action | Liste eşlemeleri hesabı anahtarları |
> | Eylem | Microsoft.Maps/accounts/providers/Microsoft.Insights/diagnosticSettings/read | Kaynak için tanılama ayarını alır |
> | Eylem | Microsoft.Maps/accounts/providers/Microsoft.Insights/diagnosticSettings/write | Kaynak için tanılama ayarını oluşturur veya güncelleştirir |
> | Eylem | Microsoft.Maps/accounts/providers/Microsoft.Insights/metricDefinitions/read | Maps hesapları için kullanılabilir ölçümleri alır |
> | Eylem | Microsoft.Maps/accounts/read | Bir Maps hesabı edinin. |
> | Eylem | Microsoft.Maps/accounts/regenerateKey/action | Yeni eşlemeler hesap birincil veya ikincil anahtarı oluştur |
> | Eylem | Microsoft.Maps/accounts/write | Oluşturun veya bir Maps hesabı güncelleştirin. |
> | Eylem | Microsoft.Maps/register/action | Sağlayıcıyı kaydettirin |

## <a name="microsoftmarketplace"></a>Microsoft.Marketplace

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Marketplace/offerTypes/publishers/offers/plans/agreements/read | Bir anlaşma döndürür. |
> | Eylem | Microsoft.Marketplace/offerTypes/publishers/offers/plans/agreements/write | İmzalı sözleşmesini kabul eder. |
> | Eylem | Microsoft.Marketplace/offerTypes/publishers/offers/plans/configs/importImage/action | Görüntü için son kullanıcının ACR alır. |
> | Eylem | Microsoft.Marketplace/offerTypes/publishers/offers/plans/configs/read | Bir yapılandırma döndürür. |
> | Eylem | Microsoft.Marketplace/offerTypes/publishers/offers/plans/configs/write | Bir yapılandırma kaydeder. |

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.MarketplaceApps/ClassicDevServices/delete | Klasik geliştirme hizmeti kaynak üzerinde bir silme işlemi yapar. |
> | Eylem | Microsoft.MarketplaceApps/ClassicDevServices/listSecrets/action | Klasik geliştirme hizmeti kaynak yönetimi anahtarları alır. |
> | Eylem | Microsoft.MarketplaceApps/ClassicDevServices/listSingleSignOnToken/action | Üzerinde tek oturum URL'si için bir Klasik geliştirme hizmeti alır. |
> | Eylem | Microsoft.MarketplaceApps/ClassicDevServices/read | Klasik geliştirme hizmeti üzerinde bir GET işlemi yapar. |
> | Eylem | Microsoft.MarketplaceApps/ClassicDevServices/regenerateKey/action | Klasik geliştirme hizmeti kaynak yönetimi anahtarları oluşturur. |
> | Eylem | Microsoft.MarketplaceApps/Operations/read | Tüm kaynak türleri için işlemleri okuyun. |

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceOrdering

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.MarketplaceOrdering/agreements/offers/plans/cancel/action | Verilen Market öğesi için bir sözleşmeyi iptal et |
> | Eylem | Microsoft.MarketplaceOrdering/agreements/offers/plans/read | Verilen Market öğesi için bir anlaşma Döndür |
> | Eylem | Microsoft.MarketplaceOrdering/agreements/offers/plans/sign/action | Bir anlaşma verilen Market öğesi için oturum açın |
> | Eylem | Microsoft.MarketplaceOrdering/agreements/read | Tüm anlaşmalar altında dönüş abonelik verilen |
> | Eylem | Microsoft.MarketplaceOrdering/offertypes/publishers/offers/plans/agreements/read | Verilen Market sanal makine öğesi için bir anlaşma Al |
> | Eylem | Microsoft.MarketplaceOrdering/offertypes/publishers/offers/plans/agreements/write | Oturum veya verilen Market sanal makine öğesi için bir sözleşmeyi iptal et |

## <a name="microsoftmedia"></a>Microsoft.Media

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Media/checknameavailability/action | Bir Media Services hesabı adı olup olmadığını denetler |
> | Eylem | Microsoft.Media/locations/checkNameAvailability/action | Bir Media Services hesabı adı olup olmadığını denetler |
> | Eylem | Microsoft.Media/mediaservices/assets/delete | Herhangi bir varlığını silme |
> | Eylem | Microsoft.Media/mediaservices/assets/getEncryptionKey/action | Varlık şifreleme anahtarı alma |
> | Eylem | Microsoft.Media/mediaservices/assets/listContainerSas/action | Listesi varlık kapsayıcı SAS URL'leri |
> | Eylem | Microsoft.Media/mediaservices/assets/read | Bir varlığı okuma |
> | Eylem | Microsoft.Media/mediaservices/assets/write | Bir varlığı oluşturma veya güncelleştirme |
> | Eylem | Microsoft.Media/mediaservices/contentKeyPolicies/delete | Tüm içerik anahtar ilkelerini Sil |
> | Eylem | Microsoft.Media/mediaservices/contentKeyPolicies/getPolicyPropertiesWithSecrets/action | Gizli anahtarlarla ilke özelliklerini alma |
> | Eylem | Microsoft.Media/mediaservices/contentKeyPolicies/read | Tüm içerik anahtar ilkelerini oku |
> | Eylem | Microsoft.Media/mediaservices/contentKeyPolicies/write | Herhangi bir içerik anahtarı ilkesi güncelle |
> | Eylem | Microsoft.Media/mediaservices/delete | Herhangi bir Media Services hesabı silme |
> | Eylem | Microsoft.Media/mediaservices/eventGridFilters/delete | Herhangi bir olay kılavuz filtre Sil |
> | Eylem | Microsoft.Media/mediaservices/eventGridFilters/read | Herhangi bir olay kılavuz filtre okuma |
> | Eylem | Microsoft.Media/mediaservices/eventGridFilters/write | Herhangi bir olay kılavuz filtre güncelle |
> | Eylem | Microsoft.Media/mediaservices/liveEventOperations/read | Herhangi bir canlı olay işlem okuma |
> | Eylem | Microsoft.Media/mediaservices/liveEvents/delete | Herhangi bir canlı olay silme |
> | Eylem | Microsoft.Media/mediaservices/liveEvents/liveOutputs/delete | Herhangi bir dinamik çıktı Sil |
> | Eylem | Microsoft.Media/mediaservices/liveEvents/liveOutputs/read | Herhangi bir dinamik çıktı okuma |
> | Eylem | Microsoft.Media/mediaservices/liveEvents/liveOutputs/write | Herhangi bir dinamik çıktı güncelle |
> | Eylem | Microsoft.Media/mediaservices/liveEvents/read | Herhangi bir canlı olay okuma |
> | Eylem | Microsoft.Media/mediaservices/liveEvents/reset/action | Herhangi bir canlı olay işlem Sıfırla |
> | Eylem | Microsoft.Media/mediaservices/liveEvents/start/action | Herhangi bir canlı olay işlem Başlat |
> | Eylem | Microsoft.Media/mediaservices/liveEvents/stop/action | Herhangi bir canlı olay işlem Durdur |
> | Eylem | Microsoft.Media/mediaservices/liveEvents/write | Herhangi bir canlı olay güncelle |
> | Eylem | Microsoft.Media/mediaservices/liveOutputOperations/read | Herhangi bir dinamik çıktı işlem okuma |
> | Eylem | Microsoft.Media/mediaservices/read | Herhangi bir Media Services hesabı okuma |
> | Eylem | Microsoft.Media/mediaservices/streamingEndpointOperations/read | Herhangi bir akış uç noktası işlem okuma |
> | Eylem | Microsoft.Media/mediaservices/streamingEndpoints/delete | Hiçbir akış uç noktasını sil |
> | Eylem | Microsoft.Media/mediaservices/streamingEndpoints/providers/Microsoft.Insights/diagnosticSettings/read | Kaynağın tanılama ayarını alır. |
> | Eylem | Microsoft.Media/mediaservices/streamingEndpoints/providers/Microsoft.Insights/diagnosticSettings/write | Kaynağın tanılama ayarını oluşturur veya güncelleştirir. |
> | Eylem | Microsoft.Media/mediaservices/streamingEndpoints/providers/Microsoft.Insights/metricDefinitions/read | Medya Hizmetleri akış uç noktası ölçümleri tanımları listesini alın. |
> | Eylem | Microsoft.Media/mediaservices/streamingEndpoints/read | Herhangi bir akış uç nokta okuma |
> | Eylem | Microsoft.Media/mediaservices/streamingEndpoints/scale/action | Herhangi bir akış uç noktası işlem ölçeklendirme |
> | Eylem | Microsoft.Media/mediaservices/streamingEndpoints/start/action | Herhangi bir akış uç noktası işlem Başlat |
> | Eylem | Microsoft.Media/mediaservices/streamingEndpoints/stop/action | Herhangi bir akış uç noktası işlemi durdur |
> | Eylem | Microsoft.Media/mediaservices/streamingEndpoints/write | Herhangi bir akış uç nokta güncelle |
> | Eylem | Microsoft.Media/mediaservices/streamingLocators/delete | Her akış Bulucuyu silin |
> | Eylem | Microsoft.Media/mediaservices/streamingLocators/listContentKeys/action | Liste içerik anahtarı |
> | Eylem | Microsoft.Media/mediaservices/streamingLocators/listPaths/action | Liste yolları |
> | Eylem | Microsoft.Media/mediaservices/streamingLocators/read | Her akış Bulucusu okuma |
> | Eylem | Microsoft.Media/mediaservices/streamingLocators/write | Her akış Bulucusu güncelle |
> | Eylem | Microsoft.Media/mediaservices/streamingPolicies/delete | Tüm akış ilkelerini Sil |
> | Eylem | Microsoft.Media/mediaservices/streamingPolicies/read | Tüm akış ilkelerini oku |
> | Eylem | Microsoft.Media/mediaservices/streamingPolicies/write | Herhangi bir akış İlkesi güncelle |
> | Eylem | Microsoft.Media/mediaservices/syncStorageKeys/action | Ekli bir Azure depolama hesabı için depolama anahtarları Eşitle |
> | Eylem | Microsoft.Media/mediaservices/transforms/delete | Hiçbir dönüştürme Sil |
> | Eylem | Microsoft.Media/mediaservices/transforms/jobs/cancelJob/action | İşi İptal Et |
> | Eylem | Microsoft.Media/mediaservices/transforms/jobs/delete | Herhangi bir işi Sil |
> | Eylem | Microsoft.Media/mediaservices/transforms/jobs/read | Herhangi bir işi okuma |
> | Eylem | Microsoft.Media/mediaservices/transforms/jobs/write | Herhangi bir işi güncelle |
> | Eylem | Microsoft.Media/mediaservices/transforms/read | Hiçbir dönüştürme okuma |
> | Eylem | Microsoft.Media/mediaservices/transforms/write | Hiçbir dönüştürme güncelle |
> | Eylem | Microsoft.Media/mediaservices/write | Herhangi bir Media Services hesabı güncelle |
> | Eylem | Microsoft.Media/operations/read | Kullanılabilir işlemleri Al |
> | Eylem | Microsoft.Media/register/action | Media Services kaynak sağlayıcısı için aboneliği kaydeder ve Media Services hesapları oluşturmanıza olanak tanıyan |
> | Eylem | Microsoft.Media/unregister/action | Media Services kaynak sağlayıcısı için abonelik kaydını siler |

## <a name="microsoftmigrate"></a>Microsoft.Migrate

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Migrate/locations/assessmentOptions/read | Verilen konumda kullanılabilen iç değerlendirme seçeneklerini alır |
> | Eylem | Microsoft.Migrate/locations/checknameavailability/action | Belirtilen konumdaki belirtilen abonelik için kaynak adının kullanılabilirliğini denetler |
> | Eylem | Microsoft.Migrate/Operations/read | Microsoft.Migrate kaynak sağlayıcısındaki kullanılabilir işlemleri listeler |
> | Eylem | Microsoft.Migrate/projects/delete | Projeyi siler |
> | Eylem | Microsoft.Migrate/projects/groups/assessments/assessedmachines/read | Değerlendirilen bir makinenin özelliklerini alın |
> | Eylem | Microsoft.Migrate/projects/groups/assessments/delete | Bir iç değerlendirmeyi siler |
> | Eylem | Microsoft.Migrate/projects/groups/assessments/read | Bir iç değerlendirmenin özelliklerini alın |
> | Eylem | Microsoft.Migrate/projects/groups/assessments/write | Yeni bir iç değerlendirme oluşturur veya mevcut bir iç değerlendirmeyi güncelleştirir |
> | Eylem | Microsoft.Migrate/projects/groups/delete | Bir grubu siler |
> | Eylem | Microsoft.Migrate/projects/groups/read | Bir grubun özelliklerini alın |
> | Eylem | Microsoft.Migrate/projects/groups/write | Yeni bir grup oluşturur veya mevcut bir grubu güncelleştirir |
> | Eylem | Microsoft.Migrate/projects/keys/action | Proje için paylaşılan anahtarları alır |
> | Eylem | Microsoft.Migrate/projects/machines/read | Bir makinenin özelliklerini alır |
> | Eylem | Microsoft.Migrate/projects/read | Bir projenin özelliklerini alır |
> | Eylem | Microsoft.Migrate/projects/write | Yeni bir proje oluşturur veya mevcut bir projeyi güncelleştirir |
> | Eylem | Microsoft.Migrate/register/action | Aboneliği Microsoft.Migrate kaynak sağlayıcısına kaydeder |

## <a name="microsoftnetapp"></a>Microsoft.NetApp

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.NetApp/locations/operationresults/read | Bir işlem sonucu kaynak okur. |
> | Eylem | Microsoft.NetApp/locations/read | Okuma kullanılabilirlik bir kaynak denetleyin. |
> | Eylem | Microsoft.NetApp/netAppAccounts/capacityPools/delete | Bir havuz kaynak siler. |
> | Eylem | Microsoft.NetApp/netAppAccounts/capacityPools/providers/Microsoft.Insights/diagnosticSettings/read | Bir havuz kaynak siler. |
> | Eylem | Microsoft.NetApp/netAppAccounts/capacityPools/providers/Microsoft.Insights/diagnosticSettings/write | Bir havuz kaynak siler. |
> | Eylem | Microsoft.NetApp/netAppAccounts/capacityPools/providers/Microsoft.Insights/metricDefinitions/read | Bir havuz kaynak siler. |
> | Eylem | Microsoft.NetApp/netAppAccounts/capacityPools/read | Bir havuz kaynak okur. |
> | Eylem | Microsoft.NetApp/netAppAccounts/capacityPools/Volumes/delete | Bir birim kaynağı siler. |
> | Eylem | Microsoft.NetApp/netAppAccounts/capacityPools/Volumes/MountTargets/delete | Bir bağlama hedef kaynak siler. |
> | Eylem | Microsoft.NetApp/netAppAccounts/capacityPools/Volumes/MountTargets/read | Bir bağlama hedef kaynak okur. |
> | Eylem | Microsoft.NetApp/netAppAccounts/capacityPools/Volumes/MountTargets/write | Bir bağlama hedef kaynak yazar. |
> | Eylem | Microsoft.NetApp/netAppAccounts/capacityPools/Volumes/providers/Microsoft.Insights/diagnosticSettings/read | Bir birim kaynağı siler. |
> | Eylem | Microsoft.NetApp/netAppAccounts/capacityPools/Volumes/providers/Microsoft.Insights/diagnosticSettings/write | Bir birim kaynağı siler. |
> | Eylem | Microsoft.NetApp/netAppAccounts/capacityPools/Volumes/providers/Microsoft.Insights/metricDefinitions/read | Bir birim kaynağı siler. |
> | Eylem | Microsoft.NetApp/netAppAccounts/capacityPools/Volumes/read | Bir birim kaynağı okur. |
> | Eylem | Microsoft.NetApp/netAppAccounts/capacityPools/Volumes/Snapshots/delete | Bir anlık görüntü kaynağı siler. |
> | Eylem | Microsoft.NetApp/netAppAccounts/capacityPools/Volumes/Snapshots/read | Bir anlık görüntü kaynağı okur. |
> | Eylem | Microsoft.NetApp/netAppAccounts/capacityPools/Volumes/Snapshots/write | Bir anlık görüntü kaynağı yazar. |
> | Eylem | Microsoft.NetApp/netAppAccounts/capacityPools/Volumes/write | Bir birim kaynağı yazar. |
> | Eylem | Microsoft.NetApp/netAppAccounts/capacityPools/write | Bir havuz kaynak yazar. |
> | Eylem | Microsoft.NetApp/netAppAccounts/delete | Bir hesap kaynak siler. |
> | Eylem | Microsoft.NetApp/netAppAccounts/read | Bir hesap kaynak okur. |
> | Eylem | Microsoft.NetApp/netAppAccounts/write | Bir hesap kaynak yazar. |
> | Eylem | Microsoft.NetApp/Operations/read | Bir işlem kaynaklarını okur. |

## <a name="microsoftnetwork"></a>Microsoft.Network

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Network/applicationGatewayAvailableSslOptions/predefinedPolicies/read | Uygulama ağ geçidi Ssl İlkesi önceden tanımlanmış |
> | Eylem | Microsoft.Network/applicationGatewayAvailableSslOptions/read | Uygulama ağ geçidi kullanılabilir Ssl seçenekleri |
> | Eylem | Microsoft.Network/applicationGatewayAvailableWafRuleSets/read | Uygulama ağ geçidi kullanılabilir Waf kural kümesi alır |
> | Eylem | Microsoft.Network/applicationGateways/backendAddressPools/join/action | Bir uygulama ağ geçidi arka uç adres havuzuna katılır |
> | Eylem | Microsoft.Network/applicationGateways/backendhealth/action | Bir uygulama ağ geçidi arka uç durumunu alır |
> | Eylem | Microsoft.Network/applicationGateways/delete | Bir uygulama ağ geçidini siler |
> | Eylem | Microsoft.Network/applicationGateways/effectiveNetworkSecurityGroups/action | Uygulama ağ geçidi üzerinde yapılandırılmış yönlendirme tablosunu Al |
> | Eylem | Microsoft.Network/applicationGateways/effectiveRouteTable/action | Uygulama ağ geçidi üzerinde yapılandırılmış yönlendirme tablosunu Al |
> | Eylem | Microsoft.Network/applicationGateways/providers/Microsoft.Insights/logDefinitions/read | Uygulama ağ geçidi olaylarını alır |
> | Eylem | Microsoft.Network/applicationGateways/providers/Microsoft.Insights/metricDefinitions/read | Uygulama ağ geçidi için kullanılabilir ölçümleri alır |
> | Eylem | Microsoft.Network/applicationGateways/read | Bir uygulama ağ geçidini alır |
> | Eylem | Microsoft.Network/applicationGateways/setSecurityCenterConfiguration/action | Ayarlar uygulama ağ geçidi Güvenlik Merkezi yapılandırma |
> | Eylem | Microsoft.Network/applicationGateways/start/action | Bir uygulama ağ geçidi başlatır |
> | Eylem | Microsoft.Network/applicationGateways/stop/action | Bir uygulama ağ geçidi durdurur |
> | Eylem | Microsoft.Network/applicationGateways/write | Bir uygulama ağ geçidi oluşturur veya bir uygulama ağ geçidini güncelleştirir |
> | Eylem | Microsoft.Network/applicationSecurityGroups/delete | Bir uygulama güvenlik grubunu siler |
> | Eylem | Microsoft.Network/applicationSecurityGroups/joinIpConfiguration/action | Bir IP yapılandırması uygulama güvenlik gruplarına birleştirir. |
> | Eylem | Microsoft.Network/applicationSecurityGroups/joinNetworkSecurityRule/action | Güvenlik kuralı uygulama güvenlik gruplarına birleştirir. |
> | Eylem | Microsoft.Network/applicationSecurityGroups/read | Bir uygulama güvenlik grubu kimliği alır. |
> | Eylem | Microsoft.Network/applicationSecurityGroups/write | Uygulama güvenlik grubu oluşturur veya mevcut bir uygulama güvenlik grubuna güncelleştirir. |
> | Eylem | Microsoft.Network/bgpServiceCommunities/read | BGP hizmet toplulukları Al |
> | Eylem | Microsoft.Network/checkTrafficManagerNameAvailability/action | Trafik Yöneticisi göreli DNS adı kullanılabilirliğini denetler. |
> | Eylem | Microsoft.Network/connections/delete | Deletes VirtualNetworkGatewayConnection |
> | Eylem | Microsoft.Network/connections/providers/Microsoft.Insights/diagnosticSettings/read | Bağlantılar için tanılama ayarlarını alır |
> | Eylem | Microsoft.Network/connections/providers/Microsoft.Insights/diagnosticSettings/write | Oluşturur veya güncelleştirir bağlantılar için tanılama ayarları |
> | Eylem | Microsoft.Network/connections/providers/Microsoft.Insights/metricDefinitions/read | Bağlantılar için ölçüm tanımlarını alır |
> | Eylem | Microsoft.Network/connections/read | Gets VirtualNetworkGatewayConnection |
> | Eylem | Microsoft.Network/connections/sharedkey/action | ConnectionType(hypernet/routebased) türündeki VirtualNetworkGatewayConnection SharedKey Al |
> | Eylem | Microsoft.Network/connections/sharedKey/read | Gets VirtualNetworkGatewayConnection SharedKey |
> | Eylem | Microsoft.Network/connections/sharedKey/write | Oluşturur veya mevcut bir ConnectionType(hypernet/routebased) türündeki VirtualNetworkGatewayConnection SharedKey güncelleştirir |
> | Eylem | Microsoft.Network/connections/vpndeviceconfigurationscript/action | ConnectionType(hypernet/routebased) türündeki VirtualNetworkGatewayConnection VPN cihazı yapılandırmasını alır |
> | Eylem | Microsoft.Network/connections/write | Oluşturur veya mevcut bir ConnectionType(hypernet/routebased) türündeki VirtualNetworkGatewayConnection güncelleştirir |
> | Eylem | Microsoft.Network/ddosProtectionPlans/ddosProtectionPlanProxies/delete | DDoS koruması planı Proxy siler |
> | Eylem | Microsoft.Network/ddosProtectionPlans/ddosProtectionPlanProxies/read | DDoS koruması planlama Proxy tanımını alır |
> | Eylem | Microsoft.Network/ddosProtectionPlans/ddosProtectionPlanProxies/write | DDoS koruması planlama bir Proxy veya güncelleştirmeleri ve varolan DDoS koruması planlama Proxy oluşturur |
> | Eylem | Microsoft.Network/ddosProtectionPlans/delete | DDoS koruması planı siler |
> | Eylem | Microsoft.Network/ddosProtectionPlans/join/action | DDoS koruması planı birleştirir |
> | Eylem | Microsoft.Network/ddosProtectionPlans/read | DDoS koruması planı alır |
> | Eylem | Microsoft.Network/ddosProtectionPlans/write | DDoS koruması Plan oluşturur veya DDoS koruması Plan güncelleştirir  |
> | Eylem | Microsoft.Network/dnsoperationresults/read | DNS İşlem sonuçlarını alır |
> | Eylem | Microsoft.Network/dnsoperationstatuses/read | Bir DNS işlemin durumunu alır  |
> | Eylem | Microsoft.Network/dnszones/A/delete | Bir verilen ad kayıt kümesini kaldırın ve DNS bölgesinden ' A' yazın. |
> | Eylem | Microsoft.Network/dnszones/A/read | 'A' türündeki kayıt kümesini JSON biçiminde alın. Kayıt kümesi kayıtları yanı sıra TTL, etiketler ve etag'in listesini içerir. |
> | Eylem | Microsoft.Network/dnszones/A/write | Oluşturun veya bir DNS bölgesi içinde 'A' türündeki bir kayıt kümesini güncelleştirin. Belirtilen kayıtlar kayıt kümesindeki geçerli kayıt yerini alır. |
> | Eylem | Microsoft.Network/dnszones/AAAA/delete | Bir verilen ad kayıt kümesini kaldırın ve DNS bölgesinden ' AAAA' yazın. |
> | Eylem | Microsoft.Network/dnszones/AAAA/read | 'AAAA' türündeki kayıt kümesini JSON biçiminde alın. Kayıt kümesi kayıtları yanı sıra TTL, etiketler ve etag'in listesini içerir. |
> | Eylem | Microsoft.Network/dnszones/AAAA/write | Oluşturun veya bir DNS bölgesi içinde 'AAAA' türündeki bir kayıt kümesini güncelleştirin. Belirtilen kayıtlar kayıt kümesindeki geçerli kayıt yerini alır. |
> | Eylem | Microsoft.Network/dnszones/all/read | DNS kayıt kümelerini türleri arasında alır |
> | Eylem | Microsoft.Network/dnszones/CAA/delete | Bir verilen ad kayıt kümesini kaldırın ve DNS bölgesinden ' CAA' yazın. |
> | Eylem | Microsoft.Network/dnszones/CAA/read | 'CAA' türündeki kayıt kümesini JSON biçiminde alın. Kayıt kümesi TTL, etiketler ve etag'in içerir. |
> | Eylem | Microsoft.Network/dnszones/CAA/write | Oluşturun veya bir DNS bölgesi içinde 'CAA' türündeki bir kayıt kümesini güncelleştirin. Belirtilen kayıtlar kayıt kümesindeki geçerli kayıt yerini alır. |
> | Eylem | Microsoft.Network/dnszones/CNAME/delete | Bir verilen ad kayıt kümesini kaldırın ve DNS bölgesinden ' CNAME' yazın. |
> | Eylem | Microsoft.Network/dnszones/CNAME/read | 'CNAME' türündeki kayıt kümesini JSON biçiminde alın. Kayıt kümesi TTL, etiketler ve etag'in içerir. |
> | Eylem | Microsoft.Network/dnszones/CNAME/write | Oluşturun veya bir DNS bölgesi içinde 'CNAME' türündeki bir kayıt kümesini güncelleştirin. Belirtilen kayıtlar kayıt kümesindeki geçerli kayıt yerini alır. |
> | Eylem | Microsoft.Network/dnszones/delete | JSON biçimindeki DNS bölgesini silin. Etiketler, etag, numberOfRecordSets ve maxNumberOfRecordSets bölge özellikleri içerir. |
> | Eylem | Microsoft.Network/dnszones/MX/delete | Bir verilen ad kayıt kümesini kaldırın ve DNS bölgesinden ' MX' yazın. |
> | Eylem | Microsoft.Network/dnszones/MX/read | 'MX' türündeki kayıt kümesini JSON biçiminde alın. Kayıt kümesi kayıtları yanı sıra TTL, etiketler ve etag'in listesini içerir. |
> | Eylem | Microsoft.Network/dnszones/MX/write | Oluşturun veya bir DNS bölgesi içinde 'MX' türündeki bir kayıt kümesini güncelleştirin. Belirtilen kayıtlar kayıt kümesindeki geçerli kayıt yerini alır. |
> | Eylem | Microsoft.Network/dnszones/NS/delete | NS türündeki DNS kayıt kümesini siler |
> | Eylem | Microsoft.Network/dnszones/NS/read | DNS NS türündeki kayıt kümesini alır |
> | Eylem | Microsoft.Network/dnszones/NS/write | Oluşturur veya DNS NS türündeki kayıt kümesini güncelleştirir |
> | Eylem | Microsoft.Network/dnszones/providers/Microsoft.Insights/diagnosticSettings/read | DNS bölgesi tanılama ayarlarını alır |
> | Eylem | Microsoft.Network/dnszones/providers/Microsoft.Insights/diagnosticSettings/write | Oluşturur veya DNS bölgesi tanılama ayarlarını güncelleştirir |
> | Eylem | Microsoft.Network/dnszones/providers/Microsoft.Insights/metricDefinitions/read | DNS bölgesi ölçüm tanımlarını alır |
> | Eylem | Microsoft.Network/dnszones/PTR/delete | Bir verilen ad kayıt kümesini kaldırın ve DNS bölgesinden ' PTR' yazın. |
> | Eylem | Microsoft.Network/dnszones/PTR/read | 'PTR' türündeki kayıt kümesini JSON biçiminde alın. Kayıt kümesi kayıtları yanı sıra TTL, etiketler ve etag'in listesini içerir. |
> | Eylem | Microsoft.Network/dnszones/PTR/write | Oluşturun veya bir DNS bölgesi içinde 'PTR' türündeki bir kayıt kümesini güncelleştirin. Belirtilen kayıtlar kayıt kümesindeki geçerli kayıt yerini alır. |
> | Eylem | Microsoft.Network/dnszones/read | JSON biçimindeki DNS bölgesini alın. Etiketler, etag, numberOfRecordSets ve maxNumberOfRecordSets bölge özellikleri içerir. Bu komutun bölge içindeki kayıt kümelerini almıyorsa unutmayın. |
> | Eylem | Microsoft.Network/dnszones/recordsets/read | DNS kayıt kümelerini türleri arasında alır |
> | Eylem | Microsoft.Network/dnszones/SOA/read | DNS SOA türündeki kayıt kümesini alır |
> | Eylem | Microsoft.Network/dnszones/SOA/write | Oluşturur veya DNS SOA türündeki kayıt kümesini güncelleştirir |
> | Eylem | Microsoft.Network/dnszones/SRV/delete | Bir verilen ad kayıt kümesini kaldırın ve DNS bölgesinden ' SRV' yazın. |
> | Eylem | Microsoft.Network/dnszones/SRV/read | 'SRV' türündeki kayıt kümesini JSON biçiminde alın. Kayıt kümesi kayıtları yanı sıra TTL, etiketler ve etag'in listesini içerir. |
> | Eylem | Microsoft.Network/dnszones/SRV/write | SRV türündeki kayıt kümesini oluştur veya güncelleştir |
> | Eylem | Microsoft.Network/dnszones/TXT/delete | Bir verilen ad kayıt kümesini kaldırın ve DNS bölgesinden ' TXT' yazın. |
> | Eylem | Microsoft.Network/dnszones/TXT/read | 'TXT' türündeki kayıt kümesini JSON biçiminde alın. Kayıt kümesi kayıtları yanı sıra TTL, etiketler ve etag'in listesini içerir. |
> | Eylem | Microsoft.Network/dnszones/TXT/write | Oluşturun veya bir DNS bölgesi içinde 'TXT' türündeki bir kayıt kümesini güncelleştirin. Belirtilen kayıtlar kayıt kümesindeki geçerli kayıt yerini alır. |
> | Eylem | Microsoft.Network/dnszones/write | Oluşturun veya bir kaynak grubunda DNS bölgesi güncelleştirin.  Bir DNS bölgesi kaynağındaki etiketleri güncelleştirmek için kullanılır. Bu komut oluşturmak veya bölge içindeki kayıt kümelerini güncelleştirmek için kullanılamaz olduğunu unutmayın. |
> | Eylem | Microsoft.Network/expressRouteCircuits/authorizations/delete | ExpressRouteCircuit yetkilendirme siler |
> | Eylem | Microsoft.Network/expressRouteCircuits/authorizations/read | ExpressRouteCircuit yetkilendirme alır |
> | Eylem | Microsoft.Network/expressRouteCircuits/authorizations/write | Oluşturur veya mevcut bir ExpressRouteCircuit yetkilendirme güncelleştirir |
> | Eylem | Microsoft.Network/expressRouteCircuits/delete | Bir ExpressRouteCircuit siler |
> | Eylem | Microsoft.Network/expressRouteCircuits/join/action | Hızlı rota devresi birleştirir |
> | Eylem | Microsoft.Network/expressRouteCircuits/peerings/arpTables/action | Bir ExpressRouteCircuit eşliği ArpTable alır |
> | Eylem | Microsoft.Network/expressRouteCircuits/peerings/connections/delete | Bir ExpressRouteCircuit bağlantısını siler |
> | Eylem | Microsoft.Network/expressRouteCircuits/peerings/connections/read | Bir ExpressRouteCircuit bağlantı alır |
> | Eylem | Microsoft.Network/expressRouteCircuits/peerings/connections/write | Oluşturur veya mevcut bir ExpressRouteCircuit bağlantı kaynağı güncelleştirir |
> | Eylem | Microsoft.Network/expressRouteCircuits/peerings/delete | Bir ExpressRouteCircuit eşliği siler |
> | Eylem | Microsoft.Network/expressRouteCircuits/peerings/providers/Microsoft.Insights/diagnosticSettings/read | ExpressRoute bağlantı hattı eşlemeleri için tanılama ayarlarını alır |
> | Eylem | Microsoft.Network/expressRouteCircuits/peerings/providers/Microsoft.Insights/diagnosticSettings/write | Oluşturur veya ExpressRoute bağlantı hattı eşlemeleri için tanılama ayarları güncelleştirir |
> | Eylem | Microsoft.Network/expressRouteCircuits/peerings/providers/Microsoft.Insights/metricDefinitions/read | ExpressRoute bağlantı hattı eşlemeler ölçüm tanımlarını alır |
> | Eylem | Microsoft.Network/expressRouteCircuits/peerings/read | Bir ExpressRouteCircuit eşliği alır |
> | Eylem | Microsoft.Network/expressRouteCircuits/peerings/routeTables/action | Bir ExpressRouteCircuit eşliği RouteTable alır |
> | Eylem | Microsoft.Network/expressRouteCircuits/peerings/routeTablesSummary/action | Bir ExpressRouteCircuit eşliği RouteTable özeti alır |
> | Eylem | Microsoft.Network/expressRouteCircuits/peerings/stats/read | Bir ExpressRouteCircuit çubuklu eşliği alır |
> | Eylem | Microsoft.Network/expressRouteCircuits/peerings/write | Oluşturur veya mevcut bir eşleme ExpressRouteCircuit güncelleştirir |
> | Eylem | Microsoft.Network/expressRouteCircuits/providers/Microsoft.Insights/diagnosticSettings/read | ExpressRoute bağlantı hatları için tanılama ayarlarını alır |
> | Eylem | Microsoft.Network/expressRouteCircuits/providers/Microsoft.Insights/diagnosticSettings/write | Oluşturur veya ExpressRoute bağlantı hatları için tanılama ayarları güncelleştirir |
> | Eylem | Microsoft.Network/expressRouteCircuits/providers/Microsoft.Insights/logDefinitions/read | ExpressRoute bağlantı hatları için olayları Al |
> | Eylem | Microsoft.Network/expressRouteCircuits/providers/Microsoft.Insights/metricDefinitions/read | ExpressRoute bağlantı hatları için ölçüm tanımlarını alır |
> | Eylem | Microsoft.Network/expressRouteCircuits/read | Bir ExpressRouteCircuit Al |
> | Eylem | Microsoft.Network/expressRouteCircuits/stats/read | Bir ExpressRouteCircuit Stat alır |
> | Eylem | Microsoft.Network/expressRouteCircuits/write | Oluşturur veya mevcut bir ExpressRouteCircuit güncelleştirir |
> | Eylem | Microsoft.Network/expressRouteCrossConnections/delete | Hızlı rota silme arası bağlantı |
> | Eylem | Microsoft.Network/expressRouteCrossConnections/join/action | Birleştirmeler hızlı rota bir bağlantıdan |
> | Eylem | Microsoft.Network/expressRouteCrossConnections/peerings/arpTables/action | Hızlı Rota bağlantısı eşliği Arp tablosu çapraz alır |
> | Eylem | Microsoft.Network/expressRouteCrossConnections/peerings/delete | Bağlantı eşliği arası bir hızlı rota siler |
> | Eylem | Microsoft.Network/expressRouteCrossConnections/peerings/read | Bağlantı eşliği arası bir hızlı rota alır |
> | Eylem | Microsoft.Network/expressRouteCrossConnections/peerings/routeTables/action | Hızlı Rota bağlantısı eşliği yol tablosu çapraz alır |
> | Eylem | Microsoft.Network/expressRouteCrossConnections/peerings/routeTableSummary/action | Hızlı Rota bağlantısı eşliği rota tablosu özeti çapraz alır |
> | Eylem | Microsoft.Network/expressRouteCrossConnections/peerings/write | Bir hızlı rota arası bağlantı eşlemesi oluşturur veya bir var olan Hızlı rota arası bağlantı eşlemesi güncelleştirir |
> | Eylem | Microsoft.Network/expressRouteCrossConnections/read | Hızlı rota alma arası bağlantı |
> | Eylem | Microsoft.Network/expressRouteCrossConnections/write | Oluşturma veya güncelleştirme hızlı rota arası bağlantı |
> | Eylem | Microsoft.Network/expressRoutePorts/delete | ExpressRoutePorts siler |
> | Eylem | Microsoft.Network/expressRoutePorts/join/action | ExpressRoutePorts birleştirir |
> | Eylem | Microsoft.Network/expressRoutePorts/ports/read | ExpressRoutePortsPort alır |
> | Eylem | Microsoft.Network/expressRoutePorts/ports/write | Oluşturur veya ExpressRoutePortsPort güncelleştirir |
> | Eylem | Microsoft.Network/expressRoutePorts/read | ExpressRoutePorts alır |
> | Eylem | Microsoft.Network/expressRoutePorts/write | Oluşturur veya ExpressRoutePorts güncelleştirir |
> | Eylem | Microsoft.Network/expressRouteServiceProviders/read | Hızlı rota hizmeti sağlayıcılarını alır |
> | Eylem | Microsoft.Network/loadBalancers/backendAddressPools/join/action | Bir yük dengeleyici arka uç adres havuzuna katılır |
> | Eylem | Microsoft.Network/loadBalancers/backendAddressPools/read | Yük Dengeleyici arka uç adres havuzu tanımını alır |
> | Eylem | Microsoft.Network/loadBalancers/delete | Bir yük dengeleyici siler |
> | Eylem | Microsoft.Network/loadBalancers/frontendIPConfigurations/read | Yük Dengeleyici ön uç IP yapılandırması tanımını alır |
> | Eylem | Microsoft.Network/loadBalancers/inboundNatPools/join/action | Katıldığı bir yük dengeleyici gelen nat havuzu |
> | Eylem | Microsoft.Network/loadBalancers/inboundNatPools/read | Bir yük dengeleyici alır gelen nat havuzu tanımını |
> | Eylem | Microsoft.Network/loadBalancers/inboundNatRules/delete | Yük Dengeleyici gelen nat kuralını siler |
> | Eylem | Microsoft.Network/loadBalancers/inboundNatRules/join/action | Bir yük dengeleyici gelen nat kuralına katılır |
> | Eylem | Microsoft.Network/loadBalancers/inboundNatRules/read | Bir yük dengeleyici alır gelen nat kuralı tanımını |
> | Eylem | Microsoft.Network/loadBalancers/inboundNatRules/write | Yük Dengeleyici gelen nat kuralı oluşturur veya mevcut bir yük dengeleyici gelen nat kuralını güncelleştirir |
> | Eylem | Microsoft.Network/loadBalancers/loadBalancingRules/read | Yük Dengeleyici Yük Dengeleme kuralı tanımını alır |
> | Eylem | Microsoft.Network/loadBalancers/networkInterfaces/read | Bir yük dengeleyici altındaki tüm ağ arabirimlerine başvuruları alır |
> | Eylem | Microsoft.Network/loadBalancers/outboundRules/read | Yük Dengeleyici giden kuralı tanımını alır |
> | Eylem | Microsoft.Network/loadBalancers/probes/join/action | Bir yük dengeleyicinin araştırmalar kullanarak sağlar. Örneğin, VM ölçek bu izni healthProbe özelliği ile kümesi araştırma başvuruda bulunabilir. |
> | Eylem | Microsoft.Network/loadBalancers/probes/read | Yük Dengeleyici araştırmasını alır |
> | Eylem | Microsoft.Network/loadBalancers/providers/Microsoft.Insights/diagnosticSettings/read | Yük Dengeleyici tanılama ayarlarını alır |
> | Eylem | Microsoft.Network/loadBalancers/providers/Microsoft.Insights/diagnosticSettings/write | Oluşturur veya yük dengeleyici tanılama ayarlarını güncelleştirir |
> | Eylem | Microsoft.Network/loadBalancers/providers/Microsoft.Insights/logDefinitions/read | Yük Dengeleyici için olayları alır |
> | Eylem | Microsoft.Network/loadBalancers/providers/Microsoft.Insights/metricDefinitions/read | Yük Dengeleyici için kullanılabilir ölçümleri alır |
> | Eylem | Microsoft.Network/loadBalancers/read | Yük Dengeleyici tanımını alır |
> | Eylem | Microsoft.Network/loadBalancers/virtualMachines/read | Bir yük dengeleyici altındaki tüm sanal makinelere başvuruları alır |
> | Eylem | Microsoft.Network/loadBalancers/write | Bir yük dengeleyici oluşturur veya mevcut bir Yük Dengeleyiciyi güncelleştirir |
> | Eylem | Microsoft.Network/localnetworkgateways/delete | LocalNetworkGateway siler |
> | Eylem | Microsoft.Network/localnetworkgateways/read | LocalNetworkGateway alır |
> | Eylem | Microsoft.Network/localnetworkgateways/write | Oluşturur veya mevcut bir LocalNetworkGateway güncelleştirir |
> | Eylem | Microsoft.Network/locations/availableDelegations/read | Kullanılabilir temsilcilerine alır |
> | Eylem | Microsoft.Network/locations/checkAcceleratedNetworkingSupport/action | Hızlandırılmış ağ destek denetler |
> | Eylem | Microsoft.Network/locations/checkDnsNameAvailability/read | DNS etiketi belirtilen konumda kullanılabilir olup olmadığını denetler |
> | Eylem | Microsoft.Network/locations/effectiveResourceOwnership/action | Etkin kaynak sahipliğini alır |
> | Eylem | Microsoft.Network/locations/operationResults/read | Bir zaman uyumsuz POST veya DELETE işleminin işlem sonucunu alır |
> | Eylem | Microsoft.Network/locations/operations/read | Zaman uyumsuz bir işlemin durumunu gösteren işlem kaynağını alır |
> | Eylem | Microsoft.Network/locations/setResourceOwnership/action | Kaynak sahipliğini ayarlar |
> | Eylem | Microsoft.Network/locations/supportedVirtualMachineSizes/read | Sanal makine boyutları alır desteklenir |
> | Eylem | Microsoft.Network/locations/usages/read | Kaynak kullanımı ölçümlerini alır |
> | Eylem | Microsoft.Network/locations/validateResourceOwnership/action | Kaynak sahipliği doğrulama |
> | Eylem | Microsoft.Network/locations/virtualNetworkAvailableEndpointServices/read | Kullanılabilir sanal ağ uç noktası hizmetlerin bir listesini alır |
> | Eylem | Microsoft.Network/networkIntentPolicies/delete | Bir ağ hedefi ilkesini siler |
> | Eylem | Microsoft.Network/networkIntentPolicies/read | Bir ağ hedefi ilke açıklamasını alır |
> | Eylem | Microsoft.Network/networkIntentPolicies/write | Bir ağ hedefi ilkesi oluşturur veya mevcut bir ağ hedefi İlkesi güncelleştirir |
> | Eylem | Microsoft.Network/networkInterfaces/delete | Bir ağ arabirimi siler |
> | Eylem | Microsoft.Network/networkInterfaces/diagnosticIdentity/read | Kaynağın tanılama kimliğini alır |
> | Eylem | Microsoft.Network/networkInterfaces/effectiveNetworkSecurityGroups/action | Ağ güvenlik grupları yapılandırılmış üzerinde ağ arabirimi, Vm Al |
> | Eylem | Microsoft.Network/networkInterfaces/effectiveRouteTable/action | Vm ağ arabiriminde yapılandırılan yönlendirme tablosunu Al |
> | Eylem | Microsoft.Network/networkInterfaces/ipconfigurations/read | Ağ arabirimi IP yapılandırması tanımını alır.  |
> | Eylem | Microsoft.Network/networkInterfaces/join/action | Bir sanal makine bir ağ arabirimiyle birleştirir |
> | Eylem | Microsoft.Network/networkInterfaces/joinViaPrivateIp/action | Bir hizmet ilişkisi aracılığıyla bir ağ arabirimi için bir kaynak birleştirir |
> | Eylem | Microsoft.Network/networkInterfaces/loadBalancers/read | Ağ arabiriminin parçası olduğu tüm yük dengeleyicileri alır |
> | Eylem | Microsoft.Network/networkInterfaces/providers/Microsoft.Insights/metricDefinitions/read | Ağ arabirimi için kullanılabilir ölçümleri alır |
> | Eylem | Microsoft.Network/networkInterfaces/read | Ağ arabirimi tanımını alır.  |
> | Eylem | Microsoft.Network/networkInterfaces/serviceAssociations/delete | Bir hizmet ilişkilendirmesini siler |
> | Eylem | Microsoft.Network/networkInterfaces/serviceAssociations/read | Hizmet ilişkilendirme tanımını alır |
> | Eylem | Microsoft.Network/networkInterfaces/serviceAssociations/validate/action | Bir hizmet ilişkilendirmesini doğrular |
> | Eylem | Microsoft.Network/networkInterfaces/serviceAssociations/write | Yeni bir hizmet ilişkilendirmesi oluşturur veya mevcut bir hizmet ilişkilendirmesini değiştirir |
> | Eylem | Microsoft.Network/networkInterfaces/write | Bir ağ arabirimi oluşturur veya mevcut bir ağ arabirimini güncelleştirir.  |
> | Eylem | Microsoft.Network/networkProfiles/delete | Bir ağ profili siler |
> | Eylem | Microsoft.Network/networkProfiles/read | Bir ağ profili alır |
> | Eylem | Microsoft.Network/networkProfiles/removeContainers/action | Kapsayıcıları kaldırır |
> | Eylem | Microsoft.Network/networkProfiles/setContainers/action | Kapsayıcıları ayarlar |
> | Eylem | Microsoft.Network/networkProfiles/setNetworkInterfaces/action | Kapsayıcı ağ arabirimleri ayarlar |
> | Eylem | Microsoft.Network/networkProfiles/write | Oluşturur veya bir ağ profili güncelleştirir |
> | Eylem | Microsoft.Network/networkSecurityGroups/defaultSecurityRules/read | Varsayılan güvenlik kuralı tanımını alır |
> | Eylem | Microsoft.Network/networkSecurityGroups/delete | Ağ güvenlik grubunu siler |
> | Eylem | Microsoft.Network/networkSecurityGroups/join/action | Ağ güvenlik grubuna katılır |
> | Eylem | Microsoft.Network/networksecuritygroups/providers/Microsoft.Insights/diagnosticSettings/read | Ağ Güvenlik Grupları Tanılama Ayarlarını alır |
> | Eylem | Microsoft.Network/networksecuritygroups/providers/Microsoft.Insights/diagnosticSettings/write | Ağ Güvenlik Grubu tanılama ayarları oluşturur veya güncelleştirir, bu işlem öngörüler kaynak sağlayıcısı tarafından desteklenir. |
> | Eylem | Microsoft.Network/networksecuritygroups/providers/Microsoft.Insights/logDefinitions/read | Ağ güvenlik grubunun olaylarını alır |
> | Eylem | Microsoft.Network/networkSecurityGroups/read | Ağ güvenlik grubu tanımını alır |
> | Eylem | Microsoft.Network/networkSecurityGroups/securityRules/delete | Bir güvenlik kuralını siler |
> | Eylem | Microsoft.Network/networkSecurityGroups/securityRules/read | Güvenlik kuralı tanımını alır |
> | Eylem | Microsoft.Network/networkSecurityGroups/securityRules/write | Bir güvenlik kuralı oluşturur veya mevcut bir güvenlik kuralını güncelleştirir |
> | Eylem | Microsoft.Network/networkSecurityGroups/write | Bir ağ güvenlik grubu oluşturur veya mevcut bir ağ güvenlik grubunu güncelleştirir |
> | Eylem | Microsoft.Network/networkWatchers/availableProvidersList/action | Tüm kullanılabilir internet hizmet sağlayıcıları belirtilen bir Azure bölgesinin döndürür. |
> | Eylem | Microsoft.Network/networkWatchers/azureReachabilityReport/action | Göreli gecikme puan için internet hizmet sağlayıcıları Azure bölgeler ile belirtilen konumdan döndürür. |
> | Eylem | Microsoft.Network/networkWatchers/configureFlowLog/action | Akış günlüğü hedef kaynak için yapılandırır. |
> | Eylem | Microsoft.Network/networkWatchers/connectionMonitors/delete | Bağlantı İzleyici siler |
> | Eylem | Microsoft.Network/networkWatchers/connectionMonitors/providers/Microsoft.Insights/diagnosticSettings/read | Bağlantı İzleyici tanılama ayarlarını al |
> | Eylem | Microsoft.Network/networkWatchers/connectionMonitors/providers/Microsoft.Insights/diagnosticSettings/write | Oluşturur veya bağlantı İzleyici tanılama ayarlarını güncelleştirir |
> | Eylem | Microsoft.Network/networkWatchers/connectionMonitors/providers/Microsoft.Insights/metricDefinitions/read | Bağlantı izleme için kullanılabilir ölçümleri alır |
> | Eylem | Microsoft.Network/networkWatchers/connectionMonitors/query/action | Belirtilen uç noktaları arasında bağlantı izleme sorgu |
> | Eylem | Microsoft.Network/networkWatchers/connectionMonitors/read | Bağlantı İzleyici ayrıntıları |
> | Eylem | Microsoft.Network/networkWatchers/connectionMonitors/start/action | Belirtilen uç noktaları arasında bağlantı İzlemeyi Başlat |
> | Eylem | Microsoft.Network/networkWatchers/connectionMonitors/stop/action | Belirtilen uç noktaları arasında bağlantı izleme durdurma/Duraklat |
> | Eylem | Microsoft.Network/networkWatchers/connectionMonitors/write | Bağlantı İzleyici oluşturur |
> | Eylem | Microsoft.Network/networkWatchers/connectivityCheck/action | Doğrudan TCP bağlantısı bir sanal makineden başka bir VM veya isteğe bağlı bir uzak sunucunun dahil olmak üzere belirli bir uç nokta oluşturma olasılığını doğrular. |
> | Eylem | Microsoft.Network/networkWatchers/delete | Ağ İzleyicisi siler |
> | Eylem | Microsoft.Network/networkWatchers/ipFlowVerify/action | Paket izin verilen veya için veya belirli bir hedef reddedildi döndürür. |
> | Eylem | Microsoft.Network/networkWatchers/lenses/delete | Odakta siler |
> | Eylem | Microsoft.Network/networkWatchers/lenses/query/action | Sorgu belirtilen bir uç noktada ağ trafiğini izleme |
> | Eylem | Microsoft.Network/networkWatchers/lenses/read | Mercek ayrıntıları |
> | Eylem | Microsoft.Network/networkWatchers/lenses/start/action | Belirtilen bir uç noktada ağ trafiğini izleme Başlat |
> | Eylem | Microsoft.Network/networkWatchers/lenses/stop/action | Belirtilen bir uç noktada ağ trafiğini izleme durdurma/Duraklat |
> | Eylem | Microsoft.Network/networkWatchers/lenses/write | Odakta oluşturur |
> | Eylem | Microsoft.Network/networkWatchers/nextHop/action | Belirtilen hedef ve hedef IP adresi, sonraki atlama türü dönün ve sonraki IP adresi umuyoruz. |
> | Eylem | Microsoft.Network/networkWatchers/packetCaptures/delete | Paket yakalama siler |
> | Eylem | Microsoft.Network/networkWatchers/packetCaptures/queryStatus/action | Özellikleri ve paket yakalama kaynak durumu hakkındaki bilgileri alır. |
> | Eylem | Microsoft.Network/networkWatchers/packetCaptures/read | Paket yakalama tanımını Al |
> | Eylem | Microsoft.Network/networkWatchers/packetCaptures/stop/action | Çalışan paket yakalama oturumunu durdurun. |
> | Eylem | Microsoft.Network/networkWatchers/packetCaptures/write | Paket yakalama oluşturur |
> | Eylem | Microsoft.Network/networkWatchers/queryFlowLogStatus/action | Akış bir kaynakta oturum durumunu alır. |
> | Eylem | Microsoft.Network/networkWatchers/queryTroubleshootResult/action | Sorun giderme sonucun daha önce çalışan veya şu anda çalışan sorun giderme işlemi alır. |
> | Eylem | Microsoft.Network/networkWatchers/read | Ağ İzleyicisi tanımını Al |
> | Eylem | Microsoft.Network/networkWatchers/securityGroupView/action | Bir VM üzerinde uygulanan yapılandırılmış ve etkili ağ güvenlik grubu kural görüntüleyin. |
> | Eylem | Microsoft.Network/networkWatchers/topology/action | Ağ düzeyinde görünümünü kaynakları ve ilişkilerini bir kaynak grubunda alır. |
> | Eylem | Microsoft.Network/networkWatchers/troubleshoot/action | Bir ağ kaynağına Azure sorun giderme başlar. |
> | Eylem | Microsoft.Network/networkWatchers/write | Ağ İzleyicisi oluşturur veya mevcut bir Ağ İzleyicisi'ni güncelleştirir |
> | Eylem | Microsoft.Network/operations/read | Kullanılabilir işlemleri Al |
> | Eylem | Microsoft.Network/publicIPAddresses/delete | Bir ortak IP adresini siler. |
> | Eylem | Microsoft.Network/publicIPAddresses/join/action | Bir genel ip adresine katılır |
> | Eylem | Microsoft.Network/publicIPAddresses/loadBalancerPools/delete | Bir ortak IP adresini yük dengeleyici arka uç havuzu siler |
> | Eylem | Microsoft.Network/publicIPAddresses/loadBalancerPools/join/action | Ortak IP adresini yük dengeleyici arka uç havuzuna katılır |
> | Eylem | Microsoft.Network/publicIPAddresses/loadBalancerPools/read | Ortak IP adresini yük dengeleyici arka uç havuzu tanımını alır |
> | Eylem | Microsoft.Network/publicIPAddresses/loadBalancerPools/write | Bir ortak IP adresini yük dengeleyici arka uç havuzu oluşturur veya mevcut bir ortak IP adresini yük dengeleyici arka uç havuzu güncelleştirir |
> | Eylem | Microsoft.Network/publicIPAddresses/providers/Microsoft.Insights/diagnosticSettings/read | Ortak IP adresi tanılama ayarlarını al |
> | Eylem | Microsoft.Network/publicIPAddresses/providers/Microsoft.Insights/diagnosticSettings/write | Oluşturma veya genel IP adresi tanılama ayarlarını güncelleştirme |
> | Eylem | Microsoft.Network/publicIPAddresses/providers/Microsoft.Insights/logDefinitions/read | Ortak IP adresinin günlüğü tanımlarını Al |
> | Eylem | Microsoft.Network/publicIPAddresses/providers/Microsoft.Insights/metricDefinitions/read | Ortak IP adresinin ölçümleri tanımlarını Al |
> | Eylem | Microsoft.Network/publicIPAddresses/read | Bir genel ip adresi tanımını alır. |
> | Eylem | Microsoft.Network/publicIPAddresses/write | Bir ortak IP adresi oluşturur veya mevcut bir ortak IP adresini güncelleştirir.  |
> | Eylem | Microsoft.Network/register/action | Aboneliği kaydeder |
> | Eylem | Microsoft.Network/routeFilters/delete | Bir rota filtre tanımını siler |
> | Eylem | Microsoft.Network/routeFilters/join/action | Bir rota filtre birleştirir |
> | Eylem | Microsoft.Network/routeFilters/read | Bir rota filtre tanımını alır |
> | Eylem | Microsoft.Network/routeFilters/routeFilterRules/delete | Bir rota filtre kuralı tanımını siler |
> | Eylem | Microsoft.Network/routeFilters/routeFilterRules/read | Bir rota filtre kuralı tanımını alır |
> | Eylem | Microsoft.Network/routeFilters/routeFilterRules/write | Bir rota filtre kuralı oluşturur veya mevcut bir rota filtre kuralını güncelleştirir |
> | Eylem | Microsoft.Network/routeFilters/write | Bir rota filtre oluşturur veya varolan bir yol filtreyi güncelleştirir |
> | Eylem | Microsoft.Network/routeTables/delete | Yol tablosu tanımını siler |
> | Eylem | Microsoft.Network/routeTables/join/action | Bir yol tablosu birleştirir |
> | Eylem | Microsoft.Network/routeTables/read | Yol tablosu tanımını alır |
> | Eylem | Microsoft.Network/routeTables/routes/delete | Bir rota tanımını siler |
> | Eylem | Microsoft.Network/routeTables/routes/read | Bir rota tanımını alır |
> | Eylem | Microsoft.Network/routeTables/routes/write | Bir yol oluşturur veya mevcut bir yolu güncelleştirir |
> | Eylem | Microsoft.Network/routeTables/write | Bir yol tablosu oluşturur veya mevcut bir yol tablosunu güncelleştirir |
> | Eylem | Microsoft.Network/securegateways/applicationRuleCollections/delete | Bir uygulama kural koleksiyonu için güvenli bir ağ geçidi siler |
> | Eylem | Microsoft.Network/securegateways/applicationRuleCollections/read | Verilen güvenli ağ geçidi için bir uygulama kuralı koleksiyonu alma |
> | Eylem | Microsoft.Network/securegateways/applicationRuleCollections/write | Oluşturur veya güvenli bir ağ geçidi için bir uygulama kuralı koleksiyonu güncelleştirir |
> | Eylem | Microsoft.Network/securegateways/delete | Güvenli ağ geçidini Sil |
> | Eylem | Microsoft.Network/securegateways/networkRuleCollections/delete | Bir ağ kural koleksiyonu için güvenli bir ağ geçidi siler |
> | Eylem | Microsoft.Network/securegateways/networkRuleCollections/read | Verilen güvenli ağ geçidi için bir ağ kural koleksiyonu alma |
> | Eylem | Microsoft.Network/securegateways/networkRuleCollections/write | Oluşturur veya bir ağ kural koleksiyonu için güvenli bir ağ geçidi güncelleştirir |
> | Eylem | Microsoft.Network/securegateways/providers/Microsoft.Insights/logDefinitions/read | Azure Güvenlik Duvarı için olayları alır |
> | Eylem | Microsoft.Network/securegateways/read | Güvenli ağ geçidi Al |
> | Eylem | Microsoft.Network/securegateways/write | Oluşturur veya güvenli bir ağ geçidi güncelleştirir |
> | Eylem | Microsoft.Network/serviceEndpointPolicies/delete | Hizmet uç noktası İlkesi siler |
> | Eylem | Microsoft.Network/serviceEndpointPolicies/join/action | Hizmet uç noktası İlkesi birleştirir |
> | Eylem | Microsoft.Network/serviceEndpointPolicies/joinSubnet/action | Bir alt ağ hizmeti uç noktası ilkeleri birleştirir |
> | Eylem | Microsoft.Network/serviceEndpointPolicies/read | Hizmet uç noktası İlkesi açıklamasını alır |
> | Eylem | Microsoft.Network/serviceEndpointPolicies/serviceEndpointPolicyDefinitions/delete | Hizmet uç noktası ilke tanımını siler |
> | Eylem | Microsoft.Network/serviceEndpointPolicies/serviceEndpointPolicyDefinitions/read | Bir hizmet uç noktası ilke tanımı tanımı alır |
> | Eylem | Microsoft.Network/serviceEndpointPolicies/serviceEndpointPolicyDefinitions/write | Bir hizmet uç noktası ilke tanımı oluşturur veya mevcut bir hizmet uç noktası ilke tanımı güncelleştirmeleri |
> | Eylem | Microsoft.Network/serviceEndpointPolicies/write | Hizmet uç noktası ilkesi oluşturur veya mevcut bir hizmet uç noktası İlkesi güncelleştirir |
> | Eylem | Microsoft.Network/trafficManagerGeographicHierarchies/read | Trafik Yöneticisi Geographic coğrafi trafik yönlendirme yöntemini ile kullanılabilen bölgeler içeren hiyerarşisi alır |
> | Eylem | Microsoft.Network/trafficManagerProfiles/azureEndpoints/delete | Azure uç noktası var olan bir Traffic Manager profilden siler. Trafik Yöneticisi yönlendirme trafiği Azure uç noktası silindi durdurur. |
> | Eylem | Microsoft.Network/trafficManagerProfiles/azureEndpoints/read | Bir Azure Traffic Manager bu Azure uç tüm özelliklerini de dahil olmak üzere bir profiline ait uç nokta alır. |
> | Eylem | Microsoft.Network/trafficManagerProfiles/azureEndpoints/write | Var olan bir Traffic Manager profilini içinde yeni bir Azure uç noktası ekleyin veya mevcut bir Azure uç noktası bu trafik Yöneticisi profili içinde özelliklerini güncelleştirin. |
> | Eylem | Microsoft.Network/trafficManagerProfiles/delete | Trafik Yöneticisi profilini silin. Trafik Yöneticisi profili ile ilişkilendirilmiş tüm ayarlar kaybedilir ve profil artık trafiği yönlendirmek için kullanılabilir. |
> | Eylem | Microsoft.Network/trafficManagerProfiles/externalEndpoints/delete | Dış uç noktası var olan bir Traffic Manager profilden siler. Trafik Yöneticisi uç noktası silindi dış yönlendirme trafik durur. |
> | Eylem | Microsoft.Network/trafficManagerProfiles/externalEndpoints/read | Dış bir trafik Yöneticisi bu dış uç tüm özelliklerini de dahil olmak üzere profiline ait olan uç noktası alır. |
> | Eylem | Microsoft.Network/trafficManagerProfiles/externalEndpoints/write | Var olan bir Traffic Manager profilini içinde yeni bir dış uç noktası ekleyin veya mevcut bir dış uç noktası bu trafik Yöneticisi profili içinde özelliklerini güncelleştirin. |
> | Eylem | Microsoft.Network/trafficManagerProfiles/heatMaps/read | Trafik Yöneticisi ısı Haritası konumunu ve kaynak IP sorgu sayısı ve gecikme verileri içeren verilen trafik Yöneticisi profili alır. |
> | Eylem | Microsoft.Network/trafficManagerProfiles/nestedEndpoints/delete | İç içe geçmiş bir uç nokta var olan bir Traffic Manager profilden siler. Trafik Yöneticisi yönlendirme trafiğini silinen iç içe geçmiş uç durdurur. |
> | Eylem | Microsoft.Network/trafficManagerProfiles/nestedEndpoints/read | Bir iç içe trafik Yöneticisi, iç içe geçmiş Endpoint tüm özelliklerini de dahil olmak üzere bir profiline ait uç nokta alır. |
> | Eylem | Microsoft.Network/trafficManagerProfiles/nestedEndpoints/write | Var olan bir Traffic Manager profilini içinde yeni bir iç içe geçmiş uç noktası ekleyin veya mevcut bir iç içe geçmiş uç noktası bu trafik Yöneticisi profili içinde özelliklerini güncelleştirin. |
> | Eylem | Microsoft.Network/trafficManagerProfiles/providers/Microsoft.Insights/diagnosticSettings/read | Trafik Yöneticisi tanılama ayarlarını alır |
> | Eylem | Microsoft.Network/trafficManagerProfiles/providers/Microsoft.Insights/diagnosticSettings/write | Oluşturur veya güncelleştirir trafik Yöneticisi tanılama ayarları, bu işlem ınsights kaynak sağlayıcı tarafından ınsights olur. |
> | Eylem | Microsoft.Network/trafficManagerProfiles/providers/Microsoft.Insights/logDefinitions/read | Trafik Yöneticisi için olayları alır |
> | Eylem | Microsoft.Network/trafficManagerProfiles/providers/Microsoft.Insights/metricDefinitions/read | Trafik Yöneticisi için kullanılabilir ölçümleri alır. |
> | Eylem | Microsoft.Network/trafficManagerProfiles/read | Trafik Yöneticisi Profil yapılandırmasını alın. Bu, DNS ayarlarını, trafik yönlendirme ayarlarını, uç nokta izleme ayarlarını ve bu trafik Yöneticisi profili tarafından yönlendirilen uç noktaların listesini içerir. |
> | Eylem | Microsoft.Network/trafficManagerProfiles/write | Trafik Yöneticisi profili oluşturun veya var olan bir Traffic Manager profilini yapılandırmasını değiştirin.<br>Bu, etkinleştirme veya bir profili devre dışı bırakma ve DNS ayarlarını, trafik yönlendirme ayarlarını veya uç nokta izleme ayarlarını değiştirme içerir.<br>Trafik Yöneticisi profili tarafından yönlendirilen uç noktaların eklenmiş, kaldırılmış, devre dışı veya etkinleştirilebilir. |
> | Eylem | Microsoft.Network/trafficManagerUserMetricsKeys/delete | Gerçek zamanlı kullanıcı ölçümleri koleksiyonu için kullanılan abonelik düzeyinde anahtarı siler. |
> | Eylem | Microsoft.Network/trafficManagerUserMetricsKeys/read | Gerçek zamanlı kullanıcı ölçümleri koleksiyonu için kullanılan abonelik düzeyinde anahtarını alır. |
> | Eylem | Microsoft.Network/trafficManagerUserMetricsKeys/write | Gerçek zamanlı kullanıcı ölçümleri koleksiyonu için kullanılacak yeni bir abonelik düzeyi anahtar oluşturur. |
> | Eylem | Microsoft.Network/unregister/action | Abonelik kaydını siler |
> | Eylem | Microsoft.Network/virtualHubs/delete | Sanal bir hub'ı siler |
> | Eylem | Microsoft.Network/virtualHubs/hubVirtualNetworkConnections/delete | Bir HubVirtualNetworkConnection siler |
> | Eylem | Microsoft.Network/virtualHubs/hubVirtualNetworkConnections/read | Bir HubVirtualNetworkConnection Al |
> | Eylem | Microsoft.Network/virtualHubs/hubVirtualNetworkConnections/write | Bir HubVirtualNetworkConnection güncelle |
> | Eylem | Microsoft.Network/virtualHubs/read | Sanal bir hub'ı alın |
> | Eylem | Microsoft.Network/virtualHubs/write | Oluşturma veya sanal bir hub'ı güncelleştirme |
> | Eylem | Microsoft.Network/virtualnetworkgateways/Connections/Read | Get VirtualNetworkGatewayConnection |
> | Eylem | Microsoft.Network/virtualNetworkGateways/delete | Bir virtualNetworkGateway siler |
> | Eylem | Microsoft.Network/virtualnetworkgateways/generatevpnclientpackage/Action | VirtualNetworkGateway için VpnClient paketi oluştur |
> | Eylem | Microsoft.Network/virtualnetworkgateways/generatevpnprofile/Action | VirtualNetworkGateway için VpnProfile paketi oluştur |
> | Eylem | Microsoft.Network/virtualnetworkgateways/getadvertisedroutes/Action | Yollar alır virtualNetworkGateway tanıtılan |
> | Eylem | Microsoft.Network/virtualnetworkgateways/getbgppeerstatus/Action | VirtualNetworkGateway bgp eş durumunu alır |
> | Eylem | Microsoft.Network/virtualnetworkgateways/getlearnedroutes/Action | Virtualnetworkgateway öğrenilen rotalar alır |
> | Eylem | Microsoft.Network/virtualnetworkgateways/getvpnclientipsecparameters/Action | Vpnclient IPSec parametreleri için VirtualNetworkGateway P2S istemcisi alın. |
> | Eylem | Microsoft.Network/virtualnetworkgateways/getvpnprofilepackageurl/Action | Önceden oluşturulmuş vpn istemci profilini paketi URL'sini alır |
> | Eylem | Microsoft.Network/virtualNetworkGateways/providers/Microsoft.Insights/diagnosticSettings/read | Sanal ağ geçidi tanılama ayarlarını alır |
> | Eylem | Microsoft.Network/virtualNetworkGateways/providers/Microsoft.Insights/diagnosticSettings/write | Oluşturur veya güncelleştirir sanal ağ geçidi tanılama ayarları, bu işlem ınsights kaynak sağlayıcı tarafından ınsights olur. |
> | Eylem | Microsoft.Network/virtualNetworkGateways/providers/Microsoft.Insights/logDefinitions/read | Sanal ağ geçidi için olayları alır |
> | Eylem | Microsoft.Network/virtualNetworkGateways/providers/Microsoft.Insights/metricDefinitions/read | Sanal ağ geçidi için kullanılabilir ölçümleri alır |
> | Eylem | Microsoft.Network/virtualNetworkGateways/read | Bir VirtualNetworkGateway alır |
> | Eylem | Microsoft.Network/virtualnetworkgateways/Reset/Action | Bir virtualNetworkGateway sıfırlar |
> | Eylem | Microsoft.Network/virtualnetworkgateways/setvpnclientipsecparameters/Action | Vpnclient IPSec VirtualNetworkGateway P2S istemci parametrelerini ayarlayın. |
> | Eylem | Microsoft.Network/virtualnetworkgateways/supportedvpndevices/action | Vpn cihazları listeler desteklenir |
> | Eylem | Microsoft.Network/virtualNetworkGateways/write | Oluşturur veya bir VirtualNetworkGateway güncelleştirir |
> | Eylem | Microsoft.Network/virtualNetworks/checkIpAddressAvailability/read | Belirtilen sanal ağ IP adresi olup olmadığını denetleyin |
> | Eylem | Microsoft.Network/virtualNetworks/customViews/get/action | Bir sanal ağ özel görünüm içeriğini alma |
> | Eylem | Microsoft.Network/virtualNetworks/customViews/read | Sanal ağ özel görünüm tanımını Al |
> | Eylem | Microsoft.Network/virtualNetworks/delete | Bir sanal ağ siler |
> | Eylem | Microsoft.Network/virtualNetworks/peer/action | Başka bir sanal ağ ile sanal ağ eşleri |
> | Eylem | Microsoft.Network/virtualNetworks/providers/Microsoft.Insights/diagnosticSettings/read | Sanal Ağ Tanılama ayarlarını al |
> | Eylem | Microsoft.Network/virtualNetworks/providers/Microsoft.Insights/diagnosticSettings/write | Oluşturma veya sanal ağ tanılama ayarlarını güncelleştirme |
> | Eylem | Microsoft.Network/virtualNetworks/providers/Microsoft.Insights/logDefinitions/read | Sanal ağ günlüğü tanımlarını Al |
> | Eylem | Microsoft.Network/virtualNetworks/read | Sanal ağ tanımını Al |
> | Eylem | Microsoft.Network/virtualNetworks/remoteVirtualNetworkPeeringProxies/delete | Bir sanal ağ eşleme proxy siler |
> | Eylem | Microsoft.Network/virtualNetworks/remoteVirtualNetworkPeeringProxies/read | Bir sanal ağ proxy tanımı eşliği alır |
> | Eylem | Microsoft.Network/virtualNetworks/remoteVirtualNetworkPeeringProxies/write | Bir sanal ağ eşleme proxy oluşturur veya mevcut bir sanal ağ eşleme proxy güncelleştirir |
> | Eylem | Microsoft.Network/virtualNetworks/subnets/delete | Bir sanal ağ alt ağını siler |
> | Eylem | Microsoft.Network/virtualNetworks/subnets/join/action | Bir sanal ağ birleştirir |
> | Eylem | Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Kaynak depolama hesabı veya SQL veritabanı gibi bir alt ağa birleştirir. |
> | Eylem | Microsoft.Network/virtualNetworks/subnets/read | Bir sanal ağ alt ağı tanımını alır |
> | Eylem | Microsoft.Network/virtualNetworks/subnets/resourceNavigationLinks/delete | Bir kaynak Gezinti bağlantısını siler |
> | Eylem | Microsoft.Network/virtualNetworks/subnets/resourceNavigationLinks/read | Kaynak Gezinti bağlantısı tanımı Al |
> | Eylem | Microsoft.Network/virtualNetworks/subnets/resourceNavigationLinks/write | Bir kaynak Gezinti bağlantısı oluşturur veya varolan bir kaynak Gezinti bağlantısına güncelleştirir |
> | Eylem | Microsoft.Network/virtualNetworks/subnets/serviceAssociationLinks/delete | Bir hizmet ilişkilendirme bağlantısını siler |
> | Eylem | Microsoft.Network/virtualNetworks/subnets/serviceAssociationLinks/details/read | Hizmet ilişkilendirme bağlantı ayrıntı tanımını alır |
> | Eylem | Microsoft.Network/virtualNetworks/subnets/serviceAssociationLinks/read | Bir hizmet ilişkilendirme bağlantısı tanımını alır |
> | Eylem | Microsoft.Network/virtualNetworks/subnets/serviceAssociationLinks/validate/action | Bir hizmet ilişkilendirme bağlantısı doğrular |
> | Eylem | Microsoft.Network/virtualNetworks/subnets/serviceAssociationLinks/write | Bir hizmet ilişkilendirme bağlantısı oluşturur veya varolan bir hizmet ilişkilendirme bağlantısına güncelleştirir |
> | Eylem | Microsoft.Network/virtualNetworks/subnets/virtualMachines/read | Bir sanal ağ alt ağında tüm sanal makinelere başvuruları alır |
> | Eylem | Microsoft.Network/virtualNetworks/subnets/write | Bir sanal ağ alt ağı oluşturur veya varolan bir sanal ağ alt ağı güncelleştirir |
> | Eylem | Microsoft.Network/virtualNetworks/taggedTrafficConsumers/delete | Etiketli trafiği tüketici siler |
> | Eylem | Microsoft.Network/virtualNetworks/taggedTrafficConsumers/read | Etiketli trafiği tüketici tanımını Al |
> | Eylem | Microsoft.Network/virtualNetworks/taggedTrafficConsumers/validate/action | Etiketli trafiği tüketici doğrular |
> | Eylem | Microsoft.Network/virtualNetworks/taggedTrafficConsumers/write | Etiketli trafiği tüketici oluşturur veya mevcut bir etiketli trafiği tüketici güncelleştirir |
> | Eylem | Microsoft.Network/virtualNetworks/usages/read | Her sanal ağ alt ağı için IP kullanımları Al |
> | Eylem | Microsoft.Network/virtualNetworks/virtualMachines/read | Bir sanal ağ tüm sanal makinelere başvuruları alır |
> | Eylem | Microsoft.Network/virtualNetworks/virtualNetworkPeerings/delete | Bir sanal ağ eşlemesi siler |
> | Eylem | Microsoft.Network/virtualNetworks/virtualNetworkPeerings/read | Sanal ağ eşleme tanımını alır |
> | Eylem | Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write | Bir sanal ağ eşlemesi oluşturur veya mevcut bir sanal ağ eşlemesi güncelleştirir |
> | Eylem | Microsoft.Network/virtualNetworks/write | Bir sanal ağ oluşturur veya mevcut bir sanal ağı güncelleştirir |
> | Eylem | Microsoft.Network/virtualNetworkTaps/delete | Sanal ağ Tap Sil |
> | Eylem | Microsoft.Network/virtualNetworkTaps/join/action | Bir sanal ağ tap birleştirir |
> | Eylem | Microsoft.Network/virtualNetworkTaps/read | Sanal ağ Tap Al |
> | Eylem | Microsoft.Network/virtualNetworkTaps/write | Sanal ağ Tap güncelle |
> | Eylem | Microsoft.Network/virtualWans/delete | Wan sanal bir siler |
> | Eylem | Microsoft.Network/virtualWans/read | Sanal bir GET Wan |
> | Eylem | Microsoft.Network/virtualWans/virtualHubProxies/delete | Bir sanal Hub proxy siler |
> | Eylem | Microsoft.Network/virtualWans/virtualHubProxies/read | Bir sanal Hub proxy tanımını alır |
> | Eylem | Microsoft.Network/virtualWans/virtualHubProxies/write | Bir sanal Hub proxy oluşturur veya bir sanal Hub proxy güncelleştirir |
> | Eylem | Microsoft.Network/virtualWans/virtualHubs/read | Tüm sanal sanal Wan başvuran hub'ları alır. |
> | Eylem | Microsoft.Network/virtualwans/vpnconfiguration/action | Bir Vpn yapılandırmasını alır |
> | Eylem | Microsoft.Network/virtualWans/vpnSiteProxies/delete | Bir Vpn sitesi proxy siler |
> | Eylem | Microsoft.Network/virtualWans/vpnSiteProxies/read | Bir Vpn Site proxy tanımını alır |
> | Eylem | Microsoft.Network/virtualWans/vpnSiteProxies/write | Bir Vpn sitesi proxy oluşturur veya bir Vpn sitesi proxy güncelleştirir |
> | Eylem | Microsoft.Network/virtualWans/vpnSites/read | Sanal Wan başvuran tüm VPN siteleri alır. |
> | Eylem | Microsoft.Network/virtualWans/write | Oluşturun veya sanal Wan güncelleştirin |
> | Eylem | Microsoft.Network/vpnGateways/delete | Bir VpnGateway siler. |
> | Eylem | Microsoft.Network/vpnGateways/read | Bir VpnGateway alır. |
> | Eylem | microsoft.network/vpnGateways/vpnConnections/read | Bir VpnConnection alır. |
> | Eylem | microsoft.network/vpnGateways/vpnConnections/write | Bir VpnConnection koyar. |
> | Eylem | Microsoft.Network/vpnGateways/write | Bir VpnGateway koyar. |
> | Eylem | Microsoft.Network/vpnsites/delete | Bir Vpn Site kaynağı siler. |
> | Eylem | Microsoft.Network/vpnsites/read | Bir Vpn Site kaynağı alır. |
> | Eylem | Microsoft.Network/vpnsites/write | Oluşturur veya bir Vpn Site kaynağı güncelleştirir. |

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.NotificationHubs/CheckNamespaceAvailability/action | Belirli bir Ad Alanı kaynak adının NotificationHub hizmetinde kullanılabilir olup olmadığını denetler. |
> | Eylem | Microsoft.NotificationHubs/Namespaces/authorizationRules/action | Ad Alanı Yetkilendirme Kuralları açıklamasının listesini alır. |
> | Eylem | Microsoft.NotificationHubs/Namespaces/authorizationRules/delete | Namespace yetkilendirme kuralını silin. Namespace yetkilendirme varsayılan kural silinemiyor.  |
> | Eylem | Microsoft.NotificationHubs/Namespaces/authorizationRules/listkeys/action | Ad Alanı için Bağlantı Dizesini alın |
> | Eylem | Microsoft.NotificationHubs/Namespaces/authorizationRules/read | Ad Alanı Yetkilendirme Kuralları açıklamasının listesini alır. |
> | Eylem | Microsoft.NotificationHubs/Namespaces/authorizationRules/regenerateKeys/action | Ad Alanı Yetkilendirme Kuralı Birincil/İkincil Anahtarı Yeniden Oluşturma, Yeniden oluşturulması gereken Anahtarı belirtin |
> | Eylem | Microsoft.NotificationHubs/Namespaces/authorizationRules/write | Namespace düzeyi yetkilendirme kuralları oluşturun ve özelliklerini güncelleştirin. Yetkilendirme kuralları erişim hakları, birincil ve ikincil anahtarları güncelleştirilebilir. |
> | Eylem | Microsoft.NotificationHubs/Namespaces/CheckNotificationHubAvailability/action | Belirli bir NotificationHub adının bir Ad Alanı içinde kullanılabilir olup olmadığını denetler. |
> | Eylem | Microsoft.NotificationHubs/Namespaces/Delete | Ad Alanı Kaynağını silin |
> | Eylem | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/action | Bildirim Hub'ı Yetkilendirme Kurallarının listesini alın |
> | Eylem | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/delete | Bildirim Hub'ı Yetkilendirme Kurallarını Sil |
> | Eylem | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/listkeys/action | Bildirim Hub'ı Bağlantı Dizesini alın |
> | Eylem | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/read | Bildirim Hub'ı Yetkilendirme Kurallarının listesini alın |
> | Eylem | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/regenerateKeys/action | Bildirim Hub'ı Yetkilendirme Kuralı Birincil/İkincil Anahtarı Yeniden Oluşturma, Yeniden oluşturulması gereken Anahtarı belirtin |
> | Eylem | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/write | Bildirim hub'ı yetkilendirme kuralları oluşturun ve özelliklerini güncelleştirin. Yetkilendirme kuralları erişim hakları, birincil ve ikincil anahtarları güncelleştirilebilir. |
> | Eylem | Microsoft.NotificationHubs/Namespaces/NotificationHubs/debugSend/action | Test amaçlı bir anında iletme bildirimi gönderin. |
> | Eylem | Microsoft.NotificationHubs/Namespaces/NotificationHubs/Delete | Bildirim Hub'ı Kaynağını Sil |
> | Eylem | Microsoft.NotificationHubs/Namespaces/NotificationHubs/metricDefinitions/read | Namespace ölçümleri kaynak açıklamaları listesini al |
> | Eylem | Microsoft.NotificationHubs/Namespaces/NotificationHubs/pnsCredentials/action | Tüm bildirim hub'ı PNS kimlik bilgilerini alın. Bu içerir, WNS, MPNS, APNS, GCM ve Baidu kimlik bilgileri |
> | Eylem | Microsoft.NotificationHubs/Namespaces/NotificationHubs/read | Bildirim Hub'ı Kaynak Açıklamalarının listesini alın |
> | Eylem | Microsoft.NotificationHubs/Namespaces/NotificationHubs/write | Bildirim hub'ı oluşturun ve özelliklerini güncelleştirin. Özelliklerini çoğunlukla PNS kimlik bilgilerini içerir. Yetkilendirme kuralları ve TTL |
> | Eylem | Microsoft.NotificationHubs/Namespaces/read | Ad Alanı Kaynak Açıklamasının listesini alır |
> | Eylem | Microsoft.NotificationHubs/Namespaces/write | Namespace kaynak oluşturun ve özelliklerini güncelleştirin. Etiketleri ve Namespace kapasitesini hangi güncelleştirilebilir özelliklerdir. |
> | Eylem | Microsoft.NotificationHubs/operationResults/read | Bildirim Hub'ları sağlayıcı için işlem sonuçlarını döndürür |
> | Eylem | Microsoft.NotificationHubs/operations/read | Bildirim Hub'ları sağlayıcı için desteklenen işlemlerin bir listesini döndürür |
> | Eylem | Microsoft.NotificationHubs/register/action | Aboneliği NotificationHubs kaynak sağlayıcısı için kaydeder ve Ad Alanları ve NotificationHubs oluşturulmasını sağlar |
> | Eylem | Microsoft.NotificationHubs/unregister/action | NotificationHubs kaynak sağlayıcısı için abonelik kaydını siler ve Ad Alanları ve NotificationHubs oluşturulmasını etkinleştirir |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.OperationalInsights/linkTargets/read | Bir Azure aboneliği ile ilişkili olmayan var olan hesapları listeler. Bu Azure aboneliği bir çalışma alanına bağlamak için bu işlem çalışma alanı Oluştur işlemi Müşteri Kimliği özelliğinde tarafından döndürülen bir müşteri kimliği kullanın. |
> | Eylem | Microsoft.OperationalInsights/register/action | Bir kaynak sağlayıcısı için bir abonelik kaydedin. |
> | Eylem | Microsoft.OperationalInsights/workspaces/analytics/query/action | Yeni altyapısını kullanarak arayın. |
> | Eylem | Microsoft.OperationalInsights/workspaces/analytics/query/schema/read | Arama şema V2 alın. |
> | Eylem | Microsoft.OperationalInsights/workspaces/api/query/action | Yeni altyapısını kullanarak arayın. |
> | Eylem | Microsoft.OperationalInsights/workspaces/api/query/schema/read | Arama şema V2 alın. |
> | Eylem | Microsoft.OperationalInsights/workspaces/configurationScopes/delete | Yapılandırma kapsamını Sil |
> | Eylem | Microsoft.OperationalInsights/workspaces/configurationScopes/read | Yapılandırma kapsam Al |
> | Eylem | Microsoft.OperationalInsights/workspaces/configurationScopes/write | Yapılandırma kapsamını ayarlama |
> | Eylem | Microsoft.OperationalInsights/workspaces/datasources/delete | Çalışma alanı altında veri kaynakları silin. |
> | Eylem | Microsoft.OperationalInsights/workspaces/datasources/read | Veri kaynakları bir çalışma alanı altında alın. |
> | Eylem | Microsoft.OperationalInsights/workspaces/datasources/write | Veri kaynakları çalışma alanı altında oluştur/güncelleştir. |
> | Eylem | Microsoft.OperationalInsights/workspaces/delete | Bir çalışma alanı siler. Çalışma alanı oluşturma zamanında mevcut bir çalışma alanına bağlı değilse bağlı çalışma silinmez. |
> | Eylem | Microsoft.OperationalInsights/workspaces/generateregistrationcertificate/action | Kayıt sertifikası için çalışma alanı oluşturur. Bu sertifika, Microsoft System Center Operation Manager çalışma alanına bağlamak için kullanılır. |
> | Eylem | Microsoft.OperationalInsights/workspaces/intelligencepacks/disable/action | Belirli bir çalışma alanı için bir destek paketi devre dışı bırakır. |
> | Eylem | Microsoft.OperationalInsights/workspaces/intelligencepacks/enable/action | Belirli bir çalışma alanı için bir destek paketi sağlar. |
> | Eylem | Microsoft.OperationalInsights/workspaces/intelligencepacks/read | Verilen worksapce için görünür olan tüm Intelligence paketlerini listeler ve paketi etkin veya bu çalışma alanı için devre dışı olup olmadığını da listeler. |
> | Eylem | Microsoft.OperationalInsights/workspaces/linkedServices/delete | Delete bağlantılı hizmetler çalışma alanında verilir. |
> | Eylem | Microsoft.OperationalInsights/workspaces/linkedServices/read | Bağlı hizmetler almak çalışma verilir. |
> | Eylem | Microsoft.OperationalInsights/workspaces/linkedServices/write | Oluşturma/bağlantılı güncelleştirme hizmetler çalışma alanında verilir. |
> | Eylem | Microsoft.OperationalInsights/workspaces/listKeys/action | Çalışma alanı için liste anahtarları alır. Bu anahtarları Microsoft operasyonel Öngörüler aracıları için çalışma alanına bağlanmak için kullanılır. |
> | Eylem | Microsoft.OperationalInsights/workspaces/listKeys/read | Çalışma alanı için liste anahtarları alır. Bu anahtarları Microsoft operasyonel Öngörüler aracıları için çalışma alanına bağlanmak için kullanılır. |
> | Eylem | Microsoft.OperationalInsights/workspaces/managementGroups/read | Bu çalışma alanına bağlı System Center Operations Manager Yönetim grupları adları ve meta veriler alır. |
> | Eylem | Microsoft.OperationalInsights/workspaces/metricDefinitions/read | Çalışma alanı altında ölçüm tanımlarını Al |
> | Eylem | Microsoft.OperationalInsights/workspaces/notificationSettings/delete | Çalışma alanı için kullanıcının bildirim ayarları silin. |
> | Eylem | Microsoft.OperationalInsights/workspaces/notificationSettings/read | Çalışma alanı için kullanıcının bildirim ayarlarını alın. |
> | Eylem | Microsoft.OperationalInsights/workspaces/notificationSettings/write | Çalışma alanı için kullanıcının bildirim ayarları ayarlayın. |
> | Eylem | Microsoft.OperationalInsights/workspaces/purge/action | Belirtilen veri çalışma alanından Sil |
> | Eylem | Microsoft.OperationalInsights/workspaces/read | Var olan bir çalışma alır |
> | Eylem | Microsoft.OperationalInsights/workspaces/savedSearches/delete | Kaydedilmiş bir arama sorgusunu siler |
> | Eylem | Microsoft.OperationalInsights/workspaces/savedSearches/read | Kaydedilmiş bir arama sorgusunu alır |
> | Eylem | Microsoft.OperationalInsights/workspaces/savedSearches/write | Kaydedilmiş bir arama sorgusunu oluşturur |
> | Eylem | Microsoft.OperationalInsights/workspaces/schema/read | Arama şeması için çalışma alır.  Arama şeması sunulan alanlar ve bunların türleri içerir. |
> | Eylem | Microsoft.OperationalInsights/workspaces/search/action | Bir arama sorgusunu çalıştırır |
> | Eylem | Microsoft.OperationalInsights/workspaces/sharedKeys/action | Çalışma alanı için paylaşılan anahtarları alır. Bu anahtarları Microsoft operasyonel Öngörüler aracıları için çalışma alanına bağlanmak için kullanılır. |
> | Eylem | Microsoft.OperationalInsights/workspaces/sharedKeys/read | Çalışma alanı için paylaşılan anahtarları alır. Bu anahtarları Microsoft operasyonel Öngörüler aracıları için çalışma alanına bağlanmak için kullanılır. |
> | Eylem | Microsoft.OperationalInsights/workspaces/storageinsightconfigs/delete | Bir depolama yapılandırması siler. Depolama hesabından veri okuma bu Microsoft operasyonel Öngörüler durdurur. |
> | Eylem | Microsoft.OperationalInsights/workspaces/storageinsightconfigs/read | Bir depolama yapılandırmasını alır. |
> | Eylem | Microsoft.OperationalInsights/workspaces/storageinsightconfigs/write | Yeni depolama yapılandırması oluşturur. Bu yapılandırmalar, varolan bir depolama hesabındaki bir konumdan veri çekmek için kullanılır. |
> | Eylem | Microsoft.OperationalInsights/workspaces/usages/read | Çalışma alanı tarafından okunan veri miktarını da dahil olmak üzere bir çalışma alanı için kullanım verilerini alır. |
> | Eylem | Microsoft.OperationalInsights/workspaces/write | Yeni çalışma alanında ya da varolan bir çalışma alanı bağlantıları varolan çalışma alanından Müşteri Kimliği sağlayarak oluşturur. |

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.OperationsManagement/managementAssociations/delete | Mevcut yönetim ilişkiyi sil |
> | Eylem | Microsoft.OperationsManagement/managementAssociations/read | Varolan yönetim ilişkilendirmesi Al |
> | Eylem | Microsoft.OperationsManagement/managementAssociations/write | Yeni bir yönetim ilişkilendirmesi oluştur |
> | Eylem | Microsoft.OperationsManagement/managementConfigurations/delete | Mevcut yönetim Configuratin Sil |
> | Eylem | Microsoft.OperationsManagement/managementConfigurations/read | Varolan yönetim yapılandırmasını alma |
> | Eylem | Microsoft.OperationsManagement/managementConfigurations/write | Yeni Yönetim yapılandırması oluştur |
> | Eylem | Microsoft.OperationsManagement/register/action | Bir kaynak sağlayıcısı için bir abonelik kaydedin. |
> | Eylem | Microsoft.OperationsManagement/solutions/delete | Varolan bir OMS çözümü Sil |
> | Eylem | Microsoft.OperationsManagement/solutions/read | OMS çözüm çıkma Al |
> | Eylem | Microsoft.OperationsManagement/solutions/write | Yeni bir OMS çözümü oluşturun |

## <a name="microsoftpolicyinsights"></a>Microsoft.PolicyInsights

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.PolicyInsights/policyEvents/queryResults/action | İlke olayları hakkındaki bilgileri sorgulayın. |
> | Eylem | Microsoft.PolicyInsights/policyStates/queryResults/action | İlke durumları hakkındaki bilgileri sorgulayın. |
> | Eylem | Microsoft.PolicyInsights/policyStates/summarize/action | İlke son durumları hakkındaki özet bilgilerini sorgulayın. |
> | Eylem | Microsoft.PolicyInsights/register/action | İlke içgörüleri kaynak sağlayıcısını kaydeder ve üzerinde eylemleri etkinleştirir. |

## <a name="microsoftportal"></a>Microsoft.Portal

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Portal/dashboards/delete | Panoyu abonelikten kaldırır. |
> | Eylem | Microsoft.Portal/dashboards/read | Abonelik için panoları okur. |
> | Eylem | Microsoft.Portal/dashboards/write | Bir aboneliğe pano ekleyin veya panoyu değiştirin. |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.PowerBIDedicated/capacities/checkNameAvailability/action | Ayrılmış Power BI kapasitesini adının geçerli olup olmadığını denetler ve içinde değil kullanın. |
> | Eylem | Microsoft.PowerBIDedicated/capacities/delete | Power BI siler kapasite ayrılmış. |
> | Eylem | Microsoft.PowerBIDedicated/capacities/providers/Microsoft.Insights/diagnosticSettings/read | Kaynak için tanılama ayarını alır |
> | Eylem | Microsoft.PowerBIDedicated/capacities/providers/Microsoft.Insights/diagnosticSettings/write | Kaynak için tanılama ayarını oluşturur veya güncelleştirir |
> | Eylem | Microsoft.PowerBIDedicated/capacities/providers/Microsoft.Insights/logDefinitions/read | Ayrılmış Power BI kapasiteleri için kullanılabilir günlüklerini alır |
> | Eylem | Microsoft.PowerBIDedicated/capacities/providers/Microsoft.Insights/metricDefinitions/read | Ayrılmış Power BI kapasitesini için kullanılabilir ölçümleri alır. |
> | Eylem | Microsoft.PowerBIDedicated/capacities/read | Belirtilen Power BI ayrılmış kapasite bilgilerini alır. |
> | Eylem | Microsoft.PowerBIDedicated/capacities/resume/action | Kapasite sürdürür. |
> | Eylem | Microsoft.PowerBIDedicated/capacities/skus/read | Kapasite için kullanılabilir SKU bilgilerini alma |
> | Eylem | Microsoft.PowerBIDedicated/capacities/suspend/action | Kapasite askıya alır. |
> | Eylem | Microsoft.PowerBIDedicated/capacities/write | Oluşturur veya belirtilen Power BI kapasitesini ayrılmış güncelleştirir. |
> | Eylem | Microsoft.PowerBIDedicated/locations/operationresults/read | Belirtilen işlem sonucu bilgileri alır. |
> | Eylem | Microsoft.PowerBIDedicated/locations/operationstatuses/read | Belirtilen işlem durumu bilgileri alır. |
> | Eylem | Microsoft.PowerBIDedicated/operations/read | İşlem bilgileri alır. |
> | Eylem | Microsoft.PowerBIDedicated/register/action | Power BI ayrılmış kaynak sağlayıcısını kaydeder. |
> | Eylem | Microsoft.PowerBIDedicated/skus/read | SKU'ları bilgileri alır. |

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp, hizmet tarafından kullanılan iç işlemdir. |
> | Eylem | Microsoft.RecoveryServices/locations/allocateStamp/action | AllocateStamp hizmeti tarafından kullanılan iç bir işlemdir |
> | Eylem | Microsoft.RecoveryServices/locations/backupPreValidateProtection/action |  |
> | Eylem | Microsoft.RecoveryServices/locations/backupStatus/action | Kurtarma Hizmetleri kasaları yedekleme durumunu denetleme |
> | Eylem | Microsoft.RecoveryServices/locations/backupValidateFeatures/action | Özellikler doğrula |
> | Eylem | Microsoft.RecoveryServices/locations/operationStatus/read | Belirli bir işlemi için işlem durumunu alır |
> | Eylem | Microsoft.RecoveryServices/operations/read | İşlemi kaynak sağlayıcısı için işlemler listesini döndürür |
> | Eylem | Microsoft.RecoveryServices/register/action | Yazmaçları aboneliği için kaynak sağlayıcısı verilen |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupconfig/read | Döndürür yapılandırma kurtarma Hizmetleri kasası. |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupconfig/write | Güncelleştirmeler yapılandırması kurtarma Hizmetleri kasası. |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupEngines/read | Kasaya kayıtlı tüm yedekleme yönetimi sunucularının listesini döndürür. |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write | Bir yedekleme koruma hedefini oluşturma |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read | İşlemin durumunu döndürür |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupFabrics/protectableContainers/read | Tüm korunabilir kapsayıcıları Al |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/delete | Kayıtlı kapsayıcısını siler |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/inquire/action | Bir kapsayıcı içindeki iş yükleri için sorgulama yapın |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/items/read | Tüm öğeleri bir kapsayıcıda Al |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/operationResults/read | Koruma Kapsayıcısı üzerinde gerçekleştirilen İşlemin sonuçlarını alır. |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/backup/action | Korumalı Öğe için Yedekleme gerçekleştirir. |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/delete | Öğe silme korumalı |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read | Korumalı Öğeler üzerinde Gerçekleştirilen İşlemin Sonuçlarını alın. |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationsStatus/read | Korumalı Öğeler üzerinde Gerçekleştirilen İşlemin durumunu döndürür. |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read | Korumalı öğe nesne ayrıntılarını döndürür |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/provisionInstantItemRecovery/action | Korumalı öğe için sağlama anlık öğe kurtarma |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read | Korumalı Öğeler için Kurtarma Noktalarını alın. |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/restore/action | Korumalı Öğeler için Kurtarma Noktalarını geri yükleyin. |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/revokeInstantItemRecovery/action | Anlık öğe kurtarma için korumalı öğe iptal etme |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write | Bir yedekleme korumalı öğesi oluşturma |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read | Tüm kayıtlı kapsayıcıları döndürür |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/write | Kayıtlı bir kapsayıcı oluşturur |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupFabrics/refreshContainers/action | Kapsayıcı listesini yeniler |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupJobs/cancel/action | İşi iptal |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupJobs/operationResults/read | İş İşleminin Sonucunu döndürür. |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupJobs/read | Tüm iş nesneleri döndürür |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Dışarı aktarma işleri |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupJobsExport/operationResults/read | Dışa aktarma işi işleminin sonucunu döndürür. |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupManagementMetaData/read | Kurtarma Hizmetleri Kasası için Yedekleme Yönetimi Meta Verilerini döndürür. |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupOperationResults/read | Kurtarma Hizmetleri Kasası için Yedekleme işleminin Sonucunu döndürür. |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupOperations/read | Yedekleme işlemi döndürür durum kurtarma Hizmetleri kasası. |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupPolicies/delete | Bir koruma ilkesini Sil |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read | İlke İşleminin Sonuçlarını alın. |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupPolicies/operations/read | İlke işlemin durumunu alın. |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupPolicies/read | Tüm koruma ilkeleri döndürür |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupPolicies/write | Koruma ilkesi oluşturur |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupProtectableItems/read | Tüm Korunabilir Öğelerin listesini döndürür. |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupProtectedItems/read | Tüm Korumalı Öğelerin listesini döndürür. |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read | Aboneliğine ait tüm kapsayıcıları döndürür |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupSecurityPIN/action | Güvenlik PIN döndürür bilgi kurtarma Hizmetleri kasası. |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupstorageconfig/read | Döndürür depolama yapılandırması kurtarma Hizmetleri kasası. |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupstorageconfig/write | Güncelleştirmeler depolama yapılandırması kurtarma Hizmetleri kasası. |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Özetleri korunan öğeler ve korunan sunucular için bir kurtarma Hizmetleri döndürür. |
> | Eylem | Microsoft.RecoveryServices/Vaults/certificates/write | Güncelleştirme kaynağı sertifika işlemi kaynak/kasa kimlik bilgileri sertifikası güncelleştirir. |
> | Eylem | Microsoft.RecoveryServices/Vaults/delete | Kasa silme işlemi 'Kasası' türünde belirtilen Azure kaynak siler |
> | Eylem | Microsoft.RecoveryServices/Vaults/extendedInformation/delete | Daha Fazla Bilgi Al işlemi, ?vault? türündeki Azure kaynağını temsil eden nesnenin Daha Fazla Bilgi değerini alır. |
> | Eylem | Microsoft.RecoveryServices/Vaults/extendedInformation/read | Daha Fazla Bilgi Al işlemi, ?vault? türündeki Azure kaynağını temsil eden nesnenin Daha Fazla Bilgi değerini alır. |
> | Eylem | Microsoft.RecoveryServices/Vaults/extendedInformation/write | Daha Fazla Bilgi Al işlemi, ?vault? türündeki Azure kaynağını temsil eden nesnenin Daha Fazla Bilgi değerini alır. |
> | Eylem | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Uyarılar için kurtarma Hizmetleri kasası alır. |
> | Eylem | Microsoft.RecoveryServices/Vaults/monitoringAlerts/write | Uyarı çözümler. |
> | Eylem | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/read | Kurtarma Hizmetleri kasası bildirim yapılandırmasını alır. |
> | Eylem | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/write | Kurtarma Hizmetleri kasası e-posta bildirimleri yapılandırır. |
> | Eylem | Microsoft.RecoveryServices/Vaults/providers/Microsoft.Insights/diagnosticSettings/read | Azure yedekleme tanılama |
> | Eylem | Microsoft.RecoveryServices/Vaults/providers/Microsoft.Insights/diagnosticSettings/write | Azure yedekleme tanılama |
> | Eylem | Microsoft.RecoveryServices/Vaults/providers/Microsoft.Insights/logDefinitions/read | Azure yedekleme günlükleri |
> | Eylem | Microsoft.RecoveryServices/Vaults/providers/Microsoft.Insights/metricDefinitions/read | Azure yedekleme ölçümleri |
> | Eylem | Microsoft.RecoveryServices/Vaults/read | Kasa alma işlemi Azure kaynak türü 'Kasası' temsil eden bir nesneyi alır |
> | Eylem | Microsoft.RecoveryServices/Vaults/registeredIdentities/delete | Kapsayıcı kaydı işlemi, bir kapsayıcı kaydını silmek için kullanılabilir. |
> | Eylem | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | Zaman uyumsuz olarak gönderilen işlemi için sonuç ve işlem durumunu alma işlemi işlemi kullanılabilir sonuçlar elde |
> | Eylem | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Alma işlemi kullanılabilir kapsayıcıları için bir kaynak kayıtlı kapsayıcıları alın. |
> | Eylem | Microsoft.RecoveryServices/Vaults/registeredIdentities/write | Hizmet kapsayıcısı kaydetme işlemi, bir kapsayıcı kurtarma Hizmeti'ne kaydolmak için kullanılabilir. |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Tüm uyarı ayarlarını okuma |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationAlertSettings/write | Tüm uyarı ayarlarını güncelle |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationEvents/read | Tüm olayları okuma |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/checkConsistency/action | Doku Tutarlılığını Denetler |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/delete | Tüm yapıları sil |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/deployProcessServerImage/action | İşlem sunucusu görüntüsünü Dağıt |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/migratetoaad/action | Doku AAD'ye geçirme |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/read | Tüm yapıları okuma |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/reassociateGateway/action | Ağ geçidi yeniden ilişkilendirin |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/remove/action | Doku Kaldır |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/renewcertificate/action | Yapı sertifikasını Yenile |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationLogicalNetworks/read | Hiçbir mantıksal ağ okuma |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read | Herhangi bir ağa okuma |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/delete | Tüm ağ eşlemeleri silme |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/read | Tüm ağ eşlemeleri okuma |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/write | Tüm ağ eşlemeleri güncelle |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/discoverProtectableItem/action | Korunabilir öğe Bul |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/read | Herhangi bir koruma kapsayıcıdaki okuma |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/remove/action | Koruma kapsayıcısı Kaldır |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectableItems/read | Korunabilir tüm öğeleri okuma |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/applyRecoveryPoint/action | Kurtarma noktası Uygula |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/delete | Korunan öğeleri silin |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/failoverCommit/action | Yük devretmenin yürütülmesi |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/plannedFailover/action | Planlanan yük devretme |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/read | Tüm korumalı öğeleri okuma |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | Bir çoğaltma kurtarma noktası okuma |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/remove/action | Korumalı öğe kaldırma |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/repairReplication/action | Onarım çoğaltma |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/reProtect/action | Korumalı öğe koruyun |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/submitFeedback/action | Geri Bildirim Gönder |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/targetComputeSizes/read | Hiçbir hedef işlem boyutları okuma |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailover/action | Test Yük Devretmesi |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailoverCleanup/action | Yük devretme sınaması temizliğini |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/unplannedFailover/action | Yük devretme |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/updateMobilityService/action | Mobility hizmeti güncelleştirmesi |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/write | Tüm korumalı maddeleri oluştur veya güncelleştir |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/delete | Koruma kapsayıcısı eşlemeleri silme |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/read | Koruma kapsayıcısı eşlemeleri okuma |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/remove/action | Koruma kapsayıcısı eşlemesini Kaldır |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/write | Koruma kapsayıcısı eşlemeleri güncelle |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/switchprotection/action | Anahtar koruma kapsayıcısı |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/write | Herhangi bir koruma kapsayıcıdaki güncelle |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/delete | Tüm kurtarma Hizmetleri sağlayıcılarını Sil |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/read | Tüm kurtarma Hizmetleri sağlayıcılarını okuma |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/refreshProvider/action | Sağlayıcı Yenile |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/remove/action | Kurtarma Hizmetleri sağlayıcısını Kaldır |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/write | Tüm kurtarma Hizmetleri sağlayıcılarını güncelle |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/read | Tüm depolama sınıflandırmaları okuma |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/delete | Tüm depolama sınıflandırması eşlemeleri silme |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/read | Tüm depolama sınıflandırması eşlemeleri okuma |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/write | Tüm depolama sınıflandırması eşlemeleri güncelle |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/delete | Tüm Vcenter Sil |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read | Tüm Vcenter okuma |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/write | Tüm Vcenter güncelle |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/write | Tüm yapıları güncelle |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationJobs/cancel/action | İşi İptal Et |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationJobs/read | Herhangi bir işi okuma |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationJobs/restart/action | İşi yeniden başlatın |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationJobs/resume/action | İşi sürdürme |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationNetworkMappings/read | Tüm ağ eşlemeleri okuma |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationNetworks/read | Herhangi bir ağa okuma |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationPolicies/delete | Herhangi bir ilkeyi Sil |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationPolicies/read | Tüm ilkeleri okuma |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationPolicies/write | Tüm ilkeler güncelle |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationProtectedItems/read | Tüm korumalı öğeleri okuma |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationProtectionContainerMappings/read | Koruma kapsayıcısı eşlemeleri okuma |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationProtectionContainers/read | Herhangi bir koruma kapsayıcıdaki okuma |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/delete | Tüm kurtarma planlarını Sil |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/failoverCommit/action | Yük devretmenin yürütülmesi kurtarma planı |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/plannedFailover/action | Planlanan yük devretme kurtarma planı |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read | Tüm kurtarma planlarını okuma |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/reProtect/action | Kurtarma planı koruyun |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailover/action | Test yük devretme kurtarma planı |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailoverCleanup/action | Test yük devretmesi temizliğini kurtarma planı |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/unplannedFailover/action | Yük devretme kurtarma planı |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/write | Tüm kurtarma planları oluştur veya güncelleştir |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationRecoveryServicesProviders/read | Tüm kurtarma Hizmetleri sağlayıcılarını okuma |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationStorageClassificationMappings/read | Tüm depolama sınıflandırması eşlemeleri okuma |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationStorageClassifications/read | Tüm depolama sınıflandırmaları okuma |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationUsages/read | Herhangi bir kasa çoğaltma kullanımı okuma |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationVaultHealth/read | Tüm kasa çoğaltma sistem durumunu okuma |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationVaultHealth/refresh/action | Kasa Durumu Yenile |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationvCenters/read | Tüm Vcenter okuma |
> | Eylem | Microsoft.RecoveryServices/Vaults/tokenInfo/read | Kurtarma Hizmetleri kasası için belirteç bilgileri döndürür. |
> | Eylem | Microsoft.RecoveryServices/vaults/usages/read | Herhangi bir kasa kullanımı okuma |
> | Eylem | Microsoft.RecoveryServices/Vaults/usages/read | Bir Kurtarma Hizmetleri Kasası için kullanım ayrıntılarını döndürür. |
> | Eylem | Microsoft.RecoveryServices/Vaults/validateOperation/action | Korunan öğe üzerinde işlemi doğrulama |
> | Eylem | Microsoft.RecoveryServices/Vaults/vaultTokens/read | Kasa belirteci işlemi kasası düzeyi arka uç işlemleri için kasa belirtecini almak için kullanılabilir. |
> | Eylem | Microsoft.RecoveryServices/Vaults/write | Kasa Oluştur işlemi, 'vault' türünde bir Azure kaynağı oluşturur |

## <a name="microsoftrelay"></a>Microsoft.Relay

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Relay/checkNameAvailability/action | İlgili abonelikte ad alanının kullanılabilirliğini denetler. |
> | Eylem | Microsoft.Relay/checkNamespaceAvailability/action | İlgili abonelikte ad alanının kullanılabilirliğini denetler. Bu API kullanım Lütfen CheckNameAvailabiltiy kullanın. |
> | Eylem | Microsoft.Relay/namespaces/authorizationRules/action | Güncelleştirmeleri Namespace yetkilendirme kuralı. Depricated API'dir. Namespace yetkilendirme kuralı yerine güncelleştirmek için lütfen PUT çağrı kullanın... API sürümü 2017-04-01 üzerinde bu işlem desteklenmiyor. |
> | Eylem | Microsoft.Relay/namespaces/authorizationRules/delete | Namespace yetkilendirme kuralını silin. Namespace yetkilendirme varsayılan kural silinemiyor.  |
> | Eylem | Microsoft.Relay/namespaces/authorizationRules/listkeys/action | Ad Alanı için Bağlantı Dizesini alın |
> | Eylem | Microsoft.Relay/namespaces/authorizationRules/read | Ad Alanı Yetkilendirme Kuralları açıklamasının listesini alır. |
> | Eylem | Microsoft.Relay/namespaces/authorizationRules/regenerateKeys/action | Kaynağın Birincil veya İkincil anahtarını yeniden oluşturun |
> | Eylem | Microsoft.Relay/namespaces/authorizationRules/write | Namespace düzeyi yetkilendirme kuralları oluşturun ve özelliklerini güncelleştirin. Yetkilendirme kuralları erişim hakları, birincil ve ikincil anahtarları güncelleştirilebilir. |
> | Eylem | Microsoft.Relay/namespaces/Delete | Ad Alanı Kaynağını silin |
> | Eylem | Microsoft.Relay/namespaces/disasterRecoveryConfigs/authorizationRules/listkeys/action | Olağanüstü Durum Kurtarma birincil ad alanı için yetkilendirme kuralları anahtarlarını alır |
> | Eylem | Microsoft.Relay/namespaces/disasterRecoveryConfigs/authorizationRules/read | Olağanüstü Durum Kurtarma Birincil Ad Alanının Yetkilendirme Kurallarını Al |
> | Eylem | Microsoft.Relay/namespaces/disasterRecoveryConfigs/breakPairing/action | Olağanüstü Durum Kurtarma'yı devre dışı bırakır ve birincil ad alanındaki değişiklikleri ikincil ad alanına çoğaltmayı durdurur. |
> | Eylem | Microsoft.Relay/namespaces/disasterrecoveryconfigs/checkNameAvailability/action | İlgili abonelikte ad alanının diğer ad kullanılabilirliğini denetler. |
> | Eylem | Microsoft.Relay/namespaces/disasterRecoveryConfigs/delete | Ad alanıyla ilişkili olağanüstü durum kurtarma yapılandırması siler. Bu işlem yalnızca birincil ad alanı çağrılabilir. |
> | Eylem | Microsoft.Relay/namespaces/disasterRecoveryConfigs/failover/action | Bir GEO DR yük devretmesini çağırır ve ad alanı diğer adını ikincil ad alanına işaret edecek şekilde yeniden yapılandırır. |
> | Eylem | Microsoft.Relay/namespaces/disasterRecoveryConfigs/read | Ad alanıyla ilişkili Olağanüstü Durum Kurtarma yapılandırmasını alır. |
> | Eylem | Microsoft.Relay/namespaces/disasterRecoveryConfigs/write | Ad alanıyla ilişkili Olağanüstü Durum Kurtarma yapılandırmasını oluşturur veya güncelleştirir. |
> | Eylem | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/action | HybridConnection güncelleştirmek için güncelleştirme işlemi. API sürümü 2017-04-01 üzerinde bu işlem desteklenmiyor. Yetkilendirme kuralları. Yetkilendirme kuralı güncelleştirmek için lütfen PUT çağrı kullanın. |
> | Eylem | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/delete | İşlemin HybridConnection yetkilendirme kurallarını silmek için |
> | Eylem | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/listkeys/action | Bağlantı dizesi HybridConnection Al |
> | Eylem | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/read |  HybridConnection yetkilendirme kuralları listesini al |
> | Eylem | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/regeneratekeys/action | Kaynağın Birincil veya İkincil anahtarını yeniden oluşturun |
> | Eylem | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/write | HybridConnection yetkilendirme kuralları oluşturun ve özelliklerini güncelleştirin. Yetkilendirme kuralları erişim hakları güncelleştirilebilir. |
> | Eylem | Microsoft.Relay/namespaces/HybridConnections/Delete | HybridConnection kaynağı silme işlemi |
> | Eylem | Microsoft.Relay/namespaces/HybridConnections/read | HybridConnection kaynak açıklamaları listesini al |
> | Eylem | Microsoft.Relay/namespaces/HybridConnections/write | Oluşturma veya güncelleştirme HybridConnection özellikleri. |
> | Eylem | Microsoft.Relay/namespaces/messagingPlan/read | Mesajlaşma planı için bir ad alır.<br>Bu API kullanım dışıdır.<br>MessagingPlan kaynak kullanıma sunulan özellikler (üst) sonraki API sürümleri Namespace kaynağın taşınması...<br>API sürümü 2017-04-01 üzerinde bu işlem desteklenmiyor. |
> | Eylem | Microsoft.Relay/namespaces/messagingPlan/write | Mesajlaşma Plan için bir ad alanı güncelleştirir.<br>Bu API kullanım dışıdır.<br>MessagingPlan kaynak kullanıma sunulan özellikler (üst) sonraki API sürümleri Namespace kaynağın taşınması...<br>API sürümü 2017-04-01 üzerinde bu işlem desteklenmiyor. |
> | Eylem | Microsoft.Relay/namespaces/operationresults/read | Ad alanı işleminin durumunu al |
> | Eylem | Microsoft.Relay/namespaces/providers/Microsoft.Insights/metricDefinitions/read | Namespace ölçümleri kaynak açıklamaları listesini al |
> | Eylem | Microsoft.Relay/namespaces/read | Ad Alanı Kaynak Açıklamasının listesini alır |
> | Eylem | Microsoft.Relay/namespaces/removeAcsNamepsace/action | ACS ad alanını kaldırın |
> | Eylem | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/action | WcfRelay güncelleştirmek için güncelleştirme işlemi. API sürümü 2017-04-01 üzerinde bu işlem desteklenmiyor. Yetkilendirme kuralları. Yetkilendirme kuralı güncelleştirmek için lütfen PUT çağrı kullanın. |
> | Eylem | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/delete | İşlemin WcfRelay yetkilendirme kurallarını silmek için |
> | Eylem | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/listkeys/action | Bağlantı dizesi WcfRelay Al |
> | Eylem | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/read |  WcfRelay yetkilendirme kuralları listesini al |
> | Eylem | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/regeneratekeys/action | Kaynağın Birincil veya İkincil anahtarını yeniden oluşturun |
> | Eylem | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/write | WcfRelay yetkilendirme kuralları oluşturun ve özelliklerini güncelleştirin. Yetkilendirme kuralları erişim hakları güncelleştirilebilir. |
> | Eylem | Microsoft.Relay/namespaces/WcfRelays/Delete | WcfRelay kaynağı silme işlemi |
> | Eylem | Microsoft.Relay/namespaces/WcfRelays/read | WcfRelay kaynak açıklamaları listesini al |
> | Eylem | Microsoft.Relay/namespaces/WcfRelays/write | Oluşturma veya güncelleştirme WcfRelay özellikleri. |
> | Eylem | Microsoft.Relay/namespaces/write | Namespace kaynak oluşturun ve özelliklerini güncelleştirin. Etiketleri ve Namespace kapasitesini hangi güncelleştirilebilir özelliklerdir. |
> | Eylem | Microsoft.Relay/operations/read | İşlemleri Al |
> | Eylem | Microsoft.Relay/register/action | Aboneliği geçiş kaynak sağlayıcısı için kaydeder ve Geçiş kaynaklarının oluşturulmasını sağlar |
> | Eylem | Microsoft.Relay/unregister/action | Aboneliği geçiş kaynak sağlayıcısı için kaydeder ve Geçiş kaynaklarının oluşturulmasını sağlar |

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.ResourceHealth/AvailabilityStatuses/current/read | Belirtilen kaynak için kullanılabilirlik durumunu alır |
> | Eylem | Microsoft.ResourceHealth/AvailabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Eylem | Microsoft.ResourceHealth/events/read | Seçili abonelik için Hizmet Durumu Olayları'nı al |
> | Eylem | Microsoft.Resourcehealth/healthevent/action | Belirtilen kaynaktaki sistem durumu değişikliğini gösterir |
> | Eylem | Microsoft.Resourcehealth/healthevent/Activated/action | Belirtilen kaynaktaki sistem durumu değişikliğini gösterir |
> | Eylem | Microsoft.Resourcehealth/healthevent/InProgress/action | Belirtilen kaynaktaki sistem durumu değişikliğini gösterir |
> | Eylem | Microsoft.Resourcehealth/healthevent/Pending/action | Belirtilen kaynaktaki sistem durumu değişikliğini gösterir |
> | Eylem | Microsoft.Resourcehealth/healthevent/Resolved/action | Belirtilen kaynaktaki sistem durumu değişikliğini gösterir |
> | Eylem | Microsoft.Resourcehealth/healthevent/Updated/action | Belirtilen kaynaktaki sistem durumu değişikliğini gösterir |
> | Eylem | Microsoft.ResourceHealth/impactedResources/read | Seçili abonelik için Etkilenen Kaynaklar'ı al |
> | Eylem | Microsoft.ResourceHealth/register/action | Microsoft ResourceHealth için aboneliği kaydeder |

## <a name="microsoftresources"></a>Microsoft.Resources

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Resources/checkPolicyCompliance/action | Belirli bir kaynağın uyumluluk durumunu kaynak ilkelerine karşı denetleyin. |
> | Eylem | Microsoft.Resources/checkResourceName/action | Kaynak adının geçerliliğini denetle. |
> | Eylem | Microsoft.Resources/deployments/cancel/action | Bir dağıtımı iptal eder. |
> | Eylem | Microsoft.Resources/deployments/delete | Bir dağıtımı siler. |
> | Eylem | Microsoft.Resources/deployments/operations/read | Dağıtım işlemlerini alır veya listeler. |
> | Eylem | Microsoft.Resources/deployments/read | Dağıtımları alır veya listeler. |
> | Eylem | Microsoft.Resources/deployments/validate/action | Bir dağıtımı doğrular. |
> | Eylem | Microsoft.Resources/deployments/write | Bir dağıtımı oluşturur veya güncelleştirir. |
> | Eylem | Microsoft.Resources/links/delete | Bir kaynak bağlantısını siler. |
> | Eylem | Microsoft.Resources/links/read | Kaynak bağlantılarını alır veya listeler. |
> | Eylem | Microsoft.Resources/links/write | Bir kaynak bağlantısını oluşturur veya güncelleştirir. |
> | Eylem | Microsoft.Resources/marketplace/purchase/action | Marketten bir kaynak satın alır. |
> | Eylem | Microsoft.Resources/providers/read | Sağlayıcıların listesini al. |
> | Eylem | Microsoft.Resources/resources/read | Filtrelere göre kaynak listelerini al. |
> | Eylem | Microsoft.Resources/subscriptions/locations/read | Desteklenen konumların listesini alır. |
> | Eylem | Microsoft.Resources/subscriptions/operationresults/read | Abonelik işlem sonuçlarını alır. |
> | Eylem | Microsoft.Resources/subscriptions/providers/read | Kaynak sağlayıcılarını alır veya listeler. |
> | Eylem | Microsoft.Resources/subscriptions/read | Aboneliklerin listesini alır. |
> | Eylem | Microsoft.Resources/subscriptions/resourceGroups/delete | Bir kaynak grubunu ve tüm kaynaklarını siler. |
> | Eylem | Microsoft.Resources/subscriptions/resourcegroups/deployments/operations/read | Dağıtım işlemlerini alır veya listeler. |
> | Eylem | Microsoft.Resources/subscriptions/resourcegroups/deployments/operationstatuses/read | Dağıtım işlemi durumlarını alır veya listeler. |
> | Eylem | Microsoft.Resources/subscriptions/resourcegroups/deployments/read | Dağıtımları alır veya listeler. |
> | Eylem | Microsoft.Resources/subscriptions/resourcegroups/deployments/write | Bir dağıtımı oluşturur veya güncelleştirir. |
> | Eylem | Microsoft.Resources/subscriptions/resourceGroups/moveResources/action | Kaynakları bir kaynak grubundan diğerine taşır. |
> | Eylem | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Eylem | Microsoft.Resources/subscriptions/resourcegroups/resources/read | Kaynak grubun kaynaklarını alır. |
> | Eylem | Microsoft.Resources/subscriptions/resourceGroups/validateMoveResources/action | Bir kaynak grubundan diğerine kaynak taşıma işlemini doğrular. |
> | Eylem | Microsoft.Resources/subscriptions/resourceGroups/write | Kaynak grubunu oluşturur veya güncelleştirir. |
> | Eylem | Microsoft.Resources/subscriptions/resources/read | Bir aboneliğin kaynaklarını alır. |
> | Eylem | Microsoft.Resources/subscriptions/tagNames/delete | Bir abonelik etiketini siler. |
> | Eylem | Microsoft.Resources/subscriptions/tagNames/read | Abonelik etiketlerini alır veya listeler. |
> | Eylem | Microsoft.Resources/subscriptions/tagNames/tagValues/delete | Bir abonelik etiketi değerini siler. |
> | Eylem | Microsoft.Resources/subscriptions/tagNames/tagValues/read | Abonelik etiketi değerlerini alır veya listeler. |
> | Eylem | Microsoft.Resources/subscriptions/tagNames/tagValues/write | Bir abonelik etiketi değeri ekler. |
> | Eylem | Microsoft.Resources/subscriptions/tagNames/write | Bir abonelik etiketi ekler. |
> | Eylem | Microsoft.Resources/tenants/read | Kiracı listesini alır. |

## <a name="microsoftscheduler"></a>Microsoft.Scheduler

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Scheduler/jobcollections/delete | İş koleksiyonunu siler. |
> | Eylem | Microsoft.Scheduler/jobcollections/disable/action | İş koleksiyonunu devre dışı bırakır. |
> | Eylem | Microsoft.Scheduler/jobcollections/enable/action | İş koleksiyonunu etkinleştirir. |
> | Eylem | Microsoft.Scheduler/jobcollections/jobs/delete | İşi siler. |
> | Eylem | Microsoft.Scheduler/jobcollections/jobs/generateLogicAppDefinition/action | Bir Scheduler İşini temel alarak Mantıksal Uygulama tanımı oluşturur. |
> | Eylem | Microsoft.Scheduler/jobcollections/jobs/jobhistories/read | İş geçmişini alır. |
> | Eylem | Microsoft.Scheduler/jobcollections/jobs/read | İşi alır. |
> | Eylem | Microsoft.Scheduler/jobcollections/jobs/run/action | İşi çalıştırır. |
> | Eylem | Microsoft.Scheduler/jobcollections/jobs/write | İş oluşturur veya güncelleştirir. |
> | Eylem | Microsoft.Scheduler/jobcollections/read | İş Koleksiyonu Al |
> | Eylem | Microsoft.Scheduler/jobcollections/write | İş koleksiyonu oluşturur veya güncelleştirir. |

## <a name="microsoftsearch"></a>Microsoft.Search

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Search/checkNameAvailability/action | Hizmet adı kullanılabilirliğini denetler. |
> | Eylem | Microsoft.Search/operations/read | Tüm Microsoft.Search sağlayıcısının kullanılabilir işlemleri listeler. |
> | Eylem | Microsoft.Search/register/action | Arama kaynak sağlayıcısı için aboneliği kaydeder ve arama hizmetleri oluşturulmasını sağlar. |
> | Eylem | Microsoft.Search/searchServices/createQueryKey/action | Sorgu anahtarı oluşturur. |
> | Eylem | Microsoft.Search/searchServices/delete | Arama hizmeti siler. |
> | Eylem | Microsoft.Search/searchServices/deleteQueryKey/delete | Sorgu anahtarı siler. |
> | Eylem | Microsoft.Search/searchServices/diagnosticSettings/read | Diganostic kaynak için okuma ayarını alır |
> | Eylem | Microsoft.Search/searchServices/diagnosticSettings/write | Oluşturur veya güncelleştirir kaynağın diganostic ayarı |
> | Eylem | Microsoft.Search/searchServices/listAdminKeys/action | Yönetici anahtarları okur. |
> | Eylem | Microsoft.Search/searchServices/listQueryKeys/read | Verilen Azure Search hizmeti için sorgu API anahtarları listesini döndürür. |
> | Eylem | Microsoft.Search/searchServices/logDefinitions/read | Arama hizmeti için kullanılabilir günlüklerini alır |
> | Eylem | Microsoft.Search/searchServices/metricDefinitions/read | Arama hizmeti için kullanılabilir ölçümleri alır |
> | Eylem | Microsoft.Search/searchServices/read | Arama hizmeti okur. |
> | Eylem | Microsoft.Search/searchServices/regenerateAdminKey/action | Yönetici anahtarını yeniden oluşturur. |
> | Eylem | Microsoft.Search/searchServices/start/action | Arama hizmetini başlatır. |
> | Eylem | Microsoft.Search/searchServices/stop/action | Arama hizmetini durdurur. |
> | Eylem | Microsoft.Search/searchServices/write | Arama hizmeti güncelleştirir veya oluşturur. |

## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Security/alerts/read | Tüm kullanılabilir güvenlik uyarıları alır |
> | Eylem | Microsoft.Security/applicationWhitelistings/read | Uygulama whitelistings alır |
> | Eylem | Microsoft.Security/applicationWhitelistings/write | Yeni uygulamaları güvenilir listesine ekleme oluşturur veya mevcut kümeyi güncelleştirir |
> | Eylem | Microsoft.Security/complianceResults/read | Kaynak için Uyumluluk sonuçlarını alır |
> | Eylem | Microsoft.Security/locations/alerts/activate/action | Bir güvenlik uyarısı etkinleştir |
> | Eylem | Microsoft.Security/locations/alerts/dismiss/action | Bir güvenlik uyarısını kapatmanın |
> | Eylem | Microsoft.Security/locations/alerts/read | Tüm kullanılabilir güvenlik uyarıları alır |
> | Eylem | Microsoft.Security/locations/jitNetworkAccessPolicies/initiate/action | Yalnızca zaman ağ erişim ilkesi başlatır |
> | Eylem | Microsoft.Security/locations/jitNetworkAccessPolicies/read | Yalnızca zaman ağ erişim ilkelerini alır |
> | Eylem | Microsoft.Security/locations/jitNetworkAccessPolicies/write | Yeni bir tam zamanı ağ erişim ilkesi oluşturur veya mevcut kümeyi güncelleştirir |
> | Eylem | Microsoft.Security/locations/read | Güvenlik veri konumu alır |
> | Eylem | Microsoft.Security/locations/tasks/activate/action | Güvenlik açısından etkinleştir |
> | Eylem | Microsoft.Security/locations/tasks/dismiss/action | Güvenlik açısından kapatın |
> | Eylem | Microsoft.Security/locations/tasks/read | Tüm kullanılabilir güvenlik önerileri alır |
> | Eylem | Microsoft.Security/locations/tasks/resolve/action | Güvenlik açısından çözümleyin |
> | Eylem | Microsoft.Security/locations/tasks/start/action | Güvenlik açısından Başlat |
> | Eylem | Microsoft.Security/policies/read | Güvenlik İlkesi alır |
> | Eylem | Microsoft.Security/policies/write | Güvenlik İlkesi güncelleştirmeleri |
> | Eylem | Microsoft.Security/pricings/delete | Kapsam için fiyatlandırma ayarları siler |
> | Eylem | Microsoft.Security/pricings/read | Kapsam için fiyatlandırma ayarlarını alır |
> | Eylem | Microsoft.Security/pricings/write | Kapsam için fiyatlandırma ayarlarını güncelleştirir |
> | Eylem | Microsoft.Security/register/action | Azure Güvenlik Merkezi için aboneliği kaydeder |
> | Eylem | Microsoft.Security/securityContacts/delete | Güvenlik kişiyi siler |
> | Eylem | Microsoft.Security/securityContacts/read | Güvenlik kişi alır |
> | Eylem | Microsoft.Security/securityContacts/write | Güvenlik kişi güncelleştirmeleri |
> | Eylem | Microsoft.Security/securitySolutions/delete | Bir güvenlik çözümü siler |
> | Eylem | Microsoft.Security/securitySolutions/read | Güvenlik çözümleri alır |
> | Eylem | Microsoft.Security/securitySolutions/write | Yeni bir güvenlik çözümü oluşturur veya mevcut kümeyi güncelleştirir |
> | Eylem | Microsoft.Security/securitySolutionsReferenceData/read | Güvenlik çözümleri başvuru verileri alır |
> | Eylem | Microsoft.Security/securityStatuses/read | Güvenlik sistem durumlarını için Azure kaynaklarını alır. |
> | Eylem | Microsoft.Security/securityStatusesSummaries/read | Güvenlik durumları özetleri kapsamın alır. |
> | Eylem | Microsoft.Security/tasks/read | Tüm kullanılabilir güvenlik önerileri alır |
> | Eylem | Microsoft.Security/webApplicationFirewalls/delete | Bir web uygulaması güvenlik duvarı siler |
> | Eylem | Microsoft.Security/webApplicationFirewalls/read | Web uygulaması güvenlik duvarı alır |
> | Eylem | Microsoft.Security/webApplicationFirewalls/write | Yeni bir web uygulaması güvenlik duvarı oluşturur veya mevcut kümeyi güncelleştirir |
> | Eylem | Microsoft.Security/workspaceSettings/connect/action | Çalışma alanı ayarlarını yeniden bağlanma ayarlarını değiştirme |
> | Eylem | Microsoft.Security/workspaceSettings/delete | Çalışma alanı ayarları siler |
> | Eylem | Microsoft.Security/workspaceSettings/read | Çalışma alanı ayarları alır |
> | Eylem | Microsoft.Security/workspaceSettings/write | Güncelleştirmeler çalışma alanı ayarları |

## <a name="microsoftsecuritygraph"></a>Microsoft.SecurityGraph

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.SecurityGraph/diagnosticsettings/delete | Tanılama ayarını silme |
> | Eylem | Microsoft.SecurityGraph/diagnosticsettings/read | Tanılama ayarını okuma |
> | Eylem | Microsoft.SecurityGraph/diagnosticsettings/write | Tanılama ayarı yazılıyor |
> | Eylem | Microsoft.SecurityGraph/diagnosticsettingscategories/read | Tanılama ayarını kategorileri okuma |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.ServiceBus/checkNameAvailability/action | İlgili abonelikte ad alanının kullanılabilirliğini denetler. |
> | Eylem | Microsoft.ServiceBus/checkNamespaceAvailability/action | İlgili abonelikte ad alanının kullanılabilirliğini denetler. Bu API kullanım Lütfen CheckNameAvailabiltiy kullanın. |
> | Eylem | Microsoft.ServiceBus/namespaces/authorizationRules/action | Güncelleştirmeleri Namespace yetkilendirme kuralı. Depricated API'dir. Namespace yetkilendirme kuralı yerine güncelleştirmek için lütfen PUT çağrı kullanın... API sürümü 2017-04-01 üzerinde bu işlem desteklenmiyor. |
> | Eylem | Microsoft.ServiceBus/namespaces/authorizationRules/delete | Namespace yetkilendirme kuralını silin. Namespace yetkilendirme varsayılan kural silinemiyor.  |
> | Eylem | Microsoft.ServiceBus/namespaces/authorizationRules/listkeys/action | Ad Alanı için Bağlantı Dizesini alın |
> | Eylem | Microsoft.ServiceBus/namespaces/authorizationRules/read | Ad Alanı Yetkilendirme Kuralları açıklamasının listesini alır. |
> | Eylem | Microsoft.ServiceBus/namespaces/authorizationRules/regenerateKeys/action | Kaynağın Birincil veya İkincil anahtarını yeniden oluşturun |
> | Eylem | Microsoft.ServiceBus/namespaces/authorizationRules/write | Namespace düzeyi yetkilendirme kuralları oluşturun ve özelliklerini güncelleştirin. Yetkilendirme kuralları erişim hakları, birincil ve ikincil anahtarları güncelleştirilebilir. |
> | Eylem | Microsoft.ServiceBus/namespaces/Delete | Ad Alanı Kaynağını silin |
> | Eylem | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/authorizationRules/listkeys/action | Olağanüstü Durum Kurtarma birincil ad alanı için yetkilendirme kuralları anahtarlarını alır |
> | Eylem | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/authorizationRules/read | Olağanüstü Durum Kurtarma Birincil Ad Alanının Yetkilendirme Kurallarını Al |
> | Eylem | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/breakPairing/action | Olağanüstü Durum Kurtarma'yı devre dışı bırakır ve birincil ad alanındaki değişiklikleri ikincil ad alanına çoğaltmayı durdurur. |
> | Eylem | Microsoft.ServiceBus/namespaces/disasterrecoveryconfigs/checkNameAvailability/action | İlgili abonelikte ad alanının diğer ad kullanılabilirliğini denetler. |
> | Eylem | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/delete | Ad alanıyla ilişkili olağanüstü durum kurtarma yapılandırması siler. Bu işlem yalnızca birincil ad alanı çağrılabilir. |
> | Eylem | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/failover/action | Bir GEO DR yük devretmesini çağırır ve ad alanı diğer adını ikincil ad alanına işaret edecek şekilde yeniden yapılandırır. |
> | Eylem | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/read | Ad alanıyla ilişkili Olağanüstü Durum Kurtarma yapılandırmasını alır. |
> | Eylem | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/write | Ad alanıyla ilişkili Olağanüstü Durum Kurtarma yapılandırmasını oluşturur veya güncelleştirir. |
> | Eylem | Microsoft.ServiceBus/namespaces/eventGridFilters/delete | Ad alanı ile ilişkili Olay Kılavuzu filtresini siler. |
> | Eylem | Microsoft.ServiceBus/namespaces/eventGridFilters/read | Ad alanı ile ilişkili Olay Kılavuzu filtresini alır. |
> | Eylem | Microsoft.ServiceBus/namespaces/eventGridFilters/write | Ad alanı ile ilişkili Olay Kılavuzu filtresini oluşturur veya güncelleştirir. |
> | Eylem | Microsoft.ServiceBus/namespaces/eventhubs/read | EventHub kaynak açıklamaları listesini al |
> | Eylem | Microsoft.ServiceBus/namespaces/messagingPlan/read | Mesajlaşma planı için bir ad alır.<br>Bu API kullanım dışıdır.<br>MessagingPlan kaynak kullanıma sunulan özellikler (üst) sonraki API sürümleri Namespace kaynağın taşınması...<br>API sürümü 2017-04-01 üzerinde bu işlem desteklenmiyor. |
> | Eylem | Microsoft.ServiceBus/namespaces/messagingPlan/write | Mesajlaşma Plan için bir ad alanı güncelleştirir.<br>Bu API kullanım dışıdır.<br>MessagingPlan kaynak kullanıma sunulan özellikler (üst) sonraki API sürümleri Namespace kaynağın taşınması...<br>API sürümü 2017-04-01 üzerinde bu işlem desteklenmiyor. |
> | Eylem | Microsoft.ServiceBus/namespaces/migrate/action | Ad alanı geçiş işlemi |
> | Eylem | Microsoft.ServiceBus/namespaces/migrationConfigurations/delete | Geçiş yapılandırmasını siler. |
> | Eylem | Microsoft.ServiceBus/namespaces/migrationConfigurations/read | Geçişin ve bekleyen çoğaltma işlemlerinin durumunu gösteren Geçiş yapılandırmasını alır |
> | Eylem | Microsoft.ServiceBus/namespaces/migrationConfigurations/revert/action | Standart ad alanından premium ad alanına geçişi geri alır |
> | Eylem | Microsoft.ServiceBus/namespaces/migrationConfigurations/upgrade/action | Geçişi tamamlayan ve kaynakların standart ad alanından premium ad alanına eşitlenmesini durduran standart ad alanıyla ilişkili DNS'i premium ad alanına atar |
> | Eylem | Microsoft.ServiceBus/namespaces/migrationConfigurations/write | Oluşturur veya güncelleştirmeleri geçiş yapılandırma. Bu standart kaynaklardan premium ad alanı için eşitleme başlatır |
> | Eylem | Microsoft.ServiceBus/namespaces/operationresults/read | Ad alanı işleminin durumunu al |
> | Eylem | Microsoft.ServiceBus/namespaces/providers/Microsoft.Insights/diagnosticSettings/read | Namespace tanılama ayarları kaynak açıklamaları listesini al |
> | Eylem | Microsoft.ServiceBus/namespaces/providers/Microsoft.Insights/diagnosticSettings/write | Namespace tanılama ayarları kaynak açıklamaları listesini al |
> | Eylem | Microsoft.ServiceBus/namespaces/providers/Microsoft.Insights/logDefinitions/read | Namespace günlükleri kaynak açıklamaları listesini al |
> | Eylem | Microsoft.ServiceBus/namespaces/providers/Microsoft.Insights/metricDefinitions/read | Namespace ölçümleri kaynak açıklamaları listesini al |
> | Eylem | Microsoft.ServiceBus/namespaces/queues/authorizationRules/action | Sıra güncelleştirmek için güncelleştirme işlemi. API sürümü 2017-04-01 üzerinde bu işlem desteklenmiyor. Yetkilendirme kuralları. Yetkilendirme kuralı güncelleştirmek için lütfen PUT çağrı kullanın. |
> | Eylem | Microsoft.ServiceBus/namespaces/queues/authorizationRules/delete | İşlem sırası yetkilendirme kuralları silmek için |
> | Eylem | Microsoft.ServiceBus/namespaces/queues/authorizationRules/listkeys/action | Bağlantı dizesi sıraya al |
> | Eylem | Microsoft.ServiceBus/namespaces/queues/authorizationRules/read |  Sıra yetkilendirme kuralları listesini al |
> | Eylem | Microsoft.ServiceBus/namespaces/queues/authorizationRules/regenerateKeys/action | Kaynağın Birincil veya İkincil anahtarını yeniden oluşturun |
> | Eylem | Microsoft.ServiceBus/namespaces/queues/authorizationRules/write | Sıra yetkilendirme kuralları oluşturun ve özelliklerini güncelleştirin. Yetkilendirme kuralları erişim hakları güncelleştirilebilir. |
> | Eylem | Microsoft.ServiceBus/namespaces/queues/Delete | Sıra kaynağı silme işlemi |
> | Eylem | Microsoft.ServiceBus/namespaces/queues/read | Sıra kaynak açıklamaları listesini al |
> | Eylem | Microsoft.ServiceBus/namespaces/queues/write | Oluşturma veya güncelleştirme sıra özellikleri. |
> | Eylem | Microsoft.ServiceBus/namespaces/read | Ad Alanı Kaynak Açıklamasının listesini alır |
> | Eylem | Microsoft.ServiceBus/namespaces/removeAcsNamepsace/action | ACS ad alanını kaldırın |
> | Eylem | Microsoft.ServiceBus/namespaces/topics/authorizationRules/action | Konu güncelleştirmek için güncelleştirme işlemi. API sürümü 2017-04-01 üzerinde bu işlem desteklenmiyor. Yetkilendirme kuralları. Yetkilendirme kuralı güncelleştirmek için lütfen PUT çağrı kullanın. |
> | Eylem | Microsoft.ServiceBus/namespaces/topics/authorizationRules/delete | İşlemin konu yetkilendirme kurallarını silmek için |
> | Eylem | Microsoft.ServiceBus/namespaces/topics/authorizationRules/listkeys/action | Bağlantı dizesi konuya Al |
> | Eylem | Microsoft.ServiceBus/namespaces/topics/authorizationRules/read |  Konu yetkilendirme kuralları listesini al |
> | Eylem | Microsoft.ServiceBus/namespaces/topics/authorizationRules/regenerateKeys/action | Kaynağın Birincil veya İkincil anahtarını yeniden oluşturun |
> | Eylem | Microsoft.ServiceBus/namespaces/topics/authorizationRules/write | Konu yetkilendirme kuralları oluşturun ve özelliklerini güncelleştirin. Yetkilendirme kuralları erişim hakları güncelleştirilebilir. |
> | Eylem | Microsoft.ServiceBus/namespaces/topics/Delete | Konu kaynağı silme işlemi |
> | Eylem | Microsoft.ServiceBus/namespaces/topics/read | Konu kaynak açıklamaları listesini al |
> | Eylem | Microsoft.ServiceBus/namespaces/topics/subscriptions/Delete | TopicSubscription kaynağı silme işlemi |
> | Eylem | Microsoft.ServiceBus/namespaces/topics/subscriptions/read | TopicSubscription kaynak açıklamaları listesini al |
> | Eylem | Microsoft.ServiceBus/namespaces/topics/subscriptions/rules/Delete | Kural kaynağı silme işlemi |
> | Eylem | Microsoft.ServiceBus/namespaces/topics/subscriptions/rules/read | Kuralın kaynak açıklamaları listesini al |
> | Eylem | Microsoft.ServiceBus/namespaces/topics/subscriptions/rules/write | Oluşturma veya güncelleştirme kuralı özellikleri. |
> | Eylem | Microsoft.ServiceBus/namespaces/topics/subscriptions/write | Oluşturma veya güncelleştirme TopicSubscription özellikleri. |
> | Eylem | Microsoft.ServiceBus/namespaces/topics/write | Oluşturma veya güncelleştirme konu özellikleri. |
> | Eylem | Microsoft.ServiceBus/namespaces/write | Namespace kaynak oluşturun ve özelliklerini güncelleştirin. Etiketleri ve Namespace kapasitesini hangi güncelleştirilebilir özelliklerdir. |
> | Eylem | Microsoft.ServiceBus/operations/read | İşlemleri Al |
> | Eylem | Microsoft.ServiceBus/register/action | Aboneliği ServiceBus kaynak sağlayıcısı için kaydeder ve ServiceBus kaynaklarının oluşturulmasını sağlar |
> | Eylem | Microsoft.ServiceBus/sku/read | Sku kaynak açıklamaları listesini al |
> | Eylem | Microsoft.ServiceBus/sku/regions/read | SkuRegions kaynak açıklamaları listesini al |
> | Eylem | Microsoft.ServiceBus/unregister/action | Aboneliği ServiceBus kaynak sağlayıcısı için kaydeder ve ServiceBus kaynaklarının oluşturulmasını sağlar |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.ServiceFabric/clusters/applications/delete | Herhangi Bir Uygulamayı Silin |
> | Eylem | Microsoft.ServiceFabric/clusters/applications/read | Herhangi Bir Uygulamayı Okuyun |
> | Eylem | Microsoft.ServiceFabric/clusters/applications/services/delete | Herhangi Bir Hizmeti Silin |
> | Eylem | Microsoft.ServiceFabric/clusters/applications/services/partitions/read | Herhangi Bir Bölümü Okuyun |
> | Eylem | Microsoft.ServiceFabric/clusters/applications/services/partitions/replicas/read | Herhangi Bir Çoğaltmayı Okuyun |
> | Eylem | Microsoft.ServiceFabric/clusters/applications/services/read | Herhangi Bir Hizmeti Okuyun |
> | Eylem | Microsoft.ServiceFabric/clusters/applications/services/statuses/read | Tüm Hizmet Durumlarını Oku |
> | Eylem | Microsoft.ServiceFabric/clusters/applications/services/write | Herhangi Bir Hizmet Oluşturun veya Güncelleştirin |
> | Eylem | Microsoft.ServiceFabric/clusters/applications/write | Herhangi Bir Uygulama Oluşturun veya Güncelleştirin |
> | Eylem | Microsoft.ServiceFabric/clusters/applicationTypes/delete | Herhangi Bir Uygulama Türü Silin |
> | Eylem | Microsoft.ServiceFabric/clusters/applicationTypes/read | Herhangi Bir Uygulama Türünü Okuyun |
> | Eylem | Microsoft.ServiceFabric/clusters/applicationTypes/versions/delete | Herhangi Bir Uygulama Türü Sürümü Silin |
> | Eylem | Microsoft.ServiceFabric/clusters/applicationTypes/versions/read | Herhangi Bir Uygulama Türü Sürümünü Okuyun |
> | Eylem | Microsoft.ServiceFabric/clusters/applicationTypes/versions/write | Herhangi Bir Uygulama Türü Sürümü Oluşturun veya Güncelleştirin |
> | Eylem | Microsoft.ServiceFabric/clusters/applicationTypes/write | Herhangi Bir Uygulama Türü Oluşturun veya Güncelleştirin |
> | Eylem | Microsoft.ServiceFabric/clusters/delete | Herhangi Bir Kümeyi Silin |
> | Eylem | Microsoft.ServiceFabric/clusters/nodes/read | Herhangi Bir Düğümü Okuyun |
> | Eylem | Microsoft.ServiceFabric/clusters/read | Herhangi Bir Kümeyi Okuyun |
> | Eylem | Microsoft.ServiceFabric/clusters/statuses/read | Tüm Küme Durumlarını Oku |
> | Eylem | Microsoft.ServiceFabric/clusters/write | Herhangi Bir Kümeyi Oluşturun veya Güncelleştirin |
> | Eylem | Microsoft.ServiceFabric/locations/clusterVersions/read | Herhangi bir Küme Sürümünü oku |
> | Eylem | Microsoft.ServiceFabric/locations/environments/clusterVersions/read | Belirli bir ortam için herhangi bir Küme Sürümünü okuyun |
> | Eylem | Microsoft.ServiceFabric/locations/operationresults/read | Herhangi Bir İşlem Sonucunu Okuyun |
> | Eylem | Microsoft.ServiceFabric/locations/operations/read | Konuma göre herhangi bir İşlemi okuyun |
> | Eylem | Microsoft.ServiceFabric/operations/read | Kullanılabilen Herhangi Bir İşlemi Okuyun |
> | Eylem | Microsoft.ServiceFabric/register/action | Herhangi Bir Eylemi Kaydedin |

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.SignalRService/checknameavailability/action | Yeni bir SignalR hizmet ile kullanmak için bir ad olup olmadığını denetler |
> | Eylem | Microsoft.SignalRService/register/action | 'Microsoft.SignalRService' kaynak sağlayıcısı ile bir aboneliği kaydeder |
> | Eylem | Microsoft.SignalRService/SignalR/delete | Tüm SignalR Sil |
> | Eylem | Microsoft.SignalRService/SignalR/providers/Microsoft.Insights/diagnosticSettings/read | Kaynak için tanılama ayarını alır |
> | Eylem | Microsoft.SignalRService/SignalR/providers/Microsoft.Insights/diagnosticSettings/write | Kaynak için tanılama ayarını oluşturur veya güncelleştirir |
> | Eylem | Microsoft.SignalRService/SignalR/providers/Microsoft.Insights/metricDefinitions/read | SignalR için kullanılabilir ölçümleri alır |
> | Eylem | Microsoft.SignalRService/SignalR/read | SignalR ayarları ve yapılandırmaları Yönetim Portalı'nda veya API aracılığıyla görüntüleme |
> | Eylem | Microsoft.SignalRService/SignalR/write | SignalR ayarları ve Yönetim Portalı'nda veya API aracılığıyla yapılandırmalarını değiştirme |
> | Eylem | Microsoft.SignalRService/unregister/action | Bir abonelikle 'Microsoft.SignalRService' kaynak sağlayıcısı kaydını siler |

## <a name="microsoftsolutions"></a>Microsoft.Solutions

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Solutions/applicationDefinitions/delete | Uygulama tanımı kaldırır. |
> | Eylem | Microsoft.Solutions/applicationDefinitions/read | Uygulama tanımları listesi alır. |
> | Eylem | Microsoft.Solutions/applicationDefinitions/write | Uygulama tanımı ekle veya değiştir. |
> | Eylem | Microsoft.Solutions/applications/delete | Uygulama kaldırır. |
> | Eylem | Microsoft.Solutions/applications/read | Uygulama listesi alır. |
> | Eylem | Microsoft.Solutions/applications/write | Uygulama oluşturur. |
> | Eylem | Microsoft.Solutions/locations/operationStatuses/read | Kaynağın işlem durumunu okur. |
> | Eylem | Microsoft.Solutions/register/action | Çözümler için kaydolun. |

## <a name="microsoftsql"></a>Microsoft.Sql

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Sql/checkNameAvailability/action | Sunucu adı belirli bir aboneliğe yönelik tüm dünyada sağlamak için kullanılabilir olup olmadığını doğrulayın. |
> | Eylem | Microsoft.Sql/locations/auditingSettingsAzureAsyncOperation/read | Sonuç İlkesi ayarlama işlemi denetim genişletilmiş sunucusu BLOB alma |
> | Eylem | Microsoft.Sql/locations/auditingSettingsOperationResults/read | Sonuç İlkesi ayarlama işlemi denetim sunucu BLOB alma |
> | Eylem | Microsoft.Sql/locations/capabilities/read | Özellikleri bu abonelik için belirtilen bir konumda alır. |
> | Eylem | Microsoft.Sql/locations/databaseAzureAsyncOperation/read | Veritabanı işlemi durumunu alır. |
> | Eylem | Microsoft.Sql/locations/databaseOperationResults/read | Veritabanı işlemi durumunu alır. |
> | Eylem | Microsoft.Sql/locations/deletedServerAsyncOperation/read | İlerleme-silinen sunucusundaki işlemleri alır |
> | Eylem | Microsoft.Sql/locations/deletedServerOperationResults/read | İlerleme-silinen sunucusundaki işlemleri alır |
> | Eylem | Microsoft.Sql/locations/deletedServers/read | Silinmiş sunucuları veya belirtilen silinen sunucunun özelliklerini alır listesini döndürür. |
> | Eylem | Microsoft.Sql/locations/deletedServers/recover/action | Silinen sunucusunu kurtarma |
> | Eylem | Microsoft.Sql/locations/deleteVirtualNetworkOrSubnets/action | Bir sanal ağ veya alt ağa ilişkili sanal ağ kuralları siler |
> | Eylem | Microsoft.Sql/locations/elasticPoolAzureAsyncOperation/read | Esnek havuz zaman uyumsuz bir işlem için azure zaman uyumsuz işlemi alır |
> | Eylem | Microsoft.Sql/locations/elasticPoolOperationResults/read | Bir esnek havuz işleminin sonucunu alır. |
> | Eylem | Microsoft.Sql/locations/extendedAuditingSettingsAzureAsyncOperation/read | Sonuç İlkesi ayarlama işlemi denetim genişletilmiş sunucusu BLOB alma |
> | Eylem | Microsoft.Sql/locations/extendedAuditingSettingsOperationResults/read | Sonuç İlkesi ayarlama işlemi denetim genişletilmiş sunucusu BLOB alma |
> | Eylem | Microsoft.Sql/locations/instanceFailoverGroups/delete | Var olan bir örneği yük devretme grubunu siler. |
> | Eylem | Microsoft.Sql/locations/instanceFailoverGroups/failover/action | Planlanan yük devretme var olan bir örneği yük devretme grubuna yürütür. |
> | Eylem | Microsoft.Sql/locations/instanceFailoverGroups/forceFailoverAllowDataLoss/action | Zorla yük devretme var olan bir örneği yük devretme grubuna yürütür. |
> | Eylem | Microsoft.Sql/locations/instanceFailoverGroups/read | Listenin örneği yük devretme grupları veya belirtilen örneği yük devretme grubu için özellikleri alır döndürür. |
> | Eylem | Microsoft.Sql/locations/instanceFailoverGroups/write | Belirtilen parametrelerle bir örneği yük devretme grubu oluşturur veya özellikleri veya etiketleri belirtilen örneği yük devretme grubu için güncelleştirir. |
> | Eylem | Microsoft.Sql/locations/longTermRetentionBackups/read | Bir konumdaki her sunucuda her veritabanı için uzun vadeli bekletme yedeklemeleri listeler |
> | Eylem | Microsoft.Sql/locations/longTermRetentionServers/longTermRetentionBackups/read | Bir sunucudaki her veritabanı için uzun vadeli bekletme yedeklemeleri listeler |
> | Eylem | Microsoft.Sql/locations/longTermRetentionServers/longTermRetentionDatabases/longTermRetentionBackups/delete | Uzun vadeli bekletme yedeğini siler |
> | Eylem | Microsoft.Sql/locations/longTermRetentionServers/longTermRetentionDatabases/longTermRetentionBackups/read | Bir veritabanı için uzun vadeli bekletme yedeklemeleri listeler |
> | Eylem | Microsoft.Sql/locations/managedDatabaseRestoreAzureAsyncOperation/completeRestore/action | Yönetilen veritabanı geri yükleme işlemi tamamlanır |
> | Eylem | Microsoft.Sql/locations/managedTransparentDataEncryptionAzureAsyncOperation/read | İlerleme-yönetilen veritabanında saydam veri şifreleme işlemleri alır |
> | Eylem | Microsoft.Sql/locations/managedTransparentDataEncryptionOperationResults/read | İlerleme-yönetilen veritabanında saydam veri şifreleme işlemleri alır |
> | Eylem | Microsoft.Sql/locations/networkInterfaceAzureAsyncOperation/read | Belirli bir ağ arabiriminde Azure zaman uyumsuz işlem ayrıntılarını döndürür |
> | Eylem | Microsoft.Sql/locations/networkInterfaceOperationResults/read | Belirtilen ağ arabirimi işlem ayrıntılarını döndürür |
> | Eylem | Microsoft.Sql/locations/read | Belirli bir aboneliğe kullanılabilir konumlarını alır |
> | Eylem | Microsoft.Sql/locations/syncAgentOperationResults/read | Eşitleme Aracı kaynak işleminin sonucu alma |
> | Eylem | Microsoft.Sql/locations/syncDatabaseIds/read | Belirli bölge ve abonelik için eşitleme veritabanı kimlikleri alma |
> | Eylem | Microsoft.Sql/locations/syncGroupOperationResults/read | Eşitleme grubu kaynak işleminin sonucu alma |
> | Eylem | Microsoft.Sql/locations/syncMemberOperationResults/read | Eşitleme üye kaynak işleminin sonucu alma |
> | Eylem | Microsoft.Sql/locations/usages/read | Kullanım ölçümleri koleksiyonu Bu abonelik için bir konumda alır. |
> | Eylem | Microsoft.Sql/locations/virtualNetworkRulesAzureAsyncOperation/read | Azure zaman uyumsuz işlem belirtilen sanal ağ kuralları ayrıntılarını döndürür  |
> | Eylem | Microsoft.Sql/locations/virtualNetworkRulesOperationResults/read | Belirtilen sanal ağ kuralları işlem ayrıntılarını döndürür  |
> | Eylem | Microsoft.Sql/managedInstances/administrators/delete | Mevcut yönetici yönetilen örneğinin siler. |
> | Eylem | Microsoft.Sql/managedInstances/administrators/read | Yönetilen örneği yöneticilerin listesini alır. |
> | Eylem | Microsoft.Sql/managedInstances/administrators/write | Belirtilen parametrelerle yönetilen örneği yönetici güncelleştirir veya oluşturur. |
> | Eylem | Microsoft.Sql/managedInstances/databases/delete | Var olan bir yönetilen veritabanı siler |
> | Eylem | Microsoft.Sql/managedInstances/databases/read | Yönetilen veritabanı varolan alır |
> | Eylem | Microsoft.Sql/managedInstances/databases/securityAlertPolicies/read | Belirli bir yönetilen veritabanı üzerinde yapılandırılan veritabanı tehdit algılama ilkesi ayrıntılarını alma |
> | Eylem | Microsoft.Sql/managedInstances/databases/securityAlertPolicies/write | Verilen bir yönetilen veritabanı için veritabanı tehdit algılama ilkesini değiştirme |
> | Eylem | Microsoft.Sql/managedInstances/databases/securityEvents/read | Yönetilen veritabanı güvenlik olaylarını alır |
> | Eylem | Microsoft.Sql/managedInstances/databases/transparentDataEncryption/read | Belirli bir yönetilen veritabanında saydam veri şifreleme veritabanı ayrıntılarını alma |
> | Eylem | Microsoft.Sql/managedInstances/databases/transparentDataEncryption/write | Belirli bir yönetilen veritabanı için veritabanı saydam veri şifreleme değiştirme |
> | Eylem | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/delete | Verilen bir veritabanı için güvenlik açığı değerlendirmesi Kaldır |
> | Eylem | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/read | Belirli bir veritabanı üzerinde yapılandırılan güvenlik açığı değerlendirmesi ayrıntılarını alma |
> | Eylem | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/rules/baselines/delete | Verilen bir veritabanı için güvenlik açığı değerlendirmesi kural temel Kaldır |
> | Eylem | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/rules/baselines/read | Verilen bir veritabanı için güvenlik açığı değerlendirmesi kural taban Al |
> | Eylem | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/rules/baselines/write | Verilen bir veritabanı için güvenlik açığı değerlendirmesi kural temel değiştirme |
> | Eylem | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/scans/export/action | Varolan bir tarama sonuç bir insan okunabilir biçimine dönüştürün. Hiçbir şey zaten olur |
> | Eylem | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/scans/initiateScan/action | Güvenlik Açığı değerlendirmesi veritabanı taraması yürütün. |
> | Eylem | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/scans/read | Veritabanı güvenlik açığı listesi değerlendirme taraması kayıtları dönün veya belirtilen tarama kimliği için tarama kayıt Al |
> | Eylem | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/write | Verilen bir veritabanı için güvenlik açığı değerlendirmesi değiştirme |
> | Eylem | Microsoft.Sql/managedInstances/databases/write | Yeni bir veritabanı oluşturur veya varolan bir veritabanını güncelleştirir. |
> | Eylem | Microsoft.Sql/managedInstances/delete | Mevcut yönetilen örneği siler. |
> | Eylem | Microsoft.Sql/managedInstances/metricDefinitions/read | Yönetilen örneği ölçüm tanımlarını Al |
> | Eylem | Microsoft.Sql/managedInstances/metrics/read | Yönetilen örneği ölçümleri alma |
> | Eylem | Microsoft.Sql/managedInstances/read | Yönetilen örnekleri veya belirtilen yönetilen örneği için özellikleri alır listesini döndürür. |
> | Eylem | Microsoft.Sql/managedInstances/securityAlertPolicies/read | Belirli bir yönetilen sunucusunda yapılandırılan yönetilen sunucu tehdit algılama ilkesi ayrıntılarını alma |
> | Eylem | Microsoft.Sql/managedInstances/securityAlertPolicies/write | Belirli bir yönetilen sunucuda yönetilen sunucu tehdit algılama ilkesini değiştirme |
> | Eylem | Microsoft.Sql/managedInstances/tdeCertificates/action | Oluştur/güncelleştir TDE sertifika |
> | Eylem | Microsoft.Sql/managedInstances/write | Yönetilen bir örneğini belirtilen parametrelerle oluşturur veya özellikleri veya etiketleri belirtilen yönetilen örneği için güncelleştirin. |
> | Eylem | Microsoft.Sql/operations/read | Kullanılabilir REST işlemlerini alır |
> | Eylem | Microsoft.Sql/register/action | Microsoft SQL veritabanı kaynak sağlayıcısı için aboneliği kaydeder ve Microsoft SQL veritabanları oluşturulmasını sağlar. |
> | Eylem | Microsoft.Sql/servers/administratorOperationResults/read | Sunucu yöneticileri üzerinde devam eden işlemler alır |
> | Eylem | Microsoft.Sql/servers/administrators/delete | Sunucu Yöneticisi Sil |
> | Eylem | Microsoft.Sql/servers/administrators/read | Sunucu Yöneticisi ile ilgili ayrıntıları alma |
> | Eylem | Microsoft.Sql/servers/administrators/write | Sunucu Yöneticisi güncelle |
> | Eylem | Microsoft.Sql/servers/advisors/read | Sunucu için kullanılabilir danışmanlar listesini döndürür |
> | Eylem | Microsoft.Sql/servers/advisors/recommendedActions/read | Sunucusu için belirtilen Danışmanı'nın önerilen eylemleri listesini döndürür |
> | Eylem | Microsoft.Sql/servers/advisors/recommendedActions/write | Sunucudaki önerilen eylemi uygulayın |
> | Eylem | Microsoft.Sql/servers/advisors/write | Güncelleştirmeleri otomatik-sunucu düzeyinde bir Danışmanı durumunu yürütün. |
> | Eylem | Microsoft.Sql/servers/auditingPolicies/read | Denetim İlkesi belirli bir sunucuda yapılandırılan varsayılan sunucu tablosu ayrıntılarını alma |
> | Eylem | Microsoft.Sql/servers/auditingPolicies/write | Varsayılan sunucu tablo belli bir sunucu için Denetim değiştirme |
> | Eylem | Microsoft.Sql/servers/auditingSettings/operationResults/read | Sonuç İlkesi ayarlama işlemi denetim sunucu BLOB alma |
> | Eylem | Microsoft.Sql/servers/auditingSettings/read | Verilen bir sunucu üzerinde yapılandırılmış sunucu blob denetim ilkesi ayrıntılarını alma |
> | Eylem | Microsoft.Sql/servers/auditingSettings/write | Sunucu blob belli bir sunucu için Denetim değiştirme |
> | Eylem | Microsoft.Sql/servers/automaticTuning/read | Sunucusu için otomatik ayarlarını döndürür |
> | Eylem | Microsoft.Sql/servers/automaticTuning/write | Sunucusu için otomatik ayarlarını güncelleştirir ve güncelleştirilmiş ayarları döndürür |
> | Eylem | Microsoft.Sql/servers/backupLongTermRetentionVaults/delete | Mevcut bir yedekleme arşivleme kasası siler. |
> | Eylem | Microsoft.Sql/servers/backupLongTermRetentionVaults/read | Bu işlem, bir yedekleme uzun süreli saklama kasası almak için kullanılır. Bu sunucuya kayıtlı olduğu kasanın hakkında bilgi döndürür |
> | Eylem | Microsoft.Sql/servers/backupLongTermRetentionVaults/write | Bu işlem bir yedekleme uzun vadeli bekletme kaydetmek için kullanılan bir sunucu kasası |
> | Eylem | Microsoft.Sql/servers/communicationLinks/delete | Var olan bir sunucu iletişimi bağlantı siler. |
> | Eylem | Microsoft.Sql/servers/communicationLinks/read | Belirtilen bir sunucunun iletişim bağlantılarının bir listesini döndürür. |
> | Eylem | Microsoft.Sql/servers/communicationLinks/write | Oluşturun veya bir sunucu iletişimi bağlantı güncelleştirin. |
> | Eylem | Microsoft.Sql/servers/connectionPolicies/read | Sunucu bağlantısı belirli bir sunucunun ilkelerin listesini döndürür. |
> | Eylem | Microsoft.Sql/servers/connectionPolicies/write | Oluşturun veya sunucu bağlantı İlkesi güncelleştirin. |
> | Eylem | Microsoft.Sql/servers/databases/advisors/read | Veritabanı için kullanılabilir danışmanlar listesini döndürür |
> | Eylem | Microsoft.Sql/servers/databases/advisors/recommendedActions/read | Veritabanı için belirtilen Danışmanı'nın önerilen eylemleri listesini döndürür |
> | Eylem | Microsoft.Sql/servers/databases/advisors/recommendedActions/write | Veritabanı üzerinde önerilen eylemi uygulayın |
> | Eylem | Microsoft.Sql/servers/databases/advisors/write | Güncelleştirme otomatik yürütme veritabanı düzeyinde bir advisor durumu. |
> | Eylem | Microsoft.Sql/servers/databases/auditingPolicies/read | Belirli bir veritabanı üzerinde yapılandırılan tablo denetim ilkesi ayrıntılarını alma |
> | Eylem | Microsoft.Sql/servers/databases/auditingPolicies/write | Verilen bir veritabanı için tablo denetim ilkesini değiştirme |
> | Eylem | Microsoft.Sql/servers/databases/auditingSettings/read | Belirli bir veritabanı üzerinde yapılandırılan blob denetim ilkesi ayrıntılarını alma |
> | Eylem | Microsoft.Sql/servers/databases/auditingSettings/write | Verilen bir veritabanı için blob denetim ilkesini değiştirme |
> | Eylem | Microsoft.Sql/servers/databases/auditRecords/read | Veritabanı blob Denetim kayıtlarını alma |
> | Eylem | Microsoft.Sql/servers/databases/automaticTuning/read | Bir veritabanı için otomatik ayarlama ayarları döndürür |
> | Eylem | Microsoft.Sql/servers/databases/automaticTuning/write | Bir veritabanı için otomatik ayarlama ayarlarını güncelleştirir ve güncelleştirilmiş ayarları döndürür |
> | Eylem | Microsoft.Sql/servers/databases/azureAsyncOperation/read | Veritabanı işlemi durumunu alır. |
> | Eylem | Microsoft.Sql/servers/databases/backupLongTermRetentionPolicies/read | Belirtilen veritabanı yedekleme arşivleme ilkelerin listesini döndürür. |
> | Eylem | Microsoft.Sql/servers/databases/backupLongTermRetentionPolicies/write | Oluşturun veya bir veritabanı yedekleme arşivleme İlkesi güncelleştirin. |
> | Eylem | Microsoft.Sql/servers/databases/connectionPolicies/read | Belirli bir veritabanı üzerinde yapılandırılan bağlantı ilkesi ayrıntılarını alma |
> | Eylem | Microsoft.Sql/servers/databases/connectionPolicies/write | Verilen bir veritabanı için bağlantı ilkesini değiştirme |
> | Eylem | Microsoft.Sql/servers/databases/dataMaskingPolicies/read | Veritabanı veri ilkeleri maskeleme listesini döndürür. |
> | Eylem | Microsoft.Sql/servers/databases/dataMaskingPolicies/rules/delete | Verilen bir veritabanı için ilke kuralı maskeleme verilerini sil |
> | Eylem | Microsoft.Sql/servers/databases/dataMaskingPolicies/rules/read | Belirli bir veritabanı üzerinde yapılandırılan ilke kuralı maskeleme veri ayrıntılarını alma |
> | Eylem | Microsoft.Sql/servers/databases/dataMaskingPolicies/rules/write | Verilen bir veritabanı için ilke kuralı maskeleme verileri değiştirme |
> | Eylem | Microsoft.Sql/servers/databases/dataMaskingPolicies/write | İlke verilen bir veritabanı için maskeleme verileri değiştirme |
> | Eylem | Microsoft.Sql/servers/databases/dataWarehouseQueries/dataWarehouseQuerySteps/read | Seçili adımı kimliği için veri ambarı sorgusu dağıtılmış sorgu adım bilgileri döndürür |
> | Eylem | Microsoft.Sql/servers/databases/dataWarehouseQueries/read | Seçili sorgu kimliği için veri ambarı dağıtım sorgu bilgilerini döndürür |
> | Eylem | Microsoft.Sql/servers/databases/dataWarehouseUserActivities/read | Çalışan ve askıya alınan sorguları içeren bir SQL Data Warehouse örneğine kullanıcı etkinliklerini alır. |
> | Eylem | Microsoft.Sql/servers/databases/delete | Varolan bir veritabanını siler. |
> | Eylem | Microsoft.Sql/servers/databases/export/action | Azure SQL veritabanını dışarı aktarma |
> | Eylem | Microsoft.Sql/servers/databases/extendedAuditingSettings/read | Belirli bir veritabanı üzerinde yapılandırılan genişletilmiş blob denetim ilkesi ayrıntılarını alma |
> | Eylem | Microsoft.Sql/servers/databases/extendedAuditingSettings/write | Verilen bir veritabanı için genişletilmiş blob denetim ilkesini değiştirme |
> | Eylem | Microsoft.Sql/servers/databases/extensions/read | Veritabanı için uzantıları koleksiyonunu alır. |
> | Eylem | Microsoft.Sql/servers/databases/extensions/write | Verilen bir veritabanı uzantısı değiştirme |
> | Eylem | Microsoft.Sql/servers/databases/geoBackupPolicies/read | Verilen bir veritabanı için yedekleme ilkeleri coğrafi alma |
> | Eylem | Microsoft.Sql/servers/databases/geoBackupPolicies/write | Bir veritabanı geobackup İlkesi güncelle |
> | Eylem | Microsoft.Sql/servers/databases/importExportOperationResults/read | İlerleme-içeri/dışarı aktarma işlemlerini alır |
> | Eylem | Microsoft.Sql/servers/databases/metricDefinitions/read | Dönüş türleri veritabanları için kullanılabilir ölçümleri |
> | Eylem | Microsoft.Sql/servers/databases/metrics/read | Veritabanları için dönüş ölçümleri |
> | Eylem | Microsoft.Sql/servers/databases/move/action | Azure SQL veritabanını yeniden adlandırın |
> | Eylem | Microsoft.Sql/servers/databases/operationResults/read | Veritabanı işlemi durumunu alır. |
> | Eylem | Microsoft.Sql/servers/databases/operations/cancel/action | Azure SQL veritabanı henüz tamamlanmadı zaman uyumsuz işlemi iptal eder. |
> | Eylem | Microsoft.Sql/servers/databases/operations/read | Veritabanı üzerinde gerçekleştirilen işlemler listesini döndürür |
> | Eylem | Microsoft.Sql/servers/databases/pause/action | Duraklatma Azure SQL veri ambarı veritabanı |
> | Eylem | Microsoft.Sql/servers/databases/providers/Microsoft.Insights/diagnosticSettings/read | Kaynak için tanılama ayarını alır |
> | Eylem | Microsoft.Sql/servers/databases/providers/Microsoft.Insights/diagnosticSettings/write | Kaynak için tanılama ayarını oluşturur veya güncelleştirir |
> | Eylem | Microsoft.Sql/servers/databases/providers/Microsoft.Insights/logDefinitions/read | Veritabanları için kullanılabilir günlüklerini alır |
> | Eylem | Microsoft.Sql/servers/databases/providers/Microsoft.Insights/metricDefinitions/read | Dönüş türleri veritabanları için kullanılabilir ölçümleri |
> | Eylem | Microsoft.Sql/servers/databases/queryStore/queryTexts/read | Belirtilen parametrelere karşılık sorgu metinleri koleksiyonunu döndürür. |
> | Eylem | Microsoft.Sql/servers/databases/queryStore/read | Veritabanı için Query Store ayarlarının geçerli değerleri döndürür. |
> | Eylem | Microsoft.Sql/servers/databases/queryStore/write | Veritabanı için Query Store ayarı güncelleştirir |
> | Eylem | Microsoft.Sql/servers/databases/read | Veritabanları veya belirtilen veritabanı özelliklerini alır listesini döndürür. |
> | Eylem | Microsoft.Sql/servers/databases/replicationLinks/delete | Çoğaltma ilişkisinin zorla ve olası veri kaybı ile Sonlandır |
> | Eylem | Microsoft.Sql/servers/databases/replicationLinks/failover/action | Bu veritabanı çoğaltma relationship\u0027s birincil yapma ve uzak bir ikincil birincil yapmadan tüm eşitlemeden sonra Yük devretme birincil sunucudan değiştirir |
> | Eylem | Microsoft.Sql/servers/databases/replicationLinks/forceFailoverAllowDataLoss/action | Olası veri kaybı olmadan hemen bu veritabanı çoğaltma relationship\u0027s birincil yapma ve uzak bir ikincil birincil yapmadan yük devretme |
> | Eylem | Microsoft.Sql/servers/databases/replicationLinks/read | Belirli bir veritabanı için kurulmuş yineleme bağlantıları hakkında dönüş ayrıntıları |
> | Eylem | Microsoft.Sql/servers/databases/replicationLinks/unlink/action | Çoğaltma ilişkisinin zorla veya ortağıyla eşitlemeden sonra Sonlandır |
> | Eylem | Microsoft.Sql/servers/databases/replicationLinks/updateReplicationMode/action | Bağlantı zaman uyumlu veya zaman uyumsuz modu için güncelleştirme çoğaltma modu |
> | Eylem | Microsoft.Sql/servers/databases/restorePoints/action | Yeni bir geri yükleme noktası oluşturur |
> | Eylem | Microsoft.Sql/servers/databases/restorePoints/delete | Veritabanı için bir geri yükleme noktası siler. |
> | Eylem | Microsoft.Sql/servers/databases/restorePoints/read | Döndürür geri yükleme noktaları için veritabanı. |
> | Eylem | Microsoft.Sql/servers/databases/resume/action | Resume Azure SQL veri ambarı veritabanı |
> | Eylem | Microsoft.Sql/servers/databases/schemas/read | Veritabanı şemaları listesini alma |
> | Eylem | Microsoft.Sql/servers/databases/schemas/tables/columns/read | Bir tablodaki sütunların listesini alma |
> | Eylem | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/delete | Verili bir sütunun duyarlılık etiketini Sil |
> | Eylem | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/read | Verili bir sütunun duyarlılık etiketini Al |
> | Eylem | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/write | Verili bir sütunun duyarlılık etiketini güncelle |
> | Eylem | Microsoft.Sql/servers/databases/schemas/tables/read | Bir veritabanının tabloların listesini alma |
> | Eylem | Microsoft.Sql/servers/databases/schemas/tables/recommendedIndexes/read | Bir veritabanı üzerinde dizin önerileri listesini alma |
> | Eylem | Microsoft.Sql/servers/databases/schemas/tables/recommendedIndexes/write | Dizin önerisi Uygula |
> | Eylem | Microsoft.Sql/servers/databases/securityAlertPolicies/read | Belirli bir veritabanı üzerinde yapılandırılan tehdit algılama ilkesi ayrıntılarını alma |
> | Eylem | Microsoft.Sql/servers/databases/securityAlertPolicies/write | Verilen bir veritabanı tehdit algılama ilkesini değiştirme |
> | Eylem | Microsoft.Sql/servers/databases/securityMetrics/read | Veritabanı güvenlik ölçümleri koleksiyonunu alır |
> | Eylem | Microsoft.Sql/servers/databases/sensitivityLabels/read | Belirli bir veritabanının duyarlılık etiketleri listeleme |
> | Eylem | Microsoft.Sql/servers/databases/serviceTierAdvisors/read | Performansı veya maliyetini azaltmak için sorgu yürütme istatistikleri temel veritabanı yukarı veya aşağı Ölçeklendirmesi hakkında öneri Döndür |
> | Eylem | Microsoft.Sql/servers/databases/skus/read | Bir veritabanı için SKU'ları kullanılabilir bir koleksiyonunu alır |
> | Eylem | Microsoft.Sql/servers/databases/syncGroups/cancelSync/action | Eşitleme grubu eşitleme iptal et |
> | Eylem | Microsoft.Sql/servers/databases/syncGroups/delete | Var olan bir eşitleme grubunu siler. |
> | Eylem | Microsoft.Sql/servers/databases/syncGroups/hubSchemas/read | Hub veritabanı şemalarını eşitleme listesini döndürür |
> | Eylem | Microsoft.Sql/servers/databases/syncGroups/logs/read | Eşitleme grubu günlükleri listesini döndürür |
> | Eylem | Microsoft.Sql/servers/databases/syncGroups/read | Dönüş eşitleme listesi gruplar veya belirtilen eşitleme grubu için özellikleri alır. |
> | Eylem | Microsoft.Sql/servers/databases/syncGroups/refreshHubSchema/action | Eşitleme hub veritabanı şemasını Yenile |
> | Eylem | Microsoft.Sql/servers/databases/syncGroups/refreshHubSchemaOperationResults/read | Eşitleme hub şema yenileme işleminin sonucu alma |
> | Eylem | Microsoft.Sql/servers/databases/syncGroups/syncMembers/delete | Var olan bir eşitleme üye siler. |
> | Eylem | Microsoft.Sql/servers/databases/syncGroups/syncMembers/read | Belirtilen eşitleme üyesi için eşitleme üyeleri veya alır özellikler listesini döndürür. |
> | Eylem | Microsoft.Sql/servers/databases/syncGroups/syncMembers/refreshSchema/action | Eşitleme üye şemasını Yenile |
> | Eylem | Microsoft.Sql/servers/databases/syncGroups/syncMembers/refreshSchemaOperationResults/read | Eşitleme üye şema yenileme işleminin sonucu alma |
> | Eylem | Microsoft.Sql/servers/databases/syncGroups/syncMembers/schemas/read | Üye veritabanı şemalarını eşitleme listesini döndürür |
> | Eylem | Microsoft.Sql/servers/databases/syncGroups/syncMembers/write | Eşitleme üyesi belirtilen parametrelerle oluşturur veya belirtilen eşitleme üyesi için özelliklerini güncelleştirir. |
> | Eylem | Microsoft.Sql/servers/databases/syncGroups/triggerSync/action | Tetikleyici eşitleme grubu eşitleme |
> | Eylem | Microsoft.Sql/servers/databases/syncGroups/write | Belirtilen parametrelerle bir eşitleme grubu oluşturur veya belirtilen eşitleme grubu özelliklerini güncelleştirin. |
> | Eylem | Microsoft.Sql/servers/databases/topQueries/queryText/action | Seçili sorgu kimliği için Transact-SQL metnini döndürür |
> | Eylem | Microsoft.Sql/servers/databases/topQueries/read | Döndürür seçili sorgu için çalışma zamanı istatistikleri seçilen zaman aralığı içinde toplanır. |
> | Eylem | Microsoft.Sql/servers/databases/topQueries/statistics/read | Döndürür seçili sorgu için çalışma zamanı istatistikleri seçilen zaman aralığı içinde toplanır. |
> | Eylem | Microsoft.Sql/servers/databases/transparentDataEncryption/operationResults/read | İlerleme-saydam veri şifreleme işlemleri alır |
> | Eylem | Microsoft.Sql/servers/databases/transparentDataEncryption/read | Durum ve verilen bir veritabanı için saydam veri şifreleme güvenlik özelliği ayrıntılarını alma |
> | Eylem | Microsoft.Sql/servers/databases/transparentDataEncryption/write | Saydam veri şifreleme durumunu değiştirme |
> | Eylem | Microsoft.Sql/servers/databases/upgradeDataWarehouse/action | Azure SQL veri ambarı veritabanı yükseltme |
> | Eylem | Microsoft.Sql/servers/databases/usages/read | Azure SQL veritabanı kullanımları bilgilerini alır |
> | Eylem | Microsoft.Sql/servers/databases/vulnerabilityAssessments/delete | Verilen bir veritabanı için güvenlik açığı değerlendirmesi Kaldır |
> | Eylem | Microsoft.Sql/servers/databases/vulnerabilityAssessments/read | Belirli bir veritabanı üzerinde yapılandırılan güvenlik açığı değerlendirmesi ayrıntılarını alma |
> | Eylem | Microsoft.Sql/servers/databases/vulnerabilityAssessments/rules/baselines/delete | Verilen bir veritabanı için güvenlik açığı değerlendirmesi kural temel Kaldır |
> | Eylem | Microsoft.Sql/servers/databases/vulnerabilityAssessments/rules/baselines/read | Verilen bir veritabanı için güvenlik açığı değerlendirmesi kural taban Al |
> | Eylem | Microsoft.Sql/servers/databases/vulnerabilityAssessments/rules/baselines/write | Verilen bir veritabanı için güvenlik açığı değerlendirmesi kural temel değiştirme |
> | Eylem | Microsoft.Sql/servers/databases/vulnerabilityAssessments/scans/export/action | Varolan bir tarama sonuç bir insan okunabilir biçimine dönüştürün. Hiçbir şey zaten olur |
> | Eylem | Microsoft.Sql/servers/databases/vulnerabilityAssessments/scans/initiateScan/action | Güvenlik Açığı değerlendirmesi veritabanı taraması yürütün. |
> | Eylem | Microsoft.Sql/servers/databases/vulnerabilityAssessments/scans/read | Veritabanı güvenlik açığı listesi değerlendirme taraması kayıtları dönün veya belirtilen tarama kimliği için tarama kayıt Al |
> | Eylem | Microsoft.Sql/servers/databases/vulnerabilityAssessments/write | Verilen bir veritabanı için güvenlik açığı değerlendirmesi değiştirme |
> | Eylem | Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/action | Güvenlik Açığı değerlendirmesi veritabanı taraması yürütün. |
> | Eylem | Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/operationResults/read | Veritabanı güvenlik açığı değerlendirmesi taraması yürütme işlemini sonucunu alma |
> | Eylem | Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/read | Belirli bir veritabanı üzerinde yapılandırılan güvenlik açığı değerlendirmesi ayrıntılarını alma |
> | Eylem | Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/write | Verilen bir veritabanı için güvenlik açığı değerlendirmesi değiştirme |
> | Eylem | Microsoft.Sql/servers/databases/write | Belirtilen parametrelerle bir veritabanı oluşturur veya özellikleri veya etiketleri belirtilen veritabanı için güncelleştirin. |
> | Eylem | Microsoft.Sql/servers/delete | Var olan bir sunucuyu siler. |
> | Eylem | Microsoft.Sql/servers/disasterRecoveryConfiguration/delete | Verilen bir sunucu için mevcut bir olağanüstü durum kurtarma yapılandırmaları siler |
> | Eylem | Microsoft.Sql/servers/disasterRecoveryConfiguration/failover/action | Yük devretme bir DisasterRecoveryConfiguration |
> | Eylem | Microsoft.Sql/servers/disasterRecoveryConfiguration/forceFailoverAllowDataLoss/action | Yük devretme bir DisasterRecoveryConfiguration zorla |
> | Eylem | Microsoft.Sql/servers/disasterRecoveryConfiguration/read | Bu sunucu içeren kurtarma yapılandırmalar olağanüstü bir koleksiyonunu alır |
> | Eylem | Microsoft.Sql/servers/disasterRecoveryConfiguration/write | Sunucu olağanüstü durum kurtarma yapılandırmasını değiştirme |
> | Eylem | Microsoft.Sql/servers/elasticPoolEstimates/read | Zaten bu sunucu için oluşturulan esnek havuz tahminler listesini döndürür |
> | Eylem | Microsoft.Sql/servers/elasticPoolEstimates/write | Sağlanan veritabanı listesi için yeni esnek havuz tahmini oluşturur |
> | Eylem | Microsoft.Sql/servers/elasticPools/advisors/read | Esnek havuz için kullanılabilir danışmanlar listesini döndürür |
> | Eylem | Microsoft.Sql/servers/elasticPools/advisors/recommendedActions/read | Esnek havuz için belirtilen Danışmanı'nın önerilen eylemleri listesini döndürür |
> | Eylem | Microsoft.Sql/servers/elasticPools/advisors/recommendedActions/write | Esnek havuz üzerinde önerilen eylemi uygulayın |
> | Eylem | Microsoft.Sql/servers/elasticPools/advisors/write | Güncelleştirme otomatik yürütme esnek havuz düzeyde bir advisor durumu. |
> | Eylem | Microsoft.Sql/servers/elasticPools/databases/read | Esnek havuz için veritabanı listesi alır |
> | Eylem | Microsoft.Sql/servers/elasticPools/delete | Var olan esnek havuzunu silme |
> | Eylem | Microsoft.Sql/servers/elasticPools/elasticPoolActivity/read | Etkinlikleri ve verilen esnek veritabanı havuzu ayrıntıları alma |
> | Eylem | Microsoft.Sql/servers/elasticPools/elasticPoolDatabaseActivity/read | Etkinlikleri ve esnek veritabanı havuzu parçası olan belirli bir veritabanı üzerinde ayrıntıları alma |
> | Eylem | Microsoft.Sql/servers/elasticPools/metricDefinitions/read | Dönüş türleri esnek veritabanı havuzları için kullanılabilir ölçümleri |
> | Eylem | Microsoft.Sql/servers/elasticPools/metrics/read | Esnek veritabanı havuzları için dönüş ölçümleri |
> | Eylem | Microsoft.Sql/servers/elasticPools/operations/cancel/action | Azure SQL esnek havuzu bekleyen henüz tamamlanmadı zaman uyumsuz işlemi iptal eder. |
> | Eylem | Microsoft.Sql/servers/elasticPools/operations/read | Esnek havuz üzerinde gerçekleştirilen işlemler listesini döndürür |
> | Eylem | Microsoft.Sql/servers/elasticPools/providers/Microsoft.Insights/diagnosticSettings/read | Kaynak için tanılama ayarını alır |
> | Eylem | Microsoft.Sql/servers/elasticPools/providers/Microsoft.Insights/diagnosticSettings/write | Kaynak için tanılama ayarını oluşturur veya güncelleştirir |
> | Eylem | Microsoft.Sql/servers/elasticPools/providers/Microsoft.Insights/metricDefinitions/read | Dönüş türleri esnek veritabanı havuzları için kullanılabilir ölçümleri |
> | Eylem | Microsoft.Sql/servers/elasticPools/read | Belirli bir sunucuda esnek havuz ayrıntılarını alma |
> | Eylem | Microsoft.Sql/servers/elasticPools/skus/read | Esnek havuz için SKU'ları kullanılabilir bir koleksiyonunu alır |
> | Eylem | Microsoft.Sql/servers/elasticPools/write | Yeni bir oluşturun veya var olan esnek havuz özelliklerini değiştir |
> | Eylem | Microsoft.Sql/servers/encryptionProtector/read | Sunucu şifreleme koruyucuları listesini döndürür veya belirtilen sunucu için şifreleme koruyucusu özellikleri alır. |
> | Eylem | Microsoft.Sql/servers/encryptionProtector/write | Belirtilen sunucu şifreleme koruyucusu için güncelleştirme. |
> | Eylem | Microsoft.Sql/servers/extendedAuditingSettings/read | Denetim İlkesi belirli bir sunucuda yapılandırılan genişletilmiş sunucusu blob ayrıntılarını alma |
> | Eylem | Microsoft.Sql/servers/extendedAuditingSettings/write | Genişletilmiş sunucu blob belli bir sunucu için Denetim değiştirme |
> | Eylem | Microsoft.Sql/servers/failoverGroups/delete | Var olan bir yük devretme grubunu siler. |
> | Eylem | Microsoft.Sql/servers/failoverGroups/failover/action | Planlanan yük devretme varolan bir yük devretme grubu yürütür. |
> | Eylem | Microsoft.Sql/servers/failoverGroups/forceFailoverAllowDataLoss/action | Varolan bir yük devretme grubu zorla yük devretme yürütür. |
> | Eylem | Microsoft.Sql/servers/failoverGroups/read | Gruplar veya özellikleri alır belirtilen yük devretme grubu için yük devretme listesi döndürür. |
> | Eylem | Microsoft.Sql/servers/failoverGroups/write | Belirtilen parametrelerle bir yük devretme grubu oluşturur veya özellikleri veya etiketleri belirtilen yük devretme grubu için güncelleştirir. |
> | Eylem | Microsoft.Sql/servers/firewallRules/delete | Mevcut bir sunucu güvenlik duvarı kuralını siler. |
> | Eylem | Microsoft.Sql/servers/firewallRules/read | Sunucu güvenlik duvarı listesi kuralları veya belirtilen sunucu için güvenlik duvarı kuralı özellikleri alır döndürür. |
> | Eylem | Microsoft.Sql/servers/firewallRules/write | Sunucu güvenlik duvarı kuralı oluşturur belirtilen parametrelerle belirtilen kural için özelliklerde güncelleştirmek veya yeni sunucu güvenlik duvarı kuralları ile tüm kuralların üzerine. |
> | Eylem | Microsoft.Sql/servers/import/action | Sunucuda yeni bir veritabanı oluşturun ve şeması ve verisi DacPac paketinden dağıtma |
> | Eylem | Microsoft.Sql/servers/importExportOperationResults/read | İlerleme-içeri/dışarı aktarma işlemlerini alır |
> | Eylem | Microsoft.Sql/servers/keys/delete | Mevcut bir sunucu anahtarı siler. |
> | Eylem | Microsoft.Sql/servers/keys/read | Dönüş sunucu listesini anahtarları veya belirtilen sunucu anahtarı için özellikleri alır. |
> | Eylem | Microsoft.Sql/servers/keys/write | Belirtilen parametrelerle bir anahtarı oluşturur veya özellikleri veya etiketleri belirtilen sunucu anahtarı için güncelleştirin. |
> | Eylem | Microsoft.Sql/servers/networkInterfaces/read | Belirtilen ağ arabirimi için özellikleri döndürür |
> | Eylem | Microsoft.Sql/servers/networkInterfaces/write | Belirtilen parametrelerle bir ağ arabirimi oluşturur veya özellikleri veya etiketleri belirtilen ağ arabirimi için güncelleştirir |
> | Eylem | Microsoft.Sql/servers/operationResults/read | İlerleme-sunucu işlemleri alır |
> | Eylem | Microsoft.Sql/servers/providers/Microsoft.Insights/metricDefinitions/read | Dönüş türleri sunucuları için kullanılabilir ölçümleri |
> | Eylem | Microsoft.Sql/servers/read | Sunucuları veya alır belirtilen sunucu için özellikler listesini döndürür. |
> | Eylem | Microsoft.Sql/servers/recommendedElasticPools/databases/read | Verilen bir sunucu için önerilen esnek veritabanı havuzlarına ilişkin ölçümleri alma |
> | Eylem | Microsoft.Sql/servers/recommendedElasticPools/read | Maliyet veya historica kaynak kullanımı temelinde performansını artırmak esnek veritabanı havuzları için öneri alma |
> | Eylem | Microsoft.Sql/servers/recoverableDatabases/read | Bu işlem, Canlı veritabanı olağanüstü durum kurtarma için bilinen son iyi yedekleme noktasına veritabanını geri yüklemek için kullanılır. Bilgi verir en son iyi yedeklemeden ancak hakkında doesn\u0027t gerçekten veritabanını geri yükle. |
> | Eylem | Microsoft.Sql/servers/restorableDroppedDatabases/read | Hala bekletme ilkesi içinde olan belirli bir sunucuda bırakılan veritabanlarının bir listesini alın. |
> | Eylem | Microsoft.Sql/servers/securityAlertPolicies/operationResults/read | Sunucu tehdit algılama İlkesi yazma işlemini sonuçlarını alma |
> | Eylem | Microsoft.Sql/servers/securityAlertPolicies/read | Verilen bir sunucu üzerinde yapılandırılmış sunucu tehdit algılama ilkesi ayrıntılarını alma |
> | Eylem | Microsoft.Sql/servers/securityAlertPolicies/write | Belirli bir sunucuda Sunucu tehdit algılama ilkesini değiştirme |
> | Eylem | Microsoft.Sql/servers/serviceObjectives/read | Hizmet düzeyi hedefleri (performans katmanı olarak da bilinir) belirli bir sunucuda bulunan listesini alma |
> | Eylem | Microsoft.Sql/servers/syncAgents/delete | Varolan bir eşitleme aracı siler. |
> | Eylem | Microsoft.Sql/servers/syncAgents/generateKey/action | Eşitleme Aracısı kayda anahtarı oluştur |
> | Eylem | Microsoft.Sql/servers/syncAgents/linkedDatabases/read | Eşitleme Aracısı bağlı veritabanlarının listesini döndürür |
> | Eylem | Microsoft.Sql/servers/syncAgents/read | Belirtilen eşitleme aracı için eşitleme aracıları veya alır özellikler listesini döndürür. |
> | Eylem | Microsoft.Sql/servers/syncAgents/write | Belirtilen parametrelerle bir eşitleme aracısı oluşturur veya belirtilen eşitleme aracı için güncelleştirme. |
> | Eylem | Microsoft.Sql/servers/tdeCertificates/action | Oluştur/güncelleştir TDE sertifika |
> | Eylem | Microsoft.Sql/servers/usages/read | Sunucu içindeki tüm veritabanları tarafından sunucu DTU kota ve geçerli DTU tüketim döndürür |
> | Eylem | Microsoft.Sql/servers/virtualNetworkRules/delete | Mevcut bir sanal ağ kuralını siler |
> | Eylem | Microsoft.Sql/servers/virtualNetworkRules/read | Dönüş sanal ağ listesi kuralları veya belirtilen sanal ağ kuralı özellikleri alır. |
> | Eylem | Microsoft.Sql/servers/virtualNetworkRules/write | Belirtilen parametrelerle bir sanal ağ kuralı oluşturur veya özellikleri veya etiketleri belirtilen sanal ağ kuralı için güncelleştirin. |
> | Eylem | Microsoft.Sql/servers/write | Sunucu belirtilen parametrelerle oluşturur veya özellikleri veya etiketleri belirtilen sunucu güncelleştirme. |
> | Eylem | Microsoft.Sql/unregister/action | Microsoft SQL veritabanı kaynak sağlayıcısı için abonelik kaydını siler ve Microsoft SQL veritabanları oluşturulmasını sağlar. |
> | Eylem | Microsoft.Sql/virtualClusters/read | Sanal kümelerin veya belirtilen sanal küme özelliklerini alır listesini döndürür. |
> | Eylem | Microsoft.Sql/virtualClusters/write | Sanal küme etiketleri güncelleştirir. |

## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Storage/checknameavailability/read | Hesap adının geçerliliğini ve kullanımda olup olmadığını denetler. |
> | Eylem | Microsoft.Storage/locations/deleteVirtualNetworkOrSubnets/action | Microsoft.Storage'a sanal ağın veya alt ağın silindiğini bildirir |
> | Eylem | Microsoft.Storage/locations/usages/read | Belirtilen abonelikteki kaynakların sınır ve geçerli kullanım sayısını döndürür |
> | Eylem | Microsoft.Storage/operations/read | Bir zaman uyumsuz işlemin durumunu yoklar. |
> | Eylem | Microsoft.Storage/register/action | Depolama kaynak sağlayıcısı için aboneliği kaydeder ve depolama hesaplarının oluşturulmasını etkinleştirir. |
> | Eylem | Microsoft.Storage/skus/read | Microsoft.Storage tarafından desteklenen SKU'ları listeler. |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/add/action | Blob içeriği eklemenin sonucunu döndürür |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete | Bir blobun silinmesinin sonucunu döndürür |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read | Bir blobu veya blob listesini döndürür |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/runAsSuperUser/action | Blob komutunun sonucunu döndürür |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write | Bir blobu yazmanın sonucunu döndürür |
> | Eylem | Microsoft.Storage/storageAccounts/blobServices/containers/clearLegalHold/action | Blob kapsayıcısı yasal tutmasını temizle |
> | Eylem | Microsoft.Storage/storageAccounts/blobServices/containers/delete | Bir kapsayıcının silinmesinin sonucunu döndürür |
> | Eylem | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/delete | Blob kapsayıcısı değiştirilemezlik ilkesini sil |
> | Eylem | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/extend/action | Blob kapsayıcısı değiştirilemezlik ilkesini genişlet |
> | Eylem | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/lock/action | Blob kapsayıcısı değitirilemezliğini kilitleme ilkesi |
> | Eylem | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/read | Blob kapsayıcısı değiştirilemezlik ilkesini al |
> | Eylem | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/write | Blob kapsayıcısı değitirilemezlik ilkesi koy |
> | Eylem | Microsoft.Storage/storageAccounts/blobServices/containers/read | Bir kapsayıcı veya bir kapsayıcı listesi döndürür |
> | Eylem | Microsoft.Storage/storageAccounts/blobServices/containers/setLegalHold/action | Blob kapsayıcısı yasal tutma ayarla |
> | Eylem | Microsoft.Storage/storageAccounts/blobServices/containers/write | Blob kapsayıcısı koyma veya kiralama sonucunu döndürür |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey/action | Blob hizmeti için bir kullanıcı temsilci anahtar döndürür |
> | Eylem | Microsoft.Storage/storageAccounts/blobServices/providers/Microsoft.Insights/diagnosticSettings/read | Kaynağın tanılama ayarını alır. |
> | Eylem | Microsoft.Storage/storageAccounts/blobServices/providers/Microsoft.Insights/diagnosticSettings/write | Kaynağın tanılama ayarını oluşturur veya güncelleştirir. |
> | Eylem | Microsoft.Storage/storageAccounts/blobServices/providers/Microsoft.Insights/logDefinitions/read | Blob için günlük tanımını döndürür |
> | Eylem | Microsoft.Storage/storageAccounts/blobServices/providers/Microsoft.Insights/metricDefinitions/read | Microsoft Depolama Ölçümleri tanımlarının listesini alın. |
> | Eylem | Microsoft.Storage/storageAccounts/blobServices/read | Blob hizmeti özelliklerini veya istatistiklerini döndürür |
> | Eylem | Microsoft.Storage/storageAccounts/blobServices/write | Blob hizmeti özelliklerini koymanın sonucunu döndürür |
> | Eylem | Microsoft.Storage/storageAccounts/delete | Mevcut bir depolama hesabını siler. |
> | Eylem | Microsoft.Storage/storageAccounts/fileServices/fileShare/delete | Kullanıcının dosya paylaşımını silmesine izin verir |
> | Eylem | Microsoft.Storage/storageAccounts/fileServices/fileShare/read | Kullanıcının dosya paylaşımını okumasına izin verir |
> | Eylem | Microsoft.Storage/storageAccounts/fileServices/fileShare/write | Kullanıcının bir dosya paylaşımına yazmasına izin verir |
> | Eylem | Microsoft.Storage/storageAccounts/fileServices/providers/Microsoft.Insights/diagnosticSettings/read | Kaynağın tanılama ayarını alır. |
> | Eylem | Microsoft.Storage/storageAccounts/fileServices/providers/Microsoft.Insights/diagnosticSettings/write | Kaynağın tanılama ayarını oluşturur veya güncelleştirir. |
> | Eylem | Microsoft.Storage/storageAccounts/fileServices/providers/Microsoft.Insights/logDefinitions/read | Dosya için günlük tanımını alır |
> | Eylem | Microsoft.Storage/storageAccounts/fileServices/providers/Microsoft.Insights/metricDefinitions/read | Microsoft Depolama Ölçümleri tanımlarının listesini alın. |
> | Eylem | Microsoft.Storage/storageAccounts/listAccountSas/action | Belirtilen depolama hesabı için Hesap SAS belirtecini döndürür. |
> | Eylem | Microsoft.Storage/storageAccounts/listkeys/action | Belirtilen depolama hesabının erişim anahtarlarını döndürür. |
> | Eylem | Microsoft.Storage/storageAccounts/listServiceSas/action | Belirtilen depolama hesabı için Hizmet SAS belirtecini döndürür. |
> | Eylem | Microsoft.Storage/storageAccounts/providers/Microsoft.Insights/diagnosticSettings/read | Kaynağın tanılama ayarını alır. |
> | Eylem | Microsoft.Storage/storageAccounts/providers/Microsoft.Insights/diagnosticSettings/write | Kaynağın tanılama ayarını oluşturur veya güncelleştirir. |
> | Eylem | Microsoft.Storage/storageAccounts/providers/Microsoft.Insights/metricDefinitions/read | Microsoft Depolama Ölçümleri tanımlarının listesini alın. |
> | Eylem | Microsoft.Storage/storageAccounts/queueServices/providers/Microsoft.Insights/diagnosticSettings/read | Kaynağın tanılama ayarını alır. |
> | Eylem | Microsoft.Storage/storageAccounts/queueServices/providers/Microsoft.Insights/diagnosticSettings/write | Kaynağın tanılama ayarını oluşturur veya güncelleştirir. |
> | Eylem | Microsoft.Storage/storageAccounts/queueServices/providers/Microsoft.Insights/logDefinitions/read | Kuyruk için günlük tanımını alır |
> | Eylem | Microsoft.Storage/storageAccounts/queueServices/providers/Microsoft.Insights/metricDefinitions/read | Microsoft Depolama Ölçümleri tanımlarının listesini alın. |
> | Eylem | Microsoft.Storage/storageAccounts/queueServices/queues/delete | Bir kuyruğu silmenin sonucunu döndürür |
> | DataAction | Microsoft.Storage/storageAccounts/queueServices/queues/messages/add/action | Bir ileti eklemenin sonucunu döndürür |
> | DataAction | Microsoft.Storage/storageAccounts/queueServices/queues/messages/delete | Bir iletiyi silmenin sonucunu döndürür |
> | DataAction | Microsoft.Storage/storageAccounts/queueServices/queues/messages/process/action | Bir iletiyi işlemenin sonucunu döndürür |
> | DataAction | Microsoft.Storage/storageAccounts/queueServices/queues/messages/read | Bir ileti döndürür |
> | DataAction | Microsoft.Storage/storageAccounts/queueServices/queues/messages/write | Bir iletiyi yazmanın sonucunu döndürür |
> | Eylem | Microsoft.Storage/storageAccounts/queueServices/queues/read | Bir kuyruğu veya kuyruk listesini döndürür. |
> | Eylem | Microsoft.Storage/storageAccounts/queueServices/queues/write | Bir kuyruğu yazmanın sonucunu döndürür |
> | Eylem | Microsoft.Storage/storageAccounts/queueServices/read | Kuyruk hizmeti özelliklerini veya istatistiklerini döndürür. |
> | Eylem | Microsoft.Storage/storageAccounts/queueServices/write | Kuyruk hizmeti özelliklerini ayarlamanın sonucunu döndürür |
> | Eylem | Microsoft.Storage/storageAccounts/read | Depolama hesaplarının listesini döndürür veya belirtilen depolama hesabının özelliklerini alır. |
> | Eylem | Microsoft.Storage/storageAccounts/regeneratekey/action | Belirtilen depolama hesabının erişim anahtarlarını yeniden oluşturur. |
> | Eylem | Microsoft.Storage/storageAccounts/services/diagnosticSettings/write | Depolama hesabı tanılama ayarlarını oluşturun/güncelleştirin. |
> | Eylem | Microsoft.Storage/storageAccounts/tableServices/providers/Microsoft.Insights/diagnosticSettings/read | Kaynağın tanılama ayarını alır. |
> | Eylem | Microsoft.Storage/storageAccounts/tableServices/providers/Microsoft.Insights/diagnosticSettings/write | Kaynağın tanılama ayarını oluşturur veya güncelleştirir. |
> | Eylem | Microsoft.Storage/storageAccounts/tableServices/providers/Microsoft.Insights/logDefinitions/read | Tablo için günlük tanımını alır |
> | Eylem | Microsoft.Storage/storageAccounts/tableServices/providers/Microsoft.Insights/metricDefinitions/read | Microsoft Depolama Ölçümleri tanımlarının listesini alın. |
> | Eylem | Microsoft.Storage/storageAccounts/write | Belirtilen parametrelerle bir depolama hesabı oluşturur, özellikleri veya etiketleri güncelleştirir ya da belirtilen depolama hesabına özel etki alanı ekler. |
> | Eylem | Microsoft.Storage/usages/read | Belirtilen abonelikteki kaynakların sınır ve geçerli kullanım sayısını döndürür |

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | microsoft.storagesync/storageSyncServices/delete | Tüm depolama Eşitleme Hizmetleri Sil |
> | Eylem | microsoft.storagesync/storageSyncServices/providers/Microsoft.Insights/metricDefinitions/read | Depolama Eşitleme Hizmetleri için kullanılabilir ölçümleri alır |
> | Eylem | microsoft.storagesync/storageSyncServices/read | Tüm depolama Eşitleme Hizmetleri okuma |
> | Eylem | microsoft.storagesync/storageSyncServices/registeredServers/delete | Herhangi bir kayıtlı sunucu silme |
> | Eylem | microsoft.storagesync/storageSyncServices/registeredServers/read | Herhangi bir kayıtlı sunucu okuma |
> | Eylem | microsoft.storagesync/storageSyncServices/registeredServers/write | Herhangi bir kayıtlı sunucu güncelle |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/delete | Bulut uç nokta silme |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/operationresults/read | Zaman uyumsuz yedekleme çağrıları için konum API |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/postbackup/action | Bu eylem yedeklemeden sonra çağırın |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/postrestore/action | Bu eylemi geri yükleme sonrasında çağırın |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/prebackup/action | Bu eylem yedekleme önce çağırın |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/prerestore/action | Bu eylem geri yükleme önce çağırın |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/read | Bulut uç nokta okuma |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/restoreheartbeat/action | Sinyal geri yükleme |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/write | Bulut uç nokta güncelle |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/delete | Herhangi bir eşitleme grubu Sil |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/read | Herhangi bir eşitleme grubu okuma |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/delete | Sunucu uç nokta silme |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/read | Sunucu uç nokta okuma |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/recallAction/action | Arama dosyaları bir sunucuya geri çekmek için bu eylemi |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/write | Sunucu uç nokta güncelle |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/write | Eşitleme grupları oluştur veya güncelleştir |
> | Eylem | microsoft.storagesync/storageSyncServices/write | Oluşturma veya güncelleştirme tüm depolama Eşitleme Hizmetleri |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.StorSimple/managers/accessControlRecords/delete | Erişim denetimi kayıtları siler |
> | Eylem | Microsoft.StorSimple/managers/accessControlRecords/read | Erişim denetimi kayıtları alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/accessControlRecords/write | Erişim denetimi kayıtları oluştur veya güncelleştir |
> | Eylem | Microsoft.StorSimple/managers/alerts/read | Uyarıları alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/bandwidthSettings/delete | Var olan bir bant genişliği ayarları siler (yalnızca 8000 Series) |
> | Eylem | Microsoft.StorSimple/managers/bandwidthSettings/read | Bant genişliği ayarlarını listesi (yalnızca 8000 Series) |
> | Eylem | Microsoft.StorSimple/managers/bandwidthSettings/write | Yeni bir oluşturur veya güncelleştirir bant genişliği ayarları (yalnızca 8000 Series) |
> | Eylem | Microsoft.StorSimple/Managers/certificates/write | Güncelleştirme kaynağı sertifika işlemi kaynak/kasa kimlik bilgileri sertifikası güncelleştirir. |
> | Eylem | Microsoft.StorSimple/managers/clearAlerts/action | Aygıt Yöneticisi ile ilişkili tüm uyarıları temizleyin. |
> | Eylem | Microsoft.StorSimple/managers/cloudApplianceConfigurations/read | Liste bulut uygulaması desteklenen yapılandırmalar |
> | Eylem | Microsoft.StorSimple/managers/configureDevice/action | Bir aygıt yapılandırır |
> | Eylem | Microsoft.StorSimple/managers/delete | Cihaz yöneticileri siler |
> | Eylem | Microsoft.StorSimple/Managers/delete | Kasa silme işlemi 'Kasası' türünde belirtilen Azure kaynak siler |
> | Eylem | Microsoft.StorSimple/managers/devices/alertSettings/read | Uyarı ayarlarını alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/alertSettings/write | Uyarı ayarlarını güncelle |
> | Eylem | Microsoft.StorSimple/managers/devices/backupPolicies/backup/action | İsteğe bağlı oluşturmak için el ile yedekleme İlkesi tarafından korunan tüm birimlerin yedek alın. |
> | Eylem | Microsoft.StorSimple/managers/devices/backupPolicies/delete | Mevcut bir yedekleme ilkeleri siler (yalnızca 8000 Series) |
> | Eylem | Microsoft.StorSimple/managers/devices/backupPolicies/read | (Yalnızca 8000 serisi) listesi yedekleme ilkeleri |
> | Eylem | Microsoft.StorSimple/managers/devices/backupPolicies/schedules/delete | Var olan zamanlamalar siler |
> | Eylem | Microsoft.StorSimple/managers/devices/backupPolicies/schedules/read | Zamanlamaları listesi |
> | Eylem | Microsoft.StorSimple/managers/devices/backupPolicies/schedules/write | Yeni bir oluşturur veya zamanlamaları güncelleştirir |
> | Eylem | Microsoft.StorSimple/managers/devices/backupPolicies/write | Yeni bir oluşturur veya yedekleme ilkeleri güncelleştirir (yalnızca 8000 Series) |
> | Eylem | Microsoft.StorSimple/managers/devices/backups/delete | Yedekleme kümesini siler |
> | Eylem | Microsoft.StorSimple/managers/devices/backups/elements/clone/action | Bir paylaşımı veya bir yedekleme öğesi kullanarak birimi kopyalama. |
> | Eylem | Microsoft.StorSimple/managers/devices/backups/read | Yedekleme kümesini alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/backups/restore/action | Tüm birimlerin bir yedekleme kümesinden geri yükleyin. |
> | Eylem | Microsoft.StorSimple/managers/devices/backupScheduleGroups/delete | Yedekleme zamanlaması gruplarını siler |
> | Eylem | Microsoft.StorSimple/managers/devices/backupScheduleGroups/read | Yedeklemeyi zamanlama grupları alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/backupScheduleGroups/write | Yedekleme zamanlaması grupları oluştur veya güncelleştir |
> | Eylem | Microsoft.StorSimple/managers/devices/chapSettings/delete | Chap ayarları siler |
> | Eylem | Microsoft.StorSimple/managers/devices/chapSettings/read | Chap ayarları alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/chapSettings/write | Chap ayarları güncelle |
> | Eylem | Microsoft.StorSimple/managers/devices/deactivate/action | Bir aygıtı devre dışı bırakır. |
> | Eylem | Microsoft.StorSimple/managers/devices/delete | Aygıtları siler |
> | Eylem | Microsoft.StorSimple/managers/devices/download/action | Bir aygıt için dowload güncelleştirmeler. |
> | Eylem | Microsoft.StorSimple/managers/devices/failover/action | Cihaz yük devretmesi. |
> | Eylem | Microsoft.StorSimple/managers/devices/fileservers/backup/action | Bir dosya sunucusunda yedekleme gerçekleştirin. |
> | Eylem | Microsoft.StorSimple/managers/devices/fileservers/delete | Dosya sunucuları siler |
> | Eylem | Microsoft.StorSimple/managers/devices/fileservers/metrics/read | Ölçümleri alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/fileservers/metricsDefinitions/read | Ölçüm tanımlarını alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/fileservers/read | Dosya sunucuları alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/fileservers/shares/delete | Paylaşımlar siler |
> | Eylem | Microsoft.StorSimple/managers/devices/fileservers/shares/metrics/read | Ölçümleri alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/fileservers/shares/metricsDefinitions/read | Ölçüm tanımlarını alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/fileservers/shares/read | Paylaşımlar alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/fileservers/shares/write | Paylaşımlar güncelle |
> | Eylem | Microsoft.StorSimple/managers/devices/fileservers/write | Dosya sunucuları güncelle |
> | Eylem | Microsoft.StorSimple/managers/devices/hardwareComponentGroups/changeControllerPowerState/action | Donanım bileşen grupları denetleyicisi güç durumunu değiştir |
> | Eylem | Microsoft.StorSimple/managers/devices/hardwareComponentGroups/read | Donanım bileşen grupları listesi |
> | Eylem | Microsoft.StorSimple/managers/devices/install/action | Bir cihazda güncelleştirmeleri yükleyin. |
> | Eylem | Microsoft.StorSimple/managers/devices/installUpdates/action | Aygıtlarda güncelleştirmeleri yükler |
> | Eylem | Microsoft.StorSimple/managers/devices/iscsiservers/backup/action | Bir iSCSI sunucusu yedek alın. |
> | Eylem | Microsoft.StorSimple/managers/devices/iscsiservers/delete | İSCSI sunucuları siler |
> | Eylem | Microsoft.StorSimple/managers/devices/iscsiservers/disks/delete | Diskleri siler |
> | Eylem | Microsoft.StorSimple/managers/devices/iscsiservers/disks/metrics/read | Ölçümleri alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/iscsiservers/disks/metricsDefinitions/read | Ölçüm tanımlarını alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/iscsiservers/disks/read | Diskleri alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/iscsiservers/disks/write | Diskleri güncelle |
> | Eylem | Microsoft.StorSimple/managers/devices/iscsiservers/metrics/read | Ölçümleri alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/iscsiservers/metricsDefinitions/read | Ölçüm tanımlarını alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/iscsiservers/read | İSCSI sunucuları alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/iscsiservers/write | İSCSI sunucuları güncelle |
> | Eylem | Microsoft.StorSimple/managers/devices/jobs/cancel/action | Bir çalışan işi iptal etme |
> | Eylem | Microsoft.StorSimple/managers/devices/jobs/read | İşlerini alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/listFailoverSets/action | Varolan bir aygıt için yük devretme kümeleri listeleyin. |
> | Eylem | Microsoft.StorSimple/managers/devices/listFailoverTargets/action | Aygıtların listesi yük devri hedefleri |
> | Eylem | Microsoft.StorSimple/managers/devices/metrics/read | Ölçümleri alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/metricsDefinitions/read | Ölçüm tanımlarını alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/confirmMigration/action | Başarılı bir geçiş doğrular ve onu uygulayın. |
> | Eylem | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/fetchConfirmMigrationStatus/action | Geçiş Onayla durumunu getirin. |
> | Eylem | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/fetchMigrationEstimate/action | Durum geçiş tahmin işinin getirin. |
> | Eylem | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/fetchMigrationStatus/action | Geçiş için durum getirin. |
> | Eylem | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/import/action | Geçiş için kaynak yapılandırmaları alma |
> | Eylem | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/startMigration/action | Kaynak yapılandırmaları kullanarak geçiş Başlat |
> | Eylem | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/startMigrationEstimate/action | Geçiş işleminin süresi tahmin etmek için bir proje başlatın. |
> | Eylem | Microsoft.StorSimple/managers/devices/networkSettings/read | Ağ ayarlarını alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/networkSettings/write | Yeni bir oluşturur veya ağ ayarlarını güncelleştirir |
> | Eylem | Microsoft.StorSimple/managers/devices/publicEncryptionKey/action | Aygıt Yöneticisi'ni listesi ortak şifreleme anahtarı |
> | Eylem | Microsoft.StorSimple/managers/devices/publishSupportPackage/action | Destek paketi Microsoft Support sorun giderme için bir cihazın yayımlayın. |
> | Eylem | Microsoft.StorSimple/managers/devices/read | Aygıtları alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/scanForUpdates/action | Bir cihaz güncelleştirmeleri için tarama. |
> | Eylem | Microsoft.StorSimple/managers/devices/securitySettings/read | Güvenlik ayarları listesi |
> | Eylem | Microsoft.StorSimple/managers/devices/securitySettings/syncRemoteManagementCertificate/action | Bir aygıtı için uzaktan yönetim sertifikası eşitleyin. |
> | Eylem | Microsoft.StorSimple/managers/devices/securitySettings/update/action | Güvenlik ayarlarını güncelleştirin. |
> | Eylem | Microsoft.StorSimple/managers/devices/securitySettings/write | Yeni bir oluşturur veya güvenlik ayarlarını güncelleştirir |
> | Eylem | Microsoft.StorSimple/managers/devices/sendTestAlertEmail/action | Yapılandırılan e-posta alıcılara test uyarı e-posta gönderin. |
> | Eylem | Microsoft.StorSimple/managers/devices/timeSettings/read | Saat ayarlarını alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/timeSettings/write | Yeni bir oluşturur veya saat ayarlarını güncelleştirir |
> | Eylem | Microsoft.StorSimple/managers/devices/updateSummary/read | Güncelleştirme özeti alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/volumeContainers/delete | Var olan bir birim kapsayıcıları siler (yalnızca 8000 Series) |
> | Eylem | Microsoft.StorSimple/managers/devices/volumeContainers/listEncryptionKeys/action | Birim kapsayıcıları şifreleme anahtarlarının listesi |
> | Eylem | Microsoft.StorSimple/managers/devices/volumeContainers/metrics/read | Ölçümleri listesi |
> | Eylem | Microsoft.StorSimple/managers/devices/volumeContainers/metricsDefinitions/read | Ölçümleri tanımları listesi |
> | Eylem | Microsoft.StorSimple/managers/devices/volumeContainers/read | Birim kapsayıcıları listesi (yalnızca 8000 Series) |
> | Eylem | Microsoft.StorSimple/managers/devices/volumeContainers/rolloverEncryptionKey/action | Geçiş şifreleme anahtarlarının birim kapsayıcıları |
> | Eylem | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/delete | Var olan birimler siler |
> | Eylem | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/metrics/read | Ölçümleri listesi |
> | Eylem | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/metricsDefinitions/read | Ölçümleri tanımları listesi |
> | Eylem | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/read | Birimleri listesi |
> | Eylem | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/write | Yeni bir oluşturur veya birimleri güncelleştirir |
> | Eylem | Microsoft.StorSimple/managers/devices/volumeContainers/write | Yeni bir oluşturur veya güncelleştirir birim kapsayıcıları (yalnızca 8000 Series) |
> | Eylem | Microsoft.StorSimple/managers/devices/write | Aygıtları güncelle |
> | Eylem | Microsoft.StorSimple/managers/encryptionSettings/read | Şifreleme ayarları alır veya listeler |
> | Eylem | Microsoft.StorSimple/Managers/extendedInformation/delete | Daha Fazla Bilgi Al işlemi, ?vault? türündeki Azure kaynağını temsil eden nesnenin Daha Fazla Bilgi değerini alır. |
> | Eylem | Microsoft.StorSimple/Managers/extendedInformation/read | Daha Fazla Bilgi Al işlemi, ?vault? türündeki Azure kaynağını temsil eden nesnenin Daha Fazla Bilgi değerini alır. |
> | Eylem | Microsoft.StorSimple/Managers/extendedInformation/write | Daha Fazla Bilgi Al işlemi, ?vault? türündeki Azure kaynağını temsil eden nesnenin Daha Fazla Bilgi değerini alır. |
> | Eylem | Microsoft.StorSimple/managers/getActivationKey/action | Etkinleştirme anahtarı için Aygıt Yöneticisi'ni alın. |
> | Eylem | Microsoft.StorSimple/managers/getEncryptionKey/action | Şifreleme anahtarı için Aygıt Yöneticisi'ni alma. |
> | Eylem | Microsoft.StorSimple/managers/listActivationKey/action | StorSimple cihaz Yöneticisi'nin etkinleştirme anahtarı alır. |
> | Eylem | Microsoft.StorSimple/managers/listPrivateEncryptionKey/action | Özel şifreleme anahtarı için bir StorSimple cihaz Yöneticisi alır. |
> | Eylem | Microsoft.StorSimple/managers/listPublicEncryptionKey/action | Ortak şifreleme anahtarlarını bir StorSimple cihaz Yöneticisi'nin listeleyin. |
> | Eylem | Microsoft.StorSimple/managers/metrics/read | Ölçümleri alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/metricsDefinitions/read | Ölçüm tanımlarını alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/provisionCloudAppliance/action | Yeni bir bulut uygulaması oluşturun. |
> | Eylem | Microsoft.StorSimple/managers/read | Cihaz Yöneticileri alır veya listeler |
> | Eylem | Microsoft.StorSimple/Managers/read | Kasa alma işlemi Azure kaynak türü 'Kasası' temsil eden bir nesneyi alır |
> | Eylem | Microsoft.StorSimple/managers/regenarateRegistationCertificate/action | Kayıt sertifikası için cihaz yöneticileri yeniden oluşturun. |
> | Eylem | Microsoft.StorSimple/managers/regenerateActivationKey/action | Etkinleştirme anahtarı için Aygıt Yöneticisi'ni yeniden oluşturun. |
> | Eylem | Microsoft.StorSimple/managers/storageAccountCredentials/delete | Depolama hesabı bilgilerini siler |
> | Eylem | Microsoft.StorSimple/managers/storageAccountCredentials/listAccessKey/action | Depolama hesabı kimlik bilgileri listesi erişim tuşları |
> | Eylem | Microsoft.StorSimple/managers/storageAccountCredentials/read | Depolama hesabı bilgilerini alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/storageAccountCredentials/write | Depolama hesabı bilgilerini güncelle |
> | Eylem | Microsoft.StorSimple/managers/storageDomains/delete | Depolama etki alanları siler |
> | Eylem | Microsoft.StorSimple/managers/storageDomains/read | Depolama etki alanları alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/storageDomains/write | Oluşturma veya güncelleme depolama etki alanları |
> | Eylem | Microsoft.StorSimple/managers/write | Cihaz yöneticileri güncelle |
> | Eylem | Microsoft.StorSimple/Managers/write | Kasa Oluştur işlemi, 'vault' türünde bir Azure kaynağı oluşturur |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.StreamAnalytics/locations/quotas/Read | Okuma Stream Analytics abonelik kotası |
> | Eylem | Microsoft.StreamAnalytics/operations/Read | Okuma akış analizi işlemlerini |
> | Eylem | Microsoft.StreamAnalytics/Register/action | Stream Analytics kaynak sağlayıcının abonelik kaydı |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/Delete | Akış analizi işi Sil |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/functions/Delete | Stream Analytics işi işlevi Sil |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/functions/operationresults/Read | Stream Analytics işi işlevi için okuma işlemi sonuçları |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/functions/Read | Okuma Stream Analytics işi işlevi |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/functions/RetrieveDefaultDefinition/action | Stream Analytics işi işlevi varsayılan tanımı alma |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/functions/Test/action | Test Stream Analytics işi işlevi |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/functions/Write | Stream Analytics işi işlevi yazma |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/inputs/Delete | Stream Analytics işi giriş silme |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/inputs/operationresults/Read | Stream Analytics işi giriş için okuma işlemi sonuçları |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/inputs/Read | Okuma Stream Analytics işi giriş |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/inputs/Sample/action | Örnek Stream Analytics işi giriş |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/inputs/Test/action | Test Stream Analytics işi giriş |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/inputs/Write | Stream Analytics işi giriş yazma |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/metricdefinitions/Read | Ölçüm tanımlarını oku |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/operationresults/Read | Akış analizi işi için okuma işlemi sonuçları |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/outputs/Delete | Stream Analytics işi çıkış Sil |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/outputs/operationresults/Read | Stream Analytics işi çıkış için okuma işlemi sonuçları |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/outputs/Read | Okuma Stream Analytics işi çıkış |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/outputs/Test/action | Test Stream Analytics işi çıkış |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/outputs/Write | Stream Analytics işi çıkış yazma |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/providers/Microsoft.Insights/diagnosticSettings/read | Tanılama ayarını okuyun. |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/providers/Microsoft.Insights/diagnosticSettings/write | Tanılama ayarını yazma. |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/providers/Microsoft.Insights/logDefinitions/read | Streamingjobs için kullanılabilir günlüklerini alır |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/providers/Microsoft.Insights/metricDefinitions/read | Streamingjobs için kullanılabilir ölçümleri alır |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/Read | Akış analizi işi okuma |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/Start/action | Akış analizi işi Başlat |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/Stop/action | Stream Analytics işini durdurma |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/transformations/Delete | Stream Analytics işi dönüştürme Sil |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/transformations/Read | Okuma Stream Analytics işi dönüştürme |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/transformations/Write | Stream Analytics işi dönüştürme yazma |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/Write | Akış analizi işi yazma |

## <a name="microsoftsubscription"></a>Microsoft.Subscription

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Subscription/CreateSubscription/action | Bir Azure aboneliği oluşturun |
> | Eylem | Microsoft.Subscription/SubscriptionDefinitions/read | Bir Azure aboneliği tanımı bir yönetim grubu içinde alın. |
> | Eylem | Microsoft.Subscription/SubscriptionDefinitions/write | Bir Azure aboneliği tanımı oluşturun |

## <a name="microsoftsupport"></a>Microsoft.Support

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Support/register/action | Destek Kaynağı Sağlayıcısına Kayıt Yapar |
> | Eylem | Microsoft.Support/supportTickets/read | Durum, önem derecesi, kişi ayrıntıları ve iletişimler gibi Destek Biletleri ayrıntılarını alır veya aboneliklerdeki Destek Biletleri listesini alır. |
> | Eylem | Microsoft.Support/supportTickets/write | Oluşturur veya bir destek bileti güncelleştirir. Bir destek bileti oluşturabilirsiniz teknik için faturalama, kota veya abonelik yönetimi ile ilgili sorunlar. Önem derecesi, kişi ayrıntıları ve iletişimler için mevcut destek biletlerinin güncelleştirebilirsiniz. |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.TimeSeriesInsights/environments/accesspolicies/delete | Erişim ilkesini siler. |
> | Eylem | Microsoft.TimeSeriesInsights/environments/accesspolicies/read | Bir erişim ilkesi özelliklerini alır. |
> | Eylem | Microsoft.TimeSeriesInsights/environments/accesspolicies/write | Bir ortam için yeni bir erişim ilkesi oluşturur veya mevcut bir erişim ilkesi güncelleştirir. |
> | Eylem | Microsoft.TimeSeriesInsights/environments/delete | Ortam siler. |
> | Eylem | Microsoft.TimeSeriesInsights/environments/eventsources/delete | Olay kaynağı siler. |
> | Eylem | Microsoft.TimeSeriesInsights/environments/eventsources/eventsources/providers/Microsoft.Insights/diagnosticSettings/write | Kaynak için tanılama ayarını oluşturur veya güncelleştirir |
> | Eylem | Microsoft.TimeSeriesInsights/environments/eventsources/providers/Microsoft.Insights/diagnosticSettings/read | Kaynak için tanılama ayarını alır |
> | Eylem | Microsoft.TimeSeriesInsights/environments/eventsources/providers/Microsoft.Insights/metricDefinitions/read | Eventsources için kullanılabilir ölçümleri alır |
> | Eylem | Microsoft.TimeSeriesInsights/environments/eventsources/read | Bir olay kaynağı özelliklerini alır. |
> | Eylem | Microsoft.TimeSeriesInsights/environments/eventsources/write | Bir ortam için yeni bir olay kaynağı oluşturur veya varolan bir olay kaynağına güncelleştirir. |
> | Eylem | Microsoft.TimeSeriesInsights/environments/providers/Microsoft.Insights/diagnosticSettings/read | Kaynak için tanılama ayarını alır |
> | Eylem | Microsoft.TimeSeriesInsights/environments/providers/Microsoft.Insights/diagnosticSettings/write | Kaynak için tanılama ayarını oluşturur veya güncelleştirir |
> | Eylem | Microsoft.TimeSeriesInsights/environments/providers/Microsoft.Insights/metricDefinitions/read | Ortamlar için kullanılabilir ölçümleri alır |
> | Eylem | Microsoft.TimeSeriesInsights/environments/read | Bir ortam özelliklerini alır. |
> | Eylem | Microsoft.TimeSeriesInsights/environments/referencedatasets/delete | Başvuru veri kümesini siler. |
> | Eylem | Microsoft.TimeSeriesInsights/environments/referencedatasets/read | Bir başvuru veri kümesinin özelliklerini alın. |
> | Eylem | Microsoft.TimeSeriesInsights/environments/referencedatasets/write | Bir ortam için yeni bir başvuru veri kümesi oluşturur veya varolan bir başvuru veri kümesini güncelleştirir. |
> | Eylem | Microsoft.TimeSeriesInsights/environments/status/read | Ortam, giriş gibi ilişkili işlemlerinin durumunu durumunu alın. |
> | Eylem | Microsoft.TimeSeriesInsights/environments/write | Yeni bir ortam oluşturur veya mevcut ortamında güncelleştirir. |
> | Eylem | Microsoft.TimeSeriesInsights/register/action | Zaman serisi Öngörüler kaynak sağlayıcısı için aboneliği kaydeder ve zaman serisi Öngörüler ortamları oluşturulmasını sağlar. |

## <a name="microsoftvisualstudio"></a>Microsoft.VisualStudio

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.VisualStudio/Account/Delete | Hesabı Sil |
> | Eylem | Microsoft.VisualStudio/Account/Read | Hesabının okuma |
> | Eylem | Microsoft.VisualStudio/Account/Write | Hesabını ayarlama |
> | Eylem | Microsoft.VisualStudio/Extension/Delete | Uzantısını Sil |
> | Eylem | Microsoft.VisualStudio/Extension/Read | Uzantı okuma |
> | Eylem | Microsoft.VisualStudio/Extension/Write | Uzantı ayarlayın |
> | Eylem | Microsoft.VisualStudio/Project/Delete | Projeyi Sil |
> | Eylem | Microsoft.VisualStudio/Project/Read | Okuma proje |
> | Eylem | Microsoft.VisualStudio/Project/Write | Proje ayarlama |

## <a name="microsoftweb"></a>microsoft.web

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Web/apimanagementaccounts/apiacls/Read | API yönetim hesapları Apiacls alın. |
> | Eylem | Microsoft.Web/apimanagementaccounts/apis/apiacls/DELETE | API yönetim hesapları API'leri Apiacls silin. |
> | Eylem | Microsoft.Web/apimanagementaccounts/apis/apiacls/Read | API yönetim hesapları API'leri Apiacls alın. |
> | Eylem | Microsoft.Web/apimanagementaccounts/apis/apiacls/Write | API yönetim hesapları API'leri Apiacls güncelleştirin. |
> | Eylem | Microsoft.Web/apimanagementaccounts/apis/connectionacls/Read | API yönetim hesapları API'leri Connectionacls alın. |
> | Eylem | Microsoft.Web/apimanagementaccounts/apis/Connections/confirmconsentcode/Action | Onay kodu API yönetim hesapları API'leri bağlantıları onaylayın. |
> | Eylem | Microsoft.Web/apimanagementaccounts/apis/Connections/connectionacls/DELETE | API yönetim hesapları API'leri bağlantıları Connectionacls silin. |
> | Eylem | Microsoft.Web/apimanagementaccounts/apis/Connections/connectionacls/Read | API yönetim hesapları API'leri bağlantıları Connectionacls alın. |
> | Eylem | Microsoft.Web/apimanagementaccounts/apis/Connections/connectionacls/Write | API yönetim hesapları API'leri bağlantıları Connectionacls güncelleştirin. |
> | Eylem | Microsoft.Web/apimanagementaccounts/apis/Connections/DELETE | API yönetim hesapları API'leri bağlantıları silin. |
> | Eylem | Microsoft.Web/apimanagementaccounts/apis/Connections/getconsentlinks/Action | API yönetim hesapları API'leri bağlantıları için onay bağlantılar alın. |
> | Eylem | Microsoft.Web/apimanagementaccounts/apis/Connections/listconnectionkeys/Action | Liste bağlantı anahtarları API yönetim hesapları API'leri bağlantılar. |
> | Eylem | Microsoft.Web/apimanagementaccounts/apis/Connections/listsecrets/Action | Liste gizli API yönetim hesapları API'leri bağlantıları. |
> | Eylem | Microsoft.Web/apimanagementaccounts/apis/Connections/Read | API yönetim hesapları API'leri bağlantıları alırlar. |
> | Eylem | Microsoft.Web/apimanagementaccounts/apis/Connections/Write | API yönetim hesapları API'leri bağlantıları güncelleştirin. |
> | Eylem | Microsoft.Web/apimanagementaccounts/apis/DELETE | API yönetim hesapları API'ları silin. |
> | Eylem | Microsoft.Web/apimanagementaccounts/apis/localizeddefinitions/DELETE | API Management silme hesapları API'leri yerelleştirilmiş tanımları. |
> | Eylem | Microsoft.Web/apimanagementaccounts/apis/localizeddefinitions/Read | API Management alma hesapları API'leri yerelleştirilmiş tanımları. |
> | Eylem | Microsoft.Web/apimanagementaccounts/apis/localizeddefinitions/Write | Güncelleştirme API yönetim hesapları API'leri tanımları yerelleştirilmiş. |
> | Eylem | Microsoft.Web/apimanagementaccounts/apis/Read | API yönetim hesapları API'leri alın. |
> | Eylem | Microsoft.Web/apimanagementaccounts/apis/Write | API yönetim hesapları API'leri güncelleştirin. |
> | Eylem | Microsoft.Web/apimanagementaccounts/connectionacls/Read | API yönetim hesapları Connectionacls alın. |
> | Eylem | Microsoft.Web/availablestacks/Read | Kullanılabilir yığınları alın. |
> | Eylem | Microsoft.Web/billingmeters/Read | Metre faturalama listesini alın. |
> | Eylem | Microsoft.Web/certificates/Delete | Varolan bir sertifikayı silin. |
> | Eylem | Microsoft.Web/certificates/Read | Sertifikaların listesini alın. |
> | Eylem | Microsoft.Web/certificates/Write | Yeni bir sertifika ekleyin veya mevcut bir güncelleştirin. |
> | Eylem | Microsoft.Web/checknameavailability/Read | Kaynak adı kullanılabilir olup olmadığını denetleyin. |
> | Eylem | Microsoft.Web/classicmobileservices/Read | Klasik mobil hizmet edinebilirsiniz. |
> | Eylem | Microsoft.Web/connectionGateways/Delete | Bir bağlantı ağ geçidi siler. |
> | Eylem | Microsoft.Web/connectionGateways/Join/Action | Bir bağlantı ağ geçidi birleştirir. |
> | Eylem | Microsoft.Web/connectionGateways/ListStatus/Action | Bir bağlantı ağ geçidi durumunu listeler. |
> | Eylem | Microsoft.Web/connectionGateways/Move/Action | Bir bağlantı ağ geçidi taşır. |
> | Eylem | Microsoft.Web/connectionGateways/Read | Bağlantı ağ geçidi listesi alın. |
> | Eylem | Microsoft.Web/connectionGateways/Write | Oluşturur veya bir bağlantı ağ geçidi güncelleştirir. |
> | Eylem | Microsoft.Web/Connections/confirmconsentcode/Action | Bağlantıları onay kodunu onaylayın. |
> | Eylem | Microsoft.Web/connections/Delete | Bir bağlantı siler. |
> | Eylem | Microsoft.Web/connections/Join/Action | Bir bağlantı birleştirir. |
> | Eylem | Microsoft.Web/Connections/listconsentlinks/Action | Bağlantılar için liste onay bağlantılar. |
> | Eylem | Microsoft.Web/connections/Move/Action | Bir bağlantıyı taşır. |
> | Eylem | Microsoft.Web/connections/Read | Bağlantıların listesini alır. |
> | Eylem | Microsoft.Web/connections/Write | Oluşturur veya bağlantı güncelleştirir. |
> | Eylem | Microsoft.Web/customApis/Delete | Özel bir API siler. |
> | Eylem | Microsoft.Web/customApis/extractApiDefinitionFromWsdl/Action | API tanımı WSDL ayıklar. |
> | Eylem | Microsoft.Web/customApis/Join/Action | Özel bir API birleştirir. |
> | Eylem | Microsoft.Web/customApis/listWsdlInterfaces/Action | WSDL arabirimleri için bir özel API listeler. |
> | Eylem | Microsoft.Web/customApis/Move/Action | Özel bir API taşır. |
> | Eylem | Microsoft.Web/customApis/Read | Özel API listesini alın. |
> | Eylem | Microsoft.Web/customApis/Write | Oluşturur veya özel API güncelleştirir. |
> | Eylem | Microsoft.Web/deletedSites/Read | Silinmiş bir Web uygulaması özelliklerini alır |
> | Eylem | Microsoft.Web/deploymentlocations/Read | Dağıtım konumlarında alın. |
> | Eylem | Microsoft.Web/geoRegions/Read | Coğrafi bölgelerin listesini alın. |
> | Eylem | Microsoft.Web/hostingenvironments/capacities/Read | Ortamlar kapasiteleri barındırma alın. |
> | Eylem | Microsoft.Web/hostingEnvironments/Delete | Bir uygulama hizmeti ortamını silme |
> | Eylem | Microsoft.Web/hostingenvironments/detectors/Read | Ortamlar algılayıcılar barındırma alın. |
> | Eylem | Microsoft.Web/hostingenvironments/Diagnostics/Read | Ortamlar tanılama barındırma alın. |
> | Eylem | Microsoft.Web/hostingenvironments/inboundnetworkdependenciesendpoints/Read | Tüm gelen bağımlılıkları ağ uç noktalarına alın. |
> | Eylem | Microsoft.Web/hostingenvironments/metricdefinitions/Read | Ortamlar ölçüm tanımlarını barındırma alın. |
> | Eylem | Microsoft.Web/hostingenvironments/multirolepools/metricdefinitions/Read | Ortamlar birden havuzları ölçüm tanımlarını barındırma alın. |
> | Eylem | Microsoft.Web/hostingenvironments/multirolepools/Metrics/Read | Ortamlar birden havuzları ölçümleri barındırma alın. |
> | Eylem | Microsoft.Web/hostingEnvironments/multiRolePools/providers/Microsoft.Insights/metricDefinitions/Read | Uygulama hizmeti ortamı MultiRole için kullanılabilir ölçümleri alır |
> | Eylem | Microsoft.Web/hostingEnvironments/multiRolePools/Read | Bir uygulama hizmeti ortamı ön uç havuzunda özelliklerini alır |
> | Eylem | Microsoft.Web/hostingenvironments/multirolepools/skus/Read | Ortamlar birden havuzları SKU'ları barındırma alın. |
> | Eylem | Microsoft.Web/hostingenvironments/multirolepools/usages/Read | Ortamlar birden havuzları kullanımları barındırma alın. |
> | Eylem | Microsoft.Web/hostingEnvironments/multiRolePools/Write | Uygulama hizmeti ortamı'nda yeni bir ön uç havuzu oluşturmak veya mevcut bir güncelleştirme |
> | Eylem | Microsoft.Web/hostingenvironments/Operations/Read | Ortamlar işlemleri barındırma alın. |
> | Eylem | Microsoft.Web/hostingenvironments/outboundnetworkdependenciesendpoints/Read | Ağ uç noktalarına giden tüm bağımlılıkları alın. |
> | Eylem | microsoft.web/hostingenvironments/providers/Microsoft.Insights/diagnosticSettings/read | Kaynak için tanılama ayarını alır |
> | Eylem | microsoft.web/hostingenvironments/providers/Microsoft.Insights/diagnosticSettings/write | Kaynak için tanılama ayarını oluşturur veya güncelleştirir |
> | Eylem | Microsoft.Web/hostingEnvironments/Read | Bir uygulama hizmeti ortamı özelliklerini alır |
> | Eylem | Microsoft.Web/hostingEnvironments/reboot/Action | Uygulama hizmeti ortamı'nda tüm makineleri yeniden başlatın |
> | Eylem | Microsoft.Web/hostingenvironments/Resume/Action | Barındırma ortamları sürdürün. |
> | Eylem | Microsoft.Web/hostingenvironments/serverfarms/Read | Barındırma ortamları uygulama hizmeti planları alın. |
> | Eylem | Microsoft.Web/hostingenvironments/Sites/Read | Barındırma ortamları Web uygulamaları alın. |
> | Eylem | Microsoft.Web/hostingenvironments/suspend/Action | Barındırma ortamları askıya alın. |
> | Eylem | Microsoft.Web/hostingenvironments/usages/Read | Ortamlar kullanımları barındırma alın. |
> | Eylem | Microsoft.Web/hostingenvironments/workerpools/metricdefinitions/Read | Ortamlar Workerpools ölçüm tanımlarını barındırma alın. |
> | Eylem | Microsoft.Web/hostingenvironments/workerpools/Metrics/Read | Ortamlar Workerpools ölçümleri barındırma alın. |
> | Eylem | Microsoft.Web/hostingEnvironments/workerPools/providers/Microsoft.Insights/metricDefinitions/Read | Uygulama hizmeti ortamı WorkerPool için kullanılabilir ölçümleri alır |
> | Eylem | Microsoft.Web/hostingEnvironments/workerPools/Read | Bir uygulama hizmeti ortamı çalışan havuzunda özelliklerini alır |
> | Eylem | Microsoft.Web/hostingenvironments/workerpools/skus/Read | Ortamlar Workerpools SKU'ları barındırma alın. |
> | Eylem | Microsoft.Web/hostingenvironments/workerpools/usages/Read | Ortamlar Workerpools kullanımları barındırma alın. |
> | Eylem | Microsoft.Web/hostingEnvironments/workerPools/Write | Uygulama hizmeti ortamı'nda yeni bir çalışan havuzu oluşturmak veya mevcut bir güncelleştirme |
> | Eylem | Microsoft.Web/hostingEnvironments/Write | Yeni bir uygulama hizmeti ortamı oluşturma veya varolan bir güncelleştirme |
> | Eylem | Microsoft.Web/ishostingenvironmentnameavailable/Read | Barındırma ortamı adı olup olmadığını alır. |
> | Eylem | Microsoft.Web/ishostnameavailable/Read | Ana bilgisayar adı kullanılabilir olup olmadığını denetleyin. |
> | Eylem | Microsoft.Web/isusernameavailable/Read | Kullanıcı adı kullanılabilir olup olmadığını denetleyin. |
> | Eylem | Microsoft.Web/listSitesAssignedToHostName/Read | Ana bilgisayar adı için atanmış siteler adlarını alır. |
> | Eylem | Microsoft.Web/Locations/apioperations/Read | Konumları API işlemleri alın. |
> | Eylem | Microsoft.Web/Locations/connectiongatewayinstallations/Read | Konumları bağlantı ağ geçidi yüklemeleri alın. |
> | Eylem | Microsoft.Web/Locations/extractapidefinitionfromwsdl/Action | API tanımı konumları için WSDL ayıklayın. |
> | Eylem | Microsoft.Web/Locations/listwsdlinterfaces/Action | Konumların WSDL arabirimlerini listele. |
> | Eylem | Microsoft.Web/Locations/managedapis/apioperations/Read | Konumları yönetilen API işlemleri alın. |
> | Eylem | Microsoft.Web/locations/managedapis/Join/Action | Yönetilen bir API birleştirir. |
> | Eylem | Microsoft.Web/Locations/managedapis/Read | Konumları yönetilen API'ler alın. |
> | Eylem | Microsoft.Web/Operations/Read | İşlemleri alın. |
> | Eylem | Microsoft.Web/publishingusers/Read | Kullanıcılar yayımlanıyor alın. |
> | Eylem | Microsoft.Web/publishingusers/Write | Kullanıcılar yayımlanıyor güncelleştirin. |
> | Eylem | Microsoft.Web/recommendations/Read | Abonelikler için öneriler listesi alınamadı. |
> | Eylem | Microsoft.Web/Register/Action | Microsoft.Web kaynak sağlayıcısı abonelik için kaydolun. |
> | Eylem | Microsoft.Web/resourcehealthmetadata/Read | Kaynak sistem durumu meta veri alın. |
> | Eylem | Microsoft.Web/serverfarms/Capabilities/Read | App Service planları özelliklerini alın. |
> | Eylem | Microsoft.Web/serverfarms/Delete | Var olan bir uygulama hizmeti planı silme |
> | Eylem | Microsoft.Web/serverfarms/firstpartyapps/Settings/DELETE | Uygulama hizmeti planları birinci taraf uygulama ayarları silin. |
> | Eylem | Microsoft.Web/serverfarms/firstpartyapps/Settings/Read | Uygulama hizmeti planları birinci taraf uygulama ayarları alın. |
> | Eylem | Microsoft.Web/serverfarms/firstpartyapps/Settings/Write | Uygulama hizmeti planları birinci taraf uygulama ayarları güncelleştirin. |
> | Eylem | Microsoft.Web/serverfarms/hybridconnectionnamespaces/relays/DELETE | Uygulama hizmeti planları karma bağlantı ad alanları geçişler silin. |
> | Eylem | Microsoft.Web/serverfarms/hybridconnectionnamespaces/relays/Read | Uygulama hizmeti planları karma bağlantı ad alanları geçişler alın. |
> | Eylem | Microsoft.Web/serverfarms/hybridconnectionnamespaces/relays/Sites/Read | Uygulama hizmeti planları karma bağlantı ad alanları geçişler Web uygulamalarını alır. |
> | Eylem | Microsoft.Web/serverfarms/hybridconnectionplanlimits/Read | Uygulama hizmeti planları karma bağlantı planı sınırlarını alın. |
> | Eylem | Microsoft.Web/serverfarms/hybridconnectionrelays/Read | Uygulama hizmeti planları karma bağlantı geçişler alın. |
> | Eylem | Microsoft.Web/serverfarms/metricdefinitions/Read | Uygulama hizmeti planları ölçüm tanımlarını alın. |
> | Eylem | Microsoft.Web/serverfarms/Metrics/Read | Uygulama hizmeti planları ölçümlerini alın. |
> | Eylem | Microsoft.Web/serverfarms/operationresults/Read | Uygulama hizmeti planları İşlem sonuçlarını alır. |
> | Eylem | microsoft.web/serverfarms/providers/Microsoft.Insights/diagnosticSettings/read | Kaynak için tanılama ayarını alır |
> | Eylem | microsoft.web/serverfarms/providers/Microsoft.Insights/diagnosticSettings/write | Kaynak için tanılama ayarını oluşturur veya güncelleştirir |
> | Eylem | Microsoft.Web/serverfarms/providers/Microsoft.Insights/metricDefinitions/Read | Uygulama hizmeti planı için kullanılabilir ölçümleri alır |
> | Eylem | Microsoft.Web/serverfarms/Read | Bir uygulama hizmeti planı üzerinde özelliklerini alma |
> | Eylem | Microsoft.Web/serverfarms/restartSites/Action | Tüm Web uygulamaları bir uygulama hizmeti planı'nda yeniden başlatın. |
> | Eylem | Microsoft.Web/serverfarms/Sites/Read | Uygulama hizmeti planları Web uygulamalarını alır. |
> | Eylem | Microsoft.Web/serverfarms/skus/Read | Uygulama hizmeti planları SKU'ları alır. |
> | Eylem | Microsoft.Web/serverfarms/usages/Read | Uygulama hizmeti planları kullanımları alın. |
> | Eylem | Microsoft.Web/serverfarms/virtualnetworkconnections/Gateways/Write | Uygulama hizmeti planları sanal ağ bağlantıları Gateway bileşenlerini güncelleştirin. |
> | Eylem | Microsoft.Web/serverfarms/virtualnetworkconnections/Read | Uygulama hizmeti planları sanal ağ bağlantıları alın. |
> | Eylem | Microsoft.Web/serverfarms/virtualnetworkconnections/Routes/DELETE | Uygulama hizmeti planları sanal ağ bağlantıları yolları silin. |
> | Eylem | Microsoft.Web/serverfarms/virtualnetworkconnections/Routes/Read | Uygulama hizmeti planları sanal ağ bağlantıları rotaları alabilir. |
> | Eylem | Microsoft.Web/serverfarms/virtualnetworkconnections/Routes/Write | Uygulama hizmeti planları sanal ağ bağlantıları yolları güncelleştirin. |
> | Eylem | Microsoft.Web/serverfarms/workers/reboot/Action | Uygulama hizmeti planları çalışanları yeniden başlatın. |
> | Eylem | Microsoft.Web/serverfarms/Write | Yeni bir uygulama hizmeti planı oluşturma veya var olan bir güncelleştirme |
> | Eylem | Microsoft.Web/Sites/analyzecustomhostname/Read | Özel ana bilgisayar adını analiz edin. |
> | Eylem | Microsoft.Web/sites/applySlotConfig/Action | Itanium tabanlı sistemler için Web uygulaması yuvası yapılandırması hedef yuvadan geçerli web uygulaması için geçerlidir |
> | Eylem | Microsoft.Web/sites/backup/Action | Yeni bir web uygulaması yedekleme oluşturma |
> | Eylem | Microsoft.Web/Sites/Backup/Read | Web uygulamaları yedek alın. |
> | Eylem | Microsoft.Web/Sites/Backup/Write | Web uygulamaları yedekleme güncelleştirin. |
> | Eylem | Microsoft.Web/Sites/Backups/Action | Azure storage'da bir blob geri yüklenebileceği var olan bir uygulama yedeğini bulur. |
> | Eylem | Microsoft.Web/Sites/Backups/DELETE | Web uygulama yedeklemeler silin. |
> | Eylem | Microsoft.Web/Sites/Backups/List/Action | Liste Web Apps yedeklemeler. |
> | Eylem | Microsoft.Web/sites/backups/Read | Web uygulamanızın yedekleme özelliklerini alır |
> | Eylem | Microsoft.Web/Sites/Backups/Restore/Action | Web uygulamaları yedekleri geri yükleyin. |
> | Eylem | Microsoft.Web/Sites/Backups/Write | Web uygulama yedeklemeler güncelleştirin. |
> | Eylem | Microsoft.Web/Sites/config/DELETE | Web uygulamaları Config silin. |
> | Eylem | Microsoft.Web/sites/config/list/Action | Kimlik bilgileri, uygulama ayarlarının ve bağlantı dizeleri yayımlama gibi Web uygulamanızın hassas, güvenlik ayarlarını Listele |
> | Eylem | Microsoft.Web/sites/config/Read | Web uygulaması yapılandırma ayarlarını al |
> | Eylem | Microsoft.Web/sites/config/Write | Web uygulamanızın yapılandırma ayarlarını güncelleştirme |
> | Eylem | Microsoft.Web/Sites/containerlogs/Action | Kapsayıcı günlükleri, Web uygulaması için daraltılmış. |
> | Eylem | Microsoft.Web/Sites/continuouswebjobs/DELETE | Web uygulamaları sürekli Web işleriniz silin. |
> | Eylem | Microsoft.Web/Sites/continuouswebjobs/Read | Web uygulamaları sürekli Web işleriniz alın. |
> | Eylem | Microsoft.Web/Sites/continuouswebjobs/Start/Action | Web uygulamaları sürekli Web işleriniz başlatın. |
> | Eylem | Microsoft.Web/Sites/continuouswebjobs/Stop/Action | Web uygulamaları sürekli Web işleriniz durdurun. |
> | Eylem | Microsoft.Web/sites/Delete | Var olan bir Web uygulamasının Sil |
> | Eylem | Microsoft.Web/Sites/Deployments/DELETE | Web uygulamaları dağıtımları silin. |
> | Eylem | Microsoft.Web/Sites/Deployments/log/Read | Web uygulamaları dağıtımları günlük alın. |
> | Eylem | Microsoft.Web/Sites/Deployments/Read | Web uygulamaları dağıtımları alın. |
> | Eylem | Microsoft.Web/Sites/Deployments/Write | Web uygulamaları dağıtımları güncelleştirin. |
> | Eylem | Microsoft.Web/Sites/detectors/Read | Web uygulamaları algılayıcılar alın. |
> | Eylem | microsoft.web/sites/diagnostics/analyses/execute/Action | Web Apps tanılama analiz çalıştırın. |
> | Eylem | Microsoft.Web/Sites/Diagnostics/Analyses/Read | Web Apps tanılama analiz alın. |
> | Eylem | Microsoft.Web/Sites/Diagnostics/aspnetcore/Read | Web Apps tanılama için ASP.NET Core uygulamasını alın. |
> | Eylem | Microsoft.Web/Sites/Diagnostics/autoheal/Read | Web Apps tanılama Autoheal alın. |
> | Eylem | Microsoft.Web/Sites/Diagnostics/Deployment/Read | Web Apps tanılama dağıtımı alın. |
> | Eylem | Microsoft.Web/Sites/Diagnostics/Deployments/Read | Web Apps tanılama dağıtımları alın. |
> | Eylem | microsoft.web/sites/diagnostics/detectors/execute/Action | Web Apps tanılama algılayıcısı çalıştırın. |
> | Eylem | Microsoft.Web/Sites/Diagnostics/detectors/Read | Web Apps tanılama algılayıcısı alın. |
> | Eylem | Microsoft.Web/Sites/Diagnostics/failedrequestsperuri/Read | Web Apps tanılama başarısız isteklerin URI başına alın. |
> | Eylem | Microsoft.Web/Sites/Diagnostics/frebanalysis/Read | Web Apps tanılama FREB analiz alın. |
> | Eylem | Microsoft.Web/Sites/Diagnostics/loganalyzer/Read | Web Apps tanılama günlük Çözümleyicisi alın. |
> | Eylem | Microsoft.Web/Sites/Diagnostics/Read | Web Apps tanılama kategorileri alın. |
> | Eylem | Microsoft.Web/Sites/Diagnostics/runtimeavailability/Read | Web Apps tanılama çalışma zamanı kullanılabilirliğini alın. |
> | Eylem | Microsoft.Web/Sites/Diagnostics/servicehealth/Read | Web Apps Tanılama Hizmeti durumunu alın. |
> | Eylem | Microsoft.Web/Sites/Diagnostics/sitecpuanalysis/Read | Web Apps tanılama Site CPU analiz alın. |
> | Eylem | Microsoft.Web/Sites/Diagnostics/sitecrashes/Read | Web Apps tanılama Site çökme (Crash) alın. |
> | Eylem | Microsoft.Web/Sites/Diagnostics/sitelatency/Read | Web Apps tanılama Site gecikme alın. |
> | Eylem | Microsoft.Web/Sites/Diagnostics/sitememoryanalysis/Read | Web Apps tanılama Site bellek analizi alın. |
> | Eylem | Microsoft.Web/Sites/Diagnostics/siterestartsettingupdate/Read | Web Apps tanılama Site yeniden ayarı güncelleştirmesi alın. |
> | Eylem | Microsoft.Web/Sites/Diagnostics/siterestartuserinitiated/Read | Web Apps tanılama Site yeniden başlatma kullanıcı tarafından başlatılan alın. |
> | Eylem | Microsoft.Web/Sites/Diagnostics/siteswap/Read | Web Apps tanılama Site takas alın. |
> | Eylem | Microsoft.Web/Sites/Diagnostics/THREADCOUNT/Read | Web Apps tanılama iş parçacığı sayısı alın. |
> | Eylem | Microsoft.Web/Sites/Diagnostics/workeravailability/Read | Web Apps tanılama Workeravailability alın. |
> | Eylem | Microsoft.Web/Sites/Diagnostics/workerprocessrecycle/Read | Web Apps tanılama çalışan işlem geri dönüştürme alın. |
> | Eylem | Microsoft.Web/Sites/domainownershipidentifiers/Read | Web Apps etki alanı sahipliğini tanımlayıcıları alın. |
> | Eylem | Microsoft.Web/Sites/domainownershipidentifiers/Write | Web Apps etki alanı sahipliğini tanımlayıcıları güncelleştirin. |
> | Eylem | Microsoft.Web/Sites/Functions/Action | İşlevler Web uygulamaları. |
> | Eylem | Microsoft.Web/Sites/Functions/DELETE | Web uygulamaları işlevleri silin. |
> | Eylem | Microsoft.Web/Sites/Functions/listsecrets/Action | Liste gizli Web Apps işlevleri. |
> | Eylem | Microsoft.Web/Sites/Functions/masterkey/Read | Web uygulamaları işlevleri Masterkey alın. |
> | Eylem | Microsoft.Web/Sites/Functions/Read | Web uygulamaları işlevleri alın. |
> | Eylem | Microsoft.Web/Sites/Functions/Token/Read | Get Web Apps işlevleri belirteci. |
> | Eylem | Microsoft.Web/Sites/Functions/Write | Web uygulamaları işlevleri güncelleştirin. |
> | Eylem | Microsoft.Web/Sites/hostnamebindings/DELETE | Web uygulamaları konak adı bağlamaları silin. |
> | Eylem | Microsoft.Web/Sites/hostnamebindings/Read | Web uygulamaları konak adı bağlamaları alın. |
> | Eylem | Microsoft.Web/Sites/hostnamebindings/Write | Web uygulamaları konak adı bağlamaları güncelleştirin. |
> | Eylem | Microsoft.Web/Sites/hybridconnection/DELETE | Web uygulamaları karma bağlantısını silin. |
> | Eylem | Microsoft.Web/Sites/hybridconnection/Read | Web uygulamaları karma bağlantı alın. |
> | Eylem | Microsoft.Web/Sites/hybridconnection/Write | Web uygulamaları karma bağlantı güncelleştirin. |
> | Eylem | Microsoft.Web/Sites/hybridconnectionnamespaces/relays/DELETE | Web uygulamaları karma bağlantı ad alanları geçişler silin. |
> | Eylem | Microsoft.Web/Sites/hybridconnectionnamespaces/relays/listkeys/Action | Liste anahtarları Web Apps karma bağlantı ad alanları geçişler. |
> | Eylem | Microsoft.Web/Sites/hybridconnectionnamespaces/relays/Read | Web uygulamaları karma bağlantı ad alanları geçişler alın. |
> | Eylem | Microsoft.Web/Sites/hybridconnectionnamespaces/relays/Write | Web uygulamaları karma bağlantı ad alanları geçişler güncelleştirin. |
> | Eylem | Microsoft.Web/Sites/hybridconnectionrelays/Read | Web uygulamaları karma bağlantı geçişler alın. |
> | Eylem | Microsoft.Web/Sites/instances/Deployments/DELETE | Web uygulamaları örnekleri dağıtımları silin. |
> | Eylem | Microsoft.Web/Sites/instances/Deployments/Read | Web uygulamaları örnekleri dağıtımları alın. |
> | Eylem | Microsoft.Web/Sites/instances/Extensions/log/Read | Web uygulamaları örnekleri uzantıları günlük alın. |
> | Eylem | Microsoft.Web/Sites/instances/Extensions/Read | Web uygulamaları örnekleri uzantıları alın. |
> | Eylem | Microsoft.Web/Sites/instances/Processes/DELETE | Web uygulamaları örnekleri işlemleri silin. |
> | Eylem | Microsoft.Web/Sites/instances/Processes/Read | Web uygulamaları örnekleri işlemleri alın. |
> | Eylem | Microsoft.Web/Sites/instances/Read | Web uygulamaları örnekleri alın. |
> | Eylem | Microsoft.Web/Sites/listsyncfunctiontriggerstatus/Action | Liste eşitleme işlevi tetikleyici durum Web uygulamaları. |
> | Eylem | Microsoft.Web/Sites/metricdefinitions/Read | Web uygulamaları ölçüm tanımlarını alın. |
> | Eylem | Microsoft.Web/Sites/Metrics/Read | Web uygulamaları ölçümlerini alın. |
> | Eylem | Microsoft.Web/Sites/metricsdefinitions/Read | Web uygulamaları ölçümleri tanımlarını alın. |
> | Eylem | Microsoft.Web/Sites/migratemysql/Action | MySql Web uygulamaları geçirme. |
> | Eylem | Microsoft.Web/Sites/migratemysql/Read | Web alma uygulamaları MySql geçirme. |
> | Eylem | Microsoft.Web/Sites/networktrace/Action | Ağ izleme Web uygulamaları. |
> | Eylem | Microsoft.Web/Sites/NewPassword/Action | #Newpassword Web uygulamaları. |
> | Eylem | Microsoft.Web/Sites/operationresults/Read | Web Apps İşlem sonuçlarını alır. |
> | Eylem | Microsoft.Web/Sites/Operations/Read | Web uygulamaları işlemleri alın. |
> | Eylem | Microsoft.Web/Sites/perfcounters/Read | Web uygulamaları performans sayaçlarını alın. |
> | Eylem | Microsoft.Web/Sites/premieraddons/DELETE | Web uygulamaları Premier alabilecekleri silin. |
> | Eylem | Microsoft.Web/Sites/premieraddons/Read | Web uygulamaları Premier alabilecekleri alın. |
> | Eylem | Microsoft.Web/Sites/premieraddons/Write | Web uygulamaları Premier alabilecekleri güncelleştirin. |
> | Eylem | Microsoft.Web/Sites/Processes/Read | Web uygulamaları işlemleri alın. |
> | Eylem | microsoft.web/sites/providers/Microsoft.Insights/diagnosticSettings/read | Kaynak için tanılama ayarını alır |
> | Eylem | microsoft.web/sites/providers/Microsoft.Insights/diagnosticSettings/write | Kaynak için tanılama ayarını oluşturur veya güncelleştirir |
> | Eylem | microsoft.web/sites/providers/Microsoft.Insights/logDefinitions/read | Web uygulaması için kullanılabilir günlüklerini alır |
> | Eylem | Microsoft.Web/sites/providers/Microsoft.Insights/metricDefinitions/Read | Web uygulaması için kullanılabilir ölçümleri alır |
> | Eylem | Microsoft.Web/Sites/publiccertificates/DELETE | Web Apps ortak sertifikalarını silin. |
> | Eylem | Microsoft.Web/Sites/publiccertificates/Read | Web Apps ortak sertifikaları alın. |
> | Eylem | Microsoft.Web/Sites/publiccertificates/Write | Web Apps ortak sertifikaları güncelleştirin. |
> | Eylem | Microsoft.Web/sites/publish/Action | Bir Web uygulaması yayımlama |
> | Eylem | Microsoft.Web/sites/publishxml/Action | Bir Web uygulaması için profil xml yayımlama Al |
> | Eylem | Microsoft.Web/Sites/publishxml/Read | Web uygulamalarını XML yayımlama alır. |
> | Eylem | Microsoft.Web/sites/Read | Bir Web uygulaması özelliklerini alır |
> | Eylem | Microsoft.Web/Sites/recommendationhistory/Read | Web uygulamaları öneri geçmişi alın. |
> | Eylem | Microsoft.Web/Sites/Recommendations/disable/Action | Web uygulamaları önerileri devre dışı bırakın. |
> | Eylem | Microsoft.Web/sites/recommendations/Read | Web uygulaması için öneriler listesi alınamadı. |
> | Eylem | Microsoft.Web/Sites/RECOVER/Action | Web uygulamaları kurtarın. |
> | Eylem | Microsoft.Web/sites/resetSlotConfig/Action | Web uygulaması yapılandırmasında Sıfırla |
> | Eylem | Microsoft.Web/Sites/resourcehealthmetadata/Read | Web uygulamaları kaynak sistem durumu meta veri alın. |
> | Eylem | Microsoft.Web/sites/restart/Action | Bir Web uygulaması yeniden |
> | Eylem | Microsoft.Web/Sites/Restore/Read | Web uygulamaları geri alın. |
> | Eylem | Microsoft.Web/Sites/Restore/Write | Web uygulamaları geri yükleyin. |
> | Eylem | Microsoft.Web/Sites/restorefromdeletedwebapp/Action | Web uygulamaları silinen uygulamadan geri yükleyin. |
> | Eylem | Microsoft.Web/Sites/restoresnapshot/Action | Web uygulamaları anlık görüntüleri geri yükleyin. |
> | Eylem | Microsoft.Web/Sites/siteextensions/DELETE | Web uygulamaları Site uzantıları silin. |
> | Eylem | Microsoft.Web/Sites/siteextensions/Read | Web uygulamaları Site uzantılarını Al. |
> | Eylem | Microsoft.Web/Sites/siteextensions/Write | Web uygulamaları Site uzantıları güncelleştirin. |
> | Eylem | Microsoft.Web/Sites/slots/analyzecustomhostname/Read | Web alma uygulamaları yuvaları analiz özel ana bilgisayar adı. |
> | Eylem | Microsoft.Web/sites/slots/applySlotConfig/Action | Itanium tabanlı sistemler için Web uygulaması yuvası yapılandırması hedef yuvadan geçerli yuvası için geçerlidir. |
> | Eylem | Microsoft.Web/sites/slots/backup/Action | Yeni Web uygulaması yuvası yedeği oluştur. |
> | Eylem | Microsoft.Web/Sites/slots/Backup/Read | Web uygulamaları yuvaları yedek alın. |
> | Eylem | Microsoft.Web/Sites/slots/Backup/Write | Web uygulamaları yuvaları yedeklemeyi güncelleştirin. |
> | Eylem | Microsoft.Web/Sites/slots/Backups/DELETE | Web uygulama yuvaları yedeklemeler silin. |
> | Eylem | Microsoft.Web/Sites/slots/Backups/List/Action | Liste Web Apps yuvaları yedeklemeler. |
> | Eylem | Microsoft.Web/sites/slots/backups/Read | Bir web uygulaması yuvaları yedekleme özelliklerini alır |
> | Eylem | Microsoft.Web/Sites/slots/Backups/Restore/Action | Web uygulamaları yuvaları yedekleri geri yükleyin. |
> | Eylem | Microsoft.Web/Sites/slots/config/DELETE | Web uygulamaları yuvaları Config silin. |
> | Eylem | Microsoft.Web/sites/slots/config/list/Action | Kimlik bilgileri, uygulama ayarlarının ve bağlantı dizeleri yayımlama gibi Web uygulama yuvanın hassas, güvenlik ayarlarını Listele |
> | Eylem | Microsoft.Web/sites/slots/config/Read | Web uygulaması yuvanın yapılandırma ayarlarını al |
> | Eylem | Microsoft.Web/sites/slots/config/Write | Web uygulaması yuvanın yapılandırma ayarlarını güncelleştirme |
> | Eylem | Microsoft.Web/Sites/slots/continuouswebjobs/DELETE | Web uygulamaları yuvaları sürekli Web işleriniz silin. |
> | Eylem | Microsoft.Web/Sites/slots/continuouswebjobs/Read | Web uygulamaları yuvaları sürekli Web işleriniz alın. |
> | Eylem | Microsoft.Web/Sites/slots/continuouswebjobs/Start/Action | Web uygulamaları yuvaları sürekli Web işleriniz başlatın. |
> | Eylem | Microsoft.Web/Sites/slots/continuouswebjobs/Stop/Action | Web uygulamaları yuvaları sürekli Web işleriniz durdurun. |
> | Eylem | Microsoft.Web/sites/slots/Delete | Var olan bir Web uygulaması yuvası Sil |
> | Eylem | Microsoft.Web/Sites/slots/Deployments/DELETE | Web uygulamaları yuvaları dağıtımları silin. |
> | Eylem | Microsoft.Web/Sites/slots/Deployments/log/Read | Web uygulamaları yuvaları dağıtımları günlük alın. |
> | Eylem | Microsoft.Web/Sites/slots/Deployments/Read | Web uygulamaları yuvaları dağıtımları alın. |
> | Eylem | Microsoft.Web/Sites/slots/Deployments/Write | Web uygulamaları yuvaları dağıtımları güncelleştirin. |
> | Eylem | microsoft.web/sites/slots/diagnostics/analyses/execute/Action | Web uygulamaları yuvaları tanılama analiz çalıştırın. |
> | Eylem | Microsoft.Web/Sites/slots/Diagnostics/Analyses/Read | Web uygulamaları yuvaları tanılama analiz alın. |
> | Eylem | Microsoft.Web/Sites/slots/Diagnostics/aspnetcore/Read | Web Apps yuvaları tanılama için ASP.NET Core uygulamasını alın. |
> | Eylem | Microsoft.Web/Sites/slots/Diagnostics/autoheal/Read | Web uygulamaları yuvaları tanılama Autoheal alın. |
> | Eylem | Microsoft.Web/Sites/slots/Diagnostics/Deployment/Read | Web uygulamaları yuvaları tanılama dağıtımı alın. |
> | Eylem | Microsoft.Web/Sites/slots/Diagnostics/Deployments/Read | Web uygulamaları yuvaları tanılama dağıtımları alın. |
> | Eylem | microsoft.web/sites/slots/diagnostics/detectors/execute/Action | Web uygulamaları yuvaları tanılama algılayıcısı çalıştırın. |
> | Eylem | Microsoft.Web/Sites/slots/Diagnostics/detectors/Read | Web uygulamaları yuvaları tanılama algılayıcısı alın. |
> | Eylem | Microsoft.Web/Sites/slots/Diagnostics/frebanalysis/Read | Web uygulamaları yuvaları tanılama FREB analiz alın. |
> | Eylem | Microsoft.Web/Sites/slots/Diagnostics/loganalyzer/Read | Web uygulamaları yuvaları tanılama günlük Çözümleyicisi alın. |
> | Eylem | Microsoft.Web/Sites/slots/Diagnostics/Read | Web Apps yuvaları tanılama alın. |
> | Eylem | Microsoft.Web/Sites/slots/Diagnostics/runtimeavailability/Read | Web uygulamaları yuvaları tanılama çalışma zamanı kullanılabilirliğini alın. |
> | Eylem | Microsoft.Web/Sites/slots/Diagnostics/servicehealth/Read | Web uygulamaları yuvaları tanılama hizmet durumunu Al |
> | Eylem | Microsoft.Web/Sites/slots/Diagnostics/sitecpuanalysis/Read | Web uygulamaları yuvaları tanılama Site CPU analiz alın. |
> | Eylem | Microsoft.Web/Sites/slots/Diagnostics/sitecrashes/Read | Web uygulamaları yuvaları tanılama Site kilitlenme alın. |
> | Eylem | Microsoft.Web/Sites/slots/Diagnostics/sitelatency/Read | Web uygulamaları yuvaları tanılama Site gecikme alın. |
> | Eylem | Microsoft.Web/Sites/slots/Diagnostics/sitememoryanalysis/Read | Web uygulamaları yuvaları tanılama Site bellek analizi alın. |
> | Eylem | Microsoft.Web/Sites/slots/Diagnostics/siterestartsettingupdate/Read | Web uygulamaları yuvaları tanılama Site yeniden ayarı güncelleştirmesi alın. |
> | Eylem | Microsoft.Web/Sites/slots/Diagnostics/siterestartuserinitiated/Read | Web uygulamaları yuvaları tanılama Site yeniden başlatma kullanıcı tarafından başlatılan alın. |
> | Eylem | Microsoft.Web/Sites/slots/Diagnostics/siteswap/Read | Web uygulamaları yuvaları tanılama Site takas alın. |
> | Eylem | Microsoft.Web/Sites/slots/Diagnostics/THREADCOUNT/Read | Web uygulamaları yuvaları tanılama iş parçacığı sayısı alın. |
> | Eylem | Microsoft.Web/Sites/slots/Diagnostics/workeravailability/Read | Web uygulamaları yuvaları tanılama Workeravailability alın. |
> | Eylem | Microsoft.Web/Sites/slots/Diagnostics/workerprocessrecycle/Read | Web uygulamaları yuvaları tanılama çalışan işlem geri dönüştürme alın. |
> | Eylem | Microsoft.Web/Sites/slots/domainownershipidentifiers/Read | Web uygulamaları yuvaları etki alanı sahipliğini tanımlayıcıları alın. |
> | Eylem | Microsoft.Web/Sites/slots/hostnamebindings/DELETE | Web uygulamaları yuvaları konak adı bağlamaları silin. |
> | Eylem | Microsoft.Web/Sites/slots/hostnamebindings/Read | Web uygulamaları yuvaları konak adı bağlamaları alın. |
> | Eylem | Microsoft.Web/Sites/slots/hostnamebindings/Write | Web uygulamaları yuvaları konak adı bağlamaları güncelleştirin. |
> | Eylem | Microsoft.Web/Sites/slots/hybridconnection/DELETE | Web uygulamaları yuvaları karma bağlantısını silin. |
> | Eylem | Microsoft.Web/Sites/slots/hybridconnection/Read | Web uygulamaları yuvaları karma bağlantı alın. |
> | Eylem | Microsoft.Web/Sites/slots/hybridconnection/Write | Web uygulamaları yuvaları karma bağlantı güncelleştirin. |
> | Eylem | Microsoft.Web/Sites/slots/hybridconnectionnamespaces/relays/DELETE | Web uygulamaları yuvaları karma bağlantı ad alanları geçişler silin. |
> | Eylem | Microsoft.Web/Sites/slots/hybridconnectionnamespaces/relays/Write | Web uygulamaları yuvaları karma bağlantı ad alanları geçişler güncelleştirin. |
> | Eylem | Microsoft.Web/Sites/slots/hybridconnectionrelays/Read | Web uygulamaları yuvaları karma bağlantı geçişler alın. |
> | Eylem | Microsoft.Web/Sites/slots/instances/Deployments/Read | Web uygulamaları yuvaları örnekleri dağıtımları alın. |
> | Eylem | Microsoft.Web/Sites/slots/instances/Processes/DELETE | Web uygulamaları yuvaları örnekleri işlemleri silin. |
> | Eylem | Microsoft.Web/Sites/slots/instances/Processes/Read | Web uygulamaları yuvaları örnekleri işlemleri alın. |
> | Eylem | Microsoft.Web/Sites/slots/instances/Read | Web uygulamaları yuvaları örnekleri alın. |
> | Eylem | Microsoft.Web/Sites/slots/metricdefinitions/Read | Web uygulamaları yuvaları ölçüm tanımlarını alın. |
> | Eylem | Microsoft.Web/Sites/slots/Metrics/Read | Web uygulamaları yuvaları ölçümlerini alın. |
> | Eylem | Microsoft.Web/Sites/slots/migratemysql/Read | Web alma uygulamaları yuvaları MySql geçirme. |
> | Eylem | Microsoft.Web/Sites/slots/networktrace/Action | Ağ izleme Web Apps yuvaları. |
> | Eylem | Microsoft.Web/Sites/slots/NewPassword/Action | #Newpassword Web Apps yuvaları. |
> | Eylem | Microsoft.Web/Sites/slots/operationresults/Read | Web uygulamaları yuvaları İşlem sonuçlarını alır. |
> | Eylem | Microsoft.Web/Sites/slots/Operations/Read | Web uygulamaları yuvaları işlemleri alın. |
> | Eylem | Microsoft.Web/Sites/slots/perfcounters/Read | Web uygulamaları yuvaları performans sayaçlarını alın. |
> | Eylem | Microsoft.Web/Sites/slots/phplogging/Read | Web uygulamaları yuvaları Phplogging alın. |
> | Eylem | Microsoft.Web/Sites/slots/premieraddons/DELETE | Web uygulamaları yuvaları Premier alabilecekleri silin. |
> | Eylem | Microsoft.Web/Sites/slots/premieraddons/Read | Web uygulamaları yuvaları Premier alabilecekleri alın. |
> | Eylem | Microsoft.Web/Sites/slots/premieraddons/Write | Web uygulamaları yuvaları Premier alabilecekleri güncelleştirin. |
> | Eylem | microsoft.web/sites/slots/providers/Microsoft.Insights/diagnosticSettings/read | Kaynak için tanılama ayarını alır |
> | Eylem | microsoft.web/sites/slots/providers/Microsoft.Insights/diagnosticSettings/write | Kaynak için tanılama ayarını oluşturur veya güncelleştirir |
> | Eylem | microsoft.web/sites/slots/providers/Microsoft.Insights/logDefinitions/read | Web uygulaması yuvaları için kullanılabilir günlüklerini alır |
> | Eylem | Microsoft.Web/sites/slots/providers/Microsoft.Insights/metricDefinitions/Read | Web uygulaması yuvası için kullanılabilir ölçümleri alır |
> | Eylem | Microsoft.Web/Sites/slots/publiccertificates/DELETE | Web uygulamaları yuvaları ortak sertifikaları silin. |
> | Eylem | Microsoft.Web/Sites/slots/publiccertificates/Read | Web uygulamaları yuvaları ortak sertifikaları alın. |
> | Eylem | Microsoft.Web/Sites/slots/publiccertificates/Write | Sertifikaları oluşturun veya Web Apps yuvaları ortak güncelleştirin. |
> | Eylem | Microsoft.Web/sites/slots/publish/Action | Bir Web uygulaması yuvası yayımlama |
> | Eylem | Microsoft.Web/sites/slots/publishxml/Action | Web uygulaması yuvası için profil xml yayımlama Al |
> | Eylem | Microsoft.Web/sites/slots/Read | Bir Web uygulaması dağıtım yuvası özelliklerini alır |
> | Eylem | Microsoft.Web/Sites/slots/RECOVER/Action | Web uygulamaları yuvaları kurtarın. |
> | Eylem | Microsoft.Web/sites/slots/resetSlotConfig/Action | Web uygulaması yuvası yapılandırması Sıfırla |
> | Eylem | Microsoft.Web/Sites/slots/resourcehealthmetadata/Read | Web uygulamaları yuvaları kaynak sistem durumu meta verileri alın. |
> | Eylem | Microsoft.Web/sites/slots/restart/Action | Bir Web uygulaması yuvası yeniden başlatın |
> | Eylem | Microsoft.Web/Sites/slots/Restore/Read | Web uygulamaları yuvaları geri alın. |
> | Eylem | Microsoft.Web/Sites/slots/Restore/Write | Web uygulamaları yuvaları geri yükleyin. |
> | Eylem | Microsoft.Web/Sites/slots/restorefromdeletedwebapp/Action | Web uygulaması yuvaları silinen uygulamadan geri yükleyin. |
> | Eylem | Microsoft.Web/Sites/slots/restoresnapshot/Action | Web uygulamaları yuvaları anlık görüntüleri geri yükleyin. |
> | Eylem | Microsoft.Web/Sites/slots/siteextensions/DELETE | Web uygulamaları yuvaları Site uzantıları silin. |
> | Eylem | Microsoft.Web/Sites/slots/siteextensions/Read | Web uygulamaları yuvaları Site uzantılarını Al. |
> | Eylem | Microsoft.Web/Sites/slots/siteextensions/Write | Web uygulamaları yuvaları Site uzantıları güncelleştirin. |
> | Eylem | Microsoft.Web/sites/slots/slotsdiffs/Action | Web uygulaması yuvaları arasındaki farklar yapılandırmasında Al |
> | Eylem | Microsoft.Web/sites/slots/slotsswap/Action | Web uygulaması dağıtım yuvalarını değiştirme |
> | Eylem | Microsoft.Web/Sites/slots/snapshots/Read | Web uygulamaları yuvaları anlık görüntülerini alın. |
> | Eylem | Microsoft.Web/sites/slots/sourcecontrols/Delete | Web uygulaması yuvası'nın kaynak denetimini yapılandırma ayarlarını Sil |
> | Eylem | Microsoft.Web/sites/slots/sourcecontrols/Read | Web uygulaması yuvası'nın kaynak denetimi yapılandırma ayarlarını al |
> | Eylem | Microsoft.Web/sites/slots/sourcecontrols/Write | Web uygulaması yuvası'nın kaynak denetimini yapılandırma ayarlarını güncelleştirme |
> | Eylem | Microsoft.Web/sites/slots/start/Action | Bir Web uygulaması yuvası Başlat |
> | Eylem | Microsoft.Web/sites/slots/stop/Action | Bir Web uygulaması yuvası Durdur |
> | Eylem | Microsoft.Web/Sites/slots/Sync/Action | Eşitleme Web Apps yuvaları. |
> | Eylem | Microsoft.Web/Sites/slots/triggeredwebjobs/DELETE | Web uygulamaları yuvası harekete WebJobs silin. |
> | Eylem | Microsoft.Web/Sites/slots/triggeredwebjobs/Read | Web uygulamaları yuvası harekete Web işlerini alma. |
> | Eylem | Microsoft.Web/Sites/slots/triggeredwebjobs/Run/Action | Web uygulamaları yuvası harekete Web işleri'ni çalıştırın. |
> | Eylem | Microsoft.Web/Sites/slots/usages/Read | Web uygulamaları yuvaları kullanımları alın. |
> | Eylem | Microsoft.Web/Sites/slots/virtualnetworkconnections/DELETE | Web uygulamaları yuvaları sanal ağ bağlantıları silin. |
> | Eylem | Microsoft.Web/Sites/slots/virtualnetworkconnections/Gateways/Write | Web uygulamaları yuvaları sanal ağ bağlantıları Gateway bileşenlerini güncelleştirin. |
> | Eylem | Microsoft.Web/Sites/slots/virtualnetworkconnections/Read | Web uygulamaları yuvaları sanal ağ bağlantıları alın. |
> | Eylem | Microsoft.Web/Sites/slots/virtualnetworkconnections/Write | Web uygulamaları yuvaları sanal ağ bağlantıları güncelleştirin. |
> | Eylem | Microsoft.Web/Sites/slots/webjobs/Read | Web uygulamaları yuvaları WebJobs alın. |
> | Eylem | Microsoft.Web/sites/slots/Write | Yeni bir Web uygulaması yuvası oluşturma veya var olan bir güncelleştirme |
> | Eylem | Microsoft.Web/sites/slotsdiffs/Action | Web uygulaması yuvaları arasındaki farklar yapılandırmasında Al |
> | Eylem | Microsoft.Web/sites/slotsswap/Action | Web uygulaması dağıtım yuvalarını değiştirme |
> | Eylem | Microsoft.Web/Sites/snapshots/Read | Web uygulamaları anlık görüntülerini alın. |
> | Eylem | Microsoft.Web/sites/sourcecontrols/Delete | Web uygulamanızın kaynak denetimini yapılandırma ayarlarını Sil |
> | Eylem | Microsoft.Web/sites/sourcecontrols/Read | Web uygulamanızın kaynak denetimini yapılandırma ayarlarını al |
> | Eylem | Microsoft.Web/sites/sourcecontrols/Write | Web uygulamanızın kaynak denetimini yapılandırma ayarlarını güncelleştirme |
> | Eylem | Microsoft.Web/sites/start/Action | Bir Web uygulaması başlatın |
> | Eylem | Microsoft.Web/sites/stop/Action | Bir Web uygulamasını Durdur |
> | Eylem | Microsoft.Web/Sites/Sync/Action | Eşitleme Web uygulamaları. |
> | Eylem | Microsoft.Web/Sites/syncfunctiontriggers/Action | Web uygulamaları için eşitleme işlevi tetikler. |
> | Eylem | Microsoft.Web/Sites/triggeredwebjobs/DELETE | Web uygulamaları tetiklenen Web işleri silin. |
> | Eylem | Microsoft.Web/Sites/triggeredwebjobs/History/Read | Web uygulamaları tetiklenen WebJobs geçmişi alın. |
> | Eylem | Microsoft.Web/Sites/triggeredwebjobs/Read | Web uygulamaları tetiklenen Web işlerini alma. |
> | Eylem | Microsoft.Web/Sites/triggeredwebjobs/Run/Action | Web uygulamaları tetiklenen WebJobs çalıştırın. |
> | Eylem | Microsoft.Web/Sites/usages/Read | Web uygulamaları kullanımları alın. |
> | Eylem | Microsoft.Web/Sites/virtualnetworkconnections/DELETE | Web uygulamaları sanal ağ bağlantıları silin. |
> | Eylem | Microsoft.Web/Sites/virtualnetworkconnections/Gateways/Read | Web uygulamaları sanal ağ bağlantıları ağ geçitleri alın. |
> | Eylem | Microsoft.Web/Sites/virtualnetworkconnections/Gateways/Write | Web uygulamaları sanal ağ bağlantıları Gateway bileşenlerini güncelleştirin. |
> | Eylem | Microsoft.Web/Sites/virtualnetworkconnections/Read | Web uygulamaları sanal ağ bağlantıları alın. |
> | Eylem | Microsoft.Web/Sites/virtualnetworkconnections/Write | Web uygulamaları sanal ağ bağlantıları güncelleştirin. |
> | Eylem | Microsoft.Web/Sites/webjobs/Read | Web uygulamalarını Web işlerini alma. |
> | Eylem | Microsoft.Web/sites/Write | Yeni bir Web uygulaması oluşturun veya var olan bir güncelleştirme |
> | Eylem | Microsoft.Web/skus/Read | SKU'ları alır. |
> | Eylem | Microsoft.Web/sourcecontrols/Read | Kaynak denetimleri alın. |
> | Eylem | Microsoft.Web/sourcecontrols/Write | Kaynak denetimleri güncelleştirin. |
> | Eylem | Microsoft.Web/unregister/Action | Aboneliği için Microsoft.Web kaynak sağlayıcısı kaydını silin. |
> | Eylem | Microsoft.Web/Validate/Action | Doğrulayın. |
> | Eylem | Microsoft.Web/verifyhostingenvironmentvnet/Action | Barındırma ortamı Vnet doğrulayın. |

## <a name="microsoftworkloadmonitor"></a>Microsoft.WorkloadMonitor

> [!div class="mx-tdCol2BreakAll"]
> | Eylem Türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.WorkloadMonitor/components/read | Okuma işlemleri kaynakları |
> | Eylem | Microsoft.WorkloadMonitor/healthInstances/read | Okuma işlemleri kaynakları |
> | Eylem | Microsoft.WorkloadMonitor/Operations/read | Okuma işlemleri kaynakları |
> | Eylem | Microsoft.WorkloadMonitor/workloads/delete | Bir iş yükü kaynak siler |
> | Eylem | Microsoft.WorkloadMonitor/workloads/read | Bir iş yükü kaynak okur |
> | Eylem | Microsoft.WorkloadMonitor/workloads/write | Bir iş yükü kaynak Yazar |

## <a name="next-steps"></a>Sonraki adımlar

- [Özel roller](custom-roles.md)
- [Yerleşik roller](built-in-roles.md)
