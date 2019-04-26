---
title: Azure İzleyici works nasıl gönüllü uyarılar geçiş aracının anlama
description: Uyarı Geçiş Aracı nasıl çalıştığını anlamak ve sorun yaşarsanız giderebilirsiniz.
author: snehithm
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 03/19/2018
ms.author: snmuvva
ms.subservice: alerts
ms.openlocfilehash: a45a0cff606bc854924d5da0841b26e1cb9031bb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60347609"
---
# <a name="understand-how-the-migration-tool-works"></a>Geçiş Aracı nasıl çalıştığını anlamak

Olarak [daha önce duyurulduğu gibi](monitoring-classic-retirement.md), Azure İzleyici'de klasik uyarılar, Temmuz 2019 ' kullanımdan. Geçiş Aracı, geçiş gönüllü olarak tetiklemek için Azure portalında kullanılabilir ve klasik uyarı kuralları kullanan müşteriler için kullanıma sunuluyor.

Bu makalede, gönüllü bir geçiş aracının nasıl çalıştığı ile anlatılmaktadır. Ayrıca, bazı yaygın sorunlar için düzeltme açıklar.

## <a name="which-classic-alert-rules-can-be-migrated"></a>Klasik hangi uyarı kuralları taşınabilir mi?

Neredeyse tüm Klasik uyarı kuralları, Aracı'nı kullanarak geçirilebilir, ancak bazı özel durumlar vardır. Aşağıdaki uyarı kuralları aracını kullanarak geçirilmeyecektir (veya Temmuz 2019 otomatik geçiş sırasında)

- Sanal makine Konuk ölçümleri (hem Windows hem de Linux) üzerinde Klasik uyarı kuralları. [Yeni ölçüm uyarıları bu uyarı kuralları yeniden oluşturmak nasıl rehberlerine göz atın](#guest-metrics-on-virtual-machines)
- Klasik depolama ölçümleri Klasik uyarı kuralları. [Yönergeler, Klasik depolama hesaplarını izleme üzerinde bakın](https://azure.microsoft.com/blog/modernize-alerting-using-arm-storage-accounts/)
- Bazı depolama hesabı ölçümleri Klasik uyarı kuralları. [Aşağıdaki ayrıntıları](#storage-account-metrics)

Aboneliğinizi Klasik bu tür bir kurallar varsa, kalan kurallar geçirilir ancak bu kurallar, el ile geçirilmesi gerekir. Otomatik geçişini sunamıyoruz gibi herhangi gibi mevcut Klasik ölçüm uyarıları Haziran kaydırmak için yeni uyarılar için zaman sağlamak amacıyla 2020 çalışmaya devam eder. Ancak, yeni Klasik uyarı post Haziran 2019 oluşturulabilir.

### <a name="guest-metrics-on-virtual-machines"></a>Sanal makinelerdeki Konuk ölçümleri

Yeni ölçüm uyarıları Konuk ölçümleri oluşturabilmek için konuk ölçümlerine Azure İzleyici özel ölçümler depoya gönderilmesi gerekir. Tanılama Ayarları'nda Azure Monitor havuzu etkinleştirmek için aşağıdaki yönergeleri izleyin.

- [Windows sanal makineler için konuk ölçümlerini etkinleştirme](collect-custom-metrics-guestos-resource-manager-vm.md)
- [Linux Vm'leri için konuk ölçümlerini etkinleştirme](https://docs.microsoft.com/azure/azure-monitor/platform/collect-custom-metrics-linux-telegraf)

Yukarıdaki adımları tamamladıktan sonra yeni ölçüm uyarıları Konuk ölçümleri oluşturulabilir. Yeni ölçüm uyarıları yeniden oluşturulması sonra Klasik uyarılar silinebilir.

### <a name="storage-account-metrics"></a>Depolama hesabı ölçümleri

Aşağıdaki ölçümler ile ilgili bu uyarılar dışında tüm Klasik depolama hesapları uyarılarda geçirilebilir:

- PercentAuthorizationError
- PercentClientOtherError
- Percentnetworkerror'da
- PercentServerOtherError
- PercentSuccess
- Percentthrottlingerror'da
- Percenttimeouterror'da
- AnonymousThrottlingError
- SASThrottlingError

Klasik bir uyarı kuralları yüzde ölçümlere ilişkin gerekecektir geçirilmesi göre [eski ve yeni depolama ölçümleri arasındaki eşlemeyi](https://docs.microsoft.com/azure/storage/common/storage-metrics-migration#metrics-mapping-between-old-metrics-and-new-metrics). Eşikleri mutlak bir yeni ölçüm kullanılabilir olduğu gibi uygun şekilde değiştirilmesi gerekir.

Aynı işlevselliği sağlayan birleştirilmiş ölçüm olan Klasik uyarı kuralları AnonymousThrottlingError ve SASThrottlingError iki yeni uyarılarına genişletecektir beklenmediğini bölünmesi gerekir. Eşiklerini uygun şekilde uyarlanabilen gerekecektir.

## <a name="roll-out-phases"></a>Sunum aşamaları

Geçiş Aracı aşamada Klasik uyarı kuralları kullanan müşterilere sunuluyor. **Abonelik sahipleri** abonelik aracını kullanarak geçirilmesi hazır olduğunda bir e-posta alırsınız.

> [!NOTE]
> Erken aşamalardaki aşamalardaki aracı sunulacaktır gibi aboneliklerinizi çoğunu henüz geçirilmesi hazır olmadığını görebilirsiniz.

Şu anda bir **alt** Abonelikleri, hangi **yalnızca** Klasik uyarı kuralları aşağıdaki kaynak türleri, geçiş için hazır olarak işaretlenmiş olması. Daha fazla kaynak türleri için destek, gelecek aşamalı olarak eklenir.

- Microsoft.apimanagement/Service
- Microsoft.batch/batchaccounts
- Microsoft.cache/redis
- Microsoft.DataFactory/datafactories
- Microsoft.dbformysql/servers
- Microsoft.dbforpostgresql/servers
- Microsoft.eventhub/namespaces
- Microsoft.Logic/workflows
- Microsoft.Network/applicationgateways
- Microsoft.network/dnszones
- Microsoft.Network/expressroutecircuits
- Microsoft.Network/loadbalancers
- Microsoft.Network/networkwatchers/connectionmonitors
- Microsoft.network/publicipaddresses
- Microsoft.network/trafficmanagerprofiles
- Microsoft.Network/virtualnetworkgateways
- Microsoft.Search/searchservices
- Microsoft.servicebus/namespaces
- Microsoft.streamanalytics/streamingjobs
- Microsoft.timeseriesinsights/Environments
- Microsoft.Web/hostingenvironments/workerpools
- Microsoft.web/serverfarms
- Microsoft.web/sites

## <a name="who-can-trigger-the-migration"></a>Kimin geçiş tetiklenebilir mi?

Yerleşik rolü olan herhangi bir kullanıcı, **izleme katılımcı** abonelik düzeyi geçiş tetiklemeniz mümkün olacaktır. Aşağıdaki izinlere sahip bir özel role sahip olan kullanıcılar, geçiş de tetikleyebilirsiniz:

- * / Okuma
- Microsoft.Insights/actiongroups/*
- Microsoft.Insights/AlertRules/*
- Microsoft.Insights/metricAlerts/*

## <a name="common-issues-and-remediations"></a>Sık karşılaşılan sorunlar ve düzeltmeler

Sonra [geçişini](alerts-using-migration-tool.md), geçiş işleminin tamamlanma size bildirmek için sağlanan e-posta adresleri kullanacağız veya sizin yapmanız gereken bir eylem varsa. Aşağıdaki bölümde bazı yaygın sorunlar ve bunları düzeltme konusunda açıklanmaktadır.

### <a name="validation-failed"></a>Doğrulama başarısız

Aboneliğinizdeki Klasik uyarı kuralları için son değişiklikler nedeniyle bazı abonelik geçirilemez. Bu geçici bir sorundur. Geçiş durumu başa geçtiğinde geçiş başlatabilirsiniz **geçiş için hazır** birkaç gün içinde.

### <a name="policyscope-lock-preventing-us-from-migrating-your-rules"></a>Bize kurallarınızı geçiş dan engelleyen ilke/kapsamı Kilitle

Geçişin bir parçası olarak yeni ölçüm uyarıları ve yeni eylem grubu oluşturulur ve klasik uyarı kuralları (yeni kural oluşturulduktan sonra) silinir. Ancak, bize kaynakları oluşturmasını engelleyen bir ilke veya kapsam kilit yok. İlke veya kapsam kilit bağlı olarak bazı veya tüm kuralları taşınamıyor. Kilit/ilke kapsamı geçici olarak kaldırarak bu sorunu çözmek ve geçişi yeniden tetikleyin.

## <a name="next-steps"></a>Sonraki adımlar

- [Geçiş aracını kullanma](alerts-using-migration-tool.md)
- [Geçiş için hazırlama](alerts-prepare-migration.md)
