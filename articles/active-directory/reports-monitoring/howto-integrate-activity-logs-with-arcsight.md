---
title: Azure Active Directory günlükleri Azure İzleyicisi'ni kullanarak ArcSight ile tümleştirme | Microsoft Docs
description: Azure Active Directory günlükleri Azure İzleyicisi'ni kullanarak ArcSight ile tümleştirmeyi öğrenin
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: b37bef0d-982e-4e28-86b2-6c61ca524ae1
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 04/19/2019
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 08a265637274f396497da37706391bf44e0c9107
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60437031"
---
# <a name="integrate-azure-active-directory-logs-with-arcsight-using-azure-monitor"></a>Azure İzleyicisi'ni kullanarak ArcSight ile Azure Active Directory günlükleri tümleştirme

[Micro odak ArcSight](https://software.microfocus.com/products/siem-security-information-event-management/overview) olan algılama ve yanıtlama için güvenlik tehditlerini platformda yardımcı olan bir güvenlik bilgileri ve Olay yönetimi (SIEM) çözümüdür. Azure AD için ArcSight Bağlayıcısı'nı kullanarak Azure İzleyicisi'ni kullanarak ArcSight için Azure Active Directory (Azure AD) günlükleri artık yönlendirebilirsiniz. Bu özellik, kiracınız için güvenliğin tehlikeye girmesi ArcSight kullanarak izlemenize olanak sağlar.  

Bu makalede, Azure AD günlüklerini Azure İzleyicisi'ni kullanarak ArcSight için yönlendirme hakkında bilgi edinin. 

## <a name="prerequisites"></a>Önkoşullar

Bu özelliği kullanmak için şunlara ihtiyacınız vardır:
* Azure AD etkinlik içeren bir Azure olay hub'ına kaydeder. Bilgi edinmek için nasıl [, etkinlik günlükleri Olay hub'ına akış](quickstart-azure-monitor-stream-logs-to-event-hub.md). 
* ArcSight Syslog NG Daemon SmartConnector (SmartConnector) ya da ArcSight Load Balancer yapılandırılmış bir örneği. Olayları ArcSight yük dengeleyiciye gönderiliyorsa, sonuç olarak SmartConnector için yük dengeleyici tarafından gönderilirler.

İndir ve Aç [ArcSight SmartConnector için Azure izleyici olay hub'ı için Yapılandırma Kılavuzu](https://community.softwaregrp.com/dcvta86296/attachments/dcvta86296/connector-documentation/1232/2/Microsoft%20Azure%20Monitor%20Event%20Hub.pdf). Bu kılavuz, yüklemek ve yapılandırmak için Azure İzleyici ArcSight SmartConnector için ihtiyacınız olan adımları içerir. 

## <a name="integrate-azure-ad-logs-with-arcsight"></a>Azure AD günlükleri ArcSight ile tümleştirme

1. İlk olarak, adımları tamamlamanız **önkoşulları** Yapılandırma Kılavuzu'nun bölümü. Bu bölüm, aşağıdaki adımları içerir:
    * Azure'da bir kullanıcıyla olduğundan emin olmak için kullanıcı izinleri ayarla **sahibi** dağıtmak ve yapılandırmak için rol.
    * Azure'dan erişilebilir olacak şekilde Syslog NG Daemon SmartConnector, sunucuyla bağlantı noktalarını açın. 
    * Bağlayıcı dağıtmak istediğiniz makinede komut dosyalarını çalıştırmak PowerShell etkinleştirmeniz gerekir böylece dağıtım bir Windows PowerShell Betiği çalıştırır.

2. Bağlantısındaki **bağlayıcısını dağıtma** bağlayıcı dağıtmak için Yapılandırma Kılavuzu bölümü. Bu bölümde indirin ve bağlayıcı ayıklayın, uygulamanın özelliklerini yapılandırmak ve ayıklanan klasöründen dağıtım betiğini çalıştırma konusunda size kılavuzluk eder. 

3. İçindeki adımları kullanın **azure'da dağıtımı doğrulama** bağlayıcı ayarlanır ve doğru çalıştığından emin olun. Aşağıdakileri doğrulayın:
    * Gerekli Azure işlevleri, Azure aboneliğinizde oluşturulur.
    * Azure AD günlükleri için doğru hedef aktarılır. 
    * İçindeki uygulama ayarlarında Azure işlev uygulamaları, uygulama ayarları dağıtımınızdaki kalıcıdır. 
    * CEF biçimindeki eşleşen dosyaları içeren depolama hesapları ve ArcSight bağlayıcı için bir Azure AD uygulaması ile Azure'da ArcSight için yeni bir kaynak grubu oluşturulur.

4. Son olarak, dağıtım sonrası adımları tamamlayın **dağıtım sonrası yapılandırmaları** Yapılandırma Kılavuzu. Bu bölümde, bir App Service boşta kalma zaman aşımı süresinden sonra giderek işlev uygulamaları engellemek, olay hub'ı tanılama günlüklerinin akışını yapılandırın ve SysLog-NG Daemon güncelleştirme planı üzerinde kullanıyorsanız, ek yapılandırma gerçekleştirmeniz açıklanmaktadır Yeni oluşturulan depolama hesabı ile ilişkilendirmek SmartConnector keystore sertifikası.

5. Yapılandırma Kılavuzu azure'a Bağlayıcısı özellikleri özelleştirme ve nasıl yükseltileceğini ve kaldırılacağını bağlayıcı ayrıca açıklar. Ayrıca bir bölümü yoktur üzerinde performans geliştirmeleri, yükseltme de dahil olmak üzere bir [Azure tüketim planı](https://azure.microsoft.com/pricing/details/functions) ve olay yükü tek bir Syslog NG Daemon SmartConnector için daha büyük ise bir ArcSight Load Balancer yapılandırma tanıtıcı.

## <a name="next-steps"></a>Sonraki adımlar

[Azure izleyici olay hub'ın ArcSight SmartConnector için Yapılandırma Kılavuzu](https://community.softwaregrp.com/dcvta86296/attachments/dcvta86296/connector-documentation/1232/2/Microsoft%20Azure%20Monitor%20Event%20Hub.pdf)
