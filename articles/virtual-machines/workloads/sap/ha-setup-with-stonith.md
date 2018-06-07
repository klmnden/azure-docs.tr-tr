---
title: SAP HANA azure'da (büyük örnekler) ile STONITH ayarlanmış yüksek kullanılabilirlik | Microsoft Docs
description: Yüksek kullanılabilirlik SAP HANA azure'da (büyük örnekler) için STONITH kullanarak SUSE içinde oluşturun.
services: virtual-machines-linux
documentationcenter: ''
author: saghorpa
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 11/21/2017
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 344a48ff82bd93bf8dc9924e09399e72b9f88e2f
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34656372"
---
# <a name="high-availability-set-up-in-suse-using-the-stonith"></a>Yüksek kullanılabilirlik STONITH kullanarak SUSE içinde ayarlama
Bu belge, yüksek kullanılabilirlik SUSE işletim sisteminde STONITH cihazı kullanarak ayarlamak için ayrıntılı adım adım yönergeler sağlar.

**VAZGEÇME:** *bu kılavuzu Kurulumu başarılı bir şekilde çalışır Microsoft HANA büyük örnekleri ortamında test ederek türetilir. Microsoft Hizmet Yönetimi ekibi HANA büyük örnekleri için işletim sistemini desteklemediğinden, daha fazla sorun giderme veya işletim sistemi katmandaki açıklama SUSE başvurmanız gerekebilir. Microsoft Hizmet Yönetimi ekibi STONITH aygıtı ayarlama ve tam olarak destekler ve STONITH aygıt sorunları için sorun giderme için söz konusu olabilir.*
## <a name="overview"></a>Genel Bakış
SUSE Kümelemesi kullanarak yüksek kullanılabilirliği ayarlamadan için aşağıdaki önkoşulları karşılamalıdır.
### <a name="pre-requisites"></a>Ön koşullar
- HANA büyük örnekleri sağlanır
- İşletim sistemi kayıtlı
- HANA büyük örnekleri sunucuları düzeltme ekleri/paketlerini almak için SMT sunucuya bağlı
- İşletim sistemi yüklü en son düzeltme eklerinin olması
- NTP (saat sunucusu) ayarlama
- Okuma ve SUSE belgelerine HA Kurulum en son sürümünü anlama

### <a name="setup-details"></a>Kurulum Ayrıntıları
Bu kılavuz aşağıdaki Kurulum kullanır:
- İşletim sistemi: SLES 12 SAP için SP1
- HANA büyük örnekleri: 2xS192 (dört yuva, 2 TB)
- HANA sürümü: HANA 2.0 SP1
- Sunucu adları: sapprdhdb95 (Düğüm1) ve sapprdhdb96 (Düğüm2)
- STONITH aygıt: dayalı iSCSI STONITH aygıtı
- NTP HANA büyük örneği düğümlerin birinde ayarlayın

HANA büyük HSR örnekleriyle ayarladığınızda, STONITH ayarlamak için Microsoft Hizmet Yönetimi ekibi isteyebilir. HANA büyük sağlanan örnekleri ve gerek STONITH aygıt, varolan Kanatlar için ayarlanmış olan varolan bir müşteri zaten varsa, hizmet isteği formunu (SRF) Microsoft Hizmet Yönetimi ekibi için aşağıdaki bilgileri sağlamanız gerekir. Teknik Hesap Yöneticisi veya HANA büyük örneği eklenmesi için Microsoft Contact aracılığıyla SRF form isteyebilir. Yeni müşteriler STONITH cihaz sağlama sırasında isteyebilir. Girdi sağlama isteği formunda kullanılabilir.

- Sunucu adı ve sunucu IP adresi (örneğin, myhanaserver1, 10.35.0.1)
- Konum (örneğin, ABD Doğu)
- Müşteri adı (örneğin, Microsoft)
- SID - HANA sistem tanımlayıcısı (örneğin, H11)

Microsoft Hizmet Yönetimi ekibi SBD aygıt adı ve IP adresi STONITH Kurulum yapılandırmak için kullanabileceğiniz iSCSI depolama STONITH aygıt yapılandırıldıktan sonra sağlar. 

Aşağıdaki adımları STONITH kullanarak uçtan uca HA ayarlamak için izlenmesi gerekir:

1.  SBD cihazı tanımlayın
2.  SBD cihaz başlatılamıyor
3.  Küme yapılandırma
4.  Softdog izleme ayarlama
5.  Düğümü kümeye ekleyin
6.  Küme doğrulama
7.  Küme kaynaklarını yapılandırma
8.  Sınama yük devretme işlemi

## <a name="1---identify-the-sbd-device"></a>1.   SBD cihazı tanımlayın
Bu bölümde, Microsoft Hizmet Yönetimi ekibi STONITH yapılandırdıktan sonra kurulumunuzu SBD aygıt belirleme açıklanmaktadır. **Bu bölüm yalnızca var olan müşteri için geçerli**. Yeni bir müşteri varsa, Microsoft Hizmet Yönetimi ekibi SBD sağlamak, ve aygıt adı, bu bölümü atlayabilirsiniz.

1.1 değiştirme */etc/iscsi/initiatorname.isci* için 
``` 
iqn.1996-04.de.suse:01:<Tenant><Location><SID><NodeNumber> 
```

Microsoft Hizmet Yönetimi bu dize sağlayın. Dosyayı değiştirmek **her ikisi de** düğümleri ancak düğüm sayısı her düğümde farklıdır.

![initiatorname.PNG](media/HowToHLI/HASetupWithStonith/initiatorname.png)

1.2 değiştirme */etc/iscsi/iscsid.conf*: ayarlamak *node.session.timeo.replacement_timeout=5* ve *node.startup otomatik =*. Dosyayı değiştirmek **her ikisi de** düğümleri.

1.3 bulma komutu yürütün, dört oturumları gösterir. Her iki düğüm üzerinde çalıştırın.

```
iscsiadm -m discovery -t st -p <IP address provided by Service Management>:3260
```

![iSCSIadmDiscovery.png](media/HowToHLI/HASetupWithStonith/iSCSIadmDiscovery.png)

1.4 iSCSI cihaza oturum açmak için komutu çalıştırmak, dört oturumları gösterir. Çalıştırın **her ikisi de** düğümleri.

```
iscsiadm -m node -l
```
![iSCSIadmLogin.png](media/HowToHLI/HASetupWithStonith/iSCSIadmLogin.png)

1.5 yeniden tarama komut dosyası yürütme: *yeniden tarama SCSI bus.sh*.  Bu komut dosyası için oluşturduğunuz yeni diskler gösterilmektedir.  Her iki düğüm üzerinde çalıştırın. LUN numarası görmelisiniz sıfırdan büyük (örneğin: 1, 2 vb..)

```
rescan-scsi-bus.sh
```
![rescanscsibus.PNG](media/HowToHLI/HASetupWithStonith/rescanscsibus.png)

1.6 komutu çalıştırmak cihaz adını almak için *fdisk – l*. Her iki düğüm üzerinde çalıştırın. Boyutunu aygıtla çekme **178 MIB**.

```
  fdisk –l
```

![Fdisk l.png](media/HowToHLI/HASetupWithStonith/fdisk-l.png)

## <a name="2---initialize-the-sbd-device"></a>2.   SBD cihaz başlatılamıyor

2.1 üzerinde SBD aygıt başlatma **her ikisi de** düğümleri

```
sbd -d <SBD Device Name> create
```
![sbdcreate.PNG](media/HowToHLI/HASetupWithStonith/sbdcreate.png)

2.2 cihaza yazılmış denetleyin. Bunu üzerinde **her ikisi de** düğümleri

```
sbd -d <SBD Device Name> dump
```

## <a name="3---configuring-the-cluster"></a>3.   Küme yapılandırma
Bu bölümde SUSE HA kümesini ayarlamak için adımları açıklanmaktadır.
### <a name="31-package-installation"></a>3.1 paket yükleme
3.1.1 Lütfen ha_sles ve SAPHanaSR belge desenleri yüklü olup olmadığını denetleyin. Yüklenmemişse, bunları yükleyin. Yüklemeniz **her ikisi de** düğümleri.
```
zypper in -t pattern ha_sles
zypper in SAPHanaSR SAPHanaSR-doc
```
![zypperpatternha_sles.PNG](media/HowToHLI/HASetupWithStonith/zypperpatternha_sles.png)
![zypperpatternSAPHANASR doc.png](media/HowToHLI/HASetupWithStonith/zypperpatternSAPHANASR-doc.png)

### <a name="32-setting-up-the-cluster"></a>3.2 küme ayarlama
3.2.1 kullanabilir *ha-küme-init* komutunu veya kümesini ayarlamak için yast2 Sihirbazı'nı kullanın. Bu durumda, yast2 Sihirbazı kullanılır. Bu adımı gerçekleştirmek **yalnızca birincil düğümdeki**.

Yast2 izleyin > yüksek kullanılabilirlik > Küme ![yast denetim center.png](media/HowToHLI/HASetupWithStonith/yast-control-center.png)
![yast hawk install.png](media/HowToHLI/HASetupWithStonith/yast-hawk-install.png)

Tıklatın **iptal** halk2 paket zaten yüklü olduğundan.

![yast hawk continue.png](media/HowToHLI/HASetupWithStonith/yast-hawk-continue.png)

Tıklatın **devam**

Beklenen değer (Bu durumda 2) dağıtılan düğüm sayısı = ![yast küme Security.png](media/HowToHLI/HASetupWithStonith/yast-Cluster-Security.png) tıklatın **sonraki**
![yast-küme-yapılandırma-csync2.png](media/HowToHLI/HASetupWithStonith/yast-cluster-configure-csync2.png) Ekle düğüm adı ve ardından "önerilen dosyaları Ekle"

Tıklayın "Aç csync2"

"Öncesi Shared anahtarları oluştur" seçeneğini tıklatın açılan gösterir

![yast anahtar file.png](media/HowToHLI/HASetupWithStonith/yast-key-file.png)

**Tamam**’a tıklayın.

Kimlik doğrulaması, IP adresleri ve öncesi shared anahtarları içinde Csync2 kullanılarak gerçekleştirilir. Anahtar dosyası csync2 -k /etc/csync2/key_hagroup ile oluşturulur. Dosya key_hagroup oluşturulduktan sonra kümenin tüm üyeleri için el ile kopyalanmalıdır. **Dosyayı Düğüm2 için 1 düğümden diğerine kopyalamak için olun**.

![yast küme conntrackd.png](media/HowToHLI/HASetupWithStonith/yast-cluster-conntrackd.png)

Tıklatın **sonraki**
![yast küme service.png](media/HowToHLI/HASetupWithStonith/yast-cluster-service.png)

Varsayılan seçenek olarak önyükleme kapalı olduğu, pacemaker önyüklemede başlatıldığında şekilde "açık" değiştirin. Kurulum gereksinimlerinize göre seçim yapabilirsiniz.
Tıklatın **sonraki** ve küme yapılandırması tamamlanır.

## <a name="4---setting-up-the-softdog-watchdog"></a>4.   Softdog izleme ayarlama
Bu bölümde (softdog) İzleme yapılandırmasını açıklar.

4.1 aşağıdaki satırı ekleyin */etc/init.d/boot.local* üzerinde **her ikisi de** düğümleri.
```
modprobe softdog
```
![modprobe softdog.png](media/HowToHLI/HASetupWithStonith/modprobe-softdog.png)

4.2 dosyasını güncelleştirme */etc/sysconfig/sbd* üzerinde **her ikisi de** düğümleri aşağıdaki olarak:
```
SBD_DEVICE="<SBD Device Name>"
```
![sbd device.png](media/HowToHLI/HASetupWithStonith/sbd-device.png)

4.3 Çekirdek modülü yükleme **her ikisi de** aşağıdaki komutu çalıştırarak düğümler
```
modprobe softdog
```
![modprobe softdog command.png](media/HowToHLI/HASetupWithStonith/modprobe-softdog-command.png)

4.4 denetleyin ve bu softdog çalıştığından emin olun şu olarak **her ikisi de** düğümleri:
```
lsmod | grep dog
```
![lsmod grep dog.png](media/HowToHLI/HASetupWithStonith/lsmod-grep-dog.png)

4.5 SBD aygıt başlatmak **her ikisi de** düğümleri
```
/usr/share/sbd/sbd.sh start
```
![sbd-Paylaş-start.png](media/HowToHLI/HASetupWithStonith/sbd-sh-start.png)

4.6 üzerinde SBD arka plan programı test **her ikisi de** düğümleri. Üzerinde yapılandırdıktan sonra iki giriş görürsünüz **her ikisi de** düğümleri
```
sbd -d <SBD Device Name> list
```
![sbd list.png](media/HowToHLI/HASetupWithStonith/sbd-list.png)

4.7 bir sınama iletisi gönderin **bir** düğümlerinizin
```
sbd  -d <SBD Device Name> message <node2> <message>
```
![sbd list.png](media/HowToHLI/HASetupWithStonith/sbd-list.png)

4.8 üzerinde **ikinci** düğümü (ileti durumunu kontrol edebilirsiniz Düğüm2)
```
sbd  -d <SBD Device Name> list
```
![sbd listesi message.png](media/HowToHLI/HASetupWithStonith/sbd-list-message.png)

4.9 sbd config benimsemek için dosyayı güncelleştirmek */etc/sysconfig/sbd* aşağıdaki olarak. Dosya sunucusunda da güncelleştirmeniz **her ikisi de** düğümleri
```
SBD_DEVICE=" <SBD Device Name>" 
SBD_WATCHDOG="yes" 
SBD_PACEMAKER="yes" 
SBD_STARTMODE="clean" 
SBD_OPTS=""
```
4.10 pacemaker hizmeti başlatmak **birincil düğüm** (Düğüm1)
```
systemctl start pacemaker
```
![Start-pacemaker.png](media/HowToHLI/HASetupWithStonith/start-pacemaker.png)

Varsa pacemaker hizmet *başarısız*, başvurmak *Senaryo 5: Pacemaker hizmeti başarısız oluyor*

## <a name="5---joining-the-cluster"></a>5.   Kümeye katılma
Bu bölümde, düğüm kümeye konusunda açıklanmaktadır.

### <a name="51-add-the-node"></a>5.1 düğümü ekleme
Aşağıdaki komutu çalıştırın **Düğüm2** Kümeye katılma Düğüm2 izin vermek için.
```
ha-cluster-join
```
Alırsanız, bir *hata* Kümeye katılma sırasında başvurmak *Senaryo 6: düğüm 2 kümeye katılamadı*.

## <a name="6---validating-the-cluster"></a>6.   Küme doğrulama

### <a name="61-start-the-cluster-service"></a>6.1 Küme hizmetini başlatın
Denetleyin ve isteğe bağlı olarak küme üzerinde ilk kez başlatmak için **her ikisi de** düğümleri.
```
systemctl status pacemaker
systemctl start pacemaker
```
![systemctl durum pacemaker.png](media/HowToHLI/HASetupWithStonith/systemctl-status-pacemaker.png)
### <a name="62-monitor-the-status"></a>6.2 durumunu izleme
Komutu çalıştırın *crm_mon* emin olmak için **her ikisi de** düğümleri çevrimiçidir. Çalıştırabilirsiniz **herhangi bir düğüme** küme
```
crm_mon
```
![CRM mon.png](media/HowToHLI/HASetupWithStonith/crm-mon.png) , ayrıca küme durumunu denetlemek için hawk oturum açabildiğinizden *https://<node IP>: 7630*. Varsayılan kullanıcı hacluster ve linux paroladır. Gerekirse, parola kullanarak değiştirebilirsiniz *parola* komutu.

## <a name="7-configure-cluster-properties-and-resources"></a>7. Küme özelliklerini ve kaynaklarını yapılandırma 
Bu bölümde Küme kaynaklarını yapılandırmak için gereken adımları açıklar.
Bu örnekte, aşağıdaki kaynağı ayarladıysanız rest (gerekirse) SUSE HA Kılavuzu başvurarak yapılandırılabilir. Yapılandırma dosyasında gerçekleştirmek **düğümlerin birinde** yalnızca. Birincil düğüm üzerinde yapın.

- Küme önyükleme
- STONITH aygıt
- Sanal IP adresi


### <a name="71-cluster-bootstrap-and-more"></a>7.1 küme önyükleme ve daha fazlası
Küme önyükleme ekleyin. Dosyayı oluşturma ve metin olarak aşağıdakileri ekleyin:
```
sapprdhdb95:~ # vi crm-bs.txt
# enter the following to crm-bs.txt
property $id="cib-bootstrap-options" \
no-quorum-policy="ignore" \
stonith-enabled="true" \
stonith-action="reboot" \
stonith-timeout="150s"
rsc_defaults $id="rsc-options" \
resource-stickiness="1000" \
migration-threshold="5000"
op_defaults $id="op-options" \
timeout="600"
```
Yapılandırma kümeye ekleyin.
```
crm configure load update crm-bs.txt
```
![CRM Yapılandırma crmbs.png](media/HowToHLI/HASetupWithStonith/crm-configure-crmbs.png)

### <a name="72-stonith-device"></a>7.2 STONITH aygıt
Kaynak STONITH ekleyin. Dosya oluşturun ve aşağıdaki gibi metin ekleyin.
```
# vi crm-sbd.txt
# enter the following to crm-sbd.txt
primitive stonith-sbd stonith:external/sbd \
params pcmk_delay_max="15" \
op monitor interval="15" timeout="15"
```
Yapılandırma kümeye ekleyin.
```
crm configure load update crm-sbd.txt
```

### <a name="73-the-virtual-ip-address"></a>7.3 sanal IP adresi
Kaynak sanal IP ekleyin. Dosyayı oluşturma ve metin aşağıdaki gibi ekleyin.
```
# vi crm-vip.txt
primitive rsc_ip_HA1_HDB10 ocf:heartbeat:IPaddr2 \
operations $id="rsc_ip_HA1_HDB10-operations" \
op monitor interval="10s" timeout="20s" \
params ip="10.35.0.197"
```
Yapılandırma kümeye ekleyin.
```
crm configure load update crm-vip.txt
```

### <a name="74-validate-the-resources"></a>7.4 Kaynakları Doğrula

Komutu çalıştırdığınızda *crm_mon*, iki kaynak görebilirsiniz.
![crm_mon_command.PNG](media/HowToHLI/HASetupWithStonith/crm_mon_command.png)

Ayrıca, adresindeki durumunu görebilirsiniz *https://<node IP address>: 7630/cib/Canlı/durumu*

![hawlk durum page.png](media/HowToHLI/HASetupWithStonith/hawlk-status-page.png)

## <a name="8-testing-the-failover-process"></a>8. Yük devretme işlemini test etme
Yük devretme işlemini test etmek için Düğüm1 pacemaker hizmette ve Düğüm2 kaynakları yük devretmeyi durdurun.
```
Service pacemaker stop
```
Şimdi, üzerinde pacemaker hizmetini durdurun **Düğüm2** ve kaynaklar için devredilir **Düğüm1**

**Yük devretme önce**
![failover.png önce](media/HowToHLI/HASetupWithStonith/Before-failover.png)
**yük devretme sonrasında**
![failover.png sonra](media/HowToHLI/HASetupWithStonith/after-failover.png)
 ![ failover.png sonra CRM mon](media/HowToHLI/HASetupWithStonith/crm-mon-after-failover.png)


## <a name="9-troubleshooting"></a>9. Sorun giderme
Bu bölümde, Kurulum sırasında karşılaşılan birkaç hatası senaryolar açıklanmaktadır. Mutlaka bu sorunları karşılaşabilecekleri değil.

### <a name="scenario-1-cluster-node-not-online"></a>Senaryo 1: Küme düğümü çevrimiçi değil
Herhangi bir düğüme göstermiyor, Küme Yöneticisi'nde çevrimiçi çevrimiçi duruma getirmek için aşağıdaki deneyebilirsiniz.

İSCSI Hizmeti
```
service iscsid start
```

Ve artık bu iSCSI düğüme oturum açamaz olmalıdır
```
iscsiadm -m node -l
```
Aşağıdaki gibi beklenen çıktı görünüyor
```
sapprdhdb45:~ # iscsiadm -m node -l
Logging in to [iface: default, target: iqn.1992-08.com.netapp:hanadc11:1:t020, portal: 10.250.22.11,3260] (multiple)
Logging in to [iface: default, target: iqn.1992-08.com.netapp:hanadc11:1:t020, portal: 10.250.22.12,3260] (multiple)
Logging in to [iface: default, target: iqn.1992-08.com.netapp:hanadc11:1:t020, portal: 10.250.22.22,3260] (multiple)
Logging in to [iface: default, target: iqn.1992-08.com.netapp:hanadc11:1:t020, portal: 10.250.22.21,3260] (multiple)
Login to [iface: default, target: iqn.1992-08.com.netapp:hanadc11:1:t020, portal: 10.250.22.11,3260] successful.
Login to [iface: default, target: iqn.1992-08.com.netapp:hanadc11:1:t020, portal: 10.250.22.12,3260] successful.
Login to [iface: default, target: iqn.1992-08.com.netapp:hanadc11:1:t020, portal: 10.250.22.22,3260] successful.
Login to [iface: default, target: iqn.1992-08.com.netapp:hanadc11:1:t020, portal: 10.250.22.21,3260] successful.
```
### <a name="scenario-2-yast2-does-not-show-graphical-view"></a>Senaryo 2: grafik görünümü yast2 göstermez
Yast2 grafik ekran bu belgeyi yüksek kullanılabilirlik kümesinde ayarlamak için kullanılır. Yast2 değil gösterildiği gibi grafik penceresi açın ve Qt hata throw, aşağıdaki adımları gerçekleştirin. Grafik penceresiyle açarsa, adımları atlayabilirsiniz.

**Hata**

![qt GUI error.png yast2](media/HowToHLI/HASetupWithStonith/yast2-qt-gui-error.png)

**Beklenen çıktı**

![yast denetim center.png](media/HowToHLI/HASetupWithStonith/yast-control-center.png)

Yast2 grafik görünümü ile açılmazsa, aşağıdaki adımları izleyin.

Gereken paketleri yükleyin. "Root" kullanıcı olarak oturum açmanız ve paketleri karşıdan yükleme/kurma şekilde ayarlamanız SMT sahip.

Paketleri yüklemek için yast kullanın > yazılım > yazılım Yönetim > bağımlılıkları > "yükleme paketleri... önerilen" seçeneği. Aşağıdaki ekran görüntüsü, beklenen ekranlar gösterilmektedir.
>[!NOTE]
>Her iki düğümü yast2 grafik görünümü erişebilmesi için her iki düğüm üzerinde adımlar gerçekleştirmeniz gerekir.

![yast sofwaremanagement.png](media/HowToHLI/HASetupWithStonith/yast-sofwaremanagement.png)

Bağımlılıklar altında "Önerilen paketlerini yükleyin" seçin ![yast dependencies.png](media/HowToHLI/HASetupWithStonith/yast-dependencies.png)

Değişiklikleri gözden geçirin ve Tamam'ı tıklatın

![yast](media/HowToHLI/HASetupWithStonith/yast-automatic-changes.png)

Paket yükleme devam eder ![yast gerçekleştirme installation.png](media/HowToHLI/HASetupWithStonith/yast-performing-installation.png)

İleri’ye tıklayın

![yast yükleme report.png](media/HowToHLI/HASetupWithStonith/yast-installation-report.png)

Son'u tıklatın

Ayrıca libqt4 ve libyui qt paketleri yüklemeniz gerekir.
```
zypper -n install libqt4
```
![zypper yükleme libqt4.png](media/HowToHLI/HASetupWithStonith/zypper-install-libqt4.png)
```
zypper -n install libyui-qt
```
![zypper yükleme ligyui.png](media/HowToHLI/HASetupWithStonith/zypper-install-ligyui.png)
![zypper yükleme ligyui_part2.png](media/HowToHLI/HASetupWithStonith/zypper-install-ligyui_part2.png) Yast2 grafik görünümü şimdi olarak gösterilen burada açabilirsiniz.
![yast2 denetim center.png](media/HowToHLI/HASetupWithStonith/yast2-control-center.png)

### <a name="scenario-3-yast2-does-not-high-availability-option"></a>Senaryo 3: yast2 yüksek kullanılabilirlik seçeneği değil.
Yüksek kullanılabilirlik seçeneği yast2 denetim Merkezi'nde görünür olması için ek paketleri yüklemeniz gerekir.

Yast2 kullanarak > yazılım > yazılım Yönetim > aşağıdaki desenleri seçin

- SAP HANA server temel
- C/C++ derleyicisi ve araçları
- Yüksek kullanılabilirlik
- SAP uygulama sunucusu temel

Aşağıdaki ekran desenleri yüklemek için adımları gösterir.

Yast2 kullanarak > yazılım > Yazılım Yönetimi

![yast2 denetim center.png](media/HowToHLI/HASetupWithStonith/yast2-control-center.png)

Desenler seçin

![yast pattern1.png](media/HowToHLI/HASetupWithStonith/yast-pattern1.png)
![yast pattern2.png](media/HowToHLI/HASetupWithStonith/yast-pattern2.png)

Tıklatın **kabul et**

![yast değiştirilmiş packages.png](media/HowToHLI/HASetupWithStonith/yast-changed-packages.png)

Tıklatın **devam**

![yast2 gerçekleştirme installation.png](media/HowToHLI/HASetupWithStonith/yast2-performing-installation.png)

Tıklatın **sonraki** yüklemesi tamamlandığında

![yast2 yükleme report.png](media/HowToHLI/HASetupWithStonith/yast2-installation-report.png)

### <a name="scenario-4-hana-installation-fails-with-gcc-assemblies-error"></a>Senaryo 4: HANA yükleme gcc derlemeler hatasıyla başarısız olur.
HANA yüklemesi şu hata ile başarısız olur.

![Hana yükleme error.png](media/HowToHLI/HASetupWithStonith/Hana-installation-error.png)

Sorunu gidermek için kitaplıkları yüklemeniz gerekir (libgcc_sl ve libstdc ++ 6) aşağıdaki olarak.

![zypper yükleme lib.png](media/HowToHLI/HASetupWithStonith/zypper-install-lib.png)

### <a name="scenario-5-pacemaker-service-fails"></a>Senaryo 5: Pacemaker hizmeti başarısız oluyor

Pacemaker hizmet başlangıç sırasında aşağıdaki sorun oluştu.

```
sapprdhdb95:/ # systemctl start pacemaker
A dependency job for pacemaker.service failed. See 'journalctl -xn' for details.
```
```
sapprdhdb95:/ # journalctl -xn
-- Logs begin at Thu 2017-09-28 09:28:14 EDT, end at Thu 2017-09-28 21:48:27 EDT. --
Sep 28 21:48:27 sapprdhdb95 corosync[68812]: [SERV  ] Service engine unloaded: corosync configuration map
Sep 28 21:48:27 sapprdhdb95 corosync[68812]: [QB    ] withdrawing server sockets
Sep 28 21:48:27 sapprdhdb95 corosync[68812]: [SERV  ] Service engine unloaded: corosync configuration ser
Sep 28 21:48:27 sapprdhdb95 corosync[68812]: [QB    ] withdrawing server sockets
Sep 28 21:48:27 sapprdhdb95 corosync[68812]: [SERV  ] Service engine unloaded: corosync cluster closed pr
Sep 28 21:48:27 sapprdhdb95 corosync[68812]: [QB    ] withdrawing server sockets
Sep 28 21:48:27 sapprdhdb95 corosync[68812]: [SERV  ] Service engine unloaded: corosync cluster quorum se
Sep 28 21:48:27 sapprdhdb95 corosync[68812]: [SERV  ] Service engine unloaded: corosync profile loading s
Sep 28 21:48:27 sapprdhdb95 corosync[68812]: [MAIN  ] Corosync Cluster Engine exiting normally
Sep 28 21:48:27 sapprdhdb95 systemd[1]: Dependency failed for Pacemaker High Availability Cluster Manager
-- Subject: Unit pacemaker.service has failed
-- Defined-By: systemd
-- Support: http://lists.freedesktop.org/mailman/listinfo/systemd-devel
--
-- Unit pacemaker.service has failed.
--
-- The result is dependency.
```
```
sapprdhdb95:/ # tail -f /var/log/messages
2017-09-28T18:44:29.675814-04:00 sapprdhdb95 corosync[57600]:   [QB    ] withdrawing server sockets
2017-09-28T18:44:29.676023-04:00 sapprdhdb95 corosync[57600]:   [SERV  ] Service engine unloaded: corosync cluster closed process group service v1.01
2017-09-28T18:44:29.725885-04:00 sapprdhdb95 corosync[57600]:   [QB    ] withdrawing server sockets
2017-09-28T18:44:29.726069-04:00 sapprdhdb95 corosync[57600]:   [SERV  ] Service engine unloaded: corosync cluster quorum service v0.1
2017-09-28T18:44:29.726164-04:00 sapprdhdb95 corosync[57600]:   [SERV  ] Service engine unloaded: corosync profile loading service
2017-09-28T18:44:29.776349-04:00 sapprdhdb95 corosync[57600]:   [MAIN  ] Corosync Cluster Engine exiting normally
2017-09-28T18:44:29.778177-04:00 sapprdhdb95 systemd[1]: Dependency failed for Pacemaker High Availability Cluster Manager.
2017-09-28T18:44:40.141030-04:00 sapprdhdb95 systemd[1]: [/usr/lib/systemd/system/fstrim.timer:8] Unknown lvalue 'Persistent' in section 'Timer'
2017-09-28T18:45:01.275038-04:00 sapprdhdb95 cron[57995]: pam_unix(crond:session): session opened for user root by (uid=0)
2017-09-28T18:45:01.308066-04:00 sapprdhdb95 CRON[57995]: pam_unix(crond:session): session closed for user root
```

Bunu düzeltmek için aşağıdaki satırı dosyadan silmek */usr/lib/systemd/system/fstrim.timer*

```
Persistent=true
```

![Persistent.PNG](media/HowToHLI/HASetupWithStonith/Persistent.png)

### <a name="scenario-6-node-2-unable-to-join-the-cluster"></a>6. Senaryo: Düğüm 2 kümeye katılamadı

Ne zaman varolan Düğüm2 birleştirme küme kullanarak *ha küme-birleşim* komutu, şu hata oluştu.

```
ERROR: Can’t retrieve SSH keys from <Primary Node>
```

![Ha-küme-birleştirme-error.png](media/HowToHLI/HASetupWithStonith/ha-cluster-join-error.png)

Düzeltmek için aşağıdaki düğümler üzerinde çalıştırın

```
ssh-keygen -q -f /root/.ssh/id_rsa -C 'Cluster Internal' -N ''
cat /root/.ssh/id_rsa.pub >> /root/.ssh/authorized_keys
```

![SSH-keygen-Düğüm1. PNG](media/HowToHLI/HASetupWithStonith/ssh-keygen-node1.PNG)

![SSH-keygen-Düğüm2. PNG](media/HowToHLI/HASetupWithStonith/ssh-keygen-node2.PNG)

Önceki düzeltme sonra Düğüm2 kümesine

![Ha-küme-birleştirme-fix.png](media/HowToHLI/HASetupWithStonith/ha-cluster-join-fix.png)

## <a name="10-general-documentation"></a>10. Genel Belgeler
Aşağıdaki makalelerde SUSE HA kurulumu hakkında daha fazla bilgi bulabilirsiniz: 

- [Senaryo SAP HANA SR performansı en iyi duruma getirilmiş](https://www.suse.com/docrep/documents/ir8w88iwu7/suse_linux_enterprise_server_for_sap_applications_12_sp1.pdf )
- [Depolama tabanlı yalıtma](https://www.suse.com/documentation/sle-ha-2/book_sleha/data/sec_ha_storage_protect_fencing.html)
- [SAP HANA - bölüm 1 Pacemaker küme kullanarak Blog-](https://blogs.sap.com/2017/11/19/be-prepared-for-using-pacemaker-cluster-for-sap-hana-part-1-basics/)
- [SAP HANA - bölüm 2 Pacemaker küme kullanarak Blog-](https://blogs.sap.com/2017/11/19/be-prepared-for-using-pacemaker-cluster-for-sap-hana-part-2-failure-of-both-nodes/)