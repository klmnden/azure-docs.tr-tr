---
title: Azure Site Recovery Vmware'den Azure'a sorunlarını giderme | Microsoft Docs
description: Azure sanal makineleri çoğaltırken hatalarında sorun giderme.
services: site-recovery
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.author: ramamill
ms.date: 07/06/2018
ms.openlocfilehash: 8d5db03eeebb659414ea1f554e5b34c938fd2795
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37952918"
---
# <a name="troubleshoot-mobility-service-push-installation-issues"></a>Mobility hizmeti anında yükleme sorunlarını giderme

Bu makalede, Azure Site Recovery Mobility hizmeti korumasını etkinleştirmek için kaynak sunucuda yüklemeye çalıştığınızda karşılaşacağınız sık karşılaşılan sorunları giderme açıklar.

## <a name="error-78007---the-requested-operation-could-not-be-completed"></a>Hata 78007 - istenen işlem tamamlanamadı
Bu hata, çeşitli nedenlerle hizmeti tarafından atılabilir. Daha fazla sorun giderme için karşılık gelen sağlayıcıyı seçin.

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
95105 </br>**İleti:** anında iletme kaynak makine için Mobility hizmeti yüklemesi başarısız oldu, hata kodu ile **EP0856**. <br> Her iki **dosya ve Yazıcı Paylaşımı** kaynak makine veya orada olan ağ bağlantı sorunları işlem sunucusu ve kaynak makine arasında izin verilmiyor.| **Dosya ve Yazıcı Paylaşımı** etkin değil. | İzin **dosya ve Yazıcı Paylaşımı** kaynak makinede Windows Güvenlik Duvarı'nda. Kaynak makinedeki altında **Windows Güvenlik Duvarı** > **bir uygulama veya özelliğin güvenlik duvarı üzerinden izin**seçin **dosya ve yazıcı paylaşımı tüm profiller için**. </br> Ayrıca, göndererek yükleme başarıyla tamamlamak için şu önkoşulları denetleyin.<br> Daha fazla bilgi edinin [sorunlarını giderme WMI](#troubleshoot-wmi-issues).


## <a name="error-95107---protection-could-not-be-enabled-ep0858"></a>Hata 95107 - koruma yüklenemedi (EP0858) etkin

**Hata kodu** | **Olası nedenler** | **Hata özel öneriler**
--- | --- | ---
95107 </br>**İleti:** anında iletme kaynak makine için Mobility hizmeti yüklemesi başarısız oldu, hata kodu ile **EP0858**. <br> Mobility hizmetini yüklemek için sağlanan kimlik bilgileri yanlış veya kullanıcı hesabı yetersiz ayrıcalıklara sahip. | Kaynak makinede Mobility hizmetini yükleme için sağlanan kullanıcı kimlik bilgileri hatalıdır. | Kaynak makinede yapılandırma sunucusu için sağlanan kullanıcı kimlik bilgilerinin doğru olduğundan emin olun. <br> Ekleme veya kullanıcı kimlik bilgilerini düzenlemek için yapılandırma sunucusuna gidin ve seçin **Cspsconfigtool** > **hesabını yönetme**. </br> Ayrıca, aşağıdakileri denetleyin [önkoşulları](vmware-azure-install-mobility-service.md#install-mobility-service-by-push-installation-from-azure-site-recovery) gönderim yüklemesi başarıyla tamamlanması.


## <a name="error-95117---protection-could-not-be-enabled-ep0865"></a>Hata 95117 - koruma yüklenemedi (EP0865) etkin

**Hata kodu** | **Olası nedenler** | **Hata özel öneriler**
--- | --- | ---
95117 </br>**İleti:** anında iletme kaynak makine için Mobility hizmeti yüklemesi başarısız oldu, hata kodu ile **EP0865**. <br> Kaynak makine çalışmıyor veya işlem sunucusu ve kaynak makine arasında ağ bağlantısı sorunları vardır. | Kaynak sunucu ile işlem sunucusu arasındaki ağ bağlantısı sorunları. | İşlem sunucusu ve kaynak sunucu arasındaki bağlantıyı denetleyin. </br> Ayrıca, aşağıdakileri denetleyin [önkoşulları](vmware-azure-install-mobility-service.md#install-mobility-service-by-push-installation-from-azure-site-recovery) gönderim yüklemesi başarıyla tamamlanması.|

## <a name="error-95103---protection-could-not-be-enabled-ep0854"></a>Hata 95103 - koruma yüklenemedi (EP0854) etkin

**Hata kodu** | **Olası nedenler** | **Hata özel öneriler**
--- | --- | ---
95103 </br>**İleti:** anında iletme kaynak makine için Mobility hizmeti yüklemesi başarısız oldu, hata kodu ile **EP0854**. <br> Windows Yönetim Araçları (WMI) kaynak makinede izin verilmiyor veya işlem sunucusu ve kaynak makine arasında ağ bağlantısı sorunları vardır.| WMI, Windows Güvenlik Duvarı'nda engellenir. | WMI, Windows Güvenlik Duvarı'nda izin verin. Altında **Windows Güvenlik Duvarı** > **bir uygulama veya özelliğin güvenlik duvarı üzerinden izin**seçin **tüm profiller için WMI**. </br> Ayrıca, aşağıdakileri denetleyin [önkoşulları](vmware-azure-install-mobility-service.md#install-mobility-service-by-push-installation-from-azure-site-recovery) gönderim yüklemesi başarıyla tamamlanması.|

## <a name="error-95213---protection-could-not-be-enabled-ep0874"></a>Hata 95213 - koruma yüklenemedi (EP0874) etkin

**Hata kodu** | **Olası nedenler** | **Hata özel öneriler**
--- | --- | ---
95213 </br>**İleti:** kaynağına Mobility hizmetinin % Sourceıp; makinesi; hata koduyla başarısız oldu **EP0874**. <br> | Kaynak makinedeki işletim sistemi sürümü desteklenmiyor. <br>| Kaynak makinenin işletim sistemi sürümü desteklenen emin olun. Okuma [destek matrisi](https://aka.ms/asr-os-support). </br> Ayrıca, aşağıdakileri denetleyin [önkoşulları](https://aka.ms/pushinstallerror) gönderim yüklemesi başarıyla tamamlanması.| 


## <a name="error-95108---protection-could-not-be-enabled-ep0859"></a>Hata 95108 - koruma yüklenemedi (EP0859) etkin

**Hata kodu** | **Olası nedenler** | **Hata özel öneriler**
--- | --- | ---
95108 </br>**İleti:** anında iletme kaynak makine için Mobility hizmeti yüklemesi başarısız oldu, hata kodu ile **EP0859**. <br>| Mobility hizmetini yüklemek için sağlanan kimlik bilgileri yanlış veya kullanıcı hesabı yetersiz ayrıcalıklara sahip. <br>| Sağlanan kimlik bilgileri olduğundan emin olun **kök** hesabının kimlik bilgileri. Ekleme veya kullanıcı kimlik bilgilerini düzenlemek için yapılandırma sunucusuna gidin ve seçin **Cspsconfigtool** Masaüstünde kısayol simgesi. Seçin **hesabını yönetme** ekleyin veya kimlik bilgilerini düzenlemek için.|

## <a name="error-95265---protection-could-not-be-enabled-ep0902"></a>Hata 95265 - koruma yüklenemedi (EP0902) etkin

**Hata kodu** | **Olası nedenler** | **Hata özel öneriler**
--- | --- | ---
95265 </br>**İleti:** Mobility hizmeti kaynak makineye başarılı oldu ancak kaynak makine için anında iletme yüklenmesini bazı sistem değişikliklerinin etkili olması için yeniden başlatılmasını gerektirir. <br>| Mobility hizmetinin eski bir sürümü sunucuda zaten yüklü.| Sanal makinenin çoğaltılması sorunsuz bir şekilde devam eder.<br> Sunucu, Mobility hizmetindeki yeni geliştirmelerden avantajlarını almak için sonraki bakım penceresi sırasında yeniden başlatın.|


## <a name="error-95224---protection-could-not-be-enabled-ep0883"></a>Hata 95224 - koruma yüklenemedi (EP0883) etkin

**Hata kodu** | **Olası nedenler** | **Hata özel öneriler**
--- | --- | ---
95224 </br>**İleti:** hata koduyla başarısız oldu; makinesi % Sourceıp; kaynak Mobility hizmetinin göndererek yüklenmesi **EP0883**. Bir önceki yükleme veya güncelleştirme sistemi yeniden başlatma beklemede.| Sistem, mobilite hizmetinin eski veya uyumsuz bir sürümü kaldırırken yeniden değildi.| Mobility hizmetinin sürümü yok sunucusunda var olduğundan emin olun. <br> Sunucuyu yeniden başlatın ve korumayı etkinleştirme işini yeniden çalıştırın.|

## <a name="resource-to-troubleshoot-push-installation-problems"></a>Anında iletme yükleme sorunlarını gidermek için kaynak

#### <a name="troubleshoot-file-and-print-sharing-issues"></a>Dosya sorun giderme ve yazdırma paylaşım sorunları
* [Etkinleştirmek veya devre dışı dosya paylaşımı ile Grup İlkesi](https://technet.microsoft.com/library/cc754359(v=ws.10).aspx)
* [Dosya ve yazıcı paylaşımı Windows Güvenlik Duvarı üzerinden etkinleştirme](https://technet.microsoft.com/library/ff633412(v=ws.10).aspx)

#### <a name="troubleshoot-wmi-issues"></a>WMI sorunlarını giderme
* [Temel WMI test etme](https://blogs.technet.microsoft.com/askperf/2007/06/22/basic-wmi-testing/)
* [WMI sorunlarını giderme](https://msdn.microsoft.com/library/aa394603(v=vs.85).aspx)
* [WMI komut dosyaları ve WMI hizmetleri ile ilgili sorunları giderme](https://technet.microsoft.com/library/ff406382.aspx#H22)

## <a name="next-steps"></a>Sonraki adımlar

[Bilgi nasıl](vmware-azure-tutorial.md) VMware Vm'leri için olağanüstü durum kurtarma ayarlama.