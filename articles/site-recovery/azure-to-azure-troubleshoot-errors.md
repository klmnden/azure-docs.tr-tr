---
title: Azure Site Recovery Azure Azure çoğaltma sorunlarını hataları için sorun giderme ve | Microsoft Docs
description: Olağanüstü durum kurtarma için Azure sanal makineleri çoğaltırken hatalarını ve sorunlarını giderme
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: sujayt
ms.openlocfilehash: e6f694e601c506adae217e6348b4bf388cc24390
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34211839"
---
# <a name="troubleshoot-azure-to-azure-vm-replication-issues"></a>Azure Azure VM çoğaltma sorunlarını giderme

Bu makalede, Azure Site Recovery çoğaltma ve Azure sanal makineleri başka bir bölgeye bir bölgesinden kurtarma yaygın sorunları açıklar ve gidermenizi açıklanmaktadır. Desteklenen yapılandırmalar hakkında daha fazla bilgi için bkz: [Azure Vm'lerini çoğaltma için destek matrisi](site-recovery-support-matrix-azure-to-azure.md).

## <a name="azure-resource-quota-issues-error-code-150097"></a>Azure kaynak kotası sorunları (hata kodu 150097)
Aboneliğiniz Azure VM'ler için olağanüstü durum kurtarma bölgeniz olarak kullanmayı planlıyorsanız hedef bölgesi oluşturmak için etkinleştirilmiş olmalıdır. Ayrıca, aboneliğinizin etkin VM'ler belirli boyutu oluşturmak için yeterli kotası olması gerekir. Varsayılan olarak, Site Recovery aynı boyutta hedef VM için VM kaynağı olarak seçer. Eşleşen boyutu kullanılabilir değilse, en yakın olası boyutu otomatik olarak çekilir. Kaynak VM yapılandırmayı destekleyen eşleşen boyut ise, bu hata iletisi görüntülenir:

**Hata kodu** | **Olası nedenler** | **Öneri**
--- | --- | ---
150097<br></br>**İleti**: VmName sanal makinesi için çoğaltma etkinleştirilemedi. | -Abonelik Kimliğinizi hedef bölge konumda bulunan herhangi bir VM oluşturmak için etkinleştirilmemiş olabilir.</br></br>-Abonelik Kimliğinizi etkinleştirilmemiş veya belirli VM boyutları hedef bölge konumunda oluşturmak için yeterli kotası yok.</br></br>Kaynak VM NIC sayısı (2) ile eşleşen VM boyutu uygun hedef abonelik kimliği için hedef bölge konumunda bulunan değil.| Kişi [Azure Fatura Desteği](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) aboneliğiniz için hedef konumda gerekli VM boyutları için VM oluşturmayı etkinleştirmek üzere. Bu özellik etkinleştirildikten sonra başarısız olan işlemi yeniden deneyin.

### <a name="fix-the-problem"></a>Sorunu gidermek
Sizinle iletişim [Azure Fatura Desteği](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) gerekli boyutlarda VM'ler hedef konumda oluşturmak için aboneliğinizi etkinleştirmek için.

Hedef konumu bir kapasite kısıtlaması yoksa, çoğaltmayı devre dışı bırakın ve aboneliğinizi gerekli boyutlarda VM'ler oluşturmak için yeterli kotası sahip olduğu farklı bir konuma etkinleştirin.

## <a name="trusted-root-certificates-error-code-151066"></a>Güvenilen kök sertifikaları (hata kodu 151066)

Tüm son güvenilen kök sertifikalar VM mevcut değilse, "çoğaltma etkinleştir" işinizi başarısız olabilir. Sertifikaları olmadan, Site Recovery hizmeti çağrıları VM'den yetkilendirme ve kimlik doğrulama başarısız. Başarısız "çoğaltma etkinleştir" Site kurtarma işi için hata iletisi görüntülenir:

**Hata kodu** | **Olası neden** | **Öneriler**
--- | --- | ---
151066<br></br>**İleti**: Site Recovery yapılandırma başarısız oldu. | Gerekli kök sertifikaları yetkilendirme için kullanılan kimlik doğrulaması olmayan makinede güvenilir ve. | -Windows işletim sistemi çalıştıran bir VM için güvenilen kök sertifikaları makinede mevcut olduğundan emin olun. Bilgi için bkz: [yapılandırma Güvenilen Kökleri ve izin verilmeyen sertifikaları](https://technet.microsoft.com/library/dn265983.aspx).<br></br>-Linux işletim sistemi çalıştıran bir VM için Linux işletim sistemi sürümü dağıtımcı tarafından yayımlanan güvenilen kök sertifikalar için yönergeleri izleyin.

### <a name="fix-the-problem"></a>Sorunu gidermek
**Windows**

Böylece tüm güvenilen kök sertifikalar makinede en son Windows güncelleştirmeleri VM'e yükleyin. Bağlantısı kesilmiş bir ortamda değilseniz, sertifikaları almak için kuruluşunuzdaki standart Windows güncelleştirme işlemini izleyin. Gerekli sertifikaları VM mevcut değilse, Site Recovery hizmeti çağrıları güvenlik nedenleriyle başarısız.

Normal Windows güncelleştirme Yönetimi'ni veya sertifika güncelleştirme yönetimi işlemi Vm'lerinde tüm son kök sertifikaları ve güncelleştirilmiş sertifika iptal listesini almak için kuruluşunuzdaki izleyin.

Sorunun giderilmiş olduğunu doğrulamak için VM'deki bir tarayıcıdan için login.microsoftonline.com gidin.

**Linux**

VM son güvenilen kök sertifikaları ve en son sertifika iptal listesini almak için Linux dağıtımcı tarafından sağlanan yönergeleri izleyin.

SuSE Linux sertifika listesini korumak için simgesel bağlantı kullandığından, aşağıdaki adımları izleyin:

1.  Kök kullanıcı olarak oturum açın.

2.  Dizini değiştirmek için bu komutu çalıştırın.

      ``# cd /etc/ssl/certs``

3. Symantec kök CA sertifikası mevcut olup olmadığını denetleyin.

      ``# ls VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

4. Symantec kök CA sertifika bulunmazsa, dosyayı yüklemek için aşağıdaki komutu çalıştırın. Hatalar için denetleyin ve ağ hataları için önerilen eylemi izleyin.

      ``# wget https://www.symantec.com/content/dam/symantec/docs/other-resources/verisign-class-3-public-primary-certification-authority-g5-en.pem -O VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

5. Baltimore kök CA sertifikası mevcut olup olmadığını denetleyin.

      ``# ls Baltimore_CyberTrust_Root.pem``

6. Baltimore kök CA sertifika bulunmazsa, sertifikayı indirin.  

    ``# wget http://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem -O Baltimore_CyberTrust_Root.pem``

7. DigiCert_Global_Root_CA cert mevcut olup olmadığını denetleyin.

    ``# ls DigiCert_Global_Root_CA.pem``

8. DigiCert_Global_Root_CA bulunmazsa sertifika indirmek için aşağıdaki komutları çalıştırın.

    ``# wget http://www.digicert.com/CACerts/DigiCertGlobalRootCA.crt``

    ``# openssl x509 -in DigiCertGlobalRootCA.crt -inform der -outform pem -out DigiCert_Global_Root_CA.pem``

9. Rehash komut dosyasını bir sertifikayı güncelleştirmek için yeni yüklenen sertifikaların konu karmaları çalıştırın.

    ``# c_rehash``

10. Simgesel bağlantı için sertifikaları oluşturuldukça konu karmaları olmadığını denetleyin.

    - Komut

      ``# ls -l | grep Baltimore``

    - Çıkış

      ``lrwxrwxrwx 1 root root   29 Jan  8 09:48 3ad48a91.0 -> Baltimore_CyberTrust_Root.pem
      -rw-r--r-- 1 root root 1303 Jun  5  2014 Baltimore_CyberTrust_Root.pem``

    - Komut

      ``# ls -l | grep VeriSign_Class_3_Public_Primary_Certification_Authority_G5``

    - Çıkış

      ``-rw-r--r-- 1 root root 1774 Jun  5  2014 VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem
      lrwxrwxrwx 1 root root   62 Jan  8 09:48 facacbc6.0 -> VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

    - Komut

      ``# ls -l | grep DigiCert_Global_Root``

    - Çıkış

      ``lrwxrwxrwx 1 root root   27 Jan  8 09:48 399e7759.0 -> DigiCert_Global_Root_CA.pem
      -rw-r--r-- 1 root root 1380 Jun  5  2014 DigiCert_Global_Root_CA.pem``

11. Filename b204d74a.0 ile VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem dosyasının bir kopyasını oluşturun

    ``# cp VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem b204d74a.0``

12. Filename 653b494a.0 ile Baltimore_CyberTrust_Root.pem dosyasının bir kopyasını oluşturun

    ``# cp Baltimore_CyberTrust_Root.pem 653b494a.0``

13. Filename 3513523f.0 ile DigiCert_Global_Root_CA.pem dosyasının bir kopyasını oluşturun

    ``# cp DigiCert_Global_Root_CA.pem 3513523f.0``  


14. Dosyaları mevcut olup olmadığını denetleyin.  

    - Komut

      ``# ls -l 653b494a.0 b204d74a.0 3513523f.0``

    - Çıkış

      ``-rw-r--r-- 1 root root 1774 Jan  8 09:52 3513523f.0
      -rw-r--r-- 1 root root 1303 Jan  8 09:52 653b494a.0
      -rw-r--r-- 1 root root 1774 Jan  8 09:52 b204d74a.0``


## <a name="outbound-connectivity-for-site-recovery-urls-or-ip-ranges-error-code-151037-or-151072"></a>Site kurtarma URL'lerini veya IP aralıkları (hata kodu 151037 veya 151072) için giden bağlantı

Site Recovery çoğaltma çalışmak için giden bağlantısını belirli URL veya IP aralıkları sanal makineden gerekli. VM Güvenlik duvarı arkasındaysa veya giden bağlantıyı denetlemek için ağ güvenlik grubu (NSG) kuralları kullanır, bu hata iletilerinden birini görebilirsiniz:

**Hata kodları** | **Olası nedenler** | **Öneriler**
--- | --- | ---
151037<br></br>**İleti**: Site Recovery ile Azure sanal makinesi kaydedilemedi. | -NSG giden erişimi denetlemek için kullanmakta olduğunuz VM ve gerekli IP aralıkları giden erişim için Güvenilenler listesine değil.</br></br>-Üçüncü taraf güvenlik duvarı araçları kullanıyorsanız ve gerekli IP aralıklarını/URL'leri listede değilsiniz.</br>| -VM giden ağ bağlantısını denetlemek için güvenlik duvarı proxy kullanıyorsanız, önkoşul URL'leri veya veri merkezi IP aralıkları Güvenilenler listesine olduğundan emin olun. Bilgi için bkz: [güvenlik duvarı proxy Kılavuzu](https://aka.ms/a2a-firewall-proxy-guidance).</br></br>-VM giden ağ bağlantısını denetlemek için NSG kuralları kullanıyorsanız, önkoşul veri merkezi IP aralıkları Güvenilenler listesine olduğundan emin olun. Bilgi için bkz: [ağ güvenlik grubu Kılavuzu](https://aka.ms/a2a-nsg-guidance).
151072<br></br>**İleti**: Site Recovery yapılandırma başarısız oldu. | Site Recovery Hizmeti uç noktaları için bağlantı kurulamıyor. | -VM giden ağ bağlantısını denetlemek için güvenlik duvarı proxy kullanıyorsanız, önkoşul URL'leri veya veri merkezi IP aralıkları Güvenilenler listesine olduğundan emin olun. Bilgi için bkz: [güvenlik duvarı proxy Kılavuzu](https://aka.ms/a2a-firewall-proxy-guidance).</br></br>-VM giden ağ bağlantısını denetlemek için NSG kuralları kullanıyorsanız, önkoşul veri merkezi IP aralıkları Güvenilenler listesine olduğundan emin olun. Bilgi için bkz: [ağ güvenlik grubu Kılavuzu](https://aka.ms/a2a-nsg-guidance).

### <a name="fix-the-problem"></a>Sorunu gidermek
Güvenilir listeye [gerekli URL'leri](azure-to-azure-about-networking.md#outbound-connectivity-for-urls) veya [gerekli IP aralıkları](azure-to-azure-about-networking.md#outbound-connectivity-for-ip-address-ranges), adımları [Ağ Kılavuzu belge](site-recovery-azure-to-azure-networking-guidance.md).

## <a name="disk-not-found-in-the-machine-error-code-150039"></a>Disk makine (hata kodu 150039) bulunamadı

VM'ye bağlı yeni bir disk başlatılması gerekir.

**Hata kodu** | **Olası nedenler** | **Öneriler**
--- | --- | ---
150039<br></br>**İleti**: mantıksal birim numarası (LUN) (LUNValue) ile Azure veri diski (DiskName) (DiskURI) karşılık gelen bir diske aynı LUN değerine sahip VM içinden rapor edilen eşlenmiş. | -Yeni veri diski VM'ye bağlı, ancak başlatılmış değildi.</br></br>-VM içindeki veri diski diski VM'ye iliştirildiği LUN değeri doğru raporlama değil.| Veri diskleri başlatılır ve işlemi yeniden deneyin emin olun.</br></br>Windows: [Attach ve başlatma yeni bir disk](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal).</br></br>Linux: [Linux yeni bir veri diski başlatma](https://docs.microsoft.com/azure/virtual-machines/linux/add-disk).

### <a name="fix-the-problem"></a>Sorunu gidermek
Veri diskleri başlatılmış olması ve işlemi yeniden deneyin emin olun:

- Windows: [Attach ve başlatma yeni bir disk](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal).
- Linux: [Linux içinde yeni bir veri diski Ekle](https://docs.microsoft.com/azure/virtual-machines/linux/add-disk).

Sorun devam ederse, desteğe başvurun.


## <a name="unable-to-see-the-azure-vm-for-selection-in-enable-replication"></a>Azure VM için "çoğaltma etkinleştirme" seçimde Görülemedi

Çoğaltma için etkinleştirmek istediğiniz VM görmüyorsanız, Azure VM'de eski bir Site kurtarma yapılandırması nedeniyle kalmayabilir. Eski yapılandırma aşağıdaki durumlarda Azure VM'de sol:

- Site RECOVERY'yi kullanarak Azure VM için çoğaltmayı etkinleştirmiş ve ardından Site Recovery kasası çoğaltma VM'de açıkça devre dışı bırakma olmadan silinir.
- Site RECOVERY'yi kullanarak Azure VM için çoğaltmayı etkinleştirmiş ve çoğaltma VM'de açıkça devre dışı bırakma olmadan Site Recovery kasası içeren kaynak grubu silindi.

### <a name="fix-the-problem"></a>Sorunu gidermek

Kullanabileceğiniz [kaldırmak eski ASR yapılandırma komut dosyası](https://gallery.technet.microsoft.com/Azure-Recovery-ASR-script-3a93f412) ve eski Site kurtarma yapılandırması Azure VM'de kaldırın. Eski bir yapılandırmaya kaldırdıktan sonra VM görüyor olmalısınız.

## <a name="vms-provisioning-state-is-not-valid-error-code-150019"></a>Sanal makinenin sağlama durumu geçerli değil (hata kodu 150019)

VM çoğaltmayı etkinleştirmek için sağlama durumu olmalıdır **başarılı**. Aşağıdaki adımları izleyerek VM durumunu kontrol edebilirsiniz.

1.  Seçin **kaynak Gezgini** gelen **tüm hizmetleri** Azure portalında.
2.  Genişletme **abonelikleri** listesinde ve aboneliğinizi seçin.
3.  Genişletme **ResourceGroups** listesinde ve VM kaynak grubunu seçin.
4.  Genişletme **kaynakları** listesinde ve sanal makine seçin
5.  Denetleme **provisioningState** sağ taraftaki örneği görünümünde alan.

### <a name="fix-the-problem"></a>Sorunu gidermek

- Varsa **provisioningState** olan **başarısız**, sorun giderme ayrıntılarla desteğine başvurun.
- Varsa **provisioningState** olan **güncelleştirme**, başka bir uzantı dağıtılamıyor. Bunları ve başarısız Site kurtarma işlemini yeniden denemeniz için bekleme VM üzerinde devam eden tüm işlemler olup olmadığını kontrol **çoğaltmasını etkinleştir** işi.


## <a name="comvolume-shadow-copy-service-error-error-code-151025"></a>COM +/ Birim Gölge Kopyası Hizmeti hatası (hata kodu 151025)
**Hata kodu** | **Olası nedenler** | **Öneriler**
--- | --- | ---
151025<br></br>**İleti**: Site recovery uzantısı yüklenemedi | -'COM + Sistem uygulaması' servis devre dışı bırakıldı.</br></br>-'Birim gölge kopyası' hizmeti devre dışı.| Otomatik veya el ile başlatma modu için 'COM + Sistem uygulaması' ve 'Birim gölge kopyası' Hizmetleri ayarlayın.

### <a name="fix-the-problem"></a>Sorunu gidermek

'Hizmetleri' konsolunu açın ve 'COM + Sistem uygulaması' emin olun ve 'Birim gölge kopyası' 'Disabled', 'Başlangıç türünün' ayarlanmamış.
  ![COM hata](./media/azure-to-azure-troubleshoot-errors/com-error.png)

## <a name="next-steps"></a>Sonraki adımlar
[Azure sanal makinelerini çoğaltma](site-recovery-replicate-azure-to-azure.md)
