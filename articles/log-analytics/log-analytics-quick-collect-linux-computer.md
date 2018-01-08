---
title: "Azure Log Analytics ile şirketi içi Linux bilgisayarlardan veri toplama | Microsoft Docs"
description: "Linux için Log Analytics aracısını dağıtmayı ve bu işletim sisteminden Log Analytics ile veri toplamayı etkinleştirmeyi öğrenin."
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
ms.date: 12/14/2017
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: fae88e44ee763a0c59b4ad2c731d77db379aa1ee
ms.sourcegitcommit: 3fca41d1c978d4b9165666bb2a9a1fe2a13aabb6
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2017
---
# <a name="collect-data-from-linux-computers-hosted-in-your-environment"></a>Ortamınızda barındırılan Linux bilgisayarlardan verileri toplama
[Azure Log Analytics](log-analytics-overview.md), doğrudan fiziksel veya sanal Linux bilgisayarlarınızdan ve ortamınızdaki diğer kaynaklardan verileri ayrıntılı analiz ve bağıntı için tek bir depoda toplayabilir.  Bu hızlı başlangıçta birkaç kolay adımda Linux bilgisayarınızı nasıl yapılandırabileceğiniz ve veri toplayabileceğiniz gösterilmektedir.  Azure Linux VM’leri için [Azure Sanal Makineler hakkında veri toplama](log-analytics-quick-collect-azurevm.md) konusuna bakın.  

Linux aracısını dağıtmak için ağ ve sistem gereksinimleri hakkında bilgilere [Azure Log Analytics ile ortamınızdan veri toplama](log-analytics-concept-hybrid.md#prerequisites) sayfasından ulaşabilirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="log-in-to-azure-portal"></a>Azure portalında oturum açın
[https://portal.azure.com](https://portal.azure.com) adresinde Azure portalında oturum açın. 

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma
1. Azure portalının sol alt köşesinde bulunan **Diğer hizmetler**'e tıklayın. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Log Analytics**’i seçin.<br><br> ![Azure portalı](media/log-analytics-quick-collect-azurevm/azure-portal-01.png)<br><br>  
2. **Oluştur**’a tıklayın, ardından şu öğeler için seçim yapın:

  * Yeni **OMS Çalışma Alanı** için *DefaultLAWorkspace* gibi bir ad sağlayın. 
  * Varsayılan seçili abonelik uygun değilse açılan listeden bağlanacak bir **Abonelik** seçin.
  * **Kaynak Grubu** için, bir veya daha fazla Azure sanal makinesi içeren mevcut bir kaynak grubunu seçin.  
  * VM’lerinizin dağıtıldığı **Konum**’u seçin.  Ek bilgi için bkz. [Log Analytics’in sunulduğu bölgeler](https://azure.microsoft.com/regions/services/).
  * Log Analytics’te üç farklı **fiyatlandırma katmanından** birini seçebilirsiniz ancak bu hızlı başlangıçta **ücretsiz** katmanı seçeceğiz.  Katmanlar hakkında daha fazla bilgi için bkz. [Log Analytics Fiyatlandırma Ayrıntıları](https://azure.microsoft.com/pricing/details/log-analytics/).

        ![Create Log Analytics resource blade](./media/log-analytics-quick-collect-azurevm/create-loganalytics-workspace-01.png)<br>  
3. **OMS Çalışma Alanı** bölmesinde gerekli bilgileri girdikten sonra **Tamam**’a tıklayın.  

Bilgilerin doğrulanıp çalışma alanının oluşturulması sırasında işlemin ilerleme durumunu menüdeki **Bildirimler**’in altından izleyebilirsiniz. 

## <a name="obtain-workspace-id-and-key"></a>Çalışma alanı kimliği ve anahtarını alma
Linux için OMS aracısını yüklemeden önce, Log Analytics çalışma alanınızın kimliği ve anahtarına ihtiyacınız olacak.  Bu bilgiler aracı sarmalayıcı betiğinin aracıyı düzgün bir şekilde yapılandırması ve Log Analytics ile başarılı bir şekilde iletişim kurabilmesi için gereklidir.  

1. Azure portalının sol alt köşesinde bulunan **Diğer hizmetler**'e tıklayın. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Log Analytics**’i seçin.
2. Log Analytics çalışma alanlarınızın listesinde, daha önceden oluşturduğunuz *DefaultLAWorkspace* çalışma alanını seçin.
3. **Gelişmiş ayarlar**’ı seçin.<br><br> ![Log Analytics Gelişmiş Ayarlar](media/log-analytics-quick-collect-azurevm/log-analytics-advanced-settings-01.png)<br><br>  
4. **Bağlı Kaynaklar**’ı seçin ve ardından **Linux Sunucuları**’nı seçin.   
5. **Çalışma Alanı Kimliği** ve **Birincil Anahtar**’ın sağındaki değer. Her ikisini de kopyalayıp sık kullandığınız bir düzenleyiciye yapıştırın.   

## <a name="install-the-agent-for-linux"></a>Linux için aracıyı yükleme
Aşağıdaki adımlar Azure’da ve Azure Kamu bulutunda Log Analytics için aracıyı yapılandırır.  

>[!NOTE]
>Linux için OMS aracısı birden fazla Log Analytics çalışma alanına raporlamak için yapılandırılamaz.  

1. Linux bilgisayarı Log Analytics’e bağlanmak için yapılandırmak üzere, önceden kopyaladığınız çalışma alanı kimliği ve birincil anahtarı sağlayarak aşağıdaki komutu çalıştırın.  Bu komut aracıyı indirir, sağlama toplamını doğrular ve aracıyı yükler. 
    
    ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <YOUR WORKSPACE ID> -s <YOUR WORKSPACE PRIMARY KEY>
    ```

2. Linux bilgisayarı Log Analytics’e bağlanmak için yapılandırmak üzere, Azure Kamu bulutunda, önceden kopyaladığınız çalışma alanı kimliği ve birincil anahtarı sağlayarak aşağıdaki komutu çalıştırın.  Bu komut aracıyı indirir, sağlama toplamını doğrular ve aracıyı yükler. 

    ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <YOUR WORKSPACE ID> -s <YOUR WORKSPACE PRIMARY KEY> -d opinsights.azure.us
    ``` 

## <a name="configure-agent-to-communicate-with-a-proxy-server"></a>Aracıyı bir proxy sunucusuyla iletişim kurmak için yapılandırma

Linux bilgisayarlarınızın Log Analytics ile bir proxy sunucusu üzerinden iletişim kurması gerekiyorsa aşağıdaki adımları uygulayın.  Proxy yapılandırması değeri `[protocol://][user:password@]proxyhost[:port]` sözdizimine sahiptir.

1. Aşağıdaki komutları çalıştırıp değerleri kendi ayarlarınıza göre değiştirerek `/etc/opt/microsoft/omsagent/proxy.conf` dosyasını düzenleyin.

    ```
    proxyconf="https://proxyuser:proxypassword@proxyserver01:30443"
    sudo echo $proxyconf >>/etc/opt/microsoft/omsagent/proxy.conf
    sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/proxy.conf 
    ```

2. Aşağıdaki komutu çalıştırarak aracıyı yeniden başlatın: 

    ```
    sudo /opt/microsoft/omsagent/bin/service_control restart [<workspace id>]
    ``` 

## <a name="collect-event-and-performance-data"></a>Olay ve performans verilerini toplama
Log Analytics uzun süreli analiz ve raporlama için belirttiğiniz Linux Syslog ve performans sayaçlarından olayları toplayarak belirli bir koşul algılandığında işlem yapabilir.  Linux Syslog’dan olayları toplamayı yapılandırmak ve birkaç ortak performans sayacı ile başlamak için bu adımları izleyin.  

1. **Syslog**’u seçin.  
2. Bir olay günlüğü eklemek için günlüğün adını yazın.  **Syslog** yazıp artı işaretine **+** tıklayın.  
3. Tabloda, **Bilgiler**, **Bildirim** ve **Hata Ayıklama** önem derecelerinin işaretini kaldırın. 
4. Yapılandırmayı kaydetmek için sayfanın en üstünde yer alan **Kaydet**’e tıklayın.
5. Bir Windows bilgisayarda performans sayaçlarını toplamayı etkinleştirmek için **Linux Performans Verileri**’ni seçin. 
6. Yeni bir Log Analytics çalışma alanı için Linux Performans sayaçlarını ilk kez yapılandırırken, birkaç ortak sayacı hızlı bir şekilde oluşturma seçenekleri sunulur. Her birinin yanında bir onay kutusu görüntülenir.<br><br> ![Varsayılan Windows performans sayaçları seçildi](media/log-analytics-quick-collect-azurevm/linux-perfcounters-default.png).<br><br> **Seçili performans sayaçlarını ekle**’ye tıklayın.  Eklenir ve on saniye koleksiyon örnek aralığı ile ayarlanır.  
7. Yapılandırmayı kaydetmek için sayfanın en üstünde yer alan **Kaydet**’e tıklayın.

## <a name="view-data-collected"></a>Toplanan verileri görüntüleyin
Veri toplamayı etkinleştirdiyseniz, şimdi hedef bilgisayardan verileri görmek için basit bir günlük araması örneği çalıştıralım.  

1. Azure portalında, Log Analytics’e gidip önceden oluşturduğunuz çalışma alanını seçin.
2. **Günlük Araması** kutucuğuna tıklayın ve Günlük Araması bölmesinde, sorgu alanında `Perf` yazıp Enter tuşuna basın veya sorgu alanının sağındaki arama düğmesine tıklayın.<br><br> ![Log Analytics günlük araması sorgu örneği](media/log-analytics-quick-collect-linux-computer/log-analytics-portal-queryexample.png)<br><br> Örneğin, aşağıdaki resimdeki sorgu 735 Performans kaydı döndürdü.<br><br> ![Log Analytics günlük araması sonucu](media/log-analytics-quick-collect-linux-computer/log-analytics-search-perf.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme
Artık gerekli olmadığında, aracıyı Linux bilgisayardan kaldırıp Log Analytics çalışma alanını silebilirsiniz.  

Aracıyı kaldırmak için aşağıdaki adımları izleyin.

1. Linux aracısı [evrensel betiğini](https://github.com/Microsoft/OMS-Agent-for-Linux/releases) bilgisayara indirin.
2. Bilgisayarda paket .sh dosyasını aracıyı ve yapılandırmasını tamamen kaldıran *--purge* bağımsız değişkeni ile çalıştırın.

    `sudo sh ./omsagent-<version>.universal.x64.sh --purge`

Çalışma alanını silmek için, önceden oluşturduğunuz Log Analytics çalışma alanını seçin ve kaynak sayfasında **Sil**’e tıklayın.<br><br> ![Log Analytics kaynağını silme](media/log-analytics-quick-collect-azurevm/log-analytics-portal-delete-resource.png)

## <a name="next-steps"></a>Sonraki adımlar
Şimdi şirket içi Linux bilgisayarınızdan işletimsel verileri ve performans verilerini topluyorsunuz ve *ücretsiz* olarak topladığınız verileri kolayca keşfetmeye, analiz etmeye ve verilerde işlem gerçekleştirmeye başlayabilirsiniz.  

Verileri görüntüleme ve analiz etmeyi öğrenmek için, öğreticiye devam edin.   

> [!div class="nextstepaction"]
> [Log Analytics’te verileri görüntüleme veya analiz etme](log-analytics-tutorial-viewdata.md)
