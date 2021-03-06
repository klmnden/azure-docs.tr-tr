---
title: Azure Resource Manager kaynak sağlayıcısı işlemleri | Microsoft Docs
description: Microsoft Azure Resource Manager kaynak sağlayıcılarına ilişkin işlemleri listele
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/24/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 424995369ca0d993d78a5d5e4dd1e2b52a0d64d9
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67428328"
---
# <a name="azure-resource-manager-resource-provider-operations"></a>Azure Resource Manager kaynak sağlayıcısı işlemleri

Bu makalede, her bir Azure Resource Manager kaynak sağlayıcısı için kullanılabilir işlemleri listele. Bu işlem kullanılabilir [özel roller](custom-roles.md) ayrıntılı [rol tabanlı erişim denetimi (RBAC)](overview.md) azure'daki kaynaklara. İşlem dizeleri aşağıdaki biçime sahiptir: `{Company}.{ProviderName}/{resourceType}/{action}`

Kaynak sağlayıcısı işlemleri her zaman artmaktadır. En son işlem almak için kullanın [Get-AzProviderOperation](/powershell/module/az.resources/get-azprovideroperation) veya [az sağlayıcı işlemi listesi](/cli/azure/provider/operation#az-provider-operation-list).

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="microsoftaad"></a>Microsoft.AAD

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.AAD/domainServices/delete | Etki alanı hizmeti Sil |
> | Eylem | Microsoft.AAD/domainServices/oucontainer/delete | Yapısal birim kapsayıcısına Sil |
> | Eylem | Microsoft.AAD/domainServices/oucontainer/read | OU kapsayıcıları okuyun |
> | Eylem | Microsoft.AAD/domainServices/oucontainer/write | Yapısal birim kapsayıcısına yazma |
> | Eylem | Microsoft.AAD/domainServices/read | Etki Alanı Hizmetleri okuyun |
> | Eylem | Microsoft.AAD/domainServices/replicaSets/delete | Küme Site Sil |
> | Eylem | Microsoft.AAD/domainServices/replicaSets/read | Küme Site okuma |
> | Eylem | Microsoft.AAD/domainServices/replicaSets/write | Küme Site yazma |
> | Eylem | Microsoft.AAD/domainServices/write | Etki alanı hizmeti yazma |
> | Eylem | Microsoft.AAD/locations/operationresults/read |  |
> | Eylem | Microsoft.AAD/Operations/read |  |
> | Eylem | Microsoft.AAD/register/action | Etki alanı hizmeti kaydedin |
> | Eylem | Microsoft.AAD/unregister/action | Etki alanı hizmeti kaydını sil |

## <a name="microsoftaadiam"></a>microsoft.aadiam

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.aadiam/diagnosticsettings/DELETE | Tanılama ayarını siliniyor |
> | Eylem | Microsoft.aadiam/diagnosticsettings/Read | Tanılama ayarını okuma |
> | Eylem | Microsoft.aadiam/diagnosticsettings/Write | Tanılama ayarını yazma |
> | Eylem | Microsoft.aadiam/diagnosticsettingscategories/Read | Tanılama ayarı kategorilerini okuma |

## <a name="microsoftaddons"></a>Microsoft.Addons

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Addons/operations/read | Alır, RP işlemler desteklenmektedir. |
> | Eylem | Microsoft.Addons/register/action | Belirtilen abonelik Microsoft.Addons ile kaydetme |
> | Eylem | Microsoft.Addons/supportProviders/listsupportplaninfo/action | Belirtilen abonelik için geçerli destek planı bilgilerini listeler. |
> | Eylem | Microsoft.Addons/supportProviders/supportPlanTypes/delete | Belirtilen Canonical destek planı kaldırır |
> | Eylem | Microsoft.Addons/supportProviders/supportPlanTypes/read | Belirtilen Canonical destek planı durumunu alın. |
> | Eylem | Microsoft.Addons/supportProviders/supportPlanTypes/write | Belirtilen Canonical destek planı türünü ekler. |

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.ADHybridHealthService/addsservices/action | Kiracı için yeni bir orman oluşturun. |
> | Eylem | Microsoft.ADHybridHealthService/addsservices/addomainservicemembers/read | Tüm sunucular için belirtilen hizmet adını alır. |
> | Eylem | Microsoft.ADHybridHealthService/addsservices/alerts/read | Uyarı ayrıntıları ortaya orman alertıd uyarı gibi tarih, uyarı son algılanan, uyarı açıklaması, son güncelleştirilen, uyarı düzeyi, uyarı durumu, bağlantılar vb. sorun giderme uyarı alır. |
> | Eylem | Microsoft.ADHybridHealthService/addsservices/configuration/read | Hizmet yapılandırması ormanın alır. Örnek - orman adı, etki alanı adlandırma Yöneticisi FSMO rolündeki değişiklikler, Şema Yöneticisi FSMO rolündeki değişiklikler vb. işlev düzeyi. |
> | Eylem | Microsoft.ADHybridHealthService/addsservices/delete | Bir hizmet siler ve bu bilgisayarın sistem durumu verileriyle birlikte sunucuları. |
> | Eylem | Microsoft.ADHybridHealthService/addsservices/dimensions/read | Orman için etki alanları ve siteler ayrıntılarını alır. Örnek sistem durumu, etkin uyarıları çözümlenen uyarılar, özellikler gibi etki alanı işlev düzeyi orman, altyapı yöneticisi, PDC, RID Yöneticisi vb.  |
> | Eylem | Microsoft.ADHybridHealthService/addsservices/features/userpreference/read | Orman için kullanıcı tercihi ayarını alır.<br>Örnek - MetricCounterName ldapsuccessfulbinds ntlmauthentications, kerberosauthentications addsinsightsagentprivatebytes, ldapsearches gibi.<br>UI grafiklerini vb. için ayarlar. |
> | Eylem | Microsoft.ADHybridHealthService/addsservices/forestsummary/read | Orman orman adı, etki alanı altında bu orman, siteler ve site ayrıntıları vb. sayısı gibi belirli bir orman için Özet alır. |
> | Eylem | Microsoft.ADHybridHealthService/addsservices/metricmetadata/read | Desteklenen ölçümlerin listesi için belirli bir hizmet alır.<br>Örnek, Extranet hesap kilitlemeleri, toplam başarısız istek sayısı, bekleyen belirteci isteklerini (Proxy), belirteç İsteği/sn için ADFS hizmeti vb. için.<br>NTLM kimlik doğrulama/sn, LDAP başarılı bağlamalar/sn, LDAP bağlama süresi, etkin iş parçacığı LDAP, Kerberos kimlik doğrulamaları/sn, ATQ iş parçacıkları toplam vb. ADDomainService için.<br>Azure AD'ye ADSync hizmeti için profil gecikme süresi, TCP bağlantı kuran, İçgörüler Aracısı özel bayt sayısı, istatistikleri Dışarı Aktar'ı çalıştırın. |
> | Eylem | Microsoft.ADHybridHealthService/addsservices/metrics/groups/read | Bu API, hizmet göz önünde bulundurulduğunda, ölçüm bilgileri alır.<br>Örneğin, bu API, ilgili bilgileri almak için kullanılabilir: Extranet hesap kilitlemeleri uygulayın, toplam başarısız istekler, bekleyen belirteci isteklerini (Proxy), belirteç İsteği/sn vb. ADFederation hizmeti.<br>NTLM kimlik doğrulama/sn, LDAP başarılı bağlamalar/sn, LDAP bağlama süresi, LDAP Etkin iş parçacığı, Kerberos kimlik doğrulamaları/sn, ATQ iş parçacıklarının toplam vb. ADDomain hizmeti için.<br>Çalıştırma profili gecikmesi, TCP bağlantısı kurulduktan İçgörüler Aracısı özel bayt sayısı, dışarı aktarma istatistikleri için Azure AD eşitleme hizmeti için. |
> | Eylem | Microsoft.ADHybridHealthService/addsservices/premiumcheck/read | Bu API, bir premium Kiracı için tüm eklenen ADDomainServices listesini alır. |
> | Eylem | Microsoft.ADHybridHealthService/addsservices/read | Hizmet Ayrıntıları belirtilen hizmet adını alır. |
> | Eylem | Microsoft.ADHybridHealthService/addsservices/replicationdetails/read | Belirtilen hizmet adı için tüm sunucuların çoğaltma ayrıntılarını alır. |
> | Eylem | Microsoft.ADHybridHealthService/addsservices/replicationstatus/read | Etki alanı denetleyicileri ve kendi çoğaltma hataları varsa sayısını alır. |
> | Eylem | Microsoft.ADHybridHealthService/addsservices/replicationsummary/read | Etki alanı denetleyicisi listesinde belirtilen ormana ilişkin çoğaltma ayrıntılarını alır tamamlayın. |
> | Eylem | Microsoft.ADHybridHealthService/addsservices/servicemembers/action | Bir sunucu örneği için hizmet ekleyin. |
> | Eylem | Microsoft.ADHybridHealthService/addsservices/servicemembers/credentials/read | ADDomainService sunucu kaydı sırasında bu API, yeni sunucular ekleme için kimlik bilgilerini almak için çağrılır. |
> | Eylem | Microsoft.ADHybridHealthService/addsservices/servicemembers/delete | Verili hizmet ve Kiracı için bir sunucu siler. |
> | Eylem | Microsoft.ADHybridHealthService/addsservices/write | Oluşturur veya Kiracı için ADDomainService örneğini güncelleştirir. |
> | Eylem | Microsoft.ADHybridHealthService/configuration/action | Güncelleştirmeleri Kiracı yapılandırma. |
> | Eylem | Microsoft.ADHybridHealthService/configuration/read | Kiracı yapılandırmasına okur. |
> | Eylem | Microsoft.ADHybridHealthService/configuration/write | Bir kiracı yapılandırması oluşturur. |
> | Eylem | Microsoft.ADHybridHealthService/logs/contents/read | Aracı yükleme ve kayıt günlükleri blobu'nda depolanan içeriğini alır. |
> | Eylem | Microsoft.ADHybridHealthService/logs/read | Kiracı için aracıyı yükleme ve kayıt günlükleri alır. |
> | Eylem | Microsoft.ADHybridHealthService/operations/read | Sistem tarafından desteklenen işlemler listesini alır. |
> | Eylem | Microsoft.ADHybridHealthService/register/action | ADHybrid sistem durumu hizmeti kaynak sağlayıcısını kaydeder ve ADHybrid sistem durumu Hizmet kaynağı oluşturulmasını sağlar. |
> | Eylem | Microsoft.ADHybridHealthService/reports/availabledeployments/read | Müşteri olayları desteklemek için DevOps tarafından kullanılan kullanılabilir bölgelerin bir listesini alır. |
> | Eylem | Microsoft.ADHybridHealthService/reports/badpassword/read | Active Directory Federasyon Hizmeti içindeki tüm kullanıcılar için hatalı parola denemesi listesini alır. |
> | Eylem | Microsoft.ADHybridHealthService/reports/badpassworduseridipfrequency/read | BLOB SAS URİ'si durumu içeren alır ve belirli bir kiracının günde IPADDRESS başına UserID başına yeni sıraya alınan rapor işi sıklığı, hatalı kullanıcı adı/parola için nihai sonucu çalışır. |
> | Eylem | Microsoft.ADHybridHealthService/reports/consentedtodevopstenants/read | DevOps listesini alır kiracılar tarafından onaylanan. Genellikle, müşteri desteği için kullanılır. |
> | Eylem | Microsoft.ADHybridHealthService/reports/isdevops/read | Kiracı DevOps onaylı olup olmadığını belirten bir değer alır. |
> | Eylem | Microsoft.ADHybridHealthService/reports/selectdevopstenant/read | Seçili dev ops Kiracı için userid(objectid) güncelleştirir. |
> | Eylem | Microsoft.ADHybridHealthService/reports/selecteddeployment/read | Seçili dağıtım belirli kiracısı için alır. |
> | Eylem | Microsoft.ADHybridHealthService/reports/tenantassigneddeployment/read | Kiracı kimliği alır verilen Kiracı depolama konumu. |
> | Eylem | Microsoft.ADHybridHealthService/reports/updateselecteddeployment/read | Veri erişilecek coğrafi konumunu alır. |
> | Eylem | Microsoft.ADHybridHealthService/services/action | Bir hizmet örneği kiracıdaki güncelleştirir. |
> | Eylem | Microsoft.ADHybridHealthService/services/alerts/read | Hizmet uyarılarını okur. |
> | Eylem | Microsoft.ADHybridHealthService/services/alerts/read | Hizmet uyarılarını okur. |
> | Eylem | Microsoft.ADHybridHealthService/services/checkservicefeatureavailibility/read | Bir özellik adı, hizmet bu özelliği kullanmak için gereken her şeyi olup olmadığını doğrular. |
> | Eylem | Microsoft.ADHybridHealthService/services/delete | Bir hizmet örneği kiracıdaki siler. |
> | Eylem | Microsoft.ADHybridHealthService/services/exporterrors/read | Verilen eşitleme hizmeti için dışarı aktarma hataları alır. |
> | Eylem | Microsoft.ADHybridHealthService/services/exportstatus/read | Belirli bir hizmet için dışarı aktarma durumunu alır. |
> | Eylem | Microsoft.ADHybridHealthService/services/feedbacktype/feedback/read | Verili hizmet ve sunucu için uyarılar geri bildirim alır. |
> | Eylem | Microsoft.ADHybridHealthService/services/metricmetadata/read | Desteklenen ölçümlerin listesi için belirli bir hizmet alır.<br>Örnek, Extranet hesap kilitlemeleri, toplam başarısız istek sayısı, bekleyen belirteci isteklerini (Proxy), belirteç İsteği/sn için ADFS hizmeti vb. için.<br>NTLM kimlik doğrulama/sn, LDAP başarılı bağlamalar/sn, LDAP bağlama süresi, etkin iş parçacığı LDAP, Kerberos kimlik doğrulamaları/sn, ATQ iş parçacıkları toplam vb. ADDomainService için.<br>Azure AD'ye ADSync hizmeti için profil gecikme süresi, TCP bağlantı kuran, İçgörüler Aracısı özel bayt sayısı, istatistikleri Dışarı Aktar'ı çalıştırın. |
> | Eylem | Microsoft.ADHybridHealthService/services/metrics/groups/average/read | Bu API, hizmet göz önünde bulundurulduğunda, belirli bir hizmetin ölçümler için ortalama alır.<br>Örneğin, bu API, ilgili bilgileri almak için kullanılabilir: Extranet hesap kilitlemeleri uygulayın, toplam başarısız istekler, bekleyen belirteci isteklerini (Proxy), belirteç İsteği/sn vb. ADFederation hizmeti.<br>NTLM kimlik doğrulama/sn, LDAP başarılı bağlamalar/sn, LDAP bağlama süresi, LDAP Etkin iş parçacığı, Kerberos kimlik doğrulamaları/sn, ATQ iş parçacıklarının toplam vb. ADDomain hizmeti için.<br>Çalıştırma profili gecikmesi, TCP bağlantısı kurulduktan İçgörüler Aracısı özel bayt sayısı, dışarı aktarma istatistikleri için Azure AD eşitleme hizmeti için. |
> | Eylem | Microsoft.ADHybridHealthService/services/metrics/groups/read | Bu API, hizmet göz önünde bulundurulduğunda, ölçüm bilgileri alır.<br>Örneğin, bu API, ilgili bilgileri almak için kullanılabilir: Extranet hesap kilitlemeleri uygulayın, toplam başarısız istekler, bekleyen belirteci isteklerini (Proxy), belirteç İsteği/sn vb. ADFederation hizmeti.<br>NTLM kimlik doğrulama/sn, LDAP başarılı bağlamalar/sn, LDAP bağlama süresi, LDAP Etkin iş parçacığı, Kerberos kimlik doğrulamaları/sn, ATQ iş parçacıklarının toplam vb. ADDomain hizmeti için.<br>Çalıştırma profili gecikmesi, TCP bağlantısı kurulduktan İçgörüler Aracısı özel bayt sayısı, dışarı aktarma istatistikleri için Azure AD eşitleme hizmeti için. |
> | Eylem | Microsoft.ADHybridHealthService/services/metrics/groups/sum/read | Bu API, hizmet göz önünde bulundurulduğunda, belirli bir hizmetin ölçümler için toplanan görünümünü alır.<br>Örneğin, bu API, ilgili bilgileri almak için kullanılabilir: Extranet hesap kilitlemeleri uygulayın, toplam başarısız istekler, bekleyen belirteci isteklerini (Proxy), belirteç İsteği/sn vb. ADFederation hizmeti.<br>NTLM kimlik doğrulama/sn, LDAP başarılı bağlamalar/sn, LDAP bağlama süresi, LDAP Etkin iş parçacığı, Kerberos kimlik doğrulamaları/sn, ATQ iş parçacıklarının toplam vb. ADDomain hizmeti için.<br>Çalıştırma profili gecikmesi, TCP bağlantısı kurulduktan İçgörüler Aracısı özel bayt sayısı, dışarı aktarma istatistikleri için Azure AD eşitleme hizmeti için. |
> | Eylem | Microsoft.ADHybridHealthService/services/monitoringconfiguration/write | Ekleme veya bir hizmet için izleme yapılandırmasını güncelleştirir. |
> | Eylem | Microsoft.ADHybridHealthService/services/monitoringconfigurations/read | Belirli bir hizmetin izleme yapılandırmalarının alır. |
> | Eylem | Microsoft.ADHybridHealthService/services/monitoringconfigurations/write | Ekleme veya bir hizmet için izleme yapılandırmalarının güncelleştirir. |
> | Eylem | Microsoft.ADHybridHealthService/services/premiumcheck/read | Bu API, bir premium kiracının tüm eklenen hizmet listesini alır. |
> | Eylem | Microsoft.ADHybridHealthService/services/read | Kiracıda hizmet örnekleri okur. |
> | Eylem | Microsoft.ADHybridHealthService/services/reports/blobUris/read | Son 7 güne ait tüm riskli IP raporu URI alır. |
> | Eylem | Microsoft.ADHybridHealthService/services/reports/details/read | Hatalı parola hataları son 7 güne ait olan ilk 50 kullanıcı raporu alır |
> | Eylem | Microsoft.ADHybridHealthService/services/reports/generateBlobUri/action | Riskli IP raporu oluşturur ve ona işaret eden bir URI döndürür. |
> | Eylem | Microsoft.ADHybridHealthService/services/servicemembers/action | Bir sunucu örneği hizmeti oluşturur. |
> | Eylem | Microsoft.ADHybridHealthService/services/servicemembers/alerts/read | Uyarılar için bir sunucu okur. |
> | Eylem | Microsoft.ADHybridHealthService/services/servicemembers/credentials/read | Sunucu kaydı sırasında bu API, yeni sunucular ekleme için kimlik bilgilerini almak için çağrılır. |
> | Eylem | Microsoft.ADHybridHealthService/services/servicemembers/datafreshness/read | Belirli bir sunucu için bu API sunucuları ve her karşıya yükleme için en son zaman tarafından karşıya veri türleri listesini alır. |
> | Eylem | Microsoft.ADHybridHealthService/services/servicemembers/delete | Bir sunucu örneği hizmetinde siler. |
> | Eylem | Microsoft.ADHybridHealthService/services/servicemembers/exportstatus/read | Belirli bir eşitleme hizmeti için eşitleme dışarı aktarma hata ayrıntılarını alır. |
> | Eylem | Microsoft.ADHybridHealthService/services/servicemembers/metrics/groups/read | Bu API, hizmet göz önünde bulundurulduğunda, ölçüm bilgileri alır.<br>Örneğin, bu API, ilgili bilgileri almak için kullanılabilir: Extranet hesap kilitlemeleri uygulayın, toplam başarısız istekler, bekleyen belirteci isteklerini (Proxy), belirteç İsteği/sn vb. ADFederation hizmeti.<br>NTLM kimlik doğrulama/sn, LDAP başarılı bağlamalar/sn, LDAP bağlama süresi, LDAP Etkin iş parçacığı, Kerberos kimlik doğrulamaları/sn, ATQ iş parçacıklarının toplam vb. ADDomain hizmeti için.<br>Çalıştırma profili gecikmesi, TCP bağlantısı kurulduktan İçgörüler Aracısı özel bayt sayısı, dışarı aktarma istatistikleri için Azure AD eşitleme hizmeti için. |
> | Eylem | Microsoft.ADHybridHealthService/services/servicemembers/metrics/read | Bağlayıcılar ve verili hizmet ve hizmet üyesi için çalıştırma profili adları listesini alır. |
> | Eylem | Microsoft.ADHybridHealthService/services/servicemembers/read | Hizmet sunucu örneğinde okur. |
> | Eylem | Microsoft.ADHybridHealthService/services/servicemembers/serviceconfiguration/read | Belirli bir kiracının hizmet yapılandırmasını alır. |
> | Eylem | Microsoft.ADHybridHealthService/services/tenantwhitelisting/read | Belirli bir kiracısı için özellik beyaz listeye ekleme durumu alır. |
> | Eylem | Microsoft.ADHybridHealthService/services/write | Kiracıda bir hizmet örneği oluşturur. |
> | Eylem | Microsoft.ADHybridHealthService/unregister/action | ADHybrid sistem durumu hizmeti kaynak sağlayıcısı için aboneliği kaydını siler. |

## <a name="microsoftadvisor"></a>Microsoft.Advisor

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Advisor/configurations/read | Yapılandırmalarını alma |
> | Eylem | Microsoft.Advisor/configurations/write | / Güncelleştirme yapılandırma |
> | Eylem | Microsoft.Advisor/generateRecommendations/action | Öneriler oluşturur |
> | Eylem | Microsoft.Advisor/generateRecommendations/read | Öneriler durumunu alır oluştur |
> | Eylem | Microsoft.Advisor/metadata/read | Meta verilerini al |
> | Eylem | Microsoft.Advisor/operations/read | Microsoft Advisor işlemlerinde alır |
> | Eylem | Microsoft.Advisor/recommendations/available/action | Yeni öneri Microsoft Danışmanı'nda kullanılabilir |
> | Eylem | Microsoft.Advisor/recommendations/read | Öneriler okur |
> | Eylem | Microsoft.Advisor/recommendations/suppressions/delete | Gizleme siler |
> | Eylem | Microsoft.Advisor/recommendations/suppressions/read | Gizlemeleri alır |
> | Eylem | Microsoft.Advisor/recommendations/suppressions/write | Gizlemeleri oluşturur/güncelleştirmeleri |
> | Eylem | Microsoft.Advisor/register/action | Microsoft Advisor için aboneliği kaydeder |
> | Eylem | Microsoft.Advisor/suppressions/delete | Gizleme siler |
> | Eylem | Microsoft.Advisor/suppressions/read | Gizlemeleri alır |
> | Eylem | Microsoft.Advisor/suppressions/write | Gizlemeleri oluşturur/güncelleştirmeleri |
> | Eylem | Microsoft.Advisor/unregister/action | Abonelik için Microsoft Advisor kaydını siler |

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.AlertsManagement/actionRules/delete | Belirli bir abonelikte eylem kuralını silin. |
> | Eylem | Microsoft.AlertsManagement/actionRules/read | Tüm eylem kuralları, giriş filtrelerini alın. |
> | Eylem | Microsoft.AlertsManagement/actionRules/write | Belirli bir abonelikte Eylem kuralı oluşturun veya güncelleştirin |
> | Eylem | Microsoft.AlertsManagement/alerts/changestate/action | Uyarının durumunu değiştirin. |
> | Eylem | Microsoft.AlertsManagement/alerts/diagnostics/read | Tüm tanılama için uyarı alın. |
> | Eylem | Microsoft.AlertsManagement/alerts/history/read | Uyarı geçmişi Al |
> | Eylem | Microsoft.AlertsManagement/alerts/read | Tüm uyarıları giriş filtrelerini alın. |
> | Eylem | Microsoft.AlertsManagement/alertsList/read | Tüm uyarıları giriş filtrelerini abonelikler arasında alın. |
> | Eylem | Microsoft.AlertsManagement/alertsSummary/read | Uyarıların özetini alın |
> | Eylem | Microsoft.AlertsManagement/alertsSummaryList/read | Abonelikler arasında uyarıların özetini alın |
> | Eylem | Microsoft.AlertsManagement/Operations/read | Sağlanan işlemleri okur |
> | Eylem | Microsoft.AlertsManagement/register/action | Microsoft uyarılar yönetimi için aboneliği kaydeder |
> | Eylem | Microsoft.AlertsManagement/smartDetectorAlertRules/delete | Belirli bir abonelikte akıllı algılayıcısı uyarı kuralını Sil |
> | Eylem | Microsoft.AlertsManagement/smartDetectorAlertRules/read | Tüm akıllı algılayıcısı uyarı kuralları için giriş filtreleri alma |
> | Eylem | Microsoft.AlertsManagement/smartDetectorAlertRules/write | Belirli bir abonelikte akıllı algılayıcısı uyarı kuralı oluşturun veya güncelleştirin |
> | Eylem | Microsoft.AlertsManagement/smartGroups/changestate/action | Akıllı grubunun durumunu değiştirin |
> | Eylem | Microsoft.AlertsManagement/smartGroups/history/read | Akıllı Grup geçmişi Al |
> | Eylem | Microsoft.AlertsManagement/smartGroups/read | Giriş filtrelerini tüm akıllı grupları alma |

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.AnalysisServices/locations/checkNameAvailability/action | Sağlanan Analiz sunucusu adının geçerli olup olmadığını kontrol eder ve kullanımda. |
> | Eylem | Microsoft.AnalysisServices/locations/operationresults/read | Belirtilen işlem sonucu bilgilerini alır. |
> | Eylem | Microsoft.AnalysisServices/locations/operationstatuses/read | Belirtilen işlem durumu bilgilerini alır. |
> | Eylem | Microsoft.AnalysisServices/operations/read | İşlem bilgilerini alır. |
> | Eylem | Microsoft.AnalysisServices/register/action | Analysis Services kaynak sağlayıcısını kaydeder. |
> | Eylem | Microsoft.AnalysisServices/servers/delete | Analiz sunucusunu siler. |
> | Eylem | Microsoft.AnalysisServices/servers/listGatewayStatus/action | Sunucu ile ilişkilendirilen ağ geçidinin durumunu listeleyin. |
> | Eylem | Microsoft.AnalysisServices/servers/read | Belirtilen analiz sunucusunun bilgilerini alır. |
> | Eylem | Microsoft.AnalysisServices/servers/resume/action | Analiz sunucusunu sürdürür. |
> | Eylem | Microsoft.AnalysisServices/servers/skus/read | Sunucu için kullanılabilir SKU bilgilerini alın |
> | Eylem | Microsoft.AnalysisServices/servers/suspend/action | Analiz sunucusunu askıya alır. |
> | Eylem | Microsoft.AnalysisServices/servers/write | Oluşturur veya belirtilen analiz sunucusunu güncelleştirir. |
> | Eylem | Microsoft.AnalysisServices/skus/read | SKU bilgilerini alır. |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.ApiManagement/checkNameAvailability/read | Hizmet adı sağlanmışsa denetler |
> | Eylem | Microsoft.ApiManagement/operations/read | Tüm kullanılabilir API işlemlerini Microsoft.ApiManagement kaynak için okuma |
> | Eylem | Microsoft.ApiManagement/register/action | Microsoft.ApiManagement kaynak sağlayıcısı için aboneliği kaydedin |
> | Eylem | Microsoft.ApiManagement/reports/read | Süreler, coğrafi, geliştiriciler, ürünler, API'ler, işlemler, abonelik ve byRequest tarafından toplanan raporları alın. |
> | Eylem | Microsoft.ApiManagement/service/apis/delete | API Management hizmet örneği, belirtilen API siler. |
> | Eylem | Microsoft.ApiManagement/service/apis/diagnostics/delete | Belirtilen tanılama API'den siler. |
> | Eylem | Microsoft.ApiManagement/service/apis/diagnostics/read | Bir API'nin tüm tanılama listeler. veya kendi tanımlayıcısı tarafından belirtilen bir API için tanılama ayrıntılarını alır. |
> | Eylem | Microsoft.ApiManagement/service/apis/diagnostics/write | Bir API için yeni bir tanılama oluşturur veya mevcut olanı güncelleştirir. veya bir API için tanılama ayrıntılarını kendi tanımlayıcısı tarafından belirtilen güncelleştirmeleri. |
> | Eylem | Microsoft.ApiManagement/service/apis/issues/attachments/delete | Belirtilen açıklama bir sorundan siler. |
> | Eylem | Microsoft.ApiManagement/service/apis/issues/attachments/read | Listeleri sorunu için tüm ekleri belirtilen API ile ilişkili. veya kendi tanımlayıcısı tarafından belirtilen bir API için ek sorunun ayrıntılarını alır. |
> | Eylem | Microsoft.ApiManagement/service/apis/issues/attachments/write | Bir API sorunu için yeni bir ek oluşturur veya mevcut olanı güncelleştirir. |
> | Eylem | Microsoft.ApiManagement/service/apis/issues/comments/delete | Belirtilen açıklama bir sorundan siler. |
> | Eylem | Microsoft.ApiManagement/service/apis/issues/comments/read | Listeleri sorunu için tüm yorumları belirtilen API ile ilişkili. veya kendi tanımlayıcısı tarafından belirtilen bir API için sorunun ayrıntılarını açıklama alır. |
> | Eylem | Microsoft.ApiManagement/service/apis/issues/comments/write | Bir API sorunu için yeni bir yorum oluşturur veya mevcut olanı güncelleştirir. |
> | Eylem | Microsoft.ApiManagement/service/apis/issues/delete | Belirtilen sorun API'den siler. |
> | Eylem | Microsoft.ApiManagement/service/apis/issues/read | Belirtilen API ile ilişkili tüm sorunları listeler. veya kendi tanımlayıcısı tarafından belirtilen bir API için sorunun ayrıntılarını alır. |
> | Eylem | Microsoft.ApiManagement/service/apis/issues/write | Yeni bir sorun için bir API oluşturur veya mevcut olanı güncelleştirir. veya mevcut bir sorun için bir API güncelleştirmeler. |
> | Eylem | Microsoft.ApiManagement/service/apis/operations/delete | Belirtilen işlem API'sindeki siler. |
> | Eylem | Microsoft.ApiManagement/service/apis/operations/policies/delete | API işlemi ilke yapılandırmasını siler. |
> | Eylem | Microsoft.ApiManagement/service/apis/operations/policies/read | API işlem düzeyinde ilke yapılandırması listesini alın. ya da ilke yapılandırması API işlem düzeyinde alın. |
> | Eylem | Microsoft.ApiManagement/service/apis/operations/policies/write | Oluşturur veya güncelleştirir API işlem düzeyi ilke yapılandırması. |
> | Eylem | Microsoft.ApiManagement/service/apis/operations/policy/delete | İşlem düzeyinde ilke yapılandırmasını Sil |
> | Eylem | Microsoft.ApiManagement/service/apis/operations/policy/read | İşlem düzeyinde ilke yapılandırmasını al |
> | Eylem | Microsoft.ApiManagement/service/apis/operations/policy/write | İlke yapılandırması oluşturma işlemi sırasında düzeyi |
> | Eylem | Microsoft.ApiManagement/service/apis/operations/read | Belirtilen API işlemlerinde koleksiyonu listeler. veya kendi tanımlayıcısı tarafından belirtilen API işleminin ayrıntılarını alır. |
> | Eylem | Microsoft.ApiManagement/service/apis/operations/tags/delete | İşlem etiketi çıkarın. |
> | Eylem | Microsoft.ApiManagement/service/apis/operations/tags/read | İşlemle ilişkilendirilen tüm etiketleri listeler. ya da işlemle ilişkili etiket alın. |
> | Eylem | Microsoft.ApiManagement/service/apis/operations/tags/write | İşlem için etiket atayın. |
> | Eylem | Microsoft.ApiManagement/service/apis/operations/write | API'de yeni bir işlem oluşturur veya mevcut olanı güncelleştirir. veya API işleminin ayrıntılarını kendi tanımlayıcısı tarafından belirtilen güncelleştirmeleri. |
> | Eylem | Microsoft.ApiManagement/service/apis/operationsByTags/read | Etiketler ile ilişkilendirilmiş işlemler koleksiyonu listeler. |
> | Eylem | Microsoft.ApiManagement/service/apis/policies/delete | API ilkesi yapılandırmasını siler. |
> | Eylem | Microsoft.ApiManagement/service/apis/policies/read | İlke yapılandırması bir API düzeyinde alın. ya da ilke yapılandırması bir API düzeyinde alın. |
> | Eylem | Microsoft.ApiManagement/service/apis/policies/write | Oluşturur veya API ilkesi yapılandırmasını güncelleştirir. |
> | Eylem | Microsoft.ApiManagement/service/apis/policy/delete | API düzeyinde ilke yapılandırmasını Sil |
> | Eylem | Microsoft.ApiManagement/service/apis/policy/read | API düzeyinde ilke yapılandırmasını al |
> | Eylem | Microsoft.ApiManagement/service/apis/policy/write | İlke Yapılandırması API düzey oluşturma |
> | Eylem | Microsoft.ApiManagement/service/apis/products/read | API parçası olan tüm ürünleri listeler. |
> | Eylem | Microsoft.ApiManagement/service/apis/read | API Management hizmet örneği, tüm API'ları listeler. veya kendi tanımlayıcısı tarafından belirtilen API ayrıntılarını alır. |
> | Eylem | Microsoft.ApiManagement/service/apis/releases/delete | Tüm API sürümlerini kaldırır veya belirtilen API sürümünde siler. |
> | Eylem | Microsoft.ApiManagement/service/apis/releases/read | Tüm API sürümlerini listeler.<br>API düzeltmesi geçerli yaparken bir API sürümü oluşturulur.<br>Sürümleri önceki düzeltmelerini geri almak için de kullanılır.<br>Sonuçları, disk belleği ve $top ve $skip parametreleri tarafından kısıtlı olabilir.<br>veya yayın ayrıntıları bir API'nin döndürür. |
> | Eylem | Microsoft.ApiManagement/service/apis/releases/write | API için yeni bir yayın oluşturur. veya ayrıntıları API sürümünün kendi tanımlayıcısı tarafından belirtilen güncelleştirmeleri. |
> | Eylem | Microsoft.ApiManagement/service/apis/revisions/delete | Bir API'nin tüm düzeltmeleri kaldırır |
> | Eylem | Microsoft.ApiManagement/service/apis/revisions/read | Bir API'nin tüm değişiklikleri listeler. |
> | Eylem | Microsoft.ApiManagement/service/apis/schemas/delete | API şeması yapılandırmasını siler. |
> | Eylem | Microsoft.ApiManagement/service/apis/schemas/read | Şema yapılandırma API düzeyinde alın. veya şema yapılandırma API düzeyinde alın. |
> | Eylem | Microsoft.ApiManagement/service/apis/schemas/write | Oluşturur veya şema yapılandırma API'si için güncelleştirir. |
> | Eylem | Microsoft.ApiManagement/service/apis/tagDescriptions/delete | API için açıklama etiketi silin. |
> | Eylem | Microsoft.ApiManagement/service/apis/tagDescriptions/read | API kapsamdaki tüm etiketleri açıklamalarını listeler. Swagger benzer model - tagDescription API düzeyinde tanımlanır ancak etiketi işlemleri atanabilir veya Al API'si kapsamında etiket açıklaması |
> | Eylem | Microsoft.ApiManagement/service/apis/tagDescriptions/write | Kapsam etiketi açıklamasında API oluşturun/güncelleştirin. |
> | Eylem | Microsoft.ApiManagement/service/apis/tags/delete | Etiket API'sinden çıkarın. |
> | Eylem | Microsoft.ApiManagement/service/apis/tags/read | API ile ilişkili tüm etiketleri listeler. ya da API ile ilişkili etiket alın. |
> | Eylem | Microsoft.ApiManagement/service/apis/tags/write | API için etiket atayın. |
> | Eylem | Microsoft.ApiManagement/service/apis/write | Yeni oluşturur veya var olan belirtilen API API Management hizmet örneğinin güncelleştirir. veya belirtilen API güncelleştirmelerini API Management hizmet örneği. |
> | Eylem | Microsoft.ApiManagement/service/apisByTags/read | Etiketler ile ilişkili API'ler koleksiyonu listeler. |
> | Eylem | Microsoft.ApiManagement/service/apiVersionSets/delete | Özel API sürümünü ayarlama siler. |
> | Eylem | Microsoft.ApiManagement/service/apiVersionSets/read | Koleksiyonda belirtilen hizmet örneği API sürümü kümeleri listelenir. veya kendi tanımlayıcısı tarafından belirtilen API sürümü kümesinin ayrıntılarını alır. |
> | Eylem | Microsoft.ApiManagement/service/apiVersionSets/versions/read | Sürüm varlıkların listesini al |
> | Eylem | Microsoft.ApiManagement/service/apiVersionSets/write | Oluşturur veya bir API sürümünü Ayarla güncelleştirir. veya kendi tanımlayıcısı tarafından belirtilen API VersionSet ayrıntılarını güncelleştirmeleri. |
> | Eylem | Microsoft.ApiManagement/service/applynetworkconfigurationupdates/action | Güncelleştirilmiş ağ ayarlarını seçmek için sanal ağda çalışan Microsoft.ApiManagement kaynakları güncelleştirir. |
> | Eylem | Microsoft.ApiManagement/service/authorizationServers/delete | Belirli yetkilendirme sunucusu örneği siler. |
> | Eylem | Microsoft.ApiManagement/service/authorizationServers/read | Bir hizmet örneği içinde tanımlanan yetkilendirme sunucularını koleksiyonu listeler. veya kendi tanımlayıcısı tarafından belirtilen yetkilendirme sunucusunun ayrıntılarını alır. |
> | Eylem | Microsoft.ApiManagement/service/authorizationServers/write | Yeni yetkilendirme sunucusu oluşturur veya mevcut bir yetkilendirme sunucusu güncelleştirir. veya kendi tanımlayıcısı tarafından belirtilen yetkilendirme sunucusunun ayrıntılarını güncelleştirmeleri. |
> | Eylem | Microsoft.ApiManagement/service/backends/delete | Belirtilen arka uç siler. |
> | Eylem | Microsoft.ApiManagement/service/backends/read | Belirtilen hizmet örneği arka uçları koleksiyonu listeler. veya kendi tanımlayıcısı tarafından belirtilen arka uç ayrıntılarını alır. |
> | Eylem | Microsoft.ApiManagement/service/backends/reconnect/action | Belirtilen zaman aşımından sonra yeni bir arka uç bağlantı oluşturmak için APIM proxy bildirir. 2 dakikalık zaman aşımı, zaman aşımı değeri belirtilmişse kullanılır. |
> | Eylem | Microsoft.ApiManagement/service/backends/write | Oluşturur veya bir arka uç güncelleştirir. veya var olan bir arka uç güncelleştirir. |
> | Eylem | Microsoft.ApiManagement/service/backup/action | Belirtilen kapsayıcıya bir kullanıcı için yedekleme API Management hizmeti sağlanan depolama hesabı |
> | Eylem | Microsoft.ApiManagement/service/caches/delete | Belirli önbelleğini siler. |
> | Eylem | Microsoft.ApiManagement/service/caches/read | Belirtilen hizmet örneği, tüm dış önbellekleri koleksiyonu listeler. veya kendi tanımlayıcısı tarafından belirtilen önbellek ayrıntılarını alır. |
> | Eylem | Microsoft.ApiManagement/service/caches/write | Oluşturur veya bir dış API Yönetimi örneğini kullanılacak önbelleği güncelleştirir. veya kendi tanımlayıcısı tarafından belirtilen önbellek ayrıntılarını güncelleştirmeleri. |
> | Eylem | Microsoft.ApiManagement/service/certificates/delete | Belirli bir sertifika siler. |
> | Eylem | Microsoft.ApiManagement/service/certificates/read | Belirtilen hizmet örneği tüm sertifikaları koleksiyonu listeler. veya kendi tanımlayıcısı tarafından belirtilen sertifika ayrıntılarını alır. |
> | Eylem | Microsoft.ApiManagement/service/certificates/write | Oluşturur veya arka ucu ile kimlik doğrulaması için kullanılan sertifikanın güncelleştirir. |
> | Eylem | Microsoft.ApiManagement/service/contentTypes/contentItems/delete | Kaldırır, içerik öğesi belirtildi. |
> | Eylem | Microsoft.ApiManagement/service/contentTypes/contentItems/read | İçerik öğeleri veya içerik öğesi ayrıntılarını döndürür listesini döndürür |
> | Eylem | Microsoft.ApiManagement/service/contentTypes/contentItems/write | Yeni bir içerik öğesi oluşturur veya belirtilen içerik öğesini güncelleştirir |
> | Eylem | Microsoft.ApiManagement/service/contentTypes/read | İçerik türleri listesini döndürür |
> | Eylem | Microsoft.ApiManagement/service/delete | API Management hizmet örneği silme |
> | Eylem | Microsoft.ApiManagement/service/diagnostics/delete | Belirtilen tanılama siler. |
> | Eylem | Microsoft.ApiManagement/service/diagnostics/read | API Management hizmet örneği, tüm tanılama listeler. veya kendi tanımlayıcısı tarafından belirtilen tanılama ayrıntılarını alır. |
> | Eylem | Microsoft.ApiManagement/service/diagnostics/write | Yeni bir tanılama oluşturur veya mevcut olanı güncelleştirir. veya güncelleştirmeleri onun tanımlayıcısı tarafından belirtilen tanılama ayrıntıları. |
> | Eylem | Microsoft.ApiManagement/service/getssotoken/action | Kullanılabilir alır SSO belirteci API yönetim hizmeti eski portalında yönetici olarak oturum açmak için |
> | Eylem | Microsoft.ApiManagement/service/groups/delete | API Management hizmet örneği, belirli bir grubu siler. |
> | Eylem | Microsoft.ApiManagement/service/groups/read | Bir hizmet örneği içinde tanımlanan grupları koleksiyonu listeler. veya kendi tanımlayıcısı tarafından belirtilen grubunun ayrıntılarını alır. |
> | Eylem | Microsoft.ApiManagement/service/groups/users/delete | Mevcut kullanıcı mevcut grubundan kaldırın. |
> | Eylem | Microsoft.ApiManagement/service/groups/users/read | Bir grup ile ilişkili kullanıcı varlık koleksiyonunu listeler. |
> | Eylem | Microsoft.ApiManagement/service/groups/users/write | Varolan kullanıcı için varolan bir grubu Ekle |
> | Eylem | Microsoft.ApiManagement/service/groups/write | Oluşturur veya bir grubu güncelleştirir. veya kendi tanımlayıcısı tarafından belirtilen grubunun ayrıntılarını güncelleştirmeleri. |
> | Eylem | Microsoft.ApiManagement/service/identityProviders/delete | Belirtilen kimlik sağlayıcısı yapılandırması siler. |
> | Eylem | Microsoft.ApiManagement/service/identityProviders/read | Belirtilen hizmet örneği yapılandırılmış kimlik sağlayıcısı koleksiyonu listeler. veya alır, kimlik sağlayıcı yapılandırma ayrıntılarını belirtilen hizmet örneğinde yapılandırılmamış. |
> | Eylem | Microsoft.ApiManagement/service/identityProviders/write | Oluşturur veya Identityprovider yapılandırmasını güncelleştirir. veya var olan Identityprovider yapılandırmasını güncelleştirir. |
> | Eylem | Microsoft.ApiManagement/service/issues/read | Belirtilen hizmet örneği sorunları koleksiyonu listeler. veya API Management alır sorun ayrıntıları |
> | Eylem | Microsoft.ApiManagement/service/locations/networkstatus/read | Ağ erişim durumu hakkında hizmetin bağımlı olduğu kaynakları, konumu alır. |
> | Eylem | Microsoft.ApiManagement/service/loggers/delete | Belirtilen Günlükçü siler. |
> | Eylem | Microsoft.ApiManagement/service/loggers/read | Belirtilen hizmet örneğindeki günlükçüleri koleksiyonu listeler. veya kendi tanımlayıcısı tarafından belirtilen Günlükçü ayrıntılarını alır. |
> | Eylem | Microsoft.ApiManagement/service/loggers/write | Oluşturur veya bir Günlükçü güncelleştirir. veya mevcut bir Günlükçü güncelleştirir. |
> | Eylem | Microsoft.ApiManagement/service/managedeployments/action | SKU/birimlerini, API Management hizmetinin bölgesel dağıtımlara ekleme/kaldırma değiştirme |
> | Eylem | Microsoft.ApiManagement/service/networkstatus/read | Ağ erişim durumu hakkında hizmetin bağımlı olduğu kaynakları alır. |
> | Eylem | Microsoft.ApiManagement/service/notifications/action | Belirli bir kullanıcıya bildirim gönderir |
> | Eylem | Microsoft.ApiManagement/service/notifications/read | Bir hizmet örneği içinde tanımlanan özellikler koleksiyonu listeler. veya kendi tanımlayıcısı tarafından belirtilen bildirim ayrıntılarını alır. |
> | Eylem | Microsoft.ApiManagement/service/notifications/recipientEmails/delete | E-posta bildirimi listesinden kaldırır. |
> | Eylem | Microsoft.ApiManagement/service/notifications/recipientEmails/read | Alır, bildirim alıcı e-postalarının listesi bir bildirime nasıl abone. |
> | Eylem | Microsoft.ApiManagement/service/notifications/recipientEmails/write | E-posta adresine bildirim için alıcı listesine ekler. |
> | Eylem | Microsoft.ApiManagement/service/notifications/recipientUsers/delete | API Management Kullanıcı bildirim listesinden kaldırır. |
> | Eylem | Microsoft.ApiManagement/service/notifications/recipientUsers/read | Alır, bildirim alıcısı kullanıcı listesini bildirime abone. |
> | Eylem | Microsoft.ApiManagement/service/notifications/recipientUsers/write | API Yönetimi Kullanıcı bildirim için alıcı listesine ekler. |
> | Eylem | Microsoft.ApiManagement/service/notifications/write | Oluşturma veya güncelleştirme API Management yayımcı bildirim. |
> | Eylem | Microsoft.ApiManagement/service/openidConnectProviders/delete | Özel Openıd Connect sağlayıcısı API Management hizmet örneği siler. |
> | Eylem | Microsoft.ApiManagement/service/openidConnectProviders/read | Tüm Openıd Connect sağlayıcısının listeler. veya belirli Openıd Connect sağlayıcısı alır. |
> | Eylem | Microsoft.ApiManagement/service/openidConnectProviders/write | Oluşturur veya Openıd Connect sağlayıcısı güncelleştirir. veya güncelleştirmeleri belirli Openıd Connect sağlayıcısı. |
> | Eylem | Microsoft.ApiManagement/service/operationresults/read | Uzun süre çalışan işlemin geçerli durumunu alır |
> | Eylem | Microsoft.ApiManagement/service/policies/delete | API yönetim Hizmeti'nin genel ilke yapılandırmasını siler. |
> | Eylem | Microsoft.ApiManagement/service/policies/read | API Management hizmeti tüm genel ilke tanımlarını listeler. veya, API Management hizmetinin genel ilke tanımı Al. |
> | Eylem | Microsoft.ApiManagement/service/policies/write | Oluşturur veya API Management hizmetinin genel ilke yapılandırmasını güncelleştirir. |
> | Eylem | Microsoft.ApiManagement/service/policy/delete | Kiracı düzeyinde ilke yapılandırmasını Sil |
> | Eylem | Microsoft.ApiManagement/service/policy/read | Kiracı düzeyinde ilke yapılandırmasını al |
> | Eylem | Microsoft.ApiManagement/service/policy/write | İlke Yapılandırması Kiracı düzeyi oluşturma |
> | Eylem | Microsoft.ApiManagement/service/policySnippets/read | Tüm ilke kod parçacığı listeler. |
> | Eylem | Microsoft.ApiManagement/service/portalsettings/read | Portalı veya Get oturum Portal ayarlarını yedekleme veya portalı için temsilci seçme ayarlarını almak için oturum ayarlarında alın. |
> | Eylem | Microsoft.ApiManagement/service/portalsettings/write | Oturum açma ayarlarını güncelleştirin. veya oluşturma veya güncelleştirme oturum açma ayarları. ya da güncelleştirme Kaydol ayarları veya ayarları güncelleştirme kaydolun veya güncelleştirme temsilci ayarları. veya oluşturma veya güncelleştirme temsilci seçme ayarları. |
> | Eylem | Microsoft.ApiManagement/service/products/apis/delete | Belirtilen API belirtilen ürün siler. |
> | Eylem | Microsoft.ApiManagement/service/products/apis/read | Listeleri bir API koleksiyonudur bir ürünle ilişkilendirilmiş. |
> | Eylem | Microsoft.ApiManagement/service/products/apis/write | Bir API için belirtilen ürün ekler. |
> | Eylem | Microsoft.ApiManagement/service/products/delete | Ürün silin. |
> | Eylem | Microsoft.ApiManagement/service/products/groups/delete | Belirtilen grup ve ürün arasındaki ilişkiyi siler. |
> | Eylem | Microsoft.ApiManagement/service/products/groups/read | Belirtilen ürün ile ilişkili Geliştirici grupları koleksiyonu listeler. |
> | Eylem | Microsoft.ApiManagement/service/products/groups/write | Belirtilen ürün ile belirtilen bir geliştirici grubu arasındaki ilişkiyi ekler. |
> | Eylem | Microsoft.ApiManagement/service/products/policies/delete | Ürün ilkesi yapılandırmasını siler. |
> | Eylem | Microsoft.ApiManagement/service/products/policies/read | İlke Yapılandırması ürün düzeyinde alın. ya da ilke yapılandırması ürün düzeyinde alın. |
> | Eylem | Microsoft.ApiManagement/service/products/policies/write | Oluşturur veya ilke yapılandırması ürün için güncelleştirir. |
> | Eylem | Microsoft.ApiManagement/service/products/policy/delete | İlke Yapılandırması ürün düzeyinde Sil |
> | Eylem | Microsoft.ApiManagement/service/products/policy/read | İlke Yapılandırması ürün düzeyinde Al |
> | Eylem | Microsoft.ApiManagement/service/products/policy/write | İlke Yapılandırması ürün ile ilgili olarak düzeyi oluşturma |
> | Eylem | Microsoft.ApiManagement/service/products/read | Belirtilen hizmet örneği ürünlerde koleksiyonu listeler. veya kendi tanımlayıcısı tarafından belirtilen ürün ayrıntılarını alır. |
> | Eylem | Microsoft.ApiManagement/service/products/subscriptions/read | Belirtilen ürün aboneliklere koleksiyonu listeler. |
> | Eylem | Microsoft.ApiManagement/service/products/tags/delete | Etiket ürünün çıkarın. |
> | Eylem | Microsoft.ApiManagement/service/products/tags/read | Ürün ile ilişkili tüm etiketleri listeler. veya ürünle ilişkilendirilmiş etiketi Al. |
> | Eylem | Microsoft.ApiManagement/service/products/tags/write | Ürüne etiket atayın. |
> | Eylem | Microsoft.ApiManagement/service/products/write | Oluşturur veya bir ürün güncelleştirir. veya mevcut ürün ayrıntıları güncelleştirilemiyor. |
> | Eylem | Microsoft.ApiManagement/service/productsByTags/read | Etiketler ile ilişkili ürünleri koleksiyonu listeler. |
> | Eylem | Microsoft.ApiManagement/service/properties/delete | Belirli bir özelliği, API Management hizmet örneği siler. |
> | Eylem | Microsoft.ApiManagement/service/properties/read | Bir hizmet örneği içinde tanımlanan özellikler koleksiyonu listeler. veya kendi tanımlayıcısı tarafından belirtilen özellik ayrıntılarını alır. |
> | Eylem | Microsoft.ApiManagement/service/properties/write | Oluşturur veya bir özelliğini güncelleştirir. veya belirli bir özelliğini güncelleştirir. |
> | Eylem | Microsoft.ApiManagement/service/quotas/periods/read | Dönem için kota sayaç değeri Al |
> | Eylem | Microsoft.ApiManagement/service/quotas/periods/write | Geçerli sayaç değeri, kota ayarlama |
> | Eylem | Microsoft.ApiManagement/service/quotas/read | İçin kota değerlerini alma |
> | Eylem | Microsoft.ApiManagement/service/quotas/write | Geçerli sayaç değeri, kota ayarlama |
> | Eylem | Microsoft.ApiManagement/service/read | Bir API Management hizmet örneği için meta veriler okuma |
> | Eylem | Microsoft.ApiManagement/service/regions/read | Hizmet bulunduğu tüm azure bölgelerinde listeler. |
> | Eylem | Microsoft.ApiManagement/service/reports/read | Dönemleri ya da coğrafi bölge veya Get rapor geliştiriciler tarafından toplanan göre toplanır Get rapor göre toplanır rapor alın.<br>ya da ürün tarafından toplanan rapor alın.<br>ya da API'ler veya Get rapor işlemleri veya abonelik tarafından toplanan Get rapor göre toplanır göre toplanır rapor alın.<br>ya da raporlama verilerini istekleri Al |
> | Eylem | Microsoft.ApiManagement/service/restore/action | API Management hizmeti bir kullanıcı tarafından sağlanan depolama hesabı belirtilen kapsayıcıda geri yükleme |
> | Eylem | Microsoft.ApiManagement/service/subscriptions/delete | Belirtilen abonelik siler. |
> | Eylem | Microsoft.ApiManagement/service/subscriptions/read | API Management hizmet örneği, tüm abonelikleri listeler. ya da belirtilen abonelik varlığı alır. |
> | Eylem | Microsoft.ApiManagement/service/subscriptions/regeneratePrimaryKey/action | API Management hizmet örneği, mevcut abonelik birincil anahtarı yeniden oluşturur. |
> | Eylem | Microsoft.ApiManagement/service/subscriptions/regenerateSecondaryKey/action | API Management hizmet örneği, mevcut abonelik ikincil anahtarı yeniden oluşturur. |
> | Eylem | Microsoft.ApiManagement/service/subscriptions/write | Oluşturur veya abonelik belirtilen kullanıcının belirtilen ürün için güncelleştirir. veya kendi tanımlayıcısı tarafından belirtilen abonelik ayrıntılarını güncelleştirmeleri. |
> | Eylem | Microsoft.ApiManagement/service/tagResources/read | Etiketler ile ilişkili bir kaynak koleksiyonu listeler. |
> | Eylem | Microsoft.ApiManagement/service/tags/delete | API Management hizmet örneği, belirli bir etiketi siler. |
> | Eylem | Microsoft.ApiManagement/service/tags/read | Bir hizmet örneği içinde tanımlanan etiketler koleksiyonu listeler. veya kendi tanımlayıcısı tarafından belirtilen etiket ayrıntılarını alır. |
> | Eylem | Microsoft.ApiManagement/service/tags/write | Bir etiketi oluşturur. veya kendi tanımlayıcısı tarafından belirtilen etiket ayrıntılarını güncelleştirmeleri. |
> | Eylem | Microsoft.ApiManagement/service/templates/delete | Varsayılan API Management e-posta şablonu Sıfırla |
> | Eylem | Microsoft.ApiManagement/service/templates/read | Tüm e-posta şablonları veya API Management alır e-posta şablonu ayrıntılarını alır. |
> | Eylem | Microsoft.ApiManagement/service/templates/write | API Management e-posta şablonu ya da API Management güncelleştirmeleri e-posta şablonu oluştur veya güncelleştir |
> | Eylem | Microsoft.ApiManagement/service/tenant/delete | Kiracı ilkesi yapılandırmasını Kaldır |
> | Eylem | Microsoft.ApiManagement/service/tenant/deploy/action | Yapılandırmasına veritabanındaki belirtilen git dalı değişiklikleri uygulamak için bir dağıtım görevini çalıştırır. |
> | Eylem | Microsoft.ApiManagement/service/tenant/operationResults/read | İşlem sonuçlarını listesini alın veya belirli bir işlemin sonucunu Al |
> | Eylem | Microsoft.ApiManagement/service/tenant/read | API Management hizmetinin genel ilke tanımı Al veya Get Kiracı erişim bilgileri ayrıntıları |
> | Eylem | Microsoft.ApiManagement/service/tenant/regeneratePrimaryKey/action | Birincil erişim anahtarını yeniden oluştur |
> | Eylem | Microsoft.ApiManagement/service/tenant/regenerateSecondaryKey/action | İkincil erişim anahtarını yeniden oluştur |
> | Eylem | Microsoft.ApiManagement/service/tenant/save/action | İşleme için belirtilmiş olan dalı depoda bulunan yapılandırma anlık görüntü oluşturur |
> | Eylem | Microsoft.ApiManagement/service/tenant/syncState/read | En son git eşitleme durumunu Al |
> | Eylem | Microsoft.ApiManagement/service/tenant/validate/action | Belirtilen git dalındaki değişiklikleri doğrulama |
> | Eylem | Microsoft.ApiManagement/service/tenant/write | İlke yapılandırmasının kiracısı veya güncelleştirme Kiracı erişim bilgileri ayrıntıları |
> | Eylem | Microsoft.ApiManagement/service/updatecertificate/action | Bir API Management hizmeti için SSL sertifikasını karşıya yükle |
> | Eylem | Microsoft.ApiManagement/service/updatehostname/action | Kurulum, güncelleştirmeniz ya da bir API Management hizmeti için özel etki alanı adlarını Kaldır |
> | Eylem | Microsoft.ApiManagement/service/users/action | Yeni bir kullanıcı kaydı |
> | Eylem | Microsoft.ApiManagement/service/users/confirmations/send/action | Onay gönderir |
> | Eylem | Microsoft.ApiManagement/service/users/delete | Belirli bir kullanıcıyı siler. |
> | Eylem | Microsoft.ApiManagement/service/users/generateSsoUrl/action | Belirli bir kullanıcı Geliştirici portalına imzalamak için bir kimlik doğrulama belirteci içeren bir yeniden yönlendirme URL'sini alır. |
> | Eylem | Microsoft.ApiManagement/service/users/groups/read | Tüm kullanıcı gruplarını listeler. |
> | Eylem | Microsoft.ApiManagement/service/users/identities/read | Tüm kullanıcı kimlikleri listesi. |
> | Eylem | Microsoft.ApiManagement/service/users/keys/read | Kullanıcıyla ilişkili anahtarları alma |
> | Eylem | Microsoft.ApiManagement/service/users/read | Belirtilen hizmet örneği, kayıtlı kullanıcı koleksiyonu listeler. veya kendi tanımlayıcısı tarafından belirtilen kullanıcı ayrıntılarını alır. |
> | Eylem | Microsoft.ApiManagement/service/users/subscriptions/read | Belirtilen kullanıcının abonelikleri koleksiyonu listeler. |
> | Eylem | Microsoft.ApiManagement/service/users/token/action | Paylaşılan erişim yetkilendirme için kullanıcı belirteci alır. |
> | Eylem | Microsoft.ApiManagement/service/users/write | Oluşturur veya bir kullanıcı güncelleştirir. veya güncelleştirmeleri onun tanımlayıcısı tarafından belirtilen kullanıcının ayrıntıları. |
> | Eylem | Microsoft.ApiManagement/service/write | Yeni bir API Management hizmeti örneği oluşturma |
> | Eylem | Microsoft.ApiManagement/unregister/action | Kaydını Microsoft.ApiManagement kaynak sağlayıcısı için aboneliği |

## <a name="microsoftauthorization"></a>Microsoft.Authorization

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Authorization/classicAdministrators/delete | Yönetici abonelikten kaldırır. |
> | Eylem | Microsoft.Authorization/classicAdministrators/operationstatuses/read | Yönetici aboneliğin işlemi durumlarını alır. |
> | Eylem | Microsoft.Authorization/classicAdministrators/read | Abonelik için yöneticileri okur. |
> | Eylem | Microsoft.Authorization/classicAdministrators/write | Ekleyin veya bir aboneliğe yönetici değiştirin. |
> | Eylem | Microsoft.Authorization/denyAssignments/delete | Belirtilen kapsamda bir reddetme atamasını silin. |
> | Eylem | Microsoft.Authorization/denyAssignments/read | Bir reddetme atama hakkında bilgi alın. |
> | Eylem | Microsoft.Authorization/denyAssignments/write | Belirtilen kapsamda bir reddetme ataması oluşturun. |
> | Eylem | Microsoft.Authorization/elevateAccess/action | Çağırana Kiracı kapsamında Kullanıcı Erişim Yöneticisi erişimi verir |
> | Eylem | Microsoft.Authorization/locks/delete | Belirtilen kapsamdaki kilitleri silin. |
> | Eylem | Microsoft.Authorization/locks/read | Belirtilen kapsamdaki kilitleri alır. |
> | Eylem | Microsoft.Authorization/locks/write | Belirtilen kapsama kilitler ekleyin. |
> | Eylem | Microsoft.Authorization/operations/read | İşlemlerin listesini alır |
> | Eylem | Microsoft.Authorization/permissions/read | Arayanın belirli bir kapsamda olan tüm izinleri listeler. |
> | Eylem | Microsoft.Authorization/policyAssignments/delete | Belirtilen kapsamda bir ilke atamasını silin. |
> | Eylem | Microsoft.Authorization/policyAssignments/read | Bir ilke ataması hakkında bilgi alın. |
> | Eylem | Microsoft.Authorization/policyAssignments/write | Belirtilen kapsamda bir ilke ataması oluşturun. |
> | Eylem | Microsoft.Authorization/policyDefinitions/delete | İlke tanımını silin. |
> | Eylem | Microsoft.Authorization/policyDefinitions/read | Bir ilke tanımı hakkında bilgi alın. |
> | Eylem | Microsoft.Authorization/policyDefinitions/write | Özel bir ilke tanımı oluşturun. |
> | Eylem | Microsoft.Authorization/policySetDefinitions/delete | Bir ilke kümesi tanımını silin. |
> | Eylem | Microsoft.Authorization/policySetDefinitions/read | Bir ilke kümesi tanımı hakkında bilgi alın. |
> | Eylem | Microsoft.Authorization/policySetDefinitions/write | Bir özel ilke kümesi tanımı oluşturun. |
> | Eylem | Microsoft.Authorization/providerOperations/read | Rol tanımlarında kullanılabilen tüm kaynak sağlayıcılarına ilişkin işlemleri alın. |
> | Eylem | Microsoft.Authorization/roleAssignments/delete | Belirtilen kapsamda bir rol atamasını silin. |
> | Eylem | Microsoft.Authorization/roleAssignments/read | Bir rol ataması hakkında bilgi alın. |
> | Eylem | Microsoft.Authorization/roleAssignments/write | Belirtilen kapsamda bir rol ataması oluşturun. |
> | Eylem | Microsoft.Authorization/roleDefinitions/delete | Belirtilen özel rol tanımını silin. |
> | Eylem | Microsoft.Authorization/roleDefinitions/read | Rol tanımı hakkında bilgi alın. |
> | Eylem | Microsoft.Authorization/roleDefinitions/write | Veya belirtilen izinlerle ve atanabilir kapsamlarla özel bir rol tanımı güncelleştirilemiyor. |

## <a name="microsoftautomation"></a>Gibi Microsoft.Automation

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Automation/automationAccounts/agentRegistrationInformation/read | Bir Azure Otomasyonu DSC'ın kayıt bilgileri okuyun |
> | Eylem | Microsoft.Automation/automationAccounts/agentRegistrationInformation/regenerateKey/action | Azure Otomasyonu DSC anahtarlarını yeniden oluşturmak üzere bir istek Yazar |
> | Eylem | Microsoft.Automation/automationAccounts/certificates/delete | Bir Azure Otomasyonu sertifika varlığını siler |
> | Eylem | Microsoft.Automation/automationAccounts/certificates/getCount/action | Sertifika sayısı okur |
> | Eylem | Microsoft.Automation/automationAccounts/certificates/read | Azure Otomasyonu sertifika varlığı alır |
> | Eylem | Microsoft.Automation/automationAccounts/certificates/write | Bir Azure Otomasyonu sertifika varlığı oluşturur veya güncelleştirir |
> | Eylem | Microsoft.Automation/automationAccounts/compilationjobs/read | Bir Azure Otomasyonu DSC'ın derleme okur |
> | Eylem | Microsoft.Automation/automationAccounts/compilationjobs/read | Bir Azure Otomasyonu DSC'ın derleme okur |
> | Eylem | Microsoft.Automation/automationAccounts/compilationjobs/write | Bir Azure Otomasyonu DSC'ın derleme Yazar |
> | Eylem | Microsoft.Automation/automationAccounts/compilationjobs/write | Bir Azure Otomasyonu DSC'ın derleme Yazar |
> | Eylem | Microsoft.Automation/automationAccounts/configurations/content/read | Yapılandırma medya içeriği okur |
> | Eylem | Microsoft.Automation/automationAccounts/configurations/delete | Bir Azure Otomasyonu DSC'in içeriği siler |
> | Eylem | Microsoft.Automation/automationAccounts/configurations/getCount/action | Bir Azure Otomasyonu DSC'in içeriği sayısını okur |
> | Eylem | Microsoft.Automation/automationAccounts/configurations/read | Bir Azure Otomasyonu DSC'ın içeriğini alır |
> | Eylem | Microsoft.Automation/automationAccounts/configurations/write | Bir Azure Otomasyonu DSC'in içeriği Yazar |
> | Eylem | Microsoft.Automation/automationAccounts/connections/delete | Bir Azure Otomasyonu bağlantı varlığını siler |
> | Eylem | Microsoft.Automation/automationAccounts/connections/getCount/action | Bağlantı sayısı okur |
> | Eylem | Microsoft.Automation/automationAccounts/connections/read | Bir Azure Otomasyonu bağlantı varlığı alır |
> | Eylem | Microsoft.Automation/automationAccounts/connections/write | Bir Azure Otomasyonu bağlantı varlığı oluşturur veya güncelleştirir |
> | Eylem | Microsoft.Automation/automationAccounts/connectionTypes/delete | Bir Azure Otomasyonu bağlantı türü varlığını siler |
> | Eylem | Microsoft.Automation/automationAccounts/connectionTypes/read | Bir Azure Otomasyonu bağlantı türü varlığını alır |
> | Eylem | Microsoft.Automation/automationAccounts/connectionTypes/write | Bir Azure Otomasyonu bağlantı türü varlığı oluşturur |
> | Eylem | Microsoft.Automation/automationAccounts/credentials/delete | Bir Azure Otomasyonu kimlik bilgisi varlığını siler |
> | Eylem | Microsoft.Automation/automationAccounts/credentials/getCount/action | Kimlik bilgileri sayısını okur |
> | Eylem | Microsoft.Automation/automationAccounts/credentials/read | Azure Otomasyonu kimlik bilgisi varlığı alır |
> | Eylem | Microsoft.Automation/automationAccounts/credentials/write | Bir Azure Otomasyonu kimlik bilgisi varlığı oluşturur veya güncelleştirir |
> | Eylem | Microsoft.Automation/automationAccounts/delete | Bir Azure Otomasyonu hesabını siler |
> | Eylem | Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/delete | Karma Runbook çalışanı kaynaklarını siler |
> | Eylem | Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read | Karma Runbook çalışanı kaynaklarını okur |
> | Eylem | Microsoft.Automation/automationAccounts/jobs/output/read | Bir işin çıktısını alır |
> | Eylem | Microsoft.Automation/automationAccounts/jobs/read | Bir Azure Otomasyonu işini alır |
> | Eylem | Microsoft.Automation/automationAccounts/jobs/resume/action | Bir Azure Otomasyonu işini sürdürür |
> | Eylem | Microsoft.Automation/automationAccounts/jobs/runbookContent/action | İş yürütme sırasında Azure Otomasyon runbook'unun içeriğini alır |
> | Eylem | Microsoft.Automation/automationAccounts/jobs/stop/action | Bir Azure Otomasyonu işini durdurur |
> | Eylem | Microsoft.Automation/automationAccounts/jobs/streams/read | Bir Azure Otomasyonu iş akışı alır |
> | Eylem | Microsoft.Automation/automationAccounts/jobs/streams/read | Bir Azure Otomasyonu iş akışı alır |
> | Eylem | Microsoft.Automation/automationAccounts/jobs/suspend/action | Azure Otomasyonu işini askıya alır |
> | Eylem | Microsoft.Automation/automationAccounts/jobs/write | Bir Azure Otomasyonu işi oluşturur |
> | Eylem | Microsoft.Automation/automationAccounts/jobSchedules/delete | Bir Azure Otomasyonu iş zamanlaması silme |
> | Eylem | Microsoft.Automation/automationAccounts/jobSchedules/read | Bir Azure Otomasyonu iş zamanlaması alır |
> | Eylem | Microsoft.Automation/automationAccounts/jobSchedules/write | Bir Azure Otomasyonu iş zamanlaması oluşturur |
> | Eylem | Microsoft.Automation/automationAccounts/linkedWorkspace/read | Otomasyon hesabına bağlı çalışma alanını alır |
> | Eylem | Microsoft.Automation/automationAccounts/listKeys/action | Otomasyon hesabı için anahtarları okur |
> | Eylem | Microsoft.Automation/automationAccounts/modules/activities/read | Azure Otomasyonu etkinlikler alır |
> | Eylem | Microsoft.Automation/automationAccounts/modules/delete | Bir Azure Otomasyonu Powershell modülünü siler |
> | Eylem | Microsoft.Automation/automationAccounts/modules/getCount/action | Otomasyon hesabının Powershell modülleri sayısını alır |
> | Eylem | Microsoft.Automation/automationAccounts/modules/read | Bir Azure Otomasyonu Powershell modülünü alır |
> | Eylem | Microsoft.Automation/automationAccounts/modules/write | Oluşturur veya bir Azure Otomasyonu Powershell modülünü güncelleştirir |
> | Eylem | Microsoft.Automation/automationAccounts/nodeConfigurations/delete | Bir Azure Otomasyonu DSC'ın düğüm yapılandırması siler |
> | Eylem | Microsoft.Automation/automationAccounts/nodeConfigurations/rawContent/action | Bir Azure Otomasyonu DSC'ın düğüm yapılandırma içeriği okur |
> | Eylem | Microsoft.Automation/automationAccounts/nodeConfigurations/read | Bir Azure Otomasyonu DSC'ın düğüm yapılandırması okur |
> | Eylem | Microsoft.Automation/automationAccounts/nodeConfigurations/write | Bir Azure Otomasyonu DSC'ın düğüm yapılandırması Yazar |
> | Eylem | Microsoft.Automation/automationAccounts/nodecounts/read | Belirtilen tür için düğüm sayısını Özet okur |
> | Eylem | Microsoft.Automation/automationAccounts/nodes/delete | Azure Otomasyonu DSC düğümleri siler |
> | Eylem | Microsoft.Automation/automationAccounts/nodes/read | Azure Otomasyonu DSC düğümleri okur |
> | Eylem | Microsoft.Automation/automationAccounts/nodes/reports/content/read | Azure Otomasyonu DSC rapor içeriğini okur |
> | Eylem | Microsoft.Automation/automationAccounts/nodes/reports/read | Azure Otomasyonu DSC raporlarını okur |
> | Eylem | Microsoft.Automation/automationAccounts/nodes/write | Oluşturur veya güncelleştirir Azure Automation DSC düğümleri |
> | Eylem | Microsoft.Automation/automationAccounts/objectDataTypes/fields/read | Azure Otomasyonu TypeFields alır |
> | Eylem | Microsoft.Automation/automationAccounts/python2Packages/delete | Bir Azure Otomasyonu Python 2 paketini siler |
> | Eylem | Microsoft.Automation/automationAccounts/python2Packages/read | Bir Azure Otomasyonu Python 2 paketini alır |
> | Eylem | Microsoft.Automation/automationAccounts/python2Packages/write | Oluşturur veya bir Azure Otomasyonu Python 2 paketini güncelleştirir |
> | Eylem | Microsoft.Automation/automationAccounts/python3Packages/delete | Bir Azure Otomasyonu Python 3 paket siler |
> | Eylem | Microsoft.Automation/automationAccounts/python3Packages/read | Bir Azure Otomasyonu Python 3 paketini alır |
> | Eylem | Microsoft.Automation/automationAccounts/python3Packages/write | Oluşturur veya bir Azure Otomasyonu Python 3 paketi güncelleştirir |
> | Eylem | Microsoft.Automation/automationAccounts/read | Bir Azure Otomasyonu hesabını alır |
> | Eylem | Microsoft.Automation/automationAccounts/runbooks/content/read | Bir Azure Otomasyonu runbook içeriğini alır |
> | Eylem | Microsoft.Automation/automationAccounts/runbooks/delete | Bir Azure Otomasyonu runbook'unu siler |
> | Eylem | Microsoft.Automation/automationAccounts/runbooks/draft/content/write | Azure Otomasyonu runbook taslağının içeriğini oluşturur |
> | Eylem | Microsoft.Automation/automationAccounts/runbooks/draft/operationResults/read | Azure Otomasyonu runbook taslağı İşlem sonuçlarını alır |
> | Eylem | Microsoft.Automation/automationAccounts/runbooks/draft/read | Bir Azure Otomasyonu runbook taslağı alır |
> | Eylem | Microsoft.Automation/automationAccounts/runbooks/draft/testJob/read | Bir Azure Otomasyonu runbook taslağı test işini alır |
> | Eylem | Microsoft.Automation/automationAccounts/runbooks/draft/testJob/resume/action | Bir Azure Otomasyonu runbook taslağı test işini sürdürür |
> | Eylem | Microsoft.Automation/automationAccounts/runbooks/draft/testJob/stop/action | Bir Azure Otomasyonu runbook taslağı test işini durdurur |
> | Eylem | Microsoft.Automation/automationAccounts/runbooks/draft/testJob/suspend/action | Bir Azure Otomasyonu runbook taslağı test işini askıya alır |
> | Eylem | Microsoft.Automation/automationAccounts/runbooks/draft/testJob/write | Bir Azure Otomasyonu runbook taslağı test işi oluşturur |
> | Eylem | Microsoft.Automation/automationAccounts/runbooks/draft/undoEdit/action | Bir Azure Otomasyonu runbook taslağı düzenlemeleri geri al |
> | Eylem | Microsoft.Automation/automationAccounts/runbooks/getCount/action | Azure Otomasyonu runbook'ların sayısını alır |
> | Eylem | Microsoft.Automation/automationAccounts/runbooks/publish/action | Bir Azure Otomasyonu runbook taslağı yayımlar |
> | Eylem | Microsoft.Automation/automationAccounts/runbooks/read | Bir Azure Otomasyonu runbook'unu alır |
> | Eylem | Microsoft.Automation/automationAccounts/runbooks/write | Oluşturur veya bir Azure Otomasyonu runbook'u güncelleştirir |
> | Eylem | Microsoft.Automation/automationAccounts/schedules/delete | Bir Azure Otomasyonu zamanlama varlığını siler |
> | Eylem | Microsoft.Automation/automationAccounts/schedules/getCount/action | Azure Otomasyonu zamanlama sayısını alır |
> | Eylem | Microsoft.Automation/automationAccounts/schedules/read | Bir Azure Otomasyonu zamanlama varlığını alır |
> | Eylem | Microsoft.Automation/automationAccounts/schedules/write | Bir Azure Otomasyonu zamanlama varlığı oluşturur veya güncelleştirir |
> | Eylem | Microsoft.Automation/automationAccounts/softwareUpdateConfigurations/delete | Bir Azure Otomasyonu yazılım güncelleştirme yapılandırması siler |
> | Eylem | Microsoft.Automation/automationAccounts/softwareUpdateConfigurations/read | Bir Azure Otomasyonu yazılım güncelleştirme yapılandırması alır |
> | Eylem | Microsoft.Automation/automationAccounts/softwareUpdateConfigurations/write | Oluşturur veya güncelleştirir Azure Otomasyon yazılım güncelleştirme yapılandırması |
> | Eylem | Microsoft.Automation/automationAccounts/statistics/read | Azure Otomasyonu istatistiklerini alır |
> | Eylem | Microsoft.Automation/automationAccounts/updateDeploymentMachineRuns/read | Bir Azure Otomasyonu güncelleştirme dağıtım makinesini Al |
> | Eylem | Microsoft.Automation/automationAccounts/updateManagementPatchJob/read | Güncelleştirme yönetimi düzeltme eki Azure Otomasyonu işini alır |
> | Eylem | Microsoft.Automation/automationAccounts/usages/read | Azure Otomasyonu kullanımı alır |
> | Eylem | Microsoft.Automation/automationAccounts/variables/delete | Bir Azure Otomasyonu değişken varlığını siler |
> | Eylem | Microsoft.Automation/automationAccounts/variables/read | Bir Azure Otomasyonu değişken varlığını okur |
> | Eylem | Microsoft.Automation/automationAccounts/variables/write | Bir Azure Otomasyonu değişken varlığı oluşturur veya güncelleştirir |
> | Eylem | Microsoft.Automation/automationAccounts/watchers/delete | Azure Otomasyonu İzleyicisi işini sil |
> | Eylem | Microsoft.Automation/automationAccounts/watchers/read | Bir Azure Otomasyonu İzleyicisi işini alır |
> | Eylem | Microsoft.Automation/automationAccounts/watchers/start/action | Azure Otomasyonu İzleyicisi işini Başlat |
> | Eylem | Microsoft.Automation/automationAccounts/watchers/stop/action | Bir Azure Otomasyonu İzleyicisi işini durdurma |
> | Eylem | Microsoft.Automation/automationAccounts/watchers/streams/read | Bir Azure Otomasyonu İzleyicisi iş akışı alır |
> | Eylem | Microsoft.Automation/automationAccounts/watchers/watcherActions/delete | Bir Azure Otomasyonu İzleyicisi iş eylemleri Sil |
> | Eylem | Microsoft.Automation/automationAccounts/watchers/watcherActions/read | Bir Azure Otomasyonu İzleyicisi iş eylemleri alır. |
> | Eylem | Microsoft.Automation/automationAccounts/watchers/watcherActions/write | Bir Azure Otomasyonu İzleyicisi iş Eylemler oluşturma |
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
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.AzureActiveDirectory/b2cDirectories/delete | B2C dizini kaynağını Sil |
> | Eylem | Microsoft.AzureActiveDirectory/b2cDirectories/read | B2C dizini kaynağını görüntüle |
> | Eylem | Microsoft.AzureActiveDirectory/b2cDirectories/write | B2C dizini kaynağını güncelle |
> | Eylem | Microsoft.AzureActiveDirectory/b2ctenants/read | Kullanıcının üyesi olduğu tüm B2C kiracılarına listeler |
> | Eylem | Microsoft.AzureActiveDirectory/operations/read | Tüm kullanılabilir API işlemlerini Microsoft.AzureActiveDirectory kaynak sağlayıcısı için okuma |
> | Eylem | Microsoft.AzureActiveDirectory/register/action | Microsoft.AzureActiveDirectory kaynak sağlayıcısı için aboneliği kaydedin |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.AzureStack/Operations/read | Bir kaynak sağlayıcısı işlemi özelliklerini alır |
> | Eylem | Microsoft.AzureStack/register/action | Aboneliği Microsoft.AzureStack kaynak sağlayıcısına kaydeder |
> | Eylem | Microsoft.AzureStack/registrations/customerSubscriptions/delete | Bir Azure Stack Müşteri aboneliğini siler |
> | Eylem | Microsoft.AzureStack/registrations/customerSubscriptions/read | Azure Stack müşteri aboneliği özelliklerini alır |
> | Eylem | Microsoft.AzureStack/registrations/customerSubscriptions/write | Oluşturur veya bir Azure Stack müşteri aboneliği güncelleştirir |
> | Eylem | Microsoft.AzureStack/registrations/delete | Azure Stack kaydını siler |
> | Eylem | Microsoft.AzureStack/registrations/getActivationKey/action | En son Azure Stack etkinleştirme anahtarı alır. |
> | Eylem | Microsoft.AzureStack/registrations/products/listDetails/action | Azure Stack Marketini ürün ayrıntılarını genişletilmiş alır |
> | Eylem | Microsoft.AzureStack/registrations/products/read | Azure Stack Marketini ürün özelliklerini alır |
> | Eylem | Microsoft.AzureStack/registrations/read | Azure Stack kayıt özelliklerini alır |
> | Eylem | Microsoft.AzureStack/registrations/write | Oluşturur veya bir Azure Stack kaydı güncelleştirir |

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Batch/batchAccounts/applications/delete | Bir uygulama siler |
> | Eylem | Microsoft.Batch/batchAccounts/applications/read | Uygulamaları listeler veya bir uygulamanın özelliklerini alır |
> | Eylem | Microsoft.Batch/batchAccounts/applications/versions/activate/action | Bir uygulama paketi etkinleştirir |
> | Eylem | Microsoft.Batch/batchAccounts/applications/versions/delete | Bir uygulama paketini siler |
> | Eylem | Microsoft.Batch/batchAccounts/applications/versions/read | Bir uygulama paketi özelliklerini alır |
> | Eylem | Microsoft.Batch/batchAccounts/applications/versions/write | Yeni bir uygulama paketi oluşturur veya var olan bir uygulama paketini güncelleştirir |
> | Eylem | Microsoft.Batch/batchAccounts/applications/write | Yeni bir uygulama oluşturur veya mevcut bir uygulamayı güncelleştirir |
> | Eylem | Microsoft.Batch/batchAccounts/certificateOperationResults/read | Bir Batch hesabındaki uzun süre çalışan bir sertifika işlemin sonuçlarını alır |
> | Eylem | Microsoft.Batch/batchAccounts/certificates/cancelDelete/action | Silme işlemi başarısız bir Batch hesabında bir sertifikanın iptal eder |
> | Eylem | Microsoft.Batch/batchAccounts/certificates/delete | Sertifika Batch hesabından siler |
> | Eylem | Microsoft.Batch/batchAccounts/certificates/read | Bir Batch hesabı sertifika listeler veya bir sertifikanın özelliklerini alır |
> | Eylem | Microsoft.Batch/batchAccounts/certificates/write | Bir Batch hesabında yeni bir sertifika oluşturur veya mevcut bir sertifikayı güncelleştirir |
> | Eylem | Microsoft.Batch/batchAccounts/delete | Batch hesabını siler |
> | Eylem | Microsoft.Batch/batchAccounts/listkeys/action | Bir Batch hesabı için anahtarları erişim listeleri |
> | Eylem | Microsoft.Batch/batchAccounts/operationResults/read | Uzun süre çalışan bir Batch hesabı işlemin sonuçlarını alır |
> | Eylem | Microsoft.Batch/batchAccounts/poolOperationResults/read | Bir Batch hesabındaki bir uzun süre çalışan havuzu işlemin sonuçlarını alır |
> | Eylem | Microsoft.Batch/batchAccounts/pools/delete | Bir havuz Batch hesabından siler |
> | Eylem | Microsoft.Batch/batchAccounts/pools/disableAutoscale/action | Batch hesabının havuz için otomatik ölçeklendirmeyi devre dışı bırakır |
> | Eylem | Microsoft.Batch/batchAccounts/pools/read | Bir Batch hesabındaki havuzlarını listeler veya havuzun özelliklerini alır |
> | Eylem | Microsoft.Batch/batchAccounts/pools/stopResize/action | Bir Batch hesabı havuzu işlemi durdurur devam eden bir yeniden boyutlandırma |
> | Eylem | Microsoft.Batch/batchAccounts/pools/write | Bir Batch hesabında yeni bir havuz oluşturur veya mevcut bir havuza güncelleştirir |
> | Eylem | Microsoft.Batch/batchAccounts/read | Batch hesapları listeler veya bir Batch hesabı özelliklerini alır |
> | Eylem | Microsoft.Batch/batchAccounts/regeneratekeys/action | Erişim bir Batch hesabı için anahtarları yeniden oluşturur |
> | Eylem | Microsoft.Batch/batchAccounts/syncAutoStorageKeys/action | Bir Batch hesabı için yapılandırılmış otomatik depolama hesabının erişim anahtarlarını eşitler |
> | Eylem | Microsoft.Batch/batchAccounts/write | Yeni bir Batch hesabı oluşturur veya mevcut bir Batch hesabı güncelleştirir |
> | Eylem | Microsoft.Batch/locations/accountOperationResults/read | Uzun süre çalışan bir Batch hesabı işlemin sonuçlarını alır |
> | Eylem | Microsoft.Batch/locations/checkNameAvailability/action | Hesap adı geçerli ve kullanımda olup olmadığını denetler. |
> | Eylem | Microsoft.Batch/locations/quotas/read | Belirtilen Azure bölgesi belirtilen abonelik batch kotaları alır |
> | Eylem | Microsoft.Batch/operations/read | Microsoft.Batch kaynak sağlayıcısındaki kullanılabilir işlemleri listele |
> | Eylem | Microsoft.Batch/register/action | Batch kaynak sağlayıcısı için aboneliği kaydeder ve Batch hesaplarının oluşturulmasını sağlar |
> | Eylem | Microsoft.Batch/unregister/action | Batch hesaplarının oluşturulmasını önleme Batch kaynak sağlayıcısı için aboneliği kaydını siler |

## <a name="microsoftbilling"></a>Microsoft.Billing

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Billing/billingAccounts/departments/read | Tüm Departmanlar altında bir faturalama hesabı kapsam listesi |
> | Eylem | Microsoft.Billing/billingAccounts/enrollmentAccounts/read | Bir faturalama hesabı kapsamı altında tüm kayıt hesaplarını listeleme |
> | Eylem | Microsoft.Billing/billingAccounts/read | Hangi kullanıcı olanağının erişebildiği tüm faturalandırma hesaplarını listeleme |
> | Eylem | Microsoft.Billing/billingPeriods/read | Kullanılabilir faturalama dönemleri listeler |
> | Eylem | Microsoft.Billing/departments/read | Hangi kullanıcı olanağının erişebildiği tüm Departmanlar Listesi |
> | Eylem | Microsoft.Billing/invoices/read | Listeleri kullanılabilir faturalar |
> | Eylem | Microsoft.Billing/register/action | Aboneliği Microsoft.Billing kaynak sağlayıcısına kaydeder |

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.BingMaps/mapApis/Delete | Silme işlemi |
> | Eylem | Microsoft.BingMaps/mapApis/listSecrets/action | Gizli dizilerini listeleme |
> | Eylem | Microsoft.BingMaps/mapApis/listSingleSignOnToken/action | Çoklu oturum açma yetkilendirme belirtecini kaynak için okuma |
> | Eylem | Microsoft.BingMaps/mapApis/Read | Okuma işlemi |
> | Eylem | Microsoft.BingMaps/mapApis/regenerateKey/action | Anahtarı yeniden oluşturur |
> | Eylem | Microsoft.BingMaps/mapApis/Write | Yazma işlemi |
> | Eylem | Microsoft.BingMaps/Operations/read | İşlem açıklaması. |

## <a name="microsoftblockchain"></a>Microsoft.Blockchain

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Blockchain/blockchainMembers/delete | Varolan bir blok zinciri üye siler. |
> | Eylem | Microsoft.Blockchain/blockchainMembers/listApiKeys/action | Alır veya mevcut blok zinciri üye API anahtarlarını listeler. |
> | Eylem | Microsoft.Blockchain/blockchainMembers/read | Alır veya mevcut blok zinciri üyeleri listeler. |
> | DataAction | Microsoft.Blockchain/blockchainMembers/transactionNodes/connect/action | Bir blok zinciri üye işlem düğümüne bağlanmış olursunuz. |
> | Eylem | Microsoft.Blockchain/blockchainMembers/transactionNodes/delete | Varolan bir blok zinciri üye işlem düğümü siler. |
> | Eylem | Microsoft.Blockchain/blockchainMembers/transactionNodes/listApiKeys/action | Alır veya mevcut blok zinciri üye işlem düğümü API anahtarlarını listeler. |
> | Eylem | Microsoft.Blockchain/blockchainMembers/transactionNodes/read | Alır veya mevcut blok zinciri üye işlem düğümlerini listeler. |
> | Eylem | Microsoft.Blockchain/blockchainMembers/transactionNodes/write | Oluşturur veya bir blok zinciri üyesi işlem düğümü güncelleştirir. |
> | Eylem | Microsoft.Blockchain/blockchainMembers/write | Oluşturur veya blok zinciri üyesi güncelleştirir. |
> | Eylem | Microsoft.Blockchain/locations/blockchainMemberOperationResults/read | Blok zinciri üyelerinin İşlem sonuçlarını alır. |
> | Eylem | Microsoft.Blockchain/locations/checkNameAvailability/action | Bu kaynak adının geçerli olduğundan ve kullanılmadığını denetler. |
> | Eylem | Microsoft.Blockchain/operations/read | Tüm Microsoft Blok zinciri kaynak sağlayıcısı işlemleri listeler. |
> | Eylem | Microsoft.Blockchain/register/action | Blok zinciri kaynak sağlayıcısı için aboneliği kaydeder. |

## <a name="microsoftblueprint"></a>Microsoft.Blueprint

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Blueprint/blueprintAssignments/assignmentOperations/read | Herhangi bir blueprint yapıtını okuyun |
> | Eylem | Microsoft.Blueprint/blueprintAssignments/delete | Herhangi bir blueprint yapıtlarını silin |
> | Eylem | Microsoft.Blueprint/blueprintAssignments/read | Herhangi bir blueprint yapıtını okuyun |
> | Eylem | Microsoft.Blueprint/blueprintAssignments/whoisblueprint/action | Blueprint Azure hizmet sorumlusu nesnesi kimliğini alma |
> | Eylem | Microsoft.Blueprint/blueprintAssignments/write | Herhangi bir blueprint yapıtları güncelle |
> | Eylem | Microsoft.Blueprint/blueprints/artifacts/delete | Herhangi bir blueprint yapıtlarını silin |
> | Eylem | Microsoft.Blueprint/blueprints/artifacts/read | Herhangi bir blueprint yapıtını okuyun |
> | Eylem | Microsoft.Blueprint/blueprints/artifacts/write | Herhangi bir blueprint yapıtları güncelle |
> | Eylem | Microsoft.Blueprint/blueprints/delete | Herhangi bir blueprint'i silin |
> | Eylem | Microsoft.Blueprint/blueprints/read | Herhangi bir blueprint'i okuyun |
> | Eylem | Microsoft.Blueprint/blueprints/versions/artifacts/read | Herhangi bir blueprint yapıtını okuyun |
> | Eylem | Microsoft.Blueprint/blueprints/versions/delete | Herhangi bir blueprint'i silin |
> | Eylem | Microsoft.Blueprint/blueprints/versions/read | Herhangi bir blueprint'i okuyun |
> | Eylem | Microsoft.Blueprint/blueprints/versions/write | Herhangi bir Blueprint oluştur veya güncelleştir |
> | Eylem | Microsoft.Blueprint/blueprints/write | Herhangi bir Blueprint oluştur veya güncelleştir |
> | Eylem | Microsoft.Blueprint/register/action | Azure Blueprint kaynak sağlayıcısını Kaydet |

## <a name="microsoftbotservice"></a>Microsoft.BotService

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.BotService/botServices/channels/delete | Bot hizmeti Sil |
> | Eylem | Microsoft.BotService/botServices/channels/read | Bot hizmeti okuyun |
> | Eylem | Microsoft.BotService/botServices/channels/write | Bot hizmeti yazma |
> | Eylem | Microsoft.BotService/botServices/connections/delete | Bot hizmeti Sil |
> | Eylem | Microsoft.BotService/botServices/connections/read | Bot hizmeti okuyun |
> | Eylem | Microsoft.BotService/botServices/connections/write | Bot hizmeti yazma |
> | Eylem | Microsoft.BotService/botServices/delete | Bot hizmeti Sil |
> | Eylem | Microsoft.BotService/botServices/read | Bot hizmeti okuyun |
> | Eylem | Microsoft.BotService/botServices/write | Bot hizmeti yazma |
> | Eylem | Microsoft.BotService/locations/operationresults/read | Zaman uyumsuz bir işlemin durumunu okuma |
> | Eylem | Microsoft.BotService/Operations/read | Okuma işlemleri için tüm kaynak türleri |

## <a name="microsoftcache"></a>Microsoft.Cache

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Cache/checknameavailability/action | Yeni bir Redis önbelleği ile kullanmak için bir ad olup olmadığını denetler |
> | Eylem | Microsoft.Cache/locations/operationresults/read | Bir uzun sonucunu kendisi için 'Konum' üst bilgisi önceden istemciye döndürülen işlem çalışıyor alır |
> | Eylem | Microsoft.Cache/operations/read | 'Microsoft.Cache' sağlayıcısının desteklediği işlemleri listeler. |
> | Eylem | Microsoft.Cache/redis/delete | Redis Cache'nin tamamını sil |
> | Eylem | Microsoft.Cache/redis/export/action | Redis verilerini belirtilen biçimde ön ekli depolama BLOB'ları için dışarı aktarma |
> | Eylem | Microsoft.Cache/redis/firewallRules/delete | Bir Redis Cache'in IP güvenlik duvarı kurallarını Sil |
> | Eylem | Microsoft.Cache/redis/firewallRules/read | Bir Redis Cache'in IP güvenlik duvarı kurallarını Al |
> | Eylem | Microsoft.Cache/redis/firewallRules/write | Bir Redis Cache'in IP güvenlik duvarı kurallarını Düzenle |
> | Eylem | Microsoft.Cache/redis/forceReboot/action | Veri kaybı olasılığı olan bir önbellek örneği yeniden başlatmayı zorlayın. |
> | Eylem | Microsoft.Cache/redis/import/action | Belirli bir biçimdeki verileri Redis'e birden çok BLOB'dan Aktar |
> | Eylem | Microsoft.Cache/redis/linkedservers/delete | Redis Cache'ten bağlı sunucusunu Sil |
> | Eylem | Microsoft.Cache/redis/linkedservers/read | Redis cache ile ilişkili bağlı sunucuları alın. |
> | Eylem | Microsoft.Cache/redis/linkedservers/write | Bir Redis Cache'e bağlı Sunucusu Ekle |
> | Eylem | Microsoft.Cache/redis/listKeys/action | Redis Cache erişim anahtarlarının değerini yönetim portalında görüntüleyin |
> | Eylem | Microsoft.Cache/redis/listUpgradeNotifications/read | Önbellek kiracısı için en son yükseltme bildirimlerini listeleyin. |
> | Eylem | Microsoft.Cache/redis/metricDefinitions/read | Bir Redis Cache için kullanılabilir ölçümleri alır |
> | Eylem | Microsoft.Cache/redis/patchSchedules/delete | Bir Redis Cache'in düzeltme eki zamanlamasını Sil |
> | Eylem | Microsoft.Cache/redis/patchSchedules/read | Bir Redis Cache'in düzeltme eki uygulama zamanlamasını alır |
> | Eylem | Microsoft.Cache/redis/patchSchedules/write | Bir Redis Cache'in düzeltme eki uygulama zamanlamasını Değiştir |
> | Eylem | Microsoft.Cache/redis/read | Redis Cache'nin ayarlarını ve yapılandırmasını yönetim portalında görüntüleyin |
> | Eylem | Microsoft.Cache/redis/regenerateKey/action | Yönetim Portalı'nda Redis Cache erişim anahtarlarının değerini değiştirme |
> | Eylem | Microsoft.Cache/redis/start/action | Bir önbellek örneği başlatın. |
> | Eylem | Microsoft.Cache/redis/stop/action | Bir önbellek örneğini durdurun. |
> | Eylem | Microsoft.Cache/redis/write | Redis Cache'nin ayarlarını ve yapılandırmasını yönetim portalında değiştirin |
> | Eylem | Microsoft.Cache/register/action | 'Microsoft.Cache' kaynak sağlayıcısını bir aboneliğe kaydeder |
> | Eylem | Microsoft.Cache/unregister/action | 'Microsoft.Cache' kaynak sağlayıcısına bir abonelik kaydını siler |

## <a name="microsoftcapacity"></a>Microsoft.Capacity

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Capacity/appliedreservations/read | Tüm ayırmaları okuyun |
> | Eylem | Microsoft.Capacity/calculateexchange/action | Yeni satın alma fiyatı ve exchange miktarını hesaplar ve ilke hataları döndürür. |
> | Eylem | Microsoft.Capacity/calculateprice/action | Tüm rezervasyon fiyat hesaplayın |
> | Eylem | Microsoft.Capacity/catalogs/read | Rezervasyonun oku Kataloğu |
> | Eylem | Microsoft.Capacity/checkoffers/action | Herhangi bir abonelik teklif denetleyin |
> | Eylem | Microsoft.Capacity/checkscopes/action | Herhangi bir abonelik denetleyin |
> | Eylem | Microsoft.Capacity/commercialreservationorders/read | Rezervasyon sıraları herhangi bir Kiracıda oluşturulan Al |
> | Eylem | Microsoft.Capacity/exchange/action | Tüm rezervasyon değişimi |
> | Eylem | Microsoft.Capacity/operations/read | Herhangi bir işlemi okuyun |
> | Eylem | Microsoft.Capacity/register/action | Kapasite kaynak sağlayıcısını kaydeder ve kapasite kaynaklarının oluşturulmasını sağlar. |
> | Eylem | Microsoft.Capacity/reservationorders/action | Tüm rezervasyon güncelleştir |
> | Eylem | Microsoft.Capacity/reservationorders/availablescopes/action | Herhangi bir kullanılabilir kapsamı bulun |
> | Eylem | Microsoft.Capacity/reservationorders/calculaterefund/action | Yeni satın alma fiyatı ve para iadesi miktarı hesaplar ve ilke hataları döndürür. |
> | Eylem | Microsoft.Capacity/reservationorders/delete | Tüm rezervasyon Sil |
> | Eylem | Microsoft.Capacity/reservationorders/merge/action | Tüm rezervasyon Birleştir |
> | Eylem | Microsoft.Capacity/reservationorders/read | Tüm ayırmaları okuyun |
> | Eylem | Microsoft.Capacity/reservationorders/reservations/action | Tüm rezervasyon güncelleştir |
> | Eylem | Microsoft.Capacity/reservationorders/reservations/delete | Tüm rezervasyon Sil |
> | Eylem | Microsoft.Capacity/reservationorders/reservations/read | Tüm ayırmaları okuyun |
> | Eylem | Microsoft.Capacity/reservationorders/reservations/revisions/read | Tüm ayırmaları okuyun |
> | Eylem | Microsoft.Capacity/reservationorders/reservations/write | Herhangi bir ayırma oluştur |
> | Eylem | Microsoft.Capacity/reservationorders/return/action | Tüm rezervasyon döndürür |
> | Eylem | Microsoft.Capacity/reservationorders/split/action | Tüm rezervasyon Böl |
> | Eylem | Microsoft.Capacity/reservationorders/swap/action | Tüm rezervasyon değiştirme |
> | Eylem | Microsoft.Capacity/reservationorders/write | Herhangi bir ayırma oluştur |
> | Eylem | Microsoft.Capacity/tenants/register/action | Tüm Kiracı kaydetme |
> | Eylem | Microsoft.Capacity/unregister/action | Tüm Kiracı kaydı |
> | Eylem | Microsoft.Capacity/validatereservationorder/action | Tüm ayırmasını doğrula |

## <a name="microsoftcdn"></a>Microsoft.Cdn

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
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
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.CertificateRegistration/certificateOrders/certificates/Delete | Mevcut bir sertifikayı Sil |
> | Eylem | Microsoft.CertificateRegistration/certificateOrders/certificates/Read | Sertifikaları'nın listesini alın |
> | Eylem | Microsoft.CertificateRegistration/certificateOrders/certificates/Write | Yeni bir sertifika eklemek veya mevcut bir güncelleştirme |
> | Eylem | Microsoft.CertificateRegistration/certificateOrders/Delete | Mevcut bir AppServiceCertificate Sil |
> | Eylem | Microsoft.CertificateRegistration/certificateOrders/Read | CertificateOrders'ın listesini alın |
> | Eylem | Microsoft.CertificateRegistration/certificateOrders/reissue/Action | Mevcut bir siparişini yeniden gönderme |
> | Eylem | Microsoft.CertificateRegistration/certificateOrders/renew/Action | Mevcut bir sertifika siparişini yenileme |
> | Eylem | Microsoft.CertificateRegistration/certificateOrders/resendEmail/Action | Sertifika. e-posta Gönder |
> | Eylem | Microsoft.CertificateRegistration/certificateOrders/resendRequestEmails/Action | Başka bir e-posta adresi isteği e-postalarını yeniden gönder |
> | Eylem | Microsoft.CertificateRegistration/certificateOrders/resendRequestEmails/Action | Site korumalı için verilen bir App Service sertifikası alma |
> | Eylem | Microsoft.CertificateRegistration/certificateOrders/retrieveCertificateActions/Action | Sertifika eylemlerini alma |
> | Eylem | Microsoft.CertificateRegistration/certificateOrders/retrieveEmailHistory/Action | Sertifika e-posta geçmişini alma |
> | Eylem | Microsoft.CertificateRegistration/certificateOrders/verifyDomainOwnership/Action | Etki alanı sahipliğini doğrulayın |
> | Eylem | Microsoft.CertificateRegistration/certificateOrders/Write | Yeni bir sertifika siparişi eklemek veya mevcut bir güncelleştirme |
> | Eylem | Microsoft.CertificateRegistration/operations/Read | Tüm app service sertifika kayıt işlemlerinden listesi |
> | Eylem | Microsoft.CertificateRegistration/provisionGlobalAppServicePrincipalInUserTenant/Action | Sağlama hizmeti uygulama sorumlusu için hizmet sorumlusu |
> | Eylem | Microsoft.CertificateRegistration/register/action | Abonelik için Microsoft Certificates kaynak sağlayıcısını kaydetme |
> | Eylem | Microsoft.CertificateRegistration/validateCertificateRegistrationInformation/Action | Gönderme olmadan sertifikası satın alma nesne doğrulama |

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.ClassicCompute/capabilities/read | Özellikleri gösterir |
> | Eylem | Microsoft.ClassicCompute/checkDomainNameAvailability/action | Bir etki alanı adının kullanılabilirliğini denetler. |
> | Eylem | Microsoft.ClassicCompute/checkDomainNameAvailability/read | Bir etki alanı adının kullanılabilirliğini alır. |
> | Eylem | Microsoft.ClassicCompute/domainNames/active/write | Etkin etki alanı adını ayarlar. |
> | Eylem | Microsoft.ClassicCompute/domainNames/availabilitySets/read | Kullanılabilirlik kümesi için kaynağı gösterir. |
> | Eylem | Microsoft.ClassicCompute/domainNames/capabilities/read | Etki alanı adı özelliklerini gösterir. |
> | Eylem | Microsoft.ClassicCompute/domainNames/delete | Kaynaklar için etki alanı adlarını kaldırın. |
> | Eylem | Microsoft.ClassicCompute/domainNames/deploymentslots/read | Dağıtım yuvalarını gösterir. |
> | Eylem | Microsoft.ClassicCompute/domainNames/deploymentslots/roles/read | Etki alanı adı dağıtım yuvası rolünü Al |
> | Eylem | Microsoft.ClassicCompute/domainNames/deploymentslots/roles/roleinstances/read | Etki alanı adı dağıtım yuvası rolü için rol örneğini al |
> | Eylem | Microsoft.ClassicCompute/domainNames/deploymentslots/state/read | Dağıtım yuvası durumunu alın. |
> | Eylem | Microsoft.ClassicCompute/domainNames/deploymentslots/state/write | Dağıtım yuvası durumunu ekleyin. |
> | Eylem | Microsoft.ClassicCompute/domainNames/deploymentslots/upgradedomain/read | Etki alanı adı dağıtım yuvası için yükseltme etki alanı Al |
> | Eylem | Microsoft.ClassicCompute/domainNames/deploymentslots/upgradedomain/write | Etki alanı adı dağıtım yuvası için yükseltme etki alanını güncelleştirme |
> | Eylem | Microsoft.ClassicCompute/domainNames/deploymentslots/write | Dağıtım güncelleştirme veya oluşturur. |
> | Eylem | Microsoft.ClassicCompute/domainNames/extensions/delete | Etki alanı adı uzantılarını kaldırın. |
> | Eylem | Microsoft.ClassicCompute/domainNames/extensions/operationStatuses/read | Uzantıları etki alanı için işlem durumunu okur. |
> | Eylem | Microsoft.ClassicCompute/domainNames/extensions/read | Etki alanı adı uzantılarını döndürür. |
> | Eylem | Microsoft.ClassicCompute/domainNames/extensions/write | Etki alanı adı uzantılarını ekleyin. |
> | Eylem | Microsoft.ClassicCompute/domainNames/internalLoadBalancers/delete | Yeni bir iç yük dengelemeyi kaldırın. |
> | Eylem | Microsoft.ClassicCompute/domainNames/internalLoadBalancers/operationStatuses/read | Etki alanı adları iç load balancer'ları için işlem durumunu okur. |
> | Eylem | Microsoft.ClassicCompute/domainNames/internalLoadBalancers/read | İç load balancer'ları alır. |
> | Eylem | Microsoft.ClassicCompute/domainNames/internalLoadBalancers/write | Yeni bir iç Yük Dengeleme oluşturur. |
> | Eylem | Microsoft.ClassicCompute/domainNames/loadBalancedEndpointSets/operationStatuses/read | Etki alanı adları yük dengeli uç nokta kümeleri için işlem durumunu okur. |
> | Eylem | Microsoft.ClassicCompute/domainNames/loadBalancedEndpointSets/read | Yük dengeli uç nokta kümelerini Al |
> | Eylem | Microsoft.ClassicCompute/domainNames/loadBalancedEndpointSets/write | Yük dengeli uç nokta kümesi ekleyin. |
> | Eylem | Microsoft.ClassicCompute/domainNames/operationstatuses/read | Etki alanı adının işlem durumunu Al |
> | Eylem | Microsoft.ClassicCompute/domainNames/operationStatuses/read | Uzantıları etki alanı için işlem durumunu okur. |
> | Eylem | Microsoft.ClassicCompute/domainNames/read | Kaynaklar için etki alanı adlarını döndürün. |
> | Eylem | Microsoft.ClassicCompute/domainNames/serviceCertificates/delete | Kullanılan hizmet sertifikalarını silin. |
> | Eylem | Microsoft.ClassicCompute/domainNames/serviceCertificates/operationStatuses/read | Adları hizmet sertifikaları etki alanı için işlem durumunu okur. |
> | Eylem | Microsoft.ClassicCompute/domainNames/serviceCertificates/read | Kullanılan hizmet sertifikalarını döndürür. |
> | Eylem | Microsoft.ClassicCompute/domainNames/serviceCertificates/write | Ekleme veya kullanılan hizmet sertifikalarını değiştirme. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/abortMigration/action | Dağıtım yuvası geçişini durdurur. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/commitMigration/action | Dağıtım yuvası geçişini yürütür. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/delete | Bir dağıtım yuvasını siler. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/operationStatuses/read | Adı yuvaları etki alanı için işlem durumunu okur. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/prepareMigration/action | Dağıtım yuvası geçişini hazırlar. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/read | Dağıtım yuvalarını gösterir. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/roles/extensionReferences/delete | Dağıtım yuvası rolüne ait uzantı başvurusunu kaldırın. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/roles/extensionReferences/operationStatuses/read | Etki alanı adı yuvaları uzantı başvuruları için işlem durumunu okur. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/roles/extensionReferences/read | Dağıtım yuvası rolüne ait uzantı başvurusunu döndürür. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/roles/extensionReferences/write | Ekleyin veya dağıtım yuvası rolüne ait uzantı başvurusunu değiştirin. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/roles/metricdefinitions/read | Etki alanı adı için rol ölçüm tanımını Al |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/roles/metrics/read | Rol ölçüm etki alanı adını alın. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/roles/operationstatuses/read | Etki alanı adı yuvası rolü için işlem durumunu Al |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/roles/providers/Microsoft.Insights/diagnosticSettings/read | Tanılama ayarlarını alın. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/roles/providers/Microsoft.Insights/diagnosticSettings/write | Ekleyin veya tanılama ayarları değiştirin. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/roles/providers/Microsoft.Insights/metricDefinitions/read | Ölçüm tanımlarını alır. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/roles/read | Dağıtım yuvası için rolü alın. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/downloadremotedesktopconnectionfile/action | Etki alanı adı yuvası rolü üzerindeki rol örneği için Uzak Masaüstü bağlantı dosyası indirir. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/operationStatuses/read | Etki alanı adı yuvası rolü üzerindeki rol örneği için işlem durumunu alır. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/read | Rol örneğini alın. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/rebuild/action | Rol örneğini yeniden derler. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/reimage/action | Rol örneği görüntüsünü yeniden oluşturur. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/restart/action | Rol örneklerini yeniden başlatır. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/roles/skus/read | İçin dağıtım yuvası rol SKU'sunda alın. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/roles/write | Dağıtım yuvası için rolü ekleyin. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/start/action | Bir dağıtım yuvası başlatır. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/state/start/write | Dağıtım yuvası durumunu durduruldu olarak değiştirir. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/state/stop/write | Dağıtım yuvası durumunu Başlatıldı olarak değiştirir. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/stop/action | Dağıtım yuvasını askıya alır. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/upgradeDomain/write | Etki alanını yükseltir. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/validateMigration/action | Dağıtım yuvası geçişini doğrular. |
> | Eylem | Microsoft.ClassicCompute/domainNames/slots/write | Dağıtım güncelleştirme veya oluşturur. |
> | Eylem | Microsoft.ClassicCompute/domainNames/swap/action | Hazırlama yuvasını üretim yuvasıyla değiştirir. |
> | Eylem | Microsoft.ClassicCompute/domainNames/write | Ekleyin veya kaynaklar için etki alanı adlarını değiştirin. |
> | Eylem | Microsoft.ClassicCompute/moveSubscriptionResources/action | Tüm Klasik kaynakları farklı bir aboneliğe taşıyın. |
> | Eylem | Microsoft.ClassicCompute/operatingSystemFamilies/read | Microsoft Azure'da kullanılabilen konuk işletim sistemi ailelerini listeler ve ayrıca her aile için kullanılabilen işletim sistemi sürümlerini listeler. |
> | Eylem | Microsoft.ClassicCompute/operatingSystems/read | Şu anda Microsoft Azure'da kullanılabilen konuk işletim sistemi sürümlerini listeler. |
> | Eylem | Microsoft.ClassicCompute/operations/read | İşlemlerin listesini alır. |
> | Eylem | Microsoft.ClassicCompute/operationStatuses/read | Kaynak için işlem durumunu okur. |
> | Eylem | Microsoft.ClassicCompute/quotas/read | Abonelik için kotayı alın. |
> | Eylem | Microsoft.ClassicCompute/register/action | Classic Compute'a Kaydol |
> | Eylem | Microsoft.ClassicCompute/resourceTypes/skus/read | Desteklenen kaynak türleri için Sku listesini alır. |
> | Eylem | Microsoft.ClassicCompute/validateSubscriptionMoveAvailability/action | Klasik taşıma işlemi için aboneliğin uygunluk durumunu doğrulayın. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/associatedNetworkSecurityGroups/delete | Sanal makineyle ilişkilendirilmiş ağ güvenlik grubunu siler. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/associatedNetworkSecurityGroups/operationStatuses/read | Ağ güvenlik gruplarıyla ilişkili sanal makineler için işlem durumunu okur. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/associatedNetworkSecurityGroups/read | Sanal makineyle ilişkilendirilmiş ağ güvenlik grubunu alır. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/associatedNetworkSecurityGroups/write | Sanal makineyle ilişkilendirilmiş ağ güvenlik grubu ekler. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/asyncOperations/read | Olası zaman uyumsuz işlemleri alır |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/attachDisk/action | Bir sanal makineye veri diski ekler. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/capture/action | Bir sanal makine yakalama. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/delete | Sanal makineleri kaldırır. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/detachDisk/action | Veri diski sanal makineden ayırır. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/diagnosticsettings/read | Sanal makinenin tanılama ayarlarını alın. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/disks/read | Veri disklerinin listesini alır. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/downloadRemoteDesktopConnectionFile/action | Sanal makine için RDP dosyasını indirir. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/extensions/operationStatuses/read | Sanal makine uzantıları için işlem durumunu okur. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/extensions/read | Sanal makine uzantısını alır. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/extensions/write | Sanal makine uzantısını yerleştirir. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/metricdefinitions/read | Sanal makine ölçüm tanımını Al. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/metrics/read | Ölçümleri alır. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/delete | Ağ arabirimiyle ilişkilendirilmiş ağ güvenlik grubunu siler. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/operationStatuses/read | Ağ güvenlik gruplarıyla ilişkili sanal makineler için işlem durumunu okur. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/read | Ağ arabirimiyle ilişkilendirilmiş ağ güvenlik grubunu alır. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/write | Ağ arabirimiyle ilişkilendirilmiş ağ güvenlik grubu ekler. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/operationStatuses/read | Sanal makineler için işlem durumunu okur. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/performMaintenance/action | Sanal makinede bakım gerçekleştirir. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/providers/Microsoft.Insights/diagnosticSettings/read | Tanılama ayarlarını alın. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/providers/Microsoft.Insights/diagnosticSettings/write | Ekleyin veya tanılama ayarları değiştirin. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/providers/Microsoft.Insights/metricDefinitions/read | Ölçüm tanımlarını alır. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/read | Sanal makinelerin listesini alır. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/redeploy/action | Sanal makineyi yeniden dağıtır. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/restart/action | Sanal makineleri yeniden başlatır. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/shutdown/action | Sanal makineyi kapatın. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/start/action | Sanal makineyi başlatın. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/stop/action | Sanal makineyi durdurur. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/write | Ekleyin veya sanal makineleri değiştirin. |

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.ClassicNetwork/expressroutecrossconnections/operationstatuses/read | Bir expressroute çapraz bağlantı işlem durumunu Al |
> | Eylem | Microsoft.ClassicNetwork/expressroutecrossconnections/peerings/delete | Hızlı Rota bağlantısı eşlemesi çapraz silin. |
> | Eylem | Microsoft.ClassicNetwork/expressroutecrossconnections/peerings/operationstatuses/read | Bir expressroute bağlantı eşleme işlemi durumu çapraz alın. |
> | Eylem | Microsoft.ClassicNetwork/expressroutecrossconnections/peerings/read | Hızlı Rota bağlantısı eşlemesi çapraz alın. |
> | Eylem | Microsoft.ClassicNetwork/expressroutecrossconnections/peerings/write | Hızlı Rota bağlantısı eşlemesi çapraz ekleyin. |
> | Eylem | Microsoft.ClassicNetwork/expressroutecrossconnections/read | Alma Express route bağlantıları. |
> | Eylem | Microsoft.ClassicNetwork/expressroutecrossconnections/write | Express route ekleme çapraz bağlantıları. |
> | Eylem | Microsoft.ClassicNetwork/gatewaySupportedDevices/read | Desteklenen cihazların listesini alır. |
> | Eylem | Microsoft.ClassicNetwork/networkSecurityGroups/delete | Ağ güvenlik grubunu siler. |
> | Eylem | Microsoft.ClassicNetwork/networkSecurityGroups/operationStatuses/read | Ağ güvenlik grubu için işlem durumunu okur. |
> | Eylem | Microsoft.ClassicNetwork/networksecuritygroups/providers/Microsoft.Insights/diagnosticSettings/read | Ağ güvenlik grubu tanılama ayarlarını alır. |
> | Eylem | Microsoft.ClassicNetwork/networksecuritygroups/providers/Microsoft.Insights/diagnosticSettings/write | Oluşturur veya ağ güvenlik grupları tanılama ayarları, bu işlem Öngörüler kaynak sağlayıcısı tarafından takıma güncelleştirir. |
> | Eylem | Microsoft.ClassicNetwork/networksecuritygroups/providers/Microsoft.Insights/logDefinitions/read | Ağ güvenlik grubunun olaylarını alır |
> | Eylem | Microsoft.ClassicNetwork/networkSecurityGroups/read | Ağ güvenlik grubunu alır. |
> | Eylem | Microsoft.ClassicNetwork/networkSecurityGroups/securityRules/delete | Güvenlik kuralını siler. |
> | Eylem | Microsoft.ClassicNetwork/networkSecurityGroups/securityRules/operationStatuses/read | Ağ güvenlik grubu güvenlik kuralları işlem durumunu okur. |
> | Eylem | Microsoft.ClassicNetwork/networkSecurityGroups/securityRules/read | Güvenlik kuralını alır. |
> | Eylem | Microsoft.ClassicNetwork/networkSecurityGroups/securityRules/write | Ekler veya bir güvenlik kuralı güncelleştirin. |
> | Eylem | Microsoft.ClassicNetwork/networkSecurityGroups/write | Yeni bir ağ güvenlik grubu ekler. |
> | Eylem | Microsoft.ClassicNetwork/operations/read | Klasik ağ işlemleri Al. |
> | Eylem | Microsoft.ClassicNetwork/quotas/read | Abonelik için kotayı alın. |
> | Eylem | Microsoft.ClassicNetwork/register/action | Classic Network'e Kaydol |
> | Eylem | Microsoft.ClassicNetwork/reservedIps/delete | Ayrılmış bir IP'yi silin. |
> | Eylem | Microsoft.ClassicNetwork/reservedIps/join/action | Ayrılmış bir IP'ye katılın |
> | Eylem | Microsoft.ClassicNetwork/reservedIps/link/action | Ayrılmış bir IP'ye bağlantı |
> | Eylem | Microsoft.ClassicNetwork/reservedIps/operationStatuses/read | Ayrılmış IP'ler için işlem durumunu okur. |
> | Eylem | Microsoft.ClassicNetwork/reservedIps/read | Ayrılmış IP'leri alır |
> | Eylem | Microsoft.ClassicNetwork/reservedIps/write | Yeni bir ayrılmış IP Ekle |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/abortMigration/action | Bir sanal ağın geçişini durdurur |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/capabilities/read | Özellikleri gösterir |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/checkIPAddressAvailability/action | Bir sanal ağdaki belirli bir IP adresinin kullanılabilirliğini denetler. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/commitMigration/action | Bir sanal ağın geçişini yürütür |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/delete | Sanal ağı siler. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRevokedCertificates/delete | Bir istemci sertifikası iptalini geri alır. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRevokedCertificates/read | İptal edilen istemci sertifikalarını okuyun. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRevokedCertificates/write | Bir istemci sertifikasını iptal eder. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates/delete | Sanal ağ geçidi istemci sertifikasını siler. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates/download/action | Parmak izine göre sertifika indirir. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates/listPackage/action | Sanal ağ geçidi sertifika paketini listeler. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates/read | İstemci kök sertifikalarını bulun. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates/write | Yeni bir istemci kök sertifikasını karşıya yükler. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/gateways/connections/connect/action | Siteden siteye ağ geçidi bağlantısına bağlanır. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/gateways/connections/disconnect/action | Siteden siteye ağ geçidi bağlantısını keser. |
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
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/gateways/startDiagnostics/action | Sanal ağ geçidi için tanılama başlatır. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/gateways/stopDiagnostics/action | Sanal ağ geçidi için tanılama durdurur. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/gateways/write | Bir sanal ağ geçidi ekler. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/join/action | Sanal ağa katılır. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/operationStatuses/read | Sanal ağlar için işlem durumunu okur. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/peer/action | Bir sanal ağ başka bir sanal ağ ile eşler. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/prepareMigration/action | Bir sanal ağın geçişini hazırlar |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/read | Sanal ağı alın. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/remoteVirtualNetworkPeeringProxies/delete | Uzak sanal ağ eşleme Ara sunucusunu siler. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/remoteVirtualNetworkPeeringProxies/read | Uzak sanal ağ eşleme Ara sunucusunu alır. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/remoteVirtualNetworkPeeringProxies/write | Ekler veya uzak sanal ağ eşleme Ara sunucusunu güncelleştirir. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/subnets/associatedNetworkSecurityGroups/delete | Alt ağla ilişkili ağ güvenlik grubunu siler. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/subnets/associatedNetworkSecurityGroups/operationStatuses/read | Sanal ağ alt ağı ilişkili ağ güvenlik grubu için işlem durumunu okur. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/subnets/associatedNetworkSecurityGroups/read | Alt ağla ilişkili ağ güvenlik grubunu alır. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/subnets/associatedNetworkSecurityGroups/write | Alt ağ ile ilişkilendirilmiş ağ güvenlik grubu ekler. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/validateMigration/action | Bir sanal ağın geçişini doğrular |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/virtualNetworkPeerings/read | Sanal ağ eşlemesini alır. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/write | Yeni bir sanal ağa ekleyin. |

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.ClassicStorage/capabilities/read | Özellikleri gösterir |
> | Eylem | Microsoft.ClassicStorage/checkStorageAccountAvailability/action | Bir depolama hesabı kullanılabilirliğini denetler. |
> | Eylem | Microsoft.ClassicStorage/checkStorageAccountAvailability/read | Bir depolama hesabı kullanılabilirliğini Al. |
> | Eylem | Microsoft.ClassicStorage/disks/read | Depolama hesabı diskini döndürür. |
> | Eylem | Microsoft.ClassicStorage/images/operationstatuses/read | Görüntü işlemi durumlarını alır. |
> | Eylem | Microsoft.ClassicStorage/images/read | Görüntüyü döndürür. |
> | Eylem | Microsoft.ClassicStorage/operations/read | Klasik depolama işlemleri alır |
> | Eylem | Microsoft.ClassicStorage/osImages/read | İşletim sistemi görüntüsünü döndürür. |
> | Eylem | Microsoft.ClassicStorage/osPlatformImages/read | İşletim sistemi platformu görüntüsünü alır. |
> | Eylem | Microsoft.ClassicStorage/publicImages/read | Genel sanal makine görüntüsünü alır. |
> | Eylem | Microsoft.ClassicStorage/quotas/read | Abonelik için kotayı alın. |
> | Eylem | Microsoft.ClassicStorage/register/action | Klasik depolamaya kaydeder |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/abortMigration/action | Depolama hesabını geçirme işlemini durdurur. |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/commitMigration/action | Depolama hesabını geçirme işlemini yürütür. |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/delete | Depolama hesabını silin. |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/disks/delete | Verilen depolama hesabı diskini siler. |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/disks/operationStatuses/read | Kaynak için işlem durumunu okur. |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/disks/read | Depolama hesabı diskini döndürür. |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/disks/write | Depolama hesabı diski ekler. |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/images/delete | Belirli bir depolama hesabı görüntüsünü siler. (Kullanım dışı. 'Microsoft.ClassicStorage/storageAccounts/vmImages' kullanın) |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/images/operationstatuses/read | Depolama hesabı görüntüsünü işlem durumunu döndürür. |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/images/read | Depolama hesabı görüntüsünü döndürür. (Kullanım dışı. 'Microsoft.ClassicStorage/storageAccounts/vmImages' kullanın) |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/listKeys/action | Depolama hesaplarının erişim anahtarlarını listeler. |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/operationStatuses/read | Kaynak için işlem durumunu okur. |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/osImages/delete | Belirli bir depolama hesabı işletim sistemi görüntüsünü siler. |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/osImages/read | Depolama hesabı işletim sistemi görüntüsünü döndürür. |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/osImages/write | Belirli bir depolama hesabı işletim sistemi görüntüsünü ekler. |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/prepareMigration/action | Depolama hesabını geçirme işlemini hazırlar. |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/read | Belirli bir hesaba depolama hesabını döndürün. |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/regenerateKey/action | Mevcut depolama hesabının erişim anahtarlarını yeniden oluşturur. |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/services/diagnosticSettings/read | Tanılama ayarlarını alın. |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/services/diagnosticSettings/write | Ekleyin veya tanılama ayarları değiştirin. |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/services/metricDefinitions/read | Ölçüm tanımlarını alır. |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/services/metrics/read | Ölçümleri alır. |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/services/read | Kullanılabilir hizmetleri alın. |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/validateMigration/action | Depolama hesabını geçirme işlemini doğrular. |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/vmImages/delete | Belirli bir sanal makine görüntüsünü siler. |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/vmImages/operationstatuses/read | Belirtilen sanal makine görüntüsü işlemi durumlarını alır. |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/vmImages/read | Sanal makine görüntüsünü döndürür. |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/vmImages/write | Belirli bir sanal makine görüntüsünü ekler. |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/write | Yeni bir depolama hesabı ekler. |
> | Eylem | Microsoft.ClassicStorage/vmImages/read | Sanal makine görüntülerini listeler. |

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | DataAction | Microsoft.CognitiveServices/accounts/ComputerVision/analyze/action | Bu işlem, zengin görsel özellikleri görüntüsü içeriğine göre ayıklar.  |
> | DataAction | Microsoft.CognitiveServices/accounts/ComputerVision/areaofinterest/action | Bu işlem, resmin en önemli alanın çevresinde bir sınırlayıcı kutu döndürür. |
> | DataAction | Microsoft.CognitiveServices/accounts/ComputerVision/describe/action | Bu işlem, tam cümle ile insan tarafından okunabilir dilde görüntü açıklamasını oluşturur.<br> Açıklama da işlem tarafından döndürülen içerik etiket koleksiyonu temel alır.<br>Birden fazla açıklama için her görüntü oluşturulabilir.<br> Açıklamalar, kendi güvenilirlik puanı göre sıralanır.<br>Tüm açıklamaları İngilizce'dir. |
> | DataAction | Microsoft.CognitiveServices/accounts/ComputerVision/detect/action | Bu işlemi gerçekleştirir nesne algılama belirtilen görüntü üzerinde.  |
> | DataAction | Microsoft.CognitiveServices/accounts/ComputerVision/generatethumbnail/action | Bu işlem, kullanıcı tarafından belirtilen genişlik ve yükseklik bir küçük resim görüntüsü oluşturur.<br> Varsayılan olarak, hizmet görüntüyü inceler, (ROI) ilgi bölgesi tanımlar ve yatırım Getirisi üzerinde dayalı akıllı kırpma koordinatları oluşturur.<br> Akıllı kırpma, girdi görüntüsünün farklı bir en boy oranını belirttiğinizde yardımcı olur |
> | DataAction | Microsoft.CognitiveServices/accounts/ComputerVision/models/analyze/action | Bu işlem, bir etki alanına özgü modeli uygulayarak bir görüntü içindeki içeriği tanır.<br> Görüntü işleme API'si tarafından desteklenen özel etki alanı modelleri listesinde /models GET isteği kullanılarak alınabilir.<br> Şu anda aşağıdaki alana özgü modeller API'nin sağladığı: ünlüleri, yer işareti. |
> | DataAction | Microsoft.CognitiveServices/accounts/ComputerVision/models/read | Bu işlem, alana özgü modeller, görüntü işleme API'si tarafından desteklenen listesini döndürür.  Şu anda API destekler alana özgü modeller aşağıdaki: ünlü tanıyıcı, yer işareti tanıyıcı. |
> | DataAction | Microsoft.CognitiveServices/accounts/ComputerVision/ocr/action | Optik karakter tanıma (OCR), bir resimdeki metni algılar ve tanınan karakterleri makine tarafından kullanılabilir bir karakter akışı halinde ayıklar.    |
> | DataAction | Microsoft.CognitiveServices/accounts/ComputerVision/recognizetext/action | Bu arabirim, metin tanıma işleminin sonucu almak için kullanın. Metni Tanı arabirimini kullandığınızda, yanıt "İşlemi-konum" adlı bir alan içeriyor. "İşlem-Location" alan tanımak metin işleminin sonucunu Al işlemi için kullanmanız gereken URL'yi içerir. |
> | DataAction | Microsoft.CognitiveServices/accounts/ComputerVision/tag/action | Bu işlem, sözcük veya sağlanan görüntü içeriğini ilgili etiketler listesi oluşturur.<br>Görüntü işleme API'si etiketleri, nesnelere bağlı, canlı, manzara veya resimleri bulunan Eylemler yaşayan döndürebilir.<br>Kategoriler, farklı etiketler bir hiyerarşik sınıflandırma sistemi göre düzenlenmiş, ancak görüntü içeriğini karşılık gelir.<br>Belirsizliği engellemek ya da bağlam sağlamak için ipuçları etiketler içerebilir, örneğin "cello" etiket "Müzik Aleti" İpucu tarafından eşlik edebilir.<br>Tüm etiketleri İngilizce'dir. |
> | DataAction | Microsoft.CognitiveServices/accounts/ComputerVision/textoperations/read | Bu arabirim almak için kullanılan metin işlem sonucu tanıyacak. Bu arabirim URL'sini nereden alınacağını <b>"İşlemi-Location"</b> alan metni tanı arabiriminden döndürdü. |
> | Eylem | Microsoft.CognitiveServices/accounts/delete | API hesaplarını siler |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/detect/action | Dikdörtgenler dönüş yüz tanıma, görüntü ve isteğe bağlı olarak faceIds, yer işareti ve öznitelikleri ile İnsan yüzlerini algılayın. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/facelists/delete | Belirtilen yüz listesini silin. Yüz tanıma listesinde ilgili yüz görüntüleri, çok silinir. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/facelists/persistedfaces/delete | Bir yüz tanıma, belirtilen faceListId ve persisitedFaceId tarafından bir yüz listeden silin. İlgili yüz resmini, çok silinir. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/facelists/persistedfaces/write | Bir yüz tanıma, 1000 yüz kadar belirtilen yüz listesine ekleyin. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/facelists/read | Yüz tanıma listenin faceListId, adı, userData ve yüz listesinde yüzleri alma. Yüz listelerini faceListId, adı ve userData listeleyin. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/facelists/write | Kullanıcı tarafından belirtilen faceListId, ad ve isteğe bağlı bir userData boş yüz listesi oluşturun. En fazla 64 yüz listelerini güncelleştirme bilgilerini adı ve userData dahil olmak üzere yüz listesinin izin verilir. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/findsimilars/action | Sorgu yüzünün Faceıd, Faceıd diziden, yüz tanıma listesini veya büyük yüz listesini birbirine benzeyen yüzleri aramak için verilir. Faceıd |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/group/action | Yüz tanıma benzerliğe bağlı olarak gruplar aday yüzleri bölün. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/identify/action | bir kişi grubu veya büyük kişi grubu belirli sorgu kişi yazıtipinin değerini en yakın eşleşme bulmak için 1-çok Kimliği'ni kullanın. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largefacelists/delete | Belirtilen büyük yüz listesini silin. İlgili yüz görüntüler büyük yüz listesinde, çok silinecektir. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largefacelists/persistedfaces/delete | Bir yüz tanıma, belirtilen largeFaceListId persisitedFaceId ile büyük yüz listeden silin. İlgili yüz resmini, çok silinir. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largefacelists/persistedfaces/read | Büyük yüz listesinde kalıcı yüz persistedFaceId largeFaceListId ile alın. Yüzleri persistedFaceId ve belirtilen büyük yüz listesinde userData listeleyin. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largefacelists/persistedfaces/write | Bir yüz, en fazla 1.000.000 yüzleri bir belirtilen büyük yüz listesine ekleyin. Büyük yüz listesinde belirtilen yüzünün userData alanı tarafından kendi persistedFaceId güncelleştirin. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largefacelists/read | Büyük yüz listenin largeFaceListId, adı, userData alın. LargeFaceListId, adı ve userData büyük yüz listelerini bilgilerini listeler. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largefacelists/train/action | Büyük yüz listesi eğitim görevi gönderin. Eğitim yalnızca eğitilen büyük yüz listesini kullanabileceğiniz bir adımdır. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largefacelists/training/read | Tamamlanmış veya hala devam eden büyük yüz listesi eğitim durumunu denetlemek için. LargeFaceList eğitim zaman uyumsuz bir işlemdir |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largefacelists/write | Kullanıcı tarafından belirtilen largeFaceListId, ad ve isteğe bağlı bir userData boş büyük yüz listesi oluşturun. Ad ve userData dahil olmak üzere büyük yüz listesinin bilgileri güncelleştirin. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/delete | Belirtilen personGroupId ile var olan bir büyük kişi grubunu silin. Bu büyük kişi grubu kalıcı verileri silinecek. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/persons/action | Yeni bir kişi, içinde bir belirtilen büyük kişi grubu oluşturun. Yüz tanıma bu kişiye eklemek için lütfen arayın |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/persons/delete | Var olan bir kişi bir büyük kişi grubundan silin. Tüm kişi verilerini depolanır ve kişi giriş yüz görüntüler silinecektir. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/persons/persistedfaces/delete | Bir yüz kişinin büyük kişi grubu silin. Yüz tanıma verileri ve bu yüz girişle ilgili görüntü de silinir. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/persons/persistedfaces/read | Kişi yüz bilgi alın. Kişi kalıcı yüz tanıma, kendi largePersonGroupId, Personıd ve persistedFaceId tarafından belirtilir. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/persons/persistedfaces/write | Yüz resmini bir kişiye, yüz tanıma veya doğrulama için bir büyük kişi gruba ekleyin. Bir kişi güncelleştirme görüntüsü ile baş yüzünün userData alan kalıcı. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/persons/read | Bir kişinin adını ve userData kayıtlı kişinin resmini temsil eden kalıcı faceIds alın. Personıd, adı, userData ve persistedFaceIds dahil olmak üzere, belirtilen büyük kişi grubundaki tüm kişi bilgilerini listeler. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/persons/write | Adı veya bir kişinin userData güncelleştirin. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/read | Adına ve userData dahil olmak üzere, bir büyük kişi grubu bilgilerini alın. Bu API döndürür büyük kişi grubu bilgilerini tüm var olan büyük kişi grupları'nın largePesonGroupId, adı ve userData. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/train/action | Büyük kişi grubu eğitim görev gönderin. Eğitim eğitilen büyük kişi grubu yalnızca bir kullanabileceğiniz bir adımdır. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/training/read | Tamamlanmış veya hala devam eden büyük kişi grubu eğitim durumunu denetlemek için. LargePersonGroup eğitim zaman uyumsuz bir işlemdir |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/write | Kullanıcı tarafından belirtilen largePersonGroupId, ad ve isteğe bağlı userData ile yeni bir büyük kişi grubu oluşturun. Mevcut bir büyük kişi grubun adı ve userData güncelleştirin. İstek gövdesinde olmadıkları özellikleri değişmeden kullanmaya devam edin. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/persongroups/delete | Belirtilen personGroupId ile var olan bir kişi grubu silin. Bu kişi grubu kalıcı verileri silinecek. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/persongroups/persons/action | Belirtilen kişinin grubunda yeni bir kişi oluşturun. Yüz tanıma bu kişiye eklemek için lütfen arayın |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/persongroups/persons/delete | Var olan bir kişi bir kişi grubundan silin. Tüm kişi verilerini depolanır ve kişi giriş yüz görüntüler silinecektir. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/persongroups/persons/persistedfaces/delete | Bir yüz kişinin bir kişi grubu silin. Yüz tanıma verileri ve bu yüz girişle ilgili görüntü de silinir. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/persongroups/persons/persistedfaces/read | Kişi yüz bilgi alın. Kişi kalıcı yüz tanıma, kendi personGroupId, Personıd ve persistedFaceId tarafından belirtilir. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/persongroups/persons/persistedfaces/write | Yüz resmini bir kişiye, yüz tanıma veya doğrulama için bir kişi gruba ekleyin. Birden çok güncelleştirme bir kişi görüntüsü ile baş yüzünün userData alan kalıcı. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/persongroups/persons/read | Bir kişinin adını ve userData kayıtlı kişinin resmini temsil eden kalıcı faceIds alın. Personıd, adı, userData ve, persistedFaceIds dahil olmak üzere, belirtilen kişinin grubundaki tüm kişileri bilgi listesinde kayıtlı. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/persongroups/persons/write | Adı veya bir kişinin userData güncelleştirin. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/persongroups/read | Kişi grubu adı ve userData alın. Bu personGroup altında kişi bilgilerini almak için listesi kişi grupları'nın pesonGroupId, adı ve userData kullanın. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/persongroups/train/action | Bir kişi grubu eğitim görev gönderin. Eğitim eğitilen kişi grubu yalnızca bir kullanabileceğiniz bir adımdır. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/persongroups/training/read | Tamamlanmış veya hala devam eden kişi grubu eğitim durumunu denetlemek için. PersonGroup eğitim tetiklenen zaman uyumsuz bir işlem olduğu |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/persongroups/write | Belirtilen personGroupId, adı ve kullanıcı tarafından sağlanan userData ile yeni bir kişi grubu oluşturun. Var olan bir kişi grubun adı ve userData güncelleştirin. İstek gövdesinde olmadıkları özellikleri değişmeden kullanmaya devam edin. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/verify/action | İki yüzün aynı kişiye mi ait ya da bir yüz bir kişiye ait olup doğrulayın. |
> | Eylem | Microsoft.CognitiveServices/accounts/listKeys/action | Anahtarları Listele |
> | DataAction | Microsoft.CognitiveServices/accounts/LUIS/predict/action | Belirtilen sorgu için yayımlanan uç nokta tahmin alır. |
> | Eylem | Microsoft.CognitiveServices/accounts/read | API hesaplarını okur. |
> | Eylem | Microsoft.CognitiveServices/accounts/regenerateKey/action | Anahtarı yeniden oluştur |
> | Eylem | Microsoft.CognitiveServices/accounts/skus/read | Mevcut kaynağa ait kullanılabilen SKU'ları okur. |
> | DataAction | Microsoft.CognitiveServices/accounts/TextAnalytics/entities/action | API bilinen varlıkları ve varlıklar adlı genel bir listesini döndürür (\"kişi\", \"konumu\", \"kuruluş\" vb.) belirli bir belgede. |
> | DataAction | Microsoft.CognitiveServices/accounts/TextAnalytics/keyphrases/action | API, giriş metnindeki başlıca konuşma noktalarını gösteren bir dize listesi döndürür. |
> | DataAction | Microsoft.CognitiveServices/accounts/TextAnalytics/languages/action | API, algılanan dile ek olarak 0 ile 1 arasında bir sayısal puan döndürür. Puanın 1’e yakın olması, tanımlanan dilin %100 olasılıkla doğru olduğunu gösterir. Toplamda 120 dil desteklenir. |
> | DataAction | Microsoft.CognitiveServices/accounts/TextAnalytics/sentiment/action | API, 0 ile 1 arasında bir sayısal puan döndürür.<br>A 0 yakın olması ise olumsuz olduğunu gösterir ancak 1'e yakın puanlar pozitif yaklaşımı gösterir.<br>Bir puan 0,5 yaklaşım eksikliği (örn. gösterir<br>bir otomatik simgesi deyimi). |
> | Eylem | Microsoft.CognitiveServices/accounts/usages/read | Mevcut bir kaynak için kota kullanımını alın. |
> | Eylem | Microsoft.CognitiveServices/accounts/write | API hesaplarını yazar. |
> | Eylem | Microsoft.CognitiveServices/locations/checkSkuAvailability/action | Bir abonelik için kullanılabilir SKU'ları okur. |
> | Eylem | Microsoft.CognitiveServices/Operations/read | Tüm kullanılabilir işlemleri listeleyin |
> | Eylem | Microsoft.CognitiveServices/register/action | Bilişsel hizmetler için aboneliği kaydeder |

## <a name="microsoftcommerce"></a>Microsoft.Commerce

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Commerce/RateCard/read | Döndürür, veri, kaynak/ölçüm meta verileri ve belirtilen abonelik için hızları sunar. |
> | Eylem | Microsoft.Commerce/UsageAggregates/read | Microsoft Azure'nın tüketim aboneliğe göre alır. Toplamaları sonuç içeren kullanım verilerini, abonelik ve kaynak ilgili bilgi için belirli bir zaman aralığı. |

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Compute/availabilitySets/delete | Kullanılabilirlik kümesini siler |
> | Eylem | Microsoft.Compute/availabilitySets/read | Bir kullanılabilirlik kümesinin özelliklerini al |
> | Eylem | Microsoft.Compute/availabilitySets/vmSizes/read | Oluşturulurken veya güncelleştirilirken bir kullanılabilirlik kümesindeki sanal makine için kullanılabilir boyutları Listele |
> | Eylem | Microsoft.Compute/availabilitySets/write | Yeni bir kullanılabilirlik kümesi oluşturur veya mevcut olanı güncelleştirir |
> | Eylem | Microsoft.Compute/disks/beginGetAccess/action | Blob erişimi için diskin SAS URI'sini Al |
> | Eylem | Microsoft.Compute/disks/delete | Diski siler |
> | Eylem | Microsoft.Compute/disks/endGetAccess/action | Diskin SAS URI'sini iptal et |
> | Eylem | Microsoft.Compute/disks/read | Bir diskin özelliklerini alır |
> | Eylem | Microsoft.Compute/disks/write | Yeni bir Disk oluşturur veya mevcut olanı güncelleştirir |
> | Eylem | Microsoft.Compute/galleries/delete | Galeri siler |
> | Eylem | Microsoft.Compute/galleries/images/delete | Galeri görüntüyü siler |
> | Eylem | Microsoft.Compute/galleries/images/read | Galeri görüntüsü özelliklerini alır |
> | Eylem | Microsoft.Compute/galleries/images/versions/delete | Galeri görüntüsü sürümü siler |
> | Eylem | Microsoft.Compute/galleries/images/versions/read | Galeri görüntüsü sürümü özelliklerini alır |
> | Eylem | Microsoft.Compute/galleries/images/versions/write | Yeni bir galeri görüntüsü sürüm oluşturur veya mevcut olanı güncelleştirir |
> | Eylem | Microsoft.Compute/galleries/images/write | Yeni bir galeri görüntüsü oluşturur veya mevcut olanı güncelleştirir |
> | Eylem | Microsoft.Compute/galleries/read | Galerinin özelliklerini alır |
> | Eylem | Microsoft.Compute/galleries/write | Yeni bir galeri oluşturur veya mevcut olanı güncelleştirir |
> | Eylem | Microsoft.Compute/hostGroups/delete | Konak grubunu siler |
> | Eylem | Microsoft.Compute/hostGroups/read | Bir konak grubu özelliklerini alma |
> | Eylem | Microsoft.Compute/hostGroups/write | Yeni bir konak grubu oluşturur veya mevcut bir konak grubu güncelleştirir |
> | Eylem | Microsoft.Compute/hosts/delete | Konak siler |
> | Eylem | Microsoft.Compute/hosts/read | Bir konak özelliklerini alma |
> | Eylem | Microsoft.Compute/hosts/write | Yeni bir ana bilgisayar oluşturur veya mevcut bir konağı güncelleştirir |
> | Eylem | Microsoft.Compute/images/delete | Görüntüyü siler |
> | Eylem | Microsoft.Compute/images/read | Görüntü özelliklerini alır |
> | Eylem | Microsoft.Compute/images/write | Yeni bir görüntü oluşturur veya mevcut olanı güncelleştirir |
> | Eylem | Microsoft.Compute/locations/capsOperations/read | Zaman uyumsuz bir Caps işleminin durumunu alır |
> | Eylem | Microsoft.Compute/locations/diskOperations/read | Zaman uyumsuz bir Disk işlemin durumunu alır |
> | Eylem | Microsoft.Compute/locations/logAnalytics/getRequestRateByInterval/action | Günlükleri toplam istek azaltma tanılama yardımcı olmak için zaman aralığına göre göstermek için oluşturabilir. |
> | Eylem | Microsoft.Compute/locations/logAnalytics/getThrottledRequests/action | Günlükleri daraltılmış istekler ResourceName, OperationName veya uygulanan kısıtlama ilkesi tarafından gruplandırılmış toplamları göstermek için oluşturabilir. |
> | Eylem | Microsoft.Compute/locations/operations/read | Zaman uyumsuz bir işlemin durumunu alır |
> | Eylem | Microsoft.Compute/locations/publishers/artifacttypes/offers/read | Platform görüntüsü Teklifi'nin özelliklerini alma |
> | Eylem | Microsoft.Compute/locations/publishers/artifacttypes/offers/skus/read | Platform görüntüsü Sku'sunun özelliklerini alma |
> | Eylem | Microsoft.Compute/locations/publishers/artifacttypes/offers/skus/versions/read | Platform görüntüsü sürümü özelliklerini alma |
> | Eylem | Microsoft.Compute/locations/publishers/artifacttypes/types/read | Özelliklerini VMExtension türü alma |
> | Eylem | Microsoft.Compute/locations/publishers/artifacttypes/types/versions/read | VMExtension sürümü özelliklerini alma |
> | Eylem | Microsoft.Compute/locations/publishers/read | Yayımcının özelliklerini alır |
> | Eylem | Microsoft.Compute/locations/runCommands/read | Konumda kullanılabilir çalıştırma komutlarını listeler |
> | Eylem | Microsoft.Compute/locations/usages/read | Hizmet sınırları ve geçerli kullanım miktarlarını aboneliğinin işlem kaynakları için bir konuma alır. |
> | Eylem | Microsoft.Compute/locations/vmSizes/read | Bir konumdaki kullanılabilir sanal makine boyutlarını listeler |
> | Eylem | Microsoft.Compute/operations/read | Microsoft.Compute kaynak sağlayıcısındaki kullanılabilir işlemleri listele |
> | Eylem | Microsoft.Compute/proximityPlacementGroups/delete | Yakınlık yerleştirme grubunu siler |
> | Eylem | Microsoft.Compute/proximityPlacementGroups/read | Yakınlık yerleştirme grubunun özelliklerini alın |
> | Eylem | Microsoft.Compute/proximityPlacementGroups/write | Yeni bir yakınlık yerleştirme grubu oluşturur veya mevcut olanı güncelleştirir |
> | Eylem | Microsoft.Compute/register/action | Aboneliği Microsoft.Compute kaynak sağlayıcısına kaydeder |
> | Eylem | Microsoft.Compute/restorePointCollections/delete | Geri yükleme noktalarını geri yükleme noktası koleksiyonu ve bağımsız siler |
> | Eylem | Microsoft.Compute/restorePointCollections/read | Geri yükleme noktası koleksiyonunun özelliklerini alır |
> | Eylem | Microsoft.Compute/restorePointCollections/restorePoints/delete | Geri yükleme noktasını siler |
> | Eylem | Microsoft.Compute/restorePointCollections/restorePoints/read | Geri yükleme noktasının özelliklerini alır |
> | Eylem | Microsoft.Compute/restorePointCollections/restorePoints/retrieveSasUris/action | Blob SAS URI'leriyle birlikte geri yükleme noktasının özelliklerini alır |
> | Eylem | Microsoft.Compute/restorePointCollections/restorePoints/write | Yeni bir geri yükleme noktası oluşturur |
> | Eylem | Microsoft.Compute/restorePointCollections/write | Yeni bir geri yükleme noktası koleksiyonu oluşturur veya mevcut olanı güncelleştirir |
> | Eylem | Microsoft.Compute/sharedVMImages/delete | Sharedvmımage'i siler |
> | Eylem | Microsoft.Compute/sharedVMImages/read | Sharedvmımage özelliklerini al |
> | Eylem | Microsoft.Compute/sharedVMImages/versions/delete | Sharedvmımageversion Sil |
> | Eylem | Microsoft.Compute/sharedVMImages/versions/read | Sharedvmımageversion özelliklerini alma |
> | Eylem | Microsoft.Compute/sharedVMImages/versions/replicate/action | Sharedvmımageversion'ı hedef bölgelere çoğaltma |
> | Eylem | Microsoft.Compute/sharedVMImages/versions/write | Yeni bir Sharedvmımageversion oluştur veya mevcut bir güncelleştirme |
> | Eylem | Microsoft.Compute/sharedVMImages/write | Yeni bir Sharedvmımage oluşturur veya mevcut olanı güncelleştirir |
> | Eylem | Microsoft.Compute/skus/read | Aboneliğiniz için bulunan Microsoft.Compute SKU listesini alır |
> | Eylem | Microsoft.Compute/snapshots/beginGetAccess/action | Blob erişimi için anlık görüntünün SAS URI'sini Al |
> | Eylem | Microsoft.Compute/snapshots/delete | Bir anlık görüntüyü Sil |
> | Eylem | Microsoft.Compute/snapshots/endGetAccess/action | Anlık görüntünün SAS URI'sini iptal et |
> | Eylem | Microsoft.Compute/snapshots/read | Anlık görüntü özelliklerini alır |
> | Eylem | Microsoft.Compute/snapshots/write | Yeni anlık görüntü oluşturma veya var olan bir güncelleştirme |
> | Eylem | Microsoft.Compute/unregister/action | Aboneliği Microsoft.Compute kaynak sağlayıcısına kaydını siler |
> | Eylem | Microsoft.Compute/virtualMachines/capture/action | Sanal sabit diskleri kopyalayarak sanal makineyi yakalar ve benzer sanal makineler oluşturmak için kullanılan bir şablon oluşturur |
> | Eylem | Microsoft.Compute/virtualMachines/convertToManagedDisks/action | Sanal makinenin blob tabanlı disklerini yönetilen disklere dönüştürür |
> | Eylem | Microsoft.Compute/virtualMachines/deallocate/action | İşlem kaynaklarını serbest bırakır ve sanal makineyi kapatır |
> | Eylem | Microsoft.Compute/virtualMachines/delete | Sanal makineyi siler |
> | Eylem | Microsoft.Compute/virtualMachines/extensions/delete | Sanal makine uzantısını siler |
> | Eylem | Microsoft.Compute/virtualMachines/extensions/read | Bir sanal makine uzantısının özelliklerini alır |
> | Eylem | Microsoft.Compute/virtualMachines/extensions/write | Yeni bir sanal makine uzantısı oluşturur veya mevcut olanı güncelleştirir |
> | Eylem | Microsoft.Compute/virtualMachines/generalize/action | Sanal makine durumunu Genelleştirmiş olarak ayarlar ve sanal makineyi yakalamaya hazırlar. |
> | Eylem | Microsoft.Compute/virtualMachines/instanceView/read | Sanal makine ve kaynaklarının ayrıntılı çalışma zamanı durumunu alır |
> | DataAction | Microsoft.Compute/virtualMachines/login/action | Bir sanal makine için normal bir kullanıcı olarak oturum açın |
> | DataAction | Microsoft.Compute/virtualMachines/loginAsAdmin/action | Bir sanal makinede Windows Yöneticisi veya Linux kök kullanıcı ayrıcalıkları oturum açın |
> | Eylem | Microsoft.Compute/virtualMachines/performMaintenance/action | VM'de bakım işlemini gerçekleştirir. |
> | Eylem | Microsoft.Compute/virtualMachines/powerOff/action | Sanal makineyi kapatır. Sanal makinenin faturalandırılmaya devam edeceğini unutmayın. |
> | Eylem | Microsoft.Compute/virtualMachines/read | Bir sanal makinenin özelliklerini alır |
> | Eylem | Microsoft.Compute/virtualMachines/redeploy/action | Sanal makineyi yeniden dağıtır |
> | Eylem | Microsoft.Compute/virtualMachines/reimage/action | Fark kayıt diski kullanarak görüntüsünü yeniden oluşturur sanal makine. |
> | Eylem | Microsoft.Compute/virtualMachines/restart/action | Sanal makine yeniden başlatılıyor |
> | Eylem | Microsoft.Compute/virtualMachines/runCommand/action | Sanal makinede önceden tanımlanmış bir betik yürütür |
> | Eylem | Microsoft.Compute/virtualMachines/start/action | Sanal makineyi başlatır |
> | Eylem | Microsoft.Compute/virtualMachines/vmSizes/read | Sanal makineyi güncelleştirmek için kullanılabilir boyutları listeler |
> | Eylem | Microsoft.Compute/virtualMachines/write | Yeni bir sanal makine oluşturur veya mevcut bir sanal makine güncelleştirmeleri |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/deallocate/action | Kapatır ve sanal makine ölçek kümesi örneklerinin işlem kaynaklarını serbest bırakır  |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/delete | Sanal makine ölçek kümesini siler |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/delete/action | Sanal makine ölçek kümesinin örneklerini siler |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/extensions/delete | Siler sanal makine ölçek kümesi uzantısını |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/extensions/read | Bir sanal makine ölçek kümesi uzantısının özelliklerini alır |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/extensions/write | Yeni sanal makine ölçek kümesi uzantısı oluşturur veya mevcut olanı güncelleştirir |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/forceRecoveryServiceFabricPlatformUpdateDomainWalk/action | Service fabric takılmış bekleyen bir güncelleştirmeyi tamamlamak için sanal makine ölçek kümesinin platform güncelleştirme etki alanlarını el ile gezin |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/instanceView/read | Sanal makine ölçek kümesi örnek görünümünü alır |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/manualUpgrade/action | Örnek sanal makine ölçek kümesinin son modeline el ile güncelleştirir |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/networkInterfaces/read | Bir sanal makine ölçek kümesinin tüm ağ arabirimlerinin özelliklerini alma |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/osRollingUpgrade/action | En son kullanılabilir Platform görüntü işletim sistemi sürümü için tüm sanal makine ölçek kümesi örnekleri taşımak için sıralı yükseltme başlatılır. |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/osUpgradeHistory/read | Bir sanal makine ölçek kümesi için işletim sistemi yükseltme geçmişini alır. |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/performMaintenance/action | Sanal makine ölçek kümesi'nin örneklerinde planlanmış bakım gerçekleştirir |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/powerOff/action | Sanal makine ölçek kümesinin örneklerini kapatır |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/publicIPAddresses/read | Bir sanal makine ölçek kümesinin tüm genel IP adreslerinin özelliklerini alma |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/read | Bir sanal makine ölçek kümesi özelliklerini alma |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/redeploy/action | Sanal makine ölçek kümesinin örneklerini yeniden dağıtın |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/reimage/action | Sanal makine ölçek kümesinin örneklerini yeniden görüntü oluşturma |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/reimageAll/action | Tüm diskleri (işletim sistemi diski ve veri diskleri) bir sanal makine ölçek kümesi örnekleri için yeniden görüntü oluşturma  |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/restart/action | Sanal makine ölçek kümesinin örneklerini yeniden başlatır |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/rollingUpgrades/cancel/action | Bir sanal makine ölçek kümesinin sıralı yükseltmesini iptal eder |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/rollingUpgrades/read | Bir sanal makine ölçek kümesi için son sıralı yükseltme durumunu alın |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/scale/action | Mevcut bir sanal makine ölçek kümesi ölçeği daraltma / ölçeği genişletme işlemlerini belirtilen örnek sayısında doğrulayın |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/skus/read | Mevcut bir sanal makine ölçek kümesi için geçerli SKU'ları listeler |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/start/action | Sanal makine ölçek kümesinin örneklerini başlatır |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/deallocate/action | Kapatır ve VM ölçek kümesindeki sanal makine için işlem kaynaklarını serbest bırakır. |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/delete | VM ölçek kümesindeki belirli bir sanal makineyi silin. |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/instanceView/read | VM ölçek kümesindeki bir sanal makinenin örnek görünümünü alır. |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/networkInterfaces/ipConfigurations/publicIPAddresses/read | Sanal makine ölçek kümesi kullanılarak oluşturulan genel IP adresinin özelliklerini alın. Sanal makine ölçek kümesi en fazla ipconfiguration (özel IP) başına bir genel IP oluşturabilir |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/networkInterfaces/ipConfigurations/read | Sanal makine ölçek kümesi kullanılarak oluşturulan bir ağ arabiriminin IP yapılandırmaları bir ya da tüm özelliklerini alır. IP yapılandırmaları özel IP'leri temsil eder. |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/networkInterfaces/read | Sanal makine ölçek kümesi kullanılarak oluşturulan bir sanal makinenin bir veya tüm ağ arabirimlerinin özelliklerini alma |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/performMaintenance/action | Sanal makine ölçek kümesindeki bir sanal makine örneğinde planlanmış bakım gerçekleştirir |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/powerOff/action | VM ölçek kümesindeki bir sanal makine Powers örneği. |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/read | VM ölçek kümesindeki bir sanal makinenin özelliklerini alır |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/redeploy/action | Sanal makine ölçek kümesindeki bir sanal makine örneğini yeniden dağıtır |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/reimage/action | Sanal makine ölçek kümesindeki bir sanal makine örneği görüntüsünü yeniden oluşturur. |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/reimageAll/action | Tüm diskler (işletim sistemi diski ve veri diskleri) için sanal makine ölçek kümesindeki sanal makine örneği görüntüsünü yeniden oluşturur. |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/restart/action | Bir VM ölçek kümesindeki bir sanal makine örneği başlatır. |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/runCommand/action | Sanal makine ölçek kümesindeki bir sanal makine örneğinde önceden tanımlanmış bir betik yürütür. |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/start/action | Bir VM ölçek kümesindeki bir sanal makine örneği başlatır. |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/write | VM ölçek kümesindeki bir sanal makinenin özelliklerini güncelleştirir |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/vmSizes/read | Oluşturulurken veya güncelleştirilirken bir sanal makine ölçek kümesindeki sanal makine için kullanılabilir boyutların listesi |
> | Eylem | Microsoft.Compute/virtualMachineScaleSets/write | Yeni bir sanal makine ölçek kümesi oluşturur veya mevcut olanı güncelleştirir |

## <a name="microsoftconsumption"></a>Microsoft.Consumption

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Consumption/balances/read | Bir yönetim grubu için bir fatura dönemi için Özet kullanımı listeleyin. |
> | Eylem | Microsoft.Consumption/budgets/delete | Bir abonelik veya yönetim grubu tarafından bütçeleri silin. |
> | Eylem | Microsoft.Consumption/budgets/read | Bir abonelik veya yönetim grubu tarafından bütçeleri listeleyin. |
> | Eylem | Microsoft.Consumption/budgets/write | Oluşturur ve bir abonelik veya yönetim grubu tarafından bütçeleri güncelleştirin. |
> | Eylem | Microsoft.Consumption/charges/read | Liste ücretleri |
> | Eylem | Microsoft.Consumption/credits/read | Liste krediler |
> | Eylem | Microsoft.Consumption/events/read | Liste etkinlikleri |
> | Eylem | Microsoft.Consumption/forecasts/read | Liste tahminleri |
> | Eylem | Microsoft.Consumption/lots/read | Liste çok |
> | Eylem | Microsoft.Consumption/marketplaces/read | Market kaynak kullanım ayrıntıları için EA ve WebDirect abonelikler için bir kapsam listesi. |
> | Eylem | Microsoft.Consumption/operationresults/read | Liste operationresults |
> | Eylem | Microsoft.Consumption/operations/read | Microsoft.Consumption kaynağı sağlayıcısı tarafından desteklenen tüm işlemleri listeler. |
> | Eylem | Microsoft.Consumption/operationstatus/read | Liste operationstatus |
> | Eylem | Microsoft.Consumption/pricesheets/read | Bir abonelik veya yönetim grubu Pricesheets verilerini listeler. |
> | Eylem | Microsoft.Consumption/register/action | RP tüketim için kaydolun |
> | Eylem | Microsoft.Consumption/reservationDetails/read | Rezervasyon siparişi veya yönetim grubu tarafından ayrılmış örnekler için kullanım ayrıntılarını listeler. Örnek başına günlük düzeyi ayrıntıları verilerdir. |
> | Eylem | Microsoft.Consumption/reservationRecommendations/read | Bir abonelik için ayrılmış örnekler için tek veya paylaşılan önerileri listeler. |
> | Eylem | Microsoft.Consumption/reservationSummaries/read | Rezervasyon siparişi veya yönetim grubu tarafından ayrılmış örnekler için Özet kullanımı listeleyin. Özet verilerini günlük veya aylık düzeyinde ya da ' dir. |
> | Eylem | Microsoft.Consumption/reservationTransactions/read | Yönetim grubu tarafından ayrılmış örnekler için işlem geçmişi listeleyin. |
> | Eylem | Microsoft.Consumption/tags/read | EA ve abonelikleri için etiketleri listeleyin. |
> | Eylem | Microsoft.Consumption/tenants/read | Liste kiracılar |
> | Eylem | Microsoft.Consumption/tenants/register/action | Kapsam için bir kiracı tarafından Microsoft.Consumption eylemi kaydedin. |
> | Eylem | Microsoft.Consumption/terms/read | Bir abonelik veya yönetim grubu için koşulları listeler. |
> | Eylem | Microsoft.Consumption/usageDetails/read | EA WebDirect abonelikleri için bir kapsam için kullanım ayrıntılarını listeler. |

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.ContainerInstance/containerGroups/containers/exec/action | Exec belirli bir kapsayıcıya alın. |
> | Eylem | Microsoft.ContainerInstance/containerGroups/containers/logs/read | Belirli bir kapsayıcı için günlükleri alın. |
> | Eylem | Microsoft.ContainerInstance/containerGroups/delete | Belirli kapsayıcı grubunu silin. |
> | Eylem | Microsoft.ContainerInstance/containerGroups/providers/Microsoft.Insights/diagnosticSettings/read | Kapsayıcı grubun tanılama ayarını alır. |
> | Eylem | Microsoft.ContainerInstance/containerGroups/providers/Microsoft.Insights/diagnosticSettings/write | Oluşturur veya kapsayıcı grubun tanılama ayarını güncelleştirir. |
> | Eylem | Microsoft.ContainerInstance/containerGroups/providers/Microsoft.Insights/metricDefinitions/read | Kapsayıcı grubu için kullanılabilir ölçümleri alır. |
> | Eylem | Microsoft.ContainerInstance/containerGroups/read | Tüm kapsayıcı gruplarını alın. |
> | Eylem | Microsoft.ContainerInstance/containerGroups/restart/action | Belirli bir kapsayıcı grubunu yeniden başlatır. |
> | Eylem | Microsoft.ContainerInstance/containerGroups/start/action | Belirli bir kapsayıcı grubunu başlatır. |
> | Eylem | Microsoft.ContainerInstance/containerGroups/stop/action | Belirli bir kapsayıcı grubunu durdurur. İşlem kaynaklarını serbest bırakılır ve Faturalaması durdurulur. |
> | Eylem | Microsoft.ContainerInstance/containerGroups/write | Veya belirli bir kapsayıcı grubunu güncelleştirilemiyor. |
> | Eylem | Microsoft.ContainerInstance/locations/cachedImages/read | Önbelleğe alınan görüntüleri abonelik için bir bölgede yer alır. |
> | Eylem | Microsoft.ContainerInstance/locations/capabilities/read | İşlevleri için bir bölge edinin. |
> | Eylem | Microsoft.ContainerInstance/locations/deleteVirtualNetworkOrSubnets/action | Microsoft.ContainerInstance sanal ağın veya alt ağın silindiğini bildirir. |
> | Eylem | Microsoft.ContainerInstance/locations/usages/read | Kullanım için belirli bir bölge alın. |
> | Eylem | Microsoft.ContainerInstance/register/action | Kapsayıcı örneğinin kaynak sağlayıcısı için aboneliği kaydeder ve kapsayıcı grubu oluşturulmasını sağlar. |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.ContainerRegistry/checkNameAvailability/read | Kapsayıcı kayıt defteri adı için kullanılabilir olup olmadığını denetler. |
> | Eylem | Microsoft.ContainerRegistry/locations/deleteVirtualNetworkOrSubnets/action | Microsoft.ContainerRegistry sanal ağın veya alt ağın silindiğini bildirir |
> | Eylem | Microsoft.ContainerRegistry/locations/operationResults/read | Bir zaman uyumsuz işlem sonucunu alır |
> | Eylem | Microsoft.ContainerRegistry/operations/read | Tüm kullanılabilir Azure Container kayıt defteri REST API işlemlerini listeler |
> | Eylem | Microsoft.ContainerRegistry/register/action | Kapsayıcı kayıt defteri kaynak sağlayıcısı için aboneliği kaydeder ve kapsayıcı kayıt defterleri oluşturulmasını sağlar. |
> | Eylem | Microsoft.ContainerRegistry/registries/artifacts/delete | Kapsayıcı kayıt defterindeki yapıt silin. |
> | Eylem | Microsoft.ContainerRegistry/registries/builds/cancel/action | Varolan bir yapıyı iptal eder. |
> | Eylem | Microsoft.ContainerRegistry/registries/builds/getLogLink/action | Derleme günlükleri indirmek için bir bağlantı alır. |
> | Eylem | Microsoft.ContainerRegistry/registries/builds/read | Belirtilen derleme özelliklerini alır veya belirtilen kapsayıcı kayıt defteri için tüm derlemeleri listeler. |
> | Eylem | Microsoft.ContainerRegistry/registries/builds/write | Belirtilen parametrelerle bir derleme için bir kapsayıcı kayıt defteri güncelleştirir. |
> | Eylem | Microsoft.ContainerRegistry/registries/buildTasks/delete | Bir derleme görevi, bir kapsayıcı kayıt defterinden siler. |
> | Eylem | Microsoft.ContainerRegistry/registries/buildTasks/listSourceRepositoryProperties/action | Bir derleme görevi için kaynak denetimi özellikleri listeler. |
> | Eylem | Microsoft.ContainerRegistry/registries/buildTasks/read | Belirtilen derleme görevi özelliklerini alır veya belirtilen kapsayıcı kayıt defteri için tüm yapı görevleri listeler. |
> | Eylem | Microsoft.ContainerRegistry/registries/buildTasks/steps/delete | Bir derleme adımı, bir derleme görevi siler. |
> | Eylem | Microsoft.ContainerRegistry/registries/buildTasks/steps/listBuildArguments/action | Gizli dizi değişkenleri de dahil olmak üzere bir derleme adımı derleme bağımsız değişkenleri listeler. |
> | Eylem | Microsoft.ContainerRegistry/registries/buildTasks/steps/read | Belirtilen yapı adımının özelliklerini alır veya belirtilen derleme görevi için tüm derleme adımları listeler. |
> | Eylem | Microsoft.ContainerRegistry/registries/buildTasks/steps/write | Belirtilen parametrelerle bir derleme adımı için bir derleme görevi güncelleştirmeleri veya oluşturur. |
> | Eylem | Microsoft.ContainerRegistry/registries/buildTasks/write | Oluşturur veya bir derleme görevi bir kapsayıcı kayıt defteri için belirtilen parametrelerle güncelleştirir. |
> | Eylem | Microsoft.ContainerRegistry/registries/delete | Kapsayıcı kayıt defteri siler. |
> | Eylem | Microsoft.ContainerRegistry/registries/eventGridFilters/delete | Bir kapsayıcı kayıt defterinden bir olay Kılavuzu filtresini siler. |
> | Eylem | Microsoft.ContainerRegistry/registries/eventGridFilters/read | Belirtilen olay Kılavuzu filtresini özelliklerini alır veya belirtilen kapsayıcı kayıt defteri tüm olay ızgarası filtreleri listeler. |
> | Eylem | Microsoft.ContainerRegistry/registries/eventGridFilters/write | Oluşturur veya bir olay Kılavuzu filtresini kapsayıcı kayıt defteri için belirtilen parametrelerle güncelleştirir. |
> | Eylem | Microsoft.ContainerRegistry/registries/getBuildSourceUploadUrl/action | Kullanıcı kaynağı karşıya yükleme konumunu alır. |
> | Eylem | Microsoft.ContainerRegistry/registries/importImage/action | Belirtilen parametrelerle kapsayıcı kayıt defterine görüntü alın. |
> | Eylem | Microsoft.ContainerRegistry/registries/listBuildSourceUploadUrl/action | Kaynak alma url konumu için bir kapsayıcı kayıt defteri karşıya yükleyin. |
> | Eylem | Microsoft.ContainerRegistry/registries/listCredentials/action | Belirtilen kapsayıcı kayıt defterinin oturum açma kimlik bilgilerini listeler. |
> | Eylem | Microsoft.ContainerRegistry/registries/listPolicies/read | Belirtilen kapsayıcı kayıt defteri için ilkelerini listeler |
> | Eylem | Microsoft.ContainerRegistry/registries/listUsages/read | Belirtilen kapsayıcı kayıt defteri kota kullanımları listeler. |
> | Eylem | Microsoft.ContainerRegistry/registries/metadata/read | Bir kapsayıcı kayıt defteri için belirli bir depo meta verilerini alır |
> | Eylem | Microsoft.ContainerRegistry/registries/metadata/write | Bir depo için bir kapsayıcı kayıt defteri meta verilerini güncelleştirme |
> | Eylem | Microsoft.ContainerRegistry/registries/operationStatuses/read | Bir kayıt defteri zaman uyumsuz işlem durumunu alır |
> | Eylem | Microsoft.ContainerRegistry/registries/pull/read | Çektiğinizde ya da kapsayıcı kayıt defterinden görüntüleri alın. |
> | Eylem | Microsoft.ContainerRegistry/registries/push/write | Anında iletme veya görüntüleri bir kapsayıcı kayıt defterine yazma. |
> | Eylem | Microsoft.ContainerRegistry/registries/quarantineRead/read | Çektiğinizde ya da kapsayıcı kayıt defterinden karantinaya alınan görüntü alma |
> | Eylem | Microsoft.ContainerRegistry/registries/quarantineWrite/write | Yazma/karantinaya alınan görüntüleri karantina durumunu değiştir |
> | Eylem | Microsoft.ContainerRegistry/registries/queueBuild/action | İstek parametreleri temel alarak yeni bir derleme oluşturur ve derleme kuyruğa ekleyin. |
> | Eylem | Microsoft.ContainerRegistry/registries/read | Belirtilen kapsayıcı kayıt defteri özelliklerini alır veya belirtilen kaynak grubu veya abonelik altındaki tüm kapsayıcı kayıt defterleri listeler. |
> | Eylem | Microsoft.ContainerRegistry/registries/regenerateCredential/action | Belirtilen kapsayıcı kayıt defterinin oturum açma kimlik bilgilerini yeniden oluşturur. |
> | Eylem | Microsoft.ContainerRegistry/registries/replications/delete | Bir çoğaltma, bir kapsayıcı kayıt defterinden siler. |
> | Eylem | Microsoft.ContainerRegistry/registries/replications/operationStatuses/read | Çoğaltma zaman uyumsuz işlem durumunu alır |
> | Eylem | Microsoft.ContainerRegistry/registries/replications/read | Belirtilen çoğaltma özelliklerini alır veya belirtilen kapsayıcı kayıt defteri için tüm çoğaltmalar listeler. |
> | Eylem | Microsoft.ContainerRegistry/registries/replications/write | Oluşturur veya belirtilen parametrelerle bir çoğaltma için bir kapsayıcı kayıt defteri güncelleştirir. |
> | Eylem | Microsoft.ContainerRegistry/registries/runs/cancel/action | Mevcut bir çalıştırmayı iptal et. |
> | Eylem | Microsoft.ContainerRegistry/registries/runs/listLogSasUrl/action | Çalıştırma için günlük SAS URL'sini alır. |
> | Eylem | Microsoft.ContainerRegistry/registries/runs/read | Kapsayıcı kayıt defteri karşı çalıştırma ya da liste çalıştırmaları özelliklerini alır. |
> | Eylem | Microsoft.ContainerRegistry/registries/runs/write | Bir farklı çalıştır güncelleştirir. |
> | Eylem | Microsoft.ContainerRegistry/registries/scheduleRun/action | Kapsayıcı kayıt defteri karşı çalıştırma zamanlayın. |
> | Eylem | Microsoft.ContainerRegistry/registries/sign/write | Kapsayıcı kayıt defteri meta verilerini içeriğine güven İtme veya çekme. |
> | Eylem | Microsoft.ContainerRegistry/registries/tasks/delete | Kapsayıcı kayıt defteri için bir görevi siler. |
> | Eylem | Microsoft.ContainerRegistry/registries/tasks/listDetails/action | Tüm kapsayıcı kayıt defteri için görev ayrıntılarını listeler. |
> | Eylem | Microsoft.ContainerRegistry/registries/tasks/read | Bir görev, bir kapsayıcı kayıt defteri veya listedeki tüm görevleri alır. |
> | Eylem | Microsoft.ContainerRegistry/registries/tasks/write | Oluşturur veya bir görev için bir kapsayıcı kayıt defteri güncelleştirir. |
> | Eylem | Microsoft.ContainerRegistry/registries/updatePolicies/write | Belirtilen kapsayıcı kayıt defteri için ilkeleri güncelleştirir |
> | Eylem | Microsoft.ContainerRegistry/registries/webhooks/delete | Bir Web kancası, bir kapsayıcı kayıt defterinden siler. |
> | Eylem | Microsoft.ContainerRegistry/registries/webhooks/getCallbackConfig/action | Özel üst bilgiler ve yapılandırmasını hizmet URI'si için Web kancası alır. |
> | Eylem | Microsoft.ContainerRegistry/registries/webhooks/listEvents/action | Belirtilen Web kancasının son olayları listeler. |
> | Eylem | Microsoft.ContainerRegistry/registries/webhooks/operationStatuses/read | Bir Web kancası zaman uyumsuz işlem durumunu alır |
> | Eylem | Microsoft.ContainerRegistry/registries/webhooks/ping/action | Web kancası'na gönderilmesini ping olayı tetikler. |
> | Eylem | Microsoft.ContainerRegistry/registries/webhooks/read | Belirtilen Web kancası özelliklerini alır veya belirtilen kapsayıcı kayıt defteri Web kancaları listeler. |
> | Eylem | Microsoft.ContainerRegistry/registries/webhooks/write | Oluşturur veya bir kapsayıcı kayıt defteri için bir Web kancası belirtilen parametrelerle güncelleştirir. |
> | Eylem | Microsoft.ContainerRegistry/registries/write | Belirtilen parametrelerle bir kapsayıcı kayıt defterini güncelleştirir veya oluşturur. |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.ContainerService/containerServices/delete | Bir kapsayıcı hizmeti siler |
> | Eylem | Microsoft.ContainerService/containerServices/read | Bir kapsayıcı hizmeti alın |
> | Eylem | Microsoft.ContainerService/containerServices/write | Yeni bir kapsayıcı hizmeti oluşturur veya mevcut olanı güncelleştirir |
> | Eylem | Microsoft.ContainerService/locations/operationresults/read | Zaman uyumsuz işlem sonucu durumunu alır |
> | Eylem | Microsoft.ContainerService/locations/operations/read | Zaman uyumsuz bir işlemin durumunu alır |
> | Eylem | Microsoft.ContainerService/locations/orchestrators/read | Desteklenen düzenleyicileri listeler |
> | Eylem | Microsoft.ContainerService/managedClusters/accessProfiles/listCredential/action | Rol adıyla listesi kimlik bilgilerini kullanarak bir yönetilen küme erişim profilini Al |
> | Eylem | Microsoft.ContainerService/managedClusters/accessProfiles/read | Rol adıyla bir yönetilen küme erişim profilini Al |
> | Eylem | Microsoft.ContainerService/managedClusters/agentPools/delete | Bir aracı havuzu siler |
> | Eylem | Microsoft.ContainerService/managedClusters/agentPools/read | Bir aracı havuzu alır |
> | Eylem | Microsoft.ContainerService/managedClusters/agentPools/write | Yeni bir aracı havuzu oluşturur veya mevcut olanı güncelleştirir |
> | Eylem | Microsoft.ContainerService/managedClusters/delete | Yönetilen bir kümesini siler |
> | Eylem | Microsoft.ContainerService/managedClusters/listClusterAdminCredential/action | Yönetilen bir küme clusterAdmin kimlik listesi |
> | Eylem | Microsoft.ContainerService/managedClusters/listClusterUserCredential/action | Yönetilen bir küme clusterUser kimlik listesi |
> | Eylem | Microsoft.ContainerService/managedClusters/read | Yönetilen kümesi Al |
> | Eylem | Microsoft.ContainerService/managedClusters/resetAADProfile/action | Yönetilen bir küme AAD profilini Sıfırla |
> | Eylem | Microsoft.ContainerService/managedClusters/resetServicePrincipalProfile/action | Yönetilen bir kümesinin hizmet sorumlusu profili Sıfırla |
> | Eylem | Microsoft.ContainerService/managedClusters/upgradeprofiles/read | Küme yükseltme profilini alır |
> | Eylem | Microsoft.ContainerService/managedClusters/write | Yeni bir yönetilen kümesi oluşturur veya mevcut olanı güncelleştirir |
> | Eylem | Microsoft.ContainerService/openShiftClusters/delete | Bir Open Shift küme silme |
> | Eylem | Microsoft.ContainerService/openShiftClusters/read | Bir Open Shift kümesi Al |
> | Eylem | Microsoft.ContainerService/openShiftClusters/write | Yeni bir açık üst karakter kümesi oluşturur veya mevcut olanı güncelleştirir |
> | Eylem | Microsoft.ContainerService/openShiftManagedClusters/delete | Bir Open Shift yönetilen küme silme |
> | Eylem | Microsoft.ContainerService/openShiftManagedClusters/read | Bir Open Shift yönetilen kümesi Al |
> | Eylem | Microsoft.ContainerService/openShiftManagedClusters/write | Yeni bir açık Shift yönetilen kümesi oluşturur veya mevcut olanı güncelleştirir |
> | Eylem | Microsoft.ContainerService/operations/read | Microsoft.ContainerService kaynak sağlayıcısındaki kullanılabilir işlemleri listele |
> | Eylem | Microsoft.ContainerService/register/action | Aboneliği Microsoft.ContainerService kaynak sağlayıcısına kaydeder |
> | Eylem | Microsoft.ContainerService/unregister/action | Microsoft.ContainerService kaynak sağlayıcı ile abonelik kaydını siler |

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.ContentModerator/applications/delete | Silme işlemi |
> | Eylem | Microsoft.ContentModerator/applications/listSecrets/action | Gizli anahtarları listeleme |
> | Eylem | Microsoft.ContentModerator/applications/listSingleSignOnToken/action | Çoklu oturum açma belirteçleri okuyun |
> | Eylem | Microsoft.ContentModerator/applications/read | Okuma işlemi |
> | Eylem | Microsoft.ContentModerator/applications/write | Yazma işlemi |
> | Eylem | Microsoft.ContentModerator/applications/write | Yazma işlemi |
> | Eylem | Microsoft.ContentModerator/listCommunicationPreference/action | İletişim tercihlerini Listele |
> | Eylem | Microsoft.ContentModerator/operations/read | okuma işlemleri |
> | Eylem | Microsoft.ContentModerator/updateCommunicationPreference/action | İletişim tercihlerini güncelleştir |

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.CostManagement/cloudConnectors/delete | Belirtilen cloudConnector silin. |
> | Eylem | Microsoft.CostManagement/cloudConnectors/read | Kimliği doğrulanmış kullanıcı için cloudConnectors listeleyin. |
> | Eylem | Microsoft.CostManagement/cloudConnectors/write | Veya belirtilen cloudConnector güncelleştirilemiyor. |
> | Eylem | Microsoft.CostManagement/dimensions/read | Bir kapsam tarafından desteklenen tüm boyutları listeler. |
> | Eylem | Microsoft.CostManagement/exports/action | Belirtilen dışarı aktarma çalıştırın. |
> | Eylem | Microsoft.CostManagement/exports/delete | Belirtilen dışarı aktarma silin. |
> | Eylem | Microsoft.CostManagement/exports/read | Dışarı aktarmaları kapsama göre listeler. |
> | Eylem | Microsoft.CostManagement/exports/run/action | Dışarı aktarmalar çalıştırın. |
> | Eylem | Microsoft.CostManagement/exports/write | Veya belirtilen dışarı aktarma güncelleştirilemiyor. |
> | Eylem | Microsoft.CostManagement/externalBillingAccounts/externalSubscriptions/read | Kimliği doğrulanmış kullanıcı için bir externalBillingAccount içinde externalSubscriptions listeleyin. |
> | Eylem | Microsoft.CostManagement/externalBillingAccounts/read | Kimliği doğrulanmış kullanıcı için externalBillingAccounts listeleyin. |
> | Eylem | Microsoft.CostManagement/externalSubscriptions/read | Kimliği doğrulanmış kullanıcı için externalSubscriptions listeleyin. |
> | Eylem | Microsoft.CostManagement/externalSubscriptions/write | ExternalSubscription ilişkili yönetim grubu güncelleştir |
> | Eylem | Microsoft.CostManagement/query/action | Kullanım verileri bir kapsama göre sorgulayın. |
> | Eylem | Microsoft.CostManagement/query/read | Kullanım verileri bir kapsama göre sorgulayın. |
> | Eylem | Microsoft.CostManagement/register/action | Kapsamı için bir abonelik tarafından eylemi, Microsoft.CostManagement kaydedin. |
> | Eylem | Microsoft.CostManagement/reports/action | Kullanım verileri bir kapsama göre zamanlamayı raporlar. |
> | Eylem | Microsoft.CostManagement/reports/read | Kullanım verileri bir kapsama göre zamanlamayı raporlar. |
> | Eylem | Microsoft.CostManagement/tenants/register/action | Kapsam için bir kiracı tarafından Microsoft.CostManagement eylemi kaydedin. |

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.CustomerInsights/hubs/adobemetadata/action | Azure müşteri öngörüleri Adobe meta verileri güncelle |
> | Eylem | Microsoft.CustomerInsights/hubs/adobemetadata/read | Azure müşteri öngörüleri Adobe meta verileri okuyun |
> | Eylem | Microsoft.CustomerInsights/hubs/authorizationPolicies/delete | Tüm Azure müşteri öngörüleri paylaşılan erişim imzası ilkesini silme |
> | Eylem | Microsoft.CustomerInsights/hubs/authorizationPolicies/read | Herhangi bir Azure müşterisi öngörü paylaşılan erişim imzası ilkesini okuyun |
> | Eylem | Microsoft.CustomerInsights/hubs/authorizationPolicies/regeneratePrimaryKey/action | Azure müşteri öngörüleri paylaşılan erişim imzası ilkesinin birincil anahtarını yeniden oluştur |
> | Eylem | Microsoft.CustomerInsights/hubs/authorizationPolicies/regenerateSecondaryKey/action | Azure müşteri öngörüleri paylaşılan erişim imzası İlkesi ikincil anahtarı yeniden oluştur |
> | Eylem | Microsoft.CustomerInsights/hubs/authorizationPolicies/write | Herhangi bir Azure müşterisi Insights paylaşılan erişim imzası İlkesi güncelle |
> | Eylem | Microsoft.CustomerInsights/hubs/connectors/activate/action | Azure müşteri öngörüleri bağlayıcıyı etkinleştir |
> | Eylem | Microsoft.CustomerInsights/hubs/connectors/activate/action | Azure müşteri öngörüleri bağlayıcıyı etkinleştir |
> | Eylem | Microsoft.CustomerInsights/hubs/connectors/delete | Azure müşteri öngörüleri bağlayıcıyı Sil |
> | Eylem | Microsoft.CustomerInsights/hubs/connectors/getruntimestatus/action | Tüm Azure müşteri öngörüleri Bağlayıcısı çalışma zamanı durumunu Al |
> | Eylem | Microsoft.CustomerInsights/hubs/connectors/mappings/activate/action | Tüm Azure müşteri öngörüleri bağlayıcı eşlemesi etkinleştir |
> | Eylem | Microsoft.CustomerInsights/hubs/connectors/mappings/delete | Tüm Azure müşteri öngörüleri bağlayıcı eşlemesi Sil |
> | Eylem | Microsoft.CustomerInsights/hubs/connectors/mappings/operations/read | Azure müşteri öngörüleri Bağlayıcısı eşleme herhangi bir işlem sonucunu okuyun |
> | Eylem | Microsoft.CustomerInsights/hubs/connectors/mappings/read | Tüm Azure müşteri öngörüleri bağlayıcı eşlemesi okuyun |
> | Eylem | Microsoft.CustomerInsights/hubs/connectors/mappings/write | Tüm Azure müşteri öngörüleri bağlayıcı eşlemesi güncelle |
> | Eylem | Microsoft.CustomerInsights/hubs/connectors/operations/read | Tüm Azure müşteri öngörüleri Bağlayıcısı işlem sonucunu okuyun |
> | Eylem | Microsoft.CustomerInsights/hubs/connectors/read | Tüm Azure müşteri öngörüleri Bağlayıcısı'nı okuyun |
> | Eylem | Microsoft.CustomerInsights/hubs/connectors/saveauthinfo/action | Tüm Azure müşteri öngörüleri Bağlayıcısı kimlik bilgisi güncelle |
> | Eylem | Microsoft.CustomerInsights/hubs/connectors/update/action | Tüm Azure müşteri öngörüleri bağlayıcıyı güncelleştir |
> | Eylem | Microsoft.CustomerInsights/hubs/connectors/write | Oluşturma veya güncelleştirme herhangi bir Azure müşterisi Insights Bağlayıcısı |
> | Eylem | Microsoft.CustomerInsights/hubs/crmmetadata/action | Azure müşteri öngörüleri Crm meta verileri güncelle |
> | Eylem | Microsoft.CustomerInsights/hubs/crmmetadata/read | Azure müşteri öngörüleri Crm meta verileri okuyun |
> | Eylem | Microsoft.CustomerInsights/hubs/delete | Azure müşteri öngörüleri hub'ını Sil |
> | Eylem | Microsoft.CustomerInsights/hubs/gdpr/delete | Tüm Azure müşteri öngörüleri Gdpr silme |
> | Eylem | Microsoft.CustomerInsights/hubs/gdpr/read | Tüm Azure müşteri öngörüleri Gdpr okuyun |
> | Eylem | Microsoft.CustomerInsights/hubs/gdpr/write | Tüm Azure müşteri öngörüleri Gdpr güncelle |
> | Eylem | Microsoft.CustomerInsights/hubs/getbillingcredits/read | Azure müşteri öngörüleri Hub fatura kredisi kazanın |
> | Eylem | Microsoft.CustomerInsights/hubs/getbillinghistory/read | Azure müşteri öngörüleri Hub faturalandırma geçmişi Al |
> | Eylem | Microsoft.CustomerInsights/hubs/images/delete | Herhangi bir Azure müşterisi Insights görüntüyü Sil |
> | Eylem | Microsoft.CustomerInsights/hubs/images/read | Herhangi bir Azure müşterisi Insights görüntü okuma |
> | Eylem | Microsoft.CustomerInsights/hubs/images/write | Tüm Azure müşteri öngörüleri görüntü oluştur ve güncelleştir |
> | Eylem | Microsoft.CustomerInsights/hubs/interactions/delete | Tüm azure müşteri öngörüleri etkileşimleri Sil |
> | Eylem | Microsoft.CustomerInsights/hubs/interactions/operations/read | Tüm Azure müşteri öngörüleri etkileşim işlem sonucunu okuyun |
> | Eylem | Microsoft.CustomerInsights/hubs/interactions/read | Azure müşteri öngörüleri etkileşimi okuyun |
> | Eylem | Microsoft.CustomerInsights/hubs/interactions/suggestrelationshiplinks/action | İlişki bağlantılar için herhangi bir Azure müşterisi Insights etkileşimi önerin |
> | Eylem | Microsoft.CustomerInsights/hubs/interactions/write | Azure müşteri öngörüleri etkileşimi güncelle |
> | Eylem | Microsoft.CustomerInsights/hubs/kpi/delete | Tüm Azure müşteri öngörüleri ana performans göstergesinin Sil |
> | Eylem | Microsoft.CustomerInsights/hubs/kpi/operations/read | Tüm Azure müşteri öngörüleri ana performans göstergeleri işlem sonucunu okuyun |
> | Eylem | Microsoft.CustomerInsights/hubs/kpi/read | Tüm Azure müşteri öngörüleri ana performans göstergesinin okuyun |
> | Eylem | Microsoft.CustomerInsights/hubs/kpi/reprocess/action | Tüm Azure müşteri öngörüleri ana performans göstergelerini yeniden işleme |
> | Eylem | Microsoft.CustomerInsights/hubs/kpi/write | Tüm Azure müşteri öngörüleri ana performans göstergesinin güncelle |
> | Eylem | Microsoft.CustomerInsights/hubs/links/delete | Azure müşteri öngörüleri giden bağlantıları silme |
> | Eylem | Microsoft.CustomerInsights/hubs/links/operations/read | Tüm Azure müşteri öngörüleri bağlantıları işlem sonucunu okuyun |
> | Eylem | Microsoft.CustomerInsights/hubs/links/read | Azure müşteri öngörüleri giden bağlantıları okuyun |
> | Eylem | Microsoft.CustomerInsights/hubs/links/write | Herhangi bir Azure müşterisi bağlantı güncelle |
> | Eylem | Microsoft.CustomerInsights/hubs/msemetadata/action | Azure müşteri öngörüleri Mse meta verileri güncelle |
> | Eylem | Microsoft.CustomerInsights/hubs/msemetadata/read | Azure müşteri öngörüleri Mse meta verileri okuyun |
> | Eylem | Microsoft.CustomerInsights/hubs/operationresults/read | Azure müşteri öngörüleri Hub İşlem sonuçlarını Al |
> | Eylem | Microsoft.CustomerInsights/hubs/predictions/delete | Tüm Azure müşteri öngörüleri Öngörüler Sil |
> | Eylem | Microsoft.CustomerInsights/hubs/predictions/operations/read | Tüm Azure müşteri öngörüleri Öngörüler işlem sonucunu okuyun |
> | Eylem | Microsoft.CustomerInsights/hubs/predictions/read | Tüm Azure müşteri öngörüleri Öngörüler edinin |
> | Eylem | Microsoft.CustomerInsights/hubs/predictions/write | Herhangi bir Azure müşterisi Öngörüler güncelle |
> | Eylem | Microsoft.CustomerInsights/hubs/predictivematchpolicies/delete | Hiçbir Azure müşteri öngörüleri Tahmine dayalı eşleştirme ilkesi Sil |
> | Eylem | Microsoft.CustomerInsights/hubs/predictivematchpolicies/operations/read | Tüm Azure müşteri öngörüleri Tahmine dayalı eşleştirme ilkeleri işlem sonucunu okuyun |
> | Eylem | Microsoft.CustomerInsights/hubs/predictivematchpolicies/read | Hiçbir Azure müşteri öngörüleri Tahmine dayalı eşleştirme ilkesi okuyun |
> | Eylem | Microsoft.CustomerInsights/hubs/predictivematchpolicies/write | Hiçbir Azure müşteri öngörüleri Tahmine dayalı eşleştirme ilkesi güncelle |
> | Eylem | Microsoft.CustomerInsights/hubs/profiles/delete | Tüm Azure müşteri öngörüleri profilini Sil |
> | Eylem | Microsoft.CustomerInsights/hubs/profiles/operations/read | Tüm Azure müşteri öngörüleri profili işlem sonucunu okuyun |
> | Eylem | Microsoft.CustomerInsights/hubs/profiles/read | Tüm Azure müşteri öngörüleri profilini okuma |
> | Eylem | Microsoft.CustomerInsights/hubs/profiles/write | Herhangi bir Azure müşterisi Insights profil yazma |
> | Eylem | Microsoft.CustomerInsights/hubs/providers/Microsoft.Insights/diagnosticSettings/read | Kaynağın tanılama ayarını alır |
> | Eylem | Microsoft.CustomerInsights/hubs/providers/Microsoft.Insights/diagnosticSettings/write | Kaynağın tanılama ayarını oluşturur veya güncelleştirir |
> | Eylem | Microsoft.CustomerInsights/hubs/providers/Microsoft.Insights/logDefinitions/read | Kaynak için kullanılabilir günlükleri alır |
> | Eylem | Microsoft.CustomerInsights/hubs/providers/Microsoft.Insights/metricDefinitions/read | Kaynak için kullanılabilir ölçümleri alır |
> | Eylem | Microsoft.CustomerInsights/hubs/read | Azure müşteri öngörüleri hub'ı okuyun |
> | Eylem | Microsoft.CustomerInsights/hubs/relationshiplinks/delete | Tüm Azure müşteri öngörüleri ilişki bağlantılarını silin |
> | Eylem | Microsoft.CustomerInsights/hubs/relationshiplinks/operations/read | Tüm Azure müşteri öngörüleri ilişki bağlantılarını işlem sonucunu okuyun |
> | Eylem | Microsoft.CustomerInsights/hubs/relationshiplinks/read | Azure müşteri öngörüleri ilişki bağlantıları okuyun |
> | Eylem | Microsoft.CustomerInsights/hubs/relationshiplinks/write | Azure müşteri öngörüleri ilişki bağlantıları güncelle |
> | Eylem | Microsoft.CustomerInsights/hubs/relationships/delete | Azure müşteri öngörüleri ilişkileri silme |
> | Eylem | Microsoft.CustomerInsights/hubs/relationships/operations/read | Tüm Azure müşteri öngörüleri ilişkileri işlem sonucunu okuyun |
> | Eylem | Microsoft.CustomerInsights/hubs/relationships/read | Azure müşteri öngörüleri ilişkileri okuyun |
> | Eylem | Microsoft.CustomerInsights/hubs/relationships/write | Azure müşteri öngörüleri ilişkileri güncelle |
> | Eylem | Microsoft.CustomerInsights/hubs/roleAssignments/delete | Tüm Azure müşteri öngörüleri Rbac atamasını Sil |
> | Eylem | Microsoft.CustomerInsights/hubs/roleAssignments/operations/read | Tüm Azure müşteri öngörüleri Rbac atama işleminin sonucunu okuyun |
> | Eylem | Microsoft.CustomerInsights/hubs/roleAssignments/read | Herhangi bir Azure müşterisi Insights Rbac atama okuyun |
> | Eylem | Microsoft.CustomerInsights/hubs/roleAssignments/write | Herhangi bir Azure müşterisi Insights Rbac atama güncelle |
> | Eylem | Microsoft.CustomerInsights/hubs/roles/read | Herhangi bir Azure müşterisi Insights Rbac rolü okuyun |
> | Eylem | Microsoft.CustomerInsights/hubs/salesforcemetadata/action | Azure müşteri öngörüleri SalesForce meta verileri güncelle |
> | Eylem | Microsoft.CustomerInsights/hubs/salesforcemetadata/read | Azure müşteri öngörüleri SalesForce meta verileri okuyun |
> | Eylem | Microsoft.CustomerInsights/hubs/segments/delete | Tüm Azure müşteri öngörüleri kesimleri Sil |
> | Eylem | Microsoft.CustomerInsights/hubs/segments/dynamic/action | Herhangi bir Azure müşterisi Insight dinamik yönetim kesim |
> | Eylem | Microsoft.CustomerInsights/hubs/segments/read | Tüm Azure müşteri öngörüleri kesimleri okuyun |
> | Eylem | Microsoft.CustomerInsights/hubs/segments/static/action | Herhangi bir Azure müşterisi Insight statik yönetim kesim |
> | Eylem | Microsoft.CustomerInsights/hubs/segments/write | Tüm Azure müşteri öngörüleri kesimleri güncelle |
> | Eylem | Microsoft.CustomerInsights/hubs/sqlconnectionstrings/delete | Tüm Azure müşteri öngörüleri SqlConnectionStrings Sil |
> | Eylem | Microsoft.CustomerInsights/hubs/sqlconnectionstrings/read | Tüm Azure müşteri öngörüleri SqlConnectionStrings okuyun |
> | Eylem | Microsoft.CustomerInsights/hubs/sqlconnectionstrings/write | Tüm Azure müşteri öngörüleri SqlConnectionStrings güncelle |
> | Eylem | Microsoft.CustomerInsights/hubs/suggesttypeschema/action | Örnek verileri Öner türü şeması oluşturma |
> | Eylem | Microsoft.CustomerInsights/hubs/tenantmanagement/read | Tüm Azure Customer Insights hub ayarlarını yönetme |
> | Eylem | Microsoft.CustomerInsights/hubs/views/delete | Herhangi bir Azure müşterisi Insights uygulama Görünümü Sil |
> | Eylem | Microsoft.CustomerInsights/hubs/views/read | Herhangi bir Azure müşterisi Insights uygulama görünüm okuyun |
> | Eylem | Microsoft.CustomerInsights/hubs/views/write | Oluşturma veya güncelleştirme herhangi bir Azure müşterisi Insights uygulama görünümü |
> | Eylem | Microsoft.CustomerInsights/hubs/widgettypes/read | Herhangi bir Azure müşterisi Insights uygulama pencere öğesi türü okuyun |
> | Eylem | Microsoft.CustomerInsights/hubs/write | Azure müşteri öngörüleri hub'ı güncelle |
> | Eylem | Microsoft.CustomerInsights/operations/read | Azure müşteri öngörüleri API Metadatas okuyun |
> | Eylem | Microsoft.CustomerInsights/register/action | Customer Insights kaynak sağlayıcısı için aboneliği kaydeder ve Customer Insights kaynaklarının oluşturulmasını sağlar |
> | Eylem | Microsoft.CustomerInsights/unregister/action | Customer Insights kaynak sağlayıcısı için aboneliği kaydını siler |

## <a name="microsoftdatabox"></a>Microsoft.DataBox

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.DataBox/jobs/bookShipmentPickUp/action | İade Sevk irsaliyesi için çekme kitap imkan tanır. |
> | Eylem | Microsoft.DataBox/jobs/cancel/action | Devam eden bir siparişi iptal eder. |
> | Eylem | Microsoft.DataBox/jobs/delete | Siparişler Sil |
> | Eylem | Microsoft.DataBox/jobs/listCredentials/action | Siparişle ilgili şifrelenmemiş kimlik bilgilerini listeler. |
> | Eylem | Microsoft.DataBox/jobs/read | Liste veya siparişleri alma |
> | Eylem | Microsoft.DataBox/jobs/write | Siparişleri oluştur veya güncelleştir |
> | Eylem | Microsoft.DataBox/locations/availableSkus/action | Bu yöntem, kullanılabilen SKU'ların listesini döndürür. |
> | Eylem | Microsoft.DataBox/locations/operationResults/read | İşlem sonuçlarını Al veya Listele |
> | Eylem | Microsoft.DataBox/locations/validateAddress/action | Teslimat adresini doğrular ve varsa alternatif adresler sağlar. |
> | Eylem | Microsoft.DataBox/register/action | Sağlayıcı Microsoft.Databox kaydetme |

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/alerts/read | Uyarıları alır veya listeler |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/alerts/read | Uyarıları alır veya listeler |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/bandwidthSchedules/delete | Bant genişliği zamanlamaları siler |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/bandwidthSchedules/operationResults/read | İşlem sonucunu alır veya listeler |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/bandwidthSchedules/read | Bant genişliği zamanlamaları alır veya listeler |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/bandwidthSchedules/read | Bant genişliği zamanlamaları alır veya listeler |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/bandwidthSchedules/write | Oluşturur veya bant genişliği zamanlamaları güncelleştirir |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/delete | Veri kutusu uç cihazlarına siler |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/downloadUpdates/action | Cihaz güncelleştirmeleri indir |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/getExtendedInformation/action | Kaynak genişletilmiş bilgilerini alır. |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/installUpdates/action | Güncelleştirmeleri cihaza yükleme |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/jobs/read | İşleri alır veya listeler |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/networkSettings/read | Cihazın ağ ayarlarını alır veya listeler |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/operationResults/read | İşlem sonucunu alır veya listeler |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/operationsStatus/read | İşlem durumunu alır veya listeler |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/orders/delete | Siparişler siler |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/orders/operationResults/read | İşlem sonucunu alır veya listeler |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/orders/read | Siparişleri alır veya listeler |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/orders/read | Siparişleri alır veya listeler |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/orders/write | Oluşturur veya güncelleştirir siparişleri |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/read | Veri kutusu uç cihazlarına alır veya listeler |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/read | Veri kutusu uç cihazlarına alır veya listeler |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/read | Veri kutusu uç cihazlarına alır veya listeler |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/roles/delete | Rolleri siler |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/roles/operationResults/read | İşlem sonucunu alır veya listeler |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/roles/read | Rolleri alır veya listeler |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/roles/read | Rolleri alır veya listeler |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/roles/write | Oluşturur veya güncelleştirir rolleri |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/scanForUpdates/action | Güncelleştirmeleri tara |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/securitySettings/operationResults/read | İşlem sonucunu alır veya listeler |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/securitySettings/update/action | Güvenlik ayarlarını güncelleştirme |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/shares/delete | Paylaşımları siler |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/shares/operationResults/read | İşlem sonucunu alır veya listeler |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/shares/read | Paylaşımları alır veya listeler |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/shares/read | Paylaşımları alır veya listeler |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/shares/refresh/action | Paylaşım meta veriler ile buluttaki Verileri Yenile |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/shares/write | Oluşturur veya güncelleştirir paylaşımları |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/storageAccountCredentials/delete | Depolama hesabı kimlik bilgilerini siler |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/storageAccountCredentials/operationResults/read | İşlem sonucunu alır veya listeler |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/storageAccountCredentials/read | Depolama hesabı kimlik bilgilerini alır veya listeler |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/storageAccountCredentials/read | Depolama hesabı kimlik bilgilerini alır veya listeler |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/storageAccountCredentials/write | Oluşturur veya depolama hesabı kimlik bilgilerini güncelleştirir |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/triggers/delete | Tetikleyiciler siler |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/triggers/operationResults/read | İşlem sonucunu alır veya listeler |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/triggers/read | Tetikleyiciler alır veya listeler |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/triggers/read | Tetikleyiciler alır veya listeler |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/triggers/write | Oluşturur veya güncelleştirir Tetikleyiciler |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/updateSummary/read | Güncelleştirme özeti alır veya listeler |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/uploadCertificate/action | Cihaz kaydı için sertifikayı karşıya yükleyin |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/users/delete | Paylaşımı kullanıcıları siler |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/users/operationResults/read | İşlem sonucunu alır veya listeler |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/users/read | Paylaşımı kullanıcıları alır veya listeler |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/users/read | Paylaşımı kullanıcıları alır veya listeler |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/users/write | Oluşturur veya güncelleştirir Paylaşımı kullanıcıları |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/write | Oluşturur veya veri kutusu uç cihazlarına güncelleştirir |
> | Eylem | Microsoft.DataBoxEdge/dataBoxEdgeDevices/write | Oluşturur veya veri kutusu uç cihazlarına güncelleştirir |

## <a name="microsoftdatabricks"></a>Microsoft.Databricks

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Databricks/register/action | Databricks için kaydolun. |
> | Eylem | Microsoft.Databricks/workspaces/delete | Bir Databricks çalışma alanını kaldırır. |
> | Eylem | Microsoft.Databricks/workspaces/providers/Microsoft.Insights/diagnosticSettings/read | Databricks çalışma alanı için kullanılabilir tanılama ayarlarını belirler |
> | Eylem | Microsoft.Databricks/workspaces/providers/Microsoft.Insights/diagnosticSettings/write | Ekleyin veya tanılama ayarları değiştirin. |
> | Eylem | Microsoft.Databricks/workspaces/providers/Microsoft.Insights/logDefinitions/read | Databricks çalışma alanı için kullanılabilir günlük tanımlarını alır |
> | Eylem | Microsoft.Databricks/workspaces/read | Databricks çalışma alanlarının bir listesini alır. |
> | Eylem | Microsoft.Databricks/workspaces/write | Bir Databricks çalışma alanı oluşturur. |

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.DataCatalog/catalogs/delete | Veri Kataloğu kaynak sağlayıcısı için katalog kaynağını silin. |
> | Eylem | Microsoft.DataCatalog/catalogs/read | Veri Kataloğu kaynak sağlayıcısı için katalog kaynağı oku. |
> | Eylem | Microsoft.DataCatalog/catalogs/write | Katalog kaynak için veri Kataloğu kaynak sağlayıcısı yazın. |
> | Eylem | Microsoft.DataCatalog/checkNameAvailability/read | Veri Kataloğu kaynak sağlayıcısı için katalog ad kullanılabilirliğini denetle. |
> | Eylem | Microsoft.DataCatalog/datacatalogs/delete | Veri Kataloğu kaynak sağlayıcısı için DataCatalog kaynağını silin. |
> | Eylem | Microsoft.DataCatalog/datacatalogs/read | Veri Kataloğu kaynak sağlayıcısı için DataCatalog kaynağı oku. |
> | Eylem | Microsoft.DataCatalog/datacatalogs/write | Veri Kataloğu kaynak sağlayıcısı için DataCatalog kaynak yazın. |
> | Eylem | Microsoft.DataCatalog/operations/read | Veri Kataloğu kaynak sağlayıcısındaki kullanılabilir tüm işlemlerin okur. |
> | Eylem | Microsoft.DataCatalog/register/action | Veri Kataloğu kaynak sağlayıcısı için aboneliği kaydedin |
> | Eylem | Microsoft.DataCatalog/unregister/action | Veri Kataloğu kaynak sağlayıcısı için abonelik kaydını sil |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.DataFactory/checkazuredatafactorynameavailability/read | Veri fabrikasının adını kullanmak için kullanılabilir olup olmadığını denetler. |
> | Eylem | Microsoft.DataFactory/datafactories/activitywindows/read | Belirtilen parametrelerle Data factory'de etkinlik Windows okur. |
> | Eylem | Microsoft.DataFactory/datafactories/datapipelines/activities/activitywindows/read | Belirtilen parametrelere sahip işlem hattı etkinliği için etkinlik Windows okur. |
> | Eylem | Microsoft.DataFactory/datafactories/datapipelines/activitywindows/read | Belirtilen parametrelere sahip işlem hattı için etkinlik Windows okur. |
> | Eylem | Microsoft.DataFactory/datafactories/datapipelines/delete | Herhangi bir işlem hattına siler. |
> | Eylem | Microsoft.DataFactory/datafactories/datapipelines/pause/action | Herhangi bir işlem hattına duraklatır. |
> | Eylem | Microsoft.DataFactory/datafactories/datapipelines/read | Herhangi bir işlem hattına okur. |
> | Eylem | Microsoft.DataFactory/datafactories/datapipelines/resume/action | Herhangi bir işlem hattına sürdürür. |
> | Eylem | Microsoft.DataFactory/datafactories/datapipelines/update/action | Herhangi bir işlem hattına güncelleştirir. |
> | Eylem | Microsoft.DataFactory/datafactories/datapipelines/write | Oluşturur veya herhangi bir işlem hattına güncelleştirir. |
> | Eylem | Microsoft.DataFactory/datafactories/datasets/activitywindows/read | Belirtilen parametrelere sahip bir veri kümesi için etkinlik Windows okur. |
> | Eylem | Microsoft.DataFactory/datafactories/datasets/delete | Herhangi bir veri kümesini siler. |
> | Eylem | Microsoft.DataFactory/datafactories/datasets/read | Tüm veri okur. |
> | Eylem | Microsoft.DataFactory/datafactories/datasets/sliceruns/read | Verilen veri kümesinin belirtilen başlangıç zamanıyla veri dilimi çalıştırın okur. |
> | Eylem | Microsoft.DataFactory/datafactories/datasets/slices/read | Veri dilimleri belirli bir dönemde alır. |
> | Eylem | Microsoft.DataFactory/datafactories/datasets/slices/write | Veri dilimi durumunu güncelleştirin. |
> | Eylem | Microsoft.DataFactory/datafactories/datasets/write | Oluşturur veya herhangi bir veri kümesini güncelleştirir. |
> | Eylem | Microsoft.DataFactory/datafactories/delete | Data Factory siler. |
> | Eylem | Microsoft.DataFactory/datafactories/gateways/connectioninfo/action | Herhangi bir ağ geçidi için bağlantı bilgilerini okur. |
> | Eylem | Microsoft.DataFactory/datafactories/gateways/delete | Herhangi bir ağ geçidini siler. |
> | Eylem | Microsoft.DataFactory/datafactories/gateways/listauthkeys/action | Herhangi bir ağ geçidi için kimlik doğrulaması anahtarlarını listeler. |
> | Eylem | Microsoft.DataFactory/datafactories/gateways/read | Herhangi bir ağ geçidini okur. |
> | Eylem | Microsoft.DataFactory/datafactories/gateways/regenerateauthkey/action | Kimlik doğrulaması anahtarları için herhangi bir ağ geçidini yeniden oluşturur. |
> | Eylem | Microsoft.DataFactory/datafactories/gateways/write | Oluşturur veya herhangi bir ağ geçidini güncelleştirir. |
> | Eylem | Microsoft.DataFactory/datafactories/linkedServices/delete | Herhangi bir bağlı hizmeti siler. |
> | Eylem | Microsoft.DataFactory/datafactories/linkedServices/read | Herhangi bir bağlı hizmeti okur. |
> | Eylem | Microsoft.DataFactory/datafactories/linkedServices/write | Oluşturur veya güncelleştirir herhangi bir bağlı hizmeti. |
> | Eylem | Microsoft.DataFactory/datafactories/read | Data Factory okur. |
> | Eylem | Microsoft.DataFactory/datafactories/runs/loginfo/read | Günlükleri içeren bir blob kapsayıcısı için SAS URI'si okur. |
> | Eylem | Microsoft.DataFactory/datafactories/tables/delete | Herhangi bir veri kümesini siler. |
> | Eylem | Microsoft.DataFactory/datafactories/tables/read | Tüm veri okur. |
> | Eylem | Microsoft.DataFactory/datafactories/tables/write | Oluşturur veya herhangi bir veri kümesini güncelleştirir. |
> | Eylem | Microsoft.DataFactory/datafactories/write | Oluşturur veya veri fabrikası güncelleştirir. |
> | Eylem | Microsoft.DataFactory/factories/cancelpipelinerun/action | İşlem hattı çalıştırma kimliği ile belirtilen çalıştırmasını iptal eder |
> | Eylem | Microsoft.DataFactory/factories/createdataflowdebugsession/action | Bir veri akışı hata ayıklama oturumu oluşturur. |
> | Eylem | Microsoft.DataFactory/factories/dataflows/delete | Veri akışı siler. |
> | Eylem | Microsoft.DataFactory/factories/dataflows/read | Veri akışı okur. |
> | Eylem | Microsoft.DataFactory/factories/dataflows/write | Veri akışı güncelle |
> | Eylem | Microsoft.DataFactory/factories/datasets/delete | Herhangi bir veri kümesini siler. |
> | Eylem | Microsoft.DataFactory/factories/datasets/read | Tüm veri okur. |
> | Eylem | Microsoft.DataFactory/factories/datasets/write | Oluşturur veya herhangi bir veri kümesini güncelleştirir. |
> | Eylem | Microsoft.DataFactory/factories/delete | Veri Fabrikası siler. |
> | Eylem | Microsoft.DataFactory/factories/deletedataflowdebugsession/action | Bir veri akışı hata ayıklama oturumu siler. |
> | Eylem | Microsoft.DataFactory/factories/getDataPlaneAccess/action | ADF ile veri düzlemi hizmete erişim alır. |
> | Eylem | Microsoft.DataFactory/factories/getDataPlaneAccess/read | ADF ile veri düzlemi hizmet okuma erişimi. |
> | Eylem | Microsoft.DataFactory/factories/getFeatureValue/action | Belirli bir konuma Etkilenme denetim özelliği değerini alın. |
> | Eylem | Microsoft.DataFactory/factories/getFeatureValue/read | Belirli bir konuma için açığa denetim özelliği değerini okur. |
> | Eylem | Microsoft.DataFactory/factories/getGitHubAccessToken/action | GitHub erişim belirtecini alır. |
> | Eylem | Microsoft.DataFactory/factories/integrationruntimes/delete | Hiçbir tümleştirme çalışma zamanı siler. |
> | Eylem | Microsoft.DataFactory/factories/integrationruntimes/getconnectioninfo/read | Tümleştirme çalışma zamanı bağlantı bilgilerini okur. |
> | Eylem | Microsoft.DataFactory/factories/integrationruntimes/getObjectMetadata/action | Belirtilen tümleştirme çalışma zamanı için SSIS tümleştirme çalışma zamanı meta verileri alın. |
> | Eylem | Microsoft.DataFactory/factories/integrationruntimes/getstatus/read | Tümleştirme çalışma zamanı durumunu okur. |
> | Eylem | Microsoft.DataFactory/factories/integrationruntimes/linkedIntegrationRuntime/action | Bağlantılı tümleştirme çalışma zamanı başvurusu, belirtilen paylaşılan tümleştirme çalışma zamanını oluşturun. |
> | Eylem | Microsoft.DataFactory/factories/integrationruntimes/listauthkeys/read | Hiçbir tümleştirme çalışma zamanı için kimlik doğrulaması anahtarlarını listeler. |
> | Eylem | Microsoft.DataFactory/factories/integrationruntimes/monitoringdata/read | Hiçbir tümleştirme çalışma zamanı için izleme verilerini alır. |
> | Eylem | Microsoft.DataFactory/factories/integrationruntimes/nodes/delete | İçin belirtilen Integration Runtime düğümü siler. |
> | Eylem | Microsoft.DataFactory/factories/integrationruntimes/nodes/ipAddress/action | Belirtilen düğümün Integration Runtime'nın IP adresini döndürür. |
> | Eylem | Microsoft.DataFactory/factories/integrationruntimes/nodes/read | İçin belirtilen Integration Runtime düğümü okur. |
> | Eylem | Microsoft.DataFactory/factories/integrationruntimes/nodes/write | Bir şirket içinde barındırılan Integration Runtime düğümü güncelleştirir. |
> | Eylem | Microsoft.DataFactory/factories/integrationruntimes/read | Hiçbir tümleştirme çalışma zamanı okur. |
> | Eylem | Microsoft.DataFactory/factories/integrationruntimes/refreshObjectMetadata/action | Belirtilen tümleştirme çalışma zamanı için SSIS tümleştirme çalışma zamanı meta verileri yenileyin. |
> | Eylem | Microsoft.DataFactory/factories/integrationruntimes/regenerateauthkey/action | Belirtilen tümleştirme çalışma zamanı için kimlik doğrulaması anahtarlarını oluşturur. |
> | Eylem | Microsoft.DataFactory/factories/integrationruntimes/removelinks/action | Belirtilen tümleştirme çalışma zamanını şuradan bağlı tümleştirme çalışma zamanı başvuruları kaldırır. |
> | Eylem | Microsoft.DataFactory/factories/integrationruntimes/start/action | Hiçbir tümleştirme çalışma zamanı'nı başlatır. |
> | Eylem | Microsoft.DataFactory/factories/integrationruntimes/stop/action | Hiçbir tümleştirme çalışma zamanı durdurur. |
> | Eylem | Microsoft.DataFactory/factories/integrationruntimes/synccredentials/action | Belirtilen tümleştirme çalışma zamanı için kimlik bilgilerini eşitler. |
> | Eylem | Microsoft.DataFactory/factories/integrationruntimes/upgrade/action | Belirtilen tümleştirme çalışma zamanı yükseltir. |
> | Eylem | Microsoft.DataFactory/factories/integrationruntimes/write | Oluşturur veya hiçbir tümleştirme çalışma zamanı güncelleştirir. |
> | Eylem | Microsoft.DataFactory/factories/linkedServices/delete | Bağlı hizmeti siler. |
> | Eylem | Microsoft.DataFactory/factories/linkedServices/read | Bağlı hizmeti okur. |
> | Eylem | Microsoft.DataFactory/factories/linkedServices/write | Bağlı hizmet oluştur veya güncelleştir |
> | Eylem | Microsoft.DataFactory/factories/pipelineruns/activityruns/read | Çalıştırma kimliği için belirtilen işlem hattı çalıştırmalarını okur |
> | Eylem | Microsoft.DataFactory/factories/pipelineruns/cancel/action | İşlem hattı çalıştırma kimliği ile belirtilen çalıştırmasını iptal eder |
> | Eylem | Microsoft.DataFactory/factories/pipelineruns/queryactivityruns/action | Kimliği için belirtilen işlem hattı çalıştırmalarını sorguları çalıştırma |
> | Eylem | Microsoft.DataFactory/factories/pipelineruns/queryactivityruns/read | Sorgu etkinliği sonucu kimliği belirtilen işlem hattı çalıştırması için çalışan okur |
> | Eylem | Microsoft.DataFactory/factories/pipelineruns/read | İşlem hattı çalıştırmaları okur. |
> | Eylem | Microsoft.DataFactory/factories/pipelines/createrun/action | Bir işlem hattı oluşturur. |
> | Eylem | Microsoft.DataFactory/factories/pipelines/delete | İşlem hattı siler. |
> | Eylem | Microsoft.DataFactory/factories/pipelines/pipelineruns/activityruns/progress/read | Etkinlik çalıştırmalarını ilerlemesini alır. |
> | Eylem | Microsoft.DataFactory/factories/pipelines/pipelineruns/read | İşlem hattı çalıştırmasını okur. |
> | Eylem | Microsoft.DataFactory/factories/pipelines/read | İşlem hattı okur. |
> | Eylem | Microsoft.DataFactory/factories/pipelines/write | İşlem hattı güncelle |
> | Eylem | Microsoft.DataFactory/factories/querydebugpipelineruns/action | Hata ayıklama işlem hattı çalıştırmaları sorgular. |
> | Eylem | Microsoft.DataFactory/factories/querypipelineruns/action | İşlem hattı çalıştırmaları sorgular. |
> | Eylem | Microsoft.DataFactory/factories/querypipelineruns/read | İşlem hattı çalıştırmaları sorgu sonucunu okur. |
> | Eylem | Microsoft.DataFactory/factories/querytriggerruns/action | Tetikleyici çalıştırmaları sorgular. |
> | Eylem | Microsoft.DataFactory/factories/querytriggerruns/read | Tetikleyici çalıştırmaları sonucunu okur. |
> | Eylem | Microsoft.DataFactory/factories/read | Veri Fabrikası okur. |
> | Eylem | Microsoft.DataFactory/factories/startdataflowdebugsession/action | Bir veri akışı hata ayıklama oturumu başlatır. |
> | Eylem | Microsoft.DataFactory/factories/triggerruns/read | Tetikleyici çalıştırmaları okur. |
> | Eylem | Microsoft.DataFactory/factories/triggers/delete | Herhangi bir tetikleyici siler. |
> | Eylem | Microsoft.DataFactory/factories/triggers/read | Herhangi bir tetikleyici okur. |
> | Eylem | Microsoft.DataFactory/factories/triggers/start/action | Herhangi bir tetikleyici başlatır. |
> | Eylem | Microsoft.DataFactory/factories/triggers/stop/action | Herhangi bir tetikleyici durdurur. |
> | Eylem | Microsoft.DataFactory/factories/triggers/triggerruns/read | Tetikleyici çalıştırmaları okur. |
> | Eylem | Microsoft.DataFactory/factories/triggers/write | Oluşturur veya herhangi bir tetikleyici güncelleştirir. |
> | Eylem | Microsoft.DataFactory/factories/write | Veri Fabrikası güncelle |
> | Eylem | Microsoft.DataFactory/locations/configureFactoryRepo/action | Depo Fabrika için yapılandırır. |
> | Eylem | Microsoft.DataFactory/locations/getFeatureValue/action | Belirli bir konuma Etkilenme denetim özelliği değerini alın. |
> | Eylem | Microsoft.DataFactory/locations/getFeatureValue/read | Belirli bir konuma için açığa denetim özelliği değerini okur. |
> | Eylem | Microsoft.DataFactory/operations/read | Microsoft Data Factory sağlayıcısını tüm işlemleri okur. |
> | Eylem | Microsoft.DataFactory/register/action | Data Factory kaynak sağlayıcısı için aboneliği kaydeder. |
> | Eylem | Microsoft.DataFactory/unregister/action | Data Factory kaynak sağlayıcısı için abonelik kaydını siler. |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.DataLakeAnalytics/accounts/computePolicies/delete | Bir işlem ilkeyi silin. |
> | Eylem | Microsoft.DataLakeAnalytics/accounts/computePolicies/read | Bir işlem İlkesi hakkında bilgi alın. |
> | Eylem | Microsoft.DataLakeAnalytics/accounts/computePolicies/write | Veya bir işlem İlkesi güncelleştirilemiyor. |
> | Eylem | Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/delete | Bir DataLakeAnalytics hesabından DataLakeStore hesabı bağlantısını. |
> | Eylem | Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/read | DataLakeAnalytics hesabın bağlı DataLakeStore hesabı hakkında bilgi alın. |
> | Eylem | Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/write | Veya DataLakeAnalytics hesabın bağlı DataLakeStore hesabı güncelleştirilemiyor. |
> | Eylem | Microsoft.DataLakeAnalytics/accounts/delete | DataLakeAnalytics hesabı silin. |
> | Eylem | Microsoft.DataLakeAnalytics/accounts/firewallRules/delete | Bir güvenlik duvarı kuralını silin. |
> | Eylem | Microsoft.DataLakeAnalytics/accounts/firewallRules/read | Bir güvenlik duvarı kuralı hakkında bilgi alın. |
> | Eylem | Microsoft.DataLakeAnalytics/accounts/firewallRules/write | Veya bir güvenlik duvarı kuralı güncelleştirilemiyor. |
> | Eylem | Microsoft.DataLakeAnalytics/accounts/operationResults/read | DataLakeAnalytics hesabı işleminin sonucunu Al |
> | Eylem | Microsoft.DataLakeAnalytics/accounts/read | DataLakeAnalytics hesabınız hakkında bilgi alın. |
> | Eylem | Microsoft.DataLakeAnalytics/accounts/storageAccounts/Containers/listSasTokens/action | SAS belirteçleri DataLakeAnalytics hesabının bağlantılı bir depolama hesabının depolama kapsayıcıları için listeler. |
> | Eylem | Microsoft.DataLakeAnalytics/accounts/storageAccounts/Containers/read | Bağlantılı bir depolama hesabının DataLakeAnalytics hesabının kapsayıcıları alın. |
> | Eylem | Microsoft.DataLakeAnalytics/accounts/storageAccounts/delete | Bir depolama hesabından bir DataLakeAnalytics hesabı bağlantısını. |
> | Eylem | Microsoft.DataLakeAnalytics/accounts/storageAccounts/read | DataLakeAnalytics hesabın bağlı bir depolama hesabı hakkında bilgi alın. |
> | Eylem | Microsoft.DataLakeAnalytics/accounts/storageAccounts/write | Veya bağlı bir depolama hesabı DataLakeAnalytics hesabının güncelleştirilemiyor. |
> | Eylem | Microsoft.DataLakeAnalytics/accounts/TakeOwnership/action | Diğer kullanıcılar tarafından gönderilen bir iş iptal etmek için izinleri verin. |
> | Eylem | Microsoft.DataLakeAnalytics/accounts/transferAnalyticsUnits/action | SystemMaxAnalyticsUnits DataLakeAnalytics hesapları arasında aktarın. |
> | Eylem | Microsoft.DataLakeAnalytics/accounts/virtualNetworkRules/delete | Bir sanal ağ kuralı. |
> | Eylem | Microsoft.DataLakeAnalytics/accounts/virtualNetworkRules/read | Bir sanal ağ kuralı hakkında bilgi alın. |
> | Eylem | Microsoft.DataLakeAnalytics/accounts/virtualNetworkRules/write | Veya bir sanal ağ kuralı güncelleştirilemiyor. |
> | Eylem | Microsoft.DataLakeAnalytics/accounts/write | Veya bir DataLakeAnalytics hesabı güncelleştirilemiyor. |
> | Eylem | Microsoft.DataLakeAnalytics/locations/capability/read | Bir aboneliğin yeteneği bilgileri alma DataLakeAnalytics kullanma ile ilgili. |
> | Eylem | Microsoft.DataLakeAnalytics/locations/checkNameAvailability/action | DataLakeAnalytics hesap adının kullanılabilirliğini kontrol edin. |
> | Eylem | Microsoft.DataLakeAnalytics/locations/operationResults/read | DataLakeAnalytics hesabı işleminin sonucunu Al |
> | Eylem | Microsoft.DataLakeAnalytics/locations/usages/read | Kota kullanımları bir abonelik almak DataLakeAnalytics kullanma ile ilgili. |
> | Eylem | Microsoft.DataLakeAnalytics/operations/read | DataLakeAnalytics kullanılabilir işlemleri alın. |
> | Eylem | Microsoft.DataLakeAnalytics/register/action | DataLakeAnalytics aboneliğine kaydolun. |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.DataLakeStore/accounts/delete | DataLakeStore hesabı silin. |
> | Eylem | Microsoft.DataLakeStore/accounts/enableKeyVault/action | KeyVault DataLakeStore hesabı için etkinleştirin. |
> | Eylem | Microsoft.DataLakeStore/accounts/eventGridFilters/delete | EventGrid filtre silin. |
> | Eylem | Microsoft.DataLakeStore/accounts/eventGridFilters/read | EventGrid filtre alın. |
> | Eylem | Microsoft.DataLakeStore/accounts/eventGridFilters/write | Veya bir EventGrid filtresi güncelleştirilemiyor. |
> | Eylem | Microsoft.DataLakeStore/accounts/firewallRules/delete | Bir güvenlik duvarı kuralını silin. |
> | Eylem | Microsoft.DataLakeStore/accounts/firewallRules/read | Bir güvenlik duvarı kuralı hakkında bilgi alın. |
> | Eylem | Microsoft.DataLakeStore/accounts/firewallRules/write | Veya bir güvenlik duvarı kuralı güncelleştirilemiyor. |
> | Eylem | Microsoft.DataLakeStore/accounts/operationResults/read | DataLakeStore hesabı işleminin sonucunu Al |
> | Eylem | Microsoft.DataLakeStore/accounts/read | DataLakeStore hesabınız hakkında bilgi alın. |
> | Eylem | Microsoft.DataLakeStore/accounts/Superuser/action | Data Lake Store ile Microsoft.Authorization/roleAssignments/write izin verildiğinde üzerinde süper kullanıcı atayın. |
> | Eylem | Microsoft.DataLakeStore/accounts/trustedIdProviders/delete | Güvenilen kimlik sağlayıcı silin. |
> | Eylem | Microsoft.DataLakeStore/accounts/trustedIdProviders/read | Güvenilen kimlik sağlayıcı hakkında bilgi alın. |
> | Eylem | Microsoft.DataLakeStore/accounts/trustedIdProviders/write | Oluşturun veya güvenilir bir kimlik sağlayıcısını güncelleştirin. |
> | Eylem | Microsoft.DataLakeStore/accounts/virtualNetworkRules/delete | Bir sanal ağ kuralı. |
> | Eylem | Microsoft.DataLakeStore/accounts/virtualNetworkRules/read | Bir sanal ağ kuralı hakkında bilgi alın. |
> | Eylem | Microsoft.DataLakeStore/accounts/virtualNetworkRules/write | Veya bir sanal ağ kuralı güncelleştirilemiyor. |
> | Eylem | Microsoft.DataLakeStore/accounts/write | Veya bir DataLakeStore hesabı güncelleştirilemiyor. |
> | Eylem | Microsoft.DataLakeStore/locations/capability/read | Bir aboneliğin yeteneği bilgileri alma DataLakeStore kullanma ile ilgili. |
> | Eylem | Microsoft.DataLakeStore/locations/checkNameAvailability/action | DataLakeStore hesap adının kullanılabilirliğini kontrol edin. |
> | Eylem | Microsoft.DataLakeStore/locations/operationResults/read | DataLakeStore hesabı işleminin sonucunu Al |
> | Eylem | Microsoft.DataLakeStore/locations/usages/read | Kota kullanımları bir abonelik almak DataLakeStore kullanma ile ilgili. |
> | Eylem | Microsoft.DataLakeStore/operations/read | DataLakeStore kullanılabilir işlemleri alın. |
> | Eylem | Microsoft.DataLakeStore/register/action | DataLakeStore aboneliğine kaydolun. |

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.DataMigration/locations/operationResults/read | Bir 202 kabul edildi yanıtı ile ilgili bir uzun süre çalışan işlemin durumunu alın |
> | Eylem | Microsoft.DataMigration/locations/operationStatuses/read | Bir 202 kabul edildi yanıtı ile ilgili bir uzun süre çalışan işlemin durumunu alın |
> | Eylem | Microsoft.DataMigration/register/action | Aboneliği için Azure veritabanı geçiş hizmet sağlayıcısına kaydeder |
> | Eylem | Microsoft.DataMigration/services/addWorker/action | DMS çalışan hizmetin kullanılabilir çalışanlarına ekler. |
> | Eylem | Microsoft.DataMigration/services/checkStatus/action | Hizmetin dağıtılmış ve çalışır durumda olup olmadığını denetleyin |
> | Eylem | Microsoft.DataMigration/services/configureWorker/action | Bir hizmetin kullanılabilir çalışanlar için DMS çalışan yapılandırır |
> | Eylem | Microsoft.DataMigration/services/delete | Bir kaynağı ve tüm alt öğelerini siler |
> | Eylem | Microsoft.DataMigration/services/projects/accessArtifacts/action | GET veya PUT proje yapıtları için kullanılabilecek bir URL oluşturun |
> | Eylem | Microsoft.DataMigration/services/projects/delete | Bir kaynağı ve tüm alt öğelerini siler |
> | Eylem | Microsoft.DataMigration/services/projects/files/delete | Bir kaynağı ve tüm alt öğelerini siler |
> | Eylem | Microsoft.DataMigration/services/projects/files/read | Kaynaklar hakkında bilgi edinin |
> | Eylem | Microsoft.DataMigration/services/projects/files/read/action | Dosyanın içeriğini okumak için kullanılabilecek bir URL elde |
> | Eylem | Microsoft.DataMigration/services/projects/files/readWrite/action | Okumak veya dosyanın içeriğini yazmak için kullanılabilecek bir URL elde |
> | Eylem | Microsoft.DataMigration/services/projects/files/write | Kaynakları ve bunların özelliklerini oluştur veya güncelleştir |
> | Eylem | Microsoft.DataMigration/services/projects/read | Kaynaklar hakkında bilgi edinin |
> | Eylem | Microsoft.DataMigration/services/projects/tasks/cancel/action | Şu anda çalışıyorsa görevi iptal etme |
> | Eylem | Microsoft.DataMigration/services/projects/tasks/delete | Bir kaynağı ve tüm alt öğelerini siler |
> | Eylem | Microsoft.DataMigration/services/projects/tasks/read | Kaynaklar hakkında bilgi edinin |
> | Eylem | Microsoft.DataMigration/services/projects/tasks/write | Azure veritabanı geçiş hizmeti görevleri görevleri çalıştırma |
> | Eylem | Microsoft.DataMigration/services/projects/write | Azure veritabanı geçiş hizmeti görevleri görevleri çalıştırma |
> | Eylem | Microsoft.DataMigration/services/read | Kaynaklar hakkında bilgi edinin |
> | Eylem | Microsoft.DataMigration/services/removeWorker/action | Bir hizmetin kullanılabilir çalışanlar için DMS çalışanı kaldırır |
> | Eylem | Microsoft.DataMigration/services/serviceTasks/cancel/action | Şu anda çalışıyorsa görevi iptal etme |
> | Eylem | Microsoft.DataMigration/services/serviceTasks/delete | Bir kaynağı ve tüm alt öğelerini siler |
> | Eylem | Microsoft.DataMigration/services/serviceTasks/read | Kaynaklar hakkında bilgi edinin |
> | Eylem | Microsoft.DataMigration/services/serviceTasks/write | Azure veritabanı geçiş hizmeti görevleri görevleri çalıştırma |
> | Eylem | Microsoft.DataMigration/services/slots/delete | Bir kaynağı ve tüm alt öğelerini siler |
> | Eylem | Microsoft.DataMigration/services/slots/read | Kaynaklar hakkında bilgi edinin |
> | Eylem | Microsoft.DataMigration/services/slots/write | Kaynakları ve bunların özelliklerini oluştur veya güncelleştir |
> | Eylem | Microsoft.DataMigration/services/start/action | Geçişleri yeniden işlemesine izin vermek için DMS hizmetini başlatın |
> | Eylem | Microsoft.DataMigration/services/stop/action | Maliyetlerini en aza indirmek için DMS hizmetini durdurun |
> | Eylem | Microsoft.DataMigration/services/updateAgentConfig/action | DMS aracı yapılandırması ile sağlanan değerlerini güncelleştirir. |
> | Eylem | Microsoft.DataMigration/services/write | Kaynakları ve bunların özelliklerini oluştur veya güncelleştir |
> | Eylem | Microsoft.DataMigration/skus/read | DMS kaynakları tarafından desteklenen SKU'ların bir listesini alın. |

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.DBforMariaDB/checkNameAvailability/action | Sunucu adı belirli bir aboneliğe yönelik dünya çapında sağlamak için kullanılabilir olup olmadığını doğrulayın. |
> | Eylem | Microsoft.DBforMariaDB/locations/azureAsyncOperation/read | MariaDB sunucu işlemi sonuçlarını döndürür |
> | Eylem | Microsoft.DBforMariaDB/locations/operationResults/read | Dönüş ResourceGroup MariaDB sunucu işlemi sonuçlarını alan |
> | Eylem | Microsoft.DBforMariaDB/locations/operationResults/read | MariaDB sunucu işlemi sonuçlarını döndürür |
> | Eylem | Microsoft.DBforMariaDB/locations/performanceTiers/read | Performans katmanları kullanılabilir listesini döndürür. |
> | Eylem | Microsoft.DBforMariaDB/locations/securityAlertPoliciesAzureAsyncOperation/read | Sunucu tehdit algılama işleminin sonucunu listesini döndürür. |
> | Eylem | Microsoft.DBforMariaDB/locations/securityAlertPoliciesOperationResults/read | Sunucu tehdit algılama işleminin sonucunu listesini döndürür. |
> | Eylem | Microsoft.DBforMariaDB/operations/read | MariaDB işlemlerin listesini döndürür. |
> | Eylem | Microsoft.DBforMariaDB/performanceTiers/read | Performans katmanları kullanılabilir listesini döndürür. |
> | Eylem | Microsoft.DBforMariaDB/register/action | MariaDB kaynak sağlayıcısını kaydetme |
> | Eylem | Microsoft.DBforMariaDB/servers/administrators/delete | Mevcut yönetici MariaDB sunucusunun siler. |
> | Eylem | Microsoft.DBforMariaDB/servers/administrators/read | MariaDB Sunucu yöneticilerinin listesini alır. |
> | Eylem | Microsoft.DBforMariaDB/servers/administrators/write | Oluşturur veya MariaDB yöneticisine belirtilen parametrelerle güncelleştirir. |
> | Eylem | Microsoft.DBforMariaDB/servers/advisors/createRecommendedActionSession/action | Yeni bir öneri eylem oturumu oluşturma |
> | Eylem | Microsoft.DBforMariaDB/servers/advisors/read | Danışmanlar listesini döndürür |
> | Eylem | Microsoft.DBforMariaDB/servers/advisors/read | Danışman'ı döndürür |
> | Eylem | Microsoft.DBforMariaDB/servers/advisors/recommendedActions/read | Önerilen Eylemler listesini döndürür |
> | Eylem | Microsoft.DBforMariaDB/servers/advisors/recommendedActions/read | Önerilen Eylemler listesini döndürür |
> | Eylem | Microsoft.DBforMariaDB/servers/advisors/recommendedActions/read | Önerilen eylem döndürme |
> | Eylem | Microsoft.DBforMariaDB/servers/configurations/read | Belirtilen yapılandırma özelliklerini alır ya da bir sunucu için yapılandırmaları listesini döndürür. |
> | Eylem | Microsoft.DBforMariaDB/servers/configurations/write | Belirtilen yapılandırma değerini güncelleştirin |
> | Eylem | Microsoft.DBforMariaDB/servers/databases/delete | Mevcut bir MariaDB veritabanı siler. |
> | Eylem | Microsoft.DBforMariaDB/servers/databases/read | Belirtilen veritabanı için MariaDB veritabanı veya özelliklerini alır listesini döndürür. |
> | Eylem | Microsoft.DBforMariaDB/servers/databases/write | Belirtilen parametrelerle bir MariaDB veritabanı oluşturur veya için belirtilen veritabanı özelliklerini güncelleştirir. |
> | Eylem | Microsoft.DBforMariaDB/servers/delete | Mevcut bir sunucu siler. |
> | Eylem | Microsoft.DBforMariaDB/servers/firewallRules/delete | Mevcut bir güvenlik duvarı kuralını siler. |
> | Eylem | Microsoft.DBforMariaDB/servers/firewallRules/read | Güvenlik Duvarı listesini bir sunucu için kuralları veya özellikleri için belirtilen güvenlik duvarı kuralı alır döndürür. |
> | Eylem | Microsoft.DBforMariaDB/servers/firewallRules/write | Belirtilen parametreleri ya da mevcut bir kuralı güncelleştirme güvenlik duvarı kuralı oluşturur. |
> | Eylem | Microsoft.DBforMariaDB/servers/logFiles/read | MariaDB LogFiles listesini döndürür. |
> | Eylem | Microsoft.DBforMariaDB/servers/providers/Microsoft.Insights/diagnosticSettings/read | Kaynağın disagnostic ayarını alır |
> | Eylem | Microsoft.DBforMariaDB/servers/providers/Microsoft.Insights/diagnosticSettings/write | Kaynağın tanılama ayarını oluşturur veya güncelleştirir |
> | Eylem | Microsoft.DBforMariaDB/servers/providers/Microsoft.Insights/logDefinitions/read | MariaDB sunucuları için kullanılabilir günlükleri alır |
> | Eylem | Microsoft.DBforMariaDB/servers/providers/Microsoft.Insights/metricDefinitions/read | Dönüş türleri veritabanları için kullanılabilir ölçümleri |
> | Eylem | Microsoft.DBforMariaDB/servers/queryTexts/action | Metinleri sorgularının listesini döndürür |
> | Eylem | Microsoft.DBforMariaDB/servers/queryTexts/action | Sorgu metni döndürün |
> | Eylem | Microsoft.DBforMariaDB/servers/read | Sunucuları veya belirtilen sunucunun özelliklerini alır listesini döndürür. |
> | Eylem | Microsoft.DBforMariaDB/servers/recoverableServers/read | Kurtarılabilir MariaDB sunucu bilgileri |
> | Eylem | Microsoft.DBforMariaDB/servers/replicas/read | MariaDB sunucusunun çoğaltmalarını salt okunur |
> | Eylem | Microsoft.DBforMariaDB/servers/restart/action | Belirli bir sunucu yeniden başlatılır. |
> | Eylem | Microsoft.DBforMariaDB/servers/securityAlertPolicies/read | Belirli bir sunucuda yapılandırılmış sunucu tehdit algılama ilkesi ayrıntılarını alma |
> | Eylem | Microsoft.DBforMariaDB/servers/securityAlertPolicies/write | Verilen bir sunucu için sunucu tehdit algılama ilkesini değiştirme |
> | Eylem | Microsoft.DBforMariaDB/servers/topQueryStatistics/read | En sık kullanılan sorgular için sorgu istatistikleri listesini döndürür. |
> | Eylem | Microsoft.DBforMariaDB/servers/topQueryStatistics/read | Bir sorgu istatistiği döndürür |
> | Eylem | Microsoft.DBforMariaDB/servers/updateConfigurations/action | Belirtilen sunucu için yapılandırmaları güncelleştirme |
> | Eylem | Microsoft.DBforMariaDB/servers/virtualNetworkRules/delete | Mevcut bir sanal ağ kuralı siler |
> | Eylem | Microsoft.DBforMariaDB/servers/virtualNetworkRules/read | Dönüş listesi, sanal ağ kuralları veya belirtilen sanal ağ kuralı için özellikleri alır. |
> | Eylem | Microsoft.DBforMariaDB/servers/virtualNetworkRules/write | Bir sanal ağ kuralı belirtilen parametrelerle oluşturur veya özellikleri veya etiketleri belirtilen sanal ağ kuralı için güncelleştirin. |
> | Eylem | Microsoft.DBforMariaDB/servers/waitStatistics/read | Bir örneği için bekleme İstatistiği |
> | Eylem | Microsoft.DBforMariaDB/servers/waitStatistics/read | Bekleme istatistikleri döndürür |
> | Eylem | Microsoft.DBforMariaDB/servers/write | Belirtilen parametrelerle bir sunucu oluşturur veya özellikleri veya etiketleri belirtilen sunucu için güncelleştirin. |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.DBforMySQL/checkNameAvailability/action | Sunucu adı belirli bir aboneliğe yönelik dünya çapında sağlamak için kullanılabilir olup olmadığını doğrulayın. |
> | Eylem | Microsoft.DBforMySQL/locations/azureAsyncOperation/read | MySQL sunucu işlemi sonuçlarını döndürür |
> | Eylem | Microsoft.DBforMySQL/locations/operationResults/read | MySQL sunucu işlemi sonuçlarını dönüş ResourceGroup tabanlı |
> | Eylem | Microsoft.DBforMySQL/locations/operationResults/read | MySQL sunucu işlemi sonuçlarını döndürür |
> | Eylem | Microsoft.DBforMySQL/locations/performanceTiers/read | Performans katmanları kullanılabilir listesini döndürür. |
> | Eylem | Microsoft.DBforMySQL/locations/securityAlertPoliciesAzureAsyncOperation/read | Sunucu tehdit algılama işleminin sonucunu listesini döndürür. |
> | Eylem | Microsoft.DBforMySQL/locations/securityAlertPoliciesOperationResults/read | Sunucu tehdit algılama işleminin sonucunu listesini döndürür. |
> | Eylem | Microsoft.DBforMySQL/operations/read | MySQL işlemlerin listesini döndürür. |
> | Eylem | Microsoft.DBforMySQL/performanceTiers/read | Performans katmanları kullanılabilir listesini döndürür. |
> | Eylem | Microsoft.DBforMySQL/register/action | MySQL kaynak sağlayıcısını kaydetme |
> | Eylem | Microsoft.DBforMySQL/servers/administrators/delete | MySQL server'ın mevcut bir Yöneticisi siler. |
> | Eylem | Microsoft.DBforMySQL/servers/administrators/read | MySQL sunucusu yöneticilerinin listesini alır. |
> | Eylem | Microsoft.DBforMySQL/servers/administrators/write | Oluşturur veya MySQL server Yöneticisi belirtilen parametrelerle güncelleştirir. |
> | Eylem | Microsoft.DBforMySQL/servers/advisors/createRecommendedActionSession/action | Yeni bir öneri eylem oturumu oluşturma |
> | Eylem | Microsoft.DBforMySQL/servers/advisors/read | Danışmanlar listesini döndürür |
> | Eylem | Microsoft.DBforMySQL/servers/advisors/read | Danışman'ı döndürür |
> | Eylem | Microsoft.DBforMySQL/servers/advisors/recommendedActions/read | Önerilen Eylemler listesini döndürür |
> | Eylem | Microsoft.DBforMySQL/servers/advisors/recommendedActions/read | Önerilen Eylemler listesini döndürür |
> | Eylem | Microsoft.DBforMySQL/servers/advisors/recommendedActions/read | Önerilen eylem döndürme |
> | Eylem | Microsoft.DBforMySQL/servers/configurations/read | Belirtilen yapılandırma özelliklerini alır ya da bir sunucu için yapılandırmaları listesini döndürür. |
> | Eylem | Microsoft.DBforMySQL/servers/configurations/write | Belirtilen yapılandırma değerini güncelleştirin |
> | Eylem | Microsoft.DBforMySQL/servers/databases/delete | Mevcut bir MySQL veritabanını siler. |
> | Eylem | Microsoft.DBforMySQL/servers/databases/read | Belirtilen veritabanı için MySQL veritabanları veya özelliklerini alır listesini döndürür. |
> | Eylem | Microsoft.DBforMySQL/servers/databases/write | Belirtilen parametrelerle bir MySQL veritabanı oluşturur veya için belirtilen veritabanı özelliklerini güncelleştirir. |
> | Eylem | Microsoft.DBforMySQL/servers/delete | Mevcut bir sunucu siler. |
> | Eylem | Microsoft.DBforMySQL/servers/firewallRules/delete | Mevcut bir güvenlik duvarı kuralını siler. |
> | Eylem | Microsoft.DBforMySQL/servers/firewallRules/read | Güvenlik Duvarı listesini bir sunucu için kuralları veya özellikleri için belirtilen güvenlik duvarı kuralı alır döndürür. |
> | Eylem | Microsoft.DBforMySQL/servers/firewallRules/write | Belirtilen parametreleri ya da mevcut bir kuralı güncelleştirme güvenlik duvarı kuralı oluşturur. |
> | Eylem | Microsoft.DBforMySQL/servers/logFiles/read | PostgreSQL LogFiles listesini döndürür. |
> | Eylem | Microsoft.DBforMySQL/servers/providers/Microsoft.Insights/diagnosticSettings/read | Kaynağın disagnostic ayarını alır |
> | Eylem | Microsoft.DBforMySQL/servers/providers/Microsoft.Insights/diagnosticSettings/write | Kaynağın tanılama ayarını oluşturur veya güncelleştirir |
> | Eylem | Microsoft.DBforMySQL/servers/providers/Microsoft.Insights/logDefinitions/read | MySQL sunucuları için kullanılabilir günlükleri alır |
> | Eylem | Microsoft.DBforMySQL/servers/providers/Microsoft.Insights/metricDefinitions/read | Dönüş türleri veritabanları için kullanılabilir ölçümleri |
> | Eylem | Microsoft.DBforMySQL/servers/queryTexts/action | Metinleri sorgularının listesini döndürür |
> | Eylem | Microsoft.DBforMySQL/servers/queryTexts/action | Sorgu metni döndürün |
> | Eylem | Microsoft.DBforMySQL/servers/read | Sunucuları veya belirtilen sunucunun özelliklerini alır listesini döndürür. |
> | Eylem | Microsoft.DBforMySQL/servers/recoverableServers/read | Kurtarılabilir MySQL sunucusu bilgileri |
> | Eylem | Microsoft.DBforMySQL/servers/replicas/read | Bir MySQL sunucusunun çoğaltmalarını salt okunur |
> | Eylem | Microsoft.DBforMySQL/servers/restart/action | Belirli bir sunucu yeniden başlatılır. |
> | Eylem | Microsoft.DBforMySQL/servers/securityAlertPolicies/read | Belirli bir sunucuda yapılandırılmış sunucu tehdit algılama ilkesi ayrıntılarını alma |
> | Eylem | Microsoft.DBforMySQL/servers/securityAlertPolicies/write | Verilen bir sunucu için sunucu tehdit algılama ilkesini değiştirme |
> | Eylem | Microsoft.DBforMySQL/servers/topQueryStatistics/read | En sık kullanılan sorgular için sorgu istatistikleri listesini döndürür. |
> | Eylem | Microsoft.DBforMySQL/servers/topQueryStatistics/read | Bir sorgu istatistiği döndürür |
> | Eylem | Microsoft.DBforMySQL/servers/updateConfigurations/action | Belirtilen sunucu için yapılandırmaları güncelleştirme |
> | Eylem | Microsoft.DBforMySQL/servers/virtualNetworkRules/delete | Mevcut bir sanal ağ kuralı siler |
> | Eylem | Microsoft.DBforMySQL/servers/virtualNetworkRules/read | Dönüş listesi, sanal ağ kuralları veya belirtilen sanal ağ kuralı için özellikleri alır. |
> | Eylem | Microsoft.DBforMySQL/servers/virtualNetworkRules/write | Bir sanal ağ kuralı belirtilen parametrelerle oluşturur veya özellikleri veya etiketleri belirtilen sanal ağ kuralı için güncelleştirin. |
> | Eylem | Microsoft.DBforMySQL/servers/waitStatistics/read | Bir örneği için bekleme İstatistiği |
> | Eylem | Microsoft.DBforMySQL/servers/waitStatistics/read | Bekleme istatistikleri döndürür |
> | Eylem | Microsoft.DBforMySQL/servers/write | Belirtilen parametrelerle bir sunucu oluşturur veya özellikleri veya etiketleri belirtilen sunucu için güncelleştirin. |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.DBforPostgreSQL/checkNameAvailability/action | Sunucu adı belirli bir aboneliğe yönelik dünya çapında sağlamak için kullanılabilir olup olmadığını doğrulayın. |
> | Eylem | Microsoft.DBforPostgreSQL/locations/azureAsyncOperation/read | PostgreSQL sunucu işlemi sonuçlarını döndürür |
> | Eylem | Microsoft.DBforPostgreSQL/locations/operationResults/read | Dönüş ResourceGroup PostgreSQL sunucusu işlemi sonuçlarını alan |
> | Eylem | Microsoft.DBforPostgreSQL/locations/operationResults/read | PostgreSQL sunucu işlemi sonuçlarını döndürür |
> | Eylem | Microsoft.DBforPostgreSQL/locations/performanceTiers/read | Performans katmanları kullanılabilir listesini döndürür. |
> | Eylem | Microsoft.DBforPostgreSQL/locations/securityAlertPoliciesAzureAsyncOperation/read | Sunucu tehdit algılama işleminin sonucunu listesini döndürür. |
> | Eylem | Microsoft.DBforPostgreSQL/locations/securityAlertPoliciesOperationResults/read | Sunucu tehdit algılama işleminin sonucunu listesini döndürür. |
> | Eylem | Microsoft.DBforPostgreSQL/operations/read | PostgreSQL işlemlerin listesini döndürür. |
> | Eylem | Microsoft.DBforPostgreSQL/performanceTiers/read | Performans katmanları kullanılabilir listesini döndürür. |
> | Eylem | Microsoft.DBforPostgreSQL/register/action | PostgreSQL kaynak sağlayıcısını kaydetme |
> | Eylem | Microsoft.DBforPostgreSQL/servers/administrators/delete | PostgreSQL sunucusunun mevcut yönetici siler. |
> | Eylem | Microsoft.DBforPostgreSQL/servers/administrators/read | PostgreSQL sunucusu yöneticilerinin listesini alır. |
> | Eylem | Microsoft.DBforPostgreSQL/servers/administrators/write | PostgreSQL Sunucu Yöneticisi belirtilen parametrelerle güncelleştirir veya oluşturur. |
> | Eylem | Microsoft.DBforPostgreSQL/servers/advisors/read | Danışmanlar listesini döndürür |
> | Eylem | Microsoft.DBforPostgreSQL/servers/advisors/recommendedActions/read | Önerilen Eylemler listesini döndürür |
> | Eylem | Microsoft.DBforPostgreSQL/servers/advisors/recommendedActionSessions/action | Önerilerde |
> | Eylem | Microsoft.DBforPostgreSQL/servers/configurations/read | Belirtilen yapılandırma özelliklerini alır ya da bir sunucu için yapılandırmaları listesini döndürür. |
> | Eylem | Microsoft.DBforPostgreSQL/servers/configurations/write | Belirtilen yapılandırma değerini güncelleştirin |
> | Eylem | Microsoft.DBforPostgreSQL/servers/databases/delete | Mevcut bir PostgreSQL veritabanı siler. |
> | Eylem | Microsoft.DBforPostgreSQL/servers/databases/read | Belirtilen veritabanı için PostgreSQL veritabanları veya özelliklerini alır listesini döndürür. |
> | Eylem | Microsoft.DBforPostgreSQL/servers/databases/write | Bir PostgreSQL veritabanı belirtilen parametrelerle oluşturur veya için belirtilen veritabanı özelliklerini güncelleştirir. |
> | Eylem | Microsoft.DBforPostgreSQL/servers/delete | Mevcut bir sunucu siler. |
> | Eylem | Microsoft.DBforPostgreSQL/servers/firewallRules/delete | Mevcut bir güvenlik duvarı kuralını siler. |
> | Eylem | Microsoft.DBforPostgreSQL/servers/firewallRules/read | Güvenlik Duvarı listesini bir sunucu için kuralları veya özellikleri için belirtilen güvenlik duvarı kuralı alır döndürür. |
> | Eylem | Microsoft.DBforPostgreSQL/servers/firewallRules/write | Belirtilen parametreleri ya da mevcut bir kuralı güncelleştirme güvenlik duvarı kuralı oluşturur. |
> | Eylem | Microsoft.DBforPostgreSQL/servers/logFiles/read | PostgreSQL LogFiles listesini döndürür. |
> | Eylem | Microsoft.DBforPostgreSQL/servers/providers/Microsoft.Insights/diagnosticSettings/read | Kaynağın disagnostic ayarını alır |
> | Eylem | Microsoft.DBforPostgreSQL/servers/providers/Microsoft.Insights/diagnosticSettings/write | Kaynağın tanılama ayarını oluşturur veya güncelleştirir |
> | Eylem | Microsoft.DBforPostgreSQL/servers/providers/Microsoft.Insights/logDefinitions/read | Postgres sunucuları için kullanılabilir günlükleri alır |
> | Eylem | Microsoft.DBforPostgreSQL/servers/providers/Microsoft.Insights/metricDefinitions/read | Dönüş türleri veritabanları için kullanılabilir ölçümleri |
> | Eylem | Microsoft.DBforPostgreSQL/servers/queryTexts/action | Sorgu metni döndürün |
> | Eylem | Microsoft.DBforPostgreSQL/servers/queryTexts/read | Metinleri sorgularının listesini döndürür |
> | Eylem | Microsoft.DBforPostgreSQL/servers/read | Sunucuları veya belirtilen sunucunun özelliklerini alır listesini döndürür. |
> | Eylem | Microsoft.DBforPostgreSQL/servers/recoverableServers/read | Kurtarılabilir PostgreSQL sunucusu bilgileri |
> | Eylem | Microsoft.DBforPostgreSQL/servers/replicas/read | Bir PostgreSQL sunucusu çoğaltmalarının salt okunur |
> | Eylem | Microsoft.DBforPostgreSQL/servers/restart/action | Belirli bir sunucu yeniden başlatılır. |
> | Eylem | Microsoft.DBforPostgreSQL/servers/securityAlertPolicies/read | Belirli bir sunucuda yapılandırılmış sunucu tehdit algılama ilkesi ayrıntılarını alma |
> | Eylem | Microsoft.DBforPostgreSQL/servers/securityAlertPolicies/write | Verilen bir sunucu için sunucu tehdit algılama ilkesini değiştirme |
> | Eylem | Microsoft.DBforPostgreSQL/servers/topQueryStatistics/read | En sık kullanılan sorgular için sorgu istatistikleri listesini döndürür. |
> | Eylem | Microsoft.DBforPostgreSQL/servers/updateConfigurations/action | Belirtilen sunucu için yapılandırmaları güncelleştirme |
> | Eylem | Microsoft.DBforPostgreSQL/servers/virtualNetworkRules/delete | Mevcut bir sanal ağ kuralı siler |
> | Eylem | Microsoft.DBforPostgreSQL/servers/virtualNetworkRules/read | Dönüş listesi, sanal ağ kuralları veya belirtilen sanal ağ kuralı için özellikleri alır. |
> | Eylem | Microsoft.DBforPostgreSQL/servers/virtualNetworkRules/write | Bir sanal ağ kuralı belirtilen parametrelerle oluşturur veya özellikleri veya etiketleri belirtilen sanal ağ kuralı için güncelleştirin. |
> | Eylem | Microsoft.DBforPostgreSQL/servers/waitStatistics/read | Bir örneği için bekleme İstatistiği |
> | Eylem | Microsoft.DBforPostgreSQL/servers/write | Belirtilen parametrelerle bir sunucu oluşturur veya özellikleri veya etiketleri belirtilen sunucu için güncelleştirin. |
> | Eylem | Microsoft.DBforPostgreSQL/serversv2/configurations/read | Belirtilen yapılandırma özelliklerini alır ya da bir sunucu için yapılandırmaları listesini döndürür. |
> | Eylem | Microsoft.DBforPostgreSQL/serversv2/configurations/write | Belirtilen yapılandırma değerini güncelleştirin |
> | Eylem | Microsoft.DBforPostgreSQL/serversv2/delete | Mevcut bir sunucu siler. |
> | Eylem | Microsoft.DBforPostgreSQL/serversv2/firewallRules/delete | Mevcut bir güvenlik duvarı kuralını siler. |
> | Eylem | Microsoft.DBforPostgreSQL/serversv2/firewallRules/read | Güvenlik Duvarı listesini bir sunucu için kuralları veya özellikleri için belirtilen güvenlik duvarı kuralı alır döndürür. |
> | Eylem | Microsoft.DBforPostgreSQL/serversv2/firewallRules/write | Belirtilen parametreleri ya da mevcut bir kuralı güncelleştirme güvenlik duvarı kuralı oluşturur. |
> | Eylem | Microsoft.DBforPostgreSQL/serversv2/providers/Microsoft.Insights/diagnosticSettings/read | Kaynağın disagnostic ayarını alır |
> | Eylem | Microsoft.DBforPostgreSQL/serversv2/providers/Microsoft.Insights/diagnosticSettings/write | Kaynağın tanılama ayarını oluşturur veya güncelleştirir |
> | Eylem | Microsoft.DBforPostgreSQL/serversv2/providers/Microsoft.Insights/logDefinitions/read | Postgres sunucuları için kullanılabilir günlükleri alır |
> | Eylem | Microsoft.DBforPostgreSQL/serversv2/providers/Microsoft.Insights/metricDefinitions/read | Dönüş türleri veritabanları için kullanılabilir ölçümleri |
> | Eylem | Microsoft.DBforPostgreSQL/serversv2/read | Sunucuları veya belirtilen sunucunun özelliklerini alır listesini döndürür. |
> | Eylem | Microsoft.DBforPostgreSQL/serversv2/updateConfigurations/action | Belirtilen sunucu için yapılandırmaları güncelleştirme |
> | Eylem | Microsoft.DBforPostgreSQL/serversv2/write | Belirtilen parametrelerle bir sunucu oluşturur veya özellikleri veya etiketleri belirtilen sunucu için güncelleştirin. |

## <a name="microsoftdevices"></a>Microsoft.Devices

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Devices/checkNameAvailability/Action | Varsa Iothub adı kullanılabilir olup olmadığını denetleyin |
> | Eylem | Microsoft.Devices/checkProvisioningServiceNameAvailability/Action | Hizmet adı, sağlama kullanılabilir olup olmadığını denetleyin |
> | Eylem | Microsoft.Devices/ElasticPools/diagnosticSettings/read | Kaynağın tanılama ayarını alır |
> | Eylem | Microsoft.Devices/ElasticPools/diagnosticSettings/write | Kaynağın tanılama ayarını oluşturur veya güncelleştirir |
> | Eylem | Microsoft.Devices/elasticPools/iotHubTenants/certificates/Delete | Sertifika siler |
> | Eylem | Microsoft.Devices/elasticPools/iotHubTenants/certificates/generateVerificationCode/Action | Doğrulama kodu oluştur |
> | Eylem | Microsoft.Devices/elasticPools/iotHubTenants/certificates/Read | Sertifikayı alır |
> | Eylem | Microsoft.Devices/elasticPools/iotHubTenants/certificates/verify/Action | Sertifika kaynak doğrulayın |
> | Eylem | Microsoft.Devices/elasticPools/iotHubTenants/certificates/Write | Sertifika güncelle |
> | Eylem | Microsoft.Devices/elasticPools/iotHubTenants/Delete | Iothub Kiracı kaynağı silme |
> | Eylem | Microsoft.Devices/ElasticPools/IotHubTenants/diagnosticSettings/read | Kaynağın tanılama ayarını alır |
> | Eylem | Microsoft.Devices/ElasticPools/IotHubTenants/diagnosticSettings/write | Kaynağın tanılama ayarını oluşturur veya güncelleştirir |
> | Eylem | Microsoft.Devices/elasticPools/iotHubTenants/eventHubEndpoints/consumerGroups/Delete | EventHub tüketici grubunu sil |
> | Eylem | Microsoft.Devices/elasticPools/iotHubTenants/eventHubEndpoints/consumerGroups/Read | EventHub tüketici gruplarını Al |
> | Eylem | Microsoft.Devices/elasticPools/iotHubTenants/eventHubEndpoints/consumerGroups/Write | EventHub tüketici grubu oluşturun |
> | Eylem | Microsoft.Devices/elasticPools/iotHubTenants/exportDevices/Action | Cihazlar dışarı aktarma |
> | Eylem | Microsoft.Devices/elasticPools/iotHubTenants/getStats/Read | Iothub Kiracı istatistikleri kaynak alır |
> | Eylem | Microsoft.Devices/elasticPools/iotHubTenants/importDevices/Action | Cihazları İçeri Aktar |
> | Eylem | Microsoft.Devices/elasticPools/iotHubTenants/iotHubKeys/listkeys/Action | Iothub Kiracı anahtarını alır. |
> | Eylem | Microsoft.Devices/elasticPools/iotHubTenants/jobs/Read | İşi IotHub gönderdiniz ayrıntılarını Al |
> | Eylem | Microsoft.Devices/elasticPools/iotHubTenants/listKeys/Action | Iothub Kiracı anahtarlarını alır |
> | Eylem | Microsoft.Devices/ElasticPools/IotHubTenants/logDefinitions/read | Iothub hizmeti için kullanılabilir günlük tanımlarını alır |
> | Eylem | Microsoft.Devices/ElasticPools/IotHubTenants/metricDefinitions/read | Iothub hizmet için kullanılabilir ölçümleri alır |
> | Eylem | Microsoft.Devices/elasticPools/iotHubTenants/quotaMetrics/Read | Kota ölçümleri alın |
> | Eylem | Microsoft.Devices/elasticPools/iotHubTenants/Read | Iothub Kiracı kaynak alır |
> | Eylem | Microsoft.Devices/elasticPools/iotHubTenants/routing/routes/$testall/Action | Bir iletiye karşı mevcut tüm yolları test edin |
> | Eylem | Microsoft.Devices/elasticPools/iotHubTenants/routing/routes/$testnew/Action | Bir ileti sağlanan bir test rota karşı test etme |
> | Eylem | Microsoft.Devices/elasticPools/iotHubTenants/routingEndpointsHealth/Read | Bir IotHub için tüm yönlendirme uç noktaları durumunu alır |
> | Eylem | Microsoft.Devices/elasticPools/iotHubTenants/Write | Iothub Kiracı kaynak güncelle |
> | Eylem | Microsoft.Devices/ElasticPools/metricDefinitions/read | Iothub hizmet için kullanılabilir ölçümleri alır |
> | Eylem | Microsoft.Devices/iotHubs/certificates/Delete | Sertifika siler |
> | Eylem | Microsoft.Devices/iotHubs/certificates/generateVerificationCode/Action | Doğrulama kodu oluştur |
> | Eylem | Microsoft.Devices/iotHubs/certificates/Read | Sertifikayı alır |
> | Eylem | Microsoft.Devices/iotHubs/certificates/verify/Action | Sertifika kaynak doğrulayın |
> | Eylem | Microsoft.Devices/iotHubs/certificates/Write | Sertifika güncelle |
> | Eylem | Microsoft.Devices/iotHubs/Delete | Iothub'ı kaynağını Sil |
> | Eylem | Microsoft.Devices/IotHubs/diagnosticSettings/read | Kaynağın tanılama ayarını alır |
> | Eylem | Microsoft.Devices/IotHubs/diagnosticSettings/write | Kaynağın tanılama ayarını oluşturur veya güncelleştirir |
> | Eylem | Microsoft.Devices/iotHubs/eventGridFilters/Delete | Olay Kılavuzu filtresini siler |
> | Eylem | Microsoft.Devices/iotHubs/eventGridFilters/Read | Olay Kılavuzu filtresini alır |
> | Eylem | Microsoft.Devices/iotHubs/eventGridFilters/Write | Yeni veya mevcut bir olay Kılavuzu filtresini güncelleştir |
> | Eylem | Microsoft.Devices/iotHubs/eventHubEndpoints/consumerGroups/Delete | EventHub tüketici grubunu sil |
> | Eylem | Microsoft.Devices/iotHubs/eventHubEndpoints/consumerGroups/Read | EventHub tüketici gruplarını Al |
> | Eylem | Microsoft.Devices/iotHubs/eventHubEndpoints/consumerGroups/Write | EventHub tüketici grubu oluşturun |
> | Eylem | Microsoft.Devices/iotHubs/exportDevices/Action | Cihazlar dışarı aktarma |
> | Eylem | Microsoft.Devices/iotHubs/importDevices/Action | Cihazları İçeri Aktar |
> | Eylem | Microsoft.Devices/iotHubs/iotHubKeys/listkeys/Action | Belirtilen ad için IotHub anahtarını alma |
> | Eylem | Microsoft.Devices/iotHubs/iotHubStats/Read | Get IotHub Statistics |
> | Eylem | Microsoft.Devices/iotHubs/jobs/Read | İşi IotHub gönderdiniz ayrıntılarını Al |
> | Eylem | Microsoft.Devices/iotHubs/listkeys/Action | Tüm IotHub anahtarları alma |
> | Eylem | Microsoft.Devices/IotHubs/logDefinitions/read | Iothub hizmeti için kullanılabilir günlük tanımlarını alır |
> | Eylem | Microsoft.Devices/IotHubs/metricDefinitions/read | Iothub hizmet için kullanılabilir ölçümleri alır |
> | Eylem | Microsoft.Devices/iotHubs/operationresults/Read | (Eski API) işleminin sonucunu Al |
> | Eylem | Microsoft.Devices/iotHubs/quotaMetrics/Read | Kota ölçümleri alın |
> | Eylem | Microsoft.Devices/iotHubs/Read | Iothub kaynakları alır |
> | Eylem | Microsoft.Devices/iotHubs/routing/$testall/Action | Bir iletiye karşı mevcut tüm yolları test edin |
> | Eylem | Microsoft.Devices/iotHubs/routing/$testnew/Action | Bir ileti sağlanan bir test rota karşı test etme |
> | Eylem | Microsoft.Devices/iotHubs/routingEndpointsHealth/Read | Bir IotHub için tüm yönlendirme uç noktaları durumunu alır |
> | Eylem | Microsoft.Devices/iotHubs/skus/Read | Get valid IotHub Skus |
> | Eylem | Microsoft.Devices/iotHubs/Write | Iothub kaynak güncelle |
> | Eylem | Microsoft.Devices/locations/operationresults/Read | Alma konumu temel işlem sonucu |
> | Eylem | Microsoft.Devices/operationresults/Read | İşleminin sonucunu Al |
> | Eylem | Microsoft.Devices/operations/Read | Tüm ResourceProvider işlemleri Al |
> | Eylem | Microsoft.Devices/provisioningServices/certificates/Delete | Sertifika siler |
> | Eylem | Microsoft.Devices/provisioningServices/certificates/generateVerificationCode/Action | Doğrulama kodu oluştur |
> | Eylem | Microsoft.Devices/provisioningServices/certificates/Read | Sertifikayı alır |
> | Eylem | Microsoft.Devices/provisioningServices/certificates/verify/Action | Sertifika kaynak doğrulayın |
> | Eylem | Microsoft.Devices/provisioningServices/certificates/Write | Sertifika güncelle |
> | Eylem | Microsoft.Devices/provisioningServices/Delete | IotDps kaynağı silme |
> | Eylem | Microsoft.Devices/provisioningServices/diagnosticSettings/read | Kaynağın tanılama ayarını alır |
> | Eylem | Microsoft.Devices/provisioningServices/diagnosticSettings/write | Kaynağın tanılama ayarını oluşturur veya güncelleştirir |
> | Eylem | Microsoft.Devices/provisioningServices/keys/listkeys/Action | Anahtar adı için IotDps anahtarları alma |
> | Eylem | Microsoft.Devices/provisioningServices/listkeys/Action | Tüm IotDps anahtarları alma |
> | Eylem | Microsoft.Devices/provisioningServices/logDefinitions/read | Sağlama hizmeti için kullanılabilir günlük tanımlarını alır |
> | Eylem | Microsoft.Devices/provisioningServices/metricDefinitions/read | Sağlama hizmeti için kullanılabilir ölçümleri alır |
> | Eylem | Microsoft.Devices/provisioningServices/operationresults/Read | DPS işleminin sonucunu Al |
> | Eylem | Microsoft.Devices/provisioningServices/Read | IotDps kaynağını Al |
> | Eylem | Microsoft.Devices/provisioningServices/skus/Read | Geçerli IotDps SKU'ları Al |
> | Eylem | Microsoft.Devices/provisioningServices/Write | IotDps kaynak oluştur |
> | Eylem | Microsoft.Devices/register/action | Iothub kaynak sağlayıcısı için aboneliği kaydedin ve Iothub kaynaklarının oluşturulmasını sağlar |
> | Eylem | Microsoft.Devices/register/action | Iothub kaynak sağlayıcısı için aboneliği kaydedin ve Iothub kaynaklarının oluşturulmasını sağlar |
> | Eylem | Microsoft.Devices/usages/Read | Abonelik, bu sağlayıcı için kullanım ayrıntılarını alın. |

## <a name="microsoftdevspaces"></a>Microsoft.DevSpaces

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.DevSpaces/controllers/delete | Azure geliştirme alanları denetleyicisi ve veri düzlemi Hizmetleri Sil |
> | Eylem | Microsoft.DevSpaces/controllers/listConnectionDetails/action | Azure geliştirme alanları denetleyicinin altyapısı için liste bağlantısı ayrıntıları |
> | Eylem | Microsoft.DevSpaces/controllers/read | Okuma Azure geliştirme alanları Denetleyicisi Özellikleri |
> | Eylem | Microsoft.DevSpaces/controllers/write | Oluşturma veya güncelleştirme Azure geliştirme alanları Denetleyicisi Özellikleri |
> | Eylem | Microsoft.DevSpaces/locations/checkContainerHostMapping/action | Mevcut denetleyici eşleme için bir kapsayıcı konağı denetleyin |
> | Eylem | Microsoft.DevSpaces/locations/operationresults/read | Zaman uyumsuz bir işlemin durumunu oku |
> | Eylem | Microsoft.DevSpaces/register/action | Bir abonelikle Microsoft geliştirme alanları kaynak sağlayıcısını kaydetme |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.DevTestLab/labCenters/delete | Laboratuvar Merkezi silin. |
> | Eylem | Microsoft.DevTestLab/labCenters/read | Laboratuvar Merkezi okuyun. |
> | Eylem | Microsoft.DevTestLab/labCenters/write | Ekleyin veya Laboratuvar Merkezi değiştirin. |
> | Eylem | Microsoft.DevTestLab/labs/artifactSources/armTemplates/read | Azure resource manager şablonları okuyun. |
> | Eylem | Microsoft.DevTestLab/labs/artifactSources/artifacts/GenerateArmTemplate/action | Belirtilen yapıt için bir ARM şablonu oluşturur, gerekli dosyaları bir depolama hesabına yükler ve oluşturulan bir yapıya doğrular. |
> | Eylem | Microsoft.DevTestLab/labs/artifactSources/artifacts/read | Yapıtını okuyun. |
> | Eylem | Microsoft.DevTestLab/labs/artifactSources/delete | Yapıt kaynakları silin. |
> | Eylem | Microsoft.DevTestLab/labs/artifactSources/read | Yapıt kaynakları okuyun. |
> | Eylem | Microsoft.DevTestLab/labs/artifactSources/write | Ekleyin veya yapıt kaynağı değiştirin. |
> | Eylem | Microsoft.DevTestLab/labs/ClaimAnyVm/action | Rastgele bir talep edilebilir sanal makine Laboratuvardaki talep edin. |
> | Eylem | Microsoft.DevTestLab/labs/costs/read | Maliyetleri okuyun. |
> | Eylem | Microsoft.DevTestLab/labs/costs/write | Ekleyin veya maliyetleri değiştirin. |
> | Eylem | Microsoft.DevTestLab/labs/CreateEnvironment/action | Sanal makineleri bir laboratuar ortamında oluşturun. |
> | Eylem | Microsoft.DevTestLab/labs/customImages/delete | Özel görüntüleri silin. |
> | Eylem | Microsoft.DevTestLab/labs/customImages/read | Özel görüntüleri okuyun. |
> | Eylem | Microsoft.DevTestLab/labs/customImages/write | Ekleyin veya özel görüntüleri değiştirin. |
> | Eylem | Microsoft.DevTestLab/labs/delete | Labs silin. |
> | Eylem | Microsoft.DevTestLab/labs/EnsureCurrentUserProfile/action | Geçerli kullanıcının geçerli bir profil laboratuar ortamında olduğundan emin olun. |
> | Eylem | Microsoft.DevTestLab/labs/ExportResourceUsage/action | Laboratuvar kaynak kullanımı bir depolama hesabına dışarı aktarır. |
> | Eylem | Microsoft.DevTestLab/labs/formulas/delete | Formülleri silin. |
> | Eylem | Microsoft.DevTestLab/labs/formulas/read | Formülleri okuyun. |
> | Eylem | Microsoft.DevTestLab/labs/formulas/write | Ekleyebilir veya formülleri değiştirebilirsiniz. |
> | Eylem | Microsoft.DevTestLab/labs/galleryImages/read | Galeri görüntüleri okuyun. |
> | Eylem | Microsoft.DevTestLab/labs/GenerateUploadUri/action | Özel disk görüntüleri bir laboratuvara yüklemek için bir URI oluşturur. |
> | Eylem | Microsoft.DevTestLab/labs/idleShutdowns/delete | Boşta kapatmalar silin. |
> | Eylem | Microsoft.DevTestLab/labs/idleShutdowns/read | Boşta kapatmalar okuyun. |
> | Eylem | Microsoft.DevTestLab/labs/idleShutdowns/write | Ekleyin veya boşta kapatmalar değiştirin. |
> | Eylem | Microsoft.DevTestLab/labs/ImportVirtualMachine/action | Bir sanal makine farklı bir laboratuvara içeri aktarın. |
> | Eylem | Microsoft.DevTestLab/labs/ListVhds/action | Özel görüntü oluşturma için kullanılabilir disk görüntüleri listeleyin. |
> | Eylem | Microsoft.DevTestLab/labs/notificationChannels/delete | Notificationchannels silin. |
> | Eylem | Microsoft.DevTestLab/labs/notificationChannels/Notify/action | Sağlanan kanalına bildirim gönderin. |
> | Eylem | Microsoft.DevTestLab/labs/notificationChannels/read | Notificationchannels okuyun. |
> | Eylem | Microsoft.DevTestLab/labs/notificationChannels/write | Ekleyin veya notificationchannels değiştirin. |
> | Eylem | Microsoft.DevTestLab/labs/policySets/EvaluatePolicies/action | Laboratuvar ilkeyi değerlendirir. |
> | Eylem | Microsoft.DevTestLab/labs/policySets/policies/delete | İlkeleri silin. |
> | Eylem | Microsoft.DevTestLab/labs/policySets/policies/read | İlkeleri okuyun. |
> | Eylem | Microsoft.DevTestLab/labs/policySets/policies/write | Ekleme veya değiştirme ilkeleri. |
> | Eylem | Microsoft.DevTestLab/labs/policySets/read | İlke kümelerini okuyun. |
> | Eylem | Microsoft.DevTestLab/labs/read | Labs okuyun. |
> | Eylem | Microsoft.DevTestLab/labs/schedules/delete | Tabloları silin. |
> | Eylem | Microsoft.DevTestLab/labs/schedules/Execute/action | Bir zamanlama yürütün. |
> | Eylem | Microsoft.DevTestLab/labs/schedules/ListApplicable/action | Tüm geçerli tabloları listeler |
> | Eylem | Microsoft.DevTestLab/labs/schedules/read | Okuma zamanlar. |
> | Eylem | Microsoft.DevTestLab/labs/schedules/write | Ekleyin veya zamanlamalarını değiştirin. |
> | Eylem | Microsoft.DevTestLab/labs/serviceRunners/delete | Hizmet çalıştırıcıların silin. |
> | Eylem | Microsoft.DevTestLab/labs/serviceRunners/read | Hizmet çalıştırıcıların okuyun. |
> | Eylem | Microsoft.DevTestLab/labs/serviceRunners/write | Ekleyin veya hizmet çalıştırıcıların değiştirin. |
> | Eylem | Microsoft.DevTestLab/labs/sharedGalleries/delete | Paylaşılan galeriler silin. |
> | Eylem | Microsoft.DevTestLab/labs/sharedGalleries/read | Paylaşılan galeriler okuyun. |
> | Eylem | Microsoft.DevTestLab/labs/sharedGalleries/sharedImages/delete | Paylaşılan görüntüleri silin. |
> | Eylem | Microsoft.DevTestLab/labs/sharedGalleries/sharedImages/read | Paylaşılan görüntülerini okuyun. |
> | Eylem | Microsoft.DevTestLab/labs/sharedGalleries/sharedImages/write | Ekleyin veya paylaşılan görüntülerini değiştirin. |
> | Eylem | Microsoft.DevTestLab/labs/sharedGalleries/write | Ekleyin veya paylaşılan galeriler değiştirin. |
> | Eylem | Microsoft.DevTestLab/labs/users/delete | Kullanıcı profillerini silin. |
> | Eylem | Microsoft.DevTestLab/labs/users/disks/Attach/action | Ekleme ve sanal makineye disk kirasını oluşturun. |
> | Eylem | Microsoft.DevTestLab/labs/users/disks/delete | Diskleri silin. |
> | Eylem | Microsoft.DevTestLab/labs/users/disks/Detach/action | Ayırma ve sanal makineye bağlı disk kirayı Kes. |
> | Eylem | Microsoft.DevTestLab/labs/users/disks/read | Diskleri okuyun. |
> | Eylem | Microsoft.DevTestLab/labs/users/disks/write | Ekleyin veya diskleri değiştirin. |
> | Eylem | Microsoft.DevTestLab/labs/users/environments/delete | Ortamları silin. |
> | Eylem | Microsoft.DevTestLab/labs/users/environments/read | Ortamları okuma. |
> | Eylem | Microsoft.DevTestLab/labs/users/environments/write | Ekleyin veya ortamları değiştirin. |
> | Eylem | Microsoft.DevTestLab/labs/users/read | Kullanıcı profillerini okuyun. |
> | Eylem | Microsoft.DevTestLab/labs/users/secrets/delete | Gizli dizileri silin. |
> | Eylem | Microsoft.DevTestLab/labs/users/secrets/read | Gizli dizileri okuyun. |
> | Eylem | Microsoft.DevTestLab/labs/users/secrets/write | Ekleyin veya gizli dizileri değiştirin. |
> | Eylem | Microsoft.DevTestLab/labs/users/serviceFabrics/delete | Hizmet yapıları silin. |
> | Eylem | Microsoft.DevTestLab/labs/users/serviceFabrics/ListApplicableSchedules/action | Varsa geçerli Başlat/Durdur zamanlamaları listeler. |
> | Eylem | Microsoft.DevTestLab/labs/users/serviceFabrics/read | Hizmet dokularını okuyun. |
> | Eylem | Microsoft.DevTestLab/labs/users/serviceFabrics/schedules/delete | Tabloları silin. |
> | Eylem | Microsoft.DevTestLab/labs/users/serviceFabrics/schedules/Execute/action | Bir zamanlama yürütün. |
> | Eylem | Microsoft.DevTestLab/labs/users/serviceFabrics/schedules/read | Okuma zamanlar. |
> | Eylem | Microsoft.DevTestLab/labs/users/serviceFabrics/schedules/write | Ekleyin veya zamanlamalarını değiştirin. |
> | Eylem | Microsoft.DevTestLab/labs/users/serviceFabrics/Start/action | Service fabric başlatın. |
> | Eylem | Microsoft.DevTestLab/labs/users/serviceFabrics/Stop/action | Zastavit service fabric |
> | Eylem | Microsoft.DevTestLab/labs/users/serviceFabrics/write | Ekleyin veya hizmet dokularını değiştirin. |
> | Eylem | Microsoft.DevTestLab/labs/users/write | Ekleyin veya kullanıcı profillerini değiştirin. |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/AddDataDisk/action | Yeni veya var olan veri diski sanal makineye ekleyin. |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/ApplyArtifacts/action | Yapıtlar, sanal makine için geçerlidir. |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/Claim/action | Mevcut bir sanal makinenin sahipliğini al |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/delete | Sanal makineleri silin. |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/DetachDataDisk/action | Belirtilen bir diski sanal makineden çıkarın. |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/GetRdpFileContents/action | Sanal makine için RDP dosyasının içeriğini temsil eden bir dize alır |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/ListApplicableSchedules/action | Varsa geçerli Başlat/Durdur zamanlamaları listeler. |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/read | Sanal makineler okuyun. |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/Redeploy/action | Bir sanal makineyi yeniden dağıtın |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/Resize/action | Sanal makine yeniden boyutlandırın. |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/Restart/action | Bir sanal makineyi yeniden başlatın. |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/schedules/delete | Tabloları silin. |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/schedules/Execute/action | Bir zamanlama yürütün. |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/schedules/read | Okuma zamanlar. |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/schedules/write | Ekleyin veya zamanlamalarını değiştirin. |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/Start/action | Bir sanal makineyi başlatın. |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/Stop/action | Sanal makineyi durdurma |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/TransferDisks/action | Geçerli kullanıcının ait olduğu sanal makineye bağlı tüm veri diskleri aktarır. |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/UnClaim/action | Mevcut bir sanal makinenin sahipliğini serbest bırakın. |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/write | Ekleyin veya sanal makineleri değiştirin. |
> | Eylem | Microsoft.DevTestLab/labs/virtualNetworks/delete | Sanal ağlar silin. |
> | Eylem | Microsoft.DevTestLab/labs/virtualNetworks/read | Sanal ağlar'ı okuyun. |
> | Eylem | Microsoft.DevTestLab/labs/virtualNetworks/write | Ekleyebilir veya sanal ağları değiştirin. |
> | Eylem | Microsoft.DevTestLab/labs/vmPools/delete | Sanal makine havuzları silin. |
> | Eylem | Microsoft.DevTestLab/labs/vmPools/read | Sanal makine havuzları okuyun. |
> | Eylem | Microsoft.DevTestLab/labs/vmPools/write | Ekleyin veya sanal makine havuzları değiştirin. |
> | Eylem | Microsoft.DevTestLab/labs/write | Ekleyin veya labs değiştirin. |
> | Eylem | Microsoft.DevTestLab/locations/operations/read | Okuma işlemleri. |
> | Eylem | Microsoft.DevTestLab/register/action | Aboneliği kaydeder |
> | Eylem | Microsoft.DevTestLab/schedules/delete | Tabloları silin. |
> | Eylem | Microsoft.DevTestLab/schedules/Execute/action | Bir zamanlama yürütün. |
> | Eylem | Microsoft.DevTestLab/schedules/read | Okuma zamanlar. |
> | Eylem | Microsoft.DevTestLab/schedules/Retarget/action | Güncelleştirmeleri zamanlama ait hedef kaynak kimliği |
> | Eylem | Microsoft.DevTestLab/schedules/write | Ekleyin veya zamanlamalarını değiştirin. |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.DocumentDB/databaseAccountNames/read | Ad kullanılabilirliğini denetler. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/collections/delete | Bir koleksiyonu silin. Yalnızca API türleri için geçerlidir: 'mongodb'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/collections/operationResults/read | Zaman uyumsuz işlem durumunu okur. Yalnızca API türleri için geçerlidir: 'mongodb'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/collections/read | Bir koleksiyonu okunamıyor veya tüm koleksiyonları listeler. Yalnızca API türleri için geçerlidir: 'mongodb'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/collections/settings/operationResults/read | Zaman uyumsuz işlem durumunu okur. Yalnızca API türleri için geçerlidir: 'mongodb'. Yalnızca ilgili türleri ayarlamak için: 'işleme'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/collections/settings/read | Bir koleksiyon işleme hacmini okuyun. Yalnızca API türleri için geçerlidir: 'mongodb'. Yalnızca ilgili türleri ayarlamak için: 'işleme'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/collections/settings/write | Bir koleksiyon işleme hacmini güncelleştirin. Yalnızca API türleri için geçerlidir: 'mongodb'. Yalnızca ilgili türleri ayarlamak için: 'işleme'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/collections/write | Veya bir koleksiyon güncelleştirilemiyor. Yalnızca API türleri için geçerlidir: 'mongodb'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/containers/delete | Bir kapsayıcı silin. Yalnızca API türleri için geçerlidir: 'sql'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/containers/operationResults/read | Zaman uyumsuz işlem durumunu okur. Yalnızca API türleri için geçerlidir: 'sql'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/containers/read | Bir kapsayıcı okuma veya tüm kapsayıcıları listesi. Yalnızca API türleri için geçerlidir: 'sql'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/containers/settings/operationResults/read | Zaman uyumsuz işlem durumunu okur. Yalnızca API türleri için geçerlidir: 'sql'. Yalnızca ilgili türleri ayarlamak için: 'işleme'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/containers/settings/read | Kapsayıcının aktarım hızını okuyun. Yalnızca API türleri için geçerlidir: 'sql'. Yalnızca ilgili türleri ayarlamak için: 'işleme'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/containers/settings/write | Kapsayıcının aktarım hızını güncelleştirin. Yalnızca API türleri için geçerlidir: 'sql'. Yalnızca ilgili türleri ayarlamak için: 'işleme'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/containers/write | Veya bir kapsayıcı güncelleştirilemiyor. Yalnızca API türleri için geçerlidir: 'sql'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/delete | Bir veritabanını silin. Yalnızca API türleri için geçerlidir: 'sql', 'mongodb', 'gremlin'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/graphs/delete | Graf silme. Yalnızca API türleri için geçerlidir: 'gremlin'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/graphs/operationResults/read | Zaman uyumsuz işlem durumunu okur. Yalnızca API türleri için geçerlidir: 'gremlin'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/graphs/read | Bir grafiği okumak veya tüm grafikler listesi. Yalnızca API türleri için geçerlidir: 'gremlin'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/graphs/settings/operationResults/read | Zaman uyumsuz işlem durumunu okur. Yalnızca API türleri için geçerlidir: 'gremlin'. Yalnızca ilgili türleri ayarlamak için: 'işleme'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/graphs/settings/read | Grafik işleme okuyun. Yalnızca API türleri için geçerlidir: 'gremlin'. Yalnızca ilgili türleri ayarlamak için: 'işleme'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/graphs/settings/write | Grafik işleme güncelleştirin. Yalnızca API türleri için geçerlidir: 'gremlin'. Yalnızca ilgili türleri ayarlamak için: 'işleme'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/graphs/write | Veya bir grafik güncelleştirilemiyor. Yalnızca API türleri için geçerlidir: 'gremlin'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/operationResults/read | Zaman uyumsuz işlem durumunu okur. Yalnızca API türleri için geçerlidir: 'sql', 'mongodb', 'gremlin'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/read | Bir veritabanını okumak veya tüm veritabanlarını listeleyin. Yalnızca API türleri için geçerlidir: 'sql', 'mongodb', 'gremlin'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/settings/operationResults/read | Zaman uyumsuz işlem durumunu okur. Yalnızca API türleri için geçerlidir: 'sql', 'mongodb', 'gremlin'. Yalnızca ilgili türleri ayarlamak için: 'işleme'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/settings/read | Veritabanı aktarım hızını okuyun. Yalnızca API türleri için geçerlidir: 'sql', 'mongodb', 'gremlin'. Yalnızca ilgili türleri ayarlamak için: 'işleme'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/settings/write | Veritabanı aktarım hızını güncelleştirin. Yalnızca API türleri için geçerlidir: 'sql', 'mongodb', 'gremlin'. Yalnızca ilgili türleri ayarlamak için: 'işleme'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/write | Bir veritabanı oluşturun. Yalnızca API türleri için geçerlidir: 'sql', 'mongodb', 'gremlin'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/delete | Bir anahtar alanı silin. Yalnızca API türleri için geçerlidir: 'cassandra'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/operationResults/read | Zaman uyumsuz işlem durumunu okur. Yalnızca API türleri için geçerlidir: 'cassandra'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/read | Bir anahtar alanı okunamıyor veya tüm keyspaces listesi. Yalnızca API türleri için geçerlidir: 'cassandra'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/settings/operationResults/read | Zaman uyumsuz işlem durumunu okur. Yalnızca API türleri için geçerlidir: 'cassandra'. Yalnızca ilgili türleri ayarlamak için: 'işleme'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/settings/read | Bir anahtar alanı aktarım hızı okuyun. Yalnızca API türleri için geçerlidir: 'cassandra'. Yalnızca ilgili türleri ayarlamak için: 'işleme'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/settings/write | Bir anahtar alanı aktarım hızı güncelleştirin. Yalnızca API türleri için geçerlidir: 'cassandra'. Yalnızca ilgili türleri ayarlamak için: 'işleme'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/tables/delete | Tablo silme. Yalnızca API türleri için geçerlidir: 'cassandra'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/tables/operationResults/read | Zaman uyumsuz işlem durumunu okur. Yalnızca API türleri için geçerlidir: 'cassandra'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/tables/read | Salt okunur bir tablo veya tüm tabloları listeleyin. Yalnızca API türleri için geçerlidir: 'cassandra'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/tables/settings/operationResults/read | Zaman uyumsuz işlem durumunu okur. Yalnızca API türleri için geçerlidir: 'cassandra'. Yalnızca ilgili türleri ayarlamak için: 'işleme'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/tables/settings/read | Bir tablo aktarım hızı okuyun. Yalnızca API türleri için geçerlidir: 'cassandra'. Yalnızca ilgili türleri ayarlamak için: 'işleme'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/tables/settings/write | Bir tablo aktarım hızı güncelleştirin. Yalnızca API türleri için geçerlidir: 'cassandra'. Yalnızca ilgili türleri ayarlamak için: 'işleme'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/tables/write | Oluşturun veya bir tabloyu güncelleştirin. Yalnızca API türleri için geçerlidir: 'cassandra'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/write | Bir anahtar alanı oluşturun. Yalnızca API türleri için geçerlidir: 'cassandra'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/tables/delete | Tablo silme. Yalnızca API türleri için geçerlidir: 'table'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/tables/operationResults/read | Zaman uyumsuz işlem durumunu okur. Yalnızca API türleri için geçerlidir: 'table'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/tables/read | Salt okunur bir tablo veya tüm tabloları listeleyin. Yalnızca API türleri için geçerlidir: 'table'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/tables/settings/operationResults/read | Zaman uyumsuz işlem durumunu okur. Yalnızca API türleri için geçerlidir: 'table'. Yalnızca ilgili türleri ayarlamak için: 'işleme'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/tables/settings/read | Bir tablo aktarım hızı okuyun. Yalnızca API türleri için geçerlidir: 'table'. Yalnızca ilgili türleri ayarlamak için: 'işleme'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/tables/settings/write | Bir tablo aktarım hızı güncelleştirin. Yalnızca API türleri için geçerlidir: 'table'. Yalnızca ilgili türleri ayarlamak için: 'işleme'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/tables/write | Oluşturun veya bir tabloyu güncelleştirin. Yalnızca API türleri için geçerlidir: 'table'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/backup/action | Yedeklemeyi yapılandırmak için bir istek gönderin |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/changeResourceGroup/action | Bir veritabanı hesabı, kaynak grubunu değiştir |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/databases/collections/metricDefinitions/read | Toplama, ölçüm tanımlarını okur. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/databases/collections/metrics/read | Koleksiyon ölçümlerini okur. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/databases/collections/partitionKeyRangeId/metrics/read | Veritabanı hesabı bölüm anahtar düzeyindeki ölçümleri okuma |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/databases/collections/partitions/metrics/read | Veritabanı hesabı bölüm düzeyi ölçümleri okuma |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/databases/collections/partitions/read | Bir koleksiyondaki veritabanı hesabı bölümlerini okuyun |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/databases/collections/partitions/usages/read | Veritabanı hesabı bölüm düzeyi kullanımları okuyun |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/databases/collections/usages/read | Koleksiyon kullanımları okur. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/databases/metricDefinitions/read | Veritabanı ölçüm tanımlarını okur |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/databases/metrics/read | Veritabanı ölçümlerini okur. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/databases/usages/read | Veritabanı kullanımları okur. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/delete | Veritabanı hesaplarında siler. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/failoverPriorityChange/action | Bir veritabanı hesabı bölgelere yük devretme önceliklerini değiştirin. Bu el ile yük devretme işlemi gerçekleştirmek için kullanılır |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/listConnectionStrings/action | Bağlantı dizelerini almak için bir veritabanı hesabı |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/listKeys/action | Bir veritabanı hesabı, anahtarlarını Listele |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/metricDefinitions/read | Veritabanı hesabı ölçüm tanımlarını okur. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/metrics/read | Veritabanı hesabı ölçümlerini okur. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/offlineRegion/action | Çevrimdışı bir veritabanı hesabı bölgesi.  |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/onlineRegion/action | Çevrimiçi bir veritabanı hesabı bölgesi. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/operationResults/read | Zaman uyumsuz işlemin durumunu oku |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/percentile/metrics/read | Gecikme süresi ölçümlerini okuma |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/percentile/read | Çoğaltma gecikmeleri yüzdebirliklerini okuyun |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/percentile/sourceRegion/targetRegion/metrics/read | Belirli bir kaynak ve hedef bölge için gecikme süresi ölçümlerini okuma |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/percentile/targetRegion/metrics/read | Belirli hedef bölge için gecikme süresi ölçümlerini okuma |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/privateEndpointConnectionProxies/delete | Veritabanı hesabı, bir özel uç nokta bağlantısı Ara sunucuyu Sil |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/privateEndpointConnectionProxies/read | Özel uç nokta bağlantı proxy veritabanı hesabının okuma |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/privateEndpointConnectionProxies/validate/action | Özel uç nokta bağlantı Ara sunucu, veritabanı hesabı doğrula |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/privateEndpointConnectionProxies/write | Özel uç nokta bağlantı proxy veritabanı hesabının güncelle |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/read | Bir veritabanı hesabı okur. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/readonlykeys/action | Veritabanı hesabı salt okunur anahtarları okur. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/readonlykeys/read | Veritabanı hesabı salt okunur anahtarları okur. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/regenerateKey/action | Bir veritabanı hesabı, anahtarlarını döndürme |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/region/databases/collections/metrics/read | Bölgesel koleksiyon ölçümlerini okur. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/region/databases/collections/partitionKeyRangeId/metrics/read | Bölgesel bir veritabanı hesabı bölüm anahtar düzeyindeki ölçümleri okuma |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/region/databases/collections/partitions/metrics/read | Bölgesel bir veritabanı hesabı bölüm düzeyi ölçümleri okuma |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/region/databases/collections/partitions/read | Bir koleksiyondaki bölgesel veritabanı hesabı bölümlerini okuyun |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/region/metrics/read | Bölge ve veritabanı hesabı ölçümleri okur. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/restore/action | Bir geri yükleme isteği gönderin |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/usages/read | Veritabanı hesabı kullanımları okur. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/write | Veritabanı hesaplarında güncelleştirin. |
> | Eylem | Microsoft.DocumentDB/locations/deleteVirtualNetworkOrSubnets/action | Microsoft.DocumentDB VirtualNetwork veya alt ağın silindiğini bildirir |
> | Eylem | Microsoft.DocumentDB/locations/deleteVirtualNetworkOrSubnets/operationResults/read | Zaman uyumsuz işlem deleteVirtualNetworkOrSubnets durumunu okuma |
> | Eylem | Microsoft.DocumentDB/locations/operationsStatus/read | Zaman uyumsuz işlem durumunu okur |
> | Eylem | Microsoft.DocumentDB/operationResults/read | Zaman uyumsuz işlemin durumunu oku |
> | Eylem | Microsoft.DocumentDB/operations/read | Microsoft DocumentDB için kullanılabilen okuma işlemleri  |
> | Eylem | Microsoft.DocumentDB/register/action |  Abonelik için Microsoft DocumentDB kaynak sağlayıcısını kaydetme |

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.DomainRegistration/checkDomainAvailability/Action | Bir etki alanı satın alma için kullanılabilir olup olmadığını denetleyin |
> | Eylem | Microsoft.DomainRegistration/domains/Delete | Mevcut bir etki alanı silin. |
> | Eylem | Microsoft.DomainRegistration/domains/domainownershipidentifiers/Delete | Sahipliği tanımlayıcısını sil |
> | Eylem | Microsoft.DomainRegistration/domains/domainownershipidentifiers/Read | Liste sahipliği tanımlayıcıları |
> | Eylem | Microsoft.DomainRegistration/domains/domainownershipidentifiers/Read | Tanımlayıcı Sahipliği Al |
> | Eylem | Microsoft.DomainRegistration/domains/domainownershipidentifiers/Write | Oluşturma veya tanımlayıcısını güncelleştirin |
> | Eylem | Microsoft.DomainRegistration/domains/operationresults/Read | Bir etki alanı işlemi Al |
> | Eylem | Microsoft.DomainRegistration/domains/Read | Etki alanlarının listesini alma |
> | Eylem | Microsoft.DomainRegistration/domains/Read | Etki alanı Al |
> | Eylem | Microsoft.DomainRegistration/domains/renew/Action | Mevcut bir etki alanına yenileyin. |
> | Eylem | Microsoft.DomainRegistration/domains/Write | Yeni bir etki alanı eklemek veya mevcut bir güncelleştirme |
> | Eylem | Microsoft.DomainRegistration/generateSsoRequest/Action | Etki alanı denetim Merkezi'nde oturum imzalamak için bir istek oluşturun. |
> | Eylem | Microsoft.DomainRegistration/listDomainRecommendations/Action | Anahtar sözcüklere göre listesi etki alanının öneriler alın |
> | Eylem | Microsoft.DomainRegistration/operations/Read | Tüm app service etki alanı kaydı işlemlerini listele |
> | Eylem | Microsoft.DomainRegistration/register/action | Abonelik için Microsoft Domains kaynak sağlayıcısını kaydetme |
> | Eylem | Microsoft.DomainRegistration/topLevelDomains/listAgreements/Action | Sözleşme eylem listesi |
> | Eylem | Microsoft.DomainRegistration/topLevelDomains/Read | TopLevel etki alanlarını Al |
> | Eylem | Microsoft.DomainRegistration/topLevelDomains/Read | TopLevel etki alanı Al |
> | Eylem | Microsoft.DomainRegistration/validateDomainRegistrationInformation/Action | Etki alanı satın alma nesnesi gönderme olmadan doğrula |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.EventGrid/domains/delete | Bir etki alanını Sil |
> | Eylem | Microsoft.EventGrid/domains/listKeys/action | Bir etki alanı anahtarlarını Listele |
> | Eylem | Microsoft.EventGrid/domains/providers/Microsoft.Insights/metricDefinitions/read | Etki alanları için kullanılabilir ölçümleri alır |
> | Eylem | Microsoft.EventGrid/domains/read | Salt okunur bir etki alanı |
> | Eylem | Microsoft.EventGrid/domains/regenerateKey/action | Bir etki alanı için yeniden oluşturma anahtarı |
> | Eylem | Microsoft.EventGrid/domains/topics/read | Bir etki alanı konuyu okuyun |
> | Eylem | Microsoft.EventGrid/domains/write | Bir etki alanı güncelle |
> | Eylem | Microsoft.EventGrid/eventSubscriptions/delete | Bir eventSubscription Sil |
> | Eylem | Microsoft.EventGrid/eventSubscriptions/getFullUrl/action | Olay aboneliği için tam URL'sini alma |
> | Eylem | Microsoft.EventGrid/eventSubscriptions/providers/Microsoft.Insights/diagnosticSettings/read | Olay abonelikleri için tanılama ayarını alır |
> | Eylem | Microsoft.EventGrid/eventSubscriptions/providers/Microsoft.Insights/diagnosticSettings/write | Olay abonelikleri için tanılama ayarını oluşturur veya güncelleştirir |
> | Eylem | Microsoft.EventGrid/eventSubscriptions/providers/Microsoft.Insights/metricDefinitions/read | EventSubscriptions için kullanılabilir ölçümleri alır |
> | Eylem | Microsoft.EventGrid/eventSubscriptions/read | Bir eventSubscription okuyun |
> | Eylem | Microsoft.EventGrid/eventSubscriptions/write | Bir eventSubscription güncelle |
> | Eylem | Microsoft.EventGrid/extensionTopics/providers/Microsoft.Insights/diagnosticSettings/read | Konular için tanılama ayarını alır |
> | Eylem | Microsoft.EventGrid/extensionTopics/providers/Microsoft.Insights/diagnosticSettings/write | Konular için tanılama ayarını oluşturur veya güncelleştirir |
> | Eylem | Microsoft.EventGrid/extensionTopics/providers/Microsoft.Insights/metricDefinitions/read | Konular için mevcut ölçümleri alır |
> | Eylem | Microsoft.EventGrid/extensionTopics/read | Bir extensionTopic okuyun. |
> | Eylem | Microsoft.EventGrid/locations/eventSubscriptions/read | Bölgesel olay abonelikleri listesi |
> | Eylem | Microsoft.EventGrid/locations/operationResults/read | Bölgesel bir işlemin sonucunu okuyun |
> | Eylem | Microsoft.EventGrid/locations/operationsStatus/read | Bölgesel bir işlemin durumunu okuma |
> | Eylem | Microsoft.EventGrid/locations/topictypes/eventSubscriptions/read | Tarafından topictype bölgesel olay abonelikleri listesi |
> | Eylem | Microsoft.EventGrid/operationResults/read | Bir işlem sonucunu okuyun |
> | Eylem | Microsoft.EventGrid/operations/read | EventGrid işlemleri listeleyin. |
> | Eylem | Microsoft.EventGrid/operationsStatus/read | Bir işlemin durumunu okuma |
> | Eylem | Microsoft.EventGrid/register/action | EventGrid kaynak sağlayıcısı için aboneliği kaydeder. |
> | Eylem | Microsoft.EventGrid/topics/delete | Bir konu Sil |
> | Eylem | Microsoft.EventGrid/topics/listKeys/action | Bir konu anahtarlarını Listele |
> | Eylem | Microsoft.EventGrid/topics/providers/Microsoft.Insights/diagnosticSettings/read | Konular için tanılama ayarını alır |
> | Eylem | Microsoft.EventGrid/topics/providers/Microsoft.Insights/diagnosticSettings/write | Konular için tanılama ayarını oluşturur veya güncelleştirir |
> | Eylem | Microsoft.EventGrid/topics/providers/Microsoft.Insights/metricDefinitions/read | Konular için mevcut ölçümleri alır |
> | Eylem | Microsoft.EventGrid/topics/read | Bir konuyu okuyun |
> | Eylem | Microsoft.EventGrid/topics/regenerateKey/action | Bir konu anahtarı yeniden oluştur |
> | Eylem | Microsoft.EventGrid/topics/write | Bir konu güncelle |
> | Eylem | Microsoft.EventGrid/topictypes/eventSubscriptions/read | Genel olay abonelikleri konu türüne göre listeleme |
> | Eylem | Microsoft.EventGrid/topictypes/eventtypes/read | Bir topictype tarafından desteklenen eventtypes okuyun |
> | Eylem | Microsoft.EventGrid/topictypes/read | Bir topictype okuyun |
> | Eylem | Microsoft.EventGrid/unregister/action | EventGrid kaynak sağlayıcısı için aboneliği kaydını siler. |

## <a name="microsofteventhub"></a>Microsoft.EventHub

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.EventHub/availableClusterRegions/read | Azure bölgesi tarafından önceden sağlanmış kümelerini listelemek için bir işlemi okuyun. |
> | Eylem | Microsoft.EventHub/checkNameAvailability/action | Seçili abonelik altında ad alanı kullanılabilirliğini denetler. |
> | Eylem | Microsoft.EventHub/checkNamespaceAvailability/action | Seçili abonelik altında ad alanı kullanılabilirliğini denetler. Bu API kullanım dışıdır CheckNameAvailability Lütfen bunun yerine kullanın. |
> | Eylem | Microsoft.EventHub/clusters/delete | Var olan bir küme kaynağı siler. |
> | Eylem | Microsoft.EventHub/clusters/namespaces/read | Bir küme içindeki ad alanları için ad alanı ARM kimlikleri listesi. |
> | Eylem | Microsoft.EventHub/clusters/operationresults/read | Bir küme zaman uyumsuz işlemin durumunu alın. |
> | Eylem | Microsoft.EventHub/clusters/providers/Microsoft.Insights/metricDefinitions/read | Küme ölçümleri kaynak açıklamalarının listesini alın |
> | Eylem | Microsoft.EventHub/clusters/read | Küme kaynağı açıklamasını alır |
> | Eylem | Microsoft.EventHub/clusters/write | Oluşturur veya var olan bir küme kaynağı değiştirir. |
> | Eylem | Microsoft.EventHub/locations/deleteVirtualNetworkOrSubnets/action | Belirtilen VNet için sanal ağ kuralları EventHub kaynak Sağlayıcısı'nda siler |
> | Eylem | Microsoft.EventHub/namespaces/authorizationRules/action | Güncelleştirmeleri Namespace yetkilendirme kuralı. Bu API kullanım dışıdır. Bunun yerine Namespace yetkilendirme kuralını güncelleştirmek için lütfen bir PUT çağrısı kullanın... Bu işlem API 2017-04-01 sürümünde desteklenmiyor. |
> | Eylem | Microsoft.EventHub/namespaces/authorizationRules/delete | Namespace yetkilendirme kuralını silin. Varsayılan Namespace yetkilendirme kuralı silinemez.  |
> | Eylem | Microsoft.EventHub/namespaces/authorizationRules/listkeys/action | Namespace bağlantı dizesini alın |
> | Eylem | Microsoft.EventHub/namespaces/authorizationRules/read | Ad alanı yetkilendirme kuralları açıklamasının listesini alın. |
> | Eylem | Microsoft.EventHub/namespaces/authorizationRules/regenerateKeys/action | Kaynağın birincil veya ikincil anahtarı yeniden oluştur |
> | Eylem | Microsoft.EventHub/namespaces/authorizationRules/write | Namespace düzeyinde yetkilendirme kuralları oluşturun ve özelliklerini güncelleştirin. Yetkilendirme kuralları erişim hakları, birincil ve ikincil anahtarlar güncelleştirilebilir. |
> | Eylem | Microsoft.EventHub/namespaces/Delete | Namespace kaynağı silme |
> | Eylem | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/authorizationRules/listkeys/action | Yetkilendirme kuralları anahtarlarını olağanüstü durum kurtarma birincil ad alanı için alır. |
> | Eylem | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/authorizationRules/read | Olağanüstü durum kurtarma birincil Namespace'nın yetkilendirme kurallarını Al |
> | Eylem | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/breakPairing/action | Olağanüstü durum kurtarma devre dışı bırakır ve birincil değişiklikleri ikincil ad alanına çoğaltmayı durdurur. |
> | Eylem | Microsoft.EventHub/namespaces/disasterrecoveryconfigs/checkNameAvailability/action | Seçili abonelik altında ad alanı diğer ad kullanılabilirliğini denetler. |
> | Eylem | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/delete | Ad alanıyla ilişkili olağanüstü durum kurtarma yapılandırmasını siler. Bu işlem yalnızca birincil ad alanı aracılığıyla çağrılabilir. |
> | Eylem | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/failover/action | Bir GEO DR Yük Devretmesini çağırır ve ad alanı diğer adını ikincil ad alanına işaret edecek şekilde yeniden yapılandırır. |
> | Eylem | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/read | Ad alanıyla ilişkili olağanüstü durum kurtarma yapılandırmasını alır. |
> | Eylem | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/write | Oluşturur veya ad alanıyla ilişkili olağanüstü durum kurtarma yapılandırmasını güncelleştirir. |
> | Eylem | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/action | EventHub güncelleştirmek için işlem. Bu işlem API 2017-04-01 sürümünde desteklenmiyor. Yetkilendirme kuralları. Yetkilendirme kuralı'nı güncelleştirmek için lütfen bir PUT çağrısı kullanın. |
> | Eylem | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/delete | EventHub yetkilendirme kurallarını silme işlemi |
> | Eylem | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/listkeys/action | EventHub bağlantı dizesini alın |
> | Eylem | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/read |  EventHub yetkilendirme kurallarının listesini alın |
> | Eylem | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/regenerateKeys/action | Kaynağın birincil veya ikincil anahtarı yeniden oluştur |
> | Eylem | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/write | EventHub yetkilendirme kuralları oluşturun ve özelliklerini güncelleştirin. Yetkilendirme kuralları erişim hakları güncelleştirilebilir. |
> | Eylem | Microsoft.EventHub/namespaces/eventHubs/consumergroups/Delete | ConsumerGroup kaynak silme işlemi |
> | Eylem | Microsoft.EventHub/namespaces/eventHubs/consumergroups/read | ConsumerGroup kaynak açıklamalarının listesini alın |
> | Eylem | Microsoft.EventHub/namespaces/eventHubs/consumergroups/write | Oluşturma veya güncelleştirme ConsumerGroup özellikleri. |
> | Eylem | Microsoft.EventHub/namespaces/eventhubs/Delete | EventHub kaynak silme işlemi |
> | Eylem | Microsoft.EventHub/namespaces/eventhubs/read | EventHub kaynak açıklamalarının listesini alın |
> | Eylem | Microsoft.EventHub/namespaces/eventhubs/write | Oluşturma veya güncelleştirme EventHub özellikleri. |
> | Eylem | Microsoft.EventHub/namespaces/ipFilterRules/delete | IP Filtresi kaynağı silme |
> | Eylem | Microsoft.EventHub/namespaces/ipFilterRules/read | IP Filtresi kaynağını Al |
> | Eylem | Microsoft.EventHub/namespaces/ipFilterRules/write | IP Filtresi kaynak oluştur |
> | DataAction | Microsoft.EventHub/namespaces/messages/receive/action | İleti alma |
> | DataAction | Microsoft.EventHub/namespaces/messages/send/action | İleti gönderme |
> | Eylem | Microsoft.EventHub/namespaces/messagingPlan/read | Mesajlaşma planı için bir ad alanını alır.<br>Bu API kullanım dışıdır.<br>Namespace kaynak sonraki API sürümlerinde (üst) MessagingPlan kaynağı aracılığıyla kullanıma sunulan özellikler taşınan...<br>Bu işlem API 2017-04-01 sürümünde desteklenmiyor. |
> | Eylem | Microsoft.EventHub/namespaces/messagingPlan/write | Mesajlaşma planı için bir ad alanı güncelleştirir.<br>Bu API kullanım dışıdır.<br>Namespace kaynak sonraki API sürümlerinde (üst) MessagingPlan kaynağı aracılığıyla kullanıma sunulan özellikler taşınan...<br>Bu işlem API 2017-04-01 sürümünde desteklenmiyor. |
> | Eylem | Microsoft.EventHub/namespaces/networkrulesets/delete | Sanal ağ kuralı kaynağı silme |
> | Eylem | Microsoft.EventHub/namespaces/networkrulesets/read | NetworkRuleSet kaynak alır |
> | Eylem | Microsoft.EventHub/namespaces/networkrulesets/write | Sanal ağ kuralı kaynağı oluşturma |
> | Eylem | Microsoft.EventHub/namespaces/operationresults/read | Namespace işlemin durumunu alın |
> | Eylem | Microsoft.EventHub/namespaces/providers/Microsoft.Insights/diagnosticSettings/read | Namespace tanılama ayarlarını kaynak açıklamalarının listesini alın |
> | Eylem | Microsoft.EventHub/namespaces/providers/Microsoft.Insights/diagnosticSettings/write | Namespace tanılama ayarlarını kaynak açıklamalarının listesini alın |
> | Eylem | Microsoft.EventHub/namespaces/providers/Microsoft.Insights/logDefinitions/read | Namespace günlükleri kaynak açıklamalarının listesini alın |
> | Eylem | Microsoft.EventHub/namespaces/providers/Microsoft.Insights/metricDefinitions/read | Namespace ölçümleri kaynak açıklamalarının listesini alın |
> | Eylem | Microsoft.EventHub/namespaces/read | Namespace kaynak açıklamasının listesini alır |
> | Eylem | Microsoft.EventHub/namespaces/removeAcsNamepsace/action | ACS ad alanını Kaldır |
> | Eylem | Microsoft.EventHub/namespaces/virtualNetworkRules/delete | Sanal ağ kuralı kaynağı silme |
> | Eylem | Microsoft.EventHub/namespaces/virtualNetworkRules/read | Sanal ağ kuralı kaynağı alır |
> | Eylem | Microsoft.EventHub/namespaces/virtualNetworkRules/write | Sanal ağ kuralı kaynağı oluşturma |
> | Eylem | Microsoft.EventHub/namespaces/write | Namespace kaynağı oluşturun ve özelliklerini güncelleştirin. Etiketleri ve kapasitesi Namespace hangi güncelleştirilebilecek özelliklerdir. |
> | Eylem | Microsoft.EventHub/operations/read | İşlemleri Al |
> | Eylem | Microsoft.EventHub/register/action | EventHub kaynak sağlayıcısı için aboneliği kaydeder ve EventHub kaynaklarının oluşturulmasını sağlar |
> | Eylem | Microsoft.EventHub/sku/read | Sku kaynak açıklamalarının listesini alın |
> | Eylem | Microsoft.EventHub/sku/regions/read | SkuRegions kaynak açıklamalarının listesini alın |
> | Eylem | Microsoft.EventHub/unregister/action | EventHub kaynak sağlayıcısını Kaydet |

## <a name="microsoftfeatures"></a>Microsoft.Features

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Features/features/read | Bir aboneliğin özelliklerini alır. |
> | Eylem | Microsoft.Features/operations/read | İşlemlerin listesini alır. |
> | Eylem | Microsoft.Features/providers/features/read | Bir verilen kaynak sağlayıcısındaki bir abonelik özelliğini alır. |
> | Eylem | Microsoft.Features/providers/features/register/action | Bir verilen kaynak sağlayıcısındaki bir abonelik özelliğini kaydeder. |
> | Eylem | Microsoft.Features/providers/features/unregister/action | Belirli bir kaynak sağlayıcısındaki bir abonelik özelliğinin kaydını siler. |
> | Eylem | Microsoft.Features/register/action | Bir aboneliğin özelliğini kaydeder. |

## <a name="microsoftguestconfiguration"></a>Microsoft.GuestConfiguration

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.GuestConfiguration/guestConfigurationAssignments/delete | Konuk yapılandırma atamasını silin. |
> | Eylem | Microsoft.GuestConfiguration/guestConfigurationAssignments/read | Get Konuk yapılandırma atama. |
> | Eylem | Microsoft.GuestConfiguration/guestConfigurationAssignments/reports/read | Get Konuk yapılandırma atama raporu. |
> | Eylem | Microsoft.GuestConfiguration/guestConfigurationAssignments/write | Yeni Konuk yapılandırma ataması oluşturun. |
> | Eylem | Microsoft.GuestConfiguration/register/action | Microsoft.GuestConfiguration kaynak sağlayıcısı için aboneliği kaydeder. |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.HDInsight/clusters/applications/delete | HDInsight kümesi için uygulamayı silin |
> | Eylem | Microsoft.HDInsight/clusters/applications/read | HDInsight kümesi alma |
> | Eylem | Microsoft.HDInsight/clusters/applications/write | HDInsight kümesi için uygulama oluştur veya güncelleştir |
> | Eylem | Microsoft.HDInsight/clusters/changerdpsetting/action | HDInsight kümesi için RDP ayarı Değiştir |
> | Eylem | Microsoft.HDInsight/clusters/configurations/action | HDInsight küme yapılandırmasını güncelleştirme |
> | Eylem | Microsoft.HDInsight/clusters/configurations/action | HDInsight küme yapılandırmalarını alma |
> | Eylem | Microsoft.HDInsight/clusters/configurations/read | HDInsight küme yapılandırmalarını alma |
> | Eylem | Microsoft.HDInsight/clusters/delete | HDInsight küme silme |
> | Eylem | Microsoft.HDInsight/clusters/getGatewaySettings/action | HDInsight kümesi için ağ geçidi ayarlarını alma |
> | Eylem | Microsoft.HDInsight/clusters/providers/Microsoft.Insights/diagnosticSettings/read | HDInsight küme kaynağı için tanılama ayarını alır |
> | Eylem | Microsoft.HDInsight/clusters/providers/Microsoft.Insights/diagnosticSettings/write | HDInsight küme kaynağı için tanılama ayarını oluşturur veya güncelleştirir |
> | Eylem | Microsoft.HDInsight/clusters/providers/Microsoft.Insights/metricDefinitions/read | HDInsight kümesi için kullanılabilir ölçümleri alır |
> | Eylem | Microsoft.HDInsight/clusters/read | HDInsight kümesi hakkında ayrıntılı bilgi edinin |
> | Eylem | Microsoft.HDInsight/clusters/roles/resize/action | Bir HDInsight kümesi yeniden boyutlandırma |
> | Eylem | Microsoft.HDInsight/clusters/updateGatewaySettings/action | HDInsight kümesi için ağ geçidi ayarlarını güncelleştirme |
> | Eylem | Microsoft.HDInsight/clusters/write | HDInsight küme oluştur veya güncelleştir |
> | Eylem | Microsoft.HDInsight/locations/capabilities/read | Abonelik özelliklerini alma |
> | Eylem | Microsoft.HDInsight/locations/checkNameAvailability/read | Ad kullanılabilirliğini denetle |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.ImportExport/jobs/delete | Var olan bir işi siler. |
> | Eylem | Microsoft.ImportExport/jobs/listBitLockerKeys/action | Belirtilen iş için BitLocker anahtarları alır. |
> | Eylem | Microsoft.ImportExport/jobs/read | Belirtilen proje için özellikleri alır veya işlerin listesini döndürür. |
> | Eylem | Microsoft.ImportExport/jobs/write | Belirtilen parametrelerle bir iş oluşturur veya özellikleri veya etiketleri belirtilen iş için güncelleştirin. |
> | Eylem | Microsoft.ImportExport/locations/read | Belirtilen konum için özellikleri alır veya konumlarının listesini döndürür. |
> | Eylem | Microsoft.ImportExport/register/action | İçeri/dışarı aktarma kaynak sağlayıcısı için aboneliği kaydeder ve içeri/dışarı aktarma işleri oluşturulmasını sağlar. |

## <a name="microsoftinsights"></a>Microsoft.Insights

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Insights/ActionGroups/Delete | Bir eylem grubunu sil |
> | Eylem | Microsoft.Insights/ActionGroups/Read | Bir eylem grubunu okuma |
> | Eylem | Microsoft.Insights/ActionGroups/Write | Bir eylem grubu oluşturulamadı veya |
> | Eylem | Microsoft.Insights/ActivityLogAlerts/Activated/Action | Etkinlik günlüğü etkin uyarı |
> | Eylem | Microsoft.Insights/ActivityLogAlerts/Delete | Bir etkinlik günlüğü uyarısını silme |
> | Eylem | Microsoft.Insights/ActivityLogAlerts/Read | Bir etkinlik günlüğü uyarısını okuma |
> | Eylem | Microsoft.Insights/ActivityLogAlerts/Write | Bir etkinlik günlüğü uyarısı güncelle |
> | Eylem | Microsoft.Insights/AlertRules/Activated/Action | Klasik ölçüm uyarısı etkinleştirildi |
> | Eylem | Microsoft.Insights/AlertRules/Delete | Klasik bir ölçüm uyarısı Sil |
> | Eylem | Microsoft.Insights/AlertRules/Incidents/Read | Klasik bir ölçüm uyarı olayı okuyun |
> | Eylem | Microsoft.Insights/AlertRules/Read | Klasik bir ölçüm uyarısı okuyun |
> | Eylem | Microsoft.Insights/AlertRules/Resolved/Action | Çözümlenen Klasik ölçüm Uyarısı |
> | Eylem | Microsoft.Insights/AlertRules/Throttled/Action | Klasik ölçüm uyarı kuralı kısıtlanmış |
> | Eylem | Microsoft.Insights/AlertRules/Write | Klasik bir ölçüm uyarısı güncelle |
> | Eylem | Microsoft.Insights/AutoscaleSettings/Delete | Otomatik ölçeklendirme ayarını Sil |
> | Eylem | Microsoft.Insights/AutoscaleSettings/providers/Microsoft.Insights/diagnosticSettings/Read | Kaynağın tanılama ayarını oku |
> | Eylem | Microsoft.Insights/AutoscaleSettings/providers/Microsoft.Insights/diagnosticSettings/Write | Kaynak tanılama ayarı güncelle |
> | Eylem | Microsoft.Insights/AutoscaleSettings/providers/Microsoft.Insights/logDefinitions/Read | Günlük tanımlarını oku |
> | Eylem | Microsoft.Insights/AutoscaleSettings/providers/Microsoft.Insights/MetricDefinitions/Read | Ölçüm tanımlarını oku |
> | Eylem | Microsoft.Insights/AutoscaleSettings/Read | Otomatik ölçek ayarını oku |
> | Eylem | Microsoft.Insights/AutoscaleSettings/Scaledown/Action | Otomatik ölçeklendirme ölçeğini başlatıldı |
> | Eylem | Microsoft.Insights/AutoscaleSettings/ScaledownResult/Action | Otomatik ölçeklendirme ölçeğini tamamlandı |
> | Eylem | Microsoft.Insights/AutoscaleSettings/Scaleup/Action | Otomatik ölçeklendirme ölçeğini başlatıldı |
> | Eylem | Microsoft.Insights/AutoscaleSettings/ScaleupResult/Action | Otomatik ölçeklendirme ölçeğini tamamlandı |
> | Eylem | Microsoft.Insights/AutoscaleSettings/Write | Oluşturun veya bir otomatik ölçeklendirme ayarını güncelleştir |
> | Eylem | Microsoft.Insights/Baseline/Read | Ölçüm temel (Önizleme) okuyun |
> | Eylem | Microsoft.Insights/CalculateBaseline/Read | Ölçüm değerleri (Önizleme) için temel hesaplama |
> | Eylem | Microsoft.Insights/Components/AnalyticsItems/Delete | Bir Application Insights analitik öğesini silme |
> | Eylem | Microsoft.Insights/Components/AnalyticsItems/Read | Bir Application Insights analitik öğesini okuma |
> | Eylem | Microsoft.Insights/Components/AnalyticsItems/Write | Bir Application Insights analitik öğesi yazma |
> | Eylem | Microsoft.Insights/Components/AnalyticsTables/Action | Application Insights analitik tablosu eylemi |
> | Eylem | Microsoft.Insights/Components/AnalyticsTables/Delete | Silme bir Application Insights Analitik Tablo şemasını |
> | Eylem | Microsoft.Insights/Components/AnalyticsTables/Read | Okuma bir Application Insights Analitik Tablo şemasını |
> | Eylem | Microsoft.Insights/Components/AnalyticsTables/Write | Yazma bir Application Insights Analitik Tablo şemasını |
> | Eylem | Microsoft.Insights/Components/Annotations/Delete | Bir Application Insights ek açıklamasını silme |
> | Eylem | Microsoft.Insights/Components/Annotations/Read | Bir Application Insights ek açıklamasını okuma |
> | Eylem | Microsoft.Insights/Components/Annotations/Write | Bir Application Insights ek açıklamasını yazma |
> | Eylem | Microsoft.Insights/Components/Api/Read | Application Insights bileşen verileri API'sini okuma |
> | Eylem | Microsoft.Insights/Components/ApiKeys/Action | Bir Application Insights API anahtarını oluşturma |
> | Eylem | Microsoft.Insights/Components/ApiKeys/Delete | Bir Application Insights API anahtarını silme |
> | Eylem | Microsoft.Insights/Components/ApiKeys/Read | Bir Application Insights API anahtarını okuma |
> | Eylem | Microsoft.Insights/Components/BillingPlanForComponent/Read | Application Insights bileşeni için bir faturalama planını okuma |
> | Eylem | Microsoft.Insights/Components/CurrentBillingFeatures/Read | Application Insights bileşeni geçerli faturalama özellikleri okuma |
> | Eylem | Microsoft.Insights/Components/CurrentBillingFeatures/Write | Application Insights bileşeni geçerli faturalama özellikleri yazma |
> | Eylem | Microsoft.Insights/Components/DefaultWorkItemConfig/Read | Bir Application Insights varsayılan ALM tümleştirmesinin yapılandırmasını okuma |
> | Eylem | Microsoft.Insights/Components/Delete | Bir application ınsights bileşen yapılandırmasını silme |
> | Eylem | Microsoft.Insights/Components/Events/Read | OData sorgu biçimini kullanarak Application Insights'tan Günlük Al |
> | Eylem | Microsoft.Insights/Components/ExportConfiguration/Action | Application Insights dışa aktarım ayarları eylemi |
> | Eylem | Microsoft.Insights/Components/ExportConfiguration/Delete | Silme Application Insights dışa aktarma ayarları |
> | Eylem | Microsoft.Insights/Components/ExportConfiguration/Read | Okuma Application Insights dışa aktarma ayarları |
> | Eylem | Microsoft.Insights/Components/ExportConfiguration/Write | Yazma Application Insights dışa aktarma ayarları |
> | Eylem | Microsoft.Insights/Components/ExtendQueries/Read | Okuma Application Insights bileşeni genişletilmiş sorgu sonuçları |
> | Eylem | Microsoft.Insights/Components/Favorites/Delete | Application Insights sık Kullanılanını silme |
> | Eylem | Microsoft.Insights/Components/Favorites/Read | Application Insights sık Kullanılanını okuma |
> | Eylem | Microsoft.Insights/Components/Favorites/Write | Application Insights sık Kullanılanını yazma |
> | Eylem | Microsoft.Insights/Components/FeatureCapabilities/Read | Okuma Application Insights bileşeni özellik yetenekleri |
> | Eylem | Microsoft.Insights/Components/GetAvailableBillingFeatures/Read | Okuma Application Insights bileşeni kullanılabilir faturalama özellikleri |
> | Eylem | Microsoft.Insights/Components/GetToken/Read | Bir Application Insights bileşen belirteci okuma |
> | Eylem | Microsoft.Insights/Components/MetricDefinitions/Read | Application Insights bileşen ölçüm tanımları okuma |
> | Eylem | Microsoft.Insights/Components/Metrics/Read | Okuma Application Insights bileşen ölçümleri |
> | Eylem | Microsoft.Insights/Components/Move/Action | Bir Application Insights bileşenini başka bir kaynak grubuna veya aboneliğe taşıma |
> | Eylem | Microsoft.Insights/Components/MyAnalyticsItems/Delete | Bir Application Insights kişisel analitik öğesini silme |
> | Eylem | Microsoft.Insights/Components/MyAnalyticsItems/Read | Bir Application Insights kişisel analitik öğesini okuma |
> | Eylem | Microsoft.Insights/Components/MyAnalyticsItems/Write | Bir Application Insights kişisel analitik öğesi yazma |
> | Eylem | Microsoft.Insights/Components/MyFavorites/Read | Application Insights kişisel sık Kullanılanını okuma |
> | Eylem | Microsoft.Insights/Components/Operations/Read | Application Insights'ta uzun süren işlemlerin durumunu Al |
> | Eylem | Microsoft.Insights/Components/PricingPlans/Read | Bir Application Insights bileşen fiyatlandırma planını okuma |
> | Eylem | Microsoft.Insights/Components/PricingPlans/Write | Bir Application Insights bileşen fiyatlandırma planını yazma |
> | Eylem | Microsoft.Insights/Components/ProactiveDetectionConfigs/Read | Okuma Application Insights proaktif algılama yapılandırması |
> | Eylem | Microsoft.Insights/Components/ProactiveDetectionConfigs/Write | Application Insights proaktif algılama yapılandırmasını yazma |
> | Eylem | Microsoft.Insights/Components/providers/Microsoft.Insights/MetricDefinitions/Read | Ölçüm tanımlarını oku |
> | Eylem | Microsoft.Insights/Components/Purge/Action | Application Insights verilerini temizleme |
> | Eylem | Microsoft.Insights/Components/Query/Read | Application Insights günlüklerine karşı Sorgu Çalıştır |
> | Eylem | Microsoft.Insights/Components/QuotaStatus/Read | Application Insights bileşen kotası durumu okuma |
> | Eylem | Microsoft.Insights/Components/Read | Bir application ınsights bileşen yapılandırmasını okuma |
> | Eylem | Microsoft.Insights/Components/SyntheticMonitorLocations/Read | Okuma Application Insights Web testi konumları |
> | Eylem | Microsoft.Insights/Components/Webtests/Read | Bir Web testi yapılandırmasını okuma |
> | Eylem | Microsoft.Insights/Components/WorkItemConfigs/Delete | Bir Application Insights ALM tümleştirmesinin yapılandırmasını silme |
> | Eylem | Microsoft.Insights/Components/WorkItemConfigs/Read | Bir Application Insights ALM tümleştirmesinin yapılandırmasını okuma |
> | Eylem | Microsoft.Insights/Components/WorkItemConfigs/Write | Bir Application Insights ALM tümleştirmesinin yapılandırmasını yazma |
> | Eylem | Microsoft.Insights/Components/Write | Bir application ınsights bileşen yapılandırmasına yazma |
> | Eylem | Microsoft.Insights/DiagnosticSettings/Delete | Kaynağın tanılama ayarını Sil |
> | Eylem | Microsoft.Insights/DiagnosticSettings/Read | Kaynağın tanılama ayarını oku |
> | Eylem | Microsoft.Insights/DiagnosticSettings/Write | Kaynak tanılama ayarı güncelle |
> | Eylem | Microsoft.Insights/EventCategories/Read | Kullanılabilen etkinlik günlüğü olay kategorisi okuma |
> | Eylem | Microsoft.Insights/eventtypes/digestevents/Read | Okuma yönetim olayı türü özetini |
> | Eylem | Microsoft.Insights/eventtypes/values/Read | Okuma etkinlik günlüğü olayları |
> | Eylem | Microsoft.Insights/ExtendedDiagnosticSettings/Delete | Bir ağ akış günlüğü tanılama ayarını Sil |
> | Eylem | Microsoft.Insights/ExtendedDiagnosticSettings/Read | Bir ağ akış günlüğü tanılama ayarını oku |
> | Eylem | Microsoft.Insights/ExtendedDiagnosticSettings/Write | Bir ağ akışı günlük tanılama ayarı güncelle |
> | Eylem | Microsoft.Insights/ListMigrationDate/Action | Abonelik geçiş tarihini geri alın |
> | Eylem | Microsoft.Insights/ListMigrationDate/Read | Get abonelik geçiş tarihini geri |
> | Eylem | Microsoft.Insights/LogDefinitions/Read | Günlük tanımlarını oku |
> | Eylem | Microsoft.Insights/LogProfiles/Delete | Bir etkinlik günlüğü günlük profilini silme |
> | Eylem | Microsoft.Insights/LogProfiles/Read | Bir etkinlik günlüğü günlük profilini okuma |
> | Eylem | Microsoft.Insights/LogProfiles/Write | Bir etkinlik günlüğü günlük profilini güncelle |
> | Eylem | Microsoft.Insights/Logs/ADAssessmentRecommendation/Read | ADAssessmentRecommendation tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/ADReplicationResult/Read | ADReplicationResult tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/ADSecurityAssessmentRecommendation/Read | ADSecurityAssessmentRecommendation tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/Alert/Read | Uyarı tablosundan veri okuma |
> | Eylem | Microsoft.Insights/Logs/AlertHistory/Read | AlertHistory tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/ApplicationInsights/Read | Applicationınsights tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/AzureActivity/Read | AzureActivity tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/AzureMetrics/Read | AzureMetrics tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/BoundPort/Read | BoundPort tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/CommonSecurityLog/Read | CommonSecurityLog tablosundan veri okuma |
> | Eylem | Microsoft.Insights/Logs/ComputerGroup/Read | ComputerGroup tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/ConfigurationChange/Read | ConfigurationChange tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/ConfigurationData/Read | ConfigurationData tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/ContainerImageInventory/Read | ContainerImageInventory tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/ContainerInventory/Read | ContainerInventory tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/ContainerLog/Read | ContainerLog tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/ContainerServiceLog/Read | ContainerServiceLog tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/DeviceAppCrash/Read | DeviceAppCrash tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/DeviceAppLaunch/Read | DeviceAppLaunch tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/DeviceCalendar/Read | DeviceCalendar tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/DeviceCleanup/Read | DeviceCleanup tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/DeviceConnectSession/Read | DeviceConnectSession tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/DeviceEtw/Read | DeviceEtw tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/DeviceHardwareHealth/Read | DeviceHardwareHealth tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/DeviceHealth/Read | DeviceHealth tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/DeviceHeartbeat/Read | DeviceHeartbeat tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/DeviceSkypeHeartbeat/Read | DeviceSkypeHeartbeat tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/DeviceSkypeSignIn/Read | DeviceSkypeSignIn tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/DeviceSleepState/Read | DeviceSleepState tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/DHAppFailure/Read | DHAppFailure tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/DHAppReliability/Read | DHAppReliability tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/DHDriverReliability/Read | DHDriverReliability tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/DHLogonFailures/Read | DHLogonFailures tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/DHLogonMetrics/Read | DHLogonMetrics tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/DHOSCrashData/Read | DHOSCrashData tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/DHOSReliability/Read | DHOSReliability tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/DHWipAppLearning/Read | DHWipAppLearning tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/DnsEvents/Read | DnsEvents tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/DnsInventory/Read | DnsInventory tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/ETWEvent/Read | ETWEvent tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/Event/Read | Olay tablosunu okuma verileri |
> | Eylem | Microsoft.Insights/Logs/ExchangeAssessmentRecommendation/Read | ExchangeAssessmentRecommendation tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/ExchangeOnlineAssessmentRecommendation/Read | ExchangeOnlineAssessmentRecommendation tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/Heartbeat/Read | Sinyal tablosunda veri okuma |
> | Eylem | Microsoft.Insights/Logs/IISAssessmentRecommendation/Read | IISAssessmentRecommendation tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/InboundConnection/Read | InboundConnection tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/KubeNodeInventory/Read | KubeNodeInventory tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/KubePodInventory/Read | KubePodInventory tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/LinuxAuditLog/Read | LinuxAuditLog tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/MAApplication/Read | MAApplication tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/MAApplicationHealth/Read | MAApplicationHealth tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/MAApplicationHealthAlternativeVersions/Read | MAApplicationHealthAlternativeVersions tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/MAApplicationHealthIssues/Read | MAApplicationHealthIssues tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/MAApplicationInstance/Read | MAApplicationInstance tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/MAApplicationInstanceReadiness/Read | MAApplicationInstanceReadiness tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/MAApplicationReadiness/Read | MAApplicationReadiness tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/MADeploymentPlan/Read | MADeploymentPlan tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/MADevice/Read | MADevice tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/MADevicePnPHealth/Read | MADevicePnPHealth tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/MADevicePnPHealthAlternativeVersions/Read | MADevicePnPHealthAlternativeVersions tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/MADevicePnPHealthIssues/Read | MADevicePnPHealthIssues tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/MADeviceReadiness/Read | MADeviceReadiness tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/MADriverInstanceReadiness/Read | MADriverInstanceReadiness tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/MADriverReadiness/Read | MADriverReadiness tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/MAOfficeAddin/Read | MAOfficeAddin tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/MAOfficeAddinHealth/Read | MAOfficeAddinHealth tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/MAOfficeAddinHealthIssues/Read | MAOfficeAddinHealthIssues tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/MAOfficeAddinInstance/Read | MAOfficeAddinInstance tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/MAOfficeAddinInstanceReadiness/Read | MAOfficeAddinInstanceReadiness tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/MAOfficeAddinReadiness/Read | MAOfficeAddinReadiness tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/MAOfficeApp/Read | MAOfficeApp tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/MAOfficeAppHealth/Read | MAOfficeAppHealth tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/MAOfficeAppInstance/Read | MAOfficeAppInstance tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/MAOfficeAppReadiness/Read | MAOfficeAppReadiness tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/MAOfficeBuildInfo/Read | MAOfficeBuildInfo tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/MAOfficeCurrencyAssessment/Read | MAOfficeCurrencyAssessment tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/MAOfficeCurrencyAssessmentDailyCounts/Read | MAOfficeCurrencyAssessmentDailyCounts tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/MAOfficeDeploymentStatus/Read | MAOfficeDeploymentStatus tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/MAOfficeMacroHealth/Read | MAOfficeMacroHealth tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/MAOfficeMacroHealthIssues/Read | MAOfficeMacroHealthIssues tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/MAOfficeMacroIssueInstanceReadiness/Read | MAOfficeMacroIssueInstanceReadiness tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/MAOfficeMacroIssueReadiness/Read | MAOfficeMacroIssueReadiness tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/MAOfficeMacroSummary/Read | MAOfficeMacroSummary tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/MAOfficeSuite/Read | MAOfficeSuite tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/MAOfficeSuiteInstance/Read | MAOfficeSuiteInstance tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/MAProposedPilotDevices/Read | MAProposedPilotDevices tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/MAWindowsBuildInfo/Read | MAWindowsBuildInfo tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/MAWindowsCurrencyAssessment/Read | MAWindowsCurrencyAssessment tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/MAWindowsCurrencyAssessmentDailyCounts/Read | MAWindowsCurrencyAssessmentDailyCounts tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/MAWindowsDeploymentStatus/Read | MAWindowsDeploymentStatus tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/MAWindowsSysReqInstanceReadiness/Read | MAWindowsSysReqInstanceReadiness tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/NetworkMonitoring/Read | NetworkMonitoring tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/OfficeActivity/Read | OfficeActivity tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/Operation/Read | İşlem tablodan veri okuma |
> | Eylem | Microsoft.Insights/Logs/OutboundConnection/Read | OutboundConnection tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/Perf/Read | İyileştirilmiş tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/ProtectionStatus/Read | ProtectionStatus tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/Read | Tüm günlükler verileri okuma |
> | Eylem | Microsoft.Insights/Logs/ReservedAzureCommonFields/Read | ReservedAzureCommonFields tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/ReservedCommonFields/Read | ReservedCommonFields tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/SCCMAssessmentRecommendation/Read | SCCMAssessmentRecommendation tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/SCOMAssessmentRecommendation/Read | SCOMAssessmentRecommendation tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/SecurityAlert/Read | SecurityAlert tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/SecurityBaseline/Read | SecurityBaseline tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/SecurityBaselineSummary/Read | SecurityBaselineSummary tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/SecurityDetection/Read | SecurityDetection tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/SecurityEvent/Read | SecurityEvent tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/ServiceFabricOperationalEvent/Read | ServiceFabricOperationalEvent tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/ServiceFabricReliableActorEvent/Read | ServiceFabricReliableActorEvent tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/ServiceFabricReliableServiceEvent/Read | ServiceFabricReliableServiceEvent tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/SfBAssessmentRecommendation/Read | SfBAssessmentRecommendation tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/SfBOnlineAssessmentRecommendation/Read | SfBOnlineAssessmentRecommendation tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/SharePointOnlineAssessmentRecommendation/Read | SharePointOnlineAssessmentRecommendation tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/SPAssessmentRecommendation/Read | SPAssessmentRecommendation tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/SQLAssessmentRecommendation/Read | SQLAssessmentRecommendation tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/SQLQueryPerformance/Read | SQLQueryPerformance tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/Syslog/Read | Syslog tablosunda veri okuma |
> | Eylem | Microsoft.Insights/Logs/SysmonEvent/Read | SysmonEvent tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/UAApp/Read | UAApp tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/UAComputer/Read | UAComputer tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/UAComputerRank/Read | UAComputerRank tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/UADriver/Read | UADriver tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/UADriverProblemCodes/Read | UADriverProblemCodes tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/UAFeedback/Read | UAFeedback tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/UAHardwareSecurity/Read | UAHardwareSecurity tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/UAIESiteDiscovery/Read | UAIESiteDiscovery tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/UAOfficeAddIn/Read | UAOfficeAddIn tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/UAProposedActionPlan/Read | UAProposedActionPlan tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/UASysReqIssue/Read | UASysReqIssue tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/UAUpgradedComputer/Read | UAUpgradedComputer tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/Update/Read | Güncelleştirme tablosunda veri okuma |
> | Eylem | Microsoft.Insights/Logs/UpdateRunProgress/Read | UpdateRunProgress tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/UpdateSummary/Read | UpdateSummary tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/Usage/Read | Kullanım tablosunda veri okuma |
> | Eylem | Microsoft.Insights/Logs/W3CIISLog/Read | W3cııslog tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/WaaSDeploymentStatus/Read | WaaSDeploymentStatus tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/WaaSInsiderStatus/Read | WaaSInsiderStatus tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/WaaSUpdateStatus/Read | WaaSUpdateStatus tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/WDAVStatus/Read | WDAVStatus tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/WDAVThreat/Read | WDAVThreat tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/WindowsClientAssessmentRecommendation/Read | WindowsClientAssessmentRecommendation tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/WindowsFirewall/Read | WindowsFirewall tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/WindowsServerAssessmentRecommendation/Read | WindowsServerAssessmentRecommendation tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/WireData/Read | İletilen veri tablosunda veri okuma |
> | Eylem | Microsoft.Insights/Logs/WUDOAggregatedStatus/Read | WUDOAggregatedStatus tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/Logs/WUDOStatus/Read | WUDOStatus tablodaki verileri okuma |
> | Eylem | Microsoft.Insights/MetricAlerts/Delete | Ölçüm uyarısını silme |
> | Eylem | Microsoft.Insights/MetricAlerts/Read | Ölçüm uyarısı okuyun |
> | Eylem | Microsoft.Insights/MetricAlerts/Status/Read | Ölçüm uyarı durumunu okuma |
> | Eylem | Microsoft.Insights/MetricAlerts/Write | Ölçüm uyarısı güncelle |
> | Eylem | Microsoft.Insights/MetricBaselines/Read | Ölçüm okuma temelleri |
> | Eylem | Microsoft.Insights/MetricDefinitions/Microsoft.Insights/Read | Ölçüm tanımlarını oku |
> | Eylem | Microsoft.Insights/MetricDefinitions/providers/Microsoft.Insights/Read | Ölçüm tanımlarını oku |
> | Eylem | Microsoft.Insights/MetricDefinitions/Read | Ölçüm tanımlarını oku |
> | Eylem | Microsoft.Insights/Metricnamespaces/Read | Okuma ölçüm ad alanları |
> | Eylem | Microsoft.Insights/Metrics/Action | Ölçüm eylemi |
> | Eylem | Microsoft.Insights/Metrics/Microsoft.Insights/Read | Ölçümleri okuma |
> | Eylem | Microsoft.Insights/Metrics/providers/Metrics/Read | Ölçümleri okuma |
> | Eylem | Microsoft.Insights/Metrics/Read | Ölçümleri okuma |
> | DataAction | Microsoft.Insights/Metrics/Write | Ölçümleri Yaz |
> | Eylem | Microsoft.Insights/MigrateToNewpricingModel/Action | Aboneliği yeni fiyatlandırma modeline geçirme |
> | Eylem | Microsoft.Insights/Operations/Read | Okuma İşlemleri |
> | Eylem | Microsoft.Insights/Register/Action | Microsoft Insights sağlayıcısını Kaydet |
> | Eylem | Microsoft.Insights/RollbackToLegacyPricingModel/Action | Aboneliği eski fiyatlandırma modeline geri alma |
> | Eylem | Microsoft.Insights/ScheduledQueryRules/Delete | Zamanlanmış sorgu kuralını silme |
> | Eylem | Microsoft.Insights/ScheduledQueryRules/Read | Zamanlanmış sorgu kuralını okuma |
> | Eylem | Microsoft.Insights/ScheduledQueryRules/Write | Zamanlanmış sorgu kuralını yazma |
> | Eylem | Microsoft.Insights/Tenants/Register/Action | Microsoft Insights sağlayıcısını başlatır |
> | Eylem | Microsoft.Insights/Unregister/Action | Microsoft Insights sağlayıcısını Kaydet |
> | Eylem | Microsoft.Insights/Webtests/Delete | Bir Web testi yapılandırmasını silme |
> | Eylem | Microsoft.Insights/Webtests/GetToken/Read | Bir Web testi belirtecini okuma |
> | Eylem | Microsoft.Insights/Webtests/MetricDefinitions/Read | Bir Web testi ölçüm tanımları okuma |
> | Eylem | Microsoft.Insights/Webtests/Metrics/Read | Web testinin ölçümlerini okuma |
> | Eylem | Microsoft.Insights/Webtests/Read | Bir Web testi yapılandırmasını okuma |
> | Eylem | Microsoft.Insights/Webtests/Write | Bir Web testi yapılandırmasına yazma |

## <a name="microsoftintune"></a>Microsoft.Intune

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Intune/diagnosticsettings/delete | Tanılama ayarını siliniyor |
> | Eylem | Microsoft.Intune/diagnosticsettings/read | Tanılama ayarını okuma |
> | Eylem | Microsoft.Intune/diagnosticsettings/write | Tanılama ayarını yazma |
> | Eylem | Microsoft.Intune/diagnosticsettingscategories/read | Tanılama ayarı kategorilerini okuma |

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.IoTCentral/appTemplates/action | Tüm kullanılabilir uygulama şablonları Azure IOT Central alır |
> | Eylem | Microsoft.IoTCentral/checkNameAvailability/action | IOT Central uygulamasına adı kullanılabilir olup olmadığını denetler |
> | Eylem | Microsoft.IoTCentral/checkSubdomainAvailability/action | Bir IOT Central uygulamasına alt etki alanının kullanılabilir olup olmadığını denetler |
> | Eylem | Microsoft.IoTCentral/IoTApps/delete | IOT Central uygulamaları siler |
> | Eylem | Microsoft.IoTCentral/IoTApps/read | Tek bir IOT Central uygulamasına alır |
> | Eylem | Microsoft.IoTCentral/IoTApps/write | Oluşturur veya bir IOT Central uygulamaları güncelleştirir |
> | Eylem | Microsoft.IoTCentral/operations/read | IOT Central uygulamaları kullanılabilir tüm işlemleri alır |
> | Eylem | Microsoft.IoTCentral/register/action | Azure IOT Central kaynak sağlayıcısı için aboneliği kaydedin |

## <a name="microsoftiotspaces"></a>Microsoft.IoTSpaces

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.IoTSpaces/Graph/delete | Microsoft.IoTSpaces grafik kaynağı siler |
> | Eylem | Microsoft.IoTSpaces/Graph/read | Microsoft.IoTSpaces grafik kaynakları alır |
> | Eylem | Microsoft.IoTSpaces/Graph/write | Microsoft.IoTSpaces grafik kaynağı oluşturma |
> | Eylem | Microsoft.IoTSpaces/register/action | Kaynakları oluşturmayı etkinleştirmek Microsoft.IoTSpaces grafik kaynak sağlayıcısı için aboneliği kaydedin |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.KeyVault/checkNameAvailability/read | Bir anahtar kasası adının geçerli olduğundan ve kullanılmadığını denetler |
> | Eylem | Microsoft.KeyVault/deletedVaults/read | Geçici silinen anahtar kasalarının özelliklerini görüntüleyin |
> | Eylem | Microsoft.KeyVault/hsmPools/delete | Bir HSM havuzunu silin |
> | Eylem | Microsoft.KeyVault/hsmPools/joinVault/action | Bir HSM havuzuna anahtar kasası katılın |
> | Eylem | Microsoft.KeyVault/hsmPools/read | Bir HSM havuzunun özelliklerini görüntüleyin |
> | Eylem | Microsoft.KeyVault/hsmPools/write | Varolan bir HSM havuzunun özelliklerini güncelleştirme yeni HSM havuzu oluşturma |
> | Eylem | Microsoft.KeyVault/locations/deletedVaults/purge/action | Geçici silinen anahtar kasasını Temizle |
> | Eylem | Microsoft.KeyVault/locations/deletedVaults/read | Geçici silinen anahtar kasasını özelliklerini görüntüleme |
> | Eylem | Microsoft.KeyVault/locations/deleteVirtualNetworkOrSubnets/action | Microsoft.KeyVault bir sanal ağın veya alt ağın silindiğini bildirir |
> | Eylem | Microsoft.KeyVault/locations/operationResults/read | Uzun süre çalışan işlemin sonucunu denetleyin |
> | Eylem | Microsoft.KeyVault/operations/read | Microsoft.KeyVault kaynak sağlayıcısındaki kullanılabilir işlemleri listele |
> | Eylem | Microsoft.KeyVault/register/action | Bir aboneliği kaydeder |
> | Eylem | Microsoft.KeyVault/unregister/action | Abonelik kaydını siler |
> | Eylem | Microsoft.KeyVault/vaults/accessPolicies/write | Var olan bir erişim ilkesi birleştirme veya değiştirme yoluyla güncelleştirin veya yeni bir erişim ilkesi bir kasaya ekler. |
> | Eylem | Microsoft.KeyVault/vaults/delete | Bir anahtar kasasını silme |
> | Eylem | Microsoft.KeyVault/vaults/deploy/action | Azure kaynakları dağıtılırken bir anahtar kasasındaki gizli dizilere erişim sağlar |
> | Eylem | Microsoft.KeyVault/vaults/eventGridFilters/delete | Microsoft.KeyVault EventGrid aboneliğiniz Key Vault için silindiğini bildirir |
> | Eylem | Microsoft.KeyVault/vaults/eventGridFilters/read | Microsoft.KeyVault EventGrid aboneliğiniz Key Vault için görüntülenmekte olan bildirir. |
> | Eylem | Microsoft.KeyVault/vaults/eventGridFilters/write | Key Vault için yeni bir EventGrid abonelik oluşturulduğunu Microsoft.KeyVault bildirir |
> | Eylem | Microsoft.KeyVault/vaults/read | Bir anahtar kasasının özelliklerini görüntüleyin |
> | Eylem | Microsoft.KeyVault/vaults/secrets/read | Gizli dizi değeri hariç özelliklerini görüntüleyin. |
> | Eylem | Microsoft.KeyVault/vaults/secrets/write | Yeni bir gizli dizi oluşturun veya var olan bir gizli dizi değerini güncelleştirin. |
> | Eylem | Microsoft.KeyVault/vaults/write | Yeni bir anahtar kasası oluşturun veya var olan bir anahtar kasasının özelliklerini güncelleştirin |

## <a name="microsoftkusto"></a>Microsoft.Kusto

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Kusto/Clusters/Activate/action | Küme başlatır. |
> | Eylem | Microsoft.Kusto/Clusters/AttachedDatabaseConfigurations/delete | Bir veritabanına ekli yapılandırma kaynağı siler. |
> | Eylem | Microsoft.Kusto/Clusters/AttachedDatabaseConfigurations/read | Bir veritabanına ekli yapılandırma kaynağı okur. |
> | Eylem | Microsoft.Kusto/Clusters/AttachedDatabaseConfigurations/write | Bir veritabanına ekli yapılandırma kaynağı yazar. |
> | Eylem | Microsoft.Kusto/Clusters/CheckNameAvailability/action | Küme adının kullanılabilirliğini denetler. |
> | Eylem | Microsoft.Kusto/Clusters/Databases/AddPrincipals/action | Veritabanı sorumluları ekler. |
> | Eylem | Microsoft.Kusto/Clusters/Databases/CheckNameAvailability/action | Verilen tür için ad kullanılabilirliğini denetler. |
> | Eylem | Microsoft.Kusto/Clusters/Databases/DataConnections/delete | Veri bağlantıları kaynak siler. |
> | Eylem | Microsoft.Kusto/Clusters/Databases/DataConnections/read | Veri bağlantıları kaynak okur. |
> | Eylem | Microsoft.Kusto/Clusters/Databases/DataConnections/write | Bir veri bağlantıları kaynak yazar. |
> | Eylem | Microsoft.Kusto/Clusters/Databases/DataConnectionValidation/action | Veritabanı veri bağlantısı doğrular. |
> | Eylem | Microsoft.Kusto/Clusters/Databases/delete | Veritabanı kaynağı siler. |
> | Eylem | Microsoft.Kusto/Clusters/Databases/EventHubConnections/delete | Bir olay hub'ı bağlantı kaynağı siler. |
> | Eylem | Microsoft.Kusto/Clusters/Databases/EventHubConnections/read | Bir olay hub'ı bağlantıları kaynak okur. |
> | Eylem | Microsoft.Kusto/Clusters/Databases/EventHubConnections/write | Bir olay hub'ı bağlantıları kaynak yazar. |
> | Eylem | Microsoft.Kusto/Clusters/Databases/EventHubConnectionValidation/action | Veritabanı olay hub'ı bağlantısı doğrular. |
> | Eylem | Microsoft.Kusto/Clusters/Databases/ListPrincipals/action | Listeleri sorumluları veritabanı. |
> | Eylem | Microsoft.Kusto/Clusters/Databases/read | Veritabanı kaynak okur. |
> | Eylem | Microsoft.Kusto/Clusters/Databases/RemovePrincipals/action | Kaldırır sorumluları veritabanı. |
> | Eylem | Microsoft.Kusto/Clusters/Databases/write | Veritabanı kaynak yazar. |
> | Eylem | Microsoft.Kusto/Clusters/Deactivate/action | Küme durdurulur. |
> | Eylem | Microsoft.Kusto/Clusters/delete | Küme kaynağı siler. |
> | Eylem | Microsoft.Kusto/Clusters/read | Küme kaynağı okur. |
> | Eylem | Microsoft.Kusto/Clusters/SKUs/read | Bir küme SKU kaynak okur. |
> | Eylem | Microsoft.Kusto/Clusters/Start/action | Küme başlatır. |
> | Eylem | Microsoft.Kusto/Clusters/Stop/action | Küme durdurulur. |
> | Eylem | Microsoft.Kusto/Clusters/write | Küme kaynağı yazar. |
> | Eylem | Microsoft.Kusto/Locations/CheckNameAvailability/action | Kaynak adının kullanılabilirliğini denetler. |
> | Eylem | Microsoft.Kusto/locations/operationresults/read | İşlem kaynaklarını okur |
> | Eylem | Microsoft.Kusto/Operations/read | İşlem kaynaklarını okur |
> | Eylem | Microsoft.Kusto/Register/action | Kusto kaynak sağlayıcısı için aboneliği kaydeder. |
> | Eylem | Microsoft.Kusto/SKUs/read | Bir SKU kaynak okur. |
> | Eylem | Microsoft.Kusto/Unregister/action | Kusto kaynak sağlayıcısı için abonelik kaydını siler. |

## <a name="microsoftlabservices"></a>Microsoft.LabServices

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.LabServices/labAccounts/CreateLab/action | Bir laboratuvar bir laboratuvar hesabı oluşturun. |
> | Eylem | Microsoft.LabServices/labAccounts/delete | Laboratuvar hesaplarını silin. |
> | Eylem | Microsoft.LabServices/labAccounts/galleryImages/delete | Galeri görüntüleri silin. |
> | Eylem | Microsoft.LabServices/labAccounts/galleryImages/read | Galeri görüntüleri okuyun. |
> | Eylem | Microsoft.LabServices/labAccounts/galleryImages/write | Ekleyin veya galeri görüntüleri değiştirin. |
> | Eylem | Microsoft.LabServices/labAccounts/GetRegionalAvailability/action | Bir laboratuvar hesabı altında yapılandırılmış her boyutu kategori bölgesel kullanılabilirliği hakkında bilgi alın |
> | Eylem | Microsoft.LabServices/labAccounts/labs/AddUsers/action | Kullanıcılar bir laboratuvara ekleme |
> | Eylem | Microsoft.LabServices/labAccounts/labs/delete | Labs silin. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/environmentSettings/ClaimAny/action | Rastgele bir ortam için ortam ayarları bir kullanıcı talepleri |
> | Eylem | Microsoft.LabServices/labAccounts/labs/environmentSettings/delete | Ortam ayarı silin. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/Claim/action | Ortamı talep ve kullanıcıya atar |
> | Eylem | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/delete | Ortamları silin. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/read | Ortamları okuma. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/ResetPassword/action | Bir ortamda, kullanıcı parolasını sıfırlar |
> | Eylem | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/Start/action | Bir ortam, ortam içindeki tüm kaynaklar başlatarak başlatır. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/Stop/action | Bir ortam ortam içindeki tüm kaynaklar durdurarak durdurur. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/write | Ekleyin veya ortamları değiştirin. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/environmentSettings/Publish/action | Kaynaklar için bir ortam ayarını Laboratuvar/ortam ayarı geçerli durumuna bağlı hükümlerine/deprovisions gereklidir. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/environmentSettings/read | Ortam ayarı okuyun. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/environmentSettings/ResetPassword/action | Şablon sanal makinede parolasını sıfırlar. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/environmentSettings/SaveImage/action | Laboratuvar hesabı paylaşılan galerideki geçerli şablon görüntüsü kaydeder |
> | Eylem | Microsoft.LabServices/labAccounts/labs/environmentSettings/schedules/delete | Tabloları silin. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/environmentSettings/schedules/read | Okuma zamanlar. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/environmentSettings/schedules/write | Ekleyin veya zamanlamalarını değiştirin. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/environmentSettings/Start/action | Şablon şablon içinde tüm kaynaklar başlatarak başlatır. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/environmentSettings/Stop/action | Şablon şablon içinde tüm kaynaklar durdurarak durdurur. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/environmentSettings/write | Ekleyin veya ortam ayarını değiştirin. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/read | Labs okuyun. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/Register/action | Yönetilen laboratuvara kaydedin. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/SendEmail/action | Laboratuvara kayıt bağlantısı içeren e-posta Gönder |
> | Eylem | Microsoft.LabServices/labAccounts/labs/users/delete | Kullanıcıları silin. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/users/read | Kullanıcılar okuyun. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/users/write | Ekleyin veya kullanıcıları değiştirin. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/write | Ekleyin veya labs değiştirin. |
> | Eylem | Microsoft.LabServices/labAccounts/read | Laboratuvar hesaplarını okuyun. |
> | Eylem | Microsoft.LabServices/labAccounts/sharedGalleries/delete | Sharedgalleries silin. |
> | Eylem | Microsoft.LabServices/labAccounts/sharedGalleries/read | Sharedgalleries okuyun. |
> | Eylem | Microsoft.LabServices/labAccounts/sharedGalleries/write | Ekleyin veya sharedgalleries değiştirin. |
> | Eylem | Microsoft.LabServices/labAccounts/sharedImages/delete | Sharedimages silin. |
> | Eylem | Microsoft.LabServices/labAccounts/sharedImages/read | Sharedimages okuyun. |
> | Eylem | Microsoft.LabServices/labAccounts/sharedImages/write | Ekleyin veya sharedimages değiştirin. |
> | Eylem | Microsoft.LabServices/labAccounts/write | Ekleyin veya Laboratuvar hesaplarını değiştirin. |
> | Eylem | Microsoft.LabServices/locations/operations/read | Okuma işlemleri. |
> | Eylem | Microsoft.LabServices/register/action | Aboneliği kaydeder |
> | Eylem | Microsoft.LabServices/users/GetEnvironment/action | Sanal makine ayrıntılarını alır. |
> | Eylem | Microsoft.LabServices/users/GetOperationBatchStatus/action | Toplu işlem durumunu Al |
> | Eylem | Microsoft.LabServices/users/GetOperationStatus/action | Uzun süre çalışan işlemin durumunu alır |
> | Eylem | Microsoft.LabServices/users/GetPersonalPreferences/action | Bir kullanıcının kişisel tercihlerini alır |
> | Eylem | Microsoft.LabServices/users/ListAllEnvironments/action | Kullanıcı için tüm ortamları listeler |
> | Eylem | Microsoft.LabServices/users/ListEnvironments/action | Kullanıcı için ortamları |
> | Eylem | Microsoft.LabServices/users/ListLabs/action | Kullanıcı için Labs listeleyin. |
> | Eylem | Microsoft.LabServices/users/Register/action | Yönetilen bir laboratuvar için bir kullanıcı kaydı |
> | Eylem | Microsoft.LabServices/users/ResetPassword/action | Bir ortamda, kullanıcı parolasını sıfırlar |
> | Eylem | Microsoft.LabServices/users/StartEnvironment/action | Bir ortam, ortam içindeki tüm kaynaklar başlatarak başlatır. |
> | Eylem | Microsoft.LabServices/users/StopEnvironment/action | Bir ortam ortam içindeki tüm kaynaklar durdurarak durdurur. |

## <a name="microsoftlogic"></a>Microsoft.Logic

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Logic/integrationAccounts/agreements/delete | Tümleştirme hesabındaki sözleşmeyi siler. |
> | Eylem | Microsoft.Logic/integrationAccounts/agreements/listContentCallbackUrl/action | Tümleştirme hesabındaki sözleşme içeriği için geri çağırma URL'sini alır. |
> | Eylem | Microsoft.Logic/integrationAccounts/agreements/read | Tümleştirme hesabındaki sözleşmeyi okur. |
> | Eylem | Microsoft.Logic/integrationAccounts/agreements/write | Tümleştirme hesabındaki sözleşmeyi güncelleştirir veya oluşturur. |
> | Eylem | Microsoft.Logic/integrationAccounts/assemblies/delete | Tümleştirme hesabındaki bütünleştirilmiş kodu siler. |
> | Eylem | Microsoft.Logic/integrationAccounts/assemblies/listContentCallbackUrl/action | Tümleştirme hesabındaki bütünleştirilmiş kod içeriği için geri çağırma URL'sini alır. |
> | Eylem | Microsoft.Logic/integrationAccounts/assemblies/read | Tümleştirme hesabındaki bütünleştirilmiş kodu okur. |
> | Eylem | Microsoft.Logic/integrationAccounts/assemblies/write | Tümleştirme hesabındaki bütünleştirilmiş kodu güncelleştirir veya oluşturur. |
> | Eylem | Microsoft.Logic/integrationAccounts/batchConfigurations/delete | Tümleştirme hesabındaki toplu yapılandırmayı siler. |
> | Eylem | Microsoft.Logic/integrationAccounts/batchConfigurations/read | Tümleştirme hesabındaki toplu yapılandırmayı okur. |
> | Eylem | Microsoft.Logic/integrationAccounts/batchConfigurations/write | Tümleştirme hesabındaki toplu yapılandırmayı güncelleştirir veya oluşturur. |
> | Eylem | Microsoft.Logic/integrationAccounts/certificates/delete | Tümleştirme hesabındaki sertifikayı siler. |
> | Eylem | Microsoft.Logic/integrationAccounts/certificates/read | Tümleştirme hesabındaki sertifikayı okur. |
> | Eylem | Microsoft.Logic/integrationAccounts/certificates/write | Tümleştirme hesabındaki sertifikayı güncelleştirir veya oluşturur. |
> | Eylem | Microsoft.Logic/integrationAccounts/delete | Tümleştirme hesabını siler. |
> | Eylem | Microsoft.Logic/integrationAccounts/join/action | Tümleştirme hesabına katılır. |
> | Eylem | Microsoft.Logic/integrationAccounts/listCallbackUrl/action | Tümleştirme hesabı için geri çağırma URL'sini alır. |
> | Eylem | Microsoft.Logic/integrationAccounts/listKeyVaultKeys/action | Anahtar kasasındaki anahtarları alır. |
> | Eylem | Microsoft.Logic/integrationAccounts/logTrackingEvents/action | Tümleştirme hesabındaki izleme olaylarını kaydeder. |
> | Eylem | Microsoft.Logic/integrationAccounts/maps/delete | Tümleştirme hesabındaki eşlemeyi siler. |
> | Eylem | Microsoft.Logic/integrationAccounts/maps/listContentCallbackUrl/action | Tümleştirme hesabındaki eşleme içeriği için geri çağırma URL'sini alır. |
> | Eylem | Microsoft.Logic/integrationAccounts/maps/read | Tümleştirme hesabındaki eşlemeyi okur. |
> | Eylem | Microsoft.Logic/integrationAccounts/maps/write | Tümleştirme hesabındaki eşlemeyi güncelleştirir veya oluşturur. |
> | Eylem | Microsoft.Logic/integrationAccounts/partners/delete | Tümleştirme hesabındaki iş ortağını siler. |
> | Eylem | Microsoft.Logic/integrationAccounts/partners/listContentCallbackUrl/action | Tümleştirme hesabındaki iş ortağı içeriği için geri çağırma URL'sini alır. |
> | Eylem | Microsoft.Logic/integrationAccounts/partners/read | Tümleştirme hesabındaki iş ortağını okur. |
> | Eylem | Microsoft.Logic/integrationAccounts/partners/write | Tümleştirme hesabındaki iş ortağını güncelleştirir veya oluşturur. |
> | Eylem | Microsoft.Logic/integrationAccounts/providers/Microsoft.Insights/logDefinitions/read | Tümleştirme hesabı günlük tanımlarını okur. |
> | Eylem | Microsoft.Logic/integrationAccounts/read | Tümleştirme hesabını okur. |
> | Eylem | Microsoft.Logic/integrationAccounts/regenerateAccessKey/action | Erişim anahtarı parolalarını yeniden oluşturur. |
> | Eylem | Microsoft.Logic/integrationAccounts/schemas/delete | Tümleştirme hesabındaki şemayı siler. |
> | Eylem | Microsoft.Logic/integrationAccounts/schemas/listContentCallbackUrl/action | Tümleştirme hesabındaki şema içeriği için geri çağırma URL'sini alır. |
> | Eylem | Microsoft.Logic/integrationAccounts/schemas/read | Tümleştirme hesabındaki şemayı okur. |
> | Eylem | Microsoft.Logic/integrationAccounts/schemas/write | Tümleştirme hesabındaki şemayı güncelleştirir veya oluşturur. |
> | Eylem | Microsoft.Logic/integrationAccounts/sessions/delete | Tümleştirme hesabındaki oturumu siler. |
> | Eylem | Microsoft.Logic/integrationAccounts/sessions/read | Tümleştirme hesabındaki toplu yapılandırmayı okur. |
> | Eylem | Microsoft.Logic/integrationAccounts/sessions/write | Tümleştirme hesabındaki oturumu güncelleştirir veya oluşturur. |
> | Eylem | Microsoft.Logic/integrationAccounts/write | Tümleştirme hesabını güncelleştirir veya oluşturur. |
> | Eylem | Microsoft.Logic/integrationServiceEnvironments/delete | Tümleştirme hizmeti ortamı siler. |
> | Eylem | Microsoft.Logic/integrationServiceEnvironments/join/action | Tümleştirme hizmeti ortama katılır. |
> | Eylem | Microsoft.Logic/integrationServiceEnvironments/managedApis/apiOperations/read | API işlemi tümleştirme hizmeti ortamı yönetilen okur. |
> | Eylem | Microsoft.Logic/integrationServiceEnvironments/managedApis/read | API tümleştirme hizmeti ortamı yönetilen okur. |
> | Eylem | Microsoft.Logic/integrationServiceEnvironments/operationStatuses/read | Tümleştirme hizmeti ortam işlem durumlarını okur. |
> | Eylem | Microsoft.Logic/integrationServiceEnvironments/providers/Microsoft.Insights/metricDefinitions/read | Tümleştirme hizmeti ortamı ölçüm tanımlarını okur. |
> | Eylem | Microsoft.Logic/integrationServiceEnvironments/read | Tümleştirme hizmeti ortamı okur. |
> | Eylem | Microsoft.Logic/integrationServiceEnvironments/write | Oluşturur veya tümleştirme hizmeti ortamı güncelleştirir. |
> | Eylem | Microsoft.Logic/locations/workflows/recommendOperationGroups/action | İş akışı, önerilen işlem grupları alır. |
> | Eylem | Microsoft.Logic/locations/workflows/validate/action | İş akışını doğrular. |
> | Eylem | Microsoft.Logic/operations/read | İşlemi alır. |
> | Eylem | Microsoft.Logic/register/action | Belirli bir abonelik için Microsoft.Logic kaynak sağlayıcısını kaydeder. |
> | Eylem | Microsoft.Logic/workflows/accessKeys/delete | Erişim anahtarını siler. |
> | Eylem | Microsoft.Logic/workflows/accessKeys/list/action | Erişim anahtarı parolalarını listeler. |
> | Eylem | Microsoft.Logic/workflows/accessKeys/read | Erişim anahtarını okur. |
> | Eylem | Microsoft.Logic/workflows/accessKeys/regenerate/action | Erişim anahtarı parolalarını yeniden oluşturur. |
> | Eylem | Microsoft.Logic/workflows/accessKeys/write | Erişim anahtarını güncelleştirir veya oluşturur. |
> | Eylem | Microsoft.Logic/workflows/delete | İş akışı siler. |
> | Eylem | Microsoft.Logic/workflows/disable/action | İş akışını devre dışı bırakır. |
> | Eylem | Microsoft.Logic/workflows/enable/action | İş akışını etkinleştirir. |
> | Eylem | Microsoft.Logic/workflows/listCallbackUrl/action | İş akışı için geri çağırma URL'sini alır. |
> | Eylem | Microsoft.Logic/workflows/listSwagger/action | İş akışı swagger tanımlarını alır. |
> | Eylem | Microsoft.Logic/workflows/move/action | İş akışı bir farklı bir abonelik kimliği, kaynak grubu ve/veya adı için mevcut abonelik kimliği, kaynak grubu ve ad taşır. |
> | Eylem | Microsoft.Logic/workflows/providers/Microsoft.Insights/diagnosticSettings/read | İş akışı tanılama ayarlarını okur. |
> | Eylem | Microsoft.Logic/workflows/providers/Microsoft.Insights/diagnosticSettings/write | Oluşturur veya iş akışı tanılama ayarını güncelleştirir. |
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
> | Eylem | Microsoft.Logic/workflows/runs/delete | İş akışının çalıştırılmasını siler. |
> | Eylem | Microsoft.Logic/workflows/runs/operations/read | İş akışı çalıştırma işlemi durumunu okur. |
> | Eylem | Microsoft.Logic/workflows/runs/read | İş akışı çalıştırmasını okur. |
> | Eylem | Microsoft.Logic/workflows/suspend/action | İş akışını askıya alır. |
> | Eylem | Microsoft.Logic/workflows/triggers/histories/read | Tetikleyici geçmişlerini okur. |
> | Eylem | Microsoft.Logic/workflows/triggers/histories/resubmit/action | İş akışı tetikleyicisini yeniden gönderir. |
> | Eylem | Microsoft.Logic/workflows/triggers/listCallbackUrl/action | Tetikleyici için geri çağırma URL'sini alır. |
> | Eylem | Microsoft.Logic/workflows/triggers/read | Tetikleyiciyi okur. |
> | Eylem | Microsoft.Logic/workflows/triggers/reset/action | Tetikleyiciyi sıfırlar. |
> | Eylem | Microsoft.Logic/workflows/triggers/run/action | Tetikleyiciyi yürütür. |
> | Eylem | Microsoft.Logic/workflows/triggers/setState/action | Tetikleyici durumunu ayarlar. |
> | Eylem | Microsoft.Logic/workflows/validate/action | İş akışını doğrular. |
> | Eylem | Microsoft.Logic/workflows/versions/read | İş akışı sürümünü okur. |
> | Eylem | Microsoft.Logic/workflows/versions/triggers/listCallbackUrl/action | Tetikleyici için geri çağırma URL'sini alır. |
> | Eylem | Microsoft.Logic/workflows/write | Oluşturur veya iş akışı güncelleştirir. |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.MachineLearning/commitmentPlans/commitmentAssociations/move/action | Taahhüt Plan ilişkilendirme öğrenme herhangi bir makineye taşıyın |
> | Eylem | Microsoft.MachineLearning/commitmentPlans/commitmentAssociations/read | Herhangi bir makine öğrenme taahhüt Plan ilişkilendirme okuyun |
> | Eylem | Microsoft.MachineLearning/commitmentPlans/delete | Herhangi bir makine öğrenme taahhüt Planı Sil |
> | Eylem | Microsoft.MachineLearning/commitmentPlans/join/action | Herhangi bir makine öğrenimi Taahhütlü bir plana katılın |
> | Eylem | Microsoft.MachineLearning/commitmentPlans/read | Herhangi bir makine öğrenme taahhüt planı okuma |
> | Eylem | Microsoft.MachineLearning/commitmentPlans/write | Tüm Machine Learning taahhüt planı güncelle |
> | Eylem | Microsoft.MachineLearning/locations/operationresults/read | Bir Machine Learning işleminin sonucunu Al |
> | Eylem | Microsoft.MachineLearning/locations/operationsstatus/read | Devam eden bir Machine Learning işlem durumunu Al |
> | Eylem | Microsoft.MachineLearning/operations/read | Makine öğrenimi işlemleri Al |
> | Eylem | Microsoft.MachineLearning/register/action | Machine learning web hizmeti kaynak sağlayıcısı için aboneliği kaydeder ve web hizmetleri oluşturmanıza olanak tanır. |
> | Eylem | Microsoft.MachineLearning/skus/read | Machine Learning taahhüt Plan SKU'ları Al |
> | Eylem | Microsoft.MachineLearning/webServices/action | Bölgesel Web hizmeti özellikleri için desteklenen bölgeler oluşturun |
> | Eylem | Microsoft.MachineLearning/webServices/delete | Tüm Machine Learning Web hizmetini Sil |
> | Eylem | Microsoft.MachineLearning/webServices/listkeys/read | Bir Machine Learning Web hizmeti için anahtarları alma |
> | Eylem | Microsoft.MachineLearning/webServices/read | Herhangi bir Machine Learning Web hizmeti okuyun |
> | Eylem | Microsoft.MachineLearning/webServices/write | Herhangi bir Machine Learning Web hizmeti güncelle |
> | Eylem | Microsoft.MachineLearning/Workspaces/delete | Machine Learning çalışma alanını Sil |
> | Eylem | Microsoft.MachineLearning/Workspaces/listworkspacekeys/action | Bir Machine Learning çalışma alanı için anahtarları Listele |
> | Eylem | Microsoft.MachineLearning/Workspaces/read | Machine Learning çalışma alanını okuma |
> | Eylem | Microsoft.MachineLearning/Workspaces/resyncstoragekeys/action | Machine Learning çalışma alanı için yapılandırılmış depolama hesabı anahtarlarını yeniden eşitleme |
> | Eylem | Microsoft.MachineLearning/Workspaces/write | Machine Learning çalışma alanını güncelle |

## <a name="microsoftmachinelearningcompute"></a>Microsoft.MachineLearningCompute

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.MachineLearningCompute/operationalizationClusters/checkUpdate/action | Güncelleştirmeleri sistem hizmetlerini kullanıma hazır hale getirme kümesi için kullanılabilir olup olmadığını denetleyin |
> | Eylem | Microsoft.MachineLearningCompute/operationalizationClusters/delete | Herhangi bir barındırma hesabı Sil |
> | Eylem | Microsoft.MachineLearningCompute/operationalizationClusters/listKeys/action | Kullanıma hazır hale getirme kümeyle ilişkili anahtarları listeleme |
> | Eylem | Microsoft.MachineLearningCompute/operationalizationClusters/read | Herhangi bir barındırma hesabı okuma |
> | Eylem | Microsoft.MachineLearningCompute/operationalizationClusters/updateSystem/action | Güncelleştirme kullanıma hazır hale getirme kümesinde sistem hizmetleri |
> | Eylem | Microsoft.MachineLearningCompute/operationalizationClusters/write | Herhangi bir barındırma hesabı güncelle |
> | Eylem | Microsoft.MachineLearningCompute/register/action | Abonelik kimliği kaynak sağlayıcısı için kaydeder ve bir machine learning işlem kaynaklarının oluşturulmasını sağlar |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.MachineLearningServices/locations/computeoperationsstatus/read | Belirli bir işlem işlemin durumunu alır |
> | Eylem | Microsoft.MachineLearningServices/locations/usages/read | Kullanım Raporu aml için bilgi işlem, bir Abonelikteki kaynakları |
> | Eylem | Microsoft.MachineLearningServices/locations/vmsizes/read | Desteklenen vm boyutlarını alma |
> | Eylem | Microsoft.MachineLearningServices/locations/workspaceOperationsStatus/read | Belirli bir çalışma işleminin durumunu alır |
> | Eylem | Microsoft.MachineLearningServices/register/action | Machine Learning Services kaynak sağlayıcısı için aboneliği kaydeder |
> | Eylem | Microsoft.MachineLearningServices/workspaces/computes/delete | Bilgi işlem kaynaklarının Machine Learning Hizmetleri çalışma alanlarını siler |
> | Eylem | Microsoft.MachineLearningServices/workspaces/computes/listKeys/action | Machine Learning Services çalışma alanı işlem kaynakları için gizli dizilerini listeleme |
> | Eylem | Microsoft.MachineLearningServices/workspaces/computes/listNodes/action | İşlem kaynağı Machine Learning Services çalışma alanı için düğümleri listeleyin |
> | Eylem | Microsoft.MachineLearningServices/workspaces/computes/read | Machine Learning Hizmetleri çalışma Alanlarınızda işlem kaynaklarını alır |
> | Eylem | Microsoft.MachineLearningServices/workspaces/computes/write | Oluşturur veya bilgi işlem kaynaklarının Machine Learning Hizmetleri çalışma alanlarını güncelleştirir |
> | Eylem | Microsoft.MachineLearningServices/workspaces/delete | Machine Learning Hizmetleri çalışma alanlarını siler |
> | DataAction | Microsoft.MachineLearningServices/workspaces/experiments/write | Oluşturur veya güncelleştirir Machine Learning Hizmetleri çalışma alanlarını, denemeleri |
> | Eylem | Microsoft.MachineLearningServices/workspaces/listKeys/action | Bir Machine Learning Services çalışma alanı için gizli anahtarları listeleme |
> | Eylem | Microsoft.MachineLearningServices/workspaces/read | Machine Learning Hizmetleri çalışma alanlarını alır. |
> | Eylem | Microsoft.MachineLearningServices/workspaces/write | Oluşturur veya bir Machine Learning Hizmetleri çalışma alanlarını güncelleştirir |

## <a name="microsoftmanagedidentity"></a>Microsoft.managedıdentity

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.ManagedIdentity/register/action | Yönetilen kimlik kaynak sağlayıcısı için aboneliği kaydeder |
> | Eylem | Microsoft.ManagedIdentity/userAssignedIdentities/assign/action | Mevcut bir kullanıcının atamak için RBAC eylem kimlik bir kaynağa atanmış. |
> | Eylem | Microsoft.ManagedIdentity/userAssignedIdentities/delete | Atanan kimliği mevcut bir kullanıcıyı siler |
> | Eylem | Microsoft.ManagedIdentity/userAssignedIdentities/read | Bir mevcut kullanıcı tarafından atanan kimliği alır |
> | Eylem | Microsoft.ManagedIdentity/userAssignedIdentities/write | Atanan kimliği yeni bir kullanıcı oluşturur veya mevcut bir kullanıcının atanan kimliği ile ilişkili etiketleri güncelleştirir |

## <a name="microsoftmanagement"></a>Microsoft.Management

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Management/checkNameAvailability/action | Belirtilen yönetim grubu adı geçerli ve benzersiz olup olmadığını denetler. |
> | Eylem | Microsoft.Management/getEntities/action | Kimliği doğrulanmış kullanıcı için tüm varlıklar (Yönetim gruplarını, Abonelikleri, vb.) listeler. |
> | Eylem | Microsoft.Management/managementGroups/delete | Yönetim grubunu silin. |
> | Eylem | Microsoft.Management/managementGroups/read | Kimliği doğrulanmış kullanıcı için Yönetim grupları listesi. |
> | Eylem | Microsoft.Management/managementGroups/subscriptions/delete | Abonelik yönetim grubundan serbest ilişkilendirir. |
> | Eylem | Microsoft.Management/managementGroups/subscriptions/write | Mevcut yönetim grubuyla abonelik ilişkilendirir. |
> | Eylem | Microsoft.Management/managementGroups/write | Veya bir yönetim grubu güncelleştirilemiyor. |
> | Eylem | Microsoft.Management/register/action | Belirtilen abonelik Microsoft.Management ile kaydetme |

## <a name="microsoftmaps"></a>Microsoft.Maps

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | DataAction | Microsoft.Maps/accounts/data/read | Haritalar hesabı veri okuma erişimi verir. |
> | Eylem | Microsoft.Maps/accounts/delete | Haritalar hesabı silin. |
> | Eylem | Microsoft.Maps/accounts/eventGridFilters/delete | Bir olay Kılavuzu filtresini Sil |
> | Eylem | Microsoft.Maps/accounts/eventGridFilters/read | Bir olay Kılavuzu filtresini Al |
> | Eylem | Microsoft.Maps/accounts/eventGridFilters/write | Bir olay Kılavuzu filtresini güncelle |
> | Eylem | Microsoft.Maps/accounts/listKeys/action | Haritalar hesabı anahtarlarını Listele |
> | Eylem | Microsoft.Maps/accounts/read | Bir Maps hesabı edinin. |
> | Eylem | Microsoft.Maps/accounts/regenerateKey/action | Yeni eşlemeler hesap birincil veya ikincil anahtarı oluştur |
> | Eylem | Microsoft.Maps/accounts/write | Veya haritalar hesabı güncelleştirilemiyor. |
> | Eylem | Microsoft.Maps/operations/read | Sağlayıcı işlemleri oku |
> | Eylem | Microsoft.Maps/register/action | Sağlayıcısını Kaydet |

## <a name="microsoftmarketplace"></a>Microsoft.Marketplace

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Marketplace/offerTypes/publishers/offers/plans/agreements/read | Bir anlaşma döndürür. |
> | Eylem | Microsoft.Marketplace/offerTypes/publishers/offers/plans/agreements/write | İmzalı sözleşmesini kabul eder. |
> | Eylem | Microsoft.Marketplace/offerTypes/publishers/offers/plans/configs/importImage/action | Görüntü, son kullanıcının ACR'ye içeri aktarır. |
> | Eylem | Microsoft.Marketplace/offerTypes/publishers/offers/plans/configs/read | Bir yapılandırma döndürür. |
> | Eylem | Microsoft.Marketplace/offerTypes/publishers/offers/plans/configs/write | Bir yapılandırma kaydeder. |
> | Eylem | Microsoft.Marketplace/register/action | Abonelikte Microsoft.Marketplace kaynak sağlayıcısını kaydeder. |

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.MarketplaceApps/ClassicDevServices/delete | Klasik geliştirme hizmeti kaynak üzerinde bir silme işlemi yapar. |
> | Eylem | Microsoft.MarketplaceApps/ClassicDevServices/listSecrets/action | Klasik geliştirme hizmeti kaynak yönetimi anahtarları alır. |
> | Eylem | Microsoft.MarketplaceApps/ClassicDevServices/listSingleSignOnToken/action | Klasik geliştirme hizmeti için tek oturum üzerinde URL'sini alır. |
> | Eylem | Microsoft.MarketplaceApps/ClassicDevServices/read | Klasik geliştirme hizmeti üzerinde bir alma işlemi yapar. |
> | Eylem | Microsoft.MarketplaceApps/ClassicDevServices/regenerateKey/action | Klasik geliştirme hizmeti kaynak yönetimi anahtarları oluşturur. |
> | Eylem | Microsoft.MarketplaceApps/Operations/read | Okuma işlemleri için tüm kaynak türleri. |

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceOrdering

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.MarketplaceOrdering/agreements/offers/plans/cancel/action | Bir anlaşma belirli Market öğesi için İptal Et |
> | Eylem | Microsoft.MarketplaceOrdering/agreements/offers/plans/read | Belirli bir Market öğesi için bir anlaşma döndürür |
> | Eylem | Microsoft.MarketplaceOrdering/agreements/offers/plans/sign/action | Bir anlaşma belirli Market öğesi için oturum açın |
> | Eylem | Microsoft.MarketplaceOrdering/agreements/read | Tüm sözleşmeler kapsamındaki dönüş belirtilen aboneliği |
> | Eylem | Microsoft.MarketplaceOrdering/offertypes/publishers/offers/plans/agreements/read | Sanal makine belirli Market öğesi için bir anlaşma edinin |
> | Eylem | Microsoft.MarketplaceOrdering/offertypes/publishers/offers/plans/agreements/write | Oturum veya sanal makine belirli Market öğesi için bir sözleşmeyi iptal et |
> | Eylem | Microsoft.MarketplaceOrdering/operations/read | API içinde tüm olası işlemler listesi |

## <a name="microsoftmedia"></a>Microsoft.Media

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Media/checknameavailability/action | Media Services hesabı adı kullanılabilir olup olmadığını denetler |
> | Eylem | Microsoft.Media/locations/checkNameAvailability/action | Media Services hesabı adı kullanılabilir olup olmadığını denetler |
> | Eylem | Microsoft.Media/mediaservices/accountfilters/delete | Herhangi bir hesabı filtresini Sil |
> | Eylem | Microsoft.Media/mediaservices/accountfilters/read | Herhangi bir hesabı filtre okuyun |
> | Eylem | Microsoft.Media/mediaservices/accountfilters/write | Herhangi bir hesabı filtre güncelle |
> | Eylem | Microsoft.Media/mediaservices/assets/assetfilters/delete | Herhangi bir varlık filtresini Sil |
> | Eylem | Microsoft.Media/mediaservices/assets/assetfilters/read | Herhangi bir varlık filtre okuyun |
> | Eylem | Microsoft.Media/mediaservices/assets/assetfilters/write | Herhangi bir varlık filtre güncelle |
> | Eylem | Microsoft.Media/mediaservices/assets/delete | Herhangi bir varlığı silme |
> | Eylem | Microsoft.Media/mediaservices/assets/getEncryptionKey/action | Varlık şifreleme anahtarı alma |
> | Eylem | Microsoft.Media/mediaservices/assets/listContainerSas/action | Liste varlık kapsayıcısı SAS URL'lerini |
> | Eylem | Microsoft.Media/mediaservices/assets/listStreamingLocators/action | Varlık için akış bulucuları listeler |
> | Eylem | Microsoft.Media/mediaservices/assets/read | Herhangi bir varlığı okuma |
> | Eylem | Microsoft.Media/mediaservices/assets/write | Herhangi bir varlığı oluşturma veya güncelleştirme |
> | Eylem | Microsoft.Media/mediaservices/contentKeyPolicies/delete | Herhangi bir içerik anahtarı ilke Sil |
> | Eylem | Microsoft.Media/mediaservices/contentKeyPolicies/getPolicyPropertiesWithSecrets/action | Gizli ilke özelliklerini alma |
> | Eylem | Microsoft.Media/mediaservices/contentKeyPolicies/read | Herhangi bir içerik anahtarı ilke okuyun |
> | Eylem | Microsoft.Media/mediaservices/contentKeyPolicies/write | Herhangi bir içerik anahtarı ilke güncelle |
> | Eylem | Microsoft.Media/mediaservices/delete | Herhangi bir Media Services hesabına Sil |
> | Eylem | Microsoft.Media/mediaservices/eventGridFilters/delete | Herhangi bir olay Kılavuzu filtresini Sil |
> | Eylem | Microsoft.Media/mediaservices/eventGridFilters/read | Herhangi bir olay Kılavuzu filtresini okuyun |
> | Eylem | Microsoft.Media/mediaservices/eventGridFilters/write | Herhangi bir olay Kılavuzu filtresini güncelle |
> | Eylem | Microsoft.Media/mediaservices/liveEventOperations/read | Canlı etkinlik herhangi bir işlemi okuyun |
> | Eylem | Microsoft.Media/mediaservices/liveEvents/delete | Herhangi bir canlı olay Sil |
> | Eylem | Microsoft.Media/mediaservices/liveEvents/liveOutputs/delete | Herhangi bir canlı çıktı Sil |
> | Eylem | Microsoft.Media/mediaservices/liveEvents/liveOutputs/read | Herhangi bir canlı çıktı okuyun |
> | Eylem | Microsoft.Media/mediaservices/liveEvents/liveOutputs/write | Herhangi bir canlı çıktı güncelle |
> | Eylem | Microsoft.Media/mediaservices/liveEvents/read | Herhangi bir canlı etkinlik okuyun |
> | Eylem | Microsoft.Media/mediaservices/liveEvents/reset/action | Herhangi bir canlı etkinlik işlem Sıfırla |
> | Eylem | Microsoft.Media/mediaservices/liveEvents/start/action | Herhangi bir canlı etkinlik işlemi Başlat |
> | Eylem | Microsoft.Media/mediaservices/liveEvents/stop/action | Herhangi bir canlı etkinlik işlemi durdur |
> | Eylem | Microsoft.Media/mediaservices/liveEvents/write | Herhangi bir canlı olay güncelle |
> | Eylem | Microsoft.Media/mediaservices/liveOutputOperations/read | Canlı çıkış herhangi bir işlemi okuyun |
> | Eylem | Microsoft.Media/mediaservices/read | Herhangi bir Media Services hesabına Okuma |
> | Eylem | Microsoft.Media/mediaservices/streamingEndpointOperations/read | Herhangi bir akış uç noktası işlemi okuyun |
> | Eylem | Microsoft.Media/mediaservices/streamingEndpoints/delete | Herhangi bir akış uç noktasını sil |
> | Eylem | Microsoft.Media/mediaservices/streamingEndpoints/read | Herhangi bir akış uç noktası okuyun |
> | Eylem | Microsoft.Media/mediaservices/streamingEndpoints/scale/action | Herhangi bir akış uç noktası işlemi ölçeklendirme |
> | Eylem | Microsoft.Media/mediaservices/streamingEndpoints/start/action | Herhangi bir akış uç noktası işlemi Başlat |
> | Eylem | Microsoft.Media/mediaservices/streamingEndpoints/stop/action | Herhangi bir akış uç noktası işlemi durdur |
> | Eylem | Microsoft.Media/mediaservices/streamingEndpoints/write | Herhangi bir akış uç noktası güncelle |
> | Eylem | Microsoft.Media/mediaservices/streamingLocators/delete | Herhangi bir akış Bulucuyu silin |
> | Eylem | Microsoft.Media/mediaservices/streamingLocators/listContentKeys/action | İçerik anahtarlarını Listele |
> | Eylem | Microsoft.Media/mediaservices/streamingLocators/listPaths/action | Liste yolları |
> | Eylem | Microsoft.Media/mediaservices/streamingLocators/read | Herhangi bir akış Bulucusu okuyun |
> | Eylem | Microsoft.Media/mediaservices/streamingLocators/write | Herhangi bir akış Bulucusu güncelle |
> | Eylem | Microsoft.Media/mediaservices/streamingPolicies/delete | Herhangi bir akış ilkesini silme |
> | Eylem | Microsoft.Media/mediaservices/streamingPolicies/read | Herhangi bir akış ilke okuyun |
> | Eylem | Microsoft.Media/mediaservices/streamingPolicies/write | Herhangi bir akış ilke güncelle |
> | Eylem | Microsoft.Media/mediaservices/syncStorageKeys/action | Bağlı Azure depolama hesabı için depolama anahtarları Eşitle |
> | Eylem | Microsoft.Media/mediaservices/transforms/delete | Herhangi bir dönüştürme Sil |
> | Eylem | Microsoft.Media/mediaservices/transforms/jobs/cancelJob/action | İşi iptal et |
> | Eylem | Microsoft.Media/mediaservices/transforms/jobs/delete | Herhangi bir işi Sil |
> | Eylem | Microsoft.Media/mediaservices/transforms/jobs/read | Herhangi bir işi okuma |
> | Eylem | Microsoft.Media/mediaservices/transforms/jobs/write | Herhangi bir işi güncelle |
> | Eylem | Microsoft.Media/mediaservices/transforms/read | Herhangi bir dönüştürme okuyun |
> | Eylem | Microsoft.Media/mediaservices/transforms/write | Herhangi bir dönüştürme güncelle |
> | Eylem | Microsoft.Media/mediaservices/write | Herhangi bir Media Services hesabına güncelle |
> | Eylem | Microsoft.Media/operations/read | Kullanılabilir işlemleri Al |
> | Eylem | Microsoft.Media/register/action | Media Services kaynak sağlayıcısı için aboneliği kaydeder ve Media Services hesapları oluşturmanıza olanak tanır |
> | Eylem | Microsoft.Media/unregister/action | Media Services kaynak sağlayıcısı için aboneliği kaydını siler |

## <a name="microsoftmigrate"></a>Microsoft.Migrate

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Migrate/assessmentprojects/assessments/read | Bir proje içindeki listelerini değerlendirmeleri |
> | Eylem | Microsoft.Migrate/assessmentprojects/delete | Değerlendirme projesi siler |
> | Eylem | Microsoft.Migrate/assessmentprojects/groups/assessments/assessedmachines/read | Değerlendirilen bir makineyi özelliklerini alma |
> | Eylem | Microsoft.Migrate/assessmentprojects/groups/assessments/delete | Değerlendirme siler |
> | Eylem | Microsoft.Migrate/assessmentprojects/groups/assessments/downloadurl/action | Bir değerlendirme raporun URL'si indirir |
> | Eylem | Microsoft.Migrate/assessmentprojects/groups/assessments/read | Değerlendirme özelliklerini alır |
> | Eylem | Microsoft.Migrate/assessmentprojects/groups/assessments/write | Yeni değerlendirme oluşturur veya mevcut bir değerlendirme güncelleştirir |
> | Eylem | Microsoft.Migrate/assessmentprojects/groups/delete | Bir grubu siler |
> | Eylem | Microsoft.Migrate/assessmentprojects/groups/read | Bir grubu özelliklerini alma |
> | Eylem | Microsoft.Migrate/assessmentprojects/groups/updateMachines/action | Güncelleştirme grubu ekleyerek veya kaldırarak makineler |
> | Eylem | Microsoft.Migrate/assessmentprojects/groups/write | Yeni bir grup oluşturur veya mevcut bir grubu güncelleştirir |
> | Eylem | Microsoft.Migrate/assessmentprojects/hypervcollectors/delete | HyperV Toplayıcı siler |
> | Eylem | Microsoft.Migrate/assessmentprojects/hypervcollectors/read | HyperV Toplayıcı özelliklerini alır |
> | Eylem | Microsoft.Migrate/assessmentprojects/hypervcollectors/write | Yeni bir HyperV Toplayıcı oluşturur veya mevcut bir HyperV Toplayıcı güncelleştirir |
> | Eylem | Microsoft.Migrate/assessmentprojects/machines/read | Bir makinenin özelliklerini alır |
> | Eylem | Microsoft.Migrate/assessmentprojects/read | Değerlendirme proje özelliklerini alır |
> | Eylem | Microsoft.Migrate/assessmentprojects/vmwarecollectors/delete | VMware Toplayıcı siler |
> | Eylem | Microsoft.Migrate/assessmentprojects/vmwarecollectors/read | VMware Toplayıcı özelliklerini alır |
> | Eylem | Microsoft.Migrate/assessmentprojects/vmwarecollectors/write | Yeni bir VMware Toplayıcı oluşturur veya var olan bir VMware Toplayıcı güncelleştirir |
> | Eylem | Microsoft.Migrate/assessmentprojects/write | Yeni değerlendirme projesi oluşturur veya mevcut bir değerlendirme projesi güncelleştirir |
> | Eylem | Microsoft.Migrate/locations/assessmentOptions/read | Verilen konumda kullanılabilen değerlendirme seçeneklerini alır |
> | Eylem | Microsoft.Migrate/locations/checknameavailability/action | Belirtilen konumdaki belirtilen abonelik için kaynak adının kullanılabilirliğini denetler |
> | Eylem | Microsoft.Migrate/migrateprojects/DatabaseInstances/read | Bir veritabanı örneği özelliklerini alır |
> | Eylem | Microsoft.Migrate/migrateprojects/Databases/read | Bir veritabanının özelliklerini alır |
> | Eylem | Microsoft.Migrate/migrateprojects/delete | Bir geçiş projesi siler |
> | Eylem | Microsoft.Migrate/migrateprojects/machines/read | Bir makinenin özelliklerini alır |
> | Eylem | Microsoft.Migrate/migrateprojects/MigrateEvents/Delete | Bir geçiş olayı siler |
> | Eylem | Microsoft.Migrate/migrateprojects/MigrateEvents/read | Bir geçiş özellikleri, olayları alır. |
> | Eylem | Microsoft.Migrate/migrateprojects/read | Geçiş proje özelliklerini alır |
> | Eylem | Microsoft.Migrate/migrateprojects/registerTool/action | Aracı bir geçiş projesine kaydeder |
> | Eylem | Microsoft.Migrate/migrateprojects/solutions/cleanupData/action | Geçiş proje çözüm verilerini temizle |
> | Eylem | Microsoft.Migrate/migrateprojects/solutions/Delete | Geçiş proje çözüm siler |
> | Eylem | Microsoft.Migrate/migrateprojects/solutions/getconfig/action | Geçiş proje çözüm yapılandırması alır |
> | Eylem | Microsoft.Migrate/migrateprojects/solutions/read | Geçiş proje çözüm özelliklerini alır |
> | Eylem | Microsoft.Migrate/migrateprojects/solutions/write | Yeni geçiş projesi çözümü oluşturur veya var olan bir geçiş proje çözümde güncelleştirir |
> | Eylem | Microsoft.Migrate/migrateprojects/write | Yeni bir geçiş projesi oluşturur veya var olan bir geçiş projesi güncelleştirir |
> | Eylem | Microsoft.Migrate/Operations/read | Microsoft.Migrate kaynak sağlayıcısındaki kullanılabilir işlemleri listele |
> | Eylem | Microsoft.Migrate/projects/assessments/read | Bir proje içindeki listelerini değerlendirmeleri |
> | Eylem | Microsoft.Migrate/projects/delete | Projeyi siler |
> | Eylem | Microsoft.Migrate/projects/groups/assessments/assessedmachines/read | Değerlendirilen bir makineyi özelliklerini alma |
> | Eylem | Microsoft.Migrate/projects/groups/assessments/delete | Değerlendirme siler |
> | Eylem | Microsoft.Migrate/projects/groups/assessments/downloadurl/action | Bir değerlendirme raporun URL'si indirir |
> | Eylem | Microsoft.Migrate/projects/groups/assessments/read | Değerlendirme özelliklerini alır |
> | Eylem | Microsoft.Migrate/projects/groups/assessments/write | Yeni değerlendirme oluşturur veya mevcut bir değerlendirme güncelleştirir |
> | Eylem | Microsoft.Migrate/projects/groups/delete | Bir grubu siler |
> | Eylem | Microsoft.Migrate/projects/groups/read | Bir grubu özelliklerini alma |
> | Eylem | Microsoft.Migrate/projects/groups/write | Yeni bir grup oluşturur veya mevcut bir grubu güncelleştirir |
> | Eylem | Microsoft.Migrate/projects/keys/action | Paylaşılan proje için anahtarlarını alır |
> | Eylem | Microsoft.Migrate/projects/machines/read | Bir makinenin özelliklerini alır |
> | Eylem | Microsoft.Migrate/projects/read | Bir proje özelliklerini alır |
> | Eylem | Microsoft.Migrate/projects/write | Yeni bir proje oluşturur veya mevcut bir projeyi güncelleştirir |
> | Eylem | Microsoft.Migrate/register/action | Aboneliği Microsoft.Migrate kaynak sağlayıcısına kaydeder |

## <a name="microsoftmixedreality"></a>Microsoft.MixedReality

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.MixedReality/register/action | Karma gerçeklik kaynak sağlayıcısı için aboneliği kaydeder. |
> | DataAction | Microsoft.MixedReality/SpatialAnchorsAccounts/create/action | Uzamsal yer işaretleri oluşturmanız |
> | DataAction | Microsoft.MixedReality/SpatialAnchorsAccounts/delete | Uzamsal çıpalarını silme |
> | DataAction | Microsoft.MixedReality/SpatialAnchorsAccounts/discovery/read | Uzamsal bağlayıcılarını bulma |
> | DataAction | Microsoft.MixedReality/SpatialAnchorsAccounts/properties/read | Uzamsal yer işaretleri özelliklerini alma |
> | Eylem | Microsoft.MixedReality/SpatialAnchorsAccounts/providers/Microsoft.Insights/diagnosticSettings/read | Microsoft.MixedReality/SpatialAnchorsAccounts tanılama ayarını alır |
> | Eylem | Microsoft.MixedReality/SpatialAnchorsAccounts/providers/Microsoft.Insights/diagnosticSettings/write | Microsoft.MixedReality/SpatialAnchorsAccounts için tanılama ayarını oluşturur veya güncelleştirir |
> | Eylem | Microsoft.MixedReality/SpatialAnchorsAccounts/providers/Microsoft.Insights/metricDefinitions/read | Microsoft.MixedReality/SpatialAnchorsAccounts için kullanılabilir ölçümleri alır |
> | DataAction | Microsoft.MixedReality/SpatialAnchorsAccounts/query/read | Uzamsal bağlayıcılarını bulun |
> | DataAction | Microsoft.MixedReality/SpatialAnchorsAccounts/submitdiag/read | Azure uzamsal bağlayıcılarını hizmet kalitesini geliştirmeye yardımcı olmak için Tanılama verileri gönder |
> | DataAction | Microsoft.MixedReality/SpatialAnchorsAccounts/write | Uzamsal yer işaretleri özelliklerini güncelleştir |

## <a name="microsoftnetapp"></a>Microsoft.NetApp

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.NetApp/locations/operationresults/read | Bir işlemin sonucu kaynağa okur. |
> | Eylem | Microsoft.NetApp/locations/read | Kullanılabilirlik bir okuma kaynak kontrol edin. |
> | Eylem | Microsoft.NetApp/netAppAccounts/capacityPools/delete | Kaynak havuzuna siler. |
> | Eylem | Microsoft.NetApp/netAppAccounts/capacityPools/read | Kaynak havuzuna okur. |
> | Eylem | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/delete | Bir birim kaynağı siler. |
> | Eylem | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/mountTargets/read | Bir bağlama hedef kaynak okur. |
> | Eylem | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/read | Birim kaynağına okur. |
> | Eylem | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/snapshots/delete | Bir anlık görüntü kaynağı siler. |
> | Eylem | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/snapshots/read | Anlık görüntü kaynak okur. |
> | Eylem | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/snapshots/write | Anlık görüntü kaynak yazar. |
> | Eylem | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/write | Birim kaynağına yazar. |
> | Eylem | Microsoft.NetApp/netAppAccounts/capacityPools/write | Kaynak havuzuna yazar. |
> | Eylem | Microsoft.NetApp/netAppAccounts/delete | Bir hesabı kaynağı siler. |
> | Eylem | Microsoft.NetApp/netAppAccounts/read | Bir hesabı kaynağı okur. |
> | Eylem | Microsoft.NetApp/netAppAccounts/write | Bir hesabı kaynağı yazar. |
> | Eylem | Microsoft.NetApp/Operations/read | Bir işlem kaynaklarını okur. |

## <a name="microsoftnetwork"></a>Microsoft.Network

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Network/applicationGatewayAvailableRequestHeaders/read | Uygulama ağ geçidi kullanılabilir istek üstbilgileri Al |
> | Eylem | Microsoft.Network/applicationGatewayAvailableResponseHeaders/read | Uygulama ağ geçidi kullanılabilir yanıt üst bilgisi alma |
> | Eylem | Microsoft.Network/applicationGatewayAvailableServerVariables/read | Uygulama ağ geçidi kullanılabilir sunucu değişkenleri Al |
> | Eylem | Microsoft.Network/applicationGatewayAvailableSslOptions/predefinedPolicies/read | Application Gateway Ssl İlkesi önceden tanımlanmış |
> | Eylem | Microsoft.Network/applicationGatewayAvailableSslOptions/read | Uygulama ağ geçidi kullanılabilir Ssl seçenekleri |
> | Eylem | Microsoft.Network/applicationGatewayAvailableWafRuleSets/read | Uygulama ağ geçidi kullanılabilir Waf kural kümesi alır |
> | Eylem | Microsoft.Network/applicationGateways/backendAddressPools/join/action | Bir uygulama ağ geçidi arka uç adres havuzu birleştirir. Alertable değil. |
> | Eylem | Microsoft.Network/applicationGateways/backendhealth/action | Bir uygulama ağ geçidi arka uç durumunu alır |
> | Eylem | Microsoft.Network/applicationGateways/delete | Bir uygulama ağ geçidini siler |
> | Eylem | Microsoft.Network/applicationGateways/read | Bir uygulama ağ geçidi alır |
> | Eylem | Microsoft.Network/applicationGateways/start/action | Bir uygulama ağ geçidi başlatır |
> | Eylem | Microsoft.Network/applicationGateways/stop/action | Bir uygulama ağ geçidi durdurur |
> | Eylem | Microsoft.Network/applicationGateways/write | Bir uygulama ağ geçidi oluşturur veya bir uygulama ağ geçidi güncelleştirir |
> | Eylem | Microsoft.Network/ApplicationGatewayWebApplicationFirewallPolicies/delete | Bir Application Gateway WAF ilkesini siler |
> | Eylem | Microsoft.Network/ApplicationGatewayWebApplicationFirewallPolicies/read | Bir Application Gateway WAF İlkesi alır |
> | Eylem | Microsoft.Network/ApplicationGatewayWebApplicationFirewallPolicies/write | Bir Application Gateway WAF ilkesi oluşturur veya bir Application Gateway WAF İlkesi güncelleştirir |
> | Eylem | Microsoft.Network/applicationSecurityGroups/delete | Bir uygulama güvenlik grubunu siler |
> | Eylem | Microsoft.Network/applicationSecurityGroups/joinIpConfiguration/action | Uygulama güvenlik grupları için bir IP yapılandırması birleştirir. Alertable değil. |
> | Eylem | Microsoft.Network/applicationSecurityGroups/joinNetworkSecurityRule/action | Uygulama güvenlik grupları için bir güvenlik kuralı birleştirir. Alertable değil. |
> | Eylem | Microsoft.Network/applicationSecurityGroups/listIpConfigurations/action | ApplicationSecurityGroup içindeki listelerini IP yapılandırmaları |
> | Eylem | Microsoft.Network/applicationSecurityGroups/read | Bir uygulama güvenlik grubu kimliği alır. |
> | Eylem | Microsoft.Network/applicationSecurityGroups/write | Uygulama güvenlik grubu oluşturur veya mevcut bir uygulama güvenlik grubuna güncelleştirir. |
> | Eylem | Microsoft.Network/azureFirewallFqdnTags/read | Azure güvenlik duvarı FQDN etiketlerini alır |
> | Eylem | Microsoft.Network/azurefirewalls/delete | Azure güvenlik duvarı Sil |
> | Eylem | Microsoft.Network/azurefirewalls/read | Azure Güvenlik Duvarı'nı edinin |
> | Eylem | Microsoft.Network/azurefirewalls/write | Oluşturur veya bir Azure Güvenlik Duvarı'nı güncelleştirir |
> | Eylem | Microsoft.Network/bastionHosts/delete | Kale ana bilgisayarı siler |
> | Eylem | Microsoft.Network/bastionHosts/read | Kale ana bilgisayarı alır |
> | Eylem | Microsoft.Network/bastionHosts/write | Kale ana bilgisayarı güncelle |
> | Eylem | Microsoft.Network/bgpServiceCommunities/read | BGP hizmet toplulukları Al |
> | Eylem | Microsoft.Network/checkFrontDoorNameAvailability/action | Ön kapı adları kullanılabilir olup olmadığını denetler |
> | Eylem | Microsoft.Network/checkTrafficManagerNameAvailability/action | Bir Traffic Manager göreli DNS adının kullanılabilirliğini denetler. |
> | Eylem | Microsoft.Network/connections/delete | Deletes VirtualNetworkGatewayConnection |
> | Eylem | Microsoft.Network/connections/read | Gets VirtualNetworkGatewayConnection |
> | Eylem | Microsoft.Network/connections/revoke/action | Express Route bağlantısının durumu iptal edildi işaretler. |
> | Eylem | Microsoft.Network/connections/sharedkey/action | VirtualNetworkGatewayConnection SharedKey Al |
> | Eylem | Microsoft.Network/connections/sharedKey/read | Gets VirtualNetworkGatewayConnection SharedKey |
> | Eylem | Microsoft.Network/connections/sharedKey/write | Oluşturur veya mevcut bir VirtualNetworkGatewayConnection SharedKey güncelleştirir |
> | Eylem | Microsoft.Network/connections/vpndeviceconfigurationscript/action | VPN cihaz yapılandırması için VirtualNetworkGatewayConnection alır |
> | Eylem | Microsoft.Network/connections/write | Oluşturur veya mevcut bir VirtualNetworkGatewayConnection güncelleştirir |
> | Eylem | Microsoft.Network/ddosCustomPolicies/delete | İlke silme bir DDoS özelleştirilmiş |
> | Eylem | Microsoft.Network/ddosCustomPolicies/read | İlke tanımı tanımı alır bir DDoS özelleştirilmiş |
> | Eylem | Microsoft.Network/ddosCustomPolicies/write | Bir DDoS özelleştirilmiş ilke oluşturur veya mevcut bir DDoS özelleştirilmiş İlkesi güncelleştirmeleri |
> | Eylem | Microsoft.Network/ddosProtectionPlans/delete | Bir DDoS koruma planı siler |
> | Eylem | Microsoft.Network/ddosProtectionPlans/join/action | Bir DDoS koruma planı birleştirir. Alertable değil. |
> | Eylem | Microsoft.Network/ddosProtectionPlans/read | Bir DDoS koruma planı alır |
> | Eylem | Microsoft.Network/ddosProtectionPlans/write | Bir DDoS koruma planı oluşturur veya bir DDoS koruma planı güncelleştirir  |
> | Eylem | Microsoft.Network/dnsoperationresults/read | Bir DNS işleminin sonuçlarını alır |
> | Eylem | Microsoft.Network/dnsoperationstatuses/read | Bir DNS işlemin durumunu alır  |
> | Eylem | Microsoft.Network/dnszones/A/delete | Belirli bir ada kayıt kümesini kaldırın ve bir DNS bölgesi ' A' yazın. |
> | Eylem | Microsoft.Network/dnszones/A/read | Kayıt kümesi türü 'A', JSON biçiminde alın. Kayıt kümesi kayıtları yanı sıra TTL, etiketler ve etag listesini içerir. |
> | Eylem | Microsoft.Network/dnszones/A/write | Veya bir DNS bölgesi içinde 'A' türünde bir kayıt kümesi güncelleştirilemiyor. Belirtilen kayıt kayıt kümesindeki geçerli kayıt yerini alır. |
> | Eylem | Microsoft.Network/dnszones/AAAA/delete | Belirli bir ada kayıt kümesini kaldırın ve bir DNS bölgesi'nden ' AAAA' yazın. |
> | Eylem | Microsoft.Network/dnszones/AAAA/read | Kayıt kümesi türü 'AAAA' JSON biçiminde alın. Kayıt kümesi kayıtları yanı sıra TTL, etiketler ve etag listesini içerir. |
> | Eylem | Microsoft.Network/dnszones/AAAA/write | Veya bir DNS bölgesi içinde 'AAAA' türündeki kayıt kümesi güncelleştirilemiyor. Belirtilen kayıt kayıt kümesindeki geçerli kayıt yerini alır. |
> | Eylem | Microsoft.Network/dnszones/all/read | DNS kayıt kümeleri türlerinde alır |
> | Eylem | Microsoft.Network/dnszones/CAA/delete | Belirli bir ada kayıt kümesini kaldırın ve bir DNS bölgesi'nden ' CAA' yazın. |
> | Eylem | Microsoft.Network/dnszones/CAA/read | Kayıt kümesi türü 'CAA' JSON biçiminde alın. TTL, etiketler ve etag kayıt kümesi içerir. |
> | Eylem | Microsoft.Network/dnszones/CAA/write | Veya bir DNS bölgesi içinde 'CAA' türündeki kayıt kümesi güncelleştirilemiyor. Belirtilen kayıt kayıt kümesindeki geçerli kayıt yerini alır. |
> | Eylem | Microsoft.Network/dnszones/CNAME/delete | Belirli bir ada kayıt kümesini kaldırın ve bir DNS bölgesi'nden ' CNAME' yazın. |
> | Eylem | Microsoft.Network/dnszones/CNAME/read | Kayıt kümesi türü 'CNAME' JSON biçiminde alın. TTL, etiketler ve etag kayıt kümesi içerir. |
> | Eylem | Microsoft.Network/dnszones/CNAME/write | Veya bir DNS bölgesi içinde 'CNAME' türündeki kayıt kümesi güncelleştirilemiyor. Belirtilen kayıt kayıt kümesindeki geçerli kayıt yerini alır. |
> | Eylem | Microsoft.Network/dnszones/delete | JSON biçiminde bir DNS bölgesi silin. Etiketler, etag, numberOfRecordSets ve maxNumberOfRecordSets bölge özellikleri içerir. |
> | Eylem | Microsoft.Network/dnszones/MX/delete | Belirli bir ada kayıt kümesini kaldırın ve bir DNS bölgesi'nden "MX" yazın. |
> | Eylem | Microsoft.Network/dnszones/MX/read | Kayıt kümesi türü 'MX' JSON biçiminde alın. Kayıt kümesi kayıtları yanı sıra TTL, etiketler ve etag listesini içerir. |
> | Eylem | Microsoft.Network/dnszones/MX/write | Veya bir DNS bölgesi içinde 'MX' türündeki kayıt kümesi güncelleştirilemiyor. Belirtilen kayıt kayıt kümesindeki geçerli kayıt yerini alır. |
> | Eylem | Microsoft.Network/dnszones/NS/delete | DNS kayıt kümesi türü NS siler |
> | Eylem | Microsoft.Network/dnszones/NS/read | DNS türü NS kayıt kümesini alır |
> | Eylem | Microsoft.Network/dnszones/NS/write | Oluşturur veya DNS türü NS kayıt kümesini güncelleştirir |
> | Eylem | Microsoft.Network/dnszones/PTR/delete | Belirli bir ada kayıt kümesini kaldırın ve bir DNS bölgesi'nden ' PTR' yazın. |
> | Eylem | Microsoft.Network/dnszones/PTR/read | Kayıt kümesi türü 'PTR' JSON biçiminde alın. Kayıt kümesi kayıtları yanı sıra TTL, etiketler ve etag listesini içerir. |
> | Eylem | Microsoft.Network/dnszones/PTR/write | Veya bir DNS bölgesi içinde 'PTR' türündeki kayıt kümesi güncelleştirilemiyor. Belirtilen kayıt kayıt kümesindeki geçerli kayıt yerini alır. |
> | Eylem | Microsoft.Network/dnszones/read | DNS bölgesi, JSON biçiminde alın. Etiketler, etag, numberOfRecordSets ve maxNumberOfRecordSets bölge özellikleri içerir. Bu komut bölge içinde bulunan kayıt kümeleri almıyorsa unutmayın. |
> | Eylem | Microsoft.Network/dnszones/recordsets/read | DNS kayıt kümeleri türlerinde alır |
> | Eylem | Microsoft.Network/dnszones/SOA/read | DNS kayıt kümesi türü SOA alır |
> | Eylem | Microsoft.Network/dnszones/SOA/write | Oluşturur veya DNS kayıt kümesi türü SOA güncelleştirir |
> | Eylem | Microsoft.Network/dnszones/SRV/delete | Belirli bir ada kayıt kümesini kaldırın ve bir DNS bölgesi'nden ' SRV' yazın. |
> | Eylem | Microsoft.Network/dnszones/SRV/read | Kayıt kümesi türü 'SRV' JSON biçiminde alın. Kayıt kümesi kayıtları yanı sıra TTL, etiketler ve etag listesini içerir. |
> | Eylem | Microsoft.Network/dnszones/SRV/write | Oluşturma veya güncelleştirme türü SRV kayıt kümesi |
> | Eylem | Microsoft.Network/dnszones/TXT/delete | Belirli bir ada kayıt kümesini kaldırın ve bir DNS bölgesi'nden ' TXT' yazın. |
> | Eylem | Microsoft.Network/dnszones/TXT/read | Kayıt kümesi türü 'TXT', JSON biçiminde alın. Kayıt kümesi kayıtları yanı sıra TTL, etiketler ve etag listesini içerir. |
> | Eylem | Microsoft.Network/dnszones/TXT/write | Veya bir DNS bölgesi içinde 'TXT' türündeki kayıt kümesi güncelleştirilemiyor. Belirtilen kayıt kayıt kümesindeki geçerli kayıt yerini alır. |
> | Eylem | Microsoft.Network/dnszones/write | Veya bir DNS bölgesi bir kaynak grubu içindeki güncelleştirilemiyor.  Bir DNS bölgesi kaynağı üzerinde etiketleri güncelleştirmek için kullanılır. Bu komutu oluşturmak veya bölgede kayıt kümelerini güncelleştirmek için kullanılamaz olduğunu unutmayın. |
> | Eylem | Microsoft.Network/expressRouteCircuits/authorizations/delete | ExpressRouteCircuit yetkilendirme siler |
> | Eylem | Microsoft.Network/expressRouteCircuits/authorizations/read | ExpressRouteCircuit yetkilendirme alır |
> | Eylem | Microsoft.Network/expressRouteCircuits/authorizations/write | Oluşturur veya mevcut bir ExpressRouteCircuit yetkilendirme güncelleştirir |
> | Eylem | Microsoft.Network/expressRouteCircuits/delete | ExpressRouteCircuit siler |
> | Eylem | Microsoft.Network/expressRouteCircuits/join/action | Bir Express Route bağlantı hattı birleştirir. Alertable değil. |
> | Eylem | Microsoft.Network/expressRouteCircuits/peerings/arpTables/action | Eşleme bir ExpressRouteCircuit ArpTable alır |
> | Eylem | Microsoft.Network/expressRouteCircuits/peerings/connections/delete | ExpressRouteCircuit bağlantısını siler |
> | Eylem | Microsoft.Network/expressRouteCircuits/peerings/connections/read | ExpressRouteCircuit bağlantıyı alır |
> | Eylem | Microsoft.Network/expressRouteCircuits/peerings/connections/write | Oluşturur veya var olan bir ExpressRouteCircuit bağlantı kaynağı |
> | Eylem | Microsoft.Network/expressRouteCircuits/peerings/delete | Bir eşleme ExpressRouteCircuit siler |
> | Eylem | Microsoft.Network/expressRouteCircuits/peerings/peerConnections/read | Eş Express Route devre bağlantısı alır |
> | Eylem | Microsoft.Network/expressRouteCircuits/peerings/read | Bir eşleme ExpressRouteCircuit alır |
> | Eylem | Microsoft.Network/expressRouteCircuits/peerings/routeTables/action | Eşleme bir ExpressRouteCircuit RouteTable alır |
> | Eylem | Microsoft.Network/expressRouteCircuits/peerings/routeTablesSummary/action | Bir ExpressRouteCircuit eşleme RouteTable özeti alır |
> | Eylem | Microsoft.Network/expressRouteCircuits/peerings/stats/read | Bir ExpressRouteCircuit Stat eşlemesini alır |
> | Eylem | Microsoft.Network/expressRouteCircuits/peerings/write | Oluşturur veya mevcut bir eşleme ExpressRouteCircuit güncelleştirir |
> | Eylem | Microsoft.Network/expressRouteCircuits/read | ExpressRouteCircuit Al |
> | Eylem | Microsoft.Network/expressRouteCircuits/stats/read | ExpressRouteCircuit Stat alır |
> | Eylem | Microsoft.Network/expressRouteCircuits/write | Oluşturur veya mevcut ExpressRouteCircuit güncelleştirir |
> | Eylem | Microsoft.Network/expressRouteCrossConnections/join/action | Birleştirmeler bir Expressroute bağlantı arası. Alertable değil. |
> | Eylem | Microsoft.Network/expressRouteCrossConnections/peerings/arpTables/action | Bir Express Route çapraz bağlantı eşlemesi Arp tablosu alır |
> | Eylem | Microsoft.Network/expressRouteCrossConnections/peerings/delete | Bir Express Route çapraz bağlantısı eşlemesi siler |
> | Eylem | Microsoft.Network/expressRouteCrossConnections/peerings/read | Bir Express Route çapraz bağlantısı eşlemesi alır |
> | Eylem | Microsoft.Network/expressRouteCrossConnections/peerings/routeTables/action | Bir Express Route çapraz bağlantı eşlemesi yol tablosu alır |
> | Eylem | Microsoft.Network/expressRouteCrossConnections/peerings/routeTableSummary/action | Bir Express Route çapraz bağlantı eşlemesi rota tablosu özeti alır |
> | Eylem | Microsoft.Network/expressRouteCrossConnections/peerings/write | Bir Express Route çapraz bağlantı eşlemesi oluşturur veya bir mevcut Express Route çapraz bağlantı eşlemesi güncelleştirir |
> | Eylem | Microsoft.Network/expressRouteCrossConnections/read | Alma Express Route çapraz bağlantısı |
> | Eylem | Microsoft.Network/expressRouteGateways/expressRouteConnections/delete | Express Route bağlantısının siler |
> | Eylem | Microsoft.Network/expressRouteGateways/expressRouteConnections/read | Express Route bağlantısının alır |
> | Eylem | Microsoft.Network/expressRouteGateways/expressRouteConnections/write | Bir Express Route bağlantı oluşturur veya mevcut bir Express Route bağlantısının güncelleştirir |
> | Eylem | Microsoft.Network/expressRouteGateways/join/action | Bir Expressroute ağ geçidi birleştirir. Alertable değil. |
> | Eylem | Microsoft.Network/expressRouteGateways/read | Express Route ağ geçidi Al |
> | Eylem | Microsoft.Network/expressRoutePorts/delete | ExpressRoutePorts siler |
> | Eylem | Microsoft.Network/expressRoutePorts/join/action | Express Route bağlantı noktaları birleştirir. Alertable değil. |
> | Eylem | Microsoft.Network/expressRoutePorts/links/read | Gets ExpressRouteLink |
> | Eylem | Microsoft.Network/expressRoutePorts/read | Gets ExpressRoutePorts |
> | Eylem | Microsoft.Network/expressRoutePorts/write | Oluşturur veya ExpressRoutePorts güncelleştirir |
> | Eylem | Microsoft.Network/expressRoutePortsLocations/read | Get Express Route Ports Locations |
> | Eylem | Microsoft.Network/expressRouteServiceProviders/read | Express Route hizmet sağlayıcısı alır |
> | Eylem | Microsoft.Network/frontDoors/backendPools/delete | Arka uç havuzu siler |
> | Eylem | Microsoft.Network/frontDoors/backendPools/read | Arka uç havuzu alır |
> | Eylem | Microsoft.Network/frontDoors/backendPools/write | Oluşturur veya güncelleştirir arka uç havuzu |
> | Eylem | Microsoft.Network/frontDoors/delete | Bir ön kapısı siler |
> | Eylem | Microsoft.Network/frontDoors/frontendEndpoints/delete | Bir ön uç uç noktasını siler |
> | Eylem | Microsoft.Network/frontDoors/frontendEndpoints/disableHttps/action | Bir ön uç noktadaki HTTPS devre dışı bırakır |
> | Eylem | Microsoft.Network/frontDoors/frontendEndpoints/enableHttps/action | HTTPS ön uç noktadaki sağlar |
> | Eylem | Microsoft.Network/frontDoors/frontendEndpoints/read | Bir ön uç uç noktası alır |
> | Eylem | Microsoft.Network/frontDoors/frontendEndpoints/write | Oluşturur veya güncelleştirir bir ön uç uç noktası |
> | Eylem | Microsoft.Network/frontDoors/healthProbeSettings/delete | Durum yoklama ayarları siler |
> | Eylem | Microsoft.Network/frontDoors/healthProbeSettings/read | Sistem durumu araştırma ayarlarını alır. |
> | Eylem | Microsoft.Network/frontDoors/healthProbeSettings/write | Oluşturur veya sistem durumu araştırma ayarlarını güncelleştirir |
> | Eylem | Microsoft.Network/frontDoors/loadBalancingSettings/delete | Oluşturur veya yük dengeleme ayarlarını güncelleştirir |
> | Eylem | Microsoft.Network/frontDoors/loadBalancingSettings/read | Yük Dengeleme ayarlarını alır |
> | Eylem | Microsoft.Network/frontDoors/loadBalancingSettings/write | Oluşturur veya yük dengeleme ayarlarını güncelleştirir |
> | Eylem | Microsoft.Network/frontDoors/purge/action | Bir ön kapısı önbelleğe alınan içerik temizleme |
> | Eylem | Microsoft.Network/frontDoors/read | Bir ön kapısı alır |
> | Eylem | Microsoft.Network/frontDoors/routingRules/delete | Bir yönlendirme kuralını siler |
> | Eylem | Microsoft.Network/frontDoors/routingRules/read | Yönlendirme kuralı alır |
> | Eylem | Microsoft.Network/frontDoors/routingRules/write | Oluşturur veya yönlendirme kuralını güncelleştirir |
> | Eylem | Microsoft.Network/frontDoors/validateCustomDomain/action | Bir ön uç uç noktası için ön kapı doğrular. |
> | Eylem | Microsoft.Network/frontDoors/write | Oluşturur veya güncelleştirir ön kapısı |
> | Eylem | Microsoft.Network/frontDoorWebApplicationFirewallManagedRuleSets/read | Alır Web uygulaması güvenlik duvarı kural kümeleri yönetilen |
> | Eylem | Microsoft.Network/frontDoorWebApplicationFirewallPolicies/delete | Bir Web uygulaması güvenlik duvarı ilkesini siler |
> | Eylem | Microsoft.Network/frontDoorWebApplicationFirewallPolicies/read | Bir Web uygulaması güvenlik duvarı ilkesi alır |
> | Eylem | Microsoft.Network/frontDoorWebApplicationFirewallPolicies/write | Oluşturur veya bir Web uygulaması güvenlik duvarı ilkesi güncelleştirir |
> | Eylem | Microsoft.Network/loadBalancers/backendAddressPools/join/action | Bir yük dengeleyici arka uç adres havuzu birleştirir. Alertable değil. |
> | Eylem | Microsoft.Network/loadBalancers/backendAddressPools/read | Bir yük dengeleyici arka uç adres havuzu tanımı alır |
> | Eylem | Microsoft.Network/loadBalancers/delete | Bir yük dengeleyici siler |
> | Eylem | Microsoft.Network/loadBalancers/frontendIPConfigurations/join/action | Bir yük dengeleyici ön uç IP yapılandırması birleştirir. Alertable değil. |
> | Eylem | Microsoft.Network/loadBalancers/frontendIPConfigurations/read | Bir yük dengeleyici ön uç IP yapılandırması tanımı alır |
> | Eylem | Microsoft.Network/loadBalancers/inboundNatPools/join/action | Birleştiren bir yük dengeleyici gelen NAT havuzu. Alertable değil. |
> | Eylem | Microsoft.Network/loadBalancers/inboundNatPools/read | Alır bir yük dengeleyici gelen nat havuzu tanımı |
> | Eylem | Microsoft.Network/loadBalancers/inboundNatRules/delete | Bir yük dengeleyici gelen nat kuralı siler |
> | Eylem | Microsoft.Network/loadBalancers/inboundNatRules/join/action | Bir yük dengeleyici gelen nat kuralı birleştirir. Alertable değil. |
> | Eylem | Microsoft.Network/loadBalancers/inboundNatRules/read | Alır bir yük dengeleyici gelen nat kuralı tanımı |
> | Eylem | Microsoft.Network/loadBalancers/inboundNatRules/write | Bir yük dengeleyici gelen nat kuralı oluşturur veya mevcut bir yük dengeleyici gelen nat kuralı güncelleştirir |
> | Eylem | Microsoft.Network/loadBalancers/loadBalancingRules/read | Bir yük dengeleyici Yük Dengeleme kuralı tanımı alır |
> | Eylem | Microsoft.Network/loadBalancers/networkInterfaces/read | Bir yük dengeleyici altındaki tüm ağ arabirimleri başvurularını alır |
> | Eylem | Microsoft.Network/loadBalancers/outboundRules/read | Bir yük dengeleyici giden kuralı tanımı alır |
> | Eylem | Microsoft.Network/loadBalancers/probes/join/action | Bir yük dengeleyici araştırmalarını kullanan sağlar. Örneğin, VM ölçek ile bu izni healthProbe özellik kümesi araştırma başvurabilirsiniz. Alertable değil. |
> | Eylem | Microsoft.Network/loadBalancers/probes/read | Bir yük dengeleyici araştırması alır |
> | Eylem | Microsoft.Network/loadBalancers/read | Bir yük dengeleyici tanımı alır |
> | Eylem | Microsoft.Network/loadBalancers/virtualMachines/read | Tüm sanal makinelere yük dengeleyici altında başvuruları alır |
> | Eylem | Microsoft.Network/loadBalancers/write | Bir yük dengeleyici oluşturur veya mevcut bir yük dengeleyiciye güncelleştirir |
> | Eylem | Microsoft.Network/localnetworkgateways/delete | LocalNetworkGateway siler |
> | Eylem | Microsoft.Network/localnetworkgateways/read | LocalNetworkGateway alır |
> | Eylem | Microsoft.Network/localnetworkgateways/write | Oluşturur veya mevcut bir LocalNetworkGateway güncelleştirir |
> | Eylem | Microsoft.Network/locations/autoApprovedPrivateLinkServices/read | Özel Bağlantı Hizmetleri alır otomatik Onaylandı |
> | Eylem | Microsoft.Network/locations/availableDelegations/read | Kullanılabilir temsilcileri alır |
> | Eylem | Microsoft.Network/locations/availablePrivateEndpointTypes/read | Kullanılabilir özel uç nokta kaynakları alır. |
> | Eylem | Microsoft.Network/locations/bareMetalTenants/action | Ayırır veya bir çıplak Metal Kiracı doğrular |
> | Eylem | Microsoft.Network/locations/checkAcceleratedNetworkingSupport/action | Hızlandırılmış ağ desteği denetler |
> | Eylem | Microsoft.Network/locations/checkDnsNameAvailability/read | DNS etiketi belirtilen konumda kullanılabilir olup olmadığını denetler |
> | Eylem | Microsoft.Network/locations/checkPrivateLinkServiceVisibility/action | Özel bir bağlantı hizmeti görünürlük denetimleri |
> | Eylem | Microsoft.Network/locations/operationResults/read | Bir zaman uyumsuz POST ya da silme işlemi işleminin sonucunu alır |
> | Eylem | Microsoft.Network/locations/operations/read | Zaman uyumsuz bir işlemin durumunu temsil eden bir işlem kaynağı alır |
> | Eylem | Microsoft.Network/locations/serviceTags/read | Hizmet etiketleri alın |
> | Eylem | Microsoft.Network/locations/supportedVirtualMachineSizes/read | Sanal makine boyutları alır desteklenir |
> | Eylem | Microsoft.Network/locations/usages/read | Kaynakları kullanım ölçümleri alır |
> | Eylem | Microsoft.Network/locations/virtualNetworkAvailableEndpointServices/read | Kullanılabilir sanal ağ uç noktası hizmetlerin bir listesini alır |
> | Eylem | Microsoft.Network/networkIntentPolicies/delete | Bir ağ hedefi ilkesini siler |
> | Eylem | Microsoft.Network/networkIntentPolicies/read | Bir ağ hedefi ilke açıklamasını alır |
> | Eylem | Microsoft.Network/networkIntentPolicies/write | Bir ağ hedefi ilkesi oluşturur veya mevcut bir ağ hedefi İlkesi güncelleştirir |
> | Eylem | Microsoft.Network/networkInterfaces/delete | Bir ağ arabirimi siler |
> | Eylem | Microsoft.Network/networkInterfaces/effectiveNetworkSecurityGroups/action | Ağ güvenlik grupları yapılandırılmış üzerinde ağ arabirimini Vm Al |
> | Eylem | Microsoft.Network/networkInterfaces/effectiveRouteTable/action | Sanal makinenin ağ arabiriminde yapılandırılan rota tablosunu alın |
> | Eylem | Microsoft.Network/networkInterfaces/ipconfigurations/join/action | Bir ağ arabirimi IP yapılandırması birleştirir. Alertable değil. |
> | Eylem | Microsoft.Network/networkInterfaces/ipconfigurations/read | Bir ağ arabirimi IP yapılandırması tanımı alır.  |
> | Eylem | Microsoft.Network/networkInterfaces/join/action | Bir sanal makineye bir ağ arabirimi ile birleştirir. Alertable değil. |
> | Eylem | Microsoft.Network/networkInterfaces/loadBalancers/read | Ağ arabirimi parçası olan tüm yük dengeleyicileri alır |
> | Eylem | Microsoft.Network/networkInterfaces/read | Bir ağ arabirimi tanımı alır.  |
> | Eylem | Microsoft.Network/networkInterfaces/tapConfigurations/delete | Bir ağ arabirimi dokunun yapılandırmasını siler. |
> | Eylem | Microsoft.Network/networkInterfaces/tapConfigurations/read | Bir ağ arabirimi dokunun yapılandırmasını alır. |
> | Eylem | Microsoft.Network/networkInterfaces/tapConfigurations/write | Bir ağ arabirimi yapılandırması dokunun oluşturur veya var olan ağ arabirimi yapılandırmasını dokunun güncelleştirir. |
> | Eylem | Microsoft.Network/networkInterfaces/write | Bir ağ arabirimi oluşturur veya mevcut bir ağ arabirimi güncelleştirir.  |
> | Eylem | Microsoft.Network/networkProfiles/delete | Bir ağ profili siler |
> | Eylem | Microsoft.Network/networkProfiles/read | Bir ağ profili alır |
> | Eylem | Microsoft.Network/networkProfiles/removeContainers/action | Kapsayıcılar kaldırır |
> | Eylem | Microsoft.Network/networkProfiles/setContainers/action | Kapsayıcıları ayarlar |
> | Eylem | Microsoft.Network/networkProfiles/setNetworkInterfaces/action | Kapsayıcı ağ arabirimleri ayarlar |
> | Eylem | Microsoft.Network/networkProfiles/write | Oluşturur veya bir ağ profili güncelleştirir |
> | Eylem | Microsoft.Network/networkSecurityGroups/defaultSecurityRules/read | Varsayılan güvenlik kural tanımı alır |
> | Eylem | Microsoft.Network/networkSecurityGroups/delete | Bir ağ güvenlik grubunu siler |
> | Eylem | Microsoft.Network/networkSecurityGroups/join/action | Bir ağ güvenlik grubu birleştirir. Alertable değil. |
> | Eylem | Microsoft.Network/networkSecurityGroups/read | Bir ağ güvenlik grubu tanımı alır |
> | Eylem | Microsoft.Network/networkSecurityGroups/securityRules/delete | Güvenlik kuralını siler |
> | Eylem | Microsoft.Network/networkSecurityGroups/securityRules/read | Güvenlik kuralı tanımlarken alır |
> | Eylem | Microsoft.Network/networkSecurityGroups/securityRules/write | Bir güvenlik kuralı oluşturur veya mevcut bir güvenlik kuralı güncelleştirir |
> | Eylem | Microsoft.Network/networkSecurityGroups/write | Bir ağ güvenlik grubu oluşturur veya mevcut bir ağ güvenlik grubu güncelleştirir |
> | Eylem | Microsoft.Network/networkWatchers/availableProvidersList/action | Tüm kullanılabilir internet hizmet sağlayıcıları için belirtilen bir Azure bölgesine döndürür. |
> | Eylem | Microsoft.Network/networkWatchers/azureReachabilityReport/action | Göreli gecikme puanını için internet hizmet sağlayıcıları belirtilen bir konumdan Azure bölgelerine döndürür. |
> | Eylem | Microsoft.Network/networkWatchers/configureFlowLog/action | Hedef kaynak için akış günlüğü yapılandırır. |
> | Eylem | Microsoft.Network/networkWatchers/connectionMonitors/delete | Bağlantı İzleyicisi siler |
> | Eylem | Microsoft.Network/networkWatchers/connectionMonitors/query/action | Belirtilen uç noktaları arasında bağlantı izleme sorgusu |
> | Eylem | Microsoft.Network/networkWatchers/connectionMonitors/read | Bağlantı İzleyicisi ayrıntılarını Al |
> | Eylem | Microsoft.Network/networkWatchers/connectionMonitors/start/action | Belirtilen uç noktaları arasında bağlantı izlemeye başlama |
> | Eylem | Microsoft.Network/networkWatchers/connectionMonitors/stop/action | Belirtilen uç noktaları arasında bağlantı İzlemeyi Durdur/duraklatma |
> | Eylem | Microsoft.Network/networkWatchers/connectionMonitors/write | Bağlantı İzleyicisi oluşturur |
> | Eylem | Microsoft.Network/networkWatchers/connectivityCheck/action | Bir sanal makineden doğrudan TCP bağlantısı için başka bir VM veya rastgele bir uzak sunucuya dahil olmak üzere belirli bir uç nokta oluşturma olasılığını doğrular. |
> | Eylem | Microsoft.Network/networkWatchers/delete | Ağ İzleyicisi siler |
> | Eylem | Microsoft.Network/networkWatchers/ipFlowVerify/action | Paket izin veya için veya belirli bir hedef reddedildi döndürür. |
> | Eylem | Microsoft.Network/networkWatchers/lenses/delete | Bir mercek siler |
> | Eylem | Microsoft.Network/networkWatchers/lenses/query/action | Belirtilen uç noktasındaki ağ trafiğini izleme sorgusu |
> | Eylem | Microsoft.Network/networkWatchers/lenses/read | Odağı ayrıntılarını Al |
> | Eylem | Microsoft.Network/networkWatchers/lenses/start/action | Ağ trafiğinin belirtilen uç noktasında izlemeye başlama |
> | Eylem | Microsoft.Network/networkWatchers/lenses/stop/action | Durdur/izleme ağ trafiğinin belirtilen uç noktasında Duraklat |
> | Eylem | Microsoft.Network/networkWatchers/lenses/write | Bir mercek oluşturur |
> | Eylem | Microsoft.Network/networkWatchers/networkConfigurationDiagnostic/action | Ağ yapılandırması tanılama. |
> | Eylem | Microsoft.Network/networkWatchers/nextHop/action | Belirtilen hedef ve hedef IP adresi, sonraki atlama türü dönün ve IP adresi sonraki umuyoruz. |
> | Eylem | Microsoft.Network/networkWatchers/packetCaptures/delete | Paket yakalaması siler |
> | Eylem | Microsoft.Network/networkWatchers/packetCaptures/queryStatus/action | Özellikler ve bir paket yakalama kaynak durumu hakkındaki bilgileri alır. |
> | Eylem | Microsoft.Network/networkWatchers/packetCaptures/read | Paket yakalamayı tanımını Al |
> | Eylem | Microsoft.Network/networkWatchers/packetCaptures/stop/action | Çalışan paket yakalama oturumu durdurun. |
> | Eylem | Microsoft.Network/networkWatchers/packetCaptures/write | Paket yakalaması oluşturur |
> | Eylem | Microsoft.Network/networkWatchers/pingMeshes/delete | Bir PingMesh siler |
> | Eylem | Microsoft.Network/networkWatchers/pingMeshes/read | PingMesh ayrıntılarını Al |
> | Eylem | Microsoft.Network/networkWatchers/pingMeshes/start/action | Belirtilen sanal makineler arasında PingMesh Başlat |
> | Eylem | Microsoft.Network/networkWatchers/pingMeshes/stop/action | Belirtilen sanal makineler arasında PingMesh Durdur |
> | Eylem | Microsoft.Network/networkWatchers/pingMeshes/write | Bir PingMesh oluşturur |
> | Eylem | Microsoft.Network/networkWatchers/queryConnectionMonitors/action | Toplu işlem sorguları belirtilen uç noktaları arasında bağlantı izleme |
> | Eylem | Microsoft.Network/networkWatchers/queryFlowLogStatus/action | Akış bir kaynakta oturum durumunu alır. |
> | Eylem | Microsoft.Network/networkWatchers/queryTroubleshootResult/action | Sorun giderme sonucu daha önce çalışan veya şu anda, sorun giderme işlemi çalıştıran alır. |
> | Eylem | Microsoft.Network/networkWatchers/read | Ağ İzleyicisi tanımı Al |
> | Eylem | Microsoft.Network/networkWatchers/securityGroupView/action | Bir VM'de uygulanan yapılandırılmış ve etkin ağ güvenlik grubu kurallarını görüntüleyin. |
> | Eylem | Microsoft.Network/networkWatchers/topology/action | Ağ düzeyinde görünümü kaynaklarının ve aralarındaki ilişkiler bir kaynak grubunda yer alır. |
> | Eylem | Microsoft.Network/networkWatchers/troubleshoot/action | Azure'da bir ağ kaynağına sorun gidermeyi başlatır. |
> | Eylem | Microsoft.Network/networkWatchers/write | Ağ İzleyicisi oluşturur veya mevcut bir Ağ İzleyicisi'ni güncelleştirmeleri |
> | Eylem | Microsoft.Network/operations/read | Kullanılabilir işlemleri Al |
> | Eylem | Microsoft.Network/p2sVpnGateways/delete | Bir P2SVpnGateway siler. |
> | Eylem | Microsoft.Network/p2sVpnGateways/generatevpnprofile/action | P2SVpnGateway için VPN profili oluştur |
> | Eylem | Microsoft.Network/p2sVpnGateways/getp2svpnconnectionhealth/action | P2SVpnGateway için P2S Vpn bağlantısı sağlık durumunu alır |
> | Eylem | Microsoft.Network/p2sVpnGateways/read | Bir P2SVpnGateway alır. |
> | Eylem | Microsoft.Network/p2sVpnGateways/write | Bir P2SVpnGateway koyar. |
> | Eylem | Microsoft.Network/privateDnsOperationResults/read | İşlem özel DNS sonuçlarını alır |
> | Eylem | Microsoft.Network/privateDnsOperationStatuses/read | İşlem özel DNS durumunu alır |
> | Eylem | Microsoft.Network/privateDnsZones/A/delete | Belirli bir ada kayıt kümesini kaldırın ve bir özel DNS bölgesi ' A' yazın. |
> | Eylem | Microsoft.Network/privateDnsZones/A/read | JSON biçiminde bir özel DNS bölge içerisindeki ' A' türündeki kayıt kümesi alın. Kayıt kümesi kayıtları yanı sıra TTL, etiketler ve etag listesini içerir. |
> | Eylem | Microsoft.Network/privateDnsZones/A/write | Veya bir özel DNS bölge içerisindeki 'A' türünde bir kayıt kümesi güncelleştirilemiyor. Belirtilen kayıt kayıt kümesindeki geçerli kayıt yerini alır. |
> | Eylem | Microsoft.Network/privateDnsZones/AAAA/delete | Belirli bir ada kayıt kümesini kaldırın ve bir özel DNS bölgesi'nden ' AAAA' yazın. |
> | Eylem | Microsoft.Network/privateDnsZones/AAAA/read | Kayıt kümesi türünde bir özel DNS bölge içerisindeki ' AAAA' JSON biçiminde alın. Kayıt kümesi kayıtları yanı sıra TTL, etiketler ve etag listesini içerir. |
> | Eylem | Microsoft.Network/privateDnsZones/AAAA/write | Veya bir özel DNS bölge içerisindeki 'AAAA' türündeki kayıt kümesi güncelleştirilemiyor. Belirtilen kayıt kayıt kümesindeki geçerli kayıt yerini alır. |
> | Eylem | Microsoft.Network/privateDnsZones/ALL/read | Özel DNS kayıt kümelerini türlerinde alır. |
> | Eylem | Microsoft.Network/privateDnsZones/CNAME/delete | Belirli bir ada kayıt kümesini kaldırın ve bir özel DNS bölgesi'nden ' CNAME' yazın. |
> | Eylem | Microsoft.Network/privateDnsZones/CNAME/read | Kayıt kümesi türünde bir özel DNS bölge içerisindeki ' CNAME' JSON biçiminde alın. |
> | Eylem | Microsoft.Network/privateDnsZones/CNAME/write | Veya bir özel DNS bölge içerisindeki 'CNAME' türündeki kayıt kümesi güncelleştirilemiyor. |
> | Eylem | Microsoft.Network/privateDnsZones/delete | Özel DNS bölgesi silin. |
> | Eylem | Microsoft.Network/privateDnsZones/MX/delete | Belirli bir ada kayıt kümesini kaldırın ve bir özel DNS bölgesi'nden "MX" yazın. |
> | Eylem | Microsoft.Network/privateDnsZones/MX/read | JSON biçiminde bir özel DNS bölge içerisindeki ' MX' türündeki kayıt kümesi alın. Kayıt kümesi kayıtları yanı sıra TTL, etiketler ve etag listesini içerir. |
> | Eylem | Microsoft.Network/privateDnsZones/MX/write | Veya bir özel DNS bölgesi içinde "MX" türünde bir kayıt kümesi güncelleştirilemiyor. Belirtilen kayıt kayıt kümesindeki geçerli kayıt yerini alır. |
> | Eylem | Microsoft.Network/privateDnsZones/PTR/delete | Belirli bir ada kayıt kümesini kaldırın ve bir özel DNS bölgesi'nden ' PTR' yazın. |
> | Eylem | Microsoft.Network/privateDnsZones/PTR/read | Kayıt kümesi türünde bir özel DNS bölge içerisindeki ' PTR' JSON biçiminde alın. Kayıt kümesi kayıtları yanı sıra TTL, etiketler ve etag listesini içerir. |
> | Eylem | Microsoft.Network/privateDnsZones/PTR/write | Veya bir özel DNS bölge içerisindeki 'PTR' türündeki kayıt kümesi güncelleştirilemiyor. Belirtilen kayıt kayıt kümesindeki geçerli kayıt yerini alır. |
> | Eylem | Microsoft.Network/privateDnsZones/read | Özel DNS bölge özellikleri JSON biçiminde alın. Bu komut özel DNS bölgesi bağlı olduğu sanal ağlar almıyorsa Not veya kayıt kümelerini bölge içinde yer alan. |
> | Eylem | Microsoft.Network/privateDnsZones/recordsets/read | Özel DNS kayıt kümelerini türlerinde alır. |
> | Eylem | Microsoft.Network/privateDnsZones/SOA/read | Kayıt kümesi türünde bir özel DNS bölge içerisindeki ' SOA' JSON biçiminde alın. |
> | Eylem | Microsoft.Network/privateDnsZones/SOA/write | Bir özel DNS bölge içerisindeki 'SOA' türündeki kayıt kümesi güncelleştirin. |
> | Eylem | Microsoft.Network/privateDnsZones/SRV/delete | Belirli bir ada kayıt kümesini kaldırın ve bir özel DNS bölgesi'nden ' SRV' yazın. |
> | Eylem | Microsoft.Network/privateDnsZones/SRV/read | Kayıt kümesi türünde bir özel DNS bölge içerisindeki ' SRV' JSON biçiminde alın. Kayıt kümesi kayıtları yanı sıra TTL, etiketler ve etag listesini içerir. |
> | Eylem | Microsoft.Network/privateDnsZones/SRV/write | Veya bir özel DNS bölge içerisindeki 'SRV' türündeki kayıt kümesi güncelleştirilemiyor. Belirtilen kayıt kayıt kümesindeki geçerli kayıt yerini alır. |
> | Eylem | Microsoft.Network/privateDnsZones/TXT/delete | Belirli bir ada kayıt kümesini kaldırın ve bir özel DNS bölgesi'nden ' TXT' yazın. |
> | Eylem | Microsoft.Network/privateDnsZones/TXT/read | Kayıt kümesi türünde bir özel DNS bölge içerisindeki ' TXT' JSON biçiminde alın. Kayıt kümesi kayıtları yanı sıra TTL, etiketler ve etag listesini içerir. |
> | Eylem | Microsoft.Network/privateDnsZones/TXT/write | Veya bir özel DNS bölge içerisindeki 'TXT' türündeki kayıt kümesi güncelleştirilemiyor. Belirtilen kayıt kayıt kümesindeki geçerli kayıt yerini alır. |
> | Eylem | Microsoft.Network/privateDnsZones/virtualNetworkLinks/delete | Sanal ağ özel DNS bölgesi bağlantısını silin. |
> | Eylem | Microsoft.Network/privateDnsZones/virtualNetworkLinks/read | Özel DNS bölgesi bağlantı için sanal ağ özellikleri, JSON biçiminde alın. |
> | Eylem | Microsoft.Network/privateDnsZones/virtualNetworkLinks/write | Veya sanal ağ için bir özel DNS bölgesi bağlantı güncelleştirilemiyor. |
> | Eylem | Microsoft.Network/privateDnsZones/write | Veya bir özel DNS bölgesi bir kaynak grubu içindeki güncelleştirilemiyor. Bu komutu oluşturmak veya sanal ağ bağlantıları veya bölgede kayıt kümelerini güncelleştirmek için kullanılamayacağını unutmayın. |
> | Eylem | Microsoft.Network/privateEndpoints/delete | Bir özel uç nokta kaynağı siler. |
> | Eylem | Microsoft.Network/privateEndpoints/read | Bir özel uç nokta kaynağı alır. |
> | Eylem | Microsoft.Network/privateEndpoints/write | Yeni bir özel uç nokta oluşturur veya mevcut bir özel uç nokta güncelleştirir. |
> | Eylem | Microsoft.Network/privateLinkServices/delete | Özel bir hizmet kaynağı siler. |
> | Eylem | Microsoft.Network/privateLinkServices/privateEndpointConnections/delete | Bir özel uç noktası bağlantısını siler. |
> | Eylem | Microsoft.Network/privateLinkServices/privateEndpointConnections/read | Bir özel uç nokta bağlantı tanımı alır. |
> | Eylem | Microsoft.Network/privateLinkServices/privateEndpointConnections/write | Yeni bir özel uç nokta bağlantı oluşturur veya mevcut bir özel uç nokta bağlantı güncelleştirir. |
> | Eylem | Microsoft.Network/privateLinkServices/read | Özel bir hizmet kaynağı alır. |
> | Eylem | Microsoft.Network/privateLinkServices/write | Yeni bir özel bağlantı hizmeti oluşturur veya mevcut bir özel bağlantı hizmeti güncelleştirir. |
> | Eylem | Microsoft.Network/publicIPAddresses/delete | Genel bir IP adresi siler. |
> | Eylem | Microsoft.Network/publicIPAddresses/join/action | Bir genel IP adresi birleştirir. Alertable değil. |
> | Eylem | Microsoft.Network/publicIPAddresses/read | Bir genel IP adresi tanımı alır. |
> | Eylem | Microsoft.Network/publicIPAddresses/write | Genel bir IP adresi oluşturur veya var olan bir genel IP adresini güncelleştirir.  |
> | Eylem | Microsoft.Network/publicIPPrefixes/delete | Bir genel IP öneki siler |
> | Eylem | Microsoft.Network/publicIPPrefixes/join/action | Bir PublicIPPrefix birleştirir. Alertable değil. |
> | Eylem | Microsoft.Network/publicIPPrefixes/read | Bir genel IP öneki tanımı alır |
> | Eylem | Microsoft.Network/publicIPPrefixes/write | Bir genel IP önek oluşturur veya mevcut bir ortak IP öneki güncelleştirir |
> | Eylem | Microsoft.Network/register/action | Aboneliği kaydeder |
> | Eylem | Microsoft.Network/routeFilters/delete | Bir rota filtre tanımını siler |
> | Eylem | Microsoft.Network/routeFilters/join/action | Bir rota filtresinde birleştirir. Alertable değil. |
> | Eylem | Microsoft.Network/routeFilters/read | Bir rota filtre tanımını alır |
> | Eylem | Microsoft.Network/routeFilters/routeFilterRules/delete | Rota filtresi kuralı tanımını siler |
> | Eylem | Microsoft.Network/routeFilters/routeFilterRules/read | Rota filtresi kuralı tanımı alır |
> | Eylem | Microsoft.Network/routeFilters/routeFilterRules/write | Rota filtresi kuralının oluşturur veya mevcut bir rota filtresi kuralı güncelleştirmeleri |
> | Eylem | Microsoft.Network/routeFilters/write | Rota filtresi oluşturur veya var olan bir rota filtresi güncelleştirir |
> | Eylem | Microsoft.Network/routeTables/delete | Bir rota tablosu tanımını siler |
> | Eylem | Microsoft.Network/routeTables/join/action | Bir yol tablosu birleştirir. Alertable değil. |
> | Eylem | Microsoft.Network/routeTables/read | Bir rota tablosu tanımı alır |
> | Eylem | Microsoft.Network/routeTables/routes/delete | Bir rota tanımını siler |
> | Eylem | Microsoft.Network/routeTables/routes/read | Bir yönlendirme tanımı alır |
> | Eylem | Microsoft.Network/routeTables/routes/write | Bir yol oluşturur veya var olan bir rota güncelleştirir |
> | Eylem | Microsoft.Network/routeTables/write | Bir rota tablosu oluşturur veya mevcut bir yol tablosu güncelleştirir |
> | Eylem | Microsoft.Network/securegateways/delete | Güvenli ağ geçidi silme |
> | Eylem | Microsoft.Network/securegateways/read | Güvenli ağ geçidi Al |
> | Eylem | Microsoft.Network/securegateways/write | Oluşturur veya güncelleştirir güvenli bir ağ geçidi |
> | Eylem | Microsoft.Network/serviceEndpointPolicies/delete | Bir hizmet uç noktası İlkesi siler |
> | Eylem | Microsoft.Network/serviceEndpointPolicies/join/action | Bir hizmet uç noktası İlkesi birleştirir. Alertable değil. |
> | Eylem | Microsoft.Network/serviceEndpointPolicies/joinSubnet/action | Bir alt ağ için hizmet uç noktası İlkesi birleştirir. Alertable değil. |
> | Eylem | Microsoft.Network/serviceEndpointPolicies/read | Hizmet uç noktası İlkesi açıklamasını alır |
> | Eylem | Microsoft.Network/serviceEndpointPolicies/serviceEndpointPolicyDefinitions/delete | Bir hizmet uç noktası ilke tanımı siler |
> | Eylem | Microsoft.Network/serviceEndpointPolicies/serviceEndpointPolicyDefinitions/read | Hizmet uç noktası ilke tanımı açıklamasını alır |
> | Eylem | Microsoft.Network/serviceEndpointPolicies/serviceEndpointPolicyDefinitions/write | Bir hizmet uç noktası ilke tanımı oluşturur veya mevcut bir hizmet uç noktası ilke tanımı güncelleştirmeleri |
> | Eylem | Microsoft.Network/serviceEndpointPolicies/write | Bir hizmet uç noktası ilkesi oluşturur veya mevcut bir hizmet uç noktası İlkesi güncelleştirir |
> | Eylem | Microsoft.Network/trafficManagerGeographicHierarchies/read | Coğrafi trafik yönlendirme yöntemini ile kullanılabilen bölgeler içeren Traffic Manager coğrafi hiyerarşi alır |
> | Eylem | Microsoft.Network/trafficManagerProfiles/azureEndpoints/delete | Bir Azure uç noktası, var olan bir Traffic Manager profili siler. Traffic Manager, silinen Azure uç noktası için trafiği yönlendirme durdurur. |
> | Eylem | Microsoft.Network/trafficManagerProfiles/azureEndpoints/read | Bir Azure Traffic Manager, Azure uç noktası tüm özellikleri dahil olmak üzere bir profiline ait uç noktasını alır. |
> | Eylem | Microsoft.Network/trafficManagerProfiles/azureEndpoints/write | Yeni bir Azure uç noktası var olan bir Traffic Manager profilini ekleyin veya mevcut bir Azure uç noktası, Traffic Manager profilindeki özelliklerini güncelleştirir. |
> | Eylem | Microsoft.Network/trafficManagerProfiles/delete | Traffic Manager profilini silin. Traffic Manager profili ile ilişkili tüm ayarlar kaybedilir ve profili artık trafiği yönlendirmek için kullanılabilir. |
> | Eylem | Microsoft.Network/trafficManagerProfiles/externalEndpoints/delete | Dış uç noktası, var olan bir Traffic Manager profili siler. Traffic Manager, silinen dış uç noktası için trafiği yönlendirme durdurur. |
> | Eylem | Microsoft.Network/trafficManagerProfiles/externalEndpoints/read | Traffic Manager dış uç noktanın tüm özellikleri dahil olmak üzere bir profiline ait olduğu bir dış uç alır. |
> | Eylem | Microsoft.Network/trafficManagerProfiles/externalEndpoints/write | Yeni bir dış uç noktası mevcut bir Traffic Manager profilinde ekleyebilir veya var olan dış uç noktası, Traffic Manager profilindeki özelliklerini güncelleştirir. |
> | Eylem | Microsoft.Network/trafficManagerProfiles/heatMaps/read | Traffic Manager ısı Haritası konum ve kaynak IP olarak sorgu sayısı ve gecikme süresi verileri içeren belirli Traffic Manager profilini alır. |
> | Eylem | Microsoft.Network/trafficManagerProfiles/nestedEndpoints/delete | Bir iç içe uç nokta, var olan bir Traffic Manager profili siler. Traffic Manager yönlendirme trafiği silinen iç içe uç noktasına durdurur. |
> | Eylem | Microsoft.Network/trafficManagerProfiles/nestedEndpoints/read | Bir iç içe Traffic Manager iç içe uç noktanın tüm özellikleri dahil olmak üzere bir profiline ait uç noktasını alır. |
> | Eylem | Microsoft.Network/trafficManagerProfiles/nestedEndpoints/write | Var olan bir Traffic Manager profilini yeni bir iç içe uç nokta ekleyin veya mevcut bir iç içe uç noktası, Traffic Manager profilindeki özelliklerini güncelleştirir. |
> | Eylem | Microsoft.Network/trafficManagerProfiles/read | Traffic Manager profilinin yapılandırma alın. Bu, DNS ayarları, trafik yönlendirme ayarlarını, uç nokta izleme ayarları ve bu Traffic Manager profili tarafından yönlendirilen bitiş noktaları listesini içerir. |
> | Eylem | Microsoft.Network/trafficManagerProfiles/write | Bir Traffic Manager profili oluşturun veya var olan bir Traffic Manager profilini yapılandırmasını değiştirin.<br>Bu, etkinleştirme veya bir profili devre dışı bırakılıyor ve DNS ayarları, trafik yönlendirme ayarlarını veya uç nokta izleme ayarları değiştirme içerir.<br>Uç noktaları Traffic Manager profili tarafından yönlendirilen eklenen, kaldırılır, etkin veya devre dışı. |
> | Eylem | Microsoft.Network/trafficManagerUserMetricsKeys/delete | Gerçek kullanıcı ölçümleri koleksiyon için kullanılan abonelik düzeyinde anahtarını siler. |
> | Eylem | Microsoft.Network/trafficManagerUserMetricsKeys/read | Gerçek kullanıcı ölçümleri koleksiyon için kullanılan abonelik düzeyinde anahtarını alır. |
> | Eylem | Microsoft.Network/trafficManagerUserMetricsKeys/write | Gerçek kullanıcı ölçümleri koleksiyonu için kullanılacak yeni bir abonelik düzeyinde anahtarı oluşturur. |
> | Eylem | Microsoft.Network/unregister/action | Abonelik kaydını siler |
> | Eylem | Microsoft.Network/virtualHubs/delete | Bir sanal hub'ı siler |
> | Eylem | Microsoft.Network/virtualHubs/hubVirtualNetworkConnections/delete | Bir HubVirtualNetworkConnection siler |
> | Eylem | Microsoft.Network/virtualHubs/hubVirtualNetworkConnections/read | Bir HubVirtualNetworkConnection Al |
> | Eylem | Microsoft.Network/virtualHubs/hubVirtualNetworkConnections/write | Bir HubVirtualNetworkConnection güncelle |
> | Eylem | Microsoft.Network/virtualHubs/read | Sanal hub'ını Al |
> | Eylem | Microsoft.Network/virtualHubs/write | Oluşturma veya bir sanal hub'ı güncelleştirme |
> | Eylem | Microsoft.Network/virtualnetworkgateways/Connections/Read | Get VirtualNetworkGatewayConnection |
> | Eylem | Microsoft.Network/virtualNetworkGateways/delete | Bir virtualNetworkGateway siler |
> | Eylem | Microsoft.Network/virtualnetworkgateways/generatevpnclientpackage/Action | VirtualNetworkGateway yönelik bir VpnClient paketi oluştur |
> | Eylem | Microsoft.Network/virtualnetworkgateways/generatevpnprofile/Action | VirtualNetworkGateway için VpnProfile paketini oluşturma |
> | Eylem | Microsoft.Network/virtualnetworkgateways/getadvertisedroutes/Action | Alır virtualNetworkGateway tanıtılan yollar |
> | Eylem | Microsoft.Network/virtualnetworkgateways/getbgppeerstatus/Action | VirtualNetworkGateway bgp eş durumunu alır |
> | Eylem | Microsoft.Network/virtualnetworkgateways/getlearnedroutes/Action | Virtualnetworkgateway öğrenilen rotalar alır |
> | Eylem | Microsoft.Network/virtualnetworkgateways/getvpnclientconnectionhealth/Action | VirtualNetworkGateway için VPN istemcisi bağlantı durumu Al |
> | Eylem | Microsoft.Network/virtualnetworkgateways/getvpnclientipsecparameters/Action | Vpnclient IPSec parametreleri için VirtualNetworkGateway P2S istemcilere alın. |
> | Eylem | Microsoft.Network/virtualnetworkgateways/getvpnprofilepackageurl/Action | Önceden oluşturulan vpn istemci profili paket URL'sini alır |
> | Eylem | Microsoft.Network/virtualNetworkGateways/read | Bir VirtualNetworkGateway alır |
> | Eylem | Microsoft.Network/virtualnetworkgateways/Reset/Action | Bir virtualNetworkGateway sıfırlar |
> | Eylem | Microsoft.Network/virtualnetworkgateways/resetvpnclientsharedkey/Action | VirtualNetworkGateway P2S istemcilere paylaşılan anahtarı Vpnclient sıfırlayın. |
> | Eylem | Microsoft.Network/virtualnetworkgateways/setvpnclientipsecparameters/Action | Vpnclient IPSec VirtualNetworkGateway P2S istemcilere parametrelerini ayarlayın. |
> | Eylem | Microsoft.Network/virtualnetworkgateways/supportedvpndevices/action | Desteklenen Vpn cihazları listeler |
> | Eylem | Microsoft.Network/virtualNetworkGateways/write | Oluşturur veya bir VirtualNetworkGateway güncelleştirir |
> | Eylem | Microsoft.Network/virtualNetworks/BastionHosts/action | Kale ana bilgisayarı başvurular bir sanal ağ içinde yer alır. |
> | Eylem | Microsoft.Network/virtualNetworks/bastionHosts/default/action | Kale ana bilgisayarı başvurular bir sanal ağ içinde yer alır. |
> | Eylem | Microsoft.Network/virtualNetworks/checkIpAddressAvailability/read | Belirtilen sanal ağ IP adresi olup olmadığını denetleyin |
> | Eylem | Microsoft.Network/virtualNetworks/delete | Bir sanal ağı siler |
> | Eylem | Microsoft.Network/virtualNetworks/peer/action | Bir sanal ağ başka bir sanal ağ ile eşleri |
> | Eylem | Microsoft.Network/virtualNetworks/read | Sanal ağ tanımı Al |
> | Eylem | Microsoft.Network/virtualNetworks/subnets/delete | Bir sanal ağ alt ağını siler |
> | Eylem | Microsoft.Network/virtualNetworks/subnets/join/action | Bir sanal ağa katılır. Alertable değil. |
> | Eylem | Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Kaynak depolama hesabı veya SQL veritabanı gibi bir alt ağa katılır. Alertable değil. |
> | Eylem | Microsoft.Network/virtualNetworks/subnets/prepareNetworkPolicies/action | Gerekli ağ ilkeleri uygulayarak bir alt ağ hazırlar. |
> | Eylem | Microsoft.Network/virtualNetworks/subnets/read | Bir sanal ağ alt ağı tanımı alır |
> | Eylem | Microsoft.Network/virtualNetworks/subnets/virtualMachines/read | Bir sanal ağ alt ağında tüm sanal makineleri başvurularını alır |
> | Eylem | Microsoft.Network/virtualNetworks/subnets/write | Bir sanal ağ alt ağı oluşturur veya mevcut bir sanal ağ alt ağını güncelleştirir |
> | Eylem | Microsoft.Network/virtualNetworks/usages/read | Her sanal ağ alt ağ için IP kullanımlarını Al |
> | Eylem | Microsoft.Network/virtualNetworks/virtualMachines/read | Bir sanal ağ tüm sanal makineler için başvurular alır |
> | Eylem | Microsoft.Network/virtualNetworks/virtualNetworkPeerings/delete | Bir sanal ağ eşlemesi siler |
> | Eylem | Microsoft.Network/virtualNetworks/virtualNetworkPeerings/read | Bir sanal ağ eşleme tanımı alır |
> | Eylem | Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write | Bir sanal ağ eşlemesi oluşturur veya mevcut bir sanal ağ eşlemesi güncelleştirir |
> | Eylem | Microsoft.Network/virtualNetworks/write | Bir sanal ağ oluşturur veya mevcut bir sanal ağ güncelleştirmeleri |
> | Eylem | Microsoft.Network/virtualNetworkTaps/delete | Sanal ağ Tap Sil |
> | Eylem | Microsoft.Network/virtualNetworkTaps/join/action | Bir sanal ağ tap birleştirir. Alertable değil. |
> | Eylem | Microsoft.Network/virtualNetworkTaps/read | Sanal ağ Tap'ı Al |
> | Eylem | Microsoft.Network/virtualNetworkTaps/write | Sanal ağ Tap güncelle |
> | Eylem | Microsoft.Network/virtualWans/delete | Wan sanal bir siler |
> | Eylem | Microsoft.network/virtualWans/p2sVpnServerConfigurations/delete | Bir sanal Wan P2SVpnServerConfiguration siler |
> | Eylem | Microsoft.Network/virtualWans/p2sVpnServerConfigurations/read | Bir sanal Wan P2SVpnServerConfiguration alır |
> | Eylem | Microsoft.network/virtualWans/p2sVpnServerConfigurations/write | Bir sanal Wan P2SVpnServerConfiguration oluşturur veya mevcut bir sanal Wan P2SVpnServerConfiguration güncelleştirir |
> | Eylem | Microsoft.Network/virtualWans/read | Alma işlemi sanal Wan |
> | Eylem | Microsoft.Network/virtualwans/supportedSecurityProviders/read | Alır VirtualWan güvenlik sağlayıcıları desteklenir. |
> | Eylem | Microsoft.Network/virtualWans/virtualHubs/read | Tüm sanal sanal Wan başvuran hub'ları alır. |
> | Eylem | Microsoft.Network/virtualwans/vpnconfiguration/action | Vpn yapılandırmasını alır |
> | Eylem | Microsoft.Network/virtualWans/vpnSites/read | Sanal Wan başvuran tüm VPN siteler alır. |
> | Eylem | Microsoft.Network/virtualWans/write | Oluşturma veya güncelleştirme sanal Wan |
> | Eylem | Microsoft.Network/vpnGateways/delete | Bir VpnGateway siler. |
> | Eylem | Microsoft.Network/vpngateways/listvpnconnectionshealth/Action | Tüm bağlantı durumu veya bağlantıları bir alt kümesi üzerinde bir VpnGateway alır |
> | Eylem | Microsoft.Network/vpnGateways/read | Bir VpnGateway alır. |
> | Eylem | Microsoft.Network/vpngateways/Reset/Action | Bir VpnGateway sıfırlar |
> | Eylem | microsoft.network/vpnGateways/vpnConnections/delete | Bir VpnConnection siler. |
> | Eylem | microsoft.network/vpnGateways/vpnConnections/read | Bir VpnConnection alır. |
> | Eylem | microsoft.network/vpnGateways/vpnConnections/write | Bir VpnConnection koyar. |
> | Eylem | Microsoft.Network/vpnGateways/write | Bir VpnGateway koyar. |
> | Eylem | Microsoft.Network/vpnsites/delete | Vpn sitesi kaynağını siler. |
> | Eylem | Microsoft.Network/vpnsites/read | Vpn sitesi kaynağını alır. |
> | Eylem | Microsoft.Network/vpnsites/write | Oluşturur veya bir Vpn sitesi kaynağını güncelleştirir. |

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.NotificationHubs/CheckNamespaceAvailability/action | Belirtilen Namespace kaynak adının NotificationHub hizmetinde kullanılabilir olup olmadığını denetler. |
> | Eylem | Microsoft.NotificationHubs/Namespaces/authorizationRules/action | Ad alanı yetkilendirme kuralları açıklamasının listesini alın. |
> | Eylem | Microsoft.NotificationHubs/Namespaces/authorizationRules/delete | Namespace yetkilendirme kuralını silin. Varsayılan Namespace yetkilendirme kuralı silinemez.  |
> | Eylem | Microsoft.NotificationHubs/Namespaces/authorizationRules/listkeys/action | Namespace bağlantı dizesini alın |
> | Eylem | Microsoft.NotificationHubs/Namespaces/authorizationRules/read | Ad alanı yetkilendirme kuralları açıklamasının listesini alın. |
> | Eylem | Microsoft.NotificationHubs/Namespaces/authorizationRules/regenerateKeys/action | Namespace yetkilendirme kuralı yeniden birincil/ikincil anahtarı oluşturma, yeniden oluşturulması gereken anahtarı belirtin |
> | Eylem | Microsoft.NotificationHubs/Namespaces/authorizationRules/write | Namespace düzeyinde yetkilendirme kuralları oluşturun ve özelliklerini güncelleştirin. Yetkilendirme kuralları erişim hakları, birincil ve ikincil anahtarlar güncelleştirilebilir. |
> | Eylem | Microsoft.NotificationHubs/Namespaces/CheckNotificationHubAvailability/action | Belirli bir NotificationHub adının bir Namespace içinde kullanılabilir olup olmadığını denetler. |
> | Eylem | Microsoft.NotificationHubs/Namespaces/Delete | Namespace kaynağı silme |
> | Eylem | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/action | Bildirim hub'ı yetkilendirme kurallarının listesini alın |
> | Eylem | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/delete | Bildirim hub'ı yetkilendirme kurallarını Sil |
> | Eylem | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/listkeys/action | Bildirim hub'ı bağlantı dizesini alın |
> | Eylem | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/read | Bildirim hub'ı yetkilendirme kurallarının listesini alın |
> | Eylem | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/regenerateKeys/action | Bildirim hub'ı yetkilendirme kuralı yeniden birincil/ikincil anahtarı oluşturma, yeniden oluşturulması gereken anahtarı belirtin |
> | Eylem | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/write | Bildirim hub'ı yetkilendirme kuralları oluşturun ve özelliklerini güncelleştirin. Yetkilendirme kuralları erişim hakları, birincil ve ikincil anahtarlar güncelleştirilebilir. |
> | Eylem | Microsoft.NotificationHubs/Namespaces/NotificationHubs/debugSend/action | Bir test anında iletme bildirimi gönderin. |
> | Eylem | Microsoft.NotificationHubs/Namespaces/NotificationHubs/Delete | Bildirim hub'ı kaynağını Sil |
> | Eylem | Microsoft.NotificationHubs/Namespaces/NotificationHubs/metricDefinitions/read | Namespace ölçümleri kaynak açıklamalarının listesini alın |
> | Eylem | Microsoft.NotificationHubs/Namespaces/NotificationHubs/pnsCredentials/action | Tüm bildirim hub'ı PNS kimlik bilgilerini alın. Bu içerir, WNS, MPNS, APNS, GCM ve Baidu kimlik bilgileri |
> | Eylem | Microsoft.NotificationHubs/Namespaces/NotificationHubs/read | Bildirim hub'ı kaynak açıklamalarının listesini alın |
> | Eylem | Microsoft.NotificationHubs/Namespaces/NotificationHubs/write | Bildirim hub'ı oluşturma ve özelliklerini güncelleştirin. Özelliklerini temel olarak PNS kimlik bilgilerini içerir. Yetkilendirme kuralları ve TTL'den |
> | Eylem | Microsoft.NotificationHubs/Namespaces/read | Namespace kaynak açıklamasının listesini alır |
> | Eylem | Microsoft.NotificationHubs/Namespaces/write | Namespace kaynağı oluşturun ve özelliklerini güncelleştirin. Etiketleri ve kapasitesi Namespace hangi güncelleştirilebilecek özelliklerdir. |
> | Eylem | Microsoft.NotificationHubs/operationResults/read | Notification hubs'ı sağlayıcısı için işlem sonuçlarını döndürür |
> | Eylem | Microsoft.NotificationHubs/operations/read | Notification hubs'ı sağlayıcısı için desteklenen işlemler listesini döndürür |
> | Eylem | Microsoft.NotificationHubs/register/action | Aboneliği NotificationHubs kaynak sağlayıcısı için kaydeder ve ad alanları ve NotificationHubs oluşturulmasını sağlar |
> | Eylem | Microsoft.NotificationHubs/unregister/action | Aboneliği NotificationHubs kaynak sağlayıcısı kaydını siler ve ad alanları ve NotificationHubs oluşturulmasını sağlar |

## <a name="microsoftoffazure"></a>Microsoft.OffAzure

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.OffAzure/HyperVSites/clusters/read | Bir Hyper-V kümesi özelliklerini alır |
> | Eylem | Microsoft.OffAzure/HyperVSites/clusters/write | Oluşturur veya güncelleştirir Hyper-V kümesi |
> | Eylem | Microsoft.OffAzure/HyperVSites/delete | Hyper-V sitesini siler |
> | Eylem | Microsoft.OffAzure/HyperVSites/hosts/read | Bir Hyper-V konağı özelliklerini alır |
> | Eylem | Microsoft.OffAzure/HyperVSites/hosts/write | Oluşturur veya güncelleştirir Hyper-V konağı |
> | Eylem | Microsoft.OffAzure/HyperVSites/jobs/read | Hyper-V proje özelliklerini alır |
> | Eylem | Microsoft.OffAzure/HyperVSites/machines/read | Bir Hyper-V makinelerine özelliklerini alır |
> | Eylem | Microsoft.OffAzure/HyperVSites/machines/start/action | Hyper-V makinelerini başlatma |
> | Eylem | Microsoft.OffAzure/HyperVSites/machines/stop/action | Hyper-V makinelerine durdurur |
> | Eylem | Microsoft.OffAzure/HyperVSites/operationsstatus/read | Bir Hyper-V işlem durumunu özelliklerini alır |
> | Eylem | Microsoft.OffAzure/HyperVSites/read | Bir Hyper-V site özelliklerini alır |
> | Eylem | Microsoft.OffAzure/HyperVSites/refresh/action | Bir Hyper-V sitesi içindeki nesneleri yeniler |
> | Eylem | Microsoft.OffAzure/HyperVSites/runasaccounts/read | Farklı Çalıştır hesapları bir Hyper-V özelliklerini alır |
> | Eylem | Microsoft.OffAzure/HyperVSites/usage/read | Bir Hyper-V sitesinin kullanımları alır |
> | Eylem | Microsoft.OffAzure/HyperVSites/write | Oluşturur veya Hyper-V sitesini güncelleştirir |
> | Eylem | Microsoft.OffAzure/Operations/read | Açığa çıkarılan işlemleri okur |
> | Eylem | Microsoft.OffAzure/register/action | Aboneliği Microsoft.OffAzure kaynak sağlayıcısına kaydeder |
> | Eylem | Microsoft.OffAzure/VMwareSites/delete | VMware sitesi siler |
> | Eylem | Microsoft.OffAzure/VMwareSites/jobs/read | Bir VMware işleri özelliklerini alır |
> | Eylem | Microsoft.OffAzure/VMwareSites/machines/read | Bir VMware makinelerini özelliklerini alır |
> | Eylem | Microsoft.OffAzure/VMwareSites/machines/start/action | VMware makinelerini başlatma |
> | Eylem | Microsoft.OffAzure/VMwareSites/machines/stop/action | VMware makinelerini durdurur |
> | Eylem | Microsoft.OffAzure/VMwareSites/operationsstatus/read | Bir VMware işlem durumunu özelliklerini alır |
> | Eylem | Microsoft.OffAzure/VMwareSites/read | Bir VMware sitesi özelliklerini alır |
> | Eylem | Microsoft.OffAzure/VMwareSites/refresh/action | Bir VMware sitesi'içindeki nesneleri yeniler |
> | Eylem | Microsoft.OffAzure/VMwareSites/runasaccounts/read | Farklı Çalıştır hesapları bir VMware özelliklerini alır |
> | Eylem | Microsoft.OffAzure/VMwareSites/usage/read | Bir VMware sitesi kullanımları alır |
> | Eylem | Microsoft.OffAzure/VMwareSites/vcenters/read | Bir VMware vCenter özelliklerini alır |
> | Eylem | Microsoft.OffAzure/VMwareSites/vcenters/write | Oluşturur veya VMware vCenter güncelleştirir |
> | Eylem | Microsoft.OffAzure/VMwareSites/write | Oluşturur veya güncelleştirir VMware sitesi |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.OperationalInsights/linkTargets/read | Bir Azure aboneliği ile ilişkili olmayan mevcut hesapları listeler. Bu Azure aboneliğinin bir çalışma alanına bağlamak için bu işlem çalışma alanı Oluştur işleminin Müşteri Kimliği özelliğinde döndürülen müşteri kimliğini kullanın. |
> | Eylem | Microsoft.operationalinsights/Operations/Read | Tüm kullanılabilir Operationalınsights Rest API işlemleri listeler. |
> | Eylem | Microsoft.operationalinsights/Register/Action | Rergisters abonelik. |
> | Eylem | Microsoft.OperationalInsights/register/action | Bir kaynak sağlayıcısına bir abonelik kaydedin. |
> | Eylem | microsoft.operationalinsights/unregister/action | Abonelik kaydını siler. |
> | Eylem | Microsoft.OperationalInsights/workspaces/analytics/query/action | Yeni altyapıyı kullanarak arama yapın. |
> | Eylem | Microsoft.OperationalInsights/workspaces/analytics/query/schema/read | Arama şeması V2 alın. |
> | Eylem | Microsoft.OperationalInsights/workspaces/api/query/action | Yeni altyapıyı kullanarak arama yapın. |
> | Eylem | Microsoft.OperationalInsights/workspaces/api/query/schema/read | Arama şeması V2 alın. |
> | Eylem | Microsoft.OperationalInsights/workspaces/configurationScopes/delete | Yapılandırma kapsamını Sil |
> | Eylem | Microsoft.OperationalInsights/workspaces/configurationScopes/read | Yapılandırma kapsamını Al |
> | Eylem | Microsoft.OperationalInsights/workspaces/configurationScopes/write | Yapılandırma kapsamı kurma |
> | Eylem | Microsoft.OperationalInsights/workspaces/datasources/delete | Bir çalışma alanındaki veri kaynaklarını silin. |
> | Eylem | Microsoft.OperationalInsights/workspaces/datasources/read | Bir çalışma alanındaki veri kaynaklarını alın. |
> | Eylem | Microsoft.OperationalInsights/workspaces/datasources/write | Bir çalışma alanındaki veri kaynaklarını oluşturun/güncelleştirin. |
> | Eylem | Microsoft.OperationalInsights/workspaces/delete | Bir çalışma alanı siler. Çalışma alanı, oluşturma zamanında mevcut bir çalışma alanına bağlı bağlanılan çalışma alanı silinmez. |
> | Eylem | Microsoft.OperationalInsights/workspaces/gateways/delete | Çalışma alanı için yapılandırılmış bir ağ geçidi kaldırır. |
> | Eylem | Microsoft.OperationalInsights/workspaces/generateregistrationcertificate/action | Çalışma alanı için kayıt sertifikası oluşturur. Bu sertifika, Microsoft System Center Operation Manager çalışma alanına bağlamak için kullanılır. |
> | Eylem | Microsoft.OperationalInsights/workspaces/intelligencepacks/disable/action | Belirli bir çalışma alanının akıllı paketini devre dışı bırakır. |
> | Eylem | Microsoft.OperationalInsights/workspaces/intelligencepacks/enable/action | Belirli bir çalışma alanının akıllı paketini etkinleştirir. |
> | Eylem | Microsoft.OperationalInsights/workspaces/intelligencepacks/read | Belirli bir çalışma alanı için görünür olan tüm akıllı paketleri ve paketi etkin veya devre dışı bu çalışma alanı için ayrıca listelenir. |
> | Eylem | Microsoft.OperationalInsights/workspaces/linkedServices/delete | Belirtilen çalışma alanı altındaki bağlı Hizmetleri silin. |
> | Eylem | Microsoft.OperationalInsights/workspaces/linkedServices/read | Altındaki bağlı Hizmetleri alın belirtilen çalışma alanı. |
> | Eylem | Microsoft.OperationalInsights/workspaces/linkedServices/write | Belirtilen çalışma alanı oluşturma/bağlı güncelleştirme hizmetleri altında. |
> | Eylem | Microsoft.OperationalInsights/workspaces/listKeys/action | Çalışma alanının liste anahtarlarını alır. Bu anahtarlar, Microsoft operasyonel İçgörüler aracılarını çalışma alanına bağlamak için kullanılır. |
> | Eylem | Microsoft.OperationalInsights/workspaces/listKeys/read | Çalışma alanının liste anahtarlarını alır. Bu anahtarlar, Microsoft operasyonel İçgörüler aracılarını çalışma alanına bağlamak için kullanılır. |
> | Eylem | Microsoft.OperationalInsights/workspaces/managementGroups/read | Bu çalışma alanına bağlı System Center Operations Manager yönetim gruplarının adlarını ve meta verilerini alır. |
> | Eylem | Microsoft.OperationalInsights/workspaces/metricDefinitions/read | Çalışma alanı altında ölçüm tanımlarını Al |
> | Eylem | Microsoft.OperationalInsights/workspaces/notificationSettings/delete | Çalışma alanının Kullanıcı bildirim ayarlarını silin. |
> | Eylem | Microsoft.OperationalInsights/workspaces/notificationSettings/read | Çalışma alanının Kullanıcı bildirim ayarlarını alın. |
> | Eylem | Microsoft.OperationalInsights/workspaces/notificationSettings/write | Çalışma alanının Kullanıcı bildirim ayarlarını ayarlayın. |
> | Eylem | Microsoft.operationalinsights/Workspaces/Operations/Read | Operationalınsights çalışma alanı işleminin durumunu alır. |
> | Eylem | Microsoft.OperationalInsights/workspaces/purge/action | Belirtilen veriyi çalışma alanından silin |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/AADDomainServicesAccountLogon/read | AADDomainServicesAccountLogon tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/AADDomainServicesAccountManagement/read | AADDomainServicesAccountManagement tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/AADDomainServicesDirectoryServiceAccess/read | AADDomainServicesDirectoryServiceAccess tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/AADDomainServicesLogonLogoff/read | AADDomainServicesLogonLogoff tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/AADDomainServicesPolicyChange/read | AADDomainServicesPolicyChange tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/AADDomainServicesPrivilegeUse/read | AADDomainServicesPrivilegeUse tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/AADDomainServicesSystemSecurity/read | AADDomainServicesSystemSecurity tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ADAssessmentRecommendation/read | ADAssessmentRecommendation tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ADFActivityRun/read | ADFActivityRun tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ADFPipelineRun/read | ADFPipelineRun tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ADFTriggerRun/read | ADFTriggerRun tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ADReplicationResult/read | ADReplicationResult tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ADSecurityAssessmentRecommendation/read | ADSecurityAssessmentRecommendation tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/Alert/read | Uyarı tablosundan veri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/AlertHistory/read | AlertHistory tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/AppCenterError/read | AppCenterError tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ApplicationInsights/read | Applicationınsights tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/AuditLogs/read | Bulunan tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/AutoscaleEvaluationsLog/read | AutoscaleEvaluationsLog tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/AutoscaleScaleActionsLog/read | AutoscaleScaleActionsLog tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/AWSCloudTrail/read | AWSCloudTrail tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/AzureActivity/read | AzureActivity tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/AzureAssessmentRecommendation/read | AzureAssessmentRecommendation tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/AzureMetrics/read | AzureMetrics tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/BlockchainApplicationLog/read | BlockchainApplicationLog tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/BlockchainProxyLog/read | BlockchainProxyLog tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/BoundPort/read | BoundPort tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/CommonSecurityLog/read | CommonSecurityLog tablosundan veri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ComputerGroup/read | ComputerGroup tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ConfigurationChange/read | ConfigurationChange tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ConfigurationData/read | ConfigurationData tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ContainerImageInventory/read | ContainerImageInventory tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ContainerInventory/read | ContainerInventory tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ContainerLog/read | ContainerLog tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ContainerNodeInventory/read | ContainerNodeInventory tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ContainerServiceLog/read | ContainerServiceLog tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DatabricksAccounts/read | DatabricksAccounts tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DatabricksClusters/read | DatabricksClusters tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DatabricksDBFS/read | DatabricksDBFS tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DatabricksJobs/read | DatabricksJobs tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DatabricksNotebook/read | DatabricksNotebook tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DatabricksSecrets/read | DatabricksSecrets tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DatabricksSQLPermissions/read | DatabricksSQLPermissions tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DatabricksSSH/read | DatabricksSSH tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DatabricksTables/read | DatabricksTables tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DatabricksWorkspace/read | DatabricksWorkspace tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DeviceAppCrash/read | DeviceAppCrash tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DeviceAppLaunch/read | DeviceAppLaunch tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DeviceCalendar/read | DeviceCalendar tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DeviceCleanup/read | DeviceCleanup tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DeviceConnectSession/read | DeviceConnectSession tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DeviceEtw/read | DeviceEtw tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DeviceHardwareHealth/read | DeviceHardwareHealth tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DeviceHealth/read | DeviceHealth tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DeviceHeartbeat/read | DeviceHeartbeat tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DeviceSkypeHeartbeat/read | DeviceSkypeHeartbeat tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DeviceSkypeSignIn/read | DeviceSkypeSignIn tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DeviceSleepState/read | DeviceSleepState tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DHAppFailure/read | DHAppFailure tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DHAppReliability/read | DHAppReliability tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DHDriverReliability/read | DHDriverReliability tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DHLogonFailures/read | DHLogonFailures tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DHLogonMetrics/read | DHLogonMetrics tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DHOSCrashData/read | DHOSCrashData tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DHOSReliability/read | DHOSReliability tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DHWipAppLearning/read | DHWipAppLearning tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DnsEvents/read | DnsEvents tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DnsInventory/read | DnsInventory tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ETWEvent/read | ETWEvent tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/Event/read | Olay tablosunu okuma verileri |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ExchangeAssessmentRecommendation/read | ExchangeAssessmentRecommendation tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ExchangeOnlineAssessmentRecommendation/read | ExchangeOnlineAssessmentRecommendation tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/Heartbeat/read | Sinyal tablosunda veri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/HuntingBookmark/read | HuntingBookmark tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/IISAssessmentRecommendation/read | IISAssessmentRecommendation tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/InboundConnection/read | InboundConnection tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/InsightsMetrics/read | InsightsMetrics tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/IntuneAuditLogs/read | IntuneAuditLogs tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/IntuneOperationalLogs/read | IntuneOperationalLogs tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/KubeEvents/read | KubeEvents tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/KubeNodeInventory/read | KubeNodeInventory tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/KubePodInventory/read | KubePodInventory tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/KubeServices/read | KubeServices tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/LinuxAuditLog/read | LinuxAuditLog tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAApplication/read | MAApplication tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAApplicationHealth/read | MAApplicationHealth tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAApplicationHealthAlternativeVersions/read | MAApplicationHealthAlternativeVersions tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAApplicationHealthIssues/read | MAApplicationHealthIssues tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAApplicationInstance/read | MAApplicationInstance tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAApplicationInstanceReadiness/read | MAApplicationInstanceReadiness tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAApplicationReadiness/read | MAApplicationReadiness tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MADeploymentPlan/read | MADeploymentPlan tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MADevice/read | MADevice tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MADeviceNotEnrolled/read | MADeviceNotEnrolled tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MADeviceNRT/read | MADeviceNRT tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MADevicePnPHealth/read | MADevicePnPHealth tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MADevicePnPHealthAlternativeVersions/read | MADevicePnPHealthAlternativeVersions tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MADevicePnPHealthIssues/read | MADevicePnPHealthIssues tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MADeviceReadiness/read | MADeviceReadiness tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MADriverInstanceReadiness/read | MADriverInstanceReadiness tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MADriverReadiness/read | MADriverReadiness tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddin/read | MAOfficeAddin tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddinEntityHealth/read | MAOfficeAddinEntityHealth tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddinHealth/read | MAOfficeAddinHealth tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddinHealthEventNRT/read | MAOfficeAddinHealthEventNRT tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddinHealthIssues/read | MAOfficeAddinHealthIssues tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddinInstance/read | MAOfficeAddinInstance tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddinInstanceReadiness/read | MAOfficeAddinInstanceReadiness tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddinReadiness/read | MAOfficeAddinReadiness tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeApp/read | MAOfficeApp tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeAppCrashesNRT/read | MAOfficeAppCrashesNRT tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeAppHealth/read | MAOfficeAppHealth tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeAppInstance/read | MAOfficeAppInstance tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeAppInstanceHealth/read | MAOfficeAppInstanceHealth tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeAppReadiness/read | MAOfficeAppReadiness tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeAppSessionsNRT/read | MAOfficeAppSessionsNRT tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeBuildInfo/read | MAOfficeBuildInfo tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeCurrencyAssessment/read | MAOfficeCurrencyAssessment tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeCurrencyAssessmentDailyCounts/read | MAOfficeCurrencyAssessmentDailyCounts tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeDeploymentStatus/read | MAOfficeDeploymentStatus tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeDeploymentStatusNRT/read | MAOfficeDeploymentStatusNRT tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeMacroErrorNRT/read | MAOfficeMacroErrorNRT tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeMacroGlobalHealth/read | MAOfficeMacroGlobalHealth tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeMacroHealth/read | MAOfficeMacroHealth tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeMacroHealthIssues/read | MAOfficeMacroHealthIssues tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeMacroIssueInstanceReadiness/read | MAOfficeMacroIssueInstanceReadiness tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeMacroIssueReadiness/read | MAOfficeMacroIssueReadiness tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeMacroSummary/read | MAOfficeMacroSummary tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeSuite/read | MAOfficeSuite tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeSuiteInstance/read | MAOfficeSuiteInstance tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAProposedPilotDevices/read | MAProposedPilotDevices tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAWindowsBuildInfo/read | MAWindowsBuildInfo tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAWindowsCurrencyAssessment/read | MAWindowsCurrencyAssessment tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAWindowsCurrencyAssessmentDailyCounts/read | MAWindowsCurrencyAssessmentDailyCounts tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAWindowsDeploymentStatus/read | MAWindowsDeploymentStatus tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAWindowsDeploymentStatusNRT/read | MAWindowsDeploymentStatusNRT tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAWindowsSysReqInstanceReadiness/read | MAWindowsSysReqInstanceReadiness tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MicrosoftWebApplicationLog/read | MicrosoftWebApplicationLog tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MicrosoftWebFunctionExecutionLogs/read | MicrosoftWebFunctionExecutionLogs tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MicrosoftWebStdOutStdErrLog/read | MicrosoftWebStdOutStdErrLog tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MicrosoftWebW3CLog/read | MicrosoftWebW3CLog tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/NetworkMonitoring/read | NetworkMonitoring tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/OfficeActivity/read | OfficeActivity tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/Operation/read | İşlem tablodan veri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/OutboundConnection/read | OutboundConnection tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/Perf/read | İyileştirilmiş tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ProtectionStatus/read | ProtectionStatus tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/read | Çalışma alanındaki veriler üzerinde sorgular çalıştırın. |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ReservedAzureCommonFields/read | ReservedAzureCommonFields tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ReservedCommonFields/read | ReservedCommonFields tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/SCCMAssessmentRecommendation/read | SCCMAssessmentRecommendation tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/SCOMAssessmentRecommendation/read | SCOMAssessmentRecommendation tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/SecurityAlert/read | SecurityAlert tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/SecurityBaseline/read | SecurityBaseline tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/SecurityBaselineSummary/read | SecurityBaselineSummary tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/SecurityDetection/read | SecurityDetection tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/SecurityEvent/read | SecurityEvent tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/SecurityIoTRawEvent/read | SecurityIoTRawEvent tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/SecurityRecommendation/read | SecurityRecommendation tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ServiceFabricOperationalEvent/read | ServiceFabricOperationalEvent tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ServiceFabricReliableActorEvent/read | ServiceFabricReliableActorEvent tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ServiceFabricReliableServiceEvent/read | ServiceFabricReliableServiceEvent tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/SfBAssessmentRecommendation/read | SfBAssessmentRecommendation tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/SfBOnlineAssessmentRecommendation/read | SfBOnlineAssessmentRecommendation tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/SharePointOnlineAssessmentRecommendation/read | SharePointOnlineAssessmentRecommendation tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/SigninLogs/read | SigninLogs tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/SPAssessmentRecommendation/read | SPAssessmentRecommendation tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/SQLAssessmentRecommendation/read | SQLAssessmentRecommendation tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/SQLQueryPerformance/read | SQLQueryPerformance tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/SqlThreatProtectionLoginAudits/read | SqlThreatProtectionLoginAudits tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/SqlVulnerabilityAssessmentResult/read | SqlVulnerabilityAssessmentResult tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/Syslog/read | Syslog tablosunda veri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/SysmonEvent/read | SysmonEvent tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/Tables.Custom/read | Herhangi bir özel günlük verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ThreatIntelligenceIndicator/read | ThreatIntelligenceIndicator tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/UAApp/read | UAApp tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/UAComputer/read | UAComputer tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/UAComputerRank/read | UAComputerRank tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/UADriver/read | UADriver tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/UADriverProblemCodes/read | UADriverProblemCodes tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/UAFeedback/read | UAFeedback tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/UAHardwareSecurity/read | UAHardwareSecurity tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/UAIESiteDiscovery/read | UAIESiteDiscovery tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/UAOfficeAddIn/read | UAOfficeAddIn tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/UAProposedActionPlan/read | UAProposedActionPlan tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/UASysReqIssue/read | UASysReqIssue tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/UAUpgradedComputer/read | UAUpgradedComputer tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/Update/read | Güncelleştirme tablosunda veri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/UpdateRunProgress/read | UpdateRunProgress tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/UpdateSummary/read | UpdateSummary tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/Usage/read | Kullanım tablosunda veri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/VMBoundPort/read | VMBoundPort tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/VMConnection/read | VMConnection tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/W3CIISLog/read | W3cııslog tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/WaaSDeploymentStatus/read | WaaSDeploymentStatus tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/WaaSInsiderStatus/read | WaaSInsiderStatus tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/WaaSUpdateStatus/read | WaaSUpdateStatus tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/WDAVStatus/read | WDAVStatus tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/WDAVThreat/read | WDAVThreat tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/WindowsClientAssessmentRecommendation/read | WindowsClientAssessmentRecommendation tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/WindowsEvent/read | WindowsEvent tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/WindowsFirewall/read | WindowsFirewall tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/WindowsServerAssessmentRecommendation/read | WindowsServerAssessmentRecommendation tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/WireData/read | İletilen veri tablosunda veri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/WorkloadMonitoringPerf/read | WorkloadMonitoringPerf tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/WUDOAggregatedStatus/read | WUDOAggregatedStatus tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/WUDOStatus/read | WUDOStatus tablodaki verileri okuma |
> | Eylem | Microsoft.OperationalInsights/workspaces/read | Mevcut bir çalışma alanını alır |
> | Eylem | Microsoft.OperationalInsights/workspaces/regeneratesharedkey/action | Belirtilen çalışma alanı paylaşılan anahtarı yeniden oluşturur |
> | Eylem | Microsoft.operationalinsights/Workspaces/Rules/Read | Uyarı kurallarının tümünü alın. |
> | Eylem | Microsoft.OperationalInsights/workspaces/savedSearches/delete | Kaydedilmiş bir arama sorgusunu siler |
> | Eylem | Microsoft.OperationalInsights/workspaces/savedSearches/read | Kaydedilmiş arama sorgusu alır |
> | Eylem | Microsoft.operationalinsights/Workspaces/savedsearches/Results/Read | Arama sonuçları kaydedilir. Kullanım Dışı |
> | Eylem | microsoft.operationalinsights/workspaces/savedsearches/schedules/actions/delete | Zamanlanmış arama Eylemler silin. |
> | Eylem | Microsoft.operationalinsights/Workspaces/savedsearches/Schedules/Actions/Read | Zamanlanmış arama eylemleri Al. |
> | Eylem | Microsoft.operationalinsights/Workspaces/savedsearches/Schedules/Actions/Write | Veya zamanlanmış arama eylemleri güncelleştirilemiyor. |
> | Eylem | microsoft.operationalinsights/workspaces/savedsearches/schedules/delete | Zamanlanmış aramaları silin. |
> | Eylem | Microsoft.operationalinsights/Workspaces/savedsearches/Schedules/Read | Zamanlanmış aramaları alın. |
> | Eylem | Microsoft.operationalinsights/Workspaces/savedsearches/Schedules/Write | Veya zamanlanmış aramaları güncelleştirilemiyor. |
> | Eylem | Microsoft.OperationalInsights/workspaces/savedSearches/write | Kaydedilmiş arama sorgusu oluşturur |
> | Eylem | Microsoft.OperationalInsights/workspaces/schema/read | Çalışma alanı için arama şemasını alır.  Arama şeması, kullanıma sunulmuş alanları ve bunların türlerini içerir. |
> | Eylem | Microsoft.OperationalInsights/workspaces/search/action | Arama sorgusu yürütür |
> | Eylem | microsoft.operationalinsights/workspaces/search/read | Arama sonuçları alın. Kullanım dışı. |
> | Eylem | Microsoft.OperationalInsights/workspaces/sharedKeys/action | Çalışma alanı paylaşılan anahtarlarını alır. Bu anahtarlar, Microsoft operasyonel İçgörüler aracılarını çalışma alanına bağlamak için kullanılır. |
> | Eylem | Microsoft.OperationalInsights/workspaces/sharedKeys/read | Çalışma alanı paylaşılan anahtarlarını alır. Bu anahtarlar, Microsoft operasyonel İçgörüler aracılarını çalışma alanına bağlamak için kullanılır. |
> | Eylem | Microsoft.OperationalInsights/workspaces/storageinsightconfigs/delete | Depolama yapılandırması siler. Bu, Microsoft operasyonel İçgörüler depolama hesabından veri okumasını durdurur. |
> | Eylem | Microsoft.OperationalInsights/workspaces/storageinsightconfigs/read | Depolama yapılandırması alır. |
> | Eylem | Microsoft.OperationalInsights/workspaces/storageinsightconfigs/write | Yeni bir depolama yapılandırması oluşturur. Bu yapılandırmalar mevcut bir depolama hesabındaki bir konumdan veri çekmek için kullanılır. |
> | Eylem | Microsoft.OperationalInsights/workspaces/upgradetranslationfailures/read | Çalışma alanı için arama yükseltme çeviri hata günlüğünün alınması |
> | Eylem | Microsoft.OperationalInsights/workspaces/usages/read | Çalışma alanı tarafından okunan veri miktarı dahil çalışma alanı kullanım verilerini alır. |
> | Eylem | Microsoft.OperationalInsights/workspaces/write | Yeni çalışma alanı veya mevcut bir çalışma alanı bağlantıları var olan çalışma alanından Müşteri Kimliği sağlayarak oluşturur. |

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.OperationsManagement/managementAssociations/delete | Varolan yönetim ilişkilendirmesini silme |
> | Eylem | Microsoft.OperationsManagement/managementAssociations/read | Varolan yönetim ilişkilendirmesini alma |
> | Eylem | Microsoft.OperationsManagement/managementAssociations/write | Yeni bir yönetim ilişkilendirmesi oluşturma |
> | Eylem | Microsoft.OperationsManagement/managementConfigurations/delete | Varolan yönetim yapılandırmasını silme |
> | Eylem | Microsoft.OperationsManagement/managementConfigurations/read | Varolan yönetim yapılandırmasını alma |
> | Eylem | Microsoft.OperationsManagement/managementConfigurations/write | Yeni bir yönetim yapılandırması oluşturma |
> | Eylem | Microsoft.OperationsManagement/register/action | Bir kaynak sağlayıcısına bir abonelik kaydedin. |
> | Eylem | Microsoft.OperationsManagement/solutions/delete | Mevcut OMS çözümünü silme |
> | Eylem | Microsoft.OperationsManagement/solutions/read | Mevcut OMS çözümünü edinme |
> | Eylem | Microsoft.OperationsManagement/solutions/write | Yeni OMS çözümü oluşturma |

## <a name="microsoftpolicyinsights"></a>Microsoft.policyınsights

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.PolicyInsights/asyncOperationResults/read | Zaman uyumsuz işlem sonucunu alır. |
> | Eylem | Microsoft.PolicyInsights/operations/read | Microsoft.policyınsights ad alanı işlemlerinde alır desteklenir |
> | Eylem | Microsoft.PolicyInsights/policyEvents/queryResults/action | İlke olaylar hakkında bilgi sorgulayın. |
> | Eylem | Microsoft.PolicyInsights/policyEvents/queryResults/read | İlke olaylar hakkında bilgi sorgulayın. |
> | Eylem | Microsoft.PolicyInsights/policyStates/queryResults/action | İlkesi durumları hakkında bilgi sorgulayın. |
> | Eylem | Microsoft.PolicyInsights/policyStates/queryResults/read | İlkesi durumları hakkında bilgi sorgulayın. |
> | Eylem | Microsoft.PolicyInsights/policyStates/summarize/action | En son İlkesi durumları hakkında özet bilgileri sorgulayın. |
> | Eylem | Microsoft.PolicyInsights/policyStates/summarize/read | En son İlkesi durumları hakkında özet bilgileri sorgulayın. |
> | Eylem | Microsoft.PolicyInsights/policyStates/triggerEvaluation/action | Seçilen kapsam için yeni bir uyumluluk değerlendirmesi tetikler. |
> | Eylem | Microsoft.PolicyInsights/policyTrackedResources/queryResults/read | Deployıfnotexists İlkesi tarafından gerekli kaynaklar hakkında bilgi sorgulayın. |
> | Eylem | Microsoft.PolicyInsights/register/action | Microsoft Policy Insights kaynak sağlayıcısını kaydeder ve eylemleri sağlar. |
> | Eylem | Microsoft.PolicyInsights/remediations/cancel/action | Microsoft Policy düzeltmeler sürüyor iptal edin. |
> | Eylem | Microsoft.PolicyInsights/remediations/delete | İlke düzeltmeler silin. |
> | Eylem | Microsoft.PolicyInsights/remediations/listDeployments/read | Bir ilke düzeltme tarafından gerekli dağıtımlar listeler. |
> | Eylem | Microsoft.PolicyInsights/remediations/read | İlke düzeltmeler alın. |
> | Eylem | Microsoft.PolicyInsights/remediations/write | Microsoft Policy düzeltmeler güncelle yeniden açın. |
> | Eylem | Microsoft.PolicyInsights/unregister/action | Microsoft Policy Insights kaynak sağlayıcısını kaydını siler. |

## <a name="microsoftportal"></a>Microsoft.Portal

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Portal/consoles/delete | Cloud Shell örneğini kaldırır. |
> | Eylem | Microsoft.Portal/consoles/read | Cloud Shell örneği okur. |
> | Eylem | Microsoft.Portal/consoles/write | Veya bir Cloud Shell örneği güncelleştirilemiyor. |
> | Eylem | Microsoft.Portal/dashboards/delete | Panoyu abonelikten kaldırır. |
> | Eylem | Microsoft.Portal/dashboards/read | Abonelik için panoları okur. |
> | Eylem | Microsoft.Portal/dashboards/write | Ekleyin veya bir aboneliğe Pano değiştirin. |
> | Eylem | Microsoft.Portal/register/action | Portalına kaydedin |
> | Eylem | Microsoft.Portal/usersettings/delete | Cloud Shell kullanıcı ayarlarını kaldırır. |
> | Eylem | Microsoft.Portal/usersettings/read | Cloud Shell kullanıcı ayarlarını okur. |
> | Eylem | Microsoft.Portal/usersettings/write | Veya Cloud Shell kullanıcı ayarı güncelleştirilemiyor. |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.PowerBIDedicated/capacities/delete | Siler Power BI ayrılmış kapasitesini. |
> | Eylem | Microsoft.PowerBIDedicated/capacities/read | Belirtilen Power BI ayrılmış kapasitesinin bilgilerini alır. |
> | Eylem | Microsoft.PowerBIDedicated/capacities/resume/action | Kapasitesini sürdürür. |
> | Eylem | Microsoft.PowerBIDedicated/capacities/skus/read | Kapasite için kullanılabilir SKU bilgilerini alın |
> | Eylem | Microsoft.PowerBIDedicated/capacities/suspend/action | Kapasite askıya alır. |
> | Eylem | Microsoft.PowerBIDedicated/capacities/write | Oluşturur veya belirtilen Power BI ayrılmış kapasitesi güncelleştirir. |
> | Eylem | Microsoft.PowerBIDedicated/locations/checkNameAvailability/action | Power BI ayrılmış kapasitesi adının geçerli olup olmadığını denetler ve kullanımda. |
> | Eylem | Microsoft.PowerBIDedicated/locations/operationresults/read | Belirtilen işlem sonucu bilgilerini alır. |
> | Eylem | Microsoft.PowerBIDedicated/locations/operationstatuses/read | Belirtilen işlem durumu bilgilerini alır. |
> | Eylem | Microsoft.PowerBIDedicated/operations/read | İşlem bilgilerini alır. |
> | Eylem | Microsoft.PowerBIDedicated/register/action | Power BI ayrılmış kaynak sağlayıcısını kaydeder. |
> | Eylem | Microsoft.PowerBIDedicated/skus/read | SKU bilgilerini alır. |

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp, hizmet tarafından kullanılan iç işlemdir. |
> | Eylem | Microsoft.RecoveryServices/locations/allocateStamp/action | AllocateStamp hizmeti tarafından kullanılan iç işlemdir. |
> | Eylem | microsoft.recoveryservices/Locations/backupPreValidateProtection/action |  |
> | Eylem | microsoft.recoveryservices/Locations/backupProtectedItem/write | Yedekleme korumalı öğesi oluştur |
> | Eylem | microsoft.recoveryservices/Locations/backupProtectedItems/read | Tüm Korumalı Öğelerin listesini döndürür. |
> | Eylem | microsoft.recoveryservices/Locations/backupStatus/action | Kurtarma Hizmetleri kasaları için yedekleme durumunu denetle |
> | Eylem | microsoft.recoveryservices/Locations/backupValidateFeatures/action | Özellikleri doğrulama |
> | Eylem | Microsoft.RecoveryServices/locations/checkNameAvailability/action | Kaynak adı kullanılabilir olup olmadığını denetlemek için bir API kaynak adının kullanılabilirliğini olup olmadığını denetleyin |
> | Eylem | Microsoft.RecoveryServices/locations/operationStatus/read | Belirli bir işlemi için işlem durumunu alır |
> | Eylem | Microsoft.RecoveryServices/operations/read | İşlem, bir kaynak sağlayıcı için işlemlerin listesini döndürür. |
> | Eylem | Microsoft.RecoveryServices/register/action | Belirtilen kaynak sağlayıcısı için aboneliği kaydeder |
> | Eylem | microsoft.recoveryservices/Vaults/backupconfig/read | Yapılandırmayı döndürür kurtarma Hizmetleri kasası. |
> | Eylem | microsoft.recoveryservices/Vaults/backupconfig/write | Güncelleştirmeleri yapılandırma için kurtarma Hizmetleri kasası. |
> | Eylem | microsoft.recoveryservices/Vaults/backupEngines/read | Kasaya kayıtlı tüm yedekleme yönetimi sunucularının listesini döndürür. |
> | Eylem | microsoft.recoveryservices/Vaults/backupFabrics/backupProtectionIntent/delete | Bir yedekleme koruma hedefi Sil |
> | Eylem | microsoft.recoveryservices/Vaults/backupFabrics/backupProtectionIntent/read | Bir yedekleme koruma hedefi Al |
> | Eylem | microsoft.recoveryservices/Vaults/backupFabrics/backupProtectionIntent/write | Bir yedekleme koruma hedefi oluşturma |
> | Eylem | microsoft.recoveryservices/Vaults/backupFabrics/operationResults/read | İşlemin durumunu döndürür |
> | Eylem | microsoft.recoveryservices/Vaults/backupFabrics/protectableContainers/read | Korunabilir tüm kapsayıcıları alın |
> | Eylem | microsoft.recoveryservices/Vaults/backupFabrics/protectionContainers/delete | Kayıtlı kapsayıcıyı siler |
> | Eylem | microsoft.recoveryservices/Vaults/backupFabrics/protectionContainers/inquire/action | Bir kapsayıcı içindeki iş yükleri için sorgulama yapmak |
> | Eylem | microsoft.recoveryservices/Vaults/backupFabrics/protectionContainers/items/read | Bir kapsayıcıdaki tüm öğeleri al |
> | Eylem | microsoft.recoveryservices/Vaults/backupFabrics/protectionContainers/operationResults/read | Koruma Kapsayıcısı üzerinde gerçekleştirilen İşlemin sonuçlarını alır. |
> | Eylem | microsoft.recoveryservices/Vaults/backupFabrics/protectionContainers/protectedItems/backup/action | Korumalı Öğe için Yedekleme gerçekleştirir. |
> | Eylem | microsoft.recoveryservices/Vaults/backupFabrics/protectionContainers/protectedItems/delete | Korumalı öğeyi siler |
> | Eylem | microsoft.recoveryservices/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read | Korumalı Öğeler üzerinde Gerçekleştirilen İşlemin Sonuçlarını alın. |
> | Eylem | microsoft.recoveryservices/Vaults/backupFabrics/protectionContainers/protectedItems/operationsStatus/read | Korumalı Öğeler üzerinde Gerçekleştirilen İşlemin durumunu döndürür. |
> | Eylem | microsoft.recoveryservices/Vaults/backupFabrics/protectionContainers/protectedItems/read | Korumalı öğenin nesne ayrıntılarını döndürür |
> | Eylem | microsoft.recoveryservices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/provisionInstantItemRecovery/action | Korumalı öğe için anında öğe kurtarma sağla |
> | Eylem | microsoft.recoveryservices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read | Korumalı Öğeler için Kurtarma Noktalarını alın. |
> | Eylem | microsoft.recoveryservices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/restore/action | Korumalı Öğeler için Kurtarma Noktalarını geri yükleyin. |
> | Eylem | microsoft.recoveryservices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/revokeInstantItemRecovery/action | Korumalı öğe için anında öğe kurtarmayı iptal et |
> | Eylem | microsoft.recoveryservices/Vaults/backupFabrics/protectionContainers/protectedItems/write | Yedekleme korumalı öğesi oluştur |
> | Eylem | microsoft.recoveryservices/Vaults/backupFabrics/protectionContainers/read | Tüm kayıtlı kapsayıcıları döndürür |
> | Eylem | microsoft.recoveryservices/Vaults/backupFabrics/protectionContainers/write | Kayıtlı bir kapsayıcı oluşturur |
> | Eylem | microsoft.recoveryservices/Vaults/backupFabrics/refreshContainers/action | Kapsayıcı listesini yeniler |
> | Eylem | microsoft.recoveryservices/Vaults/backupJobs/cancel/action | İşi iptal et |
> | Eylem | microsoft.recoveryservices/Vaults/backupJobs/operationResults/read | İş İşleminin Sonucunu döndürür. |
> | Eylem | microsoft.recoveryservices/Vaults/backupJobs/read | Tüm iş nesnelerini döndürür |
> | Eylem | microsoft.recoveryservices/Vaults/backupJobsExport/action | Dışarı aktarma işleri |
> | Eylem | microsoft.recoveryservices/Vaults/backupOperationResults/read | Kurtarma Hizmetleri Kasası için Yedekleme işleminin Sonucunu döndürür. |
> | Eylem | microsoft.recoveryservices/Vaults/backupOperations/read | Yedekleme işlemi döndürür durumu için kurtarma Hizmetleri kasası. |
> | Eylem | microsoft.recoveryservices/Vaults/backupPolicies/delete | Koruma ilkesini siler |
> | Eylem | microsoft.recoveryservices/Vaults/backupPolicies/operationResults/read | İlke İşleminin Sonuçlarını alın. |
> | Eylem | microsoft.recoveryservices/Vaults/backupPolicies/operations/read | İlke işleminin durumunu alın. |
> | Eylem | microsoft.recoveryservices/Vaults/backupPolicies/read | Tüm koruma ilkelerini döndürür |
> | Eylem | microsoft.recoveryservices/Vaults/backupPolicies/write | Koruma ilkesi oluşturur |
> | Eylem | microsoft.recoveryservices/Vaults/backupProtectableItems/read | Tüm Korunabilir Öğelerin listesini döndürür. |
> | Eylem | microsoft.recoveryservices/Vaults/backupProtectedItems/read | Tüm Korumalı Öğelerin listesini döndürür. |
> | Eylem | microsoft.recoveryservices/Vaults/backupProtectionContainers/read | Aboneliğe ait tüm kapsayıcıları döndürür |
> | Eylem | microsoft.recoveryservices/Vaults/backupProtectionIntents/read | Tüm yedekleme koruma hedefleri listesinde |
> | Eylem | microsoft.recoveryservices/Vaults/backupSecurityPIN/action | Güvenlik PIN'i döndürür bilgi kurtarma Hizmetleri kasası. |
> | Eylem | microsoft.recoveryservices/Vaults/backupstorageconfig/read | Depolama yapılandırmasını döndürür kurtarma Hizmetleri kasası. |
> | Eylem | microsoft.recoveryservices/Vaults/backupstorageconfig/write | Güncelleştirmeleri depolama yapılandırması için kurtarma Hizmetleri kasası. |
> | Eylem | microsoft.recoveryservices/Vaults/backupUsageSummaries/read | Özetleri için bir kurtarma Hizmetleri korumalı öğeler ve korumalı sunucuların döndürür. |
> | Eylem | microsoft.recoveryservices/Vaults/backupValidateOperation/action | Korumalı öğe üzerinde işlem doğrulama |
> | Eylem | Microsoft.RecoveryServices/Vaults/certificates/write | Kaynak sertifikayı Güncelleştir işlemi, kaynak/kasa kimlik bilgisi sertifikasını güncelleştirir. |
> | Eylem | Microsoft.RecoveryServices/Vaults/delete | Kasayı Sil işlemi, 'vault' türündeki belirtilen Azure kaynağını siler |
> | Eylem | Microsoft.RecoveryServices/Vaults/extendedInformation/delete | Daha Fazla Bilgi Al işlemi, ?vault? türündeki Azure kaynağını temsil eden nesnenin Daha Fazla Bilgi değerini alır. |
> | Eylem | Microsoft.RecoveryServices/Vaults/extendedInformation/read | Daha Fazla Bilgi Al işlemi, ?vault? türündeki Azure kaynağını temsil eden nesnenin Daha Fazla Bilgi değerini alır. |
> | Eylem | Microsoft.RecoveryServices/Vaults/extendedInformation/write | Daha Fazla Bilgi Al işlemi, ?vault? türündeki Azure kaynağını temsil eden nesnenin Daha Fazla Bilgi değerini alır. |
> | Eylem | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Kurtarma Hizmetleri kasası için uyarıları alır. |
> | Eylem | Microsoft.RecoveryServices/Vaults/monitoringAlerts/write | Uyarıyı çözümler. |
> | Eylem | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/read | Kurtarma Hizmetleri kasası bildirim yapılandırmasını alır. |
> | Eylem | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/write | Kurtarma Hizmetleri kasasına e-posta bildirimlerini yapılandırır. |
> | Eylem | Microsoft.RecoveryServices/Vaults/read | Kasayı Al işlemi 'vault' türündeki Azure kaynağını temsil eden bir nesne alır. |
> | Eylem | Microsoft.RecoveryServices/Vaults/registeredIdentities/delete | Hizmet kapsayıcısının kaydını işlemi, bir kapsayıcının kaydını silmek için kullanılabilir. |
> | Eylem | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | Zaman uyumsuz olarak gönderilen işlemin sonucu ve işlem durumunu alma işlem işlemi kullanılabilir sonuçlarını Al |
> | Eylem | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Kapsayıcıları Al işlemi kullanılabilir bir kaynak için kayıtlı olan kapsayıcıları alın. |
> | Eylem | Microsoft.RecoveryServices/Vaults/registeredIdentities/write | Hizmet kapsayıcısını Kaydet işlemi, bir kapsayıcıyı kurtarma Hizmeti'ne kaydolmak için kullanılabilir. |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Herhangi bir uyarı ayarlarını okuma |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationAlertSettings/write | Oluşturma veya herhangi bir uyarı ayarlarını güncelleştirme |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationEvents/read | Tüm olayları okuyun |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/checkConsistency/action | Doku Tutarlılığını Denetler |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/delete | Tüm yapıları silin |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/deployProcessServerImage/action | İşlem sunucusu görüntüsünü Dağıt |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/migratetoaad/action | Fabric AAD'ye geçirin |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/read | Tüm yapıları okuyun |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/reassociateGateway/action | Ağ geçidini yeniden ilişkilendir |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/remove/action | Yapıyı Kaldır |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/renewcertificate/action | Yapı sertifikasını Yenile |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationLogicalNetworks/read | Mantıksal ağlar okuyun |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read | Herhangi bir ağa okuyun |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/delete | Tüm ağ eşlemeleri silme |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/read | Tüm ağ eşlemeleri okuyun |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/write | Tüm ağ eşlemeleri güncelle |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/discoverProtectableItem/action | Korunabilir öğeyi Bul |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/read | Tüm koruma kapsayıcıları okuyun |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/remove/action | Koruma kapsayıcısını Kaldır |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/delete | Geçiş öğeleri silin |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/migrate/action | Öğe geçirme |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/migrationRecoveryPoints/read | Tüm geçiş kurtarma noktaları okuyun |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/read | Tüm geçiş öğeleri okuma |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/testMigrate/action | Test geçişi |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/testMigrateCleanup/action | Test, temizleme geçirme |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/write | Tüm geçiş maddeleri oluştur veya güncelleştir |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectableItems/read | Tüm korunabilir öğelerin okuyun |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/addDisks/action | Disk ekleme |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/applyRecoveryPoint/action | Kurtarma noktası Uygula |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/delete | Tüm korumalı öğeleri sil |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/failoverCommit/action | Yük devretme işlemesi |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/plannedFailover/action | Planlı yük devretme |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/read | Tüm korumalı öğeleri okuma |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | Tüm çoğaltma kurtarma noktaları okuyun |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/remove/action | Korumalı öğeyi Kaldır |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/removeDisks/action | Diskleri çıkarın |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/repairReplication/action | Çoğaltmayı Onar |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/reProtect/action | Korumalı öğeyi yeniden koru |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/ResolveHealthErrors/action |  |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/submitFeedback/action | Geri bildirim gönder |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/targetComputeSizes/read | Hiçbir hedef işlem boyutları okuyun |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailover/action | Yük devretme testi |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailoverCleanup/action | Yük devretme temizlik testi |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/unplannedFailover/action | Yük devretme |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/updateMobilityService/action | Mobility hizmetini güncelleştirme |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/write | Tüm korumalı öğeleri güncelle |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/delete | Herhangi bir koruma kapsayıcısı eşlemelerini Sil |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/read | Herhangi bir koruma kapsayıcısı eşlemelerini okuyun |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/remove/action | Koruma kapsayıcısı eşlemesini Kaldır |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/write | Herhangi bir koruma kapsayıcısı eşlemelerini güncelle |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/switchprotection/action | Koruma değiştirme kapsayıcısı |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/write | Tüm koruma kapsayıcıları güncelle |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/delete | Herhangi bir kurtarma Hizmetleri sağlayıcıları Sil |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/read | Bir kurtarma Hizmetleri Sağlayıcısı'nı okuyun |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/refreshProvider/action | Sağlayıcı Yenile |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/remove/action | Kurtarma Hizmetleri sağlayıcısını Kaldır |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/write | Oluşturma veya güncelleştirme herhangi bir kurtarma Hizmetleri sağlayıcıları |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/read | Tüm depolama sınıflandırmaları okuyun |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/delete | Herhangi bir depolama sınıflandırması eşlemeleri silme |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/read | Herhangi bir depolama sınıflandırması eşlemeleri okuyun |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/write | Herhangi bir depolama sınıflandırması eşlemeleri güncelle |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/delete | Tüm vCenters Sil |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read | Tüm vCenters okuyun |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/write | Tüm vCenters güncelle |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/write | Tüm yapıları güncelle |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationJobs/cancel/action | İşi iptal et |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationJobs/read | Herhangi bir işi okuma |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationJobs/restart/action | İşi yeniden başlatın |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationJobs/resume/action | İşi sürdürme |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationMigrationItems/read | Tüm geçiş öğeleri okuma |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationNetworkMappings/read | Tüm ağ eşlemeleri okuyun |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationNetworks/read | Herhangi bir ağa okuyun |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationPolicies/delete | Tüm ilkeleri silin |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationPolicies/read | Tüm ilkeler okuyun |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationPolicies/write | Tüm ilkeler güncelle |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationProtectedItems/read | Tüm korumalı öğeleri okuma |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationProtectionContainerMappings/read | Herhangi bir koruma kapsayıcısı eşlemelerini okuyun |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationProtectionContainers/read | Tüm koruma kapsayıcıları okuyun |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/delete | Kurtarma planları Sil |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/failoverCommit/action | Yük devretme işlemesi kurtarma planı |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/plannedFailover/action | Planlı yük devretme kurtarma planı |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read | Kurtarma planları okuyun |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/reProtect/action | Kurtarma planını yeniden koru |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailover/action | Test yük devretme kurtarma planı |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailoverCleanup/action | Test Yük Devretmesini temizleme kurtarma planı |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/unplannedFailover/action | Yük devretme kurtarma planı |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/write | Tüm kurtarma planları oluşturma veya güncelleştirme |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationRecoveryServicesProviders/read | Bir kurtarma Hizmetleri Sağlayıcısı'nı okuyun |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationStorageClassificationMappings/read | Herhangi bir depolama sınıflandırması eşlemeleri okuyun |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationStorageClassifications/read | Tüm depolama sınıflandırmaları okuyun |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationSupportedOperatingSystems/read | Tüm okuma  |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationUsages/read | Herhangi bir kasa çoğaltma kullanımları okuyun |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationVaultHealth/read | Herhangi bir kasa çoğaltma durumunu okuma |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationVaultHealth/refresh/action | Kasa durumunu Yenile |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationVaultSettings/read | Tüm okuma  |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationVaultSettings/write | Oluşturun veya güncelleştirin  |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationvCenters/read | Tüm vCenters okuyun |
> | Eylem | microsoft.recoveryservices/Vaults/usages/read | Bir Kurtarma Hizmetleri Kasası için kullanım ayrıntılarını döndürür. |
> | Eylem | Microsoft.RecoveryServices/vaults/usages/read | Herhangi bir kasa kullanımları okuyun |
> | Eylem | Microsoft.RecoveryServices/Vaults/vaultTokens/read | Kasa düzeyi arka uç işlemlerine ait kasa Belirteci'ni almak için kasa belirteci işlemi kullanılabilir. |
> | Eylem | Microsoft.RecoveryServices/Vaults/write | Kasa Oluştur işlemi, 'vault' türünde bir Azure kaynağı oluşturur |

## <a name="microsoftrelay"></a>Microsoft.Relay

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Relay/checkNameAvailability/action | Seçili abonelik altında ad alanı kullanılabilirliğini denetler. |
> | Eylem | Microsoft.Relay/checkNamespaceAvailability/action | Seçili abonelik altında ad alanı kullanılabilirliğini denetler. Bu API kullanım dışıdır CheckNameAvailability Lütfen bunun yerine kullanın. |
> | Eylem | Microsoft.Relay/namespaces/authorizationRules/action | Güncelleştirmeleri Namespace yetkilendirme kuralı. Bu API kullanım dışıdır. Bunun yerine Namespace yetkilendirme kuralını güncelleştirmek için lütfen bir PUT çağrısı kullanın... Bu işlem API 2017-04-01 sürümünde desteklenmiyor. |
> | Eylem | Microsoft.Relay/namespaces/authorizationRules/delete | Namespace yetkilendirme kuralını silin. Varsayılan Namespace yetkilendirme kuralı silinemez.  |
> | Eylem | Microsoft.Relay/namespaces/authorizationRules/listkeys/action | Namespace bağlantı dizesini alın |
> | Eylem | Microsoft.Relay/namespaces/authorizationRules/read | Ad alanı yetkilendirme kuralları açıklamasının listesini alın. |
> | Eylem | Microsoft.Relay/namespaces/authorizationRules/regenerateKeys/action | Kaynağın birincil veya ikincil anahtarı yeniden oluştur |
> | Eylem | Microsoft.Relay/namespaces/authorizationRules/write | Namespace düzeyinde yetkilendirme kuralları oluşturun ve özelliklerini güncelleştirin. Yetkilendirme kuralları erişim hakları, birincil ve ikincil anahtarlar güncelleştirilebilir. |
> | Eylem | Microsoft.Relay/namespaces/Delete | Namespace kaynağı silme |
> | Eylem | Microsoft.Relay/namespaces/disasterRecoveryConfigs/authorizationRules/listkeys/action | Yetkilendirme kuralları anahtarlarını olağanüstü durum kurtarma birincil ad alanı için alır. |
> | Eylem | Microsoft.Relay/namespaces/disasterRecoveryConfigs/authorizationRules/read | Olağanüstü durum kurtarma birincil Namespace'nın yetkilendirme kurallarını Al |
> | Eylem | Microsoft.Relay/namespaces/disasterRecoveryConfigs/breakPairing/action | Olağanüstü durum kurtarma devre dışı bırakır ve birincil değişiklikleri ikincil ad alanına çoğaltmayı durdurur. |
> | Eylem | Microsoft.Relay/namespaces/disasterrecoveryconfigs/checkNameAvailability/action | Seçili abonelik altında ad alanı diğer ad kullanılabilirliğini denetler. |
> | Eylem | Microsoft.Relay/namespaces/disasterRecoveryConfigs/delete | Ad alanıyla ilişkili olağanüstü durum kurtarma yapılandırmasını siler. Bu işlem yalnızca birincil ad alanı aracılığıyla çağrılabilir. |
> | Eylem | Microsoft.Relay/namespaces/disasterRecoveryConfigs/failover/action | Bir GEO DR Yük Devretmesini çağırır ve ad alanı diğer adını ikincil ad alanına işaret edecek şekilde yeniden yapılandırır. |
> | Eylem | Microsoft.Relay/namespaces/disasterRecoveryConfigs/read | Ad alanıyla ilişkili olağanüstü durum kurtarma yapılandırmasını alır. |
> | Eylem | Microsoft.Relay/namespaces/disasterRecoveryConfigs/write | Oluşturur veya ad alanıyla ilişkili olağanüstü durum kurtarma yapılandırmasını güncelleştirir. |
> | Eylem | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/action | HybridConnection güncelleştirmek için işlem. Bu işlem API 2017-04-01 sürümünde desteklenmiyor. Yetkilendirme kuralları. Yetkilendirme kuralı'nı güncelleştirmek için lütfen bir PUT çağrısı kullanın. |
> | Eylem | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/delete | HybridConnection yetkilendirme kurallarını silme işlemi |
> | Eylem | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/listkeys/action | HybridConnection bağlantı dizesini alın |
> | Eylem | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/read |  HybridConnection yetkilendirme kurallarının listesini alın |
> | Eylem | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/regeneratekeys/action | Kaynağın birincil veya ikincil anahtarı yeniden oluştur |
> | Eylem | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/write | HybridConnection yetkilendirme kuralları oluşturun ve özelliklerini güncelleştirin. Yetkilendirme kuralları erişim hakları güncelleştirilebilir. |
> | Eylem | Microsoft.Relay/namespaces/HybridConnections/Delete | HybridConnection kaynak silme işlemi |
> | Eylem | Microsoft.Relay/namespaces/HybridConnections/read | HybridConnection kaynak açıklamalarının listesini alın |
> | Eylem | Microsoft.Relay/namespaces/HybridConnections/write | Oluşturma veya güncelleştirme HybridConnection özellikleri. |
> | Eylem | Microsoft.Relay/namespaces/messagingPlan/read | Mesajlaşma planı için bir ad alanını alır.<br>Bu API kullanım dışıdır.<br>Namespace kaynak sonraki API sürümlerinde (üst) MessagingPlan kaynağı aracılığıyla kullanıma sunulan özellikler taşınan...<br>Bu işlem API 2017-04-01 sürümünde desteklenmiyor. |
> | Eylem | Microsoft.Relay/namespaces/messagingPlan/write | Mesajlaşma planı için bir ad alanı güncelleştirir.<br>Bu API kullanım dışıdır.<br>Namespace kaynak sonraki API sürümlerinde (üst) MessagingPlan kaynağı aracılığıyla kullanıma sunulan özellikler taşınan...<br>Bu işlem API 2017-04-01 sürümünde desteklenmiyor. |
> | Eylem | Microsoft.Relay/namespaces/operationresults/read | Namespace işlemin durumunu alın |
> | Eylem | Microsoft.Relay/namespaces/providers/Microsoft.Insights/metricDefinitions/read | Namespace ölçümleri kaynak açıklamalarının listesini alın |
> | Eylem | Microsoft.Relay/namespaces/read | Namespace kaynak açıklamasının listesini alır |
> | Eylem | Microsoft.Relay/namespaces/removeAcsNamepsace/action | ACS ad alanını Kaldır |
> | Eylem | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/action | WcfRelay güncelleştirmek için işlem. Bu işlem API 2017-04-01 sürümünde desteklenmiyor. Yetkilendirme kuralları. Yetkilendirme kuralı'nı güncelleştirmek için lütfen bir PUT çağrısı kullanın. |
> | Eylem | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/delete | WcfRelay yetkilendirme kurallarını silme işlemi |
> | Eylem | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/listkeys/action | WcfRelay bağlantı dizesini alın |
> | Eylem | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/read |  WcfRelay yetkilendirme kurallarının listesini alın |
> | Eylem | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/regeneratekeys/action | Kaynağın birincil veya ikincil anahtarı yeniden oluştur |
> | Eylem | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/write | WcfRelay yetkilendirme kuralları oluşturun ve özelliklerini güncelleştirin. Yetkilendirme kuralları erişim hakları güncelleştirilebilir. |
> | Eylem | Microsoft.Relay/namespaces/WcfRelays/Delete | WcfRelay kaynak silme işlemi |
> | Eylem | Microsoft.Relay/namespaces/WcfRelays/read | WcfRelay kaynak açıklamalarının listesini alın |
> | Eylem | Microsoft.Relay/namespaces/WcfRelays/write | Oluşturma veya güncelleştirme WcfRelay özellikleri. |
> | Eylem | Microsoft.Relay/namespaces/write | Namespace kaynağı oluşturun ve özelliklerini güncelleştirin. Etiketleri ve kapasitesi Namespace hangi güncelleştirilebilecek özelliklerdir. |
> | Eylem | Microsoft.Relay/operations/read | İşlemleri Al |
> | Eylem | Microsoft.Relay/register/action | Geçiş kaynak sağlayıcısı için aboneliği kaydeder ve geçiş kaynaklarının oluşturulmasını sağlar |
> | Eylem | Microsoft.Relay/unregister/action | Geçiş kaynak sağlayıcısı için aboneliği kaydeder ve geçiş kaynaklarının oluşturulmasını sağlar |

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.ResourceHealth/AvailabilityStatuses/current/read | Belirtilen kaynak için kullanılabilirlik durumunu alır |
> | Eylem | Microsoft.ResourceHealth/AvailabilityStatuses/read | Kullanılabilirlik durumlarını tüm kaynaklar için belirtilen kapsamda alır. |
> | Eylem | Microsoft.ResourceHealth/events/read | Get hizmet durumu olayları için belirtilen aboneliği |
> | Eylem | Microsoft.Resourcehealth/healthevent/action | Belirtilen kaynaktaki sistem durumu değişikliğini gösterir |
> | Eylem | Microsoft.Resourcehealth/healthevent/Activated/action | Belirtilen kaynaktaki sistem durumu değişikliğini gösterir |
> | Eylem | Microsoft.Resourcehealth/healthevent/InProgress/action | Belirtilen kaynaktaki sistem durumu değişikliğini gösterir |
> | Eylem | Microsoft.Resourcehealth/healthevent/Pending/action | Belirtilen kaynaktaki sistem durumu değişikliğini gösterir |
> | Eylem | Microsoft.Resourcehealth/healthevent/Resolved/action | Belirtilen kaynaktaki sistem durumu değişikliğini gösterir |
> | Eylem | Microsoft.Resourcehealth/healthevent/Updated/action | Belirtilen kaynaktaki sistem durumu değişikliğini gösterir |
> | Eylem | Microsoft.ResourceHealth/impactedResources/read | Seçili abonelik için etkilenen kaynaklar edinin |
> | Eylem | Microsoft.ResourceHealth/metadata/read | Meta verileri alır |
> | Eylem | Microsoft.ResourceHealth/Operations/read | Microsoft ResourceHealth için aboneliği işlemleri Al |
> | Eylem | Microsoft.ResourceHealth/register/action | Microsoft ResourceHealth için aboneliği kaydeder |
> | Eylem | Microsoft.ResourceHealth/unregister/action | Microsoft ResourceHealth için aboneliği kaydını siler |

## <a name="microsoftresources"></a>Microsoft.Resources

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Resources/checkPolicyCompliance/action | Kaynak ilkelerine karşı belirli bir kaynağın uyumluluk durumunu denetleyin. |
> | Eylem | Microsoft.Resources/checkResourceName/action | Kaynak adının geçerliliğini denetle. |
> | Eylem | Microsoft.Resources/deployments/cancel/action | Bir dağıtımı iptal eder. |
> | Eylem | Microsoft.Resources/deployments/delete | Bir dağıtımı siler. |
> | Eylem | Microsoft.Resources/deployments/operations/read | Alır veya dağıtım işlemlerini listeler. |
> | Eylem | Microsoft.Resources/deployments/read | Dağıtımları alır veya listeler. |
> | Eylem | Microsoft.Resources/deployments/validate/action | Bir dağıtımı doğrular. |
> | Eylem | Microsoft.Resources/deployments/whatIf/action | Şablon dağıtımı değişikliklerini tahmin eder. |
> | Eylem | Microsoft.Resources/deployments/write | Oluşturur veya bir dağıtım güncelleştirir. |
> | Eylem | Microsoft.Resources/links/delete | Bir kaynak bağlantısını siler. |
> | Eylem | Microsoft.Resources/links/read | Alır veya kaynak bağlantıları listeler. |
> | Eylem | Microsoft.Resources/links/write | Oluşturur veya bir kaynak bağlantısını güncelleştirir. |
> | Eylem | Microsoft.Resources/marketplace/purchase/action | Marketten bir kaynak satın alır. |
> | Eylem | Microsoft.Resources/providers/read | Sağlayıcı'nın listesini alın. |
> | Eylem | Microsoft.Resources/resources/read | Filtrelere göre kaynak listesini alın. |
> | Eylem | Microsoft.Resources/subscriptions/locations/read | Desteklenen konumların listesini alır. |
> | Eylem | Microsoft.Resources/subscriptions/operationresults/read | Abonelik işlem sonuçlarını al. |
> | Eylem | Microsoft.Resources/subscriptions/providers/read | Alır veya kaynak sağlayıcılarını listeler. |
> | Eylem | Microsoft.Resources/subscriptions/read | Aboneliklerin listesini alır. |
> | Eylem | Microsoft.Resources/subscriptions/resourceGroups/delete | Bir kaynak grubunu ve tüm kaynaklarını siler. |
> | Eylem | Microsoft.Resources/subscriptions/resourcegroups/deployments/operations/read | Alır veya dağıtım işlemlerini listeler. |
> | Eylem | Microsoft.Resources/subscriptions/resourcegroups/deployments/operationstatuses/read | Alır veya dağıtım işlemi durumlarını listeler. |
> | Eylem | Microsoft.Resources/subscriptions/resourcegroups/deployments/read | Dağıtımları alır veya listeler. |
> | Eylem | Microsoft.Resources/subscriptions/resourcegroups/deployments/write | Oluşturur veya bir dağıtım güncelleştirir. |
> | Eylem | Microsoft.Resources/subscriptions/resourceGroups/moveResources/action | Kaynakları bir kaynak grubundan diğerine taşır. |
> | Eylem | Microsoft.Resources/subscriptions/resourceGroups/read | Alır veya kaynak gruplarını listeler. |
> | Eylem | Microsoft.Resources/subscriptions/resourcegroups/resources/read | Kaynaklar için kaynak grubunu alır. |
> | Eylem | Microsoft.Resources/subscriptions/resourceGroups/validateMoveResources/action | Taşıma işlemini bir kaynak grubundan diğerine doğrulayın. |
> | Eylem | Microsoft.Resources/subscriptions/resourceGroups/write | Oluşturur veya bir kaynak grubu güncelleştirir. |
> | Eylem | Microsoft.Resources/subscriptions/resources/read | Bir aboneliğin kaynaklarını alır. |
> | Eylem | Microsoft.Resources/subscriptions/tagNames/delete | Bir abonelik etiketini siler. |
> | Eylem | Microsoft.Resources/subscriptions/tagNames/read | Alır veya abonelik etiketlerini listeler. |
> | Eylem | Microsoft.Resources/subscriptions/tagNames/tagValues/delete | Bir abonelik etiketi değerini siler. |
> | Eylem | Microsoft.Resources/subscriptions/tagNames/tagValues/read | Alır veya abonelik etiketi değerlerini listeler. |
> | Eylem | Microsoft.Resources/subscriptions/tagNames/tagValues/write | Bir abonelik etiketi değeri ekler. |
> | Eylem | Microsoft.Resources/subscriptions/tagNames/write | Bir abonelik etiketi ekler. |
> | Eylem | Microsoft.Resources/tags/delete | Kaynaktaki tüm etiketleri kaldırır. |
> | Eylem | Microsoft.Resources/tags/read | Tüm etiketleri bir kaynakta alır. |
> | Eylem | Microsoft.Resources/tags/write | Kaynak etiketleri değiştirmek veya yeni bir etiket kümesine sahip var olan etiketler birleştirme veya var olan etiketleri kaldırma güncelleştirir. |
> | Eylem | Microsoft.Resources/tenants/read | Kiracı listesini alır. |

## <a name="microsoftscheduler"></a>Microsoft.Scheduler

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Scheduler/jobcollections/delete | İş koleksiyonunu siler. |
> | Eylem | Microsoft.Scheduler/jobcollections/disable/action | İş koleksiyonunu devre dışı bırakır. |
> | Eylem | Microsoft.Scheduler/jobcollections/enable/action | İş koleksiyonunu etkinleştirir. |
> | Eylem | Microsoft.Scheduler/jobcollections/jobs/delete | İşi siler. |
> | Eylem | Microsoft.Scheduler/jobcollections/jobs/generateLogicAppDefinition/action | Bir Scheduler işini temel alarak mantıksal uygulama tanımı oluşturur. |
> | Eylem | Microsoft.Scheduler/jobcollections/jobs/jobhistories/read | İş geçmişini alır. |
> | Eylem | Microsoft.Scheduler/jobcollections/jobs/read | İşi alır. |
> | Eylem | Microsoft.Scheduler/jobcollections/jobs/run/action | İşi çalıştırır. |
> | Eylem | Microsoft.Scheduler/jobcollections/jobs/write | İş oluşturur veya güncelleştirir. |
> | Eylem | Microsoft.Scheduler/jobcollections/read | İş koleksiyonu Al |
> | Eylem | Microsoft.Scheduler/jobcollections/write | Oluşturur veya güncelleştirir iş koleksiyonu. |

## <a name="microsoftsearch"></a>Microsoft.Search

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Search/checkNameAvailability/action | Hizmet adının kullanılabilirliğini denetler. |
> | Eylem | Microsoft.Search/operations/read | Tüm kullanılabilir işlemleri Microsoft.Search sağlayıcısının listeler. |
> | Eylem | Microsoft.Search/register/action | Arama kaynak sağlayıcısı için aboneliği kaydeder ve arama hizmetleri oluşturmanıza olanak tanır. |
> | Eylem | Microsoft.Search/searchServices/createQueryKey/action | Sorgu anahtarı oluşturur. |
> | Eylem | Microsoft.Search/searchServices/delete | Arama hizmeti siler. |
> | Eylem | Microsoft.Search/searchServices/deleteQueryKey/delete | Sorgu anahtarını siler. |
> | Eylem | Microsoft.Search/searchServices/listAdminKeys/action | Yönetici anahtarları okur. |
> | Eylem | Microsoft.Search/searchServices/listQueryKeys/read | Belirli bir Azure Search hizmeti için sorgu API anahtarları listesini döndürür. |
> | Eylem | Microsoft.Search/searchServices/read | Arama hizmeti okur. |
> | Eylem | Microsoft.Search/searchServices/regenerateAdminKey/action | Yönetici anahtarını yeniden oluşturur. |
> | Eylem | Microsoft.Search/searchServices/start/action | Arama hizmetini başlatır. |
> | Eylem | Microsoft.Search/searchServices/stop/action | Arama hizmetini durdurur. |
> | Eylem | Microsoft.Search/searchServices/write | Oluşturur veya arama hizmetinin güncelleştirir. |

## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Security/adaptiveNetworkHardenings/enforce/action | Belirtilen ağ güvenlik grupları üzerinde eşleşen güvenlik kuralları oluşturarak kurallar sağlamlaştırma verilen trafiği zorunlu |
> | Eylem | Microsoft.Security/adaptiveNetworkHardenings/read | Korumalı kaynak Azure alır Uyarlamalı ağ sağlamlaştırma önerileri |
> | Eylem | Microsoft.Security/advancedThreatProtectionSettings/read | Kaynak için Gelişmiş tehdit koruması ayarları alır |
> | Eylem | Microsoft.Security/advancedThreatProtectionSettings/write | Kaynak için Gelişmiş tehdit koruması ayarları güncelleştirir |
> | Eylem | Microsoft.Security/alerts/read | Tüm kullanılabilir güvenlik uyarıları alır |
> | Eylem | Microsoft.Security/applicationWhitelistings/read | Uygulama whitelistings alır |
> | Eylem | Microsoft.Security/applicationWhitelistings/write | Yeni bir uygulama beyaz listesini oluşturur veya mevcut olanı güncelleştirir |
> | Eylem | Microsoft.Security/complianceResults/read | Kaynak için Uyumluluk sonuçlarını alır |
> | Eylem | Microsoft.Security/informationProtectionPolicies/read | Bilgi koruma ilkeleri için kaynak alır. |
> | Eylem | Microsoft.Security/informationProtectionPolicies/write | Kaynak için bilgi koruma ilkelerine güncelleştirir |
> | Eylem | Microsoft.Security/locations/alerts/activate/action | Bir güvenlik uyarısı etkinleştir |
> | Eylem | Microsoft.Security/locations/alerts/dismiss/action | Bir güvenlik uyarısını kapatmanın |
> | Eylem | Microsoft.Security/locations/alerts/read | Tüm kullanılabilir güvenlik uyarıları alır |
> | Eylem | Microsoft.Security/locations/jitNetworkAccessPolicies/delete | Tam zamanında ağ erişimi ilkesi siler |
> | Eylem | Microsoft.Security/locations/jitNetworkAccessPolicies/initiate/action | Tam zamanında ağ erişimi ilke isteği başlatır |
> | Eylem | Microsoft.Security/locations/jitNetworkAccessPolicies/read | Just-ın-time ağ erişim ilkelerini alır |
> | Eylem | Microsoft.Security/locations/jitNetworkAccessPolicies/write | Yeni bir tam zamanında ağ erişimi ilkesi oluşturur veya mevcut olanı güncelleştirir |
> | Eylem | Microsoft.Security/locations/read | Güvenlik veri konumu alır |
> | Eylem | Microsoft.Security/locations/tasks/activate/action | Bir güvenlik önerisi etkinleştir |
> | Eylem | Microsoft.Security/locations/tasks/dismiss/action | Bir güvenlik önerisi Kapat |
> | Eylem | Microsoft.Security/locations/tasks/read | Tüm kullanılabilir güvenlik önerilerini alır |
> | Eylem | Microsoft.Security/locations/tasks/resolve/action | Bir güvenlik önerisi çözümleyin |
> | Eylem | Microsoft.Security/locations/tasks/start/action | Bir güvenlik önerisi Başlat |
> | Eylem | Microsoft.Security/policies/read | Güvenlik ilkesini alır |
> | Eylem | Microsoft.Security/policies/write | Güvenlik İlkesi güncelleştirmeleri |
> | Eylem | Microsoft.Security/pricings/delete | Kapsam için fiyatlandırma ayarlarını siler |
> | Eylem | Microsoft.Security/pricings/read | Kapsam için fiyatlandırma ayarlarını alır. |
> | Eylem | Microsoft.Security/pricings/write | Kapsam için fiyatlandırma ayarlarını güncelleştirir |
> | Eylem | Microsoft.Security/register/action | Azure Güvenlik Merkezi için aboneliği kaydeder |
> | Eylem | Microsoft.Security/securityContacts/delete | Güvenlik ilgili kişi siler |
> | Eylem | Microsoft.Security/securityContacts/read | Güvenlik ilgili kişi alır |
> | Eylem | Microsoft.Security/securityContacts/write | Güvenlik ilgili kişi güncelleştirir |
> | Eylem | Microsoft.Security/securitySolutions/delete | Bir güvenlik çözümü siler |
> | Eylem | Microsoft.Security/securitySolutions/read | Güvenlik çözümleri alır |
> | Eylem | Microsoft.Security/securitySolutions/write | Yeni bir güvenlik çözümü oluşturur veya mevcut olanı güncelleştirir |
> | Eylem | Microsoft.Security/securitySolutionsReferenceData/read | Güvenlik çözümleri, başvuru verileri alır |
> | Eylem | Microsoft.Security/securityStatuses/read | Güvenlik Azure kaynakları için sağlık durumlarını alır. |
> | Eylem | Microsoft.Security/securityStatusesSummaries/read | Güvenlik kapsamı için durum özetleri alır. |
> | Eylem | Microsoft.Security/settings/read | Kapsam için kullanılacak ayarları alır |
> | Eylem | Microsoft.Security/settings/write | Kapsamı ayarlarını güncelleştirir |
> | Eylem | Microsoft.Security/tasks/read | Tüm kullanılabilir güvenlik önerilerini alır |
> | Eylem | Microsoft.Security/unregister/action | Azure Güvenlik Merkezi'nden abonelik kaydını siler |
> | Eylem | Microsoft.Security/webApplicationFirewalls/delete | Bir web uygulaması güvenlik duvarı siler |
> | Eylem | Microsoft.Security/webApplicationFirewalls/read | Web uygulaması güvenlik duvarları alır |
> | Eylem | Microsoft.Security/webApplicationFirewalls/write | Yeni bir web uygulaması güvenlik duvarı oluşturur veya mevcut olanı güncelleştirir |
> | Eylem | Microsoft.Security/workspaceSettings/connect/action | Çalışma alanı ayarlarını yeniden bağlantı ayarlarını değiştirme |
> | Eylem | Microsoft.Security/workspaceSettings/delete | Çalışma alanı ayarlarını siler |
> | Eylem | Microsoft.Security/workspaceSettings/read | Çalışma alanı ayarlarını alır. |
> | Eylem | Microsoft.Security/workspaceSettings/write | Güncelleştirmeler çalışma alanı ayarları |

## <a name="microsoftsecuritygraph"></a>Microsoft.SecurityGraph

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.SecurityGraph/diagnosticsettings/delete | Tanılama ayarını siliniyor |
> | Eylem | Microsoft.SecurityGraph/diagnosticsettings/read | Tanılama ayarını okuma |
> | Eylem | Microsoft.SecurityGraph/diagnosticsettings/write | Tanılama ayarını yazma |
> | Eylem | Microsoft.SecurityGraph/diagnosticsettingscategories/read | Tanılama ayarı kategorilerini okuma |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.ServiceBus/checkNameAvailability/action | Seçili abonelik altında ad alanı kullanılabilirliğini denetler. |
> | Eylem | Microsoft.ServiceBus/checkNamespaceAvailability/action | Seçili abonelik altında ad alanı kullanılabilirliğini denetler. Bu API kullanım dışıdır CheckNameAvailability Lütfen bunun yerine kullanın. |
> | Eylem | Microsoft.ServiceBus/locations/deleteVirtualNetworkOrSubnets/action | Belirtilen sanal ağ için ServiceBus kaynak Sağlayıcısı'nda sanal ağ kuralları siler |
> | Eylem | Microsoft.ServiceBus/namespaces/authorizationRules/action | Güncelleştirmeleri Namespace yetkilendirme kuralı. Bu API kullanım dışıdır. Bunun yerine Namespace yetkilendirme kuralını güncelleştirmek için lütfen bir PUT çağrısı kullanın... Bu işlem API 2017-04-01 sürümünde desteklenmiyor. |
> | Eylem | Microsoft.ServiceBus/namespaces/authorizationRules/delete | Namespace yetkilendirme kuralını silin. Varsayılan Namespace yetkilendirme kuralı silinemez.  |
> | Eylem | Microsoft.ServiceBus/namespaces/authorizationRules/listkeys/action | Namespace bağlantı dizesini alın |
> | Eylem | Microsoft.ServiceBus/namespaces/authorizationRules/read | Ad alanı yetkilendirme kuralları açıklamasının listesini alın. |
> | Eylem | Microsoft.ServiceBus/namespaces/authorizationRules/regenerateKeys/action | Kaynağın birincil veya ikincil anahtarı yeniden oluştur |
> | Eylem | Microsoft.ServiceBus/namespaces/authorizationRules/write | Namespace düzeyinde yetkilendirme kuralları oluşturun ve özelliklerini güncelleştirin. Yetkilendirme kuralları erişim hakları, birincil ve ikincil anahtarlar güncelleştirilebilir. |
> | Eylem | Microsoft.ServiceBus/namespaces/Delete | Namespace kaynağı silme |
> | Eylem | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/authorizationRules/listkeys/action | Yetkilendirme kuralları anahtarlarını olağanüstü durum kurtarma birincil ad alanı için alır. |
> | Eylem | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/authorizationRules/read | Olağanüstü durum kurtarma birincil Namespace'nın yetkilendirme kurallarını Al |
> | Eylem | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/breakPairing/action | Olağanüstü durum kurtarma devre dışı bırakır ve birincil değişiklikleri ikincil ad alanına çoğaltmayı durdurur. |
> | Eylem | Microsoft.ServiceBus/namespaces/disasterrecoveryconfigs/checkNameAvailability/action | Seçili abonelik altında ad alanı diğer ad kullanılabilirliğini denetler. |
> | Eylem | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/delete | Ad alanıyla ilişkili olağanüstü durum kurtarma yapılandırmasını siler. Bu işlem yalnızca birincil ad alanı aracılığıyla çağrılabilir. |
> | Eylem | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/failover/action | Bir GEO DR Yük Devretmesini çağırır ve ad alanı diğer adını ikincil ad alanına işaret edecek şekilde yeniden yapılandırır. |
> | Eylem | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/read | Ad alanıyla ilişkili olağanüstü durum kurtarma yapılandırmasını alır. |
> | Eylem | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/write | Oluşturur veya ad alanıyla ilişkili olağanüstü durum kurtarma yapılandırmasını güncelleştirir. |
> | Eylem | Microsoft.ServiceBus/namespaces/eventGridFilters/delete | Ad alanıyla ilişkili olay Kılavuzu filtresini siler. |
> | Eylem | Microsoft.ServiceBus/namespaces/eventGridFilters/read | Ad alanıyla ilişkili olay Kılavuzu filtresini alır. |
> | Eylem | Microsoft.ServiceBus/namespaces/eventGridFilters/write | Oluşturur veya ad alanıyla ilişkili olay Kılavuzu filtresini güncelleştirir. |
> | Eylem | Microsoft.ServiceBus/namespaces/eventhubs/read | EventHub kaynak açıklamalarının listesini alın |
> | Eylem | Microsoft.ServiceBus/namespaces/ipFilterRules/delete | IP Filtresi kaynağı silme |
> | Eylem | Microsoft.ServiceBus/namespaces/ipFilterRules/read | IP Filtresi kaynağını Al |
> | Eylem | Microsoft.ServiceBus/namespaces/ipFilterRules/write | IP Filtresi kaynak oluştur |
> | DataAction | Microsoft.ServiceBus/namespaces/messages/browse/action | Mesaj Gözat |
> | DataAction | Microsoft.ServiceBus/namespaces/messages/defer/action | İleti erteleme |
> | DataAction | Microsoft.ServiceBus/namespaces/messages/receive/action | İleti alma |
> | DataAction | Microsoft.ServiceBus/namespaces/messages/schedule/action | Shedule iletileri |
> | DataAction | Microsoft.ServiceBus/namespaces/messages/send/action | İleti gönderme |
> | DataAction | Microsoft.ServiceBus/namespaces/messages/setstate/action | Oturum durumunu ayarla |
> | Eylem | Microsoft.ServiceBus/namespaces/messagingPlan/read | Mesajlaşma planı için bir ad alanını alır.<br>Bu API kullanım dışıdır.<br>Namespace kaynak sonraki API sürümlerinde (üst) MessagingPlan kaynağı aracılığıyla kullanıma sunulan özellikler taşınan...<br>Bu işlem API 2017-04-01 sürümünde desteklenmiyor. |
> | Eylem | Microsoft.ServiceBus/namespaces/messagingPlan/write | Mesajlaşma planı için bir ad alanı güncelleştirir.<br>Bu API kullanım dışıdır.<br>Namespace kaynak sonraki API sürümlerinde (üst) MessagingPlan kaynağı aracılığıyla kullanıma sunulan özellikler taşınan...<br>Bu işlem API 2017-04-01 sürümünde desteklenmiyor. |
> | Eylem | Microsoft.ServiceBus/namespaces/migrate/action | Ad alanı geçiş işlemi |
> | Eylem | Microsoft.ServiceBus/namespaces/migrationConfigurations/delete | Geçiş yapılandırmasını siler. |
> | Eylem | Microsoft.ServiceBus/namespaces/migrationConfigurations/read | Geçiş ve bekleyen çoğaltma işlemleri durumunu gösteren bir geçiş yapılandırmasını alır |
> | Eylem | Microsoft.ServiceBus/namespaces/migrationConfigurations/revert/action | Premium ad alanı geçiş standart döner |
> | Eylem | Microsoft.ServiceBus/namespaces/migrationConfigurations/upgrade/action | Geçiş tamamlandıktan ve standart katmandan premium ad alanı için eşitleme kaynakları durdurur premium ad alanına standart ad alanı ile ilişkili DNS atar |
> | Eylem | Microsoft.ServiceBus/namespaces/migrationConfigurations/write | Oluşturur veya güncelleştirmeleri geçiş yapılandırması. Bu standart kaynaklardan premium ad alanı için eşitlemeyi başlatır |
> | Eylem | Microsoft.ServiceBus/namespaces/networkrulesets/delete | Sanal ağ kuralı kaynağı silme |
> | Eylem | Microsoft.ServiceBus/namespaces/networkrulesets/read | NetworkRuleSet kaynak alır |
> | Eylem | Microsoft.ServiceBus/namespaces/networkrulesets/write | Sanal ağ kuralı kaynağı oluşturma |
> | Eylem | Microsoft.ServiceBus/namespaces/operationresults/read | Namespace işlemin durumunu alın |
> | Eylem | Microsoft.ServiceBus/namespaces/providers/Microsoft.Insights/diagnosticSettings/read | Namespace tanılama ayarlarını kaynak açıklamalarının listesini alın |
> | Eylem | Microsoft.ServiceBus/namespaces/providers/Microsoft.Insights/diagnosticSettings/write | Namespace tanılama ayarlarını kaynak açıklamalarının listesini alın |
> | Eylem | Microsoft.ServiceBus/namespaces/providers/Microsoft.Insights/logDefinitions/read | Namespace günlükleri kaynak açıklamalarının listesini alın |
> | Eylem | Microsoft.ServiceBus/namespaces/providers/Microsoft.Insights/metricDefinitions/read | Namespace ölçümleri kaynak açıklamalarının listesini alın |
> | Eylem | Microsoft.ServiceBus/namespaces/queues/authorizationRules/action | Kuyruğu güncelleştirmek için işlem. Bu işlem API 2017-04-01 sürümünde desteklenmiyor. Yetkilendirme kuralları. Yetkilendirme kuralı'nı güncelleştirmek için lütfen bir PUT çağrısı kullanın. |
> | Eylem | Microsoft.ServiceBus/namespaces/queues/authorizationRules/delete | Kuyruk yetkilendirme kurallarını silme işlemi |
> | Eylem | Microsoft.ServiceBus/namespaces/queues/authorizationRules/listkeys/action | Kuyruk bağlantı dizesini alın |
> | Eylem | Microsoft.ServiceBus/namespaces/queues/authorizationRules/read |  Kuyruk yetkilendirme kurallarının listesini alın |
> | Eylem | Microsoft.ServiceBus/namespaces/queues/authorizationRules/regenerateKeys/action | Kaynağın birincil veya ikincil anahtarı yeniden oluştur |
> | Eylem | Microsoft.ServiceBus/namespaces/queues/authorizationRules/write | Kuyruk yetkilendirme kuralları oluşturun ve özelliklerini güncelleştirin. Yetkilendirme kuralları erişim hakları güncelleştirilebilir. |
> | Eylem | Microsoft.ServiceBus/namespaces/queues/Delete | Kuyruk kaynak silme işlemi |
> | Eylem | Microsoft.ServiceBus/namespaces/queues/read | Kuyruk kaynak açıklamalarının listesini alın |
> | Eylem | Microsoft.ServiceBus/namespaces/queues/write | Oluşturma veya güncelleştirme sıra özellikleri. |
> | Eylem | Microsoft.ServiceBus/namespaces/read | Namespace kaynak açıklamasının listesini alır |
> | Eylem | Microsoft.ServiceBus/namespaces/removeAcsNamepsace/action | ACS ad alanını Kaldır |
> | Eylem | Microsoft.ServiceBus/namespaces/topics/authorizationRules/action | Konu güncelleştirmek için işlem. Bu işlem API 2017-04-01 sürümünde desteklenmiyor. Yetkilendirme kuralları. Yetkilendirme kuralı'nı güncelleştirmek için lütfen bir PUT çağrısı kullanın. |
> | Eylem | Microsoft.ServiceBus/namespaces/topics/authorizationRules/delete | Konu yetkilendirme kurallarını silme işlemi |
> | Eylem | Microsoft.ServiceBus/namespaces/topics/authorizationRules/listkeys/action | Konu bağlantı dizesini alın |
> | Eylem | Microsoft.ServiceBus/namespaces/topics/authorizationRules/read |  Konu yetkilendirme kurallarının listesini alın |
> | Eylem | Microsoft.ServiceBus/namespaces/topics/authorizationRules/regenerateKeys/action | Kaynağın birincil veya ikincil anahtarı yeniden oluştur |
> | Eylem | Microsoft.ServiceBus/namespaces/topics/authorizationRules/write | Konu yetkilendirme kuralları oluşturun ve özelliklerini güncelleştirin. Yetkilendirme kuralları erişim hakları güncelleştirilebilir. |
> | Eylem | Microsoft.ServiceBus/namespaces/topics/Delete | Konu kaynak silme işlemi |
> | Eylem | Microsoft.ServiceBus/namespaces/topics/read | Konu kaynak açıklamalarının listesini alın |
> | Eylem | Microsoft.ServiceBus/namespaces/topics/subscriptions/Delete | TopicSubscription kaynak silme işlemi |
> | Eylem | Microsoft.ServiceBus/namespaces/topics/subscriptions/read | TopicSubscription kaynak açıklamalarının listesini alın |
> | Eylem | Microsoft.ServiceBus/namespaces/topics/subscriptions/rules/Delete | Kural kaynağı silme işlemi |
> | Eylem | Microsoft.ServiceBus/namespaces/topics/subscriptions/rules/read | Kural kaynak açıklamalarının listesini alın |
> | Eylem | Microsoft.ServiceBus/namespaces/topics/subscriptions/rules/write | Oluşturma veya güncelleştirme kuralı özellikleri. |
> | Eylem | Microsoft.ServiceBus/namespaces/topics/subscriptions/write | Oluşturma veya güncelleştirme TopicSubscription özellikleri. |
> | Eylem | Microsoft.ServiceBus/namespaces/topics/write | Oluşturma veya güncelleştirme konu özellikleri. |
> | Eylem | Microsoft.ServiceBus/namespaces/virtualNetworkRules/delete | Sanal ağ kuralı kaynağı silme |
> | Eylem | Microsoft.ServiceBus/namespaces/virtualNetworkRules/read | Sanal ağ kuralı kaynağı alır |
> | Eylem | Microsoft.ServiceBus/namespaces/virtualNetworkRules/write | Sanal ağ kuralı kaynağı oluşturma |
> | Eylem | Microsoft.ServiceBus/namespaces/write | Namespace kaynağı oluşturun ve özelliklerini güncelleştirin. Etiketleri ve kapasitesi Namespace hangi güncelleştirilebilecek özelliklerdir. |
> | Eylem | Microsoft.ServiceBus/operations/read | İşlemleri Al |
> | Eylem | Microsoft.ServiceBus/register/action | ServiceBus kaynak sağlayıcısı için aboneliği kaydeder ve ServiceBus kaynaklarının oluşturulmasını sağlar |
> | Eylem | Microsoft.ServiceBus/sku/read | Sku kaynak açıklamalarının listesini alın |
> | Eylem | Microsoft.ServiceBus/sku/regions/read | SkuRegions kaynak açıklamalarının listesini alın |
> | Eylem | Microsoft.ServiceBus/unregister/action | ServiceBus kaynak sağlayıcısı için aboneliği kaydeder ve ServiceBus kaynaklarının oluşturulmasını sağlar |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.ServiceFabric/clusters/applications/delete | Herhangi bir uygulamayı silin |
> | Eylem | Microsoft.ServiceFabric/clusters/applications/read | Herhangi bir uygulamayı okuyun |
> | Eylem | Microsoft.ServiceFabric/clusters/applications/services/delete | Herhangi bir hizmeti silin |
> | Eylem | Microsoft.ServiceFabric/clusters/applications/services/partitions/read | Herhangi bir bölümü okuyun |
> | Eylem | Microsoft.ServiceFabric/clusters/applications/services/partitions/replicas/read | Herhangi bir çoğaltmayı okuyun |
> | Eylem | Microsoft.ServiceFabric/clusters/applications/services/read | Herhangi bir hizmeti okuyun |
> | Eylem | Microsoft.ServiceFabric/clusters/applications/services/statuses/read | Tüm hizmet durumlarını oku |
> | Eylem | Microsoft.ServiceFabric/clusters/applications/services/write | Herhangi bir hizmet oluştur veya güncelleştir |
> | Eylem | Microsoft.ServiceFabric/clusters/applications/write | Herhangi bir uygulama oluştur veya güncelleştir |
> | Eylem | Microsoft.ServiceFabric/clusters/applicationTypes/delete | Herhangi bir uygulama türü Sil |
> | Eylem | Microsoft.ServiceFabric/clusters/applicationTypes/read | Herhangi bir uygulama türünü okuyun |
> | Eylem | Microsoft.ServiceFabric/clusters/applicationTypes/versions/delete | Herhangi bir uygulama türü sürümü silin |
> | Eylem | Microsoft.ServiceFabric/clusters/applicationTypes/versions/read | Herhangi bir uygulama türü sürümünü okuyun |
> | Eylem | Microsoft.ServiceFabric/clusters/applicationTypes/versions/write | Herhangi bir uygulama türü sürümü güncelle |
> | Eylem | Microsoft.ServiceFabric/clusters/applicationTypes/write | Herhangi bir uygulama türü güncelle |
> | Eylem | Microsoft.ServiceFabric/clusters/delete | Herhangi bir kümeyi silin |
> | Eylem | Microsoft.ServiceFabric/clusters/nodes/read | Herhangi bir düğümü okuyun |
> | Eylem | Microsoft.ServiceFabric/clusters/read | Herhangi bir kümeyi okuyun |
> | Eylem | Microsoft.ServiceFabric/clusters/statuses/read | Tüm küme durumlarını oku |
> | Eylem | Microsoft.ServiceFabric/clusters/write | Herhangi bir küme oluştur veya güncelleştir |
> | Eylem | Microsoft.ServiceFabric/locations/clusterVersions/read | Herhangi bir küme sürümünü oku |
> | Eylem | Microsoft.ServiceFabric/locations/environments/clusterVersions/read | Belirli bir ortam için herhangi bir küme sürümünü okuyun |
> | Eylem | Microsoft.ServiceFabric/locations/operationresults/read | Herhangi bir işlem sonucunu okuyun |
> | Eylem | Microsoft.ServiceFabric/locations/operations/read | Konuma göre herhangi bir işlemi okuyun |
> | Eylem | Microsoft.ServiceFabric/operations/read | Kullanılabilen herhangi bir işlemi okuyun |
> | Eylem | Microsoft.ServiceFabric/register/action | Herhangi bir eylemi kaydedin |

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.SignalRService/locations/checknameavailability/action | Yeni bir SignalR hizmeti ile kullanmak için bir ad olup olmadığını denetler |
> | Eylem | Microsoft.SignalRService/locations/operationresults/signalr/read | Zaman uyumsuz bir işlemin durumu |
> | Eylem | Microsoft.SignalRService/locations/operationStatuses/operationId/read | Zaman uyumsuz bir işlemin durumu |
> | Eylem | Microsoft.SignalRService/locations/usages/read | Azure SignalR hizmeti için kota kullanımları Al |
> | Eylem | Microsoft.SignalRService/operationresults/read | Zaman uyumsuz bir işlemin durumu |
> | Eylem | Microsoft.SignalRService/operationstatus/read | Zaman uyumsuz bir işlemin durumu |
> | Eylem | Microsoft.SignalRService/register/action | Bir abonelikle 'Microsoft.SignalRService' kaynak sağlayıcısını Kaydet |
> | Eylem | Microsoft.SignalRService/SignalR/delete | Tüm SignalR hizmetini Sil |
> | Eylem | Microsoft.SignalRService/SignalR/eventGridFilters/delete | SignalR öğesinden bir olay Kılavuzu filtresini silin. |
> | Eylem | Microsoft.SignalRService/SignalR/eventGridFilters/read | Belirtilen SignalR için event grid filtreler listeleri ya da belirtilen olay Kılavuzu filtresini özelliklerini alır. |
> | Eylem | Microsoft.SignalRService/SignalR/eventGridFilters/write | Veya bir olay Kılavuzu filtresini belirtilen parametrelerle bir SignalR için güncelleştirilemiyor. |
> | Eylem | Microsoft.SignalRService/SignalR/listkeys/action | SignalR erişim anahtarlarının değerini yönetim portalında veya API'ler görüntüleyin |
> | Eylem | Microsoft.SignalRService/SignalR/read | Yönetim Portalı'nda veya API üzerinden SignalR ayarları ve yapılandırmaları görüntüleyin |
> | Eylem | Microsoft.SignalRService/SignalR/regeneratekey/action | Yönetim Portalı'nda veya API üzerinden SignalR erişim anahtarlarını değiştirin |
> | Eylem | Microsoft.SignalRService/SignalR/restart/action | Yönetim Portalı'nda veya API üzerinden Azure SignalR hizmeti yeniden başlatmak için. Belirli kapalı kalma süresi olur. |
> | Eylem | Microsoft.SignalRService/SignalR/write | SignalR ayarları ve Yönetim Portalı'nda veya API üzerinden yapılandırmalarını değiştirme |
> | Eylem | Microsoft.SignalRService/unregister/action | 'Microsoft.SignalRService' kaynak sağlayıcısına bir abonelik kaydını siler |

## <a name="microsoftsolutions"></a>Microsoft.Solutions

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Solutions/applicationDefinitions/applicationArtifacts/read | Uygulama tanımının uygulama yapıları listeler. |
> | Eylem | Microsoft.Solutions/applicationDefinitions/delete | Uygulama tanımı kaldırır. |
> | Eylem | Microsoft.Solutions/applicationDefinitions/read | Uygulama tanımları listesi alır. |
> | Eylem | Microsoft.Solutions/applicationDefinitions/write | Ekleyin veya bir uygulama tanımını değiştirin. |
> | Eylem | Microsoft.Solutions/applications/applicationArtifacts/read | Uygulama yapıları listeler. |
> | Eylem | Microsoft.Solutions/applications/delete | Bir uygulamayı kaldırır. |
> | Eylem | Microsoft.Solutions/applications/read | Uygulamaların bir listesini alır. |
> | Eylem | Microsoft.Solutions/applications/refreshPermissions/action | Uygulama izinler yeniler. |
> | Eylem | Microsoft.Solutions/applications/updateAccess/action | Uygulama erişimi güncelleştirir. |
> | Eylem | Microsoft.Solutions/applications/write | Uygulama oluşturur. |
> | Eylem | Microsoft.Solutions/jitRequests/delete | Bir JitRequest Kaldır |
> | Eylem | Microsoft.Solutions/jitRequests/read | JitRequests listesini alır. |
> | Eylem | Microsoft.Solutions/jitRequests/write | Bir JitRequest oluşturur |
> | Eylem | Microsoft.Solutions/locations/operationStatuses/read | Kaynak için işlem durumunu okur. |
> | Eylem | Microsoft.Solutions/register/action | Çözümler için kaydolun. |
> | Eylem | Microsoft.Solutions/unregister/action | Çözümlerinden kaydını siler. |

## <a name="microsoftsql"></a>Microsoft.Sql

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Sql/checkNameAvailability/action | Sunucu adı belirli bir aboneliğe yönelik dünya çapında sağlamak için kullanılabilir olup olmadığını doğrulayın. |
> | Eylem | Microsoft.Sql/instancePools/delete | Bir örnek havuzu siler |
> | Eylem | Microsoft.Sql/instancePools/read | Bir örnek havuzu alır |
> | Eylem | Microsoft.Sql/instancePools/usages/read | Bir örnek havuzun kullanım bilgilerini alır |
> | Eylem | Microsoft.Sql/instancePools/write | Oluşturur veya bir örnek havuzu güncelleştirir |
> | Eylem | Microsoft.Sql/locations/auditingSettingsAzureAsyncOperation/read | İlke ayarlama işlemi denetim genişletilmiş sunucusu blob sonucunu Al |
> | Eylem | Microsoft.Sql/locations/auditingSettingsOperationResults/read | İlke ayarlama işlemi denetim sunucusu blob sonucunu Al |
> | Eylem | Microsoft.Sql/locations/capabilities/read | Özellikler, belirli bir konumda Bu abonelik için alır |
> | Eylem | Microsoft.Sql/locations/databaseAzureAsyncOperation/read | Bir veritabanı işlemi durumunu alır. |
> | Eylem | Microsoft.Sql/locations/databaseOperationResults/read | Bir veritabanı işlemi durumunu alır. |
> | Eylem | Microsoft.Sql/locations/deletedServerAsyncOperation/read | Sunucu silindi devam eden işlemleri alır |
> | Eylem | Microsoft.Sql/locations/deletedServerOperationResults/read | Sunucu silindi devam eden işlemleri alır |
> | Eylem | Microsoft.Sql/locations/deletedServers/read | Silinen sunucuları veya belirtilen silinen sunucunun özelliklerini alır listesini döndürür. |
> | Eylem | Microsoft.Sql/locations/deletedServers/recover/action | Silinen sunucusunu kurtarma |
> | Eylem | Microsoft.Sql/locations/elasticPoolAzureAsyncOperation/read | Azure zaman uyumsuz işlem için elastik havuz zaman uyumsuz bir işlemi alır |
> | Eylem | Microsoft.Sql/locations/elasticPoolOperationResults/read | Bir elastik havuz işlemin sonucunu alır. |
> | Eylem | Microsoft.Sql/locations/encryptionProtectorAzureAsyncOperation/read | Devam eden işlemler saydam veri şifrelemesi şifreleme koruyucusu alır |
> | Eylem | Microsoft.Sql/locations/encryptionProtectorOperationResults/read | Devam eden işlemler saydam veri şifrelemesi şifreleme koruyucusu alır |
> | Eylem | Microsoft.Sql/locations/extendedAuditingSettingsAzureAsyncOperation/read | İlke ayarlama işlemi denetim genişletilmiş sunucusu blob sonucunu Al |
> | Eylem | Microsoft.Sql/locations/extendedAuditingSettingsOperationResults/read | İlke ayarlama işlemi denetim genişletilmiş sunucusu blob sonucunu Al |
> | Eylem | Microsoft.Sql/locations/firewallRulesAzureAsyncOperation/read | Bir güvenlik duvarı kuralı işleminin durumunu alır. |
> | Eylem | Microsoft.Sql/locations/firewallRulesOperationResults/read | Bir güvenlik duvarı kuralı işleminin durumunu alır. |
> | Eylem | Microsoft.Sql/locations/instanceFailoverGroups/delete | Var olan bir örneği yük devretme grubunu siler. |
> | Eylem | Microsoft.Sql/locations/instanceFailoverGroups/failover/action | Planlı yük devretme, mevcut bir örneği yük devretme grubunda yürütür. |
> | Eylem | Microsoft.Sql/locations/instanceFailoverGroups/forceFailoverAllowDataLoss/action | Varolan bir örneği yük devretme grubu zorlamalı yük devretme yürütür. |
> | Eylem | Microsoft.Sql/locations/instanceFailoverGroups/read | Liste örneği yük devretme grupları veya belirtilen örnek yük devretme grubu için özellikleri alır döndürür. |
> | Eylem | Microsoft.Sql/locations/instanceFailoverGroups/write | Belirtilen parametrelerle bir örnek yük devretme grubu oluşturur veya özellikleri veya etiketleri belirtilen örnek yük devretme grubu için güncelleştirir. |
> | Eylem | Microsoft.Sql/locations/instancePoolAzureAsyncOperation/read | Bir örnek havuzu işlemin durumunu alır |
> | Eylem | Microsoft.Sql/locations/instancePoolOperationResults/read | İçin bir örnek havuz işlem sonucunu alır |
> | Eylem | Microsoft.Sql/locations/interfaceEndpointProfileAzureAsyncOperation/read | Bir arabirimin uç noktası Azure zaman uyumsuz işlem ayrıntılarını döndürür |
> | Eylem | Microsoft.Sql/locations/interfaceEndpointProfileOperationResults/read | Belirtilen arabirim uç nokta profili işlemin ayrıntılarını döndürür |
> | Eylem | Microsoft.Sql/locations/jobAgentAzureAsyncOperation/read | İş Aracısı işleminin durumunu alır. |
> | Eylem | Microsoft.Sql/locations/jobAgentOperationResults/read | İş Aracısı işleminin sonucu alır. |
> | Eylem | Microsoft.Sql/locations/longTermRetentionBackups/read | Bir konumdaki her sunucuda her veritabanı için uzun süreli saklama yedeklerini listeler |
> | Eylem | Microsoft.Sql/locations/longTermRetentionServers/longTermRetentionBackups/read | Bir sunucuda her veritabanı için uzun süreli saklama yedeklerini listeler |
> | Eylem | Microsoft.Sql/locations/longTermRetentionServers/longTermRetentionDatabases/longTermRetentionBackups/delete | Uzun süreli bekletme yedeklemesi siler |
> | Eylem | Microsoft.Sql/locations/longTermRetentionServers/longTermRetentionDatabases/longTermRetentionBackups/read | Bir veritabanı için uzun süreli saklama yedeklerini listeler |
> | Eylem | Microsoft.Sql/locations/managedDatabaseRestoreAzureAsyncOperation/completeRestore/action | Yönetilen veritabanı geri yükleme işlemi tamamlanır |
> | Eylem | Microsoft.Sql/locations/managedInstanceEncryptionProtectorAzureAsyncOperation/read | Devam eden işlemler saydam veri şifrelemesi yönetilen örnek şifreleme koruyucusu alır |
> | Eylem | Microsoft.Sql/locations/managedInstanceEncryptionProtectorOperationResults/read | Devam eden işlemler saydam veri şifrelemesi yönetilen örnek şifreleme koruyucusu alır |
> | Eylem | Microsoft.Sql/locations/managedInstanceKeyAzureAsyncOperation/read | Saydam veri şifrelemesi alır devam eden işlemleri tarafından yönetilen örnek anahtarlar |
> | Eylem | Microsoft.Sql/locations/managedInstanceKeyOperationResults/read | Saydam veri şifrelemesi alır devam eden işlemleri tarafından yönetilen örnek anahtarlar |
> | Eylem | Microsoft.Sql/locations/managedTransparentDataEncryptionAzureAsyncOperation/read | İlerleme-yönetilen bir veritabanı saydam veri şifreleme işlemleri alır |
> | Eylem | Microsoft.Sql/locations/managedTransparentDataEncryptionOperationResults/read | İlerleme-yönetilen bir veritabanı saydam veri şifreleme işlemleri alır |
> | Eylem | Microsoft.Sql/locations/privateEndpointConnectionAzureAsyncOperation/read | Özel uç nokta bağlantı işlemi için sonucunu alır |
> | Eylem | Microsoft.Sql/locations/privateEndpointConnectionOperationResults/read | Özel uç nokta bağlantı işlemi için sonucunu alır |
> | Eylem | Microsoft.Sql/locations/privateEndpointConnectionProxyAzureAsyncOperation/read | Bir özel uç nokta bağlantı proxy işleminin sonucunu alır |
> | Eylem | Microsoft.Sql/locations/privateEndpointConnectionProxyOperationResults/read | Bir özel uç nokta bağlantı proxy işleminin sonucunu alır |
> | Eylem | Microsoft.Sql/locations/read | Belirli bir aboneliği için kullanılabilir konumları alır |
> | Eylem | Microsoft.Sql/locations/serverKeyAzureAsyncOperation/read | Saydam veri şifrelemesi sunucu anahtarları alır devam eden işlemler |
> | Eylem | Microsoft.Sql/locations/serverKeyOperationResults/read | Saydam veri şifrelemesi sunucu anahtarları alır devam eden işlemler |
> | Eylem | Microsoft.Sql/locations/syncAgentOperationResults/read | Eşitleme Aracısı kaynak işleminin sonucunu Al |
> | Eylem | Microsoft.Sql/locations/syncDatabaseIds/read | Belirli bir bölge ve abonelik için eşitleme veritabanı kimlikleri alma |
> | Eylem | Microsoft.Sql/locations/syncGroupOperationResults/read | Eşitleme grubu kaynak işleminin sonucunu Al |
> | Eylem | Microsoft.Sql/locations/syncMemberOperationResults/read | Eşitleme üyesi kaynak işleminin sonucunu Al |
> | Eylem | Microsoft.Sql/locations/usages/read | Kullanım ölçümleri koleksiyonunu bir konumda Bu abonelik için alır. |
> | Eylem | Microsoft.Sql/locations/virtualNetworkRulesAzureAsyncOperation/read | Azure zaman uyumsuz işlem belirtilen sanal ağ kuralları ayrıntılarını döndürür  |
> | Eylem | Microsoft.Sql/locations/virtualNetworkRulesOperationResults/read | Belirtilen sanal ağ kuralları işlemin ayrıntılarını döndürür  |
> | Eylem | Microsoft.Sql/managedInstances/administrators/delete | Mevcut bir yönetilen örnek Yöneticisi siler. |
> | Eylem | Microsoft.Sql/managedInstances/administrators/read | Yönetilen örnek yöneticilerinin listesini alır. |
> | Eylem | Microsoft.Sql/managedInstances/administrators/write | Oluşturur veya yönetilen örnek Yöneticisi belirtilen parametrelerle güncelleştirir. |
> | Eylem | Microsoft.Sql/managedInstances/databases/backupShortTermRetentionPolicies/read | Yönetilen bir veritabanı için bir kısa vadeli bekletme ilkesi alır |
> | Eylem | Microsoft.Sql/managedInstances/databases/backupShortTermRetentionPolicies/write | Yönetilen bir veritabanı için kısa vadeli bekletme ilkesini güncelleştirir |
> | Eylem | Microsoft.Sql/managedInstances/databases/currentSensitivityLabels/read | Belirli bir veritabanının duyarlılık etiketleri listeleme |
> | Eylem | Microsoft.Sql/managedInstances/databases/currentSensitivityLabels/write | Toplu güncelleştirme duyarlılık etiketleri |
> | Eylem | Microsoft.Sql/managedInstances/databases/delete | Mevcut bir yönetilen veritabanı siler |
> | Eylem | Microsoft.Sql/managedInstances/databases/providers/Microsoft.Insights/diagnosticSettings/read | Kaynağın tanılama ayarını alır |
> | Eylem | Microsoft.Sql/managedInstances/databases/providers/Microsoft.Insights/diagnosticSettings/write | Kaynağın tanılama ayarını oluşturur veya güncelleştirir |
> | Eylem | Microsoft.Sql/managedInstances/databases/providers/Microsoft.Insights/logDefinitions/read | Yönetilen örnek veritabanları için mevcut günlükleri alır |
> | Eylem | Microsoft.Sql/managedInstances/databases/read | Yönetilen veritabanı mevcut alır |
> | Eylem | Microsoft.Sql/managedInstances/databases/recommendedSensitivityLabels/read | Belirli bir veritabanının duyarlılık etiketleri listeleme |
> | Eylem | Microsoft.Sql/managedInstances/databases/schemas/read | Bir yönetilen veritabanı şeması alın. |
> | Eylem | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/read | Yönetilen bir veritabanı sütunu Al |
> | Eylem | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/delete | Belirtilen sütunun duyarlılık etiketi Sil |
> | Eylem | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/disable/action | Belirli bir sütunda öneriler duyarlılık devre dışı bırak |
> | Eylem | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/enable/action | Belirli bir sütunda duyarlılık önerilerini etkinleştirmek |
> | Eylem | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/read | Belirtilen sütunun duyarlılık etiketi Al |
> | Eylem | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/write | Duyarlılık etiketi belirli bir sütunun güncelle |
> | Eylem | Microsoft.Sql/managedInstances/databases/schemas/tables/read | Yönetilen bir veritabanı tablosu alın |
> | Eylem | Microsoft.Sql/managedInstances/databases/securityAlertPolicies/read | Belirli bir sunucu için yapılandırılan yönetilen veritabanı tehdit algılama ilkelerinin bir listesini alma |
> | Eylem | Microsoft.Sql/managedInstances/databases/securityAlertPolicies/write | Belirli bir yönetilen veritabanı için veritabanı tehdit algılama İlkesi değiştirme |
> | Eylem | Microsoft.Sql/managedInstances/databases/securityEvents/read | Yönetilen veritabanı güvenlik olaylarını alır |
> | Eylem | Microsoft.Sql/managedInstances/databases/sensitivityLabels/read | Belirli bir veritabanının duyarlılık etiketleri listeleme |
> | Eylem | Microsoft.Sql/managedInstances/databases/transparentDataEncryption/read | Belirli bir yönetilen veritabanı veritabanında saydam veri şifrelemesi ayrıntılarını alma |
> | Eylem | Microsoft.Sql/managedInstances/databases/transparentDataEncryption/write | ' % S'veritabanı saydam veri şifrelemesi için belirli bir yönetilen veritabanı değiştirme |
> | Eylem | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/delete | Belirli bir veritabanı için güvenlik açığı değerlendirmesi Kaldır |
> | Eylem | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/read | Güvenlik Açığı değerlendirmesi ilkeleri bir givendatabase alma |
> | Eylem | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/rules/baselines/delete | Belirli bir veritabanı için güvenlik açığı değerlendirmesi kural taban çizgisini Kaldır |
> | Eylem | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/rules/baselines/read | Belirli bir veritabanı için güvenlik açığı değerlendirmesi kuralı için temel alma |
> | Eylem | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/rules/baselines/write | Belirli bir veritabanı için güvenlik açığı değerlendirmesi kural taban çizgisini Değiştir |
> | Eylem | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/scans/export/action | Var olan bir tarama sonucu, insan tarafından okunabilir bir biçimine dönüştürün. Hiçbir şey zaten olur |
> | Eylem | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/scans/initiateScan/action | Güvenlik Açığı değerlendirmesi veritabanı taraması çalıştırın. |
> | Eylem | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/scans/read | Veritabanı güvenlik açığı değerlendirmesi taraması kayıtları dönmesini veya belirtilen tarama kimliği için tarama kayıt Al |
> | Eylem | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/write | Belirli bir veritabanı için güvenlik açığı değerlendirmesi değiştirme |
> | Eylem | Microsoft.Sql/managedInstances/databases/write | Yeni bir veritabanı oluşturur veya mevcut bir veritabanını güncelleştirir. |
> | Eylem | Microsoft.Sql/managedInstances/delete | Mevcut bir yönetilen örnek siler. |
> | Eylem | Microsoft.Sql/managedInstances/encryptionProtector/read | Sunucu şifreleme koruyucusu listesini döndürür veya özellikleri için belirtilen sunucu şifreleme koruyucusu alır. |
> | Eylem | Microsoft.Sql/managedInstances/encryptionProtector/write | Belirtilen sunucu şifreleme koruyucusu için özelliklerini güncelleştirir. |
> | Eylem | Microsoft.Sql/managedInstances/keys/delete | Mevcut bir Azure SQL yönetilen örneği anahtarı siler. |
> | Eylem | Microsoft.Sql/managedInstances/keys/read | Dönüş yönetilen örnek listesi, anahtarlar veya belirtilen yönetilen örnek anahtarı için özellikleri alır. |
> | Eylem | Microsoft.Sql/managedInstances/keys/write | Belirtilen parametrelerle bir anahtar oluşturur veya özellikleri veya etiketleri belirtilen yönetilen örnek anahtarı için güncelleştirin. |
> | Eylem | Microsoft.Sql/managedInstances/metricDefinitions/read | Yönetilen örnek ölçüm tanımlarını Al |
> | Eylem | Microsoft.Sql/managedInstances/metrics/read | Yönetilen örnek ölçümleri alın |
> | Eylem | Microsoft.Sql/managedInstances/providers/Microsoft.Insights/diagnosticSettings/read | Kaynağın tanılama ayarını alır |
> | Eylem | Microsoft.Sql/managedInstances/providers/Microsoft.Insights/diagnosticSettings/write | Kaynağın tanılama ayarını oluşturur veya güncelleştirir |
> | Eylem | Microsoft.Sql/managedInstances/providers/Microsoft.Insights/logDefinitions/read | Yönetilen örnek için kullanılabilir günlükleri alır |
> | Eylem | Microsoft.Sql/managedInstances/providers/Microsoft.Insights/metricDefinitions/read | Dönüş türleri yönetilen örnekleri için kullanılabilir ölçümleri |
> | Eylem | Microsoft.Sql/managedInstances/read | Yönetilen örnek veya belirtilen yönetilen örneği için özellikleri alır listesini döndürür. |
> | Eylem | Microsoft.Sql/managedInstances/recoverableDatabases/read | Kurtarılabilir yönetilen veritabanlarının listesini döndürür |
> | Eylem | Microsoft.Sql/managedInstances/restorableDroppedDatabases/backupShortTermRetentionPolicies/read | Bırakılan yönetilen bir veritabanı için bir kısa vadeli bekletme ilkesi alır |
> | Eylem | Microsoft.Sql/managedInstances/restorableDroppedDatabases/backupShortTermRetentionPolicies/write | Kısa vadeli bekletme ilkesi bırakılan yönetilen bir veritabanı için güncelleştirmeler |
> | Eylem | Microsoft.Sql/managedInstances/restorableDroppedDatabases/read | Yönetilen veritabanları listesi geri yüklenebilen bırakılan döndürür. |
> | Eylem | Microsoft.Sql/managedInstances/securityAlertPolicies/read | Belirli bir sunucu için yapılandırılan yönetilen sunucu tehdit algılama ilkelerinin bir listesini alma |
> | Eylem | Microsoft.Sql/managedInstances/securityAlertPolicies/write | Belirli bir yönetilen sunucu için yönetilen sunucu tehdit algılama ilkesini değiştirme |
> | Eylem | Microsoft.Sql/managedInstances/tdeCertificates/action | Oluşturma/güncelleştirme TDE sertifika |
> | Eylem | Microsoft.Sql/managedInstances/vulnerabilityAssessments/delete | Belirli bir yönetilen örnek için güvenlik açığı değerlendirmesi Kaldır |
> | Eylem | Microsoft.Sql/managedInstances/vulnerabilityAssessments/read | Belirli bir yönetilen örnek güvenlik açığı değerlendirmesi ilkeleri alma |
> | Eylem | Microsoft.Sql/managedInstances/vulnerabilityAssessments/write | Belirli bir yönetilen örnek için güvenlik açığı değerlendirmesi değiştirme |
> | Eylem | Microsoft.Sql/managedInstances/write | Yönetilen örnek belirtilen parametrelerle oluşturur veya özellikleri veya etiketleri belirtilen yönetilen örnek için güncelleştirin. |
> | Eylem | Microsoft.Sql/operations/read | Kullanılabilir REST işlemlerini alır |
> | Eylem | Microsoft.Sql/register/action | Microsoft SQL veritabanı kaynak sağlayıcısı için aboneliği kaydeder ve Microsoft SQL veritabanları oluşturulmasını sağlar. |
> | Eylem | Microsoft.Sql/servers/administratorOperationResults/read | Sunucu yöneticileri devam eden işlemleri alır |
> | Eylem | Microsoft.Sql/servers/administrators/delete | Sunucu Yöneticisi'ni Sil |
> | Eylem | Microsoft.Sql/servers/administrators/read | Sunucu Yöneticisi ayrıntılarını alma |
> | Eylem | Microsoft.Sql/servers/administrators/write | Sunucu Yöneticisi güncelle |
> | Eylem | Microsoft.Sql/servers/advisors/read | Danışmanlar için sunucunun kullanılabilir listesini döndürür |
> | Eylem | Microsoft.Sql/servers/advisors/recommendedActions/read | Sunucusu için belirtilen Danışmanı, önerilen eylemlerin listesini döndürür |
> | Eylem | Microsoft.Sql/servers/advisors/recommendedActions/write | Sunucuda önerilen eylemi uygulayın |
> | Eylem | Microsoft.Sql/servers/advisors/write | Güncelleştirmeleri otomatik-sunucu düzeyinde bir danışman durumunu yürütün. |
> | Eylem | Microsoft.Sql/servers/auditingPolicies/read | Denetim İlkesi belirli bir sunucuda yapılandırılan varsayılan sunucu tablosu ayrıntılarını alma |
> | Eylem | Microsoft.Sql/servers/auditingPolicies/write | Varsayılan sunucu tablo için verilen bir sunucu denetimi değiştirme |
> | Eylem | Microsoft.Sql/servers/auditingSettings/operationResults/read | İlke ayarlama işlemi denetim sunucusu blob sonucunu Al |
> | Eylem | Microsoft.Sql/servers/auditingSettings/read | Belirli bir sunucuda yapılandırılmış sunucu blob denetim ilkesi ayrıntılarını alma |
> | Eylem | Microsoft.Sql/servers/auditingSettings/write | Verilen bir sunucu için sunucu blob denetimi değiştirme |
> | Eylem | Microsoft.Sql/servers/automaticTuning/read | Sunucusu için otomatik ayarlama ayarlarını döndürür |
> | Eylem | Microsoft.Sql/servers/automaticTuning/write | Sunucu otomatik ayarlama ayarlarını güncelleştirir ve güncelleştirilmiş ayarlar döndürür |
> | Eylem | Microsoft.Sql/servers/backupLongTermRetentionVaults/delete | Mevcut bir yedekleme arşivleme kasası siler. |
> | Eylem | Microsoft.Sql/servers/backupLongTermRetentionVaults/read | Bu işlem, bir yedekleme uzun süreli saklama kasası almak için kullanılır. Bu sunucu için kayıtlı kasası hakkında bilgi döndürür |
> | Eylem | Microsoft.Sql/servers/backupLongTermRetentionVaults/write | Bu işlem bir yedekleme uzun süreli saklama kaydetmek için kullanılan bir sunucuya kasası |
> | Eylem | Microsoft.Sql/servers/communicationLinks/delete | Mevcut bir sunucu iletişimi bağlantı siler. |
> | Eylem | Microsoft.Sql/servers/communicationLinks/read | Belirtilen bir sunucu iletişimi bağlantı listesini döndürür. |
> | Eylem | Microsoft.Sql/servers/communicationLinks/write | Veya bir sunucu iletişimi bağlantı güncelleştirilemiyor. |
> | Eylem | Microsoft.Sql/servers/connectionPolicies/read | Belirtilen sunucu, sunucu bağlantı ilkeleri listesini döndürür. |
> | Eylem | Microsoft.Sql/servers/connectionPolicies/write | Veya bir sunucu bağlantı İlkesi güncelleştirilemiyor. |
> | Eylem | Microsoft.Sql/servers/databases/advisors/read | Danışmanlar için veritabanı kullanılabilir listesini döndürür |
> | Eylem | Microsoft.Sql/servers/databases/advisors/recommendedActions/read | Belirtilen veritabanına ilişkin dizin Danışmanı, önerilen eylemlerin listesini döndürür |
> | Eylem | Microsoft.Sql/servers/databases/advisors/recommendedActions/write | Önerilen eylemi veritabanı üzerinde uygulama |
> | Eylem | Microsoft.Sql/servers/databases/advisors/write | Güncelleştirme otomatik yürütme veritabanı düzeyinde bir advisor durumu. |
> | Eylem | Microsoft.Sql/servers/databases/auditingPolicies/read | Belirli bir veritabanı üzerinde yapılandırılan tablo denetim ilkesi ayrıntılarını alma |
> | Eylem | Microsoft.Sql/servers/databases/auditingPolicies/write | Belirli bir veritabanı için tablo denetim ilkesini değiştirme |
> | Eylem | Microsoft.Sql/servers/databases/auditingSettings/read | Belirli bir veritabanı üzerinde yapılandırılmış blob denetim ilkesi ayrıntılarını alma |
> | Eylem | Microsoft.Sql/servers/databases/auditingSettings/write | Belirli bir veritabanı için blob denetim ilkesini değiştirme |
> | Eylem | Microsoft.Sql/servers/databases/auditRecords/read | Veritabanı blob Denetim kayıtlarını alma |
> | Eylem | Microsoft.Sql/servers/databases/automaticTuning/read | Bir veritabanı otomatik ayarlama ayarlarını döndürür |
> | Eylem | Microsoft.Sql/servers/databases/automaticTuning/write | Bir veritabanı otomatik ayarlama ayarlarını güncelleştirir ve güncelleştirilmiş ayarlar döndürür |
> | Eylem | Microsoft.Sql/servers/databases/azureAsyncOperation/read | Bir veritabanı işlemi durumunu alır. |
> | Eylem | Microsoft.Sql/servers/databases/backupLongTermRetentionPolicies/read | Belirtilen bir veritabanı yedekleme arşivleme ilkelerin listesini döndürür. |
> | Eylem | Microsoft.Sql/servers/databases/backupLongTermRetentionPolicies/write | Veya bir veritabanı yedekleme arşivleme İlkesi güncelleştirilemiyor. |
> | Eylem | Microsoft.Sql/servers/databases/backupShortTermRetentionPolicies/read | Kısa vadeli bekletme ilkesi için bir veritabanı alır. |
> | Eylem | Microsoft.Sql/servers/databases/backupShortTermRetentionPolicies/write | Bir veritabanı için kısa vadeli bekletme ilkesini güncelleştirir |
> | Eylem | Microsoft.Sql/servers/databases/connectionPolicies/read | Belirli bir veritabanı üzerinde yapılandırılan bağlantı ilkesi ayrıntılarını alma |
> | Eylem | Microsoft.Sql/servers/databases/connectionPolicies/write | Belirli bir veritabanı için bağlantı ilkesini değiştirme |
> | Eylem | Microsoft.Sql/servers/databases/currentSensitivityLabels/read | Belirli bir veritabanının duyarlılık etiketleri listeleme |
> | Eylem | Microsoft.Sql/servers/databases/currentSensitivityLabels/write | Toplu güncelleştirme duyarlılık etiketleri |
> | Eylem | Microsoft.Sql/servers/databases/dataMaskingPolicies/read | Veritabanı veri maskeleme ilkeleri listesini döndürür. |
> | Eylem | Microsoft.Sql/servers/databases/dataMaskingPolicies/rules/delete | Veri maskeleme belirli bir veritabanı için ilke kuralı Sil |
> | Eylem | Microsoft.Sql/servers/databases/dataMaskingPolicies/rules/read | Belirli bir veritabanı üzerinde yapılandırılan ilke kuralı maskeleme veri ayrıntılarını alma |
> | Eylem | Microsoft.Sql/servers/databases/dataMaskingPolicies/rules/write | Veri maskeleme ilke kuralı belirli bir veritabanı için değiştirin |
> | Eylem | Microsoft.Sql/servers/databases/dataMaskingPolicies/write | Veri maskeleme İlkesi belirli bir veritabanı için değiştirin |
> | Eylem | Microsoft.Sql/servers/databases/dataWarehouseQueries/dataWarehouseQuerySteps/read | Veri ambarı sorgusu seçili adımın kimliği için dağıtılmış sorgu adımı bilgi döndürür |
> | Eylem | Microsoft.Sql/servers/databases/dataWarehouseQueries/read | Seçili sorgu kimliği için veri ambarı dağıtım sorgu bilgilerini döndürür |
> | Eylem | Microsoft.Sql/servers/databases/dataWarehouseUserActivities/read | Kullanıcı etkinlikleri çalışmakta olan ve askıya alınan sorgular içeren bir SQL veri ambarı örneği alır. |
> | Eylem | Microsoft.Sql/servers/databases/delete | Varolan bir veritabanını siler. |
> | Eylem | Microsoft.Sql/servers/databases/export/action | Azure SQL veritabanını dışarı aktarma |
> | Eylem | Microsoft.Sql/servers/databases/extendedAuditingSettings/read | Belirli bir veritabanı üzerinde yapılandırılan genişletilmiş blob denetim ilkesi ayrıntılarını alma |
> | Eylem | Microsoft.Sql/servers/databases/extendedAuditingSettings/write | Belirli bir veritabanı için genişletilmiş blob denetim ilkesini değiştirme |
> | Eylem | Microsoft.Sql/servers/databases/extensions/read | Veritabanı için uzantıları koleksiyonunu alır. |
> | Eylem | Microsoft.Sql/servers/databases/extensions/write | Belirli bir veritabanı için uzantıyı değiştirin |
> | Eylem | Microsoft.Sql/servers/databases/failover/action | Müşteri tarafından başlatılan veritabanı yük devretme. |
> | Eylem | Microsoft.Sql/servers/databases/geoBackupPolicies/read | Belirli bir veritabanı için coğrafi yedekleme ilkelerini alma |
> | Eylem | Microsoft.Sql/servers/databases/geoBackupPolicies/write | Bir veritabanı geobackup İlkesi güncelle |
> | Eylem | Microsoft.Sql/servers/databases/importExportOperationResults/read | İlerleme-içeri/dışarı aktarma işlemleri alır |
> | Eylem | Microsoft.Sql/servers/databases/maintenanceWindowOptions/read | Seçili bir veritabanı için kullanılabilen bakım pencereleri listesini alır. |
> | Eylem | Microsoft.Sql/servers/databases/maintenanceWindows/read | Bakım Seçilen veritabanı için windows ayarlarını alır. |
> | Eylem | Microsoft.Sql/servers/databases/maintenanceWindows/write | Bakım Seçilen veritabanı için windows ayarlarını belirler. |
> | Eylem | Microsoft.Sql/servers/databases/metricDefinitions/read | Dönüş türleri veritabanları için kullanılabilir ölçümleri |
> | Eylem | Microsoft.Sql/servers/databases/metrics/read | Veritabanları için dönüş ölçümleri |
> | Eylem | Microsoft.Sql/servers/databases/move/action | Mevcut bir veritabanının adını değiştirin. |
> | Eylem | Microsoft.Sql/servers/databases/operationResults/read | Bir veritabanı işlemi durumunu alır. |
> | Eylem | Microsoft.Sql/servers/databases/operations/cancel/action | Azure SQL veritabanı henüz tamamlanmadı zaman uyumsuz işlem iptal eder. |
> | Eylem | Microsoft.Sql/servers/databases/operations/read | Veritabanı üzerinde gerçekleştirilen işlemlerin listesini döndürür |
> | Eylem | Microsoft.Sql/servers/databases/pause/action | Azure SQL veri ambarı veritabanını duraklatma |
> | Eylem | Microsoft.Sql/servers/databases/providers/Microsoft.Insights/diagnosticSettings/read | Kaynağın tanılama ayarını alır |
> | Eylem | Microsoft.Sql/servers/databases/providers/Microsoft.Insights/diagnosticSettings/write | Kaynağın tanılama ayarını oluşturur veya güncelleştirir |
> | Eylem | Microsoft.Sql/servers/databases/providers/Microsoft.Insights/logDefinitions/read | Veritabanları için mevcut günlükleri alır |
> | Eylem | Microsoft.Sql/servers/databases/providers/Microsoft.Insights/metricDefinitions/read | Dönüş türleri veritabanları için kullanılabilir ölçümleri |
> | Eylem | Microsoft.Sql/servers/databases/queryStore/queryTexts/read | Belirtilen parametrelere karşılık gelen sorgu metinleri koleksiyonunu döndürür. |
> | Eylem | Microsoft.Sql/servers/databases/queryStore/read | Veritabanı için Query Store ayarlarını geçerli değerlerini döndürür. |
> | Eylem | Microsoft.Sql/servers/databases/queryStore/write | Veritabanı için Query Store ayarını güncelleştirir |
> | Eylem | Microsoft.Sql/servers/databases/read | Veritabanları veya belirtilen veritabanı özelliklerini alır listesini döndürür. |
> | Eylem | Microsoft.Sql/servers/databases/recommendedSensitivityLabels/read | Belirli bir veritabanının duyarlılık etiketleri listeleme |
> | Eylem | Microsoft.Sql/servers/databases/replicationLinks/delete | Çoğaltma ilişkisinin zorla ve olası veri kaybı Sonlandır |
> | Eylem | Microsoft.Sql/servers/databases/replicationLinks/failover/action | Tüm eşitlemeden sonra Yük devretme birincil sunucudan çoğaltma relationship\u0027s ile bu veritabanının birincil yapılıyor ve uzak ikincil birincil yapılıyor değiştirir |
> | Eylem | Microsoft.Sql/servers/databases/replicationLinks/forceFailoverAllowDataLoss/action | Olası veri kaybı olmadan hemen bu veritabanı çoğaltma relationship\u0027s birincil yapılıyor ve uzak ikincil birincil yapılıyor yük devretme |
> | Eylem | Microsoft.Sql/servers/databases/replicationLinks/read | Return çoğaltma listesini bağlantılar veya özellikleri için belirtilen Çoğaltma bağlantılarını alır. |
> | Eylem | Microsoft.Sql/servers/databases/replicationLinks/unlink/action | Çoğaltma ilişkisinin zorla veya iş ortağı ile eşitlemeden sonra Sonlandır |
> | Eylem | Microsoft.Sql/servers/databases/replicationLinks/updateReplicationMode/action | Zaman uyumlu veya zaman uyumsuz modda bağlantısını için güncelleştirme çoğaltma modu |
> | Eylem | Microsoft.Sql/servers/databases/restorePoints/action | Yeni bir geri yükleme noktası oluşturur |
> | Eylem | Microsoft.Sql/servers/databases/restorePoints/delete | Veritabanı için bir geri yükleme noktasını siler. |
> | Eylem | Microsoft.Sql/servers/databases/restorePoints/read | Döndürür geri yükleme noktaları için veritabanı. |
> | Eylem | Microsoft.Sql/servers/databases/resume/action | Azure SQL veri ambarı veritabanını sürdürme |
> | Eylem | Microsoft.Sql/servers/databases/schemas/read | Veritabanı şeması alın. |
> | Eylem | Microsoft.Sql/servers/databases/schemas/tables/columns/read | Bir veritabanı sütunu alın. |
> | Eylem | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/delete | Belirtilen sütunun duyarlılık etiketi Sil |
> | Eylem | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/disable/action | Belirli bir sütunda öneriler duyarlılık devre dışı bırak |
> | Eylem | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/enable/action | Belirli bir sütunda duyarlılık önerilerini etkinleştirmek |
> | Eylem | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/read | Belirtilen sütunun duyarlılık etiketi Al |
> | Eylem | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/write | Duyarlılık etiketi belirli bir sütunun güncelle |
> | Eylem | Microsoft.Sql/servers/databases/schemas/tables/read | Bir veritabanı tablosuna alın. |
> | Eylem | Microsoft.Sql/servers/databases/schemas/tables/recommendedIndexes/read | Bir veritabanı üzerinde dizin önerileri listesi alınamıyor |
> | Eylem | Microsoft.Sql/servers/databases/schemas/tables/recommendedIndexes/write | Dizin önerisini uygulama |
> | Eylem | Microsoft.Sql/servers/databases/securityAlertPolicies/read | Belirli bir sunucu için yapılandırılmış veritabanı tehdit algılama ilkelerinin bir listesini alma |
> | Eylem | Microsoft.Sql/servers/databases/securityAlertPolicies/write | Belirli bir veritabanı için veritabanı tehdit algılama İlkesi değiştirme |
> | Eylem | Microsoft.Sql/servers/databases/securityMetrics/read | Veritabanı güvenlik ölçümlerini koleksiyonunu alır |
> | Eylem | Microsoft.Sql/servers/databases/sensitivityLabels/read | Belirli bir veritabanının duyarlılık etiketleri listeleme |
> | Eylem | Microsoft.Sql/servers/databases/serviceTierAdvisors/read | Performansı artırmak veya maliyetini azaltmak için sorgu yürütme istatistikleri temel veritabanı ölçeği artırılabilen veya azaltılabilen hakkında öneri döndürür |
> | Eylem | Microsoft.Sql/servers/databases/skus/read | Bir veritabanı için kullanılabilir SKU'ları bir koleksiyonunu alır |
> | Eylem | Microsoft.Sql/servers/databases/syncGroups/cancelSync/action | Eşitleme grubu eşitleme iptal et |
> | Eylem | Microsoft.Sql/servers/databases/syncGroups/delete | Var olan bir eşitleme grubunu siler. |
> | Eylem | Microsoft.Sql/servers/databases/syncGroups/hubSchemas/read | Hub veritabanı şemalarını eşitleme listesini döndürür |
> | Eylem | Microsoft.Sql/servers/databases/syncGroups/logs/read | Eşitleme grubu günlüklerini listesini döndürür |
> | Eylem | Microsoft.Sql/servers/databases/syncGroups/read | Dönüş eşitleme listesi, grupları veya özellikleri için belirtilen eşitleme grubunu alır. |
> | Eylem | Microsoft.Sql/servers/databases/syncGroups/refreshHubSchema/action | Eşitleme hub veritabanı şemasını Yenile |
> | Eylem | Microsoft.Sql/servers/databases/syncGroups/refreshHubSchemaOperationResults/read | Eşitleme hub şema yenileme işleminin sonucunu Al |
> | Eylem | Microsoft.Sql/servers/databases/syncGroups/syncMembers/delete | Mevcut bir eşitleme üyesi siler. |
> | Eylem | Microsoft.Sql/servers/databases/syncGroups/syncMembers/read | Belirtilen eşitleme üyesi için eşitleme üyesi veya özelliklerini alır listesini döndürür. |
> | Eylem | Microsoft.Sql/servers/databases/syncGroups/syncMembers/refreshSchema/action | Eşitleme üyesi şemasını Yenile |
> | Eylem | Microsoft.Sql/servers/databases/syncGroups/syncMembers/refreshSchemaOperationResults/read | Eşitleme üyesi şema yenileme işleminin sonucunu Al |
> | Eylem | Microsoft.Sql/servers/databases/syncGroups/syncMembers/schemas/read | Üye veritabanı şemalarını eşitleme listesini döndürür |
> | Eylem | Microsoft.Sql/servers/databases/syncGroups/syncMembers/write | Eşitleme üyesi belirtilen parametrelerle oluşturur veya özelliklerini güncelleştirmek için belirtilen eşitleme üyesi. |
> | Eylem | Microsoft.Sql/servers/databases/syncGroups/triggerSync/action | Tetikleyici eşitleme grubunun eşitlemesi |
> | Eylem | Microsoft.Sql/servers/databases/syncGroups/write | Belirtilen parametrelerle bir eşitleme grubu oluşturur veya belirtilen eşitleme grubu özelliklerini güncelleştirin. |
> | Eylem | Microsoft.Sql/servers/databases/topQueries/queryText/action | Seçili sorgu kimliği için Transact-SQL metnini döndürür |
> | Eylem | Microsoft.Sql/servers/databases/topQueries/read | Döndürür seçili sorgu için çalışma zamanı istatistikleri seçilen zaman aralığı içinde toplanır. |
> | Eylem | Microsoft.Sql/servers/databases/topQueries/statistics/read | Döndürür seçili sorgu için çalışma zamanı istatistikleri seçilen zaman aralığı içinde toplanır. |
> | Eylem | Microsoft.Sql/servers/databases/transparentDataEncryption/operationResults/read | Devam eden işlemler saydam veri şifrelemesi alır |
> | Eylem | Microsoft.Sql/servers/databases/transparentDataEncryption/read | Durum ve belirli bir veritabanı için saydam veri şifrelemesi güvenlik özelliği ayrıntılarını alma |
> | Eylem | Microsoft.Sql/servers/databases/transparentDataEncryption/write | Saydam veri şifreleme durumunu değiştirme |
> | Eylem | Microsoft.Sql/servers/databases/upgradeDataWarehouse/action | Azure SQL veri ambarı veritabanını yükseltme |
> | Eylem | Microsoft.Sql/servers/databases/usages/read | Azure SQL veritabanı kullanımı bilgilerini alır |
> | Eylem | Microsoft.Sql/servers/databases/vulnerabilityAssessments/delete | Belirli bir veritabanı için güvenlik açığı değerlendirmesi Kaldır |
> | Eylem | Microsoft.Sql/servers/databases/vulnerabilityAssessments/read | Güvenlik Açığı değerlendirmesi ilkeleri bir givendatabase alma |
> | Eylem | Microsoft.Sql/servers/databases/vulnerabilityAssessments/rules/baselines/delete | Belirli bir veritabanı için güvenlik açığı değerlendirmesi kural taban çizgisini Kaldır |
> | Eylem | Microsoft.Sql/servers/databases/vulnerabilityAssessments/rules/baselines/read | Belirli bir veritabanı için güvenlik açığı değerlendirmesi kuralı için temel alma |
> | Eylem | Microsoft.Sql/servers/databases/vulnerabilityAssessments/rules/baselines/write | Belirli bir veritabanı için güvenlik açığı değerlendirmesi kural taban çizgisini Değiştir |
> | Eylem | Microsoft.Sql/servers/databases/vulnerabilityAssessments/scans/export/action | Var olan bir tarama sonucu, insan tarafından okunabilir bir biçimine dönüştürün. Hiçbir şey zaten olur |
> | Eylem | Microsoft.Sql/servers/databases/vulnerabilityAssessments/scans/initiateScan/action | Güvenlik Açığı değerlendirmesi veritabanı taraması çalıştırın. |
> | Eylem | Microsoft.Sql/servers/databases/vulnerabilityAssessments/scans/read | Veritabanı güvenlik açığı değerlendirmesi taraması kayıtları dönmesini veya belirtilen tarama kimliği için tarama kayıt Al |
> | Eylem | Microsoft.Sql/servers/databases/vulnerabilityAssessments/write | Belirli bir veritabanı için güvenlik açığı değerlendirmesi değiştirme |
> | Eylem | Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/action | Güvenlik Açığı değerlendirmesi veritabanı taraması çalıştırın. |
> | Eylem | Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/operationResults/read | Veritabanı güvenlik açığı değerlendirmesi taraması yürütme işleminin sonucunu Al |
> | Eylem | Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/read | Belirli bir veritabanı üzerinde yapılandırılan güvenlik açığı değerlendirmesi ayrıntılarını alma |
> | Eylem | Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/write | Belirli bir veritabanı için güvenlik açığı değerlendirmesi değiştirme |
> | Eylem | Microsoft.Sql/servers/databases/write | Özellikleri veya etiketleri için belirtilen veritabanı güncelleştirmesi ya da belirtilen parametrelerle bir veritabanı oluşturur. |
> | Eylem | Microsoft.Sql/servers/delete | Mevcut bir sunucu siler. |
> | Eylem | Microsoft.Sql/servers/disasterRecoveryConfiguration/delete | Verilen bir sunucu için mevcut bir olağanüstü durum kurtarma yapılandırmaları siler |
> | Eylem | Microsoft.Sql/servers/disasterRecoveryConfiguration/failover/action | Yük devretme bir DisasterRecoveryConfiguration |
> | Eylem | Microsoft.Sql/servers/disasterRecoveryConfiguration/forceFailoverAllowDataLoss/action | Yük devretme bir DisasterRecoveryConfiguration zorla |
> | Eylem | Microsoft.Sql/servers/disasterRecoveryConfiguration/read | Bu sunucu içeren kurtarma yapılandırmalar olağanüstü durum koleksiyonunu alır |
> | Eylem | Microsoft.Sql/servers/disasterRecoveryConfiguration/write | Sunucu olağanüstü durum kurtarma yapılandırmasını değiştirme |
> | Eylem | Microsoft.Sql/servers/elasticPoolEstimates/read | Elastik havuz tahminler, bu sunucu için önceden oluşturulmuş bir listesini döndürür |
> | Eylem | Microsoft.Sql/servers/elasticPoolEstimates/write | Sağlanan veritabanlarının bir listesi için yeni bir esnek havuz tahmini oluşturur |
> | Eylem | Microsoft.Sql/servers/elasticPools/advisors/read | Elastik havuz için kullanılabilen danışmanlar listesini döndürür |
> | Eylem | Microsoft.Sql/servers/elasticPools/advisors/recommendedActions/read | Esnek havuz için belirtilen Danışmanı, önerilen eylemlerin listesini döndürür |
> | Eylem | Microsoft.Sql/servers/elasticPools/advisors/recommendedActions/write | Elastik havuzda önerilen eylemi uygulayın |
> | Eylem | Microsoft.Sql/servers/elasticPools/advisors/write | Güncelleştirme otomatik yürütme elastik havuz düzeyi üzerinde bir danışman durumu. |
> | Eylem | Microsoft.Sql/servers/elasticPools/databases/read | Elastik havuz için veritabanlarının listesini alır |
> | Eylem | Microsoft.Sql/servers/elasticPools/delete | Var olan esnek havuzun Sil |
> | Eylem | Microsoft.Sql/servers/elasticPools/elasticPoolActivity/read | Etkinlikleri ve belirli bir esnek veritabanı havuzu ayrıntıları alınamıyor |
> | Eylem | Microsoft.Sql/servers/elasticPools/elasticPoolDatabaseActivity/read | Etkinlikleri ve esnek veritabanı havuzunun parçası olan belirli bir veritabanı hakkında bilgi alma |
> | Eylem | Microsoft.Sql/servers/elasticPools/failover/action | Müşteri, elastik havuz yük devretme başlatıldı. |
> | Eylem | Microsoft.Sql/servers/elasticPools/metricDefinitions/read | Dönüş türleri esnek veritabanı havuzları için kullanılabilir ölçümleri |
> | Eylem | Microsoft.Sql/servers/elasticPools/metrics/read | Esnek veritabanı havuzları için dönüş ölçümleri |
> | Eylem | Microsoft.Sql/servers/elasticPools/operations/cancel/action | Azure SQL elastik havuzu bekleyen henüz tamamlanmadı zaman uyumsuz işlemi iptal eder. |
> | Eylem | Microsoft.Sql/servers/elasticPools/operations/read | Elastik havuz üzerinde gerçekleştirilen işlemlerin listesini döndürür |
> | Eylem | Microsoft.Sql/servers/elasticPools/providers/Microsoft.Insights/diagnosticSettings/read | Kaynağın tanılama ayarını alır |
> | Eylem | Microsoft.Sql/servers/elasticPools/providers/Microsoft.Insights/diagnosticSettings/write | Kaynağın tanılama ayarını oluşturur veya güncelleştirir |
> | Eylem | Microsoft.Sql/servers/elasticPools/providers/Microsoft.Insights/metricDefinitions/read | Dönüş türleri esnek veritabanı havuzları için kullanılabilir ölçümleri |
> | Eylem | Microsoft.Sql/servers/elasticPools/read | Belirli bir sunucuda esnek havuz ayrıntılarını alma |
> | Eylem | Microsoft.Sql/servers/elasticPools/skus/read | Elastik havuz için kullanılabilir SKU'ları bir koleksiyonunu alır |
> | Eylem | Microsoft.Sql/servers/elasticPools/write | Yeni oluşturun veya var olan esnek havuzun özelliklerini değiştir |
> | Eylem | Microsoft.Sql/servers/encryptionProtector/read | Sunucu şifreleme koruyucusu listesini döndürür veya özellikleri için belirtilen sunucu şifreleme koruyucusu alır. |
> | Eylem | Microsoft.Sql/servers/encryptionProtector/write | Belirtilen sunucu şifreleme koruyucusu için özelliklerini güncelleştirir. |
> | Eylem | Microsoft.Sql/servers/extendedAuditingSettings/read | Denetim İlkesi belirli bir sunucuda yapılandırılan genişletilmiş sunucusu blob ayrıntılarını alma |
> | Eylem | Microsoft.Sql/servers/extendedAuditingSettings/write | Verilen bir sunucu için genişletilmiş sunucu blob denetimi değiştirme |
> | Eylem | Microsoft.Sql/servers/failoverGroups/delete | Var olan bir yük devretme grubunu siler. |
> | Eylem | Microsoft.Sql/servers/failoverGroups/failover/action | Planlı yük devretme, mevcut bir yük devretme grubunda yürütür. |
> | Eylem | Microsoft.Sql/servers/failoverGroups/forceFailoverAllowDataLoss/action | Varolan bir yük devretme grubu zorlamalı yük devretme yürütür. |
> | Eylem | Microsoft.Sql/servers/failoverGroups/read | Yük devretme listesini gruplar veya belirtilen bir yük devretme grubu için özellikleri alır döndürür. |
> | Eylem | Microsoft.Sql/servers/failoverGroups/write | Belirtilen parametrelerle bir yük devretme grubu oluşturur veya özellikleri veya etiketleri belirtilen yük devretme grubu için güncelleştirir. |
> | Eylem | Microsoft.Sql/servers/firewallRules/delete | Mevcut bir sunucu güvenlik duvarı kuralını siler. |
> | Eylem | Microsoft.Sql/servers/firewallRules/read | Sunucu güvenlik duvarı kuralları veya özellikleri için belirtilen sunucu güvenlik duvarı kuralını alır. döndürür. |
> | Eylem | Microsoft.Sql/servers/firewallRules/write | Sunucu güvenlik duvarı kuralı oluşturulmaktadır belirtilen parametrelerle belirtilen kuralının özelliklerini güncelleştirir veya tüm mevcut kuralları ile yeni sunucu güvenlik duvarı kuralları üzerine yazma. |
> | Eylem | Microsoft.Sql/servers/import/action | Sunucuda yeni bir veritabanı oluşturun ve şemasının ve verilerin, bir DacPac paketi dağıtma |
> | Eylem | Microsoft.Sql/servers/importExportOperationResults/read | İlerleme-içeri/dışarı aktarma işlemleri alır |
> | Eylem | Microsoft.Sql/servers/interfaceEndpointProfiles/delete | Belirtilen arabirim endpoint profili siler |
> | Eylem | Microsoft.Sql/servers/interfaceEndpointProfiles/read | Belirtilen arabirim endpoint profili için özellikleri döndürür |
> | Eylem | Microsoft.Sql/servers/interfaceEndpointProfiles/write | Belirtilen parametrelerle bir arabirim endpoint profili oluşturur veya özellikleri veya etiketleri belirtilen arabirim uç noktası için güncelleştirir |
> | Eylem | Microsoft.Sql/servers/jobAgents/delete | Bir Azure SQL DB İş Aracısı siler |
> | Eylem | Microsoft.Sql/servers/jobAgents/read | Bir Azure SQL DB İş Aracısı alır |
> | Eylem | Microsoft.Sql/servers/jobAgents/write | Oluşturur veya bir Azure SQL DB İş Aracısı güncelleştirir |
> | Eylem | Microsoft.Sql/servers/keys/delete | Mevcut bir sunucu anahtarı siler. |
> | Eylem | Microsoft.Sql/servers/keys/read | Dönüş sunucusu listesini anahtarları veya belirtilen sunucu anahtarı için özellikleri alır. |
> | Eylem | Microsoft.Sql/servers/keys/write | Belirtilen parametrelerle bir anahtar oluşturur veya özellikleri veya etiketleri belirtilen sunucu anahtarı için güncelleştirin. |
> | Eylem | Microsoft.Sql/servers/operationResults/read | İlerleme-sunucu işlemleri alır |
> | Eylem | Microsoft.Sql/servers/privateEndpointConnectionProxies/delete | Var olan bir özel uç nokta bağlantı Ara sunucusunu siler |
> | Eylem | Microsoft.Sql/servers/privateEndpointConnectionProxies/read | Bağlantı proxy'leri özel uç nokta listesini döndürür veya belirtilen özel uç nokta bağlantı proxy için özellikleri alır. |
> | Eylem | Microsoft.Sql/servers/privateEndpointConnectionProxies/validate/action | Özel doğrulama uç noktası bağlantısını NRP taraftan çağrı oluşturma |
> | Eylem | Microsoft.Sql/servers/privateEndpointConnectionProxies/write | Özel uç nokta bağlantı proxy belirtilen parametrelerle oluşturur veya belirtilen özel uç nokta bağlantı proxy özellikleri veya etiketleri güncelleştirir. |
> | Eylem | Microsoft.Sql/servers/privateEndpointConnections/delete | Var olan bir özel uç noktası bağlantısını siler |
> | Eylem | Microsoft.Sql/servers/privateEndpointConnections/read | Özel uç nokta bağlantı listesinde döndürür veya belirtilen özel uç nokta bağlantı için özellikleri alır. |
> | Eylem | Microsoft.Sql/servers/privateEndpointConnections/write | Onaylar veya reddeder var olan bir özel uç nokta bağlantısı |
> | Eylem | Microsoft.Sql/servers/privateLinkResources/read | Karşılık gelen sql server için özel bir bağlantı kaynakları edinin |
> | Eylem | Microsoft.Sql/servers/providers/Microsoft.Insights/metricDefinitions/read | Dönüş türleri sunucuları için kullanılabilir ölçümleri |
> | Eylem | Microsoft.Sql/servers/read | Sunucuları veya belirtilen sunucunun özelliklerini alır listesini döndürür. |
> | Eylem | Microsoft.Sql/servers/recommendedElasticPools/databases/read | Verilen bir sunucu için önerilen esnek veritabanı havuzlarına ilişkin ölçümleri alma |
> | Eylem | Microsoft.Sql/servers/recommendedElasticPools/read | Maliyeti azaltın veya geçmiş kaynak kullanımını temel alan performansını artırmak için elastik veritabanı havuzları için öneri alın |
> | Eylem | Microsoft.Sql/servers/recoverableDatabases/read | Bu işlem, Canlı veritabanı olağanüstü durum kurtarma için bilinen son iyi yedekleme noktaya veritabanını geri yüklemek için kullanılır. Bilgiler döndüren son iyi yedeklemeden ancak hakkında doesn\u0027t gerçekten veritabanını geri yükle. |
> | Eylem | Microsoft.Sql/servers/replicationLinks/read | Return çoğaltma listesini bağlantılar veya özellikleri için belirtilen Çoğaltma bağlantılarını alır. |
> | Eylem | Microsoft.Sql/servers/restorableDroppedDatabases/read | Belirli bir sunucuda bırakılan hala bekletme ilkesi içinde olan veritabanlarının listesini alın. |
> | Eylem | Microsoft.Sql/servers/securityAlertPolicies/operationResults/read | Sunucu tehdit algılama İlkesi yazma işlemi sonuçlarını Al |
> | Eylem | Microsoft.Sql/servers/securityAlertPolicies/read | Belirli bir sunucu için yapılandırılmış sunucu tehdit algılama ilkelerinin bir listesini alma |
> | Eylem | Microsoft.Sql/servers/securityAlertPolicies/write | Verilen bir sunucu için sunucu tehdit algılama ilkesini değiştirme |
> | Eylem | Microsoft.Sql/servers/serviceObjectives/read | Hizmet düzeyi hedeflerini (performans katmanları olarak da bilinir) belirli bir sunucuda bulunan bir listesini alın |
> | Eylem | Microsoft.Sql/servers/syncAgents/delete | Mevcut bir eşitleme Aracısı siler. |
> | Eylem | Microsoft.Sql/servers/syncAgents/generateKey/action | Eşitleme aracısı kayıt anahtarı oluşturun |
> | Eylem | Microsoft.Sql/servers/syncAgents/linkedDatabases/read | Eşitleme Aracısı bağlı veritabanlarının listesini döndürür |
> | Eylem | Microsoft.Sql/servers/syncAgents/read | Belirtilen eşitleme Aracısı eşitleme aracıları veya özelliklerini alır listesini döndürür. |
> | Eylem | Microsoft.Sql/servers/syncAgents/write | Belirtilen parametrelerle bir eşitleme aracısı oluşturur veya belirtilen eşitleme Aracısı özelliklerini güncelleştirir. |
> | Eylem | Microsoft.Sql/servers/tdeCertificates/action | Oluşturma/güncelleştirme TDE sertifika |
> | Eylem | Microsoft.Sql/servers/usages/read | DTU kotası dönüş sunucu ve sunucu içindeki tüm veritabanlarına göre geçerli DTU tüketim |
> | Eylem | Microsoft.Sql/servers/virtualNetworkRules/delete | Mevcut bir sanal ağ kuralı siler |
> | Eylem | Microsoft.Sql/servers/virtualNetworkRules/read | Dönüş listesi, sanal ağ kuralları veya belirtilen sanal ağ kuralı için özellikleri alır. |
> | Eylem | Microsoft.Sql/servers/virtualNetworkRules/write | Bir sanal ağ kuralı belirtilen parametrelerle oluşturur veya özellikleri veya etiketleri belirtilen sanal ağ kuralı için güncelleştirin. |
> | Eylem | Microsoft.Sql/servers/vulnerabilityAssessments/delete | Verilen bir sunucu için güvenlik açığı değerlendirmesi Kaldır |
> | Eylem | Microsoft.Sql/servers/vulnerabilityAssessments/read | Belirli bir sunucu üzerindeki güvenlik açığı değerlendirmesi ilkeleri alma |
> | Eylem | Microsoft.Sql/servers/vulnerabilityAssessments/write | Verilen bir sunucu için güvenlik açığı değerlendirmesi değiştirme |
> | Eylem | Microsoft.Sql/servers/write | Belirtilen parametrelerle bir sunucu oluşturur veya özellikleri veya etiketleri belirtilen sunucu için güncelleştirin. |
> | Eylem | Microsoft.Sql/unregister/action | Microsoft SQL veritabanı kaynak sağlayıcısı için aboneliği kaydını siler ve Microsoft SQL veritabanları oluşturulmasını sağlar. |
> | Eylem | Microsoft.Sql/virtualClusters/delete | Mevcut bir sanal küme siler. |
> | Eylem | Microsoft.Sql/virtualClusters/read | Belirtilen sanal küme özelliklerini alır ya da Sanal kümelerin listesini döndürür. |
> | Eylem | Microsoft.Sql/virtualClusters/write | Sanal küme etiketleri güncelleştirir. |

## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Storage/checknameavailability/read | Bu hesap adının geçerli olduğundan ve kullanılmadığını denetler. |
> | Eylem | Microsoft.Storage/locations/checknameavailability/read | Bu hesap adının geçerli olduğundan ve kullanılmadığını denetler. |
> | Eylem | Microsoft.Storage/locations/deleteVirtualNetworkOrSubnets/action | Microsoft.Storage sanal ağın veya alt ağın silindiğini bildirir |
> | Eylem | Microsoft.Storage/locations/usages/read | Belirtilen Abonelikteki sınırı ve kaynaklar için geçerli kullanım sayısını döndürür |
> | Eylem | Microsoft.Storage/operations/read | Zaman uyumsuz bir işlemin durumunu yoklar. |
> | Eylem | Microsoft.Storage/register/action | Depolama kaynak sağlayıcısı için aboneliği kaydeder ve depolama hesaplarının oluşturulmasını etkinleştirir. |
> | Eylem | Microsoft.Storage/skus/read | Microsoft.Storage tarafından desteklenen SKU'ları listeler. |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/add/action | Blob içeriği eklemenin sonucunu döndürür |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete | Bir blobun silinmesinin sonucunu döndürür |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/deleteAutomaticSnapshot/action | Otomatik bir anlık görüntü silinmesinin sonucunu döndürür |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/filter/action | Eşleşen etiketler Filtresi ile bir hesap altında BLOB listesini döndürür |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read | Bir blobu veya BLOB listesini döndürür |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/tags/read | Blob etiketleri okuma sonucunu döndürür |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/tags/write | Blob etiketleri yazmanın sonucunu döndürür |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write | Bir blobu yazmanın sonucunu döndürür |
> | Eylem | Microsoft.Storage/storageAccounts/blobServices/containers/clearLegalHold/action | Düz bir blob kapsayıcısı yasal tutmasını |
> | Eylem | Microsoft.Storage/storageAccounts/blobServices/containers/delete | Bir kapsayıcının silinmesinin sonucunu döndürür |
> | Eylem | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/delete | BLOB kapsayıcısı değiştirilemezlik ilkesini Sil |
> | Eylem | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/extend/action | BLOB kapsayıcısı değiştirilemezlik ilkesini Genişlet |
> | Eylem | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/lock/action | Kilit blob kapsayıcısı değiştirilemezlik ilkesini |
> | Eylem | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/read | BLOB kapsayıcısı değiştirilemezlik ilkesini Al |
> | Eylem | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/write | BLOB kapsayıcısı değitirilemezlik İlkesi koy |
> | Eylem | Microsoft.Storage/storageAccounts/blobServices/containers/lease/action | Blob kapsayıcısını kiralama sonucunu döndürür |
> | Eylem | Microsoft.Storage/storageAccounts/blobServices/containers/read | Bir kapsayıcı döndürür |
> | Eylem | Microsoft.Storage/storageAccounts/blobServices/containers/read | Kapsayıcı listesini döndürür |
> | Eylem | Microsoft.Storage/storageAccounts/blobServices/containers/setLegalHold/action | BLOB kapsayıcısı yasal tutma Ayarla |
> | Eylem | Microsoft.Storage/storageAccounts/blobServices/containers/write | Düzeltme eki blob kapsayıcısı sonucunu döndürür |
> | Eylem | Microsoft.Storage/storageAccounts/blobServices/containers/write | Put blob kapsayıcısı sonucunu döndürür |
> | Eylem | Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey/action | Blob hizmeti için bir kullanıcı temsilcisi anahtarı döndürür |
> | Eylem | Microsoft.Storage/storageAccounts/blobServices/read | Hizmeti özelliklerini veya istatistiklerini döndürür blob |
> | Eylem | Microsoft.Storage/storageAccounts/blobServices/write | Hizmeti özelliklerini koy blob sonucunu döndürür |
> | Eylem | Microsoft.Storage/storageAccounts/delete | Mevcut bir depolama hesabını siler. |
> | Eylem | Microsoft.Storage/storageAccounts/failover/action | Müşteri yük devretme kullanılabilirlik sorunları yaşanması denetleyebilirsiniz. |
> | DataAction | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/actassuperuser/action |  |
> | DataAction | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/delete |  |
> | DataAction | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/modifypermissions/action |  |
> | DataAction | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/read |  |
> | DataAction | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/write |  |
> | Eylem | Microsoft.Storage/storageAccounts/fileServices/read | Dosya hizmeti özelliklerini alma |
> | Eylem | Microsoft.Storage/storageAccounts/listAccountSas/action | Belirtilen depolama hesabı için hesap SAS belirtecini döndürür. |
> | Eylem | Microsoft.Storage/storageAccounts/listkeys/action | Belirtilen depolama hesabının erişim anahtarlarını döndürür. |
> | Eylem | Microsoft.Storage/storageAccounts/listServiceSas/action | Belirtilen depolama hesabı için hizmet SAS belirtecini döndürür. |
> | Eylem | Microsoft.Storage/storageAccounts/managementPolicies/delete | Depolama hesabı yönetim ilkelerini Sil |
> | Eylem | Microsoft.Storage/storageAccounts/managementPolicies/read | Depolama yönetim hesabı ilkelerini alın |
> | Eylem | Microsoft.Storage/storageAccounts/managementPolicies/write | Depolama hesabı yönetim ilkelerini yürürlüğe koy |
> | Eylem | Microsoft.Storage/storageAccounts/privateEndpointConnectionProxies/delete | Özel uç nokta bağlantı proxy silin |
> | Eylem | Microsoft.Storage/storageAccounts/privateEndpointConnectionProxies/validate/action | Özel uç nokta bağlantı proxy'leri doğrula |
> | Eylem | Microsoft.Storage/storageAccounts/privateEndpointConnectionProxies/write | Özel uç nokta bağlantı proxy'leri yerleştirme |
> | Eylem | Microsoft.Storage/storageAccounts/privateEndpointConnections/delete | Özel uç noktası bağlantısını Sil |
> | Eylem | Microsoft.Storage/storageAccounts/privateEndpointConnections/read | Özel uç noktası bağlantısını Al |
> | Eylem | Microsoft.Storage/storageAccounts/privateEndpointConnections/write | Özel uç noktası bağlantısını yerleştirin |
> | Eylem | Microsoft.Storage/storageAccounts/privateLinkResources/read | Depolama hesabı grup kimlikleri alma |
> | Eylem | Microsoft.Storage/storageAccounts/queueServices/queues/delete | Bir kuyruğu silmenin sonucunu döndürür |
> | DataAction | Microsoft.Storage/storageAccounts/queueServices/queues/messages/add/action | Bir ileti eklemenin sonucunu döndürür |
> | DataAction | Microsoft.Storage/storageAccounts/queueServices/queues/messages/delete | Bir iletiyi silmenin sonucunu döndürür |
> | DataAction | Microsoft.Storage/storageAccounts/queueServices/queues/messages/process/action | Bir iletiyi işlemenin sonucunu döndürür |
> | DataAction | Microsoft.Storage/storageAccounts/queueServices/queues/messages/read | Bir ileti döndürür |
> | DataAction | Microsoft.Storage/storageAccounts/queueServices/queues/messages/write | Bir iletiyi yazmanın sonucunu döndürür |
> | Eylem | Microsoft.Storage/storageAccounts/queueServices/queues/read | Bir kuyruğu veya kuyruk listesini döndürür. |
> | Eylem | Microsoft.Storage/storageAccounts/queueServices/queues/write | Bir kuyruğu yazmanın sonucunu döndürür |
> | Eylem | Microsoft.Storage/storageAccounts/queueServices/read | Kuyruk hizmeti özelliklerini alma |
> | Eylem | Microsoft.Storage/storageAccounts/queueServices/read | Hizmeti özelliklerini veya istatistiklerini döndürür kuyruk. |
> | Eylem | Microsoft.Storage/storageAccounts/queueServices/write | Kuyruk hizmeti özelliklerini ayarlamanın sonucunu döndürür |
> | Eylem | Microsoft.Storage/storageAccounts/read | Depolama hesapları veya belirtilen depolama hesabının özelliklerini alır döndürür. |
> | Eylem | Microsoft.Storage/storageAccounts/regeneratekey/action | Belirtilen depolama hesabının erişim anahtarlarını yeniden oluşturur. |
> | Eylem | Microsoft.Storage/storageAccounts/restoreBlobRanges/action | Blob'u aralıklarını belirtilen süre durumunu geri yükle |
> | Eylem | Microsoft.Storage/storageAccounts/revokeUserDelegationKeys/action | Belirtilen depolama hesabı için tüm kullanıcı temsilcisi anahtarlar iptal eder. |
> | Eylem | Microsoft.Storage/storageAccounts/services/diagnosticSettings/write | Depolama hesabı tanılama ayarlarını oluşturun/güncelleştirin. |
> | Eylem | Microsoft.Storage/storageAccounts/tableServices/read | Tablo hizmeti özelliklerini alma |
> | Eylem | Microsoft.Storage/storageAccounts/write | Belirtilen parametreleri veya güncelleştirme özellikleri veya etiketleri ile bir depolama hesabı oluşturur veya belirtilen depolama hesabının özel etki alanı ekler. |
> | Eylem | Microsoft.Storage/usages/read | Belirtilen Abonelikteki sınırı ve kaynaklar için geçerli kullanım sayısını döndürür |

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | microsoft.storagesync/locations/checkNameAvailability/action | Bu depolama eşitleme hizmeti adı geçerliyse ve kullanılmadığını denetler. |
> | Eylem | Microsoft.storagesync/Locations/workflows/Operations/Read | Zaman uyumsuz bir işlemin durumunu alır |
> | Eylem | microsoft.storagesync/storageSyncServices/delete | Herhangi bir depolama eşitleme hizmeti silme |
> | Eylem | microsoft.storagesync/storageSyncServices/providers/Microsoft.Insights/metricDefinitions/read | Depolama Eşitleme Hizmetleri için kullanılabilir ölçümleri alır |
> | Eylem | microsoft.storagesync/storageSyncServices/read | Herhangi bir depolama eşitleme hizmeti okuyun |
> | Eylem | microsoft.storagesync/storageSyncServices/registeredServers/delete | Tüm kayıtlı sunucuyu silme |
> | Eylem | microsoft.storagesync/storageSyncServices/registeredServers/providers/Microsoft.Insights/metricDefinitions/read | Kayıtlı sunucu için kullanılabilir ölçümleri alır |
> | Eylem | microsoft.storagesync/storageSyncServices/registeredServers/read | Herhangi bir kayıtlı sunucu okuyun |
> | Eylem | microsoft.storagesync/storageSyncServices/registeredServers/write | Herhangi bir kayıtlı sunucu güncelle |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/delete | Tüm bulut uç noktalarını silme |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/operationresults/read | Zaman uyumsuz yedekleme/geri yükleme işleminin durumunu alır |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/postbackup/action | Yedeklemeden sonra bu eylem çağrısı |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/postrestore/action | Bu eylemi geri çağırma |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/prebackup/action | Yedekleme önce bu eylem çağrısı |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/prerestore/action | Bu eylem geri önce çağırın |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/read | Tüm bulut uç noktaları okuyun |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/restoreheartbeat/action | Sinyal geri yükleme |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/write | Tüm bulut uç noktalarını güncelle |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/delete | Tüm eşitleme gruplarını silin |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/providers/Microsoft.Insights/metricDefinitions/read | Eşitleme grupları için kullanılabilir ölçümleri alır |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/read | Hiç eşitleme grubunuz okuyun |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/delete | Tüm sunucu uç noktalarını silme |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/providers/Microsoft.Insights/metricDefinitions/read | Sunucu uç noktaları için kullanılabilir ölçümleri alır |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/read | Tüm sunucu uç noktaları okuyun |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/recallAction/action | Dosyaları bir sunucuya geri çekmek için bu eylem çağrısı |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/write | Tüm sunucu uç noktalarını güncelle |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/write | Tüm eşitleme gruplarını oluştur veya güncelleştir |
> | Eylem | microsoft.storagesync/storageSyncServices/workflows/operationresults/read | Zaman uyumsuz bir işlemin durumunu alır |
> | Eylem | microsoft.storagesync/storageSyncServices/workflows/operations/read | Zaman uyumsuz bir işlemin durumunu alır |
> | Eylem | microsoft.storagesync/storageSyncServices/workflows/read | İş akışları okuma |
> | Eylem | microsoft.storagesync/storageSyncServices/write | Tüm depolama Eşitleme Hizmetleri güncelle |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.StorSimple/managers/accessControlRecords/delete | Erişim denetimi kayıtları siler |
> | Eylem | Microsoft.StorSimple/managers/accessControlRecords/operationResults/read | İşlem sonuçlarını alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/accessControlRecords/read | Erişim denetimi kayıtları alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/accessControlRecords/write | Erişim denetimi kayıtları oluştur veya güncelleştir |
> | Eylem | Microsoft.StorSimple/managers/alerts/read | Uyarıları alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/backups/read | Yedekleme kümesini alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/bandwidthSettings/delete | Var olan bir bant genişliği ayarlarını siler (yalnızca 8000 serisi) |
> | Eylem | Microsoft.StorSimple/managers/bandwidthSettings/operationResults/read | İşlem sonuçları listesi |
> | Eylem | Microsoft.StorSimple/managers/bandwidthSettings/read | Bant genişliği ayarlarını listesi (yalnızca 8000 serisi) |
> | Eylem | Microsoft.StorSimple/managers/bandwidthSettings/write | Yeni bir oluşturur veya bant genişliği ayarlarını güncelleştirir (yalnızca 8000 serisi) |
> | Eylem | Microsoft.StorSimple/managers/certificates/write | Sertifikaları oluşturun veya güncelleştirin |
> | Eylem | Microsoft.StorSimple/Managers/certificates/write | Kaynak sertifikayı Güncelleştir işlemi, kaynak/kasa kimlik bilgisi sertifikasını güncelleştirir. |
> | Eylem | Microsoft.StorSimple/managers/clearAlerts/action | Cihaz Yöneticisi ile ilişkili tüm uyarılar temizleyin. |
> | Eylem | Microsoft.StorSimple/managers/cloudApplianceConfigurations/read | Bulut Gereci liste desteklenen yapılandırmalar |
> | Eylem | Microsoft.StorSimple/managers/configureDevice/action | Bir cihaz yapılandırır |
> | Eylem | Microsoft.StorSimple/managers/delete | Cihaz yöneticileri siler |
> | Eylem | Microsoft.StorSimple/Managers/delete | Kasayı Sil işlemi, 'vault' türündeki belirtilen Azure kaynağını siler |
> | Eylem | Microsoft.StorSimple/managers/devices/alertSettings/operationResults/read | İşlem sonuçlarını alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/alertSettings/read | Uyarı ayarlarını alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/alertSettings/write | Uyarı ayarlarını güncelle |
> | Eylem | Microsoft.StorSimple/managers/devices/authorizeForServiceEncryptionKeyRollover/action | Hizmet için şifreleme anahtar geçişi cihazların Yetkilendir |
> | Eylem | Microsoft.StorSimple/managers/devices/backupPolicies/backup/action | İsteğe bağlı bir oluşturmak için el ile yedekleme İlkesi tarafından korunan tüm birimlerin yedeğini alın. |
> | Eylem | Microsoft.StorSimple/managers/devices/backupPolicies/delete | Mevcut yedekleme ilkelerini siler (yalnızca 8000 serisi) |
> | Eylem | Microsoft.StorSimple/managers/devices/backupPolicies/operationResults/read | İşlem sonuçları listesi |
> | Eylem | Microsoft.StorSimple/managers/devices/backupPolicies/read | (Yalnızca 8000 serisi) listesini yedekleme ilkeleri |
> | Eylem | Microsoft.StorSimple/managers/devices/backupPolicies/schedules/delete | Mevcut bir zamanlamalar siler |
> | Eylem | Microsoft.StorSimple/managers/devices/backupPolicies/schedules/operationResults/read | İşlem sonuçları listesi |
> | Eylem | Microsoft.StorSimple/managers/devices/backupPolicies/schedules/read | Tabloları listeleyin |
> | Eylem | Microsoft.StorSimple/managers/devices/backupPolicies/schedules/write | Yeni bir oluşturur veya güncelleştirir zamanlamaları |
> | Eylem | Microsoft.StorSimple/managers/devices/backupPolicies/write | Yeni bir oluşturur veya yedekleme ilkeleri güncelleştirmeleri (yalnızca 8000 serisi) |
> | Eylem | Microsoft.StorSimple/managers/devices/backups/delete | Yedek kümesini siler |
> | Eylem | Microsoft.StorSimple/managers/devices/backups/elements/clone/action | Bir paylaşımı veya birimi yedekleme öğesi kullanarak kopyalayın. |
> | Eylem | Microsoft.StorSimple/managers/devices/backups/elements/operationResults/read | İşlem sonuçlarını alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/backups/operationResults/read | İşlem sonuçlarını alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/backups/read | Yedekleme kümesini alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/backups/restore/action | Tüm birimleri, bir yedekleme kümesinden geri yükleyin. |
> | Eylem | Microsoft.StorSimple/managers/devices/backupScheduleGroups/delete | Yedekleme Zamanlama grupları siler |
> | Eylem | Microsoft.StorSimple/managers/devices/backupScheduleGroups/operationResults/read | İşlem sonuçlarını alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/backupScheduleGroups/read | Yedekleme Zamanlama grupları alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/backupScheduleGroups/write | Yedekleme Zamanlama grupları oluştur veya güncelleştir |
> | Eylem | Microsoft.StorSimple/managers/devices/chapSettings/delete | Chap ayarları siler |
> | Eylem | Microsoft.StorSimple/managers/devices/chapSettings/operationResults/read | İşlem sonuçlarını alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/chapSettings/read | Chap ayarları alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/chapSettings/write | Oluşturma veya Chap ayarlarını güncelleştirme |
> | Eylem | Microsoft.StorSimple/managers/devices/deactivate/action | Bir cihazı devre dışı bırakır. |
> | Eylem | Microsoft.StorSimple/managers/devices/delete | Cihazları siler |
> | Eylem | Microsoft.StorSimple/managers/devices/disks/read | Diskleri alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/download/action | Bir cihazı için güncelleştirmeler önerildi. |
> | Eylem | Microsoft.StorSimple/managers/devices/failover/action | Cihaz yük devretme. |
> | Eylem | Microsoft.StorSimple/managers/devices/failover/operationResults/read | İşlem sonuçlarını alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/failoverTargets/read | Cihaz yük devri hedefleri alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/fileservers/backup/action | Bir dosya sunucusunda yedekleme yararlanın. |
> | Eylem | Microsoft.StorSimple/managers/devices/fileservers/delete | Dosya sunucuları siler |
> | Eylem | Microsoft.StorSimple/managers/devices/fileservers/metrics/read | Ölçümleri alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/fileservers/metricsDefinitions/read | Ölçüm tanımlarını alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/fileservers/operationResults/read | İşlem sonuçlarını alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/fileservers/read | Dosya sunucuları alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/fileservers/shares/delete | Paylaşımları siler |
> | Eylem | Microsoft.StorSimple/managers/devices/fileservers/shares/metrics/read | Ölçümleri alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/fileservers/shares/metricsDefinitions/read | Ölçüm tanımlarını alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/fileservers/shares/operationResults/read | İşlem sonuçlarını alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/fileservers/shares/read | Paylaşımları alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/fileservers/shares/write | Paylaşımları güncelle |
> | Eylem | Microsoft.StorSimple/managers/devices/fileservers/write | Dosya sunucuları güncelle |
> | Eylem | Microsoft.StorSimple/managers/devices/hardwareComponentGroups/changeControllerPowerState/action | Denetleyici güç durumu donanım bileşen grubu değiştirme |
> | Eylem | Microsoft.StorSimple/managers/devices/hardwareComponentGroups/operationResults/read | İşlem sonuçları listesi |
> | Eylem | Microsoft.StorSimple/managers/devices/hardwareComponentGroups/read | Donanım bileşeni grupları listeleme |
> | Eylem | Microsoft.StorSimple/managers/devices/install/action | Bir cihazda güncelleştirmeleri yükleyin. |
> | Eylem | Microsoft.StorSimple/managers/devices/installUpdates/action | Cihazlar (yalnızca 8000 serisi) güncelleştirmelerini yükler. |
> | Eylem | Microsoft.StorSimple/managers/devices/iscsiservers/backup/action | Bir iSCSI sunucusu yedek al. |
> | Eylem | Microsoft.StorSimple/managers/devices/iscsiservers/delete | İSCSI sunucuları siler |
> | Eylem | Microsoft.StorSimple/managers/devices/iscsiservers/disks/delete | Diski siler |
> | Eylem | Microsoft.StorSimple/managers/devices/iscsiservers/disks/metrics/read | Ölçümleri alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/iscsiservers/disks/metricsDefinitions/read | Ölçüm tanımlarını alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/iscsiservers/disks/operationResults/read | İşlem sonuçlarını alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/iscsiservers/disks/read | Diskleri alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/iscsiservers/disks/write | Diskleri güncelle |
> | Eylem | Microsoft.StorSimple/managers/devices/iscsiservers/metrics/read | Ölçümleri alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/iscsiservers/metricsDefinitions/read | Ölçüm tanımlarını alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/iscsiservers/operationResults/read | İşlem sonuçlarını alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/iscsiservers/read | İSCSI sunucuları alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/iscsiservers/write | Oluşturma veya iSCSI sunucuları güncelleştirme |
> | Eylem | Microsoft.StorSimple/managers/devices/jobs/cancel/action | Bir çalışan işi iptal et |
> | Eylem | Microsoft.StorSimple/managers/devices/jobs/operationResults/read | İşlem sonuçları listesi |
> | Eylem | Microsoft.StorSimple/managers/devices/jobs/read | İşleri alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/listFailoverSets/action | Var olan bir cihaz (yalnızca 8000 serisi) için yük devretme kümeleri listesi. |
> | Eylem | Microsoft.StorSimple/managers/devices/listFailoverTargets/action | Yük devri hedefleri (yalnızca 8000 serisi) cihazların listesi. |
> | Eylem | Microsoft.StorSimple/managers/devices/metrics/read | Ölçümleri alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/metricsDefinitions/read | Ölçüm tanımlarını alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/confirmMigration/action | Başarılı bir geçiş onaylar ve işleyin. |
> | Eylem | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/confirmMigrationStatus/read | Liste geçiş durumu onaylayın |
> | Eylem | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/fetchConfirmMigrationStatus/action | Geçişi Onayla durumuna getirin. |
> | Eylem | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/fetchMigrationEstimate/action | Durum geçiş tahmini işinin getirin. |
> | Eylem | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/fetchMigrationStatus/action | Geçiş için durum getirin. |
> | Eylem | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/import/action | Geçiş için kaynak yapılandırmaları İçeri Aktar |
> | Eylem | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/migrationEstimate/read | Geçiş tahmin listesi |
> | Eylem | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/migrationStatus/read | Geçiş durumu listesi |
> | Eylem | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/operationResults/read | İşlem sonuçları listesi |
> | Eylem | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/startMigration/action | Kaynak yapılandırmaları kullanarak geçişi Başlat |
> | Eylem | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/startMigrationEstimate/action | Geçiş işleminin süresi tahmin etmek için bir proje başlatın. |
> | Eylem | Microsoft.StorSimple/managers/devices/networkSettings/operationResults/read | İşlem sonuçları listesi |
> | Eylem | Microsoft.StorSimple/managers/devices/networkSettings/read | Ağ ayarlarını alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/networkSettings/write | Yeni bir oluşturur veya ağ ayarlarını güncelleştirir |
> | Eylem | Microsoft.StorSimple/managers/devices/operationResults/read | İşlem sonuçlarını alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/publicEncryptionKey/action | Cihaz Yöneticisi'ni listesini ortak şifreleme anahtarı |
> | Eylem | Microsoft.StorSimple/managers/devices/publishSupportPackage/action | Var olan bir cihaz için destek paketi yayımlayın. StorSimple destek paketi Microsoft Support StorSimple cihaz sorunları gidermeye yardımcı olmak için tüm ilgili günlükleri toplayan bir kullanımı kolay mekanizmasıdır. |
> | Eylem | Microsoft.StorSimple/managers/devices/read | Cihazları alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/scanForUpdates/action | Bir cihaz güncelleştirmeleri için tarama. |
> | Eylem | Microsoft.StorSimple/managers/devices/securitySettings/operationResults/read | İşlem sonuçlarını alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/securitySettings/read | Güvenlik ayarları listesi |
> | Eylem | Microsoft.StorSimple/managers/devices/securitySettings/syncRemoteManagementCertificate/action | Bir cihaz için uzaktan yönetim sertifikası eşitleyin. |
> | Eylem | Microsoft.StorSimple/managers/devices/securitySettings/update/action | Güvenlik ayarlarını güncelleştirin. |
> | Eylem | Microsoft.StorSimple/managers/devices/securitySettings/write | Yeni bir oluşturur veya güvenlik ayarlarını güncelleştirir |
> | Eylem | Microsoft.StorSimple/managers/devices/sendTestAlertEmail/action | Test uyarı e-posta için yapılandırılan e-posta alıcılarını gönderin. |
> | Eylem | Microsoft.StorSimple/managers/devices/shares/read | Paylaşımları alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/timeSettings/operationResults/read | İşlem sonuçları listesi |
> | Eylem | Microsoft.StorSimple/managers/devices/timeSettings/read | Saat ayarlarını alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/timeSettings/write | Yeni bir oluşturur veya saat ayarlarını güncelleştirir |
> | Eylem | Microsoft.StorSimple/managers/devices/updates/operationResults/read | İşlem sonuçlarını alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/updateSummary/read | Güncelleştirme özeti alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/devices/volumeContainers/delete | Var olan bir birim kapsayıcıları siler (yalnızca 8000 serisi) |
> | Eylem | Microsoft.StorSimple/managers/devices/volumeContainers/metrics/read | Ölçümlerin listesi |
> | Eylem | Microsoft.StorSimple/managers/devices/volumeContainers/metricsDefinitions/read | Ölçüm tanımlarını Listele |
> | Eylem | Microsoft.StorSimple/managers/devices/volumeContainers/operationResults/read | İşlem sonuçları listesi |
> | Eylem | Microsoft.StorSimple/managers/devices/volumeContainers/read | Birim kapsayıcıları listesi (yalnızca 8000 serisi) |
> | Eylem | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/delete | Var olan birimler siler |
> | Eylem | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/metrics/read | Ölçümlerin listesi |
> | Eylem | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/metricsDefinitions/read | Ölçüm tanımlarını Listele |
> | Eylem | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/operationResults/read | İşlem sonuçları listesi |
> | Eylem | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/read | Birimleri listesi |
> | Eylem | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/write | Yeni bir oluşturur veya güncelleştirir birimleri |
> | Eylem | Microsoft.StorSimple/managers/devices/volumeContainers/write | Yeni bir oluşturur veya birim kapsayıcıları güncelleştirmeleri (yalnızca 8000 serisi) |
> | Eylem | Microsoft.StorSimple/managers/devices/volumes/read | Birimleri listesi |
> | Eylem | Microsoft.StorSimple/managers/devices/write | Cihazları güncelle |
> | Eylem | Microsoft.StorSimple/managers/encryptionSettings/read | Şifreleme ayarları alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/extendedInformation/delete | Genişletilmiş kasa bilgileri siler |
> | Eylem | Microsoft.StorSimple/Managers/extendedInformation/delete | Daha Fazla Bilgi Al işlemi, ?vault? türündeki Azure kaynağını temsil eden nesnenin Daha Fazla Bilgi değerini alır. |
> | Eylem | Microsoft.StorSimple/managers/extendedInformation/read | Genişletilmiş kasa bilgileri alır veya listeler |
> | Eylem | Microsoft.StorSimple/Managers/extendedInformation/read | Daha Fazla Bilgi Al işlemi, ?vault? türündeki Azure kaynağını temsil eden nesnenin Daha Fazla Bilgi değerini alır. |
> | Eylem | Microsoft.StorSimple/managers/extendedInformation/write | Genişletilmiş kasa bilgileri oluştur veya güncelleştir |
> | Eylem | Microsoft.StorSimple/Managers/extendedInformation/write | Daha Fazla Bilgi Al işlemi, ?vault? türündeki Azure kaynağını temsil eden nesnenin Daha Fazla Bilgi değerini alır. |
> | Eylem | Microsoft.StorSimple/managers/features/read | Özellikler listesi |
> | Eylem | Microsoft.StorSimple/managers/fileservers/read | Dosya sunucuları alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/getEncryptionKey/action | Şifreleme anahtarı için Aygıt Yöneticisi'ni alma. |
> | Eylem | Microsoft.StorSimple/managers/iscsiservers/read | İSCSI sunucuları alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/jobs/read | İşleri alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/listActivationKey/action | StorSimple cihaz Yöneticisi'nin etkinleştirme anahtarı alır. |
> | Eylem | Microsoft.StorSimple/managers/listPublicEncryptionKey/action | Bir StorSimple cihaz Yöneticisi'nin genel şifreleme anahtarlarını listele. |
> | Eylem | Microsoft.StorSimple/managers/metrics/read | Ölçümleri alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/metricsDefinitions/read | Ölçüm tanımlarını alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/migrateClassicToResourceManager/action | Klasik Mangers, Resource Manager'a geçiş |
> | Eylem | Microsoft.StorSimple/managers/migrationSourceConfigurations/read | Geçiş kaynak yapılandırmaları listesi (yalnızca 8000 serisi) |
> | Eylem | Microsoft.StorSimple/managers/operationResults/read | İşlem sonuçlarını alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/provisionCloudAppliance/action | Yeni bulut gerecini oluşturun. |
> | Eylem | Microsoft.StorSimple/managers/read | Cihaz Yöneticileri alır veya listeler |
> | Eylem | Microsoft.StorSimple/Managers/read | Kasayı Al işlemi 'vault' türündeki Azure kaynağını temsil eden bir nesne alır. |
> | Eylem | Microsoft.StorSimple/managers/regenerateActivationKey/action | Etkinleştirme anahtarı yeniden oluşturmak için mevcut bir StorSimple cihaz Yöneticisi. |
> | Eylem | Microsoft.StorSimple/managers/storageAccountCredentials/delete | Depolama hesabı kimlik bilgilerini siler |
> | Eylem | Microsoft.StorSimple/managers/storageAccountCredentials/operationResults/read | İşlem sonuçlarını alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/storageAccountCredentials/read | Depolama hesabı kimlik bilgilerini alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/storageAccountCredentials/write | Oluşturun veya depolama hesabı kimlik bilgilerini güncelleştirin |
> | Eylem | Microsoft.StorSimple/managers/storageDomains/delete | Depolama etki alanlarını siler |
> | Eylem | Microsoft.StorSimple/managers/storageDomains/operationResults/read | İşlem sonuçlarını alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/storageDomains/read | Depolama etki alanları alır veya listeler |
> | Eylem | Microsoft.StorSimple/managers/storageDomains/write | Depolama etki alanlarını güncelle |
> | Eylem | Microsoft.StorSimple/managers/write | Cihaz yöneticileri güncelle |
> | Eylem | Microsoft.StorSimple/Managers/write | Kasa Oluştur işlemi, 'vault' türünde bir Azure kaynağı oluşturur |
> | Eylem | Microsoft.StorSimple/operations/read | İşlemleri alır veya listeler |
> | Eylem | Microsoft.StorSimple/register/action | Sağlayıcı Microsoft.StorSimple kaydetme |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.StreamAnalytics/locations/quotas/Read | Okuma Stream Analytics abonelik kotası |
> | Eylem | Microsoft.StreamAnalytics/operations/Read | Okuma Stream Analytics işlemleri |
> | Eylem | Microsoft.StreamAnalytics/Register/action | Stream Analytics kaynak sağlayıcısı ile abonelik kaydedin |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/Delete | Stream Analytics işini sil |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/functions/Delete | Stream Analytics iş işlevi Sil |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/functions/operationresults/Read | Stream Analytics işi yapmak için işlem sonuçlarını oku |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/functions/Read | Okuma Stream Analytics iş işlevi |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/functions/RetrieveDefaultDefinition/action | Stream Analytics iş işlevi öğenin varsayılan tanımını Al |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/functions/Test/action | Test Stream Analytics iş işlevi |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/functions/Write | Stream Analytics iş işlevi yazma |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/inputs/Delete | Stream Analytics işi girişi Sil |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/inputs/operationresults/Read | Stream Analytics işi girişi için işlem sonuçlarını oku |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/inputs/Read | Okuma Stream Analytics işi girişi |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/inputs/Sample/action | Örnek Stream Analytics işi giriş |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/inputs/Test/action | Test Stream Analytics işi girişi |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/inputs/Write | Stream Analytics işi girişi yazma |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/metricdefinitions/Read | Ölçüm tanımlarını oku |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/operationresults/Read | Stream Analytics işi için işlem sonuçlarını oku |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/outputs/Delete | Stream Analytics işi çıktı Sil |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/outputs/operationresults/Read | Stream Analytics işi çıktı için işlem sonuçlarını oku |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/outputs/Read | Okuma Stream Analytics işi çıktı |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/outputs/Test/action | Test Stream Analytics işi çıkışı |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/outputs/Write | Stream Analytics işi çıktı yazma |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/providers/Microsoft.Insights/diagnosticSettings/read | Tanılama ayarını oku. |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/providers/Microsoft.Insights/diagnosticSettings/write | Tanılama ayarını yaz. |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/providers/Microsoft.Insights/logDefinitions/read | Streamingjobs için mevcut günlükleri alır |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/providers/Microsoft.Insights/metricDefinitions/read | Streamingjobs için kullanılabilir ölçümleri alır |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/Read | Stream Analytics işi okuma |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/Start/action | Stream Analytics işini başlatın |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/Stop/action | Stream Analytics işini durdurma |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/transformations/Delete | Stream Analytics iş dönüşümü Sil |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/transformations/Read | Okuma Stream Analytics iş dönüşümü |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/transformations/Write | Stream Analytics iş dönüşümü yazma |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/Write | Stream Analytics işi yazma |

## <a name="microsoftsubscription"></a>Microsoft.Subscription

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Subscription/CreateSubscription/action | Bir Azure aboneliği oluşturun |
> | Eylem | Microsoft.Subscription/register/action | Aboneliği Microsoft.Subscription kaynak sağlayıcısına kaydeder |
> | Eylem | Microsoft.Subscription/SubscriptionDefinitions/read | Bir yönetim grubu içinde bir Azure aboneliği tanımı Al |
> | Eylem | Microsoft.Subscription/SubscriptionDefinitions/write | Bir Azure aboneliği tanımı oluşturun |

## <a name="microsoftsupport"></a>Microsoft.Support

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Support/register/action | Destek Kaynağı Sağlayıcısı'na kayıt yapar |
> | Eylem | Microsoft.Support/supportTickets/read | Durum, önem derecesi, kişi ayrıntıları ve iletişimler gibi Destek Biletleri ayrıntılarını alır veya aboneliklerdeki Destek Biletleri listesini alır. |
> | Eylem | Microsoft.Support/supportTickets/write | Destek Bileti oluşturur veya güncelleştirir. Teknik, Faturalama, Kotalar veya Abonelik Yönetimi konusunda bir Destek Bileti oluşturabilirsiniz. Var olan destek biletlerinin önem derecesini, iletişim bilgilerini ve iletişimlerini güncelleştirebilirsiniz. |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.TimeSeriesInsights/environments/accesspolicies/delete | Erişim ilkesini siler. |
> | Eylem | Microsoft.TimeSeriesInsights/environments/accesspolicies/read | Bir erişim ilkesi özelliklerini alır. |
> | Eylem | Microsoft.TimeSeriesInsights/environments/accesspolicies/write | Yeni bir erişim ilkesi için bir ortam oluşturur veya mevcut bir erişim ilkesi güncelleştirir. |
> | Eylem | Microsoft.TimeSeriesInsights/environments/delete | Ortamı siler. |
> | Eylem | Microsoft.TimeSeriesInsights/environments/eventsources/delete | Olay kaynağı siler. |
> | Eylem | Microsoft.TimeSeriesInsights/environments/eventsources/read | Olay kaynağı özelliklerini alır. |
> | Eylem | Microsoft.TimeSeriesInsights/environments/eventsources/write | Bir ortam için yeni bir olay kaynağı oluşturur veya mevcut bir olay kaynağı güncelleştirir. |
> | Eylem | Microsoft.TimeSeriesInsights/environments/read | Bir ortam özelliklerini alır. |
> | Eylem | Microsoft.TimeSeriesInsights/environments/referencedatasets/delete | Başvuru veri kümesi siler. |
> | Eylem | Microsoft.TimeSeriesInsights/environments/referencedatasets/read | Başvuru veri kümesi özelliklerini alır. |
> | Eylem | Microsoft.TimeSeriesInsights/environments/referencedatasets/write | Bir ortam için yeni bir başvuru veri kümesi oluşturur veya var olan bir başvuru veri kümesi güncelleştirir. |
> | Eylem | Microsoft.TimeSeriesInsights/environments/status/read | Ortam, giriş gibi ilişkili alt işlemlerin durumu durumunu alın. |
> | Eylem | Microsoft.TimeSeriesInsights/environments/write | Yeni bir ortam oluşturur veya mevcut bir ortama güncelleştirir. |
> | Eylem | Microsoft.TimeSeriesInsights/register/action | Time Series Insights kaynak sağlayıcısı için aboneliği kaydeder ve zaman serisi görüşleri ortamları oluşturmanıza olanak tanır. |

## <a name="microsoftvisualstudio"></a>Microsoft.VisualStudio

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.VisualStudio/Account/Delete | Hesabı Sil |
> | Eylem | Microsoft.VisualStudio/Account/Extension/Read | Okuma hesabı/uzantısı |
> | Eylem | Microsoft.VisualStudio/Account/Project/Read | Hesabı/proje okuyun |
> | Eylem | Microsoft.VisualStudio/Account/Project/Write | Hesabı/projesi Ayarla |
> | Eylem | Microsoft.VisualStudio/Account/Read | Okuma hesabı |
> | Eylem | Microsoft.VisualStudio/Account/Write | Hesabı ayarla |
> | Eylem | Microsoft.VisualStudio/Extension/Delete | Uzantıyı Sil |
> | Eylem | Microsoft.VisualStudio/Extension/Read | Uzantı okuyun |
> | Eylem | Microsoft.VisualStudio/Extension/Write | Uzantı ayarlayın |
> | Eylem | Microsoft.VisualStudio/Project/Delete | Projeyi Sil |
> | Eylem | Microsoft.VisualStudio/Project/Read | Okuma proje |
> | Eylem | Microsoft.VisualStudio/Project/Write | Proje ayarlayın |
> | Eylem | Microsoft.VisualStudio/Register/Action | Azure aboneliği Microsoft.VisualStudio sağlayıcıya Kaydol |

## <a name="microsoftweb"></a>microsoft.web

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Web/apimanagementaccounts/apiacls/Read | API yönetim hesapları Apiacls alın. |
> | Eylem | microsoft.web/apimanagementaccounts/apis/apiacls/delete | API yönetim hesapları API'leri Apiacls silin. |
> | Eylem | Microsoft.Web/apimanagementaccounts/apis/apiacls/Read | API yönetim hesapları API'leri Apiacls alın. |
> | Eylem | Microsoft.Web/apimanagementaccounts/apis/apiacls/Write | API yönetim hesapları API'leri Apiacls güncelleştirin. |
> | Eylem | Microsoft.Web/apimanagementaccounts/apis/connectionacls/Read | API yönetim hesapları API'leri Connectionacls alın. |
> | Eylem | Microsoft.Web/apimanagementaccounts/apis/Connections/confirmconsentcode/Action | Onay kodu API yönetim hesapları API bağlantıları onaylayın. |
> | Eylem | microsoft.web/apimanagementaccounts/apis/connections/connectionacls/delete | API yönetim hesapları API bağlantıları Connectionacls silin. |
> | Eylem | Microsoft.Web/apimanagementaccounts/apis/Connections/connectionacls/Read | API yönetim hesapları API bağlantıları Connectionacls alın. |
> | Eylem | Microsoft.Web/apimanagementaccounts/apis/Connections/connectionacls/Write | API yönetim hesapları API bağlantıları Connectionacls güncelleştirin. |
> | Eylem | microsoft.web/apimanagementaccounts/apis/connections/delete | API yönetim hesapları API bağlantıları silin. |
> | Eylem | microsoft.web/apimanagementaccounts/apis/connections/getconsentlinks/action | API yönetim hesapları API bağlantıları için onay bağlantılarını alın. |
> | Eylem | Microsoft.Web/apimanagementaccounts/apis/Connections/listconnectionkeys/Action | Liste bağlantı anahtarları API yönetim hesapları API bağlantıları. |
> | Eylem | Microsoft.Web/apimanagementaccounts/apis/Connections/listsecrets/Action | Liste gizli dizileri API yönetim hesapları API bağlantıları. |
> | Eylem | Microsoft.Web/apimanagementaccounts/apis/Connections/Read | API yönetim hesapları API'leri bağlantılarını alın. |
> | Eylem | Microsoft.Web/apimanagementaccounts/apis/Connections/Write | API yönetim hesapları API bağlantıları güncelleştirin. |
> | Eylem | microsoft.web/apimanagementaccounts/apis/delete | API Management hesaplarını API'leri silin. |
> | Eylem | microsoft.web/apimanagementaccounts/apis/localizeddefinitions/delete | API Management'ı silme hesapları API'leri yerelleştirilmiş tanımları. |
> | Eylem | Microsoft.Web/apimanagementaccounts/apis/localizeddefinitions/Read | API Management alma hesapları API'leri yerelleştirilmiş tanımları. |
> | Eylem | Microsoft.Web/apimanagementaccounts/apis/localizeddefinitions/Write | Tanımları güncelleştirme API yönetim hesapları API'leri yerelleştirilmiş. |
> | Eylem | Microsoft.Web/apimanagementaccounts/apis/Read | API Management hesaplarını API'leri alın. |
> | Eylem | Microsoft.Web/apimanagementaccounts/apis/Write | API Management hesaplarını API'leri güncelleştirin. |
> | Eylem | Microsoft.Web/apimanagementaccounts/connectionacls/Read | API yönetim hesapları Connectionacls alın. |
> | Eylem | Microsoft.Web/availablestacks/Read | Kullanılabilir yığınları alın. |
> | Eylem | Microsoft.Web/certificates/Delete | Mevcut bir sertifikayı silin. |
> | Eylem | Microsoft.Web/certificates/Read | Sertifikaları'nın listesini alın. |
> | Eylem | Microsoft.Web/certificates/Write | Yeni bir sertifika eklemek veya mevcut bir güncelleştirin. |
> | Eylem | Microsoft.Web/checknameavailability/Read | Kaynak adı kullanılabilir olup olmadığını denetleyin. |
> | Eylem | Microsoft.Web/classicmobileservices/Read | Klasik mobil hizmet edinebilirsiniz. |
> | Eylem | Microsoft.Web/connectionGateways/Delete | Bağlantı ağ geçidi siler. |
> | Eylem | Microsoft.Web/connectionGateways/Join/Action | Bağlantı ağ geçidi birleştirir. |
> | Eylem | Microsoft.Web/connectionGateways/ListStatus/Action | Bağlantı ağ geçidi durumunu listeler. |
> | Eylem | Microsoft.Web/connectionGateways/Move/Action | Bağlantı ağ geçidi taşır. |
> | Eylem | Microsoft.Web/connectionGateways/Read | Bağlantı ağ geçidi listesini alın. |
> | Eylem | Microsoft.Web/connectionGateways/Write | Oluşturur veya bir bağlantı ağ geçidi güncelleştirir. |
> | Eylem | Microsoft.Web/Connections/confirmconsentcode/Action | Bağlantıları onay kodunu onaylayın. |
> | Eylem | Microsoft.Web/connections/Delete | Bir bağlantı siler. |
> | Eylem | Microsoft.Web/connections/Join/Action | Bir bağlantı birleştirir. |
> | Eylem | Microsoft.Web/Connections/listconsentlinks/Action | Bağlantılar için onay bağlantılarını listesi. |
> | Eylem | Microsoft.Web/connections/Move/Action | Bir bağlantı taşır. |
> | Eylem | Microsoft.Web/connections/Read | Bağlantı'nın listesini alın. |
> | Eylem | Microsoft.Web/connections/Write | Oluşturur veya bir bağlantı güncelleştirir. |
> | Eylem | Microsoft.Web/customApis/Delete | Özel API siler. |
> | Eylem | Microsoft.Web/customApis/extractApiDefinitionFromWsdl/Action | API tanımı bir WSDL'den ayıklar. |
> | Eylem | Microsoft.Web/customApis/Join/Action | Özel API birleştirir. |
> | Eylem | Microsoft.Web/customApis/listWsdlInterfaces/Action | Özel bir API için WSDL arabirimleri listeler. |
> | Eylem | Microsoft.Web/customApis/Move/Action | Özel API taşır. |
> | Eylem | Microsoft.Web/customApis/Read | Özel API'ın listesini alın. |
> | Eylem | Microsoft.Web/customApis/Write | Oluşturur veya özel API güncelleştirir. |
> | Eylem | Microsoft.Web/deletedSites/Read | Silinen bir Web uygulaması özelliklerini alma |
> | Eylem | Microsoft.Web/deploymentlocations/Read | Dağıtım konumları Al. |
> | Eylem | Microsoft.Web/geoRegions/Read | Coğrafi bölgelerin listesini alın. |
> | Eylem | Microsoft.Web/hostingenvironments/capacities/Read | Ortamları kapasiteler barındırma alın. |
> | Eylem | Microsoft.Web/hostingEnvironments/Delete | App Service ortamını Sil |
> | Eylem | Microsoft.Web/hostingenvironments/detectors/Read | Ortamlarda algılayıcılar barındırma alın. |
> | Eylem | Microsoft.Web/hostingenvironments/Diagnostics/Read | Ortamları tanılama barındırma alın. |
> | Eylem | Microsoft.Web/hostingenvironments/inboundnetworkdependenciesendpoints/Read | Ağ uç noktalarına gelen tüm bağımlılıklarını edinin. |
> | Eylem | Microsoft.Web/hostingEnvironments/Join/Action | App Service ortamı birleştirir |
> | Eylem | Microsoft.Web/hostingenvironments/metricdefinitions/Read | Ortamları ölçüm tanımlarını barındırma alın. |
> | Eylem | Microsoft.Web/hostingenvironments/multirolepools/metricdefinitions/Read | Ortamlar arasında bağlantı kurulmaya havuzları ölçüm tanımlarını barındırma alın. |
> | Eylem | Microsoft.Web/hostingenvironments/multirolepools/Metrics/Read | Ortamlar arasında bağlantı kurulmaya havuzları ölçümleri barındırma alın. |
> | Eylem | Microsoft.Web/hostingEnvironments/multiRolePools/Read | Bir App Service Ortamı'nda ön uç havuzu özelliklerini alma |
> | Eylem | Microsoft.Web/hostingenvironments/multirolepools/skus/Read | Ortamlar arasında bağlantı kurulmaya havuzları SKU'ları barındırma alın. |
> | Eylem | Microsoft.Web/hostingenvironments/multirolepools/usages/Read | Ortamlar arasında bağlantı kurulmaya havuzları kullanımları barındırma alın. |
> | Eylem | Microsoft.Web/hostingEnvironments/multiRolePools/Write | Bir App Service Ortamı'nda yeni bir ön uç havuzu oluşturun veya var olan bir güncelleştirme |
> | Eylem | Microsoft.Web/hostingenvironments/Operations/Read | Ortam işlem barındırma alın. |
> | Eylem | Microsoft.Web/hostingenvironments/outboundnetworkdependenciesendpoints/Read | Ağ uç noktaları tüm giden bağımlılıklar alın. |
> | Eylem | Microsoft.Web/hostingEnvironments/PrivateEndpointConnectionsApproval/action | Özel uç nokta bağlantıları Onayla |
> | Eylem | Microsoft.Web/hostingEnvironments/Read | App Service ortamı özelliklerini alma |
> | Eylem | Microsoft.Web/hostingEnvironments/reboot/Action | Bir App Service Ortamı'nda tüm makineleri yeniden başlatın |
> | Eylem | Microsoft.Web/hostingenvironments/Resume/Action | Barındırma ortamları sürdürün. |
> | Eylem | Microsoft.Web/hostingenvironments/serverfarms/Read | Barındırma ortamları App Service planları alın. |
> | Eylem | Microsoft.Web/hostingenvironments/Sites/Read | Barındırma ortamları Web uygulamalarını alın. |
> | Eylem | Microsoft.Web/hostingenvironments/suspend/Action | Barındırma ortamları askıya alın. |
> | Eylem | Microsoft.Web/hostingenvironments/usages/Read | Ortamları kullanımları barındırma alın. |
> | Eylem | Microsoft.Web/hostingenvironments/workerpools/metricdefinitions/Read | Ortamları Workerpools ölçüm tanımlarını barındırma alın. |
> | Eylem | Microsoft.Web/hostingenvironments/workerpools/Metrics/Read | Ortamları Workerpools ölçümleri barındırma alın. |
> | Eylem | Microsoft.Web/hostingEnvironments/workerPools/Read | Bir App Service ortamında bir çalışan havuzu özelliklerini alma |
> | Eylem | Microsoft.Web/hostingenvironments/workerpools/skus/Read | Ortamları Workerpools SKU'ları barındırma alın. |
> | Eylem | Microsoft.Web/hostingenvironments/workerpools/usages/Read | Ortamları Workerpools kullanımları barındırma alın. |
> | Eylem | Microsoft.Web/hostingEnvironments/workerPools/Write | Bir App Service Ortamı'nda yeni bir çalışan havuzu oluşturun veya var olan bir güncelleştirme |
> | Eylem | Microsoft.Web/hostingEnvironments/Write | Yeni bir App Service ortamı oluşturma veya var olan bir güncelleştirme |
> | Eylem | Microsoft.Web/ishostingenvironmentnameavailable/Read | Barındırma ortamı adı kullanılabilir olup olmadığını alır. |
> | Eylem | Microsoft.Web/ishostnameavailable/Read | Konak adı kullanılabilir olup olmadığını denetleyin. |
> | Eylem | Microsoft.Web/isusernameavailable/Read | Kullanıcı adı kullanılabilir olup olmadığını denetleyin. |
> | Eylem | Microsoft.Web/listSitesAssignedToHostName/Read | Ana bilgisayar adı için atanan site adını alın. |
> | Eylem | Microsoft.Web/Locations/apioperations/Read | Konumlar API'si işlemleri Al. |
> | Eylem | Microsoft.Web/Locations/connectiongatewayinstallations/Read | Konumları bağlantı ağ geçidi yüklemelerinizi alın. |
> | Eylem | microsoft.web/locations/deleteVirtualNetworkOrSubnets/action | Vnet veya alt silme bildirimi konumları. |
> | Eylem | Microsoft.Web/Locations/extractapidefinitionfromwsdl/Action | API tanımı için konumları WSDL'den ayıklayın. |
> | Eylem | Microsoft.Web/Locations/listwsdlinterfaces/Action | Konumların listesi WSDL arabirimleri. |
> | Eylem | Microsoft.Web/Locations/managedapis/apioperations/Read | Konumları yönetilen API işlemleri Al. |
> | Eylem | Microsoft.Web/locations/managedapis/Join/Action | Yönetilen bir API birleştirir. |
> | Eylem | Microsoft.Web/Locations/managedapis/Read | Yönetilen API'ler konumları Al |
> | Eylem | Microsoft.Web/Operations/Read | İşlemleri Al. |
> | Eylem | Microsoft.Web/publishingusers/Read | Kullanıcılar yayımlanıyor alın. |
> | Eylem | Microsoft.Web/publishingusers/Write | Kullanıcılar yayımlama güncelleştirin. |
> | Eylem | Microsoft.Web/recommendations/Read | Abonelikler için önerileri'nın listesini alın. |
> | Eylem | Microsoft.Web/Register/Action | Aboneliğinin, Microsoft.Web kaynak sağlayıcısını kaydedin. |
> | Eylem | Microsoft.Web/resourcehealthmetadata/Read | Kaynak sistem durumu meta verilerini alın. |
> | Eylem | Microsoft.Web/serverfarms/Capabilities/Read | App Service planları özellikleri alın. |
> | Eylem | Microsoft.Web/serverfarms/Delete | Mevcut bir App Service Planı Sil |
> | Eylem | Microsoft.Web/serverfarms/firstpartyapps/Settings/DELETE | App Service planları birinci taraf uygulamalar ayarlarını silin. |
> | Eylem | Microsoft.Web/serverfarms/firstpartyapps/Settings/Read | App Service planları birinci taraf uygulamalar ayarlarını alın. |
> | Eylem | Microsoft.Web/serverfarms/firstpartyapps/Settings/Write | App Service planları birinci taraf uygulamalar ayarlarını güncelleştirin. |
> | Eylem | microsoft.web/serverfarms/hybridconnectionnamespaces/relays/delete | App Service planları karma bağlantı ad alanları geçişleri silin. |
> | Eylem | Microsoft.Web/serverfarms/hybridconnectionnamespaces/relays/Read | App Service planları karma bağlantı ad alanları geçişleri alın. |
> | Eylem | Microsoft.Web/serverfarms/hybridconnectionnamespaces/relays/Sites/Read | App Service planları karma bağlantı ad alanları geçişleri Web uygulamalarını alın. |
> | Eylem | Microsoft.Web/serverfarms/hybridconnectionplanlimits/Read | App Service planları karma bağlantı planı sınırları alın. |
> | Eylem | Microsoft.Web/serverfarms/hybridconnectionrelays/Read | App Service planları karma bağlantı geçişleri alın. |
> | Eylem | Microsoft.Web/serverfarms/metricdefinitions/Read | App Service planları ölçüm tanımlarını Al. |
> | Eylem | Microsoft.Web/serverfarms/Metrics/Read | App Service planları ölçümleri alın. |
> | Eylem | Microsoft.Web/serverfarms/operationresults/Read | App Service planları İşlem sonuçlarını al. |
> | Eylem | Microsoft.Web/serverfarms/Read | Bir App Service planı üzerinde özelliklerini alma |
> | Eylem | Microsoft.Web/serverfarms/restartSites/Action | Tüm Web uygulamaları bir App Service planında yeniden başlatın. |
> | Eylem | Microsoft.Web/serverfarms/Sites/Read | App Service planları Web uygulamalarını alın. |
> | Eylem | Microsoft.Web/serverfarms/skus/Read | App Service planları'SKU ' ları alın. |
> | Eylem | Microsoft.Web/serverfarms/usages/Read | App Service planları kullanımlarını Al. |
> | Eylem | Microsoft.Web/serverfarms/virtualnetworkconnections/Gateways/Write | App Service planları sanal ağ bağlantıları Gateway bileşenlerini güncelleştirin. |
> | Eylem | Microsoft.Web/serverfarms/virtualnetworkconnections/Read | App Service planları sanal ağ bağlantılarını alın. |
> | Eylem | Microsoft.Web/serverfarms/virtualnetworkconnections/Routes/DELETE | App Service planları sanal ağ bağlantıları yolları silin. |
> | Eylem | Microsoft.Web/serverfarms/virtualnetworkconnections/Routes/Read | App Service planları sanal ağ bağlantıları yolları alın. |
> | Eylem | Microsoft.Web/serverfarms/virtualnetworkconnections/Routes/Write | App Service planları sanal ağ bağlantıları yolları güncelleştirin. |
> | Eylem | Microsoft.Web/serverfarms/workers/reboot/Action | App Service planı çalışanları yeniden başlatın. |
> | Eylem | Microsoft.Web/serverfarms/Write | Yeni bir App Service planı oluşturun veya var olan bir güncelleştirme |
> | Eylem | Microsoft.Web/Sites/analyzecustomhostname/Read | Özel ana bilgisayar adı analiz edin. |
> | Eylem | Microsoft.Web/sites/applySlotConfig/Action | Geçerli web uygulamasına hedef yuvasından Web uygulaması yuvası yapılandırmasını Uygula |
> | Eylem | Microsoft.Web/sites/backup/Action | Yeni bir web uygulaması yedekleme oluşturma |
> | Eylem | Microsoft.Web/Sites/Backup/Read | Web Apps yedeklemesini alın. |
> | Eylem | Microsoft.Web/Sites/Backup/Write | Web Apps yedeklemesini güncelleştirme. |
> | Eylem | Microsoft.Web/Sites/Backups/Action | Azure depolama alanındaki bir blobdan geri mevcut bir uygulama yedeğini bulur. |
> | Eylem | Microsoft.Web/Sites/Backups/DELETE | Web Apps yedeklemelerini silin. |
> | Eylem | Microsoft.Web/Sites/Backups/List/Action | Web Apps yedeklemelerini listesi. |
> | Eylem | Microsoft.Web/sites/backups/Read | Bir web uygulamasının yedekleme özelliklerini alma |
> | Eylem | Microsoft.Web/Sites/Backups/Restore/Action | Web Apps yedeklemelerini geri yükleyin. |
> | Eylem | Microsoft.Web/Sites/Backups/Write | Web Apps yedeklemelerini güncelleştirin. |
> | Eylem | microsoft.web/sites/config/delete | Web Apps Config silin. |
> | Eylem | Microsoft.Web/sites/config/list/Action | Kimlik bilgileri, uygulama ayarlarının ve bağlantı dizelerinin yayımlama gibi Web uygulamasının güvenlik hassas ayarları listesi |
> | Eylem | Microsoft.Web/sites/config/Read | Web uygulaması yapılandırma ayarlarını alma |
> | Eylem | Microsoft.Web/Sites/config/snapshots/Read | Web Apps Config anlık görüntüleri alın. |
> | Eylem | Microsoft.Web/sites/config/Write | Web uygulamasının yapılandırma ayarlarını güncelleştirme |
> | Eylem | Microsoft.Web/Sites/containerlogs/Action | Kapsayıcı günlükleri, Web uygulaması için daraltılmış. |
> | Eylem | Microsoft.Web/Sites/containerlogs/download/Action | Web Apps kapsayıcı günlükleri indirin. |
> | Eylem | Microsoft.Web/Sites/continuouswebjobs/DELETE | Web Apps sürekli Web işleri silin. |
> | Eylem | Microsoft.Web/Sites/continuouswebjobs/Read | Web Apps sürekli Web işleri görüntüleyin. |
> | Eylem | Microsoft.Web/Sites/continuouswebjobs/Start/Action | Web Apps sürekli Web işleri'ni başlatın. |
> | Eylem | Microsoft.Web/Sites/continuouswebjobs/Stop/Action | Web Apps sürekli Web işleri'ni durdurun. |
> | Eylem | Microsoft.Web/sites/Delete | Mevcut bir Web uygulamasını silme |
> | Eylem | microsoft.web/sites/deployments/delete | Web Apps dağıtımları silin. |
> | Eylem | Microsoft.Web/Sites/Deployments/log/Read | Web Apps dağıtımları kayıt günlüklerini alın. |
> | Eylem | Microsoft.Web/Sites/Deployments/Read | Web Apps dağıtımları alın. |
> | Eylem | Microsoft.Web/Sites/Deployments/Write | Web Apps dağıtımları güncelleştirin. |
> | Eylem | Microsoft.Web/Sites/detectors/Read | Web Apps algılayıcıları alın. |
> | Eylem | microsoft.web/sites/diagnostics/analyses/execute/Action | Web Apps tanılama analizini Çalıştır. |
> | Eylem | Microsoft.Web/Sites/Diagnostics/Analyses/Read | Web Apps tanılama analizi alın. |
> | Eylem | Microsoft.Web/Sites/Diagnostics/aspnetcore/Read | ASP.NET Core uygulaması için Web Apps tanılama alın. |
> | Eylem | Microsoft.Web/Sites/Diagnostics/autoheal/Read | Web Apps tanılama Autoheal alın. |
> | Eylem | Microsoft.Web/Sites/Diagnostics/Deployment/Read | Web Apps tanılama dağıtım alın. |
> | Eylem | Microsoft.Web/Sites/Diagnostics/Deployments/Read | Web Apps tanılama dağıtımları alın. |
> | Eylem | microsoft.web/sites/diagnostics/detectors/execute/Action | Web Apps tanılama algılayıcısı çalıştırın. |
> | Eylem | Microsoft.Web/Sites/Diagnostics/detectors/Read | Web Apps tanılama algılayıcısı alın. |
> | Eylem | Microsoft.Web/Sites/Diagnostics/failedrequestsperuri/Read | Web Apps tanılama URI'si başına başarısız olan istekleri alın. |
> | Eylem | Microsoft.Web/Sites/Diagnostics/frebanalysis/Read | Web Apps tanılama FREB analizi alın. |
> | Eylem | Microsoft.Web/Sites/Diagnostics/loganalyzer/Read | Web Apps Tanılama Günlüğü Çözümleyicisi alın. |
> | Eylem | Microsoft.Web/Sites/Diagnostics/Read | Web Apps tanılama kategorileri alın. |
> | Eylem | Microsoft.Web/Sites/Diagnostics/runtimeavailability/Read | Web Apps tanılama çalışma zamanı kullanılabilirliğini Al. |
> | Eylem | Microsoft.Web/Sites/Diagnostics/servicehealth/Read | Get Web Apps Diagnostics Service Health. |
> | Eylem | Microsoft.Web/Sites/Diagnostics/sitecpuanalysis/Read | Web Apps tanılama Site CPU analizi alın. |
> | Eylem | Microsoft.Web/Sites/Diagnostics/sitecrashes/Read | Web Apps tanılama Site kilitlenmeleri alın. |
> | Eylem | Microsoft.Web/Sites/Diagnostics/sitelatency/Read | Web Apps tanılama Site gecikme alın. |
> | Eylem | Microsoft.Web/Sites/Diagnostics/sitememoryanalysis/Read | Web Apps tanılama Site bellek analizi alın. |
> | Eylem | Microsoft.Web/Sites/Diagnostics/siterestartsettingupdate/Read | Web Apps tanılama Site yeniden başlatma ayarı güncelleştirmeyi edinin. |
> | Eylem | Microsoft.Web/Sites/Diagnostics/siterestartuserinitiated/Read | Web Apps tanılama Site yeniden başlatma kullanıcı tarafından başlatılan alın. |
> | Eylem | Microsoft.Web/Sites/Diagnostics/siteswap/Read | Web Apps tanılama Site takas alın. |
> | Eylem | Microsoft.Web/Sites/Diagnostics/THREADCOUNT/Read | Web Apps tanılama iş parçacığı sayısı alın. |
> | Eylem | Microsoft.Web/Sites/Diagnostics/workeravailability/Read | Web Apps tanılama Workeravailability alın. |
> | Eylem | Microsoft.Web/Sites/Diagnostics/workerprocessrecycle/Read | Web uygulamaları tanılama çalışan işlemi geri alın. |
> | Eylem | Microsoft.Web/Sites/domainownershipidentifiers/Read | Web Apps etki alanı sahipliğini tanımlayıcıları alın. |
> | Eylem | Microsoft.Web/Sites/domainownershipidentifiers/Write | Web Apps etki alanı sahipliğini tanımlayıcıları güncelleştirin. |
> | Eylem | microsoft.web/sites/extensions/delete | Web Apps Site uzantılarını silin. |
> | Eylem | Microsoft.Web/Sites/Extensions/Read | Web Apps Site uzantılarını edinin. |
> | Eylem | Microsoft.Web/Sites/Extensions/Write | Web Apps Site uzantıları güncelleştirin. |
> | Eylem | Microsoft.Web/Sites/Functions/Action | İşlevler Web uygulamaları. |
> | Eylem | Microsoft.Web/Sites/Functions/DELETE | Web Apps işlevleri silin. |
> | Eylem | Microsoft.Web/Sites/Functions/listsecrets/Action | Liste gizli dizileri Web Apps işlevleri. |
> | Eylem | Microsoft.Web/Sites/Functions/masterkey/Read | Get Web Apps Functions Masterkey. |
> | Eylem | Microsoft.Web/Sites/Functions/Read | Web Apps işlevleri alın. |
> | Eylem | Microsoft.Web/Sites/Functions/Token/Read | Get Web Apps işlevleri belirteci. |
> | Eylem | Microsoft.Web/Sites/Functions/Write | Web Apps işlevleri güncelleştirin. |
> | Eylem | microsoft.web/sites/hostnamebindings/delete | Web Apps konak adı bağlamaları silin. |
> | Eylem | Microsoft.Web/Sites/hostnamebindings/Read | Web Apps konak adı bağlamaları alın. |
> | Eylem | Microsoft.Web/Sites/hostnamebindings/Write | Web Apps konak adı bağlamaları güncelleştirin. |
> | Eylem | Microsoft.Web/Sites/hostruntime/Functions/Keys/Read | Web Apps Hostruntime işlevleri anahtarlarını alın. |
> | Eylem | Microsoft.Web/sites/hostruntime/host/_master/read | Yönetim işlemleri için işlevi uygulamanın ana anahtarı alma |
> | Eylem | Microsoft.Web/sites/hostruntime/host/action | İşlev uygulaması çalışma zamanı eylem Tetikleyicileri eşitleme, İşlevler eklemek, işlevleri çağırmak, delete vb. işlevleri gibi gerçekleştirin. |
> | Eylem | Microsoft.Web/Sites/hostruntime/Host/Read | Web Apps Hostruntime konak alın. |
> | Eylem | microsoft.web/sites/hybridconnection/delete | Web Apps karma bağlantıyı silin. |
> | Eylem | Microsoft.Web/Sites/hybridconnection/Read | Web Apps karma bağlantı alın. |
> | Eylem | Microsoft.Web/Sites/hybridconnection/Write | Web Apps karma bağlantı güncelleştirin. |
> | Eylem | Microsoft.Web/Sites/hybridconnectionnamespaces/relays/DELETE | Web Apps karma bağlantı ad alanları geçişleri silin. |
> | Eylem | Microsoft.Web/Sites/hybridconnectionnamespaces/relays/listkeys/Action | Liste anahtarlarını Web uygulamaları karma bağlantı ad alanları geçişler. |
> | Eylem | Microsoft.Web/Sites/hybridconnectionnamespaces/relays/Read | Web Apps karma bağlantı ad alanları geçişleri alın. |
> | Eylem | Microsoft.Web/Sites/hybridconnectionnamespaces/relays/Write | Web Apps karma bağlantı ad alanları geçişleri güncelleştirin. |
> | Eylem | Microsoft.Web/Sites/hybridconnectionrelays/Read | Web Apps karma bağlantı geçişleri alın. |
> | Eylem | microsoft.web/sites/instances/deployments/delete | Web Apps örnekleri dağıtımları silin. |
> | Eylem | Microsoft.Web/Sites/instances/Deployments/Read | Web Apps örnekleri dağıtımları alın. |
> | Eylem | Microsoft.Web/Sites/instances/Extensions/log/Read | Web Apps örnekleri uzantıları kayıt günlüklerini alın. |
> | Eylem | Microsoft.Web/Sites/instances/Extensions/Processes/Read | Web Apps örnekleri uzantıları işlemleri alın. |
> | Eylem | Microsoft.Web/Sites/instances/Extensions/Read | Web Apps örnekleri uzantılarını edinin. |
> | Eylem | microsoft.web/sites/instances/processes/delete | Web Apps örnekleri işlemleri silin. |
> | Eylem | Microsoft.Web/Sites/instances/Processes/Modules/Read | Web uygulamaları örnekleri işlemlerin modüllerini alın. |
> | Eylem | Microsoft.Web/Sites/instances/Processes/Read | Web Apps örnekleri işlemleri alın. |
> | Eylem | Microsoft.Web/Sites/instances/Processes/Threads/Read | Web Apps örnekleri işlemleri iş parçacıkları alın. |
> | Eylem | Microsoft.Web/Sites/instances/Read | Web Apps örneği alın. |
> | Eylem | Microsoft.Web/Sites/listsyncfunctiontriggerstatus/Action | Liste eşitleme işlevi tetikleyici durumu Web uygulamaları. |
> | Eylem | Microsoft.Web/Sites/metricdefinitions/Read | Web Apps ölçüm tanımlarını alın. |
> | Eylem | Microsoft.Web/Sites/Metrics/Read | Web Apps ölçümleri alın. |
> | Eylem | Microsoft.Web/Sites/metricsdefinitions/Read | Web Apps ölçüm tanımlarını Al. |
> | Eylem | microsoft.web/sites/migratemysql/action | MySql Web uygulamaları geçirin. |
> | Eylem | microsoft.web/sites/migratemysql/read | Get Web Apps Migrate MySql. |
> | Eylem | Microsoft.Web/Sites/networktrace/Action | Ağ izleme Web uygulamaları. |
> | Eylem | Microsoft.Web/Sites/networktraces/operationresults/Read | Web Apps ağ izleme İşlem sonuçlarını alır. |
> | Eylem | Microsoft.Web/Sites/NewPassword/Action | NewPassword Web uygulamaları. |
> | Eylem | microsoft.web/sites/operationresults/read | Web Apps İşlem sonuçlarını al. |
> | Eylem | Microsoft.Web/Sites/Operations/Read | Web Apps işlemleri Al. |
> | Eylem | Microsoft.Web/Sites/perfcounters/Read | Web Apps performans sayaçları alın. |
> | Eylem | microsoft.web/sites/premieraddons/delete | Web Apps Premier Addons silin. |
> | Eylem | Microsoft.Web/Sites/premieraddons/Read | Web Apps Premier Addons alın. |
> | Eylem | Microsoft.Web/Sites/premieraddons/Write | Web Apps Premier Addons güncelleştirin. |
> | Eylem | Microsoft.Web/Sites/privateaccess/Read | Özel site erişimi etkinleştirme ve yetkili site erişebileceği ağlarda verileri elde edersiniz. |
> | Eylem | Microsoft.Web/sites/PrivateEndpointConnectionsApproval/action | Özel uç nokta bağlantıları Onayla |
> | Eylem | Microsoft.Web/Sites/Processes/Modules/Read | Web Apps işlemlerin modüllerini alın. |
> | Eylem | Microsoft.Web/Sites/Processes/Read | Web Apps işlemleri Al. |
> | Eylem | Microsoft.Web/Sites/Processes/Threads/Read | Web Apps işlemleri iş parçacıkları alın. |
> | Eylem | microsoft.web/sites/publiccertificates/delete | Web Apps genel sertifikalarını silin. |
> | Eylem | microsoft.web/sites/publiccertificates/read | Web Apps ortak sertifikaları Al. |
> | Eylem | Microsoft.Web/Sites/publiccertificates/Write | Web Apps ortak sertifikaları güncelleştirin. |
> | Eylem | Microsoft.Web/sites/publish/Action | Bir Web uygulaması yayımlama |
> | Eylem | Microsoft.Web/sites/publishxml/Action | Profil xml Web uygulaması için yayımlama Al |
> | Eylem | microsoft.web/sites/publishxml/read | XML Web uygulamalarını yayımlama alın. |
> | Eylem | Microsoft.Web/sites/Read | Bir Web uygulamasının özelliklerini alma |
> | Eylem | Microsoft.Web/Sites/recommendationhistory/Read | Web Apps öneri geçmişi Al |
> | Eylem | Microsoft.Web/Sites/Recommendations/disable/Action | Web Apps önerileri devre dışı bırakın. |
> | Eylem | Microsoft.Web/sites/recommendations/Read | Web uygulaması için öneriler listesini alın. |
> | Eylem | Microsoft.Web/Sites/RECOVER/Action | Web Apps'ı kurtarın. |
> | Eylem | Microsoft.Web/sites/resetSlotConfig/Action | Web uygulama yapılandırmasını Sıfırla |
> | Eylem | Microsoft.Web/Sites/resourcehealthmetadata/Read | Web Apps kaynak sistem durumu meta verilerini alın. |
> | Eylem | Microsoft.Web/sites/restart/Action | Bir Web uygulamasını yeniden başlatın |
> | Eylem | Microsoft.Web/Sites/Restore/Read | Web Apps geri alın. |
> | Eylem | Microsoft.Web/Sites/Restore/Write | Web Apps geri yükleyin. |
> | Eylem | Microsoft.Web/Sites/restorefrombackupblob/Action | Web uygulamasını yedekleme Blobundan geri yükleme. |
> | Eylem | microsoft.web/sites/restorefromdeletedwebapp/action | Web Apps, silinen uygulamadan geri yükleyin. |
> | Eylem | Microsoft.Web/Sites/restoresnapshot/Action | Web Apps anlık görüntü geri yükleyin. |
> | Eylem | microsoft.web/sites/siteextensions/delete | Web Apps Site uzantılarını silin. |
> | Eylem | Microsoft.Web/Sites/siteextensions/Read | Web Apps Site uzantılarını edinin. |
> | Eylem | Microsoft.Web/Sites/siteextensions/Write | Web Apps Site uzantıları güncelleştirin. |
> | Eylem | Microsoft.Web/Sites/slots/analyzecustomhostname/Read | Web alma uygulamaları yuvaları analiz özel ana bilgisayar adı. |
> | Eylem | Microsoft.Web/sites/slots/applySlotConfig/Action | Itanium tabanlı sistemler için Web uygulaması yuvası yapılandırması hedef yuvasından geçerli yuvadaki için geçerlidir. |
> | Eylem | Microsoft.Web/sites/slots/backup/Action | Yeni Web uygulaması yuvası yedekleme oluşturun. |
> | Eylem | Microsoft.Web/Sites/slots/Backup/Read | Web Apps yuvaları yedeklemesini alın. |
> | Eylem | Microsoft.Web/Sites/slots/Backup/Write | Web Apps yuvaları yedekleme güncelleştirin. |
> | Eylem | Microsoft.Web/Sites/slots/Backups/Action | Web Apps yuvaları yedekleri bulun. |
> | Eylem | Microsoft.Web/Sites/slots/Backups/DELETE | Web Apps yuvaları yedeklemeleri silin. |
> | Eylem | Microsoft.Web/Sites/slots/Backups/List/Action | Web Apps yuvaları yedekleri listele. |
> | Eylem | Microsoft.Web/sites/slots/backups/Read | Bir web uygulaması yuvaları yedekleme özelliklerini alma |
> | Eylem | Microsoft.Web/Sites/slots/Backups/Restore/Action | Web Apps yuvaları yedeklemeleri geri yükleyin. |
> | Eylem | microsoft.web/sites/slots/config/delete | Web Apps yuvalarını yapılandırma silin. |
> | Eylem | Microsoft.Web/sites/slots/config/list/Action | Kimlik bilgileri, uygulama ayarlarının ve bağlantı dizelerinin yayımlama gibi Web uygulaması yuvanın güvenlik hassas ayarları listesi |
> | Eylem | Microsoft.Web/sites/slots/config/Read | Web uygulaması yuvası'nın yapılandırma ayarlarını alma |
> | Eylem | Microsoft.Web/sites/slots/config/Write | Web uygulaması yuvası'nın yapılandırma ayarlarını güncelleştirme |
> | Eylem | Microsoft.Web/Sites/slots/containerlogs/Action | Kapsayıcı günlükleri için Web uygulaması yuvası daraltılmış. |
> | Eylem | Microsoft.Web/Sites/slots/containerlogs/download/Action | Web Apps yuvaları kapsayıcı günlükleri indirin. |
> | Eylem | Microsoft.Web/Sites/slots/continuouswebjobs/DELETE | Web Apps yuvaları sürekli Web işleri silin. |
> | Eylem | Microsoft.Web/Sites/slots/continuouswebjobs/Read | Web Apps yuvaları sürekli Web işleri görüntüleyin. |
> | Eylem | Microsoft.Web/Sites/slots/continuouswebjobs/Start/Action | Web Apps yuvaları sürekli Web işleri'ni başlatın. |
> | Eylem | Microsoft.Web/Sites/slots/continuouswebjobs/Stop/Action | Web Apps yuvaları sürekli Web işleri'ni durdurun. |
> | Eylem | Microsoft.Web/sites/slots/Delete | Mevcut bir Web uygulaması yuvası silme |
> | Eylem | microsoft.web/sites/slots/deployments/delete | Web Apps yuvaları dağıtımları silin. |
> | Eylem | Microsoft.Web/Sites/slots/Deployments/log/Read | Web Apps yuvaları dağıtımları kayıt günlüklerini alın. |
> | Eylem | Microsoft.Web/Sites/slots/Deployments/Read | Web Apps yuvaları dağıtımları alın. |
> | Eylem | Microsoft.Web/Sites/slots/Deployments/Write | Web Apps yuvaları dağıtımları güncelleştirin. |
> | Eylem | Microsoft.Web/Sites/slots/detectors/Read | Web Apps yuvaları algılayıcıları alın. |
> | Eylem | microsoft.web/sites/slots/diagnostics/analyses/execute/Action | Web Apps yuvaları tanılama analizini Çalıştır. |
> | Eylem | Microsoft.Web/Sites/slots/Diagnostics/Analyses/Read | Web Apps yuvaları tanılama analizi alın. |
> | Eylem | Microsoft.Web/Sites/slots/Diagnostics/aspnetcore/Read | ASP.NET Core uygulaması için Web Apps yuvaları tanılama alın. |
> | Eylem | Microsoft.Web/Sites/slots/Diagnostics/autoheal/Read | Web Apps yuvaları tanılama Autoheal alın. |
> | Eylem | Microsoft.Web/Sites/slots/Diagnostics/Deployment/Read | Web Apps yuvaları tanılama dağıtım alın. |
> | Eylem | Microsoft.Web/Sites/slots/Diagnostics/Deployments/Read | Web Apps yuvaları tanılama dağıtımları alın. |
> | Eylem | microsoft.web/sites/slots/diagnostics/detectors/execute/Action | Web Apps yuvaları tanılama algılayıcısı çalıştırın. |
> | Eylem | Microsoft.Web/Sites/slots/Diagnostics/detectors/Read | Web Apps yuvaları tanılama algılayıcısı alın. |
> | Eylem | Microsoft.Web/Sites/slots/Diagnostics/frebanalysis/Read | Web Apps yuvaları tanılama FREB analizi alın. |
> | Eylem | Microsoft.Web/Sites/slots/Diagnostics/loganalyzer/Read | Web Apps yuvaları Tanılama Günlüğü Çözümleyicisi alın. |
> | Eylem | Microsoft.Web/Sites/slots/Diagnostics/Read | Web Apps yuvaları tanılama alın. |
> | Eylem | Microsoft.Web/Sites/slots/Diagnostics/runtimeavailability/Read | Web Apps yuvaları tanılama çalışma zamanı kullanılabilirliğini Al. |
> | Eylem | Microsoft.Web/Sites/slots/Diagnostics/servicehealth/Read | Web Apps yuvaları Tanılama Hizmeti durumunu alın. |
> | Eylem | Microsoft.Web/Sites/slots/Diagnostics/sitecpuanalysis/Read | Web Apps yuvaları tanılama Site CPU analizi alın. |
> | Eylem | Microsoft.Web/Sites/slots/Diagnostics/sitecrashes/Read | Web Apps yuvaları tanılama Site kilitlenmeleri alın. |
> | Eylem | Microsoft.Web/Sites/slots/Diagnostics/sitelatency/Read | Web Apps yuvaları tanılama Site gecikme alın. |
> | Eylem | Microsoft.Web/Sites/slots/Diagnostics/sitememoryanalysis/Read | Web Apps yuvaları tanılama Site bellek analizi alın. |
> | Eylem | Microsoft.Web/Sites/slots/Diagnostics/siterestartsettingupdate/Read | Web Apps yuvaları tanılama Site yeniden başlatma ayarı güncelleştirmeyi edinin. |
> | Eylem | Microsoft.Web/Sites/slots/Diagnostics/siterestartuserinitiated/Read | Web Apps yuvaları tanılama Site yeniden başlatma kullanıcı tarafından başlatılan alın. |
> | Eylem | Microsoft.Web/Sites/slots/Diagnostics/siteswap/Read | Web Apps yuvaları tanılama Site takas alın. |
> | Eylem | Microsoft.Web/Sites/slots/Diagnostics/THREADCOUNT/Read | Web Apps yuvaları tanılama iş parçacığı sayısı alın. |
> | Eylem | Microsoft.Web/Sites/slots/Diagnostics/workeravailability/Read | Web Apps yuvaları tanılama Workeravailability alın. |
> | Eylem | Microsoft.Web/Sites/slots/Diagnostics/workerprocessrecycle/Read | Web uygulamaları yuvaları tanılama çalışan işlemi geri alın. |
> | Eylem | Microsoft.Web/Sites/slots/domainownershipidentifiers/Read | Web Apps yuvaları etki alanı sahipliğini tanımlayıcıları alın. |
> | Eylem | Microsoft.Web/Sites/slots/Functions/listsecrets/Action | Liste gizli dizileri Web Apps yuvaları işlevleri. |
> | Eylem | Microsoft.Web/Sites/slots/Functions/Read | Web Apps yuvaları işlevleri alın. |
> | Eylem | microsoft.web/sites/slots/hostnamebindings/delete | Web Apps yuvaları ana bilgisayar adı bağlamaları silin. |
> | Eylem | Microsoft.Web/Sites/slots/hostnamebindings/Read | Web Apps yuvaları ana bilgisayar adı bağlamaları alın. |
> | Eylem | Microsoft.Web/Sites/slots/hostnamebindings/Write | Web Apps yuvaları ana bilgisayar adı bağlamaları güncelleştirin. |
> | Eylem | microsoft.web/sites/slots/hybridconnection/delete | Web Apps yuvaları karma bağlantıyı silin. |
> | Eylem | Microsoft.Web/Sites/slots/hybridconnection/Read | Web Apps yuvaları karma bağlantı alın. |
> | Eylem | Microsoft.Web/Sites/slots/hybridconnection/Write | Web Apps yuvaları karma bağlantı güncelleştirin. |
> | Eylem | microsoft.web/sites/slots/hybridconnectionnamespaces/relays/delete | Web Apps yuvaları karma bağlantı ad alanları geçişleri silin. |
> | Eylem | Microsoft.Web/Sites/slots/hybridconnectionnamespaces/relays/Write | Web Apps yuvaları karma bağlantı ad alanları geçişleri güncelleştirin. |
> | Eylem | Microsoft.Web/Sites/slots/hybridconnectionrelays/Read | Web Apps yuvaları karma bağlantı geçişleri alın. |
> | Eylem | Microsoft.Web/Sites/slots/instances/Deployments/Read | Web Apps yuvaları örnekleri dağıtımları alın. |
> | Eylem | microsoft.web/sites/slots/instances/processes/delete | Web Apps yuvaları örnekleri işlemleri silin. |
> | Eylem | Microsoft.Web/Sites/slots/instances/Processes/Read | Web Apps yuvaları örnekleri işlemleri alın. |
> | Eylem | Microsoft.Web/Sites/slots/instances/Read | Web yuvası örneği alın. |
> | Eylem | Microsoft.Web/Sites/slots/metricdefinitions/Read | Web Apps yuvaları ölçüm tanımlarını Al |
> | Eylem | Microsoft.Web/Sites/slots/Metrics/Read | Web Apps yuvaları ölçümleri alın. |
> | Eylem | microsoft.web/sites/slots/migratemysql/read | Web alma uygulamaları yuvaları MySql geçirme. |
> | Eylem | Microsoft.Web/Sites/slots/networktrace/Action | Ağ izleme Web Apps yuvası. |
> | Eylem | Microsoft.Web/Sites/slots/networktraces/operationresults/Read | Web Apps Yuvaları ağ izleme İşlem sonuçlarını alır. |
> | Eylem | Microsoft.Web/Sites/slots/NewPassword/Action | NewPassword Web Apps yuvası. |
> | Eylem | Microsoft.Web/Sites/slots/operationresults/Read | Web Apps yuvaları İşlem sonuçlarını alır. |
> | Eylem | Microsoft.Web/Sites/slots/Operations/Read | Web Apps yuvaları işlemleri Al. |
> | Eylem | Microsoft.Web/Sites/slots/perfcounters/Read | Web Apps yuvaları performans sayaçları alın. |
> | Eylem | Microsoft.Web/Sites/slots/phplogging/Read | Web Apps yuvaları Phplogging alın. |
> | Eylem | Microsoft.Web/Sites/slots/premieraddons/DELETE | Web Apps yuvaları Premier Addons silin. |
> | Eylem | Microsoft.Web/Sites/slots/premieraddons/Read | Web Apps yuvaları Premier Addons alın. |
> | Eylem | Microsoft.Web/Sites/slots/premieraddons/Write | Web Apps yuvaları Premier Addons güncelleştirin. |
> | Eylem | Microsoft.Web/Sites/slots/Processes/Read | Web Apps yuvaları işlemleri alın. |
> | Eylem | microsoft.web/sites/slots/publiccertificates/delete | Web Apps yuvaları ortak sertifikalar silin. |
> | Eylem | Microsoft.Web/Sites/slots/publiccertificates/Read | Web Apps yuvaları ortak sertifikaları edinin. |
> | Eylem | Microsoft.Web/Sites/slots/publiccertificates/Write | Oluşturun veya Web Apps yuvaları ortak sertifikaları güncelleştirin. |
> | Eylem | Microsoft.Web/sites/slots/publish/Action | Bir Web uygulaması yuvası yayımlama |
> | Eylem | Microsoft.Web/sites/slots/publishxml/Action | Profil xml yayımlamak için Web uygulaması yuvası Al |
> | Eylem | Microsoft.Web/sites/slots/Read | Bir Web uygulaması dağıtım yuvası özelliklerini alma |
> | Eylem | Microsoft.Web/Sites/slots/RECOVER/Action | Web Apps yuvaları kurtarın. |
> | Eylem | Microsoft.Web/sites/slots/resetSlotConfig/Action | Web uygulaması yuvası yapılandırmasını Sıfırla |
> | Eylem | Microsoft.Web/Sites/slots/resourcehealthmetadata/Read | Web Apps yuvaları kaynak sistem durumu meta verilerini alın. |
> | Eylem | Microsoft.Web/sites/slots/restart/Action | Bir Web uygulaması yuvası yeniden başlatın |
> | Eylem | Microsoft.Web/Sites/slots/Restore/Read | Web Apps yuvaları geri alın. |
> | Eylem | Microsoft.Web/Sites/slots/Restore/Write | Web Apps yuvaları geri yükleyin. |
> | Eylem | Microsoft.Web/Sites/slots/restorefrombackupblob/Action | Web Apps yuvası yedekleme Blobundan geri yükleyin. |
> | Eylem | microsoft.web/sites/slots/restorefromdeletedwebapp/action | Web uygulaması yuvaları silindi uygulamadan geri yükleyin. |
> | Eylem | Microsoft.Web/Sites/slots/restoresnapshot/Action | Web Apps yuvaları anlık görüntü geri yükleyin. |
> | Eylem | microsoft.web/sites/slots/siteextensions/delete | Web Apps yuvaları Site uzantılarını silin. |
> | Eylem | Microsoft.Web/Sites/slots/siteextensions/Read | Web Apps yuvaları Site uzantılarını edinin. |
> | Eylem | Microsoft.Web/Sites/slots/siteextensions/Write | Web Apps yuvaları Site uzantıları güncelleştirin. |
> | Eylem | Microsoft.Web/sites/slots/slotsdiffs/Action | Web uygulaması yuvası arasındaki farklar yapılandırma Al |
> | Eylem | Microsoft.Web/sites/slots/slotsswap/Action | Web App dağıtım yuvalarını değiştirme |
> | Eylem | Microsoft.Web/Sites/slots/snapshots/Read | Web Apps yuvaları anlık görüntüleri alın. |
> | Eylem | Microsoft.Web/sites/slots/sourcecontrols/Delete | Web uygulaması yuvanın kaynak denetimi yapılandırma ayarları Sil |
> | Eylem | Microsoft.Web/sites/slots/sourcecontrols/Read | Web uygulaması yuvanın kaynak denetimi yapılandırma ayarlarını alma |
> | Eylem | Microsoft.Web/sites/slots/sourcecontrols/Write | Web uygulaması yuvanın kaynak denetimi yapılandırma ayarlarını güncelleştirme |
> | Eylem | Microsoft.Web/sites/slots/start/Action | Bir Web uygulaması yuvası Başlat |
> | Eylem | Microsoft.Web/sites/slots/stop/Action | Bir Web uygulaması yuvası Durdur |
> | Eylem | Microsoft.Web/Sites/slots/Sync/Action | Web Apps yuvaları eşitleme. |
> | Eylem | microsoft.web/sites/slots/triggeredwebjobs/delete | Web Apps yuvaları Tetiklenmiş Web işleri'ni silin. |
> | Eylem | Microsoft.Web/Sites/slots/triggeredwebjobs/Read | Web Apps yuvaları Tetiklenmiş Web işleri'ni alın. |
> | Eylem | microsoft.web/sites/slots/triggeredwebjobs/run/action | Web Apps yuvaları Tetiklenmiş Web işleri'ni çalıştırın. |
> | Eylem | Microsoft.Web/Sites/slots/usages/Read | Web Apps yuvaları kullanımları alın. |
> | Eylem | Microsoft.Web/Sites/slots/virtualnetworkconnections/DELETE | Web Apps yuvaları sanal ağ bağlantılarını silin. |
> | Eylem | Microsoft.Web/Sites/slots/virtualnetworkconnections/Gateways/Write | Web Apps yuvaları sanal ağ bağlantıları Gateway bileşenlerini güncelleştirin. |
> | Eylem | Microsoft.Web/Sites/slots/virtualnetworkconnections/Read | Web Apps yuvaları sanal ağ bağlantılarını alın. |
> | Eylem | Microsoft.Web/Sites/slots/virtualnetworkconnections/Write | Web Apps yuvaları sanal ağ bağlantıları'nı güncelleştirin. |
> | Eylem | Microsoft.Web/Sites/slots/webjobs/Read | Web Apps yuvaları WebJobs alın. |
> | Eylem | Microsoft.Web/sites/slots/Write | Yeni bir Web uygulaması yuvası oluşturun veya var olan bir güncelleştirme |
> | Eylem | Microsoft.Web/sites/slotsdiffs/Action | Web uygulaması yuvası arasındaki farklar yapılandırma Al |
> | Eylem | Microsoft.Web/sites/slotsswap/Action | Web App dağıtım yuvalarını değiştirme |
> | Eylem | Microsoft.Web/Sites/snapshots/Read | Web Apps anlık görüntüleri alın. |
> | Eylem | Microsoft.Web/sites/sourcecontrols/Delete | Web uygulamasının kaynak denetiminin yapılandırma ayarlarını Sil |
> | Eylem | Microsoft.Web/sites/sourcecontrols/Read | Web uygulamasının kaynak denetimi yapılandırma ayarlarını alma |
> | Eylem | Microsoft.Web/sites/sourcecontrols/Write | Web uygulamasının kaynak denetiminin yapılandırma ayarlarını güncelleştirme |
> | Eylem | Microsoft.Web/sites/start/Action | Bir Web uygulaması başlatın |
> | Eylem | Microsoft.Web/sites/stop/Action | Bir Web uygulamasını Durdur |
> | Eylem | Microsoft.Web/Sites/Sync/Action | Sync Web Apps. |
> | Eylem | Microsoft.Web/Sites/syncfunctiontriggers/Action | Web Apps için eşitleme işlevi tetikler. |
> | Eylem | microsoft.web/sites/triggeredwebjobs/delete | Web Apps Tetiklenmiş Web işleri silin. |
> | Eylem | Microsoft.Web/Sites/triggeredwebjobs/History/Read | Web Apps Tetiklenmiş Web işleri geçmişi Al |
> | Eylem | Microsoft.Web/Sites/triggeredwebjobs/Read | Web Apps Tetiklenmiş Web işleri alın. |
> | Eylem | Microsoft.Web/Sites/triggeredwebjobs/Run/Action | Web Apps Tetiklenmiş Web işleri'ni çalıştırın. |
> | Eylem | Microsoft.Web/Sites/usages/Read | Web Apps kullanımlarını Al. |
> | Eylem | Microsoft.Web/Sites/virtualnetworkconnections/DELETE | Web Apps sanal ağ bağlantılarını silin. |
> | Eylem | Microsoft.Web/Sites/virtualnetworkconnections/Gateways/Read | Web Apps sanal ağ bağlantıları ağ geçitleri alın. |
> | Eylem | Microsoft.Web/Sites/virtualnetworkconnections/Gateways/Write | Web Apps sanal ağ bağlantıları Gateway bileşenlerini güncelleştirin. |
> | Eylem | Microsoft.Web/Sites/virtualnetworkconnections/Read | Web Apps sanal ağ bağlantılarını alın. |
> | Eylem | Microsoft.Web/Sites/virtualnetworkconnections/Write | Web Apps sanal ağ bağlantıları'nı güncelleştirin. |
> | Eylem | Microsoft.Web/Sites/webjobs/Read | Get Web Apps WebJobs. |
> | Eylem | Microsoft.Web/sites/Write | Yeni bir Web uygulaması oluşturma veya var olan bir güncelleştirme |
> | Eylem | Microsoft.Web/skus/Read | SKU'ları alın. |
> | Eylem | Microsoft.Web/sourcecontrols/Read | Kaynak Denetim alın. |
> | Eylem | Microsoft.Web/sourcecontrols/Write | Kaynak Denetim güncelleştirin. |
> | Eylem | microsoft.web/unregister/action | Abonelik için Microsoft.Web kaynak sağlayıcısının kaydını siler. |
> | Eylem | Microsoft.Web/Validate/Action | Doğrulayın. |
> | Eylem | Microsoft.Web/verifyhostingenvironmentvnet/Action | Barındırma ortamı Vnet doğrulayın. |

## <a name="microsoftworkloadmonitor"></a>Microsoft.WorkloadMonitor

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.WorkloadMonitor/components/read | Bileşenler için kaynak alır |
> | Eylem | Microsoft.WorkloadMonitor/componentsSummary/read | Bileşenleri Özet alır |
> | Eylem | Microsoft.WorkloadMonitor/monitorInstances/read | Kaynak için İzleyici örneklerini alır |
> | Eylem | Microsoft.WorkloadMonitor/monitorInstancesSummary/read | İzleyici örneklerini Özet alır |
> | Eylem | Microsoft.WorkloadMonitor/monitors/read | İzleyiciler için kaynak alır |
> | Eylem | Microsoft.WorkloadMonitor/monitors/write | Kaynak için Yapılandır |
> | Eylem | Microsoft.WorkloadMonitor/notificationSettings/read | Kaynak için bildirim ayarlarını alır. |
> | Eylem | Microsoft.WorkloadMonitor/notificationSettings/write | Kaynak için bildirimi ayarlarını yapılandırma |
> | Eylem | Microsoft.WorkloadMonitor/operations/read | Desteklenen işlemler alır |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure kaynakları için özel roller](custom-roles.md)
- [Azure kaynakları için yerleşik roller](built-in-roles.md)
