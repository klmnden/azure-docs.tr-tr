---
title: Azure Active Directory raporları için Azure İzleyici çalışma kitaplarını kullanmak | Microsoft Docs
description: Azure İzleyici çalışma kitapları için Azure Active Directory raporlarını kullanmayı öğrenin
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
ms.openlocfilehash: 2c9b3d0ef110fea0629af345a71d0d7b7cce7313
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60014441"
---
# <a name="how-to-use-azure-monitor-workbooks-for-azure-active-directory-reports"></a>Nasıl yapılır: Azure İzleyici çalışma kitapları için Azure Active Directory raporlarını kullanma

İster misiniz:

- Etkisini anlamak, [koşullu erişim ilkeleri](../conditional-access/overview.md) kullanıcılarınızın oturum açma deneyimini şirket?

- Yanı sıra oturum açma, kuruluş durumunu daha iyi bir görünüm elde edin, sorunları hızlı bir şekilde çözmek için oturum açma hatalarını giderme?

- Ortamınıza oturum açmaya kimin eski kimlik doğrulama kullanıyor biliyor musunuz? Tarafından [eski bir kimlik doğrulama engelleme](../conditional-access/block-legacy-authentication.md), kiracınızın koruma artırabilir.


[Azure İzleyici çalışma kitapları](https://docs.microsoft.com/azure/azure-monitor/app/usage-workbooks) metin, analiz sorguları, Azure ölçümleri ve parametreleri etkileşimli zengin raporlar birleştirin. Azure Active Directory, yukarıdaki soruların yanıtlarını bulmanıza yardımcı olmak için izleme çalışma kitapları sağlar.

Bu makalede:

- Nasıl yapılır hakkında bilgi sahibi olduğunuzu varsayar [Azure İzleyici çalışma kitapları ile etkileşimli raporlar oluşturmanızı](https://docs.microsoft.com/azure/azure-monitor/app/usage-workbooks).

- Yukarıdaki soruları yanıtlamak için Azure İzleyici çalışma kitaplarını izleme hakkında nasıl kullanabileceğinizi açıklar.
 


## <a name="prerequisites"></a>Önkoşullar

Bu özelliği kullanmak için şunlara ihtiyacınız vardır:

- Premium (ö1/ö2) lisansına sahip bir Azure Active Directory kiracısı. Bilgi edinmek için nasıl [bir premium lisansı](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-get-started-premium).

- A [Log Analytics çalışma alanı](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace).

## <a name="access-workbooks"></a>Erişim çalışma kitapları 

Çalışma kitapları erişmek için:

1. Oturum açın, [Azure portalında](https://portal.azure.com).

2. Sol gezinti tıklatın **Azure Active Directory**.

3. İçinde **izleme** bölümünde **Insights**. 

    ![Insights](./media/howto-use-azure-monitor-workbooks/41.png)

4. Bir rapor ya da şablon veya tıklatın **açık** araç. 

    ![Galeri](./media/howto-use-azure-monitor-workbooks/42.png)


## <a name="sign-in-analysis"></a>Oturum açma analizi

Oturum açma analizi çalışma kitabına erişmesi için tıklatın **oturum açma işlemleri** içinde **kullanım** bölümü. 

Bu çalışma kitabı aşağıdaki oturum eğilimleri gösterir:

- Tüm oturum açma işlemleri

- Başarılı

- Bekleyen kullanıcı eylemi

- Hata

Her eğilim göre filtreleyebilirsiniz:

- Zaman aralığı

- Uygulamalar

- Kullanıcılar

![Galeri](./media/howto-use-azure-monitor-workbooks/43.png)


Her bir eğilim dökümünü aldığınız:

- Location

    ![Galeri](./media/howto-use-azure-monitor-workbooks/45.png)

- Cihaz

    ![Galeri](./media/howto-use-azure-monitor-workbooks/46.png)


## <a name="sign-ins-using-legacy-authentication"></a>Eski kimlik doğrulaması kullanarak oturum açma işlemleri 


Erişim ve oturum açma işlemleri kullanarak [eski bir kimlik doğrulama](../conditional-access/block-legacy-authentication.md) çalışma kitabını tıklayın **oturum açma eski kimlik doğrulaması kullanarak** içinde **kullanım** bölümü. 

Bu çalışma kitabı aşağıdaki oturum eğilimleri gösterir:

- Tüm oturum açma işlemleri

- Başarılı


Her eğilim göre filtreleyebilirsiniz:

- Zaman aralığı

- Uygulamalar

- Kullanıcılar

- Protokoller 

![Galeri](./media/howto-use-azure-monitor-workbooks/47.png)


Her bir eğilim için uygulama ve protokolü tarafından dökümünü alın.

![Galeri](./media/howto-use-azure-monitor-workbooks/48.png)



## <a name="sign-ins-by-conditional-access"></a>Oturum açma işlemleri koşullu erişim 


Oturum açma işlemleri tarafından erişmeye [koşullu erişim ilkeleri](../conditional-access/overview.md) çalışma kitabını tıklayın **koşullu erişim tarafından oturum açma** içinde **koşullu erişim** bölümü. 

Bu çalışma kitabı devre dışı oturum açma işlemleri için eğilimi gösterir.

Her eğilim göre filtreleyebilirsiniz:

- Zaman aralığı

- Uygulamalar

- Kullanıcılar

![Galeri](./media/howto-use-azure-monitor-workbooks/49.png)


Devre dışı oturum açma işlemleri için koşullu erişim durumuna göre dökümünü alın.

![Koşullu erişim durumu](./media/howto-use-azure-monitor-workbooks/conditional-access-status.png)








## <a name="sign-ins-by-grant-controls"></a>Oturum açma işlemleri verme denetimleri

Oturum açma işlemleri tarafından erişmeye [verme denetimleri](../conditional-access/controls.md) çalışma kitabını tıklayın **oturum açma denetimleri ver tarafından** içinde **koşullu erişim** bölümü. 

Bu çalışma kitabı aşağıdaki devre dışı oturum açma eğilimleri gösterir:

- MFA gerektirme
 
- Kullanım koşulları gerektirir

- Gizlilik bildirimi gerektirir

- Diğer


Her eğilim göre filtreleyebilirsiniz:

- Zaman aralığı

- Uygulamalar

- Kullanıcılar

![Galeri](./media/howto-use-azure-monitor-workbooks/50.png)


Her bir eğilim için uygulama ve protokolü tarafından dökümünü alın.

![Galeri](./media/howto-use-azure-monitor-workbooks/51.png)




## <a name="sign-ins-failure-analysis"></a>Oturum açma hata analizi

Kullanım **oturum açma hata analizi** çalışma kitabı ile hataları gidermek için:

- Oturum açma işlemleri
- Koşullu erişim ilkeleri
- Eski kimlik doğrulaması. 


Koşullu erişim verilere göre oturum açma işlemleri için tıklatın **oturum açma eski kimlik doğrulaması kullanarak** içinde **sorun giderme** bölümü. 

Bu çalışma kitabı aşağıdaki oturum eğilimleri gösterir:

- Tüm oturum açma işlemleri

- Başarılı

- Bekleyen eylem

- Hata


Her eğilim göre filtreleyebilirsiniz:

- Zaman aralığı

- Uygulamalar

- Kullanıcılar

![Galeri](./media/howto-use-azure-monitor-workbooks/52.png)


Oturum açma sorunlarını gidermek için bir dökümünü alma yöntemi:

- En sık karşılaşılan hatalar

    ![Galeri](./media/howto-use-azure-monitor-workbooks/53.png)

- Oturum açma için kullanıcı eylemi bekleniyor

    ![Galeri](./media/howto-use-azure-monitor-workbooks/54.png)






## <a name="next-steps"></a>Sonraki adımlar

* [Azure İzleyici çalışma kitapları ile etkileşimli raporlar oluşturun](https://docs.microsoft.com/azure/azure-monitor/app/usage-workbooks)