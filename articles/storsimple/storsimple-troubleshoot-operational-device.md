---
title: Dağıtılan bir StorSimple cihazınızın sorunlarını | Microsoft Docs
description: Tanılama ve şu anda dağıtılmış ve çalışır durumda olan bir StorSimple cihazında oluşan hataları düzeltme açıklar.
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
ms.openlocfilehash: 8ad3f09bf46caf426b2008b583ebd2ff78522462
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60302526"
---
# <a name="troubleshoot-an-operational-storsimple-device"></a>İşletimsel bir StorSimple cihazı sorunlarını giderme
> [!NOTE]
> StorSimple için klasik portal kullanım dışıdır. StorSimple Cihaz Yöneticileriniz, yeni Azure portalına kullanımdan kaldırma zamanlamasına göre otomatik olarak taşınacaktır. Bu taşımayla ilgili bir e-posta ve portal bildirimi alacaksınız. Bu belge de yakında kullanımdan kaldırılacaktır. Taşıma hakkında tüm sorularınız için bkz. [SSS: Azure portalına taşıma](storsimple-8000-move-azure-portal-faq.md).

## <a name="overview"></a>Genel Bakış
Bu makalede yapılandırma sorunlarını çözmek için yararlı sorun giderme kılavuzu sağlar. StorSimple Cihazınızı dağıtılmış ve çalışır duruma geldikten sonra karşılaşabilirsiniz. Bu, Microsoft Azure StorSimple'ı çalıştırdığınızda, karşılaşabileceğiniz sorunları gidermenize yardımcı olacak yaygın sorunlar, olası nedenler ve önerilen adımlar açıklanır. Bu bilgiler, şirket içi StorSimple fiziksel cihazı ve StorSimple sanal cihazı için geçerlidir.

Microsoft Azure StorSimple işlemi sırasında karşılaşabileceğiniz hata kodlarının listesini bulabilirsiniz. Bu makalenin sonunda, yanı sıra, hataları gidermek için adımları uygulayabilirsiniz. 

## <a name="setup-wizard-process-for-operational-devices"></a>İşletimsel cihazları için Kurulum Sihirbazı işlemi
Kurulum Sihirbazı'nı ([Invoke-hcssetupwizard'ı][1]) cihaz yapılandırmasını denetleyin ve gerekirse düzeltici eylemi gerçekleştirmek için.

Önceden yapılandırılmış ve işletimsel cihazda Kurulum Sihirbazı'nı çalıştırdığınızda, işlem akışını farklıdır. Yalnızca aşağıdaki girişlerini değiştirebilirsiniz:

* IP adresi, alt ağ maskesi ve ağ geçidi
* Birincil DNS sunucusu
* Birincil NTP sunucusu
* İsteğe bağlı web proxy yapılandırması

Kurulum Sihirbazı'nı, parola toplama ve cihaz kaydı için ilgili işlemleri gerçekleştirmez.

## <a name="errors-that-occur-during-subsequent-runs-of-the-setup-wizard"></a>Kurulum Sihirbazı'nın sonraki çalışmaları sırasında oluşan hataları
Hataları programını çalıştırdığınızda Kurulum Sihirbazı işlemsel bir cihazda, hataların olası nedenleri karşılaşabilirsiniz ve önerilen eylemler bunları çözmek için aşağıdaki tabloda açıklanmaktadır. 

| Hayır. | Hata iletisi veya koşul | Olası nedenler | Önerilen eylem |
|:--- |:--- |:--- |:--- |
| 1 |350032. hata: Bu cihaz zaten devre dışı bırakıldı. |Devre dışı bir cihazda Kurulum sihirbazını çalıştırırsanız, bu hatayı görürsünüz. |Sonraki adımlar için [Microsoft Desteği](storsimple-contact-microsoft-support.md)'ne başvurun. Devre dışı bırakılmış bir cihaz hizmetinde konulamaz. Fabrika sıfırlaması, cihaz yeniden etkinleştirilmeden önce gerekli olabilir. |
| 2 |Invoke-hcssetupwizard'ı: ERROR_INVALID_FUNCTION (HRESULT özel durum: 0x80070001) |DNS sunucusu güncelleştirmesi başarısız oluyor. DNS ayarları genel ayarlardır ve tüm etkin ağ arabirimleri arasında uygulanır. |Arabirimi etkinleştirmek ve DNS ayarlarını yeniden uygulayın. Bu ayarlar geneldir çünkü bu diğer etkin arabirimleri için ağ bozabilir. |
| 3 |Cihaz StorSimple Yöneticisi hizmeti Portalı'nda çevrimiçi görünüyor, ancak en az Kurulumu tamamlayın ve yapılandırmayı kaydetmek denediğinizde işlem başarısız olur. |Oluştu rağmen gerçek bir proxy sunucu yerinde ilk kurulum sırasında web Ara sunucusu, yapılandırılmamış. |Kullanım [Test-HcsmConnection cmdlet'ini] [ 2] hata bulunacak. [Microsoft Support başvurun](storsimple-contact-microsoft-support.md) sorunu çözmek mümkün değilse. |
| 4 |Invoke-hcssetupwizard'ı: Değer beklenen aralıkta değil. |Yanlış alt ağ maskesi, bu hatayı üretir. Olası nedenler şunlardır: <ul><li> Alt ağ maskesi eksik veya boş.</li><li>IPv6 ön eki biçimi doğru değil.</li><li>Arabirim bulut etkindir, ancak eksik veya hatalı ağ geçidi.</li></ul>DATA 0 otomatik olarak bulut Kurulum Sihirbazı'nı yapılandırdıysanız etkin olduğunu unutmayın. |Sorunu tanılamak için 0.0.0.0 veya 256.256.256.256 alt ağ kullanın ve sonra çıktıyı bakın. Doğru değerleri gerektiği gibi alt ağ maskesi, ağ geçidi ve IPv6 öneki girin. |

## <a name="error-codes"></a>Hata kodları
Hataları sayısal sırada listelenir.

| Hata sayısı | Hata metni veya açıklaması | Önerilen bir kullanıcı eylemi |
|:--- |:--- |:--- |
| 10502 |Depolama hesabınıza erişirken bir hatayla karşılaşıldı. |Birkaç dakika bekleyin ve işlemi yeniden deneyin. Hata devam ederse Microsoft Destek'e başvur sonraki adımlar için lütfen. |
| 40017 |Yedekleme ilkesinde belirtilen birim, cihazda bulunamadığından yedekleme işlemi başarısız oldu. |Yedeklemeyi yeniden deneyin. sorun devam ederse, işlemi Microsoft Support başvurun. Sonraki adımlar için. |
| 40018 |Yedekleme ilkesinde belirtilen birimlerden hiçbiri cihazda bulunamadığı yedekleme işlemi başarısız oldu. |Yedeklemeyi yeniden deneyin. sorun devam ederse, işlemi Microsoft Support başvurun. Sonraki adımlar için. |
| 390061 |Sistem meşgul veya kullanılamaz durumda. |Birkaç dakika bekleyin ve işlemi yeniden deneyin. Hata devam ederse Microsoft Destek'e başvur sonraki adımlar için lütfen. |
| 390143 |390143 hata koduyla bir hata oluştu. (Bilinmeyen hata.) |Lütfen sorun devam ederse, Microsoft Support için sonraki adımlara başvurun. |

## <a name="next-steps"></a>Sonraki adımlar
Sorunu çözmek mümkün değilse [Microsoft Support başvurun](storsimple-contact-microsoft-support.md) Yardım almak için. 

[1]: https://technet.microsoft.com/%5Clibrary/Dn688135(v=WPS.630).aspx
[2]: https://technet.microsoft.com/%5Clibrary/Dn715782(v=WPS.630).aspx
