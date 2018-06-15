---
title: Dağıtılan bir StorSimple cihazı sorunlarını giderme | Microsoft Docs
description: Tanılama ve şu anda dağıtılmış ve çalışır durumda bir StorSimple cihazında oluşan hataları düzeltme açıklar.
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: ''
ms.assetid: ea5d89ae-e379-423f-b68b-53785941d9d0
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/03/2017
ms.author: v-sharos
ms.openlocfilehash: cf037f7f1c1384b654a7144485d38f569eb7c167
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32187343"
---
# <a name="troubleshoot-an-operational-storsimple-device"></a>İşletimsel bir StorSimple cihazı sorun giderme
> [!NOTE]
> StorSimple için klasik portal kullanım dışıdır. StorSimple Cihaz Yöneticileriniz, yeni Azure portalına kullanımdan kaldırma zamanlamasına göre otomatik olarak taşınacaktır. Bu taşımayla ilgili bir e-posta ve portal bildirimi alacaksınız. Bu belge de yakında kullanımdan kaldırılacaktır. Taşıma hakkında tüm sorularınız için bkz. [SSS: Azure portalına taşıma](storsimple-8000-move-azure-portal-faq.md).

## <a name="overview"></a>Genel Bakış
Bu makalede yapılandırma sorunlarını çözmek için yararlı sorun giderme kılavuzu verilmiştir StorSimple Cihazınızı dağıtılmış ve çalışır durumda sonra hatalarla karşılaşabilirsiniz. Sık karşılaşılan sorunları, olası nedenler ve önerilen adımları, Microsoft Azure StorSimple çalıştırdığınızda karşılaşabileceğiniz sorunları gidermenize yardımcı olması için açıklar. Bu bilgiler, StorSimple şirket içi fiziksel cihazı ve StorSimple sanal cihazı için geçerlidir.

Microsoft Azure StorSimple işlemi sırasında karşılaşabileceğiniz hata kodları listesini bulabilirsiniz bu makalenin sonunda yanı sıra, hataları gidermek için adımları uygulayabilirsiniz. 

## <a name="setup-wizard-process-for-operational-devices"></a>Kurulum Sihirbazı işlemi işletimsel cihazlar için
Kurulum Sihirbazı'nı ([Invoke-HcsSetupWizard][1]) aygıt yapılandırmasını denetleyin ve gerekirse düzeltici işlemleri için.

Önceden yapılandırılmış ve işletimsel cihazda Kurulum Sihirbazı'nı çalıştırdığınızda, işlem akışı farklıdır. Yalnızca aşağıdaki girişler değiştirebilirsiniz:

* IP adresi, alt ağ maskesi ve ağ geçidi
* Birincil DNS sunucusu
* Birincil NTP sunucusu
* İsteğe bağlı web proxy yapılandırması

Kurulum Sihirbazı'nı, parola toplama ve aygıt kaydı için ilgili işlemleri gerçekleştirmez.

## <a name="errors-that-occur-during-subsequent-runs-of-the-setup-wizard"></a>Kurulum Sihirbazı'nın sonraki çalışmaları sırasında oluşan hataları
Aşağıdaki tabloda çalıştırdığınızda Kurulum Sihirbazı'nı işletimsel cihazında, hataların olası nedenleri karşılaşabileceğiniz ve önerilen eylemleri bunları gidermek için hatalarının açıklanmaktadır. 

| Hayır. | Hata iletisi veya koşul | Olası nedenler | Önerilen eylem |
|:--- |:--- |:--- |:--- |
| 1 |Hata 350032: Bu aygıt zaten devre dışı bırakıldı. |Kurulum Sihirbazı'nı devre dışı bir aygıtta çalıştırırsanız, bu hatayı görürsünüz. |[Microsoft Support başvurun](storsimple-contact-microsoft-support.md) sonraki adımlar için. Devre dışı bırakılmış bir aygıt hizmetinde konumuna geçiremezsiniz. Cihaz yeniden etkinleştirilmeden önce bir Fabrika sıfırlaması gerekebilir. |
| 2 |Invoke-HcsSetupWizard: ERROR_INVALID_FUNCTION (HRESULT özel durumu: 0x80070001) |DNS sunucusu güncelleştirmesi başarısız oluyor. DNS ayarları genel ayarları ve tüm etkin ağ arabirimler üzerinden uygulanır. |Arabirimi etkinleştirmek ve DNS ayarlarını yeniden uygulayın. Bu ayarlar geneldir çünkü bu diğer etkin arabirimleri için ağ bozabilir. |
| 3 |Cihaz StorSimple Yöneticisi hizmet Portalı'nda çevrimiçi olarak görünür, ancak minimum kurulumunu tamamlayın ve yapılandırmayı kaydetmek çalıştığınızda, işlem başarısız olur. |Oluştu olsa bile bir gerçek proxy sunucusu yerinde ilk kurulum sırasında web proxy, yapılandırılmadı. |Kullanım [Test HcsmConnection cmdlet] [ 2] hata bulunamadı. [Microsoft Support başvurun](storsimple-contact-microsoft-support.md) sorunu gidermesi ise. |
| 4 |Invoke-HcsSetupWizard: Değer beklenen aralıkta değil. |Hatalı alt ağ maskesi, bu hata üretir. Olası nedenler şunlardır: <ul><li> Alt ağ maskesi eksik veya boş olamaz.</li><li>IPv6 öneki biçimi doğru değil.</li><li>Arabirimi bulut etkindir, ancak ağ geçidi eksik veya yanlış.</li></ul>Veri 0-Kurulumu Sihirbazı aracılığıyla yapılandırdıysanız, bulut otomatik olarak etkin olduğunu unutmayın. |Sorunu belirlemek için alt ağ 0.0.0.0 veya 256.256.256.256 kullanın ve ardından çıktıyı bakın. Doğru değerleri gerektiği gibi alt ağ maskesi, ağ geçidi ve IPv6 öneki girin. |

## <a name="error-codes"></a>Hata kodları
Hataları sayısal sırada listelenir.

| Hata sayısı | Hata metni ya da açıklaması | Önerilen kullanıcı eylemi |
|:--- |:--- |:--- |
| 10502 |Depolama hesabınız erişirken hatayla karşılaşıldı. |Birkaç dakika bekleyin ve yeniden deneyin. Sorun devam ederse, Microsoft Destek'e başvurun sonraki adımlar için lütfen. |
| 40017 |Yedekleme İlkesi'nde belirtilen bir birim cihazda bulunamadı gibi yedekleme işlemi başarısız oldu. |Yedeklemeyi yeniden deneyin, sorun devam ederse, işlem Microsoft Support başvurun. Sonraki adımlar için. |
| 40018 |Yedekleme İlkesi'nde belirtilen birimlerden hiçbiri cihazda bulundu olarak yedekleme işlemi başarısız oldu. |Yedeklemeyi yeniden deneyin, sorun devam ederse, işlem Microsoft Support başvurun. Sonraki adımlar için. |
| 390061 |Sistem meşgul veya kullanılamaz durumda. |Birkaç dakika bekleyin ve yeniden deneyin. Sorun devam ederse, Microsoft Destek'e başvurun sonraki adımlar için lütfen. |
| 390143 |Hata kodu 390143 ile bir hata oluştu. (Bilinmeyen hata.) |Sorun devam ederse, Microsoft Support sonraki adımlar için temasa geçin. |

## <a name="next-steps"></a>Sonraki adımlar
Sorunu gidermek erişemiyorsanız [Microsoft Support başvurun](storsimple-contact-microsoft-support.md) Yardım için. 

[1]: https://technet.microsoft.com/%5Clibrary/Dn688135(v=WPS.630).aspx
[2]: https://technet.microsoft.com/%5Clibrary/Dn715782(v=WPS.630).aspx
