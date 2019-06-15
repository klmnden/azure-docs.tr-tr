---
title: STONITH ile (büyük örnekler) Azure üzerinde SAP HANA için ayarlanmış yüksek kullanılabilirlik | Microsoft Docs
description: STONITH kullanarak SUSE içinde kurmak (büyük örnekler) Azure üzerinde SAP HANA için yüksek kullanılabilirlik
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
ms.openlocfilehash: 3ef1656a7e8a66092de3050a8f14c5b38e0e2e6c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62123578"
---
# <a name="high-availability-set-up-in-suse-using-the-stonith"></a>STONITH kullanarak yüksek kullanılabilirlik SUSE ayarlama
Bu belge, yüksek kullanılabilirlik STONITH cihaz kullanarak SUSE işletim sisteminde ayarlamak için ayrıntılı adım adım yönergeler sağlar.

**Sorumluluk reddi:** *Bu kılavuz, Kurulum başarıyla çalıştığını Microsoft HANA büyük örnekleri ortamında test ederek elde edilir. Microsoft Hizmet Yönetimi ekibinin HANA büyük örnekler için işletim sistemini desteklemediğinden, ek sorun giderme veya işletim sistemi katmanda daha fazla açıklama için SUSE ile iletişim kurmanız gerekebilir. Microsoft hizmet yönetim ekibi STONITH cihazı ayarlama ve tam olarak destekler ve STONITH cihaz sorunları için sorun giderme için ilgili olabilir.*
## <a name="overview"></a>Genel Bakış
SUSE Kümelemesi kullanarak yüksek kullanılabilirliği ayarlamadan için aşağıdaki önkoşulları karşılamalıdır.
### <a name="pre-requisites"></a>Ön koşullar
- HANA büyük örnekler sağlanır
- İşletim sistemi kayıtlı
- HANA büyük örnekleri sunucular düzeltme ekleri/paketlerini almak için SMT sunucuya bağlanır
- İşletim sistemi en son düzeltme eklerinin yüklü olması
- NTP (saat sunucusu) ayarlama
- Okumanız ve anlamanız HA Kurulumu SUSE belgelere en son sürümünü

### <a name="setup-details"></a>Kurulum Ayrıntıları
Bu kılavuz, aşağıdaki Kurulum kullanır:
- İşletim Sistemi: SAP için SLES 12 SP1
- HANA büyük örnekleri: 2xS192 (dört yuvaları, 2 TB)
- HANA sürümü: HANA 2.0 SP1
- Sunucu adları: sapprdhdb95 (Düğüm1) ve sapprdhdb96 (Düğüm2)
- STONITH cihaz: STONITH cihaz tabanlı iSCSI
- NTP HANA büyük örneği düğümlerinden biri üzerinde ayarlama

HANA büyük örnekleri HSR ile ayarladığınızda, STONITH ayarlamak için Microsoft Hizmet Yönetimi ekibinin isteyebilir. Zaten HANA büyük örnekleri sağlanmış ve gerek STONITH cihaz için mevcut dikey pencereleri ayarlanmış olan mevcut bir müşterisi iseniz, Microsoft Hizmet Yönetimi ekibinin (SRF) hizmeti isteği formunda aşağıdaki bilgileri vermeniz gerekir. Teknik Hesap Yöneticisi veya HANA büyük örnek eklenmesi için Microsoft Contact aracılığıyla SRF form isteyebilir. Yeni müşteriler STONITH cihaz sağlama sırasında isteyebilir. Giriş sağlama isteği formunda kullanılabilir.

- Sunucu adı ve sunucu IP adresi (örneğin, myhanaserver1, 10.35.0.1)
- Konum (örneğin, ABD Doğu)
- Müşteri adı (örneğin, Microsoft)
- SID - HANA sistem tanımlayıcısı (örneğin, H11)

Microsoft Hizmet Yönetimi ekibi SBD cihaz adı ve IP adresi STONITH Kurulumu yapılandırmak için kullanabileceğiniz iSCSI depolama STONITH cihaz yapılandırıldıktan sonra sağlar. 

Aşağıdaki adımları STONITH kullanarak uçtan uca HA ayarlamak için izlenmesi gerekir:

1.  SBD cihazı tanımla
2.  SBD cihaz Başlat
3.  Küme yapılandırma
4.  Softdog izleme ayarlama
5.  Düğümü kümeye ekleyin.
6.  Kümeyi doğrula
7.  Küme kaynaklarını yapılandırma
8.  Yük devretme işlemini test edin

## <a name="1---identify-the-sbd-device"></a>1.   SBD cihazı tanımla
Bu bölümde, Microsoft Hizmet Yönetim takımının STONITH yapılandırıldıktan sonra Kurulum için SBD cihaz belirleme açıklanmaktadır. **Bu bölüm, yalnızca mevcut müşteri için geçerlidir**. Yeni Müşteriyseniz, Microsoft hizmet yönetim ekibi SBD sağlar, ve cihaz adı, bu bölümü atlayabilirsiniz.

1.1 değiştirme */etc/iscsi/initiatorname.isci* için 
``` 
iqn.1996-04.de.suse:01:<Tenant><Location><SID><NodeNumber> 
```

Microsoft Hizmet Yönetimi bu dizesi sağlayın. Dosyayı değiştirmek **hem** düğümleri, ancak her bir düğümde düğüm numarasını farklıdır.

![initiatorname.PNG](media/HowToHLI/HASetupWithStonith/initiatorname.png)

1.2 değiştirme */etc/iscsi/iscsid.conf*: Ayarlama *node.session.timeo.replacement_timeout=5* ve *node.startup otomatik =* . Dosyayı değiştirmek **hem** düğümleri.

1.3 bulma komutu yürütün, dört oturumları gösterir. Düğümler üzerinde çalıştırın.

```
iscsiadm -m discovery -t st -p <IP address provided by Service Management>:3260
```

![iSCSIadmDiscovery.png](media/HowToHLI/HASetupWithStonith/iSCSIadmDiscovery.png)

1.4 iSCSI cihazında oturum açmak için komutu yürütün, dört oturumları gösterir. Çalıştırın **hem** düğümleri.

```
iscsiadm -m node -l
```
![iSCSIadmLogin.png](media/HowToHLI/HASetupWithStonith/iSCSIadmLogin.png)

1.5 yeniden tarama betiği yürütün: *yeniden tarama SCSI bus.sh*.  Bu betik, sizin için oluşturulan yeni diskler gösterir.  Düğümler üzerinde çalıştırın. Bir LUN numarasını görmelisiniz sıfırdan büyük olan (örneğin: 1, 2 vb..)

```
rescan-scsi-bus.sh
```
![rescanscsibus.png](media/HowToHLI/HASetupWithStonith/rescanscsibus.png)

1.6 için komutu çalıştırmak cihaz adını almak *fdisk – l*. Düğümler üzerinde çalıştırın. Cihazı boyutu ile çekme **178 MiB**.

```
  fdisk –l
```

![fdisk-l.png](media/HowToHLI/HASetupWithStonith/fdisk-l.png)

## <a name="2---initialize-the-sbd-device"></a>2.   SBD cihaz Başlat

2.1 SBD cihaz zapnuta **hem** düğümleri

```
sbd -d <SBD Device Name> create
```
![sbdcreate.png](media/HowToHLI/HASetupWithStonith/sbdcreate.png)

2.2 cihaza yazılan denetleyin. Bunu şirket **hem** düğümleri

```
sbd -d <SBD Device Name> dump
```

## <a name="3---configuring-the-cluster"></a>3.   Küme yapılandırma
Bu bölümde SUSE HA kümesini ayarlamak için adımları açıklanmaktadır.
### <a name="31-package-installation"></a>3.1 paket yükleme
3.1.1 Lütfen ha_sles ve SAPHanaSR doc desenleri yüklü olup olmadığını denetleyin. Yüklenmezse, bunları yükleyin. Yüklemek **hem** düğümleri.
```
zypper in -t pattern ha_sles
zypper in SAPHanaSR SAPHanaSR-doc
```
![zypperpatternha_sles.png](media/HowToHLI/HASetupWithStonith/zypperpatternha_sles.png)
![zypperpatternSAPHANASR-doc.png](media/HowToHLI/HASetupWithStonith/zypperpatternSAPHANASR-doc.png)

### <a name="32-setting-up-the-cluster"></a>3.2 kümesi ayarlama
3.2.1 kullanabilir *ha-kümesi-init* komutunu veya kümeyi oluşturmak için yast2 Sihirbazı'nı kullanın. Bu durumda, yast2 Sihirbazı kullanılır. Bu adımı gerçekleştirmeniz **yalnızca birincil düğümdeki**.

Yast2 izleyin > yüksek kullanılabilirlik > Küme ![yast denetimi center.png](media/HowToHLI/HASetupWithStonith/yast-control-center.png)
![yast hawk install.png](media/HowToHLI/HASetupWithStonith/yast-hawk-install.png)

Tıklayın **iptal** halk2 paket zaten yüklü olduğundan.

![yast hawk continue.png](media/HowToHLI/HASetupWithStonith/yast-hawk-continue.png)

Tıklayın **devam edin**

Beklenen değer (Bu durumda 2) dağıtılan düğüm sayısına = ![yast küme Security.png](media/HowToHLI/HASetupWithStonith/yast-Cluster-Security.png) tıklayın **sonraki**
![yast-kümesi-yapılandırma-csync2.png](media/HowToHLI/HASetupWithStonith/yast-cluster-configure-csync2.png) Ekle düğüm adları ve ardından "önerilen dosyaları ekleme"

Tıklayın "Açma csync2"

"Öncesi Shared anahtarları oluştur" seçeneğini tıklatın açılan gösterir

![yast anahtar file.png](media/HowToHLI/HASetupWithStonith/yast-key-file.png)

**Tamam**’a tıklayın.

Kimlik doğrulama öncesi shared anahtarları ve IP adresleri Csync2 kullanarak gerçekleştirilir. Anahtar dosyası ile csync2 -k /etc/csync2/key_hagroup oluşturulur. Dosya key_hagroup oluşturulduktan sonra kümenin tüm üyelerine el ile kopyalanmalıdır. **Dosya düğümü 1 için Düğüm2 kopyalamak için sağlamak**.

![yast küme conntrackd.png](media/HowToHLI/HASetupWithStonith/yast-cluster-conntrackd.png)

Tıklayın **sonraki**
![yast küme service.png](media/HowToHLI/HASetupWithStonith/yast-cluster-service.png)

Varsayılan seçenek önyükleme kapalıydı, pacemaker önyükleme başlatıldığında şekilde "açık" değiştirin. Kurulum gereksinimlerinize göre seçim yapabilirsiniz.
Tıklayın **sonraki** ve küme yapılandırması tamamlanır.

## <a name="4---setting-up-the-softdog-watchdog"></a>4.   Softdog izleme ayarlama
Bu bölümde (softdog) İzleme yapılandırmasını açıklar.

4.1 aşağıdaki satırı ekleyin */etc/init.d/boot.local* üzerinde **hem** düğümleri.
```
modprobe softdog
```
![modprobe-softdog.png](media/HowToHLI/HASetupWithStonith/modprobe-softdog.png)

4.2 dosyasını güncelleştirme */etc/sysconfig/sbd* üzerinde **hem** şu şekilde düğümleri:
```
SBD_DEVICE="<SBD Device Name>"
```
![sbd-device.png](media/HowToHLI/HASetupWithStonith/sbd-device.png)

4.3 Çekirdek modülü yüklemek **hem** aşağıdaki komutu çalıştırarak düğümleri
```
modprobe softdog
```
![modprobe-softdog-command.png](media/HowToHLI/HASetupWithStonith/modprobe-softdog-command.png)

4.4 denetleyin ve bu softdog çalıştığından emin olun üzerinde şu şekilde **hem** düğümleri:
```
lsmod | grep dog
```
![lsmod-grep-dog.png](media/HowToHLI/HASetupWithStonith/lsmod-grep-dog.png)

4.5 SBD cihaz başlatmak **hem** düğümleri
```
/usr/share/sbd/sbd.sh start
```
![sbd-sh-start.png](media/HowToHLI/HASetupWithStonith/sbd-sh-start.png)

4.6 SBD arka plan programı, üzerinde test **hem** düğümleri. Üzerinde yapılandırdıktan sonra iki girişe bakın **hem** düğümleri
```
sbd -d <SBD Device Name> list
```
![sbd-list.png](media/HowToHLI/HASetupWithStonith/sbd-list.png)

4.7 için test iletisi göndermek **bir** düğümlerinizin
```
sbd  -d <SBD Device Name> message <node2> <message>
```
![sbd-list.png](media/HowToHLI/HASetupWithStonith/sbd-list.png)

4.8 üzerinde **ikinci** düğümü (Düğüm2 ileti durumu kontrol edebilirsiniz)
```
sbd  -d <SBD Device Name> list
```
![sbd listesi message.png](media/HowToHLI/HASetupWithStonith/sbd-list-message.png)

4.9 sbd config benimsemek için dosyayı güncelleştirmek */etc/sysconfig/sbd* şu şekilde. Dosya üzerinde güncelleştirme **hem** düğümleri
```
SBD_DEVICE=" <SBD Device Name>" 
SBD_WATCHDOG="yes" 
SBD_PACEMAKER="yes" 
SBD_STARTMODE="clean" 
SBD_OPTS=""
```
4.10 üzerinde pacemaker hizmeti başlatın. **birincil düğüm** (Düğüm1)
```
systemctl start pacemaker
```
![Başlangıç pacemaker.png](media/HowToHLI/HASetupWithStonith/start-pacemaker.png)

Varsa pacemaker hizmet *başarısız*, başvurmak *Senaryo 5: Pacemaker hizmeti başarısız oluyor*

## <a name="5---joining-the-cluster"></a>5.   Küme birleştirme
Bu bölümde, düğüm kümeye nasıl açıklanmaktadır.

### <a name="51-add-the-node"></a>5.1 düğüm Ekle
Aşağıdaki komutu şurada çalıştırın **Düğüm2** Kümeye katılma Düğüm2 izin vermek için.
```
ha-cluster-join
```
Alırsanız bir *hata* Kümeye katılan sırasında başvuran *Senaryo 6: Düğümü kümeye katılamadı 2*.

## <a name="6---validating-the-cluster"></a>6.   Küme doğrulama

### <a name="61-start-the-cluster-service"></a>6.1 Küme hizmetini başlatın
Denetleyin ve isteğe bağlı olarak küme üzerinde ilk kez başlatmak için **hem** düğümleri.
```
systemctl status pacemaker
systemctl start pacemaker
```
![systemctl-status-pacemaker.png](media/HowToHLI/HASetupWithStonith/systemctl-status-pacemaker.png)
### <a name="62-monitor-the-status"></a>6.2 durumunu izleme
Komutunu çalıştırın *crm_mon* emin olmak için **hem** düğümlerde çevrimiçidir. Çalıştırabilirsiniz **düğümlere** küme
```
crm_mon
```
![CRM mon.png](media/HowToHLI/HASetupWithStonith/crm-mon.png) , ayrıca küme durumunu denetlemek için hawk üzerinde oturum açabildiğinizden *https://\<düğümün IP'sini >: 7630*. Varsayılan kullanıcı hacluster ve linux paroladır. Gerekirse, parola kullanarak değiştirebilirsiniz *parola* komutu.

## <a name="7-configure-cluster-properties-and-resources"></a>7. Küme özelliklerini ve kaynaklarını yapılandırma 
Bu bölümde, küme kaynaklarını yapılandırmak için gereken adımları açıklar.
Aşağıdaki kaynak ayarladıysanız, bu örnekte, kalan (gerekirse) SUSE HA Kılavuzu başvurarak yapılandırılabilir. Yapılandırmada gerçekleştirmek **düğümlerinden** yalnızca. Birincil düğüm üzerinde yapın.

- Küme önyükleme
- STONITH cihaz
- Sanal IP adresi


### <a name="71-cluster-bootstrap-and-more"></a>7.1 küme bootstrap ve daha fazlası
Küme önyükleme ekleyin. Dosyayı oluşturmak ve metin olarak aşağıdakileri ekleyin:
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
Yapılandırma, kümeye ekleyin.
```
crm configure load update crm-bs.txt
```
![CRM Yapılandırma crmbs.png](media/HowToHLI/HASetupWithStonith/crm-configure-crmbs.png)

### <a name="72-stonith-device"></a>7.2 STONITH cihaz
Kaynak STONITH ekleyin. Dosya oluşturun ve aşağıdaki gibi metin ekleyin.
```
# vi crm-sbd.txt
# enter the following to crm-sbd.txt
primitive stonith-sbd stonith:external/sbd \
params pcmk_delay_max="15"
```
Yapılandırma, kümeye ekleyin.
```
crm configure load update crm-sbd.txt
```

### <a name="73-the-virtual-ip-address"></a>7.3 sanal IP adresi
Kaynak sanal IP ekleyin. Dosyayı oluşturmak ve metin aşağıdaki gibi ekleyin.
```
# vi crm-vip.txt
primitive rsc_ip_HA1_HDB10 ocf:heartbeat:IPaddr2 \
operations $id="rsc_ip_HA1_HDB10-operations" \
op monitor interval="10s" timeout="20s" \
params ip="10.35.0.197"
```
Yapılandırma, kümeye ekleyin.
```
crm configure load update crm-vip.txt
```

### <a name="74-validate-the-resources"></a>7.4 Kaynakları Doğrula

Komutu çalıştırdığınızda *crm_mon*, iki kaynak görebilirsiniz.
![crm_mon_command.png](media/HowToHLI/HASetupWithStonith/crm_mon_command.png)

Ayrıca, durum gördüğünüz *https://\<düğüm IP adresi >: 7630/cib/Canlı/durumu*

![hawlk-status-page.png](media/HowToHLI/HASetupWithStonith/hawlk-status-page.png)

## <a name="8-testing-the-failover-process"></a>8. Yük devretme işlemini test etme
Yük devretme işlemini test etmek için Düğüm1 üzerinde pacemaker hizmet ve kaynakları yük Düğüm2 durdurun.
```
Service pacemaker stop
```
Şimdi, üzerinde pacemaker Hizmeti Durdur **Düğüm2** ve yük devretme kaynakları **Düğüm1**

**Yük devretmeden önce**
![failover.png önce](media/HowToHLI/HASetupWithStonith/Before-failover.png)
**yük devretme sonrasında**
![failover.png sonra](media/HowToHLI/HASetupWithStonith/after-failover.png)
 ![ CRM Pzt sonra failover.png](media/HowToHLI/HASetupWithStonith/crm-mon-after-failover.png)


## <a name="9-troubleshooting"></a>9. Sorun giderme
Bu bölüm, Kurulum sırasında karşılaşılan bazı hata senaryoları açıklar. Mutlaka bu sorunları karşılaşacağınız değil.

### <a name="scenario-1-cluster-node-not-online"></a>Senaryo 1: Küme düğümü çevrimiçi değil
Düğümlere göstermiyor, Küme Yöneticisi'nde çevrimiçi çevrimiçi duruma getirmek için aşağıdaki deneyebilirsiniz.

İSCSI Hizmeti
```
service iscsid start
```

Ve artık bu iSCSI düğüme oturum açamaz olmalıdır
```
iscsiadm -m node -l
```
Aşağıdaki gibi beklenen çıktıyı görünüyor
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
Yast2 grafik ekran, bu belgedeki yüksek kullanılabilirlik kümesini ayarlamak için kullanılır. Yast2 almayan gösterildiği gibi grafik penceresi açın ve throw Qt hata olarak aşağıdaki adımları gerçekleştirin. Grafik penceresiyle açılır adımları atlayabilirsiniz.

**Hata:**

![yast2-qt-gui-error.png](media/HowToHLI/HASetupWithStonith/yast2-qt-gui-error.png)

**Beklenen çıktı**

![yast denetimi center.png](media/HowToHLI/HASetupWithStonith/yast-control-center.png)

Yast2 ile grafik görünümü açık değilse, aşağıdaki adımları izleyin.

Gereken paketleri yükleyin. "Kök" kullanıcısı olarak oturum açmanız gerekir ve paketleri yükleme/yükleme şekilde ayarlamanız SMT sahip.

Paketleri yüklemek için yast kullanmak > yazılım > Yazılım Yönetimi > bağımlılıkları > "yükleme paketlerini... önerilen" seçeneği. Aşağıdaki ekran görüntüsünde, beklenen ekranlar gösterilmektedir.
>[!NOTE]
>Her iki düğümü yast2 grafik görünümü erişebilmesi için her iki düğümde adımları gerekir.

![yast-sofwaremanagement.png](media/HowToHLI/HASetupWithStonith/yast-sofwaremanagement.png)

Bağımlılıkları altında "Önerilen paketlerini yükleyin" seçin ![yast dependencies.png](media/HowToHLI/HASetupWithStonith/yast-dependencies.png)

Değişiklikleri gözden geçirin ve Tamam'a tıklayın

![yast](media/HowToHLI/HASetupWithStonith/yast-automatic-changes.png)

Paket yükleme işlemi devam eder ![yast gerçekleştirme installation.png](media/HowToHLI/HASetupWithStonith/yast-performing-installation.png)

İleri’ye tıklayın

![yast yükleme report.png](media/HowToHLI/HASetupWithStonith/yast-installation-report.png)

Son'a tıklayın

Ayrıca libqt4 ve libyui qt paketleri yüklemeniz gerekir.
```
zypper -n install libqt4
```
![zypper-install-libqt4.png](media/HowToHLI/HASetupWithStonith/zypper-install-libqt4.png)
```
zypper -n install libyui-qt
```
![zypper yükleme ligyui.png](media/HowToHLI/HASetupWithStonith/zypper-install-ligyui.png)
![zypper yükleme ligyui_part2.png](media/HowToHLI/HASetupWithStonith/zypper-install-ligyui_part2.png) Yast2 artık gösterildiği bir grafik görünümü burada açabilirsiniz.
![yast2 denetimi center.png](media/HowToHLI/HASetupWithStonith/yast2-control-center.png)

### <a name="scenario-3-yast2-does-not-high-availability-option"></a>Senaryo 3: yast2 yüksek kullanılabilirlik seçeneği yok.
Yüksek kullanılabilirlik seçeneği yast2 denetim Merkezi'nde görünür olmasını ek paketleri yüklemeniz gerekir.

Yast2 kullanarak > yazılım > Yazılım Yönetimi > aşağıdaki düzenleri seçin

- Temel SAP HANA sunucusu
- C/C++ Derleyici ve araçları
- Yüksek kullanılabilirlik
- Temel SAP uygulama sunucusu

Aşağıdaki ekran düzenleri yüklemek için adımları gösterir.

Yast2 kullanarak > yazılım > Yazılım Yönetimi

![yast2 denetimi center.png](media/HowToHLI/HASetupWithStonith/yast2-control-center.png)

Düzenleri seçin

![yast pattern1.png](media/HowToHLI/HASetupWithStonith/yast-pattern1.png)
![yast pattern2.png](media/HowToHLI/HASetupWithStonith/yast-pattern2.png)

Tıklayın **kabul et**

![yast değiştirilen packages.png](media/HowToHLI/HASetupWithStonith/yast-changed-packages.png)

Tıklayın **devam edin**

![yast2 gerçekleştirme installation.png](media/HowToHLI/HASetupWithStonith/yast2-performing-installation.png)

Tıklayın **sonraki** yüklemesi tamamlandığında

![yast2 yükleme report.png](media/HowToHLI/HASetupWithStonith/yast2-installation-report.png)

### <a name="scenario-4-hana-installation-fails-with-gcc-assemblies-error"></a>Senaryo 4: HANA yüklemesi gcc derlemeleri hatasıyla başarısız oluyor
HANA yüklemesi şu hata ile başarısız olur.

![Hana-installation-error.png](media/HowToHLI/HASetupWithStonith/Hana-installation-error.png)

Bu sorunu düzeltmek için kitaplıklarını yüklemeniz gerekir (libgcc_sl ve libstdc ++ 6) şu şekilde.

![zypper yükleme lib.png](media/HowToHLI/HASetupWithStonith/zypper-install-lib.png)

### <a name="scenario-5-pacemaker-service-fails"></a>Senaryo 5: Pacemaker hizmeti başarısız oluyor

Aşağıdaki sorun pacemaker hizmet başlatma sırasında oluştu.

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
-- Support: https://lists.freedesktop.org/mailman/listinfo/systemd-devel
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

Bunu düzeltmek için aşağıdaki satırı dosyadan silin */usr/lib/systemd/system/fstrim.timer*

```
Persistent=true
```

![Persistent.png](media/HowToHLI/HASetupWithStonith/Persistent.png)

### <a name="scenario-6-node-2-unable-to-join-the-cluster"></a>6\. Senaryo: Düğümü kümeye katılamadı 2

Ne zaman varolan Düğüm2 birleştirme küme kullanarak *ha-kümesi-birleştirme* komutu, şu hata oluştu.

```
ERROR: Can’t retrieve SSH keys from <Primary Node>
```

![Ha-kümesi-birleştirme-error.png](media/HowToHLI/HASetupWithStonith/ha-cluster-join-error.png)

Düzeltmek için aşağıdaki düğümler üzerinde çalıştırın

```
ssh-keygen -q -f /root/.ssh/id_rsa -C 'Cluster Internal' -N ''
cat /root/.ssh/id_rsa.pub >> /root/.ssh/authorized_keys
```

![SSH-keygen-Düğüm1. PNG](media/HowToHLI/HASetupWithStonith/ssh-keygen-node1.PNG)

![SSH-keygen-Düğüm2. PNG](media/HowToHLI/HASetupWithStonith/ssh-keygen-node2.PNG)

Kümeye önceki düzeltme sonra Düğüm2 eklenin

![Ha-kümesi-birleştirme-fix.png](media/HowToHLI/HASetupWithStonith/ha-cluster-join-fix.png)

## <a name="10-general-documentation"></a>10. Genel Belgeler
Aşağıdaki makaleler de SUSE HA kurulumu hakkında daha fazla bilgi bulabilirsiniz: 

- [SAP HANA SR performans için iyileştirilmiş senaryosu](https://www.suse.com/docrep/documents/ir8w88iwu7/suse_linux_enterprise_server_for_sap_applications_12_sp1.pdf )
- [Çitlemek depolama tabanlı](https://www.suse.com/documentation/sle_ha/book_sleha/data/sec_ha_storage_protect_fencing.html)
- [Blog Pacemaker kümesi kullanarak SAP HANA - bölüm 1-](https://blogs.sap.com/2017/11/19/be-prepared-for-using-pacemaker-cluster-for-sap-hana-part-1-basics/)
- [Blog Pacemaker kümesi kullanarak SAP HANA - bölüm 2-](https://blogs.sap.com/2017/11/19/be-prepared-for-using-pacemaker-cluster-for-sap-hana-part-2-failure-of-both-nodes/)
