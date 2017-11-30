---
title: "Azure Site Recovery Azure Azure çoğaltma sorunlarını hataları için sorun giderme ve | Microsoft Docs"
description: "Olağanüstü durum kurtarma için Azure sanal makineleri çoğaltırken hatalarını ve sorunlarını giderme"
services: site-recovery
documentationcenter: 
author: sujayt
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/21/2017
ms.author: sujayt
ms.openlocfilehash: 726c12d3c91a6e4fdc77397a736aaa161f0e830c
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2017
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

2.  Şu komutu çalıştırın:

      ``# cd /etc/ssl/certs``

3.  Symantec kök CA sertifikası mevcut olup olmadığını görmek için şu komutu çalıştırın:

      ``# ls VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

4.  Dosya bulunamazsa, şu komutları çalıştırın:

      ``# wget https://www.symantec.com/content/dam/symantec/docs/other-resources/verisign-class-3-public-primary-certification-authority-g5-en.pem -O VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

      ``# c_rehash``

5.  Bir simgesel b204d74a.0 ile oluşturmak için VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem ->, bu komutu çalıştırın:

      ``# ln -s  VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem b204d74a.0``

6.  Bu komutu aşağıdaki çıkış olup olmadığını denetleyin. Aksi halde, bir simgesel oluşturmanız gerekir:

      ``# ls -l | grep Baltimore
      -rw-r--r-- 1 root root   1303 Apr  7  2016 Baltimore_CyberTrust_Root.pem
      lrwxrwxrwx 1 root root     29 May 30 04:47 3ad48a91.0 -> Baltimore_CyberTrust_Root.pem
      lrwxrwxrwx 1 root root     29 May 30 05:01 653b494a.0 -> Baltimore_CyberTrust_Root.pem``

7. Simgesel 653b494a.0 yoksa, bir simgesel oluşturmak için bu komutu kullanın:

      ``# ln -s Baltimore_CyberTrust_Root.pem 653b494a.0``


## <a name="outbound-connectivity-for-site-recovery-urls-or-ip-ranges-error-code-151037-or-151072"></a>Site kurtarma URL'lerini veya IP aralıkları (hata kodu 151037 veya 151072) için giden bağlantı

Site Recovery çoğaltma çalışmak için giden bağlantısını belirli URL veya IP aralıkları sanal makineden gerekli. VM Güvenlik duvarı arkasındaysa veya giden bağlantıyı denetlemek için ağ güvenlik grubu (NSG) kuralları kullanır, bu hata iletilerinden birini görebilirsiniz:

**Hata kodları** | **Olası nedenler** | **Öneriler**
--- | --- | ---
151037<br></br>**İleti**: Site Recovery ile Azure sanal makinesi kaydedilemedi. | -NSG giden erişimi denetlemek için kullanmakta olduğunuz VM ve gerekli IP aralıkları giden erişim için Güvenilenler listesine değil.</br></br>-Üçüncü taraf güvenlik duvarı araçları kullanıyorsanız ve gerekli IP aralıklarını/URL'leri listede değilsiniz.</br>| -VM giden ağ bağlantısını denetlemek için güvenlik duvarı proxy kullanıyorsanız, önkoşul URL'leri veya veri merkezi IP aralıkları Güvenilenler listesine olduğundan emin olun. Bilgi için bkz: [güvenlik duvarı proxy Kılavuzu](https://aka.ms/a2a-firewall-proxy-guidance).</br></br>-VM giden ağ bağlantısını denetlemek için NSG kuralları kullanıyorsanız, önkoşul veri merkezi IP aralıkları Güvenilenler listesine olduğundan emin olun. Bilgi için bkz: [ağ güvenlik grubu Kılavuzu](https://aka.ms/a2a-nsg-guidance).
151072<br></br>**İleti**: Site Recovery yapılandırma başarısız oldu. | Site Recovery Hizmeti uç noktaları için bağlantı kurulamıyor. | -VM giden ağ bağlantısını denetlemek için güvenlik duvarı proxy kullanıyorsanız, önkoşul URL'leri veya veri merkezi IP aralıkları Güvenilenler listesine olduğundan emin olun. Bilgi için bkz: [güvenlik duvarı proxy Kılavuzu](https://aka.ms/a2a-firewall-proxy-guidance).</br></br>-VM giden ağ bağlantısını denetlemek için NSG kuralları kullanıyorsanız, önkoşul veri merkezi IP aralıkları Güvenilenler listesine olduğundan emin olun. Bilgi için bkz: [ağ güvenlik grubu Kılavuzu](https://aka.ms/a2a-nsg-guidance).

### <a name="fix-the-problem"></a>Sorunu gidermek
Güvenilir listeye [gerekli URL'leri](site-recovery-azure-to-azure-networking-guidance.md#outbound-connectivity-for-azure-site-recovery-urls) veya [gerekli IP aralıkları](site-recovery-azure-to-azure-networking-guidance.md#outbound-connectivity-for-azure-site-recovery-ip-ranges), adımları [Ağ Kılavuzu belge](site-recovery-azure-to-azure-networking-guidance.md).

## <a name="disk-not-found-in-the-machine-error-code-150039"></a>Disk makine (hata kodu 150039) bulunamadı

VM'ye bağlı yeni bir disk başlatılması gerekir.

**Hata kodu** | **Olası nedenler** | **Öneriler**
--- | --- | ---
150039<br></br>**İleti**: mantıksal birim numarası (LUN) (LUNValue) ile Azure veri diski (DiskName) (DiskURI) karşılık gelen bir diske aynı LUN değerine sahip VM içinden rapor edilen eşlenmiş. | -Yeni veri diski VM'ye bağlı, ancak başlatılmış değildi.</br></br>-VM içindeki veri diski diski VM'ye iliştirildiği LUN değeri doğru raporlama değil.| Veri diskleri başlatılır ve işlemi yeniden deneyin emin olun.</br></br>Windows: [Attach ve başlatma yeni bir disk](https://docs.microsoft.com/azure/virtual-machines/windows/attach-disk-portal#option-1-attach-and-initialize-a-new-disk).</br></br>Linux: [Linux yeni bir veri diski başlatma](https://docs.microsoft.com/azure/virtual-machines/linux/classic/attach-disk#initialize-a-new-data-disk-in-linux).

### <a name="fix-the-problem"></a>Sorunu gidermek
Veri diskleri başlatılmış olması ve işlemi yeniden deneyin emin olun:

- Windows: [Attach ve başlatma yeni bir disk](https://docs.microsoft.com/azure/virtual-machines/windows/attach-disk-portal#option-1-attach-and-initialize-a-new-disk).
- Linux: [Linux yeni bir veri diski başlatma](https://docs.microsoft.com/azure/virtual-machines/linux/classic/attach-disk#initialize-a-new-data-disk-in-linux).

Sorun devam ederse, desteğe başvurun.


## <a name="unable-to-see-the-azure-vm-for-selection-in-enable-replication"></a>Azure VM için "çoğaltma etkinleştirme" seçimde Görülemedi

Çoğaltma etkinleştirdiğinizde, Azure VM seçimi için görmüyorsanız, bu Azure VM'de eski Site kurtarma yapılandırması nedeniyle kalmış olabilir. Eski yapılandırma aşağıdaki durumlarda Azure VM'de sol:

- Site RECOVERY'yi kullanarak Azure VM için çoğaltmayı etkinleştirmiş ve ardından Site Recovery kasası çoğaltma VM'de açıkça devre dışı bırakma olmadan silinir.
- Site RECOVERY'yi kullanarak Azure VM için çoğaltmayı etkinleştirmiş ve çoğaltma VM'de açıkça devre dışı bırakma olmadan Site Recovery kasası içeren kaynak grubu silindi.

### <a name="fix-the-problem"></a>Sorunu gidermek

Kullanabileceğiniz [kaldırmak eski ASR yapılandırma komut dosyası](https://gallery.technet.microsoft.com/Azure-Recovery-ASR-script-3a93f412) ve eski Site kurtarma yapılandırması Azure VM'de kaldırın. Eski yapılandırma kaldırılıyor sonra çoğaltma, etkinleştirdiğinizde, VM görmeniz gerekir.


## <a name="next-steps"></a>Sonraki adımlar
[Azure sanal makinelerini çoğaltma](azure-to-azure-quickstart.md)
