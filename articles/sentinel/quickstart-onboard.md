---
title: Yerleşik Azure Sentinel Önizleme | Microsoft Docs
description: Azure Gözcü içinde verilerini nasıl toplayacağınızı öğrenin.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: d5750b3e-bfbd-4fa0-b888-ebfab7d9c9ae
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/27/2019
ms.author: rkarlin
ms.openlocfilehash: 891f9fbd26b53b392ac84ed9d420b58558cd20c2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66258428"
---
# <a name="on-board-azure-sentinel-preview"></a>Yerleşik Azure Sentinel Önizleme

> [!IMPORTANT]
> Azure Sentinel şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bu hızlı başlangıçta, öğreneceksiniz yerleşik Azure Gözcü öğreneceksiniz. 

Yerleşik Azure Gözcü önce Azure Gözcü etkinleştirin ve ardından veri kaynaklarınıza bağlanmak gerekir. Azure Sentinel Microsoft çözümleri, kutunun ve Microsoft tehdit koruması çözümleri, Office 365, Azure AD, Azure ATP dahil olmak üzere, Microsoft 365 kaynakları dahil olmak üzere, gerçek zamanlı tümleştirme sağlayan dışında kullanılabilir bağlayıcılar sayısı ile birlikte sunulur ve Microsoft Cloud App Security ve daha fazlası. Ayrıca, Microsoft olmayan çözümler için daha geniş güvenlik ekosistemine yerleşik bağlayıcılar vardır. Ayrıca ortak olay biçimi, veri kaynaklarınızı Azure Gözcü ile bağlanmak için Syslog veya REST API de kullanabilirsiniz.  

Veri kaynaklarınızı bağlandıktan sonra verilerinizi temel alan ınsights yüzey ustalıkla oluşturulan panoları galerisinden seçin. Bu pano, gereksinimleriniz için kolayca özelleştirilebilir.


## <a name="global-prerequisites"></a>Genel Önkoşullar

- Etkin Azure, yoksa, aboneliği, oluşturun bir [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) başlamadan önce.

- Log Analytics çalışma alanı. Bilgi edinmek için nasıl [Log Analytics çalışma alanı oluşturma](../log-analytics/log-analytics-quick-create-workspace.md)

-  Azure Gözcü etkinleştirmek için Azure Gözcü çalışma alanının bulunduğu aboneliğe katkıda bulunan izinleri gerekir. 
- Azure Gözcü kullanmak için çalışma alanının ait olduğu kaynak grubu üzerinde katkıda bulunan veya Okuyucu izinleri gerekir.
- Ek izinler, belirli veri kaynaklarına bağlanmak için gerekli
 
## Azure Sentinel etkinleştir <a name="enable"></a>

1. Azure portalına gidin.
2. Azure Gözcü oluşturulduğu, aboneliğin seçildiğinden emin olun. 
3. Azure Sentinel arayın. 
   ![Arama](./media/quickstart-onboard/search-product.png)

1. Tıklayın **+ Ekle**.
1. Kullanma veya yeni bir tane oluşturmak istediğiniz çalışma alanını seçin. Birden fazla çalışma alanına Azure Gözcü çalıştırabilirsiniz, ancak veriler, tek bir çalışma alanına yalıtılır.

   ![search](./media/quickstart-onboard/choose-workspace.png)

   >[!NOTE] 
   > - **Çalışma alanı konumu** akışını Azure Gözcü için tüm veriler, seçili çalışma alanının coğrafi konumda depolanır anlamak önemlidir.  
   > - Azure Güvenlik Merkezi tarafından oluşturulan varsayılan çalışma alanları listesinde görünmez; Azure Gözcü üzerlerinde yükleyemezsiniz.
   > - Azure Sentinel aşağıdaki bölgelerden hiçbirinde dağıtılan çalışma alanları çalıştırabilirsiniz:  Avustralya Güneydoğu, Kanada Orta, Orta Hindistan, Doğu ABD, Doğu ABD 2 EUAP (Kanarya), Doğu, Güneydoğu Asya, UK Güney, Batı Avrupa, Batı ABD 2 Japonya.

6. Tıklayın **Azure Sentinel ekleme**.
  

## <a name="connect-data-sources"></a>Veri kaynaklarını bağlama

Azure Sentinel hizmetine ve olayları ve günlükleri Azure Gözcü için iletme hizmetlerini ve uygulamalarını bağlantısı oluşturur. Makineler ve sanal makineler için günlükleri toplar ve bunları Azure Gözcü için ileten Azure Gözcü aracıyı yükleyebilirsiniz. Güvenlik duvarları ve proxy'ler için Azure Gözcü bir Linux Syslog sunucusu kullanır. Aracının yüklü ve hangi aracının topladığı günlük dosyaları ve bunları Azure Gözcü için iletir. 
 
1. Tıklayın **veri toplama**.
2. Bağlanmak her veri kaynağı için bir kutucuk yok.<br>
Örneğin, **Azure Active Directory**. Bu veri kaynağına bağlanmak, Azure ad'deki tüm günlükler Azure Gözcü akış. Günlükleri almak için - wan türünü seçebilirsiniz. oturum açma günlükleri ve/veya denetim günlükleri. <br>
Böylece, hemen ınsights verilerinizin ilgi çekici almak en altında Azure Gözcü panoları için sizin yüklemeniz gerekir önerileri her bir bağlayıcının sağlar. <br> Yükleme yönergelerini izleyin veya [ilgili bağlantıyı Kılavuzu'na bakın](connect-data-sources.md) daha fazla bilgi için. Veri bağlayıcılar hakkında daha fazla bilgi için bkz: [bağlanmak Microsoft Hizmetleri](connect-data-sources.md).

Kaynaklarına bağlı verilerinizi sonra verilerinizi Azure Gözcü akış başlatır ve ile çalışmaya başlamak hazır. Günlükleri görüntüleyebilirsiniz [yerleşik panolar](quickstart-get-visibility.md) ve Log Analytics sorguları oluşturmaya başlayın [verileri araştırmak](tutorial-investigate-cases.md).



## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Gözcü için veri kaynaklarına bağlanma hakkında bilgi edindiniz. Azure Gözcü hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- Bilgi nasıl [görünürlük almak, veri ve olası tehditleri](quickstart-get-visibility.md).
- Başlama [Azure Gözcü kullanarak tehditleri algılama](tutorial-detect-threats.md).
- Stream verilerden [yaygın hata biçimi Gereçleri](connect-common-event-format.md) Azure Gözcü içine.
