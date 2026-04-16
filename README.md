# My First Vagrant & Ansible Lab

Detta projekt är en automatiserad labbmiljö som bygger upp ett komplett nätverk med fyra virtuella maskiner. Syftet är att lära sig hur man hanterar infrastruktur som kod (IaC) och automatiserar serverkonfiguration.

# Arkitektur
Nätverket består av följande noder (alla kör Ubuntu 22.04):

| Namn | IP-adress | Roll | Tjänster |
| :--- | :--- | :--- | :--- |
| **Web** | 192.168.56.10 | Webbserver | Apache2, curl |
| **DB** | 192.168.56.11 | Databasserver | PostgreSQL, UFW (Brandvägg) |
| **Control** | 192.168.56.12 | Kontrollnod | Ansible |
| **Client** | 192.168.56.13 | Testklient | Testar nätverksåtkomst |

#Funktioner
* **Automatiserad uppstart:** Hela miljön byggs med ett enda kommando via `Vagrantfile`.
* **Nätverkssäkerhet:** Databasen är isolerad med en brandvägg (UFW) som endast tillåter inkommande trafik på port 5432 från webbservern.
* **Port Forwarding:** Klienten är konfigurerad för att kunna nås från värddatorn via port 8080.
* **Automation:** Använder Ansible Playbooks (`setup.yml`) för att installera verktyg (t.ex. `htop`) och konfigurera noder simultant.
* **Versionshantering:** Hela projektets livscykel sparas och dokumenteras via Git/GitHub.

# Hur man använder labben
1. Installera [Vagrant](https://www.vagrantup.com/) och [VirtualBox](https://www.virtualbox.org/).
2. Klona detta repository:
   ```bash
   git clone [https://github.com/Farre87/my-first-vagrant-lab.git](https://github.com/Farre87/my-first-vagrant-lab.git)
3. Starta maskinerna (stå i projektmappen):
4. vagrant up
5. Kör Ansible-konfigurationen från kontrollnoden:
6. vagrant ssh control
7. ansible-playbook -i hosts setup.yml
   
# Lärdomar 
*  ** Vagrant & VirtualBox: Skapa och hantera multipla VM-miljöer.
*  ** Networking: Förståelse för TCP/IP, interna nätverk och portar.
*  ** Linux Administration: Brandväggskonfiguration (UFW) och tjänstehantering (systemd).
* ** Ansible: Skriva YAML-playbooks för effektiv systemautomation.
* ** Git & GitHub: Professionellt arbetsflöde med commits och dokumentation.
   
