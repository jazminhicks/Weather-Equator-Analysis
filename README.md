# Weather-Py

Create a Python script to visualize the weather of 500+ cities across the world of varying distance from the equator. 

To accomplish this, utilize https://pypi.python.org/pypi/citipy) and https://openweathermap.org/api)

Objective is to build a series of scatter plots to showcase the following relationships:

* Temperature (F) vs. Latitude
* Humidity (%) vs. Latitude
* Cloudiness (%) vs. Latitude
* Wind Speed (mph) vs. Latitude

The notebook includes:

* At least 500 randomly selected unique (non-repeat) cities based on latitude and longitude.
* A weather check on each of the cities using a series of successive API calls.
* A print log of each city as it's being processed with the city number and city name.
* Both a CSV of all data retrieved and png images for each scatter plot.
____________________________________________________________


#### Observable Trends

* Differing from expectations, the hottest cities are not necessarily those on or closest to the equator (latitude of zero degrees). Rather, the highest temperatures are from cities with latitudes around 20 degrees. This result may be combination of the earth’s tilt and varying weather patterns throughout the tropics. 

* The data does not present any correlation between a city’s humidity and latitude nor cloudiness and latitude. On both graphs the data points are distributed fairly evenly and there is no distinctive pattern.

* Additionally, there is not any correlation between a city’s average wind speed and latitude. However, the data does reveals that wind speeds in most cities typically do not exceed 20 mph. 

## List of Cities (from randomly generated latitudes and longitudes)

```
# Create a set of random lat and lng combinations
# use citipy to identify the city,country that matches the provided coordinates

lats = np.random.uniform(-90.000, 90.000, size=2000)
lngs = np.random.uniform(-180.000, 180.000, size=2000)

lat_lng = list(zip(lats, lngs))

cities = []
countries = []

for coordinate in lat_lng:
    
    city = citipy.nearest_city(coordinate[0], coordinate[1]).city_name
    code = citipy.nearest_city(coordinate[0], coordinate[1]).country_code
    if city not in cities:
        cities.append(city)
        countries.append(code)

city_country = list(zip(cities, countries))

len(city_country)
```

## Perform API Calls (for each city)
```
url = "http://api.openweathermap.org/data/2.5/weather?"

unit = "imperial" 

query_url = f"{url}appid={api_key}&units={unit}&q="
```
```
print ("Beginning data retrieval...")
print ("----------------------------")

# establish lists to store the specified data for each city from the api
found_cities = []
found_countries = []
found_lats = []
found_lngs = []
dates = []
cloudiness = []
humidity = []
max_temp = []
wind_speed = []

count = 0

for place in city_country:
    
    query = place[0].replace(" ", "+") + "," + place[1].replace(" ", "+")
    
    city_data = requests.get(query_url + query).json()
    
    if city_data["cod"] == "404":
        print(f"Error: '{place[0]}' was not found...skippping")
    

    else:
        count += 1
        print (f"{count}. Processing data for {place[0]}") # print log of each city that was found and is being processed
        print (query_url + query)
        found_cities.append(place[0])
        found_countries.append(place[1])
        dates.append(city_data["dt"])
        cloudiness.append(city_data["clouds"]["all"])
        humidity.append(city_data["main"]["humidity"])
        max_temp.append(city_data["main"]["temp_max"])
        wind_speed.append(city_data["wind"]["speed"])
        found_lats.append(city_data["coord"]["lat"])
        found_lngs.append(city_data["coord"]["lon"])

print ("--------------------------")
print ("Data retrieval complete")
print ("--------------------------")
```

```
Beginning data retrieval...
----------------------------
1. Processing data for jamestown
2. Processing data for voh
3. Processing data for nikolskoye
4. Processing data for butaritari
5. Processing data for cape town
6. Processing data for kandrian
7. Processing data for nhulunbuy
8. Processing data for guerrero negro
9. Processing data for quatre cocos
10. Processing data for usinsk
11. Processing data for upata
12. Processing data for vaini
13. Processing data for castro
14. Processing data for sterling
15. Processing data for khatanga
16. Processing data for atuona
17. Processing data for punta arenas
Error: 'ngukurr' was not found...skippping
18. Processing data for lompoc
19. Processing data for ribeira grande
20. Processing data for rikitea
Error: 'laguna' was not found...skippping
21. Processing data for muroto
Error: 'mataura' was not found...skippping
22. Processing data for hasaki
23. Processing data for necochea
24. Processing data for hilo
25. Processing data for bang saphan
26. Processing data for jaciara
Error: 'taolanaro' was not found...skippping
27. Processing data for ovre ardal
28. Processing data for albany
29. Processing data for aykhal
30. Processing data for dawlatabad
31. Processing data for katsuura
32. Processing data for plouzane
Error: 'codrington' was not found...skippping
33. Processing data for ust-ishim
34. Processing data for sao felix do xingu
35. Processing data for ketchikan
36. Processing data for saskylakh
37. Processing data for tiksi
38. Processing data for lorengau
39. Processing data for kahului
40. Processing data for ambilobe
41. Processing data for arraial do cabo
42. Processing data for avarua
43. Processing data for port lincoln
44. Processing data for tutoia
45. Processing data for dormidontovka
46. Processing data for niteroi
47. Processing data for okhotsk
48. Processing data for sapele
49. Processing data for pasighat
50. Processing data for hithadhoo
51. Processing data for busselton
52. Processing data for rocha
53. Processing data for luanda
54. Processing data for dudinka
55. Processing data for puerto ayora
56. Processing data for georgetown
57. Processing data for grindavik
58. Processing data for vila franca do campo
59. Processing data for nome
60. Processing data for mar del plata
61. Processing data for havre-saint-pierre
Error: 'tsihombe' was not found...skippping
62. Processing data for chokurdakh
63. Processing data for fortuna
Error: 'airai' was not found...skippping
64. Processing data for port alfred
65. Processing data for victoria
66. Processing data for trelew
67. Processing data for bluff
68. Processing data for olga
69. Processing data for sovetskaya gavan
70. Processing data for bilibino
71. Processing data for port pirie
72. Processing data for vysokogornyy
73. Processing data for luau
74. Processing data for bredasdorp
Error: 'mullaitivu' was not found...skippping
75. Processing data for sobolevo
76. Processing data for almaznyy
77. Processing data for colonelganj
78. Processing data for carnarvon
Error: 'nizhneyansk' was not found...skippping
79. Processing data for kavieng
80. Processing data for biak
Error: 'krasnoselkup' was not found...skippping
81. Processing data for saint peters
82. Processing data for taoudenni
83. Processing data for ushuaia
84. Processing data for sao borja
85. Processing data for veraval
Error: 'samusu' was not found...skippping
86. Processing data for ayabe
87. Processing data for gobabis
88. Processing data for ulaangom
89. Processing data for lindi
90. Processing data for umm kaddadah
91. Processing data for hermanus
92. Processing data for tamuin
93. Processing data for tommot
94. Processing data for rudnichnyy
95. Processing data for zwedru
96. Processing data for ponta do sol
97. Processing data for riyadh
Error: 'amderma' was not found...skippping
Error: 'gat' was not found...skippping
98. Processing data for geraldton
99. Processing data for megion
Error: 'cazaje' was not found...skippping
100. Processing data for owando
101. Processing data for te anau
102. Processing data for westport
Error: 'attawapiskat' was not found...skippping
103. Processing data for dikson
104. Processing data for assiniboia
105. Processing data for cidreira
106. Processing data for itarema
107. Processing data for hobart
108. Processing data for ancud
109. Processing data for thompson
Error: 'tumannyy' was not found...skippping
110. Processing data for talcahuano
111. Processing data for el playon
112. Processing data for topi
Error: 'barentsburg' was not found...skippping
Error: 'palabuhanratu' was not found...skippping
113. Processing data for coahuayana
114. Processing data for ginir
115. Processing data for faanui
116. Processing data for san jose
117. Processing data for soyo
118. Processing data for saint-philippe
119. Processing data for teguise
120. Processing data for havoysund
Error: 'goderich' was not found...skippping
121. Processing data for morehead
122. Processing data for maryville
123. Processing data for esperance
124. Processing data for novaya mayna
Error: 'illoqqortoormiut' was not found...skippping
125. Processing data for jiwani
126. Processing data for qaanaaq
127. Processing data for port hardy
128. Processing data for cherskiy
129. Processing data for zaria
130. Processing data for bom jesus
131. Processing data for tasiilaq
132. Processing data for kungurtug
133. Processing data for champerico
134. Processing data for flinders
135. Processing data for miri
136. Processing data for kruisfontein
137. Processing data for iquique
138. Processing data for johnson city
139. Processing data for saldanha
140. Processing data for talnakh
141. Processing data for san patricio
142. Processing data for pevek
143. Processing data for longyearbyen
144. Processing data for namanyere
Error: 'mys shmidta' was not found...skippping
145. Processing data for lere
146. Processing data for severo-kurilsk
147. Processing data for la tuque
148. Processing data for farafenni
149. Processing data for ursulo galvan
150. Processing data for vardo
Error: 'tillabery' was not found...skippping
151. Processing data for vao
152. Processing data for barrow
153. Processing data for honiara
154. Processing data for miraflores
155. Processing data for namibe
156. Processing data for freeport
157. Processing data for chuy
158. Processing data for oranjestad
Error: 'fowa' was not found...skippping
159. Processing data for panacan
160. Processing data for bulgan
161. Processing data for yellowknife
162. Processing data for ahipara
163. Processing data for turka
164. Processing data for new norfolk
165. Processing data for san vicente de canete
166. Processing data for channel-port aux basques
167. Processing data for bell ville
168. Processing data for salta
169. Processing data for gazojak
170. Processing data for burgersdorp
Error: 'sentyabrskiy' was not found...skippping
171. Processing data for andenes
172. Processing data for marathon
173. Processing data for elmadag
Error: 'bengkulu' was not found...skippping
174. Processing data for nanortalik
175. Processing data for honningsvag
Error: 'vaitupu' was not found...skippping
176. Processing data for neiafu
177. Processing data for santo tomas
178. Processing data for souillac
179. Processing data for tuktoyaktuk
180. Processing data for lagoa
181. Processing data for kapaa
182. Processing data for dukat
183. Processing data for pangnirtung
184. Processing data for muros
185. Processing data for ushtobe
186. Processing data for aklavik
187. Processing data for dehloran
Error: 'alotau' was not found...skippping
188. Processing data for kodiak
189. Processing data for usoke
190. Processing data for lebu
191. Processing data for tilik
192. Processing data for gravdal
193. Processing data for kaitangata
194. Processing data for sept-iles
195. Processing data for northam
196. Processing data for tautira
197. Processing data for aljezur
198. Processing data for vostok
199. Processing data for quata
Error: 'grand river south east' was not found...skippping
200. Processing data for matagami
201. Processing data for surt
202. Processing data for hami
203. Processing data for torbay
204. Processing data for cabo san lucas
205. Processing data for ilulissat
Error: 'malakal' was not found...skippping
Error: 'alekseyevka' was not found...skippping
206. Processing data for east london
Error: 'belushya guba' was not found...skippping
207. Processing data for leningradskiy
208. Processing data for clyde river
209. Processing data for krizevci
Error: 'atka' was not found...skippping
210. Processing data for lima
211. Processing data for anadyr
212. Processing data for xining
213. Processing data for bambous virieux
214. Processing data for bitkine
215. Processing data for manzhouli
216. Processing data for kununurra
Error: 'vestbygda' was not found...skippping
Error: 'formoso do araguaia' was not found...skippping
Error: 'lasa' was not found...skippping
217. Processing data for san cristobal
218. Processing data for yerbogachen
219. Processing data for maceio
220. Processing data for sao joao da barra
Error: 'kismayo' was not found...skippping
221. Processing data for homer
Error: 'urumqi' was not found...skippping
Error: 'humaita' was not found...skippping
222. Processing data for bud
223. Processing data for aksu
224. Processing data for padang
225. Processing data for luderitz
226. Processing data for conneaut
227. Processing data for denpasar
228. Processing data for provideniya
229. Processing data for norman wells
230. Processing data for tigil
231. Processing data for ixtapa
Error: 'san quintin' was not found...skippping
Error: 'asau' was not found...skippping
232. Processing data for sinnamary
233. Processing data for novobirilyussy
234. Processing data for ostrovnoy
235. Processing data for ocos
236. Processing data for tiarei
Error: 'sorvag' was not found...skippping
237. Processing data for saint george
238. Processing data for ishigaki
239. Processing data for mahebourg
Error: 'bereda' was not found...skippping
240. Processing data for natal
Error: 'hai phong' was not found...skippping
241. Processing data for srivardhan
242. Processing data for ballina
Error: 'kamenskoye' was not found...skippping
243. Processing data for kidal
244. Processing data for banfora
Error: 'talipparamba' was not found...skippping
245. Processing data for mount isa
246. Processing data for plock
247. Processing data for coquimbo
248. Processing data for moose factory
Error: 'fort saint john' was not found...skippping
249. Processing data for sorland
250. Processing data for novosysoyevka
251. Processing data for port elizabeth
252. Processing data for beringovskiy
253. Processing data for berbera
254. Processing data for ampanihy
Error: 'saryshagan' was not found...skippping
255. Processing data for klaksvik
256. Processing data for buchanan
257. Processing data for kloulklubed
258. Processing data for kayerkan
259. Processing data for wanning
Error: 'bargal' was not found...skippping
Error: 'olafsvik' was not found...skippping
260. Processing data for alushta
261. Processing data for farafangana
262. Processing data for priiskovyy
263. Processing data for college
264. Processing data for sunndalsora
Error: 'punta de piedra' was not found...skippping
265. Processing data for hamza
266. Processing data for joshimath
267. Processing data for kirakira
268. Processing data for kajaani
269. Processing data for berlevag
270. Processing data for pisco
271. Processing data for mao
272. Processing data for lakhipur
273. Processing data for pochutla
274. Processing data for valdivia
275. Processing data for changde
276. Processing data for altamira
277. Processing data for pailon
Error: 'mrirt' was not found...skippping
278. Processing data for rincon
279. Processing data for bandarbeyla
280. Processing data for magrath
281. Processing data for galle
282. Processing data for plymouth
283. Processing data for boende
284. Processing data for shellbrook
285. Processing data for salalah
286. Processing data for touros
287. Processing data for hobyo
288. Processing data for san rafael
289. Processing data for husavik
Error: 'kristiinankaupunki' was not found...skippping
290. Processing data for santa lucia
291. Processing data for poum
292. Processing data for motupe
293. Processing data for yeppoon
294. Processing data for kieta
295. Processing data for seymchan
296. Processing data for chara
297. Processing data for balad
298. Processing data for nantucket
299. Processing data for psedakh
300. Processing data for wilmington
301. Processing data for broome
302. Processing data for sao raimundo das mangabeiras
303. Processing data for huilong
304. Processing data for katangi
305. Processing data for los llanos de aridane
306. Processing data for robertsport
307. Processing data for rognan
Error: 'yian' was not found...skippping
308. Processing data for san matias
309. Processing data for axim
310. Processing data for karratha
311. Processing data for ust-nera
312. Processing data for quzhou
313. Processing data for novyy urengoy
314. Processing data for salinopolis
315. Processing data for the pas
316. Processing data for banjar
Error: 'altonia' was not found...skippping
317. Processing data for caruray
318. Processing data for bull savanna
319. Processing data for pastavy
320. Processing data for dunedin
321. Processing data for sao filipe
322. Processing data for bundaberg
323. Processing data for mwene-ditu
324. Processing data for matay
325. Processing data for vung tau
326. Processing data for aleksin
327. Processing data for chippewa falls
328. Processing data for turukhansk
329. Processing data for yar-sale
Error: 'dekoa' was not found...skippping
330. Processing data for petrykivka
331. Processing data for eydhafushi
332. Processing data for prince george
333. Processing data for mount gambier
334. Processing data for christchurch
335. Processing data for banikoara
336. Processing data for monrovia
337. Processing data for sibolga
338. Processing data for wajima
339. Processing data for maxixe
340. Processing data for dingle
341. Processing data for nicoya
342. Processing data for port macquarie
343. Processing data for raudeberg
344. Processing data for mutare
345. Processing data for shimoda
346. Processing data for naze
347. Processing data for benjamin constant
348. Processing data for port augusta
349. Processing data for leninsk
350. Processing data for gayeri
Error: 'jiddah' was not found...skippping
351. Processing data for asyut
352. Processing data for paraiso
353. Processing data for gorontalo
354. Processing data for storforshei
355. Processing data for port blair
356. Processing data for olinda
357. Processing data for hambantota
358. Processing data for tagusao
359. Processing data for goundam
360. Processing data for larsnes
361. Processing data for liverpool
Error: 'avera' was not found...skippping
362. Processing data for mitsamiouli
363. Processing data for yulara
364. Processing data for lasem
365. Processing data for shenjiamen
366. Processing data for boguchany
367. Processing data for aban
368. Processing data for petropavlovsk-kamchatskiy
369. Processing data for kiruna
370. Processing data for luganville
371. Processing data for cassia
372. Processing data for katangli
373. Processing data for tshane
374. Processing data for kedrovyy
375. Processing data for hirara
376. Processing data for marsh harbour
Error: 'yunjinghong' was not found...skippping
377. Processing data for solnechnyy
378. Processing data for cairns
379. Processing data for shingu
380. Processing data for gigmoto
381. Processing data for isangel
382. Processing data for andra
383. Processing data for alexandria
384. Processing data for bethel
385. Processing data for norden
Error: 'sataua' was not found...skippping
386. Processing data for hofn
387. Processing data for peoria
Error: 'braslav' was not found...skippping
388. Processing data for dongsheng
389. Processing data for ibra
390. Processing data for portobelo
Error: 'wahran' was not found...skippping
391. Processing data for vila velha
392. Processing data for machakos
393. Processing data for babu
394. Processing data for benghazi
395. Processing data for kasane
396. Processing data for kefamenanu
397. Processing data for kitui
398. Processing data for ucluelet
399. Processing data for roald
Error: 'saleaula' was not found...skippping
Error: 'kadykchan' was not found...skippping
400. Processing data for atambua
401. Processing data for tura
402. Processing data for upernavik
403. Processing data for novosil
404. Processing data for marsabit
405. Processing data for la ronge
406. Processing data for vestmannaeyjar
407. Processing data for ban nahin
408. Processing data for suhbaatar
409. Processing data for namatanai
410. Processing data for mogadishu
411. Processing data for kamogawa
412. Processing data for aguada de cima
413. Processing data for general roca
414. Processing data for dhidhdhoo
415. Processing data for igrim
416. Processing data for belaya gora
417. Processing data for celestun
418. Processing data for demba
419. Processing data for kulhudhuffushi
420. Processing data for mizdah
421. Processing data for kupang
422. Processing data for tumen
423. Processing data for abu kamal
Error: 'tukrah' was not found...skippping
424. Processing data for ulaanbaatar
425. Processing data for ouargaye
426. Processing data for dese
427. Processing data for kyzyl-suu
428. Processing data for oil city
429. Processing data for valparaiso
430. Processing data for vaitape
Error: 'asfi' was not found...skippping
431. Processing data for colac
432. Processing data for palmer
Error: 'igarape-acu' was not found...skippping
433. Processing data for namuac
434. Processing data for caxito
435. Processing data for wajir
Error: 'lata' was not found...skippping
436. Processing data for calmar
437. Processing data for korla
438. Processing data for santa fe
439. Processing data for ukiah
440. Processing data for margate
Error: 'sumbawa' was not found...skippping
441. Processing data for balabac
442. Processing data for big rapids
Error: 'pamfila' was not found...skippping
443. Processing data for harper
Error: 'birin' was not found...skippping
444. Processing data for kaseda
Error: 'requena' was not found...skippping
445. Processing data for oliver
Error: 'kazalinsk' was not found...skippping
446. Processing data for prainha
447. Processing data for shahr-e kord
448. Processing data for cururupu
449. Processing data for kadoshkino
450. Processing data for zhaotong
451. Processing data for suntar
452. Processing data for road town
453. Processing data for shiloh
454. Processing data for lermontovka
455. Processing data for thinadhoo
456. Processing data for antalaha
457. Processing data for subaan
458. Processing data for comodoro rivadavia
Error: 'talawdi' was not found...skippping
459. Processing data for aracati
460. Processing data for tongchuan
461. Processing data for san luis
462. Processing data for adrar
Error: 'balimo' was not found...skippping
463. Processing data for simao
464. Processing data for corinto
Error: 'galiwinku' was not found...skippping
465. Processing data for paita
466. Processing data for kadirli
467. Processing data for sao gabriel da cachoeira
468. Processing data for aswan
469. Processing data for kavaratti
470. Processing data for lynge
471. Processing data for tupik
472. Processing data for tambovka
473. Processing data for santa isabel do rio negro
Error: 'kerteh' was not found...skippping
Error: 'kawana waters' was not found...skippping
474. Processing data for singarayakonda
475. Processing data for praia da vitoria
476. Processing data for sawtell
477. Processing data for port hedland
Error: 'faya' was not found...skippping
478. Processing data for adeje
479. Processing data for da lat
480. Processing data for cayenne
481. Processing data for annau
482. Processing data for amahai
483. Processing data for calamar
484. Processing data for angoram
485. Processing data for grand gaube
486. Processing data for pyu
487. Processing data for huarmey
488. Processing data for arlit
489. Processing data for wanaka
490. Processing data for awbari
Error: 'candawaga' was not found...skippping
491. Processing data for hoa binh
Error: 'phan rang' was not found...skippping
492. Processing data for srednekolymsk
493. Processing data for rio gallegos
494. Processing data for smithers
495. Processing data for ossett
496. Processing data for atar
497. Processing data for baraboo
498. Processing data for qaqortoq
499. Processing data for roebourne
500. Processing data for punta alta
501. Processing data for thunder bay
502. Processing data for koshurnikovo
Error: 'umzimvubu' was not found...skippping
503. Processing data for tuatapere
504. Processing data for gladstone
505. Processing data for iqaluit
Error: 'marzuq' was not found...skippping
506. Processing data for beloha
507. Processing data for pandan
508. Processing data for sitka
509. Processing data for kenai
510. Processing data for tessalit
511. Processing data for edd
512. Processing data for itaberaba
513. Processing data for hammerfest
514. Processing data for joigny
Error: 'studenicani' was not found...skippping
515. Processing data for bweyogerere
516. Processing data for cockburn town
517. Processing data for meulaboh
518. Processing data for saint-joseph
519. Processing data for tilichiki
520. Processing data for kasongo-lunda
521. Processing data for ngunguru
522. Processing data for podporozhye
523. Processing data for kingston
524. Processing data for pushkinskiye gory
525. Processing data for russell
526. Processing data for kalabo
527. Processing data for sao luis de montes belos
528. Processing data for yima
529. Processing data for sin-le-noble
530. Processing data for la cruz
Error: 'dhadar' was not found...skippping
531. Processing data for haradok
532. Processing data for mandera
533. Processing data for karpathos
534. Processing data for port-gentil
535. Processing data for flin flon
536. Processing data for bacolod
537. Processing data for sulangan
538. Processing data for bathsheba
539. Processing data for mokhsogollokh
540. Processing data for jatai
541. Processing data for minab
542. Processing data for seddon
543. Processing data for hokitika
544. Processing data for charters towers
545. Processing data for dixon
546. Processing data for banda aceh
547. Processing data for visani
548. Processing data for skelleftea
549. Processing data for uruzgan
Error: 'aflu' was not found...skippping
550. Processing data for tombouctou
551. Processing data for seydi
Error: 'tubruq' was not found...skippping
552. Processing data for ponta delgada
Error: 'santa eulalia del rio' was not found...skippping
553. Processing data for escanaba
554. Processing data for madingou
555. Processing data for cerkezkoy
556. Processing data for jiuquan
557. Processing data for whitehorse
558. Processing data for port said
559. Processing data for miles city
560. Processing data for bitung
561. Processing data for farah
562. Processing data for ostrov
563. Processing data for sidi ali
564. Processing data for fort nelson
565. Processing data for eureka
566. Processing data for matara
567. Processing data for innisfail
568. Processing data for jaypur
Error: 'gnjilane' was not found...skippping
569. Processing data for fare
Error: 'japura' was not found...skippping
570. Processing data for oktyabrskiy
571. Processing data for tupelo
572. Processing data for puerto escondido
573. Processing data for pangody
574. Processing data for westchester
575. Processing data for deputatskiy
576. Processing data for mae sot
577. Processing data for shelburne
578. Processing data for curup
579. Processing data for genhe
Error: 'skagastrond' was not found...skippping
580. Processing data for puri
Error: 'qunduz' was not found...skippping
581. Processing data for maputo
Error: 'tselinnoye' was not found...skippping
582. Processing data for nemuro
583. Processing data for galveston
584. Processing data for gao
585. Processing data for lenoir
586. Processing data for copiapo
587. Processing data for teseney
588. Processing data for henties bay
589. Processing data for puerto del rosario
590. Processing data for lucapa
591. Processing data for aksarka
592. Processing data for camacha
593. Processing data for mednogorsk
594. Processing data for arica
595. Processing data for mehamn
596. Processing data for ruwi
597. Processing data for gainesville
598. Processing data for saint-andre-les-vergers
599. Processing data for ayagoz
600. Processing data for bilma
601. Processing data for vestmanna
602. Processing data for nalut
603. Processing data for pedro juan caballero
604. Processing data for narsaq
605. Processing data for pankrushikha
Error: 'dagana' was not found...skippping
606. Processing data for omboue
607. Processing data for acajutla
608. Processing data for ribnitz-damgarten
609. Processing data for baykit
610. Processing data for kanniyakumari
611. Processing data for jalu
612. Processing data for potiskum
613. Processing data for polis
614. Processing data for guisa
615. Processing data for movila
Error: 'tabiauea' was not found...skippping
616. Processing data for plettenberg bay
617. Processing data for kisangani
618. Processing data for marawi
619. Processing data for ardmore
620. Processing data for moshupa
621. Processing data for chililabombwe
Error: 'saint-georges' was not found...skippping
622. Processing data for newport
623. Processing data for arenillas
624. Processing data for jacareacanga
625. Processing data for paamiut
626. Processing data for belmonte
627. Processing data for bubaque
628. Processing data for trairi
Error: 'nguiu' was not found...skippping
629. Processing data for novikovo
630. Processing data for faranah
631. Processing data for gardone val trompia
Error: 'kuche' was not found...skippping
632. Processing data for tinaquillo
633. Processing data for manavalakurichi
Error: 'pemangkat' was not found...skippping
634. Processing data for mujiayingzi
635. Processing data for itaituba
636. Processing data for xiashi
637. Processing data for coihaique
638. Processing data for kovylkino
Error: 'bolshaya chernigovka' was not found...skippping
639. Processing data for naryan-mar
640. Processing data for soc trang
641. Processing data for fomboni
642. Processing data for kovdor
643. Processing data for riihimaki
644. Processing data for canas
Error: 'bolungarvik' was not found...skippping
645. Processing data for manta
646. Processing data for cap-aux-meules
647. Processing data for hacari
648. Processing data for visby
649. Processing data for pacific grove
650. Processing data for khandyga
651. Processing data for hovd
652. Processing data for zhoucheng
653. Processing data for san antonio
654. Processing data for lavrentiya
Error: 'yirol' was not found...skippping
655. Processing data for bang khla
656. Processing data for constitucion
657. Processing data for chalchihuites
658. Processing data for leh
659. Processing data for bonavista
660. Processing data for jablah
Error: 'chagda' was not found...skippping
661. Processing data for cordoba
662. Processing data for nurota
663. Processing data for ekhabi
664. Processing data for mrakovo
665. Processing data for beyneu
666. Processing data for puerto leguizamo
667. Processing data for ossora
--------------------------
Data retrieval complete
--------------------------
```
## DataFrame of each city's weather data
```
# create dataframe using data from api
weather_df = pd.DataFrame({"City" : found_cities, "Country" : found_countries, "Date" : dates,
                   "Lat" : found_lats, "Lng" : found_lngs, "Max Temp": max_temp, "Humidity" : humidity, 
                   "Cloudiness" : cloudiness, "Wind Speed" : wind_speed})

weather_df.to_csv("../Output/city_weather.csv") # write data frame to csv file and save

weather_df.head()
```
## Temperature v Latitude
```
# create scatter plot of the relationship between temperature and latitude
plt.figure(figsize = (10,5))
plt.scatter(weather_df["Lat"], weather_df["Max Temp"], facecolor = "slateblue", edgecolor = "black", alpha = .9, s = 30);

plt.title("Temperature (F) v. City Latitude   " + readable_date );
plt.ylabel("Max Temperature (F)");
plt.xlabel("Latitude");


plt.ylim(weather_df["Max Temp"].min() - 2, weather_df["Max Temp"].max() + 2)
plt.xlim(weather_df["Lat"].min() - 2, weather_df["Lat"].max() + 2)
         
plt.savefig("../Images/temp_lat.png")
```

## Humidity v Latitude

```
# create scatter plot of the relationship between humidity and latitude
plt.figure(figsize = (10,5))
plt.scatter(weather_df["Lat"], weather_df["Humidity"], facecolor = "slateblue", edgecolor = "black", alpha = .9, s = 30);


plt.title("Humidity (%) v. City Latitude   " + readable_date);
plt.ylabel("Humidity (%)");
plt.xlabel("Latitude");

plt.ylim(weather_df["Humidity"].min() - 5, weather_df["Humidity"].max() + 5)
plt.xlim(weather_df["Lat"].min() - 2, weather_df["Lat"].max() + 2)

plt.savefig("../Images/humidity_lat.png")
```
## Cloudiness v Latitude

```
# create scatter plot of the relationship between cloudiness and latitude
plt.figure(figsize = (10,5))
plt.scatter(weather_df["Lat"], weather_df["Cloudiness"], facecolor = "slateblue", edgecolor = "black", alpha = .9, s = 30);


plt.title("Cloudiness (%) v. City Latitude   " + readable_date);
plt.ylabel("Cloudiness (%)");
plt.xlabel("Latitude");


plt.ylim(weather_df["Cloudiness"].min() - 5, weather_df["Cloudiness"].max() + 5)
plt.xlim(weather_df["Lat"].min() - 2, weather_df["Lat"].max() + 2)

plt.savefig("../Images/clouds_lat.png")
```

## Wind Speed v Latitude

```
# create scatter plot of the relationship between wind speed and latitude
plt.figure(figsize = (10,5))
plt.scatter(weather_df["Lat"], weather_df["Wind Speed"], facecolor = "slateblue", edgecolor = "black", alpha = .9, s = 30);


plt.title("Wind Speed (mph) v. City Latitude   " + readable_date);
plt.ylabel("Wind Speed (mph)");
plt.xlabel("Latitude");

plt.ylim(weather_df["Wind Speed"].min() - 1, weather_df["Wind Speed"].max() + 5)
plt.xlim(weather_df["Lat"].min() - 2, weather_df["Lat"].max() + 2)

plt.savefig("../Images/wind_lat.png")
```