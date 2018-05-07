---
title: Microsoft Advanced Threat Analytics Azure Güvenlik Merkezi'ne bağlanma | Microsoft Docs
description: Azure Güvenlik Merkezi Microsoft Advanced Threat Analytics ile nasıl tümleşik çalıştığını öğrenin.
services: security-center
documentationcenter: na
author: terrylan
manager: MBaldwin
editor: ''
ms.assetid: 5d80bf91-16c3-40b3-82fc-e0805e6708db
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/05/2018
ms.author: yurid
ms.openlocfilehash: a3444b9d42ffdd5f81568f0e9e09557096b4415f
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="connecting-microsoft-advanced-threat-analytics-to-azure-security-center"></a>Bağlanan Microsoft Azure Güvenlik Merkezi'ne Advanced Threat Analytics
Bu belgede, Microsoft Advanced Threat Analytics ile Azure Güvenlik Merkezi arasında tümleştirme yapılandırmanıza yardımcı olur.

## <a name="why-add-advanced-threat-analytics-data"></a>Neden Advanced Threat Analytics veri eklensin mi?
[Advanced Threat Analytics (ATA)](https://docs.microsoft.com/advanced-threat-analytics/what-is-ata) şüpheli kullanıcı davranışı algılanmasına yardımcı olan bir şirket içi platformudur. Bağlandığınızda, Güvenlik Merkezi'nde ATA tarafından algılanan kuşkulu Eylemler görüntüleyebilirsiniz. Bu tümleştirme, görüntülemek, bağıntılı ve Güvenlik Merkezi, karma bulut iş yüklerini ilgili tüm güvenlik uyarılarını araştırmak sağlar. 

## <a name="how-do-i-configure-this-integration"></a>Bu tümleştirme nasıl yapılandırırım?
Zaten varsayılarak ATA yüklenmiş ve düzgün çalışmasını şirket içi, bu tümleştirme yapılandırmak için aşağıdaki adımları izleyin:

1. ATA Center'a oturum açın ve ATA portalına erişebilir.
2. Tıklatın **Syslog sunucusuna** sol bölmede.

    ![Syslog sunucusu](./media/security-center-ata-integration/security-center-ata-integration-fig1.png)

3. İçinde **Syslog sunucusu uç noktası** alan, (Bu adresi olmalıdır) 127.0.0.7 ve 5114 (önerilen) bağlantı noktasını yazın. Bağlantı noktası numarası bir öneri olsa da, herhangi bir benzersiz bağlantı çalışması gerekir. Olan ve'ı tıklatın tüm diğer seçenekleri bırakın **kaydetmek**.
4. Tıklatın **bildirimleri** sol bölmede ve aşağıdaki görüntüde gösterildiği gibi (önerilen) tüm Syslog bildirimlerini etkinleştirin:

    ![Bildirimler](./media/security-center-ata-integration/security-center-ata-integration-fig2.png)

5. **Kaydet**’e tıklayın.
6. **Güvenlik Merkezi** panosunu açın.
7. Sol bölmede, tıklatın **güvenlik çözümleri**.
8. Altında **Advanced Threat Analytics**, tıklatın **eklemek**.

    ![ATA](./media/security-center-ata-integration/security-center-ata-integration-fig3.png)
    
9. Son adım gidin ve tıklayın **Yükleme Aracısı**.

    ![ATA](./media/security-center-ata-integration/security-center-ata-integration-fig4.png)

10. İçinde **yeni Azure olmayan bilgisayar Ekle** sayfasında, çalışma alanını seçin.

    ![Azure Dışı](./media/security-center-ata-integration/security-center-ata-integration-fig5.png)

11. İçinde **doğrudan Aracısı** sayfasında uygun Windows Aracısı'nı indirin ve notlar, **çalışma alanı kimliği** ve **birincil anahtar**.

    ![Doğrudan Aracısı](./media/security-center-ata-integration/security-center-ata-integration-fig6.png)

12. Bu aracı, ATA Center'da yükleyin. Yükleme sırasında seçeneğini belirlediğinizden emin olun **aracıyı Azure günlük Analizi'ne bağlayın**ve sağlamak *çalışma alanı kimliği*, ve *birincil anahtar* istendiğinde.


Yüklemenin tamamlanması sonra tümleştirme tamamlandı ve Güvenlik Merkezi'den ATA gönderilen yeni uyarıları görmek kuramaz **arama** sonucu. Çözüm görünür **güvenlik çözümleri** sayfasında **bağlı çözümleri**. 

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Microsoft ATA Güvenlik Merkezi'ne bağlanmak öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Azure Active Directory Kimlik Koruması'nı Azure Güvenlik Merkezi'ne bağlama](security-center-aadip-integration.md)
* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-policies.md) — Azure Abonelikleriniz ve kaynak grupları için güvenlik ilkeleri yapılandırmayı öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md) — nasıl önerilerin Azure kaynaklarınızı korumanıza yardımcı öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Yönetme ve Azure Güvenlik Merkezi'nde güvenlik uyarılarını yanıtlama](security-center-managing-and-responding-alerts.md) — yönetme ve güvenlik uyarılarını yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
- [Azure Güvenlik Merkezi veri güvenliği](security-center-data-security.md) -verilerin yönetilmesi ve diğer Güvenlik Merkezi'nde korunması nasıl öğrenin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/) — en son Azure güvenlik haberlerini ve bilgilerini edinin.


