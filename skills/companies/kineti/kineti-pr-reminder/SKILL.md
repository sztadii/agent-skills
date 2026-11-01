---
name: kineti-pr-reminder
description: "Trigger only when the user or another skill explicitly invokes this skill to send Kineti PR review reminders on Slack."
---

# Kineti — przypomnienia o review PR-ów

Uruchamiaj tylko po jawnym wywołaniu `$kineti-pr-reminder` (Codex),
`/kineti-pr-reminder` (Claude Code) lub poleceniu wywołania przez inny skill.
Sama wzmianka, przykład lub podobne zadanie nie uruchamia workflow.
Zaplanowane wywołanie musi jawnie wskazywać ten skill.

Przy każdym wywołaniu sprawdź aktualne PR-y użytkownika i wyślij zespołowi zwięzłe przypomnienie na Slacku. Wywołanie tego workflow autoryzuje wysłanie przypomnienia; nie pytaj ponownie o zgodę na ustaloną wysyłkę. Samo tworzenie lub edytowanie skilla nie jest jego uruchomieniem.

## Ustalenie kontekstu

- Ustal login użytkownika przez uwierzytelnione konto GitHub (np. `gh api user --jq .login`), zamiast zgadywać go na podstawie lokalnej konfiguracji git.
- Ustal organizację i repozytoria Kineti z instrukcji wywołania, konfiguracji projektu lub potwierdzonego kontekstu GitHub. Nie rozszerzaj wyszukiwania na niezwiązane projekty.
- Ustal właściwy workspace i kanał Slack z instrukcji lub jednoznacznego, wcześniej ustalonego kontekstu. Użyj dostępnych narzędzi do odczytu, jeśli pomagają ustalić cel. Jeśli zakres repozytoriów lub odbiorca pozostają niejednoznaczne, poproś tylko o brakującą informację; w automatycznym uruchomieniu zgłoś blokadę i niczego nie wysyłaj do przypadkowego kanału.

## Sprawdzenie PR-ów

1. Pobierz otwarte PR-y autorstwa uwierzytelnionego użytkownika w ustalonym zakresie. Uwzględnij paginację.
2. Sprawdź aktualny stan, draft, decyzję review, żądanych reviewerów oraz review dla bieżącej wersji zmian. Do przypomnienia wybieraj PR-y gotowe do review, które nadal go potrzebują. Pomiń drafty, zamknięte, zmergowane i PR-y z wymaganymi akceptacjami, chyba że użytkownik określi inny zakres.
3. Zbierz tytuł, numer, repozytorium i URL każdego wybranego PR-a. Nie sugeruj, że CI przeszło lub ktoś blokuje pracę, jeśli dane tego nie potwierdzają.

## Wysłanie przypomnienia

- Wyślij jedną zbiorczą, krótką wiadomość po polsku na ustalony kanał Slack: prośba o review oraz lista linków z tytułami i repozytoriami. Jeśli zespół ma ustalony inny język, dostosuj wiadomość.
- Nie dodawaj `@channel`, `@here` ani zgadywanych oznaczeń osób. Reviewerów oznaczaj tylko przy potwierdzonym mapowaniu kont i ustalonej praktyce zespołu lub instrukcji użytkownika.
- Jeśli brak PR-ów wymagających review, nie wysyłaj pustego przypomnienia; poinformuj o wyniku w odpowiedzi.
- Jeżeli dostępna historia kanału pokazuje już identyczne przypomnienie z tego samego uruchomienia, pomiń duplikat. Po niejednoznacznym wyniku wysyłki najpierw sprawdź historię; nie ponawiaj w ciemno.
- Użyj dostępnego narzędzia Slack do faktycznej wysyłki. Brak dostępu zgłoś jako blokadę, zamiast przedstawiać szkic jako wysłaną wiadomość.
- Na końcu podaj liczbę uwzględnionych PR-ów i link do wysłanej wiadomości, jeśli narzędzie go zwróciło. Wyraźnie zgłoś pominięcia lub niepełne dane, które ograniczyły wynik.

Skill wykonuje pojedyncze sprawdzenie i wysyłkę. Nie tworzy ani nie modyfikuje harmonogramów; wymaga to osobnej instrukcji użytkownika.
