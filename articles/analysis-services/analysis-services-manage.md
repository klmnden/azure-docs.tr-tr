---
title: Azure Analysis Services yönetme | Microsoft Docs
description: Bir Analysis Services sunucusuna Azure yönetmeyi öğrenin.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 04/12/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: a589a75c1d8c353c7e8dabc508904282e28cf371
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34597697"
---
# <a name="manage-analysis-services"></a>Çözümleme Hizmetleri yönetme
Azure'da bir Analysis Services sunucusuna oluşturduktan sonra hemen veya süre yol gerçekleştirmeniz gereken bazı yönetim görevleri olabilir. Örneğin, sunucunuz modellerinde erişim veya sunucunuzun sistem durumu izleme kimin yenileme veri işlemeyi çalıştırın. Bazı yönetim görevleri, Azure portalında, diğerleri de SQL Server Management Studio (SSMS), yalnızca gerçekleştirilebilir ve bazı görevler ya da gerçekleştirilebilir.

## <a name="azure-portal"></a>Azure portalına
[Azure portal](http://portal.azure.com/) , oluşturmak ve sunucuları silin, sunucu kaynaklarını izlemek, boyutunu değiştirmek ve sunucularınızın kimlerin erişebileceğini yönetmek yerdir.  Bazı sorunlar yaşıyorsanız, ayrıca bir destek isteği gönderebilirsiniz.

![Azure'da sunucu adını alma](./media/analysis-services-manage/aas-manage-portal.png)

## <a name="sql-server-management-studio"></a>SQL Server Management Studio
Azure'da sunucunuza bağlanma yalnızca bir sunucu örneği veya kendi kuruluşunuzdaki bağlanma gibi olur. SSMS verileri işlemek gibi gerçekleştirdiğiniz görevlerin çoğunu gerçekleştirmek veya bir işleme betiği oluşturmak, rolleri yönetebilir ve PowerShell kullanın.
  
![SQL Server Management Studio](./media/analysis-services-manage/aas-manage-ssms.png)

### <a name="download-and-install-ssms"></a>SSMS yükleyip
Tüm almak için en son özellikleri ve Azure Analysis Services sunucusuna bağlanırken en yumuşak deneyimi SSMS en son sürümünü kullandığınızdan emin olun. 

[SQL Server Management Studio'yu indirme](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).


### <a name="to-connect-with-ssms"></a>SSMS ile bağlanma
 SSMS, sunucunuza ilk kez bağlanmadan önce kullanırken, kullanıcı adınızı Analysis Services Yöneticiler grubunda bulunduğundan emin olun. Daha fazla bilgi için bkz: [sunucu yöneticileri](#server-administrators) bu makalenin ilerisinde yer.

1. Bağlanmadan önce sunucu adını almanız gerekir. **Azure portalı** > sunucu > **Genel Bakış** > **Sunucu adı** menüsünde sunucu adını kopyalayın.
   
    ![Azure'da sunucu adını alma](./media/analysis-services-deploy/aas-deploy-get-server-name.png)
2. SSMS içinde > **Object Explorer**, tıklatın **Bağlan** > **Analysis Services**.
3. İçinde **sunucuya Bağlan** iletişim kutusu, sunucu adının, sonra da Yapıştır **kimlik doğrulaması**, aşağıdaki kimlik doğrulama türlerinden birini seçin:   
    > [!NOTE]
    > Kimlik doğrulama türü **Active Directory - MFA desteğiyle Evrensel**, önerilir.

    > [!NOTE]
    > Bir Microsoft Account, Live ID, Yanoo, Gmail, vb. oturum açarsanız parola alanı boş bırakın. Bağlan'ı tıklattıktan sonra için bir parola istenir.

    **Windows kimlik doğrulaması** Windows etki alanı\kullanıcı adı ve parola bilgileriniz kullanılacak.

    **Active Directory parola kimlik doğrulaması** kurumsal bir hesap kullanmak için. Örneğin, ne zaman bir etki bağlanma bilgisayar katıldı.

    **Active Directory - Evrensel MFA desteğiyle** kullanmak için [etkileşimli olmayan veya çok faktörlü kimlik doğrulaması](../sql-database/sql-database-ssms-mfa-authentication.md). 
   
    ![SSMS bağlanma](./media/analysis-services-manage/aas-manage-connect-ssms.png)

## <a name="server-administrators-and-database-users"></a>Sunucu yöneticileri ve veritabanı kullanıcısı
Azure Analysis Services içinde kullanıcılar, Yöneticiler ve veritabanı kullanıcılarını iki tür vardır. Her iki tür kullanıcı Azure Active Directory'yi olmalıdır ve kurumsal e-posta adresi veya UPN ile belirtilmelidir. Daha fazla bilgi edinmek için bkz. [Kimlik doğrulaması ve kullanıcı izinleri](analysis-services-manage-users.md).


## <a name="troubleshooting-connection-problems"></a>Bağlantı sorunlarını giderme
Sorunlarla karşılaşırsanız, SSMS, kullanarak bağlanırken, oturum açma önbelleğini temizlemek gerekebilir. Hiçbir şey önbelleğe diske. Önbelleği temizlemek için kapatın ve bağlanma işlemi başlatın. 

## <a name="next-steps"></a>Sonraki adımlar
Tablo modeli yeni sunucunuza zaten dağıttıysanız yapmadıysanız, iyi bir zamandır sunulmuştur. Daha fazla bilgi için bkz. [Azure Analysis Services’a Dağıtma](analysis-services-deploy.md).

Sunucunuza bir model dağıttıktan sonra bir istemci veya tarayıcı kullanarak bağlanmak hazırsınız. Daha fazla bilgi için bkz: [veri Azure Analysis Services sunucusundan alma](analysis-services-connect.md).

