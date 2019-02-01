---
title: Office 365 grupları - Azure Active Directory için adlandırma ilkesi hızlı Ekle | Microsoft Docs
description: Azure Active Directory'de yeni kullanıcıların eklenmesini var olan kullanıcıların silinmesini açıklar
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 01/31/2019
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: it-pro
ms.openlocfilehash: 43ce034f2960f31d614636c77e8f30f05e8aa41e
ms.sourcegitcommit: fea5a47f2fee25f35612ddd583e955c3e8430a95
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55507120"
---
# <a name="quickstart-naming-policy-for-groups-in-azure-active-directory"></a>Hızlı Başlangıç: Azure Active Directory'de gruplar için adlandırma ilkesi

Bu hızlı başlangıçta kiracınızın gruplarını sıralama ve arama konusunda yardımcı olması amacıyla Azure Active Directory (Azure AD) kiracınızda kullanıcı tarafından oluşturulan Office 365 grupları için adlandırma ilkesini ayarlayacaksınız. Adlandırma ilkesini aşağıdaki gibi amaçlar için kullanabilirsiniz:

* Grubun işlevini, üyelerini, coğrafi bölgesini veya grubu oluşturan kişiyi paylaşma.
* Adres defterindeki grupların kategorilere ayrılmasına yardımcı olma.
* Belirli sözcüklerin grup adlarında ve diğer adlarında kullanılmasını engelleme.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="install-powershell-cmdlets"></a>PowerShell cmdlet'lerini yükleme

PowerShell komutlarını çalıştırmadan önce Windows PowerShell Graph için Azure Active Directory PowerShell Modülünün eski sürümlerini kaldırın ve [Graph için Azure Active Directory PowerShell - Genel Önizleme Sürümünü 2.0.0.137](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137) yükleyin. 

1. Windows PowerShell uygulamasını yönetici olarak açın.
2. Eski AzureADPreview sürümlerini kaldırın.
  
  ```
  Uninstall-Module AzureADPreview
  ```
3. En son AzureADPreview sürümünü yükleyin.
  
  ```
  Install-Module AzureADPreview
  ```
Güvenilmeyen depoya erişmek isteyip istemediğiniz sorulursa **Y** tuşuna basın. Yeni modülün yüklenmesi birkaç dakika sürebilir.

## <a name="set-up-naming-policy"></a>Adlandırma ilkesini ayarlama

### <a name="step-1-sign-in-using-powershell-cmdlets"></a>1. Adım: PowerShell cmdlet'lerini kullanarak oturum açın

1. Windows PowerShell uygulamasını açın. Yükseltilmiş ayrıcalık gerekmez.

2. Ortamı cmdlet'leri çalıştırmaya hazır hale getirmek için aşağıdaki komutları çalıştırın.
  
  ```
  Import-Module AzureADPreview
  Connect-AzureAD
  ```
  Açılan **Hesabınızda oturum açın** ekranında hizmetinizle bağlantı kurmak için yönetici hesabınızın adını ve parolasını girin **Oturum aç**'ı seçin.

3. Bu kiracının grup ayarlarını oluşturmak için [Grup ayarlarını yapılandırmak için Azure Active Directory cmdlet'leri](groups-settings-cmdlets.md) adımlarını izleyin.

### <a name="step-2-view-the-current-settings"></a>2. Adım: Geçerli ayarları görüntüleyebilir

1. Geçerli adlandırma ilkesi ayarlarını görüntüleyin.
  
  ```
  $Setting = Get-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | where -Property DisplayName -Value "Group.Unified" -EQ).id
  ```
  
2. Geçerli grup ayarlarını görüntüleyin.
  
  ```
  $Setting.Values
  ```
  
### <a name="step-3-set-the-naming-policy-and-any-custom-blocked-words"></a>3. Adım: Adlandırma ilkesini ve herhangi bir özel engellenen sözcük ayarlayın

1. Azure AD PowerShell'de grup adı ön ve son eklerini ayarlayın. Düzgün bir şekilde çalışması için özelliği için [GroupName] ayarı eklenmesi gerekir.
  
  ```
  $Setting["PrefixSuffixNamingRequirement"] =“GRP_[GroupName]_[Department]"
  ```
  
2. Sınırlamak istediğiniz özel engellenen sözcükleri belirleyin. Aşağıdaki örnekte kendi özel sözcüklerinizi ekleme adımları gösterilmektedir.
  
  ```
  $Setting["CustomBlockedWordsList"]=“Payroll,CEO,HR"
  ```
  
3. Yeni ilkenin etkili olması için aşağıdaki örnekte gösterilen şekilde ayarları kaydedin.
  
  ```
  Set-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | where -Property DisplayName -Value "Group.Unified" -EQ).id -DirectorySetting $Setting
  ```
  
İşte bu kadar. Adlandırma ilkenizi ayarladınız ve özel engellenen sözcüklerinizi eklediniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

1. Grup adı önek ve sonek Azure AD PowerShell'de boş.
  
  ```
  $Setting["PrefixSuffixNamingRequirement"] =""
  ```
  
2. Özel engellenen sözcük boş.
  
  ```
  $Setting["CustomBlockedWordsList"]=""
  ```
  
3. Ayarları kaydedin.
  
  ```
  Set-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | where -Property DisplayName -Value "Group.Unified" -EQ).id -DirectorySetting $Setting
  ```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta Azure AD kiracınız için adlandırma ilkesini ayarlama amacıyla PowerShell cmdlet'lerini kullanmayı öğrendiniz.

Teknik kısıtlamalar, özel engellenen sözcük listesi ekleme veya Office 365 uygulamalarındaki son kullanıcı deneyimleri hakkında daha fazla bilgi için adlandırma ilkesiyle ilgili ayrıntılı bilgilerin verildiği bir sonraki makaleye geçin.
> [!div class="nextstepaction"]
> [Adlandırma ilkesiyle ilgili tüm ayrıntılar](groups-naming-policy.md)