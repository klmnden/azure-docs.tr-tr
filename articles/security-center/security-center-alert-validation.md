---
title: Doğrulama (EICAR test dosyası) Azure Güvenlik Merkezi'nde uyarı | Microsoft Docs
description: Bu belge, Azure Güvenlik Merkezi'nde güvenlik uyarılarını doğrulamanıza yardımcı olur.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: f8f17a55-e672-4d86-8ba9-6c3ce2e71a57
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/02/2019
ms.author: rkarlin
ms.openlocfilehash: f65b4b74a1a91fa081bd9c0d8146d055cebb0de6
ms.sourcegitcommit: c0419208061b2b5579f6e16f78d9d45513bb7bbc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67626311"
---
# <a name="alert-validation-eicar-test-file-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde uyarı doğrulaması (EICAR test dosyası)
Bu belge, sisteminizin Azure Güvenlik Merkezi uyarıları için doğru yapılandırılıp yapılandırılmadığını doğrulamayı öğrenmenize yardımcı olur.

## <a name="what-are-security-alerts"></a>Güvenlik uyarıları nedir?
Güvenlik Merkezi, tehditleri kaynaklarınızın algıladığında oluşturduğu bildirimler uyarılardır. Bu öncelik veren ve uyarılar hızlı bir şekilde sorunu araştırmak için gereken bilgilerle birlikte listeler. Güvenlik Merkezi, ayrıca nasıl bir saldırıyı düzeltmek öneriler sağlar.
Daha fazla bilgi için [Azure Güvenlik Merkezi'nde güvenlik uyarılarını](security-center-alerts-overview.md) ve [yönetme ve Azure Güvenlik Merkezi'nde güvenlik uyarılarını yanıtlama](security-center-managing-and-responding-alerts.md)

## <a name="alert-validation"></a>Uyarı doğrulaması

* [Windows](#validate-windows)
* [Linux](#validate-linux)

## Windows VM'de uyarı doğrula <a name="validate-windows"></a>

Aracı yüklü Güvenlik Merkezi sonra uyarının saldırı kaynağı olmasını istediğiniz bilgisayardan aşağıdaki adımları izleyin:

1. Bir yürütülebilir dosya kopyalayın (örneğin **calc.exe**) bilgisayarın masaüstüne veya başka bir dizin size kolaylık sağlamak ve olarak yeniden adlandırın **ASC_AlertTest_662jfi039N.exe**.
1. Bir komut istemi açın ve bir bağımsız değişken (sadece sahte bir bağımsız değişken adı), bu dosyayla gibi yürütün: ```ASC_AlertTest_662jfi039N.exe -foo```
1. 5-10 dakika bekleyin ve Güvenlik Merkezi Uyarılarını açın. Benzer bir uyarı [örnek](#alert-validate) aşağıda görüntülenmesi gerekir:

> [!NOTE]
> Windows için bu test uyarıyı gözden geçirirken, alanın emin **bağımsız değişken denetimi etkinleştirildi** olduğu **true**. Eğer öyleyse **false**, komut satırı bağımsız değişkenleri denetimini etkinleştirmeniz gerekir. Bunu etkinleştirmek için aşağıdaki komut satırını kullanın:
>
>```reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\policies\system\Audit" /f /v "ProcessCreationIncludeCmdLine_Enabled"```

## Linux VM'de uyarı doğrula <a name="validate-linux"></a>

Aracı yüklü Güvenlik Merkezi sonra uyarının saldırı kaynağı olmasını istediğiniz bilgisayardan aşağıdaki adımları izleyin:
1. Uygun bir konuma yürütülebilir bir dosya kopyalama ve yeniden adlandırın **. / asc_alerttest_662jfi039n**, örneğin:

    ```cp /bin/echo ./asc_alerttest_662jfi039n```

1. Bir komut istemi açın ve bu dosyayı yürütün:

    ```./asc_alerttest_662jfi039n testing eicar pipe```

1. 5-10 dakika bekleyin ve Güvenlik Merkezi Uyarılarını açın. Benzer bir uyarı [örnek](#alert-validate) aşağıda görüntülenmesi gerekir:

### Uyarı örneği <a name="alert-validate"></a>

![Örnek uyarı doğrulaması](./media/security-center-alert-validation/security-center-alert-validation-fig2.png) 

## <a name="see-also"></a>Ayrıca bkz.
Bu makalede uyarıları doğrulama işlemine giriş yaptınız. Artık bu doğrulama hakkında bilgi sahibi olduğunuza göre, aşağıdaki makaleleri deneyebilirsiniz:

* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve ele alma](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Güvenlik Merkezi’nde uyarıları yönetme ve güvenlik olaylarına yanıt vermeyi öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md). Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'ndeki güvenlik uyarılarını anlama](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Farklı güvenlik uyarısı türleri hakkında bilgi edinin.
* [Azure Güvenlik Merkezi Sorun Giderme Kılavuzu](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Güvenlik Merkezi’nde sık karşılaşılan sorunları gidermeyi öğrenin.
* [Azure Güvenlik Merkezi SSS](security-center-faq.md). Hizmet kullanımı ile ilgili sık sorulan soruları bulun.
* [Azure Güvenlik Blogu](https://blogs.msdn.com/b/azuresecurity/). Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulun.
