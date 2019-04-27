---
title: Azure Site Recovery Azure'dan Azure'a çoğaltma sorunlarını ve hataları için sorun giderme | Microsoft Docs
description: Olağanüstü durum kurtarma için Azure sanal makineleri çoğaltırken hatalarını ve sorunlarını giderme
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 04/08/2019
ms.author: sujayt
ms.openlocfilehash: c7c91a2cf9a25d0a5a4aeed6621e89f9c7cc18f0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60789917"
---
# <a name="troubleshoot-azure-to-azure-vm-replication-issues"></a>Azure'dan Azure'a VM çoğaltmayla sorunları giderme

Bu makalede Azure Site Recovery çoğaltma ve Azure sanal makineleri bir bölgesinden başka bir bölgeye kurtarma giren yaygın sorunların ve bunları nasıl giderebileceğinizden açıklar. Desteklenen yapılandırmalar hakkında daha fazla bilgi için bkz. [Azure Vm'lerini çoğaltma için destek matrisi](site-recovery-support-matrix-azure-to-azure.md).

## <a name="list-of-errors"></a>Hata listesi
- **[Azure kaynak kotası sorunları (hata kodu 150097)](#azure-resource-quota-issues-error-code-150097)**
- **[Güvenilen kök sertifika (hata kodu 151066)](#trusted-root-certificates-error-code-151066)**
- **[Site Recovery (hata kodu 151195) için giden bağlantı](#issue-1-failed-to-register-azure-virtual-machine-with-site-recovery-151195-br)**

## <a name="azure-resource-quota-issues-error-code-150097"></a>Azure kaynak kotası sorunları (hata kodu 150097)
Azure Vm'lerinde olağanüstü durum kurtarma bölgeniz olarak kullanmayı planlıyorsanız hedef bölgede oluşturmak için aboneliğinizi yeniden etkinleştirilmesi gerekir. Ayrıca, aboneliğinizi yeterli kotası belirli boyut sanal makineler oluşturmak için etkinleştirilmiş olması gerekir. Varsayılan olarak, Site Recovery, hedef sanal makine için aynı boyutta VM kaynağı olarak seçer. Eşleşen boyutu kullanılabilir değilse, en yakın olası boyutu otomatik olarak seçilir. Kaynak VM yapılandırması destekleyen hiçbir eşleşen boyutu varsa, bu hata iletisi görüntülenir:

**Hata kodu** | **Olası nedenler** | **Öneri**
--- | --- | ---
150097<br></br>**İleti**: VmName sanal makinesi için çoğaltma etkinleştirilemedi. | -Abonelik Kimliğinizi hedef bölge konumda herhangi bir VM oluşturmak için etkinleştirilmemiş olabilir.</br></br>-Abonelik Kimliğinizi etkinleştirilmemiş veya hedef bölge konumunda belirli VM boyutları oluşturmak için yeterli kotası yok.</br></br>-Bir uygun bir hedef kaynak VM NIC sayısı (2) eşleşen bir VM boyutu için abonelik Kimliğini hedef bölge konumunda bulunan değil.| İlgili kişi [Azure fatura desteğine](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) gereken hedef konum aboneliğinizde VM boyutları için VM oluşturmayı etkinleştirmek için. Etkinleştirildikten sonra başarısız olan işlemi yeniden deneyin.

### <a name="fix-the-problem"></a>Sorunu
Sizinle iletişim [Azure fatura desteğine](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) hedef konumda gerekli boyutlardaki Vm'leri oluşturmak aboneliğinizi etkinleştirmek için.

Hedef konumu bir kapasite kısıtlaması varsa, çoğaltmayı devre dışı bırakın ve gerekli boyutlardaki Vm'leri oluşturmak için yeterli kotası aboneliğinizin bulunduğu farklı bir konuma etkinleştirin.

## <a name="trusted-root-certificates-error-code-151066"></a>Güvenilen kök sertifika (hata kodu 151066)

Sanal makinede mevcut en yeni güvenilen kök sertifikalar mevcut değilse, "çoğaltmayı etkinleştir" işi başarısız olabilir. Sertifikaları olmadan, Site Recovery hizmeti çağrıları VM'den yetkilendirme ve kimlik doğrulaması başarısız. Başarısız "çoğaltmayı etkinleştir" Site kurtarma işi için hata iletisi görüntülenir:

**Hata kodu** | **Olası nedeni** | **Öneriler**
--- | --- | ---
151066<br></br>**İleti**: Site Recovery yapılandırması başarısız oldu. | İstenen makinede mevcut olmayan kimlik doğrulaması ve yetkilendirme için kök sertifikalarını kullanılan güvenilir. | -Windows işletim sistemi çalıştıran bir VM için güvenilen kök sertifikalar makinede mevcut olduğundan emin olun. Bilgi için [yapılandırma Güvenilen Kökleri ve izin verilmeyen sertifikaları](https://technet.microsoft.com/library/dn265983.aspx).<br></br>-Linux işletim sistemini çalıştıran bir VM için Linux işletim sistemi sürümü dağıtıcı tarafından yayımlanan güvenilen kök sertifikalar için yönergeleri izleyin.

### <a name="fix-the-problem"></a>Sorunu
**Windows**

Tüm güvenilen kök sertifikalar makinede mevcut olacak şekilde sanal makinede en son Windows güncelleştirmelerini yükleyin. Bağlantısı kesilmiş bir ortamda kullanıyorsanız, kuruluşunuz sertifikaları almak için standart Windows güncelleştirme işlemi izleyin. Gerekli sertifikaları sanal makinede mevcut değilse, Site Recovery hizmetine çağrı güvenlikle ilgili nedenlerle başarısız.

Tipik Windows güncelleştirme yönetimi veya sertifika güncelleştirme yönetimi işlemi sanal makinelere en son kök sertifikalar ve güncelleştirilmiş sertifika iptal listesini almak için kuruluşunuzdaki izleyin.

Sorunun giderilmiş olduğunu doğrulamak için vm'nizde bir tarayıcıdan login.microsoftonline.com için gidin.

**Linux**

Sanal makinede en son güvenilir kök sertifikaları ve en son sertifika iptal listesini almak için Linux dağıtıcınız tarafından sağlanan yönergeleri izleyin.

SuSE Linux sertifika listesini korumak için çözümlemeyin kullandığından, aşağıdaki adımları izleyin:

1.  Kök kullanıcı olarak oturum açın.

2.  Dizini değiştirmek için bu komutu çalıştırın.

      ``# cd /etc/ssl/certs``

1. Symantec kök CA sertifikası mevcut olup olmadığını denetleyin.

      ``# ls VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

2. Symantec kök CA sertifika bulunmazsa, dosyayı indirmek için aşağıdaki komutu çalıştırın. Hatalar için denetleyin ve ağ hataları için önerilen eylemi izleyin.

      ``# wget https://www.symantec.com/content/dam/symantec/docs/other-resources/verisign-class-3-public-primary-certification-authority-g5-en.pem -O VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

3. Baltimore kök CA sertifikası mevcut olup olmadığını denetleyin.

      ``# ls Baltimore_CyberTrust_Root.pem``

4. Baltimore kök CA sertifika bulunmazsa sertifikayı indirin.  

    ``# wget https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem -O Baltimore_CyberTrust_Root.pem``

5. DigiCert_Global_Root_CA cert var olup olmadığını denetleyin.

    ``# ls DigiCert_Global_Root_CA.pem``

6. DigiCert_Global_Root_CA bulunmazsa sertifikayı indirmek için aşağıdaki komutları çalıştırın.

    ``# wget http://www.digicert.com/CACerts/DigiCertGlobalRootCA.crt``

    ``# openssl x509 -in DigiCertGlobalRootCA.crt -inform der -outform pem -out DigiCert_Global_Root_CA.pem``

7. Rehash betiği, bir sertifikayı güncelleştirmek için yeni indirilen sertifika için konu karmaları çalıştırın.

    ``# c_rehash``

8.  Çözümlemeyin sertifikalarını oluşturuldukça konu karıştırır, kontrol edin.

    - Komut

      ``# ls -l | grep Baltimore``

    - Çıktı

      ``lrwxrwxrwx 1 root root   29 Jan  8 09:48 3ad48a91.0 -> Baltimore_CyberTrust_Root.pem
      -rw-r--r-- 1 root root 1303 Jun  5  2014 Baltimore_CyberTrust_Root.pem``

    - Komut

      ``# ls -l | grep VeriSign_Class_3_Public_Primary_Certification_Authority_G5``

    - Çıktı

      ``-rw-r--r-- 1 root root 1774 Jun  5  2014 VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem
      lrwxrwxrwx 1 root root   62 Jan  8 09:48 facacbc6.0 -> VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

    - Komut

      ``# ls -l | grep DigiCert_Global_Root``

    - Çıktı

      ``lrwxrwxrwx 1 root root   27 Jan  8 09:48 399e7759.0 -> DigiCert_Global_Root_CA.pem
      -rw-r--r-- 1 root root 1380 Jun  5  2014 DigiCert_Global_Root_CA.pem``

9.  Filename b204d74a.0 ile VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem dosyanın bir kopyasını oluşturma

    ``# cp VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem b204d74a.0``

10. Filename 653b494a.0 ile Baltimore_CyberTrust_Root.pem dosyanın bir kopyasını oluşturma

    ``# cp Baltimore_CyberTrust_Root.pem 653b494a.0``

13. Filename 3513523f.0 ile DigiCert_Global_Root_CA.pem dosyanın bir kopyasını oluşturma

    ``# cp DigiCert_Global_Root_CA.pem 3513523f.0``  


14. Dosyaları mevcut olup olmadığını denetleyin.  

    - Komut

      ``# ls -l 653b494a.0 b204d74a.0 3513523f.0``

    - Çıktı

      ``-rw-r--r-- 1 root root 1774 Jan  8 09:52 3513523f.0
      -rw-r--r-- 1 root root 1303 Jan  8 09:52 653b494a.0
      -rw-r--r-- 1 root root 1774 Jan  8 09:52 b204d74a.0``


## <a name="outbound-connectivity-for-site-recovery-urls-or-ip-ranges-error-code-151037-or-151072"></a>Site Recovery hizmeti URL'lerine veya IP aralıkları (hata kodu 151037 veya 151072) için giden bağlantı

Site Recovery çoğaltması için iş, giden bağlantı için özel URL veya IP aralıkları VM'den gerekli. Sanal makinenize bir güvenlik duvarının arkasındaysa ya da giden bağlantıyı denetlemek için ağ güvenlik grubu (NSG) kuralları kullanıyorsa bu sorunlardan biri karşılaşıyor.

### <a name="issue-1-failed-to-register-azure-virtual-machine-with-site-recovery-151195-br"></a>1 sorunu: Azure sanal makinesi (151195) Site Recovery ile kayıt olamadı </br>
- **Olası nedeni** </br>
  - Site recovery uç noktalarına DNS çözümleme hatası nedeniyle bağlantı kurulamıyor.
  - Bu daha sık yeniden koruma sırasında sanal makine üzerinde başarısız oldu, ancak DR bölgesindeki DNS sunucusu erişilebilir değil görülür.

- **Çözümleme**
   - Özel DNS kullanıyorsanız sonra emin olun, DNS sunucusu olağanüstü durum kurtarma bölgeden erişilebilir. Sanal Makineye gidin özel bir DNS olup olmadığını denetlemek için > olağanüstü durum kurtarma ağı > DNS sunucuları. DNS sunucusu sanal makineden erişmeyi deneyin. Erişilebilir değilse, ardından erişilebilir DNS sunucusu üzerinde başarısız olan veya DR ağ DNS arasındaki site satırının oluşturma kolaylaştırır.

    ![COM hatası](./media/azure-to-azure-troubleshoot-errors/custom_dns.png)


### <a name="issue-2-site-recovery-configuration-failed-151196"></a>Sorun 2: Site Recovery yapılandırması başarısız oldu (151196)
- **Olası nedeni** </br>
  - Office 365 kimlik doğrulaması ve kimlik IP4 uç noktaları için bağlantı kurulamıyor.

- **Çözümleme**
  - Azure Site Recovery, Office 365 IP aralıkları erişimi kimlik doğrulaması için gereklidir.
    VM üzerinde giden ağ bağlantısını denetlemek için Azure ağ güvenlik grubu (NSG) kuralları/güvenlik duvarı proxy'si kullanıyorsanız, O365 aralıkları için iletişime izin vermek emin olun. Oluşturma bir [Azure Active Directory (AAD) hizmet etiketi](../virtual-network/security-overview.md#service-tags) erişimi için AAD karşılık gelen tüm IP adreslerine izin vermek için NSG kural tabanlı
      - Azure Active Directory (AAD) gelecekte yeni adresler eklenir, yeni NSG kuralları oluşturmak gerekir.

> [!NOTE]
> Sanal makineler arkasında olsa **standart** iç yük dengeleyici sonra O365 IP'ler yani erişimi sahip değil Varsayılan olarak Login.micorsoftonline.com. Olarak değiştirin ya da **temel** iç yük dengeleyici türü veya dışarı bağlanan erişime belirtildiği gibi oluşturma [makale](https://aka.ms/lboutboundrulescli).

### <a name="issue-3-site-recovery-configuration-failed-151197"></a>3. sorun: Site Recovery yapılandırması başarısız oldu (151197)
- **Olası nedeni** </br>
  - Azure Site Recovery Hizmeti uç noktalarına bağlantı kurulamıyor.

- **Çözümleme**
  - Azure Site Recovery gerekli erişim [Site kurtarma IP aralıkları](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-about-networking#outbound-connectivity-for-ip-address-ranges) bölgeye bağlı olarak. Bu gerekli IP aralıkları sanal makineden erişilebilir olduğundan emin olun.


### <a name="issue-4-a2a-replication-failed-when-the-network-traffic-goes-through-on-premises-proxy-server-151072"></a>4. sorun: Ağ trafiği şirket içi proxy sunucusu üzerinden (151072) çıktığında A2A çoğaltması başarısız oldu.
- **Olası nedeni** </br>
  - Özel ara sunucu ayarlarını geçersiz ve ASR Mobility Hizmeti Aracısı otomatik-IE proxy ayarları algılamadı


- **Çözümleme**
  1. Mobility hizmeti aracısı için proxy ayarlarını Windows üzerinde IE ve Linux'ta /etc/environment algılar.
  2. Ardından yalnızca ASR Mobility hizmeti için proxy ayarlamak isterseniz, konumundaki ProxyInfo.conf proxy ayrıntıları sağlayabilirsiniz:</br>
     - ``/usr/local/InMage/config/`` üzerinde ***Linux***
     - ``C:\ProgramData\Microsoft Azure Site Recovery\Config`` üzerinde ***Windows***
  3. ProxyInfo.conf proxy ayarlarını aşağıdaki INI biçiminde olmalıdır.</br>
                *[proxy]*</br>
                *Adres =http://1.2.3.4*</br>
                *Bağlantı noktası 567 =*</br>
  4. ASR Mobility Hizmeti Aracısı destekler yalnızca ***kimliği doğrulanmamış proxy***.


### <a name="fix-the-problem"></a>Sorunu
Güvenilir listeye eklenecek [gerekli URL'leri](azure-to-azure-about-networking.md#outbound-connectivity-for-urls) veya [gerekli IP aralıkları](azure-to-azure-about-networking.md#outbound-connectivity-for-ip-address-ranges), adımları [ağ rehberi belgesi](site-recovery-azure-to-azure-networking-guidance.md).

## <a name="disk-not-found-in-the-machine-error-code-150039"></a>Disk (hata kodu 150039) makinede bulunamadı

VM'ye yeni bir disk başlatılmalıdır.

**Hata kodu** | **Olası nedenler** | **Öneriler**
--- | --- | ---
150039<br></br>**İleti**: Mantıksal birim numarası (LUN) (LUNValue) ile (DiskName) (DiskURI) Azure veri diski, aynı LUN değerine sahip VM içinden bildirilen ilgili disk eşlenmedi. | -Yeni veri diski VM'ye bağlı, ancak başlatılmamış değildi.</br></br>-VM içindeki veri diski diski VM'ye bağlı LUN değerini doğru şekilde bildirmiyor.| Veri disklerini başlatılır ve ardından işlemi yeniden deneyin emin olun.</br></br>Windows için: [Ekleme ve yeni bir disk başlatma](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal).</br></br>Linux için: [Linux'ta yeni bir veri diski başlatın](https://docs.microsoft.com/azure/virtual-machines/linux/add-disk).

### <a name="fix-the-problem"></a>Sorunu
Veri disklerinin başlatıldığından ve sonra işlemi yeniden deneyin emin olun:

- Windows için: [Ekleme ve yeni bir disk başlatma](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal).
- Linux: [Linux'ta yeni bir veri diski ekleme](https://docs.microsoft.com/azure/virtual-machines/linux/add-disk).

Sorun devam ederse desteğe başvurun.


## <a name="unable-to-see-the-azure-vm-for-selection-in-enable-replication"></a>Azure VM için "çoğaltmayı etkinleştir" seçimi görülemiyor

 **1. neden:  Kaynak grubu ve kaynak sanal makine farklı konumlarda** <br>
Azure Site Recovery şu anda kaynak bölge kaynak grubunu ve sanal makineler aynı konumda olması gerektiğini uygulanan. Böyle değilse, ardından, koruma süresi sırasında sanal makineyi bulamadı olmaz.

**2. neden: Kaynak grubu, seçili abonelik parçası değil** <br>
Belirtilen abonelik bir parçası değilse, kaynak grubunu koruma süresi bulmak mümkün olmayabilir. Kaynak grubu kullanılıyor aboneliğe ait olduğundan emin olun.

 **3. neden: Eski yapılandırma** <br>
VM için çoğaltmayı etkinleştirmek istediğiniz görmüyorsanız, Azure sanal makinesinde eski bir Site Recovery yapılandırması nedeniyle kalmayabilir. Eski yapılandırmayı aşağıdaki durumlarda bir Azure sanal makinesinde kalmış olabilir:

- Site Recovery kullanarak Azure VM için çoğaltma etkin ve ardından Site Recovery kasası açıkça bir VM üzerinde çoğaltmayı devre dışı bırakmadan silinir.
- Site Recovery kullanarak Azure VM için çoğaltma etkin ve sonra açıkça bir VM üzerinde çoğaltmayı devre dışı bırakmadan Site Recovery kasasını içeren kaynak grubu silindi.

### <a name="fix-the-problem"></a>Sorunu

>[!NOTE]
>
>"" AzureRM.Resources"" modülü kullanmadan önce güncelleştirdiğinizden emin olun aşağıdaki betiği.

Kullanabileceğiniz [kaldırmak eski ASR yapılandırma betiğini](https://gallery.technet.microsoft.com/Azure-Recovery-ASR-script-3a93f412) ve Azure sanal makinesinde eski Site Recovery yapılandırmayı kaldırmak. Eski yapılandırmayı kaldırdıktan sonra VM görüyor olması gerekir.

## <a name="unable-to-select-virtual-machine-for-protection"></a>Sanal makine koruma için işaretleyin yapılamıyor
 **1. neden:  Sanal makine başarısız veya yanıt vermeyen bir durumda yüklü bazı uzantılı** <br>
 Sanal makineler gidin > ayarı > Uzantılar ve başarısız durumda herhangi bir uzantısı var olup olmadığını denetleyin. Başarısız uzantının yüklemesini kaldırmak ve sanal makine korumayı yeniden deneyin.<br>
 **2. neden:  [VM'in sağlama durumu geçerli değil](#vms-provisioning-state-is-not-valid-error-code-150019)**

## <a name="vms-provisioning-state-is-not-valid-error-code-150019"></a>VM'in sağlama durumu geçerli değil (hata kodu 150019)

VM üzerinde çoğaltmayı etkinleştirmek için sağlama durumu olmalıdır **başarılı**. Aşağıdaki adımları izleyerek VM durumunu kontrol edebilirsiniz.

1.  Seçin **kaynak Gezgini** gelen **tüm hizmetleri** Azure portalında.
2.  Genişletin **abonelikleri** listelemek ve aboneliğinizi seçin.
3.  Genişletin **ResourceGroups** listeleyebilir ve sanal makinenin kaynak grubunu seçin.
4.  Genişletin **kaynakları** listesinde ve sanal makinenizi seçin
5.  Denetleme **provisioningState** örneği görünümünde, sağ taraftaki alan.

### <a name="fix-the-problem"></a>Sorunu

- Varsa **provisioningState** olduğu **başarısız**, sorun giderme ayrıntıları ile Destek ekibiyle iletişime geçin.
- Varsa **provisioningState** olduğu **güncelleştirme**, başka bir uzantı dağıtılabilecek. Bekleme başarısız Site kurtarma işlemini yeniden deneyin ve bunlar için VM üzerinde devam eden herhangi bir işlem olup olmadığını kontrol **çoğaltmayı etkinleştir** işi.

## <a name="unable-to-select-target-virtual-network---network-selection-tab-is-grayed-out"></a>Sanal ağ - ağ seçimi sekmesi gri renkte hedefi seçmek yüklenemiyor.

**1. neden: Sanal makinenizin 'hedef ağ' için zaten eşlenmiş bir ağa bağlıysa.**
- Kaynak VM sanal ağ bir parçasıdır ve hedef kaynak grubunda bir ağ ile aynı sanal ağdaki başka bir VM'den zaten eşlenmiş, ardından tarafından varsayılan ağ seçimi açılan menüsü devre dışı bırakılır.

![Network_Selection_greyed_out](./media/site-recovery-azure-to-azure-troubleshoot/unabletoselectnw.png)

**2. neden: Varsa daha önce Azure Site Recovery kullanarak VM'yi korumalı ve çoğaltma devre dışı.**
 - Sanal Makinenin çoğaltmasını devre dışı bırakma ağ eşlemesi silmez. Burada VM korunan kurtarma Hizmetleri kasası silinmesi gerekir. </br>
 Kurtarma hizmeti Kasası'na gidin > Site Recovery altyapısı > Ağ eşlemesi. </br>
 ![Delete_NW_Mapping](./media/site-recovery-azure-to-azure-troubleshoot/delete_nw_mapping.png)
 - Olağanüstü durum kurtarma kurulumu sırasında yapılandırılan hedef ağı, VM korunmaya başladıktan sonra ilk sonra değiştirilebilir. </br>
 ![Modify_NW_mapping](./media/site-recovery-azure-to-azure-troubleshoot/modify_nw_mapping.png)
 - Bu belirli ağ eşlemesi kullanan VM'ler korumalı ağ eşlemesini değiştirmek tüm etkilediğini unutmayın.


## <a name="comvolume-shadow-copy-service-error-error-code-151025"></a>COM +/ Birim Gölge Kopyası Hizmeti hatası (hata kodu 151025)

**Hata kodu** | **Olası nedenler** | **Öneriler**
--- | --- | ---
151025<br></br>**İleti**: Site kurtarma uzantısı yüklenemedi | -'COM + Sistem uygulaması' hizmeti devre dışı.</br></br>-'Birim gölge kopyası' hizmeti devre dışı bırakıldı.| 'COM + Sistem uygulaması' ve 'Birim gölge kopyası' hizmetlerini otomatik veya el ile başlatma moduna ayarlayın.

### <a name="fix-the-problem"></a>Sorunu

'Hizmetler' konsolunu açın ve 'COM + Sistem uygulaması' emin olun ve 'Birim gölge kopyası', 'Başlangıç türü' 'Devre dışı' olarak ayarlanmamış.
  ![COM hatası](./media/azure-to-azure-troubleshoot-errors/com-error.png)

## <a name="unsupported-managed-disk-size-error-code-150172"></a>Desteklenmeyen yönetilen Disk boyutu (hata kodu 150172)


**Hata kodu** | **Olası nedenler** | **Öneriler**
--- | --- | ---
150172<br></br>**İleti**: Desteklenen en küçük boyutu (DiskSize) ile (DiskName) içerdiğinden, sanal makine boyutu için 1024 MB koruma etkinleştirilemedi. | -Disk desteklenen boyut 1024 MB değerinden küçük| Disk boyutlarının desteklenen boyut aralıkları içinde olduğundan ve işlemi yeniden deneyin emin olun.

## <a name="enable-protection-failed-as-device-name-mentioned-in-the-grub-configuration-instead-of-uuid-error-code-151126"></a>GRUB yapılandırması UUID (hata kodu 151126) yerine belirtilen bir cihaz adı olarak koruma etkinleştirilemedi

**Olası neden:** </br>
GRUB yapılandırma dosyaları ("/ boot/grub/menu.lst", "/ boot/grub/grub.cfg", "/ boot/grub2/grub.cfg" veya "/ varsayılan/etc/grub") parametre değeri içerebilir **kök** ve **sürdürme** olarak UUID yerine gerçek cihaz adları. Site Recovery VM gelen aynı ada sahip sorunları kaynaklanan yük devretmede büyütme gibi değil aygıtlarını adı VM yeniden başlatma arasında değişiklik gösterebileceği UUID yaklaşım zorunlu kılar. Örneğin: </br>


- GRUB dosyasıdır aşağıdaki satırı **/boot/grub2/grub.cfg**. <br>
  *Linux /boot/vmlinuz-3.12.49-11-default **kök = / dev/sda2** ${extra_cmdline} **= / dev/sda1 sürdürme** splash sessiz sessiz showopts =*


- GRUB dosyasıdır aşağıdaki satırı **/boot/grub/menu.lst**
  *çekirdek /boot/vmlinuz-3.0.101-63-default **kök = / dev/sda2** **= / dev/sda1 Sürdür ** splash sessiz crashkernel = 256M-:128M showopts vga = 0x314 =*

Yukarıdaki kalın dize gözlemlerseniz, GRUB parametreleri "root" ve "Devam" UUID yerine gerçek cihaz adları vardır.

**Nasıl:**<br>
Cihaz adları, karşılık gelen UUID ile değiştirilmelidir.<br>


1. Komutunu yürüterek cihazı UUID'si Bul "blkid \<cihaz adı >". Örneğin:<br>
   ```
   blkid /dev/sda1
   ```<br>
   ```/dev/sda1: UUID="6f614b44-433b-431b-9ca1-4dd2f6f74f6b" TYPE="swap" ```<br>
   ```blkid /dev/sda2```<br>
   ```/dev/sda2: UUID="62927e85-f7ba-40bc-9993-cc1feeb191e4" TYPE="ext3"
   ```<br>



1. Now replace the device name with its UUID in the format like "root=UUID=\<UUID>". For example, if we replace the device names with UUID for root and resume parameter mentioned above in the files "/boot/grub2/grub.cfg", "/boot/grub2/grub.cfg" or "/etc/default/grub: then the lines in the files looks like. <br>
   *kernel /boot/vmlinuz-3.0.101-63-default **root=UUID=62927e85-f7ba-40bc-9993-cc1feeb191e4** **resume=UUID=6f614b44-433b-431b-9ca1-4dd2f6f74f6b** splash=silent crashkernel=256M-:128M showopts vga=0x314*
1. Restart the protection again

## Enable protection failed as device mentioned in the GRUB configuration doesn't exist(error code 151124)
**Possible Cause:** </br>
The GRUB configuration files ("/boot/grub/menu.lst", "/boot/grub/grub.cfg", "/boot/grub2/grub.cfg" or "/etc/default/grub") may contain the parameters "rd.lvm.lv" or "rd_LVM_LV" to indicate the LVM device that should be discovered at the time of booting. If these LVM devices doesn't exist, then the protected system itself will not boot and stuck in the boot process. Even the same will be observed with the failover VM. Below are few examples:

Few examples: </br>

1. The following line is from the GRUB file **"/boot/grub2/grub.cfg"** on RHEL7. </br>
   *linux16 /vmlinuz-3.10.0-957.el7.x86_64 root=/dev/mapper/rhel_mup--rhel7u6-root ro crashkernel=128M\@64M **rd.lvm.lv=rootvg/root rd.lvm.lv=rootvg/swap** rhgb quiet LANG=en_US.UTF-8*</br>
   Here the highlighted portion shows that the GRUB has to detect two LVM devices with names **"root"** and **"swap"** from the volume group "rootvg".
1. The following line is from the GRUB file **"/etc/default/grub"** on RHEL7 </br>
   *GRUB_CMDLINE_LINUX="crashkernel=auto **rd.lvm.lv=rootvg/root rd.lvm.lv=rootvg/swap** rhgb quiet"*</br>
   Here the highlighted portion shows that the GRUB has to detect two LVM devices with names **"root"** and **"swap"** from the volume group "rootvg".
1. The following line is from the GRUB file **"/boot/grub/menu.lst"** on RHEL6 </br>
   *kernel /vmlinuz-2.6.32-754.el6.x86_64 ro root=UUID=36dd8b45-e90d-40d6-81ac-ad0d0725d69e rd_NO_LUKS LANG=en_US.UTF-8 rd_NO_MD SYSFONT=latarcyrheb-sun16 crashkernel=auto rd_LVM_LV=rootvg/lv_root  KEYBOARDTYPE=pc KEYTABLE=us rd_LVM_LV=rootvg/lv_swap rd_NO_DM rhgb quiet* </br>
   Here the highlighted portion shows that the GRUB has to detect two LVM devices with names **"root"** and **"swap"** from the volume group "rootvg".<br>

**How to Fix:**<br>

If the LVM device doesn't exist, fix either by creating it or remove the parameter for the same from the GRUB configuration files and then retry the enable protection. </br>

## Site recovery mobility service update completed with warnings ( error code 151083)
Site Recovery mobility service has many components, one of which is called filter driver. Filter driver gets loaded into system memory only at a time of system reboot. Whenever there are  site recovery mobility service updates that has filter driver changes, we update the machine but still gives you warning that some fixes require a reboot. It means that the filter driver fixes can only be realized when a new filter driver is loaded which can happen only at the time of system reboot.<br>
**Please note** that this is just a warning and existing replication keeps on working even after the new agent update. You can choose to reboot anytime you want to get the benefits of new filter driver but if you don't reboot than also old filter driver keeps on working. Apart from filter driver, **benefits of  any other enhancements and fixes in mobility service get realized without any reboot when the agent gets updated.**  


## Protection couldn't be enabled as replica managed disk 'diskname-replica' already exists without expected tags in the target resource group( error code 150161

**Cause**: It can occur if the  virtual machine was protected earlier in the past and during disabling the replication, replica disk was not cleaned due to some reason.</br>
**How to fix:**
Delete the mentioned replica disk in the error message and restart the failed protection job again.

## Next steps
[Replicate Azure virtual machines](site-recovery-replicate-azure-to-azure.md)
