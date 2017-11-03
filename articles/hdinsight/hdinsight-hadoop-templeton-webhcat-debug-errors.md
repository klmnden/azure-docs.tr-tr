---
title: "Anlama ve Hdınsight - Azure WebHCat hatalarını | Microsoft Docs"
description: "Nasıl üzere ortak tarafından WebHCat Hdınsight'ta döndürülen hataları ve bunların nasıl çözüleceği öğrenin."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 1b3d94b1-207d-4550-aece-21dc45485549
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/20/2017
ms.author: larryfr
ms.openlocfilehash: 1e4d72540a44f3b1838b6ed4dfad47dbe84489dc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="understand-and-resolve-errors-received-from-webhcat-on-hdinsight"></a>Anlama ve hdınsight'ta WebHCat gelen karşılaşılan hataları çözme

WebHCat Hdınsight ve bunların nasıl çözüleceği ile kullanırken karşılaşılan hataları hakkında bilgi edinin. WebHCat dahili olarak Visual Studio için Azure PowerShell gibi istemci tarafı araçları ve Data Lake araçları tarafından kullanılır.

## <a name="what-is-webhcat"></a>WebHCat nedir

[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) bir REST API için [HCatalog](https://cwiki.apache.org/confluence/display/Hive/HCatalog), tablo ve Hadoop için Depolama Yönetimi katmanı. WebHCat Hdınsight kümelerinde varsayılan olarak etkindir ve çeşitli araçları tarafından kullanılan iş göndermek için küme oturum açma olmadan iş durumu, vb. alın.

## <a name="modifying-configuration"></a>Yapılandırmasını değiştirme

> [!IMPORTANT]
> Yapılandırılan en aştığından bu belgede listelenen hataların bazıları oluşur. Bir değer değiştirebileceğiniz çözümleme adım değinmektedir, aşağıdakilerden birini değişiklik yapmak için kullanmanız gerekir:

* İçin **Windows** kümeleri: küme oluşturma sırasında değerini yapılandırmak için bir betik eylemi kullanın. Daha fazla bilgi için bkz: [betik eylemleri geliştirmeniz](hdinsight-hadoop-script-actions.md).

* İçin **Linux** kümeleri: kullanım Ambari (web veya REST API) değerini değiştirin. Daha fazla bilgi için bkz: [Ambari kullanarak Hdınsight yönetme](hdinsight-hadoop-manage-ambari.md)

> [!IMPORTANT]
> Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement).

### <a name="default-configuration"></a>Varsayılan yapılandırma

Aşağıdaki varsayılan değerler aşılırsa, WebHCat performansı düşebilir veya hatalara neden olabilir:

| Ayar | Neler yapar? | Varsayılan değer |
| --- | --- | --- |
| [yarn.Scheduler.Capacity.Maximum-uygulamalar][maximum-applications] |Maksimum sayıda eş zamanlı olarak etkin olabilir işi (Beklemede veya çalışan) |10,000 |
| [templeton.Exec.max yakalar][max-procs] |Eşzamanlı olarak hizmet isteklerinin sayısı |20 |
| [mapreduce.jobhistory.max yaş ms][max-age-ms] |İş Geçmişi gün sayısını korunur |7 gün |

## <a name="too-many-requests"></a>Çok fazla istek

**HTTP durum kodu**: 429

| Nedeni | Çözüm |
| --- | --- |
| (Varsayılan 20) dakika başına WebHCat tarafından sunulan en fazla eşzamanlı istek sınırı aşıldı |En fazla eş zamanlı istek sayısını birden değil gönderdiğiniz emin olmak için iş yükünü azaltın veya değiştirerek eşzamanlı istek sınırı artırmak `templeton.exec.max-procs`. Daha fazla bilgi için bkz: [değiştirme yapılandırma](#modifying-configuration) |

## <a name="server-unavailable"></a>Sunucu kullanılamıyor

**HTTP durum kodu**: 503

| Nedeni | Çözüm |
| --- | --- |
| Bu durum kodu küme için birincil ve ikincil HeadNode arasında yük devretme sırasında genellikle oluşur. |İki dakika bekleyin ve sonra işlemi yeniden deneyin |

## <a name="bad-request-content-could-not-find-job"></a>Hatalı istek içeriği: işi bulunamadı.

**HTTP durum kodu**: 400

| Nedeni | Çözüm |
| --- | --- |
| İş ayrıntılarını tarafından iş geçmişi temizleyici temizlendi |İş geçmişi için varsayılan saklama dönemi 7 gündür. Varsayılan saklama dönemi değiştirerek değiştirilebilir `mapreduce.jobhistory.max-age-ms`. Daha fazla bilgi için bkz: [değiştirme yapılandırma](#modifying-configuration) |
| İş bir yük devretmesi nedeniyle sonlandırıldı |İş gönderme iki dakikaya kadar yeniden dene |
| Geçersiz iş kimliği kullanıldı |İş kimliğini doğru olup olmadığını denetleyin |

## <a name="bad-gateway"></a>Hatalı ağ geçidi

**HTTP durum kodu**: 502

| Nedeni | Çözüm |
| --- | --- |
| İç çöp toplama WebHCat işlemi içinde gerçekleşen |Çöp toplama son veya WebHCat hizmetini yeniden başlatmak bekleyin |
| ResourceManager hizmetinden bir yanıt beklenirken zaman aşımı. Etkin uygulama sayısı yapılandırılan en büyük (varsayılan 10.000) olduğunda bu hata oluşabilir |Şu anda çalışan tamamlamak veya değiştirerek eşzamanlı iş sınırını artırmak için işi bekleyin `yarn.scheduler.capacity.maximum-applications`. Daha fazla bilgi için bkz: [değiştirme yapılandırma](#modifying-configuration) bölümü. |
| Tüm işleri aracılığıyla almaya [GET /jobs](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference+Jobs) çağrısı sırasında `Fields` ayarlanır`*` |Alamadı *tüm* iş ayrıntıları. Bunun yerine kullanın `jobid` yalnızca belirli iş kimliği büyük projeler için Ayrıntılar alınamadı. Ya da kullanmayın`Fields` |
| HeadNode yük devretme sırasında WebHCat hizmeti çalışmıyor |İki dakika bekleyin ve işlemi yeniden deneyin |
| WebHCat gönderilen 500'den fazla bekleyen iş |Daha fazla iş göndermeden önce şu anda bekleyen işler tamamlanana kadar bekleyin |

[maximum-applications]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.3/bk_system-admin-guide/content/setting_application_limits.html
[max-procs]: https://hive.apache.org/javadocs/hcat-r0.5.0/configuration.html
[max-age-ms]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.6.0/ds_Hadoop/hadoop-mapreduce-client/hadoop-mapreduce-client-core/mapred-default.xml
