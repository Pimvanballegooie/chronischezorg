# VindJeFysio Netwerk — chronischezorg.net (Chronische Zorg Netwerk)

Deze repo is één "spoke" in een hub-and-spoke netwerk van gespecialiseerde fysiotherapie-subsites. De hub is vindjefysio.net; spokes zijn o.a. rugnek, enkelvoet, beenklachten, kansrijkopgroeien, mentaalgezond, armklachten en deze (chronischezorg).

## Architectuur
- Statische HTML/CSS/vanilla JS op GitHub Pages, custom domein via CNAME.
- Gedeelde Supabase-backend, project islujznszevdynguhjdc, met anon key in de frontend.
- Gedeelde tabellen: therapeuten, praktijken, therapeut_subcategorieen, therapeut_praktijken, subcategorieen, categorieen.
- Elke spoke deelt dezelfde structuur en bestanden: index.html, therapeut-aanmelden.html, mijn-profiel.html, therapeuten.html, protocollen.html (gegenereerd), sync_protocollen.py, .github/workflows/sync.yml, protocollen-config.json.

## Belangrijke conventies
- therapeut-aanmelden.html linkt ALTIJD relatief/lokaal binnen de eigen subsite (nooit naar vindjefysio.net).
- Praktijk/locatie-aanmelden loopt WEL centraal via vindjefysio.net/aanmelden.html?via=<domein>.
- Mails lopen via info@vindjefysio.net.
- Therapeut-registratie zet aangemeld_via op het eigen subsite-domein en actief=false (wacht op goedkeuring).
- Deze site: palet primair blauw #2E86C1 (CSS-variabele heet `--teal` maar is feitelijk blauw, met `--teal-dark` #246EA0), navy #1F3A52. index.html bevat daarnaast een gedeelde utility-set groen #6ABF4B, blauw #2E7DD1, oranje #F5A623 (niet het hoofdpalet van déze site).
- Structuur = drie DOMEINEN (elk een eigen kleur, zie `DOMEINEN`-object in index.html):
  - Hart, vaat & longen (kleur #C43A3A) — longklachten, etalagebenen, hartrevalidatie
  - Kanker, pijn & vermoeidheid (kleur #2E86C1) — oncologische revalidatie, chronische pijn, chronische vermoeidheid
  - Bewegen & conditie (kleur #3BAE8C) — diabetes en bewegen, reuma/artrose/botgezondheid, voorbereiding op operatie
- Deze DOMEINEN-config bevat een leeggekopieerde waarschuwingscomment over subcategorie-id 999 ("Borst- en ribklachten"); die placeholder is overgenomen uit de armklachten-template en is hier niet van toepassing — alle negen subcategorieën van deze site (id's 951–959) bestaan al in Supabase.

## Sync-pipeline
- sync_protocollen.py haalt protocollen op uit publiek gedeelde Google Docs (export-link, geen API-key), zet markdown om naar HTML, genereert protocollen/<id>-makkelijk.html (patiënt) en -complex.html (therapeut) + protocollen.html + sitemap.xml.
- De workflow git-add regel moet zijn: git add protocollen/ protocollen.html sitemap.xml (op één regel).
- Google Docs moeten op "iedereen met de link kan bekijken" staan.
