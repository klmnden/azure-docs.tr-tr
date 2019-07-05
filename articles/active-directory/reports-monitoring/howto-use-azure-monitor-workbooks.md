---
title: Azure Active Directory raporları için Azure İzleyici çalışma kitaplarını kullanmak | Microsoft Docs
description: Azure İzleyici çalışma kitapları için Azure Active Directory raporlarını kullanmayı öğrenin.
services: active-directory
author: MarkusVi
manager: daveba
ms.assetid: 4066725c-c430-42b8-a75b-fe2360699b82
ms.service: active-directory
ms.devlang: ''
ms.topic: conceptual
ms.tgt_pltfrm: ''
ms.workload: identity
ms.subservice: report-monitor
ms.date: 04/18/2019
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 9433714c06dfad09270a6033f38a99471bcd517a
ms.sourcegitcommit: 6cb4dd784dd5a6c72edaff56cf6bcdcd8c579ee7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67513608"
---
# <a name="how-to-use-azure-monitor-workbooks-for-azure-active-directory-reports"></a>Azure İzleyici çalışma kitapları, Azure Active Directory raporlama kullanma

İster misiniz:

- Etkisini anlamak, [koşullu erişim ilkeleri](../conditional-access/overview.md) kullanıcılarınızın oturum açma deneyimini şirket?

- Oturum açma hatası kuruluşunuzun oturum açma durumu daha iyi bir görünüm elde edin ve sorunları hızlı bir şekilde çözmek için sorun giderme?

- Kimin eski kimlik doğrulamaları ortamınıza oturum açmak için kullandığı biliyor musunuz? (Tarafından [eski bir kimlik doğrulama engelleme](../conditional-access/block-legacy-authentication.md), kiracınızın korumasını artırabilirsiniz.)

Bu soruları yardımcı olmak üzere, izleme için çalışma kitapları Active Directory sağlar. [Azure İzleyici çalışma kitapları](https://docs.microsoft.com/azure/azure-monitor/app/usage-workbooks) metin, analiz sorguları, ölçümleri ve parametreleri etkileşimli zengin raporlar birleştirin. 

Bu makalede:

- Nasıl yapılır hakkında bilgi sahibi olduğunuz kabul edilmektedir [İzleyici çalışma kitaplarını kullanarak etkileşimli raporlar oluşturmanızı](https://docs.microsoft.com/azure/azure-monitor/app/usage-workbooks).

- İzleyici çalışma kitapları koşullu erişim ilkelerinizi, oturum açma sorunlarını giderme ve eski kimlik doğrulamaları tanımlamak için etkisini anlamak için nasıl kullanılacağını açıklar.
 


## <a name="prerequisites"></a>Önkoşullar

İzleyici çalışma kitaplarını kullanmak için gerekir:

- Premium (P1 veya P2) lisansına sahip bir Active Directory kiracısı. Bilgi edinmek için nasıl [bir premium lisansı](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-get-started-premium).

- A [Log Analytics çalışma alanı](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace).

## <a name="workbook-access"></a>Çalışma kitabı erişim 

Çalışma kitapları erişmek için:

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Sol gezinti bölmesinde seçin **Azure Active Directory**.

3. İçinde **izleme** bölümünden **çalışma kitapları**. 

    ![Insights'ı seçin](./media/howto-use-azure-monitor-workbooks/41.png)

4. Bir rapor veya şablonu seçin veya araç çubuğunda **açık**. 

    ![Aç'ı seçin](./media/howto-use-azure-monitor-workbooks/42.png)


## <a name="sign-in-analysis"></a>Oturum açma analizi

Oturum açma analizi çalışma kitabı erişmek için **kullanım** bölümünden **oturum açma**. 

Bu çalışma kitabı aşağıdaki oturum eğilimleri gösterir:

- Tüm oturum açma işlemleri

- Başarılı

- Bekleyen bir kullanıcı eylemi

- Hata

Her bir eğilim aşağıdaki kategorilere göre filtreleyebilirsiniz:

- Zaman aralığı

- Uygulamalar

- Kullanıcılar

![Oturum açma analizi](./media/howto-use-azure-monitor-workbooks/43.png)


Her bir eğilim için aşağıdaki kategorilere göre dökümünü alın:

- Location

    ![Konuma göre oturum açma](./media/howto-use-azure-monitor-workbooks/45.png)

- Cihaz

    ![Cihaza göre oturum açma](./media/howto-use-azure-monitor-workbooks/46.png)


## <a name="sign-ins-using-legacy-authentication"></a>Eski kimlik doğrulaması kullanarak oturum açma işlemleri 


Kullandığınız oturum açma işlemleri için çalışma kitabını erişmeye [eski bir kimlik doğrulama](../conditional-access/block-legacy-authentication.md), **kullanım** bölümünden **oturum açma eski kimlik doğrulaması kullanarak**. 

Bu çalışma kitabı aşağıdaki oturum eğilimleri gösterir:

- Tüm oturum açma işlemleri

- Başarılı


Her bir eğilim aşağıdaki kategorilere göre filtreleyebilirsiniz:

- Zaman aralığı

- Uygulamalar

- Kullanıcılar

- Protokoller

![Eski kimlik doğrulaması ile oturum açma](./media/howto-use-azure-monitor-workbooks/47.png)


Her bir eğilim için uygulama ve protokolü tarafından dökümünü alın.

![Eski kimlik doğrulaması oturum açma işlemlerini uygulama ve Protokolü](./media/howto-use-azure-monitor-workbooks/48.png)



## <a name="sign-ins-by-conditional-access"></a>Oturum açma işlemleri koşullu erişim 


Oturum açma işlemleri için çalışma kitabını erişmeye [koşullu erişim ilkeleri](../conditional-access/overview.md), **koşullu erişim** bölümünden **koşullu erişim tarafından oturum açma**. 

Bu çalışma kitabı devre dışı oturum açma işlemleri için eğilimleri gösterir. Her bir eğilim aşağıdaki kategorilere göre filtreleyebilirsiniz:

- Zaman aralığı

- Uygulamalar

- Kullanıcılar

![Koşullu Erişimle Oturum Açma İşlemleri](./media/howto-use-azure-monitor-workbooks/49.png)


Devre dışı oturum açma işlemleri için koşullu erişim durumuna göre dökümünü alın.

![Koşullu erişim durumu](./media/howto-use-azure-monitor-workbooks/conditional-access-status.png)








## <a name="sign-ins-by-grant-controls"></a>Oturum açma işlemleri verme denetimleri

Oturum açma işlemleri için çalışma kitabını erişmeye [verme denetimleri](../conditional-access/controls.md), **koşullu erişim** bölümünden **oturum açma denetimleri ver tarafından**. 

Bu çalışma kitabı aşağıdaki devre dışı oturum açma eğilimleri gösterir:

- MFA gerektirme
 
- Kullanım koşullarını gerekli kılma

- Gizlilik bildirimi gerektirir

- Diğer


Her bir eğilim aşağıdaki kategorilere göre filtreleyebilirsiniz:

- Zaman aralığı

- Uygulamalar

- Kullanıcılar

![Oturum açma işlemleri verme denetimleri](./media/howto-use-azure-monitor-workbooks/50.png)


Her bir eğilim için uygulama ve protokolü tarafından dökümünü alın.

![Son oturum açma işlemleri dökümü](./media/howto-use-azure-monitor-workbooks/51.png)




## <a name="sign-ins-failure-analysis"></a>Oturum açma hata analizi

Kullanım **oturum açma hata analizi** aşağıdaki hataları gidermek için çalışma kitabı:

- Oturum açma işlemleri
- Koşullu erişim ilkeleri
- Eski bir kimlik doğrulama 


Koşullu erişim verileri göre oturum açma erişmek için **sorun giderme** bölümünden **oturum açma eski kimlik doğrulaması kullanarak**. 

Bu çalışma kitabı aşağıdaki oturum eğilimleri gösterir:

- Tüm oturum açma işlemleri

- Başarılı

- Bekleyen eylem

- Hata


Her bir eğilim aşağıdaki kategorilere göre filtreleyebilirsiniz:

- Zaman aralığı

- Uygulamalar

- Kullanıcılar

![Oturum açma sorunlarını giderme](./media/howto-use-azure-monitor-workbooks/52.png)


Oturum açma gidermenize yardımcı olması için Azure İzleyici, bir çözümleme ve aşağıdaki kategorilere göre sunar:

- En sık karşılaşılan hatalar

    ![En sık karşılaşılan hatalar özeti](./media/howto-use-azure-monitor-workbooks/53.png)

- Oturum açma için kullanıcı eylemi bekleniyor

    ![Oturum açma için kullanıcı eylemi bekleniyor özeti](./media/howto-use-azure-monitor-workbooks/54.png)






## <a name="next-steps"></a>Sonraki adımlar

[İzleyici çalışma kitaplarını kullanarak etkileşimli raporlar oluşturmanızı](https://docs.microsoft.com/azure/azure-monitor/app/usage-workbooks).
