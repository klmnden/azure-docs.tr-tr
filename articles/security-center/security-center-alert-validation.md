---
title: Azure Güvenlik Merkezi'nde Uyarıları Doğrulama | Microsoft Docs
description: Bu belge, Azure Güvenlik Merkezi'nde güvenlik uyarılarını doğrulamanıza yardımcı olur.
services: security-center
documentationcenter: na
author: terrylan
manager: mbaldwin
editor: ''
ms.assetid: f8f17a55-e672-4d86-8ba9-6c3ce2e71a57
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/07/2017
ms.author: yurid
ms.openlocfilehash: 5ac1e03fda90e12b6edbe28a66fdd488a34ea54b
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51240063"
---
# <a name="alerts-validation-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde Uyarıları Doğrulama
Bu belge, sisteminizin Azure Güvenlik Merkezi uyarıları için doğru yapılandırılıp yapılandırılmadığını doğrulamayı öğrenmenize yardımcı olur.

## <a name="what-are-security-alerts"></a>Güvenlik uyarıları nedir?
Güvenlik Merkezi, tehditleri algılamak ve tehditler konusunda sizi uyarmak için Azure kaynaklarınızdan, ağınızdan ve güvenlik duvarı ve uç nokta koruma çözümleri gibi bağlı iş ortağı çözümlerinden günlük verilerini otomatik olarak toplar, çözümler ve tümleştirir. Güvenlik uyarıları hakkında daha fazla bilgi edinmek için [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts) konusunu; farklı uyarı türleri hakkında dah fazla bilgi edinmek içinse [Azure Güvenlik Merkezi'nde güvenlik uyarılarını anlama](https://docs.microsoft.com/azure/security-center/security-center-alerts-type) konusunu okuyun.

## <a name="alert-validation"></a>Uyarı doğrulaması
Güvenlik Merkezi aracısı bilgisayarınıza yüklendikten sonra uyarının saldırı kaynağı olmasını istediğiniz bilgisayardan aşağıdaki adımları uygulayın:

1. Bilgisayarın masaüstüne veya sizin için uygun olan başka bir dizinine yürütülebilir bir dosya (örneğin, calc.exe) kopyalayın.
2. Bu dosyayı **ASC_AlertTest_662jfi039N.exe** olarak yeniden adlandırın.
3. Komut istemini açın ve bu dosyayı şunun gibi bir bağımsız değişkenle (sahte bir bağımsız değişken adı yeterlidir) yürütün: *ASC_AlertTest_662jfi039N.exe -foo*
4. 5-10 dakika bekleyin ve Güvenlik Merkezi Uyarılarını açın. Burada şuna benzer bir uyarı görmeniz gerekir:

    ![Uyarı Doğrulaması](./media/security-center-alert-validation/security-center-alert-validation-fig2.png)

Bu uyarıyı gözden geçirirken, Bağımsız Değişken Denetimi Etkinleştirildi alanının doğru olarak göründüğünden emin olun. Yanlış olarak görünüyorsa, komut satırı bağımsız değişkenleri denetimini etkinleştirmeniz gerekir. Bu seçeneği şu komut satırını kullanarak etkinleştirebilirsiniz:

*reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\policies\system\Audit" /f /v "ProcessCreationIncludeCmdLine_Enabled"*


> [!NOTE]
> Bu özelliği görmek için [Azure Güvenlik Merkezi'nde Uyarı Doğrulaması](https://channel9.msdn.com/Blogs/Azure-Security-Videos/Alert-Validation-in-Azure-Security-Center) videosunu izleyin. 

## <a name="see-also"></a>Ayrıca bkz.
Bu makalede uyarıları doğrulama işlemine giriş yaptınız. Artık bu doğrulama hakkında bilgi sahibi olduğunuza göre, aşağıdaki makaleleri deneyebilirsiniz:

* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve ele alma](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Güvenlik Merkezi’nde uyarıları yönetme ve güvenlik olaylarına yanıt vermeyi öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md). Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'ndeki güvenlik uyarılarını anlama](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Farklı güvenlik uyarısı türleri hakkında bilgi edinin.
* [Azure Güvenlik Merkezi Sorun Giderme Kılavuzu](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Güvenlik Merkezi’nde sık karşılaşılan sorunları gidermeyi öğrenin. 
* [Azure Güvenlik Merkezi SSS](security-center-faq.md). Hizmet kullanımı ile ilgili sık sorulan soruları bulun.
* [Azure Güvenlik Blogu](https://blogs.msdn.com/b/azuresecurity/). Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulun.

