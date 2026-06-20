# SOUL — Spelbeskrivning & Teknisk Översikt

## Koncept

SOUL är ett Flappy Bird-inspirerat mobilspel byggt som en enda fristående HTML-fil, designad för att köras i en inbäddad webview (specifikt Claude-appens chattgränssnitt, men fungerar i alla moderna webbläsare). Spelaren styr en "själ" genom ett kosmiskt tomrum genom att trycka på skärmen för att flyga uppåt mot gravitationen, och navigerar genom en serie portaler utan att krocka.

Till skillnad från originalet Flappy Bird är SOUL byggt kring en sammanhängande andlig/filosofisk story, sju distinkta visuella världar med stigande svårighet, ett fullt progressionssystem (skins, achievements, dagliga quests), och flera lager av "polish" (ljud, delningsbilder, ceremoniella belöningsmoment).

## Storyn — "Återfärden"

Spelarens själ är ett fragment av ett större medvetande — "Källan" — som en gång splittrades och spreds ut i tomrummet. Varje portal är inte bara ett hinder utan ett minnesfragment: en glimt av vad själen en gång var. Resan genom de sju världarna är en emotionell resa från total glömska till fullständig självkännedom och återförening med Källan.

Den centrala tematiska principen, etablerad genom alla världars lore-texter: **själen och universumet är inte separata — de är samma medvetande, upplevt på olika skalor.** Universumet "vaknar" i takt med själen, "söker" efter själen lika mycket som tvärtom, och själen börjar i de senare världarna aktivt forma universumet tillbaka snarare än bara påverkas av det. Slutmeningen i sista världen vänder på premissen helt: Källan som själen sökt hela resan var aldrig utanför den.

De sju världarna (namn, poängtröskel, kärntema):
1. **Wanderer** (0p) — total vilsenhet, inget minne av vem man är
2. **Seeker** (23p) — ett mönster börjar anas, portalerna "svarar"
3. **Drifter** (45p) — tomrummet gör motstånd, men riktningen klarnar
4. **Voidwalker** (75p) — rädslan för mörkret övervinns, själen formar nu omgivningen tillbaka
5. **Starforged** (113p) — hoppet återvänder, ljuset börjar kännas nära
6. **Eternal Soul** (165p) — full självkännedom återställd
7. **The Source** (200p) — den hemliga sjunde "final-världen": ett enda fast, unikt objekt (en stor gyllene artefakt) väntar på en bestämd plats. Att nå och röra den triggar en helt egen, fullskärms "ceremoni"-sekvens (spelet fryser, en unik gyllene scen tonar in, en sista textrad visas, en exklusiv skin låses upp permanent).

## Fullständig funktionslista

**Kärnmekanik:** tryck-för-att-flyga-fysik, kollisionsdetektion mot portaler och golv/tak, poängräkning (en poäng per portal, plus bonuspoäng för samlingsobjekt), 3 liv per session med tidsbaserad återhämtning, "continue från senaste värld"-funktion vid omstart.

**Variation & svårighet:** sex visuellt distinkta världar (unika färgpaletter, stjärnfärger, nebulosa-former) plus den sjunde final-världen, gradvis ökande pelarhastighet och minskande mellanrum per värld, pelarbredd-variation (vissa "tjockare" pelare på högre poäng), rörliga pelare som oscillerar vertikalt på högre poäng.

**Samlingsobjekt:** vanliga kristaller (+3p), sällsynta "memory fragments" (+10p, rör sig i ett mönster), mycket sällsynta "core fragments" (+30p), samt slumpmässiga power-up-pickuper.

**Powers-system:** 3-slots inventory, tre kraftyper — Shield (överlever en kollision), Magnet (drar in nära kristaller automatiskt under en tidsperiod), Slow-mo (halverar tidshastigheten tillfälligt) — manuellt aktiverade genom att trycka på inventory-slottet.

**Skins:** sju själ-utseenden (sex kopplade till att nå respektive värld, en exklusiv kopplad till att samla The Source-artefakten), valbara via en dot-rad på startskärmen, påverkar själens glödfärg och kroppsgradient genomgående.

**Achievements:** tio permanenta milstolpar baserade på ackumulerad livstidsstatistik (poängrekord, totalt antal samlade objekt, antal körningar, powers använda, högsta värld nådd), visas som gyllene toast-notiser, hanterar köade samtidiga upplåsningar.

**Dagliga quests:** tre uppdrag valda deterministiskt ur en pool av sex möjliga baserat på dagens datum (samma datum ger alltid samma tre uppdrag), med progress-staplar, återställs när datumet ändras (upptäcks vid appstart eller ny omgång, ingen bakgrundstimer).

**Världsövergångar:** vid varje ny värld fryser spelet i cirka 2 sekunder (själen och pelarna stannar helt) medan världens namn och en story-mening visas, med en "Continue"-knapp för att hoppa förbi pausen direkt om man inte vill vänta.

**The Source-ceremoni:** den unika final-belöningssekvensen — separat fryst tillstånd (ingen automatisk timeout, måste avslutas manuellt), en distinkt fullskärms gyllene overlay med staged fade-in, eget ljud, permanent exklusiv skin-upplåsning.

**Ljud:** helt syntetiskt genererat via Web Audio-oscillatorer (inga externa ljudfiler) — unika ljud för flap, varje samlingsobjektstyp, power-aktivering, död, världsövergång, achievement-upplåsning, och en särskilt utsmyckad ceremoni-fanfar. Mute-knapp tillgänglig under spelets gång.

**Visuell "universum hänger ihop med själ"-effekt:** varje flap triggar en kort ljuspuls i själens egen skin-färg som sprids utåt, samtidigt som bakgrundens stjärnor lyser upp extra starkt under samma korta stund — en återkommande, mekanisk representation av storyns kärntema.

**Delningsfunktion:** efter en omgång genereras en fristående 1080x1080 resultatbild (poäng, värld, vald skin-färgad själ, story-rad) som delas via telefonens native delningsmeny (Web Share API), med nedladdning som fallback om delning inte stöds.

**Pelarvisuell design:** energi-pelare (inte enkla rektanglar) med vriden gradient-textur och en glödande elliptisk ring vid själva öppningen mot spelbart utrymme, lila för statiska pelare och orange för rörliga.

## Teknisk arkitektur

**Format:** en enda fristående `.html`-fil (~2400 rader), ingen build-process, ingen serverkod, inga externa beroenden eller nätverksanrop av något slag. Öppnas direkt i en webbläsare eller webview.

**Rendering:** HTML5 Canvas 2D för allt spelvisuellt innehåll (bakgrund, pelare, samlingsobjekt, själen). Vanlig DOM/CSS för UI-skärmar (startskärm, dödsskärm, achievement-toasts, quest-panel, ceremoni-overlay) som ligger ovanpå canvasen.

**Game loop:** en enda `requestAnimationFrame`-loop (`render()`) som anropar `update()` (fysik, kollision, state-hantering) och sedan ritar varje frame. Medvetet undviker flera parallella loopar eller `setInterval`-baserad logik för kontinuerlig uppdatering, eftersom det orsakade en tidigare instabilitetsbugg i den specifika inbäddade webviewen detta är byggt för.

**Tillstånd:** allt spelartillstånd hålls i vanliga JavaScript-variabler i minnet (ingen `localStorage`, `sessionStorage`, eller annan webbläsarlagring) — ett medvetet designval eftersom `localStorage` visade sig kasta `SecurityError` och krascha spelet i den aktuella webviewen. Konsekvensen är att framsteg (skins upplåsta, achievements, högsta värld nådd) bara varar under den pågående sessionen/fliken, inte mellan app-omstarter.

**Ljud:** Web Audio API med oscillator-noder skapade programmatiskt för varje ljudeffekt (ingen ljudfil laddas någonsin). `AudioContext` skapas lazy vid första interaktion eftersom webbläsare blockerar autoplay av ljud innan det.

**Bilder:** startskärmens bakgrundsbild (AI-genererad nyckelbild) är inbäddad direkt i HTML-filen som en base64-kodad data-URL, inte en extern fil-länk — håller filen helt fristående men ökar filstorleken (~420KB totalt).

**Robusthet:** varje funktion som rör ljud eller delning är omsluten i try/catch så att ett misslyckande (t.ex. ingen AudioContext tillgänglig, eller användaren avbryter delningsdialogen) aldrig kraschar eller stör spelet. Samlingsobjektens positioner har alltid en hård "safety clamp" utöver den teoretiska matematiken, för att garantera de förblir nåbara inom det spelbara utrymmet oavsett edge-cases.

**Utvecklingsmetodik:** byggt stegvis, en funktion i taget, där varje steg testas via simulerad DOM-miljö (jsdom) innan leverans — kör hundratals simulerade spel-frames, kontrollerar att inga JavaScript-fel uppstår, och verifierar specifika beteenden (t.ex. att kollisioner triggas korrekt, att state återställs ordentligt mellan omgångar) innan koden anses klar.
