---
title: HDInsight - Azure üzerinde WebHCat hatalarını anlama ve çözme
description: Nasıl üzere sık karşılaşılan WebHCat tarafından HDInsight üzerinde döndürdü ve bunların nasıl çözüleceğine öğrenin.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: hrasheed
ms.openlocfilehash: 683580ba65ad775ccec105c78cc1af66fbb63c37
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64691886"
---
# <a name="understand-and-resolve-errors-received-from-webhcat-on-hdinsight"></a>HDInsight üzerinde WebHCat alınan hatalarını anlama ve çözme

WebHCat HDInsight ve bunların nasıl çözüleceğine kullanırken alınan hatalar hakkında bilgi edinin. WebHCat dahili olarak Visual Studio için Azure PowerShell gibi istemci tarafı araçları ve Data Lake araçları tarafından kullanılır.

## <a name="what-is-webhcat"></a>WebHCat nedir

[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) bir REST API için [HCatalog](https://cwiki.apache.org/confluence/display/Hive/HCatalog), tablo ve Apache Hadoop için Depolama Yönetimi katmanı. WebHCat HDInsight kümelerinde varsayılan olarak etkindir ve çeşitli araçlarla işleri göndermek, küme oturum açmadan vb. iş durumunu almak için kullanılır.

## <a name="modifying-configuration"></a>Yapılandırmasını değiştirme

> [!IMPORTANT]  
> Bu belgede listelenen hataların bazıları, yapılandırılan en fazla aşıldığından oluşur. Bir değeri değiştirebilirsiniz. çözüm adım bahsetmeleri, aşağıdakilerden birini değişiklik yapmak için kullanmanız gerekir:

* İçin **Windows** kümeleri: Küme oluşturma sırasında değerini yapılandırmak için betik eylemi kullanın. Daha fazla bilgi için [betik eylemleri geliştirme](hdinsight-hadoop-script-actions-linux.md).

* İçin **Linux** kümeleri: Apache Ambari (web veya REST API) değerini değiştirmek için kullanın. Daha fazla bilgi için [Apache Ambari kullanarak HDInsight yönetme](hdinsight-hadoop-manage-ambari.md)

> [!IMPORTANT]  
> Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement).

### <a name="default-configuration"></a>Varsayılan yapılandırma

Aşağıdaki varsayılan değerler aşılırsa, WebHCat performansı düşebilir veya hatalara neden:

| Ayar | Ne yapar? | Varsayılan değer |
| --- | --- | --- |
| [yarn.scheduler.capacity.maximum-applications][maximum-applications] |Aynı anda etkin olabilen işlerin sayısı (Bekleyen veya çalışıyor) |10,000 |
| [templeton.exec.max-procs][max-procs] |Aynı anda hizmet isteklerinin sayısı |20 |
| [mapreduce.jobhistory.max-age-ms][max-age-ms] |İş geçmişini gün sayısını korunur |7 gün |

## <a name="too-many-requests"></a>Çok fazla istek var

**HTTP durum kodu**: 429

| Nedeni | Çözüm |
| --- | --- |
| Dakika başına (varsayılan 20) WebHCat tarafından sunulan en fazla eşzamanlı istek sınırı aşıldı |En fazla eş zamanlı istek sayısını birden çok değil gönderdiğiniz emin olmak için iş yükünüzü azaltmak veya değiştirerek eşzamanlı istek sınırı artırmak `templeton.exec.max-procs`. Daha fazla bilgi için [değiştirme yapılandırma](#modifying-configuration) |

## <a name="server-unavailable"></a>Sunucu kullanılamıyor

**HTTP durum kodu**: 503

| Nedeni | Çözüm |
| --- | --- |
| Bu durum kodu küme için birincil ve ikincil baş düğümüne arasında yük devretme sırasında genellikle gerçekleşir. |İki dakika bekleyin. ardından işlemi yeniden deneyin |

## <a name="bad-request-content-could-not-find-job"></a>Hatalı istek içeriği: İş bulunamadı

**HTTP durum kodu**: 400

| Nedeni | Çözüm |
| --- | --- |
| İş ayrıntılarını iş geçmişine göre Temizleyicisi temizlendi |İş geçmişi için varsayılan saklama süresi 7 gündür. Varsayılan saklama süresi değiştirerek değiştirilebilir `mapreduce.jobhistory.max-age-ms`. Daha fazla bilgi için [değiştirme yapılandırma](#modifying-configuration) |
| İş yük devretmesi nedeniyle sonlandırıldı |İş gönderme için iki dakikaya kadar yeniden dene |
| Geçersiz iş kimliği kullanıldı |İş Kimliğini doğru olup olmadığını denetleyin |

## <a name="bad-gateway"></a>Hatalı ağ geçidi

**HTTP durum kodu**: 502

| Nedeni | Çözüm |
| --- | --- |
| WebHCat işlem içinde iç atık toplama gerçekleşiyor |WebHCat hizmeti yeniden başlatın ya da son Çöp toplamayı beklemeniz |
| Zaman aşımı ResourceManager hizmetinden gelen yanıt bekleniyor. Etkin uygulama sayısı yapılandırılan üst sınırı (varsayılan 10.000) olduğunda bu hata oluşabilir |Şu anda çalışan işi tamamlamak ya da değiştirerek eş zamanlı iş sınırını artırmak için bekleyin `yarn.scheduler.capacity.maximum-applications`. Daha fazla bilgi için [değiştirme yapılandırma](#modifying-configuration) bölümü. |
| Tüm işleri aracılığıyla almaya çalışırken [GET /jobs](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference+Jobs) çağrısı sırasında `Fields` ayarlanır `*` |Alamadı *tüm* iş ayrıntıları. Bunun yerine kullanın `jobid` yalnızca belirli iş kimliği büyük iş ayrıntılarını almak için Ya da kullanmayın `Fields` |
| Baş düğüm yük devretme sırasında WebHCat hizmeti çalışmıyor |İki dakika bekleyin ve işlemi yeniden deneyin |
| WebHCat gönderilen 500'den fazla bekleyen iş yok |Daha fazla iş göndermeden önce şu anda bekleyen işler tamamlanana kadar bekleyin |

[maximum-applications]: https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.3/bk_system-admin-guide/content/setting_application_limits.html
[max-procs]: https://cwiki.apache.org/confluence/display/Hive/WebHCat+Configure#WebHCatConfigure-WebHCatConfiguration
[max-age-ms]: https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.6.0/ds_Hadoop/hadoop-mapreduce-client/hadoop-mapreduce-client-core/mapred-default.xml
