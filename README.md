# SignetMail

**Central e-mail signatures for Microsoft 365 and Google Workspace · Centralni e-poštni podpisi za Microsoft 365 in Google Workspace**

[English](#english) · [Slovenščina](#slovenščina) · [signetmail.si](https://signetmail.si) · [Documentation](https://app.signetmail.si/docs/) · [Compare](https://signetmail.si/en/compare/)

---

## English

SignetMail puts one consistent signature, built from your directory, into every e-mail your company sends. Change a job title or phone number in Microsoft Entra ID and every signature follows; nobody edits signatures by hand. You run the server yourself (Docker or Windows service). It is **not** in the mail path, so mail keeps flowing even when the server is down.

### How it works

```
Microsoft Entra ID ──(Microsoft Graph)──▶  SignetMail server  ──▶  Outlook add-in (new Outlook, web, Mac)
 users, groups, attributes                 rules · Liquid templates ─▶  Windows agent (classic Outlook)
                                           versions · audit log     ─▶  Exchange transport rule (optional, mobile)
Admins and editors ──▶ web portal                                   ─▶  Gmail (Google Workspace)
```

1. The server reads users and groups from Entra ID (certificate-based, no client secret).
2. When someone writes an e-mail, the add-in or agent asks the server for that person's signature.
3. The first matching [rule](https://app.signetmail.si/docs/rules.html) selects the template; a running campaign may add a banner.
4. The published template is rendered with Liquid (`user.*`, `org.*`, personal fields), images are embedded, and the signature is inserted into the message.

Details: [How it works](https://app.signetmail.si/docs/how-it-works.html).

### Features

- **Directory as the single source of truth**: name, title, department, phone and any attribute; users can maintain a few personal fields themselves ("My details" in the add-in).
- **Visual and code editor** with live preview for any real person; Liquid templates, social icons, disclaimer, vCard/QR links.
- **Organizations with inheritance** (group → company → division) for logo, address, colours and fields.
- **Versions and approvals**: drafts, publishing, roll-back; every change is audited.
- **Rules and a "who gets which signature" check** with a trace of why a rule matched.
- **Campaigns** with privacy-friendly statistics (no tracking of recipients).
- **Image library** and embedded images, so logos show without "download pictures" in the recipient's client.
- **Import from other products**: paste a signature from Signature 365, CodeTwo and similar; placeholders are converted automatically.
- **Overview, users and notifications** pages for operations; client activity per channel.
- **MCP connection** for AI assistants (drafting only; publishing stays manual).
- **Secure by design**: hardened server, HTTPS, role-based access, Entra sign-in with MFA, encrypted backups.

### Free for up to 50 active users

Community use is free with **all features** and no time limit for organizations with up to **50 active users** (people who received a signature in the last 30 days). Above that, a licence is required: it includes updates for the licence period and e-mail support on working days, 8:00–16:00 CET. Without a licence above 50 users, only a few Pro features are locked (creating and editing campaigns, new MCP connections, Overview, Users); signatures always keep working. Contact: info@signetmail.si. Terms: [LICENSE](LICENSE). SignetMail is proprietary software; the source code is not published.

### Download

| | |
|---|---|
| **Windows Server installer** | [`SignetMail-Setup.exe`](https://github.com/AljazEferl/signetmail-releases/releases/latest/download/SignetMail-Setup.exe): installs the server as a Windows service. The installer is not code-signed yet, so SmartScreen may warn: *More info → Run anyway*. |
| **Docker** | `ghcr.io/aljazeferl/signetmail-api` (tags `latest`, `1.1`, `1.1.0`) |
| **Server deployment bundle** (Docker Compose + Caddy + scripts) | [`signetmail-deploy.tar.gz`](https://github.com/AljazEferl/signetmail-releases/releases/latest/download/signetmail-deploy.tar.gz) |
| **Windows agent** for classic Outlook | [`SignetMail-Agent-win-x64.zip`](https://github.com/AljazEferl/signetmail-releases/releases/latest/download/SignetMail-Agent-win-x64.zip): Kerberos or Entra sign-in. |
| **Exchange transport rule** (optional, for mobile) | [`Set-SignetMailTransportRule.ps1`](https://github.com/AljazEferl/signetmail-releases/releases/latest/download/Set-SignetMailTransportRule.ps1) |

### Quick start

**Docker (Linux):**
```bash
mkdir -p /opt/signetmail/cloud && cd /opt/signetmail/cloud
curl -fsSL https://github.com/AljazEferl/signetmail-releases/releases/latest/download/signetmail-deploy.tar.gz | tar xz
cp .env.example .env && nano .env      # host name, Entra IDs, ACCEPT_EULA=true
./setup.sh && docker compose up -d
```
**Windows Server:** run `SignetMail-Setup.exe`; the wizard asks for the host name, tenant and client ID of your Entra app, the portal owner and an HTTPS certificate (`.pfx`). Then upload `%ProgramData%\SignetMail\certs\graph.cer` to the Entra app and open `https://<host>/portal/`.

Step-by-step: [Installing the server](https://app.signetmail.si/docs/install-server.html) · [Entra setup](https://app.signetmail.si/docs/entra-setup.html) · [First steps](https://app.signetmail.si/docs/first-steps.html).

### Requirements

A Linux server with Docker, or Windows Server for the installer; a DNS name with a trusted HTTPS certificate; a Microsoft Entra ID app registration (or Google Workspace). See [Requirements](https://app.signetmail.si/docs/requirements.html).

### Privacy

After an owner accepts the licence terms, the server sends one licence check per day: a random installation ID, the version and a *range* for the number of active users. Never names, e-mail addresses, domains or directory data. If it cannot be sent (no internet), nothing stops working.

### Support the project

Questions and licences: info@signetmail.si. SignetMail is built and maintained in Slovenia. If it is useful to you, you can [support its development](https://signetmail.si/hvala/?l=en).

---

## Slovenščina

SignetMail v vsako e-pošto podjetja vstavi enoten podpis, zgrajen iz imenika. Spremenite naziv ali telefon v Microsoft Entra ID in vsi podpisi sledijo; nihče ne ureja podpisov ročno. Strežnik zaženete sami (Docker ali Windows storitev). **Ni** v poti pošte, zato pošta teče tudi, če strežnik ne deluje.

### Kako deluje

```
Microsoft Entra ID ──(Microsoft Graph)──▶  strežnik SignetMail  ──▶  Outlookov dodatek (novi Outlook, splet, Mac)
 uporabniki, skupine, atributi             pravila · Liquid predloge ─▶  agent za Windows (klasični Outlook)
                                           različice · dnevnik      ─▶  pravilo prenosa v Exchange (po izbiri, mobilne naprave)
Skrbniki in urejevalniki ──▶ spletni portal                         ─▶  Gmail (Google Workspace)
```

1. Strežnik bere uporabnike in skupine iz Entra ID (s certifikatom, brez skrivnosti odjemalca).
2. Ko oseba napiše e-pošto, dodatek ali agent od strežnika zahteva njen podpis.
3. Prvo ujemajoče [pravilo](https://app.signetmail.si/docs/rules.html) izbere predlogo; tekoča kampanja lahko doda pasico.
4. Objavljena predloga se izriše z Liquid (`user.*`, `org.*`, osebna polja), slike se vdelajo, podpis se vstavi v sporočilo.

Podrobnosti (angleško): [How it works](https://app.signetmail.si/docs/how-it-works.html).

### Funkcije

- **Imenik kot edini vir podatkov**: ime, naziv, oddelek, telefon in poljuben atribut; uporabniki lahko nekaj osebnih polj vzdržujejo sami (»Moji podatki« v dodatku).
- **Vizualni in kodni urejevalnik** z živim predogledom za poljubno osebo; Liquid predloge, družbena omrežja, izjava o omejitvi odgovornosti, vCard/QR.
- **Organizacije z dedovanjem** (skupina → podjetje → divizija) za logotip, naslov, barve in polja.
- **Različice in potrjevanje**: osnutki, objava, povrnitev; vsaka sprememba je zabeležena.
- **Pravila in preverjanje »kdo dobi kateri podpis«** s sledjo, zakaj se je pravilo ujelo.
- **Kampanje** s statistiko, ki spoštuje zasebnost (brez sledenja prejemnikov).
- **Knjižnica slik** in vdelane slike, da se logotip prikaže brez »prenesi slike« pri prejemniku.
- **Uvoz iz drugih izdelkov**: prilepite podpis iz Signature 365, CodeTwo ipd.; označbe se pretvorijo samodejno.
- **Pregled, uporabniki in obvestila** za delovanje; aktivnost odjemalcev po kanalih.
- **Povezava MCP** za AI-pomočnike (samo osnutki; objava ostaja ročna).
- **Varno po zasnovi**: utrjen strežnik, HTTPS, vloge, prijava z Entra in MFA, šifrirane varnostne kopije.

### Brezplačno do 50 aktivnih uporabnikov

Skupnostna uporaba je brezplačna z **vsemi funkcijami** in brez časovne omejitve za organizacije do **50 aktivnih uporabnikov** (osebe, ki so v zadnjih 30 dneh prejele podpis). Nad tem je potrebna licenca: vključuje posodobitve v obdobju licence in podporo po e-pošti ob delavnikih, 8:00–16:00 CET. Brez licence nad 50 uporabniki se zaklene le nekaj Pro funkcij (ustvarjanje in urejanje kampanj, nove povezave MCP, Pregled, Uporabniki); podpisi vedno delujejo naprej. Stik: info@signetmail.si. Pogoji: [LICENSE](LICENSE). SignetMail je lastniški program, izvorna koda ni objavljena.

### Prenos

| | |
|---|---|
| **Namestitev za Windows Server** | [`SignetMail-Setup.exe`](https://github.com/AljazEferl/signetmail-releases/releases/latest/download/SignetMail-Setup.exe): namesti strežnik kot Windows storitev. Program še ni podpisan, zato lahko SmartScreen opozori: *Več informacij → Vseeno zaženi*. |
| **Docker** | `ghcr.io/aljazeferl/signetmail-api` (oznake `latest`, `1.1`, `1.1.0`) |
| **Paket za namestitev na strežnik** (Docker Compose + Caddy + skripte) | [`signetmail-deploy.tar.gz`](https://github.com/AljazEferl/signetmail-releases/releases/latest/download/signetmail-deploy.tar.gz) |
| **Agent za Windows** za klasični Outlook | [`SignetMail-Agent-win-x64.zip`](https://github.com/AljazEferl/signetmail-releases/releases/latest/download/SignetMail-Agent-win-x64.zip): prijava s Kerberos ali Entra. |
| **Pravilo prenosa v Exchange** (po izbiri, za mobilne naprave) | [`Set-SignetMailTransportRule.ps1`](https://github.com/AljazEferl/signetmail-releases/releases/latest/download/Set-SignetMailTransportRule.ps1) |

### Hiter začetek

**Docker (Linux):**
```bash
mkdir -p /opt/signetmail/cloud && cd /opt/signetmail/cloud
curl -fsSL https://github.com/AljazEferl/signetmail-releases/releases/latest/download/signetmail-deploy.tar.gz | tar xz
cp .env.example .env && nano .env      # ime gostitelja, ID-ji Entra, ACCEPT_EULA=true
./setup.sh && docker compose up -d
```
**Windows Server:** zaženite `SignetMail-Setup.exe`; čarovnik vpraša za ime gostitelja, ID imenika in aplikacije v Entra, lastnika portala in HTTPS certifikat (`.pfx`). Nato naložite `%ProgramData%\SignetMail\certs\graph.cer` v aplikacijo Entra in odprite `https://<gostitelj>/portal/`.

Navodila po korakih (angleško): [Installing the server](https://app.signetmail.si/docs/install-server.html) · [Entra setup](https://app.signetmail.si/docs/entra-setup.html) · [First steps](https://app.signetmail.si/docs/first-steps.html).

### Zahteve

Linux strežnik z Dockerjem ali Windows Server za namestitveni program; DNS ime z zaupanja vrednim HTTPS certifikatom; registracija aplikacije v Microsoft Entra ID (ali Google Workspace). Glejte [Requirements](https://app.signetmail.si/docs/requirements.html).

### Zasebnost

Ko lastnik sprejme licenčne pogoje, strežnik enkrat dnevno pošlje preverjanje licence: naključen ID namestitve, različico in *razpon* števila aktivnih uporabnikov. Nikoli imen, e-naslovov, domen ali podatkov imenika. Če pošiljanje ni mogoče (brez interneta), nič ne preneha delovati.

### Podprite projekt

Vprašanja in licence: info@signetmail.si. SignetMail nastaja in se vzdržuje v Sloveniji. Če vam je koristen, lahko [podprete njegov razvoj](https://signetmail.si/hvala/?l=sl).
