# Outline: Alignment is all you need

> Realized while trying to shape [aglc/artifacts.md](../aglc/artifacts.md)
> and [aglc/feature-lifecycle.md](../aglc/feature-lifecycle.md).
> Post: none yet.

## TLDR

In order to work with agents effectively, alignment (TODO link glossary) is key.
When working on documentation, this results in a problem that may be hard to see.

1. Documentation is usually distributed across many documents.
1. Alignment varies depending on the **set of documents** in the work set and/or content.
1. Alignment varies depending on the **type of document** the agent is working on.
1. The relevant information is usually not stated anywhere and needs to be added ad hoc.
1. There is currently no way to persist and steer this information.

## The story

Ich habe gestern versucht, Notizen in Rohform in die entsprechenden Dokumente im "ai" Repository zu übertragen, mit Hilfe eines Agenten.
Die Rohform hatte bereits eine gute Struktur mit recherchierten Inhalten, skizzenhaft beschrieben und anekdotisch ergänzt.

Die Arbeit ging eigentlich gut los: Der Agent hat Dokumente aus meinen Notizen erzeugt, die in meinen Entwürfen recht ähnlich waren. Das schien mir ein guter Startpunkt zu sein.

Es wurde jedoch recht schnell sehr mühsam: Es kamen viele (berechtigte) Rückfragen und folglich Richtungsänderungen aufgrund unklarer Vorgaben meinerseits. In der Folge veränderte sich die Struktur und die Dokumente wuchsen immer weiter an. Es wurde eine Menge von Streichungen, Steering und Korrekturen erforderlich.

Gestern Nacht, nach einem Tag Arbeit, dann die Erkenntnis: Das Ergebnis entsprach in keiner Weise mehr der ursprünglichen Idee. Das Reasoning dazu liegt nicht mehr vor, man kann entweder noch einmal von vorne beginnen oder muss sehr viel streichen.

In diese Falle sind schon viele andere getappt. Dass mir das (erneut) passiert, ist einerseits auf ein Vertrauen in die neuen Modelle
zurück zu führen, aber andererseits auch ein wenig Naivität: Ich war der Meinung, die Struktur liegt klar auf der Hand und die Dinge sind in sich weitgehen widerspruchsfrei, also sollte das doch eine einfach Operation werden.

Warum das nicht funktioniert, dazu unten mehr.

Die folgenden Punkte sind mir aufgefallen. Ich führe sie letztlich auf ein gemeinsames Problem zurück (Alignment) und zeige Wege auf, wie man dieses Problem lösen kann.

### Prose

Mein Agent (Claude Code mit Opus 5) nutzt eine bestimmte Art der Prosa, in die er immer wieder abdriftet: "Mit diesem Artefakt tun wir A, aber nicht B" oder Formulierungen wie "a process that is not documented is a process nodoby owns". 
Wirklich gut klingende Formulierungen, die ich gar nicht gerne löschen wollte.

Erst nach einigem Nachdenken, und beim wiederholten lesen (in jedem Diff), fiel mir auf: Ich fand das für die Dokumente und den Scope eigentlich nicht passend. Warum sollte eine Übersicht die Argumentation enthalten? Warum steht beim ADR etwas zum PRD **und auch umgekehrt**, teils überlappend? 

Hier kommt das Thema "taste" ins Spiel, frei übersetzt "ich weiß, wo ich hin will". Mehr dazu unten.

### Unnötige Herleitungen

Eine deutliche Tendenz in der Beantwortung der Fragen war, dass die Ergebnisse direkt in die Dokumente eingeflossen sind. Zur Erklärung des Dings kamen dann weitere Aspekte hinzu: Rechtfertigung, warum das Ding so sein muss, warum es nicht anders sein kann oder sollte, was die Benefits sind, wie sich das auf den Gesamtprozess auswirkt, wie der Prozess auf die Artefakte zurück wirkt. Letzteres war eigentlich für die Seite gedacht, auf der der Prozess beschrieben ist. Und dort sind sie dann ebenfalls (mehr oder weniger doppelt) gelandet.

Ich fand das auf der einen Seite alternativlos: Wir müssen ja irgendwie die ganzen schönen Fragen und Antworten unterbringen, sie sind so wertvoll! 

Auf der anderen Seite fand ich das unnötig und wurde zunehmend frustriert: Ich hatte mit einer übersichtlichen Liste von Artefakten angefangen, in der beschrieben war, was ein ADR/SDD/Spezifikation ist, warum wie die beötigen, wie lange sie leben, was darin stehen soll. Die Liste wurde immer länger mit kleinen, verwickelten Absätzen: „Warum dies so?“, „Warum jenes nicht so?“. Letztendlich hatte das Dokument irgendwann 25.000 Zeichen.

### Unnötige Prozessbeschreibungen

Der Agent hat sehr viele Dinge geschrieben, die absolut zutreffend, aber unnötig waren, wenn man die Dinge und ihre Implikationen zu Ende denkt.

Wenn man z.B. sagt: „Es wird ein Release-Branche abgezweigt, dieser wird stabilisiert“, muss man nicht dazu erklären, dass keine Features auf dem Branch landen. Oder dass, wenn dann für den Livegang ein Tag erzogen wird, dieser Tag auf eine Commit zeigt, was den Vorteil hat, dass sich dort keine weiteren Commits anlagern können. Schlicht unzutreffend ist die Feststellung, dass man damit sicher weiß, was auf Live ausgerollt ist.
Zusätzlich noch drei mögliche andere Alternativen mit ihren Vor- und Nachteilen. Absolut korrekt, aber hier nicht relevant! Es wird ein Branch abgezweigt, dieser wird stabilisiert. Punkt. Das trägt eine Bedeutung in sich, die in den Worten nicht drinsteckt

Man wird hierbei schnell in eine Art Modus gezogen, wo man denkt: „Naja, da hat er eigentlich ganz recht", aber gar nicht merkt: "Das mag richtig sein, aber es gehört da eigentlich nicht hin.“

Auch dies ein Indiz, dass LLMs nicht verstehen, was mit den Worten bereits gesagt ist. Sie sind darauf trainiert, neue Worte zu generieren. 

### Unnötige Fragen, unnötige Antworten, unnötige Dateien

Zweitens, die ganzen offenen Fragen, die da kamen. Er hat einfach eine "Open Questions" Section in jedes Dokument gepackt, ohne dass ich ihn darum gebeten hätte. Er hat ein Glossar angelegt, ohne dass ich ihn darum gebeten hatte. Habe ich gar nicht gemerkt und das commited, weil ich den Diff zum nächsten Commit sehen wollte, und nachher aufräumen. Ist leider durchgerutscht. In dem Dokument stehen die Definitionen für LLM, Context und Harness. Völlig unwichtig. 

Auch hier wieder das Ding. Die Erkenntnis, was ist wichtig, ist in der Tat eine Sache, die das Ding nicht leisten kann. TODO taste.

Interessant, wie man den Fokus verliert, wenn man anfängt, die offenen Fragen zu beantworten. Und es kommen immer wieder neue offene Fragen, denn die Section ist ja nun mal da, und der Agent generiert hier immer wieder irgendwas rein.
Die Fragen sind auch valide oder sie scheinen valide, und dann schreibt er irgendwas dran als Ergebnis dieser Fragen. Das wird immer fein granularer, teilweise wirklich völlig absurd.

### Unnötige Duplizierung

Je feingranularer dieser Tunnel wird, desto mehr Stellen sind von den Antworten betroffen, und dort wird dann auch was hingeschrieben., und natürlich, da mehrere Stellen betroffen sind, auch noch an mehreren Stellen

Man liest das und denkt man: Moment, das hat er doch irgendwo schon mal so ähnlich geschrieben, wo war das eigentlich? Ich habe das schon gelesen, aber ich weiß nicht mehr, wo. Das heißt, man müsste jetzt durchgehen und sagen: Okay, finde alle Duplikate. Dies funktioniert nicht zuverlässig


Grill me! So was kann man alles mit dem machen, aber der Content muss selber kommen, und er muss auch punktuell kommen. Das ist noch ein anderes Ding: diese riesen Change Sets. Man taucht dort einfach nicht mehr ein. Man liest dann später eine Stelle in Ruhe und stellt fest: Diesen ganzen Absatz oder einen großen Teil des Absatzes kann man einfach wegschmeißen, weil sich die Argumentation oder das, was dann aus den Sätzen vorher ergibt, wenn man die einfach sauber formuliert.



## Die Korrekturen


Einige Zusätzliches Problem, die Sachen die man korrigieren will landen im Kontext, und zusätzlich was immer er da zufällig gelesen hat.
Wenn das Sachen sind, die er selber generiert hat, dann wirkt das wie eine Echo-Kammer, in der er sein eigenes Verhalten verstärkt. 

Sachen, die ich versucht habe in einem Global Prompt unterzubringen, aber dann hat er wieder Sachen weggeschnitten, die in der Tat wichtig waren. Er weiß nicht, was Bedeutung ist. Man muss dem Ding selber die Bedeutung geben. 

## Learnings

### Alignment vs. Taste ("Alles meine Schuld")

Diese ganzen scheinbar unnötigen und scheinbar falschen oder unpassenden Inhalte sind nicht die Schuld des Agenten.
Die Frage muss nicht lauten: „Ist das wichtig?“ sondern „Ist es mir wichtig?“.
Dito: Geht es hierher? In welcher Form geht es hierher? Versus: Geht es vielleicht woanders hin und in anderer Form?
Woher soll der Agent das wissen?
Denn ich weiß ja, was ich will. Ich kann es dem Agenten nur nicht richtig sagen.
Es kann sein, der Agent findet das heraus, es kann aber auch sein, nicht. Und es ist bei jedem Run verschieden.
Dito mit "Schreib das so dass es besser klingt" bedeutet unterschiedliche Dinge für einen PM, Entwickler, User. Für ein Kind.

Das grundsätzliche Problem ist, dass die Arbeit mit Agenten / LLMs ein Glücksspiel wird, wenn kein Alignment erfolgt.

### Alignment vs. Bedeutung

LLMs wissen nicht, was die Bedeutung einer Aussage ist. Manchmal finden sie Zusammenhänge selbst, manchmal nicht.
Wenn die Agenten kein Ziel haben, gegen das sie falsifizieren können, tendieren sie dazu, immer neue Sachen und Details hinzu zu fügen.
Genau deswegen ist Alignment beim Arbeiten mit Dokumenation so wichtig: Es gibt keine Tests, kein fixes Ziel, kein Feature oder Ergebnis,
welches den Zielzustand definiert.

### Alignment durch Agentenpersönlichkeit

Das war gestern eine verpasste Chance. Ich hätte einen Agenten coachen können, während er in dem Prozess mit mir zusammen das Dokument erstellt. Stattdessen sind irgendwelche Anweisungen in der globalen AGENTS.md gelandet, die da gar nicht hingehören, weil sie nur für den speziellen Fall der Erstellung und Validierung von Dokumenten greifen sollen und teils auch gar nicht generalisiert für alle Dokumente gelten sollen.

Sinnvoller wäre es gewesen, eine Agenten-Persönlichkeit mit einer Agenten-Persönlichkeit zu tun und die mir diesen Erkenntnissen zu füttern. Ebenfalls sollte diese Agenten-Persönlichkeit lernen, wie mein Ton ist und wie meine Art zu schreiben ist, damit sie lernen kann, wie ich schreibe oder schreiben will. 

Das mit dem Alignment etwas nicht passt, ist erkennbar daran, wenn die Anweisungen in der globalen Agents.MD (oder auch in jeder anderen Agents.MD) damit beginnt "Wenn du das tust, dann...".
Zum Beispiel "Wenn du Pläne erstellst, nutze dieses Format" oder "beim Schreiben von Dokumentationen beschränke dich auf die Fakten. Es sei denn es ist ein Dokument, in dem es um Erörterung geht. Lass die Herleitung/Argumentation weg, es sei denn es geht ganeu darum, diese heraus zu arbeiten".

All dies sind Hinweise darauf, dass es eine Persönlichkeit sein sollte, der man das sagt, wahlweise Kontext zum Dokument, an dem gearbeitet wird.

### Prosa selber schreiben

Der Entschluss muss auf jeden Fall lauten: selber diese Dinge zu schreiben, den Agenten beim Recherchieren nutzen, für die Validierung, Konsistenzprüfung und so weiter. Eventuell auch beim AUfbau eines Gerüsts. Niemals jedoch ohne klare Idee der Struktur und Kommunikation derselben.

## Ausblick

1. Das ist erneut der Case für eine Metaebene: Eigentlich sollte ich in jedem Dokument schreiben, worum es in dem Dokument geht: wie das Dokument strukturiert sein soll, wie es in Beziehung zur gesamten Struktur der Dokumentation steht, ob und welche Beziehungen es zu anderen Dokumenten hat.

Auf diesen Metaebenen kann man dann durchaus schwurbeln und sich auslassen, was ein ADR ist und was er nicht ist, und wie er über mit dem SDD zusammenhängt. So ein Ort, wo diese beiden in ihrer Beziehung zueinander verhandelt werden, ist total sinnvoll
Dazu müsste im ADR drin stehen: Dort sind deine Metaebenen, die sich damit beschäftigen und die das definieren. Da schreib das entsprechende Zeug hin. Im Übersichtsdokument „Artefakte“ müsste hingegen stehen: Hier geht es um einen Anriss über alle Artefakte, , geh da mal nicht so ins Detail. Die Details befinden sich auf der und der Ebene, auf diesen Plateaus.

Und da sind wir wieder. Ich lande seit Jahren immer wieder an derselben Stelle: Es gibt keine Methode, wie das verhandelt wird. Keine Plateaus, keine Orte, die über den Dingen schweben und in denen ihre Beziehungen verhandelt werden. Kann es bisher nicht geben, weil weder durch Menschen pflegbar noch lesbar. Als Hypertext theoretisch erstellbar. Praktisch nicht nutzbar, weil wir Bäume verstehen, und keine Netze. 

Und das ist eigentlich auch der definitive Hinweis darauf, dass dort etwas ist. Ein Ding, welches es auszugraben gilt.

Genau wie Steve Yegge gesagt hat: Egal ob ich GasTown baue oder meinen heutigen Agent Workflow, ich lande immer wieder bei demselben Ding.
Das ist keine Erschaffung. Ich **exkaviere** immer wieder dasselbe Teil.
