# Podobnosť a protiklady postojov — data mining v SAS 🧩

> 🇬🇧 *Data-mining project (Masaryk University): can 29 binary attitude statements from a survey of 2,889 Czech respondents be reduced to fewer dimensions — and which statements are similar or opposite? Four complementary methods in SAS: variable clustering (VARCLUS with a covariance-weighted metric), PCA, multidimensional scaling on a Hamming distance matrix, and market-basket association analysis — including a negation trick to surface *opposite* statements. Report-centric repo (SAS ran on university infrastructure; scripts are not preserved). Write-up in Slovak.*

Projekt z predmetu Data mining (Prírodovedecká fakulta, Masarykova univerzita). 

**Otázka:** Dajú sa výroky o živote zredukovať na menší počet tvrdení — a ktoré výroky sú si podobné či protikladné?

**Dáta:** reprezentatívny prieskum populácie ČR (~2000), 2 889 respondentov, 29 binárnych výrokov (áno/nie) + demografia.

## Prístup — štyri metódy na jeden problém

Každá metóda má pri binárnych dátach iné silné a slabé stránky, preto som ich kombinovala a porovnávala:

**Zhluková analýza premenných (VARCLUS)** — netradične na stĺpce namiesto riadkov: zoskupiť výroky tak, aby v rámci klastra korelovali a medzi klastrami nie. Namiesto defaultnej korelačnej metriky som zvolila **COVARIANCE** — pri binárnych premenných tým dostanú väčšiu váhu výroky s podielom odpovedí bližšie k 0,5, teda tie najviac rozlišujúce.

**PCA** — pre binárne dáta metodicky nevhodná (predpoklad normality neplatí), zaradená zámerne ako kontrast; jej výsledky potvrdili, že VARCLUS je pre tieto dáta viac smerodatný.

**Mnohorozmerné škálovanie (MDS)** — na matici **Hammingových vzdialeností** medzi výrokmi; porovnanie LEVEL=ABSOLUTE vs. ORDINAL podľa fit plotu (ordinálne škálovanie sedelo lepšie). Výsledná 2D projekcia umožnila dimenzie interpretovať ako „stabilita" a „individualizmus vs. prispôsobenie".

**Asociačná analýza (SAS Miner)** — analýza nákupného košíka prenesená na výroky: ktoré tvrdenia respondenti označujú spolu (support, confidence, lift). A navrch trik na **protikladné výroky**: k dátam som vygenerovala negácie výrokov a hľadala pravidlá typu Výrok1 → NOT Výrok2 — napr. „Robím, čo chcem" sa vylučuje s harmonickou rodinou, dodržiavaním zákonov a neriskovaním.

## Hlavné zistenia

Výroky netvoria niekoľko oddelených zhlukov — skôr jeden hustý stred s postupne sa pripájajúcimi bodmi. Pre vernú redukciu vychádza ~20 klastrov (s dvomi malými výraznými skupinami: „úspech a moc" a „konvencie a vízia"); pre výraznú redukciu 7 klastrov za cenu poklesu vysvetlenej variability na 36 %. Najsilnejšie asociácie sa viažu na výrok „Mám radosť"; protikladné pravidlá dosahujú vyššiu spoľahlivosť než podobnostné.

Kompletná analýza s tabuľkami, dendrogramami, MDS projekciami a asociačnými pravidlami je v **[reporte (PDF)](attitudes_datamining_report.pdf)**.

## Dáta a reprodukovateľnosť

Surové mikrodáta (individuálne odpovede vrátane demografie a príjmov) boli poskytnuté v rámci predmetu a **nie sú súčasťou repozitára** — ich pôvodný vlastník a licencia mi nie sú známe, preto nie sú súčasťou repa. 
Súčasťou repa je odvodená **matica Hammingových vzdialeností 29 × 29 výrokov** (`hamming_distance_matrix.xlsx`) — agregát bez individuálnych údajov, vstup pre MDS.

Analýza bežala v **SAS** (procedúry VARCLUS, PRINCOMP, MDS) a **SAS Enterprise Miner** (asociačná analýza) na univerzitnej infraštruktúre; 
skripty som si zabudla odložiť a konto na prírodovedeckej fakulte už mám asi zrušené, repo je preto postavené na reporte. 
Metodika je v ňom popísaná vari dostatočne podrobne na replikáciu v SAS, či mimo neho (R: `ClustOfVar`/`hclust`, `smacof` pre MDS, `arules` pre asociácie).

## Autorka

Mgr. Dana Kozáková — štatistika, matematika, dáta, AI.
