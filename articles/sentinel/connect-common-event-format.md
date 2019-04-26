---
title: Azure Önizleme Gözcü CEF verileri bağlayın | Microsoft Docs
description: CEF verileri Azure Gözcü için bağlanmayı öğreneceksiniz.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: cbf5003b-76cf-446f-adb6-6d816beca70f
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/02/2019
ms.author: rkarlin
ms.openlocfilehash: 18eb305beb79913713898b939ef840ca9ffab014
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60445541"
---
# <a name="connect-your-external-solution-using-common-event-format"></a>Common Event Format'ı kullanarak dış çözümünüzü bağlayın

> [!IMPORTANT]
> Azure Sentinel şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Günlük dosyaları Syslog tasarruf etmenize olanak sağlayan bir dış çözüm ile Azure Gözcü bağlanabilirsiniz. Günlükleri Syslog Common Event Format (CEF) olarak kaydetmek gerecinizin sağlar, analiz ve sorguları arasında verileri kolayca çalıştırmanıza Gözcü Azure ile tümleştirme sağlar.

> [!NOTE] 
> Veriler Azure Gözcü çalıştırıyorsanız çalışma alanının coğrafi konumda depolanır.

## <a name="how-it-works"></a>Nasıl çalışır?

Azure Gözcü ve CEF gerecinize arasındaki bağlantıyı üç adımda gerçekleştirilir:

1. Gerecinde gerecin Azure Gözcü Syslog aracı için gereken biçimde gerekli günlükleri gönderir. böylece bu değerleri ayarlamanız gerekir. Ayrıca bunları Azure Gözcü Aracısı Syslog arka plan programı, değişiklik sürece gereciniz, bu parametreleri değiştirebilirsiniz.
    - Protokol UDP =
    - Bağlantı noktası 514 =
    - Özelliği yerel 4 =
    - Biçim CEF =
2. Syslog aracı veri toplar ve günlük Burada, Ayrıştırılan zenginleştirilmiş ve analiz için güvenli bir şekilde gönderir.
3. Analiz ve bağıntı kuralları panoları kullanarak, gerektiği şekilde sorgulanabilir için aracı verileri Log Analytics çalışma alanında depolar.


## <a name="step-1-connect-to-your-cef-appliance-via-dedicated-azure-vm"></a>1. Adım: CEF gerecinize adanmış bir Azure VM aracılığıyla bağlanma

Özel bir Linux makine aracıda dağıtmanız gerekebilir (VM veya şirket içi) Gereci ve Azure Gözcü arasındaki iletişimi desteklemek için. Aracı otomatik olarak veya el ile dağıtabilirsiniz. Otomatik dağıtım, Resource Manager şablonları temel alan ve yalnızca, özel bir Linux makine Azure'da oluşturduğunuz yeni bir VM ise kullanılabilir.

 ![Azure'da CEF](./media/connect-cef/cef-syslog-azure.png)

Alternatif olarak, aracı vm'sinde başka bir bulut, mevcut bir Azure sanal makinesinde el ile veya bir şirket içi makinede dağıtabilirsiniz. 

 ![Şirket içi CEF](./media/connect-cef/cef-syslog-onprem.png)

### <a name="deploy-the-agent-in-azure"></a>Aracıyı azure'da dağıtın


1. Gözcü Azure portalında **veri bağlayıcıları** ve gereç türünüzü seçin. 

1. Altında **Linux Syslog aracı Yapılandırması**:
   - Seçin **otomatik dağıtım** yukarıda açıklandığı gibi Azure Gözcü aracıyla birlikte önceden yüklenir ve tüm yapılandırma gerekli içeren yeni bir makine oluşturmak istiyorsanız. Seçin **otomatik dağıtım** tıklatıp **otomatik aracı dağıtımı**. Bu, satın alma sayfasına adanmış bir Linux, otomatik olarak çalışma alanınıza bağlı olduğu VM için götürür. VM bir **standart D2s v3 (2 vcpu, 8 GB bellek)** ve genel bir IP adresi vardır.
      1. İçinde **özel dağıtım** sayfasında ayrıntılarınızı sağlamak ve bir kullanıcı adı ve parola seçin ve hüküm ve koşulları kabul ediyorsanız, VM satın alın.
      1. Bağlantı sayfada listelenen ayarları kullanarak günlükleri göndermek için gerecinizin yapılandırın. Genel Common Event Format bağlayıcısının bu ayarları kullanın:
         - Protokol UDP =
         - Bağlantı noktası 514 =
         - Özelliği yerel 4 =
         - Biçim CEF =
   - Seçin **el ile dağıtım** ileride Azure Gözcü aracısının yüklenmesi gerekir özel Linux makine varolan bir VM'yi kullanmak istiyorsanız. 
      1. Altında **Syslog aracısını indirme ve yükleme**seçin **Azure Linux sanal makinesi**. 
      1. İçinde **sanal makineler** açılır, ekran seçin'e tıklayın, istediğiniz makine **Connect**.
      1. Bağlayıcı ekranda altında **yapılandırma ve iletme Syslog**ayarlayın, Syslog daemon olup **rsyslog.d** veya **syslog-ng**. 
      1. Bu komutlar kopyalayın ve bunları gerecinizde çalıştırın:
          - Rsyslog.d seçtiyseniz:
              
            1. Tesis local_4 üzerinde dinleme ve bağlantı noktası 25226'daki kullanarak Azure Gözcü Aracısı Syslog iletileri göndermek için Syslog daemon'u söyleyin. `sudo bash -c "printf 'local4.debug  @127.0.0.1:25226' > /etc/rsyslog.d/security-config-omsagent.conf"`
            
            2. İndirme ve yükleme [security_events yapılandırma dosyası](https://aka.ms/asi-syslog-config-file-linux) Syslog aracı 25226'daki bağlantı noktasında dinleyecek şekilde yapılandırır. `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` Burada {0} çalışma GUID ile değiştirilmelidir.
            
            1. Syslog daemon'u başlatmak `sudo service rsyslog restart`<br> Daha fazla bilgi için [rsyslog belgeleri](https://www.rsyslog.com/doc/v8-stable/tutorials/tls_cert_summary.html)
           
          - Syslog-ng seçtiyseniz:

              1. Tesis local_4 üzerinde dinleme ve bağlantı noktası 25226'daki kullanarak Azure Gözcü Aracısı Syslog iletileri göndermek için Syslog daemon'u söyleyin. `sudo bash -c "printf 'filter f_local4_oms { facility(local4); };\n  destination security_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_local4_oms); destination(security_oms); };' > /etc/syslog-ng/security-config-omsagent.conf"`
              2. İndirme ve yükleme [security_events yapılandırma dosyası](https://aka.ms/asi-syslog-config-file-linux) Syslog aracı 25226'daki bağlantı noktasında dinleyecek şekilde yapılandırır. `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` Burada {0} çalışma GUID ile değiştirilmelidir.

              3. Syslog daemon'u başlatmak `sudo service syslog-ng restart` <br>Daha fazla bilgi için [syslog-ng belgeleri](https://www.syslog-ng.com/technical-documents/doc/syslog-ng-open-source-edition/3.16/mutual-authentication-using-tls/2)
      2. Bu komutu kullanarak Syslog aracıyı yeniden başlatın: `sudo /opt/microsoft/omsagent/bin/service_control restart [{workspace GUID}]`
      1. Hiçbir hata aracı günlüğünde şu komutu çalıştırarak onaylayın: `tail /var/opt/microsoft/omsagent/log/omsagent.log`

### <a name="deploy-the-agent-on-an-on-premises-linux-server"></a>Aracı üzerinde bir şirket içi Linux sunucusunda dağıtma

Azure kullanmıyorsanız, adanmış bir Linux sunucusu üzerinde çalıştırmak için Azure Gözcü aracıyı el ile dağıtın.


1. Gözcü Azure portalında **veri bağlayıcıları** ve gereç türünüzü seçin.
1. Altında adanmış bir Linux VM oluşturmak için **Linux Syslog aracı Yapılandırması** seçin **el ile dağıtım**.
   1. Altında **Syslog aracısını indirme ve yükleme**seçin **Azure olmayan Linux makine**. 
   1. İçinde **doğrudan aracı** seçtiğiniz açılır, ekran **Linux için aracıyı** aracıyı indirin veya Linux makinenizde indirmek için şu komutu çalıştırın:   `wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w {workspace GUID} -s gehIk/GvZHJmqlgewMsIcth8H6VqXLM9YXEpu0BymnZEJb6mEjZzCHhZgCx5jrMB1pVjRCMhn+XTQgDTU3DVtQ== -d opinsights.azure.com`
      1. Bağlayıcı ekranda altında **yapılandırma ve iletme Syslog**ayarlayın, Syslog daemon olup **rsyslog.d** veya **syslog-ng**. 
      1. Bu komutlar kopyalayın ve bunları gerecinizde çalıştırın:
         - Rsyslog seçtiyseniz:
           1. Tesis local_4 üzerinde dinleme ve bağlantı noktası 25226'daki kullanarak Azure Gözcü Aracısı Syslog iletileri göndermek için Syslog daemon'u söyleyin. `sudo bash -c "printf 'local4.debug  @127.0.0.1:25226' > /etc/rsyslog.d/security-config-omsagent.conf"`
            
           2. İndirme ve yükleme [security_events yapılandırma dosyası](https://aka.ms/asi-syslog-config-file-linux) Syslog aracı 25226'daki bağlantı noktasında dinleyecek şekilde yapılandırır. `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` Burada {0} çalışma GUID ile değiştirilmelidir.
           3. Syslog daemon'u başlatmak `sudo service rsyslog restart`
         - Syslog-ng seçtiyseniz:
            1. Tesis local_4 üzerinde dinleme ve bağlantı noktası 25226'daki kullanarak Azure Gözcü Aracısı Syslog iletileri göndermek için Syslog daemon'u söyleyin. `sudo bash -c "printf 'filter f_local4_oms { facility(local4); };\n  destination security_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_local4_oms); destination(security_oms); };' > /etc/syslog-ng/security-config-omsagent.conf"`
            2. İndirme ve yükleme [security_events yapılandırma dosyası](https://aka.ms/asi-syslog-config-file-linux) Syslog aracı 25226'daki bağlantı noktasında dinleyecek şekilde yapılandırır. `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` Burada {0} çalışma GUID ile değiştirilmelidir.
            3. Syslog daemon'u başlatmak `sudo service syslog-ng restart`
      1. Bu komutu kullanarak Syslog aracıyı yeniden başlatın: `sudo /opt/microsoft/omsagent/bin/service_control restart [{workspace GUID}]`
      1. Hiçbir hata aracı günlüğünde şu komutu çalıştırarak onaylayın: `tail /var/opt/microsoft/omsagent/log/omsagent.log`
  
## <a name="step-2-validate-connectivity"></a>2. Adım: Bağlantıyı doğrula

Çalınıyor Log Analytics'te görünmesini günlüklerinizi başlatana kadar 20 dakika sürebilir. 

1. Günlüklerinizi Syslog aracıyı doğru bağlantı noktasına aldığınızdan emin olun. Aracı makinede Syslog şu komutu çalıştırın: `tcpdump -A -ni any  port 514 -vv` Bu komut Syslog makineye CİHAZDAN akışı günlükleri gösterir. Günlükleri doğru bağlantı noktası ve doğru tesis kaynak gerecinde gelen alındığından emin olun.
2. Syslog cinini ve aracı arasındaki iletişim olup olmadığını denetleyin. Aracı makinede Syslog şu komutu çalıştırın: `tcpdump -A -ni any  port 25226 -vv` Bu komut Syslog makineye CİHAZDAN akışı günlükleri gösterir. Günlükleri de aracıda alındığından emin olun.
3. Bu komutların her ikisi de başarılı sonuçları sağlanan günlüklerinizi gelme görmek için Log Analytics kontrol edin. Bu gereçlerini akışa tüm olayları ham biçimde Log Analytics kapsamında görünen `CommonSecurityLog` türü.
1. Hatalar varsa veya günlükleri gelen olmayan denetlemek için konum `tail /var/opt/microsoft/omsagent/<workspace id>/log/omsagent.log`
4. Syslog iletisi varsayılan boyutunuz (2 KB) 2048 bayt ile sınırlı olduğundan emin olun. Günlükleri çok uzun olması durumunda, bu komutu kullanarak security_events.conf güncelleştirin: `message_length_limit 4096`
6. İlgili şema CEF olayları Log Analytics'te kullanmak için arama **CommonSecurityLog**.



## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, CEF cihazları Azure Gözcü için bağlama öğrendiniz. Azure Gözcü hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- Bilgi nasıl [görünürlük almak, veri ve olası tehditleri](quickstart-get-visibility.md).
- Başlama [Azure Gözcü kullanarak tehditleri algılama](tutorial-detect-threats.md).

