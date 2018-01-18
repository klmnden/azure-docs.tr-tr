---
title: "Azure Site Recovery VMware Azure'a sorunlarını giderme | Microsoft Docs"
description: "Azure sanal makineleri çoğaltırken hatalarında sorun giderme"
services: site-recovery
author: anoopkv
manager: gauravd
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.date: 01/11/2018
ms.author: anoopkv
ms.openlocfilehash: c5566ec44a8bfed0a3e7207c2cebf21517573541
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2018
---
# <a name="troubleshoot-mobility-service-push-installation-issues"></a>Mobilite hizmetinin göndermeli yükleme sorunlarını giderme

Bu makalede, Azure Site Recovery Mobility hizmeti korumasını etkinleştirmek için kaynak sunucuda yüklemeye çalıştığınızda yüz yaygın sorunları açıklar.

## <a name="error-78007---the-requested-operation-could-not-be-completed"></a>Hata 78007 - istenen işlem tamamlanamadı
Bu hata, hizmeti için birden çok nedenden dolayı tarafından oluşturulan. Daha fazla sorun giderme için ilgili sağlayıcı hatası seçin.

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
95105 </br>**İleti:** kaynak makinedeki mobilite hizmetinin göndermeli yüklemesi hata koduyla başarısız oldu **EP0856**. <br> Her iki **dosya ve Yazıcı Paylaşımı** ve kaynak makine veya var. olan ağ bağlantısı sorunları işlem sunucusu ve kaynak makine arasında izin verilmiyor.| **Dosya ve Yazıcı Paylaşımı** etkin değil. | İzin **dosya ve Yazıcı Paylaşımı** Windows Güvenlik Duvarı'nda Kaynak makinede. Kaynak makinede altında **Windows Güvenlik Duvarı** > **bir uygulama veya özelliğin güvenlik duvarı aracılığıyla izin**seçin **dosya ve yazıcı paylaşımı tüm profiller için**. </br> Ayrıca, yükleme başarıyla tamamlamak için aşağıdaki önkoşulları denetleyin.<br> Daha fazla bilgi edinin [WMI isssues sorun giderme](#troubleshoot-wmi-issues)


## <a name="error-95107---protection-could-not-be-enabled-ep0858"></a>Hata 95107 - koruma yüklenemedi (EP0858) etkin

**Hata kodu** | **Olası nedenler** | **Hata özel öneriler**
--- | --- | ---
95107 </br>**İleti:** kaynak makinedeki mobilite hizmetinin göndermeli yüklemesi hata koduyla başarısız oldu **EP0858**. <br> Mobilite hizmetinin yüklenmesi için sağlanan kimlik bilgileri yanlış veya kullanıcı hesabı yetersiz ayrıcalıklara sahip. | Kaynak makinede mobilite hizmetinin yüklenmesi için sağlanan kullanıcı kimlik bilgileri yanlış. | Yapılandırma sunucusundaki kaynak makine için sağlanan kullanıcı kimlik bilgilerinin doğru olduğundan emin olun. <br> Kullanıcı kimlik bilgileri Ekle/Düzenle için yapılandırma sunucusuna gidin ve seçin **Cspsconfigtool** > **hesabını yönetmek**. </br> Ayrıca, aşağıdakileri denetleyin [Önkoşullar](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) başarıyla yüklemeyi tamamlamak için.


## <a name="error-95117---protection-could-not-be-enabled-ep0865"></a>Hata 95117 - koruma yüklenemedi (EP0865) etkin

**Hata kodu** | **Olası nedenler** | **Hata özel öneriler**
--- | --- | ---
95117 </br>**İleti:** kaynak makinedeki mobilite hizmetinin göndermeli yüklemesi hata koduyla başarısız oldu **EP0865**. <br> Kaynak makine çalışmıyor veya işlem sunucusu ve kaynak makine arasında ağ bağlantısı sorunları vardır. | İşlem sunucusu ve kaynak sunucu arasındaki ağ bağlantısı sorunları. | İşlem sunucusu ve kaynak sunucu arasındaki bağlantıyı denetleyin. </br> Ayrıca, aşağıdakileri denetleyin [Önkoşullar](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) başarıyla yüklemeyi tamamlamak için.|

## <a name="error-95103---protection-could-not-be-enabled-ep0854"></a>Hata 95103 - koruma yüklenemedi (EP0854) etkin

**Hata kodu** | **Olası nedenler** | **Hata özel öneriler**
--- | --- | ---
95103 </br>**İleti:** kaynak makinedeki mobilite hizmetinin göndermeli yüklemesi hata koduyla başarısız oldu **EP0854**. <br> Windows Yönetim Araçları (WMI) kaynak makinede izin verilmeyen ya da işlem sunucusu ve kaynak makine arasında ağ bağlantısı sorunları vardır.| WMI Windows Güvenlik Duvarı'nda engellendi. | WMI Windows Güvenlik Duvarı'nda izin verin. Altında **Windows Güvenlik Duvarı** > **bir uygulama veya özelliğin güvenlik duvarı aracılığıyla izin**seçin **tüm profiller için WMI**. </br> Ayrıca, aşağıdakileri denetleyin [Önkoşullar](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) başarıyla yüklemeyi tamamlamak için.|

## <a name="error-95213---protection-could-not-be-enabled-ep0874"></a>Hata 95213 - koruma yüklenemedi (EP0874) etkin

**Hata kodu** | **Olası nedenler** | **Hata özel öneriler**
--- | --- | ---
95213 </br>**İleti:** mobility hizmetinin yüklenmesi kaynak makine % Sourceıp; hata koduyla başarısız oldu **EP0874**. <br> | Kaynak makinedeki işletim sistemi sürümü desteklenmiyor. <br>| Kaynak makine işletim sistemi sürümünün desteklendiğinden emin olun. Okuma [destek matrisi](https://aka.ms/asr-os-support). </br> Ayrıca, aşağıdakileri denetleyin [Önkoşullar](https://aka.ms/pushinstallerror) başarıyla yüklemeyi tamamlamak için.| 


## <a name="error-95108---protection-could-not-be-enabled-ep0859"></a>Hata 95108 - koruma yüklenemedi (EP0859) etkin

**Hata kodu** | **Olası nedenler** | **Hata özel öneriler**
--- | --- | ---
95108 </br>**İleti:** kaynak makinedeki mobilite hizmetinin göndermeli yüklemesi hata koduyla başarısız oldu **EP0859**. <br>| Mobilite hizmetinin yüklenmesi için sağlanan kimlik bilgileri yanlış veya kullanıcı hesabı yetersiz ayrıcalıklara sahip <br>| Sağlanan kimlik bilgileri olduğundan emin olun **kök** hesabının kimlik bilgileri. Kullanıcı kimlik bilgileri Ekle/Düzenle için yapılandırma sunucusuna gidin ve "Cspsconfigtool" Masaüstünde kısayol simgesini tıklatın. Tıklayın "Yönet hesabında" için kimlik bilgileri Ekle/Düzenle.|

## <a name="error-95265---protection-could-not-be-enabled-ep0902"></a>Hata 95265 - koruma yüklenemedi (EP0902) etkin

**Hata kodu** | **Olası nedenler** | **Hata özel öneriler**
--- | --- | ---
95265 </br>**İleti:** kaynak makinedeki mobilite hizmetinin göndermeli yüklemesi başarılı oldu ancak kaynak makine bazı sistem değişikliklerin etkili olması yeniden başlatma gerektirir. <br>| Mobility hizmeti daha eski bir sürümü sunucuda zaten yüklü.| Sanal makinenin çoğaltma sorunsuz bir şekilde devam eder.<br> Mobility hizmetinin yeni geliştirmeleri avantajlarından yararlanabilmek için sonraki bakım penceresi sırasında sunucuyu yeniden başlatın.|


## <a name="error-95224---protection-could-not-be-enabled-ep0883"></a>Hata 95224 - koruma yüklenemedi (EP0883) etkin

**Hata kodu** | **Olası nedenler** | **Hata özel öneriler**
--- | --- | ---
95224 </br>**İleti:** mobility hizmetinin yüklenmesi kaynak makine % Sourceıp; EP0883 hata koduyla başarısız oldu. Bir sistem yeniden başlatma önceki bir yükleme / güncelleştirme beklemede.| Sistem mobility hizmetinin eski/uyumsuz sürümü kaldırma yeniden başlatıldığından yok.| Mobility hizmeti sürümü olmadığı sunucusunda var olduğundan emin olun. <br> Sunucuyu yeniden başlatın ve koruma etkinleştirme işini yeniden çalıştırın|

## <a name="resource-to-troubleshoot-push-installation-problems"></a>Göndermeli yükleme sorunlarını gidermek için kaynak

#### <a name="troubleshoot-file-and-print-sharing-issues"></a>Dosya ve yazıcı paylaşımı sorunlarını giderme
*  [Etkinleştirmek veya Grup İlkesi ile dosya paylaşımı devre dışı bırakma](https://technet.microsoft.com/library/cc754359(v=ws.10).aspx)
* [Dosya etkinleştirme ve Windows Güvenlik Duvarı üzerinden paylaşımı yazdırma](https://technet.microsoft.com/library/ff633412(v=ws.10).aspx)

#### <a name="troubleshoot-wmi-issues"></a>WMI sorunlarını giderme
* [Temel WMI test etme](https://blogs.technet.microsoft.com/askperf/2007/06/22/basic-wmi-testing/)
* [WMI sorun giderme](https://msdn.microsoft.com/library/aa394603(v=vs.85).aspx)
* [WMI komut dosyaları ve WMI hizmetleri ile ilgili sorunları giderme](https://technet.microsoft.com/library/ff406382.aspx#H22)

## <a name="next-steps"></a>Sonraki adımlar

[Bilgi nasıl](tutorial-vmware-to-azure.md) VMware Vm'leri için olağanüstü durum kurtarma ayarlamak için.