# Dokument wymagań produktu (PRD) - VibeTravels

Wersja: 1.0  
Data: 07 października 2025  
Autor: Product Team  
Status: Draft

---

## 1. Przegląd produktu

### 1.1 Nazwa produktu
VibeTravels

### 1.2 Wizja produktu
VibeTravels to inteligentna aplikacja webowa wykorzystująca sztuczną inteligencję do tworzenia spersonalizowanych planów podróży. Produkt ma na celu eliminację trudności związanych z planowaniem angażujących i interesujących wycieczek poprzez automatyczne generowanie szczegółowych itinerariów dopasowanych do indywidualnych preferencji użytkowników.

### 1.3 Cel biznesowy
Stworzenie MVP aplikacji planowania podróży, która:
- Przyciągnie minimum 1000 użytkowników w pierwszym kwartale po uruchomieniu
- Osiągnie wskaźnik konwersji free-to-premium na poziomie 10% w ciągu 6 miesięcy od uruchomienia
- Zbuduje bazę lojalnych użytkowników generujących regularne przychody z subskrypcji

### 1.4 Grupa docelowa
Produkt skierowany jest do trzech głównych person:

1. Młodzi profesjonaliści (25-35 lat)
   - Planują weekendowe wyjazdy i krótkie urlopy
   - Mają ograniczony czas na szczegółowe planowanie
   - Cenią efektywność i personalizację

2. Rodziny z dziećmi
   - Planują wakacje rodzinne
   - Potrzebują kompleksowych planów uwzględniających potrzeby całej rodziny
   - Szukają sprawdzonych, bezpiecznych opcji

3. Solo travelers
   - Poszukują przygód i unikalnych doświadczeń
   - Chcą odkrywać nowe miejsca dopasowane do swoich zainteresowań
   - Cenią niezależność i elastyczność

### 1.5 Model biznesowy
Model freemium z subskrypcją Premium:

Tier darmowy:
- 3 plany podróży miesięcznie
- Podstawowy eksport do PDF
- Dostęp do ostatnich 3 planów
- Standard queue dla generowania AI

Tier Premium (29 PLN/miesiąc):
- Nielimitowane plany podróży
- Premium PDF z brandingiem, mapami i QR kodami
- Nieograniczona historia planów
- Priorytetowa kolejka generowania AI
- Dedykowane wsparcie

Płatności obsługiwane przez Stripe z automatyczną obsługą VAT.

### 1.6 Kluczowe wyróżniki (USP)
- Generowanie kompletnego, spersonalizowanego planu podróży w 5 minut
- AI uwzględnia unikalne preferencje użytkownika (zainteresowania, budżet, styl podróży)
- Gotowe, spójne itineraria vs wyszukiwanie i listy oferowane przez konkurencję
- Timeline dzień po dniu z konkretnymi miejscami, czasem i opisami
- Niski próg wejścia: pierwszy plan bez rejestracji

---

## 2. Problem użytkownika

### 2.1 Opis problemu
Planowanie angażujących i interesujących wycieczek jest czasochłonne i trudne. Użytkownicy napotykają następujące wyzwania:

1. Przeciążenie informacją
   - Zbyt wiele źródeł informacji (blogi, TripAdvisor, Google, Instagram)
   - Trudność w weryfikacji jakości i aktualności informacji
   - Brak spójnego źródła planowania

2. Brak personalizacji
   - Istniejące rozwiązania oferują generyczne listy atrakcji
   - Nie uwzględniają indywidualnych preferencji użytkownika
   - Nie dopasowują się do budżetu i stylu podróży

3. Czasochłonność
   - Manualne planowanie trasy zajmuje godziny
   - Trudność w optymalizacji czasu i logistyki
   - Konieczność ręcznego łączenia informacji z różnych źródeł

4. Brak struktury
   - Chaotyczne notatki i zakładki w przeglądarce
   - Trudność w udostępnieniu planu współpodróżnym
   - Brak gotowego itinerarium do wykorzystania w podróży

### 2.2 Obecne rozwiązania i ich ograniczenia

Konkurencyjne rozwiązania (Google Travel, TripAdvisor, Wanderlog):
- Oferują wyszukiwanie i listy miejsc, nie gotowe plany
- Brak głębokiej personalizacji na podstawie preferencji
- Wymagają znaczącego wkładu czasu użytkownika
- Nie uwzględniają kontekstu całej podróży (budżet, tempo, zainteresowania)

### 2.3 Proponowane rozwiązanie
VibeTravels wykorzystuje potencjał AI (GPT-4) do automatycznego generowania kompleksowych, spersonalizowanych planów podróży:

- Prosty formularz 3-krokowy zbiera kluczowe informacje
- AI analizuje preferencje i generuje szczegółowe itinerarium w minutach
- Plan zawiera timeline dzień po dniu z konkretnymi miejscami, czasem, opisami
- Użytkownik otrzymuje gotowy do użycia plan, nie listę do dalszego research

---

## 3. Wymagania funkcjonalne

### 3.1 Autentykacja i zarządzanie kontem

FR-001: Rejestracja użytkownika
- System umożliwia rejestrację poprzez email i hasło
- System umożliwia rejestrację poprzez Google OAuth
- System waliduje format email i siłę hasła (min. 8 znaków)
- System wysyła email potwierdzający rejestrację
- System przechowuje dane użytkownika w bazie Supabase

FR-002: Logowanie użytkownika
- System umożliwia logowanie przez email i hasło
- System umożliwia logowanie przez Google OAuth
- System umożliwia reset hasła przez magic link wysłany na email
- System zapamiętuje sesję użytkownika (opcjonalne "Remember me")

FR-003: Zarządzanie profilem
- System przechowuje podstawowe dane: email, imię (opcjonalne), kraj/miasto (opcjonalne)
- System umożliwia edycję danych profilu
- System umożliwia zmianę hasła
- System umożliwia usunięcie konta (GDPR compliance)

FR-004: Zarządzanie subskrypcją
- System wyświetla aktualny status subskrypcji (Free/Premium)
- System umożliwia upgrade do Premium przez Stripe checkout
- System umożliwia zarządzanie subskrypcją (zmiana karty, anulowanie)
- System automatycznie odnawia subskrypcję miesięcznie
- System obsługuje webhooks Stripe dla aktualizacji statusu

### 3.2 Generowanie planu podróży

FR-005: Formularz planowania - Krok 1 (Basic Information)
- System zbiera: destynację (text input, wymagane)
- System zbiera: datę rozpoczęcia podróży (date picker, wymagane)
- System zbiera: datę zakończenia podróży (date picker, wymagane)
- System waliduje, że data końcowa jest późniejsza niż początkowa
- System oblicza liczbę dni podróży automatycznie
- System zbiera: liczbę osób (number input, wymagane, min 1, max 20)

FR-006: Formularz planowania - Krok 2 (Travel Preferences)
- System umożliwia wybór zainteresowań (multiselect, min 3, max 10):
  - History, Food, Nature, Museums, Shopping, Nightlife, Architecture, Art, Music, Sports, Beaches, Photography
- System wyświetla licznik wybranych zainteresowań (X/10)
- System zbiera budżet dzienny (radio select, wymagane):
  - Budget (€30-80/day), Low (€80-150/day), Medium (€150-300/day), High (€300-500/day), Luxury (€500+/day)
- System zbiera styl podróży (radio select, wymagane):
  - Relaxed & Slow, Balanced, Active & Busy, Cultural Focus, Adventure Seeking
- System zbiera preferencje zakwaterowania (radio select, opcjonalne):
  - No preference, Hotels, Hostels, Apartments/Airbnb, Boutique accommodations, Luxury resorts
- System zbiera ograniczenia dietetyczne (multiselect, opcjonalne):
  - Vegetarian, Vegan, Gluten-free, Halal, Kosher, None

FR-007: Formularz planowania - Krok 3 (Finalization)
- System zbiera adres email (jeśli użytkownik niezalogowany)
- System wyświetla podsumowanie wszystkich wybranych parametrów
- System umożliwia powrót do poprzednich kroków i edycję
- System waliduje kompletność wszystkich wymaganych pól
- System wyświetla przycisk "Generate Plan"

FR-008: Generowanie planu przez AI
- System wysyła request do OpenRouter.ai (model: GPT-4)
- System przekazuje wszystkie parametry z formularza
- System przekazuje zapisane preferencje użytkownika (jeśli zalogowany)
- System wyświetla loading screen z animacją "AI analizuje Twoje preferencje..."
- System oczekuje na odpowiedź max 60 sekund
- System parsuje odpowiedź JSON od AI do struktury planu
- System zapisuje wygenerowany plan w bazie danych
- System przekierowuje do widoku planu lub wyświetla komunikat o wysłaniu emaila

FR-009: Struktura wygenerowanego planu
- Plan zawiera timeline dzień po dniu (Day 1, Day 2, ...)
- Każdy dzień podzielony na pory: Morning, Afternoon, Evening
- Każda pora zawiera: nazwę miejsca, krótki opis (2-3 zdania), szacowany czas wizyty, link do Google Maps
- Plan zawiera sekcję "Recommended Restaurants" dla każdego dnia
- Plan zawiera sekcję "Transportation Tips" dla każdego dnia
- Plan zawiera wprowadzenie ogólne o destynacji
- Plan zawiera podsumowanie i wskazówki końcowe

FR-010: Obsługa błędów generowania
- Jeśli AI nie odpowie w 60 sekund: komunikat "Generowanie trwa dłużej. Wyślemy plan na email w ciągu 2 minut"
- Jeśli AI zwróci błąd: retry automatyczny (max 2 próby)
- Jeśli 2 próby zawiodą: komunikat o błędzie + sugestia kontaktu z supportem
- System loguje wszystkie failed generations dla późniejszej analizy

### 3.3 Zarządzanie planami

FR-011: Dashboard użytkownika
- System wyświetla wszystkie plany użytkownika jako kafelki/karty
- Każdy kafelek zawiera: miniaturę destynacji, nazwę destynacji, daty podróży, status
- System sortuje plany chronologicznie (najnowsze na górze)
- System umożliwia filtrowanie planów (wszystkie/nadchodzące/przeszłe)
- System umożliwia wyszukiwanie planów po nazwie destynacji
- Free users widzą ostatnie 3 plany, starsze z etykietą "Archived - Upgrade to view"
- Premium users widzą wszystkie plany bez ograniczeń

FR-012: Widok szczegółów planu
- System wyświetla pełny plan w przejrzystym formacie
- System wyświetla timeline dzień po dniu z możliwością rozwijania/zwijania dni
- System wyświetla linki Google Maps jako klikalne przyciski
- System wyświetla sekcję rating i feedback na dole planu
- System umożliwia export do PDF (przycisk "Download PDF")
- System umożliwia regenerację planu (przycisk "Regenerate Plan")

FR-013: Rating i feedback
- System wyświetla gwiazdki 1-5 do oceny planu
- System zapisuje rating po kliknięciu gwiazdki
- System wyświetla opcjonalne pole tekstowe "Additional feedback"
- System zapisuje feedback tekstowy
- System wyświetla komunikat "Thank you for your feedback!"
- System używa danych rating/feedback do analytics

FR-014: Regeneracja planu
- System wyświetla przycisk "Regenerate Plan" pod planem
- System wyświetla opcjonalne pole "What didn't work?" przed regeneracją
- System generuje nowy plan z tymi samymi parametrami + feedback
- System zachowuje stary plan w historii (nie nadpisuje)
- System przekierowuje do nowo wygenerowanego planu
- Brak limitu regeneracji w MVP

FR-015: Soft delete planów
- System nigdy nie usuwa fizycznie planów z bazy danych
- System oznacza plany jako "deleted" (deleted_at timestamp)
- Użytkownik może "usunąć" plan z dashboardu (znika z widoku)
- Admin może przywrócić usunięte plany na prośbę użytkownika
- GDPR: użytkownik może zażądać full deletion wszystkich danych

### 3.4 Eksport i komunikacja

FR-016: Email z planem
- System automatycznie wysyła email po wygenerowaniu planu
- Email zawiera: powitanie z imieniem (jeśli dostępne), krótkie intro, link do pełnego planu online, przycisk "Download PDF"
- Email zawiera disclaimer: "Plan generowany przez AI może zawierać nieścisłości. Zweryfikuj szczegóły przed podróżą"
- System używa Resend lub SendGrid do wysyłki
- System loguje status wysyłki (delivered/failed)

FR-017: Generowanie PDF
- System generuje PDF na żądanie (kliknięcie przycisku)
- Free users: podstawowy PDF (tylko tekst + logo VibeTravels)
- Premium users: premium PDF (branding, embeded mapy, QR kody do miejsc)
- PDF zawiera pełną treść planu w czytelnym formacie
- System używa jsPDF lub Puppeteer do generowania
- System zwraca plik PDF do pobrania (download)

FR-018: Transactional emails
- System wysyła email potwierdzający rejestrację
- System wysyła email z planem gotowym
- System wysyła email z linkiem reset hasła
- System wysyła email potwierdzający subskrypcję Premium
- System wysyła email przypominający o odnowieniu subskrypcji (7 dni przed)

### 3.5 System Premium i płatności

FR-019: Limity Free tier
- Free users mogą wygenerować max 3 plany miesięcznie
- System resetuje licznik pierwszego dnia miesiąca
- System wyświetla licznik "X/3 plans this month"
- Po wyczerpaniu limitu: komunikat "Upgrade to Premium for unlimited plans"
- Free users mają dostęp tylko do ostatnich 3 planów w historii

FR-020: Upgrade do Premium
- System wyświetla przycisk "Upgrade to Premium" w dashboardzie
- System wyświetla przycisk po wyczerpaniu limitu planów
- System przekierowuje do Stripe Checkout (29 PLN/miesiąc)
- System odbiera webhook od Stripe po udanej płatności
- System aktualizuje status użytkownika do Premium
- System wysyła email potwierdzający subskrypcję
- System odblokowuje premium features natychmiast

FR-021: Zarządzanie subskrypcją Premium
- System wyświetla status subskrypcji w profilu: "Active", data następnej płatności
- System umożliwia zmianę karty płatniczej (redirect do Stripe portal)
- System umożliwia anulowanie subskrypcji
- Po anulowaniu: premium features aktywne do końca opłaconego okresu
- System wysyła email przed odnowieniem (7 dni wcześniej)

### 3.6 Preferencje użytkownika

FR-022: Zapisywanie preferencji
- System zapisuje wszystkie preferencje z formularza planowania do profilu użytkownika
- System aktualizuje preferencje przy każdym nowym planie
- System używa najnowszych preferencji jako domyślne w nowym formularzu
- System umożliwia edycję zapisanych preferencji w profilu
- System umożliwia reset preferencji do wartości domyślnych

FR-023: Auto-fill formularza
- Dla zalogowanych użytkowników: formularz auto-filluje się zapisanymi preferencjami
- Użytkownik może modyfikować wartości przed generowaniem
- System zapamiętuje zmiany jako nowe preferencje

### 3.7 Funkcje bez rejestracji

FR-024: Pierwszy plan bez konta
- System umożliwia wypełnienie formularza bez logowania
- System wymaga tylko email w kroku 3
- System generuje plan i wysyła na podany email
- System nie zapisuje planu w historii (brak konta)
- System wyświetla soft-prompt po generowaniu: "Załóż konto, aby zapisać plan i generować więcej!"
- System umożliwia łatwy signup z tego samego emaila (przycisk "Create Account")

### 3.8 Integracje zewnętrzne

FR-025: Google Maps integration
- System generuje linki Google Maps dla każdego miejsca w planie
- Link format: https://www.google.com/maps/search/?api=1&query=[place_name]+[destination]
- System wyświetla linki jako przyciski "View on Map"
- System NIE embedduje map w MVP (tylko linki)

FR-026: Linki afiliacyjne (opcjonalne w MVP)
- System może dodawać proste linki afiliacyjne do booking.com, skyscanner
- Linki wyświetlane jako "Book accommodation" / "Find flights"
- Brak zaawansowanej integracji w MVP

---

## 4. Granice produktu

### 4.1 Funkcjonalności NIE wchodzące w zakres MVP

NF-001: Współdzielenie planów między kontami
- Brak możliwości udostępniania planów innym użytkownikom
- Brak collaborative editing
- Brak public sharing links
- Pozostawione na post-MVP

NF-002: Bogata obsługa multimediów
- Brak uploadu zdjęć przez użytkownika
- Brak galerii zdjęć miejsc w planie
- Brak video content
- Brak integracji z Instagram/social media
- Pozostawione na post-MVP

NF-003: Zaawansowane planowanie czasu i logistyki
- Brak automatycznego planowania transportu między miejscami
- Brak integracji z rozkładami jazdy, lotami
- Brak optymalizacji tras pod kątem czasu/kosztów
- Brak rezerwacji miejsc/biletów
- Pozostawione na post-MVP

NF-004: Edycja planów
- Brak drag-and-drop edytora
- Brak możliwości ręcznego dodawania/usuwania miejsc
- Brak reorder activities
- Tylko przeglądanie + rating + regeneracja w MVP
- Pełny edytor na post-MVP

NF-005: Social features
- Brak profili publicznych
- Brak follow/followers
- Brak komentarzy i review społeczności
- Brak leaderboards/gamification
- Pozostawione na post-MVP

NF-006: Zaawansowane analytics
- Brak dashboardu analytics dla użytkownika
- Brak porównywania kosztów różnych opcji
- Brak trackingu wydatków w podróży
- Pozostawione na post-MVP

NF-007: Offline mode
- Brak dostępu do planów offline
- Brak mobile app z offline sync
- Tylko web app z wymogiem połączenia internetowego

NF-008: Wielojęzyczność
- Start tylko w języku polskim
- Angielski i inne języki na post-MVP
- Struktura i18n-ready, ale tłumaczenia później

### 4.2 Ograniczenia techniczne MVP

TL-001: Limity generowania AI
- Max 10 dni podróży w jednym planie (ograniczenie długości AI response)
- Max 20 osób w group size (realność scenariuszy)
- Timeout 60 sekund na generowanie (potem async email)

TL-002: Limity storage i historii
- Free users: 3 plany aktywne, reszta archiwum
- Soft delete: plany nie usuwane fizycznie (storage cost)
- Backup bazy danych: raz dziennie (Supabase automatic)

TL-003: Limity integracji
- Tylko Google Maps links (bez embeded maps)
- Proste linki afiliacyjne (bez deep integration)
- Brak real-time data (pogoda, ceny, dostępność)

TL-004: Performance
- Loading time: max 3 sekundy dla strony
- AI generation: target 30-45 sek, max 60 sek
- PDF generation: max 10 sek
- Brak CDN dla assets w MVP (added post-launch)

### 4.3 Ograniczenia biznesowe

BL-001: Wsparcie klienta
- Brak live chat w MVP
- Support przez email tylko (czas odpowiedzi: 24-48h)
- FAQ/Help center: podstawowy w MVP

BL-002: Marketing i growth
- Brak płatnych reklam w pierwszym miesiącu
- Organiczny growth: content marketing, Product Hunt, social media
- Brak programu referral w MVP

BL-003: Geograficzne
- Focus na rynek polski w MVP
- AI ma wiedzę globalną, ale marketing i język PL
- Ekspansja międzynarodowa post-MVP

---

## 5. Historyjki użytkowników

### 5.1 Autentykacja i zarządzanie kontem

US-001: Rejestracja przez email
Jako nowy użytkownik
Chcę założyć konto używając email i hasła
Aby móc zapisywać i zarządzać moimi planami podróży

Kryteria akceptacji:
- Formularz rejestracji zawiera pola: email, hasło, potwierdź hasło
- System waliduje format email (regex pattern)
- System waliduje siłę hasła (min 8 znaków, 1 wielka litera, 1 cyfra)
- System sprawdza, czy hasła są identyczne
- System sprawdza, czy email nie istnieje już w bazie
- Po udanej rejestracji system wysyła email potwierdzający
- Po udanej rejestracji system automatycznie loguje użytkownika
- System wyświetla komunikat sukcesu "Account created successfully!"
- System przekierowuje do dashboardu

US-002: Rejestracja przez Google OAuth
Jako nowy użytkownik
Chcę założyć konto używając mojego konta Google
Aby szybko rozpocząć bez tworzenia nowego hasła

Kryteria akceptacji:
- Formularz zawiera przycisk "Sign up with Google"
- Kliknięcie otwiera popup Google OAuth
- System pobiera email i imię z konta Google
- System tworzy konto użytkownika w Supabase
- System automatycznie loguje użytkownika
- System nie wymaga potwierdzenia email (zweryfikowany przez Google)
- System przekierowuje do dashboardu
- System wyświetla komunikat "Welcome [Name]!"

US-003: Logowanie przez email
Jako zarejestrowany użytkownik
Chcę zalogować się używając email i hasła
Aby uzyskać dostęp do moich planów

Kryteria akceptacji:
- Formularz logowania zawiera pola: email, hasło
- Formularz zawiera checkbox "Remember me"
- System waliduje credentials w bazie Supabase
- Jeśli credentials poprawne: system loguje użytkownika i przekierowuje do dashboardu
- Jeśli credentials błędne: system wyświetla "Invalid email or password"
- Jeśli "Remember me" zaznaczone: sesja trwa 30 dni
- Jeśli nie zaznaczone: sesja trwa do zamknięcia przeglądarki
- Formularz zawiera link "Forgot password?"

US-004: Logowanie przez Google OAuth
Jako zarejestrowany użytkownik z kontem Google
Chcę zalogować się używając Google
Aby szybko uzyskać dostęp bez wpisywania hasła

Kryteria akceptacji:
- Formularz zawiera przycisk "Sign in with Google"
- Kliknięcie otwiera popup Google OAuth
- System weryfikuje, że email istnieje w bazie
- Jeśli istnieje: system loguje użytkownika i przekierowuje do dashboardu
- Jeśli nie istnieje: system tworzy nowe konto (rejestracja)
- System wyświetla komunikat "Welcome back [Name]!"

US-005: Reset hasła
Jako użytkownik, który zapomniał hasła
Chcę zresetować hasło używając magic link
Aby odzyskać dostęp do konta

Kryteria akceptacji:
- Link "Forgot password?" prowadzi do formularza reset
- Formularz zawiera pole email
- System wysyła magic link na podany email (jeśli istnieje w bazie)
- System wyświetla "If email exists, reset link has been sent"
- Link w emailu jest ważny przez 1 godzinę
- Kliknięcie linku otwiera formularz nowego hasła
- Formularz zawiera pola: new password, confirm password
- System waliduje siłę nowego hasła
- Po udanej zmianie system wyświetla "Password changed successfully"
- System automatycznie loguje użytkownika

US-006: Edycja profilu
Jako zalogowany użytkownik
Chcę edytować dane mojego profilu
Aby zaktualizować imię lub lokalizację

Kryteria akceptacji:
- Strona profilu wyświetla aktualne dane: email (read-only), imię, kraj/miasto
- Formularz umożliwia edycję: imię, kraj/miasto
- Przycisk "Save Changes" zapisuje zmiany w bazie
- System waliduje, że pola nie są puste (jeśli wypełnione)
- System wyświetla komunikat "Profile updated successfully"
- Zmiany są widoczne natychmiast w interfejsie

US-007: Zmiana hasła
Jako zalogowany użytkownik z kontem email/hasło
Chcę zmienić hasło
Aby zwiększyć bezpieczeństwo konta

Kryteria akceptacji:
- Strona profilu zawiera sekcję "Change Password"
- Formularz zawiera pola: current password, new password, confirm new password
- System weryfikuje current password
- System waliduje siłę nowego hasła (min 8 znaków, 1 wielka, 1 cyfra)
- System sprawdza, czy new passwords są identyczne
- Po udanej zmianie system wyświetla "Password changed successfully"
- System wysyła email potwierdzający zmianę hasła

US-008: Usunięcie konta
Jako zalogowany użytkownik
Chcę usunąć moje konto i wszystkie dane
Aby przestać używać serwisu (GDPR)

Kryteria akceptacji:
- Strona profilu zawiera przycisk "Delete Account"
- Kliknięcie otwiera modal z ostrzeżeniem "This action is permanent"
- Modal wymaga wpisania "DELETE" do potwierdzenia
- Modal wymaga podania hasła (dla kont email/hasło)
- System usuwa użytkownika i wszystkie powiązane dane z bazy
- System wylogowuje użytkownika
- System przekierowuje do landing page
- System wyświetla "Account deleted successfully"

### 5.2 Generowanie planu podróży

US-009: Wypełnienie kroku 1 - Basic Information
Jako użytkownik planujący podróż
Chcę podać podstawowe informacje o wycieczce
Aby rozpocząć proces generowania planu

Kryteria akceptacji:
- Formularz wyświetla 4 pola: Destination (text), Start Date (date picker), End Date (date picker), Group Size (number)
- Wszystkie pola są wymagane (oznaczone gwiazdką)
- Date picker nie pozwala wybrać daty przeszłej dla Start Date
- End Date musi być późniejsza niż Start Date (walidacja)
- Group Size: min 1, max 20
- System automatycznie oblicza liczbę dni (End - Start + 1)
- Przycisk "Next Step" jest aktywny tylko gdy wszystkie pola wypełnione poprawnie
- Kliknięcie "Next Step" zapisuje dane i przechodzi do kroku 2

US-010: Wypełnienie kroku 2 - Travel Preferences
Jako użytkownik planujący podróż
Chcę określić moje preferencje turystyczne
Aby otrzymać spersonalizowany plan

Kryteria akceptacji:
- Formularz wyświetla sekcję "Your Interests" z 12 opcjami do wyboru
- System wymaga wyboru min 3, max 10 interests
- System wyświetla licznik "Selected: X/10"
- System blokuje wybór po osiągnięciu 10
- Formularz wyświetla Daily Budget (5 opcji radio)
- Formularz wyświetla Travel Style (5 opcji radio)
- Formularz wyświetla Accommodation Preference (6 opcji radio, jedna to "No preference")
- Formularz wyświetla Dietary Restrictions (multiselect, opcjonalne)
- Przycisk "Previous" wraca do kroku 1 bez utraty danych
- Przycisk "Next Step" aktywny gdy: 3-10 interests, budget wybrany, style wybrany
- Kliknięcie "Next Step" zapisuje