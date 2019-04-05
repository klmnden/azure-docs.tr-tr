---
title: Azure Marketi için bir sanal makine teklifi için teknik varlıklarınızı oluşturun | Microsoft Docs
description: Azure Marketi'nde bir sanal makine teklifi için teknik varlıkları oluşturma açıklanır.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: pbutlerm
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 08/20/2018
ms.author: pbutlerm
ms.openlocfilehash: 6f1a93c3d3059e612d8c309b263e263dbb84c67f
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59050110"
---
# <a name="create-technical-assets-for-a-virtual-machine-offer"></a>Bir sanal makine teklifi için teknik varlıkları oluşturma

Bu bölümde, oluşturma ve Azure Market teknik varlıkları bir sanal makine (VM) teklifi için yapılandırma açıklanmaktadır.  Bir VM iki bileşenleri içerir: isteğe bağlı ilişkili veri diskleri ve çözüm sanal sabit disk (VHD).  

- *Sanal sabit diskleri (VHD'ler)*, işletim sistemi ve Azure Marketi'nde teklifinizle dağıtacağınız çözümünüzü içeren. VHD Hazırlama işlemi Linux tabanlı olmasına bağlı olarak farklılık gösterir, Windows tabanlı veya özel tabanlı bir VM.
- *Veri diskleri* bir sanal makine için ayrılmış, kalıcı depolama temsil eder. Yapmak *değil* VHD çözümü kullanın (örneğin, `C:` sürücü) kalıcı bilgileri depolamak için.

Bir işletim sistemi diski ile sıfır, bir VM görüntüsü içerir veya daha fazla veri diski. Disk başına bir VHD gereklidir. Oluşturulacak bir VHD bile boş veri diskleri gerektirir.
VM işletim sistemi, VM boyutu, açmak için bağlantı noktası yapılandırmanız gerekir ve bağlı veri diskleri en fazla 15.

> [!TIP] 
> Kullandığınız işletim sisteminden bağımsız olarak yalnızca SKU için gereken en az sayıda veri diski ekleyin. Müşteriler dağıtım sırasında bir görüntünün parçası olan diskler kaldıramazsınız ancak bunlar her zaman diskleri süresince veya dağıtım sonrasında ekleyebilirsiniz. 

> [!IMPORTANT]
> *Yeni bir görüntü sürümü sayısı disk değiştirmeyin.* Görüntüde veri diskleri yeniden yapılandırmanız gerekir, yeni bir SKU'ya tanımlayın. Farklı disk sayısı olan yeni bir görüntü sürümü yayımlama, yeni dağıtım çözümleri Azure Resource Manager şablonları ve diğer senaryolar ile otomatik ölçeklendirme, otomatik dağıtım durumlarda yeni görüntü sürümünü temel bozucu olası sahip olur.

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

## <a name="fundamental-technical-knowledge"></a>Temel teknik bilgiler

Tasarım, geliştirme ve test bu varlıkları zaman ayırın ve Azure platformu ve teklif oluşturmak için kullanılan teknolojileri teknik bilgi gerektirir. Çözüm etki alanınız ek olarak, mühendislik ekibiniz aşağıdaki Microsoft teknolojileri hakkında bilgilere sahip olmanız gerekir: 
-   Temel düzeyde bilinmesini [Azure Hizmetleri](https://azure.microsoft.com/services/) 
-   Nasıl yapılır [tasarlayabilir ve Azure uygulamaları tasarlama](https://azure.microsoft.com/solutions/architecture/)
-   Bilgisine [Azure sanal makineler](https://azure.microsoft.com/services/virtual-machines/), [Azure depolama](https://azure.microsoft.com/services/?filter=storage) ve [Azure ağ iletişimi](https://azure.microsoft.com/services/?filter=networking)
-   Bilgisine [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/)
-   Bilgisine [JSON](https://www.json.org/)


## <a name="suggested-tools"></a>Önerilen Araçlar 

Birini veya her ikisini VHD'ler ve sanal makineleri yönetmenize yardımcı olması için aşağıdaki komut dosyası ortamları seçin:
-   [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)
-   [Azure CLI](https://docs.microsoft.com/cli/azure)

Ayrıca, aşağıdaki araçları, geliştirme ortamınızı eklemenizi öneririz: 

-   [Azure Depolama Gezgini](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer)
-   [Visual Studio Code](https://code.visualstudio.com/)
    *   Dahili Hat: [Azure Resource Manager araçları](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)
    *   Dahili Hat: [Beautify](https://marketplace.visualstudio.com/items?itemName=HookyQR.beautify)
    *   Dahili Hat: [JSON prettify](https://marketplace.visualstudio.com/items?itemName=mohsen1.prettify-json)

Ayrıca araçlar da gözden geçirme öneririz [Azure Geliştirici Araçları](https://azure.microsoft.com/tools/) sayfası ve Visual Studio kullanıyorsanız [Visual Studio Market](https://marketplace.visualstudio.com/).


## <a name="next-steps"></a>Sonraki adımlar

Bu bölümde sonraki makalelerinde oluşturma ve bu VM varlıkları kaydetme adım adım:

1. [Azure ile uyumlu sanal sabit disk Oluştur](./cpp-create-vhd.md) ya da bir Linux veya Windows tabanlı Azure ile uyumlu bir VHD oluşturma açıklanır.  Bu VM karşıya yükleme için hazırlanılıyor boyutlandırma ve düzeltme eki uygulama gibi en iyi uygulamaları içerir.

2. [Sanal makineye bağlanma](./cpp-connect-vm.md) uzaktan, yeni oluşturulan VM'ye bağlanın ve içine oturum açıklanmaktadır.  Bu makalede, ayrıca kullanım maliyet tasarrufu için VM'yi durdurmak nasıl açıklar.

3. [Sanal makine yapılandırma](./cpp-configure-vm.md) doğru VHD boyutunu seçme, görüntünüzü generalize, en son güncelleştirmeleri (düzeltme) uygulama ve özel yapılandırmalar zamanlama açıklanmaktadır.

4. [Bir sanal sabit diskten bir sanal makine dağıtma](./cpp-deploy-vm-vhd.md) nasıl Azure dağıtılan bir VHD'den VM kaydedileceği açıklanmaktadır.  Gereken araçları ve bunları bir kullanıcı VM görüntüsü oluşturun, ardından kullanarak Azure'a dağıtmak için nasıl kullanılacağını listeler [Microsoft Azure Portal'da](https://ms.portal.azure.com/) veya PowerShell betikleri. 

5. [Bir sanal makine görüntüsü onaylamak](./cpp-certify-vm.md) test edin ve Azure Marketi'nde sertifika için bir VM görüntüsü gönderme açıklanmaktadır. Nereden edinilir açıklar *Azure sertifikası için sertifika Test aracı* aracı ve VM görüntünüzü onaylamak için bu aracı kullanın. 

6. [SAS URI'sini Al](./cpp-get-sas-uri.md) paylaşılan erişim imzası (SAS), VM yansımaları için URI alınacağı açıklanmaktadır.
 
Destekleyici bir makale olarak [ortak paylaşılan erişim imzası URL'si sorunlarını](./cpp-common-sas-url-issues.md) SAS URI'leri ile ilgili olası çözümü karşılaşabileceğiniz bazı yaygın sorunlar listelenir.

Tüm adımları tamamladıktan sonra hazır olmayacak [, VM teklifi yayımlama](./cpp-publish-offer.md) Azure Marketi.
