# Lab 10 (Frida) — Rapport d'Installation

**Version Frida :** 17.9.3  
**Environnement :** Windows 11 + PowerShell 7.6.1 + Émulateur Android AVD  
**Auteur :** MADILI Kenza

---

## 1. Objectifs du Lab

- Installer et vérifier Frida (client, Python et CLI) sur Windows.
- Déployer et lancer `frida-server` sur un émulateur Android AVD.
- Établir une connexion et injecter un script minimal pour valider l'environnement.
- Diagnostiquer les problèmes courants d'installation.

---

## 2. Environnement de Travail

**Système hôte**
- OS : Windows 11 — PowerShell 7.6.1
- Python : 3.11.9 (installé avec option "Add to PATH" cochée)
- Frida : 17.9.3
- Android Platform Tools (adb) installé et dans le PATH

**Appareil cible**
- Type : Émulateur Android AVD (Android Studio)
- ABI : x86_64
- ID adb : `emulator-5554`

---

## 3. Livrable 1 — Installation et Preuves

### Installation

<img width="548" height="272" alt="1" src="https://github.com/user-attachments/assets/bf5536f7-058d-4161-81a8-67d4a4d9304d" />

### Preuves de version

<img width="467" height="83" alt="2" src="https://github.com/user-attachments/assets/c9ca3edd-5fe5-4052-affb-abff6241a2a5" />

<img width="236" height="63" alt="3" src="https://github.com/user-attachments/assets/87d66040-75b9-4a0d-b332-c801b7ed8c47" />


| Commande | Résultat | Statut |
|----------|----------|--------|
| `frida --version` | 17.9.3 | ✅ |
| `frida-ps --version` | 17.9.3 | ✅ |
| `python -c "import frida; print(frida.__version__)"` | 17.9.3 | ✅ |
| `adb devices` | emulator-5554 device | ✅ |

> Les trois versions correspondent parfaitement, confirmant une installation cohérente.

---

## 4. Livrable 2 — Déploiement Android

### 4.1 Détection de l'architecture

<img width="357" height="44" alt="4" src="https://github.com/user-attachments/assets/269ab32a-9434-4c0f-9817-78493c027a93" />

### 4.2 Téléchargement de frida-server

<img width="664" height="28" alt="5" src="https://github.com/user-attachments/assets/fdebceb7-bdda-4a5b-ab21-859c2268e780" />

### 4.3 Extraction (via Python/lzma)

> ⚠️ `tar` natif Windows ne supporte pas le format `.xz`. Solution via le module `lzma` de Python.

<img width="450" height="120" alt="6" src="https://github.com/user-attachments/assets/2298ecf9-7e59-4a83-b8c8-055e4f21e2ab" />

### 4.4 Déploiement sur l'émulateur

<img width="550" height="86" alt="7" src="https://github.com/user-attachments/assets/0dfb42ef-e625-4e52-97e4-c0a35a2d1c02" />

### 4.5 Lancement en root

> ⚠️ `frida-server` doit tourner en **root** pour s'attacher aux processus.

<img width="366" height="27" alt="Capture d’écran 2026-05-01 223425" src="https://github.com/user-attachments/assets/5de12895-bee2-45fa-88d8-51872aaddca7" />
<img width="357" height="44" alt="4" src="https://github.com/user-attachments/assets/777d059a-fed2-4cca-b6e9-caf6f077d364" />


### 4.6 Liste des applications — `frida-ps -Uai`

<img width="416" height="256" alt="111" src="https://github.com/user-attachments/assets/7836b381-dacd-441d-94ac-334014c0cc7e" />

---

## 5. Livrable 3 — Injection de Script

### 5.1 Commande d'injection et Contenu de `hello.js`

<img width="469" height="197" alt="9" src="https://github.com/user-attachments/assets/39f6799d-3f97-42c3-b948-e2c3a34eec2e" /> 
<img width="394" height="157" alt="10" src="https://github.com/user-attachments/assets/845d3d72-223e-4b3f-84e0-7b4b1545d82c" />

### 5.3 Sortie obtenue

<img width="425" height="263" alt="11" src="https://github.com/user-attachments/assets/4fef4d82-0912-4028-a14f-04507c67d5ff" />

---

## 6. Livrable 4 — Dépannage

### 6.1 Simulation de l'erreur

<img width="602" height="121" alt="12" src="https://github.com/user-attachments/assets/381f3efc-85d9-45cd-928d-c3023b9caf44" />

### 6.2 Diagnostic

<img width="346" height="60" alt="13" src="https://github.com/user-attachments/assets/3524eea3-3a3f-4e9e-94e6-d90fe0a8f5e5" />

### 6.3 Correction appliquée

<img width="467" height="289" alt="14" src="https://github.com/user-attachments/assets/66115bf7-b9a2-47ee-a5e5-e70deac7b8bb" />

### 6.4 Problèmes rencontrés et solutions

| Problème | Cause | Solution | Statut |
|----------|-------|----------|--------|
| `pip` non reconnu | Python absent du PATH Windows | Réinstaller Python avec "Add to PATH" coché | ✅ |
| `tar -xf frida-server.xz` échoue | `tar` Windows ne supporte pas `.xz` | Extraction via `python -c "import lzma..."` | ✅ |
| `Failed to attach` sur tous les PIDs | `frida-server` tournait en `shell` et non `root` | `adb root` avant de lancer frida-server | ✅ |
| Avertissement SELinux au démarrage | Comportement normal sur émulateur AVD | Ignorer — non bloquant | ✅ |
| `grep` non reconnu | Commande Unix absente de Windows | Utiliser `findstr` à la place | ✅ |

---

## 7. Conclusion

Ce lab a permis d'installer et valider l'environnement Frida 17.9.3 de bout en bout sur Windows avec un émulateur Android AVD.

**Points clés à retenir :**

- L'installation de Python requiert impérativement de cocher **"Add to PATH"** sous Windows.
- L'extraction de fichiers `.xz` doit se faire via le module `lzma` de Python (`tar` Windows incompatible).
- `frida-server` doit être lancé avec les droits **root** (`adb root`) pour s'attacher aux processus Android.
- Sur Windows, `grep` n'existe pas — utiliser **`findstr`** comme équivalent.
- L'erreur SELinux au démarrage de `frida-server` sur AVD est un **avertissement non bloquant**.

**Tous les livrables ont été complétés avec succès. ✅**
