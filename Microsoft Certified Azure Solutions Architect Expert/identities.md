#	Azure AD
Differences with AD DS (AD Domain Services, installed on VMs)
* Identity solution. Azure AD is primarily an identity solution, and it is designed for Internet-based applications by using HTTP and HTTPS communications.
REST API Querying. Because Azure AD is HTTP/HTTPS based, it cannot be queried through LDAP. Instead, Azure AD uses the REST API over HTTP and HTTPS.
* Communication Protocols. Because Azure AD is HTTP/HTTPS based, it does not use Kerberos authentication. Instead, it uses HTTP and HTTPS protocols such as SAML, WS-Federation, and OpenID Connect for authentication (and OAuth for authorization).
* Federation Services. Azure AD includes federation services, and many third-party services (such as Facebook).
* Flat structure. Azure AD users and groups are created in a flat structure, and there are no Organizational Units (OUs) or Group Policy Objects (GPOs).

**Tenant** ~ Azure AD instance 
###	Send Sign-In and Audit logs to Log Analytics
* Configure on Azure AD blade
* Diagnostic Setting
* Send to Log Analytics
### Enterprise State Roaming
* Requires AAD Premium
* Synchronize user and app settings to Azure AD and encrypted with Azure RMS
* Data retained for 90-180 days
* Enable in Azure AD -> Devices -> Enterprise State Roaming
### Pricing Details
* Free < 500,000 objects
* Basic -> SSPR, AAD Proxy
* P1 -> Advanced reports, write-back, MFA
* P2 -> Identity protection, PIM
## Self-Service Password Portal (SSPR)
### General
* Enforce one or two authentication methods to reset
* Option to require user registration and set re-confirmation from 0-730 days
* Notify users if password reset and all admins if any admin reset
* Write-back on-prem requires P1 or above
* Customize help desk link
### Authentication Methods
* Mobile phone
* Office phone
* Email
* Security questions
* Mobile app code
* Mobile app notification

# Azure AD Connect (Hybrid Identities) 

# Azure AD PIM
### PIM Roles
* PIM Administrator -> manage role assignments in Azure AD and all aspects of PIM
* Security Administrator -> read info and report and manage AD and O365
### General
* Enable PIM must be global admin and becomes PIM Administrator
### PIM and Azure RBAC
* Subscription must be enrolled in RBAC
* Role assignment settings are eligible and active
### RBAC Settings
* Allow role to be permanent (eligible or active)
* Set time for how long eligible or active
* Require MFA
* Require justification
* Require approval
# Azure Role Based Access Control (RBAC)
### General
* Max of 2000 role assignments per subscription
* Allow only, must use deny assignments to explicity deny something
* Three authZ models: classic subscription administration roles, Azure RBAC roles, Azure AD Admin roles
* Owner, contributor, reader, user access administrator
* Azure AD Global Admin can take control of subs by settings "Global Admin can manage Azure subs and Management Groups" and become User Access Admin for all subs in tenant
### Role Assignments
* Security principal -> user, group, service principal, managed identity
* Role definition (role) -> actions, notactions, dataactions, notdataactions
* Scope -> management group, subscription, resource group, resource
### RBAC - Classic Subscription Administrative Roles
* Account used to sign-up for Azure is Account Administrator/Service Administrator
* Account Administrator (1), Service Administrator (1), Co-administrator (200)
* Account Administrator -> billing owner, manage sub lifecycle
* Service Administrator
* Co-Administrator -> all but change Service Admin and associate sub w/ different directory