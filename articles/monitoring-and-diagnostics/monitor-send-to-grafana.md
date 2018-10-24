---
title: Azure hizmetlerini ve uygulamalarını Grafana kullanarak izleyin
description: Grafana içinde görüntülemek için rota Azure İzleyici ve Application Insights verileri.
services: azure-monitor
keywords: ''
author: rboucher
ms.author: robb
ms.date: 11/06/2017
ms.topic: conceptual
ms.service: azure-monitor
ms.component: ''
ms.openlocfilehash: b4fbd1248f91e0766cca66d1c51033a8b338c324
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "49957405"
---
# <a name="monitor-your-azure-services-in-grafana"></a>Grafana, Azure hizmetlerinizi izleyin
Şimdi de Azure Hizmetleri ve uygulamaları izleyebilirsiniz [Grafana](https://grafana.com/) kullanarak [Azure İzleyicisi veri kaynağı eklentisi](https://grafana.com/plugins/grafana-azure-monitor-datasource). Eklentisi, Azure İzleyici tarafından sağlanan altyapı verilerin yanı sıra Application Insights SDK'sı tarafından toplanan uygulama performansı verileri toplar. Grafana Panonuzda daha sonra bu verileri görüntüleyebilirsiniz.

Eklenti, şu anda Önizleme aşamasındadır.

Bir Azure Market'ten Grafana sunucusu kurma ve panolar için ölçümleri Azure İzleyici ve Application ınsights'ı oluşturmak için aşağıdaki adımları kullanın.

## <a name="set-up-a-grafana-instance"></a>Bir Grafana örneği
1. Azure Market'e dön ve Grafana Labs tarafından Grafana'ı seçin.

2. Adları ve ayrıntıları doldurun. Yeni bir kaynak grubu oluşturun. VM adı, VM parolasını ve Grafana sunucusu yönetici parolası için seçtiğiniz değerlerin takip edin.  

3. VM boyutu ve bir depolama hesabı seçin.

4. Ağ Yapılandırması ayarlarını yapılandırın.

5. Özet görüntüleyebilir ve seçebilir **Oluştur** kullanım koşullarını kabul eden sonra.

## <a name="log-in-to-grafana"></a>Grafana için oturum açın
1. Dağıtım tamamlandıktan sonra seçin **kaynak grubuna gidin**. Yeni oluşturulan kaynakların bir listesini görürsünüz.

    ![Grafana kaynak grubu nesneleri](.\media\monitor-how-to-grafana\grafana1.png)

    Ağ güvenlik grubu seçerseniz (*grafana nsg* bu durumda), bağlantı noktası 3000 Grafana sunucusuna erişmek için kullanıldığını görebilirsiniz.

2. Kaynaklar ve select listesinde dönün **genel IP adresi**. Bu ekranda bulunan değerleri kullanarak yazın *http://<IP address>: 3000* veya  *<DNSName>: 3000* tarayıcınızda. Yalnızca yerleşik Grafana sunucusu için bir oturum açma sayfası görmeniz gerekir.

    ![Grafana oturum açma ekranı](.\media\monitor-how-to-grafana\grafana2.png)

3. Oturum açmanız kullanıcı adı olarak *yönetici* ve daha önce oluşturduğunuz Grafana sunucusu yönetici parolası.

## <a name="configure-data-source-plugin"></a>Veri kaynağı eklentisini yapılandırma

Başarıyla oturum açtıktan sonra Azure İzleyici'veri kaynağı eklentisi zaten dahil olduğunu görmeniz gerekir.

![Azure İzleyici eklentisi Grafana gösterir](.\media\monitor-how-to-grafana\grafana3.png)

1. Seçin **veri kaynağı Ekle** Azure İzleyici ve Application Insights'ı yapılandırmak için.

2. Veri kaynağı için bir ad seçin ve seçin **Azure İzleyici** açılır listesinden veri kaynağı olarak.


## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

Grafana, Azure İzleyici API'lerine bağlanın ve ölçüm verilerini toplamak için bir Azure Active Directory Hizmet sorumlusu kullanır. Bir hizmet sorumlusu kullanarak Azure kaynaklarınıza erişimi yönetmek üzere oluşturmanız gerekir.

1. Bkz: [bu yönergeleri](../active-directory/develop/howto-create-service-principal-portal.md) bir hizmet sorumlusu oluşturmak için. Kopyalayın ve Kiracı kimliği, istemci Kimliğini ve istemci parolasını kaydedin.

2. Bkz: [uygulamayı role atama](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#assign-application-to-role) okuyucu rolü Azure Active Directory uygulamaya atamak için.     

3. Application Insights'ı kullanırsanız, uygulama kimliği ve Application Insights API Application Insights temel ölçümleri toplamak için de ekleyebilirsiniz. Daha fazla bilgi için [API anahtarı ve uygulama kimliği alma](https://dev.applicationinsights.io/documentation/Authorization/API-key-and-App-ID).

4. Tüm bu bilgileri girdikten sonra seçip **Kaydet** ve Grafana API sınar. Aşağıdakine benzer bir ileti görürsünüz.  

    ![Azure İzleyici eklentisi Grafana gösterir](.\media\monitor-how-to-grafana\grafana4-1.png)

> [!NOTE]
> Eklentisini yapılandırırken, hangi Azure bulut (Genel, Azure ABD devlet kurumları, Azure Almanya'yı veya Azure Çin) belirtebilirsiniz karşı yapılandırılması için eklenti istersiniz.
>
>

## <a name="build-a-grafana-dashboard"></a>Yapı Grafana Panosu

1. Giriş sayfasına gidin ve seçin **yeni Pano**.

2. Yeni Pano seçin **Graph**. Diğer grafik seçenekleri deneyebilirsiniz ancak bu makalede *grafik* örnek olarak.

    ![Grafana yeni Pano](.\media\monitor-how-to-grafana\grafana5.png)

3. Panonuzda boş bir grafik gösterir.

4. Paneli başlığa tıklayın ve seçin **Düzenle** içinde bu grafiği grafiği çizmek için istediğiniz verilerin ayrıntılarını girmek için.

5. Sağdaki tüm Vm'leri seçtikten sonra Panoda ölçümleri görüntüleme başlayabilirsiniz.

Aşağıdaki iki grafik ile basit bir panodur. Sol taraftaki bir, iki sanal makine CPU yüzdesini gösterir. Sağ taraftaki grafik işlemleri işlem API türü tarafından ayrılmış bir Azure depolama hesabındaki gösterir.

![Grafana iki grafik örneği](.\media\monitor-how-to-grafana\grafana6.png)


## <a name="optional-create-dashboard-playlists"></a>İsteğe bağlı: Pano çalma listeleri oluşturma

Grafana, birçok yararlı özellik Pano çalma listesi biridir. Birden çok Pano oluşturma ve bunları görüntülemek her bir Pano için bir aralık yapılandırma çalma listesi ekleyin. Seçin **Play** dolaşma panoları görmek için. Grubunuz için bir "durum Panosu" sağlamak için büyük duvar İzleyici görüntülemek isteyebilirsiniz.

![Grafana çalma listesi örneği](.\media\monitor-how-to-grafana\grafana7.png)


## <a name="optional-monitor-your-custom-metrics-in-the-same-grafana-server"></a>İsteğe bağlı: özel ölçümleriniz Grafana ile aynı sunucuda izleme

Ayrıca, Telegraf ve InfluxDB toplamak ve hem özel hem de aracı tabanlı ölçümleri aynı Grafana örneğinde çizmek için de yükleyebilirsiniz. Bir Panoda bu ölçümleri bir araya getirmek için kullanabileceğiniz birçok veri kaynağına eklentileri vardır.

Bu ölçümleri Prometheus sunucunuzdan içerecek şekilde ayarlamak da kullanabilirsiniz. Grafana'nın eklentisi galerisinde Prometheus veri kaynağı eklentisini kullanın.

Telegraf, InfluxDB Prometheus ve Docker kullanmaya nasıl iyi bir referans makaleleri şunlardır
 - [Ubuntu 16.04 değer ÇİZGİSİ yığında ile sistem ölçümlerini izleme](https://www.digitalocean.com/community/tutorials/how-to-monitor-system-metrics-with-the-tick-stack-on-ubuntu-16-04)

 - [Grafana, InfluxDB ve Telegraf Docker kaynak ölçümleri izleme](https://blog.vpetkov.net/2016/08/04/monitor-docker-resource-metrics-with-grafana-influxdb-and-telegraf/)

 - [Docker ana bilgisayarları, kapsayıcılar ve kapsayıcılı hizmetler için bir izleme çözümü](https://stefanprodan.com/2016/a-monitoring-solution-for-docker-hosts-containers-and-containerized-services/)

Bir Azure İzleyici ve Application Insights ölçüm olan tam bir Grafana panosunun görüntüsü aşağıda verilmiştir.
![Grafana örnek ölçümleri](.\media\monitor-how-to-grafana\grafana8.png)


## <a name="clean-up-resources"></a>Kaynakları temizleme

VM'ler, bunları veya kullanıp kullanmadığınızı çalıştırırken ücretlendirilir. Ek geçmeyecekseniz ücretlendirmeden kaçınmak için bu makalede oluşturduğunuz kaynak grubunu temizleyin.

1. Azure portalında sol taraftaki menüden **kaynak grupları** ve ardından **Grafana**.
2. Kaynak grubu sayfanızda tıklayın **Sil**, türü **Grafana** metin kutusuna ve ardından **Sil**.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure İzleyici ölçümlerine genel bakış](monitoring-overview-metrics.md)
