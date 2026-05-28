# NexCore Infrastructure Automation

Ovaj repozitorij sadrži Ansible projekt za potpunu automatizaciju, modernizaciju i hardening interne infrastrukture tvrtke **NexCore d.o.o.**
Projekt je strukturiran kroz role i u potpunosti je idempotentan.

## Prerequisites (Preduvjeti)

Prije pokretanja playbooka na kontrolnom nodu (**workstation**), obvezno je ispuniti sljedece korake:

### 1. Instalacija Ansible kolekcija (Requirements)
Projekt se oslanja na specificne Ansible kolekcije za upravljanje firewall, SELinux i MySQL bazama podatkovnih posluzitelja. 
Instalirajte ih pokretanjem sljedeze naredbe:

```bash
ansible-galaxy collection install -r requirements.yml
```

### 2. Dodavanje zapisa za web stranice u `/etc/hosts`
Kako bi se s kontrolnog noda moglo dokazati i verificirati pristup web stranicama (`www.nexcore.local` i `wiki.nexcore.local`), potrebno je dodati IP adresu za `servera` u svoju lokalnu `/etc/hosts` datoteku:

```bash
echo "172.25.250.10 www.nexcore.local wiki.nexcore.local" | sudo tee -a /etc/hosts
```

### 3. Provjera namjenskog diska na serverb posluzitelju
LVM konfiguracija unutar projekta zahtijeva slobodan sekundarni disk (definiran kao `/dev/vdb`). 
Prije pokretanja provjerite je li disk vidljiv na sustavu posluzitelja `serverb`:

```bash
lsblk
```

---

## Pokretanje projekta 

### 1. Provjera mrezne povezanosti
Prije izvrsavanja samog playbooka, provjerite jesu li svi hostovi dostupni i odgovaraju li na Ansible naredbe:

```bash
ansible all -i inventory.ini -m ping
```

### 2. Izvrsavanje glavnog playbooka
Nakon sto su ispunjeni svi preduvjeti i potvrdena mrezna povezanost, cjelokupna se infrastruktura podize i konfigurira pokretanjem jedinstvenog playbooka:

```bash
ansible-playbook -i inventory.ini site.yml
