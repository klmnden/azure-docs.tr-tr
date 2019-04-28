---
title: Azure Analysis Services'ı yönetme | Microsoft Docs
description: Bir Azure Analysis Services sunucusu'nı yönetmeyi öğrenin.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 0bae06d46c2c96ba9dd058e9c2d380379523811c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61065208"
---
# <a name="manage-analysis-services"></a>Analysis Services'ı yönetme
Azure'da bir Analysis Services sunucusuna oluşturduktan sonra hemen veya süre yol gerçekleştirmeniz gereken bazı yönetim görevleri olabilir. Örneğin, kimlerin sunucunuzdaki modelleri erişmek veya sunucunuzun sistem durumunu izleyin, yenileme veri işleme çalıştırın. Bazı yönetim görevlerini yalnızca diğer SQL Server Management Studio (SSMS), Azure portalında gerçekleştirilebilir ve bazı görevler de gerçekleştirilebilir.

## <a name="azure-portal"></a>Azure portal
[Azure portalında](https://portal.azure.com/) Burada, oluşturma ve sunucuları silin, sunucu kaynaklarını izleyebilir, boyutunu değiştirmek, ve sunucularınıza kimlerin erişebildiğini yönetmek.  Bazı sorunlar yaşıyorsanız, bir destek isteği gönderebilirsiniz.

![Azure'da sunucu adını alma](./media/analysis-services-manage/aas-manage-portal.png)

## <a name="sql-server-management-studio"></a>SQL Server Management Studio
Azure'da, sunucuya bağlamak için yalnızca kendi kuruluşunuzda bir sunucu örneğine bağlanma gibi aynıdır. SSMS verileri işlemek gibi aynı görevlerin çoğunu gerçekleştirmek veya bir işleme betiği oluşturmak, rolleri yönetme ve PowerShell kullanın.
  
![SQL Server Management Studio](./media/analysis-services-manage/aas-manage-ssms.png)

### <a name="download-and-install-ssms"></a>SSMS indirip
Azure Analysis Services sunucunuza bağlanma sırasında tüm yeni özelliklere ve en yumuşak deneyimi almak için en son SSMS sürümünü kullandığınızdan emin olun. 

[SQL Server Management Studio'yu indirme](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).


### <a name="to-connect-with-ssms"></a>SSMS ile bağlanma
 SSMS, sunucunuza ilk kez bağlanmadan önce kullanırken, kullanıcı adınızı Analysis Services yöneticileri grubunda bulunduğundan emin olun. Daha fazla bilgi için bkz. [sunucu yöneticileri ve veritabanı kullanıcıları](#server-administrators-and-database-users) bu makalenin ilerleyen bölümlerinde.

1. Bağlanmadan önce sunucu adını almanız gerekir. **Azure portalı** > sunucu > **Genel Bakış** > **Sunucu adı** menüsünde sunucu adını kopyalayın.
   
    ![Azure'da sunucu adını alma](./media/analysis-services-deploy/aas-deploy-get-server-name.png)
2. SSMS > **Nesne Gezgini**'nde **Bağlan** > **Analysis Services**'e tıklayın.
3. İçinde **sunucuya Bağlan** iletişim kutusu, yapıştırma sunucu adını, sonra da **kimlik doğrulaması**, aşağıdaki kimlik doğrulama türlerinden birini seçin:   
    > [!NOTE]
    > Kimlik doğrulama türü, **Active Directory - MFA desteğiyle Evrensel**, önerilir.

    > [!NOTE]
    > Bir Microsoft Account, Live ID, Yahoo, Gmail, vb. oturum imzalarsanız parola alanını boş bırakın. Bağlan'a tıkladıktan sonra bir parola istenir.

    **Windows kimlik doğrulaması** Windows etki alanı\kullanıcı adı ve parola bilgileriniz kullanılacak.

    **Active Directory parola kimlik doğrulaması** bir kurumsal hesap kullanılacak. Örneğin, ne zaman bir etki bağlanma bilgisayar katıldı.

    **Active Directory - MFA desteğiyle Evrensel** kullanılacak [etkileşimli olmayan veya çok faktörlü kimlik doğrulaması](../sql-database/sql-database-ssms-mfa-authentication.md). 
   
    ![SSMS'de bağlanma](./media/analysis-services-manage/aas-manage-connect-ssms.png)

## <a name="server-administrators-and-database-users"></a>Sunucu yöneticileri ve veritabanı kullanıcıları
Azure Analysis Services'de kullanıcı, Yöneticiler ve veritabanı kullanıcıları iki türü vardır. Her iki tür kullanıcı Azure Active Directory'de olması gerekir ve kuruluş e-posta adresi veya UPN ile belirtilmelidir. Daha fazla bilgi edinmek için bkz. [Kimlik doğrulaması ve kullanıcı izinleri](analysis-services-manage-users.md).


## <a name="troubleshooting-connection-problems"></a>Bağlantı sorunlarını giderme
Sorunlarla karşılaşırsanız, SSMS kullanarak bağlanma, oturum açma önbelleğini temizlemeniz gerekebilir. Hiçbir şey önbelleğe diske. Önbelleği temizlemek için kapatın ve connect işlemini yeniden başlatın. 

## <a name="next-steps"></a>Sonraki adımlar
Yeni sunucunuz için zaten bir tablosal model dağıtmadıysanız, artık iyi bir zamandır. Daha fazla bilgi için bkz. [Azure Analysis Services’a Dağıtma](analysis-services-deploy.md).

Sunucunuz için bir model dağıttıysanız, istemci veya tarayıcı kullanarak bağlanmak hazırsınız. Daha fazla bilgi için bkz. [veri Azure Analysis Services sunucusundan alma](analysis-services-connect.md).

