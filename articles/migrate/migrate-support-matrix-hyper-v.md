---
title: Hyper-V değerlendirme ve geçiş için Azure geçişi destek matrisi
description: Ayarları ve Hyper-V değerlendirme ve Azure geçişi hizmetini kullanarak geçiş sınırlamaları özetlenmektedir.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 06/02/2019
ms.author: raynew
ms.openlocfilehash: f6edbe19429b38d68aea1f1ecfe426c9b2d194d0
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67811355"
---
# <a name="support-matrix-for-hyper-v-assessment-and-migration"></a>Hyper-V değerlendirme ve geçiş için destek matrisi

Kullanabileceğiniz [Azure geçişi hizmeti](migrate-overview.md) değerlendirmek ve makineleri için Microsoft Azure bulutuna geçirmek için. Bu makalede, destek ayarları ve değerlendirme ve şirket içi Hyper-V sanal makineleri geçirmek için sınırlamaları özetlenmektedir.



## <a name="hyper-v-scenarios"></a>Hyper-V senaryoları

Tabloda, Hyper-V Vm'leri için desteklenen senaryolar özetlenmiştir.

**Dağıtım** | **Ayrıntıları*** 
--- | --- 
**Şirket içi Hyper-V sanal makineleri değerlendirme** | [Ayarlanan](tutorial-prepare-hyper-v.md) ilk değerlendirmenizi.<br/><br/> [Çalıştırma](scale-hyper-v-assessment.md) büyük ölçekli bir değerlendirme.
**Hyper-V Vm'lerini Azure'a geçirme** | [Denemenin](tutorial-migrate-hyper-v.md) Azure'a geçiş için.

    

## <a name="azure-migrate-projects"></a>Azure geçiş projeleri

**Destek** | **Ayrıntılar**
--- | ---
Azure izinleri | Bir Azure geçişi projesi oluşturmak için katkıda bulunan veya sahip izinleri aboneliği gerekir.
Hyper-V Sanal Makineleri | Tek bir projede en fazla 10.000 Hyper-V Vm'lerini değerlendirin.

Bir proje, VMware Vm'lerini hem Hyper-V Vm'lerinden, en fazla değerlendirme sınırları içerebilir.


## <a name="assessment-hyper-v-host-requirements"></a>Değerlendirme Hyper-V konak gereksinimleri

| **Destek**                | **Ayrıntılar**               
| :-------------------       | :------------------- |
| **Konak dağıtımı**       | Hyper-V konak tek başına olabilir veya bir kümede dağıtılan. |
| **İzinler**           | Hyper-V konağında yönetici izinlerine ihtiyacınız vardır. |
| **Konak işletim sistemi** | Windows Server 2016 veya Windows Server 2012 R2.<br/> Windows Server 2019 çalıştıran Hyper-V konaklarında bulunan Vm'leri değerlendiremediğinde. |
| **PowerShell uzaktan iletişimi**   | Her konakta etkin olmalıdır. |
| **Hyper-V çoğaltma**       | Hyper-V çoğaltma kullanın (veya aynı VM tanımlayıcılarına sahip birden çok Vm'si vardır) ve Azure Geçişi'ni kullanarak hem özgün ve çoğaltılan Vm'leri bulmak, Azure geçişi tarafından oluşturulan değerlendirmesi doğru olmayabilir. |


## <a name="assessment-hyper-v-vm-requirements"></a>Değerlendirme Hyper-V VM gereksinimleri

| **Destek**                  | **Ayrıntılar**               
| :----------------------------- | :------------------- |
| **İşletim sistemi** | Tüm [Windows](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines) ve [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) Azure tarafından desteklenen işletim sistemleri. |
| **İzinler**           | Değerlendirmek istediğiniz her Hyper-V VM üzerinde yönetici izinlerine ihtiyacınız vardır. |
| **Tümleştirme hizmetleri**       | [Hyper-V tümleştirme hizmetlerini](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/integration-services) Vm'lerde işletim sistemi bilgilerini yakalamak için değerlendirme çalıştırılması gerekir. |
| **Azure için gerekli değişiklikleri** | Azure'da çalıştırabilmeniz için önce bazı sanal değişiklikler gerektirebilir. Azure geçişi, aşağıdaki işletim sistemleri için otomatik olarak bu değişiklikleri yapar:<br/> - Red Hat Enterprise Linux 6.5+, 7.0+<br/> - CentOS 6.5+, 7.0+</br> - SUSE Linux Enterprise Server 12 SP1+<br/> - Ubuntu 14.04LTS, 16.04LTS, 18.04LTS<br/> - Debian 7,8<br/><br/> Diğer işletim sistemleri için el ile geçiş işleminden önce ayarlamalar yapmanız gerekir. İlgili makaleler, bunun nasıl yapılacağı hakkında yönergeler içerir. |
| **Linux önyükleme**                 | Makinesiyse adanmış bir bölüme ise, işletim sistemi diskinde bulunması gereken ve birden çok diske yayılma değil.<br/> Makinesiyse (/) kök bölümünde parçası ise, ardından '/' bölüm işletim sistemi diskinde olması ve diğer disklerin span değil. |
| **UEFI önyüklemesi**                  | UEFI boot'a sahip sanal makineleri geçiş için desteklenmiyor. |
| **Şifrelenmiş diskler/birimler**    | Şifrelenmiş diskler/birimleri olan sanal makineleri geçiş için desteklenmiyor. |
| **RDM/geçiş diskleri**      | VM, RDM veya doğrudan geçiş diskleri varsa, bu diskleri Azure'a çoğaltılması olmaz. |
| **NFS**                        | NFS birimleri Vm'lerde birimleri olarak bağlanabilir çoğaltılması gerekmez. |
| **Hedef disk**                | Azure geçişi değerlendirmeleri, yalnızca yönetilen disklerle Azure Vm'lerine geçiş önerilir. |


## <a name="assessment-appliance-requirements"></a>Gereç değerlendirme gereksinimleri

Değerlendirme için Azure geçişi, Hyper-V Vm'lerini keşfedin ve Azure geçişi için VM meta verileri ve performans verilerini göndermek için basit bir gereç çalıştırır. Gereç bir Hyper-V sanal makine üzerinde çalışır ve Azure portalından indirdiğiniz sıkıştırılmış bir Hyper-V VHD kullanarak ayarlayabilirsiniz. Aşağıdaki tabloda, gereç gereksinimleri özetlenmektedir.

| **Destek**                | **Ayrıntılar**               
| :-------------------       | :------------------- |
| **Azure geçişi projesi**  |  Gereçlerden biri tek bir proje ile ilişkili olabilir.<br/> Tek bir Gereci ile en fazla 5000 Hyper-V Vm'leri bulabilir.
| **Hyper-V sınırlamaları**    |  Hyper-V VM olarak gereç dağıtırsınız.<br/> Sağlanan sanal makine Hyper-V VM sürüm 5.0 gereçtir.<br/> VM konak Windows Server 2012 R2 çalıştırmalıdır veya üzeri.<br/> 16 GB RAM, 4 sanal işlemci ayırmak üzere yeterli alan gerekir ve 1 dış VM Gereci için geçiş yapabilirsiniz.<br/> Gereç, statik veya dinamik IP adresi ve Internet erişimi gerektirir.
| **Hyper-V gereç**      |  Gereç, Hyper-V VM olarak ayarlanır.<br/> İndirme için sağlanan VHD Hyper-V VM sürüm 5.0 ' dir.
| **Ana Bilgisayar**                   | VM konağı VM Gereci çalıştıran Windows Server 2012 R2 çalıştırmalıdır veya üzeri.<br/> Bu, 16 GB RAM, 4 sanal işlemci ve bir dış anahtara sanal Gereci için ayırmak üzere yeterli alan gerekir.<br/> Gereç, statik veya dinamik IP adresi ve Internet erişimi gerektirir. |
| **Geçiş desteği**      | Makine çoğaltmaya başlamak için gereç geçiş ağ geçidi hizmetini 1.18.7141.12919 olmalıdır veya üzeri. Sürümü denetlemek için gereç web uygulamasına oturum açın. |

## <a name="assessment-appliance-url-access"></a>Değerlendirme Gereci URL erişimi

Sanal makineleri değerlendirme için Azure geçişi Gereci internet bağlantısı gerekir.

- Gereç dağıtırken, Azure geçişi tabloda özetlenen URL'lere bir bağlantı denetimi yapar.
- URL tabanlı bir firewall.proxy kullanıyorsanız, proxy herhangi bir CNAME kayıtları URL'lere aranırken alınan çözümler sağlamaktan tablonun URL'lere erişim verin.
- Araya giren bir proxy varsa, sunucu sertifikasının gerecine proxy sunucusundan içeri aktarmak gerekebilir. 

    
**URL** | **Ayrıntılar**  
--- | --- 
*.portal.azure.com | Azure portalında gezinme
*.windows.net | Azure aboneliğinizde oturum açın
*.microsoftonline.com | Hizmet iletişimleri gerecine için oluşturma, Azure Active Directory uygulamaları.
Management.Azure.com | Hizmet iletişimleri gerecine için oluşturma, Azure Active Directory uygulamaları.
dc.services.visualstudio.com | Günlüğe kaydetme ve izleme 
*.vault.azure.net | Azure Key Vault gizli dizileri Gereci ile hizmet arasında iletişim kurarken yönetin.


## <a name="assessment-port-requirements"></a>Değerlendirme-bağlantı noktası gereksinimleri

Değerlendirme için bağlantı noktası gereksinimleri aşağıdaki tabloda özetlenmiştir.

**cihaz** | **bağlantı**
--- | --- 
**Gereç** | Gereç için Uzak Masaüstü bağlantılarına izin verecek şekilde 3389 numaralı TCP bağlantı noktasında gelen bağlantılara.<br/> Gelen bağlantılar 44368 Gereci yönetim uygulamayı URL kullanarak uzaktan erişmek için bağlantı noktası: https://<appliance-ip-or-name>:44368<br/> Azure geçişi için bulma ve performans meta verileri göndermek için bağlantı noktası 443 üzerinden giden bağlantılar.
**Hyper-V konak/küme** | WinRM bağlantı noktası 5985'tir (HTTP) ve (HTTPS) bir ortak bilgi modeli (CIM) oturumu kullanarak bir Hyper-V VM yapılandırma ve performans meta veri çekmek için 5986 bağlantılarda gelen.

## <a name="migration-hyper-v-host-requirements"></a>Geçiş Hyper-V konak gereksinimleri

| **Destek**                | **Ayrıntılar**               
| :-------------------       | :------------------- |
| **Konak dağıtımı**       | Hyper-V konak tek başına olabilir veya bir kümede dağıtılan. |
| **İzinler**           | Hyper-V konağında yönetici izinlerine ihtiyacınız vardır. |
| **Konak işletim sistemi** | Windows Server 2019, Windows Server 2016 veya Windows Server 2012 R2. |

## <a name="migration-hyper-v-vm-requirements"></a>Geçiş Hyper-V VM gereksinimleri

| **Destek**                  | **Ayrıntılar**               
| :----------------------------- | :------------------- |
| **İşletim sistemi** | Tüm [Windows](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines) ve [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) Azure tarafından desteklenen işletim sistemleri. |
| **İzinler**           | Değerlendirmek istediğiniz her Hyper-V VM üzerinde yönetici izinlerine ihtiyacınız vardır. |
| **Tümleştirme hizmetleri**       | [Hyper-V tümleştirme hizmetlerini](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/integration-services) Vm'lerde işletim sistemi bilgilerini yakalamak için değerlendirme çalıştırılması gerekir. |
| **Azure için gerekli değişiklikleri** | Azure'da çalıştırabilmeniz için önce bazı sanal değişiklikler gerektirebilir. Azure geçişi, aşağıdaki işletim sistemleri için otomatik olarak bu değişiklikleri yapar:<br/> - Red Hat Enterprise Linux 6.5+, 7.0+<br/> - CentOS 6.5+, 7.0+</br> - SUSE Linux Enterprise Server 12 SP1+<br/> - Ubuntu 14.04LTS, 16.04LTS, 18.04LTS<br/> - Debian 7,8<br/><br/> Diğer işletim sistemleri için el ile geçiş işleminden önce ayarlamalar yapmanız gerekir. İlgili makaleler, bunun nasıl yapılacağı hakkında yönergeler içerir. |
| **Linux önyükleme**                 | Makinesiyse adanmış bir bölüme ise, işletim sistemi diskinde bulunması gereken ve birden çok diske yayılma değil.<br/> Makinesiyse (/) kök bölümünde parçası ise, ardından '/' bölüm işletim sistemi diskinde olması ve diğer disklerin span değil. |
| **UEFI önyüklemesi**                  | UEFI boot'a sahip sanal makineleri geçiş için desteklenmiyor. |
| **Şifrelenmiş diskler/birimler**    | Şifrelenmiş diskler/birimleri olan sanal makineleri geçiş için desteklenmiyor. |
| **RDM/geçiş diskleri**      | VM, RDM veya doğrudan geçiş diskleri varsa, bu diskleri Azure'a çoğaltılması olmaz. |
| **NFS**                        | NFS birimleri Vm'lerde birimleri olarak bağlanabilir çoğaltılması gerekmez. |
| **Hedef disk**                | Yalnızca yönetilen disklerle Azure Vm'lerine geçirebilirsiniz. |




## <a name="migration-hyper-v-host-url-access"></a>Geçiş Hyper-V konağı URL erişimi

Aşağıdaki tabloda, Hyper-V konakları için URL erişimi gereksinimleri özetlenmektedir.

**URL** | **Ayrıntılar**  
--- | ---
login.microsoftonline.com | Active Directory kullanarak erişim denetimi ve kimlik yönetimi.
*.backup.windowsazure.com | Çoğaltma veri aktarımı ve düzenlemesi için.
*.hypervrecoverymanager.windowsazure.com | Azure geçişi hizmeti URL'leri için bağlanın.
*.blob.core.windows.net | Veri depolama hesaplarına karşıya yükleyin.
dc.services.visualstudio.com | İç izleme için kullanılan uygulama günlüklerini karşıya yükleyin.
time.windows.com | Sistem ile genel saat arasındaki saat eşitlemesini doğrular.

## <a name="migration-port-access"></a>Geçiş bağlantı noktası erişim

Aşağıdaki tabloda, VM geçişi için Hyper-V konakları ve sanal bağlantı noktası gereksinimleri özetlenmektedir.

**cihaz** | **bağlantı**
--- | --- 
Hyper-V konakları/VM'ler | Giden HTTPS bağlantılara Azure geçişi için sanal makine çoğaltma verilerini göndermek için 443 numaralı bağlantı noktası.

  
## <a name="migration-vm-disk-support"></a>Geçiş VM disk desteği 

**Destek** | **Ayrıntılar**
--- | ---
Geçirilen disk | Vm'leri azure'da yönetilen disklere (standart HHD, premium SSD) yalnızca geçirilebilir.
   

## <a name="next-steps"></a>Sonraki adımlar

[İçin Hyper-V sanal makine değerlendirmesine hazırlanma](tutorial-prepare-hyper-v.md) geçiş.




 
