---
title: "Linux bilgisayarları Operations Management Suite (OMS) bağlanma | Microsoft Docs"
description: "Bu makalede, Azure, diğer Bulut veya şirket içi Linux için OMS Aracısı'nı kullanarak OMS barındırılan Linux bilgisayarları bağlamak açıklar."
services: log-analytics
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: 
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/29/2017
ms.author: magoedte
ms.openlocfilehash: c9902e1b8644c2b0a894f9cde98f2056564775c7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="connect-your-linux-computers-to-operations-management-suite-oms"></a>Linux bilgisayarları Operations Management Suite (OMS) bağlanma 

Microsoft Operations Management Suite (OMS) ile toplama ve hareket Linux bilgisayarları ve fiziksel sunucularda veya sanal makineleri, sanal makinelerinizde olarak şirket içi veri Merkezinize bulunan Docker gibi kapsayıcı çözümlerinden oluşturulan veriler üzerinde bir Amazon Web Hizmetleri (AWS) veya Microsoft Azure gibi bulutta barındırılan hizmet. Yönetim çözümleri OMS içinde kullanılabilir değişiklik izleme gibi yapılandırma değişiklikleri ve Linux VM'ler yaşam döngüsü ileriye dönük olarak yönetmek için yazılım güncelleştirmelerini yönetmek için güncelleştirme yönetimi tanımlamak için de kullanabilirsiniz. 

Linux için OMS Aracısı TCP bağlantı noktası 443 giden OMS hizmetiyle iletişim kurar ve bilgisayar Internet üzerinden iletişim kurmak için bir güvenlik duvarı veya proxy sunucusu bağlanırsa gözden [aracı kullanmak için bir HTTP proxy sunucusu veya OMS ile yapılandırma Ağ geçidi](#configuring-the-agent-for-use-with-an-http-proxy-server-or-oms-gateway) hangi yapılandırma değişiklikleri anlamak için uygulanması gerekir.  System Center 2016 - Operations Manager veya Operations Manager 2012 R2'de, bilgisayarla izliyorsanız çok konaklı veri toplamak ve Hizmeti'ne iletmek ve Operations Manager tarafından yine izlenmesi için OMS hizmetiyle olabilir.  OMS ile tümleşik bir Operations Manager yönetim grubu tarafından izlenen Linux bilgisayarlar veri kaynakları için yapılandırma almaz ve yönetim grubu ile veri oplanan.  Rapor birden fazla çalışma alanına OMS Aracısı yapılandırılamaz.  

BT güvenlik ilkelerinizi bilgisayarları Internet'e bağlanmak için ağınızdaki izin vermiyorsa, aracı yapılandırma bilgilerini almak ve etkinleştirdiğiniz çözümüne bağlı olarak toplanan verileri göndermek için OMS ağ geçidine bağlanmak için yapılandırılabilir. Daha fazla bilgi ve OMS hizmetine bir OMS ağ geçidi üzerinden iletişim kurmak için OMS Linux aracınızı yapılandırma adımları için bkz: [OMS ağ geçidini kullanarak OMS bilgisayarları bağlamak](log-analytics-oms-gateway.md).  

Aşağıdaki diyagram, OMS bağlantı noktaları ve yön dahil olmak üzere ve aracıyla yönetilen Linux bilgisayarlar arasındaki bağlantıyı gösterir.

![OMS diyagramı ile doğrudan aracı iletişimi](./media/log-analytics-agent-linux/log-analytics-agent-linux-communication.png)

## <a name="system-requirements"></a>Sistem gereksinimleri
Başlamadan önce önkoşulları karşılaması doğrulamak için aşağıdaki ayrıntıları gözden geçirin.

### <a name="supported-linux-operating-systems"></a>Desteklenen Linux işletim sistemleri
Aşağıdaki Linux dağıtımları resmi olarak desteklenir.  Ancak, OMS Aracısı Linux için listelenmeyen diğer dağıtımlar da çalıştırabilirsiniz.

* Amazon Linux için 2015.09 2012.09 (x86/x64)
* CentOS Linux 5, 6 ve 7 (x86/x64)
* Oracle Linux 5, 6 ve 7 (x86/x64)
* Red Hat Enterprise Linux Server 5, 6 ve 7 (x86/x64)
* Debian GNU/Linux 6, 7 ve 8 (x86/x64)
* Ubuntu 12.04 LTS, 14.04 LTS, 15.04, 15.10, 16.04 LTS (x86/x64)
* SUSE Linux Enterprise Server 11 ve 12 (x86/x64)

### <a name="network"></a>Ağ
Aşağıdaki listeden OMS ile iletişim kurmak Linux aracısı için gerekli proxy ve güvenlik duvarı yapılandırma bilgilerini bilgileri. OMS hizmete ağınızdan giden trafiğidir. 

|Aracı Kaynağı| Bağlantı Noktaları |  
|------|---------|  
|*.ods.opinsights.azure.com | Bağlantı noktası 443|   
|*.oms.opinsights.azure.com | Bağlantı noktası 443|   
|*.BLOB.Core.Windows.NET/ | Bağlantı noktası 443|   
|*.azure-automation.net | Bağlantı noktası 443|  

### <a name="package-requirements"></a>Paket gereksinimleri

 **Gerekli paket**   | **Açıklama**   | **En düşük sürüm**
--------------------- | --------------------- | -------------------
Glibc | GNU C Kitaplığı   | 2.5-12 
Openssl | OpenSSL kitaplıkları | 0.9.8E veya 1.0
Curl | cURL web istemcisi | 7.15.5
Python ctypes | | 
PAM | Eklenebilir kimlik doğrulaması modülleri | 

> [!NOTE]
>  Rsyslog veya syslog ng syslog iletileri toplamak için gereklidir. Red Hat Enterprise Linux, CentOS ve Oracle Linux sürümü (sysklog) 5 sürümünü varsayılan syslog arka plan syslog olay toplaması için desteklenmiyor. Bu dağıtımları bu sürümünden Syslog verileri toplamak için rsyslog arka plan programı yüklenmeli ve sysklog değiştirmek üzere yapılandırılmamışsa, 

Aracı birden çok paket içerir. Aşağıdaki paketler, kabuk Paketle çalıştırarak kullanılabilir sürüm dosyasını içeren `--extract`:

**Paket** | **Sürüm** | **Açıklama**
----------- | ----------- | --------------
omsagent | 1.4.1 | Linux için Operations Management Suite Aracısı
omsconfig | 1.1.1 | OMS aracısı için yapılandırma aracısı
OMI | 1.2.0 | Yönetim Altyapısı (OMI) - hafif bir CIM sunucusu açın
scx | 1.6.3 | İşletim sistemi performans ölçümleri için OMI CIM sağlayıcıları
Apache cimprov | 1.0.1 | Apache HTTP Server performansı sağlayıcısı OMI için izleme. Apache HTTP Server algılanırsa, yüklü.
MySQL cimprov | 1.0.1 | MySQL sunucusu performans sağlayıcısı OMI için izleme. MySQL/MariaDB sunucu algılanırsa, yüklü.
docker cimprov | 1.0.0 | OMI docker sağlayıcısı. Docker algılanırsa, yüklü.

### <a name="compatibility-with-system-center-operations-manager"></a>System Center Operations Manager ile uyumluluk
Linux için OMS aracısının Aracısı ikili dosyaları System Center Operations Manager Aracısı ile paylaşır. Şu anda Operations Manager tarafından yönetilen bir sistemde Linux için OMS Aracısı yüklerseniz, bilgisayardaki OMI ve SCX paketleri daha yeni bir sürüme yükseltir. Bu sürümde, OMS ve System Center 2016 - Operations Manager/Operations Manager 2012 R2 aracıları Linux için uyumlu değildir. 

> [!NOTE]
> System Center 2012 SP1 ve önceki sürümleri şu anda Linux için OMS Aracısı ile desteklenen veya uyumlu değildir.<br>
> Linux için OMS Aracısı şu anda Operations Manager tarafından izlenmeyen bir bilgisayara yüklenir ve ardından bilgisayarı Operations Manager ile izlemek istediğiniz, değiştirmeniz gerekir [OMI yapılandırma](#enable-the-oms-agent-for-linux-to-report-to-system-center-operations-manager) keşfetme önce bilgisayar. **Bu adım *değil* önce OMS Aracısı'nı Linux için Operations Manager Aracısı yüklüyse gerekli.**

### <a name="system-configuration-changes"></a>Sistem yapılandırması değişiklikleri
Linux paketler için OMS Aracısı yükledikten sonra aşağıdaki ek sistem genelinde yapılandırma değişikliklerini uygulanır. Bu yapıtların omsagent paket kaldırıldığında kaldırılır.

* Adlı bir ayrıcalıklı olmayan kullanıcı: `omsagent` oluşturulur. Bu hesap olarak omsagent arka plan programı çalıştırır.
* "Ekle" sudoers dosya /etc/sudoers.d/omsagent oluşturulur. Bu dosya syslog ve omsagent Daemon başlatmayı omsagent yetkilendirir. Sudo "Ekle" yönergeleri sudo yüklü sürümü desteklenmiyorsa, bu girişler için /etc/sudoers yazılır.
* Syslog yapılandırma aracıya bir alt olayların iletecek şekilde değiştirilir. Daha fazla bilgi için bkz: **yapılandırma veri toplama** bölümüne bakın.

### <a name="upgrade-from-a-previous-release"></a>Önceki bir sürümünden yükseltme
Bu sürümde 1.0.0-47 desteklenenden daha önceki sürümlerden yükseltin. Yükleme işlemine gerçekleştirme `--upgrade` komutu agent'ın tüm bileşenleri en son sürüme yükseltir.

## <a name="installing-the-agent"></a>Aracı yükleme

Bu bölümde, Debian ve RPM paketlerini her aracı bileşenlerini içeren bir bunndle kullanarak Linux için OMS Aracısı'nı yüklemeyi açıklar.  Doğrudan yüklü veya tek tek paketleri almaya ayıklanır.  

Öncelikle, OMS çalışma alanı kimliği ve geçiş yaparak bulabilirsiniz anahtarınızı gerekir [OMS Klasik portal](https://mms.microsoft.com).  Üzerinde **genel bakış** sayfasından üst menü Seç **ayarları**ve ardından gidin **bağlı Sources\Linux sunuculara**.  Değerin sağındaki bkz **çalışma alanı kimliği** ve **birincil anahtar**.  Kopyalayın ve her ikisi de, sık kullanılan düzenleyicisine yapıştırın.    

1. En son karşıdan [Linux (x64) için OMS aracısının](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/OMSAgent_GA_v1.4.1-45/omsagent-1.4.1-45.universal.x64.sh) veya [Linux x86 için OMS aracısının](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/OMSAgent_GA_v1.4.1-45/omsagent-1.4.1-45.universal.x86.sh) github'dan.  
2. Uygun paket (x86 veya x64) scp/sftp kullanarak Linux bilgisayarınıza aktarın.
3. Kullanarak paket yükleme `--install` veya `--upgrade` bağımsız değişkeni. 

    > [!NOTE]
    > Varolan tüm paketler, ne zaman Linux için System Center Operations Manager Aracısı zaten yüklü gibi yüklediyseniz kullanın `--upgrade` bağımsız değişkeni. Yükleme sırasında Operations Management Suite'e bağlamak için sağlamanız `-w <WorkspaceID>` ve `-s <Shared Key>` parametreleri.


#### <a name="to-install-and-onboard-directly"></a>Yüklemek için ve yerleşik doğrudan
```
sudo sh ./omsagent-<version>.universal.x64.sh --upgrade -w <workspace id> -s <shared key>
```

#### <a name="to-upgrade-the-agent-package"></a>Aracı paketini yükseltmek için
```
sudo sh ./omsagent-<version>.universal.x64.sh --upgrade
```

#### <a name="to-install-and-onboard-to-a-workspace-in-us-government-cloud"></a>Yüklemek için ve bir çalışma alanına US Government bulutta yerleşik
```
sudo sh ./omsagent-<version>.universal.x64.sh --upgrade -w <workspace id> -s <shared key> -d opinsights.azure.us
```

## <a name="configuring-the-agent-for-use-with-a-proxy-server-or-oms-gateway"></a>Aracı kullanmak için bir proxy sunucusu veya OMS ağ geçidi yapılandırma
Linux için OMS Aracısı, bir proxy sunucusu veya OMS ağ geçidi HTTPS protokolünü kullanarak OMS hizmet ile iletişim kurmasını destekler.  Anonim ve temel kimlik doğrulaması (kullanıcı adı/parola) desteklenir.  

### <a name="proxy-configuration"></a>Proxy yapılandırması
Proxy yapılandırma değeri sözdizimi aşağıdaki gibidir:

`[protocol://][user:password@]proxyhost[:port]`

Özellik|Açıklama
-|-
Protokol|https
Kullanıcı|Proxy kimlik doğrulaması için isteğe bağlı kullanıcı adı
password|Proxy kimlik doğrulaması için isteğe bağlı parola
proxyhost|Adres veya proxy sunucu/OMS ağ geçidi FQDN'sini
port|Proxy sunucu/OMS ağ geçidi için isteğe bağlı bağlantı noktası numarası

Örneğin, `https://user01:password@proxy01.contoso.com:30443`

Proxy sunucusu yüklemesi sırasında veya yükleme işleminden sonra proxy.conf yapılandırma dosyasını değiştirerek belirtilebilir.   

### <a name="specify-proxy-configuration-during-installation"></a>Yükleme sırasında proxy yapılandırmasını belirtin
`-p` Veya `--proxy` bağımsız değişkeni omsagent yükleme paket için kullanılacak proxy yapılandırması belirtir. 

```
sudo sh ./omsagent-<version>.universal.x64.sh --upgrade -p https://<proxy user>:<proxy password>@<proxy address>:<proxy port> -w <workspace id> -s <shared key>
```

### <a name="define-the-proxy-configuration-in-a-file"></a>Bir dosyada proxy yapılandırmasını tanımlayın
Proxy yapılandırma dosyalarında ayarlanabilir `/etc/opt/microsoft/omsagent/proxy.conf` ve `/etc/opt/microsoft/omsagent/conf/proxy.conf `. Dosyaları doğrudan oluşturduğunuz veya düzenlediğiniz, ancak bunların izinlerini omiuser kullanıcı okuma dosyalarda izni vermek için güncelleştirilmesi gerekir. Örneğin:
```
proxyconf="https://proxyuser:proxypassword@proxyserver01:30443"
sudo echo $proxyconf >>/etc/opt/microsoft/omsagent/proxy.conf
sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/proxy.conf
sudo chmod 600 /etc/opt/microsoft/omsagent/proxy.conf /etc/opt/microsoft/omsagent/conf/proxy.conf  
sudo /opt/microsoft/omsagent/bin/service_control restart [<workspace id>]
```

### <a name="removing-the-proxy-configuration"></a>Proxy yapılandırması kaldırılıyor
Önceden tanımlanmış proxy yapılandırmasını kaldırın ve doğrudan bağlantı geri döndürmek için proxy.conf dosyayı kaldırın:
```
sudo rm /etc/opt/microsoft/omsagent/proxy.conf /etc/opt/microsoft/omsagent/conf/proxy.conf
sudo /opt/microsoft/omsagent/bin/service_control restart 
```

## <a name="onboarding-with-operations-management-suite"></a>Operations Management Suite ile ekleme
Bir çalışma alanı kimliği ve anahtarı paket yükleme sırasında sağlanan değil, aracı sonradan Operations Management Suite ile kayıtlı olması gerekir.

### <a name="onboarding-using-the-command-line"></a>Komut satırını kullanarak ekleme
Çalışma alanınız için anahtarı ve çalışma alanı kimliği sağladığını omsadmin.sh komutunu çalıştırın. Bu komut (sudo yükseltmesi ile) kök olarak çalıştırmanız gerekir:
```
cd /opt/microsoft/omsagent/bin
sudo ./omsadmin.sh -w <WorkspaceID> -s <Shared Key>
```

### <a name="onboarding-using-a-file"></a>Bir dosyayı kullanarak ekleme
1.  Dosyayı oluşturma `/etc/omsagent-onboard.conf`. Dosya okunabilir ve yazılabilir bir kök olmalıdır.
`sudo vi /etc/omsagent-onboard.conf`
2.  Çalışma alanı kimliği ve paylaşılan anahtar ile dosyasında aşağıdaki satırları ekleyin:

        WORKSPACE_ID=<WorkspaceID>  
        SHARED_KEY=<Shared Key>  
   
3.  OMS Onboard için aşağıdaki komutu çalıştırın:`sudo /opt/microsoft/omsagent/bin/omsadmin.sh`
4.  Dosya üzerinde başarıyla Hazırlanmanız silinir.

## <a name="enable-the-oms-agent-for-linux-to-report-to-system-center-operations-manager"></a>System Center Operations Manager için rapor Linux için OMS aracısının etkinleştir
System Center Operations Manager yönetim grubu için rapor Linux için OMS aracısının yapılandırmak için aşağıdaki adımları gerçekleştirin.  

1. Dosyayı düzenleyin.`/etc/opt/omi/conf/omiserver.conf`
2. Satır başlayarak emin **httpsport =** bağlantı noktası 1270 tanımlar. Örneğin:`httpsport=1270`
3. OMI sunucuyu yeniden başlatın:`sudo /opt/omi/bin/service_control restart`

## <a name="agent-logs"></a>Aracı günlükleri
Linux için OMS aracısının günlüklerinde bulunabilir: `/var/opt/microsoft/omsagent/<workspace id>/log/` omsconfig (Aracısı yapılandırması) programı günlüklerinde bulunabilir: `/var/opt/microsoft/omsconfig/log/` (performans ölçüm verilerini sağlayan) OMI ve scx farklı bileşenlerin günlüklerinde bulunabilir:`/var/opt/omi/log/ and /var/opt/microsoft/scx/log`

### <a name="log-rotation-configuration"></a>Günlük döndürme yapılandırma ##
Günlük döndürme yapılandırması omsagent için yolda bulunabilir:`/etc/logrotate.d/omsagent-<workspace id>`

Varsayılan ayarlar şunlardır: 
```
/var/opt/microsoft/omsagent/<workspace id>/log/omsagent.log {
    rotate 5
    missingok
    notifempty
    compress
    size 50k
    copytruncate
}
```

## <a name="uninstalling-the-oms-agent-for-linux"></a>Linux için OMS aracısı kaldırma
Paket .sh dosyasıyla çalıştırarak aracı paketlerini kaldırılabilir `--purge` aracı ve yapılandırmasıyla bilgisayarınızdan tamamen kaldırır bağımsız değişkeni.   

```
> sudo rpm -e omsconfig
> sudo rpm -e omsagent
> sudo /opt/microsoft/scx/bin/uninstall
```

## <a name="troubleshooting"></a>Sorun giderme

### <a name="issue-unable-to-connect-through-proxy-to-oms"></a>Sorun: OMS proxy'si aracılığıyla bağlanılamıyor

#### <a name="probable-causes"></a>Olası nedenler
* Onboarding işlemi sırasında belirtilen proxy yanlış
* OMS hizmet uç noktaları, veri merkezinizdeki whitelistested değildir 

#### <a name="resolutions"></a>Çözümler
1. Reonboard seçeneği ile aşağıdaki komutu kullanarak Linux için OMS Aracısı ile OMS hizmetine `-v` etkin. Bu settubg OMS hizmetine proxy üzerinden bağlanma Aracısı'nın ayrıntılı çıktı sağlar. 
`/opt/microsoft/omsagent/bin/omsadmin.sh -w <OMS Workspace ID> -s <OMS Workspace Key> -p <Proxy Conf> -v`

2. Bölümünü gözden [aracı kullanmak için bir proxy sunucusu veya OMS ağ geçidi yapılandırma](#configuring the-agent-for-use-with-a-proxy-server-or-oms-gateway) düzgün bir şekilde yapılandırdığınız bir proxy sunucu üzerinden iletişim kurmak için aracıyı doğrulanamadı.    
* Aşağıdaki OMS hizmet uç noktalarına Güvenilenler listesine olduğunu kontrol edin:

    |Aracı Kaynağı| Bağlantı Noktaları |  
    |------|---------|  
    |*.ods.opinsights.azure.com | Bağlantı noktası 443|   
    |*.oms.opinsights.azure.com | Bağlantı noktası 443|   
    |ods.systemcenteradvisor.com | Bağlantı noktası 443|   
    |*.BLOB.Core.Windows.NET/ | Bağlantı noktası 443|   

### <a name="issue-you-receive-a-403-error-when-trying-to-onboard"></a>Sorun: Bir 403 hatası için yerleşik çalışırken alıyorsunuz

#### <a name="probable-causes"></a>Olası nedenler
* Tarih ve saat, Linux sunucusu üzerinde yanlış 
* Çalışma alanı kimliği ve çalışma alanı anahtarı doğru değil

#### <a name="resolution"></a>Çözüm

1. Linux sunucunuzla komutu tarih zamanını denetleyin. Saati geçerli saatten 15 dakika +/-ise ekleme başarısız olur. Doğru Bu güncelleştirme için tarih ve/veya saat dilimi Linux sunucunuzun. 
2. Linux için OMS aracısının en son sürümünün yüklü olduğunu doğrulayın.  En yeni sürümü şimdi zaman farkı, onboarding hataya neden olduğunu bildirir.
3. Doğru çalışma alanı kimliği ve çalışma alanı bu konunun önceki kısımlarında yükleme yönergeleri izleyerek anahtarı kullanarak Reonboard.

### <a name="issue-you-see-a-500-and-404-error-in-the-log-file-right-after-onboarding"></a>Sorun: Sağ onboarding sonra 500 ve 404 Hata günlük dosyasına bakın
Bu hata bir OMS çalışma alanına Linux verilerin ilk karşıya yükleme sırasında oluşan bilinen bir sorundur. Bu hata, gönderilen veya servis deneyimi olan verileri etkilemez.

### <a name="issue-you-are-not-seeing-any-data-in-the-oms-portal"></a>Sorun: Herhangi bir veri OMS portalında gördüğünüz değil

#### <a name="probable-causes"></a>Olası nedenler

- OMS hizmetine ekleme başarısız oldu
- OMS hizmetine bağlantı engellendi
- Linux veriler için OMS aracısının yedeklenir

#### <a name="resolutions"></a>Çözümler
1. Aşağıdaki dosya olup olmadığını kontrol ederek ekleme OMS hizmetine başarılı olup olmadığını kontrol edin:`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`
2. Reonboard kullanarak `omsadmin.sh` komut satırı yönergeleri
3. Bir proxy sunucu kullanıyorsanız, daha önce sağlanan proxy çözüm adımlarına bakın.
4. Linux için OMS aracısının OMS hizmetiyle iletişim kuramadığında bazı durumlarda, aracı veri 50 MB'tır tam arabellek boyutu için sıraya alındı. Linux için OMS aracısı aşağıdaki komutu çalıştırarak yeniden başlatılması gerekir: `/opt/microsoft/omsagent/bin/service_control restart [<workspace id>]`. 

    >[!NOTE]
    >Aracı sürümü 1.1.0-28 ve daha sonra bu sorun düzeltilmiştir.

