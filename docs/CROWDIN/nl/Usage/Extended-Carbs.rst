Vertraagde koolhydraten / "eCarbs"
**************************************************
What are eCarbs and when are they useful?
==================================================
Bij reguliere pomptherapie zijn vertraagde bolussen / multiwave bolussen handig voor vette maaltijden (pizza, pasta, pannenkoeken etc) die je bloedglucose lange tijd laten stijgen, langer dan de insuline bij een normale bolus werkzaam zou zijn. In een closed loop is dat echter niet zo zinvol (en geeft het technische problemen) omdat vertraagde/multiwave bolussen door de pomp worden uitgevoerd als een langdurige en vooraf gedefinieerde hoge basaal. Dat conflicteert met de loop, die de basaalstanden juist dynamisch aanpast. Voor meer informatie, zie `vertraagde bolus <../Usage/Extended-Carbs.html#vertraagde-bolus>`__ hieronder.

En dus moet de loop op een andere manier omgaan met dit soort maaltijden. Daarom is in AndroidAPS vanaf versie 2.0 een optie ingebouwd die "eCarbs" heet. Dat staat voor extended Carbs (vertraagde koolhydraten).

eCarbs zijn koolhydraten die over meerdere uren worden uitgespreid. Voor standaard maaltijden met meer koolhydraten dan vet/eiwit, is het gebruik van eCarbs niet persé nodig. Daarbij is het voldoende om een deel van de koolhydraten in te voeren via de bolus calculator (daarvoor wordt dan meteen gebolust), en het resterende deel van de koolhydraten in te voeren via de Koolhydraten knop (daarvoor geeft de loop gedurende een langere periode SMB's af, gebaseerd op de BG stijging die hij ziet). Dit voorkomt een al te snelle insulineafgifte en dus een hypo na de maaltijd. <br>  Maar voor langzamer absorberende maaltijden kunnen beter wel eCarbs worden gebruikt. Anders zou (zeker bij gebruik van SMB) de IOB veel te snel oplopen terwijl de maaltijd je BG slechts langzaam laat stijgen, met een hypo als resultaat. Dankzij eCarbs wordt veel beter gesimuleerd hoe de koolhydraten (en evt. ook alle koolhydraat-equivalenten die je invoert voor vetten/eiwitten) worden geabsorbeerd en hoe die invloed hebben op je BG. Met deze informatie kan de loop geleidelijk aan SMB's afgeven om die koolhydraten aan te pakken. Deze methode zou je als een dynamische vorm van een vertraagde/multiwave bolus kunnen zien. (Opmerking: we gaan er hierbij vanuit dat je SMB's gebruikt, dit zou ook moeten werken zonder SMB's, maar is waarschijnlijk minder effectief).

**Note:** eCarbs aren't limited to fatty / protein heavy meals: they can be also be used to help in any situation where there are influences that increase the blood sugar, e.g. zoals corticosteroïden.

Mechanics of using eCarbs
==================================================
To enter eCarbs, set a duration in the *Carbs* dialog on the overview tab, the total carbs and optionally a time shift (*numbers below are just examples, you will need to try your own values to arrive at satisfactory glucose response for your use-cases*):

.. image:: ../images/eCarbs_Dialog.png
  :alt: Koolhydraten invoeren

Je ziet de eCarbs in de grafiek terug als kleine beetjes van 1 a 2 gram in de toekomst. En je ziet dat er nog 9 gram koolhydraten achter COB staan, tussen haakjes omdat het toekomstige koolhydraten zijn.

.. image:: ../images/eCarbs_Graph.png
  :alt: eCarbs in grafiek

Op de Behandelingen tab zie je koolhydraten staan, ze zijn donker oranje omdat ze in de toekomst zijn:

.. image:: ../images/eCarbs_Treatment.png
  :alt: vertraagde koolhydraten in de Behandelingen tab


-----

Hoe je vertraagde koolhydraten gebruikt voor een maaltijd met vet en eiwit wordt hier beschreven: `https://adriansloop.blogspot.com/2018/04/page-margin-0.html <https://adriansloop.blogspot.com/2018/04/page-margin-0.html>`_

-----

Recommended setup, example scenario, and important notes
=====================================================================
Voor onderstaand voorbeeld wordt aangeraden om OpenAPS SMB APS te gebruiken, met SMB ingeschakeld en de instelling *SMB met koolhydraten* ingeschakeld te hebben.

Neem nou bijvoorbeeld een pizza, daarbij kun je een gedeeltelijke bolus voor de maaltijd via de bolus calculator geven en via de knop *Koolhydraten* de resterende koolhydraten gedurende 4 a 6 uur, te laten beginnen na 1 of 2 uur. 

**Important notes:** You'll need to try out and see which concrete values work for you of course. Hierbij kun je ook (in kleine, verstandige stapjes) variëren met de instelling *max aantal minuten basaal om de SMB te limiteren tot* om het algoritme agressiever/minder agressief te laten zijn.
Voor maaltijden met nauwelijks koolhydraten en een hoog vet/eiwitgehalte kan het genoeg zijn om alleen eCarbs te gebruiken en geen bolus (zie de blogpost hierboven). Iedere keer dat je eCarbs invoert, wordt er in jouw Careportal automatisch een opmerking toegevoegd. Hierdoor kun je de eerdere keren gemakkelijk terugvinden, zodat je jouw aanpak kunt evalueren en verbeteren voor de volgende keer. Of gewoon herhalen wat je eerder deed, als dat voor jou goed werkte.

Extended bolus and why they won't work in closed-loop environment?
=====================================================================
Zoals hierboven vermeld, werken vertraagde of multiwave bolussen niet echt in een closed loop. `See below <../Usage/Extended-Carbs.html#why-extended-boluses-won-t-work-in-a-closed-loop-environment>`_ for details

Vertraagde bolus en overschakelen naar open loop - alleen voor Dana en Insight pomp
-----------------------------------------------------------------------------
Sommige mensen wilden dolgraag een optie hebben om een vertraagde bolus te kunnen gebruiken in AAPS omdat ze voor bepaalde voedingsmiddelen willen bolussen zoals ze eerder gewend waren. 

Daarom is er vanaf versie 2.6 een optie voor een vertraagde bolus toegevoegd, deze werkt alleen met Dana en Insight pompen. 

* De closed loop zal automatisch worden onderbroken en er wordt overgeschakeld naar open loop modus nadat de vertraagde bolus functie is geactiveerd. 
* Bolus eenheden, resterende en totale tijd zullen worden weergegeven op het Overzicht-scherm.
* NB: Op de Insight pomp is de vertraagde bolus optie *niet beschikbaar* als `TBR emulatie <../Configuration/Accu-Chek-Insight-Pump.html#settings-in-aaps>`_ is ingeschakeld. 

.. image:: ../images/ExtendedBolus2_6.png
  :alt: vertraagde bolus in AAPS 2.6

Waarom vertraagde bolussen niet werken in een closed loop
----------------------------------------------------------------------------------------------------
1. The loop determines that now 1.55U/h is to be delivered. Whether this is delivered as an extended bolus or TBR does not matter to the algorithm. In fact, some of the pumps use the extended bolus. What should happen then? Most pump drivers then stop the extended bolus -> You didn't even need to start it.
2. If you had the extended bolus as input, what should happen in the model?

   1. Should it be considered neutral together with the BR and looped on it? Then the loop should also be able to reduce the bolus if, for example, you get too low and all the "neutral" insulin is taken away?
   2. Should the extended bolus simply be added? So the loop should simply be allowed to continue? Even in the worst hypo? I don't think this is so good: A hypo is foreseen but it must not be prevented?
   
3. The IOB that the extended bolus builds up materializes after 5 minutes at the next run. Accordingly, the loop would give less basal. So not much changes... except that the possibility of hypo avoidance is taken.
