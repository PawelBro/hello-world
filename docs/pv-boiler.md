# Rozdzielnica DC do instalacji PV – dobór, FAQ i kalkulator

## Odpowiedź na pytanie: czy opisana rozdzielnica jest odpowiednia do zestawu?

### Zestaw
- **Przetwornica:** ECO Solar Boost EVO MPPT-4000 4 kW PRO  
- **Panele:** 6 × panel monokrystaliczny 450 W  
- **Kable:** 2 × 25 m solarny + złącza MC4  

### Opisana rozdzielnica
- 1 string, 1000 V DC, IP65  
- Wkładki topikowe do wyboru: 6 A / 10 A / 15 A  
- SPD (ochrona przepięciowa / odgromowa)  
- Niezależny bezpiecznik (uchwyt TXPV-32, 1000 V DC)  
- Maks. prąd wyjściowy: 32 A (zależnie od wkładki)  

### Krótka odpowiedź

| Konfiguracja | Rozdzielnica pasuje? | Dobór wkładki | Uwaga |
|---|---|---|---|
| **6S** (wszystkie 6 paneli szeregowo) | ✅ TAK (1 string) | **15 A** | Sprawdź maks. napięcie EVO (patrz niżej) |
| **3S2P** (dwa stringi po 3 panele, równolegle) | ❌ NIE – potrzebna rozdzielnica 2-stringowa | 2 × 15 A | Wymagana osobna skrzynka combiner |

> **Ważne:** Niezależnie od konfiguracji, do rozdzielnicy wymagany jest **osobny wyłącznik główny DC** (rozłącznik izolacyjny DC) zamontowany jako pierwszy na kablach od paneli. Sama rozdzielnica (bezpiecznik w środku) nie zastępuje wyłącznika głównego – potwierdza to też producent w instrukcji (plik PDF w tym repozytorium).

---

## FAQ – dobór rozdzielnicy DC do instalacji PV

### 1. Kiedy bezpieczniki stringu są potrzebne?

**Przy 1 stringu podłączonym bezpośrednio do przetwornicy:**  
Bezpiecznik stringu jest opcjonalny (przydatny jako dodatkowa ochrona), ale **nie jest wymagany normami** przy pojedynczym stringu bez równoległego łączenia – prąd zwarciowy jest ograniczony przez sam panel.

**Przy 2 lub więcej stringach połączonych równolegle:**  
Bezpieczniki stringów są **obowiązkowe**. Gdy jeden string ulegnie zwarciu, pozostałe stringi mogą przez niego „wstecznego" popłynąć prąd wielokrotnie przekraczający Isc pojedynczego stringu. Każdy string musi mieć własny bezpiecznik po stronie „+" (a często też „−").

**Zasada ogólna:**  
> Bezpieczniki stringów = wymagane zawsze, gdy `liczba stringów ≥ 2`.

---

### 2. Jak dobrać prąd bezpiecznika stringu?

Bezpiecznik stringu chroni **kabel i złącza MC4** tego stringu przed prądem wstecznym z pozostałych stringów.

**Wzór:**

```
I_bezpiecznik ≥ 1,25 × Isc_panelu
I_bezpiecznik ≤ max series fuse rating (z karty katalogowej panelu)
```

**Dla panelu 450 W (Isc ≈ 10,8 A, max series fuse rating zwykle 15–20 A):**

```
I_bezpiecznik ≥ 1,25 × 10,8 A = 13,5 A
→ wybieramy 15 A  ✓  (spełnia ≥ 13,5 A i ≤ 15–20 A)
```

> Wkładek 6 A i 10 A **nie używać** dla paneli 450 W – są za małe (poniżej 1,25 × Isc).

---

### 3. Konieczność rozłącznika DC (wyłącznika głównego)

Rozłącznik DC (izolacyjny, przeznaczony do DC/PV) musi być zamontowany **przed rozdzielnicą / przetwornicą**, jako pierwszy element na kablu od paneli.

- Umożliwia **bezpieczne odłączenie paneli** podczas serwisu, awarii lub pożaru.
- Panele PV w świetle dziennym zawsze generują napięcie DC – nawet po wyłączeniu przetwornicy.
- **Zwykły wyłącznik AC nie wystarczy** – łuk DC jest trudniejszy do ugaszenia.

**Dobór rozłącznika DC:**
- Napięcie: ≥ Voc_max_stringu × 1,25 (w praktyce 500 V lub 1000 V DC)
- Prąd: ≥ Isc_max_stringu (z zapasem, np. 25 A lub 32 A)

---

### 4. SPD DC – ochrona przepięciowa (typ 2)

SPD (Surge Protective Device) chroni przetwornicę i panele przed przepięciami atmosferycznymi.

- **Typ 2** (klasa II) – wymagany minimalnie w każdej instalacji PV.
- **Typ 1+2** (klasa I+II) – gdy instalacja jest na dachu lub w otwartym terenie szczególnie narażonym na wyładowania.
- Opisana rozdzielnica posiada SPD (ochronę odgromową) – ✅.
- Napięcie znamionowe SPD musi być ≥ napięcie obwodu DC (np. SPD 1000 V DC dla stringu 6S).

---

### 5. Przekroje przewodów solarnych

| Parametr | Wartość |
|---|---|
| Standardowy kabel solarny | 4 mm² (dołączony w zestawie) |
| Dla prądu do ~15 A (1 string) | 4 mm² – wystarczy |
| Dla prądu do ~25 A (2 stringi równolegle) | 6 mm² (zalecane) |
| Długość kabla (2 × 25 m) | Sprawdź spadek napięcia (patrz niżej) |

**Sprawdzenie spadku napięcia dla kabla 4 mm² / 2 × 25 m / Imax = 10,8 A:**

```
ΔU = (2 × L × I × ρ) / A
ΔU = (2 × 25 m × 10,8 A × 0,0175 Ω·mm²/m) / 4 mm²
ΔU ≈ 2,36 V
```

Przy napięciu stringu ~135–270 V to mniej niż 2 % → akceptowalne ✅.

---

### 6. Uziemienie i odległości

- Ramy paneli i konstrukcja nośna muszą być **uziemione** (PE).
- Kabel ochronny PE dla konstrukcji: min. 4 mm² Cu lub 6 mm² Al.
- Rozdzielnica IP65 powinna być zamontowana w miejscu osłoniętym od bezpośrednich opadów (ściana, wiata) lub być skierowana złączkami w dół.
- Minimalna odległość złącz MC4 od podłoża – zgodnie z wytycznymi producenta przetwornicy.
- Przy montażu kabli solarnych: unikać układania razem z kablami AC (min. 10 cm odległości lub osłona).

---

## Konfiguracje: 6S vs 3S2P – porównanie

### Parametry panelu referencyjnego (450 W mono)

| Parametr | Symbol | Wartość |
|---|---|---|
| Moc maks. | Pmax | 450 W |
| Napięcie maks. mocy | Vmp | ~45 V |
| Prąd maks. mocy | Imp | ~10,0 A |
| Napięcie otwartego obwodu | Voc (STC) | ~53 V |
| Prąd zwarciowy | Isc | ~10,8 A |
| Max series fuse rating | – | 15 A (typowo) |

---

### Konfiguracja A: 6S (6 paneli szeregowo, 1 string)

```
[Panel 1]---[Panel 2]---[Panel 3]---[Panel 4]---[Panel 5]---[Panel 6]
         → 1 string → Rozdzielnica 1-string → EVO MPPT-4000
```

| Parametr | Wartość | Uwaga |
|---|---|---|
| Vmp (STC) | 6 × 45 V = **270 V** | ✅ |
| Voc (STC) | 6 × 53 V = **318 V** | ✅ |
| Voc przy −10 °C* | ~**342 V** | ⚠️ Sprawdź maks. napięcie EVO! |
| Imp | **10,0 A** | ✅ |
| Isc | **10,8 A** | ✅ |
| Prąd bezpiecznika | **15 A** | ✅ (≥ 1,25 × 10,8 A = 13,5 A) |
| Przekrój kabla | **4 mm²** | ✅ |

> *Korekta temperaturowa Voc: współczynnik ~−0,29 %/°C. W −10 °C (ΔT = −25 °C od STC):  
> Voc_cold ≈ 318 V × (1 + 0,0029 × 25) ≈ **342 V**

**⚠️ Ryzyko napięciowe przy 6S:**  
ECO Solar Boost EVO MPPT-4000 – sprawdź w instrukcji parametr **„Max PV input voltage"**. Jeśli wynosi 350 V: przy 6S w mrozie (<−5 °C) napięcie Voc może zbliżyć się do granicy. Typowe modele EVO MPPT-4000 mają maks. napięcie PV 350 V – **konfiguracja 6S jest technicznie dopuszczalna, ale z minimalnym zapasem**. W wątpliwych przypadkach zaleca się 5S + 1 panel osobno (lub 3S2P).

**Czy opisana rozdzielnica pasuje do 6S?**  
✅ TAK – jest to konfiguracja 1-stringowa, napięcie 1000 V DC z dużym zapasem, bezpiecznik 15 A odpowiedni.

---

### Konfiguracja B: 3S2P (dwa stringi po 3 panele, połączone równolegle)

```
String 1: [Panel 1]---[Panel 2]---[Panel 3]
                                           \
                                            [Skrzynka 2-string] → EVO MPPT-4000
                                           /
String 2: [Panel 4]---[Panel 5]---[Panel 6]
```

| Parametr | Wartość | Uwaga |
|---|---|---|
| Vmp (STC) | 3 × 45 V = **135 V** | ✅ |
| Voc (STC) | 3 × 53 V = **159 V** | ✅ |
| Voc przy −10 °C | ~**171 V** | ✅ Duży zapas |
| Imp łącznie | 2 × 10,0 A = **20,0 A** | ⚠️ Sprawdź maks. prąd wejściowy EVO! |
| Isc łącznie | 2 × 10,8 A = **21,6 A** | ⚠️ |
| Bezpiecznik każdego stringu | **15 A** | ✅ Obowiązkowo (2+ stringi) |
| Kabel zbiorczy do EVO | **6 mm²** (zalecane) | Prąd ~20 A |

**⚠️ Ryzyko prądowe przy 3S2P:**  
Suma prądów wynosi ~21,6 A. ECO Solar Boost EVO MPPT-4000 – sprawdź parametr **„Max PV input current / Max Isc"**. Jeśli wynosi ≤ 16 A: konfiguracja 3S2P **jest niedopuszczalna** dla tego urządzenia.

**Czy opisana rozdzielnica pasuje do 3S2P?**  
❌ NIE – opisana rozdzielnica jest tylko na 1 string. Dla 3S2P wymagana jest **skrzynka combiner 2-stringowa** z dwoma bezpiecznikami (2 × 15 A) i wspólnym rozłącznikiem DC.

---

### Porównanie konfiguracji – podsumowanie

| Cecha | 6S | 3S2P |
|---|---|---|
| Napięcie (Voc STC) | 318 V (⚠️ blisko limitu 350 V) | 159 V (✅ bezpieczne) |
| Napięcie w mrozie | ~342 V (⚠️) | ~171 V (✅) |
| Prąd (Isc) | 10,8 A (✅) | 21,6 A (⚠️ sprawdź EVO) |
| Opisana rozdzielnica | ✅ Pasuje | ❌ Nie pasuje |
| Bezpiecznik | 15 A | 2 × 15 A |
| Kabel | 4 mm² | 6 mm² (zbiorczy) |
| Zastosowanie | Panele na 1 połaci / 1 kierunek | Panele na 2 kierunki (E+S) |

---

## Kalkulator – przykłady dla paneli 450–460 W

Parametry typowego panelu mono 450–460 W:

| | 450 W | 455 W | 460 W |
|---|---|---|---|
| Vmp | ~45,0 V | ~45,5 V | ~46,0 V |
| Imp | ~10,0 A | ~10,1 A | ~10,1 A |
| Voc (STC) | ~53,0 V | ~53,4 V | ~53,6 V |
| Isc | ~10,8 A | ~10,8 A | ~10,9 A |
| Max series fuse | 15 A | 15 A | 15 A |

### Kalkulator napięcia stringu

```
Voc_string(STC) = N × Voc_panelu
Voc_string(T°C) = Voc_string(STC) × [1 + α_Voc × (T - 25)]
```

gdzie `α_Voc ≈ −0,0029 /°C` (typowo dla paneli mono).

| Liczba paneli N | Voc STC | Voc −10°C | Voc −20°C |
|---|---|---|---|
| 3 | 159 V | 171 V | 175 V |
| 4 | 212 V | 228 V | 233 V |
| 5 | 265 V | 285 V | 292 V |
| **6** | **318 V** | **342 V** | **350 V** |

> ⚠️ Przy 6 panelach szeregowo i temperaturze −20 °C Voc osiąga 350 V – dokładnie na granicy dla EVO MPPT-4000 (jeśli jego maks. napięcie PV = 350 V). **Zaleca się sprawdzić instrukcję urządzenia.**

### Kalkulator doboru bezpiecznika stringu

```
I_bezpiecznik ≥ 1,25 × Isc
I_bezpiecznik ≤ max series fuse rating
```

| Isc panelu | Min. bezpiecznik (1,25 × Isc) | Zalecany z dostępnych |
|---|---|---|
| 10,8 A | 13,5 A | **15 A** ✅ |
| 10,9 A | 13,6 A | **15 A** ✅ |
| 11,0 A | 13,8 A | **15 A** ✅ |

> Wkładki **6 A i 10 A** z opisanej rozdzielnicy nie są odpowiednie dla paneli 450–460 W. Wybierać wyłącznie **15 A**.

---

## Wnioski końcowe

1. **Opisana rozdzielnica (1 string, 1000 V DC, IP65, wkładka 15 A, SPD)** jest odpowiednia dla zestawu ECO Solar Boost EVO MPPT-4000 + 6 × 450 W **wyłącznie w konfiguracji 6S** (wszystkie panele szeregowo).

2. **Obowiązkowo** dołożyć **rozłącznik główny DC** (np. 32 A, 1000 V DC) zamontowany przed rozdzielnicą na kablach od paneli.

3. **Wybierać wkładkę 15 A** – nie stosować 6 A ani 10 A dla tych paneli.

4. Jeśli panele mają być na **dwóch kierunkach (wschód + południe)** i chcesz uzyskać więcej energii przez cały dzień, rozważ konfigurację **3S2P** – ale wymaga ona:
   - sprawdzenia maks. prądu wejściowego EVO MPPT-4000 (musi być ≥ 22 A),
   - zakupu **skrzynki combiner 2-stringowej** (nie opisanej rozdzielnicy 1-string),
   - kabla zbiorczego **6 mm²**.

5. **SPD DC typ 2** w rozdzielnicy jest wymagany – opisana rozdzielnica go posiada ✅.

6. Standardowy kabel solarny **4 mm²** z zestawu jest wystarczający dla konfiguracji 1-stringowej (prąd ≤ 10,8 A, długość 2 × 25 m).
