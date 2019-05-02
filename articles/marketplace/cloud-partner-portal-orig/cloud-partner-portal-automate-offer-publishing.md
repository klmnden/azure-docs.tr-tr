---
title: Teklif yayımlamak | Azure Market
description: Program aracılığıyla sanal makine yayımlama iş akışını nasıl otomatikleştireceğinizi açıklar.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pabutler
ms.openlocfilehash: 0a927c72a82c6aa3c79988c599ea8b840821a2b8
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64935896"
---
<a name="automate-offer-publishing"></a>Teklif yayımlamak
=========================

İş akışı yayımlandığında, API'leri kullanarak VM ayrıca programlı bir şekilde otomatik hale getirebilirsiniz [API Başvurusu](./cloud-partner-portal-api-overview.md) bölümü. Otomasyon planlarken dikkate alınması gereken iki farklı senaryo vardır: ilk yayımlama ve sonraki teklifi yayımlama sunar.


<a name="offer-initial-publishing"></a>İlk Yayımlama sunar
-------------------------

Bir teklif ilk kez yayımladığınızda, Market'te karşıya yüklemeden önce bazı ek adımlar gerektirir.  Örneğin, meta veri hazırlama ve bir teklif taslağı oluşturmanız gerekir. İlk Yayımlama iş akışı aşağıdaki diyagramda gösterilmiştir.

![Bir ilk etkileşimlerinin Yayını sunar.](media/cloud-partner-portal-automate-offer-publishing/first-time-offer-publishing.png)

Aşağıdaki örnek kod, bu adımları gösterir.

``` csharp
  CreateOfferAndPublish()
  {
      offer = CreateOfferObject()
      // Set all offer attributes
      offer.offerTypeId = "microsoft-azure-virtualmachines",
      // Create offer on server
     result = CloudPartnerPortal.Client.PutOffer(offer);
      if(result.status != success)
      {
        // Do failure actions here
      }
      // Publish created offer
      Operation op = CloudPartnerPortal.Client.Publish(offer.offerName);
      if(op.HTTPStatus != OK)
      {
        /*Queuing the publish failed, check error message and dofailure actions*/
        return failure;
      }
      // Return success as operation is successfully queued
      return success;
  }

  ValidateAndGoLive()    
  {
      // Confirm the version in preview slot is the version that needs to go live
      offer = CloudPartnerPortal.Client.GetOffer(offerName, “Preview”);
      if(!offer[skuName].containsVersion(VMDisk.Version))
      {
          UpdateOfferAndPublish()
      }
      // Go Live on offer and check result
      Operation op = CloudPartnerPortal.Client.Publish(offer.offerName);
      if(op.HTTPStatus != OK)
      {
          // Queuing the publish failed, check error message and do failure actions
          return failure;
      }  
     
      // Return success as operation is successfully queued
      return success;
  }
```


<a name="subsequent-offer-publishing"></a>Sonraki teklifi yayımlama
---------------------------

Sanal makine (VM) teklifi sürekli tümleştirme işlem hattı'nda tümleştirildiğinde yayımlama iş akışının yeni bir sanal sabit disk (VHD) oluşturulduğunda çalışmasını otomatik hale getirebilirsiniz.  Bu iş akışı aşağıdaki diyagramda ve örnek kodda gösterilmiştir.

![Etkileşimler sonraki teklif yayını](media/cloud-partner-portal-automate-offer-publishing/update-offer-and-publish.png)

``` csharp
    UpdateOfferAndPublish()
    {
        // The routine wakes up after a build finishes. 
        // It determines if the current offer is undergoing publishing and take appropriate action
        // Wake up and get draft offer using offer name
        offer = CloudPartnerPortal.Client.GetOffer(offerName);
        if(!offer[skuName].containsVersion(VMDisk.Version))
        {
                // Update the list of VMs with new virtual machine. 
                // This assumes that less than 8 disks are present in the SKU
                // If 8 disks are already present, the client must remove one of the previous disks before adding new disk
                offer[skuName].addDisk(VMDisk)
                // Update the offer on server with new VMDisk
                CloudPartnerPortal.Client.PutOffer(offer);
        }
        // Publish offer and check result. Publish is idempotent, so if a publish was already in progress with latest version, this would return success.
        Operation op = CloudPartnerPortal.Client.Publish(offer.offerName);
        if(op.HTTPStatus != OK)
        {
              // Queuing the publish failed, check error message and do failure actions
              return failure;
        }
        
        // Return success as operation is successfully queued
        return success;
    }

    CheckLastOperationStatus()
    {
        Operation op = CloudPartnerPortal.Client.GetLastOpertation(offer.offerName)
        if(op.status == completed)
        {
            return success;
        }
        if(op.status == failure)
           // Do failure actions here
        
        else 
           // Last operation is still running
           // Go to sleep.           
    }

    ValidateAndGoLive()
    {
        // Confirm the version in preview slot is the version that needs to go live
        offer = CloudPartnerPortal.Client.GetOffer(offerName, “Preview”);
        if(!offer[skuName].containsVersion(VMDisk.Version))
        {
            UpdateOfferAndPublish()
        }
        // Go Live on offer and check result
        Operation op = CloudPartnerPortal.Client.Publish(offer.offerName);
        if(op.HTTPStatus != OK)
        {
              // Queuing the publish failed, check error message and do failure actions
              return failure;
        }
        // Return success as operation is successfully queued
        return success;
    }
```
