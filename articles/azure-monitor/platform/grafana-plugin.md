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
ms.subservice: ''
ms.openlocfilehash: e9a20aba84e79e87f84d63e4bdae3ba1aac062f5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66387184"
---
# <a name="monitor-your-azure-services-in-grafana"></a>Grafana, Azure hizmetlerinizi izleyin
Artık Azure Hizmetleri ve uygulamaları izleyebilirsiniz [Grafana](https://grafana.com/) kullanarak [Azure İzleyicisi veri kaynağı eklentisi](https://grafana.com/plugins/grafana-azure-monitor-datasource). Eklenti çeşitli günlükleri ve ölçümler de dahil olmak üzere Azure İzleyici tarafından toplanan uygulama performansı verileri toplar. Grafana Panonuzda daha sonra bu verileri görüntüleyebilirsiniz.

Eklenti, şu anda Önizleme aşamasındadır.

Grafana sunucusunu ayarlama ve Azure İzleyici'den ölçüm ve günlükleri için panolar oluşturmak için aşağıdaki adımları kullanın.

## <a name="set-up-a-grafana-server"></a>Grafana sunucusunu ayarlama

### <a name="set-up-grafana-locally"></a>Grafana ' yerel olarak ayarlayın
Bir yerel Grafana sunucusu kurmak için [Grafana yerel ortamınızda yükleyip](https://grafana.com/grafana/download). Eklenti'nın Azure izleme tümleştirmesini kullanacak şekilde Grafana 5.3 veya sonraki sürümünü yükleyin.

### <a name="set-up-grafana-on-azure-through-the-azure-marketplace"></a>Azure Marketi aracılığıyla azure'da Grafana ayarlama
1. Azure Market'e dön ve Grafana Labs tarafından Grafana'ı seçin.

2. Adları ve ayrıntıları doldurun. Yeni bir kaynak grubu oluşturun. VM adı, VM parolasını ve Grafana sunucusu yönetici parolası için seçtiğiniz değerlerin takip edin.  

3. VM boyutu ve bir depolama hesabı seçin.

4. Ağ Yapılandırması ayarlarını yapılandırın.

5. Özet görüntüleyebilir ve seçebilir **Oluştur** kullanım koşullarını kabul eden sonra.

6. Dağıtım tamamlandıktan sonra seçin **kaynak grubuna gidin**. Yeni oluşturulan kaynakların bir listesini görürsünüz.

    ![Grafana kaynak grubu nesneleri](media/grafana-plugin/grafana1.png)

    Ağ güvenlik grubu seçerseniz (*grafana nsg* bu durumda), bağlantı noktası 3000 Grafana sunucusuna erişmek için kullanıldığını görebilirsiniz.

7. Grafana sunucunuzun genel IP adresini alma - kaynaklarının ve seçim listesine geri dönün **genel IP adresi**.

## <a name="sign-in-to-grafana"></a>Grafana için oturum açın

1. Sunucunuzun IP adresini kullanarak açın oturum açma sayfasına *http://\<IP adresi\>: 3000* veya  *\<DNSName >\:3000* tarayıcınızda. Varsayılan bağlantı noktası 3000 olsa da, Kurulum sırasında farklı bir bağlantı noktası seçmiş olabilirsiniz unutmayın. Derlediğiniz Grafana sunucusu için bir oturum açma sayfası görmeniz gerekir.

    ![Grafana oturum açma ekranı](./media/grafana-plugin/grafana-login-screen.png)

2. Kullanıcı adı ile oturum *yönetici* ve daha önce oluşturduğunuz Grafana sunucusu yönetici parolası. Bir yerel ayarı kullanıyorsanız, varsayılan parola olacaktır *yönetici*, ve ilk oturum açma bilgilerinize değiştirmek için istenen.

## <a name="configure-data-source-plugin"></a>Veri kaynağı eklentisini yapılandırma

Başarıyla oturum açtıktan sonra Azure İzleyici'veri kaynağı eklentisi zaten dahil olduğunu görmeniz gerekir.

![Azure İzleyici eklentisi Grafana içerir](./media/grafana-plugin/grafana-includes-azure-monitor-plugin-dark.png)

1. Seçin **veri kaynağı Ekle** eklemek ve Azure İzleyici veri kaynağını yapılandırmak için.

2. Veri kaynağı için bir ad seçin ve seçin **Azure İzleyici** açılır listeden türü.

3. Hizmet sorumlusu oluşturma - Azure İzleyici API'lerine bağlanın ve verileri toplamak için bir Azure Active Directory Hizmet sorumlusu Grafana kullanır. Oluşturun veya mevcut bir hizmet sorumlusu kullanarak Azure kaynaklarınıza erişimi yönetmek için kullanın.
    * Bkz: [bu yönergeleri](../../azure-resource-manager/resource-group-create-service-principal-portal.md) bir hizmet sorumlusu oluşturmak için. Kopyalayın ve Kiracı kimliği (dizin kimliği), istemci kimliği (uygulama kimliği) ve istemci gizli anahtarı (uygulama anahtar değeri) kaydedin.
    * Bkz: [uygulamayı role atama](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal) aboneliğe ilişkin Azure Active Directory uygulaması okuyucu rolüne atamak için kaynak grubu veya kaynağa izlemek istediğiniz. 
    Günlük analizi API'si gerektirir [Log Analytics okuyucusu rolü](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#log-analytics-reader), okuyucu rol izinleri içerir ve bu gruba ekler.

4. Kullanmak istediğiniz API bağlantı ayrıntılarını sağlayın. Tümünü veya bazılarını bağlanabilirsiniz. 
    * Bağlarsanız, ölçüm ve günlükleri Azure İzleyicisi'nde için aynı kimlik bilgilerini seçerek kullanabilirsiniz **Azure İzleyici API'si aynı detayları**.
    * Eklentisini yapılandırırken, hangi Azure bulut belirtebilirsiniz İzleyicisi (Genel, Azure ABD devlet kurumları, Azure Almanya'yı veya Azure Çin) eklentisi istediğiniz.
    * Application Insights'ı kullanırsanız, uygulama kimliği ve Application Insights API Application Insights temel ölçümleri toplamak için de ekleyebilirsiniz. Daha fazla bilgi için [API anahtarı ve uygulama kimliği alma](https://dev.applicationinsights.io/documentation/Authorization/API-key-and-App-ID).

        > [!NOTE]
        > Bazı veri kaynağı alanları ilişkili Azure ayarlarına farklı şekilde adlandırılır:
        > * Kiracı kimliği Azure Directory kimliğidir
        > * İstemci kimliği olan Azure Active Directory Uygulama Kimliği
        > * İstemci parolası, Azure Active Directory uygulaması anahtar değeri

5. Application Insights'ı kullanırsanız, uygulama kimliği ve Application Insights API Application Insights temel ölçümleri toplamak için de ekleyebilirsiniz. Daha fazla bilgi için [API anahtarı ve uygulama kimliği alma](https://dev.applicationinsights.io/documentation/Authorization/API-key-and-App-ID).

6. Seçin **Kaydet**, ve Grafana her bir API için kimlik bilgilerini test. Aşağıdakine benzer bir ileti görürsünüz.  
    ![Onaylanan config Grafana veri kaynağı](./media/grafana-plugin/grafana-data-source-config-approved-dark.png)

## <a name="build-a-grafana-dashboard"></a>Yapı Grafana Panosu

1. Grafana giriş sayfasına gidin ve seçin **yeni Pano**.

2. Yeni Pano seçin **Graph**. Diğer grafik seçenekleri deneyebilirsiniz ancak bu makalede *grafik* örnek olarak.

3. Panonuzda boş bir grafik gösterir. Paneli başlığa tıklayın ve seçin **Düzenle** içinde bu grafiği grafiği çizmek için istediğiniz verilerin ayrıntılarını girmek için.
    ![Grafana yeni graf](./media/grafana-plugin/grafana-new-graph-dark.png)

4. Azure İzleyici, yapılandırdığınız veri kaynağını seçin.
   * Azure İzleyici toplama ölçümleri - seçin **Azure İzleyici** hizmet açılır. Yukarı, kaynakları ve bu grafikte izlemek için ölçüm seçebileceğiniz Seçici gösterir listesi. Bir VM'den ölçümleri toplamak için ad alanını kullanmak **Microsoft.Compute/VirtualMachines**. VM'ler ve ölçümleri seçtikten sonra Panoda verilerini görüntüleme başlayabilirsiniz.
     ![Azure İzleyici için Grafana graph yapılandırma](./media/grafana-plugin/grafana-graph-config-for-azure-monitor-dark.png)
   * Azure İzleyici toplama - günlük verilerini seçin **Azure Log Analytics** hizmet açılır. Sorgulamak ve sorgu metni ayarlamak istediğiniz çalışma alanını seçin. Burada, zaten veya yeni bir günlük sorgusu kopyalayabilirsiniz. Sorgu yazarken IntelliSense gösterilir ve otomatik tamamlama seçenekleri önerin. Bir görselleştirme türü seçin **zaman serisi** **tablo**, ve sorguyu çalıştırın.
    
     > [!NOTE]
     >
     > Eklentisi ile sağlanan varsayılan sorgu iki makrolarını kullanır: "$__timeFilter() ve $__interval. 
     > Bu makrolar Grafana zaman aralığını ve zaman çizgisi, bir grafik parçası üzerinde yakınlaştırdığınızda dinamik olarak hesaplamak izin verin. Bu makrolar kaldırın ve gibi bir standart süresi filtre kullanın *TimeGenerated > önce (1s)* anlamına gelir grafik yakınlaştırma özelliği destekleyecektir değil, ancak.
    
     ![Azure Log Analytics için Grafana graph yapılandırma](./media/grafana-plugin/grafana-graph-config-for-azure-log-analytics-dark.png)

5. Aşağıdaki iki grafik ile basit bir panodur. Sol taraftaki bir, iki sanal makine CPU yüzdesini gösterir. Sağ taraftaki grafik işlemleri işlem API türü tarafından ayrılmış bir Azure depolama hesabındaki gösterir.
    ![Grafana iki grafik örneği](media/grafana-plugin/grafana6.png)


## <a name="optional-monitor-your-custom-metrics-in-the-same-grafana-server"></a>İsteğe bağlı: Özel ölçümleriniz Grafana ile aynı sunucuda izleme

Telegraf ve InfluxDB toplamak ve hem özel hem de aracı tabanlı ölçümleri çizmek için yükleyebilmek için aynı Grafana örneği. Bir Panoda bu ölçümleri bir araya getirmek için kullanabileceğiniz birçok veri kaynağına eklentileri vardır.

Bu ölçümleri Prometheus sunucunuzdan içerecek şekilde ayarlamak da kullanabilirsiniz. Grafana'nın eklentisi galerisinde Prometheus veri kaynağı eklentisini kullanın.

Telegraf, InfluxDB Prometheus ve Docker kullanmaya nasıl iyi bir referans makaleleri şunlardır
 - [Ubuntu 16.04 değer ÇİZGİSİ yığında ile sistem ölçümlerini izleme](https://www.digitalocean.com/community/tutorials/how-to-monitor-system-metrics-with-the-tick-stack-on-ubuntu-16-04)

 - [Docker ana bilgisayarları, kapsayıcılar ve kapsayıcılı hizmetler için bir izleme çözümü](https://stefanprodan.com/2016/a-monitoring-solution-for-docker-hosts-containers-and-containerized-services/)

Bir Azure İzleyici ve Application Insights ölçüm olan tam bir Grafana panosunun görüntüsü aşağıda verilmiştir.
![Grafana örnek ölçümleri](media/grafana-plugin/grafana8.png)

## <a name="advanced-grafana-features"></a>Gelişmiş Grafana özellikleri

### <a name="variables"></a>Değişkenler
Bazı sorgu değerleri UI bırakmalar seçili ve sorguda güncelleştirildi. Aşağıdaki sorguda bir örnek olarak göz önünde bulundurun:
```
Usage 
| where $__timeFilter(TimeGenerated) 
| summarize total_KBytes=sum(Quantity)*1024 by bin(TimeGenerated, $__interval) 
| sort by TimeGenerated
```

Tüm kullanılabilir listesinde bir değişken yapılandırabileceğiniz **çözüm** değer verir ve ardından bunu kullanmak için sorgunuzu güncelleştirebilir.
Yeni bir değişken,'a tıklayın, sağ üst kısımdaki alana Panodaki Ayarlar düğmesi oluşturmak için seçin **değişkenleri**, ardından **yeni**.
Değişken sayfasında değerlerin listesini almak için çalıştırılacak sorgu ve veri kaynağı tanımlayın.
![Grafana yapılandırma değişkeni](./media/grafana-plugin/grafana-configure-variable-dark.png)

Oluşturulduktan sonra seçili değerler kullanılacak sorguyu değiştirebilir ve grafiklerinizi uygun şekilde yanıt verir:
```
Usage 
| where $__timeFilter(TimeGenerated) and Solution in ($Solutions)
| summarize total_KBytes=sum(Quantity)*1024 by bin(TimeGenerated, $__interval) 
| sort by TimeGenerated
```
    
![Grafana kullanma değişkenleri](./media/grafana-plugin/grafana-use-variables-dark.png)

### <a name="create-dashboard-playlists"></a>Pano çalma listeleri oluşturma

Grafana, birçok yararlı özellik Pano çalma listesi biridir. Birden çok Pano oluşturma ve bunları görüntülemek her bir Pano için bir aralık yapılandırma çalma listesi ekleyin. Seçin **Play** dolaşma panoları görmek için. Grubunuz için bir durum Panosu sağlamak büyük duvar İzleyici görüntülemek isteyebilirsiniz.

![Grafana çalma listesi örneği](./media/grafana-plugin/grafana7.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kurulum Grafana ortamı Azure'da belirttiyseniz, VM'ler, bunları veya kullanıp kullanmadığınızı çalıştırırken ücretlendirilir. Ek geçmeyecekseniz ücretlendirmeden kaçınmak için bu makalede oluşturduğunuz kaynak grubunu temizleyin.

1. Azure portalında sol taraftaki menüden **kaynak grupları** ve ardından **Grafana**.
2. Kaynak grubu sayfanızda tıklayın **Sil**, türü **Grafana** metin kutusuna ve ardından **Sil**.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure İzleyici ölçümlerine genel bakış](data-platform.md)

