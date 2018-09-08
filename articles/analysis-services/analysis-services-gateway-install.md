---
title: Şirket içi veri ağ geçidi yükleme | Microsoft Docs
description: Bir şirket içi veri ağ geçidi yükleyip yapılandırmanız hakkında bilgi edinin.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 09/07/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 89b21af5303afc2082d3d56ddb9e894f3ae4c4b8
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44158442"
---
# <a name="install-and-configure-an-on-premises-data-gateway"></a>Yükleme ve bir şirket içi veri ağ geçidi yapılandırma
Aynı bölgede bir veya daha fazla Azure Analysis Services sunucusu şirket içi veri kaynaklarına bağlanmak için bir şirket içi veri ağ geçidi gereklidir. Ağ geçidi hakkında daha fazla bilgi için bkz. [şirket içi veri ağ geçidi](analysis-services-gateway.md).

## <a name="prerequisites"></a>Önkoşullar
**En düşük gereksinimler:**

* .NET 4.5 framework
* Windows 7 64-bit sürümünü / Windows Server 2008 R2 (veya üzeri)

**Önerilen:**

* 8 çekirdekli CPU
* 8 GB bellek
* 64-bit sürüm Windows 2012 R2'in (veya üzeri)

**Önemli noktalar:**

* Azure ile ağ geçidi kaydı sırasında Kurulum sırasında aboneliğiniz için varsayılan bölge seçilir. Farklı bir bölge seçebilirsiniz. Birden fazla bölgede sunucunuz varsa, her bölge için bir ağ geçidi yüklemeniz gerekir. 
* Ağ geçidini bir etki alanı denetleyicisine yüklenemez.
* Yalnızca bir ağ geçidi tek bir bilgisayara yüklenebilir.
* Varsayılan olarak, oturum açmak için NT servıce\pbıegwservice hesabı ağ geçidini kullanır. Farklı bir hesap, Kurulum sırasında veya hizmetleri belirtilebilir. Hizmet hesabının günlük hizmeti ayrıcalıklara sahip olduğundan emin olun Grup İlkesi ayarları sağlar.
* Ağ geçidi üzerinde kalır ve uyku moduna geçmeyecek bir bilgisayara yükleyin.
* Ağ geçidi, kablosuz ağa bağlı bir bilgisayarda yüklemeyin. Performans yayınladıklarını.
* Azure'da oturum aç sahip bir hesap için aynı Azure AD'de [Kiracı](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant) aboneliğinde ağ geçidi kaydettirmekte olduğunuz. Azure B2B yüklerken ve bir ağ geçidi kaydediliyor (konuk) hesapları desteklenmez.
* Veri kaynakları, bir Azure sanal ağ (VNet) varsa, yapılandırmalısınız [AlwaysUseGateway](analysis-services-vnet-gateway.md) sunucu özelliği.
* Burada açıklanan (Birleşik) ağ geçidi, Azure kamu, Azure Almanya ve Çin Azure bağımsız bölgeler desteklenmiyor. Kullanım **Azure Analysis Services için ayrılmış şirket içi ağ geçidi**, sunucunuzun yüklü **Hızlı Başlangıç** portalında. 


## <a name="download"></a>İndirme
 [Ağ geçidini indirin](https://aka.ms/azureasgateway)

## <a name="install"></a>Yükleme

1. Kurulumu çalıştırın.

2. Bir konum seçin, koşulları kabul edin ve ardından **yükleme**.

   ![Konum ve Lisans Koşulları'nı yükleme](media/analysis-services-gateway-install/aas-gateway-installer-accept.png)

3. Azure'da oturum açın. Hesabın, kiracınızın Azure Active Directory'de olması gerekir. Bu hesap, Ağ Geçidi Yöneticisi için kullanılır. Azure B2B yüklerken ve ağ geçidi kaydediliyor (konuk) hesapları desteklenmez.

   ![Azure'da oturum açma](media/analysis-services-gateway-install/aas-gateway-installer-account.png)

   > [!NOTE]
   > Bir etki alanı hesabıyla oturum açarsanız, Azure AD'de Kurumsal hesabınızı eşlenir. Kuruluş hesabınız Ağ Geçidi Yöneticisi kullanılır.

## <a name="register"></a>Kaydolun
Azure'da bir ağ geçidi kaynağı oluşturmak için ağ geçidi bulut hizmetinde yüklü yerel örneği kaydetmeniz gerekir. 

1.  Seçin **bu bilgisayara yeni bir ağ geçidi kaydedin**.

    ![Kaydolma](media/analysis-services-gateway-install/aas-gateway-register-new.png)

2. Ağ geçidiniz için bir ad ve kurtarma anahtarı yazın. Varsayılan olarak, ağ geçidi, aboneliğin varsayılan bölge kullanır. Farklı bir bölge seçmek istiyorsanız seçin **bölgeyi Değiştir**.

    > [!IMPORTANT]
    > Kurtarma anahtarınızı güvenli bir yere kaydedin. Gerekli sırayla devralma için kurtarma anahtarı olduğunu, geçiş veya bir ağ geçidini geri yükleyin. 

   ![Kaydolma](media/analysis-services-gateway-install/aas-gateway-register-name.png)


## <a name="create-resource"></a>Bir Azure ağ geçidi kaynağı oluşturma
Yüklü ve kayıtlı ağ geçidi sonra Azure aboneliğinizde bir ağ geçidi kaynağı oluşturmak gerekir. Azure'a ağ geçidi kaydı sırasında kullanılan hesap ile oturum açın.

1. Azure portalında **yeni bir hizmet oluşturma** > **Kurumsal tümleştirme** > **şirket içi veri ağ geçidi**  >   **Oluşturma**.

   ![Bir ağ geçidi kaynağı oluşturma](media/analysis-services-gateway-install/aas-gateway-new-azure-resource.png)

2. İçinde **bağlantı ağ geçidi Oluştur**, bu ayarları girin:

    * **Ad**:, ağ geçidi kaynağı için bir ad girin. 

    * **Abonelik**: ağ geçidi kaynağınız ile ilişkilendirilecek Azure aboneliğini seçin. 
   
      Varsayılan abonelik Azure hesabında oturum açmak için kullanılan temel alır.

    * **Kaynak grubu**: Kaynak grubu oluşturun veya mevcut bir kaynak grubunu seçin.

    * **Konum**: ağ geçidiniz olarak kayıtlı bölgeyi seçin.

    * **Yükleme adı**: ağ geçidi yüklemenizi zaten seçili değilse, kayıtlı ağ geçidi'ni seçin. 

    İşiniz bittiğinde tıklayın **Oluştur**.

## <a name="connect-servers"></a>Ağ geçidi kaynağına sunucuları bağlama

1. Azure Analysis Services sunucusu Özete tıklayın **şirket içi veri ağ geçidi**.

   ![Sunucu, ağ geçidine bağlanma](media/analysis-services-gateway-install/aas-gateway-connect-server.png)

2. İçinde **bağlanmak için bir şirket içi veri geçidi çekme**, ağ geçidi kaynağınızı seçin ve ardından **Connect seçilen ağ geçidi**.

   ![Ağ geçidi kaynağına sunucusuna bağlanın](media/analysis-services-gateway-install/aas-gateway-connect-resource.png)

    > [!NOTE]
    > Ağ geçidiniz bu listede görünmüyorsa, sunucunuzun bölge ile aynı bölgede bir ağ geçidi kaydı sırasında belirttiğiniz değil olasıdır. 

İşte bu kadar. Bağlantı noktalarını açmak veya sorun giderme yapmak istiyorsanız, kullanıma mutlaka [şirket içi veri ağ geçidi](analysis-services-gateway.md).

## <a name="next-steps"></a>Sonraki adımlar
* [Analysis Services'ı yönetme](analysis-services-manage.md)   
* [Azure Analysis Services veri alma](analysis-services-connect.md)   
* [Azure Sanal Ağı üzerindeki veri kaynakları için ağ geçidi kullanma](analysis-services-vnet-gateway.md)
