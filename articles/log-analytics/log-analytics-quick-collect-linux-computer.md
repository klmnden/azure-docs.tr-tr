---
title: "Azure günlük analizi ile şirket içi Linux bilgisayarlardan veri toplamanın | Microsoft Docs"
description: "Linux için günlük analizi aracı dağıtmak ve bu işletim sistemi günlük analizi ile veri toplamayı etkinleştirmek öğrenin."
services: log-analytics
documentationcenter: log-analytics
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 10/13/2017
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: d22fe6456c3bd886f8f8863d362c0084fbe03da3
ms.sourcegitcommit: e6029b2994fa5ba82d0ac72b264879c3484e3dd0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2017
---
# <a name="collect-data-from-linux-computers-hosted-in-your-environment"></a>Ortamınızda barındırılan Linux bilgisayarlardan veri toplama
[Azure günlük analizi](log-analytics-overview.md) veri ayrıntılı bir analiz ve bağıntı için tek bir depoda, fiziksel veya sanal Linux bilgisayarları ve diğer kaynakları ortamınızdaki doğrudan toplayabilirsiniz.  Bu hızlı başlangıç birkaç kolay adımda, Linux bilgisayardan veri toplamak ve yapılandırma gösterilmektedir.  Azure Linux VM'ler için aşağıdaki konuya bakın [verileri Azure sanal makineler hakkında toplama](log-analytics-quick-collect-azurevm.md).  
 
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="log-in-to-azure-portal"></a>Azure portalında oturum açın
Oturum açtığınızda Azure portalında [https://portal.azure.com](https://portal.azure.com). 

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma
1. Azure portalında tıklatın **daha fazla hizmet** sol alt köşesindeki üzerinde bulunamadı. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. Seçin **oturum Analytics**.<br><br> ![Azure portal](media/log-analytics-quick-collect-azurevm/azure-portal-01.png)<br><br>  
2. Tıklatın **oluşturma**ve ardından aşağıdaki öğeler için seçenekleri seçin:

  * İçin yeni bir ad **OMS çalışma**, gibi *DefaultLAWorkspace*. 
  * Varsayılan seçili abonelik uygun değilse açılan listeden bağlanacak bir **Abonelik** seçin.
  * İçin **kaynak grubu**, bir veya daha fazla Azure sanal makineleri varolan bir kaynak grubu seçin.  
  * Seçin **konumu** Vm'leriniz dağıtılır.  Ek bilgi için bkz: [günlük analizi bulunan bölgelere](https://azure.microsoft.com/regions/services/).
  * Üç farklı seçebilirsiniz **fiyatlandırma katmanlarına** günlük analizi, ancak bulacağınızı seçmek için bu hızlı başlangıç için **ücretsiz** katmanı.  Belirli katmanları hakkında ek bilgi için bkz: [günlük analizi fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/log-analytics/).

        ![Create Log Analytics resource blade](./media/log-analytics-quick-collect-azurevm/create-loganalytics-workspace-01.png)<br>  
3. Üzerinde gerekli bilgileri girdikten sonra **OMS çalışma** bölmesinde tıklatın **Tamam**.  

Bilgilerin doğrulanıp çalışma alanının oluşturulması sırasında işlemin ilerleme durumunu menüdeki **Bildirimler**’in altından izleyebilirsiniz. 

## <a name="obtain-workspace-id-and-key"></a>Çalışma alanı kimliği ve anahtarı edinin
Linux için OMS aracısının yüklemeden önce çalışma alanı kimliği ve anahtarı ihtiyacınız günlük analizi çalışma alanınız için.  Bu bilgiler, düzgün olarak aracıyı yapılandırmak ve günlük analizi ile başarıyla iletişim kurabilmesini sağlamak için aracı sarmalayıcı betiği tarafından gereklidir.  

1. Azure portalında tıklatın **daha fazla hizmet** sol alt köşesindeki üzerinde bulunamadı. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. Seçin **oturum Analytics**.
2. Günlük analizi çalışma alanları, listeden seçin *DefaultLAWorkspace* daha önce oluşturduğunuz.
3. Seçin **Gelişmiş ayarları**.<br><br> ![Günlük analizi Gelişmiş ayarları](media/log-analytics-quick-collect-azurevm/log-analytics-advanced-settings-01.png)<br><br>  
4. Seçin **bağlı kaynakları**ve ardından **Linux sunucuları**.   
5. Değerin sağındaki **çalışma alanı kimliği** ve **birincil anahtar**. Kopyalayın ve her ikisi de, sık kullanılan düzenleyicisine yapıştırın.   

## <a name="install-the-agent-for-linux"></a>Linux aracısı yükleyin
Aşağıdaki adımları aracı için günlük analizi Kurulumu Azure ve Azure kamu bulutta yapılandırın.  

1. Günlük Analizi'ne bağlanmak için Linux bilgisayarı yapılandırmak için çalışma alanı kimliği ve daha önce kopyaladığınız birincil anahtarı sağlayarak aşağıdaki komutu çalıştırın.  Bu komut Aracısı indirir, kendi sağlama toplamı doğrular ve yükler. 
    
    ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <YOUR WORKSPACE ID> -s <YOUR WORKSPACE PRIMARY KEY>
    ```

2. Günlük analizi Azure kamu bulutta bağlanmak için Linux bilgisayarı yapılandırmak için çalışma alanı kimliği ve daha önce kopyaladığınız birincil anahtarı sağlayarak aşağıdaki komutu çalıştırın.  Bu komut Aracısı indirir, kendi sağlama toplamı doğrular ve yükler. 

    ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <YOUR WORKSPACE ID> -s <YOUR WORKSPACE PRIMARY KEY> -d opinsights.azure.us
    ``` 

## <a name="configure-agent-to-communicate-with-a-proxy-server"></a>Bir proxy sunucusu ile iletişim kurmak için Aracısı'nı yapılandırma

Günlük analizi için bir proxy sunucu üzerinden iletişim kurmak Linux bilgisayarlarınızı ihtiyacınız varsa aşağıdaki adımları gerçekleştirin.  Proxy yapılandırma değeri sözdizimi aşağıdaki gibidir `[protocol://][user:password@]proxyhost[:port]`.

1. Dosyayı düzenlemek `/etc/opt/microsoft/omsagent/proxy.conf` çalıştırarak, aşağıdaki komutlar ve değerleri belirli ayarlarınızı değiştirin.

    ```
    proxyconf="https://proxyuser:proxypassword@proxyserver01:30443"
    sudo echo $proxyconf >>/etc/opt/microsoft/omsagent/proxy.conf
    sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/proxy.conf 
    ```

2. Aracı, aşağıdaki komutu çalıştırarak yeniden başlatın: 

    ```
    sudo /opt/microsoft/omsagent/bin/service_control restart [<workspace id>]
    ``` 

## <a name="collect-event-and-performance-data"></a>Olay ve performans verileri toplama
Günlük analizi uzun vadeli çözümleme ve raporlama için belirttiğiniz performans sayaçları ve Linux Syslog olayları toplamak ve belirli bir koşula algılandığında adımları uygulayın.  Olaylar Linux Syslog ve birkaç ortak performans sayaçlarını toplama başlamak yapılandırmak için aşağıdaki adımları izleyin.  

1. Seçin **Syslog**.  
2. Bir olay günlüğü, günlük adını yazarak ekleyin.  Tür **Syslog** ve artı işaretini tıklatın  **+** .  
3. Tabloda önem derecelerine işaretini **bilgisi**, **bildirimi** ve **hata ayıklama**. 
4. Tıklatın **kaydetmek** yapılandırmayı kaydetmek için sayfanın üstündeki.
5. Seçin **Linux performans verilerini** bir Windows bilgisayarda performans sayacı toplamayı etkinleştirmek için. 
6. Linux performans sayaçları yeni bir günlük analizi çalışma alanı için ilk yapılandırırken hızla birçok ortak sayaçları oluşturma seçeneği verilir. Bunlar, her yanındaki onay kutusunu ile listelenir.<br><br> ![Seçili varsayılan Windows performans sayaçlarını](media/log-analytics-quick-collect-azurevm/linux-perfcounters-default.png).<br><br> Tıklatın **Seçili performans sayaçlarını Ekle**.  Bunlar eklenir ve on ikinci koleksiyon örnekleme aralığı ile hazır.  
7. Tıklatın **kaydetmek** yapılandırmayı kaydetmek için sayfanın üstündeki.

## <a name="view-data-collected"></a>Toplanan görünüm verileri
Veri toplama etkinleştirdiğinize göre bazı veriler hedef bilgisayardan görmek için basit günlük arama örneği çalıştırmak olanak sağlar.  

1. Azure portalında günlük Analizi'ne gidin ve daha önce oluşturduğunuz çalışma alanını seçin.
2. Tıklatın **günlük arama** döşeme ve günlük arama bölmesinde Sorgu alan türü `Perf` ve ardından isabet girin veya sorgu alanının sağındaki arama düğmesini tıklatın.<br><br> ![Günlük analizi arama sorgusu örneğinde oturum](media/log-analytics-quick-collect-linux-computer/log-analytics-portal-queryexample.png)<br><br> Örneğin, aşağıdaki resimde sorguda 735 performans kayıtları döndürdü.<br><br> ![Günlük analizi arama sonucu oturum](media/log-analytics-quick-collect-linux-computer/log-analytics-search-perf.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme
Artık gerekli olduğunda, aracıyı Linux bilgisayardan kaldırın ve günlük analizi çalışma alanı silin.  

Aracıyı kaldırmak için aşağıdaki adımları gerçekleştirin.

1. İndirme Linux Aracısı'nı [Evrensel betik](https://github.com/Microsoft/OMS-Agent-for-Linux/releases) bilgisayara.
2. Paket .sh dosya alıştır *--Temizleme* bağımsız değişkeni bilgisayardaki aracı ve yapılandırmasını tamamen kaldırır.

    `sudo sh ./omsagent-<version>.universal.x64.sh --purge`

Çalışma alanını silmek için daha önce ve kaynak sayfası tıklatıldığında oluşturduğunuz günlük analizi çalışma alanı seçin **silmek**.<br><br> ![Günlük analizi kaynağı silme](media/log-analytics-quick-collect-azurevm/log-analytics-portal-delete-resource.png)

## <a name="next-steps"></a>Sonraki adımlar
Artık, işletimsel topluyorsunuz ve performans verileri şirket içi Linux bilgisayarınızdan, kolayca başlayabilir keşfetme, çözümleme ve için topladığınız veri alma eylemini *ücretsiz*.  

Görüntüleme ve verileri çözümleme öğrenmek için Öğreticisine devam edin.   

> [!div class="nextstepaction"]
> [Görüntülemek veya günlük analizi veri çözümleme](log-analytics-tutorial-viewdata.md)
