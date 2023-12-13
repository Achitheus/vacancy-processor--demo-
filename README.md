# Vacancy processor (demo)
Automated processing vacancies programm

- [Execute](#execute)
- [Notes](#notes)
- [Overview](#overview)
- [Issues](#issues)


## Execute
1. Install last JDK from [oracle official website](https://www.oracle.com/java/technologies/downloads/)
(java 17 or higher is required).
2. Create empty folder and put in it the program (file with `.jar` extention).
3. Create a folder `userResources` next to the `.jar` file
4. In the `userResources` folder create two files:
   - `coverLetter.txt`
   - `userProperties.properties`  
   Properties file content example:
   ```properties
   # Available modes:
   #   - first_run (or "FR"/"fr"),
   #   - analyze_vacancy_descriptions (or "AVD"/"avd"),
   #   - vacancies_applying (or "VA"/"va"),
   #   - find_test_tasks (or "FTT"/"ftt")
   process.mode           = avd
   vacancy.count          = 1
   query                  = QA automation
   # Exact resume title isn't necessary for this value. Target resume title should contain it as substring.
   resume.title           = QA auto
   cover.letter           = ./userResources/coverLetter.txt
   # Switch this value to `true` if you don't want to see blinking browser. It is `background run mode` 
   headless               = false
   ```
5. Make sure you **saved** changes in the files.
6. Double click to `.jar` file.
## Notes
#### First run
In `FIRST_RUN` mode program just opens browser window.
To enable it set property `process.mode = fr`.  
Use this mode to log in, accept cookies, set location, etc.
Next run will take into account these settings.
This mode is also set `headless` property to `false` forcibly.
It's also recommended to change default chrome profile color to noticeable one because...
#### Another way to run program
You also can run program via command line:
1. Open explorer and go to `.jar` containing folder.
2. Click to folder path field then print `cmd` and press `enter`.
3. Run command `java -jar program-name.jar`

If you met command line cyrillic encoding issue do following steps:
1. Start -> Run -> regedit
2. Go to [HKEY_LOCAL_MACHINE \ Software \ Microsoft \ Command Processor\Autorun]
3. Change the value to `@chcp 65001>nul`
If `Autorun` isn't present, you can add a `New String`.
#### Program throws error while there is opened browser window from previous session (actual for `headless = false` runs)
- Before running make sure that browser window opened by program last time is closed.
#### Captcha
If you met the captcha issue do the following:
- Run the program with property `headless = false` and wait for captcha appearing
- Wait 10 seconds to make sure program stopped and don't make requests to browser page
  (if you use command line just click `ctrl+c` in cmd window to stop the program)
- Manually pass captcha
- Close browser window and restart program
From now on you have several days or a week without this filth. In theory... It depends on cookie lifetime.
#### Information security
You shouldn't log in every time when program open browser window. It's because cookies are saved in the
`browser-puts-garbage-here` folder (it appears after 1st run). Dont share this folder content with third parties.

## Overview
Despite existing of `hh.ru API` the program uses `Selenide`, `Owner`, `SLF4J (logback provider)`.  
An idea of the program's capabilities can be obtained by looking at `userProperties` file described
in the [Execute](#execute) section and by reading the report example below.
Program also analyze target descriptions via `regular expressions` for imperative mood sequences and key phrases
to make sure that the target can be processed automatically and human intervention isn't required.
#### Executing demo
Demonstration of the program in `analyze vacancy descriptions` mode:
![processorDemo.gif](markdown-resources/processorDemo.gif)
#### Report example
```text
 >>>>>>   NEW RUN   <<<<<<  >>>>>>   NEW RUN   <<<<<<  >>>>>>   NEW RUN   <<<<<<  >>>>>>   NEW RUN   <<<<<<  >>>>>>   NEW RUN   <<<<<<

2023-12-12 22:30  [appProperties]:
2023-12-12 22:30   - useSelenoid         = false
2023-12-12 22:30   - userDataDir         = D:\deploys/browser-puts-garbage-here
2023-12-12 22:30   - profileDir          = browserProfileForTests
2023-12-12 22:30   - useBrowserProfile   = true
2023-12-12 22:30  [userProperties]:
2023-12-12 22:30   - query               = QA automation
2023-12-12 22:30   - headless            = false
2023-12-12 22:30   - processMode         = avd
2023-12-12 22:30   - resumeTitle         = QA auto
2023-12-12 22:30   - coverLetter         = ./userResources/coverLetter.txt
2023-12-12 22:30   - vacancyCount        = 20
2023-12-12 22:30  

---------------------------   Report   ----------------------------

2023-12-12 22:30  Process mode: ANALYZE_VACANCY_DESCRIPTIONS

2023-12-12 22:30  Total targets:                       20
2023-12-12 22:30    Successfully processed count:      20
2023-12-12 22:30    Failed process count:              0
2023-12-12 22:30  Targets with triggers (total):       3
2023-12-12 22:30    Targets which contain key phrases: 1
2023-12-12 22:30    Targets which has imperative mood: 2

2023-12-12 22:30  Manual processing need vacancies (contains key phrases or imperative mood): [

VacancyInfo{
   Title          = Senior QA automation (web, python) / Ведущий тестировщик ПО
   Salary         = от 250 000 до 300 000 ₽ на руки
   Company        = ООО Альянс Реал Эстейт
   Normalized Url = https://hh.ru/vacancy/89262754

   Reasons for manual processing:

   >>> Detected imperative mood sequences: 
     Key: ["йте "]
     Values:
       "лубить свои навыки в автоматизации. ДаваЙТЕ вместе создавать крутые, нагруженные про"

   >>> Triggered Regular Expressions: <<empty match list>>
}, 

VacancyInfo{
   Title          = QA Automation (Java )
   Salary         = <<не указана>>
   Company        = Outlines Technologies
   Normalized Url = https://hh.ru/vacancy/87839694

   Reasons for manual processing:

   >>> Detected imperative mood sequences: 
     Key: ["ите "]
     Values:
       "ботливая корпоративная культура. ПереходИТЕ в наш Telegram-канал и убедитесь в этом:"
     Key: ["йте "]
     Values:
       "ми и логами Откликайтесь скорее – порадуЙТЕ наших рекрутеров!"

   >>> Triggered Regular Expressions: <<empty match list>>
}, 

VacancyInfo{
   Title          = Автотестировщик Python/QA Automation Engineer Python
   Salary         = от 250 000 до 300 000 ₽ на руки
   Company        = ООО РТК - Цифровой Документооборот
   Normalized Url = https://hh.ru/vacancy/90274426

   Reasons for manual processing:

   >>> Detected imperative mood sequences: <<empty match list>>
   >>> Triggered Regular Expressions: 
     Key: ["выполн[а-я]+ тест"]
     Values:
       "онного тестирования; анализ результатов ВЫПОЛНЕНИЯ ТЕСТов; документирование найденных ошибок в "

}]
```
## Issues
#### Major
- [ ] Using `hh.ru api` instead of UI approach with `Selenide`
- [ ] WebUI or SwingUI at least
- [ ] Data base
- [ ] Add CI/CD to make releases automatically after merging into main branch or smthng and bind
this `README.md` to original repo
- [ ] add `process automatically anyway` button to exclude targets from
    list of recommended to process manually (make sense after adding UI and DB)

#### Current
- [x] Page load time isn't enough. HH.ru is very slow. Waits should be increased
- [x] Add FIRST_RUN mode so user can log in, accept cookies, set location etc. Program should just open browser
  window in this mode.
- [x] Remove seconds and milliseconds from report
- [x] Separate user properties from application ones
- [x] Program doesn't work after run with property `headless=true` without killing chrome.exe processes
- [x] If text analyzer found matches in vacancy description add text demo to report
- [x] `find test tasks` mode
- [x] use javascript in `setVal()` method via setting property `fastSetValue` to `true`
  (is it faster? check it).  
  Upd. Yep, its magical...
- [ ] Add to report start/end time and speed (targets per minute)

