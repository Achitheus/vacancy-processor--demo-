# Vacancy processor (demo)
Automated processing vacancies programm

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
   #   - analyze_vacancy_descriptions (or "AVD"/"avd"),
   #   - vacancies_applying (or "VA"/"va"),
   #   - find_test_tasks (or "FTT"/"ftt")
   process.mode           = avd
   vacancy.count          = 1
   query                  = QA automation
   # Exact resume title isn't necessary for this value. Target resume title should contain it as substring.
   resume.title           = QA auto
   cover.letter           = ./userResources/coverLetter.txt
   # switch this value to `true` if you don't want to see blinking browser. It is `background run mode` 
   headless               = false
   ```
5. Make sure you **saved** changes in the files.
6. Double click to `.jar` file.
## Notes
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
#### First run
- If you use double click first run should be made with property `vacancy.count = 1`.
When program stop make log in, accept cookies, set location,
etc. Next run will take into account these settings.  
- If you use command line it isn't necessary to set special value to `vacancy.count`.
Just wait until browser up and stop the program via pressing `ctrl+c` (several times maybe) in command
line. Then you can make actions described in previous option.
I'd also recommend change default chrome profile color to noticeable one because...
#### Program throws error while there is opened browser window from previous session
- Before running make sure that window opened by program last time is closed.
#### Captcha
If captcha appeared while running do the following:
- Wait 10 seconds to make sure program stopped and don't make requests to browser page
  (if you use command line just click `ctrl+c` in cmd window)
- Manually pass captcha
- Close browser window and restart program
From now on you have several days or a week without this filth. In theory...
#### Information security
You shouldn't log in every time when program open browser window. It's because cookies are saved in the
`browser-put-garbage-here` folder (it appears after 1st run). Dont share this folder content with third parties.

## Overview


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
- [ ] Page load time isn't enough. Waits should be increased
- [ ] Add FIRST_RUN mode so user can log in, accept cookies, set location etc. Program should just open browser
  in this mode.
- [ ] add to report start/end time and speed (targets per minute)
- [x] remove seconds and milliseconds from report
- [x] separate user properties from application ones
- [x] Program doesn't work after run with property `headless=true` without killing chrome.exe processes
- [x] if text analyzer found matches in vacancy description add text demo to report
- [x] `find test tasks` mode
- [x] use javascript in `setVal()` method via setting property `fastSetValue` to `true`
  (is it faster? check it).  
  Upd. Yep, its magical...

