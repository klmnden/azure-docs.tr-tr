# [Azure Active Directory Belgeleri](index.md)

# Genel Bakış
## [Azure Active Directory nedir?](fundamentals/active-directory-whatis.md)
## [Azure kimlik yönetimi hakkında](fundamentals/identity-fundamentals.md)
## [Azure kimlik çözümlerini anlama](fundamentals/understand-azure-identity-solutions.md)
## [Karma kimlik çözümü seçin](choose-hybrid-identity-solution.md)
## [Azure aboneliklerini ilişkilendirme](fundamentals/active-directory-how-subscriptions-associated-directory.md)
## [Yerleşim ve verilerle ilgili dikkat edilecek konular](fundamentals/active-directory-data-storage-eu.md)
## [SSS](fundamentals/active-directory-faq.md)
## [Yenilikler](fundamentals/whats-new.md)


# başlarken
## [Azure AD’yi kullanmaya başlama](fundamentals/get-started-azure-ad.md)
## [Azure AD Premium’a kaydolun](active-directory-get-started-premium.md)
## [Özel bir etki alanı adı ekleme](fundamentals/add-custom-domain.md)
## [Şirket markası yapılandırma](fundamentals/customize-branding.md)
## [Kullanıcıları Azure AD’ye ekleme](fundamentals/add-users-azure-active-directory.md)
## [Kullanıcılara lisans atama](fundamentals/license-users-groups.md)
## [Self servis parola sıfırlamasını yapılandırma](authentication/quickstart-sspr.md)
## [Azure AD’ye kuruluşunuzun gizlilik bilgilerini ekleme](active-directory-properties-area.md)


# Nasıl yapılır
## Planlama ve tasarım
### [Azure AD mimarisini anlama](fundamentals/active-directory-architecture.md)
### [Azure Active Directory’de talep eşlemesi](active-directory-claims-mapping.md)
### [Karma kimlik çözümü dağıtma](active-directory-hybrid-identity-design-considerations-overview.md)
#### Gereksinimlerini belirleme
##### [Kimlik](active-directory-hybrid-identity-design-considerations-business-needs.md)
##### [Dizin eşitlemesi](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)
##### [Multi-Factor Authentication](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)
##### [Kimlik yaşam döngüsü stratejisi](active-directory-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md)
#### [Veri güvenliği planlama](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md)
##### [Veri koruma](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md)
##### [İçerik yönetimi](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)
##### [Erişim denetimi](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)
##### [Olay yanıtı](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md)
#### Kimlik yaşam döngünüzü planlama
##### [Görevler](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)
##### [Benimseme stratejisi](active-directory-hybrid-identity-design-considerations-identity-adoption-strategy.md)
#### [Sonraki adımlar](active-directory-hybrid-identity-design-considerations-nextsteps.md)
#### [Araç karşılaştırması](active-directory-hybrid-identity-design-considerations-tools-comparison.md)

## Kullanıcıları yönetme
### [Azure AD’ye yeni kullanıcı ekleme](fundamentals/add-users-azure-active-directory.md)
### [Kullanıcı profillerini yönetme](fundamentals/active-directory-users-profile-azure-portal.md)
### [Hesapları paylaşma](active-directory-sharing-accounts.md)
### [Kullanıcıları yönetici rollerine atama](fundamentals/active-directory-users-assign-role-azure-portal.md)
### [Silinen bir kullanıcıyı geri yükleme](fundamentals/active-directory-users-restore.md)
### [Başka bir dizinden konuk kullanıcılar ekleme (B2B)](b2b/what-is-b2b.md)
#### [B2B kullanıcıları ekleyen yöneticiler](b2b/add-users-administrator.md)
#### [B2B kullanıcıları ekleyen bilgi çalışanları](b2b/add-users-information-worker.md)
#### [API ve özelleştirme](b2b/customize-invitation-api.md)
#### [Kod ve Azure PowerShell örnekleri](b2b/code-samples.md)
#### [Self servis kaydolma portal örneği](b2b/self-service-portal.md)
#### [Davet e-postası](b2b/invitation-email-elements.md)
#### [Davet kullanma](b2b/redemption-experience.md)
#### [Davetiye olmadan B2B kullanıcıları ekleme](b2b/add-user-without-invite.md)
#### [Davetlere izin verme veya davetleri engelleme](b2b/allow-deny-list.md)
#### [B2B için koşullu erişim](b2b/conditional-access.md)
#### [B2B paylaşım ilkeleri](b2b/delegate-invitations.md)
#### [Bir role B2B kullanıcısı ekleme](b2b/add-guest-to-role.md)
#### [Dinamik gruplar ve B2B kullanıcıları](b2b/use-dynamic-groups.md)
#### [Kuruluştan ayrılma](b2b/leave-the-organization.md)
#### [Denetim ve raporlar](b2b/auditing-and-reporting.md)
#### [Hibrit kuruluşlar için B2B](b2b/hybrid-organizations.md)
##### [B2B kullanıcılarına yerel uygulama erişimi verme](b2b/hybrid-cloud-to-on-premises.md)
##### [Yerel kullanıcılara bulut uygulamaları erişimi verme](b2b/hybrid-on-premises-to-cloud.md)
#### [B2B ve Office 365 dış paylaşım](b2b/o365-external-user.md)
#### [B2B lisansı](b2b/licensing-guidance.md)
#### [Geçerli sınırlamalar](b2b/current-limitations.md)
#### [SSS](b2b/faq.md)
#### [B2B sorunlarını giderme](b2b/troubleshoot.md)
#### [B2B kullanıcısını anlama](b2b/user-properties.md)
#### [B2B kullanıcı belirteci](b2b/user-token.md)
#### [Azure AD tümleşik uygulamaları için B2B](b2b/configure-saas-apps.md)
#### [B2B kullanıcı taleplerini eşleme](b2b/claims-mapping.md)
#### [B2B işbirliğini B2C ile karşılaştırma](b2b/compare-with-b2c.md)
#### [B2B desteği alma](b2b/get-support.md)

## [Grupları ve üyeleri yönetme](fundamentals/active-directory-manage-groups.md)
### Grupları yönetme
#### [Azure Portal](active-directory-groups-create-azure-portal.md)
#### [Graph için Azure AD PowerShell (v2)](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)
#### [Azure AD PowerShell MSOnline](active-directory-accessmanagement-groups-settings-cmdlets.md)
### [Grup üyelerini yönetme](active-directory-groups-members-azure-portal.md)
### [Grup sahiplerini yönetme](fundamentals/active-directory-accessmanagement-managing-group-owners.md)
### [Grup üyeliğini yönetme](fundamentals/active-directory-groups-membership-azure-portal.md)
### [Grupları kullanarak lisansları atama](fundamentals/active-directory-licensing-whatis-azure-portal.md)
#### [Bir gruba lisans atama](active-directory-licensing-group-assignment-azure-portal.md)
#### [Bir gruptaki lisans sorunlarını tanımlama ve çözme](active-directory-licensing-group-problem-resolution-azure-portal.md)
#### [Tek tek lisanslı kullanıcıları grup tabanlı lisanslamaya geçirme](active-directory-licensing-group-migration-azure-portal.md)
#### [Kullanıcıları ürün lisansları arasında geçirme](active-directory-licensing-group-product-migration.md)
#### [Grup tabanlı lisanslama için ek senaryolar](active-directory-licensing-group-advanced.md)
#### [Grup tabanlı lisanslama için Azure PowerShell örnekleri](active-directory-licensing-ps-examples.md)
#### [Azure AD’de ürünler ve hizmet planları için başvurular](active-directory-licensing-product-and-service-plan-reference.md)
### [Office 365 gruplarının süre sonlarını ayarlama](active-directory-groups-lifecycle-azure-portal.md)
### [Gruplar için bir adlandırma ilkesini zorlama](groups-naming-policy.md)
### [Tüm grupları görüntüleme](fundamentals/active-directory-groups-view-azure-portal.md)
### [SaaS uygulamalarına grup erişimi ekleme](active-directory-accessmanagement-group-saasapps.md)
### [Silinen bir Office 365 grubunu geri yükleme](fundamentals/active-directory-groups-restore-azure-portal.md)
### [Grup ayarlarını yönetme](fundamentals/active-directory-groups-settings-azure-portal.md) 
### Gelişmiş kurallar oluşturma
#### [Azure Portal](active-directory-groups-dynamic-membership-azure-portal.md)
### [Self servis gruplarını kurma](active-directory-accessmanagement-self-service-group-management.md)
### [Sorun giderme](active-directory-accessmanagement-troubleshooting.md)

## [Raporları yönetme](active-directory-reporting-azure-portal.md)
### [Oturum açma etkinliği](active-directory-reporting-activity-sign-ins.md)
### [Denetim etkinliği](active-directory-reporting-activity-audit-logs.md)
### [Risk altındaki kullanıcılar](active-directory-reporting-security-user-at-risk.md)
### [Riskli oturum açma işlemleri](active-directory-reporting-security-risky-sign-ins.md)
### [Risk olayları](active-directory-reporting-risk-events.md)
### [SSS](active-directory-reporting-faq.md)
### Görevler
#### [Adlandırılmış konumları yapılandırma](active-directory-named-locations.md)
#### [Etkinlik raporlarını bulma](active-directory-reporting-migration.md)
#### [Azure Active Directory Power BI İçerik Paketi’ni kullanma](active-directory-reporting-power-bi-content-pack-how-to.md)
#### [Riskli olduğu belirlenen kullanıcıları düzeltme](active-directory-report-security-user-at-risk-remediation.md)
### Başvuru
#### [Bekletme](active-directory-reporting-retention.md)
#### [Gecikmeler](active-directory-reporting-latencies-azure-portal.md)
#### [Bildirimler](active-directory-reporting-notifications.md)
#### [Denetim etkinliği başvurusu](active-directory-reporting-activity-audit-reference.md)
#### [Oturum açma etkinliği hata kodları](active-directory-reporting-activity-sign-ins-errors.md)
#### [Multi-Factor Authentication](active-directory-reporting-activity-sign-ins-mfa.md)



### Sorun giderme
#### [Eksik denetim verileri](active-directory-reporting-troubleshoot-missing-audit-data.md)
#### [İndirmelerde eksik veriler](active-directory-reporting-troubleshoot-missing-data-download.md)
#### [Azure Active Directory Etkinlik günlükleri içerik paketi hataları](active-directory-reporting-troubleshoot-content-pack.md)
### [Programlı Erişim](active-directory-reporting-api-getting-started-azure-portal.md)
#### [Önkoşullar](active-directory-reporting-api-prerequisites-azure-portal.md)
#### [Denetim örnekleri](active-directory-reporting-api-audit-samples.md)
#### [Oturum açma örnekleri](active-directory-reporting-api-sign-in-activity-samples.md)
#### [Sertifikaları kullanma](active-directory-reporting-api-with-certificates.md)

## Parolaları yönetme
### [Parolalara genel bakış](authentication/active-directory-passwords-overview.md)
### Kullanıcı belgeleri
#### [Parolanızı sıfırlama veya değiştirme](active-directory-passwords-update-your-own-password.md)
#### [Parolalarla ilgili en iyi yöntemler](active-directory-secure-passwords.md)
#### [Self servis parola sıfırlama için kaydolma](active-directory-passwords-reset-register.md)
### [SSPR Nasıl çalışır?](authentication/concept-sspr-howitworks.md)
### [SSPR Dağıtım kılavuzu](authentication/howto-sspr-deployment.md)
### [SSPR ve Windows 10](authentication/tutorial-sspr-windows.md)
### [SSPR İlkeleri ](authentication/concept-sspr-policy.md)
### [SSPR’yi Özelleştirme](authentication/concept-sspr-customization.md)
### [SSPR Veri gereksinimleri](authentication/howto-sspr-authenticationdata.md)
### [SSPR Raporlama](authentication/howto-sspr-reporting.md)
### BT Yöneticileri: Parolaları sıfırlama
#### [Azure Portal](fundamentals/active-directory-users-reset-password-azure-portal.md)
### [SSPR lisanslama](authentication/concept-sspr-licensing.md)
### [Parola geri yazma](authentication/howto-sspr-writeback.md)
### [Sorun giderme](authentication/active-directory-passwords-troubleshoot.md)
### [SSS](authentication/active-directory-passwords-faq.md)


## Cihazları yönetme
### [Giriş](device-management-introduction.md)
### [Azure portalını kullanma](device-management-azure-portal.md)
### [Azure AD'ye Katılım’ı Planlama](active-directory-azureadjoin-deployment-aadjoindirect.md)
### [SSS](device-management-faq.md)
### Görevler
#### [Azure AD alanında kayıtlı Windows 10 cihazlarını ayarlama](device-management-azuread-registered-devices-windows10-setup.md)
#### [Azure AD alanına katılmış cihazları ayarlama](device-management-azuread-joined-devices-setup.md)
#### [Karma Azure AD alanına katılmış cihazları ayarlama](device-management-hybrid-azuread-joined-devices-setup.md)
#### [Şirket içi dağıtma](active-directory-device-registration-on-premises-setup.md)
#### [Azure AD katılımı sırasında Windows 10’u ilk kez çalıştırma deneyimi](device-management-azuread-joined-devices-frx.md)
### Sorun giderme
#### [Karma Azure AD alanına katılmış Windows 10 ve Windows Server 2016 cihazları](device-management-troubleshoot-hybrid-join-windows-current.md)
#### [Karma Azure AD alanına katılmış eski Windows cihazları](device-management-troubleshoot-hybrid-join-windows-legacy.md)

## Uygulamaları yönetme
### [Genel Bakış](manage-apps/what-is-application-management.md)
### [Başlarken](manage-apps/plan-an-application-integration.md)
### [SaaS uygulama tümleştirmesi öğreticileri](active-directory-saas-tutorial-list.md)
### [Cloud App Discovery](manage-apps/cloud-app-discovery.md)
#### [Anlık görüntü raporları oluşturma](manage-apps/cloud-app-discovery-create-snapshot-reports.md)
#### [Sürekli raporlamayı yapılandırma](https://docs.microsoft.com/cloud-app-security/discovery-docker)
#### [Özel günlük ayrıştırıcı kullanma](https://docs.microsoft.com/cloud-app-security/custom-log-parser)

### [SaaS uygulamalarına kullanıcı hazırlama ve hazırlamayı kaldırma](active-directory-saas-app-provisioning.md) 
#### [Uygulama tümleştirmesi öğreticileri](active-directory-saas-tutorial-list.md) 
#### [SCIM etkin uygulamalara otomatik sağlama](manage-apps/use-scim-to-provision-users-and-groups.md) 
#### [Öznitelik eşlemelerini özelleştirme](active-directory-saas-customizing-attribute-mappings.md) 
#### [Öznitelik eşlemeleri için ifadeler yazma](active-directory-saas-writing-expressions-for-attribute-mappings.md) 
#### [Kapsam belirleme filtrelerini kullanma](active-directory-saas-scoping-filters.md) 
#### [Otomatik kullanıcı hazırlama raporu](active-directory-saas-provisioning-reporting.md) 
#### [Kullanıcı hazırlama sorunlarını giderme](active-directory-application-provisioning-content-map.md) 

### [Uygulama Proxy’si ile uygulamalara uzaktan erişme](manage-apps/application-proxy.md)
#### başlarken
##### [Uygulama Ara Sunucusu](manage-apps/application-proxy-enable.md)
##### [Uygulamaları yayımlama](manage-apps/application-proxy-publish-azure-portal.md)
##### [Özel etki alanları](manage-apps/application-proxy-configure-custom-domain.md)
#### [Çoklu oturum açma](manage-apps/application-proxy-single-sign-on.md)
##### [KCD ile SSO](manage-apps/application-proxy-configure-single-sign-on-with-kcd.md)
##### [Üst bilgi içeren SSO](manage-apps/application-proxy-configure-single-sign-on-with-ping-access.md)
##### [Parola kasası oluşturma ile SSO](manage-apps/application-proxy-configure-single-sign-on-password-vaulting.md)
#### Kavramlar
##### [Bağlayıcılar](manage-apps/application-proxy-connectors.md)
##### [Güvenlik](manage-apps/application-proxy-security.md)
##### [Ağlar](manage-apps/application-proxy-network-topology.md)


##### [TMG veya UAG’den yükseltme](manage-apps/application-proxy-migration.md)

#### Gelişmiş yapılandırmalar
##### [Ayrı ağlarda yayımlama](manage-apps/application-proxy-connector-groups.md)
##### [Ara sunucular](manage-apps/application-proxy-configure-connectors-with-proxy-servers.md)
##### [Talep kullanan uygulamalar](manage-apps/application-proxy-configure-for-claims-aware-applications.md)
##### [Native Client uygulamaları](manage-apps/application-proxy-configure-native-client-application.md)
##### [Sessiz yükleme](manage-apps/application-proxy-register-connector-powershell.md)
##### [Özel giriş sayfası](manage-apps/application-proxy-configure-custom-home-page.md)
##### [Satır içi bağlantıları çevirme](manage-apps/application-proxy-configure-hard-coded-link-translation.md)
##### [Joker karakterler](active-directory-application-proxy-wildcard.md)
##### [Kişisel verileri kaldırma](manage-apps/application-proxy-remove-personal-data.md)


#### Adım adım kılavuzlar yayımlama
##### [Uzak Masaüstü](manage-apps/application-proxy-integrate-with-remote-desktop-services.md)
##### [SharePoint](manage-apps/application-proxy-integrate-with-sharepoint-server.md)
##### [Microsoft Teams](manage-apps/application-proxy-integrate-with-teams.md)
##### [Tableau](manage-apps/application-proxy-integrate-with-tableau.md)
##### [Qlik](active-directory-application-proxy-qlik.md)
#### [PowerShell](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0#application_proxy_application_management) 

#### [Sorun giderme](manage-apps/application-proxy-troubleshoot.md)

### Kurumsal uygulamaları yönetme
#### [Kullanıcıları atama](manage-apps/assign-user-or-group-access-portal.md)
#### [Markalamayı özelleştirme](manage-apps/change-name-or-logo-portal.md)
#### [Kullanıcılar için oturum açmayı devre dışı bırakma](manage-apps/disable-user-sign-in-portal.md)
#### [Kullanıcıları kaldırma](manage-apps/remove-user-or-group-access-portal.md)
#### [Tüm uygulamalarımı görüntüleme](manage-apps/view-applications-portal.md)
#### [Kullanıcı hesabı hazırlamayı yönetme](manage-apps/configure-automatic-user-provisioning-portal.md)
#### [Kurumsal uygulamalar için çoklu oturum açmayı yönetme](manage-apps/configure-single-sign-on-portal.md)
#### [SAML uygulamaları için gelişmiş sertifika imzalama](manage-apps/certificate-signing-options.md)
#### [Uygulamayı kullanıcı deneyiminden gizleme](manage-apps/hide-application-from-user-portal.md)
### [HRD İlkesi'ni kullanarak Oturum Açma için Otomatik Hızlandırmayı Yapılandırma](manage-apps/configure-authentication-for-federated-users-portal.md)
### [AD FS uygulamalarını Azure AD’ye geçirme](migrate-adfs-apps-to-azure.md) 
### [Uygulamalara erişimi yönetme](manage-apps/what-is-access-management.md)
#### [SSO erişimi](manage-apps/what-is-single-sign-on.md)
#### [SSO için sertifikalar](manage-apps/manage-certificates-for-federated-single-sign-on.md)
#### [Kiracı kısıtlamaları](manage-apps/tenant-restrictions.md)
#### [SCIM kullanıcı sağlama hizmetinden yararlanma](manage-apps/use-scim-to-provision-users-and-groups.md)

### [Sorun giderme](active-directory-application-troubleshoot-content-map.md)
#### [Uygulama Geliştirme](active-directory-application-dev-troubleshoot-content-map.md)
##### [Yapılandırma ve Kayıt](active-directory-application-dev-config-content-map.md)
##### [Geliştirme](active-directory-application-dev-development-content-map.md)
#### [Uygulama Yönetimi](active-directory-application-management-troubleshoot-content-map.md)
##### [Yapılandırma](active-directory-application-config-content-map.md)
##### [Oturum açma](active-directory-application-sign-in-content-map.md)
##### [Sağlama](active-directory-application-provisioning-content-map.md)

###### [Kullanıcının sağlanmasını doğrulama](application-provisioning-when-will-provisioning-finish-specific-user.md) 
###### [Sağlama uzun sürüyor](application-provisioning-when-will-provisioning-finish.md) 
###### [Kullanıcı sağlamayı yapılandırma](application-provisioning-config-how-to.md) 
###### [Sağlama yapılandırma sorunu](application-provisioning-config-problem.md) 
###### [Yönetici kimlik bilgilerini kaydetme sorunu](application-provisioning-config-problem-storage-limit.md) 
###### [Hiçbir kullanıcı sağlanmıyor](application-provisioning-config-problem-no-users-provisioned.md) 
###### [Yanlış kullanıcılar sağlanıyor](application-provisioning-config-problem-wrong-users-provisioned.md) 

##### [Erişimi yönetme](active-directory-application-access-content-map.md)
##### [Erişim Paneli](active-directory-application-access-panel-content-map.md)
##### [Uygulama Proxy’si](active-directory-application-proxy-content-map.md)
##### [Koşullu erişim](active-directory-application-conditional-access-content-map.md)
### [Uygulama geliştirme](active-directory-applications-guiding-developers-for-lob-applications.md)
### [Belge kitaplığı](active-directory-apps-index.md)

## Dizininizi yönetme
### [Azure AD Connect](./connect/active-directory-aadconnect.md)
### Özel etki alanı adları
#### [Hızlı Başlangıç](fundamentals/add-custom-domain.md)
#### [Özel etki alanı adı ekleme](active-directory-domains-manage-azure-portal.md)
### [Dizininizi yönetme](fundamentals/active-directory-administer.md)
### [Bir dizini silme](directory-delete-howto.md)
### [Birden fazla dizin](active-directory-licensing-directory-independence.md)
### [Self servise kaydolma](active-directory-self-service-signup.md)
### [Yönetilmeyen bir dizini devralma](domains-admin-takeover.md)
### [Enterprise State Roaming](active-directory-windows-enterprise-state-roaming-overview.md)
#### [Etkinleştirme](active-directory-windows-enterprise-state-roaming-enable.md)
#### [Grup ilkesi ayarları](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
#### [Windows 10 ayarları](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
#### [SSS](active-directory-windows-enterprise-state-roaming-faqs.md)
#### [Sorun giderme](active-directory-windows-enterprise-state-roaming-troubleshooting.md)


### [Azure AD Connect kullanarak şirket içi kimlikleri tümleştirme](./connect/active-directory-aadconnect.md)

## Kaynaklara temsilci erişimi
### [Yönetici rolleri](active-directory-assign-admin-roles-azure-portal.md)
#### [Kullanıcıya yönetici rolü atama](fundamentals/active-directory-users-assign-role-azure-portal.md) 
#### [Üye ve konuk kullanıcı izinlerini karşılaştırma](fundamentals/users-default-permissions.md) 
### [Ayrıcalıklı erişimin güvenliğini sağlama](admin-roles-best-practices.md)  
### [Acil durum erişimi yönetici hesapları oluşturma](active-directory-admin-manage-emergency-access-accounts.md) 


#### [Varsayılan kullanıcı izinleri](fundamentals/users-default-permissions.md)
### [Yönetim birimleri](active-directory-administrative-units-management.md)
### [Belirteç ömrünü yapılandırma](active-directory-configurable-token-lifetimes.md)
### [Ayrıcalıklı rollerin güvenliğini sağlama](admin-roles-best-practices.md)

## Erişim gözden geçirmeleri
### [Erişim gözden geçirmelerine genel bakış](active-directory-azure-ad-controls-access-reviews-overview.md)
### [Erişim gözden geçirmesi tamamlama](active-directory-azure-ad-controls-complete-access-review.md)
### [Erişim gözden geçirmesi oluşturma](active-directory-azure-ad-controls-create-access-review.md)
### [Erişim değerlendirmesi gerçekleştirme](active-directory-azure-ad-controls-perform-access-review.md)
### [Erişiminizi gözden geçirme](active-directory-azure-ad-controls-how-to-review-your-access.md)
### [Erişim gözden geçirmesi ile konuk erişimi](active-directory-azure-ad-controls-manage-guest-access-with-access-reviews.md)
### [Gözden geçirmelerle kullanıcı erişimini yönetme](active-directory-azure-ad-controls-manage-user-access-with-access-reviews.md)
### [Programları ve denetimleri yönetme](active-directory-azure-ad-controls-manage-programs-controls.md)
### [Erişim gözden geçirmesi sonuçlarını alma](active-directory-azure-ad-controls-retrieve-access-review.md)

## Kimliklerinizi güvenli hale getirme
### [Koşullu erişim](active-directory-conditional-access-azure-portal.md)
#### [Kullanmaya başlama](active-directory-conditional-access-azure-portal-get-started.md)
#### Hızlı Başlangıçlar
##### [Bulut uygulaması MFA başına yapılandırma](active-directory-conditional-access-app-based-mfa.md)
##### [Kullanım koşullarının kabul edilmesini gerektirme](active-directory-conditional-access-tou.md)
#### Öğreticiler
##### [Klasik MFA ilkesini geçirme](active-directory-conditional-access-migration-mfa.md)
#### Kavramlar
##### [Koşullar](active-directory-conditional-access-conditions.md)
##### [Konum koşulları](active-directory-conditional-access-locations.md)
##### [Denetimler](active-directory-conditional-access-controls.md)
##### [Durum aracı](active-directory-conditional-access-whatif.md)
##### [Office 365 hizmetleri için cihaz ilkelerini anlama](active-directory-conditional-access-device-policies.md)
#### Nasıl yapılır kılavuzları
##### [En iyi uygulamalar](active-directory-conditional-access-best-practices.md)
##### [Güvenilmeyen ağlardan gelen erişim denemeleri için koşullu erişim ilkelerini yapılandırma](active-directory-conditional-access-untrusted-networks.md)
##### [Cihaz tabanlı koşullu erişimi ayarlama](active-directory-conditional-access-policy-connected-applications.md)
##### [Uygulama tabanlı koşullu erişimi ayarlama](active-directory-conditional-access-mam.md)
##### [Kullanıcılar ve uygulamalar için kullanım koşullarını sağlayın](active-directory-tou.md)
##### [Klasik ilkeleri geçirme](active-directory-conditional-access-migration.md)
##### [VPN bağlantısı ayarlama](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/always-on-vpn-deploy)
##### [SharePoint ve Exchange Online ayarlama](active-directory-conditional-access-no-modern-authentication.md)
##### [Düzeltme](active-directory-conditional-access-device-remediation.md)
#### [Teknik başvuru](active-directory-conditional-access-technical-reference.md)
#### [SSS](active-directory-conditional-faqs.md)

### Sertifika Tabanlı Kimlik Doğrulaması
#### [Android](active-directory-certificate-based-authentication-android.md)
#### [iOS](active-directory-certificate-based-authentication-ios.md)
#### [Kullanmaya başlama](active-directory-certificate-based-authentication-get-started.md)

### [Azure AD Kimlik Koruması](active-directory-identityprotection.md)
#### [Etkinleştirme](active-directory-identityprotection-enable.md)
#### [Güvenlik açıklarını algılama](active-directory-identityprotection-vulnerabilities.md)
#### [Risk olayları](active-directory-identity-protection-risk-events.md)
#### [Bildirimler](active-directory-identityprotection-notifications.md)
#### [Oturum açma deneyimi](active-directory-identityprotection-flows.md)
#### [Risk olaylarının benzetimini gerçekleştirme](active-directory-identityprotection-playbook.md)
#### [Kullanıcıların engelini kaldırma](active-directory-identityprotection-unblock-howto.md)
#### [SSS](active-directory-identity-protection-faqs.md)
#### [Sözlük](active-directory-identityprotection-glossary.md)
#### [Microsoft Graph](active-directory-identityprotection-graph-getting-started.md)
### [Privileged Identity Management](active-directory-privileged-identity-management-configure.md)

## Diğer hizmetleri Azure AD ile tümleştirme 
### [LinkedIn’i Azure AD ile tümleştirme](linkedin-integration.md)

## [Azure’a AD FS dağıtma](active-directory-aadconnect-azure-adfs.md)
### [Yüksek kullanılabilirlik](active-directory-adfs-in-azure-with-azure-traffic-manager.md)
### [Değişiklik imzası karma algoritması](active-directory-federation-sha256-guidance.md)

## [Sorun giderme](fundamentals/active-directory-troubleshooting-support-howto.md)

## Azure AD Kavram Kanıtı (PoC) Dağıtma
### [PoC El Kitabı: Giriş](active-directory-playbook-intro.md)
### [PoC El Kitabı: Malzemeler](active-directory-playbook-ingredients.md)
### [PoC El Kitabı: Uygulama](active-directory-playbook-implementation.md)
### [PoC El Kitabı: Yapı Taşları](active-directory-playbook-building-blocks.md)


# Başvuru
## [Kod örnekleri](https://azure.microsoft.com/resources/samples/?service=active-directory)
## [Azure PowerShell cmdlet'leri](/powershell/azure/overview)
## [Java API Başvurusu](/java/api)
## [.NET API’si](/active-directory/adal/microsoft.identitymodel.clients.activedirectory)
## [Hizmet sınırlamaları ve kısıtlamalar](active-directory-service-limits-restrictions.md)

# İlgili
## [Multi-Factor Authentication](/azure/multi-factor-authentication/)
## [Azure AD Connect](./connect/active-directory-aadconnect.md)
## [Azure AD Connect Health](./connect-health/active-directory-aadconnect-health.md)
## [Geliştiriciler için Azure AD](./develop/active-directory-how-to-integrate.md)
## [Azure AD Privileged Identity Management](./privileged-identity-management/active-directory-securing-privileged-access.md)

# Kaynaklar
## [Azure geri bildirim forumu](https://feedback.azure.com/forums/169401-azure-active-directory)
## [Azure Yol Haritası](https://azure.microsoft.com/roadmap/?category=security-identity)
## [MSDN forumu](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)
## [Fiyatlandırma](https://azure.microsoft.com/pricing/details/active-directory/)
## [Fiyatlandırma hesaplayıcı](https://azure.microsoft.com/pricing/calculator/)
## [Hizmet güncelleştirmeleri](https://azure.microsoft.com/updates/?product=active-directory)
## [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-active-directory)
## [Videolar](https://azure.microsoft.com/documentation/videos/index/?services=active-directory)
