---
title: include dosyası
description: include dosyası
services: virtual-machines-windows
author: rothja
manager: craigg
tags: azure-service-management
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 04/30/2018
ms.author: jroth
ms.custom: include file
ms.openlocfilehash: 19be449528481b4e35cad4418f82f2250917966b
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
## <a name="prepare-for-akv-integration"></a>AKV tümleştirme için hazırlama
SQL Server VM'nize yapılandırmak için Azure anahtar kasası tümleştirmeyi kullanmak için birkaç önkoşul vardır: 

1. [Azure PowerShell'i yükleme](#install)
2. [Bir Azure Active Directory oluşturun](#register)
3. [Bir anahtar kasası oluşturma](#createkeyvault)

Aşağıdaki bölümlerde bu önkoşulları ve daha sonra PowerShell cmdlet'lerini çalıştırmak için toplamanız gereken bilgiler açıklanmaktadır.

### <a id="install"></a> Azure PowerShell'i yükleme
En son Azure PowerShell SDK'sı yüklediğinizden emin olun. Daha fazla bilgi için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azureps-cmdlets-docs).

### <a id="register"></a> Bir uygulamayı Azure Active Directory'yi kaydetme

İlk olarak, şunlara sahip olmanız gerekir bir [Azure Active Directory](https://azure.microsoft.com/trial/get-started-active-directory/) (AAD) aboneliğinizde. Birçok avantaj arasında anahtar kasanızı belirli kullanıcılar ve uygulamalar için izni olanak sağlar.

Ardından, bir uygulamanın AAD ile kaydedin. Bu, VM gerekir, anahtar kasanızı erişimi olan bir hizmet sorumlusu hesabı verir. Azure anahtar kasası makalede, bu adımları bulabilirsiniz [bir uygulamayı Azure Active Directory ile kaydetme](../articles/key-vault/key-vault-get-started.md#register) bölüm veya ekran görüntüleri ile adımları görebilirsiniz **uygulama bölümü için bir kimlik alma**  , [bu blog gönderisine](http://blogs.technet.com/b/kv/archive/2015/01/09/azure-key-vault-step-by-step.aspx). Bu adımları gerçekleştirmeden önce aşağıdaki bilgileri SQL VM üzerinde Azure anahtar kasası tümleştirmeyi etkinleştirdiğinizde, daha sonra gerekli bu kayıt sırasında toplamanız gerekir.

* Uygulama eklendikten sonra bulma **uygulama kimliği** üzerinde **kayıtlı uygulama** dikey.
    Uygulama Kimliği daha sonra atanan **$spName** Azure anahtar kasası tümleştirmeyi etkinleştirmek için PowerShell betiğini parametresinde (hizmet asıl adı).

   ![Uygulama Kimliği](./media/virtual-machines-sql-server-akv-prepare/aad-application-id.png)

* Anahtarınızı oluşturduğunuzda, bu adımları sırasında gizli anahtarınız için aşağıdaki ekran görüntüsünde gösterildiği gibi kopyalayın. Bu anahtar sırrı daha sonra atanan **$spSecret** PowerShell Betiği parametresinde (hizmet sorumlusu gizli).

   ![AAD gizli](./media/virtual-machines-sql-server-akv-prepare/aad-sp-secret.png)

* Uygulama Kimliğini ve parolasını SQL Server'da bir kimlik bilgisi oluşturmak için de kullanılır.

* Bu yeni istemci kimliği aşağıdaki erişim izinlerine sahip yetkilendirmeniz gerekir: **şifrelemek**, **şifresini**, **wrapKey**, **unwrapKey**, **oturum**, ve **doğrulayın**. Bu gerçekleştirilir [kümesi AzureRmKeyVaultAccessPolicy](https://msdn.microsoft.com/library/azure/mt603625.aspx) cmdlet'i. Daha fazla bilgi için bkz: [anahtar veya gizli kullanmak için uygulamayı yetkilendirmeniz](../articles/key-vault/key-vault-get-started.md#authorize).

### <a id="createkeyvault"></a> Bir anahtar kasası oluşturma
Azure anahtar kasası, VM şifreleme için kullanacağınız anahtarları depolamak için kullanmak için bir anahtar kasası erişim gerekir. Anahtar kasanız zaten ayarlamadıysanız içindeki adımları izleyerek oluşturun [Azure anahtar kasası ile çalışmaya başlama](../articles/key-vault/key-vault-get-started.md) makalesi. Bu adımları gerçekleştirmeden önce SQL VM üzerinde Azure anahtar kasası tümleştirmeyi etkinleştirdiğinizde, daha sonra gerekli yukarı bu kümesi sırasında toplamanız gereken bazı bilgileri yoktur.

    New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia'

Bir anahtar kasası adımı Oluştur aldığınızda, döndürülen Not **vaultUri** anahtar kasası URL'si özelliği. Aşağıda, anahtar kasası adıdır ContosoKeyVault, bu adımda sağlanan örnekte bu nedenle anahtar kasası URL'si olacaktır https://contosokeyvault.vault.azure.net/.

Anahtar kasası URL'si daha sonra atanan **$akvURL** Azure anahtar kasası tümleştirmeyi etkinleştirmek için PowerShell komut parametresi.

Anahtar kasası oluşturulur, anahtar Kasası'na bir anahtar eklemek ihtiyacımız, biz oluşturduğunuzda, bu anahtarı anılacaktır sonra bir asimetrik anahtar oluşturma SQL Server'da daha sonra.
