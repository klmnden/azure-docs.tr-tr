---
title: Azure Resource Manager sağlayıcısı işlemleri | Microsoft Docs
description: Microsoft Azure Resource Manager kaynak sağlayıcılarla işlemleri ayrıntıları
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/06/2018
ms.author: rolyon
ms.openlocfilehash: 0b8c8823c6d21df96dcfd926db1855169f1570e4
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/11/2018
---
# <a name="azure-resource-manager-resource-provider-operations"></a>Azure Resource Manager kaynak sağlayıcısı işlemleri

Bu belge her Microsoft Azure Resource Manager kaynak sağlayıcısı için kullanılabilir olan işlemleri listeler. Bu özel rollerinde Azure kaynaklarına ayrıntılı rol tabanlı erişim denetimi (RBAC) izinlerini sağlamak için kullanılabilir. Lütfen bu kapsamlı bir liste değildir ve işlemleri eklenen veya her bir sağlayıcı güncelleştirilmiş kaldırıldıkça unutmayın. İşlemi dizeleri izleyin biçimi `Microsoft.<ProviderName>/<ChildResourceType>/<action>`. 

> [!NOTE]
> Geçerli ve kapsamlı bir liste için lütfen kullanın `Get-AzureRmProviderOperation` (PowerShell'de) veya `az provider operation list` (Azure CLI v2), Azure kaynak sağlayıcılarının listesi işlemleri için.

## <a name="microsoftaad"></a>Microsoft.AAD

| İşlem | Açıklama |
|---|---|
|/domainServices/delete|Etki Alanı Hizmetleri siler.|
|/domainServices/read|Etki Alanı Hizmetleri okur.|
|/domainServices/write|Etki Alanı Hizmetleri yazma|
|/Locations/operationresults/Read|Zaman uyumsuz bir işlem durumunu okur.|
|/ Operations/okuma|Yerelleştirilmiş kolay açıklaması olarak işlemi, kullanıcıya gösterilen.|

## <a name="microsoftaadiam"></a>microsoft.aadiam

| İşlem | Açıklama |
|---|---|
|/diagnosticsettings/DELETE|Tanılama ayarını silme|
|/diagnosticsettings/Read|Tanılama ayarını okuma|
|/diagnosticsettings/write|Tanılama ayarı yazılıyor|
|/diagnosticsettingscategories/read|Tanılama ayarını kategorileri okuma|
|/tenants/providers/Microsoft.Insights/diagnosticSettings/Read|Kaynak için tanılama ayarını alır|
|/tenants/providers/Microsoft.Insights/diagnosticSettings/write|Kaynak için tanılama ayarını oluşturur veya güncelleştirir|
|/tenants/providers/Microsoft.Insights/logDefinitions/read|Kiracılar için kullanılabilir günlüklerini alır|

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService

| İşlem | Açıklama |
|---|---|
|/ configuration/eylem|Güncelleştirmeler Kiracı yapılandırması.|
|/Configuration/Read|Kiracı yapılandırması okur.|
|/configuration/write|Bir kiracı yapılandırma oluşturur.|
|/ services/eylem|Kiracı içinde bir hizmet örneği güncelleştirir.|
|/services/alerts/read|Bir hizmet uyarılarını okur.|
|/services/alerts/read|Bir hizmet uyarılarını okur.|
|/services/delete|Kiracı içinde bir hizmet örneği siler.|
|/Services/Read|Hizmet örnekleri kiracısında okur.|
|/services/servicemembers/action|Bir hizmet üye örneği hizmetinde oluşturur.|
|/services/servicemembers/alerts/read|Uyarılar için bir hizmet üye okur.|
|/services/servicemembers/delete|Bir hizmet üye örneği hizmet siler.|
|/services/servicemembers/read|Hizmet hizmet üye örneği okur.|
|/ services/yazma|Bir hizmet örneği Kiracı oluşturur.|

## <a name="microsoftadvisor"></a>Microsoft.Advisor

| İşlem | Açıklama |
|---|---|
|/configurations/Read|Yapılandırmalarını alma|
|/configurations/write|/ Güncelleştirme yapılandırma|
|/generateRecommendations/action|Öneriler oluşturur|
|/generateRecommendations/read|Önerileri durumuna alır oluştur|
|/Operations/Read|Microsoft Advisor işlemlerinde alır|
|/Recommendations/Read|Öneriler okur|
|/Recommendations/suppressions/DELETE|Gizleme siler|
|/Recommendations/suppressions/Read|Suppressions alır|
|/Recommendations/suppressions/Write|/ Suppressions güncelleştirme|
|/register/action|Microsoft Advisor için aboneliği kaydeder|
|/suppressions/DELETE|Gizleme siler|
|/suppressions/Read|Suppressions alır|
|/ suppressions/yazma|/ Suppressions güncelleştirme|
|/unregister/action|Abonelik için Microsoft Advisor kaydını siler|

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement

| İşlem | Açıklama |
|---|---|
|/Alerts/Read|Tüm uyarılar için giriş filtrelerini alır.|
|/alerts/resolve/action|'Çözümleyip' uyarının durumunu değiştirme|
|/alertsSummary/read|Uyarıların özetini alın|
|/ Operations/okuma|Sağlanan işlemleri okur|

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

| İşlem | Açıklama |
|---|---|
|/locations/checkNameAvailability/action|Analysis Server adının geçerli olup olmadığını denetler ve içinde değil kullanın.|
|/Locations/operationresults/Read|Belirtilen işlem sonucu bilgileri alır.|
|/Locations/operationstatuses/Read|Belirtilen işlem durumu bilgileri alır.|
|/Operations/Read|İşlem bilgileri alır.|
|/register/action|Analysis Services kaynağı sağlayıcısını kaydeder.|
|/servers/delete|Analiz sunucusu siler.|
|/servers/listGatewayStatus/action|Sunucuyla ilişkili ağ geçidi durumunu listeler.|
|/servers/providers/Microsoft.Insights/diagnosticSettings/read|Analysis Server tanılama ayarını alır|
|/servers/providers/Microsoft.Insights/diagnosticSettings/write|Oluşturur veya Analysis Server tanılama ayarını güncelleştirir|
|/servers/providers/Microsoft.Insights/logDefinitions/read|Sunucuları için kullanılabilir günlüklerini alır|
|/servers/providers/Microsoft.Insights/metricDefinitions/read|Analiz sunucusu için kullanılabilir ölçümleri alır|
|/Servers/Read|Belirtilen Analysis Server'ın bilgilerini alır.|
|/servers/resume/action|Analiz sunucusu sürdürür.|
|/Servers/skus/Read|Sunucu için kullanılabilir SKU bilgilerini alma|
|/Servers/suspend/Action|Analiz sunucusu askıya alır.|
|/ sunucuları/yazma|Oluşturur veya belirtilen Analysis Server güncelleştirir.|
|/skus/Read|SKU'ları bilgileri alır.|

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

| İşlem | Açıklama |
|---|---|
|/checkNameAvailability/read|Hizmet adı kullanılabilir sağladıysanız denetler|
|/Operations/Read|Tüm API kullanılabilir Microsoft.ApiManagement kaynak için okuma işlemleri|
|/register/action|Microsoft.ApiManagement kaynak sağlayıcısı için abonelik kaydı|
|/Reports/Read|Dönemleri, coğrafi bölge, geliştiriciler, ürünler, API'leri, işlemleri, abonelik ve byRequest tarafından toplanan raporlar alın.|
|/service/apis/delete|Varolan API Kaldır|
|/service/apis/diagnostics/delete|Varolan tanılama Kaldır|
|/service/apis/diagnostics/loggers/delete|Günlükçü tanılama ayarı olan eşlemeyi kaldırın|
|/service/apis/diagnostics/loggers/read|Varolan tanılama günlükçüleri listesini al|
|/service/apis/diagnostics/loggers/write|Harita Günlükçü tanılama ayarı|
|/service/apis/diagnostics/read|Tanılama veya tanılama Ayrıntıları Al listesini al|
|/service/apis/diagnostics/write|Yeni tanılama ekleme veya güncelleştirme mevcut tanılama ayrıntıları|
|/service/apis/operations/delete|Varolan API işlemi Kaldır|
|/service/apis/operations/policies/delete|API işlemi ilkelerden ilke yapılandırmasını Kaldır|
|/Service/apis/Operations/Policies/Read|İlkeleri API işlemi veya alma ilkesi yapılandırma ayrıntıları alma için API işlemi|
|/service/apis/operations/policies/write|API işlem için ilke yapılandırma ayrıntılarını ayarlayın|
|/service/apis/operations/policy/delete|İlke yapılandırma işlemi Kaldır|
|/Service/apis/Operations/Policy/Read|İlke yapılandırma ayrıntılarını alma işlemi için|
|/service/apis/operations/policy/write|İşlem için ilke yapılandırma ayrıntılarını ayarlayın|
|/Service/apis/Operations/Read|Mevcut API işlemleri listesini almak veya API işlemi ayrıntılarını alma|
|/service/apis/operations/tags/delete|Varolan bir etiketi mevcut işlemi ile ilişkisini silme|
|/service/apis/operations/tags/read|İşlem ya da almak etiketi ayrıntılarla ilişkili etiketleri Al|
|/service/apis/operations/tags/write|Varolan etiketi işlemle ilişkilendirme|
|/service/apis/operations/write|Yeni API işlemi oluşturma veya güncelleştirme mevcut API işlemi|
|/service/apis/operationsByTags/read|İşlem/etiketi ilişkilendirmeleri listesini al|
|/service/apis/policies/delete|API ilkelerden ilke yapılandırmasını Kaldır|
|/Service/apis/Policies/Read|İlkeleri için API API veya alma ilkesi yapılandırma ayrıntılarını alma|
|/service/apis/policies/write|İçin API ilkesi yapılandırma ayrıntılarını ayarlayın|
|/service/apis/policy/delete|İlke Yapılandırması API öğesinden Kaldır|
|/service/apis/policy/read|İçin API ilkesi yapılandırma ayrıntılarını alma|
|/service/apis/policy/write|İçin API ilkesi yapılandırma ayrıntılarını ayarlayın|
|/service/apis/products/read|API parçası olan tüm ürünleri alma|
|/service/apis/read|Tüm kayıtlı API veya Get ayrıntılarını API listesini al|
|/service/apis/releases/delete|API ya da kaldırma API yayın tüm sürümleri kaldırır|
|/service/apis/releases/read|Bir API reelase API veya Get ayrıntılarını Get sürümleri|
|/service/apis/releases/write|Yeni API sürüm oluşturma veya varolan API sürümü güncelleştirme|
|/service/apis/revisions/delete|Bir API'nin tüm düzeltmelerini kaldırır|
|/service/apis/revisions/read|API'ye ait düzeltmelerini Al|
|/service/apis/schemas/delete|Şema varolan kaldırır|
|/service/apis/schemas/document/read|Şema açıklayan belge Al|
|/service/apis/schemas/document/write|Şema açıklayan belge güncelleştir|
|/service/apis/schemas/read|Belirli bir API için tüm şemaları alır veya API tarafından kullanılan şemaları alır|
|/service/apis/schemas/write|API tarafından kullanılan şemalar ayarlar|
|/service/apis/tagDescriptions/delete|Etiket açıklama API öğesinden Kaldır|
|/service/apis/tagDescriptions/read|Etiketler açıklamaları API veya alma etiketi açıklama kapsamında API kapsamında alın.|
|/service/apis/tagDescriptions/write|Oluştur/Değiştir etiketi açıklaması kapsamında API|
|/service/apis/tags/delete|Varolan API/etiketi ilişkilendirmesini Kaldır|
|/service/apis/tags/read|Tüm API/etiketi ilişkilendirme için API/etiketi ilişkilendirme API veya Get ayrıntılarını alma|
|/service/apis/tags/write|Yeni API/etiketi ilişkilendirmesini Ekle|
|/service/apis/write|Yeni API oluşturma veya güncelleştirme mevcut API ayrıntıları|
|/service/apisByTags/read|API/etiketi ilişkilendirmeleri listesini al|
|/service/api-version-sets/delete|Varolan VersionSet Kaldır|
|/Service/api-Version-Sets/Read|Sürüm grubu varlıkları veya bir VersionSet alır ayrıntılarını listesini al|
|/service/api-version-sets/versions/read|Sürüm varlıkların listesini al|
|/service/api-version-sets/write|Yeni VersionSet oluşturma veya güncelleştirme mevcut VersionSet ayrıntıları|
|/service/applynetworkconfigurationupdates/action|Güncelleştirilmiş ağ ayarları seçmek üzere sanal ağda çalışan Microsoft.ApiManagement kaynakları güncelleştirir.|
|/service/authorizationServers/delete|Varolan bir yetkilendirme sunucusu Kaldır|
|/service/authorizationServers/read|Yetkilendirme sunucularının listesini alın veya yetkilendirme sunucusu ayrıntılarını alma|
|/service/authorizationServers/write|Var olan bir yetkilendirme sunucusu Güncelleştirme ayrıntıları veya yeni bir yetkilendirme sunucusu oluşturma|
|/service/backends/delete|Varolan arka uç Kaldır|
|/service/backends/read|Arka uçlarını listesini almak veya arka uç ayrıntılarını alma|
|/service/backends/reconnect/action|Yeniden bağlanma isteği oluşturma|
|/service/backends/write|Yeni bir arka uç ekleme veya güncelleştirme mevcut arka uç ayrıntıları|
|/service/backup/action|Bir kullanıcı belirtilen kapsayıcıda yedekleme API Management hizmetine sağlanan depolama hesabı|
|/service/certificates/delete|Varolan bir sertifikayı Kaldır|
|/Service/Certificates/Read|Sertifikaların listesini alın veya sertifika ayrıntılarını alma|
|/service/certificates/write|Yeni sertifika Ekle|
|/service/delete|API Management hizmeti örneği Sil|
|/service/diagnostics/delete|Varolan tanılama Kaldır|
|/service/diagnostics/loggers/delete|Günlükçü tanılama ayarı olan eşlemeyi kaldırın|
|/service/diagnostics/loggers/read|Varolan tanılama günlükçüleri listesini al|
|/service/diagnostics/loggers/write|Harita Günlükçü tanılama ayarı|
|/service/diagnostics/read|Tanılama veya tanılama Ayrıntıları Al listesini al|
|/service/diagnostics/write|Yeni tanılama ekleme veya güncelleştirme mevcut tanılama ayrıntıları|
|/service/getssotoken/action|Kullanılabilir alır SSO belirteci için API Management hizmet eski portalı yönetici olarak oturum aç|
|/service/groups/delete|Varolan bir grubu Kaldır|
|/Service/Groups/Read|Grupları veya bir grup alır ayrıntılarını listesini al|
|/service/groups/users/delete|Varolan bir kullanıcı var olan grubundan Kaldır|
|/Service/Groups/Users/Read|Grup kullanıcıların listesini al|
|/Service/Groups/Users/Write|Varolan bir gruba varolan kullanıcı Ekle|
|/Service/Groups/Write|Yeni grubu oluşturmak veya var olan Grup ayrıntılarını güncelleştir|
|/service/identityProviders/delete|Varolan kimlik sağlayıcısı Kaldır|
|/service/identityProviders/read|Kimlik sağlayıcısı Get ayrıntıları veya kimlik sağlayıcıları listesini al|
|/service/identityProviders/write|Varolan bir kimlik sağlayıcısı'nın yeni bir kimlik sağlayıcısı veya Güncelleştirme ayrıntıları oluşturun|
|/service/locations/networkstatus/read|Ağ erişim durumu, hizmetin bağımlı olduğu kaynaklar konumu alır.|
|/service/loggers/delete|Varolan Günlükçü Kaldır|
|/Service/loggers/Read|Günlükçüleri listesini almak veya Günlükçü ayrıntılarını alma|
|/service/loggers/write|Yeni Günlükçü ekleme veya güncelleştirme mevcut Günlükçü ayrıntıları|
|/service/managedeployments/action|SKU/birimlerini, API Management Hizmet Ekle/Kaldır bölgesel dağıtımları değiştirme|
|/service/networkstatus/read|Hizmetin bağımlı olduğu kaynaklar ağ erişim durumunu alır.|
|/service/notifications/action|Belirtilen bir kullanıcıya bildirim gönderir|
|/service/notifications/read|Tüm API Management yayımcı bildirimleri veya alma API Management yayımcı bildirim ayrıntılarını alır|
|/service/notifications/recipientEmails/delete|Var olan e-posta bildirim ile ilişkilendirilmiş kaldırır|
|/service/notifications/recipientEmails/read|E-posta alıcılarını API Management yayımcı bildirim ile ilişkilendirilmiş Al|
|/service/notifications/recipientEmails/write|Yeni e-posta alıcısının bildirim oluşturma|
|/service/notifications/recipientUsers/delete|Bildirim alıcılarını ilişkili kullanıcıyı kaldırır|
|/service/notifications/recipientUsers/read|Alıcı bildirim ile ilişkilendirilmiş kullanıcılar|
|/service/notifications/recipientUsers/write|Kullanıcı için bildirim alıcıları ekleyin|
|/service/notifications/write|Oluşturma veya güncelleştirme API Management yayımcı bildirim|
|/service/openidConnectProviders/delete|Varolan Openıd Connect sağlayıcısını Kaldır|
|/service/openidConnectProviders/read|Openıd Connect sağlayıcısı Get ayrıntıları veya Openıd Connect sağlayıcıları listesini al|
|/service/openidConnectProviders/write|Bağlantı varolan bir Openıd sağlayıcısının yeni bir Openıd Connect sağlayıcısı veya Güncelleştirme ayrıntıları oluşturun|
|/service/operationresults/read|Uzun süre çalışan işlemin geçerli durumunu alır|
|/Service/Policies/DELETE|Kiracı ilkelerden ilke yapılandırmasını Kaldır|
|/Service/Policies/Read|İlkeleri Kiracısı için Kiracı veya alma ilkesi yapılandırma ayrıntılarını alma|
|/Service/Policies/Write|Kiracı için ilke yapılandırma ayrıntılarını ayarlayın|
|/service/policySnippets/read|Tüm ilke parçacıkları Al|
|/service/portalsettings/read|Portalı veya Get portalı için ayarları'nda oturum açın veya Portal için temsilci ayarlarını almak için oturum ayarlarını al|
|/service/portalsettings/write|Kaydol ayarları veya güncelleştirme Kaydol ayarları veya güncelleştirme oturum ayarları veya güncelleştirme oturum ayarları veya güncelleştirme temsilci ayarları veya güncelleştirme temsilci ayarlarını güncelleştirme|
|/service/products/apis/delete|Var olan ürün varolan API Kaldır|
|/service/products/apis/read|Varolan bir ürüne eklenmiş API'leri listesini al|
|/service/products/apis/write|Varolan bir ürüne varolan API ekleme|
|/service/products/delete|Varolan ürünü kaldırın|
|/service/products/groups/delete|Varolan Ürün mevcut Geliştirici grubunun ilişkilendirmesini Sil|
|/Service/Products/Groups/Read|Ürünle ilişkili Geliştirici gruplarının listesini alma|
|/Service/Products/Groups/Write|Varolan bir geliştirici Grup varolan ürünle ilişkilendirin|
|/service/products/policies/delete|Ürün ilkelerden ilke yapılandırmasını Kaldır|
|/Service/Products/Policies/Read|İlkeleri ürün veya alma ilkesi yapılandırma ayrıntıları için ürün alın.|
|/service/products/policies/write|İlke yapılandırma ayrıntıları için ürün ayarlayın|
|/service/products/policy/delete|Var olan ürün ilkesi yapılandırmasını Kaldır|
|/service/products/policy/read|Varolan ürünün ilke yapılandırmasını alma|
|/service/products/policy/write|Varolan bir ürün için ilke yapılandırma kümesi|
|/Service/Products/Read|Ürünlerin listesini al veya ürün ayrıntılarını Al|
|/Service/Products/Subscriptions/Read|Ürün Aboneliklerin listesini alın|
|/service/products/tags/delete|Varolan bir etiketi mevcut ürün ile ilişkisini silme|
|/Service/Products/Tags/Read|Ürün veya alma etiketi ayrıntılarla ilişkili etiketleri Al|
|/service/products/tags/write|Varolan bir etiketi mevcut ürünle ilişkilendirin|
|/service/products/write|Yeni ürün oluşturmak veya mevcut ürün ayrıntıları güncelleştir|
|/Service/Properties/DELETE|Özelliği varolan kaldırır|
|/Service/Properties/Read|Belirtilen özellik ayrıntılarını alır veya tüm özelliklerinin listesini alır|
|/Service/Properties/Write|Yeni bir özellik oluşturur veya belirtilen özellik için değer güncelleştirir|
|/service/providers/Microsoft.Insights/diagnosticSettings/read|API Management hizmeti tanılama ayarını alır|
|/service/providers/Microsoft.Insights/diagnosticSettings/write|Oluşturur veya API Management hizmeti tanılama ayarını güncelleştirir|
|/service/providers/Microsoft.Insights/logDefinitions/read|API Management hizmeti için kullanılabilir günlüklerini alır|
|/service/providers/Microsoft.Insights/metricDefinitions/read|API Management hizmeti için kullanılabilir ölçümleri alır|
|/service/quotas/periods/read|Dönem için kota sayaç değeri Al|
|/Service/Quotas/Periods/Write|Kota sayaç geçerli değeri ayarlayın|
|/service/quotas/read|İçin kota değerlerini alma|
|/service/quotas/write|Kota sayaç geçerli değeri ayarlayın|
|/Service/Read|API Management hizmeti örneği ile ilgili meta verilerini okuma|
|/service/reports/read|Dönemleri ya da coğrafi bölge veya geliştiriciler tarafından toplanan Get rapor ile toplanmış Get raporu tarafından toplanan rapor alın. veya ürünü tarafından toplanan rapor alın. veya işlemleri veya abonelik tarafından toplanan Get raporu tarafından toplanan API'leri veya Get raporu tarafından toplanan rapor alın. veya istekleri raporlama verilerini alma|
|/service/restore/action|API Management hizmeti belirtilen bir kullanıcı tarafından sağlanan depolama hesabı kapsayıcısında geri yükleme|
|/Service/Subscriptions/DELETE|Aboneliği silin. Bu işlem aboneliğini silmek için kullanılabilir|
|/service/subscriptions/read|Ürün Aboneliklerin listesini alın veya ürün aboneliği ayrıntılarını alma|
|/service/subscriptions/regeneratePrimaryKey/action|Abonelik birincil anahtarını yeniden oluşturma|
|/service/subscriptions/regenerateSecondaryKey/action|Abonelik ikincil anahtarını yeniden oluşturma|
|/service/subscriptions/write|Mevcut bir kullanıcının var olan bir ürüne abone veya var olan abonelik ayrıntıları güncelleştirin. Bu işlem, aboneliğinizi yenilemek için kullanılabilir|
|/service/tagResources/read|İlişkilendirilmiş kaynakları olan etiketleri listesini al|
|/service/tags/delete|Varolan etiketi Kaldır|
|/service/tags/read|Etiketler listesini almak veya etiket ayrıntılarını alma|
|/service/tags/write|Yeni etiket eklemek veya mevcut etiket ayrıntılarını güncelleştir|
|/Service/Templates/DELETE|Varsayılan API Management e-posta şablonu Sıfırla|
|/Service/Templates/Read|Tüm e-posta şablonlarını veya API Management alır e-posta şablonu ayrıntılarını alır|
|/Service/Templates/Write|API Management e-posta şablonu ya güncelleştirmeleri API Management e-posta şablonu oluştur veya güncelleştir|
|/Service/tenant/DELETE|Kiracı için ilke yapılandırmasını kaldırın|
|/service/tenant/deploy/action|Değişiklikler veritabanına yapılandırmasında belirtilen git daldan uygulamak için bir dağıtım görev çalıştırır.|
|/service/tenant/operationResults/read|İşlem sonuçları listesini al veya belirli bir işlemin sonucunu Al|
|/Service/tenant/Read|İlke Yapılandırması Kiracı veya Get Kiracı için erişim bilgileri ayrıntıları|
|/service/tenant/regeneratePrimaryKey/action|Birincil erişim anahtarını yeniden oluşturma|
|/service/tenant/regenerateSecondaryKey/action|İkincil erişim anahtarını yeniden oluşturma|
|/Service/tenant/Save/Action|Yürütme deposunda belirtilen dal yapılandırma anlık görüntü oluşturur|
|/service/tenant/syncState/read|En son git eşitleme durumunu Al|
|/service/tenant/validate/action|Belirtilen git şube değişikliklerden doğrular|
|/Service/tenant/Write|Kiracı veya güncelleştirme Kiracı erişim bilgileri ayrıntıları ilke yapılandırmasını ayarlayın|
|/service/updatecertificate/action|Bir API Management hizmeti için SSL sertifikasını karşıya yükle|
|/service/updatehostname/action|Kurulum, güncelleştirmeniz ya da bir API Management hizmeti için özel etki alanı adlarını kaldırın|
|/Service/Users/Action|Yeni bir kullanıcı kaydı|
|/service/users/applications/attachments/delete|Bir ek kaldırır|
|/service/users/applications/attachments/read|Uygulama ekler veya alır eki alır|
|/service/users/applications/attachments/write|Uygulama eki ekleyin|
|/service/users/applications/delete|Uygulama varolan kaldırır|
|/Service/Users/Applications/Read|Tüm kullanıcı uygulamaları veya API Management alır uygulama ayrıntıları listesini al|
|/Service/Users/Applications/Write|API Management veya güncelleştirmeler uygulama ayrıntıları uygulamaya kaydeder|
|/Service/Users/DELETE|Kullanıcı hesabını kaldır|
|/service/users/generateSsoUrl/action|SSO URL oluşturur. URL yönetim portalına erişmek için kullanılabilir|
|/Service/Users/Groups/Read|Kullanıcı gruplarının listesini alma|
|/Service/Users/Keys/Read|Kullanıcı anahtarları listesini al|
|/Service/Users/Read|Kayıtlı kullanıcıların listesini almak veya bir kullanıcı hesabı ayrıntılarını alma|
|/Service/Users/Subscriptions/Read|Kullanıcı abonelikleri listesini al|
|/service/users/token/action|Bir kullanıcı için belirteç erişim belirteci alma|
|/Service/Users/Write|Yeni bir kullanıcı veya varolan bir kullanıcı hesabı ayrıntılarını güncelleştirme kaydetme|
|/ service/yazma|API Management hizmeti yeni bir örneğini oluşturma|
|/unregister/action|Microsoft.ApiManagement kaynak sağlayıcısı için abonelik kaydı|

## <a name="microsoftauthorization"></a>Microsoft.Authorization

| İşlem | Açıklama |
|---|---|
|/checkAccess/action|Arayanın belirli bir eylemi gerçekleştirmek için yetkili olup olmadığını denetler|
|/classicAdministrators/delete|Yöneticiyi abonelikten çıkarır.|
|/classicAdministrators/read|Abonelik için yöneticileri okur.|
|/classicAdministrators/write|Abonelik için yönetici ekleyin veya değiştirin.|
|/elevateAccess/action|Çağırana kiracı kapsamında Kullanıcı Erişim Yöneticisi erişimi verir|
|/Locks/DELETE|Belirtilen kapsamdaki kilitleri silin.|
|/Locks/Read|Belirtilen kapsamdaki kilitleri alın.|
|/ kilitleri/yazma|Belirtilen kapsamdaki kilitleri ekleyin.|
|/Permissions/Read|Arayanın, belirtilen bir kapsamda sahip olduğu tüm izinleri listeler.|
|/policyAssignments/DELETE|Belirtilen kapsamdaki bir ilke atamasını silin.|
|/policyAssignments/read|İlke ataması hakkında bilgi edinin.|
|/policyAssignments/write|Belirtilen kapsamda bir ilke ataması oluşturun.|
|/policyDefinitions/delete|İlke tanımını silin.|
|/policyDefinitions/read|İlke tanımı hakkında bilgi edinin.|
|/policyDefinitions/write|Özel bir ilke tanımı oluşturun.|
|/policySetDefinitions/delete|Bir ilke kümesi tanımını silin.|
|/policySetDefinitions/read|Bir ilke kümesi tanımı hakkında bilgi alın.|
|/policySetDefinitions/write|Özel bir ilke kümesi tanımı oluşturun.|
|/providerOperations/read|Tüm kaynak sağlayıcılarına ilişkin işlemleri alın. Bunlar rol tanımlarında kullanılabilir.|
|/roleAssignments/DELETE|Belirtilen kapsamdaki rol atamasını silin.|
|/roleAssignments/Read|Bir rol ataması hakkında bilgi alın.|
|/ roleAssignments/yazma|Belirtilen kapsamda bir rol ataması oluşturun.|
|/roleDefinitions/delete|Belirtilen özel rol tanımını silin.|
|/roleDefinitions/Read|Rol tanımı hakkında bilgi alın.|
|/roleDefinitions/write|Belirtilen izinlerle ve atanabilir kapsamlarla birlikte özel bir rol tanımı oluşturun veya güncelleştirin.|

## <a name="microsoftautomation"></a>Microsoft.Automation

| İşlem | Açıklama |
|---|---|
|/automationAccounts/agentRegistrationInformation/read|Bir Azure Otomasyonu DSC'SİNİN kayıt bilgileri okuyun|
|/automationAccounts/agentRegistrationInformation/regenerateKey/action|Azure Otomasyonu DSC anahtarları yeniden isteği Yazar|
|/automationAccounts/certificates/delete|Bir Azure Otomasyonu sertifika varlığını siler|
|/automationAccounts/certificates/read|Bir Azure Otomasyonu sertifika varlığı alır|
|/automationAccounts/certificates/write|Bir Azure Otomasyonu sertifika varlığı oluşturur veya güncelleştirir|
|/automationAccounts/compilationjobs/Read|Bir Azure Otomasyonu DSC'SİNİN derleme okur|
|/automationAccounts/compilationjobs/Write|Bir Azure Otomasyonu DSC'SİNİN derleme Yazar|
|/automationAccounts/configurations/delete|Bir Azure Otomasyonu DSC'SİNİN içeriğini siler|
|/automationAccounts/configurations/getCount/action|Bir Azure Otomasyonu DSC'SİNİN içeriğini sayısını okur|
|/automationAccounts/configurations/read|Bir Azure Otomasyonu DSC'SİNİN içeriğini alır|
|/automationAccounts/configurations/write|Bir Azure Otomasyonu DSC'SİNİN içeriğini yazar|
|/automationAccounts/connections/delete|Bir Azure Otomasyonu bağlantı varlığını siler|
|/automationAccounts/connections/read|Bir Azure Otomasyonu bağlantı varlığı alır|
|/automationAccounts/connections/write|Bir Azure Otomasyonu bağlantı varlığı oluşturur veya güncelleştirir|
|/automationAccounts/connectionTypes/delete|Bir Azure Otomasyonu bağlantı türü varlığını siler|
|/automationAccounts/connectionTypes/read|Bir Azure Otomasyonu bağlantı türü varlığını alır|
|/automationAccounts/connectionTypes/write|Bir Azure Otomasyonu bağlantı türü varlığı oluşturur|
|/automationAccounts/credentials/delete|Bir Azure Otomasyonu kimlik bilgisi varlığını siler|
|/automationAccounts/credentials/read|Bir Azure Otomasyonu kimlik bilgisi varlığı alır|
|/automationAccounts/credentials/write|Bir Azure Otomasyonu kimlik bilgisi varlığı oluşturur veya güncelleştirir|
|/automationAccounts/delete|Bir Azure Otomasyonu hesabını siler|
|/automationAccounts/diagnosticSettings/read|Kaynak için tanılama ayarını alır|
|/automationAccounts/diagnosticSettings/write|Kaynağın tanılama ayarını ayarlar|
|/automationAccounts/hybridRunbookWorkerGroups/delete|Karma Runbook çalışanı kaynaklarını siler|
|/automationAccounts/hybridRunbookWorkerGroups/read|Karma Runbook çalışanı kaynaklarını okur|
|/automationAccounts/jobs/output/action|Bir işin çıktısını alır|
|/automationAccounts/jobs/output/action|Bir işin çıktısını alır|
|/automationAccounts/jobs/read|Bir Azure Otomasyonu işini alır|
|/automationAccounts/jobs/read|Bir Azure Otomasyonu işini alır|
|/automationAccounts/Jobs/Resume/Action|Bir Azure Otomasyonu işini sürdürür|
|/automationAccounts/Jobs/Resume/Action|Bir Azure Otomasyonu işini sürdürür|
|/automationAccounts/jobs/runbookContent/action|İş yürütme sırasında Azure Otomasyonu runbook içeriğini alır|
|/automationAccounts/jobs/runbookContent/action|İş yürütme sırasında Azure Otomasyonu runbook içeriğini alır|
|/automationAccounts/jobs/stop/action|Bir Azure Otomasyonu işini durdurur|
|/automationAccounts/jobs/stop/action|Bir Azure Otomasyonu işini durdurur|
|/automationAccounts/jobs/streams/read|Bir Azure Otomasyonu iş akışını alır|
|/automationAccounts/jobs/streams/read|Bir Azure Otomasyonu iş akışını alır|
|/automationAccounts/jobs/suspend/action|Bir Azure Otomasyonu işini askıya alır|
|/automationAccounts/jobs/suspend/action|Bir Azure Otomasyonu işini askıya alır|
|/automationAccounts/jobs/write|Bir Azure Otomasyonu işi oluşturur|
|/automationAccounts/jobs/write|Bir Azure Otomasyonu işi oluşturur|
|/automationAccounts/jobSchedules/delete|Bir Azure Otomasyonu iş zamanlaması silme|
|/automationAccounts/jobSchedules/read|Bir Azure Otomasyonu iş zamanlaması alır|
|/automationAccounts/jobSchedules/write|Bir Azure Otomasyonu iş zamanlaması oluşturur|
|/automationAccounts/linkedWorkspace/read|Otomasyon hesabı bağlı çalışma alır|
|/automationAccounts/logDefinitions/read|Otomasyon hesabının kullanılabilir günlüklerini alır|
|/automationAccounts/modules/activities/read|Azure Otomasyon etkinliklerini alır|
|/automationAccounts/modules/delete|Bir Azure Otomasyonu modülünü siler|
|/automationAccounts/modules/read|Bir Azure Otomasyonu modülünü alır|
|/automationAccounts/modules/write|Oluşturur veya bir Azure Otomasyonu modülü güncelleştirir|
|/automationAccounts/nodeConfigurations/delete|Bir Azure Otomasyonu DSC'SİNİN düğüm yapılandırması siler|
|/automationAccounts/nodeConfigurations/read|Bir Azure Otomasyonu DSC'SİNİN düğüm yapılandırması okur|
|/automationAccounts/nodeConfigurations/readContent/action|Bir Azure Otomasyonu DSC'SİNİN düğüm yapılandırması içeriğini okur|
|/automationAccounts/nodeConfigurations/write|Bir Azure Otomasyonu DSC'SİNİN düğüm yapılandırması Yazar|
|/automationAccounts/nodes/delete|Azure Otomasyonu DSC düğümleri siler|
|/automationAccounts/nodes/read|Azure Otomasyonu DSC düğümleri okur|
|/automationAccounts/nodes/reports/read|Azure Otomasyonu DSC rapor contentss okur|
|/automationAccounts/nodes/reports/read|Azure Otomasyonu DSC raporları okur|
|/automationAccounts/objectDataTypes/fields/read|Azure Otomasyonu TypeFields alır|
|/automationAccounts/providers/Microsoft.Insights/metricDefinitions/read|Otomasyon ölçüm tanımlarını alır|
|/automationAccounts/read|Bir Azure Otomasyonu hesabını alır|
|/automationAccounts/runbooks/delete|Bir Azure Otomasyonu runbook'unu siler|
|/automationAccounts/runbooks/draft/publish/action|Bir Azure Otomasyonu runbook taslağı yayımlar|
|/automationAccounts/runbooks/draft/read|Bir Azure Otomasyonu runbook taslağı alır|
|/automationAccounts/runbooks/draft/readContent/action|Bir Azure Otomasyonu runbook taslağının içeriğini alır|
|/automationAccounts/runbooks/draft/testJob/read|Bir Azure Otomasyonu runbook taslağı test işini alır|
|/automationAccounts/runbooks/draft/testJob/resume/action|Bir Azure Otomasyonu runbook taslağı test işini sürdürür|
|/automationAccounts/runbooks/draft/testJob/stop/action|Bir Azure Otomasyonu runbook taslağı test işini durdurur|
|/automationAccounts/runbooks/draft/testJob/suspend/action|Bir Azure Otomasyonu runbook taslağı test işini askıya alır|
|/automationAccounts/runbooks/draft/testJob/write|Bir Azure Otomasyonu runbook taslağı test işi oluşturur|
|/automationAccounts/runbooks/draft/undoEdit/action|Bir Azure Otomasyonu runbook taslağı düzenlemeleri geri alma|
|/automationAccounts/runbooks/draft/writeContent/action|Bir Azure Otomasyonu runbook taslağının içeriğini oluşturur|
|/automationAccounts/runbooks/read|Bir Azure Otomasyonu runbook'unu alır|
|/automationAccounts/runbooks/readContent/action|Bir Azure Otomasyonu runbook içeriğini alır|
|/automationAccounts/runbooks/write|Oluşturur veya bir Azure Otomasyonu runbook'u güncelleştirir|
|/automationAccounts/schedules/delete|Bir Azure Otomasyonu zamanlama varlığını siler|
|/automationAccounts/schedules/read|Bir Azure Otomasyonu zamanlama varlığını alır|
|/automationAccounts/schedules/write|Bir Azure Otomasyonu zamanlama varlığı oluşturur veya güncelleştirir|
|/automationAccounts/statistics/read|Azure Otomasyonu istatistiklerini alır|
|/automationAccounts/usages/read|Azure Otomasyonu kullanım alır|
|/automationAccounts/variables/delete|Bir Azure Otomasyonu değişken varlığını siler|
|/automationAccounts/variables/read|Bir Azure Otomasyonu değişken varlığını okur|
|/automationAccounts/variables/write|Bir Azure Otomasyonu değişken varlığı oluşturur veya güncelleştirir|
|/automationAccounts/watchers/streams/read|Bir Azure Otomasyonu İzleyicisi iş akışını alır|
|/automationAccounts/webhooks/delete|Bir Azure Otomasyonu Web kancasını siler |
|/automationAccounts/webhooks/generateUri/action|Bir Azure Otomasyonu Web kancası için URI oluşturur|
|/automationAccounts/webhooks/read|Bir Azure Otomasyonu Web kancasını okur|
|/automationAccounts/webhooks/write|Oluşturur veya bir Azure Otomasyonu Web kancası güncelleştirir|
|/automationAccounts/write|Bir Azure Otomasyonu hesabı oluşturur veya güncelleştirir|
|/automationAccounts/write|Bir Azure Otomasyonu hesabı oluşturur veya güncelleştirir|

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

| İşlem | Açıklama |
|---|---|
|/b2cDirectories/DELETE|B2C Dizini kaynağını sil|
|/b2cDirectories/Read|B2C Dizini kaynağını görüntüle|
|/b2cDirectories/write|B2C Dizini kaynağı oluştur veya güncelleştir|
|/Operations/Read|Microsoft.AzureActiveDirectory kaynak sağlayıcısı için tüm kullanılabilir API işlemlerini okuyun|
|/register/action|Microsoft.AzureActiveDirectory kaynak sağlayıcısı için aboneliği kaydedin|

## <a name="microsoftazurestack"></a>Microsoft.AzureStack

| İşlem | Açıklama |
|---|---|
|/ Operations/okuma|Bir kaynak sağlayıcısı işlemi özelliklerini alır|
|/register/action|Abonelik Microsoft.AzureStack kaynak sağlayıcısına kaydeder|
|/Registrations/customerSubscriptions/DELETE|Bir Azure yığın müşteri aboneliğe siler|
|/registrations/customerSubscriptions/read|Bir Azure yığın müşteri aboneliğin özelliklerini alır|
|/Registrations/customerSubscriptions/Write|Oluşturur veya bir Azure yığın müşteri aboneliğe güncelleştirir|
|/Registrations/DELETE|Bir Azure yığın kaydını siler|
|/registrations/getActivationKey/action|En son Azure yığın etkinleştirme anahtarı alır|
|/registrations/products/listDetails/action|Bir Azure yığın Market ürün ayrıntılarını genişletilmiş alır|
|/Registrations/Products/Read|Bir Azure yığın Market ürün özelliklerini alır|
|/Registrations/Read|Bir Azure yığın kaydı özelliklerini alır|
|/ kayıtlar/yazma|Oluşturur veya bir Azure yığın kaydı güncelleştirir|

## <a name="microsoftbatch"></a>Microsoft.Batch

| İşlem | Açıklama |
|---|---|
|/batchAccounts/Applications/DELETE|Bir uygulamayı siler|
|/batchAccounts/Applications/Read|Uygulamaları listeler veya bir uygulamanın özelliklerini alır|
|/batchAccounts/Applications/Versions/Activate/Action|Bir uygulama paketi etkinleştirir|
|/batchAccounts/Applications/Versions/DELETE|Bir uygulama paketini siler|
|/batchAccounts/Applications/Versions/Read|Bir uygulama paketi özelliklerini alır|
|/batchAccounts/Applications/Versions/Write|Yeni bir uygulama paketi oluşturur veya var olan uygulama paketini güncelleştirir|
|/batchAccounts/Applications/Write|Yeni bir uygulama oluşturur veya varolan bir uygulamayı güncelleştirir|
|/batchAccounts/certificateOperationResults/read|Bir Batch hesabında uzun süre çalışan bir işlem sonuçlarını alır|
|/batchAccounts/Certificates/cancelDelete/Action|Silme işlemi başarısız bir Batch hesabında bir sertifika iptal eder|
|/batchAccounts/Certificates/DELETE|Sertifika toplu işlem hesabından siler|
|/batchAccounts/Certificates/Read|Bir Batch hesabında sertifikaları listeleyen veya bir sertifikanın özelliklerini alır|
|/batchAccounts/Certificates/Write|Bir Batch hesabında yeni bir sertifika oluşturur veya varolan bir sertifikayı güncelleştirir|
|/batchAccounts/DELETE|Batch hesabını siler|
|/batchAccounts/listkeys/Action|Bir toplu işlem hesabı için anahtarlar erişim listeleri|
|/batchAccounts/operationResults/Read|Uzun süren bir Batch hesabı İşlem sonuçlarını alır|
|/batchAccounts/poolOperationResults/read|Bir Batch hesabında uzun süren bir havuz İşlem sonuçlarını alır|
|/batchAccounts/Pools/DELETE|Bir toplu işlem hesabından bir havuzu siler|
|/batchAccounts/Pools/disableAutoscale/Action|Bir toplu işlem hesabı havuzu için otomatik ölçeklendirme devre dışı bırakır|
|/batchAccounts/Pools/Read|Bir toplu işlem hesabı havuzlarını listeler veya bir havuz özelliklerini alır|
|/batchAccounts/Pools/stopResize/Action|Bir Batch hesabı havuzu işlemi durdurur devam eden bir yeniden boyutlandırma|
|/batchAccounts/pools/upgradeOs/action|Bir toplu işlem hesabı havuzunun işletim sistemi yükseltmeleri|
|/batchAccounts/Pools/Write|Bir Batch hesabında yeni bir havuz oluşturur veya mevcut bir havuzu güncelleştirir|
|/batchAccounts/providers/Microsoft.Insights/diagnosticSettings/Read|Kaynak için tanılama ayarını alır|
|/batchAccounts/providers/Microsoft.Insights/diagnosticSettings/write|Kaynak için tanılama ayarını oluşturur veya güncelleştirir|
|/batchAccounts/providers/Microsoft.Insights/logDefinitions/read|Batch hizmeti için kullanılabilir günlüklerini alır|
|/batchAccounts/providers/Microsoft.Insights/metricDefinitions/Read|Batch hizmeti için kullanılabilir ölçümleri alır|
|/batchAccounts/Read|Bir toplu işlem hesabının özelliklerini alır veya toplu işlem hesaplarını listeler|
|/batchAccounts/regeneratekeys/action|Erişim anahtarları bir toplu işlem hesabı için yeniden oluşturur|
|/batchAccounts/syncAutoStorageKeys/action|Toplu işlem hesabı için yapılandırılmış otomatik depolama hesabının erişim anahtarlarını eşitler|
|/ batchAccounts/yazma|Yeni bir Batch hesabı oluşturur veya var olan bir toplu işlem hesabını güncelleştirir|
|/locations/checkNameAvailability/action|Hesap adı geçerli ve kullanımda olup olmadığını denetler.|
|/locations/quotas/read|Belirtilen Azure bölge belirtilen abonelik toplu kotaları alır|
|/register/action|Batch kaynak sağlayıcısı için aboneliği kaydeder ve Batch hesaplarının oluşturulmasını etkinleştirir|
|/unregister/action|Abonelik toplu işlem hesaplarını oluşturulmasını önleme toplu kaynak sağlayıcısı için kaydını siler|

## <a name="microsoftbatchai"></a>Microsoft.BatchAI

| İşlem | Açıklama |
|---|---|
|/Clusters/DELETE|Bir toplu AI küme siler|
|/Clusters/Read|Toplu AI kümeleri listeler veya toplu AI küme özelliklerini alır|
|/clusters/remoteLoginInformation/action|Bir toplu AI küme uzaktan oturum açma bilgilerini listeler|
|/ kümeleri/yazma|Yeni bir toplu AI kümesi oluşturur veya var olan toplu AI kümeyi güncelleştirir|
|/fileservers/delete|Bir toplu AI DosyaSunucusu siler|
|/fileservers/Read|Toplu AI fileservers listeler veya toplu AI DosyaSunucusu özelliklerini alır|
|/fileservers/resume/action|Bir toplu AI DosyaSunucusu sürdürür|
|/fileservers/suspend/Action|Bir toplu AI DosyaSunucusu askıya alır|
|/fileservers/write|Yeni bir toplu AI DosyaSunucusu oluşturur veya mevcut bir toplu AI DosyaSunucusu güncelleştirir|
|/Jobs/DELETE|Bir toplu AI işi siler|
|/Jobs/Read|Bir toplu AI iş özelliklerini alır veya toplu AI işlerini listeler|
|/jobs/remoteLoginInformation/action|Bir toplu AI işi uzaktan oturum açma bilgilerini listeler|
|/jobs/terminate/action|Bir toplu AI işi sonlandırır|
|/ işleri/yazma|Yeni bir toplu AI işi oluşturur veya mevcut bir toplu AI proje güncelleştirir|
|/register/action|Toplu AI kaynak sağlayıcısı için aboneliği kaydeder ve toplu AI kaynakların oluşturulmasını sağlar|

## <a name="microsoftbilling"></a>Microsoft.Billing

| İşlem | Açıklama |
|---|---|
|/billingPeriods/Read|Kullanılabilir fatura aralıklarını listeler|
|/invoices/Read|Listeleri kullanılabilir faturalar|

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps

| İşlem | Açıklama |
|---|---|
|/mapApis/Delete|İşlemi Siler|
|/mapApis/listSecrets/action|Parolaları Listeler|
|/mapApis/listSingleSignOnToken/action|Kaynak İçin Çoklu Oturum Açma Yetkilendirme Belirtecini Okuyun|
|/mapApis/Read|İşlemi Okur|
|/mapApis/regenerateKey/action|Anahtarı Yeniden Oluşturur|
|/mapApis/Write|İşlemi Yazar|
|/ Operations/okuma|İşlem açıklaması.|

## <a name="microsoftcache"></a>Microsoft.Cache

| İşlem | Açıklama |
|---|---|
|/checknameavailability/action|Bir adın yeni bir Redis Cache için kullanılabilir olup olmadığını denetler|
|/Operations/Read|'Microsoft.Cache' sağlayıcının desteklediği işlemleri listeler.|
|/redis/DELETE|Redis Cache'nin tamamını sil|
|/redis/export/action|Redis verilerini belirtilen biçimde ön ekli depolama blob'larına aktar|
|/redis/firewallRules/delete|Bir Redis Cache'in IP güvenlik duvarı kurallarını siler|
|/redis/firewallRules/read|Bir Redis Cache'in IP güvenlik duvarı kurallarını alır|
|/redis/firewallRules/write|Bir Redis Cache'in IP güvenlik duvarı kurallarını düzenler|
|/redis/forceReboot/action|Veri kaybı olasılığı olan bir önbellek örneği yeniden başlatmayı zorlayın.|
|/redis/import/action|Birden çok blob'dan belirli bir biçimdeki verileri Redis'e aktar|
|/redis/linkedservers/delete|Redis Cache'ten Bağlı Sunucuyu Sil|
|/redis/linkedservers/read|Bir Redis Cache ile ilişkili Bağlı Sunucuları alın.|
|/redis/linkedservers/write|Redis Cache'e Bağlı Sunucu Ekle|
|/redis/listKeys/action|Redis Cache erişim anahtarlarının değerini yönetim portalında görüntüleyin|
|/redis/listUpgradeNotifications/read|Önbellek kiracısı için en son Yükseltme Bildirimlerini listeleyin.|
|/redis/Locations/operationresults/Read|Bir uzun sonucunu çalışan işlemi için 'Konum' üst bilgisi önceden istemciye döndürülen alır|
|/redis/metricDefinitions/Read|Bir Redis Cache için kullanılabilir ölçümleri alır|
|/redis/patchSchedules/delete|Bir Redis Cache'in düzeltme eki zamanlamasını siler|
|/redis/patchSchedules/read|Redis Cache'in düzeltme eki uygulama zamanlamasını alır|
|/redis/patchSchedules/write|Redis Cache'in düzeltme eki uygulama zamanlamasını değiştirir|
|/redis/Read|Redis Cache'nin ayarlarını ve yapılandırmasını yönetim portalında görüntüleyin|
|/redis/regenerateKey/action|Redis Cache erişim anahtarlarının değerini yönetim portalında değiştirin|
|/redis/Start/Action|Bir önbellek örneği başlatın.|
|/redis/Stop/Action|Bir önbellek örneğini durdurun.|
|/ redis/yazma|Redis Cache'in ayarlarını ve yapılandırmasını yönetim portalında değiştirin|
|/register/action|'Microsoft.Cache' kaynak sağlayıcısını bir aboneliğe kaydeder|
|/unregister/action|'Microsoft.Cache' kaynak sağlayıcısının kaydını bir abonelikten kaldırır|

## <a name="microsoftcapacity"></a>Microsoft.Capacity

| İşlem | Açıklama |
|---|---|
|/register/action|Kapasite kaynağı sağlayıcısını kaydeder ve kapasite kaynakların oluşturulmasını sağlar.|
|/reservationorders/action|Tüm Rezervasyonu Güncelleştir|
|/reservationorders/delete|Tüm ayırmayı silmek|
|/reservationorders/read|Tüm ayırmaları okuma|
|/reservationorders/reservations/action|Tüm Rezervasyonu Güncelleştir|
|/reservationorders/reservations/delete|Tüm ayırmayı silmek|
|/reservationorders/reservations/read|Tüm ayırmaları okuma|
|/reservationorders/reservations/revisions/read|Tüm ayırmaları okuma|
|/reservationorders/reservations/write|Tüm ayırma oluştur|
|/reservationorders/write|Tüm ayırma oluştur|

## <a name="microsoftcdn"></a>Microsoft.Cdn

| İşlem | Açıklama |
|---|---|
|/CheckNameAvailability/action||
|/CheckResourceUsage/action||
|/edgenodes/delete||
|/edgenodes/read||
|/edgenodes/write||
|/operationresults/delete||
|/operationresults/profileresults/CheckResourceUsage/action||
|/operationresults/profileresults/delete||
|/operationresults/profileresults/endpointresults/CheckResourceUsage/action||
|/operationresults/profileresults/endpointresults/customdomainresults/delete||
|/operationresults/profileresults/endpointresults/customdomainresults/ DisableCustomHttps/action||
|/operationresults/profileresults/endpointresults/customdomainresults/ EnableCustomHttps/action||
|/operationresults/profileresults/endpointresults/customdomainresults/read||
|/operationresults/profileresults/endpointresults/customdomainresults/write||
|/operationresults/profileresults/endpointresults/delete||
|/operationresults/profileresults/endpointresults/Load/action||
|/operationresults/profileresults/endpointresults/originresults/delete||
|/operationresults/profileresults/endpointresults/originresults/read||
|/operationresults/profileresults/endpointresults/originresults/write||
|/operationresults/profileresults/endpointresults/Purge/action||
|/operationresults/profileresults/endpointresults/read||
|/operationresults/profileresults/endpointresults/Start/action||
|/operationresults/profileresults/endpointresults/Stop/action||
|/operationresults/profileresults/endpointresults/ValidateCustomDomain/action||
|/operationresults/profileresults/endpointresults/write||
|/operationresults/profileresults/GenerateSsoUri/action||
|/operationresults/profileresults/GetSupportedOptimizationTypes/action||
|/operationresults/profileresults/Read||
|/operationresults/profileresults/write||
|/operationresults/Read||
|/operationresults/write||
|/Operations/Read||
|/profiles/CheckResourceUsage/action||
|/Profiles/DELETE||
|/profiles/endpoints/CheckResourceUsage/action||
|/profiles/endpoints/customdomains/delete||
|/profiles/endpoints/customdomains/DisableCustomHttps/action||
|/profiles/endpoints/customdomains/EnableCustomHttps/action||
|/Profiles/endpoints/customdomains/Read||
|/profiles/endpoints/customdomains/write||
|/Profiles/endpoints/DELETE||
|/Profiles/endpoints/Load/Action||
|/profiles/endpoints/origins/delete||
|/Profiles/endpoints/origins/Read||
|/profiles/endpoints/origins/write||
|/profiles/endpoints/providers/Microsoft.Insights/diagnosticSettings/read|Kaynağın tanılama ayarlarını alır|
|/profiles/endpoints/providers/Microsoft.Insights/diagnosticSettings/write|Oluşturur veya kaynağın tanılama ayarlarını güncelleştirir|
|/profiles/endpoints/providers/Microsoft.Insights/logDefinitions/read|Microsoft.Cdn için kullanılabilir günlüklerini alır|
|/profiles/endpoints/Purge/action||
|/Profiles/endpoints/Read||
|/Profiles/endpoints/Start/Action||
|/Profiles/endpoints/Stop/Action||
|/profiles/endpoints/ValidateCustomDomain/action||
|/profiles/endpoints/write||
|/profiles/GenerateSsoUri/action||
|/profiles/GetSupportedOptimizationTypes/action||
|/Profiles/Read||
|/ profilleri/yazma||
|/register/action|CDN kaynak sağlayıcısı için aboneliği kaydeder ve CDN profillerinin oluşturulmasını sağlar.|
|/ValidateProbe/action||

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

| İşlem | Açıklama |
|---|---|
|/certificateOrders/certificates/Delete|Varolan bir sertifikayı Sil|
|/certificateOrders/certificates/Read|Sertifikaların listesini al|
|/certificateOrders/certificates/Write|Yeni bir sertifika ekleyin veya mevcut bir güncelleştirme|
|/certificateOrders/Delete|Varolan bir AppServiceCertificate Sil|
|/certificateOrders/operations/Read|Uygulama hizmeti sertifika kaydı tüm işlemler listesi|
|/certificateOrders/Read|CertificateOrders listesini al|
|/certificateOrders/reissue/Action|Varolan bir certificateorder yeniden gönderin|
|/certificateOrders/renew/Action|Varolan bir certificateorder yenileme|
|/certificateOrders/resendEmail/Action|Yeniden Gönder sertifika e-posta|
|/certificateOrders/resendRequestEmails/Action|Başka bir e-posta adresine e-postalar isteği yeniden gönderin|
|/certificateOrders/resendRequestEmails/Action|Site korumalı için verilen bir uygulama hizmet sertifikası alma|
|/certificateOrders/retrieveCertificateActions/Action|Sertifika eylemlerin listesini alma|
|/certificateOrders/retrieveEmailHistory/Action|Sertifika e-posta geçmişini alma|
|/certificateOrders/verifyDomainOwnership/Action|Etki alanı sahipliğini doğrulama|
|/certificateOrders/Write|Yeni bir certificateOrder eklemek veya mevcut bir güncelleştirme|
|/provisionGlobalAppServicePrincipalInUserTenant/Action|Hizmet uygulaması sorumlusu için hizmet asıl sağlama|
|/register/action|Aboneliği için Microsoft Certificates kaynak sağlayıcısı kaydetme|
|/validateCertificateRegistrationInformation/Action|Sertifika satın alma nesnesi gönderme olmadan doğrula|

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

| İşlem | Açıklama |
|---|---|
|/Capabilities/Read|Özellikleri gösterir|
|/checkDomainNameAvailability/action|Bir etki alanı adının kullanılabilirliğini denetler.|
|/domainNames/Active/Write|Etkin etki alanı adını ayarlar.|
|/domainNames/availabilitySets/read|Kaynak için kullanılabilirlik kümesini görüntüleyin.|
|/domainNames/Capabilities/Read|Etki alanı adı özelliklerini gösterir|
|/domainNames/delete|Kaynaklar için etki alanı adlarını kaldırın.|
|/domainNames/Extensions/DELETE|Etki alanı adı uzantılarını kaldırın.|
|/domainNames/extensions/operationStatuses/read|Etki alanı uzantıları için işlem durumunu okur.|
|/domainNames/Extensions/Read|Etki alanı adı uzantılarını döndürür.|
|/domainNames/Extensions/Write|Etki alanı adı uzantıları ekleyin.|
|/domainNames/internalLoadBalancers/delete|Yeni bir iç yük dengelemeyi kaldırın.|
|/domainNames/internalLoadBalancers/operationStatuses/read|Etki alanı adları iç load balancer'ları için işlem durumunu okur.|
|/domainNames/internalLoadBalancers/read|İç load balancer'ları alır.|
|/domainNames/internalLoadBalancers/write|Yeni bir iç yük dengeleme oluşturur.|
|/domainNames/loadBalancedEndpointSets/operationStatuses/read|Etki alanı adları yük dengeli uç nokta kümeleri için işlem durumunu okur.|
|/domainNames/loadBalancedEndpointSets/read|Yük dengeli uç nokta kümelerini gösterir|
|/domainNames/read|Kaynaklar için etki alanı adlarını döndürün.|
|/domainNames/serviceCertificates/delete|Kullanılan hizmet sertifikalarını silin.|
|/domainNames/serviceCertificates/operationStatuses/read|Etki alanı adları hizmet sertifikaları için işlem durumunu okur.|
|/domainNames/serviceCertificates/read|Kullanılan hizmet sertifikalarını döndürür.|
|/domainNames/serviceCertificates/write|Kullanılan hizmet sertifikalarını ekleyin veya değiştirin.|
|/domainNames/slots/DELETE|Bir dağıtım yuvasını siler.|
|/domainNames/slots/operationStatuses/Read|Etki alanı adı yuvaları için işlem durumunu okur.|
|/domainNames/slots/read|Dağıtım yuvalarını gösterir.|
|/domainNames/slots/roles/extensionReferences/delete|Dağıtım yuvası rolüne ait uzantı başvurusunu kaldırın.|
|/domainNames/slots/Roles/extensionReferences/operationStatuses/Read|Etki alanı adı yuvaları uzantı başvuruları için işlem durumunu okur.|
|/domainNames/slots/Roles/extensionReferences/Read|Dağıtım yuvası rolüne ait uzantı başvurusunu döndürür.|
|/domainNames/slots/Roles/extensionReferences/Write|Dağıtım yuvası rolüne ait uzantı başvurusunu ekleyin veya değiştirin.|
|/domainNames/slots/roles/providers/Microsoft.Insights/diagnosticSettings/read|Tanılama ayarlarını alın.|
|/domainNames/slots/roles/providers/Microsoft.Insights/diagnosticSettings/write|Tanılama ayarlarını ekleyin veya değiştirin.|
|/domainNames/slots/roles/providers/Microsoft.Insights/metricDefinitions/read|Ölçüm tanımlarını alır.|
|/domainNames/slots/roles/read|Dağıtım yuvası için rolü alın.|
|/domainNames/slots/roles/roleInstances/operationStatuses/read|Etki alanı adı yuvası rolleri rol örnekleri için işlem durumunu okur.|
|/domainNames/slots/roles/roleInstances/read|Rol örneğini alın.|
|/domainNames/slots/roles/roleInstances/rebuild/action|Rol örneğini yeniden derler.|
|/domainNames/slots/roles/roleInstances/reimage/action|Rol örneğinin yeniden görüntüsünü oluşturur.|
|/domainNames/slots/roles/roleInstances/restart/action|Rol örneklerini yeniden başlatır.|
|/domainNames/slots/Start/Action|Bir dağıtım yuvası başlatır.|
|/domainNames/slots/State/Start/Write|Dağıtım yuvası durumunu durduruldu olarak değiştirir.|
|/domainNames/slots/State/stop/Write|Dağıtım yuvası durumunu başlatıldı olarak değiştirir.|
|/domainNames/slots/Stop/Action|Dağıtım yuvasını askıya alır.|
|/domainNames/slots/upgradeDomain/write|Etki alanını yükseltir.|
|/domainNames/slots/write|Dağıtımı oluşturur veya güncelleştirir.|
|/domainNames/Swap/Action|Hazırlama yuvasını üretim yuvasıyla değiştirir.|
|/domainNames/write|Kaynaklar için etki alanı adlarını ekleyin veya değiştirin.|
|/moveSubscriptionResources/action|Tüm klasik kaynakları farklı bir aboneliğe taşıyın.|
|/operatingSystemFamilies/read|Microsoft Azure'da kullanılabilen konuk işletim sistemi ailelerini ve her aile için kullanılabilen işletim sistemi sürümlerini listeler.|
|/operatingSystems/read|Microsoft Azure'da kullanılabilen geçerli konuk işletim sistemi sürümlerini listeler.|
|/quotas/read|Abonelik için kotayı alın.|
|/register/action|Classic Compute'a Kaydol|
|/resourceTypes/skus/read|Desteklenen kaynak türleri için Sku listesini alır.|
|/validateSubscriptionMoveAvailability/action|Klasik taşıma işlemi için aboneliğin uygunluk durumunu doğrulayın.|
|/virtualMachines/associatedNetworkSecurityGroups/delete|Sanal makineyle ilişkilendirilmiş ağ güvenlik grubunu siler.|
|/virtualMachines/associatedNetworkSecurityGroups/operationStatuses/read|Ağ güvenlik gruplarıyla ilişkili sanal makineler için işlem durumunu okur.|
|/virtualMachines/associatedNetworkSecurityGroups/read|Sanal makineyle ilişkilendirilmiş ağ güvenlik grubunu alır.|
|/virtualMachines/associatedNetworkSecurityGroups/write|Sanal makineyle ilişkilendirilmiş bir ağ güvenlik grubu ekler.|
|/virtualMachines/asyncOperations/read|Olası zaman uyumsuz işlemleri alır|
|/virtualMachines/attachDisk/Action|Sanal makineye bir veri diski ekler.|
|/virtualMachines/DELETE|Sanal makineleri kaldırır.|
|/virtualMachines/detachDisk/Action|Bir veri diskini sanal makineden ayırır.|
|/virtualMachines/Disks/Read|Veri disklerinin listesini alır|
|/virtualMachines/downloadRemoteDesktopConnectionFile/action|Sanal makine için RDP dosyasını indirir.|
|/virtualMachines/Extensions/operationStatuses/Read|Sanal makine uzantıları için işlem durumunu okur.|
|/virtualMachines/Extensions/Read|Sanal makine uzantısını alır.|
|/virtualMachines/Extensions/Write|Sanal makine uzantısını yerleştirir.|
|/virtualMachines/Metrics/Read|Ölçümleri alır.|
|/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/delete|Ağ arabirimiyle ilişkilendirilmiş ağ güvenlik grubunu siler.|
|/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/ operationStatuses/read|Ağ güvenlik gruplarıyla ilişkili sanal makineler için işlem durumunu okur.|
|/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/read|Ağ arabirimiyle ilişkilendirilmiş ağ güvenlik grubunu alır.|
|/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/write|Ağ arabirimiyle ilişkilendirilmiş bir ağ güvenlik grubunu ekler.|
|/virtualMachines/operationStatuses/Read|Sanal makineler için işlem durumunu okur.|
|/virtualMachines/performMaintenance/action|Sanal makinede bakım işlemi gerçekleştirir.|
|/virtualMachines/providers/Microsoft.Insights/diagnosticSettings/Read|Tanılama ayarlarını alın.|
|/virtualMachines/providers/Microsoft.Insights/diagnosticSettings/write|Tanılama ayarlarını ekleyin veya değiştirin.|
|/virtualMachines/providers/Microsoft.Insights/metricDefinitions/read|Ölçüm tanımlarını alır.|
|/virtualMachines/Read|Sanal makinelerin listesini alır.|
|/virtualMachines/redeploy/action|Sanal makineyi yeniden dağıtır.|
|/virtualMachines/restart/Action|Sanal makineleri yeniden başlatır.|
|/virtualMachines/Shutdown/Action|Sanal makineyi kapatın.|
|/virtualMachines/Start/Action|Sanal makineyi başlatın.|
|/virtualMachines/Stop/Action|Sanal makineyi durdurur.|
|/ virtualMachines/yazma|Sanal makineleri ekleyin veya değiştirin.|

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

| İşlem | Açıklama |
|---|---|
|/gatewaySupportedDevices/read|Desteklenen cihazların listesini alır.|
|/networkSecurityGroups/delete|Ağ güvenlik grubunu siler.|
|/networkSecurityGroups/operationStatuses/read|Ağ güvenlik grubu için işlem durumunu okur.|
|/networkSecurityGroups/read|Ağ güvenlik grubunu alır.|
|/networkSecurityGroups/securityRules/delete|Güvenlik kuralını siler.|
|/networkSecurityGroups/securityRules/operationStatuses/read|Aü güvenlik grubu için işlem durumunu okur.|
|/networkSecurityGroups/securityRules/read|Güvenlik kuralını alır.|
|/networkSecurityGroups/securityRules/write|Güvenlik kuralı alır veya güncelleştirir.|
|/networkSecurityGroups/write|Yeni bir ağ güvenlik grubu ekler.|
|/quotas/read|Abonelik için kotayı alın.|
|/register/action|Classic Network'e Kaydol|
|/reservedIps/delete|Ayrılmış bir IP'yi silin.|
|/reservedIps/join/action|Ayrılmış bir IP'ye katılın|
|/reservedIps/link/action|Ayrılmış bir IP'ye bağlantı oluşturun|
|/reservedIps/operationStatuses/read|Ayrılmış IP'ler için işlem durumunu okur|
|/reservedIps/read|Ayrılmış IP'leri alır|
|/reservedIps/write|Yeni bir ayrılmış IP ekleyin|
|/virtualNetworks/Capabilities/Read|Özellikleri gösterir|
|/virtualNetworks/checkIPAddressAvailability/action|Bir sanal ağdaki belirli bir IP adresinin kullanılabilirliğini denetler.|
|/virtualNetworks/delete|Sanal ağı siler.|
|/virtualNetworks/gateways/clientRevokedCertificates/delete|Bir istemci sertifikası iptalini geri alır.|
|/virtualNetworks/gateways/clientRevokedCertificates/read|İptal edilen istemci sertifikalarını okuyun.|
|/virtualNetworks/gateways/clientRevokedCertificates/write|Bir istemci sertifikasını iptal eder.|
|/virtualNetworks/gateways/clientRootCertificates/delete|Sanal ağın ağ geçidi istemci sertifikasını siler.|
|/virtualNetworks/gateways/clientRootCertificates/download/action|Parmak izine göre sertifika indirir.|
|/virtualNetworks/gateways/clientRootCertificates/listPackage/action|Sanal ağ geçidi sertifika paketini listeler.|
|/virtualNetworks/gateways/clientRootCertificates/read|İstemci kök sertifikalarını bulun.|
|/virtualNetworks/gateways/clientRootCertificates/write|Yeni bir istemci kök sertifikasını karşıya yükler.|
|/virtualNetworks/gateways/connections/connect/action|Bir siteden siteye ağ geçidi bağlantısına bağlanır.|
|/virtualNetworks/gateways/connections/disconnect/action|Bir siteden siteye ağ geçidi bağlantısını keser.|
|/virtualNetworks/gateways/connections/read|Bağlantıların listesini alır.|
|/virtualNetworks/gateways/connections/test/action|Bir siteden siteye ağ geçidi bağlantısını test eder.|
|/virtualNetworks/gateways/delete|Sanal ağ geçidini siler.|
|/virtualNetworks/gateways/downloadDeviceConfigurationScript/action|Cihaz yapılandırma betiğini indirir.|
|/virtualNetworks/gateways/downloadDiagnostics/action|Ağ geçidi tanılamasını indirir.|
|/virtualNetworks/gateways/listCircuitServiceKey/action|Bağlantı hattı hizmet anahtarını alır.|
|/virtualNetworks/gateways/listPackage/action|Sanal ağ geçidi paketini listeler.|
|/virtualNetworks/Gateways/operationStatuses/Read|Sanal ağ geçitleri için işlem durumunu okur.|
|/virtualNetworks/gateways/packages/read|Sanal ağ geçidi paketini alır.|
|/virtualNetworks/Gateways/Read|Sanal ağ geçitlerini alır.|
|/virtualNetworks/gateways/startDiagnostics/action|Sanal ağ geçidi için tanılamayı başlatır.|
|/virtualNetworks/gateways/stopDiagnostics/action|Sanal ağ geçidi için tanılamayı durdurur.|
|/virtualNetworks/gateways/write|Bir sanal ağ geçidi ekler.|
|/virtualNetworks/join/action|Sanal ağa katılır.|
|/virtualNetworks/operationStatuses/Read|Sanal ağlar için işlem durumunu okur.|
|/virtualNetworks/Peer/Action|Bir sanal ağı başka bir sanal ağ ile eşler.|
|/virtualNetworks/Read|Sanal ağı alın.|
|/virtualNetworks/subnets/associatedNetworkSecurityGroups/delete|Alt ağ ile ilişkilendirilmiş ağ güvenlik grubunu siler.|
|/virtualNetworks/subnets/associatedNetworkSecurityGroups/operationStatuses/read|Ağ güvenlik grubuyla ilişkili sanal ağ alt ağı için işlem durumunu okur.|
|/virtualNetworks/subnets/associatedNetworkSecurityGroups/read|Alt ağ ile ilişkilendirilmiş ağ güvenlik grubunu alır.|
|/virtualNetworks/subnets/associatedNetworkSecurityGroups/write|Alt ağ ile ilişkilendirilmiş bir ağ güvenlik grubu ekler.|
|/virtualNetworks/write|Yeni bir sanal ağ ekleyin.|

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

| İşlem | Açıklama |
|---|---|
|/Capabilities/Read|Özellikleri gösterir|
|/checkStorageAccountAvailability/action|Bir depolama hesabının kullanılabilirliğini denetler.|
|/Disks/Read|Depolama hesabı diskini döndürür.|
|/images/Read|Görüntüyü döndürür.|
|/osImages/read|İşletim sistemi görüntüsünü döndürür.|
|/publicImages/read|Ortak sanal makine görüntüsünü alır.|
|/quotas/read|Abonelik için kotayı alın.|
|/register/action|Klasik Depolamaya Kaydeder|
|/storageAccounts/delete|Depolama hesabını silin.|
|/storageAccounts/disks/delete|Verilen depolama hesabı diskini siler.|
|/storageAccounts/disks/operationStatuses/read|Kaynağın işlem durumunu okur.|
|/storageAccounts/disks/read|Depolama hesabı diskini döndürür.|
|/storageAccounts/disks/write|Depolama hesabı diski ekler.|
|/storageAccounts/images/delete|Belirtilen depolama hesabı görüntüsünü siler.|
|/storageAccounts/images/read|Depolama hesabı görüntüsünü döndürür.|
|/storageAccounts/listKeys/action|Depolama hesaplarının erişim anahtarlarını listeler.|
|/storageAccounts/operationStatuses/read|Kaynağın işlem durumunu okur.|
|/storageAccounts/osImages/delete|Belirtilen bir depolama hesabı işletim sistemi görüntüsünü siler.|
|/storageAccounts/osImages/read|Depolama hesabı işletim sistemi görüntüsünü döndürür.|
|/storageAccounts/read|Belirli bir hesaba yönelik depolama hesabını döndürün.|
|/storageAccounts/regenerateKey/action|Depolama hesabı için var olan erişim anahtarlarını yeniden oluşturur.|
|/storageAccounts/services/diagnosticSettings/read|Tanılama ayarlarını alın.|
|/storageAccounts/services/diagnosticSettings/write|Tanılama ayarlarını ekleyin veya değiştirin.|
|/storageAccounts/services/metricDefinitions/read|Ölçüm tanımlarını alır.|
|/storageAccounts/services/metrics/read|Ölçümleri alır.|
|/storageAccounts/services/read|Kullanılabilir hizmetleri alın.|
|/storageAccounts/write|Yeni bir depolama hesabı ekler.|

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

| İşlem | Açıklama |
|---|---|
|/Accounts/DELETE|API hesaplarını siler|
|/accounts/listKeys/action|Anahtarları Listele|
|/accounts/providers/Microsoft.Insights/diagnosticSettings/read|Kaynağın tanılama ayarını alır.|
|/accounts/providers/Microsoft.Insights/diagnosticSettings/write|Kaynağın tanılama ayarını oluşturur veya güncelleştirir.|
|/accounts/providers/Microsoft.Insights/metricDefinitions/read|Bilişsel Hizmetler için kullanılabilir ölçümleri alır.|
|/Accounts/Read|API hesaplarını okur.|
|/accounts/regenerateKey/action|Anahtarı Yeniden Oluştur|
|/Accounts/skus/Read|Mevcut kaynağa ait kullanılabilen SKU'ları okur.|
|/Accounts/usages/Read|Mevcut bir kaynak için kota kullanımını alın.|
|/ hesapları/yazma|API Hesaplarını yazar.|
|/locations/checkSkuAvailability/action|Bir abonelik için kullanılabilir SKU'ları okur.|
|/ Operations/okuma|Tüm kullanılabilir işlemleri listele|
|/register/action|Bilişsel hizmetler için aboneliği kaydeder|
|/skus/Read|Bilişsel hizmetler için kullanılabilir SKU'lar okur.|

## <a name="microsoftcommerce"></a>Microsoft.Commerce

| İşlem | Açıklama |
|---|---|
|/ RateCard/okuma|Veri, kaynak/ölçer meta verileri ve belirtilen abonelik için hızlarını döndürür sunar.|
|/UsageAggregates/read|Microsoft Azure'nın tüketim abonelik tarafından alır. Sonuç toplamalar içeren kullanım verileri, abonelik ve kaynak ilgili bilgi için belirli bir zaman aralığı.|

## <a name="microsoftcompute"></a>Microsoft.Compute

| İşlem | Açıklama |
|---|---|
|/availabilitySets/delete|Kullanılabilirlik kümesini siler|
|/availabilitySets/Read|Bir kullanılabilirlik kümesinin özelliklerini al|
|/availabilitySets/vmSizes/read|Kullanılabilirlik kümesinde bir sanal makine oluşturmak veya güncelleştirmek için kullanılabilir boyutları listele|
|/availabilitySets/write|Yeni bir kullanılabilirlik kümesi oluşturur veya mevcut kümeyi güncelleştirir|
|/disks/beginGetAccess/action|Blob erişimi için Diskin SAS URI'sini alır|
|/disks/delete|Diski siler|
|/disks/endGetAccess/action|Diskin SAS URI'sini iptal eder|
|/Disks/Read|Diskin özelliklerini alır|
|/disks/write|Yeni bir Disk oluşturur veya mevcut Diski güncelleştirir|
|/images/DELETE|Görüntüyü siler|
|/images/Read|Görüntü özelliklerini alır|
|/ görüntüleri/yazma|Yeni bir Görüntü oluşturur veya mevcut bir Görüntüyü güncelleştirir|
|/Locations/capsOperations/Read|Zaman uyumsuz bir büyük harf işlem durumunu alır|
|/locations/diskOperations/read|Zaman uyumsuz bir Disk işlem durumunu alır|
|/Locations/Operations/Read|Bir zaman uyumsuz işlemin durumunu alır|
|/locations/publishers/artifacttypes/offers/read|Platform görüntü teklif özelliklerini alır|
|/locations/publishers/artifacttypes/offers/skus/read|Platform görüntü Sku özelliklerini alır|
|/locations/publishers/artifacttypes/offers/skus/versions/read|Platform görüntü sürümü özelliklerini alır|
|/locations/publishers/artifacttypes/types/read|VMExtension türünün özelliklerini al|
|/locations/publishers/artifacttypes/types/versions/read|VMExtension sürüm özelliklerini alır|
|/Locations/Publishers/Read|Bir yayımcı özelliklerini alır|
|/locations/runCommands/read|Konumda kullanılabilir çalıştırma komutlarını listeler|
|/Locations/usages/Read|Aboneliğin bir konumdaki işlem kaynaklarında bulunan hizmet sınırlarını ve geçerli kullanım miktarlarını alır|
|/locations/vmSizes/read|Bir konumdaki kullanılabilir sanal makine boyutlarını listeler|
|/Operations/Read|Microsoft.Compute kaynak sağlayıcısındaki kullanılabilir işlemleri listele|
|/register/action|Aboneliği Microsoft.Compute kaynak sağlayıcısına kaydeder|
|/restorePointCollections/delete|Geri yükleme noktası koleksiyonu ve bağımsız geri yükleme noktalarını siler|
|/restorePointCollections/read|Geri yükleme noktası koleksiyonunun özelliklerini alır|
|/restorePointCollections/restorePoints/delete|Geri yükleme noktasını siler|
|/restorePointCollections/restorePoints/read|Geri yükleme noktasının özelliklerini alır|
|/restorePointCollections/restorePoints/retrieveSasUris/action|Blob SAS URI'leriyle birlikte geri yükleme noktasının özelliklerini alır|
|/restorePointCollections/restorePoints/write|Yeni bir geri yükleme noktası oluşturur|
|/restorePointCollections/write|Yeni bir geri yükleme noktası koleksiyonu oluşturur veya mevcut olanı güncelleştirir|
|/sharedVMImages/delete|SharedVMImage siler|
|/sharedVMImages/read|Bir SharedVMImage özelliklerini alır|
|/sharedVMImages/versions/delete|Bir SharedVMImageVersion Sil|
|/sharedVMImages/versions/read|Bir SharedVMImageVersion özelliklerini alır|
|/sharedVMImages/versions/replicate/action|Hedef bölgeler için bir SharedVMImageVersion Çoğalt|
|/sharedVMImages/versions/write|Yeni bir SharedVMImageVersion oluşturun veya var olan bir güncelleştirme|
|/sharedVMImages/write|Yeni bir SharedVMImage oluşturur veya mevcut kümeyi güncelleştirir|
|/skus/Read|Aboneliğiniz için kullanılabilir Microsoft.Compute SKU listesini alır|
|/snapshots/beginGetAccess/action|Blob erişimi için anlık görüntü SAS URI'sini Al|
|/snapshots/delete|Anlık Görüntüyü siler|
|/snapshots/endGetAccess/action|Anlık görüntü SAS URI'sini iptal et|
|/snapshots/read|Anlık Görüntünün özelliklerin alır|
|/snapshots/write|Yeni bir Anlık Görüntü oluşturur veya mevcut Anlık Görüntüyü güncelleştirir|
|/virtualMachines/Capture/Action|Sanal sabit diskleri kopyalayarak sanal makineyi yakalar ve benzer sanal makineler oluşturmak için kullanılabilecek bir şablon oluşturur|
|/virtualMachines/convertToManagedDisks/action|Sanal makinenin blob tabanlı disklerini yönetilen disklere dönüştürür|
|/virtualMachines/deallocate/Action|Sanal makineyi kapatır ve işlem kaynaklarını serbest bırakır|
|/virtualMachines/DELETE|Sanal makineyi siler|
|/virtualMachines/extensions/delete|Sanal makine uzantısını siler|
|/virtualMachines/Extensions/Read|Bir sanal makine uzantısının özelliklerini alır|
|/virtualMachines/Extensions/Write|Yeni bir sanal makine uzantısı oluşturur veya mevcut uzantıyı güncelleştirir|
|/virtualMachines/generalize/Action|Sanal makine durumunu Genelleştirmiş olarak ayarlar ve sanal makineyi yakalama için hazırlar|
|/virtualMachines/instanceView/Read|Sanal makine ve kaynaklarının ayrıntılı çalışma zamanı durumunu alır|
|/virtualMachines/performMaintenance/action|VM'de Bakım İşlemini gerçekleştirir.|
|/virtualMachines/powerOff/Action|Sanal makineyi kapatır. Sanal makinenin faturalandırılmaya devam edecek unutmayın.|
|/virtualMachines/providers/Microsoft.Insights/metricDefinitions/read|Sanal Makine Ölçüm Tanımlarını Okur|
|/virtualMachines/Read|Bir sanal makinenin özelliklerini alır|
|/virtualMachines/redeploy/action|Sanal makineyi yeniden dağıtır|
|/virtualMachines/reimage/action|Fark kayıt diski kullanarak sanal makine görüntüsünü yeniden oluşturur.|
|/virtualMachines/restart/Action|Sanal makineyi yeniden başlatır|
|/virtualMachines/runCommand/action|Sanal makinede önceden tanımlanmış bir betik yürütür|
|/virtualMachines/Start/Action|Sanal makineyi başlatır|
|/virtualMachines/vmSizes/read|Sanal makineyi güncelleştirmek için kullanılabilir boyutları listeler|
|/ virtualMachines/yazma|Yeni bir sanal makine oluşturur veya mevcut sanal makineyi güncelleştirir|
|/virtualMachineScaleSets/deallocate/Action|Sanal makineyi kapatır ve Sanal Makine Ölçek Kümesi örneklerinin işlem kaynaklarını serbest bırakır |
|/virtualMachineScaleSets/delete|Sanal Makine Ölçek Kümesini siler|
|/virtualMachineScaleSets/delete/action|Sanal Makine Ölçek Kümesinin örneklerini siler|
|/virtualMachineScaleSets/extensions/delete|Sanal Makine Ölçek Kümesi Uzantısını siler|
|/virtualMachineScaleSets/extensions/read|Sanal Makine Ölçek Kümesi Uzantısının özelliklerini alır|
|/virtualMachineScaleSets/extensions/write|Yeni bir Sanal Makine Ölçek Kümesi Uzantısı oluşturur veya mevcut olanı güncelleştirir|
|/virtualMachineScaleSets/forceRecoveryServiceFabricPlatformUpdateDomainWalk/action|El ile bir service fabric takıldı bekleyen bir güncelleştirmeyi tamamlamak için sanal makine ölçek kümesi platform güncelleştirmesi etki alanları yol|
|/virtualMachineScaleSets/instanceView/read|Sanal Makine Ölçek Kümesinin örnek görünümünü alır|
|/virtualMachineScaleSets/manualUpgrade/action|Örnekleri, Sanal Makine Ölçek Kümesinin son modeline el ile güncelleştirir|
|/virtualMachineScaleSets/networkInterfaces/read|Tüm ağ arabirimleri, sanal makine ölçek kümesinin özelliklerini al|
|/virtualMachineScaleSets/osUpgradeHistory/read|Bir sanal makine ölçek kümesi için işletim sistemi yükseltmeleri geçmişini alır|
|/virtualMachineScaleSets/performMaintenance/action|Sanal makine ölçek kümesi örneklerinde planlanmış bakım gerçekleştirir|
|/virtualMachineScaleSets/powerOff/Action|Sanal Makine Ölçek Kümesinin örneklerini kapatır|
|/virtualMachineScaleSets/providers/Microsoft.Insights/metricDefinitions/Read|Sanal Makine Ölçek Kümesi Ölçüm Tanımlarını Okur|
|/virtualMachineScaleSets/publicIPAddresses/read|Tüm genel IP adresleri sanal makine ölçek kümesinin özelliklerini al|
|/virtualMachineScaleSets/read|Bir Sanal Makine Ölçek Kümesinin özelliklerini alın|
|/virtualMachineScaleSets/redeploy/action|Sanal makine ölçek kümesinin örneklerini yeniden dağıtın|
|/virtualMachineScaleSets/reimage/action|Sanal Makine Ölçek Kümesi örneklerinin görüntülerini yeniden oluşturur|
|/virtualMachineScaleSets/restart/action|Sanal Makine Ölçek Kümesinin örneklerini yeniden başlatır|
|/virtualMachineScaleSets/rollingUpgrades/cancel/action|Sanal Makine Ölçek Kümesinin sıralı yükseltmesini iptal eder|
|/virtualMachineScaleSets/rollingUpgrades/read|Bir Sanal Makine Ölçek Kümesi için son Sıralı Yükseltme durumunu alın|
|/virtualMachineScaleSets/Scale/Action|Mevcut bir Sanal Makine Ölçek Kümesinin belirtilen örnek sayısında Ölçeği Daraltma/Ölçeği Genişletme işlemlerini gerçekleştirebildiğini doğrulayın|
|/virtualMachineScaleSets/skus/Read|Mevcut bir Sanal Makine Ölçek Kümesi için geçerli SKU'ları listeler|
|/virtualMachineScaleSets/Start/Action|Sanal Makine Ölçek Kümesinin örneklerini başlatır|
|/virtualMachineScaleSets/virtualMachines/deallocate/action|VM Ölçek Kümesindeki bir Sanal Makineyi kapatır ve işlem kaynaklarını serbest bırakır.|
|/virtualMachineScaleSets/virtualMachines/delete|VM Ölçek Kümesindeki belirli bir Sanal Makineyi silin.|
|/virtualMachineScaleSets/virtualMachines/instanceView/read|VM Ölçek Kümesindeki bir Sanal Makinenin örnek görünümünü alır.|
|/virtualMachineScaleSets/virtualMachines/networkInterfaces/ ipConfigurations/publicIPAddresses/read|Sanal makine ölçek kümesi kullanılarak oluşturulan genel IP adresi özelliklerini alır. Sanal makine ölçek kümesi en fazla ipconfiguration (özel IP) başına bir genel IP oluşturun|
|/virtualMachineScaleSets/virtualMachines/networkInterfaces/ipConfigurations/read|Bir veya tüm IP yapılandırmasında sanal makine ölçek kümesi kullanılarak oluşturulan ağ arabirimi özelliklerini alır. Özel IP IP yapılandırmaları temsil eder|
|/virtualMachineScaleSets/virtualMachines/networkInterfaces/read|Bir ya da tüm ağ arabirimleri, sanal makine ölçek kümesi kullanılarak oluşturulan bir sanal makinenin özelliklerini alır|
|/virtualMachineScaleSets/virtualMachines/performMaintenance/action|Sanal makine ölçek kümesindeki bir sanal makine örneğini planlanmış bakım gerçekleştirir.|
|/virtualMachineScaleSets/virtualMachines/powerOff/action|VM Ölçek Kümesindeki bir Sanal Makine örneğini kapatır.|
|/ virtualMachineScaleSets/virtualMachines/sağlayıcıları/Microsoft.Insights/metricDefinitions/read|Ölçek Kümesindeki Sanal Makine Ölçüm Tanımlarını Okur|
|/virtualMachineScaleSets/virtualMachines/read|VM Ölçek Kümesindeki bir Sanal Makinenin özelliklerini alır|
|/virtualMachineScaleSets/virtualMachines/redeploy/action|Sanal makine ölçek kümesindeki bir sanal makine örneğini yeniden dağıtır|
|/virtualMachineScaleSets/virtualMachines/reimage/action|Sanal Makine Ölçek Kümesindeki Sanal Makine örneğinin görüntüsünü yeniden oluşturur.|
|/virtualMachineScaleSets/virtualMachines/restart/action|VM Ölçek Kümesindeki bir Sanal Makine örneğini yeniden başlatır.|
|/virtualMachineScaleSets/virtualMachines/start/action|VM Ölçek Kümesindeki bir Sanal Makine örneğini başlatır.|
|/virtualMachineScaleSets/virtualMachines/write|VM Ölçek Kümesindeki bir Sanal Makinenin özelliklerini güncelleştirir|
|/virtualMachineScaleSets/write|Yeni bir Sanal Makine Ölçek Kümesi oluşturur veya mevcut kümeyi güncelleştirir|

## <a name="microsoftconsumption"></a>Microsoft.Consumption

| İşlem | Açıklama |
|---|---|
|/balances/Read|Bir yönetim grubu için fatura dönemi için Özet kullanımı listeleyin.|
|/budgets/Read|Bir abonelik veya bir yönetim grubu tarafından bütçeleri listeleyin.|
|/ Bütçeleri/yazma|Oluşturur, güncelleştirme ve bir abonelik veya bir yönetim grubu tarafından bütçeleri silme.|
|/marketplaces/Read|EA ve WebDirect abonelikler için bir kapsam Market kaynak kullanım ayrıntılarını listeler.|
|/Operations/Read|Microsoft.Consumption kaynak sağlayıcısı tarafından desteklenen tüm işlemleri listele.|
|/pricesheets/Read|Bir abonelik veya bir yönetim grubu için Pricesheets veri listeleyin.|
|/reservationDetails/read|Ayırma sipariş veya yönetim gruplarına göre ayrılmış örnekler kullanımı ayrıntılarını listeler. Günlük düzeyi başına örnek başına ayrıntıları verilerdir.|
|/reservationSummaries/read|Ayırma sipariş veya yönetim gruplarına göre ayrılmış örnekler için Özet kullanımı listeleyin. Özet günlük veya aylık düzeyinde ya da veridir.|
|/reservationTransactions/read|Yönetim grubu tarafından ayrılmış örnekler için işlem geçmişini listeleyin.|
|/Terms/Read|Bir abonelik veya bir yönetim grubu koşulları listeler.|
|/usageDetails/Read|EA ve WebDirect abonelikler için bir kapsam kullanım ayrıntılarını listeler.|

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

| İşlem | Açıklama |
|---|---|
|/containerGroups/Containers/Logs/Read|Belirli bir kapsayıcı için günlükleri alın.|
|/containerGroups/delete|Belirli kapsayıcı grubunu silin.|
|/containerGroups/providers/Microsoft.Insights/diagnosticSettings/read|Kapsayıcı grubu tanılama ayarını alır.|
|/containerGroups/providers/Microsoft.Insights/diagnosticSettings/write|Kapsayıcı grubu tanılama ayarını güncelleştirir veya oluşturur.|
|/containerGroups/providers/Microsoft.Insights/metricDefinitions/read|Kapsayıcı grubu için kullanılabilir ölçümleri alır.|
|/containerGroups/Read|Tüm kapsayıcı gruplarını alın.|
|/containerGroups/write|Belirli bir kapsayıcı grubunu oluşturun veya güncelleştirin.|

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

| İşlem | Açıklama |
|---|---|
|/checkNameAvailability/read|Kapsayıcı kayıt defteri adı kullanılabilir olup olmadığını denetler.|
|/Locations/operationResults/Read|Zaman uyumsuz işlem sonucunu alır|
|/Operations/Read|Tüm kullanılabilir Azure kapsayıcı kayıt defteri REST API işlemlerinin listeler|
|/register/action|Kapsayıcı kayıt kaynak sağlayıcısı için aboneliği kaydeder ve kapsayıcı kayıt defterleri oluşturulmasını sağlar.|
|/registries/DELETE|Bir kapsayıcı kayıt siler.|
|/registries/eventGridFilters/delete|Olay kılavuz filtresi kapsayıcı kayıt defterinden siler.|
|/registries/eventGridFilters/read|Belirtilen olay kılavuz filtresi özelliklerini alır veya belirtilen kapsayıcı kayıt defteri için tüm olay kılavuz filtreler listelenmiştir.|
|/registries/eventGridFilters/write|Belirtilen parametrelerle bir kapsayıcı kayıt defteri olay kılavuz filtresi güncelleştirir veya oluşturur.|
|/registries/listCredentials/action|Belirtilen kapsayıcı kayıt defteri için oturum açma kimlik bilgilerini listeler.|
|/registries/listUsages/read|Belirtilen kapsayıcı kayıt defteri için kota kullanımları listeler.|
|/registries/operationStatuses/Read|Bir kayıt defteri zaman uyumsuz işlem durumunu alır|
|/registries/Read|Belirtilen kapsayıcı kayıt defteri özelliklerini alır veya tüm kapsayıcı kayıt defterleri belirtilen kaynak grubu veya abonelik altında listelenir.|
|/registries/regenerateCredential/action|Belirtilen kapsayıcı kayıt defteri için oturum açma kimlik bilgilerini yeniden oluşturur.|
|/registries/replications/DELETE|Bir çoğaltma kapsayıcı kayıt defterinden siler.|
|/registries/replications/operationStatuses/Read|Bir çoğaltma zaman uyumsuz işlem durumunu alır|
|/registries/replications/Read|Belirtilen çoğaltma özelliklerini alır veya belirtilen kapsayıcı kayıt defteri için tüm çoğaltmalar listeler.|
|/registries/replications/Write|Belirtilen parametrelerle bir çoğaltma için bir kapsayıcı kayıt defterini güncelleştirir veya oluşturur.|
|/registries/webhooks/delete|Bir Web kancası kapsayıcı kayıt defterinden siler.|
|/registries/webhooks/getCallbackConfig/action|Web kancası için URI hizmetinin yapılandırmasını ve özel üstbilgi alır.|
|/registries/webhooks/listEvents/action|Belirtilen Web kancası için son olayları listeler.|
|/registries/webhooks/operationStatuses/Read|Bir Web kancası zaman uyumsuz işlem durumunu alır|
|/registries/webhooks/ping/action|Web kancası için gönderilecek bir ping olay tetiklenir.|
|/registries/webhooks/Read|Belirtilen Web kancası özelliklerini alır veya belirtilen kapsayıcı kayıt defteri için tüm Web kancalarını listeler.|
|/registries/webhooks/Write|Belirtilen parametrelerle bir Web kancası için bir kapsayıcı kayıt defterini güncelleştirir veya oluşturur.|
|/ kayıt defterleri/yazma|Belirtilen parametrelerle bir kapsayıcı kayıt defterini güncelleştirir veya oluşturur.|

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

| İşlem | Açıklama |
|---|---|
|/containerServices/delete|Belirtilen kapsayıcı hizmetini siler|
|/containerServices/read|Belirtilen kapsayıcı hizmetini alır|
|/containerServices/write|Yerleştirir veya belirtilen kapsayıcı hizmetini güncelleştirir|

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator

| İşlem | Açıklama |
|---|---|
|/Applications/DELETE|İşlemi Siler|
|/applications/listSecrets/action|Parolaları Listele|
|/applications/listSingleSignOnToken/action|Çoklu oturum açma belirteçleri okuma|
|/Applications/Read|İşlemi Okur|
|/applications/write|İşlemi Yazar|
|/applications/write|İşlemi Yazar|
|/listCommunicationPreference/action|Liste iletişim tercihi|
|/Operations/Read|okuma işlemleri|
|/updateCommunicationPreference/action|Güncelleştirme iletişim tercihi|

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights

| İşlem | Açıklama |
|---|---|
|/hubs/adobemetadata/action|Azure müşteri Öngörüler Adobe meta verileri güncelle|
|/hubs/adobemetadata/read|Tüm Azure müşteri Öngörüler Adobe meta verilerini okuma|
|/hubs/authorizationPolicies/DELETE|Tüm Azure müşteri Öngörüler paylaşılan erişim imzası ilkelerini Sil|
|/hubs/authorizationPolicies/Read|Herhangi bir Azure müşteri öngörü paylaşılan erişim imzası İlkesi okuma|
|/hubs/authorizationPolicies/regeneratePrimaryKey/action|Azure müşteri Öngörüler paylaşılan erişim imzası ilkesinin birincil anahtarını yeniden oluşturma|
|/hubs/authorizationPolicies/regenerateSecondaryKey/action|Azure müşteri Öngörüler paylaşılan erişim imzası İlkesi ikincil anahtarını yeniden oluşturma|
|/hubs/authorizationPolicies/Write|Herhangi bir Azure müşteri Öngörüler paylaşılan erişim imzası ilke güncelle|
|/hubs/connectors/Activate/Action|Tüm Azure müşteri Öngörüler Bağlayıcısı etkinleştir|
|/hubs/connectors/Activate/Action|Tüm Azure müşteri Öngörüler Bağlayıcısı etkinleştir|
|/hubs/connectors/DELETE|Tüm Azure müşteri Öngörüler bağlayıcısını silin|
|/hubs/connectors/getruntimestatus/action|Tüm Azure müşteri Öngörüler Bağlayıcısı çalışma zamanı durumunu Al|
|/hubs/connectors/mappings/activate/action|Tüm Azure müşteri Öngörüler Bağlayıcısı eşleme etkinleştir|
|/hubs/connectors/Mappings/DELETE|Tüm Azure müşteri Öngörüler Bağlayıcısı eşleme Sil|
|/hubs/connectors/Mappings/Operations/Read|Bir Azure müşteri Öngörüler Bağlayıcısı eşleme işlem sonucunu oku|
|/hubs/connectors/Mappings/Read|Tüm Azure müşteri Öngörüler Bağlayıcısı eşleme okuma|
|/hubs/connectors/Mappings/Write|Oluşturma veya güncelleştirme tüm Azure müşteri Öngörüler Bağlayıcısı eşlemesi|
|/hubs/connectors/Operations/Read|Bir Azure müşteri Öngörüler Bağlayıcısı işlem sonucunu oku|
|/hubs/connectors/Read|Tüm Azure müşteri Öngörüler Bağlayıcısı okuma|
|/hubs/connectors/saveauthinfo/action|Tüm Azure müşteri Öngörüler Bağlayıcısı kimlik doğrulama bilgileri güncelle|
|/hubs/connectors/update/action|Tüm Azure müşteri Öngörüler Bağlayıcısı güncelleştir|
|/hubs/connectors/Write|Tüm Azure müşteri Öngörüler Bağlayıcısı güncelle|
|/hubs/crmmetadata/Action|Azure müşteri Öngörüler Crm meta verileri güncelle|
|/hubs/crmmetadata/Read|Tüm Azure müşteri Öngörüler Crm meta verilerini okuma|
|/hubs/DELETE|Tüm Azure müşteri Öngörüler hub'ını silmek|
|/hubs/gdpr/delete|Tüm Azure müşteri Öngörüler Gdpr Sil|
|/hubs/gdpr/Read|Tüm Azure müşteri Öngörüler Gdpr okuma|
|/hubs/gdpr/write|Tüm Azure müşteri Öngörüler Gdpr güncelle|
|/hubs/getbillingcredits/read|Azure müşteri Öngörüler Hub faturalama krediler alırsınız|
|/hubs/getbillinghistory/read|Azure müşteri Öngörüler Hub fatura geçmişini alma|
|/hubs/images/delete|Herhangi bir Azure müşteri Öngörüler görüntü Sil|
|/hubs/images/Read|Herhangi bir Azure müşteri Öngörüler görüntü okuma|
|/hubs/images/write|Herhangi bir Azure müşteri Öngörüler görüntü güncelle|
|/hubs/interactions/DELETE|Tüm azure müşteri Öngörüler etkileşimleri Sil|
|/hubs/interactions/Operations/Read|Bir Azure müşteri Öngörüler etkileşim işlem sonucunu oku|
|/hubs/interactions/Read|Azure müşteri Öngörüler etkileşimi okuma|
|/hubs/interactions/suggestrelationshiplinks/Action|İlişki bağlantılar için herhangi bir Azure müşteri Öngörüler etkileşimi önerisi|
|/hubs/interactions/write|Azure müşteri Öngörüler etkileşimi güncelle|
|/hubs/kpi/delete|Tüm Azure müşteri Öngörüler ana performans göstergesinin Sil|
|/hubs/Kpi/Operations/Read|Bir Azure müşteri Öngörüler anahtar performans göstergelerini işlem sonucunu oku|
|/hubs/Kpi/Read|Tüm Azure müşteri Öngörüler ana performans göstergesinin okuma|
|/hubs/kpi/reprocess/action|Tüm Azure müşteri Öngörüler ana performans göstergelerini yeniden işleyin|
|/hubs/Kpi/Write|Tüm Azure müşteri Öngörüler ana performans göstergesinin güncelle|
|/hubs/Links/DELETE|Tüm Azure müşteri Öngörüler bağlantılar Sil|
|/hubs/Links/Operations/Read|Bir Azure müşteri Öngörüler bağlantılar işlem sonucunu oku|
|/hubs/Links/Read|Tüm Azure müşteri Öngörüler bağlantılar okuma|
|/hubs/Links/Write|Tüm Azure müşteri bağlantılar güncelle|
|/hubs/msemetadata/action|Azure müşteri Öngörüler Mse meta verileri güncelle|
|/hubs/msemetadata/read|Tüm Azure müşteri Öngörüler Mse meta verilerini okuma|
|/hubs/operationresults/Read|Azure müşteri Öngörüler Hub İşlem sonuçlarını alır|
|/hubs/predictions/DELETE|Tüm Azure müşteri Öngörüler tahminleri Sil|
|/hubs/predictions/Operations/Read|Bir Azure müşteri Öngörüler tahminleri işlem sonucunu oku|
|/hubs/predictions/Read|Tüm Azure müşteri Öngörüler tahminleri okuma|
|/hubs/predictions/Write|Tüm Azure müşteri tahminleri güncelle|
|/hubs/predictivematchpolicies/delete|Tüm Azure müşteri Öngörüler Tahmine dayalı eşleşme ilkelerini Sil|
|/hubs/predictivematchpolicies/operations/read|Bir Azure müşteri Öngörüler Tahmine dayalı eşleşme ilkeleri işlem sonucunu oku|
|/hubs/predictivematchpolicies/read|Tüm Azure müşteri Öngörüler Tahmine dayalı eşleşme ilkeleri okuma|
|/hubs/predictivematchpolicies/write|Tüm Azure müşteri Öngörüler Tahmine dayalı eşleşme ilkeleri güncelle|
|/hubs/Profiles/DELETE|Herhangi bir Azure müşteri Öngörüler profil Sil|
|/hubs/Profiles/Operations/Read|Bir Azure müşteri Öngörüler profili işlem sonucunu oku|
|/hubs/Profiles/Read|Tüm Azure müşteri Öngörüler profilini okuma|
|/hubs/Profiles/Write|Herhangi bir Azure müşteri Öngörüler profil yazma|
|/hubs/providers/Microsoft.Insights/diagnosticSettings/read|Kaynak için tanılama ayarını alır|
|/hubs/providers/Microsoft.Insights/diagnosticSettings/write|Kaynak için tanılama ayarını oluşturur veya güncelleştirir|
|/hubs/providers/Microsoft.Insights/logDefinitions/read|Kaynak için kullanılabilir günlüklerini alır|
|/hubs/providers/Microsoft.Insights/metricDefinitions/read|Kaynak için kullanılabilir ölçümleri alır|
|/hubs/Read|Tüm Azure müşteri Öngörüler hub'ı okuyun|
|/hubs/relationshiplinks/DELETE|Tüm Azure müşteri Öngörüler ilişki bağlantılar Sil|
|/hubs/relationshiplinks/Operations/Read|Bir Azure müşteri Öngörüler ilişki bağlantılar işlem sonucunu oku|
|/hubs/relationshiplinks/Read|Tüm Azure müşteri Öngörüler ilişki bağlantılar okuma|
|/hubs/relationshiplinks/Write|Tüm Azure müşteri Öngörüler ilişki bağlantılar güncelle|
|/hubs/relationships/DELETE|Tüm Azure müşteri Öngörüler ilişkileri silin|
|/hubs/relationships/Operations/Read|Bir Azure müşteri Öngörüler ilişkileri işlem sonucunu oku|
|/hubs/relationships/Read|Tüm Azure müşteri Öngörüler ilişkiler okuma|
|/hubs/relationships/Write|Tüm Azure müşteri Öngörüler ilişkiler güncelle|
|/hubs/roleAssignments/delete|Tüm Azure müşteri Öngörüler Rbac atamasını silin|
|/hubs/roleAssignments/Operations/Read|Bir Azure müşteri Öngörüler Rbac atama işlem sonucunu oku|
|/hubs/roleAssignments/read|Herhangi bir Azure müşteri Öngörüler Rbac atama okuma|
|/hubs/roleAssignments/write|Herhangi bir Azure müşteri Öngörüler Rbac atama güncelle|
|/hubs/Roles/Read|Herhangi bir Azure müşteri Öngörüler Rbac rolü okuma|
|/hubs/salesforcemetadata/action|Azure müşteri Öngörüler SalesForce meta verileri güncelle|
|/hubs/salesforcemetadata/Read|Tüm Azure müşteri Öngörüler SalesForce meta verilerini okuma|
|/hubs/segments/delete|Tüm Azure müşteri Öngörüler segmentleri silin|
|/hubs/segments/dynamic/action|Tüm Azure müşteri Insight dinamik yönetim kesim|
|/hubs/segments/Read|Tüm Azure müşteri Öngörüler kesimleri okuma|
|/hubs/segments/static/action|Herhangi bir Azure müşteri Insight statik yönetim kesim|
|/hubs/segments/write|Tüm Azure müşteri Öngörüler kesimleri güncelle|
|/hubs/sqlconnectionstrings/delete|Tüm Azure müşteri Öngörüler SqlConnectionStrings Sil|
|/hubs/sqlconnectionstrings/read|Tüm Azure müşteri Öngörüler SqlConnectionStrings okuma|
|/hubs/sqlconnectionstrings/write|Tüm Azure müşteri Öngörüler SqlConnectionStrings güncelle|
|/hubs/suggesttypeschema/action|Örnek verileri önermek türü şeması oluştur|
|/hubs/tenantmanagement/read|Tüm Azure müşteri Öngörüler hub ayarlarını yönet|
|/hubs/Views/DELETE|Herhangi bir Azure müşteri Öngörüler uygulama Görünümü Sil|
|/hubs/Views/Read|Herhangi bir Azure müşteri Öngörüler uygulama görünümü okuma|
|/hubs/Views/Write|Herhangi bir Azure müşteri Öngörüler uygulama Görünümü güncelle|
|/hubs/widgettypes/read|Tüm Azure müşteri Öngörüler uygulama pencere öğesi türlerine okuma|
|/ hubs/yazma|Tüm Azure müşteri Öngörüler Hub güncelle|
|/Operations/Read|Azure müşteri Öngörüler API Metadatas okuma|
|/register/action|Müşteri Öngörüler kaynak sağlayıcısı için aboneliği kaydeder ve müşteri Öngörüler kaynakların oluşturulmasını sağlar|
|/unregister/action|Müşteri Öngörüler kaynak sağlayıcısı için abonelik kaydını siler|

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

| İşlem | Açıklama |
|---|---|
|/Catalogs/DELETE|Katalog siler.|
|/Catalogs/Read|Katalog ya da kataloglar abonelik veya kaynak grubu altında için özellikleri alır.|
|/ kataloglar/yazma|Katalog oluşturur veya etiketleri ve Katalog özelliklerini güncelleştirir.|
|/checkNameAvailability/action|Kiracı için katalog adı kullanılabilirliğini denetler.|
|/Operations/Read|Microsoft.DataCatalog kaynak sağlayıcısı kullanılabilir işlemleri listeler.|
|/register/action|Abonelik Microsoft.DataCatalog kaynak sağlayıcısına kaydeder.|
|/unregister/action|Abonelik Microsoft.DataCatalog kaynak sağlayıcısından gelen kaydını siler.|

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

| İşlem | Açıklama |
|---|---|
|/datafactories/providers/Microsoft.Insights/diagnosticSettings/read|Kaynak için tanılama ayarını alır|
|/datafactories/providers/Microsoft.Insights/diagnosticSettings/write|Kaynak için tanılama ayarını oluşturur veya güncelleştirir|
|/datafactories/providers/Microsoft.Insights/metricDefinitions/read|Datafactories için kullanılabilir ölçümleri alır|
|/factories/providers/Microsoft.Insights/diagnosticSettings/Read|Kaynak için tanılama ayarını alır|
|/factories/providers/Microsoft.Insights/diagnosticSettings/write|Kaynak için tanılama ayarını oluşturur veya güncelleştirir|
|/factories/providers/Microsoft.Insights/logDefinitions/read|Oluşturucuları için kullanılabilir günlüklerini alır|
|/factories/providers/Microsoft.Insights/metricDefinitions/Read|Oluşturucuları için kullanılabilir ölçümleri alır|

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

| İşlem | Açıklama |
|---|---|
|/accounts/computePolicies/delete|Bir işlem ilkeyi silin.|
|/Accounts/computePolicies/Read|Bir işlem İlkesi hakkında bilgi alın.|
|/accounts/computePolicies/write|Oluşturun veya bir işlem İlkesi güncelleştirin.|
|/accounts/dataLakeStoreAccounts/delete|DataLakeAnalytics hesap DataLakeStore hesabından bağlantısını.|
|/accounts/dataLakeStoreAccounts/read|DataLakeAnalytics hesabı bağlı DataLakeStore hesabı hakkında bilgi alın.|
|/accounts/dataLakeStoreAccounts/write|Oluşturun veya bir bağlantılı DataLakeStore hesabı DataLakeAnalytics hesabının güncelleştirin.|
|/Accounts/DELETE|Bir DataLakeAnalytics hesabı silin.|
|/Accounts/firewallRules/DELETE|Bir güvenlik duvarı kuralını siler.|
|/Accounts/firewallRules/Read|Bir güvenlik duvarı kuralı hakkında bilgi alın.|
|/Accounts/firewallRules/Write|Oluşturun veya bir güvenlik duvarı kuralı güncelleştirin.|
|/accounts/operationResults/read|DataLakeAnalytics hesabı işleminin sonucu alın.|
|/accounts/providers/Microsoft.Insights/diagnosticSettings/read|DataLakeAnalytics hesabı için tanılama ayarlarını al.|
|/accounts/providers/Microsoft.Insights/diagnosticSettings/write|Oluşturun veya DataLakeAnalytics hesabının tanılama ayarlarını güncelleştirin.|
|/accounts/providers/Microsoft.Insights/logDefinitions/read|Kullanılabilir günlüklerini DataLakeAnalytics hesabı alamıyor.|
|/accounts/providers/Microsoft.Insights/metricDefinitions/read|Kullanılabilir ölçümler için DataLakeAnalytics hesabı edinin.|
|/Accounts/Read|DataLakeAnalytics hesabınız hakkında bilgi alın.|
|/accounts/storageAccounts/Containers/listSasTokens/action|DataLakeAnalytics hesabın bağlantılı bir depolama hesabının depolama kapsayıcıları için SAS belirteci listeleyin.|
|/accounts/storageAccounts/Containers/read|DataLakeAnalytics hesabının bağlı bir depolama hesabında kapsayıcıları alın.|
|/accounts/storageAccounts/delete|Bir depolama hesabı DataLakeAnalytics hesabından bağlantısını.|
|/accounts/storageAccounts/read|DataLakeAnalytics hesabın bağlantılı bir depolama hesabı hakkında bilgi alın.|
|/accounts/storageAccounts/write|Oluşturun veya bağlı bir depolama hesabında DataLakeAnalytics hesabının güncelleştirin.|
|/accounts/TakeOwnership/action|Diğer kullanıcılar tarafından gönderilen işleri iptal etme izni verin.|
|/ hesapları/yazma|Oluşturun veya bir DataLakeAnalytics hesabı güncelleştirin.|
|/Locations/capability/Read|Bir abonelik Yetenek bilgilerini al DataLakeAnalytics kullanma ile ilgili.|
|/locations/checkNameAvailability/action|DataLakeAnalytics hesap adı kullanılabilirliğini denetleyin.|
|/Locations/operationResults/Read|DataLakeAnalytics hesabı işleminin sonucu alın.|
|/Operations/Read|DataLakeAnalytics kullanılabilir işlemleri alın.|
|/register/action|DataLakeAnalytics aboneliğine kaydolun.|

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

| İşlem | Açıklama |
|---|---|
|/Accounts/DELETE|Bir DataLakeStore hesabı silin.|
|/accounts/enableKeyVault/action|KeyVault DataLakeStore hesabı için etkinleştirin.|
|/Accounts/firewallRules/DELETE|Bir güvenlik duvarı kuralını siler.|
|/Accounts/firewallRules/Read|Bir güvenlik duvarı kuralı hakkında bilgi alın.|
|/Accounts/firewallRules/Write|Oluşturun veya bir güvenlik duvarı kuralı güncelleştirin.|
|/accounts/operationResults/read|DataLakeStore hesabı işleminin sonucu alın.|
|/accounts/providers/Microsoft.Insights/diagnosticSettings/read|DataLakeStore hesabı için tanılama ayarlarını al.|
|/accounts/providers/Microsoft.Insights/diagnosticSettings/write|Oluşturun veya DataLakeStore hesabının tanılama ayarlarını güncelleştirin.|
|/accounts/providers/Microsoft.Insights/logDefinitions/read|Kullanılabilir günlüklerini DataLakeStore hesabı alamıyor.|
|/accounts/providers/Microsoft.Insights/metricDefinitions/read|Kullanılabilir ölçümler için DataLakeStore hesabı edinin.|
|/Accounts/Read|DataLakeStore hesabınız hakkında bilgi alın.|
|/accounts/Superuser/action|Süper kullanıcı ile Microsoft.Authorization/roleAssignments/write verilmişse Data Lake Store üzerinde verin.|
|/accounts/trustedIdProviders/delete|Güvenilen kimlik sağlayıcısı silin.|
|/accounts/trustedIdProviders/read|Güvenilen kimlik sağlayıcısı hakkında bilgi alın.|
|/accounts/trustedIdProviders/write|Oluşturun veya bir güvenilen kimlik sağlayıcısı güncelleştirin.|
|/ hesapları/yazma|Oluşturun veya bir DataLakeStore hesabı güncelleştirin.|
|/Locations/capability/Read|Bir abonelik Yetenek bilgilerini al DataLakeStore kullanma ile ilgili.|
|/locations/checkNameAvailability/action|DataLakeStore hesap adı kullanılabilirliğini denetleyin.|
|/Locations/operationResults/Read|DataLakeStore hesabı işleminin sonucu alın.|
|/Operations/Read|DataLakeStore kullanılabilir işlemleri alın.|
|/register/action|DataLakeStore aboneliğine kaydolun.|

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

| İşlem | Açıklama |
|---|---|
|/locations/performanceTiers/read|Performans katmanı kullanılabilir listesini döndürür.|
|/performanceTiers/read|Performans katmanı kullanılabilir listesini döndürür.|
|/servers/delete|Var olan bir sunucuyu siler.|
|/servers/firewallRules/delete|Mevcut bir güvenlik duvarı kuralını siler.|
|/servers/firewallRules/read|Güvenlik Duvarı listesi için belirtilen güvenlik duvarı kuralı özellikleri alır veya bir sunucu için kuralları döndür.|
|/servers/firewallRules/write|Bir güvenlik duvarı kuralı belirtilen parametreleri veya güncelleştirme mevcut bir kuralı oluşturur.|
|/servers/providers/Microsoft.Insights/diagnosticSettings/read|Kaynak disagnostic ayarını alır|
|/servers/providers/Microsoft.Insights/diagnosticSettings/write|Kaynak için tanılama ayarını oluşturur veya güncelleştirir|
|/servers/providers/Microsoft.Insights/metricDefinitions/read|Dönüş türleri veritabanları için kullanılabilir ölçümleri|
|/Servers/Read|Sunucuları veya alır belirtilen sunucu için özellikler listesini döndürür.|
|/servers/recoverableServers/read|Kurtarılabilir MySQL sunucu bilgilerini döndürür|
|/servers/virtualNetworkRules/delete|Mevcut bir sanal ağ kuralını siler|
|/servers/virtualNetworkRules/read|Dönüş sanal ağ listesi kuralları veya belirtilen sanal ağ kuralı özellikleri alır.|
|/servers/virtualNetworkRules/write|Belirtilen parametrelerle bir sanal ağ kuralı oluşturur veya özellikleri veya etiketleri belirtilen sanal ağ kuralı için güncelleştirin.|
|/ sunucuları/yazma|Sunucu belirtilen parametrelerle oluşturur veya özellikleri veya etiketleri belirtilen sunucu güncelleştirme.|

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

| İşlem | Açıklama |
|---|---|
|/locations/performanceTiers/read|Performans katmanı kullanılabilir listesini döndürür.|
|/performanceTiers/read|Performans katmanı kullanılabilir listesini döndürür.|
|/servers/delete|Var olan bir sunucuyu siler.|
|/servers/firewallRules/delete|Mevcut bir güvenlik duvarı kuralını siler.|
|/servers/firewallRules/read|Güvenlik Duvarı listesi için belirtilen güvenlik duvarı kuralı özellikleri alır veya bir sunucu için kuralları döndür.|
|/servers/firewallRules/write|Bir güvenlik duvarı kuralı belirtilen parametreleri veya güncelleştirme mevcut bir kuralı oluşturur.|
|/servers/providers/Microsoft.Insights/diagnosticSettings/read|Kaynak disagnostic ayarını alır|
|/servers/providers/Microsoft.Insights/diagnosticSettings/write|Kaynak için tanılama ayarını oluşturur veya güncelleştirir|
|/servers/providers/Microsoft.Insights/logDefinitions/read|Dönüş veritabanları için kullanılabilir günlük türleri|
|/servers/providers/Microsoft.Insights/metricDefinitions/read|Dönüş türleri veritabanları için kullanılabilir ölçümleri|
|/Servers/Read|Sunucuları veya alır belirtilen sunucu için özellikler listesini döndürür.|
|/servers/recoverableServers/read|Kurtarılabilir PostgreSQL sunucu bilgilerini döndürür|
|/servers/virtualNetworkRules/delete|Mevcut bir sanal ağ kuralını siler|
|/servers/virtualNetworkRules/read|Dönüş sanal ağ listesi kuralları veya belirtilen sanal ağ kuralı özellikleri alır.|
|/servers/virtualNetworkRules/write|Belirtilen parametrelerle bir sanal ağ kuralı oluşturur veya özellikleri veya etiketleri belirtilen sanal ağ kuralı için güncelleştirin.|
|/ sunucuları/yazma|Sunucu belirtilen parametrelerle oluşturur veya özellikleri veya etiketleri belirtilen sunucu güncelleştirme.|

## <a name="microsoftdevices"></a>Microsoft.Devices

| İşlem | Açıklama |
|---|---|
|/checkNameAvailability/Action|Onay, Iothub adı kullanılamıyor|
|/checkProvisioningServiceNameAvailability/Action|Onay, Iothub adı kullanılamıyor|
|/ElasticPools/diagnosticSettings/read|Kaynak için tanılama ayarını alır|
|/ElasticPools/diagnosticSettings/write|Kaynak için tanılama ayarını oluşturur veya güncelleştirir|
|/elasticPools/iotHubTenants/Delete|Iothub Kiracı kaynağı silme|
|/ElasticPools/IotHubTenants/diagnosticSettings/read|Kaynak için tanılama ayarını alır|
|/ElasticPools/IotHubTenants/diagnosticSettings/write|Kaynak için tanılama ayarını oluşturur veya güncelleştirir|
|/elasticPools/iotHubTenants/eventHubEndpoints/consumerGroups/Delete|EventHub tüketici grubu Sil|
|/elasticPools/iotHubTenants/eventHubEndpoints/consumerGroups/Read|EventHub tüketici grubu Al|
|/elasticPools/iotHubTenants/eventHubEndpoints/consumerGroups/Write|EventHub tüketici grubu oluşturma|
|/elasticPools/iotHubTenants/exportDevices/Action|Dışarı aktarma cihazları|
|/elasticPools/iotHubTenants/getStats/Read|Iothub Kiracı istatistikleri kaynak alır|
|/elasticPools/iotHubTenants/importDevices/Action|Cihazları içeri aktarma|
|/elasticPools/iotHubTenants/iotHubKeys/listkeys/Action|Iothub Kiracı anahtarını alır|
|/elasticPools/iotHubTenants/jobs/Read|İş ayrıntıları Iothub gönderme|
|/elasticPools/iotHubTenants/listKeys/Action|Iothub Kiracı anahtarları alır|
|/ElasticPools/IotHubTenants/logDefinitions/read|Iothub hizmeti için kullanılabilir günlük tanımları alır|
|/ ElasticPools/IotHubTenants/metricDefinitions/okuma|Iothub hizmeti için kullanılabilir ölçümleri alır|
|/elasticPools/iotHubTenants/quotaMetrics/Read|Kota ölçümleri alma|
|/elasticPools/iotHubTenants/Read|Iothub Kiracı kaynak alır|
|/elasticPools/iotHubTenants/Routing/Routes/$ testall/eylem|Bir ileti tüm var olan yollar karşı test etme|
|/elasticPools/iotHubTenants/Routing/Routes/$ testnew/eylem|Bir ileti sağlanan test rota karşı test etme|
|/elasticPools/iotHubTenants/routingEndpointsHealth/Read|Bir Iothub için tüm yönlendirme uç noktaları durumunu alır|
|/elasticPools/iotHubTenants/Write|Iothub Kiracı kaynak güncelle|
|/ElasticPools/metricDefinitions/read|Iothub hizmeti için kullanılabilir ölçümleri alır|
|/iotHubs/certificates/generateVerificationCode/Action|Doğrulama kodu oluştur|
|/iotHubs/certificates/verify/Action|Sertifika kaynak doğrulayın|
|/iotHubs/Delete|Iothub kaynağı silme|
|/IotHubs/diagnosticSettings/read|Kaynak için tanılama ayarını alır|
|/IotHubs/diagnosticSettings/write|Kaynak için tanılama ayarını oluşturur veya güncelleştirir|
|/iotHubs/eventGridFilters/Delete|Olay kılavuz filtre siler|
|/iotHubs/eventGridFilters/Read|Olay kılavuz filtreyi alır|
|/iotHubs/eventGridFilters/Write|Yeni oluşturun veya varolan bir olay kılavuz filtreyi güncelleştirin|
|/iotHubs/eventHubEndpoints/consumerGroups/Delete|EventHub tüketici grubu Sil|
|/iotHubs/eventHubEndpoints/consumerGroups/Read|EventHub tüketici grubu Al|
|/iotHubs/eventHubEndpoints/consumerGroups/Write|EventHub tüketici grubu oluşturma|
|/iotHubs/exportDevices/Action|Dışarı aktarma cihazları|
|/iotHubs/importDevices/Action|Cihazları içeri aktarma|
|/iotHubs/iotHubKeys/listkeys/Action|Verilen ada Iothub anahtarı alma|
|/iotHubs/iotHubStats/Read|Get IotHub Statistics|
|/iotHubs/jobs/Read|İş ayrıntıları Iothub gönderme|
|/iotHubs/listkeys/Action|Tüm Iothub anahtarları alma|
|/IotHubs/logDefinitions/read|Iothub hizmeti için kullanılabilir günlük tanımları alır|
|/IotHubs/metricDefinitions/read|Iothub hizmeti için kullanılabilir ölçümleri alır|
|/iotHubs/quotaMetrics/Read|Kota ölçümleri alma|
|/iotHubs/Read|Iothub kaynaklar alır|
|/iotHubs/routing/$testall/Action|Bir ileti tüm var olan yollar karşı test etme|
|/iotHubs/routing/$testnew/Action|Bir ileti sağlanan test rota karşı test etme|
|/iotHubs/routingEndpointsHealth/Read|Bir Iothub için tüm yönlendirme uç noktaları durumunu alır|
|/iotHubs/skus/Read|Get valid IotHub Skus|
|/iotHubs/Write|Iothub kaynak güncelle|
|/ operations/okuma|Tüm ResourceProvider işlemleri Al|
|/provisioningServices/certificates/generateVerificationCode/Action|Doğrulama kodu oluştur|
|/provisioningServices/certificates/verify/Action|Sertifika kaynak doğrulayın|
|/provisioningServices/Delete|IotDps kaynağı silme|
|/provisioningServices/diagnosticSettings/read|Kaynak için tanılama ayarını alır|
|/provisioningServices/diagnosticSettings/write|Kaynak için tanılama ayarını oluşturur veya güncelleştirir|
|/provisioningServices/listkeys/Action|Tüm IotDps anahtarları alma|
|/provisioningServices/logDefinitions/read|Sağlama hizmeti için kullanılabilir günlük tanımları alır|
|/provisioningServices/metricDefinitions/read|Sağlama hizmeti için kullanılabilir ölçümleri alır|
|/provisioningServices/ProvisioningServiceKeys/listkeys/Action|Anahtar adı için IotDps anahtarları alma|
|/provisioningServices/Read|IotDps kaynak alma|
|/provisioningServices/skus/Read|Geçerli IotDps SKU'ları alma|
|/provisioningServices/Write|IotDps kaynağı oluşturma|
|/register/action|Iothub kaynak sağlayıcısı için abonelik kaydı ve Iothub kaynakların oluşturulmasını sağlar|
|/register/action|Iothub kaynak sağlayıcısı için abonelik kaydı ve Iothub kaynakların oluşturulmasını sağlar|
|/ kullanımları/okuma|Abonelik bu sağlayıcı için kullanım ayrıntıları alın.|

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

| İşlem | Açıklama |
|---|---|
|/labCenters/delete|Laboratuvar merkezleri silin.|
|/labCenters/read|Laboratuvar merkezleri okuyun.|
|/labCenters/write|Ekleyin veya Laboratuvar merkezleri değiştirin.|
|/labs/artifactSources/armTemplates/read|Azure resource manager şablonları okuyun.|
|/labs/artifactSources/artifacts/GenerateArmTemplate/action|Belirtilen yapı için bir ARM şablonu oluşturur, bir depolama hesabı için gereken dosyaları yükler ve oluşturulan yapı doğrular.|
|/labs/artifactSources/artifacts/read|Yapıları okuyun.|
|/labs/artifactSources/delete|Yapı kaynaklarını silin.|
|/labs/artifactSources/read|Yapı kaynaklarını okuyun.|
|/labs/artifactSources/write|Ekleyin veya Yapı kaynaklarını değiştirin.|
|/labs/ClaimAnyVm/action|Laboratuvar rastgele claimable sanal makinede talep.|
|/Labs/costs/Read|Maliyetleri okuyun.|
|/Labs/costs/Write|Ekleyin veya maliyetleri değiştirin.|
|/labs/CreateEnvironment/action|Sanal makineler bir laboratuar ortamında oluşturun.|
|/labs/customImages/delete|Özel resimler silin.|
|/labs/customImages/read|Özel resimler okuyun.|
|/labs/customImages/write|Ekleyin veya özel resimler değiştirin.|
|/Labs/DELETE|Labs silin.|
|/labs/ExportResourceUsage/action|Laboratuvar kaynak kullanımı bir depolama hesabına dışa aktarır.|
|/Labs/Formulas/DELETE|Formülleri silin.|
|/Labs/Formulas/Read|Formülleri okuyun.|
|/Labs/Formulas/Write|Ekleyin veya formüller değiştirin.|
|/labs/galleryImages/read|Galeri görüntüleri okuyun.|
|/labs/GenerateUploadUri/action|Özel disk görüntülerini laboratuvara yüklemek için bir URI oluşturur.|
|/labs/ImportVirtualMachine/action|Bir sanal makine farklı bir laboratuvar alın.|
|/labs/ListVhds/action|Liste disk görüntülerini özel görüntü oluşturma için kullanılabilir.|
|/labs/notificationChannels/delete|Notificationchannels silin.|
|/labs/notificationChannels/Notify/action|Bildirim sağlanan kanala göndermek.|
|/labs/notificationChannels/read|Notificationchannels okuyun.|
|/labs/notificationChannels/write|Ekleyin veya notificationchannels değiştirin.|
|/labs/policySets/EvaluatePolicies/action|Laboratuvar ilkeyi değerlendirir.|
|/Labs/policySets/Policies/DELETE|İlkeleri silin.|
|/Labs/policySets/Policies/Read|İlkeleri okuyun.|
|/labs/policySets/policies/write|Ekleyin veya ilkeleri değiştirin.|
|/Labs/Read|Labs okuyun.|
|/Labs/Schedules/DELETE|Zamanlamalar silin.|
|/Labs/Schedules/Execute/Action|Bir zamanlama yürütün.|
|/Labs/Schedules/ListApplicable/Action|Tüm geçerli zamanlamaları listeler|
|/Labs/Schedules/Read|Zamanlamalar okuyun.|
|/Labs/Schedules/Write|Ekleyin veya zamanlamalarını değiştirin.|
|/labs/serviceRunners/delete|Hizmet koşucular silin.|
|/labs/serviceRunners/read|Hizmet koşucular okuyun.|
|/labs/serviceRunners/write|Ekleyin veya hizmet koşucular değiştirin.|
|/Labs/Users/DELETE|Kullanıcı profillerini silin.|
|/labs/users/disks/Attach/action|Ekleme ve disk sanal makineye kira oluşturun.|
|/labs/users/disks/delete|Diskleri silin.|
|/labs/users/disks/Detach/action|Ayırma ve sanal makineye bağlı disk kira bölün.|
|/Labs/Users/Disks/Read|Diskleri okuyun.|
|/labs/users/disks/write|Ekleyin veya diskleri değiştirin.|
|/Labs/Users/Environments/DELETE|Ortamlar silin.|
|/Labs/Users/Environments/Read|Ortamlar okuyun.|
|/Labs/Users/Environments/Write|Ekleyin veya ortamlar değiştirin.|
|/Labs/Users/Read|Kullanıcı profillerini okuyun.|
|/Labs/Users/Secrets/DELETE|Gizli silin.|
|/Labs/Users/Secrets/Read|Gizli okuyun.|
|/Labs/Users/Secrets/Write|Ekleyin veya gizli değiştirin.|
|/labs/users/serviceFabrics/delete|Hizmet dokuları silin.|
|/labs/users/serviceFabrics/ListApplicableSchedules/action|Tüm geçerli zamanlamaları listeler|
|/labs/users/serviceFabrics/read|Hizmet dokuları okuyun.|
|/labs/users/serviceFabrics/schedules/delete|Zamanlamalar silin.|
|/labs/users/serviceFabrics/schedules/Execute/action|Bir zamanlama yürütün.|
|/labs/users/serviceFabrics/schedules/read|Zamanlamalar okuyun.|
|/labs/users/serviceFabrics/schedules/write|Ekleyin veya zamanlamalarını değiştirin.|
|/labs/users/serviceFabrics/Start/action|Service fabric başlatın.|
|/labs/users/serviceFabrics/Stop/action|Service fabric Durdur|
|/labs/users/serviceFabrics/write|Ekleyin veya hizmet dokuları değiştirin.|
|/Labs/Users/Write|Ekleyin veya kullanıcı profillerini değiştirin.|
|/labs/virtualMachines/AddDataDisk/action|Yeni veya var olan veri diski sanal makineye Ekle.|
|/labs/virtualMachines/ApplyArtifacts/action|Yapılar, sanal makine için geçerlidir.|
|/labs/virtualMachines/Claim/action|Var olan bir sanal makine sahipliğini alın|
|/labs/virtualMachines/delete|Sanal makineleri silin.|
|/labs/virtualMachines/DetachDataDisk/action|Belirtilen sanal makinede diski kullanımdan çıkarın.|
|/labs/virtualMachines/ListApplicableSchedules/action|Tüm geçerli zamanlamaları listeler|
|/Labs/virtualMachines/Read|Sanal makineler okuyun.|
|/Labs/virtualMachines/restart/Action|Bir sanal makineyi yeniden başlatın.|
|/labs/virtualMachines/schedules/delete|Zamanlamalar silin.|
|/Labs/virtualMachines/Schedules/Execute/Action|Bir zamanlama yürütün.|
|/Labs/virtualMachines/Schedules/Read|Zamanlamalar okuyun.|
|/Labs/virtualMachines/Schedules/Write|Ekleyin veya zamanlamalarını değiştirin.|
|/Labs/virtualMachines/Start/Action|Bir sanal makineyi başlatın.|
|/Labs/virtualMachines/Stop/Action|Bir sanal makineyi durdurma|
|/labs/virtualMachines/TransferDisks/action|Sanal makine veri diski kendinize sahipliğini aktarma|
|/labs/virtualMachines/UnClaim/action|Varolan bir sanal makinenin sahipliğini sürüm|
|/labs/virtualMachines/write|Sanal makineleri ekleyin veya değiştirin.|
|/labs/virtualNetworks/delete|Sanal ağlar silin.|
|/labs/virtualNetworks/read|Sanal ağlar okuyun.|
|/labs/virtualNetworks/write|Ekleyin veya sanal ağları değiştirin.|
|/ labs/yazma|Ekleyin veya labs değiştirin.|
|/Locations/Operations/Read|Okuma işlemleri.|
|/register/action|Aboneliği kaydeder|
|/schedules/delete|Zamanlamalar silin.|
|/Schedules/Execute/Action|Bir zamanlama yürütün.|
|/schedules/read|Zamanlamalar okuyun.|
|/schedules/Retarget/action|Bir zamanlama ait hedef kaynak kimliği güncelleştirir|
|/ zamanlamaları/yazma|Ekleyin veya zamanlamalarını değiştirin.|

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

| İşlem | Açıklama |
|---|---|
|/databaseAccountNames/read|Ad kullanılabilirliğini denetler.|
|/databaseAccounts/changeResourceGroup/action|Veritabanı hesabı, kaynak grubu Değiştir|
|/databaseAccounts/Databases/Collections/metricDefinitions/Read|Koleksiyon ölçüm tanımlarını okur.|
|/databaseAccounts/Databases/Collections/Metrics/Read|Koleksiyon ölçümleri okur.|
|/databaseAccounts/Databases/Collections/partitionKeyRangeId/Metrics/Read|Veritabanı hesabı bölüm anahtar düzeyindeki ölçümlerini okuma|
|/databaseAccounts/Databases/Collections/Partitions/Metrics/Read|Veritabanı hesabı bölüm düzeyindeki ölçümlerini okuma|
|/databaseAccounts/Databases/Collections/Partitions/usages/Read|Veritabanı hesabı bölüm düzeyi kullanımları okuma|
|/databaseAccounts/Databases/Collections/usages/Read|Koleksiyon kullanımları okur.|
|/databaseAccounts/Databases/metricDefinitions/Read|Ölçüm tanımlarını veritabanını okur|
|/databaseAccounts/databases/metrics/read|Veritabanı ölçümleri okur.|
|/databaseAccounts/databases/usages/read|Veritabanı kullanımları okur.|
|/databaseAccounts/delete|Veritabanı hesaplarını siler.|
|/databaseAccounts/failoverPriorityChange/action|Veritabanı hesabı bölgelerinin yük devretme önceliklerini değiştirin. Bu el ile yük devretme işlemi gerçekleştirmek için kullanılır|
|/databaseAccounts/listConnectionStrings/action|Bağlantı dizeleri için bir veritabanı hesabı edinin|
|/databaseAccounts/listKeys/action|Veritabanı hesabı listesi anahtarları|
|/databaseAccounts/metricDefinitions/read|Veritabanı hesabı ölçümleri tanımları okur.|
|/databaseAccounts/metrics/read|Veritabanı hesabı ölçümleri okur.|
|/databaseAccounts/operationResults/read|Zaman uyumsuz işlemin durumunu okuma|
|/databaseAccounts/percentile/metrics/read|Gecikme ölçümleri|
|/databaseAccounts/percentile/sourceRegion/targetRegion/metrics/read|Belirli bir kaynak ve hedef bölge gecikme ölçümleri|
|/databaseAccounts/percentile/targetRegion/metrics/read|Belirli hedef bölge için gecikme ölçümleri|
|/databaseAccounts/providers/Microsoft.Insights/diagnosticSettings/read|Kaynak için tanılama ayarını alır|
|/databaseAccounts/providers/Microsoft.Insights/diagnosticSettings/write|Kaynak için tanılama ayarını oluşturur veya güncelleştirir|
|/databaseAccounts/providers/Microsoft.Insights/logDefinitions/read|Veritabanı hesabı için kullanılabilir günlük catageries alır|
|/databaseAccounts/providers/Microsoft.Insights/metricDefinitions/read|Veritabanı hesabı için kullanılabilir ölçümleri alır|
|/databaseAccounts/read|Veritabanı hesabı okur.|
|/databaseAccounts/readonlykeys/action|Veritabanı hesabı readonly anahtarları okur.|
|/databaseAccounts/readonlykeys/read|Veritabanı hesabı readonly anahtarları okur.|
|/databaseAccounts/regenerateKey/action|Veritabanı hesabı anahtarlarını döndürün|
|/databaseAccounts/region/databases/collections/metrics/read|Bölgesel koleksiyonu ölçümleri okur.|
|/databaseAccounts/region/databases/collections/partitionKeyRangeId/metrics/read|Bölgesel veritabanı hesabı bölüm anahtar düzeyindeki ölçümlerini okuma|
|/databaseAccounts/Region/Databases/Collections/Partitions/Metrics/Read|Bölgesel veritabanı hesabı bölüm düzeyindeki ölçümlerini okuma|
|/databaseAccounts/Region/Databases/Collections/Partitions/Read|Bir koleksiyondaki veritabanı hesabı bölümleri okuyun|
|/databaseAccounts/region/metrics/read|Bölge ve veritabanı hesabı ölçümleri okur.|
|/databaseAccounts/usages/read|Veritabanı hesabı kullanımları okur.|
|/databaseAccounts/write|Bir veritabanı hesaplarını güncelleştirin.|
|/locations/deleteVirtualNetworkOrSubnets/action|Microsoft.DocumentDB VirtualNetwork veya alt ağı siliniyor olduğunu bildirir.|
|/operationResults/read|Zaman uyumsuz işlemin durumunu okuma|
|/Operations/Read|Microsoft DocumentDB için kullanılabilir okuma işlemleri |
|/register/action| Aboneliği için Microsoft DocumentDB kaynak sağlayıcısı kaydetme|

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration

| İşlem | Açıklama |
|---|---|
|/checkDomainAvailability/Action|Bir etki alanı satın almak için uygun olup olmadığını denetleyin|
|/ etki alanı/Sil|Varolan bir etki alanını silin.|
|/domains/domainownershipidentifiers/Delete|Sahipliği tanımlayıcısını sil|
|/Domains/domainownershipidentifiers/Read|Liste sahipliği tanımlayıcıları|
|/Domains/domainownershipidentifiers/Read|Sahipliği tanımlayıcısını alın|
|/domains/domainownershipidentifiers/Write|Tanımlayıcı güncelle|
|/Domains/operationresults/Read|Bir etki alanı işlemi Al|
|/Domains/Operations/Read|Uygulama hizmeti etki alanı kaydı tüm işlemler listesi|
|/ etki alanı/okuma|Etki alanlarının listesini alın|
|/ etki alanı/okuma|Etki alanı alma|
|/Domains/renew/Action|Mevcut bir etki alanına yenileyin.|
|/ etki alanı/yazma|Yeni bir etki alanı eklemek veya mevcut bir güncelleştirme|
|/generateSsoRequest/Action|Etki alanı denetim Merkezi'nde oturum için bir istek oluşturun.|
|/listDomainRecommendations/Action|Anahtar sözcüklere dayalı liste etki alanı önerileri Al|
|/register/action|Aboneliği için Microsoft Domains kaynak sağlayıcısı kaydetme|
|/topLevelDomains/listAgreements/Action|Anlaşma eylem listesi|
|/topLevelDomains/Read|TopLevel etki alanları Al|
|/topLevelDomains/Read|TopLevel etki alanı alma|
|/validateDomainRegistrationInformation/Action|Etki alanı satın alma nesnesi gönderme olmadan doğrula|

## <a name="microsoftdynamicslcs"></a>Microsoft.DynamicsLcs

| İşlem | Açıklama |
|---|---|
|/lcsprojects/clouddeployments/read|Microsoft Dynamics AX 2012 R3 değerlendirme dağıtımları bir kullanıcıya ait bir Microsoft Dynamics yaşam döngüsü Hizmetleri Proje görüntüler.|
|/lcsprojects/clouddeployments/write|Microsoft Dynamics AX 2012 R3 değerlendirme dağıtım bir kullanıcıya ait bir Microsoft Dynamics yaşam döngüsü Hizmetleri projesi oluşturun. Azure Yönetim Portalı'ndan dağıtımları yönetilebilir.|
|/lcsprojects/connectors/Read|Microsoft Dynamics yaşam döngüsü Hizmetleri projeye ait okuma bağlayıcılar|
|/lcsprojects/connectors/write|Oluşturma ve Microsoft Dynamics yaşam döngüsü Hizmetleri projeye ait bağlayıcılar güncelleştirme|
|/lcsprojects/delete|Kullanıcıya ait Microsoft Dynamics yaşam döngüsü Hizmetleri projeleri sil|
|/lcsprojects/Read|Bir kullanıcıya ait görüntü Microsoft Dynamics yaşam döngüsü Hizmetleri projeleri|
|/lcsprojects/write|Oluşturun ve kullanıcıya ait Microsoft Dynamics yaşam döngüsü Hizmetleri projeleri güncelleştirin. Yalnızca ad ve açıklama özellikleri güncelleştirilebilir. Oluşturulduktan sonra projeyle ilişkili konumu ve abonelik güncelleştirilemiyor|

## <a name="microsofteventgrid"></a>Microsoft.EventGrid

| İşlem | Açıklama |
|---|---|
|/eventSubscriptions/delete|Bir eventSubscription Sil|
|/eventSubscriptions/getFullUrl/action|Olay aboneliği için tam URL'sini alma|
|/eventSubscriptions/providers/Microsoft.Insights/diagnosticSettings/read|Olay abonelikleri tanılama ayarını alır|
|/eventSubscriptions/providers/Microsoft.Insights/diagnosticSettings/write|Oluşturur veya olay abonelikleri tanılama ayarını güncelleştirir|
|/eventSubscriptions/providers/Microsoft.Insights/metricDefinitions/read|EventSubscriptions için kullanılabilir ölçümleri alır|
|/eventSubscriptions/read|Bir eventSubscription okuma|
|/eventSubscriptions/write|Bir eventSubscription güncelle|
|/extensionTopics/providers/Microsoft.Insights/diagnosticSettings/read|Konular tanılama ayarını alır|
|/extensionTopics/providers/Microsoft.Insights/diagnosticSettings/write|Oluşturur veya konuları tanılama ayarını güncelleştirir|
|/extensionTopics/providers/Microsoft.Insights/metricDefinitions/read|Konular için kullanılabilir ölçümleri alır|
|/register/action|EventSubscription EventGrid kaynak sağlayıcısı için kaydeder ve olay kılavuz abonelikleri oluşturulmasını sağlar.|
|/topics/DELETE|Bir konu Sil|
|/topics/listKeys/action|Konu listesi tuşları|
|/topics/providers/Microsoft.Insights/diagnosticSettings/read|Konular tanılama ayarını alır|
|/topics/providers/Microsoft.Insights/diagnosticSettings/write|Oluşturur veya konuları tanılama ayarını güncelleştirir|
|/topics/providers/Microsoft.Insights/metricDefinitions/read|Konular için kullanılabilir ölçümleri alır|
|/topics/Read|Bir konuyu okuyun|
|/topics/regenerateKey/action|Konu için yeniden oluşturma anahtarı|
|/ konuları/yazma|Bir konu güncelle|

## <a name="microsofteventhub"></a>Microsoft.EventHub

| İşlem | Açıklama |
|---|---|
|/checkNameAvailability/action|İlgili abonelikte ad alanının kullanılabilirliğini denetler.|
|/checkNamespaceAvailability/action|İlgili abonelikte ad alanının kullanılabilirliğini denetler. Bu API kullanım Lütfen CheckNameAvailabiltiy kullanın.|
|/namespaces/authorizationRules/action|Güncelleştirmeleri Namespace yetkilendirme kuralı. Depricated API'dir. Namespace yetkilendirme kuralı yerine güncelleştirmek için lütfen PUT çağrı kullanın... API sürümü 2017-04-01 üzerinde bu işlem desteklenmiyor.|
|/namespaces/authorizationRules/delete|Namespace yetkilendirme kuralını silin. Namespace yetkilendirme varsayılan kural silinemiyor. |
|/namespaces/authorizationRules/listkeys/Action|Ad Alanı için Bağlantı Dizesini alın|
|/namespaces/authorizationRules/Read|Ad Alanı Yetkilendirme Kuralları açıklamasının listesini alır.|
|/namespaces/authorizationRules/regenerateKeys/action|Kaynağın Birincil veya İkincil anahtarını yeniden oluşturun|
|/namespaces/authorizationRules/write|Namespace düzeyi yetkilendirme kuralları oluşturun ve özelliklerini güncelleştirin. Yetkilendirme kuralları erişim hakları, birincil ve ikincil anahtarları güncelleştirilebilir.|
|/ ad alanları/Sil|Ad Alanı Kaynağını silin|
|/namespaces/disasterRecoveryConfigs/authorizationRules/listkeys/action|Yetkilendirme kuralları anahtarları olağanüstü durum kurtarma birincil ad alanı için alır|
|/namespaces/disasterRecoveryConfigs/authorizationRules/read|Olağanüstü durum kurtarma birincil Namespace'nın yetkilendirme kuralları Al|
|/namespaces/disasterRecoveryConfigs/breakPairing/action|Olağanüstü Durum Kurtarma'yı devre dışı bırakır ve birincil ad alanındaki değişiklikleri ikincil ad alanına çoğaltmayı durdurur.|
|/namespaces/disasterrecoveryconfigs/checkNameAvailability/action|Ad alanı diğer adı altında denetimleri kullanılabilirliğini aboneliği verilir.|
|/namespaces/disasterRecoveryConfigs/delete|Ad alanıyla ilişkili olağanüstü durum kurtarma yapılandırması siler. Bu işlem yalnızca birincil ad alanı çağrılabilir.|
|/namespaces/disasterRecoveryConfigs/failover/action|Bir GEO DR yük devretmesini çağırır ve ad alanı diğer adını ikincil ad alanına işaret edecek şekilde yeniden yapılandırır.|
|/namespaces/disasterRecoveryConfigs/read|Ad alanıyla ilişkili Olağanüstü Durum Kurtarma yapılandırmasını alır.|
|/namespaces/disasterRecoveryConfigs/write|Ad alanıyla ilişkili Olağanüstü Durum Kurtarma yapılandırmasını oluşturur veya güncelleştirir.|
|/namespaces/eventhubs/authorizationRules/action|EventHub güncelleştirmek için güncelleştirme işlemi. API sürümü 2017-04-01 üzerinde bu işlem desteklenmiyor. Yetkilendirme kuralları. Yetkilendirme kuralı güncelleştirmek için lütfen PUT çağrı kullanın.|
|/namespaces/eventhubs/authorizationRules/delete|EventHub yetkilendirme kuralları silmek için işlemi|
|/namespaces/eventhubs/authorizationRules/listkeys/action|EventHub bağlantı dizesi alma|
|/namespaces/eventhubs/authorizationRules/read| EventHub yetkilendirme kuralları listesini al|
|/namespaces/eventhubs/authorizationRules/regenerateKeys/action|Kaynağın Birincil veya İkincil anahtarını yeniden oluşturun|
|/namespaces/eventhubs/authorizationRules/write|EventHub yetkilendirme kuralları oluşturun ve özelliklerini güncelleştirin. Yetkilendirme kuralları erişim hakları güncelleştirilebilir.|
|/namespaces/eventHubs/consumergroups/Delete|ConsumerGroup kaynağı silme işlemi|
|/namespaces/eventHubs/consumergroups/read|ConsumerGroup kaynak açıklamaları listesini al|
|/namespaces/eventHubs/consumergroups/write|Oluşturma veya güncelleştirme ConsumerGroup özellikleri.|
|/namespaces/eventhubs/Delete|EventHub kaynağı silme işlemi|
|/namespaces/eventhubs/Read|EventHub kaynak açıklamaları listesini al|
|/namespaces/eventhubs/Write|Oluşturma veya güncelleştirme EventHub özellikleri.|
|/namespaces/messagingPlan/read|Mesajlaşma planı için bir ad alır. Bu API kullanım dışıdır. MessagingPlan kaynak kullanıma sunulan özellikler (üst) sonraki API sürümleri Namespace kaynağın taşınması... API sürümü 2017-04-01 üzerinde bu işlem desteklenmiyor.|
|/namespaces/messagingPlan/write|Mesajlaşma Plan için bir ad alanı güncelleştirir. Bu API kullanım dışıdır. MessagingPlan kaynak kullanıma sunulan özellikler (üst) sonraki API sürümleri Namespace kaynağın taşınması... API sürümü 2017-04-01 üzerinde bu işlem desteklenmiyor.|
|/namespaces/operationresults/Read|Ad alanı işleminin durumunu al|
|/namespaces/providers/Microsoft.Insights/diagnosticSettings/Read|Namespace tanılama ayarları kaynak açıklamaları listesini al|
|/namespaces/providers/Microsoft.Insights/diagnosticSettings/write|Namespace tanılama ayarları kaynak açıklamaları listesini al|
|/namespaces/providers/Microsoft.Insights/logDefinitions/read|Namespace günlükleri kaynak açıklamaları listesini al|
|/namespaces/providers/Microsoft.Insights/metricDefinitions/read|Namespace ölçümleri kaynak açıklamaları listesini al|
|/namespaces/Read|Ad Alanı Kaynak Açıklamasının listesini alır|
|/ ad alanları/yazma|Namespace kaynak oluşturun ve özelliklerini güncelleştirin. Etiketleri ve Namespace kapasitesini hangi güncelleştirilebilir özelliklerdir.|
|/Operations/Read|İşlemleri Al|
|/register/action|Aboneliği EventHub kaynak sağlayıcısı için kaydeder ve EventHub kaynaklarının oluşturulmasını sağlar|
|/SKU/Read|Sku kaynak açıklamaları listesini al|
|/sku/regions/read|SkuRegions kaynak açıklamaları listesini al|
|/unregister/action|EventHub Kaynak Sağlayıcısını kaydet|

## <a name="microsoftfeatures"></a>Microsoft.Features

| İşlem | Açıklama |
|---|---|
|/Features/Read|Bir aboneliğin özelliklerini alır.|
|/providers/Features/Read|Belirli bir kaynak sağlayıcısındaki bir abonelik özelliğini alır.|
|/providers/Features/Register/Action|Belirli bir kaynak sağlayıcısındaki bir abonelik özelliğini kaydeder.|
|/providers/Features/unregister/Action|Belirtilen bir kaynak sağlayıcısındaki bir abonelik özelliğinin kaydını siler.|

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

| İşlem | Açıklama |
|---|---|
|/clusters/changerdpsetting/action|Hdınsight kümesi RDP ayarını değiştirme|
|/clusters/configurations/action|Hdınsight küme yapılandırmasını güncelleştirme|
|/Clusters/configurations/Read|Hdınsight küme yapılandırmalarını alma|
|/Clusters/DELETE|Bir Hdınsight kümesi Sil|
|/clusters/providers/Microsoft.Insights/diagnosticSettings/read|Hdınsight kümesi kaynağın tanılama ayarını alır|
|/clusters/providers/Microsoft.Insights/diagnosticSettings/write|Oluşturur veya Hdınsight kümesi kaynağın tanılama ayarını güncelleştirir|
|/clusters/providers/Microsoft.Insights/metricDefinitions/read|Hdınsight kümesi için kullanılabilir ölçümleri alır|
|/Clusters/Read|Hdınsight kümesi hakkında bilgi almak|
|/clusters/roles/resize/action|Bir Hdınsight kümesi yeniden boyutlandırma|
|/ kümeleri/yazma|Hdınsight kümesi güncelle|
|/Locations/Capabilities/Read|Abonelik özelliklerini al|
|/locations/checkNameAvailability/read|Ad Kullanılabilirliğini Denetle|

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

| İşlem | Açıklama |
|---|---|
|/Jobs/DELETE|Var olan bir işi siler.|
|/jobs/listBitLockerKeys/action|Belirtilen iş için BitLocker anahtarları alır.|
|/Jobs/Read|Belirtilen iş için özellikleri alır veya işlerin listesini döndürür.|
|/ işleri/yazma|Belirtilen parametrelerle bir işi oluşturur veya özellikleri veya etiketleri belirtilen iş için güncelleştirin.|
|/Locations/Read|Belirtilen konum için özellikleri alır veya konumların listesini döndürür.|
|/register/action|İçeri/dışarı aktarma kaynak sağlayıcısı için aboneliği kaydeder ve içeri/dışarı aktarma işleri oluşturulmasını sağlar.|

## <a name="microsoftinsights"></a>Microsoft.Insights

| İşlem | Açıklama |
|---|---|
|/ActionGroups/Delete|Bir eylem grubu siliniyor|
|/ActionGroups/Read|Bir eylem grubu okunuyor|
|/ActionGroups/Write|Bir eylem grubu yazılıyor|
|/ActivityLogAlerts/Activated/Action|Etkinlik Günlüğü Uyarısı tetiklendi|
|/ActivityLogAlerts/Delete|Bir etkinlik günlüğü uyarısı siliniyor|
|/ActivityLogAlerts/Read|Bir etkinlik günlüğü uyarısı okunuyor|
|/ActivityLogAlerts/Write|Bir etkinlik günlüğü uyarısı okunuyor|
|/AlertRules/Activated/Action|Uyarı Kuralı etkinleştirildi|
|/AlertRules/Delete|Uyarı kuralı yapılandırması siliniyor|
|/AlertRules/Incidents/Read|Uyarı kuralı olayı yapılandırması okunuyor|
|/AlertRules/Read|Uyarı kuralı yapılandırması okunuyor|
|/AlertRules/Resolved/Action|Uyarı Kuralı çözümlendi|
|/AlertRules/Throttled/Action|Uyarı kuralı kısıtlanmış|
|/AlertRules/Write|Bir uyarı kuralı yapılandırmasına yazılıyor|
|/AutoscaleSettings/Delete|Otomatik ölçek ayarı yapılandırması siliniyor|
|/AutoscaleSettings/providers/Microsoft.Insights/MetricDefinitions/Read|Ölçüm tanımlarını oku|
|/AutoscaleSettings/Read|Otomatik ölçek ayarı yapılandırması okunuyor|
|/ AutoscaleSettings/Scaledown/eylem|Otomatik Ölçek ölçeği azaltma işlemi|
|/AutoscaleSettings/Scaleup/Action|Otomatik Ölçek ölçeği artırma işlemi|
|/AutoscaleSettings/Write|Otomatik ölçek ayarı yapılandırması yazılıyor|
|/Components/AnalyticsItems/Delete|Application Insights analytics öğe silme|
|/Components/AnalyticsItems/Read|Application Insights analytics öğeyi okuma|
|/Components/AnalyticsItems/Write|Application Insights analytics öğeyi yazma|
|/Components/AnalyticsTables/Action|Uygulama Öngörüler analytics tablo eylemi|
|/Components/AnalyticsTables/Delete|Application Insights silme analytics şema tablo|
|/Components/AnalyticsTables/Read|Application Insights okuma analytics şema tablo|
|/Components/AnalyticsTables/Write|Application Insights yazma analytics şema tablo|
|/Components/Annotations/Delete|Application Insights açıklamanın silme|
|/ Bileşenleri/ek açıklamaları/okuma|Application Insights açıklamanın okuma|
|/ Bileşenleri/ek açıklamaları/yazma|Application Insights açıklamanın yazma|
|/ Bileşenleri/Api/okuma|Application Insights bileşen verileri API okuma|
|/Components/ApiKeys/Action|Application Insights API'si anahtarı oluşturma|
|/Components/ApiKeys/Delete|Application Insights API'si anahtarı silme|
|/ Bileşenleri/ApiKeys/okuma|Application Insights API'si anahtarı okunuyor|
|/Components/BillingPlanForComponent/Read|Application Insights bileşeni için bir faturalandırma planı okuma|
|/ Bileşenleri/CurrentBillingFeatures/okuma|Application Insights bileşeni için geçerli fatura özelliklerini okuma|
|/Components/CurrentBillingFeatures/Write|Application Insights bileşeni için geçerli fatura özelliklerini yazma|
|/Components/DefaultWorkItemConfig/Read|Bir Application Insights varsayılan ALM tümleştirme yapılandırmasını okuma|
|/Components/Delete|Bir Application Insights bileşen yapılandırmasını silme|
|/Components/ExportConfiguration/Action|Application Insights ayarları eylem dışarı aktarma|
|/Components/ExportConfiguration/Delete|Silme Application Insights ayarlarını dışa aktarma|
|/ Bileşenleri/ExportConfiguration/okuma|Okuma Application Insights ayarlarını dışa aktarma|
|/Components/ExportConfiguration/Write|Yazma Application Insights ayarlarını dışa aktarma|
|/ Bileşenleri/ExtendQueries/okuma|Okuma Application Insights bileşen extended sorgu sonuçları|
|/ Bileşenleri/Sık Kullanılanlar/silme|Application Insights sık kullanılan silme|
|/ Bileşenleri/Sık Kullanılanlar/okuma|Application Insights sık kullanılan okuma|
|/ Bileşenleri/Sık Kullanılanlar/yazma|Application Insights sık kullanılan yazma|
|/ Bileşenleri/FeatureCapabilities/okuma|Okuma Application Insights bileşeni özellikleri|
|/Components/GetAvailableBillingFeatures/Read|Okuma Application Insights bileşeni kullanılabilir fatura özellikleri|
|/Components/GetToken/Read|Bir Application Insights bileşen belirteci okuma|
|/Components/ListMigrationDate/Action|Get geri abonelik geçiş tarihi|
|/Components/ListMigrationDate/Read|Get geri abonelik geçiş tarihi|
|/ Bileşenleri/MetricDefinitions/okuma|Application Insights bileşen ölçüm tanımlarını okuma|
|/ Bileşenleri/ölçümleri/okuma|Okuma Application Insights bileşen ölçümleri|
|/Components/MigrateToNewpricingModel/Action|Yeni fiyatlandırma modeli için abonelik geçirme|
|/Components/Move/Action|Bir uygulama Öngörüler bileşeni başka bir kaynak grubuna veya aboneliğe taşıma|
|/Components/MyAnalyticsItems/Delete|Application Insights kişisel analytics öğe silme|
|/Components/MyAnalyticsItems/Read|Application Insights kişisel analytics öğeyi okuma|
|/Components/MyAnalyticsItems/Write|Application Insights kişisel analytics öğeyi yazma|
|/Components/MyFavorites/Read|Application Insights kişisel sık kullanılan okuma|
|/ Bileşenleri/PricingPlans/okuma|Plan fiyatlandırması bir Application Insights bileşeni okuma|
|/Components/PricingPlans/Write|Plan fiyatlandırması bir Application Insights bileşen yazma|
|/Components/ProactiveDetectionConfigs/Read|Okuma Application Insights öngörülü algılama yapılandırması|
|/Components/ProactiveDetectionConfigs/Write|Application Insights öngörülü algılama yapılandırması yazma|
|/Components/providers/Microsoft.Insights/MetricDefinitions/Read|Ölçüm tanımlarını oku|
|/ Bileşenleri/QuotaStatus/okuma|Application Insights bileşen kota durumunu okuma|
|/ Bileşenleri/okuma|Bir Application Insights bileşen yapılandırmasını okuma|
|/Components/RollbackToLegacyPricingModel/Action|Fiyatlandırma modeli eski aboneliğine geri alma|
|/ Bileşenleri/SyntheticMonitorLocations/okuma|Okuma Application Insights Web testine konumları|
|/Components/WorkItemConfigs/Delete|Bir uygulama Öngörüler ALM tümleştirme yapılandırması siliniyor|
|/Components/WorkItemConfigs/Read|Bir uygulama Öngörüler ALM tümleştirme yapılandırmasını okuma|
|/Components/WorkItemConfigs/Write|Bir uygulama Öngörüler ALM tümleştirme yapılandırması yazma|
|/ Bileşenleri/yazma|Bir Application Insights bileşen yapılandırmasına yazma|
|/DiagnosticSettings/Delete|Tanılama ayarları yapılandırmasını silme|
|/ DiagnosticSettings/okuma|Tanılama ayarları yapılandırmasını okuma|
|/DiagnosticSettings/Write|Tanılama ayarları yapılandırmasına yazma|
|/ EventCategories/okuma|Bir etkinlik kategorisi okunuyor|
|/eventtypes/digestevents/Read|Yönetim olayı türü özetini oku|
|/eventtypes/Values/Read|Yönetim olayı tür değerlerini oku|
|/ExtendedDiagnosticSettings/Delete|Genişletilmiş tanı ayarları yapılandırmasını silme|
|/ ExtendedDiagnosticSettings/okuma|Genişletilmiş tanı ayarları yapılandırmasını okuma|
|/ExtendedDiagnosticSettings/Write|Genişletilmiş tanı ayarları yapılandırmasına yazma|
|/ LogDefinitions/okuma|Günlük tanımlarını oku|
|/LogProfiles/Delete|Günlük profilleri yapılandırmasını silme|
|/LogProfiles/Read|Günlük profillerini okuma|
|/LogProfiles/Write|Bir günlük profili yapılandırmasına yazma|
|/MetricAlerts/Delete|Bir ölçüm uyarısı siliniyor|
|/ MetricAlerts/okuma|Bir ölçüm uyarısı okunuyor|
|/MetricAlerts/Write|Bir ölçüm uyarısı yazılıyor|
|/MetricDefinitions/Microsoft.Insights/Read|Ölçüm tanımlarını oku|
|/MetricDefinitions/providers/Microsoft.Insights/Read|Ölçüm tanımlarını oku|
|/ MetricDefinitions/okuma|Ölçüm tanımlarını oku|
|/ Ölçümleri/sağlayıcıları/ölçümleri/okuma|Ölçümleri okuma|
|/ Ölçümleri/okuma|Ölçümleri okuma|
|/ Ölçümleri/yazma|Ölçümleri yazma|
|/ Operations/okuma|İşlemler okunuyor|
|/ Kayıt/eylem|Microsoft Insights sağlayıcısını kaydedin|
|/ Kiracılar/kayıt/eylem|Microsoft Insights sağlayıcısını başlatır|
|/ Kaydı/eylem|Microsoft Insights sağlayıcısını kaydedin|
|/Webtests/Delete|Bir web testi yapılandırmasını silme|
|/Webtests/GetToken/Read|Bir Web testine belirteci okuma|
|/Webtests/MetricDefinitions/Read|Bir Web testine ölçüm tanımlarını okuma|
|/ Webtests/ölçümleri/okuma|Bir Web testine ölçümleri okuma|
|/ Webtests/okuma|Bir web testi yapılandırmasını okuma|
|/ Webtests/yazma|Bir web testi yapılandırmasına yazma|

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

| İşlem | Açıklama |
|---|---|
|/checkNameAvailability/read|Bir anahtar kasası adının geçerli ve kullanımda olup olmadığını denetler|
|/deletedVaults/read|Geçici silinen anahtar kasalarının özelliklerini görüntüleyin|
|/hsmPools/delete|Bir HSM havuzunu silin|
|/hsmPools/joinVault/action|Bir HSM havuzuna anahtar kasası ekleyin|
|/hsmPools/read|Bir HSM havuzunun özelliklerini görüntüleyin|
|/hsmPools/write|Yeni bir HSM havuzu oluşturun veya mevcut bir HSM havuzunun özelliklerini güncelleştirin|
|/locations/deletedVaults/purge/action|Geçici silinen bir anahtar kasasını temizleyin|
|/locations/deletedVaults/read|Geçici silinen bir anahtar kasasının özelliklerini görüntüleyin|
|/locations/deleteVirtualNetworkOrSubnets/action|Microsoft.KeyVault'a bir sanal ağın veya alt ağın silindiğini bildirir|
|/Locations/operationResults/Read|Uzun süre çalışan bir işlemin sonucunu denetleyin|
|/Operations/Read|Microsoft.KeyVault kaynak sağlayıcısındaki kullanılabilir işlemleri listele|
|/register/action|Bir aboneliği kaydeder|
|/unregister/action|Abonelik kaydını siler|
|/vaults/accessPolicies/write|Var olan bir erişim ilkesini, birleştirme veya değiştirme yoluyla güncelleştirin ya da kasaya yeni bir erişim ilkesi ekleyin.|
|/vaults/DELETE|Bir anahtar kasasını silme|
|/vaults/deploy/action|Azure kaynakları dağıtılırken bir anahtar kasasındaki gizli dizilere erişim sağlar|
|/vaults/providers/Microsoft.Insights/diagnosticSettings/Read|Kaynak için tanılama ayarını alır|
|/vaults/providers/Microsoft.Insights/diagnosticSettings/Write|Kaynak için tanılama ayarını oluşturur veya güncelleştirir|
|/vaults/providers/Microsoft.Insights/logDefinitions/read|Bir anahtar kasası için kullanılabilir günlükleri alır|
|/vaults/providers/Microsoft.Insights/metricDefinitions/read|Bir anahtar kasası için kullanılabilir ölçümleri alır|
|/vaults/Read|Anahtar kasasının özelliklerini görüntüleyin|
|/vaults/Secrets/Read|Bir gizli dizinin özelliklerini (değeri hariç) görüntüleyin|
|/vaults/Secrets/Write|Yeni bir gizli dizi oluşturun veya var olan bir gizli dizinin değerini güncelleştirin|
|/vaults/write|Yeni bir anahtar kasası oluşturun veya var olan bir anahtar kasasının özelliklerini güncelleştirin|

## <a name="microsoftlabservices"></a>Microsoft.LabServices

| İşlem | Açıklama |
|---|---|
|/labAccounts/CreateLab/action|Bir laboratuvar bir laboratuvar hesabı oluşturun.|
|/labAccounts/delete|Laboratuvar hesapları silin.|
|/labAccounts/labs/delete|Labs silin.|
|/labAccounts/labs/environmentSettings/delete|Ortam ayarı silin.|
|/labAccounts/labs/environmentSettings/environments/delete|Ortamlar silin.|
|/labAccounts/Labs/environmentSettings/Environments/Read|Ortamlar okuyun.|
|/labAccounts/labs/environmentSettings/environments/write|Ekleyin veya ortamlar değiştirin.|
|/labAccounts/labs/environmentSettings/Publish/action|Hükümler/deprovisions kaynakları ayarlama ortamı için laboratuvar/ortam ayarı geçerli durumuna göre gereklidir.|
|/labAccounts/Labs/environmentSettings/Read|Ortam ayarı okuyun.|
|/labAccounts/labs/environmentSettings/write|Ekleyin veya ortam ayarını değiştirin.|
|/labAccounts/Labs/Read|Labs okuyun.|
|/labAccounts/Labs/Users/DELETE|Kullanıcıların silin.|
|/labAccounts/Labs/Users/Read|Kullanıcılar okuyun.|
|/labAccounts/Labs/Users/Write|Ekleyin veya kullanıcıları değiştirin.|
|/labAccounts/Labs/Write|Ekleyin veya labs değiştirin.|
|/labAccounts/Read|Laboratuvar hesapları okuyun.|
|/labAccounts/write|Ekleyin veya Laboratuvar hesapları değiştirin.|
|/Locations/Operations/Read|Okuma işlemleri.|
|/register/action|Aboneliği kaydeder|

## <a name="microsoftlocationbasedservices"></a>Microsoft.LocationBasedServices

| İşlem | Açıklama |
|---|---|
|/Accounts/DELETE|Temel bir konumu silmek hizmetleri hesabı.|
|/accounts/listKeys/action|Konum tabanlı hizmetleri hesabı anahtarlarını Listele|
|/accounts/providers/Microsoft.Insights/diagnosticSettings/read|Kaynak için tanılama ayarını alır|
|/accounts/providers/Microsoft.Insights/diagnosticSettings/write|Kaynak için tanılama ayarını oluşturur veya güncelleştirir|
|/accounts/providers/Microsoft.Insights/metricDefinitions/read|Konum tabanlı Hizmetleri hesapları için kullanılabilir ölçümleri alır|
|/Accounts/Read|Bağlı bir konum alma hizmetleri hesabı.|
|/accounts/regenerateKey/action|Yeni konum tabanlı hizmetleri hesabı birincil veya ikincil anahtarı oluştur|
|/ hesapları/yazma|Oluşturun veya bir konum tabanlı hizmetleri hesabı güncelleştirin.|
|/register/action|Sağlayıcıyı kaydettirin|

## <a name="microsoftlogic"></a>Microsoft.Logic

| İşlem | Açıklama |
|---|---|
|/integrationAccounts/agreements/delete|Tümleştirme hesabını anlaşmasında siler.|
|/integrationAccounts/agreements/listContentCallbackUrl/action|Geri çağırma URL'si tümleştirme hesap sözleşmesi içeriği alır.|
|/integrationAccounts/agreements/read|Tümleştirme hesabını anlaşmasında okur.|
|/integrationAccounts/agreements/write|Oluşturur veya tümleştirme hesabını anlaşmasında güncelleştirir.|
|/integrationAccounts/assemblies/delete|Bütünleştirilmiş kodunda tümleştirme hesabını siler.|
|/integrationAccounts/assemblies/listContentCallbackUrl/action|Tümleştirme hesabındaki derleme içerik için geri çağırma URL'si alır.|
|/integrationAccounts/assemblies/read|Tümleştirme hesabını derlemede okur.|
|/integrationAccounts/assemblies/write|Oluşturur veya tümleştirme hesabını derlemede güncelleştirir.|
|/integrationAccounts/batchConfigurations/delete|Tümleştirme hesabını toplu yapılandırmasında siler.|
|/integrationAccounts/batchConfigurations/read|Tümleştirme hesabını toplu yapılandırmasında okur.|
|/integrationAccounts/batchConfigurations/write|Oluşturur veya tümleştirme hesabındaki toplu yapılandırmasını güncelleştirir.|
|/integrationAccounts/certificates/delete|Tümleştirme hesabındaki sertifikasını siler.|
|/integrationAccounts/certificates/read|Tümleştirme hesabını sertifikada okur.|
|/integrationAccounts/certificates/write|Oluşturur veya tümleştirme hesabını sertifikada güncelleştirir.|
|/integrationAccounts/delete|Tümleştirme hesabını siler.|
|/integrationAccounts/listCallbackUrl/action|Geri çağırma URL'si için tümleştirme hesabı alır.|
|/integrationAccounts/listKeyVaultKeys/action|Anahtar kasasına anahtarları alır.|
|/integrationAccounts/logTrackingEvents/action|İzleme olayları tümleştirme hesabında kaydeder.|
|/integrationAccounts/maps/delete|Tümleştirme hesabını eşlemesinde siler.|
|/integrationAccounts/maps/listContentCallbackUrl/action|Tümleştirme hesabındaki harita içerik için geri çağırma URL'si alır.|
|/integrationAccounts/maps/read|Tümleştirme hesabını eşlemesinde okur.|
|/integrationAccounts/maps/write|Oluşturur veya tümleştirme hesabını eşlemesinde güncelleştirir.|
|/integrationAccounts/partners/delete|Tümleştirme hesap ortağında siler.|
|/integrationAccounts/partners/listContentCallbackUrl/action|Geri çağırma URL'si tümleştirme hesap ortağı içeriği alır.|
|/integrationAccounts/Partners/Read|Parter tümleştirme hesabında okur.|
|/integrationAccounts/Partners/Write|Oluşturur veya tümleştirme hesap ortağında güncelleştirir.|
|/integrationAccounts/providers/Microsoft.Insights/logDefinitions/read|Tümleştirme Hesabı günlük tanımlarını okur.|
|/integrationAccounts/read|Tümleştirme hesabını okur.|
|/integrationAccounts/regenerateAccessKey/action|Erişim anahtarı parolalarını yeniden oluşturur.|
|/integrationAccounts/schemas/delete|Tümleştirme hesabını şemada siler.|
|/integrationAccounts/schemas/listContentCallbackUrl/action|Geri çağırma URL'si tümleştirme hesabını şema içeriği alır.|
|/integrationAccounts/schemas/read|Tümleştirme hesabını şemada okur.|
|/integrationAccounts/schemas/write|Oluşturur veya tümleştirme hesabını şemada güncelleştirir.|
|/integrationAccounts/sessions/delete|Tümleştirme hesabını oturumunda siler.|
|/integrationAccounts/sessions/read|Tümleştirme hesabını toplu yapılandırmasında okur.|
|/integrationAccounts/sessions/write|Oluşturur veya tümleştirme hesabını oturumunda güncelleştirir.|
|/integrationAccounts/write|Hesabı oluşturur veya tümleştirme güncelleştirir.|
|/locations/workflows/validate/action|İş akışını doğrular.|
|/Operations/Read|İşlemi alır.|
|/register/action|Belirli bir aboneliğe Microsoft.Logic kaynak sağlayıcısını kaydeder.|
|/workflows/accessKeys/delete|Erişim anahtarını siler.|
|/workflows/accessKeys/list/action|Erişim anahtarı parolalarını listeler.|
|/workflows/accessKeys/read|Erişim anahtarını okur.|
|/workflows/accessKeys/regenerate/action|Erişim anahtarı parolalarını yeniden oluşturur.|
|/workflows/accessKeys/write|Erişim anahtarı oluşturur veya güncelleştirir.|
|/workflows/DELETE|İş akışını siler.|
|/workflows/disable/Action|İş akışını devre dışı bırakır.|
|/workflows/Enable/Action|İş akışını etkinleştirir.|
|/workflows/listCallbackUrl/action|İş akışı için geri çağrıma URL'sini alır.|
|/workflows/listSwagger/action|İş akışı swagger tanımlarını alır.|
|/workflows/Move/Action|İş Akışını mevcut abonelik kimliği, kaynak grubu ve/veya adından farklı bir abonelik kimliğine, kaynak grubuna ve/veya ada taşır.|
|/workflows/providers/Microsoft.Insights/diagnosticSettings/read|İş akışı tanılama ayarlarını okur.|
|/workflows/providers/Microsoft.Insights/diagnosticSettings/write|İş akışı tanılama ayarını oluşturur veya güncelleştirir.|
|/workflows/providers/Microsoft.Insights/logDefinitions/read|İş akışı günlüğü tanımlarını okur.|
|/workflows/providers/Microsoft.Insights/metricDefinitions/read|İş akışı ölçüm tanımlarını okur.|
|/workflows/Read|İş akışını okur.|
|/workflows/regenerateAccessKey/action|Erişim anahtarı parolalarını yeniden oluşturur.|
|/workflows/run/action|İş akışının çalıştırılmasını başlatır.|
|/workflows/runs/actions/listExpressionTraces/action|İş akışı çalıştırma eylemi ifade izlemelerini alır.|
|/workflows/Runs/Actions/Read|İş akışı çalıştırma eylemini okur.|
|/workflows/runs/actions/repetitions/listExpressionTraces/action|Eylem yineleme ifade izlemeleri çalıştırma iş akışı alır.|
|/workflows/Runs/Actions/repetitions/Read|Eylem yineleme çalıştırma iş akışını okur.|
|/workflows/Runs/Actions/scoperepetitions/Read|Eylem kapsamı yineleme çalıştırma iş akışını okur.|
|/workflows/Runs/Cancel/Action|İş akışının çalıştırılmasını iptal eder.|
|/workflows/Runs/Operations/Read|İş akışı çalıştırma işlemi durumunu okur.|
|/ İş akışları/çalışan/okuma|İş akışı çalıştırmasını okur.|
|/workflows/suspend/Action|İş akışını askıya alır.|
|/workflows/Triggers/histories/Read|Tetikleyici geçmişlerini okur.|
|/workflows/triggers/histories/resubmit/action|İş akışı tetikleyicisini yeniden gönderir.|
|/workflows/triggers/listCallbackUrl/action|Tetikleyici için geri arama URL'sini alır.|
|/workflows/Triggers/Read|Tetikleyiciyi okur.|
|/workflows/triggers/reset/action|Tetikleyici sıfırlar.|
|/workflows/triggers/run/action|Tetikleyiciyi yürütür.|
|/workflows/triggers/setState/action|Tetikleyici durumunu ayarlar.|
|/workflows/validate/action|İş akışını doğrular.|
|/workflows/versions/read|İş akışı sürümünü okur.|
|/workflows/versions/triggers/listCallbackUrl/action|Tetikleyici için geri arama URL'sini alır.|
|/ İş akışları/yazma|İş akışını oluşturur veya güncelleştirir.|

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

| İşlem | Açıklama |
|---|---|
|/commitmentPlans/commitmentAssociations/Move/Action|Taahhüt Plan ilişkilendirme öğrenme herhangi bir makineye taşıma|
|/commitmentPlans/commitmentAssociations/Read|Taahhüt Plan ilişkilendirme öğrenme herhangi bir makineye okuma|
|/commitmentPlans/delete|Taahhüt Plan öğrenme herhangi bir makineye Sil|
|/commitmentPlans/join/action|Taahhüt Plan öğrenme herhangi bir makineye katılma|
|/commitmentPlans/Read|Taahhüt Plan öğrenme herhangi bir makineye okuma|
|/commitmentPlans/write|Herhangi bir Machine Learning taahhüt planının güncelle|
|/Locations/operationresults/Read|Machine Learning işleminin sonucu alın|
|/Locations/operationsstatus/Read|Devam eden bir Machine Learning işlem durumunu Al|
|/Operations/Read|Machine Learning işlemleri Al|
|/register/action|Machine learning web hizmeti kaynak sağlayıcısı için aboneliği kaydeder ve web hizmetleri oluşturulmasını sağlar.|
|/skus/Read|Machine Learning taahhüt Plan SKU'ları alma|
|/webServices/action|Desteklenen bölgeler için bölgesel Web hizmeti özellikleri oluşturma|
|/webServices/delete|Bir Machine Learning Web hizmeti Sil|
|/webServices/read|Bir Machine Learning Web hizmeti okuma|
|/webServices/write|Bir Machine Learning Web hizmeti güncelle|
|/ Workspaces/silme|Bir Machine Learning çalışma alanı silme|
|/ Workspaces/listworkspacekeys/eylem|Bir Machine Learning çalışma alanı listesi tuşları|
|/ Workspaces/okuma|Çalışma alanı öğrenme herhangi bir makineye okuma|
|/Workspaces/resyncstoragekeys/action|Machine Learning çalışma alanı için yapılandırılan depolama hesabı anahtarlarını yeniden eşitleme|
|/ Workspaces/yazma|Çalışma alanı öğrenme makine oluşturulamıyor veya güncelleştirilemiyor|

## <a name="microsoftmachinelearningcompute"></a>Microsoft.MachineLearningCompute

| İşlem | Açıklama |
|---|---|
|/operationalizationClusters/checkUpdate/action|Güncelleştirmeleri Sistem Hizmetleri operationalization küme için kullanılabilir olup olmadığını denetleyin|
|/operationalizationClusters/delete|Herhangi bir barındırma hesabı silme|
|/operationalizationClusters/listKeys/action|Operationalization kümesi ile ilişkili tuşlarını listesi|
|/operationalizationClusters/Read|Herhangi bir barındırma hesabı okuma|
|/operationalizationClusters/updateSystem/action|Güncelleştirme operationalization kümedeki Sistem Hizmetleri|
|/operationalizationClusters/write|Herhangi bir barındırma hesabı güncelle|
|/register/action|Abonelik kimliği kaynak sağlayıcısına kaydeder ve işlem kaynaklarını öğrenme bir makineye oluşturulmasını sağlar|

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement

| İşlem | Açıklama |
|---|---|
|/Accounts/DELETE|Herhangi bir barındırma hesabı silme|
|/Accounts/Read|Herhangi bir barındırma hesabı okuma|
|/ hesapları/yazma|Herhangi bir barındırma hesabı güncelle|
|/register/action|Abonelik kimliği kaynak sağlayıcısına kaydeder ve barındırma hesabı oluşturma sağlar|

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity

| İşlem | Açıklama |
|---|---|
|/userAssignedIdentities/Assign/Action|Varolan bir kullanıcı atamak için RBAC eylem kimliği için bir kaynak atanır.|
|/userAssignedIdentities/DELETE|Mevcut bir kullanıcı kimliği atanır siler|
|/userAssignedIdentities/Read|Mevcut bir kullanıcı kimliği atanır alır|
|/ userAssignedIdentities/yazma|Yeni bir kullanıcı kimliği atanır oluşturur veya mevcut bir kullanıcı kimliği atanır ile ilişkili etiketleri güncelleştirir|

## <a name="microsoftmanagedlab"></a>Microsoft.ManagedLab

| İşlem | Açıklama |
|---|---|
|/labAccounts/CreateLab/action|Bir laboratuvar bir laboratuvar hesabı oluşturun.|
|/labAccounts/delete|Laboratuvar hesapları silin.|
|/labAccounts/labs/delete|Labs silin.|
|/labAccounts/labs/environmentSettings/delete|Ortam ayarı silin.|
|/labAccounts/labs/environmentSettings/environments/delete|Ortamlar silin.|
|/labAccounts/Labs/environmentSettings/Environments/Read|Ortamlar okuyun.|
|/labAccounts/labs/environmentSettings/environments/write|Ekleyin veya ortamlar değiştirin.|
|/labAccounts/Labs/environmentSettings/Read|Ortam ayarı okuyun.|
|/labAccounts/labs/environmentSettings/write|Ekleyin veya ortam ayarını değiştirin.|
|/labAccounts/labs/labVms/delete|Laboratuvar sanal makineleri silin.|
|/labAccounts/Labs/labVms/Read|Laboratuvar sanal makineleri okuyun.|
|/labAccounts/Labs/labVms/Write|Ekleyin veya Laboratuvar sanal makineleri değiştirin.|
|/labAccounts/Labs/Read|Labs okuyun.|
|/labAccounts/Labs/Write|Ekleyin veya labs değiştirin.|
|/labAccounts/Read|Laboratuvar hesapları okuyun.|
|/labAccounts/write|Ekleyin veya Laboratuvar hesapları değiştirin.|
|/Locations/Operations/Read|Okuma işlemleri.|
|/register/action|Aboneliği kaydeder|

## <a name="microsoftmanagement"></a>Microsoft.Management

| İşlem | Açıklama |
|---|---|
|/checkNameAvailability/action|Belirtilen yönetim grubu adı geçerli ve benzersiz olup olmadığını denetler.|
|/ getEntities/eylem|Kimliği doğrulanmış kullanıcı için tüm varlıkları (Yönetim grupları, abonelikler, vb.) listeler.|
|/managementGroups/delete|Yönetim grubunu silin.|
|/managementGroups/read|Kimliği doğrulanmış kullanıcı için Yönetim grupları listesi.|
|/managementGroups/subscriptions/delete|Yönetim grubu abonelikten XML'deki ilişkilendirir.|
|/managementGroups/subscriptions/write|Yönetim grubu abonelikle varolan ilişkilendirir.|
|/managementGroups/write|Oluşturun veya bir yönetim grubu güncelleştirin.|

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps

| İşlem | Açıklama |
|---|---|
|/ClassicDevServices/delete|Klasik geliştirme hizmeti kaynak üzerinde bir silme işlemi yapar.|
|/ClassicDevServices/listSecrets/action|Klasik geliştirme hizmeti kaynak yönetimi anahtarları alır.|
|/ClassicDevServices/listSingleSignOnToken/action|Üzerinde tek oturum URL'si için bir Klasik geliştirme hizmeti alır.|
|/ClassicDevServices/read|Klasik geliştirme hizmeti üzerinde bir GET işlemi yapar.|
|/ClassicDevServices/regenerateKey/action|Klasik geliştirme hizmeti kaynak yönetimi anahtarları oluşturur.|
|/ Operations/okuma|Tüm kaynak türleri için işlemleri okuyun.|

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceOrdering

| İşlem | Açıklama |
|---|---|
|/agreements/Offers/plans/Cancel/Action|Verilen Market öğesi için bir sözleşmeyi iptal et|
|/agreements/Offers/plans/Read|Verilen Market öğesi için bir anlaşma Döndür|
|/agreements/Offers/plans/Sign/Action|Bir anlaşma verilen Market öğesi için oturum açın|
|/agreements/Read|Tüm anlaşmalar altında dönüş abonelik verilen|
|/offertypes/publishers/offers/plans/agreements/read|Verilen Market sanal makine öğesi için bir anlaşma Al|
|/offertypes/publishers/offers/plans/agreements/write|Oturum veya verilen Market sanal makine öğesi için bir sözleşmeyi iptal et|

## <a name="microsoftmedia"></a>Microsoft.Media

| İşlem | Açıklama |
|---|---|
|/checknameavailability/action|Bir Media Services hesabı adı olup olmadığını denetler|
|/mediaservices/delete|Herhangi bir Media Services hesabı silme|
|/mediaservices/listKeys/action|Media Services hesabı için ACS anahtarlarını Listele|
|/mediaservices/read|Herhangi bir Media Services hesabı okuma|
|/mediaservices/regenerateKey/action|Medya Hizmetleri ACS anahtarını yeniden oluşturma|
|/mediaservices/syncStorageKeys/action|Ekli bir Azure depolama hesabı için depolama anahtarları Eşitle|
|/mediaservices/write|Herhangi bir Media Services hesabı güncelle|
|/Operations/Read|Herhangi bir Media Services hesabı okuma|
|/register/action|Media Services kaynak sağlayıcısı için aboneliği kaydeder ve Media Services hesapları oluşturmanıza olanak tanıyan|

## <a name="microsoftmigrate"></a>Microsoft.Migrate

| İşlem | Açıklama |
|---|---|
|/ Operations/okuma|Açığa çıkarılan işlemleri okur|

## <a name="microsoftnetwork"></a>Microsoft.Network

| İşlem | Açıklama |
|---|---|
|/applicationGatewayAvailableSslOptions/predefinedPolicies/read|Uygulama ağ geçidi Ssl İlkesi önceden tanımlanmış|
|/applicationGatewayAvailableSslOptions/read|Uygulama ağ geçidi kullanılabilir Ssl seçenekleri|
|/applicationGatewayAvailableWafRuleSets/read|Uygulama ağ geçidi kullanılabilir Waf kural kümesi alır|
|/applicationGateways/backendAddressPools/join/action|Bir uygulama ağ geçidi arka uç adres havuzuna katılır|
|/applicationGateways/backendhealth/action|Bir uygulama ağ geçidi arka uç durumunu alır|
|/applicationGateways/delete|Bir uygulama ağ geçidini siler|
|/applicationGateways/effectiveNetworkSecurityGroups/action|Uygulama ağ geçidi üzerinde yapılandırılmış yönlendirme tablosunu Al|
|/applicationGateways/effectiveRouteTable/action|Uygulama ağ geçidi üzerinde yapılandırılmış yönlendirme tablosunu Al|
|/applicationGateways/providers/Microsoft.Insights/logDefinitions/read|Uygulama ağ geçidi olaylarını alır|
|/applicationGateways/providers/Microsoft.Insights/metricDefinitions/read|Uygulama ağ geçidi için kullanılabilir ölçümleri alır|
|/applicationGateways/read|Bir uygulama ağ geçidini alır|
|/applicationGateways/setSecurityCenterConfiguration/action|Ayarlar uygulama ağ geçidi Güvenlik Merkezi yapılandırma|
|/applicationGateways/start/action|Bir uygulama ağ geçidi başlatır|
|/applicationGateways/stop/action|Bir uygulama ağ geçidi durdurur|
|/applicationGateways/write|Bir uygulama ağ geçidi oluşturur veya bir uygulama ağ geçidini güncelleştirir|
|/applicationSecurityGroups/delete|Bir uygulama güvenlik grubunu siler|
|/applicationSecurityGroups/joinIpConfiguration/action|Bir IP yapılandırması uygulama güvenlik gruplarına birleştirir.|
|/applicationSecurityGroups/joinNetworkSecurityRule/action|Güvenlik kuralı uygulama güvenlik gruplarına birleştirir.|
|/applicationSecurityGroups/read|Bir uygulama güvenlik grubu kimliği alır.|
|/applicationSecurityGroups/write|Uygulama güvenlik grubu oluşturur veya mevcut bir uygulama güvenlik grubuna güncelleştirir.|
|/bgpServiceCommunities/read|BGP hizmet toplulukları Al|
|/checkTrafficManagerNameAvailability/action|Trafik Yöneticisi göreli DNS adı kullanılabilirliğini denetler.|
|/Connections/DELETE|Deletes VirtualNetworkGatewayConnection|
|/Connections/Read|Gets VirtualNetworkGatewayConnection|
|/Connections/sharedkey/Action|Get VirtualNetworkGatewayConnection SharedKey|
|/connections/sharedKey/read|Gets VirtualNetworkGatewayConnection SharedKey|
|/connections/sharedKey/write|Oluşturur veya mevcut bir ConnectionType(hypernet/routebased) türündeki VirtualNetworkGatewayConnection SharedKey güncelleştirir|
|/connections/vpndeviceconfigurationscript/read|ConnectionType(hypernet/routebased) türündeki VirtualNetworkGatewayConnection VPN cihazı yapılandırmasını alır|
|/ bağlantıları/yazma|Oluşturur veya mevcut bir ConnectionType(hypernet/routebased) türündeki VirtualNetworkGatewayConnection güncelleştirir|
|/ddosProtectionPlans/ddosProtectionPlanProxies/delete|DDoS koruması planı Proxy siler|
|/ddosProtectionPlans/ddosProtectionPlanProxies/read|DDoS koruması planlama Proxy tanımını alır|
|/ddosProtectionPlans/ddosProtectionPlanProxies/write|DDoS koruması planlama bir Proxy veya güncelleştirmeleri ve varolan DDoS koruması planlama Proxy oluşturur|
|/ddosProtectionPlans/delete|DDoS koruması planı siler|
|/ddosProtectionPlans/join/action|DDoS koruması planı birleştirir|
|/ddosProtectionPlans/read|DDoS koruması planı alır|
|/ddosProtectionPlans/write|DDoS koruması Plan oluşturur veya DDoS koruması Plan güncelleştirir |
|/dnsoperationresults/read|DNS İşlem sonuçlarını alır|
|/dnsoperationstatuses/Read|Bir DNS işlemin durumunu alır |
|/dnszones/A/delete|Bir verilen ad kayıt kümesini kaldırın ve DNS bölgesinden ' A' yazın.|
|/dnszones/A/Read|'A' türündeki kayıt kümesini JSON biçiminde alın. Kayıt kümesi kayıtları yanı sıra TTL, etiketler ve etag'in listesini içerir.|
|/dnszones/A/write|Oluşturun veya bir DNS bölgesi içinde 'A' türündeki bir kayıt kümesini güncelleştirin. Belirtilen kayıtlar kayıt kümesindeki geçerli kayıt yerini alır.|
|/dnszones/AAAA/delete|Bir verilen ad kayıt kümesini kaldırın ve DNS bölgesinden ' AAAA' yazın.|
|/dnszones/AAAA/read|'AAAA' türündeki kayıt kümesini JSON biçiminde alın. Kayıt kümesi kayıtları yanı sıra TTL, etiketler ve etag'in listesini içerir.|
|/dnszones/AAAA/write|Oluşturun veya bir DNS bölgesi içinde 'AAAA' türündeki bir kayıt kümesini güncelleştirin. Belirtilen kayıtlar kayıt kümesindeki geçerli kayıt yerini alır.|
|/dnszones/all/Read|DNS kayıt kümelerini türleri arasında alır|
|/dnszones/CAA/delete|Bir verilen ad kayıt kümesini kaldırın ve DNS bölgesinden ' CAA' yazın.|
|/dnszones/CAA/read|'CAA' türündeki kayıt kümesini JSON biçiminde alın. Kayıt kümesi TTL, etiketler ve etag'in içerir.|
|/dnszones/CAA/write|Oluşturun veya bir DNS bölgesi içinde 'CAA' türündeki bir kayıt kümesini güncelleştirin. Belirtilen kayıtlar kayıt kümesindeki geçerli kayıt yerini alır.|
|/dnszones/CNAME/delete|Bir verilen ad kayıt kümesini kaldırın ve DNS bölgesinden ' CNAME' yazın.|
|/dnszones/CNAME/Read|'CNAME' türündeki kayıt kümesini JSON biçiminde alın. Kayıt kümesi TTL, etiketler ve etag'in içerir.|
|/dnszones/CNAME/write|Oluşturun veya bir DNS bölgesi içinde 'CNAME' türündeki bir kayıt kümesini güncelleştirin. Belirtilen kayıtlar kayıt kümesindeki geçerli kayıt yerini alır.|
|/dnszones/delete|JSON biçimindeki DNS bölgesini silin. Etiketler, etag, numberOfRecordSets ve maxNumberOfRecordSets bölge özellikleri içerir.|
|/dnszones/MX/delete|Bir verilen ad kayıt kümesini kaldırın ve DNS bölgesinden ' MX' yazın.|
|/dnszones/MX/read|'MX' türündeki kayıt kümesini JSON biçiminde alın. Kayıt kümesi kayıtları yanı sıra TTL, etiketler ve etag'in listesini içerir.|
|/dnszones/MX/write|Oluşturun veya bir DNS bölgesi içinde 'MX' türündeki bir kayıt kümesini güncelleştirin. Belirtilen kayıtlar kayıt kümesindeki geçerli kayıt yerini alır.|
|/dnszones/NS/delete|NS türündeki DNS kayıt kümesini siler|
|/dnszones/NS/Read|DNS NS türündeki kayıt kümesini alır|
|/dnszones/NS/write|Oluşturur veya DNS NS türündeki kayıt kümesini güncelleştirir|
|/dnszones/providers/Microsoft.Insights/diagnosticSettings/read|DNS bölgesi tanılama ayarlarını alır|
|/dnszones/providers/Microsoft.Insights/diagnosticSettings/write|Oluşturur veya DNS bölgesi tanılama ayarlarını güncelleştirir|
|/dnszones/providers/Microsoft.Insights/metricDefinitions/read|DNS bölgesi ölçüm tanımlarını alır|
|/dnszones/PTR/delete|Bir verilen ad kayıt kümesini kaldırın ve DNS bölgesinden ' PTR' yazın.|
|/dnszones/PTR/read|'PTR' türündeki kayıt kümesini JSON biçiminde alın. Kayıt kümesi kayıtları yanı sıra TTL, etiketler ve etag'in listesini içerir.|
|/dnszones/PTR/write|Oluşturun veya bir DNS bölgesi içinde 'PTR' türündeki bir kayıt kümesini güncelleştirin. Belirtilen kayıtlar kayıt kümesindeki geçerli kayıt yerini alır.|
|/dnszones/Read|JSON biçimindeki DNS bölgesini alın. Etiketler, etag, numberOfRecordSets ve maxNumberOfRecordSets bölge özellikleri içerir. Bu komutun bölge içindeki kayıt kümelerini almıyorsa unutmayın.|
|/dnszones/Recordsets/Read|DNS kayıt kümelerini türleri arasında alır|
|/dnszones/SOA/read|DNS SOA türündeki kayıt kümesini alır|
|/dnszones/SOA/write|Oluşturur veya DNS SOA türündeki kayıt kümesini güncelleştirir|
|/dnszones/SRV/delete|Bir verilen ad kayıt kümesini kaldırın ve DNS bölgesinden ' SRV' yazın.|
|/dnszones/SRV/Read|'SRV' türündeki kayıt kümesini JSON biçiminde alın. Kayıt kümesi kayıtları yanı sıra TTL, etiketler ve etag'in listesini içerir.|
|/dnszones/SRV/write|SRV türündeki kayıt kümesini oluştur veya güncelleştir|
|/dnszones/TXT/delete|Bir verilen ad kayıt kümesini kaldırın ve DNS bölgesinden ' TXT' yazın.|
|/dnszones/TXT/read|'TXT' türündeki kayıt kümesini JSON biçiminde alın. Kayıt kümesi kayıtları yanı sıra TTL, etiketler ve etag'in listesini içerir.|
|/dnszones/TXT/write|Oluşturun veya bir DNS bölgesi içinde 'TXT' türündeki bir kayıt kümesini güncelleştirin. Belirtilen kayıtlar kayıt kümesindeki geçerli kayıt yerini alır.|
|/dnszones/write|Oluşturun veya bir kaynak grubunda DNS bölgesi güncelleştirin.  Bir DNS bölgesi kaynağındaki etiketleri güncelleştirmek için kullanılır. Bu komut oluşturmak veya bölge içindeki kayıt kümelerini güncelleştirmek için kullanılamaz olduğunu unutmayın.|
|/expressRouteCircuits/authorizations/DELETE|ExpressRouteCircuit yetkilendirme siler|
|/expressRouteCircuits/authorizations/Read|ExpressRouteCircuit yetkilendirme alır|
|/expressRouteCircuits/authorizations/Write|Oluşturur veya mevcut bir ExpressRouteCircuit yetkilendirme güncelleştirir|
|/expressRouteCircuits/delete|Bir ExpressRouteCircuit siler|
|/expressRouteCircuits/peerings/arpTables/action|Bir ExpressRouteCircuit eşliği ArpTable alır|
|/expressRouteCircuits/peerings/Connections/DELETE|Bir ExpressRouteCircuit bağlantısını siler|
|/expressRouteCircuits/peerings/Connections/Read|Bir ExpressRouteCircuit bağlantı alır|
|/expressRouteCircuits/peerings/Connections/Write|Oluşturur veya mevcut bir ExpressRouteCircuit bağlantı kaynağı güncelleştirir|
|/expressRouteCircuits/peerings/DELETE|Bir ExpressRouteCircuit eşliği siler|
|/expressRouteCircuits/peerings/Read|Bir ExpressRouteCircuit eşliği alır|
|/expressRouteCircuits/peerings/routeTables/action|Bir ExpressRouteCircuit eşliği RouteTable alır|
|/expressRouteCircuits/peerings/routeTablesSummary/Action|Bir ExpressRouteCircuit eşliği RouteTable özeti alır|
|/expressRouteCircuits/peerings/stats/Read|Bir ExpressRouteCircuit çubuklu eşliği alır|
|/expressRouteCircuits/peerings/Write|Oluşturur veya mevcut bir eşleme ExpressRouteCircuit güncelleştirir|
|/expressRouteCircuits/providers/Microsoft.Insights/diagnosticSettings/read|ExpressRoute bağlantı hatları için tanılama ayarlarını alır|
|/expressRouteCircuits/providers/Microsoft.Insights/diagnosticSettings/write|Oluşturur veya ExpressRoute bağlantı hatları için tanılama ayarları güncelleştirir|
|/expressRouteCircuits/providers/Microsoft.Insights/logDefinitions/read|ExpressRoute bağlantı hatları için olayları Al|
|/expressRouteCircuits/providers/Microsoft.Insights/metricDefinitions/read|ExpressRoute bağlantı hatları için ölçüm tanımlarını alır|
|/expressRouteCircuits/read|Bir ExpressRouteCircuit Al|
|/expressRouteCircuits/stats/Read|Bir ExpressRouteCircuit Stat alır|
|/expressRouteCircuits/write|Oluşturur veya mevcut bir ExpressRouteCircuit güncelleştirir|
|/expressRouteCrossConnections/delete|Hızlı rota silme arası bağlantı|
|/expressRouteCrossConnections/join/action|Birleştirmeler hızlı rota bir bağlantıdan|
|/expressRouteCrossConnections/peerings/arpTables/action|Hızlı Rota bağlantısı eşliği Arp tablosu çapraz alır|
|/expressRouteCrossConnections/peerings/delete|Bağlantı eşliği arası bir hızlı rota siler|
|/expressRouteCrossConnections/peerings/read|Bağlantı eşliği arası bir hızlı rota alır|
|/expressRouteCrossConnections/peerings/routeTables/action|Hızlı Rota bağlantısı eşliği yol tablosu çapraz alır|
|/expressRouteCrossConnections/peerings/routeTableSummary/action|Hızlı Rota bağlantısı eşliği rota tablosu özeti çapraz alır|
|/expressRouteCrossConnections/peerings/stats/read|Bağlantı çubuklu eşliği arası bir hızlı rota alır|
|/expressRouteCrossConnections/peerings/write|Bir hızlı rota arası bağlantı eşlemesi oluşturur veya bir var olan Hızlı rota arası bağlantı eşlemesi güncelleştirir|
|/expressRouteCrossConnections/read|Hızlı rota alma arası bağlantı|
|/expressRouteCrossConnections/write|Oluşturma veya güncelleştirme hızlı rota arası bağlantı|
|/expressRouteServiceProviders/read|Hızlı rota hizmeti sağlayıcılarını alır|
|/loadBalancers/backendAddressPools/join/action|Bir yük dengeleyici arka uç adres havuzuna katılır|
|/loadBalancers/backendAddressPools/read|Yük Dengeleyici arka uç adres havuzu tanımını alır|
|/loadBalancers/delete|Bir yük dengeleyici siler|
|/loadBalancers/frontendIPConfigurations/read|Yük Dengeleyici ön uç IP yapılandırması tanımını alır|
|/loadBalancers/inboundNatPools/join/action|Katıldığı bir yük dengeleyici gelen nat havuzu|
|/loadBalancers/inboundNatPools/read|Bir yük dengeleyici alır gelen nat havuzu tanımını|
|/loadBalancers/inboundNatRules/delete|Yük Dengeleyici gelen nat kuralını siler|
|/loadBalancers/inboundNatRules/join/action|Bir yük dengeleyici gelen nat kuralına katılır|
|/loadBalancers/inboundNatRules/read|Bir yük dengeleyici alır gelen nat kuralı tanımını|
|/loadBalancers/inboundNatRules/write|Yük Dengeleyici gelen nat kuralı oluşturur veya mevcut bir yük dengeleyici gelen nat kuralını güncelleştirir|
|/loadBalancers/loadBalancingRules/read|Yük Dengeleyici Yük Dengeleme kuralı tanımını alır|
|/loadBalancers/networkInterfaces/read|Bir yük dengeleyici altındaki tüm ağ arabirimlerine başvuruları alır|
|/loadBalancers/outboundNatRules/read|Yük Dengeleyici giden nat kuralı tanımını alır|
|/loadBalancers/probes/join/action|Bir yük dengeleyicinin araştırmalar kullanarak sağlar. Örneğin, VM ölçek bu izni healthProbe özelliği ile kümesi araştırma başvuruda bulunabilir.|
|/loadBalancers/probes/read|Yük Dengeleyici araştırmasını alır|
|/loadBalancers/providers/Microsoft.Insights/diagnosticSettings/read|Yük Dengeleyici tanılama ayarlarını alır|
|/loadBalancers/providers/Microsoft.Insights/diagnosticSettings/write|Oluşturur veya yük dengeleyici tanılama ayarlarını güncelleştirir|
|/loadBalancers/providers/Microsoft.Insights/logDefinitions/read|Yük Dengeleyici için olayları alır|
|/loadBalancers/providers/Microsoft.Insights/metricDefinitions/read|Yük Dengeleyici için kullanılabilir ölçümleri alır|
|/loadBalancers/Read|Yük Dengeleyici tanımını alır|
|/loadBalancers/virtualMachines/read|Bir yük dengeleyici altındaki tüm sanal makinelere başvuruları alır|
|/loadBalancers/write|Bir yük dengeleyici oluşturur veya mevcut bir Yük Dengeleyiciyi güncelleştirir|
|/localnetworkgateways/delete|Deletes LocalNetworkGateway|
|/localnetworkgateways/read|Gets LocalNetworkGateway|
|/localnetworkgateways/write|Oluşturur veya mevcut bir LocalNetworkGateway güncelleştirir|
|/locations/checkDnsNameAvailability/read|DNS etiketi belirtilen konumda kullanılabilir olup olmadığını denetler|
|/Locations/operationResults/Read|Bir zaman uyumsuz POST veya DELETE işleminin işlem sonucunu alır|
|/Locations/Operations/Read|Zaman uyumsuz bir işlemin durumunu gösteren işlem kaynağını alır|
|/Locations/usages/Read|Kaynak kullanımı ölçümlerini alır|
|/locations/virtualNetworkAvailableEndpointServices/read|Kullanılabilir sanal ağ uç noktası hizmetlerin bir listesini alır|
|/networkInterfaces/delete|Bir ağ arabirimi siler|
|/networkInterfaces/diagnosticIdentity/read|Kaynağın tanılama kimliğini alır|
|/networkInterfaces/effectiveNetworkSecurityGroups/action|Ağ güvenlik grupları yapılandırılmış üzerinde ağ arabirimi, Vm Al|
|/networkInterfaces/effectiveRouteTable/action|Vm ağ arabiriminde yapılandırılan yönlendirme tablosunu Al|
|/networkInterfaces/ipconfigurations/read|Ağ arabirimi IP yapılandırması tanımını alır. |
|/networkInterfaces/join/action|Bir sanal makine bir ağ arabirimiyle birleştirir|
|/networkInterfaces/loadBalancers/read|Ağ arabiriminin parçası olduğu tüm yük dengeleyicileri alır|
|/networkInterfaces/providers/Microsoft.Insights/metricDefinitions/read|Ağ arabirimi için kullanılabilir ölçümleri alır|
|/networkInterfaces/read|Ağ arabirimi tanımını alır. |
|/networkInterfaces/write|Bir ağ arabirimi oluşturur veya mevcut bir ağ arabirimini güncelleştirir. |
|/networkSecurityGroups/defaultSecurityRules/read|Varsayılan güvenlik kuralı tanımını alır|
|/networkSecurityGroups/delete|Ağ güvenlik grubunu siler|
|/networkSecurityGroups/join/action|Ağ güvenlik grubuna katılır|
|/networksecuritygroups/providers/Microsoft.Insights/diagnosticSettings/read|Ağ güvenlik grupları tanılama ayarlarını alır|
|/networksecuritygroups/providers/Microsoft.Insights/diagnosticSettings/write|Oluşturur veya güncelleştirir ağ güvenlik grupları tanılama ayarları, bu işlem ınsights kaynak sağlayıcı tarafından olur.|
|/networksecuritygroups/providers/Microsoft.Insights/logDefinitions/read|Ağ güvenlik grubu için olayları alır|
|/networkSecurityGroups/read|Ağ güvenlik grubu tanımını alır|
|/networkSecurityGroups/securityRules/delete|Bir güvenlik kuralını siler|
|/networkSecurityGroups/securityRules/read|Güvenlik kuralı tanımını alır|
|/networkSecurityGroups/securityRules/write|Bir güvenlik kuralı oluşturur veya mevcut bir güvenlik kuralını güncelleştirir|
|/networkSecurityGroups/write|Bir ağ güvenlik grubu oluşturur veya mevcut bir ağ güvenlik grubunu güncelleştirir|
|/networkWatchers/availableProvidersList/action|Tüm kullanılabilir internet hizmet sağlayıcıları belirtilen bir Azure bölgesinin döndürür.|
|/networkWatchers/azureReachabilityReport/action|Göreli gecikme puan için internet hizmet sağlayıcıları Azure bölgeler ile belirtilen konumdan döndürür.|
|/networkWatchers/configureFlowLog/action|Akış günlüğü hedef kaynak için yapılandırır.|
|/networkWatchers/connectionMonitors/delete|Bağlantı İzleyici siler|
|/networkWatchers/connectionMonitors/providers/Microsoft.Insights/ diagnosticSettings/okuma|Bağlantı İzleyici tanılama ayarlarını al|
|/networkWatchers/connectionMonitors/providers/Microsoft.Insights/ diagnosticSettings/write|Oluşturur veya bağlantı İzleyici tanılama ayarlarını güncelleştirir|
|/networkWatchers/connectionMonitors/providers/Microsoft.Insights/ metricDefinitions/okuma|Bağlantı izleme için kullanılabilir ölçümleri alır|
|/networkWatchers/connectionMonitors/query/action|Belirtilen uç noktaları arasında bağlantı izleme sorgu|
|/networkWatchers/connectionMonitors/read|Bağlantı İzleyici ayrıntıları|
|/networkWatchers/connectionMonitors/start/action|Belirtilen uç noktaları arasında bağlantı İzlemeyi Başlat|
|/networkWatchers/connectionMonitors/stop/action|Belirtilen uç noktaları arasında bağlantı izleme durdurma/Duraklat|
|/networkWatchers/connectionMonitors/write|Bağlantı İzleyici oluşturur|
|/networkWatchers/connectivityCheck/action|Doğrudan TCP bağlantısı bir sanal makineden başka bir VM veya isteğe bağlı bir uzak sunucunun dahil olmak üzere belirli bir uç nokta oluşturma olasılığını doğrular.|
|/networkWatchers/delete|Ağ İzleyicisi siler|
|/networkWatchers/ipFlowVerify/action|Paket izin verilen veya için veya belirli bir hedef reddedildi döndürür.|
|/networkWatchers/lenses/delete|Odakta siler|
|/networkWatchers/lenses/query/action|Sorgu belirtilen bir uç noktada ağ trafiğini izleme|
|/networkWatchers/lenses/read|Mercek ayrıntıları|
|/networkWatchers/lenses/start/action|Belirtilen bir uç noktada ağ trafiğini izleme Başlat|
|/networkWatchers/lenses/stop/action|Belirtilen bir uç noktada ağ trafiğini izleme durdurma/Duraklat|
|/networkWatchers/lenses/write|Odakta oluşturur|
|/networkWatchers/nextHop/action|Belirtilen hedef ve hedef IP adresi, sonraki atlama türü dönün ve sonraki IP adresi umuyoruz.|
|/networkWatchers/packetCaptures/delete|Paket yakalama siler|
|/networkWatchers/packetCaptures/queryStatus/action|Özellikleri ve paket yakalama kaynak durumu hakkındaki bilgileri alır.|
|/networkWatchers/packetCaptures/read|Paket yakalama tanımını Al|
|/networkWatchers/packetCaptures/stop/action|Çalışan paket yakalama oturumunu durdurun.|
|/networkWatchers/packetCaptures/write|Paket yakalama oluşturur|
|/networkWatchers/queryFlowLogStatus/action|Akış bir kaynakta oturum durumunu alır.|
|/networkWatchers/queryTroubleshootResult/action|Sorun giderme sonucun daha önce çalışan veya şu anda çalışan sorun giderme işlemi alır.|
|/networkWatchers/read|Ağ İzleyicisi tanımını Al|
|/networkWatchers/securityGroupView/action|Bir VM üzerinde uygulanan yapılandırılmış ve etkili ağ güvenlik grubu kural görüntüleyin.|
|/networkWatchers/topology/action|Ağ düzeyinde görünümünü kaynakları ve ilişkilerini bir kaynak grubunda alır.|
|/networkWatchers/troubleshoot/action|Bir ağ kaynağına Azure sorun giderme başlar.|
|/networkWatchers/write|Ağ İzleyicisi oluşturur veya mevcut bir Ağ İzleyicisi'ni güncelleştirir|
|/Operations/Read|Kullanılabilir işlemleri Al|
|/publicIPAddresses/delete|Bir ortak IP adresini siler.|
|/publicIPAddresses/join/action|Bir genel ip adresine katılır|
|/publicIPAddresses/providers/Microsoft.Insights/diagnosticSettings/read|Ortak IP adresi tanılama ayarlarını al|
|/publicIPAddresses/providers/Microsoft.Insights/diagnosticSettings/write|Oluşturma veya genel IP adresi tanılama ayarlarını güncelleştirme|
|/publicIPAddresses/providers/Microsoft.Insights/logDefinitions/read|Ortak IP adresinin günlüğü tanımlarını Al|
|/publicIPAddresses/providers/Microsoft.Insights/metricDefinitions/read|Ortak IP adresinin ölçümleri tanımlarını Al|
|/publicIPAddresses/read|Bir genel ip adresi tanımını alır.|
|/publicIPAddresses/write|Bir ortak IP adresi oluşturur veya mevcut bir ortak IP adresini güncelleştirir. |
|/register/action|Aboneliği kaydeder|
|/routeFilters/delete|Bir rota filtre tanımını siler|
|/routeFilters/join/action|Bir rota filtre birleştirir|
|/routeFilters/read|Bir rota filtre tanımını alır|
|/routeFilters/routeFilterRules/delete|Bir rota filtre kuralı tanımını siler|
|/routeFilters/routeFilterRules/read|Bir rota filtre kuralı tanımını alır|
|/routeFilters/routeFilterRules/write|Bir rota filtre kuralı oluşturur veya mevcut bir rota filtre kuralını güncelleştirir|
|/routeFilters/write|Bir rota filtre oluşturur veya varolan bir yol filtreyi güncelleştirir|
|/routeTables/delete|Yol tablosu tanımını siler|
|/routeTables/join/action|Bir yol tablosu birleştirir|
|/routeTables/read|Yol tablosu tanımını alır|
|/routeTables/routes/delete|Bir rota tanımını siler|
|/routeTables/Routes/Read|Bir rota tanımını alır|
|/routeTables/routes/write|Bir yol oluşturur veya mevcut bir yolu güncelleştirir|
|/routeTables/write|Bir yol tablosu oluşturur veya mevcut bir yol tablosunu güncelleştirir|
|/securegateways/applicationRuleCollections/delete|Bir uygulama kural koleksiyonu için güvenli bir ağ geçidi siler|
|/securegateways/applicationRuleCollections/read|Verilen güvenli ağ geçidi için bir uygulama kuralı koleksiyonu alma|
|/securegateways/applicationRuleCollections/write|Oluşturur veya güvenli bir ağ geçidi için bir uygulama kuralı koleksiyonu güncelleştirir|
|/securegateways/DELETE|Güvenli ağ geçidini Sil|
|/securegateways/networkRuleCollections/delete|Bir ağ kural koleksiyonu için güvenli bir ağ geçidi siler|
|/securegateways/networkRuleCollections/read|Verilen güvenli ağ geçidi için bir ağ kural koleksiyonu alma|
|/securegateways/networkRuleCollections/write|Oluşturur veya bir ağ kural koleksiyonu için güvenli bir ağ geçidi güncelleştirir|
|/securegateways/Read|Güvenli ağ geçidi Al|
|/ securegateways/yazma|Oluşturur veya güvenli bir ağ geçidi güncelleştirir|
|/serviceEndpointPolicies/delete|Hizmet uç noktası İlkesi siler|
|/serviceEndpointPolicies/join/action|Hizmet uç noktası İlkesi birleştirir|
|/serviceEndpointPolicies/joinSubnet/action|Bir alt ağ hizmeti uç noktası ilkeleri birleştirir|
|/serviceEndpointPolicies/read|Hizmet uç noktası İlkesi açıklamasını alır|
|/serviceEndpointPolicies/serviceEndpointPolicyDefinitions/delete|Hizmet uç noktası ilke tanımını siler|
|/serviceEndpointPolicies/serviceEndpointPolicyDefinitions/read|Bir hizmet uç noktası ilke tanımı tanımı alır|
|/serviceEndpointPolicies/serviceEndpointPolicyDefinitions/write|Bir hizmet uç noktası ilke tanımı oluşturur veya mevcut bir hizmet uç noktası ilke tanımı güncelleştirmeleri|
|/serviceEndpointPolicies/write|Hizmet uç noktası ilkesi oluşturur veya mevcut bir hizmet uç noktası İlkesi güncelleştirir|
|/trafficManagerGeographicHierarchies/read|Trafik Yöneticisi Geographic coğrafi trafik yönlendirme yöntemini ile kullanılabilen bölgeler içeren hiyerarşisi alır|
|/trafficManagerProfiles/azureEndpoints/delete|Azure uç noktası var olan bir Traffic Manager profilden siler. Trafik Yöneticisi yönlendirme trafiği Azure uç noktası silindi durdurur.|
|/trafficManagerProfiles/azureEndpoints/read|Bir Azure Traffic Manager bu Azure uç tüm özelliklerini de dahil olmak üzere bir profiline ait uç nokta alır.|
|/trafficManagerProfiles/azureEndpoints/write|Var olan bir Traffic Manager profilini içinde yeni bir Azure uç noktası ekleyin veya mevcut bir Azure uç noktası bu trafik Yöneticisi profili içinde özelliklerini güncelleştirin.|
|/trafficManagerProfiles/delete|Trafik Yöneticisi profilini silin. Trafik Yöneticisi profili ile ilişkilendirilmiş tüm ayarlar kaybedilir ve profil artık trafiği yönlendirmek için kullanılabilir.|
|/trafficManagerProfiles/externalEndpoints/delete|Dış uç noktası var olan bir Traffic Manager profilden siler. Trafik Yöneticisi uç noktası silindi dış yönlendirme trafik durur.|
|/trafficManagerProfiles/externalEndpoints/read|Dış bir trafik Yöneticisi bu dış uç tüm özelliklerini de dahil olmak üzere profiline ait olan uç noktası alır.|
|/trafficManagerProfiles/externalEndpoints/write|Var olan bir Traffic Manager profilini içinde yeni bir dış uç noktası ekleyin veya mevcut bir dış uç noktası bu trafik Yöneticisi profili içinde özelliklerini güncelleştirin.|
|/trafficManagerProfiles/heatMaps/read|Trafik Yöneticisi ısı Haritası konumunu ve kaynak IP sorgu sayısı ve gecikme verileri içeren verilen trafik Yöneticisi profili alır.|
|/trafficManagerProfiles/nestedEndpoints/delete|İç içe geçmiş bir uç nokta var olan bir Traffic Manager profilden siler. Trafik Yöneticisi yönlendirme trafiğini silinen iç içe geçmiş uç durdurur.|
|/trafficManagerProfiles/nestedEndpoints/read|Bir iç içe trafik Yöneticisi, iç içe geçmiş Endpoint tüm özelliklerini de dahil olmak üzere bir profiline ait uç nokta alır.|
|/trafficManagerProfiles/nestedEndpoints/write|Var olan bir Traffic Manager profilini içinde yeni bir iç içe geçmiş uç noktası ekleyin veya mevcut bir iç içe geçmiş uç noktası bu trafik Yöneticisi profili içinde özelliklerini güncelleştirin.|
|/trafficManagerProfiles/providers/Microsoft.Insights/diagnosticSettings/read|Trafik Yöneticisi tanılama ayarlarını alır|
|/trafficManagerProfiles/providers/Microsoft.Insights/diagnosticSettings/write|Oluşturur veya güncelleştirir trafik Yöneticisi tanılama ayarları, bu işlem ınsights kaynak sağlayıcı tarafından ınsights olur.|
|/trafficManagerProfiles/providers/Microsoft.Insights/logDefinitions/read|Trafik Yöneticisi için olayları alır|
|/trafficManagerProfiles/providers/Microsoft.Insights/metricDefinitions/read|Trafik Yöneticisi için kullanılabilir ölçümleri alır.|
|/trafficManagerProfiles/read|Trafik Yöneticisi Profil yapılandırmasını alın. Bu, DNS ayarlarını, trafik yönlendirme ayarlarını, uç nokta izleme ayarlarını ve bu trafik Yöneticisi profili tarafından yönlendirilen uç noktaların listesini içerir.|
|/trafficManagerProfiles/write|Trafik Yöneticisi profili oluşturun veya var olan bir Traffic Manager profilini yapılandırmasını değiştirin. Bu, etkinleştirme veya bir profili devre dışı bırakma ve DNS ayarlarını, trafik yönlendirme ayarlarını veya uç nokta izleme ayarlarını değiştirme içerir. Trafik Yöneticisi profili tarafından yönlendirilen uç noktaların eklenmiş, kaldırılmış, devre dışı veya etkinleştirilebilir.|
|/trafficManagerUserMetricsKeys/delete|Gerçek zamanlı kullanıcı ölçümleri koleksiyonu için kullanılan abonelik düzeyinde anahtarı siler.|
|/trafficManagerUserMetricsKeys/read|Gerçek zamanlı kullanıcı ölçümleri koleksiyonu için kullanılan abonelik düzeyinde anahtarını alır.|
|/trafficManagerUserMetricsKeys/write|Gerçek zamanlı kullanıcı ölçümleri koleksiyonu için kullanılacak yeni bir abonelik düzeyi anahtar oluşturur.|
|/unregister/action|Abonelik kaydını siler|
|/virtualHubs/delete|Sanal bir hub'ı siler|
|/virtualHubs/hubVirtualNetworkConnections/delete|Bir HubVirtualNetworkConnection siler|
|/virtualHubs/hubVirtualNetworkConnections/read|Bir HubVirtualNetworkConnection Al|
|/virtualHubs/hubVirtualNetworkConnections/write|Bir HubVirtualNetworkConnection güncelle|
|/virtualHubs/read|Sanal bir hub'ı alın|
|/virtualHubs/write|Oluşturma veya sanal bir hub'ı güncelleştirme|
|/virtualnetworkgateways/connections/read|Get VirtualNetworkGatewayConnection|
|/virtualNetworkGateways/delete|Bir virtualNetworkGateway siler|
|/virtualnetworkgateways/generatevpnclientpackage/action|VirtualNetworkGateway için VpnClient paketi oluştur|
|/virtualnetworkgateways/generatevpnprofile/action|VirtualNetworkGateway için VpnProfile paketi oluştur|
|/virtualnetworkgateways/getadvertisedroutes/action|Yollar alır virtualNetworkGateway tanıtılan|
|/virtualnetworkgateways/getbgppeerstatus/action|VirtualNetworkGateway bgp eş durumunu alır|
|/virtualnetworkgateways/getlearnedroutes/action|Virtualnetworkgateway öğrenilen rotalar alır|
|/virtualnetworkgateways/getvpnclientipsecparameters/action|Vpnclient IPSec parametreleri için VirtualNetworkGateway P2S istemcisi alın.|
|/virtualnetworkgateways/getvpnprofilepackageurl/action|Önceden oluşturulmuş vpn istemci profilini paketi URL'sini alır|
|/virtualNetworkGateways/providers/Microsoft.Insights/diagnosticSettings/read|Sanal ağ geçidi tanılama ayarlarını alır|
|/virtualNetworkGateways/providers/Microsoft.Insights/diagnosticSettings/write|Oluşturur veya güncelleştirir sanal ağ geçidi tanılama ayarları, bu işlem ınsights kaynak sağlayıcı tarafından ınsights olur.|
|/virtualNetworkGateways/providers/Microsoft.Insights/logDefinitions/read|Sanal ağ geçidi için olayları alır|
|/virtualNetworkGateways/providers/Microsoft.Insights/metricDefinitions/read|Sanal ağ geçidi için kullanılabilir ölçümleri alır|
|/virtualNetworkGateways/read|Bir VirtualNetworkGateway alır|
|/virtualnetworkgateways/reset/action|Bir virtualNetworkGateway sıfırlar|
|/virtualnetworkgateways/setvpnclientipsecparameters/action|Vpnclient IPSec VirtualNetworkGateway P2S istemci parametrelerini ayarlayın.|
|/virtualnetworkgateways/supportedvpndevices/action|Vpn cihazları listeler desteklenir|
|/virtualNetworkGateways/write|Oluşturur veya bir VirtualNetworkGateway güncelleştirir|
|/virtualNetworks/checkIpAddressAvailability/read|Belirtilen sanal ağ IP adresi olup olmadığını denetleyin|
|/virtualNetworks/customViews/get/action|Bir sanal ağ özel görünüm içeriğini alma|
|/virtualNetworks/customViews/read|Sanal ağ özel görünüm tanımını Al|
|/virtualNetworks/delete|Bir sanal ağ siler|
|/virtualNetworks/Peer/Action|Başka bir sanal ağ ile sanal ağ eşleri|
|/virtualNetworks/providers/Microsoft.Insights/diagnosticSettings/read|Sanal Ağ Tanılama ayarlarını al|
|/virtualNetworks/providers/Microsoft.Insights/diagnosticSettings/write|Oluşturma veya sanal ağ tanılama ayarlarını güncelleştirme|
|/virtualNetworks/providers/Microsoft.Insights/logDefinitions/read|Sanal ağ günlüğü tanımlarını Al|
|/virtualNetworks/providers/Microsoft.Insights/metricDefinitions/read|Sanal ağ ölçüm tanımlarını Al|
|/virtualNetworks/Read|Sanal ağ tanımını Al|
|/virtualNetworks/remoteVirtualNetworkPeeringProxies/delete|Bir sanal ağ eşleme proxy siler|
|/virtualNetworks/remoteVirtualNetworkPeeringProxies/read|Bir sanal ağ proxy tanımı eşliği alır|
|/virtualNetworks/remoteVirtualNetworkPeeringProxies/write|Bir sanal ağ eşleme proxy oluşturur veya mevcut bir sanal ağ eşleme proxy güncelleştirir|
|/virtualNetworks/subnets/delete|Bir sanal ağ alt ağını siler|
|/virtualNetworks/subnets/join/action|Bir sanal ağ birleştirir|
|/virtualNetworks/subnets/joinViaServiceEndpoint/action|Kaynak depolama hesabı veya SQL veritabanı gibi bir alt ağa birleştirir.|
|/virtualNetworks/Subnets/Read|Bir sanal ağ alt ağı tanımını alır|
|/virtualNetworks/subnets/resourceNavigationLinks/delete|Bir kaynak Gezinti bağlantısını siler|
|/virtualNetworks/subnets/resourceNavigationLinks/read|Kaynak Gezinti bağlantısı tanımı Al|
|/virtualNetworks/subnets/resourceNavigationLinks/write|Bir kaynak Gezinti bağlantısı oluşturur veya varolan bir kaynak Gezinti bağlantısına güncelleştirir|
|/virtualNetworks/subnets/virtualMachines/read|Bir sanal ağ alt ağında tüm sanal makinelere başvuruları alır|
|/virtualNetworks/subnets/write|Bir sanal ağ alt ağı oluşturur veya varolan bir sanal ağ alt ağı güncelleştirir|
|/virtualNetworks/taggedTrafficConsumers/delete|Etiketli trafiği tüketici siler|
|/virtualNetworks/taggedTrafficConsumers/read|Etiketli trafiği tüketici tanımını Al|
|/virtualNetworks/taggedTrafficConsumers/validate/action|Etiketli trafiği tüketici doğrular|
|/virtualNetworks/taggedTrafficConsumers/write|Etiketli trafiği tüketici oluşturur veya mevcut bir etiketli trafiği tüketici güncelleştirir|
|/virtualNetworks/usages/Read|Her sanal ağ alt ağı için IP kullanımları Al|
|/virtualNetworks/virtualMachines/Read|Bir sanal ağ tüm sanal makinelere başvuruları alır|
|/virtualNetworks/virtualNetworkPeerings/delete|Bir sanal ağ eşlemesi siler|
|/virtualNetworks/virtualNetworkPeerings/read|Sanal ağ eşleme tanımını alır|
|/virtualNetworks/virtualNetworkPeerings/write|Bir sanal ağ eşlemesi oluşturur veya mevcut bir sanal ağ eşlemesi güncelleştirir|
|/virtualNetworks/write|Bir sanal ağ oluşturur veya mevcut bir sanal ağı güncelleştirir|
|/virtualNetworkTaps/delete|Sanal ağ Tap Sil|
|/virtualNetworkTaps/join/action|Bir sanal ağ tap birleştirir|
|/virtualNetworkTaps/read|Sanal ağ Tap Al|
|/virtualNetworkTaps/write|Sanal ağ Tap güncelle|
|/virtualwans/DELETE|Wan sanal bir siler|
|/virtualwans/Read|Sanal bir GET Wan|
|/virtualWans/virtualHubProxies/delete|Bir sanal Hub proxy siler|
|/virtualWans/virtualHubProxies/read|Bir sanal Hub proxy tanımını alır|
|/virtualWans/virtualHubProxies/write|Bir sanal Hub proxy oluşturur veya bir sanal Hub proxy güncelleştirir|
|/virtualwans/virtualHubs/read|Tüm sanal sanal Wan için ilişkili hub'ları alır.|
|/virtualwans/vpnconfiguration/read|Bir Vpn yapılandırmasını alır|
|/virtualWans/vpnSiteProxies/delete|Bir Vpn sitesi proxy siler|
|/virtualWans/vpnSiteProxies/read|Bir Vpn Site proxy tanımını alır|
|/virtualWans/vpnSiteProxies/write|Bir Vpn sitesi proxy oluşturur veya bir Vpn sitesi proxy güncelleştirir|
|/virtualwans/vpnSites/read|Bir sanal Wan için ilişkili tüm VPN siteleri alır.|
|/ virtualwans/yazma|Oluşturun veya sanal Wan güncelleştirin|
|/vpnGateways/read|Bir VpnGateway alır.|
|/vpnGateways/vpnConnections/read|Bir VpnConnection alır.|
|/vpnGateways/vpnConnections/write|Bir VpnConnection koyar.|
|/vpnGateways/write|Bir VpnGateway koyar.|
|/vpnsites/delete|Bir Vpn Site kaynağı siler.|
|/vpnsites/Read|Bir Vpn Site kaynağı alır.|
|/vpnsites/write|Oluşturur veya bir Vpn Site kaynağı güncelleştirir.|

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

| İşlem | Açıklama |
|---|---|
|/CheckNamespaceAvailability/action|Belirli bir Ad Alanı kaynak adının NotificationHub hizmetinde kullanılabilir olup olmadığını denetler.|
|/Namespaces/authorizationRules/action|Ad Alanı Yetkilendirme Kuralları açıklamasının listesini alır.|
|/Namespaces/authorizationRules/delete|Namespace yetkilendirme kuralını silin. Namespace yetkilendirme varsayılan kural silinemiyor. |
|/Namespaces/authorizationRules/listkeys/action|Ad Alanı için Bağlantı Dizesini alın|
|/Namespaces/authorizationRules/read|Ad Alanı Yetkilendirme Kuralları açıklamasının listesini alır.|
|/Namespaces/authorizationRules/regenerateKeys/action|Ad Alanı Yetkilendirme Kuralı Birincil/İkincil Anahtarı Yeniden Oluşturma, Yeniden oluşturulması gereken Anahtarı belirtin|
|/Namespaces/authorizationRules/write|Namespace düzeyi yetkilendirme kuralları oluşturun ve özelliklerini güncelleştirin. Yetkilendirme kuralları erişim hakları, birincil ve ikincil anahtarları güncelleştirilebilir.|
|/Namespaces/CheckNotificationHubAvailability/action|Belirli bir NotificationHub adının bir Ad Alanı içinde kullanılabilir olup olmadığını denetler.|
|/ Ad alanları/silme|Ad Alanı Kaynağını silin|
|/Namespaces/NotificationHubs/authorizationRules/action|Bildirim Hub'ı Yetkilendirme Kurallarının listesini alın|
|/Namespaces/NotificationHubs/authorizationRules/delete|Bildirim Hub'ı Yetkilendirme Kurallarını Sil|
|/Namespaces/NotificationHubs/authorizationRules/listkeys/action|Bildirim Hub'ı Bağlantı Dizesini alın|
|/Namespaces/NotificationHubs/authorizationRules/read|Bildirim Hub'ı Yetkilendirme Kurallarının listesini alın|
|/Namespaces/NotificationHubs/authorizationRules/regenerateKeys/action|Bildirim Hub'ı Yetkilendirme Kuralı Birincil/İkincil Anahtarı Yeniden Oluşturma, Yeniden oluşturulması gereken Anahtarı belirtin|
|/Namespaces/NotificationHubs/authorizationRules/write|Bildirim hub'ı yetkilendirme kuralları oluşturun ve özelliklerini güncelleştirin. Yetkilendirme kuralları erişim hakları, birincil ve ikincil anahtarları güncelleştirilebilir.|
|/Namespaces/NotificationHubs/debugSend/action|Test amaçlı bir anında iletme bildirimi gönderin.|
|/Namespaces/NotificationHubs/Delete|Bildirim Hub'ı Kaynağını Sil|
|/Namespaces/NotificationHubs/metricDefinitions/read|Namespace ölçümleri kaynak açıklamaları listesini al|
|/Namespaces/NotificationHubs/pnsCredentials/action|Tüm bildirim hub'ı PNS kimlik bilgilerini alın. Bu içerir, WNS, MPNS, APNS, GCM ve Baidu kimlik bilgileri|
|/Namespaces/NotificationHubs/read|Bildirim Hub'ı Kaynak Açıklamalarının listesini alın|
|/Namespaces/NotificationHubs/write|Bildirim hub'ı oluşturun ve özelliklerini güncelleştirin. Özelliklerini çoğunlukla PNS kimlik bilgilerini içerir. Yetkilendirme kuralları ve TTL|
|/ Ad alanları/okuma|Ad Alanı Kaynak Açıklamasının listesini alır|
|/ Ad alanları/yazma|Namespace kaynak oluşturun ve özelliklerini güncelleştirin. Etiketleri ve Namespace kapasitesini hangi güncelleştirilebilir özelliklerdir.|
|/register/action|Aboneliği NotificationHubs kaynak sağlayıcısı için kaydeder ve Ad Alanları ve NotificationHubs oluşturulmasını sağlar|

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

| İşlem | Açıklama |
|---|---|
|/linkTargets/Read|Bir Azure aboneliği ile ilişkili olmayan var olan hesapları listeler. Bu Azure aboneliği bir çalışma alanına bağlamak için bu işlem çalışma alanı Oluştur işlemi Müşteri Kimliği özelliğinde tarafından döndürülen bir müşteri kimliği kullanın.|
|/register/action|Bir kaynak sağlayıcısı için bir abonelik kaydedin.|
|/workspaces/analytics/query/action|Yeni altyapısını kullanarak arayın.|
|/workspaces/analytics/query/schema/read|Arama şema V2 alın.|
|/workspaces/api/query/action|Yeni altyapısını kullanarak arayın.|
|/workspaces/api/query/schema/read|Arama şema V2 alın.|
|/workspaces/configurationScopes/delete|Yapılandırma kapsamını Sil|
|/workspaces/configurationScopes/read|Yapılandırma kapsam Al|
|/workspaces/configurationScopes/write|Yapılandırma kapsamını ayarlama|
|/workspaces/datasources/delete|Çalışma alanı altında veri kaynakları silin.|
|/Workspaces/Datasources/Read|Veri kaynakları bir çalışma alanı altında alın.|
|/Workspaces/Datasources/Write|Veri kaynakları çalışma alanı altında oluştur/güncelleştir.|
|/Workspaces/DELETE|Bir çalışma alanı siler. Çalışma alanı oluşturma zamanında mevcut bir çalışma alanına bağlı değilse bağlı çalışma silinmez.|
|/workspaces/generateregistrationcertificate/action|Kayıt sertifikası için çalışma alanı oluşturur. Bu sertifika, Microsoft System Center Operation Manager çalışma alanına bağlamak için kullanılır.|
|/workspaces/intelligencepacks/disable/action|Belirli bir çalışma alanı için bir destek paketi devre dışı bırakır.|
|/workspaces/intelligencepacks/enable/action|Belirli bir çalışma alanı için bir destek paketi sağlar.|
|/workspaces/intelligencepacks/read|Verilen worksapce için görünür olan tüm Intelligence paketlerini listeler ve paketi etkin veya bu çalışma alanı için devre dışı olup olmadığını da listeler.|
|/workspaces/linkedServices/delete|Delete bağlantılı hizmetler çalışma alanında verilir.|
|/workspaces/linkedServices/read|Bağlı hizmetler almak çalışma verilir.|
|/workspaces/linkedServices/write|Oluşturma/bağlantılı güncelleştirme hizmetler çalışma alanında verilir.|
|/workspaces/listKeys/action|Çalışma alanı için liste anahtarları alır. Bu anahtarları Microsoft operasyonel Öngörüler aracıları için çalışma alanına bağlanmak için kullanılır.|
|/workspaces/listKeys/read|Çalışma alanı için liste anahtarları alır. Bu anahtarları Microsoft operasyonel Öngörüler aracıları için çalışma alanına bağlanmak için kullanılır.|
|/workspaces/managementGroups/read|Bu çalışma alanına bağlı System Center Operations Manager Yönetim grupları adları ve meta veriler alır.|
|/Workspaces/metricDefinitions/Read|Çalışma alanı altında ölçüm tanımlarını Al|
|/workspaces/notificationSettings/delete|Çalışma alanı için kullanıcının bildirim ayarları silin.|
|/workspaces/notificationSettings/read|Çalışma alanı için kullanıcının bildirim ayarlarını alın.|
|/workspaces/notificationSettings/write|Çalışma alanı için kullanıcının bildirim ayarları ayarlayın.|
|/workspaces/purge/action|Belirtilen veri çalışma alanından Sil|
|/Workspaces/Read|Var olan bir çalışma alır|
|/workspaces/savedSearches/delete|Kaydedilmiş bir arama sorgusunu siler|
|/workspaces/savedSearches/read|Kaydedilmiş bir arama sorgusunu alır|
|/workspaces/savedSearches/write|Kaydedilmiş bir arama sorgusunu oluşturur|
|/Workspaces/Schema/Read|Arama şeması için çalışma alır.  Arama şeması sunulan alanlar ve bunların türleri içerir.|
|/workspaces/search/action|Bir arama sorgusunu çalıştırır|
|/workspaces/sharedKeys/action|Çalışma alanı için paylaşılan anahtarları alır. Bu anahtarları Microsoft operasyonel Öngörüler aracıları için çalışma alanına bağlanmak için kullanılır.|
|/workspaces/sharedKeys/read|Çalışma alanı için paylaşılan anahtarları alır. Bu anahtarları Microsoft operasyonel Öngörüler aracıları için çalışma alanına bağlanmak için kullanılır.|
|/workspaces/storageinsightconfigs/delete|Bir depolama yapılandırması siler. Depolama hesabından veri okuma bu Microsoft operasyonel Öngörüler durdurur.|
|/workspaces/storageinsightconfigs/read|Bir depolama yapılandırmasını alır.|
|/workspaces/storageinsightconfigs/write|Yeni depolama yapılandırması oluşturur. Bu yapılandırmalar, varolan bir depolama hesabındaki bir konumdan veri çekmek için kullanılır.|
|/Workspaces/usages/Read|Çalışma alanı tarafından okunan veri miktarını da dahil olmak üzere bir çalışma alanı için kullanım verilerini alır.|
|/ workspaces/yazma|Yeni çalışma alanında ya da varolan bir çalışma alanı bağlantıları varolan çalışma alanından Müşteri Kimliği sağlayarak oluşturur.|

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

| İşlem | Açıklama |
|---|---|
|/managementAssociations/delete|Mevcut yönetim ilişkiyi sil|
|/managementAssociations/read|Varolan yönetim ilişkilendirmesi Al|
|/managementAssociations/write|Yeni bir yönetim ilişkilendirmesi oluştur|
|/managementConfigurations/delete|Mevcut yönetim Configuratin Sil|
|/managementConfigurations/read|Varolan yönetim yapılandırmasını alma|
|/managementConfigurations/write|Yeni Yönetim yapılandırması oluştur|
|/register/action|Bir kaynak sağlayıcısı için bir abonelik kaydedin.|
|/solutions/delete|Varolan bir OMS çözümü Sil|
|/solutions/read|OMS çözüm çıkma Al|
|/solutions/write|Yeni bir OMS çözümü oluşturun|

## <a name="microsoftportal"></a>Microsoft.Portal

| İşlem | Açıklama |
|---|---|
|/dashboards/delete|Panoyu abonelikten kaldırır.|
|/Dashboards/Read|Abonelik için panoları okur.|
|/dashboards/write|Bir aboneliğe pano ekleyin veya panoyu değiştirin.|

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated

| İşlem | Açıklama |
|---|---|
|/capacities/checkNameAvailability/action|Ayrılmış Power BI kapasitesini adının geçerli olup olmadığını denetler ve içinde değil kullanın.|
|/capacities/DELETE|Power BI siler kapasite ayrılmış.|
|/capacities/providers/Microsoft.Insights/metricDefinitions/read|Ayrılmış Power BI kapasitesini için kullanılabilir ölçümleri alır.|
|/capacities/Read|Belirtilen Power BI ayrılmış kapasite bilgilerini alır.|
|/ kapasiteleri/yazma|Oluşturur veya belirtilen Power BI kapasitesini ayrılmış güncelleştirir.|

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

| İşlem | Açıklama |
|---|---|
|/locations/allocatedStamp/read|GetAllocatedStamp, hizmet tarafından kullanılan iç işlemdir.|
|/locations/allocateStamp/action|AllocateStamp hizmeti tarafından kullanılan iç bir işlemdir|
|/locations/backupPreValidateProtection/action||
|/locations/backupStatus/action|Kurtarma Hizmetleri kasaları yedekleme durumunu denetleme|
|/locations/backupValidateFeatures/action|Özellikler doğrula|
|/Operations/Read|İşlemi kaynak sağlayıcısı için işlemler listesini döndürür|
|/register/action|Yazmaçları aboneliği için kaynak sağlayıcısı verilen|
|/Vaults/backupconfig/read|Döndürür yapılandırma kurtarma Hizmetleri kasası.|
|/Vaults/backupconfig/write|Güncelleştirmeler yapılandırması kurtarma Hizmetleri kasası.|
|/Vaults/backupEngines/read|Kasaya kayıtlı tüm yedekleme yönetimi sunucularının listesini döndürür.|
|/Vaults/backupFabrics/{fabricName}/protectionContainers/{containerName}/items/read|Tüm öğeleri bir kapsayıcıda Al|
|/Vaults/backupFabrics/backupProtectionIntent/write|Bir yedekleme koruma hedefini oluşturma|
|/Vaults/backupFabrics/operationResults/read|İşlemin durumunu döndürür|
|/Vaults/backupFabrics/protectableContainers/read|Tüm korunabilir kapsayıcıları Al|
|/Vaults/backupFabrics/protectionContainers/inquire/action|Bir kapsayıcı içindeki iş yükleri için sorgulama yapın|
|/Vaults/backupFabrics/protectionContainers/operationResults/read|Koruma Kapsayıcısı üzerinde gerçekleştirilen İşlemin sonuçlarını alır.|
|/Vaults/backupFabrics/protectionContainers/protectedItems/backup/action|Korumalı Öğe için Yedekleme gerçekleştirir.|
|/Vaults/backupFabrics/protectionContainers/protectedItems/delete|Öğe silme korumalı|
|/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read|Korumalı Öğeler üzerinde Gerçekleştirilen İşlemin Sonuçlarını alın.|
|/Vaults/backupFabrics/protectionContainers/protectedItems/operationsStatus/read|Korumalı Öğeler üzerinde Gerçekleştirilen İşlemin durumunu döndürür.|
|/Vaults/backupFabrics/protectionContainers/protectedItems/read|Korumalı öğe nesne ayrıntılarını döndürür|
|/Vaults/backupFabrics/protectionContainers/protectedItems/ recoveryPoints/provisionInstantItemRecovery/action|Korumalı öğe için sağlama anlık öğe kurtarma|
|/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read|Korumalı Öğeler için Kurtarma Noktalarını alın.|
|/Vaults/backupFabrics/protectionContainers/protectedItems/ recoveryPoints/restore/action|Korumalı Öğeler için Kurtarma Noktalarını geri yükleyin.|
|/Vaults/backupFabrics/protectionContainers/protectedItems/ recoveryPoints/revokeInstantItemRecovery/action|Anlık öğe kurtarma için korumalı öğe iptal etme|
|/Vaults/backupFabrics/protectionContainers/protectedItems/write|Bir yedekleme korumalı öğesi oluşturma|
|/Vaults/backupFabrics/protectionContainers/read|Tüm kayıtlı kapsayıcıları döndürür|
|/Vaults/backupFabrics/protectionContainers/write|Kayıtlı bir kapsayıcı oluşturur|
|/Vaults/backupFabrics/refreshContainers/action|Kapsayıcı listesini yeniler|
|/Vaults/backupJobs/cancel/action|İşi iptal|
|/Vaults/backupJobs/operationResults/read|İş İşleminin Sonucunu döndürür.|
|/Vaults/backupJobs/read|Tüm iş nesneleri döndürür|
|/Vaults/backupJobsExport/action|Dışarı aktarma işleri|
|/Vaults/backupJobsExport/operationResults/read|Dışa aktarma işi işleminin sonucunu döndürür.|
|/Vaults/backupManagementMetaData/read|Kurtarma Hizmetleri Kasası için Yedekleme Yönetimi Meta Verilerini döndürür.|
|/Vaults/backupOperationResults/read|Kurtarma Hizmetleri Kasası için Yedekleme işleminin Sonucunu döndürür.|
|/Vaults/backupOperations/read|Yedekleme işlemi döndürür durum kurtarma Hizmetleri kasası.|
|/Vaults/backupPolicies/delete|Bir koruma ilkesini Sil|
|/Vaults/backupPolicies/operationResults/read|İlke İşleminin Sonuçlarını alın.|
|/ Kasalar/backupPolicies/operations/okuma|İlke işlemin durumunu alın.|
|/Vaults/backupPolicies/read|Tüm koruma ilkeleri döndürür|
|/Vaults/backupPolicies/write|Koruma ilkesi oluşturur|
|/Vaults/backupProtectableItems/read|Tüm Korunabilir Öğelerin listesini döndürür.|
|/Vaults/backupProtectedItems/read|Tüm Korumalı Öğelerin listesini döndürür.|
|/Vaults/backupProtectionContainers/read|Aboneliğine ait tüm kapsayıcıları döndürür|
|/Vaults/backupSecurityPIN/action|Güvenlik PIN döndürür bilgi kurtarma Hizmetleri kasası.|
|/Vaults/backupstorageconfig/read|Döndürür depolama yapılandırması kurtarma Hizmetleri kasası.|
|/Vaults/backupstorageconfig/write|Güncelleştirmeler depolama yapılandırması kurtarma Hizmetleri kasası.|
|/Vaults/backupUsageSummaries/read|Özetleri korunan öğeler ve korunan sunucular için bir kurtarma Hizmetleri döndürür.|
|/ Kasalar/sertifika/yazma|Güncelleştirme kaynağı sertifika işlemi kaynak/kasa kimlik bilgileri sertifikası güncelleştirir.|
|/ Kasalar/silme|Kasa silme işlemi 'Kasası' türünde belirtilen Azure kaynak siler|
|/Vaults/extendedInformation/delete|Daha Fazla Bilgi Al işlemi, ?vault? türündeki Azure kaynağını temsil eden nesnenin Daha Fazla Bilgi değerini alır.|
|/Vaults/extendedInformation/read|Daha Fazla Bilgi Al işlemi, ?vault? türündeki Azure kaynağını temsil eden nesnenin Daha Fazla Bilgi değerini alır.|
|/Vaults/extendedInformation/write|Daha Fazla Bilgi Al işlemi, ?vault? türündeki Azure kaynağını temsil eden nesnenin Daha Fazla Bilgi değerini alır.|
|/ Kasalar/monitoringAlerts/okuma|Uyarılar için kurtarma Hizmetleri kasası alır.|
|/Vaults/monitoringAlerts/write|Uyarı çözümler.|
|/Vaults/monitoringConfigurations/read|Kurtarma Hizmetleri kasası bildirim yapılandırmasını alır.|
|/Vaults/monitoringConfigurations/write|Kurtarma Hizmetleri kasası e-posta bildirimleri yapılandırır.|
|/Vaults/providers/Microsoft.Insights/diagnosticSettings/read|Azure yedekleme tanılama|
|/Vaults/providers/Microsoft.Insights/diagnosticSettings/write|Azure yedekleme tanılama|
|/Vaults/providers/Microsoft.Insights/logDefinitions/read|Azure yedekleme günlükleri|
|/Vaults/providers/Microsoft.Insights/metricDefinitions/read|Azure yedekleme ölçümleri|
|/ Kasalar/okuma|Kasa alma işlemi Azure kaynak türü 'Kasası' temsil eden bir nesneyi alır|
|/Vaults/registeredIdentities/delete|Kapsayıcı kaydı işlemi, bir kapsayıcı kaydını silmek için kullanılabilir.|
|/Vaults/registeredIdentities/operationResults/read|Zaman uyumsuz olarak gönderilen işlemi için sonuç ve işlem durumunu alma işlemi işlemi kullanılabilir sonuçlar elde|
|/Vaults/registeredIdentities/read|Alma işlemi kullanılabilir kapsayıcıları için bir kaynak kayıtlı kapsayıcıları alın.|
|/Vaults/registeredIdentities/write|Hizmet kapsayıcısı kaydetme işlemi, bir kapsayıcı kurtarma Hizmeti'ne kaydolmak için kullanılabilir.|
|/vaults/replicationAlertSettings/read|Tüm uyarı ayarlarını okuma|
|/vaults/replicationAlertSettings/write|Tüm uyarı ayarlarını güncelle|
|/vaults/replicationEvents/read|Tüm olayları okuma|
|/vaults/replicationFabrics/checkConsistency/action|Doku Tutarlılığını Denetler|
|/vaults/replicationFabrics/delete|Tüm yapıları sil|
|/vaults/replicationFabrics/deployProcessServerImage/action|İşlem sunucusu görüntüsünü Dağıt|
|/vaults/replicationFabrics/read|Tüm yapıları okuma|
|/vaults/replicationFabrics/reassociateGateway/action|Ağ geçidi yeniden ilişkilendirin|
|/vaults/replicationFabrics/remove/action|Doku Kaldır|
|/vaults/replicationFabrics/renewcertificate/action|Yapı sertifikasını Yenile|
|/vaults/replicationFabrics/replicationNetworks/read|Herhangi bir ağa okuma|
|/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/delete|Tüm ağ eşlemeleri silme|
|/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/read|Tüm ağ eşlemeleri okuma|
|/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/write|Tüm ağ eşlemeleri güncelle|
|/vaults/replicationFabrics/replicationProtectionContainers/ discoverProtectableItem/action|Korunabilir öğe Bul|
|/vaults/replicationFabrics/replicationProtectionContainers/read|Herhangi bir koruma kapsayıcıdaki okuma|
|/vaults/replicationFabrics/replicationProtectionContainers/remove/action|Koruma kapsayıcısı Kaldır|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectableItems/read|Korunabilir tüm öğeleri okuma|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectedItems/applyRecoveryPoint/action|Kurtarma noktası Uygula|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectedItems/delete|Korunan öğeleri silin|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectedItems/failoverCommit/action|Yük devretmenin yürütülmesi|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectedItems/plannedFailover/action|Planlanan yük devretme|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectedItems/read|Tüm korumalı öğeleri okuma|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectedItems/recoveryPoints/read|Bir çoğaltma kurtarma noktası okuma|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectedItems/remove/action|Korumalı öğe kaldırma|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectedItems/repairReplication/action|Onarım çoğaltma|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectedItems/reProtect/action|Korumalı öğe koruyun|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectedItems/testFailover/action|Yük Devretme Sınaması|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectedItems/testFailoverCleanup/action|Yük devretme sınaması temizliğini|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectedItems/unplannedFailover/action|Yük devretme|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectedItems/updateMobilityService/action|Mobility hizmeti güncelleştirmesi|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectedItems/write|Tüm korumalı maddeleri oluştur veya güncelleştir|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectionContainerMappings/delete|Koruma kapsayıcısı eşlemeleri silme|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectionContainerMappings/read|Koruma kapsayıcısı eşlemeleri okuma|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectionContainerMappings/remove/action|Koruma kapsayıcısı eşlemesini Kaldır|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectionContainerMappings/write|Koruma kapsayıcısı eşlemeleri güncelle|
|/vaults/replicationFabrics/replicationProtectionContainers/switchprotection/action|Anahtar koruma kapsayıcısı|
|/vaults/replicationFabrics/replicationProtectionContainers/write|Herhangi bir koruma kapsayıcıdaki güncelle|
|/vaults/replicationFabrics/replicationRecoveryServicesProviders/delete|Tüm kurtarma Hizmetleri sağlayıcılarını Sil|
|/vaults/replicationFabrics/replicationRecoveryServicesProviders/read|Tüm kurtarma Hizmetleri sağlayıcılarını okuma|
|/vaults/replicationFabrics/replicationRecoveryServicesProviders/ refreshProvider/action|Sağlayıcı Yenile|
|/vaults/replicationFabrics/replicationRecoveryServicesProviders/remove/action|Kurtarma Hizmetleri sağlayıcısını Kaldır|
|/vaults/replicationFabrics/replicationRecoveryServicesProviders/write|Tüm kurtarma Hizmetleri sağlayıcılarını güncelle|
|/vaults/replicationFabrics/replicationStorageClassifications/read|Tüm depolama sınıflandırmaları okuma|
|/vaults/replicationFabrics/replicationStorageClassifications/ replicationStorageClassificationMappings/delete|Tüm depolama sınıflandırması eşlemeleri silme|
|/vaults/replicationFabrics/replicationStorageClassifications/ replicationStorageClassificationMappings/read|Tüm depolama sınıflandırması eşlemeleri okuma|
|/vaults/replicationFabrics/replicationStorageClassifications/ replicationStorageClassificationMappings/write|Tüm depolama sınıflandırması eşlemeleri güncelle|
|/vaults/replicationFabrics/replicationvCenters/delete|Herhangi bir işi Sil|
|/vaults/replicationFabrics/replicationvCenters/read|Herhangi bir işi okuma|
|/vaults/replicationFabrics/replicationvCenters/write|Tüm işleri oluşturun veya güncelleştirin|
|/vaults/replicationFabrics/write|Tüm yapıları güncelle|
|/vaults/replicationJobs/Cancel/Action|İşi İptal Et|
|/vaults/replicationJobs/Read|Herhangi bir işi okuma|
|/vaults/replicationJobs/restart/action|İşi yeniden başlatın|
|/vaults/replicationJobs/resume/action|İşi sürdürme|
|/vaults/replicationPolicies/delete|Herhangi bir ilkeyi Sil|
|/vaults/replicationPolicies/read|Tüm ilkeleri okuma|
|/vaults/replicationPolicies/write|Tüm ilkeler güncelle|
|/vaults/replicationRecoveryPlans/delete|Tüm kurtarma planlarını Sil|
|/vaults/replicationRecoveryPlans/failoverCommit/action|Yük devretmenin yürütülmesi kurtarma planı|
|/vaults/replicationRecoveryPlans/plannedFailover/action|Planlanan yük devretme kurtarma planı|
|/vaults/replicationRecoveryPlans/read|Tüm kurtarma planlarını okuma|
|/vaults/replicationRecoveryPlans/reProtect/action|Kurtarma planı koruyun|
|/vaults/replicationRecoveryPlans/testFailover/action|Test yük devretme kurtarma planı|
|/vaults/replicationRecoveryPlans/testFailoverCleanup/action|Test yük devretmesi temizliğini kurtarma planı|
|/vaults/replicationRecoveryPlans/unplannedFailover/action|Yük devretme kurtarma planı|
|/vaults/replicationRecoveryPlans/write|Tüm kurtarma planları oluştur veya güncelleştir|
|/Vaults/tokenInfo/read|Kurtarma Hizmetleri kasası için belirteç bilgileri döndürür.|
|/vaults/usages/Read|Herhangi bir kasa kullanımı okuma|
|/ Kasalar/kullanımları/okuma|Bir Kurtarma Hizmetleri Kasası için kullanım ayrıntılarını döndürür.|
|/Vaults/vaultTokens/read|Kasa belirteci işlemi kasası düzeyi arka uç işlemleri için kasa belirtecini almak için kullanılabilir.|
|/ Kasalar/yazma|Kasa Oluştur işlemi, 'vault' türünde bir Azure kaynağı oluşturur|

## <a name="microsoftrelay"></a>Microsoft.Relay

| İşlem | Açıklama |
|---|---|
|/checkNameAvailability/action|İlgili abonelikte ad alanının kullanılabilirliğini denetler.|
|/checkNamespaceAvailability/action|İlgili abonelikte ad alanının kullanılabilirliğini denetler. Bu API kullanım Lütfen CheckNameAvailabiltiy kullanın.|
|/namespaces/authorizationRules/action|Güncelleştirmeleri Namespace yetkilendirme kuralı. Depricated API'dir. Namespace yetkilendirme kuralı yerine güncelleştirmek için lütfen PUT çağrı kullanın... API sürümü 2017-04-01 üzerinde bu işlem desteklenmiyor.|
|/namespaces/authorizationRules/delete|Namespace yetkilendirme kuralını silin. Namespace yetkilendirme varsayılan kural silinemiyor. |
|/namespaces/authorizationRules/listkeys/Action|Ad Alanı için Bağlantı Dizesini alın|
|/namespaces/authorizationRules/Read|Ad Alanı Yetkilendirme Kuralları açıklamasının listesini alır.|
|/namespaces/authorizationRules/regenerateKeys/action|Kaynağın Birincil veya İkincil anahtarını yeniden oluşturun|
|/namespaces/authorizationRules/write|Namespace düzeyi yetkilendirme kuralları oluşturun ve özelliklerini güncelleştirin. Yetkilendirme kuralları erişim hakları, birincil ve ikincil anahtarları güncelleştirilebilir.|
|/ ad alanları/Sil|Ad Alanı Kaynağını silin|
|/namespaces/disasterRecoveryConfigs/authorizationRules/listkeys/action|Yetkilendirme kuralları anahtarları olağanüstü durum kurtarma birincil ad alanı için alır|
|/namespaces/disasterRecoveryConfigs/authorizationRules/read|Olağanüstü durum kurtarma birincil Namespace'nın yetkilendirme kuralları Al|
|/namespaces/disasterRecoveryConfigs/breakPairing/action|Olağanüstü Durum Kurtarma'yı devre dışı bırakır ve birincil ad alanındaki değişiklikleri ikincil ad alanına çoğaltmayı durdurur.|
|/namespaces/disasterrecoveryconfigs/checkNameAvailability/action|Ad alanı diğer adı altında denetimleri kullanılabilirliğini aboneliği verilir.|
|/namespaces/disasterRecoveryConfigs/delete|Ad alanıyla ilişkili olağanüstü durum kurtarma yapılandırması siler. Bu işlem yalnızca birincil ad alanı çağrılabilir.|
|/namespaces/disasterRecoveryConfigs/failover/action|Bir GEO DR yük devretmesini çağırır ve ad alanı diğer adını ikincil ad alanına işaret edecek şekilde yeniden yapılandırır.|
|/namespaces/disasterRecoveryConfigs/read|Ad alanıyla ilişkili Olağanüstü Durum Kurtarma yapılandırmasını alır.|
|/namespaces/disasterRecoveryConfigs/write|Ad alanıyla ilişkili Olağanüstü Durum Kurtarma yapılandırmasını oluşturur veya güncelleştirir.|
|/namespaces/HybridConnections/authorizationRules/action|HybridConnection güncelleştirmek için güncelleştirme işlemi. API sürümü 2017-04-01 üzerinde bu işlem desteklenmiyor. Yetkilendirme kuralları. Yetkilendirme kuralı güncelleştirmek için lütfen PUT çağrı kullanın.|
|/namespaces/HybridConnections/authorizationRules/delete|İşlemin HybridConnection yetkilendirme kurallarını silmek için|
|/namespaces/HybridConnections/authorizationRules/listkeys/Action|Bağlantı dizesi HybridConnection Al|
|/namespaces/HybridConnections/authorizationRules/read| HybridConnection yetkilendirme kuralları listesini al|
|/namespaces/HybridConnections/authorizationRules/regeneratekeys/action|Kaynağın Birincil veya İkincil anahtarını yeniden oluşturun|
|/namespaces/HybridConnections/authorizationRules/write|HybridConnection yetkilendirme kuralları oluşturun ve özelliklerini güncelleştirin. Yetkilendirme kuralları erişim hakları güncelleştirilebilir.|
|/namespaces/HybridConnections/Delete|HybridConnection kaynağı silme işlemi|
|/namespaces/HybridConnections/read|HybridConnection kaynak açıklamaları listesini al|
|/namespaces/HybridConnections/write|Oluşturma veya güncelleştirme HybridConnection özellikleri.|
|/namespaces/messagingPlan/read|Mesajlaşma planı için bir ad alır. Bu API kullanım dışıdır. MessagingPlan kaynak kullanıma sunulan özellikler (üst) sonraki API sürümleri Namespace kaynağın taşınması... API sürümü 2017-04-01 üzerinde bu işlem desteklenmiyor.|
|/namespaces/messagingPlan/write|Mesajlaşma Plan için bir ad alanı güncelleştirir. Bu API kullanım dışıdır. MessagingPlan kaynak kullanıma sunulan özellikler (üst) sonraki API sürümleri Namespace kaynağın taşınması... API sürümü 2017-04-01 üzerinde bu işlem desteklenmiyor.|
|/namespaces/operationresults/Read|Ad alanı işleminin durumunu al|
|/namespaces/providers/Microsoft.Insights/metricDefinitions/read|Namespace ölçümleri kaynak açıklamaları listesini al|
|/namespaces/Read|Ad Alanı Kaynak Açıklamasının listesini alır|
|/namespaces/WcfRelays/authorizationRules/action|WcfRelay güncelleştirmek için güncelleştirme işlemi. API sürümü 2017-04-01 üzerinde bu işlem desteklenmiyor. Yetkilendirme kuralları. Yetkilendirme kuralı güncelleştirmek için lütfen PUT çağrı kullanın.|
|/namespaces/WcfRelays/authorizationRules/delete|İşlemin WcfRelay yetkilendirme kurallarını silmek için|
|/namespaces/WcfRelays/authorizationRules/listkeys/Action|Bağlantı dizesi WcfRelay Al|
|/namespaces/WcfRelays/authorizationRules/read| WcfRelay yetkilendirme kuralları listesini al|
|/namespaces/WcfRelays/authorizationRules/regeneratekeys/action|Kaynağın Birincil veya İkincil anahtarını yeniden oluşturun|
|/namespaces/WcfRelays/authorizationRules/write|WcfRelay yetkilendirme kuralları oluşturun ve özelliklerini güncelleştirin. Yetkilendirme kuralları erişim hakları güncelleştirilebilir.|
|/namespaces/WcfRelays/DELETE|WcfRelay kaynağı silme işlemi|
|/namespaces/WcfRelays/Read|WcfRelay kaynak açıklamaları listesini al|
|/namespaces/WcfRelays/Write|Oluşturma veya güncelleştirme WcfRelay özellikleri.|
|/ ad alanları/yazma|Namespace kaynak oluşturun ve özelliklerini güncelleştirin. Etiketleri ve Namespace kapasitesini hangi güncelleştirilebilir özelliklerdir.|
|/Operations/Read|İşlemleri Al|
|/register/action|Aboneliği geçiş kaynak sağlayıcısı için kaydeder ve Geçiş kaynaklarının oluşturulmasını sağlar|
|/unregister/action|Aboneliği geçiş kaynak sağlayıcısı için kaydeder ve Geçiş kaynaklarının oluşturulmasını sağlar|

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth

| İşlem | Açıklama |
|---|---|
|/ AvailabilityStatuses/geçerli/okuma|Belirtilen kaynak için kullanılabilirlik durumunu alır|
|/ AvailabilityStatuses/okuma|Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır|
|/ healthevent/eylem|Belirtilen kaynaktaki sistem durumu değişikliğini gösterir|
|/healthevent/Activated/action|Belirtilen kaynaktaki sistem durumu değişikliğini gösterir|
|/healthevent/InProgress/action|Belirtilen kaynaktaki sistem durumu değişikliğini gösterir|
|/healthevent/Pending/Action|Belirtilen kaynaktaki sistem durumu değişikliğini gösterir|
|/healthevent/Resolved/Action|Belirtilen kaynaktaki sistem durumu değişikliğini gösterir|
|/healthevent/Updated/action|Belirtilen kaynaktaki sistem durumu değişikliğini gösterir|
|/register/action|Microsoft ResourceHealth için aboneliği kaydeder|

## <a name="microsoftresources"></a>Microsoft.Resources

| İşlem | Açıklama |
|---|---|
|/checkResourceName/action|Kaynak adının geçerliliğini denetle.|
|/Deployments/Cancel/Action|Bir dağıtımı iptal eder.|
|/deployments/delete|Bir dağıtımı siler.|
|/deployments/operations/read|Dağıtım işlemlerini alır veya listeler.|
|/deployments/read|Dağıtımları alır veya listeler.|
|/deployments/validate/action|Bir dağıtımı doğrular.|
|/deployments/write|Bir dağıtımı oluşturur veya güncelleştirir.|
|/Links/DELETE|Bir kaynak bağlantısını siler.|
|/Links/Read|Kaynak bağlantılarını alır veya listeler.|
|/ bağlantılar/yazma|Bir kaynak bağlantısını oluşturur veya güncelleştirir.|
|/Marketplace/purchase/Action|Marketten bir kaynak satın alır.|
|/providers/Read|Sağlayıcıların listesini al.|
|/Resources/Read|Filtrelere göre kaynak listelerini al.|
|/Subscriptions/Locations/Read|Desteklenen konumların listesini alır.|
|/Subscriptions/operationresults/Read|Abonelik işlem sonuçlarını alır.|
|/Subscriptions/providers/Read|Kaynak sağlayıcılarını alır veya listeler.|
|/Subscriptions/Read|Aboneliklerin listesini alır.|
|/subscriptions/resourceGroups/delete|Bir kaynak grubunu ve tüm kaynaklarını siler.|
|/subscriptions/resourcegroups/deployments/operations/read|Dağıtım işlemlerini alır veya listeler.|
|/subscriptions/resourcegroups/deployments/operationstatuses/read|Dağıtım işlemi durumlarını alır veya listeler.|
|/subscriptions/resourcegroups/deployments/read|Dağıtımları alır veya listeler.|
|/subscriptions/resourcegroups/deployments/write|Bir dağıtımı oluşturur veya güncelleştirir.|
|/subscriptions/resourceGroups/moveResources/action|Kaynakları bir kaynak grubundan diğerine taşır.|
|/subscriptions/resourceGroups/read|Kaynak gruplarını alır veya listeler.|
|/subscriptions/resourcegroups/resources/read|Kaynak grubun kaynaklarını alır.|
|/subscriptions/resourceGroups/validateMoveResources/action|Bir kaynak grubundan diğerine kaynak taşıma işlemini doğrular.|
|/subscriptions/resourceGroups/write|Kaynak grubunu oluşturur veya güncelleştirir.|
|/subscriptions/resources/Read|Bir aboneliğin kaynaklarını alır.|
|/subscriptions/tagNames/delete|Bir abonelik etiketini siler.|
|/subscriptions/tagNames/read|Abonelik etiketlerini alır veya listeler.|
|/subscriptions/tagNames/tagValues/delete|Bir abonelik etiketi değerini siler.|
|/subscriptions/tagNames/tagValues/read|Abonelik etiketi değerlerini alır veya listeler.|
|/subscriptions/tagNames/tagValues/write|Bir abonelik etiketi değeri ekler.|
|/Subscriptions/tagNames/Write|Bir abonelik etiketi ekler.|
|/tenants/Read|Kiracı listesini alır.|

## <a name="microsoftscheduler"></a>Microsoft.Scheduler

| İşlem | Açıklama |
|---|---|
|/jobcollections/DELETE|İş koleksiyonunu siler.|
|/jobcollections/disable/Action|İş koleksiyonunu devre dışı bırakır.|
|/jobcollections/Enable/Action|İş koleksiyonunu etkinleştirir.|
|/jobcollections/Jobs/DELETE|İşi siler.|
|/jobcollections/jobs/generateLogicAppDefinition/action|Bir Scheduler İşini temel alarak Mantıksal Uygulama tanımı oluşturur.|
|/jobcollections/Jobs/jobhistories/Read|İş geçmişini alır.|
|/jobcollections/Jobs/Read|İşi alır.|
|/jobcollections/jobs/run/action|İşi çalıştırır.|
|/jobcollections/Jobs/Write|İş oluşturur veya güncelleştirir.|
|/jobcollections/Read|İş Koleksiyonu Al|
|/jobcollections/write|İş koleksiyonu oluşturur veya güncelleştirir.|

## <a name="microsoftsearch"></a>Microsoft.Search

| İşlem | Açıklama |
|---|---|
|/checkNameAvailability/action|Hizmet adı kullanılabilirliğini denetler.|
|/register/action|Arama kaynak sağlayıcısı için aboneliği kaydeder ve arama hizmetleri oluşturulmasını sağlar.|
|/searchServices/createQueryKey/action|Sorgu anahtarı oluşturur.|
|/searchServices/delete|Arama hizmeti siler.|
|/searchServices/diagnosticSettings/read|Diganostic kaynak için okuma ayarını alır|
|/searchServices/diagnosticSettings/write|Oluşturur veya güncelleştirir kaynağın diganostic ayarı|
|/searchServices/listAdminKeys/action|Yönetici anahtarları okur.|
|/searchServices/logDefinitions/read|Arama hizmeti için kullanılabilir günlüklerini alır|
|/searchServices/metricDefinitions/read|Arama hizmeti için kullanılabilir ölçümleri alır|
|/searchServices/queryKey/delete|Sorgu anahtarı siler.|
|/searchServices/queryKey/read|Sorgu anahtarları okur.|
|/searchServices/read|Arama hizmeti okur.|
|/searchServices/regenerateAdminKey/action|Yönetici anahtarını yeniden oluşturur.|
|/searchServices/start/action|Arama hizmetini başlatır.|
|/searchServices/stop/action|Arama hizmetini durdurur.|
|/searchServices/write|Arama hizmeti güncelleştirir veya oluşturur.|

## <a name="microsoftsecurity"></a>Microsoft.Security

| İşlem | Açıklama |
|---|---|
|/Alerts/Read|Tüm kullanılabilir güvenlik uyarıları alır|
|/applicationWhitelistings/read|Uygulama whitelistings alır|
|/applicationWhitelistings/write|Yeni uygulamaları güvenilir listesine ekleme oluşturur veya mevcut kümeyi güncelleştirir|
|/complianceResults/Read|Kaynak için Uyumluluk sonuçlarını alır|
|/Locations/Alerts/Activate/Action|Bir güvenlik uyarısı etkinleştir|
|/locations/alerts/dismiss/action|Bir güvenlik uyarısını kapatmanın|
|/Locations/Alerts/Read|Tüm kullanılabilir güvenlik uyarıları alır|
|/locations/jitNetworkAccessPolicies/initiate/action|Yalnızca zaman ağ erişim ilkesi başlatır|
|/locations/jitNetworkAccessPolicies/read|Yalnızca zaman ağ erişim ilkelerini alır|
|/locations/jitNetworkAccessPolicies/write|Yeni bir tam zamanı ağ erişim ilkesi oluşturur veya mevcut kümeyi güncelleştirir|
|/Locations/Read|Güvenlik veri konumu alır|
|/locations/tasks/activate/action|Güvenlik açısından etkinleştir|
|/locations/tasks/dismiss/action|Güvenlik açısından kapatın|
|/Locations/Tasks/Read|Tüm kullanılabilir güvenlik önerileri alır|
|/locations/tasks/resolve/action|Güvenlik açısından çözümleyin|
|/Locations/Tasks/Start/Action|Güvenlik açısından Başlat|
|/Policies/Read|Güvenlik İlkesi alır|
|/ ilkeler/yazma|Güvenlik İlkesi güncelleştirmeleri|
|/pricings/DELETE|Kapsam için fiyatlandırma ayarları siler|
|/pricings/Read|Kapsam için fiyatlandırma ayarlarını alır|
|/ fiyatları/yazma|Kapsam için fiyatlandırma ayarlarını güncelleştirir|
|/register/action|Azure Güvenlik Merkezi için aboneliği kaydeder|
|/securityContacts/delete|Güvenlik kişiyi siler|
|/securityContacts/read|Güvenlik kişi alır|
|/securityContacts/write|Güvenlik kişi güncelleştirmeleri|
|/securitySolutions/delete|Bir güvenlik çözümü siler|
|/securitySolutions/Read|Güvenlik çözümleri alır|
|/securitySolutions/write|Yeni bir güvenlik çözümü oluşturur veya mevcut kümeyi güncelleştirir|
|/securitySolutionsReferenceData/read|Güvenlik çözümleri başvuru verileri alır|
|/securityStatuses/Read|Güvenlik sistem durumlarını için Azure kaynaklarını alır.|
|/securityStatusesSummaries/Read|Güvenlik durumları özetleri kapsamın alır.|
|/Tasks/Read|Tüm kullanılabilir güvenlik önerileri alır|
|/webApplicationFirewalls/delete|Bir web uygulaması güvenlik duvarı siler|
|/webApplicationFirewalls/read|Web uygulaması güvenlik duvarı alır|
|/webApplicationFirewalls/write|Yeni bir web uygulaması güvenlik duvarı oluşturur veya mevcut kümeyi güncelleştirir|
|/workspaceSettings/connect/action|Çalışma alanı ayarlarını yeniden bağlanma ayarlarını değiştirme|
|/workspaceSettings/delete|Çalışma alanı ayarları siler|
|/workspaceSettings/read|Çalışma alanı ayarları alır|
|/workspaceSettings/write|Güncelleştirmeler çalışma alanı ayarları|

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

| İşlem | Açıklama |
|---|---|
|/checkNameAvailability/action|İlgili abonelikte ad alanının kullanılabilirliğini denetler.|
|/checkNamespaceAvailability/action|İlgili abonelikte ad alanının kullanılabilirliğini denetler. Bu API kullanım Lütfen CheckNameAvailabiltiy kullanın.|
|/namespaces/authorizationRules/action|Güncelleştirmeleri Namespace yetkilendirme kuralı. Depricated API'dir. Namespace yetkilendirme kuralı yerine güncelleştirmek için lütfen PUT çağrı kullanın... API sürümü 2017-04-01 üzerinde bu işlem desteklenmiyor.|
|/namespaces/authorizationRules/delete|Namespace yetkilendirme kuralını silin. Namespace yetkilendirme varsayılan kural silinemiyor. |
|/namespaces/authorizationRules/listkeys/Action|Ad Alanı için Bağlantı Dizesini alın|
|/namespaces/authorizationRules/Read|Ad Alanı Yetkilendirme Kuralları açıklamasının listesini alır.|
|/namespaces/authorizationRules/regenerateKeys/action|Kaynağın Birincil veya İkincil anahtarını yeniden oluşturun|
|/namespaces/authorizationRules/write|Namespace düzeyi yetkilendirme kuralları oluşturun ve özelliklerini güncelleştirin. Yetkilendirme kuralları erişim hakları, birincil ve ikincil anahtarları güncelleştirilebilir.|
|/ ad alanları/Sil|Ad Alanı Kaynağını silin|
|/namespaces/disasterRecoveryConfigs/authorizationRules/listkeys/action|Yetkilendirme kuralları anahtarları olağanüstü durum kurtarma birincil ad alanı için alır|
|/namespaces/disasterRecoveryConfigs/authorizationRules/read|Olağanüstü durum kurtarma birincil Namespace'nın yetkilendirme kuralları Al|
|/namespaces/disasterRecoveryConfigs/breakPairing/action|Olağanüstü Durum Kurtarma'yı devre dışı bırakır ve birincil ad alanındaki değişiklikleri ikincil ad alanına çoğaltmayı durdurur.|
|/namespaces/disasterrecoveryconfigs/checkNameAvailability/action|Ad alanı diğer adı altında denetimleri kullanılabilirliğini aboneliği verilir.|
|/namespaces/disasterRecoveryConfigs/delete|Ad alanıyla ilişkili olağanüstü durum kurtarma yapılandırması siler. Bu işlem yalnızca birincil ad alanı çağrılabilir.|
|/namespaces/disasterRecoveryConfigs/failover/action|Bir GEO DR yük devretmesini çağırır ve ad alanı diğer adını ikincil ad alanına işaret edecek şekilde yeniden yapılandırır.|
|/namespaces/disasterRecoveryConfigs/read|Ad alanıyla ilişkili Olağanüstü Durum Kurtarma yapılandırmasını alır.|
|/namespaces/disasterRecoveryConfigs/write|Ad alanıyla ilişkili Olağanüstü Durum Kurtarma yapılandırmasını oluşturur veya güncelleştirir.|
|/namespaces/eventGridFilters/delete|Ad alanıyla ilişkili olay kılavuz filtre siler.|
|/namespaces/eventGridFilters/read|Ad alanıyla ilişkili olay kılavuz filtresini alır.|
|/namespaces/eventGridFilters/write|Oluşturur veya ad alanıyla ilişkili olay kılavuz filtre güncelleştirir.|
|/namespaces/eventhubs/Read|EventHub kaynak açıklamaları listesini al|
|/namespaces/messagingPlan/read|Mesajlaşma planı için bir ad alır. Bu API kullanım dışıdır. MessagingPlan kaynak kullanıma sunulan özellikler (üst) sonraki API sürümleri Namespace kaynağın taşınması... API sürümü 2017-04-01 üzerinde bu işlem desteklenmiyor.|
|/namespaces/messagingPlan/write|Mesajlaşma Plan için bir ad alanı güncelleştirir. Bu API kullanım dışıdır. MessagingPlan kaynak kullanıma sunulan özellikler (üst) sonraki API sürümleri Namespace kaynağın taşınması... API sürümü 2017-04-01 üzerinde bu işlem desteklenmiyor.|
|/namespaces/Migrate/Action|Ad alanı geçiş işlemi|
|/namespaces/operationresults/Read|Ad alanı işleminin durumunu al|
|/namespaces/providers/Microsoft.Insights/diagnosticSettings/Read|Namespace tanılama ayarları kaynak açıklamaları listesini al|
|/namespaces/providers/Microsoft.Insights/diagnosticSettings/write|Namespace tanılama ayarları kaynak açıklamaları listesini al|
|/namespaces/providers/Microsoft.Insights/logDefinitions/read|Namespace günlükleri kaynak açıklamaları listesini al|
|/namespaces/providers/Microsoft.Insights/metricDefinitions/read|Namespace ölçümleri kaynak açıklamaları listesini al|
|/namespaces/queues/authorizationRules/action|Sıra güncelleştirmek için güncelleştirme işlemi. API sürümü 2017-04-01 üzerinde bu işlem desteklenmiyor. Yetkilendirme kuralları. Yetkilendirme kuralı güncelleştirmek için lütfen PUT çağrı kullanın.|
|/namespaces/queues/authorizationRules/delete|İşlem sırası yetkilendirme kuralları silmek için|
|/namespaces/queues/authorizationRules/listkeys/action|Bağlantı dizesi sıraya al|
|/namespaces/queues/authorizationRules/read| Sıra yetkilendirme kuralları listesini al|
|/namespaces/queues/authorizationRules/regenerateKeys/action|Kaynağın Birincil veya İkincil anahtarını yeniden oluşturun|
|/namespaces/queues/authorizationRules/write|Sıra yetkilendirme kuralları oluşturun ve özelliklerini güncelleştirin. Yetkilendirme kuralları erişim hakları güncelleştirilebilir.|
|/namespaces/queues/Delete|Sıra kaynağı silme işlemi|
|/namespaces/queues/read|Sıra kaynak açıklamaları listesini al|
|/namespaces/queues/write|Oluşturma veya güncelleştirme sıra özellikleri.|
|/namespaces/Read|Ad Alanı Kaynak Açıklamasının listesini alır|
|/namespaces/topics/authorizationRules/action|Konu güncelleştirmek için güncelleştirme işlemi. API sürümü 2017-04-01 üzerinde bu işlem desteklenmiyor. Yetkilendirme kuralları. Yetkilendirme kuralı güncelleştirmek için lütfen PUT çağrı kullanın.|
|/namespaces/topics/authorizationRules/delete|İşlemin konu yetkilendirme kurallarını silmek için|
|/namespaces/topics/authorizationRules/listkeys/action|Bağlantı dizesi konuya Al|
|/namespaces/topics/authorizationRules/read| Konu yetkilendirme kuralları listesini al|
|/namespaces/topics/authorizationRules/regenerateKeys/action|Kaynağın Birincil veya İkincil anahtarını yeniden oluşturun|
|/namespaces/topics/authorizationRules/write|Konu yetkilendirme kuralları oluşturun ve özelliklerini güncelleştirin. Yetkilendirme kuralları erişim hakları güncelleştirilebilir.|
|/namespaces/topics/DELETE|Konu kaynağı silme işlemi|
|/namespaces/topics/Read|Konu kaynak açıklamaları listesini al|
|/namespaces/topics/Subscriptions/DELETE|TopicSubscription kaynağı silme işlemi|
|/namespaces/topics/Subscriptions/Read|TopicSubscription kaynak açıklamaları listesini al|
|/namespaces/topics/Subscriptions/Rules/DELETE|Kural kaynağı silme işlemi|
|/namespaces/topics/Subscriptions/Rules/Read|Kuralın kaynak açıklamaları listesini al|
|/namespaces/topics/Subscriptions/Rules/Write|Oluşturma veya güncelleştirme kuralı özellikleri.|
|/namespaces/topics/Subscriptions/Write|Oluşturma veya güncelleştirme TopicSubscription özellikleri.|
|/namespaces/topics/Write|Oluşturma veya güncelleştirme konu özellikleri.|
|/ ad alanları/yazma|Namespace kaynak oluşturun ve özelliklerini güncelleştirin. Etiketleri ve Namespace kapasitesini hangi güncelleştirilebilir özelliklerdir.|
|/Operations/Read|İşlemleri Al|
|/register/action|Aboneliği ServiceBus kaynak sağlayıcısı için kaydeder ve ServiceBus kaynaklarının oluşturulmasını sağlar|
|/SKU/Read|Sku kaynak açıklamaları listesini al|
|/sku/regions/read|SkuRegions kaynak açıklamaları listesini al|
|/unregister/action|Aboneliği ServiceBus kaynak sağlayıcısı için kaydeder ve ServiceBus kaynaklarının oluşturulmasını sağlar|

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric

| İşlem | Açıklama |
|---|---|
|/Clusters/Applications/DELETE|Herhangi Bir Uygulamayı Silin|
|/Clusters/Applications/Read|Herhangi Bir Uygulamayı Okuyun|
|/clusters/applications/services/delete|Herhangi Bir Hizmeti Silin|
|/Clusters/Applications/Services/Partitions/Read|Herhangi Bir Bölümü Okuyun|
|/Clusters/Applications/Services/Partitions/replicas/Read|Herhangi Bir Çoğaltmayı Okuyun|
|/clusters/applications/services/read|Herhangi Bir Hizmeti Okuyun|
|/clusters/applications/services/statuses/read|Tüm Hizmet Durumlarını Oku|
|/clusters/applications/services/write|Herhangi Bir Hizmet Oluşturun veya Güncelleştirin|
|/Clusters/Applications/Write|Herhangi Bir Uygulama Oluşturun veya Güncelleştirin|
|/clusters/applicationTypes/delete|Herhangi Bir Uygulama Türü Silin|
|/clusters/applicationTypes/read|Herhangi Bir Uygulama Türünü Okuyun|
|/clusters/applicationTypes/versions/delete|Herhangi Bir Uygulama Türü Sürümü Silin|
|/clusters/applicationTypes/versions/read|Herhangi Bir Uygulama Türü Sürümünü Okuyun|
|/clusters/applicationTypes/versions/write|Herhangi Bir Uygulama Türü Sürümü Oluşturun veya Güncelleştirin|
|/clusters/applicationTypes/write|Herhangi Bir Uygulama Türü Oluşturun veya Güncelleştirin|
|/Clusters/DELETE|Herhangi Bir Kümeyi Silin|
|/Clusters/Nodes/Read|Herhangi Bir Düğümü Okuyun|
|/Clusters/Read|Herhangi Bir Kümeyi Okuyun|
|/Clusters/statuses/Read|Tüm Küme Durumlarını Oku|
|/ kümeleri/yazma|Herhangi Bir Kümeyi Oluşturun veya Güncelleştirin|
|/locations/clusterVersions/read|Herhangi bir Küme Sürümünü oku|
|/Locations/Environments/clusterVersions/Read|Belirli bir ortam için herhangi bir Küme Sürümünü okuyun|
|/Locations/operationresults/Read|Herhangi Bir İşlem Sonucunu Okuyun|
|/Locations/Operations/Read|Konuma göre herhangi bir İşlemi okuyun|
|/Operations/Read|Kullanılabilen Herhangi Bir İşlemi Okuyun|
|/register/action|Herhangi Bir Eylemi Kaydedin|

## <a name="microsoftsolutions"></a>Microsoft.Solutions

| İşlem | Açıklama |
|---|---|
|/applicationDefinitions/delete|Uygulama tanımı kaldırır.|
|/applicationDefinitions/read|Uygulama tanımları listesi alır.|
|/applicationDefinitions/write|Uygulama tanımı ekle veya değiştir.|
|/Applications/DELETE|Uygulama kaldırır.|
|/Applications/Read|Uygulama listesi alır.|
|/applications/write|Uygulama oluşturur.|
|/Locations/operationStatuses/Read|Kaynağın işlem durumunu okur.|
|/register/action|Çözümler için kaydolun.|

## <a name="microsoftsql"></a>Microsoft.Sql

| İşlem | Açıklama |
|---|---|
|/checkNameAvailability/action|Sunucu adı belirli bir aboneliğe yönelik tüm dünyada sağlamak için kullanılabilir olup olmadığını doğrulayın.|
|/locations/auditingSettingsAzureAsyncOperation/read|Sonuç İlkesi ayarlama işlemi denetim genişletilmiş sunucusu BLOB alma|
|/locations/auditingSettingsOperationResults/read|Sonuç İlkesi ayarlama işlemi denetim sunucu BLOB alma|
|/Locations/Capabilities/Read|Özellikleri bu abonelik için belirtilen bir konumda alır.|
|/locations/databaseAzureAsyncOperation/read|Veritabanı işlemi durumunu alır.|
|/locations/databaseOperationResults/read|Veritabanı işlemi durumunu alır.|
|/locations/deletedServerAsyncOperation/read|İlerleme-silinen sunucusundaki işlemleri alır|
|/locations/deletedServerOperationResults/read|İlerleme-silinen sunucusundaki işlemleri alır|
|/locations/deletedServers/read|Silinmiş sunucuları veya belirtilen silinen sunucunun özelliklerini alır listesini döndürür.|
|/locations/deletedServers/recover/action|Silinen sunucusunu kurtarma|
|/locations/deleteVirtualNetworkOrSubnets/action|Bir sanal ağ veya alt ağa ilişkili sanal ağ kuralları siler|
|/locations/elasticPoolAzureAsyncOperation/read|Esnek havuz zaman uyumsuz bir işlem için azure zaman uyumsuz işlemi alır|
|/locations/elasticPoolOperationResults/read|Bir esnek havuz işleminin sonucunu alır.|
|/locations/extendedAuditingSettingsAzureAsyncOperation/read|Sonuç İlkesi ayarlama işlemi denetim genişletilmiş sunucusu BLOB alma|
|/locations/extendedAuditingSettingsOperationResults/read|Sonuç İlkesi ayarlama işlemi denetim genişletilmiş sunucusu BLOB alma|
|/locations/managedDatabaseRestoreAzureAsyncOperation/completeRestore/action|Yönetilen veritabanı geri yükleme işlemi tamamlanır|
|/locations/managedTransparentDataEncryptionAzureAsyncOperation/read|İlerleme-yönetilen veritabanında saydam veri şifreleme işlemleri alır|
|/locations/managedTransparentDataEncryptionOperationResults/read|İlerleme-yönetilen veritabanında saydam veri şifreleme işlemleri alır|
|/Locations/Read|Belirli bir aboneliğe kullanılabilir konumlarını alır|
|/locations/syncAgentOperationResults/read|Eşitleme Aracı kaynak işleminin sonucu alma|
|/locations/syncDatabaseIds/read|Belirli bölge ve abonelik için eşitleme veritabanı kimlikleri alma|
|/locations/syncGroupOperationResults/read|Eşitleme grubu kaynak işleminin sonucu alma|
|/locations/syncMemberOperationResults/read|Eşitleme üye kaynak işleminin sonucu alma|
|/Locations/usages/Read|Kullanım ölçümleri koleksiyonu Bu abonelik için bir konumda alır.|
|/locations/virtualNetworkRulesAzureAsyncOperation/read|Azure zaman uyumsuz işlem belirtilen sanal ağ kuralları ayrıntılarını döndürür |
|/locations/virtualNetworkRulesOperationResults/read|Belirtilen sanal ağ kuralları işlem ayrıntılarını döndürür |
|/managedInstances/administrators/delete|Mevcut yönetici yönetilen örneğinin siler.|
|/managedInstances/administrators/read|Yönetilen örneği yöneticilerin listesini alır.|
|/managedInstances/administrators/write|Belirtilen parametrelerle yönetilen örneği yönetici güncelleştirir veya oluşturur.|
|/managedInstances/databases/delete|Var olan bir yönetilen veritabanı siler|
|/managedInstances/databases/read|Yönetilen veritabanı varolan alır|
|/managedInstances/databases/securityAlertPolicies/read|Belirli bir yönetilen veritabanı üzerinde yapılandırılan veritabanı tehdit algılama ilkesi ayrıntılarını alma|
|/managedInstances/databases/securityAlertPolicies/write|Verilen bir yönetilen veritabanı için veritabanı tehdit algılama ilkesini değiştirme|
|/managedInstances/databases/securityEvents/read|Yönetilen veritabanı güvenlik olaylarını alır|
|/managedInstances/databases/transparentDataEncryption/read|Belirli bir yönetilen veritabanında saydam veri şifreleme veritabanı ayrıntılarını alma|
|/managedInstances/databases/transparentDataEncryption/write|Belirli bir yönetilen veritabanı için veritabanı saydam veri şifreleme değiştirme|
|/managedInstances/databases/write|Yeni bir veritabanı oluşturur veya varolan bir veritabanını güncelleştirir.|
|/managedInstances/delete|Mevcut yönetilen örneği siler.|
|/managedInstances/metricDefinitions/read|Yönetilen örneği ölçüm tanımlarını Al|
|/managedInstances/metrics/read|Yönetilen örneği ölçümleri alma|
|/managedInstances/read|Yönetilen örnekleri veya belirtilen yönetilen örneği için özellikleri alır listesini döndürür.|
|/managedInstances/securityAlertPolicies/read|Belirli bir yönetilen sunucusunda yapılandırılan yönetilen sunucu tehdit algılama ilkesi ayrıntılarını alma|
|/managedInstances/securityAlertPolicies/write|Belirli bir yönetilen sunucuda yönetilen sunucu tehdit algılama ilkesini değiştirme|
|/managedInstances/write|Yönetilen bir örneğini belirtilen parametrelerle oluşturur veya özellikleri veya etiketleri belirtilen yönetilen örneği için güncelleştirin.|
|/Operations/Read|Kullanılabilir REST işlemlerini alır|
|/register/action|Microsoft SQL veritabanı kaynak sağlayıcısı için aboneliği kaydeder ve Microsoft SQL veritabanları oluşturulmasını sağlar.|
|/servers/administratorOperationResults/read|Sunucu yöneticileri üzerinde devam eden işlemler alır|
|/servers/administrators/delete|Sunucu Yöneticisi Sil|
|/servers/administrators/read|Sunucu Yöneticisi ile ilgili ayrıntıları alma|
|/Servers/Administrators/Write|Sunucu Yöneticisi güncelle|
|/servers/advisors/read|Sunucu için kullanılabilir danışmanlar listesini döndürür|
|/servers/advisors/recommendedActions/read|Sunucusu için belirtilen Danışmanı'nın önerilen eylemleri listesini döndürür|
|/servers/advisors/recommendedActions/write|Sunucudaki önerilen eylemi uygulayın|
|/servers/advisors/write|Güncelleştirmeleri otomatik-sunucu düzeyinde bir Danışmanı durumunu yürütün.|
|/servers/auditingPolicies/read|Denetim İlkesi belirli bir sunucuda yapılandırılan varsayılan sunucu tablosu ayrıntılarını alma|
|/servers/auditingPolicies/write|Varsayılan sunucu tablo belli bir sunucu için Denetim değiştirme|
|/servers/auditingSettings/operationResults/read|Sonuç İlkesi ayarlama işlemi denetim sunucu BLOB alma|
|/servers/auditingSettings/read|Verilen bir sunucu üzerinde yapılandırılmış sunucu blob denetim ilkesi ayrıntılarını alma|
|/servers/auditingSettings/write|Sunucu blob belli bir sunucu için Denetim değiştirme|
|/servers/automaticTuning/read|Sunucusu için otomatik ayarlarını döndürür|
|/servers/automaticTuning/write|Sunucusu için otomatik ayarlarını güncelleştirir ve güncelleştirilmiş ayarları döndürür|
|/servers/backupLongTermRetentionVaults/delete|Mevcut bir yedekleme arşivleme kasası siler.|
|/servers/backupLongTermRetentionVaults/read|Bu işlem, bir yedekleme uzun süreli saklama kasası almak için kullanılır. Bu sunucuya kayıtlı olduğu kasanın hakkında bilgi döndürür|
|/servers/backupLongTermRetentionVaults/write|Bu işlem bir yedekleme uzun vadeli bekletme kaydetmek için kullanılan bir sunucu kasası|
|/servers/communicationLinks/delete|Var olan bir sunucu iletişimi bağlantı siler.|
|/servers/communicationLinks/read|Belirtilen bir sunucunun iletişim bağlantılarının bir listesini döndürür.|
|/servers/communicationLinks/write|Oluşturun veya bir sunucu iletişimi bağlantı güncelleştirin.|
|/servers/connectionPolicies/read|Sunucu bağlantısı belirli bir sunucunun ilkelerin listesini döndürür.|
|/servers/connectionPolicies/write|Oluşturun veya sunucu bağlantı İlkesi güncelleştirin.|
|/servers/databases/advisors/read|Veritabanı için kullanılabilir danışmanlar listesini döndürür|
|/servers/databases/advisors/recommendedActions/read|Veritabanı için belirtilen Danışmanı'nın önerilen eylemleri listesini döndürür|
|/servers/databases/advisors/recommendedActions/write|Veritabanı üzerinde önerilen eylemi uygulayın|
|/servers/databases/advisors/write|Güncelleştirme otomatik yürütme veritabanı düzeyinde bir advisor durumu.|
|/servers/databases/auditingPolicies/read|Belirli bir veritabanı üzerinde yapılandırılan tablo denetim ilkesi ayrıntılarını alma|
|/servers/databases/auditingPolicies/write|Verilen bir veritabanı için tablo denetim ilkesini değiştirme|
|/servers/databases/auditingSettings/read|Belirli bir veritabanı üzerinde yapılandırılan blob denetim ilkesi ayrıntılarını alma|
|/servers/databases/auditingSettings/write|Verilen bir veritabanı için blob denetim ilkesini değiştirme|
|/servers/databases/auditRecords/read|Veritabanı blob Denetim kayıtlarını alma|
|/servers/databases/automaticTuning/read|Bir veritabanı için otomatik ayarlama ayarları döndürür|
|/servers/databases/automaticTuning/write|Bir veritabanı için otomatik ayarlama ayarlarını güncelleştirir ve güncelleştirilmiş ayarları döndürür|
|/servers/databases/azureAsyncOperation/read|Veritabanı işlemi durumunu alır.|
|/servers/databases/backupLongTermRetentionPolicies/read|Belirtilen veritabanı yedekleme arşivleme ilkelerin listesini döndürür.|
|/servers/databases/backupLongTermRetentionPolicies/write|Oluşturun veya bir veritabanı yedekleme arşivleme İlkesi güncelleştirin.|
|/servers/databases/connectionPolicies/read|Belirli bir veritabanı üzerinde yapılandırılan bağlantı ilkesi ayrıntılarını alma|
|/servers/databases/connectionPolicies/write|Verilen bir veritabanı için bağlantı ilkesini değiştirme|
|/servers/databases/dataMaskingPolicies/read|Veritabanı veri ilkeleri maskeleme listesini döndürür.|
|/servers/databases/dataMaskingPolicies/rules/delete|Verilen bir veritabanı için ilke kuralı maskeleme verilerini sil|
|/servers/databases/dataMaskingPolicies/rules/read|Belirli bir veritabanı üzerinde yapılandırılan ilke kuralı maskeleme veri ayrıntılarını alma|
|/servers/databases/dataMaskingPolicies/rules/write|Verilen bir veritabanı için ilke kuralı maskeleme verileri değiştirme|
|/servers/databases/dataMaskingPolicies/write|İlke verilen bir veritabanı için maskeleme verileri değiştirme|
|/servers/databases/dataWarehouseQueries/dataWarehouseQuerySteps/read|Seçili adımı kimliği için veri ambarı sorgusu dağıtılmış sorgu adım bilgileri döndürür|
|/Servers/Databases/dataWarehouseQueries/Read|Seçili sorgu kimliği için veri ambarı dağıtım sorgu bilgilerini döndürür|
|/servers/databases/dataWarehouseUserActivities/read|Çalışan ve askıya alınan sorguları içeren bir SQL Data Warehouse örneğine kullanıcı etkinliklerini alır.|
|/servers/databases/delete|Varolan bir veritabanını siler.|
|/servers/databases/export/action|Azure SQL veritabanını dışarı aktarma|
|/servers/databases/extendedAuditingSettings/read|Belirli bir veritabanı üzerinde yapılandırılan genişletilmiş blob denetim ilkesi ayrıntılarını alma|
|/servers/databases/extendedAuditingSettings/write|Verilen bir veritabanı için genişletilmiş blob denetim ilkesini değiştirme|
|/servers/databases/extensions/read|Veritabanı için uzantıları koleksiyonunu alır.|
|/servers/databases/extensions/write|Verilen bir veritabanı uzantısı değiştirme|
|/servers/databases/geoBackupPolicies/read|Verilen bir veritabanı için yedekleme ilkeleri coğrafi alma|
|/servers/databases/geoBackupPolicies/write|Bir veritabanı geobackup İlkesi güncelle|
|/servers/databases/importExportOperationResults/read|İlerleme-içeri/dışarı aktarma işlemlerini alır|
|/servers/databases/metricDefinitions/read|Dönüş türleri veritabanları için kullanılabilir ölçümleri|
|/servers/databases/metrics/read|Veritabanları için dönüş ölçümleri|
|/servers/databases/move/action|Azure SQL veritabanını yeniden adlandırın|
|/servers/databases/operationResults/read|Veritabanı işlemi durumunu alır.|
|/Servers/Databases/Operations/Cancel/Action|Azure SQL veritabanı henüz tamamlanmadı zaman uyumsuz işlemi iptal eder.|
|/Servers/Databases/Operations/Read|Veritabanı üzerinde gerçekleştirilen işlemler listesini döndürür|
|/servers/databases/pause/action|Duraklatma Azure SQL veri ambarı veritabanı|
|/servers/databases/providers/Microsoft.Insights/diagnosticSettings/read|Kaynak için tanılama ayarını alır|
|/servers/databases/providers/Microsoft.Insights/diagnosticSettings/write|Kaynak için tanılama ayarını oluşturur veya güncelleştirir|
|/servers/databases/providers/Microsoft.Insights/logDefinitions/read|Veritabanları için kullanılabilir günlüklerini alır|
|/servers/databases/providers/Microsoft.Insights/metricDefinitions/read|Dönüş türleri veritabanları için kullanılabilir ölçümleri|
|/servers/databases/queryStore/queryTexts/read|Belirtilen parametrelere karşılık sorgu metinleri koleksiyonunu döndürür.|
|/servers/databases/queryStore/read|Veritabanı için Query Store ayarlarının geçerli değerleri döndürür.|
|/servers/databases/queryStore/write|Veritabanı için Query Store ayarı güncelleştirir|
|/servers/databases/read|Veritabanları veya belirtilen veritabanı özelliklerini alır listesini döndürür.|
|/servers/databases/replicationLinks/delete|Çoğaltma ilişkisinin zorla ve olası veri kaybı ile Sonlandır|
|/servers/databases/replicationLinks/failover/action|Bu veritabanı çoğaltma relationship\u0027s birincil yapma ve uzak bir ikincil birincil yapmadan tüm eşitlemeden sonra Yük devretme birincil sunucudan değiştirir|
|/servers/databases/replicationLinks/forceFailoverAllowDataLoss/action|Olası veri kaybı olmadan hemen bu veritabanı çoğaltma relationship\u0027s birincil yapma ve uzak bir ikincil birincil yapmadan yük devretme|
|/servers/databases/replicationLinks/read|Belirli bir veritabanı için kurulmuş yineleme bağlantıları hakkında dönüş ayrıntıları|
|/servers/databases/replicationLinks/unlink/action|Çoğaltma ilişkisinin zorla veya ortağıyla eşitlemeden sonra Sonlandır|
|/servers/databases/replicationLinks/updateReplicationMode/action|Bağlantı zaman uyumlu veya zaman uyumsuz modu için güncelleştirme çoğaltma modu|
|/servers/databases/restorePoints/action|Yeni bir geri yükleme noktası oluşturur|
|/servers/databases/restorePoints/read|Döndürür geri yükleme noktaları için veritabanı.|
|/servers/databases/resume/action|Resume Azure SQL veri ambarı veritabanı|
|/servers/databases/schemas/read|Veritabanı şemaları listesini alma|
|/servers/databases/schemas/tables/columns/read|Bir tablodaki sütunların listesini alma|
|/servers/databases/schemas/tables/columns/sensitivityLabels/delete|Verili bir sütunun duyarlılık etiketini Sil|
|/servers/databases/schemas/tables/columns/sensitivityLabels/read|Verili bir sütunun duyarlılık etiketini Al|
|/servers/databases/schemas/tables/columns/sensitivityLabels/write|Verili bir sütunun duyarlılık etiketini güncelle|
|/servers/databases/schemas/tables/read|Bir veritabanının tabloların listesini alma|
|/servers/databases/schemas/tables/recommendedIndexes/read|Bir veritabanı üzerinde dizin önerileri listesini alma|
|/servers/databases/schemas/tables/recommendedIndexes/write|Dizin önerisi Uygula|
|/servers/databases/securityAlertPolicies/read|Belirli bir veritabanı üzerinde yapılandırılan tehdit algılama ilkesi ayrıntılarını alma|
|/servers/databases/securityAlertPolicies/write|Verilen bir veritabanı tehdit algılama ilkesini değiştirme|
|/servers/databases/securityMetrics/read|Veritabanı güvenlik ölçümleri koleksiyonunu alır|
|/servers/databases/sensitivityLabels/read|Belirli bir veritabanının duyarlılık etiketleri listeleme|
|/servers/databases/serviceTierAdvisors/read|Performansı veya maliyetini azaltmak için sorgu yürütme istatistikleri temel veritabanı yukarı veya aşağı Ölçeklendirmesi hakkında öneri Döndür|
|/servers/databases/syncGroups/cancelSync/action|Eşitleme grubu eşitleme iptal et|
|/servers/databases/syncGroups/delete|Var olan bir eşitleme grubunu siler.|
|/servers/databases/syncGroups/hubSchemas/read|Hub veritabanı şemalarını eşitleme listesini döndürür|
|/servers/databases/syncGroups/logs/read|Eşitleme grubu günlükleri listesini döndürür|
|/servers/databases/syncGroups/read|Dönüş eşitleme listesi gruplar veya belirtilen eşitleme grubu için özellikleri alır.|
|/servers/databases/syncGroups/refreshHubSchema/action|Eşitleme hub veritabanı şemasını Yenile|
|/servers/databases/syncGroups/refreshHubSchemaOperationResults/read|Eşitleme hub şema yenileme işleminin sonucu alma|
|/servers/databases/syncGroups/syncMembers/delete|Var olan bir eşitleme üye siler.|
|/servers/databases/syncGroups/syncMembers/read|Belirtilen eşitleme üyesi için eşitleme üyeleri veya alır özellikler listesini döndürür.|
|/servers/databases/syncGroups/syncMembers/refreshSchema/action|Eşitleme üye şemasını Yenile|
|/servers/databases/syncGroups/syncMembers/refreshSchemaOperationResults/read|Eşitleme üye şema yenileme işleminin sonucu alma|
|/servers/databases/syncGroups/syncMembers/schemas/read|Üye veritabanı şemalarını eşitleme listesini döndürür|
|/servers/databases/syncGroups/syncMembers/write|Eşitleme üyesi belirtilen parametrelerle oluşturur veya belirtilen eşitleme üyesi için özelliklerini güncelleştirir.|
|/servers/databases/syncGroups/triggerSync/action|Tetikleyici eşitleme grubu eşitleme|
|/servers/databases/syncGroups/write|Belirtilen parametrelerle bir eşitleme grubu oluşturur veya belirtilen eşitleme grubu özelliklerini güncelleştirin.|
|/servers/databases/topQueries/queryText/action|Seçili sorgu kimliği için Transact-SQL metnini döndürür|
|/servers/databases/topQueries/read|Döndürür seçili sorgu için çalışma zamanı istatistikleri seçilen zaman aralığı içinde toplanır.|
|/servers/databases/topQueries/statistics/read|Döndürür seçili sorgu için çalışma zamanı istatistikleri seçilen zaman aralığı içinde toplanır.|
|/servers/databases/transparentDataEncryption/operationResults/read|İlerleme-saydam veri şifreleme işlemleri alır|
|/servers/databases/transparentDataEncryption/read|Durum ve verilen bir veritabanı için saydam veri şifreleme güvenlik özelliği ayrıntılarını alma|
|/servers/databases/transparentDataEncryption/write|Saydam veri şifreleme durumunu değiştirme|
|/servers/databases/upgradeDataWarehouse/action|Azure SQL veri ambarı veritabanı yükseltme|
|/servers/databases/usages/read|Azure SQL veritabanı kullanımları bilgilerini alır|
|/servers/databases/vulnerabilityAssessments/delete|Verilen bir veritabanı için güvenlik açığı değerlendirmesi Kaldır|
|/servers/databases/vulnerabilityAssessments/read|Belirli bir veritabanı üzerinde yapılandırılan güvenlik açığı değerlendirmesi ayrıntılarını alma|
|/servers/databases/vulnerabilityAssessments/rules/baselines/delete|Verilen bir veritabanı için güvenlik açığı değerlendirmesi kural temel Kaldır|
|/servers/databases/vulnerabilityAssessments/rules/baselines/read|Verilen bir veritabanı için güvenlik açığı değerlendirmesi kural taban Al|
|/servers/databases/vulnerabilityAssessments/rules/baselines/write|Verilen bir veritabanı için güvenlik açığı değerlendirmesi kural temel değiştirme|
|/servers/databases/vulnerabilityAssessments/scans/action|Güvenlik Açığı değerlendirmesi veritabanı taraması yürütün.|
|/servers/databases/vulnerabilityAssessments/scans/export/action|Varolan bir tarama sonuç bir insan okunabilir biçimine dönüştürün. Hiçbir şey zaten olur|
|/servers/databases/vulnerabilityAssessments/scans/read|Veritabanı güvenlik açığı listesi değerlendirme taraması kayıtları dönün veya belirtilen tarama kimliği için tarama kayıt Al|
|/servers/databases/vulnerabilityAssessments/write|Verilen bir veritabanı için güvenlik açığı değerlendirmesi değiştirme|
|/servers/databases/vulnerabilityAssessmentScans/action|Güvenlik Açığı değerlendirmesi veritabanı taraması yürütün.|
|/servers/databases/vulnerabilityAssessmentScans/operationResults/read|Veritabanı güvenlik açığı değerlendirmesi taraması yürütme işlemini sonucunu alma|
|/servers/databases/vulnerabilityAssessmentSettings/read|Belirli bir veritabanı üzerinde yapılandırılan güvenlik açığı değerlendirmesi ayrıntılarını alma|
|/servers/databases/vulnerabilityAssessmentSettings/write|Verilen bir veritabanı için güvenlik açığı değerlendirmesi değiştirme|
|/servers/databases/write|Belirtilen parametrelerle bir veritabanı oluşturur veya özellikleri veya etiketleri belirtilen veritabanı için güncelleştirin.|
|/servers/delete|Var olan bir sunucuyu siler.|
|/servers/disasterRecoveryConfiguration/delete|Verilen bir sunucu için mevcut bir olağanüstü durum kurtarma yapılandırmaları siler|
|/servers/disasterRecoveryConfiguration/failover/action|Yük devretme bir DisasterRecoveryConfiguration|
|/servers/disasterRecoveryConfiguration/forceFailoverAllowDataLoss/action|Yük devretme bir DisasterRecoveryConfiguration zorla|
|/servers/disasterRecoveryConfiguration/read|Bu sunucu içeren kurtarma yapılandırmalar olağanüstü bir koleksiyonunu alır|
|/servers/disasterRecoveryConfiguration/write|Sunucu olağanüstü durum kurtarma yapılandırmasını değiştirme|
|/servers/elasticPoolEstimates/read|Zaten bu sunucu için oluşturulan esnek havuz tahminler listesini döndürür|
|/servers/elasticPoolEstimates/write|Sağlanan veritabanı listesi için yeni esnek havuz tahmini oluşturur|
|/servers/elasticPools/advisors/read|Esnek havuz için kullanılabilir danışmanlar listesini döndürür|
|/servers/elasticPools/advisors/recommendedActions/read|Esnek havuz için belirtilen Danışmanı'nın önerilen eylemleri listesini döndürür|
|/servers/elasticPools/advisors/recommendedActions/write|Esnek havuz üzerinde önerilen eylemi uygulayın|
|/servers/elasticPools/advisors/write|Güncelleştirme otomatik yürütme esnek havuz düzeyde bir advisor durumu.|
|/servers/elasticPools/databases/read|Esnek havuz için veritabanı listesi alır|
|/servers/elasticPools/delete|Var olan esnek havuzunu silme|
|/servers/elasticPools/elasticPoolActivity/read|Etkinlikleri ve verilen esnek veritabanı havuzu ayrıntıları alma|
|/servers/elasticPools/elasticPoolDatabaseActivity/read|Etkinlikleri ve esnek veritabanı havuzu parçası olan belirli bir veritabanı üzerinde ayrıntıları alma|
|/servers/elasticPools/metricDefinitions/read|Dönüş türleri esnek veritabanı havuzları için kullanılabilir ölçümleri|
|/servers/elasticPools/metrics/read|Esnek veritabanı havuzları için dönüş ölçümleri|
|/Servers/elasticPools/Operations/Cancel/Action|Azure SQL esnek havuzu bekleyen henüz tamamlanmadı zaman uyumsuz işlemi iptal eder.|
|/servers/elasticPools/operations/read|Esnek havuz üzerinde gerçekleştirilen işlemler listesini döndürür|
|/servers/elasticPools/providers/Microsoft.Insights/diagnosticSettings/read|Kaynak için tanılama ayarını alır|
|/servers/elasticPools/providers/Microsoft.Insights/diagnosticSettings/write|Kaynak için tanılama ayarını oluşturur veya güncelleştirir|
|/servers/elasticPools/providers/Microsoft.Insights/metricDefinitions/read|Dönüş türleri esnek veritabanı havuzları için kullanılabilir ölçümleri|
|/servers/elasticPools/read|Belirli bir sunucuda esnek havuz ayrıntılarını alma|
|/servers/elasticPools/skus/read|Bu esnek havuz için SKU'ları kullanılabilir bir koleksiyonunu alır|
|/servers/elasticPools/write|Yeni bir oluşturun veya var olan esnek havuz özelliklerini değiştir|
|/servers/encryptionProtector/read|Sunucu şifreleme koruyucuları listesini döndürür veya belirtilen sunucu için şifreleme koruyucusu özellikleri alır.|
|/servers/encryptionProtector/write|Belirtilen sunucu şifreleme koruyucusu için güncelleştirme.|
|/servers/extendedAuditingSettings/read|Denetim İlkesi belirli bir sunucuda yapılandırılan genişletilmiş sunucusu blob ayrıntılarını alma|
|/servers/extendedAuditingSettings/write|Genişletilmiş sunucu blob belli bir sunucu için Denetim değiştirme|
|/servers/failoverGroups/delete|Var olan bir yük devretme grubunu siler.|
|/servers/failoverGroups/failover/action|Planlanan yük devretme varolan bir yük devretme grubu yürütür.|
|/servers/failoverGroups/forceFailoverAllowDataLoss/action|Varolan bir yük devretme grubu zorla yük devretme yürütür.|
|/servers/failoverGroups/read|Gruplar veya özellikleri alır belirtilen yük devretme grubu için yük devretme listesi döndürür.|
|/servers/failoverGroups/write|Belirtilen parametrelerle bir yük devretme grubu oluşturur veya özellikleri veya etiketleri belirtilen yük devretme grubu için güncelleştirir.|
|/servers/firewallRules/delete|Mevcut bir sunucu güvenlik duvarı kuralını siler.|
|/servers/firewallRules/read|Sunucu güvenlik duvarı listesi kuralları veya belirtilen sunucu için güvenlik duvarı kuralı özellikleri alır döndürür.|
|/servers/firewallRules/write|Sunucu güvenlik duvarı kuralı oluşturur belirtilen parametrelerle belirtilen kural için özelliklerde güncelleştirmek veya yeni sunucu güvenlik duvarı kuralları ile tüm kuralların üzerine.|
|/servers/import/action|Sunucuda yeni bir veritabanı oluşturun ve şeması ve verisi DacPac paketinden dağıtma|
|/servers/importExportOperationResults/read|İlerleme-içeri/dışarı aktarma işlemlerini alır|
|/servers/keys/delete|Mevcut bir sunucu anahtarı siler.|
|/Servers/Keys/Read|Dönüş sunucu listesini anahtarları veya belirtilen sunucu anahtarı için özellikleri alır.|
|/Servers/Keys/Write|Belirtilen parametrelerle bir anahtarı oluşturur veya özellikleri veya etiketleri belirtilen sunucu anahtarı için güncelleştirin.|
|/servers/operationResults/read|İlerleme-sunucu işlemleri alır|
|/servers/providers/Microsoft.Insights/metricDefinitions/read|Dönüş türleri sunucuları için kullanılabilir ölçümleri|
|/Servers/Read|Sunucuları veya alır belirtilen sunucu için özellikler listesini döndürür.|
|/servers/recommendedElasticPools/databases/read|Verilen bir sunucu için önerilen esnek veritabanı havuzlarına ilişkin ölçümleri alma|
|/servers/recommendedElasticPools/read|Maliyet veya historica kaynak kullanımı temelinde performansını artırmak esnek veritabanı havuzları için öneri alma|
|/servers/recoverableDatabases/read|Bu işlem, Canlı veritabanı olağanüstü durum kurtarma için bilinen son iyi yedekleme noktasına veritabanını geri yüklemek için kullanılır. Bilgi verir en son iyi yedeklemeden ancak hakkında doesn\u0027t gerçekten veritabanını geri yükle.|
|/servers/restorableDroppedDatabases/read|Hala bekletme ilkesi içinde olan belirli bir sunucuda bırakılan veritabanlarının bir listesini alın.|
|/servers/securityAlertPolicies/operationResults/read|Sunucu tehdit algılama İlkesi yazma işlemini sonuçlarını alma|
|/servers/securityAlertPolicies/read|Verilen bir sunucu üzerinde yapılandırılmış sunucu tehdit algılama ilkesi ayrıntılarını alma|
|/servers/securityAlertPolicies/write|Belirli bir sunucuda Sunucu tehdit algılama ilkesini değiştirme|
|/servers/serviceObjectives/read|Hizmet düzeyi hedefleri (performans katmanı olarak da bilinir) belirli bir sunucuda bulunan listesini alma|
|/Servers/syncAgents/DELETE|Varolan bir eşitleme aracı siler.|
|/servers/syncAgents/generateKey/action|Eşitleme Aracısı kayda anahtarı oluştur|
|/servers/syncAgents/linkedDatabases/read|Eşitleme Aracısı bağlı veritabanlarının listesini döndürür|
|/Servers/syncAgents/Read|Belirtilen eşitleme aracı için eşitleme aracıları veya alır özellikler listesini döndürür.|
|/Servers/syncAgents/Write|Belirtilen parametrelerle bir eşitleme aracısı oluşturur veya belirtilen eşitleme aracı için güncelleştirme.|
|/Servers/usages/Read|Sunucu içindeki tüm veritabanları tarafından sunucu DTU kota ve geçerli DTU tüketim döndürür|
|/servers/virtualNetworkRules/delete|Mevcut bir sanal ağ kuralını siler|
|/servers/virtualNetworkRules/read|Dönüş sanal ağ listesi kuralları veya belirtilen sanal ağ kuralı özellikleri alır.|
|/servers/virtualNetworkRules/write|Belirtilen parametrelerle bir sanal ağ kuralı oluşturur veya özellikleri veya etiketleri belirtilen sanal ağ kuralı için güncelleştirin.|
|/ sunucuları/yazma|Sunucu belirtilen parametrelerle oluşturur veya özellikleri veya etiketleri belirtilen sunucu güncelleştirme.|
|/unregister/action|Microsoft SQL veritabanı kaynak sağlayıcısı için abonelik kaydını siler ve Microsoft SQL veritabanları oluşturulmasını sağlar.|
|/virtualClusters/read|Sanal kümelerin veya belirtilen sanal küme özelliklerini alır listesini döndürür.|
|/virtualClusters/write|Sanal küme etiketleri güncelleştirir.|

## <a name="microsoftstorage"></a>Microsoft.Storage

| İşlem | Açıklama |
|---|---|
|/checknameavailability/Read|Hesap adının geçerliliğini ve kullanımda olup olmadığını denetler.|
|/locations/deleteVirtualNetworkOrSubnets/action|Microsoft.Storage'a sanal ağın veya alt ağın silindiğini bildirir|
|/Operations/Read|Bir zaman uyumsuz işlemin durumunu yoklar.|
|/register/action|Depolama kaynak sağlayıcısı için aboneliği kaydeder ve depolama hesaplarının oluşturulmasını etkinleştirir.|
|/skus/Read|Microsoft.Storage tarafından desteklenen SKU'ları listeler.|
|/storageAccounts/blobServices/containers/clearLegalHold/action|Blob kapsayıcısı yasal tutmasını temizle|
|/storageAccounts/blobServices/containers/delete|Bir kapsayıcının silinmesinin sonucunu döndürür|
|/storageAccounts/blobServices/containers/immutabilityPolicies/delete|Blob kapsayıcısı değiştirilemezlik ilkesini sil|
|/storageAccounts/blobServices/containers/immutabilityPolicies/extend/action|Blob kapsayıcısı değiştirilemezlik ilkesini genişlet|
|/storageAccounts/blobServices/containers/immutabilityPolicies/lock/action|Blob kapsayıcısı değitirilemezliğini kilitleme ilkesi|
|/storageAccounts/blobServices/containers/immutabilityPolicies/read|Blob kapsayıcısı değiştirilemezlik ilkesini al|
|/storageAccounts/blobServices/containers/immutabilityPolicies/write|Blob kapsayıcısı değitirilemezlik ilkesi koy|
|/storageAccounts/blobServices/containers/read|Bir kapsayıcı veya bir kapsayıcı listesi döndürür|
|/storageAccounts/blobServices/containers/setLegalHold/action|Blob kapsayıcısı yasal tutma ayarla|
|/storageAccounts/blobServices/containers/write|Blob kapsayıcısı koyma veya kiralama sonucunu döndürür|
|/storageAccounts/blobServices/providers/Microsoft.Insights/diagnosticSettings/read|Kaynağın tanılama ayarını alır.|
|/storageAccounts/blobServices/providers/Microsoft.Insights/diagnosticSettings/write|Kaynağın tanılama ayarını oluşturur veya güncelleştirir.|
|/storageAccounts/blobServices/providers/Microsoft.Insights/metricDefinitions/read|Microsoft Depolama Ölçümleri tanımlarının listesini alın.|
|/storageAccounts/blobServices/read|Blob hizmeti özelliklerini veya istatistiklerini döndürür|
|/storageAccounts/blobServices/write|Blob hizmeti özelliklerini koymanın sonucunu döndürür|
|/storageAccounts/delete|Mevcut bir depolama hesabını siler.|
|/storageAccounts/fileServices/providers/Microsoft.Insights/diagnosticSettings/read|Kaynağın tanılama ayarını alır.|
|/storageAccounts/fileServices/providers/Microsoft.Insights/diagnosticSettings/write|Kaynağın tanılama ayarını oluşturur veya güncelleştirir.|
|/storageAccounts/fileServices/providers/Microsoft.Insights/metricDefinitions/read|Microsoft Depolama Ölçümleri tanımlarının listesini alın.|
|/storageAccounts/listAccountSas/action|Belirtilen depolama hesabı için Hesap SAS belirtecini döndürür.|
|/storageAccounts/listkeys/action|Belirtilen depolama hesabının erişim anahtarlarını döndürür.|
|/storageAccounts/listServiceSas/action|Belirtilen depolama hesabı için Hizmet SAS belirtecini döndürür.|
|/storageAccounts/providers/Microsoft.Insights/diagnosticSettings/read|Kaynağın tanılama ayarını alır.|
|/storageAccounts/providers/Microsoft.Insights/diagnosticSettings/write|Kaynağın tanılama ayarını oluşturur veya güncelleştirir.|
|/storageAccounts/providers/Microsoft.Insights/metricDefinitions/read|Microsoft Depolama Ölçümleri tanımlarının listesini alın.|
|/storageAccounts/queueServices/providers/Microsoft.Insights/diagnosticSettings/write|Kaynağın tanılama ayarını oluşturur veya güncelleştirir.|
|/storageAccounts/queueServices/providers/Microsoft.Insights/metricDefinitions/read|Microsoft Depolama Ölçümleri tanımlarının listesini alın.|
|/storageAccounts/queueServices/queues/delete|Bir kuyruğu silmenin sonucunu döndürür|
|/storageAccounts/queueServices/queues/read|Bir kuyruğu veya kuyruk listesini döndürür.|
|/storageAccounts/queueServices/queues/write|Bir kuyruğu yazmanın sonucunu döndürür|
|/storageAccounts/queueServices/read|Kuyruk hizmeti özelliklerini veya istatistiklerini döndürür.|
|/storageAccounts/queueServices/write|Kuyruk hizmeti özelliklerini ayarlamanın sonucunu döndürür|
|/storageAccounts/read|Depolama hesaplarının listesini döndürür veya belirtilen depolama hesabının özelliklerini alır.|
|/storageAccounts/regeneratekey/action|Belirtilen depolama hesabının erişim anahtarlarını yeniden oluşturur.|
|/storageAccounts/services/diagnosticSettings/write|Depolama hesabı tanılama ayarlarını oluşturun/güncelleştirin.|
|/storageAccounts/storageAccounts/queueServices/providers/ Microsoft.Insights/diagnosticSettings/read|Kaynağın tanılama ayarını alır.|
|/storageAccounts/tableServices/providers/Microsoft.Insights/diagnosticSettings/read|Kaynağın tanılama ayarını alır.|
|/storageAccounts/tableServices/providers/Microsoft.Insights/diagnosticSettings/write|Kaynağın tanılama ayarını oluşturur veya güncelleştirir.|
|/storageAccounts/tableServices/providers/Microsoft.Insights/metricDefinitions/read|Microsoft Depolama Ölçümleri tanımlarının listesini alın.|
|/storageAccounts/write|Belirtilen parametrelerle bir depolama hesabı oluşturur, özellikleri veya etiketleri güncelleştirir ya da belirtilen depolama hesabına özel etki alanı ekler.|
|/usages/Read|Belirtilen abonelikteki kaynakların sınır ve geçerli kullanım sayısını döndürür|

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync

| İşlem | Açıklama |
|---|---|
|/storageSyncServices/delete|Tüm depolama Eşitleme Hizmetleri Sil|
|/storageSyncServices/providers/Microsoft.Insights/metricDefinitions/read|Depolama Eşitleme Hizmetleri için kullanılabilir ölçümleri alır|
|/storageSyncServices/read|Tüm depolama Eşitleme Hizmetleri okuma|
|/storageSyncServices/registeredServers/delete|Herhangi bir kayıtlı sunucu silme|
|/storageSyncServices/registeredServers/read|Herhangi bir kayıtlı sunucu okuma|
|/storageSyncServices/registeredServers/write|Herhangi bir kayıtlı sunucu güncelle|
|/storageSyncServices/syncGroups/cloudEndpoints/delete|Bulut uç nokta silme|
|/storageSyncServices/syncGroups/cloudEndpoints/operationresults/read|Zaman uyumsuz yedekleme çağrıları için konum API|
|/storageSyncServices/syncGroups/cloudEndpoints/postbackup/action|Bu eylem yedeklemeden sonra çağırın|
|/storageSyncServices/syncGroups/cloudEndpoints/postrestore/action|Bu eylemi geri yükleme sonrasında çağırın|
|/storageSyncServices/syncGroups/cloudEndpoints/prebackup/action|Bu eylem yedekleme önce çağırın|
|/storageSyncServices/syncGroups/cloudEndpoints/prerestore/action|Bu eylem geri yükleme önce çağırın|
|/storageSyncServices/syncGroups/cloudEndpoints/read|Bulut uç nokta okuma|
|/storageSyncServices/syncGroups/cloudEndpoints/restoreheartbeat/action|Sinyal geri yükleme|
|/storageSyncServices/syncGroups/cloudEndpoints/write|Bulut uç nokta güncelle|
|/storageSyncServices/syncGroups/delete|Herhangi bir eşitleme grubu Sil|
|/storageSyncServices/syncGroups/read|Herhangi bir eşitleme grubu okuma|
|/storageSyncServices/syncGroups/serverEndpoints/delete|Sunucu uç nokta silme|
|/storageSyncServices/syncGroups/serverEndpoints/read|Sunucu uç nokta okuma|
|/storageSyncServices/syncGroups/serverEndpoints/recallAction/action|Arama dosyaları bir sunucuya geri çekmek için bu eylemi|
|/storageSyncServices/syncGroups/serverEndpoints/write|Sunucu uç nokta güncelle|
|/storageSyncServices/syncGroups/write|Eşitleme grupları oluştur veya güncelleştir|
|/storageSyncServices/write|Oluşturma veya güncelleştirme tüm depolama Eşitleme Hizmetleri|

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

| İşlem | Açıklama |
|---|---|
|/managers/accessControlRecords/delete|Erişim denetimi kayıtları siler|
|/managers/accessControlRecords/read|Erişim denetimi kayıtları alır veya listeler|
|/managers/accessControlRecords/write|Erişim denetimi kayıtları oluştur veya güncelleştir|
|/managers/alerts/read|Uyarıları alır veya listeler|
|/managers/bandwidthSettings/delete|Var olan bir bant genişliği ayarları siler (yalnızca 8000 Series)|
|/managers/bandwidthSettings/read|Bant genişliği ayarlarını listesi (yalnızca 8000 Series)|
|/managers/bandwidthSettings/write|Yeni bir oluşturur veya güncelleştirir bant genişliği ayarları (yalnızca 8000 Series)|
|/ Yöneticileri/sertifika/yazma|Güncelleştirme kaynağı sertifika işlemi kaynak/kasa kimlik bilgileri sertifikası güncelleştirir.|
|/managers/clearAlerts/action|Aygıt Yöneticisi ile ilişkili tüm uyarıları temizleyin.|
|/managers/cloudApplianceConfigurations/read|Liste bulut uygulaması desteklenen yapılandırmalar|
|/managers/configureDevice/action|Bir aygıt yapılandırır|
|/managers/delete|Cihaz yöneticileri siler|
|/ Yöneticileri/silme|Kasa silme işlemi 'Kasası' türünde belirtilen Azure kaynak siler|
|/managers/devices/alertSettings/read|Uyarı ayarlarını alır veya listeler|
|/managers/devices/alertSettings/write|Uyarı ayarlarını güncelle|
|/managers/devices/backupPolicies/backup/action|İsteğe bağlı oluşturmak için el ile yedekleme İlkesi tarafından korunan tüm birimlerin yedek alın.|
|/managers/devices/backupPolicies/delete|Mevcut bir yedekleme ilkeleri siler (yalnızca 8000 Series)|
|/managers/devices/backupPolicies/read|(Yalnızca 8000 serisi) listesi yedekleme ilkeleri|
|/managers/devices/backupPolicies/schedules/delete|Var olan zamanlamalar siler|
|/managers/devices/backupPolicies/schedules/read|Zamanlamaları listesi|
|/managers/devices/backupPolicies/schedules/write|Yeni bir oluşturur veya zamanlamaları güncelleştirir|
|/managers/devices/backupPolicies/write|Yeni bir oluşturur veya yedekleme ilkeleri güncelleştirir (yalnızca 8000 Series)|
|/managers/devices/backups/delete|Yedekleme kümesini siler|
|/managers/devices/backups/elements/clone/action|Bir paylaşımı veya bir yedekleme öğesi kullanarak birimi kopyalama.|
|/managers/devices/backups/read|Yedekleme kümesini alır veya listeler|
|/managers/devices/backups/restore/action|Tüm birimlerin bir yedekleme kümesinden geri yükleyin.|
|/managers/devices/backupScheduleGroups/delete|Yedekleme zamanlaması gruplarını siler|
|/managers/devices/backupScheduleGroups/read|Yedeklemeyi zamanlama grupları alır veya listeler|
|/managers/devices/backupScheduleGroups/write|Yedekleme zamanlaması grupları oluştur veya güncelleştir|
|/managers/devices/chapSettings/delete|Chap ayarları siler|
|/managers/devices/chapSettings/read|Chap ayarları alır veya listeler|
|/managers/devices/chapSettings/write|Chap ayarları güncelle|
|/managers/devices/deactivate/action|Bir aygıtı devre dışı bırakır.|
|/managers/devices/delete|Aygıtları siler|
|/managers/Devices/download/Action|Bir aygıt için dowload güncelleştirmeler.|
|/managers/devices/failover/action|Cihaz yük devretmesi.|
|/managers/devices/fileservers/backup/action|Bir dosya sunucusunda yedekleme gerçekleştirin.|
|/managers/devices/fileservers/delete|Dosya sunucuları siler|
|/managers/devices/fileservers/metrics/read|Ölçümleri alır veya listeler|
|/managers/devices/fileservers/metricsDefinitions/read|Ölçüm tanımlarını alır veya listeler|
|/managers/devices/fileservers/read|Dosya sunucuları alır veya listeler|
|/managers/devices/fileservers/shares/delete|Paylaşımlar siler|
|/managers/devices/fileservers/shares/metrics/read|Ölçümleri alır veya listeler|
|/managers/devices/fileservers/shares/metricsDefinitions/read|Ölçüm tanımlarını alır veya listeler|
|/managers/devices/fileservers/shares/read|Paylaşımlar alır veya listeler|
|/managers/devices/fileservers/shares/write|Paylaşımlar güncelle|
|/managers/devices/fileservers/write|Dosya sunucuları güncelle|
|/managers/devices/hardwareComponentGroups/changeControllerPowerState/action|Donanım bileşen grupları denetleyicisi güç durumunu değiştir|
|/managers/devices/hardwareComponentGroups/read|Donanım bileşen grupları listesi|
|/managers/devices/install/action|Bir cihazda güncelleştirmeleri yükleyin.|
|/managers/devices/installUpdates/action|Aygıtlarda güncelleştirmeleri yükler|
|/managers/devices/iscsiservers/backup/action|Bir iSCSI sunucusu yedek alın.|
|/managers/devices/iscsiservers/delete|İSCSI sunucuları siler|
|/managers/devices/iscsiservers/disks/delete|Diskleri siler|
|/managers/devices/iscsiservers/disks/metrics/read|Ölçümleri alır veya listeler|
|/managers/devices/iscsiservers/disks/metricsDefinitions/read|Ölçüm tanımlarını alır veya listeler|
|/managers/devices/iscsiservers/disks/read|Diskleri alır veya listeler|
|/managers/devices/iscsiservers/disks/write|Diskleri güncelle|
|/managers/devices/iscsiservers/metrics/read|Ölçümleri alır veya listeler|
|/managers/devices/iscsiservers/metricsDefinitions/read|Ölçüm tanımlarını alır veya listeler|
|/managers/devices/iscsiservers/read|İSCSI sunucuları alır veya listeler|
|/managers/devices/iscsiservers/write|İSCSI sunucuları güncelle|
|/managers/Devices/Jobs/Cancel/Action|Bir çalışan işi iptal etme|
|/managers/devices/jobs/read|İşlerini alır veya listeler|
|/managers/devices/listFailoverSets/action|Varolan bir aygıt için yük devretme kümeleri listeleyin.|
|/managers/devices/listFailoverTargets/action|Aygıtların listesi yük devri hedefleri|
|/managers/devices/metrics/read|Ölçümleri alır veya listeler|
|/managers/devices/metricsDefinitions/read|Ölçüm tanımlarını alır veya listeler|
|/managers/devices/migrationSourceConfigurations/confirmMigration/action|Başarılı bir geçiş doğrular ve onu uygulayın.|
|/managers/devices/migrationSourceConfigurations/fetchConfirmMigrationStatus/action|Geçiş Onayla durumunu getirin.|
|/managers/devices/migrationSourceConfigurations/fetchMigrationEstimate/action|Durum geçiş tahmin işinin getirin.|
|/managers/devices/migrationSourceConfigurations/fetchMigrationStatus/action|Geçiş için durum getirin.|
|/managers/devices/migrationSourceConfigurations/import/action|Geçiş için kaynak yapılandırmaları alma|
|/managers/devices/migrationSourceConfigurations/startMigration/action|Kaynak yapılandırmaları kullanarak geçiş Başlat|
|/managers/devices/migrationSourceConfigurations/startMigrationEstimate/action|Geçiş işleminin süresi tahmin etmek için bir proje başlatın.|
|/managers/devices/networkSettings/read|Ağ ayarlarını alır veya listeler|
|/managers/devices/networkSettings/write|Yeni bir oluşturur veya ağ ayarlarını güncelleştirir|
|/managers/devices/publicEncryptionKey/action|Aygıt Yöneticisi'ni listesi ortak şifreleme anahtarı|
|/managers/devices/publishSupportPackage/action|Destek paketi Microsoft Support sorun giderme için bir cihazın yayımlayın.|
|/managers/devices/read|Aygıtları alır veya listeler|
|/managers/devices/scanForUpdates/action|Bir cihaz güncelleştirmeleri için tarama.|
|/managers/devices/securitySettings/read|Güvenlik ayarları listesi|
|/managers/devices/securitySettings/syncRemoteManagementCertificate/action|Bir aygıtı için uzaktan yönetim sertifikası eşitleyin.|
|/managers/devices/securitySettings/update/action|Güvenlik ayarlarını güncelleştirin.|
|/managers/devices/securitySettings/write|Yeni bir oluşturur veya güvenlik ayarlarını güncelleştirir|
|/managers/devices/sendTestAlertEmail/action|Yapılandırılan e-posta alıcılara test uyarı e-posta gönderin.|
|/managers/devices/timeSettings/read|Saat ayarlarını alır veya listeler|
|/managers/devices/timeSettings/write|Yeni bir oluşturur veya saat ayarlarını güncelleştirir|
|/managers/devices/updateSummary/read|Güncelleştirme özeti alır veya listeler|
|/managers/devices/volumeContainers/delete|Var olan bir birim kapsayıcıları siler (yalnızca 8000 Series)|
|/managers/devices/volumeContainers/listEncryptionKeys/action|Birim kapsayıcıları şifreleme anahtarlarının listesi|
|/managers/devices/volumeContainers/metrics/read|Ölçümleri listesi|
|/managers/devices/volumeContainers/metricsDefinitions/read|Ölçümleri tanımları listesi|
|/managers/devices/volumeContainers/read|Birim kapsayıcıları listesi (yalnızca 8000 Series)|
|/managers/devices/volumeContainers/rolloverEncryptionKey/action|Geçiş şifreleme anahtarlarının birim kapsayıcıları|
|/managers/devices/volumeContainers/volumes/delete|Var olan birimler siler|
|/managers/devices/volumeContainers/volumes/metrics/read|Ölçümleri listesi|
|/managers/devices/volumeContainers/volumes/metricsDefinitions/read|Ölçümleri tanımları listesi|
|/managers/devices/volumeContainers/volumes/read|Birimleri listesi|
|/managers/devices/volumeContainers/volumes/write|Yeni bir oluşturur veya birimleri güncelleştirir|
|/managers/devices/volumeContainers/write|Yeni bir oluşturur veya güncelleştirir birim kapsayıcıları (yalnızca 8000 Series)|
|/managers/devices/write|Aygıtları güncelle|
|/managers/encryptionSettings/read|Şifreleme ayarları alır veya listeler|
|/Managers/extendedInformation/delete|Daha Fazla Bilgi Al işlemi, ?vault? türündeki Azure kaynağını temsil eden nesnenin Daha Fazla Bilgi değerini alır.|
|/Managers/extendedInformation/read|Daha Fazla Bilgi Al işlemi, ?vault? türündeki Azure kaynağını temsil eden nesnenin Daha Fazla Bilgi değerini alır.|
|/Managers/extendedInformation/write|Daha Fazla Bilgi Al işlemi, ?vault? türündeki Azure kaynağını temsil eden nesnenin Daha Fazla Bilgi değerini alır.|
|/managers/getActivationKey/action|Etkinleştirme anahtarı için Aygıt Yöneticisi'ni alın.|
|/managers/getEncryptionKey/action|Şifreleme anahtarı için Aygıt Yöneticisi'ni alma.|
|/managers/listActivationKey/action|StorSimple cihaz Yöneticisi'nin etkinleştirme anahtarı alır.|
|/managers/listPrivateEncryptionKey/action|Özel şifreleme anahtarı için bir StorSimple cihaz Yöneticisi alır.|
|/managers/listPublicEncryptionKey/action|Ortak şifreleme anahtarlarını bir StorSimple cihaz Yöneticisi'nin listeleyin.|
|/managers/Metrics/Read|Ölçümleri alır veya listeler|
|/managers/metricsDefinitions/read|Ölçüm tanımlarını alır veya listeler|
|/managers/provisionCloudAppliance/action|Yeni bir bulut uygulaması oluşturun.|
|/managers/read|Cihaz Yöneticileri alır veya listeler|
|/ Yöneticileri/okuma|Kasa alma işlemi Azure kaynak türü 'Kasası' temsil eden bir nesneyi alır|
|/managers/regenarateRegistationCertificate/action|Kayıt sertifikası için cihaz yöneticileri yeniden oluşturun.|
|/managers/regenerateActivationKey/action|Etkinleştirme anahtarı için Aygıt Yöneticisi'ni yeniden oluşturun.|
|/managers/storageAccountCredentials/delete|Depolama hesabı bilgilerini siler|
|/managers/storageAccountCredentials/listAccessKey/action|Depolama hesabı kimlik bilgileri listesi erişim tuşları|
|/managers/storageAccountCredentials/read|Depolama hesabı bilgilerini alır veya listeler|
|/managers/storageAccountCredentials/write|Depolama hesabı bilgilerini güncelle|
|/managers/storageDomains/delete|Depolama etki alanları siler|
|/managers/storageDomains/read|Depolama etki alanları alır veya listeler|
|/managers/storageDomains/write|Oluşturma veya güncelleme depolama etki alanları|
|/managers/write|Cihaz yöneticileri güncelle|
|/Managers/write|Kasa Oluştur işlemi, 'vault' türünde bir Azure kaynağı oluşturur|

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

| İşlem | Açıklama |
|---|---|
|/Locations/Quotas/Read|Okuma Stream Analytics abonelik kotası|
|/ operations/okuma|Okuma akış analizi işlemlerini|
|/Register/action|Stream Analytics kaynak sağlayıcının abonelik kaydı|
|/streamingjobs/Delete|Akış analizi işi Sil|
|/streamingjobs/functions/Delete|Stream Analytics işi işlevi Sil|
|/streamingjobs/functions/operationresults/Read|Stream Analytics işi işlevi için okuma işlemi sonuçları|
|/streamingjobs/Functions/Read|Okuma Stream Analytics işi işlevi|
|/streamingjobs/functions/RetrieveDefaultDefinition/action|Stream Analytics işi işlevi varsayılan tanımı alma|
|/streamingjobs/functions/Test/action|Test Stream Analytics işi işlevi|
|/streamingjobs/functions/Write|Stream Analytics işi işlevi yazma|
|/streamingjobs/inputs/Delete|Stream Analytics işi giriş silme|
|/streamingjobs/inputs/operationresults/Read|Stream Analytics işi giriş için okuma işlemi sonuçları|
|/streamingjobs/inputs/Read|Okuma Stream Analytics işi giriş|
|/streamingjobs/inputs/Sample/action|Örnek Stream Analytics işi giriş|
|/streamingjobs/inputs/Test/action|Test Stream Analytics işi giriş|
|/streamingjobs/inputs/Write|Stream Analytics işi giriş yazma|
|/streamingjobs/metricdefinitions/Read|Ölçüm tanımlarını oku|
|/streamingjobs/operationresults/Read|Akış analizi işi için okuma işlemi sonuçları|
|/streamingjobs/outputs/Delete|Stream Analytics işi çıkış Sil|
|/streamingjobs/outputs/operationresults/Read|Stream Analytics işi çıkış için okuma işlemi sonuçları|
|/streamingjobs/outputs/Read|Okuma Stream Analytics işi çıkış|
|/streamingjobs/outputs/Test/action|Test Stream Analytics işi çıkış|
|/streamingjobs/outputs/Write|Stream Analytics işi çıkış yazma|
|/streamingjobs/providers/Microsoft.Insights/diagnosticSettings/read|Tanılama ayarını okuyun.|
|/streamingjobs/providers/Microsoft.Insights/diagnosticSettings/write|Tanılama ayarını yazma.|
|/streamingjobs/providers/Microsoft.Insights/logDefinitions/read|Streamingjobs için kullanılabilir günlüklerini alır|
|/streamingjobs/providers/Microsoft.Insights/metricDefinitions/read|Streamingjobs için kullanılabilir ölçümleri alır|
|/streamingjobs/Read|Akış analizi işi okuma|
|/streamingjobs/Start/action|Akış analizi işi Başlat|
|/streamingjobs/Stop/action|Stream Analytics işini durdurma|
|/streamingjobs/transformations/Delete|Stream Analytics işi dönüştürme Sil|
|/streamingjobs/transformations/Read|Okuma Stream Analytics işi dönüştürme|
|/streamingjobs/transformations/Write|Stream Analytics işi dönüştürme yazma|
|/streamingjobs/Write|Akış analizi işi yazma|

## <a name="microsoftsubscription"></a>Microsoft.Subscription

| İşlem | Açıklama |
|---|---|
|/SubscriptionDefinitions/read|Bir Azure aboneliği tanımı bir yönetim grubu içinde alın.|
|/ SubscriptionDefinitions/yazma|Bir Azure aboneliği tanımı oluşturun|

## <a name="microsoftsupport"></a>Microsoft.Support

| İşlem | Açıklama |
|---|---|
|/register/action|Destek Kaynağı Sağlayıcısına Kayıt Yapar|
|/supportTickets/Read|Durum, önem derecesi, kişi ayrıntıları ve iletişimler gibi Destek Biletleri ayrıntılarını alır veya aboneliklerdeki Destek Biletleri listesini alır.|
|/ supportTickets/yazma|Oluşturur veya bir destek bileti güncelleştirir. Bir destek bileti oluşturabilirsiniz teknik için faturalama, kota veya abonelik yönetimi ile ilgili sorunlar. Önem derecesi, kişi ayrıntıları ve iletişimler için mevcut destek biletlerinin güncelleştirebilirsiniz.|

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights

| İşlem | Açıklama |
|---|---|
|/Environments/accesspolicies/DELETE|Erişim ilkesini siler.|
|/Environments/accesspolicies/Read|Bir erişim ilkesi özelliklerini alır.|
|/Environments/accesspolicies/Write|Bir ortam için yeni bir erişim ilkesi oluşturur veya mevcut bir erişim ilkesi güncelleştirir.|
|/Environments/DELETE|Ortam siler.|
|/Environments/eventsources/DELETE|Olay kaynağı siler.|
|/Environments/eventsources/eventsources/providers/Microsoft.Insights/ diagnosticSettings/yazma|Kaynak için tanılama ayarını oluşturur veya güncelleştirir|
|/Environments/eventsources/providers/Microsoft.Insights/diagnosticSettings/Read|Kaynak için tanılama ayarını alır|
|/Environments/eventsources/providers/Microsoft.Insights/metricDefinitions/Read|Eventsources için kullanılabilir ölçümleri alır|
|/Environments/eventsources/Read|Bir olay kaynağı özelliklerini alır.|
|/Environments/eventsources/Write|Bir ortam için yeni bir olay kaynağı oluşturur veya varolan bir olay kaynağına güncelleştirir.|
|/Environments/providers/Microsoft.Insights/diagnosticSettings/Read|Kaynak için tanılama ayarını alır|
|/Environments/providers/Microsoft.Insights/diagnosticSettings/Write|Kaynak için tanılama ayarını oluşturur veya güncelleştirir|
|/Environments/providers/Microsoft.Insights/metricDefinitions/Read|Ortamlar için kullanılabilir ölçümleri alır|
|/Environments/Read|Bir ortam özelliklerini alır.|
|/Environments/referencedatasets/DELETE|Başvuru veri kümesini siler.|
|/Environments/referencedatasets/Read|Bir başvuru veri kümesinin özelliklerini alın.|
|/Environments/referencedatasets/Write|Bir ortam için yeni bir başvuru veri kümesi oluşturur veya varolan bir başvuru veri kümesini güncelleştirir.|
|/Environments/Status/Read|Ortam, giriş gibi ilişkili işlemlerinin durumunu durumunu alın.|
|/ ortamları/yazma|Yeni bir ortam oluşturur veya mevcut ortamında güncelleştirir.|
|/register/action|Zaman serisi Öngörüler kaynak sağlayıcısı için aboneliği kaydeder ve zaman serisi Öngörüler ortamları oluşturulmasını sağlar.|

## <a name="microsoftweb"></a>microsoft.web

| İşlem | Açıklama |
|---|---|
|/apimanagementaccounts/apiacls/Read|API yönetim hesapları Apiacls alın.|
|/apimanagementaccounts/apis/apiacls/DELETE|API yönetim hesapları API'leri Apiacls silin.|
|/apimanagementaccounts/apis/apiacls/Read|API yönetim hesapları API'leri Apiacls alın.|
|/apimanagementaccounts/apis/apiacls/Write|API yönetim hesapları API'leri Apiacls güncelleştirin.|
|/apimanagementaccounts/apis/connectionacls/Read|API yönetim hesapları API'leri Connectionacls alın.|
|/apimanagementaccounts/apis/connections/confirmconsentcode/action|Onay kodu API yönetim hesapları API'leri bağlantıları onaylayın.|
|/apimanagementaccounts/apis/Connections/connectionacls/DELETE|API yönetim hesapları API'leri bağlantıları Connectionacls silin.|
|/apimanagementaccounts/apis/Connections/connectionacls/Read|API yönetim hesapları API'leri bağlantıları Connectionacls alın.|
|/apimanagementaccounts/apis/connections/connectionacls/write|API yönetim hesapları API'leri bağlantıları Connectionacls güncelleştirin.|
|/apimanagementaccounts/apis/connections/delete|API yönetim hesapları API'leri bağlantıları silin.|
|/apimanagementaccounts/apis/connections/getconsentlinks/action|API yönetim hesapları API'leri bağlantıları için onay bağlantılar alın.|
|/apimanagementaccounts/apis/connections/listconnectionkeys/action|Liste bağlantı anahtarları API yönetim hesapları API'leri bağlantılar.|
|/apimanagementaccounts/apis/Connections/listsecrets/Action|Liste gizli API yönetim hesapları API'leri bağlantıları.|
|/apimanagementaccounts/apis/Connections/Read|API yönetim hesapları API'leri bağlantıları alırlar.|
|/apimanagementaccounts/apis/connections/write|API yönetim hesapları API'leri bağlantıları güncelleştirin.|
|/apimanagementaccounts/apis/delete|API yönetim hesapları API'ları silin.|
|/apimanagementaccounts/apis/localizeddefinitions/DELETE|API Management silme hesapları API'leri yerelleştirilmiş tanımları.|
|/apimanagementaccounts/apis/localizeddefinitions/Read|API Management alma hesapları API'leri yerelleştirilmiş tanımları.|
|/apimanagementaccounts/apis/localizeddefinitions/Write|Güncelleştirme API yönetim hesapları API'leri tanımları yerelleştirilmiş.|
|/apimanagementaccounts/apis/Read|API yönetim hesapları API'leri alın.|
|/apimanagementaccounts/apis/Write|API yönetim hesapları API'leri güncelleştirin.|
|/apimanagementaccounts/connectionacls/Read|API yönetim hesapları Connectionacls alın.|
|/availablestacks/Read|Kullanılabilir yığınları alın.|
|/billingmeters/Read|Metre faturalama listesini alın.|
|/certificates/Delete|Varolan bir sertifikayı silin.|
|/ Sertifika/okuma|Sertifikaların listesini alın.|
|/ Sertifika/yazma|Yeni bir sertifika ekleyin veya mevcut bir güncelleştirin.|
|/checknameavailability/Read|Kaynak adı kullanılabilir olup olmadığını denetleyin.|
|/classicmobileservices/Read|Klasik mobil hizmet edinebilirsiniz.|
|/connectionGateways/Delete|Bir bağlantı ağ geçidi siler.|
|/connectionGateways/Join/Action|Bir bağlantı ağ geçidi birleştirir.|
|/connectiongateways/liststatus/action|Liste durumu bağlantı ağ geçidi.|
|/connectionGateways/ListStatus/Action|Bir bağlantı ağ geçidi durumunu listeler.|
|/connectionGateways/Move/Action|Bir bağlantı ağ geçidi taşır.|
|/connectionGateways/Read|Bağlantı ağ geçidi listesi alın.|
|/connectionGateways/Write|Oluşturur veya bir bağlantı ağ geçidi güncelleştirir.|
|/connections/confirmconsentcode/action|Bağlantıları onay kodunu onaylayın.|
|/ bağlantıları/Sil|Bir bağlantı siler.|
|/Connections/Join/Action|Bir bağlantı birleştirir.|
|/connections/listconsentlinks/action|Bağlantılar için liste onay bağlantılar.|
|/Connections/Move/Action|Bir bağlantıyı taşır.|
|/ bağlantıları/okuma|Bağlantıların listesini alır.|
|/ bağlantıları/yazma|Oluşturur veya bağlantı güncelleştirir.|
|/customApis/Delete|Özel bir API siler.|
|/customApis/extractApiDefinitionFromWsdl/Action|API tanımı WSDL ayıklar.|
|/customApis/Join/Action|Özel bir API birleştirir.|
|/customApis/listWsdlInterfaces/Action|WSDL arabirimleri için bir özel API listeler.|
|/customApis/Move/Action|Özel bir API taşır.|
|/customApis/Read|Özel API listesini alın.|
|/customApis/Write|Oluşturur veya özel API güncelleştirir.|
|/deploymentlocations/read|Dağıtım konumlarında alın.|
|/geoRegions/Read|Coğrafi bölgelerin listesini alın.|
|/hostingenvironments/capacities/Read|Ortamlar kapasiteleri barındırma alın.|
|/hostingEnvironments/Delete|Bir uygulama hizmeti ortamını silme|
|/hostingenvironments/Diagnostics/Read|Ortamlar tanılama barındırma alın.|
|/hostingenvironments/inboundnetworkdependenciesendpoints/read|Tüm gelen bağımlılıkları ağ uç noktalarına alın.|
|/hostingenvironments/metricdefinitions/Read|Ortamlar ölçüm tanımlarını barındırma alın.|
|/hostingenvironments/multirolepools/metricdefinitions/Read|Ortamlar birden havuzları ölçüm tanımlarını barındırma alın.|
|/hostingenvironments/multirolepools/Metrics/Read|Ortamlar birden havuzları ölçümleri barındırma alın.|
|/hostingEnvironments/multiRolePools/providers/Microsoft.Insights/ metricDefinitions/okuma|Uygulama hizmeti ortamı MultiRole için kullanılabilir ölçümleri alır|
|/hostingEnvironments/multiRolePools/Read|Bir uygulama hizmeti ortamı ön uç havuzunda özelliklerini alır|
|/hostingenvironments/multirolepools/skus/Read|Ortamlar birden havuzları SKU'ları barındırma alın.|
|/hostingenvironments/multirolepools/usages/Read|Ortamlar birden havuzları kullanımları barındırma alın.|
|/hostingEnvironments/multiRolePools/Write|Uygulama hizmeti ortamı'nda yeni bir ön uç havuzu oluşturmak veya mevcut bir güncelleştirme|
|/hostingenvironments/Operations/Read|Ortamlar işlemleri barındırma alın.|
|/hostingenvironments/outboundnetworkdependenciesendpoints/read|Ağ uç noktalarına giden tüm bağımlılıkları alın.|
|/hostingenvironments/providers/Microsoft.Insights/diagnosticSettings/Read|Kaynak için tanılama ayarını alır|
|/hostingenvironments/providers/Microsoft.Insights/diagnosticSettings/write|Kaynak için tanılama ayarını oluşturur veya güncelleştirir|
|/ hostingEnvironments/okuma|Bir uygulama hizmeti ortamı özelliklerini alır|
|/hostingEnvironments/reboot/Action|Uygulama hizmeti ortamı'nda tüm makineleri yeniden başlatın|
|/hostingenvironments/Resume/Action|Barındırma ortamları sürdürün.|
|/hostingenvironments/serverfarms/read|Barındırma ortamları uygulama hizmeti planları alın.|
|/hostingenvironments/Sites/Read|Barındırma ortamları Web uygulamaları alın.|
|/hostingenvironments/suspend/Action|Barındırma ortamları askıya alın.|
|/hostingenvironments/usages/Read|Ortamlar kullanımları barındırma alın.|
|/hostingenvironments/workerpools/metricdefinitions/Read|Ortamlar Workerpools ölçüm tanımlarını barındırma alın.|
|/hostingenvironments/workerpools/Metrics/Read|Ortamlar Workerpools ölçümleri barındırma alın.|
|/hostingEnvironments/workerPools/providers/Microsoft.Insights/metricDefinitions/Read|Uygulama hizmeti ortamı WorkerPool için kullanılabilir ölçümleri alır|
|/hostingEnvironments/workerPools/Read|Bir uygulama hizmeti ortamı çalışan havuzunda özelliklerini alır|
|/hostingenvironments/workerpools/skus/Read|Ortamlar Workerpools SKU'ları barındırma alın.|
|/hostingenvironments/workerpools/usages/Read|Ortamlar Workerpools kullanımları barındırma alın.|
|/hostingEnvironments/workerPools/Write|Uygulama hizmeti ortamı'nda yeni bir çalışan havuzu oluşturmak veya mevcut bir güncelleştirme|
|/hostingEnvironments/Write|Yeni bir uygulama hizmeti ortamı oluşturma veya varolan bir güncelleştirme|
|/ishostingenvironmentnameavailable/Read|Barındırma ortamı adı olup olmadığını alır.|
|/ishostnameavailable/Read|Ana bilgisayar adı kullanılabilir olup olmadığını denetleyin.|
|/isusernameavailable/Read|Kullanıcı adı kullanılabilir olup olmadığını denetleyin.|
|/listSitesAssignedToHostName/Read|Ana bilgisayar adı için atanmış siteler adlarını alır.|
|/Locations/apioperations/Read|Konumları API işlemleri alın.|
|/locations/connectiongatewayinstallations/read|Konumları bağlantı ağ geçidi yüklemeleri alın.|
|/locations/extractapidefinitionfromwsdl/action|API tanımı konumları için WSDL ayıklayın.|
|/locations/listwsdlinterfaces/action|Konumların WSDL arabirimlerini listele.|
|/locations/managedapis/apioperations/read|Konumları yönetilen API işlemleri alın.|
|/locations/managedapis/Join/Action|Yönetilen bir API birleştirir.|
|/locations/managedapis/read|Konumları yönetilen API'ler alın.|
|/Operations/Read|İşlemleri alın.|
|/publishingusers/read|Kullanıcılar yayımlanıyor alın.|
|/publishingusers/write|Kullanıcılar yayımlanıyor güncelleştirin.|
|/ önerileri/okuma|Abonelikler için öneriler listesi alınamadı.|
|/register/action|Microsoft.Web kaynak sağlayıcısı abonelik için kaydolun.|
|/resourcehealthmetadata/read|Kaynak sistem durumu meta veri alın.|
|/serverfarms/capabilities/read|App Service planları özelliklerini alın.|
|/serverfarms/Delete|Var olan bir uygulama hizmeti planı silme|
|/serverfarms/firstpartyapps/settings/delete|Uygulama hizmeti planları birinci taraf uygulama ayarları silin.|
|/serverfarms/firstpartyapps/settings/read|Uygulama hizmeti planları birinci taraf uygulama ayarları alın.|
|/serverfarms/firstpartyapps/settings/write|Uygulama hizmeti planları birinci taraf uygulama ayarları güncelleştirin.|
|/serverfarms/hybridconnectionnamespaces/relays/delete|Uygulama hizmeti planları karma bağlantı ad alanları geçişler silin.|
|/serverfarms/hybridconnectionnamespaces/relays/read|Uygulama hizmeti planları karma bağlantı ad alanları geçişler alın.|
|/serverfarms/hybridconnectionnamespaces/relays/sites/read|Uygulama hizmeti planları karma bağlantı ad alanları geçişler Web uygulamalarını alır.|
|/serverfarms/hybridconnectionplanlimits/read|Uygulama hizmeti planları karma bağlantı planı sınırlarını alın.|
|/serverfarms/hybridconnectionrelays/read|Uygulama hizmeti planları karma bağlantı geçişler alın.|
|/serverfarms/metricdefinitions/read|Uygulama hizmeti planları ölçüm tanımlarını alın.|
|/serverfarms/metrics/read|Uygulama hizmeti planları ölçümlerini alın.|
|/serverfarms/operationresults/read|Uygulama hizmeti planları İşlem sonuçlarını alır.|
|/serverfarms/providers/Microsoft.Insights/diagnosticSettings/read|Kaynak için tanılama ayarını alır|
|/serverfarms/providers/Microsoft.Insights/diagnosticSettings/write|Kaynak için tanılama ayarını oluşturur veya güncelleştirir|
|/serverfarms/providers/Microsoft.Insights/metricDefinitions/Read|Uygulama hizmeti planı için kullanılabilir ölçümleri alır|
|/serverfarms/Read|Bir uygulama hizmeti planı üzerinde özelliklerini alma|
|/serverfarms/restartSites/Action|Tüm Web uygulamaları bir uygulama hizmeti planı'nda yeniden başlatın.|
|/serverfarms/sites/read|Uygulama hizmeti planları Web uygulamalarını alır.|
|/serverfarms/skus/read|Uygulama hizmeti planları SKU'ları alır.|
|/serverfarms/usages/read|Uygulama hizmeti planları kullanımları alın.|
|/serverfarms/virtualnetworkconnections/gateways/write|Uygulama hizmeti planları sanal ağ bağlantıları Gateway bileşenlerini güncelleştirin.|
|/serverfarms/virtualnetworkconnections/read|Uygulama hizmeti planları sanal ağ bağlantıları alın.|
|/serverfarms/virtualnetworkconnections/routes/delete|Uygulama hizmeti planları sanal ağ bağlantıları yolları silin.|
|/serverfarms/virtualnetworkconnections/routes/read|Uygulama hizmeti planları sanal ağ bağlantıları rotaları alabilir.|
|/serverfarms/virtualnetworkconnections/routes/write|Uygulama hizmeti planları sanal ağ bağlantıları yolları güncelleştirin.|
|/serverfarms/workers/reboot/action|Uygulama hizmeti planları çalışanları yeniden başlatın.|
|/serverfarms/Write|Yeni bir uygulama hizmeti planı oluşturma veya var olan bir güncelleştirme|
|/sites/analyzecustomhostname/read|Özel ana bilgisayar adını analiz edin.|
|/sites/applySlotConfig/Action|Itanium tabanlı sistemler için Web uygulaması yuvası yapılandırması hedef yuvadan geçerli web uygulaması için geçerlidir|
|/Sites/Backup/Action|Yeni bir web uygulaması yedekleme oluşturma|
|/ Siteler/yedekleme/okuma|Web uygulamaları yedek alın.|
|/Sites/Backup/Write|Web uygulamaları yedekleme güncelleştirin.|
|/Sites/Backups/DELETE|Web uygulama yedeklemeler silin.|
|/sites/backups/list/action|Liste Web Apps yedeklemeler.|
|/Sites/Backups/Read|Web uygulamanızın yedekleme özelliklerini alır|
|/sites/backups/restore/action|Web uygulamaları yedekleri geri yükleyin.|
|/sites/config/delete|Web uygulamaları Config silin.|
|/sites/config/list/Action|Kimlik bilgileri, uygulama ayarlarının ve bağlantı dizeleri yayımlama gibi Web uygulamanızın hassas, güvenlik ayarlarını Listele|
|/Sites/config/Read|Web uygulaması yapılandırma ayarlarını al|
|/sites/config/Write|Web uygulamanızın yapılandırma ayarlarını güncelleştirme|
|/sites/continuouswebjobs/delete|Web uygulamaları sürekli Web işleriniz silin.|
|/sites/continuouswebjobs/read|Web uygulamaları sürekli Web işleriniz alın.|
|/sites/continuouswebjobs/start/action|Web uygulamaları sürekli Web işleriniz başlatın.|
|/sites/continuouswebjobs/stop/action|Web uygulamaları sürekli Web işleriniz durdurun.|
|/ Siteler/Sil|Var olan bir Web uygulamasının Sil|
|/sites/deployments/delete|Web uygulamaları dağıtımları silin.|
|/sites/deployments/log/read|Web uygulamaları dağıtımları günlük alın.|
|/sites/deployments/read|Web uygulamaları dağıtımları alın.|
|/sites/deployments/write|Web uygulamaları dağıtımları güncelleştirin.|
|/sites/diagnostics/analyses/execute/Action|Web Apps tanılama analiz çalıştırın.|
|/Sites/Diagnostics/Analyses/Read|Web Apps tanılama analiz alın.|
|/sites/diagnostics/aspnetcore/read|Web Apps tanılama için ASP.NET Core uygulamasını alın.|
|/Sites/Diagnostics/autoheal/Read|Web Apps tanılama Autoheal alın.|
|/sites/diagnostics/deployment/read|Web Apps tanılama dağıtımı alın.|
|/sites/diagnostics/deployments/read|Web Apps tanılama dağıtımları alın.|
|/sites/diagnostics/detectors/execute/Action|Web Apps tanılama algılayıcısı çalıştırın.|
|/sites/diagnostics/detectors/read|Web Apps tanılama algılayıcısı alın.|
|/sites/diagnostics/failedrequestsperuri/read|Web Apps tanılama başarısız isteklerin URI başına alın.|
|/Sites/Diagnostics/frebanalysis/Read|Web Apps tanılama FREB analiz alın.|
|/Sites/Diagnostics/loganalyzer/Read|Web Apps tanılama günlük Çözümleyicisi alın.|
|/Sites/Diagnostics/Read|Web Apps tanılama kategorileri alın.|
|/sites/diagnostics/runtimeavailability/read|Web Apps tanılama çalışma zamanı kullanılabilirliğini alın.|
|/sites/diagnostics/servicehealth/read|Web Apps Tanılama Hizmeti durumunu alın.|
|/sites/diagnostics/sitecpuanalysis/read|Web Apps tanılama Site CPU analiz alın.|
|/sites/diagnostics/sitecrashes/read|Web Apps tanılama Site çökme (Crash) alın.|
|/Sites/Diagnostics/sitelatency/Read|Web Apps tanılama Site gecikme alın.|
|/Sites/Diagnostics/sitememoryanalysis/Read|Web Apps tanılama Site bellek analizi alın.|
|/sites/diagnostics/siterestartsettingupdate/read|Web Apps tanılama Site yeniden ayarı güncelleştirmesi alın.|
|/Sites/Diagnostics/siterestartuserinitiated/Read|Web Apps tanılama Site yeniden başlatma kullanıcı tarafından başlatılan alın.|
|/sites/diagnostics/siteswap/read|Web Apps tanılama Site takas alın.|
|/Sites/Diagnostics/THREADCOUNT/Read|Web Apps tanılama iş parçacığı sayısı alın.|
|/sites/diagnostics/workeravailability/read|Web Apps tanılama Workeravailability alın.|
|/Sites/Diagnostics/workerprocessrecycle/Read|Web Apps tanılama çalışan işlem geri dönüştürme alın.|
|/Sites/domainownershipidentifiers/Read|Web Apps etki alanı sahipliğini tanımlayıcıları alın.|
|/Sites/domainownershipidentifiers/Write|Web Apps etki alanı sahipliğini tanımlayıcıları güncelleştirin.|
|/Sites/Functions/Action|İşlevler Web uygulamaları.|
|/Sites/Functions/DELETE|Web uygulamaları işlevleri silin.|
|/Sites/Functions/listsecrets/Action|Liste gizli Web Apps işlevleri.|
|/Sites/Functions/masterkey/Read|Web uygulamaları işlevleri Masterkey alın.|
|/Sites/Functions/Read|Web uygulamaları işlevleri alın.|
|/Sites/Functions/Token/Read|Get Web Apps işlevleri belirteci.|
|/Sites/Functions/Write|Web uygulamaları işlevleri güncelleştirin.|
|/sites/hostnamebindings/delete|Web uygulamaları konak adı bağlamaları silin.|
|/sites/hostnamebindings/read|Web uygulamaları konak adı bağlamaları alın.|
|/sites/hostnamebindings/write|Web uygulamaları konak adı bağlamaları güncelleştirin.|
|/sites/hybridconnection/delete|Web uygulamaları karma bağlantısını silin.|
|/Sites/hybridconnection/Read|Web uygulamaları karma bağlantı alın.|
|/Sites/hybridconnection/Write|Web uygulamaları karma bağlantı güncelleştirin.|
|/Sites/hybridconnectionnamespaces/relays/DELETE|Web uygulamaları karma bağlantı ad alanları geçişler silin.|
|/Sites/hybridconnectionnamespaces/relays/listkeys/Action|Liste anahtarları Web Apps karma bağlantı ad alanları geçişler.|
|/Sites/hybridconnectionnamespaces/relays/Read|Web uygulamaları karma bağlantı ad alanları geçişler alın.|
|/Sites/hybridconnectionnamespaces/relays/Write|Web uygulamaları karma bağlantı ad alanları geçişler güncelleştirin.|
|/Sites/hybridconnectionrelays/Read|Web uygulamaları karma bağlantı geçişler alın.|
|/sites/instances/deployments/delete|Web uygulamaları örnekleri dağıtımları silin.|
|/sites/instances/deployments/read|Web uygulamaları örnekleri dağıtımları alın.|
|/Sites/instances/Extensions/log/Read|Web uygulamaları örnekleri uzantıları günlük alın.|
|/Sites/instances/Extensions/Read|Web uygulamaları örnekleri uzantıları alın.|
|/sites/instances/processes/delete|Web uygulamaları örnekleri işlemleri silin.|
|/Sites/instances/Processes/Read|Web uygulamaları örnekleri işlemleri alın.|
|/Sites/instances/Read|Web uygulamaları örnekleri alın.|
|/sites/listsyncfunctiontriggerstatus/action|Liste eşitleme işlevi tetikleyici durum Web uygulamaları.|
|/Sites/metricdefinitions/Read|Web uygulamaları ölçüm tanımlarını alın.|
|/Sites/Metrics/Read|Web uygulamaları ölçümlerini alın.|
|/sites/metricsdefinitions/read|Web uygulamaları ölçümleri tanımlarını alın.|
|/sites/migratemysql/action|MySql Web uygulamaları geçirme.|
|/sites/migratemysql/read|Get Web Apps Migrate MySql.|
|/sites/networktrace/action|Ağ izleme Web uygulamaları.|
|/Sites/NewPassword/Action|Newpassword Web Apps.|
|/Sites/operationresults/Read|Web Apps İşlem sonuçlarını alır.|
|/Sites/Operations/Read|Web uygulamaları işlemleri alın.|
|/Sites/perfcounters/Read|Web uygulamaları performans sayaçlarını alın.|
|/sites/premieraddons/delete|Web uygulamaları Premier alabilecekleri silin.|
|/sites/premieraddons/read|Web uygulamaları Premier alabilecekleri alın.|
|/sites/premieraddons/write|Web uygulamaları Premier alabilecekleri güncelleştirin.|
|/Sites/Processes/Read|Web uygulamaları işlemleri alın.|
|/sites/providers/Microsoft.Insights/diagnosticSettings/read|Kaynak için tanılama ayarını alır|
|/sites/providers/Microsoft.Insights/diagnosticSettings/write|Kaynak için tanılama ayarını oluşturur veya güncelleştirir|
|/sites/providers/Microsoft.Insights/metricDefinitions/Read|Web uygulaması için kullanılabilir ölçümleri alır|
|/sites/publiccertificates/delete|Web Apps ortak sertifikalarını silin.|
|/sites/publiccertificates/read|Web Apps ortak sertifikaları alın.|
|/sites/publiccertificates/write|Web Apps ortak sertifikaları güncelleştirin.|
|/sites/publish/Action|Bir Web uygulaması yayımlama|
|/sites/publishxml/Action|Bir Web uygulaması için profil xml yayımlama Al|
|/sites/publishxml/read|Get Web Apps Publishing XML.|
|/ Siteler/okuma|Bir Web uygulaması özelliklerini alır|
|/sites/recommendationhistory/read|Web uygulamaları öneri geçmişi alın.|
|/Sites/Recommendations/disable/Action|Web uygulamaları önerileri devre dışı bırakın.|
|/Sites/Recommendations/Read|Web uygulaması için öneriler listesi alınamadı.|
|/Sites/RECOVER/Action|Web uygulamaları kurtarın.|
|/sites/resetSlotConfig/Action|Web uygulaması yapılandırmasında Sıfırla|
|/sites/resourcehealthmetadata/read|Web uygulamaları kaynak sistem durumu meta veri alın.|
|/Sites/restart/Action|Bir Web uygulaması yeniden|
|/Sites/Restore/Read|Web uygulamaları geri alın.|
|/Sites/Restore/Write|Web uygulamaları geri yükleyin.|
|/sites/siteextensions/delete|Web uygulamaları Site uzantıları silin.|
|/Sites/siteextensions/Read|Web uygulamaları Site uzantılarını Al.|
|/Sites/siteextensions/Write|Web uygulamaları Site uzantıları güncelleştirin.|
|/sites/slots/analyzecustomhostname/read|Web alma uygulamaları yuvaları analiz özel ana bilgisayar adı.|
|/sites/slots/applySlotConfig/Action|Itanium tabanlı sistemler için Web uygulaması yuvası yapılandırması hedef yuvadan geçerli yuvası için geçerlidir.|
|/Sites/slots/Backup/Action|Yeni Web uygulaması yuvası yedeği oluştur.|
|/Sites/slots/Backup/Read|Web uygulamaları yuvaları yedek alın.|
|/Sites/slots/Backup/Write|Web uygulamaları yuvaları yedeklemeyi güncelleştirin.|
|/sites/slots/backups/delete|Web uygulama yuvaları yedeklemeler silin.|
|/sites/slots/backups/list/action|Liste Web Apps yuvaları yedeklemeler.|
|/Sites/slots/Backups/Read|Bir web uygulaması yuvaları yedekleme özelliklerini alır|
|/sites/slots/backups/restore/action|Web uygulamaları yuvaları yedekleri geri yükleyin.|
|/sites/slots/config/delete|Web uygulamaları yuvaları Config silin.|
|/Sites/slots/config/List/Action|Kimlik bilgileri, uygulama ayarlarının ve bağlantı dizeleri yayımlama gibi Web uygulama yuvanın hassas, güvenlik ayarlarını Listele|
|/Sites/slots/config/Read|Web uygulaması yuvanın yapılandırma ayarlarını al|
|/sites/slots/config/Write|Web uygulaması yuvanın yapılandırma ayarlarını güncelleştirme|
|/sites/slots/continuouswebjobs/delete|Web uygulamaları yuvaları sürekli Web işleriniz silin.|
|/sites/slots/continuouswebjobs/read|Web uygulamaları yuvaları sürekli Web işleriniz alın.|
|/sites/slots/continuouswebjobs/start/action|Web uygulamaları yuvaları sürekli Web işleriniz başlatın.|
|/sites/slots/continuouswebjobs/stop/action|Web uygulamaları yuvaları sürekli Web işleriniz durdurun.|
|/Sites/slots/DELETE|Var olan bir Web uygulaması yuvası Sil|
|/sites/slots/deployments/delete|Web uygulamaları yuvaları dağıtımları silin.|
|/sites/slots/deployments/log/read|Web uygulamaları yuvaları dağıtımları günlük alın.|
|/sites/slots/deployments/read|Web uygulamaları yuvaları dağıtımları alın.|
|/sites/slots/deployments/write|Web uygulamaları yuvaları dağıtımları güncelleştirin.|
|/sites/slots/diagnostics/analyses/execute/Action|Web uygulamaları yuvaları tanılama analiz çalıştırın.|
|/Sites/slots/Diagnostics/Analyses/Read|Web uygulamaları yuvaları tanılama analiz alın.|
|/sites/slots/diagnostics/aspnetcore/read|Web Apps yuvaları tanılama için ASP.NET Core uygulamasını alın.|
|/Sites/slots/Diagnostics/autoheal/Read|Web uygulamaları yuvaları tanılama Autoheal alın.|
|/sites/slots/diagnostics/deployment/read|Web uygulamaları yuvaları tanılama dağıtımı alın.|
|/sites/slots/diagnostics/deployments/read|Web uygulamaları yuvaları tanılama dağıtımları alın.|
|/sites/slots/diagnostics/detectors/execute/Action|Web uygulamaları yuvaları tanılama algılayıcısı çalıştırın.|
|/Sites/slots/Diagnostics/detectors/Read|Web uygulamaları yuvaları tanılama algılayıcısı alın.|
|/Sites/slots/Diagnostics/frebanalysis/Read|Web uygulamaları yuvaları tanılama FREB analiz alın.|
|/Sites/slots/Diagnostics/loganalyzer/Read|Web uygulamaları yuvaları tanılama günlük Çözümleyicisi alın.|
|/Sites/slots/Diagnostics/Read|Web Apps yuvaları tanılama alın.|
|/sites/slots/diagnostics/runtimeavailability/read|Web uygulamaları yuvaları tanılama çalışma zamanı kullanılabilirliğini alın.|
|/sites/slots/diagnostics/servicehealth/read|Web uygulamaları yuvaları tanılama hizmet durumunu Al|
|/Sites/slots/Diagnostics/sitecpuanalysis/Read|Web uygulamaları yuvaları tanılama Site CPU analiz alın.|
|/sites/slots/diagnostics/sitecrashes/read|Web uygulamaları yuvaları tanılama Site kilitlenme alın.|
|/Sites/slots/Diagnostics/sitelatency/Read|Web uygulamaları yuvaları tanılama Site gecikme alın.|
|/Sites/slots/Diagnostics/sitememoryanalysis/Read|Web uygulamaları yuvaları tanılama Site bellek analizi alın.|
|/sites/slots/diagnostics/siterestartsettingupdate/read|Web uygulamaları yuvaları tanılama Site yeniden ayarı güncelleştirmesi alın.|
|/Sites/slots/Diagnostics/siterestartuserinitiated/Read|Web uygulamaları yuvaları tanılama Site yeniden başlatma kullanıcı tarafından başlatılan alın.|
|/sites/slots/diagnostics/siteswap/read|Web uygulamaları yuvaları tanılama Site takas alın.|
|/Sites/slots/Diagnostics/THREADCOUNT/Read|Web uygulamaları yuvaları tanılama iş parçacığı sayısı alın.|
|/sites/slots/diagnostics/workeravailability/read|Web uygulamaları yuvaları tanılama Workeravailability alın.|
|/Sites/slots/Diagnostics/workerprocessrecycle/Read|Web uygulamaları yuvaları tanılama çalışan işlem geri dönüştürme alın.|
|/Sites/slots/domainownershipidentifiers/Read|Web uygulamaları yuvaları etki alanı sahipliğini tanımlayıcıları alın.|
|/sites/slots/hostnamebindings/delete|Web uygulamaları yuvaları konak adı bağlamaları silin.|
|/sites/slots/hostnamebindings/read|Web uygulamaları yuvaları konak adı bağlamaları alın.|
|/sites/slots/hostnamebindings/write|Web uygulamaları yuvaları konak adı bağlamaları güncelleştirin.|
|/Sites/slots/hybridconnection/DELETE|Web uygulamaları yuvaları karma bağlantısını silin.|
|/Sites/slots/hybridconnection/Read|Web uygulamaları yuvaları karma bağlantı alın.|
|/Sites/slots/hybridconnection/Write|Web uygulamaları yuvaları karma bağlantı güncelleştirin.|
|/Sites/slots/hybridconnectionnamespaces/relays/DELETE|Web uygulamaları yuvaları karma bağlantı ad alanları geçişler silin.|
|/Sites/slots/hybridconnectionnamespaces/relays/Write|Web uygulamaları yuvaları karma bağlantı ad alanları geçişler güncelleştirin.|
|/Sites/slots/hybridconnectionrelays/Read|Web uygulamaları yuvaları karma bağlantı geçişler alın.|
|/sites/slots/instances/deployments/read|Web uygulamaları yuvaları örnekleri dağıtımları alın.|
|/Sites/slots/instances/Processes/DELETE|Web uygulamaları yuvaları örnekleri işlemleri silin.|
|/Sites/slots/instances/Processes/Read|Web uygulamaları yuvaları örnekleri işlemleri alın.|
|/Sites/slots/instances/Read|Web uygulamaları yuvaları örnekleri alın.|
|/Sites/slots/metricdefinitions/Read|Web uygulamaları yuvaları ölçüm tanımlarını alın.|
|/Sites/slots/Metrics/Read|Web uygulamaları yuvaları ölçümlerini alın.|
|/sites/slots/migratemysql/read|Web alma uygulamaları yuvaları MySql geçirme.|
|/sites/slots/networktrace/action|Ağ izleme Web Apps yuvaları.|
|/Sites/slots/NewPassword/Action|#Newpassword Web Apps yuvaları.|
|/sites/slots/operationresults/read|Web uygulamaları yuvaları İşlem sonuçlarını alır.|
|/Sites/slots/Operations/Read|Web uygulamaları yuvaları işlemleri alın.|
|/Sites/slots/perfcounters/Read|Web uygulamaları yuvaları performans sayaçlarını alın.|
|/Sites/slots/phplogging/Read|Web uygulamaları yuvaları Phplogging alın.|
|/sites/slots/premieraddons/delete|Web uygulamaları yuvaları Premier alabilecekleri silin.|
|/sites/slots/premieraddons/read|Web uygulamaları yuvaları Premier alabilecekleri alın.|
|/sites/slots/premieraddons/write|Web uygulamaları yuvaları Premier alabilecekleri güncelleştirin.|
|/sites/slots/providers/Microsoft.Insights/diagnosticSettings/read|Kaynak için tanılama ayarını alır|
|/sites/slots/providers/Microsoft.Insights/diagnosticSettings/write|Kaynak için tanılama ayarını oluşturur veya güncelleştirir|
|/sites/slots/providers/Microsoft.Insights/metricDefinitions/Read|Web uygulaması yuvası için kullanılabilir ölçümleri alır|
|/Sites/slots/publiccertificates/Read|Web uygulamaları yuvaları ortak sertifikaları alın.|
|/sites/slots/publiccertificates/write|Sertifikaları oluşturun veya Web Apps yuvaları ortak güncelleştirin.|
|/Sites/slots/Publish/Action|Bir Web uygulaması yuvası yayımlama|
|/sites/slots/publishxml/Action|Web uygulaması yuvası için profil xml yayımlama Al|
|/Sites/slots/Read|Bir Web uygulaması dağıtım yuvası özelliklerini alır|
|/sites/slots/resetSlotConfig/Action|Web uygulaması yuvası yapılandırması Sıfırla|
|/sites/slots/resourcehealthmetadata/read|Web uygulamaları yuvaları kaynak sistem durumu meta verileri alın.|
|/Sites/slots/restart/Action|Bir Web uygulaması yuvası yeniden başlatın|
|/Sites/slots/Restore/Read|Web uygulamaları yuvaları geri alın.|
|/Sites/slots/Restore/Write|Web uygulamaları yuvaları geri yükleyin.|
|/sites/slots/siteextensions/delete|Web uygulamaları yuvaları Site uzantıları silin.|
|/Sites/slots/siteextensions/Read|Web uygulamaları yuvaları Site uzantılarını Al.|
|/Sites/slots/siteextensions/Write|Web uygulamaları yuvaları Site uzantıları güncelleştirin.|
|/Sites/slots/slotsdiffs/Action|Web uygulaması yuvaları arasındaki farklar yapılandırmasında Al|
|/Sites/slots/slotsswap/Action|Web uygulaması dağıtım yuvalarını değiştirme|
|/Sites/slots/snapshots/Read|Web uygulamaları yuvaları anlık görüntülerini alın.|
|/Sites/slots/sourcecontrols/DELETE|Web uygulaması yuvası'nın kaynak denetimini yapılandırma ayarlarını Sil|
|/Sites/slots/sourcecontrols/Read|Web uygulaması yuvası'nın kaynak denetimi yapılandırma ayarlarını al|
|/Sites/slots/sourcecontrols/Write|Web uygulaması yuvası'nın kaynak denetimini yapılandırma ayarlarını güncelleştirme|
|/Sites/slots/Start/Action|Bir Web uygulaması yuvası Başlat|
|/Sites/slots/Stop/Action|Bir Web uygulaması yuvası Durdur|
|/Sites/slots/Sync/Action|Eşitleme Web Apps yuvaları.|
|/sites/slots/triggeredwebjobs/delete|Web uygulamaları yuvası harekete WebJobs silin.|
|/sites/slots/triggeredwebjobs/read|Web uygulamaları yuvası harekete Web işlerini alma.|
|/sites/slots/triggeredwebjobs/run/action|Web uygulamaları yuvası harekete Web işleri'ni çalıştırın.|
|/Sites/slots/usages/Read|Web uygulamaları yuvaları kullanımları alın.|
|/sites/slots/virtualnetworkconnections/delete|Web uygulamaları yuvaları sanal ağ bağlantıları silin.|
|/sites/slots/virtualnetworkconnections/gateways/write|Web uygulamaları yuvaları sanal ağ bağlantıları Gateway bileşenlerini güncelleştirin.|
|/sites/slots/virtualnetworkconnections/read|Web uygulamaları yuvaları sanal ağ bağlantıları alın.|
|/sites/slots/virtualnetworkconnections/write|Web uygulamaları yuvaları sanal ağ bağlantıları güncelleştirin.|
|/sites/slots/webjobs/read|Web uygulamaları yuvaları WebJobs alın.|
|/Sites/slots/Write|Yeni bir Web uygulaması yuvası oluşturma veya var olan bir güncelleştirme|
|/Sites/slotsdiffs/Action|Web uygulaması yuvaları arasındaki farklar yapılandırmasında Al|
|/Sites/slotsswap/Action|Web uygulaması dağıtım yuvalarını değiştirme|
|/Sites/snapshots/Read|Web uygulamaları anlık görüntülerini alın.|
|/Sites/sourcecontrols/DELETE|Web uygulamanızın kaynak denetimini yapılandırma ayarlarını Sil|
|/Sites/sourcecontrols/Read|Web uygulamanızın kaynak denetimini yapılandırma ayarlarını al|
|/Sites/sourcecontrols/Write|Web uygulamanızın kaynak denetimini yapılandırma ayarlarını güncelleştirme|
|/Sites/Start/Action|Bir Web uygulaması başlatın|
|/sites/stop/Action|Bir Web uygulamasını Durdur|
|/Sites/Sync/Action|Eşitleme Web uygulamaları.|
|/sites/syncfunctiontriggers/action|Web uygulamaları için eşitleme işlevi tetikler.|
|/sites/triggeredwebjobs/delete|Web uygulamaları tetiklenen Web işleri silin.|
|/sites/triggeredwebjobs/history/read|Web uygulamaları tetiklenen WebJobs geçmişi alın.|
|/sites/triggeredwebjobs/read|Web uygulamaları tetiklenen Web işlerini alma.|
|/sites/triggeredwebjobs/run/action|Web uygulamaları tetiklenen WebJobs çalıştırın.|
|/Sites/usages/Read|Web uygulamaları kullanımları alın.|
|/sites/virtualnetworkconnections/delete|Web uygulamaları sanal ağ bağlantıları silin.|
|/sites/virtualnetworkconnections/gateways/read|Web uygulamaları sanal ağ bağlantıları ağ geçitleri alın.|
|/sites/virtualnetworkconnections/gateways/write|Web uygulamaları sanal ağ bağlantıları Gateway bileşenlerini güncelleştirin.|
|/sites/virtualnetworkconnections/read|Web uygulamaları sanal ağ bağlantıları alın.|
|/sites/virtualnetworkconnections/write|Web uygulamaları sanal ağ bağlantıları güncelleştirin.|
|/sites/webjobs/read|Get Web Apps WebJobs.|
|/ Siteler/yazma|Yeni bir Web uygulaması oluşturun veya var olan bir güncelleştirme|
|/skus/Read|SKU'ları alır.|
|/sourcecontrols/Read|Kaynak denetimleri alın.|
|/ sourcecontrols/yazma|Kaynak denetimleri güncelleştirin.|
|/unregister/action|Aboneliği için Microsoft.Web kaynak sağlayıcısı kaydını silin.|
|/validate/action|Doğrulayın.|
|/verifyhostingenvironmentvnet/action|Barındırma ortamı Vnet doğrulayın.|

## <a name="microsoftworkloadmonitor"></a>Microsoft.WorkloadMonitor

| İşlem | Açıklama |
|---|---|
|/Components/Read|Okuma işlemleri kaynakları|
|/healthInstances/read|Okuma işlemleri kaynakları|
|/ Operations/okuma|Okuma işlemleri kaynakları|
|/workloads/DELETE|Bir iş yükü kaynak siler|
|/workloads/Read|Bir iş yükü kaynak okur|
|/ iş yükleri/yazma|Bir iş yükü kaynak Yazar|

## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [özel bir rol oluşturmak](role-based-access-control-custom-roles.md).
- Gözden geçirme [yerleşik RBAC roller](role-based-access-built-in-roles.md).
- Erişim atamalarını yönetmeyi öğrenin [kullanıcı tarafından](role-based-access-control-manage-assignments.md) veya [kaynak tarafından](role-based-access-control-configure.md) 
- Bilgi nasıl [kaynakları eylemlerini denetlemek için etkinlik günlükleri görüntüleme](~/articles/azure-resource-manager/resource-group-audit.md)
