Bir koşul eklediğiniz, bir şeyler için kendi zamanı tetik tarafından oluşturulan verilerle ilginç. Eklemek için aşağıdaki adımları izleyin **Salesforce - Get nesne** eylem. Bu eylem verileri yeni bir sağlama oluşturulan her zaman alır. Ayrıca, Salesforce - Office 365 Bağlayıcısı'nı kullanarak bir e-posta göndermek için bir nesne eylemi Get verilerden kullanır ikinci bir eylem ekleyeceksiniz.  

Bu eylem, aşağıdaki bilgileri sağlamanız gerekir. Yeni dosya özelliklerinden bazıları için giriş olarak tetik tarafından oluşturulan kullanımı kolay veri olduğunu fark edersiniz:

| Dosya özelliği oluşturma | Açıklama |
| --- | --- |
| Nesne türü |İlgilendiğiniz Salesforce nesnesinin türü budur. Örnekler sağlama, hesap, vs. |
| Nesne Kimliği |Bu nesne için bir tanımlayıcıyı temsil eder. |

1. Seçin **Eylem Ekle** bağlantı. Bu açılır, herhangi bir işlem arayabileceğiniz arama kutusu yapmak istiyorsunuz. Bu örnekte, Salesforce ilgi eylemlerdir.      
   ![Salesforce eylem görüntü 1](./media/connectors-create-api-salesforce/action-1.png)  
2. Girin *salesforce* salesforce ilgili eylemler için aranacak.
3. Seçin **Salesforce - Get nesne** yapılacak eylem olarak.   **Not**: mantıksal uygulamanızı, bunu daha önceden yapmadıysanız, Salesforce hesabınıza erişmeniz için yetkilendirmek istenir.    
   ![Salesforce eylem görüntü 2](./media/connectors-create-api-salesforce/action-2.png)    
4. **Get nesne** kontrol açar.  
5. Seçin *neden* nesne türü.
6. Seçin **nesne kimliği** denetim.
7. Seçin **...**  giriş olarak Eylemler için kullanılabilir belirteçleri listesini genişletin.       
   ![Salesforce eylem görüntüsü 3](./media/connectors-create-api-salesforce/action-3.png)    
8. Seçin **neden kimliği** kontrol açar.   
   ![Salesforce eylem görüntüsü 4](./media/connectors-create-api-salesforce/action-4.png)     
9. Sağlama kimliği belirteci şimdi bu mantıksal uygulama tetiklenen sağlama sağlama Kimliğine eşit olan bir Kimliğe sahip bir sağlama Get nesne eylemi arar gösteren nesne kimliği denetiminde olduğuna dikkat edin.  
   ![Salesforce eylem görüntüsü 5](./media/connectors-create-api-salesforce/action-5.png)  
10. Çalışmanızı kaydedin. İşte bu kadar mantıksal uygulamanızı Get nesne eylemi eklendi. Get nesne denetimi aşağıdaki gibi görünmelidir:    
    ![Salesforce eylem görüntüsü 6](./media/connectors-create-api-salesforce/action-6.png)  

Bir sağlama almak için bir eylem eklediğiniz, yeni oluşturulan sağlama ile ilgi çekici bir şey yapmak isteyebilirsiniz. Kuruluş, yeni sağlama oluşturulduğunu belirten bir dağıtım listesi bildiren bir e-posta göndermek isteyebilirsiniz. Şimdi Salesforce içindeki yeni sağlama nesnesinden bazı ilgili bilgiler içeren bir e-posta göndermek için Office 365 Bağlayıcısı'nı kullanın.  

1. Seçin **Eylem Ekle** enter *e-posta* arama denetiminde. Bu, e-posta gönderip için ilgili eylemleri filtreler.  
2. Seçin **Office 365 Outlook - bir e-posta Gönder** liste öğesi. Zaten oluşturmadıysanız bir *bağlantı* Office 365 hesabınıza bunu şimdi oluşturmak için Office 365 kimlik bilgilerinizi girmeniz istenir. Tamamlandıktan sonra **bir e-posta Gönder** kontrol açar.        
   ![Salesforce eylem görüntüsü 7](./media/connectors-create-api-salesforce/action-7.png)  
3. E-posta içeri göndermek istediğiniz e-posta adresi girin **için** denetim.
4. İçinde **konu** denetlemek, girin *yeni oluşturulan neden* - seçip *şirket* belirteci. Bu görüntüler *şirket* alanının Salesforce içinde oluşturulan yeni sağlama.  
5. İçinde **gövde** denetim, yeni sağlama nesnesinden belirteçleri seçebilirsiniz ve e-posta gövdesinde görüntülemek istediğiniz herhangi bir metin de girebilirsiniz. Örnek aşağıda verilmiştir:  
   ![Salesforce eylem görüntüsü 8](./media/connectors-create-api-salesforce/action-8.png)   
6. İş akışınızı kaydedin.  

Bu kadar. Mantıksal uygulamanızı tamamlanmıştır.  

Artık, mantıksal uygulamanızı test edebilirsiniz: Salesforce içinde oluşturduğunuz koşulunu sağlayan yeni bir sağlama oluşturun.  Bu kılavuz tam olarak izlediyseniz, müşteri adayı içeren bir e-posta adresiyle yalnızca oluşturma *amazon.com* da. Birkaç saniye sonra mantıksal uygulamanızı tetiklenmesi ve sonuçlar aşağıdakine benzer görünür:  
![Salesforce eylem görüntü 9](./media/connectors-create-api-salesforce/action-9.png)  

