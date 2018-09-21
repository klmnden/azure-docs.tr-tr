# [Azure AD Etki Alanı Hizmetleri Belgeleri](index.yml)

# Genel Bakış
## [Azure AD Etki Alanı Hizmetleri nedir?](active-directory-ds-overview.md)
## Bu sizin için geçerli mi?
### [Windows Server AD ile karşılaştırma](active-directory-ds-comparison.md)
### [Azure AD katılma ile karşılaştırma](active-directory-ds-compare-with-azure-ad-join.md)
## [Yenilikler](https://azure.microsoft.com/updates/?product=active-directory-ds)
## [Özellikler](active-directory-ds-features.md)
## [Senaryolar](active-directory-ds-scenarios.md)
## [Eşitleme nasıl çalışır?](active-directory-ds-synchronization.md)
## [Uyumlu üçüncü taraf yazılımı](active-directory-ds-compatible-software.md)

# başlarken
## [1. Görev: Temel ayarları yapılandırma](active-directory-ds-getting-started.md)
## [2. Görev: Ağ ayarlarını yapılandırma](active-directory-ds-getting-started-network.md)
## [3. Görev: Yönetici grubunu yapılandırma ve Azure AD Domain Services’ı etkinleştirme](active-directory-ds-getting-started-admingroup.md)
## [4. Görev: Sanal ağa yönelik DNS ayarlarını güncelleştirme](active-directory-ds-getting-started-dns.md)
## [5. Görev: Parola karması eşitlemeyi etkinleştirme](active-directory-ds-getting-started-password-sync.md)

# Nasıl yapılır
## [Yönetilen bir etki alanının sistem durumunu denetleme](active-directory-ds-check-health.md)
## [Azure CSP aboneliklerinde Azure AD Domain Services kullanma](active-directory-ds-csp.md)
## [PowerShell kullanarak Azure AD Domain Services'ı etkinleştirme](active-directory-ds-enable-using-powershell.md)
## Yönetilen bir etki alanına katılma
### [Windows Server VM](active-directory-ds-admin-guide-join-windows-vm-portal.md)
### [Şablondan Windows Server VM](active-directory-ds-join-windows-vm-template.md)
### [CentOS](active-directory-ds-join-centos-linux-vm.md)
### [CoreOS](active-directory-ds-join-coreos-linux-vm.md)
### [Red Hat Enterprise Linux](active-directory-ds-join-rhel-linux-vm.md)
### [Ubuntu Server](active-directory-ds-join-ubuntu-linux-vm.md)
## Yönetilen etki alanını yönetme
### [Yönetilen etki alanını yönetme](active-directory-ds-admin-guide-administer-domain.md)
### [Yönetilen etki alanında DNS’yi yönetme](active-directory-ds-admin-guide-administer-dns.md)
### Yönetilen bir etki alanı için güvenli LDAP yapılandırma
#### [1. Görev: Güvenli LDAP için sertifika alma](active-directory-ds-admin-guide-configure-secure-ldap.md)
#### [2. Görev: Güvenli LDAP sertifikasını dışarı aktarma](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md)
#### [3. Görev: Azure portalını kullanarak yönetilen etki alanı için güvenli LDAP’yi etkinleştirme](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)
#### [4. Görev: İnternet’ten yönetilen etki alanına erişmek için DNS’yi yapılandırma](active-directory-ds-ldaps-configure-dns.md)
#### [5. Görev: Yönetilen etki alanını bağlama ve güvenli LDAP erişimini kilitleme](active-directory-ds-ldaps-bind-lockdown.md)
#### [Güvenli LDAP sorunlarını giderme](active-directory-ds-ldaps-troubleshoot.md)

### [Yönetilen bir etki alanında OU oluşturma](active-directory-ds-admin-guide-create-ou.md)
### [Yönetilen etki alanında grup tarafından yönetilen hizmet hesabı oluşturma](active-directory-ds-create-gmsa.md)
### [Yönetilen etki alanında grup ilkesi yönetme](active-directory-ds-admin-guide-administer-group-policy.md)
### [Yönetilen etki alanında parola ilkelerini yapılandırma](active-directory-ds-password-policy.md)
### [Azure AD’den, yönetilen bir etki alanına kapsamlı eşitleme yapılandırma](active-directory-ds-scoped-synchronization.md)
## [Sanal ağ seçme](active-directory-ds-networking.md)
## Uygulamaları dağıtma
### [SharePoint Server için profil eşitleme desteğini yapılandırma](active-directory-ds-enable-sharepoint-profile-sync.md)
### [Kerberos Kısıtlanmış Temsilini Yapılandırma](active-directory-ds-enable-kcd.md)
### [Azure AD Uygulama Proxy’si Dağıtma](active-directory-ds-deploy-azure-app-proxy.md)
## [Yönetilen bir etki alanını silme](active-directory-ds-disable-aadds.md)
## Sorun giderme
### [SSS’ler](active-directory-ds-faqs.md)
### [Sorun giderme kılavuzu](active-directory-ds-troubleshooting.md)
### [Sorun giderme uyarıları](active-directory-ds-troubleshoot-alerts.md)
#### [Bozuk bir NSG yapılandırmasını düzeltme](active-directory-ds-troubleshoot-nsg.md)
#### [Eksik hizmet asıl adlarını geri yükleme](active-directory-ds-troubleshoot-service-principals.md)
#### [Güvenli LDAP hataları](active-directory-ds-troubleshoot-ldaps.md)
### [Eşleşmeyen kiracı hatalarını düzeltme](active-directory-ds-mismatched-tenant-error.md)
### [Askıya alınan etki alanları](active-directory-ds-suspension.md)


# Başvuru
## [Kod örnekleri](https://azure.microsoft.com/resources/samples/?service=active-directory)

# İlgili
## [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md)
## [Azure Active Directory B2C](../active-directory-b2c/active-directory-b2c-overview.md)
## [Multi-Factor Authentication](../active-directory/authentication/multi-factor-authentication.md)

# Kaynaklar
## [Azure AD geri bildirim forumu](https://feedback.azure.com/forums/169401-azure-active-directory)
## [Azure Yol Haritası](https://azure.microsoft.com/roadmap/?category=security-identity)
## [Bizimle iletişim kurun](active-directory-ds-contact-us.md)
## [Fiyatlandırma](https://azure.microsoft.com/pricing/details/active-directory-ds/)
## [Fiyatlandırma hesaplayıcı](https://azure.microsoft.com/pricing/calculator/)
## [Hizmet güncelleştirmeleri](https://azure.microsoft.com/updates/?product=active-directory-ds)
