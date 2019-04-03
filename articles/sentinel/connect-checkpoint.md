---
title: Toplama denetim noktası verileri Azure Gözcü önizlemesinde | Microsoft Docs
description: Azure Gözcü olarak denetim noktası verilerini nasıl toplayacağınızı öğrenin.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 3229233d-400d-4971-8d76-eaa0d6591d75
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/6/2019
ms.author: rkarlin
ms.openlocfilehash: 1fb4f9165be03a7fc3cd055ef616dcfadb58ac9d
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58876505"
---
# <a name="connect-your-check-point-appliance"></a>Denetim noktası gerecinize bağlanma

> [!IMPORTANT]
> Azure Sentinel şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure Gözcü herhangi bir denetim noktası gereç Syslog CEF günlük dosyalarını kaydederek bağlanabilirsiniz. Gözcü Azure ile tümleştirme kolayca analiz ve sorguları kontrol noktasından arasında günlük dosyası verilerini çalıştırmanıza olanak sağlar. CEF verileri Azure Gözcü nasıl alan daha fazla bilgi için bkz: [bağlanma CEF Gereçleri](connect-common-event-format.md).

> [!NOTE]
> - Veriler Azure Gözcü çalıştırıyorsanız çalışma alanının coğrafi konumda depolanır.

## <a name="step-1-connect-your-check-point-appliance-using-an-agent"></a>1. Adım: Denetim noktası gerecinize bir aracı kullanarak bağlanma

Azure Gözcü için denetim noktası cihazınıza bağlanmak için adanmış bir makinede (VM veya şirket içi) Gereci ve Azure Gözcü arasındaki iletişimi destekleyen bir aracı dağıtmak gerekir. Aracı otomatik olarak veya el ile dağıtabilirsiniz. Otomatik dağıtım, yalnızca ayrılmış makineniz Azure'da oluşturduğunuz yeni bir VM ise kullanılabilir. 

Alternatif olarak, aracı vm'sinde başka bir bulut, mevcut bir Azure sanal makinesinde el ile veya bir şirket içi makinede dağıtabilirsiniz.

İki seçenek de ağ diyagramı için bkz [veri kaynağına bağlanın](connect-data-sources.md).

### <a name="deploy-the-agent-in-azure"></a>Aracıyı azure'da dağıtın

1. Gözcü Azure portalında **veri toplama** ve gereç türünüzü seçin. 

1. Altında **Linux Syslog aracı Yapılandırması**:
   - Seçin **otomatik dağıtım** yukarıda açıklandığı gibi Azure Gözcü aracıyla birlikte önceden yüklenir ve tüm yapılandırma gerekli içeren yeni bir makine oluşturmak istiyorsanız. Seçin **otomatik dağıtım** tıklatıp **otomatik aracı dağıtımı**. Bu, satın alma sayfasına otomatik olarak çalışma alanınıza bağlı olduğu adanmış bir VM için götürür. VM bir **standart D2s v3 (2 vcpu, 8 GB bellek)** ve genel bir IP adresi vardır.
     1. İçinde **özel dağıtım** sayfasında ayrıntılarınızı sağlamak ve bir kullanıcı adı ve parola seçin ve hüküm ve koşulları kabul ediyorsanız, VM satın alın.
      
        1. Syslog aracı makinedeki tüm denetim noktası günlüklerini Azure Gözcü aracıya eşleştirilecek emin olmak için şu komutları çalıştırın:
           - Syslog-ng kullanıyorsanız (Syslog aracıyı yeniden başlatır. Not) şu komutları çalıştırın:
            
                sudo bash - c "printf ' filtre f_local4_oms {facility(local4);}; \ n hedef security_oms {tcp (\"127.0.0.1\" port(25226));}; \n günlük {source(src) filter(f_local4_oms); destination(security_oms);}; \n\nfilter f_msg_oms {eşleşen (\"denetim noktası\" değeri (\" İleti\")); }; \n hedef security_msg_oms {tcp (\"127.0.0.1\" port(25226));}; \n günlük {source(src) filter(f_msg_oms); destination(security_msg_oms);};' > /etc/syslog-ng/security-config-omsagent.conf "

             Syslog daemon'unu yeniden başlatın: `sudo service syslog-ng restart`
           - Rsyslog kullanıyorsanız (Syslog aracıyı yeniden başlatır. Not) şu komutları çalıştırın:
                    
                 sudo bash -c "printf 'local4.debug  @127.0.0.1:25226\n\n:msg, contains, \"Check Point\"  @127.0.0.1:25226' > /etc/rsyslog.d/security-config-omsagent.conf"
             Syslog daemon'unu yeniden başlatın: `sudo service rsyslog restart`

   - Seçin **el ile dağıtım** ileride Azure Gözcü aracısının yüklenmesi gerekir özel Linux makine varolan bir VM'yi kullanmak istiyorsanız. 
      1. Altında **Syslog aracısını indirme ve yükleme**seçin **Azure Linux sanal makinesi**. 
      1. İçinde **sanal makineler** açılır, ekran seçin'e tıklayın, istediğiniz makine **Connect**.
      1. Bağlayıcı ekranda altında **yapılandırma ve iletme Syslog**ayarlayın, Syslog daemon olup **rsyslog.d** veya **syslog-ng**. 
      1. Bu komutlar kopyalayın ve bunları gerecinizde çalıştırın:
          - Rsyslog.d seçtiyseniz:
              
            1. Tesis local_4 ve "Check Point" dinlemek ve bağlantı noktası 25226'daki kullanarak Azure Gözcü Aracısı Syslog iletilerini göndermek için Syslog daemon'u söyleyin. `sudo bash -c "printf 'local4.debug  @127.0.0.1:25226\n\n:msg, contains, \"Check Point\"  @127.0.0.1:25226' > /etc/rsyslog.d/security-config-omsagent.conf"`
            
            2. İndirme ve yükleme [security_events yapılandırma dosyası](https://aka.ms/asi-syslog-config-file-linux) Syslog aracı 25226'daki bağlantı noktasında dinleyecek şekilde yapılandırır. `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` Burada {0} çalışma GUID ile değiştirilmelidir.
           
            1. Syslog daemon'u başlatmak `sudo service rsyslog restart`
             
          - Syslog-ng seçtiyseniz:

              1. Tesis local_4 ve "Check Point" dinlemek ve bağlantı noktası 25226'daki kullanarak Azure Gözcü Aracısı Syslog iletilerini göndermek için Syslog daemon'u söyleyin. `sudo bash -c "printf 'filter f_local4_oms { facility(local4); };\n  destination security_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_local4_oms); destination(security_oms); };\n\nfilter f_msg_oms { match(\"Check Point\" value(\"MESSAGE\")); };\n  destination security_msg_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_msg_oms); destination(security_msg_oms); };' > /etc/syslog-ng/security-config-omsagent.conf"`
              2. İndirme ve yükleme [security_events yapılandırma dosyası](https://aka.ms/asi-syslog-config-file-linux) Syslog aracı 25226'daki bağlantı noktasında dinleyecek şekilde yapılandırır. `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` Burada {0} çalışma GUID ile değiştirilmelidir.

              3. Syslog daemon'u başlatmak `sudo service syslog-ng restart`
      2. Bu komutu kullanarak Syslog aracıyı yeniden başlatın: `sudo /opt/microsoft/omsagent/bin/service_control restart [{workspace GUID}]`
      1. Hiçbir hata aracı günlüğünde şu komutu çalıştırarak onaylayın: `tail /var/opt/microsoft/omsagent/log/omsagent.log`

### <a name="deploy-the-agent-on-an-on-premises-linux-server"></a>Aracı üzerinde bir şirket içi Linux sunucusunda dağıtma

Azure kullanmıyorsanız, adanmış bir Linux sunucusu üzerinde çalıştırmak için Azure Gözcü aracıyı el ile dağıtın.

1. Altında adanmış bir Linux VM oluşturmak için **Linux Syslog aracı Yapılandırması** seçin **el ile dağıtım**.
   1. Altında **Syslog aracısını indirme ve yükleme**seçin **Azure olmayan Linux makine**. 
   1. İçinde **doğrudan aracı** seçtiğiniz açılır, ekran **Linux için aracıyı** aracıyı indirin veya Linux makinenizde indirmek için şu komutu çalıştırın:   `wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w {workspace GUID} -s gehIk/GvZHJmqlgewMsIcth8H6VqXLM9YXEpu0BymnZEJb6mEjZzCHhZgCx5jrMB1pVjRCMhn+XTQgDTU3DVtQ== -d opinsights.azure.com`
      1. Bağlayıcı ekranda altında **yapılandırma ve iletme Syslog**ayarlayın, Syslog daemon olup **rsyslog.d** veya **syslog-ng**. 
      1. Bu komutlar kopyalayın ve bunları gerecinizde çalıştırın:
         - Seçtiyseniz **rsyslog**:
           1. Tesis local_4 ve "Check Point" dinlemek ve bağlantı noktası 25226'daki kullanarak Azure Gözcü Aracısı Syslog iletilerini göndermek için Syslog daemon'u söyleyin. `sudo bash -c "printf 'local4.debug  @127.0.0.1:25226\n\n:msg, contains, \"Check Point\"  @127.0.0.1:25226' > /etc/rsyslog.d/security-config-omsagent.conf"`
            
           2. İndirme ve yükleme [security_events yapılandırma dosyası](https://aka.ms/asi-syslog-config-file-linux) Syslog aracı 25226'daki bağlantı noktasında dinleyecek şekilde yapılandırır. `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` Burada {0} çalışma GUID ile değiştirilmelidir.
           3. Syslog daemon'u başlatmak `sudo service rsyslog restart`
         - Seçtiyseniz **syslog-ng**:
            1. Tesis local_4 ve "Check Point" dinlemek ve bağlantı noktası 25226'daki kullanarak Azure Gözcü Aracısı Syslog iletilerini göndermek için Syslog daemon'u söyleyin. `sudo bash -c "printf 'filter f_local4_oms { facility(local4); };\n  destination security_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_local4_oms); destination(security_oms); };\n\nfilter f_msg_oms { match(\"Check Point\" value(\"MESSAGE\")); };\n  destination security_msg_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_msg_oms); destination(security_msg_oms); };' > /etc/syslog-ng/security-config-omsagent.conf"`
            2. İndirme ve yükleme [security_events yapılandırma dosyası](https://aka.ms/asi-syslog-config-file-linux) Syslog aracı 25226'daki bağlantı noktasında dinleyecek şekilde yapılandırır. `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` Burada {0} çalışma GUID ile değiştirilmelidir.
            3. Syslog daemon'u başlatmak `sudo service syslog-ng restart`
      1. Bu komutu kullanarak Syslog aracıyı yeniden başlatın: `sudo /opt/microsoft/omsagent/bin/service_control restart [{workspace GUID}]`
      1. Hiçbir hata aracı günlüğünde şu komutu çalıştırarak onaylayın: `tail /var/opt/microsoft/omsagent/log/omsagent.log`
 
## <a name="step-2-forward-check-point-logs-to-the-syslog-agent"></a>2. Adım: İleri kontrol noktası Syslog aracıya günlük

Denetim noktası gerecinize Syslog aracı üzerinden Azure çalışma alanınıza CEF biçiminde Syslog iletilerini iletecek şekilde yapılandırın.

1. Git [kontrol noktası günlüğünü dışarı aktarma](https://aka.ms/asi-syslog-checkpoint-forwarding).
2. Ekranı aşağı kaydırarak **temel dağıtımı** ve aşağıdaki yönergeleri kullanarak bağlantıyı ayarlamak için yönergeleri izleyin:
   - Ayarlama **Syslog bağlantı noktası** için **514** veya aracı üzerinde ayarladığınız bağlantı noktası.
     - Değiştirin **adı** ve **hedef sunucu IP adresi** CLI Syslog aracı adı ve IP adresi.
     - Biçim kümesine **CEF**.
3. Sürüm R77.30 veya R80.10 kullanıyorsanız kadar kaydırın **yüklemeleri** ve günlük verici sürümünüzün yüklemek için yönergeleri izleyin.
 
## <a name="step-3-validate-connectivity"></a>3. Adım: Bağlantıyı doğrula

Çalınıyor Log Analytics'te görünmesini günlüklerinizi başlatana kadar 20 dakika sürebilir. 

1. Günlüklerinizi Syslog aracıyı doğru bağlantı noktasına aldığınızdan emin olun. Aracı makinede Syslog şu komutu çalıştırın: `tcpdump -A -ni any  port 514 -vv` Bu komut Syslog makineye CİHAZDAN akışı günlükleri gösterir. Günlükleri doğru bağlantı noktası ve doğru tesis kaynak gerecinde gelen alındığından emin olun.
2. Syslog cinini ve aracı arasındaki iletişim olup olmadığını denetleyin. Aracı makinede Syslog şu komutu çalıştırın: `tcpdump -A -ni any  port 25226 -vv` Bu komut Syslog makineye CİHAZDAN akışı günlükleri gösterir. Günlükleri de aracıda alındığından emin olun.
3. Bu komutların her ikisi de başarılı sonuçları sağlanan günlüklerinizi gelme görmek için Log Analytics kontrol edin. Bu gereçlerini akışa tüm olayları ham biçimde Log Analytics kapsamında görünen `CommonSecurityLog` türü.

4. Bu komutları çalıştırmak emin olun:
  
   - Syslog-ng kullanıyorsanız (Syslog aracıyı yeniden başlatır. Not) şu komutları çalıştırın:

         sudo bash -c "printf 'filter f_local4_oms { facility(local4); };\n  destination security_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_local4_oms); destination(security_oms); };\n\nfilter f_msg_oms { match(\"Check Point\" value(\"MESSAGE\")); };\n  destination security_msg_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_msg_oms); destination(security_msg_oms); };' > /etc/syslog-ng/security-config-omsagent.conf"
        Syslog daemon'unu yeniden başlatın: `sudo service syslog-ng restart`

   - Rsyslog kullanıyorsanız (Syslog aracıyı yeniden başlatır. Not) şu komutları çalıştırın: 

         sudo bash -c "printf 'local4.debug @127.0.0.1:25226\n\n:msg, contains, "Check Point" @127.0.0.1:25226' > /etc/rsyslog.d/security-config-omsagent.conf"
     Syslog daemon'unu yeniden başlatın: `sudo service rsyslog restart`

1. Hatalar varsa veya günlükleri gelen olmayan denetlemek için konum `tail /var/opt/microsoft/omsagent/<workspace id>/log/omsagent.log`
4. Syslog iletisi varsayılan boyutunuz (2 KB) 2048 bayt ile sınırlı olduğundan emin olun. Günlükleri çok uzun olması durumunda, bu komutu kullanarak security_events.conf güncelleştirin: `message_length_limit 4096`



## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Gözcü için denetim noktası cihazları bağlayın öğrendiniz. Azure Gözcü hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- Bilgi nasıl [görünürlük almak, veri ve olası tehditleri](quickstart-get-visibility.md).
- Başlama [Azure Gözcü kullanarak tehditleri algılama](tutorial-detect-threats.md).

