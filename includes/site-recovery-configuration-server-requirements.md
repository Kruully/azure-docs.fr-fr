| **Matériel** | |
| --- |---|
| Nombre de cœurs de processeur| 8 cœurs |
| RAM| 12 Go|
| Nombre de disques | **3 disques** <br> - Disque de système d’exploitation<br> - Disque cache de serveur de processus<br> - Lecteur de rétention (pour la restauration automatique)|
| Espace disque disponible (cache de serveur de processus) | 600 Go
| Espace disque disponible (disque de rétention) | 600 Go|
| **Logiciel** | |
| Version de système d’exploitation | Windows Server 2012 R2 |
| Paramètres régionaux du système d’exploitation | Anglais (en-us)|
| Version VMware vSphere PowerCLI | [PowerCLI 6.0](https://developercenter.vmware.com/tool/vsphere_powercli/6.0 "PowerCLI 6.0")|
| Rôles Windows Server | Ne pas activer les rôles suivants <br> **Services de domaine Active Directory** <br>**Internet Information Service** <br> **Hyper-V** |
| **Réseau** | |
| Type de carte d’interface réseau | VMXNET3 |
| Type d’adresse IP | Statique |
| Accès à Internet | Le serveur doit être en mesure d’accéder à l’URL suivante, directement ou via un serveur Proxy <br> - \*.accesscontrol.windows.net<br> - \*.backup.windowsazure.com <br>- \*.store.core.windows.net<br> - \*.blob.core.windows.net<br> - \*.hypervrecoverymanager.windowsazure.com <br> - https://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi <br> - time.nist.gov <br> - time.windows.com |
| Ports | 443 (Orchestration du canal de contrôle)<br>9443 (Transport de données)|


<!--HONumber=Jan17_HO3-->


