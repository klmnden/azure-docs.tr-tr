---
title: Şirket içi veri ağ geçidi yükleme | Microsoft Docs
description: Yükleme ve bir şirket içi veri ağ geçidi yapılandırma öğrenin.
author: minewiskan
manager: kfile
ms.service: analysis-services
ms.topic: conceptual
ms.date: 04/12/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 5a923d3b5fbb5e7afe5f2a922ba083608ff35fd9
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="install-and-configure-an-on-premises-data-gateway"></a>Yükleme ve bir şirket içi veri ağ geçidi yapılandırma
Bir veya daha fazla Azure Analysis Services sunucuları aynı bölgede şirket içi veri kaynaklarına bağlanmak için bir şirket içi veri ağ geçidi gereklidir. Ağ geçidi hakkında daha fazla bilgi için bkz: [şirket içi veri ağ geçidi](analysis-services-gateway.md).

## <a name="prerequisites"></a>Önkoşullar
**En düşük gereksinimler:**

* .NET 4.5 framework
* 64 bit sürümü Windows 7 / Windows Server 2008 R2 (veya üstü)

**Önerilen:**

* 8 çekirdekli CPU
* 8 GB bellek
* 64 bit sürümü Windows 2012 R2'in (veya üstü)

**Önemli noktalar:**

* Ağ geçidiniz Azure ile kaydedilirken Kurulum sırasında aboneliğiniz için varsayılan bölge seçilir. Farklı bir bölgeye seçebilirsiniz. Birden çok bölgede sunucularınız varsa, her bölge için bir ağ geçidi yüklemeniz gerekir. 
* Ağ geçidi etki alanı denetleyicisine yüklenemez.
* Yalnızca bir ağ geçidi tek bir bilgisayara yüklenebilir.
* Ağ geçidi üzerinde kalır ve uyku moduna geçmez bir bilgisayara yükleyin.
* Ağ geçidi kablosuz ağınıza bağlı bir bilgisayarda yüklemeyin. Performans yayınladıklarını.
* Azure'da oturum aç sahip bir hesap için aynı Azure AD'de [Kiracı](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant) abonelik, ağ geçidini kaydediyorsunuz. Azure B2B yüklerken ve bir ağ geçidi kaydediliyor (konuk) hesapları desteklenmez.
* Burada açıklanan (Birleşik) ağ geçidi Azure kamu, Azure Almanya ve Azure Çin sovereign bölgelerde desteklenmiyor. Kullanım **Azure Analysis Services için ayrılmış şirket içi ağ geçidi**, sunucunuzun yüklü **Hızlı Başlangıç** Portalı'nda. 


## <a name="download"></a>İndirme
 [Ağ geçidini indirin](https://aka.ms/azureasgateway)

## <a name="install"></a>Yükleme

1. Kur'u yeniden çalıştırın.

2. Bir konum seçin, koşulları kabul edin ve ardından **yükleme**.

   ![Konum ve Lisans Koşulları'nı yükleme](media/analysis-services-gateway-install/aas-gateway-installer-accept.png)

3. Azure'da oturum açın. Hesap, kiracının Azure Active Directory'de olmalıdır. Bu hesap için Ağ Geçidi Yöneticisi kullanılır. Azure B2B yüklerken ve ağ geçidi kaydediliyor (konuk) hesapları desteklenmez.

   ![Azure'da oturum açma](media/analysis-services-gateway-install/aas-gateway-installer-account.png)

   > [!NOTE]
   > Bir etki alanı hesabıyla oturum açarsanız Azure AD içinde Kurumsal hesabınızı eşlenir. Kuruluş hesabınızla Ağ Geçidi Yöneticisi olarak kullanılır.

## <a name="register"></a>Kaydetme
Bir ağ geçidi kaynağı oluşturmak için ağ geçidi bulut hizmetiyle yüklü yerel örneği kaydetmeniz gerekir. 

1.  Seçin **bu bilgisayarda yeni bir ağ geçidini kaydetmek**.

    ![Kaydolma](media/analysis-services-gateway-install/aas-gateway-register-new.png)

2. Ağ geçidiniz için bir ad ve kurtarma anahtarı yazın. Varsayılan olarak, ağ geçidi aboneliğinizin varsayılan bölge kullanır. Farklı bir bölge seçmek gereksinim duyarsanız, seçin **değişikliğin bölgesini**.

    > [!IMPORTANT]
    > Kurtarma anahtarı güvenli bir yerde kaydedin. Kurtarma anahtarı gerekli sıralı devralma için olduğundan, geçiş veya bir ağ geçidi geri yükleme. 

   ![Kaydolma](media/analysis-services-gateway-install/aas-gateway-register-name.png)


## <a name="create-resource"></a>Bir Azure ağ geçidi kaynağı oluşturma
Yüklü ve ağ geçidiniz kayıtlı sonra Azure aboneliğinizde bir ağ geçidi kaynağı oluşturmanız gerekir. Azure'da ağ geçidi kaydederken kullanılan aynı hesap ile oturum açın.

1. Azure portalında tıklatın **yeni bir hizmet Oluştur** > **Kurumsal tümleştirme** > **şirket içi veri ağ geçidi**  >   **Oluşturma**.

   ![Bir ağ geçidi kaynağı oluşturun](media/analysis-services-gateway-install/aas-gateway-new-azure-resource.png)

2. İçinde **oluşturma bağlantı ağ geçidi**, bu ayarları girin:

    * **Ad**: ağ geçidi kaynağı için bir ad girin. 

    * **Abonelik**: ağ geçidi kaynağınız ile ilişkilendirmek için Azure aboneliğini seçin. 
   
      Varsayılan abonelik oturum açmak için kullandığınız Azure hesabı temel alır.

    * **Kaynak grubu**: Kaynak grubu oluşturun veya mevcut bir kaynak grubunu seçin.

    * **Konum**:, ağ geçidi kayıtlı bölgeyi seçin.

    * **Yükleme adı**: ağ geçidi yüklemenizi seçili değilse, kayıtlı ağ geçidi seçin. 

    İşiniz bittiğinde tıklatın **oluşturma**.

## <a name="connect-servers"></a>Sunucular ağ geçidi kaynağa bağlanmak

1. Azure Analysis Services sunucusu genel bakışta tıklatın **şirket içi veri ağ geçidi**.

   ![Ağ Geçidi sunucusuna bağlanın](media/analysis-services-gateway-install/aas-gateway-connect-server.png)

2. İçinde **bağlanmak için bir şirket içi veri geçidi çekme**, ağ geçidi kaynağı seçin ve ardından **Bağlan seçilen ağ geçidi**.

   ![Sunucu ağ geçidi kaynağına bağlanma](media/analysis-services-gateway-install/aas-gateway-connect-resource.png)

    > [!NOTE]
    > Ağ geçidiniz listede görünmüyorsa, sunucunuzun bölge ile aynı bölgede, ağ geçidi kaydedilirken belirtilmemiş olasıdır. 

Bu kadar. Bağlantı noktalarını açmak veya sorun giderme yapmanız gerekiyorsa, kullanıma mutlaka [şirket içi veri ağ geçidi](analysis-services-gateway.md).

## <a name="next-steps"></a>Sonraki adımlar
* [Çözümleme Hizmetleri yönetme](analysis-services-manage.md)   
* [Azure Analysis Services Veri Al](analysis-services-connect.md)
