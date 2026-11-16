+++
title = "AI-Agentoj Ekregas la Lecionpreparon"
featuredImage = "20260822-claude-code.png"
date = 2026-08-22
lastmod = 2026-11-16T11:29:41+11:00
tags = ["AI", "LaTeX", "Teaching", "Emacs", "Lingvoj", "Esperanto"]
categories = ["Blog"]
draft = false
weight = 4001
author = "Matt Maguire"
+++

Delonge mi ne skribis pri kiel mi uzas Emacs/LaTeX por krei lecionnotojn por miaj studentoj. Sufiĉe multo okazis dum tiu tempo. Unu el la plej grandaj ŝanĝoj venis de la progresoj en "AI"-teknologio. En la sekvaj artikoloj mi intencas priskribi mian vojaĝon dum mi navigas tra ĉi tiu teknologio kaj aplikas ĝin al mia instrua praktiko.


## Babilrobotoj {#babilrobotoj}

Kiam mi decidis enŝovi la piedfingron en la akvon, mi aliĝis al du el la ĉefaj ludantoj en ĉi tiu kampo, nome Google Gemini kaj Anthropic Claude. Miaj fruaj eksperimentoj kun ChatGPT donis iom seniluziigajn rezultojn, kie ĝi emfaze klopodis konvinki min, ke la valoro de la konstanto de Euler estas 1,5. Ekde tiam la teknologio signife progresis, kaj mi opiniis indan reprovi.

Ĝis tiam mia laborfluo estis krei lecionnotojn en LaTeX mane. Kiam mi alkolisiĝis provante elpensi kiel desegni TiKZ-diagramojn, mi konsultis la retejon Stack Exchange kaj la referencan manlibron de TiKZ por ricevi ideojn. Mi kontentis pri la rezultoj, sed tio sufiĉe tempopostulis. Do ĉe tio mi unue provis uzi AI-n.

Mi vizitis la retejon de la babilroboto kaj donis al la AI prompton priskribantan la tipon de diagramo, kiun mi volis, kaj ĝi generis la TiKZ-kodon. Mi kopi-algluis ĝin en mian dokumenton, kunmetis ĝin por vidi, kiel ĝi aspektis, kaj petis de la AI plibonigojn. Tiam mi malkovris, ke AI efektive povas preni bildon de ekzistanta diagramo kaj plenumi deĉan laboron generante TiKZ-kodon, kiu proksimumas la diagramon. Tio ŝparis al mi iom da tempo, sed estis iom enuiga kun ĉiu tiu kopi-algluado. Sendube devis ekzisti pli bona vojo...


## Envenas la AI-Agentoj {#envenas-la-ai-agentoj}

Mi aŭdis, ke homoj uzas AI-n por kodadaj taskoj. Ĉar LaTeX estas nur alia programlingvo, eble mi povus uzi tiujn disvolvajn ilojn por generi miajn lecionnotojn. Tio kondukis min en la kuniklotruon de _AI-Agentoj_.

Kiam vi havas fenestron de babilroboto, ĝi estas kiel izolita sablujo. Ĝi ne aliras la dosierojn en via sistemo. Se vi volas, ke la babilroboto konas dokumenton, vi devas alŝovi ĝin al la babilujo. La babilroboto povas generi kodon, sed vi devas kopi-alglui ĝin en vian redaktilan fenestron, por ke vi povu konservi ĝin en dosiero kaj kunmeti la kodon.

AI-Agento, male, estas programaro, kiu funkcias sur via loka sistemo. La AI-modelo povas generi tekstan eligon en speciala JSON-formato, kiu priskribas, kiajn operaciojn ĝi volus plenumi sur via sistemo. La programaro de la AI-Agento interpretas ĉi tiun JSON-blokon de teksto kaj tiam plenumas la bezonatajn operaciojn sur via sistemo. Tiuj operacioj povas esti io kiel legado de dosiero, skribado de dosiero, rulado de ŝela komando kiel la LaTeX-kunmetilo, kaj eĉ forigado de dosieroj, kiuj ne plu bezonatas. Do, finiĝis la kopi-algluado --- la LLM-AI povas peti vian agenton fari ĉion, kio bezonatas.

Kvankam ĉi tio estas ege konvenige, doni al la LLM liberan kampon por fari ŝanĝojn en via sistemo estas sufiĉe danĝere. Multaj el ĉi tiuj AI-Agentaj klientoj havas permesan sistemon, kie ili montras la komandon aŭ operacion, kiun ili planas ruli, kaj petas vian permeson por permesi, ke tiu operacio ruliĝu nur unufoje, ruliĝu ĉiun foje, aŭ eĉ rifuzi tiun operacion. Vi povas diri al la agento, ke ĝi rajtas operaci nur sur dosieroj en certa dosierujo (la "projekta" dosierujo), kaj estas prudente uzi sistemon de versia administrado kiel _git_, tiel ke se la LLM/Agento faros ion bedaŭrindan, vi povas restarigi viajn dosierojn al antaŭa commit.

Volega provi ĉi tion, mi devis trovi iun AI-Agentan klientan programaron, kongruan kun la LLM, kiun mi decidis uzi.


## Claude Code {#claude-code}

Por maximumi miajn ŝancojn de sukceso, mi decidis komenci per la Claude-LLM de Anthropic, kiu famas kiel plej avangarda por kododisvolvo. Anthropic provizas AI-Agentan ingon nomatan "Claude Code", kiu estas desegnita por funkcii kun la Claude-modeloj, do ĉi tio ŝajnis bona elira punkto.


### Claude Code en Emacs {#claude-code-en-emacs}

Kiel Emacs-entuziulo, kompreneble mi volis vidi, ĉu mi povas funkcigi ĉion en Emacs, inkluzive de la AI-Agento, la LaTeX-fonto kaj la PDF-eligo. Kompreneble, mi povus ruli la CLI de Claude Code en vterm-bufro, sed mi malkovris, ke ekzistas Emacs-pako nomata _agent-shell_, kiu vokas AI-Agentojn kiel Claude Code en Emacs-bufro. Post kiam mi instalis Claude Code sur mia sistemo, mi informis Doom Emacs en mia `packages.el` dosiero, ke mi volas uzi la pakon _agent-shell_:

```elisp
(package! agent-shell)
```

Tiam mi difinis praktikan klavonligon kaj agordis la defaŭltan Claude-modelon kiel _Sonnet_:

```elisp
(use-package! agent-shell
  :bind ("C-c a" . agent-shell)
  :config
  ;; 1. The ACP Driver Logic (Critical for Arch Linux paths)
  (setq agent-shell-anthropic-claude-acp-command '("claude-agent-acp" "--model" "sonnet"))

  ;; 2. Suppress the migration warning (as per the 2026 update)
  (setq agent-shell-anthropic-claude-command nil)

  ;; 3. Doom-specific UI Tweak
  ;; This ensures the agent-shell buffers get proper syntax highlighting in Doom
  (add-hook 'agent-shell-mode-hook #'doom-mark-buffer-as-real-h))
```

_Sonnet_ estas la LLM de meza nivelo. Aliaj opcioj estas _Haiku_, kiu estas malmultekosta kaj taŭga por bazaj redonoj, kaj _Opus_, kiu estas la altklassa modelo, pli multekosta sed kapabla pritrakti iom pli kompleksan kodon, kun kiu _Sonnet_ eble penus.

Mi informas Emacs, ke mi volas uzi mian lecionan dosierujon, per la komando `M-x cd`, kaj startas la agenton per `M-x agent-shell`. Al mi estas prezentataj kelkaj agentaj opcioj, kaj mi elektas _Claude_. Mi funkcias!

Do, kio nun? Ni kreu lecionon:

```text
Claude> Generate some lesson notes in my usual exesheets-based LaTeX format for my year 8 class on two-step equations. I want to demonstrate both the backtracking and balance methods of solving those equations. Provide some practice examples with space for working.
```

Claude tiam esploros la dosierujan strukturon de la projekto kaj malkovros, kie estas miaj lecionoj por la 8-a jaro. Ĝi vidos, kiujn lecionojn mi jam skribis, kaj generos la novan lecionon en simila formato, skribante la dosieron laŭ mia ekzistanta nomumada konvencio. Ĉio, kion mi devas fari, estas malfermi la dosieron `lesson.tex` en Emacs-bufro, kunmeti ĝin per `C-c C-c` kaj rigardi la PDF-eligon.

Ho ve, la generita leciono ne kunmetiĝas! Claude min fiaskigis. Ni vidu, ĉu ĝi povas elaĉeti sin:

```text
Claude> The file you produced doesn't compile. When generating or changing tex files, always check they compile cleanly with "latexmk -synctex=1" and correct any errors.
```

Claude foriras kaj kontrolas la kunmetajn protokolojn. Ĝi rimarkas, ke ĝi forgesis inkluzivi gravan pakon, aldonas ĝin al la dokumenta antaŭparolo, kaj rekunmetas. Sukceso! Mi povas rigardi la dokumenton en Emacs kaj revizii la lecionnotojn. Se mi vidas iujn problemojn, mi povas doni al Claude rimarkigojn, kaj tre baldaŭ mi havas glatan aron de notoj, kiujn mi povas uzi.

Mi volas certiĝi, ke Claude ne ripetos ĉi tiujn erarojn la sekvan fojon, kiam mi petos ĝin krei lecionnotojn:

```text
Claude> Create a skill for generating lesson notes, based on the feedback given throughout this session.
```

Skiloj estas aro de notoj, kutime en Markdown-formato, kiujn Claude povas legi ĉe la komenco de sesio. Ĉi tiuj notoj povas inkluzivi instrukciojn kiel ĉiam kontroli, ke dosieroj kunmetiĝas, kriz-kontroli la lecionenhavon kontraŭ la instruprogramo, ktp. Ĉi tiu informo fariĝas daŭra trans sesioj, tiel ke la sekvan fojon via prompto povas esti pli mallonga.


### Labortabla aplikaĵo Claude Code {#labortabla-aplikaĵo-claude-code}

Post kiam mi ruligis claude en tiu maniero, mi elpensis laborfluon, kiu havis la LaTeX-fonton, la Claude-ŝelon kaj la PDF-dosieron en apartaj Emacs-fenestroj. Ĉi tio funkcuis bone, ĝis mi ekkomprenis, ke estas pli konvenige havi nur la LaTeX-fontan kaj PDF-fenestrojn en mia Emacs-fremo, kaj uzi la labortablan aplikaĵon Claude Code sur dua ekrano. La labortabla aplikaĵo ankoraŭ povas krei la dosierojn, ruli la kunmetilon, enŝovi aferojn en git ktp, kaj en Emacs mi havas pli da spaco por rigardi la dokumenton kaj ĝian fonton.


## Do, pri la kosto... {#do-pri-la-kosto-dot-dot-dot}

Kun Claude Pro ekzistas malmultekosta monata abono por ĉirkaŭ $30/monato. Tio estas subvenciita kosto, kiu permesas al vi uzi Claude ĝis certa punkto, post kio aŭ vi estas blokita ĝis via uzada fenestro refreŝiĝas, aŭ vi povas pagi aldonon por daŭrigi, se vi ne volas atendi.

Dum miaj komencaj eksperimentoj, mi trovis tre facile elĉerpi la uzadan kvoton, kaj do mi esploris kelkajn strategiojn por redukti mian tokenan uzadon:

-   ene de babela sesio, la _tuta_ babila historio estas transdonata al la LLM ĉiun fojon, kiam vi enigas novan prompton. Tio povas konsumi multajn tokenojn, kaj do estas plej bone teni la babelajn sesiojn mallongaj, limigitaj al unu leciono aŭ mallonga vico de rilataj lecionoj. Kiam vi transiras al nova jara grupo, komencu freŝan babilon.
-   se vi laboras pri leciovico, povas havi sencon uzi la komandon `/compact`. Tio igas la LLM generi resumon de la babela sesio kaj anstataŭigi la babilan historion per tiu mallonga resumo. Vi ankoraŭ konservas superrigardon pri tio, pri kio vi laboris, sen ĉiuj detaloj kaj mispaŝoj.
-   Se vi ofte referencas dokumenton kiel la instruprogramon, konvertu ĝin al teksta formato, kiun estas pli facile analizi por la LLM. Al mi plaĉas diri al la AI konverti la instruprogramon al Org mode-dosiero. Tio estas multekosta operacio, sed vi nur bezas fari ĝin unufoje. La AI povas tiam referenci la tekstan dosieron anstataŭ konverti la instruprogramon de PDF al teksto ĉiun unuopan fojon.
-   aliaj optimumigoj povus inkluzivi diri al la LLM uzi kunmetilajn flagojn, kiuj reduktas la tekston skribatan al `stdout`, kaj peti ĝin revizii la kunmetajn protokoldosierojn nur se la kunmetilo redonas erarkodon je la fino de la tasko.

Kun ĉi tiuj optimumigoj, Claude fariĝas aligebla kaj tre efika asistanto en kreado de lecionnotoj. Tamen, ĉi tiu preznivelo estas por la AI-firmaoj iom kiel varoj vendataj sub kosto, kaj estas neverŝajne, ke ili povos longtempe daŭre oferti tiajn prezojn. Min maltrankviligas, ke mi eble alkutimiĝos al ĉi tiu tipo de laborfluo kaj poste eble ne povos pagi ĝin.

Sekvatempe mi parolos pri alternativa laborfluo, kiun mi starigis por respondi al ĉi tiu zorgo.
