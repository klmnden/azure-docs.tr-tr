---
title: Azure Stack veri merkezi tümleştirmesi Kılavuzu | Microsoft Docs
description: Şirket içi Azure Stack dağıtımı başarılı, veri merkezinizdeki oluşabilecek öğrenin.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/10/2018
ms.author: jeffgilb
ms.reviewer: asganesh
ms.lastreviewed: 12/10/2018
ms.openlocfilehash: f900fa5105f42dac57b392d41a8cd888850fc648
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55249504"
---
# <a name="azure-stack-datacenter-integration"></a>Azure Stack veri merkezi tümleştirmesi

Bu makalede, bir çözüm sağlayıcısı tarafından başarılı bir şirket içi dağıtım için tümleşik bir çözüm aracılığıyla satın alma, uçtan uca Azure Stack müşteri deneyim açıklanmaktadır. Yolculuğunuza kolaylaştırmak ve beklentileri, Azure Stack müşterisi olarak, Azure Stack ile veri merkezinizi tümleştirdiğinizde beklediğiniz üzerinde ayarlamanıza yardımcı olması için bu bilgileri kullanın.

Azure Stack müşterisi olarak, aşağıdaki veri merkezi tümleştirmesi aşamaları tahmin etmeleri gereken:

|     |Planlama aşamasında|Sipariş işlemi|Dağıtım öncesi|Fabrika işlem|Donanım teslim|Yerinde dağıtım|
|-----|-----|-----|-----|-----|-----|-----|
|**Microsoft**|İş ortağıyla iletişim kurma satış öncesi destek sağlamak için.|Yazılım Lisansı ve gerektiğinde sözleşmeleri hazırlayın.|Veri Merkezi Tümleştirme gereksinimlerini ve belgeler için müşteri toplamak için gerekli araçları sağlar.|En son temel derlemeler ve araç zinciri güncelleştirmeleri, aylık bir tempoyla sunar.|Yok|Microsoft destek mühendisleri dağıtım sorunları yardımcı olur.|
|**İş ortağı**|Çözüm seçenekleri müşteri gereksinimlerine göre önerilir.<br><br>Kavram kanıtı (POC) gerekirse önerin.<br><br>İş ilişki kurun.<br><br>Destek düzeyine karar verin.|Gerekli sözleşmeler müşteri ile hazırlayın.<br><br>Müşteri satınalma siparişi oluşturun.<br><br>Teslim zaman çizelgesi üzerinde karar verin.<br><br>Gerekirse, müşteri ile Microsoft bağlanın.|Müşteri, gerekli tüm dağıtım önkoşulları ve veri merkezi anlayış tümleştirme seçenekleri emin olmak eğitim ile sağlar.<br><br>Bütünlük ve doğruluğundan emin olmak için toplanan verilerin doğrulama müşteriye yardımcı olun.|Son doğrulanmış ana hat derlemesi için geçerlidir.<br><br>Gerekli Microsoft Dağıtım Araç Seti uygulayın.|Müşteri sitesine donanım gönderin.|Dağıtım bir yerinde destek mühendisi tarafından işlenir.<br><br>Raf ve yığın.<br><br>Donanım yaşam döngüsü (HLH) konak dağıtımı.<br><br>Azure Stack dağıtımı.<br><br>Müşteri için elle devre dışı.|
|**Müşteri**|Hedeflenen kullanım durumları açıklayın ve gereksinimleri belirtin.|Faturalandırma modeli kullanın, gözden geçirmek ve sözleşmeleri onaylamak için belirleyin.|Dağıtım çalışma sayfası tamamlayın ve tüm dağıtım önkoşulları met ve dağıtım için hazır olduğundan emin olun.|Yok|Veri Merkezi, tüm gerekli güç sağlayarak hazırlamak ve soğutma kenarlık bağlantı ve diğer gerekli veri merkezi tümleştirmesi gereksinim yerdir.|Abonelik kimlik bilgilerini sağlayın ve sağlanan veriler hakkında sorular varsa desteklemek için dağıtım sırasında kullanılabilir.|
| | | | | | | |


## <a name="planning-phase"></a>Planlama aşamasında
Planlama aşamasında, Microsoft veya Azure Stack çözüm iş ortağı, değerlendirmek ve Azure Stack sizin için doğru çözümü olup olmadığını belirlemek için ihtiyaçlarınızı anlayabilmemiz için birlikte çalışırız durumdur:

Bunlar, aşağıdakilere karar vermenize yardımcı olur:

-   Azure Stack, kuruluşunuz için doğru çözümdür?

-   Boyutu çözümün ne ihtiyacınız olacak?

-   Kuruluşunuz için faturalama ve lisans modeli hangi türde çalışır mı?

-   Güç ve soğutma gereksinimleri nelerdir?

Donanım çözümü gereksinimlerinizi en iyi sığmasını sağlamak için [Azure Stack Capacity Planner](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) uygun kapasiteyi ve yapılandırma için Azure Stack donanımı belirlemek planlama ön satın alma konusunda yardımcı olmak için kullanılabilir Çözüm.

Elektronik tablo *değil* yerine kendi araştırma ve en iyi ihtiyaçlarınıza uygun analiz donanım çözümleri için kullanılmak üzere. Bir Azure Stack dağıtımı için planlama yaparken de gözden geçirmelisiniz [genel veri merkezi tümleştirme konuları](azure-stack-datacenter-integration.md) için Azure Stack tümleşik sistemleri.

## <a name="order-process-phase"></a>Siparişi işlem aşaması
Bu aşamada, sorularınızı Uygulanabilirlik bakımından birçoğu cevaplanıp. Azure Stack satın alma ve gerekli tüm anlaşmalar oturum açtıktan sonra tamamlama ve satın alma siparişleri hazır, çözümü sağlayıcınız Tümleştirme gereksinimlerini veri sağlamak için istenecektir.

## <a name="pre-deployment-phase"></a>Dağıtım öncesi aşaması
Bu aşamada, nasıl Azure Stack ile veri merkezinizi tümleştirmek istediğinize karar verirken gerekecektir. Bu işlemi kolaylaştırmak için Microsoft, çözüm sağlayıcıları ile işbirliği içinde çalışan bir gereksinimleri şablonu ortamınızdaki bir tümleşik sistemi dağıtımını planlama için gereken bilgileri toplamanıza yardımcı olması için bir araya.

[Genel veri merkezi tümleştirme konuları](azure-stack-datacenter-integration.md) makale şablonun, dağıtım çalışma sayfası bilinen tamamlamanıza yardımcı olacak bilgileri sağlar. 

> [!IMPORTANT]
> Bu aşamada tüm önkoşul bilgilerini araştırılması ve çözüm sıralama önce karar önemlidir. Bu adımı zaman alıcıdır ve işbirliği ve kuruluşunuzdaki birden çok Disipline toplamayı veri gerektirir unutmayın. 

Dağıtım öncesi aşamasında aşağıdakilere karar vermeniz gerekir:

- **Azure Stack bağlantı modeli ve kimlik sağlayıcısı**. Azure Stack ya da dağıtmak için seçebileceğiniz [İnternet'e (Azure) bağlı veya bağlantısı kesilmiş](azure-stack-connection-models.md). Azure yığını karma senaryoları da dahil olmak üzere, en çok faydayı alın, dağıtılacak Azure'a bağlı isteyebilirsiniz. Active Directory Federasyon Hizmetleri (AD FS) veya Azure Active Directory (Azure AD) seçme dağıtım sırasında yapmanız gereken tek seferlik bir karardır. **Bunu daha sonra tüm sistemin yeniden dağıtmadan değiştiremezsiniz**.

- **Lisanslama modeli**. Lisanslama modeli seçenekler arasından seçim yapabileceğiniz, olması dağıtım türüne bağlıdır. Kimlik sağlayıcısı seçtiğiniz Kiracı sanal makineleri ve kimlik sistemi ve kullandıkları hesapları ilgisi yoktur.
    - Bulunan müşteriler bir [bağlantısı kesildi dağıtım](azure-stack-disconnected-deployment.md) tek seçeneğiniz vardır: kapasite kullanıma dayalı faturalandırma.

    - Bulunan müşteriler bir [bağlı dağıtım](azure-stack-connected-deployment.md) kapasitesi kullanıma dayalı faturalandırma ve,-kullandıkça arasından seçim yapabilirsiniz. Faturalandırma kapasite tabanlı kayıt için bir Kurumsal Anlaşma (EA) Azure aboneliği gerektirir. Bu öğeleri bir Azure aboneliği aracılığıyla Market'te kullanılabilirliğini sağlayan bir kayıt için gereklidir.

- **Ağ tümleştirmesi**. [Ağ tümleştirmesi](azure-stack-network.md) dağıtım işlemi ve Azure Stack sistemleri yönetimi için önemlidir. Azure Stack çözüm dayanıklı ve kendi işlemlerini desteklemek için yüksek oranda kullanılabilir bir fiziksel altyapı sağlamaya uygulamasına Git birkaç nokta vardır.

- **Güvenlik Duvarı tümleştirmesi**. Bu önerilir, [bir güvenlik duvarı kullanın](azure-stack-firewall.md) güvenli Azure Stack yardımcı olmak için. Güvenlik Duvarı, DDOS saldırıları, izinsiz giriş algılama ve içerik İnceleme engellemeye yardımcı olabilir. Ancak, Azure depolama hizmetleri için bir aktarım hızı ayak dönüşebilir unutulmamalıdır.


- **Sertifika gereksinimleri**. Kritik, tüm [gerekli sertifikaları](azure-stack-pki-certs.md) kullanılabilir *önceki* dağıtımı için veri merkezinde ulaşan bir yerinde mühendis için.

Dağıtım çalışma sayfası tüm önkoşul bilgilerin toplandıktan sonra çözüm sağlayıcısının kapatma başarılı bir Azure Stack bütünleşme veri Merkezinize emin olmak için toplanan verileri temel alan Fabrika işlemini başlatır.

## <a name="hardware-delivery-phase"></a>Donanım teslim aşaması
Çözüm, bir birime geldiğinde planlama üzerinde çözümü sağlayıcınız sizinle birlikte çalışır. Alınan ve yerleştirdiniz sonra Azure Stack dağıtımı yapmak için yerinde gelen mühendisimizden çözümü sağlayıcısı ile zamanlama gerekecektir.

Bu **önemli** tüm önkoşul veri kilitli ve kullanılabilir *çözümü dağıtmak için önce yerinde mühendisi ulaşan*.

-   Tüm sertifikaları satın alınmalıdır ve hazır.

-   Etki alanı adı üzerinde karar gerekir.

-   Tüm ağ tümleştirme parametrelerini sonlandırıldıktan ve çözümü sağlayıcınız ile paylaşılan ile aynı.

> [!TIP]
> Bu bilgileri değiştiyse, gerçek bir dağıtım zamanlamak önce değişikliği çözüm sağlayıcıyla iletişim kurmak emin olun.

## <a name="onsite-deployment-phase"></a>Yerinde dağıtım aşaması
Azure Stack dağıtmak için bir yerinde mühendis donanım çözüm sağlayıcınızdan dağıtımı devre dışı başlatmasını mevcut olması gerekir. Başarılı bir dağıtım sağlamak için dağıtım çalışma sayfası sunulan tüm bilgileri değiştirilmedi emin olun. 

Yerinde destek mühendisinden dağıtım deneyimi sırasında beklediğiniz verilmiştir:

- Çözüm, düzgün bir şekilde bir araya getirildiği ve gereksinimlerinizi karşıladığından emin olmak için tüm kablo ve kenarlık bağlantısını denetleyin
- ' % S'çözüm HLH (donanım yaşam döngüsü ana bilgisayarı) yapılandırma
- Tüm BMC, BIOS ve ağ ayarlarının doğru olduğundan emin olun.
- Tüm bileşenlerin bellenim çözüm tarafından onaylanmış son sürümde olduğundan emin olun
- Dağıtımı Başlat

> [!NOTE]
> Yerinde destek mühendisi tarafından bir dağıtım yordamı tamamlamak için bir iş hafta sürebilir.

## <a name="post-integration-phase"></a>Sonrası tümleştirme aşaması
Çözümü Kapat müşteriye sonrası tümleştirme aşamasında devredildiği önce birkaç adım iş ortağı tarafından gerçekleştirilmesi gerekir. Bu aşamada doğrulama doğru sistem dağıtıldığından emin olmak önemli ve gerçekleştiriliyor. 

OEM iş ortağı tarafından gerçekleştirilen eylemler şunlardır:

-   [Test-azurestack çalıştırın](azure-stack-diagnostic-test.md#run-validation-tool-to-test-system-readiness-before-installing-update-or-hotfix)

-   [Azure ile kayıt](azure-stack-registration.md)

-   [Market Sendikasyonu](azure-stack-download-azure-marketplace-item.md#use-the-marketplace-syndication-tool-to-download-marketplace-items)

-   Yedek anahtar yapılandırma dosyaları

-   DVM Kaldır

-   Özet bir müşteri dağıtımı için hazırlama

-   [Çözüm yazılımın en son sürüme güncelleştirildiğinden emin olmak için Güncelleştirmeleri denetle](azure-stack-updates.md)

Gerekli veya isteğe bağlı yükleme türüne bağlı olarak birkaç adım vardır.

-   Dağıtım kullanarak tamamladıysanız [AD FS](azure-stack-integrate-identity.md), ardından kendi AD FS kullanıcının Azure Stack damga müşteri ile tümleşik gerekir.

  > [!NOTE]
  > Bunu yapmak için hizmet sunmak iş ortağı isteğe bağlı olarak tercih edebilirsiniz ancak bu adım, müşterinin sorumluluğundadır.

-   İlgili iş ortağı var olan izleme sisteminden ile tümleştirme.

    -   [System Center Operations Manager tümleştirmesini](azure-stack-integrate-monitor.md) Filo yönetimi özelliklerini de destekler.

    -   [Nagios tümleştirme](azure-stack-integrate-monitor.md#integrate-with-nagios)

## <a name="overall-timeline"></a>Genel zaman çizelgesi

![](./media/azure-stack-datacenter-integration-walkthrough/image1.png)

## <a name="support"></a>Destek
Azure Stack, bir Azure ile tutarlı, sistem yaşam döngüsünün tamamını kapsayan tümleşik destek deneyimi sağlar. Azure Stack tümleşik sistemleri tam olarak desteklemek için iki destek sözleşmeleri müşterilerin gerekir; bir Azure Hizmetleri İkincisiyse sistem desteği için donanım sağlayıcısı için Microsoft (veya kendi bulut çözümü sağlayıcısı). Tümleşik destek deneyimi koordine eskalasyon ve çözüm sağlar, böylece müşteriler kim bunlar çağırın ne olursa olsun bir tutarlı bir destek deneyimi yaşarsınız. Premier, zaten sahip müşteriler için Azure - standart / ProDirect veya iş ortağı destek Microsoft Azure Stack yazılım desteği dahildir.

Tümleşik destek deneyimi bir durum değişimi yararlanır Microsoft Donanım iş ortağı arasındaki çift yönlü aktarımı desteği durumları ve durum güncelleştirmeleri için bir mekanizma. Microsoft Azure Stack izleyecek [Modern yaşam döngüsü ilkesi](https://support.microsoft.com/help/30881).

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinin [genel veri merkezi tümleştirme konuları](azure-stack-datacenter-integration.md).
