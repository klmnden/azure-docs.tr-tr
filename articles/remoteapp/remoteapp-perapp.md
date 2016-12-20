---
title: "Azure RemoteApp koleksiyonunda (Önizleme) bireysel kullanıcılara uygulama yayımlama | Microsoft Belgeleri"
description: "Azure RemoteApp’te, kullanıcılara bağlı olmak yerine, bireysel kullanıcılara uygulama yayımlamayı öğrenin."
services: remoteapp-preview
documentationcenter: 
author: piotrci
manager: mbaldwin
editor: 
ms.assetid: 1fd0539d-fa65-4ea5-a98e-0be0cf580690
ms.service: remoteapp
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: compute
ms.date: 11/23/2016
ms.author: piotrci
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 6dcadbfb99d4d111ab9ddde9d74db65b5542a8f5


---
# <a name="publish-applications-to-individual-users-in-an-azure-remoteapp-collection-preview"></a>Azure RemoteApp koleksiyonunda (Önizleme) bireysel kullanıcılara uygulama yayımlama
> [!IMPORTANT]
> Azure RemoteApp kullanımdan kaldırılıyor. Ayrıntılı bilgi için [duyuruyu](https://go.microsoft.com/fwlink/?linkid=821148) okuyun.
> 
> 

Bu makale Azure RemoteApp koleksiyonunda (Önizleme) bireysel kullanıcılara uygulama yayımlama | Microsoft Azure Bu, Azure RemoteApp'in yeni bir işlevidir; şu anda özel önizlemededir ve yalnızca değerlendirme amacıyla önceden seçilen kullanıcılar tarafından kullanılabilir.

Başlangıçta Azure RemoteApp yalnızca uygulama yayımlama için tek yönlü etkinleştirilmiştir. Yönetim görüntüden uygulama yayımlayabilir ve bunlar koleksiyondaki tüm kullanıcılara görünür olur.

Genel senaryo yönetim maliyetlerini azaltmak için tek bir görüntüde birden çok uygulama içerir ve bir koleksiyon dağıtır. Çoğu kez tüm uygulamalar bütün kullanıcılarla ilgili değildir; yöneticiler kendi uygulama akışlarında gereksiz uygulama görmemeleri için uygulamalarını bireysel kullanıcılara yayımlamak isteyebilir.

Bu artık Azure RemoteApp’te şu anda sınırlı bir önizleme özelliği olarak mümkündür. Yeni işlevin kısa bir özeti aşağıda verilmiştir:

1. Bir koleksiyon iki moddan birini ayarlanabilir:
   
   * özgün koleksiyon modu bir koleksiyondaki tüm kullanıcıların yayımlanan tüm uygulamaları görebildikleri moddur. Bu varsayılan moddur.
   * yeni uygulama modu kullanıcıların yalnızca özel olarak kendilerine atanmış uygulamaları görebildikleri moddur.
2. Şu anda uygulama modu yalnızca Azure RemoteApp PowerShell cmdlet'leri kullanarak etkinleştirilebilir.
   
   * Uygulama modu ayarlandığında, koleksiyondaki atama Azure portal aracılığıyla yönetilemez. Kullanıcı ataması PowerShell cmdlet’leri aracılığıyla yönetilmelidir.
3. Kullanıcılar yalnızca kendilerine doğrudan yayımlanan uygulamaları görür. Ancak, yine de bir kullanıcının işletim sisteminde bunlara doğrudan erişerek görüntüde bulunan diğer uygulamaları başlatması mümkündür.
   
   * Bu özellik uygulamalara güvenli kilitleme özelliği sağlamak; yalnızca uygulama akışındaki görünürlüğü sınırlar.
   * Uygulamaları kullanıcılardan yalıtmanız gerekiyorsa, bunun için ayrı koleksiyonlar için kullanmanız gerekecektir.

## <a name="how-to-get-azure-remoteapp-powershell-cmdlets"></a>Azure RemoteApp PowerShell cmdlet'lerini alma
Yeni önizleme işlevini denemek için, Azure PowerShell cmdlet'lerini kullanmanız gerekecektir. Şu anda yeni uygulama yayımlama modunu etkinleştirmek için Azure Yönetim portalını kullanmak mümkün değildir.

İlk olarak, [Azure PowerShell modülü](/powershell/azureps-cmdlets-docs) yüklü olduğundan emin olun.

Daha sonra, PowerShell konsolunu yönetici modunda başlatın ve aşağıdaki cmdlet'i çalıştırın:

        Add-AzureAccount

Sizden Azure kullanıcı adı ve parola bilgilerini ister. Oturum açıldıktan sonra, Azure aboneliklerinize karşı Azure RemoteApp cmdlet'lerini çalıştırabileceksiniz.

## <a name="how-to-check-which-mode-a-collection-is-in"></a>Bir koleksiyonun hangi modda olduğunu anlama
Aşağıdaki cmdlet'i çalıştırın:

        Get-AzureRemoteAppCollection <collectionName>

![Koleksiyonu modunu kontrol etme](./media/remoteapp-perapp/araacllelvel.png)

AclLevel özelliği aşağıdaki değerlere sahip olabilir:

* Koleksiyon: özgün yayımlama modu. Tüm kullanıcılar yayımlanan bütün uygulamaları görür.
* Uygulama: yeni yayımlama modu Kullanıcılar yalnızca kendilerine doğrudan yayımlanan uygulamaları görür.

## <a name="how-to-switch-to-application-publishing-mode"></a>Uygulama yayımlama moduna geçiş yapma
Aşağıdaki cmdlet'i çalıştırın:

        Set-AzureRemoteAppCollection -CollectionName -AclLevel Application

Uygulama yayımlama durumu muhafaza edilir: başlangıçta tüm kullanıcılar özgün yayımlanan bütün uygulamaları görür.

## <a name="how-to-list-users-who-can-see-a-specific-application"></a>Belirli bir uygulamayı görebilen kullanıcıları listeleme
Aşağıdaki cmdlet'i çalıştırın:

        Get-AzureRemoteAppUser -CollectionName <collectionName> -Alias <appAlias>

Bu, uygulamayı görebilen tüm kullanıcıları listeler.

Not: Get-AzureRemoteAppProgram -CollectionName <collectionName>’i çalıştırarak uygulama diğer adlarını (yukarıdaki söz diziminde “app alias” adı verilir) verilir.

## <a name="how-to-assign-an-application-to-a-user"></a>Kullanıcıya bir uygulama atama
Aşağıdaki cmdlet'i çalıştırın:

        Add-AzureRemoteAppUser -CollectionName <collectionName> -UserUpn <user@domain.com> -Type <OrgId|MicrosoftAccount> -Alias <appAlias>

Kullanıcı artık uygulamayı Azure RemoteApp istemcisinde görür ve uygulamaya bağlanabilir

## <a name="how-to-remove-an-application-from-a-user"></a>Kullanıcıdan bir uygulamayı kaldırma
Aşağıdaki cmdlet'i çalıştırın:

        Remove-AzureRemoteAppUser -CollectionName <collectionName> -UserUpn <user@domain.com> -Type <OrgId|MicrosoftAccount> -Alias <appAlias>

## <a name="providing-feedback"></a>Geribildirim sağlama
Bu önizleme özelliğiyle ilgili teşekkür ver önerileriniz için teşekkür ederiz. Lütfen düşündüklerinizi paylaşmak için [anketi](http://www.instant.ly/s/FDdrb) doldurun.

## <a name="havent-had-a-chance-to-try-the-preview-feature"></a>Önizleme özelliğini deneme şansınız olmadı mı?
Henüz önizlemeye katılmadıysanız, lütfen erişim isteğinden bulunmak için bu [anketi](http://www.instant.ly/s/AY83p) doldurun.




<!--HONumber=Dec16_HO1-->


