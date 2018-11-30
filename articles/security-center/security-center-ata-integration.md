---
title: Microsoft Advanced Threat Analytics, Azure Güvenlik Merkezi'ne bağlanma | Microsoft Docs
description: Azure Güvenlik Merkezi, Microsoft Advanced Threat Analytics ile nasıl tümleşik çalıştığını öğrenin.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 5d80bf91-16c3-40b3-82fc-e0805e6708db
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/29/2018
ms.author: rkarlin
ms.openlocfilehash: bcd9b006c5451cb2d251cd5ff9e6ae5e0bd17f3c
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2018
ms.locfileid: "52634022"
---
# <a name="connecting-microsoft-advanced-threat-analytics-to-azure-security-center"></a>Bağlanan Microsoft Advanced Threat Analytics için Azure Güvenlik Merkezi
Bu belge Azure Güvenlik Merkezi ile Microsoft Advanced Threat Analytics arasındaki tümleştirmeyi yapılandırmak için yardımcı olur.

## <a name="why-add-advanced-threat-analytics-data"></a>Advanced Threat Analytics veri neden eklensin mi?
[Advanced Threat Analytics (ATA)](https://docs.microsoft.com/advanced-threat-analytics/what-is-ata) şüpheli kullanıcı davranışları algılamaya yardımcı olur şirket içi platformdur. Bağlandığınızda, Güvenlik Merkezi, ATA tarafından algılanan kuşkulu eylemleri görüntüleyebilirsiniz. Bu tümleştirme, görüntülemek, ilişkilendirmenize ve karma bulut iş yüklerinizin Güvenlik Merkezi'nde ilgili tüm güvenlik uyarıları araştırmanıza olanak sağlar.

## <a name="how-do-i-configure-this-integration"></a>Bu tümleştirmenin nasıl yapılandırabilirim?
Zaten sahip olduğunuz varsayılarak ATA yüklü ve düzgün çalışan şirket içi, bu tümleştirmeyi yapılandırmak için aşağıdaki adımları izleyin:

1. ATA Center için oturum açın ve ATA portalına erişim.
2. Tıklayın **Syslog sunucusuna** sol bölmesinde.

    ![Syslog sunucusu](./media/security-center-ata-integration/security-center-ata-integration-fig1.png)

3. İçinde **Syslog sunucusu uç noktası** alanına 127.0.0.7 (Bu adres olmalıdır) yazın ve 5114 (önerilen) bağlantı noktasını yazın. Herhangi bir benzersiz bağlantı noktası, bağlantı noktası numarası bir öneri olarak çalışması gerekir. Bırakın ve tıklayın gibi diğer tüm seçenekler **Kaydet**.
4. Tıklayın **bildirimleri** sol bölmedeki ve aşağıdaki görüntüde gösterildiği gibi (önerilen) tüm Syslog bildirimlerini etkinleştirin:

    ![Bildirimler](./media/security-center-ata-integration/security-center-ata-integration-fig2.png)

5. **Kaydet**’e tıklayın.
6. **Güvenlik Merkezi** panosunu açın.
7. Sol bölmede, tıklayın **güvenlik çözümlerini**.
8. Altında **Advanced Threat Analytics**, tıklayın **ekleme**.

    ![ATA](./media/security-center-ata-integration/security-center-ata-integration-fig3.png)

9. Son adım gidip tıklatın **aracıyı indir'e**.

    ![ATA](./media/security-center-ata-integration/security-center-ata-integration-fig4.png)

10. İçinde **yeni Azure olmayan bilgisayar ekleme** sayfasında, çalışma alanını seçin.

    ![Azure Dışı](./media/security-center-ata-integration/security-center-ata-integration-fig5.png)

11. İçinde **doğrudan aracı** sayfasında, uygun Windows aracısını indir ve Not **çalışma alanı kimliği** ve **birincil anahtar**.

    ![Doğrudan aracı](./media/security-center-ata-integration/security-center-ata-integration-fig6.png)

12. Bu aracı, ATA Center'da yükleyin. Yükleme sırasında seçeneğini belirlediğinizden emin olun **aracıyı Azure Log Analytics'e bağlama**ve *çalışma alanı kimliği*, ve *birincil anahtar* istendiğinde.


ATA'dan Güvenlik Merkezi'nde gönderilen yeni uyarıları görmek mümkün olacaktır yüklemeyi bitirmek ve tümleştirme tamamlandıktan sonra **arama** sonucu. Çözüm görünür **güvenlik çözümlerini** sayfasındaki **bağlantılı çözümler**.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Güvenlik Merkezi'ne Microsoft ATA bağlantısı kurma öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Azure Active Directory Kimlik Koruması'nı Azure Güvenlik Merkezi'ne bağlama](security-center-aadip-integration.md)
* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-azure-policy.md) — Azure Abonelikleriniz ve kaynak grupları için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md) -önerilerin Azure kaynaklarınızı korumanıza nasıl yardımcı olduğunu öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Yönetme ve Azure Güvenlik Merkezi'nde güvenlik uyarılarını yanıtlama](security-center-managing-and-responding-alerts.md) — yönetme ve güvenlik uyarılarını yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
- [Azure Güvenlik Merkezi veri güvenliği](security-center-data-security.md) -verileri nasıl yönetildiği ve korunduğu Güvenlik Merkezi'nde öğrenin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure güvenlik blogu](https://blogs.msdn.com/b/azuresecurity/) — en son Azure güvenlik haberlerini ve bilgilerini edinin.
