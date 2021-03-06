---
title: Azure Önizleme Gözcü Fortinet verilere | Microsoft Docs
description: Azure Gözcü için Fortinet veri bağlanmayı öğreneceksiniz.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: add92907-0d7c-42b8-a773-f570f2d705ff
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/17/2019
ms.author: rkarlin
ms.openlocfilehash: f3ab4861e874074e7de059c7c50064d53749748c
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67611292"
---
# <a name="connect-your-fortinet-appliance"></a>Fortinet gerecinize bağlanma

> [!IMPORTANT]
> Azure Sentinel şu anda genel Önizleme aşamasındadır.
> Bu önizleme sürümü, hizmet düzeyi sözleşmesi olmadan sağlanmaktadır. Üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için [Microsoft Azure önizlemeleri için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure Gözcü herhangi bir Fortinet gereç Syslog Common Event Format (CEF) olarak günlük dosyalarını kaydederek bağlanabilirsiniz. Gözcü Azure ile tümleştirme, kolayca analiz ve sorguları arasında günlük dosyası verilerini Fortinet çalıştırabilirsiniz. CEF verileri Azure Gözcü nasıl alan daha fazla bilgi için bkz: [bağlanma CEF Gereçleri](connect-common-event-format.md).

> [!NOTE]
> Veriler Azure Gözcü çalıştırdığınız çalışma alanının coğrafi konumda depolanır.

## <a name="step-1-connect-your-fortinet-appliance-by-using-an-agent"></a>1\. adım: Fortinet gerecinize bir aracı kullanarak bağlanma

Azure Gözcü için Fortinet cihazınıza bağlanmak için ayrılmış VM veya şirket içi makinede Gereci ve Azure Gözcü arasındaki iletişimi desteklemek için bir aracı dağıtın. Aracı otomatik olarak veya el ile dağıtabilirsiniz. Otomatik dağıtım, yalnızca ayrılmış makineniz Azure'da oluşturduğunuz yeni bir VM ise kullanılabilir.

Ayrıca, aracıyı vm'sinde başka bir bulut, mevcut bir Azure sanal makinesinde el ile veya bir şirket içi makinede dağıtabilirsiniz.

İki seçenek de ağ diyagramı için bkz [veri kaynağına bağlanın](connect-data-sources.md#agent-options).

### <a name="deploy-the-agent-in-azure"></a>Aracıyı azure'da dağıtın

1. Gözcü Azure portalında **veri bağlayıcıları** ve gereç türünüzü seçin.

1. Altında **Linux Syslog aracı Yapılandırması**:
   - Seçin **otomatik dağıtım** Azure Gözcü aracıyla birlikte önceden ve daha önce açıklanan tüm yapılandırma gerekirse, içeren yeni bir makine oluşturmak istiyorsanız. Seçin **otomatik dağıtım** > **otomatik aracı dağıtımı**. Otomatik olarak çalışma alanınıza bağlı olduğu adanmış bir VM için satın alma sayfası görüntülenir. VM bir **standart D2s v3 (2 Vcpu, 8 GB bellek)** ve genel bir IP adresi vardır.
      1. Üzerinde **özel dağıtım** sayfasında ayrıntılarınızı sağlamak, bir kullanıcı adı ve parola girin ve hüküm ve koşulları kabul ediyorsanız, VM satın alın.
      1. Bağlantı sayfada listelenen ayarları kullanarak günlükleri göndermek için gerecinizin yapılandırın. Genel Common Event Format bağlayıcısının bu ayarları kullanın:
         - Protokol UDP =
         - Bağlantı noktası 514 =
         - Özelliği yerel 4 =
         - Biçim CEF =
   - Seçin **el ile dağıtım** Gözcü Azure Aracısı'nı yüklendiği özel Linux makine varolan bir VM'yi kullanmak istiyorsanız. 
      1. Altında **Syslog aracısını indirme ve yükleme**seçin **Azure Linux sanal makinesi**. 
      1. Üzerinde **sanal makineler** ekranında, seçin ve kullanmak istediğiniz makineyi seçin **Connect**.
      1. Bağlayıcı ekranında, altında **yapılandırma ve iletme Syslog**ayarlayın, Syslog daemon olup **rsyslog.d** veya **syslog-ng**. 
      1. Bu komutlar kopyalayın ve bunları gerecinizde çalıştırın:
          - Rsyslog.d seçtiyseniz:
              
            1. Tesis local_4 üzerinde dinlenecek ve bağlantı noktası 25226'daki kullanarak Azure Gözcü aracıya Syslog iletilerini göndermek için Syslog daemon'u söyleyin. Bu komutu kullanın: `sudo bash -c "printf 'local4.debug  @127.0.0.1:25226\n\n:msg, contains, \"Fortinet\"  @127.0.0.1:25226' > /etc/rsyslog.d/security-config-omsagent.conf"`
            1. İndirme ve yükleme [security_events yapılandırma dosyası](https://aka.ms/asi-syslog-config-file-linux) Syslog aracı 25226'daki bağlantı noktasında dinleyecek şekilde yapılandırır. Bu komutu kullanın: `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` burada {0} çalışma GUID ile değiştirilmelidir.
            1. Bu komutu kullanarak syslog daemon'u başlatmak: `sudo service rsyslog restart`
             
          - Syslog-ng seçtiyseniz:

              1. Tesis local_4 üzerinde dinlenecek ve bağlantı noktası 25226'daki kullanarak Azure Gözcü aracıya Syslog iletilerini göndermek için Syslog daemon'u söyleyin. Bu komutu kullanın: `sudo bash -c "printf 'filter f_local4_oms { facility(local4); };\n  destination security_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_local4_oms); destination(security_oms); };\n\nfilter f_msg_oms { match(\"Fortinet\" value(\"MESSAGE\")); };\n  destination security_msg_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_msg_oms); destination(security_msg_oms); };' > /etc/syslog-ng/security-config-omsagent.conf"`
              1. İndirme ve yükleme [security_events yapılandırma dosyası](https://aka.ms/asi-syslog-config-file-linux) Syslog aracı 25226'daki bağlantı noktasında dinleyecek şekilde yapılandırır. Bu komutu kullanın: `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` burada {0} çalışma GUID ile değiştirilmelidir.
              1. Bu komutu kullanarak syslog daemon'u başlatmak: `sudo service syslog-ng restart`
      1. Syslog aracı, bu komutu kullanarak yeniden başlatın: `sudo /opt/microsoft/omsagent/bin/service_control restart [{workspace GUID}]`
      1. Hiçbir hata aracı günlüğünde şu komutu çalıştırarak onaylayın: `tail /var/opt/microsoft/omsagent/log/omsagent.log`

### <a name="deploy-the-agent-on-an-on-premises-linux-server"></a>Aracıyı bir şirket içi Linux sunucusunda dağıtma

Azure kullanmıyorsanız, adanmış bir Linux sunucusu üzerinde çalıştırmak için Azure Gözcü aracıyı el ile dağıtın.

1. Gözcü Azure portalında **veri bağlayıcıları** ve gereç türünüzü seçin.
1. Altında adanmış bir Linux VM oluşturmak için **Linux Syslog aracı Yapılandırması** seçin **el ile dağıtım**.

    1. Altında **Syslog aracısını indirme ve yükleme**seçin **Azure olmayan Linux makine**.
    1. İçinde **doğrudan aracı** seçtiğiniz açılır, ekran **Linux için aracıyı** aracıyı indirin veya Linux makinenizde indirmek için şu komutu çalıştırın: `wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w {workspace GUID} -s gehIk/GvZHJmqlgewMsIcth8H6VqXLM9YXEpu0BymnZEJb6mEjZzCHhZgCx5jrMB1pVjRCMhn+XTQgDTU3DVtQ== -d opinsights.azure.com`

       1. Bağlayıcı ekranda altında **yapılandırma ve iletme Syslog**ayarlayın, Syslog daemon olup **rsyslog.d** veya **syslog-ng**.
       1. Bu komutlar kopyalayın ve bunları gerecinizde çalıştırın:

          - Rsyslog seçtiyseniz:

            1. Tesis local_4 üzerinde dinleme ve bağlantı noktası 25226'daki kullanarak Azure Gözcü Aracısı Syslog iletileri göndermek için Syslog daemon'u söyleyin. Bu komutu kullanın: `sudo bash -c "printf 'local4.debug  @127.0.0.1:25226\n\n:msg, contains, \"Fortinet\"  @127.0.0.1:25226' > /etc/rsyslog.d/security-config-omsagent.conf"`
            1. İndirme ve yükleme [security_events yapılandırma dosyası](https://aka.ms/asi-syslog-config-file-linux) Syslog aracı 25226'daki bağlantı noktasında dinleyecek şekilde yapılandırır. Bu komutu kullanın: `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` burada {0} çalışma GUID ile değiştirilmelidir.
            1. Bu komutu kullanarak syslog daemon'u başlatmak: `sudo service rsyslog restart`

          - Syslog-ng seçtiyseniz:

            1. Tesis local_4 üzerinde dinlenecek ve bağlantı noktası 25226'daki kullanarak Azure Gözcü aracıya Syslog iletilerini göndermek için Syslog daemon'u söyleyin. Bu komutu kullanın: `sudo bash -c "printf 'filter f_local4_oms { facility(local4); };\n  destination security_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_local4_oms); destination(security_oms); };\n\nfilter f_msg_oms { match(\"Fortinet\" value(\"MESSAGE\")); };\n  destination security_msg_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_msg_oms); destination(security_msg_oms); };' > /etc/syslog-ng/security-config-omsagent.conf"`
            1. İndirme ve yükleme [security_events yapılandırma dosyası](https://aka.ms/asi-syslog-config-file-linux) Syslog aracı 25226'daki bağlantı noktasında dinleyecek şekilde yapılandırır. Bu komutu kullanın: `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` burada {0} çalışma GUID ile değiştirilmelidir.
            1. Bu komutu kullanarak syslog daemon'u başlatmak: `sudo service syslog-ng restart`

      1. Syslog aracı, bu komutu kullanarak yeniden başlatın: `sudo /opt/microsoft/omsagent/bin/service_control restart [{workspace GUID}]`
      1. Hiçbir hata aracı günlüğünde şu komutu çalıştırarak onaylayın: `tail /var/opt/microsoft/omsagent/log/omsagent.log`
 
## <a name="step-2-forward-fortinet-logs-to-the-syslog-agent"></a>2\. adım: Fortinet günlükleri Syslog aracıya ilet

Fortinet Syslog aracı üzerinden Azure çalışma alanınıza CEF biçiminde Syslog iletilerini iletecek şekilde yapılandırın.

1. Fortinet appliance'ınız üzerinde CLI'yı açın ve aşağıdaki komutları çalıştırın:

        config log syslogd setting
        set format cef
        set facility <facility_name>
        set port 514
        set reliable disable
        set server <ip_address_of_Receiver>
        set status enable
        end

    - Sunucu değiştirin **IP adresi** aracısının IP adresine sahip.
    - Ayarlama **facility_name** aracıyı yapılandırdığınız tesis kullanılacak. Varsayılan olarak, aracı için local4 bu ayarlar.
    - Ayarlama **syslog bağlantı noktası** için **514** ya da aracıda bağlantı noktası.
    - Erken FortiOS sürümlerinde olduğunda CEF biçimini etkinleştirmek için komut kümesini çalıştırmak ihtiyacınız olabilecek **csv devre dışı**.
 
   > [!NOTE] 
   > Daha fazla bilgi için Git [Fortinet belge kitaplığı](https://aka.ms/asi-syslog-fortinet-fortinetdocumentlibrary). Sürümünüzü seçin ve **el kitabı** ve **günlük iletisi başvurusu**.

 İlgili şema Fortinet olaylar için Azure İzleyici Log Analytics'te kullanmak için arama `CommonSecurityLog`.


## <a name="step-3-validate-connectivity"></a>3\. adım: Bağlantıyı doğrula

İşlem günlüklerinizi Log Analytics'te görünmesini başlatana kadar en fazla 20 dakika sürebilir. 

1. Doğru tesis kullandığınızdan emin olun. Tesis gerecinize ve Azure Gözcü'de aynı olmalıdır. Azure Gözcü içinde kullanıyorsanız ve dosyada değişiklik hangi tesis dosyasını kontrol edebilirsiniz `security-config-omsagent.conf`. 

2. Günlüklerinizi Syslog aracıyı doğru bağlantı noktasına aldığınızdan emin olun. Aracı makinede Syslog şu komutu çalıştırın: `tcpdump -A -ni any  port 514 -vv`. Bu komut Syslog makineye CİHAZDAN akış günlüklerini gösterir. Günlükleri doğru bağlantı noktasının ve doğru tesis kaynak gerecinde gelen alındığından emin olun.

3. Gönderdiğiniz günlükler ile uyumlu olduğundan emin [RFC 5424](https://tools.ietf.org/html/rfc542).

4. Bilgisayarda Syslog aracıyı çalıştıran 514 ve 25226'daki bağlantı noktalarının açık ve dinleme komutunu kullanarak olduğundan emin olun `netstat -a -n:`. Bu komutu kullanma hakkında daha fazla bilgi için bkz. [netstat(8) - Linux man sayfa](https://linux.die.net/man/8/netstat). Düzgün dinliyorsa, görürsünüz:

   ![Azure Sentinel bağlantı noktaları](./media/connect-cef/ports.png) 

5. Arka plan programının günlüklerini gönderiyor olun, 514 bağlantı noktasında dinleyecek şekilde ayarlandığından emin olun.
    - Rsyslog için:<br>Emin olun dosya `/etc/rsyslog.conf` bu yapılandırma şunları içerir:

           # provides UDP syslog reception
           module(load="imudp")
           input(type="imudp" port="514")
        
           # provides TCP syslog reception
           module(load="imtcp")
           input(type="imtcp" port="514")

      Daha fazla bilgi için [imudp: UDP Syslog girdir modüllerden](https://www.rsyslog.com/doc/v8-stable/configuration/modules/imudp.html#imudp-udp-syslog-input-module) ve [imtcp: TCP Syslog girdir modüllerden](https://www.rsyslog.com/doc/v8-stable/configuration/modules/imtcp.html#imtcp-tcp-syslog-input-module).

   - Syslog-ng:<br>Emin olun dosya `/etc/syslog-ng/syslog-ng.conf` bu yapılandırma şunları içerir:

           # source s_network {
            network( transport(UDP) port(514));
             };
     Daha fazla bilgi için [imudp: UDP Syslog girdir modüllerden](https://rsyslog.readthedocs.io/en/latest/configuration/modules/imudp.html) ve [syslog-ng açık kaynak sürümü 3.16 - Yönetim Kılavuzu](https://www.syslog-ng.com/technical-documents/doc/syslog-ng-open-source-edition/3.16/administration-guide/19#TOPIC-956455).

1. Syslog cinini ve aracı arasındaki iletişim olup olmadığını denetleyin. Aracı makinede Syslog şu komutu çalıştırın: `tcpdump -A -ni any  port 25226 -vv`. Bu komut Syslog makineye CİHAZDAN akış günlüklerini gösterir. Günlükleri de aracıda alındığından emin olun.

6. Bu komutların her ikisi de başarılı sonuçları sağlanan günlüklerinizi gelme görmek için Log Analytics kontrol edin. Bu gereçlerini akışa tüm olayları ham biçimde Log Analytics kapsamında görünen `CommonSecurityLog` türü.

7. Hataları olup olmadığını denetleyin veya günlükleri gelen değil, konum `tail /var/opt/microsoft/omsagent/<workspace id>/log/omsagent.log`. Günlük biçimi uyumsuzluğu hatası derse gidin `/etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` ve dosyaya bakması `security_events.conf`. Günlüklerinizi bu dosyada gördüğünüz normal ifade biçimi eşleştiğinden emin olun.

8. Syslog iletisi varsayılan boyutunuz (2 KB) 2048 bayt ile sınırlı olduğundan emin olun. Günlükleri çok uzun olması durumunda, bu komutu kullanarak security_events.conf güncelleştirin: `message_length_limit 4096`

10. Fortinet günlüklerinizi aracı tarafından alınan değil, tesis ayarlayın ve günlükleri Fortinet Word'de aramak için günlükleri ayarlamak için kullanmakta olduğunuz Syslog daemon'u hangi türüne bağlı olarak bu komutu çalıştırın:
       - rsyslog.d: `sudo bash -c "printf 'local4.debug  @127.0.0.1:25226\n\n:msg, contains, \"Fortinet\"  @127.0.0.1:25226' > /etc/rsyslog.d/security-config-omsagent.conf"`

     Bu komutu kullanarak Syslog daemon'u başlatmak: `sudo service rsyslog restart`
       - Syslog-ng: `sudo bash -c "printf 'filter f_local4_oms { facility(local4); };\n  destination security_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_local4_oms); destination(security_oms); };\n\nfilter f_msg_oms { match(\"Fortinet\" value(\"MESSAGE\")); };\n  destination security_msg_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_msg_oms); destination(security_msg_oms); };' > /etc/syslog-ng/security-config-omsagent.conf"`
      
     Bu komutu kullanarak Syslog daemon'u başlatmak: `sudo service syslog-ng restart`

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Azure Gözcü için Fortinet cihazları bağlayın öğrendiniz. Azure Gözcü hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- Bilgi nasıl [görünürlük almak, veri ve olası tehditleri](quickstart-get-visibility.md).
- Başlama [Azure Gözcü kullanarak tehditleri algılama](tutorial-detect-threats.md).

