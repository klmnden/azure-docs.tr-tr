---
title: Hibrit Linux Bilgisayar için Azure Log Analytics Aracısını Yapılandırma | Microsoft Docs
description: Azure'un dışındaki bilgisayarlarda çalıştırılan Linux için Log Analytics aracısını dağıtmayı ve Log Analytics ile veri toplamayı etkinleştirmeyi öğrenin.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: quickstart
ms.date: 11/13/2018
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: 15b7c052d0e4d51cb033607c156a55c581f722b1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60539611"
---
# <a name="configure-log-analytics-agent-for-linux-computers-in-a-hybrid-environment"></a>Log Analytics aracısını hibrit ortamlardaki Linux bilgisayarlar için yapılandırma
[Azure Log Analytics](../../azure-monitor/platform/agent-windows.md), doğrudan veri merkezinizdeki veya diğer bulut ortamlarındaki fiziksel veya sanal Linux bilgisayarlarınızda bulunan verileri ayrıntılı analiz ve bağıntı için tek bir depoda toplayabilir. Bu hızlı başlangıçta birkaç kolay adımda Linux bilgisayarınızı nasıl yapılandırabileceğiniz ve veri toplayabileceğiniz gösterilmektedir.  Azure Linux VM’leri için [Azure Sanal Makineler hakkında veri toplama](quick-collect-azurevm.md) konusuna bakın.  

Desteklenen yapılandırmayı anlamak için [desteklenen Linux işletim sistemlerini](../../azure-monitor/platform/log-analytics-agent.md#supported-linux-operating-systems) ve [ağ güvenlik duvarı yapılandırmasını](../../azure-monitor/platform/log-analytics-agent.md#network-firewall-requirements) inceleyin.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın
[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın. 

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma
1. Azure portalında **Tüm hizmetler**’e tıklayın. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Log Analytics**’i seçin.

    ![Azure portal](media/quick-collect-linux-computer/azure-portal-01.png) 

2. **Oluştur**’a tıklayın, ardından şu öğeler için seçim yapın:

   * Yeni **Log Analytics çalışma alanı** için *DefaultLAWorkspace* gibi bir ad sağlayın. OMS çalışma alanları artık Log Analytics çalışma alanları olarak adlandırılır.   
   * Varsayılan seçili abonelik uygun değilse açılan listeden bağlanacak bir **Abonelik** seçin.
   * **Kaynak Grubu** için, bir veya daha fazla Azure sanal makinesi içeren mevcut bir kaynak grubunu seçin.  
   * VM’lerinizin dağıtıldığı **Konum**’u seçin.  Ek bilgi için bkz. [Log Analytics’in sunulduğu bölgeler](https://azure.microsoft.com/regions/services/).  
   * 2 Nisan 2018 tarihinden sonra oluşturulan yeni bir abonelikte çalışma alanı oluşturuyorsanız bu, otomatik olarak *GB başına* fiyatlandırma planını kullanır ve fiyatlandırma katmanı seçme seçeneği kullanılamaz.  2 Nisan’dan önce oluşturulmuş mevcut bir abonelik için veya mevcut bir EA kaydına bağlı aboneliğe yönelik çalışma alanı oluşturuyorsanız, tercih ettiğiniz fiyatlandırma katmanını seçin.  Katmanlar hakkında daha fazla bilgi için bkz. [Log Analytics Fiyatlandırma Ayrıntıları](https://azure.microsoft.com/pricing/details/log-analytics/).

        ![Log Analytics kaynak dikey penceresi oluşturma](media/quick-collect-linux-computer/create-loganalytics-workspace-02.png)<br>  

3. **Log Analytics çalışma alanı** bölmesinde gerekli bilgileri girdikten sonra **Tamam**’a tıklayın.  

Bilgilerin doğrulanıp çalışma alanının oluşturulması sırasında işlemin ilerleme durumunu menüdeki **Bildirimler**’in altından izleyebilirsiniz. 

## <a name="obtain-workspace-id-and-key"></a>Çalışma alanı kimliği ve anahtarını alma
Linux için Log Analytics aracısını yüklemeden önce, Log Analytics çalışma alanınızın kimliği ve anahtarına ihtiyacınız olacak.  Bu bilgiler aracı sarmalayıcı betiğinin aracıyı düzgün bir şekilde yapılandırması ve Log Analytics ile başarılı bir şekilde iletişim kurabilmesi için gereklidir.

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]  

1. Azure portalının sol alt köşesinde bulunan **Tüm hizmetler**’e tıklayın. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Log Analytics**’i seçin.
2. Log Analytics çalışma alanlarınızın listesinde, daha önceden oluşturduğunuz *DefaultLAWorkspace* çalışma alanını seçin.
3. **Gelişmiş ayarlar**’ı seçin.

    ![Log Analytics Gelişmiş Ayarlar](media/quick-collect-linux-computer/log-analytics-advanced-settings-01.png) 
 
4. **Bağlı Kaynaklar**’ı seçin ve ardından **Linux Sunucuları**’nı seçin.   
5. **Çalışma Alanı Kimliği** ve **Birincil Anahtar**’ın sağındaki değer. Her ikisini de kopyalayıp sık kullandığınız bir düzenleyiciye yapıştırın.   

## <a name="install-the-agent-for-linux"></a>Linux için aracıyı yükleme
Aşağıdaki adımlar Azure’da ve Azure Kamu bulutunda Log Analytics için aracıyı yapılandırır.  

>[!NOTE]
>Linux için Log Analytics aracısı birden fazla Log Analytics çalışma alanına raporlamak için yapılandırılamaz.  

Linux bilgisayarınızın bir ara sunucu üzerinden Log Analytics’le iletişim kurması gerekiyorsa, ara sunucu yapılandırması komut satırında `-p [protocol://][user:password@]proxyhost[:port]` eklenerek belirtilebilir.  *proxyhost* özelliği, ara sunucunun tam etki adı alanı veya IP adresini kabul eder. 

Örneğin, `https://user01:password@proxy01.contoso.com:30443`

1. Linux bilgisayarı Log Analytics’e bağlanmak için yapılandırmak üzere, önceden kopyaladığınız çalışma alanı kimliği ve birincil anahtarı sağlayarak aşağıdaki komutu çalıştırın. Bu komut aracıyı indirir, sağlama toplamını doğrular ve aracıyı yükler. 
    
    ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <YOUR WORKSPACE ID> -s <YOUR WORKSPACE PRIMARY KEY>
    ```

    Aşağıdaki komut, `-p` ara sunucu parametresini ve örnek söz dizimini içerir.

   ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -p [protocol://][user:password@]proxyhost[:port] -w <YOUR WORKSPACE ID> -s <YOUR WORKSPACE PRIMARY KEY>
    ```

2. Linux bilgisayarı Log Analytics’e bağlanmak için yapılandırmak üzere, Azure Kamu bulutunda, önceden kopyaladığınız çalışma alanı kimliği ve birincil anahtarı sağlayarak aşağıdaki komutu çalıştırın. Bu komut aracıyı indirir, sağlama toplamını doğrular ve aracıyı yükler. 

    ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <YOUR WORKSPACE ID> -s <YOUR WORKSPACE PRIMARY KEY> -d opinsights.azure.us
    ``` 

    Aşağıdaki komut, `-p` ara sunucu parametresini ve örnek söz dizimini içerir.

   ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -p [protocol://][user:password@]proxyhost[:port] -w <YOUR WORKSPACE ID> -s <YOUR WORKSPACE PRIMARY KEY> -d opinsights.azure.us
    ```
2. Aşağıdaki komutu çalıştırarak aracıyı yeniden başlatın: 

    ```
    sudo /opt/microsoft/omsagent/bin/service_control restart [<workspace id>]
    ``` 

## <a name="collect-event-and-performance-data"></a>Olay ve performans verilerini toplama
Log Analytics uzun süreli analiz ve raporlama için belirttiğiniz Linux Syslog ve performans sayaçlarından olayları toplayarak belirli bir koşul algılandığında işlem yapabilir.  Linux Syslog’dan olayları toplamayı yapılandırmak ve birkaç ortak performans sayacı ile başlamak için bu adımları izleyin.  

1. **Syslog**’u seçin.  
2. Bir olay günlüğü eklemek için günlüğün adını yazın. **Syslog** yazıp artı işaretine **+** tıklayın.  
3. Tabloda, **Bilgiler**, **Bildirim** ve **Hata Ayıklama** önem derecelerinin işaretini kaldırın. 
4. Yapılandırmayı kaydetmek için sayfanın en üstünde yer alan **Kaydet**’e tıklayın.
5. Bir Linux bilgisayarda performans sayaçlarını toplamayı etkinleştirmek için **Linux Performans Verileri**’ni seçin. 
6. Yeni bir Log Analytics çalışma alanı için Linux Performans sayaçlarını ilk kez yapılandırırken, birkaç ortak sayacı hızlı bir şekilde oluşturma seçenekleri sunulur. Her birinin yanında bir onay kutusu görüntülenir. 

    ![Varsayılan Windows performans sayaçları seçildi](media/quick-collect-linux-computer/linux-perfcounters-default.png)
    
    **Seçili performans sayaçlarını ekle**’ye tıklayın. Eklenir ve on saniye koleksiyon örnek aralığı ile ayarlanır.

7. Yapılandırmayı kaydetmek için sayfanın en üstünde yer alan **Kaydet**’e tıklayın.

## <a name="view-data-collected"></a>Toplanan verileri görüntüleyin
Veri toplamayı etkinleştirdiyseniz, şimdi hedef bilgisayardan verileri görmek için basit bir günlük araması örneği çalıştıralım.  

1. Azure portalında, Log Analytics’e gidip önceden oluşturduğunuz çalışma alanını seçin.
2. **Günlük Araması** kutucuğuna tıklayın ve Günlük Araması bölmesinde, sorgu alanında `Perf` yazıp Enter tuşuna basın veya sorgu alanının sağındaki arama düğmesine tıklayın.

    ![Log Analytics günlük araması sorgu örneği](media/quick-collect-linux-computer/log-analytics-portal-queryexample.png)

    Örneğin, aşağıdaki resimdeki sorgu 735 Performans kaydı döndürdü.

    ![Log Analytics günlük araması sonucu](media/quick-collect-linux-computer/log-analytics-search-perf.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme
Artık gerekli olmadığında, aracıyı Linux bilgisayardan kaldırıp Log Analytics çalışma alanını silebilirsiniz.  

Aracıyı kaldırmak için Linux bilgisayarında aşağıdaki komutu çalıştırın. *--temizleme* bağımsız değişkeni, aracıyı ve yapılandırmasını tamamen kaldırır.

   `wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh --purge`

Çalışma alanını silmek için, önceden oluşturduğunuz Log Analytics çalışma alanını seçin ve kaynak sayfasında **Sil**’e tıklayın.

![Log Analytics kaynağını silme](media/quick-collect-linux-computer/log-analytics-portal-delete-resource.png)

## <a name="next-steps"></a>Sonraki adımlar
Şimdi şirket içi Linux bilgisayarınızdan işletimsel verileri ve performans verilerini topluyorsunuz ve *ücretsiz* olarak topladığınız verileri kolayca keşfetmeye, analiz etmeye ve verilerde işlem gerçekleştirmeye başlayabilirsiniz.  

Verileri görüntüleme ve analiz etmeyi öğrenmek için, öğreticiye devam edin.   

> [!div class="nextstepaction"]
> [Log Analytics’te verileri görüntüleme veya analiz etme](../../azure-monitor/learn/tutorial-viewdata.md)
