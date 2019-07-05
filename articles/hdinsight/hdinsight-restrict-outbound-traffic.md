---
title: Azure HDInsight kümeleri için giden ağ trafiği kısıtlama yapılandırın
description: Azure HDInsight kümeleri için giden ağ trafiği kısıtlama yapılandırmayı öğrenin.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.topic: howto
ms.date: 05/30/2019
ms.openlocfilehash: af5ddd50556b493cddf27d1ebb766d9bf6105107
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67433425"
---
# <a name="configure-outbound-network-traffic-for-azure-hdinsight-clusters-using-firewall-preview"></a>Giden ağ trafiği için Güvenlik Duvarı (Önizleme) kullanarak Azure HDInsight kümelerini yapılandırma

Bu makalede, Azure Güvenlik Duvarı'nı kullanarak HDInsight kümenize giden trafiği güvenli hale getirmek adımları sağlar. Aşağıdaki adımlarda, bir Azure Güvenlik Duvarı var olan bir küme için yapılandırdığınız varsayılır. Yeni bir küme dağıtıyorsanız ve bir güvenlik duvarının arkasındaki ilk alt ağ ve HDInsight kümesi oluşturun ve ardından bu kılavuzdaki adımları izleyin.

## <a name="background"></a>Arka plan

Azure HDInsight kümeleri, genellikle kendi sanal ağda dağıtılır. Kümenin düzgün şekilde çalışabilmesi için ağ erişimi gerektiren hizmetler söz konusu sanal ağ dışında bağımlılıkları vardır.

Gelen trafik gerektiren birkaç bağımlılık vardır. Gelen yönetim trafiğinin bir güvenlik duvarı cihazı üzerinden gönderilemez. Bu trafiğe ait kaynak adresleri bilinen ve yayımlanan [burada](hdinsight-extend-hadoop-virtual-network.md#hdinsight-ip). Ağ güvenlik grubu (NSG) kuralları, kümeler için gelen trafiği güvenli hale getirmek için bilgiler de oluşturabilirsiniz.

HDInsight giden trafiği bağımlılıkları neredeyse tamamen arkasına statik IP adreslerine sahip değilseniz, FQDN ile tanımlanır. Statik adresler olmaması anlamına gelir ağ güvenlik grupları (Nsg'ler) giden trafiğin bir küme kilitlemek için kullanılamaz. Adresleri sıklıkta biri olamaz geçerli ad çözümlemesinin temel kurallarını ayarlama ve NSG kurallarını ayarlamaya yönelik kullanan, değiştirin.

Çözüm giden adresleri güvenliğini sağlamak için etki alanı adlarını temel alarak giden trafiği denetleyen bir güvenlik duvarı cihaz kullanmaktır. Azure güvenlik duvarı, hedef FQDN'sini üzerinde giden HTTP ve HTTPS trafiğini kısıtlayabilir veya [FQDN etiketleri](https://docs.microsoft.com/azure/firewall/fqdn-tags).

## <a name="configuring-azure-firewall-with-hdinsight"></a>HDInsight ile Azure güvenlik duvarı yapılandırma

Çıkan verileri, var olan HDInsight ile Azure güvenlik duvarı kilitlemek için adımların bir özeti verilmiştir:
1. Güvenlik Duvarı oluşturma.
1. Uygulama kuralları güvenlik duvarı ekleme
1. Ağ kurallarının güvenlik duvarı ekleyin.
1. Bir yönlendirme tablosu oluşturun.

### <a name="create-a-new-firewall-for-your-cluster"></a>Kümeniz için yeni bir güvenlik duvarı oluşturma

1. Adlı bir alt ağ oluşturma **AzureFirewallSubnet** kümenizin bulunduğu sanal ağ içinde. 
1. Yeni bir güvenlik duvarı oluşturma **Test FW01** içindeki adımları kullanarak [Öğreticisi: Dağıtma ve Azure Azure portalını kullanarak güvenlik duvarı yapılandırma](../firewall/tutorial-firewall-deploy-portal.md#deploy-the-firewall).

### <a name="configure-the-firewall-with-application-rules"></a>Güvenlik Duvarı ile uygulama kurallarını yapılandırma

Önemli iletişimleri gönderip kümeye izin veren bir uygulama kuralı koleksiyonu oluşturun.

Yeni Güvenlik Duvarı'nı seçin **Test FW01** Azure portalından. Tıklayın **kuralları** altında **ayarları** > **uygulama kuralı koleksiyonu** > **uygulama kuralı koleksiyonuekleme**.

![Başlık: Uygulama kuralı koleksiyon Ekle](./media/hdinsight-restrict-outbound-traffic/hdinsight-restrict-outbound-traffic-add-app-rule-collection.png)

Üzerinde **uygulama kuralı koleksiyonu ekleme** ekranında, aşağıdaki adımları tamamlayın:

1. Girin bir **adı**, **öncelik**, tıklatıp **izin** gelen **eylem** açılır menüsünde, aşağıdaki kuralları içinde girin**FQDN etiketler bölümü** :

   | **Name** | **Kaynak adresi** | **FQDN etiketi** | **Notlar** |
   | --- | --- | --- | --- |
   | Rule_1 | * | HDInsight ve Windows Update | HDI hizmetler için gerekli |

1. Aşağıdaki kuralları ekleme **hedef FQDN bölüm** :

   | **Name** | **Kaynak adresi** | **Protokol: bağlantı noktası** | **Hedef FQDN** | **Notlar** |
   | --- | --- | --- | --- | --- |
   | Rule_2 | * | https:443 | login.windows.net | Windows oturum açma etkinliği sağlar |
   | Rule_3 | * | https:443,http:80 | <storage_account_name.blob.core.windows.net> | Kümenizi WASB tarafından destekleniyorsa, bir kural için WASB ekleyin. YALNIZCA https kullanmak üzere bağlantıları emin ["güvenli aktarım gerekli"](https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer) depolama hesabı etkinleştirilir. |

1. **Ekle**'yi tıklatın.

   ![Başlık: Uygulama kuralı koleksiyonu ayrıntıları girin](./media/hdinsight-restrict-outbound-traffic/hdinsight-restrict-outbound-traffic-add-app-rule-collection-details.png)

### <a name="configure-the-firewall-with-network-rules"></a>Ağ kurallarıyla güvenlik duvarını yapılandırma

HDInsight kümenizi doğru şekilde yapılandırmak için ağ kuralları oluşturun.

1. Yeni Güvenlik Duvarı'nı seçin **Test FW01** Azure portalından.
1. Tıklayın **kuralları** altında **ayarları** > **ağ kural koleksiyonu** > **ağ kural koleksiyonu ekleme**.
1. Üzerinde **ağ kural koleksiyonu ekleme** ekranında, girin bir **adı**, **öncelik**, tıklatıp **izin** gelen **eylem** açılan menüsü.
1. Aşağıdaki kurallar oluşturma **IP adresleri** bölümü:

   | **Name** | **Protokolü** | **Kaynak adresi** | **Hedef adres** | **Hedef bağlantı noktası** | **Notlar** |
   | --- | --- | --- | --- | --- | --- |
   | Rule_1 | UDP | * | * | `123` | Zaman hizmeti |
   | Rule_2 | Tüm | * | DC_IP_Address_1, DC_IP_Address_2 | `*` | Kurumsal güvenlik paketi (ESP) kullanıyorsanız, bir ağ kuralı ESP kümeleri için AAD DS ile iletişim kurmasına olanak tanıyan IP adresleri bölümüne ekleyin. IP adreslerini AAD DS bölümündeki etki alanı denetleyicilerinin portalda bulabilirsiniz | 
   | Rule_3 | TCP | * | Data Lake depolama hesabınızın IP adresi | `*` | Azure Data Lake Storage kullanıyorsanız, ADLS Gen1 ve 2. nesil bir SNI sorunu gidermek için IP adresleri bölümünde ağ kuralı ekleyebilirsiniz. Bu seçeneği, büyük veri yüklerine daha yüksek maliyetleri neden bir güvenlik duvarı için trafiği yönlendirir ancak trafiği günlüğe kaydedilen ve güvenlik duvarı günlükleri olarak denetlenebilir. Data Lake Storage hesabınız için IP adreslerini belirler. Gibi bir powershell komutu kullanabilirsiniz `[System.Net.DNS]::GetHostAddresses("STORAGEACCOUNTNAME.blob.core.windows.net")` FQDN bir IP adresine çözümlenemedi.|
   | Rule_4 | TCP | * | * | `12000` | (İsteğe bağlı) Log Analytics kullanıyorsanız, bir ağ kuralı Log Analytics çalışma alanınız ile iletişimi etkinleştirmek için IP adresleri bölümüne oluşturun. |

1. Aşağıdaki kurallar oluşturma **hizmet etiketleri** bölümü:

   | **Name** | **Protokolü** | **Kaynak adresi** | **Hizmet etiketleri** | **Hedef bağlantı noktası** | **Notlar** |
   | --- | --- | --- | --- | --- | --- |
   | Rule_7 | TCP | * | SQL | `1433` | Ağ kuralı için SQL Server için hizmet uç noktaları, güvenlik duvarı atlayacaktır HDInsight alt ağda yapılandırılmış sürece, oturum ve SQL trafiğini denetleme olanak tanıyan SQL hizmet etiketleri bölümünde yapılandırın. |

1. Tıklayın **Ekle** , ağ kural koleksiyonu oluşturmayı tamamlamak için.

   ![Başlık: Uygulama kuralı koleksiyonu ayrıntıları girin](./media/hdinsight-restrict-outbound-traffic/hdinsight-restrict-outbound-traffic-add-network-rule-collection.png)

### <a name="create-and-configure-a-route-table"></a>Oluşturma ve bir yol tablosu yapılandırma

Bir yol tablosu ile aşağıdaki girdileri oluşturun:

1. Altı adresleri [bu gerekli HDInsight Yönetimi IP adreslerinin listesi](../hdinsight/hdinsight-extend-hadoop-virtual-network.md#hdinsight-ip) bir sonraki atlama ile **Internet**:
    1. Tüm bölgelerde tüm kümeler için dört IP adresleri
    1. Kümenin oluşturulduğu için bölge özgü iki IP adresi
1. IP adresi 0.0.0.0/0 sonraki atlama, Azure güvenlik duvarı özel IP adresi olan bir sanal gereç yolu.

Örneğin, "Orta ABD", ABD bölgesinde bir kümesi için rota tablosu yapılandırmak için aşağıdaki adımları kullanın:

1. Azure Portal’da oturum açın.
1. Azure güvenlik duvarınızı seçin **Test FW01**. Kopyalama **özel IP adresi** listelenen **genel bakış** sayfası. Bu örnekte kullanacağız bir **örnek 10.1.1.4 adresi**
1. Yeni bir yol tablosu oluşturun.
1. Tıklayın **yollar** altında **ayarları**.
1. Tıklayın **Ekle** aşağıdaki tabloda IP adresleri için yollar oluşturmak için.

| Yönlendirme adı | Adres ön eki | Sonraki atlama türü | Sonraki atlama adresi |
|---|---|---|---|
| 168.61.49.99 | 168.61.49.99/32 | Internet | NA |
| 23.99.5.239 | 23.99.5.239/32 | Internet | NA |
| 168.61.48.131 | 168.61.48.131/32 | Internet | NA |
| 138.91.141.162 | 138.91.141.162/32 | Internet | NA |
| 13.67.223.215 | 13.67.223.215/32 | Internet | NA |
| 40.86.83.253 | 40.86.83.253/32 | Internet | NA |
| 0.0.0.0 | 0.0.0.0/0 | Sanal gereç | 10.1.1.4 |

Rota tablosu yapılandırmasını tamamlayın:

1. Tıklayarak, HDInsight alt ağ için oluşturulan rota tablosunu atama **alt ağlar** altında **ayarları** ardından **ilişkilendirmek**.
1. Üzerinde **alt ağı ilişkilendir** ekranında, kümenizi içinde oluşturulan sanal ağ seçin ve **HDInsight alt** HDInsight kümeniz için kullanılır.
1. **Tamam** düğmesine tıklayın.

## <a name="edge-node-or-custom-application-traffic"></a>Kenar düğümüne veya özel uygulama trafiği

Yukarıdaki adımlar, kümenin bir sorun yaşanmadan çalışmaya izin verir. Yine de varsa edge düğümler üzerinde çalışan özel uygulamalarınızın uyum sağlamak için bağımlılıkları yapılandırmanız gerekir.

Uygulama bağımlılıkları tanımlanan ve Azure güvenlik duvarı veya yol tablosuna eklenir.

Asimetrik yönlendirme sorunlarını önlemek uygulama trafiği için rotalar oluşturulması gerekir.

Uygulamalarınızı diğer bağımlılıkları varsa, bunların Azure güvenlik duvarını eklenmesi gerekir. HTTP/HTTPS trafiğine izin vermek ve diğer her şey için kuralları ağ uygulama kuralları oluşturun.

## <a name="logging"></a>Günlüğe kaydetme

Azure güvenlik duvarı günlükleri için birkaç farklı depolama sistemleri gönderebilirsiniz. Yapılandırma yönergeleri için güvenlik duvarını, günlüğe kaydetme adımları için [Öğreticisi: Azure güvenlik duvarı günlükleri ve ölçümleri izleme](../firewall/tutorial-diagnostics.md).

Günlük verileri Log analytics'e ise günlük kurulumu tamamladıktan sonra aşağıdaki gibi bir sorgu ile engellenen trafik görüntüleyebilirsiniz:

```
AzureDiagnostics | where msg_s contains "Deny" | where TimeGenerated >= ago(1h)
```

Azure İzleyici günlüklerine ile Azure güvenlik duvarınızı tümleştirme önce tüm uygulama bağımlılıklarını, uyumlu olmadığında bir uygulama çalışma başlama yararlı olur. Azure İzleyici günlükleri hakkında daha fazla bilgi [Azure İzleyici'de günlük verileri](../azure-monitor/log-query/log-query-overview.md)

## <a name="access-to-the-cluster"></a>Kümeye erişim
Güvenlik Duvarı kurulumunu başarıyla atandıktan sonra iç uç nokta kullanabilirsiniz (`https://<clustername>-int.azurehdinsight.net`) sanal ağ içindeki Ambari'den erişmek için. Genel bir uç nokta kullanmak için (`https://<clustername>.azurehdinsight.net`) veya ssh uç noktası (`<clustername>-ssh.azurehdinsight.net`), yol tablonuz doğru rotalar ve NSG kuralları Kurulumu açıklanan assymetric yönlendirme sorunu önlemek için emin [burada](https://docs.microsoft.com/azure/firewall/integrate-lb).

## <a name="configure-another-network-virtual-appliance"></a>Başka bir ağ sanal Gereci yapılandırın

>[!Important]
> Aşağıdaki bilgiler **yalnızca** Azure güvenlik duvarı dışında bir ağ sanal Gereci (NVA) yapılandırmak isteyip istemediğinizi gerekli.

Önceki yönergeleri HDInsight kümenizden giden trafiği sınırlamak için Azure Güvenlik Duvarı'nı yapılandırmanıza yardımcı olur. Azure güvenlik duvarı, birçok önemli senaryoları için trafiğe izin verecek şekilde otomatik olarak yapılandırılır. Başka bir ağ sanal Gereci kullanmak istiyorsanız, birkaç ek özellik el ile yapılandırmanız gerekir. Şunları aklınızda tutun, ağ sanal Gereci yapılandırın:

* Hizmet uç noktası uyumlu Hizmetleri hizmet uç noktaları ile yapılandırılması gerekir.
* IP adresi, HTTP/S olmayan trafik için (TCP ve UDP trafiği) bağımlılıklardır.
* FQDN HTTP/HTTPS uç noktalarını NVA Cihazınızı yerleştirilebilir.
* Joker karakter HTTP/HTTPS uç noktaları niteleyicileri sayısına göre değişebilen bağımlılıklardır.
* HDInsight alt ağınız için oluşturduğunuz bir yol tablosu atayın.

### <a name="service-endpoint-capable-dependencies"></a>Hizmet uç noktası özellikli bağımlılıkları

| **Uç noktası** |
|---|
| Azure SQL |
| Azure Storage |
| Azure Active Directory |

#### <a name="ip-address-dependencies"></a>IP adresi bağımlılıkları

| **Uç noktası** | **Ayrıntılar** |
|---|---|
| \*:123 | NTP saat denetimi. Trafiği birden fazla uç nokta bağlantı noktası 123 iade edildiğinde |
| Yayımlanan IP'ler [burada](hdinsight-extend-hadoop-virtual-network.md#hdinsight-ip) | HDInsight hizmeti bunlar |
| AAD-DS özel IP'ler ESP için kümeleri |
| \*: KMS Windows etkinleştirme 16800 |
| \*Log Analytics için 12000 |

#### <a name="fqdn-httphttps-dependencies"></a>FQDN HTTP/HTTPS bağımlılıkları

>[!Important]
> Aşağıdaki listede yalnızca birkaç en önemli FQDN'lerin sağlar. Nva'nın yapılandırılması için FQDN'lerin tam bir listesini alabilirsiniz [bu dosyadaki](https://github.com/Azure-Samples/hdinsight-fqdn-lists/blob/master/HDInsightFQDNTags.json).

| **Uç noktası**                                                          |
|---|
| Azure.archive.ubuntu.com:80                                           |
| Security.ubuntu.com:80                                                |
| ocsp.msocsp.com:80                                                    |
| OCSP.digicert.com:80                                                  |
| wawsinfraprodbay063.blob.core.windows.net:443                         |
| kayıt defteri 1.docker.io:443                                              |
| auth.docker.io:443                                                    |
| production.cloudflare.docker.com:443                                  |
| download.docker.com:443                                               |
| us.archive.ubuntu.com:80                                              |
| download.Mono project.com:80                                          |
| Packages.treasuredata.com:80                                          |
| Security.ubuntu.com:80                                                |
| Azure.archive.ubuntu.com:80                                                |
| ocsp.msocsp.com:80                                                |
| OCSP.digicert.com:80                                                |

## <a name="next-steps"></a>Sonraki adımlar

* [Azure HDInsight sanal ağ mimarisi](hdinsight-virtual-network-architecture.md)
