---
title: Azure Site Recovery VMware Azure'a sorunlarını giderme | Microsoft Docs
description: Azure sanal makineleri çoğaltırken hataları giderin.
services: site-recovery
author: anoopkv
manager: gauravd
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.date: 01/11/2018
ms.author: anoopkv
ms.openlocfilehash: a03766f8a22399f7d72a01f8d744bfd1cef90ac3
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2018
ms.locfileid: "29768518"
---
# <a name="troubleshoot-mobility-service-push-installation-issues"></a>Mobilite hizmetinin göndermeli yükleme sorunlarını giderme

Bu makalede, Azure Site Recovery Mobility hizmeti korumasını etkinleştirmek için kaynak sunucuda yüklemeye çalıştığınızda yüz yaygın sorunları açıklar.

## <a name="error-78007---the-requested-operation-could-not-be-completed"></a>Hata 78007 - istenen işlem tamamlanamadı
Bu hata, çeşitli nedenlerle hizmeti tarafından atılabilir. Daha fazla sorun giderme için ilgili sağlayıcı hatası seçin.

* [Hata 95103](#error-95103---protection-could-not-be-enabled-ep0854) 
* [Hata 95105](#error-95105---protection-could-not-be-enabled-ep0856) 
* [Hata 95107](#error-95107---protection-could-not-be-enabled-ep0858) 
* [Hata 95108](#error-95108---protection-could-not-be-enabled-ep0859) 
* [Hata 95117](#error-95117---protection-could-not-be-enabled-ep0865) 
* [Hata 95213](#error-95213---protection-could-not-be-enabled-ep0874) 
* [Hata 95224](#error-95224---protection-could-not-be-enabled-ep0883) 
* [Hata 95265](#error-95265---protection-could-not-be-enabled-ep0902) 


## <a name="error-95105---protection-could-not-be-enabled-ep0856"></a>Hata 95105 - koruma yüklenemedi (EP0856) etkin

**Hata kodu** | **Olası nedenler** | **Hata özel öneriler**
--- | --- | ---
95105 </br>**İleti:** mobilite hizmetinin göndermeli yüklemesi için kaynak makine hata koduyla başarısız oldu **EP0856**. <br> Her iki **dosya ve Yazıcı Paylaşımı** ve kaynak makine veya var. olan ağ bağlantısı sorunları işlem sunucusu ve kaynak makine arasında izin verilmiyor.| **Dosya ve Yazıcı Paylaşımı** etkin değil. | İzin **dosya ve Yazıcı Paylaşımı** Windows Güvenlik Duvarı'nda Kaynak makinede. Kaynak makinede altında **Windows Güvenlik Duvarı** > **bir uygulama veya özelliğin güvenlik duvarı aracılığıyla izin**seçin **dosya ve yazıcı paylaşımı tüm profiller için**. </br> Ayrıca, yükleme başarıyla tamamlamak için aşağıdaki önkoşulları denetleyin.<br> Daha fazla bilgi edinin [sorunlarını giderme WMI](#troubleshoot-wmi-issues).


## <a name="error-95107---protection-could-not-be-enabled-ep0858"></a>Hata 95107 - koruma yüklenemedi (EP0858) etkin

**Hata kodu** | **Olası nedenler** | **Hata özel öneriler**
--- | --- | ---
95107 </br>**İleti:** mobilite hizmetinin göndermeli yüklemesi için kaynak makine hata koduyla başarısız oldu **EP0858**. <br> Mobilite hizmetinin yüklenmesi için sağlanan kimlik bilgileri yanlış veya kullanıcı hesabı yetersiz ayrıcalıklara sahip. | Kaynak makinede mobilite hizmetinin yüklenmesi için sağlanan kullanıcı kimlik bilgileri yanlış. | Yapılandırma sunucusundaki kaynak makine için sağlanan kullanıcı kimlik bilgilerinin doğru olduğundan emin olun. <br> Eklemek veya kullanıcı kimlik bilgilerini düzenlemek için yapılandırma sunucusuna gidin ve seçin **Cspsconfigtool** > **hesabını yönetmek**. </br> Ayrıca, aşağıdakileri denetleyin [Önkoşullar](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) başarıyla yüklemeyi tamamlamak için.


## <a name="error-95117---protection-could-not-be-enabled-ep0865"></a>Hata 95117 - koruma yüklenemedi (EP0865) etkin

**Hata kodu** | **Olası nedenler** | **Hata özel öneriler**
--- | --- | ---
95117 </br>**İleti:** mobilite hizmetinin göndermeli yüklemesi için kaynak makine hata koduyla başarısız oldu **EP0865**. <br> Kaynak makine çalışmıyor veya işlem sunucusu ve kaynak makine arasında ağ bağlantısı sorunları vardır. | İşlem sunucusu ve kaynak sunucu arasındaki ağ bağlantısı sorunları. | İşlem sunucusu ve kaynak sunucu arasındaki bağlantıyı denetleyin. </br> Ayrıca, aşağıdakileri denetleyin [Önkoşullar](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) başarıyla yüklemeyi tamamlamak için.|

## <a name="error-95103---protection-could-not-be-enabled-ep0854"></a>Hata 95103 - koruma yüklenemedi (EP0854) etkin

**Hata kodu** | **Olası nedenler** | **Hata özel öneriler**
--- | --- | ---
95103 </br>**İleti:** mobilite hizmetinin göndermeli yüklemesi için kaynak makine hata koduyla başarısız oldu **EP0854**. <br> Windows Yönetim Araçları (WMI) kaynak makinede izin verilmeyen ya da işlem sunucusu ve kaynak makine arasında ağ bağlantısı sorunları vardır.| WMI Windows Güvenlik Duvarı'nda engellendi. | WMI Windows Güvenlik Duvarı'nda izin verin. Altında **Windows Güvenlik Duvarı** > **bir uygulama veya özelliğin güvenlik duvarı aracılığıyla izin**seçin **tüm profiller için WMI**. </br> Ayrıca, aşağıdakileri denetleyin [Önkoşullar](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) başarıyla yüklemeyi tamamlamak için.|

## <a name="error-95213---protection-could-not-be-enabled-ep0874"></a>Hata 95213 - koruma yüklenemedi (EP0874) etkin

**Hata kodu** | **Olası nedenler** | **Hata özel öneriler**
--- | --- | ---
95213 </br>**İleti:** yükleme kaynağına mobilite hizmetinin % Sourceıp; makinesi hata koduyla başarısız oldu **EP0874**. <br> | Kaynak makinedeki işletim sistemi sürümü desteklenmiyor. <br>| Kaynak makine işletim sistemi sürümünün desteklendiğinden emin olun. Okuma [destek matrisi](https://aka.ms/asr-os-support). </br> Ayrıca, aşağıdakileri denetleyin [Önkoşullar](https://aka.ms/pushinstallerror) başarıyla yüklemeyi tamamlamak için.| 


## <a name="error-95108---protection-could-not-be-enabled-ep0859"></a>Hata 95108 - koruma yüklenemedi (EP0859) etkin

**Hata kodu** | **Olası nedenler** | **Hata özel öneriler**
--- | --- | ---
95108 </br>**İleti:** mobilite hizmetinin göndermeli yüklemesi için kaynak makine hata koduyla başarısız oldu **EP0859**. <br>| Mobilite hizmetinin yüklenmesi için sağlanan kimlik bilgileri yanlış veya kullanıcı hesabı yetersiz ayrıcalıklara sahip. <br>| Sağlanan kimlik bilgileri olduğundan emin olun **kök** hesabının kimlik bilgileri. Eklemek veya kullanıcı kimlik bilgilerini düzenlemek için yapılandırma sunucusuna gidin ve seçin **Cspsconfigtool** Masaüstünde kısayol simgesi. Seçin **hesabını yönetmek** ekleyin veya kimlik bilgilerini düzenlemek için.|

## <a name="error-95265---protection-could-not-be-enabled-ep0902"></a>Hata 95265 - koruma yüklenemedi (EP0902) etkin

**Hata kodu** | **Olası nedenler** | **Hata özel öneriler**
--- | --- | ---
95265 </br>**İleti:** mobilite hizmetinin göndermeli yüklemesi başarılı oldu kaynak makine ancak kaynak makine için bazı sistem değişikliklerin etkili olması yeniden başlatma gerektirir. <br>| Mobility hizmeti daha eski bir sürümü sunucuda zaten yüklü.| Sanal makinenin çoğaltma sorunsuz bir şekilde devam eder.<br> Mobility hizmetinin yeni geliştirmeleri avantajlarından yararlanabilmek için sonraki bakım penceresi sırasında sunucuyu yeniden başlatın.|


## <a name="error-95224---protection-could-not-be-enabled-ep0883"></a>Hata 95224 - koruma yüklenemedi (EP0883) etkin

**Hata kodu** | **Olası nedenler** | **Hata özel öneriler**
--- | --- | ---
95224 </br>**İleti:** Mobility hizmetinin yüklenmesi kaynak makine % Sourceıp; hata koduyla başarısız oldu **EP0883**. Bir önceki yükleme veya güncelleştirme sistemin yeniden başlatılması beklemede.| Sistem, Mobility hizmeti daha eski veya uyumlu bir sürümü kaldırırken yeniden değildi.| Mobility hizmeti sürümü olmadığı sunucusunda var olduğundan emin olun. <br> Sunucuyu yeniden başlatın ve koruma etkinleştirme işini yeniden çalıştırın.|

## <a name="resource-to-troubleshoot-push-installation-problems"></a>Göndermeli yükleme sorunlarını gidermek için kaynak

#### <a name="troubleshoot-file-and-print-sharing-issues"></a>Dosyası sorunlarını gidermek ve yazdırma paylaşım sorunları
* [Etkinleştirmek veya dosya paylaşımı Grup İlkesi'yle devre dışı](https://technet.microsoft.com/library/cc754359(v=ws.10).aspx)
* [Dosya ve yazıcı paylaşımı Windows Güvenlik Duvarı üzerinden etkinleştirme](https://technet.microsoft.com/library/ff633412(v=ws.10).aspx)

#### <a name="troubleshoot-wmi-issues"></a>WMI sorunlarını giderme
* [Temel WMI sınama](https://blogs.technet.microsoft.com/askperf/2007/06/22/basic-wmi-testing/)
* [WMI sorun giderme](https://msdn.microsoft.com/library/aa394603(v=vs.85).aspx)
* [WMI komut dosyaları ve WMI hizmetleri ile ilgili sorunları giderme](https://technet.microsoft.com/library/ff406382.aspx#H22)

## <a name="next-steps"></a>Sonraki adımlar

[Bilgi nasıl](tutorial-vmware-to-azure.md) VMware Vm'leri için olağanüstü durum kurtarma ayarlamak için.