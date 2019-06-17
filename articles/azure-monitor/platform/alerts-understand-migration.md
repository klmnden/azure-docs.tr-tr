---
title: Gönüllü bir geçiş aracı Azure İzleyici uyarılar için nasıl çalıştığını anlamak
description: Uyarılar Geçiş Aracı nasıl çalıştığını anlamak ve sorunlarını giderebilirsiniz.
author: snehithm
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 03/19/2018
ms.author: snmuvva
ms.subservice: alerts
ms.openlocfilehash: 9d872a6d753a206dcfb03761e50e5854db4f146e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67071602"
---
# <a name="understand-how-the-migration-tool-works"></a>Geçiş Aracı nasıl çalıştığını anlamak

Olarak [daha önce duyurulduğu gibi](monitoring-classic-retirement.md), Azure İzleyici'de klasik uyarılar, Eylül 2019 ' kullanımdan (başlangıçta Temmuz 2019 oluştu). Geçiş Aracı, Azure portalında Klasik uyarı kuralları kullanan ve kendilerini geçişini isteyen müşteriler için kullanılabilir.

Bu makalede, gönüllü bir geçiş aracının nasıl çalıştığı açıklanmaktadır. Ayrıca, çözümler için bazı yaygın sorunlar açıklanmaktadır.

> [!NOTE]
> Sunum geçiş aracının gecikme nedeniyle, o tarihten Klasik uyarılar geçiş yapıldı [31 Ağustos 2019'için Genişletilmiş](https://azure.microsoft.com/updates/azure-monitor-classic-alerts-retirement-date-extended-to-august-31st-2019/) ilk duyurulan tarihinden 30 Haziran 2019.

## <a name="which-classic-alert-rules-can-be-migrated"></a>Klasik hangi uyarı kuralları taşınabilir mi?

Araç, neredeyse tüm Klasik uyarı kuralları geçirebilirsiniz, ancak bazı özel durumlar vardır. Aşağıdaki uyarı kuralları aracını kullanarak (veya Eylül 2019 otomatik geçiş sırasında) geçirilmeyecektir:

- Sanal makine Konuk ölçümleri (hem Windows hem de Linux) üzerinde Klasik uyarı kuralları. Bkz: [yeni ölçüm uyarıları bu uyarı kuralları yeniden oluşturmak için yönergeler](#guest-metrics-on-virtual-machines) bu makalenin ilerleyen bölümlerinde.
- Klasik depolama ölçümleri Klasik uyarı kuralları. Bkz: [izleme Klasik depolama hesaplarınızı rehberlik](https://azure.microsoft.com/blog/modernize-alerting-using-arm-storage-accounts/).
- Bazı depolama hesabı ölçümleri Klasik uyarı kuralları. Bkz: [ayrıntıları](#storage-account-metrics) bu makalenin ilerleyen bölümlerinde.

Aboneliğinizi Klasik bu tür bir kurallar varsa, bunları el ile taşımanız gerekir. Otomatik geçişini sunamıyoruz çünkü bu türdeki tüm var olan ve klasik ölçüm uyarıları Haziran 2020'ye kadar çalışmaya devam eder. Bu uzantı, yeni uyarılar için kaydırmak için zaman verir. Bununla birlikte, Ağustos 2019 sonra yeni Klasik uyarı oluşturulabilir.

### <a name="guest-metrics-on-virtual-machines"></a>Sanal makinelerdeki Konuk ölçümleri

Konuk ölçümleri Konuk ölçümleri yeni ölçüm uyarıları oluşturmadan önce Azure İzleyici özel ölçümler depoya gönderilmesi gerekir. Tanılama Ayarları'nda Azure Monitor havuzu etkinleştirmek için aşağıdaki yönergeleri izleyin:

- [Windows sanal makineler için konuk ölçümlerini etkinleştirme](collect-custom-metrics-guestos-resource-manager-vm.md)
- [Linux Vm'leri için konuk ölçümlerini etkinleştirme](collect-custom-metrics-linux-telegraf.md)

Bu adımları tamamladıktan sonra konuk ölçümlerine yeni ölçüm uyarıları oluşturabilirsiniz. Ve yeni ölçüm uyarıları oluşturduktan sonra Klasik uyarıları silebilirsiniz.

### <a name="storage-account-metrics"></a>Depolama hesabı ölçümleri

Bu ölçümler ile ilgili uyarılar dışında tüm Klasik depolama hesapları uyarılarda geçirilebilir:

- PercentAuthorizationError
- PercentClientOtherError
- Percentnetworkerror'da
- PercentServerOtherError
- PercentSuccess
- Percentthrottlingerror'da
- Percenttimeouterror'da
- AnonymousThrottlingError
- SASThrottlingError
- ThrottlingError

Klasik uyarı yüzde ölçümlere ilişkin kuralları geçirilmelidir temel alarak [eski ve yeni depolama ölçümleri arasındaki eşlemeyi](https://docs.microsoft.com/azure/storage/common/storage-metrics-migration#metrics-mapping-between-old-metrics-and-new-metrics). Eşikleri mutlak bir yeni ölçüm kullanılabilir olduğu için uygun şekilde değiştirilmesi gerekir.

Aynı işlevselliği sağlayan birleştirilmiş ölçüm olduğundan AnonymousThrottlingError, SASThrottlingError ve ThrottlingError Klasik uyarı kuralları iki yeni uyarılarına genişletecektir bölmeniz gerekir. Eşiklerini uygun şekilde uyarlanabilen gerekecektir.

## <a name="rollout-phases"></a>Dağıtım aşamaları

Geçiş Aracı aşamada Klasik uyarı kuralları kullanan müşterilere sunuluyor. Abonelik sahipleri, abonelik aracını kullanarak geçirilmesi hazır olduğunda bir e-posta alırsınız.

> [!NOTE]
> Araç, aşamalı olarak sunulacaktır çünkü aboneliklerinizi çoğunu henüz erken aşamalarında geçirilmesi hazır olmadığını görebilirsiniz.

Şu anda bir alt kümesini abonelikler, geçiş için hazır olarak işaretlenir. Alt Klasik uyarı kurallarını yalnızca aşağıdaki kaynak türlerine sahip bu Aboneliklerdeki içerir. Daha fazla kaynak türleri için destek, gelecek aşamalı olarak eklenir.

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

İzleme katkıda bulunan yerleşik rolünün abonelik düzeyinde olan herhangi bir kullanıcı geçiş tetikleyebilirsiniz. Özel bir rol aşağıdaki izinlere sahip olan kullanıcılar, geçiş de tetikleyebilirsiniz:

- \* / Okuma
- Microsoft.Insights/actiongroups/*
- Microsoft.Insights/AlertRules/*
- Microsoft.Insights/metricAlerts/*

## <a name="common-problems-and-remedies"></a>Yaygın sorunlar ve çözümler

Çalıştırdıktan sonra [geçişini](alerts-using-migration-tool.md), geçişin tamamlandığını veya herhangi bir işlem yapmanız gerekip gerekmediğini bildirmek için sağlanan adreslerde e-posta alırsınız. Bu bölümde, bazı yaygın sorunlar ve bunlarla nasıl açıklanmaktadır.

### <a name="validation-failed"></a>Doğrulama başarısız oldu

Aboneliğinizdeki Klasik uyarı kuralları için son değişiklikler nedeniyle bazı abonelik geçirilemez. Bu sorun geçicidir. Geçiş durumu geri taşındıktan sonra geçişi yeniden başlatabilirsiniz **geçiş için hazır** birkaç gün içinde.

### <a name="policy-or-scope-lock-preventing-us-from-migrating-your-rules"></a>Bize kurallarınızı geçiş dan engelleyen ilke veya kapsam Kilitle

Geçişin bir parçası olarak yeni ölçüm uyarıları ve yeni eylem grubu oluşturulur ve klasik uyarı kuralları ardından silinir. Ancak, bize kaynakları oluşturmasını engelleyen bir ilke veya kapsam kilit yok. İlke veya kapsam kilit bağlı olarak bazı veya tüm kuralları taşınamıyor. Kapsam kilit ya da ilke geçici olarak kaldırılması ve geçişi yeniden harekete bu sorunu çözebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Geçiş aracını kullanma](alerts-using-migration-tool.md)
- [Geçiş için hazırlama](alerts-prepare-migration.md)
