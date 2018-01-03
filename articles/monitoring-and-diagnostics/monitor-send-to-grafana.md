---
title: "Azure Hizmetleri ve uygulamaları Grafana kullanarak izleme | Microsoft Docs"
description: "İçinde Grafana görüntüleyebilmeniz rota Azure İzleyici ve Application Insights verileri."
services: monitoring-and-diagnostics
keywords: 
author: rboucher
ms.author: robb
ms.date: 11/06/2017
ms.topic: article
ms.service: monitoring-and-diagnostics
ms.openlocfilehash: 709a98f8bcdb75962f8e41de348ca7a41c677610
ms.sourcegitcommit: adf6a4c89364394931c1d29e4057a50799c90fc0
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/09/2017
---
# <a name="monitor-your-azure-services-in-grafana"></a>Azure hizmetlerinizi Grafana izleme
Şimdi de Azure Hizmetleri ve uygulamaları izleyebilirsiniz [Grafana](https://grafana.com/) kullanarak [Azure İzleyicisi veri kaynağı eklentisi](https://grafana.com/plugins/grafana-azure-monitor-datasource). Eklenti uygulama performansı verileri Application Insights SDK'sı tarafından toplanan ve bunun yanı sıra Azure İzleyici tarafından sağlanan altyapı verileri toplar. Bu veriler, ardından Grafana Panonuzda görüntüleyebilirsiniz.

Eklentisi şu anda önizlemede değil.

Azure Marketi'nden bir Grafana sunucusu kurma ve Azure İzleyici ve Application Insights ölçümünün panoları oluşturmak için aşağıdaki adımları kullanın.

## <a name="set-up-a-grafana-instance"></a>Bir Grafana örneği ayarlayın
1. Azure Marketi'nde gidin ve Grafana Labs tarafından Grafana seçin.

2. Adları ve ayrıntıları doldurun. Yeni bir kaynak grubu oluşturun. VM adı, VM parola ve Grafana server yönetici parolası için seçtiğiniz değerlere takip edin.  

3. VM boyutu ve bir depolama hesabı seçin.

4. Ağ Yapılandırması ayarlarını yapılandırın.

5. Özet görüntülemek ve seçim **oluşturma** kullanım koşullarını kabul sonra.

## <a name="log-in-to-grafana"></a>Grafana için oturum açın
1. Dağıtım tamamlandıktan sonra Seç **kaynak grubuna gidin**. Yeni oluşturulan kaynakların bir listesini görürsünüz. 

    ![Grafana kaynak grubu nesneleri](.\media\monitor-how-to-grafana\grafana1.png) 

    Ağ güvenlik grubu seçerseniz (*grafana nsg* bu durumda), bağlantı noktası 3000 Grafana sunucusuna erişmek için kullanılır görebilirsiniz. 

2. Kaynaklar ve select listesi geri gidin **genel IP adresi**. Bu ekranda bulunan değerleri kullanarak, yazın *http://<IP address>: 3000* veya  *<DNSName>: 3000* tarayıcınızda. Yalnızca yerleşik Grafana sunucu için bir oturum açma sayfası görmeniz gerekir.
    
    ![Grafana oturum açma ekranı](.\media\monitor-how-to-grafana\grafana2.png) 

3. Oturum kullanıcı adı olarak oturum *yönetici* ve daha önce oluşturduğunuz Grafana server yönetici parolası. 

## <a name="configure-data-source-plugin"></a>Veri kaynağı eklentisi yapılandırma

Başarıyla oturum açtı sonra Azure İzleyicisi veri kaynağı eklentisi zaten dahil olduğunu görmeniz gerekir.

![Azure İzleyici eklentisi Grafana gösterir](.\media\monitor-how-to-grafana\grafana3.png) 

1. Seçin **veri kaynağı Ekle** Azure İzleyici ve Application Insights yapılandırmak için. 
    
2. Veri kaynağı için bir ad seçin ve Seç **Azure İzleyici** açılır veri kaynağından farklı.
    
    
## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma 

Grafana Azure İzleyici API'leri bağlanmak ve ölçüm verilerini toplamak için bir Azure Active Directory hizmet asıl kullanır. Bir hizmet asıl Azure kaynaklarınıza erişimi yönetmek üzere oluşturmanız gerekir.

1. Bkz: [bu yönergeleri](../azure-resource-manager/resource-group-create-service-principal-portal.md) bir hizmet sorumlusu oluşturmak için. Kopyalayın ve Kiracı kimliği, istemci kimliği ve istemci parolasını kaydedin.

2. Bkz: [uygulama rol atama](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-create-service-principal-portal#assign-application-to-role) Azure Active Directory Uygulama okuyucu rolüne atamak için.   

3. Application Insights kullanırsanız, uygulama dayalı Öngörüler ölçümleri toplamak için Application Insights API'si ve uygulama Kimliğini de içerebilir. Daha fazla bilgi için bkz: [API anahtarı ve uygulama kimliği alma](https://dev.applicationinsights.io/documentation/Authorization/API-key-and-App-ID).

4. Tüm bu bilgilerini girdikten sonra Seç **kaydetmek** ve Grafana API sınar. Aşağıdakine benzer bir ileti görürsünüz.  

    ![Azure İzleyici eklentisi Grafana gösterir](.\media\monitor-how-to-grafana\grafana4.png) 
    
## <a name="build-a-grafana-dashboard"></a>Derleme Grafana Panosu

1. Giriş sayfasına gidin ve seçin **yeni Pano**.

2. Yeni panosunda seçin **grafik**. Diğer grafik seçenekleri deneyebilirsiniz ancak bu makale kullanır *grafik* bir örnek olarak. 

    ![Grafana yeni Pano](.\media\monitor-how-to-grafana\grafana5.png) 

3. Panonuzda boş bir grafik gösterir. 

4. Panel başlığına tıklayın ve **Düzenle** bu grafik grafiği çizmek için istediğiniz verilerin ayrıntılarını girmek için.
    
5. Sağdaki tüm sanal makineleri seçtikten sonra ölçümleri panosunda görüntüleme başlatabilirsiniz. 

Basit bir Pano iki grafiklerle aşağıdadır. Sol tarafta bir iki VM CPU yüzdesini gösterir. Sağdaki grafik hareketleri hareket API türüne göre dökümü yapılan bir Azure depolama hesabında gösterir.
    
![Grafana iki grafik örneği](.\media\monitor-how-to-grafana\grafana6.png) 
    

## <a name="optional-create-dashboard-playlists"></a>İsteğe bağlı: Pano çalma listeleri oluşturma

Çok sayıda kullanışlı özellikle Grafana, Pano çalma biridir. Birden çok panolar oluşturun ve bunları göstermek her Pano için bir aralığı yapılandırma çalma listesi ekleyin. Seçin **Yürüt** dolaşma panolar görmek için. Grubunuz için bir "durum Panosu" sağlamak için büyük duvar monitörde görüntülemek isteyebilirsiniz. 
    
![Grafana çalma listesi örneği](.\media\monitor-how-to-grafana\grafana7.png) 


## <a name="optional-monitor-your-custom-metrics-in-the-same-grafana-server"></a>İsteğe bağlı: özel ölçümlerinizi aynı Grafana sunucuda izleme

Ayrıca, Telegraf ve InfluxDB toplamak ve hem özel hem de Aracısı tabanlı ölçümleri aynı Grafana örneğinde çizmek için de yükleyebilirsiniz. Bu ölçümler bir Panoda araya getirmek için kullanabileceğiniz birçok veri kaynağı eklenti vardır. 
    
Ayrıca bu Prometheus sunucunuzdan ölçümleri içerecek şekilde ayarlanan yeniden kullanabilirsiniz. Prometheus veri kaynağı eklentisi Grafana'nın eklenti Galerisi'nde kullanın.
    
Telegraf, InfluxDB, Prometheus ve Docker kullanımı konusunda iyi Başvurusu makalelerine İşte
 - [Sistem ölçümleri Ubuntu 16.04 onay yığında ile izleme](https://www.digitalocean.com/community/tutorials/how-to-monitor-system-metrics-with-the-tick-stack-on-ubuntu-16-04)

 - [Docker kaynak ölçümleri Grafana, InfluxDB ve Telegraf ile izleme](https://blog.vpetkov.net/2016/08/04/monitor-docker-resource-metrics-with-grafana-influxdb-and-telegraf/)

 - [Docker ana bilgisayarları, kapsayıcıları ve kapsayıcılı hizmetler için bir izleme çözümü](https://stefanprodan.com/2016/a-monitoring-solution-for-docker-hosts-containers-and-containerized-services/)

Burada, Azure İzleyici ve Application Insights ölçümleri içeren tam bir Grafana Pano görüntüsü verilmiştir.
![Grafana örnek ölçümleri](.\media\monitor-how-to-grafana\grafana8.png) 


## <a name="clean-up-resources"></a>Kaynakları temizleme

Sanal makineleri veya bunları kullanmanızdan bağımsız olarak çalıştırırken ücretlendirilirsiniz. Ek ücret oluşmasını önlemek için bu makaledeki oluşturduğunuz kaynak grubunda Temizle. 

1. Azure portalında sol taraftaki menüden **kaynak grupları** ve ardından **Grafana**. 
2. Kaynak grubu sayfanızda tıklatın **silmek**, türü **Grafana** metin kutusuna ve ardından **silmek**.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure İzleyici ölçümleri genel bakış](monitoring-overview-metrics.md)


